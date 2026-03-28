## 日本語環境でMMToolのGUI表示を修正する手順

MMToolはAMI BIOS用のツール。日本語版Windowsでは画面の一部が表示されず、モジュール操作ができない。
Resource HackerでMMTool内のフォント設定を書き換えることで解決する。

---

### 必要なもの

- `MMTool.exe`
- `Resource Hacker`（ZIP版）  
  → `http://www.angusj.com/resourcehacker/` のページ最下部「ZIP install」からダウンロード  
 
---

### 手順

1. `ResourceHacker.exe` を起動する
2. `MMTool.exe` をResource Hackerで開く  
   ドラッグ＆ドロップ、または `Ctrl+O` で指定
3. 左ペインで `Dialog` → `102 : 1033` を選択する  
   右ペインに詳細が表示され、別ウィンドウ「Dialog - 102」が開く
4. 右ペインの `FONT 8, "MS Sans Serif"` を以下に書き換える

   ```
   FONT 10, "Tahoma"
   ```

5. `▶`ボタン（または `F5`）で **Compile Script** を実行する
6. **別名で保存する**（`File` → `Save As...`）  
   元の `MMTool.exe` は上書きしない

