---
title: dosya dahil etme
description: dosya dahil etme
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: include
ms.date: 08/04/2020
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 16c1dec9e52be98e5b4e0103a583bdc219bc2417
ms.sourcegitcommit: bfeae16fa5db56c1ec1fe75e0597d8194522b396
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2020
ms.locfileid: "88031820"
---
## <a name="push-image-to-registry"></a>Kayıt defterine görüntü gönderme

Azure Container kayıt defterine görüntü gönderebilmeniz için önce bir görüntünüz olmalıdır. Henüz herhangi bir yerel kapsayıcı görüntünüz yoksa, Docker Hub 'ından mevcut bir görüntüyü çekmek için aşağıdaki [Docker Pull][docker-pull] komutunu çalıştırın. Bu örnek için `hello-world` görüntüyü çekin.

```
docker pull hello-world
```

Bir görüntüyü Kayıt defterinize gönderebilmeniz için önce kayıt defteri oturum açma sunucunuzun tam adı ile etiketlemelisiniz. Oturum açma sunucusu adı * \<registry-name\> . azurecr.io* biçimindedir (tümü küçük harf), örneğin, *mycontainerregistry.azurecr.io*.

Görüntüyü [docker tag][docker-tag] komutunu kullanarak etiketleyin. `<login-server>` değerini, ACR örneğinizin sunucu adıyla değiştirin.

```
docker tag hello-world <login-server>/hello-world:v1
```

Örnek:

```
docker tag hello-world mycontainerregistry.azurecr.io/hello-world:v1
```


Son olarak, [Docker Push][docker-push] kullanarak görüntüyü kayıt defteri örneğine gönderin. `<login-server>`Kayıt defteri örneğinizin oturum açma sunucusu adıyla değiştirin. Bu örnek, görüntüyü içeren **Merhaba-Dünya** deposunu oluşturur `hello-world:v1` .

```
docker push <login-server>/hello-world:v1
```

Görüntüyü kapsayıcı Kayıt defterinize gönderdikten sonra, `hello-world:v1` görüntüyü yerel Docker ortamınızdan kaldırın. (Bu [Docker rmi][docker-rmi] komutunun, görüntüyü Azure Container Registry 'nizin **Hello-World** deposundan kaldırmadığını unutmayın.)

```
docker rmi <login-server>/hello-world:v1
```

<!-- LINKS - External -->
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-pull]: https://docs.docker.com/engine/reference/commandline/pull/
[docker-rmi]: https://docs.docker.com/engine/reference/commandline/rmi/
[docker-run]: https://docs.docker.com/engine/reference/commandline/run/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/

<!-- LINKS - Internal -->

