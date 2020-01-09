---
title: Etkinlik performansı ve ayarlama Kılavuzu kopyalayın
description: Kopyalama etkinliği kullandığınızda, Azure Data factory'de veri taşımayı performansını etkileyen anahtar Etkenler hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
ms.assetid: 4b9a6a4f-8cf5-4e0a-a06f-8133a2b7bc58
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 05/25/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 9ca44b1917cfaed5d01c31f8f06d98e5e4b611a8
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75438933"
---
# <a name="copy-activity-performance-and-tuning-guide"></a>Etkinlik performansı ve ayarlama Kılavuzu kopyalayın

> [!div class="op_single_selector" title1="Kullandığınız Data Factory hizmeti sürümünü seçin:"]
> * [Sürüm 1](data-factory-copy-activity-performance.md)
> * [Sürüm 2 (geçerli sürüm)](../copy-activity-performance.md)

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Data Factory hizmetinin geçerli sürümünü kullanıyorsanız, bkz. [Data Factory için etkinlik performansını ve ayarlama kılavuzunu kopyalama](../copy-activity-performance.md).

Azure Data Factory kopyalama etkinliği, çözüm yüklenirken birinci sınıf bir güvenli, güvenilir ve yüksek performanslı veri sunar. Birçok farklı bulut her gün terabaytlarca veriyi onlarca kopyasını sağlar ve şirket içi veri depoları. İnanılmaz derecede hızlı veri yükleme performansını anahtarıdır çekirdek "büyük veri" soruna odaklanmak emin olmak için: Gelişmiş analiz çözümleri oluşturmak ve tüm bu verilerden ayrıntılı Öngörüler alma.

Azure Kurumsal düzeyde veri depolama ve veri ambarı çözümleri sunmaktadır ve kopyalama etkinliği yapılandırmak ve ayarlamak kolaydır deneyimi yüklenirken yüksek oranda iyileştirilmiş bir veri sunar. Yalnızca tek bir kopyalama etkinlikli, elde edebilirsiniz:

* Verileri yükleme **Azure SQL veri ambarı** adresindeki **1,2 GB/sn**. Kullanım örneği ile bir kılavuz için bkz. [1 TB 15 dakikadan daha kısa Azure Data Factory ile Azure SQL Data Warehouse'a veri yükleme](data-factory-load-sql-data-warehouse.md).
* Verileri yükleme **Azure Blob Depolama** adresindeki **1,0 GB/sn**
* Verileri yükleme **Azure Data Lake Store** adresindeki **1,0 GB/sn**

Bu makalede açıklanır:

* [Performans başvuru sayıları](#performance-reference) desteklenen; proje planlamanıza yardımcı olması için kaynak ve havuz veri deposu
* [Bulut veri taşıma birimleri](#cloud-data-movement-units), [paralel kopyalama](#parallel-copy)ve [hazırlanan kopya](#staged-copy)dahil olmak üzere farklı senaryolarda kopyalama aktarım hızını arttıran özellikler;
* [Performans ayarlama Kılavuzu](#performance-tuning-steps) performans ve kopyalama performansı etkileyen önemli faktörlerin ayarlama konusunda.

> [!NOTE]
> Kopyalama etkinliğini genel olarak bilmiyorsanız, bu makaleyi okumadan önce [kopyalama etkinliğini kullanarak verileri taşıma](data-factory-data-movement-activities.md) bölümüne bakın.
>

## <a name="performance-reference"></a>Performans başvurusu

Bir başvuru olarak, aşağıdaki tabloda, şirket içi test temelinde verilen kaynak ve havuz çiftleri için aktarım hızı sayısını MB/sn cinsinden gösterir. Karşılaştırma için aynı zamanda [bulut veri taşıma birimleri](#cloud-data-movement-units) veya [veri yönetimi ağ geçidi ölçeklenebilirliği](data-factory-data-management-gateway-high-availability-scalability.md) (birden çok ağ geçidi düğümleri) farklı ayarlarının, kopyalama performansı konusunda nasıl yardımcı olabileceğini gösterir.

![Performans Matrisi](./media/data-factory-copy-activity-performance/CopyPerfRef.png)

>[!IMPORTANT]
>Azure Data Factory sürüm 1 ' de, buluttan buluta kopyalama için en az bulut veri taşıma birimi ikdir. Belirtilmemişse, [bulut veri taşıma birimlerinde](#cloud-data-movement-units)kullanılan varsayılan veri taşıma birimleri bölümüne bakın.

**Şunlara işaret eder:**
* Aşağıdaki formülü kullanarak aktarım hızı hesaplanır: [kaynak Okuma boyutu veri] / [kopyalama etkinliği çalıştırma süresi].
* Tablodaki performans başvuru numaraları, tek bir kopyalama etkinliği çalıştırmasında, [TPC-H](http://www.tpc.org/tpch/) veri kümesi kullanılarak ölçülür.
* Azure veri depoları, kaynak ve havuz aynı Azure bölgesinde olan.
* Şirket içi ve bulut veri depoları arasındaki karma kopya için, her ağ geçidi düğümü, aşağıdaki belirtim ile şirket içi veri deposundan ayrı olan bir makinede çalışmaktadır. Ağ geçidinde tek bir etkinlik çalıştırılırken, kopyalama işlemi test makinesinin CPU, bellek veya ağ bant genişliğinin yalnızca küçük bir kısmını kullandı. [Veri yönetimi ağ geçidi için dikkat etmeniz gereken](#considerations-for-data-management-gateway)daha fazla bilgi edinin.
    <table>
    <tr>
        <td>CPU</td>
        <td>2\.20 GHz Intel Xeon E5-2660 v2 32 çekirdek</td>
    </tr>
    <tr>
        <td>Hafıza</td>
        <td>128 GB</td>
    </tr>
    <tr>
        <td>Ağ</td>
        <td>Internet arabirimi: 10 GB/sn; intranet arabiriminde: 40 GB/sn</td>
    </tr>
    </table>


> [!TIP]
> Buluttan buluta kopyalama etkinliği çalıştırması için 32 olan varsayılan maksimum DMUs değerinden daha fazla veri taşıma birimi (DMU) kullanarak daha yüksek aktarım hızı elde edebilirsiniz. Örneğin, 100 DMUs ile, verileri Azure Blob 'tan **1,0 GB/sn**'ye kopyalamaya Azure Data Lake Store sağlayabilirsiniz. Bu özellik ve desteklenen senaryo hakkındaki ayrıntılar için [bulut verileri taşıma birimleri](#cloud-data-movement-units) bölümüne bakın. Daha fazla DMUs istemek için [Azure desteğine](https://azure.microsoft.com/support/) başvurun.

## <a name="parallel-copy"></a>Paralel kopya
Kaynaktan verileri okuyabilir veya **bir kopyalama etkinliği çalıştırmasında paralel olarak**hedefe veri yazabilirsiniz. Bu özellik bir kopyalama işleminin verimini geliştirir ve verilerin taşınması için geçen süreyi azaltır.

Bu ayar, etkinlik tanımındaki **eşzamanlılık** özelliğinden farklıdır. **Eşzamanlılık** özelliği, farklı etkinlik pencerelerinin verilerini işlemek Için **eşzamanlı kopyalama etkinliği** sayısını belirler (1, 2, 2, 3, 10:00, 3,-4, vb.). Bu özellik bir geçmiş yükü gerçekleştirdiğinizde faydalıdır. Paralel kopyalama özelliği **tek bir etkinlik çalıştırması**için geçerlidir.

Örnek senaryoya göz atalım. Aşağıdaki örnekte, geçmişteki bir çok dilimin işlenmesi gerekir. Data Factory her dilim için bir kopyalama etkinliği (etkinlik çalıştırması) örneği çalıştırır:

* İlk etkinlik penceresindeki (1 ' den 2 ' ye kadar) veri dilimi = = > etkinlik çalışması 1
* İkinci etkinlik penceresindeki veri dilimi (2-3 ' e kadar) = = > etkinlik çalışması 2
* İkinci etkinlik penceresindeki veri dilimi (3 ile 4 ' e kadar) = = > etkinlik çalıştırması 3

Etki alanları bu hiyerarşi sıralamasıyla devam eder.

Bu örnekte, **eşzamanlılık** değeri 2 olarak ayarlandığında, **etkinlik 1** ' i çalıştırın ve etkinlik, veri taşıma performansını geliştirmek için iki etkinlikten **aynı anda** **2** ' yi kopyalayın. Ancak, birden çok dosya etkinlik çalıştırması 1 ile ilişkiliyse, veri taşıma hizmeti dosyaları kaynaktan bir seferde hedef tek bir dosyaya kopyalar.

### <a name="cloud-data-movement-units"></a>Bulut veri taşıma birimleri
Bir **bulut veri taşıma birimi (DMU)** Data Factory içindeki tek bir birimin gücünü (CPU, bellek ve ağ kaynak ayırma birleşimi) temsil eden bir ölçüdür. DMU, karma kopyada değil, buluttan buluta kopyalama işlemleri için geçerlidir.

**Kopyalama etkinliği çalıştırmaya güç sağlamak için en az bulut veri taşıma birimleri iki.** Belirtilmezse, aşağıdaki tablo farklı kopyalama senaryolarında kullanılan varsayılan DMUs 'leri listeler:

| Kopyalama senaryosu | Hizmete göre belirlenen varsayılan DMUs |
|:--- |:--- |
| Dosya tabanlı depoları arasında veri kopyalama | 4 ile 16 arasında, dosyaların sayısına ve boyutuna bağlı olarak. |
| Tüm diğer kopyalama senaryolarında | 4 |

Bu varsayılanı geçersiz kılmak için, **Clouddatamovementunits** özelliği için aşağıdaki gibi bir değer belirtin. **Clouddatamovementunits** özelliği için **izin verilen değerler** 2, 4, 8, 16, 32 ' dir. Kopyalama işleminin çalışma zamanında kullandığı **gerçek bulut DMU sayısı** , veri örünseçeneğe bağlı olarak yapılandırılan değere eşit veya ondan daha az. Özel kopyalama kaynağı ve havuz için daha fazla birimi yapılandırırken alabilirsiniz performans kazancı düzeyi hakkında bilgi için bkz [Performans başvurusu](#performance-reference).

```json
"activities":[
    {
        "name": "Sample copy activity",
        "description": "",
        "type": "Copy",
        "inputs": [{ "name": "InputDataset" }],
        "outputs": [{ "name": "OutputDataset" }],
        "typeProperties": {
            "source": {
                "type": "BlobSource",
            },
            "sink": {
                "type": "AzureDataLakeStoreSink"
            },
            "cloudDataMovementUnits": 32
        }
    }
]
```

> [!NOTE]
> Daha yüksek bir verimlilik için daha fazla bulut DMUs gerekiyorsa [Azure desteği](https://azure.microsoft.com/support/)'ne başvurun. 8 ve üzeri ayarı şu anda yalnızca **BLOB depolama/Data Lake Store/Amazon S3/Cloud FTP/Cloud SFTP 'Den blob Storage/Data Lake Store/Azure SQL veritabanı 'na birden çok dosya kopyaladığınızda**geçerlidir.
>

### <a name="parallelcopies"></a>parallelCopies
Kullanabileceğiniz **parallelCopies** kopyalama etkinliği, kullanmak istediğiniz paralellik belirtmek için özelliği. Bu özellik veri kaynağınızdan okuyabilen veya, havuz veri depolarına paralel yazma kopyalama etkinliği içinde iş parçacığı sayısı olarak düşünebilirsiniz.

Her kopya etkinlik çalıştırma için Data Factory veri depolamak ve için hedef veri deposu kaynak sunucudan veri kopyalamak için paralel kopya sayısını belirler. Varsayılan paralel kopya sayısı, kullanmakta olduğunuz kaynak ve havuz türüne bağlıdır.

| Kaynak ve havuz | Hizmet tarafından belirlenen varsayılan paralel kopya sayısı |
| --- | --- |
| Dosya tabanlı mağazalar (BLOB depolama;) arasında veri kopyalama Data Lake Store; Amazon S3; Şirket içi dosya sistemi; Şirket içi bir " |1 ile 32 arasında. , Verilerin boyutuna ve iki bulut veri deposu arasında veri kopyalamak için kullanılan bulut veri taşıma birimi (DMUs) sayısının veya karma kopya için kullanılan ağ geçidi makinesinin fiziksel yapılandırmasının (örneğin, şirket içi veri deposuna veri kopyalamak için) bağlı olarak değişir. |
| **Herhangi bir kaynak veri deposundan Azure Tablo depolama alanına** veri kopyalama |4 |
| Diğer tüm kaynak ve havuz çiftleri |1 |

Genellikle, varsayılan davranış size en iyi verimi vermelidir. Ancak, veri mağazalarınızı barındıran makinelerde yükü denetlemek veya kopyalama performansını ayarlamak için, varsayılan değeri geçersiz kılmayı ve **Parallelcopy** özelliği için bir değer belirtmeyi seçebilirsiniz. Değer 1 ile 32 (ikisi de dahil) arasında olmalıdır. Çalışma zamanında, en iyi performans için ayarladığınız değerine eşit veya daha az olan bir değer kopyalama etkinliği kullanır.

```json
"activities":[
    {
        "name": "Sample copy activity",
        "description": "",
        "type": "Copy",
        "inputs": [{ "name": "InputDataset" }],
        "outputs": [{ "name": "OutputDataset" }],
        "typeProperties": {
            "source": {
                "type": "BlobSource",
            },
            "sink": {
                "type": "AzureDataLakeStoreSink"
            },
            "parallelCopies": 8
        }
    }
]
```
Dikkat edilecek noktalar:

* Dosya tabanlı depoları arasında veri kopyalama **parallelCopies** paralellik dosya düzeyinde belirlenir. Tek bir dosyada Öbekleme altındaki otomatik ve şeffaf şekilde olacağını ve paralel ve parallelCopies dikgen verileri yüklemek için en uygun öbek boyutu için belirtilen kaynak veri deposu türü kullanmak için tasarlanmıştır. Gerçek veri taşıma Hizmeti'nde kopyalama işleminin çalışma zamanında kullandığı paralel kopya sayısı sahip olduğunuz dosyaların sayısı, en fazla ' dir. Kopyalama davranışını ise **mergeFile**, kopyalama etkinliği, dosya düzeyinde paralellik avantajlarından alamaz.
* **Parallelcopy** özelliği için bir değer belirttiğinizde, kaynak ve havuz Veri depolarındaki yük artışını ve karma kopya ise ağ geçidini göz önünde bulundurun. Bu, özellikle birden çok etkinlikler veya aynı veri deposuna karşı çalışan aynı etkinliklerden eş zamanlı çalıştırma olduğunda gerçekleşir. Veri deposunun veya ağ geçidinin yük ile aşırı olduğunu fark ederseniz, yükü düşürmek için **Parallelkopyaların** değerini azaltın.
* Dosya tabanlı depoları için dosya tabanlı olmayan depolarından verileri kopyaladığınızda, veri taşıma Hizmeti'nde yok sayar **parallelCopies** özelliği. Paralellik belirtilmiş olsa bile, bu durumda uygulanmaz.

> [!NOTE]
> Karma kopya yaparken **Parallelcopy** özelliğini kullanmak Için veri yönetimi Gateway sürüm 1,11 veya üstünü kullanmanız gerekir.
>
>

Bu iki özelliği daha iyi kullanmak ve veri taşıma aktarım hızını geliştirmek için bkz. örnek kullanım örnekleri. Varsayılan davranışın avantajlarından yararlanmak için **Parallelkopyaları** yapılandırmanız gerekmez. Yapılandırma ve **Parallelkopyaları** çok küçükse, birden fazla bulut DMU tam olarak kullanılamayabilir.

### <a name="billing-impact"></a>Faturalama etkisi
Sahip **önemli** tabanlı kopyalama işlemi toplam zamanında ücretlendirilir unutmayın. Bir kopyalama işi tek bir bulut birimiyle bir saat geçmesi için kullanılırsa ve dört bulut birimi ile 15 dakika sürüyorsa, genel faturanız neredeyse aynı kalır. Örneğin, dört bulut birimi kullanırsınız. İlk bulut birimi 10 dakika, ikinci bir, 10 dakika, üçüncü bir, 5 dakika ve dört dakika, hepsi tek bir kopyalama etkinliğinde çalışır. 10 + 10 + 5 + 5 = 30 dakikalık toplam kopyalama (veri taşıma) zamanına göre ücretlendirilirsiniz. **Parallelkopyaları** kullanmak faturalandırmayı etkilemez.

## <a name="staged-copy"></a>Hazırlanmış kopya
Bir kaynak veri deposundan bir havuz veri deposuna veri kopyalama, geçici bir hazırlama deposu Blob depolamayı kullanmak seçebilirsiniz. Hazırlama aşağıdaki durumlarda kullanışlıdır:

1. **PolyBase aracılığıyla SQL veri ambarı'na çeşitli veri depolarından veri alabilen istediğiniz**. SQL veri ambarı PolyBase, SQL veri ambarı'na büyük miktarda veri yüklemek için yüksek performanslı mekanizması olarak kullanır. Ancak, kaynak verilerin BLOB depolama alanında olması ve ek ölçütlere uyması gerekir. BLOB depolama dışında bir veri deposundan veri yüklediğinizde, veri kopyalamayı, geçici hazırlama blob depolaması aracılığıyla etkinleştirebilirsiniz. Bu durumda, Data Factory, PolyBase gereksinimlerini karşıladığından emin olmak için gerekli veri dönüşümleri gerçekleştirir. Ardından verileri SQL Data Warehouse'a veri yüklemek için PolyBase kullanır. Daha ayrıntılı bilgi için bkz. [Azure SQL veri ambarı 'na veri yüklemek Için PolyBase kullanma](data-factory-azure-sql-data-warehouse-connector.md#use-polybase-to-load-data-into-azure-sql-data-warehouse). Kullanım örneği ile bir kılavuz için bkz. [1 TB 15 dakikadan daha kısa Azure Data Factory ile Azure SQL Data Warehouse'a veri yükleme](data-factory-load-sql-data-warehouse.md).
2. **Bazen bir karma veri hareketini (yani, şirket içi veri deposu ile bulut veri deposu arasında kopyalamak için) yavaş bir ağ bağlantısı üzerinden gerçekleştirme işlemi biraz zaman alır**. Performansı artırmak için Şirket içindeki verileri sıkıştırarak verileri bulutta hazırlama veri deposuna taşımak daha az zaman alır. Daha sonra, hedef veri deposuna yüklemeden önce hazırlama deposundaki verileri açabilir.
3. **80 numaralı bağlantı noktası dışındaki bağlantı noktaları açın ve kurumsal BT ilkeleri nedeniyle, güvenlik duvarında 443 bağlantı noktasına istemediğiniz**. Örneğin, bir Azure SQL veritabanı havuz veya bir Azure SQL veri ambarı havuzu için bir şirket içi veri deposundan veri kopyaladığınızda, Windows Güvenlik Duvarı hem kurumsal güvenlik ağınızın 1433 numaralı bağlantı noktasında giden TCP iletişimine'ı etkinleştirmeniz gerekir. Bu senaryoda, bağlantı noktası 443 ' de HTTP veya HTTPS üzerinden bir BLOB depolama hazırlama örneğine ilk olarak veri kopyalamak için ağ geçidinin avantajlarından yararlanın. Daha sonra, blob Storage hazırlama işleminden verileri SQL veritabanı veya SQL veri ambarı 'na yükleyin. Bu akışta 1433 numaralı bağlantı noktasını etkinleştirmek gerekmez.

### <a name="how-staged-copy-works"></a>Nasıl hazırlanmış kopya çalışır
Hazırlama özelliğini etkinleştirdiğinizde, önce veriler kaynak veri deposundan hazırlama veri deposuna kopyalanır (kendinizinkini getirin). Ardından, veri hazırlama veri deposundan havuz veri deposuna kopyalanır. Data Factory, sizin için iki aşamalı akışı otomatik olarak yönetir. Veri taşıma tamamlandıktan sonra veri fabrikası geçici verileri hazırlama depolama biriminden da temizler.

Bulut kopyalama senaryosunda (hem kaynak hem de havuz veri depoları bulutta bulunur) ağ geçidi kullanılmaz. Data Factory hizmeti kopyalama işlemlerini gerçekleştirir.

![Hazırlanan kopya: bulut senaryosu](media/data-factory-copy-activity-performance/staged-copy-cloud-scenario.png)

Karma kopyalama senaryosunda (kaynak şirket içi ve havuz bulutta ise) ağ geçidi, verileri kaynak veri deposundan hazırlama veri deposuna taşıdır. Data Factory hizmeti verileri hazırlama veri deposundan havuz veri deposuna taşıırın. Bir bulut veri deposundan verileri hazırlama yoluyla şirket içi veri deposuna kopyalama, tersine çevrilmiş akışta de desteklenir.

![Hazırlanan kopya: karma senaryo](media/data-factory-copy-activity-performance/staged-copy-hybrid-scenario.png)

Hazırlama deposu kullanarak veri taşıma etkinleştirdiğinizde, verileri bir geçiş veya hazırlama veri deposu için kaynak veri deposundan veri taşımadan önce sıkıştırılmış ve ardından bir geçiş veri taşıma ve veri hazırlık önce eklenmişti isteyip istemediğinizi belirtebilirsiniz Havuz veri deposuna depolayın.

Şu anda, hazırlama deposu kullanarak iki şirket içi veri depoları arasında veri kopyalanamıyor. Bu seçeneğin yakında kullanılabilir olmasını bekler.

### <a name="configuration"></a>Yapılandırma
Yapılandırma **enableStaging** verileri Blob Depolama alanında çoğaltılmadan önce bir hedef veri deposuna yüklemek isteyip istemediğinizi belirtmek için kopyalama etkinliği ayarlama. **Enablehazırlama** özelliğini true olarak belirlediğinizde, sonraki tabloda listelenen ek özellikleri belirtin. Yoksa, depolama paylaşılan erişim imzası bağlı hizmeti hazırlama için veya bir Azure depolama oluşturulması gerekir.

| Özellik | Açıklama | Varsayılan değer | Gereklidir |
| --- | --- | --- | --- |
| **enableStaging** |Veri deposunu hazırlama bir geçiş aracılığıyla kopyalamak isteyip istemediğinizi belirtin. |Yanlış |Hayır |
| **linkedServiceName** |Bir [Azurestorage](data-factory-azure-blob-connector.md#azure-storage-linked-service) veya [Azurestokıgesas](data-factory-azure-blob-connector.md#azure-storage-sas-linked-service) bağlı hizmetinin adını belirtin ve bu, geçici hazırlama deposu olarak kullandığınız depolamanın örneğine başvurur. <br/><br/> PolyBase aracılığıyla SQL veri ambarı'na veri yüklemek için depolama ile paylaşılan erişim imzası kullanamazsınız. Diğer tüm senaryolarda kullanabilirsiniz. |Yok |Evet, **enableStaging** TRUE olarak ayarlayın |
| **Yolu** |Hazırlanmış verinin içermesini istediğiniz Blob Depolama yolu belirtin. Hizmet, bir yol belirtmezseniz, geçici verileri depolamak için bir kapsayıcı oluşturur. <br/><br/> Yalnızca depolama ile paylaşılan erişim imzası kullanın veya geçici veri belirli bir konumda olmasını gerektiren bir yol belirtin. |Yok |Hayır |
| **enableCompression** |Hedefe kopyalamadan önce verilerin sıkıştırılmasının gerekli olup olmadığını belirtir. Bu ayar, aktarılan veri hacmini azaltır. |Yanlış |Hayır |

Kopyalama etkinliği önceki tabloda açıklanan özellikler ile bir örnek tanımı aşağıda verilmiştir:

```json
"activities":[
{
    "name": "Sample copy activity",
    "type": "Copy",
    "inputs": [{ "name": "OnpremisesSQLServerInput" }],
    "outputs": [{ "name": "AzureSQLDBOutput" }],
    "typeProperties": {
        "source": {
            "type": "SqlSource",
        },
        "sink": {
            "type": "SqlSink"
        },
        "enableStaging": true,
        "stagingSettings": {
            "linkedServiceName": "MyStagingBlob",
            "path": "stagingcontainer/path",
            "enableCompression": true
        }
    }
}
]
```

### <a name="billing-impact"></a>Faturalama etkisi
Bağlı olarak iki adımı ücretlendirilir: kopyalama süresi ve kopyalayın türü.

* Bir bulut kopyası sırasında hazırlama (bir bulut veri deposundan başka bir bulut veri deposuna veri kopyalama) kullandığınızda, [1. adım için kopyalama süresi toplamı ve 2. adım] x [bulut kopya birim fiyatı] ücretlendirilirsiniz.
* Karma kopya sırasında hazırlama kullandığınızda (verileri şirket içi veri deposundan bir bulut veri deposuna kopyalama), [karma kopyalama süresi] x [karma kopya birim fiyatı] + [bulut kopyalama süresi] x [bulut kopyalama birimi fiyatı] için ücretlendirilirsiniz.

## <a name="performance-tuning-steps"></a>Performans ayarlama adımları
Kopyalama etkinliği, Data Factory hizmetine performansını ayarlamak için aşağıdaki adımları atmanız öneririz:

1. **Bir taban çizgisi oluşturma**. Geliştirme aşamasında, bir temsilci veri örneği karşı kopyalama etkinliği'ni kullanarak işlem hattınızı test etme. Çalıştığınız veri miktarını sınırlandırmak için Data Factory [Dilimleme modelini](data-factory-scheduling-and-execution.md) kullanabilirsiniz.

   **İzleme ve yönetim uygulamasını**kullanarak yürütme süresi ve performans özelliklerini toplayın. Data Factory giriş sayfanızda **izleyici & Yönet** ' i seçin. Ağaç görünümünde **Çıkış veri kümesini**seçin. **Etkinlik pencereleri** listesinde kopyalama etkinliği Çalıştır ' ı seçin. **Etkinlik pencereleri** kopyalama etkinlik süresini ve kopyalandığı verilerin boyutunu listeler. Verimlilik, **etkinlik penceresi Gezgini**' nde listelenir. Uygulama hakkında daha fazla bilgi edinmek için bkz. [izleme ve yönetim uygulamasını kullanarak Azure Data Factory işlem hatlarını izleme ve yönetme](data-factory-monitor-manage-app.md).

   ![Etkinlik çalışma ayrıntıları](./media/data-factory-copy-activity-performance/mmapp-activity-run-details.png)

   Makalenin ilerleyen kısımlarında, senaryolarınızın performansını ve yapılandırmasını, etkinliğimizden etkinliğin [performans başvurusunu](#performance-reference) kopyalamak için karşılaştırabilirsiniz.
2. **Tanılama ve performans iyileştirme**. Siz gözleyin performans beklentilerinizi karşılamıyorsa, performans sorunlarını tanımlamak gerekir. Ardından, kaldırmak veya performans etkisini azaltmak için performansı iyileştirin. Performans Tanılama tam açıklamasını bu makalenin kapsamı dışındadır, ancak bazı genel konular şunlardır:

   * Performans özellikleri:
     * [Paralel kopyalama](#parallel-copy)
     * [Bulut veri taşıma birimleri](#cloud-data-movement-units)
     * [Hazırlanmış kopya](#staged-copy)
     * [Veri Yönetimi ağ geçidi ölçeklenebilirliği](data-factory-data-management-gateway-high-availability-scalability.md)
   * [Veri Yönetimi Ağ Geçidi](#considerations-for-data-management-gateway)
   * [Kaynak](#considerations-for-the-source)
   * [Havuz](#considerations-for-the-sink)
   * [Serileştirme ve seri durumundan çıkarma](#considerations-for-serialization-and-deserialization)
   * [Sıkıştırma](#considerations-for-compression)
   * [Sütun eşleme](#considerations-for-column-mapping)
   * [Diğer konular](#other-considerations)
3. **Tüm veri kümeniz Yapılandırması**. Yürütme sonuçları ve performansından memnun olduğunuzda, tüm veri kümesini kapsayacak şekilde tanım ve işlem hattı etkin dönemini genişletebilirsiniz.

## <a name="considerations-for-data-management-gateway"></a>Veri Yönetimi ağ geçidi ile ilgili konular
**Ağ geçidi kurulumu**: veri yönetimi ağ geçidini barındırmak için adanmış bir makine kullanmanızı öneririz. [Veri yönetimi ağ geçidini kullanma konularına göz](data-factory-data-management-gateway.md#considerations-for-using-gateway)atın.

**Ağ geçidi izleme ve ölçeği artırma/genişletme**: bir veya daha fazla ağ geçidi düğümü içeren tek bir mantıksal ağ geçidi aynı anda birden çok kopyalama etkinliği çalışmasına izin verebilir. Kaynak kullanımının neredeyse gerçek zamanlı anlık görüntüsünü (CPU, bellek, ağ (gelen/giden), vb.) bir ağ geçidi makinesi üzerinde ve Azure portal sınırı ile çalışan eşzamanlı işlerin sayısını görüntüleyebilirsiniz. bkz. [portalda ağ geçidini izleme](data-factory-data-management-gateway.md#monitor-gateway-in-the-portal). Çok sayıda eşzamanlı kopyalama etkinliği çalıştırması veya kopyalamak için büyük miktarda veri içeren karma veri hareketi hakkında ağır gereksiniminiz varsa, kaynağınızı daha iyi bir şekilde kullanmak veya daha fazla kaynak sağlamak için, [ağ geçidini ölçeklendirmeye veya ölçeklendirmeye](data-factory-data-management-gateway-high-availability-scalability.md#scale-considerations) göz önünde bulundurun.

## <a name="considerations-for-the-source"></a>Kaynağı için konular
### <a name="general"></a>Genel
Alttaki veri deposuna veya bunlara karşı çalışan diğer iş yükleri tarafından dolmasını değil emin olun.

Microsoft veri depoları için bkz: [izleme ve ayarlama konuları](#performance-reference) veri depoları ve veri performans özelliklerini depolamak, yanıt süreleri en aza indirmek ve aktarım hızını en üst düzeye anlamanıza yardım özgü olan.

Blob depolamadaki verileri SQL veri ambarı 'na kopyalarsanız, performansı artırmak için **PolyBase** kullanmayı düşünün. Bkz: [Azure SQL veri ambarı'na veri yüklemek için PolyBase kullanma](data-factory-azure-sql-data-warehouse-connector.md#use-polybase-to-load-data-into-azure-sql-data-warehouse) Ayrıntılar için. Kullanım örneği ile bir kılavuz için bkz. [1 TB 15 dakikadan daha kısa Azure Data Factory ile Azure SQL Data Warehouse'a veri yükleme](data-factory-load-sql-data-warehouse.md).

### <a name="file-based-data-stores"></a>Dosya tabanlı veri depoları
*(Blob Storage, Data Lake Store, Amazon S3, şirket içi dosya sistemleri ve şirket içi olarak) dahildir*

* **Dosya boyutu ve dosya sayısı ortalaması**: kopyalama etkinliği, bir kerede bir veri dosyası aktarır. İle aynı taşınacak veri miktarına, hizmetin genel performansını veriler nedeniyle her dosya için önyükleme aşaması birkaç büyük dosyalar yerine çok sayıda küçük dosya oluşuyorsa düşüktür. Bu nedenle, mümkünse, küçük dosyaları daha yüksek performans elde etmek için daha büyük dosyalarına birleştirin.
* **Dosya biçimi ve sıkıştırma**: performansı artırmak daha fazla yolu için bkz [serileştirme ve seri durumundan çıkarma için Değerlendirmeler](#considerations-for-serialization-and-deserialization) ve [sıkıştırma dikkate alınacak noktalar](#considerations-for-compression) bölümler.
* **Veri yönetimi ağ geçidinin** gerekli olduğu **Şirket içi dosya sistemi** senaryosu Için, [veri yönetimi ağ geçidine yönelik hususlar](#considerations-for-data-management-gateway) bölümüne bakın.

### <a name="relational-data-stores"></a>İlişkisel veri deposu
*(SQL veritabanı dahildir; SQL veri ambarı; Amazon Redshift; SQL Server veritabanları; ve Oracle, MySQL, DB2, Teradata, Sybase ve PostgreSQL veritabanları, vb.)*

* **Veri modelini**: Tablo şemanızı kopyalama aktarım hızını etkiler. Büyük satır boyutu, küçük satır boyutu, aynı miktarda veri kopyalamak için daha iyi bir performans sunar. Veritabanı daha az satır içeren veri daha az toplu işler daha verimli bir şekilde alabilirsiniz nedenidir.
* **Sorgu veya saklı yordam**: sorgu veya saklı yordam, belirttiğiniz verileri daha verimli bir şekilde getirmek için kopyalama etkinliği kaynak mantığını iyileştirin.
* **Veri yönetimi ağ geçidinin**kullanımını gerektiren SQL Server ve Oracle gibi **Şirket içi ilişkisel veritabanları**için veri yönetimi ağ geçidine yönelik Konular bölümüne bakın.

## <a name="considerations-for-the-sink"></a>Havuz için dikkat edilmesi gerekenler
### <a name="general"></a>Genel
Alttaki veri deposuna veya bunlara karşı çalışan diğer iş yükleri tarafından dolmasını değil emin olun.

Microsoft veri depoları için başvurmak [izleme ve ayarlama konuları](#performance-reference) özgü veri depoları. Bu konular, veri deposu performans özelliklerine ve yanıt sürelerini en aza indirmek ve aktarım hızını en üst düzeye çıkarmak nasıl anlamanıza yardımcı olabilir.

**BLOB depolamadan** **SQL veri ambarı**'na veri kopyalıyorsanız, performansı artırmak için **PolyBase** kullanmayı düşünün. Bkz: [Azure SQL veri ambarı'na veri yüklemek için PolyBase kullanma](data-factory-azure-sql-data-warehouse-connector.md#use-polybase-to-load-data-into-azure-sql-data-warehouse) Ayrıntılar için. Kullanım örneği ile bir kılavuz için bkz. [1 TB 15 dakikadan daha kısa Azure Data Factory ile Azure SQL Data Warehouse'a veri yükleme](data-factory-load-sql-data-warehouse.md).

### <a name="file-based-data-stores"></a>Dosya tabanlı veri depoları
*(Blob Storage, Data Lake Store, Amazon S3, şirket içi dosya sistemleri ve şirket içi olarak) dahildir*

* **Kopyalama davranışı**: farklı dosya tabanlı veri deposundan verileri kopyalarsanız, kopyalama etkinliği ile üç seçenek vardır. **copyBehavior** özelliği. Hiyerarşi korur, hiyerarşi düzleştirir veya dosyayı birleştirir. Koruma veya hiyerarşi düzleştirme çok az kayıpla veya hiç performansa sahiptir, ancak dosyaları birleştirme artırmak performansa neden olur.
* **Dosya biçimi ve sıkıştırma**: bkz [serileştirme ve seri durumundan çıkarma için Değerlendirmeler](#considerations-for-serialization-and-deserialization) ve [sıkıştırma dikkate alınacak noktalar](#considerations-for-compression) performansını artırmak daha yollarını bölümler.
* **BLOB depolama**: Şu anda BLOB depolama, en iyileştirilmiş veri aktarımı ve aktarım hızı için yalnızca blok bloblarını destekler.
* **Veri yönetimi ağ geçidi**kullanımını gerektiren **Şirket içi dosya sistemleri** senaryolarında, [veri yönetimi ağ geçidine yönelik konular](#considerations-for-data-management-gateway) bölümüne bakın.

### <a name="relational-data-stores"></a>İlişkisel veri deposu
*(SQL veritabanı, SQL veri ambarı, SQL Server veritabanları ve Oracle veritabanları dahildir)*

* **Kopyalama davranışı**: özellikler için ayarladığınız bağlı olarak **sqlSink**, kopyalama etkinliği farklı şekillerde hedef veritabanına veri yazar.
  * Varsayılan olarak, en iyi performans sağlayan modu, veri taşıma hizmeti kullandığı veri eklemek için toplu kopyalama API ekleyin.
  * Havuza bir saklı yordam yapılandırırsanız, veritabanı veri bir satırı toplu yükleme yerine anda geçerlidir. Performansı ciddi ölçüde düştüğü. Veri kümesi büyükse, uygulanabiliyorsa, **Sqlwritercleanupscript** özelliğini kullanmaya geçmeyi düşünün.
  * Her bir kopyalama etkinliği için **Sqlwritercleanupscript** özelliğini yapılandırırsanız, hizmet betiği tetikler ve ardından verileri eklemek Için toplu kopyalama API 'sini kullanırsınız. Örneğin, en son verileriyle tüm tablo üzerine yazmak için önce toplu yeni veri kaynağından yükleme önce tüm kayıtları silmek için bir betik belirtebilirsiniz.
* **Veri düzeni ve toplu işlem boyutu**:
  * Tablo şemanızı kopyalama aktarım hızını etkiler. Veritabanı daha verimli bir şekilde daha az toplu veri kaydedebilir miyim çünkü aynı miktarda veri kopyalamak için büyük satır boyutu, küçük satır boyutu daha iyi performans sağlar.
  * Kopyalama etkinliği, toplu bir dizi içinde veri ekler. Bir toplu işte satır sayısını kullanarak ayarlayabilirsiniz **writeBatchSize** özelliği. Verilerinizi küçük satır varsa, ayarlayabileceğiniz **writeBatchSize** özellik toplu işlem ek yükü azaltır ve daha yüksek performans avantajlarından yararlanarak daha yüksek bir değere sahip. Verilerinizin satır boyutu büyükse, artırdığınızda dikkat **writeBatchSize**. Yüksek bir değere bir kopyalama hatasının veritabanı aşırı yüklenerek neden neden olabilir.
* **Veri yönetimi ağ geçidinin**kullanımını gerektiren SQL Server ve Oracle gibi **Şirket içi ilişkisel veritabanları** için [veri yönetimi ağ geçidine yönelik konular](#considerations-for-data-management-gateway) bölümüne bakın.

### <a name="nosql-stores"></a>NoSQL depoları
*(Tablo depolamayı ve Azure Cosmos DB içerir)*

* İçin **tablo depolama**:
  * **Bölüm**: veri araya eklemeli bölümlere önemli ölçüde yazma performansı bozar. Veri kaynağınızı bölüm anahtarına göre sıralama verileri verimli bir şekilde bir bölüme diğerinden sonra eklenir veya tek bir bölüm için veri yazmak için mantığı ayarlayın.
* **Azure Cosmos DB**için:
  * **Toplu iş boyutu**: **writebatchsize** özelliği, belge oluşturmak için Azure Cosmos DB hizmetine paralel istek sayısını ayarlar. Azure Cosmos DB için daha fazla paralel istek gönderildiğinden **Writebatchsize** ' i artırdığınızda daha iyi performans sağlayabilirsiniz. Ancak, Azure Cosmos DB yazdığınızda azaltma için izleyin (hata iletisi "Istek hızı büyük olur"). Belge boyutu, belgelerdeki terim sayısı ve hedef koleksiyonun dizin oluşturma ilkesi de dahil olmak üzere çeşitli faktörler azalmasına neden olabilir. Daha yüksek kopya işleme sağlamak için daha iyi bir koleksiyon (örneğin, S3) kullanmayı düşünün.

## <a name="considerations-for-serialization-and-deserialization"></a>Serileştirme ve seri durumundan çıkarma için dikkat edilmesi gerekenler
Serileştirme ve seri durumundan çıkarma girdi veri kümesini veya çıkış veri kümesi bir dosya olduğunda ortaya çıkabilir. Bkz: [desteklenen dosya ve sıkıştırma biçimleri](data-factory-supported-file-and-compression-formats.md) kopyalama etkinliği tarafından desteklenen dosya biçimleri hakkında daha fazla ayrıntı içeren.

**Kopyalama davranışı**:

* Dosya tabanlı veri depoları arasında dosyaları kopyalanıyor:
  * Hem giriş hem de çıkış veri kümeleri aynı veya dosya biçimi ayarlarına sahip olduğunda, veri taşıma hizmeti herhangi bir serileştirme veya serisini kaldırma olmadan bir ikili kopya yürütür. Kaynak ve havuz dosya biçimi ayarları birbirinden farklı senaryo ile karşılaştırıldığında daha yüksek bir aktarım hızı görürsünüz.
  * Ne zaman giriş ve çıkış veri kümeleri hem metin biçimi ve yalnızca kodlama olan türü farklı olmadığı, veri taşıma Hizmeti'nde yalnızca kodlama dönüştürme yapar. Herhangi bir serileştirme yapmaz ve seri durumdan yükü ikili kopyasını kıyasla bazı performans neden olur.
  * Farklı dosya biçimleri veya sınırlayıcılar gibi farklı yapılandırmaları, veri taşıma Hizmeti'nde girdi ve çıktı veri kümeleri hem olduğunda akışı, dönüştürme ve belirttiğiniz çıkış biçimine seri hale için kaynak verileri seri durumdan çıkarır. Bu işlem ek yükü diğer senaryolarda kıyasla çok daha önemli bir performans sonuçlanır.
* (Örneğin, bir dosya tabanlı depolama alanından ilişkisel bir veri deposuna) dosya tabanlı olmayan bir veri deposuna/deposundan veri dosyalarını kopyalarken, serileştirme veya seri durumundan çıkarma adım gereklidir. Bu adım, önemli performans ek yükü oluşur.

**Dosya biçimi**: seçtiğiniz dosya biçimi kopyalama performansı etkileyebilir. Örneğin, Avro verileriyle meta verileri depolar sıkıştırılmış bir ikili biçimi ' dir. Hadoop ekosistemindeki işleme ve sorgulama için geniş destek vardır. Ancak, Avro daha pahalı serileştirme ve seri kaldırma metin biçimine kıyasla daha düşük kopyalama aktarım hızı ile sonuçlanır. Dosya biçimi işleme akışı boyunca tercih ettiğiniz bütünlüklü olarak yapın. Hangi veri formu ile Başlat, kaynak veri depolarını veya dış sistemlerden ayıklanacak depolanır; Depolama, analitik işleme ve sorgulama için en iyi biçimi; ve hangi biçimde uygulamasına raporlama ve görselleştirme araçları için veri reyonları veri verilmesi. Bazen için yetersiz bir dosya biçimi okuma ve yazma performansını genel analitik işlemi düşünürken iyi bir seçim olabilir.

## <a name="considerations-for-compression"></a>Sıkıştırma dikkate alınacak noktalar
Giriş veya çıkış veri kümesi bir dosya olduğunda, hedef konuma verileri yazar gibi sıkıştırma veya sıkıştırmayı açma gerçekleştirmek için kopyalama etkinliği ayarlayabilirsiniz. Sıkıştırma seçtiğinizde, giriş/çıkış (g/ç) arasında bir denge duruma ve CPU. Bilgi işlem kaynaklarının ek veri maliyetlerini sıkıştırma. Ancak, ağ g/ç ve depolama azaltır. Verilere bağlı olarak bir artırma genel kopyalama aktarım hızının görebilirsiniz.

**Codec**: kopyalama etkinliği gzip, bzip2 ve söndür sıkıştırma türlerini destekler. Azure HDInsight, işlenmek üzere üç türü de kullanabilir. Her sıkıştırma codec bileşeni avantajlara sahiptir. Örneğin, bzıp2 düşük kopyalama aktarım hızına sahip, ancak işleme için bölme çünkü bzıp2 ile en iyi Hive sorgu performansı elde. Gzip en dengeli bir seçenektir ve en sık kullanılır. Uçtan uca senaryonuza uygun codec bileşeni seçin.

**Düzey**: her bir sıkıştırma codec için iki seçenek arasından seçim yapabilirsiniz: hızlı sıkıştırılmış ve verimli sıkıştırılmış. Sıkıştırılmış en hızlı seçenek bile elde edilen dosyanın en uygun şekilde sıkıştırılmamış veri, mümkün olan en kısa sürede sıkıştırır. En uygun şekilde sıkıştırılmış seçeneği sıkıştırma üzerinde daha fazla zaman harcadığını ve en az miktarda veri ortaya çıkarır. İki seçenek de durumunuzdaki genel daha iyi performans sağlayan görmek için test edebilirsiniz.

**Göz**önünde bulundurun: şirket içi bir mağaza ve bulut arasında büyük miktarda veriyi kopyalamak için, sıkıştırma ile ara BLOB depolama kullanmayı düşünün. Geçici depolama kullanarak şirket ağınıza ve Azure hizmetlerinizi bant sınırlayan faktör ve giriş veri kümesi ve çıktı veri kümesi hem sıkıştırılmamış biçiminde olmasını istediğiniz olduğunda yararlıdır. Daha belirgin bir şekilde, tek bir kopyalama etkinliğini iki kopyalama etkinliğine kesebilirsiniz. İlk kopyalama etkinliği kaynaktan sıkıştırılmış biçimde bir ara veya hazırlama blobuna kopyalanır. İkinci kopyalama etkinliği, sıkıştırılmış verileri hazırlama işleminden kopyalar ve sonra havuza yazarken bu verileri açar.

## <a name="considerations-for-column-mapping"></a>Sütun eşlemesi için dikkat edilmesi gerekenler
Ayarlayabileceğiniz **Bunun amacı** kopyalama etkinliği özelliği tüm harita veya çıkış sütunları için giriş sütun alt kümesi. Veri taşıma Hizmeti'nde veri kaynağından okur. sonra havuz için verileri yazar önce veriler üzerinde sütun eşleme gerçekleştirmek gerekir. Bu ek işlem kopyalama performansını düşürür.

Kaynak veri deponuzda sorgulanabilir, örneğin, SQL veritabanı veya SQL Server gibi ilişkisel bir mağaza veya tablo depolama veya Azure Cosmos DB gibi bir NoSQL deposu olması durumunda sütun filtreleme gönderme ve mantığı yeniden sıralama göz önünde bulundurun **sorgu** sütun eşlemesi kullanmak yerine özellik. Veri taşıma Hizmeti'nde çok daha verimli olduğu kaynak veri deposundan veri okurken bu şekilde yansıtma gerçekleşir.

## <a name="other-considerations"></a>Diğer konular
Kopyalamak istediğiniz verilerin boyutu büyükse, Data Factory Dilimleme mekanizmasını kullanarak verileri daha fazla bölümlemek için iş mantığınızı ayarlayabilirsiniz. Sonra, kopyalama etkinliğini, her kopyalama etkinliği çalışmasının veri boyutunu azaltmak için daha sık çalışacak şekilde zamanlayın.

Aynı anda aynı veri deposuna bağlayıcı Data Factory gerektiren veri kümeleri ve kopyalama etkinliklerinin sayısı hakkında dikkatli olun. Birçok eş zamanlı kopyası işleri bir veri deposu azaltma ve performansın düşmesine neden, kopyalama işi iç deneme ve bazı durumlarda, yürütme hatalarını sağlama.

## <a name="sample-scenario-copy-from-an-on-premises-sql-server-to-blob-storage"></a>Örnek senaryo: bir şirket içi SQL Server'dan Blob Depolama Kopyala
**Senaryo**: bir işlem hattı, bir şirket içi SQL Server'dan Blob Depolama alanında CSV biçimi veri kopyalamak için oluşturulmuştur. Kopyalama işini daha hızlı hale getirmek için CSV dosyaları bzıp2 biçimine sıkıştırılmasının gerekli.

**Test ve analiz**: performans Kıyaslama yavaş olduğu değerinden 2 MB/sn aktarım hızı kopyalama etkinliği olan.

**Performans Analizi ve ayarlama**: performans sorunu gidermek için nasıl veri işlenen taşınır ve konumunda göz atalım.

1. **Verileri oku**: ağ geçidi, SQL Server bir bağlantı açar ve sorguyu gönderir. SQL Server, veri akışını intranet üzerinden ağ geçidine göndererek yanıt verir.
2. **Verileri serileştirme ve sıkıştırma**: ağ geçidi, VERI akışını CSV biçiminde serileştirir ve verileri bir bzip2 akışına sıkıştırır.
3. **Veri yazma**: ağ geçidi, bzip2 akışını Internet üzerinden blob depolamaya yükler.

Gördüğünüz gibi, veriler işlenir ve akış sıralı bir şekilde taşınır: SQL Server > LAN > Ağ Geçidi > WAN > BLOB depolama. **Genel performansını en düşük aktarım hızı ile işlem hattı Geçitli**.

![Veri akışı](./media/data-factory-copy-activity-performance/case-study-pic-1.png)

Bir veya daha fazla aşağıdaki faktörleri performans sorunu neden olabilir:

* **Kaynak**: SQL Server aktarım hızının düşük olmasını nedeniyle ağır yükleri vardır.
* **Veri yönetimi ağ geçidi**:
  * **LAN**: Gateway, SQL Server makineden uzakta bulunur ve düşük bant genişliğine sahip bir bağlantıya sahiptir.
  * **Ağ geçidi**: ağ geçidi, aşağıdaki işlemleri gerçekleştirmek için yük kısıtlamalarına ulaştı:
    * **Serileştirme**: veri akışı CSV biçimi için seri hale getirme sahip yavaş aktarım hızı.
    * **Sıkıştırma**: yavaş sıkıştırma codec (Core i7 ile 2.8 MB/sn, örneğin, bzıp2,) seçtiniz.
  * **WAN**: Kurumsal ağ ve Azure hizmetlerinizi arasında bant genişliği düşük olduğu (örneğin, T1 1,544 kbps; = T2 6,312 kbps =).
* **Havuz**: Blob depolama alanına sahip düşük aktarım hızı. (En az 60 MB/sn kendi SLA'sı garanti eder, çünkü bu senaryo bir olasılıktır.)

Bu durumda, bzıp2 veri sıkıştırma tüm işlem hattını yavaşlatmasını. Gzip sıkıştırma codec bileşenine geçiş, bu performans sorunu kolaylaştırmak.

## <a name="sample-scenarios-use-parallel-copy"></a>Örnek senaryolar: paralel kopya kullan
**Senaryo ı:** Şirket içi dosya sisteminden blob depolamaya 1.000 1-MB dosyalarını kopyalayın.

**Analiz ve performans ayarlaması**: Örneğin, bir dört çekirdekli makineye ağ geçidi yüklediyseniz, Data Factory dosyaları dosya sisteminden blob depolamaya eşzamanlı olarak taşımak için 16 paralel kopya kullanır. Bu paralel yürütme, yüksek verimlilik elde etmelidir. Ayrıca paralel kopya sayısını açıkça belirtebilirsiniz. Birçok küçük dosyayı kopyaladığınızda, paralel kopyalar kaynakları daha verimli bir şekilde kullanarak aktarım hızına önemli ölçüde yardımcı olur.

![Senaryo 1](./media/data-factory-copy-activity-performance/scenario-1.png)

**Senaryo II**: her biri Blob depolamadan Data Lake Store Analytics 'e 20 blob 500 MB kopyalayın ve ardından performansı ayarlayın.

**Analiz ve performans ayarlaması**: bu senaryoda Data Factory, tek kopya (**parallelcopy** 1 olarak ayarlanmış) ve tek bulut veri taşıma birimleri kullanarak verileri blob depolamadan Data Lake Store kopyalar. Gözettiğiniz aktarım hızı, [performans Başvurusu bölümünde](#performance-reference)açıklanacak şekilde kapatılacak.

![Senaryo 2](./media/data-factory-copy-activity-performance/scenario-2.png)

**Senaryo III**: tek dosya boyutu düzinelerce MB 'den büyük ve toplam birim büyük.

**Analiz ve performans açılıyor**: **parallelcopy** 'leri artırma, tek bir bulut DMU 'nin kaynak sınırlamaları nedeniyle daha iyi kopyalama performansına neden olmaz. Bunun yerine, veri hareketini gerçekleştirmeye yönelik daha fazla kaynak almak için daha fazla bulut DMU belirtmelisiniz. **Parallelkopyaların** özelliği için bir değer belirtmeyin. Data Factory, sizin için paralellik uygular. Bu durumda, **Clouddatamovementunits** 'i 4 olarak ayarlarsanız, yaklaşık dört kez üretilen iş meydana gelir.

![Senaryo 3](./media/data-factory-copy-activity-performance/scenario-3.png)

## <a name="reference"></a>Başvuru
Desteklenen bazı veri depoları için performans izleme ve ayarlama başvuruları aşağıda verilmiştir:

* Azure Blob depolama: BLOB depolama için [ölçeklenebilirlik ve performans hedefleri](../../storage/blobs/scalability-targets.md) , [BLOB depolaması için performans ve ölçeklenebilirlik denetim listesi](../../storage/blobs/storage-performance-checklist.md).
* Azure Tablo Depolama: tablo depolaması için tablo depolama ve performans [ve ölçeklenebilirlik denetim listesi](../../storage/tables/storage-performance-checklist.md) [için ölçeklenebilirlik ve performans hedefleri](../../storage/tables/scalability-targets.md) .
* Azure SQL veritabanı: Yapabilecekleriniz [performansını izleme](../../sql-database/sql-database-single-database-monitor.md) ve veritabanı işlem birimi (DTU) yüzde denetleyin
* Azure SQL veri ambarı: Kendi yeteneği veri ambarı birimi (Dwu) ölçülür bkz: [Yönet işlem gücünü Azure SQL veri ambarı (Genel)](../../sql-data-warehouse/sql-data-warehouse-manage-compute-overview.md)
* Azure Cosmos DB: [Azure Cosmos DB'de performans düzeyleri](../../cosmos-db/performance-levels.md)
* Şirket içi SQL Server: [izleme ve performansı ayarlama](https://msdn.microsoft.com/library/ms189081.aspx)
* Şirket içi dosya sunucusu: [dosya sunucuları için performans ayarlama](https://msdn.microsoft.com/library/dn567661.aspx)
