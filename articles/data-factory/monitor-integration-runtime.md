---
title: Azure Veri Fabrikası'nda tümleştirme çalışma süresini izleme
description: Azure Veri Fabrikası'nda farklı türde tümleştirme çalışma süresini nasıl izleyeceğinizi öğrenin.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 07/25/2018
author: djpmsft
ms.author: daperlov
manager: anandsub
ms.openlocfilehash: 2399849b87e44c5cb70d2db987ae18d8d2d9c552
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79261144"
---
# <a name="monitor-an-integration-runtime-in-azure-data-factory"></a>Azure Data Factory'deki tümleştirme çalışma zamanını izleme  
**Tümleştirme çalışma süresi,** Azure Veri Fabrikası tarafından farklı ağ ortamlarında çeşitli veri tümleştirme özellikleri sağlamak için kullanılan bilgi işlem altyapısıdır. Veri Fabrikası tarafından sunulan üç tür tümleştirme çalışma süreleri vardır:

- Azure tümleştirme çalışma zamanı
- Kendinden konak tümleştirme çalışma zamanı
- Azure SSIS tümleştirmesi çalışma zamanı

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Tümleştirme çalışma zamanı (IR) örneğinin durumunu almak için aşağıdaki PowerShell komutunu çalıştırın: 

```powershell
Get-AzDataFactoryV2IntegrationRuntime -DataFactoryName MyDataFactory -ResourceGroupName MyResourceGroup -Name MyAzureIR -Status
``` 

Cmdlet, farklı entegrasyon çalışma zamanı türleri için farklı bilgiler döndürür. Bu makalede, her tümleştirme çalışma zamanı türü için özellikleri ve durumları açıklanmaktadır.  

## <a name="azure-integration-runtime"></a>Azure tümleştirme çalışma zamanı
Azure tümleştirme çalışma zamanının bilgi işlem kaynağı Azure'da tam olarak yönetilir. Aşağıdaki **tablo, Get-AzDataFactoryV2IntegrationRuntime** komutu tarafından döndürülen özellikler için açıklamalar sağlar:

### <a name="properties"></a>Özellikler
Aşağıdaki tablo, Azure tümleştirme çalışma zamanı için cmdlet tarafından döndürülen özelliklerin açıklamalarını sağlar:

| Özellik | Açıklama |
-------- | ------------- | 
| Adı | Azure tümleştirme çalışma zamanının adı. |  
| Durum | Azure tümleştirme çalışma zamanının durumu. | 
| Konum | Azure tümleştirme çalışma zamanının konumu. Azure tümleştirme çalışma zamanının konumu hakkında ayrıntılı bilgi [için](concepts-integration-runtime.md)bkz. |
| DataFactoryName | Azure tümleştirme çalışma zamanının ait olduğu veri fabrikasının adı. | 
| ResourceGroupName | Veri fabrikasının ait olduğu kaynak grubunun adı.  |
| Açıklama | Tümleştirme çalışma zamanının açıklaması.  |

### <a name="status"></a>Durum
Aşağıdaki tablo, Azure tümleştirme çalışma zamanının olası durumlarını sağlar:

| Durum | Yorumlar / Senaryolar | 
| ------ | ------------------ |
| Çevrimiçi | Azure tümleştirme çalışma zamanı çevrimiçi ve kullanıma hazır. | 
| Çevrimdışı | Azure tümleştirme çalışma süresi, iç hata nedeniyle çevrimdışıdır. |

## <a name="self-hosted-integration-runtime"></a>Kendinden konak tümleştirme çalışma zamanı
Bu bölümde Get-AzDataFactoryV2IntegrationRuntime cmdlet tarafından döndürülen özellikler için açıklamalar verilmektedir. 

> [!NOTE] 
> Döndürülen özellikler ve durum, genel olarak barındırılan tümleştirme çalışma zamanı ve çalışma zamanındaki her düğüm hakkında bilgi içerir.  

### <a name="properties"></a>Özellikler

Aşağıdaki **tablo, her düğüm**için izleme özellikleri açıklamaları sağlar:

| Özellik | Açıklama | 
| -------- | ----------- | 
| Adı | Kendi kendine barındırılan tümleştirme çalışma zamanının ve onunla ilişkili düğümlerin adı. Düğüm, kendi kendine barındırılan tümleştirme çalışma zamanı yüklü olan şirket içi bir Windows makinesidir. |  
| Durum | Genel olarak barındırılan tümleştirme çalışma zamanının ve her düğümün durumu. Örnek: Çevrimiçi/Çevrimdışı/Sınırlı/vb. Bu durumlar hakkında bilgi için bir sonraki bölüme bakın. | 
| Sürüm | Kendi kendine barındırılan tümleştirme çalışma zamanı ve her düğümün sürümü. Kendi kendine barındırılan tümleştirme çalışma süresinin sürümü, gruptaki düğümlerin çoğunluğunun sürümüne göre belirlenir. Kendi kendine barındırılan tümleştirme çalışma zamanı kurulumunda farklı sürümlere sahip düğümler varsa, yalnızca mantıksal kendi kendine barındırılan tümleştirme çalışma zamanı işleviyle aynı sürüm numarasına sahip düğümler düzgün bir şekilde. Diğerleri sınırlı moddadır ve el ile güncelleştirilmelidir (yalnızca otomatik güncelleştirme başarısız olursa). | 
| Kullanılabilir bellek | Kendi barındırılan tümleştirme çalışma zamanı düğümünde kullanılabilir bellek. Bu değer neredeyse gerçek zamanlı anlık görüntüdür. | 
| CPU kullanımı | Kendi kendine barındırılan tümleştirme çalışma zamanı düğümünün CPU kullanımı. Bu değer neredeyse gerçek zamanlı anlık görüntüdür. |
| Ağ (Dahili/Çıkış) | Kendi kendine barındırılan tümleştirme çalışma zamanı düğümünün ağ kullanımı. Bu değer neredeyse gerçek zamanlı anlık görüntüdür. | 
| Eşzamanlı İşler (Çalışan / Limit) | **Koşmak**. Her düğümde çalışan iş veya görev sayısı. Bu değer neredeyse gerçek zamanlı anlık görüntüdür. <br/><br/>**Sınır .** Sınır, her düğüm için en büyük eşzamanlı işleri belirtir. Bu değer makine boyutuna göre tanımlanır. CPU, bellek veya ağ yeterince kullanılmadığında bile etkinlikler zamanlandığında, gelişmiş senaryolarda eşzamanlı iş yürütmeyi ölçeklendirme sınırını artırabilirsiniz. Bu özellik, tek düğümlü kendi kendine barındırılan tümleştirme çalışma süresiyle de kullanılabilir. |
| Rol | Çok düğümlü kendi kendine barındırılan tümleştirme çalışma zamanında iki tür rol vardır – sevk çisi ve çalışan. Tüm düğümler işçidir, bu da hepsinin işleri yürütmek için kullanılabildiği anlamına gelir. Bulut hizmetlerinden görevleri/işleri çekmek ve bunları farklı işçi düğümlerine göndermek için kullanılan tek bir sevk düğümü vardır. Sevk eden düğüm aynı zamanda bir işçi düğümüdür. |

Özelliklerin bazı ayarları, kendi kendine barındırılan tümleştirme çalışma zamanında iki veya daha fazla düğüm olduğunda (diğer bir deyişle, bir ölçeklendirme senaryosunda) daha anlamlıdır.

#### <a name="concurrent-jobs-limit"></a>Eşzamanlı iş sınırı

Eşzamanlı işler sınırının varsayılan değeri makine boyutuna göre ayarlanır. Bu değeri hesaplamak için kullanılan faktörler RAM miktarına ve makinenin CPU çekirdeği sayısına bağlıdır. Yani daha fazla çekirdek ve daha fazla bellek, eşzamanlı işlerin varsayılan sınırı daha yüksek.

Düğüm sayısını artırarak ölçeklendirin. Düğüm sayısını artırdığınızda, eşzamanlı iş sınırı, kullanılabilir tüm düğümlerin eşzamanlı iş sınırı değerlerinin toplamıdır.  Örneğin, bir düğüm en fazla on iki eşzamanlı iş çalıştırmanıza izin veriyorsa, üç benzer düğüm daha ekleyerek en fazla 48 eşzamanlı iş çalıştırmanızı sağlar (yani 4 x 12). Eşzamanlı iş sınırını yalnızca her düğümde varsayılan değerleriçeren düşük kaynak kullanımı gördüğünüzde artırmanızı öneririz.

Azure portalında hesaplanan varsayılan değeri geçersiz kılabilirsiniz. Yazar > Bağlantıları > Tümleştirme Çalışma Süreleri > > Düğüm >leri Eş zamanlı iş değerini değiştir'i seçin. PowerShell [update-Azdatafactoryv2integrationruntimenode](https://docs.microsoft.com/powershell/module/az.datafactory/update-Azdatafactoryv2integrationruntimenode#examples) komutunu da kullanabilirsiniz.
  
### <a name="status-per-node"></a>Durum (düğüm başına)
Aşağıdaki tablo, kendi kendine barındırılan tümleştirme çalışma zamanı düğümünün olası durumlarını sağlar:

| Durum | Açıklama |
| ------ | ------------------ | 
| Çevrimiçi | Düğüm Veri Fabrikası hizmetine bağlıdır. |
| Çevrimdışı | Düğüm çevrimdışı. |
| Yükseltme | Düğüm otomatik olarak güncelleniyor. |
| Sınırlı | Bir bağlantı sorunu nedeniyle. HTTP bağlantı noktası 8050 sorunu, servis veri meskeni bağlantısı sorunu veya kimlik bilgisi eşitleme sorunu nedeniyle olabilir. |
| Etkin değil | Düğüm, diğer çoğunluk düğümlerinin yapılandırmasından farklı bir yapılandırmadadır. |

Düğüm, diğer düğümlere bağlanamıyorsa etkin olmayabilir.

### <a name="status-overall-self-hosted-integration-runtime"></a>Durum (genel olarak barındırılan tümleştirme çalışma zamanı)
Aşağıdaki tablo, kendi kendine barındırılan tümleştirme çalışma zamanının olası durumlarını sağlar. Bu durum, çalışma süresine ait tüm düğümlerin durumlarına bağlıdır. 

| Durum | Açıklama |
| ------ | ----------- | 
| İhtiyaç Kaydı | Bu barındırılan tümleştirme çalışma süresine henüz düğüm kaydedilmemiştir. |
| Çevrimiçi | Tüm düğümler çevrimiçi. |
| Çevrimdışı | Hiçbir düğüm çevrimiçi değildir. |
| Sınırlı | Bu kendi kendine barındırılan tüm düğümleri sağlıklı bir durumda değildir. Bu durum, bazı düğümlerin kapana kısAlınabileceğine dair bir uyarıdır. Bu durum, sevk/işçi düğümündeki bir kimlik bilgisi eşitleme sorunundan kaynaklanıyor olabilir. |

Cmdlet'in yürütülmesi sırasında ayrıntılı kendi kendine barındırılan tümleştirme çalışma zamanı özelliklerini ve anlık görüntü değerlerini içeren JSON yükünü almak için **Get-AzDataFactoryV2IntegrationRuntimeMetric** cmdlet'i kullanın.

```powershell
Get-AzDataFactoryV2IntegrationRuntimeMetric -name $integrationRuntimeName -ResourceGroupName $resourceGroupName -DataFactoryName $dataFactoryName | ConvertTo-Json 
```

Örnek çıktı (bu kendi kendine barındırılan tümleştirme çalışma süresiyle ilişkili iki düğüm olduğunu varsayar):

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
Azure-SSIS tümleştirme çalışma süresi, SSIS paketlerinizi çalıştırmak için ayrılmış tam olarak yönetilen bir Azure sanal makine kümesidir . Azure Veri Fabrikası'nın diğer etkinliklerini çalıştırmaz. Bir kez sağlanan, özelliklerini sorgulayabilir ve genel / düğüm özgü durumlarını izleyebilirsiniz.

### <a name="properties"></a>Özellikler

| Özellik / Durum | Açıklama |
| --------------- | ----------- |
| CreateTime | Azure-SSIS tümleştirme çalışma sürenizin oluşturulduğu UTC zamanı. |
| Düğümler | Azure-SSIS tümleştirme çalışma sürenizin düğüme özgü durumlarla (başlangıç/kullanılabilir/geri dönüşüm/kullanılamaz) ve işlem uygulanabilir hatalarla ayrılmış/kullanılabilir düğümleri. |
| Diğer Hatalar | Azure-SSIS tümleştirme çalışma zamanınızda düğüme özgü olmayan eylem hataları. |
| Son İşlem | Azure-SSIS tümleştirme çalışma zamanınızdaki son başlatma/durdurma işleminin sonucu, başarısız olursa işlem hatası(lar) ile birlikte. |
| Durum | Azure-SSIS tümleştirme çalışma sürenizin genel durumu (başlangıç/başlangıç/başlatma/durdurma/durdurma/durdurma). |
| Konum | Azure-SSIS tümleştirme çalışma sürenizin konumu. |
| DüğümBoyutu | Azure-SSIS tümleştirme çalışma sürenizin her düğümünün boyutu. |
| Düğüm Sayısı | Azure-SSIS tümleştirme çalışma zamanınızdaki düğüm sayısı. |
| MaxParallelExecutionsPerNode | Azure-SSIS tümleştirme çalışma zamanınızda düğüm başına paralel yürütme sayısı. |
| KatalogServerEndpoint | SSISDB'yi barındırmak için varolan Azure SQL Veritabanı/Yönetilen Örnek sunucunuzun bitiş noktası. |
| KatalogAdminUserName | Mevcut Azure SQL Veritabanı/Yönetilen Örnek sunucunuzun yönetici kullanıcı adı. Veri Fabrikası hizmeti, bu bilgileri sizin adınıza SSISDB'yi hazırlamak ve yönetmek için kullanır. |
| KatalogAdminPassword | Mevcut Azure SQL Veritabanı/Yönetilen Örnek sunucunuzun yönetici parolası. |
| KatalogFiyatlandırmaTier | SSISDB'nin fiyatlandırma katmanı, mevcut Azure SQL Veritabanı sunucunuz tarafından barındırılır.  SSISDB barındırma Azure SQL Veritabanı Yönetilen Örnek için geçerli değildir. |
| VNetid | Azure-SSIS tümleştirmeçalışma sürenizin katılması için sanal ağ kaynak kimliği. |
| Alt ağ | Katılmak için Azure-SSIS tümleştirme çalışma sürenizin alt ağ adı. |
| Kimlik | Azure-SSIS tümleştirme çalışma sürenizin kaynak kimliği. |
| Tür | Azure-SSIS tümleştirme çalışma sürenizin türü (Yönetilen/Kendi Kendine Barındırılan) türü. |
| ResourceGroupName | Veri fabrikanızın ve Azure-SSIS tümleştirme çalışma saatinin oluşturulduğu Azure Kaynak Grubunuzun adı. |
| DataFactoryName | Azure veri fabrikanızın adı. |
| Adı | Azure-SSIS tümleştirme çalışma sürenizin adı. |
| Açıklama | Azure-SSIS tümleştirme çalışma sürenizin açıklaması. |

  
### <a name="status-per-node"></a>Durum (düğüm başına)

| Durum | Açıklama |
| ------ | ----------- | 
| Başlatılıyor | Bu düğüm hazırlanıyor. |
| Kullanılabilir | Bu düğüm, SSIS paketlerini dağıtmanız/yürütmeniz için hazırdır. |
| Geri dönüşüm | Bu düğüm onarılıyor/yeniden başlatılıyor. |
| Kullanılamaz | Bu düğüm, SSIS paketlerini dağıtmanız/yürütmeniz için hazır değildir ve çözebileceğiniz işlem yapılabilir hatalar/sorunlar vardır. |

### <a name="status-overall-azure-ssis-integration-runtime"></a>Durum (genel Azure-SSIS tümleştirme çalışma zamanı)

| Genel durum | Açıklama | 
| -------------- | ----------- | 
| Başlangıç | Azure-SSIS tümleştirme çalışma sürenizin düğümleri tahsis edilmedi/hazırlanmadı. | 
| Başlatılıyor | Azure-SSIS tümleştirme çalışma sürenizin düğümleri tahsis ediliyor/hazırlandı ve faturalandırma başlatıldı. |
| Başlatıldı | Azure-SSIS tümleştirme çalışma sürenizin düğümleri ayrıldı/hazırlandı ve SSIS paketlerini dağıtmanız/yürütmeniz için hazır. |
| Durduruluyor  | Azure-SSIS tümleştirme çalışma sürenizin düğümleri serbest bırakılıyor. |
| Durduruldu | Azure-SSIS tümleştirme çalışma sürenizin düğümleri serbest bırakıldı ve faturalandırma durduruldu. |

### <a name="monitor-the-azure-ssis-integration-runtime-in-the-azure-portal"></a>Azure portalında Azure-SSIS tümleştirme çalışma süresini izleme

Aşağıdaki ekran görüntüleri, izlenecek Azure-SSIS IR'nin nasıl seçilebildiğini gösterir ve görüntülenen bilgilerin bir örneğini sağlar.

![İzlemek için Azure-SSIS tümleştirme çalışma süresini seçin](media/monitor-integration-runtime/monitor-azure-ssis-ir-image1.png)

![Azure-SSIS tümleştirme çalışma süresi hakkındaki bilgileri görüntüleme](media/monitor-integration-runtime/monitor-azure-ssis-ir-image2.png)

### <a name="monitor-the-azure-ssis-integration-runtime-with-powershell"></a>PowerShell ile Azure-SSIS tümleştirme çalışma süresini izleme

Azure-SSIS IR'nin durumunu denetlemek için aşağıdaki örnekteki gibi bir komut dosyası kullanın.

```powershell
Get-AzDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSSISName -ResourceGroupName $ResourceGroupName -Status
```

### <a name="more-info-about-the-azure-ssis-integration-runtime"></a>Azure-SSIS tümleştirme çalışma süresi hakkında daha fazla bilgi

Azure-SSIS tümleştirme çalışma süresi hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Azure-SSIS Tümleştirme Çalışma Zamanı.](concepts-integration-runtime.md#azure-ssis-integration-runtime) Bu makalede, Azure-SSIS IR dahil olmak üzere genel olarak tümleştirme çalışma saatleri hakkında kavramsal bilgiler sağlar. 
- [Öğretici: SSIS paketlerini Azure’a dağıtma](tutorial-create-azure-ssis-runtime-portal.md). Bu makale bir Azure-SSIS IR oluşturmaya ilişkin adım adım yönergeler sağlar ve SSIS kataloğunu barındırmak için bir Azure SQL veritabanı kullanır. 
- [Nasıl yapılır: Azure-SSIS tümleştirme çalışma zamanı oluşturma](create-azure-ssis-integration-runtime.md). Bu makale, öğreticiyi genişletir ve Azure SQL Veritabanı Yönetilen Örneği'ni kullanma ve IR'yi sanal bir ağa katılma hakkında yönergeler sağlar. 
- [Azure-SSIS IR’yi yönetme](manage-azure-ssis-integration-runtime.md). Bu makale bir Azure-SSIS IR’yi durdurma, başlatma veya kaldırma işlemini gösterir. Ayrıca, IR’ye daha fazla düğüm ekleyerek Azure-SSIS IR’nizi ölçeklendirmeyi gösterir. 
- [Azure-SSIS IR’yi bir sanal ağa ekleyin](join-azure-ssis-integration-runtime-virtual-network.md). Bu makale Azure-SSIS IR’yi bir Azure sanal ağına ekleme hakkında kavramsal bilgiler sağlar. Ayrıca, Azure-SSIS IR'nin sanal ağa katılabilmesi için sanal ağı yapılandırmak için Azure portalını kullanmak için adımlar da sağlar. 

## <a name="next-steps"></a>Sonraki adımlar
Boru hatlarını farklı şekillerde izlemek için aşağıdaki makalelere bakın: 

- [Quickstart: bir veri fabrikası oluşturun.](quickstart-create-data-factory-dot-net.md)
- [Veri Fabrikası ardışık hatlarını izlemek için Azure Monitörünü kullanma](monitor-using-azure-monitor.md)
