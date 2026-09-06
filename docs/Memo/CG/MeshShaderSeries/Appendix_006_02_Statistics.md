# Appendix PIPELINE_STATISTICSについて  
今回のInstancingではImgui内にInvocation,要は呼び出し回数が表示されている.  
実際の呼び出し回数なんかはGraphicsの場合、RenderDocだったりNsightとかとかで解析するのも多いかもしれない.  
ただ、この辺は自分で情報を取ることも可能！今回は簡単にツールを入れずに取る方法を見ていく.  

今回はPIPELINE_STATISTICSというものを使って実装を行っていく.  
これはDirectXに実装されているもので、呼び出し回数を実際に必要区間で取ることできる.  
実装は簡単で、まず最初のDirectXの初期化時にQueryHeapを初期化しておく.  

```c++
	// Pipeline Statistics
	{
		D3D12_QUERY_HEAP_TYPE queryType = m_isUseMeshlet ? D3D12_QUERY_HEAP_TYPE_PIPELINE_STATISTICS1 : D3D12_QUERY_HEAP_TYPE_PIPELINE_STATISTICS;
		D3D12_QUERY_HEAP_DESC queryDesc = { queryType, 1 };
		hr = m_device->CreateQueryHeap(&queryDesc, IID_PPV_ARGS(&m_queryHeap));
		if (FAILED(hr))
		{
			ELOGA("Error: ID3D12Device::CreateQueryHeap() Failed.");
			return false;
		}
	}
```

ここで大事なのは`D3D12_QUERY_HEAP_TYPE_PIPELINE_STATISTICS`と`D3D12_QUERY_HEAP_TYPE_PIPELINE_STATISTICS1`.  
structを見ると分かりやすいので、まずは`D3D12_QUERY_HEAP_TYPE_PIPELINE_STATISTICS`[^1]側から見てみる.  

```c++
typedef struct D3D12_QUERY_DATA_PIPELINE_STATISTICS {
  UINT64 IAVertices;
  UINT64 IAPrimitives;
  UINT64 VSInvocations;
  UINT64 GSInvocations;
  UINT64 GSPrimitives;
  UINT64 CInvocations;
  UINT64 CPrimitives;
  UINT64 PSInvocations;
  UINT64 HSInvocations;
  UINT64 DSInvocations;
  UINT64 CSInvocations;
} D3D12_QUERY_DATA_PIPELINE_STATISTICS;
```

うん、基本的なInvocationやデータが取れる.  
ただ、MSやASといったものがないので、Mesh Shader側のデータがない...  
ここで`D3D12_QUERY_DATA_PIPELINE_STATISTICS1`[^2]側を見てみる.  

```c++
typedef struct D3D12_QUERY_DATA_PIPELINE_STATISTICS1 {
  UINT64 IAVertices;
  UINT64 IAPrimitives;
  UINT64 VSInvocations;
  UINT64 GSInvocations;
  UINT64 GSPrimitives;
  UINT64 CInvocations;
  UINT64 CPrimitives;
  UINT64 PSInvocations;
  UINT64 HSInvocations;
  UINT64 DSInvocations;
  UINT64 CSInvocations;
  UINT64 ASInvocations;
  UINT64 MSInvocations;
  UINT64 MSPrimitives;
} D3D12_QUERY_DATA_PIPELINE_STATISTICS1;
```

ここで追加されてるのは以下.  
```c++
  UINT64 ASInvocations;
  UINT64 MSInvocations;
  UINT64 MSPrimitives;
```

なるほど、Mesh Shaderの場合は`D3D12_QUERY_DATA_PIPELINE_STATISTICS1`を使う、それ以外は`D3D12_QUERY_DATA_PIPELINE_STATISTICS`を使えばいいってわけだね.  

計測においては`BeginQuery`と`EndQuery`で挟むことで計測が可能.    
今回はこんな感じで簡略なコマンドをまとめて実装してみた.  
```c++
void GraphicsProxy::BeginQuery(ID3D12GraphicsCommandList* command)
{
    auto query = GraphicsDevice::Instance().GetQuery();
    bool isMeshlet = IsUseMeshlet();
    auto type = isMeshlet ? D3D12_QUERY_TYPE_PIPELINE_STATISTICS1 : D3D12_QUERY_TYPE_PIPELINE_STATISTICS;
    command->BeginQuery(query, type, 0);
}

void GraphicsProxy::EndQuery(ID3D12GraphicsCommandList* command)
{
    auto query = GraphicsDevice::Instance().GetQuery();
    bool isMeshlet = IsUseMeshlet();
    auto type = isMeshlet ? D3D12_QUERY_TYPE_PIPELINE_STATISTICS1 : D3D12_QUERY_TYPE_PIPELINE_STATISTICS;
    command->EndQuery(query, type, 0);
}
```

これは次のような感じで実際に描画する際に挟んであげればよいだけ.  

```c++
    // set descriptor heap table
    GraphicsProxy::BeginQuery(command); // 測定開始
    for (auto i = 0u; i < m_model.GetMeshCount(); i++)
    {
        auto mesh = m_model.GetMesh(i).lock();
        UINT meshletCount = mesh->GetMeshletCount();

        //camera
        command->SetGraphicsRoot32BitConstants(0, 16, &proj, 0);
        command->SetGraphicsRoot32BitConstants(0, 1, &instanceCount, 16);
        command->SetGraphicsRoot32BitConstants(0, 1, &meshletCount, 17);

        // meshlet
        command->SetGraphicsRootShaderResourceView(1, mesh->GetPositions().GetGpuAddress());
        command->SetGraphicsRootShaderResourceView(2, mesh->GetMeshlets().GetGpuAddress());
        command->SetGraphicsRootShaderResourceView(3, mesh->GetUniqueVertexIndices().GetGpuAddress());
        command->SetGraphicsRootShaderResourceView(4, mesh->GetPrimitiveIndices().GetGpuAddress());
        command->SetGraphicsRootShaderResourceView(5, m_instancesBuffer.GetGpuAddress());

        // Draw
        UINT threadGroupCountX = static_cast<UINT>((meshletCount * instanceCount) / 32) + 1;
        command->DispatchMesh(threadGroupCountX, 1, 1);
    }
    GraphicsProxy::EndQuery(command); // 測定終了

    // ResolveQuery
    GraphicsProxy::ResolveQuery(command); // 抽出
```

最後の`ResolveQuery`でデータの抽出が行える.  
これは以下のように自分はまとめておいた.  

```c++
void GraphicsProxy::ResolveQuery(ID3D12GraphicsCommandList* command)
{
    auto query = GraphicsDevice::Instance().GetQuery();
    bool isMeshlet = IsUseMeshlet();
    auto type = isMeshlet ? D3D12_QUERY_TYPE_PIPELINE_STATISTICS1 : D3D12_QUERY_TYPE_PIPELINE_STATISTICS;
    command->ResolveQueryData(query, type, 0, 1, m_queryBuffer.GetResource(), 0);
    m_isResolvedQuery = true;
}
```

あとはすでにQueryがあるときにデータを取ってくれば良い.  
Mapでデータを取り出して書き込めばOK.  
```c++
    // GetQuery
    D3D12_QUERY_DATA_PIPELINE_STATISTICS1 pipelineStatistics = {};
    if (GraphicsProxy::HasQuery())
    {
        void* pointer = nullptr;
        GraphicsProxy::GetQueryResource()->Map(0, nullptr, &pointer);
        memcpy(&pipelineStatistics, pointer, sizeof(D3D12_QUERY_DATA_PIPELINE_STATISTICS1));
        GraphicsProxy::GetQueryResource()->Unmap(0, nullptr);
    }
```

後は自由に画面に描画するなり、出力するなり好きにするとよい.  
自分は以下のような感じでImGuiで常に表示するようにしてみた.  

```c++
    ImGui::Text(std::format("CInvocations: {}", pipelineStatistics.CInvocations).c_str()); ImGui::NextColumn();
    ImGui::Text(std::format("CPrimitives: {}", pipelineStatistics.CPrimitives).c_str()); ImGui::NextColumn();
    ImGui::Text(std::format("PSInvocations: {}", pipelineStatistics.PSInvocations).c_str()); ImGui::NextColumn();
    ImGui::Text(std::format("ASInvocations: {}", pipelineStatistics.ASInvocations).c_str()); ImGui::NextColumn();
    ImGui::Text(std::format("MSInvocations: {}", pipelineStatistics.MSInvocations).c_str()); ImGui::NextColumn();
    ImGui::Text(std::format("MSPrimitives: {}", pipelineStatistics.MSPrimitives).c_str()); ImGui::NextColumn();
```

今回は`D3D12_QUERY_DATA_PIPELINE_STATISTICS1`を見てきた.  
ツールを使わずに簡単に計測ができるのは非常に嬉しいところ.  
次回からはこのInvocationを実際に見ることで、カリングがどれくらいされているかを確認していくことになる.  

[^1]: [D3D12_QUERY_DATA_PIPELINE_STATISTICS](https://learn.microsoft.com/ja-jp/windows/win32/api/d3d12/ns-d3d12-d3d12_query_data_pipeline_statistics)  
[^2]: [D3D12_QUERY_DATA_PIPELINE_STATISTICS1](https://learn.microsoft.com/ja-jp/windows/win32/api/d3d12/ns-d3d12-d3d12_query_data_pipeline_statistics1)
