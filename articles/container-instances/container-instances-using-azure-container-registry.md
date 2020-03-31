---
title: Azure Kapsayıcı Kayıt Defteri'nden kapsayıcı görüntüsünü dağıtma
description: Bir Azure kapsayıcı kayıt defterinden kapsayıcı görüntüleri çekerek kapsayıcıları Azure Kapsayıcı Örnekleri'nde nasıl dağıtılayın öğrenin.
services: container-instances
ms.topic: article
ms.date: 02/18/2020
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: 50c209483a12adc3545b63fb66685e386d9ad10a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78252134"
---
# <a name="deploy-to-azure-container-instances-from-azure-container-registry"></a>Azure Kapsayıcı Kayıt Defteri'nden Azure Kapsayıcı Örneklerine Dağıtma

[Azure Kapsayıcı Kayıt Defteri,](../container-registry/container-registry-intro.md) özel Docker kapsayıcı görüntülerini depolamak için kullanılan Azure tabanlı, yönetilen bir kapsayıcı kayıt defteri hizmetidir. Bu makalede, Azure Kapsayıcı Örnekleri'ne dağıtılırken Azure kapsayıcı kayıt defterinde depolanan kapsayıcı görüntülerinin nasıl çekileceği açıklanmaktadır. Kayıt defteri erişimini yapılandırmanın önerilen yolu, bir Azure Active Directory hizmet müdürü ve parola oluşturmak ve oturum açma kimlik bilgilerini bir Azure anahtar kasasında depolamaktır.

## <a name="prerequisites"></a>Ön koşullar

**Azure kapsayıcı kayıt defteri**: Bu makaledeki adımları tamamlamak için bir Azure kapsayıcı kayıt defterine ve kayıt defterinde en az bir kapsayıcı resmine ihtiyacınız var. Bir kayıt defterine ihtiyacınız varsa, [bkz.](../container-registry/container-registry-get-started-azure-cli.md)

**Azure CLI**: Bu makaledeki komut satırı örnekleri [Azure CLI'yi](/cli/azure/) kullanır ve Bash kabuğu için biçimlendirilir. Azure [CLI'yi](/cli/azure/install-azure-cli) yerel olarak yükleyebilir veya [Azure Bulut Kabuğu'nu][cloud-shell-bash]kullanabilirsiniz.

## <a name="configure-registry-authentication"></a>Kayıt defteri kimlik doğrulamasını yapılandırma

"Başsız" hizmetlere ve uygulamalara erişim sağladığınız bir üretim senaryosunda, bir [hizmet ilkesi](../container-registry/container-registry-auth-service-principal.md)kullanarak kayıt defteri erişimini yapılandırmanız önerilir. Servis ilkesi, kapsayıcı resimlerinize [rol tabanlı erişim denetimi](../container-registry/container-registry-roles.md) sağlamanıza olanak tanır. Örneğin, bir hizmet sorumlusunu bir kayıt defterine yalnızca çekme erişimiyle yapılandırabilirsiniz.

Azure Kapsayıcı Kayıt Defteri ek [kimlik doğrulama seçenekleri](../container-registry/container-registry-authentication.md)sağlar.

> [!NOTE]
> Aynı kapsayıcı grubunda yapılandırılan yönetilen bir [kimlik](container-instances-managed-identity.md) kullanarak kapsayıcı grubu dağıtımı sırasında görüntüleri çekmek için Azure Kapsayıcı Kayıt Defteri'ne kimlik doğrulaması yapamazsınız.

Aşağıdaki bölümde, bir Azure anahtar kasası ve bir hizmet sorumlusu oluşturur sunuz ve servis sorumlusunun kimlik bilgilerini kasada saklarsınız. 

### <a name="create-key-vault"></a>Anahtar kasası oluşturma

[Azure Key Vault](../key-vault/key-vault-overview.md) içinde henüz bir kasanız yoksa, aşağıdaki komutları kullanarak Azure CLI ile bir kasa oluşturun.

Değişkeni, `RES_GROUP` anahtar kasasını oluşturmak için varolan bir kaynak `ACR_NAME` grubunun adı ve kapsayıcı kayıt defterinizin adı ile güncelleştirin. Kısaltma için, bu makaledeki komutlar kayıt defterinizin, anahtar kasanızın ve kapsayıcı örneklerinin hepsinin aynı kaynak grubunda oluşturulduğunu varsayar.

 Yeni anahtar kasanızın adını `AKV_NAME`belirtin. Kasa adı Azure içinde benzersiz olmalıdır ve uzunluğu 3-24 alfanümerik karakter olmalıdır, bir harfle başlamalı, harf veya rakamla bitmelidir ve ardışık tireler içeremez.

```azurecli
RES_GROUP=myresourcegroup # Resource Group name
ACR_NAME=myregistry       # Azure Container Registry registry name
AKV_NAME=mykeyvault       # Azure Key Vault vault name

az keyvault create -g $RES_GROUP -n $AKV_NAME
```

### <a name="create-service-principal-and-store-credentials"></a>Hizmet sorumlusu oluşturma ve kimlik bilgilerini depolama

Şimdi bir hizmet müdürü oluşturun ve kimlik bilgilerini anahtar kasanızda saklayın.

Aşağıdaki komut, hizmet ilkesini oluşturmak [için az reklam sp create-for-rbac'ı][az-ad-sp-create-for-rbac] ve servis sorumlusunun **parolasını** kasada depolamak için [az keyvault gizli kümesini][az-keyvault-secret-set] kullanır.

```azurecli
# Create service principal, store its password in vault (the registry *password*)
az keyvault secret set \
  --vault-name $AKV_NAME \
  --name $ACR_NAME-pull-pwd \
  --value $(az ad sp create-for-rbac \
                --name http://$ACR_NAME-pull \
                --scopes $(az acr show --name $ACR_NAME --query id --output tsv) \
                --role acrpull \
                --query password \
                --output tsv)
```

Önceki `--role` komuttaki bağımsız değişken, hizmet ilkesini *acrpull* rolüyle yapılandırır ve bu da kayıt defterine yalnızca çekme erişimi verir. Hem itme hem de çekme `--role` erişimi sağlamak için, bağımsız değişkeni kısaltma olarak *değiştirin.*

Ardından, hizmet sorumlusunun *appId'ini,* kimlik doğrulaması için Azure Konteyner Kayıt Defteri'ne geçtiğiniz **kullanıcı adı** olan kasada saklayın.

```azurecli
# Store service principal ID in vault (the registry *username*)
az keyvault secret set \
    --vault-name $AKV_NAME \
    --name $ACR_NAME-pull-usr \
    --value $(az ad sp show --id http://$ACR_NAME-pull --query appId --output tsv)
```

Bir Azure anahtar kasası oluşturdunuz ve içinde iki gizli dizi depoladınız:

* `$ACR_NAME-pull-usr`: Kapsayıcı kayıt defterinin **kullanıcı adı** olarak kullanılacak hizmet sorumlusu kimliği.
* `$ACR_NAME-pull-pwd`: Kapsayıcı kayıt defterinin **parolası** olarak kullanılacak hizmet sorumlusu parolası.

Artık siz veya uygulamalarınız ve hizmetleriniz kayıt defterinden görüntüleri çektiğinde bu gizli dizilere ada göre başvurabilirsiniz.

## <a name="deploy-container-with-azure-cli"></a>Azure CLI ile kapsayıcı dağıtma

Artık hizmet temel kimlik bilgileri Azure Key Vault sırlarında depolandıklarından, uygulamalarınız ve hizmetleriniz bunları özel kayıt defterinize erişmek için kullanabilir.

Önce [az acr show][az-acr-show] komutunu kullanarak kayıt defterinin giriş sunucusu adını alın. Giriş sunucusu adı tüm küçük ve `myregistry.azurecr.io`benzer .

```azurecli
ACR_LOGIN_SERVER=$(az acr show --name $ACR_NAME --resource-group $RES_GROUP --query "loginServer" --output tsv)
```

Bir kapsayıcı örneği dağıtmak için aşağıdaki [az container create][az-container-create] komutunu yürütün. Komut, konteyner kayıt defterinizde kimlik doğrulaması yapmak için Hizmet sorumlusunun Azure Anahtar Kasası'nda depolanan kimlik bilgilerini kullanır ve [aci-helloworld](container-instances-quickstart.md) görüntüsünü daha önce kayıt defterinize ittiğinizi varsayar. Kayıt `--image` defterinizden farklı bir resim kullanmak istiyorsanız değeri güncelleştirin.

```azurecli
az container create \
    --name aci-demo \
    --resource-group $RES_GROUP \
    --image $ACR_LOGIN_SERVER/aci-helloworld:v1 \
    --registry-login-server $ACR_LOGIN_SERVER \
    --registry-username $(az keyvault secret show --vault-name $AKV_NAME -n $ACR_NAME-pull-usr --query value -o tsv) \
    --registry-password $(az keyvault secret show --vault-name $AKV_NAME -n $ACR_NAME-pull-pwd --query value -o tsv) \
    --dns-name-label aci-demo-$RANDOM \
    --query ipAddress.fqdn
```

Değer `--dns-name-label` Azure içinde benzersiz olmalıdır, bu nedenle önceki komut kapsayıcının DNS ad etiketine rasgele bir sayı ekler. Komutun çıktısı, kapsayıcının tam etki alanı adını (FQDN) gösterir, örneğin:

```output
"aci-demo-25007.eastus.azurecontainer.io"
```

Kapsayıcı başarıyla başladıktan sonra, uygulamanın başarılı bir şekilde çalıştığını doğrulamak için tarayıcınızdaki FQDN'sine gidebilirsiniz.

## <a name="deploy-with-azure-resource-manager-template"></a>Azure Kaynak Yöneticisi şablonuyla dağıtma

Azure kapsayıcı kayıt defterinizin özelliklerini, kapsayıcı grubu tanımına `imageRegistryCredentials` özelliği ekleyerek Azure Kaynak Yöneticisi şablonunda belirtebilirsiniz. Örneğin, kayıt defteri kimlik bilgilerini doğrudan belirtebilirsiniz:

```JSON
[...]
"imageRegistryCredentials": [
  {
    "server": "imageRegistryLoginServer",
    "username": "imageRegistryUsername",
    "password": "imageRegistryPassword"
  }
]
[...]
```

Tam kapsayıcı grubu ayarları için [Kaynak Yöneticisi şablon başvurusuna](/azure/templates/Microsoft.ContainerInstance/2018-10-01/containerGroups)bakın.    

Kaynak Yöneticisi şablonundaki Azure Anahtar Kasası sırlarına başvurma yla ilgili ayrıntılar için dağıtım [sırasında güvenli parametre değerini geçmek için Azure Anahtar Kasası'nı kullanın'a](../azure-resource-manager/templates/key-vault-parameter.md)bakın.

## <a name="deploy-with-azure-portal"></a>Azure portalı ile dağıtma

Bir Azure kapsayıcı kayıt defterinde kapsayıcı resimleri tutarsanız, Azure portalını kullanarak Azure Kapsayıcı Örnekleri'nde kolayca bir kapsayıcı oluşturabilirsiniz. Bir kapsayıcı kayıt defterinden bir kapsayıcı örneğini dağıtmak için portalı kullanırken, kayıt defterinin [yönetici hesabını](../container-registry/container-registry-authentication.md#admin-account)etkinleştirmeniz gerekir. Yönetici hesabı, tek bir kullanıcının kayıt defterine erişebilmek için tasarlanmıştır, özellikle sınama amacıyla. 

1. Azure portalında konteyner kayıt defterinize gidin.

1. Yönetici hesabının etkin olduğunu doğrulamak için **Access tuşlarını**seçin ve **Admin kullanıcı** altında **Etkinleştir'i**seçin.

1. **Depoları**seçin, ardından dağıtmak istediğiniz depoyu seçin, dağıtmak istediğiniz kapsayıcı resmi için etiketi sağ tıklatın ve **Örneği Çalıştır'ı**seçin.

    ![Azure portalında Azure Kapsayıcı Kayıt Defteri'nde "Çalıştırma örneği"][acr-runinstance-contextmenu]

1. Kapsayıcı için bir ad ve kaynak grubu için bir ad girin. İsterseniz varsayılan değerleri de değiştirebilirsiniz.

    ![Azure Kapsayıcı Örnekleri için menü oluşturma][acr-create-deeplink]

1. Dağıtım tamamlandıktan sonra, IP adresini ve diğer özelliklerini bulmak için bildirimler bölmesinden kapsayıcı grubuna gidebilirsiniz.

    ![Azure Kapsayıcı Örnekleri kapsayıcı grubu için ayrıntılar görünümü][aci-detailsview]

## <a name="next-steps"></a>Sonraki adımlar

Azure Kapsayıcı Kayıt Defteri kimlik doğrulaması hakkında daha fazla bilgi için, [Azure kapsayıcı kayıt defteriyle Kimlik Doğrulaması'ya](../container-registry/container-registry-authentication.md)bakın.

<!-- IMAGES -->
[acr-create-deeplink]: ./media/container-instances-using-azure-container-registry/acr-create-deeplink.png
[aci-detailsview]: ./media/container-instances-using-azure-container-registry/aci-detailsview.png
[acr-runinstance-contextmenu]: ./media/container-instances-using-azure-container-registry/acr-runinstance-contextmenu.png

<!-- LINKS - External -->
[cloud-shell-bash]: https://shell.azure.com/bash
[cloud-shell-try-it]: https://shell.azure.com/powershell

<!-- LINKS - Internal -->
[az-acr-show]: /cli/azure/acr#az-acr-show
[az-ad-sp-create-for-rbac]: /cli/azure/ad/sp#az-ad-sp-create-for-rbac
[az-container-create]: /cli/azure/container#az-container-create
[az-keyvault-secret-set]: /cli/azure/keyvault/secret#az-keyvault-secret-set
