---
title: Azure Izleyici müşteri tarafından yönetilen anahtar
description: Azure Key Vault bir anahtar kullanarak Log Analytics çalışma alanlarınızdaki verileri şifrelemek üzere müşteri tarafından yönetilen anahtar (CMK) yapılandırma hakkında bilgi ve adımlar.
ms.subservice: logs
ms.topic: conceptual
author: yossi-y
ms.author: yossiy
ms.date: 05/04/2020
ms.openlocfilehash: 64dd56339244364340ea9b78fbbe1e63cf611c44
ms.sourcegitcommit: 31236e3de7f1933be246d1bfeb9a517644eacd61
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/04/2020
ms.locfileid: "82780836"
---
# <a name="azure-monitor-customer-managed-key-configuration"></a>Azure Izleyici müşteri tarafından yönetilen anahtar yapılandırması 

Bu makalede, Log Analytics çalışma alanlarınız için müşteri tarafından yönetilen anahtarları (CMK) yapılandırmaya yönelik arka plan bilgileri ve adımlar sağlanmaktadır. Yapılandırıldıktan sonra, çalışma alanlarına gönderilen tüm veriler Azure Key Vault anahtarınızla şifrelenir.

Yapılandırmadan önce aşağıdaki [sınırlamaları ve kısıtlamaları](#limitations-and-constraints) incelemenizi öneririz.

## <a name="disclaimers"></a>Bildirimler

- Azure Izleyici CMK, bir erken erişim özelliğidir ve kayıtlı abonelikler için etkinleştirilmiştir.

- Bu makalede açıklanan CMK dağıtımı, üretim kalitesiyle birlikte dağıtılır ve bir erken erişim özelliği olduğundan desteklenir.

- CMK özelliği, bir Azure Veri Gezgini (ADX) kümesi olan ve günde 1 GB ya da daha fazlasını gönderen müşterilere uygun olan adanmış bir veri deposu kümesine dağıtılır. 

- CMK fiyatlandırma modeli şu anda kullanılamaz ve bu makalede ele alınmamaktadır. Takvim yılının (CY) 2020 ' nin ikinci çeyreğinde adanmış ADX kümesine yönelik fiyatlandırma modeli beklenir ve var olan CMK dağıtımları için geçerli olacaktır.

## <a name="customer-managed-key-cmk-overview"></a>Müşteri tarafından yönetilen anahtar (CMK) genel bakış

[Bekleyen şifreleme](https://docs.microsoft.com/azure/security/fundamentals/encryption-atrest) , kuruluşlarda ortak bir gizlilik ve güvenlik gereksinimidir. Azure 'un bekleyen şifrelemeyi tamamen yönetmesine izin verebilir, şifreleme veya şifreleme anahtarlarını yakından yönetmek için çeşitli seçenekleriniz vardır.

Azure Izleyici veri deposu, Azure depolama 'da depolanırken Azure tarafından yönetilen anahtarlar kullanılarak bekleyen tüm verilerin şifrelenmesini sağlar. Azure Izleyici Ayrıca, sistem tarafından atanan [yönetilen kimlik](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) doğrulaması kullanılarak erişilen [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-overview)depolanan kendi anahtarınızı kullanarak veri şifrelemesi için bir seçenek sağlar. Bu anahtar, [yazılım ya da donanım HSM korumalı](https://docs.microsoft.com/azure/key-vault/key-vault-overview)olabilir.
Azure Izleyici şifreleme kullanımı, [Azure depolama şifrelemesiyle](https://docs.microsoft.com/azure/storage/common/storage-service-encryption#about-azure-storage-encryption) aynı şekilde çalışır.

Azure Izleyici depolama 'nın sarmalama ve sarmalama işlemleri için Key Vault erişim sıklığı, 6 ila 60 saniye arasındadır.Azure Izleyici depolaması, her zaman bir saat içindeki anahtar izinlerinde yapılan değişikliklere uyar.

Son 14 gün içinde alınan veriler, verimli sorgu altyapısı işlemi için etkin-önbellek (SSD-desteklenen) olarak da tutulur. Bu veriler CMK yapılandırmasına bakılmaksızın Microsoft anahtarlarıyla şifreli olarak kalır, ancak SSD 2020 'nin ilk yarısında CMK ile şifrelendiğinden SSD 'yi şifrelebilmemiz için çalışıyoruz.

## <a name="how-cmk-works-in-azure-monitor"></a>Azure Izleyici 'de CMK nasıl çalışmaktadır

Azure Izleyici, Azure Key Vault erişim sağlamak için sistem tarafından atanan yönetilen kimliğin yararlanır.Sistem tarafından atanan yönetilen kimlik, yalnızca tek bir Azure kaynağıyla ilişkilendirilebilir. Azure Izleyici veri deposu (ADX kümesi) kimliği, küme düzeyinde desteklenir ve bu, CMK özelliğinin adanmış bir ADX kümesine teslim edildiğini belirler. Birden çok çalışma alanı üzerinde CMK 'yi desteklemek için yeni bir Log Analytics kaynak (*küme*) Key Vault ve Log Analytics çalışma alanlarınız arasında ara kimlik bağlantısı olarak gerçekleştirilir. Bu kavram, sistem tarafından atanan kimlik kısıtlamasına uygundur ve kimlik, ADX kümesi ile Log Analytics *küme* kaynağı arasında saklanır, ancak tüm ilişkili çalışma alanlarının verileri Key Vault anahtarınızla korunur. Underlay ADX küme depolaması, Azure Key Vault kimlik doğrulaması yapmak ve\'Azure Active Directory aracılığıyla erişmek için *küme* kaynağıyla ilişkili yönetilen kimliği kullanır.

![CMK genel bakış](media/customer-managed-keys/cmk-overview-8bit.png)
1.    Müşterinin Key Vault.
2.    Müşterinin Log Analytics *küme* kaynağı, Key Vault izinleri olan yönetilen kimliğe sahip: kimlik, veri deposu (ADX kümesi) düzeyinde desteklenir.
3.    Azure Izleyici adanmış ADX kümesi.
4.    CMK şifrelemesi için *küme* kaynağıyla ilişkili müşteri çalışma alanları.

## <a name="encryption-keys-operation"></a>Şifreleme anahtarları işlemi

Depolama veri şifrelemesi ile ilgili 3 tür anahtar vardır:

- **Kek** -anahtar şifreleme anahtarı (CMK)
- **AEK** hesabı şifreleme anahtarı
- **Dek** -veri şifreleme anahtarı

Aşağıdaki kurallar geçerlidir:

- ADX depolama hesapları, AEK olarak bilinen her depolama hesabı için benzersiz bir şifreleme anahtarı oluşturur.

- AEK, diske yazılan her veri bloğunu şifrelemek için kullanılan anahtarlar olan DEKs 'leri türetmek için kullanılır.

- Key Vault ' de anahtarınızı yapılandırıp *küme* kaynağında buna başvurduğunuzda, Azure depolama, veri şifreleme ve şifre çözme işlemleri gerçekleştirmek için AEK 'i sarmalamak ve sarmalamak üzere Azure Key Vault istekleri gönderir.

- KEK, Key Vault hiçbir durumda kalmayacak ve HSM anahtarı durumunda donanımdan ayrılmayacaktır.

- Azure depolama, Azure Active Directory üzerinden Azure Key Vault kimlik doğrulaması yapmak ve erişmek için *küme* kaynağıyla ilişkili yönetilen kimliği kullanır.

## <a name="cmk-provisioning-procedure"></a>CMK sağlama prosedürü

1. Abonelik beyaz listesi--bu erken erişim özelliği için gereklidir
2. Azure Key Vault oluşturma ve anahtar depolama
3. *Küme* kaynağı oluşturma
4. Azure Izleyici veri deposu (ADX kümesi) sağlama
5. Key Vault izinler veriliyor
6. Log Analytics çalışma alanlarını ilişkilendirme

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

CMK özelliği, erken erişim özelliğidir. *Küme* kaynakları oluşturmayı planladığınız abonelikler, Azure ürün grubu tarafından önceden listelenmiş olmalıdır. Abonelik kimliklerinizi sağlamak için kişilerinizi Microsoft 'a kullanın.

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

*Küme* kaynağı oluştururken kapasite ayırma düzeyini (SKU) belirtmeniz gerekir. Kapasite ayırma düzeyi günde 1.000 ila 2.000 GB aralığında olabilir ve daha sonra 100 adımlarında güncelleştirebilirsiniz. Gün başına 2.000 GB 'den yüksek kapasite ayırma düzeyine ihtiyacınız varsa, bunu etkinleştirmek için Microsoft kişinize ulaşın. Bu özellik şu anda faturalandırmayı etkilemez; adanmış küme için fiyatlandırma modeli sunulduktan sonra faturalandırma, var olan CMK dağıtımları için geçerli olacaktır.

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
    "clusterType": "LogAnalytics",
    },
  "location": "<region-name>",
}
```
Kimlik, oluşturma zamanında *küme* kaynağına atanır.

**Yanıtıyla**

200 Tamam ve üst bilgi.
Özelliğin erken erişim süresi boyunca, ADX kümesi el ile sağlanır. Düşük bir ADX kümesinin tamamlanmasını sağlarken, sağlama durumunu iki şekilde kontrol edebilirsiniz:
1. Yanıttan Azure-AsyncOperation URL değerini kopyalayın ve [zaman uyumsuz işlemler durum denetimini](#asynchronous-operations-and-status-check)izleyin.
2. *Küme* KAYNAĞıNDA bir get isteği gönderin ve *provisioningstate* değerine bakın. Sağlama sırasında *Provisioningaccount* , tamamlandığında *başarılı oldu* .

### <a name="azure-monitor-data-store-adx-cluster-provisioning"></a>Azure Izleyici veri deposu (ADX kümesi) sağlama

Özelliğin erken erişim süresi boyunca, ADX kümesi, önceki adımlar tamamlandıktan sonra ürün ekibi tarafından el ile sağlanır. Bu adım için Microsoft kanalınızı kullanın ve *küme* kaynak yanıtını belirtin. 

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

> [!Warning]
> *Küme* kaynak güncelleştirmesinde *kimlik*, *SKU*, *keyvaultproperties* ve *Location*' ı içeren bir tam gövde sağlamanız gerekir. *Keyvaultproperties* ayrıntılarının eksik olması, anahtar tanımlayıcısını *küme* kaynağından kaldırır ve [anahtar iptalinin](#cmk-kek-revocation)oluşmasına neden olur.

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
     "capacity": 1000
     },
   "properties": {
     "KeyVaultProperties": {
       KeyVaultUri: "https://<key-vault-name>.vault.azure.net",
       KeyName: "<key-name>",
       KeyVersion: "<current-version>"
       },
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
    "KeyVaultProperties": {
      KeyVaultUri: "https://key-vault-name.vault.azure.net",
      KeyName: "key-name",
      KeyVersion: "current-version"
      },
    "provisioningState": "Succeeded",
    "clusterType": "LogAnalytics", 
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
> Bu adım yalnızca ADX küme sağlamasının ardından gerçekleştirilmelidir. Sağlama öncesinde çalışma alanlarını ve veri alma verilerini ilişkilendirirseniz, alınan veriler bırakılır ve kurtarılmayacaktır.

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
    "retentionInDays": days,
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

Anahtarınızı devre dışı bırakarak veya Key Vault *küme* kaynağı erişim ilkesini silerek verilerinize erişiminizi iptal edebilirsiniz. Azure Izleyici depolaması, her zaman bir saat içindeki anahtar izinlerinde yapılan değişikliklere göre her zaman uygulanır, normalde daha erken ve depolama kullanılamaz hale gelir. *Küme* kaynağınız ile ilişkili çalışma alanlarına alınan tüm veriler bırakılır ve sorgular başarısız olur. Daha önce alınan veriler, *küme* kaynağınız olduğunuz ve çalışma alanlarınızın silinmediği sürece Azure izleyici depolamada erişilemez durumda kalır. Erişilemeyen veriler veri bekletme ilkesine tabidir ve bekletmeye ulaşıldığında temizlenir.

Depolama, şifreleme anahtarını sarmalamak ve erişildikten sonra, veri alımı ve sorgu 30 dakika içinde devam ettirmeye çalışmak için Key Vault düzenli aralıklarla yoklar.

## <a name="cmk-kek-rotation"></a>CMK (KEK) döndürme

CMK 'nin dönmesi, *küme* kaynağının Azure Key Vault yeni anahtar sürümü ile açık güncelleştirilmesini gerektirir. Azure Izleyicisini yeni anahtar sürümünüzle güncelleştirmek için, " *küme* kaynağını anahtar tanımlayıcı ayrıntıları ile güncelleştirme" adımındaki yönergeleri izleyin. Anahtar sürümünüzü Key Vault güncelleştirir ve *küme* kaynağında yeni anahtar tanımlayıcısı ayrıntılarını güncelleştirmemeniz durumunda, Azure izleyici depolaması önceki anahtarınızı kullanmaya devam eder.
Tüm verileriniz, döndürmeden önce ve sonra gelen veriler de dahil olmak üzere anahtar döndürme işleminden sonra erişilebilir olur, çünkü tüm veriler artık yeni anahtar şifreleme anahtarı (KEK) sürümünüz tarafından şifrelenirken hesap şifreleme anahtarı (AEK) tarafından şifrelenir.

## <a name="limitations-and-constraints"></a>Sınırlamalar ve kısıtlamalar

- CMK özelliği, ADX küme düzeyinde desteklenir ve günde 1 GB veya daha fazla TB gönderme gereksinimi içeren adanmış bir Azure Izleyici ADX kümesi gerektirir.

- Abonelik başına en fazla *küme* kaynağı sayısı 2 ile sınırlıdır

- Çalışma alanına yönelik *küme* kaynağı ILIŞKILENDIRMESI, yalnızca ADX küme sağlama tamamlandığını doğruladıktan sonra yapılmalıdır. Hazırlama işlemi tamamlanmadan önce çalışma alanınıza gönderilen veriler bırakılır ve geri alınamaz.

- CMK şifrelemesi, CMK yapılandırmasından sonra yeni alınan veriler için geçerlidir. CMK yapılandırmasından önce alınan veriler, Microsoft anahtarıyla şifreli olarak kalır. CMK yapılandırmasından önce ve sonra gelen verileri sorunsuz bir şekilde sorgulayabilirsiniz.

- Söz konusu çalışma alanı için CMK 'nin gerekli olmadığını saptarken bir *küme* kaynağından bir çalışma alanı ilişkilendirebilirsiniz. Serbest ilişkilendirme işleminden sonra yeni alınan veriler, *küme* kaynağıyla ilişkilendirilmeden önce olduğundan, paylaşılan Log Analytics depolama alanında depolanır. *Küme* kaynağınızın sağlanması ve geçerli Key Vault anahtarıyla yapılandırılması durumunda, aynı şekilde, yeniden ilişkilendirmeden önce ve sonra gelen verileri sorgulayabilirsiniz.

- Azure Key Vault kurtarılabilir olarak yapılandırılmalıdır. Bu özellikler varsayılan olarak etkinleştirilmemiştir ve CLı veya PowerShell kullanılarak yapılandırılmalıdır:

  - [Geçici silme](https://docs.microsoft.com/azure/key-vault/key-vault-ovw-soft-delete) açık olmalıdır
  - Geçici silme işleminden sonra bile gizli/kasaların silinmesini zorlamak için [Temizleme koruması](https://docs.microsoft.com/azure/key-vault/key-vault-ovw-soft-delete#purge-protection) açılmalıdır.

- *Küme* kaynağı başka bir kaynak grubuna veya aboneliğe taşıma Şu anda desteklenmiyor.

- Azure Key Vault, *küme* kaynağı ve ilişkili çalışma alanlarınız aynı bölgede ve aynı Azure Active Directory (Azure AD) kiracısında olmalıdır, ancak farklı aboneliklerde olabilir.

- *Küme* kaynağıyla çalışma alanı ilişkilendirmesi, başka bir *küme* kaynağıyla ilişkiliyse başarısız olur


## <a name="troubleshooting-and-management"></a>Sorun giderme ve yönetim

- Key Vault kullanılabilirlik konuları
    - Normal işlemde--depolama, kısa süreler için AEK önbelleğe alınır ve düzenli aralıklarla sarmalaması için Key Vault geri gider.
    
    - Geçici bağlantı hataları--depolama, anahtarların kısa bir süre boyunca önbellekte kalmasına izin vererek geçici hataları (zaman aşımları, bağlantı hataları, DNS sorunları) işler ve bu da tüm küçük sinyalleri 'leri kullanılabilirliğinden fazla. Sorgu ve alma özellikleri kesinti olmadan devam eder.
    
    - Canlı site--yaklaşık 30 dakikalık bir işlem, depolama hesabının kullanılamaz hale gelmesine neden olur. Sorgu özelliği kullanılamaz ve veri kaybını önlemek için Microsoft anahtar kullanarak birkaç saat boyunca önbelleğe alınır. Key Vault erişimi geri yüklendiğinde, sorgu kullanılabilir hale gelir ve geçici önbelleğe alınmış veriler veri deposuna alınır ve CMK ile şifrelenir.

- Bir *küme* kaynağı oluşturur ve KeyVaultProperties 'i hemen belirtirseniz, sistem kimliği *küme* kaynağına atanana kadar erişim ilkesi tanımlanmadığından işlem başarısız olabilir.

- Mevcut *küme* kaynağını KeyVaultProperties ile güncelleştirirseniz ve ' Get ' anahtar erişim ilkesi Key Vault eksik olduğunda, işlem başarısız olur.

- Bir çalışma alanıyla ilişkili bir *küme* kaynağını silmeye çalışırsanız, silme işlemi başarısız olur.

- Bir *küme* kaynağı oluştururken çakışma hatası alırsanız, *küme* kaynağınızı son 14 gün içinde silmiş ve bu da geçici silme döneminde olabilir. *Küme* kaynak adı, geçici silme döneminde ayrılmış kalır ve bu adla yeni bir küme oluşturamazsınız. Ad, *küme* kaynağı kalıcı olarak silindiğinde geçici silme süresinden sonra serbest bırakılır.

- Bir işlem devam ederken *küme* kaynağınızı güncelleştirirseniz, işlem başarısız olur.

- Bir kaynak grubu için tüm *küme* kaynaklarını al:

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
              keyVaultUri: "https://key-vault-name.vault.azure.net",
              keyName: "key-name",
              keyVersion: "current-version"
              },
          "provisioningState": "Succeeded",
          "clusterType": "LogAnalytics", 
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

- Bir abonelik için tüm *küme* kaynaklarını al:

  ```rst
  GET https://management.azure.com/subscriptions/<subscription-id>/providers/Microsoft.OperationalInsights/clusters?api-version=2020-03-01-preview
  Authorization: Bearer <token>
  ```
    
  **Yanıtıyla**
    
  ' Kaynak grubu için*küme* kaynakları ' için aynı yanıt, ancak abonelik kapsamı.

- *Küme* kaynağında *Kapasite ayırmayı* Güncelleştir--ilişkili çalışma alanlarınızın veri hacmi değiştiğinde ve faturalandırma hususları için kapasite ayırma düzeyini güncelleştirmek istediğinizde, [ *küme* kaynağını güncelleştirin](#update-cluster-resource-with-key-identifier-details) ve yeni kapasite değerini sağlayın. Kapasite ayırma düzeyi 1.000 aralığında, gün başına 2.000 GB ve 100 adımlarında olabilir. Günde 2.000 GB 'den yüksek düzey için, Microsoft kişinize ulaşın.

- *Küme* kaynağınızı silme--bu işlemi gerçekleştirmek için *küme* kaynağında ' yazma ' izinlerine sahip olmanız gerekir. Silme işleminin yanlışlıkla mi yoksa bilerek mi olduğunu, verileri 14 gün içinde içeren *küme* kaynağınızın kurtarılmasına izin vermek için bir geçici silme işlemi yapılır. *Küme* kaynak adı, geçici silme döneminde ayrılmış kalır ve bu adla yeni bir küme oluşturamazsınız. Geçici silme süresinden sonra, *küme* kaynağı adı serbest bırakılır, *küme* kaynağınız ve verileriniz kalıcı olarak silinir ve kurtarılamaz. Herhangi bir ilişkili çalışma alanı silme işleminde *küme* kaynağı ile ilişkili olur. Yeni alınan veriler paylaşılan Log Analytics depolama alanında depolanır ve Microsoft anahtarıyla şifrelenir. Çalışma alanları ile ilişkili işlem zaman uyumsuzdur.

  ```rst
  DELETE https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters/<cluster-name>?api-version=2020-03-01-preview
  Authorization: Bearer <token>
  ```

  **Yanıtıyla**

  200 TAMAM

- *Küme* kaynağınızı ve verilerinizi kurtarma--son 14 gün Içinde silinen bir *küme* kaynağı, geçici silme durumunda ve kurtarılabilir. Bu, şu anda ürün grubu tarafından el ile gerçekleştirilir. Kurtarma isteklerinde Microsoft kanalınızı kullanın.

