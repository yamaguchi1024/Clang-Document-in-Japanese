### 概要
* Clang Toolsは早い構文検査や自動の書式設定等、clangを使っている開発者向けのCUIツールです。
* ClangのメインのSubversionに入っているのは限られた基本的なツールだけで、他のツールはサブプロジェクトで開発されているため、ツールが必要なければビルドする必要がありません。他のツールを使いたくなったら、SubversionやGitでいつもと同様にビルドすることができます：
    * Subversion
        ```
        cd llvm/tools/clang/tools
        svn co http://llvm.org/svn/llvm-project/clang-tools-extra/trunk extra
        ```
    * Git
        ```
        cd llvm/tools/clang/tools
        git clone http://llvm.org/git/clang-tools-extra.git extra
        ```
* このドキュメントではClang Toolsの仕組みについて抽象的に概観するとともに、いくつかの重要なツールについての導入をお行います。注意事項として、このドキュメントはClangとそのディベロッパーを対象にしており、ツールのエンドユーザーには焦点を当てていません。

### Clang Tools の全体像
* Clang ToolsはC++のデベロッパーに直接使われることを目的としたCLI,GUIのプログラムです。つまりこれらは元々ClangのデベロッパーのためのツールではなくClangを使うことになったC++開発者のために試験運用している機能です。
* Clang Toolsは３つの構成で出来ています。Clangベースのスタンドアローンなツールを作るためのインフラ、たくさんのツールの中でライブラリをリファクタリングや書き直すために使われているコアとなる論理、ツール自身です。

* Clang ToolsのためにインフラはLibTooling platformです。ドキュメントを見てこのインフラがどうやって動くのか詳細な情報を得てください。リファクタリングとリライティングのlibraryもLibTooling platformの一部です。

* 基本的な機能の例やテストケースとして、少数のClang ToolsはClang libraryのコアと一緒に開発されてきました。しかしながら、コアのライブラリとの分離のためほとんどのツールは他のレポジトリで開発されてきました。注意深くリビューをし、良いAPIを探しツールから掬い上げclangのコアなライブラリに入れるため、我々は意図的にサイドリポジトリではpublic librariesをサポートしていません。

* Clang Toolsのリポジトリがどこにあるかに関わらず、開発過程や実験はClang自身のものです。これらは全体としてClang projectです。

### 重要なClang Toolsの紹介
* 以下のClang Toolsの重要な機能は具体的にClangの補完をする機能で、Clang独自の機能を使ったりテストしたりすることができます。

#### clang-check
* ClangCheckはコマンドラインで高速にClang風の構文をチェックするLibToolingのフレームワークです。フラグをつけることによって、チェックした結果を様々な形式で表示することができ、これはIDEやエディタで使うのに役立ちます。さらに、fixit-hintsを直接適用するfixit-modeで使うことができます。clang-checkのセットアップと使い方についてはHow To Setup Clang Tooling For LLVMを参照してください。

#### clang-format
* Clang-formatはライブラリツールでもあり、スタンドアローンツールでもあるC++のソースファイルをclang風にフォーマットする機能です。ClangのLexerを用いて入力されたファイルをトークンに分割し、そのトークンの周囲にあるスペースを変更します。clang-formatはユーザーツールとして、またリファクタリングツールとして役立つことを目的としています。例えば、リネーム作業の途中にすべての行の形式を整えるなどです。

### その他のClang Tools
* 様々なタイプのClang Toolsがその他のリポジトリに入れられていて、これらはここで見つけられます。これらのツールのドキュメントは他のツールのデベロッパーのために作られていて、それぞれのツールがそれぞれのユーザーに焦点を当てたドキュメントを提供しています。

#### clang-tidy
* clang-tidyはclangをベースにしたC++の静的解析ツールです。バグが発生しやすいパターン、パフォーマンス、移植性、保守性を検出し改善するコンパイラをベースにした静的解析のフレームワークを提供します。

### 新しいツールのアイディア
* C++のcastを変換するツール。Cの((type) value)をC++の(static_cast,const_cast, または reinterpret_cast)に変換する。

* 非メンバ関数のbegin()とend()の変換ツール。fooが通常のコンテナだった場合にfoo.begin()をbegin(foo)に変換し、end()も同様に変換する。同じように、配列に対しても似たようなパターンを検出できる。

* tr1を削除するツール。TR1ライブラリを使っているソースコードをC++11ライブラリのコードに移植する。例えば、
```
#include <tr1/unordered_map>
int main()
{
        std::tr1::unordered_map <int, int> ma;
            std::cout << ma.size () << std::endl;
                return 0;
}
```
は以下のように書き換えられる。
```
#include <unordered_map>
int main()
{
        std::unordered_map <int, int> ma;
            std::cout << ma.size () << std::endl;
                return 0;
}
```

* autoを削除するツール。autoを明示的な型に変換するか、推論した型をコメントとして書き残す。これは、ソースコードの挙動がわからなくなるのを恐れてautoを使いたがらない開発者がいるからである。

* C++14: 曖昧でないoperatorの関数オブジェクト(N3421)。例えば
```
sort(v.begin(), v.end(), greater<ValueType>());
```
は以下のように書き換えられる。
```
sort(v.begin(), v.end(), greater<>());
```
