---
title: Azure Cosmos DB'de tanılama günlüğüne kaydetme
description: Azure Cosmos DB depolanan verileri günlüğe kaydetmek ve izlemek için kullanabileceğiniz farklı yollar hakkında bilgi edinin.
author: SnehaGunda
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: sngun
ms.custom: seodec18
ms.openlocfilehash: bdbc50983708327cf5d3857282c92fcab1c28b09
ms.sourcegitcommit: a10074461cf112a00fec7e14ba700435173cd3ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2019
ms.locfileid: "73930541"
---
# <a name="diagnostic-logging-in-azure-cosmos-db"></a>Azure Cosmos DB'de tanılama günlüğüne kaydetme 

Bir veya daha fazla Azure Cosmos veritabanını kullanmaya başladıktan sonra, veritabanlarınıza nasıl ve ne zaman erişildiğini izlemek isteyebilirsiniz. Bu makalede, Azure platformunda bulunan günlüklere genel bir bakış sunulmaktadır. [Azure depolama](https://azure.microsoft.com/services/storage/)'ya Günlükler göndermek, [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/)günlüklerin akışını sağlamak ve günlükleri [Azure izleyici günlüklerine](https://azure.microsoft.com/services/log-analytics/)aktarmak için izleme amacıyla tanılama günlüğünü nasıl etkinleştireceğinizi öğreneceksiniz.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="logs-available-in-azure"></a>Azure 'da kullanılabilir Günlükler

Azure Cosmos DB hesabınızı nasıl izleyeceğinizi anlamadan önce, günlüğe kaydetme ve izleme hakkında birkaç şeyi açıklığa kavuşturalım. Azure platformunda farklı türlerde Günlükler vardır. [Azure etkinlik günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs), [Azure tanılama günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs), [Azure ölçümleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-metrics), olayları, sinyal izleme, işlem günlükleri vb. vardır. Günlüklerin bir plethora vardır. Günlüklerin tüm listesini Azure portal [Azure izleyici günlüklerinde](https://azure.microsoft.com/services/log-analytics/) bulabilirsiniz. 

Aşağıdaki görüntüde, kullanılabilir farklı Azure günlükleri türü gösterilmektedir:

![Farklı türlerde Azure günlükleri](./media/logging/azurelogging.png)

Görüntüde, **işlem kaynakları** Microsoft Konuk işletim sistemine erişebileceğiniz Azure kaynaklarını temsil eder. Örneğin, Azure sanal makineleri, sanal makine ölçek kümeleri, Azure Container Service vb., işlem kaynakları olarak kabul edilir. İşlem kaynakları, etkinlik günlükleri, tanılama günlükleri ve uygulama günlükleri oluşturur. Daha fazla bilgi edinmek için [Azure 'da izleme verileri kaynakları](../azure-monitor/platform/data-sources.md) makalesine bakın.

**Işlem dışı kaynaklar** , temel alınan işletim sistemine erişemeyen ve kaynakla doğrudan çalışan kaynaklardır. Örneğin, ağ güvenlik grupları, Logic Apps vb. Azure Cosmos DB işlem olmayan bir kaynaktır. İşlem dışı kaynakların günlüklerini etkinlik günlüğünde görüntüleyebilir veya portalda tanılama günlükleri seçeneğini etkinleştirebilirsiniz. Daha fazla bilgi edinmek için [Azure izleyici 'de veri kaynakları](../azure-monitor/platform/data-sources.md) makalesine başvurun.

Etkinlik günlüğü, işlemleri Azure Cosmos DB için bir abonelik düzeyinde kaydeder. ListKeys, yazma DatabaseAccounts ve daha fazlası gibi işlemler günlüğe kaydedilir. Tanılama günlükleri daha ayrıntılı günlük kaydı sağlar ve DataPlaneRequests (oluşturma, okuma, sorgulama vb.) ve MongoRequests 'i günlüğe kaydetbilmenizi sağlar.


Bu makalede, Azure etkinlik günlüğü, Azure tanılama günlükleri ve Azure ölçümlerine odaklanıyoruz. Bu üç günlük arasındaki fark nedir? 

### <a name="azure-activity-log"></a>Azure Etkinlik Günlüğü

Azure etkinlik günlüğü, Azure 'da oluşan abonelik düzeyindeki olaylara ilişkin Öngörüler sağlayan bir abonelik günlüğü. Etkinlik günlüğü, yönetim kategorisinin altında abonelikleriniz için denetim düzlemi olaylarını raporlar. Abonelik günlüğünü, aboneliğinizdeki kaynaklar üzerinde herhangi bir yazma işlemi için "ne, kim ve ne zaman" belirlemek için kullanabilirsiniz. Ayrıca, işlemin durumunu ve ilgili diğer özellikleri de anlayabilirsiniz. 

Etkinlik günlüğü, tanılama günlüklerinden farklıdır. Etkinlik günlüğü, dışarıdaki ( _Denetim düzlemi_) bir kaynaktaki işlemlerle ilgili veriler sağlar. Azure Cosmos DB bağlamında, denetim düzlemi işlemleri kapsayıcı oluşturma, anahtar listeleme, anahtar silme, veritabanı listeleme vb. içerir. Tanılama günlükleri bir kaynak tarafından dağıtılır ve bu kaynağın ( _veri düzlemi_) işlemi hakkında bilgiler sağlar. Tanılama günlüğündeki veri düzlemi işlemlerine bazı örnekler, DELETE, INSERT ve ReadFeed.

Etkinlik günlükleri (denetim düzlemi işlemleri) doğası gereği daha zengin olabilir ve arayanın, çağıran IP adresinin, kaynak adının, işlem adının, Tenantıd 'nin ve daha pek çok e-posta adresini içerebilir. Etkinlik günlüğü birçok veri [kategorisi](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-activity-log-schema) içerir. Bu kategorilerin şemaların serileştirilmesi hakkında tam Ayrıntılar için bkz. [Azure etkinlik günlüğü olay şeması](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-activity-log-schema). Ancak, kişisel veriler genellikle bu günlüklerden çıkarılır ve tanılama günlükleri doğası gereği kısıtlayıcı olabilir. Çağıranın IP adresine sahip olabilirsiniz, ancak en son OCDE kaldırılır.

### <a name="azure-metrics"></a>Azure ölçümleri

Azure [ölçümleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-metrics) , çoğu Azure kaynağı tarafından yayınlanan en önemli Azure telemetri verileri türüne ( _performans sayaçları_da denir) sahiptir. Ölçümler aktarım hızı, depolama, tutarlılık, kullanılabilirlik ve Azure Cosmos DB kaynaklarınızın gecikmesi hakkındaki bilgileri görüntülemenize olanak sağlar. Daha fazla bilgi için bkz. [Azure Cosmos DB ölçümlerle izleme ve hata ayıklama](use-metrics.md).

### <a name="azure-diagnostic-logs"></a>Azure tanılama günlükleri

Azure tanılama günlükleri bir kaynak tarafından dağıtılır ve bu kaynağın çalışması hakkında zengin ve sık veriler sağlar. Bu Günlükler istek başına yakalanır. Bu günlüklerin içeriği kaynak türüne göre değişir. Kaynak düzeyi tanılama günlükleri, Konuk işletim sistemi düzeyi tanılama günlüklerinden da farklıdır. Konuk işletim sistemi tanılama günlükleri, bir sanal makine veya desteklenen başka bir kaynak türü içinde çalışan bir aracı tarafından toplanır. Kaynak düzeyinde tanılama günlükleri, hiçbir aracı gerektirmez ve Azure platformunun kendisinden kaynağa özgü verileri yakalayamaz. Konuk IŞLETIM sistemi düzeyi tanılama günlükleri, bir sanal makinede çalışan işletim sisteminden ve uygulamalardan verileri yakalar.

![Depolama, Event Hubs veya Azure Izleyici günlüklerine tanılama günlüğü](./media/logging/azure-cosmos-db-logging-overview.png)

### <a name="what-is-logged-by-azure-diagnostic-logs"></a>Azure tanılama günlükleri tarafından günlüğe kaydedilen nedir?

* Erişim izinlerinin, sistem hatalarının veya hatalı isteklerin sonucu olarak, başarısız istekler dahil tüm API 'lerde tüm kimlik doğrulamalı arka uç istekleri (TCP/REST) günlüğe kaydedilir. Kullanıcı tarafından başlatılan Graph, Cassandra ve Tablo API'si istekleri için destek şu anda kullanılamıyor.
* Tüm belgeleri, kapsayıcılar ve veritabanları CRUD işlemleri içeren işlemler veritabanında kendisi.
* Anahtar oluşturmayı, değiştirmeyi veya silmeyi de kapsayan hesap anahtarları üzerinde işlemler.
* Bir 401 yanıtına neden olan kimliği doğrulanmamış istekler. Örneğin, bir taşıyıcı belirteci olmayan veya hatalı biçimlendirilmiş ya da geçerliliği olmayan ya da geçersiz bir belirtece sahip olan istekler.

<a id="#turn-on"></a>
## <a name="turn-on-logging-in-the-azure-portal"></a>Azure portal günlüğü açma

Azure portal tanılama günlüğünü etkinleştirmek için aşağıdaki adımları kullanın:

1. [Azure portalında](https://portal.azure.com) oturum açın. 

1. Azure Cosmos hesabınıza gidin. **Tanılama ayarları** bölmesini açın ve ardından **Tanılama ayarı Ekle** seçeneğini belirleyin.

    ![Azure portal Azure Cosmos DB için tanılama günlüğünü açın](./media/logging/turn-on-portal-logging.png)

1. **Tanılama ayarları** sayfasında, formu aşağıdaki ayrıntılarla doldurabilirsiniz: 

    * **Ad**: oluşturulacak Günlükler için bir ad girin.

    * Günlükleri şu hizmetlere kaydedebilirsiniz:

      * **Bir depolama hesabına arşivleme**: Bu seçeneği kullanmak için, bağlanmak için mevcut bir depolama hesabınızın olması gerekir. Portalda yeni bir depolama hesabı oluşturmak için bkz. [depolama hesabı oluşturma](../storage/common/storage-create-storage-account.md) makalesi. Ardından, depolama hesabınızı seçmek için portalda Azure Cosmos DB Tanılama ayarları bölmesine dönün. Yeni oluşturulan depolama hesaplarının açılan menüde görünmesi birkaç dakika sürebilir.

      * **Bir olay hub 'ına akış**: Bu seçeneği kullanmak için, mevcut bir Event Hubs ad alanına ve bağlanmak üzere Olay Hub 'ına ihtiyacınız vardır. Event Hubs bir ad alanı oluşturmak için, bkz. [Azure Portal kullanarak Event Hubs ad alanı ve Olay Hub 'ı oluşturma](../event-hubs/event-hubs-create.md). Ardından, Olay Hub 'ı ad alanını ve ilke adını seçmek için portalda bu sayfaya geri dönün.

      * **Log Analytics gönder**: Bu seçeneği kullanmak için, portalda [Yeni bir çalışma alanı oluşturma](../azure-monitor/learn/quick-collect-azurevm.md#create-a-workspace) adımlarını izleyerek mevcut bir çalışma alanını kullanın veya yeni bir Log Analytics çalışma alanı oluşturun. 

   * Aşağıdaki verileri günlüğe kaydedebilirsiniz:

      * **Dataplanerequests**: Azure Cosmos DB içindeki SQL, Graph, MongoDB, Cassandra ve tablo API'si hesaplarını Içeren tüm API 'lere arka uç isteklerini günlüğe kaydetmek için bu seçeneği belirleyin. Bir depolama hesabına arşivleme yapıyorsanız, tanılama günlükleri için bekletme dönemini seçebilirsiniz. Günlükler, saklama süresi dolduktan sonra otomatik olarak silinir. Aşağıdaki JSON verileri, DataPlaneRequests kullanılarak günlüğe kaydedilen ayrıntıların örnek çıktılarından oluşur. Önemli özellikler: Requestücret, statusCode, ClientIpAddress ve PartitionID:

       ```
       { "time": "2019-04-23T23:12:52.3814846Z", "resourceId": "/SUBSCRIPTIONS/<your_subscription_ID>/RESOURCEGROUPS/<your_resource_group>/PROVIDERS/MICROSOFT.DOCUMENTDB/DATABASEACCOUNTS/<your_database_account>", "category": "DataPlaneRequests", "operationName": "ReadFeed", "properties": {"activityId": "66a0c647-af38-4b8d-a92a-c48a805d6460","requestResourceType": "Database","requestResourceId": "","collectionRid": "","statusCode": "200","duration": "0","userAgent": "Microsoft.Azure.Documents.Common/2.2.0.0","clientIpAddress": "10.0.0.24","requestCharge": "1.000000","requestLength": "0","responseLength": "372","resourceTokenUserRid": "","region": "East US","partitionId": "062abe3e-de63-4aa5-b9de-4a77119c59f8","keyType": "PrimaryReadOnlyMasterKey","databaseName": "","collectionName": ""}}
       ```

      * **Mongorequests**: Ön uçtaki kullanıcı tarafından başlatılan istekleri günlüğe kaydetmek için bu seçeneği belirleyin. Bu seçenek, Istekleri MongoDB IÇIN Azure Cosmos DB API 'sine istemcilere sunar. MongoDB istekleri, MongoRequests ve DataPlaneRequests içinde görünür. Bir depolama hesabına arşivleme yapıyorsanız, tanılama günlükleri için bekletme dönemini seçebilirsiniz. Günlükler, saklama süresi dolduktan sonra otomatik olarak silinir. Aşağıdaki JSON verileri, MongoRequests kullanılarak günlüğe kaydedilen ayrıntıların örnek çıktılarından oluşur. Önemli özellikler: Requestücret, opCode:

       ```
       { "time": "2019-04-10T15:10:46.7820998Z", "resourceId": "/SUBSCRIPTIONS/<your_subscription_ID>/RESOURCEGROUPS/<your_resource_group>/PROVIDERS/MICROSOFT.DOCUMENTDB/DATABASEACCOUNTS/<your_database_account>", "category": "MongoRequests", "operationName": "ping", "properties": {"activityId": "823cae64-0000-0000-0000-000000000000","opCode": "MongoOpCode_OP_QUERY","errorCode": "0","duration": "0","requestCharge": "0.000000","databaseName": "admin","collectionName": "$cmd","retryCount": "0"}}
       ```

      * **QueryRuntimeStatistics**: yürütülen sorgu metnini günlüğe kaydetmek için bu seçeneği belirleyin.  Aşağıdaki JSON verileri, QueryRuntimeStatistics kullanarak günlüğe kaydedilen ayrıntıların örnek çıktılarından oluşur:

       ```
       { "time": "2019-04-14T19:08:11.6353239Z", "resourceId": "/SUBSCRIPTIONS/<your_subscription_ID>/RESOURCEGROUPS/<your_resource_group>/PROVIDERS/MICROSOFT.DOCUMENTDB/DATABASEACCOUNTS/<your_database_account>", "category": "QueryRuntimeStatistics", "properties": {"activityId": "278b0661-7452-4df3-b992-8aa0864142cf","databasename": "Tasks","collectionname": "Items","partitionkeyrangeid": "0","querytext": "{"query":"SELECT *\nFROM c\nWHERE (c.p1__10 != true)","parameters":[]}"}}
       ```

      * **Partitionkeystatistics**: Bu günlük, Bölüm anahtarlarının istatistiklerini raporlar. Şu anda istatistikler, Bölüm anahtarlarının depolama boyutu (KB) ile temsil edilir. Günlük, çoğu veri depolamasını kaplayan ilk üç bölüm anahtarına göre yayılır.

       ```
       { "time": "2019-10-11T02:33:24.2018744Z", "resourceId": "/SUBSCRIPTIONS/<your_subscription_ID>/RESOURCEGROUPS/<your_resource_group>/PROVIDERS/MICROSOFT.DOCUMENTDB/DATABASEACCOUNTS/<your_database_account>", "category": "PartitionKeyStatistics", "properties": {"subscriptionId": "<your_subscription_ID>","regionName": "West US 2","databaseName": "KustoQueryResults","collectionname": "CapacityMetrics","partitionkey": "["CapacityMetricsPartition.136"]","sizeKb": "2048270"}}
       ```

      * **Ölçüm istekleri**: ayrıntılı verileri [Azure ölçümlerinde](../azure-monitor/platform/metrics-supported.md)depolamak için bu seçeneği belirleyin. Bir depolama hesabına arşivleme yapıyorsanız, tanılama günlükleri için bekletme dönemini seçebilirsiniz. Günlükler, saklama süresi dolduktan sonra otomatik olarak silinir.

3. **Kaydet**’i seçin.

    "\<çalışma alanı adı için tanılama güncellenemedi > bildiren bir hata alırsanız. Abonelik \<abonelik kimliği > Microsoft. Insights 'ı kullanmak üzere kaydedilmemiş, "hesabı kaydetmek için [sorun giderme Azure tanılama](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-storage) yönergelerini izleyin ve ardından bu yordamı yeniden deneyin.

    Tanılama günlüklerinizin gelecekte herhangi bir noktada nasıl kaydedildiğini değiştirmek istiyorsanız, hesabınızın tanılama günlüğü ayarlarını değiştirmek için bu sayfaya dönün.

## <a name="turn-on-logging-by-using-azure-cli"></a>Azure CLı kullanarak günlüğü açma

Azure CLı kullanarak ölçümleri ve tanılama günlüğünü etkinleştirmek için aşağıdaki komutları kullanın:

- Bir depolama hesabında tanılama günlüklerinin depolanmasını etkinleştirmek için şu komutu kullanın:

   ```azurecli-interactive
   az monitor diagnostic-settings create --name DiagStorage --resource <resourceId> --storage-account <storageAccountName> --logs '[{"category": "QueryRuntimeStatistics", "enabled": true, "retentionPolicy": {"enabled": true, "days": 0}}]'
   ```

   `resource`, Azure Cosmos DB hesabının adıdır. Kaynak "/Subscriptions/`<subscriptionId>`/resourceGroups/`<resource_group_name>`/providers/Microsoft.DocumentDB/databaseAccounts/< Azure_Cosmos_account_name > biçimindedir. `storage-account`, günlükleri göndermek istediğiniz depolama hesabının adıdır. Kategori parametre değerlerini "MongoRequests" veya "DataPlaneRequests" olarak güncelleştirerek diğer günlükleri günlüğe kaydedebilirsiniz. 

- Tanılama günlüklerinin bir olay hub 'ına akışını etkinleştirmek için şu komutu kullanın:

   ```azurecli-interactive
   az monitor diagnostic-settings create --name cdbdiagsett --resourceId <resourceId> --event-hub-rule <eventHubRuleID> --logs '[{"category":"QueryRuntimeStatistics","enabled":true,"retentionPolicy":{"days":6,"enabled":true}}]'
   ```

   `resource`, Azure Cosmos DB hesabının adıdır. `event-hub-rule`, Olay Hub 'ı kural KIMLIĞIDIR. 

- Tanılama günlüklerini bir Log Analytics çalışma alanına göndermeyi etkinleştirmek için şu komutu kullanın:

   ```azurecli-interactive
   az monitor diagnostic-settings create --name cdbdiagsett --resourceId <resourceId> --workspace <resource id of the log analytics workspace> --logs '[{"category":"QueryRuntimeStatistics","enabled":true,"retentionPolicy":{"days":6,"enabled":true}}]'
   ```

Birden çok çıkış seçeneğini etkinleştirmek için bu parametreleri birleştirebilirsiniz.

## <a name="turn-on-logging-by-using-powershell"></a>PowerShell kullanarak günlüğü açma

PowerShell kullanarak tanılama günlüğünü açmak için, en düşük 1.0.1 sürümü ile Azure PowerShell gerekir.

Azure PowerShell'i yüklemek ve Azure aboneliğinizle ilişkilendirmek için bkz. [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/overview).

Azure PowerShell zaten yüklediyseniz ve sürümü bilmiyorsanız PowerShell konsolu türünden `(Get-Module azure -ListAvailable).Version`.  

### <a id="connect"></a>Aboneliklerinize bağlanma
Bir Azure PowerShell oturumu başlatın ve aşağıdaki komutla Azure hesabınızda oturum açın:  

```powershell
Connect-AzAccount
```

Açılır tarayıcı penceresinde Azure hesabı kullanıcı adınızı ve parolanızı girin. Azure PowerShell, bu hesapla ilişkili tüm abonelikleri alır ve varsayılan olarak ilk olanı kullanır.

Birden fazla aboneliğiniz varsa, Azure anahtar kasanızı oluşturmak için kullanılan belirli bir aboneliği belirtmeniz gerekebilir. Hesabınıza yönelik abonelikleri görmek için aşağıdaki komutu yazın:

```powershell
Get-AzSubscription
```

Daha sonra, günlük kaydı yaptığınız Azure Cosmos DB hesapla ilişkili aboneliği belirtmek için aşağıdaki komutu yazın:

```powershell
Set-AzContext -SubscriptionId <subscription ID>
```

> [!NOTE]
> Hesabınızla ilişkili birden fazla aboneliğiniz varsa, kullanmak istediğiniz aboneliği belirtmeniz önemlidir...
>
>

Azure PowerShell yapılandırma hakkında daha fazla bilgi için bkz. [Azure PowerShell nasıl yüklenir ve yapılandırılır](/powershell/azure/overview).

### <a id="storage"></a>Günlükleriniz için yeni bir depolama hesabı oluşturma
Günlüklerinizin mevcut bir depolama hesabını kullanabilseniz de, bu öğreticide, Azure Cosmos DB günlüklerine adanmış yeni bir depolama hesabı oluşturacağız. Kolaylık olması için, depolama hesabı ayrıntılarını **sa**adlı bir değişkende depolarız.

Bu öğreticide, daha fazla yönetim kolaylığı sağlamak için Azure Cosmos veritabanını içeren kaynakla aynı kaynak grubunu kullanacağız. **Contosoresourcegroup**, **contosocosmosdblogs**ve **Orta Kuzey ABD** parametrelerinin değerlerini uygun şekilde değiştirin:

```powershell
$sa = New-AzStorageAccount -ResourceGroupName ContosoResourceGroup `
-Name contosocosmosdblogs -Type Standard_LRS -Location 'North Central US'
```

> [!NOTE]
> Mevcut bir depolama hesabını kullanmaya karar verirseniz, hesabın Azure Cosmos DB aboneliğiniz ile aynı aboneliği kullanması gerekir. Hesap, klasik dağıtım modeli yerine Kaynak Yöneticisi dağıtım modelini de kullanmalıdır.
>
>

### <a id="identify"></a>Günlüklerinizin Azure Cosmos DB hesabını tanımla
Azure Cosmos DB hesap adını **Hesap**adlı bir değişkene ayarlayın; burada **resourceName** , Azure Cosmos DB hesabının adıdır.

```powershell
$account = Get-AzResource -ResourceGroupName ContosoResourceGroup `
-ResourceName contosocosmosdb -ResourceType "Microsoft.DocumentDb/databaseAccounts"
```

### <a id="enable"></a>Günlüğe kaydetmeyi etkinleştirme
Azure Cosmos DB için günlüğe kaydetmeyi etkinleştirmek üzere, yeni depolama hesabı, Azure Cosmos DB hesabı ve günlüğe kaydetme için etkinleştirilecek kategori için değişkenlerle birlikte `Set-AzDiagnosticSetting` cmdlet 'ini kullanın. Aşağıdaki komutu çalıştırın ve **-Enabled** bayrağını **$true**olarak ayarlayın:

```powershell
Set-AzDiagnosticSetting  -ResourceId $account.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories DataPlaneRequests
```

Komutun çıktısı aşağıdaki örneğe benzemelidir:

```powershell
    StorageAccountId            : /subscriptions/<subscription-ID>/resourceGroups/ContosoResourceGroup/providers`
    /Microsoft.Storage/storageAccounts/contosocosmosdblogs
    ServiceBusRuleId            :
    EventHubAuthorizationRuleId :
    Metrics
        TimeGrain       : PT1M
        Enabled         : False
        RetentionPolicy
        Enabled : False
        Days    : 0
    
    Logs
        Category        : DataPlaneRequests
        Enabled         : True
        RetentionPolicy
        Enabled : False
        Days    : 0
    
    WorkspaceId                 :
    Id                          : /subscriptions/<subscription-ID>/resourcegroups/ContosoResourceGroup/providers`
    /microsoft.documentdb/databaseaccounts/contosocosmosdb/providers/microsoft.insights/diagnosticSettings/service
    Name                        : service
    Type                        :
    Location                    :
    Tags                        :
```

Komutun çıktısı, günlüğe kaydetmenin veritabanınız için etkin olduğunu onaylar ve bilgiler depolama hesabınıza kaydediliyor.

İsteğe bağlı olarak, eski günlüklerin otomatik olarak silinmesi gibi günlüklerinizin bekletme ilkesini de ayarlayabilirsiniz. Örneğin, bekletme ilkesini **-RetentionEnabled** bayrağıyla **$true**olarak ayarlayın. 90 günden daha eski günlüklerin otomatik olarak silinmesini sağlamak için **-retentionındays** parametresini **90** olarak ayarlayın.

```powershell
Set-AzDiagnosticSetting -ResourceId $account.ResourceId`
 -StorageAccountId $sa.Id -Enabled $true -Categories DataPlaneRequests`
  -RetentionEnabled $true -RetentionInDays 90
```

### <a id="access"></a>Günlüklerinize erişme
**Dataplanerequests** kategorisi için Azure Cosmos DB günlükleri, belirttiğiniz depolama hesabındaki **Öngörüler-logs-dataplanerequests** kapsayıcısında depolanır. 

İlk olarak, kapsayıcı adı için bir değişken oluşturun. Değişken, izlenecek yol boyunca kullanılır.

```powershell
    $container = 'insights-logs-dataplanerequests'
```

Bu kapsayıcıdaki tüm Blobları listelemek için şunu yazın:

```powershell
Get-AzStorageBlob -Container $container -Context $sa.Context
```

Komutun çıktısı aşağıdaki örneğe benzemelidir:

```powershell
ICloudBlob        : Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob
BlobType          : BlockBlob
Length            : 10510193
ContentType       : application/octet-stream
LastModified      : 9/28/2017 7:49:04 PM +00:00
SnapshotTime      :
ContinuationToken:
Context           : Microsoft.WindowsAzure.Commands.Common.Storage.`
                    LazyAzureStorageContext
Name              : resourceId=/SUBSCRIPTIONS/<subscription-ID>/RESOURCEGROUPS/CONTOSORESOURCEGROUP/PROVIDERS`
/MICROSOFT.DOCUMENTDB/DATABASEACCOUNTS/CONTOSOCOSMOSDB/y=2017/m=09/d=28/h=19/m=00/PT1H.json
```

Bu çıktıdan görebileceğiniz gibi, Bloblar bir adlandırma kuralını takip edebilir: `resourceId=/SUBSCRIPTIONS/<subscription-ID>/RESOURCEGROUPS/<resource group name>/PROVIDERS/MICROSOFT.DOCUMENTDB/DATABASEACCOUNTS/<Database Account Name>/y=<year>/m=<month>/d=<day of month>/h=<hour>/m=<minute>/filename.json`

Tarih ve saat değerleri UTC'yi kullanır.

Aynı depolama hesabı birden çok kaynak için günlükleri toplamak üzere kullanılabileceği için, blob adı içindeki tam kaynak KIMLIĞINI kullanarak ihtiyacınız olan belirli bloblara erişebilir ve bunları indirebilirsiniz. Bunu yapmadan önce tüm Blobların nasıl indirileceği ele alınmaktadır.

İlk olarak, blobları yüklemek için bir klasör oluşturun. Örneğin:

```powershell
New-Item -Path 'C:\Users\username\ContosoCosmosDBLogs'`
 -ItemType Directory -Force
```

Ardından, Blobların tümünün bir listesini alın:  

```powershell
$blobs = Get-AzStorageBlob -Container $container -Context $sa.Context
```

Blob 'ları hedef klasöre indirmek için bu listeyi `Get-AzStorageBlobContent` komutu aracılığıyla boru yapın:

```powershell
$blobs | Get-AzStorageBlobContent `
 -Destination 'C:\Users\username\ContosoCosmosDBLogs'
```

Bu ikinci komutu çalıştırdığınızda, blob adlarında **/** sınırlayıcısı hedef klasör altında tam bir klasör yapısı oluşturur. Bu klasör yapısı, Blobları dosya olarak indirmek ve depolamak için kullanılır.

Blobları seçmeli olarak indirmek için jokerleri kullanın. Örneğin:

* Birden çok veritabanınız varsa ve **CONTOSOCOSMOSDB3**adlı yalnızca bir veritabanı için günlükleri indirmek istiyorsanız komutunu kullanın:

    ```powershell
    Get-AzStorageBlob -Container $container `
     -Context $sa.Context -Blob '*/DATABASEACCOUNTS/CONTOSOCOSMOSDB3
    ```

* Birden çok kaynak grubunuz varsa ve tek bir kaynak grubu için günlükleri indirmek istiyorsanız `-Blob '*/RESOURCEGROUPS/<resource group name>/*'`komutunu kullanın:

    ```powershell
    Get-AzStorageBlob -Container $container `
    -Context $sa.Context -Blob '*/RESOURCEGROUPS/CONTOSORESOURCEGROUP3/*'
    ```
* 2017 Temmuz ayına ait tüm günlükleri indirmek isterseniz, `-Blob '*/year=2017/m=07/*'`komutunu kullanın:

    ```powershell
    Get-AzStorageBlob -Container $container `
     -Context $sa.Context -Blob '*/year=2017/m=07/*'
    ```

Ayrıca aşağıdaki komutları çalıştırabilirsiniz:

* Veritabanı kaynağınızın tanılama ayarlarının durumunu sorgulamak için `Get-AzDiagnosticSetting -ResourceId $account.ResourceId`komutunu kullanın.
* Veritabanı hesabı kaynağınız için **Dataplanerequests** kategorisinin günlüğe kaydedilmesini devre dışı bırakmak için `Set-AzDiagnosticSetting -ResourceId $account.ResourceId -StorageAccountId $sa.Id -Enabled $false -Categories DataPlaneRequests`komutunu kullanın.


Bu sorguların her birinde döndürülen Bloblar, aşağıdaki kodda gösterildiği gibi metin olarak depolanır ve JSON blobu olarak biçimlendirilir:

```json
{
    "records":
    [
        {
           "time": "Fri, 23 Jun 2017 19:29:50.266 GMT",
           "resourceId": "contosocosmosdb",
           "category": "DataPlaneRequests",
           "operationName": "Query",
           "resourceType": "Database",
           "properties": {"activityId": "05fcf607-6f64-48fe-81a5-f13ac13dd1eb",`
           "userAgent": "documentdb-dotnet-sdk/1.12.0 Host/64-bit MicrosoftWindowsNT/6.2.9200.0 AzureSearchIndexer/1.0.0",`
           "resourceType": "Database","statusCode": "200","documentResourceId": "",`
           "clientIpAddress": "13.92.241.0","requestCharge": "2.260","collectionRid": "",`
           "duration": "9250","requestLength": "72","responseLength": "209", "resourceTokenUserRid": ""}
        }
    ]
}
```

Her JSON blobundan oluşan veriler hakkında bilgi edinmek için bkz. [Azure Cosmos DB günlüklerinizi yorumlama](#interpret).

## <a name="manage-your-logs"></a>Günlüklerinizi yönetme

Tanılama günlükleri hesabınızda iki saat boyunca Azure Cosmos DB işlem yapıldığı zamandan itibaren kullanılabilir hale getirilir. Depolama hesabınızdaki günlüklerinizi yönetmek size bağlıdır:

* Günlüklerinizi güvenli hale getirmek ve bunlara kimlerin erişebileceğini kısıtlamak için standart Azure erişim denetimi yöntemlerini kullanın.
* Artık depolama hesabınızda tutmak istemediğiniz günlükleri silin.
* **Günlük DataPlaneRequests** ayarı seçildiğinde, bir depolama hesabına arşivlenen veri düzlemi istekleri için bekletme süresi portalda yapılandırılır. Bu ayarı değiştirmek için, bkz. [Azure Portal günlük kaydını açma](#turn-on-logging-in-the-azure-portal).


<a id="#view-in-loganalytics"></a>
## <a name="view-logs-in-azure-monitor-logs"></a>Günlükleri Azure Izleyici günlüklerinde görüntüleme

Tanılama günlüğünü açtığınızda **Log Analytics gönder** seçeneğini belirlediyseniz, kapsayıcıınızdan Tanılama verileri iki saat Içinde Azure izleyici günlüklerine iletilir. Günlüğe kaydetmeyi etkinleştirdikten hemen sonra Azure Izleyici günlüklerine baktığınızda, hiçbir veri görmezsiniz. Yalnızca iki saat bekleyip yeniden deneyin. 

Günlüklerinizi görüntülemeden önce, Log Analytics çalışma alanınızın yeni kusto sorgu dilini kullanmak üzere yükseltildiğini kontrol edin. Bunu denetlemek için [Azure Portal](https://portal.azure.com)açın, en soldaki **Log Analytics çalışma alanlarını** seçin ve ardından sonraki görüntüde gösterildiği gibi çalışma alanı adını seçin. **Log Analytics çalışma alanı** sayfası görüntülenir:

![Azure portal Azure Izleyici günlükleri](./media/logging/azure-portal.png)

>[!NOTE]
>OMS çalışma alanları artık Log Analytics çalışma alanları olarak adlandırılır.  

**Log Analytics çalışma alanı** sayfasında aşağıdaki iletiyi görürseniz, çalışma alanınız yeni dili kullanmak üzere yükseltilmemiştir. Yeni sorgu diline yükseltme hakkında daha fazla bilgi için bkz. [Azure Log Analytics çalışma alanınızı yeni günlük aramasına yükseltme](../log-analytics/log-analytics-log-search-upgrade.md). 

![Azure Izleyici günlükleri yükseltme iletisi](./media/logging/upgrade-notification.png)

Azure Izleyici günlüklerinde tanılama verilerinizi görüntülemek için, aşağıdaki görüntüde gösterildiği gibi sol menüden veya sayfanın **Yönetim** alanından **günlük araması** sayfasını açın:

![Azure portal günlük arama seçenekleri](./media/logging/log-analytics-open-log-search.png)

Veri toplamayı etkinleştirmiş olduğunuza göre, yeni sorgu dilini kullanarak en son 10 günlük `AzureDiagnostics | take 10`görmek için aşağıdaki günlük arama örneğini çalıştırın.

![En son 10 günlüğe örnek günlük arama](./media/logging/log-analytics-query.png)

<a id="#queries"></a>
### <a name="cosmosdb-log-analytics-queries-in-azure-monitor"></a>Azure Izleyici 'de CosmosDB Log Analytics sorguları

Azure Cosmos Kapsayıcılarınızı izlemenize yardımcı olması için **günlük araması** kutusuna girebileceğiniz bazı ek sorgular aşağıda verilmiştir. Bu sorgular [Yeni dille](../log-analytics/log-analytics-log-search-upgrade.md)çalışır.  

Her günlük arama tarafından döndürülen verilerin anlamı hakkında bilgi edinmek için bkz. [Azure Cosmos DB günlüklerinizi yorumlama](#interpret).

* Belirli bir süre için Azure Cosmos DB tüm tanılama günlüklerini sorgulamak için:

    ```
    AzureDiagnostics 
    | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests"
    ```

* En son günlüğe kaydedilen 10 olayı sorgulamak için:

    ```
    AzureDiagnostics 
    | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" 
    | limit 10
    ```

* İşlem türüne göre gruplandırılan tüm işlemleri sorgulamak için:

    ```
    AzureDiagnostics 
    | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" 
    | summarize count() by OperationName
    ```

* **Kaynağa**göre gruplandırılan tüm işlemleri sorgulamak için:

    ```
    AzureActivity 
    | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" 
    | summarize count() by Resource
    ```

* Kaynağa göre gruplandırılan tüm kullanıcı etkinliklerini sorgulamak için:

    ```
    AzureActivity 
    | where Caller == "test@company.com" and ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" 
    | summarize count() by Resource
    ```
    > [!NOTE]
    > Bu komut, tanılama günlüğü değil, etkinlik günlüğü içindir.

* Data Planerequests ve QueryRunTimeStatistics 'ten alınan 100 ru 'dan büyük tüm sorguları almak için

    ```
    AzureDiagnostics
    | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" and todouble(requestCharge_s) > 100.0
    | project activityId_g, requestCharge_s
    | join kind= inner (
           AzureDiagnostics
           | where ResourceProvider =="MICROSOFT.DOCUMENTDB" and Category == "QueryRuntimeStatistics"
           | project activityId_g, querytext_s
    ) on $left.activityId_g == $right.activityId_g
    | order by requestCharge_s desc
    | limit 100
    ```
    
      

* 3 milisaniyeden daha uzun sürecek işlemleri sorgulamak için:

    ```
    AzureDiagnostics 
    | where toint(duration_s) > 3 and ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" 
    | summarize count() by clientIpAddress_s, TimeGenerated
    ```

* İşlemleri çalıştıran aracı sorgulamak için:

    ```
    AzureDiagnostics 
    | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" 
    | summarize count() by OperationName, userAgent_s
    ```

* Uzun süre çalışan işlemlerin ne zaman gerçekleştirildiğini sorgulamak için:

    ```
    AzureDiagnostics 
    | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" 
    | project TimeGenerated , duration_s 
    | render timechart
    ```
    
* Veritabanı hesabı için ilk 3 bölümde eğriliği değerlendirmek üzere bölüm anahtarı istatistiklerini almak için:

    ```
    AzureDiagnostics 
    | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="PartitionKeyStatistics" 
    | project SubscriptionId, regionName_s, databaseName_s, collectionname_s, partitionkey_s, sizeKb_s, ResourceId 
    
   
    ```
    

Yeni günlük araması dilini kullanma hakkında daha fazla bilgi için bkz. [Azure izleyici günlüklerinde günlük aramalarını anlama](../log-analytics/log-analytics-log-search-new.md). 

## <a id="interpret"></a>Günlüklerinizi yorumlama

Azure depolama ve Azure Izleyici günlüklerinde depolanan Tanılama verileri benzer bir şema kullanır. 

Aşağıdaki tabloda her günlük girişinin içeriği açıklanmaktadır.

| Azure depolama alanı veya özelliği | Azure Izleyici günlükleri özelliği | Açıklama |
| --- | --- | --- |
| **ışınızda** | **TimeGenerated** | İşlem gerçekleştiği tarih ve saat (UTC). |
| **RESOURCEID** | **Kaynak** | Günlüklerin etkinleştirildiği Azure Cosmos DB hesabı.|
| **alan** | **Kategori** | Azure Cosmos DB günlükleri için, **Dataplanerequests** kullanılabilir tek değerdir. |
| **operationName** | **OperationName** | İşlemin adı. Bu değer şu işlemlerden herhangi biri olabilir: oluşturma, güncelleştirme, okuma, ReadFeed, silme, değiştirme, yürütme, SqlQuery, Query, JSQuery, Head, HeadFeed veya upsert.   |
| **özelliklerinin** | yok | Bu alanın içeriği, izleyen satırlarda açıklanmıştır. |
| **Etkinlik kimliği** | **activityId_g** | Günlüğe kaydedilen işlem için benzersiz GUID. |
| **Kullanıcı** | **userAgent_s** | İsteği gerçekleştiren istemci Kullanıcı aracısını belirten bir dize. Biçim {User Agent Name}/{Version} biçimindedir.|
| **requestResourceType** | **requestResourceType_s** | Erişilen kaynağın türü. Bu değer, şu kaynak türlerinden herhangi biri olabilir: veritabanı, kapsayıcı, belge, ek, Kullanıcı, Izin, StoredProcedure, tetikleyici, UserDefinedFunction veya teklif. |
| **Durum** | **statusCode_s** | İşlemin yanıt durumu. |
| **Requestresourceıd** | **RESOURCEID** | İstekle ilgili olan RESOURCEID. Değer, gerçekleştirilen işleme bağlı olarak databaseRid, collectionRid veya belge kimliği ' ni işaret edebilir.|
| **ClientIpAddress** | **clientIpAddress_s** | İstemcinin IP adresi. |
| **Istek ücreti** | **requestCharge_s** | İşlem tarafından kullanılan RUs sayısı |
| **collectionRid** | **collectionId_s** | Koleksiyonun benzersiz KIMLIĞI.|
| **sürenin** | **duration_s** | İşlemin süresi (milisaniye cinsinden). |
| **requestLength** | **requestLength_s** | İsteğin bayt cinsinden uzunluğu. |
| **responseLength** | **responseLength_s** | Yanıtın bayt cinsinden uzunluğu.|
| **resourceTokenUserRid** | **resourceTokenUserRid_s** | Bu değer, kimlik doğrulaması için [kaynak belirteçleri](https://docs.microsoft.com/azure/cosmos-db/secure-access-to-data#resource-tokens) kullanılırken boş değildir. Değer, kullanıcının kaynak KIMLIĞINE işaret eder. |

## <a name="next-steps"></a>Sonraki adımlar

- Günlük kaydını etkinleştirmeyi ve ayrıca çeşitli Azure hizmetleri tarafından desteklenen ölçümleri ve günlük kategorilerini öğrenmek için, [Microsoft Azure içindeki ölçümlere genel bakışı](../monitoring-and-diagnostics/monitoring-overview-metrics.md) ve [Azure tanılama günlükleri makalelerine genel bakış](../azure-monitor/platform/resource-logs-overview.md) makalesini okuyun.
- Olay Hub 'ları hakkında bilgi edinmek için şu makaleleri okuyun:
   - [Azure Event Hubs nedir?](../event-hubs/event-hubs-what-is-event-hubs.md)
   - [Event Hubs kullanmaya başlayın](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)
- [Azure depolama 'dan ölçümleri ve tanılama günlüklerini](../storage/blobs/storage-quickstart-blobs-dotnet.md#download-blobs)okuyun.
- [Azure izleyici günlüklerinde günlük aramalarını anlayın](../log-analytics/log-analytics-log-search-new.md).
