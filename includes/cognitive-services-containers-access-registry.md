---
author: IEvangelist
ms.author: dapine
ms.date: 06/25/2019
ms.service: cognitive-services
ms.topic: include
ms.openlocfilehash: b9d0d2b97472eb3264f5e4600fddbfc7d3250918
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "67704272"
---
## <a name="use-the-docker-cli-to-authenticate-the-private-container-registry"></a>Özel kapsayıcı kayıt defterinin kimliğini doğrulamak için Docker CLı 'yi kullanma

Çeşitli yollarla bilişsel hizmetler kapsayıcıları için özel kapsayıcı kayıt defteriyle kimlik doğrulaması yapabilirsiniz, ancak komut satırından önerilen yöntem [Docker CLI](https://docs.docker.com/engine/reference/commandline/cli/)'yı kullanmaktır.

Aşağıdaki örnekte gösterildiği gibi [ `docker login` komutunu](https://docs.docker.com/engine/reference/commandline/login/), bilişsel `containerpreview.azurecr.io` Hizmetler kapsayıcıları için özel kapsayıcı kayıt defteri ' nde oturum açmak için kullanın. *\<username\>* Kullanıcı adıyla ve Azure bilişsel *\<password\>* Hizmetler takımınızdan aldığınız kimlik bilgilerinde belirtilen parolayla değiştirin.

```
docker login containerpreview.azurecr.io -u <username> -p <password>
```

Kimlik bilgilerinizi bir metin dosyasında güvenli hale yaptıysanız, komutunu kullanarak, `cat` `docker login` Aşağıdaki örnekte gösterildiği gibi, bu metin dosyasının içeriğini komut ile birleştirebilirsiniz. *\<passwordFile\>* Parolayı ve *\<username\>* kimlik bilgilerinizle belirtilen kullanıcı adını içeren metin dosyasının yolu ve adıyla değiştirin.

```
cat <passwordFile> | docker login containerpreview.azurecr.io -u <username> --password-stdin
```
