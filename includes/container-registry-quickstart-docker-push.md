---
title: include dosyası
description: include dosyası
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: include
ms.date: 01/23/2019
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 09eaf9465ec3912dea6e1f3ee1693f6bfed50abc
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "67188927"
---
## <a name="push-image-to-registry"></a>Kayıt defterine görüntü gönderme

Azure Container kayıt defterine görüntü gönderebilmeniz için önce bir görüntünüz olmalıdır. Henüz yerel kapsayıcı görüntüleriniz yoksa, Docker Hub'dan varolan bir görüntüyü çekmek için aşağıdaki [docker çekme][docker-pull] komutunu çalıştırın. Bu örnekte, `hello-world` görüntüyü çekin.

```
docker pull hello-world
```

Kayıt defterinize görüntü gönderebilmeniz için, ACR oturum açma sunucunuzun tam adını etiketlemeniz gerekir. Giriş sunucusu adı biçim * \<kayıt defteri adı\>.azurecr.io* (tüm küçük harf), örneğin, *mycontainerregistry007.azurecr.io.*

Görüntüyü [docker tag][docker-tag] komutunu kullanarak etiketleyin. `<acrLoginServer>` değerini, ACR örneğinizin sunucu adıyla değiştirin.

```
docker tag hello-world <acrLoginServer>/hello-world:v1
```

Son olarak, [docker push][docker-push] komutunu kullanarak görüntüyü ACR örneğine gönderin. `<acrLoginServer>` değerini, ACR örneğinizin sunucu adıyla değiştirin. Bu örnek, **hello-world** `hello-world:v1` görüntüyü içeren merhaba dünyası deposu oluşturur.

```
docker push <acrLoginServer>/hello-world:v1
```

Görüntüyü konteyner kayıt defterinize ittikten `hello-world:v1` sonra, resmi yerel Docker ortamınızdan kaldırın. (Bu [docker rmi][docker-rmi] komutu, Azure konteyner kayıt defterinizdeki **merhaba dünyası** deposundan görüntüyü kaldırmaz.)

```
docker rmi <acrLoginServer>/hello-world:v1
```

<!-- LINKS - External -->
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-pull]: https://docs.docker.com/engine/reference/commandline/pull/
[docker-rmi]: https://docs.docker.com/engine/reference/commandline/rmi/
[docker-run]: https://docs.docker.com/engine/reference/commandline/run/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/

<!-- LINKS - Internal -->

