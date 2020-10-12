# GitHubの運用手順についての提案
以下に記載するのはGitHubフローやWIP(Work In Progress)パターンを基本とした運用手順ですが、一つの作業に対しての工程が多く、工数が必要となるのは確かです。このような手順を省いた場合と同様の開発スピードを維持するためには、gitそのものやgitクライアント(SourceTree等)に関する**知識や経験**が不可欠です。文章を書くことに不慣れな人は、他の人よりさらに工数が必要になるかも知れません。

目先の開発スピードを優先しなければならない場面があることは事実です。しかし、「やったことが無いから」「時間がかかるから」と**避けている限り、いつまで経っても知識や経験は身に付きません**。つまり、「できない人」「作業が遅い人」のままです。

最初はしんどいかも知れませんが、時間に余裕のある学生のうちに慣れ親しんでおくことをお勧めします。

## 1. 機能追加やバグ改修等、必要な作業が出てきた時点でIssueを作成する
着手するか否かを問わず、どんどんIssueを作成しましょう。基本的に、ここで作成するIssueが今後の作業の大元となります。  
このように、GitHubのIssueやRedmineのチケットを作業開始のきっかけにするような開発手法を「TiDD(Ticket Driven Development)」「チケット駆動開発」と呼びます。

### Issueの内容について
* **Issueはプログラマ目線で書かないよう心がける**  
Issueはプログラマ以外のチームメンバや一般ユーザも読み書きすることを前提とし、コードの詳細に言及することは極力避け、仕様や機能やバグの内容を書くよう心がけましょう。  
実装内容の詳細についてはIssueではなく、Pull Request(以下、PRと表記)に記載すべきです。

* **背景をきちんと書く**  
バグや質問のIssueの場合はあまり問題にならないのですが、特にプログラマが仕様変更や機能追加に関するIssueを起票する場合、そのIssueに至った背景の記載が抜けがちです。時間が経ってからそのIssueを見たときに「そもそも、なぜそのような機能を追加する必要があったのか」ということがわからなくなり、困ることが多いです。  
背景がわかるよう、丁寧に書くことを心がけましょう。

## 2. 作業を開始するタイミングでfeature branchを作成し、PRを作成する
Issueに関するコーディングに着手することが決定したらPRを作成しますが、GitHubではPRを作成するにあたり
* feature branchが存在すること
* merge先のbranchとfeature branchの内容に差分が存在すること

という制約があるので、この時点で
1. feature branchを作成する
1. 作成したブランチに空コミットを行い、pushする

という作業が必要です。  
SourceTreeのGUIからは空コミットを作成できないようなので、ターミナルからコマンドを入力する必要があります。  
決して難しい作業ではありません。慣れましょう。

README.mdの更新等の細かい作業であれば変更をpushしてからPRを作成しても良いかもしれませんが、この方法を採ると手間を減らせる代わりに後述の『PRをレビューしてもらう』という機会を失いますので注意しましょう。

### PR作成時の注意点
* **作業中(未完成)であることがわかるようにしておく**  
作成したPRは[Draftに設定](https://github.blog/jp/2019-02-19-introducing-draft-pull-requests/)しておき、mergeされないようにしておきましょう。  
privateリポジトリの場合はDraft機能が使えないようなので、その場合はタイトルの先頭に`[WIP]`と記載しておく等、運用でカバーしましょう。  
このようにしておくことで、「このPRは目下作業中であり、正式にコードレビューを依頼出来る状態に無い」ということがわかります。

* **どのIssueに対するPRなのかを設定する**   
PRを作成しコーディングに着手したということは**そのPRの元となるIssueが存在するはず**ですが、IssueとPRを紐付けておかないとどのIssueに対する作業なのかがパッと見てわからないので不便です。  
まず、PRのタイトルの末尾に`(#123)`のようにIssue番号を記載しておくと、PR一覧を表示させたときに探しやすいです。  
ただし、タイトルに記載されたIssue番号をハイパーリンクにする機能がGitHubには無いようなので、元となるIssueをPRの設定項目からも指定しておきましょう。

## 3. PRに記載した実装方針に問題が無いか、第三者にレビューしてもらう
熟練者であれば作業内容が確定した時点で実装方針もほぼ決まることが多のですが、初学者はここで手が止まることが多いです。「どうすればその機能を実現できるのかがわからない/思いつかない」という状態ですね。学生の進捗遅れの原因の大部分は、このフェーズでのタイムロスです。自分で考えることは大切ですが、埒が明かなければ早めにクラスメイトや先生に助けを求めましょう。

## 4. 方針に問題が無ければコーディングを開始する
PRに書いた実装方針で問題無さそうならコーディングに着手します。最低でも一日に数回commit・pushを行い、細かく作業を進めましょう。

ここで細かく分割してコミットする理由はいくつかありますが、大きなものを列挙しておきます。
1. **不具合発生時に追跡しやすくするため**  
不具合が発生した際、「その不具合はどのコミットで混入したのか」や「何のためにそのコミットを行ったのか」等を調査する作業は日常的に発生します。コミットを小さくしておけばそのコミット自体を元に戻してしまうだけで済む等、修正が容易です。逆に、コミットが大きいとこれらの作業が大変になります。

1. **作業状況を第三者に知らせるため**  
PRの作業が終わるまでローカルで作業を進めていると、その間作業が進んでいるのかどうかが第三者からは全くわかりません。細かくpushすることで、「ちゃんと進めていますよ！」ということを周囲に伝えましょう。
WIPパターンの主な目的は **『作業の見える化』** です。

1. **コミットの整理をしやすくするため**  
後述しますが、コードレビューの完了後PRを終了する前にコミット履歴の整理作業をします。このとき、各コミットの粒度が大きすぎると非常に作業しづらいです。小さいコミットを纏めるのは容易ですが、大きいコミットを分けるのは大変です。レビューでの指摘是正は必然的に細かいコミットになりがちですが、これらと意味合い的に同じコミットをくっつけようとしたときにレビュー前のコミットが大きいと「コミットの分割」→「コミットの並べ替え」→「コミットの結合」という手順となり、本来の目的である並べ替えと結合に取り掛かるまでに分割作業で消耗する羽目になります。

## 5. 作業が完了したタイミングでDraftを外し、レビュワーにコードレビューを依頼
コーディングが完了したら、次はレビュワーにコードをレビューしてもらうフェイズです。レビューの依頼方法はプロジェクトごとに決めておけば良いですが、PRに設定していた『Draft』の設定を解除すればその旨がSlackに通知されますので、この通知を利用するのも良いと思います。正式にレビューを依頼する旨を、PRのメッセージ欄からレビュワーにメンションするのも丁寧で良いですね。

## 6. 質問への回答や指摘事項の是正→是正確認依頼を繰り返す
レビュワーは自身の作業とは別に、わざわざ時間を割いてあなたのコードを読んでくれています。指摘や質問のコメントは、そこからさらに時間を割いて書いてくれたものです。  
質問には回答しましょう。  
指摘に対しては、対応するかどうかを返信しましょう。  
コメントの意味や意図がわからなければ質問しましょう。  
自分からレビューを依頼したにも拘わらずレビュワーからのコメントを放置する、などという失礼極まりないことをしてはいけません！

指摘を受けコードを是正した場合は、是正確認を指摘者に依頼してください。

上記いずれのやり取りも、PRのレビューコメント機能及びReply機能をうまく使えば話題が発散しなくて便利です。また、コードのどの部分の話をしているのかもCommitへのコメントやIssueでやり取りするよりもPRを利用した方が断然わかりやすいです。

## 7. featureブランチでの作業内容がapproveされればmerge可能になるので、コミットを整理
レビューでの指摘を是正し是正確認もしてもらえたのであれば、そのfeatureブランチはmergeしても問題無い内容になっているはずです。  
ただし、この時点ではコミットの履歴が非常に細かくなっています。やろうとしたことは変わっていなくても途中でミスに気付いて修正をしたせいで飛び飛びの別のコミットになっているものがあるかも知れません。レビュワーから指摘された誤字の修正だけのコミットのようなものもあるかも知れません。  
そのようなコミットが枝分かれ元のブランチにmergeされるとただのノイズになってしまうので、これらはmerge前に順番を変えたり複数のコミットを一つに纏めたりして整理すべきです。

また、featureブランチで作業をしている間に別の人のfeatureブランチが本線にmergeされ、枝分かれ元のコミットが作業開始時点よりも進んでいることも多々あります。下手をするとそのせいでmergeしようとすると競合が発生する場合もあります。  
レビュワーがPRのMergeボタンをクリックするだけでmergeできるようrebaseを行い、merge先ブランチの最新のコミットから枝分かれして作業を開始したかのような履歴にすべきです。これは、mergeコミットを作成するかどうかとは別の問題です。その際に発生する競合等を解決するのは、レビュワーではなくレビューイの責務です。

## 8. レビュワーにmergeしてもらい、PRをclose
コミットの整理及びmerge先の先頭コミットへのrebaseが終われば、レビュワーにmergeしてもらい、PRを閉じてもらいましょう。featureブランチは用済みですので、これもPRの画面から削除してもらうと良いでしょう。  
PRのcloseと同時にIssueもcloseできる場合はそのように設定しておくと自動的にIssueがcloseされるので便利です。

これでPRでの作業が終了しました。お疲れさまでした。  
一息ついたら、次のPRを出して作業に着手しましょう！！
