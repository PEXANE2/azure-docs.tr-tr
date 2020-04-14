---
title: Azure Monitör müşteri tarafından yönetilen anahtar yapılandırması
description: Azure Key Vault anahtarını kullanarak Log Analytics çalışma alanlarınızdaki verileri şifrelemek için Müşteri Tarafından Yönetilen Anahtar (CMK) yapılandırmak için gereken bilgiler ve adımlar.
ms.subservice: logs
ms.topic: conceptual
author: yossi-y
ms.author: yossiy
ms.date: 04/12/2020
ms.openlocfilehash: dbd217c7135172c52a5ec7459930977960c452aa
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81260879"
---
# <a name="azure-monitor-customer-managed-key-configuration"></a>Azure Monitör müşteri tarafından yönetilen anahtar yapılandırması 

Bu makalede, Log Analytics çalışma alanlarınız ve Uygulama Öngörüleri bileşenleriniz için Müşteri Tarafından Yönetilen Anahtarları (CMK) yapılandırmak için arka plan bilgileri ve adımlar sağlar. Yapılandırıldıktan sonra, çalışma alanlarınıza veya bileşenlerinize gönderilen tüm veriler Azure Key Vault anahtarınızla şifrelenir.

Yapılandırmadan önce aşağıdaki [sınırlamaları ve kısıtlamaları](#limitations-and-constraints) gözden geçirmenizi öneririz.

## <a name="disclaimers"></a>Bildirimler

- Azure Monitor CMK, kayıtlı abonelikler için etkinleştirilen bir erken erişim özelliğidir.

- Bu makalede açıklanan CMK dağıtımı üretim kalitesinde teslim edilir ve erken erişim özelliği olmasına rağmen bu şekilde desteklenir.

- CMK özelliği, Azure Veri Gezgini (ADX) kümesi olan ve günde 1 TB veya daha fazla gönderen müşteriler için uygun olan özel bir veri deposu kümesinde teslim edilir. 

- CMK fiyatlandırma modeli şu anda kullanılamıyor ve bu makalede yer almıyor. Özel ADX kümesi için bir fiyatlandırma modeli takvim yılı (CY) 2020'nin ikinci çeyreğinde beklenmektedir ve varolan CMK dağıtımları için geçerli olacaktır.

- Bu makalede, Log Analytics çalışma alanları için CMK yapılandırması açıklanmaktadır. Uygulama Öngörüleri için CMK bileşenleri de bu makale kullanılarak desteklenirken, farklılıklar Ek'te listelenir.

> [!NOTE]
> Log Analytics ve Application Insights aynı veri depolama platformve sorgu motorlarını kullanıyor.
> Azure Monitor altında tek bir birleşik günlük mağazası oluşturmak için Application Insights'ın Log Analytics'e entegrasyonu yoluyla bu iki mağazayı bir araya getiriyoruz. Bu değişiklik, 2020 takvim yılının ikinci çeyreği için planlanmıştır. O zamana kadar Application Insights verileriniz için CMK dağıtmanız yoksa, bu tür dağıtımlar konsolidasyon tarafından kesintiye uğrayacağı ve Log Analytics çalışma alanına geçişten sonra CMK'yı yeniden yapılandırmanız gerekeceği için konsolidasyonun tamamlanmasını beklemenizi öneririz. Günde en az 1 TB küme düzeyinde geçerlidir ve ikinci üç aylık dönemde konsolidasyon tamamlanana kadar Application Insights ve Log Analytics ayrı kümeler gerektirir.

## <a name="customer-managed-key-cmk-overview"></a>Müşteri tarafından yönetilen anahtar (CMK) genel bakış

[Rest'te şifreleme,](https://docs.microsoft.com/azure/security/fundamentals/encryption-atrest) kuruluşlarda yaygın bir gizlilik ve güvenlik gereksinimidir. Şifreleme veya şifreleme anahtarlarını yakından yönetmek için çeşitli seçenekleriniz varken Azure'un Rest'te Şifreleme'yi tamamen yönetmesine izin verebilirsiniz.

Azure Monitor veri deposu, Azure Depolama'da depolanırken Azure tarafından yönetilen anahtarları kullanarak istirahatte şifrelenen tüm verilerin olmasını sağlar. Azure Monitor ayrıca, sistem tarafından atanan [yönetilen kimlik](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) doğrulaması kullanılarak erişilen [Azure Key Vault'unuzda](https://docs.microsoft.com/azure/key-vault/key-vault-overview)depolanan kendi anahtarınızı kullanarak veri şifreleme seçeneği de sunar. Bu anahtar [yazılım veya donanım-HSM korumalı](https://docs.microsoft.com/azure/key-vault/key-vault-overview)olabilir.
Azure Monitor şifreleme kullanımı, Azure [Depolama şifrelemesinin](https://docs.microsoft.com/azure/storage/common/storage-service-encryption#about-azure-storage-encryption) çalışma şekliyle aynıdır.

Azure Monitor Depolama'nın kaydırma ve açma işlemleri için Key Vault'a erişme sıklığı 6 ila 60 saniye arasındadır.Azure Monitör Depolama, bir saat içinde anahtar izinlerindeki değişikliklere her zaman saygı duyar.

Son 14 gün içinde alınan veriler, verimli sorgu motoru çalışması için sıcak önbellekte (SSD destekli) de tutulur. Bu veriler CMK yapılandırması ne olursa olsun Microsoft anahtarları ile şifrelenmiş kalır, ancak 2020 yılının ilk yarısında SSD CMK ile şifrelenmiş olması için çalışıyoruz.

## <a name="how-cmk-works-in-azure-monitor"></a>CMK Azure Monitör'de nasıl çalışır?

Azure Monitor, Azure Anahtar Kasanıza erişim sağlamak için sistem tarafından atanmış yönetilen kimliklerden yararlanır.Sistem tarafından atanan yönetilen kimlik yalnızca tek bir Azure kaynağıyla ilişkilendirilebilir. Azure Monitor veri deposunun (ADX kümesi) kimliği küme düzeyinde desteklenir ve bu, CMK özelliğinin özel bir ADX kümesinde teslim edilmesini belirler. CMK'yı birden çok çalışma alanlarında desteklemek için, yeni bir Log Analytics kaynağı *(Cluster)* Key Vault'unuz la Log Analytics çalışma alanlarınız arasında ara kimlik bağlantısı olarak gerçekleştirir. Bu kavram Sistem tarafından atanan kimlik kısıtlamasına uygundur ve kimlik ADX kümesi ile Log Analytics *Cluster* kaynağı arasında korunurken, ilişkili tüm çalışma alanlarının verileri Key Vault anahtarınızla korunur. Alttaki ADX küme depolama alanı,\'Azure Etkin Dizini aracılığıyla Azure Anahtar Kasanızın kimliğini doğrulamak ve erişmek için *Küme* kaynağıyla ilişkili yönetilen kimliği kullanır.

![CMK Genel Bakış](media/customer-managed-keys/cmk-overview-8bit.png)
1.    Müşterinin Anahtar Kasası.
2.    Key Vault izinleri ile yönetilen kimliğe sahip müşterinin Log Analytics *Cluster* kaynağı – Kimlik veri deposu (ADX kümesi) düzeyinde desteklenir.
3.    Azure Monitor özel ADX kümesi.
4.    CMK şifrelemesi için *Cluster* kaynağıyla ilişkili müşterinin çalışma alanları.

## <a name="encryption-keys-management"></a>Şifreleme anahtarları yönetimi

Depolama veri şifrelemesinde yer alan 3 tür anahtar vardır:

- **KEK** - Anahtar Şifreleme Anahtarı (CMK)
- **AEK** - Hesap Şifreleme Anahtarı
- **DEK** - Veri Şifreleme Anahtarı

Aşağıdaki kurallar geçerlidir:

- ADX depolama hesapları, AEK olarak bilinen her Depolama hesabı için benzersiz şifreleme anahtarı oluşturur.

- AEK, diske yazılan her veri bloğunu şifrelemek için kullanılan anahtarlar olan DEK'leri türetmek için kullanılır.

- Anahtarınızı Key Vault'ta yapılandırıp *Küme* kaynağında referans aldığınızda, Azure Depolama, veri şifreleme ve şifre çözme işlemleri gerçekleştirmek için AEK'yi sarmak ve açmak için Azure Key Vault'unuza istekler gönderir.

- KEK'iniz Key Vault'unuzu asla terk eder ve HSM anahtarı durumunda donanımdan asla ayrılmaz.

- Azure Depolama, Azure Etkin Dizini aracılığıyla Azure Anahtar Kasası'nın kimliğini doğrulamak ve erişim için *Küme* kaynağıyla ilişkili yönetilen kimliği kullanır.

## <a name="cmk-provisioning-procedure"></a>CMK sağlama prosedürü

Uygulama Öngörüleri CMK yapılandırması için, 3 ve 6.

1. Abonelik beyaz listesi -- bu erken erişim özelliği için gereklidir
2. Azure Anahtar Kasası oluşturma ve depolama anahtarı
3. *Küme* kaynağı oluşturma
4. Azure Monitor veri deposu (ADX kümesi) sağlama
5. Anahtar Kasanıza izin verme
6. Log Analytics çalışma alanlarını ilişkilendirme

Yordam şu anda UI'de desteklenmez ve sağlama işlemi REST API üzerinden gerçekleştirilir.

> [!IMPORTANT]
> Herhangi bir API isteği, istek üstbilgisine bir Taşıyıcı yetkilendirme belirteci içermelidir.

Örneğin:

```rst
GET https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/Microsoft.OperationalInsights/workspaces/<workspace-name>?api-version=2020-03-01-preview
Authorization: Bearer eyJ0eXAiO....
```

Nerede *eyJ0eXAiO ....* tam Yetki belirteci temsil eder. 

Aşağıdaki yöntemlerden birini kullanarak belirteci edinebilirsiniz:

1. [Uygulama kayıtları](https://docs.microsoft.com/graph/auth/auth-concepts#access-tokens) yöntemini kullanın.
2. Azure portalında
    1. "Geliştirici aracı" (F12) varken Azure portalına gidin
    1. "Batch?api-version" örneklerinden birinde "İstek Başlıkları" altında yetkilendirme dizesini arayın. Bu gibi görünüyor: "yetkilendirme: Taşıyıcı eyJ0eXAiO ....". 
    1. Aşağıdaki örneklere göre API çağrınıza kopyalayın ve ekleyin.
3. Azure REST dokümantasyon sitesine gidin. Herhangi bir API'de "Deneyin" tuşuna basın ve Taşıyıcı belirteci'ni kopyalayın.

### <a name="asynchronous-operations-and-status-check"></a>Eşzamanlı işlemler ve durum kontrolü

Bu yapılandırma yordamındaki bazı işlemler, hızlı bir şekilde tamamlanamadığından eş senkronize olarak çalıştırın. Eşzamanlı işlem yanıtı, kabul edildiğinde başlangıçta bir HTTP durum kodu 200 (Tamam) ve *Azure-AsyncOperation* özelliğine sahip üstbilgi döndürür:
```json
"Azure-AsyncOperation": "https://management.azure.com/subscriptions/subscription-id/providers/Microsoft.OperationalInsights/locations/region-name/operationStatuses/operation-id?api-version=2020-03-01-preview"
```

*Azure-AsyncOperation* üstbilgi değerine GET isteği göndererek eşzamanlı işlemin durumunu kontrol edebilirsiniz:
```rst
GET https://management.azure.com/subscriptions/subscription-id/providers/microsoft.operationalInsights/locations/region-name/operationstatuses/operation-id?api-version=2020-03-01-preview
Authorization: Bearer <token>
```

Yanıt, işlem ve *Durumu*hakkında bilgi içerir. Aşağıdakilerden biri olabilir:

Operasyon devam ediyor
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

CMK özelliği erken erişim özelliğidir. *Küme* kaynakları oluşturmayı planladığınız aboneliklerin Azure ürün grubu tarafından önceden beyaz listeye alınması gerekir. Abonelik lerinizdeki kişileri sağlamak için Microsoft'ta kişilerinizi kullanın.

> [!IMPORTANT]
> CMK yeteneği bölgeseldir. Azure Key Vault, *Cluster* kaynağınız ve ilişkili Log Analytics çalışma alanlarınınız aynı bölgede olmalıdır, ancak farklı aboneliklerde olabilir.

### <a name="storing-encryption-key-kek"></a>Şifreleme anahtarının saklanması (KEK)

Zaten oluşturmanız gereken bir Azure Anahtar Kasası oluşturun veya kullanın veya veri şifreleme için kullanılacak bir anahtar içe aktarın. Azure Anahtar Kasası, anahtarınızı ve verilerinize Erişimi Azure Monitor'da korumak için kurtarılabilir olarak yapılandırılmalıdır. Bu yapılandırmayı Key Vault'unuzdaki özelliklerin altında doğrulayabilirsiniz, hem *Yumuşak silme* hem de *Temizleme koruması* etkinleştirilmelidir.

![Yumuşak silme ve temizleme koruma ayarları](media/customer-managed-keys/soft-purge-protection.png)

Bu ayarlar CLI ve PowerShell üzerinden kullanılabilir:
- [Geçici Silme](https://docs.microsoft.com/azure/key-vault/key-vault-ovw-soft-delete)
- Yumuşak silme sonra bile gizli / tonoz kuvvet silme karşı koruma korumaları [temizleme](https://docs.microsoft.com/azure/key-vault/key-vault-ovw-soft-delete#purge-protection)

### <a name="create-cluster-resource"></a>*Küme* kaynağı oluşturma

Bu kaynak, Key Vault'unuz la Log Analytics çalışma alanlarınız arasında ara kimlik bağlantısı olarak kullanılır. Aboneliklerinizin beyaz listeye alındığına dair onay aldıktan sonra, çalışma alanlarınızın bulunduğu bölgede bir Log Analytics *Cluster* kaynağı oluşturun. Uygulama Öngörüleri ve Günlük Analizi ayrı *Küme* kaynakları türleri gerektirir. *Cluster* kaynağının türü, *clusterType* özelliğini *LogAnalytics*veya *ApplicationInsights*olarak ayarlayarak oluşturma zamanında tanımlanır. Küme kaynak türü sonra değiştirilemez.

Uygulama Öngörüleri CMK yapılandırması için Ek içeriği takip edin.

*Küme* kaynağı oluştururken kapasite rezervasyon düzeyini (sku) belirtmeniz gerekir. Kapasite rezervasyon seviyesi günde 1.000 ila 2.000 GB aralığında olabilir ve daha sonra 100 adımda güncelleyebilirsiniz. Günlük 2.000 GB'dan yüksek kapasite rezervasyon düzeyine ihtiyacınız varsa, bunu etkinleştirmek için Microsoft ilgili kişinize ulaşın. Bu özellik şu anda faturalandırmayı etkilemez - özel küme için fiyatlandırma modeli tanıtıldıktan sonra, faturalandırma varolan CMK dağıtımları için geçerli olacaktır.

**Oluştur**

Bu Kaynak Yöneticisi isteği eşzamanlı işlemdir.

```rst
PUT https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters/<cluster-name>?api-version=2019-08-01-preview
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
Kimlik oluşturma zamanında *Küme* kaynağına atanır.

**Yanıt**

200 Tamam ve üstbilgi.
Özelliğin erken erişim döneminde, ADX kümesi el ile birlikte verilir. AdX kümesinin sağlanmasının tamamlanması bir süre olsa da, sağlama durumunu iki şekilde denetleyebilirsiniz:
1. Yanıttan Azure-AsyncOperation URL değerini kopyalayın ve [eşzamanlı işlemler durum denetimini](#asynchronous-operations-and-status-check)izleyin.
2. *Küme* kaynağına GET isteği gönderin ve *provisioningState* değerine bakın. Bu hükmünde alma ve tamamlandığında *Başarılı* iken *Provisioning Hesabıdır.*

### <a name="azure-monitor-data-store-adx-cluster-provisioning"></a>Azure Monitor veri deposu (ADX kümesi) sağlama

Özelliğin erken erişim döneminde, önceki adımlar tamamlandıktan sonra ADX kümesi ürün ekibi tarafından el ile birlikte verilir. Bu adım için Microsoft kanalınızı kullanın ve *Cluster* kaynak yanıtını sağlayın. 

```rst
GET https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters/<cluster-name>?api-version=2019-08-01-preview
Authorization: Bearer <token>
```

> [!IMPORTANT]
> Sonraki adımlardaki ayrıntılara ihtiyacınız olacağından yanıtı kopyalayın ve kaydedin.

**Yanıt**

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

"PrincipalId" GUID Küme *kaynağı* için yönetilen kimlik hizmeti tarafından oluşturulur.

### <a name="grant-key-vault-permissions"></a>Anahtar Kasa sı izinlerini ver

Anahtar Kasanızı *Küme* kaynağınıza izin veren yeni bir erişim ilkesiyle güncelleştirin. Bu izinler, veri şifreleme için alt yapı Azure Monitor Depolama tarafından kullanılır. Azure portalında Key Vault'unuzu açın ve bu ayarlara sahip bir ilke oluşturmak için "Erişim İlkeleri"ni ve ardından "+ Erişim İlkesi Ekle"yi tıklayın:

- Anahtar izinleri: 'Al', 'Anahtarı Sart' ve 'Anahtarı Aç' izinlerini seçin.
- Anapara seçin: önceki adımda yanıt döndürülen asıl kimlik değerini girin.

![Anahtar Kasa izinleri vermek](media/customer-managed-keys/grant-key-vault-permissions-8bit.png)

Anahtar Kasanızın anahtarınızı ve Azure Monitör verilerinize erişimi korumak için kurtarılabilir olarak yapılandırıldığını doğrulamak için *Alma* izni gereklidir.

### <a name="update-cluster-resource-with-key-identifier-details"></a>Anahtar tanımlayıcı ayrıntılarıyla Cluster kaynağını güncelleştirme

Bu adım, Key Vault'unuzda ki ilk ve gelecekteki anahtar sürüm güncelleştirmeleri sırasında gerçekleştirilir. Veri şifrelemesinde kullanılacak anahtar sürüm hakkında Azure Monitor Depolama'ya bilgi verir. Güncelleştirildiğinde, yeni anahtarınız Depolama anahtarına (AEK) sarıp açmak için kullanılır.

*Küme* kaynağını Key Vault *Key tanımlayıcı* ayrıntılarınızla güncellemek için, Anahtar tanımlayıcı ayrıntılarını almak için Azure Key Vault'ta anahtarınızın geçerli sürümünü seçin.

![Anahtar Kasa sı izinlerini ver](media/customer-managed-keys/key-identifier-8bit.png)

Anahtar Tanımlayıcı ayrıntılarıyla *Cluster* kaynak KeyVaultProperties'i güncelleştirin.

**Güncelleştir**

Bu Kaynak Yöneticisi isteği eşzamanlı işlemdir.

> [!Warning]
> *Kimlik,* *sku*, *KeyVaultProperties* ve *konum*içeren *Küme* kaynak güncelleştirmesinde tam bir gövde sağlamanız gerekir. *KeyVaultProperties* ayrıntılarının eksik kümesi *kaynaktan* anahtar tanımlayıcısı kaldırılır ve [anahtar iptaline](#cmk-kek-revocation)neden olur.

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

**Yanıt**

200 Tamam ve üstbilgi.
Anahtar tanımlayıcısının yayılmasının tamamlanması birkaç dakika sürer. Sağlama durumunu iki şekilde denetleyebilirsiniz:
1. Yanıttan Azure-AsyncOperation URL değerini kopyalayın ve [eşzamanlı işlemler durum denetimini](#asynchronous-operations-and-status-check)izleyin.
2. *Küme* kaynağına GET isteği gönderin ve *KeyVault Properties* özelliklerine bakın. En son güncellenen Anahtar tanımlayıcı ayrıntılarınız yanıt olarak geri dönmelidir.

Anahtar tanımlayıcı güncelleştirmesi tamamlandığında *Küme* kaynağındaki GET isteğine verilen yanıt aşağıdaki gibi görünmelidir:

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

### <a name="workspace-association-to-cluster-resource"></a>*Kaynak Kümeleme* için çalışma alanı ilişkilendirme
Uygulama Öngörüleri CMK yapılandırması için bu adım için Ek içeriği izleyin.

Bu işlemi gerçekleştirmek için hem çalışma alanınız hem de *Küme* kaynağınız için 'yazma' izinleriniz olması gerekir:

- Çalışma alanında: Microsoft.OperationalInsights/workspaces/write
- *Küme* kaynağında: Microsoft.OperationalInsights/clusters/write

> [!IMPORTANT]
> Bu adım yalnızca ADX küme sağlama dan sonra gerçekleştirilmelidir. Çalışma alanlarını ilişkilendirirseniz ve verileri sağlamadan önce sindirirseniz, yutulan veriler bırakılır ve kurtarılamaz.

**Çalışma alanını ilişkilendirme**

Bu Kaynak Yöneticisi isteği eşzamanlı işlemdir.

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

**Yanıt**

200 Tamam ve üstbilgi.
Yutulan veriler, ilişkilendirme işleminden sonra yönetilen anahtarınızla şifrelenir ve işlemin tamamlanması 90 dakika kadar sürebilir. Çalışma alanı ilişkilendirme durumunu iki şekilde denetleyebilirsiniz:
1. Yanıttan Azure-AsyncOperation URL değerini kopyalayın ve [eşzamanlı işlemler durum denetimini](#asynchronous-operations-and-status-check)izleyin.
2. Çalışma [Alanları](https://docs.microsoft.com/rest/api/loganalytics/workspaces/get) Gönder – İstek alın ve yanıtı gözlemleyin, ilişkili çalışma alanı "özellikler" altında bir clusterResourceId'ye sahip olacaktır.

```rest
GET https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalInsights/workspaces/<workspace-name>?api-version=2020-03-01-preview
```

**Yanıt**

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

Anahtarınızı devre dışı bırakarak veya Key Vault'unuzda *Küme* kaynak erişim ilkesini silerek verilerinize erişiminizi iptal edebilirsiniz. Azure Monitör Depolama, normalde bir saat içinde anahtar izinlerindeki değişikliklere her zaman saygı duyar ve Depolama kullanılamaz hale gelir. *Küme* kaynağınızla ilişkili çalışma alanlarına alınan tüm veriler bırakılır ve sorgular başarısız olur. *Küme* kaynağınız olduğunuz ve çalışma alanlarınız silinmediğiniz sürece, daha önce yutulan veriler Azure MonitörÜ Depolama'da erişilemez. Erişilemeyen veriler veri saklama ilkesi tarafından yönetilir ve bekletme ulaşıldığında tasfiye edilecektir.

Depolama, şifreleme anahtarını açmak için Anahtar Kasanızı düzenli aralıklarla yoklar ve erişildikten sonra veri alımı ve sorgu 30 dakika içinde devam edecektir.

## <a name="cmk-kek-rotation"></a>CMK (KEK) rotasyonu

CMK'nın döndürülmesi, Azure Anahtar Kasası'ndaki yeni anahtar sürümüyle *Cluster* kaynağının açık bir şekilde güncelleştirin. Azure Monitor'u yeni anahtar sürümünüzle güncellemek için *"Küme* kaynağını Anahtar tanımlayıcı ayrıntılarıyla güncelleştir" adımındaki yönergeleri izleyin. Anahtar Vault'ta anahtar sürümünüzü günceller ve *Küme* kaynağındaki yeni Anahtar tanımlayıcı ayrıntılarını güncellemezseniz, Azure MonitörÜ Depolama önceki anahtarınızı kullanmaya devam eder.
AEK artık yeni Anahtar Şifreleme Anahtarı (KEK) sürümünüz tarafından şifrelenirken tüm veriler hesap şifreleme anahtarı (AEK) tarafından şifrelenmiş olduğundan, tüm veriler döndürme den önce ve sonrasında alınan veriler de dahil olmak üzere anahtar döndürme işleminden sonra erişilebilir.

## <a name="limitations-and-constraints"></a>Sınırlamalar ve kısıtlamalar

- CMK özelliği ADX küme düzeyinde desteklenir ve günde 1 TB veya daha fazla gönderme gereksinimi olan özel bir Azure Monitor ADX kümesi gerektirir.

- Abonelik başına maksimum *Küme* kaynağı sayısı 2 ile sınırlıdır

- *AdX* küme sağlamanın yerine getirildiğini doğruladıktan sonra yalnızca çalışma alanına küme kaynak ilişkilendirmesi yapılmalıdır. Bu sağlamadan önce gönderilen veriler bırakılır ve kurtarılamaz.

- CMK şifreleme, CMK yapılandırması sonrasında yeni alınan veriler için geçerlidir. CMK yapılandırması öncesinde alınan veriler Microsoft anahtarıyla şifrelenmiş olarak kalır. CMK yapılandırması öncesinde ve sonrasında alınan verileri sorunsuz bir şekilde sorgulayabilirsiniz.

- Çalışma alanı bir *Küme* kaynağıyla ilişkilendirildikten sonra, veriler anahtarınızla şifrelendiğinden ve Azure Key Vault'ta KEK'iniz olmadan erişilebilir olmadığından, *Küme* kaynağından ilişkilendirilemez.

- Azure Anahtar Kasası kurtarılabilir olarak yapılandırılmalıdır. Bu özellikler varsayılan olarak etkinleştirilmez ve CLI ve PowerShell kullanılarak yapılandırılmalıdır:

  - [Yumuşak Silme](https://docs.microsoft.com/azure/key-vault/key-vault-ovw-soft-delete) açık olmalıdır
  - [Temizleme koruması,](https://docs.microsoft.com/azure/key-vault/key-vault-ovw-soft-delete#purge-protection) yumuşak silme işleminden sonra bile gizli / kasanın zorla silinmesine karşı korunmak için açılmalıdır

- Uygulama Öngörüleri ve Günlük Analizi ayrı *Küme* kaynakları gerektirir. *Küme* kaynağının türü, "clusterType" özelliğini "LogAnalytics" veya 'ApplicationInsights' olarak ayarlayarak oluşturma zamanında tanımlanır. *Küme* kaynak türü değiştirilemez.

- *Kaynak kümesi* başka bir kaynak grubuna taşınması veya abonelik şu anda desteklenmiyor.

- Azure Anahtar Kasası, *Küme* kaynağınız ve ilişkili çalışma alanlarınız aynı bölgede ve aynı Azure Etkin Dizin (Azure AD) kiracısında olmalıdır, ancak farklı aboneliklerde olabilir.

- Başka bir *Küme* kaynağıyla ilişkiliyse, Cluster *Cluster* kaynağına çalışma alanı ilişkilendirme

## <a name="troubleshooting-and-management"></a>Sorun giderme ve yönetimi

- Anahtar Vault durumu
    - Normal çalışmada -- Depolama AEK'yi kısa süreler için önbelleğe alıyor ve periyodik olarak açmak için Key Vault'a geri dönüyor.
    
    - Geçici bağlantı hataları -- Depolama, anahtarların kısa bir süre daha önbellekte kalmasına izin vererek geçici hataları (zaman aşımları, bağlantı hataları, DNS sorunları) işler ve bu da kullanılabilirlikteki küçük hataların üstesinden gelebilir. Sorgu ve alma yetenekleri kesintisiz olarak devam eder.
    
    - Canlı site -- yaklaşık 30 dakikanın kullanılamaması Depolama hesabının kullanılamamasına neden olur. Sorgu özelliği kullanılamıyor ve yutulan veriler veri kaybını önlemek için Microsoft anahtarı kullanılarak birkaç saat önbelleğe alındı. Key Vault'a erişim geri yüklendiğinde, sorgu kullanılabilir hale gelir ve geçici önbelleğe alınan veriler veri deposuna yutularak CMK ile şifrelenir.

- Bir *Küme* kaynağı oluşturur ve KeyVaultProperties'i hemen belirtirseniz, sistem kimliği *Küme* kaynağına atanana kadar erişim ilkesi tanımlanamayacağından işlem başarısız olabilir.

- KeyVaultProperties ile varolan *Cluster* kaynağını güncellerseniz ve Key Vault'ta 'Get' tuşu Erişim İlkesi eksikse, işlem başarısız olur.

- Çalışma alanıyla ilişkili bir *Küme* kaynağını silmeye çalışırsanız, silme işlemi başarısız olur.

- Bir kaynak grubu için tüm *Küme* kaynaklarını alın:

  ```rst
  GET https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters?api-version=2020-03-01-preview
  Authorization: Bearer <token>
  ```
    
  **Yanıt**
  
  ```json
  {
    "value": [
      {
        "identity": {
          "type": "SystemAssigned",
          "tenantId": "tenant-id",
          "principalId": "principal-Id"
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
        "id": "/subscriptions/subscription-id/resourcegroups/resource-group-name/providers/microsoft.operationalinsights/workspaces/workspace-name",
        "name": "cluster-name",
        "type": "Microsoft.OperationalInsights/clusters",
        "location": "region-name"
      }
    ]
  }
  ```

- Abonelik için tüm *Cluster* kaynaklarını alın:

  ```rst
  GET https://management.azure.com/subscriptions/<subscription-id>/providers/Microsoft.OperationalInsights/clusters?api-version=2020-03-01-preview
  Authorization: Bearer <token>
  ```
    
  **Yanıt**
    
  'Kaynak grubu için*küme* kaynakları' ile aynı yanıt, ancak abonelik kapsamında.
    
- *Küme* kaynağınızı silin -- Silme işlemi yanlışlıkla veya kasıtlı olsun, Küme kaynağınızın, verilerinizin ve ilişkili çalışma alanlarının 14 gün içinde kurtarılmasına izin vermek için bir yumuşak silme işlemi gerçekleştirilir. *Küme* kaynak adı yumuşak silme döneminde ayrılmış kalır ve bu ada sahip yeni bir küme oluşturamazsınız. Yumuşak silme döneminden sonra *Cluster* kaynağınız ve verileriniz kurtarılamaz. İlişkili çalışma alanları *Küme* kaynağından çıkarılmaz ve yeni veriler paylaşılan Depolama'ya alınır ve Microsoft anahtarıyla şifrelenir.

  ```rst
  DELETE https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters/<cluster-name>?api-version=2020-03-01-preview
  Authorization: Bearer <token>
  ```

  **Yanıt**

  200 TAMAM

- *Küme* kaynağınızı ve verilerinizi kurtarın - yumuşak silme döneminde, aynı ada ve aynı abonelikte, kaynak grubunda ve bölgede bir *Küme* kaynağı oluşturun. *Küme* kaynağınızı kurtarmak için ** *Küme* Oluştur kaynak** adımını izleyin.


## <a name="appendix"></a>Ek

Uygulama Öngörüleri Müşteri Yönetimi Anahtarı (CMK) de desteklenir, ancak Uygulama Öngörübileşenleriniz için CMK dağıtımını planlamanıza yardımcı olmak için aşağıdaki değişikliği göz önünde bulundurmanız gerekir.

Log Analytics ve Application Insights aynı veri depolama platformve sorgu motorlarını kullanıyor. Bu iki mağazayı, Uygulama Öngörüleri'nin Log Analytics'e entegre edilerek Azure Monitor altında tek bir birleşik günlük mağazası sağlamasını sağlıyoruz.
2020. Bu değişiklik, Uygulama Öngörü verilerinizi Log Analytics çalışma alanlarına getirir ve sorguları, öngörüleri ve diğer iyileştirmeleri mümkün kılarken, CMK'nın çalışma alanınızdaki yapılandırması Da Apply Insights verileriniz için de geçerli olacaktır.

> [!NOTE]
> Tümleştirmeden önce Application Insight verileriniz için CMK dağıtmanız yoksa, bu tür dağıtımlar tümleştirme tarafından kesintiye uğrayacağı ve Log Analytics çalışma alanına geçişten sonra CMK'yı yeniden yapılandırmanız gerekeceği için Application Insights CMK ile beklemenizi öneririz. Günde en az 1 TB küme düzeyinde geçerlidir ve ikinci üç aylık dönemde konsolidasyon tamamlanana kadar Application Insights ve Log Analytics ayrı kümeler gerektirir.

## <a name="application-insights-cmk-configuration"></a>Uygulama Öngörüleri CMK yapılandırması

Application Insights CMK yapılandırması, aşağıdaki adımlar dışında kısıtlamalar ve sorun giderme de dahil olmak üzere bu makalede gösterilen işlemle aynıdır:

- *Küme* kaynağı oluşturma
- Bileşeni *küme* kaynağıyla ilişkilendirme

Uygulama Öngörüleri için CMK'yı yapılandırırken, yukarıda listelenenler yerine bu adımları kullanın.

### <a name="create-a-cluster-resource"></a>*Küme* kaynağı oluşturma

Bu kaynak, Anahtar Kasanız ve bileşenleriniz arasında ara kimlik bağlantısı olarak kullanılır. Aboneliklerinizin beyaz listeye alındığına dair bir onay aldıktan sonra, bileşenlerinizin bulunduğu bölgede bir Log Analytics *Cluster* kaynağı oluşturun. *Cluster* kaynağının türü, *clusterType* özelliğini *LogAnalytics*veya *ApplicationInsights*olarak ayarlayarak oluşturma zamanında tanımlanır. Bu Uygulama Insights CMK için *ApplicationInsights* olmalıdır. *ClusterType* ayarı yapılandırmadan sonra değiştirilemez.

**Oluştur**

Bu Kaynak Yöneticisi isteği eşzamanlı işlemdir.

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
    "clusterType":"ApplicationInsights"
    },
  "location": "<region-name>"
}
```

**Yanıt**

200 Tamam ve üstbilgi.
Özelliğin erken erişim döneminde, ADX kümesi el ile birlikte verilir. AdX kümesinin sağlanmasının tamamlanması bir süre olsa da, sağlama durumunu iki şekilde denetleyebilirsiniz:
1. Yanıttan Azure-AsyncOperation URL değerini kopyalayın ve [eşzamanlı işlemler durum denetimini](#asynchronous-operations-and-status-check)izleyin.
2. *Küme* kaynağına GET isteği gönderin ve *provisioningState* değerine bakın. Bu hükmünde alma ve tamamlandığında *Başarılı* iken *Provisioning Hesabıdır.*

### <a name="associate-a-component-to-a-cluster-resource-using-components---create-or-update-api"></a>Bileşenleri kullanarak bileşeni *küme* kaynağıyla ilişkilendirme - API [Oluştur veya Güncelleştir](https://docs.microsoft.com/rest/api/application-insights/components/createorupdate)

Bu işlemi gerçekleştirmek için hem bileşeninizde hem de *Küme* kaynağınızda bu eylemleri içeren 'yazma' izinleriniz olması gerekir:

- Bileşen olarak: Microsoft.Insights/component/write
- *Küme* kaynağında: Microsoft.OperationalInsights/clusters/write

> [!IMPORTANT]
> Bu adım yalnızca ADX küme sağlama dan sonra gerçekleştirilmelidir. Bileşenleri ilişkilendirir ve verileri sağlamadan önce sindirirseniz, yutulan veriler bırakılır ve kurtarılamaz.
> ADX kümesinin sağlanmış olduğunu doğrulamak için Cluster *kaynağını* execute REST API'sını alın ve *provisioningState* değerinin *Başarılı*olup olmadığını denetleyin.

```rst
GET https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters/<cluster-name>?api-version=2020-03-01-preview
Authorization: Bearer <token>
```

**Yanıt**
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
    "KeyVaultProperties": {
      KeyVaultUri: "https://key-vault-name.vault.azure.net",
      KeyName: "key-name",
      KeyVersion: "current-version"
      },
    "provisioningState": "Succeeded",
    "clusterType": "ApplicationInsights", 
    "clusterId": "cluster-id"
    },
  "id": "/subscriptions/subscription-id/resourceGroups/resource-group-name/providers/Microsoft.OperationalInsights/clusters/cluster-name",
  "name": "cluster-name",
  "type": "Microsoft.OperationalInsights/clusters",
  "location": "region-name"
  }
```

> [!IMPORTANT]
> Sonraki adımlarda ihtiyacınız olacağıiçin yanıtı kopyalayın ve saklayın.

**Bir bileşeni ilişkilendirme**

```rst
PUT https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Insights/components/<component-name>?api-version=2015-05-01
Authorization: Bearer <token>
Content-type: application/json

{
  "properties": {
    "clusterDefinitionId": "cluster-id"
  },
  "location": "<region-name>",
  "kind": "<component-type>"
}
```
"clusterDefinitionId", önceki adımdan verilen yanıtta sağlanan "clusterId" değeridir.
"tür" örnek "web"dir.

**Yanıt**

```json
{
  "id": "/subscriptions/subscription-id/resourcegroups/resource-group-name/providers/microsoft.insights/components/component-name",
  "name": "component-name",
  "type": "Microsoft.Insights/components",
  "location": "region-name",
  "tags": "",
  "kind": "",
  "properties": {
    "clusterDefinitionId": "cluster-id"
    "ApplicationId": "",
    "AppId": "",
    "Application_Type": "",
    "Flow_Type": "",
    "Request_Source": "",
    "InstrumentationKey": "",
    "CreationDate": "",
    "TenantId": "",
    "HockeyAppId": "",
    "HockeyAppToken": "",
    "provisioningState": "",
    "SamplingPercentage":,
    "RetentionInDays":,
    "ConnectionString": "",
    "DisableIpMasking":,
    "ImmediatePurgeDataOn30Days": 
  }
}
```
"clusterDefinitionId", bu bileşenle ilişkili *Küme* kaynak kimliğidir.

İlişkilendirmeden sonra bileşenlerinize gönderilen veriler yönetilen anahtarınızla şifrelenmiş olarak depolanır.
