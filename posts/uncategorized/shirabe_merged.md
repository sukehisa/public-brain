
# ./20211017の調べ.md
---
title: 20211017の調べ
categories: []
date: 2021-10-17 14:44:23
updated:
tags:
---


## UNYSYS技報 
- No146 ハイブリッドクラウドの最新動向　を読んで
    - [ネットワークⅣ【日本ユニシス】](https://www.unisys.co.jp/tec_info/tr146/146abs.htm)
- IBM と Redhat
    - [IBMがRedHatを買った理由が今さらながらわかってきた OpenShiftとCloud Paks \- orangeitems’s diary](https://www.orangeitems.com/entry/2019/10/07/181325)
    - IBM製品は、ゆくゆくは、コンテナ化され、どの環境でも動くようになるんだろうな。そうすると、既存システムのクラウド化がようやくスムーズになるかも？
- VMware
    - SDDC (Software Defined Data Center)を各パブリッククラウドと連携して、hostedな private cloudを提供
- Nutanix
    - Invisible Infra がコンセプトの会社
        - ハイブリッドクラウド環境をone interfaceで管理する　など
    - HCI, private cloud, hybrid cloud

- パブリッククラウド　on オンプレの動き
    - AWS: Outposts
    - Azure Stack
    - セキュリティやネットワークレイテンシ厳しいアプリなどの理由で、結局はオンプレ環境を持たざるを得ない事情があるのだなぁ

- パブリッククラウドやら、プライベートクラウドやら、ハイブリッドクラウドやら色々あるが、インフラレイヤをさらに抽象化して、いく動き。システムの世界がもう一段、抽象化されてきた印象。アプリを作って、その下のレイヤは　「いい感じによろしく」という世界になっていくはず。
# ./20210912の調べ.md
---
title: 20210912の調べ
categories: []
date: 2021-09-12 10:24:23
updated:
tags:
---

## 分散トレーシング
- AWS X-Ray
    - [PowerPoint Presentation](https://d1.awsstatic.com/webinars/jp/pdf/services/20200526_BlackBelt_X-Ray.pdf)
    - AWS App Mesh　
        - 裏側はEnvoy. 設定で、X-Rayと連動できる
- Service Mesh
    - [サービスメッシュについて調査してみた件 \- Qiita](https://qiita.com/mamomamo/items/92085e0e508e18bc8532)
    - 分散システムの課題を解決する。Sidecar/Proxyで、ネットワーキングを制御
        - Service Discovery
        - Fault Tolerance
            - e.g. Circuit Breaker
        - Tracing, Observability
        - Security
            - Auth
        - アプリレベルでやっていた制御
            - リトライ
    - [20201014 AWS Black Belt Online Seminar AWS App Mesh Deep Dive](https://www.slideshare.net/AmazonWebServicesJapan/20201014-aws-black-belt-online-seminar-aws-app-mesh-deep-dive)


## SAM
- [AWS サーバーレスアプリケーションモデル \- アマゾン ウェブ サービス](https://aws.amazon.com/jp/serverless/sam/)
    - serverless Application Model
- serverless frameworkと完全に同じ位置づけだな・・
# ./20210731の調べ.md
---
title: 20210731の調べ
categories: []
date: 2021-07-31 16:50:37
updated:
tags:
---

## C4 model for visualizing SW arch
- [Visualising and documenting software architecture cheat sheets \- Coding the Architecture](http://www.codingthearchitecture.com/2017/04/27/visualising_and_documenting_software_architecture_cheat_sheets.html)

## Spinnaker
- [SpinnakerによるContinuous Delivery \| メルカリエンジニアリング](https://engineering.mercari.com/blog/entry/2017-08-21-092743/)
- Continuous Delivery用のplatform
    - blue/greenとか、CD用のpipeline作ったり（e.g. レビューを挟む、トリガーでキックする）といったことができる模様
# ./20210725の調べ.md
---
title: 20210725の調べ
categories: []
date: 2021-07-25 12:15:44
updated:
tags:
---


## Log4j 2  performance
- [Log4j – Performance](https://logging.apache.org/log4j/2.x/performance.html)

## Docker logging
- [Dockerコンテナのロギング機能を使ってみる \| さくらのナレッジ](https://knowledge.sakura.ad.jp/6752/)
    - 標準出力、エラーに出たログを、コンテナを稼働しているホストのファイルシステムに記録する
        - Docker デーモンにログが送られて記録される

## Docker Development Environments
[Development Environments Preview \| Docker Documentation](https://docs.docker.com/desktop/dev-environments/)
- 開発環境を配るのに良さそう。2021/07/25現在、Preview
- gitのproject指定→自動で、良さそうなイメージが選択されて、VS Codeが上がる　というのもできるし、
- 自前のdocker-compose.yamlを指定してもOK
    - DBとかも一緒に配る場合は、こっちだろうな。

- docker-composeの場合のサンプルを試してみる
    - 1 git project -> 3 docker containers
        - vs codeでアタッチすると、３コンテナでソースコードは共有されている？
            - 1つのコンテナでソースコードを修正すると、全部、その変更が伝搬される。
            - 同じソースコードが格納されている場所がマウントされている模様


## Docker internals
- [Understanding the Docker Internals \| by Nitin AGARWAL \| Medium](https://medium.com/@BeNitinAgarwal/understanding-the-docker-internals-7ccb052ce9fe)
    - Container Format = namespaces + cgroups + UnionFSの組み合わせ
        - デフォルトは、`libcontainer`
- [Docker Internals \-\- Docker Saigon](http://docker-saigon.github.io/post/Docker-Internals/#overview-of-linux-containers:cb6baf67dddd3a71c07abfd705dc7d4b)

## MacのDocker desktopの docker hostはどこ？
- [Let me explain Docker for Mac in a little more detail \[I work on this project at\.\.\. \| Hacker News](https://news.ycombinator.com/item?id=11352594)
- なるほど。昔は、VMにLinuxを立ち上げていたが、現在は、MacOS Xのnative アプリとして実装されている。
- WindowsはWSLの仕組みを使っている。
# ./20210703の調べ.md
---
title: 20210703の調べ
date: 2021-07-03 10:35:31
updated:
categories: [Learning]
tags:
---


<!-- more -->

## Lakehouse

- [Lakehouse: A New Generation of Open Platforms that Unify Data Warehousing and Advanced Analytics](http://cidrdb.org/cidr2021/papers/cidr2021_paper17.pdf)
- [データレイクとデータウェアハウスとは？それぞれの強み・弱みと次世代のデータ管理システム「データレイクハウス」を解説 \- Databricks ブログ](https://databricks.com/jp/blog/2020/01/30/what-is-a-data-lakehouse.html)


- Data Lakeだと、結局　BIなどの用途で、ETLかましたDWHができる問題あり。
- 分析、ML系のworkloadで、SQL越しではなく、効率的に直接DataFrameアクセスしたくなるというのは納得
    - Datawarehouseだときついところ（structured data store)
- Lakehouse
    - コンピュート　と　データの分離
        - DWHは一体ですからね。　分析系のworkloadは、コンピュートがバラけているから、扱いづらい

## Cloud Spanner と CAP

### Cloud Spannerとはなんだろう？
分散データベースだけどACIDが実現できるらしい　プロダクト？　RDB?
- グローバルに分散したデータベースである
- Googleのプロダクト。
    - Google Photosとか色々で使われている
    - もともと使っていたMySQLを代替。
- ConsistencyとScalabilityを両立
    - SQLのsyntaxで、トランザクションも提供可能
    - globalに、scalableにできるNoSQL的な利点も享受
- どういうときに使う？
    - 通常のRDBで、スケールの限界にぶつかったとき。
        - シャーディングやNoSQLが選択肢となるが、
        - SQLを使いたければ、Spannerが選択肢になってくる
- ref
    - [Googleがグローバルな分散データベースCloud Spannerをローンチ、SQLとNoSQLの‘良いとこ取り’を実装 \| TechCrunch Japan](https://jp.techcrunch.com/2017/02/15/20170214google-launches-cloud-spanner-a-new-globally-distributed-database-service/)
    - [Cloud Spanner \- YouTube](https://www.youtube.com/watch?v=amcf6W2Xv6M&t=2s)

- GCPのデータベースプロダクト
    - [Google Cloud データベース](https://cloud.google.com/products/databases)

### Cloud Spannerに対抗する、AWSやAzureのサービスはある？
- AWSの場合、AuroraとDynamo DBと思われる
    - Aurora
        - auto scale up and down, 
        - Spannerの持つ、multi-masterには勝てない？
            - これが必要なのは、ごく少数の超巨大グローバル企業のはずで、ほとんどのユースケースでは変わらないかも
    - Dynamo DB
        - auto scaling
        - multi region, multi-master
        - NoSQL

### Spannerの原理、仕組みは？
- Paxos, 原子時計。難しい
    - [夢のデータベース？　「Cloud Spanner」の実力は？：Mostly Harmless（2/2 ページ） \- ITmedia エンタープライズ](https://www.itmedia.co.jp/enterprise/articles/1706/29/news040_2.html)
    - Paxosは院生のとき流行って、論文読んだがよくわからなかった記憶がある。。

- CAP定理と関連して
    - 説明だけ見ると、C,A,P同時に満たしている夢のデータベースに見える
- [Cloud Spanner と CAP 定理 \| Google Cloud Blog](https://cloud.google.com/blog/ja/products/gcp/inside-cloud-spanner-and-the-cap-theorem)がわかりやすい

- 確かに。
```
CAP 定理の意図は本来、設計者にこのトレードオフを真剣に考えてもらうことにありました。ですが、ここでは 2 つの注意すべきことを挙げます。第 1 に、一貫性や可用性を犠牲にしなければならないのは、実際に分断が起こっている間だけで、その間であっても、それを緩和する方法は多くあるということ。第 2 に、定理には 100% の可用性とありますが、そこで議論すべきなのは、現実的な高可用性の実現に伴うトレードオフについてです。
```

- 基本、CAで、分断時はCPシステムである
```
はたして Spanner は CAP で定義されているような CA システムなのだろうかと。端的に答えると、技術的には違いますが、実際のところ CA システムだと考えて構いません。
分断が起きると Spanner は C を選択し、A を犠牲にします。つまり技術的に見ると、Spanner は CP システムなのです。
```

- Availabilityを犠牲にしているとはいえ、現実的には無視できるレベル　だからOKである。

- 結局仕組みはよくわからない。
    - Googleの管理するインフラ、NWの中でのみ使えるということ　がポイントなのだろう。
    - グローバルに一貫したタイムスタンプを付与する仕組みは？
        - 原子時計？

### CAP
- [CAP定理を見直す。“CAPの3つから2つを選ぶ”という説明はミスリーディングだった － Publickey](https://www.publickey1.jp/blog/13/capcap32.html)
- ACID, BASE, CAP
    - `一貫性対可用性の議論の最初のバージョンはACID対BASEというかたちで現れました`
    - `CAP定理の目的はより広く設計について探索する必要があることを示すことでした`

- そもそも、CPってどういうシステム？
    - Pがわからない。
        - P: the system continues to work in spite of `Network` failure
        - データベースに対する、cache (e.g. Redis)が該当するっぽい
    - Network failureを許容して、Consistensy　を維持ってどういうこと？
        - Network分断の時点で、Consistency保てないのでは？
    - PでRedisがいて、すべてのクライアントが同じデータを見れるから、CPなのかな。
    ![xxx](2021-07-03-15-17-58.png)
        - [CAP theorem with databases that “choose” CA, CP and AP \| Download Scientific Diagram](https://www.researchgate.net/figure/CAP-theorem-with-databases-that-choose-CA-CP-and-AP_fig1_282519669)より
- APは 
    - 他ノードへのConsistencyはおいておいて、アクセスできるノードにアクセスしてしまう。（それが、consistentかはおいておいて）→`A`の選択
    - `P`は？
- やっぱり、この2つ選ぶというのはミスリーディングである。

- [Please stop calling databases CP or AP — Martin Kleppmann’s blog](https://martin.kleppmann.com/2015/05/11/please-stop-calling-databases-cp-or-ap.
html)
    - Pは、犠牲にする・しない　という問題じゃない。インターネットは常にこの問題をはらむ
    
        > Partition Tolerance (terribly misnamed) basically means that you’re communicating over an asynchronous network that may delay or drop messages. The internet and all our data centres have this property, so you don’t really have any choice in this matter.

### Cloud Spanner使ってみる


# ./20210627の調べ.md
---
title: 20210627の調べ, dumb pipesとAPI GatewayのふりをしたESB
date: 2021-06-27 08:48:04
tags:
---


## Layered platform teams
- [Layered platform teams \| Technology Radar \| ThoughtWorks](https://www.thoughtworks.com/radar/techniques/layered-platform-teams)
    - Techniquesのセクションで、`HOLD`の位置づけとなっている。自分の感覚とも合うので、掘り下げてみる

このチームモデルは、レイヤでチームを分離するということ。

つまり、
- フロントエンドはフロントチーム
- ビジネスロジックは、ビジネスロジックチーム
- データは、データチーム。

現在、広く知られているのは、やっぱりConway's Lawということ。
つまり、チームの単位は、Business Capabilityの単位とすべきで、各チームはend-to-endで、別にTechnologyのレイヤを意識せずに、責任を持つべしということ。

これは実体験としても、同意である。
- チームが増えれば、チーム間のコミュニケーションが増える。依存関係ができて、ほかチームの完成を待つ　とか。
- 分離されてしまって、個々のチーム（特に、目に見えない部分）は、全体としての目的を見失う


## ESBs in API Gateway's clothing
- [ESBs in API Gateway's clothing \| Technology Radar \| ThoughtWorks](https://www.thoughtworks.com/radar/techniques/esbs-in-api-gateway-s-clothing)
    - Techniquesのセクションで、`HOLD`の位置づけ

ESBのアプローチに対する懸念あり。Microservice architectureとしては、`dumb pipes`にしたいのに、pipeにロジックが入り込んでしまうため、疎結合化できないし、ビジネスロジックを持ってしまうことで、ESBもメンテ対象になるし、ESB製品にロックされてしまう。たしかに。
> we're observing a pattern of traditional ESBs rebranding themselves, creating ESBs in API gateway's clothing that naturally encourage overambitious API gateways

もともとESBとしての製品が、たしかに API Gatewayという服着てるだけで、本質的にはESBであることに変わりはない。実体験とも合致する。
無駄にレイヤを増やして開発物が増えている印象。

> API gateways can still act as a useful abstraction for crosscutting concerns, but we believe the smarts should live in the APIs themselves.

これに尽きる。API gatewayは認証・認可、ロギング、セキュリティ系など、共通的なedge functionの役割を担うべきだと思う。


このAPI Gatewayのふりした、ESBが、[Overambitious API gateways \| Technology Radar \| ThoughtWorks](https://www.thoughtworks.com/radar/platforms/overambitious-api-gateways)と呼ばれている。

> any domain smarts should live in applications or services.

> concerned about business logic and process orchestration implemented in middleware, especially where it requires expert skills and tooling while creating single points of scaling and control. 

API Gateway `is essentially a reverse proxy`

このMagic QuadrantのFull Lifecycle API Managementの中にも、入っているはずで、製品ベンダの謳い文句にも疑いを持っていきましょうですね。
![](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/bf1133e7-a76d-458d-a09e-6b3376ab7d66.jpg "Magic QuadrantのFull Lifecycle API Management")


[Overambitious API gateways \- Kevin Sookocheff](https://sookocheff.com/post/api/overambitious-api-gateways/)の記事でも書いてあった。
確かに、多くのAPI Gateway製品は、多種多様な変換とか、aggregationというまさにロジックを差別化のポイントとして、売っている。


ESB的なAPI Gatewayは、heavyになってくると、開発のスピードを落とす懸念もある。
- API Gatewayは位置づけ的に、クライアントに対して、入り口なので、ここがデプロイされないと、アプリケーションが動かない。ボトルネックになり得るため。
- なので、API Gatewayの更新はできる限り、lightweightにする必要がある。　ビジネスロジックが入り込むと、開発・テスト、デプロイが必要になり、全体の足を引っ張る。


`API Composition`の役割をどこで持つかという問いが生まれる。API Gatewayは薄く、reverse proxy/routingに徹するとすると、バックのサービスとなるため、compositionするサービスを持つのが良いのかな。

関連のマイクロサービスアーキテクチャのqueryパターン
- [API composition](https://microservices.io/patterns/data/api-composition.html)
    - サービスに持たせるのもありだし、API Gatewayに持たせるのもあり。　後者について懸念があるということ
- [Command Query Responsibility Segregation \(CQRS\)](https://microservices.io/patterns/data/cqrs.html)

## Lehman's laws of software evolution
[Lehman's laws of software evolution \- Wikipedia](https://en.wikipedia.org/wiki/Lehman%27s_laws_of_software_evolution)

初めて知った。
# ./20210605の調べ.md
---
title: 20210605の調べ
date: 2021-06-05 21:43:37
tags:
---

## Anthos
- Problem, Context
    - 100%クラウド！とはなかなか行かず、クラウドとオンプレを上手く組み合わせる”Hybrid Cloud"を採用せざるを得ないケースが、現実問題として多い
    - Hybrid Cloudの目的
        - データの機密性に応じて使い分ける
        - 高負荷時、クラウドで負荷分散させる
        - など
    - しかし、Hybrid Cloudは当然、構成や運用が複雑になりがち

- Anthos as a Solution
    - オンプレ、クラウド　で統一的にコンテナ管理する仕組みを提供する
        - オンプレはAnthos GKEベースのKubernatesを動かす必要あり
        - クラウド
            - GCPだとGKE
            - AWSだと、Anthos Clusters for AWS
    - Service Mesh (Istio)

## 分散トランザクション
- これがわかりやすいね
    - [Distributed Transactions: The Icebergs of Microservices • Evolvable MeEvolvable Me](https://www.grahamlea.com/2016/08/distributed-transactions-microservices-icebergs/)

- データの整合性
    - 厳密な整合性 vs BASE（結果整合性）
    - 厳密な整合性
        - 2PC, XA(DBの機能で実現)
    - BASEなアプローチ
        - Message Broker
        - transaction発行元サービスがcoordinationする
            - TCC (Try/Confirm/Cancel)
            - State Machineを持つ
    - 補償トランザクション
    - データのリコンサイル

事例
- アリペイ
- メルペイ
    - [マイクロサービスにおける決済トランザクション管理 \| メルカリエンジニアリング](https://engineering.mercari.com/blog/entry/2019-06-07-155849/)
# ./20210101の調べ.md
---
title: 20210101の調べ
date: 2021-01-01 11:22:13
tags:
---

## Zero-trust architecture
- [Zero trust architecture \| Technology Radar \| ThoughtWorks](https://www.thoughtworks.com/radar/techniques/zero-trust-architecture)
# ./20201230の調べ.md
---
title: 20201230の調べ
date: 2020-12-30 08:53:10
tags:
---

## hotwire
- [HTML Over The Wire \| Hotwire](https://hotwire.dev/)
- フロントのJavascript処理をサーバサイドに寄せて、全体最適化するというような話？

- `Turbo Streams deliver page changes over WebSocket`が面白い

- WebSocket
    - [RFC 6455 \- The WebSocket Protocol](https://tools.ietf.org/html/rfc6455)
    - it's on TCP
    - The goal of
   this technology is to provide a mechanism for browser-based
   applications that need two-way communication with servers that does
   not rely on opening multiple HTTP connections

## Thought Works
- [Technology Radar \| An opinionated guide to technology frontiers \| ThoughtWorks](https://www.thoughtworks.com/radar#download-current-radar)

- Dependency drift fitness function　＋ Cost
    - 進化アーキテクチャの文脈における、fitness functionで、ちゃんと依存先のメンテしようという話
        - 実体験としても、mvn/gradle, npmで管理されるライブラリのバージョン追従は苦労する・・
    - 同様にCostも追従すべし

- Security policy as code
    - 具体的にはどんな製品？   
        - Open Policy Agent
            - `OPA for a unified toolset and framework for policy across the cloud native stack.`
        - Istio

- Tailored Service template
    - [Tailored service templates \| Technology Radar \| ThoughtWorks](https://www.thoughtworks.com/radar/techniques/tailored-service-templates)
    - 大企業だとわりとやっているイメージ
# ./20201219の調べ.md
---
title: 20201219の調べ
date: 2020-12-19 18:12:59
tags:
---

## MySQL関連
- [SELECT \.\.\. FOR UPDATE同士でデッドロックさせる \- かみぽわーる](https://blog.kamipo.net/entry/2020/12/15/213359)
    - 知らなかった。
    - 確かに、ロックを取るのにも時間がかかるから、SELECT FOR UPDATEでもデッドロックはあり得るな
    - FORCE INDEXで使用するindexを故意に別々にして、デッドロックを起こしている例

## Docker 関連
- [【Go言語】自作コンテナ沼。スクラッチでミニDockerを作ろう \- カミナシ開発者ブログ](https://kaminashi-developer.hatenablog.jp/entry/dive-into-swamp-container-scratch)
    - mini dockerを作ってみるというもの
    - Namespaces, Cgroupの仕組みが気になってきた


- Dockerのコア技術がLinux Kernelの技術だから、Docker imageのベースは全部Linuxのはずだよね？
    - Windows OSがベースのものはないはず？
        - [Windows base OS images \- Docker Hub](https://hub.docker.com/_/microsoft-windows-base-os-images)
        - これは何だ
        - でも`Windows requires the host OS version to match the container OS version`なので、WindowsのDocker実装があるということかな
            - [Windowsコンテナのリバース エンジニアリングから分かったこと](https://unit42.paloaltonetworks.jp/what-i-learned-from-reverse-engineering-windows-containers/)

- [Docker Internals \-\- Docker Saigon](http://docker-saigon.github.io/post/Docker-Internals/)
    - docker internalのまとめ
    - リソース（cpu, memory)の分離・制御
    - Networking
        - むずい、よくわからん
    - Security
        - it was omitted
    - Container Types
        - System Containers
        - App Containers 
        - Pods : hostとは分離しているが、サブグループとしてPIDとかネットワークなどを共有し得る
    - Images & Layers
        - UFS
        - ここのFSもいくつか種類があり、pro/conある
    - Container Format
        - containerd : by Docker
        - OCI
        - AppC by CoreOS : あったな。ここの標準化のところの競り合いだったのか
# ./20201213の調べ.md
---
title: 20201213の調べ
date: 2020-12-13 11:30:35
tags:
---

## Docker on M1関連
- [Apple Silicon M1 Chips and Docker \- Docker Blog](https://www.docker.com/blog/apple-silicon-m1-chips-and-docker/?utm_campaign=IT+Pro&utm_content=1605547520&utm_medium=social&utm_source=Organic)を見ると

    - Docker Desktop for Mac　がMacでのDocker 実行環境
        - 裏で、VMを動かし、その上でLinuxを動かしている
        - VMを動かすために、Appleのhypervisor frameworkを使用しており、その対応　がないとM1で動かせない
        - それ以外にも、細かいところで、goやElectronの対応などもある

    - [Hypervisor \| Apple Developer Documentation](https://developer.apple.com/documentation/hypervisor)
        - このM1対応が必要？
        - Intel Macの場合は、Intelの hardware-assistedなvirtualizationに依存しているはず。M1はどうなってるんだろう・・・
            - ARMの仮想化とは？

        - そもそも仮想化には`ホスト型`と`Hypervisor型`の方式がある
            - [ARMへの移行で変わるMacの「仮想化」 \- 新・OS X ハッキング\!\(269\) \| マイナビニュース](https://news.mynavi.jp/article/osxhack-269/)
            - ホスト型では、HWアクセスが、必ずホストOSを通過するので、オーバーヘッド大きい
            - 一方、Hypervisor型では、直接HWを制御ができる。これには、HWのアシストが必要で、Intelの場合はVT-X, EPTで、それに対するFWとして、MacではHypervisor.frameworkが存在する
                - つまり、x86-64に依存しているということ

        - [Docker Desktop 3\.0\.0: Smaller, Faster Releases \- Docker Blog](https://www.docker.com/blog/docker-desktop-3-0-0-smaller-faster-releases/)
            - Previewでは対応版が公開されているとのこと、早い
# ./20201212の調べ.md
---
title: 20201212の調べ
date: 2020-12-12 10:19:56
tags:
---

## Apple M1関連
- M1
	- ISA: ARM 64
	- 強さ
		- メモリ性能とIO性能が高い
	- M1で動くアプリ
		- ARM64 nativeアプリ
		- x86-64--Rosetta-->ARM64
		
	- Intelと比較
		- intelに最適化されてると勝てない
		
- Rosettaは何者？
	- x86-64 CISC -> ARM64のコンパイラ　not インタプリタ
	- M1がApple設計なので、OoO実行などCPUレベルの最適化もきっと入っているだろう
	
## Docker関連
- 参考
    - [Don't Panic: Kubernetes and Docker \| Kubernetes](https://kubernetes.io/blog/2020/12/02/dont-panic-kubernetes-and-docker/)
    - [Kubernetes 1\.20からDockerが非推奨になる理由 \- inductor's blog](https://blog.inductor.me/entry/2020/12/03/061329)
    - [KubernetesのDockershim廃止における開発者の対応 \- inductor's blog](https://blog.inductor.me/entry/2020/12/03/144834)

- Kubernetes - Container Runtime　のインタフェースをCRI (Container Runtime Interface)と呼ぶ
- DockerがCRIをサポートしていないので、`dockershim`というブリッジが Docker API-CRIの変換をしていたが、ここがdeprecateになる
- そもそも、Dockerの機能のうち、Kubernetesが使用するのはコアなランタイムのみ（containerd)
    - [containerd/containerd: An open and reliable container runtime](https://github.com/containerd/containerd/)
    - Dockerに内包されているが、DockerがCRIに対応していないので、`dockershim`が必要だった


- Kubernates
    - Red Hatのdistribution: OpenShift
        - ここで使われるのは、cri-o というランタイム

- Container Runtimeの構造
    - High Level Runtime(CRI Runtime)
        - e.g. containerd, cri-o
        - kuberlet とやりとりする
    - Low Level Runtime(OCI Runtime)
        - runC, gVisor
        - CRI Runtimeとやりとりする

- 影響まとめ
    - でも、Dockerにはcontainerdが内包されているので、互換性は高い
    - Docker-produced images will continue to work in your cluster with all runtimes, as they always have.
    - Docker is still a useful tool for building containers, and the images that result from running docker build can still run in your Kubernetes cluster.

    - The image that Docker produces isn’t really a Docker-specific image -- it’s an OCI (Open Container Initiative) image. 
    - Any OCI-compliant image, regardless of the tool you use to build it, will look the same to Kubernetes. 
    - Both containerd and CRI-O know how to pull those images and run them.

- コンテナの仕組みってそもそもなんだ・・
    - Linuxの`chroot`のすごいバージョンである
        - 特定のディレクトリをrootにして、それ以下のみの世界を作る技術
    - そのファイルシステムに対して、`namespace`でユーザやプロセス、NWを割り当て
    - `cgroups`でリソースの制御をかける
    - そして、VMのように扱える　という技術
- コンテナイメージ
    - DebianとかUbuntuが動く最低限のファイルシステムを含んだものをtarしたもの

- runCの仕組み
    - ホストOS・カーネルは、マシンごとに共通である
    - 特権が必要なので、runCに脆弱性があると、危険
- gVisorの仕組み
    - Linux上でプロセスとして動くゲストカーネル
    - gVisorの上で複数コンテナが動き、コンテナのシステムコールをptraceでフックし、gVisorが代理実行する

## Kotlin関連
- null safety関連の機能
- Javaからの移行はどうなるんだろう？
    - Kotlin自体もJVMで動く
    - Java-Kotlin間は相互運用可能（お互いにcallable)
    - Spring Initializerでも、Kotlinを既にサポートしている
    - 言語的に大きくJavaに対して優位性があれば切り替えられるが・・・
        - 実際には会社や世の中の開発者のスキルセット　とか
        - 既存アセット
        - に引っ張られて、すぐに変わる原動力にはならなそう。個人的には気になる

## Pen, offensive security
- Hack the box, VolnHub, TryHackMe というサービスがある
- VolnHub: 脆弱性ありのVMイメージ配布サイト
    - [Vulnerable By Design ~ VulnHub](https://www.vulnhub.com/)
    - 初学者向けではないな。練習用