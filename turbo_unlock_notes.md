### **【2026年最新版】Dell Precision T5810/T7810/T7910 ターボアンロック リサーチガイド**

#### **1. BIOS改造・マイクロコード削除済みROMの探索**
Haswell-EP（Xeon E5 v3）のターボアンロックを阻害するマイクロコードを特定・除去するための主要な情報源です。

* **[Win-Raid Forum (Level1Techs)](https://winraid.level1techs.com/t/guide-v3-xeon-turbo-unlock/32527)**
    * **重要度：高**。BIOS改造の「聖地」。Dell機特有の「BIOS Lock」解除手順や、チェックサムエラー回避方法が網羅されています。
* **[TechPowerUp BIOS Modding Section](https://www.techpowerup.com/forums/forums/bios-modding.19/)**
    * 実機での成功報告や、改造BIOS専門のスレッド（BIOS Modding）が活発です。

#### **2. v3turbo EFIドライバとアンロックツール（GitHub）**
オリジナルの「v3payne」が更新停止している場合でも、現在メンテナンス・配布されている信頼できるリポジトリです。

* **[v3_payne (Official Mirror)](https://github.com/v3payne/v3payne.github.io)**
    * 本家 `v3turbo.efi` の配布ページ。
* **[Ultimate-Xeon-Unlocker (GitHub)](https://github.com/K-S-V/Ultimate-Xeon-Unlocker)**
    * **推奨。** Haswell-EPのアンロックに必要なEFIドライバ一式と、自動化ツールを提供しているプロジェクト。
* **[UEFITool (LongSoft)](https://github.com/LongSoft/UEFITool/releases)**
    * マイクロコード（Ffsファイル）の削除や、EFIドライバの挿入に必須のツール。※編集には「0.28.0」などのリリース版を使用してください。

#### **3. 解説サイト・詳細手順**
* **[Miyconst (Xeon Turbo Boost Unlock Guide)](https://miyconst.github.io/cpu/2020/03/27/xeon-e5-v3-turbo-boost-unlock.html)**
    * **必読。** Dell Precisionシリーズでの成功例が最も詳しくまとめられているガイドサイトです。

---

### **【作業手順の概要】**

1.  **物理バックアップ**: CH341A等のプログラマで現在のROMを必ず吸い出す。
2.  **BIOS Lock解除**: `setup_var` を使い、Dell特有の書き込み保護（BIOS Lock）を `Disabled` に設定。
3.  **マイクロコード削除**: `UEFITool` で `CPU Microcode` パッチを特定して削除（これをしないとドライバが効きません）。
4.  **ドライバ導入**:
    * **永続化**: BIOS ROM内に `v3turbo.efi` を直接注入。
    * **安全策**: **[OpenCore](https://github.com/acidanthera/OpenCorePkg)** 等のブートローダー経由で、起動時に動的にドライバをロード。

---

### **⚠️ 警告・注意点**
- **電圧リスク**: T5810のROMは1.8V動作の場合があるため、CH341A使用時は必ず「1.8Vアダプタ（電圧変換器）」を使用してください。3.3Vを直結するとチップが破損します。
- **ブリックリスク**: 書き込み失敗時は起動不能になります。リカバリにはCH341AとバックアップROMが必須です。
- **熱対策**: 全コア最大クロック化により消費電力と発熱が増大します。冷却の強化と、`-50mV` 程度の電圧オフセット（アンダーボルト）設定を推奨します。

---
**自己責任で作業を行ってください。**
