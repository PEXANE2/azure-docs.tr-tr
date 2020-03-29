---
author: IEvangelist
ms.author: dapine
ms.date: 06/25/2019
ms.service: cognitive-services
ms.topic: include
ms.openlocfilehash: b9d0d2b97472eb3264f5e4600fddbfc7d3250918
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "67704272"
---
## <a name="use-the-docker-cli-to-authenticate-the-private-container-registry"></a>Özel konteyner kayıt defterinin kimliğini doğrulamak için Docker CLI'yi kullanın

Bilişsel Hizmetler Kapsayıcıları için özel konteyner kayıt defteriile çeşitli şekillerde kimlik doğrulayabilirsiniz, ancak komut satırından önerilen yöntem [Docker CLI'yi](https://docs.docker.com/engine/reference/commandline/cli/)kullanmaktır.

Aşağıdaki örnekte gösterildiği gibi, Bilişsel Hizmetler Kapsayıcıları için özel kapsayıcı kayıt defterine `containerpreview.azurecr.io`giriş yapmak için [ `docker login` komutu](https://docs.docker.com/engine/reference/commandline/login/)kullanın. * \<Kullanıcı\> adını,* Azure Bilişsel Hizmetler ekibinden aldığınız kimlik bilgilerinde sağlanan parolayla kullanıcı adı ve * \<\> parolayla* değiştirin.

```
docker login containerpreview.azurecr.io -u <username> -p <password>
```

Kimlik bilgilerinizi bir metin dosyasında güvence altına almışsanız, aşağıdaki örnekte gösterildiği gibi `cat` komutu `docker login` kullanarak bu metin dosyasının içeriğini biraraya bulabilirsiniz. * \<passwordFile'ı,\> * kimlik bilgilerinizde sağlanan kullanıcı adı ile parola ve * \<kullanıcı adını\> * içeren metin dosyasının yolu ve adı ile değiştirin.

```
cat <passwordFile> | docker login containerpreview.azurecr.io -u <username> --password-stdin
```
