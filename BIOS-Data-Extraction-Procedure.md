# BIOS Data Extraction Guide (Ubuntu/flashrom)
# BIOSデータ吸い出し手順書

## 1. Environment Setup / 環境構築
 必要なツールをインストールします

sudo apt update && sudo apt install -y flashrom


## 2. Identify the Chip / 接続確認
 チップが正しく認識されるか確認します

sudo flashrom -p ch341a_spi

 [判定基準]
 - チップ名（例: W25Q128.V）が表示されれば成功です
 - "No EEPROM/flash device found" と出る場合は、クリップを挟み直してください


## 3. Read and Verify / 吸い出しと検証
 データの化け（ノイズ）を防ぐため、2回吸い出してバイナリ比較を行います

 1回目の吸い出し
sudo flashrom -p ch341a_spi -r backup1.bin

# 2回目の吸い出し
 (一度クリップを付け直すと、接触不良による「偶然の一致」を防げます)
sudo flashrom -p ch341a_spi -r backup2.bin

# 2つのファイルをバイナリ比較
 何も出力されなければ、データは完全に一致しており成功です
diff backup1.bin backup2.bin


## 4. Specify Model (Optional) / 型番指定
 自動検出で候補が複数出た場合に、-c オプションで明示します

 例: W25Q128.V の場合
sudo flashrom -p ch341a_spi -c "W25Q128.V" -r bios_backup.bin


## 5. Quick Check / 簡易確認
 ファイルが空（すべてFFや00）でないか、ハッシュ値を記録します

 ヘッダー情報の表示
hexdump -C backup1.bin | head -n 20

# SHA256ハッシュ値の生成（記録用）

sha256sum backup1.bin
