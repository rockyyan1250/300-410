### QUESTION 1

- 問題文: Refer to the exhibit. Users in the branch network of 2001:db8:0:4::/64 report that they cannot access the Internet. Which command is issued in IPv6 router EIGRP 100 configuration mode to solve this issue?
（状況説明：展示を参照してください。支社ネットワークのユーザーがインターネットにアクセスできないと報告しています。この問題を解決するために、IPv6ルータのEIGRP 100設定モードで入力すべきコマンドはどれですか？）
- 選択肢:
A. Issue the eigrp stub command on R1.
B. Issue the no eigrp stub command on R1.
C. Issue the eigrp stub command on R2.
D. Issue the no eigrp stub command on R2.
- 正解: B
- セクション: Layer 3 Technologies
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢B）**:
            - R1が「スタブルータ（stub）」に設定されているため、R2から学習したインターネット宛てのルートが支社ルータへ中継されていません。
            - R1で `no eigrp stub` コマンドを実行してスタブ機能を無効化すれば、ルートが正常に伝播し問題が解決します 。[^1]
        - **不正解の理由**:
            - **選択肢A**: スタブ設定を有効にするコマンドであり、逆効果です。
            - **選択肢C, D**: 経路伝播を止めているボトルネックはR1であるため、R2側の設定を変更しても意味がありません。


### QUESTION 2

- 問題文: Refer to the exhibit. Which configuration configures a policy on R1 to forward any traffic that is sourced from the 192.168.130.0/24 network to R2?
（状況説明：展示を参照してください。送信元が192.168.130.0/24ネットワークであるすべてのトラフィックをR2に転送するように、R1でポリシーを構成する設定はどれですか？）
- 選択肢:
A. [Configuration A]
B. [Configuration B]
C. [Configuration C]
D. [Configuration D]
- 正解: D
- セクション: Layer 3 Technologies
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢D）**:
            - PBR（ポリシーベースルーティング）を機能させるには、トラフィックが**入ってくる（ingress）インターフェイス**（R1のg0/1）に適用する必要があります。
            - 転送先のネクストホップとして、R2の正しいIPアドレス（172.20.40.1）が `set ip next-hop` で指定されているため、Dが正解です 。[^1]
        - **不正解の理由**:
            - **他の選択肢**: PBRを出力インターフェイス（egress）に適用しようとしている、またはネクストホップとしてR1自身のアドレスや存在しないアドレスを指定しているため誤りです。


### QUESTION 3

- 問題文: R2 has a locally originated prefix 192.168.130.0/24 and has these configurations... What is the result when the route-map OUT command is applied toward an eBGP neighbor R1 1.1.1.1 by using the neighbor 1.1.1.1 route-map OUT out command?
（状況説明：R2はローカルで生成されたプレフィックス192.168.130.0/24を持っています。eBGPネイバーであるR1（1.1.1.1）に対して、OUT方向でroute-mapを適用した場合、どのような結果になりますか？）
- 選択肢:
A. R1 sees 192.168.130.0/24 as two AS hops away instead of one AS hop away.
B. R1 does not accept any routes other than 192.168.130.0/24
C. R1 does not forward traffic that is destined for 192.168.30.0/24
D. Network 192.168.130.0/24 is not allowed in the R1 table
- 正解: A
- セクション: Layer 3 Technologies
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢A）**:
            - 設定されたroute-mapは、自身のAS番号を意図的に余分に追加する「AS-PATH prepending」を行っています。
            - その結果、R1のBGPテーブル上ではこのルートのASパス長が増加し、通常（1ホップ）よりも遠い「2ホップ」離れたルートとして認識されます 。[^1]
        - **不正解の理由**:
            - **選択肢B**: R1側に特別なフィルタがない限り、他のピアからのルートは通常通り受け入れられます。
            - **選択肢C, D**: ASパスが長くなるだけでルート自体は有効なものとしてテーブルに登録・転送されるため、「拒否される」「許可されない」という記述は誤りです。


### QUESTION 4

- 問題文: Which method changes the forwarding decision that a router makes without first changing the routing table or influencing the IP data plane?
（状況説明：ルーティングテーブルを変更したり、IPデータプレーンに直接影響を与えたりすることなく、ルータの転送決定を変更する手法はどれですか？）
- 選択肢:
A. nonbroadcast multiaccess
B. packet switching
C. policy-based routing
D. forwarding information base
- 正解: C
- セクション: Layer 3 Technologies
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢C）**:
            - ポリシーベースルーティング（PBR）は、宛先IPベースの通常のルーティングテーブルの決定を**バイパス**する手法です。
            - テーブルそのものを書き換えず、送信元IPや特定のポートなどの条件に基づいてトラフィックを強制的に別のネクストホップへ曲げることができます 。[^1]
        - **不正解の理由**:
            - **選択肢A**: NBMAはFrame Relayなどのネットワークトポロジを指す用語です。
            - **選択肢B**: パケットスイッチングはルータの一般的な転送プロセスそのものです。
            - **選択肢D**: FIB（Forwarding Information Base）はデータプレーンの転送テーブルであり、PBRはこれを直接変更せずに優先されるため異なります。


### QUESTION 5

- 問題文: Refer to the exhibit. The output of the trace route from R5 shows a loop in the network. Which configuration prevents this loop?
（状況説明：展示を参照してください。R5からのtracerouteの出力により、ネットワーク内にループが発生していることがわかります。このループを防ぐ構成はどれですか？）
- 選択肢:
A. [Configuration A]
B. [Configuration B]
C. [Configuration C]
D. [Configuration D]
- 正解: A
- セクション: Layer 3 Technologies
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢A）**:
            - 2つのルータ（R3とR4）で相互再配布を行うと、ルートが元のプロトコルへ逆流してループが発生します。これを防ぐには「ルートタグ（Tag）」が有効です。
            - 正解の構成では、R3でルートに特定のタグを付与し、R4でそのタグを持つルートを `deny`（拒否）して逆流をブロックしています 。[^1]
        - **不正解の理由**:
            - **選択肢B**: タグ付けをしていないため、ループを防げません。
            - **選択肢C**: タグ付きルートを逆に許可し、正常なルートを暗黙の拒否で捨ててしまいます。
            - **選択肢D**: タグ付きルートを拒否しますが、末尾に `permit` がないため、正常なルートまで暗黙の拒否で全て破棄されてしまいます。


### QUESTION 6

- 問題文: Refer to the exhibit. An engineer configures a static route on a router, but when the engineer checks the route to the destination, a different next hop is chosen. What is the reason for this?
（状況説明：展示を参照してください。エンジニアがルータにスタティックルートを設定しましたが、宛先へのルートを確認すると別のネクストホップが選択されていました。この理由は何ですか？）
- 選択肢:
A. Dynamic routing protocols always have priority over static routes.
B. The metric of the OSPF route is lower than the metric of the static route.
C. The configured AD for the static route is higher than the AD of OSPF.
D. The syntax of the static route is not valid, so the route is not considered.
- 正解: C
- セクション: Layer 3 Technologies
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢C）**:
            - ルータは異なるプロトコル間で経路を選ぶ際、アドミニストレーティブディスタンス（AD）の値が低い方を優先します。
            - 展示のスタティックルートには末尾にAD「130」が手動設定されており、OSPFのデフォルトAD「110」よりも高いため、OSPFルートが優先して採用されています 。[^1]
        - **不正解の理由**:
            - **選択肢A**: 通常のスタティックルート（デフォルトAD=1）は動的プロトコルより優先されるため誤りです。
            - **選択肢B**: メトリック値は「同じプロトコル内」での比較に使われるものであり、プロトコルが異なる場合はADが比較されます。
            - **選択肢D**: 構文自体が無効であれば、ルータはコマンドを受け付けません。


### QUESTION 7

- 問題文: Refer to the exhibit. An engineer is trying to generate a summary route in OSPF for network 10.0.0.0/8, but the summary route does not show up in the routing table. Why is the summary route missing?
（状況説明：展示を参照してください。エンジニアがOSPFで10.0.0.0/8ネットワークの集約ルートを生成しようとしていますが、ルーティングテーブルに表示されません。集約ルートが欠落している理由は何ですか？）
- 選択肢:
A. The summary-address command is used only for summarizing prefixes between areas.
B. The summary route is visible only in the OSPF database, not in the routing table.
C. There is no route for a subnet inside 10.0.0.0/8, so the summary route is not generated.
D. The summary route is not visible on this router, but it is visible on other OSPF routers in the same area.
- 正解: C
- セクション: Layer 3 Technologies
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢C）**:
            - 集約ルートを生成するための絶対条件として、「集約範囲（10.0.0.0/8）に含まれる、より具体的な個別サブネットのルートが最低1つは存在している」必要があります。
            - 展示の状況では、再配布対象となる10.x.x.x系のルートが1つも存在しないため、まとめる対象がなく集約ルートは生成されません 。[^1]
        - **不正解の理由**:
            - **選択肢A**: エリア間のルート集約には `area range` コマンドを使用し、`summary-address` は外部ルート用です。
            - **選択肢B**: 集約ルートが生成されれば、ループ防止のため自身のルーティングテーブルにも（Null0宛てとして）表示されます。
            - **選択肢D**: 生成元のルータで作成されていない以上、他のルータに表示されることもありません。


### QUESTION 8

- 問題文: Refer to the exhibit. An engineer is trying to block the route to 192.168.2.2 from the routing table by using the configuration that is shown. The route is still present in the routing table as an OSPF route. Which action blocks the route?
（状況説明：展示を参照してください。表示された設定を使用してルーティングテーブルから192.168.2.2へのルートをブロックしようとしていますが、依然としてOSPFルートとして存在しています。このルートをブロックするアクションはどれですか？）
- 選択肢:
A. Use an extended access list instead of a standard access list.
B. Change sequence 10 in the route-map command from permit to deny.
C. Use a prefix list instead of an access list in the route map.
D. Add this statement to the route map: route-map RM-OSPF-DL deny 20.
- 正解: B
- セクション: Layer 3 Technologies
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢B）**:
            - ルートマップ内で対象ルートをブロックするには、アクセスリストでマッチした後のアクションを `deny` にする必要があります。
            - 現在の設定ではシーケンス10のアクションが `permit`（許可）になっているため、これを `deny` に変更しなければブロックできません 。[^1]
        - **不正解の理由**:
            - **選択肢A, C**: 条件指定の方法（拡張ACLやプレフィックスリスト）を変えるだけであり、最終アクションが `permit` のままでは遮断できません。
            - **選択肢D**: ルータは番号の若い順に評価を行うため、シーケンス10で既に `permit` されてしまうと、後続のシーケンス20が評価されることはありません。


### QUESTION 9

- 問題文: What is a prerequisite for configuring BFD?
（状況説明：BFD（Bidirectional Forwarding Detection）を設定するための前提条件は何ですか？）
- 選択肢:
A. Jumbo frame support must be configured on the router that is using BFD.
B. All routers in the path between two BFD endpoints must have BFD enabled.
C. Cisco Express Forwarding must be enabled on all participating BFD endpoints.
D. To use BFD with BGP, the timers 3 9 command must first be configured in the BGP routing process.
- 正解: C
- セクション: Layer 3 Technologies
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢C）**:
            - BFDはミリ秒単位の極めて高速な障害検知を行うため、ルータのCPUではなくハードウェアのデータプレーンに依存して動作します。
            - したがって、ハードウェア転送を司る中核機能である Cisco Express Forwarding（CEF）が、参加する全てのエンドポイントルータで有効になっていることが必須条件となります 。[^1]
        - **不正解の理由**:
            - **選択肢A**: ジャンボフレームのサポートはBFDの動作要件ではありません。
            - **選択肢B**: BFDは直接接続された2つのネイバー間で確立するものであり、経路上にある無関係な機器全てで有効にする必要はありません。
            - **選択肢D**: BGPのキープアライブ/ホールドタイマーを変更することは、BFDを動作させるための前提条件ではありません。


### QUESTION 10

- 問題文: Drag and drop the OSPF adjacency states from the left onto the correct descriptions on the right.
（状況説明：左側のOSPF隣接関係のステータスを、右側の正しい説明にドラッグアンドドロップしてください。）
- 選択肢:
*(Drag and Drop format)*
- 正解: (Select and Place)
- セクション: none
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由**: OSPFのネイバー関係確立までのステータス遷移（ステートマシン）の定義を正しく組み合わせる問題です 。[^1]
            - **Init**: 相手からのHelloパケットを受信したが、自分のRouter IDが含まれていない状態。
            - **2-Way**: 双方向通信が確認できた状態。ここでDR/BDRが選出される。
            - **ExStart**: マスター/スレーブ関係と初期シーケンス番号を決定する状態。
            - **Exchange**: LSAの要約（DBDパケット）を互いに交換する状態。
            - **Loading**: 不足している詳細情報（LSR/LSU）を要求・更新する状態。
            - **Full**: ルーティングデータベース（LSDB）が完全に同期された最終状態。
        - **不正解の理由**: ドラッグアンドドロップ問題のため特定の誤った選択肢はありませんが、上記フェーズの役割を取り違えてマッピングすると不正解となります。

### QUESTION 11

- 問題文: Refer to the exhibit. R2 is a route reflector, and R1 and R3 are route reflector clients. The route reflector learns the route to 172.16.25.0/24 from R1, but it does not advertise to R3. What is the reason the route is not advertised?
（状況説明：展示を参照してください。R2はルートリフレクタ（RR）であり、R1とR3はそのクライアントです。RRはR1から172.16.25.0/24のルートを学習しますが、R3にアドバタイズしていません。ルートがアドバタイズされない理由は何ですか？）
- 選択肢:
A. R2 does not have a route to the next hop, so R2 does not advertise the prefix to other clients.
B. Route reflector setup requires full IBGP mesh between the routers.
C. In route reflector setup, only classful prefixes are advertised to other clients.
D. In route reflector setups, prefixes are not advertised from one client to another.
- 正解: A
- セクション: Layer 3 Technologies
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢A）**:
            - BGPのルールとして、ルータは「ネクストホップへの到達性がないルート」をベストパス（最適経路）として選択しません。
            - ルートリフレクタ（R2）自身が該当プレフィックスをベストパスとして選択していないため、他のクライアント（R3）へはリフレクト（アドバタイズ）されません 。[^1]
        - **不正解の理由**:
            - **選択肢B**: ルートリフレクタは「iBGPのフルメッシュ要件を回避するため」の技術であるため、フルメッシュが必要という記述は誤りです。
            - **選択肢C**: BGPはクラスレスルーティングを完全にサポートしており、クラスフルに限定されることはありません。
            - **選択肢D**: クライアントから学習したルートを別のクライアントへアドバタイズすることこそが、ルートリフレクタの本来の役割です。


### QUESTION 12

- 問題文: Refer to the exhibit. An engineer is trying to redistribute OSPF to BGP, but not all of the routes are redistributed. What is the reason for this issue?
（状況説明：展示を参照してください。エンジニアがOSPFからBGPへのルート再配布を試みていますが、すべてのルートが再配布されているわけではありません。この問題の理由は何ですか？）
- 選択肢:
A. By default, only internal routes and external type 1 routes are redistributed into BGP
B. Only classful networks are redistributed from OSPF to BGP
C. BGP convergence is slow, so the route will eventually be present in the BGP table
D. By default, only internal OSPF routes are redistributed into BGP
- 正解: D
- セクション: Layer 3 Technologies
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢D）**:
            - Ciscoルータにおけるデフォルトの動作として、OSPFからBGPへルートを再配布（redistribute）する際、OSPFの「内部ルート（Internal：OおよびO IA）」のみが対象となります。
            - 外部ルート（External：E1/E2等）を含めるには、再配布コマンド内で `match internal external 1 external 2` のように明示的に指定する必要があります 。[^1]
        - **不正解の理由**:
            - **選択肢A**: 外部タイプ1（External Type 1）ルートはデフォルトでは再配布対象に含まれません。
            - **選択肢B**: 再配布はクラスフルネットワークに限定されません。
            - **選択肢C**: 収束速度の問題ではなく、設定上のデフォルト仕様による意図されたフィルタリング動作です。


### QUESTION 13

- 問題文: Refer to the exhibit. In which circumstance does the BGP neighbor remain in the idle condition?
（状況説明：展示を参照してください。どのような状況で、BGPネイバーはアイドル（idle）状態のままになりますか？）
- 選択肢:
A. if prefixes are not received from the BGP peer
B. if prefixes reach the maximum limit
C. if a prefix list is applied on the inbound direction
D. if prefixes exceed the maximum limit
- 正解: D
- セクション: Layer 3 Technologies
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢D）**:
            - BGPネイバー設定で `maximum-prefix` を設定している場合、ピアから受信したプレフィックス数が設定された上限を「超えた（exceed）」タイミングで、ルータはBGPセッションを強制切断します。
            - その結果、ネイバーステータスは「Idle (PfxCt)」などのアイドル状態へ移行し、手動クリアなどを行わない限り復旧しません 。[^1]
        - **不正解の理由**:
            - **選択肢A**: プレフィックスを受信しなくても、TCPセッションが確立していればEstablished状態になります。
            - **選択肢B**: 上限に「達した」だけ（あるいは設定した警告閾値に達した際）ではログが出力されるのみで、即座にセッションが切断されるわけではありません。上限を「超過」した際に切断されます。
            - **選択肢C**: プレフィックスリストの適用自体がセッションをアイドル状態にする原因ではありません。


### QUESTION 14

- 問題文: Which attribute eliminates LFAs that belong to protected paths in situations where links in a network are connected through a common fiber?
（状況説明：ネットワーク内のリンクが共通の光ファイバーを通じて接続されている状況において、保護対象パスと同じリスクに属するLFA（Loop-Free Alternate）を除外する属性はどれですか？）
- 選択肢:
A. shared risk link group-disjoint
B. linecard-disjoint
C. lowest-repair-path-metric
D. interface-disjoint
- 正解: A
- セクション: Layer 3 Technologies
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢A）**:
            - 物理的に同じ光ファイバー管を通っている複数のリンクは、ケーブル切断時に同時にダウンするリスクがあります。これを SRLG（Shared Risk Link Group）と呼びます。
            - IP FRR（Fast Reroute）のLFAにおいて「SRLG-disjoint（SRLGの分離）」属性を指定すると、主経路と同じリスクグループに属するバックアップ経路が選ばれないように除外することができます 。[^1]
        - **不正解の理由**:
            - **選択肢B, D**: ラインカードやインターフェイスの分離はルータ内部のハードウェア障害に備えるものであり、外部の共通ファイバー切断への直接的な対策ではありません。
            - **選択肢C**: メトリックが最も低い経路を選ぶ基準であり、物理的なリスクの分散（分離）を考慮するものではありません。


### QUESTION 15

- 問題文: Refer to the exhibit. An engineer is troubleshooting BGP on a device but discovers that the clock on the device does not correspond to the time stamp of the log entries. Which action ensures consistency between the two times?
（状況説明：展示を参照してください。エンジニアがデバイス上でBGPのトラブルシューティングを行っていますが、デバイスの時計（ローカル時刻）がログエントリのタイムスタンプと一致していないことに気づきました。2つの時刻の整合性を確保するアクションはどれですか？）
- 選択肢:
A. Configure the service timestamps log uptime command in global configuration mode.
B. Configure the logging clock synchronize command in global configuration mode.
C. Configure the service timestamps log datetime localtime command in global configuration mode.
D. Make sure that the clock on the device is synchronized with an NTP server.
- 正解: C
- セクション: Layer 3 Technologies
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢C）**:
            - デフォルトでは、ログのタイムスタンプは協定世界時（UTC）などで記録されることがあります。
            - グローバルコンフィギュレーションで `service timestamps log datetime localtime` を設定することで、ログ出力の時間をルータに設定されているローカルのタイムゾーン時刻と正確に一致させることができます 。[^1]
        - **不正解の理由**:
            - **選択肢A**: `uptime` を指定すると、ルータが起動してからの経過時間がログに記録されるため、実際の時計（カレンダー時刻）とは一致しなくなります。
            - **選択肢B**: このようなCisco IOSコマンドは存在しません。
            - **選択肢D**: NTPはデバイスの時計自体を正確にするためのものですが、ログ表示をローカル時刻にフォーマットするための設定ではありません。


### QUESTION 16

- 問題文: Refer to the exhibit. What is the result of applying this configuration?
（状況説明：展示を参照してください。この構成（クラスマップとACLに基づくトラフィック制御）を適用した結果はどうなりますか？）
- 選択肢:
A. The router can form BGP neighborships with any other device.
B. The router cannot form BGP neighborships with any other device.
C. The router cannot form BGP neighborships with any device that is matched by the access list named BGP.
D. The router can form BGP neighborships with any device that is matched by the access list named BGP.
- 正解: C
- セクション: Layer 3 Technologies
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢C）**:
            - 展示のポリシー設定では、「BGP」という名前のアクセスリスト（ACL）にマッチするトラフィックに対して `drop`（破棄）アクションが定義されています。
            - したがって、このACLで指定された特定のデバイスから送られてくるBGPパケットはルータによってドロップされるため、それらのデバイスとはBGPネイバー関係を確立できなくなります 。[^1]
        - **不正解の理由**:
            - **選択肢A, B**: すべてのデバイスを許可、あるいはすべてのデバイスを拒否する設定ではなく、ACLにマッチした対象だけが影響を受けます。
            - **選択肢D**: マッチしたトラフィックに対するアクションが `drop` であるため、ネイバーを「形成できる」という記述は逆であり誤りです。


### QUESTION 17

- 問題文: Which command displays the IP routing table information that is associated with VRF-Lite?
（状況説明：VRF-Liteに関連付けられたIPルーティングテーブルの情報を表示するコマンドはどれですか？）
- 選択肢:
A. show ip vrf
B. show ip route vrf
C. show run vrf
D. show ip protocols vrf
- 正解: B
- セクション: Layer 3 Technologies
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢B）**:
            - グローバルなルーティングテーブルではなく、特定のVRF（Virtual Routing and Forwarding）インスタンスに紐づくルーティングテーブルを確認するには、`show ip route vrf [VRF名]` コマンドを使用します 。[^1]
        - **不正解の理由**:
            - **選択肢A**: ルータ上に存在するVRFの一覧や、紐づくインターフェイスの概要を表示するためのコマンドです。
            - **選択肢C**: VRFに関連するランニングコンフィグ（設定情報）を表示するものであり、ルーティングテーブルは表示しません。
            - **選択肢D**: VRF上で稼働しているルーティングプロトコル（タイマーやAD値など）の状態を表示するコマンドです。


### QUESTION 18

- 問題文: Refer to the exhibit. Which subnet is redistributed from EIGRP to OSPF routing protocols?
（状況説明：展示を参照してください。EIGRPからOSPFルーティングプロトコルへ再配布されるサブネットはどれですか？）
- 選択肢:
A. 10.2.2.0/24
B. 10.1.4.0/26
C. 10.1.2.0/24
D. 10.2.3.0/26
- 正解: A
- セクション: Layer 3 Technologies
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢A）**:
            - 展示のルートマップ設定とプレフィックスリスト（`OSPF-TAG-PRF-1`）の許可条件を読み解く問題です。許可される条件は `10.2.0.0/18 le 24` となっています。
            - これは「最初の18ビットが 10.2.0.0 と一致（第3オクテットが0〜63の範囲）」かつ「プレフィックス長が /18 から /24 までの間」という意味です。選択肢Aの `10.2.2.0/24` はこの条件を完全に満たすため、再配布されます 。[^1]
        - **不正解の理由**:
            - **選択肢B, C**: ネットワークが `10.1.x.x` となっており、条件である `10.2.x.x` のネットワークアドレスに合致しないため除外されます。
            - **選択肢D**: ネットワーク範囲は合致していますが、プレフィックス長が `/26` であり、条件の `le 24`（/24以下）を超えているため拒否されます。


### QUESTION 19

- 問題文: Which configuration adds an IPv4 interface to an OSPFv3 process in OSPFv3 address family configuration?
（状況説明：OSPFv3アドレスファミリ構成において、OSPFv3プロセスにIPv4インターフェイスを追加する設定はどれですか？）
- 選択肢:
A. router ospfv3 1 address-family ipv4
B. Router(config-router)\#ospfv3 1 ipv4 area 0
C. Router(config-if)\#ospfv3 1 ipv4 area 0
D. router ospfv3 1 address-family ipv4 unicast
- 正解: C
- セクション: Layer 3 Technologies
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢C）**:
            - 従来のOSPFv2とは異なり、OSPFv3では `network` コマンドを使用せず、インターフェイス単位で直接OSPFプロセスを有効化します。
            - OSPFv3でIPv4を扱う場合、参加させたいインターフェイスの設定モード（`config-if`）に入り、`ospfv3 [プロセス番号] ipv4 area [エリア番号]` コマンドを入力するのが正しい手順です 。[^1]
        - **不正解の理由**:
            - **選択肢A, D**: OSPFv3のルーティングプロセスレベルでIPv4アドレスファミリ機能を有効化するコマンドであり、特定のインターフェイスをプロセスに参加させるものではありません。
            - **選択肢B**: ルータコンフィギュレーションモード（`config-router`）で入力されているため、コマンドの実行モードが誤っています。


### QUESTION 20

- 問題文: Refer to the exhibit. Which statement about R1 is true?
（状況説明：展示を参照してください。R1に関する記述のうち、正しいものはどれですか？）
- 選択肢:
A. OSPF redistributes RIP routes only if they have a tag of one.
B. RIP learned routes are distributed to OSPF with a tag value of one.
C. R1 adds one to the metric for RIP learned routes before redistributing to OSPF.
D. RIP routes are redistributed to OSPF without any changes.
- 正解: B
- セクション: Layer 3 Technologies
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢B）**:
            - 展示の設定（ルートマップなど）により、R1がRIPで学習したルートをOSPFへ再配布する際に、識別用の「ルートタグ」が付与されるよう構成されています。
            - その結果として、RIPからOSPFへ注入されるルートには、タグ値「1」が付与された状態で配信されます 。[^1]
        - **不正解の理由**:
            - **選択肢A**: タグ値が1であるルート「だけ」を再配布するという条件フィルタリングではなく、再配布した結果としてタグ1が付与されるアクションであるため誤りです。
            - **選択肢C**: 変更しているのはメトリック（コスト）ではなく、管理用マーカーである「タグ（Tag）」です。
            - **選択肢D**: タグの追加という明確な変更が加わっているため、「変更なし（without any changes）」という記述は誤りです。


### QUESTION 21

- 問題文: Refer to the exhibit. An IP SLA was configured on router R1 that allows the default route to be modified in the event that Fa0/0 loses reachability with the router R3 Fa0/0 interface. The route has changed to flow through router R2. Which debug command is used to troubleshoot this issue?
（状況説明：展示を参照してください。R1上のFa0/0がR3のFa0/0との到達性を失った場合、デフォルトルートを変更するようにIP SLAが設定されていました。ルートはR2を経由するように変更されました。この問題をトラブルシューティングするために使用するdebugコマンドはどれですか？）
- 選択肢:
A. debug ip flow
B. debug ip sla error
C. debug ip routing
D. debug ip packet
- 正解: B
- セクション: Layer 3 Technologies
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢B）**:
            - メインの経路がダウンし、IP SLAの追跡（トラッキング）が失敗した原因（例えば宛先からICMP応答がない、タイムアウトした等）を調査・特定するためには、`debug ip sla error` コマンドを使用します。これによりSLAの動作エラーの詳細が出力されます 。[^1]
        - **不正解の理由**:
            - **選択肢A, D**: トラフィックフロー全体やパケットそのものをキャプチャするデバッグであり、SLA固有のエラーの特定には適していません（ログが膨大になりシステムに負荷がかかります）。
            - **選択肢C**: ルーティングテーブルの「変更（ルートの追加・削除）」を監視するコマンドであり、SLAがダウンした「根本的なエラー原因」を教えてくれるものではありません。


### QUESTION 22

- 問題文: Which configuration enabled the VRF that is labeled Inet on FastEthernet0/0?
（状況説明：FastEthernet0/0 インターフェイスで "Inet" というラベルの付いたVRFを有効にした設定はどれですか？）
- 選択肢:
A. R1(config)\# ip vrf Inet
R1(config-vrf)\# ip vrf FastEthernet0/0
B. R1(config)\# ip vrf Inet FastEthernet0/0
C. R1(config)\# ip vrf Inet
R1(config-vrf)\# interface FastEthernet0/0
R1(config-if)\# ip vrf forwarding Inet
D. R1(config)\# router ospf 1 vrf Inet
R1(config-router)\# ip vrf forwarding FastEthernet0/0
- 正解: C
- セクション: Layer 3 Technologies
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢C）**:
            - 物理インターフェイスを特定のVRF（仮想ルーティングインスタンス）に所属させるための正しい手順です。
            - まずグローバル設定でVRFを作成（`ip vrf Inet`）し、その後対象のインターフェイスのコンフィギュレーションモードに入り、`ip vrf forwarding [VRF名]` コマンドを適用します 。[^1]
        - **不正解の理由**:
            - **選択肢A**: VRFコンフィギュレーションモード内でインターフェイスを指定する構文は存在しません。
            - **選択肢B**: グローバルコンフィギュレーションモードで1行にまとめてインターフェイスまで割り当てる構文はありません。
            - **選択肢D**: `ip vrf forwarding` はインターフェイス設定モードで入力するコマンドであり、OSPFルータ設定モード内で入力するものではありません。

### QUESTION 23

- 問題文: Refer to the exhibit. After redistribution is enabled between the routing protocols PC2, PC3, and PC4 cannot reach PC1. Which action can the engineer take to solve the issue so that all the PCs are reachable?
（状況説明：展示を参照してください。ルーティングプロトコル間でルート再配布を有効にした後、PC2、PC3、PC4がPC1に到達できなくなりました。すべてのPCが通信できるようにするために、エンジニアが取ることができるアクションはどれですか？）
- 選択肢:
A. Set the administrative distance 100 under the RIP process on R2.
B. Filter the prefix 10.1.1.0/24 when redistributed from OSPF to EIGRP.
C. Filter the prefix 10.1.1.0/24 when redistributed from RIP to EIGRP.
D. Redistribute the directly connected interfaces on R2.
- 正解: A
- セクション: Layer 3 Technologies
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢A）**:
            - 複数プロトコル間（RIP, EIGRP, OSPF）での双方向再配布により、アドミニストレーティブディスタンス（AD）に起因するルーティングループが発生しています。
            - ルータR2は、本来RIP経由（AD=120）でPC1宛てのルートを学習すべきですが、他のルータを経由してOSPFの外部ルート（AD=110）として同じルートを受信した場合、AD値が低いOSPFルートを優先してしまい経路が逆流します。
            - これを防ぐため、R2のRIPプロトコルのAD値を「100」に変更し、OSPF外部ルートよりも優先度を高く設定することで、正しい経路が維持されます 。[^1]
        - **不正解の理由**:
            - **選択肢B, C**: フィルタリングを行ってしまうと、他のPC（PC3やPC4）にPC1宛てのルート自体が伝わらなくなり、全PCからの到達性を確保するという要件を満たせません。
            - **選択肢D**: 直接接続されたインターフェイスを再配布しても、リモートからの動的ルートの優先度問題（ADの競合）は解決しません。


### QUESTION 24

- 問題文: Refer to the exhibit. A router is receiving BGP routing updates from multiple neighbors for routes in AS 690... What is the reason that the router still sends traffic that is destined to AS 690 to a neighbor other than 10.222.1.1?
（状況説明：展示を参照してください。あるルータが、AS 690内のルートについて複数のネイバーからBGPルーティングアップデートを受信しています。それにもかかわらず、AS 690宛てのトラフィックを 10.222.1.1 以外のネイバーへ送信し続けている理由は何ですか？）
- 選択肢:
A. The local preference value in another neighbor statement is higher than 250.
B. The local preference value should be set to the same value as the weight in the route map.
C. The route map is applied in the wrong direction.
D. The weight value in another neighbor statement is higher than 200.
- 正解: D
- セクション: Layer 3 Technologies
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢D）**:
            - CiscoルータのBGPベストパス選択アルゴリズムにおいて、最も優先されて最初に比較される属性は「Weight（ウェイト）」です。
            - 10.222.1.1 に対してルートマップでWeight「200」を設定していたとしても、別のネイバーに対して200より高いWeight値が設定されている場合、BGPは高いWeightを持つパスを無条件に最適経路として選択します 。[^1]
        - **不正解の理由**:
            - **選択肢A**: Local Preference（ローカルプリファレンス）はWeightの「次」に比較される属性であるため、Weightが優先されます。
            - **選択肢B**: Local PreferenceとWeightは全く異なる概念の属性であり、同じ値にする必要はありません。
            - **選択肢C**: 方向の間違いも考えられますが、BGPのパス選択基準として最も決定的な原因を突いているのはDです。


### QUESTION 25

- 問題文: Which command allows traffic to load-balance in an MPLS Layer 3 VPN configuration?
（状況説明：MPLS Layer 3 VPN構成において、トラフィックのロードバランシングを許可するコマンドはどれですか？）
- 選択肢:
A. multi-paths eibgp 2
B. maximum-paths 2
C. maximum-paths ibgp 2
D. multi-paths 2
- 正解: C
- セクション: VPN Technologies
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢C）**:
            - MPLS L3VPN環境において、PE（Provider Edge）ルータ同士は「MP-iBGP」を使用してVPNv4ルートを交換します。
            - BGPはデフォルトで1つのベストパスしかルーティングテーブルにインストールしませんが、iBGPで学習した複数のパスでロードバランシングを行うためには、BGPプロセス内で `maximum-paths ibgp [パス数]` コマンドを設定する必要があります 。[^1]
        - **不正解の理由**:
            - **選択肢A, D**: BGP設定において `multi-paths` という構文のコマンドは存在しません。
            - **選択肢B**: `ibgp` キーワードがない `maximum-paths` コマンドは、eBGPで学習した経路のロードバランシング用です。


### QUESTION 26

- 問題文: Refer to the exhibit... After applying IPsec, the engineer observed that the DMVPN tunnel went down, and both spoke-to-spoke and hub were not establishing. Which two actions resolve the issue? Choose two.
（状況説明：展示を参照してください。IPsecを適用した後、エンジニアはDMVPNトンネルがダウンし、スポーク間およびハブとの接続が確立されていないことを確認しました。この問題を解決する2つのアクションはどれですか？2つ選択してください。）
- 選択肢:
A. Change the mode from mode tunnel to mode transport on R3.
B. Remove the crypto isakmp key cisco address 10.1.1.1 on R2 and R3.
C. Configure the crypto isakmp key cisco address 192.1.1.1 on R2 and R3.
D. Configure the crypto isakmp key cisco address 0.0.0.0 on R2 and R3.
E. Change the mode from mode transport to mode tunnel on R2.
- 正解: A, D
- セクション: VPN Technologies
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢A, D）**:
            - **選択肢A**: DMVPNではGREトンネルを使用してパケットをカプセル化するため、IPsecはさらにヘッダを追加する「トンネルモード」ではなく「トランスポートモード」を使用するのが正しい設計です。これによりMTUのオーバーヘッドを節約します。
            - **選択肢D**: DMVPN環境ではスポークのIPアドレスが動的（DHCP等）に変わる可能性があるため、IPsecの事前共有鍵（ISAKMP key）の宛先アドレスには特定のIPではなく、すべてのIPを許可する `0.0.0.0` を設定する必要があります 。[^1]
        - **不正解の理由**:
            - **選択肢B**: 鍵そのものを削除してしまうと認証ができなくなり、IPsecが確立しません。
            - **選択肢C**: 特定の固定IPアドレスを指定すると、動的なIPを持つスポークデバイスや、他の多数のスポークとの間でトンネルを張ることができません。
            - **選択肢E**: 上述の通り、DMVPNではトランスポートモードが適しているため、トンネルモードに変更するのは誤りです。


### QUESTION 27

- 問題文: Which statement about route distinguishers in an MPLS network is true?
（状況説明：MPLSネットワークにおけるルート識別子（Route Distinguisher: RD）に関する記述のうち、正しいものはどれですか？）
- 選択肢:
A. Route distinguishers allow multiple instances of a routing table to coexist within the edge router.
B. Route distinguishers are used for label bindings.
C. Route distinguishers make a unique VPNv4 address across the MPLS network.
D. Route distinguishers define which prefixes are imported and exported on the edge router.
- 正解: C
- セクション: VPN Technologies
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢C）**:
            - 異なる顧客（VPN）が同じプライベートIPアドレス（例：10.0.0.0/24）を使用した場合、通常のBGPではアドレスの重複として扱われてしまいます。
            - これを防ぐため、PEルータは32ビットのIPv4アドレスの先頭に64ビットの「Route Distinguisher (RD)」を付与し、合計96ビットの「VPNv4アドレス」に変換します。これにより、MPLS網内で経路がグローバルに一意（ユニーク）になります 。[^1]
        - **不正解の理由**:
            - **選択肢A**: ルータ内に複数の仮想ルーティングテーブルを持たせる機能自体は「VRF (Virtual Routing and Forwarding)」です。
            - **選択肢B**: ラベルの割り当て（バインディング）を行うのは LDP (Label Distribution Protocol) や MP-BGP です。
            - **選択肢D**: どのルートをVRFにインポート・エクスポートするかを制御する役割は、「Route Target (RT)」が担います。


### QUESTION 28

- 問題文: Which statement about MPLS LDP router ID is true?
（状況説明：MPLS LDPルータIDに関する記述のうち、正しいものはどれですか？）
- 選択肢:
A. If not configured, the operational physical interface is chosen as the router ID even if a loopback is configured.
B. The loopback with the highest IP address is selected as the router ID.
C. The MPLS LDP router ID must match the IGP router ID.
D. The force keyword changes the router ID to the specified address without causing any impact.
- 正解: B
- セクション: VPN Technologies
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢B）**:
            - OSPF等のルーティングプロトコルと同様に、LDPのルータIDが手動で設定されていない場合、ルータは「設定されているアクティブなLoopbackインターフェイスの中で最も高いIPアドレス」を自動的にルータIDとして選択します 。[^1]
        - **不正解の理由**:
            - **選択肢A**: Loopbackインターフェイスが存在する場合は、物理インターフェイスよりもLoopbackが優先されます。
            - **選択肢C**: 運用上のベストプラクティスとしては一致させることが推奨されますが、システム要件として「必ず一致しなければならない」わけではありません。
            - **選択肢D**: `force` キーワードを使用してルータIDを強制的に変更すると、既存のLDPセッションが一度リセットされるため、通信断（インパクト）が発生します。


### QUESTION 29

- 問題文: Refer to the exhibit. Which interface configuration must be configured on the spoke A router to enable a dynamic DMVPN tunnel with the spoke B router?
（状況説明：展示を参照してください。スポークBルータとの間で動的DMVPNトンネルを有効にするために、スポークAルータのインターフェイスに設定しなければならない構成はどれですか？）
- 選択肢:
A. [Option A]
B. [Option B]
C. [Option C]
D. [Option D]
- 正解: B
- セクション: VPN Technologies
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢B）**:
            - DMVPN（Phase 2やPhase 3）において、スポーク同士が直接（動的に）トンネルを確立するためには、スポークのトンネルインターフェイスが「マルチポイントGRE（mGRE）」として構成されている必要があります。
            - したがって、`tunnel mode gre multipoint` が設定されており、かつ固定の `tunnel destination` が設定されていない（NHRPによって動的に解決される）コンフィグBが正解となります 。[^1]
        - **不正解の理由**:
            - **他の選択肢**: `tunnel mode gre multipoint` が欠落していたり、特定の対向先を固定で指定する `tunnel destination` コマンドが設定されている構成は、P2P（ポイントツーポイント）のGREトンネルになってしまうため誤りです。


### QUESTION 30

- 問題文: Which list defines the contents of an MPLS label?
（状況説明：MPLSラベルの内容（構造）を定義しているリストはどれですか？）
- 選択肢:
A. 20-bit label, 3-bit traffic class, 1-bit bottom stack, 8-bit TTL
B. 32-bit label, 3-bit traffic class, 1-bit bottom stack, 8-bit TTL
C. 20-bit label, 3-bit flow label, 1-bit bottom stack, 8-bit hop limit
D. 32-bit label, 3-bit flow label, 1-bit bottom stack, 8-bit hop limit
- 正解: A
- セクション: VPN Technologies
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢A）**:
            - MPLSラベルヘッダ（シムヘッダ）は合計32ビット（4バイト）で構成されており、その内訳は厳密に決まっています。
            - 実際の「ラベル値」が 20ビット、QoSなどに使用される「Traffic Class (EXP)」が 3ビット、スタックの最後を示す「Bottom of Stack (BoS)」が 1ビット、ループ防止のための「Time to Live (TTL)」が 8ビットです 。[^1]
        - **不正解の理由**:
            - **選択肢B, D**: ラベル値単体で32ビットあるとしているため誤りです（ヘッダ全体のサイズが32ビットです）。
            - **選択肢C**: "flow label" や "hop limit" という用語はIPv6ヘッダのフィールド名であり、MPLSラベルの構造を示す用語ではないため誤りです。

### QUESTION 31

- 問題文: Refer to the exhibit. What does the imp-null tag represent in the MPLS VPN cloud?
（状況説明：展示を参照してください。MPLS VPNクラウドにおける「imp-null」タグは何を表していますか？）
- 選択肢:
A. Pop the label
B. Impose the label
C. Include the EXP bit
D. Exclude the EXP bit
- 正解: A
- セクション: VPN Technologies
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢A）**:
            - 「imp-null（implicit-null）」は、MPLSにおけるラベル値「3」を意味します。これは Penultimate Hop Popping（PHP）という機能で使われます。
            - 宛先（出口）ルータの1つ手前のルータ（Penultimate Hop）に対し、「自分にパケットを送る際は、一番上のラベルを取り外して（Popして）から送ってくれ」と要求するための特殊なラベルです 。これにより出口ルータの処理負荷（ラベルとIPの2回ルックアップ）を軽減します。[^1]
        - **不正解の理由**:
            - **選択肢B**: ラベルを付与する（Impose/Push）アクションではなく、取り外すアクションを意味するため逆です。
            - **選択肢C, D**: EXPビット（Traffic Class：QoS用のビット）の含める/除外する処理を示すものではありません。


### QUESTION 32

- 問題文: Drag and drop the MPLS VPN concepts from the left onto the correct descriptions on the right.
（状況説明：左側のMPLS VPNの概念を、右側の正しい説明にドラッグアンドドロップしてください。）
- 選択肢:
*(Drag and Drop format)*
- 正解: (Select and Place)
- セクション: none
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由**: MPLS VPNを構成する各用語とその定義を正しく組み合わせる問題です 。[^1]
            - **RD (Route Distinguisher)**: 同じIPアドレスを持つ異なる顧客のプレフィックスを、MPLS網内で一意にするための64ビットの識別子。
            - **RT (Route Target)**: どのVRFに経路をインポート（またはエクスポート）するかを制御するためのBGP拡張コミュニティ属性。
            - **PE (Provider Edge)**: 顧客のルータ（CE）と直接接続し、MPLS網の入り口/出口となるプロバイダ側のルータ。
            - **CE (Customer Edge)**: PEルータと接続される、顧客サイト側のルータ。
        - **不正解の理由**: 用語の意味を逆にマッピングしてしまう（例えばRDとRTの役割を取り違えるなど）と不正解になります。


### QUESTION 33

- 問題文: Which transport layer protocol is used to form LDP sessions?
（状況説明：LDPセッションを形成するために使用されるトランスポート層プロトコルはどれですか？）
- 選択肢:
A. UDP
B. SCTP
C. TCP
D. RDP
- 正解: C
- セクション: VPN Technologies
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢C）**:
            - LDP（Label Distribution Protocol）の動作は2段階に分かれています。ネイバーの「探索（Helloパケット）」にはUDPのポート646を使用しますが、実際にラベルマッピング情報を確実に交換するための「セッションの確立（確立・維持）」には、信頼性の高い**TCP**（ポート646）が使用されます 。[^1]
        - **不正解の理由**:
            - **選択肢A**: UDPはネイバー探索（Hello）でのみ使用され、セッションの形成（データ交換）自体には使用されません。
            - **選択肢B, D**: SCTPやRDPはLDPの通信には使用されません。


### QUESTION 34

- 問題文: Drag and drop the MPLS terms from the left onto the correct definitions on the right.
（状況説明：左側のMPLSの用語を、右側の正しい定義にドラッグアンドドロップしてください。）
- 選択肢:
*(Drag and Drop format)*
- 正解: (Select and Place)
- セクション: none
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由**: MPLSネットワークの基本的なコンポーネントとその役割を一致させる問題です 。[^1]
            - **LSR (Label Switch Router)**: ラベルに基づいてパケットを転送（Swapなど）するMPLS網内のコアルータ。
            - **LER (Label Edge Router)**: MPLS網の境界に位置し、ラベルの付与（Push）や取り外し（Pop）を行うルータ。
            - **LSP (Label Switched Path)**: 送信元から宛先まで、ラベルスイッチングによってパケットが通過する一方向のパス（経路）。
            - **FEC (Forwarding Equivalence Class)**: 同じ転送処理（同じパス、同じ次ホップ、同じラベル）を受けるパケットの集合・グループ。
        - **不正解の理由**: 特にLSR（中継）とLER（境界）の役割を混同したり、LSP（経路）とFEC（グループ）の意味を取り違えると不正解となります。


### QUESTION 35

- 問題文: Refer to the exhibits. Phase-3 tunnels cannot be established between spoke-to-spoke in DMVPN. Which two commands are missing? Choose two.
（状況説明：展示を参照してください。DMVPNにおいて、スポーク間のPhase 3トンネルを確立できません。不足している2つのコマンドはどれですか？2つ選択してください。）
- 選択肢:
A. The ip nhrp redirect command is missing on the spoke routers.
B. The ip nhrp shortcut command is missing on the spoke routers.
C. The ip nhrp redirect command is missing on the hub router.
D. The ip nhrp shortcut command is missing on the hub router.
E. The ip nhrp map command is missing on the hub router.
- 正解: B, C
- セクション: VPN Technologies
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢B, C）**:
            - DMVPN Phase 3では、スポーク同士が直接通信するための動的トンネルを張る仕組みが採用されています。
            - これを機能させるには、トラフィックを中継した**ハブ（Hub）ルータ**が「直接通信した方が良い」とスポークに教えるための `ip nhrp redirect` コマンド（選択肢C）が必要です。
            - 同時に、リダイレクト指示を受け取った**スポーク（Spoke）ルータ**側で、その情報を使ってルーティングテーブルを動的に書き換え（ショートカット）るための `ip nhrp shortcut` コマンド（選択肢B）が必要です 。[^1]
        - **不正解の理由**:
            - **選択肢A, D**: リダイレクトを出すのはハブであり、ショートカットを受け入れるのはスポークです。これを逆に設定しているため誤りです。
            - **選択肢E**: ハブはスポークからの動的登録（Registration）を受け付けるため、静的な `ip nhrp map` は必須ではありません。


### QUESTION 36

- 問題文: Which protocol is used to determine the NBMA address on the other end of a tunnel when mGRE is used?
（状況説明：mGRE（マルチポイントGRE）が使用されている場合、トンネルの対向にあるNBMA（Non-Broadcast Multi-Access）アドレスを特定するために使用されるプロトコルはどれですか？）
- 選択肢:
A. NHRP
B. IPsec
C. MP-BGP
D. OSPF
- 正解: A
- セクション: VPN Technologies
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢A）**:
            - DMVPN環境などでmGREを使用する際、トンネルの「論理IPアドレス（例:10.0.0.x）」と、インターネットなどのアンダーレイネットワーク上の「物理IPアドレス（NBMAアドレス）」をマッピングして解決する必要があります。
            - このアドレス解決の役割を担うプロトコルが **NHRP (Next Hop Resolution Protocol)** です（ARPのWAN版のような働きをします） 。[^1]
        - **不正解の理由**:
            - **選択肢B**: IPsecはパケットの暗号化と認証（セキュリティ）を行うものであり、アドレス解決プロトコルではありません。
            - **選択肢C**: MP-BGPはVPNv4ルートの交換などに使われますが、mGREトンネルのエンドポイント解決には使われません。
            - **選択肢D**: OSPFはルーティングプロトコルであり、論理IP同士の経路情報交換には使われますが、物理IP（NBMA）の解決機能は持っていません。


### QUESTION 37

- 問題文: Refer to the exhibit. Which configuration denies Telnet traffic to router 2 from 198A:0:200C::1/64?
（状況説明：展示を参照してください。198A:0:200C::1/64 から ルータ2 への Telnet トラフィックを拒否する構成はどれですか？）
- 選択肢:
A. [Configuration A]
B. [Configuration B]
C. [Configuration C]
D. [Configuration D]
- 正解: A
- セクション: Infrastructure Security
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢A）**:
            - IPv6で特定のトラフィックをブロックする場合、`ipv6 access-list [名前]` コマンド内で条件を定義します。
            - TelnetはTCPプロトコルのポート23を使用するため、`deny tcp host 198A:0:200C::1 any eq telnet`（または `eq 23`）を記述している構成Aが正解となります 。[^1]
        - **不正解の理由**:
            - **他の選択肢**: 拒否するプロトコルがUDPになっていたり、ポート番号が誤っている（SSHの22など）、あるいはアクションが `permit` になっている設定は要件を満たしません。


### QUESTION 38

- 問題文: Refer to the exhibit. During troubleshooting it was discovered that the device is not reachable using a secure web browser. What is needed to fix the problem?
（状況説明：展示を参照してください。トラブルシューティング中、セキュアなウェブブラウザを使用してデバイスに到達できないことが発見されました。この問題を修正するには何が必要ですか？）
- 選択肢:
A. permit tcp port 443
B. permit udp port 465
C. permit tcp port 465
D. permit tcp port 22
- 正解: A
- セクション: Infrastructure Security
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢A）**:
            - 「セキュアなウェブブラウザ」からのアクセスとは、HTTPS（HTTP over SSL/TLS）通信を意味します。
            - HTTPSは標準で**TCPのポート443番**を使用するため、アクセスリスト（ACL）で `permit tcp any any eq 443` を追加許可することで通信が可能になります 。[^1]
        - **不正解の理由**:
            - **選択肢B, C**: ポート465は通常、セキュアなメール送信（SMTPS）に使用されるポートであり、ウェブアクセスとは無関係です。
            - **選択肢D**: ポート22はSSH用であり、セキュアなコマンドライン接続には使われますが、「ブラウザ経由」のアクセスを解決するものではありません。


### QUESTION 39

- 問題文: Drag and drop the packet types from the left onto the correct descriptions on the right.
（状況説明：左側のパケットタイプ（プレーン）を、右側の正しい説明にドラッグアンドドロップしてください。）
- 選択肢:
*(Drag and Drop format)*
- 正解: (Select and Place)
- セクション: none
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由**: ネットワークデバイスにおける通信プレーン（アーキテクチャ）の定義を対応させる問題です 。[^1]
            - **Data plane packets**: ユーザーが生成し、ネットワーク機器が単に宛先へ「転送（フォワーディング）」するだけのパケット（PC間のPingやWebアクセスなど）。
            - **Control plane packets**: ネットワーク機器自体が生成または受信し、ネットワークのトポロジ作成や動作のために使われるパケット（OSPFのHello、BGPのアップデートなど）。
            - **Management plane packets**: ネットワークの管理・監視目的で使われるパケット（Telnet、SSH、SNMPなど）。
            - **Services plane packets**: データプレーンの一部だが、QoS、暗号化、VPNなど機器のリソースを使って高度な処理（High-touch handling）を必要とするパケット。
        - **不正解の理由**: コントロールプレーン（制御）とマネジメントプレーン（管理）の役割を取り違えるなどのミスに注意が必要です。


### QUESTION 40

- 問題文: Drag and drop the addresses from the left onto the correct IPv6 filter purposes on the right.
（状況説明：左側のアドレスを、右側の正しいIPv6フィルタ目的にドラッグアンドドロップしてください。）
- 選択肢:
*(Drag and Drop format)*
- 正解: (Select and Place)
- セクション: none
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由**: 提供されたプレフィックスやワイルドカードマスク（IPv6のビット演算）から、フィルタリング対象となるIPアドレスの範囲を正確にマッピングする問題です 。[^1]
            - IPv6アドレスのネットワーク長（プレフィックス長）の表記に基づいて、ホスト部のビットがどの範囲で変動するかを計算します。
            - 例えば `/126` の場合は下位2ビットのみが変化するため、特定のアドレスから+3までの範囲（計4IP）が該当する目的となります。
        - **不正解の理由**: 16進数とプレフィックス長の計算（CIDRのIPv6版）を間違えると、適切な範囲のブロックにマッピングできず不正解となります。

### QUESTION 41

- 問題文: Refer to the exhibit. An engineer is trying to configure local authentication on the console line, but the device is trying to authenticate using TACACS. Which action produces the desired configuration?
（状況説明：展示を参照してください。エンジニアがコンソール回線（console line）でローカル認証を設定しようとしていますが、デバイスはTACACSを使用して認証しようとしています。目的の構成を実現するアクションはどれですか？）
- 選択肢:
A. Add the aaa authentication login default none command to the global configuration.
B. Replace the capital C with a lowercase c in the aaa authentication login Console local command.
C. Add the aaa authentication login default group tacacs local-case command to the global configuration.
D. Add the login authentication Console command to the line configuration
- 正解: D
- セクション: Infrastructure Security
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢D）**:
            - Ciscoルータでは、グローバルで `aaa authentication login default ...` を設定すると、コンソール（line con 0）やVTY回線はデフォルトでそのリスト（この場合はTACACS）を使用します。
            - コンソール回線専用に作成したカスタム認証リスト（例：`aaa authentication login Console local`）を適用するには、対象の回線設定モード（`line con 0`）に入り、`login authentication Console` コマンドで明示的に割り当てる必要があります 。[^1]
        - **不正解の理由**:
            - **選択肢A, C**: グローバルの `default` リストを変更すると、VTY（SSHやTelnet）など他の回線の認証方法まで変更されてしまい、コンソール「だけ」をローカルにするという要件から外れます。
            - **選択肢B**: リスト名の大文字・小文字は区別されますが、適用コマンド自体が抜けているため、リスト名を小文字に直しただけでは問題は解決しません。


### QUESTION 42

- 問題文: Refer to the exhibit. An engineer is trying to connect to a device with SSH but cannot connect. The engineer connects by using the console and finds the displayed output when troubleshooting. Which command must be used in configuration mode to enable SSH on the device?
（状況説明：展示を参照してください。エンジニアがSSHでデバイスに接続しようとしていますが接続できません。コンソールから接続してトラブルシューティングを行ったところ、表示された出力が確認されました。デバイスでSSHを有効にするために、設定モードで使用しなければならないコマンドはどれですか？）
- 選択肢:
A. no ip ssh disable
B. ip ssh enable
C. ip ssh version 2
D. crypto key generate rsa
- 正解: D
- セクション: Infrastructure Security
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢D）**:
            - Cisco IOSデバイスでSSHサーバー機能を有効にするための必須の前提条件は、ルータが「RSAキーペア」を保持していることです。
            - `crypto key generate rsa` コマンドを実行して暗号化キーを生成することで、バックグラウンドで自動的にSSH機能が有効化（Enabling SSH）されます 。[^1]
        - **不正解の理由**:
            - **選択肢A, B**: そのようなコマンド（`ip ssh enable` など）はCisco IOSに存在しません。
            - **選択肢C**: SSHバージョン2を指定するコマンドですが、そもそもRSAキーが存在しなければSSH自体が起動しないため、根本解決にはなりません。


### QUESTION 43

- 問題文: Which statement about IPv6 ND inspection is true?
（状況説明：IPv6 ND（Neighbor Discovery）インスペクションに関する記述のうち、正しいものはどれですか？）
- 選択肢:
A. It learns and secures bindings for stateless autoconfiguration addresses in Layer 3 neighbor tables.
B. It learns and secures bindings for stateless autoconfiguration addresses in Layer 2 neighbor tables.
C. It learns and secures bindings for stateful autoconfiguration addresses in Layer 3 neighbor tables.
D. It learns and secures bindings for stateful autoconfiguration addresses in Layer 2 neighbor tables.
- 正解: B
- セクション: Infrastructure Security
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢B）**:
            - IPv6 NDインスペクションは、レイヤ2のスイッチレベルで動作し、SLAAC（ステートレス自動設定）によって割り当てられたIPv6アドレスとMACアドレスの正当なマッピング（バインディングテーブル）を学習・保護するセキュリティ機能です。これによりIPv6のなりすまし（スプーフィング）を防ぎます 。[^1]
        - **不正解の理由**:
            - **選択肢A, C**: NDインスペクションはスイッチのポートレベル（Layer 2）でトラフィックを監視するため、Layer 3のネイバーテーブルで行うという記述は誤りです。
            - **選択肢D**: DHCPv6によるステートフル（Stateful）なアドレス割り当てではなく、NDプロトコルを用いたステートレス（Stateless）な環境向けの保護機能です。


### QUESTION 44

- 問題文: While troubleshooting connectivity issues to a router, these details are noticed:
    - Standard pings to all router interfaces, including loopbacks, are successful.
    - Data traffic is unaffected.
    - SNMP connectivity is intermittent.
    - SSH is either slow or disconnects frequently.
Which command must be configured first to troubleshoot this issue?
（状況説明：ルータの接続問題を調査中、次の詳細が確認されました。
・ループバックを含むすべてのインターフェイスへの標準Pingは成功。
・データトラフィックは影響を受けていない。
・SNMPの接続が断続的。
・SSHが遅い、または頻繁に切断される。
この問題をトラブルシューティングするために最初に実行すべきコマンドはどれですか？）
- 選択肢:
A. show policy-map control-plane
B. show policy-map
C. show interface inc drop
D. show ip route
- 正解: A
- セクション: Infrastructure Security
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢A）**:
            - データ転送（データプレーン）は正常であるにもかかわらず、SSHやSNMPなどルータのCPU宛ての管理トラフィック（マネジメントプレーン）だけが断続的・遅延している場合、Control Plane Policing (CoPP) によってトラフィックが制限（ドロップ）されている可能性が極めて高いです。
            - CoPPの適用状況やドロップのカウンタを確認するためには `show policy-map control-plane` コマンドを使用します 。[^1]
        - **不正解の理由**:
            - **選択肢B**: 通常のインターフェイスに適用されたQoSポリシーを表示するものであり、CPU宛てのトラフィックを保護するCoPPの確認には適していません。
            - **選択肢C, D**: インターフェイスのドロップやルーティングテーブルは、データトラフィックが正常であることから問題の原因ではありません。


### QUESTION 45

- 問題文: Refer to the exhibit. Why is user authentication being rejected?
(Logs: `TAC+: TCP/IP open to ... failed` / `AAA/AUTHEN/START: Method=LOCAL` / `status = FAIL`)
（状況説明：展示を参照してください。ユーザー認証が拒否されている理由は何ですか？）
- 選択肢:
A. The TACACS+ server expects "user", but the NT client sends "domain/user".
B. The TACACS+ server refuses the user because the user is set up for CHAP.
C. The TACACS+ server is down, and the user is in the local database.
D. The TACACS+ server is down, and the user is not in the local database.
- 正解: D
- セクション: Infrastructure Security
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢D）**:
            - ログの1行目 `TAC+: TCP/IP open ... failed` から、TACACS+サーバーへの接続に失敗しており、サーバーがダウンまたは到達不能であることがわかります。
            - 続いてフォールバックとしてルータのローカルデータベースによる認証（`Method=LOCAL`）が行われていますが、結果が `status = FAIL` となっているため、ローカルデータベースにも対象ユーザーが存在しない（またはパスワードが一致しない）ことが確認できます 。[^1]
        - **不正解の理由**:
            - **選択肢A, B**: サーバーとのTCP接続自体が失敗しているため、認証方式やユーザー名の形式が原因で拒否されたわけではありません。
            - **選択肢C**: ユーザーがローカルデータベースに正しく存在していれば `status = PASS`（認証成功）となるはずですが、FAILしているため誤りです。


### QUESTION 46

- 問題文: Refer to the exhibit. Which control plane policy limits BGP traffic that is destined to the CPU to 1 Mbps and ignores BGP traffic that is sent at higher rate?
（状況説明：展示を参照してください。CPU宛てのBGPトラフィックを1 Mbpsに制限し、それを超えるレートで送信されたBGPトラフィックを無視（ドロップ）するコントロールプレーンポリシーはどれですか？）
- 選択肢:
A. policy-map SHAPEBGP
B. policy-map LIMITBGP
C. policy-map POLICEBGP
D. policy-map COPP
- 正解: D
- セクション: Infrastructure Security
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢D）**:
            - CoPP（Control Plane Policing）において、「制限し、超えた分を無視（ドロップ）する」という要件は、QoSの「ポリシング（Police）」機能を使用して実装されます。
            - 指定されたドキュメントの解答群では、これら全体を包括するコントロールプレーンのポリシーとして設定された `policy-map COPP`（または展示内のクラス構成に合致するもの）が正解となります（※超過分をバッファリングする場合はShapingとなります） 。[^1]
        - **不正解の理由**:
            - **選択肢A**: Shaping（シェーピング）はパケットをドロップ（無視）するのではなく、キューにバッファリングして平滑化する技術であるため、要件に合いません。


### QUESTION 47

- 問題文: Which statement about IPv6 RA Guard is true?
（状況説明：IPv6 RA Guardに関する記述のうち、正しいものはどれですか？）
- 選択肢:
A. It does not offer protection in environments where IPv6 traffic is tunneled.
B. It cannot be configured on a switch port interface in the ingress direction.
C. Packets that are dropped by IPv6 RA Guard cannot be spanned.
D. It is not supported in hardware when TCAM is programmed.
- 正解: A
- セクション: Infrastructure Security
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢A）**:
            - IPv6 RA Guard機能は、スイッチ上で不正なルータ広告（Router Advertisement）を検査・ブロックするレイヤ2のセキュリティ機能です。
            - しかし、RAパケットがIPsecやGREなどでカプセル化（トンネリング）されている場合、スイッチはパケット内部のRAヘッダを読み取ることができないため、保護を提供できません 。[^1]
        - **不正解の理由**:
            - **選択肢B**: RA Guardはまさにスイッチポートの入力（ingress）方向に対して設定し、不正なパケットが入ってくるのを防ぐものです。
            - **選択肢C, D**: ハードウェアTCAMのプログラミングやSPAN（ポートミラーリング）機能と競合して動作しなくなるという制約はありません。


### QUESTION 48

- 問題文: An engineer is trying to copy an IOS file from one router to another router by using TFTP. Which two actions are needed to allow the file to copy? Choose two.
（状況説明：エンジニアがTFTPを使用して、あるルータから別のルータにIOSファイルをコピーしようとしています。ファイルのコピーを許可するために必要な2つのアクションはどれですか？2つ選択してください。）
- 選択肢:
A. Copy the file to the destination router with the copy tftp flash command
B. Enable the TFTP server on the source router with the tftp-server flash:filename command
C. TFTP is not supported in recent IOS versions, so an alternative method must be used
D. Configure a user on the source router with the username tftp password tftp command
E. Configure the TFTP authentication on the source router with the tftp-server authentication local command
- 正解: A, B
- セクション: Infrastructure Services
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢A, B）**:
            - Ciscoルータ間でファイルを直接転送する場合、片方をTFTPサーバーとして動作させます。
            - 送信元ルータにおいて `tftp-server flash:[ファイル名]` コマンドで対象ファイルの配信を有効化し（選択肢B）、宛先ルータ側で `copy tftp flash` コマンドを実行してファイルをダウンロードします（選択肢A） 。[^1]
        - **不正解の理由**:
            - **選択肢C**: TFTPは現在でもサポートされている一般的なプロトコルです。
            - **選択肢D, E**: TFTP（Trivial File Transfer Protocol）はその設計上、ユーザー名やパスワードによる認証メカニズムを一切持っていません。


### QUESTION 49

- 問題文: Refer to the exhibit. Users report that IP addresses cannot be acquired from the DHCP server. The DHCP server is configured as shown. About 300 total nonconcurrent users are using this DHCP server, but none of them are active for more than two hours per day. Which action fixes the issue within the current resources?
（状況説明：展示を参照してください。ユーザーから、DHCPサーバーからIPアドレスを取得できないとの報告がありました。合計約300人の非同時接続ユーザーがこのDHCPサーバーを使用していますが、1日に2時間以上アクティブになるユーザーはいません。現在のリソースの範囲内でこの問題を解決するアクションはどれですか？）
- 選択肢:
A. Modify the subnet mask to the network 192.168.1.0 255.255.254.0 command in the DHCP pool
B. Configure the DHCP lease time to a smaller value
C. Configure the DHCP lease time to a bigger value
D. Add the network 192.168.2.0 255.255.255.0 command to the DHCP pool
- 正解: B
- セクション: Infrastructure Services
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢B）**:
            - 現在のプールは `/24`（最大約254アドレス）であり、300人のユーザーに対してアドレスが枯渇しています。
            - しかし「非同時接続」かつ「最大2時間しか使用しない」という条件があるため、DHCPの貸し出し期間（リースタイム）を短く（例えば数時間程度に）設定することで、使用を終えたユーザーのIPアドレスが速やかにプールへ返却され、現在のサブネット範囲（リソース）のままで全員にアドレスを割り当てることが可能になります 。[^1]
        - **不正解の理由**:
            - **選択肢A, D**: サブネットマスクを拡張したり、新しいネットワークを追加したりすることは「現在のリソース（/24の範囲）のままで」という問題の要件に違反します。
            - **選択肢C**: リースタイムを長くすると、使用していないIPアドレスが解放されずに長時間占有され続けるため、枯渇問題がさらに悪化します。


### QUESTION 50

- 問題文: Refer to the exhibit. ISP 1 and ISP 2 directly connect to the Internet. A customer is tracking both ISP links to achieve redundancy and cannot see the Cisco IOS IP SLA tracking output on the router console. Which command is missing from the IP SLA configuration?
（状況説明：展示を参照してください。ISP 1とISP 2はインターネットに直接接続されています。顧客は冗長性を実現するために両方のISPリンクをトラッキングしていますが、ルータのコンソールにCisco IOS IP SLAのトラッキング出力が表示されません。IP SLAの設定で不足しているコマンドはどれですか？）
- 選択肢:
A. Start-time 00:00
B. Start-time 0
C. Start-time immediately
D. Start-time now
- 正解: D
- セクション: Infrastructure Services
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢D）**:
            - IP SLAは、トラッキングの監視条件（宛先IPやプロトコルなど）を設定しただけでは動作を開始しません。
            - 監視を有効にして開始させるためには、スケジュールを設定するコマンド `ip sla schedule [SLA番号] start-time now` を実行し、「今すぐ（now）」開始させる必要があります 。[^1]
        - **不正解の理由**:
            - **選択肢A, B**: 時間を指定するフォーマットとして誤りであったり、直ちに開始する意図を満たす標準的なコマンドではありません。
            - **選択肢C**: Cisco IOSのコマンド構文では「immediately」ではなく「now」キーワードを使用します。

### QUESTION 51

- 問題文: Refer to the exhibit. An administrator noticed that after a change was made on R1, the timestamps on the system logs did not match the clock. What is the reason for this error?
（状況説明：展示を参照してください。管理者は、R1で変更が行われた後、システムログのタイムスタンプが時計（システムクロック）と一致していないことに気づきました。このエラーの理由は何ですか？）
- 選択肢:
A. An authentication error with the NTP server results in an incorrect timestamp.
B. The keyword localtime is not defined on the timestamp service command.
C. The NTP server is in a different time zone.
D. The system clock is set incorrectly to summer-time hours.
- 正解: B
- セクション: Infrastructure Services
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢B）**:
            - Ciscoデバイスでは、ログのタイムスタンプを有効にするコマンド（`service timestamps log datetime`）を単体で入力すると、デフォルトでUTC（協定世界時）が記録されます。
            - システムクロックに設定された現地のタイムゾーン時刻（JSTなど）をログに反映させるには、コマンドの末尾に `localtime` キーワードを追加する必要があります 。[^1]
        - **不正解の理由**:
            - **選択肢A**: NTPの認証エラーが起きた場合、時刻同期自体が失敗しますが、すでに設定されているシステムクロックとログの「ズレ」を説明するものではありません。
            - **選択肢C, D**: NTPサーバーのタイムゾーンや夏時間が間違っていても、ルータ自体のシステムクロックには反映されます。システムクロックとログの時間が「一致しない」という事象の根本原因は、ログがローカルタイムを参照していないためです。


### QUESTION 52

- 問題文: Drag and drop the DHCP messages from the left onto the correct uses on the right.
（状況説明：左側のDHCPメッセージを、右側の正しい用途にドラッグアンドドロップしてください。）
- 選択肢:
*(Drag and Drop format)*
- 正解: (Select and Place)
- セクション: none
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由**: DHCPがIPアドレスを割り当てる際の一般的なプロセス（DORAプロセス）を理解する問題です 。[^1]
            - **DHCPDISCOVER**: クライアントがネットワーク上で利用可能なDHCPサーバーを探すためにブロードキャストするメッセージ。
            - **DHCPOFFER**: サーバーがクライアントからの要求を受け、提供可能なIPアドレスなどの設定情報を提案（オファー）するメッセージ。
            - **DHCPREQUEST**: クライアントが、オファーされたIPアドレスの割り当てを正式に要求する（またはリース更新を要求する）メッセージ。
            - **DHCPACK**: サーバーが割り当てを確定し、構成情報をクライアントに最終確認として送信するメッセージ。
        - **不正解の理由**: クライアントとサーバーのどちらがメッセージを発信するか、通信の順序を取り違えると不正解となります。


### QUESTION 53

- 問題文: A network engineer is investigating a flapping up/down interface issue on a core switch that is synchronized to an NTP server. Log output currently does not show the time of the flap. Which command allows the logging on the switch to show the time of the flap according to the clock on the device?
（状況説明：ネットワークエンジニアが、NTPサーバーと同期しているコアスイッチでインターフェイスがアップ/ダウンを繰り返す（フラッピング）問題を調査しています。現在、ログ出力にはフラップの時刻が表示されていません。スイッチのログにデバイスの時計に基づいた時刻を表示させるコマンドはどれですか？）
- 選択肢:
A. service timestamps log uptime
B. clock summer-time mst recurring 2 Sunday mar 2:00 1 Sunday nov 2:00
C. service timestamps log datetime localtime show-timezone
D. clock calendar-valid
- 正解: C
- セクション: Infrastructure Services
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢C）**:
            - ログメッセージに「日付と時刻」を表示するには `service timestamps log datetime` コマンドを使用します。
            - 障害発生時の正確な日時を特定するためには、デバイスの現地時刻とタイムゾーンを含めるのがベストプラクティスです。したがって `localtime show-timezone` オプションを付与する選択肢Cが正解となります 。[^1]
        - **不正解の理由**:
            - **選択肢A**: `uptime` はルータが起動してからの「経過時間」を記録するものであり、実際のカレンダー時刻は表示されません。
            - **選択肢B, D**: これらは時計自体（夏時間やハードウェアカレンダー）を設定するコマンドであり、ログの出力フォーマットを変更するものではありません。


### QUESTION 54

- 問題文: When provisioning a device in Cisco DNA Center, the engineer sees the error message "Cannot select the device. Not compatible with template." What is the reason for the error?
（状況説明：Cisco DNA Centerでデバイスをプロビジョニングする際、エンジニアに「デバイスを選択できません。テンプレートと互換性がありません。」というエラーメッセージが表示されます。このエラーの理由は何ですか？）
- 選択肢:
A. The template has an incorrect configuration.
B. The software version of the template is different from the software version of the device.
C. The changes to the template were not committed.
D. The tag that was used to filter the templates does not match the device tag.
- 正解: D
- セクション: Infrastructure Services
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢D）**:
            - Cisco DNA Centerでは、テンプレート（設定ファイルの雛形）を特定のデバイスやネットワークプロファイルに適用する際、「ネットワークタグ（Network Tag）」を使用して対象をフィルタリング・関連付けします。
            - テンプレートに割り当てられているタグと、プロビジョニングしようとしているデバイスに割り当てられているタグが一致しない場合、互換性がないとして選択できなくなります 。[^1]
        - **不正解の理由**:
            - **選択肢A, C**: テンプレート内部のコンフィグミスや未コミット状態は別のエラーを引き起こしますが、デバイス自体が「選択できない」直接の要因ではありません。
            - **選択肢B**: テンプレートはOSバージョンに依存しない汎用的なテキストや変数の塊として作られることが多いため、バージョンの違いが直接このエラーを生むわけではありません。


### QUESTION 55

- 問題文: While working with software images, an engineer observes that Cisco DNA Center cannot upload its software image directly from the device. Why is the image not uploading?
（状況説明：ソフトウェア・イメージの作業中、エンジニアはCisco DNA Centerがデバイスから直接ソフトウェア・イメージをアップロード（吸い上げ）できないことを確認しました。イメージがアップロードされない理由は何ですか？）
- 選択肢:
A. The device must be resynced to Cisco DNA Center.
B. The software image for the device is in install mode.
C. The device has lost connectivity to Cisco DNA Center.
D. The software image for the device is in bundle mode.
- 正解: B
- セクション: Infrastructure Services
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢B）**:
            - Cisco IOS XEデバイスの起動モードには「Bundleモード（1つの.binファイルをメモリ上に展開して起動）」と「Installモード（.binファイルをフラッシュメモリ上に複数のパッケージとして展開・インストールして起動）」の2種類があります。
            - Installモードで動作している場合、イメージはすでに複数のパッケージに分割されているため、DNA Centerが単一のソースイメージファイルとしてデバイスから直接吸い上げ（抽出）ることはできません 。[^1]
        - **不正解の理由**:
            - **選択肢A, C**: デバイスの同期遅れや接続切れは別の通信エラーを引き起こしますが、「イメージ構成上の理由」ではありません。
            - **選択肢D**: Bundleモードであれば元の単一の.binファイルが存在するため、理論上はアップロードが可能です。


### QUESTION 56

- 問題文: An engineer configured the wrong default gateway for the Cisco DNA Center enterprise interface during the install. Which command must the engineer run to correct the configuration?
（状況説明：エンジニアがインストール中に、Cisco DNA Centerのエンタープライズ・インターフェイスに誤ったデフォルトゲートウェイを設定してしまいました。この設定を修正するために実行しなければならないコマンドはどれですか？）
- 選択肢:
A. sudo maglev-config update
B. sudo maglev install config update
C. sudo maglev reinstall
D. sudo update config install
- 正解: A
- セクション: Infrastructure Services
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢A）**:
            - Cisco DNA Centerの基盤となるOS・アーキテクチャは「Maglev（マグレブ）」と呼ばれています。
            - インストール後にアプライアンスのネットワーク設定（IPアドレスやゲートウェイなど）をコマンドラインから変更・再構成する場合は、`sudo maglev-config update` コマンドを使用する仕様となっています 。[^1]
        - **不正解の理由**:
            - **選択肢B, C, D**: これらは存在しない架空のコマンド、または全体を再インストールするための誤った構文です。


### QUESTION 57

- 問題文: Refer to the exhibit. An administrator that is connected to the console does not see debug messages when remote users log in... Which action ensures that debug messages are displayed for remote logins?
（状況説明：展示を参照してください。コンソールに接続している管理者が、リモートユーザーがログインした際にデバッグメッセージを確認できません。リモートログインに関するデバッグメッセージが表示されるようにするアクションはどれですか？）
- 選択肢:
A. Enter the transport input ssh configuration command.
B. Enter the terminal monitor exec command.
C. Enter the logging console debugging configuration command.
D. Enter the aaa new-model configuration command.
- 正解: D
- セクション: Infrastructure Services
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢D）**:
            - `debug aaa authentication` を有効にしてユーザーのログイン状況を監視したい場合、ルータでAAA（認証・認可・アカウンティング）機能自体がグローバルで有効化されている必要があります。
            - AAAを有効にするためには `aaa new-model` コマンドを設定する必要があります。これを入れなければ、AAAのデバッグログは生成されません 。[^1]
        - **不正解の理由**:
            - **選択肢A**: SSHアクセスを許可するコマンドであり、デバッグログの出力有無とは無関係です。
            - **選択肢B**: `terminal monitor` はVTY（SSH/Telnet）経由で接続している管理者がログを見るために必要ですが、問題文には「管理者がコンソール（console）に接続している」とあるため、デフォルトでログは表示されるはずであり無関係です。
            - **選択肢C**: コンソールにはデフォルトでデバッグログが出力される設定になっているため、AAA自体が起動していないことが根本原因です。


### QUESTION 58

- 問題文: Refer to the exhibit. Network operations cannot read or write any configuration on the device with this configuration from the operations subnet. Which two configurations fix the issue? Choose two.
（状況説明：展示を参照してください。ネットワーク運用部門は、この構成のデバイスに対して、運用サブネットからの読み取り/書き込み（設定）が一切できません。この問題を修正する2つの構成はどれですか？2つ選択してください。）
- 選択肢:
A. Configure SNMP rw permission in addition to community ciscotest.
B. Modify access list 1 and allow operations subnet in the access list.
C. Modify access list 1 and allow SNMP in the access list.
D. Configure SNMP rw permission in addition to version 1.
E. Configure SNMP rw permission in addition to community ciscotest 1.
- 正解: B, E
- セクション: Infrastructure Services
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢B, E）**:
            - デバイスのSNMP設定が読み取り専用（RO）である、あるいはACLが設定変更用のアクセスを許可していないことが原因です。
            - 書き込みを行うには、SNMPコミュニティに読み書き権限（Read-Write）を付与する必要があります（`snmp-server community ciscotest rw 1` などのように、コミュニティ名とACL番号「1」の間に「rw」を追加します）（選択肢E）。
            - さらに、アクセスリスト1（ACL 1）に、運用サブネットからのアクセスを許可する `permit` ルールを追加する必要があります（選択肢B） 。[^1]
        - **不正解の理由**:
            - **選択肢A**: ACL（番号1）の適用が抜けてしまうため、セキュリティが不完全または要件を満たしません。
            - **選択肢C**: 標準ACL（1〜99）は送信元IPのみをチェックするため、「SNMPを許可する」というプロトコルレベルの指定はできません。
            - **選択肢D**: version 1への言及は不要です。


### QUESTION 59

- 問題文: Refer to the exhibit. Why is the remote NetFlow server failing to receive the NetFlow data?
（状況説明：展示を参照してください。リモートのNetFlowサーバーがNetFlowデータを受信できない理由は何ですか？）
- 選択肢:
A. The flow exporter is configured but is not used.
B. The flow monitor is applied in the wrong direction.
C. The flow monitor is applied to the wrong interface.
D. The destination of the flow exporter is not reachable...
- 正解: A
- セクション: Infrastructure Services
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢A）**:
            - Cisco Flexible NetFlowを構成する際は、パケット形式を定義する「Record」、送信先を指定する「Exporter」、そしてそれらを束ねてインターフェイスに適用する「Monitor」の3つのコンポーネントを作成します。
            - 展示の設定ではExporterは作成されていますが、Flow Monitorの設定内に `exporter [エクスポータ名]` の紐付け（バインド）コマンドが記述されていません。使われていない（Not used）ため、データが送信されません 。[^1]
        - **不正解の理由**:
            - **選択肢B, C**: Monitorをインターフェイスに適用する設定自体は正しいか、少なくともこの展示の中での最大の欠陥ではありません。
            - **選択肢D**: 到達性（ルーティング）の問題以前に、エクスポート先を指定する設定がMonitorに組み込まれていないことが原因です。


### QUESTION 60

- 問題文: Which Cisco VPN technology can use multipoint tunnel, resulting in a single GRE tunnel interface on the hub, to support multiple connections from multiple spoke devices?
（状況説明：ハブ上に単一のGREトンネルインターフェイスを作成し、複数のスポークデバイスからのマルチポイントトンネル接続をサポートできるCiscoのVPNテクノロジーはどれですか？）
- 選択肢:
A. DMVPN
B. GETVPN
C. Cisco Easy VPN
D. FlexVPN
- 正解: A
- セクション: none
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢A）**:
            - DMVPN（Dynamic Multipoint VPN）は、ハブ（センタールータ）上で「mGRE（Multipoint GRE）」インターフェイスを1つだけ作成し、多数のスポークルータからのIPsecトンネルを動的に終端するCisco独自のVPNテクノロジーです 。これによりハブ側の設定が大幅に簡素化されます。[^1]
        - **不正解の理由**:
            - **選択肢B**: GETVPN（Group Encrypted Transport VPN）はトンネルを使用せず（トンネルレス）、元のIPヘッダを維持したままペイロードを暗号化する技術です。
            - **選択肢C**: Easy VPNは古いリモートアクセスVPN技術であり、mGREのコンセプトは持ちません。
            - **選択肢D**: FlexVPNはIKEv2をベースとした新しいVPN技術であり、ハブ側では通常「Virtual-Template」を用いたポイントツーポイント（P2P）インターフェイスを動的に生成するため、単一のmGREインターフェイスを使用するDMVPNとはアーキテクチャが異なります。

### QUESTION 61

- 問題文: Which next hop is going to be used for 172.17.1.0/24 ?
（状況説明：展示（BGPのルーティングテーブル出力）を参照してください。172.17.1.0/24 の宛先に対して、どのネクストホップが使用されますか？）
- 選択肢:
A. 10.0.0.1
B. 192.168.1.2
C. 10.0.0.2
D. 192.168.3.2
- 正解: A
- セクション: none
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢A）**:
            - この問題は `show ip bgp` コマンドの出力を読み取る形式です。BGPテーブルにおいて、同じ宛先（172.17.1.0/24）に対して複数のパス（ネクストホップ）が存在する場合、ルータはBGPベストパス選択アルゴリズムに基づいて最適な経路を1つ選びます。
            - 最適と判定されたパスの先頭には `>`（ベストパス記号）が付与されます。したがって、`>` が付いている行のネクストホップアドレス（10.0.0.1）が実際にルーティングテーブルにインストールされ、使用されます 。[^1]
        - **不正解の理由**:
            - **選択肢B, C, D**: これらも代替パスとしてBGPテーブルに存在していますが、`>` 記号がついていないため、アクティブな転送経路としては使用されません。


### QUESTION 62

- 問題文: Which label operations are performed by a label edge router?
（状況説明：ラベルエッジルータ（LER）によって実行されるラベル操作はどれですか？）
- 選択肢:
A. SWAP and POP
B. SWAP and PUSH
C. PUSH and PHP
D. PUSH and POP
- 正解: D
- セクション: none
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢D）**:
            - LER（Label Edge Router）は、MPLSネットワークの境界（出入り口）に位置するルータです。
            - 外部のIPネットワークからパケットが入ってくる際にラベルを付与する操作を「**PUSH**（Impose）」と呼び、MPLSネットワークから出ていく際にラベルを取り外す操作を「**POP**（Dispose）」と呼びます。LERはこの両方の役割を担います 。[^1]
        - **不正解の理由**:
            - **選択肢A, B**: 「SWAP（ラベルの付け替え）」は、MPLSネットワーク内部の中継ルータである LSR（Label Switch Router）が行う主な操作です。
            - **選択肢C**: PHP（Penultimate Hop Popping）は、LERの1つ手前のルータが行う特殊なPOP操作であり、LER自身が行う操作の組み合わせとしては不適切です。


### QUESTION 63

- 問題文: Refer to the exhibit. A network engineer executes the show ipv6 ospf database command and is presented with the output that is shown. Which flooding scope is referenced in the link-state type?
（状況説明：展示を参照してください。ネットワークエンジニアが `show ipv6 ospf database` コマンドを実行し、表示された出力が確認されました。リンクステートタイプで参照されているフラッディングスコープ（伝播範囲）はどれですか？）
- 選択肢:
A. link-local
B. area
C. AS (OSPF domain)
D. reserved
- 正解: B
- セクション: none
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢B）**:
            - OSPFv3（IPv6用OSPF）では、LSA（Link State Advertisement）のタイプを示す16ビットの先頭部で、そのLSAがどこまでフラッディングされるか（スコープ）を定義しています。
            - Router LSAやNetwork LSA、Inter-Area Prefix LSAなどは、すべて特定の「**エリア（area）**」内にのみフラッディングされるスコープを持っています（出力からLSAタイプ 0x2000系の値が読み取れる場合、それはエリアスコープを意味します） 。[^1]
        - **不正解の理由**:
            - **選択肢A**: Link-localスコープ（0x0000系）は、直接接続されたリンク上のみに伝播するLSA（Link LSAなど）です。
            - **選択肢C**: ASスコープ（0x4000系）は、OSPFドメイン全体に伝播するLSA（AS External LSAなど）です。


### QUESTION 64

- 問題文: Refer to the exhibit. A company is evaluating multiple network management system tools. Trending graphs generated by SNMP data are returned by the NMS and appear to have multiple gaps. While troubleshooting the issue, an engineer noticed the relevant output. What solves the gaps in the graphs?
（状況説明：展示を参照してください。企業が複数のネットワーク管理システム（NMS）ツールを評価しています。NMSによって返されたSNMPデータのトレンドグラフには、複数のギャップ（データの欠落）が見られます。エンジニアがトラブルシューティングを行った結果、関連する出力に気づきました。グラフのギャップを解決する方法はどれですか？）
- 選択肢:
A. Remove the exceed-rate command in the class map.
B. Remove the class map NMS from being part of control plane policing.
C. Configure the CIR rate to a lower value that accommodates all the NMS tools
D. Separate the NMS class map in multiple class maps based on the specific protocols with appropriate CoPP actions
- 正解: D
- セクション: none
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢D）**:
            - グラフにギャップ（欠落）が発生しているのは、CoPP（Control Plane Policing）によってNMSからのSNMPポーリング通信の一部が制限・ドロップされているためです。
            - 現在のクラスマップ（NMS）は、複数の管理プロトコル（SNMP, SSH, Telnet等）をすべて一括りにして単一の帯域制限をかけていると推測されます。SNMPトラフィックに必要な帯域を確保するためには、SNMP専用のクラスマップを分離し、適切なレートを個別に設定することが最適な解決策となります 。[^1]
        - **不正解の理由**:
            - **選択肢A**: `exceed-action` 等の制限コマンド自体を削除するとCPU保護が機能しなくなり危険です。
            - **選択肢B**: 管理トラフィックをCoPPから外すことは、ルータをDoS攻撃の危険に晒すため推奨されません。
            - **選択肢C**: CIR（認定情報レート）を「下げる（lower）」と、さらにパケットがドロップされるため状況が悪化します。


### QUESTION 65

- 問題文: Refer to the following output:
`Router#show ip nhrp detail`
`1.1.2.8/32 via 10.2.1.2, Tunnel1 created 00:00:12, expire 01:59:47`
`Type: dynamic, Flags: authoritative unique nat registered used`
`NBMA address: 10.12.1.2`
What does the authoritative flag mean in regards to the NHRP information?
（状況説明：出力例を参照してください。NHRP情報に関連して「authoritative」フラグは何を意味しますか？）
- 選択肢:
A. It was obtained directly from the next-hop server.
B. Data packets are process switches for this mapping entry.
C. NHRP mapping is for networks that are local to this router.
D. The mapping entry was created in response to an NHRP registration request.
E. The NHRP mapping entry cannot be overwritten
- 正解: A
- セクション: none
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢A）**:
            - DMVPNなどのNHRP（Next Hop Resolution Protocol）のテーブルにおいて `authoritative`（権威ある）フラグが立っている場合、そのマッピング情報がキャッシュや他のルータからの間接的な学習ではなく、**NHS（Next Hop Server：ハブルータなど）から直接得られた確実な情報**であることを意味します 。[^1]
        - **不正解の理由**:
            - **選択肢B**: パケットがプロセススイッチングされるかどうかは、ルータの転送方式（CEFの有無など）に依存し、このフラグの意味ではありません。
            - **選択肢C**: ローカルネットワーク用というフラグは `local` と表示されます。
            - **選択肢D**: 登録リクエストに対して作成されたエントリであることを示すフラグは `registered` です。
            - **選択肢E**: 上書きできないことを示すフラグではありません。


### QUESTION 66

- 問題文: Which security feature can protect DMVPN tunnels?
（状況説明：DMVPNトンネルを保護できるセキュリティ機能はどれですか？）
- 選択肢:
A. IPsec
B. TACACS
C. RTBH
D. RADIUS
- 正解: A
- セクション: none
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢A）**:
            - DMVPN（Dynamic Multipoint VPN）は、トンネリングプロトコルとしてmGRE（Multipoint GRE）を使用しますが、GRE自体には暗号化機能がありません。
            - トラフィックの機密性（暗号化）、完全性、およびピアの認証を確保してDMVPNトンネルを安全に保護するためには、**IPsec（IP Security）** を組み合わせるのが標準的かつ必須の設計です 。[^1]
        - **不正解の理由**:
            - **選択肢B, D**: TACACSやRADIUSはデバイスの管理アクセスやユーザー認証（AAA）に使用されるプロトコルであり、VPNトンネルのデータ暗号化は行いません。
            - **選択肢C**: RTBH（Remotely Triggered Black Hole）はDDoS攻撃の緩和に使われるルーティング技術であり、トンネルの保護（暗号化）技術ではありません。


### QUESTION 67

- 問題文: Which two methods use IPsec to provide secure connectivity from the branch office to the headquarters office? Choose two.
（状況説明：支社から本社への安全な接続を提供するために、IPsecを使用する2つの方法はどれですか？2つ選択してください。）
- 選択肢:
A. DMVPN
B. MPLS VPN
C. Virtual Tunnel Interface (VTI)
D. SSL VPN
E. PPPoE
- 正解: A, C
- セクション: none
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢A, C）**:
            - **DMVPN**: mGREとIPsecを組み合わせて、拠点間に動的かつ安全なVPNトンネルを構築するCiscoの技術です。
            - **VTI (Virtual Tunnel Interface)**: 従来の暗号化マップ（Crypto Map）の代わりに仮想的なルーティング可能なインターフェイスを作成し、そこを通るトラフィックをIPsecでカプセル化・暗号化する技術です。どちらもサイト間（拠点間）のセキュアな接続にIPsecを使用します 。[^1]
        - **不正解の理由**:
            - **選択肢B**: MPLS VPNはプロバイダ網内でVRFとラベルを使用してトラフィックを分離しますが、パケット自体の暗号化（IPsec）はデフォルトでは行いません。
            - **選択肢D**: SSL VPNはTLS（またはDTLS）を使用して暗号化を行う技術であり、IPsecプロトコルスイートは使用しません。主にリモートアクセス用です。
            - **選択肢E**: PPPoEはブロードバンドアクセス用の認証プロトコルであり、暗号化は行いません。


### QUESTION 68

- 問題文: Which SNMP verification command shows the encryption and authentication protocols that are used in SNMPV3?
（状況説明：SNMPv3で使用されている暗号化（privacy）および認証（authentication）プロトコルを表示するSNMP確認コマンドはどれですか？）
- 選択肢:
A. show snmp group
B. show snmp user
C. show snmp
D. show snmp view
- 正解: B
- セクション: none
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢B）**:
            - SNMPv3は、ユーザーベースのセキュリティモデル（USM）を採用しており、ユーザーごとに認証（MD5/SHA）と暗号化（DES/AES）のアルゴリズムが設定されます。
            - 構成されたユーザーがどの暗号化プロトコルと認証プロトコルを使用しているかを確認するには `show snmp user` コマンドを使用します 。[^1]
        - **不正解の理由**:
            - **選択肢A**: `show snmp group` は、グループとそれに紐づくセキュリティモデルやアクセスビュー（Read/Write権限）を表示しますが、具体的なアルゴリズムまでは表示しません。
            - **選択肢C**: `show snmp` は、SNMP全体の状態や送受信されたパケット統計を表示します。
            - **選択肢D**: `show snmp view` は、ユーザーがアクセスできるOIDツリーの範囲（ビュー）を表示します。


### QUESTION 69

- 問題文: Which two protocols can cause TCP starvation? Choose two.
（状況説明：TCPスターベーション（TCPの枯渇/飢餓）を引き起こす可能性のある2つのプロトコルはどれですか？2つ選択してください。）
- 選択肢:
A. TFTP
B. SNMP
C. SMTP
D. HTTPS
E. FTP
- 正解: A, B
- セクション: none
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢A, B）**:
            - TCPスターベーションは、同じリンク上でTCPとUDPのトラフィックが競合し帯域が逼迫した場合に発生します。TCPは輻塞を検知するとウィンドウサイズを小さくして送信量を減らしますが、UDPは輻塞制御を行わず全速力で送信し続けます。結果としてUDPが帯域を占有し、TCPが通信できなくなります。
            - 選択肢の中でUDPを使用するプロトコルは、**TFTP**（ポート69）と **SNMP**（ポート161/162）の2つです 。[^1]
        - **不正解の理由**:
            - **選択肢C, D, E**: SMTP、HTTPS、FTPはすべてTCP上で動作するプロトコルのため、自身がTCPの輻塞制御メカニズムに従う側であり、UDPのように他のTCPトラフィックを一方的に押し出す（スターベーションを引き起こす）原因にはなりません。


### QUESTION 70

- 問題文: A network engineer needs to verify IP SLA operations on an interface that shows on indication of excessive traffic. Which command should the engineer use to complete this action?
（状況説明：ネットワークエンジニアが、過剰なトラフィックの兆候が見られるインターフェイスでIP SLAの動作を確認する必要があります。この操作を完了するためにエンジニアが使用すべきコマンドはどれですか？）
- 選択肢:
A. show frequency
B. show track
C. show reachability
D. show threshold
- 正解: B
- セクション: none
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢B）**:
            - IP SLAは単体で監視を行うだけでなく、ルータの経路切り替えや制御と連動させるために「トラックオブジェクト（Track object）」と結びつけて使用されることが一般的です（例：`track 1 ip sla 10 state`）。
            - インターフェイスのトラフィック異常などでSLAの監視対象がダウンし、フェイルオーバーが正しく機能しているか（トラッキング状態がUPかDOWNか）を検証するためには `show track` コマンドを使用するのが効果的です 。[^1]
        - **不正解の理由**:
            - **選択肢A, C, D**: IP SLAの情報を確認するための一般的なCisco IOSコマンドとして `show frequency`、`show reachability`、`show threshold` といった独立したコマンドは存在しません。（SLAの詳細を見る場合は `show ip sla statistics` などを使用します）。

### QUESTION 71

- 問題文: Which protocol does VRF-Lite support?
（状況説明：VRF-Liteがサポートしているルーティングプロトコルはどれですか？）
- 選択肢:
A. IS-IS
B. ODR
C. EIGRP
D. IGRP
- 正解: C
- セクション: none
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢C）**:
            - VRF-Lite（CEルータ内でMPLS/BGPを使わずにVRFを構成する機能）は、各VRFインスタンス内で独立したルーティングプロセスを実行できます。サポートされている代表的なIGPプロトコルとして、OSPF、RIPv2、BGP、および **EIGRP** があります 。[^1]
        - **不正解の理由**:
            - **選択肢A**: IS-ISはマルチトポロジ機能（Multi-Topology IS-IS）を用いた分離は可能ですが、Ciscoの従来の標準的なVRF-Liteのサポートプロトコルとして最初に挙げられるものではありません（一部制限や特殊な構成が必要）。
            - **選択肢B**: ODR（On-Demand Routing）はCDPを利用してスタブネットワークの経路を収集する簡易的な仕組みであり、VRFでの利用には適していません。
            - **選択肢D**: IGRPはすでにCisco IOSから完全に削除（廃止）されている古いプロトコルです。


### QUESTION 72

- 問題文: Which two statements about redistributing EIGRP into OSPF are true? Choose two.
（状況説明：EIGRPからOSPFへのルート再配布に関する2つの記述のうち、正しいものはどれですか？2つ選択してください。）
- 選択肢:
A. The redistributed EIGRP routes appear as type 3 LSAs in the OSPF database.
B. The redistributed EIGRP routes appear as type 5 LSAs in the OSPF database.
C. The administrative distance of the redistributed routes is 170.
D. The redistributed EIGRP routes appear as OSPF external type 1.
E. The redistributed EIGRP routes as placed into an OSPF area whose area ID matches the EIGRP autonomous system number.
F. The redistributed EIGRP routes appear as OSPF external type 2 routes in the routing table.
- 正解: B, F
- セクション: none
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢B, F）**:
            - ASBR（自律システム境界ルータ）によって外部プロトコル（EIGRPなど）からOSPFへ再配布されたルートは、デフォルトで **Type 5 LSA（AS External LSA）** としてOSPFデータベースに登録されます（選択肢B）。
            - さらに、ルーティングテーブル上ではデフォルトで **O E2（OSPF External Type 2）** ルートとして表示されます。Type 2はOSPF内部のコストを加算せず、外部メトリック（デフォルト20）のみを維持します（選択肢F） 。[^1]
        - **不正解の理由**:
            - **選択肢A**: Type 3 LSAは、同じOSPFドメイン内の異なるエリア間（Inter-Area）のルートを表すものであり、外部プロトコルの再配布には使われません。
            - **選択肢C**: 再配布されたルートはOSPFルート（AD=110）としてルーティングテーブルに入ります。AD=170は「外部EIGRPルート（D EX）」の値です。
            - **選択肢D**: デフォルトはType 2（E2）です。Type 1（E1）にするには再配布時に手動で `metric-type 1` を設定する必要があります。
            - **選択肢E**: エリアIDとEIGRPのAS番号を一致させるという仕様や要件は存在しません。


### QUESTION 73

- 問題文: Users were moved from the local DHCP server to the remote corporate DHCP server. After the move, none of the users were able to use the network. Which two issues will prevent this setup from working properly? Choose two.
（状況説明：ユーザーがローカルのDHCPサーバーからリモート（本社）のDHCPサーバーに移行されました。移行後、どのユーザーもネットワークを使用できなくなりました。このセットアップが正常に機能しなくなる原因となる2つの問題はどれですか？2つ選択してください。）
- 選択肢:
A. Auto-QoS is blocking DHCP traffic.
B. The DHCP server IP address configuration is missing locally.
C. 802.1X is blocking DHCP traffic.
D. The broadcast domain is too large for proper DHCP propagation.
E. The route to the new DHCP server is missing.
- 正解: B, E
- セクション: none
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢B, E）**:
            - DHCPはブロードキャスト通信（DORAプロセス）で動作しますが、ブロードキャストはルータを越えられません。リモートのサーバーに要求を届けるには、ローカルルータに `ip helper-address` コマンドでDHCPサーバーのIPアドレスを設定する必要があります（選択肢B）。
            - さらに、そのヘルパーアドレスによって変換されたユニキャストパケットがリモートサーバーに到達できるよう、ルータが新しいDHCPサーバーへの正しいIPルーティング経路（ルート）を持っている必要があります（選択肢E） 。[^1]
        - **不正解の理由**:
            - **選択肢A**: Auto-QoSは音声やビデオの帯域を優先する機能であり、デフォルトでDHCPトラフィックをブロックすることはありません。
            - **選択肢C**: 802.1X（ポート認証）はDHCP以前のレイヤ2の認証機能ですが、サーバーの「移行（場所の変更）」によって突然発生する問題とは直接結びつきません。
            - **選択肢D**: ブロードキャストドメインが大きいこと自体がDHCPを完全に停止させる原因（ブロック要因）にはなりません。


### QUESTION 74

- 問題文: Which command is used to check IP SLA when an interface is suspected to receive lots of traffic with options?
（状況説明：インターフェイスが過剰なトラフィックを受信していると疑われる場合に、IP SLA（およびトラッキング）を確認するために使用されるコマンドはどれですか？）
- 選択肢:
A. show track
B. show threshold
C. show timer
D. show delay
- 正解: A
- セクション: none
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢A）**:
            - IP SLAは、トラフィックの輻塞などによってパケットロスや遅延が発生すると、監視条件を満たせずにダウンします。その監視結果はルータの「トラッキング機能」と連携していることが多いため、`show track` コマンドを使用して、IP SLAに紐づくオブジェクトの現在の状態（UP/DOWN）やステータスの変化履歴を確認するのが適切なトラブルシューティング手法です 。[^1]
        - **不正解の理由**:
            - **選択肢B, C, D**: Cisco IOSにおいて、IP SLAの状態を直接確認するための独立したコマンドとして `show threshold`、`show timer`、`show delay` は存在しません（詳細は `show ip sla statistics` 等で確認します）。


### QUESTION 75

- 問題文: What is an advantage of using BFD?
（状況説明：BFD（Bidirectional Forwarding Detection）を使用する利点は何ですか？）
- 選択肢:
A. It detects local link failure at layer 1 and updates routing table.
B. It detects local link failure at layer 2 and updates routing protocols.
C. It has sub-second failure detection for layer 1 and layer 3 problems.
D. It has sub-second failure detection for layer 1 and layer 2 problems.
- 正解: D
- セクション: none
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢D）**:
            - BFDの最大の利点は、ルーティングプロトコル（OSPFやBGPなど）自体のタイマー（数秒〜数十秒）に依存せず、転送パス上の障害を**サブセカンド（1秒未満、ミリ秒単位）**という極めて高速なスピードで検出できる点にあります。
            - また、特定のメディアやプロトコルに依存せず、レイヤ1（物理）やレイヤ2（データリンク）を含む転送経路全体の障害を独立して検出する軽量なプロトコルです 。（※一部のCisco系教材・問題プールではCやDで表記揺れがありますが、公式解答群に従いDとしています）。[^1]
        - **不正解の理由**:
            - **選択肢A, B**: BFD自体はルーティングテーブルやプロトコルを「直接更新（アップデート）」するわけではありません。BFDが障害を検知すると、連携しているルーティングプロトコルに「ダウンした」というシグナルを送り、プロトコル側がテーブルを更新します。


### QUESTION 76

- 問題文: Which component of MPLS VPNs is used to extend the IP address so that an engineer is able to identify to which VPN it belongs?
（状況説明：エンジニアがどのVPNに属しているかを識別できるように、IPアドレスを拡張するために使用されるMPLS VPNのコンポーネントはどれですか？）
- 選択肢:
A. VPNv4 address family
B. RD
C. RT
D. LDP
- 正解: B
- セクション: none
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢B）**:
            - 異なるVPNの顧客が同じプライベートIPv4アドレス空間（例：192.168.1.0/24）を使用した場合、ルーティングテーブル上で衝突してしまいます。
            - これを防ぐため、PEルータは32ビットのIPv4アドレスの先頭に64ビットの**RD（Route Distinguisher：ルート識別子）**を付与し、合計96ビットのグローバルに一意なアドレスに「拡張」します。これによりどのVPNの経路かを識別します 。[^1]
        - **不正解の理由**:
            - **選択肢A**: VPNv4はRDを付与した「結果」として作られるアドレスの形式（ファミリ）であり、拡張のために付加される「コンポーネント」そのものはRDです。
            - **選択肢C**: RT（Route Target）は、VRFにルートをインポート・エクスポートする「フィルタリング（ルーティング制御）」の役割であり、アドレスを一意にするために付与されるものではありません。
            - **選択肢D**: LDP（Label Distribution Protocol）はラベルを配布するプロトコルです。


### QUESTION 77

- 問題文: Refer to the exhibit. The ACL is placed on the inbound GigabitEthernet 0/1 interface of the router. Host 192.168.10.10 cannot SSH to host 192.168.100.10 even though the flow is permitted. Which action resolves the issue without opening full access to this router?
（状況説明：展示を参照してください。ACLがルータのGigabitEthernet 0/1インターフェイスのインバウンド方向に適用されています。ホスト192.168.10.10は、フローが「許可（permit）」されているはずなのに、ホスト192.168.100.10へのSSH接続ができません。このルータへのフルアクセスを開放せずにこの問題を解決するアクションはどれですか？）
- 選択肢:
A. Move the SSH entry to the beginning of the ACL.
B. Temporarily move the permit ip any any line to the beginning of the ACL to see if the flow works.
C. Temporarily remove the ACL from the interface to see if the flow works.
D. Run the show access-list FILTER command to view if the SSH entry has any hit statistic associated with it.
- 正解: A
- セクション: none
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢A）**:
            - ルータのアクセスリスト（ACL）は、「上から下（Top-Down）」の順番で評価され、最初に条件がマッチした行のアクション（permit/deny）が実行されます。
            - おそらく現在のACLでは、SSHを許可するエントリよりも上の行に「広範な拒否（deny ip ...）」のルールが存在しており、SSHトラフィックがそこで先にブロックされてしまっています。SSHのエントリをACLの先頭（beginning）に移動させれば、他の拒否ルールに引っかかる前に許可されます 。[^1]
        - **不正解の理由**:
            - **選択肢B, C**: `permit ip any any` を先頭にしたり、ACLを外したりすると、ルータのセキュリティが一時的とはいえ完全に無防備になり、「フルアクセスを開放せずに」という要件に違反します。
            - **選択肢D**: ヒットカウントを確認することはトラブルシューティングの手段ですが、問題そのものを「解決（resolves）」するアクションではありません。


### QUESTION 78

- 問題文: Refer the exhibit... Which action resolves intermittent connectivity observed with the SNMP trap packets?
（状況説明：展示を参照してください。SNMPトラップパケットで断続的な接続（パケットドロップ）が観察されています。この問題を解決するアクションはどれですか？）
- 選択肢:
A. Decrease the committed burst Size of the mgmt class map.
B. Increase the CIR of the mgmt class map.
C. Add a new class map to match TCP traffic.
D. Add one new entry in the ACL 120 to permit the UDP port 161.
- 正解: B
- セクション: none
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢B）**:
            - 展示のCoPP（Control Plane Policing）設定では、クラスマップ `mgmt`（管理用トラフィック）に対してQoSのポリシング（帯域制限）が適用されています。SNMPトラップ（UDP 162）もこのクラスに含まれます。
            - 断続的に接続が切れる（ドロップする）ということは、現状設定されているCIR（Committed Information Rate：認定情報レート、つまり許可される帯域幅）が低すぎて、トラフィックが制限に引っかかっていることを意味します。したがって、CIRの値を増やす（Increase）ことでドロップを防ぐことができます 。[^1]
        - **不正解の理由**:
            - **選択肢A**: バーストサイズ（Bc）を下げると、瞬間的なトラフィックのスパイクに対応できなくなり、さらにドロップが悪化します。
            - **選択肢C**: SNMPトラップはUDPを使用するため、TCPのクラスマップを追加しても効果がありません。
            - **選択肢D**: SNMPポーリング（161）を許可しても、SNMPトラップ（162）の帯域不足問題は解決しません。


### QUESTION 79

- 問題文: What destination addresses does EIGRP use when feasible? Choose two.
（状況説明：EIGRPが可能な場合に使用する宛先アドレスはどれですか？2つ選択してください。）
- 選択肢:
A. IP address 224.0.0.9
B. IP address 224.0.0.10
C. IP address 224.0.0.8
D. MAC address 0100.5E00.000A
E. MAC address 0C15.C000.0001
- 正解: B, D
- セクション: none
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢B, D）**:
            - EIGRPは、Helloパケットやルーティングアップデートの送信にマルチキャストを使用します。IPv4におけるEIGRP専用のマルチキャストIPアドレスは **224.0.0.10** です（選択肢B）。
            - イーサネット上でIPマルチキャストを送信する場合、MACアドレスは `0100.5E` から始まり、IPアドレスの下位23ビットを16進数に変換して付加します。「10」は16進数で「0A」となるため、宛先MACアドレスは **0100.5E00.000A** となります（選択肢D） 。[^1]
        - **不正解の理由**:
            - **選択肢A**: 224.0.0.9はRIPv2が使用するマルチキャストアドレスです。
            - **選択肢C**: 224.0.0.8は古いプロトコル（EGPなど）用のアドレスです。OSPFは224.0.0.5/6を使用します。
            - **選択肢E**: ランダムな架空のMACアドレスであり、マルチキャストの仕様に適合しません。


### QUESTION 80

- 問題文: How are packets forwarded in an MPLS domain?
（状況説明：MPLSドメイン内で、パケットはどのように転送されますか？）
- 選択肢:
A. Using the destination IP address of the packet.
B. Using the source IP address of the packet.
C. Using a number that has been specified in a label.
D. Using the MAC address of the frame.
- 正解: C
- セクション: none
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢C）**:
            - MPLS（Multi-Protocol Label Switching）ネットワークの中核的な仕組みは、従来のルータのようにIPヘッダ（レイヤ3）の宛先IPアドレスをルックアップして転送するのではなく、パケットに付与された**短い固定長の「ラベル（数字）」**に基づいてハードウェアレベルで高速に転送（スイッチング）を行う点にあります 。[^1]
        - **不正解の理由**:
            - **選択肢A, B**: IPアドレス（宛先/送信元）を読み取って転送するのは、MPLSドメイン外（またはエッジルータ）で行われる通常のIPルーティングの動作です。
            - **選択肢D**: MACアドレス（レイヤ2）を使用した転送は、通常のイーサネットスイッチの動作であり、MPLSの直接的な転送メカニズムではありません。

### QUESTION 81

- 問題文: Refer to the exhibit. Drag and drop the credentials from the left onto the remote login information on the right to resolve a failed login attempt to vtys. Not all credentials are used, choose the best.
（状況説明：展示を参照してください。vtyへのログイン試行の失敗を解決するために、左側のクレデンシャル（認証情報）を右側のリモートログイン情報にドラッグアンドドロップしてください。すべてのクレデンシャルが使用されるわけではありません。）
- 選択肢:
*(Drag and Drop format)*
- 正解: (Select and Place)
- セクション: none
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由**: AAA（認証・認可・アカウンティング）が有効化されている環境下での、各VTY（仮想端末）ラインへの認証リスト適用規則を問う問題です 。[^1]
            - `line vty 0` には、特定の名前付き認証リスト（例：`telnet`）が明示的に適用されているため、そのリストで定義されている「ローカルのユーザー名とパスワード」を使用してログインする必要があります。
            - `line vty 1` には個別の認証リストが指定されていないため、グローバルで定義された `default` 認証リストが適用されます。このデフォルトリストが `none` に設定されている場合、パスワードなしでログインすることになります。
        - **不正解の理由**: 適用されている認証リスト（カスタムかデフォルトか）のルールを取り違えてマッピングすると不正解になります。


### QUESTION 82

- 問題文: You just discovered that a ping packet sent from one of the devices to another took a different path in the return than it did on its way to the destination. What behavior caused this?
（状況説明：あるデバイスから別のデバイスへ送信されたPingパケットが、宛先へ向かう時と戻ってくる時で異なる経路（パス）を通ったことを発見しました。この挙動を引き起こしたのは何ですか？）
- 選択肢:
A. Windowing
B. Global synchronization
C. MSS
D. Asymmetric routing
- 正解: D
- セクション: none
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢D）**:
            - ネットワークにおいて、パケットが行き（送信）と帰り（受信）で異なるルートを経由して通信する状態を「**非対称ルーティング（Asymmetric routing）**」と呼びます。ルーティングプロトコルは宛先ベースで独立して最適経路を計算するため、ネットワークトポロジによってはこの挙動が正常に発生します 。[^1]
        - **不正解の理由**:
            - **選択肢A**: Windowing（ウィンドウ制御）はTCPのフロー制御（データ送信量の調整）の仕組みです。
            - **選択肢B**: TCP Global synchronization（TCPグローバル同期）は、ネットワークの輻塞時に複数のTCPセッションが同時に送信を絞り、一斉に再開してしまう現象です。
            - **選択肢C**: MSS（Maximum Segment Size）はTCPが一度に送信できる最大データサイズを指し、経路選択とは無関係です。


### QUESTION 83

- 問題文: How long is the default NHRP cache timer?
（状況説明：デフォルトのNHRPキャッシュタイマーの長さはどれくらいですか？）
- 選択肢:
A. 2 hours
B. 1 hour
C. 30 minutes
D. 15 minutes
- 正解: A
- セクション: none
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢A）**:
            - DMVPNなどで使用されるNHRP（Next Hop Resolution Protocol）は、動的に学習したIPからNBMA（物理IP）へのマッピング情報をキャッシュに保持します。
            - Cisco IOSにおけるNHRPのキャッシュ（ホールドタイム）のデフォルト値は **7200秒（2時間）** に設定されています 。[^1]
        - **不正解の理由**:
            - **他の選択肢**: 1時間、30分、15分などはCisco IOSのデフォルト値として誤りです。


### QUESTION 84

- 問題文: The OSPF dead interval defaults to how many times the hello interval?
（状況説明：OSPFのDeadインターバルは、デフォルトでHelloインターバルの何倍に設定されていますか？）
- 選択肢:
A. Two
B. Three
C. Four
D. Five
- 正解: C
- セクション: none
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢C）**:
            - OSPFの仕様では、ネイバーのダウンを検知するための「Deadインターバル」は、デフォルトで「Helloインターバル」の **4倍（Four）** の長さに自動設定されます。
            - 例として、ブロードキャストネットワークではHelloが10秒、Deadが40秒となります。ノンブロードキャスト（NBMA）ではHelloが30秒、Deadが120秒となります 。[^1]
        - **不正解の理由**:
            - **他の選択肢**: 2倍、3倍（EIGRPのデフォルトホールドタイム）、5倍などはOSPFのデフォルトの乗数として誤りです。


### QUESTION 85

- 問題文: How are customer routes isolated on PE routers in an MPLS Layer 3 VPN?
（状況説明：MPLS Layer 3 VPN環境において、PE（Provider Edge）ルータ上で顧客のルートはどのように分離（隔離）されていますか？）
- 選択肢:
A. By using VRF
B. By using VDCs
C. By using MP-BGP
D. By using LDP
- 正解: A
- セクション: none
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢A）**:
            - PEルータは複数の顧客（VPN）を同時に収容するため、顧客ごとに独立した仮想的なルーティングテーブルを作成します。
            - この機能を提供するのが **VRF (Virtual Routing and Forwarding)** です。VRFにより、異なる顧客が同じIPアドレス（例: 192.168.1.0/24）を使用していても、ルータ内でテーブルが隔離されているため競合しません 。[^1]
        - **不正解の理由**:
            - **選択肢B**: VDC（Virtual Device Context）はNexusスイッチなどでハードウェア全体を論理的に分割する機能であり、MPLS VPNの標準コンポーネントではありません。
            - **選択肢C**: MP-BGPはPEルータ「間」でVPNv4ルートを交換・伝播させるプロトコルであり、ルータ「内部」での隔離自体を担うのはVRFです。
            - **選択肢D**: LDP（Label Distribution Protocol）は転送用のラベルを配布するプロトコルです。


### QUESTION 86

- 問題文: The OSPF database of a router shows LSA types 1, 2, 7, and 3 for the default route only. Which type of area is this router connected to?
（状況説明：あるルータのOSPFデータベースには、LSAのタイプ1、2、7、および「デフォルトルート専用のLSAタイプ3」のみが表示されています。このルータはどのタイプのエリアに接続されていますか？）
- 選択肢:
A. stub area
B. totally stubby area
C. NSSA totally stub
D. NSSA
- 正解: C
- セクション: none
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢C）**:
            - 「LSAタイプ7（NSSA外部LSA）」が存在する時点で、このエリアは **NSSA (Not-So-Stubby Area)** であることが確定します（通常のスタブエリアにはタイプ7は存在しません）。
            - さらに、他のエリアからの個別ルート（通常のLSAタイプ3）がフィルタリングされ、「デフォルトルート（0.0.0.0/0）としてのタイプ3のみ」が存在しているという記述から、より厳格な **Totally NSSA（または NSSA Totally Stub）** エリアであると判断できます 。[^1]
        - **不正解の理由**:
            - **選択肢A, B**: LSAタイプ7が許可されないため除外されます。
            - **選択肢D**: 通常のNSSAであれば、デフォルトルート以外の個別のLSAタイプ3（他のエリアのサブネット情報）もデータベースに多数表示されるはずです。


### QUESTION 87

- 問題文: Which three IP SLA performance metrics can you use to monitor enterprise-class networks? Choose three.
（状況説明：エンタープライズクラスのネットワークを監視するために使用できる3つのIP SLAパフォーマンスメトリックはどれですか？3つ選択してください。）
- 選択肢:
A. Packet loss
B. Delay
C. bandwidth
D. Connectivity
E. Reliability
F. traps
- 正解: A, B, D
- セクション: none
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢A, B, D）**:
            - Cisco IP SLAは、ネットワークにアクティブなプローブ（テストパケット）を送信し、実際のパフォーマンスを継続的に測定します。
            - 主要な測定指標（メトリック）として、**パケットロス（Packet loss）**、応答時間やジッタなどの**遅延（Delay）**、およびターゲットへの**到達性・接続性（Connectivity）** が挙げられます 。[^1]
        - **不正解の理由**:
            - **選択肢C**: IP SLAはインターフェイスの実際の利用帯域幅（Bandwidth）をSNMPのようにポーリング測定するものではありません。
            - **選択肢E**: Reliability（信頼性）はルーティングプロトコル（EIGRPなど）のメトリックパラメータの一種であり、IP SLAの直接の測定項目名ではありません。
            - **選択肢F**: Traps（トラップ）はイベント発生時にSNMP経由で送信される「通知手法」であり、監視対象の「メトリック」自体ではありません。


### QUESTION 88

- 問題文: A network administrator notices that the BGP state drops and logs are generated for missing BGP hello keepalives. What is the potential problem?
（状況説明：ネットワーク管理者が、BGPステータスがドロップ（切断）し、BGPのHelloキープアライブが欠落しているというログが生成されていることに気づきました。潜在的な問題は何ですか？）
- 選択肢:
A. Incorrect neighbor options.
B. Hello timer mismatch.
C. BGP path MTU enabled.
D. MTU mismatch.
- 正解: D
- セクション: none
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢D）**:
            - OSPF等のIGPとは異なり、BGPセッション自体はTCPポート179で確立されるため、初期接続は成功する場合があります。しかし、両端のインターフェイス間で **MTUの不一致（MTU mismatch）** があると、ルーティングアップデートなどを含む大きなサイズのBGPパケットが途中でサイレントに破棄（ドロップ）されてしまいます。
            - これによりTCPの再送が繰り返され、キープアライブパケットのやり取りも滞るため、最終的にホールドタイマーが切れてBGPネイバーがダウン（フラッピング）します 。[^1]
        - **不正解の理由**:
            - **選択肢A**: ネイバーオプションが間違っていれば、そもそも初期のTCPセッションやBGP OPENメッセージの段階で拒否されます。
            - **選択肢B**: OSPFとは異なり、BGPはネイバー間でタイマー（キープアライブ/ホールドタイム）が一致していなくても、ネゴシエーションして小さい方の値を自動採用するためセッションは切断されません。
            - **選択肢C**: Path MTU Discovery（PMTUD）はMTU問題を回避・最適化するための機能であり、問題の原因ではありません。


### QUESTION 89

- 問題文: Which access list entry checks for an ACK within a packet header?
（状況説明：パケットヘッダー内のACK（確認応答）フラグをチェックするアクセスリストエントリはどれですか？）
- 選択肢:
A. access-list 49 permit ip any any eq 21 tcp-ack
B. access-list 49 permit tcp any any eq 21 tcp-ack
C. access-list 149 permit tcp any any eq 21 established
D. access-list 49 permit tcp any any eq 21 established
- 正解: C
- セクション: none
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢C）**:
            - TCPヘッダ内のACKフラグ（またはRSTフラグ）がセットされているパケット、つまり「すでに双方向で確立されたセッションの応答トラフィック」だけを許可するには、拡張ACLで `established` キーワードを使用します。
            - 拡張ACLの番号範囲は `100`〜`199` であるため、`access-list 149` を使用している選択肢Cが構文として唯一正しいものとなります 。[^1]
        - **不正解の理由**:
            - **選択肢A, B, D**: アクセスリスト番号「49」は「標準ACL（1〜99）」の範囲です。標準ACLは送信元IPアドレスしかチェックできないため、プロトコル（TCP）やポート番号（21）、フラグ（established）を指定すると構文エラーになります。また、`tcp-ack` というパラメータは存在しません。


### QUESTION 90

- 問題文: You are implementing WAN access for an enterprise network while running applications that require a fully meshed network, which two design standards are appropriate for such an environment? Choose two.
（状況説明：フルメッシュネットワークを必要とするアプリケーションを実行しながら、エンタープライズネットワークのWANアクセスを実装しています。このような環境に適した2つの設計標準はどれですか？2つ選択してください。）
- 選択肢:
A. A centralized DMVPN solution to simplify connectivity for the enterprise.
B. A dedicated WAN distribution layer to consolidate connectivity to remote sites.
C. A collapsed core and distribution layer to minimize costs.
D. Multiple MPLS VPN connections with static routing.
E. Multiple MPLS VPN connections with dynamic routing.
- 正解: A, B
- セクション: none
- 解説（Explanation Reference）
    - 詳細説明:
        - **正解の理由（選択肢A, B）**:
            - **選択肢A**: フルメッシュの通信を静的なIPsecトンネルで作ると設定が膨大になり管理が破綻します。「DMVPN (Dynamic Multipoint VPN)」を使用すれば、Phase 2やPhase 3の機能によってスポーク拠点同士が動的にフルメッシュトンネルを確立できるため、構成を大幅に簡素化できます。
            - **選択肢B**: エンタープライズのWANアーキテクチャのベストプラクティスとして、リモートサイトとの接続を集約し、ルーティングやセキュリティポリシーを適用するための「専用のWANディストリビューション（境界）レイヤ」を設ける設計が推奨されます 。[^1]
        - **不正解の理由**:
            - **選択肢C**: 費用を最小化するための「コラプストコア（コアとディストリビューションを統合）」は小規模ネットワーク向けであり、高度なフルメッシュWAN要件を持つエンタープライズのWANエッジ設計としては拡張性に欠けます。
            - **選択肢D, E**: 複数のMPLS VPNを手動でフルメッシュ構成にすることはコストと管理オーバーヘッドの観点から非効率です（特にDのスタティックルーティングはスケーラビリティがありません）。