Dell Precision T5810 向け Turbo Boost Unlock（TBU）プロジェクト。Haswell-EP Xeon v3 のマイクロコード制限を回避し、全コアをシングルコア最大倍率で動作させるための EFI ドライバ導入と BIOS 改変の検証を行う。T5810A32 BIOS の解析、microcode 0x06F2 の削除、v3turbo / payne loader の動作確認、電流制限の調査などを含む研究用リポジトリ。

# T5810 Turbo Boost Unlock Project

本リポジトリは Dell Precision T5810 における Haswell-EP Xeon v3 の Turbo Boost Unlock を研究するためのものです。マイクロコード制限を回避し、EFI ドライバを利用して全コアを最大倍率で動作させる手法を記録します。

[Turbo Unlock Notes](turbo_unlock_notes.md)

## 概要

- T5810A32 BIOS のバックアップと解析  
- microcode 0x06F2 の削除  
- v3turbo / v3_payne EFI ドライバの導入  
- Turbo Boost Unlock の動作検証  
- Dell 特有の電流・電圧制限の調査  

## 注意事項

本プロジェクトは教育・研究目的です。BIOS 書き換えには高いリスクがあり、実行は自己責任となります。
