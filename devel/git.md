Git almodul klónozás:
```
git clone --recurse-submodules -j8 git://github.com/foo/bar.git
```
(-j8 opcionális, 2.8-tól, párhuzamosan 8 almodult tölt le)
```
git clone git://github.com/foo/bar.git
cd bar
git submodule update --init --recursive
```

Git branch letöltése:
```
$ git branch -r
origin/HEAD -> origin/master
origin/daves_branch
origin/discover
origin/master

$ git fetch origin discover
$ git checkout discover

git checkout --track origin/daves_branch
```

Git branch frissítése master-ből (az elsőnél egy plusz commit lesz):
```
git checkout b1
git merge origin/master
git push origin b1
```

```
git checkout b1
git fetch
git rebase origin/master
```

Ha a GitHub SSH-n keresztül kidob (csak végszükség esetén!):
```
ssh-keyscan github.com >> ~/.ssh/known_hosts
```
