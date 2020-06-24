---
title: Etkinlik performansını en iyi duruma getirme özelliklerini Kopyala
description: Azure Data Factory. ' de kopyalama etkinliği performansını iyileştirmenize yardımcı olan temel özellikler hakkında bilgi edinin
services: data-factory
documentationcenter: ''
ms.author: jingwang
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 06/15/2020
ms.openlocfilehash: dfd439affe488805b4645211477c6d32bbbe7489
ms.sourcegitcommit: 99d016949595c818fdee920754618d22ffa1cd49
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/15/2020
ms.locfileid: "84770943"
---
# <a name="copy-activity-performance-optimization-features"></a>Etkinlik performansını en iyi duruma getirme özelliklerini Kopyala

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bu makalede, Azure Data Factory ' de kullanabileceğiniz etkinlik performansını en iyi duruma getirme özellikleri özetlenmektedir.

## <a name="data-integration-units"></a>Veri Tümleştirme Birimleri

Veri tümleştirme birimi, Azure Data Factory içinde tek bir birimin gücünü (CPU, bellek ve ağ kaynak ayırma birleşimi) temsil eden bir ölçüdür. Veri tümleştirme birimi yalnızca [Azure tümleştirme çalışma zamanı](concepts-integration-runtime.md#azure-integration-runtime)için geçerlidir, ancak [Şirket içinde barındırılan tümleştirme çalışma zamanı](concepts-integration-runtime.md#self-hosted-integration-runtime)için geçerli değildir.

Kopyalama etkinliği çalıştırmasını güçlendiren izin verilen, **2 ile 256 arasındadır**. Belirtilmemişse veya Kullanıcı arabiriminde "otomatik" seçeneğini belirlerseniz, Data Factory kaynak havuzu çiftinize ve veri düzenine göre en uygun DIU ayarını dinamik olarak uygular. Aşağıdaki tablo, farklı kopyalama senaryolarında desteklenen DIU aralıklarını ve varsayılan davranışı listeler:

| Kopyalama senaryosu | Desteklenen DIU aralığı | Hizmete göre belirlenen varsayılan değer |
|:--- |:--- |---- |
| Dosya depoları arasında |- **Veya tek dosyadan Kopyala**: 2-4 <br>- **Ve birden çok dosyaya kopyalama**: 2-256, dosyaların sayısına ve boyutuna bağlı olarak <br><br>Örneğin, 4 büyük dosya içeren bir klasörden veri kopyalar ve hiyerarşiyi korumayı seçerseniz, en yüksek etkin DIU 16 ' dır; dosyayı birleştirmeyi seçtiğinizde, en fazla etkin DIU 4 ' tir. |Dosyaların sayısına ve boyutuna bağlı olarak 4 ile 32 arasında |
| Dosya deposundan dosya olmayan depoya |- **Tek dosyadan Kopyala**: 2-4 <br/>- **Birden çok dosyadan kopyalama**: 2-256, dosyaların sayısına ve boyutuna bağlı olarak <br/><br/>Örneğin, 4 büyük dosya içeren bir klasörden veri kopyalarsanız, en yüksek etkin DIU 16 ' dır. |- Havuz katmanına (DTU/ru) ve kaynak dosya düzenine bağlı olarak, **Azure SQL veritabanı 'na veya Azure Cosmos DB**: 4 ile 16 arasında kopyalama<br>- PolyBase veya COPY deyimlerini kullanarak **Azure SYNAPSE Analytics 'e kopyalama** : 2<br>-Diğer senaryo: 4 |
| Dosya depolamadan dosya deposuna |- **Bölüm-seçenek etkinleştirilmiş veri depolarından** ( [Oracle](connector-oracle.md#oracle-as-source) / [Netezza](connector-netezza.md#netezza-as-source) / [Teradata](connector-teradata.md#teradata-as-source)dahil) kopyalama: bir klasöre yazarken 2-256 ve tek bir dosyaya yazarken 2-4. Kaynak veri bölümü başına, en fazla 4 DIUs kullanabilirsiniz.<br>- **Diğer senaryolar**: 2-4 |- **REST veya http 'Den Kopyala**: 1<br/>- Yüklemeyi kaldırma kullanarak **Amazon Redshift 'Tan Kopyala** : 2<br>- **Diğer senaryo**: 4 |
| Dosya olmayan depolar arasında |- **Bölüm-seçenek etkinleştirilmiş veri depolarından** ( [Oracle](connector-oracle.md#oracle-as-source) / [Netezza](connector-netezza.md#netezza-as-source) / [Teradata](connector-teradata.md#teradata-as-source)dahil) kopyalama: bir klasöre yazarken 2-256 ve tek bir dosyaya yazarken 2-4. Kaynak veri bölümü başına, en fazla 4 DIUs kullanabilirsiniz.<br/>- **Diğer senaryolar**: 2-4 |- **REST veya http 'Den Kopyala**: 1<br>- **Diğer senaryo**: 4 |

Kopyalama etkinliği izleme görünümü veya etkinlik çıkışında her bir kopya için kullanılan orus 'yi görebilirsiniz. Daha fazla bilgi için bkz. [kopyalama etkinliği izleme](copy-activity-monitoring.md). Bu varsayılanı geçersiz kılmak için, özellik için aşağıdaki gibi bir değer belirtin `dataIntegrationUnits` . Kopyalama işleminin çalışma zamanında kullandığı *gerçek sayı* , veri düzenlerinize bağlı olarak yapılandırılan değere eşit veya ondan daha az.

**Kullanılan mus \* kopyalama süresi \* birim fiyatı/Diu-saat**üzerinden ücretlendirilecektir. Geçerli fiyatlara [buradan](https://azure.microsoft.com/pricing/details/data-factory/data-pipeline/)bakın. Yerel para birimi ve ayrı ayırt sayma, abonelik türü başına uygulanabilir.

**Örneğinde**

```json
"activities":[
    {
        "name": "Sample copy activity",
        "type": "Copy",
        "inputs": [...],
        "outputs": [...],
        "typeProperties": {
            "source": {
                "type": "BlobSource",
            },
            "sink": {
                "type": "AzureDataLakeStoreSink"
            },
            "dataIntegrationUnits": 128
        }
    }
]
```

## <a name="self-hosted-integration-runtime-scalability"></a>Şirket içinde barındırılan tümleştirme çalışma zamanı ölçeklenebilirliği

Daha yüksek aktarım hızı elde etmek isterseniz, şirket içinde barındırılan IR 'yi ölçeklendirebilir veya ölçeklendirebilirsiniz:

- Şirket içinde barındırılan IR düğümündeki CPU ve kullanılabilir bellek tam olarak kullanılmaz, ancak eşzamanlı işlerin yürütülmesi sınıra ulaştığı takdirde, bir düğümde çalışabilecek eşzamanlı işlerin sayısını artırarak ölçeklendirmelisiniz.  Yönergeler için [buraya](create-self-hosted-integration-runtime.md#scale-up) bakın.
- Öte yandan, CPU şirket içinde barındırılan IR düğümünde yüksek veya kullanılabilir bellek düşükse, birden çok düğüm genelinde yükün ölçeğini genişletmek için yeni bir düğüm ekleyebilirsiniz.  Yönergeler için [buraya](create-self-hosted-integration-runtime.md#high-availability-and-scalability) bakın.

Aşağıdaki senaryolarda, tek kopya etkinliği yürütme birden çok şirket içinde barındırılan IR düğümünden yararlanabilir:

- Dosya tabanlı mağazalardan, dosyaların sayısına ve boyutuna bağlı olarak verileri kopyalayın.
- Veri bölümlerinin sayısına bağlı olarak, Bölüm seçenekleri etkinleştirilmiş veri deposundan ( [Oracle](connector-oracle.md#oracle-as-source), [Netezza](connector-netezza.md#netezza-as-source), [Teradata](connector-teradata.md#teradata-as-source), [SAP HANA](connector-sap-hana.md#sap-hana-as-source), [SAP tablosu](connector-sap-table.md#sap-table-as-source)ve [SAP açık hub](connector-sap-business-warehouse-open-hub.md#sap-bw-open-hub-as-source)dahil) verileri kopyalayın.

## <a name="parallel-copy"></a>Paralel kopya

Kopyalama etkinliğinin `parallelCopies` kullanmasını istediğiniz paralellik belirtmek için kopyalama etkinliğinde paralel kopyalama (özellik) ayarlayabilirsiniz. Bu özelliği, kopyalama etkinliğindeki en fazla iş parçacığı sayısı olarak, kaynağınızdan okunan veya kanal veri depolarınız paralel olarak yazabilirsiniz.

Paralel kopya, [veri tümleştirme birimlerine](#data-integration-units) veya [ŞIRKET içinde barındırılan IR düğümlerine](#self-hosted-integration-runtime-scalability)diklik edilir. Tüm orus veya şirket içinde barındırılan IR düğümlerinde sayılır.

Her kopyalama etkinliği çalıştırması için, varsayılan olarak Azure Data Factory, kaynak havuzu çiftine ve veri düzenine göre en iyi paralel kopyalama ayarını dinamik olarak uygular. 

> [!TIP]
> Paralel kopyanın varsayılan davranışı genellikle, ADF tarafından otomatik olarak belirlenen ve kaynak havuzu çiftinize, veri düzenine ve en iyi barındırılan IR CPU/bellek/düğüm sayısına bağlı olarak otomatik olarak belirlenen en iyi aktarım hızını verir. Paralel kopyayı ayarlama sırasında [etkinlik performansını kopyalama sorunlarını giderme](copy-activity-performance-troubleshooting.md) bölümüne bakın.

Aşağıdaki tabloda, paralel kopyalama davranışı listelenmektedir:

| Kopyalama senaryosu | Paralel kopyalama davranışı |
| --- | --- |
| Dosya depoları arasında | `parallelCopies`**dosya düzeyinde**paralellik belirler. Her bir dosya içindeki parçalama otomatik ve şeffaf bir şekilde gerçekleşir. Verileri paralel olarak yüklemek için belirli bir veri deposu türü için en iyi uygun öbek boyutunu kullanmak üzere tasarlanmıştır. <br/><br/>Kopya etkinliğinin gerçek sayısı, çalışma zamanında kullanılan, sahip olduğunuz dosya sayısından daha fazla değil. Kopyalama davranışı dosya havuzunda **birleştirme dosyası** ise kopyalama etkinliği dosya düzeyinde paralellik özelliğinden yararlanamaz. |
| Dosya deposundan dosya olmayan depoya | -Verileri Azure SQL veritabanı 'na veya Azure Cosmos DB kopyalarken, varsayılan paralel kopya Ayrıca havuz katmanına (DTU 'Lar/ru sayısı) göre değişir.<br>-Verileri Azure tablosuna kopyalarken, varsayılan paralel kopya 4 ' dir. |
| Dosya depolamadan dosya deposuna | -Seçenek etkinleştirilmiş veri deposundan ( [Oracle](connector-oracle.md#oracle-as-source), [Netezza](connector-netezza.md#netezza-as-source), [Teradata](connector-teradata.md#teradata-as-source), [SAP HANA](connector-sap-hana.md#sap-hana-as-source), [SAP tablosu](connector-sap-table.md#sap-table-as-source)ve [SAP açık hub](connector-sap-business-warehouse-open-hub.md#sap-bw-open-hub-as-source)dahil) veri kopyalarken, varsayılan paralel kopya 4 ' dir. Kopya etkinliğinin gerçek sayısı, çalışma zamanında kullanılan, sahip olduğunuz veri bölümlerinin sayısından fazla değil. Şirket içinde barındırılan Integration Runtime ve Azure Blob/ADLS 2. ' e kopyalama sırasında, en fazla etkin paralel kopya, IR düğümü başına 4 veya 5 ' tir.<br>-Diğer senaryolar için paralel kopya etkili olmaz. Paralellik belirtilmiş olsa bile, uygulanmaz. |
| Dosya olmayan depolar arasında | -Verileri Azure SQL veritabanı 'na veya Azure Cosmos DB kopyalarken, varsayılan paralel kopya Ayrıca havuz katmanına (DTU 'Lar/ru sayısı) göre değişir.<br/>-Seçenek etkinleştirilmiş veri deposundan ( [Oracle](connector-oracle.md#oracle-as-source), [Netezza](connector-netezza.md#netezza-as-source), [Teradata](connector-teradata.md#teradata-as-source), [SAP HANA](connector-sap-hana.md#sap-hana-as-source), [SAP tablosu](connector-sap-table.md#sap-table-as-source)ve [SAP açık hub](connector-sap-business-warehouse-open-hub.md#sap-bw-open-hub-as-source)dahil) veri kopyalarken, varsayılan paralel kopya 4 ' dir.<br>-Verileri Azure tablosuna kopyalarken, varsayılan paralel kopya 4 ' dir. |

Veri mağazalarınızı barındıran makinelerde yükü denetlemek veya kopyalama performansını ayarlamak için varsayılan değeri geçersiz kılabilir ve özellik için bir değer belirtebilirsiniz `parallelCopies` . Değer 1 ' den büyük veya buna eşit bir tamsayı olmalıdır. Çalışma zamanında, en iyi performans için kopyalama etkinliği, ayarladığınız değerden küçük veya bu değere eşit bir değer kullanır.

Özelliği için bir değer belirttiğinizde `parallelCopies` , kaynak ve havuz Veri depolarındaki yük artışını hesapta yapın. Ayrıca, kopyalama etkinliği tarafından güçde bulunursa, şirket içinde barındırılan tümleştirme çalışma zamanına yönelik yük artışını de göz önünde bulundurun. Bu yük artışı, özellikle aynı veri deposunda çalışan aynı etkinliklerin birden çok etkinliğiniz veya eş zamanlı çalıştırmaları olduğunda gerçekleşir. Veri deposunun veya şirket içinde barındırılan tümleştirme çalışma zamanının yük ile azaldığını fark ederseniz, `parallelCopies` yükü ortadan kaldırmak için değeri azaltın.

**Örneğinde**

```json
"activities":[
    {
        "name": "Sample copy activity",
        "type": "Copy",
        "inputs": [...],
        "outputs": [...],
        "typeProperties": {
            "source": {
                "type": "BlobSource",
            },
            "sink": {
                "type": "AzureDataLakeStoreSink"
            },
            "parallelCopies": 32
        }
    }
]
```

## <a name="staged-copy"></a>Hazırlanmış kopya

Bir kaynak veri deposundan bir havuz veri deposuna veri kopyaladığınızda, blob Storage 'ı geçici bir hazırlama deposu olarak kullanmayı tercih edebilirsiniz. Hazırlama, özellikle aşağıdaki durumlarda yararlı olur:

- **Çeşitli veri depolarından verileri, PolyBase aracılığıyla Azure SYNAPSE Analytics 'e (eski adıyla SQL veri ambarı) almak istiyorsunuz.** Azure SYNAPSE Analytics, Azure SYNAPSE Analytics 'e büyük miktarda veri yüklemek için yüksek performanslı bir mekanizma olarak PolyBase 'i kullanır. Kaynak verilerin blob depolaması veya Azure Data Lake Store olması ve ek ölçütlere uyması gerekir. BLOB depolama veya Azure Data Lake Store dışında bir veri deposundan veri yüklediğinizde, veri kopyalamayı, geçici hazırlama blob depolaması aracılığıyla etkinleştirebilirsiniz. Bu durumda Azure Data Factory, PolyBase 'in gereksinimlerini karşıladığından emin olmak için gerekli veri dönüştürmelerini gerçekleştirir. Daha sonra, Azure SYNAPSE Analytics 'e verileri verimli bir şekilde yüklemek için PolyBase 'i kullanır. Daha fazla bilgi için bkz. [Azure SQL veri ambarı 'na veri yüklemek Için PolyBase kullanma](connector-azure-sql-data-warehouse.md#use-polybase-to-load-data-into-azure-sql-data-warehouse).
- **Bazen bir karma veri hareketini (yani, şirket içi veri deposundan bir bulut veri deposuna kopyalamak için) yavaş bir ağ bağlantısı üzerinden gerçekleştirme işlemi biraz zaman alır.** Performansı artırmak için, hazırlanan kopyayı, verileri bulutta hazırlama veri deposuna taşımak daha az zaman alması amacıyla Şirket içindeki verileri sıkıştırmak için kullanabilirsiniz. Daha sonra, hedef veri deposuna yüklemeden önce hazırlama deposundaki verileri açabilir.
- **Şirket BT ilkeleri nedeniyle güvenlik duvarınızdaki bağlantı noktası 80 ve bağlantı noktası 443 dışındaki bağlantı noktalarını açmak istemezsiniz.** Örneğin, şirket içi bir veri deposundan verileri bir Azure SQL veritabanı havuzuna veya bir Azure SYNAPSE Analytics havuzuna kopyaladığınızda, bağlantı noktası 1433 üzerinde giden TCP iletişimini hem Windows Güvenlik Duvarı hem de kurumsal güvenlik duvarı için etkinleştirmeniz gerekir. Bu senaryoda, hazırlanan kopya, ilk olarak şirket içinde barındırılan tümleştirme çalışma zamanından yararlanarak, önce verileri HTTP veya 443 numaralı bağlantı noktasında HTTP veya HTTPS üzerinden BLOB depolama hazırlama örneğine kopyalayabilir. Ardından, BLOB depolama alanı hazırlamadaki verileri SQL veritabanı veya Azure SYNAPSE Analytics 'e yükleyebilirsiniz. Bu akışta 1433 numaralı bağlantı noktasını etkinleştirmeniz gerekmez.

### <a name="how-staged-copy-works"></a>Aşamalı kopya nasıl çalışacaktır?

Hazırlama özelliğini etkinleştirdiğinizde, önce veriler kaynak veri deposundan hazırlama BLOB depolama alanına kopyalanır (kendinizinkini getirin). Ardından, veriler hazırlama veri deposundan havuz veri deposuna kopyalanır. Azure Data Factory, sizin için iki aşamalı akışı otomatik olarak yönetir. Azure Data Factory, veri taşıma işlemi tamamlandıktan sonra hazırlama depolamadan geçici verileri de temizler.

![Hazırlanmış kopya](media/copy-activity-performance/staged-copy.png)

Veri hareketini bir hazırlama deposu kullanarak etkinleştirdiğinizde, verileri kaynak veri deposundan bir ara veya hazırlama veri deposuna taşımadan önce verilerin sıkıştırılıp sıkıştırılmayacağını belirtebilir ve ardından verileri bir geçici veya hazırlama veri deposundan havuz veri deposuna taşımadan önce sıkıştırması açılır.

Şu anda, otomatik olarak barındırılan farklı IRS 'ler arasında bağlı olan iki veri deposu arasında veri kopyalayamazsınız, ancak hazırlanmamış kopya olmadan, verileri kopyalayamazsınız. Bu tür senaryolar için, kaynaktan hazırlama ve daha sonra hazırlama durumundan havuza kopyalamak üzere iki açık zincirleme kopyalama etkinliği yapılandırabilirsiniz.

### <a name="configuration"></a>Yapılandırma

Hedef veri deposuna yüklemeden önce, verilerin blob depolamada hazırlanması isteyip istemediğinizi belirtmek için kopyalama etkinliğinde **Enablehazırlama** ayarını yapılandırın. **Enablehazırlama** `TRUE` ' ı ayarladığınızda, aşağıdaki tabloda listelenen ek özellikleri belirtin. Ayrıca, yoksa, hazırlama için bir Azure depolama veya depolama paylaşılan erişim imzası ile bağlantılı hizmet oluşturmanız gerekir.

| Özellik | Açıklama | Varsayılan değer | Gerekli |
| --- | --- | --- | --- |
| Enablehazırlama |Verileri bir geçici hazırlama deposu aracılığıyla kopyalamak isteyip istemediğinizi belirtin. |False |No |
| linkedServiceName |Geçici hazırlama deposu olarak kullandığınız depolama örneğine başvuran bir [Azurestorage](connector-azure-blob-storage.md#linked-service-properties) Linked hizmetinin adını belirtin. <br/><br/> PolyBase aracılığıyla Azure SYNAPSE Analytics 'e veri yüklemek için paylaşılan erişim imzasıyla depolama kullanamazsınız. Diğer tüm senaryolarda kullanabilirsiniz. |Yok |Evet, **Enablehazırlama** true olarak ayarlandığında |
| yol |Hazırlanan verileri içermesini istediğiniz BLOB depolama yolunu belirtin. Bir yol sağlamazsanız, hizmet geçici verileri depolamak için bir kapsayıcı oluşturur. <br/><br/> Yalnızca bir paylaşılan erişim imzasıyla depolama kullanırsanız veya geçici verilerin belirli bir konumda olmasını istiyorsanız bir yol belirtin. |Yok |No |
| enableCompression |Verilerin hedefe kopyalanmadan önce sıkıştırılması gerekip gerekmediğini belirtir. Bu ayar, aktarılmakta olan verilerin hacmini azaltır. |False |No |

>[!NOTE]
> Hazırlanan bir kopyayı sıkıştırma etkinken kullanırsanız, hazırlama blobu bağlı hizmeti için hizmet sorumlusu veya MSI kimlik doğrulaması desteklenmez.

Aşağıda, önceki tabloda açıklanan özelliklerle birlikte kopyalama etkinliğinin örnek bir tanımı verilmiştir:

```json
"activities":[
    {
        "name": "Sample copy activity",
        "type": "Copy",
        "inputs": [...],
        "outputs": [...],
        "typeProperties": {
            "source": {
                "type": "SqlSource",
            },
            "sink": {
                "type": "SqlSink"
            },
            "enableStaging": true,
            "stagingSettings": {
                "linkedServiceName": {
                    "referenceName": "MyStagingBlob",
                    "type": "LinkedServiceReference"
                },
                "path": "stagingcontainer/path",
                "enableCompression": true
            }
        }
    }
]
```

### <a name="staged-copy-billing-impact"></a>Hazırlanan kopya faturalandırma etkisi

İki adım temelinde ücretlendirilirsiniz: kopyalama süresi ve kopya türü.

* Bulut kopyası sırasında hazırlama kullandığınızda, verileri bir bulut veri deposundan başka bir bulut veri deposuna kopyalar. her iki aşama de Azure tümleştirme çalışma zamanı tarafından güçlendirilirse, [1. adım ve 2. adım için kopyalama süresi toplamı] x [bulut kopyalama birimi fiyatı] ücretlendirirsiniz.
* Bir karma kopya sırasında hazırlama kullandığınızda, verileri şirket içi veri deposundan bir bulut veri deposuna kopyalayan bir aşama, kendinden konak bir tümleştirme çalışma zamanı tarafından güçlendirildiği için [karma kopyalama süresi] x [karma kopya birim fiyatı] + [bulut kopyalama süresi] x [bulut kopyalama birimi fiyatı] için ücretlendirilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Diğer kopyalama etkinliği makalelerine bakın:

- [Kopyalama etkinliğine genel bakış](copy-activity-overview.md)
- [Kopyalama etkinliği performans ve ölçeklenebilirlik Kılavuzu](copy-activity-performance.md)
- [Kopyalama etkinliği performansını sorun giderme](copy-activity-performance-troubleshooting.md)
- [Data Lake veya veri ambarınızdan verileri Azure 'a geçirmek için Azure Data Factory kullanın](data-migration-guidance-overview.md)
- [Verileri Amazon S3'ten Azure Depolama'ya geçirme](data-migration-guidance-s3-azure-storage.md)