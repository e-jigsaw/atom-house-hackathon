gitでの一般的なフロー
===============

ひと通り作業にケリがついたらコミットしよう。
コミットの粒度は細かいとよい。

例えば以下の作業内容があったとき

* {ファイル,フォルダ}を生成した
* ひとつのメソッドとそのメソッドに関するテストやドキュメンを書いた
* コメントを書いた

この3つの内容はすべて別コミットにするようにするとよい。
つまり

```
# 適宜自分のいるブランチの最新版をもってくる
git pull origin master

# ファイルを生成
touch HogeController.rb spec/controller/HogeController_spec.rb

# どのファイルを変更したか？どのファイルを作成したかを確認
git status

# すべてのファイルの差分が見たい
git diff 

# ファイルを指定してdiffを見る
# 変更したファイルがわかってる時とかコミットしたいファイルがわかっているときに使う
git diff HogeController.rb spec/controller/HogeController_spec.rb 

git add HogeController.rb spec/controller/HogeController_spec.rb

# 間違ってないか確認
git status

# ステージにあげた（addした）ファイルのdiffをみる
git diff --cached
# コミット
git commit -m 'HogeControllerとテストファイルを作成'
# pushはまだしない（後述）

# ファイル修正
emacs HogeController.rb
#テストを書かないプログラマは悪いプログラマだ。
emacs spec/controller/HogeController_spec.rb 

# コード追記したのでコミットする
git status
git diff HogeController.rb spec/controller/HogeController_spec.rb
git add HogeController.rb spec/controller/HogeController_spec.rb
git status
git commit -m '実装とテストコード書いたよ'

# ちょっとアレゲな実装があるので補足でコメントをかく
emacs HogeController.rb
git status
git diff HogeController.rb
git add HogeController.rb
git commit -m 'コメント書いた'

# コミットしたログを見る
git log
# 各コミットのファイルを見る
git log --stat

# 大丈夫そうだったらpushする
git push origin master 
```

gitでの一連のコマンドはこんな感じです。
基本的にはこれだけ覚えているとよさそう。
適宜困ったりその他なにかあればIssuesで聞くといいと思います。

# 以下補足
たくさんstatusとかdiff見てますけど、初心者的にコミットを戻したり歴史を改ざんしようとすると破滅する運命の道を歩むこともあるので、慎重にしよう。
**後ろは振り返らない。下も上も横も見ない。前だけを見よう。**

このあとの作業も基本的には繰り返しで、適宜編集を加えるとコミットします。
コミットの粒度を細かくしたのは過去に遡ったときに、たくさんのファイルをコミットしてあるコミット同士でコンフリクトが起きたりすると面倒だったりするので、まぁ細かくしておく、ちゃんと戻るときに細かく歴史を指定して戻れる方が楽だったりする。

## pushはすぐにしない理由
ひとつの機能がきちんと実装された段階でpushしよう。
毎回pushしてもいいけど、一緒に作業してる人がpullした段階でマージが発生するので注意。

## gitのお作法
gitのベストプラクティスはいろいろありますが、基本的に無茶なコミット(例えば100ファイル10000行が1つコミット)とかしない限りは思うままにコミットしまくりましょう。
コミットが細かいということはそれだけコミットメッセージが残って学習にもなります。あと単純にコミットの量が増えるからコミットへの敷居が下がるとおもう。
多分OSSに貢献するとかじゃない限りコミットメッセージは日本語でもいいし、コミットメッセージのフォーマットも適当でいいと思うし、いろんな人とgit使っているとだんだんわかってくるし、ゆるふわにgitを使いましょう。


## 応用
このコミットを細かくわける方法はブランチの中でたくさんのコミットが出てきて嫌っていう人もいるけど、自分のスキルとあわせてうまく折り合わせていこう。
どうしてもその人が気難しく「コミット細かくするのダメー」って人ならば

```
git pull origin devlop

# 新しく新機能用のブランチを切る
git checkout -b feuture

# 新機能（改修）などなど
emacs hogehoge

# 普通にコミット
git add .
git commit -m '{作成した,実装した,コメント書いた}'

# コミット終わった！
# developブランチに戻る
git co develop

# mergeするけどsquashオプションをつける
git merge --squash feature

#マージコミットにはならずに、featureブランチで作業したファイルたちがステージされた状態になる
# 普通にコミットする
git commit -m 'hogehogeっていう機能を実装した'

git push origin develop
```

こうすると、developブランチからは

```
git co develop
git log --oneline --graph
* 9d37f70 hogeの実装をしたよ。piyoを新規で実装したよ。
* 534f8f5 hoge
```
こうみえるけど、featureブランチにいくと

```
git checkout feature
git log --oneline --graph
* 041b3db vimのコマンドが入ってたので修正
* b1eb4a7 修正
* b558333 piyoな機能を実装
* d5bb90a piyoファイルを生成
* 534f8f5 hoge
```
developブランチではfeatureのコミットは見えないけど、ちゃんとfeatureブランチにはコミットが残ってる。

全体で見ると（適宜コメント書いてます）

```
yuyaMBP:git-study asonas$ git graph
* <9d37f70> 2012-05-31 [asonas]  (HEAD, develop) piyoを新規で実装したよ。
| * <041b3db> 2012-05-31 [asonas]  (feature) vimのコマンドが入ってたので修正 #ここがfeatureブランチでの最後のコミット
| * <b1eb4a7> 2012-05-31 [asonas]  修正
| * <b558333> 2012-05-31 [asonas]  piyoな機能を実装
| * <d5bb90a> 2012-05-31 [asonas]  piyoファイルを生成
|/  # 分岐点だよ。右がfeatureブランチでの作業。左がdevelopブランチでの作業
* <534f8f5> 2012-05-31 [asonas]  (master) hoge
```

という感じになる。
つまり、皆がみる（リモートにある）**develop**ブランチでは1つのまとまったコミットに見えるけど、実際に自分が作業したコミットはローカルの**feature**というブランチに残っている。

もしfeatureでやった実装にバグがあればまたfeatureブランチに移動して修正を加え、コミットし、squashオプションをつけてコミットすればよい。


## まぁでも
書いてることむずかしいし、ひとりで作業してるなら、適当にmasterブランチでごりごり作業してmasterにどんどんコミットすればいいよ。
gitもうちょっと詳しくなりたいってときにこの応用とか読むとかなりよさそうです。

