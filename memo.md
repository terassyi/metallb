# Metallb source code reading by terassyi

## .github/workflows/ci.yaml

- e2e をみる
- setup -> ./.github/workflows/composite/setup/actions.yaml

### Deploy Metallb

- [invoke(inv)](https://www.pyinvoke.org/) なるタスクランナーを利用している
- `sudo -E env "PATH=$PATH" inv e2etest --skip $SKIP $NATIVE_BGP -e /tmp/kind_logs` で e2e test をやってる
- e2e test の実態は `./e2etest/e2etest_suite_teset.go#TestMain`

## .github/workflows/composite/setup/actions.yaml

- docker load で speacker, controller のコンテナイメージをロードしてる

## e2etest

- [README](https://github.com/metallb/metallb/tree/main/e2etest)

## dev-env

- [README](https://github.com/metallb/metallb/tree/main/dev-env)


## Log

### controller

#### 起動時

```
controller-7fdbd8896f-hgj2x controller {"level":"info","ts":1677855829.9932172,"msg":"Starting workers","controller":"ipaddresspool","controllerGroup":"metallb.io","controllerKind":"IPAddressPool","worker count":1}
controller-7fdbd8896f-hgj2x controller {"caller":"pool_controller.go:47","controller":"PoolReconciler","level":"info","start reconcile":"/metallb-system","ts":"2023-03-03T15:03:49Z"}
controller-7fdbd8896f-hgj2x controller {"caller":"pool_controller.go:101","controller":"PoolReconciler","event":"force service reload","level":"info","ts":"2023-03-03T15:03:49Z"}
controller-7fdbd8896f-hgj2x controller {"caller":"pool_controller.go:112","controller":"PoolReconciler","event":"config reloaded","level":"info","ts":"2023-03-03T15:03:49Z"}
controller-7fdbd8896f-hgj2x controller {"caller":"pool_controller.go:113","controller":"PoolReconciler","end reconcile":"/metallb-system","level":"info","ts":"2023-03-03T15:03:49Z"}
```

```
controller-7fdbd8896f-hgj2x controller {"caller":"service_controller.go:103","controller":"ServiceReconciler","end reconcile":"default/kubernetes","level":"info","ts":"2023-03-03T15:03:49Z"}
controller-7fdbd8896f-hgj2x controller {"caller":"service_controller.go:60","controller":"ServiceReconciler","level":"info","start reconcile":"kube-system/kube-dns","ts":"2023-03-03T15:03:49Z"}
controller-7fdbd8896f-hgj2x controller {"caller":"service_controller.go:103","controller":"ServiceReconciler","end reconcile":"kube-system/kube-dns","level":"info","ts":"2023-03-03T15:03:49Z"}
controller-7fdbd8896f-hgj2x controller {"caller":"service_controller_reload.go:61","controller":"ServiceReconciler - reprocessAll","level":"info","start reconcile":"metallbreload/reload","ts":"2023-03-03T15:03:49Z"}
controller-7fdbd8896f-hgj2x controller {"caller":"pool_controller.go:47","controller":"PoolReconciler","level":"info","start reconcile":"/local-path-storage","ts":"2023-03-03T15:03:49Z"}
controller-7fdbd8896f-hgj2x controller {"caller":"service_controller_reload.go:104","controller":"ServiceReconciler - reprocessAll","end reconcile":"metallbreload/reload","level":"info","ts":"2023-03-03T15:03:49Z"}
controller-7fdbd8896f-hgj2x controller {"caller":"pool_controller.go:101","controller":"PoolReconciler","event":"force service reload","level":"info","ts":"2023-03-03T15:03:49Z"}
controller-7fdbd8896f-hgj2x controller {"caller":"pool_controller.go:112","controller":"PoolReconciler","event":"config reloaded","level":"info","ts":"2023-03-03T15:03:49Z"}
controller-7fdbd8896f-hgj2x controller {"caller":"pool_controller.go:113","controller":"PoolReconciler","end reconcile":"/local-path-storage","level":"info","ts":"2023-03-03T15:03:49Z"}
controller-7fdbd8896f-hgj2x controller {"caller":"service_controller_reload.go:61","controller":"ServiceReconciler - reprocessAll","level":"info","start reconcile":"metallbreload/reload","ts":"2023-03-03T15:03:49Z"}
controller-7fdbd8896f-hgj2x controller {"caller":"service_controller_reload.go:104","controller":"ServiceReconciler - reprocessAll","end reconcile":"metallbreload/reload","level":"info","ts":"2023-03-03T15:03:49Z"}
```

```
controller-7fdbd8896f-hgj2x controller {"level":"info","ts":1677855831.4361925,"logger":"controller-runtime.builder","msg":"skip registering a mutating webhook, object does not implement admission.Defaulter or WithDefaulter wasn't called","GVK":"metallb.io/v1beta1, Kind=AddressPool"}
controller-7fdbd8896f-hgj2x controller {"level":"info","ts":1677855831.4362576,"logger":"controller-runtime.builder","msg":"Registering a validating webhook","GVK":"metallb.io/v1beta1, Kind=AddressPool","path":"/validate-metallb-io-v1beta1-addresspool"}
controller-7fdbd8896f-hgj2x controller {"level":"info","ts":1677855831.4363286,"logger":"controller-runtime.webhook","msg":"Registering webhook","path":"/validate-metallb-io-v1beta1-addresspool"}
controller-7fdbd8896f-hgj2x controller {"level":"info","ts":1677855831.4363697,"logger":"controller-runtime.webhook.webhooks","msg":"Starting webhook server"}
controller-7fdbd8896f-hgj2x controller {"level":"info","ts":1677855831.43644,"logger":"controller-runtime.webhook","msg":"Registering webhook","path":"/convert"}
controller-7fdbd8896f-hgj2x controller {"level":"info","ts":1677855831.436467,"logger":"controller-runtime.builder","msg":"Conversion webhook enabled","GVK":"metallb.io/v1beta1, Kind=AddressPool"}
controller-7fdbd8896f-hgj2x controller {"level":"info","ts":1677855831.4364753,"logger":"controller-runtime.builder","msg":"skip registering a mutating webhook, object does not implement admission.Defaulter or WithDefaulter wasn't called","GVK":"metallb.io/v1beta1, Kind=IPAddressPool"}
controller-7fdbd8896f-hgj2x controller {"level":"info","ts":1677855831.436483,"logger":"controller-runtime.builder","msg":"Registering a validating webhook","GVK":"metallb.io/v1beta1, Kind=IPAddressPool","path":"/validate-metallb-io-v1beta1-ipaddresspool"}
controller-7fdbd8896f-hgj2x controller {"level":"info","ts":1677855831.4365056,"logger":"controller-runtime.webhook","msg":"Registering webhook","path":"/validate-metallb-io-v1beta1-ipaddresspool"}
controller-7fdbd8896f-hgj2x controller {"level":"info","ts":1677855831.4365506,"logger":"controller-runtime.builder","msg":"skip registering a mutating webhook, object does not implement admission.Defaulter or WithDefaulter wasn't called","GVK":"metallb.io/v1beta2, Kind=BGPPeer"}
controller-7fdbd8896f-hgj2x controller {"level":"info","ts":1677855831.4365582,"logger":"controller-runtime.builder","msg":"Registering a validating webhook","GVK":"metallb.io/v1beta2, Kind=BGPPeer","path":"/validate-metallb-io-v1beta2-bgppeer"}
controller-7fdbd8896f-hgj2x controller {"level":"info","ts":1677855831.436581,"logger":"controller-runtime.webhook","msg":"Registering webhook","path":"/validate-metallb-io-v1beta2-bgppeer"}
controller-7fdbd8896f-hgj2x controller {"level":"info","ts":1677855831.4365811,"logger":"controller-runtime.certwatcher","msg":"Updated current TLS certificate"}
```

### speaker

#### 起動時

```
speaker-t5ktb speaker {"caller":"k8s.go:389","level":"info","msg":"Starting Manager","op":"Run","ts":"2023-03-03T15:03:13Z"}
speaker-t5ktb speaker {"caller":"speakerlist.go:310","level":"info","msg":"node event - forcing sync","node addr":"172.18.0.3","node event":"NodeJoin","node name":"kind-worker","ts":"2023-03-03T15:03:13Z"}
speaker-t5ktb speaker {"level":"info","ts":1677855793.300656,"msg":"Starting EventSource","controller":"bgppeer","controllerGroup":"metallb.io","controllerKind":"BGPPeer","source":"kind source: *v1beta2.BGPPeer"}
speaker-t5ktb speaker {"level":"info","ts":1677855793.3007903,"msg":"Starting EventSource","controller":"bgppeer","controllerGroup":"metallb.io","controllerKind":"BGPPeer","source":"kind source: *v1beta1.IPAddressPool"}
speaker-t5ktb speaker {"level":"info","ts":1677855793.3008003,"msg":"Starting EventSource","controller":"bgppeer","controllerGroup":"metallb.io","controllerKind":"BGPPeer","source":"kind source: *v1.Node"}
speaker-t5ktb speaker {"level":"info","ts":1677855793.3008056,"msg":"Starting EventSource","controller":"bgppeer","controllerGroup":"metallb.io","controllerKind":"BGPPeer","source":"kind source: *v1beta1.BGPAdvertisement"}
```
