---
author: IEvangelist
ms.author: dapine
ms.service: cognitive-services
ms.topic: include
ms.date: 03/04/2020
ms.openlocfilehash: 88d83676de1e7fa18c4c1dcbf347da8d685ba2fa
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83593349"
---
Kapsayıcıya erişim istemek için bilişsel [Hizmetler kapsayıcıları istek formunu](https://aka.ms/cognitivegate) doldurun ve iletin. Form, siz, şirketiniz ve kapsayıcısını kullanacağınız kullanıcı senaryosu hakkında bilgi ister. Formu gönderdikten sonra, Azure bilişsel hizmetler ekibi, özel kapsayıcı kayıt defterine erişim ölçütlerini karşıladığınızdan emin olmak için bunu inceler.

> [!IMPORTANT]
> Formunda bir Microsoft hesabı (MSA) veya Azure Active Directory (Azure AD) hesabıyla ilişkili bir e-posta adresi kullanmanız gerekir. Kabul ölçütleri hakkında daha fazla bilgi için bkz. [bilişsel hizmetler-başarı işlemi](../articles/cognitive-services/cognitive-services-gating-process.md).

İsteğiniz onaylanırsa, kimlik bilgilerinizin nasıl alınacağını ve özel kapsayıcı kayıt defterine nasıl erişebileceğini açıklayan yönergeler içeren bir e-posta alırsınız.

## <a name="log-in-to-the-private-container-registry"></a>Özel kapsayıcı kayıt defterinde oturum açma

Bilişsel hizmetler kapsayıcıları için özel kapsayıcı kayıt defteriyle kimlik doğrulamanın birkaç yolu vardır. [Docker CLI](https://docs.docker.com/engine/reference/commandline/cli/)kullanarak komut satırı yöntemini kullanmanızı öneririz.

Bilişsel hizmetler kapsayıcıları için özel kapsayıcı kayıt defteri olan oturumu açmak için aşağıdaki örnekte gösterildiği gibi [Docker Login](https://docs.docker.com/engine/reference/commandline/login/) komutunu kullanın `containerpreview.azurecr.io` . Kullanıcı adını, Azure bilişsel hizmetler takımınızdan aldığınız kimlik bilgilerinde belirtilen parola ile Kullanıcı adı ve * \< parola \> * *ile değiştirin. \< \> *

```
docker login containerpreview.azurecr.io -u <username> -p <password>
```

Kimlik bilgilerinizi bir metin dosyasında güvenli hale getirebilirsiniz, bu metin dosyasının içeriğini `docker login` komutuna birleştirebilirsiniz. `cat`Aşağıdaki örnekte gösterildiği gibi komutunu kullanın. * \< PasswordFile \> * değerini parolayı içeren metin dosyasının yolu ve adıyla değiştirin. Kullanıcı adını kimlik bilgilerinizle belirtilen kullanıcı *adıyla değiştirin. \< \> *

```
cat <passwordFile> | docker login containerpreview.azurecr.io -u <username> --password-stdin
```

