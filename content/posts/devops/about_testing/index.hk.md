---
title: 關於Testing
date: 2026-01-02
summary: 📝 Testing嘅快速筆記...
tags:
  - devops
  - testing
---

## 結構化測試

- 喺實作之前, 先將改動範圍分類, 用嚟決定測試要做到幾深入:
  - Domain / Logic Layer: 業務規則或計算邏輯
  - Data Persistence Layer: DB嘅schema, query同index
  - Transport / Interface Layer: API contract, routing或middleware
  - Cross-Cutting Concerns: 效能, 安全性或並發

### 測試分類與策略

#### 單元測試

- 目標: Service或Domain入面嘅單一function或method
- 成功情境: 驗證合法輸入會唔會得到預期輸出
- 邊界值分析(BVA): 測試最小值, 最大值同啱啱超出範圍嘅數值
- 錯誤處理: 確保對於非法輸入, 系統會throw正確嘅exception或error
- 隔離性: 對外部依賴使用Mock或Stub, 避免受其他模組影響

#### 整合測試

- 目標: Repository層同模組之間嘅溝通
- 臨時環境: 使用Docker container模擬真實嘅Database或Redis環境做測試
- 狀態驗證: 確保Repository寫入嘅資料可以正確讀返出嚟, 並符合schema限制
- 副作用: 驗證邏輯改動之後, 相關事件(例如MQ trigger)有冇正常運作

#### API與Contract測試

- 目標: HTTP或gRPC嘅request同response行為(Handler層)
- Payload驗證: 確保API會拒絕格式錯誤嘅JSON, 並回傳正確嘅HTTP status code
- 認證同授權: 測試冇有效token嘅情況下, 受保護嘅route係入唔到
- End-to-End流程: Request → Handler → Service → DB → Response

#### 持續品質保證

- Bug-Driven Development: 每次發現bug, 都要喺改之前先寫一個可以重現問題嘅測試, 確保之後唔會再返嚟
- 冪等性測試: 確保同一個API或function重複call多次, 都唔會導致資料狀態不一致

#### 自動化與 CI Pipeline

- 設定CI, 以下情況一律block merge:
  - 測試失敗
  - 測試覆蓋率下降
  - Lint或Style檢查唔通過
    - Dead Code: 宣告咗但從來冇用過嘅變數
    - 安全風險: Hardcode密碼, 使用insecure或deprecated嘅function

## CI/CD

### 定義

- CI: Continuous Integration
  - 每次push code → 自動run測試 → build專案 → 確保冇嘢壞咗
- CD: Continuous Deployment / Delivery
  - CI 成功之後, 自動將程式部署喺server或hosting platform

### 工作流程

#### GitHub Actions
1. Push code喺GitHub
2. GitHub Actions開始執行CI
   - Install dependencies
   - Build專案
   - Run tests
3. CI全部通過之後執行CD
4. 應用程式自動更新
