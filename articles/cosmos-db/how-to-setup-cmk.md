---
title: Azure Cosmos DB hesabınız için müşteri tarafından yönetilen anahtarları yapılandırın
description: Azure Cosmos DB hesabınız için müşteri tarafından yönetilen anahtarları Azure Key Vault ile nasıl yapılandırabilirsiniz öğrenin
author: ThomasWeiss
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/19/2020
ms.author: thweiss
ROBOTS: noindex, nofollow
ms.openlocfilehash: 8f58887a056c8ca0cd175a44127556562338de38
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81450041"
---
# <a name="configure-customer-managed-keys-for-your-azure-cosmos-account-with-azure-key-vault"></a>Azure Cosmos hesabınız için müşteri tarafından yönetilen anahtarları Azure Key Vault ile yapılandırın

> [!NOTE]
> Şu anda, bu özelliği kullanmak için erişim istemeniz gerekir. Bunu yapmak için [azurecosmosdbcmk@service.microsoft.com](mailto:azurecosmosdbcmk@service.microsoft.com)lütfen .

Azure Cosmos hesabınızda depolanan veriler, Microsoft tarafından yönetilen anahtarlarla **(hizmet tarafından yönetilen anahtarlar)** otomatik olarak ve sorunsuz bir şekilde şifrelenir. İsteğe bağlı olarak, yönettiğiniz anahtarlarla **(müşteri tarafından yönetilen anahtarlar)** ikinci bir şifreleme katmanı eklemeyi seçebilirsiniz.

![Müşteri verileri etrafında şifreleme katmanları](./media/how-to-setup-cmk/cmk-intro.png)

Müşteri tarafından yönetilen anahtarları [Azure Anahtar Kasası'nda](../key-vault/general/overview.md) depolamalı ve müşteri tarafından yönetilen anahtarlarla etkinleştirilen her Azure Cosmos hesabı için bir anahtar sağlamanız gerekir. Bu anahtar, bu hesapta depolanan tüm verileri şifrelemek için kullanılır.

> [!NOTE]
> Şu anda, müşteri tarafından yönetilen anahtarlar yalnızca yeni Azure Cosmos hesapları için kullanılabilir. Bunları hesap oluşturma sırasında yapılandırmanız gerekir.

## <a name="register-the-azure-cosmos-db-resource-provider-for-your-azure-subscription"></a><a id="register-resource-provider"></a>Azure aboneliğiniz için Azure Cosmos DB kaynak sağlayıcısını kaydedin

1. [Azure portalında](https://portal.azure.com/)oturum açın, Azure aboneliğinize gidin ve **Ayarlar** sekmesi altında **Kaynak sağlayıcılarını** seçin:

   ![Sol menüden "kaynak sağlayıcılar" girişi](./media/how-to-setup-cmk/portal-rp.png)

1. **Microsoft.DocumentDB** kaynak sağlayıcısını arayın. Kaynak sağlayıcısının zaten kayıtlı olarak işaretli olup olmadığını doğrulayın. Değilse, kaynak sağlayıcı seçin ve **Kayıt**seçin:

   ![Microsoft.DocumentDB kaynak sağlayıcısını kaydetme](./media/how-to-setup-cmk/portal-rp-register.png)

## <a name="configure-your-azure-key-vault-instance"></a>Azure Anahtar Kasası örneğini yapılandırma

Azure Cosmos DB ile müşteri tarafından yönetilen anahtarları kullanmak, şifreleme anahtarlarınızı barındırmak için kullanmayı planladığınız Azure Anahtar Kasası örneğinde iki özellik ayarlamanızı gerektirir. Bu özellikler **Yumuşak Silme** ve Temizleme **yok**içerir. Bu özellikler varsayılan olarak etkinleştirilir. Bunları PowerShell veya Azure CLI'yi kullanarak etkinleştirebilirsiniz.

Bu özellikleri varolan bir Azure Anahtar Kasası örneğinde nasıl etkinleştireceklerini öğrenmek için aşağıdaki makalelerden birinde "Yumuşak silmeyi etkinleştirme" ve "Temizleme Koruması etkinleştirme" bölümlerine bakın:

- [PowerShell ile yumuşak silme nasıl kullanılır?](../key-vault/general/soft-delete-powershell.md)
- [Azure CLI ile yumuşak silme nasıl kullanılır?](../key-vault/general/soft-delete-cli.md)

## <a name="add-an-access-policy-to-your-azure-key-vault-instance"></a>Azure Anahtar Kasası örneğinize bir erişim ilkesi ekleme

1. Azure portalından, şifreleme anahtarlarınızı barındırmak için kullanmayı planladığınız Azure Anahtar Kasası örneğine gidin. Sol menüden **Erişim İlkeleri'ni** seçin:

   ![Sol menüden "Erişim ilkeleri"](./media/how-to-setup-cmk/portal-akv-ap.png)

1. Access **İlkesi Ekle +** Seçin.

1. Anahtar **izinleri** açılır menüsü altında, **Al**, Aç Tuşu ve Anahtar İzinlerini **Sve'** yi seçin: **Wrap Key**

   ![Doğru izinleri seçme](./media/how-to-setup-cmk/portal-akv-add-ap-perm2.png)

1. **Select principal**altında, **Seçili Yok'u**seçin. Ardından, **Azure Cosmos DB** sorumlusunu arayın ve seçin (bulmayı kolaylaştırmak için, asıl `a232010e-820c-4083-83bb-3ace5fc29d0b` kimliğin bulunduğu Azure Devlet bölgeleri dışındaki `57506a73-e302-42a9-b869-6f12d9ec29e9`herhangi bir Azure bölgesi için asıl kimlikle de arama yapabilirsiniz). Son olarak, en altta **Seç'i** seçin. Azure **Cosmos DB** ilkesi listede yoksa, **microsoft.documentdb** kaynak sağlayıcısını bu [makalenin kaynak sağlayıcısını kaydedin](#register-resource-provider) bölümünde açıklandığı şekilde yeniden kaydetmeniz gerekebilir.

   ![Azure Cosmos DB ilkesini seçin](./media/how-to-setup-cmk/portal-akv-add-ap.png)

1. Yeni erişim ilkesini eklemek için **Ekle'yi** seçin.

## <a name="generate-a-key-in-azure-key-vault"></a>Azure Anahtar Kasası'nda anahtar oluşturma

1. Azure portalından, şifreleme anahtarlarınızı barındırmak için kullanmayı planladığınız Azure Anahtar Kasası örneğine gidin. Ardından, sol menüden **Tuşlar'ı** seçin:

   ![Sol menüden "Tuşlar" girişi](./media/how-to-setup-cmk/portal-akv-keys.png)

1. **Oluştur/İçe Aktar'ı**seçin, yeni anahtar için bir ad sağlayın ve rsa anahtar boyutu seçin. En iyi güvenlik için en az 3072 önerilir. Sonra **Oluştur'u**seçin:

   ![Yeni bir anahtar oluşturma](./media/how-to-setup-cmk/portal-akv-gen.png)

1. Anahtar oluşturulduktan sonra, yeni oluşturulan anahtarı ve ardından geçerli sürümünü seçin.

1. Anahtarın Anahtar **Tanımlayıcısını**kopyalayın , son ileri eğik çizgiden sonraki kısım hariç:

   ![Anahtarın anahtar tanımlayıcısını kopyalama](./media/how-to-setup-cmk/portal-akv-keyid.png)

## <a name="create-a-new-azure-cosmos-account"></a>Yeni bir Azure Cosmos hesabı oluşturma

### <a name="using-the-azure-portal"></a>Azure portalını kullanma

Azure portalından yeni bir Azure Cosmos DB hesabı oluşturduğunuzda, **Şifreleme** adımında **Müşteri tarafından yönetilen anahtarı** seçin. Temel **URI** alanında, önceki adımdan kopyaladığınız Azure Anahtar Kasası anahtarının URI/anahtar tanımlayıcısını yapıştırın:

![Azure portalında CMK parametrelerini ayarlama](./media/how-to-setup-cmk/portal-cosmos-enc.png)

### <a name="using-azure-powershell"></a>Azure PowerShell’i kullanma

PowerShell ile yeni bir Azure Cosmos DB hesabı oluşturduğunuzda:

- Daha önce **PropertyObject'teki** **VaultKeyUri anahtarı** altında kopyalanan Azure Key Vault anahtarının URI'sini geçirin.

- **2019-12-12'yi** API sürümü olarak kullanın.

> [!IMPORTANT]
> Hesabın müşteri `Location` tarafından yönetilen anahtarlarla başarıyla oluşturulabilmesi için parametreyi açıkça ayarlamanız gerekir.

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

### <a name="using-an-azure-resource-manager-template"></a>Azure Kaynak Yöneticisi şablonu kullanma

Azure Kaynak Yöneticisi şablonu aracılığıyla yeni bir Azure Cosmos hesabı oluşturduğunuzda:

- **Özellikler** nesnesindeki **VaultKeyUri anahtarıaltında** daha önce kopyaladığınız Azure Key Vault anahtarının URI'sini geçirin.

- **2019-12-12'yi** API sürümü olarak kullanın.

> [!IMPORTANT]
> Hesabın müşteri `Location` tarafından yönetilen anahtarlarla başarıyla oluşturulabilmesi için parametreyi açıkça ayarlamanız gerekir.

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

Şablonu aşağıdaki PowerShell komut dosyasıyla dağıtın:

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

### <a name="using-the-azure-cli"></a>Azure CLI kullanma

Azure CLI üzerinden yeni bir Azure Cosmos hesabı oluşturduğunuzda, daha önce kopyaladığınız Azure Anahtar Kasası anahtarının URI'sini **anahtar-uri** parametresi altında geçirin.

```azurecli-interactive
resourceGroupName='myResourceGroup'
accountName='mycosmosaccount'
keyVaultKeyUri = 'https://<my-vault>.vault.azure.net/keys/<my-key>'

az cosmosdb create \
    -n $accountName \
    -g $resourceGroupName \
    --locations regionName='West US 2' failoverPriority=0 isZoneRedundant=False \
    --key-uri $keyVaultKeyUri
```

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="is-there-any-additional-charge-for-using-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarları kullanmak için ek ücret var mı?

Evet. Veri şifrelemeve şifre çözmeyi müşteri tarafından yönetilen anahtarlarla yönetmek için gereken ek bilgi işlem yükünü hesaba katmak için, Azure Cosmos hesabına karşı yürütülen tüm [işlemler, İstek Birimlerinde](./request-units.md)yüzde 25'lik bir artış tüketir.

### <a name="what-data-gets-encrypted-with-the-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarlarla hangi veriler şifrelenir?

Azure Cosmos hesabınızda depolanan tüm veriler, aşağıdaki meta veriler dışında müşteri tarafından yönetilen anahtarlarla şifrelenir:

- Azure Cosmos DB [hesaplarınızın, veritabanlarınızın ve kapsayıcılarınızın](./account-overview.md#elements-in-an-azure-cosmos-account) adları

- [Depolanan yordamlarınızın](./stored-procedures-triggers-udfs.md) adları

- [Dizin oluşturma ilkelerinizde](./index-policy.md) bildirilen özellik yolları

- Kapsayıcılarınızın [bölüm anahtarlarının](./partitioning-overview.md) değerleri

### <a name="are-customer-managed-keys-supported-for-existing-azure-cosmos-accounts"></a>Müşteri tarafından yönetilen anahtarlar, mevcut Azure Cosmos hesapları için desteklenir mi?

Bu özellik şu anda yalnızca yeni hesaplar için kullanılabilir.

### <a name="is-there-a-plan-to-support-finer-granularity-than-account-level-keys"></a>Hesap düzeyi anahtarlardan daha ince parçalılığı desteklemek için bir plan var mı?

Şu anda değil, ancak konteyner düzeyi anahtarları dikkate alınmaktadır.

### <a name="how-do-customer-managed-keys-affect-a-backup"></a>Müşteri tarafından yönetilen anahtarlar yedeklemeyi nasıl etkiler?

Azure Cosmos DB, hesabınızda depolanan verilerin [düzenli ve otomatik yedeklemelerini](./online-backup-and-restore.md) alır. Bu işlem, şifrelenmiş verileri yedekler. Geri yüklenen yedeklemeyi kullanmak için yedekleme sırasında kullandığınız şifreleme anahtarı gereklidir. Bu, iptal yapılmadığı ve yedekleme sırasında kullanılan anahtarın sürümünün yine de etkinleştirileceği anlamına gelir.

### <a name="how-do-i-revoke-an-encryption-key"></a>Şifreleme anahtarını nasıl iptal edebilirim?

Anahtar iptali anahtarın en son sürümünü devre dışı bırakarak yapılır:

![Anahtarın sürümünü devre dışı](./media/how-to-setup-cmk/portal-akv-rev2.png)

Alternatif olarak, bir Azure Anahtar Kasası örneğindeki tüm anahtarları iptal etmek için Azure Cosmos DB ilkesine verilen erişim ilkesini silebilirsiniz:

![Azure Cosmos DB ilkesinin erişim ilkesini silme](./media/how-to-setup-cmk/portal-akv-rev.png)

### <a name="what-operations-are-available-after-a-customer-managed-key-is-revoked"></a>Müşteri tarafından yönetilen bir anahtar iptal edildikten sonra hangi işlemler kullanılabilir?

Şifreleme anahtarı iptal edildiğinde mümkün olan tek işlem hesap silme işlemidir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Cosmos DB'de veri şifreleme](./database-encryption-at-rest.md)hakkında daha fazla bilgi edinin.
- [Cosmos DB'deki verilere güvenli erişime](secure-access-to-data.md)genel bir bakış alın.
