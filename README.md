atom-house-hackathon
====================

atom house hackathon

##はじめにやっておくといいこと

最低限必要なもの

``` ~/.gitconfig
[user]
	name = githubに登録したID
	email = githubに登録したメアド
```

登録しておくとまぁなんか見栄えがよい。あと、Githubのコミットログにアイコンとかも反映される
あるとよいもの

``` ~/.gitconfig
[core]
	excludesfile = ~/.gitignore

[alias]
	st = status
	df = diff
	graph = log --graph --date-order -C -M --pretty=format:\"<%h> %ad [%an] %Cgreen%d%Creset %s\" --all --date=short

[color]
	ui = true
	diff = true
```
などなど
@see [asonas/dotfiles](https://github.com/asonas/dotfiles)

###説明
[core]はまぁcoreって感じで覚えておけばOK
diffで使うコマンドをlessからlvにしたりとかもできる。普通はしなくてもいい

``` ~/.gitconfig
[core]
	excludesfile = ~/.gitignore
```
これの意味は、すべてのリポジトリで除去したいものをホームディレクトリにおいておく
例えば、中身はこんなかんじ

``` ~/.gitignore
.DS_Store #macのアレ
*~ #emacsのアレ
*.swp #vimのアレ
```

みたいな感じ。しておくと便利。しなくてもいいけど、リポジトリにスワップファイルあったらキレる人もいるからまぁ入れておくとよい

[alias]もそのまんまエイリアス。FF7の花売りみたいなの。
コマンドが長くてだるいからタイプ数減らしたいときに使う

``` ~/.gitconfig
[alias]
	st = status
	df = diff
	graph = log --graph --date-order -C -M --pretty=format:\"<%h> %ad [%an] %Cgreen%d%Creset %s\" --all --date=short

```

めんどさ極まると

``` .bashrc
alias g="git"
```
とか書き始める。

``` とある日のコミットまでのコマンド
g fc
g pl origin dev
g wtf
g st
g ci -am 'fix bug #21'
```
だんだん呪文とかす

あとなにかそのたもろもればissuesなどでどうぞ。