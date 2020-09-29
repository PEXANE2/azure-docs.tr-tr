---
title: Azure Izleyici, ayrılmış kümeleri günlüğe kaydeder
description: 1 TB 'den fazla veri izleme verisi alan müşteriler, paylaşılan kümeler yerine adanmış olarak kullanılabilir
ms.subservice: logs
ms.topic: conceptual
author: rboucher
ms.author: robb
ms.date: 09/16/2020
ms.openlocfilehash: 4ad3aa7169fcf7eeda6e56a2eab6669b8783d77d
ms.sourcegitcommit: a0c4499034c405ebc576e5e9ebd65084176e51e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/29/2020
ms.locfileid: "91461470"
---
# <a name="azure-monitor-logs-dedicated-clusters"></a>Azure Izleyici, ayrılmış kümeleri günlüğe kaydeder

Azure Izleyici günlükleri adanmış kümeler, yüksek hacimli müşterilere daha iyi hizmeti sağlamak için kullanılabilen bir dağıtım seçeneğidir. Günde 4 TB 'den fazla veri alan müşteriler adanmış kümeler kullanacaktır. Adanmış kümelere sahip müşteriler bu kümelerde barındırılacak çalışma alanlarını seçebilirler.

Yüksek hacimli destek dışında, adanmış kümeler kullanmanın başka avantajları da vardır:

- **Hız limiti** -müşteri, yalnızca adanmış kümede daha yüksek alım oranı sınırlarına sahip olabilir.
- **Özellikler** -belirli kurumsal özellikler yalnızca özel kümeler üzerinde mevcuttur-özellikle müşteri tarafından yönetilen anahtarlar (CMK) ve kasa desteği. 
- **Tutarlılık** -müşteriler kendi özel kaynaklarına sahiptir ve bu nedenle aynı paylaşılan altyapıda çalışan diğer müşterilerin hiçbir etkisi yoktur.
- **Maliyet verimliliği** -atanan kapasite rezervasyon katmanları tüm küme alımını hesaba alcağından ve bu, bazıları küçük ve kapasite rezervasyon indirimlerine uygun olmasa bile, tüm çalışma alanları için geçerli olduğundan adanmış küme kullanımı daha uygun maliyetli olabilir.
- Tüm çalışma alanları aynı kümede ise, **çoklu çalışma** alanları sorguları daha hızlı çalışır.

Adanmış kümeler, müşterilerin gün başına en az 1 TB veri alma kapasitesi kullanarak işlemesini gerektirir. Adanmış bir kümeye geçiş basittir. Veri kaybı veya hizmet kesintisi yok. 

> [!IMPORTANT]
> Adanmış kümeler onaylanır ve üretim dağıtımları için tam olarak desteklenir. Ancak geçici kapasite kısıtlamaları nedeniyle, özelliği kullanmak için ön kayıt yapmanız gerekir. Abonelik kimliklerinizi sağlamak için kişilerinizi Microsoft 'a kullanın.

## <a name="management"></a>Yönetim 

Adanmış kümeler, Azure Izleyici günlük kümelerini temsil eden bir Azure kaynağı aracılığıyla yönetilir. Tüm işlemler bu kaynakta PowerShell veya REST API kullanılarak yapılır.

Küme oluşturulduktan sonra, yapılandırılabilir ve onunla bağlantılı çalışma alanları bulunabilir. Bir çalışma alanı bir kümeye bağlandığında, çalışma alanına gönderilen yeni veriler kümede bulunur. Yalnızca kümeyle aynı bölgedeki çalışma alanları kümeyle bağlantılı olabilir. Çalışma alanları, bazı sınırlamalar içeren bir kümeden beğenilmiş olabilir. Bu sınırlamalar hakkında daha fazla ayrıntı bu makaleye dahildir. 

Küme düzeyindeki tüm işlemler `Microsoft.OperationalInsights/clusters/write` kümede eylem iznini gerektirir. Bu izin, eylemi içeren sahip veya katkıda bulunan veya `*/write` eylemi içeren Log Analytics katkıda bulunan rolü aracılığıyla verilebilir `Microsoft.OperationalInsights/*` . Log Analytics izinler hakkında daha fazla bilgi için bkz. [Azure izleyici 'de günlük verilerine ve çalışma alanlarına erişimi yönetme](../platform/manage-access.md). 

## <a name="billing"></a>Faturalama

Adanmış kümeler yalnızca kapasite ayırma katmanları olan veya içermeyen GB başına plan kullanan çalışma alanları için desteklenir. Adanmış kümelerin, bu tür kümeler için 1 TB 'den fazla alma işlemi yapan müşteriler için ek ücret alınmaz. "Alma-alma" işlemi, küme düzeyinde en az 1 TB/günlük bir kapasite ayırma katmanı atamış oldukları anlamına gelir. Kapasite rezervasyonu küme düzeyinde iliştirirken, verilerin gerçek şarjı için iki seçenek vardır:

- *Küme* (varsayılan)-kümeniz için kapasite ayırma maliyetleri, *küme* kaynağına atanır.
- *Çalışma alanları* -kümenizin kapasite ayırma maliyeti, kümedeki çalışma alanlarıyla orantılı olarak atanır. Gün için toplam alınan veriler kapasite rezervasyonunun altındaysa, *küme* kaynağı bazı kullanımlarda faturalandırılır. Küme fiyatlandırma modeli hakkında daha fazla bilgi edinmek için bkz. [Log Analytics adanmış kümeler](../platform/manage-cost-storage.md#log-analytics-dedicated-clusters) .

Adanmış kümelerin faturalandırılması hakkında daha fazla bilgi için bkz. [Log Analytics adanmış küme faturalaması](../platform/manage-cost-storage.md#log-analytics-dedicated-clusters).

## <a name="creating-a-cluster"></a>Küme oluşturma

Önce adanmış bir küme oluşturmaya başlamak için küme kaynakları oluşturursunuz.

Aşağıdaki özellikler belirtilmelidir:

- **Clustername**: yönetimsel amaçlar için kullanılır. Kullanıcılar bu ada gösterilmez.
- **Resourcegroupname**: tüm Azure kaynakları için, kümeler bir kaynak grubuna aittir. Kümeler genellikle kuruluştaki birçok ekip tarafından paylaşıldığından, merkezi bir BT kaynak grubu kullanmanızı öneririz. Daha fazla tasarım konusunda dikkat edilmesi gereken noktalar için [Azure Izleyici günlüklerinin dağıtımını tasarlama](../platform/design-logs-deployment.md) konusunu inceleyin
- **Konum**: bir küme, belirli bir Azure bölgesinde bulunur. Yalnızca bu bölgede bulunan çalışma alanları bu kümeyle bağlantılı olabilir.
- **Skucapacity**: bir *küme* kaynağı oluştururken *Kapasite ayırma* düzeyini (SKU) belirtmeniz gerekir. *Kapasite ayırma* DÜZEYI 1.000 gb Ila 3.000 GB arasında olabilir. Bunu, daha sonra gerekirse 100 adımlarında güncelleştirebilirsiniz. Gün başına 3.000 GB 'den yüksek kapasite ayırma düzeyine ihtiyacınız varsa, adresinden bizimle iletişime geçin LAIngestionRate@microsoft.com . Küme maliyetleri hakkında daha fazla bilgi için bkz. [Log Analytics kümeleri Için maliyetleri yönetme](../platform/manage-cost-storage.md#log-analytics-dedicated-clusters)

*Küme* kaynağını oluşturduktan sonra, *SKU*, * Keyvaultproperties veya *billingtype*gibi ek özellikleri düzenleyebilirsiniz. Daha fazla ayrıntı için aşağıya bakın.

> [!WARNING]
> Küme oluşturma işlemi kaynak ayırmayı ve sağlamayı tetikler. Bu işlemin tamamlanması bir saate kadar sürebilir. Bunu zaman uyumsuz olarak çalıştırmanız önerilir.

Kümeleri oluşturan kullanıcı hesabının standart Azure kaynak oluşturma izni: `Microsoft.Resources/deployments/*` ve küme yazma iznine sahip olması gerekir `(Microsoft.OperationalInsights/clusters/write)` .

### <a name="create"></a>Oluştur 

**PowerShell**

```powershell
New-AzOperationalInsightsCluster -ResourceGroupName {resource-group-name} -ClusterName {cluster-name} -Location {region-name} -SkuCapacity {daily-ingestion-gigabyte} -AsJob

# Check when the job is done
Get-Job -Command "New-AzOperationalInsightsCluster*" | Format-List -Property *
```

**REST**

*Call* 
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

*Response*

200 OK ve bir üst bilgi olmalıdır.

### <a name="check-provisioning-status"></a>Sağlama durumunu denetleme

Log Analytics kümesinin sağlanması bir süre sürer. Sağlama durumunu çeşitli yollarla kontrol edebilirsiniz:

- Get-Azoperationalınsightscluster PowerShell komutunu kaynak grubu adıyla çalıştırın ve ProvisioningState özelliğini denetleyin. Bu değer, sağlama sırasında *Provisioningaccount* ve tamamlandığında *başarılı* olur.
  ```powershell
  New-AzOperationalInsightsCluster -ResourceGroupName {resource-group-name} 
  ```

- Yanıttan Azure-AsyncOperation URL değerini kopyalayın ve zaman uyumsuz işlemler durum denetimini izleyin.

- *Küme* KAYNAĞıNDA bir get isteği gönderin ve *provisioningstate* değerine bakın. Bu değer, sağlama sırasında *Provisioningaccount* ve tamamlandığında *başarılı* olur.

   ```rst
   GET https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters/<cluster-name>?api-version=2020-03-01-preview
   Authorization: Bearer <token>
   ```

   **Response**

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
       "billingType": "cluster",
       "clusterId": "cluster-id"
       },
     "id": "/subscriptions/subscription-id/resourceGroups/resource-group-name/providers/Microsoft.OperationalInsights/clusters/cluster-name",
     "name": "cluster-name",
     "type": "Microsoft.OperationalInsights/clusters",
     "location": "region-name"
   }
   ```

*PrincipalId* GUID, *küme* kaynağı için yönetilen kimlik hizmeti tarafından oluşturulur.

## <a name="change-cluster-properties"></a>Küme özelliklerini değiştirme

*Küme* kaynağınızı oluşturduktan ve tam olarak sağlandıktan sonra, PowerShell veya REST API kullanarak küme düzeyindeki ek özellikleri düzenleyebilirsiniz. Küme oluşturma sırasında kullanılabilen özellikler dışında, ek özellikler yalnızca küme sağlandıktan sonra ayarlanabilir:

- **Keyvaultproperties**: bir [Azure Monitor müşteri tarafından yönetilen anahtarı](../platform/customer-managed-keys.md#cmk-provisioning-procedure)sağlamak için kullanılan Azure Key Vault yapılandırmak için kullanılır. Şu parametreleri içerir:  *Keyvaulturi*, *KeyName*, *keyversion*. 
- **billingtype** - *billingtype* özelliği *küme* kaynağı ve verileri için faturalandırma atışmasını belirler:
- **Küme** (varsayılan)-kümeniz Için kapasite ayırma maliyetleri, *küme* kaynağına atanır.
- **Çalışma alanları** -bu, günün Toplam alınan verileri kapasite rezervasyonunun altındaysa *, kümeniz için* kapasite ayırma maliyetleri kümedeki çalışma alanlarıyla orantılı olarak atanır. Küme fiyatlandırma modeli hakkında daha fazla bilgi edinmek için bkz. [Log Analytics adanmış kümeler](../platform/manage-cost-storage.md#log-analytics-dedicated-clusters) . 

> [!NOTE]
> *Billingtype* özelliği PowerShell 'de desteklenmez.
> Küme özelliği güncelleştirmeleri zaman uyumsuz olarak yürütülemeyebilir ve tamamlanması biraz zaman alabilir.

**PowerShell**

```powershell
Update-AzOperationalInsightsCluster -ResourceGroupName {resource-group-name} -ClusterName {cluster-name} -KeyVaultUri {key-uri} -KeyName {key-name} -KeyVersion {key-version}
```

**REST**

> [!NOTE]
> Düzeltme ekini kullanarak *cluster* kaynak *SKU 'su*, *Keyvaultproperties* veya *billingtype* 'ı güncelleştirebilirsiniz.

Örneğin: 

*Call*

```rst
PATCH https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters/<cluster-name>?api-version=2020-03-01-preview
Authorization: Bearer <token>
Content-type: application/json

{
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

*Response*

200 OK ve üst bilgi

### <a name="check-cluster-update-status"></a>Küme güncelleştirme durumunu denetle

Anahtar tanımlayıcısının yayılmasının tamamlanmasının birkaç dakika sürer. Güncelleştirme durumunu iki şekilde kontrol edebilirsiniz:

- Yanıttan Azure-AsyncOperation URL değerini kopyalayın ve zaman uyumsuz işlemler durum denetimini izleyin. 

   VEYA

- *Küme* KAYNAĞıNDA bir get isteği gönderin ve *keyvaultproperties* özelliklerine bakın. Son güncellenen anahtar tanımlayıcı ayrıntılarınız yanıta döndürmelidir.

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
       "billingType": "cluster",
       "clusterId": "cluster-id"
     },
     "id": "/subscriptions/subscription-id/resourceGroups/resource-group-name/providers/Microsoft.OperationalInsights/clusters/cluster-name",
     "name": "cluster-name",
     "type": "Microsoft.OperationalInsights/clusters",
     "location": "region-name"
  }
  ```

## <a name="link-a-workspace-to-the-cluster"></a>Çalışma alanını kümeye bağlama

Bir çalışma alanı adanmış bir kümeyle bağlandığında, çalışma alanına alınan yeni veriler mevcut kümede kaldığında yeni kümeye yönlendirilir. Adanmış küme, müşteri tarafından yönetilen anahtarlar (CMK) kullanılarak şifrelendiyse, anahtarla yalnızca yeni veriler şifrelenir. Sistem bu farkı kullanıcılardan soyutlarken, sistem arka uçta çapraz küme sorguları gerçekleştirdiğinden, Kullanıcı çalışma alanını her zamanki gibi sorgular.

Bir küme, en fazla 100 çalışma alanına bağlanabilir. Bağlantılı çalışma alanları, kümeyle aynı bölgede bulunur. Sistem arka ucunu korumak ve verilerin parçalanmasını önlemek için bir çalışma alanı bir aya göre iki kez bir kümeye bağlanamaz.

Bağlantı işlemini gerçekleştirmek için hem çalışma alanı hem de *küme* kaynağı için ' yazma ' izinlerine sahip olmanız gerekir:

- Çalışma alanında: *Microsoft. Operationalınsights/Workspaces/Write*
- *Küme* kaynağında: *Microsoft. operationalınsights/kümeler/yazma*

Faturalama yönünden farklı olarak, bağlantılı çalışma alanı veri saklama süresi gibi kendi ayarlarını korur.
Çalışma alanı ve küme farklı aboneliklerde olabilir. Azure açık Thouse, her ikisini de tek bir kiracıya eşlemek için kullanılıyorsa, çalışma alanı ve kümenin farklı kiracılarda olması mümkündür.

Herhangi bir küme işlemi olarak, bir çalışma alanını bağlama yalnızca Log Analytics kümesi sağlama işleminin tamamlanmasından sonra gerçekleştirilebilir.

> [!WARNING]
> Çalışma alanını bir kümeye bağlamak için birden fazla arka uç bileşeninin eşitlenmesi ve eşitleme için bir doldurma gerekir. Bu işlemin tamamlanması iki saate kadar sürebilir. Bunu zaman uyumsuz olarak çalıştırmanızı öneririz.


### <a name="link-operations"></a>Bağlantı işlemleri

**PowerShell**

Bir kümeye bağlanmak için aşağıdaki PowerShell komutunu kullanın:

```powershell
# Find cluster resource ID
$clusterResourceId = (Get-AzOperationalInsightsCluster -ResourceGroupName {resource-group-name} -ClusterName {cluster-name}).id

# Link the workspace to the cluster
Set-AzOperationalInsightsLinkedService -ResourceGroupName {resource-group-name} -WorkspaceName {workspace-name} -LinkedServiceName cluster -WriteAccessResourceId $clusterResourceId -AsJob

# Check when the job is done
Get-Job -Command "Set-AzOperationalInsightsLinkedService" | Format-List -Property *
```


**REST**

Bir kümeye bağlanmak için aşağıdaki REST çağrısını kullanın:

*Gönder*

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

*Response*

200 Tamam ve üst bilgi.

### <a name="using-customer-managed-keys-with-linking"></a>Müşteri tarafından yönetilen anahtarları bağlama ile kullanma

Müşteri tarafından yönetilen anahtarlar kullanıyorsanız, alınan veriler ilişkilendirme işleminden sonra yönetilen anahtarınızla şifrelenmiş olarak depolanır ve bu da tamamlanması 90 dakika sürebilir. 

Çalışma alanı ilişkilendirme durumunu iki şekilde kontrol edebilirsiniz:

- Yanıttan Azure-AsyncOperation URL değerini kopyalayın ve zaman uyumsuz işlemler durum denetimini izleyin.

- [Çalışma alanları gönderin – Isteği alın](https://docs.microsoft.com/rest/api/loganalytics/workspaces/get) ve yanıtı gözlemleyin. İlişkili çalışma alanında "Özellikler" altında bir Clusterresourceıd vardır.

Gönderme isteği aşağıdakine benzer:

*Gönder*

```rest
GET https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalInsights/workspaces/<workspace-name>?api-version=2020-03-01-preview
Authorization: Bearer <token>
```

*Response*

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

## <a name="unlink-a-workspace-from-a-dedicated-cluster"></a>Çalışma alanının adanmış bir kümeden bağlantısını kaldırma

Bir çalışma alanının bir kümeden bağlantısını kaldırabilirsiniz. Bir çalışma alanının kümeden bağlantısını kaldırdıktan sonra, bu çalışma alanıyla ilişkili yeni veriler adanmış kümeye gönderilmez. Ayrıca, çalışma alanı faturalandırması artık küme aracılığıyla yapılmaz. Bağlantısız çalışma alanının eski verileri kümede kalabilir. Bu veriler, müşteri tarafından yönetilen anahtarlar (CMK) kullanılarak şifrelenirse Key Vault gizli dizileri tutulur. Sistem, bu değişikliği Log Analytics kullanıcılarından soyutlar. Kullanıcılar çalışma alanını her zamanki gibi sorgulayabilir. Sistem, kullanıcılara hiçbir belirtime gerek olmadan arka uçta çapraz küme sorguları gerçekleştirir.  

> [!WARNING] 
> Bir ay içinde çalışma alanı başına iki bağlama işlemi vardır. İşlem kaldırma eylemlerini uygun şekilde göz önünde bulundurmanız ve planlamanız zaman alın. 

## <a name="delete-a-dedicated-cluster"></a>Adanmış kümeyi silme

Ayrılmış bir küme kaynağı silinebilir. Silmeden önce, tüm çalışma alanlarının kümeden bağlantısını kaldırmanız gerekir. Küme kaynağı silindikten sonra, fiziksel küme bir temizleme ve silme işlemi girer. Küme silindiğinde kümede depolanan tüm veriler silinir. Veriler geçmişte kümeyle bağlantılı çalışma alanlarından olabilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Log Analytics adanmış küme faturalaması](../platform/manage-cost-storage.md#log-analytics-dedicated-clusters) hakkında bilgi edinin
- [Log Analytics çalışma alanlarının doğru tasarımı](../platform/design-logs-deployment.md) hakkında bilgi edinin
