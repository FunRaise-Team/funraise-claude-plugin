---
name: funraise
description: 用 Funraise 方睿的台灣不動產與企業資料回答問題 —— 實價登錄（買賣／租賃／預售）、商辦大樓、地籍與地號、使用分區、都市更新、產業園區、市地重劃、公司登記與統編、上市櫃取得處分資產、區域市場量價走勢。使用者輸入 /funraise，或問到台灣的房地產、土地、商辦、廠辦、企業登記資料時使用。Use for Taiwan real-estate, land and company-registry questions answered from the Funraise connector.
---

# 用 Funraise 資料回答

使用者在 `/funraise` 後面寫的就是問題。沒有寫問題時，問他要查什麼（地點、標的、期間）。

## 1. 先確認連接器在

Funraise 的工具名稱都是 `{mcp_id}__{tool_name}` 的形狀，例如 `actual-price-sale__search_actual_sales`、`land-info__land_boundary`。在可用工具裡找這個形狀的工具。

- **一支都找不到**：Funraise 連接器還沒連上。請使用者到本 plugin 的 **Connectors** 分頁按連線（或自行新增連接器 `https://connector.mcp.funraise.ai/c/default/mcp`），用 Google 或 Microsoft 帳號登入。**不要改用網路搜尋或自己的知識冒充 Funraise 的資料。**
- **找得到，但缺某個資料源**：使用者的設定只開了部分資料源。說清楚缺的是哪一個（用下方參考檔裡的名稱），並請他到 https://app.mcp.funraise.ai/admin/ 開啟。
- 工具清單以實際可用的為準，不要拼湊清單裡沒有的名字。

## 2. 挑資料源、讀 schema、再呼叫

1. 依 `references/data-sources.md` 決定要用哪些資料源。
2. **呼叫前讀該工具 `inputSchema` 裡每個參數的 `description`**，不要靠參數名猜語意。伺服器會拒絕 schema 沒宣告的參數。
3. 需要多個資料源時分別呼叫。有批次工具就用批次（例如 `business-registry__lookup_businesses_batch`、`land-info__land_boundaries_batch`），不要一筆一筆打 —— 逐筆呼叫會慢很多。
4. 回答時列出用了哪些資料源與查詢條件（縣市、行政區、期間、篩選），讓使用者能核對。

## 3. 怎麼讀回應

- **`accepted: false` 加 `problem`**：參數有問題，伺服器沒有執行查詢。照 `problem` 的指示修正後重試；**這不是「查無資料」**。
- **`found: false`**：查過了，確實沒有。這本身就是答案。
- **`isError: true` 或錯誤回應**：真的失敗了。告訴使用者，不要編造數字。
- 回應裡有 `coverage_note`、`match_note`、`notes`、`data_quality` 時一定要讀，並把其中的限制轉述給使用者。
- **合法的 0 筆比錯誤更容易誤導。** 拿到 0 筆時，先檢查篩選條件的寫法（見下一節），再下「沒有」的結論。

## 4. 已知陷阱

- **辦公室用 `main_use`（如「辦公用」「一般事務所」），不是 `building_type`。** 很多辦公室登記在住宅大樓或華廈裡，用 `building_type` 會漏掉大半。
- **`actual-price-sale__aggregate_sales_by_district` 的 `year_range` 是建築完成年，不是成交年。** 依成交年統計要用 `transaction_year_range`。
- **行政區只填行政區名**（`中山區`），縣市另外用 `city` 參數給；把「台北市中山區」整串塞進 `district` 會得到 0 筆。
- **查地段一律帶 `city`。** 段碼與段名在不同縣市會重複（例如 `0421` 在 10 個縣市都存在）。地號可用 `392-6` 這種寫法；回應裡的 `landno` 可原樣拿去做下一次查詢。
- **統編是 8 碼。** 使用者少打前導零時可以直接查，伺服器會補零。
- **付費工具（`transcripts__crawl_*`）一定分兩步**：先用 dry-run 取得費用估算，告訴使用者金額，等他明確同意才執行。絕不自行執行付費爬取。
- 回應尾端出現本月剩餘次數的提醒時，轉告使用者。
