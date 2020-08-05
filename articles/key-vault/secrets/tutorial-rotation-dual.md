---
title: İki kimlik bilgileri kümesi olan kaynaklar için döndürme öğreticisi
description: İki kimlik doğrulama kimlik doğrulaması kümesini kullanan kaynaklar için gizli dizi döndürmeyi nasıl otomatikleştirebileceğinizi öğrenmek için bu öğreticiyi kullanın.
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: rotation
ms.service: key-vault
ms.subservice: general
ms.topic: tutorial
ms.date: 06/22/2020
ms.author: jalichwa
ms.openlocfilehash: 0d2ee8fbcb71d8703702f2c72e0bf629563667b9
ms.sourcegitcommit: 8def3249f2c216d7b9d96b154eb096640221b6b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2020
ms.locfileid: "87542204"
---
# <a name="automate-the-rotation-of-a-secret-for-resources-with-two-sets-of-authentication-credentials"></a>İki kimlik doğrulama kimlik kümesi içeren kaynaklar için gizli dizi döndürmeyi otomatikleştirin

Azure hizmetlerinde kimlik doğrulaması yapmanın en iyi yolu [yönetilen bir kimlik](../general/managed-identity.md)kullanmaktır, ancak bir seçenek olmadığı durumlarda bazı senaryolar vardır. Bu durumlarda, erişim tuşları veya parolalar kullanılır. Erişim anahtarları ve parolalar sıklıkla döndürülmelidir.

Bu öğreticide, iki kimlik doğrulama kimlik doğrulaması kümesini kullanan veritabanları ve hizmetler için düzenli aralıklarla yapılan gizli dizi döndürmenin nasıl otomatikleştirdiği gösterilmektedir. Özellikle, bu öğretici, Azure Event Grid bildirimi tarafından tetiklenen bir işlevi kullanarak Azure Key Vault depolanan Azure depolama hesabı anahtarlarını, gizli dizi olarak döndürür. :

> [!NOTE]
> Depolama hesabı anahtarları, depolama hesabına temsilci erişimi için paylaşılan erişim imza belirteçleri sağlanarak Key Vault otomatik olarak yönetilebilir. Erişim anahtarı ile depolama hesabı bağlantı dizesi gerektiren hizmetler vardır ve bu senaryo için bu çözüm önerilir

![Döndürme çözümünün diyagramı](../media/secrets/rotation-dual/rotation-diagram.png)

Yukarıdaki çözümde, Azure Key Vault depolama hesabı ayrı erişim anahtarlarını, sonraki sürümlerde birincil ve ikincil anahtar arasında değişen aynı gizli dizi sürümleri olarak depolar. Tek bir erişim anahtarı gizli dizinin en son sürümünde depolandığından, alternatif anahtar yeniden oluşturulur ve parolanın yeni ve en son sürümü olarak Key Vault eklenir. Bu çözüm, en yeni yeniden üretilen anahtar için yenileme için tüm geçiş döngüsünü uygulamalar sağlar. 

1. bir parolanın süresi dolduktan 30 gün önce, Key Vault Event Grid "sona erme tarihi" olayını yayınlar.
1. Event Grid olay aboneliklerini denetler ve olaya abone olan işlev uygulama uç noktasını çağırmak için HTTP POST kullanır.
1. İşlev uygulaması alternatif anahtarı (en son dışında) tanımlar ve depolama hesabı 'nı yeniden oluşturmak için çağırır
1. İşlev uygulaması, yeni bir parola Azure Key Vault için yeni bir anahtar ekler.

## <a name="prerequisites"></a>Önkoşullar
* Bir Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Azure Key Vault
* İki Azure depolama hesabı

Mevcut anahtar kasası ve depolama hesaplarınız yoksa, aşağıdaki dağıtım bağlantısı kullanılabilir:

[!["Azure 'a dağıt" etiketli bir düğmeyi gösteren resim.](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fjlichwa%2FKeyVault-Rotation-StorageAccountKey-PowerShell%2Fmaster%2Farm-templates%2FInitial-Setup%2Fazuredeploy.json)

1. **Kaynak grubu**altında **Yeni oluştur**' u seçin. Grubu **akvdönüşü** olarak adlandırın ve **Tamam**' a tıklayın.
1. **Gözden geçir + oluştur**' u seçin.
1. **Oluştur**’u seçin

    ![Kaynak grubu oluşturma](../media/secrets/rotation-dual/dual-rotation-1.png)

Artık bir anahtar kasası ve iki depolama hesabınız olacak. Aşağıdaki komutu çalıştırarak bu kurulumu Azure CLı 'da doğrulayabilirsiniz:

```azurecli
az resource list -o table -g akvrotation
```

Sonuç aşağıdaki çıktıyı görür:

```console
Name                     ResourceGroup         Location    Type                               Status
-----------------------  --------------------  ----------  ---------------------------------  --------
akvrotation-kv         akvrotation      eastus      Microsoft.KeyVault/vaults
akvrotationstorage     akvrotation      eastus      Microsoft.Storage/storageAccounts
akvrotationstorage2    akvrotation      eastus      Microsoft.Storage/storageAccounts
```

## <a name="create-and-deploy-storage-account-key-rotation-function"></a>Depolama hesabı anahtar döndürme işlevi oluşturma ve dağıtma

Daha sonra, diğer gerekli bileşenlere ek olarak, sistem tarafından yönetilen kimliğe sahip bir işlev uygulaması oluşturun ve depolama hesabı anahtar döndürme işlevlerini dağıtın

İşlev uygulaması döndürme işlevleri bu bileşenleri ve yapılandırmayı gerektirir:
- Azure App Service planı
- İşlev uygulaması tetikleyici yönetimi için gerekli bir depolama hesabı
- Key Vault 'de gizli anahtarlara erişim için bir erişim ilkesi
- Depolama hesabı erişim anahtarlarına erişmek için işlev uygulamasına depolama hesabı anahtar Işletmeni hizmeti rolü atama
- Olay tetikleyicisi ve http tetikleyicisi ile depolama hesabı anahtar döndürme işlevleri (isteğe bağlı döndürme)
- **Secretyaklaştığında süre sonu** olayı Için eventgrid olay aboneliği

1. Azure şablonu dağıtım bağlantısını seçin: 

   [!["Azure 'a dağıt" etiketli bir düğmeyi gösteren resim.](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fjlichwa%2FKeyVault-Rotation-StorageAccountKey-PowerShell%2Fmaster%2Farm-templates%2FInitial-Setup%2Fazuredeploy.json)

1. **Kaynak grubu** listesinde, **akvdönüşü**' ni seçin.
1. **Depolama hesabı adı**alanına, döndürme için erişim anahtarlarına sahip depolama hesabı adını yazın
1. **Key Vault adına**, Anahtar Kasası adını yazın
1. **İşlev uygulaması adına**, işlev uygulama adını yazın
1. **Gizli adı**alanına erişim anahtarlarının depolanacağı gizli adı yazın
1. **Depo URL 'si**içinde, Işlev kodu GitHub Location () yazın **https://github.com/jlichwa/KeyVault-Rotation-StorageAccountKey-PowerShell.git**
1. **Gözden geçir + oluştur**' u seçin.
1. **Oluştur**’u seçin

   ![Gözden geçir + oluştur](../media/secrets/rotation-dual/dual-rotation-2.png)

Yukarıdaki adımları tamamladıktan sonra bir depolama hesabınız, bir sunucu grubu, bir işlev uygulaması, Application Insights vardır. Dağıtım tamamlandıktan sonra aşağıdaki ekranı görmeniz gerekir: ![ dağıtım tamamlandı](../media/secrets/rotation-dual/dual-rotation-3.png)
> [!NOTE]
> Herhangi bir başarısızlık durumunda kalan bileşenlerin dağıtımını bitirebilmeniz için yeniden **Dağıt** ' a tıklayabilirsiniz.


Dağıtım şablonları ve döndürme işlevleri kodu [GitHub](https://github.com/jlichwa/KeyVault-Rotation-StorageAccountKey-PowerShell)'da bulunabilir.

## <a name="add-storage-account-access-key-to-key-vault"></a>Key Vault depolama hesabı erişim anahtarı Ekle

İlk olarak, erişim ilkenizi kullanıcılara gizli dizi *yönetme* izinleri vermek için ayarlayın:

```azurecli
az keyvault set-policy --upn <email-address-of-user> --name akvrotation-kv --secret-permissions set delete get list
```

Artık bir depolama hesabı erişim anahtarı olarak değer olarak yeni bir gizli dizi oluşturabilirsiniz. Ayrıca, depolama hesabı kaynak KIMLIĞI, gizli dizi geçerlilik süresi ve gizli dizi için anahtar KIMLIĞI gerekir, bu nedenle döndürme işlevi depolama hesabında anahtarı yeniden oluşturabilir.

Depolama hesabı kaynak KIMLIĞINI alın. Değer, özellik altında bulunabilir `id`
```azurecli
az storage account show -n akvrotationstorage
```

Anahtar değerlerini almak için depolama hesabı erişim anahtarlarını listeleyin

```azurecli
az storage account keys list -n akvrotationstorage 
```

**Key1Value** ve **storageaccountresourceıd** için alınan değerleri doldur

```azurecli
$tomorrowDate = (get-date).AddDays(+1).ToString("yyy-MM-ddThh:mm:ssZ")
az keyvault secret set --name storageKey --vault-name akvrotation-kv --value <key1Value> --tags "CredentialId=key1" "ProviderAddress=<storageAccountResourceId>" "ValidityPeriodDays=60" --expires $tomorrowDate
```

Kısa süre sonu tarihiyle gizli `SecretNearExpiry` dizi oluşturmak birkaç dakika içinde bir olay yayımlar, bu da bir olayı, gizli anahtarı döndürmek için tetikleyecektir.

Erişim anahtarlarının, depolama hesabı anahtarlarını alarak ve karşılaştırarak ve gizli dizi Key Vault yeniden oluşturulduğunu doğrulayabilirsiniz.

Aşağıdaki komutu kullanarak gizli bilgileri gösterebilirsiniz:
```azurecli
az keyvault secret show --vault-name akvrotation-kv --name storageKey
```
`CredentialId`Alternatif olarak güncellendiğine `keyName` ve gizli dizi `value` tarafından yeniden üretildiğine dikkat edin ![](../media/secrets/rotation-dual/dual-rotation-4.png)

Değeri doğrulamak için erişim tuşlarını al
```azurecli
az storage account keys list -n akvrotationstorage 
```
![Erişim anahtarı listesi](../media/secrets/rotation-dual/dual-rotation-5.png)

## <a name="add-additional-storage-accounts-for-rotation"></a>Döndürme için ek depolama hesapları ekleme

Birden çok depolama hesabını döndürmek için aynı işlev uygulaması yeniden kullanılabilir. 

Mevcut işleve dönme için ek depolama hesabı anahtarları eklenmesi şunları gerektirir:
- Depolama hesabı erişim anahtarlarına erişmek için işlev uygulamasına depolama hesabı anahtar Işletmeni hizmeti rolü atama
- **Secretyaklaştığında süre sonu** olayı Için eventgrid olay aboneliği

1. Azure şablonu dağıtım bağlantısını seçin: 

   [!["Azure 'a dağıt" etiketli bir düğmeyi gösteren resim.](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fjlichwa%2FKeyVault-Rotation-StorageAccountKey-PowerShell%2Fmaster%2Farm-templates%2FAdd-Event-Subscription%2Fazuredeploy.json)

1. **Kaynak grubu** listesinde, **akvdönüşü**' ni seçin.
1. **Depolama hesabı adı**alanına, döndürme için erişim anahtarlarına sahip depolama hesabı adını yazın
1. **Key Vault adına**, Anahtar Kasası adını yazın
1. **İşlev uygulaması adına**, işlev uygulama adını yazın
1. **Gizli adı**alanına erişim anahtarlarının depolanacağı gizli adı yazın
1. **Gözden geçir + oluştur**' u seçin.
1. **Oluştur**’u seçin

   ![Gözden geçir + oluştur](../media/secrets/rotation-dual/dual-rotation-7.png)

### <a name="add-another-storage-account-access-key-to-key-vault"></a>Key Vault başka bir depolama hesabı erişim anahtarı ekleyin

Depolama hesabı kaynak KIMLIĞINI alın. Değer, özellik altında bulunabilir `id`
```azurecli
az storage account show -n akvrotationstorage2
```

Key2 değerini almak için depolama hesabı erişim anahtarlarını listeleyin

```azurecli
az storage account keys list -n akvrotationstorage2 
```

**Key2Value** ve **storageaccountresourceıd** için alınan değerleri doldur

```azurecli
$tomorrowDate = (get-date).AddDays(+1).ToString("yyy-MM-ddThh:mm:ssZ")
az keyvault secret set --name storageKey2 --vault-name akvrotation-kv --value <key2Value> --tags "CredentialId=key2" "ProviderAddress=<storageAccountResourceId>" "ValidityPeriodDays=60" --expires $tomorrowDate
```

Aşağıdaki komutu kullanarak gizli bilgileri göster:
```azurecli
az keyvault secret show --vault-name akvrotation-kv --name storageKey2
```
`CredentialId`Alternatif olarak güncellendiğine `keyName` ve gizli dizi `value` tarafından yeniden üretildiğine dikkat edin ![](../media/secrets/rotation-dual/dual-rotation-8.png)

Değeri doğrulamak için erişim tuşlarını al
```azurecli
az storage account keys list -n akvrotationstorage 
```
![Erişim anahtarı listesi](../media/secrets/rotation-dual/dual-rotation-9.png)

## <a name="available-key-vault-dual-credential-rotation-functions"></a>Key Vault çift kimlik bilgisi döndürme işlevlerini kullanılabilir

- [Depolama hesabı](https://github.com/jlichwa/KeyVault-Rotation-StorageAccountKey-PowerShell)
- [Redis Cache](https://github.com/jlichwa/KeyVault-Rotation-RedisCacheKey-PowerShell)

## <a name="learn-more"></a>Daha fazla bilgi edinin
- Genel Bakış: [Azure Event Grid Key Vault izleme (Önizleme)](../general/event-grid-overview.md)
- Nasıl yapılır: [Azure Portal ilk işlevinizi oluşturma](../../azure-functions/functions-create-first-azure-function.md)
- Nasıl yapılır: [Anahtar Kasası gizli anahtarı değiştiğinde e-posta alma](../general/event-grid-logicapps.md)
- [Azure Key Vault için Azure Event Grid olay şeması (Önizleme)](../../event-grid/event-schema-key-vault.md)
