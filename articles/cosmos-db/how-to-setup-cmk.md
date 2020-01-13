---
title: Azure Cosmos DB hesabınız için müşteri tarafından yönetilen anahtarları yapılandırın
description: Azure Cosmos DB hesabınız için müşteri tarafından yönetilen anahtarları yapılandırmayı öğrenin
author: ThomasWeiss
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 01/11/2020
ms.author: thweiss
ROBOTS: noindex, nofollow
ms.openlocfilehash: 964c3e4e2de43e6bcae353f0b525eb62e6613361
ms.sourcegitcommit: f9601bbccddfccddb6f577d6febf7b2b12988911
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2020
ms.locfileid: "75911883"
---
# <a name="configure-customer-managed-keys-for-your-azure-cosmos-db-account"></a>Azure Cosmos DB hesabınız için müşteri tarafından yönetilen anahtarları yapılandırın

> [!NOTE]
> Şu anda bu özelliği kullanmak için erişim istemeniz gerekir. Bunu yapmak için lütfen [cosmosdbpm@microsoft.com](mailto:cosmosdbpm@microsoft.com)başvurun.

Azure Cosmos DB hesabınızda depolanan veriler otomatik olarak ve sorunsuz bir şekilde şifrelenir. Azure Cosmos DB, bekleyen verilerinizi şifrelemek için kullanılan anahtarları yönetmek için iki seçenek sunar:
- **Hizmet tarafından yönetilen anahtarlar**. Varsayılan olarak, Microsoft Azure Cosmos DB hesabınızı şifrelemek için kullanılan anahtarları yönetir.
- **Müşteri tarafından yönetilen anahtarlar (CMK)** . İsteğe bağlı olarak, yönettiğiniz anahtarlarla ikinci bir şifreleme katmanı eklemeyi seçebilirsiniz.

Müşteri tarafından yönetilen anahtarların [Azure Key Vault](../key-vault/key-vault-overview.md)içinde depolanması gerekir. Her CMK etkin hesap için bir anahtar belirtilmelidir ve bu hesapta depolanan tüm verileri şifrelemek için kullanılır.

## <a name="setup"></a>Kurulum

Şu anda, müşteri tarafından yönetilen anahtarlar yalnızca yeni hesaplar için kullanılabilir ve hesap oluşturma sırasında ayarlanması gerekir.

### <a name="1-make-sure-the-azure-cosmos-db-resource-provider-is-registered-for-your-azure-subscription"></a>1. Azure aboneliğiniz için Azure Cosmos DB kaynak sağlayıcısının kayıtlı olduğundan emin olun

Azure portal Azure aboneliğinize gidin ve sol menüden **kaynak sağlayıcıları** ' nı seçin:

![Sol menüden "kaynak sağlayıcıları" girdisi](./media/how-to-setup-cmk/portal-rp.png)

**Microsoft. DocumentDB** kaynak sağlayıcısı için arama yapın.
- Kaynak sağlayıcı zaten kayıtlı olarak işaretlenmişse, hiçbir şeyin gerçekleştirilmesi gerekmez.
- Aksi takdirde, seçin ve **Kaydet**' e tıklayın:

    ![Microsoft. DocumentDB kaynak sağlayıcısını kaydetme](./media/how-to-setup-cmk/portal-rp-register.png)

### <a name="2-configure-your-azure-key-vault-instance"></a>2. Azure Key Vault örneğinizi yapılandırın

Azure Cosmos DB ile müşteri tarafından yönetilen anahtarların kullanılması, şifreleme anahtarlarınızı barındırmak için kullanmayı planladığınız Azure Key Vault örneğinde iki özellik ayarlanmasını gerektirir: **geçici silme** ve **Temizleme**. Bu özellikler varsayılan olarak etkinleştirilmez, ancak PowerShell veya Azure CLı kullanılarak etkinleştirilebilir.

Mevcut bir Azure Key Vault örneğinde bu özellikleri nasıl etkinleştireceğinizi öğrenmek için, aşağıdaki makalelerden birinde geçici silme özelliğini etkinleştirme ve Temizleme korumasını etkinleştirme başlıklı bölümlere bakın:
- [PowerShell ile geçici silmeyi kullanma](../key-vault/key-vault-soft-delete-powershell.md)
- [Azure CLı ile geçici silme kullanma](../key-vault/key-vault-soft-delete-cli.md)

### <a name="3-add-an-access-policy-to-your-azure-key-vault-instance"></a>3. Azure Key Vault örneğine bir erişim ilkesi ekleyin

Azure portal, şifreleme anahtarlarınızı barındırmak için kullanmayı planladığınız Azure Key Vault örneğine gidin. Ardından, sol menüden **erişim ilkeleri** ' ni seçin:

![Sol menüden "erişim ilkeleri"](./media/how-to-setup-cmk/portal-akv-ap.png)

- **+ Erişim Ilkesi Ekle** ' yi seçin
- **Anahtar izinleri** açılan menüsünde **Al**, **sarmalama tuşu** ve **sarmalama tuşu**' nı seçin:

    ![Doğru izinleri seçme](./media/how-to-setup-cmk/portal-akv-add-ap-perm2.png)

- **Asıl seçin**altında **hiçbiri seçili**' i seçin. Ardından, **Azure Cosmos DB** sorumlusunu arayıp seçin. Son olarak, en altta **Seç** ' e tıklayın ( **Azure Cosmos DB** sorumlusu bulunamazsa, 1 **. adımda Microsoft. DocumentDB** kaynak sağlayıcısını yeniden kaydetmeniz gerekebilir):

    ![Azure Cosmos DB sorumlusunu seçme](./media/how-to-setup-cmk/portal-akv-add-ap.png)

- Yeni erişim ilkesini eklemek için **Ekle** ' yi seçin

### <a name="4-generate-a-key-in-azure-key-vault"></a>4. Azure Key Vault bir anahtar oluşturun

Azure portal, şifreleme anahtarlarınızı barındırmak için kullanmayı planladığınız Azure Key Vault örneğine gidin. Ardından, sol menüden **anahtarlar** ' ı seçin:

![Sol menüden "anahtarlar" girdisi](./media/how-to-setup-cmk/portal-akv-keys.png)

- **Oluştur/Içeri aktar** 'ı seçin
- Yeni anahtar için bir ad girin, bir RSA anahtar boyutu seçin (en iyi güvenlik için en az 3072 önerilir) ve **Oluştur**' u seçin:

    ![Yeni anahtar oluşturma](./media/how-to-setup-cmk/portal-akv-gen.png)

- Anahtar oluşturulduktan sonra, yeni oluşturulan anahtara, sonra geçerli sürümüne tıklayın
- Son eğik çizgiden sonraki bölüm dışında anahtarın **anahtar tanımlayıcısını** kopyalayın:

    ![Anahtarın anahtar tanımlayıcısı kopyalanıyor](./media/how-to-setup-cmk/portal-akv-keyid.png)

### <a name="5-create-a-new-azure-cosmos-db-account"></a>5. yeni bir Azure Cosmos DB hesabı oluşturun

#### <a name="using-the-azure-portal"></a>Azure portalını kullanma

Azure portal yeni bir Azure Cosmos DB hesabı oluştururken **şifreleme** adımında **müşteri tarafından yönetilen anahtar** ' ı seçin. **Anahtar URI 'si** alanında, 4. adımdan kopyalanmış Azure Key Vault anahtarın URI 'sini geçirin:

![Azure portal CMK parametrelerini ayarlama](./media/how-to-setup-cmk/portal-cosmos-enc.png)

#### <a name="using-powershell"></a>PowerShell'i kullanma

PowerShell ile yeni bir Azure Cosmos DB hesabı oluştururken,
- 4\. adımda, **Propertyobject**Içindeki **Keyvaultkeyuri** özelliği ALTıNA kopyalanmış Azure Key Vault anahtarın URI 'sini geçirin,
- API sürümü olarak "2019-12-12" kullandığınızdan emin olun.

> [!IMPORTANT]
> Hesabın CMK ile başarıyla oluşturulabilmesi için `Location` parametresi açıkça ayarlanmalıdır.

```powershell
$resourceGroupName = "myResourceGroup"
$accountLocation = "West US 2"
$accountName = "mycosmosaccount"

$failoverLocations = @(
    @{ "locationName"="West US 2"; "failoverPriority"=0 }
)

$CosmosDBProperties = @{
    "databaseAccountOfferType"="Standard";
    "locations"=$failoverLocations;
    "keyVaultKeyUri" = "https://<my-vault>.vault.azure.net/keys/<my-key>";
}

New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2019-12-12" -ResourceGroupName $resourceGroupName `
    -Location $accountLocation -Name $accountName -PropertyObject $CosmosDBProperties
```

#### <a name="using-azure-resource-manager-templates"></a>Azure Resource Manager şablonları kullanma

Bir Azure Resource Manager şablonuyla yeni bir Azure Cosmos DB hesabı oluştururken:
- **Properties** nesnesinde **Keyvaultkeyuri** özelliği altındaki 4. ADıMDAN kopyalanmış Azure Key Vault anahtarının URI 'sini geçirin
- API sürümü olarak "2019-12-12" kullandığınızdan emin olun

> [!IMPORTANT]
> Hesabın CMK ile başarıyla oluşturulabilmesi için `location` parametresi açıkça ayarlanmalıdır.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "accountName": {
            "type": "string"
        },
        "location": {
            "type": "string"
        },
        "keyVaultKeyUri": {
            "type": "string"
        }
    },
    "resources": 
    [
        {
            "type": "Microsoft.DocumentDB/databaseAccounts",
            "name": "[parameters('accountName')]",
            "apiVersion": "2019-12-12",
            "kind": "GlobalDocumentDB",
            "location": "[parameters('location')]",
            "properties": {
                "locations": [ 
                    {
                        "locationName": "[parameters('location')]",
                        "failoverPriority": 0,
                        "isZoneRedundant": false
                    }
                ],
                "databaseAccountOfferType": "Standard",
                "keyVaultKeyUri": "[parameters('keyVaultKeyUri')]"
            }
        }
    ]
}

```

```powershell
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$accountLocation = "West US 2"
$keyVaultKeyUri = "https://<my-vault>.vault.azure.net/keys/<my-key>"

New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateFile "deploy.json" `
    -accountName $accountName `
    -location $accountLocation `
    -keyVaultKeyUri $keyVaultKeyUri
```

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="is-there-any-additional-charge-when-using-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarlar kullanılırken başka ücret de var mı?

Evet. Müşteri tarafından yönetilen anahtarlarla veri şifrelemeyi ve şifre çözmeyi yönetmek için gereken ek işlem yükünü hesaba eklemek için, Azure Cosmos DB hesapta yürütülen tüm işlemler, tüketilen [Istek birimlerinde](./request-units.md) %25 artış alır.

### <a name="what-data-gets-encrypted-with-the-cmk"></a>CMK ile hangi veriler şifrelenir?

Aşağıdaki meta veriler hariç Azure Cosmos DB hesabınızda depolanan tüm veriler CMK ile şifrelenir:
- Azure Cosmos DB [hesaplarınızın, veritabanlarının ve kapsayıcılarınızın](./account-overview.md#elements-in-an-azure-cosmos-account)adları
- [saklı yordamlarınızın](./stored-procedures-triggers-udfs.md)adları,
- [Dizin oluşturma ilkeleriniz](./index-policy.md)içinde belirtilen özellik yolları
- kapsayıcılarınızın [bölüm anahtarı](./partitioning-overview.md)değerleri.

### <a name="will-customer-managed-keys-be-supported-for-existing-accounts"></a>Mevcut hesaplar için müşteri tarafından yönetilen anahtarlar desteklenecek mi?

Bu özellik şu anda yalnızca yeni hesaplar için kullanılabilir.

### <a name="is-there-a-plan-to-support-finer-granularity-than-account-level-keys"></a>Hesap düzeyi anahtarlardan daha ince ayrıntı düzeyi desteklemeye yönelik bir plan var mı?

Şu anda değil, kapsayıcı düzeyi anahtarlar kabul ediliyor.

### <a name="how-does-customer-managed-keys-affect-backups"></a>Müşteri tarafından yönetilen anahtarlar yedeklemeleri nasıl etkiler?

Azure Cosmos DB, hesabınızda depolanan verilerin [düzenli ve otomatik yedeklemelerini](./online-backup-and-restore.md) alır. Bu işlem şifrelenmiş verileri yedekler. Geri yüklenen bir yedeklemenin kullanılabilir olması için, yedekleme sırasında kullanılan şifreleme anahtarı yine de kullanılabilir olmalıdır. Bu, hiçbir iptali yapılmadığı ve yedeklemenin yapıldığı sırada kullanılan anahtarın sürümü hala etkinleştirilmeyeceği anlamına gelir.

### <a name="how-do-i-revoke-an-encryption-key"></a>Nasıl yaparım? bir şifreleme anahtarı iptal edilsin mi?

Anahtarın en son sürümü devre dışı bırakılarak anahtar iptali yapılır:

![Anahtarın sürümünü devre dışı bırakma](./media/how-to-setup-cmk/portal-akv-rev2.png)

Alternatif olarak, bir Azure Key Vault örneğinden tüm anahtarları iptal etmek için, Azure Cosmos DB sorumlusuna verilen erişim ilkesini silebilirsiniz:

![Azure Cosmos DB sorumlusu için erişim ilkesi siliniyor](./media/how-to-setup-cmk/portal-akv-rev.png)

### <a name="what-operations-are-available-after-a-customer-managed-key-is-revoked"></a>Müşteri tarafından yönetilen anahtar iptal edildikten sonra hangi işlemler kullanılabilir?

Şifreleme anahtarı iptal edildiğinde mümkün olan tek işlem, hesap silme işlemidir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Cosmos DB 'de veri şifreleme](./database-encryption-at-rest.md) hakkında daha fazla bilgi edinin
- [Cosmos DB verilerine güvenli erişim](secure-access-to-data.md) hakkında genel bakış alın