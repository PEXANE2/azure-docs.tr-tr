---
title: Azure Cosmos DB hesabınız için müşteri tarafından yönetilen anahtarları yapılandırın
description: Azure Cosmos DB hesabınız için müşteri tarafından yönetilen anahtarları Azure Key Vault ile yapılandırma hakkında bilgi edinin
author: ThomasWeiss
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 01/14/2020
ms.author: thweiss
ROBOTS: noindex, nofollow
ms.openlocfilehash: 44bbd7eab80ecb1cbfef9738e42b4070dff31180
ms.sourcegitcommit: 934776a860e4944f1a0e5e24763bfe3855bc6b60
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/20/2020
ms.locfileid: "77506051"
---
# <a name="configure-customer-managed-keys-for-your-azure-cosmos-account-with-azure-key-vault"></a>Azure Key Vault ile Azure Cosmos hesabınız için müşteri tarafından yönetilen anahtarlar yapılandırın

> [!NOTE]
> Şu anda bu özelliği kullanmak için erişim istemeniz gerekir. Bunu yapmak için lütfen [azurecosmosdbcmk@service.microsoft.com](mailto:azurecosmosdbcmk@service.microsoft.com)başvurun.

Azure Cosmos hesabınızda depolanan veriler otomatik olarak ve sorunsuz bir şekilde şifrelenir. Azure Cosmos DB, bekleyen verileri şifrelemek için kullanılan anahtarları yönetmek için iki seçenek sunar:

- **Hizmet tarafından yönetilen anahtarlar**: varsayılan olarak, Microsoft, Azure Cosmos hesabınızdaki verileri şifrelemek için kullanılan anahtarları yönetir.

- **Müşteri tarafından yönetilen anahtarlar (CMK)** : isteğe bağlı olarak, kendi anahtarlarınızın ikinci bir şifreleme katmanını eklemeyi seçebilirsiniz.

Müşteri tarafından yönetilen anahtarları [Azure Key Vault](../key-vault/key-vault-overview.md) depolamanız ve müşterinin yönettiği anahtarlarla etkinleştirilen her bir Azure Cosmos hesabı için bir anahtar sağlamanız gerekir. Bu anahtar, bu hesapta depolanan tüm verileri şifrelemek için kullanılır.

> [!NOTE]
> Şu anda, müşteri tarafından yönetilen anahtarlar yalnızca yeni Azure Cosmos hesapları için kullanılabilir. Hesap oluşturma sırasında bunları yapılandırmanız gerekir.

## <a id="register-resource-provider"></a>Azure aboneliğiniz için Azure Cosmos DB kaynak sağlayıcısını kaydetme

1. [Azure Portal](https://portal.azure.com/)oturum açın, Azure aboneliğinize gidin ve **Ayarlar** sekmesinde **kaynak sağlayıcıları** ' nı seçin:

   ![Sol menüden "kaynak sağlayıcıları" girdisi](./media/how-to-setup-cmk/portal-rp.png)

1. **Microsoft. DocumentDB** kaynak sağlayıcısı için arama yapın. Kaynak sağlayıcının zaten kayıtlı olarak işaretlendiğinden emin olun. Aksi takdirde, kaynak sağlayıcısını seçip **Kaydet**' i seçin:

   ![Microsoft. DocumentDB kaynak sağlayıcısını kaydetme](./media/how-to-setup-cmk/portal-rp-register.png)

## <a name="configure-your-azure-key-vault-instance"></a>Azure Key Vault örneğinizi yapılandırma

Azure Cosmos DB ile müşteri tarafından yönetilen anahtarların kullanılması, şifreleme anahtarlarınızı barındırmak için kullanmayı planladığınız Azure Key Vault örneğinde iki özellik ayarlamanızı gerektirir. Bu özellikler, **geçici silme** ve **Temizleme işlemi**içerir. Bu özellikler varsayılan olarak etkin değildir. PowerShell veya Azure CLı kullanarak bunları etkinleştirebilirsiniz.

Mevcut bir Azure Key Vault örneğinde bu özellikleri nasıl etkinleştireceğinizi öğrenmek için, aşağıdaki makalelerden birinde "geçici silme özelliğini etkinleştirme" ve "Temizleme korumasını etkinleştirme" bölümlerine bakın:

- [PowerShell ile geçici silmeyi kullanma](../key-vault/key-vault-soft-delete-powershell.md)
- [Azure CLı ile geçici silme kullanma](../key-vault/key-vault-soft-delete-cli.md)

## <a name="add-an-access-policy-to-your-azure-key-vault-instance"></a>Azure Key Vault örneğine bir erişim ilkesi ekleme

1. Azure portal, şifreleme anahtarlarınızı barındırmak için kullanmayı planladığınız Azure Key Vault örneğine gidin. Sol menüden **erişim ilkeleri** ' ni seçin:

   ![Sol menüden "erişim ilkeleri"](./media/how-to-setup-cmk/portal-akv-ap.png)

1. **+ Erişim Ilkesi Ekle**' yi seçin.

1. **Anahtar izinleri** aşağı açılan menüsünde **Al**, **sarmalama tuşu**ve **sarmalama anahtarı** izinlerini seçin:

   ![Doğru izinleri seçme](./media/how-to-setup-cmk/portal-akv-add-ap-perm2.png)

1. **Asıl seçin**altında **hiçbiri seçili**' i seçin. Daha sonra, **Azure Cosmos DB** sorumlusu arayıp seçin (bulmayı kolaylaştırmak için, asıl kimlik `57506a73-e302-42a9-b869-6f12d9ec29e9`olan Azure Kamu bölgeleri hariç), asıl kimliğe göre de arama yapabilirsiniz: `a232010e-820c-4083-83bb-3ace5fc29d0b`. Son olarak, en altta **Seç** ' i seçin. **Azure Cosmos DB** sorumlusu listede yoksa, bu makalenin [kaynak sağlayıcısını kaydetme](#register-resource-provider) bölümünde açıklandığı gibi **Microsoft. DocumentDB** kaynak sağlayıcısını yeniden kaydetmeniz gerekebilir.

   ![Azure Cosmos DB sorumlusu seçin](./media/how-to-setup-cmk/portal-akv-add-ap.png)

1. Yeni erişim ilkesini eklemek için **Ekle** ' yi seçin.

## <a name="generate-a-key-in-azure-key-vault"></a>Azure Key Vault bir anahtar oluşturun

1. Azure portal, şifreleme anahtarlarınızı barındırmak için kullanmayı planladığınız Azure Key Vault örneğine gidin. Ardından, sol menüden **anahtarlar** ' ı seçin:

   ![Sol menüden "anahtarlar" girdisi](./media/how-to-setup-cmk/portal-akv-keys.png)

1. **Oluştur/al**' ı seçin, yeni anahtar için bir ad girin ve bir RSA anahtar boyutu seçin. En iyi güvenlik için en az 3072 önerilir. Sonra **Oluştur**' u seçin.

   ![Yeni anahtar oluştur](./media/how-to-setup-cmk/portal-akv-gen.png)

1. Anahtar oluşturulduktan sonra, yeni oluşturulan anahtarı ve ardından geçerli sürümünü seçin.

1. Son eğik çizgiden sonraki bölüm dışında anahtarın **anahtar tanımlayıcısını**kopyalayın:

   ![Anahtarın anahtar tanımlayıcısı kopyalanıyor](./media/how-to-setup-cmk/portal-akv-keyid.png)

## <a name="create-a-new-azure-cosmos-account"></a>Yeni bir Azure Cosmos hesabı oluşturun

### <a name="using-the-azure-portal"></a>Azure portalını kullanma

Azure portal yeni bir Azure Cosmos DB hesabı oluşturduğunuzda, **şifreleme** adımında **müşteri tarafından yönetilen anahtar** ' ı seçin. **Anahtar URI 'si** alanında, önceki adımdan kopyaladığınız Azure Key Vault anahtarının URI/anahtar tanımlayıcısını yapıştırın:

![Azure portal CMK parametrelerini ayarlama](./media/how-to-setup-cmk/portal-cosmos-enc.png)

### <a name="using-azure-powershell"></a>Azure PowerShell’i kullanma

PowerShell ile yeni bir Azure Cosmos DB hesabı oluşturduğunuzda:

- Daha önce, **Propertyobject**Içindeki **keyvaultkeyuri** özelliğinin ALTıNA kopyalanmış Azure Key Vault anahtarının URI 'sini geçirin.

- API sürümü olarak **2019-12-12** kullanın.

> [!IMPORTANT]
> Hesap için `Location` parametresini, müşteri tarafından yönetilen anahtarlarla başarıyla oluşturulacak şekilde ayarlamanız gerekir.

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

### <a name="using-an-azure-resource-manager-template"></a>Azure Resource Manager şablonu kullanma

Azure Resource Manager şablonuyla yeni bir Azure Cosmos hesabı oluşturduğunuzda:

- Daha önce, **Properties** nesnesine **keyvaultkeyuri** özelliği altında kopyaladığınız Azure Key Vault anahtarının URI 'sini geçirin.

- API sürümü olarak **2019-12-12** kullanın.

> [!IMPORTANT]
> Hesap için `Location` parametresini, müşteri tarafından yönetilen anahtarlarla başarıyla oluşturulacak şekilde ayarlamanız gerekir.

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

Şablonu aşağıdaki PowerShell betiği ile dağıtın:

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

### <a name="is-there-any-additional-charge-for-using-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarları kullanmak için ek ücret var mı?

Evet. Müşteri tarafından yönetilen anahtarlarla veri şifrelemeyi ve şifre çözmeyi yönetmek için gereken ek işlem yükünü hesaba eklemek için, Azure Cosmos hesabına karşı yürütülen tüm işlemler, [Istek birimlerinde](./request-units.md)yüzde 25 artışını kullanır.

### <a name="what-data-gets-encrypted-with-the-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarlarla hangi veriler şifrelenir?

Azure Cosmos hesabınızda depolanan tüm veriler, aşağıdaki meta veriler dışında müşteri tarafından yönetilen anahtarlarla şifrelenir:

- Azure Cosmos DB [hesaplarınızın, veritabanlarının ve kapsayıcılarınızın](./account-overview.md#elements-in-an-azure-cosmos-account) adları

- [Saklı yordamlarınızın](./stored-procedures-triggers-udfs.md) adları

- [Dizin oluşturma ilkeleriniz](./index-policy.md) içinde belirtilen özellik yolları

- Kapsayıcılarınızın [bölüm anahtarlarının](./partitioning-overview.md) değerleri

### <a name="are-customer-managed-keys-supported-for-existing-azure-cosmos-accounts"></a>Mevcut Azure Cosmos hesapları için müşteri tarafından yönetilen anahtarlar destekleniyor mu?

Bu özellik şu anda yalnızca yeni hesaplar için kullanılabilir.

### <a name="is-there-a-plan-to-support-finer-granularity-than-account-level-keys"></a>Hesap düzeyi anahtarlardan daha ince ayrıntı düzeyi desteklemeye yönelik bir plan var mı?

Şu anda değil, ancak kapsayıcı düzeyi anahtarlar kabul ediliyor.

### <a name="how-do-customer-managed-keys-affect-a-backup"></a>Müşteri tarafından yönetilen anahtarlar yedeklemeyi nasıl etkiler?

Azure Cosmos DB, hesabınızda depolanan verilerin [düzenli ve otomatik yedeklemelerini](./online-backup-and-restore.md) alır. Bu işlem şifrelenmiş verileri yedekler. Geri yüklenen yedeklemeyi kullanmak için, yedekleme sırasında kullandığınız şifreleme anahtarı gereklidir. Bu, hiçbir iptali yapılmadığı ve yedeklemenin yapıldığı sırada kullanılan anahtarın sürümünün etkinleştirilmeyeceği anlamına gelir.

### <a name="how-do-i-revoke-an-encryption-key"></a>Nasıl yaparım? bir şifreleme anahtarı iptal edilsin mi?

Anahtarın en son sürümü devre dışı bırakılarak anahtar iptali yapılır:

![Anahtarın sürümünü devre dışı bırakma](./media/how-to-setup-cmk/portal-akv-rev2.png)

Alternatif olarak, bir Azure Key Vault örneğinden tüm anahtarları iptal etmek için, Azure Cosmos DB sorumlusuna verilen erişim ilkesini silebilirsiniz:

![Azure Cosmos DB sorumlusu için erişim ilkesi siliniyor](./media/how-to-setup-cmk/portal-akv-rev.png)

### <a name="what-operations-are-available-after-a-customer-managed-key-is-revoked"></a>Müşteri tarafından yönetilen anahtar iptal edildikten sonra hangi işlemler kullanılabilir?

Şifreleme anahtarı iptal edildiğinde mümkün olan tek işlem, hesap silme işlemidir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Cosmos DB 'de veri şifreleme](./database-encryption-at-rest.md)hakkında daha fazla bilgi edinin.
- [Cosmos DB verilerine güvenli erişim](secure-access-to-data.md)hakkında genel bakış alın.
