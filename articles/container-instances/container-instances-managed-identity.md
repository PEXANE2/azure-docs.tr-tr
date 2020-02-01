---
title: Kapsayıcı grubunda yönetilen kimliği etkinleştir
description: Diğer Azure hizmetleriyle kimlik doğrulayabilecek Azure Container Instances yönetilen bir kimliği nasıl etkinleştirebileceğinizi öğrenin
ms.topic: article
ms.date: 01/29/2020
ms.openlocfilehash: 003055d5021dd8ad7c3bab6d2900298ffd13b222
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2020
ms.locfileid: "76901939"
---
# <a name="how-to-use-managed-identities-with-azure-container-instances"></a>Azure Container Instances ile yönetilen kimlikler kullanma

Kod içinde herhangi bir gizli dizi veya kimlik bilgisi olmadan diğer Azure hizmetleriyle etkileşim kuran Azure Container Instances kod çalıştırmak için [Azure kaynakları için Yönetilen kimlikler](../active-directory/managed-identities-azure-resources/overview.md) kullanın. Özelliği, Azure Active Directory otomatik olarak yönetilen bir kimlikle Azure Container Instances dağıtımı sağlar.

Bu makalede, Azure Container Instances ve içindeki yönetilen kimlikler hakkında daha fazla bilgi edinebilirsiniz:

> [!div class="checklist"]
> * Kapsayıcı grubunda Kullanıcı tarafından atanan veya sistem tarafından atanan bir kimliği etkinleştirme
> * Azure Anahtar Kasası 'na kimlik erişimi verme
> * Çalışan bir kapsayıcıdan anahtar kasasına erişmek için yönetilen kimliği kullanma

Diğer Azure hizmetlerine erişmek için Azure Container Instances kimlikleri etkinleştirmek ve kullanmak üzere örnekleri uyarlayın. Bu örnekler etkileşimlidir. Ancak, kapsayıcıda kapsayıcı görüntüleriniz Azure hizmetlerine erişmek için kodu çalıştırır.

> [!NOTE]
> Şu anda bir sanal ağa dağıtılan bir kapsayıcı grubunda yönetilen bir kimlik kullanamazsınız.

## <a name="why-use-a-managed-identity"></a>Yönetilen kimlik neden kullanılmalıdır?

Kapsayıcı kodunuzda kimlik bilgilerini yönetmeksizin [Azure AD kimlik doğrulamasını destekleyen](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md#azure-services-that-support-azure-ad-authentication) herhangi bir hizmette kimlik doğrulaması yapmak için çalışan bir kapsayıcıda yönetilen bir kimlik kullanın. AD kimlik doğrulamasını desteklemeyen hizmetler için, gizli dizileri bir Azure anahtar kasasında saklayabilir ve kimlik bilgilerini almak için anahtar kasasına erişmek üzere yönetilen kimliği kullanabilirsiniz. Yönetilen kimlik kullanma hakkında daha fazla bilgi için bkz. [Azure kaynakları için Yönetilen kimlikler nelerdir?](../active-directory/managed-identities-azure-resources/overview.md)

> [!IMPORTANT]
> Bu özellik şu anda önizleme sürümündedir. Önizlemeler, [ek kullanım koşullarını](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) kabul etmeniz şartıyla kullanımınıza sunulur. Bu özelliğin bazı yönleri genel kullanıma açılmadan önce değişebilir. Şu anda Azure Container Instances Yönetilen kimlikler yalnızca Linux kapsayıcılarıyla desteklenir ve henüz Windows kapsayıcılarıyla desteklenmez.
>  

### <a name="enable-a-managed-identity"></a>Yönetilen bir kimliği etkinleştirme

 Azure Container Instances, Azure kaynakları için Yönetilen kimlikler, REST API sürüm 2018-10-01 ve karşılık gelen SDK 'lar ve araçlar olarak desteklenmektedir. Bir kapsayıcı grubu oluşturduğunuzda bir [Containergroupıdentity](/rest/api/container-instances/containergroups/createorupdate#containergroupidentity) özelliği ayarlayarak bir veya daha fazla yönetilen kimliği etkinleştirin. Ayrıca, bir kapsayıcı grubu çalıştıktan sonra yönetilen kimlikleri etkinleştirebilir veya güncelleştirebilirsiniz-her iki işlem de kapsayıcı grubunun yeniden başlatılmasına neden olur. Yeni veya mevcut bir kapsayıcı grubundaki kimlikleri ayarlamak için Azure CLı, Kaynak Yöneticisi şablonu veya bir YAML dosyası kullanın. 

Azure Container Instances hem yönetilen Azure kimlik türlerini destekler: Kullanıcı tarafından atanan ve sistem tarafından atanan. Bir kapsayıcı grubunda, sistem tarafından atanan bir kimliği, bir veya daha fazla kullanıcı tarafından atanan kimliği veya her iki tür kimliği etkinleştirebilirsiniz. 

* **Kullanıcı tarafından atanan** yönetilen kimlik, Azure AD kiracısında kullanımda olan aboneliğin güvendiği tek başına bir Azure kaynağı olarak oluşturulur. Kimlik oluşturulduktan sonra, kimlik bir veya daha fazla Azure kaynağına atanabilir (Azure Container Instances veya diğer Azure hizmetlerinde). Kullanıcı tarafından atanan kimliğin yaşam döngüsü, kendisine atanan kapsayıcı gruplarının veya diğer hizmet kaynaklarının yaşam döngülerinden ayrı olarak yönetilir. Bu davranış, Azure Container Instances özellikle yararlıdır. Kimlik bir kapsayıcı grubunun ömrünü aşacak şekilde uzanıyor olduğundan, kapsayıcı grubu dağıtımlarınızın yüksek oranda yinelenebilir olmasını sağlamak için diğer standart ayarlarla birlikte yeniden kullanabilirsiniz.

* **Sistem tarafından atanan** yönetilen kimlik Azure Container Instances doğrudan bir kapsayıcı grubunda etkinleştirilir. Azure, etkinleştirildiğinde Azure AD kiracısında örneğin aboneliği tarafından güvenilen grup için bir kimlik oluşturur. Kimlik oluşturulduktan sonra, kapsayıcı grubundaki her kapsayıcıda kimlik bilgileri sağlanır. Sistem tarafından atanan kimliğin yaşam döngüsü, etkinleştirilmiş olduğu kapsayıcı grubuna doğrudan bağlıdır. Grup silindiğinde Azure, kimlik bilgilerini ve kimlik bilgilerini Azure AD 'de otomatik olarak temizler.

### <a name="use-a-managed-identity"></a>Yönetilen kimlik kullanma

Yönetilen bir kimlik kullanmak için, kimliğin başlangıçta abonelikte bir veya daha fazla Azure hizmet kaynağına (bir Web uygulaması, bir Anahtar Kasası veya bir depolama hesabı gibi) erişim verilmesi gerekir. Azure kaynaklarına çalışan bir kapsayıcıdan erişmek için, kodunuzun bir Azure AD uç noktasından *erişim belirteci* edinmesi gerekir. Daha sonra kodunuz, erişim belirtecini Azure AD kimlik doğrulamasını destekleyen bir hizmete çağrı üzerine gönderir. 

Çalışan bir kapsayıcıda yönetilen bir kimlik kullanılması, aslında bir Azure VM 'de kimlik kullanmayla aynıdır. [Belirteç](../active-directory/managed-identities-azure-resources/how-to-use-vm-token.md), [Azure POWERSHELL veya Azure CLI](../active-directory/managed-identities-azure-resources/how-to-use-vm-sign-in.md)veya [Azure SDK](../active-directory/managed-identities-azure-resources/how-to-use-vm-sdk.md)'larını kullanma için VM yönergelerine bakın.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLı 'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu makale, Azure CLı sürüm 2.0.49 veya üstünü çalıştırıyor olmanızı gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme](/cli/azure/install-azure-cli).

## <a name="create-an-azure-key-vault"></a>Azure Anahtar Kasası oluşturma

Bu makaledeki örneklerde Azure Anahtar Kasası gizliliğine erişmek için Azure Container Instances yönetilen bir kimlik kullanılır. 

İlk olarak, *eastus* konumunda *myresourcegroup* adlı bir kaynak grubu aşağıdaki [az Group Create](/cli/azure/group?view=azure-cli-latest#az-group-create) komutu ile oluşturun:

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Bir Anahtar Kasası oluşturmak için [az keykasacreate](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-create) komutunu kullanın. Benzersiz bir Anahtar Kasası adı belirttiğinizden emin olun. 

```azurecli-interactive
az keyvault create \
  --name mykeyvault \
  --resource-group myResourceGroup \ 
  --location eastus
```

[Az keykasasecret set](/cli/azure/keyvault/secret?view=azure-cli-latest#az-keyvault-secret-set) komutunu kullanarak anahtar kasasında örnek bir gizli dizi depolayın:

```azurecli-interactive
az keyvault secret set \
  --name SampleSecret \
  --value "Hello Container Instances" \
  --description ACIsecret --vault-name mykeyvault
```

Azure Container Instances içinde Kullanıcı tarafından atanan veya sistem tarafından atanan bir yönetilen kimlik kullanarak anahtar kasasına erişmek için aşağıdaki örneklerle devam edin.

## <a name="example-1-use-a-user-assigned-identity-to-access-azure-key-vault"></a>Örnek 1: Azure anahtar kasasına erişmek için Kullanıcı tarafından atanan bir kimlik kullanma

### <a name="create-an-identity"></a>Kimlik oluşturma

İlk olarak [az Identity Create](/cli/azure/identity?view=azure-cli-latest#az-identity-create) komutunu kullanarak aboneliğinizde bir kimlik oluşturun. Anahtar kasasını oluşturmak için kullanılan kaynak grubunu kullanabilir veya farklı bir tane kullanabilirsiniz.

```azurecli-interactive
az identity create \
  --resource-group myResourceGroup \
  --name myACIId
```

Aşağıdaki adımlarda kimliği kullanmak için, kimliğin hizmet sorumlusu KIMLIĞINI ve kaynak KIMLIĞINI değişkenlerde depolamak üzere [az Identity Show](/cli/azure/identity?view=azure-cli-latest#az-identity-show) komutunu kullanın.

```azurecli-interactive
# Get service principal ID of the user-assigned identity
spID=$(az identity show --resource-group myResourceGroup --name myACIId --query principalId --output tsv)

# Get resource ID of the user-assigned identity
resourceID=$(az identity show --resource-group myResourceGroup --name myACIId --query id --output tsv)
```

### <a name="enable-a-user-assigned-identity-on-a-container-group"></a>Kapsayıcı grubunda Kullanıcı tarafından atanan kimliği etkinleştirme

Microsoft 'un `azure-cli` görüntüsünü temel alan bir kapsayıcı örneği oluşturmak için aşağıdaki [az Container Create](/cli/azure/container?view=azure-cli-latest#az-container-create) komutunu çalıştırın. Bu örnek, diğer Azure hizmetlerine erişmek için Azure CLı 'yı çalıştırmak üzere etkileşimli olarak kullanabileceğiniz tek bir kapsayıcı grubu sağlar. Bu bölümde, yalnızca temel Ubuntu işletim sistemi kullanılır. 

`--assign-identity` parametresi, Kullanıcı tarafından atanan yönetilen kimliğinizi gruba geçirir. Uzun süre çalışan komut kapsayıcıyı çalışır durumda tutar. Bu örnek, anahtar kasasını oluşturmak için kullanılan kaynak grubunu kullanır, ancak farklı bir tane belirtebilirsiniz.

```azurecli-interactive
az container create \
  --resource-group myResourceGroup \
  --name mycontainer \
  --image mcr.microsoft.com/azure-cli \
  --assign-identity $resourceID \
  --command-line "tail -f /dev/null"
```

Birkaç saniye içinde Azure CLI'den dağıtımın tamamlandığını belirten bir yanıt almanız gerekir. [Az Container Show](/cli/azure/container?view=azure-cli-latest#az-container-show) komutuyla durumunu kontrol edin.

```azurecli-interactive
az container show \
  --resource-group myResourceGroup \
  --name mycontainer
```

Çıktının `identity` bölümü aşağıdakine benzer şekilde görünür ve bu, kimliğin kapsayıcı grubunda ayarlandığını gösterir. `userAssignedIdentities` altındaki `principalID`, Azure Active Directory oluşturduğunuz kimliğin hizmet sorumlusu olur:

```console
[...]
"identity": {
    "principalId": "null",
    "tenantId": "xxxxxxxx-f292-4e60-9122-xxxxxxxxxxxx",
    "type": "UserAssigned",
    "userAssignedIdentities": {
      "/subscriptions/xxxxxxxx-0903-4b79-a55a-xxxxxxxxxxxx/resourcegroups/danlep1018/providers/Microsoft.ManagedIdentity/userAssignedIdentities/myACIId": {
        "clientId": "xxxxxxxx-5523-45fc-9f49-xxxxxxxxxxxx",
        "principalId": "xxxxxxxx-f25b-4895-b828-xxxxxxxxxxxx"
      }
    }
  },
[...]
```

### <a name="grant-user-assigned-identity-access-to-the-key-vault"></a>Anahtar kasasına Kullanıcı tarafından atanan kimlik erişimi verme

Anahtar kasasında bir erişim ilkesi ayarlamak için aşağıdaki [az keykasası Set-Policy](/cli/azure/keyvault?view=azure-cli-latest) komutunu çalıştırın. Aşağıdaki örnek, Kullanıcı tarafından atanan kimliğin anahtar kasasından gizli dizileri almasına izin verir:

```azurecli-interactive
 az keyvault set-policy \
    --name mykeyvault \
    --resource-group myResourceGroup \
    --object-id $spID \
    --secret-permissions get
```

### <a name="use-user-assigned-identity-to-get-secret-from-key-vault"></a>Anahtar kasasından gizli dizi almak için Kullanıcı tarafından atanan kimlik kullan

Artık anahtar kasasına erişmek için çalışan kapsayıcı örneği içinde yönetilen kimliği kullanabilirsiniz. Önce kapsayıcıda bir bash kabuğu başlatın:

```azurecli-interactive
az container exec \
  --resource-group myResourceGroup \
  --name mycontainer \
  --exec-command "/bin/bash"
```

Kapsayıcıda bash kabuğu 'nda aşağıdaki komutları çalıştırın. Anahtar kasasında kimlik doğrulaması yapmak için Azure Active Directory kullanmak üzere bir erişim belirteci almak için aşağıdaki komutu çalıştırın:

```bash
curl 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fvault.azure.net' -H Metadata:true -s
```

Çıktı:

```bash
{"access_token":"xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Imk2bEdrM0ZaenhSY1ViMkMzbkVRN3N5SEpsWSIsImtpZCI6Imk2bEdrM0ZaenhSY1ViMkMzbkVRN3N5SEpsWSJ9......xxxxxxxxxxxxxxxxx","refresh_token":"","expires_in":"28799","expires_on":"1539927532","not_before":"1539898432","resource":"https://vault.azure.net/","token_type":"Bearer"}
```

Erişim belirtecini, kimlik doğrulaması için sonraki komutlarda kullanılacak bir değişkende depolamak için aşağıdaki komutu çalıştırın:

```bash
token=$(curl 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fvault.azure.net' -H Metadata:true | jq -r '.access_token')

```

Artık anahtar kasasında kimlik doğrulamak ve gizli dizi okumak için erişim belirtecini kullanın. Anahtar kasanızın adını URL 'de ( *https://mykeyvault.vault.azure.net/..* ) değiştirdiğinizden emin olun:

```bash
curl https://mykeyvault.vault.azure.net/secrets/SampleSecret/?api-version=2016-10-01 -H "Authorization: Bearer $token"
```

Yanıt, gizliliği gösteren aşağıdakine benzer şekilde görünür. Kodunuzda, parolayı elde etmek için bu çıktıyı ayrıştırırdınız. Daha sonra, başka bir Azure kaynağına erişmek için sonraki bir işlemde gizli dizi kullanın.

```bash
{"value":"Hello Container Instances","contentType":"ACIsecret","id":"https://mykeyvault.vault.azure.net/secrets/SampleSecret/xxxxxxxxxxxxxxxxxxxx","attributes":{"enabled":true,"created":1539965967,"updated":1539965967,"recoveryLevel":"Purgeable"},"tags":{"file-encoding":"utf-8"}}
```

## <a name="example-2-use-a-system-assigned-identity-to-access-azure-key-vault"></a>Örnek 2: Azure Anahtar Kasası 'na erişmek için sistem tarafından atanan bir kimlik kullanma

### <a name="enable-a-system-assigned-identity-on-a-container-group"></a>Kapsayıcı grubunda sistem tarafından atanan kimliği etkinleştirme

Microsoft 'un `azure-cli` görüntüsünü temel alan bir kapsayıcı örneği oluşturmak için aşağıdaki [az Container Create](/cli/azure/container?view=azure-cli-latest#az-container-create) komutunu çalıştırın. Bu örnek, diğer Azure hizmetlerine erişmek için Azure CLı 'yı çalıştırmak üzere etkileşimli olarak kullanabileceğiniz tek bir kapsayıcı grubu sağlar. 

Ek değer içermeyen `--assign-identity` parametresi, Grup üzerinde sistem tarafından atanan yönetilen kimliği mümkün bir şekilde sunar. Kimlik, kapsayıcı grubunun kaynak grubunun kapsamına alınır. Uzun süre çalışan komut kapsayıcıyı çalışır durumda tutar. Bu örnek, anahtar kasasını oluşturmak için kullanılan kaynak grubunu kullanır, ancak farklı bir tane belirtebilirsiniz.

```azurecli-interactive
# Get the resource ID of the resource group
rgID=$(az group show --name myResourceGroup --query id --output tsv)

# Create container group with system-managed identity
az container create \
  --resource-group myResourceGroup \
  --name mycontainer \
  --image mcr.microsoft.com/azure-cli \
  --assign-identity --scope $rgID \
  --command-line "tail -f /dev/null"
```

Birkaç saniye içinde Azure CLI'den dağıtımın tamamlandığını belirten bir yanıt almanız gerekir. [Az Container Show](/cli/azure/container?view=azure-cli-latest#az-container-show) komutuyla durumunu kontrol edin.

```azurecli-interactive
az container show \
  --resource-group myResourceGroup \
  --name mycontainer
```

Çıktıda `identity` bölümü, sistem tarafından atanan kimliğin Azure Active Directory oluşturulduğunu gösteren aşağıdakine benzer şekilde görünür:

```console
[...]
"identity": {
    "principalId": "xxxxxxxx-528d-7083-b74c-xxxxxxxxxxxx",
    "tenantId": "xxxxxxxx-f292-4e60-9122-xxxxxxxxxxxx",
    "type": "SystemAssigned",
    "userAssignedIdentities": null
},
[...]
```

Sonraki adımlarda kullanmak üzere kimliğin `principalId` (hizmet sorumlusu KIMLIĞI) değerine bir değişken ayarlayın.

```azurecli-interactive
spID=$(az container show --resource-group myResourceGroup --name mycontainer --query identity.principalId --out tsv)
```

### <a name="grant-container-group-access-to-the-key-vault"></a>Anahtar kasasına kapsayıcı grubu erişimi verme

Anahtar kasasında bir erişim ilkesi ayarlamak için aşağıdaki [az keykasası Set-Policy](/cli/azure/keyvault?view=azure-cli-latest) komutunu çalıştırın. Aşağıdaki örnek, sistem tarafından yönetilen kimliğin anahtar kasasından gizli dizileri almasına izin verir:

```azurecli-interactive
 az keyvault set-policy \
   --name mykeyvault \
   --resource-group myResourceGroup \
   --object-id $spID \
   --secret-permissions get
```

### <a name="use-container-group-identity-to-get-secret-from-key-vault"></a>Anahtar kasasından gizli dizi almak için kapsayıcı grubu kimliğini kullanın

Artık yönetilen kimliği, çalışan kapsayıcı örneği içindeki anahtar kasasına erişmek için kullanabilirsiniz. Önce kapsayıcıda bir bash kabuğu başlatın:

```azurecli-interactive
az container exec \
  --resource-group myResourceGroup \
  --name mycontainer \
  --exec-command "/bin/bash"
```

Kapsayıcıda bash kabuğu 'nda aşağıdaki komutları çalıştırın. Önce yönetilen kimliği kullanarak Azure CLı 'da oturum açın:

```bash
az login --identity
```

Çalışan kapsayıcıdan, anahtar kasasından gizli dizi alın:

```bash
az keyvault secret show \
  --name SampleSecret \
  --vault-name mykeyvault --query value
```

Gizli dizinin değeri alınır:

```bash
"Hello Container Instances"
```

## <a name="enable-managed-identity-using-resource-manager-template"></a>Kaynak Yöneticisi şablonu kullanarak yönetilen kimliği etkinleştirme

Bir [Kaynak Yöneticisi şablonu](container-instances-multi-container-group.md)kullanarak bir kapsayıcı grubundaki yönetilen bir kimliği etkinleştirmek için, `Microsoft.ContainerInstance/containerGroups` nesnesinin `identity` özelliğini bir `ContainerGroupIdentity` nesnesi ile ayarlayın. Aşağıdaki kod parçacıkları farklı senaryolar için yapılandırılmış `identity` özelliğini gösterir. [Kaynak Yöneticisi şablonu başvurusuna](/azure/templates/microsoft.containerinstance/containergroups)bakın. En düşük `apiVersion` `2018-10-01`belirtin.

### <a name="user-assigned-identity"></a>Kullanıcı tarafından atanan kimlik

Kullanıcı tarafından atanan kimlik, formun bir kaynak KIMLIĞIDIR:

```
"/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ManagedIdentity/userAssignedIdentities/{identityName}"
``` 

Kullanıcı tarafından atanan bir veya daha fazla kimliği etkinleştirebilirsiniz.

```json
"identity": {
    "type": "UserAssigned",
    "userAssignedIdentities": {
        "myResourceID1": {
            }
        }
    }
```

### <a name="system-assigned-identity"></a>Sistem tarafından atanan kimlik

```json
"identity": {
    "type": "SystemAssigned"
    }
```

### <a name="system--and-user-assigned-identities"></a>Sistem ve Kullanıcı tarafından atanan kimlikler

Bir kapsayıcı grubunda, hem sistem tarafından atanan hem de bir veya daha fazla kullanıcı tarafından atanan kimlik sağlayabilirsiniz.

```json
"identity": {
    "type": "System Assigned, UserAssigned",
    "userAssignedIdentities": {
        "myResourceID1": {
            }
        }
    }
...
```

## <a name="enable-managed-identity-using-yaml-file"></a>YAML dosyasını kullanarak yönetilen kimliği etkinleştirme

Bir [YAML dosyası](container-instances-multi-container-yaml.md)kullanılarak dağıtılan bir kapsayıcı grubundaki yönetilen bir kimliği etkinleştirmek için aşağıdaki YAML 'yi ekleyin.
En düşük `apiVersion` `2018-10-01`belirtin.

### <a name="user-assigned-identity"></a>Kullanıcı tarafından atanan kimlik

Kullanıcı tarafından atanan kimlik, formun bir kaynak KIMLIĞIDIR 

```
'/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ManagedIdentity/userAssignedIdentities/{identityName}'
```

Kullanıcı tarafından atanan bir veya daha fazla kimliği etkinleştirebilirsiniz.

```YAML
identity:
  type: UserAssigned
  userAssignedIdentities:
    {'myResourceID1':{}}
```

### <a name="system-assigned-identity"></a>Sistem tarafından atanan kimlik

```YAML
identity:
  type: SystemAssigned
```

### <a name="system--and-user-assigned-identities"></a>Sistem ve Kullanıcı tarafından atanan kimlikler

Bir kapsayıcı grubunda, hem sistem tarafından atanan hem de bir veya daha fazla kullanıcı tarafından atanan kimlik sağlayabilirsiniz.

```YAML
identity:
  type: SystemAssigned, UserAssigned
  userAssignedIdentities:
   {'myResourceID1':{}}
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure Container Instances ' deki Yönetilen kimlikler hakkında bilgi edindiniz ve şunları yapabilirsiniz:

> [!div class="checklist"]
> * Kapsayıcı grubunda Kullanıcı tarafından atanan veya sistem tarafından atanan bir kimliği etkinleştirme
> * Azure Anahtar Kasası 'na kimlik erişimi verme
> * Çalışan bir kapsayıcıdan anahtar kasasına erişmek için yönetilen kimliği kullanma

* [Azure kaynakları için Yönetilen kimlikler](/azure/active-directory/managed-identities-azure-resources/)hakkında daha fazla bilgi edinin.

* Azure Container Instances 'ten bir anahtar kasasına erişmek için yönetilen kimlik kullanma hakkında bir [Azure go SDK örneğine](https://medium.com/@samkreter/c98911206328) bakın.
