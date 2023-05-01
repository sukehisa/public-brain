---
title: Database Design and Implementation 2ndを読みつつ、Java関連を学んだ足跡
categories: []
date: 2022-04-29 17:04:34
updated:
tags:
---

# Chapter 2: JDBC
## Explicit Transaction Handling
- JDBCで、setAutoCommit(false)を明示的にすることでトランザクション境界を制御するが、Spring BootなどのFWでは、Controller In/Outでそのようになっているのか
    - `@Transactional` の宣言的アノテーションを使うと、そのメソッドがトランザクション境界となる

- この`@Transactional`　のようなアノテーションの仕組み（AOPの実装）は？
    - 実行時にclass fileを書き換える、作成する　といった黒魔術的な技術によって支えられている
        - [ASM](https://asm.ow2.io/)
            - byte code parser
            - `jacoco`などのCode coverage取得など、色々使われている
        - [Byte Buddy \- runtime code generation for the Java virtual machine](https://bytebuddy.net/#/)
    - Javaのclass fileはフォーマットがかっちりしているので、ごにょごにょできるわけだ
        - [Chapter 4\. The class File Format](https://docs.oracle.com/javase/specs/jvms/se8/html/jvms-4.html)
            - みると結構勉強になる

    - 黒魔術は危険な側面もあるが、便利な側面もある

        > However, this picture changes when creating libraries that need to interact with arbitrary code and unknown type hierarchies. In this context, a library implementer must often choose between either requiring a user to implement library-proprietary interfaces or to generate code at runtime when the user's type hierarchy becomes first known to the library.
        >
        > -- [Byte Buddy \- runtime code generation for the Java virtual machine](https://bytebuddy.net/#/)

        なるほど。インタフェースを実装してもらうか、ランタイムに情報を集めるしか無い。
        黒魔術によって、POJO化できる。
        が、ソースコードを追ってるとき、裏で何をやってるかわけわからん　みたいなことにはなる。

    - Spring Frameworkでは、`Proxy`という仕組みと呼ばれており、そのインスタンスが実際には実行され、周辺処理が挟み込みされる. 
    - AspectJ
    - 参考
        - [第7回 Springの宣言的トランザクションのしくみ【AOP】 \| DevelopersIO](https://dev.classmethod.jp/articles/declarative-trasaction-by-spring-aop/)

## Transaction Isolation Level
- デフォルトのisolation levelと、個別ロジックでの変更/Overrideの考え方。
    - Insert/Deleteだとレベル関係ない
    - ロジックによっては、低くて良いなど

## Prepared Statement
- parameter valuesが確定していなくてもコンパイルできるというところが味噌。
    - loopしたりして繰り返し使用される場面で効率が良い
- Q. DB engine側のクエリ処理は、parameter valuesが確定しない状態でコンパイルができる？
    - Query Treeを作るところまでは、同じ構造のクエリならば同じになるかどうか？
        - [The Internals of PostgreSQL : Chapter 3 Query Processing](https://www.interdb.jp/pg/pgsql03.html)

## Scrollable and Updatable Result Sets
- JDBCのResult setは基本的に、`forward-only` and `Non-updatable`であるが、`scrollable`, `updatable`に変更も可能
- Q. JDBCのResult Setの内部的な扱われ方はどうなっているのか。件数が多いときでも全量？もしくは、ある種のBatch/Paging的なことが起きてる？
    - デフォルトでは全量。設定で、`stream`することも可能 (fetch-sizeの指定)
        - [Chapter 5\. Issuing a Query and Processing the Result](https://jdbc.postgresql.org/documentation/head/query.html#fetchsize-example)
        - [MySQL :: MySQL Connector/J 8\.0 Developer Guide :: 6\.4 JDBC API Implementation Notes](https://dev.mysql.com/doc/connector-j/8.0/en/connector-j-reference-implementation-notes.html)

## Exercises
### 2.1 `Derby`は`autocommit` offを推奨しているが、なぜだと思う？
- [Avoid inserts in autocommit mode if possible](https://db.apache.org/derby/docs/10.7/tuning/ctunperf16800.html)
    - これのこと。
    - 答えは書いていたが
        - insertの度に発生するcommitのオーバーヘッド（e.g. 更新の確定によるディスク書き込み待ち、ロギング）
