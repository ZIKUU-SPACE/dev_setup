# Linux (Ubuntu) 開発者向けセットアップ

[モノづくり塾『ZIKUU』](https://zikuu.space)塾長の作業環境から、汎用的に使えそうな設定を抜き出した資料です。プログラマーやエンジニアを目指す方にオススメです。

ZIKUUでは標準OSをUbuntu系ということにしていますので、Ubuntuがインストールされていることを前提として作成しました。その他のLinuxディストリビューションを使う場合は、個別に調査するか塾長に相談してください。

この文書では、各ソフトウェアのインストールと基本設定をコピーペーストして行えるようにしています。
各項目の詳細な説明は省略しているので、操作方法等はそれぞれネット検索してわかりやすい説明を探してください。

## Gitのインストール

```
sudo apt update
sudo apt install git
```

Ubuntuは最初からgitがインストールされている筈ですので、この作業は不要です。

## Neovimのインストール

ZIKUUで使用しているNeovimの設定です。インストールするには次のコマンドを実行します。次のコマンドでインストールします。


```
sudo add-apt-repository ppa:neovim-ppa/stable
sudo apt update
sudo apt install neovim
```

Neovimを使いこなせると、サーバーの設定や遠隔コンピューターの管理など、GUIを使えないときのテキスト編集作業で役に立ちます。


### Neovimの設定をGithubからダウンロード

```
git clone https://github.com/ZIKUU-SPACE/neovim-config.git $HOME/.config/nvim
```

### Packerのインストール

PackerはNeovim用のパッケージマネージャーです。

次のコマンドでPacker をインストールします。

```
sudo apt install curl
git clone --depth 1 https://github.com/wbthomason/packer.nvim\
 ~/.local/share/nvim/site/pack/packer/start/packer.nvim
```

### Neovimプラグインのインストール

nvimを起動して、PackerInstallコマンドを実行します。

## VSCodiumのインストール

```
sudo apt update
sudo apt upgrade
sudo apt install dirmngr software-properties-common apt-transport-https curl -y
curl -fSsL https://gitlab.com/paulcarroty/vscodium-deb-rpm-repo/raw/master/pub.gpg | sudo gpg --dearmor | sudo tee /usr/share/keyrings/vscodium.gpg > /dev/null
echo deb [signed-by=/usr/share/keyrings/vscodium.gpg] https://download.vscodium.com/debs vscodium main | sudo tee /etc/apt/sources.list.d/vscodium.list
sudo apt update
sudo apt install codium -y
```

## zshのインストール

### パケージマネージャを使ってzshをインストール

```
sudo apt update
sudo apt install zsh
```

### zshをデフォルトシェルにする

```
sudo chsh -s $(which zsh)
```

一旦ログアウトしてログインする。

### zshプラグインをインストールする

gitがインストールされていない場合は次のコマンドでgitをインストールする。

```
sudo apt install git
```

プラグインのインストール

```
sh -c "$(curl -fsSL https://install.ohmyz.sh/)"
```




## Githubへのsshキーの登録（任意）

これはGithubとのやり取りで毎回パスワードを入力しなくても良くする設定です。

### sshキーの生成

```
ssh-keygen -C メールアドレス

[出力]
Generating public/private ed25519 key pair.
Enter file in which to save the key (/home/ユーザー名/.ssh/id_ed25519): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
```
Passphraseを入力する（推奨）。入力しなくても良い。

こんな出力が得られる。

```
Your identification has been saved in /home/ktsubaki/.ssh/id_ed25519
Your public key has been saved in /home/ktsubaki/.ssh/id_ed25519.pub
The key fingerprint is:
SHA256:c4y4ScmAyOdil9M9/Hi/GYJrrCA7rvTtORyWixxf5qY メールアドレス
The key's randomart image is:
+--[ED25519 256]--+
|                 |
|.. .             |
|.....            |
|  o oooo o       |
| o = .*+S o      |
|. o..= ==o       |
| o..* Xo + .     |
|..+oo=.=o o o    |
|+o...E*.   +.    |
+----[SHA256]-----+
```

### 公開キーをクリップボードにコピーする

```
cat ~/.ssh/id_ed25519.pub

[出力]
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAINo8qOc7todYPkNj1F/2TkApqwzRRk9t3+4q+nbM4wi1 メールアドレス
```

### Githubサイトを開く

![](./img/01_github.png)

### Settingsメニューを選ぶ
![](./img/02_github_select_settings.png)

### SSHキー登録画面を選ぶ

![](./img/03_github_sshkeys.png)

### クリップボードにコピーした公開キーを登録する

![](./img/04_add_sshkey.png)

### Githubに接続確認

```
ssh -T git@github.com

[出力]
The authenticity of host 'github.com (20.27.177.113)' can't be established.
ED25519 key fingerprint is SHA256:+DiY3wvvV6TuJJhbpZisF/zLDA0zPMSvHdkr4UvCOqU.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'github.com' (ED25519) to the list of known hosts.
Hi kazz12211! You've successfully authenticated, but GitHub does not provide shell access.
```

## Dockerのインストール

公式ページによるインストール方法

```
sudo apt-get update

sudo apt-get install ca-certificates curl

sudo install -m 0755 -d /etc/apt/keyrings

sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update


sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

sudo usermod -aG docker $USER
```


## 最新版のNodeJSのインストール（任意）

```
sudo apt update && sudo apt upgrade
```
```
sudo apt install curl apt-transport-https ca-certificates
```
```
curl -fsSL https://deb.nodesource.com/gpgkey/nodesource-repo.gpg.key | sudo gpg --dearmor -o /usr/share/keyrings/nodesource.gpg
```

最新版のバージョンを確認する。

[https://nodejs.org/en](https://nodejs.org/en)

*執筆では最新バージョンは22だった。*

```
echo "deb [signed-by=/etc/apt/keyrings/nodesource.gpg] https://deb.nodesource.com/node_22.x nodistro main" | sudo tee /usr/share/keyrings/nodesource.gpg
```
```
sudo apt install nodejs
```


## Rustのインストール（任意）

```
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

## tmuxのインストール (R6/12/26追記)

tmuxはターミナルベースのウィンドウマネージャで、ターミナル画面を分割して複数のウィンドウを表示することができます。

```
sudo apt update
sudo apt install tmux
```

インストールが終わったら
```
tmux
```
で起動し、Ctrl+B, " (ダブルクオート)で画面を上下分割、Ctrl+B %で画面を左右分割できるのが確認できると思います。

## Glowのインストール（任意）

Glowはターミナル上でMarkdown形式の文書をプレビューするソフトウエアです。

```
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://repo.charm.sh/apt/gpg.key | sudo gpg --dearmor -o /etc/apt/keyrings/charm.gpg
echo "deb [signed-by=/etc/apt/keyrings/charm.gpg] https://repo.charm.sh/apt/ * *" | sudo tee /etc/apt/sources.list.d/charm.list
sudo apt update && sudo apt install glow
```
