# データ構造周りの実装  
* Array  
    固定配列  
    Impl: [link](https://github.com/terraritto/DataStructurePractice/blob/main/Ex_02/TArray.h) 

* ArrayStack  
    動的配列、なくなったら2倍のサイズで確保  
    Impl: [link](https://github.com/terraritto/DataStructurePractice/blob/main/Ex_02/ArrayStack.h) 

* FastArrayStack  
    std::copyで書き換えただけ  
    Impl: [link](https://github.com/terraritto/DataStructurePractice/blob/main/Ex_02/FastArrayStack.h) 

* ArrayQueue  
    リングバッファによる管理  
    Impl: [link](https://github.com/terraritto/DataStructurePractice/blob/main/Ex_02/ArrayQueue.h) 

* ArrayDeque  
    前後でずらして追加してく方式  
    Impl: [link](https://github.com/terraritto/DataStructurePractice/blob/main/Ex_02/ArrayDeque.h) 

* DualArrayDeque  
    2つのArrayStackをくっつけて追加していく  
    Impl: [link](https://github.com/terraritto/DataStructurePractice/blob/main/Ex_02/DualArrayDeque.h) 

* RootishArrayStack  
    ブロック単位で確保する  
    Impl: [link](https://github.com/terraritto/DataStructurePractice/blob/main/Ex_02/RootishArrayStack.h) 

* BDeque  
    固定サイズで確保した中でやりくりするDeque  
    Impl: [link](https://github.com/terraritto/DataStructurePractice/blob/main/Ex_03/BDeque.h) 

* SLList  
    単方向リスト  
    Impl: [link](https://github.com/terraritto/DataStructurePractice/blob/main/Ex_03/SLList.h) 

* DLList  
    双方向リスト  
    Impl: [link](https://github.com/terraritto/DataStructurePractice/blob/main/Ex_03/DLList.h) 

* SEList  
    ブロック単位で確保する双方向リスト  
    Impl: [link](https://github.com/terraritto/DataStructurePractice/blob/main/Ex_03/SEList.h) 

* XORList  
    XORで賢くつなぐ双方向リスト  
    Impl: [link](https://github.com/terraritto/DataStructurePractice/blob/main/Ex_03/XORList.h) 

* SkipListSSet  
    スキップリストによるSetの実装  
    Impl: [link](https://github.com/terraritto/DataStructurePractice/blob/main/Ex_04/SkipListSSet.h) 

* SkipListList  
    スキップリストによるListの実装  
    Impl: [link](https://github.com/terraritto/DataStructurePractice/blob/main/Ex_04/SkipListList.h) 