# Remote repositories.
Jadi sebelumnya anda telah bekerja dengan git pada local machine, komputer anda. Sekarang kita akan menggunakan remote machine yang disediakan gratis oleh Github, Bitbucket dan provide Git lainnya. Selain protokol Git sebenarnya kita bisa menggunakan protokol remote machine lainnya sbb:

- `ssh://`
- `http[s]://`
- `git://`
- `file://`

Kita akan menggunakan protokol `https` untuk kemudahan akses (karena biasanya protokol lainnya di-blok) di Github. Silahkan register akun Github jika belum punya, dan buat repository baru.  kita buat repo di Bitbucket, anggap kita sudah punya akun Bitbucket, jika belum silakan buat akun Bitbucket. Sceenshot dibawah ini adalah contoh pembuatan repo baru dengan nama coba (.git).

![github_create](./pics/coba.png)

Silakan isi nama repo, misalnya coba, setelah itu klik create repository, maka selanjutnya akan muncul panduan untu meng-git repo local (folder coba) di komputer kita ke repo `coba` di Github. Kembali pada folder yang telah dibuat di terminal anda, misal folder `coba`, maka kita akan coba meng-add remote dan push pada remote repository di Github. Ingat, sebelumnya anda telah meng-init, add dan commit.

```
git remote add origin https://bitbucket.org/bagustris/coba.git
git push -u origin master
```

Karena kita sudah membuat readme.md, baris pertama pada panduan Github diatas kita skip, kita langsung menuju pada git remote dan git push. Masukkan perintah-perintah diatas pada terminal. Baris pertama adalah menambahkan (add) repo github ke git kita berikut linknya sebagai remote origin, link github dimana kita meng-host kode-kode kita. Baris terakhir adalah mem-push file-file di komputer kita ke server Github. Kita akan dimintai akun dan password git saat mem-push tersebut. Jika proses push berhasil, akan muncul notifikasi berikut di terminal, 

```
Writing objects: 100% (3/3), 231 bytes | 0 bytes/s, done.
Total 3 (delta 0), reused 0 (delta 0)
To https://github.com/bagustris/coba.git
 * [new branch]      master -> master
Branch master set up to track remote branch master from origin.
```

## Cloning, fetch dan merge
Sekarang berpindahlah dari repository/direktori kerja anda ke direktori yang lain. Kita akan melakukan proses cloning repository dengan dari suatu project yang di-host di github dengan `git clone` pada repository saya yang bernama`git-short`.
```
$ git clone https://github.com/bagustris/git-short.git
Cloning into 'basic_git'...
warning: You appear to have cloned an empty repository.
Checking connectivity... done.
```
A good idea would be to check the romte on our local version, this is done in two ways. First:
```
$ git remote -v
origin	https://github.com/bagustris/git-short.git (fetch)
origin	https://github.com/bagustris/git-short.git (push)
```
Second:
```
$ git remote show origin
* remote origin
  Fetch URL: https://github.com/bagustris/git-short.git
  Push  URL: https://github.com/bagustris/git-short.git
  HEAD branch: master
  Remote branch:
    master tracked
  Local branch configured for 'git pull':
    master merges with remote master
  Local ref configured for 'git push':
    master pushes to master (up to date)
```
Notice that `origin` is nothing but a default name for a remote. Remote repositories are used to `push` and `pull` data. Now that we have a working copy of the projet, doing some work would be a good idea:
```
# Gunakan nano jika anda belum terbiasa dengan vim
vim first.txt
git commit -a -m "first commit by..."
```
At this level everything we did was local. Pushing our jod to the remote url named `origin` on the `master branch`is just as simple as:
```
$ git push origin master
Counting objects: 3, done.
Writing objects: 100% (3/3), 243 bytes | 0 bytes/s, done.
Total 3 (delta 0), reused 0 (delta 0)
To https://github.com/bagustris/git-short.git
 * [new branch]      master -> master
```
Now check the GitHub web page you'll see your work has been loaded online.

**Agenda sunt:** Let the lecturer push some work, and try to push yourself. What happens?

# Managing Remotes and the Correct Way to Collaborate.
Suppose now it's 30 people collaborating on the same project. It would be than a good idea to have your own remote version, together with the development remote version of the repository. Git based platforms such has GitHub, provide this feature, and they often call it *Fork*. 

![alt text](./pics/fork_0.png)

![alt text](./pics/fork_1.png)

Now that you have your own remote copy of the project you can clone it:
```
$ git clone https://github.com/bagustris/git-short.git
Cloning into 'basic_git'...
Warning: Permanently added the RSA host key for IP address '192.30.252.130' to the list of known hosts.
remote: Counting objects: 9, done.
remote: Compressing objects: 100% (4/4), done.
remote: Total 9 (delta 1), reused 9 (delta 1), pack-reused 0
Receiving objects: 100% (9/9), done.
Resolving deltas: 100% (1/1), done.
Checking connectivity... done.
```
You want to link your local copy, with your remote and the official remote. This is done adding the official repo to your remote list:
```
$ git remote add bagustris https://github.com/bagustris/git-short.git
$ git remote -v
bagustris	https://github.com/bagustris/git-short.git (fetch)
bagustris	https://github.com/bagustris/git-short.git(push)
origin	https://github.com/bagustris/git-short.git (fetch)
origin	https://github.com/bagustris/git-short.git (push)
```

Suppose you go and get a coffe, and the developers have pushed some work into the project. It is a good idea to sync your local and remote repositories with the original one.
```
$ git fetch --all 
Fetching origin
Fetching bagustris
remote: Counting objects: 3, done.
remote: Compressing objects: 100% (1/1), done.
remote: Total 3 (delta 1), reused 3 (delta 1), pack-reused 0
Unpacking objects: 100% (3/3), done.
From github.com/bagustris/git-short
 * [new branch]      master     -> bagustris/master
```
You want `bagustris/master` branch to be merged on your local and remote copies. You need first to set your `HEAD` to `master` on local repository.
```
$ git checkout master
Already on 'master'
Your branch is up-to-date with 'origin/master'.
```
Then you want to `merge` local with `bagustris/master`.
```
$ git merge bagustris/master
Updating 7a0395a..e7c0b24
Fast-forward
 first.txt | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)
 ```
 And finally push this data to the remote branch.
 ```
$ git push origin master
Counting objects: 5, done.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 326 bytes | 0 bytes/s, done.
Total 3 (delta 1), reused 0 (delta 0)
To git@github.com:teststudentmhpc/basic_git.git
   7a0395a..e7c0b24  master -> master
```

# Branching
It's your time to work, finally. A good habit is to keep the master branch *untouched* and do your dirty stuff into a local branch:
```
$ git checkout -b dirty
Switched to a new branch 'dirty'
$ git branch
* dirty
  master
$ vim first.txt 
$ git commit -a -m "some student's work"
[dirty 4e37220] some student's work
 1 file changed, 1 insertion(+), 1 deletion(-)
$ git push origin dirty
Counting objects: 5, done.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 305 bytes | 0 bytes/s, done.
Total 3 (delta 1), reused 0 (delta 0)
To git@github.com:teststudentmhpc/basic_git.git
 * [new branch]      dirty -> dirty
```
Now you believe that your dirty work is good enought to be shared with the community, but you want it to be reviwed by the developers. What you usually do is create a *pull request*.

![pull_request](./pics/pull_request.png)
![network](./pics/network.png)

## Resume:
- git remote
- git clone
- git push
- git pull
- git merge
- git fetch
- git branch

# Bahan Bacaan
- [Git clone, Push dan Pull](http://www.bagustris.tk/2015/04/pengenalan-git-clone-push-dan-pull.html)
