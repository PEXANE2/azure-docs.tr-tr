---
title: Azure İzleyici müşteri tarafından yönetilen anahtar
description: Azure Key Vault anahtarı kullanarak Log Analytics çalışma alanlarınızdaki verileri şifrelemek üzere Customer-Managed anahtarı (CMK) yapılandırma hakkında bilgi ve adımlar.
ms.subservice: logs
ms.topic: conceptual
author: yossi-y
ms.author: yossiy
ms.date: 09/09/2020
ms.openlocfilehash: 532d96163e2ec66730dc3fdf87f10904fd584224
ms.sourcegitcommit: ae6e7057a00d95ed7b828fc8846e3a6281859d40
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2020
ms.locfileid: "92108006"
---
# <a name="azure-monitor-customer-managed-key"></a>Azure İzleyici müşteri tarafından yönetilen anahtar 

Bu makalede, Log Analytics çalışma alanlarınız için müşteri tarafından yönetilen anahtarları (CMK) yapılandırmaya yönelik arka plan bilgileri ve adımlar sağlanmaktadır. Yapılandırıldıktan sonra, çalışma alanlarına gönderilen tüm veriler Azure Key Vault anahtarınızla şifrelenir.

Yapılandırmadan önce aşağıdaki [sınırlamaları ve kısıtlamaları](#limitationsandconstraints) incelemenizi öneririz.

## <a name="customer-managed-key-cmk-overview"></a>Müşteri tarafından yönetilen anahtar (CMK) genel bakış

[Bekleyen şifreleme](../../security/fundamentals/encryption-atrest.md) , kuruluşlarda ortak bir gizlilik ve güvenlik gereksinimidir. Azure 'un bekleyen şifrelemeyi tamamen yönetmesine izin verebilir, şifreleme veya şifreleme anahtarlarını yakından yönetmek için çeşitli seçenekleriniz vardır.

Azure Izleyici, tüm veri ve kaydedilmiş sorguların Microsoft tarafından yönetilen anahtarlar (MMK) kullanılarak Rest 'te şifrelenmesini sağlar. Azure Izleyici Ayrıca, [Azure Key Vault](../../key-vault/general/overview.md) depolanan ve sistem tarafından atanan [yönetilen kimlik](../../active-directory/managed-identities-azure-resources/overview.md) doğrulaması kullanan depolama tarafından erişilen kendi anahtarınızı kullanarak şifreleme için bir seçenek sağlar. Bu anahtar (CMK) [yazılım ya da donanım HSM korumalı](../../key-vault/general/overview.md)olabilir. Azure Izleyici şifreleme kullanımı, [Azure depolama şifrelemesiyle](../../storage/common/storage-service-encryption.md#about-azure-storage-encryption) aynı şekilde çalışır.

CMK özelliği adanmış Log Analytics kümelerine teslim edilir ve verilerinize erişimi istediğiniz zaman iptal etmenizi ve [kasa](#customer-lockbox-preview) denetimiyle korumanızı sağlar. Bölgenizde adanmış küme için gereken kapasiteye sahip olduğunuzu doğrulamak için, aboneliğinizin önceden izin verilmesini gerektiririz. CMK 'yi yapılandırmaya başlamadan önce aboneliğinizi izin verilen Microsoft Kişinizden yararlanın.

[Log Analytics kümeleri fiyatlandırma modeli](./manage-cost-storage.md#log-analytics-dedicated-clusters) 1000 GB/gün düzeyinden Itibaren kapasite rezervasyonları kullanır.

Son 14 gün içinde alınan veriler, verimli sorgu altyapısı işlemi için etkin-önbellek (SSD-desteklenen) olarak da tutulur. Bu veriler CMK yapılandırmasına bakılmaksızın Microsoft anahtarlarıyla şifreli olarak kalır, ancak SSD verileri üzerindeki denetiminiz, [anahtar iptalinde](#cmk-kek-revocation)kalır. 2020 ikinci yarısında CMK ile şifrelenen SSD verilerinin olması için çalışıyoruz.

## <a name="how-cmk-works-in-azure-monitor"></a>Azure Izleyici 'de CMK nasıl çalışmaktadır

Azure Izleyici, Azure Key Vault erişim sağlamak için sistem tarafından atanan yönetilen kimliğin yararlanır. Sistem tarafından atanan yönetilen kimlik yalnızca tek bir Azure kaynağıyla ilişkilendirilebilir ancak Log Analytics kümesinin kimliği, küme düzeyinde desteklenirken, bu, CMK özelliğinin adanmış bir Log Analytics kümesinde teslim edildiğini belirler. Birden çok çalışma alanı üzerinde CMK 'yi desteklemek için yeni bir Log Analytics *küme* kaynağı, Key Vault ve Log Analytics çalışma alanlarınız arasında ara kimlik bağlantısı olarak gerçekleştirilir. Log Analytics küme depolaması, \' Azure Active Directory aracılığıyla Azure Key Vault kimlik doğrulaması yapmak Için *küme* kaynağıyla ilişkili yönetilen kimliği kullanır. 

CMK yapılandırmasından sonra, adanmış kümenize bağlı çalışma alanlarına alınan tüm veriler, Key Vault anahtarındaki anahtarınızla şifrelenir. Her zaman çalışma alanlarının kümeden bağlantısını kaldırabilirsiniz. Yeni veriler, yeni ve eski verilerinizi sorunsuz bir şekilde sorgulayabilir Log Analytics depolama ve Microsoft anahtar ile şifrelenmiş olarak kullanıma alınır.


![CMK genel bakış](media/customer-managed-keys/cmk-overview.png)

1. Key Vault
2. Log Analytics *küme* kaynağı, Key Vault izinleri olan yönetilen kimliğe sahip--kimlik, adanmış Log Analytics küme depolamasına yayılmıştır
3. Adanmış Log Analytics kümesi
4. CMK şifrelemesi için *küme* kaynağına bağlı çalışma alanları

## <a name="encryption-keys-operation"></a>Şifreleme anahtarları işlemi

Depolama veri şifrelemesi ile ilgili 3 tür anahtar vardır:

- **Kek** -anahtar şifreleme anahtarı (CMK)
- **AEK** hesabı şifreleme anahtarı
- **Dek** -veri şifreleme anahtarı

Aşağıdaki kurallar geçerlidir:

- Log Analytics küme depolama hesapları, AEK olarak bilinen her depolama hesabı için benzersiz şifreleme anahtarı oluşturur.

- AEK, diske yazılan her veri bloğunu şifrelemek için kullanılan anahtarlar olan DEKs 'leri türetmek için kullanılır.

- Key Vault ' de anahtarınızı yapılandırıp kümede buna başvurduğunuzda, Azure depolama, veri şifreleme ve şifre çözme işlemleri gerçekleştirmek için AEK 'i sarmalamak ve sarmalamak üzere Azure Key Vault istekleri gönderir.

- KEK, Key Vault hiçbir durumda kalmayacak ve HSM anahtarı durumunda donanımdan ayrılmayacaktır.

- Azure depolama, Azure Active Directory üzerinden Azure Key Vault kimlik doğrulaması yapmak ve erişmek için *küme* kaynağıyla ilişkili yönetilen kimliği kullanır.

## <a name="cmk-provisioning-procedure"></a>CMK sağlama prosedürü

1. Aboneliğe izin verme--CMK özelliği adanmış Log Analytics kümelerine teslim edilir. Bölgenizde gerekli kapasiteye sahip olduğunuzu doğrulamak için aboneliğinizin önceden izin verilmesini istiyoruz. Aboneliğinizi izin verilen Microsoft Kişinizden yararlanın.
2. Azure Key Vault oluşturma ve anahtar depolama
3. Küme oluşturuluyor
4. Key Vault izinler veriliyor
5. Log Analytics çalışma alanlarını bağlama

CMK yapılandırması Azure portal desteklenmez ve sağlama [PowerShell](https://docs.microsoft.com/powershell/module/az.operationalinsights/), [CLI](https://docs.microsoft.com/cli/azure/monitor/log-analytics) veya [rest](https://docs.microsoft.com/rest/api/loganalytics/) istekleri aracılığıyla gerçekleştirilir.

### <a name="asynchronous-operations-and-status-check"></a>Zaman uyumsuz işlemler ve durum denetimi

Yapılandırma adımlarının bazıları hızla tamamlanamadığından zaman uyumsuz olarak çalışır. Yapılandırmada REST istekleri kullanılırken, yanıt başlangıçta HTTP durum kodu 200 (Tamam) ve üst bilgi döndürür. kabul edildiğinde *Azure-AsyncOperation* özelliği:
```json
"Azure-AsyncOperation": "https://management.azure.com/subscriptions/subscription-id/providers/Microsoft.OperationalInsights/locations/region-name/operationStatuses/operation-id?api-version=2020-08-01"
```

Ardından, *Azure-AsyncOperation* üst bilgi DEĞERINE bir get isteği göndererek zaman uyumsuz işlemin durumunu kontrol edebilirsiniz:
```rst
GET https://management.azure.com/subscriptions/subscription-id/providers/microsoft.operationalInsights/locations/region-name/operationstatuses/operation-id?api-version=2020-08-01
Authorization: Bearer <token>
```

Yanıt, işlem ve *durumu*hakkında bilgi içerir. Bu, bunlardan biri olabilir:

İşlem devam ediyor
```json
{
    "id": "Azure-AsyncOperation URL value from the GET operation",
    "name": "operation-id", 
    "status" : "InProgress", 
    "startTime": "2017-01-06T20:56:36.002812+00:00",
}
```

Anahtar tanımlayıcı güncelleştirme işlemi devam ediyor
```json
{
    "id": "Azure-AsyncOperation URL value from the GET operation",
    "name": "operation-id", 
    "status" : "Updating", 
    "startTime": "2017-01-06T20:56:36.002812+00:00",
    "endTime": "2017-01-06T20:56:56.002812+00:00",
}
```

Küme silme devam ediyor--bağlantılı çalışma alanlarına sahip bir kümeyi sildiğinizde, bağlantı kaldırma işlemi her çalışma alanı için zaman uyumsuz olarak gerçekleştirilir ve işlem biraz zaman alabilir.
Bu, bağlı bir çalışma alanı olmayan bir kümeyi sildiğinizde ilgili değildir; bu durumda küme hemen silinir.
```json
{
    "id": "Azure-AsyncOperation URL value from the GET operation",
    "name": "operation-id", 
    "status" : "Deleting", 
    "startTime": "2017-01-06T20:56:36.002812+00:00",
    "endTime": "2017-01-06T20:56:56.002812+00:00",
}
```

İşlem tamamlandı
```json
{
    "id": "Azure-AsyncOperation URL value from the GET operation",
    "name": "operation-id", 
    "status" : "Succeeded", 
    "startTime": "2017-01-06T20:56:36.002812+00:00",
    "endTime": "2017-01-06T20:56:56.002812+00:00",
}
```

İşlem başarısız oldu
```json
{
    "id": "Azure-AsyncOperation URL value from the GET operation",
    "name": "operation-id", 
    "status" : "Failed", 
    "startTime": "2017-01-06T20:56:36.002812+00:00",
    "endTime": "2017-01-06T20:56:56.002812+00:00",
    "error" : { 
        "code": "error-code",  
        "message": "error-message" 
    }
}
```

### <a name="allowing-subscription-for-cmk-deployment"></a>CMK dağıtımı için aboneliğe izin verme

CMK özelliği adanmış Log Analytics kümelerine teslim edilir.Bölgenizde gerekli kapasiteye sahip olduğunuzu doğrulamak için aboneliğinizin önceden izin verilmesini istiyoruz. Abonelik kimliklerinizi sağlamak için kişilerinizi Microsoft 'a kullanın.

> [!IMPORTANT]
> CMK özelliği bölgesel. Azure Key Vault, kümeniz ve bağlı Log Analytics çalışma alanlarınızın aynı bölgede olması gerekir, ancak bunlar farklı aboneliklerde olabilir.

### <a name="storing-encryption-key-kek"></a>Şifreleme anahtarını depolama (KEK)

Zaten oluşturmak için gereken bir Azure Key Vault oluşturun veya kullanın veya veri şifrelemesi için kullanılacak bir anahtarı içeri aktarın. Azure Key Vault anahtarınızı ve Azure Izleyici 'deki verilerinize erişimi korumak için kurtarılabilir olarak yapılandırılmalıdır. Bu yapılandırmayı Key Vault Özellikler altında doğrulayabilirsiniz, hem *geçici silme* hem de *Temizleme koruması* etkinleştirilmelidir.

![Geçici silme ve Temizleme koruması ayarları](media/customer-managed-keys/soft-purge-protection.png)

Bu ayarlar, CLı ve PowerShell aracılığıyla güncelleştirilebilen:

- [Geçici Silme](../../key-vault/general/soft-delete-overview.md)
- Geçici silme işleminden sonra bile gizli/kasaların silinmesini zorlamak için [koruma koruyucuları temizle](../../key-vault/general/soft-delete-overview.md#purge-protection)

### <a name="create-cluster"></a>Küme oluşturma

[Adanmış kümeler](https://docs.microsoft.com/azure/azure-monitor/log-query/logs-dedicated-clusters#creating-a-cluster)makalesinde gösterilen yordamı izleyin. 

> [!IMPORTANT]
> Sonraki adımlarda ayrıntılara ihtiyacınız olacağı için yanıtı kopyalayın ve kaydedin.

### <a name="grant-key-vault-permissions"></a>Key Vault izinleri verme

Kümenize izin vermek için Key Vault yeni bir erişim ilkesiyle güncelleştirin. Bu izinler, veri şifrelemesi için Azure Izleyici depolaması 'nın temelini oluşturmak için kullanılır. Key Vault Azure portal açın ve bu ayarlarla bir ilke oluşturmak için "erişim Ilkeleri" ve "+ erişim Ilkesi Ekle" seçeneğine tıklayın:

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

```powershell
Update-AzOperationalInsightsCluster -ResourceGroupName "resource-group-name" -ClusterName "cluster-name" -KeyVaultUri "key-uri" -KeyName "key-name" -KeyVersion "key-version"
```

> [!NOTE]
> Düzeltme ekini kullanarak kümenin *SKU 'su*, *keyvaultproperties* veya *billingtype* 'ı güncelleştirebilirsiniz.

```rst
PATCH https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters/<cluster-name>?api-version=2020-08-01
Authorization: Bearer <token>
Content-type: application/json

{
   "identity": { 
     "type": "systemAssigned" 
     },
   "sku": {
     "name": "capacityReservation",
     "capacity": 1000
     },
   "properties": {
    "billingType": "cluster",
     "KeyVaultProperties": {
       "KeyVaultUri": "https://<key-vault-name>.vault.azure.net",
       "KeyName": "<key-name>",
       "KeyVersion": "<current-version>"
       }
   },
   "location":"<region-name>"
}
```

**Response**

200 Tamam ve üst bilgi.
Anahtar tanımlayıcısının yayılması birkaç dakika sürer. Güncelleştirme durumunu iki şekilde kontrol edebilirsiniz:
1. Yanıttan Azure-AsyncOperation URL değerini kopyalayın ve [zaman uyumsuz işlemler durum denetimini](#asynchronous-operations-and-status-check)izleyin.
2. Küme üzerinde bir GET isteği gönderin ve *Keyvaultproperties* özelliklerine bakın. Son güncellenen anahtar tanımlayıcı ayrıntılarınız yanıta döndürmelidir.

Anahtar tanımlayıcı güncelleştirmesi tamamlandığında GET isteğinin yanıtı şuna benzemelidir:

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

## <a name="cmk-kek-revocation"></a>CMK (KEK) iptali

Anahtarınızı devre dışı bırakarak veya Key Vault kümenin erişim ilkesini silerek verilere erişimi iptal edebilirsiniz. Log Analytics küme depolaması, her zaman bir saat veya daha kısa bir süre içinde anahtar izinlerinde yapılan değişikliklere uyar ve depolama alanı kullanılamaz hale gelir. Kümenizle bağlantılı çalışma alanlarına alınan yeni veriler bırakılır ve geri alınamaz, verilere erişilemez ve bu çalışma alanlarına yönelik sorgular başarısız olur. Önceden alınan veriler, kümeniz ve çalışma alanlarınız silinmediği sürece depolamada kalır. Erişilemeyen veriler veri bekletme ilkesine tabidir ve bekletmeye ulaşıldığında temizlenir. 

Son 14 gün içinde alınan veriler, verimli sorgu altyapısı işlemi için etkin-önbellek (SSD-desteklenen) olarak da tutulur. Bu, anahtar iptali işleminde silinir ve erişilmez hale gelir.

Depolama, şifreleme anahtarını sarmalamadan ve erişildikten sonra 30 dakika içinde veri alımı ve sorgu sürdürülmeye çalışmak için Key Vault düzenli olarak yoklar.

## <a name="cmk-kek-rotation"></a>CMK (KEK) döndürme

CMK 'nin dönmesi, Azure Key Vault yeni anahtar sürümü ile kümeye açık güncelleştirme yapılmasını gerektirir. "Kümeyi anahtar tanımlayıcısı ayrıntıları ile güncelleştirme" adımındaki yönergeleri izleyin. Kümedeki yeni anahtar tanımlayıcı ayrıntılarını güncelleştirmemeniz durumunda Log Analytics küme depolaması şifreleme için önceki anahtarınızı kullanmaya devam edecektir. Kümedeki yeni anahtarı güncelleştirmeden önce eski anahtarınızı devre dışı bırakır veya silerseniz, [anahtar iptal](#cmk-kek-revocation) durumuna sahip olursunuz.

Verilerin her zaman Key Vault ' de yeni anahtar şifreleme anahtarı (KEK) ile şifrelenmesi sırasında, veriler her zaman hesap şifreleme anahtarıyla (AEK) şifrelendiğinden, tüm verileriniz anahtar döndürme işleminden sonra erişilebilir durumda kalır.

## <a name="cmk-for-queries"></a>Sorgular için CMK

Log Analytics ' de kullanılan sorgu dili ifade edilebilir ve sorgulara eklediğiniz açıklamalarda veya sorgu söz diziminde gizli bilgiler içerebilir. Bazı kuruluşlar, bu tür bilgilerin CMK ilkesinin bir parçası olarak korunmasını gerektirir ve sorgularınızı anahtarınızla şifreli olarak kaydetmeniz gerekir. Azure Izleyici, çalışma alanınıza bağlıyken kendi depolama hesabınızda anahtarınızla şifrelenen *kayıtlı aramaları* ve *günlük uyarıları* sorgularını depolamanıza olanak sağlar. 

> [!NOTE]
> Log Analytics sorguları, kullanılan senaryoya bağlı olarak çeşitli depolarda kaydedilebilir. CMK yapılandırmasına bakılmaksızın sorgular Microsoft Key (MMK) ile şifrelenmeye devam eder: Azure Izleyici 'deki çalışma kitapları, Azure panoları, Azure mantıksal uygulaması, Azure Notebooks ve Otomasyon Runbook 'Ları.

Kendi depolama alanınızı (BYOS) getirip çalışma alanınıza bağladığınızda, hizmet *kayıtlı aramaları* ve *günlük uyarıları* sorgularını depolama hesabınıza yükler. Diğer bir deyişle, Log Analytics kümesindeki verileri şifrelemek için kullandığınız anahtarı veya farklı bir anahtarı kullanarak depolama hesabını ve [geri kalan şifreleme ilkesini](../../storage/common/customer-managed-keys-overview.md) denetlersiniz. Bununla birlikte, bu depolama hesabıyla ilişkili maliyetlerden de sorumlu olursunuz. 

**Sorgular için CMK ayarlamadan önce dikkat edilecek noktalar**
* Hem çalışma alanınız hem de depolama hesabınızda ' Write ' izinlerine sahip olmanız gerekir
* Log Analytics çalışma alanınız bulunduğundan, depolama hesabınızı aynı bölgede oluşturduğunuzdan emin olun
* Depolamadaki *aramalar* , hizmet yapıtları olarak değerlendirilir ve bunların biçimi değişebilir
* Mevcut *kaydetme aramaları* çalışma alanınızdan kaldırılır. Yapılandırma öncesinde ihtiyacınız olan aramaları kopyalayın ve *kaydeder* . *Kayıtlı aramalarınızı* [PowerShell](/powershell/module/az.operationalinsights/get-azoperationalinsightssavedsearch) kullanarak görüntüleyebilirsiniz
* Sorgu geçmişi desteklenmiyor ve çalıştırdığınız sorguları göremezsiniz
* Sorguları kaydetmek amacıyla tek bir depolama hesabını çalışma alanına bağlayabilirsiniz, ancak her ikisi de *kaydedilmiş aramalar* ve *log-Alerts* sorguları kullanabilir.
* Panoya sabitle desteklenmiyor

**Kayıtlı arama sorguları için KCG 'LERI yapılandırma**

Çalışma alanınıza *sorgu* için bir depolama hesabı bağlayın-- *kayıtlı aramalar* sorguları depolama hesabınıza kaydedilir. 

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

## <a name="cmk-management"></a>CMK yönetimi

- **Bir kaynak grubundaki tüm kümeleri al**
  
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

  ```powershell
  Update-AzOperationalInsightsCluster -ResourceGroupName "resource-group-name" -ClusterName "cluster-name" -SkuCapacity "daily-ingestion-gigabyte"
  ```

  ```rst
  PATCH https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters/<cluster-name>?api-version=2020-08-01
  Authorization: Bearer <token>
  Content-type: application/json

  {
    "sku": {
      "name": "capacityReservation",
      "Capacity": 1000
    }
  }
  ```

- **Kümede *Billingtype* 'ı güncelleştirme**

  *Billingtype* özelliği, küme ve verileri için faturalandırma atısyonu belirler:
  - *küme* (varsayılan)--Faturalandırma, küme kaynağınızı barındıran abonelikle ilişkilidir
  - *çalışma alanları* --faturalandırma, çalışma alanlarınızı orantılı olarak barındıran aboneliklerle ilişkilidir
  
  [Güncelleştirme kümesini](#update-cluster-with-key-identifier-details) izleyin ve yeni billingType değerini sağlayın. Tam REST istek gövdesini sağlamanız gerekmez ve *Billingtype*öğesini içermelidir:

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

  ```powershell
  Remove-AzOperationalInsightsLinkedService -ResourceGroupName "resource-group-name" -Name "workspace-name" -LinkedServiceName cluster
  ```

  ```rest
  DELETE https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/workspaces/<workspace-name>/linkedservices/cluster?api-version=2020-08-01
  Authorization: Bearer <token>
  ```

  **Response**

  200 Tamam ve üst bilgi.

  Kaldırma işlemi Log Analytics depolama alanında depolandıktan sonra alınan veriler, bu işlemin tamamlanması 90 dakika sürebilir. Çalışma alanı bağlantısını kaldırma durumunu iki şekilde kontrol edebilirsiniz:

  1. Yanıttan Azure-AsyncOperation URL değerini kopyalayın ve [zaman uyumsuz işlemler durum denetimini](#asynchronous-operations-and-status-check)izleyin.
  2. [Çalışma alanları gönderme – Istek al](/rest/api/loganalytics/workspaces/get) ve yanıtı gözlemleyin, bağlantısız çalışma alanının *Özellikler*bölümünde *kümelekoya* sahip olmaz.

- **Çalışma alanı bağlantı durumunu denetle**
  
  Çalışma alanında Get işlemini gerçekleştirin ve *Kümeresourceıd* özelliğinin *Özellikler*altında yanıtta bulunup bulunmayacağını gözlemleyin. Bağlı bir çalışma alanı *Clusterresourceıd* özelliğine sahip olur.

  ```powershell
  Get-AzOperationalInsightsWorkspace -ResourceGroupName "resource-group-name" -Name "workspace-name"
  ```

- **Kümenizi silme**

  Bu işlemi gerçekleştirmek için kümede ' Write ' izinlerine sahip olmanız gerekir. Bir geçici silme işlemi, kümenizin, verileri 14 gün içinde, silme işleminin yanlışlıkla mi yoksa bilerek mi olduğunu da içerecek şekilde kurtarılmasına olanak tanımak için gerçekleştirilir. Küme adı, geçici silme döneminde ayrılmış kalır ve bu adla yeni bir küme oluşturamazsınız. Geçici silme süresinden sonra, küme adı serbest bırakılır ve kümeniz ile verileri kalıcı olarak silinir ve kurtarılamaz. Herhangi bir bağlı çalışma alanının, silme işleminde kümeden bağlantısı kaldırılır. Yeni alınan veriler Log Analytics depolama alanında depolanır ve Microsoft anahtarıyla şifrelenir. 
  
  Bağlantıyı kaldır işlemi zaman uyumsuzdur ve tamamlanması 90 dakika sürebilir.

  ```powershell
  Remove-AzOperationalInsightsCluster -ResourceGroupName "resource-group-name" -ClusterName "cluster-name"
  ```

  ```rst
  DELETE https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters/<cluster-name>?api-version=2020-08-01
  Authorization: Bearer <token>
  ```

  **Response**

  200 TAMAM

- **Kümenizi ve verilerinizi kurtarma** 
  
  Son 14 gün içinde silinen bir küme, geçici silme durumunda ve verileriyle kurtarılabilir. Tüm çalışma alanlarının silinme sırasında kümeden bağlantısı olmadığından, CMK şifrelemesi kurtarmasından sonra çalışma alanlarınızı yeniden bağlamanız gerekir. Kurtarma işlemi şu anda ürün grubu tarafından el ile gerçekleştirilir. Kurtarma isteklerinde Microsoft kanalınızı kullanın.

## <a name="limitationsandconstraints"></a>Sınırlamalar ve kısıtlamalar

- CMK, adanmış Log Analytics kümesinde desteklenir ve günde 1 GB ya da daha fazlasını gönderen müşteriler için uygundur.

- Bölge başına en fazla küme sayısı ve abonelik 2 ' dir

Kümeye en fazla bağlantılı çalışma alanı 100

- Bir çalışma alanını kümenize bağlayabilir ve ardından çalışma alanı için CMK gerekmiyorsa bağlantısını kaldırabilirsiniz. Belirli çalışma alanındaki çalışma alanı bağlantı işlemlerinin sayısı, 30 günlük bir dönemde 2 ile sınırlıdır.

- Kümeye çalışma alanı bağlantısı yalnızca Log Analytics kümesi sağlama tamamlandığını doğruladıktan sonra taşınmalıdır. Tamamlanmadan önce çalışma alanınıza gönderilen veriler bırakılır ve geri alınamaz.

- CMK şifrelemesi, CMK yapılandırmasından sonra yeni alınan veriler için geçerlidir. CMK yapılandırmasından önce alınan veriler, Microsoft anahtarıyla şifreli olarak kalır. CMK yapılandırmasından önce ve sonra gelen verileri sorunsuz bir şekilde sorgulayabilirsiniz.

- Azure Key Vault kurtarılabilir olarak yapılandırılmalıdır. Bu özellikler varsayılan olarak etkinleştirilmemiştir ve CLı veya PowerShell kullanılarak yapılandırılmalıdır:<br>
  - [Geçici Silme](../../key-vault/general/soft-delete-overview.md)
  - Geçici silme işleminden sonra bile gizli/kasaların silinmesini zorlamak için [Temizleme koruması](../../key-vault/general/soft-delete-overview.md#purge-protection) açılmalıdır.

- Küme başka bir kaynak grubuna veya aboneliğe taşıma Şu anda desteklenmiyor.

- Azure Key Vault, kümeniz ve bağlı çalışma alanlarınız aynı bölgede ve aynı Azure Active Directory (Azure AD) kiracısında olmalıdır, ancak farklı aboneliklerde olabilir.

- Başka bir kümeyle bağlantı varsa, kümeye çalışma alanı bağlantısı başarısız olur

## <a name="troubleshooting"></a>Sorun giderme

- Key Vault kullanılabilirliği ile davranış
  - Normal işlemde--depolama, kısa süreler için AEK önbelleğe alınır ve düzenli aralıklarla sarmalaması için Key Vault geri gider.
    
  - Geçici bağlantı hataları--depolama, anahtarların kısa bir süre boyunca önbellekte kalmasına izin vererek geçici hataları (zaman aşımları, bağlantı hataları, DNS sorunları) işler ve bu da tüm küçük sinyalleri 'leri kullanılabilirliğinden fazla. Sorgu ve alma özellikleri kesinti olmadan devam eder.
    
  - Canlı site--yaklaşık 30 dakikalık bir işlem, depolama hesabının kullanılamaz hale gelmesine neden olur. Sorgu özelliği kullanılamaz ve veri kaybını önlemek için Microsoft anahtar kullanarak birkaç saat boyunca önbelleğe alınır. Key Vault erişimi geri yüklendiğinde, sorgu kullanılabilir hale gelir ve geçici önbelleğe alınmış veriler veri deposuna alınır ve CMK ile şifrelenir.

  - Key Vault erişim oranı--Azure Izleyici depolaması 'nın sarmalama ve sarmalama işlemleri için Key Vault, 6 ila 60 saniye arasındadır.

- Bir küme oluşturur ve KeyVaultProperties 'i hemen belirtirseniz, sistem kimliği kümeye atanana kadar erişim ilkesi tanımlanmadığından işlem başarısız olabilir.

- Mevcut kümeyi KeyVaultProperties ile güncelleştirirseniz ve ' Get ' anahtar erişimi Ilkesi Key Vault eksikse, işlem başarısız olur.

- Bir küme oluştururken çakışma hatası alırsanız, kümenizi son 14 gün içinde silmiş ve bu da geçici silme döneminde olabilir. Küme adı, geçici silme döneminde ayrılmış kalır ve bu adla yeni bir küme oluşturamazsınız. Bu ad, küme kalıcı olarak silindiğinde geçici silme süresinden sonra serbest bırakılır.

- Bir işlem devam ederken kümenizi güncelleştirirseniz işlem başarısız olur.

- Kümenizin dağıtımı başarısız olursa, Azure Key Vault, kümenizin ve bağlı Log Analytics çalışma alanlarınızın aynı bölgede olduğunu doğrulayın. Farklı aboneliklerde olabilir.

- Anahtar sürümünüzü Key Vault güncelleştirir ve kümedeki yeni anahtar tanımlayıcı ayrıntılarını güncelleştirmemeniz durumunda, Log Analytics kümesi önceki anahtarınızı kullanmaya devam eder ve verileriniz erişilemez hale gelir. Veri alımı ve veri sorgulama yeteneği sağlamak için kümedeki yeni anahtar tanımlayıcı ayrıntılarını güncelleştirin.

- Bazı işlemler uzun sürer ve işlemin tamamlanması biraz zaman alabilir; bunlar küme oluşturma, küme anahtarı güncelleştirme ve küme silme işlemlerini gerçekleştirebilir. İşlem durumunu iki şekilde denetleyebilirsiniz:
  1. REST kullanırken, Azure-AsyncOperation URL değerini yanıttan kopyalayın ve [zaman uyumsuz işlemler durum denetimini](#asynchronous-operations-and-status-check)izleyin.
  2. Kümeye veya çalışma alanına GET isteği gönderin ve yanıtı gözlemleyin. Örneğin, bağlantısız çalışma alanının *Özellikler*bölümünde *kümeresourceıd* yok.

- Müşteri tarafından yönetilen anahtarla ilgili destek ve yardım için kişilerinizi Microsoft 'a kullanın.

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
