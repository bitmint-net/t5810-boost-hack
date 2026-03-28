## TBU .ffsファイルの注入手順（UEFITool使用）- シングルソケット向け

シングルソケット向けはPchS3Peimモジュールを**置き換える**方式。  
デュアルソケット向けの「Insert after」とは異なり、**「Replace as is」**を使う。

---

### 前提

- 使用するUEFIToolは **旧版（バージョン0.28系 / old_engine）** であること  
  ⚠️ 新しい「NE」版（New Engine）では編集ができない  
  → GitHubの `LongSoft/UEFITool` のReleasesページで `UEFITool_0.28.0` をダウンロードする
- 注入する `.ffs` ファイルはMiyconst One DriveのTBU 2023 Q4フォルダから事前にダウンロードしておく
- 前のステップで保存したBIOSファイルを使用する
- **事前確認：BIOSにPchS3Peimモジュールが存在するか確認してから作業を始める**  
  存在しない場合はDXEドライバを使う方法（デュアルソケット向け手順）に切り替える

---

### 手順

1. `UEFITool.exe` を**右クリック → 管理者として実行**する

2. **BIOSファイルを開く**
   - メニュー `File` → `Open image file...` をクリックする
   - 前のステップで保存したBIOSファイルを選択する  
     例：`T5810_BIOS_backup_mod2.bin`
   - 画面左側のStructureツリーにBIOSの構造が表示される

3. **PchS3Peimエントリを探す**
   - `PchS3Peim` はBIOS内に含まれるPEI（Pre-EFI Initialization）フェーズのドライバモジュール（GUID: `271DD6F2-54CB-45E6-8585-8C923C1AC706`）。このモジュールをTBU用カスタム版に丸ごと置き換えることで、CPU起動時およびスリープ復帰後にターボブーストアンロックが実行されるようになる
   - メニュー `File` → `Search...`（または `Ctrl+F`）を開く
   - `GUID` タブを選択する
   - **Search scope** は **`Header only`** を選択する
   - 以下のGUIDを入力して `[Search]` をクリックする
```
     271DD6F2-54CB-45E6-8585-8C923C1AC706
```

   - 画面下部のMessagesエリアに検索結果が1行表示される
```
     GUID pattern "271DD6F2-..." found as "F2D61D27..." in 271DD6F2-... at header-offset 0h
```
   - その行を**ダブルクリック**すると、画面左側のStructureツリーが自動スクロールし、`271DD6F2-54CB-45E6-8585-8C923C1AC706`（Type: File / PEI module）の行が**青くハイライト**される
   - **検索結果に何も表示されない場合** → このBIOSにはPchS3Peimが存在しない  
     → デュアルソケット向け手順（DXEドライバ注入）に切り替える

4. **ffsファイルで置き換える**
   - Structureツリーで青くハイライトされている `271DD6F2-54CB-45E6-8585-8C923C1AC706` の行を**右クリック**する
   - コンテキストメニューから **`Replace as is...`** を選択する  
     ⚠️ `Insert after` ではなく必ず `Replace as is` を選ぶこと
   - ファイル選択ダイアログが開くので、事前にダウンロードしておいた `.ffs` ファイルを選択して `[開く]` をクリックする
   - Structureツリーで以下のように表示されれば置き換え成功
     - 元の `271DD6F2-...` のAction列に **`Remove`** と表示される
     - その直下に新しい `271DD6F2-...` が追加され、Action列に **`Replace`** と表示される
     - 周囲の関連エントリのAction列に **`Rebuild`** と表示される

5. **変更を保存する**
   - メニュー `File` → `Save image file...` をクリックする
   - **元のファイルとは別の名前**で保存する  
     例：`T5810_BIOS_backup_mod3.bin`
   - 保存が完了したらUEFIToolを閉じる

---

### 注意事項

- `Replace as is...` がグレーアウトして選択できない場合、NE版を使用している可能性がある → 0.28系に切り替える
- 保存後、再度UEFIToolでファイルを開き、PchS3Peimエントリが変更済みになっていることを確認してから次のステップへ進む
- デュアルソケット構成の場合はこの手順ではなく、別途「デュアルソケット向け手順」を参照すること
