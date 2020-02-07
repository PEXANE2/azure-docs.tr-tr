---
title: Azure Izleyici müşteri tarafından yönetilen anahtar yapılandırması
description: Azure Key Vault bir anahtar kullanarak Log Analytics çalışma alanlarınızdaki verileri şifrelemek üzere müşteri tarafından yönetilen anahtar (CMK) yapılandırma hakkında bilgi ve adımlar.
ms.service: azure-monitor
ms.subservice: logs
ms.topic: conceptual
author: yossi-y
ms.author: yossiy
ms.date: 02/05/2020
ms.openlocfilehash: eff751465c7b64429968b0305e6ad483943c374b
ms.sourcegitcommit: 57669c5ae1abdb6bac3b1e816ea822e3dbf5b3e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/06/2020
ms.locfileid: "77048180"
---
# <a name="azure-monitor-customer-managed-key-configuration"></a>Azure Izleyici müşteri tarafından yönetilen anahtar yapılandırması 

Bu makale, Log Analytics çalışma alanlarınız ve Application Insights bileşenleriniz için müşteri tarafından yönetilen anahtarları (CMK) yapılandırmaya yönelik arka plan bilgileri ve adımları sağlar. Yapılandırıldıktan sonra, çalışma alanlarınızı veya bileşenlerinizi gönderilen tüm veriler Azure Key Vault anahtarınızla şifrelenir.

Yapılandırmadan önce aşağıdaki [sınırlamaları ve kısıtlamaları](#limitations-and-constraints) incelemenizi öneririz.

## <a name="disclaimers"></a>Bildirimler

- Azure Izleyici CMK, bir erken erişim özelliğidir ve kayıtlı abonelikler için etkinleştirilmiştir.

- Bu makalede açıklanan CMK dağıtımı, üretim kalitesiyle birlikte dağıtılır ve bir erken erişim özelliği olduğundan desteklenir.

- CMK özelliği, bir Azure Veri Gezgini (ADX) kümesi olan ve günde 1 GB ya da daha fazlasını gönderen müşterilere uygun olan adanmış bir veri deposu kümesine dağıtılır. 

- CMK fiyatlandırma modeli şu anda kullanılamaz ve bu makalede ele alınmamaktadır. Takvim yılının (CY) 2020 ' nin ikinci çeyreğinde adanmış ADX kümesine yönelik fiyatlandırma modeli beklenir ve var olan CMK dağıtımları için geçerli olacaktır.

- Bu makalede Log Analytics çalışma alanları için CMK yapılandırması açıklanır. Application Insights bileşenleri için CMK Ayrıca, ek içinde farklar listelenirken Bu makale kullanılarak da desteklenir.

> [!NOTE]
> Log Analytics ve Application Insights aynı veri deposu platformunu ve sorgu altyapısını kullanıyor.
> Bu iki depoyu, Azure Izleyici altında tek bir Birleşik Günlükler deposu oluşturmak için Log Analytics Application Insights tümleştirerek bir araya getiriyoruz. Bu değişiklik takvim yılı 2020 ' nin ikinci çeyreğinde planlanmaktadır. Application Insights verileriniz için CMK 'yı dağıtmanız gerekmiyorsa, bu tür dağıtımlar birleştirme tarafından kesintiye uğradığından ve günlüğe geçişten sonra CMK 'yı yeniden yapılandırmanız gerekecektir. Analiz çalışma alanı. Gün başına 1 TB, küme düzeyinde uygulanır ve birleştirme ikinci çeyrek boyunca tamamlanana kadar Application Insights ve Log Analytics ayrı kümeler gerektirir.

## <a name="customer-managed-key-cmk-overview"></a>Müşteri tarafından yönetilen anahtar (CMK) genel bakış

[Bekleyen şifreleme](https://docs.microsoft.com/azure/security/fundamentals/encryption-atrest) , kuruluşlarda ortak bir gizlilik ve güvenlik gereksinimidir. Azure 'un bekleyen şifrelemeyi tamamen yönetmesine izin verebilir, şifreleme veya şifreleme anahtarlarını yakından yönetmek için çeşitli seçenekleriniz vardır.

Azure Izleyici veri deposu, Azure depolama 'da depolanırken Azure tarafından yönetilen anahtarlar kullanılarak bekleyen tüm verilerin şifrelenmesini sağlar. Azure Izleyici Ayrıca, sistem tarafından atanan [yönetilen kimlik](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) doğrulaması kullanılarak erişilen [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-overview)' de depolanan kendi anahtarınızı kullanarak veri şifrelemesi için bir seçenek sağlar. Bu anahtar, [yazılım ya da donanım HSM korumalı](https://docs.microsoft.com/azure/key-vault/key-vault-overview)olabilir.
Azure Izleyici şifreleme kullanımı, [Azure depolama şifrelemesiyle](https://docs.microsoft.com/azure/storage/common/storage-service-encryption#about-azure-storage-encryption) aynı şekilde çalışır.

Azure Izleyici depolama 'nın sarmalama ve sarmalama işlemleri için Key Vault erişim sıklığı, 6 ila 60 saniye arasındadır. Azure Izleyici depolaması, her zaman bir saat içindeki anahtar izinlerinde yapılan değişikliklere uyar.

Son 14 gün içinde alınan veriler, verimli sorgu altyapısı işlemi için etkin-önbellek (SSD-desteklenen) olarak da tutulur. Bu veriler CMK yapılandırmasına bakılmaksızın Microsoft anahtarlarıyla şifreli olarak kalır, ancak SSD 'nin CMK erken 2020 ile şifrelenmesini sağlamak için çalışıyoruz.

## <a name="how-cmk-works-in-azure-monitor"></a>Azure Izleyici 'de CMK nasıl çalışmaktadır

Azure Izleyici, Azure Key Vault erişim sağlamak için sistem tarafından atanan yönetilen kimliğin yararlanır. Sistem tarafından atanan yönetilen kimlik, yalnızca tek bir Azure kaynağıyla ilişkilendirilebilir. Azure Izleyici veri deposu (ADX kümesi) kimliği, küme düzeyinde desteklenir ve bu, CMK özelliğinin adanmış bir ADX kümesine teslim edildiğini belirler. Birden çok çalışma alanı üzerinde CMK 'yi desteklemek için yeni bir Log Analytics kaynak (*küme*) Key Vault ve Log Analytics çalışma alanlarınız arasında ara kimlik bağlantısı olarak gerçekleştirilir. Bu kavram, sistem tarafından atanan kimlik kısıtlamasına uygundur ve kimlik, ADX kümesi ile Log Analytics *küme* kaynağı arasında saklanır, ancak tüm ilişkili çalışma alanlarının verileri Key Vault anahtarınızla korunur. Underlay ADX küme depolaması, Azure Active Directory aracılığıyla Azure Key Vault kimlik doğrulamak ve erişmek için *küme* kaynağıyla ilişkili\'yönetilen kimliği kullanır.

![CMK genel bakış](media/customer-managed-keys/cmk-overview.png)
1.  Müşterinin Key Vault.
2.  Müşterinin Log Analytics küme kaynağı, Key Vault izinleri olan yönetilen kimliğe sahip: kimlik, veri deposu (ADX kümesi) düzeyinde desteklenir.
3.  Azure Izleyici adanmış ADX kümesi.
4.  CMK şifrelemesi için küme kaynağıyla ilişkili müşteri çalışma alanları.

## <a name="encryption-keys-management"></a>Şifreleme anahtarları yönetimi

Depolama veri şifrelemesi ile ilgili 3 tür anahtar vardır:

- Key Vault 'de **kek** anahtar şifreleme anahtarı (CMK)
- **AEK** hesabı şifreleme anahtarı
- **Dek** -veri şifreleme anahtarı

Aşağıdaki kurallar geçerlidir:

- ADX depolama hesabı, AEK olarak bilinen her depolama hesabı için benzersiz bir şifreleme anahtarı oluşturur.

- AEK, diske yazılan her veri bloğunu şifrelemek için kullanılan anahtarlar olan DEKs 'leri türetmek için kullanılır.

- Anahtarınızı Key Vault ' de yapılandırıp *küme* kaynağında ona başvurduğunuzda, Azure depolama, Azure Key Vault içindeki kek Ile AEK 'i sarmalar.

- KEK, Key Vault hiçbir durumda kalmayacak ve HSM anahtarı durumunda donanımdan ayrılmayacaktır.

- Azure depolama, Azure Active Directory üzerinden Azure Key Vault kimlik doğrulaması yapmak ve erişmek için *küme* kaynağıyla ilişkili yönetilen kimliği kullanır.

- Okuma/yazma işlemleri için, Azure depolama, şifreleme ve şifre çözme işlemleri gerçekleştirmek üzere AEK 'i sarmalamak ve sarmalamak için Azure Key Vault istek gönderir.

## <a name="cmk-provisioning-procedure"></a>CMK sağlama prosedürü

Application Insights CMK yapılandırması için 3 ve 6. adımlar için ek içeriğini izleyin.

1. Abonelik beyaz listesi--bu erken erişim özelliği için gereklidir
2. Azure Key Vault oluşturma ve anahtar depolama
3. *Küme* kaynağı oluşturma
4. Key Vault izin verin
5. Azure Izleyici veri deposu (ADX kümesi) sağlama
6. Log Analytics çalışma alanları ilişkilendirmesi

Yordam, şu anda Kullanıcı arabiriminde desteklenmez ve sağlama işlemi REST API aracılığıyla gerçekleştirilir.

> [!IMPORTANT]
> Herhangi bir API isteği, istek üst bilgisinde bir taşıyıcı yetkilendirme belirteci içermelidir.

Örnek:

```rst
GET
https://management.azure.com/subscriptions/<subscriptionId>/resourcegroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>?api-version=2015-11-01-preview
Authorization: Bearer eyJ0eXAiO....
```

Burada *eyJ0eXAiO....* tam yetkilendirme belirtecini temsil eder. 

Aşağıdaki yöntemlerden birini kullanarak belirteci edinebilirsiniz:

1. [Uygulama kayıtları](https://docs.microsoft.com/graph/auth/auth-concepts#access-tokens) yöntemi kullanın.

2. Azure portal
    1. "Geliştirici aracında Azure portal git (F12)
    1. "Batch? api-Version" örneklerinden birinde "Istek üstbilgileri" altında yetkilendirme dizesini arayın. Şöyle görünür: "yetkilendirme: taşıyıcı \<belirteç\>". 
    1. Aşağıdaki örneklere göre kopyalayıp API çağrına ekleyin.

3. Azure REST belge sitesine gidin. Herhangi bir API 'de "deneyin" düğmesine basın ve taşıyıcı belirtecini kopyalayın.

### <a name="subscription-whitelisting"></a>Abonelik beyaz listesi

CMK özelliği, erken erişim özelliğidir. *Küme* kaynakları oluşturmayı planladığınız abonelikler, Azure ürün grubu tarafından önceden listelenmiş olmalıdır. Abonelik kimliklerinizi sağlamak için kişilerinizi Microsoft 'a kullanın.

> [!IMPORTANT]
> CMK özelliği bölgesel. Azure Key Vault, depolama hesabınız, *küme* kaynağınız ve ilişkili Log Analytics çalışma alanlarınızın aynı bölgede olması gerekir, ancak bunlar farklı aboneliklerde olabilir.

### <a name="storing-encryption-key-kek"></a>Şifreleme anahtarını depolama (KEK)

Azure Key Vault bir kaynak oluşturun, ardından veri şifrelemesi için kullanılacak bir anahtar oluşturun veya içeri aktarın.

Azure Key Vault anahtarınızı ve Azure Izleyici verilerinize erişimi korumak için kurtarılabilir olarak yapılandırılmalıdır.

Bu ayarlar CLı ve PowerShell aracılığıyla kullanılabilir:
- [Geçici silme](https://docs.microsoft.com/azure/key-vault/key-vault-ovw-soft-delete) açık olmalıdır
- Geçici silme işleminden sonra bile gizli/kasaların silinmesini zorlamak için [Temizleme koruması](https://docs.microsoft.com/azure/key-vault/key-vault-ovw-soft-delete#purge-protection) açılmalıdır

### <a name="create-cluster-resource"></a>*Küme* kaynağı oluştur

Bu kaynak, Key Vault ve çalışma alanlarınız arasında ara kimlik bağlantısı olarak kullanılır. Aboneliklerinizin beyaz listeye alınmasını doğruladıktan sonra, çalışma alanlarınızın bulunduğu bölgede bir Log Analytics *küme* kaynağı oluşturun. Application Insights ve Log Analytics ayrı küme kaynakları gerektirir. Küme kaynağının türü, "clusterType" özelliği ' LogAnalytics ' veya ' ApplicationInsights ' olarak ayarlanarak oluşturma sırasında tanımlanmıştır. Küme kaynak türü değiştirilemez.

Application Insights CMK yapılandırması için bu adım için ek içeriğini izleyin.

**Oluşturma**

```rst
PUT https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters/<cluster-name>?api-version=2019-08-01-preview
Authorization: Bearer <token>
Content-type: application/json

{
  "location": "<region-name>",
   "properties": {
      "clusterType": "LogAnalytics"
    },
   "identity": {
      "type": "systemAssigned"
   }
}
```
"clusterType" değeri, Application Insights CMK için "ApplicationInsights".

**Yanıt**

Kimlik, oluşturma zamanında *küme* kaynağına atanır.

```json
{
  "identity": {
    "type": "SystemAssigned",
    "tenantId": "tenant-id",
    "principalId": "principle-id"
  },
  "properties": {
    "provisioningState": "Succeeded",
    "clusterType": "LogAnalytics", 
    "clusterId": "cluster-id"
  },
  "id": "/subscriptions/subscription-id/resourceGroups/resource-group-name/providers/Microsoft.OperationalInsights/clusters/cluster-name",    //The cluster resource Id
  "name": "cluster-name",
  "type": "Microsoft.OperationalInsights/clusters",
  "location": "region-name"
}

```
"PrincipalId", *küme* kaynağı için yönetilen kimlik hizmeti tarafından oluşturulan bir GUID 'dir.

> [!IMPORTANT]
> Sonraki adımlarda gereksinim duyduğundan bu yana "küme kimliği" değerini kopyalayın ve saklayın.

Herhangi bir nedenden dolayı *küme* kaynağını silmek için--örneğin, farklı bir ad veya clustertype ile oluşturun, bu API çağrısını kullanın:

```rst
DELETE
https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters/<cluster-name>?api-version=2019-08-01-preview
```

### <a name="grant-key-vault-permissions"></a>Key Vault izinleri verme

Key Vault güncelleştirin ve küme kaynağı için erişim ilkesi ekleyin. Key Vault izinler, daha sonra veri şifreleme için kullanılacak Azure Izleyici depolama alanının altına yayılır.
Key Vault Azure portal açın ve bu ayarlarla yeni bir ilke oluşturmak için "erişim Ilkeleri" ve ardından "+ erişim Ilkesi Ekle" seçeneğine tıklayın:

- Anahtar izinleri: ' Get ', ' Wrap Key ' ve ' Wrap Key ' izinlerini seçin.

- Asıl öğe seçin: önceki adımın yanıtında bulunan "Clusterıd" değeri olan küme kimliğini girin.

![Key Vault izinleri verme](media/customer-managed-keys/grant-key-vault-permissions.png)

Key Vault, anahtarınızı korumak ve Azure Izleyici verilerinize erişmek için kurtarılabilir olarak yapılandırıldığını doğrulamak için *Get* izni gerekir.

*Küme* kaynağı Azure Resource Manager yayılana kadar birkaç dakika sürer. Bu erişim Ilkesini *küme* kaynağı oluşturulduktan hemen sonra yapılandırırken geçici bir hata oluşabilir. Bu durumda, birkaç dakika sonra yeniden deneyin.

### <a name="update-cluster-resource-with-key-identifier-details"></a>Anahtar tanımlayıcı ayrıntıları ile küme kaynağını güncelleştirme

Bu adım Key Vault sonraki önemli sürüm güncelleştirmelerini aşağıda uygular. Azure Izleyici depolamanın yeni anahtar sürümünü kullanmasına izin vermek için, *küme* kaynağını Key Vault *anahtar tanımlayıcı* ayrıntıları ile güncelleştirin. Anahtar tanımlayıcı ayrıntılarını almak için Azure Key Vault ' de anahtarınızın güncel sürümünü seçin.

![Key Vault izinleri verme](media/customer-managed-keys/key-identifier-8bit.png)

KeyVaultProperties *küme* kaynağını anahtar tanımlayıcı ayrıntıları ile güncelleştirin.

**Güncelleştirme**

```rst
PUT https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters/<cluster-name>?api-version=2019-08-01-preview
Authorization: Bearer <token>
Content-type: application/json

{
   "properties": {
       "KeyVaultProperties": {
            KeyVaultUri: "https://<key-vault-name>.vault.azure.net",
            KeyName: "<key-name>",
            KeyVersion: "<current-version>"
            },
   },
   "location":"<region-name>",
   "identity": { 
        "type": "systemAssigned" 
        }
}
```
"KeyVaultProperties" Key Vault anahtar tanımlayıcısı ayrıntılarını içerir.

**Yanıt**

```json
{
  "identity": {
    "type": "SystemAssigned",
    "tenantId": "tenant-id",
    "principalId": "principle-id"
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

### <a name="azure-monitor-data-store-adx-cluster-provisioning"></a>Azure Izleyici veri deposu (ADX kümesi) sağlama

Özelliğin erken erişim süresi boyunca, ADX kümesi, önceki adımlar tamamlandıktan sonra ürün ekibi tarafından el ile sağlanır. Aşağıdaki bilgileri sağlamak için Microsoft ile sahip olduğunuz kanalı kullanın:

- Yukarıdaki adımların başarıyla tamamlandığını onaylayın.

- Önceki adımdan gelen JSON yanıtı. Get API çağrısı kullanılarak herhangi bir zamanda alınabilir:

   ```rst
   GET https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters/<cluster-name>?api-version=2019-08-01-preview
   Authorization: Bearer <token>
   ```

   **Yanıt**
   ```json
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
     "id": "/subscriptions/subscription-id/resourceGroups/resource-group-name/providers/Microsoft.OperationalInsights/clusters/cluster-name",
     "name": "cluster-name",
     "type": "Microsoft.OperationalInsights/clusters",
     "location": "region-name"
   }
   ```

### <a name="workspace-association-to-cluster-resource"></a>*Küme* kaynağıyla çalışma alanı ilişkilendirmesi

> [!NOTE]
> Bu adım **yalnızca** , Microsoft kanalınızdan, **Azure izleyici veri deposu (ADX kümesi) sağlamasının** karşılandığına ait bir ürün grubundan onay aldıktan sonra taşınmalıdır. Bu **sağlamadan**önce çalışma alanlarını ve veri alma verilerini ilişkilendirirseniz, veriler bırakılır ve kurtarılamaz.

Application Insights CMK yapılandırması için bu adım için ek içeriğini izleyin.

```rst
PUT https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/workspaces/<workspace-name>/linkedservices/cluster?api-version=2019-08-01-preview 
Authorization: Bearer <token>
Content-type: application/json

{
  "properties": {
    "WriteAccessResourceId": "subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/clusters/<cluster-name>"
    }
}
```
*Clusterdefinitionıd* , önceki adımdan alınan yanıtta belirtilen *clusterıd* değeridir.

**Yanıt**

```json
{
  "properties": {
    "WriteAccessResourceId": "subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/clusters/<cluster-name>"
    },
  "id": "/subscriptions/subscription-id/resourcegroups/resource-group-name/providers/microsoft.operationalinsights/workspaces/workspace-name/linkedservices/cluster",
  "name": "workspace-name/cluster",
  "type": "microsoft.operationalInsights/workspaces/linkedServices",
}
```

İlişkilendirmeden sonra, çalışma alanınıza gönderilen veriler yönetilen anahtarınızla şifreli olarak depolanır.

### <a name="workspace-association-verification"></a>Çalışma alanı ilişkilendirme doğrulaması
Çalışma [alanları – yanıtı al](https://docs.microsoft.com/rest/api/loganalytics/workspaces/get) ' a bakarak bir çalışma alanının bir *custıcı* kaynakla ilişkilendirildiğini doğrulayabilirsiniz. İlişkili çalışma alanı, *küme* kaynak kimliği ile bir ' clusterresourceıd ' özelliğine sahip olacaktır.

```rest
GET https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalInsights/workspaces/<workspace-name>?api-version=2015-11-01-preview
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
      "enableLogAccessUsingOnlyResourcePermissions": true/false,
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

Azure Izleyici depolaması, her zaman bir saat içindeki anahtar izinlerinde yapılan değişikliklere göre her zaman uygulanır, normalde daha erken ve depolama kullanılamaz hale gelir. *Küme* kaynağınız ile ilişkili çalışma alanlarına alınan tüm veriler bırakılır ve sorgular başarısız olur. Anahtar iptal edildiği ve çalışma alanlarınızın silinmediği sürece, önceden alınan veriler Azure Izleyici depolaması 'nda erişilemez durumda kalır. Erişilemeyen veriler veri bekletme ilkesine tabidir ve bekletmeye ulaşıldığında temizlenir.

Depolama, şifreleme anahtarını sarmalamak ve erişildikten sonra, veri alımı ve sorgu 30 dakika içinde devam ettirmeye çalışmak için Key Vault düzenli aralıklarla yoklar.

## <a name="cmk-kek-rotation"></a>CMK (KEK) döndürme

CMK 'nin dönmesi, yeni Azure Key Vault anahtarı sürümü ile *küme* kaynağının açık güncelleştirilmesini gerektirir. Azure Izleyicisini yeni anahtar sürümünüzle güncelleştirmek için, " *küme* kaynağını *anahtar tanımlayıcı* ayrıntıları ile güncelleştirme" adımındaki yönergeleri izleyin.

Anahtarınızı Key Vault ' de güncelleştirir ve *küme* kaynağında yeni *anahtar tanımlayıcı* ayrıntılarını güncelleştirmezseniz, Azure izleyici depolaması önceki anahtarınızı kullanmaya devam eder.

## <a name="limitations-and-constraints"></a>Sınırlamalar ve kısıtlamalar

- CMK özelliği, ADX küme düzeyinde desteklenir ve adanmış bir Azure Izleyici ADX kümesi gerektirir

- Abonelik başına en fazla *küme* kaynağı sayısı 5 ile sınırlıdır

- Çalışma alanıyla *küme* kaynağı ilişkilendirmesi yalnızca, bir ürün grubundan ADX küme sağlamasının yerine getirildiğine ilişkin bir onay aldıktan sonra taşınmalıdır. Bu sağlamadan önce gönderilen veriler bırakılır ve geri alınamaz.

- CMK şifrelemesi, CMK yapılandırmasından sonra yeni alınan veriler için geçerlidir. CMK yapılandırmasından önce gelen veriler, Microsoft anahtarıyla şifrelenmeden kaldı. Yapılandırmanın öncesinde ve sonrasında verileri sorunsuz bir şekilde sorgulayabilirsiniz.

- Çalışma alanı bir *küme* kaynağıyla ilişkilendirildiğinde, veriler anahtarınızla şifrelendiğinden ve Azure Key Vault ' de kek olmadan erişilebilir olmadığından, *küme* kaynağı ile ilişkili olamaz.

- Azure Key Vault kurtarılabilir olarak yapılandırılmalıdır. Bu özellikler varsayılan olarak etkinleştirilmemiştir ve CLı ve PowerShell kullanılarak yapılandırılmalıdır:

  - [Geçici silme](https://docs.microsoft.com/azure/key-vault/key-vault-ovw-soft-delete) açık olmalıdır
  - Geçici silme işleminden sonra bile gizli/kasaların silinmesini zorlamak için [Temizleme koruması](https://docs.microsoft.com/azure/key-vault/key-vault-ovw-soft-delete#purge-protection) açılmalıdır

- Application Insights ve Log Analytics ayrı *küme* kaynakları gerektirir. *Küme* kaynağının türü, "clustertype" özelliği ' loganalytics ' veya ' ApplicationInsights ' olarak ayarlanarak oluşturma sırasında tanımlanmıştır. *Küme* kaynak türü değiştirilemez.

- *Küme* kaynağı başka bir kaynak grubuna veya aboneliğe taşıma Şu anda desteklenmiyor.

- Azure Key Vault, *küme* kaynağı ve ilişkili çalışma alanlarınız aynı bölgede ve aynı Azure Active Directory (Azure AD) kiracısında olmalıdır, ancak farklı aboneliklerde olabilir.

- *Küme* kaynağıyla çalışma alanı ilişkilendirmesi, başka bir *küme* kaynağıyla ilişkiliyse başarısız olur

## <a name="troubleshooting-and-management"></a>Sorun giderme ve yönetim

- Key Vault kullanılabilirliği
    - Normal işlemde, daha kısa süreler için depolama için AEK, her zaman sarmalama geri Key Vault.
    
    - Geçici bağlantı hataları. Depolama, anahtarların kısa bir süre boyunca önbellekte kalmasına izin vererek geçici hataları (zaman aşımları, bağlantı hataları, DNS sorunları) işler ve bu çok daha fazla küçük blinler sağlar. Sorgu ve alma özellikleri kesinti olmadan devam eder.
    
    - Canlı site, yaklaşık 30 dakikalık bir süre sonra, depolama hesabının kullanılamaz hale gelmesine neden olur. Sorgu özelliği kullanılamaz ve veri kaybını önlemek için Microsoft anahtar kullanarak birkaç saat boyunca önbelleğe alınır. Key Vault erişimi geri yüklendiğinde, sorgu kullanılabilir hale gelir ve geçici önbelleğe alınmış veriler veri deposuna alınır ve CMK ile şifrelenir.

- Bir *küme* kaynağı oluşturur ve KeyVaultProperties 'i hemen belirtirseniz, sistem kimliği *küme* kaynağına atanana kadar erişim ilkesi tanımlanmadığından işlem başarısız olabilir.

- Mevcut *küme* kaynağını KeyVaultProperties ile güncelleştirirseniz ve ' Get ' anahtar erişim ilkesi Key Vault eksik olduğunda, işlem başarısız olur.

- Bir çalışma alanıyla ilişkili bir *küme* kaynağını silmeye çalışırsanız, silme işlemi başarısız olur.

- Bir kaynak grubu için tüm *küme* kaynaklarını almak üzere bu API 'yi kullanın:

  ```rst
  GET https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters?api-version=2019-08-01-preview
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

- Bir aboneliğin tüm *küme* kaynaklarını almak IÇIN bu API çağrısını kullanın:

  ```rst
  GET https://management.azure.com/subscriptions/<subscription-id>/providers/Microsoft.OperationalInsights/clusters?api-version=2019-08-01-preview
  Authorization: Bearer <token>
  ```
    
  **Yanıt**
    
  ' Kaynak grubu için*küme* kaynakları ' için aynı yanıt, ancak abonelik kapsamı.
    
- *Küme* kaynağını silmek IÇIN bu API çağrısını kullanın. *küme* kaynağınızı silebilmek için önce tüm ilişkili çalışma alanlarını silmeniz gerekir:

  ```rst
  DELETE
  https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters/<cluster-name>?api-version=2019-08-01-preview
  Authorization: Bearer <token>
  ```

  **Yanıt**

  200 TAMAM


## <a name="appendix"></a>Ek

Application Insights müşteri tarafından yönetilen anahtar (CMK) de desteklenir, ancak uygulama Insight bileşenleriniz için CMK dağıtımını planlamaya yardımcı olması için aşağıdaki değişikliği göz önünde bulundurmanız gerekir.

Log Analytics ve Application Insights aynı veri deposu platformunu ve sorgu altyapısını kullanıyor. Bu iki depoyu, Azure Izleyici altında, ' nin ikinci çeyreğinde tek bir Birleşik günlük deposu sağlamak üzere Log Analytics Application Insights tümleştirerek bir araya getiriyoruz.
2020. Bu değişiklik, uygulamanızın bilgi işlem verilerini Log Analytics çalışma alanlarına getirecek ve çalışma alanınızdaki CMK yapılandırması, Application Insights verilerinize da uygulanamaken sorguları, öngörüleri ve diğer geliştirmeleri sunacaktır.

> [!NOTE]
> Tümleştirmeden önce Application Insight verileriniz için CMK 'yı dağıtmanız gerekmiyorsa, bu tür dağıtımlar tümleştirme tarafından kesintiye uğradığından ve günlüğe geçiş sonrasında CMK 'yi yeniden yapılandırmanız gerekecektir. Application Insights CMK ile beklemeyi öneririz. Analiz çalışma alanı. Gün başına 1 TB, küme düzeyinde uygulanır ve birleştirme ikinci çeyrek boyunca tamamlanana kadar Application Insights ve Log Analytics ayrı kümeler gerektirir.

## <a name="application-insights-cmk-configuration"></a>Application Insights CMK yapılandırması

Application Insights CMK 'nin yapılandırması, bu makalede açıklanan, kısıtlamalar ve sorun giderme adımları dahil olmak üzere bu makalede gösterilen işlemle aynıdır:

- *Küme* kaynağı oluşturma

- Bir bileşeni bir *küme* kaynağıyla ilişkilendir

Application Insights için CMK yapılandırılırken, yukarıda listelenenler yerine bu adımları kullanın.

### <a name="create-a-cluster-resource"></a>*Küme* kaynağı oluşturma

Bu kaynak, Key Vault ve bileşenleriniz arasında ara kimlik bağlantısı olarak kullanılır. Aboneliklerinizin daha fazla listelendiğini belirten bir onay aldıktan sonra, bileşenlerinizin bulunduğu bölgede bir Log Analytics *küme* kaynağı oluşturun. *Küme* kaynağının türü, *Clustertype* özelliği *loganalytics*veya *ApplicationInsights*olarak ayarlanarak oluşturma sırasında tanımlanmıştır. Application Insights CMK için *ApplicationInsights* olması gerekir. *Clustertype* ayarı yapılandırmadan sonra değiştirilemez.

**Oluşturma**

```rst
PUT https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters/<cluster-name>?api-version=2019-08-01-preview
Authorization: Bearer <token>
Content-type: application/json

{
  "location": "<region-name>",
  "properties": {
      "clusterType":"ApplicationInsights"
  },
  "identity": {
      "type": "systemAssigned"
  }
}
```

**Yanıt**

Kimlik, oluşturma zamanında *küme* kaynağına atanır.

```json

{
  "identity": {
    "type": "SystemAssigned",
    "tenantId": "tenant-id",
    "principalId": "principle-id"
  },
  "properties": {
    "provisioningState": "Succeeded",
    "clusterType": "ApplicationInsights",    //The value is ‘ApplicationInsights’ for Application Insights CMK
    "clusterId": "cluster-id" 
  },
  "id": "/subscriptions/subscription-id/resourceGroups/resource-group-name/providers/Microsoft.OperationalInsights/clusters/cluster-name", //The cluster resource Id
  "name": "cluster-name",
  "type": "Microsoft.OperationalInsights/clusters",
  "location": "region-name"
}
```
"PrincipalId", yönetilen kimlik hizmeti tarafından oluşturulan bir GUID 'dir.

> [!IMPORTANT]
> Sonraki adımlarda gereksinim duyduğundan bu yana "küme kimliği" değerini kopyalayın ve saklayın.

### <a name="associate-a-component-to-a-cluster-resource-using-components---create-or-updatehttpsdocsmicrosoftcomrestapiapplication-insightscomponentscreateorupdate-api"></a>Bileşenleri kullanarak bir bileşeni *küme* kaynağıyla ilişkilendirme [-API oluşturma veya güncelleştirme](https://docs.microsoft.com/rest/api/application-insights/components/createorupdate)

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
"Clusterdefinitionıd", önceki adımdan yanıt olarak belirtilen "Clusterıd" değeridir.
"tür" örneği "Web" dir.

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
"Clusterdefinitionıd", bu bileşenle ilişkili *küme* kaynak kimliğidir.

İlişkilendirmeden sonra, bileşenlerinizi gönderilen veriler, yönetilen anahtarınızla şifreli olarak depolanır.
