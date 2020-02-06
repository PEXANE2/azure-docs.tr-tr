---
title: Kayıt defteri kimlik doğrulama seçenekleri
description: Bir Azure Active Directory kimlikle oturum açma, hizmet sorumlularını kullanma ve isteğe bağlı yönetici kimlik bilgilerini kullanma dahil olmak üzere, özel bir Azure Kapsayıcı kayıt defteri için kimlik doğrulama seçenekleri.
ms.topic: article
ms.date: 01/30/2020
ms.openlocfilehash: 384f401a986c58dc6ce63384ce3e2a43b8db27fa
ms.sourcegitcommit: f0f73c51441aeb04a5c21a6e3205b7f520f8b0e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77029886"
---
# <a name="authenticate-with-an-azure-container-registry"></a>Azure Container Registry ile kimlik doğrulama

Her biri bir veya daha fazla kayıt defteri kullanımı senaryosu için geçerli olan bir Azure Container Registry ile kimlik doğrulamanın birkaç yolu vardır.

Önerilen yollarla, [tek tek oturum açma](#individual-login-with-azure-ad)yoluyla bir kayıt defterine doğrudan kimlik doğrulaması dahildir veya uygulamalarınızın ve kapsayıcı düzenleyicilerinin bir Azure Active Directory (Azure AD) [hizmet sorumlusu](#service-principal)kullanarak katılımsız veya "gözetimsiz" kimlik doğrulaması gerçekleştirebilir.

## <a name="authentication-options"></a>Kimlik doğrulaması seçenekleri

Aşağıdaki tabloda, kullanılabilir kimlik doğrulama yöntemleri ve önerilen senaryolar listelenmektedir. Ayrıntılar için bkz. bağlantılı içerik.

| Yöntem                               | Kimlik doğrulaması nasıl yapılır?                                           | Senaryolar                                                            | RBAC                             | Sınırlamalar                                |
|---------------------------------------|-------------------------------------------------------|---------------------------------------------------------------------|----------------------------------|--------------------------------------------|
| [BIREYSEL ad kimlik](#individual-login-with-azure-ad)                | Azure CLı 'de `az acr login`                              | Geliştiricilere ve test edicilere göre etkileşimli gönderim/çekme                                    | Yes                              | AD belirtecinin her 3 saatte bir yenilenmesi gerekir     |
| [Ad hizmet sorumlusu](#service-principal)                  | `docker login`<br/><br/>Azure CLı 'de `az acr login`<br/><br/> API 'lerde veya araç defterindeki kayıt defteri oturum açma ayarları<br/><br/> Kubernetes çekme gizli                                           | CI/CD ardışık düzeninde katılımsız gönderim<br/><br/> Azure 'a veya dış hizmetlere katılımsız çekme  | Yes                              | SP parolasının varsayılan süre sonu 1 yıldır       |                                                           
| [AKS  Ile tümleştirme](../aks/cluster-container-registry-integration.md?toc=/azure/container-registry/toc.json&bc=/azure/container-registry/breadcrumb/toc.json)                   | AKS kümesi oluşturulduğunda veya güncelleştirilirken kayıt defteri Ekle  | AKS kümesine katılımsız çekme                                                  | Hayır, yalnızca çekme erişimi             | Yalnızca AKS kümesi ile kullanılabilir            |
| [Azure kaynakları Için yönetilen kimlik](container-registry-authentication-managed-identity.md)  | `docker login`<br/><br/>Azure CLı 'de  `az acr login`                                        | Azure CI/CD ardışık düzeninde katılımsız gönderim<br/><br/> Azure hizmetlerine katılımsız çekme<br/><br/>   | Yes                              | Yalnızca [Azure kaynakları için yönetilen kimlikleri destekleyen](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md#azure-services-that-support-managed-identities-for-azure-resources) Azure hizmetlerinden kullanın              |
| [Yönetici kullanıcı](#admin-account)                            | `docker login`                                          | Bireysel geliştirici veya test edici tarafından etkileşimli gönderim/çekme                           | Hayır, her zaman çek ve anında erişim  | Kayıt defteri başına tek hesap, birden çok kullanıcı için önerilmez         |
| [Depo kapsamlı erişim belirteci](container-registry-repository-scoped-permissions.md)               | `docker login`<br/><br/>Azure CLı 'de `az acr login`   | Bireysel geliştirici veya test edici tarafından depoya etkileşimli gönderim/çekme<br/><br/> Ayrı sistem veya dış cihaz tarafından yapılan katılımsız gönderme/depoya çekme                  | Yes                              | Şu anda AD kimliğiyle tümleştirildi  |

## <a name="individual-login-with-azure-ad"></a>Azure AD ile bireysel oturum açma

Kayıt defteriyle doğrudan bir geliştirme iş istasyonundan görüntü çekme ve gönderme gibi, [Azure CLI](/cli/azure/install-azure-cli)'de [az ACR Login](/cli/azure/acr?view=azure-cli-latest#az-acr-login) komutunu kullanarak kimlik doğrulaması yapma.

```azurecli
az acr login --name <acrName>
```

`az acr login`ile oturum açtığınızda CLı, kayıt defterinizle oturumunuzu sorunsuz bir şekilde doğrulamak için [az Login](/cli/azure/reference-index#az-login) çalıştırdığınızda oluşturulan belirteci kullanır. Kimlik doğrulama akışını gerçekleştirmek için, Docker 'ın ortamınızda yüklü ve çalışır durumda olması gerekir. `az acr login`, `docker.config` dosyasında bir Azure Active Directory belirteci ayarlamak için Docker istemcisini kullanır. Bu şekilde oturum açtıktan sonra, kimlik bilgileriniz önbelleğe alınır ve oturuminizdeki sonraki `docker` komutları bir Kullanıcı adı veya parola gerektirmez.

> [!TIP]
> Ayrıca, Docker görüntüleri dışındaki yapıtları, Kayıt defterinize ( [OCI yapıları](container-registry-oci-artifacts.md)gibi) göndermek veya çekmek istediğinizde tek bir kimliğin kimliğini doğrulamak için `az acr login` kullanın.  


Kayıt defteri erişimi için `az acr login` tarafından kullanılan belirteç **3 saat**için geçerlidir, bu nedenle `docker` komutunu çalıştırmadan önce her zaman kayıt defterinde oturum açmanız önerilir. Belirtecin süresi dolarsa, yeniden kimlik doğrulaması yapmak için `az acr login` komutunu tekrar kullanarak yenileyebilirsiniz. 

Azure kimlikleri ile `az acr login` kullanmak, [rol tabanlı erişim](../role-based-access-control/role-assignments-portal.md)sağlar. Bazı senaryolarda, Azure AD 'de kendi bireysel kimliklerinizi kullanarak bir kayıt defterinde oturum açmak isteyebilirsiniz. Şirketler arası senaryolar için veya tek tek erişimi yönetmek istemediğiniz bir çalışma grubunun veya geliştirme iş akışının ihtiyaçlarını işlemek için [Azure kaynakları için yönetilen bir kimlikle](container-registry-authentication-managed-identity.md)da oturum açabilirsiniz.

## <a name="service-principal"></a>Hizmet sorumlusu

Kayıt defterinize bir [hizmet sorumlusu](../active-directory/develop/app-objects-and-service-principals.md) atarsanız, uygulamanız veya hizmetiniz bunu gözetimsiz kimlik doğrulaması için kullanabilir. Hizmet sorumluları, bir kayıt defterine [rol tabanlı erişime](../role-based-access-control/role-assignments-portal.md) izin verir ve bir kayıt defterine birden çok hizmet sorumlusu atayabilirsiniz. Birden çok hizmet sorumlusu, farklı uygulamalar için farklı erişim tanımlamanızı sağlar.

Bir kapsayıcı kayıt defteri için kullanılabilir roller şunlardır:

* **Acrpull**: çekme

* **Acrpush**: çekme ve gönderme

* **Sahip**: diğer kullanıcılara roller çekme, gönderme ve atama

Rollerin tüm listesi için bkz. [Azure Container Registry rolleri ve izinleri](container-registry-roles.md).

CLı betikleri için bir Azure Container Registry ile kimlik doğrulamaya yönelik bir hizmet sorumlusu oluşturma ve daha fazla rehberlik için bkz. [hizmet sorumluları ile Azure Container Registry kimlik doğrulaması](container-registry-auth-service-principal.md).

## <a name="admin-account"></a>Yönetici hesabı

Her kapsayıcı kayıt defteri, varsayılan olarak devre dışı bırakılan bir yönetici kullanıcı hesabı içerir. Yönetici kullanıcıyı etkinleştirebilir ve Azure portal kimlik bilgilerini yönetebilir veya Azure CLı veya diğer Azure araçlarını kullanarak yapılandırabilirsiniz.

> [!IMPORTANT]
> Yönetici hesabı, genellikle test amacıyla tek bir kullanıcının kayıt defterine erişmesi için tasarlanmıştır. Yönetici hesabı kimlik bilgilerini birden çok kullanıcı arasında paylaşmayı önermiyoruz. Yönetici hesabıyla kimlik doğrulaması yapan tüm kullanıcılar, kayıt defterine gönderme ve çekme erişimi olan tek bir kullanıcı olarak görüntülenir. Bu hesabın değiştirilmesi veya devre dışı bırakılması, kimlik bilgilerini kullanan tüm kullanıcılar için kayıt defteri erişimini devre dışı bırakır. Gözetimsiz senaryolara yönelik kullanıcılar ve hizmet sorumluları için bireysel kimlik önerilir.
>

Yönetici hesabı, her ikisi de yeniden üretilbilen iki parolayla sağlanır. İki parola, diğerini yeniden oluştururken bir parola kullanarak kayıt defteriyle bağlantıyı korumanıza olanak sağlar. Yönetici hesabı etkinleştirilmişse, kayıt defterine temel kimlik doğrulaması istendiğinde, Kullanıcı adını ve parolayı `docker login` komutuna geçirebilirsiniz. Örnek:

```
docker login myregistry.azurecr.io 
```

Oturum açma kimlik bilgilerini yönetmek için en iyi uygulamalar için [Docker Login](https://docs.docker.com/engine/reference/commandline/login/) komut başvurusuna bakın.

Mevcut bir kayıt defteri için yönetici kullanıcıyı etkinleştirmek üzere, Azure CLı 'de [az ACR Update](/cli/azure/acr?view=azure-cli-latest#az-acr-update) komutunun `--admin-enabled` parametresini kullanabilirsiniz:

```azurecli
az acr update -n <acrName> --admin-enabled true
```

Kayıt defterinizde gezinerek yönetici kullanıcıyı Azure portal etkinleştirebilirsiniz, **Ayarlar**altında **erişim anahtarları** ' nı seçip **Yönetici Kullanıcı**altında **etkinleştirin** .

![Azure portal Yönetici Kullanıcı Kullanıcı arabirimini etkinleştirme][auth-portal-01]

## <a name="next-steps"></a>Sonraki adımlar

* [Azure CLı kullanarak ilk görüntünüzü gönderin](container-registry-get-started-azure-cli.md)

<!-- IMAGES -->
[auth-portal-01]: ./media/container-registry-authentication/auth-portal-01.png
