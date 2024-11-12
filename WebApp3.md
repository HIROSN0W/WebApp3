# WebApp3

<div style="text-align: right;">
3I24 中川 寛之
</div>

## 目的
- WebSocketについて理解する
- MQTTについて理解する
- ネットワークプロトコルの違いにより、アーキテクチャ、実装方法、通信量などが異なることを理解する
- Webアプリケーション開発における、ネットワークプロトコルの選定について学ぶ

## 使用したツール

- docker (コンテナ)
- Node.js
- React (Frontend Web Framework: javascript/typescript)
- WebSocket
- MQTT(mosquitto)


## 環境構築1: 
### 作成したシステムの概略
Formに入力したメッセージを表示するWebアプリの構築  
*メッセージは永続化されない
### 動作確認の方法と結果


<chromeの画像>
![環境開発１](image.png)
<edgeの画像>
![alt text](image-2.png)


Formに入力してメッセージが表示されることが確認された。  
また、メッセージは双方のWebに表示されなかった。

## 課題1: 
### 動作確認の方法と結果

<HTTPストリームの内容(抜粋)>

```
OPTIONS /submit HTTP/1.1
Host: localhost:3001
Connection: keep-alive
Accept: */*
Access-Control-Request-Method: POST
Access-Control-Request-Headers: access-control-allow-credentials,access-control-allow-origin,content-type
Origin: http://localhost:3000

HTTP/1.1 200 OK
Access-Control-Allow-Origin: http://localhost:3000
Access-Control-Allow-Credentials: true
Access-Control-Allow-Headers: access-control-allow-credentials, access-control-allow-origin, content-type
Access-Control-Allow-Methods: DELETE, GET, HEAD, OPTIONS, PATCH, POST, PUT
```
- **Access-Control-Request-Method**:送ろうとしている使用したいメソッド(POST)
 <=> **Access-Control-Allow-Methods**:許可しているメソッド 

- **Access-Control-Request-Headers**:送ろうとしているヘッダー  
 <=>**Access-Control-Allow-Headers**:サーバが許可してるヘッダー  

- **Origin**:送ろうとしているヘッダー  
 <=>**Access-Control-Allow-Origin**:許可されたオリジン(localhost:3000)



[参考](https://qiita.com/rudy39/items/cc70f3a2a494850f6028)

<tsharkの様子>
![課題１](image-1.png)

```
パケット総数は1773[byte]
```

## 環境構築2: 
### 作成したシステムの概略
REST APIに変わり、WebSocketを用いてサーバ、クライアント間のデータ送受信を行う実装
### 動作確認の方法と結果

![環境構築２](image-6.png)

上記の画像を確認すると、chrome側で送信されたメッセージがedge側で確認できた

## 課題2: 
### 動作確認の方法と結果

<WebSocketのストリーム内容(抜粋)>

```
GET /list HTTP/1.1
Host: localhost:3002
Connection: Upgrade
Upgrade: websocket
Origin: http://localhost:3000
Sec-WebSocket-Version: 13
Sec-WebSocket-Key: +IofNf891WAdvvEdKvBaPw==

HTTP/1.1 101 
Upgrade: WebSocket
Connection: Upgrade
Sec-WebSocket-Accept: 6NlBz+lpAyFsPqh+E+kJyM61qrc=
```

- **Upgrade: websocket**：HTTPからWebSocketへのプロトコル切り替えを要求
- **Sec-WebSocket-Key**:特定のクライアントとのコネクションの確立を立証する為に使われる

<tsharkの様子>
![alt text](image-8.png)

```
パケット総数は1866[byte]
```

## 環境構築3: 
### 作成したシステムの概略
### 動作確認の方法と結果
## 課題3: 
### 動作確認の方法と結果
```
codespace ➜ /workspaces/webapp3-HIROSN0W/src (main) $ docker compose run --rm mqtt-sub mosquitto_sub  -u appuser -P UYII8ceNiICh -h mqtt-broker -t "+/+/+/+/temp" -v
snct/building1/3F/room1305/temp 28
snct/building1/3F/room1301/temp 25
snct/building1/2F/room1204/temp 24
```

```
codespace ➜ /workspaces/webapp3-HIROSN0W/src (main) $ docker compose run --rm mqtt-sub mosquitto_sub  -u appuser -P UYII8ceNiICh -h mqtt-broker -t "+/+/+/+/humi" -v
snct/building1/3F/room1305/humi 56
snct/building1/3F/room1301/humi 65
snct/building1/2F/room1204/humi 70
```
## 環境構築4: 
### 作成したシステムの概略
### 動作確認の方法と結果
## 課題4: 
### 動作確認の方法と結果

```
<mqtt_get>

GET / HTTP/1.1
Host: localhost:8080
Connection: Upgrade
Pragma: no-cache
Cache-Control: no-cache
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/130.0.0.0 Safari/537.36 Edg/130.0.0.0
Upgrade: websocket
Origin: http://localhost:3000
Sec-WebSocket-Version: 13
Accept-Encoding: gzip, deflate, br, zstd
Accept-Language: ja,en;q=0.9,en-GB;q=0.8,en-US;q=0.7
Sec-WebSocket-Key: xw+cLUHKv1BRP0f01FbhDg==
Sec-WebSocket-Extensions: permessage-deflate; client_max_window_bits
Sec-WebSocket-Protocol: mqtt

HTTP/1.1 101 Switching Protocols
Upgrade: WebSocket
Connection: Upgrade
Sec-WebSocket-Accept: hLXiH03q3q4XAJCgNHG5vz0QYp4=
Sec-WebSocket-Protocol: mqtt
```


```
<mptt_Follow TCP_stream>
GET / HTTP/1.1
Host: localhost:8080
Connection: Upgrade
Pragma: no-cache
Cache-Control: no-cache
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/130.0.0.0 Safari/537.36 Edg/130.0.0.0
Upgrade: websocket
Origin: http://localhost:3000
Sec-WebSocket-Version: 13
Accept-Encoding: gzip, deflate, br, zstd
Accept-Language: ja,en;q=0.9,en-GB;q=0.8,en-US;q=0.7
Sec-WebSocket-Key: xw+cLUHKv1BRP0f01FbhDg==
Sec-WebSocket-Extensions: permessage-deflate; client_max_window_bits
Sec-WebSocket-Protocol: mqtt

HTTP/1.1 101 Switching Protocols
Upgrade: WebSocket
Connection: Upgrade
Sec-WebSocket-Accept: hLXiH03q3q4XAJCgNHG5vz0QYp4=
Sec-WebSocket-Protocol: mqtt

....K...K......VK.......~...f...z...{...{.......~...z...K...;...9.......(........ .......Q...Q...2.u.`.`.#.u.c.?.........~..0...chat1/thread2/user3{"nickname":"hiro","postmessage":"hello","posttime":"Mon Nov 11 2024 15:59:34 GMT+0900 (...............)"}.~..0....chat1/thread2/user3{"nickname":"taro","postmessage":"ohayo-","posttime":"Mon Nov 11 2024 15:59:36 GMT+0900 (...............)"}
```

2379[byte]


## 理解したこと、理解できていないこと

## 写真フォルダー

![alt text](image-3.png)

![alt text](image-5.png)


![課題２](image-7.png)

![課題４](image-9.png)

![課題４](image-10.png)
![課題４](image-11.png)
```
UYII8ceNiICh
```