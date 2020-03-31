---
title: Hizmet sorumlusuyla kimlik doğrulama
description: Azure Active Directory hizmet ilkesini kullanarak özel konteyner kayıt defterinizdeki resimlere erişim sağlayın.
ms.topic: article
ms.date: 10/04/2019
ms.openlocfilehash: 37da784c8e95a5f5b924532e4a019552924a1a3f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74455414"
---
# <a name="azure-container-registry-authentication-with-service-principals"></a>Hizmet ilkeleriyle Azure Kapsayıcı Kayıt Defteri kimlik doğrulaması

Kapsayıcı görüntüsü `docker push` ve `pull` konteyner kayıt defterinize erişim sağlamak için bir Azure Etkin Dizin (Azure AD) hizmet ilkesi kullanabilirsiniz. Bir hizmet ilkesi kullanarak, "başsız" hizmetlere ve uygulamalara erişim sağlayabilirsiniz.

## <a name="what-is-a-service-principal"></a>Hizmet sorumlusu nedir?

Azure AD *hizmet ilkeleri,* aboneliğiniz dahilinde Azure kaynaklarına erişim sağlar. Bir hizmet ilkesini, "hizmet"in kaynaklara erişmesi gereken herhangi bir uygulama, hizmet veya platform olduğu bir hizmetin kullanıcı kimliği olarak düşünebilirsiniz. Yalnızca belirttiğiniz kaynaklara göre kapsamlı erişim hakları içeren bir hizmet ilkesini yapılandırabilirsiniz. Ardından, uygulamanızı veya hizmetinizi bu kaynaklara erişmek için hizmet sorumlusunun kimlik bilgilerini kullanacak şekilde yapılandırın.

Azure Kapsayıcı Kayıt Defteri bağlamında, Azure'daki özel kayıt defterinize çekme, itme ve çekme veya diğer izinleri içeren bir Azure REKLAM hizmeti ilkesi oluşturabilirsiniz. Tam liste için [Azure Kapsayıcı Kayıt Defteri rolleri ve izinlerine](container-registry-roles.md)bakın.

## <a name="why-use-a-service-principal"></a>Neden bir hizmet müdürü kullanıyorsun?

Azure AD hizmet ilkesini kullanarak, özel konteyner kayıt defterinize kapsamlı erişim sağlayabilirsiniz. Her biri kayıt defterinize özel erişim haklarına sahip, her bir uygulama veya hizmet için farklı hizmet ilkeleri oluşturun. Ayrıca, hizmetler ve uygulamalar arasında kimlik bilgilerini paylaşmaktan kaçınabileceğinizden, kimlik bilgilerini döndürebilir veya yalnızca seçtiğiniz hizmet sorumlusu (ve dolayısıyla uygulama) için erişimi iptal edebilirsiniz.

Örneğin, web uygulamanızı yalnızca görüntü `pull` erişimi sağlayan bir hizmet ilkesi kullanacak şekilde yapılandırın, yapı sisteminiz `push` `pull` ise hem hem de erişim sağlayan bir hizmet ilkesi kullanır. Uygulamanızın gelişimi el değiştirirse, yapı sistemini etkilemeden hizmet temel kimlik bilgilerini döndürebilirsiniz.

## <a name="when-to-use-a-service-principal"></a>Hizmet sorumlusu ne zaman kullanılır?

**Başsız senaryolarda**kayıt defterine erişim sağlamak için bir hizmet sorumlusu kullanmanız gerekir. Diğer bir deyişle, kapsayıcı görüntülerini otomatik veya gözetimsiz bir şekilde itmesi veya çekmesi gereken herhangi bir uygulama, hizmet veya komut dosyası. Örnek:

  * *Çekme*: Konteynerleri kayıt defterinden Kubernetes, DC/OS ve Docker Swarm gibi orkestrasyon sistemlerine dağıtın. Ayrıca, konteyner kayıt kayıtlarından [Azure Kubernetes Hizmeti (AKS)](../aks/cluster-container-registry-integration.md), [Azure Konteyner Örnekleri,](container-registry-auth-aci.md) [Uygulama Hizmeti,](../app-service/index.yml) [Toplu İşlem](../batch/index.yml), [Hizmet Kumaşı](/azure/service-fabric/)ve diğerleri gibi ilgili Azure hizmetlerine de çekebilirsiniz.

  * *Push*: Azure Pipelines veya Jenkins gibi sürekli tümleştirme ve dağıtım çözümlerini kullanarak kapsayıcı görüntüleri oluşturun ve bunları bir kayıt defterine taşıyın.

Bir kayıt defterine tek tek erişim için (örneğin, geliştirme iş istasyonunuza bir kapsayıcı görüntüsünü el ile çektiğinizde), kayıt defteri erişimi yerine kendi [Azure AD kimliğinizi](container-registry-authentication.md#individual-login-with-azure-ad) kullanmanızı öneririz (örneğin, [az acr girişi).][az-acr-login]

[!INCLUDE [container-registry-service-principal](../../includes/container-registry-service-principal.md)]

### <a name="sample-scripts"></a>Örnek komut dosyaları

Azure CLI için önceki örnek komut dosyalarını GitHub'da ve Azure PowerShell sürümlerinde bulabilirsiniz:

* [Azure CLI][acr-scripts-cli]
* [Azure PowerShell][acr-scripts-psh]

## <a name="authenticate-with-the-service-principal"></a>Hizmet ilkesiyle kimlik doğrulaması

Konteyner kayıt defterinize erişim izni vermiş olduğunuz bir hizmet müdürünüz olduktan sonra, "başsız" hizmetlere ve uygulamalara erişmek `docker login` için kimlik bilgilerini yapılandırabilir veya komutu kullanarak girebilirsiniz. Aşağıdaki değerleri kullanın:

* **Kullanıcı adı** - hizmet ana başvuru kimliği *(istemci kimliği*olarak da adlandırılır)
* **Parola** - servis ana şifresi *(istemci sırrı*olarak da adlandırılır)

Her değer formun `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx`bir GUID'sidir. 

> [!TIP]
> [Az reklam sp sıfırlama kimlik bilgileri](/cli/azure/ad/sp/credential#az-ad-sp-credential-reset) komutunu çalıştırarak bir hizmet sorumlusunun parolasını yeniden oluşturabilirsiniz.
>

### <a name="use-credentials-with-azure-services"></a>Azure hizmetleriyle kimlik bilgilerini kullanma

Azure kapsayıcı kayıt defteriyle kimlik doğrulaması yapan herhangi bir Azure hizmetinin hizmet temel kimlik bilgilerini kullanabilirsiniz.  Çeşitli senaryolar için kayıt defterinin yönetici kimlik bilgileri yerine hizmet temel kimlik bilgilerini kullanın.

Örneğin, bir Azure kapsayıcı kayıt defterinden [Azure Kapsayıcı Örnekleri'ne](container-registry-auth-aci.md)görüntü çekmek için kimlik bilgilerini kullanın.

### <a name="use-with-docker-login"></a>Docker girişi ile kullanın

Bir hizmet `docker login` ilkesi ni kullanarak çalıştırabilirsiniz. Aşağıdaki örnekte, hizmet temel uygulama kimliği ortam `$SP_APP_ID`değişkenine ve parola `$SP_PASSWD`değişkeninde geçirilir. Docker kimlik bilgilerini yönetmek için en iyi uygulamalar için [docker giriş](https://docs.docker.com/engine/reference/commandline/login/) komutu başvurusuna bakın.

```bash
# Log in to Docker with service principal credentials
docker login myregistry.azurecr.io --username $SP_APP_ID --password $SP_PASSWD
```

Oturum açtıktan sonra Docker kimlik bilgilerini önbelleğe alıyor.

### <a name="use-with-certificate"></a>Sertifika ile kullanma

Hizmet müdürünüze bir sertifika eklediyseniz, sertifika tabanlı kimlik doğrulamayla Azure CLI'de oturum açabilir ve bir kayıt defterine erişmek için [az acr giriş][az-acr-login] komutunu kullanabilirsiniz. CLI'yi kullandığınızda sertifikayı parola yerine gizli kullanmak ek güvenlik sağlar. 

Bir [hizmet ilkesi oluşturduğunuzda](/cli/azure/create-an-azure-service-principal-azure-cli)kendi imzalı bir sertifika oluşturulabilir. Veya varolan bir hizmet ilkesine bir veya daha fazla sertifika ekleyin. Örneğin, bu makaledeki komut dosyalarından birini, bir kayıt defterinden görüntüleri çekme veya itme haklarına sahip bir hizmet sorumlusu oluşturmak veya güncelleştirmek için kullanıyorsanız, [az reklam sp kimlik bilgilerini sıfırlama][az-ad-sp-credential-reset] komutunu kullanarak bir sertifika ekleyin.

[Azure CLI'de oturum açma](/cli/azure/authenticate-azure-cli#sign-in-with-a-service-principal)sertifikası olan hizmet ilkesini kullanmak için sertifikanın PEM biçiminde olması ve özel anahtarı içermesi gerekir. Sertifikanız gerekli biçimde değilse, dönüştürmek `openssl` için bir araç kullanın. Hizmet ilkesini kullanarak CLI'de oturum açmak için [az oturum][az-login] açtığınızda, hizmet sorumlusunun başvuru kimliğini ve Active Directory kiracı kimliğini de sağlayın. Aşağıdaki örnekte bu değerler ortam değişkenleri olarak gösterilmektedir:

```azurecli
az login --service-principal --username $SP_APP_ID --tenant $SP_TENANT_ID  --password /path/to/cert/pem/file
```

Ardından, kayıt defteriyle kimlik doğrulaması yapmak için [az acr girişi][az-acr-login] çalıştırın:

```azurecli
az acr login --name myregistry
```

CLI, oturumunuzu kayıt defteriyle `az login` doğrulamak için koştunuzunuzda oluşturulan belirteci kullanır.

## <a name="next-steps"></a>Sonraki adımlar

* Azure kapsayıcı kayıt defteriyle kimlik doğrulaması yapmak için diğer senaryolar için [kimlik doğrulama genel görünümünü](container-registry-authentication.md) görün.

* Kapsayıcı kayıt defteri için hizmet temel kimlik bilgilerini depolamak ve almak için Azure anahtar kasası kullanma örneği için, [ACR Görevleri'ni kullanarak kapsayıcı görüntüsü oluşturmak ve dağıtmak](container-registry-tutorial-quick-task.md)için öğreticiye bakın.

<!-- LINKS - External -->
[acr-scripts-cli]: https://github.com/Azure/azure-docs-cli-python-samples/tree/master/container-registry
[acr-scripts-psh]: https://github.com/Azure/azure-docs-powershell-samples/tree/master/container-registry

<!-- LINKS - Internal -->
[az-acr-login]: /cli/azure/acr#az-acr-login
[az-login]: /cli/azure/reference-index#az-login
[az-ad-sp-credential-reset]: /cli/azure/ad/sp/credential#az-ad-sp-credential-reset
