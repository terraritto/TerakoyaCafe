# Appendix ImGuiの初期化の書き方が変わってた件  
DirectXだったりOpenGLだったり、低レイヤーでUIを使いたい...  
自分で作るのは面倒なので、それっぽく出せるものがあれば...というときに毎回使うのはみんなImGuiだと思う.  

初期化にそれっぽく渡すだけで簡単に出せるので、自分も毎回脳死でコピーして使っている.  
以下のような感じでOK.  
```c++
    // window側初期化
	IMGUI_CHECKVERSION();
	ImGui::CreateContext();
    ImGui_ImplWin32_Init(m_hWnd);

    // imguiのDirectX用初期化
    auto descriptorForImGui = m_device->AllocateDescriptor();
	ImGui_ImplDX12_Init
	(
		m_device->GetDevice().Get(),
		m_device->BACK_BUFFER_COUNT,
		DXGI_FORMAT_R8G8B8A8_UNORM,
		heap.Get(),
		descriptorForImGui.m_cpuHandle,
		descriptorForImGui.m_gpuHandle
	);
```

今回新しくまたGithub[^1]から取ってきて、初期化をコピーして書いた.  
そして実行したら、動かない...  
そんな馬鹿な...と思って調べたらこの書き方、非推奨になったらしい.  
いや、非推奨なら動いてよ...と思ったけど、まあそれならということで新しい書き方にシフトすることにした.  

さて、まず最初のWindowの初期化.  
こいつに関しては何も変わらない.  
```c++
    IMGUI_CHECKVERSION();
    ImGui::CreateContext();
    ImGui_ImplWin32_Init(window);
```

違うのはここから,新しいバージョンでは`ImGui_ImplDX12_InitInfo`で初期化のためのデータを入れてあげる必要がある.  
これを渡すことで初期化を行う寸法である.  
```c++
    ImGui_ImplDX12_InitInfo initInfo;
    ImGui_ImplDX12_Init(&initInfo);
```

infoの構造体は基本的に突っ込むものは同じ.  
追加でCommandQueueがあるくらい？それ以外は全く同じな気がする.  
```c++
    initInfo.Device = GraphicsProxy::GetD3D12Device();
    initInfo.CommandQueue = GraphicsProxy::GetGraphicsQueue().lock()->GetQueue();
    initInfo.NumFramesInFlight = desc.numFrames;
    initInfo.RTVFormat = desc.rtvFormat;
    initInfo.DSVFormat = desc.dsvFormat;
    auto* resourceDescriptorHeap = GraphicsProxy::GetResourceDescriptorHeap();
    initInfo.SrvDescriptorHeap = resourceDescriptorHeap->GetHeap();
```

ではちょっと違うところは？というと、DescriptorのAllocateとFreeをFunctionとして渡す必要がある.  
```c++
    initInfo.SrvDescriptorAllocFn = &ImguiManager::Allocate;
    initInfo.SrvDescriptorFreeFn = &ImguiManager::Free;
```

要は今までは自分で割り振ってた部分をImgui側がどこかで必要な時にAllocate/Freeしてくれるようになるわけか.  
確かにこっちの方が開発側も地味に扱いやすいのかもしれない.  
今回は適当にImguiManagerに`m_holders`で持たせることにした.  
```c++
	static std::vector<DescriptorHolder> m_holders;
```

Alloc/Free時に登録/解除をしてあげれば問題ない寸法.  
```c++
void ImguiManager::Allocate(ImGui_ImplDX12_InitInfo* info, D3D12_CPU_DESCRIPTOR_HANDLE* outCpuDescHandle, D3D12_GPU_DESCRIPTOR_HANDLE* outGpuDescHandle)
{
    // allocate
    auto handleSRV = GraphicsProxy::GetResourceDescriptorHeap()->Allocate(1);
    assert(handleSRV.IsValid());

    DescriptorHolder holder = DescriptorHolder(DescriptorHolder::HEAP_RES, handleSRV);
    m_holders.push_back(holder);

    // set
    outCpuDescHandle->ptr = holder.GetCpuHandle().ptr;
    outGpuDescHandle->ptr = holder.GetGpuHandle().ptr;
}

void ImguiManager::Free(ImGui_ImplDX12_InitInfo* info, D3D12_CPU_DESCRIPTOR_HANDLE cpuDescHandle, D3D12_GPU_DESCRIPTOR_HANDLE gpuDescHandle)
{
    auto iter = std::find_if(m_holders.begin(), m_holders.end(), [cpuDescHandle, gpuDescHandle](DescriptorHolder& holder)
        {
            bool isCpuEqual = holder.GetCpuHandle().ptr == cpuDescHandle.ptr;
            bool isGpuEqual = holder.GetGpuHandle().ptr == gpuDescHandle.ptr;
            return isCpuEqual && isGpuEqual;
        });

    if (iter != m_holders.end())
    {
        // free resource
        iter->Reset();
        m_holders.erase(iter);
    }
}
```

正しい対応かは分かんないけど、とりあえずこれで動いてるしまあいいか...となってる.  
Alloc/Freeを作るという手間さえできればこれでImGuiの初期化完了!!  
忘れそうなので一旦まとめておいた.  
なんかImGuiを久々に使うけど、なぜか動かないな...という人のためになれば幸いです.  

[^1]: [Dear ImGui](https://github.com/ocornut/imgui)