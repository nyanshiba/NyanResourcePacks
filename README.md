# NyanResourcePacks

<!-- 190707 -->

<!-- TOC -->

- [NyanResourcePacks](#nyanresourcepacks)
    - [Installation](#installation)
    - [NyanSilentPack](#nyansilentpack)
        - [下げたレベルの一覧](#下げたレベルの一覧)
        - [スクリプト](#スクリプト)
    - [NyanTimelapsePack](#nyantimelapsepack)
        - [スクリプト](#スクリプト-1)
    - [NyanGrayscaleUIPack](#nyangrayscaleuipack)
    - [NyanRedstonePack](#nyanredstonepack)
    - [NyanDiggingPack](#nyandiggingpack)

<!-- /TOC -->

## Installation

これらのリソースパックはMinecraft Java Edition 1.14以降向けに作られています。  
リソースパックのバージョンは`4`なので、1.13とも一定の互換性はあります。  

1. Clone or download -> Download ZIPを選択。  

2. ダウンロードしたzipファイルを解凍。  

3. `%AppData%\.minecraft`またはゲームディレクトリ内の`resourcepacks`内に、  
解凍したリソースパック(NyanSilentPackやNyanRedstonePack等、NyanResourcePacks毎入れても動作しません！)を置きます。  

4. Minecraft内でリソースパックを選択してください。  

## NyanSilentPack

エンダーマンの音とか、トロッコ(友好的生物🤔)とか、爆発音とかうるさいので小さくしました。  
静音リソパは他にも幾らかあると思いますが、適切なレベルでなかったり音声が大きく劣化(不適切なエンコード)していたりするので、少し気をつけて処理しました。  

### 下げたレベルの一覧  
これ要らないって時の参考までに。  
```
NyanSilentPack\assets\minecraft\sounds
├─item
│  └─trident -6dB
├─minecart -6dB
├─mob
│  ├─blaze -6dB
│  ├─endermen -18dB
│  ├─guardian -6dB
│  ├─magmacube -6dB
│  └─wither -12dB
├─portal -6dB
├─random -6dB
├─records cat <- よく余るレコードは何かに使えそう
└─tile
    └─piston -6dB
```

### スクリプト

出来る限り音質劣化を避けたかったので、元よりファイルサイズが大きくなっているよ。  
サンプルレートは44.1kHzっぽいので合わせた。  

```powershell
(Get-ChildItem *.ogg).BaseName | ForEach-Object {
    ffmpeg -i "$_.ogg" -af volume=-6dB -c:a libvorbis -q:a 10 -ar 44100 "${_}_2.ogg"
    Remove-Item "$_.ogg"
    Rename-Item "${_}_2.ogg" "$_.ogg"
}
```

## NyanTimelapsePack

サバイバルモードで定点からタイムラプスを撮る時に邪魔になりうるもの全てを透過したリソパ。  

```
NyanTimelapsePack\assets\minecraft\textures
├─block ガラスで囲っても映らないように
├─effect 知らん
├─font F1押してもチャット欄付近でMCID・UUIDが誤って表示されたことがあったので
├─gui 上に同じ
├─misc かぼちゃ使えたり、囲いの上にアイテムが落ちても影が出来ないように、周囲が暗くならないように等
├─mob_effect 精神衛生上
└─particle 撮影の邪魔なので
```

プレイヤーを保護するブロックを透過するだけではダメな例。  

![2019-07-01_13 17 47](https://user-images.githubusercontent.com/31783332/60410322-bb6f1e00-9c02-11e9-8f88-cfbe5234d34d.png)

撮影時はF1キーを押す必要があるが、押さなくてもこれだけ透過される。  

![2019-07-01_13 18 06](https://user-images.githubusercontent.com/31783332/60410323-bb6f1e00-9c02-11e9-95ac-7f97bca27841.png)

### スクリプト

```powershell
#すべてのpngファイルを再帰的に取得
Get-ChildItem *.png -Recurse | ForEach-Object {
    #1行で解像度を保持しつつ透過だけするImageMagick引数
    magick convert "$_" -fill none -fuzz 0% +opaque none "$_"
}
```

## NyanGrayscaleUIPack

↑のImageMagick引数を試行錯誤した際の副産物。  
何だかんだ視覚的に静かで落ち着くので愛用してたり。  

## NyanRedstonePack

オブザーバー、粘着ピストン、ピストン等をどの角度から見ても信号の有無が分かるようにした。  
既出だけど、自分好みのデザインにしたかった。  

[Vanilla Tweaks](https://vanillatweaks.net/picker/resource-packs/) と [ぽぽおいう](https://twitter.com/kt_popooiu)さんの[[1.14]Glass++++](https://www.dropbox.com/sh/fm57am9tke0q1bs/AACgl6MUwbnETU6QiQD60Ew7a?preview=%5B1.14%5DGlass%2B%2B%2B%2B.zip) を参考にさせて頂きました。  

上からでもホッパーの向きがわかる  

![Hopper](https://user-images.githubusercontent.com/31783332/60409774-ee63e280-9bff-11e9-9a60-f62d7ded54ee.png)

オブザーバー、粘着ピストン、ピストンはどの方向からでも状態が分かる  

![Observer and Sticky Piston](https://user-images.githubusercontent.com/31783332/60409775-ee63e280-9bff-11e9-9db4-6ab380c8ed13.png)
![Observer and Sticky Piston](https://user-images.githubusercontent.com/31783332/60409776-ee63e280-9bff-11e9-8842-9ec6209e5d00.png)

このリソパはほんと難しかった。ディレクトリ名の誤字に気付くのが...。  

## NyanDiggingPack

整地の時、ブロック破壊時のパーティクルが極めて視覚的・映像的に邪魔だったので、消しました。  
US版Google等でresourcepack particles block break...色々調べましたが解決方法は見当たらず。  
しかし、[NyanRedstonePack](#nyanredstonepack)に使った"ambientocclusion"プロパティについて調べていたら気になることが。  

https://minecraft-ja.gamepedia.com/%E3%83%A2%E3%83%87%E3%83%AB

どうやら、ブロック破壊時のパーティクルの参照先は指定できるようで、適当に本来は存在しない透明な`transparent.png`でも指定してあげれば透過できるんじゃないかと思ーーできました。  

あとは一括処理ですね。  
(1.14.2.jarから抽出するなどした)`\models\block`内のjsonをコピーして以下を叩けば`"particle": "block/transparent"`を追加or置き換えしてくれます。  

```powershell
#カレントディレクトリ
Set-Location ".\resourcepacks\NyanDiggingPack\assets\minecraft\models\block"

#全てのjsonファイル
Get-ChildItem *.json | ForEach-Object {
    $_.Name
    [PSCustomObject]$Model = Get-Content $_.Name | ConvertFrom-Json -Depth 100
    $Model."textures" | Add-Member -MemberType NoteProperty -Name "particle" -Value "block/transparent" -Force
    $Model | ConvertTo-Json -Depth 100 | Out-File $_.Name -Encoding UTF8
}
```

https://www.youtube.com/embed/jABanUb0tm4
![https://www.youtube.com/embed/jABanUb0tm4](https://i.ytimg.com/vi/jABanUb0tm4/maxresdefault.jpg)