![DRF](img/header_logo.png)

# このドキュメントについて

Webアプリケーションを作成するにあたって得た技術を記録するドキュメントサイト
このサイトは[MkDocs](https://www.mkdocs.org)で制作しています。

---
# このドキュメントを編集するには

## 編集環境の構築

1. Python3をインストール [Python公式サイト](https://www.python.org/).  
2. Gitをインストール [Git公式サイト](https://git-scm.com/).  
3. repositoryのクローン
    - HTTPS ⇒ `git clone https://github.com/create-webapp-menbers/webappdocs.github.io.git`
    - SSH ⇒ `git clone git@github.com:create-webapp-menbers/webappdocs.github.io.git`

4. 依存関係の一括インストール
```
pip install -r requirements.txt
```
5. サーバー起動
```
mkdocs serve
```
以下出力
```bash
INFO    -  Building documentation... 
INFO    -  Cleaning site directory 
INFO    -  Documentation built in 0.22 seconds 
[I 201126 22:58:35 server:296] Serving on http://127.0.0.1:8000
INFO    -  Serving on http://127.0.0.1:8000
[I 201126 22:58:35 handlers:62] Start watching changes
INFO    -  Start watching changes
[I 201126 22:58:35 handlers:64] Start detecting changes
INFO    -  Start detecting changes
```
6. `http://127.0.0.1:8001/`にアクセス

!!! note
    **各編集方法については以下を参照**  
    > [MkDocs ](https://www.mkdocs.org).  
    > [mkdocs-material](https://squidfunk.github.io/mkdocs-material/).  
    > https://facelessuser.github.io/pymdown-extensions/

    ダイアグラム  
    > https://mermaid-js.github.io/mermaid/#/
---

# 補足情報
## virtualenv(仮想Python環境)

Pythonの仮想環境があるとホストコンピューターのPython環境を汚すことなく開発ができる

- 仮想環境のvirtualenvをインストール
  > * `sudo apt install virtualenv` - ubuntu(Linux) install 
  >     * `pip install virtualenv` - windows install

* `virtualenv .env` - 仮想環境作成 rootディレクトり内でコマンド入力
* `source .env/bin/activate` - 仮想環境の有効化以下のように`(.env) `が付くと有効

## 依存関係についてのtips
Pythonの依存ライブラリを扱うためのコマンド

* `pip install <ライブラリ名>` - 依存するライブラリのインストール
* `pip freeze > requirements.txt` - 依存するライブラリのリストの出力


## MkdocsのCommands

* `mkdocs new [dir-name]` - プロジェクトを新規作成
* `mkdocs serve` - ドキュメントファイルが編集されると自動更新するサーバー起動
* `mkdocs build` - HTMLを生成
* `mkdocs -h` - MkDocsコマンドのヘルプ

## Project layout

    mkdocs.yml    # The configuration file.
    docs/
        index.md  # The documentation homepage.
        ...       # Other markdown pages, images and other files.
