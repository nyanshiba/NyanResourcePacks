# NyanResourcePacks

<!-- 190707 -->

<!-- TOC -->

- [NyanResourcePacks](#nyanresourcepacks)
    - [Installation](#installation)
    - [NyanSilentPack](#nyansilentpack)
        - [音量を下げた音声ファイルの一覧](#音量を下げた音声ファイルの一覧)
        - [スクリプト](#スクリプト)
    - [NyanTimelapsePack](#nyantimelapsepack)
        - [スクリプト](#スクリプト-1)
    - [NyanGrayscaleUIPack](#nyangrayscaleuipack)
    - [NyanRedstonePack](#nyanredstonepack)
    - [NyanDiggingPack](#nyandiggingpack)
        - [スクリプト](#スクリプト-2)
    - [NyanFontPack](#nyanfontpack)

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

### 音量を下げた音声ファイルの一覧
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

<table border="0">
    <tr>
        <td><img src="https://user-images.githubusercontent.com/31783332/60409775-ee63e280-9bff-11e9-9db4-6ab380c8ed13.png" alt="Observer and Sticky Piston"/></td>
        <td><img src="https://user-images.githubusercontent.com/31783332/60409776-ee63e280-9bff-11e9-8842-9ec6209e5d00.png" alt="Observer and Sticky Piston"/></td>
    </tr>
</table>

ピストン等を`"ambientocclusion": false`してfpsが気持ち向上する  

![NyanRedstonePack](https://user-images.githubusercontent.com/31783332/62188290-dcb05f00-b3a6-11e9-952c-5c0dc929cec0.png)

このリソパはほんと難しかった。ディレクトリ名の誤字に気付くのが...。  

## NyanDiggingPack

整地の時、ブロック破壊時のパーティクルが極めて視覚的・映像的に邪魔だったので消しました。  
ブロックパーティクルの参照先に透過されたPNGを指定しているので、見えないパーティクルが出ている状態です。  
[モデル - Minecraft Wiki](https://minecraft-ja.gamepedia.com/%E3%83%A2%E3%83%87%E3%83%AB)

https://www.youtube.com/embed/jABanUb0tm4
![https://www.youtube.com/embed/jABanUb0tm4](https://i.ytimg.com/vi/jABanUb0tm4/maxresdefault.jpg)

### スクリプト

下記の手順で一括処理しました。  
`%APPDATA%\.minecraft\versions\バージョン\バージョン.jar`からJDK同梱のjar.exe等で抽出した`\assets\minecraft\models\block`内で下記のPowerShellスクリプトを実行すると、`"particle": "block/transparent"`を追加or置き換えしてくれます。  

```powershell
# block particleが透過されたmodelの出力先
$OutputDir = "$env:USERPROFILE\Desktop\hoge"

# 渡されたmodelのblock particleの透過とファイル出力を行う関数
function Out-TransparentBlockParticleModel
{
    [CmdletBinding()]
    param (
        [Parameter()]
        [string]
        $Path
    )

    "transparent: $Path"

    # jsonからmodelを取得
    $Model = Get-Content $Path | ConvertFrom-Json -Depth 100

    # texturesクラスに"particle": "block/transparent"を追加
    $Model.textures | Add-Member -MemberType NoteProperty -Name "particle" -Value "block/transparent" -Force

    # 改変されたmodelをjson出力
    $Model | ConvertTo-Json -Depth 100 | Out-File "$OutputDir\$Path" -Encoding UTF8
}

# カレントディレクトリ下の全てのjsonファイルを取得してそれぞれ実行
# Set-Location 1.16-rc1\assets\minecraft\models\block
foreach ($json in (Get-ChildItem *.json))
{
    # jsonからmodelを取得、modelが参照するparentのみ格納
    $ParentModel = (Get-Content $json.Name | ConvertFrom-Json -Depth 100).parent

    if ($ParentModel -match "minecraft:" -And $ParentModel -ne "minecraft:block/cube")
    {
        # parent modelを参照しているmodelは、parent model側でblock particleを透過すれば良いので、parent modelのパスだけ記録しここでは何もしない
        "ignore:$($json.Name), parent: $ParentModel"
        [string[]]$Parents += $ParentModel
    }
    else
    {
        # parent modelを参照していない独立したmodelか、接頭辞minecraft:がないparent modelを参照している特殊なブロックは、model毎にblock particleを透過させる必要がある
        Out-TransparentBlockParticleModel -Path $json.Name
    }
}

# parent modelも同様に処理する
foreach ($parentModelJsonPath in ($Parents | Sort-Object | Get-Unique))
{
    Out-TransparentBlockParticleModel -Path "$($parentModelJsonPath.Replace('minecraft:block/','')).json"
}

# rsyncでリポジトリに反映させる
# $WslOutputDir = [Regex]::Replace($OutputDir, "^([A-Z]):(\\.*)?", { "/mnt/" + $args.Groups[1].Value.ToLower() + $args.Groups[2].Value.Replace('\','/')})
# Start-Process -FilePath wsl -ArgumentList "rsync -av --delete --checksum $WslOutputDir/ /mnt/c/Minecraft/NyanResourcePacks/NyanDiggingPack/assets/minecraft/models/block/" -Wait -NoNewWindow
```

このほか、`water.json` `lava.json`はブロックパーティクルを透過すると透明になってしまうので削除し、  
1.15で追加された`beehive.json` `beehive_honey.json` `bee_nest.json` `bee_nest_honey.json`は同様のブロックと異なった挙動を示すため、手動で書き換えています（workaround）。

## NyanFontPack

![NyanFontPack](https://user-images.githubusercontent.com/31783332/82983764-af1a5d80-a02b-11ea-96d4-791a892d27d5.jpg)

通常、Minecraftにカスタムフォントを適用するためには画像形式に変換する必要がありましたが、下記を参考にTrueTypeフォントを使えるようにしました。  
[How to add a TTF font to your resource pack (1.13-pre7 and 1.13-pre8) : Minecraft](https://www.reddit.com/r/Minecraft/comments/8yjroi/how_to_add_a_ttf_font_to_your_resource_pack/)

OpenTypeフォントには非対応なので、[FontForge](https://fontforge.org/en-US/)を使用してTrueTypeフォントに変換しています。

NyanFontPackでは、Minecraft内できれいに表示できたものとして、[Noto Sans JP](https://fonts.google.com/specimen/Noto+Sans+JP)と[Noto Serif JP](https://fonts.google.com/specimen/Noto+Serif+JP) を使用しています。  
All Noto fonts are published under the SIL Open Font License, Version 1.1.


文字の品質が悪い場合、以下を試してみて下さい。
- Minecraftの解像度を高くする(フルスクリーンなど)
- 描画品質を改善する(看板にはOptiFineのアンチエイリアス、UIにはFXAAが効きます)
