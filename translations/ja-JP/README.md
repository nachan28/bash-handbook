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
- [日本語](/translations/ja-JP/README.md)

[**他の言語の翻訳をリクエストする**][tr-request]

[tr-request]: https://github.com/denysdovhan/bash-handbook/issues/new?title=Translation%20Request:%20%5BPlease%20enter%20language%20here%5D&body=I%20am%20able%20to%20translate%20this%20language%20%5Byes/no%5D

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
# 目次

- [イントロダクション](#イントロダクション)
- [シェルとモード](#シェルとモード)
  - [インタラクティブモード](#インタラクティブモード)
  - [非インタラクティブモード](#非インタラクティブモード)
  - [終了コード](#終了コード)
- [コメント](#コメント)
- [変数](#変数)
  - [ローカル変数](#ローカル変数)
  - [環境変数](#環境変数)
  - [位置パラメータ](#位置パラメータ)
- [シェルの展開](#シェルの展開)
  - [ブレース展開](#ブレース展開)
  - [コマンド置換](#コマンド置換)
  - [算術式展開](#算術式展開)
  - [ダブルクォートとシングルクォート](#ダブルクォートとシングルクォート)
- [配列](#配列)
  - [配列の宣言](#配列の宣言)
  - [配列の展開](#配列の展開)
  - [配列のスライス](#配列のスライス)
  - [配列に要素を追加する](#配列に要素を追加する)
  - [配列から要素を削除する](#配列から要素を削除する)
- [ストリーム・パイプ・リスト](#ストリーム・パイプ・リスト)
  - [ストリーム](#ストリーム)
  - [パイプ](#パイプ)
  - [コマンドのリスト](#コマンドのリスト)
- [条件文](#条件文)
  - [基本式と結合式](#基本式と結合式)
  - [`if`文を使う](#`if`文を使う)
  - [`case`文を使う](#`case`文を使う)
- [ループ](#ループ)
  - [`for`ループ](#`for`ループ)
  - [`while`ループ](#`while`ループ)
  - [`until`ループ](#`until`ループ)
  - [`select`ループ](#`select`ループ)
  - [ループ制御](#ループ制御)
- [関数](#関数)
  - [デバッグ](#デバッグ)
- [ユーザー入力の読み取り](#ユーザー入力の読み取り)
- [あとがき](#あとがき)
- [もっと学びたい？](#もっと学びたい？)
- [その他のリソース](#その他のリソース)
- [ライセンス](#ライセンス)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# イントロダクション

もしあなたが開発者なら、時間の価値をよく知っていることでしょう。作業プロセスを最適化することは、あなたの仕事の中でも最も重要な側面です。

効率と生産性を高めるための道のりの中で、私たちはしばしば、繰り返し行わなければならない作業に直面します。

* スクリーンショットを撮ってサーバーにアップロードする
* 様々な形式で渡されてくるテキストを処理する
* ファイルを異なるフォーマットに変換する
* プログラムの出力をパースする

そこで登場するのが、私たちの救世主 **Bash** です。

bashは、GNUプロジェクトの一環で[Bourne shell](https://en.wikipedia.org/wiki/Bourne_shell)に代わる無料ソフトウェアとして開発されたUnixのシェルです。[Brian Fox][]によって書かれ、1989年にリリースされました。

しかしなぜ、30年以上も前に書かれたものを学ぶ必要があるのでしょうか？答えは単純です：bashは今日において、Unixベースのシステム上で効率的なスクリプトを書くための最も強力でポータブルなツールだからです。そのため、あなたはbashを学ぶべきなのです。以上。

このハンドブックでは、bashの中で最も重要な概念について、例を交えながら説明していきます。この概論があなたの役に立つことを願っています。

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

_展開_ はコマンドラインが _トークン_ に分割された後に実行される処理で、算術演算の計算やコマンド実行の結果の保存などのためのメカニズムです。

もし興味があれば、[more about shell expansions](https://www.gnu.org/software/bash/manual/bash.html#Shell-Expansions)をぜひ読んでみてください。

## ブレース展開

ブレース展開は、任意の文字列を生成することを可能にします。これは  _ファイル名の展開_ とよく似ています。例えば：

```bash
echo beg{i,a,u}n # begin began begun
```

また、ブレース展開は範囲（ループを使ってイテレーションされる）を生成するのにも使用することができます。

```bash
echo {0..5} # 0 1 2 3 4 5
echo {00..8..2} # 00 02 04 06 08
```
**Note: {00..8..2} は {start..end..steps} の意味です**

`echo {0..10..3} # 0 3 6 9`

## コマンド置換

コマンド置換を使うと、コマンドを評価して別のコマンドや変数で置換することができます。 コマンド置換を利用するにはコマンドを``` `` ```もしくは`$()`で囲む必要があります。例えば、次のようにすることができます：

```bash
now=`date +%T`
# もしくは
now=$(date +%T)

echo $now # 19:08:26
```

## 算術式展開

bashの中で私たちは自由に算術演算を行うことができます。ただし、式は`$(( ))`で囲まれている必要があります。算術式展開のフォーマットは以下の通りです：

```bash
result=$(( ((10 + 5*3) - 7) / 2 ))
echo $result # 9
```

算術式展開の中では、通常、変数の頭に`$`をつけません。

```bash
x=4
y=7
echo $(( x + y ))     # 11
echo $(( ++x + y++ )) # 12
echo $(( x + y ))     # 13
```

## ダブルクォートとシングルクォート
ダブルクォートとシングルクォートの間にはある重要な違いがあります。ダブルクォートの中では変数やコマンド置換が展開されますが、シングルクォートの中ではされません。例えば：

```bash
echo "Your home: $HOME" # Your home: /Users/<username>
echo 'Your home: $HOME' # Your home: $HOME
```

ローカル変数や環境変数が空白を含む可能性があるなら、ダブルクォートの中で展開するように注意してください。無害な例として、ユーザーからの入力を`echo`を使って表示する場合を考えてみましょう。

```bash
INPUT="A string  with   strange    whitespace."
echo $INPUT   # A string with strange whitespace.
echo "$INPUT" # A string  with   strange    whitespace.
```

ひとつ目の`echo`は5つの引数を受け取ります - つまり、$INPUTは個別の単語に分けられ、`echo`はそれぞれの引数の間にひとつの空白をプリントします。ふたつ目の例では、`echo`は単一の引数を受け取ります（空白を含んだ$INPUT全体）。

次に、もっと深刻な例を考えてみましょう。

```bash
FILE="Favorite Things.txt"
cat $FILE   # `Favorite`と`Things.txt`の2つのファイルを表示しようとする
cat "$FILE" # `Favorite Things.txt`という1つのファイルを表示する
```

この例の問題はFILEを`Favorite-Things.txt`に改名することで解決できますが、環境変数、位置パラメータ、あるいは他のコマンド（`find`や`cat`、etc）の出力などが入力として与えられる場合も考えなければなりません。もし入力が空白を含む可能性が*少しでも*あるなら、ダブルクォートで囲むように注意してください。

# 配列

他のプログラミング言語と同様に、配列は複数の値への参照を可能にする変数です。bashにおいても、配列は0ベースです。つまり、配列の最初の要素のインデックスは0ということです。

配列を扱うとき、私たちは`IFS`という特別な環境変数について意識するべきです。**IFS**、あるいは**Input Field Separator**は、配列の中の要素を区切る文字です。デフォルトの値は空白`IFS=' '`です。

## 配列の宣言

bashでは、単純に配列のインデックスに値を割り当てることで、配列を作成します。

```bash
fruits[0]=Apple
fruits[1]=Pear
fruits[2]=Plum
```

もしくは、以下のような一括割り当てを使用することでも配列を作成できます。

```bash
fruits=(Apple Pear Plum)
```

## 配列の展開

配列の要素は、他の変数と同じように取り出すことができます。

```bash
echo ${fruits[1]} # Pear
```

配列の全ての要素を取り出すには、`*`もしくは`@`を数値のインデックスの代わりに用います。

```bash
echo ${fruits[*]} # Apple Pear Plum
echo ${fruits[@]} # Apple Pear Plum
```

上記の2行にはひとつ重要な違いがあります：以下のような、空白を含んだ配列の要素を考えてください：

```bash
fruits[0]=Apple
fruits[1]="Desert fig" # 空白を含んだ要素
fruits[2]=Plum
```

配列のそれぞれの要素を取り出して、各行に表示したいとします。そのために、ビルトインの`printf`を使ってみます。

```bash
printf "+ %s\n" ${fruits[*]}
# + Apple
# + Desert
# + fig
# + Plum
```

なぜ`Desert`と`fig`が異なる行に表示されたのでしょうか？試しに引用符を使ってみましょう：

```bash
printf "+ %s\n" "${fruits[*]}"
# + Apple Desert fig Plum
```

今度は全ての要素が1行に表示されてしまいました - これは私たちの求めていたものではありません！
こんなとき、`${fruits[@]}`の出番です。

```bash
printf "+ %s\n" "${fruits[@]}"
# + Apple
# + Desert fig
# + Plum
```

ダブルクォーテーション内で`${fruits[@]}`を使用すると、配列内の各要素ごとに個別の引数として展開されます；配列要素内の空白も保持されます。

## 配列のスライス

加えて、_slice_ 演算子を使うことで配列のスライスを取得することができます。

```bash
echo ${fruits[@]:0:2} # Apple Desert fig
```

上記の例では、`${fruits[@]}` が配列のすべてのコンテンツを展開し、`:0:2`がインデックス0から始まる長さ2のスライスを取得しています。

## 配列に要素を追加する

配列に要素を追加するやり方も、とてもシンプルです。複合代入はこの場合特に有用です。

```bash
fruits=(Orange "${fruits[@]}" Banana Cherry)
echo ${fruits[@]} # Orange Apple Desert fig Plum Banana Cherry
```

上記の例では、`${fruits[@]}`はその配列要素全てを複合代入の中で展開・置換し、新たな値を配列`fruits`に再代入しています。

## 配列から要素を削除する

配列から要素を削除するには、`unset`コマンドを使います。


```bash
unset fruits[0]
echo ${fruits[@]} # Apple Desert fig Plum Banana Cherry
```

# ストリーム、パイプ、リスト

bashは他のプログラムやそれらの出力と一緒に動作するための強力なツールを備えています。ストリームを使用することで、あるプログラムの出力を他のプログラムやファイルに送ることができます。このようにして私たちはログを書きこんだり、私たちのしたいことをできるわけです。

パイプは、コマンドのベルトコンベアを作り出し、コマンドの実行を制御することを可能にします。

これらの強力で洗練されたツールの使い方を理解することはとても重要です。

## ストリーム

bashは入力を受け取り出力を文字の連続：**ストリーム**として返します。これらストリームは、ファイルや別のストリームにリダイレクトさせることができます。

システムには、3つの基本的なファイルディスクリプタがあります。

| コード | ディスクリプタ | 説明          |
| :--: | :--------: | :------------------- |
| `0`  | `stdin`    | 標準入力  |
| `1`  | `stdout`   | 標準出力 |
| `2`  | `stderr`   | 標準エラー出力   |

リダイレクトはコマンドの出力がどこに行くか、コマンドの入力がどこから来るかを制御します。ストリームをリダイレクトするためには、以下のような演算子が使われます。

| 演算子 | 説明                                  |
| :------: | :------------------------------------------- |
| `>`      | 出力をリダイレクトする                          |
| `&>`     | 出力とエラー出力をリダイレクトする          |
| `&>>`    | リダイレクトされた標準出力と標準エラー出力を追記する|
| `<`      | 入力をリダイレクトする                            |
| `<<`     | [ヒアドキュメント](http://tldp.org/LDP/abs/html/here-docs.html) syntax |
| `<<<`    | [ヒアストリング](http://www.tldp.org/LDP/abs/html/x17837.html) |

以下はリダイレクトの例です。

```bash
# lsの出力はlist.txtに書き込まれる
ls -l > list.txt

# lsの出力はlist.txtのファイル末尾に追記される
ls -a >> list.txt

# エラーは全てerrors.txtに書き込まれる
grep da * 2> errors.txt

# errors.txtから読み出す
less < errors.txt
```

## パイプ

ストリームは、ファイルにリダイレクトするだけでなく、他のプログラムにリダイレクトすることも可能です。**パイプ**はまさに、あるプログラムの出力を他のプログラムの入力として使用できるようにしてくれます。

以下の例では、`command1`は自身の出力を`command2`に送り、さらに`command2`は自身の出力を`command3`の入力として渡しています。

    command1 | command2 | command3

このような構造は**パイプライン**と呼ばれます。

実用では、これはいくつかのプログラムを通してデータを処理するときに使われます。例えば、以下の例では`ls -l`の出力が`grep`プログラムに渡され、`grep`は`.md`の拡張子を持つファイルだけを表示します。その出力は最終的に`less`プログラムに渡されています：

    ls -l | grep .md$ | less

パイプラインの終了ステータス（exit status）は、通常パイプラインの最後のコマンドの終了ステータスになります。シェルはパイプラインの中の全てのコマンドが完了するまで終了ステータスを返しません。もしパイプラインの中のいずれかのコマンドが失敗した場合にパイプライン全体のステータスも失敗とみなしたいなら、pipefail オプションをセットしてください：

    set -o pipefail

## コマンドのリスト

**コマンドのリスト**は、`;`, `&`, `&&` あるいは `||`で区切られたひとつ以上のパイプラインの集合です。

コマンドが制御演算子`&`で終了すると、シェルはコマンドをサブシェル内で非同期的に実行します。言い換えれば、そのコマンドはバックグラウンドで実行されます。

`;`で区切られた複数のコマンドは、順番に実行されます。シェルはそれぞれのコマンドが終了するのを待つということです。

```bash
# command2はcommand1の後に実行される
command1 ; command2

# 上記は以下と同じ
command1
command2
```

`&&`と`||`で区切られたコマンドリストはそれぞれ _AND_ リスト、 _OR_ リストと呼ばれます。

_ANDリスト_ は以下のような見た目です。

```bash
# command2はcommand1が正常に終了した（終了コード0を返した）場合のみ実行される
command1 && command2
```

_ORリスト_ は以下のような見た目です。

```bash
# command2はcommand1が正常に終了しなかった（エラーコードを返した）場合のみ実行される
command1 || command2
```

_ANDリスト_ 、 _ORリスト_ が返す終了コードは、最後に実行されたコマンドの終了コードです。

# 条件文
他の言語と同様で、bashにおいても、条件文を使うとある処理を実行するかどうかを決めることができます。`[[]]`で囲まれた式を評価し、結果が返されます。

条件式は`&&`や`||`演算子を含むことができます（それぞれ _AND_ 、 _OR_ という意味です）。これに加えて、たくさんの[その他の便利な式](#primary-and-combining-expressions)が存在します。

条件文には2種類あります：`if`文と`case`文です。

## 基本式と結合式

`[[]]`(`sh`では`[]`)で囲まれえた式は**テストコマンド**あるいは**基本式**と呼ばれます。これらの式は、条件の結果を示すのに役立ちます。以下の表では、`sh`でも動作するように`[]`を使っています。両者の違いについては[the difference between double and single square brackets in bash（bashにおける二重角括弧と一重角括弧の違い）](http://serverfault.com/a/52050)に書かれています。

**ファイルシステムを扱う**

| 基本式       | 意味                                                     |
| :-----------: | :----------------------------------------------------------- |
| `[ -e FILE ]` | `FILE`が存在するならTrue                                   |
| `[ -f FILE ]` | `FILE`が存在し、かつ`FILE`が通常のファイルならTrue             |
| `[ -d FILE ]` | `FILE`が存在し、かつ`FILE`がディレクトリならTrue                |
| `[ -s FILE ]` | `FILE`が存在し、かつ`FILE`が空ではない（サイズが0より大きい）ならTrue
| `[ -r FILE ]` | `FILE`が存在し、かつ`FILE`が読み取り可能ならTrue                   |
| `[ -w FILE ]` | `FILE`が存在し、かつ`FILE`が書き込み可能ならTrue                   |
| `[ -x FILE ]` | `FILE`が存在し、かつ`FILE`が実行可能ならTrue                 |
| `[ -L FILE ]` | `FILE`が存在し、かつ`FILE`がシンボリックリンクならTrue              |
| `[ FILE1 -nt FILE2 ]` | FILE1がFILE2より新しければTrue                   |
| `[ FILE1 -ot FILE2 ]` | FILE1がFILE2より古ければTrue                   |

**文字列を扱う**

| 基本式        | 意味                                                     |
| :------------: | :---------------------------------------------------------- |
| `[ -z STR ]`   | `STR`が空（長さが0）                    |
| `[ -n STR ]`   | `STR`が空でない（長さが0でない）            |
| `[ STR1 == STR2 ]` | `STR1`と`STR2`が等しい                           |
| `[ STR1 != STR2 ]` | `STR1`と`STR2`が等しくない                        |

**算術二項演算子**

| 基本式             | 意味                                                |
| :-----------------: | :----------------------------------------------------- |
| `[ ARG1 -eq ARG2 ]` | `ARG1`が`ARG2`と等しい                         |
| `[ ARG1 -ne ARG2 ]` | `ARG1`と`ARG2`が等しくない                 |
| `[ ARG1 -lt ARG2 ]` | `ARG1`が`ARG2`より小さい                    |
| `[ ARG1 -le ARG2 ]` | `ARG1`が`ARG2`以下        |
| `[ ARG1 -gt ARG2 ]` | `ARG1`が`ARG2`より大きい                 |
| `[ ARG1 -ge ARG2 ]` | `ARG1`が`ARG2`以上     |

条件式は**結合式**を使って結合することもできます。

| 結合式      | 効果                                                      |
| :------------: | :---------------------------------------------------------- |
| `[ ! EXPR ]`   | `EXPR`FalseならTrue                                    |
| `[ (EXPR) ]`   | `EXPR`の値を返す                                |
| `[ EXPR1 -a EXPR2 ]` | 論理演算 _AND_ 。`EXPR1`と`EXPR2`が両方TrueならTrue |
| `[ EXPR1 -o EXPR2 ]` | 論理演算 _OR_ 。`EXPR1`か`EXPR2`がTrueならTrue|

もちろん、他にも便利な基本式があります。それらは[Bash man pages](http://www.gnu.org/software/bash/manual/html_node/Bash-Conditional-Expressions.html)で簡単に見つけることができます。

## `if`文を使う

`if`文は他のプログラミング言語のそれと同じように動作します。もし括弧の中の式がtrueなら、`then`と`fi`の間のコードが実行されます。`fi`は条件付きで実行されるコードの終わりを示します。

```bash
# 1行
if [[ 1 -eq 1 ]]; then echo "true"; fi

# 複数行
if [[ 1 -eq 1 ]]; then
  echo "true"
fi
```

同じように、`if..else`文も使用可能です。

```bash
# 1行
if [[ 2 -ne 1 ]]; then echo "true"; else echo "false"; fi

# 複数行
if [[ 2 -ne 1 ]]; then
  echo "true"
else
  echo "false"
fi
```

時には`if..else`文がやりたいことを実現するのに不十分なときもあるでしょう。そんな時は`if..elif..else`文の存在を思い出しましょう。

以下の例を見てください：

```bash
if [[ `uname` == "Adam" ]]; then
  echo "Do not eat an apple!"
elif [[ `uname` == "Eva" ]]; then
  echo "Do not take an apple!"
else
  echo "Apples are delicious!"
fi
```

## `case`文を使う

いくつかの異なる処理を実行する可能性がある場合、`case`文の方が、ネストされた`if`文よりも便利かもしれません。より複雑な条件には、以下のように`case`文を使用してください。

```bash
case "$extension" in
  "jpg"|"jpeg")
    echo "It's image with jpeg extension."
  ;;
  "png")
    echo "It's image with png extension."
  ;;
  "gif")
    echo "Oh, it's a giphy!"
  ;;
  *)
    echo "Woops! It's not image!"
  ;;
esac
```

各ケースはパターンにマッチする式です。`|`は複数のパターンを区切るのに使用され、`)`演算子はパターンリストを終了させるのに使用されます。最初にマッチしたケースのコマンドが実行されます。`*`は定義されたどのパターンにも一致しなかった場合のパターンです。それぞれのブロックは`;;`演算子で区切られます。

# ループ

ここでは驚くことはないでしょう。他のどのプログラミング言語でも同じで、bashのループは条件式がtrueの間処理を繰り返すコードブロックです。

bashでは4種類のループがあります：`for`、`while`、 `until`、`select`です。

## `for`ループ

`for`はC言語のそれととてもよく似ています。このような感じです：

```bash
for arg in elem1 elem2 ... elemN
do
  # 処理
done
```

ループを通過するたびに `arg`は`elem1`から`elemN`までの値を取ります。値にはワイルドカードや[ブレース展開](#ブレース展開)が含まれることがあります。

また、`for`ループは1行で書くこともできます。ただし、その場合以下のように`do`の前にセミコロンをつける必要があります：

```bash
for i in {1..5}; do echo $i; done
```

ところで、, もし`for..in..do`が少し変に思えるなら、`for`ループをC言語ライクなスタイルで書くこともできます：

```bash
for (( i = 0; i < 10; i++ )); do
  echo $i
done
```

`for`はディレクトリ内の各ファイルに同じ処理をしたいときにとても便利です。例えば、全ての`.bash`ファイルを`script`ディレクトリに移して、かつそれらに実行許可を付与するとき、スクリプトは次のようになります：

```bash
#!/bin/bash

for FILE in $HOME/*.bash; do
  mv "$FILE" "${HOME}/scripts"
  chmod +x "${HOME}/scripts/${FILE}"
done
```

## `while`ループ

`while`ループは、条件をテストし、その条件が _true_ である限りコマンドを繰り返し実行します。条件は`if..then`文のところで利用した[基本式](#基本式と結合式)と同じです。そのため、`while`ループは以下のようになります。

```bash
while [[ condition ]]
do
  # 処理
done
```

`for`ループの場合と同じように、もし`do`と条件を同じ行に書きたいなら、`do`の前にセミコロンをつける必要があります。

実際に動作する例は以下です：

```bash
#!/bin/bash

# 0から9までの数字を2乗する
x=0
while [[ $x -lt 10 ]]; do # 10より小さい値
  echo $(( x * x ))
  x=$(( x + 1 )) # xをインクリメントする
done
```

## `until`ループ

`until`ループはちょうど`while`ループの反対です。`while`のように条件をチェックしますが、条件が _false_ の間処理を繰り返します：

```bash
until [[ condition ]]; do
  # 処理
done
```

## `select`ループ

`select`ループはユーザーメニューを制御するのに役立ちます。`select`は`for`ループとほとんど同じ文法を持ちます：

```bash
select answer in elem1 elem2 ... elemN
do
  # 処理
done
```

`select`は`elem1`から`elemN`までの全てをシーケンス番号と共に画面に表示し, その後ユーザープロンプトを表示します。通常、これは`$?` (`PS3`変数)として表示されます。ユーザーの回答は`answer`に格納されます。`answer`が`1..N`の中の数字だったら、`処理` が実行され`select`は次のイテレーションに移行します。そのため、`break`文を使用する必要があります。

実際に動作する例は以下です。

```bash
#!/bin/bash

PS3="Choose the package manager: "
select ITEM in bower npm gem pip
do
  echo -n "Enter the package name: " && read PACKAGE
  case $ITEM in
    bower) bower install $PACKAGE ;;
    npm)   npm   install $PACKAGE ;;
    gem)   gem   install $PACKAGE ;;
    pip)   pip   install $PACKAGE ;;
  esac
  break # 無限ループを防ぐ
done
```

この例では、何のパッケージマネージャーを使いたいかをユーザーに聞いています。その後、何のパッケージをインストールしたいかを聞き、最後にインストールに進みます。

実行結果として、以下が得られます：

```
$ ./my_script
1) bower
2) npm
3) gem
4) pip
Choose the package manager: 2
Enter the package name: bash-handbook
<installing bash-handbook>
```

## ループの制御

通常のループの終了を待たずにループを止めたい、あるいはイテレーションをスキップしたい状況もあるでしょう。その場合、シェルのビルトインの`break`と`continue`文が使用可能です。これらはどの種類のループでも動作します。

`break`文はループの終了を待たずに現在のループから脱出するために使われます。私たちはすでに`break`を使う例を見てきました。

`continue` 文はイテレーションを1回分スキップします。このように使用することができます：

```bash
for (( i = 0; i < 10; i++ )); do
  if [[ $(( i % 2 )) -eq 0 ]]; then continue; fi
  echo $i
done
```

上記の例を実行すると、0から9までの数字のうち奇数であるものを全て表示します。

# 関数
スクリプトの中では、関数を定義したり呼び出したりすることができます。他のプログラミング言語と同じように、bashの関数はコードのまとまりですが、そこには違いがあります。

bashにおいて、関数は一連のコマンドをひとつの名前の下にまとめたもので、その名前が関数の _名前_ です。関数を呼ぶことは他のプログラムを呼ぶことと同じで、ただ関数の名前を書くだけで関数が起動します。

このようにして関数を宣言することができます：

```bash
my_func () {
  # 処理
}

my_func # 関数を呼び出す
```

関数は、必ず呼び出す前に宣言する必要があります。

関数は引数を受け取ることができ、結果として終了コードを返します。関数内における引数は、[非インタラクティブモード](#非インタラクティブモード)でスクリプトに渡された引数と同じように[位置パラメータ](#位置パラメータ)を使用して扱われます。終了コードは`return`コマンドを使用して返されます。

以下は 引数nameを受け取って`0`（正常な終了を示す）を返す関数です。

```bash
# function with params
greeting () {
  if [[ -n $1 ]]; then
    echo "Hello, $1!"
  else
    echo "Hello, unknown!"
  fi
  return 0
}

greeting Denys  # Hello, Denys!
greeting        # Hello, unknown!
```

私たちはすでに[終了コード](#終了コード)については学習しました。何も引数を取らない`return`コマンドは、最後に実行されたコマンドの終了コードを返します。上記の例の`return 0`は、正常な終了を示す終了コードである `0`を返します。

## デバッグ

シェルはデバッグのためのツールも提供します。スクリプトをデバッグモードで実行したい場合は、shebang（シバン：スクリプトの最初に書かれる #! の記述）のオプションを使用します：

```bash
#!/bin/bash options
```

これらのオプションはシェルの挙動を変更する設定です。以下の表はオプションのリストです。あなたの役に立つかもしれません：

| ショートオプション | 名前       | 説明                                            |
| :---: | :---------- | :----------------------------------------------------- |
| `-f`  | noglob      | ファイル名の展開(globbing)を無効にする                |
| `-i`  | interactive | スクリプトを _インタラクティブ_ モードで実行する                     |
| `-n`  | noexec      | コマンドを読み込むが、実行はしない(構文チェック用)  |
|       | pipefail    | （最後のコマンドが失敗した場合だけでなく）いずれかのコマンドが失敗したらパイプラインを失敗させる |
| `-t`  | —           | 最初のコマンドの直後に終了する                            |
| `-v`  | verbose     | 実行する前に`stderr`にそれぞれのコマンドを表示する    |
| `-x`  | xtrace      | 実行する前に`stderr`にそれぞれのコマンドと展開された引数を表示する|

例えば、`-x`オプションを使用したスクリプトはこのようになります：

```bash
#!/bin/bash -x

for (( i = 0; i < 3; i++ )); do
  echo $i
done
```

このスクリプトは変数の値を`stdout`に出力し、`stderr`に出力された有用なデバッグ情報と共に表示します。

```
$ ./my_script
+ (( i = 0 ))
+ (( i < 3 ))
+ echo 0
0
+ (( i++  ))
+ (( i < 3 ))
+ echo 1
1
+ (( i++  ))
+ (( i < 3 ))
+ echo 2
2
+ (( i++  ))
+ (( i < 3 ))
```

時には、スクリプトを部分的にデバッグしたい場合もあるでしょう。そんなときは、`set`コマンドを使うと便利です。このコマンドはオプションを有効にしたり無効にしたりすることができます。`-`で有効に、`+`で無効にできます：

```bash
#!/bin/bash

echo "xtrace is turned off"
set -x
echo "xtrace is enabled"
set +x
echo "xtrace is turned off again"
```

# ユーザー入力の読み取り

`read`コマンドを使用することで、ユーザーはシェル変数にデータを入力することができます。

## `read`

以下のコマンドは標準入力からの入力を受け取り、変数に格納します。

```bash
read [-ers] [-a array] [-d delim] [-i text] [-n nchars] [-N nchars]
     [-p prompt] [-t timeout] [-u fd] [variable1 ...] [variable2 ...]
```

もし何の変数名も指定されない場合、ユーザーからの入力はデフォルトで`$REPLY`という特殊な変数に格納されます。

```bash
#!/bin/bash

read          # ユーザーからの入力を待つ
echo $REPLY   # テキストを表示する
```

| ショートオプション | 名前        | 説明                                            |
| :---: | :---------- | :----------------------------------------------------- |
| `-a`  | array | 複数の単語を読み込み、[配列](#配列) `$array`に格納する |
| `-e` | | 区切り文字に到達するまで、ターミナルからのデータを一文字ずつ読み込む |
| `-d`  | delimiter | 任意の区切り文字を設定できる。<br>デフォルトでは、改行('\n')が区切り文字 |
| `-n` | nchars | **n** 個の文字を読み込むか、区切り文字に到達したら読み込みを停止する |
| `-N` | nchars | **n** 個の文字を読み込むか、EOFに達したら読み込みを停止する<br>**区切り文字を無視する。** |
| `-p` | prompt | コンソールにプロンプト文字を表示する |
| `-i` | interactive | プレースホルダーテキスト（ユーザーが編集できる初期値）を表示する。<br> `-e`オプション（行編集モード）と併せて使用される。 |
| `-r` | raw input | シェルによる$や*などの特殊文字の変換を無効にする |
| `-s` | silent | ターミナルから読み込んだ文字を表示しない（パスワードの入力時など）|
| `-t` | timeout | 一定の時間待ってから終了させる |
| `-u` | file descriptor | 指定されたファイルディスクリプタからの入力を読み込む |

```bash
#!/bin/bash
read -p 'Enter your name: ' name
echo Hello, "$name"!

read -sp 'Enter Password: ' password
if [ "$password" == "1234" ]; then # `[` の左右に空白が必要
  echo -e "\nSuccess!"
else
  echo -e "\nTry Again!"
fi
echo ""
read -p "Enter numbers: " -a array
echo ${array[2]}
```

このスクリプトは次のような出力を返します：

```
Enter your name: User1
Enter password:
Success #if password entered was 1234

Enter numbers: 100 200 300 400
300
```

# あとがき

この小さなハンドブックが面白く、役立つものであることを願っています。正直にいうと、私は自分のために、bashの基礎を忘れないようにこのハンドブックを書きました。なるべく正確かつ有意義になるよう努めたので、それを理解してくださると嬉しいです。

このハンドブックは私自身のbashの経験を書き連ねたもので、網羅的であるわけではありません。そのため、もしもっと学びたいなら、`man bash`を実行し、そこからスタートしてください。

コントリビューションは大歓迎です。どんな修正や質問にも感謝します。もし何かあれば[イシュー](https://github.com/denysdovhan/bash-handbook/issues)を作成してください。

このハンドブックを読んでくださりありがとうございます！

# もっと学びたい？

以下はBashを扱っている文献のリストです。

* Bashのmanページ。`man bash`コマンドを実行することで、ヘルプシステムである`man`がbashに関する情報を表示してくれます。bashを実行できる環境ならどんな環境でも見られます。`man`コマンドに関するさらなる情報は、[The Linux Information Project](http://www.linfo.org/)によってホストされているWebページ["The man Command"](http://www.linfo.org/man.html)を見てください。
* ["Bourne-Again SHell manual"](https://www.gnu.org/software/bash/manual/)。HTML, Info, TeX, PDF, and Texinfoを含む様々なフォーマットで閲覧可能です。<https://www.gnu.org/>によってホストされています。2016/01の時点では、2015/02/02にアップデートされたversion 4.3をカバーしています。
* ["Bash Shell Script"](http://www.gnu.org/software/bash/manual/bashref.html#Shell-Scripts)。bashを用いたスクリプトの書き方を学ぶことができます。<https://www.gnu.org/>によってホストされています。

# その他のリソース

* [awesome-bash](https://github.com/awesome-lists/awesome-bash)はbashスクリプトのキュレーションリストです。
* [awesome-shell](https://github.com/alebcay/awesome-shell)はshellリソースのキュレーションリストです。
* [bash-it](https://github.com/Bash-it/bash-it)は日々の業務の中でシェルスクリプトやカスタムコマンドを利用・開発し、メンテナンスするためのフレームワークを提供します。
* [Bash Guide for Beginners](http://tldp.org/LDP/Bash-Beginners-Guide/html/)は[HOWTO](http://tldp.org/HOWTO/Bash-Prog-Intro-HOWTO.html)と[Bash Scripting](http://tldp.org/LDP/abs/html/)の中間のような、良い情報源です。
* [dotfiles.github.io](http://dotfiles.github.io/)はbashやその他のシェルで利用可能な様々なdotfilesやシェルフレームワークへのリンクを集めた、非常に便利な情報源です。
* [learnyoubash](https://github.com/denysdovhan/learnyoubash)はあなたが初めてbashでスクリプトを書くのに役立つでしょう。
* [shellcheck](https://github.com/koalaman/shellcheck)はシェルスクリプトの静的解析ツールです。[www.shellcheck.net](http://www.shellcheck.net/)から使うか、コマンドラインで実行して使用することができます。インストールに関する手引書は[koalaman/shellcheck](https://github.com/koalaman/shellcheck)のgithubリポジトリページにあります。

最後に、Stack Overflowにはたくさんの[bashのタグがついた](https://stackoverflow.com/questions/tagged/bash)質問があります。そこから学ぶこともできますし、つまずいたときには質問してみるのもいいでしょう。

# ライセンス

[![CC 4.0][cc-image]][cc-url]

&copy; [Denys Dovhan](http://denysdovhan.com)

[cc-url]: http://creativecommons.org/licenses/by/4.0/
[cc-image]: https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg?style=flat-square

[npm-url]: https://npmjs.org/package/bash-handbook
[npm-image]: https://img.shields.io/npm/v/bash-handbook.svg?style=flat-square

[gitter-url]: https://gitter.im/denysdovhan/bash-handbook
[gitter-image]: https://img.shields.io/gitter/room/nwjs/nw.js.svg?style=flat-square