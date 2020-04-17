---
title: Müşteri tarafından yönetilen anahtarlarla şifreleme-at-rest
description: Azure kapsayıcı kayıt defterinizin geri kalanında şifreleme ve Azure Anahtar Kasası'nda depolanan müşteri tarafından yönetilen bir anahtarla kayıt defterinizi nasıl şifreleriniz hakkında bilgi edinin
ms.topic: article
ms.date: 03/10/2020
ms.custom: ''
ms.openlocfilehash: 2d5561998cf0b19698c8059a861a4014a171a7e7
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81461761"
---
# <a name="encryption-using-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarları kullanarak şifreleme

Görüntüleri ve diğer yapıları bir Azure kapsayıcı kayıt defterinde depoladiğinizde, Azure kayıt defteri içeriğini [hizmet tarafından yönetilen anahtarlarla](../security/fundamentals/encryption-atrest.md#data-encryption-models)otomatik olarak şifreler. Azure Key Vault'ta oluşturduğunuz ve yönettiğiniz bir anahtarı kullanarak varsayılan şifrelemeyi ek bir şifreleme katmanı ile tamamlayabilirsiniz. Bu makale, Azure CLI ve Azure portalını kullanarak adımları niçin gözden geçirmenizi kolaylaştırır.

Müşteri tarafından yönetilen anahtarlarla sunucu tarafı şifreleme, [Azure Key Vault](../key-vault/general/overview.md)ile tümleştirme yoluyla desteklenir. Kendi şifreleme anahtarlarınızı oluşturabilir ve bunları bir anahtar kasasında saklayabilir veya şifreleme anahtarları oluşturmak için Azure Key Vault'un API'lerini kullanabilirsiniz. Azure Key Vault ile anahtar kullanımını da denetleyebilirsiniz.

Bu özellik **Premium** konteyner kayıt hizmeti katmanında kullanılabilir. Kayıt defteri hizmeti katmanları ve sınırları hakkında daha fazla bilgi için [Azure Konteyner Kayıt Defteri SK'leri'ne](container-registry-skus.md)bakın.

> [!IMPORTANT]
> Bu özellik şu anda önizlemededir ve bazı [sınırlamalar](#preview-limitations) geçerlidir. Önizlemeler, [ek kullanım koşullarını][terms-of-use] kabul etmeniz şartıyla kullanımınıza sunulur. Bu özelliğin bazı yönleri genel kullanıma açılmadan önce değişebilir.
>
   
## <a name="preview-limitations"></a>Önizleme sınırlamaları 

* Şu anda bu özelliği yalnızca bir kayıt defteri oluşturduğunuzda etkinleştirebilirsiniz.
* Bir kayıt defterinde müşteri tarafından yönetilen bir anahtarı etkinleştirdikten sonra, anahtarı devre dışı kalamazsınız.
* [İçerik güveni](container-registry-content-trust.md) şu anda müşteri tarafından yönetilen bir anahtarla şifrelenmiş bir kayıt defterinde desteklenmez.
* Müşteri tarafından yönetilen bir anahtarla şifrelenmiş bir kayıt defterinde, [ACR Görevleri](container-registry-tasks-overview.md) için çalıştır günlükleri şu anda yalnızca 24 saat saklanır. Günlükleri daha uzun bir süre tutmanız gerekiyorsa, [görev çalıştırma günlüklerini dışa aktarma ve depolama](container-registry-tasks-logs.md#alternative-log-storage)kılavuzuna bakın.

## <a name="prerequisites"></a>Ön koşullar

Bu makalede Azure CLI adımlarını kullanmak için Azure CLI sürüm 2.2.0 veya sonrası gerekir. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme](/cli/azure/install-azure-cli).

## <a name="enable-customer-managed-key---cli"></a>Müşteri tarafından yönetilen anahtarı etkinleştirme - CLI

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Gerekirse, anahtar tonoz, konteyner kayıt defteri ve diğer gerekli kaynakları oluşturmak için bir kaynak grubu oluşturmak için [az grubu oluşturma][az-group-create] komutunu çalıştırın.

```azurecli
az group create --name <resource-group-name> --location <location>
```

### <a name="create-a-user-assigned-managed-identity"></a>Kullanıcı tarafından atanan yönetilen kimlik oluşturma

Az kimlik oluşturma komutuyla [Azure kaynakları için](../active-directory/managed-identities-azure-resources/overview.md) kullanıcı tarafından atanmış yönetilen bir [kimlik oluşturun.][az-identity-create] Bu kimlik, Kayıt defteriniz tarafından Key Vault hizmetine erişmek için kullanılacaktır.

```azurecli
az identity create \
  --resource-group <resource-group-name> \
  --name <managed-identity-name> 
```

Komut çıkışında aşağıdaki değerlere dikkat `id` edin: ve `principalId`. Kayıt defteri erişimini anahtar kasasına yapılandırmak için sonraki adımlarda bu değerlere ihtiyacınız vardır.

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

Kolaylık sağlamak için, bu değerleri ortam değişkenlerinde saklayın:

```azurecli
identityID=$(az identity show --resource-group <resource-group-name> --name <managed-identity-name> --query 'id' --output tsv)

identityPrincipalID=$(az identity show --resource-group <resource-group-name> --name <managed-identity-name> --query 'principalId' --output tsv)
```

### <a name="create-a-key-vault"></a>Bir anahtar kasası oluşturma

Kayıt defteri şifrelemesi için müşteri tarafından yönetilen bir anahtarı depolamak için [az keyvault oluşturduğu bir anahtar][az-keyvault-create] kasası oluşturun. 

Yanlışlıkla anahtar veya anahtar silme neden veri kaybını önlemek için, aşağıdaki ayarları etkinleştirmeniz gerekir: **Yumuşak silme** ve **Temizleme koruması.** Aşağıdaki örnekte bu ayarlar için parametreler yer almaktadır: 

```azurecli
az keyvault create --name <key-vault-name> \
  --resource-group <resource-group-name> \
  --enable-soft-delete \
  --enable-purge-protection
```

### <a name="add-key-vault-access-policy"></a>Anahtar kasa erişim ilkesi ekleme

Kimlik erişebilsin diye anahtar kasası için bir ilke yapılandırın. Aşağıdaki [az keyvault kümesi ilkesi][az-keyvault-set-policy] komutunda, oluşturduğunuz yönetilen kimliğin, daha önce bir ortam değişkeninde depolanan ana kimliğini geçirirsiniz. **Almak**için anahtar izinlerini ayarlayın , **unwrapKey**, ve **wrapKey**.  

```azurecli
az keyvault set-policy \
  --resource-group <resource-group-name> \
  --name <key-vault-name> \
  --object-id $identityPrincipalID \
  --key-permissions get unwrapKey wrapKey 
```

### <a name="create-key-and-get-key-id"></a>Anahtar oluşturun ve anahtar kimliği alın

Anahtar tonozbir anahtar oluşturmak için [az keyvault tuşuna oluşturun.][az-keyvault-key-create]

```azurecli
az keyvault key create \
  --name <key-name> \
  --vault-name <key-vault-name>
```

Komut çıkışında, anahtarın kimliğine dikkat `kid`edin. Bu kimliği bir sonraki adımda kullanırsınız:

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
Kolaylık sağlamak için, bu değeri bir ortam değişkeninde saklayın:

```azurecli
keyID=$(az keyvault key show --name <keyname> --vault-name <key-vault-name> --query 'key.kid' --output tsv)
```

### <a name="create-a-registry-with-customer-managed-key"></a>Müşteri tarafından yönetilen anahtarla kayıt defteri oluşturma

Kayıt defteri oluşturmak ve müşteri tarafından yönetilen anahtarı etkinleştirmek için [az acr oluşturma][az-acr-create] komutunu çalıştırın. Yönetilen kimlik asıl kimliğini ve daha önce ortam değişkenlerinde depolanan anahtar kimliğini geçirin:

```azurecli
az acr create \
  --resource-group <resource-group-name> \
  --name <container-registry-name> \
  --identity $identityID \
  --key-encryption-key $keyID \
  --sku Premium
```

### <a name="show-encryption-status"></a>Şifreleme durumunu göster

Müşteri tarafından yönetilen bir anahtarla kayıt defteri şifrelemesinin etkin olup olmadığını göstermek için [az acr şifreleme göster][az-acr-encryption-show] komutunu çalıştırın:

```azurecli
az acr encryption show --name <registry-name> 
```

## <a name="enable-customer-managed-key---portal"></a>Müşteri tarafından yönetilen anahtarı etkinleştir - portal

### <a name="create-a-managed-identity"></a>Yönetilen bir kimlik oluşturma

Azure portalında [Azure kaynakları için](../active-directory/managed-identities-azure-resources/overview.md) kullanıcı tarafından atanmış yönetilen bir kimlik oluşturun. Adımlar için bkz. [Kullanıcı tarafından atanan kimlik oluşturma.](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md#create-a-user-assigned-managed-identity)

Yönetilen kimliğin **Kaynak Adını** dikkate alın. Daha sonraki adımlarda bu isme ihtiyacınız var.

![Azure portalında kullanıcı tarafından atanmış yönetilen kimlik oluşturma](./media/container-registry-customer-managed-keys/create-managed-identity.png)

### <a name="create-a-key-vault"></a>Bir anahtar kasası oluşturma

Önemli bir kasa oluşturmak için adımlar için [Quickstart: Azure portalını kullanarak Azure Key Vault'tan bir sır ayarlayın ve alın.](../key-vault/secrets/quick-create-portal.md)

**Basics** sekmesinde, müşteri tarafından yönetilen bir anahtar için anahtar kasaoluştururken, aşağıdaki koruma ayarlarını etkinleştirmeniz gerekir: **Yumuşak silme** ve **Temizleme koruması.** Bu ayarlar, yanlışlıkla anahtar veya anahtar silme neden olduğu veri kaybını önlemeye yardımcı olur.

![Azure portalında anahtar kasası oluşturma](./media/container-registry-customer-managed-keys/create-key-vault.png)

### <a name="add-key-vault-access-policy"></a>Anahtar kasa erişim ilkesi ekleme

Kimlik erişebilsin diye anahtar kasası için bir ilke yapılandırın.

1. Anahtar kasanıza gidin.
1. **Ayarlar** > **Erişim ilkelerini seçin > +Ekle Erişim İlkesi.**
1. **Anahtar izinlerini**seçin ve **Al**, **Aç Tuşu**ve **Şal Anahtarı'nı**seçin.
1. **Anapara seçin** ve kullanıcı tarafından atanan yönetilen kimliğinizin kaynak adını seçin.  
1. **Ekle'yi**seçin, ardından **Kaydet'i**seçin.

![Anahtar kasa erişim ilkesi oluşturma](./media/container-registry-customer-managed-keys/add-key-vault-access-policy.png)

### <a name="create-key"></a>Oluşturma anahtarı

1. Anahtar kasanıza gidin.
1. **Ayarlar** > **Tuşlarını**seçin.
1. **+Oluştur/İçe'yi** seçin ve anahtar için benzersiz bir ad girin.
1. Kalan varsayılan değerleri kabul edin ve **Oluştur'u**seçin.
1. Oluşturmadan sonra, anahtarı seçin ve geçerli anahtar sürümüne dikkat edin.

### <a name="create-azure-container-registry"></a>Azure kapsayıcı kayıt defteri oluşturma

1. Kaynak > **Kapsayıcılar** > **Kayıt Defteri** **Oluştur'u**seçin.
1. Temel **Bilgiler** sekmesinde, bir kaynak grubu seçin veya oluşturun ve bir kayıt defteri adı girin. **SKU'da** **Premium'u**seçin.
1. **Şifreleme** sekmesinde, **Müşteri tarafından yönetilen anahtarda** **Etkin'i**seçin.
1. **Identity'de**oluşturduğunuz yönetilen kimliği seçin.
1. **Şifreleme anahtarında,** **Key Vault'tan Seç'i**seçin.
1. Azure **Key Vault penceresinden Select tuşu'nda,** önceki bölümde oluşturduğunuz anahtar kasasını, anahtarı ve sürümü seçin.
1. **Şifreleme** sekmesinde, **Gözden Geçir + oluştur'u**seçin.
1. Kayıt defteri örneğini dağıtmak için **Oluştur'u** seçin.

![Azure portalında kapsayıcı kayıt defteri oluşturma](./media/container-registry-customer-managed-keys/create-encrypted-registry.png)

Kayıt defterinizin şifreleme durumunu portalda görmek için kayıt defterinize gidin. **Ayarlar**altında **Şifreleme (Önizleme) seçeneğini belirleyin.**

## <a name="enable-customer-managed-key---template"></a>Müşteri tarafından yönetilen anahtarı etkinleştirme - şablon

Kayıt defteri oluşturmak ve müşteri tarafından yönetilen bir anahtarla şifrelemeyi etkinleştirmek için kaynak yöneticisi şablonu da kullanabilirsiniz. 

Aşağıdaki şablon, yeni bir kapsayıcı kayıt defteri ve kullanıcı tarafından atanmış yönetilen bir kimlik oluşturur. Aşağıdaki içeriği yeni bir dosyaya kopyalayın ve dosya `CMKtemplate.json`adı gibi bir dosya adı kullanarak kaydedin.

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

* Anahtar kasası, adıyla tanımlanır
* Anahtar kasa sı, anahtar kimliği ile tanımlanır

Önceki şablon dosyasını kullanarak kayıt defteri oluşturmak için aşağıdaki [az grup oluşturma][az-group-deployment-create] komutunu çalıştırın. Belirtilen durumlarda, oluşturduğunuz anahtar kasa adı ve anahtar kimliğinin yanı sıra yeni bir kayıt defteri adı ve yönetilen kimlik adı sağlayın. 

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

Kayıt defteri şifreleme durumunu göstermek için [az acr şifreleme durumu][az-acr-encryption-show-status] komutunu çalıştırın:

```azurecli
az acr encryption show-status --name <registry-name> 
```

## <a name="use-the-registry"></a>Kayıt defterini kullanma

Bir kayıt defterinin müşteri tarafından yönetilen bir anahtarı kullanarak verileri şifrelemesini etkinleştirdikten sonra, müşteri tarafından yönetilen bir anahtarla şifrelenmemiş bir kayıt defterinde gerçekleştirdiğiniz aynı kayıt defteri işlemlerini gerçekleştirebilirsiniz. Örneğin, kayıt defteriyle kimlik doğrulaması yapabilir ve Docker görüntülerini itebilirsiniz. [Itme ve görüntü çekme'deki](container-registry-get-started-docker-cli.md)örnek komutlara bakın.

## <a name="rotate-key"></a>Anahtarı döndür

Azure Key Vault'ta müşteri tarafından yönetilen bir anahtarı uyumluluk ilkelerinize göre döndürebilirsiniz. Yeni bir anahtar oluşturun ve yeni anahtarı kullanarak verileri şifrelemek için kayıt defterini güncelleştirin. Bu adımları Azure CLI'yi kullanarak veya portalda gerçekleştirebilirsiniz.

Örneğin, yeni bir anahtar oluşturmak için [az keyvault tuşunu çalıştırın][az-keyvault-key-create] komutu:

```azurecli
az keyvault key create –-name <new-key-name> --vault-name <key-vault-name> 
```

Ardından [az acr şifreleme döndürme anahtarı][az-acr-encryption-rotate-key] komutunu çalıştırın ve yeni anahtar kimliğini ve daha önce yapılandırdığınız yönetilen kimliğin temel kimliğini geçirin:

```azurecli
az acr encryption rotatekey \
  --name <registry-name> \
  --key-encryption-key <new-key-id> \
  --identity $identityPrincipalID
```

## <a name="revoke-key"></a>Anahtarı iptal et

Anahtar kasasındaki erişim ilkesini değiştirerek veya anahtarı silerek müşteri tarafından yönetilen şifreleme anahtarını iptal edin. Örneğin, kayıt defteriniz tarafından kullanılan yönetilen kimliğin erişim ilkesini değiştirmek için [az keyvault silme ilkesi][az-keyvault-delete-policy] komutunu kullanın. Örneğin:

```azurecli
az keyvault delete-policy \
  --resource-group <resource-group-name> \
  --name <key-vault-name> \
  --object-id $identityPrincipalID
```

Kayıt defteri şifreleme anahtarına erişemediğinden, anahtarın iptali tüm kayıt defteri verilerine erişimi etkin bir şekilde engeller. Anahtara erişim etkinleştirilmişse veya silinen anahtar geri yüklenirse, şifrelenmiş kayıt defteri verilerine yeniden erişebilmeniz için kayıt defteriniz anahtarı seçer.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure'da dinlenilerde şifreleme](../security/fundamentals/encryption-atrest.md)hakkında daha fazla bilgi edinin.
* Erişim ilkeleri ve [önemli bir kasaya erişimin](../key-vault/general/secure-your-key-vault.md)nasıl güvenli hale ulaşılabildiğini öğrenin.
* Azure Konteyner Kayıt Defteri için müşteri tarafından yönetilen anahtarlar hakkında geri bildirim sağlamak için [ACR GitHub sitesini](https://aka.ms/acr/issues)ziyaret edin.


<!-- LINKS - external -->
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms

<!-- LINKS - internal -->

[az-feature-register]: /cli/azure/feature#az-feature-register
[az-feature-show]: /cli/azure/feature#az-feature-show
[az-group-create]: /cli/azure/group#az-group-create
[az-identity-create]: /cli/azure/identity#az-identity-create
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-group-deployment-create]: /cli/azure/group/deployment#az-group-deployment-create
[az-keyvault-create]: /cli/azure/keyvault#az-keyvault-create
[az-keyvault-key-create]: /cli/azure/keyvault/key#az-keyvault-key-create
[az-keyvault-set-policy]: /cli/azure/keyvault#az-keyvault-set-policy
[az-keyvault-delete-policy]: /cli/azure/keyvault#az-keyvault-delete-policy
[az-resource-show]: /cli/azure/resource#az-resource-show
[az-acr-create]: /cli/azure/acr#az-acr-create
[az-acr-show]: /cli/azure/acr#az-acr-show
[az-acr-encryption-rotate-key]: /cli/azure/acr/encryption#az-acr-encryption-rotate-key
[az-acr-encryption-show]: /cli/azure/acr/encryption#az-acr-encryption-show
