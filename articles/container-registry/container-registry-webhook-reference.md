---
title: Kayıt defteri webhook şema başvurusu
description: Yapı itme veya silme olayları için webhook'lar etkinleştirildiğinde oluşturulan Azure kapsayıcı kayıt defterindeki webhook istekleri için JSON yükü için başvuru
ms.topic: article
ms.date: 03/05/2019
ms.openlocfilehash: 8354ef9db24d5825238155ac567d5d829f9b0d7f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74455967"
---
# <a name="azure-container-registry-webhook-reference"></a>Azure Konteyner Kayıt Defteri webhook başvurusu

Konteyner kayıt defteriniz için [web hooks'ları yapılandırabilirsiniz](container-registry-webhook.md) ve bu da belirli eylemler gerçekleştirildiğinde olay oluşturur. Örneğin, bir kapsayıcı resmi veya Miğfer grafiği bir kayıt defterine itildiğinde veya silindiğinde tetiklenen web hook'ları etkinleştirin. Bir web hook tetiklendiğinde, Azure Kapsayıcı Kayıt Defteri, olayla ilgili bilgileri içeren bir HTTP veya HTTPS isteğini belirttiğiniz bir bitiş noktasına kadar yayınlar. Bitiş noktanız daha sonra webhook'u işleyebilir ve buna göre hareket edebilir.

Aşağıdaki bölümlerde desteklenen olaylar tarafından oluşturulan webhook isteklerinin şeması ayrıntılı. Olay bölümleri, olay türü için yük şema, bir örnek istek yükü ve webhook tetiklemek bir veya daha fazla örnek komutları içerir.

Azure kapsayıcı kayıt defteriniz için web hooks yapılandırma hakkında daha fazla bilgi için [bkz.](container-registry-webhook.md)

## <a name="webhook-requests"></a>Webhook istekleri

### <a name="http-request"></a>HTTP isteği

Tetiklenen bir webhook, `POST` webhook'u yapılandırdığınızda belirttiğiniz URL bitiş noktasına BIR HTTP isteği nde bulunun.

### <a name="http-headers"></a>HTTP üstbilgi

Webhook istekleri, `Content-Type` `application/json` webhook'unuzun `Content-Type` özel üstbilgisini belirtmediyseniz bir tane içerir.

Webhook için belirtmiş olabileceğiniz özel üstbilgidışında isteğe başka üstbilgi eklenmez.

## <a name="push-event"></a>Itme olayı

Webhook, bir kapsayıcı görüntüsü depoya itildiğinde tetiklenir.

### <a name="push-event-payload"></a>Itme olay yükü

|Öğe|Tür|Açıklama|
|-------------|----------|-----------|
|`id`|Dize|Webhook olayının kimliği.|
|`timestamp`|DateTime|Webhook olayının tetiklendiği saat.|
|`action`|Dize|Webhook olayını tetikleyen eylem.|
|[Hedef](#target)|Karmaşık Tür|Webhook olayını tetikleyen olayın hedefi.|
|[Istek](#request)|Karmaşık Tür|Webhook olayını oluşturan istek.|

### <a name="target"></a><a name="target"></a>Hedef

|Öğe|Tür|Açıklama|
|------------------|----------|-----------|
|`mediaType`|Dize|Başvurulan nesnenin MIME türü.|
|`size`|Int32|İçeriğin bayt sayısı. Uzunluk alanıyla aynı.|
|`digest`|Dize|Kayıt Defteri V2 HTTP API Belirtimi tarafından tanımlandığı gibi içeriğin özeti.|
|`length`|Int32|İçeriğin bayt sayısı. Boyut alanıyla aynı.|
|`repository`|Dize|Depo adı.|
|`tag`|Dize|Resim etiketi adı.|

### <a name="request"></a><a name="request"></a>Istek

|Öğe|Tür|Açıklama|
|------------------|----------|-----------|
|`id`|Dize|Olayı başlatan isteğin kimliği.|
|`host`|Dize|Gelen isteklerde HTTP ana bilgisayar üstbilgisinde belirtildiği gibi, kayıt defteri örneğinin dışarıdan erişilebilir ana adı.|
|`method`|Dize|Olayı oluşturan istek yöntemi.|
|`useragent`|Dize|İsteğin kullanıcı aracısı üstbilgisi.|

### <a name="payload-example-image-push-event"></a>Yük örneği: görüntü itme olayı

```JSON
{
  "id": "cb8c3971-9adc-488b-xxxx-43cbb4974ff5",
  "timestamp": "2017-11-17T16:52:01.343145347Z",
  "action": "push",
  "target": {
    "mediaType": "application/vnd.docker.distribution.manifest.v2+json",
    "size": 524,
    "digest": "sha256:xxxxd5c8786bb9e621a45ece0dbxxxx1cdc624ad20da9fe62e9d25490f33xxxx",
    "length": 524,
    "repository": "hello-world",
    "tag": "v1"
  },
  "request": {
    "id": "3cbb6949-7549-4fa1-xxxx-a6d5451dffc7",
    "host": "myregistry.azurecr.io",
    "method": "PUT",
    "useragent": "docker/17.09.0-ce go/go1.8.3 git-commit/afdb6d4 kernel/4.10.0-27-generic os/linux arch/amd64 UpstreamClient(Docker-Client/17.09.0-ce \\(linux\\))"
  }
}
```

Görüntü **itme** olay webhook tetikleyen Örnek [Docker CLI](https://docs.docker.com/engine/reference/commandline/cli/) komutu:

```bash
docker push myregistry.azurecr.io/hello-world:v1
```

## <a name="chart-push-event"></a>Grafik itme olayı

Bir Miğfer grafiği bir depoya itildiğinde Webhook tetiklenir.

### <a name="chart-push-event-payload"></a>Grafik itme olay yükü

|Öğe|Tür|Açıklama|
|-------------|----------|-----------|
|`id`|Dize|Webhook olayının kimliği.|
|`timestamp`|DateTime|Webhook olayının tetiklendiği saat.|
|`action`|Dize|Webhook olayını tetikleyen eylem.|
|[Hedef](#helm_target)|Karmaşık Tür|Webhook olayını tetikleyen olayın hedefi.|

### <a name="target"></a><a name="helm_target"></a>Hedef

|Öğe|Tür|Açıklama|
|------------------|----------|-----------|
|`mediaType`|Dize|Başvurulan nesnenin MIME türü.|
|`size`|Int32|İçeriğin bayt sayısı.|
|`digest`|Dize|Kayıt Defteri V2 HTTP API Belirtimi tarafından tanımlandığı gibi içeriğin özeti.|
|`repository`|Dize|Depo adı.|
|`tag`|Dize|Grafik etiket adı.|
|`name`|Dize|Grafik adı.|
|`version`|Dize|Grafik versiyonu.|

### <a name="payload-example-chart-push-event"></a>Yük örneği: grafik itme olayı

```JSON
{
  "id": "6356e9e0-627f-4fed-xxxx-d9059b5143ac",
  "timestamp": "2019-03-05T23:45:31.2614267Z",
  "action": "chart_push",
  "target": {
    "mediaType": "application/vnd.acr.helm.chart",
    "size": 25265,
    "digest": "sha256:xxxx8075264b5ba7c14c23672xxxx52ae6a3ebac1c47916e4efe19cd624dxxxx",
    "repository": "repo",
    "tag": "wordpress-5.4.0.tgz",
    "name": "wordpress",
    "version": "5.4.0.tgz"
  }
}
```

Örnek [Azure CLI](/cli/azure/acr) komutu **chart_push** olay webhook tetikler:

```azurecli
az acr helm push wordpress-5.4.0.tgz --name MyRegistry
```

## <a name="delete-event"></a>Olayı silme

Bir görüntü deposu veya bildirimi silindiğinde webhook tetiklenir. Etiket silindiğinde tetiklenmez.

### <a name="delete-event-payload"></a>Olay yükünü silme

|Öğe|Tür|Açıklama|
|-------------|----------|-----------|
|`id`|Dize|Webhook olayının kimliği.|
|`timestamp`|DateTime|Webhook olayının tetiklendiği saat.|
|`action`|Dize|Webhook olayını tetikleyen eylem.|
|[Hedef](#delete_target)|Karmaşık Tür|Webhook olayını tetikleyen olayın hedefi.|
|[Istek](#delete_request)|Karmaşık Tür|Webhook olayını oluşturan istek.|

### <a name="target"></a><a name="delete_target"></a>Hedef

|Öğe|Tür|Açıklama|
|------------------|----------|-----------|
|`mediaType`|Dize|Başvurulan nesnenin MIME türü.|
|`digest`|Dize|Kayıt Defteri V2 HTTP API Belirtimi tarafından tanımlandığı gibi içeriğin özeti.|
|`repository`|Dize|Depo adı.|

### <a name="request"></a><a name="delete_request"></a>Istek

|Öğe|Tür|Açıklama|
|------------------|----------|-----------|
|`id`|Dize|Olayı başlatan isteğin kimliği.|
|`host`|Dize|Gelen isteklerde HTTP ana bilgisayar üstbilgisinde belirtildiği gibi, kayıt defteri örneğinin dışarıdan erişilebilir ana adı.|
|`method`|Dize|Olayı oluşturan istek yöntemi.|
|`useragent`|Dize|İsteğin kullanıcı aracısı üstbilgisi.|

### <a name="payload-example-image-delete-event"></a>Taşıma örneği: resim silme olayı

```JSON
{
    "id": "afc359ce-df7f-4e32-xxxx-1ff8aa80927b",
    "timestamp": "2017-11-17T16:54:53.657764628Z",
    "action": "delete",
    "target": {
      "mediaType": "application/vnd.docker.distribution.manifest.v2+json",
      "digest": "sha256:xxxxd5c8786bb9e621a45ece0dbxxxx1cdc624ad20da9fe62e9d25490f33xxxx",
      "repository": "hello-world"
    },
    "request": {
      "id": "3d78b540-ab61-4f75-xxxx-7ca9ecf559b3",
      "host": "myregistry.azurecr.io",
      "method": "DELETE",
      "useragent": "python-requests/2.18.4"
    }
  }
```

Örnek Bir **silme** olayı webhook tetikleyen [Azure CLI](/cli/azure/acr) komutları:

```azurecli
# Delete repository
az acr repository delete --name MyRegistry --repository MyRepository

# Delete image
az acr repository delete --name MyRegistry --image MyRepository:MyTag
```

## <a name="chart-delete-event"></a>Grafik silme olayı

Bir Miğfer grafiği veya deposu silindiğinde webhook tetiklenir. 

### <a name="chart-delete-event-payload"></a>Grafik silme olay yükü

|Öğe|Tür|Açıklama|
|-------------|----------|-----------|
|`id`|Dize|Webhook olayının kimliği.|
|`timestamp`|DateTime|Webhook olayının tetiklendiği saat.|
|`action`|Dize|Webhook olayını tetikleyen eylem.|
|[Hedef](#chart_delete_target)|Karmaşık Tür|Webhook olayını tetikleyen olayın hedefi.|

### <a name="target"></a><a name="chart_delete_target"></a>Hedef

|Öğe|Tür|Açıklama|
|------------------|----------|-----------|
|`mediaType`|Dize|Başvurulan nesnenin MIME türü.|
|`size`|Int32|İçeriğin bayt sayısı.|
|`digest`|Dize|Kayıt Defteri V2 HTTP API Belirtimi tarafından tanımlandığı gibi içeriğin özeti.|
|`repository`|Dize|Depo adı.|
|`tag`|Dize|Grafik etiket adı.|
|`name`|Dize|Grafik adı.|
|`version`|Dize|Grafik versiyonu.|

### <a name="payload-example-chart-delete-event"></a>Taşıma örneği: grafik silme olayı

```JSON
{
  "id": "338a3ef7-ad68-4128-xxxx-fdd3af8e8f67",
  "timestamp": "2019-03-06T00:10:48.1270754Z",
  "action": "chart_delete",
  "target": {
    "mediaType": "application/vnd.acr.helm.chart",
    "size": 25265,
    "digest": "sha256:xxxx8075264b5ba7c14c23672xxxx52ae6a3ebac1c47916e4efe19cd624dxxxx",
    "repository": "repo",
    "tag": "wordpress-5.4.0.tgz",
    "name": "wordpress",
    "version": "5.4.0.tgz"
  }
}
```

Örnek [Azure CLI](/cli/azure/acr) komutu **chart_delete** olay webhook tetikler:

```azurecli
az acr helm delete wordpress --version 5.4.0 --name MyRegistry
```

## <a name="next-steps"></a>Sonraki adımlar

[Azure Kapsayıcı Kayıt Defteri webhooks kullanma](container-registry-webhook.md)