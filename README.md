# Fine_tuning_LLMs

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/f901107/Fine_tuning_LLMs/blob/main/Fine_tune_Llama_2_in_Google_Colab.ipynb)

# 介紹

在 OpenAI 的帶動下自然語言的大型語言模型（Large Language Model, LLMs）取得了驚人的進步。然而，這些模型也有其局限性。人們對於隱私的擔憂，遵循一套固定的規則，並限制於它們最後一次訓練的日期。像 PaLM / PaLM 2 和 GPT-3.5 / GPT-4 這樣的預訓練 LLM 也不是開源的。這意味著開發者和研究人員無法獲取模型的內部運作，限制了他們針對特定用例進行微調和定制的能力。

## Llama 2.0

Llama 2.0 帶來了一個新時代，它是由 Meta 推出的開源 LLM，允許在您自己的數據集上進行微調，減少隱私擔憂，並提供個性化的 AI 體驗。此外， Quantized Low-Rank Adaptation (QLora) 方法為微調 LLMs 提供了一種高效的方式，QLora 透過降低記憶體使用，只需要一個 GPU，根據個人需求定制模型更為方便和經濟高效。

### 介紹 Llama 2.0

Meta 最新 (2023/07) 的 LLM 模型，Llama 2.0 展示了一系列預訓練和微調的模型，從 70 億個參數到 700 億個參數。

與 Llama 1.0 相較之處有：

- **Llama 2**：它的前身 Llama 1 的重新設計版本，來自各種公開可用資源的更新訓練數據。提供三種版本：7B、13B 和 70B 參數。
- **Llama 2-Chat**：是 Llama 2 的優化版本，特別針對對話為基礎的用例進行微調。和 Llama 2 一樣，提供三種版本：7B、13B 和 70B 參數。

### Llama 2.0 有哪些更新

- 將上下文窗口從 2048 token 擴大到 4096 token，使模型能夠處理更大量的資訊。
- 為了解決注意力與 token 數量二次成本的問題，作者引入了 Grouped-Query Attention，在多個頭部之間 sharing key and value projections。
- 利用更多數據進行訓練，結合從網路上抓取的數據和基於人類標註者反饋資訊。模型選擇使用公開數據，確保與開源的兼容性，減少可能發生的法律問題。

**Llama 2.0 在各種測試中都優於開源模型**。儘管它在與 GPT-4 和 PaLM 這樣的封閉源模型競爭時略顯不足，但考慮到它們明顯更大的參數大小和使用私有數據進行訓練，這是預期之內的。Llama 2 模型系列是一個開源的寶庫，可供研究和有限的商業使用。

## Parameter-Efficient Fine Tuning (PEFT)

在自己的數據集上微調語言模型是有潛力的。然而，這個過程經常需要大量的 GPU 記憶體( video RAM, vRAM)，且可能是一項資源密集型的工作。例如，微調一個有 650 億參數的模型需要 780 GB GPU 記憶體，相當於十個A100 80 GB GPUs。這種資源需求已經超出一般大眾的財力範圍。

EJ Hu 在2021年提出的 LoRA： Low-Rank Adaptation of Large Language Models。LoRA 的架構涉及凍結預訓練模型 Transformer 的權重，並在矩陣中訓練額外的權重變化，而不犧牲關鍵資訊。
