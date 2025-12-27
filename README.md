# 本地文件問答 RAG 系統

[![Python Version](https://img.shields.io/badge/Python-3.13%2B-blue?logo=python)](https://www.python.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Built with Gemini](https://img.shields.io/badge/Built%20with-Gemini-4285F4?logo=google&logoColor=white)](https://ai.google.dev/)

本專案是一個使用 Jupyter Notebook 實現的本地檢索增強生成 (RAG) 流程，用於與本地文件進行問答。

---

## 總覽

本專案在一個 Jupyter Notebook (`main.ipynb`) 中完整地展示了一個 RAG 的核心工作流程。它不依賴複雜的框架，讓您可以清楚地看到從文件處理到答案生成的每一個環節。

**工作流程一覽:**

```
📄 文件載入 (doc.md)
   │
   └─> ✂️ 文本分割 (Chunking)
         │
         └─> 🧠 向量嵌入 (Embedding)
               │
               └─> 💾 存入向量資料庫 (ChromaDB)
                     │
                     └─> 🔍 檢索 (Retrieval)
                           │
                           └─> 📈 精準重排 (Reranking)
                                 │
                                 └─> 💬 生成答案 (LLM)
```

---

## 功能

-   **本地化**: 除了最後的 LLM API 請求外，所有資料處理和檢索流程都在本地執行。

-   **持久化儲存**: 使用 `ChromaDB` 將文件向量持久化儲存在本地，無需每次重新建立索引。

-   **嵌入**: 採用 `Qwen/Qwen3-Embedding-0.6B` 模型生成文本向量。

-   **二階段檢索**: 結合了快速的向量相似性搜索和精準的 `Cross-Encoder` 重排序，有效提升檢索結果的相關性。

-   **生成答案**: 使用  `Gemini 2.5 Flash` 模型生成最終的摘要性回答。

---

## 技術棧

| 元件 | 技術 / 模型 |
| :--- | :--- |
| **核心環境** | Jupyter Notebook |
| **文本處理** | `langchain-community`, `langchain-text-splitters` |
| **向量嵌入模型** | `sentence-transformers` (`Qwen/Qwen3-Embedding-0.6B`) |
| **向量資料庫** | `ChromaDB` (Persistent Client) |
| **重排序模型** | `CrossEncoder` (`BAAI/bge-reranker-v2-m3`) |
| **問答生成模型** | Google `Gemini 2.5 Flash` via `google-genai` |
| **環境與套件管理** | `uv` |

---

## 🛠️ 環境設定

本專案建議使用 `uv` 來管理 Python 虛擬環境和依賴套件，以確保環境的一致性。

1.  **安裝 uv**

    如果您尚未安裝 `uv`，可以透過 pip 或官方推薦的方式安裝：
    ```bash
    # 使用 pip
    pip install uv
    ```

2.  **建立並啟用虛擬環境**

    在專案的根目錄下，執行以下指令來建立虛擬環境（uv 會將其建立在 `.venv` 資料夾中）：
    ```bash
    uv venv
    ```
    接著，啟用虛擬環境。在 Windows (PowerShell) 中：
    ```bash
    .venv\Scripts\activate
    ```
    在 macOS 或 Linux 中：
    ```bash
    source .venv/bin/activate
    ```

3.  **安裝依賴套件**

    啟用虛擬環境後，使用 `uv pip sync` 來安裝 `uv.lock` 文件中鎖定的所有依賴套件。這能確保您安裝的版本與專案開發時完全一致。同時，我們也需要 `jupyter` 來運行筆記本。
    ```bash
    uv pip sync
    
    ```

4.  **設定 API 金鑰**

    這個專案需要使用 Google Gemini API。請在專案根目錄下建立一個 `.env` 文件，並在其中加入您的 API 金鑰：
    ```
    GOOGLE_API_KEY="YOUR_API_KEY_HERE"
    ```

## 🚀 如何運行

完成環境設定後，建議使用 `uv run` 指令來啟動 Jupyter Notebook。這個指令會自動在 `uv` 管理的虛擬環境中執行，無需手動啟用 (`activate`) 環境。

1.  **啟動 Jupyter Notebook**

    在專案的根目錄下，執行以下指令：
    ```bash
    uv run jupyter lab
    ```

2.  **開啟筆記本**

    上述指令會自動在您的預設瀏覽器中開啟 Jupyter 介面。如果沒有，您可以手動複製終端機中顯示的網址（通常包含一個 token）。

    接著，點擊並打開 `main.ipynb` 文件。

3.  **運行儲存格**

    您可以從頭到尾依次運行每個儲存格，以觀察整個 RAG 流程的運作。

---

這只是一個基礎的學習和演示專案，還有很多可以改進的地方。

---
由 falltwo 維護
