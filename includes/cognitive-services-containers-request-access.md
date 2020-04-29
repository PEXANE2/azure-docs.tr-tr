---
author: IEvangelist
ms.author: dapine
ms.service: cognitive-services
ms.topic: include
ms.date: 7/5/2019
ms.openlocfilehash: 993b0e8cc5b1ec482b2f6041dfc970dc7e7409dd
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "68229190"
---
Kapsayıcıya erişim istemek için bilişsel [Hizmetler Vision kapsayıcıları istek formunu](https://aka.ms/VisionContainersPreview) doldurun ve iletin. Form, siz, şirketiniz ve kapsayıcısını kullanacağınız kullanıcı senaryosu hakkında bilgi ister. Formu gönderdikten sonra, Azure bilişsel hizmetler ekibi, özel kapsayıcı kayıt defterine erişim ölçütlerini karşıladığınızdan emin olmak için bunu inceler.

> [!IMPORTANT]
> Formunda bir Microsoft hesabı (MSA) veya Azure Active Directory (Azure AD) hesabıyla ilişkili bir e-posta adresi kullanmanız gerekir.

İsteğiniz onaylanırsa, kimlik bilgilerinizin nasıl alınacağını ve özel kapsayıcı kayıt defterine nasıl erişebileceğini açıklayan yönergeler içeren bir e-posta alırsınız.

## <a name="log-in-to-the-private-container-registry"></a>Özel kapsayıcı kayıt defterinde oturum açma

Bilişsel hizmetler kapsayıcıları için özel kapsayıcı kayıt defteriyle kimlik doğrulamanın birkaç yolu vardır. [Docker CLI](https://docs.docker.com/engine/reference/commandline/cli/)kullanarak komut satırı yöntemini kullanmanızı öneririz.

Bilişsel hizmetler kapsayıcıları için özel kapsayıcı kayıt defteri olan oturumu `containerpreview.azurecr.io`açmak için aşağıdaki örnekte gösterildiği gibi [Docker Login](https://docs.docker.com/engine/reference/commandline/login/) komutunu kullanın. Kullanıcı adını, Azure bilişsel hizmetler takımınızdan aldığınız kimlik bilgilerinde belirtilen parola ile Kullanıcı adı ve * \<parola\> * *ile değiştirin \<\> *

```
docker login containerpreview.azurecr.io -u <username> -p <password>
```

Kimlik bilgilerinizi bir metin dosyasında güvenli hale getirebilirsiniz, bu metin dosyasının içeriğini `docker login` komutuna birleştirebilirsiniz. Aşağıdaki örnekte `cat` gösterildiği gibi komutunu kullanın. * \<PasswordFile\> * değerini parolayı içeren metin dosyasının yolu ve adıyla değiştirin. Kullanıcı adını kimlik bilgilerinizle belirtilen kullanıcı *adıyla değiştirin \<\> *

```
cat <passwordFile> | docker login containerpreview.azurecr.io -u <username> --password-stdin
```

