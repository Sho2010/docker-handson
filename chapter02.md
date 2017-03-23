# 永続化に関する話

containerが終了と同時に保存したファイルなども全て破棄されます。
あくまでコンテナはプロセスなので永続化の仕組みは持っていません。

`先程のMySQLの例でもそのコンテナを終了させた場合、作ったデータベースはなくなります。`

これでは使いものにならないのでvolumeという仕組みを利用します。
※ docker commitという手段もありますがあまり実践的でないので触れません


volumeとは
`docker hostのファイルシステムをcontainerのファイルシステムへmountするという仕組みです。`

# まずはどういう動作をするか確認

## 保存できないケース

containerはあくまでプロセスなので保存することは出来ないということを確認する

~~~
$ docker run -it ubuntu bash

# ここからコンテナ内部で実行
$ echo "awesome text" > awesome.txt
$ ls
# ctrl+cで終了

$ docker run -it ubuntu bash
# ここから再びコンテナ内部で実行
$ ls
# ！！！さっきのファイルがない！！！
# ctrl+cで終了
~~~

# volumeを使ってみる？

~~~
$ mkdir -p /tmp/mount_dir #任意のdirectory
$ cd /tmp/mount_dir
$ echo "hello volume" > file1.txt
$ echo abc > file2.txt

# chapter2をcontainer内の/mount_dirにmountします
$ docker run -it -v /tmp/mount_dir:/mount_dir ubuntu bash

# ここからコンテナ内部で実行
$ ls /
$ cd /mount_dir
$ echo "append_text" >> file1.txt
# ctrl+cで終了

# 再びホストマシンに戻る
$ cat /tmp/mount_dir/file1.txt

# !!!container内部の作業がhostのファイルにも反映された!!!

# 再度同じvolumeをmountしてcontainerを起動
$ docker run -it -v /tmp/mount_dir:/mount_dir ubuntu bash
$ cat /mount_dir/file1.txt
# ctrl+cで終了
~~~


つまり...

# volumeを使って情報を永続化しよう！

各コンテナのDBの情報やjenkins job情報などをhost側のマシンに保存しておけば永続化が可能である！

当然こういう疑問が出てくるはず...
各コンテナ(mysql, jenkins等)がどこのファイルパスに永続化するべき情報が書いてあるの？
