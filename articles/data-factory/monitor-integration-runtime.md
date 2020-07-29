---
title: Azure Data Factory tümleştirme çalışma zamanını izleme
description: Azure Data Factory farklı tümleştirme çalışma zamanı türlerini nasıl izleyeceğinizi öğrenin.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 07/25/2020
author: djpmsft
ms.author: daperlov
manager: anandsub
ms.openlocfilehash: 14f9ab0b1c3b8b437e46a7b6a2d8b87f03442a02
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87290565"
---
# <a name="monitor-an-integration-runtime-in-azure-data-factory"></a>Azure Data Factory'deki tümleştirme çalışma zamanını izleme

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]
  
**Integration Runtime** , farklı ağ ortamlarında çeşitli veri tümleştirme özellikleri sağlamak için Azure Data Factory (ADF) tarafından kullanılan işlem altyapısıdır. Data Factory tarafından sunulan üç tür tümleştirme çalışma zamanı vardır:

- Azure tümleştirme çalışma zamanı
- Kendinden konak tümleştirme çalışma zamanı
- Azure-SQL Server Integration Services (SSIS) tümleştirme çalışma zamanı

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Tümleştirme çalışma zamanı (IR) örneğinin durumunu almak için aşağıdaki PowerShell komutunu çalıştırın: 

```powershell
Get-AzDataFactoryV2IntegrationRuntime -DataFactoryName MyDataFactory -ResourceGroupName MyResourceGroup -Name MyAzureIR -Status
``` 

Cmdlet 'i farklı tümleştirme çalışma zamanı türleri için farklı bilgiler döndürür. Bu makalede, her bir tümleştirme çalışma zamanı türünün özellikleri ve durumları açıklanmaktadır.  

## <a name="azure-integration-runtime"></a>Azure tümleştirme çalışma zamanı

Azure tümleştirme çalışma zamanı için işlem kaynağı, Azure 'da tam olarak yönetilen esnek. Aşağıdaki tabloda **Get-AzDataFactoryV2IntegrationRuntime** komutu tarafından döndürülen özellikler için açıklamalar verilmiştir:

### <a name="properties"></a>Özellikler

Aşağıdaki tabloda, bir Azure tümleştirme çalışma zamanı için cmdlet tarafından döndürülen özelliklerin açıklamaları verilmiştir:

| Özellik | Açıklama |
-------- | ------------- | 
| Ad | Azure tümleştirme çalışma zamanının adı. |  
| Durum | Azure tümleştirme çalışma zamanının durumu. | 
| Konum | Azure tümleştirme çalışma zamanının konumu. Bir Azure tümleştirme çalışma zamanının konumu hakkındaki ayrıntılar için bkz. [Integration Runtime 'A giriş](concepts-integration-runtime.md). |
| DataFactoryName | Azure tümleştirme çalışma zamanının ait olduğu veri fabrikasının adı. | 
| ResourceGroupName | Data Factory 'nin ait olduğu kaynak grubunun adı.  |
| Açıklama | Tümleştirme çalışma zamanının açıklaması.  |

### <a name="status"></a>Durum

Aşağıdaki tabloda bir Azure tümleştirme çalışma zamanının olası durumları verilmiştir:

| Durum | Açıklamalar/senaryolar | 
| ------ | ------------------ |
| Çevrimiçi | Azure tümleştirme çalışma zamanı çevrimiçi ve kullanılabilir hale gelmiştir. | 
| Çevrimdışı | Azure Integration Runtime bir iç hata nedeniyle çevrimdışı. |

## <a name="self-hosted-integration-runtime"></a>Kendinden konak tümleştirme çalışma zamanı

Bu bölüm Get-AzDataFactoryV2IntegrationRuntime cmdlet 'i tarafından döndürülen özelliklerle ilgili açıklamalar sağlar. 

> [!NOTE] 
> Döndürülen özellikler ve durum, genel olarak barındırılan tümleştirme çalışma zamanı ve çalışma zamanındaki her düğüm hakkında bilgi içerir.  

### <a name="properties"></a>Özellikler

Aşağıdaki tabloda **her düğüm**Için izleme özelliklerinin açıklamaları verilmiştir:

| Özellik | Açıklama | 
| -------- | ----------- | 
| Ad | Şirket içinde barındırılan tümleştirme çalışma zamanının ve onunla ilişkili düğümlerin adı. Düğüm, şirket içinde barındırılan tümleştirme çalışma zamanının yüklü olduğu şirket içi bir Windows makinedir. |  
| Durum | Genel olarak barındırılan tümleştirme çalışma zamanının ve her düğümün durumu. Örnek: çevrimiçi/çevrimdışı/sınırlı/vb. Bu durumlar hakkında daha fazla bilgi için sonraki bölüme bakın. | 
| Sürüm | Şirket içinde barındırılan tümleştirme çalışma zamanının ve her düğümün sürümü. Şirket içinde barındırılan tümleştirme çalışma zamanının sürümü, gruptaki düğümlerin çoğunluğu sürümüne göre belirlenir. Şirket içinde barındırılan tümleştirme çalışma zamanı kurulumunda farklı sürümlere sahip düğümler varsa, yalnızca mantıksal şirket içinde barındırılan tümleştirme çalışma zamanı ile aynı sürüm numarasına sahip düğümler düzgün şekilde çalışır. Diğerleri sınırlı moddadır ve el ile güncelleştirilmesi gerekir (yalnızca büyük/küçük harfe otomatik güncelleştirme başarısız olur). | 
| Kullanılabilir bellek | Şirket içinde barındırılan tümleştirme çalışma zamanı düğümünde kullanılabilir bellek. Bu değer, neredeyse gerçek zamanlı bir anlık görüntüdür. | 
| CPU kullanımı | Şirket içinde barındırılan tümleştirme çalışma zamanı düğümünün CPU kullanımı. Bu değer, neredeyse gerçek zamanlı bir anlık görüntüdür. |
| Ağ (ın/out) | Şirket içinde barındırılan tümleştirme çalışma zamanı düğümünün ağ kullanımı. Bu değer, neredeyse gerçek zamanlı bir anlık görüntüdür. | 
| Eşzamanlı Işler (çalışıyor/limit) | **Çalışıyor**. Her düğümde çalışan iş veya görev sayısı. Bu değer, neredeyse gerçek zamanlı bir anlık görüntüdür. <br/><br/>**Sınır**. Limit her düğüm için en fazla eş zamanlı işi belirtir. Bu değer makine boyutuna göre tanımlanır. CPU, bellek veya ağ kullanımda olsa bile etkinlikler zaman aşımına uğradıkça, Gelişmiş senaryolarda eşzamanlı iş yürütülmesini ölçeklendirmeye yönelik sınırı artırabilirsiniz. Bu yetenek, tek düğümlü bir şirket içinde barındırılan tümleştirme çalışma zamanı ile de kullanılabilir. |
| Rol | Çok düğümlü bir şirket içinde barındırılan tümleştirme çalışma zamanı – dağıtıcı ve çalışan içinde iki tür rol vardır. Tüm düğümler çalışanlardır, yani işleri yürütmek için hepsi kullanılabilirler. Bulut hizmetlerinden görevleri/işleri çekmek ve bunları farklı çalışan düğümlerine göndermek için kullanılan yalnızca bir dağıtıcı düğümü vardır. Dağıtıcı düğümü de bir çalışan düğümüdür. |

Özelliklerin bazı ayarları, şirket içinde barındırılan tümleştirme çalışma zamanı 'nda (yani, bir genişleme senaryosunda) iki veya daha fazla düğüm olduğunda daha anlamlı hale getirir.

#### <a name="concurrent-jobs-limit"></a>Eşzamanlı işlerin sınırı

Eşzamanlı işler sınırının varsayılan değeri makine boyutuna göre ayarlanır. Bu değeri hesaplamak için kullanılan faktörler, RAM miktarına ve makinenin CPU çekirdekleri sayısına bağlıdır. Bu nedenle, daha fazla çekirdek ve daha fazla bellek, eşzamanlı işlerin varsayılan sınırının daha yüksektir.

Düğüm sayısını artırarak ölçeklendirebilirsiniz. Düğüm sayısını artırdığınızda, eşzamanlı işler sınırı, tüm kullanılabilir düğümlerin eşzamanlı iş sınırı değerlerinin toplamıdır.  Örneğin, bir düğüm en fazla on iki eşzamanlı iş çalıştırmanızı sağlar, daha sonra üç benzer düğüm eklemek en fazla 48 eşzamanlı iş (yani 4 x 12) çalıştırmanızı sağlar. Yalnızca her bir düğümdeki varsayılan değerlerle kaynak kullanımını düşük bir şekilde gördüğünüzde, eşzamanlı iş sınırını artırmanız önerilir.

Azure portal hesaplanan varsayılan değeri geçersiz kılabilirsiniz. > bağlantıları > tümleştirme çalışma zamanları > > düğümlerini Düzenle > düğüm başına eşzamanlı iş değerini değiştir ' i seçin. PowerShell [Update-Azdatafactoryv2integrationruntimenode](https://docs.microsoft.com/powershell/module/az.datafactory/update-Azdatafactoryv2integrationruntimenode#examples) komutunu da kullanabilirsiniz.
  
### <a name="status-per-node"></a>Durum (düğüm başına)

Aşağıdaki tabloda, şirket içinde barındırılan tümleştirme çalışma zamanı düğümünün olası durumları verilmiştir:

| Durum | Açıklama |
| ------ | ------------------ | 
| Çevrimiçi | Düğüm Data Factory hizmetine bağlandı. |
| Çevrimdışı | Düğüm çevrimdışı. |
| Yükseltmenin | Düğüm otomatik olarak güncelleştiriliyor. |
| Sınırlı | Bir bağlantı sorunu nedeniyle. HTTP bağlantı noktası 8050 sorunu, Service Bus bağlantı sorunu veya bir kimlik bilgisi eşitleme sorunuyla kaynaklanıyor olabilir. |
| Etkin değil | Düğüm, diğer çoğunluk düğümlerin yapılandırmasından farklı bir yapılandırmadır. |

Düğüm, diğer düğümlere bağlanamıyorsa devre dışı olabilir.

### <a name="status-overall-self-hosted-integration-runtime"></a>Durum (genel olarak barındırılan tümleştirme çalışma zamanı)

Aşağıdaki tabloda, şirket içinde barındırılan tümleştirme çalışma zamanının olası durumları verilmiştir. Bu durum, çalışma zamanına ait tüm düğümlerin durumlarına bağlıdır. 

| Durum | Açıklama |
| ------ | ----------- | 
| Kayıt gerekiyor | Henüz şirket içinde barındırılan tümleştirme çalışma zamanına hiçbir düğüm kaydedilmemiş. |
| Çevrimiçi | Tüm düğümler çevrimiçi. |
| Çevrimdışı | Çevrimiçi düğüm yok. |
| Sınırlı | Bu şirket içinde barındırılan tümleştirme çalışma zamanındaki düğümlerin hepsi sağlıklı durumda değil. Bu durum, bazı düğümlerin kapatılmış olabileceğini belirten bir uyarıdır. Bu durum, dağıtıcı/çalışan düğümündeki bir kimlik bilgisi eşitleme sorunundan kaynaklanıyor olabilir. |

Ayrıntılı şirket içinde barındırılan tümleştirme çalışma zamanı özelliklerini ve cmdlet 'inin yürütülmesi sırasında anlık görüntü değerlerini içeren JSON yükünü getirmek için **Get-AzDataFactoryV2IntegrationRuntimeMetric** cmdlet 'ini kullanın.

```powershell
Get-AzDataFactoryV2IntegrationRuntimeMetric -name $integrationRuntimeName -ResourceGroupName $resourceGroupName -DataFactoryName $dataFactoryName | ConvertTo-Json 
```

Örnek çıktı (Bu şirket içinde barındırılan tümleştirme çalışma zamanı ile ilişkili iki düğüm olduğunu varsayar):

```json
{
    "IntegrationRuntimeName":  "<Name of your integration runtime>",
    "ResourceGroupName":  "<Resource Group Name>",
    "DataFactoryName":  "<Data Factory Name>",
    "Nodes":  [
        {
            "NodeName":  "<Node Name>",
            "AvailableMemoryInMB":  <Value>,
            "CpuUtilization":  <Value>,
            "ConcurrentJobsLimit":  <Value>,
            "ConcurrentJobsRunning":  <Value>,
            "MaxConcurrentJobs":  <Value>,
            "SentBytes":  <Value>,
            "ReceivedBytes":  <Value>
        },
        {
            "NodeName":  "<Node Name>",
            "AvailableMemoryInMB":  <Value>,
            "CpuUtilization":  <Value>,
            "ConcurrentJobsLimit":  <Value>,
            "ConcurrentJobsRunning":  <Value>,
            "MaxConcurrentJobs":  <Value>,
            "SentBytes":  <Value>,
            "ReceivedBytes":  <Value>
        }

    ]
} 
```

## <a name="azure-ssis-integration-runtime"></a>Azure SSIS tümleştirmesi çalışma zamanı

Azure-SSIS IR, SSIS paketlerinizi çalıştırmak için adanmış, tam olarak yönetilen bir Azure sanal makine (veya düğümleri) kümesidir. Örneğin, Azure etkin SQL Server Veri Araçları (SSDT), AzureDTExec komut satırı yardımcı programı, T-SQL on SQL Server Management Studio (SSMS)/SQL Server Agent aracılığıyla ve SSIS paket etkinliklerini ADF işlem hatlarında yürütmek üzere çeşitli yöntemleri kullanarak Azure-SSIS IR SSIS paket yürütmelerini çağırabilirsiniz. Azure-SSIS IR diğer ADF etkinliklerini çalıştırmaz. Sağlandıktan sonra, Azure PowerShell, Azure portal ve Azure Izleyici aracılığıyla genel/düğüme özgü özellikleri ve durumları izleyebilirsiniz.

### <a name="monitor-the-azure-ssis-integration-runtime-with-azure-powershell"></a>Azure-SSIS tümleştirme çalışma zamanını Azure PowerShell ile izleme

Aşağıdaki Azure PowerShell cmdlet 'ini kullanarak genel/düğüm özgü özellikleri ve Azure-SSIS IR durumlarını izleyin.

```powershell
Get-AzDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSSISName -ResourceGroupName $ResourceGroupName -Status
```

#### <a name="properties"></a>Özellikler

Aşağıdaki tabloda, bir Azure-SSIS IR için yukarıdaki cmdlet tarafından döndürülen özelliklerin açıklamaları verilmiştir.

| Özellik/durum              | Açıklama                  |
| ---------------------------- | ---------------------------- |
| CreateTime                   | Azure-SSIS IR oluşturulduğu UTC saati. |
| Düğümler                        | Azure-SSIS IR düğüme özgü durumlar (başlangıç/kullanılabilir/geri dönüştürme/kullanılamaz) ve eyleme dönüştürülebilir hatalar içeren ayrılmış/kullanılabilir düğümler. |
| Diğer hatalar                  | Azure-SSIS IR düğüme özgü olmayan işlem yapılabilir hatalar. |
| LastOperation                | Başarısız olduysa hata (lar) ile Azure-SSIS IR en son başlatma/durdurma işleminin sonucu. |
| Durum                        | Azure-SSIS IR genel durumu (ilk/başlangıç/başlatma/durdurma/durdurulma). |
| Konum                     | Azure-SSIS IR konumu. |
| NodeSize                     | Azure-SSIS IR her bir düğümün boyutu. |
| NodeCount                    | Azure-SSIS IR düğümlerin sayısı. |
| MaxParallelExecutionsPerNode | Azure-SSIS IR düğüm başına en fazla paralel yürütme sayısı. |
| CatalogServerEndpoint        | SSIS kataloğunu (SSSıSDB) barındırmak için mevcut Azure SQL veritabanı sunucunuzun veya yönetilen örneğinizin uç noktası. |
| CatalogAdminUserName         | Mevcut Azure SQL veritabanı sunucunuz veya yönetilen örnek için Yönetici Kullanıcı adı. ADF, bu bilgileri, sizin adınıza SSıSDB hazırlamak ve yönetmek için kullanır. |
| CatalogAdminPassword         | Mevcut Azure SQL veritabanı sunucunuz veya yönetilen örnek için yönetici parolası. |
| CatalogPricingTier           | Azure SQL veritabanı sunucusu tarafından barındırılan SSSıSDB fiyatlandırma katmanı.  SSıSDB barındıran Azure SQL yönetilen örneği için geçerli değildir. |
| Vnetıd                       | Azure-SSIS IR katılacak sanal ağ kaynak KIMLIĞI. |
| Alt ağ                       | Azure-SSIS IR katılacak alt ağ adı. |
| ID                           | Azure-SSIS IR kaynak KIMLIĞI. |
| Tür                         | Azure-SSIS IR IR türü (yönetilen/kendiliğinden konak). |
| ResourceGroupName            | ADF ve Azure-SSIS IR oluşturulduğu Azure Kaynak grubunuzun adı. |
| DataFactoryName              | ADF 'nizin adı. |
| Ad                         | Azure-SSIS IR adı. |
| Açıklama                  | Azure-SSIS IR açıklaması. |
  
#### <a name="status-per-azure-ssis-ir-node"></a>Durum (Azure-SSIS IR düğüm başına)

Aşağıdaki tabloda bir Azure-SSIS IR düğümünün olası durumları verilmiştir:

| Düğüme özgü durum | Açıklama |
| -------------------- | ----------- | 
| Başlatılıyor             | Bu düğüm hazırlanıyor. |
| Kullanılabilir            | Bu düğüm, SSIS paketlerini dağıtmanız/yürütmeniz için hazırlayın. |
| 2            | Bu düğüm onarılıyor/yeniden başlatılıyor. |
| Kullanılamaz          | Bu düğüm, SSIS paketlerini dağıtmanıza/yürütmeye yönelik değil ve giderebileceğiniz hata/sorunlar oluştu. |

#### <a name="status-overall-azure-ssis-ir"></a>Durum (Genel Azure-SSIS IR)

Aşağıdaki tabloda Azure-SSIS IR genel durumları verilmiştir. İçindeki genel durum, Azure-SSIS IR ait olan tüm düğümlerin Birleşik durumlarına bağlıdır. 

| Genel durum | Açıklama | 
| -------------- | ----------- | 
| Başlangıç        | Azure-SSIS IR düğümleri ayrılmadı/hazırlandı. | 
| Başlatılıyor       | Azure-SSIS IR düğümleri ayrılmakta/hazırlanmakta ve faturalandırma başladı. |
| Başlarken        | Azure-SSIS IR düğümleri ayrıldı/hazırlandı ve SSIS paketlerini dağıtmanıza/yürütmeniz için hazır. |
| Durduruluyor       | Azure-SSIS IR düğümleri serbest bırakılıyor. |
| Durduruldu        | Azure-SSIS IR düğümleri yayımlanmıştır ve faturalandırma durdurulur. |

### <a name="monitor-the-azure-ssis-integration-runtime-in-azure-portal"></a>Azure portal 'de Azure-SSIS tümleştirme çalışma zamanını izleme

Azure-SSIS IR Azure portal izlemek için, ADF Kullanıcı arabirimindeki **izleyici** hub 'ının **tümleştirme çalışma zamanları** sayfasına gidin; burada tüm tümleştirme çalışma zamanlarını görebilirsiniz.

![Tüm tümleştirme çalışma zamanlarını izle](media/monitor-integration-runtime/monitor-integration-runtimes.png)

Sonra, izleme sayfasını açmak için Azure-SSIS IR adını seçin; burada, genel/düğüme özgü özellikleri ve durumlarını görebilirsiniz.

![Azure-SSIS IR izleyin](media/monitor-integration-runtime/monitor-azure-ssis-integration-runtime.png)

Azure-SSIS IR izleme sayfanızın **durum** kutucuğunda, genel durumunu (örneğin, **çalışıyor** veya **durduruldu**) görebilirsiniz. **Çalışma** durumunun seçilmesi, Azure-SSIS IR durdurmak Için canlı **durdurma** düğmesine sahip bir pencere açılır. **Durdurulmuş** durum seçildiğinde Azure-SSIS IR başlatmak Için canlı **Başlat** düğmesini içeren bir pencere açılır. Açılır pencerede, Azure-SSIS IR üzerinde çalışan SSIS paketi yürütme etkinliği ile bir ADF işlem hattını otomatik olarak oluşturmak için bir **SSIS paketi yürütme** düğmesi vardır (bkz. [SSIS paketlerini ADF Işlem hatları 'Nda SSIS paketi etkinliklerini yürütme](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity)) ve bir **kaynak kimliği** metin kutusu `/subscriptions/YourAzureSubscripton/resourcegroups/YourResourceGroup/providers/Microsoft.DataFactory/factories/YourADF/integrationruntimes/YourAzureSSISIR` ' den bağımsız yazılım satıcıları (ISV 'ler) için ek Premium/lisanslı ssıs BILEŞENLERI satın almak üzere kullanılabilecek Azure-SSIS IR kaynak kimliğinizi () kopyalayabilir ve bunları Azure-SSIS IR bağlayabilirsiniz (bkz. [Azure-SSIS IR Premium/lisanslı bileşenleri yükleme](https://docs.microsoft.com/azure/data-factory/how-to-develop-azure-ssis-ir-licensed-components)).

![Azure-SSIS IR durumu Kutucuğunuzu izleyin](media/monitor-integration-runtime/monitor-azure-ssis-integration-runtime-status.png)

Paketlerin Azure SQL veritabanı sunucunuz veya yönetilen örneğiniz tarafından barındırılan SSSıSDB 'de depolandığı proje dağıtım modelini kullanıyorsanız, Azure-SSIS IR izleme sayfanızda **SSıSDB sunucu uç noktası** kutucuğunu görürsünüz (bkz. [Azure-SSIS IR dağıtım ayarlarınızı yapılandırma](https://docs.microsoft.com/azure/data-factory/tutorial-deploy-ssis-packages-azure#deployment-settings-page)). Bu kutucukta, bir pencere açmak için Azure SQL veritabanı sunucunuzu veya yönetilen örneğinizi tanımlayarak bir bağlantı seçebilirsiniz. buradan, sunucu uç noktasını bir metin kutusundan kopyalayabilir ve SMS 'den bağlanırken, paketlerinizi dağıtmak, yapılandırmak, çalıştırmak ve yönetmek için kullanabilirsiniz. Açılır pencerede, Azure portal SSıSDB 'nizi yeniden yapılandırmak/yeniden boyutlandırmak için **Azure SQL veritabanınızı veya yönetilen örnek ayarlarını görüntüleyin** bağlantısını da seçebilirsiniz.

![Azure-SSIS IR-SSıSDB Kutucuğunuzu izleyin](media/monitor-integration-runtime/monitor-azure-ssis-integration-runtime-ssisdb.png)

Azure-SSIS IR bir sanal ağa katılırsanız, Azure-SSIS IR izleme sayfanızda **VNET/subnet 'ı doğrula** kutucuğunu görürsünüz (bkz. [Azure-SSIS IR VNET 'e katılma](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network)). Bu kutucukta, VNET ve alt ağınızı tanımlayarak bir pencere açmaya yönelik bir bağlantı seçebilirsiniz. burada, sanal ağ veya alt ağ `/subscriptions/YourAzureSubscripton/resourceGroups/YourResourceGroup/providers/Microsoft.Network/virtualNetworks/YourARMVNet` adınızı metin kutularından kopyalayabilir ve ayrıca, gerekli gelen/giden ağ traffics ve yönetiminin Azure-SSIS IR olmamasını sağlamak Için VNET ve alt ağ yapılandırmalarından kimlik doğrulaması yapabilirsiniz.

![Azure-SSIS IR-doğrulama Kutucuğunuzu izleyin](media/monitor-integration-runtime/monitor-azure-ssis-integration-runtime-validate.png)

Azure-SSIS IR izleme sayfanızın **bağlantıyı Tanıla** kutucuğunda, bir pencere açmak Için bağlantıyı **Test** et bağlantısını seçebilirsiniz. buradan, Azure-SSIS IR ve ilgili paket/yapılandırma/veri depoları ile Yönetim Hizmetleri arasındaki bağlantıları, tam etkı alanı adı (FQDN)/IP adresi ve belirlenmiş bağlantı noktası aracılığıyla kontrol edebilirsiniz (bkz. [Azure-SSIS IR bağlantıları test](https://docs.microsoft.com/azure/data-factory/ssis-integration-runtime-diagnose-connectivity-faq)edin).

![Azure-SSIS IR tanılama Kutucuğunuzu izleyin](media/monitor-integration-runtime/monitor-azure-ssis-integration-runtime-diagnose.png)

Paketlerin Azure SQL yönetilen örneğiniz tarafından barındırılan ve Azure-SSIS IR paket depoları aracılığıyla yönetilen dosya sistemi/Azure dosyaları/SQL Server veritabanı 'nda (MSDB) depolandığı paket dağıtım modelini kullanıyorsanız, **paket depoları** kutucuğunu Azure-SSIS IR izleme sayfanızda görürsünüz (bkz. [Azure-SSIS IR dağıtım ayarlarınızı yapılandırma](https://docs.microsoft.com/azure/data-factory/tutorial-deploy-ssis-packages-azure#deployment-settings-page)). Bu kutucukta, Azure SQL yönetilen örneğiniz tarafından barındırılan dosya sistemi/Azure dosyaları/MSDB 'nin en üstünde yer alan Azure-SSIS IR paket depolarınız için ilgili bağlı hizmetleri yeniden yapılandırabileceğiniz bir pencere açmak için Azure-SSIS IR bağlı paket deposunun sayısını tanımlayarak bir bağlantı seçebilirsiniz.

![Azure-SSIS IR PAKETI Kutucuğunuzu izleyin](media/monitor-integration-runtime/monitor-azure-ssis-integration-runtime-package.png)

Azure-SSIS IR başlatma/durdurma/bakım/yükseltme ile ilgili sorunlar varsa, Azure-SSIS IR izleme sayfanızda ek bir **hata (ler)** kutucuğu görürsünüz. Bu kutucukta, bir pencere açmak için Azure-SSIS IR tarafından üretilen hataların sayısını tanımlayarak bir bağlantı seçebilirsiniz. burada bu hataları daha ayrıntılı olarak görebileceğiniz ve sorun giderme kılavuzumuzdaki önerilen çözümleri bulmak üzere kopyalayabilir (bkz. [Azure-SSIS IR sorunlarını giderme](https://docs.microsoft.com/azure/data-factory/ssis-integration-runtime-management-troubleshoot)).

### <a name="monitor-the-azure-ssis-integration-runtime-with-azure-monitor"></a>Azure Izleyici ile Azure-SSIS tümleştirme çalışma zamanını izleme

Azure Izleyici ile Azure-SSIS IR izlemek için bkz. [Azure izleyici Ile SSIS Işlemlerini izleme](https://docs.microsoft.com/azure/data-factory/monitor-using-azure-monitor#monitor-ssis-operations-with-azure-monitor).

### <a name="more-info-about-the-azure-ssis-integration-runtime"></a>Azure-SSIS Integration Runtime hakkında daha fazla bilgi

Azure-SSIS Integration Runtime hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Azure-SSIS Integration Runtime](concepts-integration-runtime.md#azure-ssis-integration-runtime). Bu makalede, Azure-SSIS IR dahil olmak üzere genel olarak tümleştirme çalışma zamanları hakkında kavramsal bilgiler verilmektedir. 
- [Öğretici: SSIS paketlerini Azure’a dağıtma](tutorial-create-azure-ssis-runtime-portal.md). Bu makalede, bir Azure-SSIS IR oluşturmak ve SSIS kataloğunu barındırmak için SQL veritabanı 'nı kullanması için adım adım yönergeler sağlanmaktadır. 
- [Nasıl yapılır: Azure-SSIS tümleştirme çalışma zamanı oluşturma](create-azure-ssis-integration-runtime.md). Bu makale öğreticide genişler ve SQL yönetilen örneği kullanma ve IR 'yi bir sanal ağa katma hakkında yönergeler sağlar. 
- [Azure-SSIS IR’yi yönetme](manage-azure-ssis-integration-runtime.md). Bu makale bir Azure-SSIS IR’yi durdurma, başlatma veya kaldırma işlemini gösterir. Ayrıca, IR’ye daha fazla düğüm ekleyerek Azure-SSIS IR’nizi ölçeklendirmeyi gösterir. 
- [Azure-SSIS IR’yi bir sanal ağa ekleyin](join-azure-ssis-integration-runtime-virtual-network.md). Bu makale Azure-SSIS IR’yi bir Azure sanal ağına ekleme hakkında kavramsal bilgiler sağlar. Ayrıca, Azure-SSIS IR sanal ağa katılabilmesi için sanal ağı yapılandırmak üzere Azure portal kullanma adımları da sağlar. 

## <a name="next-steps"></a>Sonraki adımlar
İşlem hatlarını farklı yollarla izlemek için aşağıdaki makalelere bakın: 

- [Hızlı başlangıç: Veri Fabrikası oluşturma](quickstart-create-data-factory-dot-net.md).
- [Data Factory işlem hatlarını izlemek için Azure Izleyicisini kullanın](monitor-using-azure-monitor.md)