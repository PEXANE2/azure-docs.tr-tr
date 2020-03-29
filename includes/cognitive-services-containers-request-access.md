---
author: IEvangelist
ms.author: dapine
ms.service: cognitive-services
ms.topic: include
ms.date: 7/5/2019
ms.openlocfilehash: 993b0e8cc5b1ec482b2f6041dfc970dc7e7409dd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "68229190"
---
Konteynıra erişim istemek için [Bilişsel Hizmetler Vizyon Kapları İstek formunu](https://aka.ms/VisionContainersPreview) doldurun ve gönderin. Form, sizin, şirketiniz ve kapsayıcıyı kullanacağınız kullanıcı senaryosu hakkında bilgi ister. Formu gönderdikten sonra, Azure Bilişsel Hizmetler ekibi, özel konteyner kayıt defterine erişim ölçütlerini karşıladığınızdan emin olmak için formu gözden geçirir.

> [!IMPORTANT]
> Formda bir Microsoft Hesabı (MSA) veya Azure Etkin Dizin (Azure AD) hesabıyla ilişkili bir e-posta adresi kullanmanız gerekir.

İsteğiniz onaylanırsa, kimlik bilgilerinizi nasıl elde ettiğinizi ve özel konteyner kayıt defterine nasıl erişilenleri açıklayan yönergeler içeren bir e-posta alırsınız.

## <a name="log-in-to-the-private-container-registry"></a>Özel konteyner kayıt defterine giriş yapın

Bilişsel Hizmetler kapsayıcıları için özel konteyner kayıt defteri ile doğrulamanın çeşitli yolları vardır. [Docker CLI'yi](https://docs.docker.com/engine/reference/commandline/cli/)kullanarak komut satırı yöntemini kullanmanızı öneririz.

Aşağıdaki örnekte `containerpreview.azurecr.io`gösterildiği [gibi,](https://docs.docker.com/engine/reference/commandline/login/) Bilişsel Hizmetler kapsayıcıları için özel konteyner kayıt defteri olan giriş yapmak için docker giriş komutunu kullanın. * \<Kullanıcı\> adını,* Azure Bilişsel Hizmetler ekibinden aldığınız kimlik bilgilerinde sağlanan parolayla kullanıcı adı ve * \<\> parolayla* değiştirin.

```
docker login containerpreview.azurecr.io -u <username> -p <password>
```

Kimlik bilgilerinizi bir metin dosyasında güvenli hale geldiyseniz, bu metin dosyasının içeriğini `docker login` komuta ekleyebilirsiniz. Aşağıdaki `cat` örnekte gösterildiği gibi komutu kullanın. * \<passwordFile'ı\> * parolayı içeren metin dosyasının yolu ve adı ile değiştirin. * \<Kullanıcı adını\> * kimlik bilgilerinizde sağlanan kullanıcı adı ile değiştirin.

```
cat <passwordFile> | docker login containerpreview.azurecr.io -u <username> --password-stdin
```

