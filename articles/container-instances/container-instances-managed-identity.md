---
title: Konteyner grubunda yönetilen kimliği etkinleştirme
description: Azure Kapsayıcı Örnekleri'nde diğer Azure hizmetleriyle kimlik doğrulaması yapabilen yönetilen bir kimliği nasıl etkinleştirebileceğinizi öğrenin
ms.topic: article
ms.date: 01/29/2020
ms.openlocfilehash: 19d2ab22eea15278c7753046f9222c7856fbf5ef
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81685654"
---
# <a name="how-to-use-managed-identities-with-azure-container-instances"></a>Azure Kapsayıcı Örnekleri ile yönetilen kimlikler nasıl kullanılır?

Azure [kaynaklarının,](../active-directory/managed-identities-azure-resources/overview.md) koddaki herhangi bir sır veya kimlik belgesi saklamadan diğer Azure hizmetleriyle etkileşimde bulunan Azure Kapsayıcı Örnekleri'nde kod çalıştırmak için yönetilen kimlikleri kullanın. Bu özellik, Azure Etkin Dizini'nde otomatik olarak yönetilen bir kimliğe sahip bir Azure Kapsayıcı Örnekleri dağıtımı sağlar.

Bu makalede, Azure Kapsayıcı Örnekleri'nde yönetilen kimlikler hakkında daha fazla bilgi edinin ve:

> [!div class="checklist"]
> * Kapsayıcı grubunda kullanıcı tarafından atanmış veya sistem tarafından atanmış bir kimliği etkinleştirme
> * Azure anahtar kasasına kimlik erişimi verme
> * Çalışan bir kapsayıcıdan anahtar kasasına erişmek için yönetilen kimliği kullanma

Diğer Azure hizmetlerine erişmek için Azure Kapsayıcı Örnekleri'ndeki kimlikleri etkinleştirmek ve kullanmak için örnekleri uyarlayın. Bu örnekler etkileşimlidir. Ancak, uygulamada kapsayıcı resimleriniz Azure hizmetlerine erişmek için kod çalıştırırdı.

> [!NOTE]
> Şu anda sanal ağa dağıtılan bir kapsayıcı grubunda yönetilen bir kimlik kullanamazsınız.

## <a name="why-use-a-managed-identity"></a>Neden yönetilen bir kimlik kullanıyorsun?

Kapsayıcı kodunuzda kimlik bilgilerini yönetmeden [Azure AD kimlik doğrulamasını destekleyen](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md#azure-services-that-support-azure-ad-authentication) herhangi bir hizmetin kimliğini doğrulamak için çalışan bir kapsayıcıda yönetilen bir kimlik kullanın. AD kimlik doğrulamasını desteklemeyen hizmetler için, sırları Azure anahtar kasasında saklayabilir ve kimlik bilgilerini almak için anahtar kasasına erişmek için yönetilen kimliği kullanabilirsiniz. Yönetilen bir kimlik kullanma hakkında daha fazla bilgi için Azure [kaynakları için yönetilen kimlikler nedir?](../active-directory/managed-identities-azure-resources/overview.md)

> [!IMPORTANT]
> Bu özellik şu anda önizleme sürümündedir. Önizlemeler, [ek kullanım koşullarını](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) kabul etmeniz şartıyla kullanımınıza sunulur. Bu özelliğin bazı yönleri genel kullanıma açılmadan önce değişebilir. Şu anda Azure Kapsayıcı Örnekleri'nde yönetilen kimlikler yalnızca Linux kapsayıcılarıyla desteklenir ve henüz Windows kapsayıcılarıyla desteklenmez.
>  

### <a name="enable-a-managed-identity"></a>Yönetilen bir kimliği etkinleştirme

 Azure Kapsayıcı Örnekleri'nde, AZURE kaynakları için yönetilen kimlikler REST API sürümü 2018-10-01 ve ilgili SDK'lar ve araçlar gibi desteklenir. Bir kapsayıcı grubu oluşturduğunuzda, [ContainerGroupIdentity](/rest/api/container-instances/containergroups/createorupdate#containergroupidentity) özelliği ayarlayarak bir veya daha fazla yönetilen kimlik etkinleştirin. Ayrıca, bir kapsayıcı grubu çalışmaya başladıktan sonra yönetilen kimlikleri etkinleştirebilir veya güncelleştirebilirsiniz - ya eylem kapsayıcı grubunun yeniden başlatılmasına neden olur. Kimlikleri yeni veya varolan bir kapsayıcı grubuna ayarlamak için Azure CLI, Kaynak Yöneticisi şablonu veya YAML dosyasını kullanın. 

Azure Kapsayıcı Örnekleri, her iki yönetilen Azure kimliği türünü destekler: kullanıcı tarafından atanmış ve sistem tarafından atanmış. Kapsayıcı grubunda, sistem tarafından atanmış bir kimliği, bir veya daha fazla kullanıcı tarafından atanmış kimlikleri veya her iki kimlik türünü etkinleştirebilirsiniz. 

* **Kullanıcı tarafından atanan** yönetilen kimlik, Azure AD kiracısında, abonelik tarafından kullanılan güvenilen bağımsız bir Azure kaynağı olarak oluşturulur. Kimlik oluşturulduktan sonra, kimlik bir veya daha fazla Azure kaynağına (Azure Kapsayıcı Örnekleri'nde veya diğer Azure hizmetlerinde) atanabilir. Kullanıcı tarafından atanan bir kimliğin yaşam döngüsü, kap gruplarının veya atandığı diğer hizmet kaynaklarının yaşam döngüsünden ayrı olarak yönetilir. Bu davranış özellikle Azure Kapsayıcı Örnekleri'nde yararlıdır. Kimlik bir kapsayıcı grubunun ömrünün ötesine uzandığından, kapsayıcı grup dağıtımlarınızı son derece yinelenebilir hale getirmek için diğer standart ayarlarla birlikte yeniden kullanabilirsiniz.

* **Sistem tarafından atanan** yönetilen kimlik, Azure Kapsayıcı Örnekleri'ndeki bir kapsayıcı grubunda doğrudan etkinleştirilir. Azure etkinleştirildiğinde, Azure AD kiracısında, örneğin aboneliği tarafından güvenilen bir grup için bir kimlik oluşturur. Kimlik oluşturulduktan sonra, kimlik bilgileri kapsayıcı grubundaki her kapsayıcıda karşılanır. Sistemtarafından atanan bir kimliğin yaşam döngüsü, etkinleştirilen kapsayıcı grubuna doğrudan bağlıdır. Grup silindiğinde, Azure Azure AD'deki kimlik bilgilerini ve kimliği otomatik olarak temizler.

### <a name="use-a-managed-identity"></a>Yönetilen kimlik kullanma

Yönetilen bir kimliği kullanmak için, kimlik başlangıçta abonelikteki bir veya daha fazla Azure hizmet kaynağına (web uygulaması, önemli bir kasa veya depolama hesabı gibi) erişim izni verilmesi gerekir. Çalışan bir kapsayıcıdan Azure kaynaklarına erişmek için, kodunuz bir Azure AD bitiş noktasından bir *erişim belirteci* edinmeniz gerekir. Ardından kodunuz, bir çağrıdaki erişim jetonunu Azure AD kimlik doğrulamasını destekleyen bir hizmete gönderir. 

Çalışan bir kapsayıcıda yönetilen bir kimlik kullanmak, temelde Azure VM'de kimlik kullanmakla aynıdır. [Bir belirteç](../active-directory/managed-identities-azure-resources/how-to-use-vm-token.md), [Azure PowerShell veya Azure CLI](../active-directory/managed-identities-azure-resources/how-to-use-vm-sign-in.md)veya [Azure SDC'leri](../active-directory/managed-identities-azure-resources/how-to-use-vm-sdk.md)kullanmak için VM kılavuzuna bakın.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yüklemeyi ve kullanmayı seçerseniz, bu makalede Azure CLI sürümü 2.0.49 veya sonraki sürümlerini çalıştırdığınız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme](/cli/azure/install-azure-cli).

## <a name="create-an-azure-key-vault"></a>Azure anahtar kasası oluşturma

Bu makaledeki örnekler, Azure anahtar kasası sırrına erişmek için Azure Kapsayıcı Örnekleri'nde yönetilen bir kimlik kullanır. 

İlk olarak aşağıdaki [az group create](/cli/azure/group?view=azure-cli-latest#az-group-create) komutunu kullanarak *eastus* bölgesinde *myResourceGroup* adlı bir kaynak grubu oluşturun:

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Bir [anahtar tonoz](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-create) oluşturmak için az keyvault oluşturma komutunu kullanın. Benzersiz bir anahtar kasa adı belirttiğinden emin olun. 

```azurecli-interactive
az keyvault create \
  --name mykeyvault \
  --resource-group myResourceGroup \ 
  --location eastus
```

[Az keyvault gizli set](/cli/azure/keyvault/secret?view=azure-cli-latest#az-keyvault-secret-set) komutunu kullanarak anahtar kasasında bir örnek sırrı saklayın:

```azurecli-interactive
az keyvault secret set \
  --name SampleSecret \
  --value "Hello Container Instances" \
  --description ACIsecret --vault-name mykeyvault
```

Azure Kapsayıcı Örnekleri'nde kullanıcı tarafından atanmış veya sistem tarafından atanan yönetilen bir kimliği kullanarak anahtar kasasına erişmek için aşağıdaki örneklerle devam edin.

## <a name="example-1-use-a-user-assigned-identity-to-access-azure-key-vault"></a>Örnek 1: Azure anahtar kasasına erişmek için kullanıcı tarafından atanmış bir kimlik kullanın

### <a name="create-an-identity"></a>Kimlik oluşturma

Önce az kimlik oluşturma komutunu kullanarak aboneliğinizde bir [kimlik oluşturun.](/cli/azure/identity?view=azure-cli-latest#az-identity-create) Anahtar kasasını oluşturmak için kullanılan aynı kaynak grubunu kullanabilir veya farklı bir kaynak kullanabilirsiniz.

```azurecli-interactive
az identity create \
  --resource-group myResourceGroup \
  --name myACIId
```

Aşağıdaki adımlarda kimliği kullanmak için, kimliğin hizmet ana kimliğini ve kaynak kimliğini değişkenlerde depolamak için [az identity show](/cli/azure/identity?view=azure-cli-latest#az-identity-show) komutunu kullanın.

```azurecli-interactive
# Get service principal ID of the user-assigned identity
spID=$(az identity show --resource-group myResourceGroup --name myACIId --query principalId --output tsv)

# Get resource ID of the user-assigned identity
resourceID=$(az identity show --resource-group myResourceGroup --name myACIId --query id --output tsv)
```

### <a name="enable-a-user-assigned-identity-on-a-container-group"></a>Kapsayıcı grubunda kullanıcı tarafından atanan kimliği etkinleştirme

Microsoft'un [az container create](/cli/azure/container?view=azure-cli-latest#az-container-create) `azure-cli` resmine dayalı bir kapsayıcı örneği oluşturmak için aşağıdaki az kapsayıcı oluşturma komutunu çalıştırın. Bu örnek, diğer Azure hizmetlerine erişmek için Azure CLI'yi çalıştırmak için etkileşimli olarak kullanabileceğiniz tek kapsayıcı bir grup sağlar. Bu bölümde, sadece baz Ubuntu işletim sistemi kullanılır. 

Parametre, `--assign-identity` kullanıcı tarafından atanan yönetilen kimliğinizi gruba geçirir. Uzun süren komut kapsayıcıyı çalışır durumda tutar. Bu örnek, anahtar tonozoluşturmak için kullanılan aynı kaynak grubunu kullanır, ancak farklı bir tane belirtebilirsiniz.

```azurecli-interactive
az container create \
  --resource-group myResourceGroup \
  --name mycontainer \
  --image mcr.microsoft.com/azure-cli \
  --assign-identity $resourceID \
  --command-line "tail -f /dev/null"
```

Birkaç saniye içinde Azure CLI'den dağıtımın tamamlandığını belirten bir yanıt almanız gerekir. [Az kapsayıcı göster](/cli/azure/container?view=azure-cli-latest#az-container-show) komutu ile durumunu kontrol edin.

```azurecli-interactive
az container show \
  --resource-group myResourceGroup \
  --name mycontainer
```

Çıktıdaki `identity` bölüm, kimliğin kapsayıcı grubunda ayarlanırken aşağıdakine benzer. `principalID` Alt, `userAssignedIdentities` Azure Etkin Dizini'nde oluşturduğunuz kimliğin hizmet ilkesidir:

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

### <a name="grant-user-assigned-identity-access-to-the-key-vault"></a>Anahtar kasasına kullanıcı tarafından atanan kimlik erişimi verme

Anahtar kasasında bir erişim ilkesi ayarlamak için aşağıdaki [az keyvault ayar ilkesi](/cli/azure/keyvault?view=azure-cli-latest) komutunu çalıştırın. Aşağıdaki örnek, kullanıcı tarafından atanan kimliğin anahtar kasasından sırları almalarına olanak tanır:

```azurecli-interactive
 az keyvault set-policy \
    --name mykeyvault \
    --resource-group myResourceGroup \
    --object-id $spID \
    --secret-permissions get
```

### <a name="use-user-assigned-identity-to-get-secret-from-key-vault"></a>Anahtar kasasından gizli kalmak için kullanıcı tarafından atanan kimliği kullanma

Artık anahtar kasasına erişmek için çalışan kapsayıcı örneğinde yönetilen kimliği kullanabilirsiniz. Konteyner de ilk başlatmak bir bash kabuk:

```azurecli-interactive
az container exec \
  --resource-group myResourceGroup \
  --name mycontainer \
  --exec-command "/bin/bash"
```

Konteynerde bash kabuk aşağıdaki komutları çalıştırın. Anahtar kasasına kimlik doğrulamak için Azure Active Directory'yi kullanmak için bir erişim jetonu almak için aşağıdaki komutu çalıştırın:

```bash
curl 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fvault.azure.net' -H Metadata:true -s
```

Çıktı:

```bash
{"access_token":"xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Imk2bEdrM0ZaenhSY1ViMkMzbkVRN3N5SEpsWSIsImtpZCI6Imk2bEdrM0ZaenhSY1ViMkMzbkVRN3N5SEpsWSJ9......xxxxxxxxxxxxxxxxx","refresh_token":"","expires_in":"28799","expires_on":"1539927532","not_before":"1539898432","resource":"https://vault.azure.net/","token_type":"Bearer"}
```

Erişim belirtecinin kimlik doğrulaması için sonraki komutlarda kullanılacak bir değişkende depolamasını sağlamak için aşağıdaki komutu çalıştırın:

```bash
token=$(curl 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fvault.azure.net' -H Metadata:true | jq -r '.access_token')

```

Şimdi anahtar kasasına kimlik doğrulamak ve bir sırrı okumak için erişim belirteci kullanın. URL'deki anahtar kasanızın adını değiştirin (*https:\//mykeyvault.vault.azure.net/...*):

```bash
curl https://mykeyvault.vault.azure.net/secrets/SampleSecret/?api-version=2016-10-01 -H "Authorization: Bearer $token"
```

Yanıt, sırrı gösteren aşağıdaki gibi görünüyor. Kodunuzda, sırrı elde etmek için bu çıktıyı ayrıştırabilirsiniz. Ardından, başka bir Azure kaynağına erişmek için sonraki bir işlemde sırrı kullanın.

```bash
{"value":"Hello Container Instances","contentType":"ACIsecret","id":"https://mykeyvault.vault.azure.net/secrets/SampleSecret/xxxxxxxxxxxxxxxxxxxx","attributes":{"enabled":true,"created":1539965967,"updated":1539965967,"recoveryLevel":"Purgeable"},"tags":{"file-encoding":"utf-8"}}
```

## <a name="example-2-use-a-system-assigned-identity-to-access-azure-key-vault"></a>Örnek 2: Azure anahtar kasasına erişmek için sistem tarafından atanmış bir kimlik kullanın

### <a name="enable-a-system-assigned-identity-on-a-container-group"></a>Kapsayıcı grubunda sistem tarafından atanmış bir kimliği etkinleştirme

Microsoft'un [az container create](/cli/azure/container?view=azure-cli-latest#az-container-create) `azure-cli` resmine dayalı bir kapsayıcı örneği oluşturmak için aşağıdaki az kapsayıcı oluşturma komutunu çalıştırın. Bu örnek, diğer Azure hizmetlerine erişmek için Azure CLI'yi çalıştırmak için etkileşimli olarak kullanabileceğiniz tek kapsayıcı bir grup sağlar. 

Ek `--assign-identity` değeri olmayan parametre, grup üzerinde sistem tarafından atanmış yönetilen bir kimlik sağlar. Kimlik, kapsayıcı grubunun kaynak grubuna kapsamlıdır. Uzun süren komut kapsayıcıyı çalışır durumda tutar. Bu örnek, anahtar tonozoluşturmak için kullanılan aynı kaynak grubunu kullanır, ancak farklı bir tane belirtebilirsiniz.

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

Birkaç saniye içinde Azure CLI'den dağıtımın tamamlandığını belirten bir yanıt almanız gerekir. [Az kapsayıcı göster](/cli/azure/container?view=azure-cli-latest#az-container-show) komutu ile durumunu kontrol edin.

```azurecli-interactive
az container show \
  --resource-group myResourceGroup \
  --name mycontainer
```

Çıktıdaki `identity` bölüm, Azure Etkin Dizini'nde sistem tarafından atanmış bir kimliğin oluşturulduğunu gösteren aşağıdakibölüme benzer:

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

Daha sonraki adımlarda `principalId` kullanmak üzere kimliğin değerine (hizmet ana kimliği) bir değişken ayarlayın.

```azurecli-interactive
spID=$(az container show --resource-group myResourceGroup --name mycontainer --query identity.principalId --out tsv)
```

### <a name="grant-container-group-access-to-the-key-vault"></a>Anahtar kasasına konteyner grubu erişimi ver

Anahtar kasasında bir erişim ilkesi ayarlamak için aşağıdaki [az keyvault ayar ilkesi](/cli/azure/keyvault?view=azure-cli-latest) komutunu çalıştırın. Aşağıdaki örnek, sistem tarafından yönetilen kimliğin anahtar kasasından sır lar almanızı sağlar:

```azurecli-interactive
 az keyvault set-policy \
   --name mykeyvault \
   --resource-group myResourceGroup \
   --object-id $spID \
   --secret-permissions get
```

### <a name="use-container-group-identity-to-get-secret-from-key-vault"></a>Anahtar kasasından gizli almak için konteyner grubu kimliğini kullanın

Artık çalışan kapsayıcı örneğindeki anahtar kasasına erişmek için yönetilen kimliği kullanabilirsiniz. Konteyner de ilk başlatmak bir bash kabuk:

```azurecli-interactive
az container exec \
  --resource-group myResourceGroup \
  --name mycontainer \
  --exec-command "/bin/bash"
```

Konteynerde bash kabuk aşağıdaki komutları çalıştırın. Yönetilen kimliği kullanarak Azure CLI'ye ilk giriş yapın:

```bash
az login --identity
```

Çalışan konteynerden, anahtarı kasadan sırrı alın:

```bash
az keyvault secret show \
  --name SampleSecret \
  --vault-name mykeyvault --query value
```

Sırrın değeri alınır:

```bash
"Hello Container Instances"
```

## <a name="enable-managed-identity-using-resource-manager-template"></a>Kaynak Yöneticisi şablonu kullanarak yönetilen kimliği etkinleştirme

[Kaynak Yöneticisi şablonu](container-instances-multi-container-group.md)kullanarak kapsayıcı grubunda yönetilen bir `identity` kimliği etkinleştirmek için nesnenin özelliğini `Microsoft.ContainerInstance/containerGroups` nesneyle `ContainerGroupIdentity` ayarlayın. Aşağıdaki parçacıklar, farklı `identity` senaryolar için yapılandırılan özelliği gösterir. Kaynak [Yöneticisi şablon başvurusuna](/azure/templates/microsoft.containerinstance/containergroups)bakın. En az `apiVersion` `2018-10-01`.

### <a name="user-assigned-identity"></a>Kullanıcı tarafından atanan kimlik

Kullanıcı tarafından atanan kimlik, formun kaynak kimliğidir:

```
"/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ManagedIdentity/userAssignedIdentities/{identityName}"
``` 

Bir veya daha fazla kullanıcı tarafından atanan kimlikleri etkinleştirebilirsiniz.

```json
"identity": {
    "type": "UserAssigned",
    "userAssignedIdentities": {
        "myResourceID1": {
            }
        }
    }
```

### <a name="system-assigned-identity"></a>Sisteme atanmış kimlik

```json
"identity": {
    "type": "SystemAssigned"
    }
```

### <a name="system--and-user-assigned-identities"></a>Sistem ve kullanıcı tarafından atanan kimlikler

Kapsayıcı grubunda, hem sistem tarafından atanmış bir kimliği hem de kullanıcı tarafından atanmış bir veya daha fazla kimliği etkinleştirebilirsiniz.

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

[YAML dosyası](container-instances-multi-container-yaml.md)kullanılarak dağıtılan bir kapsayıcı grubunda yönetilen bir kimliği etkinleştirmek için aşağıdaki YAML'yi ekleyin.
En az `apiVersion` `2018-10-01`.

### <a name="user-assigned-identity"></a>Kullanıcı tarafından atanan kimlik

Kullanıcı tarafından atanan kimlik, formun kaynak kimliğidir 

```
'/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ManagedIdentity/userAssignedIdentities/{identityName}'
```

Bir veya daha fazla kullanıcı tarafından atanan kimlikleri etkinleştirebilirsiniz.

```YAML
identity:
  type: UserAssigned
  userAssignedIdentities:
    {'myResourceID1':{}}
```

### <a name="system-assigned-identity"></a>Sisteme atanmış kimlik

```YAML
identity:
  type: SystemAssigned
```

### <a name="system--and-user-assigned-identities"></a>Sistem ve kullanıcı tarafından atanan kimlikler

Kapsayıcı grubunda, hem sistem tarafından atanmış bir kimliği hem de kullanıcı tarafından atanmış bir veya daha fazla kimliği etkinleştirebilirsiniz.

```YAML
identity:
  type: SystemAssigned, UserAssigned
  userAssignedIdentities:
   {'myResourceID1':{}}
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure Kapsayıcı Örnekleri'nde yönetilen kimlikleri ve nasıl şunları öğrendiniz:

> [!div class="checklist"]
> * Kapsayıcı grubunda kullanıcı tarafından atanmış veya sistem tarafından atanmış bir kimliği etkinleştirme
> * Azure anahtar kasasına kimlik erişimi verme
> * Çalışan bir kapsayıcıdan anahtar kasasına erişmek için yönetilen kimliği kullanma

* [Azure kaynakları için yönetilen kimlikler](/azure/active-directory/managed-identities-azure-resources/)hakkında daha fazla bilgi edinin.

* Azure Kapsayıcı Örnekleri'nden önemli bir kasaya erişmek için yönetilen bir kimlik kullanmanın Azure [Go SDK örneğine](https://medium.com/@samkreter/c98911206328) bakın.
