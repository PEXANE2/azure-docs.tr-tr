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
ms.date: 03/09/2020
ms.openlocfilehash: a31c6229220142acea9ded571128ab54c50d34b7
ms.sourcegitcommit: f97d3d1faf56fb80e5f901cd82c02189f95b3486
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/11/2020
ms.locfileid: "79125790"
---
# <a name="copy-activity-performance-optimization-features"></a>Etkinlik performansını en iyi duruma getirme özelliklerini Kopyala

Bu makalede, Azure Data Factory ' de kullanabileceğiniz etkinlik performansını en iyi duruma getirme özellikleri özetlenmektedir.

## <a name="data-integration-units"></a>Veri tümleştirme birimleri

Veri tümleştirme birimi, Azure Data Factory içinde tek bir birimin gücünü (CPU, bellek ve ağ kaynak ayırma birleşimi) temsil eden bir ölçüdür. Veri tümleştirme birimi yalnızca [Azure tümleştirme çalışma zamanı](concepts-integration-runtime.md#azure-integration-runtime)için geçerlidir, ancak [Şirket içinde barındırılan tümleştirme çalışma zamanı](concepts-integration-runtime.md#self-hosted-integration-runtime)için geçerli değildir.

Kopyalama etkinliği çalıştırmasını güçlendiren izin verilen, **2 ile 256 arasındadır**. Belirtilmezse veya Kullanıcı arabiriminde "otomatik" seçeneğini belirlerseniz, kaynak havuzu çiftine ve veri düzenine göre en iyi DIU ayarını dinamik olarak uygulayın Data Factory. Aşağıdaki tablo, farklı kopyalama senaryolarında desteklenen DIU aralıklarını ve varsayılan davranışı listeler:

| Kopyalama senaryosu | Desteklenen DIU aralığı | Hizmet tarafından belirlenen varsayılan DIUs |
|:--- |:--- |---- |
| Dosya depoları arasında |- **veya tek dosyadan kopyalama**: 2-4 <br>**ve birden çok dosyaya kopyalama**- : dosyaların sayısına ve boyutuna bağlı olarak 2-256 <br><br>Örneğin, 4 büyük dosya içeren bir klasörden veri kopyalar ve hiyerarşiyi korumayı seçerseniz, en yüksek etkin DIU 16 ' dır; dosyayı birleştirmeyi seçtiğinizde, en fazla etkin DIU 4 ' tir. |Dosyaların sayısına ve boyutuna bağlı olarak 4 ile 32 arasında |
| Dosya deposundan dosya olmayan depoya |**tek dosyadan kopyalama**- : 2-4 <br/>dosyaların sayısına ve boyutuna bağlı olarak, **birden çok dosyadan kopyalama**- : 2-256 <br/><br/>Örneğin, 4 büyük dosya içeren bir klasörden veri kopyalarsanız, en yüksek etkin DIU 16 ' dır. |- **Azure SQL veritabanı 'na veya Azure Cosmos DB kopyalamak**: havuz katmanına (DTU/ru) ve kaynak dosya düzenine bağlı olarak 4 ile 16 arasında.<br>PolyBase veya COPY deyimlerini kullanarak **Azure SYNAPSE Analytics 'e - kopyalayın** : 2<br>-Diğer senaryo: 4 |
| Dosya depolamadan dosya deposuna |**bölüm seçeneği etkinleştirilmiş veri depolarından** ( [Oracle](connector-oracle.md#oracle-as-source)/[Netezza](connector-netezza.md#netezza-as-source)/[Teradata](connector-teradata.md#teradata-as-source)dahil) kopyalama - : bir klasöre yazarken 2-256 ve tek bir dosyaya yazarken 2-4. Kaynak veri bölümü başına, en fazla 4 DIUs kullanabilirsiniz.<br>**diğer senaryolar**- : 2-4 |**rest 'den veya http 'Den kopyalama**- : 1<br/>yüklemeyi kaldırma kullanarak **Amazon Redshift 'Tan kopyalama** - : 2<br>- **diğer senaryo**: 4 |
| Dosya olmayan depolar arasında |**bölüm seçeneği etkinleştirilmiş veri depolarından** ( [Oracle](connector-oracle.md#oracle-as-source)/[Netezza](connector-netezza.md#netezza-as-source)/[Teradata](connector-teradata.md#teradata-as-source)dahil) kopyalama - : bir klasöre yazarken 2-256 ve tek bir dosyaya yazarken 2-4. Kaynak veri bölümü başına, en fazla 4 DIUs kullanabilirsiniz.<br/>**diğer senaryolar**- : 2-4 |**rest 'den veya http 'Den kopyalama**- : 1<br>- **diğer senaryo**: 4 |

Kopyalama etkinliği izleme görünümü veya etkinlik çıkışında her bir kopya için kullanılan orus 'yi görebilirsiniz. Daha fazla bilgi için bkz. [kopyalama etkinliği izleme](copy-activity-monitoring.md). Bu varsayılanı geçersiz kılmak için `dataIntegrationUnits` özelliği için aşağıdaki gibi bir değer belirtin. Kopyalama işleminin çalışma zamanında kullandığı *gerçek sayı* , veri düzenlerinize bağlı olarak yapılandırılan değere eşit veya ondan daha az.

**Kullanılan mus \* kopyalama süresi \* birim fiyatı/DIU-saat**olarak ücretlendirilirsiniz. Geçerli fiyatlara [buradan](https://azure.microsoft.com/pricing/details/data-factory/data-pipeline/)bakın. Yerel para birimi ve ayrı ayırt sayma, abonelik türü başına uygulanabilir.

**Örnek:**

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
- Diğer taraftan, CPU şirket içinde barındırılan IR düğümünde yüksek veya kullanılabilir bellek düşükse, birden çok düğümde yükün ölçeğini genişletmek için yeni bir düğüm ekleyebilirsiniz.  Yönergeler için [buraya](create-self-hosted-integration-runtime.md#high-availability-and-scalability) bakın.

Aşağıdaki senaryolarda, tek kopya etkinliği yürütme birden çok şirket içinde barındırılan IR düğümünden yararlanabilir:

- Dosya tabanlı mağazalardan, dosyaların sayısına ve boyutuna bağlı olarak verileri kopyalayın.
- Veri bölümlerinin sayısına bağlı olarak, Bölüm seçenekleri etkinleştirilmiş veri deposundan ( [Oracle](connector-oracle.md#oracle-as-source), [Netezza](connector-netezza.md#netezza-as-source), [Teradata](connector-teradata.md#teradata-as-source), [SAP HANA](connector-sap-hana.md#sap-hana-as-source), [SAP tablosu](connector-sap-table.md#sap-table-as-source)ve [SAP açık hub](connector-sap-business-warehouse-open-hub.md#sap-bw-open-hub-as-source)dahil) verileri kopyalayın.

## <a name="parallel-copy"></a>Paralel kopya

Kopyalama etkinliğinin kullanmasını istediğiniz paralellik belirtmek için kopyalama etkinliğinde paralel kopyalama (`parallelCopies` özelliği) ayarlayabilirsiniz. Bu özelliği, kopyalama etkinliğindeki en fazla iş parçacığı sayısı olarak, kaynağınızdan okunan veya kanal veri depolarınız paralel olarak yazabilirsiniz.

Paralel kopya, [veri tümleştirme birimlerine](#data-integration-units) veya [ŞIRKET içinde barındırılan IR düğümlerine](#self-hosted-integration-runtime-scalability)diklik edilir. Tüm orus veya şirket içinde barındırılan IR düğümlerinde sayılır.

Her kopyalama etkinliği çalıştırması için, varsayılan olarak Azure Data Factory, en iyi paralel kopyalama ayarını kaynak havuzu çiftine ve veri düzenine göre dinamik olarak uygular. 

> [!TIP]
> Paralel kopyanın varsayılan davranışı genellikle, ADF tarafından otomatik olarak belirlenen ve kaynak havuzu çiftinize, veri düzenine ve en iyi barındırılan IR CPU/bellek/düğüm sayısına bağlı olarak otomatik olarak belirlenen en iyi aktarım hızını verir. Paralel kopyayı ayarlama sırasında [etkinlik performansını kopyalama sorunlarını giderme](copy-activity-performance-troubleshooting.md) bölümüne bakın.

Aşağıdaki tabloda, paralel kopyalama davranışı listelenmektedir:

| Kopyalama senaryosu | Paralel kopyalama davranışı |
| --- | --- |
| Dosya depoları arasında | `parallelCopies`, **dosya düzeyinde**paralellik belirler. Her bir dosya içindeki parçalama otomatik ve şeffaf bir şekilde gerçekleşir. Verileri paralel olarak yüklemek için belirli bir veri deposu türü için en iyi uygun öbek boyutunu kullanmak üzere tasarlanmıştır. <br/><br/>Kopya etkinliğinin gerçek sayısı, çalışma zamanında kullanılan, sahip olduğunuz dosya sayısından daha fazla değil. Kopyalama davranışı dosya havuzunda **birleştirme dosyası** ise kopyalama etkinliği dosya düzeyinde paralellik özelliğinden yararlanamaz. |
| Dosya deposundan dosya olmayan depoya | -Verileri Azure SQL veritabanı 'na veya Azure Cosmos DB kopyalarken, varsayılan paralel kopya Ayrıca havuz katmanına (DTU 'Lar/ru sayısı) göre değişir.<br>-Verileri Azure tablosuna kopyalarken, varsayılan paralel kopya 4 ' dir. |
| Dosya depolamadan dosya deposuna | -Seçenek etkinleştirilmiş veri deposundan ( [Oracle](connector-oracle.md#oracle-as-source), [Netezza](connector-netezza.md#netezza-as-source), [Teradata](connector-teradata.md#teradata-as-source), [SAP HANA](connector-sap-hana.md#sap-hana-as-source), [SAP tablosu](connector-sap-table.md#sap-table-as-source)ve [SAP açık hub](connector-sap-business-warehouse-open-hub.md#sap-bw-open-hub-as-source)dahil) veri kopyalarken, varsayılan paralel kopya 4 ' dir. Kopya etkinliğinin gerçek sayısı, çalışma zamanında kullanılan, sahip olduğunuz veri bölümlerinin sayısından fazla değil. Şirket içinde barındırılan Integration Runtime ve Azure Blob/ADLS 2. ' e kopyalama sırasında, en fazla etkin paralel kopya, IR düğümü başına 4 veya 5 ' tir.<br>-Diğer senaryolar için paralel kopya etkili olmaz. Paralellik belirtilmiş olsa bile, uygulanmaz. |
| Dosya olmayan depolar arasında | -Verileri Azure SQL veritabanı 'na veya Azure Cosmos DB kopyalarken, varsayılan paralel kopya Ayrıca havuz katmanına (DTU 'Lar/ru sayısı) göre değişir.<br/>-Verileri Azure tablosuna kopyalarken, varsayılan paralel kopya 4 ' dir. |

Veri mağazalarınızı barındıran makinelerde yükü denetlemek veya kopyalama performansını ayarlamak için, varsayılan değeri geçersiz kılabilir ve `parallelCopies` özelliği için bir değer belirtebilirsiniz. Büyük veya 1'e eşit bir tamsayı değeri olmalıdır. Çalışma zamanında, en iyi performans için kopyalama etkinliği, ayarladığınız değerden küçük veya bu değere eşit bir değer kullanır.

`parallelCopies` özelliği için bir değer belirttiğinizde, kaynak ve havuz Veri depolarındaki yük artışını hesapta yapın. Ayrıca, kopyalama etkinliği tarafından güçde bulunursa, şirket içinde barındırılan tümleştirme çalışma zamanına yönelik yük artışını de göz önünde bulundurun. Bu yük artışı, özellikle aynı veri deposunda çalışan aynı etkinliklerin birden çok etkinliğiniz veya eş zamanlı çalıştırmaları olduğunda gerçekleşir. Veri deposunun veya şirket içinde barındırılan tümleştirme çalışma zamanının yük ile azaldığını fark ederseniz, yükü ortadan kaldırmak için `parallelCopies` değerini azaltın.

**Örnek:**

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

Bir kaynak veri deposundan bir havuz veri deposuna veri kopyalama, geçici bir hazırlama deposu Blob depolamayı kullanmak seçebilirsiniz. Hazırlama aşağıdaki durumlarda kullanışlıdır:

- **PolyBase aracılığıyla çeşitli veri depolarındaki verileri SQL veri ambarı 'na almak istiyorsunuz.** SQL veri ambarı PolyBase, SQL veri ambarı'na büyük miktarda veri yüklemek için yüksek performanslı mekanizması olarak kullanır. Kaynak verilerin blob depolaması veya Azure Data Lake Store olması ve ek ölçütlere uyması gerekir. Blob Depolama farklı bir veri deposu ya da Azure Data Lake Store veri yüklediğinizde, verileri geçici hazırlama Blob depolamaya kopyalama etkinleştirebilirsiniz. Bu durumda Azure Data Factory, PolyBase 'in gereksinimlerini karşıladığından emin olmak için gerekli veri dönüştürmelerini gerçekleştirir. Ardından verileri verimli bir şekilde SQL veri ambarı'na yüklemek için PolyBase kullanır. Daha fazla bilgi için bkz. [Azure SQL veri ambarı 'na veri yüklemek Için PolyBase kullanma](connector-azure-sql-data-warehouse.md#use-polybase-to-load-data-into-azure-sql-data-warehouse).
- **Bazen bir karma veri hareketini (yani, şirket içi veri deposundan bir bulut veri deposuna kopyalamak için) yavaş bir ağ bağlantısı üzerinden gerçekleştirme işlemi biraz zaman alır.** Performansı artırmak için, hazırlanan kopyayı, verileri bulutta hazırlama veri deposuna taşımak daha az zaman alması amacıyla Şirket içindeki verileri sıkıştırmak için kullanabilirsiniz. Daha sonra, hedef veri deposuna yüklemeden önce hazırlama deposundaki verileri açabilir.
- **Şirket BT ilkeleri nedeniyle güvenlik duvarınızdaki bağlantı noktası 80 ve bağlantı noktası 443 dışındaki bağlantı noktalarını açmak istemezsiniz.** Örneğin, bir Azure SQL veritabanı havuz veya bir Azure SQL veri ambarı havuzu için bir şirket içi veri deposundan veri kopyaladığınızda, Windows Güvenlik Duvarı hem kurumsal güvenlik ağınızın 1433 numaralı bağlantı noktasında giden TCP iletişimine'ı etkinleştirmeniz gerekir. Bu senaryoda, hazırlanan kopya, ilk olarak şirket içinde barındırılan tümleştirme çalışma zamanından yararlanarak, önce verileri HTTP veya 443 numaralı bağlantı noktasında HTTP veya HTTPS üzerinden BLOB depolama hazırlama örneğine kopyalayabilir. Ardından, BLOB depolama alanı hazırlamadaki verileri SQL veritabanı veya SQL veri ambarı 'na yükleyebilir. Bu akışta 1433 numaralı bağlantı noktasını etkinleştirmek gerekmez.

### <a name="how-staged-copy-works"></a>Nasıl hazırlanmış kopya çalışır

Hazırlama özelliğini etkinleştirdiğinizde, ilk veriler kaynak veri deposundan hazırlama Blob depolama alanına kopyalanır (kendi işleyicinizi getirin). Ardından, veri hazırlama veri deposundan havuz veri deposuna kopyalanır. Azure Data Factory, sizin için iki aşamalı akışı otomatik olarak yönetir. Azure Data Factory, veri taşıma işlemi tamamlandıktan sonra hazırlama depolamadan geçici verileri de temizler.

![Hazırlanmış kopya](media/copy-activity-performance/staged-copy.png)

Veri hareketini bir hazırlama deposu kullanarak etkinleştirdiğinizde, verileri kaynak veri deposundan bir ara veya hazırlama veri deposuna taşımadan önce verilerin sıkıştırılıp sıkıştırılmayacağını ve sonra verileri bir ara veya hazırlama dat 'dan taşımadan önce sıkıştırması açılacak şekilde belirtebilirsiniz Havuz veri deposuna bir mağaza.

Şu anda, otomatik olarak barındırılan farklı IRS 'ler arasında bağlı olan iki veri deposu arasında veri kopyalayamazsınız, ancak hazırlanmamış kopya olmadan, verileri kopyalayamazsınız. Bu tür senaryolar için, kaynaktan hazırlama ve daha sonra hazırlama durumundan havuza kopyalamak üzere iki açık zincirleme kopyalama etkinliği yapılandırabilirsiniz.

### <a name="configuration"></a>Yapılandırma

Hedef veri deposuna yüklemeden önce, verilerin blob depolamada hazırlanması isteyip istemediğinizi belirtmek için kopyalama etkinliğinde **Enablehazırlama** ayarını yapılandırın. **Enablehazırlama** `TRUE`olarak ayarladığınızda, aşağıdaki tabloda listelenen ek özellikleri belirtin. Ayrıca, yoksa, hazırlama için bir Azure depolama veya depolama paylaşılan erişim imzası ile bağlantılı hizmet oluşturmanız gerekir.

| Özellik | Açıklama | Varsayılan değer | Gerekli |
| --- | --- | --- | --- |
| enableStaging |Veri deposunu hazırlama bir geçiş aracılığıyla kopyalamak isteyip istemediğinizi belirtin. |False |Hayır |
| linkedServiceName |Geçici hazırlama deposu olarak kullandığınız depolama örneğine başvuran bir [Azurestorage](connector-azure-blob-storage.md#linked-service-properties) Linked hizmetinin adını belirtin. <br/><br/> PolyBase aracılığıyla SQL veri ambarı 'na veri yüklemek için, paylaşılan erişim imzasıyla depolama kullanamazsınız. Diğer tüm senaryolarda kullanabilirsiniz. |Yok |Evet, **Enablehazırlama** true olarak ayarlandığında |
| yol |Hazırlanmış verinin içermesini istediğiniz Blob Depolama yolu belirtin. Bir yol sağlamazsanız, hizmet geçici verileri depolamak için bir kapsayıcı oluşturur. <br/><br/> Yalnızca depolama ile paylaşılan erişim imzası kullanın veya geçici veri belirli bir konumda olmasını gerektiren bir yol belirtin. |Yok |Hayır |
| enableCompression |Verilerin hedefe kopyalanmadan önce sıkıştırılması gerekip gerekmediğini belirtir. Bu ayar, aktarılan veri hacmini azaltır. |False |Hayır |

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

### <a name="staged-copy-billing-impact"></a>Faturalama etkisi kopyalama hazırlandı

İki adım temelinde ücretlendirilirsiniz: kopyalama süresi ve kopya türü.

* Bulut kopyası sırasında hazırlama kullandığınızda, verileri bir bulut veri deposundan başka bir bulut veri deposuna kopyalar. her iki aşama de Azure tümleştirme çalışma zamanı tarafından güçlendirilirse, [1. adım ve 2. adım için kopyalama süresi toplamı] x [bulut kopyalama birimi fiyatı] ücretlendirirsiniz.
* Bir karma kopya sırasında hazırlama kullandığınızda, verileri şirket içi veri deposundan bir bulut veri deposuna kopyalayan bir aşama, kendinden konak bir tümleştirme çalışma zamanı tarafından güçlendirildiği için, [karma kopyalama süresi] x [karma kopya birim fiyatı] + [bulut kopyalama süresi] için ücret ödersiniz. x [bulut kopya birim fiyatı].

## <a name="next-steps"></a>Sonraki adımlar
Diğer kopyalama etkinliği makalelerine bakın:

- [Kopyalama etkinliğine genel bakış](copy-activity-overview.md)
- [Kopyalama etkinliği performans ve ölçeklenebilirlik Kılavuzu](copy-activity-performance.md)
- [Kopyalama etkinliği performansını sorun giderme](copy-activity-performance-troubleshooting.md)
- [Data Lake veya veri ambarınızdan verileri Azure 'a geçirmek için Azure Data Factory kullanın](data-migration-guidance-overview.md)
- [Amazon S3 'ten Azure Storage 'a veri geçirme](data-migration-guidance-s3-azure-storage.md)