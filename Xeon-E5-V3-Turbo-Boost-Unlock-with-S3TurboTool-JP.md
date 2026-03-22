# Xeon E5 V3 Turbo Boost Unlock with S3TurboTool

### Xeon E5 V3 Turbo Boost Unlock - ステップバイステップガイド

必要なファイルとツールはすべて、私のMicrosoft One Driveからダウンロードできます：
Miyconst Hardware / LGA 2011-3 / TBU 2023 Q4.

#### 06F2 CPU マイクロコードの削除
1. **mmtool_a5.exe** アプリケーションを起動します。
2. **[Load Image]** ボタンをクリックし、BIOSファイルを選択します。
3. **[Cpu Patch]** タブに移動します。
4. テーブルの中から CPU ID が **06F2** に一致するすべてのエントリを探して削除します。
5. 変更したBIOSを保存します。

#### CPU C State オプションの設定
1. **AMIBCP5.exe** アプリケーションを起動します。
2. 前のステップで保存したBIOSファイルを開きます。
3. 左側のツリーから **CPU C State Control** オプションを探します。通常は以下のパスにあります：
   `Common RefCode Configuration` → `IntelRCSetup` → `Advanced Power Management Configuration` → `CPU C State Control`
4. 以下の値を設定します：
   * **Package C State limit** を **C2 state** に設定。
   * **CPU C3 report** を **Enable** に設定。
   * **CPU C6 report** を **Disable** に設定。
5. AMIBCP5 アプリケーションを閉じ、変更の保存に同意します。

#### 必要な TBU ドライバの注入 - シングルソケット
1. **UEFITool.exe** アプリケーションを起動します。
2. 前のステップで保存したBIOSファイルを開きます。
3. `Intel image` → `BIOS region` の中から、UIDが `271DD6F2-54CB-45E6-8585-8C923C1AC706` の **PchS3Peim** エントリを検索します。
4. **PchS3Peim** エントリを右クリックし、**[Replace as is...]** を選択します。
5. 注入したい目的の **.ffs** ファイルを選択します。
6. 変更を保存します。

#### 必要な TBU ドライバの注入 - デュアルソケット
1. **UEFITool.exe** アプリケーションを起動します。
2. 前のステップで保存したBIOSファイルを開きます。
3. `Intel image` → `BIOS region` の中から、最後から2番目の **FFSv2** エントリ（UID: `8C8CE578-8A3D-4F1C-9935-896185C32DD3`）を検索します。
4. その中の最後の **DXE** エントリを探します（私のケースでは UID: `A0327FE0-1FDA-4E5B-905D-B510C45A61D0`）。
5. それを右クリックし、**[Insert after...]** を選択します。
6. 注入したい目的の **.ffs** ファイルを選択します。
7. 変更を保存します。

#### BIOSの書き込み（フラッシュ）
BIOSを書き込みます（方法は下記参照）。再起動してBIOS設定に入ります。BIOSのデフォルト設定を復元（Restore Defaults）します。変更を保存して再起動します。追加の設定を調整したい場合は、再度BIOSに入ります。

改造したBIOSを書き込むには、以下のオプションがあります：
* **Mi899**：私のアプリを信頼し、自分でコンソール（コマンドライン）を操作したくない場合。
* **FPT** および **AFU** アプリケーション：以下の構文を参照してください。
* **外部フラッシュプログラマー**（BIOSがソフトウェア書き込みに対してロックされている場合）：
  [🇬🇧 CH341a – minimal usage guide how to read & write a motherboard BIOS](https://youtu.be/4qX2zihB6UE?si=vXVf4Fo430RfLJ05)

**補足事項：**
* バイナリBIOSファイルである限り、.rom や .bin といったファイル拡張子は重要ではありません。
* プログラマーを購入する際は、電圧切り替えスイッチがあるもの（例：CH341A Programmer V1.7 1.8V - 5.0V）を確認してください。

#### FPT の構文
現在のBIOSを保存/ダンプするには、以下を実行します：
`fptw64.exe -d bios-file-name.bin`

ファイルからBIOSを書き込むには、以下を実行します：
`fptw64.exe -f bios-file-name.bin`

※コンソール（CMD）を管理者として実行し、`bios-file-name.bin` を実際のファイル名に置き換えてください。

#### AFU の構文
現在のBIOSを保存/ダンプするには、以下を実行します：
**重要：** AFUはBIOSのフルサイズ（16MB）ではなく半分（8MB）しか保存しません。このようなダンプはAFUで書き込むことはできますが、FPTでは書き込めません。
`AFUWINx64.exe bios-file-name.bin /O`

ファイルからBIOSを書き込むには、以下を実行します：
`AFUWINx64.exe bios-file-name.bin /P /B /N /X`

#### TBU 後に UEFI システムが起動しない場合
これは LGA 2011-3 X99 プラットフォームの既知の UEFI バグです。修正は簡単です：
1. コンピュータをシャットダウンします。
2. 電源コードを抜きます。
3. すべてのストレージデバイス（HDD/SSD）を取り外します。
4. コンピュータの電源を入れます。
5. BIOSに入り、デフォルト設定を復元（Restore Defaults）します。
6. 変更を保存して再起動します。
7. 必要に応じてBIOSに入り、RAMタイミングなどの追加設定を適用します（任意）。
8. コンピュータの電源を切り、電源コードを抜きます。
9. すべてのストレージデバイスを再接続します。
10. コンピュータの電源を入れれば、正常に動作するはずです。
