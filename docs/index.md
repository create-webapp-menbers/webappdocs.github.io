# このドキュメントについて

## 必須開発環境

- Python3がインストール済み `python3.8.2`

- 仮想環境のvirtualenvがインストール済み
  > * `sudo apt install virtualenv` - ubuntu(Linux) install 
  >     * `pip install virtualenv` - windows install


## virtualenv使用法

* `virtualenv .env` - 仮想環境作成 rootディレクトり内でコマンド入力
* `source .env/bin/activate` - 仮想環境の有効化以下のように`(.env) `が付くと有効

## 依存関係

* `pip install <ライブラリ名>` - 依存するライブラリのインストール
* `pip install -r requirements.txt` - 依存するライブラリの一括インストール
* `pip freeze > requirements.txt` - 依存するライブラリのリストの出力


## Mkdocs Commands

* `mkdocs new [dir-name]` - Create a new project.
* `mkdocs serve` - Start the live-reloading docs server.
* `mkdocs build` - Build the documentation site.
* `mkdocs -h` - Print help message and exit.

> MkDocs [mkdocs.org](https://www.mkdocs.org).
> mkdocs-material [mkdocs-material](https://squidfunk.github.io/mkdocs-material/).

## Project layout

    mkdocs.yml    # The configuration file.
    docs/
        index.md  # The documentation homepage.
        ...       # Other markdown pages, images and other files.


https://facelessuser.github.io/pymdown-extensions/extensions/