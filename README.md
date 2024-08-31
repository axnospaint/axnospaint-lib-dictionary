# axnospaint-lib-dictionary
AXNOS Paint用辞書ファイル（無保証のサンプルデータ）

## 概要
[AXNOS Paint](https://github.com/axnospaint/axnospaint-lib)で使用するユーザー辞書ファイルのサンプルです。  

| ファイル | 内容 |
| ---- | ---- |
| mydic.json | 辞書ファイルのテンプレート |
| en.json | 英語UIのサンプル |

中身はJSON形式のファイルになっており、置換対象となる単語のそれぞれに固有のキーが割り当てられています。

以下の例は「補助ツールのセーブボタン」の定義です。"セーブ"の部分を別の単語に書き換えることで、画面上の表示を変更することができます。
```json
"@MISC.BUTTON_SAVE": "セーブ",
```
AXNOS Paint本体と同じフォルダに書き換えた辞書ファイルを配置し、起動オプションの`dictionary`にファイルパスを指定すると、起動時に辞書ファイルを読み込み、単語の変換が行われます。


```js
new AXNOSPaint({
    // 中略
    dictionary: 'mydic.json',
});
```

## 仕様

* 辞書に記述がない単語は変換されません。（全ての単語が記述されている必要はなく、変更が必要な部分だけ切り取った辞書でも動作します）
* 指定された辞書が読み込めない場合、また、辞書の内容に異常がある場合は単語の変換を行いません。
* AXNOS Paintのバージョン更新により、変換されない単語が追加されたり、変換に問題が発生する可能性があります。
* 公開している辞書ファイルはサンプルであり、内容について公式サポートしているものではありません。辞書を使用することで発生する不具合に関しては原則として対応を控えさせていただきます。


## 使用例

### 投稿ボタンのラベルを変更したい

`mydic.json`の以下の部分を書き換えます。
```json
    "@POST.BUTTON_SUBMIT": "お絵カキコする！",
    "@POST.BUTTON_POSTING": "投稿中です……",
```

### 投稿画面の注意事項を変更したい

`mydic.json`の以下の部分を書き換えます。
```json
    "@POST.NOTICE": "投稿前の注意事項",
    "@POST.NOTICE1": "投稿時にすべてのレイヤーが統合されます。",
    "@POST.NOTICE2": "",
    "@POST.NOTICE3": "",
    "@POST.NOTICE4": ""
```
最大４行まで文章を追加できます。""(空文字)の行は表示されません。

### 英語UIに対応したい

AXNOS Paintの起動前に、`Navigator.languages`を参照して対応する辞書ファイルを選択し、起動オプションで動的に指定することで、ユーザーの推奨言語に沿った単語変換を行わせることができます。
（例）
```js
    const languages = navigator.languages || [navigator.language || navigator.userLanguage];
    // 最上位の言語を取得
    const primaryLanguage = languages[0];
    // 言語情報の変換
    let dictonaryFile = null;
    if (primaryLanguage.startsWith('en')) {
        // 先頭がenから始まる言語は、en共通
        dictonaryFile = 'en.json';
    } 
    new AXNOSPaint({
        // 中略
        dictionary: dictonaryFile,
    });
```