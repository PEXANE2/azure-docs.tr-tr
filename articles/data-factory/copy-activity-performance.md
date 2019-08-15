---
title: Azure Data Factory 'de etkinlik performansını ve ayarlama kılavuzunu kopyala | Microsoft Docs
description: Kopyalama etkinliğini kullandığınızda Azure Data Factory veri hareketinin performansını etkileyen anahtar faktörleri hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 07/02/2019
ms.author: jingwang
ms.openlocfilehash: d8ce0a4f6bacdd1c8c858d474e6f3957a23c6357
ms.sourcegitcommit: 5d6c8231eba03b78277328619b027d6852d57520
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2019
ms.locfileid: "68967354"
---
# <a name="copy-activity-performance-and-tuning-guide"></a>Kopyalama etkinliği performansı ve ayarlama Kılavuzu
> [!div class="op_single_selector" title1="Kullanmakta olduğunuz Azure Data Factory sürümünü seçin:"]
> * [Sürüm 1](v1/data-factory-copy-activity-performance.md)
> * [Geçerli sürüm](copy-activity-performance.md)


Azure Data Factory kopyalama etkinliği, birinci sınıf güvenli, güvenilir ve yüksek performanslı bir veri yükleme çözümü sağlar. Bu uygulamayı, her gün çok sayıda terabaytlık verileri çeşitli bulut ve şirket içi veri depoları arasında kopyalamak için kullanabilirsiniz. Hızlı veri yükleme performansı, çekirdek büyük veri sorununa odaklanabilmenizi sağlayan bir anahtardır. gelişmiş analiz çözümleri oluşturma ve tüm bu verilerden derin Öngörüler alma.

Azure, kurumsal düzeyde veri depolama ve veri ambarı çözümleri kümesi sağlar. Kopyalama etkinliği, yapılandırılması ve ayarlanması kolay olan, yüksek oranda iyileştirilmiş bir veri yükleme deneyimi sunar. Tek bir kopyalama etkinliğiyle, verileri uygulamasına yükleyebilirsiniz:

* 1,2 GBps 'de Azure SQL veri ambarı.
* 1,0 GBps 'de Azure Blob depolama.
* 1,0 GBps 'de Azure Data Lake Store.

Bu makalede açıklanır:

* Projenizin planlanmasına yardımcı olmak için desteklenen kaynak ve havuz veri depoları için [Performans başvuru numaraları](#performance-reference) .
* [Veri tümleştirme birimleri](#data-integration-units) (Dius), [paralel kopya](#parallel-copy)ve [aşamalı kopya](#staged-copy)dahil olmak üzere farklı senaryolarda kopyalama aktarım hızını arttıran özellikler.
* Performansını ve kopyalama performansını etkileyebilecek önemli faktörleri ayarlama hakkında [performans ayarlama Kılavuzu](#performance-tuning-steps) .

> [!NOTE]
> Kopyalama etkinliğini genel olarak bilmiyorsanız, bu makaleyi kullanmadan önce [kopyalama etkinliğine genel bakış](copy-activity-overview.md) bölümüne bakın.
>

## <a name="performance-reference"></a>Performans başvurusu

Bir başvuru olarak, aşağıdaki tabloda, tek bir kopyalama etkinliğinde verilen kaynak ve havuz çiftleri için, şirket içi teste dayalı olarak çalıştırılan bir kaynak ve havuz çiftleri için aktarım hızı sayısını MBps cinsinden gösterir. Karşılaştırma için aynı zamanda [veri tümleştirme birimlerinin](#data-integration-units) veya [Şirket içinde barındırılan tümleştirme çalışma zamanı ölçeklenebilirliğinin](concepts-integration-runtime.md#self-hosted-integration-runtime) (birden çok düğüm) farklı ayarlarının performansı kopyalama konusunda nasıl yardımcı olabileceğini gösterir.

![Performans Matrisi](./media/copy-activity-performance/CopyPerfRef.png)

> [!IMPORTANT]
> Kopyalama etkinliği bir Azure tümleştirme çalışma zamanı üzerinde çalıştırıldığında, en düşük izin verilen veri tümleştirme birimleri (eski adıyla veri taşıma birimleri olarak bilinir) ikiye ayarlanır. Belirtilmezse, [veri tümleştirme birimlerinde](#data-integration-units)kullanılan varsayılan veri tümleştirme birimleri ' ne bakın.

**Şunlara işaret eder:**

* Aktarım hızı şu formül kullanılarak hesaplanır: [kaynaktan okunan veri boyutu]/[kopyalama etkinliği çalışma süresi].
* Tablodaki performans başvuru numaraları, tek bir kopyalama etkinliği çalıştırmasında bir [TPC-H](http://www.tpc.org/tpch/) veri kümesi kullanılarak ölçülür. Dosya tabanlı mağazalara yönelik test dosyaları, boyutu 10 GB olan birden çok dosya.
* Azure veri depoları, kaynak ve havuz aynı Azure bölgesinde olan.
* Şirket içi ve bulut veri depoları arasında karma kopya için, her bir şirket içinde barındırılan tümleştirme çalışma zamanı düğümü, veri deposundan aşağıdaki belirtiyle ayrı olan bir makinede çalışmaktadır. Tek bir etkinlik çalıştırırken test makinenin CPU, bellek veya ağ bant genişliği için küçük bir bölümü kopyalama işlemi kullanılan.
    <table>
    <tr>
        <td>CPU</td>
        <td>2\.20 GHz Intel Xeon E5-2660 v2 32 çekirdek</td>
    </tr>
    <tr>
        <td>Bellek</td>
        <td>128 GB</td>
    </tr>
    <tr>
        <td>Ağ</td>
        <td>Internet arabirimi: 10 Gbps; intranet arabirimi: 40 Gbps</td>
    </tr>
    </table>


> [!TIP]
> Daha fazla bilgi kullanarak daha yüksek aktarım hızı elde edebilirsiniz. Örneğin, 100 DIUs ile Azure Blob depolamadan Azure Data Lake Store, 1,0 GB/sn 'lik verileri kopyalayabilirsiniz. Bu özellik ve desteklenen senaryo hakkında daha fazla bilgi için [veri tümleştirme birimleri](#data-integration-units) bölümüne bakın. 

## <a name="data-integration-units"></a>Veri tümleştirme birimleri

Veri tümleştirme birimi, Azure Data Factory içinde tek bir birimin gücünü (CPU, bellek ve ağ kaynak ayırma birleşimi) temsil eden bir ölçüdür. Veri tümleştirme birimi yalnızca [Azure tümleştirme çalışma zamanı](concepts-integration-runtime.md#azure-integration-runtime)için geçerlidir, ancak [Şirket içinde barındırılan tümleştirme çalışma zamanı](concepts-integration-runtime.md#self-hosted-integration-runtime)için geçerli değildir.

Kopyalama etkinliği çalıştırmanın en düşük düzeyde olması iki ' dir. Belirtilmezse, aşağıdaki tabloda farklı kopyalama senaryolarında kullanılan varsayılan DIUs listelenmektedir:

| Kopyalama senaryosu | Hizmet tarafından belirlenen varsayılan DIUs |
|:--- |:--- |
| Dosya tabanlı depoları arasında veri kopyalama | Dosyaların sayısına ve boyutuna bağlı olarak 4 ile 32 arasında |
| Verileri Azure SQL veritabanı 'na veya Azure Cosmos DB kopyalama |Azure SQL veritabanı 'nın veya Cosmos DB katmanına bağlı olarak 4 ile 16 arasında (DTU sayısı/ru) |
| Diğer tüm kopyalama senaryoları | 4 |

Bu varsayılanı geçersiz kılmak için bir değer belirtin. **dataIntegrationUnits** özelliğini aşağıdaki gibi. **Dataıntegrationunits** özelliği için *izin verilen değerler* 256 ' e kadar olur. *DIUs gerçek sayısını* eşit veya daha az, veri modelini bağlı olarak yapılandırılmış bir değeri, kopyalama işleminin çalışma zamanında kullanır. Özel kopyalama kaynağı ve havuz için daha fazla birimi yapılandırırken alabilirsiniz performans kazancı düzeyi hakkında bilgi için bkz [Performans başvurusu](#performance-reference).

Bir etkinlik çalıştırmasını izlerken kopyalama etkinliği çıkışında her bir kopya için kullanılan orus 'yi görebilirsiniz. Daha fazla bilgi için bkz. [kopyalama etkinliği izleme](copy-activity-overview.md#monitoring).

> [!NOTE]
> Dörtten daha büyük bir ayar, şu anda yalnızca Azure depolama, Azure Data Lake Storage, Amazon S3, Google bulut depolama, bulut FTP veya Cloud SFTP diğer bulut veri depolarından birden çok dosya kopyaladığınızda geçerlidir.
>

**Örnek**

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
            "dataIntegrationUnits": 32
        }
    }
]
```

### <a name="data-integration-units-billing-impact"></a>Veri tümleştirme birimleri faturalama etkisi

Kopyalama işleminin toplam süresine göre ücretlendirileceğini unutmayın. Veri taşıma için faturalandırdığınız toplam süre, DIUs genelinde sürenin toplamıdır. Bir kopyalama işi bir saat ile iki bulut birimleri almak için kullanılan ve artık bu sekiz bulut birimiyle 15 dakika sürer, toplam fatura neredeyse aynı kalır.

## <a name="parallel-copy"></a>Paralel kopya

Kopyalama etkinliğinin kullanmasını istediğiniz paralellik belirtmek için **Parallelkopyaları** özelliğini kullanabilirsiniz. Bu özelliği, kopyalama etkinliğindeki en fazla iş parçacığı sayısı olarak, kaynağınızdan okuyabilecek veya kanal veri depolarınız paralel olarak yazabileceğiniz şekilde düşünebilirsiniz.

Her kopyalama etkinliği çalıştırması için Azure Data Factory, verileri kaynak veri deposundan ve hedef veri deposuna kopyalamak için kullanılacak paralel kopya sayısını belirler. Varsayılan paralel kopya sayısı, kullandığınız kaynak ve havuz türüne bağlıdır.

| Kopyalama senaryosu | Hizmet tarafından belirlenen varsayılan paralel kopya sayısı |
| --- | --- |
| Dosya tabanlı depoları arasında veri kopyalama |Dosyaların boyutuna ve iki bulut veri deposu arasında veri kopyalamak için kullanılan DIUs sayısının yanı sıra şirket içinde barındırılan tümleştirme çalışma zamanı makinesinin fiziksel yapılandırması da bağlıdır. |
| Tüm kaynak depolardan Azure Tablo depolamaya veri kopyalama |4 |
| Tüm diğer kopyalama senaryolarında |1\. |

> [!TIP]
> Dosya tabanlı mağazalar arasında veri kopyaladığınızda, varsayılan davranış genellikle en iyi performansı sağlar. Varsayılan davranış, kaynak dosya örüntüsünün temel alınarak otomatik olarak belirlenir.

Veri mağazalarınızı barındıran makinelerde yükü denetlemek veya kopyalama performansını ayarlamak için, varsayılan değeri geçersiz kılabilir ve **Parallelcopy** özelliği için bir değer belirtebilirsiniz. Büyük veya 1'e eşit bir tamsayı değeri olmalıdır. Çalışma zamanında, en iyi performans için kopyalama etkinliği, ayarladığınız değerden küçük veya bu değere eşit bir değer kullanır.

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

**Şunlara işaret eder:**

* Dosya tabanlı mağazalar arasında veri kopyaladığınızda **Parallelcopy** , dosya düzeyinde paralellik belirler. Tek bir dosya içindeki parçalama otomatik ve şeffaf bir şekilde gerçekleşir. Verileri paralel ve **paralelde**paralel olarak yüklemek için belirli bir kaynak veri deposu türü için en uygun öbek boyutunu kullanmak üzere tasarlanmıştır. Gerçek veri taşıma Hizmeti'nde kopyalama işleminin çalışma zamanında kullandığı paralel kopya sayısı sahip olduğunuz dosyaların sayısı, en fazla ' dir. Kopyalama davranışı **Mergefile**ise, kopyalama etkinliği dosya düzeyinde paralellik özelliğinden yararlanamaz.
* Dosya tabanlı olmayan mağazalardan ( [Oracle](connector-oracle.md#oracle-as-source), [Teradata](connector-teradata.md#teradata-as-source), [SAP tablosu](connector-sap-table.md#sap-table-as-source)ve [SAP Open hub](connector-sap-business-warehouse-open-hub.md#sap-bw-open-hub-as-source) Bağlayıcısı dışında, veri bölümleme özelliği etkinleştirilmiş bir kaynak olarak) verileri kopyaladığınızda, veri taşıma hizmeti olan depolar **Parallelkopyaları** özelliğini yoksayar. Paralellik belirtilmiş olsa bile, bu durumda uygulanmaz.
* **Parallelkopyaların** özelliği **dataıntegrationunits**öğesine göre belirlenir. Önceki tüm veri tümleştirme birimlerinizde sayılır.
* **Parallelkopyaları** özelliği için bir değer belirttiğinizde, kaynak ve havuz Veri depolarındaki yük artışını göz önünde bulundurun. Ayrıca, kopyalama etkinliği, karma kopya için, bu, örneğin, karma kopyaya karşı güç alıyorsa, şirket içinde barındırılan tümleştirme çalışma zamanına yönelik yük artışını de göz önünde bulundurun. Bu yük artışı, özellikle aynı veri deposunda çalışan aynı etkinliklerin birden çok etkinliğiniz veya eş zamanlı çalıştırmaları olduğunda gerçekleşir. Veri deposunun veya şirket içinde barındırılan tümleştirme çalışma zamanının yük ile azaldığını fark ederseniz, yükü ortadan kaldırmak için **Parallelkopyaların** değerini azaltın.

## <a name="staged-copy"></a>Hazırlanmış kopya

Bir kaynak veri deposundan bir havuz veri deposuna veri kopyalama, geçici bir hazırlama deposu Blob depolamayı kullanmak seçebilirsiniz. Hazırlama aşağıdaki durumlarda kullanışlıdır:

- **PolyBase aracılığıyla çeşitli veri depolarındaki verileri SQL veri ambarı 'na almak istiyorsunuz.** SQL veri ambarı PolyBase, SQL veri ambarı'na büyük miktarda veri yüklemek için yüksek performanslı mekanizması olarak kullanır. Kaynak verilerin blob depolaması veya Azure Data Lake Store olması ve ek ölçütlere uyması gerekir. Blob Depolama farklı bir veri deposu ya da Azure Data Lake Store veri yüklediğinizde, verileri geçici hazırlama Blob depolamaya kopyalama etkinleştirebilirsiniz. Bu durumda Azure Data Factory, PolyBase 'in gereksinimlerini karşıladığından emin olmak için gerekli veri dönüştürmelerini gerçekleştirir. Ardından verileri verimli bir şekilde SQL veri ambarı'na yüklemek için PolyBase kullanır. Daha fazla bilgi için [Azure SQL veri ambarı'na veri yüklemek için PolyBase kullanma](connector-azure-sql-data-warehouse.md#use-polybase-to-load-data-into-azure-sql-data-warehouse).
- **Bazen bir karma veri hareketini (yani, şirket içi veri deposundan bir bulut veri deposuna kopyalamak için) yavaş bir ağ bağlantısı üzerinden gerçekleştirme işlemi biraz zaman alır.** Performansı artırmak için, hazırlanan kopyayı, verileri bulutta hazırlama veri deposuna taşımak daha az zaman alması amacıyla Şirket içindeki verileri sıkıştırmak için kullanabilirsiniz. Daha sonra, hedef veri deposuna yüklemeden önce hazırlama deposundaki verileri açabilir.
- **Şirket BT ilkeleri nedeniyle güvenlik duvarınızdaki bağlantı noktası 80 ve bağlantı noktası 443 dışındaki bağlantı noktalarını açmak istemezsiniz.** Örneğin, bir Azure SQL veritabanı havuz veya bir Azure SQL veri ambarı havuzu için bir şirket içi veri deposundan veri kopyaladığınızda, Windows Güvenlik Duvarı hem kurumsal güvenlik ağınızın 1433 numaralı bağlantı noktasında giden TCP iletişimine'ı etkinleştirmeniz gerekir. Bu senaryoda, hazırlanan kopya, ilk olarak şirket içinde barındırılan tümleştirme çalışma zamanından yararlanarak, önce verileri HTTP veya 443 numaralı bağlantı noktasında HTTP veya HTTPS üzerinden BLOB depolama hazırlama örneğine kopyalayabilir. Ardından, BLOB depolama alanı hazırlamadaki verileri SQL veritabanı veya SQL veri ambarı 'na yükleyebilir. Bu akışta 1433 numaralı bağlantı noktasını etkinleştirmek gerekmez.

### <a name="how-staged-copy-works"></a>Nasıl hazırlanmış kopya çalışır

Hazırlama özelliğini etkinleştirdiğinizde, ilk veriler kaynak veri deposundan hazırlama Blob depolama alanına kopyalanır (kendi işleyicinizi getirin). Ardından, veri hazırlama veri deposundan havuz veri deposuna kopyalanır. Azure Data Factory, sizin için iki aşamalı akışı otomatik olarak yönetir. Azure Data Factory, veri taşıma işlemi tamamlandıktan sonra hazırlama depolamadan geçici verileri de temizler.

![Hazırlanmış kopya](media/copy-activity-performance/staged-copy.png)

Veri hareketini bir hazırlama deposu kullanarak etkinleştirdiğinizde, verileri kaynak veri deposundan bir ara veya hazırlama veri deposuna taşımadan önce verilerin sıkıştırılıp sıkıştırılmayacağını ve sonra verileri bir ara veya hazırlama dat 'dan taşımadan önce sıkıştırması açılacak şekilde belirtebilirsiniz Havuz veri deposuna bir mağaza.

Şu anda, otomatik olarak barındırılan farklı IRS 'ler arasında bağlı olan iki veri deposu arasında veri kopyalayamazsınız, ancak hazırlanmamış kopya olmadan, verileri kopyalayamazsınız. Bu tür senaryolar için, kaynaktan hazırlama ve daha sonra hazırlama durumundan havuza kopyalamak üzere iki açık zincirleme kopyalama etkinliği yapılandırabilirsiniz.

### <a name="configuration"></a>Yapılandırma

Hedef veri deposuna yüklemeden önce, verilerin blob depolamada hazırlanması isteyip istemediğinizi belirtmek için kopyalama etkinliğinde **Enablehazırlama** ayarını yapılandırın. **Enablehazırlama** `TRUE`' ı ayarladığınızda, aşağıdaki tabloda listelenen ek özellikleri belirtin. Ayrıca, yoksa, hazırlama için bir Azure depolama veya depolama paylaşılan erişim imzası ile bağlantılı hizmet oluşturmanız gerekir.

| Özellik | Açıklama | Varsayılan değer | Gerekli |
| --- | --- | --- | --- |
| enableStaging |Veri deposunu hazırlama bir geçiş aracılığıyla kopyalamak isteyip istemediğinizi belirtin. |False |Hayır |
| linkedServiceName |Adını bir [AzureStorage](connector-azure-blob-storage.md#linked-service-properties) bağlı hizmeti, geçici bir hazırlama deposu kullanan depolama örneğine başvurur. <br/><br/> PolyBase aracılığıyla SQL veri ambarı 'na veri yüklemek için, paylaşılan erişim imzasıyla depolama kullanamazsınız. Diğer tüm senaryolarda kullanabilirsiniz. |Yok |Evet, **enableStaging** TRUE olarak ayarlayın |
| path |Hazırlanmış verinin içermesini istediğiniz Blob Depolama yolu belirtin. Bir yol sağlamazsanız, hizmet geçici verileri depolamak için bir kapsayıcı oluşturur. <br/><br/> Yalnızca depolama ile paylaşılan erişim imzası kullanın veya geçici veri belirli bir konumda olmasını gerektiren bir yol belirtin. |Yok |Hayır |
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

## <a name="performance-tuning-steps"></a>Performans ayarlama adımları

Kopyalama etkinliğiyle Azure Data Factory hizmetinizin performansını ayarlamak için bu adımları uygulayın.

1. **Taban çizgisi oluşturun.** Geliştirme aşamasında, bir temsili veri örneğine göre kopyalama etkinliğini kullanarak işlem hattınızı test edin. [Kopyalama etkinliği izlemeyi](copy-activity-overview.md#monitoring)izleyen yürütme ayrıntılarını ve performans özelliklerini toplayın.

2. **Performansı tanılayın ve iyileştirin.** Gözlemlediğiniz performans beklentilerinizi karşılamıyorsa, performans sorunlarını belirleyin. Ardından, kaldırmak veya performans etkisini azaltmak için performansı iyileştirin.

    Bazı durumlarda, Azure Data Factory bir kopyalama etkinliği çalıştırdığınızda, aşağıdaki örnekte gösterildiği gibi [kopyalama etkinliği izleme sayfasının](copy-activity-overview.md#monitor-visually)en üstünde yer alan "performans ayarlama ipuçları" iletisini görürsünüz. İleti size verilen kopya çalıştırması için tanımlanan performans sorunlarını söyler. Ayrıca, kopyalama aktarım hızını artırmak için nelerin değiştirileceği konusunda size rehberlik eder. Performans ayarlama ipuçları şu anda şu şekilde öneriler sunmaktadır:

    - Azure SQL veri ambarı 'na veri kopyaladığınızda PolyBase kullanın.
    - Veri deposu tarafındaki kaynak performans sorunu olduğunda Azure Cosmos DB Istek birimlerini veya Azure SQL veritabanı DTU 'ları (veritabanı Işleme birimleri) artırın.
    - Gereksiz hazırlanmış kopyayı kaldırın.

    Performans ayarlama kuralları da kademeli olarak zenginleşmelidir.

    **Örnek: Performans ayarlama ipuçlarıyla Azure SQL veritabanına kopyalama**

    Bu örnekte, bir kopya çalıştırması sırasında, Azure SQL veritabanı 'nın, yazma işlemlerini yavaşlatan yüksek DTU kullanımına ulaştığını Azure Data Factory bildirimler. Öneri, Azure SQL veritabanı katmanını daha fazla DTU ile artırmaya yönelik bir öneridir. 

    ![Performansı ayarlama ipuçlarıyla izleme kopyalama](./media/copy-activity-overview/copy-monitoring-with-performance-tuning-tips.png)

    Buna ek olarak, bazı yaygın hususlar aşağıda verilmiştir. Performans Tanılama 'nın tam açıklaması Bu makalenin kapsamı dışındadır.

   * Performans özellikleri:
     * [Paralel kopyalama](#parallel-copy)
     * [Veri Tümleştirme Birimleri](#data-integration-units)
     * [Hazırlanmış kopya](#staged-copy)
     * [Şirket içinde barındırılan tümleştirme çalışma zamanı ölçeklenebilirliği](concepts-integration-runtime.md#self-hosted-integration-runtime)
   * [Şirket içinde barındırılan integration runtime](#considerations-for-self-hosted-integration-runtime)
   * [Kaynak](#considerations-for-the-source)
   * [Havuz](#considerations-for-the-sink)
   * [Serileştirme ve seri durumundan çıkarma](#considerations-for-serialization-and-deserialization)
   * [Sıkıştırma](#considerations-for-compression)
   * [Sütun eşleme](#considerations-for-column-mapping)
   * [Diğer konular](#other-considerations)

3. **Yapılandırmayı tüm veri kümeniz için genişletin.** Yürütme sonuçları ve performansından memnun olduğunuzda, tüm veri kümenizi kapsayacak şekilde tanımı ve işlem hattını genişletebilirsiniz.

## <a name="considerations-for-self-hosted-integration-runtime"></a>Şirket içinde barındırılan tümleştirme çalışma zamanı konuları

Kopyalama etkinliğiniz şirket içinde barındırılan bir tümleştirme çalışma zamanı üzerinde çalışıyorsa aşağıdakilere göz önünde bulunur:

**Kurulum**: Tümleştirme çalışma zamanını barındırmak için adanmış bir makine kullanmanızı öneririz. [Şirket içinde barındırılan tümleştirme çalışma zamanını kullanma konularına göz](concepts-integration-runtime.md)atın.

**Ölçeği genişletme**: Bir veya daha fazla düğümü olan tek bir mantıksal şirket içinde barındırılan tümleştirme çalışma zamanı, aynı anda birden çok kopyalama etkinliği çalışmasına servis sunabilir. Çok sayıda eşzamanlı kopyalama etkinliği çalıştırması veya kopyalamak için büyük miktarda veri içeren karma veri hareketi hakkında ağır gereksiniminiz varsa, daha fazla kaynak sağlamak için [Şirket içinde barındırılan tümleştirme çalışma zamanının ölçeğini](create-self-hosted-integration-runtime.md#high-availability-and-scalability) azaltmayı göz önünde bulundurun.

## <a name="considerations-for-the-source"></a>Kaynağı için konular

### <a name="general"></a>Genel

Temel alınan veri deposunun üzerinde veya üzerinde çalışan diğer iş yükleri tarafından çalışmadığından emin olun.

Microsoft veri depoları için, bkz. veri depolarına özgü [konuları izleme ve ayarlama](#performance-reference) . Bu konular, veri deposu performans özelliklerine ve yanıt sürelerini en aza indirmek ve aktarım hızını en üst düzeye çıkarmak nasıl anlamanıza yardımcı olabilir.

* Blob depolamadaki verileri SQL veri ambarı 'na kopyalarsanız, performansı artırmak için PolyBase kullanmayı düşünün. Daha fazla bilgi için [Azure SQL veri ambarı'na veri yüklemek için PolyBase kullanma](connector-azure-sql-data-warehouse.md#use-polybase-to-load-data-into-azure-sql-data-warehouse).
* Azure Blob depolama alanına veya Azure Data Lake Store veri kopyaladığınızda, performansı artırmak için DistCp kullanmayı göz önünde bulundurun. Daha fazla bilgi için bkz. bir [sunucudan verileri kopyalamak Için DistCp kullanma](connector-hdfs.md#use-distcp-to-copy-data-from-hdfs).
* Redshift 'den Azure SQL veri ambarı 'na, Azure BLob depolama alanına veya Azure Data Lake Store veri kopyalarsanız, performansı artırmak için KALDıRMA kullanmayı göz önünde bulundurun. Daha fazla bilgi için bkz. [Amazon Redshift 'tan veri kopyalamak IÇIN kaldırma kullanma](connector-amazon-redshift.md#use-unload-to-copy-data-from-amazon-redshift).

### <a name="file-based-data-stores"></a>Dosya tabanlı veri depoları

* **Ortalama dosya boyutu ve dosya sayısı**: Kopyalama etkinliği verileri tek seferde bir dosyaya aktarır. İle aynı taşınacak veri miktarına, hizmetin genel performansını veriler nedeniyle her dosya için önyükleme aşaması birkaç büyük dosyalar yerine çok sayıda küçük dosya oluşuyorsa düşüktür. Mümkünse, daha yüksek aktarım hızı kazanmak için küçük dosyaları daha büyük dosyalara birleştirin.
* **Dosya biçimi ve sıkıştırma**: Performansı geliştirmenin daha fazla yolu için, [serileştirme ve seri durumdan çıkarma](#considerations-for-serialization-and-deserialization) ve sıkıştırma bölümlerine [dikkat](#considerations-for-compression) edilmesi gerekenler bölümüne bakın.

### <a name="relational-data-stores"></a>İlişkisel veri deposu

* **Veri stili**: Tablo şemanızı kopyalama aktarım hızını etkiler. Büyük bir satır boyutu, aynı miktarda veriyi kopyalamak için küçük bir satır boyutundan daha iyi bir performans sağlar. Veritabanı daha az satır içeren veri daha az toplu işler daha verimli bir şekilde alabilirsiniz nedenidir.
* **Sorgu veya saklı yordam**: Verileri daha verimli bir şekilde getirmek için kopyalama etkinliği kaynağında belirttiğiniz sorgu veya saklı yordamın mantığını iyileştirin.

## <a name="considerations-for-the-sink"></a>Havuz için dikkat edilmesi gerekenler

### <a name="general"></a>Genel

Temel alınan veri deposunun üzerinde veya üzerinde çalışan diğer iş yükleri tarafından çalışmadığından emin olun.

Microsoft veri depoları için, bkz. veri depolarına özgü [konuları izleme ve ayarlama](#performance-reference) . Bu konular, veri deposu performans özelliklerine ve yanıt sürelerini en aza indirmek ve aktarım hızını en üst düzeye çıkarmak nasıl anlamanıza yardımcı olabilir.

* Herhangi bir veri deposundan Azure SQL veri ambarı 'na veri kopyalarsanız, performansı artırmak için PolyBase kullanmayı düşünün. Daha fazla bilgi için [Azure SQL veri ambarı'na veri yüklemek için PolyBase kullanma](connector-azure-sql-data-warehouse.md#use-polybase-to-load-data-into-azure-sql-data-warehouse).
* Azure Blob depolama alanına veya Azure Data Lake Store veri kopyaladığınızda, performansı artırmak için DistCp kullanmayı göz önünde bulundurun. Daha fazla bilgi için bkz. bir [sunucudan verileri kopyalamak Için DistCp kullanma](connector-hdfs.md#use-distcp-to-copy-data-from-hdfs).
* Redshift 'den Azure SQL veri ambarı 'na, Azure Blob depolama alanına veya Azure Data Lake Store veri kopyalarsanız, performansı artırmak için KALDıRMA kullanmayı göz önünde bulundurun. Daha fazla bilgi için bkz. [Amazon Redshift 'tan veri kopyalamak IÇIN kaldırma kullanma](connector-amazon-redshift.md#use-unload-to-copy-data-from-amazon-redshift).

### <a name="file-based-data-stores"></a>Dosya tabanlı veri depoları

* **Kopyalama davranışı**: Farklı dosya tabanlı bir veri deposundan veri kopyalarsanız kopyalama etkinliğinin **Copybehavior** özelliği aracılığıyla üç seçeneği vardır. Hiyerarşi korur, hiyerarşi düzleştirir veya dosyayı birleştirir. Koruma veya hiyerarşi düzleştirme çok az kayıpla veya hiç performansa sahiptir, ancak dosyaları birleştirme artırmak performansa neden olur.
* **Dosya biçimi ve sıkıştırma**: Performansı geliştirmenin daha fazla yolu için, [serileştirme ve seri durumdan çıkarma](#considerations-for-serialization-and-deserialization) ve sıkıştırma bölümlerine [dikkat](#considerations-for-compression) edilmesi gerekenler bölümüne bakın.

### <a name="relational-data-stores"></a>İlişkisel veri deposu

* **Kopyalama davranışı ve performans uygulaması**: Verileri bir SQL havuzuna yazmanın farklı yolları vardır. [Azure SQL veritabanı 'na veri yüklemek Için en iyi uygulamalardan](connector-azure-sql-database.md#best-practice-for-loading-data-into-azure-sql-database)daha fazla bilgi edinin.

* **Veri düzeni ve toplu işlem boyutu**:
  * Tablo şemanızı kopyalama aktarım hızını etkiler. Veritabanı daha verimli bir şekilde daha az toplu veri kaydedebilir miyim çünkü aynı miktarda veri kopyalamak için büyük satır boyutu, küçük satır boyutu daha iyi performans sağlar.
  * Kopyalama etkinliği, verileri bir dizi toplu iş halinde ekler. Bir toplu işte satır sayısını kullanarak ayarlayabilirsiniz **writeBatchSize** özelliği. Verilerinizi küçük satır varsa, ayarlayabileceğiniz **writeBatchSize** özellik toplu işlem ek yükü azaltır ve daha yüksek performans avantajlarından yararlanarak daha yüksek bir değere sahip. Verilerinizin satır boyutu büyükse, artırdığınızda dikkat **writeBatchSize**. Yüksek bir değere bir kopyalama hatasının veritabanı aşırı yüklenerek neden neden olabilir.

### <a name="nosql-stores"></a>NoSQL depoları

* İçin **tablo depolama**:
  * **Bölüm**: Araya eklemeli bölümlere veri yazmak performansı önemli ölçüde düşürür. Verilerin bir bölümden sonra bir bölüme etkili bir şekilde eklenmesini sağlamak için kaynak verilerinizi bölüm anahtarına göre sıralayın. Ya da verileri tek bir bölüme yazma mantığını ayarlayabilirsiniz.

## <a name="considerations-for-serialization-and-deserialization"></a>Serileştirme ve seri durumundan çıkarma için dikkat edilmesi gerekenler

Giriş veri kümeniz veya çıkış veri kümeniz bir dosya olduğunda serileştirme ve seri durumdan çıkarma gerçekleşebilir. Kopyalama etkinliğine göre desteklenen dosya biçimleri hakkında daha fazla bilgi için bkz. [Desteklenen dosya ve sıkıştırma biçimleri](supported-file-formats-and-compression-codecs.md).

**Kopyalama davranışı**:

* Dosya tabanlı veri depoları arasında dosyaları kopyalanıyor:
  * Giriş ve çıkış veri kümelerinde her ikisi de aynı veya dosya biçimi ayarlarına sahip olduğunda, veri taşıma hizmeti herhangi bir serileştirme veya serisini kaldırma olmadan bir *ikili kopya* yürütür. Kaynak ve havuz dosya biçimi ayarları birbirinden farklı senaryo ile karşılaştırıldığında daha yüksek bir aktarım hızı görürsünüz.
  * Hem giriş hem de çıkış veri kümeleri metin biçiminde olduğunda ve yalnızca kodlama türü farklıysa, veri taşıma hizmeti yalnızca kodlama dönüştürmesi yapar. Herhangi bir serileştirme yapmaz ve seri durumdan yükü ikili kopyasını kıyasla bazı performans neden olur.
  * Giriş ve çıkış veri kümelerinde her ikisi de sınırlayıcılar gibi farklı dosya biçimlerine veya farklı yapılandırmalara sahip olduğunda, veri taşıma hizmeti kaynak verileri akışa alma, dönüştürme ve daha sonra belirttiğiniz çıkış biçiminde serileştirmez. Bu işlem ek yükü diğer senaryolarda kıyasla çok daha önemli bir performans sonuçlanır.
* Dosya tabanlı olmayan bir veri deposuna veya dosyasına dosya kopyaladığınızda (örneğin, dosya tabanlı bir mağazadan ilişkisel bir depoya), serileştirme veya serisini kaldırma adımı gereklidir. Bu adım, önemli performans ek yükü oluşur.

**Dosya biçimi**: Seçtiğiniz dosya biçimi, kopyalama performansını etkileyebilir. Örneğin, Avro verileriyle meta verileri depolar sıkıştırılmış bir ikili biçimi ' dir. Hadoop ekosistemindeki işleme ve sorgulama için geniş destek vardır. Avro, serileştirme ve seri durumundan çıkarma için daha pahalıdır, bu da metin biçimiyle karşılaştırıldığında daha düşük kopya üretilen iş elde edilir. 

Dosya biçimi işleme akışı boyunca tercih ettiğiniz bütünlüklü olarak yapın. Başlangıç:

- Verilerin depolandığı form, kaynak veri depoları veya dış sistemlerden ayıklanmak üzere.
- Depolama, analitik işleme ve sorgulama için en iyi biçim.
- Verilerin raporlama ve görselleştirme araçları için veri Reyonları 'na aktarılması gereken biçim.

Bazen için yetersiz bir dosya biçimi okuma ve yazma performansını genel analitik işlemi düşünürken iyi bir seçim olabilir.

## <a name="considerations-for-compression"></a>Sıkıştırma dikkate alınacak noktalar

Giriş veya çıkış veri kümeniz bir dosya olduğunda kopyalama etkinliğini, hedefe veri yazarken sıkıştırma veya sıkıştırmayı gerçekleştirecek şekilde ayarlayabilirsiniz. Sıkıştırma seçtiğinizde, giriş/çıkış (g/ç) arasında bir denge duruma ve CPU. Bilgi işlem kaynaklarının ek veri maliyetlerini sıkıştırma. Ancak, ağ g/ç ve depolama azaltır. Verilerinize bağlı olarak, genel kopya aktarım hızına yönelik bir artırma görebilirsiniz.

**Codec bileşeni**: Her sıkıştırma codec bileşeni avantajlara sahiptir. Örneğin, bzıp2 düşük kopyalama aktarım hızına sahip, ancak işleme için bölme çünkü bzıp2 ile en iyi Hive sorgu performansı elde. Gzip en dengeli seçenektir ve en sık kullanılır. Uçtan uca senaryonuza uygun codec bileşeni seçin.

**Düzey**: Her sıkıştırma codec bileşeni için iki seçenekten birini tercih edebilirsiniz: en hızlı sıkıştırılmış ve en iyi sıkıştırılmış. En hızlı sıkıştırılmış seçenek, elde edilen dosya en iyi şekilde sıkıştırımasa bile, verileri olabildiğince çabuk sıkıştırır. En uygun şekilde sıkıştırılmış seçeneği sıkıştırma üzerinde daha fazla zaman harcadığını ve en az miktarda veri ortaya çıkarır. İki seçenek de durumunuzdaki genel daha iyi performans sağlayan görmek için test edebilirsiniz.

**Göz önünde bulundurun**: Şirket içi bir mağaza ve bulut arasında büyük miktarda veriyi kopyalamak için, sıkıştırma etkinken [hazırlanmış kopyayı](#staged-copy) kullanmayı düşünün. Şirket ağınızın ve Azure hizmetlerinizin bant genişliği sınırlayan faktör olduğunda ve giriş veri kümesi ve çıktı veri kümesinin her ikisi de sıkıştırılmamış biçimde olmasını istiyorsanız geçici depolama kullanmak faydalıdır.

## <a name="considerations-for-column-mapping"></a>Sütun eşlemesi için dikkat edilmesi gerekenler

Bir kopyalama etkinliğinde **ColumnMappings** özelliğini, giriş sütunlarının tümünü veya bir alt kümesini çıkış sütunlarına eşlemek için ayarlayabilirsiniz. Veri taşıma Hizmeti'nde veri kaynağından okur. sonra havuz için verileri yazar önce veriler üzerinde sütun eşleme gerçekleştirmek gerekir. Bu ek işlem kopyalama performansını düşürür.

Kaynak veri deponuzda sorgulanabilir, örneğin, SQL veritabanı veya SQL Server gibi ilişkisel bir mağaza veya tablo depolama veya Azure Cosmos DB gibi bir NoSQL deposu olması durumunda sütun filtreleme gönderme ve mantığı yeniden sıralama göz önünde bulundurun **sorgu** sütun eşlemesi kullanmak yerine özellik. Bu şekilde, veri taşıma hizmeti, kaynak veri deposundan verileri okurken ve çok daha verimli olduğu için projeksiyon oluşur.

[Kopyalama etkinliği şema eşlemesiyle](copy-activity-schema-and-type-mapping.md)daha fazla bilgi edinin.

## <a name="other-considerations"></a>Dikkat edilecek diğer noktalar

Kopyalamak istediğiniz veri boyutu büyükse, verileri daha fazla bölümlemek için iş mantığınızı ayarlayabilirsiniz. Kopyalama etkinliğini, çalıştıran her kopyalama etkinliğinin veri boyutunu azaltmak için daha sık çalışacak şekilde zamanlayabilirsiniz.

Aynı anda aynı veri deposuna bağlanmak için Azure Data Factory gerektiren veri kümesi ve kopyalama etkinliği sayısı hakkında dikkatli olun. Birçok eş zamanlı kopyası işleri bir veri deposu azaltma ve performansın düşmesine neden, kopyalama işi iç deneme ve bazı durumlarda, yürütme hatalarını sağlama.

## <a name="sample-scenario-copy-from-an-on-premises-sql-server-to-blob-storage"></a>Örnek senaryo: Şirket içi SQL Server 'dan blob depolamaya kopyalama

**Senaryo**: Bir işlem hattı, verileri şirket içi SQL Server 'dan CSV biçiminde blob depolamaya kopyalamak için oluşturulmuştur. Kopyalama işini daha hızlı hale getirmek için CSV dosyaları bzıp2 biçimine sıkıştırılmasının gerekli.

**Test ve analiz**: Kopyalama etkinliğinin verimlilik 2 MBps 'den azdır, bu da performans kıyaslamasından çok daha yavaştır.

**Performans Analizi ve ayarlama**: Performans sorununu gidermek için, verilerin nasıl işlendiğini ve taşındığını inceleyelim.

- **Verileri oku**: Integration Runtime SQL Server bir bağlantı açar ve sorguyu gönderir. SQL Server, veri akışını intranet aracılığıyla tümleştirme çalışma zamanına göndererek yanıt verir.
- **Verileri serileştirme ve sıkıştırma**: Integration Runtime, veri akışını CSV biçimine seri hale getirir ve verileri bir bzip2 akışına sıkıştırır.
- **Veri yaz**: Integration Runtime, bzip2 akışını Internet üzerinden blob depolamaya yükler.

Gördüğünüz gibi, veriler işlenir ve akış sıralı bir şekilde taşınır: SQL Server > LAN > Integration Runtime > WAN > BLOB depolama. Genel performans, işlem hattı genelinde en düşük aktarım hızına göre işlenir.

![Veri akışı](./media/copy-activity-performance/case-study-pic-1.png)

Bir veya daha fazla aşağıdaki faktörleri performans sorunu neden olabilir:

* **Kaynak**: SQL Server, ağır yükler nedeniyle düşük aktarım hızına sahiptir.
* **Şirket içinde barındırılan tümleştirme çalışma zamanı**:
  * **LAN**: Tümleştirme çalışma zamanı SQL Server makineden uzakta bulunur ve düşük bant genişliğine sahip bir bağlantıya sahiptir.
  * **Tümleştirme çalışma zamanı**: Tümleştirme çalışma zamanı, aşağıdaki işlemleri gerçekleştirmek için yük kısıtlamalarına ulaştı:
    * **Serileştirme**: Veri akışının CSV biçimine serileştirilmesi, performansı düşürür.
    * **Sıkıştırma**: Core i7 ile 2,8 MBps olan bzip2 gibi yavaş bir sıkıştırma codec bileşeni seçtiniz.
  * **WAN**: Şirket ağı ile Azure hizmetleriniz arasındaki bant genişliği düşük, örneğin T1 = 1.544 kbps; T2 = 6.312 kbps.
* **Havuz**: Blob depolamada düşük aktarım hızı vardır. Hizmet düzeyi sözleşmesi (SLA) en az 60 MBps 'Lik bir değer sağladığından bu senaryo çok düşüktür.

Bu durumda, bzıp2 veri sıkıştırma tüm işlem hattını yavaşlatmasını. Gzip sıkıştırma codec bileşenine geçiş, bu performans sorunu kolaylaştırmak.

## <a name="references"></a>Referanslar

Desteklenen bazı veri depoları için performans izleme ve ayarlama başvuruları aşağıda verilmiştir:

* Blob depolamayı ve tablo depolamayı içeren Azure Storage: [Azure depolama ölçeklenebilirlik hedefleri](../storage/common/storage-scalability-targets.md) ve [Azure depolama performansı ve ölçeklenebilirlik denetim listesi](../storage/common/storage-performance-checklist.md).
* Azure SQL Veritabanı: [Performansı izleyebilir](../sql-database/sql-database-single-database-monitor.md) ve veritabanı işlem BIRIMI (DTU) yüzdesini kontrol edebilirsiniz.
* Azure SQL veri ambarı: Özelliği, veri ambarı birimlerinde (DWU) ölçülür. Bkz. [Azure SQL veri ambarı 'nda işlem gücünü yönetme (genel bakış)](../sql-data-warehouse/sql-data-warehouse-manage-compute-overview.md).
* Azure Cosmos DB: [Azure Cosmos DB 'de performans düzeyleri](../cosmos-db/performance-levels.md).
* Şirket içi SQL Server: [Performansı izleyin ve ayarlayın](https://msdn.microsoft.com/library/ms189081.aspx).
* Şirket içi dosya sunucusu: [Dosya sunucuları Için performans ayarlama](https://msdn.microsoft.com/library/dn567661.aspx).

## <a name="next-steps"></a>Sonraki adımlar
Diğer kopyalama etkinliği makalelerine bakın:

- [Kopyalama etkinliği'ne genel bakış](copy-activity-overview.md)
- [Etkinlik şeması eşlemesini Kopyala](copy-activity-schema-and-type-mapping.md)
- [Kopyalama etkinliği hataya dayanıklılık](copy-activity-fault-tolerance.md)
