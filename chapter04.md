# 自作docker imageを作ってみよう

~~~
$ cd my_dockerfile
$ docker build -t my_container -f Dockerfile .
$ docker build -t my_container02 -f Dockerfile02 .
$ docker build -t my_container03 -f Dockerfile03 .
~~~

### 作ったイメージを見てみる

~~~
$ docker images
~~~

### 作ったイメージを立ち上げてみる

~~~
$ docker run my_container
$ docker run my_container02
$ docker run my_container03
~~~

# 中身を見てみる

めんどくさいからじぶんで喋る

- - -


いままでさんざん起動してきた
`docker run my_container` はDockerfileのCMDに書いた内容が実行されてたということがわかります。

では...? 
これは？

`$ docker run -it my_container bash`

末尾に実行するコマンドをつけると`CMD`で書かれた内容を上書きして実行する！
という意味だったんですね

`-it` はttyでInteractive shellということなので気になるなら自分で調べてください！！

## やってみよう

* my_container02で`docker build`を行わずに表示される内容を変更してみよう(volumeを使うと...)
