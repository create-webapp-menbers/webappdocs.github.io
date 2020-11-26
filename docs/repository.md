# Repository

## Git Commands

- Pages先のrepository
```bash 
git remote add origin git@github.com:create-webapp-menbers/webappdocs.github.io.git
```
- ソースを格納するrepository
```bash 
git remote add source git@github.com:create-webapp-menbers/webappdocs.git
```

### その他
* `git push --force <remote> <branch>` - 強制プッシュ
  
### branch

```mermaid
gitGraph:
options
{
    "nodeSpacing": 150,
    "nodeRadius": 10
}
end
commit
branch newbranch
checkout newbranch
commit
commit
checkout master
commit
commit
```

### GitHub Action ワークファイル

```yml
name: github pages

on:
  push:
    branches:
      - master

jobs:
  deploy:
    runs-on: ubuntu-18.04
    steps:
      - uses: actions/checkout@v2

      - name: Setup Python
        uses: actions/setup-python@v1
        with:
          python-version: '3.6'
          architecture: 'x64'

      - name: Cache dependencies
        uses: actions/cache@v1
        with:
          path: ~/.cache/pip
          key: ${{ runner.os }}-pip-${{ hashFiles('**/requirements.txt') }}
          restore-keys: |
            ${{ runner.os }}-pip-
      - name: Install dependencies
        run: |
          python3 -m pip install --upgrade pip
          python3 -m pip install -r ./requirements.txt
      - run: mkdocs build

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./site
```