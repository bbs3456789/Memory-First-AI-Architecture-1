# Memory-First AI Architecture
記憶優先AI架構：對當前大模型範式的反思

定位（「個人研究用白皮書，非產品、不含程式碼」）
時間線（「初稿：2025/08/08，復刻更新：2025/12/06」

> A systematic alternative to the "bigger is better" approach in AI development
> 一個系統性的替代方案，挑戰"越大越好"的AI發展思路

## 🌟 核心理念

本項目實現了一個**記憶優先的AI架構**，強調：
- ✅ **外部記憶系統**替代單純擴大模型規模
- ✅ **多層記憶結構**（工作記憶、情節記憶、語義記憶、長期記憶）
- ✅ **協同對話循環**（檢索→推理→共識→記憶更新）
- ✅ **輕量級推理模型**（7-70B）+ 記憶系統
- ✅ **本地部署**支持（LM Studio 整合）

## 📁 項目結構

```
Memory-First-AI-Architecture/
├── memory_engine.py          # 記憶引擎（四層記憶結構）
├── retrieval_engine.py       # 檢索引擎（混合檢索策略）
├── reasoning_engine.py       # 推理引擎（意圖理解、推理鏈）
├── dialogue_manager.py       # 對話管理器（協同對話循環）
├── attention_mechanism.py    # 注意力機制
├── knowledge_graph.py        # 知識圖譜
├── memory_consolidation.py   # 記憶整合
├── llm_interface.py          # LLM 接口（支持多後端）
├── config.py                 # 配置管理
├── example_lmstudio.py       # LM Studio 完整示例
└── utils.py                  # 工具函數
```

### 基本對話

```python
import asyncio
from memory_engine import MemoryEngine
from retrieval_engine import RetrievalEngine
from reasoning_engine import ReasoningEngine
from dialogue_manager import DialogueManager
from llm_interface import LLMFactory

async def main():
    # 創建 LLM 接口（自動使用配置的後端）
    llm = LLMFactory.create_llm()

    # 初始化組件
    memory_engine = MemoryEngine()
    retrieval_engine = RetrievalEngine(memory_engine)
    reasoning_engine = ReasoningEngine(llm_interface=llm)

    # 創建對話管理器
    dialogue_manager = DialogueManager(
        memory_engine=memory_engine,
        retrieval_engine=retrieval_engine,
        reasoning_engine=reasoning_engine
    )

    # 開始對話
    session_id = dialogue_manager.create_session("user123")
    turn = await dialogue_manager.process_turn(
        session_id=session_id,
        user_input="什麼是Python？"
    )

    print(turn.response)

asyncio.run(main())
```
## 🏗️ 架構特點

### 1. 四層記憶結構

- **工作記憶**：短期活躍信息（容量限制）
- **情節記憶**：時間序列經驗（帶時間衰減）
- **語義記憶**：結構化知識（高共識信息）
- **長期記憶**：持久化存儲（所有歷史記憶）

### 2. 協同對話循環

```
用戶問題 → 意圖理解 → 記憶檢索 → 推理討論 →
共識構建 → 記憶更新 → 生成響應 → 循環
```

### 3. 混合檢索策略

- 語義相似度檢索（基於嵌入）
- 時間相關性檢索（最近優先）
- 重要性加權檢索
- 跨層記憶檢索

## 🤝 貢獻

歡迎貢獻代碼、報告問題或提出建議！

## 🙏 致謝

本項目基於"記憶優先的AI架構：對當前大模型範式的反思"論文的核心理念實現。
