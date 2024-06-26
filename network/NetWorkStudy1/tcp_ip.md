### TCP / IPの階層モデル

- TCP / IP(*Transmission Control Protocol / Internet Protocol*)とは

現在のインターネット通信およびイントラネット通信において最も利用されている通信プロトコル。TCP / IPは複数のプロトコルからなるが、中心的な役割を果たすのがTCPとIPであることからTCP / IPと呼ばれるようになった

- TCP / IPの階層モデル

TCP / IPにおける階層モデルは4階層から構成されている。アプリケーション層・トランスポート層・インターネット層・ネットワークインターフェース層の4つ。ネットワークインターフェース層は単にネットワークアクセス層とも呼ばれている

<img width="500" alt="" src="./images/hierarchy_model.png">

- 各階層における役割

・アプリケーション層(TCP / IPプロトコルスタック)  
主に`アプリケーションごとの固有`の規定。HTTP・FTP・SMTP・SSHなどの`アプリケーション層のプロトコル`により、通信アプリケーションの機能が実現される<br><br>
・トランスポート層  
主に`ノード間のデータ転送の信頼性を確保する`ための規定。`TCPまたはUDPのプロトコル`を使用する。TCPを使用する場合は信頼性の高い通信を実現し、UDPを使用する場合は信頼性ではなく効率重視のデータ転送を実現することになる<br><br>
・インターネット層  
主に`ネットワーク間のエンドツーエンドの通信`のための規定。IPが代表的なプロトコル。IPにより、ネットワーク上のノードに対し自分の位置情報となる論理アドレス(IPアドレス)を割り当てられるのでエンドツーエンドの通信を実現する<br><br>
・ネットワークインターフェイス層  
主に`直接的に接続されたノード間の通信`のための規定。LANプロトコルはイーサネット、WANプロトコルはPPPが代表的なプロトコル。イーサネットでは、セグメント上のノードに対して、自分の位置情報となる物理アドレス(MACアドレス)を割り当てることができ、ローカルでのノード間の通信が実現することになる

- TCP / IPの階層モデルにおけるカプセル化・非カプセル化

コンピュータ間で通信する場合、送信側のコンピュータではアプリケーション層、トランスポート層、インターネット層、ネットワークインターフェース層、通信ケーブルの順番でカプセル化を行いデータが送出され、受信側では反対の順番で非カプセル化を行って送信側で付加したヘッダを取り除いていく。考え方はOSI参照モデルと同様。  
イーサネットLANにあるコンピュータがWebサイトを参照するためには、アプリケーション層ではHTTPプロトコル、トランスポート層ではTCPプロトコル、インターネット層ではIPプロトコル、ネットワークインターフェース層ではEthernetプロトコルを使用し、送信側では各層で処理されるたびに、これらのプロトコルのヘッダが付加されて送信されて、受信側では各階層でそのヘッダを取り除いている

<img width="500" alt="" src="./images/ヘッダ処理.png">

### IP

- IP(*Internet Protocol*)とは

OSI参照モデルでは`ネットワーク層`で動作し、TCP / IP階層モデルでは`インターネット層`で動作するプロトコル。IPは論理アドレス(IPアドレス)を各ノードに割り当てることで、各ノードを識別することができる。このIPアドレスの宛先を確認することで、あるノードから別のノードへデータを送信することができる。IPアドレスの宛先情報については`IPヘッダ`に含まれている

<img width="400" alt="" src="./images/IPヘッダ.png">

- IPヘッダのフォーマット

サイズは20バイトだが、オプションが追加された場合は最大で60バイトとなる(一般的には使用されない)。データはIPヘッダではなくIPペイロードと呼ばれる部分

<img width="600" alt="" src="./images/ヘッダフォーマット.png">

`バージョン`(4bit)  
IPヘッダのバージョン番号の情報  
`ヘッダ長`(4bit)  
IPヘッダの長さの情報。単位は32ビットであることからオプションを使用しない  
`サービスタイプ`(8bit)  
IPパケットの優先順位の情報。音声トラフィックとデータトラフィックとでは、音声トラフィックのデータを優先して送出することができる。QoS処理用  
`全長`(16bit)  
IPヘッダを含むパケットの全長(パケット長)  
`識別番号`(16bit)  
`個々のパケットを識別する`ための情報。パケットが分割された時に分割されたパケットには同じ識別番号にすることで、受信側で複数の分割されたパケットを受信した場合においても、この識別番号に基づき正しく組み立て処理できる  
`フラグ`(3bit)  
パケット分割における制御の情報  
`フラグメントオフセット`(13bit)  
フラグメントされたパケットが元のパケットのどの位置であったか示す情報  
`生存時間`(8bit)  
パケットの生存時間を示す情報。実際には、何台のルータ or L3スイッチを通過することができるかという情報。1台のルータを通過するごとにTTL値は1ずつ減らされて、TTL値が0になると、パケットは破棄される  
`プロトコル`(8bit)  
上位層(トランスポート層)のプロトコルを示す情報。ICANNによりプロトコル名と番号が定義されている。上位層プロトコルにTCPを使用する場合、このプロトコル番号は6となる  
`ヘッダチェックサム`(16bit)  
IPヘッダのチェックサムを表す情報。IPヘッダにエラーがないかチェックする  
`送信元IPアドレス`(32bit)  
32ビット(4byte)で構成された、送信元のIPアドレスの情報  
`宛先IPアドレス`(32bit)  
32ビット(4byte)で構成された、宛先のIPアドレスの情報  
`オプション`(可変長)  
通常使用されない。デバッグやテストに使用される情報  
`パディング`(可変長)  
通常使用されない。上記のオプションを使用した場合にはIPヘッダ長が32ビットの整数倍にならない場合がある。32ビットの整数倍にするために詰め物(*Padding*)として空データの0を入れることにより調整する

IPヘッダのフィールドにあるフラグ値には、以下の3ビットの組み合わせが値として入る。  
先頭ビットの0ビット目は未使用で、必ず値が0である必要がある。  
次に中央ビットの1ビット目は`パケットの分割(フラグメント)を許可する`場合は値を0にして、フラグメントを許可しない場合は1にする。  
最終ビットの2ビット目はパケットがフラグメントされた場合に使用される。`分割された最後のパケットである場合`は値を0、そうでなければ1にする<br><br>
IPヘッダのフィールドにあるプロトコルの値には以下のようなプロトコル番号が値として入ることになる。ICMPやOSPF等はIPと同じインターネット層のプロトコルだが、これらのプロトコルはIPヘッダが付加されIPパケットとして転送されることから上位層プロトコルの番号がある

|プロトコル番号|名称|
|-----------|----|
|1|ICMP(*Internet Control Message Protocol*)|
|6|TCP(*Transmission Control Protocol*)|
|17|UDP(*User Datagram Protocol*)|
|88|EIGRP(*Enhanced Interior Gateway Routing Protocol*)|
|89|OSPF(*Open Shortest Path First*)|

- IPプロトコルの特徴

3つの特徴がある。IPプロトコル自体には信頼性のあるものではない。信頼性のある通信にするかどうかは上位層に任せている。TCPを使用すれば信頼性の高いTCP / IPの通信となる。UDPを使用した場合は、信頼性のある通信にはならないが、効率性の高いUDP / IPの通信となる。  
`コネクションレス型通信`  
ネットワーク通信に際して、事前にコンピュータ間でコネクションを確立せずにいきなりデータ伝送をはじめる通信のこと。上位層プロトコルにTCPを使用すれば、コンピュータ間の通信でみればコネクション型の通信となる  
`ベストエフォート型通信`  
ネットワーク通信に際して、最善の努力(ベストエフォート)は尽くすが、十分な品質は保証しない通信のこと。上位層プロトコルにTCPを使用することで、IPを使用した通信でもパケット損失がないように見せることができる  
`階層型アドレッシング`  
IPプロトコルにより割り当てられる論理アドレス(IPアドレス)は、コンピュータが所属しているグループ(ネットワーク部)と、そのネットワークに接続されているコンピュータを識別する番号(ホスト部)の2階層により構成されている

### ARP、RARPとは

- ARP(*Address Resolution Protocol*)とは

`IPアドレスからEthernetのMACアドレスの情報を得られる`プロトコル。LANに接続されたコンピュータ間で通信するためには、IPパケットは下位のレイヤでL2ヘッダが付加された上で伝送されることからMACアドレスの情報が必要となる。しかしこれらのIPアドレスとMACアドレスは自動的な関連付けがないので、ARPでMACアドレスを得る必要がある。TCP / IPを利用したコンピュータのLAN通信では、IPアドレスとMACアドレスが分かることで通信できる

- ARPの仕組み

`ARPリクエスト`と`ARPリプライ`という2種類のパケットがある。ARPはこれら2種類のパケットを利用し、宛先となるIPアドレスを持つノードのMACアドレスの情報を得る。下図においてコンピュータAがコンピュータBと通信したいとする。その場合、先ずコンピュータAからARPリクエストを同じセグメントの全ての端末に送信するためにARPリクエストをブロードキャストする

<img width="500" alt="" src="./images/ARPリクエスト.png">

このARPリクエストのパケットの中には、`MACアドレスを知りたいノードのIPアドレス情報`が入っている。ARPリクエストはブロードキャストされるので、全ノードがこのパケットを受信するが、ARPパケットの中身を見て探索しているIPアドレスが自分（192.168.0.2）と分かったコンピュータBは、コンピュータBのMACアドレス情報をコンピュータAに伝えるために`ARPリプライのパケットをコンピュータAだけ`に送る。これでコンピュータAはBのMACアドレスを知ることができるので、LAN上での通信ができるようになる

<img width="500" alt="" src="./images/ARPリプライ.png">

ここまでがARPの基本的な動作。コンピュータAが通信したい相手のコンピュータのIPアドレスが172.16.0.1だとする。その場合、172.16.0.1と通信したいので、172.16.0.1のMACアドレスを得るためのARPリクエストではなく、デフォルトゲートウェイのIPである192.168.0.254のMACアドレスを得るためのARPリクエストを送信する。一般的には、デフォルトゲートウェイのIP = ルータのIP

<img width="500" alt="" src="./images/ARPリクエストDG.png">

- RARP(*Reverse Address Resolution Protocol*)とは

`EthernetのMACアドレスからIPアドレスの情報を得る`ことのできるプロトコル。ARPとは逆の動きのこのプロトコルは、現在ほぼ使用されていない。IPアドレスを持たないと通信できない機器であるにも関わらず、IPアドレスの設定ができない(またはIPアドレスの設定が保存できない)機器がある場合に使用される。  
このような機器は起動時には自身のIPアドレスは分からないが、インターフェイスのハードウェア部分にMACアドレス情報としては保持していることから、RARPリクエストをすることで、RARPサーバからIPアドレスを取得できる

<img width="500" alt="" src="./images/RARPリクエスト.png">

### GARPとは

- GARPとは

*Gratuitous ARP*はARPパケットの1つで、2つの役割を持っているプロトコル。  
・自身に設定する`IPアドレスが重複していないか検出する`  
・同一セグメントのネットワーク機器上のARPキャッシュを更新させる  
GARPは、自身に設定するIPアドレスに対するARPのこと。通常のARPでは宛先のIPアドレスに対して宛先のMACアドレスを得ようとするが、GARPでは自身のIPアドレスに対して自身のMACアドレスを得ようとする。上記の2つの役割を実現するため、自身のMACアドレスは知っていても自身のIPアドレスに対してARPを行う必要がある

- 役割1 : IPアドレスの重複を検出する

自身にIPアドレスがアサインされる際に他のホストが同じIPアドレスを使用していないかどうかを確認できる  
1.GARPによるARPリクエストを送信  
2.既にアドレスが使用されているため、伝えるためにGARPを送信したホストにARPリプライを送信  
3.GARPに対する送信があったため、IPアドレスの重複を検知

<img width="500" alt="" src="./images/GARP重複検出.png">

- 役割2 : ARPキャッシュの更新

同一セグメントのネットワーク機器上のARPキャッシュを更新することで、VRRPやHSRP(ルータ)などで切り替わりが発生した機器とすぐ通信できるようにするためにも使用される

<img width="500" alt="" src="./images/GARPキャッシュ更新.png">

|    |説明|
|----|---|
|ARP |相手のIPアドレスからMACアドレスを得る|
|RARP|MACアドレスから自身のIPアドレスを得る|
|GARP|自身のIPアドレスが重複していないか検出する<br>同一セグメントのネットワーク機器上のARPキャッシュを更新させる|

### ARPキャッシュとARPテーブル

- ARPキャッシュとARPテーブル

1度、ARPリクエストとARPリプライによりARPの情報がやりとりされると`ARPキャッシュ`として一定時間情報が残る。その場合、ARPリクエストとARPリプライの通信をすることなくLANでの通信が可能。ARPのキャッシュ情報は、ARPテーブルに保存され、PCの場合は`arp -a`で情報を確認できる。削除したい場合は`arp -d`で削除できる。  
・`インターフェイス`  
自身のIPアドレス情報  
・`インターネットアドレス`  
通信の宛先となるIPアドレス情報  
・`物理アドレス`  
通信の宛先となるMACアドレス情報(ARPリクエストにより得られた情報)  
・`種類`  
arpが動的に学習された場合は`dynamic`、手動で登録した情報は`static`

<img width="500" alt="" src="./images/arpテーブル.png">

Cisco機器でARPテーブルを確認する場合は`show ip arp`を入力する。Ciscoルータでは自分自身のIPアドレスとMACアドレスの情報もARPテーブルにのせる。CiscoルータのインターフェイスのIPアドレスは192.168.0.254となるが、自身の情報はARPのキャッシュ時間の項目(Age)のところに`-`が表示される。このキャッシュは消えない。Cisco IOS SoftwareのARPエントリのタイムアウトはデフォルトで240分となっている

<img width="500" alt="" src="./images/arpテーブル_cisco.png">

- ARPパケットのフォーマット

IPと同様にOSI参照モデルの`ネットワークで動作する`、IPパケットではなく`ARPパケット`である、`イーサネットフレームにカプセル化(L2ヘッダの付加)される`

<img width="500" alt="" src="./images/イーサネットフレーム.png">

### プロトコル番号一覧表

- プロトコル番号(*IP Protocol Number*)

上位層のプロトコルを識別するための番号であり、IPヘッダの8ビットの情報。プロトコル番号が6の場合はTCP、17の場合はUDP。プロトコル番号の枠は0 ~ 255

<img width="600" alt="" src="./images/ヘッダフォーマット.png">

- Ciscoで使用する拡張ACL

拡張ACL(100 ~ 199)の`permit / deny`の後に、`ip`または`ipプロトコル番号`を入力することで、Ciscoの拡張ACLを定義できる。Cisco機器ではIPプロトコル番号を入力する代わりにキーワードを指定できる。TCP上でアクセス制限をかけたい場合、6でもTCPでも可能  
1.`access-list 100 permit tcp any any eq www`  
2.`access-list 100 permit 6 any any eq 80`  
どちらも同じ効能のACLだが、1が一般的

### ICMPとは

- ICMP(*Internet Control Message Protocol*)とは

IPプロトコルのエラー通知や制御メッセージを転送するためのプロトコル。TCP / IPが実装されたコンピュータ間で、通信状態を確認するために使用され、インターネット層(OSI参照モデルのネットワーク層)で動作する。ネットワーク診断プログラムの`ping`や`traceroute`はICMPプロトコルを使用したプログラム

- ICMPのフォーマット

ネットワーク層で動作するとはいえ、正確にはIPプロトコルの上位に位置しており、タイプ・コード・チェックサム・データの4フィールドで構成される

<img width="500" alt="" src="./images/ICMPフォーマット.png">

`タイプ`(8bit)  
ICMPメッセージの機能タイプの値が入る  
`コード`(8bit)  
ICMPメッセージの詳細な機能コードの値が入る  
`チェックサム`(16bit)  
エラーがないかチェック  
`データ`(可変長)  
ICMPのタイプにより長さが異なる

- ICMPの2種類のメッセージ

`Query`(問い合わせ)  
あるノードから特定のノードに対する通信状態を確認できる。これを利用した代表的な通信プログラムがpingとtraceroute  
`Error`(エラー通知)  
ノード間の通信で経路途中でパケットが廃棄されたりした場合に、その原因を送信元のノードにエラー通知する

### ICMP・pingコマンド

- ping(ピング)

ICMPプロトコルを使用したネットワークの診断プログラム。PCやネットワークの機器等で実際に使用するコマンドもpingという単語を使用する。宛先となるノードと通信できるか(IPの到達性があるか)どうかを確認することができる。pingが成功すれば送信元と宛先とでIPの通信に問題がないことが証明され、pingが失敗すればIP上の通信に問題があると判断できる

- Windowsのping : *Reply From*(からの応答)

Windowsのコマンドプロンプトでのpingの実行結果(`ping 192.168.0.1`)。192.168.0.1にpingが成功しており、pingコマンドを実行したPCとIPで通信できることを証明している。Windowsの場合はデフォルトで4回ICMPメッセージが送信される

<img width="600" alt="" src="./images/pingReplyFrom.png">

`Reply from`  
pingが成功した場合に表示  
`byte = 32`  
ICMPパケットのデータサイズ。デフォルトでは32byteのデータが送信  
`time < ims`  
ICMPパケットの応答までに経過した時間。今回は1ミリ秒要したということ  
`TTL = 128`  
何台のレイヤ3機器を通過することができるか示す。この場合は残り128台通過できる

- Windowsのping : *Request time out*(要求がタイムアウトしました)

宛先の機器と通信できないことを意味している。ただし、WindowsファイアウォールのようにICMPだけをブロックしているケース、経路途中にあるルータなどでセキュリティ上の観点でICMP通信を制限しているケースなどでは、このメッセージが出力されても`通信できないのはICMPの通信だけ`であり、TCP / IPにおける通信が問題ないケースがある

<img width="600" alt="" src="./images/pingRequestTimeOut.png">

- Windowsのping : *Destination net unreachable*(宛先ネットワークに到達できません)

この場合はpingが失敗している。`ping 172.16.0.1`の結果、172.16.0.1からの応答はないが、代わりに192.168.0.254が応答している状態。ルータまではパケットが到達したがルーティング情報がなかったため、宛先ネットワークに対してパケット転送ができなかったことを示している

<img width="600" alt="" src="./images/pingDestinationNetUnreachable.png">

- Windowsのping : *Destination host unreachable*(宛先ホストに到達できません)

この場合もpingが失敗している。この場合はルータまでは到達して宛先のネットワークへの到達性があるが、該当する機器(ホスト)に到達できないことを意味する。またはホストへのルーティングが見つけられないことを意味する

<img width="600" alt="" src="./images/pingDestinationHostUnreachable.png">

- Windowsのping : *TTL expired transit*(転送中にTTLが期限切れになりました)

TTLの超過によるエラーをルータからpingを実行したホストに伝達している。つまり、宛先にパケットを届けるために128台以上のルータ(L3)を経由することが必要となった結果、途中でパケットが廃棄されたことを意味する。しかし、このエラーメッセージが出力される場合にはルータ側の設定ミスによるルーティングループが原因であることがほとんど

### ICMP・pingオプションコマンド

- pingのオプション

`ping + 宛先IPアドレス`の後にオプションの値を追加できる。`ping`とだけ入力すればオプションの情報が出力される  
ネットワーク通信試験を実施する際にpingを打ち続けたい場合は`-t`のオプション値を入力すると実現でき、Ctrl + Cで停止する。macOSの場合は`ping -option 宛先IPアドレス`の順で指定。よく使用されるオプションは以下の通り

|オプション|値|説明|
|--------|--|---|
|-t      |なし|中断されるまで指定したホストにpingを実行し続ける|
|-l      |なし|ICMPパケットのデータ部のサイズ。デフォルトは32バイト|
|-f      |なし|IPパケットの分割(フラグメント)の禁止。デフォルトは許可されている|

- pingの1回目が"Request timed out."の場合

1回目のICMPエコー要求が失敗しているが、その後の全てのエコー応答が得られている場合。このような現象は異なるネットワーク間でpingを行う際に、ホストでルータのARP情報が得られていない時に発生する。  
異なるネットワークと通信する場合、ホストはルータのMACアドレス情報が必要となるのでルータに対しARPを実行する。MACアドレス情報が得られれば、デフォルトゲートウェイへパケットを送出しはじめるが、このARPを行っている間にPINGが失敗してしまう。異なるネットワーク間の通信でも1度ルータのARP情報を得て、ARPテーブルにキャッシュされている限りはこのような現象は発生しない

<img width="600" alt="" src="./images/ping失敗例.png">

- pingの宛先アドレスにドメイン名を指定

宛先IPアドレスを実行する方法だけでなく、ドメイン名を指定しても実行できる。前提として、PC側で正しくDNSサーバのアドレスを指定していることが必要。DNSサーバで名前解決(ホスト名からIPアドレスを導出する)が行われている

### ICMP・tracerouteオプションコマンド

- tracerouteとは

ICMPプロトコルを使用したネットワークの診断プログラム。使用するコマンドはWindowsの場合は`tracert`、Cisco機器の場合は`traceroute`というコマンド。あるホストから宛先のホストまでに到達するためにどのネットワーク経路を使用しているのかが分かり、宛先ノードまでのネクストホップアドレス(ルーティングで次にパケットを転送する隣接ルータのIPアドレス)の一覧が表示される

- tracerouteが使用するプロトコル

ICMPプロトコルまたはUDPプロトコルのどちらかを使用する。Windowsのtracertは`ICMPプロトコル`を使用するが、LinuxやCisco IOSでは`UDP`を使用する。OSにより使用プロトコルは異なるので、Windowsでのtracertと、Ciscoでのtracerouteの結果が異なる場合がある

- tracerouteの仕組み

ホストAからホストBに`tracert 192.168.3.100`を実行する。tracerouteではICMPパケットを送る際に、IPヘッダのTTL値を`1`にすう。1台目を経由するルータがこのパケットを受信するとTTL値が`0`になり、ルータはパケットを破棄して*ICMP Time Excceed*のメッセージを送信する。このICMPメッセージを受信することでホストAは、宛先の機器に到達するまでに経由するルータが分かる(通過する伝送経路が分かる)ため

<img width="500" alt="" src="./images/tracert1.png">

次に、ホストAはTTL値を1増やして`2`にする。ルータAでTTL値が1つ減らされ、ルータBでTTL値が1つ減らされるので、今度はルータBの時点でTTL値が`0`になる。今度はルータBが*ICMP Time Exceeded*のメッセージをホストAに送信する。これにより、2台目の経由ルータは`192.168.2.2`と分かる。これによりホストAは、ルータAとルータBまで通信できると分かる

<img width="500" alt="" src="./images/tracert2.png">

今度はTTL値をさらに1つ増やして`3`にする。ルータAで、TTL値が1つ減らされて、ルータBでTTL値が1減らされるが、残り1のTTL値は減らされないのでホストBにICMPエコー要求が到達する。ICMPエコー要求を受信したホストBは、ホストAにICMPエコー応答のメッセージを送信する。ICMPエコー応答を受信したホストAは、tracertによるICMPパケットの送出をこれで終了する

<img width="500" alt="" src="./images/tracert3.png">

ホストAでtracertの実行結果。応答時間(ミリ秒)が3回表示されているが、Windowsでは1回のtracertの実行につき、デフォルトで3回パケットが送信されるので、3回の応答時間が表示されている。表示されているアドレスは宛先機器までの伝送経路にある経由するルータのアドレス。ここに表示されているアドレスがネクストホップアドレスだが、最後のIPアドレスは宛先機器であるホストBとなる

<img width="500" alt="" src="./images/tracert_result.png">

### pingが1回目だけ失敗する原因

- pingが1回目だけ失敗する(応答しない)原因

1度目はICMPエコー応答を得られず、その後は成功する場合がある。この現象は、`異なるネットワーク間の通信において、ホスト側でルータのMACアドレスが得られていない場合`に発生する。つまり、MACアドレス情報が得られていない場合に発生する。  
異なるネットワークのデバイスと通信する場合、ホスト側でルータのMACアドレス情報が必要となるため先ずルータに対してARPを実行する。MACアドレス情報が得られてARPテーブルができればデフォルトゲートウェイ(ルータ)に対してパケットを送出し始めるが、ARPを実行している間にその最初の1回はpingが失敗してしまう。最初の1回目が失敗するのは障害や問題があるわけではない。異なるネットワーク間の通信でも、`ARPテーブルに通信先となるMACアドレス情報がキャッシュされている限りこのような現象は発生しない`

<img width="500" alt="" src="./images/pingTimeOut.png">

### pingの応答がない際の8つの確認事項

- pingが返ってこない原因、問題の特定方法

問題特定はpingを実行する`送信元から(近くから)`確認していくことが重要。Step0 ~ 8の順で確認していくことによって、どこに障害ポイントがあるのかを特定できる

- step0 : *Windows Defendor*ファイアウォールが無効(オフ)になっているかを確認

前提として、PING通信を実行するWindows OSでデフォルトで有効化されている、*Windows Defendor* / *Microsoft Defendor*ファイアウォールが無効(オフ)になっていることを確認。pingが失敗する問題を特定しやすくするために、送信元と宛先の両方で無効化し双方向にpingが通る状態にする

- step1 : PCのLANポート(NIC)がリンクアップしているか確認

1.有線LANの場合、PCのNICに`LANケーブルが接続`されているか  
2.無線LANの場合、PCの`ワイヤレス(機能)がON`になっているか  
3.PCのLANアダプタの設定は`有効`か  
4.有線LANの場合、PCの接続先となるL2スイッチなどのポートの`LEDは緑`になっているか

- step2 : PCの有線LAN、無線LANの両方がリンクアップしていないことを確認

ネットワーク機器で適切なルーティングが行われている場合、PC側で有線LAN、無線LANの両方が有効であっても問題なく通信可能だが、pingが通らない場合は障害ポイントを特定するためにも、PC側で`有効にするのは、有線LANまたは無線LANのどちらかだけ`にする

- step3: PCに設定しているIPアドレスが正しいかどうかを確認

PCの有線LAN・無線LANに設定している、IPアドレス・サブネットマスク・デフォルトゲートウェイの設定情報が正しいかどうか、`ipconfig`コマンドで確認する

<img width="500" alt="" src="./images/pingCheck.png">

- step4 : PCに設定されたデフォルトゲートウェイからping応答があるか確認

PCに設定されたデフォルトゲートウェイとなる`L3スイッチまたはルータのIPアドレス`に、pingを実行する。ICMPをブロックしたACLのフィルタリング設定がなければping応答があるはず。デフォルトゲートウェイからping応答がない場合、以下の原因が考えられる。  
原因1 : PCに設定しているIPアドレス・サブネットマスクの値が間違っている  
原因2 : デフォルトゲートウェイとなるL3デバイスのIPアドレスの設定が間違っている  
原因3 : デフォルトゲートウェイとなるL3デバイスのLANポートを有効化(*no shutdown*)していない  
原因4 : デフォルトゲートウェイとなるL3デバイスのLANポートに、ストレート(クロス)ケーブルの適切なLANケーブルが接続されておらず、ポートLEDがグリーン状態ではない  
原因5 : PCとL3デバイスの間に位置するL2スイッチで、適切なVLAN設定がされていない

- step5 : 宛先デバイスの宛先IPアドレスにtracertを実行

コマンドプロンプトでtracertを実行すると、宛先デバイスに到達するまでの経路において、どこのL3デバイスまで設定通りの正常なルーティングができているのかを確認できる。上図構成で、PCから宛先デバイスのIPアドレスにtracertを実行すると、`説明図の赤丸部分のIPアドレス`がコマンドプロンプトに表示されていく。  
宛先デバイスへ到達するまでに経由するL3デバイスでACL設定がない場合は、宛先デバイスへ到達するまでに経由するL3デバイスのIPアドレスが表示されるので、応答がないL3デバイスを特定できる。図の構成では、192.168.1.254, 192.168.2.254, 192.168.3.254, 192.168.4.10の順で正常時にtracertの結果が出力される

- step6 : 正常にルーティングできていないL3デバイスの確認

tracertにより正常にルーティングできていないL3デバイスを特定できた場合は、そのL3デバイスのルーティングテーブルの内容を確認する。図の構成では、ルーティングテーブルで以下2つのルートが存在するかどうかを確認する。  
・送信元デバイスのネットワーク(192.168.1.0 / 24)  
・宛先デバイスのネットワーク(192.168.4.0 / 24)  
宛先ネットワークのルート情報だけではなく、送信元ネットワークのルート情報も必要。ルーティングでは`「行きのルート」と「帰りのルート」を双方向で必要`。ダイナミックルーティングの場合は動的にルート情報がやり取りされるが、スタティックルートの場合は要注意。また、ネクストホップのIPアドレスが正しいかも確認

- step7 : 宛先デバイスとなるサーバ側で、NICやIPアドレスの設定に問題ないか確認

・サーバのNICにLANケーブルが接続されているか  
・サーバのNICのLANアダプタの設定は`有効`か  
・サーバの接続先(L2スイッチなどの)ポートでは、`ポートLEDがグリーン`か  
・サーバのIPアドレス・サブネットマスク・デフォルトゲートウェイは`正しい設定`か  
・サーバ側でiLO専用ポート以外に、データ通信用NICに複数のLANケーブルが接続されている場合、可能であるなら問題切り分けのためにケーブルを抜いて、1つのリンクだけにしてみる  
・サーバからデフォルトゲートウェイ(192.168.4.254)にpingを実行し応答があるかどうか確認  
・サーバ側にWindowsファイアウォールなどでICMPがブロックされていないかどうか確認。ルータなど同じセグメントにいるデバイスから、サーバへpingを打って確認することができる

- step8 : サーバ側に`route add`が設定されていないか確認

PCやサーバではルーティングテーブルを持っており、内容はコマンドプロンプトで`route print`と入力して確認できる。  
`route add`コマンドの設定により、GUIでのTCP / IPの設定以外にコマンドプロンプト上でスタティックルートを定義できる。意図しないスタティックルート(*route add*)がある場合はそれを削除する

### TCPとは

- TCP(*Transmission Control Protocol*)とは

IPと同様にインターネットにおいて標準的に利用されているプロトコルで、IPの上位プロトコルで`トランスポート層で動作`する。ネットワーク層のIPとセッション層以上のプロトコルの橋渡しをする形で動作している。TCPは`信頼性の高い通信を実現する`ために使用されるプロトコルに対して、UDPは信頼性は高くないが高速性やリアルタイム性を求める通信に使用されるプロトコル。優劣はなく、通信特性によりTCP / UDPは使い分けされる

- ポート番号とは

コンピュータが通信を行うために通信先の`アプリケーションを特定するための番号`のこと。コンピュータ間の通信で通信する宛先のIPアドレスが分かれば、そのIPアドレスにデータを送信できるが、そのデータを受信したコンピュータが、どのアプリケーションでそれを受信するのか判断するために必要。  
コンピュータ上でWebブラウザとメーラを起動させていたとする。Webブラウザでインターネットを閲覧するために、コンピュータ内でデータをWebブラウザに送り届ける必要がある。TCPヘッダにポート番号情報が付加することで、どのアプリケーションなのかを識別し、これを実現している

<img width="500" alt="" src="./images/portNumber.png">

ポート番号は0 ~ 65535(2の16乗)の範囲で割り当てられ、以下の3つに分類される  
・ウェルノウンポート番号(0 ~ 1023)  
IANAで管理。`サーバのアプリケーション`に割り当てられる  
・登録済ポート番号(1024 ~ 49151)  
IANAで管理。`独自に作成したアプリケーション`に割り当てられるポート番号  
・ダイナミックポート番号(49152 ~ 65535)  
クライアント側のアプリケーションに`動的に割り当てられる`ポート番号

クライアントPCがWebページを開こうとすると、クライアントPCは宛先ポート番号を`80`と指定しアクセスを行う。WebブラウザとWebサーバのデータ送受信はHTTPプロトコル(ポート番号80)を使用。  
サーバからのWebページの情報を得るためには、クライアントPC側にもポート番号が必要。このポート番号はアプリケーションがランダムに動的に割り当てる。今回はWebブラウザが自身のポート番号を`54321`と割り当てたので、それを自身のポート番号(送信元ポート番号)としてパケットを送出する。  
WeサーバがクライアントPCからのパケットを受信すると、送信元IPアドレスと宛先のポート番号の情報が分かるので、この送信元の情報を宛先の情報にしてデータを送り返すことで双方向の通信が成立する。  
クライアントPCが複数のWebブラウザを開いている場合、そのWebブラウザごとに正しくWebページを表示させるためにWebブラウザごとに異なるポート番号がランダムに割り当てられる

<img width="500" alt="" src="./images/portNumber2.png">

### TCPとは - TCPヘッダ

- TCPヘッダのフォーマット

TCPセグメント(TCPパケット)はTCPヘッダとTCPペイロードで構成される。TCPペイロードはTCPより上位のプロトコルを含むデータのこと

<img width="500" alt="" src="./images/TCP_format.png">

`送信元ポート番号`(16bit)  
`宛先ポート番号`(16bit)  
`シーケンス番号`(32bit)  
送信したデータの順序を示す値。「相手から受信した確認応答番号」の値  
`確認応答番号`(32bit)  
「相手から受信したシーケンス番号」 + 「データサイズ」  
`データオフセット`(4bit)  
TCPヘッダの長さを示す値  
`予約`(3bit)  
全ビット`0`が入る。将来の拡張のため用意されている  
`コントロールフラグ`(9bit)  
1ビットずつのフラグに分類されており、`1`(フラグを立てる)場合に意味をなす  
`ウィンドウサイズ`(16bit)  
受信側が一度に受信できるデータ量を送信側に通知するために使用される。送信側は、この値のデータ量を超えて送信することはできない  
`チェックサム`(16bit)  
TCPヘッダとデータ部分のエラーチェックを行うために使用される値が入る  
`緊急ポインタ`(16bit)  
コントロールフラグのURGの値が`1`である場合にのみ使用されるフィールド。緊急データの開始位置を示す情報が入る  
`オプション`(可変長)  
TCPの通信において、性能を向上させるために利用する。TCPコネクションの際に、MSSを決定するために使用されたりする  
`パディング`(可変長)  
TCPヘッダの長さを32ビットの整数にするために詰め物(*Padding*)として空データの`0`の値を入れることにより調整する

`NS` : ECN-nonce輻輳(フクソウ)保護を示す  
`CWR` : 輻輳制御ウィンドウ縮小(*Congestion Window Reduced*)を示す  
`ECE` : ECN-Echoを示す。SYNフラグがセットされている場合、ECNが利用であることを意味する  
`URG`(*Urgent*) : 緊急に処理すべきデータが含まれていることを示す  
`ACK`(*Acknowledgement*) : 確認応答番号のフィールドが有効であることを示す。コネクション確立時以外は値が`1`  
`PSH`(*Push*) : 受信したデータをバッファリングせずに、即座にアプリケーション(上位)に渡すことを示す  
`RAT`(*Reset*) : コネクションが強制的に切断させることを示す  
`SYN`(*Synchronize*) : コネクションの確立を要求することを示す  
`FIN`(*Fin*) : コネクションの正常な終了を要求することを示す  
これらのTCPヘッダの値はTCPの特徴の`ポート番号を使用`、`コネクションの確立、維持切断`、`順序制御`、`再送制御`、`ウィンドウ制御`、`フロー制御`で生かされている

### TCPとは - 3ウェイハンドシェイク

- TCP - コネクションの確立と切断

TCPはコネクション型プロトコルであるため、データ転送を行う前にコネクションの確立を行い(`3ウェイハンドシェイク`)、3回のやり取りによって確立される

<img width="400" alt="" src="./images/3ウェイハンドシェイク.png">

1.ホストAからホストBに`コネクション確立の要求`をする。つまり、TCPヘッダの中にある`SYNビットは1`であり`ACKビットが0`であると分かる。シーケンス番号は最初だけはランダムな値が割り当てられる。今回は例として0とする。確認応答番号(ACK番号)は通信の開始にはない<br><br>
2.ホストBはホストAの`コネクション要求に応える`。そしてホストBからもコネクション確立を要求する。つまり`SYNは1`、`ACKは1`であると分かる。シーケンス番号は最初だけランダムな値が割り当てられる。例えば0として、確認応答番号は`相手から受信したシーケンス番号` + `データサイズの値`で、3ウェイハンドシェイクでは`相手から受信したシーケンス番号`に1を加算した値となる<br><br>
3.ホストAもホストBからの`コネクション要求に応える`。`SYNは0`、`ACKは1`であると分かる。シーケンス番号は`相手から受信した確認応答番号`なので1となり、確認応答番号は2と同じ考え方なので相手から受信したシーケンス番号に1を加算した値となる

以下は、3ウェイハンドシェイクにより確立したコネクション上でデータをやり取りする時の流れ

<img width="400" alt="" src="./images/データ転送.png">

4.ホストAからホストBに100byteのデータを送る場合。データ転送では3ウェイハンドシェイク後のシーケンス番号と確認応答番号の値が引き続き使用される。そのためシーケンス番号、確認応答番号ともに1<br><br>
5.ホストBからホストAに1300byteのデータを送る場合。シーケンス番号は`相手から受信した確認応答番号`を使用するので`1`となる。確認応答番号は`相手から受信したシーケンス番号`にデータサイズを加えた値なので`101`となる

データのやり取りが完了してから通信を終了(コネクションの切断)をする場合は、4回のやり取りが必要

<img width="400" alt="" src="./images/コネクション切断.png">

8.ホストBからホストAへのデータ送信が完了したため、ホストBは`コネクションの切断要求`を送信、FINビットが`1`となる。7でホストBからAへデータを送信した後、さらに連続してホストBからAへパケットを送信している。この場合のシーケンス番号は`直前の自分のシーケンス番号` + `自分が送信したデータサイズ`となる。シーケンス番号は`2601`、確認応答番号はそのまま`301`<br><br>
9.FINを受信したホストAはコネクションの`切断要求への確認応答`をする。シーケンス番号は`相手から受信した確認応答番号`だから`301`となり、確認応答番号は今回のようにFINへの確認応答の場合`相手から受信したシーケンス番号`に`1`を加算し、`2602`<br><br>
10.ホストAもコネクションの切断要求を送信。シーケンス番号と確認応答番号はそのまま<br><br>
11.FINを受信したホストBはコネクションの`切断要求への確認応答`をする。シーケンス番号は`相手から受信した確認応答番号`で`2602`となり、確認応答番号は、今回のようにFINへの応答確認の場合は`相手から受信したシーケンス番号`に`1`を加算して`302`となり、TCPコネクションが正常に終了する

- TCP - 3ウェイハンドシェイクの際に伝えられるMSS(*Maximum Segment Size*)について

3ウェイハンドシェイク時に伝えられる情報の、TCPヘッダのオプションで、1つのTCPパケットで運ぶことができるデータ量を指す。一般的なTCP / IP環境での最大サイズのEthernetフレーム(1518byte)では、MSSは1460バイトになる。つまりホストAからホストBに14,600バイトバイトのTCPデータを送信したい場合、10回のTCPパケットを送出する

<img width="500" alt="" src="./images/MSS.png">

### TCPとは - 順序制御、再送制御

- TCP - 信頼性の確保(順序制御)

TCPでは全てのTCPパケットにシーケンス番号を付与する。以下ではホストAからホストBに対して2,600byteのデータを分割して送信している。TCP / IPでは、1度に送信できるデータのサイズ(MSS)に上限があるためデータの分割を行う必要がある。以下では1300バイトずつに分割しているが、`シーケンス番号の値があるため`受信側はもとのデータを正しく組み立てられる

<img width="500" alt="" src="./images/sequence.png">

- TCP - 信頼性の確保(再送制御)

TCPでは、データ送信の度に無事にデータが到達したことを知らせるために確認応答(ACK)を行う。ホストAからBへデータを送り無事に届けば、ホストBはAに確認応答としてACKビットを立てて返信する。または、この確認応答がない場合はネットワーク伝送経路の途中でパケットが破棄された可能性がある。TCPでは一定時間内にACKが帰ってこない場合は、パケットが破棄されたと判断して再度データを送る

<img width="500" alt="" src="./images/再送制御.png">
