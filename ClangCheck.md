### ClangCheck
ClangCheckはLibToolingの小さなラッパーで、基本的なエラーチェックとAST dumpができます。

```
$ cat <<EOF > snippet.cc
> void f() {
>   int a = 0
> }
> EOF
$ ~/clang/build/bin/clang-check snippet.cc -ast-dump --
Processing: /Users/danieljasper/clang/llvm/tools/clang/docs/snippet.cc.
/Users/danieljasper/clang/llvm/tools/clang/docs/snippet.cc:2:12: error: expected ';' at end of
      declaration
  int a = 0
           ^
           ;
(TranslationUnitDecl 0x7ff3a3029ed0 <<invalid sloc>>
  (TypedefDecl 0x7ff3a302a410 <<invalid sloc>> __int128_t '__int128')
  (TypedefDecl 0x7ff3a302a470 <<invalid sloc>> __uint128_t 'unsigned __int128')
  (TypedefDecl 0x7ff3a302a830 <<invalid sloc>> __builtin_va_list '__va_list_tag [1]')
  (FunctionDecl 0x7ff3a302a8d0 </Users/danieljasper/clang/llvm/tools/clang/docs/snippet.cc:1:1, line:3:1> f 'void (void)'
    (CompoundStmt 0x7ff3a302aa10 <line:1:10, line:3:1>
      (DeclStmt 0x7ff3a302a9f8 <line:2:3, line:3:1>
        (VarDecl 0x7ff3a302a980 <line:2:3, col:11> a 'int'
          (IntegerLiteral 0x7ff3a302a9d8 <col:11> 'int' 0))))))
1 error generated.
Error while processing snippet.cc.
```

* clang-checkがコンパイルデータベースを検索するのを防ぐために行末の'-'は必要です。 How To Setup Clang Tooling For LLVM にセットアップの情報とプロジェクトでのclang-checkの使い方が載っています。