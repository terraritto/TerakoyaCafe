# TangNano9kでシンプルなCPUを作ってみる その4: 使いやすいように拡張  
今回でSimpleCpuは一旦終了.  
前回でTD4ができたので、実際に使いやすくするために拡張を行っていく.  

まずは入出力周りが結構ごちゃっとしてる.  

![SimpleCpu_03_01_01](Image/MakeSimpleCpu_03_01.webp)  

この中で特に

* リセットボタン
* クロック
* LED

という部分に着目する.  
これはクロックの同期や処理の合わせる制御を司る「制御バス」とキーボードやらディスプレイやらとの接続を司る「I/O」バスの二つがある.  
これを実際に図にすると次のような感じ.  

![MakeSimpleCpu_04_01](Image/MakeSimpleCpu_04_01.webp)  

制御バスはクロックとボタンというメンバを持っており、両方入力方向.  
そのため、このメンバをまずinterfaceで用意.  
その後、ポートに関しては入力として定義すればＯＫ.  
```verilog
// 制御用バスのインターフェース
interface ctrl_bus_if();
    logic clk;      // クロック
    logic n_reset;  // リセットボタン

    // 両方入力として定義
    modport master(input clk, input n_reset);
endinterface
```

I/Oバスについて.  
こちらはメンバとしてledのみ.  
そして、方向は出力となるように設定すればよい.  
今回のLEDは4つなので、4本のワイヤーが必要.  
そのため、`WIDTH`というパラメータで指定できるようにする.  

```verilog
// 入力用バスのインターフェース
// 入力としてビット数をWIDTHとして設定できるように
interface io_bus_if #(parameter WIDTH) ();
    logic [WIDTH-1:0] led;      // LED

    // LEDは出力
    modport master(output led);
endinterface
```

ここまで2つのバスが書けた！  

次はバスを適用させていく.  
まずはtopにバスを書いて、mother_boardに渡す.  
`WIDTH`は4bit CPUなので、4を指定すればよい.  
```verilog
    // 制御用バス
    ctrl_bus_if ctrl_bus();
    assign ctrl_bus.clk = clk;
    assign ctrl_bus.n_reset = pin_n_reset;

    // 入力用バス
    io_bus_if #(.WIDTH(4)) io_bus();
    assign pin_led = io_bus.led;

    mother_board mother_board(.ctrl_bus, .io_bus);
```
マザーボード側もちょっとだけ変更.  
```verilog
module mother_board
(
    ctrl_bus_if.master ctrl_bus,    // 制御用バス
    io_bus_if.master io_bus         // 入力用バス
);
```

いったんここまでで次はメモリバスを見ていく.  

メモリバスは単純にCPUとROM側で今まで渡しあってた  

* Address
* Data

の二つのメンバのことを表している.  
図にすると次のような感じ.  

![MakeSimpleCpu_04_02](Image/MakeSimpleCpu_04_02.webp)  

CPUはROMにアドレスを出力,ROMはアドレスを入力として受け取る.  
そうしてアドレスにあるデータを出力し、CPUはこれを入力として受け取るという構図だ.  

これつまりメンバは同じだけど、方向が真逆というものがものなわけだ.  
こういう時はポート側で方向を真逆にした2つを用意すればよい.  
今回はこれを「マスター」と「スレイブ」で用意する.  

```verilog
// メモリ用バスのインターフェース
interface mem_bus_if #(parameter ADDR_WIDTH, DATA_WIDTH) ();
    logic [ADDR_WIDTH-1:0] address;     // アドレス
    logic [DATA_WIDTH-1:0] data;        // データ

    modport master(output address, input data); // マスターポート
    modport slave(input address, output data);  // スレーブポート
endinterface
```

さて、そしたらこれをmother_board側に埋め込む.  
アドレスは4bit,命令はopecodeで8bitなので、そのように設定.  
あとはcpuとromに波及するようにすればOK!!  
cpuに関しては制御バスとI/Oバスを入れるのを忘れずに.  

```verilog
    // メモリ用バス
    mem_bus_if #(.ADDR_WIDTH(4), .DATA_WIDTH(8)) mem_bus();

    // モジュールの埋め込み
    cpu cpu(.ctrl_bus, .io_bus, .mem_bus);
    rom rom(.mem_bus);
```

そしたらCPUとROMも渡すものを変えちゃおう.  
ここで`.master`と`.slave`で使い分けるだけでOK！  
非常に簡単ですね.  
```verilog
module cpu
(
    ctrl_bus_if.master ctrl_bus,    // 制御用バス
    io_bus_if.master io_bus,        // 入力用バス
    mem_bus_if.master mem_bus       // メモリ用バス
);

module rom
(
    mem_bus_if.slave mem_bus    // メモリ用バス
);
```
これでバスの設定も完了！  

さて、次にやるのはROMを別ファイルに移す対応.  
前回までは以下のように直書きしていた.  
```verilog
    always_comb begin
        case (address)                    // address  data  operate
            4'b0000: data = 8'b0111_1010; // 0        7A    mov B, 10
        // ...
```

これだとプログラムを書く毎にverilogに書いてて、ちょっと気持ち悪い...  
そのため、ファイルから読み込むようにする.  
ファイルの読み込みは簡単で、まず`rom_array`で読み込み先の配列を用意.  
その後、`readmemh`でファイルを指定して読み込めばOK.  
最後にこの配列に対して、アドレスを使ってアクセスすればデータを取得できるわけだ.  
```verilog
    logic [7:0] rom_array [16]; // 8bitワイヤーx16本
    initial $readmemh("rom_data.mem", rom_array); // romの読み込み
    assign mem_bus.data = rom_array[mem_bus.address]; // 繋ぐ
endmodule
```

書いてみると非常に単純なものだ.  

ファイルの中身は次のような感じ.  
```verilog
        // address  data  operate
7F      // 0        7F    mov B, 15
90      // 1        90    OUT B
```
うん、直書きだ.  
ここまでくればコンパイラで生成したのをアセンブリにして機械語に...という流れも漠然と見えてくるものである.  