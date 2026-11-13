<!-- 產品／專案：Slash1to3｜法人／事業體：斜槓一二三 / Slash123（獨立法人；完整法定名稱 [暫定]／[待確認]）｜日期：2026-11-13（Asia/Taipei）｜草稿 v0.3.1 -->
# Slash1to3
C2-AE-S13 斜槓一二三 Slash123 個人專家數位分身能力授權平台 for 自由工作者、專家、微型任務
# 斜槓一二三 (slash1to3.com) - 專家數位分身能力授權與標售平台

[![Repository Archetype: Functional App](https://img.shields.io/badge/Archetype-C3--Functional--App-blue.svg)](#)
[![PMO ID](https://img.shields.io/badge/PMO--ID-C3--AE--S13--F-orange.svg)](#)
[![Domain](https://img.shields.io/badge/Domain-aegis.expert-green.svg)](#)
[![Security Level](https://img.shields.io/badge/Security-FHE%20%2F%20Zero--Knowledge%20Inference-critical.svg)](#)

> **核心定位**：平台本身**不訓練模型、不收存明文知識包**。本平台是將部署於地端 **密文智庫（EncryMind）** 上已成熟的專家能力，轉化為「可授權、可標售、可計量、可稽核」之數位分身的權利交易與驗證中樞。

---

## 1. 核心架構哲學與原則

1. **買授權，不買資料本體**：買方取得的是「向專家分身提問並解密答案」的權利（Inference-only Capability），原始文件、配方與權重永不出地端 NAS。
2. **金鑰掌控，硬體驗證**：解密權利透過密鑰或實體 USB 安全金鑰（FIDO2 / Secure Element）執行；USB 不在位時，即使權限有效亦無法解出答案。
3. **匿名使用，不可否認**：平台與第三方無法窺探買方提問與回答內容；每次調用僅留下可驗證之不可逆授權收據（Cryptographic Audit Receipt）。
4. **帳號與分身解耦**：專家個人身分與數位分身（Twin）分離。單一專家可依領域與邊界繁衍多個獨立分身，分別進行授權標售。

---

## 2. 系統功能架構五大維度

```
                    ┌──────────────────────────────────────────┐
                    │          斜槓一二三 平台控制平面             │
                    │   (專家帳號 / 分身市集 / 授權狀態 / 帳務)   │
                    └───────────────┬──────────────────────────┘
                                    │ (授權驗證 / 狀態同步)
        ┌───────────────────────────┴───────────────────────────┐
        ▼                                                       ▼
┌──────────────────────────────┐              ┌──────────────────────────────┐
│       專家側密文智庫         │              │        買方提問端 / 客戶端     │
│ ┌──────────────────────────┐ │              │ ┌──────────────────────────┐ │
│ │ 地端保護區 (Knowledge)    │ │              │ │ 實體 USB 金鑰 / 本地 Client │ │
│ └────────────┬─────────────┘ │   密文推理    │ └────────────┬─────────────┘ │
│ ┌────────────▼─────────────┐ │ ◄──────────► │ ┌────────────▼─────────────┐ │
│ │ 本地 SLM / LLM (分身能力) │ │ (FHE/TLS通道)│ │ 匿名提問 / 本地解密        │ │
│ └──────────────────────────┘ │              │ └──────────────────────────┘ │
└──────────────────────────────┘              └──────────────────────────────┘
```

### 2.1 專家側：把所知變成可問的能力
* **NAS 節點綁定**：專家透過安全協議註冊其密文智庫，平台僅維持心跳（Heartbeat）與狀態可信證明，無法讀取 NAS 儲存區。
* **地端知識包隔離**：研究、案例、配方與問答集只進 NAS 受保護區，完全不傳輸至平台伺服器。
* **本機能力化**：本地端小語言模型（SLM）或私有模型透過受保護知識完成微調或 RAG 索引，長出高專精推論能力。
* **分身（Twin）繁衍與邊界定義**：
  * 單一專家可衍生多個數位分身，每個分身對應特定的領域知識子集。
  * **Skills 規格定義**：清晰規範「能回答什麼」、「不能回答什麼（邊界）」、適用情境、版本與更新頻率。
* **上架與曝光控制**：專家可針對分身設定是否公開、限定白名單或暫停接受新授權。

### 2.2 授權與金鑰：誰能問、誰能解
* **密鑰授權體系**：交付「能問、能解」的憑證，支援 USB 硬體金鑰或與 USB 綁定的短命憑證（Capability Token）。
* **分級權限矩陣**：
  * `QNA_ONLY`：僅限問答推理。
  * `WITH_CITATION`：允許查閱引用來源片段索引（不含原始全文）。
  * `OFFLINE_CIPHERTEXT`：允許暫存密文結果供離線驗證。
  * `REAUTHORIZE`：再授權許可（預設停用，嚴禁轉售）。
* **生命週期與額度管理**：授權可約束期限（Valid Until）、調用次數（Quota）、領域範疇與是否允許轉讓。
* **即時撤銷與邊界控制**：撤銷授權後，新請求立即失敗；已離線持有之舊密文是否能解，取決於 USB 實體金鑰策略。

### 2.3 市場：智慧轉型標售
* **分身資產卡片**：展示專長領域、Skills 清單、問答範例邊界、授權模式、計價方案及「已轉出 N 個分身」履約紀錄。
* **交易機制**：支援公開定價訂閱、次數包購買與限額競標；標的為「權利物件（Grant）」，非資料集。
* **檢索與篩選**：依技能標籤、USB 支援性、匿名評級與計費方式過濾。
* **防洩漏試問機制**：未購買前僅提供公開合成示範，或回傳經混淆之密文樣本，嚴禁試出核心知識。

### 2.4 使用與紀錄：匿名但不可否認
* **端對端加密通道**：持鑰買方對分身 NAS 直連提問，不經由平台伺服器中繼明文。
* **零知識日誌架構**：
  * 平台與公開節點完全無法反推發問者身分與 Prompt/Response 明文。
  * 日誌僅記錄：`分身識別碼 (Twin ID)`、`授權不可逆指紋 (Grant Hash)`、`時間戳記`、`成功/拒絕狀態`、`額度扣減值`。
* **不可否認性（Non-repudiation）**：每次請求生成節點與金鑰簽署的收據（Audit Receipt），支持雙方內部稽核。

### 2.5 平台本體與治理
* **帳號實體分離**：專家帳號（真實法律實體）與分身帳號（數位資產單位）明確解耦。
* **金鑰隔離原則**：平台僅託管授權狀態關係與 ACL，不持有任何可用於解密的私鑰或對稱金鑰。
* **零明文爭議仲裁**：客訴爭議僅比對憑證、收據指紋、Skills 描述與調用成功率，不調閱任何對話內容。
* **評價治理體系**：僅針對「回答範疇是否符合宣告之 Skills 邊界」評分，不揭露問答細節。

---

## 3. 版本邊界與 MVP 交付定義

### 3.1 最小可用版本 (MVP 交付範圍)
- [x] 專家綁定密文智庫節點與心跳偵測。
- [x] 分身建立、Skills 邊界定義與上架設定。
- [x] 權利合約生成、定價與金鑰授權管理。
- [x] USB 綁定與短命權限憑證簽發（5–15 分鐘過期機制）。
- [x] 持鑰驗證提問通道與額度原子扣除。
- [x] 匿名、不可否認之鏈下審計收據生成。

### 3.2 第一版明確排除項目 (Out of Scope for v1)
- ❌ **雲端機器人化**：不提供託管於公有雲、脫離 NAS 的對外公開聊天機器人。
- ❌ **原始檔案下載**：不提供匯出或下載專家原始知識包（Raw Corpus）的功能。
- ❌ **明文對話歷史庫**：平台不儲存任何買賣雙方的歷史問答文本日誌。
- ❌ **無授權試用滲透**：嚴禁未付款訪客透過模糊 Prompt 探測或抽取引導核心機密。
- ❌ **S0 單向輸入加密架構（Legacy）**：不採用僅於前端輸入端加密的套殼方案。
  - 註：HEShell 採 L0–L3 分級，不以 S0–S3 評級；S0 指架構類型，不指特定產品。
  - 本平台 v1 將整合 HEShell（神盾殼）。整合方式與時程 [待確認]。

---

## 4. 核心資料模型規格

### 4.1 授權物件 (Grant Object Schema)
```json
{
  "$schema": "https://slash1to3.com/schemas/grant-v1.json",
  "grant_id": "gr_01J8F9X...",
  "twin_id": "twin_chem_expert_88",
  "holder_thumbprint": "sha256:e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855",
  "right_level": "QNA_ONLY",
  "usb_binding": {
    "required": true,
    "key_attestation_id": "fido2:aaguid:4a5c..."
  },
  "constraints": {
    "valid_from": "2026-10-01T00:00:00Z",
    "valid_until": "2026-12-31T23:59:59Z",
    "quota_queries_total": 500,
    "allowed_skill_domains": ["fine_chemicals", "process_safety"]
  },
  "revocation_epoch": 1,
  "state": "ACTIVE"
}
```

### 4.2 匿名審計收據 (Audit Receipt Schema)
```json
{
  "$schema": "https://slash1to3.com/schemas/receipt-v1.json",
  "receipt_id": "rcpt_98234...",
  "twin_id": "twin_chem_expert_88",
  "grant_fingerprint": "b7f9a2e1...",
  "timestamp": "2026-10-10T12:34:56Z",
  "quota_consumed": 1,
  "execution_status": "SUCCESS",
  "node_signature": "ed25519:3b9f...",
  "client_auth_proof": "dpop:thumbprint:7a2c..."
}
```

---

## 5. Repository 檔案結構骨架

```
aegis-slash1to3-app/
├── README.md                     # 本專案核心總覽與規範說明
├── ARCHITECTURE.md               # 系統細部架構、安全邊界與時序圖
├── REQUIREMENTS.md               # 追溯需求矩陣 (FR-xxx, NFR-xxx, SEC-xxx)
├── QA-ACCEPTANCE.md              # 七道 QA 閘門檢驗標核清單與測試案例
├── TEST-PLAN.md                  # 單元、端對端、模糊測試與紅隊穿透計畫
├── DEPLOYMENT.md                 # 混合部屬指南 (Cloud Gateway + Edge NAS Node)
├── SECURITY.md                   # 密鑰邊界、個資法遵 (PDPA) 與漏洞通報指引
├── CONTRIBUTING.md               # 協作分支策略、PR 規範與 CODEOWNERS 說明
│
├── .github/
│   ├── CODEOWNERS                # 模組審查權責分配
│   ├── pull_request_template.md  # PR 提交檢核模板
│   └── workflows/
│       ├── ci-gate-checks.yml    # Lint, TypeCheck, Unit Tests
│       └── security-scan.yml     # Secret Scanning, SBOM, SAST
│
├── apps/
│   ├── web-portal/               # 專家控制台與分身市場 UI (Next.js / SSR)
│   ├── rights-gateway/           # 授權檢查、額度扣減與收據簽發中樞
│   └── nas-agent/                # 運行於專家密文智庫的本機守護服務
│
├── packages/
│   ├── crypto-core/              # 密鑰封裝、不可逆指紋、USB FIDO2 驗證庫
│   ├── protocol-schemas/         # JSON Schema, protobuf 與 OpenAPI 規格定義
│   └── audit-logger/             # 匿名不可否認審計日誌客戶端
│
└── tests/
    ├── e2e/                      # 端對端流程：註冊→標售→持鑰提問→收據驗證
    ├── mock-nas/                 # 用於 CI/CD 模擬測試的虛擬 NAS 節點
    └── red-team/                 # 提示抽取防禦、越權存取與重放攻擊驗證
```

---

## 6. 本地開發與快速上手

### 6.1 前置環境需求
- Node.js `>= 20.x`
- Docker & Docker Compose (用於啟動模擬 NAS 節點與 Redis 額度原子扣除鎖)
- pnpm `>= 9.x`
- 相容 FIDO2 / CTAP2 之安全金鑰（或軟體虛擬 Authenticator）

### 6.2 啟動開發環境
```bash
# 1. 複製代碼庫
git clone https://github.com/aegis-expert/aegis-slash1to3-app.git
cd aegis-slash1to3-app

# 2. 安裝相依套件
pnpm install

# 3. 啟動本機相依服務與虛擬 NAS 測試節點
docker compose -f docker/docker-compose.dev.yml up -d

# 4. 初始化環境變數
cp .env.example .env.local

# 5. 啟動平台控制平面與 Gateway
pnpm dev
```

### 6.3 執行整合測試與資安驗證
```bash
# 執行所有單元測試與能力矩陣驗證
pnpm test

# 模擬持鑰提問與匿名收據生成管線
pnpm test:e2e:inference-flow

# 執行防提示抽取 (Anti-Extraction) 邊界檢驗
pnpm test:security:prompt-isolation
```

---

## 7. QA 品質閘門檢驗標準 (7-Gate Governance)

本代碼庫遵循專案 PMO 制定之七道品質把關流程，PR 合併與發布均需通過自動化或人工驗證：

| 閘門代號 | 閘門名稱 | 核心查核重點 |
| :--- | :--- | :--- |
| **G1** | **Scope Ready** | 需求符合 MVP 邊界，確認無未授權試問與明文下載等排除項目。 |
| **G2** | **Design Ready** | API 符合零知識原則；授權與金鑰資料流通過威脅建模評估。 |
| **G3** | **Build Ready** | CI 構建完成，單元測試覆蓋率 $\ge 85\%$，靜態安全掃描零高危弱點。 |
| **G4** | **Integration Ready** | 雲端 Rights Gateway 與地端 NAS Agent 密文握手成功，心跳逾時判定正常。 |
| **G5** | **Content & Legal** | 標售頁 Skills 邊界宣告清晰；符合台灣個人資料保護法（PDPA）免責與規範。 |
| **G6** | **Release Ready** | USB 插拔中斷、額度並發扣減競爭條件（Race condition）壓力測試通過。 |
| **G7** | **Post-Release** | 生產節點不可否認收據鏈路通暢，監控無未預期明文日誌輸出。 |

---

## 8. PMO 角色職責與聯絡矩陣

依照專案組織控管規範，相關角色與自動轉發聯絡群組配置如下：

* **Repo Admin / 權限維護**：`admin-aegis-slash1to3-app@aegis.expert`
* **Lead Architect / 開發負責人**：`dev-aegis-slash1to3-app@aegis.expert`
* **QA & 驗收簽核團隊**：`qa-aegis-slash1to3-app@aegis.expert`
* **Collaborator 協同開發組**：`collab-aegis-slash1to3-app@aegis.expert`
* **全域資安應變應急 (CSIRT)**：`github-security@wonderland.center`


## v0.3.1 變更註記（2026-11-13，Asia/Taipei）

依使用者 2026-11-13 22:20 指示：刪除「偽隱私」用語，改為中性表述；確認本平台 v1 將整合 HEShell，整合方式與時程仍 [待確認]。本文件為草稿 v0.3.1，尚未最終確認。

## v0.3 變更註記（2026-11-11，Asia/Taipei）

僅修正本次涉及之品牌顯示名稱、網域文字及與 t12u 直接衝突的法人關係段落；repo／org、技術代碼、URL、路徑、分支及 email local part 均未變更。品牌與網域確認不代表內容核准；本文件為草稿 v0.3，其他來源規格僅供參考，未驗證的功能、效能、時程及合規不得視為核准。

### 人工審核清單

- 完整法定名稱、登記資訊、設立沿革、網域持有人與技術協作／託管契約仍以已確認錨點為限，其他 [待確認] 標註維持。
- 既有版權權利人與授權條款未由本次品牌修訂推定變更；請確認 README 的版權行及 LICENSE 第 189 行 `Copyright [yyyy] [name of copyright owner]` 範本尚未填寫之權利人與年份。
- 保留的技術 URL／徽章／代碼若含歷史文字，並非現行品牌或法人歸屬聲明；如需替換 URL 或改名須另行確認。
- 其餘效能、法遵、交付與產品主張未經核驗，均待人工審核，不因本次修字或 PR 合併視為核准。
