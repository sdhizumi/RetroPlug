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
- 通常のVSTプラグイン同様にロードしてください
- UIをダブルクリックしてファイルブラウザーを開くか、直接ドラッグしてROMをロードしてください
- ROMを読み込む際に同じ名前の.savファイルが同階層にあれば自動的に読み込まれます
- 右クリックでオプションメニューを開くことができます
- オプションメニューからsaveをクリックすると、エミュレーターのステートがDAWのプロジェクトファイル内に保存され、設定内容を維持できます。 **.savファイルは自動で保存されません** もし.savファイルを保存したい場合は、オプションメニュー内から手動で保存してください。
- ボタンマッピングを変更したい場合は、`Settings -> Open Settings Folder...`を開き、`buttons.json`を編集してください。(使用可能なボタンリストは下記にて)
- mGBを使用する場合は、一般的なArduinoboyでの使用手順を参照してください: https://github.com/trash80/mGB - 追加の設定は特に必要なく、ノートをすぐに鳴らせます!
- LSDjを使用している場合、Settingsメニューに追加項目が表示され、ここからSyncモードを選択できます。(Arduinoboyエミュレーション)

## ボタンマッピング
キーボードとボタンのマッピングは、現在JSONフォーマットの設定ファイルでのみ設定可能です。 初回起動時に設定ファイルが `C:\Users\USERNAME\AppData\Roaming\RetroPlug` にデフォルトのボタンマッピングを含んだ状態で作成されます:

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
1つのウィンドウ内で複数のエミュレーターインスタンスを立ち上げる事ができ、バーチャル通信ケーブルでそれらを接続(リンク)する事ができます。 この機能の目的は複数のLSDjインスタンスでの作業を効率化することです。 追加のエミュレーターインスタンスを作成するには `Project -> Add Instance` サブメニューを使用してください。 以下のオプションから1つを選択してください:
- **Load ROM...** - ROMを新たに読み込んで新しいインスタンスを作成します。
- **Same ROM** - ROMを現在アクティブなインスタンスから読み込んで作成します。 作成されたインスタンスは.savファイルを読み込んで起動します。
- **Duplicate** - 現在アクティブなインスタンスのステートをコピーして作成します。

追加情報:
- 最大4つのインスタンスが作成可能です。
- インスタンス群は `Game Link` オプションを有効にするとリンクできます。 このオプションはリンクする全てのインスタンスで有効化してください。
- `Tab` キーでインスタンスを切り替えることができます。

### オーディオルーティング
デフォルトの状態では、全てのインスタンスの音声は1つのステレオにミックスされて出力されます。 `Project -> Audio Routing` メニューからオーディオルーティング設定を変更できます:

- **Stereo Mixdown** - デフォルトの設定です。 全てのインスタンスの音声は2chのステレオ信号にミックスされて出力されます。
- **Two Channels Per Instance** - それぞれのインスタンスが別々の2chとして出力されます。 インスタンス1はチャンネル1-2, インスタンス2はチャンネル3-4...といった形になります。 DAW側でも設定が必要になるかもしれません。 _この機能はスタンドアロン版では使用できません。_

### MIDIルーティング
デフォルトの状態では、それぞれのインスタンスは全てのMIDIチャンネルのメッセージを受信します。 `Project -> MIDI Routing` メニューからMIDIルーティング設定を変更できます:

- **All Channels to All Instances** - デフォルトの設定です。 全てのMIDIメッセージを全てのインスタンスに送ります。
    - 例えばMIDI Sync (Arduinoboy Variation)モードでLSDjを使用していて、複数のインスタンスを1つの `C-2` note onでスタートさせたい場合に便利です。
- **Four Channels Per Instance** - MIDIチャンネルを最大4つのインスタンスに均等にマッピングします。
    - インスタンス1はチャンネル1-4, インスタンス2はチャンネル5-8...といった形になります。
    - mGBでは、MIDIの16チャンネルを4つのmGBインスタンスに均等に振り分けて使用する事ができて便利です。
    - また、LSDjのMIDI Mapモードは1インスタンスにつきMIDIを2チャンネルを使用するので、こちらでも便利です。
- **One Channel Per Instance** - 上記モードに似ていますが、よりシンプルです。
    - インスタンス1はMIDIチャンネル1を受信し、インスタンス2はMIDIチャンネル2を受信...といった形になります。
    - MIDI Sync (Arduinoboy Variation)モードでLSDjを使用していて、複数のインスタンスを個別に制御したい場合には、これが分かりやすいモードです。

### マルチLSDjのセットアップ
バージョン v6.1.0 未満のLSDjの場合、インスタンスのどれか1つは `MASTER` モード、他のインスタンスは `SLAVE` モードに設定する必要があります。 バージョン v6.1.0 以降ではこれらのモードは無くなり、 `LSDJ` モードに統合されました。 このモードでは、いずれかのインスタンスから再生を開始すると、そのインスタンスが自動的にマスターになります。 この新しいモードは従来よりオーバーヘッドが大きいらしく、同期がわずかにずれることがあります。 ですので、もしLSDjの複数のインスタンスを完全に同期したい場合は、MIDI Syncモードのどちらかを使用する事をお勧めします。 これはインスタンス同士を同期させるのではなく、全てのインスタンスをDAWに同期させます。

## LSDjインテグレーション
LSDjが起動していることを検知すると、オプションメニューに追加項目が表示されます。
### Syncモード
- **Off**: DAWと一切の同期を行いません。 LSDj内で再生ボタンを押すと、他の動作に関係なく再生されます。

- **MIDI Sync**:
  * DAWが再生中の間、MIDIクロックを受け取ります。
  * LSDj内で再生ボタンを押しても、DAW側の再生ボタンを押さなければ再生されません。 LSDjはPROJECTスクリーンから "MIDI" モードに設定する必要があります。
  * このモードではLSDjはDAW上の再生位置を認識せず、MIDIクロックを受信した際に再生する事だけを行います。

- **MIDI Sync (Arduinoboy Variation)**:
  * MIDIクロックをDAWから受け取りますが、C-2ノートを受信するまで再生は開始しません。
  * その他のArduinoboyのオプションもエミュレートされており、全ての機能リストは[こちら](https://github.com/trash80/Arduinoboy/#mode-1---lsdj-as-midi-slave-sync)から確認できます。

- **MIDI Map**:
  * MIDIクロックをDAWから受け取り、MIDIノートの値に対応する行番号を再生します。 C-0であれば0行目、C#-0であれば1行目…といった形です。
  * チャンネル2から送信されたノートは128行目以降にマッピングされています。 チャンネル3以降のノートは無視されます。
  * 行の再生はnote offを受け取った際、またはDAW側の再生を停止した際に停止します。
  * Arduinoboy向けにビルドされたLSDjが必要です。

- **Auto Play**:
  * このオプションを有効にすると、RetroPlugはDAWの再生開始と停止が行われるたびに、LSDj側でスタートボタンを押す動作をシミュレートします。
  * 他のSyncモードと併用することが可能です。
  * このオプションはあまり賢くない(LSDjが再生中かどうかを認識できない)ので、手動で `Start` ボタンを押すと、再生／停止の判断が逆になってしまうことがあります。 その場合は `Start` をもう一度押してあげてください。

### .sav操作
liblsdjのお陰でLSDjのセーブデータ中のsongをリスト化、エクスポート、インポート、ロード、削除できるようになりました。 `LSDj Songs` メニューにこれらの機能が含まれています。 また、ウィンドウに `.lsdsng` ファイルをドラッグすると、 `.sav` にsongが追加されて即座に読み込まれます(`.sav` にsong分の空き容量が必要です)。

### Kitパッチング
`LSDj Kits` メニューから、Kitをパッチ、削除、エクスポートすることが可能です。
* `.kit` ファイルから、またはパッチ済みのLSDjのROMからKitをインポートすることが可能です。
* (ROMから)Kitを利用可能な次のスロットにインポートする場合は、 `LSDj Kits -> Import...` を使用します。 同名のKitが既にROMにある場合はインポートされません。 これにより、現在のROMに無い全てのKitをインポートすることができます。
* 特定のスロットにKitをパッチしたり、既にあるKitを置き換えたりする場合は、置き換えたいKitまたはスロットをメニュー上で選択し、`Load...` もしくは `Replace...` をそれぞれ選択してください。
* Kitを新しいスロットにインポートする際にはリセットが必要です(これは自動的に行われ、songデータは失われません)。 既にあるKitを置き換えた場合はリセットは必要なく、**再生中のKitをパッチする**ことがリアルタイムで行えます!
* Kit群はsaveを押した際にDAWのプロジェクトファイル内に保存されるほか、 プロジェクトをディスク上にセーブした際には `.retroplug` ファイル内に保存されます。

ディスク上から読み込んだROMには実際は変更が行われません。 パッチしたROMを実機または他のエミュレーターで使用する場合は、`System -> Save ROM...` オプションを使用してください。

### LSDjのアップデート
カスタムサンプルをパッチしてあるLSDjを新しいバージョンにアップデートする作業は非常に面倒ですが、RetroPlugはこれを楽にするため、パッチされたカスタムサンプルを維持したままROMを差し替える手段を提供しています。 これを行うには、 `System -> Replace ROM...` メニューから新しいLSDjのROMを選択してください。

### その他のオプション
- **Keyboard Shortcuts**: キーボードでLSDjをより直感的に扱うための一般的なショートカットを有効化します。 この機能はLSDjにボタンコンビネーションを送信しているため、常に完璧には動作しないかもしれません。 ショートカットのうち多くはボタン設定ファイルで変更可能です([ボタンマッピング](#ボタンマッピング)を参照してください)が、一部は変更できません:

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
    - 32bitビルド
    - Macビルド
    - 出力を別々のオーディオチャンネルへ
    - Luaスクリプティング
- v2.0.0
    - 追加のエミュレーター。 C64、GBA、メガドライブ/Genesisのサポートを検討中。

## 依存関係
- [SameBoy](https://github.com/LIJI32/SameBoy) - エミュレーター本体
- [iPlug2](https://github.com/iPlug2/iPlug2) - オーディオプラグインフレームワーク
- [libsdj](https://github.com/stijnfrishert/liblsdj) - LSDjセーブデータ操作関連の機能追加のため
- [rapidjson](https://github.com/Tencent/rapidjson) - 設定ファイルとセーブステートの取り扱いのためのJSONライブラリ

## ビルド手順
### Windows
RetroPlugはVisual Studio 2019で開発されていますが、SameBoyはWindows上でmsys2 (mingw)を使ってビルドする必要があります。 Visual Studioではmingwで生成されたライブラリを静的リンクすることができないため、ビルドプロセスから出力されたライブラリ(DLL)は最終的にビルドされるVSTのDLLファイルにリソースとしてコンパイルされ、ランタイムにメモリからロードされます。 これによりVSTを単一のDLLとして配布する事ができます。
- [msys2](https://www.msys2.org/)をデフォルトの位置にインストールする。
- [rgbds](https://github.com/rednex/rgbds) をコマンドラインからアクセスできる様にしておく。(PATH変数に追加するか、それ以外の何らかの方法を使う)
- `thirdparty/SameBoy/retroplug/build.bat` を実行。
- `RetroPlug.sln` をVisual Studio 2019で開いてビルドする。

### Mac
Coming soon!

## トラブルシューティング
GitHubのissueには使用方法に関する質問は書かずに、バグ報告や将来的な機能追加のリクエストのためだけに使用してください。 ヘルプが必要な場合は、The PSG Cabal の Discord内にある、このプラグインの公式サポートチャンネルまでお願いします: https://discord.gg/9MdBJST

### FAQ

**Q**: 助けて～! キーボードが動いてくれないよ ;(

**A**: 全てのホストプログラムはそれぞれ異なっていて、キーボード入力をVSTインストゥルメントへルーティングすることに制限があるものもあります。 まず最初に、ウィンドウの中心をクリックしてホストが正しいコントロール先を参照するように強制してみてください。これでも動作しない場合は、ホスト側の設定でプラグインへのキーボード入力が許可されているか確認してください (例えばRenoiseには "Enable keyboard" オプションがあります)。これでもなお動作しない場合は、`buttons.json` (初回起動時に `C:\Users\USERNAME\AppData\Roaming\RetroPlug` に作成されます) のデフォルトのキーボードマッピングを変更してみてください。DAWによっては色々と癖があります...
- **Reaper** はctrlキーをVSTに送信できないため、何か別のキーにマッピング変更する必要があります。

もし特定のDAWで問題を見つけた場合は、お気軽にバグレポートをお寄せください。

**Q**: なんてこった! LSDjがDAWの再生ボタンを押しても再生しないぞ :o

**A**: LSDj内とオプションメニューの両方で正しいSyncモードが設定されていることを確認してみてください!

## 寄付について
RetroPlugの開発をサポートして頂ける場合は、ぜひ寄付をよろしくお願いいたします。
[![Donate](https://www.paypalobjects.com/en_AU/i/btn/btn_donate_SM.gif)](https://www.paypal.com/cgi-bin/webscr?cmd=_donations&business=TJTBWD3P7S7PG&currency_code=AUD&source=url)

## ライセンス
MIT
