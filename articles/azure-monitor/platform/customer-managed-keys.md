---
title: Azure Izleyici müşteri tarafından yönetilen anahtar
description: Azure Key Vault bir anahtar kullanarak Log Analytics çalışma alanlarınızdaki verileri şifrelemek üzere müşteri tarafından yönetilen anahtar (CMK) yapılandırma hakkında bilgi ve adımlar.
ms.subservice: logs
ms.topic: conceptual
author: yossi-y
ms.author: yossiy
ms.date: 05/20/2020
ms.openlocfilehash: 037edb8af6e04a2ff65977a92a66482c9f4f880f
ms.sourcegitcommit: 1f25aa993c38b37472cf8a0359bc6f0bf97b6784
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/26/2020
ms.locfileid: "83845107"
---
# <a name="azure-monitor-customer-managed-key"></a>Azure Izleyici müşteri tarafından yönetilen anahtar 

Bu makalede, Log Analytics çalışma alanlarınız için müşteri tarafından yönetilen anahtarları (CMK) yapılandırmaya yönelik arka plan bilgileri ve adımlar sağlanmaktadır. Yapılandırıldıktan sonra, çalışma alanlarına gönderilen tüm veriler Azure Key Vault anahtarınızla şifrelenir.

Yapılandırmadan önce aşağıdaki [sınırlamaları ve kısıtlamaları](#limitationsandconstraints) incelemenizi öneririz.

## <a name="customer-managed-key-cmk-overview"></a>Müşteri tarafından yönetilen anahtar (CMK) genel bakış

[Bekleyen şifreleme](https://docs.microsoft.com/azure/security/fundamentals/encryption-atrest)   , kuruluşlarda ortak bir gizlilik ve güvenlik gereksinimidir.Azure 'un bekleyen şifrelemeyi tamamen yönetmesine izin verebilir, şifreleme veya şifreleme anahtarlarını yakından yönetmek için çeşitli seçenekleriniz vardır.

Azure Izleyici, Azure tarafından yönetilen anahtarlar kullanılarak tüm verilerin Rest 'te şifrelenmesini sağlar.Azure Izleyici Ayrıca, [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-overview)depolanan   ve sistem tarafından atanan [yönetilen kimlik](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview)doğrulaması kullanan depolama tarafından erişilen kendi anahtarınızı kullanarak veri şifrelemesi için bir seçenek sağlar   .Bu anahtar, [yazılım ya da donanım HSM korumalı](https://docs.microsoft.com/azure/key-vault/key-vault-overview)olabilir.

Azure Izleyici şifreleme kullanımı, [Azure depolama şifrelemesiyle](https://docs.microsoft.com/azure/storage/common/storage-service-encryption#about-azure-storage-encryption)aynı şekilde   çalışır.

CMK, verilerinize erişimi denetlemenize ve istediğiniz zaman iptal etmenize olanak tanır. Azure Izleyici depolaması, her zaman bir saat içindeki anahtar izinlerinde yapılan değişikliklere uyar. Son 14 gün içinde alınan veriler, verimli sorgu altyapısı işlemi için etkin-önbellek (SSD-desteklenen) olarak da tutulur. Bu veriler CMK yapılandırmasına bakılmaksızın Microsoft anahtarlarıyla şifreli olarak kalır, ancak SSD verileri üzerindeki denetiminiz, [anahtar iptalinde](#cmk-kek-revocation)kalır. 2020 ikinci yarısında CMK ile şifrelenen SSD verilerinin olması için çalışıyoruz.

CMK özelliği adanmış Log Analytics kümelerine teslim edilir. Bölgenizde gereken kapasiteye sahip olduğunuzu doğrulamak için aboneliğinizin daha önce beyaz listeye alınmasını istiyoruz. CMK 'yi yapılandırmaya başlamadan önce aboneliğinizi beyaz listeye almak için Microsoft kişinizi kullanın.

 [Log Analytics kümeleri fiyatlandırma modeli](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#log-analytics-dedicated-clusters)   1000 GB/gün düzeyinden itibaren kapasite rezervasyonları kullanır.

## <a name="how-cmk-works-in-azure-monitor"></a>Azure Izleyici 'de CMK nasıl çalışmaktadır

Azure Izleyici, Azure Key Vault erişim sağlamak için sistem tarafından atanan yönetilen kimliğin yararlanır. Sistem tarafından atanan yönetilen kimlik yalnızca tek bir Azure kaynağıyla ilişkilendirilebilir ancak Log Analytics kümesinin kimliği, küme düzeyinde desteklenirken, bu, CMK özelliğinin adanmış bir Log Analytics kümesinde teslim edildiğini belirler. Birden çok çalışma alanı üzerinde CMK 'yi desteklemek için yeni bir Log Analytics *küme* kaynağı, Key Vault ve Log Analytics çalışma alanlarınız arasında ara kimlik bağlantısı olarak gerçekleştirilir. Log Analytics küme depolaması, \' Azure Active Directory aracılığıyla Azure Key Vault kimlik doğrulaması yapmak Için *küme* kaynağıyla ilişkili yönetilen kimliği kullanır. 

CMK yapılandırmasından sonra, *küme* kaynağınızın ilişkili çalışma alanlarına alınan tüm veriler, Key Vault anahtarındaki anahtarınızla şifrelenir. Çalışma alanlarının herhangi bir zamanda *küme* kaynağıyla ilişkisini kaldırabilirsiniz. Yeni veriler, yeni ve eski verilerinizi sorunsuz bir şekilde sorgulayabilir Log Analytics depolama ve Microsoft anahtar ile şifrelenmiş olarak kullanıma alınır.


![CMK genel bakış](media/customer-managed-keys/cmk-overview-8bit.png)

1. Key Vault
2. Log Analytics *küme* kaynağı, Key Vault izinleri olan yönetilen kimliğe sahip--kimlik, adanmış Log Analytics küme depolamasına yayılmıştır
3. Adanmış Log Analytics kümesi
4. CMK şifrelemesi için *küme* kaynağıyla ilişkili çalışma alanları

## <a name="encryption-keys-operation"></a>Şifreleme anahtarları işlemi

Depolama veri şifrelemesi ile ilgili 3 tür anahtar vardır:

- **Kek** -anahtar şifreleme anahtarı (CMK)
- **AEK** hesabı şifreleme anahtarı
- **Dek** -veri şifreleme anahtarı

Aşağıdaki kurallar geçerlidir:

- Log Analytics küme depolama hesapları, AEK olarak bilinen her depolama hesabı için benzersiz şifreleme anahtarı oluşturur.

- AEK, diske yazılan her veri bloğunu şifrelemek için kullanılan anahtarlar olan DEKs 'leri türetmek için kullanılır.

- Key Vault ' de anahtarınızı yapılandırıp *küme* kaynağında buna başvurduğunuzda, Azure depolama, veri şifreleme ve şifre çözme işlemleri gerçekleştirmek için AEK 'i sarmalamak ve sarmalamak üzere Azure Key Vault istekleri gönderir.

- KEK, Key Vault hiçbir durumda kalmayacak ve HSM anahtarı durumunda donanımdan ayrılmayacaktır.

- Azure depolama, Azure Active Directory üzerinden Azure Key Vault kimlik doğrulaması yapmak ve erişmek için *küme* kaynağıyla ilişkili yönetilen kimliği kullanır.

## <a name="cmk-provisioning-procedure"></a>CMK sağlama prosedürü

1. Abonelik beyaz listesi--CMK özelliği adanmış Log Analytics kümelerine dağıtılır. Bölgenizde gereken kapasiteye sahip olduğunuzu doğrulamak için aboneliğinizin daha önce beyaz listeye alınmasını istiyoruz. Aboneliğinizi beyaz listeye almak için Microsoft kişinizi kullanın
2. Azure Key Vault oluşturma ve anahtar depolama
3. *Küme* kaynağı oluşturma
4. Key Vault izinler veriliyor
5. Log Analytics çalışma alanlarını ilişkilendirme

Yordam, şu anda Kullanıcı arabiriminde desteklenmez ve sağlama işlemi REST API aracılığıyla gerçekleştirilir.

> [!IMPORTANT]
> Herhangi bir API isteği, istek üst bilgisinde bir taşıyıcı yetkilendirme belirteci içermelidir.

Örneğin:

```rst
GET https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/Microsoft.OperationalInsights/workspaces/<workspace-name>?api-version=2020-03-01-preview
Authorization: Bearer eyJ0eXAiO....
```

Burada *eyJ0eXAiO....* tam yetkilendirme belirtecini temsil eder. 

Aşağıdaki yöntemlerden birini kullanarak belirteci edinebilirsiniz:

1. [Uygulama kayıtları](https://docs.microsoft.com/graph/auth/auth-concepts#access-tokens) yöntemi kullanın.
2. Azure portalında
    1. "Geliştirici Aracı" (F12) sırasında Azure portal git
    1. "Batch? api-Version" örneklerinden birinde "Istek üstbilgileri" altında yetkilendirme dizesini arayın. Şöyle görünür: "yetkilendirme: taşıyıcı eyJ0eXAiO....". 
    1. Aşağıdaki örneklere göre kopyalayıp API çağrına ekleyin.
3. Azure REST belge sitesine gidin. Herhangi bir API 'de "deneyin" düğmesine basın ve taşıyıcı belirtecini kopyalayın.

### <a name="asynchronous-operations-and-status-check"></a>Zaman uyumsuz işlemler ve durum denetimi

Bu yapılandırma yordamındaki bazı işlemler hızlı bir şekilde tamamlanamadığından zaman uyumsuz olarak çalışır. Zaman uyumsuz işlem yanıtı başlangıçta, kabul edildiğinde *Azure-AsyncOperation* özelliğine sahıp bir http durum kodu 200 (Tamam) ve üst bilgi döndürür:
```json
"Azure-AsyncOperation": "https://management.azure.com/subscriptions/subscription-id/providers/Microsoft.OperationalInsights/locations/region-name/operationStatuses/operation-id?api-version=2020-03-01-preview"
```

*Azure-AsyncOperation* üst bilgi DEĞERINE bir get isteği göndererek zaman uyumsuz işlemin durumunu kontrol edebilirsiniz:
```rst
GET https://management.azure.com/subscriptions/subscription-id/providers/microsoft.operationalInsights/locations/region-name/operationstatuses/operation-id?api-version=2020-03-01-preview
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

*Küme* kaynağı silme işlemi devam ediyor. çalışma alanları ilişkili çalışma alanları olan bir *küme* kaynağını sildiğinizde, zaman uyumlu olmayan işlemlerdeki her çalışma alanı için bir ilişkilendirme işlemi yapılır.
Bu, ilişkili bir çalışma alanı olmayan bir *kümeyi* sildiğinizde ilgili değildir; bu durumda *küme* kaynağı hemen silinir.
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

### <a name="subscription-whitelisting"></a>Abonelik beyaz listesi

CMK özelliği adanmış Log Analytics kümelerine teslim edilir.Bölgenizde gereken kapasiteye sahip olduğunuzu doğrulamak için aboneliğinizin daha önce beyaz listeye alınmasını istiyoruz. Abonelik kimliklerinizi sağlamak için kişilerinizi Microsoft 'a kullanın.

> [!IMPORTANT]
> CMK özelliği bölgesel. Azure Key Vault, *küme* kaynağınız ve ilişkili Log Analytics çalışma alanlarınızın aynı bölgede olması gerekir, ancak bunlar farklı aboneliklerde olabilir.

### <a name="storing-encryption-key-kek"></a>Şifreleme anahtarını depolama (KEK)

Zaten oluşturmak için gereken bir Azure Key Vault oluşturun veya kullanın veya veri şifrelemesi için kullanılacak bir anahtarı içeri aktarın. Azure Key Vault anahtarınızı ve Azure Izleyici 'deki verilerinize erişimi korumak için kurtarılabilir olarak yapılandırılmalıdır. Bu yapılandırmayı Key Vault Özellikler altında doğrulayabilirsiniz, hem *geçici silme* hem de *Temizleme koruması* etkinleştirilmelidir.

![Geçici silme ve Temizleme koruması ayarları](media/customer-managed-keys/soft-purge-protection.png)

Bu ayarlar CLı ve PowerShell aracılığıyla kullanılabilir:
- [Geçici Silme](https://docs.microsoft.com/azure/key-vault/key-vault-ovw-soft-delete)
- Geçici silme işleminden sonra bile gizli/kasaların silinmesini zorlamak için [koruma koruyucuları temizle](https://docs.microsoft.com/azure/key-vault/key-vault-ovw-soft-delete#purge-protection)

### <a name="create-cluster-resource"></a>*Küme* kaynağı oluştur

Bu kaynak, Key Vault ve Log Analytics çalışma alanlarınız arasında ara kimlik bağlantısı olarak kullanılır. Aboneliklerinizin beyaz listeye alınmasını doğruladıktan sonra, çalışma alanlarınızın bulunduğu bölgede bir Log Analytics *küme* kaynağı oluşturun.

*Küme* kaynağı oluştururken *Kapasite ayırma* düzeyini (SKU) belirtmeniz gerekir. *Kapasite ayırma* düzeyi günde 1.000 Ila 2.000 GB aralığında olabilir ve daha sonra 100 adımlarında güncelleştirebilirsiniz. Gün başına 2.000 GB 'den yüksek kapasite ayırma düzeyine ihtiyacınız varsa, adresinden bizimle iletişime geçin LAIngestionRate@microsoft.com . [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#log-analytics-clusters)

*Billingtype* özelliği *küme* kaynağı ve verileri için faturalandırma atışmasını belirler:
- *küme* (varsayılan)--Faturalandırma, *küme* kaynağınızı barındıran abonelikle ilişkilidir
- *çalışma alanları* --faturalandırma, çalışma alanlarınızı orantılı olarak barındıran aboneliklerle ilişkilidir

> [!NOTE]
> *Küme* kaynağını oluşturduktan sonra, düzeltme eki Rest isteği kullanarak *SKU*, *Keyvaultproperties* veya *billingtype* ile güncelleştirebilirsiniz.

**Oluştur**

Bu Kaynak Yöneticisi isteği zaman uyumsuz bir işlemdir.

```rst
PUT https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters/<cluster-name>?api-version=2020-03-01-preview
Authorization: Bearer <token>
Content-type: application/json

{
  "identity": {
    "type": "systemAssigned"
    },
  "sku": {
    "name": "capacityReservation",
    "Capacity": 1000
    },
  "properties": {
    "billingType": "cluster",
    },
  "location": "<region-name>",
}
```

Kimlik, oluşturma zamanında *küme* kaynağına atanır.

**Yanıtıyla**

200 Tamam ve üst bilgi.

Log Analytics kümesinin tamamlanmasını sağlarken, sağlama durumunu iki şekilde kontrol edebilirsiniz:

1. Yanıttan Azure-AsyncOperation URL değerini kopyalayın ve [zaman uyumsuz işlemler durum denetimini](#asynchronous-operations-and-status-check)izleyin.
2. *Küme* KAYNAĞıNDA bir get isteği gönderin ve *provisioningstate* değerine bakın. Sağlama sırasında *Provisioningaccount* , tamamlandığında *başarılı oldu* .

```rst
GET https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters/<cluster-name>?api-version=2020-03-01-preview
Authorization: Bearer <token>
```

> [!IMPORTANT]
> Sonraki adımlarda ayrıntılara ihtiyacınız olacağı için yanıtı kopyalayın ve kaydedin.

**Yanıtıyla**

```json
{
  "identity": {
    "type": "SystemAssigned",
    "tenantId": "tenant-id",
    "principalId": "principal-id"
    },
  "sku": {
    "name": "capacityReservation",
    "capacity": 1000,
    "lastSkuUpdate": "Sun, 22 Mar 2020 15:39:29 GMT"
    },
  "properties": {
    "provisioningState": "ProvisioningAccount",
    "clusterType": "LogAnalytics",
    "billingType": "cluster",
    "clusterId": "cluster-id"
    },
  "id": "/subscriptions/subscription-id/resourceGroups/resource-group-name/providers/Microsoft.OperationalInsights/clusters/cluster-name",
  "name": "cluster-name",
  "type": "Microsoft.OperationalInsights/clusters",
  "location": "region-name"
}
```

"PrincipalId" GUID 'SI, *küme* kaynağı için yönetilen kimlik hizmeti tarafından oluşturulur.

### <a name="grant-key-vault-permissions"></a>Key Vault izinleri verme

Key Vault, *küme* kaynağınız için izin veren yeni bir erişim ilkesiyle güncelleştirin. Bu izinler, veri şifrelemesi için Azure Izleyici depolaması 'nın temelini oluşturmak için kullanılır. Key Vault Azure portal açın ve bu ayarlarla bir ilke oluşturmak için "erişim Ilkeleri" ve "+ erişim Ilkesi Ekle" seçeneğine tıklayın:

- Anahtar izinleri: ' Get ', ' Wrap Key ' ve ' Wrap Key ' izinlerini seçin.
- Sorumlu seçin: önceki adımda verilen yanıtta döndürülen asıl kimlik değerini girin.

![Key Vault izinleri verme](media/customer-managed-keys/grant-key-vault-permissions-8bit.png)

Key Vault, anahtarınızı korumak ve Azure Izleyici verilerinize erişmek için kurtarılabilir olarak yapılandırıldığını doğrulamak için *Get* izni gerekir.

### <a name="update-cluster-resource-with-key-identifier-details"></a>Anahtar tanımlayıcı ayrıntıları ile küme kaynağını güncelleştirme

Bu adım, Key Vault ilk ve gelecekteki önemli sürüm güncelleştirmelerinde gerçekleştirilir. Veri şifreleme için kullanılacak anahtar sürümü hakkında Azure Izleyici depolama 'Ya bildirir. Güncelleştirilirken, yeni anahtarınız, depolama anahtarını sarmalamak ve geri sarmak için kullanılır (AEK).

Key Vault *anahtar* tanımlayıcı ayrıntılarınız ile *küme* kaynağını güncelleştirmek için, anahtar tanımlayıcı ayrıntılarını almak üzere Azure Key Vault anahtarın geçerli sürümünü seçin.

![Key Vault izinleri verme](media/customer-managed-keys/key-identifier-8bit.png)

KeyVaultProperties *küme* kaynağını anahtar tanımlayıcı ayrıntıları ile güncelleştirin.

**Güncelleştir**

Bu Kaynak Yöneticisi isteği, anahtar tanımlayıcı ayrıntıları güncelleştirilirken zaman uyumsuz bir işlemdir, kapasite değeri güncelleştirilirken zaman uyumludur.

> [!NOTE]
> Bir *SKU*, *keyvaultproperties* veya *Billingtype*güncelleştirmek için *küme* kaynağında kısmi gövde sağlayabilirsiniz.

```rst
PATCH https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters/<cluster-name>?api-version=2020-03-01-preview
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

"KeyVaultProperties" Key Vault anahtar tanımlayıcısı ayrıntılarını içerir.

**Yanıtıyla**

200 Tamam ve üst bilgi.
Anahtar tanımlayıcısının yayılması birkaç dakika sürer. Güncelleştirme durumunu iki şekilde kontrol edebilirsiniz:
1. Yanıttan Azure-AsyncOperation URL değerini kopyalayın ve [zaman uyumsuz işlemler durum denetimini](#asynchronous-operations-and-status-check)izleyin.
2. *Küme* KAYNAĞıNDA bir get isteği gönderin ve *keyvaultproperties* özelliklerine bakın. Son güncellenen anahtar tanımlayıcı ayrıntılarınız yanıta döndürmelidir.

Anahtar tanımlayıcı güncelleştirmesi tamamlandığında, *küme* kaynağında GET isteğinin yanıtı şuna benzemelidir:

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
    "clusterType": "LogAnalytics", 
    "billingType": "cluster",
    "clusterId": "cluster-id"
  },
  "id": "/subscriptions/subscription-id/resourceGroups/resource-group-name/providers/Microsoft.OperationalInsights/clusters/cluster-name",
  "name": "cluster-name",
  "type": "Microsoft.OperationalInsights/clusters",
  "location": "region-name"
}
```

### <a name="workspace-association-to-cluster-resource"></a>*Küme* kaynağıyla çalışma alanı ilişkilendirmesi

Bu işlemi gerçekleştirmek için hem çalışma alanınız hem de *küme* kaynağınız için ' Write ' izinlerinizin olması gerekir ve bu işlem bu eylemleri içerir:

- Çalışma alanında: Microsoft. Operationalınsights/çalışma alanları/yazma
- *Küme* kaynağında: Microsoft. Operationalınsights/kümeler/yaz

> [!IMPORTANT]
> Bu adım yalnızca Log Analytics kümesi sağlama işleminin tamamlanmasından sonra gerçekleştirilmelidir. Sağlama öncesinde çalışma alanlarını ve veri alma verilerini ilişkilendirirseniz, alınan veriler bırakılır ve kurtarılmayacaktır.

**Çalışma alanını ilişkilendirme**

Bu Kaynak Yöneticisi isteği zaman uyumsuz bir işlemdir.

```rst
PUT https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/workspaces/<workspace-name>/linkedservices/cluster?api-version=2020-03-01-preview 
Authorization: Bearer <token>
Content-type: application/json

{
  "properties": {
    "WriteAccessResourceId": "/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/clusters/<cluster-name>"
    }
}
```

**Yanıtıyla**

200 Tamam ve üst bilgi.

Alınan veriler ilişkilendirme işleminden sonra yönetilen anahtarınızla şifrelenmiş olarak depolanır, bu da tamamlanması 90 dakika sürebilir. Çalışma alanı ilişkilendirme durumunu iki şekilde kontrol edebilirsiniz:

1. Yanıttan Azure-AsyncOperation URL değerini kopyalayın ve [zaman uyumsuz işlemler durum denetimini](#asynchronous-operations-and-status-check)izleyin.
2. [Çalışma alanları gönderme – Istek al](https://docs.microsoft.com/rest/api/loganalytics/workspaces/get) ve yanıtı gözlemleyin, ilişkili çalışma alanı "Özellikler" altında bir kümelekoya sahip olur.

```rest
GET https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalInsights/workspaces/<workspace-name>?api-version=2020-03-01-preview
Authorization: Bearer <token>
```

**Yanıtıyla**

```json
{
  "properties": {
    "source": "Azure",
    "customerId": "workspace-name",
    "provisioningState": "Succeeded",
    "sku": {
      "name": "pricing-tier-name",
      "lastSkuUpdate": "Tue, 28 Jan 2020 12:26:30 GMT"
    },
    "retentionInDays": 31,
    "features": {
      "legacy": 0,
      "searchVersion": 1,
      "enableLogAccessUsingOnlyResourcePermissions": true,
      "clusterResourceId": "/subscriptions/subscription-id/resourceGroups/resource-group-name/providers/Microsoft.OperationalInsights/clusters/cluster-name"
    },
    "workspaceCapping": {
      "dailyQuotaGb": -1.0,
      "quotaNextResetTime": "Tue, 28 Jan 2020 14:00:00 GMT",
      "dataIngestionStatus": "RespectQuota"
    }
  },
  "id": "/subscriptions/subscription-id/resourcegroups/resource-group-name/providers/microsoft.operationalinsights/workspaces/workspace-name",
  "name": "workspace-name",
  "type": "Microsoft.OperationalInsights/workspaces",
  "location": "region-name"
}
```

## <a name="cmk-kek-revocation"></a>CMK (KEK) iptali

Anahtarınızı devre dışı bırakarak veya Key Vault *küme* kaynağı erişim ilkesini silerek verilere erişimi iptal edebilirsiniz. Log Analytics küme depolaması, her zaman bir saat veya daha kısa bir süre içinde anahtar izinlerinde yapılan değişikliklere uyar ve depolama alanı kullanılamaz hale gelir.  *Küme*kaynağınız ile ilişkili çalışma alanlarına alınan yeni veriler   bırakılır ve geri alınamaz, verilere erişilemez ve bu çalışma alanlarına yönelik sorgular başarısız olur. Önceden alınan veriler, *küme* kaynağınız ve çalışma alanlarınız silinmediği sürece depolamada kalır. Erişilemeyen veriler veri bekletme ilkesine tabidir ve bekletmeye ulaşıldığında temizlenir. 

Son 14 gün içinde alınan veriler, verimli sorgu altyapısı işlemi için etkin-önbellek (SSD-desteklenen) olarak da tutulur. Bu, anahtar iptali işleminde silinir ve erişilmez hale gelir.

Depolama, şifreleme anahtarını sarmalamadan ve erişildikten sonra 30 dakika içinde veri alımı ve sorgu sürdürülmeye çalışmak için Key Vault düzenli olarak yoklar.

## <a name="cmk-kek-rotation"></a>CMK (KEK) döndürme

CMK 'nin dönmesi, *küme* kaynağında Azure Key Vault yeni anahtar sürümü ile açık güncelleştirme yapılmasını gerektirir. " *Küme* kaynağını anahtar tanımlayıcı ayrıntıları ile güncelleştirme" adımındaki yönergeleri izleyin. *Küme* kaynağında yeni anahtar tanımlayıcı ayrıntılarını güncelleştirmemeniz durumunda Log Analytics küme depolaması şifreleme için önceki anahtarınızı kullanmaya devam edecektir. *Küme* kaynağında yeni anahtarı güncelleştirmeden önce eski anahtarınızı devre dışı bırakır veya silerseniz, [anahtar iptal](#cmk-kek-revocation) durumuna sahip olursunuz.

Verilerin her zaman Key Vault ' de yeni anahtar şifreleme anahtarı (KEK) ile şifrelenmesi sırasında, veriler her zaman hesap şifreleme anahtarıyla (AEK) şifrelendiğinden, tüm verileriniz anahtar döndürme işleminden sonra erişilebilir durumda kalır.

## <a name="cmk-manage"></a>CMK Yönet

- **Kaynak grubu için tüm *küme* kaynaklarını al**

  ```rst
  GET https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters?api-version=2020-03-01-preview
  Authorization: Bearer <token>
  ```

  **Yanıtıyla**
  
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
          "clusterType": "LogAnalytics", 
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

- **Bir abonelik için tüm *küme* kaynaklarını al**

  ```rst
  GET https://management.azure.com/subscriptions/<subscription-id>/providers/Microsoft.OperationalInsights/clusters?api-version=2020-03-01-preview
  Authorization: Bearer <token>
  ```
    
  **Yanıtıyla**
    
  ' Kaynak grubu için*küme* kaynakları ' için aynı yanıt, ancak abonelik kapsamı.

- ***Küme* kaynağında *Kapasite ayırmayı* Güncelleştir**

  İlişkili çalışma alanlarınızın veri hacmi zaman içinde değişiklik yaparken ve kapasite ayırma düzeyini uygun şekilde güncellemek istediğinizde. [Güncelleştirme *kümesi* kaynağını](#update-cluster-resource-with-key-identifier-details) izleyin ve yeni kapasite değerini sağlayın. Bu, gün başına 1.000 GB ve 100 adımlarında 2.000 arasında olabilir. Günde 2.000 GB 'den yüksek düzey için, Microsoft kişinize ulaşın. Tam REST istek gövdesini sağlamanız ve SKU 'yu içermesi gerektiğini unutmayın:

  **bölümü**
  ```json
  {
    "sku": {
      "name": "capacityReservation",
      "Capacity": 1000
    }
  }
  ```

- ***Küme* kaynağında *billingtype* güncelleştirme**

  *Billingtype* özelliği *küme* kaynağı ve verileri için faturalandırma atışmasını belirler:
  - *küme* (varsayılan)--Faturalandırma, küme kaynağınızı barındıran abonelikle ilişkilidir
  - *çalışma alanları* --faturalandırma, çalışma alanlarınızı orantılı olarak barındıran aboneliklerle ilişkilidir
  
  [Güncelleştirme *kümesi* kaynağını](#update-cluster-resource-with-key-identifier-details) Izleyin ve yeni billingtype değerini sağlayın. Tam REST istek gövdesini sağlamanız gerekmez ve *Billingtype*öğesini içermelidir:

  **bölümü**
  ```json
  {
    "properties": {
      "billingType": "cluster",
      }  
  }
  ``` 

- **Çalışma alanının ilişkisini kaldır**

  Bu işlemi gerçekleştirmek için çalışma alanında ve *küme* kaynağında ' yazma ' izinlerine sahip olmanız gerekir. Bir çalışma alanının *küme* kaynağından ilişkisini dilediğiniz zaman kaldırabilirsiniz. Yeniden ilişkilendirme işleminden sonra yeni alınan veriler Log Analytics depolama alanında depolanır ve Microsoft anahtarıyla şifrelenir. *Küme* kaynağı sağlandığı ve geçerli Key Vault anahtarıyla yapılandırıldığı sürece, aynı şekilde, çalışma alanınıza alınan verileri, aynı şekilde yeniden ilişkilendirmeden önce ve sonra sorunsuz bir şekilde sorgulayabilirsiniz.

  Bu Kaynak Yöneticisi isteği zaman uyumsuz bir işlemdir.

  ```rest
  DELETE https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/workspaces/<workspace-name>/linkedservices/cluster?api-version=2020-03-01-preview
  Authorization: Bearer <token>
  ```

  **Yanıtıyla**

  200 Tamam ve üst bilgi.

  Verileri ayırma işlemi Log Analytics depolama alanında depolandıktan sonra alınan veriler, bu işlemin tamamlanması 90 dakika sürebilir. Çalışma alanı ilişkisini kaldırma durumunu iki şekilde denetleyebilirsiniz:

  1. Yanıttan Azure-AsyncOperation URL değerini kopyalayın ve [zaman uyumsuz işlemler durum denetimini](#asynchronous-operations-and-status-check)izleyin.
  2. [Çalışma alanları gönderme – Istek al](https://docs.microsoft.com/rest/api/loganalytics/workspaces/get) ve yanıtı gözlemleyin, ilişkisi kaldırılmış çalışma alanı *Özellikler*altında *kümelekoya* sahip olmayacaktır.

- ***Küme* kaynağınızı silme**

  Bu işlemi gerçekleştirmek için *küme* kaynağında ' yazma ' izinlerine sahip olmanız gerekir. Silme işleminin yanlışlıkla mi yoksa bilerek mi olduğunu, verileri 14 gün içinde içeren *küme* kaynağınızın kurtarılmasına izin vermek için bir geçici silme işlemi yapılır. *Küme* kaynak adı, geçici silme döneminde ayrılmış kalır ve bu adla yeni bir küme oluşturamazsınız. Geçici silme süresinden sonra, *küme* kaynağı adı serbest bırakılır, *küme* kaynağınız ve verileriniz kalıcı olarak silinir ve kurtarılamaz. Herhangi bir ilişkili çalışma alanı, silme işleminde *küme* kaynağı ile ilişkisi alınır. Yeni alınan veriler Log Analytics depolama alanında depolanır ve Microsoft anahtarıyla şifrelenir. İlişkisi kaldırılan çalışma alanları işlemi zaman uyumsuzdur ve tamamlanması 90 dakika sürebilir.

  ```rst
  DELETE https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters/<cluster-name>?api-version=2020-03-01-preview
  Authorization: Bearer <token>
  ```

  **Yanıtıyla**

  200 TAMAM

- ***Küme* kaynağınızı ve verilerinizi kurtarma** 
  
  Son 14 gün içinde silinen bir *küme* kaynağı, geçici silme durumunda ve verileriyle kurtarılabilir. *Küme* kaynağı silme ile *küme* kaynağı ile tüm çalışma alanları ilişkilendirmesi yaptığından, CMK şifrelemesi kurtarmasından sonra çalışma alanlarınızı yeniden ilişkilendirmeniz gerekir. Kurtarma işlemi şu anda ürün grubu tarafından el ile gerçekleştirilir. Kurtarma isteklerinde Microsoft kanalınızı kullanın.

## <a name="limitationsandconstraints"></a>Sınırlamalar ve kısıtlamalar

-CMK, adanmış Log Analytics kümesinde desteklenir ve günde 1 GB ya da daha fazlasını gönderen müşteriler için uygundur.

-Her bölge ve abonelik için en fazla *küme*   kaynağı sayısı 2 ' dir

-Çalışma alanı *Cluster*   için CMK gerekmiyorsa, bir çalışma alanını küme kaynağınız ile ilişkilendirebilir ve ardından ilişkiyi kaldırabilirsiniz.30 günlük bir dönemdeki belirli çalışma alanındaki çalışma alanı ilişkisinin sayısı 2 ile sınırlıdır

- *Küme*kaynağıyla çalışma alanı ilişkilendirmesi   yalnızca Log Analytics kümesi sağlama tamamlandığını doğruladıktan sonra taşınmalıdır.Tamamlanmadan önce çalışma alanınıza gönderilen veriler bırakılır ve geri alınamaz.

-CMK şifrelemesi, CMK yapılandırmasından sonra yeni alınan veriler için geçerlidir      .CMK yapılandırmasından önce alınan veriler      , Microsoft anahtarıyla şifreli olarak kalır.    CMK yapılandırmasından önce ve sonra gelen verileri sorunsuz bir şekilde sorgulayabilirsiniz.

-Azure Key Vault kurtarılabilir olarak yapılandırılmalıdır.Bu özellikler varsayılan olarak etkin değildir ve CLI veya PowerShell kullanılarak yapılandırılmalıdır:    -  [geçici silme](https://docs.microsoft.com/azure/key-vault/key-vault-ovw-soft-delete) 
      özelliği açık olmalıdır, geçici silme    -  [Purge protection](https://docs.microsoft.com/azure/key-vault/key-vault-ovw-soft-delete#purge-protection)   sonrasında bile gizli/kasaların silinmesini önlemek için, temizleme koruması etkinleştirilmiş olmalıdır.

- *Küme*   kaynak başka bir kaynak grubuna veya aboneliğe geçiş      Şu anda desteklenmiyor.

-Azure Key Vault, *küme*   kaynağı ve ilişkili çalışma alanlarınızın aynı bölgede ve aynı Azure ACTIVE DIRECTORY (Azure AD) kiracısında olması gerekir, ancak bunlar farklı aboneliklerde olabilir.

- *Küme*kaynağıyla çalışma alanı ilişkilendirmesi,        başka bir *küme*kaynağıyla ilişkiliyse başarısız olur  

## <a name="troubleshooting"></a>Sorun giderme

- Key Vault kullanılabilirliği ile davranış
  - Normal işlemde--depolama, kısa süreler için AEK önbelleğe alınır ve düzenli aralıklarla sarmalaması için Key Vault geri gider.
    
  - Geçici bağlantı hataları--depolama, anahtarların kısa bir süre boyunca önbellekte kalmasına izin vererek geçici hataları (zaman aşımları, bağlantı hataları, DNS sorunları) işler ve bu da tüm küçük sinyalleri 'leri kullanılabilirliğinden fazla. Sorgu ve alma özellikleri kesinti olmadan devam eder.
    
  - Canlı site--yaklaşık 30 dakikalık bir işlem, depolama hesabının kullanılamaz hale gelmesine neden olur. Sorgu özelliği kullanılamaz ve veri kaybını önlemek için Microsoft anahtar kullanarak birkaç saat boyunca önbelleğe alınır. Key Vault erişimi geri yüklendiğinde, sorgu kullanılabilir hale gelir ve geçici önbelleğe alınmış veriler veri deposuna alınır ve CMK ile şifrelenir.

  - Key Vault erişim oranı--Azure Izleyici depolaması 'nın sarmalama ve sarmalama işlemleri için Key Vault, 6 ila 60 saniye arasındadır.

- Bir *küme* kaynağı oluşturur ve KeyVaultProperties 'i hemen belirtirseniz, sistem kimliği *küme* kaynağına atanana kadar erişim ilkesi tanımlanmadığından işlem başarısız olabilir.

- Mevcut *küme* kaynağını KeyVaultProperties ile güncelleştirirseniz ve ' Get ' anahtar erişim ilkesi Key Vault eksik olduğunda, işlem başarısız olur.

- Bir çalışma alanıyla ilişkili bir *küme* kaynağını silmeye çalışırsanız, silme işlemi başarısız olur.

- Bir *küme* kaynağı oluştururken çakışma hatası alırsanız, *küme* kaynağınızı son 14 gün içinde silmiş ve bu da geçici silme döneminde olabilir. *Küme* kaynak adı, geçici silme döneminde ayrılmış kalır ve bu adla yeni bir küme oluşturamazsınız. Ad, *küme* kaynağı kalıcı olarak silindiğinde geçici silme süresinden sonra serbest bırakılır.

- Bir işlem devam ederken *küme* kaynağınızı güncelleştirirseniz, işlem başarısız olur.

- *Küme* kaynağınızı dağıtmanız başarısız olursa Azure Key Vault, *küme*   kaynağı ve ilişkili Log Analytics çalışma alanlarınızın aynı bölgede olduğunu doğrulayın. Farklı aboneliklerde olabilir.

- Anahtar sürümünüzü Key Vault güncelleştirir ve *küme* kaynağındaki yeni anahtar tanımlayıcı ayrıntılarını güncelleştirmemeniz durumunda, Log Analytics kümesi önceki anahtarınızı kullanmaya devam eder ve verileriniz erişilemez hale gelir. Veri alımı ve veri sorgulama yeteneği sağlamak için *küme* kaynağında yeni anahtar tanımlayıcı ayrıntılarını güncelleştirin.

- Müşteri tarafından yönetilen anahtarla ilgili destek ve yardım için kişilerinizi Microsoft 'a kullanın.
