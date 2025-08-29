# HmTimerMessageBox

![HmTimerMessageBox v1.0.0](https://img.shields.io/badge/HmTimerMessageBox-v1.0.0-6479ff.svg)
[![BSD 3-Clause](https://img.shields.io/badge/license-BSD_3_Clause-blue.svg?style=flat)](LICENSE)
![Hidemaru 7.00](https://img.shields.io/badge/Hidemaru-v7.00-6479ff.svg)

この自動終了式のメッセージボックスは、ボタンを押さなかったとしても、指定した時間で終了する、そんな特殊なメッセージボックスです。汎用で利用可能です。

# 使い方

`HmTimerMessageBox.exe [本文] [タイトル] [Flags] [表示ミリ秒]`

## 引数

- **`本文`** (必須)
  - メッセージボックスに表示するテキスト。

- **`タイトル`** (任意)
  - メッセージボックスのタイトルバーに表示するテキスト。
  - **デフォルト**: `""` (なし)

- **`Flags`** (任意)
  - メッセージボックスのボタンの種類やアイコンを指定するための数値。
  - [Microsoftの`MessageBox`関数のドキュメント](https://learn.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-messagebox)に記載されている `uType` の値を組み合わせて指定します。
  - **デフォルト**: `0` (`MB_OK`)
  - **よく使われる値の例:**
    - **ボタンの種類:**
      - `0` (MB_OK)
      - `1` (MB_OKCANCEL)
      - `4` (MB_YESNO)
    - **アイコンの種類:**
      - `16` (MB_ICONERROR)
      - `32` (MB_ICONQUESTION)
      - `48` (MB_ICONWARNING)
      - `64` (MB_ICONINFORMATION)
    - 例えば、「はい/いいえ」ボタンと「情報」アイコンを表示したい場合は `4 + 64 = 68` を指定します。

- **`表示ミリ秒`** (任意)
  - メッセージボックスが自動的に閉じるまでの時間（ミリ秒単位）。
  - **デフォルト**: `3000` (3秒)

# 戻り値

プログラムの終了コードとして、押されたボタンに応じた値が返されます。バッチファイルなどで `%ERRORLEVEL%` を使って判定できます。

- `1` (IDOK): OK ボタン
- `2` (IDCANCEL): キャンセル ボタン
- `3` (IDABORT): 中止 ボタン
- `4` (IDRETRY): 再試行 ボタン
- `5` (IDIGNORE): 無視 ボタン
- `6` (IDYES): はい ボタン
- `7` (IDNO): いいえ ボタン
- `32000` (IDTIMEOUT): タイムアウトにより自動で閉じた場合

# 文字コード

引数として渡された「本文」と「タイトル」の文字列にUTF-8が含まれている場合、Windowsのメッセージボックスで正しく表示するために、内部で自動的にShift_JIS (CP932) へ変換します。これにより、スクリプトファイルなどをUTF-8で保存していても文字化けを防ぎます。

# 例

## 例1: 基本的な使い方
```bat
HmTimerMessageBox.exe "本文はこれこれこれ" "次はタイトルだ!!" 1 5000
```
- OK/Cancelボタンを表示し、5秒でタイムアウトします。

## 例2: 必須引数のみ
```bat
HmTimerMessageBox.exe "本文のみ指定"
```
- デフォルト値が適用され、OKボタンのみで3秒でタイムアウトします。

## 例3: アイコンとボタンの組み合わせ
```bat
HmTimerMessageBox.exe "保存しますか？" "確認" 36
```
- `36` は `4` (MB_YESNO) + `32` (MB_ICONQUESTION) です。
- 「はい/いいえ」ボタンと「疑問符」アイコンが表示されます。

[https://秀丸マクロ.net/?page=nobu_tool_hm_timer_msgbox](https://秀丸マクロ.net/?page=nobu_tool_hm_timer_msgbox)