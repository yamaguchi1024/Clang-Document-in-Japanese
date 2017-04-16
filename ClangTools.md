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

