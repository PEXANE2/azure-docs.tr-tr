---
title: Azure İzleyici müşteri tarafından yönetilen anahtar
description: Azure Key Vault anahtarı kullanarak Log Analytics çalışma alanlarınızdaki verileri şifrelemek için Customer-Managed anahtarı yapılandırmaya yönelik bilgiler ve adımlar.
ms.subservice: logs
ms.topic: conceptual
author: yossi-y
ms.author: yossiy
ms.date: 11/18/2020
ms.openlocfilehash: 7bfd951d7cec27e0b8264aaabf9bc3a17875256a
ms.sourcegitcommit: 642988f1ac17cfd7a72ad38ce38ed7a5c2926b6c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/18/2020
ms.locfileid: "94873531"
---
# <a name="azure-monitor-customer-managed-key"></a>Azure İzleyici müşteri tarafından yönetilen anahtar 

Bu makalede, Log Analytics çalışma alanlarınız için müşteri tarafından yönetilen anahtarları yapılandırma hakkında arka plan bilgileri ve adımlar sağlanmaktadır. Yapılandırıldıktan sonra, çalışma alanlarına gönderilen tüm veriler Azure Key Vault anahtarınızla şifrelenir.

Yapılandırmadan önce aşağıdaki [sınırlamaları ve kısıtlamaları](#limitationsandconstraints) incelemenizi öneririz.

## <a name="customer-managed-key-overview"></a>Müşteri tarafından yönetilen anahtara genel bakış

[Bekleyen şifreleme](../../security/fundamentals/encryption-atrest.md) , kuruluşlarda ortak bir gizlilik ve güvenlik gereksinimidir. Azure 'un bekleyen şifrelemeyi tamamen yönetmesine izin verebilir, şifreleme ve şifreleme anahtarlarını yakından yönetmek için çeşitli seçenekleriniz vardır.

Azure Izleyici, tüm veri ve kaydedilmiş sorguların Microsoft tarafından yönetilen anahtarlar (MMK) kullanılarak Rest 'te şifrelenmesini sağlar. Azure Izleyici Ayrıca, [Azure Key Vault](../../key-vault/general/overview.md) depolanan kendi anahtarınızı kullanarak şifreleme için bir seçenek sağlar ve size istediğiniz zaman verilerinize erişimi iptal etmek için size denetim verir. Azure Izleyici şifreleme kullanımı, [Azure depolama şifrelemesiyle](../../storage/common/storage-service-encryption.md#about-azure-storage-encryption) aynı şekilde çalışır.

Customer-Managed anahtar, daha yüksek koruma düzeyi ve denetimi sağlayan adanmış Log Analytics kümelerinde dağıtılır. Adanmış kümelere alınan veriler iki kez şifrelenir: Microsoft tarafından yönetilen anahtarlar veya müşteri tarafından yönetilen anahtarlar kullanılarak hizmet düzeyinde bir kez ve altyapı düzeyinde iki farklı şifreleme algoritması ve iki farklı anahtar kullanan bir kez. [Çift şifreleme](../../storage/common/storage-service-encryption.md#doubly-encrypt-data-with-infrastructure-encryption) , şifreleme algoritmalarından veya anahtarlardan birinin tehlikeye girdiği bir senaryoya karşı koruma sağlar. Bu durumda, ek şifreleme katmanı verilerinizi korumaya devam eder. Adanmış küme ayrıca verilerinizi [kasa](#customer-lockbox-preview) denetimiyle korumanıza olanak sağlar.

Son 14 gün içinde alınan veriler, verimli sorgu altyapısı işlemi için etkin-önbellek (SSD-desteklenen) olarak da tutulur. Bu veriler, müşteri tarafından yönetilen anahtar yapılandırmasına bakılmaksızın Microsoft anahtarlarıyla şifreli olarak kalır, ancak SSD verileri üzerindeki denetiminiz [anahtar iptalinde](#key-revocation)kalır. SSD verilerinin 2021 birinci yarısında Customer-Managed anahtarla şifrelenmesini sağlamak için çalışıyoruz.

[Log Analytics kümeleri fiyatlandırma modeli](./manage-cost-storage.md#log-analytics-dedicated-clusters) 1000 GB/gün düzeyinden Itibaren kapasite rezervasyonları kullanır.

> [!IMPORTANT]
> Geçici kapasite kısıtlamaları nedeniyle, bir küme oluşturmadan önce ' a ön kayıt yapmanız gerekir. Kişilerinizi Microsoft 'a kullanın veya abonelik kimliklerinizi kaydetmek için destek isteği ' ni açın.

## <a name="how-customer-managed-key-works-in-azure-monitor"></a>Customer-Managed anahtarı Azure Izleyici 'de nasıl kullanılır

Azure Izleyici, Azure Key Vault erişim sağlamak için sistem tarafından atanan yönetilen kimliğin yararlanır. Sistem tarafından atanan yönetilen kimlik yalnızca tek bir Azure kaynağıyla ilişkilendirilene karşın Log Analytics kümesinin kimliği küme düzeyinde desteklenirken, bu özelliğin adanmış bir Log Analytics kümesine teslim edildiğini belirler. Birden çok çalışma alanı üzerinde Customer-Managed anahtarı desteklemek için, yeni bir Log Analytics *küme* kaynağı Key Vault ve Log Analytics çalışma alanlarınız arasında ara kimlik bağlantısı olarak gerçekleştirilir. Log Analytics küme depolaması, \' Azure Active Directory aracılığıyla Azure Key Vault kimlik doğrulaması yapmak Için *küme* kaynağıyla ilişkili yönetilen kimliği kullanır. 

Yapılandırma sonrasında, adanmış kümenize bağlı çalışma alanlarına alınan tüm veriler, Key Vault anahtarındaki anahtarınızla şifrelenir. Her zaman çalışma alanlarının kümeden bağlantısını kaldırabilirsiniz. Yeni veriler daha sonra, yeni ve eski verilerinizi sorunsuz bir şekilde sorgulayabilmeniz için Log Analytics depolama ve Microsoft anahtarıyla şifrelenmiş olarak alınır.


![Customer-Managed anahtara genel bakış](media/customer-managed-keys/cmk-overview.png)

1. Key Vault
2. Log Analytics *küme* kaynağı, Key Vault izinleri olan yönetilen kimliğe sahip--kimlik, adanmış Log Analytics küme depolamasına yayılmıştır
3. Adanmış Log Analytics kümesi
4. *Küme* kaynağına bağlı çalışma alanları 

## <a name="encryption-keys-operation"></a>Şifreleme anahtarları işlemi

Depolama veri şifrelemesi ile ilgili 3 tür anahtar vardır:

- **Kek** -anahtar şifreleme anahtarı (Customer-Managed anahtarınız)
- **AEK** hesabı şifreleme anahtarı
- **Dek** -veri şifreleme anahtarı

Aşağıdaki kurallar geçerlidir:

- Log Analytics küme depolama hesapları, AEK olarak bilinen her depolama hesabı için benzersiz şifreleme anahtarı oluşturur.
- AEK, diske yazılan her veri bloğunu şifrelemek için kullanılan anahtarlar olan DEKs 'leri türetmek için kullanılır.
- Key Vault ' de anahtarınızı yapılandırıp kümede buna başvurduğunuzda, Azure depolama, veri şifreleme ve şifre çözme işlemleri gerçekleştirmek için AEK 'i sarmalamak ve sarmalamak üzere Azure Key Vault istekleri gönderir.
- KEK, Key Vault hiçbir durumda kalmayacak ve HSM anahtarı durumunda donanımdan ayrılmayacaktır.
- Azure depolama, Azure Active Directory üzerinden Azure Key Vault kimlik doğrulaması yapmak ve erişmek için *küme* kaynağıyla ilişkili yönetilen kimliği kullanır.

## <a name="customer-managed-key-provisioning-procedure"></a>Customer-Managed anahtar sağlama yordamı

1. Küme oluşturmaya izin vermek için aboneliğinizi kaydetme
1. Azure Key Vault oluşturma ve anahtar depolama
1. Küme oluşturuluyor
1. Key Vault izinler veriliyor
1. Log Analytics çalışma alanlarını bağlama

Customer-Managed anahtar yapılandırması Azure portal desteklenmez ve sağlama [PowerShell](https://docs.microsoft.com/powershell/module/az.operationalinsights/), [CLI](https://docs.microsoft.com/cli/azure/monitor/log-analytics) veya [rest](https://docs.microsoft.com/rest/api/loganalytics/) istekleri aracılığıyla gerçekleştirilir.

### <a name="asynchronous-operations-and-status-check"></a>Zaman uyumsuz işlemler ve durum denetimi

Yapılandırma adımlarının bazıları hızla tamamlanamadığından zaman uyumsuz olarak çalışır. REST kullanılırken, yanıt, kabul edildiğinde *Azure-AsyncOperation* özelliğine sahıp bir http durum kodu 200 (Tamam) ve üst bilgi döndürür:
```json
"Azure-AsyncOperation": "https://management.azure.com/subscriptions/subscription-id/providers/Microsoft.OperationalInsights/locations/region-name/operationStatuses/operation-id?api-version=2020-08-01"
```

*Azure-AsyncOperation* üst bilgi DEĞERINE bir get isteği göndererek zaman uyumsuz işlemin durumunu kontrol edebilirsiniz:
```rst
GET https://management.azure.com/subscriptions/subscription-id/providers/microsoft.operationalInsights/locations/region-name/operationstatuses/operation-id?api-version=2020-08-01
Authorization: Bearer <token>
```

`status`' In yanıtı, ' InProgress ', ' Updating ', ' siliyor ', ' SUCCEEDED veya ' Failed ' hata kodu dahil olabilir.

### <a name="allowing-subscription"></a>Aboneliğe izin verme

> [!IMPORTANT]
> Customer-Managed anahtar özelliği bölgesel. Azure Key Vault, kümeniz ve bağlı Log Analytics çalışma alanlarınızın aynı bölgede olması gerekir, ancak bunlar farklı aboneliklerde olabilir.

### <a name="storing-encryption-key-kek"></a>Şifreleme anahtarını depolama (KEK)

Zaten oluşturmak için gereken bir Azure Key Vault oluşturun veya kullanın veya veri şifrelemesi için kullanılacak bir anahtarı içeri aktarın. Azure Key Vault anahtarınızı ve Azure Izleyici 'deki verilerinize erişimi korumak için kurtarılabilir olarak yapılandırılmalıdır. Bu yapılandırmayı Key Vault Özellikler altında doğrulayabilirsiniz, hem *geçici silme* hem de *Temizleme koruması* etkinleştirilmelidir.

![Geçici silme ve Temizleme koruması ayarları](media/customer-managed-keys/soft-purge-protection.png)

Bu ayarlar, CLı ve PowerShell aracılığıyla Key Vault güncelleştirilebilen:

- [Geçici Silme](../../key-vault/general/soft-delete-overview.md)
- Geçici silme işleminden sonra bile gizli/kasaların silinmesini zorlamak için [koruma koruyucuları temizle](../../key-vault/general/soft-delete-overview.md#purge-protection)

### <a name="create-cluster"></a>Küme oluşturma

[Adanmış kümeler](https://docs.microsoft.com/azure/azure-monitor/log-query/logs-dedicated-clusters#creating-a-cluster)makalesinde gösterilen yordamı izleyin. 

> [!IMPORTANT]
> Sonraki adımlarda ayrıntılara ihtiyacınız olacağı için yanıtı kopyalayın ve kaydedin.

### <a name="grant-key-vault-permissions"></a>Key Vault izinleri verme

Kümenize izin vermek için Key Vault erişim ilkesi oluşturun. Bu izinler, veri şifrelemesi için Azure Izleyici depolaması 'nın temelini oluşturmak için kullanılır. Key Vault Azure portal açın ve bu ayarlarla bir ilke oluşturmak için "erişim Ilkeleri" ve "+ erişim Ilkesi Ekle" seçeneğine tıklayın:

- Anahtar izinleri: ' Get ', ' Wrap Key ' ve ' Wrap Key ' izinlerini seçin.
- Sorumlu seçin: önceki adımda verilen yanıtta döndürülen küme adını veya asıl kimlik değerini girin.

![Key Vault izinleri verme](media/customer-managed-keys/grant-key-vault-permissions-8bit.png)

Key Vault, anahtarınızı korumak ve Azure Izleyici verilerinize erişmek için kurtarılabilir olarak yapılandırıldığını doğrulamak için *Get* izni gerekir.

### <a name="update-cluster-with-key-identifier-details"></a>Anahtar tanımlayıcı ayrıntıları ile kümeyi güncelleştirme

Kümedeki tüm işlemler Microsoft. Operationalınsights/kümeler/yazma eylemi iznini gerektirir. Bu izin,/Write eylemini içeren sahip veya katkıda bulunan veya *Microsoft. Operationalınsights/Action içeren Log Analytics katkıda bulunan rolü aracılığıyla* verilebilir.

Bu adım, Azure Izleyici depolama alanını veri şifreleme için kullanılacak anahtar ve sürümle güncelleştirir. Güncelleştirilirken, yeni anahtarınız depolama anahtarını sarmalamak ve sarmalamak için kullanılır (AEK).

Anahtar tanımlayıcı ayrıntılarını almak için Azure Key Vault ' de anahtarınızın güncel sürümünü seçin.

![Key Vault izinleri verme](media/customer-managed-keys/key-identifier-8bit.png)

Anahtar tanımlayıcı ayrıntıları ile kümedeki KeyVaultProperties 'i güncelleştirin.

İşlem zaman uyumsuzdur ve tamamlanması biraz zaman alabilir.

```azurecli
az monitor log-analytics cluster update --name "cluster-name" --resource-group "resource-group-name" --key-name "key-name" --key-vault-uri "key-uri" --key-version "key-version"
```

```powershell
Update-AzOperationalInsightsCluster -ResourceGroupName "resource-group-name" -ClusterName "cluster-name" -KeyVaultUri "key-uri" -KeyName "key-name" -KeyVersion "key-version"
```

```rst
PATCH https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters/cluster-name"?api-version=2020-08-01
Authorization: Bearer <token> 
Content-type: application/json
 
{
  "properties": {
    "keyVaultProperties": {
      "keyVaultUri": "https://key-vault-name.vault.azure.net",
      "kyName": "key-name",
      "keyVersion": "current-version"
  },
  "sku": {
    "name": "CapacityReservation",
    "capacity": 1000
  }
}
```

**Response**

Anahtar tanımlayıcısının yayılması birkaç dakika sürer. Güncelleştirme durumunu iki şekilde kontrol edebilirsiniz:
1. Yanıttan Azure-AsyncOperation URL değerini kopyalayın ve [zaman uyumsuz işlemler durum denetimini](#asynchronous-operations-and-status-check)izleyin.
2. Küme üzerinde bir GET isteği gönderin ve *Keyvaultproperties* özelliklerine bakın. Son güncellenen anahtar tanımlayıcı ayrıntılarınız yanıta döndürmelidir.

Anahtar tanımlayıcı güncelleştirmesi tamamlandığında GET isteğinin yanıtı şuna benzemelidir: 200 OK ve Header
```json
{
  "identity": {
    "type": "SystemAssigned",
    "tenantId": "tenant-id",
    "principalId": "principle-id"
    },
  "sku": {
    "name": "capacityReservation",
    "capacity": 1000,
    "lastSkuUpdate": "Sun, 22 Mar 2020 15:39:29 GMT"
    },
  "properties": {
    "keyVaultProperties": {
      "keyVaultUri": "https://key-vault-name.vault.azure.net",
      "kyName": "key-name",
      "keyVersion": "current-version"
      },
    "provisioningState": "Succeeded",
    "billingType": "cluster",
    "clusterId": "cluster-id"
  },
  "id": "/subscriptions/subscription-id/resourceGroups/resource-group-name/providers/Microsoft.OperationalInsights/clusters/cluster-name",
  "name": "cluster-name",
  "type": "Microsoft.OperationalInsights/clusters",
  "location": "region-name"
}
```

### <a name="link-workspace-to-cluster"></a>Çalışma alanını kümeye bağla

Bu işlemi gerçekleştirmek için hem çalışma alanınızda hem de kümenizde ' Write ' izinlerine sahip olmanız gerekir:

- Çalışma alanında: Microsoft. Operationalınsights/çalışma alanları/yazma
- Kümede: Microsoft. Operationalınsights/kümeler/Write

> [!IMPORTANT]
> Bu adım yalnızca Log Analytics kümesi sağlama işleminin tamamlanmasından sonra gerçekleştirilmelidir. Çalışma alanlarını bağlarsanız ve sağlama öncesinde verileri içe alıyorsa, alınan veriler bırakılır ve geri alınamaz.

Bu işlem zaman uyumsuzdur ve tamamlanırken bir süre olabilir.

[Adanmış kümeler](https://docs.microsoft.com/azure/azure-monitor/log-query/logs-dedicated-clusters#link-a-workspace-to-the-cluster)makalesinde gösterilen yordamı izleyin.

## <a name="key-revocation"></a>Anahtar iptali

Anahtarınızı devre dışı bırakarak veya Key Vault kümenin erişim ilkesini silerek verilere erişimi iptal edebilirsiniz. Log Analytics küme depolaması, her zaman bir saat veya daha kısa bir süre içinde anahtar izinlerinde yapılan değişikliklere uyar ve depolama alanı kullanılamaz hale gelir. Kümenizle bağlantılı çalışma alanlarına alınan yeni veriler bırakılır ve geri alınamaz, verilere erişilemez ve bu çalışma alanlarına yönelik sorgular başarısız olur. Önceden alınan veriler, kümeniz ve çalışma alanlarınız silinmediği sürece depolamada kalır. Erişilemeyen veriler veri bekletme ilkesine tabidir ve bekletmeye ulaşıldığında temizlenir. 

Son 14 gün içinde alınan veriler, verimli sorgu altyapısı işlemi için etkin-önbellek (SSD-desteklenen) olarak da tutulur. Bu, anahtar iptali işleminde silinir ve erişilmez hale gelir.

Depolama, şifreleme anahtarını sarmalamadan ve erişildikten sonra 30 dakika içinde veri alımı ve sorgu sürdürülmeye çalışmak için Key Vault düzenli olarak yoklar.

## <a name="key-rotation"></a>Anahtar döndürme

Customer-Managed anahtar döndürme, Azure Key Vault yeni anahtar sürümü ile kümeye açık bir güncelleştirme gerektirir. "Kümeyi anahtar tanımlayıcısı ayrıntıları ile güncelleştirme" adımındaki yönergeleri izleyin. Kümedeki yeni anahtar tanımlayıcı ayrıntılarını güncelleştirmemeniz durumunda Log Analytics küme depolaması şifreleme için önceki anahtarınızı kullanmaya devam edecektir. Kümedeki yeni anahtarı güncelleştirmeden önce eski anahtarınızı devre dışı bırakır veya silerseniz, [anahtar iptal](#key-revocation) durumuna sahip olursunuz.

Verilerin her zaman Key Vault ' de yeni anahtar şifreleme anahtarı (KEK) ile şifrelenmesi sırasında, veriler her zaman hesap şifreleme anahtarıyla (AEK) şifrelendiğinden, tüm verileriniz anahtar döndürme işleminden sonra erişilebilir durumda kalır.

## <a name="customer-managed-key-for-queries"></a>Sorgular için Customer-Managed anahtarı

Log Analytics ' de kullanılan sorgu dili ifade edilebilir ve sorgulara eklediğiniz açıklamalarda veya sorgu söz diziminde gizli bilgiler içerebilir. Bazı kuruluşlar, bu tür bilgilerin Customer-Managed anahtar ilkesi altında korunmasını gerektirir ve sorgularınızı anahtarınızla şifreli olarak kaydetmeniz gerekir. Azure Izleyici, çalışma alanınıza bağlıyken kendi depolama hesabınızda anahtarınızla şifrelenen *kayıtlı aramaları* ve *günlük uyarıları* sorgularını depolamanıza olanak sağlar. 

> [!NOTE]
> Log Analytics sorguları, kullanılan senaryoya bağlı olarak çeşitli depolarda kaydedilebilir. Sorgular, Azure Izleyici 'deki çalışma kitapları, Azure panoları, Azure mantıksal uygulaması, Azure Notebooks ve Otomasyon Runbook 'Ları Customer-Managed bağımsız olarak, aşağıdaki senaryolarda Microsoft anahtar (MMK) ile şifreli olarak kalır.

Kendi depolama alanınızı (BYOS) getirip çalışma alanınıza bağladığınızda, hizmet *kayıtlı aramaları* ve *günlük uyarıları* sorgularını depolama hesabınıza yükler. Diğer bir deyişle, Log Analytics kümesindeki verileri şifrelemek için kullandığınız anahtarı veya farklı bir anahtarı kullanarak depolama hesabını ve [geri kalan şifreleme ilkesini](../../storage/common/customer-managed-keys-overview.md) denetlersiniz. Bununla birlikte, bu depolama hesabıyla ilişkili maliyetlerden de sorumlu olursunuz. 

**Sorgular için Customer-Managed anahtarı ayarlamadan önce dikkat edilecek noktalar**
* Hem çalışma alanınız hem de depolama hesabınızda ' Write ' izinlerine sahip olmanız gerekir
* Log Analytics çalışma alanınız bulunduğundan, depolama hesabınızı aynı bölgede oluşturduğunuzdan emin olun
* Depolamadaki *aramalar* , hizmet yapıtları olarak değerlendirilir ve bunların biçimi değişebilir
* Mevcut *kaydetme aramaları* çalışma alanınızdan kaldırılır. Yapılandırma öncesinde ihtiyacınız olan aramaları kopyalayın ve *kaydeder* . *Kayıtlı aramalarınızı* [PowerShell](/powershell/module/az.operationalinsights/get-azoperationalinsightssavedsearch) kullanarak görüntüleyebilirsiniz
* Sorgu geçmişi desteklenmiyor ve çalıştırdığınız sorguları göremezsiniz
* Sorguları kaydetmek amacıyla tek bir depolama hesabını çalışma alanına bağlayabilirsiniz, ancak her ikisi de *kaydedilmiş aramalar* ve *log-Alerts* sorguları kullanabilir.
* Panoya sabitle desteklenmiyor

**Kayıtlı arama sorguları için KCG 'LERI yapılandırma**

Çalışma alanınıza *sorgu* için bir depolama hesabı bağlayın-- *kayıtlı aramalar* sorguları depolama hesabınıza kaydedilir. 

```azurecli
$storageAccountId = '/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Storage/storageAccounts/<storage name>'
az monitor log-analytics workspace linked-storage create --type Query --resource-group "resource-group-name" --workspace-name "workspace-name" --storage-accounts $storageAccountId
```

```powershell
$storageAccount.Id = Get-AzStorageAccount -ResourceGroupName "resource-group-name" -Name "storage-account-name"
New-AzOperationalInsightsLinkedStorageAccount -ResourceGroupName "resource-group-name" -WorkspaceName "workspace-name" -DataSourceType Query -StorageAccountIds $storageAccount.Id
```

```rst
PUT https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/Microsoft.OperationalInsights/workspaces/<workspace-name>/linkedStorageAccounts/Query?api-version=2020-08-01
Authorization: Bearer <token> 
Content-type: application/json
 
{
  "properties": {
    "dataSourceType": "Query", 
    "storageAccountIds": 
    [
      "/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Storage/storageAccounts/<storage-account-name>"
    ]
  }
}
```

Yapılandırmadan sonra, yeni *Kaydedilmiş arama* sorgusu, depolama alanına kaydedilir.

**KCG 'LERI log-Alerts sorguları için yapılandırma**

Çalışma alanınıza *Uyarılar* için bir depolama hesabı bağlayın-- *log-Alerts* sorguları depolama hesabınıza kaydedilir. 

```azurecli
$storageAccountId = '/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Storage/storageAccounts/<storage name>'
az monitor log-analytics workspace linked-storage create --type ALerts --resource-group "resource-group-name" --workspace-name "workspace-name" --storage-accounts $storageAccountId
```

```powershell
$storageAccount.Id = Get-AzStorageAccount -ResourceGroupName "resource-group-name" -Name "storage-account-name"
New-AzOperationalInsightsLinkedStorageAccount -ResourceGroupName "resource-group-name" -WorkspaceName "workspace-name" -DataSourceType Alerts -StorageAccountIds $storageAccount.Id
```

```rst
PUT https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/Microsoft.OperationalInsights/workspaces/<workspace-name>/linkedStorageAccounts/Alerts?api-version=2020-08-01
Authorization: Bearer <token> 
Content-type: application/json
 
{
  "properties": {
    "dataSourceType": "Alerts", 
    "storageAccountIds": 
    [
      "/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Storage/storageAccounts/<storage-account-name>"
    ]
  }
}
```

Yapılandırmadan sonra, tüm yeni uyarı sorgusu depolama alanına kaydedilir.

## <a name="customer-lockbox-preview"></a>Müşteri Kasası (Önizleme)
Kasa, destek isteği sırasında verilerinize erişmek için Microsoft mühendis isteğini onaylama veya reddetme denetimi sağlar.

Azure Izleyici 'de, Log Analytics adanmış kümenize bağlı olan çalışma alanlarındaki veriler üzerinde bu denetime sahip olursunuz. Kasa denetimi, kasa korumalı aboneliğiniz altındaki kümenin depolama hesaplarında yalıtılmış olduğu Log Analytics adanmış bir kümede depolanan veriler için geçerlidir.  

[Microsoft Azure için müşteri kasası](../../security/fundamentals/customer-lockbox-overview.md) hakkında daha fazla bilgi edinin

## <a name="customer-managed-key-operations"></a>Customer-Managed anahtar işlemleri

- **Bir kaynak grubundaki tüm kümeleri al**
  
  ```azurecli
  az monitor log-analytics cluster list --resource-group "resource-group-name"
  ```

  ```powershell
  Get-AzOperationalInsightsCluster -ResourceGroupName "resource-group-name"
  ```

  ```rst
  GET https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters?api-version=2020-08-01
  Authorization: Bearer <token>
  ```

  **Response**
  
  ```json
  {
    "value": [
      {
        "identity": {
          "type": "SystemAssigned",
          "tenantId": "tenant-id",
          "principalId": "principal-Id"
        },
        "sku": {
          "name": "capacityReservation",
          "capacity": 1000,
          "lastSkuUpdate": "Sun, 22 Mar 2020 15:39:29 GMT"
          },
        "properties": {
           "keyVaultProperties": {
              "keyVaultUri": "https://key-vault-name.vault.azure.net",
              "keyName": "key-name",
              "keyVersion": "current-version"
              },
          "provisioningState": "Succeeded",
          "billingType": "cluster",
          "clusterId": "cluster-id"
        },
        "id": "/subscriptions/subscription-id/resourcegroups/resource-group-name/providers/microsoft.operationalinsights/workspaces/workspace-name",
        "name": "cluster-name",
        "type": "Microsoft.OperationalInsights/clusters",
        "location": "region-name"
      }
    ]
  }
  ```

- **Bir abonelikteki tüm kümeleri al**

  ```azurecli
  az monitor log-analytics cluster list
  ```

  ```powershell
  Get-AzOperationalInsightsCluster
  ```

  ```rst
  GET https://management.azure.com/subscriptions/<subscription-id>/providers/Microsoft.OperationalInsights/clusters?api-version=2020-08-01
  Authorization: Bearer <token>
  ```
    
  **Response**
    
  ' Kaynak grubunda bulunan ' kümesi ile aynı yanıt, ancak abonelik kapsamı.

- **Kümede *Kapasite ayırmayı* Güncelleştir**

  Bağlı çalışma alanlarınızdaki veri hacmi zaman içinde değişiklik yaparken ve kapasite ayırma düzeyini uygun şekilde güncellemek istediğinizde. [Güncelleştirme kümesini](#update-cluster-with-key-identifier-details) izleyin ve yeni kapasite değerini sağlayın. Bu, gün başına 1000 GB ve 100 adımlarında 3000 arasında olabilir. Günde 3000 GB 'den yüksek düzey için, Microsoft kişinize ulaşın. Tam REST istek gövdesini sağlamanız gerekmez, ancak SKU 'yu içermelidir:

  ```azurecli
  az monitor log-analytics cluster update --name "cluster-name" --resource-group "resource-group-name" --sku-capacity daily-ingestion-gigabyte
  ```

  ```powershell
  Update-AzOperationalInsightsCluster -ResourceGroupName "resource-group-name" -ClusterName "cluster-name" -SkuCapacity daily-ingestion-gigabyte
  ```

  ```rst
  PATCH https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters/<cluster-name>?api-version=2020-08-01
  Authorization: Bearer <token>
  Content-type: application/json

  {
    "sku": {
      "name": "capacityReservation",
      "Capacity": daily-ingestion-gigabyte
    }
  }
  ```

- **Kümede *Billingtype* 'ı güncelleştirme**

  *Billingtype* özelliği, küme ve verileri için faturalandırma atısyonu belirler:
  - *küme* (varsayılan)--Faturalandırma, küme kaynağınızı barındıran abonelikle ilişkilidir
  - *çalışma alanları* --faturalandırma, çalışma alanlarınızı orantılı olarak barındıran aboneliklerle ilişkilidir
  
  [Güncelleştirme kümesini](#update-cluster-with-key-identifier-details) izleyin ve yeni billingType değerini sağlayın. Tam REST istek gövdesini sağlamanız gerekmez ve *Billingtype* öğesini içermelidir:

  ```rst
  PATCH https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters/<cluster-name>?api-version=2020-08-01
  Authorization: Bearer <token>
  Content-type: application/json

  {
    "properties": {
      "billingType": "cluster",
      }  
  }
  ``` 

- **Çalışma alanının bağlantısını kaldırma**

  Bu işlemi gerçekleştirmek için çalışma alanı ve kümede ' yazma ' izinlerine sahip olmanız gerekir. Bir çalışma alanının kümenizi dilediğiniz zaman kaldırabilirsiniz. Bağlantıyı kaldırma işlemi sonrasında yeni alınan veriler Log Analytics depolama alanında depolanır ve Microsoft anahtarıyla şifrelenir. Küme sağlanmakta olduğu ve geçerli Key Vault anahtarıyla yapılandırıldığı sürece, çalışma alanınıza alınan ve bu bağlantıyı sorunsuz bir şekilde yeniden bağlamak istediğiniz verileri sorgulayabilirsiniz.

  Bu işlem zaman uyumsuzdur ve tamamlanırken bir süre olabilir.

  ```azurecli
  az monitor log-analytics workspace linked-service delete --resource-group "resource-group-name" --name "cluster-name" --workspace-name "workspace-name"
  ```

  ```powershell
  Remove-AzOperationalInsightsLinkedService -ResourceGroupName "resource-group-name" -Name "workspace-name" -LinkedServiceName cluster
  ```

  ```rest
  DELETE https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/workspaces/<workspace-name>/linkedservices/cluster?api-version=2020-08-01
  Authorization: Bearer <token>
  ```

  - **Çalışma alanı bağlantı durumunu denetle**
  
  Çalışma alanında Get işlemini gerçekleştirin ve *Kümeresourceıd* özelliğinin *Özellikler* altında yanıtta bulunup bulunmayacağını gözlemleyin. Bağlı bir çalışma alanı *Clusterresourceıd* özelliğine sahip olur.

  ```azurecli
  az monitor log-analytics cluster show --resource-group "resource-group-name" --name "cluster-name"
  ```

  ```powershell
  Get-AzOperationalInsightsWorkspace -ResourceGroupName "resource-group-name" -Name "workspace-name"
  ```

- **Kümenizi silme**

  Bu işlemi gerçekleştirmek için kümede ' Write ' izinlerine sahip olmanız gerekir. Bir geçici silme işlemi, kümenizin, verileri 14 gün içinde, silme işleminin yanlışlıkla mi yoksa bilerek mi olduğunu da içerecek şekilde kurtarılmasına olanak tanımak için gerçekleştirilir. Küme adı, geçici silme döneminde ayrılmış kalır ve bu adla yeni bir küme oluşturamazsınız. Geçici silme süresinden sonra, küme adı serbest bırakılır ve kümeniz ile verileri kalıcı olarak silinir ve kurtarılamaz. Herhangi bir bağlı çalışma alanının, silme işleminde kümeden bağlantısı kaldırılır. Yeni alınan veriler Log Analytics depolama alanında depolanır ve Microsoft anahtarıyla şifrelenir. 
  
  Bağlantıyı kaldır işlemi zaman uyumsuzdur ve tamamlanması 90 dakika sürebilir.

  ```azurecli
  az monitor log-analytics cluster delete --resource-group "resource-group-name" --name "cluster-name"
  ```
 
  ```powershell
  Remove-AzOperationalInsightsCluster -ResourceGroupName "resource-group-name" -ClusterName "cluster-name"
  ```

  ```rst
  DELETE https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters/<cluster-name>?api-version=2020-08-01
  Authorization: Bearer <token>
  ```
  
- **Kümenizi ve verilerinizi kurtarma** 
  
  Son 14 gün içinde silinen bir küme, geçici silme durumunda ve verileriyle kurtarılabilir. Tüm çalışma alanlarının küme silinmesinden bağlantısı olmadığından, çalışma alanınızı kümenin kurtarmasından sonra yeniden bağlamanız gerekir. Kurtarma işlemi şu anda ürün grubu tarafından el ile gerçekleştirilir. Microsoft kanalınızı kullanın veya silinen kümenin kurtarılması için destek isteği açın.

## <a name="limitations-and-constraints"></a>Sınırlamalar ve kısıtlamalar

- Customer-Managed anahtar, adanmış Log Analytics kümesinde desteklenir ve günde 1 GB ya da daha fazlasını gönderen müşteriler için uygundur.

- Bölge başına en fazla küme sayısı ve abonelik 2 ' dir

- Kümeye en fazla bağlantılı çalışma alanı 1000

- Bir çalışma alanını kümenize bağlayabilir ve sonra bağlantısını kesebilirsiniz. Belirli çalışma alanındaki çalışma alanı bağlantı işlemlerinin sayısı, 30 günlük bir dönemde 2 ile sınırlıdır.

- Kümeye çalışma alanı bağlantısı yalnızca Log Analytics kümesi sağlama tamamlandığını doğruladıktan sonra taşınmalıdır. Tamamlanmadan önce çalışma alanınıza gönderilen veriler bırakılır ve geri alınamaz.

- Customer-Managed anahtar şifrelemesi, yapılandırma zamanından sonra yeni alınan veriler için geçerlidir. Yapılandırmadan önce alınan veriler, Microsoft anahtarıyla şifreli olarak kalır. Customer-Managed anahtar yapılandırmasından önce ve sonra gelen verileri sorunsuz bir şekilde sorgulayabilirsiniz.

- Azure Key Vault kurtarılabilir olarak yapılandırılmalıdır. Bu özellikler varsayılan olarak etkinleştirilmemiştir ve CLı veya PowerShell kullanılarak yapılandırılmalıdır:<br>
  - [Geçici Silme](../../key-vault/general/soft-delete-overview.md)
  - Geçici silme işleminden sonra bile gizli/kasaların silinmesini zorlamak için [Temizleme koruması](../../key-vault/general/soft-delete-overview.md#purge-protection) açılmalıdır.

- Küme başka bir kaynak grubuna veya aboneliğe taşıma Şu anda desteklenmiyor.

- Azure Key Vault, kümeniz ve bağlı çalışma alanlarınız aynı bölgede ve aynı Azure Active Directory (Azure AD) kiracısında olmalıdır, ancak farklı aboneliklerde olabilir.

- Başka bir kümeyle bağlantı varsa, kümeye çalışma alanı bağlantısı başarısız olur.

## <a name="troubleshooting"></a>Sorun giderme

- Key Vault kullanılabilirliği ile davranış
  - Normal işlemde--depolama, kısa süreler için AEK önbelleğe alınır ve düzenli aralıklarla sarmalaması için Key Vault geri gider.
    
  - Geçici bağlantı hataları--depolama, anahtarların kısa bir süre boyunca önbellekte kalmasına izin vererek geçici hataları (zaman aşımları, bağlantı hataları, DNS sorunları) işler ve bu da tüm küçük sinyalleri 'leri kullanılabilirliğinden fazla. Sorgu ve alma özellikleri kesinti olmadan devam eder.
    
  - Canlı site--yaklaşık 30 dakikalık bir işlem, depolama hesabının kullanılamaz hale gelmesine neden olur. Sorgu özelliği kullanılamaz ve veri kaybını önlemek için Microsoft anahtar kullanarak birkaç saat boyunca önbelleğe alınır. Key Vault erişimi geri yüklendiğinde, sorgu kullanılabilir hale gelir ve geçici önbelleğe alınmış veriler veri deposuna alınır ve Customer-Managed anahtarıyla şifrelenir.

  - Key Vault erişim oranı--Azure Izleyici depolaması 'nın sarmalama ve sarmalama işlemleri için Key Vault, 6 ila 60 saniye arasındadır.

- Bir küme oluşturur ve KeyVaultProperties 'i hemen belirtirseniz, sistem kimliği kümeye atanana kadar erişim ilkesi tanımlanmadığından işlem başarısız olabilir.

- Mevcut kümeyi KeyVaultProperties ile güncelleştirirseniz ve ' Get ' anahtar erişimi Ilkesi Key Vault eksikse, işlem başarısız olur.

- Bir küme oluştururken çakışma hatası alırsanız, kümenizi son 14 gün içinde silmiş ve bu da geçici silme döneminde olabilir. Küme adı, geçici silme döneminde ayrılmış kalır ve bu adla yeni bir küme oluşturamazsınız. Bu ad, küme kalıcı olarak silindiğinde geçici silme süresinden sonra serbest bırakılır.

- Bir işlem devam ederken kümenizi güncelleştirirseniz işlem başarısız olur.

- Kümenizin dağıtımı başarısız olursa, Azure Key Vault, kümenizin ve bağlı Log Analytics çalışma alanlarınızın aynı bölgede olduğunu doğrulayın. Farklı aboneliklerde olabilir.

- Anahtar sürümünüzü Key Vault güncelleştirir ve kümedeki yeni anahtar tanımlayıcı ayrıntılarını güncelleştirmemeniz durumunda, Log Analytics kümesi önceki anahtarınızı kullanmaya devam eder ve verileriniz erişilemez hale gelir. Veri alımı ve veri sorgulama yeteneği sağlamak için kümedeki yeni anahtar tanımlayıcı ayrıntılarını güncelleştirin.

- Bazı işlemler uzun sürer ve işlemin tamamlanması biraz zaman alabilir; bunlar küme oluşturma, küme anahtarı güncelleştirme ve küme silme işlemlerini gerçekleştirebilir. İşlem durumunu iki şekilde denetleyebilirsiniz:
  1. REST kullanırken, Azure-AsyncOperation URL değerini yanıttan kopyalayın ve [zaman uyumsuz işlemler durum denetimini](#asynchronous-operations-and-status-check)izleyin.
  2. Kümeye veya çalışma alanına GET isteği gönderin ve yanıtı gözlemleyin. Örneğin, bağlantısız çalışma alanının *Özellikler* bölümünde *kümeresourceıd* yok.

- [Çift şifreleme](../../storage/common/storage-service-encryption.md#doubly-encrypt-data-with-infrastructure-encryption) , bölgede Çift şifreleme olduğunda, 2020 Ekim 'den oluşturulan kümeler için otomatik olarak yapılandırılır. Bir küme oluşturur ve "<Region-adı> kümeler için çift şifrelemeyi desteklemiyor.", kümeyi yine de oluşturabilirsiniz, ancak çift şifrelemeyi devre dışı bırakabilirsiniz. Küme oluşturulduktan sonra etkinleştirilemez veya devre dışı bırakılamaz. Bölgede Çift şifreleme desteklenmediğinde bir küme oluşturmak için `"properties": {"isDoubleEncryptionEnabled": false}` rest istek gövdesine ekleyin.

- Hata iletileri
  
  Küme oluşturma:
  -  400--küme adı geçerli değil. Küme adı, a-z, A-Z, 0-9 ve 3-63 uzunluğunda karakterler içerebilir.
  -  400--isteğin gövdesi null veya hatalı biçimde.
  -  400--SKU adı geçersiz. SKU adını Capacityreservatıon olarak ayarlayın.
  -  400--kapasite sağlandı ancak SKU, Capacityreservatıon değil. SKU adını Capacityreservatıon olarak ayarlayın.
  -  400--SKU 'da eksik kapasite. Kapasite değerini 100 (GB) adımlarında 1000 veya üzeri olarak ayarlayın.
  -  400--SKU 'da kapasite Aralık içinde değil. Çalışma alanınızdaki en az 1000 ve ' kullanım ve tahmini maliyet ' altında kullanılabilir olan en fazla izin verilen kapasiteye kadar olmalıdır.
  -  400--kapasite 30 gün boyunca kilitlidir. Güncelleştirme sonrasında kapasiteyi düşürmek 30 gün daha olur.
  -  400--SKU ayarlanmadı. SKU adını, 100 (GB) adımlarında Capacityreservatıon ve Capacity değerini 1000 veya üzeri olarak ayarlayın.
  -  400--Identity null ya da boş. SystemAssigned türüyle kimliği ayarlayın.
  -  400--KeyVaultProperties oluşturma sırasında ayarlanır. Anahtar Vaultproperties kümesini küme oluşturulduktan sonra güncelleştirin.
  -  400--işlem şu anda yürütülemiyor. Zaman uyumsuz işlem başarılı dışında bir durumda. Herhangi bir güncelleştirme işlemi gerçekleştirilmeden önce kümenin işlemini tamamlaması gerekir.

  Küme güncelleştirmesi
  -  400--küme silme durumunda. Zaman uyumsuz işlem devam ediyor. Herhangi bir güncelleştirme işlemi gerçekleştirilmeden önce kümenin işlemini tamamlaması gerekir.
  -  400--KeyVaultProperties boş değil, ancak biçimi hatalı. Bkz. [anahtar tanımlayıcısı güncelleştirmesi](#update-cluster-with-key-identifier-details).
  -  400--Key Vault anahtarı doğrulanamadı. İzin eksikliği veya anahtar bulunmadığı için olabilir. Key Vault ' de [anahtar ve erişim ilkesini ayarlamış](#grant-key-vault-permissions) olduğunuzdan emin olun.
  -  400--anahtar kurtarılabilir değil. Key Vault, geçici-silme ve Temizleme koruması olarak ayarlanmalıdır. [Key Vault belgelerine](../../key-vault/general/soft-delete-overview.md) bakın
  -  400--işlem şu anda yürütülemiyor. Zaman uyumsuz işlemin tamamlanmasını bekleyin ve yeniden deneyin.
  -  400--küme silme durumunda. Zaman uyumsuz işlemin tamamlanmasını bekleyin ve yeniden deneyin.

  Küme al:
    -  404--küme bulunamadı, küme silinmiş olabilir. Bu ada sahip bir küme oluşturmaya ve çakışmaya yol açmaya çalışırsanız, küme 14 gün boyunca geçici silme aşamasında olur. Bu dosyayı kurtarmak için desteğe başvurabilirsiniz veya yeni bir küme oluşturmak için başka bir ad kullanabilirsiniz. 

  Küme silme
    -  409--sağlama durumundayken bir kümeyi silemez. Zaman uyumsuz işlemin tamamlanmasını bekleyin ve yeniden deneyin.

  Çalışma alanı bağlantısı:
  -  404--çalışma alanı bulunamadı. Belirttiğiniz çalışma alanı yok veya silinmiş.
  -  409--işlemdeki çalışma alanı bağlantısı veya bağlantıyı kaldırma işlemi.
  -  400--küme bulunamadı, belirttiğiniz küme yok veya silinmiş. Bu ada sahip bir küme oluşturmaya ve çakışmaya yol açmaya çalışırsanız, küme 14 gün boyunca geçici silme aşamasında olur. Bu uygulamayı kurtarmak için desteğe başvurabilirsiniz.

  Çalışma alanına bağlantıyı kaldır:
  -  404--çalışma alanı bulunamadı. Belirttiğiniz çalışma alanı yok veya silinmiş.
  -  409--işlemdeki çalışma alanı bağlantısı veya bağlantıyı kaldırma işlemi.
