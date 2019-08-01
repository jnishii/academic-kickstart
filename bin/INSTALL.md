# submodule
academic-themeをsubmoduleで使っていると色々面倒なことが起きたので，
今はこうしている

別リポジトリとしてkickstartをclone
```
$ git clone https://github.com/sourcethemes/academic-kickstart.git academic-kickstart
$ cd academic-kickstart
$ git submodule update --init --recursive
```
symlinkをはる
```
$ cd hugo-academic/themes
$ ln -sf ../../academic-kickstart/themes/academic .
```

## 以下は，themeを分離する前のメモ

### install academic theme

```
$ git submodule add  git@github.com:gcushen/hugo-academic.git 
```

### uninstall academic theme submodule

```
$ git submodule deinit -f themes/academic
$ git rm -f themes/academic
```

```
$ rm .git/modules
```

# その他の設定

[Change "post/" to "blog/" #356](https://github.com/gcushen/hugo-academic/issues/356)