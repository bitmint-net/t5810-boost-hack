## CPU C状態オプションの設定手順（AMIBCP5使用）

AMIBCPはBIOSファイル内の設定値をPC上で直接書き換えるツール。
BIOSに焼き込む前にファイルを編集するため、変更はBIOSへの書き込み後に反映される。

---

### 前提

- 前のステップ（06F2マイクロコード削除）で保存したBIOSファイルを使用する
- 作業前に対象BIOSファイルを別フォルダにバックアップしておくこと

---

### 手順

1. `AMIBCP5.exe` を**右クリック → 管理者として実行**する

2. **BIOSファイルを開く**
   - メニュー `File` → `Open` をクリックする
   - ファイル選択ダイアログで、前のステップで保存したBIOSファイルを選択する  
     例：`T5810_BIOS_backup_mod.bin`
   - 画面左側にBIOS設定のツリーが展開される

3. **CPU C State Controlを探す**
   - 左ペインのツリーを以下の順に展開していく  
     `Common RefCode Configuration`  
     　→ `IntelRCSetup`  
     　　→ `Advanced Power Management Configuration`  
     　　　→ `CPU C State Control`
   - `CPU C State Control` をクリックすると、右ペインに設定項目の一覧が表示される

4. **各設定値を変更する**
   - 右ペインの表に `Optimal` 列がある。この列の値をダブルクリックすると変更できる
   - 以下の3項目をそれぞれ変更する

   | 設定項目 | 変更後の値 |
   |---|---|
   | Package C State limit | `C2 state` |
   | CPU C3 Report | `Enabled` |
   | CPU C6 Report | `Disabled` |

   - 各項目のOptimal列をダブルクリックするとドロップダウンが表示されるので、上記の値を選択する

5. **ファイルを保存してAMIBCPを閉じる**
   - メニュー `File` → `Save` をクリックする（上書き保存）  
     または `File` → `Save As` で別名保存する場合はわかりやすい名前をつける  
     例：`T5810_BIOS_backup_mod2.bin`
   - 保存確認ダイアログが表示されたら `[Yes]` をクリックする
   - AMIBCPを閉じる

---

### 注意事項

- `CPU C State Control` が見つからない場合は、パスが環境により異なる場合がある。  
  ツリー上部の検索機能（`Ctrl+F` など）で `C State` と入力して検索する
- Optimal列の値を変更しても、見た目上すぐ反映されないことがある。保存後に再度ファイルを開いて確認するとよい
- この段階ではまだBIOSへの書き込みは行っていない。次のステップでBIOSチップへの書き込みを行う
