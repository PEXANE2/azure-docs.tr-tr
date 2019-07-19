---
title: Azure Container Registry ile kimlik doğrulama
description: Bir Azure Active Directory kimlikle oturum açma, hizmet sorumlularını kullanma ve isteğe bağlı yönetici kimlik bilgilerini kullanma dahil olmak üzere bir Azure Container Registry için kimlik doğrulama seçenekleri.
services: container-registry
author: dlepow
manager: gwallace
ms.service: container-registry
ms.topic: article
ms.date: 12/21/2018
ms.author: danlep
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 82fe80e098ee95c09c4a1400068ab813910e0e1a
ms.sourcegitcommit: f5075cffb60128360a9e2e0a538a29652b409af9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68309828"
---
# <a name="authenticate-with-a-private-docker-container-registry"></a>Özel bir Docker kapsayıcı kayıt defteri ile kimlik doğrulama

Her biri bir veya daha fazla kayıt defteri kullanımı senaryosu için geçerli olan bir Azure Container Registry ile kimlik doğrulamanın birkaç yolu vardır.

[Tek bir oturum açma](#individual-login-with-azure-ad)yoluyla doğrudan bir kayıt defterinde oturum açabilirsiniz veya Uygulamalarınız ve kapsayıcı düzenleyiciler Azure Active Directory (Azure AD) [hizmet sorumlusu](#service-principal)kullanarak katılımsız veya "gözetimsiz" kimlik doğrulaması yapabilir.

## <a name="individual-login-with-azure-ad"></a>Azure AD ile bireysel oturum açma

Kayıt defteriyle doğrudan bir geliştirme iş istasyonundan görüntü çekme ve gönderme gibi, [Azure CLI](/cli/azure/install-azure-cli)'de [az ACR Login](/cli/azure/acr?view=azure-cli-latest#az-acr-login) komutunu kullanarak kimlik doğrulaması yapma.

```azurecli
az acr login --name <acrName>
```

İle `az acr login`oturum açtığınızda, CLI, kayıt defterinizle oturumunuzu sorunsuz bir şekilde doğrulamak için [az Login](/cli/azure/reference-index#az-login) çalıştırdığınızda oluşturulan belirteci kullanır. Bu şekilde oturum açtıktan sonra, kimlik bilgileriniz önbelleğe alınır ve oturuminizdeki sonraki `docker` komutlar Kullanıcı adı veya parola gerektirmez. 

Kayıt defteri erişimi için tarafından `az acr login` kullanılan belirteç 1 saat için geçerlidir; bu nedenle, bir `docker` komutu çalıştırmadan önce her zaman kayıt defterinde oturum açmanız önerilir. Belirtecin süresi dolarsa, yeniden kimlik doğrulaması yapmak için `az acr login` komutunu tekrar kullanarak yenileyebilirsiniz. 

Azure `az acr login` kimlikleri ile kullanılması, [rol tabanlı erişim](../role-based-access-control/role-assignments-portal.md)sağlar. Bazı senaryolarda, Azure AD 'de kendi bireysel kimliklerinizi kullanarak bir kayıt defterinde oturum açmak isteyebilirsiniz. Şirketler arası senaryolar için veya bireysel erişimi yönetmek istemediğiniz bir çalışma grubunun ihtiyaçlarını işlemek üzere [Azure kaynakları için yönetilen bir kimlikle](container-registry-authentication-managed-identity.md)da oturum açabilirsiniz.

## <a name="service-principal"></a>Hizmet sorumlusu

Kayıt defterinize bir [hizmet sorumlusu](../active-directory/develop/app-objects-and-service-principals.md) atarsanız, uygulamanız veya hizmetiniz bunu gözetimsiz kimlik doğrulaması için kullanabilir. Hizmet sorumluları, bir kayıt defterine [rol tabanlı erişime](../role-based-access-control/role-assignments-portal.md) izin verir ve bir kayıt defterine birden çok hizmet sorumlusu atayabilirsiniz. Birden çok hizmet sorumlusu, farklı uygulamalar için farklı erişim tanımlamanızı sağlar.

Bir kapsayıcı kayıt defteri için kullanılabilir roller şunlardır:

* **Acrpull**: çekme

* **Acrpush**: çekme ve gönderme

* **Sahip**: diğer kullanıcılara roller çekme, gönderme ve atama

Rollerin tüm listesi için bkz. [Azure Container Registry rolleri ve izinleri](container-registry-roles.md).

CLı betikleri için bir Azure Container Registry ile kimlik doğrulaması yapmak üzere bir hizmet sorumlusu uygulama KIMLIĞI ve parolası oluşturmak veya mevcut bir hizmet sorumlusunu kullanmak için bkz. [hizmet sorumluları ile Azure Container Registry kimlik doğrulaması](container-registry-auth-service-principal.md).

Hizmet sorumluları, aşağıdaki gibi hem çekme hem de itme senaryolarında bir kayıt defterine gözetimsiz bağlantı sağlar:

  * *Çekme*: Bir kayıt defterinden Kubernetes, DC/OS ve Docker Sısınma dahil olmak üzere düzenleme sistemlerine kapsayıcı dağıtın. Ayrıca, kapsayıcı kayıt defterlerinden [Azure Kubernetes hizmeti](container-registry-auth-aks.md), [Azure Container Instances](container-registry-auth-aci.md), [App Service](../app-service/index.yml), [Batch](../batch/index.yml), [Service Fabric](/azure/service-fabric/)ve diğerleri gibi ilgili Azure hizmetlerine de çekebilirsiniz.

  * *Gönderim*: Kapsayıcı görüntüleri oluşturun ve Azure Pipelines veya Jenkins gibi sürekli tümleştirme ve dağıtım çözümlerini kullanarak bunları bir kayıt defterine gönderin.

Ayrıca, bir hizmet sorumlusu ile doğrudan oturum açabilirsiniz. Aşağıdaki komutu çalıştırdığınızda, istendiğinde hizmet sorumlusu AppID (Kullanıcı adı) ve parolayı etkileşimli olarak sağlayın. Oturum açma kimlik bilgilerini yönetmek için en iyi uygulamalar için bkz. [Docker Login](https://docs.docker.com/engine/reference/commandline/login/) komut başvurusu:

```
docker login myregistry.azurecr.io
```

Oturum açıldıktan sonra Docker kimlik bilgilerini önbelleğe alır, bu nedenle uygulama KIMLIĞINI hatırlamak zorunda kalmazsınız.

> [!TIP]
> [Az ad SP Reset-Credentials](/cli/azure/ad/sp?view=azure-cli-latest) komutunu çalıştırarak bir hizmet sorumlusunun parolasını yeniden oluşturabilirsiniz.
>

## <a name="admin-account"></a>Yönetici hesabı

Her kapsayıcı kayıt defteri, varsayılan olarak devre dışı bırakılan bir yönetici kullanıcı hesabı içerir. Yönetici kullanıcıyı etkinleştirebilir ve Azure portal kimlik bilgilerini yönetebilir veya Azure CLı veya diğer Azure araçlarını kullanarak yapılandırabilirsiniz.

> [!IMPORTANT]
> Yönetici hesabı, genellikle test amacıyla tek bir kullanıcının kayıt defterine erişmesi için tasarlanmıştır. Yönetici hesabı kimlik bilgilerini birden çok kullanıcıyla paylaşmayı önermiyoruz. Yönetici hesabıyla kimlik doğrulaması yapan tüm kullanıcılar, kayıt defterine gönderme ve çekme erişimi olan tek bir kullanıcı olarak görüntülenir. Bu hesabın değiştirilmesi veya devre dışı bırakılması, kimlik bilgilerini kullanan tüm kullanıcılar için kayıt defteri erişimini devre dışı bırakır. Gözetimsiz senaryolara yönelik kullanıcılar ve hizmet sorumluları için bireysel kimlik önerilir.
>

Yönetici hesabı, her ikisi de yeniden üretilbilen iki parolayla sağlanır. İki parola, diğerini yeniden oluştururken bir parola kullanarak kayıt defteriyle bağlantıyı korumanıza olanak sağlar. Yönetici hesabı etkinleştirilmişse, kayıt defterine temel kimlik doğrulaması sorulduğunda, Kullanıcı adını ve parolayı `docker login` komuta geçirebilirsiniz. Örneğin:

```
docker login myregistry.azurecr.io 
```


Mevcut bir kayıt defteri için yönetici kullanıcıyı etkinleştirmek üzere, Azure CLI 'de `--admin-enabled` [az ACR Update](/cli/azure/acr?view=azure-cli-latest#az-acr-update) komutunun parametresini kullanabilirsiniz:

```azurecli
az acr update -n <acrName> --admin-enabled true
```

Kayıt defterinizde gezinerek yönetici kullanıcıyı Azure portal etkinleştirebilirsiniz, **Ayarlar**altında **erişim anahtarları** ' nı seçip **Yönetici Kullanıcı**altında **etkinleştirin** .

![Azure portal Yönetici Kullanıcı Kullanıcı arabirimini etkinleştirme][auth-portal-01]

## <a name="next-steps"></a>Sonraki adımlar

* [Azure CLı kullanarak ilk görüntünüzü gönderin](container-registry-get-started-azure-cli.md)

<!-- IMAGES -->
[auth-portal-01]: ./media/container-registry-authentication/auth-portal-01.png
