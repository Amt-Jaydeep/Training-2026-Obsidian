# 🧠 Obsidian 環境安裝與設定

---

## A. ⚙️ 安裝

1. 安裝 Obsidian ✅
   * 下載：https://obsidian.md/download

2. 安裝 Google Drive 桌面版 ✅
   * 下載：https://www.google.com/drive/download/

3. 登入 Google Drive 並掛載雲端硬碟 ✅
   1. 開啟 Google Drive App → 登入 Google 帳號
   2. 操作過程可以，直接點 **略過**（不需要額外同步 Downloads 之類的本機資料夾），直到完成
   3. 完成後等它掛載，用以下指令驗證：
      ```powershell
      Test-Path "G:\我的雲端硬碟"
      ```
   > 💡 磁碟機代號可參考裝置管理員

4. 建立 Vault 資料夾 ✅
   ```powershell
   New-Item -Path "G:\我的雲端硬碟\Secondbrain" -ItemType Directory
   ```
   驗證：
   ```powershell
   Test-Path "G:\我的雲端硬碟\Secondbrain"
   ```

5. 用 Obsidian 開啟 Vault ✅
   * 開啟 Obsidian → **Open folder as vault** → 指到 `G:\我的雲端硬碟\Secondbrain`
   * 驗證資料夾內自動產生 `.obsidian` 子資料夾：
     ```powershell
     Test-Path "G:\我的雲端硬碟\Secondbrain\.obsidian"
     ```

---

## B. 🔗 設定（串接 mcpvault / Agent：OpenCode）

6. 安裝 mcpvault ✅
   ```powershell
   npm install -g @bitbonsai/mcpvault
   ```
   確認安裝位置：
   ```powershell
   where.exe mcpvault
   ```

7. 寫入 OpenCode MCP 設定 ✅
   > ⚠️ 重要：OpenCode 實際讀取的設定檔是 `opencode.jsonc`（副檔名帶 `c`），不是 `opencode.json`。兩個檔案容易搞混，寫錯位置 OpenCode 不會讀到。

   設定檔位置：
   ```
   C:\Users\<你>\.config\opencode\opencode.jsonc
   ```

   寫入內容（PowerShell，分步驟）：

   **A. 準備內容（貼上 PowerShell，先預覽不寫入）**
   ```powershell
   @'
   {
     "$schema": "https://opencode.ai/config.json",
     "mcp": {
       "obsidian": {
         "type": "local",
         "command": ["npx", "@bitbonsai/mcpvault", "G:\\我的雲端硬碟\\Secondbrain"],
         "enabled": true
       }
     }
   }
   '@
   ```
   > 💡 這段是貼到 PowerShell 執行的指令，不是打開檔案手動編輯。`@' ... '@` 是 PowerShell 的多行字串寫法。
   > 貼上時若跳出「警告：您即將貼上含有多行的文字」，點 **仍要貼上** 即可，這是 Windows Terminal 的正常安全提示。

   **B. 實際寫入檔案**（在①的基礎上，最後加一行 `| Out-File ...`）
   ```powershell
   @'
   {
     "$schema": "https://opencode.ai/config.json",
     "mcp": {
       "obsidian": {
         "type": "local",
         "command": ["npx", "@bitbonsai/mcpvault", "G:\\我的雲端硬碟\\Secondbrain"],
         "enabled": true
       }
     }
   }
   '@ | Out-File -FilePath "$env:USERPROFILE\.config\opencode\opencode.jsonc" -Encoding utf8
   ```

   **C. 確認寫入成功**
   ```powershell
   Get-Content "$env:USERPROFILE\.config\opencode\opencode.jsonc"
   ```

8. 建立 CLAUDE.md
   * 先不用（後續有需要再補）

9. 重啟 OpenCode 並驗證 ✅
   * A. 完全關閉 OpenCode，重新開啟
   * B. 測試讀取：
      ```
      請列出我的 Obsidian vault 根目錄的資料夾。
      ```
      右側面板應顯示 **MCP → obsidian: Connected**
   * C. 測試寫入：
      ```
      請在我的 Obsidian 建立一則測試筆記，內容寫「OpenCode 已成功連接 Obsidian」。
      ```
      完成後打開 Obsidian 確認筆記真的出現在 Vault 裡

---


## C. 🚀 進階（未執行先記錄，需要時再安裝）

### CLI-Anything Obsidian CLI

若需要全文檢索、metadata 操作、tag 管理、筆記分析等進階功能，可安裝 CLI-Anything 的 Obsidian CLI。

1. **前置需求**
A. 在 Obsidian 內安裝 [Local REST API](https://github.com/coddingtonbear/obsidian-local-rest-api) 社群插件
B. 插件設定中啟用 HTTPS（非必要）並記下 API key

2. **安裝**
```bash
pip install cli-anything-hub
cli-hub install obsidian
```

3. **使用範例**
```bash
cli-anything-obsidian search "SKILL.md"
cli-anything-obsidian tag list
cli-anything-obsidian note get "專案工作流程.md"
```

> 💡 mcpvault（本文件主線方案）適合基本讀寫；Obsidian CLI 適合需要查詢、分析、批量操作的情境。兩者可並存。

---

## 📝 備註

* 同台電腦使用兩個 Google 帳號 ( 規劃方式待確認)
