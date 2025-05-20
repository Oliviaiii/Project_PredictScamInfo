AI 訊息辨識與預警系統 (ScamGuard AI)
專案簡介
面對日益猖獗的網路詐騙，每年詐騙案件數量與金額屢創新高。詐騙訊息形式多樣，從傳統的電話、簡訊，延伸到 LINE、Facebook、Instagram 等社交平台，讓人們難辨真偽，所有人都可能成為受害者。

ScamGuard AI 旨在建立一套智慧型的 AI 系統，能即時辨識潛在的詐騙訊息，並提示可疑字詞，以達到預警目的，幫助使用者有效防範詐騙。

專案目標
即時詐騙辨識：使用者輸入文字訊息（例如：貼上 LINE 簡訊），AI 模型將分析內容語意，預測該訊息是否為詐騙。
可疑詞句提示：系統會顯示訊息中被識別為可疑的片段或關鍵字（如：「中獎」、「連結」、「帳號異常」），提升使用者對風險的感知。
模型可信度顯示：提供模型預測的可信度百分比，讓使用者了解判斷的依據。
歷史紀錄儲存：未來可儲存歷史偵測紀錄，建立個人警示資料庫。
未來擴充性：期望未來能擴充為 LINE 機器人、Chrome 瀏覽器外掛、手機 APP 等多平台應用。
技術領域
本專案主要應用自然語言處理 (NLP - Natural Language Processing) 領域的技術，結合文字分類與語意理解，專注於詐騙訊息的偵測。

模型架構：BERT + LSTM + CNN
我們採用混合式模型架構，結合了當前 NLP 領域的先進技術，以達到精準的詐騙訊息辨識能力：

資料前處理：
移除雜訊符號：清洗輸入文本，去除不必要的特殊字元或亂碼。
Tokenizer (分詞器)：將文本切割成模型可理解的最小單位，並轉換為數值表示。
針對 BERT，會使用其專用的 Subword Tokenization。
特徵擷取 (BERT)：
利用預訓練的 BERT (Bidirectional Encoder Representations from Transformers) 模型，為每個詞彙生成具有上下文語意的詞向量 (Contextualized Word Embeddings)。BERT 能夠理解詞語在不同語境下的真實含義，對於捕捉詐騙訊息中的語氣和語意變化至關重要。
語序理解模組 (LSTM)：
在 BERT 產生的詞向量基礎上，使用 LSTM (Long Short-Term Memory) 網路。LSTM 是一種遞歸神經網路 (RNN) 的變體，擅長處理序列資料並捕捉文本中的長期依賴關係，有助於理解訊息的語序、語氣變化和語意遞進。
局部片語特徵提取 (CNN)：
引入 CNN (Convolutional Neural Network) 來提取文本的局部特徵。CNN 能夠透過不同大小的卷積核，有效捕捉訊息中常見的固定或半固定的詐騙片語模式，例如「立即點擊」、「帳號異常」、「恭喜中獎」等關鍵詞串。
分類器 (Dense + Softmax)：
最終的分類層，將前幾層提取到的複雜特徵映射到二元分類（詐騙或非詐騙）空間。Softmax 函數會輸出訊息為「詐騙」或「非詐騙」的機率。
輸出：0 代表「詐騙訊息」；1 代表「非詐騙訊息」。
輸出顯示：
向使用者顯示風險提示（如：⚠️ 可疑訊息）。
提供模型預測的可信度百分比。
將偵測到的可疑片段關鍵字在原始訊息中標紅，或以列表形式清晰呈現。
專案架構 (前後端分離)
本專案採用前後端分離的架構，使開發、部署和擴展更加靈活。

Project_PredictScamInfo/
├── frontend/             # 使用者前端畫面 (純靜態頁面)
│   ├── index.html        # 主頁面 (文字輸入、按鈕、結果顯示)
│   ├── style.css         # CSS 樣式
│   └── script.js         # JS 腳本，負責發送 POST 請求給 FastAPI 後端
├── backend/              # 後端 (FastAPI)
│   ├── main.py           # FastAPI 主應用 (含 /predict API)
│   ├── model/            # 模型與預測邏輯
│   │   ├── model.pkl     # (未來) 訓練好的模型檔案
│   │   ├── predictor.py  # 載入模型與執行預測的程式
│   │   └── (bert_tokenizer_files/) # BERT tokenizer 相關檔案 (如 vocab.txt)
│   ├── requirements.txt  # 所需 Python 套件 (fastapi, uvicorn, tensorflow/torch, transformers 等)
│   └── .env              # 環境變數 (例如模型路徑，應加入 .gitignore)
├── data/                 # (可選) 原始訓練資料與前處理檔案
│   └── messages.csv      # 範例資料檔案
├── README.md             # 專案說明文件
└── .gitignore            # Git 版本控制忽略文件
如何運行專案
前置準備
複製專案：
Bash

git clone <你的專案Git網址>
cd Project_PredictScamInfo
設定 Python 虛擬環境 (建議)：
Bash

python3 -m venv venv_backend
source venv_backend/bin/activate  # macOS/Linux
# 或 .\venv_backend\Scripts\activate  # Windows
安裝後端依賴： 進入 backend 目錄，安裝 requirements.txt 中列出的所有套件。
Bash

cd backend
pip install -r requirements.txt
(請確保 requirements.txt 包含了 fastapi, uvicorn, pydantic, tensorflow 或 torch, transformers 等你模型所需的套件)
# backend/requirements.txt 範例
fastapi
uvicorn
pydantic
# 以下為模型可能需要的套件，請根據實際情況添加或修改
# tensorflow # 或 torch
# transformers
# numpy
# pandas
# scikit-learn
確保 frontend 檔案存在： 確保 frontend/index.html, frontend/style.css, frontend/script.js 檔案都已存在。
啟動後端服務
切換到 backend/ 目錄：
Bash

cd backend
運行 FastAPI 應用：
Bash

uvicorn main:app --reload --port 8000
你應該會看到類似 Uvicorn running on http://127.0.0.1:8000 (Press CTRL+C to quit) 的訊息。這表示後端 API 服務已經在運行。
啟動前端介面
直接開啟 index.html： 在你的瀏覽器中，直接開啟位於 Project_PredictScamInfo/frontend/index.html 的檔案。 (你可以直接雙擊檔案，或將檔案拖曳到瀏覽器視窗中)。
使用與測試
在瀏覽器中打開的網頁，在文字輸入框中輸入你想要檢測的訊息。
點擊「檢測！」按鈕。
頁面下方將會顯示模擬的檢測結果，包含是否為詐騙、可信度以及可疑詞句。
開發進度與未來展望
目前專案已完成前後端的基本串接，並具備模擬的詐騙訊息辨識功能。下一步將著重於整合實際訓練好的 AI 模型，並逐步實現歷史紀錄儲存等進階功能。