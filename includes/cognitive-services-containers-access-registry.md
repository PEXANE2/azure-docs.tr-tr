---
author: IEvangelist
ms.author: dapine
ms.date: 06/25/2019
ms.service: cognitive-services
ms.topic: include
ms.openlocfilehash: b9d0d2b97472eb3264f5e4600fddbfc7d3250918
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "67704272"
---
## <a name="use-the-docker-cli-to-authenticate-the-private-container-registry"></a>Özel kapsayıcı kayıt defterinin kimliğini doğrulamak için Docker CLı 'yi kullanma

Çeşitli yollarla bilişsel hizmetler kapsayıcıları için özel kapsayıcı kayıt defteriyle kimlik doğrulaması yapabilirsiniz, ancak komut satırından önerilen yöntem [Docker CLI](https://docs.docker.com/engine/reference/commandline/cli/)'yı kullanmaktır.

Aşağıdaki örnekte gösterildiği gibi [ `docker login` komutunu](https://docs.docker.com/engine/reference/commandline/login/), bilişsel `containerpreview.azurecr.io` Hizmetler kapsayıcıları için özel kapsayıcı kayıt defteri ' nde oturum açmak için kullanın. Kullanıcı adını, Azure bilişsel hizmetler takımınızdan aldığınız kimlik bilgilerinde belirtilen parola ile Kullanıcı adı ve * \< parola \> * *ile değiştirin. \< \> *

```
docker login containerpreview.azurecr.io -u <username> -p <password>
```

Kimlik bilgilerinizi bir metin dosyasında güvenli hale yaptıysanız, komutunu kullanarak, `cat` `docker login` Aşağıdaki örnekte gösterildiği gibi, bu metin dosyasının içeriğini komut ile birleştirebilirsiniz. * \< PasswordFile \> * değerini, kimlik bilgilerinizle belirtilen kullanıcı adıyla * \< \> parolayı ve Kullanıcı adını içeren* metin dosyasının yolu ve adıyla değiştirin.

```
cat <passwordFile> | docker login containerpreview.azurecr.io -u <username> --password-stdin
```
