---
title: Müşteri tarafından yönetilen bir anahtarla bekleyen şifreleme
description: Azure Container Registry 'nizin geri kalanı hakkında bilgi edinin ve Premium kayıt defterinizi, Azure Key Vault depolanan müşteri tarafından yönetilen bir anahtarla nasıl şifreleyeceğinizi öğrenin
ms.topic: article
ms.date: 05/01/2020
ms.custom: ''
ms.openlocfilehash: 393e51e687e95c1ff4c6a50429dd342005aad296
ms.sourcegitcommit: 5504d5a88896c692303b9c676a7d2860f36394c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84509551"
---
# <a name="encrypt-registry-using-a-customer-managed-key"></a>Müşteri tarafından yönetilen anahtar kullanarak kayıt defterini şifreleme

Azure Container Registry 'de görüntüleri ve diğer yapıtları depoladığınız zaman, Azure, [hizmet tarafından yönetilen anahtarlarla](../security/fundamentals/encryption-atrest.md#data-encryption-models)geri kalan kayıt defteri içeriğini otomatik olarak şifreler. Azure Key Vault içinde oluşturduğunuz ve yönettiğiniz bir anahtarı kullanarak ek şifreleme katmanıyla varsayılan şifrelemeyi destekleyebilirsiniz. Bu makalede, Azure CLı ve Azure portal arasındaki adımlarda adım adım açıklanmaktadır.

Müşteri tarafından yönetilen anahtarlarla sunucu tarafında şifreleme, [Azure Key Vault](../key-vault/general/overview.md)tümleştirme aracılığıyla desteklenir. Kendi şifreleme anahtarlarınızı oluşturabilir ve bunları bir anahtar kasasında saklayabilir veya anahtarlar oluşturmak için Azure Key Vault API 'Lerini kullanabilirsiniz. Azure Key Vault, anahtar kullanımını da denetleyebilirsiniz.

Bu özellik **Premium** kapsayıcı kayıt defteri hizmet katmanında kullanılabilir. Kayıt defteri hizmeti katmanları ve limitleri hakkında bilgi için bkz. [Azure Container Registry hizmet katmanları](container-registry-skus.md).

   
## <a name="things-to-know"></a>Bilinmesi gerekenler

* Şu anda yalnızca bir kayıt defteri oluşturduğunuzda müşterinin yönettiği bir anahtarı etkinleştirebilirsiniz.
* Bir kayıt defterinde müşteri tarafından yönetilen anahtarı etkinleştirdikten sonra devre dışı bırakabilirsiniz.
* [İçerik güveni](container-registry-content-trust.md) , müşteri tarafından yönetilen bir anahtarla şifrelenen bir kayıt defterinde şu anda desteklenmiyor.
* Müşteri tarafından yönetilen bir anahtarla şifrelenen bir kayıt defterinde, [ACR görevleri](container-registry-tasks-overview.md) için çalıştırılan Günlükler Şu anda yalnızca 24 saat boyunca saklanır. Daha uzun bir süre için günlükleri tutmanız gerekiyorsa, bkz. [görev çalıştırma günlüklerini dışarı ve depolama](container-registry-tasks-logs.md#alternative-log-storage)Kılavuzu.

## <a name="prerequisites"></a>Ön koşullar

Bu makaledeki Azure CLı adımlarını kullanmak için Azure CLı sürüm 2.2.0 veya sonraki bir sürümü gerekir. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme](/cli/azure/install-azure-cli).

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

### <a name="create-a-key-vault"></a>Bir anahtar kasası oluşturma

Kayıt defteri şifrelemesi için müşteri tarafından yönetilen bir anahtar depolamak üzere [az keykasacreate][az-keyvault-create] ile bir Anahtar Kasası oluşturun. 

Yanlışlıkla anahtar veya Anahtar Kasası silme işlemlerinin neden olduğu veri kaybını engellemek için şu ayarları etkinleştirmelisiniz: **geçici silme** ve **Temizleme koruması**. Aşağıdaki örnek bu ayarların parametrelerini içerir: 

```azurecli
az keyvault create --name <key-vault-name> \
  --resource-group <resource-group-name> \
  --enable-soft-delete \
  --enable-purge-protection
```

### <a name="add-key-vault-access-policy"></a>Anahtar Kasası erişim ilkesi ekleme

Kimliğin erişebilmesi için Anahtar Kasası için bir ilke yapılandırın. Aşağıdaki [az keykasası Set-Policy][az-keyvault-set-policy] komutunda, oluşturduğunuz yönetilen KIMLIğIN asıl kimliğini daha önce bir ortam değişkeninde depoladığınız şekilde geçirirsiniz. **Get**, **unwrapKey**ve **wrapKey**için anahtar izinleri ayarlayın.  

```azurecli
az keyvault set-policy \
  --resource-group <resource-group-name> \
  --name <key-vault-name> \
  --object-id $identityPrincipalID \
  --key-permissions get unwrapKey wrapKey 
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
    "kid": "https://mykeyvault.vault.azure.net/keys/mykey/xxxxxxxxxxxxxxxxxxxxxxxx",
    "kty": "RSA",
[...]
```
Kolaylık olması için bu değeri bir ortam değişkeninde saklayın:

```azurecli
keyID=$(az keyvault key show \
  --name <keyname> \
  --vault-name <key-vault-name> \
  --query 'key.kid' --output tsv)
```

### <a name="create-a-registry-with-customer-managed-key"></a>Müşteri tarafından yönetilen anahtarla kayıt defteri oluşturma

Premium hizmet katmanında bir kayıt defteri oluşturmak ve müşterinin yönettiği anahtarı etkinleştirmek için [az ACR Create][az-acr-create] komutunu çalıştırın. Daha önce ortam değişkenlerinde depolanan yönetilen kimlik asıl KIMLIĞINI ve anahtar KIMLIĞINI geçirin:

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

Çıkış şuna benzer olacaktır:

```console
{
  "keyVaultProperties": {
    "identity": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "keyIdentifier": "https://myvault.vault.azure.net/keys/myresourcegroup/abcdefg123456789...",
    "versionedKeyIdentifier": "https://myvault.vault.azure.net/keys/myresourcegroup/abcdefg123456789..."
  },
  "status": "enabled"
}
```

## <a name="enable-customer-managed-key---portal"></a>Müşteri tarafından yönetilen anahtarı etkinleştirme-Portal

### <a name="create-a-managed-identity"></a>Yönetilen kimlik oluşturma

Azure portal [Azure kaynakları için](../active-directory/managed-identities-azure-resources/overview.md) Kullanıcı tarafından atanan bir yönetilen kimlik oluşturun. Adımlar için bkz. [Kullanıcı tarafından atanan kimlik oluşturma](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md#create-a-user-assigned-managed-identity).

Daha sonraki adımlarda kimliğin adını kullanırsınız.

![Azure portal Kullanıcı tarafından atanan yönetilen kimlik oluşturma](./media/container-registry-customer-managed-keys/create-managed-identity.png)

### <a name="create-a-key-vault"></a>Bir anahtar kasası oluşturma

Bir Anahtar Kasası oluşturma adımları için bkz. [hızlı başlangıç: Azure Key Vault Azure Portal kullanarak bir gizli dizi ayarlama ve alma](../key-vault/secrets/quick-create-portal.md).

Müşteri tarafından yönetilen anahtar için bir Anahtar Kasası oluştururken, **temel bilgiler** sekmesinde, Şu koruma ayarlarını etkinleştirin: **geçici silme** ve **Temizleme koruması**. Bu ayarlar, yanlışlıkla anahtar veya Anahtar Kasası silmeleri nedeniyle oluşan veri kaybını önlemeye yardımcı olur.

![Azure portal Anahtar Kasası oluşturma](./media/container-registry-customer-managed-keys/create-key-vault.png)

### <a name="add-key-vault-access-policy"></a>Anahtar Kasası erişim ilkesi ekleme

Kimliğin erişebilmesi için Anahtar Kasası için bir ilke yapılandırın.

1. Anahtar kasanıza gidin.
1. **Ayarlar**  >  **erişim ilkeleri > + erişim ilkesi Ekle**' yi seçin.
1. **Anahtar izinleri**' ni seçin ve **Al**, **anahtar kaydırmayı kaldır**ve **anahtar sarmalama**' i seçin.
1. **Sorumlu Seç** ' i seçin ve Kullanıcı tarafından atanan yönetilen kimliğinizin kaynak adını seçin.  
1. **Ekle**' yi ve ardından **Kaydet**' i seçin.

![Anahtar Kasası erişim ilkesi oluşturma](./media/container-registry-customer-managed-keys/add-key-vault-access-policy.png)

### <a name="create-key"></a>Anahtar oluştur

1. Anahtar kasanıza gidin.
1. **Ayarlar**  >  **anahtarlar**' ı seçin.
1. **+ Oluştur/Içeri aktar** ' ı seçin ve anahtar için benzersiz bir ad girin.
1. Kalan varsayılan değerleri kabul edin ve **Oluştur**' u seçin.
1. Oluşturulduktan sonra anahtarı seçin ve geçerli anahtar sürümüne göz atın.

### <a name="create-azure-container-registry"></a>Azure kapsayıcı kayıt defteri oluşturma

1. Container Registry **kaynak kapsayıcıları oluştur**' u seçin  >  **Containers**  >  **Container Registry**.
1. **Temel bilgiler** sekmesinde, bir kaynak grubu seçin veya oluşturun ve bir kayıt defteri adı girin. **SKU**'da **Premium**' u seçin.
1. **Şifreleme** sekmesinde, **müşteri tarafından yönetilen anahtar**' ın **etkin**' i seçin.
1. **Kimlik**' te, oluşturduğunuz yönetilen kimliği seçin.
1. **Şifreleme**' de **Key Vault seçin**' i seçin.
1. **Azure Key Vault anahtar seçin** penceresinde, önceki bölümde oluşturduğunuz Anahtar Kasası, anahtar ve sürümü seçin.
1. **Şifreleme** sekmesinde, **gözden geçir + oluştur**' u seçin.
1. Kayıt defteri örneğini dağıtmak için **Oluştur** ' u seçin.

![Azure portal kapsayıcı kayıt defteri oluşturma](./media/container-registry-customer-managed-keys/create-encrypted-registry.png)

Portalda kayıt defterinizin şifreleme durumunu görmek için Kayıt defterinize gidin. **Ayarlar**altında **şifreleme**' yi seçin.

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

Önceki şablon dosyasını kullanarak kayıt defterini oluşturmak için aşağıdaki [az Group Deployment Create][az-group-deployment-create] komutunu çalıştırın. Burada gösterildiği gibi, yeni bir kayıt defteri adı ve yönetilen kimlik adı ve oluşturduğunuz Anahtar Kasası adı ile anahtar KIMLIĞI belirtin. 

```bash
az group deployment create \
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

Kayıt defteri şifrelemesi için kullanılan müşteri tarafından yönetilen bir anahtarı uyumluluk ilkelerinize döndürün. Yeni bir anahtar oluşturun veya anahtar sürümünü güncelleştirin ve anahtarı kullanarak verileri şifrelemek için kayıt defterini güncelleştirin. Bu adımları Azure CLı 'yı kullanarak veya portalda gerçekleştirebilirsiniz.

Bir anahtar döndürürken, genellikle kayıt defteri oluştururken kullanılan kimliği belirtirsiniz. İsteğe bağlı olarak, anahtar erişimi için Kullanıcı tarafından atanan yeni bir kimlik yapılandırın veya etkinleştirin ve kayıt defterinin sistem tarafından atanan kimliğini belirleyin.

> [!NOTE]
> Anahtar erişimi için yapılandırdığınız kimlik için gereken [Anahtar Kasası erişim ilkesinin](#add-key-vault-access-policy) ayarlanmış olduğundan emin olun. 

### <a name="azure-cli"></a>Azure CLI

Anahtar Kasası Anahtarlarınızı oluşturmak veya yönetmek için [az keykasakey][az-keyvault-key] komutları kullanın. Örneğin, yeni bir anahtar sürümü veya anahtar oluşturmak için [az keykasa Key Create][az-keyvault-key-create] komutunu çalıştırın:

```azurecli
# Create new version of existing key
az keyvault key create \
  –-name <key-name> \
  --vault-name <key-vault-name> 

# Create new key
az keyvault key create \
  –-name <new-key-name> \
  --vault-name <key-vault-name> 
```

Ardından, yeni anahtar KIMLIĞINI ve yapılandırmak istediğiniz kimliği geçirerek [az ACR Encryption döndürme-Key][az-acr-encryption-rotate-key] komutunu çalıştırın:

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

### <a name="portal"></a>Portal

Anahtar sürümünü, anahtarı, anahtar kasasını veya müşteri tarafından yönetilen anahtar için kullanılan kimlik ayarlarını güncelleştirmek için kayıt defterinin **şifreleme** ayarlarını kullanın. 

Örneğin, yeni bir anahtar sürümü oluşturmak ve yapılandırmak için:

1. Portalda Kayıt defterinize gidin. 
1. **Ayarlar**altında **şifreleme**  >  **değişiklik anahtarı**' nı seçin.
1. **Anahtar Seç** ' i seçin
    
    ![Azure portal anahtarı döndür](./media/container-registry-customer-managed-keys/rotate-key.png)
1. **Azure Key Vault anahtar seçin** penceresinde, daha önce yapılandırdığınız anahtar kasasını ve anahtarı seçin ve **sürümde** **Yeni oluştur**' u seçin.
1. **Anahtar oluştur** **penceresinde Oluştur '** u ve ardından **Oluştur**' u seçin.
1. Anahtar seçimini tamamlayıp **Kaydet**' i seçin.

## <a name="revoke-key"></a>Anahtarı iptal et

Anahtar kasasındaki erişim ilkesini değiştirerek veya anahtarı silerek müşteri tarafından yönetilen şifreleme anahtarını iptal edin. Örneğin, kayıt defteriniz tarafından kullanılan yönetilen kimliğin erişim ilkesini değiştirmek için [az keykasadelete-Policy][az-keyvault-delete-policy] komutunu kullanın:

```azurecli
az keyvault delete-policy \
  --resource-group <resource-group-name> \
  --name <key-vault-name> \
  --object-id $identityPrincipalID
```

Kayıt defteri şifreleme anahtarına erişebileceğinden anahtar, tüm kayıt defteri verilerine erişimi etkin bir şekilde iptal etme. Anahtara erişim etkinleştirilirse veya silinen anahtar geri yüklenirse, şifrelenmiş kayıt defteri verilerine yeniden erişebilmek için kayıt defteriniz anahtarı seçer.

## <a name="advanced-scenarios"></a>Gelişmiş senaryolar

### <a name="system-assigned-identity"></a>Sistem tarafından atanan kimlik

Şifreleme anahtarları için anahtar kasasına erişmek üzere bir kayıt defterinin sistem tarafından atanan yönetilen kimliğini yapılandırabilirsiniz. Azure kaynakları için farklı yönetilen kimliklere alışkın değilseniz bkz. [genel bakış](../active-directory/managed-identities-azure-resources/overview.md).

Kayıt defterinin sistem tarafından atanan kimliğini portalda etkinleştirmek için:

1. Portalda Kayıt defterinize gidin. 
1. **Ayar**  >   **kimliğini**seçin.
1. **Sistem atandı**altında **durumu** **Açık**olarak ayarlayın. **Kaydet**'i seçin.
1. Kimliğin **nesne kimliğini** kopyalayın.

Anahtar kasanıza kimlik erişimi vermek için:

1. Anahtar kasanıza gidin.
1. **Ayarlar**  >  **erişim ilkeleri > + erişim ilkesi Ekle**' yi seçin.
1. **Anahtar izinleri**' ni seçin ve **Al**, **anahtar kaydırmayı kaldır**ve **anahtar sarmalama**' i seçin.
1. **Sorumlu Seç** ' i seçin ve sistem tarafından atanan yönetilen KIMLIĞINIZIN nesne kimliğini veya kayıt defterinizin adını arayın.  
1. **Ekle**' yi ve ardından **Kaydet**' i seçin.

Kayıt defterinin şifreleme ayarlarını kimliği kullanacak şekilde güncelleştirmek için:

1. Portalda Kayıt defterinize gidin. 
1. **Ayarlar**altında **şifreleme**  >  **değişiklik anahtarı**' nı seçin.
1. **Kimlik**' te, **sistem atandı**' ı seçin ve **Kaydet**' i seçin.

### <a name="key-vault-firewall"></a>Key Vault güvenlik duvarı

Azure anahtar kasanızın bir Key Vault güvenlik duvarı olan bir sanal ağda dağıtılması halinde aşağıdaki adımları uygulayın:

1. Kayıt defteri şifrelemesini, kayıt defterinin sistem tarafından atanan kimliğini kullanacak şekilde yapılandırın. Yukarıdaki bölüme bakın.
2. Anahtar kasasını, herhangi bir [güvenilir hizmet](../key-vault/general/overview-vnet-service-endpoints.md#trusted-services)tarafından erişime izin verecek şekilde yapılandırın. 

Ayrıntılı adımlar için bkz. [Azure Key Vault güvenlik duvarlarını ve sanal ağları yapılandırma](../key-vault/general/network-security.md). 

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
[az-group-deployment-create]: /cli/azure/group/deployment#az-group-deployment-create
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
