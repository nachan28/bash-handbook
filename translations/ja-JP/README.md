# bash-handbook

[![CC 4.0][cc-image]][cc-url]
[![NPM version][npm-image]][npm-url]
[![Gitter][gitter-image]][gitter-url]

このドキュメントは、bashを学びたいがあまり深入りはしすぎたくないという人に向けて書かれています。

> **Tip**: このハンドブックに基づいたインタラクティブなワークショップ：[**learnyoubash**](https://git.io/learnyoubash) を試してみてください！

# パッケージ化されたマニュアル

このハンドブックはnpmを使ってインストールすることができます。以下を実行してください。

```
$ npm install -g bash-handbook
```

今、あなたは`bash-handbook`をコマンドラインで実行できるようになっているはずです。このコマンドにより、あなたの選択した `$PAGER`でマニュアルが開かれます。もしくは、このままここでマニュアルを読むこともできます。

ソースはここから見ることができます。: <https://github.com/denysdovhan/bash-handbook>

# 翻訳

現在、**bash-handbook** には以下の翻訳が存在します。

- [Português (Brasil)](/translations/pt-BR/README.md)
- [简体中文 (中国)](/translations/zh-CN/README.md)
- [繁體中文（台灣）](/translations/zh-TW/README.md)
- [한국어 (한국)](/translations/ko-KR/README.md)

[**他の言語の翻訳をリクエストする**][tr-request]

[tr-request]: https://github.com/denysdovhan/bash-handbook/issues/new?title=Translation%20Request:%20%5BPlease%20enter%20language%20here%5D&body=I%20am%20able%20to%20translate%20this%20language%20%5Byes/no%5D

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
# 目次

- [イントロダクション](#introduction)
- [シェルとモード](#shells-and-modes)
  - [インタラクティブモード](#interactive-mode)
  - [非インタラクティブモード](#non-interactive-mode)
  - [終了コード](#exit-codes)
- [コメント](#comments)
- [変数](#variables)
  - [ローカル変数](#local-variables)
  - [環境変数](#environment-variables)
  - [位置パラメータ](#positional-parameters)
- [シェルの展開](#shell-expansions)
  - [ブレース展開](#brace-expansion)
  - [コマンド置換](#command-substitution)
  - [算術式展開](#arithmetic-expansion)
  - [ダブルクォートとシングルクォート](#double-and-single-quotes)
- [配列](#arrays)
  - [配列の宣言](#array-declaration)
  - [配列の展開](#array-expansion)
  - [配列のスライス](#array-slice)
  - [配列に要素を追加する](#adding-elements-into-an-array)
  - [配列から要素を削除する](#deleting-elements-from-an-array)
- [ストリーム・パイプ・リスト](#streams-pipes-and-lists)
  - [ストリーム](#streams)
  - [パイプ](#pipes)
  - [コマンドのリスト](#lists-of-commands)
- [条件文](#conditional-statements)
  - [基本式と結合式](#primary-and-combining-expressions)
  - [`if`文を使う](#using-an-if-statement)
  - [`case`文を使う](#using-a-case-statement)
- [Loops](#loops)
  - [`for`ループ](#for-loop)
  - [`while`ループ](#while-loop)
  - [`until`ループ](#until-loop)
  - [`select`ループ](#select-loop)
  - [ループ制御](#loop-control)
- [関数](#functions)
  - [デバッグ](#debugging)
- [ユーザー入力の読み取り](#reading-user-input)
- [あとがき](#afterword)
- [もっと学びたい？](#want-to-learn-more)
- [その他のリソース](#other-resources)
- [ライセンス](#license)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# イントロダクション

もしあなたが開発者なら、時間の価値をよく知っていることでしょう。作業プロセスを最適化することは、あなたの仕事の中でも最も重要な側面です。

効率と生産性を高めるための道のりの中で、私たちはしばしば、繰り返しk＼行わなければならない作業に直面します。

* スクリーンショットを撮ってサーバーにアップロードする
* 様々な形式で渡されてくるテキストを処理する
* ファイルを異なるフォーマットに変換する
* プログラムの出力をパースする

そこで登場するのが、私たちの救世主 **Bash** です。

bashは、GNUプロジェクトの一環で[Bourne shell](https://en.wikipedia.org/wiki/Bourne_shell)に代わる無料ソフトウェアとして開発されたUnixのシェルです。[Brian Fox][]によって書かれ、1989年にリリースされました。

しかしなぜ、30年以上も前に書かれたものを学ぶ必要があるのでしょうか？答えは単純です：bashは今日において、Unixベースのシステム上で効率的なスクリプトを書くための最も強力でポータブルなツールだからです。そのため、あなたはbashを学ぶべきなのです。以上。

このハンドブックでは、bashの中で最も重要な概念について、例を交えながら説明していくつもりです。この概論があなたの役に立つことを願っています。

# シェルとモード

bashはインタラクティブモードと非インタラクティブモードの2つのモードで動きます。

## インタラクティブモード

もしあなたがUbuntuを使っているなら、7つの仮想ターミナルが利用可能です。
GUIデスクトップ環境は7つ目の仮想ターミナルに割り当てられているため、`Ctrl-Alt-F7`でGUIに切り替えることができます。

シェルは`Ctrl-Alt-F1`で開くことができます。そうすると、見慣れたGUIは姿を消し、ひとつの仮想ターミナルが現れます。

もし以下のようなものが見えたら、あなたはインタラクティブモードで作業しているということです。

    user@host:~$

ここでは、`ls`, `grep`, `cd`, `mkdir`, `rm` のような様々なUnixコマンドを入力し、その実行結果を見ることができます。

私たちは、これをシェルのインタラクティブモードと呼びます。なぜなら、シェルがユーザと直接やり取りをするからです。

仮想ターミナルを使うことはそれほど便利とはいえません。例えば、ドキュメントの編集と別のコマンドの実行を同時に行いたいときなどは、以下のような仮想ターミナルエミュレーターを使った方がいいでしょう。

- [GNOME Terminal](https://en.wikipedia.org/wiki/GNOME_Terminal)
- [Terminator](https://en.wikipedia.org/wiki/Terminator_(terminal_emulator))
- [iTerm2](https://en.wikipedia.org/wiki/ITerm2)
- [ConEmu](https://en.wikipedia.org/wiki/ConEmu)

## 非インタラクティブモード

非インタラクティブモードでは、シェルはコマンドをファイルやパイプから読み取って実行します。インタプリタがファイルの末尾に到達すると、シェルプロセスはセッションを終了し親プロセスに戻ります。

シェルを非インタラクティブモードで実行するには、以下のコマンドを使ってください。

    sh /path/to/script.sh
    bash /path/to/script.sh

上記の例では、`script.sh`は単なるテキストファイルで、シェルのインタプリタが評価できるコマンドで構成されます。`sh`や`bash`はシェルのインタプリタプログラムです。`script.sh`はあなたの好きなエディタ（vim, nano, Sublime Text, Atomなど）で作成することができます。

`chmod`コマンドを使用してスクリプトファイルを実行可能にすることで、スクリプトの実行を簡単にすることもできます。

    chmod +x /path/to/script.sh

このとき、スクリプトの最初の行は、ファイルを実行するのにどのプログラムを使うべきかを示す必要があります。例えば以下のようにです：

```bash
#!/bin/bash
echo "Hello, world!"
```

もしあなたが`bash`の代わりに`sh`を使いたければ、`#!/bin/bash`の部分を`#!/bin/sh`に変えてください。`#!`という文字列は[shebang(シバン)](http://en.wikipedia.org/wiki/Shebang_%28Unix%29)として知られています。今、あなたはスクリプトを以下のようにして実行できます。

    /path/to/script.sh

先ほどの例で使用した便利なテクニックは、`echo`を使ってテキストをターミナル画面に表示することです。

シバンのもうひとつの使い方は、以下の通りです。

```bash
#!/usr/bin/env bash
echo "Hello, world!"
```

このシバンの使い方の利点は、環境変数`PATH`に基づいて目的のプログラム（今回の場合は`bash`）を検索してくれる点です。この方法は、しばしば最初の方法よりも好まれます。その理由は、プログラムのファイルシステム上の場所がいつも分かっているとは限らないためです。これは、システムの`PATH`変数がプログラムの別バージョンを指すように設定されている場合にも便利です。例えば、`bash`のオリジナルバージョンを保持したまま新しいバージョンをインストールし、新しいバージョンの場所を`PATH`変数に挿入したときなどです。`#!/bin/bash`を使うとオリジナルの`bash`が使用されることになりますが、`#!/usr/bin/env bash`を使えば新しいバージョンが使用されます。


##  終了コード

全てのコマンドは**終了コード** (**リターンステータス** もしくは **終了ステータス**)を返します。成功したコマンドは常に`0`（ゼロコード）を返し、失敗したコマンドは`0`以外の値（エラーコード）を返します。エラーコードは1から255までの自然数と決まっています。

スクリプトを書く際に便利なコマンドのもうひとつは、`exit`です。このコマンドは現在の実行を終了して終了コードをシェルに渡すために使われます。`exit`コードを引数なしで実行すると、実行中のスクリプトが終了し、`exit`の前に実行された最後のコマンドの終了コードを返します。

プログラムが終了するとき、シェルはそのプログラムの終了コードを環境変数`$?`に渡します。`$?`変数は、スクリプトの実行が成功したかどうかを確認するのに使われます。

スクリプトを終了させるのに`exit`を使うことができるのと同様に、関数を終了して呼び出し元に**終了コード**を返すのには`return`コマンドを使うことができます。関数の中で`exit`を使うこともできますが、その場合関数だけでなくプログラム自体も終了します。

# コメント

スクリプトには _コメント_ も含めることができます。コメントは`shell`のインタプリタに無視される特別な文です。`#`から始まり、行の最後まで続きます。

例えば：

```bash
#!/bin/bash
# This script will print your username.
whoami
```

> **Tip**: あなたの書いたスクリプトが何をするのか・なぜそうするのかを説明するためにコメントを使ってみてください。

# 変数
ほとんどのプログラミング言語と同様に、bashでも変数を作成することができます。

bashにはデータ型がありません。変数には、数値もしくは一文字以上の文字列のみを入れることができます。変数には3つの種類があります：ローカル変数、環境変数、 _位置パラメータ_ としての変数です。

## ローカル変数

**ローカル変数**は、ひとつのスクリプト内にのみ存在する変数です。これらは他のプログラムやスクリプトにアクセスすることはできません。

ローカル変数は`=`を使って宣言することができ（ルールとして、変数名・`=`・値の間にはスペースを挿入しません）、変数の値は`$`を使って取り出すことができます。例えば：

```bash
username="denysdovhan"  # 変数を宣言する
echo $username          # 変数の値を表示する
unset username          # 変数を削除する
```

`local`キーワードを使うと、ひとつの関数に対してローカルな変数を宣言することができます。この場合、関数が終了すると変数も消えることになります。

```bash
local local_var="I'm a local value"
```

## 環境変数

**環境変数**は現在のシェルのセッションで実行中のどのプログラムやスクリプトにもアクセス可能な変数です。ローカル変数と同じように作成されますが、`export`キーワードを使用する点が異なります。

```bash
export GLOBAL_VAR="I'm a global variable"
```

bashには多くのグローバル変数が存在します。あなたはこれらの変数をかなり頻繁に見ることになるでしょう。特に実用的なものについての早見表を以下に示します。

| 変数     |
説明                                                   |
| :----------- | :------------------------------------------------------------ |
| `$HOME`      | 現在のユーザーのホームディレクトリ。                            |
| `$PATH`      | コロンで区切られたディレクトリのリスト。シェルはこれを見てコマンドを検索する。 |
| `$PWD`       | 現在の作業ディレクトリ                                |
| `$RANDOM`    | 0から32767までのランダムな数値。                           |
| `$UID`       | 現在のユーザーのユーザーID（数値）。               |
| `$PS1`       | 第一プロンプト文字列。                                   |
| `$PS2`       | 第二プロンプト文字列。                                  |

bashにおける環境変数のさらなるリストを見るには、[このリンク](http://tldp.org/LDP/Bash-Beginners-Guide/html/sect_03_02.html#sect_03_02_04)を辿ってください。

## 位置パラメータ

**位置パラメータ**は、関数が評価されるときに、その引数として位置に基づいて割り当てられる変数です。以下の表は、位置パラメータとその他の特殊な変数、そしてそれらの意味を示しています。

| パラメータ     | 説明                                                 |
| :------------- | :---------------------------------------------------------- |
| `$0`           | スクリプトの名前。                                              |
| `$1 … $9`      | パラメータ配列の1〜9番目の要素。                     |
| `${10} … ${N}` | パラメータ配列の10〜N番目の要素。                    |
| `$*` or `$@`   | `$0`以外のすべての位置パラメータ。                      |
| `$#`           | `$0`を除いたパラメータの数。                 |
| `$FUNCNAME`    | 関数名（関数の中でしか値を持たない）。     |

以下の例では、位置パラメータは`$0='./script.sh'`, `$1='foo'`, `$2='bar'`のようになる：

    ./script.sh foo bar

変数は _デフォルト_ の値をもつこともできます。以下のような文法を使うことでデフォルト値を定義することが可能です。

```bash
# 変数が空である場合、デフォルト値を設定する
: ${VAR:='default'}
: ${1:='first'}
# もしくは、以下のように書くこともできる
FOO=${FOO:-'default'}
```

# シェルの展開













