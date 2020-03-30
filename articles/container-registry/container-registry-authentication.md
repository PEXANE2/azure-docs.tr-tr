---
title: Kayıt defteri kimlik doğrulama seçenekleri
description: Azure Etkin Dizin kimliğiyle oturum açma, hizmet ilkelerini kullanma ve isteğe bağlı yönetici kimlik bilgilerini kullanma dahil olmak üzere özel bir Azure kapsayıcı kayıt defteri için kimlik doğrulama seçenekleri.
ms.topic: article
ms.date: 01/30/2020
ms.openlocfilehash: 5459ac29c1264b18404cb2863b9d4209907ac029
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79247052"
---
# <a name="authenticate-with-an-azure-container-registry"></a>Azure kapsayıcı kayıt defteriyle kimlik doğrulaması

Her biri bir veya daha fazla kayıt defteri kullanım senaryosu için geçerli olan bir Azure kapsayıcı kayıt defteriyle kimlik doğrulaması yapmanın birkaç yolu vardır.

Önerilen yollar, doğrudan [tek tek giriş](#individual-login-with-azure-ad)yoluyla bir kayıt defterine kimlik doğrulaması yapmayı içerir veya uygulamalarınız ve konteyner orkestratörleri bir Azure Etkin Dizin (Azure AD) hizmet [yöneticisi](#service-principal)kullanarak katılımsız veya "başsız" kimlik doğrulama gerçekleştirebilir.

## <a name="authentication-options"></a>Kimlik doğrulaması seçenekleri

Aşağıdaki tabloda kullanılabilir kimlik doğrulama yöntemleri ve önerilen senaryolar listelenmektedir. Ayrıntılar için bağlantılı içeriğe bakın.

| Yöntem                               | Kimlik doğrulaması nasıl                                           | Senaryolar                                                            | RBAC                             | Sınırlamalar                                |
|---------------------------------------|-------------------------------------------------------|---------------------------------------------------------------------|----------------------------------|--------------------------------------------|
| [Bireysel AD kimliği](#individual-login-with-azure-ad)                | `az acr login` azure cli içinde                             | Geliştiriciler, test edenler tarafından etkileşimli itme/çekme                                    | Evet                              | AD belirteci her 3 saatte bir yenilenmelidir     |
| [AD hizmet müdürü](#service-principal)                  | `docker login`<br/><br/>`az acr login`azure cli içinde<br/><br/> API'lerde veya araçlamada kayıt defteri giriş ayarları<br/><br/> [Kubernetes gizli çekin](container-registry-auth-kubernetes.md)                                           | CI/CD boru hattından gözetimsiz itme<br/><br/> Azure veya harici hizmetlere katılımsız çekme  | Evet                              | SP şifre varsayılan son kullanma tarihi 1 yıldır       |                                                           
| [AKS ile tümleştir](../aks/cluster-container-registry-integration.md?toc=/azure/container-registry/toc.json&bc=/azure/container-registry/breadcrumb/toc.json)                    | AKS kümesi oluşturulduğunda veya güncelleştirildiğinde kayıt defterini ekleme  | AKS kümesine katılımsız çekme                                                  | Hayır, yalnızca erişimi çekin             | Yalnızca AKS kümesi ile kullanılabilir            |
| [Azure kaynakları için yönetilen kimlik](container-registry-authentication-managed-identity.md)  | `docker login`<br/><br/> `az acr login` azure cli içinde                                       | Azure CI/CD ardışık kaynaktan gözetimsiz itme<br/><br/> Azure hizmetlerine katılımsız çekme<br/><br/>   | Evet                              | Yalnızca Azure kaynakları [için yönetilen kimlikleri destekleyen](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md#azure-services-that-support-managed-identities-for-azure-resources) Azure hizmetlerinden kullanın              |
| [Yönetici kullanıcısı](#admin-account)                            | `docker login`                                          | Bireysel geliştirici veya test örücü tarafından etkileşimli itme/çekme                           | Hayır, her zaman erişimi çekin ve itin  | Kayıt defteri başına tek hesap, birden çok kullanıcı için önerilmez         |
| [Depo kapsamı namına erişim belirteci](container-registry-repository-scoped-permissions.md)               | `docker login`<br/><br/>`az acr login`azure cli içinde   | Tek tek geliştirici veya test edici tarafından depoya etkileşimli itme/çekme<br/><br/> Tek tek sistem veya harici cihaz tarafından depoya gözetimsiz itme/çekme                  | Evet                              | Şu anda AD kimliğiyle entegre değil  |

## <a name="individual-login-with-azure-ad"></a>Azure AD ile bireysel oturum açma

Görüntüleri bir geliştirme iş istasyonuna çekme ve itme gibi doğrudan kayıt defterinizle çalışırken, [Azure CLI'deki](/cli/azure/install-azure-cli) [az acr giriş](/cli/azure/acr?view=azure-cli-latest#az-acr-login) komutunu kullanarak kimlik doğrulaması yapın:

```azurecli
az acr login --name <acrName>
```

Oturum açtığınızda `az acr login`CLI, az [girişi](/cli/azure/reference-index#az-login) yaptığınızda oluşturulan belirteci kullanır ve oturumunuzu kayıt defterinizle sorunsuz bir şekilde doğrular. Kimlik doğrulama akışını tamamlamak için Docker'ın ortamınızda yüklü ve çalışıyor olması gerekir. `az acr login`dosyada bir Azure Etkin Dizin belirteci `docker.config` ayarlamak için Docker istemcisini kullanır. Bu şekilde oturum açtıktan sonra kimlik bilgileriniz önbelleğe `docker` alınır ve oturumunuzdaki sonraki komutlar kullanıcı adı veya parola gerektirmez.

> [!TIP]
> Docker `az acr login` görüntüleri dışındaki yapıları kayıt defterinize itmek veya çekmek istediğinizde, tek bir kimliği doğrulamak için de kullanın( [OCI yapıları](container-registry-oci-artifacts.md)gibi).  


Kayıt defteri erişimi için kullanılan `az acr login` belirteç **3 saat**boyunca geçerlidir, bu nedenle bir `docker` komutu çalıştırmadan önce her zaman kayıt defterine giriş yapmanızı öneririz. Belirteçlerinizin süresi doluyorsa, `az acr login` yeniden kimlik doğrulaması için komutu kullanarak yeniden yenileyebilirsiniz. 

Azure `az acr login` kimlikleriyle [kullanmak rol tabanlı erişim](../role-based-access-control/role-assignments-portal.md)sağlar. Bazı senaryolarda, Azure AD'de kendi bireysel kimliğinizle bir kayıt defterinde oturum açmak isteyebilirsiniz. Çapraz hizmet senaryoları veya tek tek erişimi yönetmek istemediğiniz bir çalışma grubunun veya geliştirme iş akışının gereksinimlerini işlemek [için, Azure kaynakları için yönetilen](container-registry-authentication-managed-identity.md)bir kimlikle de oturum açabilirsiniz.

## <a name="service-principal"></a>Hizmet sorumlusu

Kayıt defterinize bir [hizmet müdürü](../active-directory/develop/app-objects-and-service-principals.md) atarsanız, uygulamanız veya hizmetiniz bunu başsız kimlik doğrulaması için kullanabilir. Hizmet ilkeleri, bir kayıt defterine [rol tabanlı erişime](../role-based-access-control/role-assignments-portal.md) izin verir ve bir kayıt defterine birden çok hizmet ilkesi atayabilirsiniz. Birden çok hizmet ilkesi, farklı uygulamalar için farklı erişim tanımlamanıza olanak sağlar.

Kapsayıcı kayıt defteri için kullanılabilir roller şunlardır:

* **AcrPull**: çekme

* **AcrPush**: çekme ve itme

* **Sahibi**: diğer kullanıcılara çekme, itme ve atama rolleri

Rollerin tam listesi için [Azure Kapsayıcı Kayıt Defteri rolleri ve izinlerine](container-registry-roles.md)bakın.

CLI komut dosyalarının azure kapsayıcı kayıt defteriyle kimlik doğrulaması için bir hizmet ilkesi ve daha fazla kılavuz oluşturması [için, hizmet ilkeleriyle birlikte Azure Kapsayıcı Kayıt Defteri kimlik doğrulaması'na](container-registry-auth-service-principal.md)bakın.

## <a name="admin-account"></a>Yönetici hesabı

Her kapsayıcı kayıt defteri, varsayılan olarak devre dışı bırakılan bir yönetici kullanıcı hesabı içerir. Yönetici kullanıcıyı Etkinleştirebilir ve azure portalında veya Azure CLI veya diğer Azure araçlarını kullanarak kimlik bilgilerini yönetebilirsiniz.

> [!IMPORTANT]
> Yönetici hesabı, tek bir kullanıcının kayıt defterine erişebilmek için tasarlanmıştır, özellikle sınama amacıyla. Yönetici hesabı kimlik bilgilerini birden çok kullanıcı arasında paylaşmanızı önermiyoruz. Yönetici hesabıyla kimlik doğrulaması yapılan tüm kullanıcılar, kayıt defterine itme ve çekme erişimi olan tek bir kullanıcı olarak görünür. Bu hesabın değiştirilmesi veya devre dışı bırakılması, kimlik bilgilerini kullanan tüm kullanıcılar için kayıt defteri erişimini devre dışı bırakma. Başsız senaryolar için kullanıcılar ve hizmet ilkeleri için bireysel kimlik önerilir.
>

Yönetici hesabı, her ikisi de yeniden oluşturulabilen iki parolayla sağlanır. İki parola, diğerini yeniden oluştururken bir parola kullanarak kayıt defterine bağlantıyı korumanızı sağlar. Yönetici hesabı etkinse, temel kimlik doğrulaması istendiğinde `docker login` kullanıcı adını ve parolayı komuta geçirebilirsiniz. Örnek:

```
docker login myregistry.azurecr.io 
```

Giriş kimlik bilgilerini yönetmek için en iyi uygulamalar için [docker giriş](https://docs.docker.com/engine/reference/commandline/login/) komutu başvurusuna bakın.

Varolan bir kayıt defteri için yönetici kullanıcıyı `--admin-enabled` etkinleştirmek için Azure CLI'deki [az acr güncelleştirme](/cli/azure/acr?view=azure-cli-latest#az-acr-update) komutunun parametresini kullanabilirsiniz:

```azurecli
az acr update -n <acrName> --admin-enabled true
```

Kayıt defterinizde gezinerek, **AYARLAR**altında **Access tuşlarını** seçerek Azure portalında yönetici kullanıcıyı etkinleştirebilirsiniz, ardından **Yönetici kullanıcısı**altında **etkinleştirin.**

![Azure portalında yönetici kullanıcı arabirimi sağlama][auth-portal-01]

## <a name="next-steps"></a>Sonraki adımlar

* [Azure CLI'yi kullanarak ilk resminizi itin](container-registry-get-started-azure-cli.md)

<!-- IMAGES -->
[auth-portal-01]: ./media/container-registry-authentication/auth-portal-01.png
