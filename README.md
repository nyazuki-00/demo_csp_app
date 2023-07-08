# XSS 攻撃を対策する CSP のデモアプリ

## 環境構築

### API 側

```
cd server
docker-compose build
docker-compose run app db:create db:migrate
docker-compose up
```

### クライアント側

```
cd client
npm run start
```

## CSP の検証方法

1. App.tsx の 9~14 行目をコメントアウト
2. title に適当な文字列、content 部分に`<img src="invalid-image" onerror="alert(localStorage.getItem('accessToken'))">`を入れて作成。
3. XSS が成功すれば、js の alert()が起動し、画面に"アクセストークンの中身"という localStorage の値が表示される。
4. 1.でコメントアウトした部分を解除
5. 一覧画面をリロード(2 で作成したデータを削除してしまった場合は再度作成し直す)
6. alert が表示されず、検証ツールで確認すると、CSP の'script-src', 'self'directive によって inline event が refuse されたという由のエラーメッセージが表示されれば成功
