---
title: Kayıt defterini müşterinin yönettiği bir anahtarla şifreleyin
description: Azure Container Registry 'nizin geri kalanı hakkında bilgi edinin ve Premium kayıt defterinizi, Azure Key Vault depolanan müşteri tarafından yönetilen bir anahtarla nasıl şifreleyeceğinizi öğrenin
ms.topic: article
ms.date: 12/03/2020
ms.custom: ''
ms.openlocfilehash: fb30610457e539250c33d7d9726fe10f9c0f8c5a
ms.sourcegitcommit: 1a98b3f91663484920a747d75500f6d70a6cb2ba
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99062737"
---
# <a name="encrypt-registry-using-a-customer-managed-key"></a>Müşteri tarafından yönetilen anahtar kullanarak kayıt defterini şifreleme

Azure Container Registry 'de görüntüleri ve diğer yapıtları depoladığınız zaman, Azure, [hizmet tarafından yönetilen anahtarlarla](../security/fundamentals/encryption-models.md)geri kalan kayıt defteri içeriğini otomatik olarak şifreler. Azure Key Vault (müşteri tarafından yönetilen bir anahtar) içinde oluşturduğunuz ve yönettiğiniz bir anahtarı kullanarak, varsayılan şifrelemeyi ek bir şifreleme katmanıyla birlikte destekleyebilirsiniz. Bu makalede, Azure CLı ve Azure portal arasındaki adımlarda adım adım açıklanmaktadır.

Müşteri tarafından yönetilen anahtarlarla sunucu tarafında şifreleme, [Azure Key Vault](../key-vault/general/overview.md)tümleştirme aracılığıyla desteklenir: 

* Kendi şifreleme anahtarlarınızı oluşturabilir ve bunları bir anahtar kasasında saklayabilir veya anahtarlar oluşturmak için Azure Key Vault API 'Lerini kullanabilirsiniz. 
* Azure Key Vault, anahtar kullanımını da denetleyebilirsiniz.
* Azure Container Registry, Azure Key Vault yeni bir anahtar sürümü kullanılabilir olduğunda, kayıt defteri şifreleme anahtarlarının otomatik döndürmesini destekler. Ayrıca, kayıt defteri şifreleme anahtarlarını el ile de döndürebilirsiniz.

Bu özellik **Premium** kapsayıcı kayıt defteri hizmet katmanında kullanılabilir. Kayıt defteri hizmeti katmanları ve limitleri hakkında bilgi için bkz. [Azure Container Registry hizmet katmanları](container-registry-skus.md).


## <a name="things-to-know"></a>Bilinmesi gerekenler

* Şu anda yalnızca bir kayıt defteri oluşturduğunuzda müşterinin yönettiği bir anahtarı etkinleştirebilirsiniz. Anahtarı etkinleştirirken, anahtar kasasına erişmek için *Kullanıcı tarafından atanan* bir yönetilen kimlik yapılandırırsınız.
* Bir kayıt defterinde müşterinin yönettiği anahtarla şifrelemeyi etkinleştirdikten sonra şifrelemeyi devre dışı bırakamıyoruz.  
* Azure Container Registry yalnızca RSA veya RSA-HSM anahtarlarını destekler. Eliptik Eğri anahtarları şu anda desteklenmiyor.
* [İçerik güveni](container-registry-content-trust.md) , müşteri tarafından yönetilen bir anahtarla şifrelenen bir kayıt defterinde şu anda desteklenmiyor.
* Müşteri tarafından yönetilen bir anahtarla şifrelenen bir kayıt defterinde, [ACR görevleri](container-registry-tasks-overview.md) için çalıştırılan Günlükler Şu anda yalnızca 24 saat boyunca saklanır. Daha uzun bir süre için günlükleri tutmanız gerekiyorsa, bkz. [görev çalıştırma günlüklerini dışarı ve depolama](container-registry-tasks-logs.md#alternative-log-storage)Kılavuzu.


> [!NOTE]
> Azure anahtar kasanıza erişim [Key Vault bir güvenlik duvarı](../key-vault/general/network-security.md)ile bir sanal ağ kullanılarak kısıtlanmışsa, ek yapılandırma adımları gereklidir. Kayıt defterini oluşturduktan ve müşteri tarafından yönetilen anahtarı etkinleştirdikten sonra, kayıt defterinin *sistem tarafından atanan* yönetilen kimliğini kullanarak anahtara erişimi ayarlayın ve kayıt defterini Key Vault güvenlik duvarını atlayacak şekilde yapılandırın. Müşteri tarafından yönetilen bir anahtarla şifrelemeyi etkinleştirmek için önce bu makaledeki adımları izleyin ve ardından Gelişmiş senaryo için rehbere bakın [: Key Vault güvenlik duvarı](#advanced-scenario-key-vault-firewall) Bu makalede daha sonra.

## <a name="automatic-or-manual-update-of-key-versions"></a>Anahtar sürümlerinin otomatik veya el ile güncelleştirilmesi

Müşteri tarafından yönetilen anahtarla şifrelenen bir kayıt defterinin güvenliği için önemli bir göz önünde bulundurmanız gereken, şifreleme anahtarını ne sıklıkta güncelleştirmiş (döndürdüğünüzde). Kuruluşunuz, müşteri tarafından yönetilen anahtarlar olarak kullanıldığında Azure Key Vault depolanan anahtar [sürümlerinin](../key-vault/general/about-keys-secrets-certificates.md#objects-identifiers-and-versioning) düzenli olarak güncelleştirilmesini gerektiren uyumluluk ilkelerine sahip olabilir. 

Kayıt defteri şifrelemesini müşteri tarafından yönetilen bir anahtarla yapılandırdığınızda, şifreleme için kullanılan anahtar sürümünü güncelleştirmek için iki seçeneğiniz vardır:

* **Anahtar sürümünü otomatik olarak güncelleştir** -Azure Key Vault yeni bir sürüm kullanılabilir olduğunda, müşteri tarafından yönetilen bir anahtarı otomatik olarak güncelleştirmek için, müşteri tarafından yönetilen bir anahtarla kayıt defteri şifrelemesini etkinleştirdiğinizde anahtar sürümünü atlayın. Bir kayıt defteri sürümü olmayan bir anahtarla şifrelendiğinde, Azure Container Registry yeni bir anahtar sürümünün anahtar kasasını düzenli olarak kontrol eder ve müşteri tarafından yönetilen anahtarı 1 saat içinde güncelleştirir. Azure Container Registry, anahtarın en son sürümünü otomatik olarak kullanır.

* **Anahtar sürümünü el ile güncelleştirme** -kayıt defteri şifrelemesi için bir anahtarın belirli bir sürümünü kullanmak üzere, müşteri tarafından yönetilen bir anahtarla kayıt defteri şifrelemesini etkinleştirdiğinizde bu anahtar sürümünü belirtin. Bir kayıt defteri belirli bir anahtar sürümüyle şifrelendiğinde, Azure Container Registry, müşteri tarafından yönetilen anahtarı el ile döndürünceye kadar bu sürümü şifreleme için kullanır.

Ayrıntılar için, bu makalenin sonraki kısımlarında yer alarak anahtar KIMLIĞI, anahtar sürümü ve [güncelleştirme anahtarı sürümünü](#update-key-version) [seçin](#choose-key-id-with-or-without-key-version) bölümüne bakın.

## <a name="prerequisites"></a>Önkoşullar

Bu makaledeki Azure CLı adımlarını kullanmak için Azure CLı sürüm 2.2.0 veya üzeri ya da Azure Cloud Shell gerekir. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme](/cli/azure/install-azure-cli).

## <a name="enable-customer-managed-key---cli"></a>Müşteri tarafından yönetilen anahtar CLı 'yı etkinleştir

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Gerekirse, Anahtar Kasası, kapsayıcı kayıt defteri ve diğer gerekli kaynakları oluşturmak üzere bir kaynak grubu oluşturmak için [az Group Create][az-group-create] komutunu çalıştırın.

```azurecli
az group create --name <resource-group-name> --location <location>
```

### <a name="create-a-user-assigned-managed-identity"></a>Kullanıcı tarafından atanan yönetilen kimlik oluşturma

[Az Identity Create][az-identity-create] komutuyla [Azure kaynakları için](../active-directory/managed-identities-azure-resources/overview.md) Kullanıcı tarafından atanan bir yönetilen kimlik oluşturun. Bu kimlik kayıt defteriniz tarafından Key Vault hizmetine erişmek için kullanılacaktır.

```azurecli
az identity create \
  --resource-group <resource-group-name> \
  --name <managed-identity-name>
```

Komut çıkışında, aşağıdaki değerleri göz önünde atın: `id` ve `principalId` . Anahtar kasasına kayıt defteri erişimini yapılandırmak için sonraki adımlarda bu değerlere ihtiyacınız vardır.

```JSON
{
  "clientId": "xxxx2bac-xxxx-xxxx-xxxx-192cxxxx6273",
  "clientSecretUrl": "https://control-eastus.identity.azure.net/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/myresourcegroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/myidentityname/credentials?tid=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx&oid=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx&aid=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/myresourcegroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/myresourcegroup",
  "location": "eastus",
  "name": "myidentityname",
  "principalId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "resourceGroup": "myresourcegroup",
  "tags": {},
  "tenantId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "type": "Microsoft.ManagedIdentity/userAssignedIdentities"
}
```

Kolaylık olması için bu değerleri ortam değişkenlerine depolayın:

```azurecli
identityID=$(az identity show --resource-group <resource-group-name> --name <managed-identity-name> --query 'id' --output tsv)

identityPrincipalID=$(az identity show --resource-group <resource-group-name> --name <managed-identity-name> --query 'principalId' --output tsv)
```

### <a name="create-a-key-vault"></a>Anahtar kasası oluşturma

Kayıt defteri şifrelemesi için müşteri tarafından yönetilen bir anahtar depolamak üzere [az keykasacreate][az-keyvault-create] ile bir Anahtar Kasası oluşturun.

Varsayılan olarak, **geçici silme** ayarı yeni bir anahtar kasasında otomatik olarak etkinleştirilir. Yanlışlıkla anahtar veya Anahtar Kasası silme işlemlerinin neden olduğu veri kaybını engellemek için, **Temizleme koruma** ayarını da etkinleştirin:

```azurecli
az keyvault create --name <key-vault-name> \
  --resource-group <resource-group-name> \
  --enable-purge-protection
```

Sonraki adımlarda kullanmak için, anahtar kasasının kaynak KIMLIĞINI alın:

```azurecli
keyvaultID=$(az keyvault show --resource-group <resource-group-name> --name <key-vault-name> --query 'id' --output tsv)
```

### <a name="enable-key-vault-access"></a>Anahtar Kasası erişimini etkinleştir

Kimliğin erişebilmesi için Anahtar Kasası için bir ilke yapılandırın. Aşağıdaki [az keykasası Set-Policy][az-keyvault-set-policy] komutunda, oluşturduğunuz yönetilen KIMLIğIN asıl kimliğini daha önce bir ortam değişkeninde depoladığınız şekilde geçirirsiniz. **Get**, **unwrapKey** ve **wrapKey** için anahtar izinleri ayarlayın.  

```azurecli
az keyvault set-policy \
  --resource-group <resource-group-name> \
  --name <key-vault-name> \
  --object-id $identityPrincipalID \
  --key-permissions get unwrapKey wrapKey
```

Alternatif olarak, anahtar kasasına erişmek için kimliğe izinler atamak üzere [Key Vault Için Azure RBAC](../key-vault/general/rbac-guide.md) (Önizleme) kullanın. Örneğin, [az role atama Create](/cli/azure/role/assignment#az-role-assignment-create) komutunu kullanarak Key Vault şifreleme hizmeti şifreleme rolünü kimliğe atayın:

```azurecli 
az role assignment create --assignee $identityPrincipalID \
  --role "Key Vault Crypto Service Encryption (preview)" \
  --scope $keyvaultID
```

### <a name="create-key-and-get-key-id"></a>Anahtar oluştur ve anahtar KIMLIĞI al

Anahtar kasasında bir anahtar oluşturmak için [az keykasa Key Create][az-keyvault-key-create] komutunu çalıştırın.

```azurecli
az keyvault key create \
  --name <key-name> \
  --vault-name <key-vault-name>
```

Komut çıkışında, anahtarın KIMLIĞINI bir yere göz atın `kid` . Sonraki adımda bu KIMLIĞI kullanırsınız:

```JSON
[...]
  "key": {
    "crv": null,
    "d": null,
    "dp": null,
    "dq": null,
    "e": "AQAB",
    "k": null,
    "keyOps": [
      "encrypt",
      "decrypt",
      "sign",
      "verify",
      "wrapKey",
      "unwrapKey"
    ],
    "kid": "https://mykeyvault.vault.azure.net/keys/mykey/<version>",
    "kty": "RSA",
[...]
```

### <a name="choose-key-id-with-or-without-key-version"></a>Anahtar sürümü olan veya olmayan anahtar KIMLIĞINI seçin

Kolaylık sağlaması için, $keyID ortam değişkeninde anahtar KIMLIĞI için seçtiğiniz biçimi saklayın. Sürüm olmadan bir sürüm veya anahtarla anahtar KIMLIĞINI kullanabilirsiniz.

#### <a name="manual-key-rotation---key-id-with-version"></a>El ile anahtar döndürme-sürümü olan anahtar KIMLIĞI

Müşteri tarafından yönetilen bir anahtarla bir kayıt defterini şifrelemek için kullanıldığında, bu anahtar yalnızca Azure Container Registry içinde el ile anahtar dönüşmesine izin verir.

Bu örnek, anahtarın özelliğini depolar `kid` :

```azurecli
keyID=$(az keyvault key show \
  --name <keyname> \
  --vault-name <key-vault-name> \
  --query 'key.kid' --output tsv)
```

#### <a name="automatic-key-rotation---key-id-omitting-version"></a>Otomatik anahtar döndürme-anahtar KIMLIĞI atlama sürümü 

Bu anahtar, müşteri tarafından yönetilen bir anahtarla bir kayıt defterini şifrelemek için kullanıldığında, Azure Key Vault yeni bir anahtar sürümü algılandığında otomatik anahtar döndürmeyi sağlar.

Bu örnek, sürümü anahtarın `kid` özelliğinden kaldırır:

```azurecli
keyID=$(az keyvault key show \
  --name <keyname> \
  --vault-name <key-vault-name> \
  --query 'key.kid' --output tsv)

keyID=$(echo $keyID | sed -e "s/\/[^/]*$//")
```

### <a name="create-a-registry-with-customer-managed-key"></a>Müşteri tarafından yönetilen anahtarla kayıt defteri oluşturma

Premium hizmet katmanında bir kayıt defteri oluşturmak ve müşterinin yönettiği anahtarı etkinleştirmek için [az ACR Create][az-acr-create] komutunu çalıştırın. Daha önce ortam değişkenlerinde depolanan yönetilen kimlik KIMLIĞINI ve anahtar KIMLIĞINI geçirin:

```azurecli
az acr create \
  --resource-group <resource-group-name> \
  --name <container-registry-name> \
  --identity $identityID \
  --key-encryption-key $keyID \
  --sku Premium
```

### <a name="show-encryption-status"></a>Şifreleme durumunu göster

Müşteri tarafından yönetilen bir anahtarla kayıt defteri şifrelemesinin etkinleştirilip etkinleştirilmeyeceğini göstermek için [az ACR ENCRYPTION Show][az-acr-encryption-show] komutunu çalıştırın:

```azurecli
az acr encryption show --name <registry-name>
```

Kayıt defterini şifrelemek için kullanılan anahtara bağlı olarak, çıkış şuna benzerdir:

```console
{
  "keyVaultProperties": {
    "identity": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "keyIdentifier": "https://myvault.vault.azure.net/keys/myresourcegroup/abcdefg123456789...",
    "versionedKeyIdentifier": "https://myvault.vault.azure.net/keys/myresourcegroup/abcdefg123456789...",
    "keyRotationEnabled": true,
    "lastKeyRotationTimestamp": xxxxxxxx
  },
  "status": "enabled"
}
```

## <a name="enable-customer-managed-key---portal"></a>Müşteri tarafından yönetilen anahtarı etkinleştirme-Portal

### <a name="create-a-managed-identity"></a>Yönetilen kimlik oluşturma

Azure portal [Azure kaynakları için](../active-directory/managed-identities-azure-resources/overview.md) Kullanıcı tarafından atanan bir yönetilen kimlik oluşturun. Adımlar için bkz. [Kullanıcı tarafından atanan kimlik oluşturma](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md#create-a-user-assigned-managed-identity).

Daha sonraki adımlarda kimliğin adını kullanırsınız.

:::image type="content" source="media/container-registry-customer-managed-keys/create-managed-identity.png" alt-text="Azure portal Kullanıcı tarafından atanan kimlik oluşturma":::

### <a name="create-a-key-vault"></a>Anahtar kasası oluşturma

Bir Anahtar Kasası oluşturma adımları için bkz. [hızlı başlangıç: Azure Portal kullanarak bir Anahtar Kasası oluşturma](../key-vault/general/quick-create-portal.md).

Müşteri tarafından yönetilen anahtar için bir Anahtar Kasası oluştururken, **temel bilgiler** sekmesinde, **korumayı temizle** ayarını etkinleştirin. Bu ayar, yanlışlıkla anahtar veya Anahtar Kasası silmeleri nedeniyle oluşan veri kaybını önlemeye yardımcı olur.

:::image type="content" source="media/container-registry-customer-managed-keys/create-key-vault.png" alt-text="Azure portal Anahtar Kasası oluşturma":::

### <a name="enable-key-vault-access"></a>Anahtar Kasası erişimini etkinleştir

Kimliğin erişebilmesi için Anahtar Kasası için bir ilke yapılandırın.

1. Anahtar kasanıza gidin.
1. **Ayarlar**  >  **erişim ilkeleri > + erişim ilkesi Ekle**' yi seçin.
1. **Anahtar izinleri**' ni seçin ve **Al**, **anahtar kaydırmayı kaldır** ve **anahtar sarmalama**' i seçin.
1. **Asıl seçin**' de, Kullanıcı tarafından atanan yönetilen kimliğinizin kaynak adını seçin.  
1. **Ekle**' yi ve ardından **Kaydet**' i seçin.

:::image type="content" source="media/container-registry-customer-managed-keys/add-key-vault-access-policy.png" alt-text="Anahtar Kasası erişim ilkesi oluşturma":::

Alternatif olarak, anahtar kasasına erişmek için kimliğe izinler atamak üzere [Key Vault Için Azure RBAC](../key-vault/general/rbac-guide.md) (Önizleme) kullanın. Örneğin, Key Vault şifreleme hizmeti şifreleme rolünü kimliğe atayın.

1. Anahtar kasanıza gidin.
1. **Erişim denetimi (IAM)**  >  **+**  >  **Rol Ekle ataması** Ekle ' yi seçin.
1. **Rol ataması Ekle** penceresinde:
    1. **Key Vault şifreleme hizmeti şifreleme (Önizleme)** rolü ' nü seçin. 
    1. **Kullanıcı tarafından atanan yönetilen kimliğe** erişim atayın.
    1. Kullanıcı tarafından atanan yönetilen kimliğinizin kaynak adını seçin ve **Kaydet**' i seçin.

### <a name="create-key-optional"></a>Anahtar oluştur (isteğe bağlı)

İsteğe bağlı olarak, kayıt defterini şifrelemek için kullanılacak anahtar kasasında bir anahtar oluşturun. Müşteri tarafından yönetilen anahtar olarak belirli bir anahtar sürümünü seçmek istiyorsanız bu adımları izleyin. 

1. Anahtar kasanıza gidin.
1. **Ayarlar**  >  **anahtarlar**' ı seçin.
1. **+ Oluştur/Içeri aktar** ' ı seçin ve anahtar için benzersiz bir ad girin.
1. Kalan varsayılan değerleri kabul edin ve **Oluştur**' u seçin.
1. Oluşturulduktan sonra anahtarı seçin ve ardından geçerli sürümü seçin. Anahtar sürümü için **anahtar tanımlayıcısı** ' nı kopyalayın.

### <a name="create-azure-container-registry"></a>Azure kapsayıcı kayıt defteri oluşturma

1. Container Registry **kaynak kapsayıcıları oluştur**' u seçin  >    >  .
1. **Temel bilgiler** sekmesinde, bir kaynak grubu seçin veya oluşturun ve bir kayıt defteri adı girin. **SKU**'da **Premium**' u seçin.
1. **Şifreleme** sekmesinde, **müşteri tarafından yönetilen anahtar**' ın **etkin**' i seçin.
1. **Kimlik**' te, oluşturduğunuz yönetilen kimliği seçin.
1. **Şifreleme** bölümünde aşağıdakilerden birini seçin:
    * **Key Vault Seç**' i seçin ve var olan bir anahtar kasası ve anahtarı seçin ya da **Yeni oluştur**' a tıklayın. Seçtiğiniz anahtar sürümlenmemiş ve otomatik anahtar döndürmeyi etkinleştirmesine izin vermez.
    * **Anahtar URI 'Si girin**' i seçin ve doğrudan bir anahtar tanımlayıcısı sağlayın. Sürümlü anahtar URI 'SI (el ile döndürülmesi gereken bir anahtar için) veya sürümü bulunmayan bir anahtar URI (otomatik anahtar dönüşü sağlayan) sağlayabilirsiniz. 
1. **Şifreleme** sekmesinde, **gözden geçir + oluştur**' u seçin.
1. Kayıt defteri örneğini dağıtmak için **Oluştur** ' u seçin.

:::image type="content" source="media/container-registry-customer-managed-keys/create-encrypted-registry.png" alt-text="Azure portal şifreli kayıt defteri oluşturma":::

Portalda kayıt defterinizin şifreleme durumunu görmek için Kayıt defterinize gidin. **Ayarlar** altında **şifreleme**' yi seçin.

## <a name="enable-customer-managed-key---template"></a>Müşteri tarafından yönetilen anahtar şablonunu etkinleştir

Ayrıca, bir kayıt defteri oluşturmak ve müşterinin yönettiği bir anahtarla şifrelemeyi etkinleştirmek için bir Kaynak Yöneticisi şablonu da kullanabilirsiniz.

Aşağıdaki şablon yeni bir kapsayıcı kayıt defteri ve Kullanıcı tarafından atanan yönetilen kimlik oluşturur. Aşağıdaki içerikleri yeni bir dosyaya kopyalayın ve gibi bir dosya adı kullanarak kaydedin `CMKtemplate.json` .

```JSON
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "vault_name": {
      "defaultValue": "",
      "type": "String"
    },
    "registry_name": {
      "defaultValue": "",
      "type": "String"
    },
    "identity_name": {
      "defaultValue": "",
      "type": "String"
    },
    "kek_id": {
      "type": "String"
    }
  },
  "variables": {},
  "resources": [
    {
      "type": "Microsoft.ContainerRegistry/registries",
      "apiVersion": "2019-12-01-preview",
      "name": "[parameters('registry_name')]",
      "location": "[resourceGroup().location]",
      "sku": {
        "name": "Premium",
        "tier": "Premium"
      },
      "identity": {
        "type": "UserAssigned",
        "userAssignedIdentities": {
          "[resourceID('Microsoft.ManagedIdentity/userAssignedIdentities', parameters('identity_name'))]": {}
        }
      },
      "dependsOn": [
        "[resourceId('Microsoft.ManagedIdentity/userAssignedIdentities', parameters('identity_name'))]"
      ],
      "properties": {
        "adminUserEnabled": false,
        "encryption": {
          "status": "enabled",
          "keyVaultProperties": {
            "identity": "[reference(resourceId('Microsoft.ManagedIdentity/userAssignedIdentities', parameters('identity_name')), '2018-11-30').clientId]",
            "KeyIdentifier": "[parameters('kek_id')]"
          }
        },
        "networkRuleSet": {
          "defaultAction": "Allow",
          "virtualNetworkRules": [],
          "ipRules": []
        },
        "policies": {
          "quarantinePolicy": {
            "status": "disabled"
          },
          "trustPolicy": {
            "type": "Notary",
            "status": "disabled"
          },
          "retentionPolicy": {
            "days": 7,
            "status": "disabled"
          }
        }
      }
    },
    {
      "type": "Microsoft.KeyVault/vaults/accessPolicies",
      "apiVersion": "2018-02-14",
      "name": "[concat(parameters('vault_name'), '/add')]",
      "dependsOn": [
        "[resourceId('Microsoft.ManagedIdentity/userAssignedIdentities', parameters('identity_name'))]"
      ],
      "properties": {
        "accessPolicies": [
          {
            "tenantId": "[subscription().tenantId]",
            "objectId": "[reference(resourceId('Microsoft.ManagedIdentity/userAssignedIdentities', parameters('identity_name')), '2018-11-30').principalId]",
            "permissions": {
              "keys": [
                "get",
                "unwrapKey",
                "wrapKey"
              ]
            }
          }
        ]
      }
    },
    {
      "type": "Microsoft.ManagedIdentity/userAssignedIdentities",
      "apiVersion": "2018-11-30",
      "name": "[parameters('identity_name')]",
      "location": "[resourceGroup().location]"
    }
  ]
}
```

Aşağıdaki kaynakları oluşturmak için önceki bölümlerdeki adımları izleyin:

* Anahtar Kasası, ada göre tanımlanır
* Anahtar KIMLIĞI ile tanımlanan anahtar kasası anahtarı

Önceki şablon dosyasını kullanarak kayıt defterini oluşturmak için aşağıdaki [az Deployment Group Create][az-deployment-group-create] komutunu çalıştırın. Burada gösterildiği gibi, yeni bir kayıt defteri adı ve yönetilen kimlik adı ve oluşturduğunuz Anahtar Kasası adı ile anahtar KIMLIĞI belirtin.

```bash
az deployment group create \
  --resource-group <resource-group-name> \
  --template-file CMKtemplate.json \
  --parameters \
    registry_name=<registry-name> \
    identity_name=<managed-identity> \
    vault_name=<key-vault-name> \
    kek_id=<key-vault-key-id>
```

### <a name="show-encryption-status"></a>Şifreleme durumunu göster

Kayıt defteri şifrelemenin durumunu göstermek için [az ACR ENCRYPTION Show][az-acr-encryption-show] komutunu çalıştırın:

```azurecli
az acr encryption show --name <registry-name>
```

## <a name="use-the-registry"></a>Kayıt defterini kullanma

Bir kayıt defterinde müşteri tarafından yönetilen anahtarı etkinleştirdikten sonra, müşteri tarafından yönetilen bir anahtarla şifrelenmeyen bir kayıt defterinde gerçekleştirdiğiniz kayıt defteri işlemlerini gerçekleştirebilirsiniz. Örneğin, kayıt defteri ile kimlik doğrulaması yapabilir ve Docker görüntülerini gönderebilirsiniz. Bkz. [gönderim ve görüntü çekme](container-registry-get-started-docker-cli.md)içinde örnek komutlar.

## <a name="rotate-key"></a>Anahtarı döndür

Azure Key Vault temel sürümü güncelleştirin veya yeni bir anahtar oluşturun ve anahtarı kullanarak verileri şifrelemek için kayıt defterini güncelleştirin. Bu adımları Azure CLı 'yı kullanarak veya portalda gerçekleştirebilirsiniz.

Bir anahtar döndürürken, genellikle kayıt defteri oluştururken kullanılan kimliği belirtirsiniz. İsteğe bağlı olarak, anahtar erişimi için Kullanıcı tarafından atanan yeni bir kimlik yapılandırın veya etkinleştirin ve kayıt defterinin sistem tarafından atanan kimliğini belirleyin.

> [!NOTE]
> Anahtar erişimi için yapılandırdığınız kimlik için gereken [Anahtar Kasası erişiminin](#enable-key-vault-access) ayarlandığından emin olun.

### <a name="update-key-version"></a>Anahtar sürümünü Güncelleştir

Yaygın bir senaryo, müşteri tarafından yönetilen anahtar olarak kullanılan anahtarın sürümünü güncelleştirmedir. Kayıt defteri şifrelemesinin nasıl yapılandırıldığına bağlı olarak, Azure Container Registry içindeki müşteri tarafından yönetilen anahtar otomatik olarak güncelleştirilir veya el ile güncelleştirilmeleri gerekir.

### <a name="azure-cli"></a>Azure CLI’si

Anahtar Kasası Anahtarlarınızı oluşturmak veya yönetmek için [az keykasakey][az-keyvault-key] komutları kullanın. Yeni bir anahtar sürümü oluşturmak için [az keykasa Key Create][az-keyvault-key-create] komutunu çalıştırın:

```azurecli
# Create new version of existing key
az keyvault key create \
  –-name <key-name> \
  --vault-name <key-vault-name>
```

Sonraki adım, kayıt defteri şifrelemesinin nasıl yapılandırıldığına bağlıdır:

* Kayıt defteri, anahtar sürüm güncelleştirmelerini algılayacak şekilde yapılandırıldıysa, müşteri tarafından yönetilen anahtar, 1 saat içinde otomatik olarak güncelleştirilir.

* Kayıt defteri yeni bir anahtar sürümü için el ile güncelleştirme gerektirecek şekilde yapılandırıldıysa, yeni anahtar KIMLIĞINI ve yapılandırmak istediğiniz kimliği geçirerek [az ACR Encryption döndürme anahtarı][az-acr-encryption-rotate-key] komutunu çalıştırın:

Müşteri tarafından yönetilen anahtar sürümünü el ile güncelleştirmek için:

```azurecli
# Rotate key and use user-assigned identity
az acr encryption rotate-key \
  --name <registry-name> \
  --key-encryption-key <new-key-id> \
  --identity <principal-id-user-assigned-identity>

# Rotate key and use system-assigned identity
az acr encryption rotate-key \
  --name <registry-name> \
  --key-encryption-key <new-key-id> \
  --identity [system]
```

> [!TIP]
> Çalıştırdığınızda `az acr encryption rotate-key` , sürümlü anahtar kimliği veya sürümlenmemiş anahtar kimliği geçirebilirsiniz. Sürümü bulunmayan bir anahtar KIMLIĞI kullanırsanız, kayıt defteri daha sonra sonraki anahtar sürüm güncelleştirmelerini otomatik olarak algılayacak şekilde yapılandırılır.

### <a name="portal"></a>Portal

Müşterinin yönettiği anahtar için kullanılan Anahtar Kasası, anahtar veya kimlik ayarlarını güncelleştirmek için kayıt defterinin **şifreleme** ayarlarını kullanın.

Örneğin, yeni bir anahtar yapılandırmak için:

1. Portalda Kayıt defterinize gidin.
1. **Ayarlar** altında **şifreleme**  >  **değişiklik anahtarı**' nı seçin.

    :::image type="content" source="media/container-registry-customer-managed-keys/rotate-key.png" alt-text="Azure portal anahtarı döndür":::
1. **Şifreleme** bölümünde aşağıdakilerden birini seçin:
    * **Key Vault Seç**' i seçin ve var olan bir anahtar kasası ve anahtarı seçin ya da **Yeni oluştur**' a tıklayın. Seçtiğiniz anahtar sürümlenmemiş ve otomatik anahtar döndürmeyi etkinleştirmesine izin vermez.
    * **Anahtar URI 'Si girin**' i seçin ve doğrudan bir anahtar tanımlayıcısı sağlayın. Sürümlü anahtar URI 'SI (el ile döndürülmesi gereken bir anahtar için) veya sürümü bulunmayan bir anahtar URI (otomatik anahtar dönüşü sağlayan) sağlayabilirsiniz.
1. Anahtar seçimini tamamlayıp **Kaydet**' i seçin.

## <a name="revoke-key"></a>Anahtarı iptal et

Anahtar kasasındaki erişim ilkesini veya izinleri değiştirerek veya anahtarı silerek müşteri tarafından yönetilen şifreleme anahtarını iptal edin. Örneğin, kayıt defteriniz tarafından kullanılan yönetilen kimliğin erişim ilkesini değiştirmek için [az keykasadelete-Policy][az-keyvault-delete-policy] komutunu kullanın:

```azurecli
az keyvault delete-policy \
  --resource-group <resource-group-name> \
  --name <key-vault-name> \
  --object-id $identityPrincipalID
```

Kayıt defteri şifreleme anahtarına erişebileceğinden anahtar, tüm kayıt defteri verilerine erişimi etkin bir şekilde iptal etme. Anahtara erişim etkinleştirilirse veya silinen anahtar geri yüklenirse, şifrelenmiş kayıt defteri verilerine yeniden erişebilmek için kayıt defteriniz anahtarı seçer.

## <a name="advanced-scenario-key-vault-firewall"></a>Gelişmiş senaryo: Key Vault güvenlik duvarı

Azure anahtar kasanızın bir Key Vault güvenlik duvarı olan bir sanal ağda dağıtılması durumunda, kayıt defterinizde müşteri tarafından yönetilen anahtar şifrelemesini etkinleştirdikten sonra aşağıdaki ek adımları gerçekleştirin.

1. Kayıt defteri şifrelemesini, kayıt defterinin sistem tarafından atanan kimliğini kullanacak şekilde yapılandırma
1. Key Vault güvenlik duvarını atlamak için kayıt defterini etkinleştirin
1. Müşteri tarafından yönetilen anahtarı döndürme

### <a name="configure-system-assigned-identity"></a>Sistem tarafından atanan kimliği yapılandırma

Şifreleme anahtarları için anahtar kasasına erişmek üzere bir kayıt defterinin sistem tarafından atanan yönetilen kimliğini yapılandırabilirsiniz. Azure kaynakları için farklı yönetilen kimliklere alışkın değilseniz bkz. [genel bakış](../active-directory/managed-identities-azure-resources/overview.md).

Kayıt defterinin sistem tarafından atanan kimliğini portalda etkinleştirmek için:

1. Portalda Kayıt defterinize gidin.
1. **Ayar**  >   **kimliğini** seçin.
1. **Sistem atandı** altında **durumu** **Açık** olarak ayarlayın. **Kaydet**’i seçin.
1. Kimliğin **nesne kimliğini** kopyalayın.

Anahtar kasanıza kimlik erişimi vermek için:

1. Anahtar kasanıza gidin.
1. **Ayarlar**  >  **erişim ilkeleri > + erişim ilkesi Ekle**' yi seçin.
1. **Anahtar izinleri**' ni seçin ve **Al**, **anahtar kaydırmayı kaldır** ve **anahtar sarmalama**' i seçin.
1. **Sorumlu Seç** ' i seçin ve sistem tarafından atanan yönetilen KIMLIĞINIZIN nesne kimliğini veya kayıt defterinizin adını arayın.  
1. **Ekle**' yi ve ardından **Kaydet**' i seçin.

Kayıt defterinin şifreleme ayarlarını kimliği kullanacak şekilde güncelleştirmek için:

1. Portalda Kayıt defterinize gidin.
1. **Ayarlar** altında **şifreleme**  >  **değişiklik anahtarı**' nı seçin.
1. **Kimlik**' te, **sistem atandı**' ı seçin ve **Kaydet**' i seçin.

### <a name="enable-key-vault-bypass"></a>Anahtar Kasası atlamayı etkinleştir

Key Vault güvenlik duvarıyla yapılandırılmış bir anahtar kasasına erişmek için, kayıt defterinin güvenlik duvarını atlaması gerekir. Anahtar kasasının herhangi bir [güvenilir hizmet](../key-vault/general/overview-vnet-service-endpoints.md#trusted-services)tarafından erişime izin verecek şekilde yapılandırıldığından emin olun. Azure Container Registry, güvenilen hizmetlerden biridir.

1. Portalda anahtar kasanıza gidin.
1. **Ayarlar**  >  **ağ**' ı seçin.
1. Sanal ağ ayarlarını onaylayın, güncelleştirin veya ekleyin. Ayrıntılı adımlar için bkz. [Azure Key Vault güvenlik duvarlarını ve sanal ağları yapılandırma](../key-vault/general/network-security.md).
1. **Microsoft güvenilir hizmetlerinin bu güvenlik duvarını atlamasına Izin ver**' de, **Evet**' i seçin. 

### <a name="rotate-the-customer-managed-key"></a>Müşteri tarafından yönetilen anahtarı döndürme

Yukarıdaki adımları tamamladıktan sonra anahtarı güvenlik duvarının arkasındaki anahtar kasasında yeni bir anahtara döndürün. Adımlar için, bu makaledeki [anahtarı Döndür](#rotate-key) bölümüne bakın.

## <a name="troubleshoot"></a>Sorun giderme

### <a name="removing-managed-identity"></a>Yönetilen kimlik kaldırılıyor


Şifrelemeyi yapılandırmak için kullanılan bir kayıt defterinden Kullanıcı tarafından atanan veya sistem tarafından atanan bir yönetilen kimliği kaldırmaya çalışırsanız şuna benzer bir hata iletisi görebilirsiniz:
 
```
Azure resource '/subscriptions/xxxx/resourcegroups/myGroup/providers/Microsoft.ContainerRegistry/registries/myRegistry' does not have access to identity 'xxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx' Try forcibly adding the identity to the registry <registry name>. For more information on bring your own key, please visit 'https://aka.ms/acr/cmk'.
```
 
Şifreleme anahtarını da değiştiremeyecektir (döndüremezsiniz). Çözümleme adımları, şifreleme için kullanılan kimlik türüne bağlıdır.

**Kullanıcı tarafından atanan kimlik**

Bu sorun Kullanıcı tarafından atanan bir kimlikle oluşursa, önce hata iletisinde görünen GUID 'yi kullanarak kimliği yeniden atayın. Örneğin:

```azurecli
az acr identity assign -n myRegistry --identities xxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx
```
        
Ardından, anahtarı değiştirdikten ve farklı bir kimlik atadıktan sonra, Kullanıcı tarafından atanan özgün kimliği kaldırabilirsiniz.

**Sistem tarafından atanan kimlik**

Bu sorun sistem tarafından atanan bir kimlikle oluşursa, kimlik geri yükleme konusunda yardım için lütfen [bir Azure destek bileti oluşturun](https://azure.microsoft.com/support/create-ticket/) .


## <a name="next-steps"></a>Sonraki adımlar

* [Azure 'da bekleyen şifreleme](../security/fundamentals/encryption-atrest.md)hakkında daha fazla bilgi edinin.
* Erişim ilkeleri hakkında daha fazla bilgi edinin ve [bir anahtar kasasına erişimi güvenli hale](../key-vault/general/secure-your-key-vault.md)getirin.


<!-- LINKS - external -->

<!-- LINKS - internal -->

[az-feature-register]: /cli/azure/feature#az-feature-register
[az-feature-show]: /cli/azure/feature#az-feature-show
[az-group-create]: /cli/azure/group#az-group-create
[az-identity-create]: /cli/azure/identity#az-identity-create
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-deployment-group-create]: /cli/azure/deployment/group#az-deployment-group-create
[az-keyvault-create]: /cli/azure/keyvault#az-keyvault-create
[az-keyvault-key-create]: /cli/azure/keyvault/key#az-keyvault-key-create
[az-keyvault-key]: /cli/azure/keyvault/key
[az-keyvault-set-policy]: /cli/azure/keyvault#az-keyvault-set-policy
[az-keyvault-delete-policy]: /cli/azure/keyvault#az-keyvault-delete-policy
[az-resource-show]: /cli/azure/resource#az-resource-show
[az-acr-create]: /cli/azure/acr#az-acr-create
[az-acr-show]: /cli/azure/acr#az-acr-show
[az-acr-encryption-rotate-key]: /cli/azure/acr/encryption#az-acr-encryption-rotate-key
[az-acr-encryption-show]: /cli/azure/acr/encryption#az-acr-encryption-show
