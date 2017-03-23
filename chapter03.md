# dockerhubを見てみる

今まで使った環境変数や永続化するべきファイルの位置は
どこで調べればいいの？と言う当然の疑問が出てくると思います。
https://hub.docker.com/

ここから今回動かしたapplicationを探してみましょう
ubuntu, mysql, nginx, jenkins... etc etc

## やってみよう

* mysqlを任意のパスワードを設定して起動してみよう
* dockerhubのmysqlオフィシャルページでデータが保存される位置を調べて volume機能を使って永続化してみよう
