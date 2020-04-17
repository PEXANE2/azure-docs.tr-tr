---
title: ISEs'te kalan yerlerdeki verileri şifrelemek için müşteri tarafından yönetilen anahtarları ayarlama
description: Azure Logic Apps'ta entegrasyon hizmeti ortamları (ISEs) için verileri istirahatte güvende hale getirmek için kendi şifreleme anahtarlarınızı oluşturun ve yönetin
services: logic-apps
ms.suite: integration
ms.reviewer: klam, rarayudu, logicappspm
ms.topic: conceptual
ms.date: 03/11/2020
ms.openlocfilehash: 7314559849f0b2019820ec3cb4fb10c684d330d6
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81458446"
---
# <a name="set-up-customer-managed-keys-to-encrypt-data-at-rest-for-integration-service-environments-ises-in-azure-logic-apps"></a>Azure Logic Apps'ta tümleştirme hizmeti ortamları (ISEs) için verileri istirahatte şifrelemek için müşteri tarafından yönetilen anahtarları ayarlama

Azure Logic Apps, [verileri istirahatte](../storage/common/storage-service-encryption.md)depolamak ve otomatik olarak şifrelemek için Azure Depolama'ya güvenir. Bu şifreleme verilerinizi korur ve kuruluş güvenliği ve uyumluluk taahhütlerinizi karşılamanıza yardımcı olur. Varsayılan olarak Azure Depolama, verilerinizi şifrelemek için Microsoft tarafından yönetilen anahtarları kullanır. Azure Depolama şifrelemesi hakkında daha fazla bilgi [için, veriler için Azure Depolama şifrelemesi](../storage/common/storage-service-encryption.md) ve [Beklemede Azure Veri](../security/fundamentals/encryption-atrest.md)Şifrelemesi'ne bakın.

Mantıksal uygulamalarınızı barındırmak için bir [entegrasyon hizmeti ortamı (Ise)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md) oluşturduğunuzda ve Azure Depolama tarafından kullanılan şifreleme anahtarları üzerinde daha fazla denetim istediğinizde, [Azure Key Vault'u](../key-vault/general/overview.md)kullanarak kendi anahtarınızı ayarlayabilir, kullanabilir ve yönetebilirsiniz. Bu özellik "Kendi Anahtarını Getir" (BYOK) olarak da bilinir ve anahtarınıza "müşteri tarafından yönetilen anahtar" denir.

Bu konu, Logic Apps REST API'sini kullanarak İmKB'nizi oluştururken kullanılacak kendi şifreleme anahtarınızı nasıl ayarlayıp belirtdiğinizi gösterir. Logic Apps REST API ile Bir İmKB oluşturmak için genel adımlar için, [Bkz. Logic Apps REST API'yi kullanarak bir entegrasyon hizmet ortamı (İmKB) oluşturun.](../logic-apps/create-integration-service-environment-rest-api.md)

## <a name="considerations"></a>Dikkat edilmesi gerekenler

* Şu anda, Bir İmKB için müşteri tarafından yönetilen anahtar desteği yalnızca bu Azure bölgelerinde kullanılabilir: Batı ABD 2, Doğu ABD ve Güney Orta ABD

* Müşteri tarafından yönetilen bir anahtarı, yalnızca Daha sonra değil, *İmKB'nizi oluşturduğunuzda*belirtebilirsiniz. İmKB'niz oluşturulduktan sonra bu anahtarı devre dışı kalamazsınız. Şu anda, bir İmKB için müşteri tarafından yönetilen bir anahtarı döndürmek için destek yok.

* Müşteri tarafından yönetilen anahtarları desteklemek için Ise'inizin [sistem tarafından atanmış yönetilen kimliğinin](../active-directory/managed-identities-azure-resources/overview.md#how-does-the-managed-identities-for-azure-resources-work) etkinleştirilmesi gerekir. Bu kimlik, İmKB'nin diğer Azure Etkin Dizin (Azure AD) kiracılarında bulunan kaynaklara erişimi doğrulamasına olanak tanır, böylece kimlik bilgilerinizle oturum açmanız gerekmez.

* Şu anda, müşteri tarafından yönetilen anahtarları destekleyen ve sistem tarafından atanmış kimliği etkinleştirilmiş bir İmKB oluşturmak için, bir HTTPS PUT isteği kullanarak Logic Apps REST API'yi aramanız gerekir.

* İmKB'nizi oluşturan HTTPS PUT isteğini gönderdikten *30 dakika* sonra, [İmKB'nizin sistemle atanmış kimliğine anahtar kasaerişimi vermelisiniz.](#identity-access-to-key-vault) Aksi takdirde, İmKB oluşturma başarısız olur ve bir izinhatası atar.

## <a name="prerequisites"></a>Ön koşullar

* Azure portalında Bir İmKB oluşturduğunuzda olduğu [gibi İmKB'nize erişimi etkinleştirmek için](../logic-apps/connect-virtual-network-vnet-isolated-environment.md#enable-access) aynı [ön koşullar](../logic-apps/connect-virtual-network-vnet-isolated-environment.md#prerequisites) ve gereksinimler

* **Yumuşak Silme** ve **Temizleme özelliklerini** etkinleştiren Azure anahtar kasası

  Bu özellikleri etkinleştirme hakkında daha fazla bilgi için [Azure Key Vault yumuşak silme genel bakışı](../key-vault/general/overview-soft-delete.md) ve Azure Key Vault ile müşteri tarafından yönetilen anahtarları [yapılandırın.](../storage/common/storage-encryption-keys-portal.md) Azure Key Vault'ta yeniyseniz, Azure portalını kullanarak veya Azure PowerShell komutu Olan [New-AzKeyVault'u](https://docs.microsoft.com/powershell/module/az.keyvault/new-azkeyvault)kullanarak [nasıl bir anahtar kasası oluşturabileceğinizi](../key-vault/secrets/quick-create-portal.md#create-a-vault) öğrenin.

* Anahtar kasanızda, bu özellik değerleri yle oluşturulmuş bir anahtar:

  | Özellik | Değer |
  |----------|-------|
  | **Anahtar Türü** | RSA |
  | **RSA Anahtar Boyutu** | 2048 |
  | **Etkin** | Evet |
  |||

  ![Müşteri tarafından yönetilen şifreleme anahtarınızı oluşturun](./media/customer-managed-keys-integration-service-environment/create-customer-managed-key-for-encryption.png)

  Daha fazla bilgi için Azure Key Vault veya Azure PowerShell komutu ekle [ile](https://docs.microsoft.com/powershell/module/az.keyvault/Add-AzKeyVaultKey) [müşteri tarafından yönetilen anahtarları yapılandırın.](../storage/common/storage-encryption-keys-portal.md)

* Https PUT isteğiyle Logic Apps REST API'sini arayarak İmKB'nizi oluşturmak için kullanabileceğiniz bir araçtır. Örneğin, [Postacı'yı](https://www.getpostman.com/downloads/)kullanabilir veya bu görevi gerçekleştiren bir mantık uygulaması oluşturabilirsiniz.

<a name="enable-support-key-system-identity"></a>

## <a name="create-ise-with-key-vault-and-managed-identity-support"></a>Anahtar kasa ve yönetilen kimlik desteği ile İmKB'yi oluşturun

Logic Apps REST API'yi arayarak İmKB'nizi oluşturmak için şu HTTPS PUT isteğini yapın:

`PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Logic/integrationServiceEnvironments/{integrationServiceEnvironmentName}?api-version=2019-05-01`

> [!IMPORTANT]
> Logic Apps REST API 2019-05-01 sürümü, İmKB konektörleri için kendi HTTP PUT isteğinizi oluşturmanızı gerektirir.

Dağıtımın tamamlanması genellikle iki saat içinde gerçekleşir. Bazen, dağıtım dört saat kadar sürebilir. Dağıtım durumunu denetlemek için, Azure araç çubuğunuzdaki [Azure portalında,](https://portal.azure.com)bildirimler bölmesini açan bildirimler simgesini seçin.

> [!NOTE]
> Dağıtım başarısız olursa veya İmKB'nizi silerseniz, Azure'un alt ağlarınızı serbest bırakması bir saat kadar sürebilir. Bu gecikme, bu alt ağları başka bir ISE'de yeniden kullanmadan önce beklemeniz anlamına gelir.
>
> Sanal ağınızı silerseniz, Azure'un alt ağlarınızı serbest bırakması genellikle iki saat kadar sürer, ancak bu işlem daha uzun sürebilir. 
> Sanal ağları silerken, hiçbir kaynağın hala bağlı olmadığından emin olun. 
> Bkz. [Sanal ağı sil.](../virtual-network/manage-virtual-network.md#delete-a-virtual-network)

### <a name="request-header"></a>İstek üst bilgisi

İstek üstbilgisinde şu özellikleri ekleyin:

* `Content-type`: Bu özellik `application/json`değerini .

* `Authorization`: Bu özellik değerini, kullanmak istediğiniz Azure aboneliğine veya kaynak grubuna erişimi olan müşteri için taşıyıcı belirteci olarak ayarlayın.

### <a name="request-body"></a>İstek gövdesi

İstek gövdesinde, Kendi Bilgileri İmKB tanımınızda sağlayarak bu ek öğeler için destek sağlayabilir:

* İmKB'nizin anahtar kasanıza erişmek için kullandığı sistem atanmış yönetilen kimlik
* Anahtar kasanız ve kullanmak istediğiniz müşteri tarafından yönetilen anahtar

#### <a name="request-body-syntax"></a>İstek gövdesi sözdizimi

İmKB'nizi oluştururken kullanılacak özellikleri açıklayan istek gövdesi sözdizimi aşağıda verilmiştir:

```json
{
   "id": "/subscriptions/{Azure-subscription-ID/resourceGroups/{Azure-resource-group}/providers/Microsoft.Logic/integrationServiceEnvironments/{ISE-name}",
   "name": "{ISE-name}",
   "type": "Microsoft.Logic/integrationServiceEnvironments",
   "location": "{Azure-region}",
   "sku": {
      "name": "Premium",
      "capacity": 1
   },
   "identity": {
      "type": "SystemAssigned"
   },
   "properties": {
      "networkConfiguration": {
         "accessEndpoint": {
            // Your ISE can use the "External" or "Internal" endpoint. This example uses "External".
            "type": "External"
         },
         "subnets": [
            {
               "id": "/subscriptions/{Azure-subscription-ID}/resourceGroups/{Azure-resource-group}/providers/Microsoft.Network/virtualNetworks/{virtual-network-name}/subnets/{subnet-1}",
            },
            {
               "id": "/subscriptions/{Azure-subscription-ID}/resourceGroups/{Azure-resource-group}/providers/Microsoft.Network/virtualNetworks/{virtual-network-name}/subnets/{subnet-2}",
            },
            {
               "id": "/subscriptions/{Azure-subscription-ID}/resourceGroups/{Azure-resource-group}/providers/Microsoft.Network/virtualNetworks/{virtual-network-name}/subnets/{subnet-3}",
            },
            {
               "id": "/subscriptions/{Azure-subscription-ID}/resourceGroups/{Azure-resource-group}/providers/Microsoft.Network/virtualNetworks/{virtual-network-name}/subnets/{subnet-4}",
            }
         ]
      },
      "encryptionConfiguration": {
         "encryptionKeyReference": {
            "keyVault": {
               "id": "subscriptions/{Azure-subscription-ID}/resourceGroups/{Azure-resource-group}/providers/Microsoft.KeyVault/vaults/{key-vault-name}",
            },
            "keyName": "{customer-managed-key-name}",
            "keyVersion": "{key-version-number}"
         }
      }
   }
}
```

#### <a name="request-body-example"></a>İstek gövdesi örneği

Bu örnek istek gövdesi örnek değerleri gösterir:

```json
{
   "id": "/subscriptions/********************/resourceGroups/Fabrikam-RG/providers/Microsoft.Logic/integrationServiceEnvironments/Fabrikam-ISE",
   "name": "Fabrikam-ISE",
   "type": "Microsoft.Logic/integrationServiceEnvironments",
   "location": "WestUS2",
   "identity": {
      "type": "SystemAssigned"
   },
   "sku": {
      "name": "Premium",
      "capacity": 1
   },
   "properties": {
      "networkConfiguration": {
         "accessEndpoint": {
            // Your ISE can use the "External" or "Internal" endpoint. This example uses "External".
            "type": "External"
         },
         "subnets": [
            {
               "id": "/subscriptions/********************/resourceGroups/Fabrikam-RG/providers/Microsoft.Network/virtualNetworks/Fabrikam-VNET/subnets/subnet-1",
            },
            {
               "id": "/subscriptions/********************/resourceGroups/Fabrikam-RG/providers/Microsoft.Network/virtualNetworks/Fabrikam-VNET/subnets/subnet-2",
            },
            {
               "id": "/subscriptions/********************/resourceGroups/Fabrikam-RG/providers/Microsoft.Network/virtualNetworks/Fabrikam-VNET/subnets/subnet-3",
            },
            {
               "id": "/subscriptions/********************/resourceGroups/Fabrikam-RG/providers/Microsoft.Network/virtualNetworks/Fabrikam-VNET/subnets/subnet-4",
            }
         ]
      },
      "encryptionConfiguration": {
         "encryptionKeyReference": {
            "keyVault": {
               "id": "subscriptions/********************/resourceGroups/Fabrikam-RG/providers/Microsoft.KeyVault/vaults/FabrikamKeyVault",
            },
            "keyName": "Fabrikam-Encryption-Key",
            "keyVersion": "********************"
         }
      }
   }
}
```

<a name="identity-access-to-key-vault"></a>

## <a name="grant-access-to-your-key-vault"></a>Anahtar kasanıza erişim izni ver

İmKB'nizi oluşturmak için HTTP PUT isteğini gönderdikten *30 dakika* sonra, İmKB'nizin sistemle atanmış kimliği için anahtar kasanıza bir erişim ilkesi eklemeniz gerekir. Aksi takdirde, İmKB'niz için oluşturma başarısız olur ve bir izin hatası alırsınız. 

Bu görev için Azure PowerShell [Set-AzKeyVaultAccessPolicy](https://docs.microsoft.com/powershell/module/az.keyvault/set-azkeyvaultaccesspolicy) komutunu kullanabilir veya Azure portalında aşağıdaki adımları izleyebilirsiniz:

1. Azure [portalında](https://portal.azure.com)Azure anahtar kasanızı açın.

1. Anahtar kasa menüsünde,**Access** **ilkelerini** > ekle ilkelerini seçin , örneğin:

   ![Sistem tarafından atanan yönetilen kimlik için erişim ilkesi ekleme](./media/customer-managed-keys-integration-service-environment/add-ise-access-policy-key-vault.png)

1. Erişim **ekle ilkesi** bölmesi açıldıktan sonra aşağıdaki adımları izleyin:

   1. Şu seçenekleri seçin:

      | Ayar | Değerler |
      |---------|--------|
      | **Şablon (isteğe bağlı) listesinden yapılandırma** | Anahtar Yönetimi |
      | **Anahtar izinler** | - **Anahtar Yönetim İşlemleri**: Al, Liste <p><p>- **Şifreleme İşlemleri**: Açma Tuşu, Şal Anahtarı |
      |||

      !["Anahtar İzni" > "Anahtar Yönetimi"ni seçin](./media/customer-managed-keys-integration-service-environment/select-key-permissions.png)

   1. **Select principal**için, **Seçili Yok'u**seçin. **Asıl** bölme açıldıktan sonra, arama kutusunda Ise'nizi bulun ve seçin. Bittikten sonra >  **Ekle'yi seçin.****Add**

      ![Asıl olarak kullanmak için İmKB'nizi seçin](./media/customer-managed-keys-integration-service-environment/select-service-principal-ise.png)

   1. **Access ilkeleri** bölmesini bitirdikten sonra **Kaydet'i**seçin.

Daha fazla bilgi için [bkz.](../key-vault/general/managed-identity.md#grant-your-app-access-to-key-vault)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Anahtar Kasası](../key-vault/general/overview.md) hakkında daha fazla bilgi edinin