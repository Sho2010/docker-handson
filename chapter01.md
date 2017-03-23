# とりあえず動かす

とりあえずド定番
ubuntu, nginx, mysql, jenkinsを動かしてみよう

### ubuntu

~~~
$ docker run -it ubuntu bash
# ...ここからコンテナ内部
$ cat /etc/lsb-release
> DISTRIB_ID=Ubuntu
> DISTRIB_RELEASE=16.04
> DISTRIB_CODENAME=xenial
> DISTRIB_DESCRIPTION="Ubuntu 16.04.1 LTS"

# ctrl+cで終了

$ docker run -it ubuntu:17.04 bash
# ...ここからコンテナ内部
$ cat /etc/lsb-release
> DISTRIB_ID=Ubuntu
> DISTRIB_RELEASE=17.04
> DISTRIB_CODENAME=zesty
> DISTRIB_DESCRIPTION="Ubuntu Zesty Zapus (development branch)"

# ctrl+cで終了
~~~


### nginx

~~~
$ docker run -p 80:80 nginx
# ブラウザで見てみます
# ctrl+cで終了
~~~


### mysql

~~~
$ docker run -e MYSQL_ALLOW_EMPTY_PASSWORD=true mysql 

error: database is uninitialized and password option is not specified
  You need to specify one of MYSQL_ROOT_PASSWORD, MYSQL_ALLOW_EMPTY_PASSWORD and MYSQL_RANDOM_ROOT_PASSWORD

# お、何やらエラーが

$ docker run -p 3306:3306 -e MYSQL_ALLOW_EMPTY_PASSWORD mysql 

# mysql clientが入ってるなら
# localhostではなく127.0.0.1で指定すること
mysql -h 127.0.0.1 -u root

# ctrl+cで終了
~~~

### jenkins

~~~
$ docker run -p 8080:8080 -p 50000:50000 jenkins
http://localhost:8080
# ctrl+cで終了

# アクセスしたら自動生成したkeyを入れろと表示されたはず
# containerの中に入って操作する方法に関してはあとでやります

~~~

# 動作中のコンテナに対して何かを行う

このままでも動作するがshellのフロントを専有するのはいささか不便なので
今までの `docker run` commandに-dをつけてみようデーモンで起動するはず

~~~
$ docker run -d -e MYSQL_ALLOW_EMPTY_PASSWORD=true mysql 

# 実行後にcontainer idが表示されるはず
>fccad2137c4a496cb04b5a8fe3fae66f3a5d8130519ee7526826e2cb6c2bac78

$ docker ps
# コンテナ情報が表示されたはず

# コンテナが出力されたログを見てみる
# IDは頑張ってマッチしてくれるのでフルじゃなくてもいい
# e.g. docker logs fcc
$ docker logs -f ${container_id}
# ctrl+cで終了

# containerを止める
docker stop ${container_id}

~~~

# 基本チートシート


動いてるコンテナが見たい
`$ docker ps`

動いてるコンテナから出力されたログを見たい
`$ docker logs ${contaner_id}` # tailと一緒で-fに対応している

動いているcontainerを止めたい
`$ docker stop ${container_id}`

動作中のcontainerに対して何かしたい
`$ docker exec -it ${container_id} ${command}`

e.g. 動作中のcontainerにbashで入りたい場合(imageによってはbashが存在しないのでその場合shとかで)
`docker exec -it fccad bash`

