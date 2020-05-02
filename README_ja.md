# RetroPlug
GameBoyエミュレーターSameBoyのVSTラッパー。Arduinoboyをサポートしています。

## 機能
- [SameBoy](https://github.com/LIJI32/SameBoy) v0.12.1ベース
- [mGB](https://github.com/trash80/mGB)のMIDI制御をすべてサポート
- [LSDj](https://www.littlesounddj.com)をDAWと同期して使用可能
- [Arduinoboy](https://github.com/trash80/Arduinoboy)の様々なモードをエミュレーション
- LSDjへのリアルタイムサンプルパッチングが可能!

## 現在の制限事項 (変更の可能性あり)
- VST2のみ
- Windowsのみ
- 64bit版のみ

## ダウンロード
[releases](https://github.com/tommitytom/RetroPlug/releases)ページから最新版をダウンロードしてください。

## 使用方法
- 通常のVSTプラグインとしてロードしてください
- UIをダブルクリックしてファイルブラウザーを開くか、直接ドラッグしてROMをロードしてください
- ROMを読み込む際に同じ名前の.savファイルが同階層にあれば読み込みます
- 右クリックでオプションメニューを開くことができます
- エミュレーターのステートはオプションメニューからsaveをクリックするとプロジェクトファイルとして保存でき、設定内容を保存できます。 **.savファイルは自動で保存されません** もし.savファイルを保存したい場合は、オプションメニュー内から手動で保存してください。
- ボタンマップを変更したい場合は、`Settings -> Open Settings Folder...`を開き、`buttons.json`を編集してください。(使用可能なボタンリストは下部にあります)
- mGBを使用する場合は、一般的なArduinoboyでの使用手順を参照してください: https://github.com/trash80/mGB - 追加の設定は特に必要なく、ノートをすぐに鳴らせます!
- LSDjを使用する場合は、Settingsメニューから追加の設定をする必要があります。Syncモードの使用を有効にしてください。(Arduinoboyエミュレーション)

## ボタンマッピング
キーボードボタンのマッピングは現在JSON設定ファイルでのみ設定可能です。 初回起動時に設定ファイルが `C:\Users\USERNAME\AppData\Roaming\RetroPlug` にデフォルトのボタンマッピングを含んだ状態で作成されます:

|ボタン|デフォルトキー|
|------|-----------|
|A|Z|
|B|X|
|Up|UpArrow|
|Down|DownArrow|
|Left|LeftArrow|
|Right|RightArrow|
|Select|Ctrl|
|Start|Enter|

### buttons.jsonでサポートされているキー:
`0 - 9` および `A - Z`はそのまま設定ファイルに書き込むとそれぞれのキーとして使用でき、これら以外に使用できるキーは以下の通りです:

```
Backspace, Tab, Clear, Enter, Shift, Ctrl, Alt, Pause, Caps, Esc, Space, PageUp, PageDown, End, Home, LeftArrow, UpArrow, RightArrow, DownArrow, Select, Print, Execute, PrintScreen, Insert, Delete, Help, LeftWin, RightWin, Sleep, NumPad0, NumPad1, NumPad2, NumPad3, NumPad4, NumPad5, NumPad6, NumPad7, NumPad8, NumPad9, Multiply, Add, Separator, Subtract, Decimal, Divide, F1, F2, F3, F4, F5, F6, F7, F8, F9, F10, F11, F12, F13, F14, F15, F16, F17, F18, F19, F20, F21, F22, F23, F24, NumLock, Scroll
```
全てのキー設定名で大文字小文字を区別するので注意してください!

## 複数インスタンス
一つのウィンドウ内で複数のエミュレーターインスタンスを立ち上げる事ができ、バーチャル通信ケーブルでそれらを接続(リンク)する事ができます。 この機能の目的はLSDjを複数インスタンスでの作業を効率化させることです。 追加のエミュレーターインスタンスを作成するには `Project -> Add Instance` サブメニューを使用してください。 以下のオプションから一つを選択してください:
- **Load ROM...** - ROMを新たに読み込んで新しいインスタンスを作成します。
- **Same ROM** - ROMを現在アクティブなインスタンスから読み込んで作成します。 作成されたインスタンスは.savファイルを読み込んで起動します。
- **Duplicate** - 現在アクティブなインスタンスのステートをコピーして作成します。

追加情報:
- 最大4つのインスタンスが作成可能です。
- インスタンス群は `Game Link` オプションを有効にするとリンクできます。 このオプションはリンクする全てのインスタンスで有効可してください。
- インスタンスを切り替える際は `Tab` キーを使用します。

### オーディオルーティング
デフォルトの状態では、全てのインスタンスの音声は一つのステレオにミックスされて出力されます。 `Project -> Audio Routing` メニューに追加のオーディオルーティング設定があります:

- **Stereo Mixdown** - デフォルトの設定です。全てのインスタンスの音声は2chのステレオ信号にミックスされて出力されます。
- **Two Channels Per Instance** - それぞれのインスタンスが別々の2chとして出力されます。 インスタンス1はチャンネル1-2, インスタンス2はチャンネル3-4...といった形になります。 恐らくDAW上での設定が必要です。 _この機能はスタンドアロン版では使用できません。_

### MIDIルーティング
デフォルトの状態では、それぞれのインスタンスは全てのMIDIチャンネルのメッセージを受信します。 `Project -> MIDI Routing` メニューに追加のMIDIルーティング設定があります:

- **All Channels to All Instances** - デフォルトの設定です。 全てのMIDIメッセージを全てのインスタンスに送ります。
    - 例えばLSDjをArduinoboy MIDI sync modeで使用している際、複数のインスタンスを一つの `C-2` note onで鳴らしたい状況で便利です。
- **Four Channels Per Instance** - MIDIチャンネルを最大4つのインスタンスに均等にマッピングします。
    - インスタンス1はチャンネル1-4, インスタンス2はチャンネル5-8...といった形になります。
    - mGBでは、MIDIの16チャンネルを4つのmGBインスタンスに均等に振り分けて使用する事ができて便利です。
    - また、LSDjのMIDIマップモードでは、1インスタンスにつきMIDIを2チャンネルを使用するようになり、こらでも便利です。
- **One Channel Per Instance** - 上記モードに似ていますが、よりシンプルです。
    - インスタンス1はMIDIチャンネル1を受信し、インスタンス2はMIDIチャンネル2を受信...といった形になります。
    - LSDjでArduinoboyのMIDI sync modeを使用している際に、個々のインスタンスを制御したい場合に分かりやすいモードです。

### マルチLSDjのセットアップ
バージョン v6.1.0 までのLSDjの場合、インスタンス1は `MASTER` にセットして、それ以外は `SLAVE` に設定する必要があります。 バージョン v6.1.0 以降ではこの機能は無くなりましたが、代わりに特殊な `LSDJ` モードがあり、操作中のインスタンスから再生を開始した際に、そのインスタンスが自動的にマスターになります。 この新しいモードには同期が少し取れないような新たなオーバーヘッドがあるかもしれません - もしLSDjの複数のインスタンスを完全に同期したい場合は、MIDI Syncモードのうちの一つを使用する事をお勧めします。 これはインスタンス同士を同期させるのではなく、複数のインスタンスをDAWに同期させます。

## LSDj統合機能
LSDjが起動していることを検知すると、追加のオプションを使用する事ができるようになります。
### Syncモード
- **Off**: DAWと一切の同期を行いません。 LSDj内で再生ボタンを押すと、他の動作に関係なく再生されます。

- **MIDI Sync**:
  * DAWが再生を開始した際にMIDIクロックを受け取ります。
  * LSDj内で再生ボタンを押しても、DAW側の再生ボタンを押さなければ再生されません。LSDjはプロジェクトページから "MIDI" モードに設定する必要があります。
  * このモードではLSDjはDAW上の再生位置を認識せず、MIDIクロックを受信した際に再生する事だけを行います。

- **MIDI Sync (Arduinoboy Variation)**:
  * MIDIクロックをDAWから受け取りますが、C-2ノートを受信したときのみ再生します。
  * Arduinoboyの追加オプションもエミュレートされており、全ての機能リストは[こちら](https://github.com/trash80/Arduinoboy/#mode-1---lsdj-as-midi-slave-sync)から確認できます。

- **MIDI Map**:
  * MIDIクロックをDAWから受け取り、MIDIノートの値に対して相対的に行番号を再生します。 C-0であれば0行目、C#-0であれば1行目…といった形です。
  * チャンネル2から送信されたノートは128行以上でマッピングされます。 それ以外のチャンネルでは無視されます。
  * 行の再生はノートオフを受け取った際、またはDAW側の再生を停止した際に停止します。
  * Arduinoboy向けにビルドされたLSDjが必要です。

- **Auto Play**:
  * このオプションを有効にした際は、RetroPlugはDAWの再生開始と停止が行われた際に、LSDj側でスタートボタンを押す動作をシミュレートします。
  * これ以外のSyncモードと連携することが可能です。
  * このオプションは非常に間抜け(dumb)で(LSDjが再生中かどうかを判断しない)、手動で `Start` ボタンを押していた際に間違った状態になる事があります。 その場合は `Start` をもう一度押してあげてください。

### .sav操作
liblsdjのお陰でLSDjのセーブデータ中のsongをリスト化、エクスポート、インポート、ロード、削除できるようになりました。 `LSDj Songs` メニューにこれらの機能が含まれています。 追加機能として、ウィンドウに `.lsdsng` ファイルをドラッグすると、 `.sav` に楽曲が追加されて即座に読み込まれます(`.sav` に楽曲分の空き容量が必要です)。

### Kitパッチング
`LSDj Kits` メニューから、Kitをパッチ、削除、エクスポートすることが可能です。
* `.kit` ファイルから、またはパッチ済みのLSDjのROMからKitをインポートすることが可能です。
* (ROMから)Kitを利用可能な次のスロットにインポートする場合は、 `LSDj Kits -> Import...` を使用します。 同名のKitが既にROMにある場合はインポートされません。 これにより、現在のROMに無い全てのKitをインポートすることができます。
* 特定のスロットにKitをパッチしたり、既にあるKitを置き換えたりする場合は、置き換えたいkit/slotをメニュー上で選択し、`Load...` もしくは `Replace...` をそれぞれ選択してください。
* Kitを新しいスロットにインポートする際にはリセットが必要です(これは自動的に行われ、楽曲データは失われません)。 既にあるKitを置き換えた場合はリセットは必要なく、**再生中にKitをパッチングする**ことがリアルタイムで行えます!
* Kit群はsaveを押した際にDAWのプロジェクトファイルに、または `.retroplug` ファイルとしてプロジェクトファイルをディスク上にセーブした際に保存されます。

ディスク上から読み込んだROMは実際には変更されていません。 パッチしたROMを実機または他のエミュレーターで使用する場合は、`System -> Save ROM...` オプションを使用してください。

### LSDjのアップデート
カスタムサンプルをパッチしてあるLSDjを新しいバージョンにアップデートする作業は非常に面倒ですが、RetroPlugはパッチされたカスタムサンプルを維持したままROMを差し替える簡単な手段を提供しています。 これを行うには、 `System -> Replace ROM...` メニューから新しいLSDjのROMを選択してください。

### 追加のオプション
- **Keyboard Shortcuts**: キーボードでLSDjをより直感的に扱うための一般的なショートカットを有効化します。 この機能はLSDjにボタンコンビネーションを送信しているため、常に完璧には動作しないかもしれません。 多くのホットキーはボタン設定ファイルで変更可能です([ボタンマッピング](#ボタンマッピング)を参照してください)が、一部のキーは変更できません:

| 操作 | デフォルトキー | 変更の可不可|
|--------|----------------|-------------|
|ScreenUp|W|可|
|ScreenLeft|A|可|
|ScreenDown|S|可|
|ScreenRight|D|可|
|DownTenRows|PageDown|可|
|UpTenRows|PageUp|可|
|CancelSelection|Esc|可|
|Delete|Delete|可|
|StartSelection|Shift (Hold)|不可|
|CopySelection|Ctrl + C|不可|
|CutSelection|Ctrl + X|不可|
|PasteSelection|Ctrl + V|不可|

## ロードマップ
- v1.0.0
- v1.0.0
    - 32bitビルド
    - Macビルド
    - 出力を別々のオーディオチャンネルへ
    - Luaスクリプティング
- v2.0.0
    - 追加のエミュレーター。 C64、GBA、メガドライブ/Genesisのサポートを検討中。

## 依存関係
- [SameBoy](https://github.com/LIJI32/SameBoy) - エミュレーター部分
- [iPlug2](https://github.com/iPlug2/iPlug2) - オーディオプラグインフレームワーク
- [libsdj](https://github.com/stijnfrishert/liblsdj) - LSDjセーブデータ操作関連の機能追加のため
- [rapidjson](https://github.com/Tencent/rapidjson) - 設定ファイルとセーブステートの取り扱いのためのJSONライブラリー

## ビルド
### Windows
RetroPlugはVisual Studio 2019で開発されていますが、SameboyはWindows上でmsys2 (mingw)を使ってビルドする必要があります。 Visual Studioではmingwで生成されたライブラリを静的リンクすることができないため、ビルドプロセスから出力されたライブラリ(DLL)は最終的にビルドされるVSTのDLLファイルにリソースとしてコンパイルされ、実行時にはメモリ上にランタイムとしてロードされます。 これによりVSTを単一のDLLとして配布する事ができます。
- [msys2](https://www.msys2.org/)をデフォルトの位置にインストールする。
- [rgbds](https://github.com/rednex/rgbds) をコマンドラインからアクセスできる場所に配置する。(PATH変数に追加するか、それ以外の何らかの方法を使う)
- `thirdparty/SameBoy/retroplug/build.bat` を実行。
- `RetroPlug.sln` をVisual Studio 2019で開いてビルドする。

### Mac
Coming soon!

## トラブルシューティング
GitHubのissueには使用方法に関する質問を書かずに、純粋にバグ報告や将来的な機能追加のリクエストの為に使用してください。 ヘルプが必要な場合は、the PSG Cabal discord内のこのプラグインの公式サポートチャンネルにお願いします: https://discord.gg/9MdBJST

### FAQ

**Q**: 助けて～! キーボードが動いてくれないよ ;(

**A**: 全てのホストプログラムはそれぞれ異なっていて、VSTインストゥルメントの為のキーボード入力ルーティングに制限があるものもあります。 まず最初に、ウィンドウの中心をクリックしてホストが正しいコントロール先を参照するように強制してみてください。これでも動作しない場合は、ホスト側でプラグインへのキーボード入力を許可するよう設定してください (例えばRenoiseは "Enable keybord" オプションがあります)。これでもなお動作しない場合は、`buttons.json` (初回起動時に `C:\Users\USERNAME\AppData\Roaming\RetroPlug` に作成されます) のデフォルトのキーボードマッピングを変更してみてください。特定のDAWでは色々と癖があります...
- **Reaper** はctrlキーをVSTに送信できないため、何か別のキーに再マッピングする必要があります。

もし特定のDAWで問題を見つけた場合は、お気軽にバグレポートをお寄せください。

**Q**: なんてこった! LSDjがDAWの再生ボタンを押しても再生しないぞ :o

**A**: LSDj内とオプションメニューの両方で正しいsyncモードが設定されていることを確認してみてください!

## 寄付について
RetroPlugの開発についてサポートして頂ける場合は、ぜひ寄付をよろしくお願いいたします。
[![Donate](https://www.paypalobjects.com/en_AU/i/btn/btn_donate_SM.gif)](https://www.paypal.com/cgi-bin/webscr?cmd=_donations&business=TJTBWD3P7S7PG&currency_code=AUD&source=url)

## ライセンス
MIT