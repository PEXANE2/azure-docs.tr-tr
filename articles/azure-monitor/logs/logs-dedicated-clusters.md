---
title: Azure Izleyici, ayrılmış kümeleri günlüğe kaydeder
description: 1 TB 'den fazla veri izleme verisi alan müşteriler, paylaşılan kümeler yerine adanmış olarak kullanılabilir
ms.subservice: logs
ms.topic: conceptual
author: rboucher
ms.author: robb
ms.date: 09/16/2020
ms.openlocfilehash: fe7bd4b9f800b59d2c16d4aa3dadd3626c55b7e1
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101707651"
---
# <a name="azure-monitor-logs-dedicated-clusters"></a>Azure Izleyici, ayrılmış kümeleri günlüğe kaydeder

Azure Izleyici günlükleri adanmış kümeler, Azure Izleyici günlükleri müşterilerine yönelik gelişmiş özellikleri sağlayan bir dağıtım seçeneğidir. Adanmış kümelere sahip müşteriler bu kümelerde barındırılacak çalışma alanlarını seçebilirler.

Adanmış kümeler gerektiren yetenekler şunlardır:

- **[Müşteri tarafından yönetilen anahtarlar](../logs/customer-managed-keys.md)** -müşteri tarafından sunulan ve denetlenen anahtarları kullanarak küme verilerini şifreleyin.
- **[Kasa](../logs/customer-managed-keys.md#customer-lockbox-preview)** -müşteriler, Microsoft Destek mühendislerinin verilere yönelik erişim isteklerini denetleyebilir.
- **[Çift şifreleme](../../storage/common/storage-service-encryption.md#doubly-encrypt-data-with-infrastructure-encryption)** , şifreleme algoritmalarından veya anahtarlardan birinin tehlikeye girdiği bir senaryoya karşı koruma sağlar. Bu durumda, ek şifreleme katmanı verilerinizi korumaya devam eder.
- **[Çoklu çalışma alanı](../logs/cross-workspace-query.md)** -bir müşteri üretim için birden fazla çalışma alanı kullanıyorsa, adanmış kümeyi kullanmak mantıklı olabilir. Tüm çalışma alanları aynı kümede ise, çoklu çalışma alanları sorguları daha hızlı çalışır. Atanan kapasite rezervasyon katmanları tüm küme alımını hesaba alcağından ve bu, bazıları küçük ve kapasite rezervasyon indirimlerine uygun olmasa bile, tüm çalışma alanları için geçerli olan adanmış kümeyi kullanmak daha uygun maliyetli olabilir.

Adanmış kümeler, müşterilerin gün başına en az 1 TB veri alma kapasitesi kullanarak işlemesini gerektirir. Adanmış bir kümeye geçiş basittir. Veri kaybı veya hizmet kesintisi yok. 

## <a name="management"></a>Yönetim 

Adanmış kümeler, Azure Izleyici günlük kümelerini temsil eden bir Azure kaynağı aracılığıyla yönetilir. Tüm işlemler bu kaynakta PowerShell veya REST API kullanılarak yapılır.

Küme oluşturulduktan sonra, yapılandırılabilir ve onunla bağlantılı çalışma alanları bulunabilir. Bir çalışma alanı bir kümeye bağlandığında, çalışma alanına gönderilen yeni veriler kümede bulunur. Yalnızca kümeyle aynı bölgedeki çalışma alanları kümeyle bağlantılı olabilir. Çalışma alanları, bazı sınırlamalar içeren bir kümeden beğenilmiş olabilir. Bu sınırlamalar hakkında daha fazla ayrıntı bu makaleye dahildir. 

Adanmış kümelere alınan veriler iki kez şifrelenir: Microsoft tarafından yönetilen anahtarlar veya [müşteri tarafından yönetilen anahtar](../logs/customer-managed-keys.md)kullanılarak hizmet düzeyinde bir kez ve altyapı düzeyinde iki farklı şifreleme algoritması ve iki farklı anahtar kullanan bir kez. [Çift şifreleme](../../storage/common/storage-service-encryption.md#doubly-encrypt-data-with-infrastructure-encryption) , şifreleme algoritmalarından veya anahtarlardan birinin tehlikeye girdiği bir senaryoya karşı koruma sağlar. Bu durumda, ek şifreleme katmanı verilerinizi korumaya devam eder. Adanmış küme ayrıca verilerinizi [kasa](../logs/customer-managed-keys.md#customer-lockbox-preview) denetimiyle korumanıza olanak sağlar.

Küme düzeyindeki tüm işlemler `Microsoft.OperationalInsights/clusters/write` kümede eylem iznini gerektirir. Bu izin, eylemi içeren sahip veya katkıda bulunan veya `*/write` eylemi içeren Log Analytics katkıda bulunan rolü aracılığıyla verilebilir `Microsoft.OperationalInsights/*` . Log Analytics izinler hakkında daha fazla bilgi için bkz. [Azure izleyici 'de günlük verilerine ve çalışma alanlarına erişimi yönetme](./manage-access.md). 


## <a name="cluster-pricing-model"></a>Küme fiyatlandırma modeli

Log Analytics adanmış kümeler, en az 1000 GB/gün olan bir kapasite ayırma fiyatlandırma modeli kullanır. Rezervasyon düzeyinin üzerindeki tüm kullanımlar, Kullandıkça Öde fiyatı üzerinden faturalandırılır.  Kapasite ayırma fiyatlandırma bilgileri, [Azure izleyici fiyatlandırma sayfasında]( https://azure.microsoft.com/pricing/details/monitor/)bulunabilir.  

Küme kapasitesi ayırma düzeyi, altındaki parametresi kullanılarak Azure Resource Manager aracılığıyla program aracılığıyla yapılandırılır `Capacity` `Sku` . `Capacity`GB cinsinden belirtilir ve 100 GB/gün artışlarla 1000 GB veya daha fazla değere sahip olabilir.

Bir kümede kullanıma yönelik iki faturalandırma modu vardır. Bu, `billingType` kümeniz yapılandırılırken parametresi tarafından belirtilebilir. 

1. **Küme**: Bu durumda (varsayılan), alınan verilerin faturalandırılması küme düzeyinde yapılır. Kümeyle ilişkilendirilen her çalışma alanından alınan veri miktarları, küme için günlük faturanızı hesaplamak üzere toplanır. 

2. **Çalışma alanları**: kümenizin kapasite ayırma maliyeti, kümedeki çalışma alanlarıyla orantılı olarak atanır (her çalışma alanı Için [Azure Güvenlik Merkezi](../../security-center/index.yml) 'nden düğüm başına ayırmalar için hesap oluşturulduktan sonra).

Çalışma alanınız, düğüm başına eski fiyatlandırma katmanını kullanıyorsa, bir kümeye bağlandığında kümenin kapasite rezervasyonuna ve düğüm başına artık olmayan verilere göre faturalandırılır. Azure Güvenlik Merkezi 'nden düğüm başına veri ayırmaları uygulanmasına devam edilecek.

Log Analytics adanmış kümeler için daha fazla ayrıntı faturalandırılır. []( https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#log-analytics-dedicated-clusters)

## <a name="asynchronous-operations-and-status-check"></a>Zaman uyumsuz işlemler ve durum denetimi

Yapılandırma adımlarının bazıları hızla tamamlanamadığından zaman uyumsuz olarak çalışır. Yanıt ' daki durum, şu hata kodu dahil olmak üzere, ' InProgress ', ' Updating ', ' siliyor ', ' SUCCEEDED veya ' Failed ' olabilir. REST kullanılırken, yanıt başlangıçta bir HTTP durum kodu 202 (kabul edildi) ve üst bilgi Azure-AsyncOperation özelliğini döndürür:

```JSON
"Azure-AsyncOperation": "https://management.azure.com/subscriptions/subscription-id/providers/Microsoft.OperationalInsights/locations/region-name/operationStatuses/operation-id?api-version=2020-08-01"
```

Azure-AsyncOperation üst bilgi değerine bir GET isteği göndererek zaman uyumsuz işlemin durumunu kontrol edebilirsiniz:

```rst
GET https://management.azure.com/subscriptions/subscription-id/providers/microsoft.operationalInsights/locations/region-name/operationstatuses/operation-id?api-version=2020-08-01
Authorization: Bearer <token>
```

## <a name="creating-a-cluster"></a>Küme oluşturma

Önce adanmış bir küme oluşturmaya başlamak için küme kaynakları oluşturursunuz.

Aşağıdaki özellikler belirtilmelidir:

- **Clustername**: yönetimsel amaçlar için kullanılır. Kullanıcılar bu ada gösterilmez.
- **Resourcegroupname**: tüm Azure kaynakları için, kümeler bir kaynak grubuna aittir. Kümeler genellikle kuruluştaki birçok ekip tarafından paylaşıldığından, merkezi bir BT kaynak grubu kullanmanızı öneririz. Daha fazla tasarım konusunda dikkat edilmesi gereken noktalar için [Azure Izleyici günlüklerinin dağıtımını tasarlama](../logs/design-logs-deployment.md) konusunu inceleyin
- **Konum**: bir küme, belirli bir Azure bölgesinde bulunur. Yalnızca bu bölgede bulunan çalışma alanları bu kümeyle bağlantılı olabilir.
- **Skucapacity**: bir *küme* kaynağı oluştururken *Kapasite ayırma* düzeyini (SKU) belirtmeniz gerekir. *Kapasite ayırma* DÜZEYI 1.000 gb Ila 3.000 GB arasında olabilir. Bunu, daha sonra gerekirse 100 adımlarında güncelleştirebilirsiniz. Gün başına 3.000 GB 'den yüksek kapasite ayırma düzeyine ihtiyacınız varsa, adresinden bizimle iletişime geçin LAIngestionRate@microsoft.com . Küme maliyetleri hakkında daha fazla bilgi için bkz. [Log Analytics kümeleri Için maliyetleri yönetme](./manage-cost-storage.md#log-analytics-dedicated-clusters)

*Küme* kaynağını oluşturduktan sonra, *SKU*, * Keyvaultproperties veya *billingtype* gibi ek özellikleri düzenleyebilirsiniz. Daha fazla ayrıntı için aşağıya bakın.

Her bölge için abonelik başına en fazla 2 etkin kümeniz olabilir. Küme silinirse, hala 14 gün için ayrılmıştır. Her bölge için abonelik başına en fazla 4 ayrılmış kümeniz olabilir (etkin veya son silinen).

> [!WARNING]
> Küme oluşturma işlemi kaynak ayırmayı ve sağlamayı tetikler. Bu işlemin tamamlanması birkaç saat sürebilir. Bunu zaman uyumsuz olarak çalıştırmanız önerilir.

Kümeleri oluşturan kullanıcı hesabı, standart Azure kaynak oluşturma iznine sahip olmalıdır: `Microsoft.Resources/deployments/*` ve `Microsoft.OperationalInsights/clusters/write` rol atamalarında bu belirli eylem ya da veya ya da buna sahip olarak küme yazma izni olmalıdır `Microsoft.OperationalInsights/*` `*/write` .

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
PUT https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters/<cluster-name>?api-version=2020-08-01
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

202 (kabul edildi) ve bir üst bilgi olmalıdır.

### <a name="check-cluster-provisioning-status"></a>Küme sağlama durumunu denetleme

Log Analytics kümesinin sağlanması bir süre sürer. Sağlama durumunu çeşitli yollarla kontrol edebilirsiniz:

- Get-AzOperationalInsightsCluster PowerShell komutunu kaynak grubu adıyla çalıştırın ve ProvisioningState özelliğini denetleyin. Bu değer, sağlama sırasında *Provisioningaccount* ve tamamlandığında *başarılı* olur.
  ```powershell
  New-AzOperationalInsightsCluster -ResourceGroupName {resource-group-name} 
  ```

- Yanıttan Azure-AsyncOperation URL değerini kopyalayın ve zaman uyumsuz işlemler durum denetimini izleyin.

- *Küme* KAYNAĞıNDA bir get isteği gönderin ve *provisioningstate* değerine bakın. Bu değer, sağlama sırasında *Provisioningaccount* ve tamamlandığında *başarılı* olur.

   ```rst
   GET https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters/<cluster-name>?api-version=2020-08-01
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

## <a name="link-a-workspace-to-cluster"></a>Çalışma alanını kümeye bağlama

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
PUT https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/workspaces/<workspace-name>/linkedservices/cluster?api-version=2020-08-01 
Authorization: Bearer <token>
Content-type: application/json

{
  "properties": {
    "WriteAccessResourceId": "/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/clusters/<cluster-name>"
    }
}
```

*Response*

202 (kabul edildi) ve üst bilgi.

### <a name="check-workspace-link-status"></a>Çalışma alanı bağlantı durumunu denetle
  
Müşteri tarafından yönetilen anahtarlar kullanıyorsanız, alınan veriler ilişkilendirme işleminden sonra yönetilen anahtarınızla şifrelenmiş olarak depolanır ve bu da tamamlanması 90 dakika sürebilir. 

Çalışma alanı ilişkilendirme durumunu iki şekilde kontrol edebilirsiniz:

- Yanıttan Azure-AsyncOperation URL değerini kopyalayın ve zaman uyumsuz işlemler durum denetimini izleyin.

- Çalışma alanında Get işlemini gerçekleştirin ve *Kümeresourceıd* özelliğinin *Özellikler* altında yanıtta bulunup bulunmayacağını gözlemleyin.

**CLI**

```azurecli
az monitor log-analytics cluster show --resource-group "resource-group-name" --name "cluster-name"
```

**PowerShell**

```powershell
Get-AzOperationalInsightsWorkspace -ResourceGroupName "resource-group-name" -Name "workspace-name"
```

**REST**

*Call*

```rest
GET https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/workspaces/<workspace-name>?api-version=2020-08-01
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

## <a name="change-cluster-properties"></a>Küme özelliklerini değiştirme

*Küme* kaynağınızı oluşturduktan ve tam olarak sağlandıktan sonra, PowerShell veya REST API kullanarak küme düzeyindeki ek özellikleri düzenleyebilirsiniz. Küme oluşturma sırasında kullanılabilen özellikler dışında, ek özellikler yalnızca küme sağlandıktan sonra ayarlanabilir:

- **Keyvaultproperties** -Azure Key Vault anahtarı güncelleştirir. Bkz. [anahtar tanımlayıcı ayrıntıları ile küme güncelleştirme](../logs/customer-managed-keys.md#update-cluster-with-key-identifier-details). Şu parametreleri içerir: *Keyvaulturi*, *KeyName*, *keyversion*. 
- **billingtype** - *billingtype* özelliği *küme* kaynağı ve verileri için faturalandırma atışmasını belirler:
  - **Küme** (varsayılan)-kümeniz Için kapasite ayırma maliyetleri, *küme* kaynağına atanır.
  - **Çalışma alanları** -bu, günün Toplam alınan verileri kapasite rezervasyonunun altındaysa *, kümeniz için* kapasite ayırma maliyetleri kümedeki çalışma alanlarıyla orantılı olarak atanır. Küme fiyatlandırma modeli hakkında daha fazla bilgi edinmek için bkz. [Log Analytics adanmış kümeler](./manage-cost-storage.md#log-analytics-dedicated-clusters) . 

> [!NOTE]
> *Billingtype* özelliği PowerShell 'de desteklenmez.

### <a name="get-all-clusters-in-resource-group"></a>Kaynak grubundaki tüm kümeleri al
  
**CLI**

```azurecli
az monitor log-analytics cluster list --resource-group "resource-group-name"
```

**PowerShell**

```powershell
Get-AzOperationalInsightsCluster -ResourceGroupName "resource-group-name"
```

**REST**

*Call*

  ```rst
  GET https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters?api-version=2020-08-01
  Authorization: Bearer <token>
  ```

*Response*
  
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

### <a name="get-all-clusters-in-subscription"></a>Abonelikteki tüm kümeleri al

**CLI**

```azurecli
az monitor log-analytics cluster list
```

**PowerShell**

```powershell
Get-AzOperationalInsightsCluster
```

**REST**

*Call*

```rst
GET https://management.azure.com/subscriptions/<subscription-id>/providers/Microsoft.OperationalInsights/clusters?api-version=2020-08-01
Authorization: Bearer <token>
```
    
*Response*
    
' Kaynak grubundaki kümeler ', ancak abonelik kapsamında.



### <a name="update-capacity-reservation-in-cluster"></a>Kümede kapasite ayırmayı Güncelleştir

Bağlı çalışma alanlarınızdaki veri hacmi zaman içinde değişiklik yaparken ve kapasite ayırma düzeyini uygun şekilde güncellemek istediğinizde. Kapasite GB birimleri cinsinden belirtilir ve 100 GB/gün artışlarla 1000 GB veya daha fazla değere sahip olabilir. Tam REST istek gövdesini sağlamanız gerekmez, ancak SKU 'yu içermelidir.

**CLI**

```azurecli
az monitor log-analytics cluster update --name "cluster-name" --resource-group "resource-group-name" --sku-capacity 1000
```

**PowerShell**

```powershell
Update-AzOperationalInsightsCluster -ResourceGroupName "resource-group-name" -ClusterName "cluster-name" -SkuCapacity 1000
```

**REST**

*Call*

  ```rst
  PATCH https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters/<cluster-name>?api-version=2020-08-01
  Authorization: Bearer <token>
  Content-type: application/json

  {
    "sku": {
      "name": "capacityReservation",
      "Capacity": 2000
    }
  }
  ```

### <a name="update-billingtype-in-cluster"></a>Kümede billingType 'ı güncelleştirme

*Billingtype* özelliği, küme ve verileri için faturalandırma atısyonu belirler:
- *küme* (varsayılan)--Faturalandırma, küme kaynağınızı barındıran abonelikle ilişkilidir
- *çalışma alanları* --faturalandırma, çalışma alanlarınızı orantılı olarak barındıran aboneliklerle ilişkilidir

**REST**

*Call*

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

### <a name="unlink-a-workspace-from-cluster"></a>Çalışma alanının kümeden bağlantısını kaldırma

Bir çalışma alanının bir kümeden bağlantısını kaldırabilirsiniz. Bir çalışma alanının kümeden bağlantısını kaldırdıktan sonra, bu çalışma alanıyla ilişkili yeni veriler adanmış kümeye gönderilmez. Ayrıca, çalışma alanı faturalandırması artık küme aracılığıyla yapılmaz. Bağlantısız çalışma alanının eski verileri kümede kalabilir. Bu veriler, müşteri tarafından yönetilen anahtarlar (CMK) kullanılarak şifrelenirse Key Vault gizli dizileri tutulur. Sistem, bu değişikliği Log Analytics kullanıcılarından soyutlar. Kullanıcılar çalışma alanını her zamanki gibi sorgulayabilir. Sistem, kullanıcılara hiçbir belirtime gerek olmadan arka uçta çapraz küme sorguları gerçekleştirir.  

> [!WARNING] 
> Bir ay içindeki belirli bir çalışma alanı için iki bağlama işlemi sınırlıdır. İşlem kaldırma eylemlerini uygun şekilde göz önünde bulundurmanız ve planlamanız zaman alın.

**CLI**

```azurecli
az monitor log-analytics workspace linked-service delete --resource-group "resource-group-name" --workspace-name "MyWorkspace" --name cluster
```

**PowerShell**

Bir çalışma alanının kümeden bağlantısını kaldırmak için aşağıdaki PowerShell komutunu kullanın:

```powershell
# Unlink a workspace from cluster
Remove-AzOperationalInsightsLinkedService -ResourceGroupName {resource-group-name} -WorkspaceName {workspace-name} -LinkedServiceName cluster
```

### <a name="delete-cluster"></a>Kümeyi silme

Ayrılmış bir küme kaynağı silinebilir. Silmeden önce, tüm çalışma alanlarının kümeden bağlantısını kaldırmanız gerekir. Bu işlemi gerçekleştirmek için *küme* kaynağında ' yazma ' izinlerine sahip olmanız gerekir. 

Küme kaynağı silindikten sonra, fiziksel küme bir temizleme ve silme işlemi girer. Küme silindiğinde kümede depolanan tüm veriler silinir. Veriler geçmişte kümeyle bağlantılı çalışma alanlarından olabilir.

Son 14 gün içinde silinen bir *küme* kaynağı, geçici silme durumunda ve verileriyle kurtarılabilir. *Küme* kaynağı silme işlemi ile tüm çalışma alanları *küme* kaynağı ile ilişkili olduğundan, çalışma alanlarınızı kurtarma sonrasında yeniden ilişkilendirmeniz gerekir. Kurtarma işlemi, Kullanıcı tarafından Microsoft kanalınıza veya kurtarma isteklerine yönelik desteğe başvurarak gerçekleştirilemiyor.

Silinmeden sonra 14 gün içinde, küme kaynak adı ayrılmıştır ve diğer kaynaklar tarafından kullanılamaz.

> [!WARNING] 
> Abonelik başına üç küme sınırı vardır. Hem etkin hem de geçici olarak silinen kümeler bunun bir parçası olarak sayılır. Müşteriler küme oluşturup silen yinelenen yordamlar oluşturmamalıdır. Log Analytics arka uç sistemlerinde önemli bir etkisi vardır.

**PowerShell**

Kümeyi silmek için aşağıdaki PowerShell komutunu kullanın:

  ```powershell
  Remove-AzOperationalInsightsCluster -ResourceGroupName "resource-group-name" -ClusterName "cluster-name"
  ```

**REST**

Kümeyi silmek için aşağıdaki REST çağrısını kullanın:

  ```rst
  DELETE https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters/<cluster-name>?api-version=2020-08-01
  Authorization: Bearer <token>
  ```

  **Response**

  200 TAMAM

## <a name="limits-and-constraints"></a>Sınırlar ve kısıtlamalar

- Bölge ve abonelik başına en fazla etkin küme sayısı 2 ' dir

- Her bölge ve abonelik için en fazla ayrılmış küme (etkin veya son silinen) sayısı 4 ' ün 

- Kümeye en fazla bağlantılı çalışma alanı 1000

- Bir çalışma alanını kümenize bağlayabilir ve sonra bağlantısını kesebilirsiniz. Belirli çalışma alanındaki çalışma alanı bağlantı işlemlerinin sayısı, 30 günlük bir dönemde 2 ile sınırlıdır.

- Küme başka bir kaynak grubuna veya aboneliğe taşıma Şu anda desteklenmiyor.

- Kasa Çin 'de Şu anda kullanılamıyor. 

- Desteklenen bölgelerde, Ekim 2020 ' den oluşturulan kümeler için [Çift şifreleme](../../storage/common/storage-service-encryption.md#doubly-encrypt-data-with-infrastructure-encryption) otomatik olarak yapılandırılır. Kümede bir GET isteği göndererek ve `isDoubleEncryptionEnabled` değerin `true` çift şifrelemeyi etkin kümeler için olduğunu gözlemleyerek, kümenizin Çift şifreleme için yapılandırılıp yapılandırılmadığını doğrulayabilirsiniz. 
  - Bir küme oluşturur ve "<Region-adı> kümeler için çift şifrelemeyi desteklemez.", REST istek gövdesine ekleyerek kümeyi, Çift şifrelemeyi oluşturmaya devam edebilirsiniz `"properties": {"isDoubleEncryptionEnabled": false}` .
  - Küme oluşturulduktan sonra çift şifreleme ayarı değiştirilemez.

## <a name="troubleshooting"></a>Sorun giderme

- Bir küme oluştururken çakışma hatası alırsanız, kümenizi son 14 gün içinde silmiş ve bu da geçici silme durumunda olabilir. Küme adı, geçici silme döneminde ayrılmış kalır ve bu adla yeni bir küme oluşturamazsınız. Bu ad, küme kalıcı olarak silindiğinde geçici silme süresinden sonra serbest bırakılır.

- Küme sağlama veya güncelleştirme durumundayken kümenizi güncelleştirirseniz güncelleştirme başarısız olur.

- Bazı işlemler uzun sürer ve işlemin tamamlanması biraz zaman alabilir; bunlar küme oluşturma, küme anahtarı güncelleştirme ve küme silme işlemlerini gerçekleştirebilir. İşlem durumunu iki şekilde denetleyebilirsiniz:
  - REST kullanırken, Azure-AsyncOperation URL değerini yanıttan kopyalayın ve [zaman uyumsuz işlemler durum denetimini](#asynchronous-operations-and-status-check)izleyin.
  - Kümeye veya çalışma alanına GET isteği gönderin ve yanıtı gözlemleyin. Örneğin, bağlantısız çalışma alanının *Özellikler* bölümünde *kümeresourceıd* yok.

- Başka bir kümeyle bağlantı varsa, kümeye çalışma alanı bağlantısı başarısız olur.

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
  -  400--KeyVaultProperties boş değil, ancak biçimi hatalı. Bkz. [anahtar tanımlayıcısı güncelleştirmesi](../logs/customer-managed-keys.md#update-cluster-with-key-identifier-details).
  -  400--Key Vault anahtarı doğrulanamadı. İzin eksikliği veya anahtar bulunmadığı için olabilir. Key Vault ' de [anahtar ve erişim ilkesini ayarlamış](../logs/customer-managed-keys.md#grant-key-vault-permissions) olduğunuzdan emin olun.
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

## <a name="next-steps"></a>Sonraki adımlar

- [Log Analytics adanmış küme faturalaması](./manage-cost-storage.md#log-analytics-dedicated-clusters) hakkında bilgi edinin
- [Log Analytics çalışma alanlarının doğru tasarımı](../logs/design-logs-deployment.md) hakkında bilgi edinin