---
author: IEvangelist
ms.author: dapine
ms.date: 06/25/2019
ms.service: cognitive-services
ms.topic: include
ms.openlocfilehash: b9d0d2b97472eb3264f5e4600fddbfc7d3250918
ms.sourcegitcommit: ce4a99b493f8cf2d2fd4e29d9ba92f5f942a754c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/28/2019
ms.locfileid: "67704272"
---
## <a name="use-the-docker-cli-to-authenticate-the-private-container-registry"></a>Özel kapsayıcı kayıt defterinin kimliğini doğrulamak için Docker CLı 'yi kullanma

Çeşitli yollarla bilişsel hizmetler kapsayıcıları için özel kapsayıcı kayıt defteriyle kimlik doğrulaması yapabilirsiniz, ancak komut satırından önerilen yöntem [Docker CLI](https://docs.docker.com/engine/reference/commandline/cli/)'yı kullanmaktır.

Bilişsel hizmetler kapsayıcıları için özel kapsayıcı kayıt defterinde `containerpreview.azurecr.io`oturum açmak için aşağıdaki örnekte gösterildiği gibi [`docker login` komutunu](https://docs.docker.com/engine/reference/commandline/login/)kullanın. *\<username\>* Kullanıcı adı ve *\<Password\>* ile Azure bilişsel hizmetler takımınızdan aldığınız kimlik bilgilerinde sağlanmış olan parolayla değiştirin.

```
docker login containerpreview.azurecr.io -u <username> -p <password>
```

Kimlik bilgilerinizi bir metin dosyasında güvenli hale yaptıysanız, aşağıdaki örnekte gösterildiği gibi, `cat` komutunu kullanarak bu metin dosyasının içeriğini `docker login` komutuna ekleyebilirsiniz. *\<passwordFile\>* parolasını içeren metin dosyasının yolu ve adı ile\<kullanıcı adı *\>* kimlik bilgilerinizle belirtilen kullanıcı adıyla değiştirin.

```
cat <passwordFile> | docker login containerpreview.azurecr.io -u <username> --password-stdin
```
