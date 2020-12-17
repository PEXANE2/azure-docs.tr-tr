---
title: İki kimlik bilgileri kümesi olan kaynaklar için döndürme öğreticisi
description: İki kimlik doğrulama kimlik doğrulaması kümesini kullanan kaynaklar için gizli dizi döndürmeyi nasıl otomatikleştirebileceğinizi öğrenmek için bu öğreticiyi kullanın.
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: rotation
ms.service: key-vault
ms.subservice: secrets
ms.topic: tutorial
ms.date: 06/22/2020
ms.author: jalichwa
ms.openlocfilehash: 097b5c7d71076c11cdc30fce618f3a4ac4ef67a1
ms.sourcegitcommit: ad677fdb81f1a2a83ce72fa4f8a3a871f712599f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97655467"
---
# <a name="automate-the-rotation-of-a-secret-for-resources-that-have-two-sets-of-authentication-credentials"></a>İki kimlik doğrulama kimlik doğrulaması kümesine sahip kaynaklar için gizli dizi döndürmeyi otomatikleştirin

Azure hizmetlerinde kimlik doğrulaması yapmanın en iyi yolu [yönetilen bir kimlik](../general/authentication.md)kullanmaktır, ancak bir seçenek olmadığı durumlarda bazı senaryolar vardır. Bu durumlarda, erişim tuşları veya parolalar kullanılır. Erişim anahtarlarını ve parolaları sıklıkla döndürmelisiniz.

Bu öğreticide, iki kimlik doğrulama kimlik doğrulaması kümesini kullanan veritabanları ve hizmetler için düzenli aralıklarla yapılan gizli dizi döndürmenin nasıl otomatikleştirdiği gösterilmektedir. Özellikle bu öğreticide, Azure Key Vault 'de depolanan Azure depolama hesabı anahtarlarının gizli dizi olarak nasıl döndürüleceğini gösterilmektedir. Azure Event Grid bildirimi tarafından tetiklenen bir işlev kullanacaksınız. 

> [!NOTE]
> Depolama hesabına atanan erişim için paylaşılan erişim imzası belirteçleri sağlarsanız, depolama hesabı anahtarları Key Vault otomatik olarak yönetilebilir. Erişim anahtarlarına sahip depolama hesabı bağlantı dizeleri gerektiren hizmetler vardır. Bu senaryo için bu çözümü öneririz.

Bu öğreticide açıklanan döndürme çözümü aşağıda verilmiştir: 

![Döndürme çözümünü gösteren diyagram.](../media/secrets/rotation-dual/rotation-diagram.png)

Bu çözümde Azure Key Vault, depolama hesabı ayrı erişim anahtarlarını aynı gizli dizinin sürümleri olarak depolar ve sonraki sürümlerde birincil ve ikincil anahtar arasında geçiş yapın. Bir erişim anahtarı gizli dizinin en son sürümünde depolandığında, alternatif anahtar yeniden oluşturulur ve parolanın yeni en son sürümü olarak Key Vault eklenir. Çözüm, en yeni yeniden üretilen anahtara yenilemek için uygulamanın tüm döndürme döngüsünü sağlar. 

1. Bir parolanın süre sonu tarihinden otuz gün önce, Key Vault Event Grid için yakın süre sonu olayını yayınlar.
1. Event Grid olay aboneliklerini denetler ve olaya abone olan işlev uygulama uç noktasını çağırmak için HTTP POST kullanır.
1. İşlev uygulaması, alternatif anahtarı tanımlar (en son olanı değil) ve depolama hesabını yeniden oluşturmak için çağırır.
1. İşlev uygulaması, yeni bir yeniden oluşturulan anahtarı yeni bir parola sürümü olarak Azure Key Vault ekler.

## <a name="prerequisites"></a>Önkoşullar
* Azure aboneliği. [Ücretsiz bir tane oluşturun.](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
* Azure Key Vault.
* İki Azure depolama hesabı.

Mevcut bir anahtar kasası ve mevcut depolama hesaplarınız yoksa, bu dağıtım bağlantısını kullanabilirsiniz:

[![Azure 'a etiketlenmiş bir dağıtım bağlantısı.](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fjlichwa%2FKeyVault-Rotation-StorageAccountKey-PowerShell%2Fmaster%2Farm-templates%2FInitial-Setup%2Fazuredeploy.json)

1. **Kaynak grubu** altında **Yeni oluştur**' u seçin. Grubu **akvdönüşü** olarak adlandırın ve ardından **Tamam**' ı seçin.
1. **Gözden geçir ve oluştur**’u seçin.
1. **Oluştur**’u seçin.

    ![Kaynak grubu oluşturmayı gösteren ekran görüntüsü.](../media/secrets/rotation-dual/dual-rotation-1.png)

Artık bir anahtar kasası ve iki depolama hesabınız olacak. Şu komutu çalıştırarak bu kurulumu Azure CLı 'da doğrulayabilirsiniz:

```azurecli
az resource list -o table -g akvrotation
```

Sonuç şu çıkışa benzer şekilde görünecektir:

```console
Name                     ResourceGroup         Location    Type                               Status
-----------------------  --------------------  ----------  ---------------------------------  --------
akvrotation-kv         akvrotation      eastus      Microsoft.KeyVault/vaults
akvrotationstorage     akvrotation      eastus      Microsoft.Storage/storageAccounts
akvrotationstorage2    akvrotation      eastus      Microsoft.Storage/storageAccounts
```

## <a name="create-and-deploy-the-key-rotation-function"></a>Anahtar döndürme işlevini oluşturma ve dağıtma

Daha sonra, sistem tarafından yönetilen kimliğe sahip bir işlev uygulamasını, diğer gerekli bileşenlere ek olarak oluşturacaksınız. Ayrıca, depolama hesabı anahtarları için döndürme işlevini de dağıtacaksınız.

İşlev uygulaması döndürme işlevi aşağıdaki bileşenleri ve yapılandırmayı gerektirir:
- Azure App Service planı
- İşlev uygulaması tetikleyicilerini yönetmek için bir depolama hesabı
- Key Vault 'de gizli anahtarlara erişim için bir erişim ilkesi
- Depolama hesabı erişim anahtarlarına erişebilmeleri için işlev uygulamasına atanan depolama hesabı anahtar Işletmeni hizmeti rolü
- Olay tetikleyicisi ve HTTP tetikleyicisi (isteğe bağlı döndürme) ile bir anahtar döndürme işlevi
- **Secretyaklaştığında süre sonu** olayı için Event Grid olay aboneliği

1. Azure şablonu dağıtım bağlantısını seçin: 

   [![Azure şablonu dağıtım bağlantısı.](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fjlichwa%2FKeyVault-Rotation-StorageAccountKey-PowerShell%2Fmaster%2Farm-templates%2FFunction%2Fazuredeploy.json)

1. **Kaynak grubu** listesinde, **akvdönüşü**' ni seçin.
1. **Depolama HESABı RG** kutusunda, depolama hesabınızın bulunduğu kaynak grubunun adını girin. Depolama hesabınız, anahtar döndürme işlevini dağıtacağınız kaynak grubunda zaten bulunuyorsa, **[resourceGroup (). Name]** varsayılan değerini koruyun.
1. **Depolama hesabı adı** kutusuna, döndürülecek erişim anahtarlarını içeren depolama hesabının adını girin.
1. **Key Vault RG** kutusuna anahtar kasanızın bulunduğu kaynak grubunun adını girin. Anahtar kasanızın anahtar döndürme işlevini dağıtacağınız kaynak grubunda zaten mevcut olması durumunda **[resourceGroup (). Name]** varsayılan değerini koruyun.
1. **Key Vault adı** kutusuna anahtar kasasının adını girin.
1. **İşlev uygulaması adı** kutusuna, işlev uygulamasının adını girin.
1. Gizli dizi **adı** kutusuna, erişim anahtarlarını depoladığınız parolanın adını girin.
1. **Depo URL 'si** kutusuna, Işlev kodunun GitHub konumunu girin: **https://github.com/jlichwa/KeyVault-Rotation-StorageAccountKey-PowerShell.git** .
1. **Gözden geçir ve oluştur**’u seçin.
1. **Oluştur**’u seçin.

   ![İlk depolama hesabının nasıl oluşturulacağını gösteren ekran görüntüsü.](../media/secrets/rotation-dual/dual-rotation-2.png)

Yukarıdaki adımları tamamladıktan sonra bir depolama hesabınız, bir sunucu grubu, bir işlev uygulaması ve Application Insights olacaktır. Dağıtım tamamlandığında, bu sayfayı görürsünüz: ![ dağıtımınızın tamamlanmış sayfasını gösteren ekran görüntüsü.](../media/secrets/rotation-dual/dual-rotation-3.png)
> [!NOTE]
> Bir hatayla karşılaşırsanız, bileşenlerin dağıtımını sona bırakmak için yeniden **Dağıt** ' ı seçebilirsiniz.


[GitHub](https://github.com/jlichwa/KeyVault-Rotation-StorageAccountKey-PowerShell)üzerinde döndürme işlevi için dağıtım şablonlarını ve kodu bulabilirsiniz.

## <a name="add-the-storage-account-access-keys-to-key-vault"></a>Depolama hesabı erişim anahtarlarını Key Vault ekleyin

İlk olarak, erişim ilkenizi kullanıcılara gizli dizi **yönetme** izinleri vermek için ayarlayın:

```azurecli
az keyvault set-policy --upn <email-address-of-user> --name akvrotation-kv --secret-permissions set delete get list
```

Artık değeri olarak depolama hesabı erişim anahtarı ile yeni bir gizli dizi oluşturabilirsiniz. Ayrıca, döndürme işlevinin depolama hesabındaki anahtarı yeniden oluşturabilmesi için, depolama hesabı kaynak KIMLIĞI, gizli geçerlilik süresi ve anahtar KIMLIĞI ' ne ihtiyacınız olacak.

Depolama hesabı kaynak KIMLIĞINI belirleme. Bu değeri `id` özelliğinde bulabilirsiniz.
```azurecli
az storage account show -n akvrotationstorage
```

Anahtar değerlerini alabilmeniz için depolama hesabı erişim anahtarlarını listeleyin:

```azurecli
az storage account keys list -n akvrotationstorage 
```

Ve için elde edilen değerleri kullanarak bu komutu çalıştırın `key1Value` `storageAccountResourceId` :

```azurecli
$tomorrowDate = (get-date).AddDays(+1).ToString("yyy-MM-ddTHH:mm:ssZ")
az keyvault secret set --name storageKey --vault-name akvrotation-kv --value <key1Value> --tags "CredentialId=key1" "ProviderAddress=<storageAccountResourceId>" "ValidityPeriodDays=60" --expires $tomorrowDate
```

Kısa süre sonu tarihiyle bir gizli dizi oluşturursanız, bir `SecretNearExpiry` olay birkaç dakika içinde yayımlanacak. Bu olay, gizli anahtarı döndürmek için işlevi tetikler.

Depolama hesabı anahtarını ve Key Vault gizliliğini alarak ve bunları karşılaştırarak erişim anahtarlarının yeniden oluşturulduğunu doğrulayabilirsiniz.

Gizli bilgileri almak için bu komutu kullanın:
```azurecli
az keyvault secret show --vault-name akvrotation-kv --name storageKey
```
`CredentialId`Alternatif olarak güncelleştirilmiş ve yeniden oluşturulan `keyName` bir uyarı `value` : ![ ilk depolama hesabı için z keykasası gizli göster komutunun çıkışını gösteren ekran görüntüsü.](../media/secrets/rotation-dual/dual-rotation-4.png)

Değerleri karşılaştırmak için erişim tuşlarını alın:
```azurecli
az storage account keys list -n akvrotationstorage 
```
![İlk depolama hesabı için z depolama hesabı anahtarları listesi komutunun çıkışını gösteren ekran görüntüsü.](../media/secrets/rotation-dual/dual-rotation-5.png)

## <a name="add-storage-accounts-for-rotation"></a>Döndürme için depolama hesapları ekleme

Aynı işlev uygulamasını, birden fazla depolama hesabına yönelik anahtarları döndürmek için yeniden kullanabilirsiniz. 

Döndürme için mevcut bir işleve depolama hesabı anahtarları eklemek için şunlar gerekir:
- Depolama hesabı erişim anahtarlarına erişebilmek için işlev uygulamasına atanan depolama hesabı anahtar Işletmeni hizmeti rolü.
- **Secretyaklaştığında süre sonu** olayı için Event Grid bir olay aboneliği.

1. Azure şablonu dağıtım bağlantısını seçin: 

   [![Azure şablonu dağıtım bağlantısı.](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fjlichwa%2FKeyVault-Rotation-StorageAccountKey-PowerShell%2Fmaster%2Farm-templates%2FAdd-Event-Subscriptions%2Fazuredeploy.json)

1. **Kaynak grubu** listesinde, **akvdönüşü**' ni seçin.
1. **Depolama hesabı adı** kutusuna, döndürülecek erişim anahtarlarını içeren depolama hesabının adını girin.
1. **Key Vault adı** kutusuna anahtar kasasının adını girin.
1. **İşlev uygulaması adı** kutusuna, işlev uygulamasının adını girin.
1. Gizli dizi **adı** kutusuna, erişim anahtarlarını depoladığınız parolanın adını girin.
1. **Gözden geçir ve oluştur**’u seçin.
1. **Oluştur**’u seçin.

   ![Ek depolama hesabı oluşturmayı gösteren ekran görüntüsü.](../media/secrets/rotation-dual/dual-rotation-7.png)

### <a name="add-another-storage-account-access-key-to-key-vault"></a>Key Vault başka bir depolama hesabı erişim anahtarı ekleyin

Depolama hesabı kaynak KIMLIĞINI belirleme. Bu değeri `id` özelliğinde bulabilirsiniz.
```azurecli
az storage account show -n akvrotationstorage2
```

Depolama hesabı erişim anahtarlarını, key2 değerini alabilmeniz için listeleyin:

```azurecli
az storage account keys list -n akvrotationstorage2 
```

Ve için elde edilen değerleri kullanarak bu komutu çalıştırın `key2Value` `storageAccountResourceId` :

```azurecli
tomorrowDate=`date -d tomorrow -Iseconds -u | awk -F'+' '{print $1"Z"}'`
az keyvault secret set --name storageKey2 --vault-name akvrotation-kv --value <key2Value> --tags "CredentialId=key2" "ProviderAddress=<storageAccountResourceId>" "ValidityPeriodDays=60" --expires $tomorrowDate
```

Gizli bilgileri almak için bu komutu kullanın:
```azurecli
az keyvault secret show --vault-name akvrotation-kv --name storageKey2
```
`CredentialId`Alternatif olarak güncelleştirilmiş `keyName` ve yeniden oluşturulan `value` bir uyarı: ![ ikinci depolama hesabı için z keykasası gizli göster komutunun çıkışını gösteren ekran görüntüsü.](../media/secrets/rotation-dual/dual-rotation-8.png)

Değerleri karşılaştırmak için erişim tuşlarını alın:
```azurecli
az storage account keys list -n akvrotationstorage 
```
![İkinci depolama hesabı için z depolama hesabı anahtarları listesi komutunun çıkışını gösteren ekran görüntüsü.](../media/secrets/rotation-dual/dual-rotation-9.png)

## <a name="key-vault-dual-credential-rotation-functions"></a>Key Vault çift kimlik bilgisi döndürme işlevleri

- [Depolama hesabı](https://github.com/jlichwa/KeyVault-Rotation-StorageAccountKey-PowerShell)
- [Redis Cache](https://github.com/jlichwa/KeyVault-Rotation-RedisCacheKey-PowerShell)

## <a name="next-steps"></a>Sonraki adımlar
- Genel Bakış: [Azure Event Grid Key Vault izleme](../general/event-grid-overview.md)
- Nasıl yapılır: [Azure Portal ilk işlevinizi oluşturma](../../azure-functions/functions-create-first-azure-function.md)
- Nasıl yapılır: [Key Vault gizli dizi değiştiğinde e-posta alma](../general/event-grid-logicapps.md)
- Başvuru: [Azure Key Vault için Azure Event Grid olay şeması](../../event-grid/event-schema-key-vault.md)
