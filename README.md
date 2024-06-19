# istio-circuit-breaker-test

minikube での Istio の Circuit Breaker (ingress 経由) の個人確認用

## 事前準備

- minikube のインストール
- kubectl のインストール
- istioctl のインストール

```bash
minikube start
minikube addons enable ingress
istioctl install --set profile=demo -y
kubectl label namespace default istio-injection=enabled
```

## 手順

1. サンプルアプリケーションのデプロイ

```bash
kubectl apply -f ./
```

2. 手で istio-ingressgateway の Service を NodePort に変更

```
kubectl edit svc istio-ingressgateway -n istio-system
```

3. minikube tunnel の起動

```bash
minikube tunnel
```

3. /etc/hosts に追加

```bash
127.0.0.1 my-app.example.com
```

4. ブラウザでアクセス

- http://my-app.example.com/
- http://my-app.example.com/status/200 で 200 が返ってくることを確認
- http://my-app.example.com/status/500 で 500 が返ってくることを確認
- http://my-app.example.com/status/200 で 503 が 3 分間返ってくることを確認
- http://my-app.example.com/status/200 で 200 が返ってくることを確認
