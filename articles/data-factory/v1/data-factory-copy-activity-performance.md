---
title: Kopyalama Etkinliği performansı ve ayarlama kılavuzu
description: Kopyalama Etkinliği'ni kullandığınızda Azure Veri Fabrikası'ndaki veri hareketinin performansını etkileyen önemli etkenler hakkında bilgi edinin.
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
ms.openlocfilehash: c4ca328aa0ddc61d86a435b93fe775f294287b98
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79527393"
---
# <a name="copy-activity-performance-and-tuning-guide"></a>Kopyalama Etkinliği performansı ve ayarlama kılavuzu

> [!div class="op_single_selector" title1="Kullandığınız Veri Fabrikası hizmetisürümünü seçin:"]
> * [Sürüm 1](data-factory-copy-activity-performance.md)
> * [Sürüm 2 (geçerli sürüm)](../copy-activity-performance.md)

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Veri Fabrikası hizmetinin geçerli sürümünü kullanıyorsanız, [Veri Fabrikası için Kopyalama etkinlik performansı ve atonlama kılavuzuna](../copy-activity-performance.md)bakın.

Azure Veri Fabrikası Kopyalama Etkinliği birinci sınıf güvenli, güvenilir ve yüksek performanslı veri yükleme çözümü sunar. Zengin çeşitli bulut ve şirket içi veri depolarında her gün onlarca terabaytlık veri kopyalamanıza olanak tanır. En hızlı veri yükleme performansı, temel "büyük veri" sorununa odaklanabildiğinizden emin olmak için önemlidir: gelişmiş analitik çözümleri oluşturmak ve tüm bu verilerden derin öngörüler elde etmek.

Azure kurumsal sınıf veri depolama ve veri ambarı çözümleri sunar ve Copy Activity, yapılandırması ve kurulumu kolay son derece optimize edilmiş bir veri yükleme deneyimi sunar. Yalnızca tek bir kopyalama etkinliğiyle şunları elde edebilirsiniz:

* **Verileri 1,2 GBps'de** **Azure SQL Veri Ambarı'na** yükleme. Kullanım örneğine sahip bir geçiş için, [Azure Veri Fabrikası ile 15 dakikanın altında Azure SQL Veri Ambarına 1 TB Yükle'ye](data-factory-load-sql-data-warehouse.md)bakın.
* **Verileri 1,0 GBps'de** **Azure Blob depolama alanına** yükleme
* Verileri **1,0 GBps'de** **Azure Veri Gölü Deposu'na** yükleme

Bu makalede açıklanır:

* Projenizi planlamanıza yardımcı olmak için desteklenen kaynak ve lavabo veri depoları için [performans referans numaraları;](#performance-reference)
* [Bulut veri hareketi birimleri,](#cloud-data-movement-units) [paralel kopya](#parallel-copy)ve [aşamalı Kopya](#staged-copy)dahil olmak üzere farklı senaryolarda kopya veri sini artırabilen özellikler;
* Performansın nasıl ayarlanabildiği ve kopya performansını etkilenebilen önemli etkenler hakkında [performans ayarlama kılavuzu.](#performance-tuning-steps)

> [!NOTE]
> Genel olarak Kopyalama Etkinliği'ni bilmiyorsanız, bu makaleyi okumadan önce [Kopyalama Etkinliği'ni kullanarak verileri taşı'ya](data-factory-data-movement-activities.md) bakın.
>

## <a name="performance-reference"></a>Performans başvurusu

Referans olarak, aşağıdaki tabloda verilen kaynak ve lavabo çiftleri için MBps kopya iş verme numarasını gösterir şirket içi testdayalı. Karşılaştırma için, bulut veri hareketi [birimlerinin](#cloud-data-movement-units) veya [Veri Yönetimi Ağ Geçidi ölçeklenebilirliğinin](data-factory-data-management-gateway-high-availability-scalability.md) (birden çok ağ geçidi düğümü) farklı ayarlarının kopyalama performansına nasıl yardımcı olabileceğini de gösterir.

![Performans matrisi](./media/data-factory-copy-activity-performance/CopyPerfRef.png)

>[!IMPORTANT]
>Azure Veri Fabrikası sürüm 1'de, bulut-bulut kopyalama için en az bulut veri hareketi birimleri ikidir. Belirtilmemişse, bulut veri hareketi [birimlerinde](#cloud-data-movement-units)kullanılan varsayılan veri hareketi birimlerine bakın.

**Dikkat edilmesi gereken noktalar:**
* İş ortası aşağıdaki formül kullanılarak hesaplanır: [Kaynaktan okunan verilerin boyutu]/[Etkinlik çalıştırma süresini kopyala].
* Tablodaki performans başvuru numaraları, tek bir kopyalama etkinliği çalışmasında [TPC-H](http://www.tpc.org/tpch/) veri kümesi kullanılarak ölçüldü.
* Azure veri depolarında kaynak ve lavabo aynı Azure bölgesindedir.
* Şirket içi ve bulut veri depoları arasındaki karma kopya için, her ağ geçidi düğümü, aşağıda belirtimi içeren şirket içi veri deposundan ayrı bir makinede çalışıyordu. Tek bir etkinlik ağ geçidinde çalışırken, kopyalama işlemi test makinesinin CPU'su, belleği veya ağ bant genişliğinin yalnızca küçük bir bölümünü tüketti. [Veri Yönetimi Ağ Geçidi'ni göz önünde bulundurarak](#considerations-for-data-management-gateway)daha fazla bilgi edinin.
    <table>
    <tr>
        <td>CPU</td>
        <td>32 çekirdek 2.20 GHz Intel Xeon E5-2660 v2</td>
    </tr>
    <tr>
        <td>Bellek</td>
        <td>128 GB</td>
    </tr>
    <tr>
        <td>Ağ</td>
        <td>İnternet arayüzü: 10 Gbps; intranet arayüzü: 40 Gbps</td>
    </tr>
    </table>


> [!TIP]
> Buluttan buluta kopyalama etkinliği için 32 olan varsayılan maksimum DMU'lardan daha fazla veri hareketi birimi (DMU) yararlanarak daha yüksek iş kaynağı elde edebilirsiniz. Örneğin, 100 DMUs ile Azure Blob'dan Azure Veri Gölü Deposu'na **1,0GBps'de**veri kopyalama elde edebilirsiniz. Bu özellik ve desteklenen senaryo hakkında ayrıntılar için [Bulut veri hareketi birimleri](#cloud-data-movement-units) bölümüne bakın. Daha fazla DMU'su istemek için [Azure desteğine](https://azure.microsoft.com/support/) başvurun.

## <a name="parallel-copy"></a>Paralel kopya
Verileri kaynaktan okuyabilir veya Kopyalama Etkinliği **çalıştırımı içinde hedefe paralel olarak**veri yazabilirsiniz. Bu özellik, bir kopyalama işleminin veri alım savesini artırır ve verileri taşımak için gereken süreyi azaltır.

Bu ayar, etkinlik tanımındaki **eşzamanlılık** özelliğinden farklıdır. **Eşzamanlılık** özelliği, farklı etkinlik pencerelerinden (1 AM-2, 2 AM-3, 3 AM- 4 vb.) verileri işlemek için **eşzamanlı Kopyalama Etkinliği çalışır** sayısını belirler. Bu özellik, geçmiş bir yük gerçekleştirdiğiniz zaman yararlıdır. Paralel kopyalama özelliği tek bir **etkinlik çalışması**için geçerlidir.

Örnek bir senaryoya bakalım. Aşağıdaki örnekte, geçmişten gelen birden çok dilimin işlenmesi gerekir. Veri Fabrikası, her dilim için Kopyalama Etkinliği (etkinlik çalışması) örneğini çalıştırın:

* İlk etkinlik penceresinden veri dilimi (1 - 2 AM) ==> Etkinlik çalıştırın 1
* İkinci etkinlik penceresinden veri dilimi (2 - 3 AM) ==> Etkinlik çalıştırın 2
* İkinci etkinlik penceresinden veri dilimi (3 - 4 AM) ==> Etkinlik 3 çalıştır

Etki alanları bu hiyerarşi sıralamasıyla devam eder.

Bu örnekte, **eşzamanlılık** değeri 2 olarak ayarlandığında, **Etkinlik çalıştırın 1** ve Etkinlik, veri hareketi performansını artırmak için iki etkinlik penceresinden **aynı anda** 2 kopya veri **çalıştırın.** Ancak, birden çok dosya Etkinlik çalıştır 1 ile ilişkiliyse, veri hareketi hizmeti dosyaları kaynaktan hedef e tek er dosyaya kopyalar.

### <a name="cloud-data-movement-units"></a>Bulut veri hareketi birimleri
**Bulut veri hareketi birimi (DMU),** Veri Fabrikası'ndaki tek bir birimin gücünü (CPU, bellek ve ağ kaynağı ayırmanın birleşimi) temsil eden bir ölçüdür. DMU buluttan buluta kopyalama işlemleri için geçerlidir, ancak karma kopyada geçerli değildir.

**Kopyalama Etkinliği çalışmasını güçlendirmek için en az bulut veri hareketi birimleri ikidir.** Belirtilmemişse, aşağıdaki tablo da farklı kopyalama senaryolarında kullanılan varsayılan DMU'ları listeler:

| Senaryoyu kopyalama | Hizmet tarafından belirlenen Varsayılan DMUs'lar |
|:--- |:--- |
| Dosya tabanlı mağazalar arasında veri kopyalama | Dosyaların sayısına ve boyutuna bağlı olarak 4 ile 16 arasında. |
| Diğer tüm kopyalama senaryoları | 4 |

Bu varsayılanı geçersiz kılmak için **cloudDataMovementUnits** özelliği için aşağıdaki gibi bir değer belirtin. **cloudDataMovementUnits** özelliği için **izin verilen değerler** 2, 4, 8, 16, 32'dir. Kopyalama işleminin çalışma zamanında kullandığı **gerçek bulut DMU sayısı,** veri deseninize bağlı olarak yapılandırılan değere eşit veya daha azdır. Belirli bir kopya kaynağı ve lavabo için daha fazla birim yapılandırdığınızda elde edebileceğiniz performans kazancı düzeyi hakkında bilgi için [performans başvurusuna](#performance-reference)bakın.

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
> Daha yüksek bir iş için daha fazla bulut DMU'ya ihtiyacınız varsa, [Azure desteğine](https://azure.microsoft.com/support/)başvurun. 8 ve üzeri ayarı şu anda yalnızca **Blob depolama/Veri Gölü Deposu/Amazon S3/cloud FTP/cloud SFTP'den Blob depolama/Data Lake Store/Azure SQL Veritabanı'na birden fazla dosya kopyaladiğinizde**çalışır.
>

### <a name="parallelcopies"></a>parallelKopya
Copy Activity'in kullanmasını istediğiniz paralelliği belirtmek için **paralel Kopya özelliğini** kullanabilirsiniz. Bu özelliği, Kaynağınızdan okuyabilen veya paralel olarak lavabo veri depolarınıza yazabilen Kopyalama Etkinliği içindeki maksimum iş parçacığı sayısı olarak düşünebilirsiniz.

Her Kopyalama Etkinliği çalışması için, Veri Fabrikası kaynak veri deposundan ve hedef veri deposundan verileri kopyalamak için kullanılacak paralel kopya sayısını belirler. Kullandığı paralel kopyaların varsayılan sayısı, kullanmakta olduğunuz kaynak ve lavabonun türüne bağlıdır.

| Kaynak ve lavabo | Hizmet tarafından belirlenen varsayılan paralel kopya sayısı |
| --- | --- |
| Dosya tabanlı mağazalar arasında veri kopyalama (Blob depolama; Veri Gölü Deposu; Amazon S3; şirket içi dosya sistemi; şirket içi HDFS) |1 ile 32 arası. Dosyaların boyutuna ve iki bulut veri deposu arasındaki verileri kopyalamak için kullanılan bulut veri hareketi birimlerinin (DMU) sayısına veya karma kopya için kullanılan Ağ Geçidi makinesinin fiziksel yapılandırmasına (şirket içi veri deposuna veya şirket içi veri deposundan veri kopyalamak için) bağlıdır. |
| **Verileri herhangi bir kaynak veri deposundan Azure Tablo depolamasına** kopyalama |4 |
| Diğer tüm kaynak ve lavabo çiftleri |1 |

Genellikle, varsayılan davranış size en iyi iş verme nizi vermelidir. Ancak, veri depolarınızı barındıran makinelerdeki yükü denetlemek veya kopyalama performansını ayarlamak için varsayılan değeri geçersiz kılmayı ve **parallelCopy** özelliği için bir değer belirtmeyi seçebilirsiniz. Değer 1 ile 32 arasında olmalıdır (her ikisi de dahil). Çalışma zamanında, en iyi performans için Kopyalama Etkinliği, belirlediğiniz değerden daha az veya eşit bir değer kullanır.

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
Dikkat edilmesi gereken noktalar:

* Dosya tabanlı depolar arasında veri kopyaladiğinizde, **paralel Kopyalar** dosya düzeyinde paralelliği belirler. Tek bir dosya içinde chunking otomatik ve saydam altında olur ve paralel kopyalar için paralel ve ortogonal veri yüklemek için belirli bir kaynak veri deposu türü için en uygun yığın boyutunu kullanmak için tasarlanmıştır. Veri hareketi hizmetinin çalışma sırasında kopyalama işlemi için kullandığı gerçek paralel kopya sayısı, sahip olduğunuz dosya sayısından fazla değildir. Kopyalama davranışı **birleştirme Dosya**ise, Kopyalama Etkinliği dosya düzeyinde paralellik yararlanamaz.
* **ParallelCopys** özelliği için bir değer belirttiğinizde, kaynak ve lavabo veri depolarınızdaki yük artışını ve karma kopyaysa ağ geçidine göz önünde bulundurun. Bu, özellikle aynı veri deposuna karşı çalışan aynı etkinliklerin birden çok aktivitesi veya eşzamanlı çalıştırmaları varsa olur. Veri deposunun veya Ağ Geçidi'nin yükten bunaldığını fark ederseniz, yükü hafifletmek için **paralel Kopya değerini** azaltın.
* Dosya tabanlı olmayan mağazalardaki verileri dosya tabanlı mağazalara kopyaladiğinizde, veri hareketi hizmeti **paralel Kopyalar** özelliğini yoksa. Paralellik belirtilmiş olsa bile, bu durumda uygulanmaz.

> [!NOTE]
> Karma kopya yaparken **paralel Kopyaözelliğini** kullanmak için Veri Yönetimi Ağ Geçidi sürüm 1.11 veya sonraki sürümlerini kullanmanız gerekir.
>
>

Bu iki özelliği daha iyi kullanmak ve veri hareketi veri biricünüzü artırmak için örnek kullanım örneklerine bakın. Varsayılan davranıştan yararlanmak için **paralel Kopyaları** yapılandırmanız gerekmez. Yapılandırma yaparsanız ve **parallelCopyçok** küçükse, birden çok bulut DMU'su tam olarak kullanılmayabilir.

### <a name="billing-impact"></a>Faturalama etkisi
Kopyalama işleminin toplam süresine bağlı olarak ücretlendirildiğiniz unutulmaması önemlidir. **important** Bir kopyalama işi bir bulut birimiyle bir saat sürüyorsa ve şimdi dört bulut birimiyle 15 dakika sürüyorsa, toplam fatura hemen hemen aynı kalır. Örneğin, dört bulut birimi kullanırsınız. İlk bulut birimi 10 dakika, ikincisi 10 dakika, üçüncü, 5 dakika ve dördüncü, 5 dakika, hepsi bir Kopya Etkinliği çalışmasında harcar. 10 + 10 + 5 + 5 = 30 dakika olan toplam kopyalama (veri hareketi) süresi için ücretlendirilirsiniz. **ParallelCopy** kullanmak faturalandırmayı etkilemez.

## <a name="staged-copy"></a>Aşamalı kopya
Verileri bir kaynak veri deposundan lavabo veri deposuna kopyaladiğinizde, Blob depolama alanını geçici bir hazırlama deposu olarak kullanmayı seçebilirsiniz. Evreleme özellikle aşağıdaki durumlarda yararlıdır:

1. **Çeşitli veri depolarından gelen verileri PolyBase üzerinden SQL Veri Ambarı'na almak istiyorsunuz.** SQL Veri Ambarı, büyük miktarda veriyi SQL Veri Ambarı'na yüklemek için yüksek iş kaynağı mekanizması olarak PolyBase kullanır. Ancak, kaynak verilerin Blob depolama olmalıdır ve ek ölçütleri karşılamalıdır. Blob depolama dışında bir veri deposundan veri yüklediğinizde, geçici evreleme Blob depolama yoluyla veri kopyalamayı etkinleştirebilirsiniz. Bu durumda, Veri Fabrikası, PolyBase gereksinimlerini karşıladığından emin olmak için gerekli veri dönüşümlerini gerçekleştirir. Daha sonra SQL Veri Ambarı'na veri yüklemek için PolyBase kullanır. Daha fazla bilgi [için, verileri Azure SQL Veri Ambarı'na yüklemek için PolyBase'i kullanın'a](data-factory-azure-sql-data-warehouse-connector.md#use-polybase-to-load-data-into-azure-sql-data-warehouse)bakın. Kullanım örneğine sahip bir geçiş için, [Azure Veri Fabrikası ile 15 dakikanın altında Azure SQL Veri Ambarına 1 TB Yükle'ye](data-factory-load-sql-data-warehouse.md)bakın.
2. **Bazen yavaş bir ağ bağlantısı üzerinden karma veri hareketi (diğer bir gün şirket içi veri deposu ve bulut veri deposu arasında kopyalama) gerçekleştirmek için biraz zaman alır.** Performansı artırmak için, verileri buluttaki hazırlama veri deposuna taşımanın daha az zaman alması için verileri şirket içinde sıkıştırabilirsiniz. Ardından, hedef veri deposuna yüklemeden önce hazırlama deposundaki verileri sıkıştırabilirsiniz.
3. **Kurumsal BT ilkeleri nedeniyle güvenlik duvarınızda 80 ve bağlantı noktası 443 dışındaki bağlantı noktalarını açmak istemiyorsunuz.** Örneğin, şirket içi veri deposundan Azure SQL Veritabanı lavabosuna veya Azure SQL Veri Ambarı lavabosuna verileri kopyalarken, hem Windows güvenlik duvarınız hem de şirket güvenlik duvarınız için 1433 bağlantı noktasında giden TCP iletişimini etkinleştirmeniz gerekir. Bu senaryoda, verileri ilk olarak 443 no'daki porttaki HTTP veya HTTPS üzerinden Blob depolama evreleme örneğine kopyalamak için ağ geçidinden yararlanın. Ardından, verileri Blob depolama evrelemesinden SQL Veritabanı'na veya SQL Veri Ambarı'na yükleyin. Bu akışta, bağlantı noktası 1433'e etkinleştirmeniz gerekmez.

### <a name="how-staged-copy-works"></a>Aşamalı kopya nasıl çalışır?
Hazırlama özelliğini etkinleştirdiğinizde, önce veriler kaynak veri deposundan evreleme veri deposuna kopyalanır (kendi veri deponuzu getirin). Ardından, veriler hazırlama veri deposundan lavabo veri deposuna kopyalanır. Veri Fabrikası sizin için iki aşamalı akışı otomatik olarak yönetir. Veri Fabrikası, veri hareketi tamamlandıktan sonra hazırlama depolamasından geçici verileri de temizler.

Bulut kopyalama senaryosunda (hem kaynak hem de lavabo veri depoları buluttadır), ağ geçidi kullanılmaz. Veri Fabrikası hizmeti kopyalama işlemlerini gerçekleştirir.

![Aşamalı kopya: Bulut senaryosu](media/data-factory-copy-activity-performance/staged-copy-cloud-scenario.png)

Karma kopyalama senaryosunda (kaynak şirket içinde ve lavabo buluttadır), ağ geçidi verileri kaynak veri deposundan bir hazırlama veri deposuna taşır. Veri Fabrikası hizmeti, verileri hazırlama veri deposundan lavabo veri deposuna taşır. Bir bulut veri deposundan, evreleme yoluyla şirket içi veri deposuna veri kopyalanması da ters akışla desteklenir.

![Aşamalı kopya: Karma senaryo](media/data-factory-copy-activity-performance/staged-copy-hybrid-scenario.png)

Bir evreleme deposu kullanarak veri hareketini etkinleştirdiğinizde, verileri kaynak veri deposundan geçici veya evreleme veri deposuna taşımadan önce verilerin sıkıştırılıp sıkıştırılmamasını isteyip istemediğinizve verileri geçici veya evreleme verilerinden taşımadan önce desıkıştırılmanızı isteyip istemediğinizbelirtebilirsiniz lavabo veri deposuna saklayın.

Şu anda, bir hazırlama deposu kullanarak iki şirket içi veri deposu arasındaki verileri kopyalayamazsınız. Bu seçeneğin yakında kullanıma sunulmasını bekliyoruz.

### <a name="configuration"></a>Yapılandırma
Verileri bir hedef veri deposuna yüklemeden önce Blob depolamaalanında düzenesin isteyip istemediğinizi belirtmek için Kopyalama Etkinliği'ndeki **etkinleştirme Oluşturma** ayarını yapılandırın. **enableStaging'i** TRUE olarak ayarladığınızda, sonraki tabloda listelenen ek özellikleri belirtin. Yoksa, evreleme için Azure Depolama veya Depolama paylaşılan erişim imzasına bağlı bir hizmet de oluşturmanız gerekir.

| Özellik | Açıklama | Varsayılan değer | Gerekli |
| --- | --- | --- | --- |
| **enableStaging** |Geçici bir hazırlama deposu aracılığıyla verileri kopyalamak isteyip istemediğinizi belirtin. |False |Hayır |
| **linkedServiceName** |Geçici bir hazırlama deposu olarak kullandığınız Depolama örneğine atıfta bulunan [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service) veya [AzureStorageSas](data-factory-azure-blob-connector.md#azure-storage-sas-linked-service) bağlantılı bir hizmetin adını belirtin. <br/><br/> PolyBase üzerinden SQL Veri Ambarı'na veri yüklemek için paylaşılan erişim imzasıyla Depolama'yı kullanamazsınız. Diğer tüm senaryolarda kullanabilirsiniz. |Yok |Evet, **enableStaging** TRUE olarak ayarlandığında |
| **Yolu** |Aşamalı verileri içermek istediğiniz Blob depolama yolunu belirtin. Bir yol sağlamazsanız, hizmet geçici verileri depolamak için bir kapsayıcı oluşturur. <br/><br/> Bir yol belirtin, yalnızca paylaşılan bir erişim imzasıyla Depolama'yı kullanıyorsanız veya geçici verilerin belirli bir konumda olmasını istiyorsanız. |Yok |Hayır |
| **etkinleştirsıkıştırma** |Verilerin hedefe kopyalanıp kopyalanmayacağını belirtir. Bu ayar aktarılan veri hacmini azaltır. |False |Hayır |

Aşağıda, önceki tabloda açıklanan özelliklere sahip Kopyalama Etkinliği örnek tanımı verilmiştir:

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
İki adıma göre ücretlendirilirsiniz: kopyalama süresi ve kopya türü.

* Bir bulut kopyası sırasında evreleme kullandığınızda (bulut veri deposundan başka bir bulut veri deposuna veri kopyalama), [adım 1 ve adım 2] x [bulut kopyalama birimi fiyatı] için kopya süresi nin toplamı olarak ücretlendirilirsiniz.
* Karma kopya sırasında evreleme kullandığınızda (şirket içi veri deposundan bulut veri deposuna veri kopyalama), [karma kopyalama süresi] x [karma kopya birim fiyatı] + [bulut kopyalama süresi] x [bulut kopyalama birim fiyatı] için ücretlendirilirsiniz.

## <a name="performance-tuning-steps"></a>Performans alamı adımları
Veri Fabrikası hizmetinizin performansını Kopyalama Etkinliği ile ayarlamak için aşağıdaki adımları atmanızı öneririz:

1. **Bir taban çizgisi kurun.** Geliştirme aşamasında, Kopya Etkinliği'ni temsili bir veri örneğine karşı kullanarak ardışık hattınızı test edin. Çalıştığınız veri miktarını sınırlamak için Veri Fabrikası [dilimleme modelini](data-factory-scheduling-and-execution.md) kullanabilirsiniz.

   İzleme ve **Yönetim Uygulamasını**kullanarak yürütme süresi ve performans özelliklerini toplayın. Veri Fabrikası ana sayfanızda **İzle & Yönet'i** seçin. Ağaç görünümünde çıktı **veri kümesini**seçin. Etkinlik **Windows** listesinde, Etkinlik Kopyasını seçin. **Etkinlik Windows,** Kopyalama Etkinliği süresini ve kopyalanan verilerin boyutunu listeler. Elde edilen iş, **Etkinlik Penceresi Gezgini'nde**listelenir. Uygulama hakkında daha fazla bilgi edinmek için İzleme [ve Yönetim Uygulamasını kullanarak Azure Veri Fabrikası ardışık hatlarını izleyin ve yönetin.](data-factory-monitor-manage-app.md)

   ![Etkinlik çalışma ayrıntıları](./media/data-factory-copy-activity-performance/mmapp-activity-run-details.png)

   Makalenin ilerleyen saatlerinde, senaryonuzun performansını ve yapılandırmasını testlerimizden Etkinlik'in [performans referansını](#performance-reference) kopyala ile karşılaştırabilirsiniz.
2. **Performansı tanıve optimize edin.** Gözlemlediğiniz performans beklentilerinizi karşılamıyorsa, performans darboğazlarını belirlemeniz gerekir. Ardından, performans sorunlarını ortadan kaldırmak veya azaltmak için performansı optimize edin. Performans tanısıtam bir açıklama bu makalenin kapsamı dışındadır, ancak burada bazı sık sık hususlar şunlardır:

   * Performans özellikleri:
     * [Paralel kopya](#parallel-copy)
     * [Bulut veri hareketi birimleri](#cloud-data-movement-units)
     * [Aşamalı kopya](#staged-copy)
     * [Veri Yönetimi Ağ Geçidi ölçeklenebilirliği](data-factory-data-management-gateway-high-availability-scalability.md)
   * [Veri Yönetimi Ağ Geçidi](#considerations-for-data-management-gateway)
   * [Kaynak](#considerations-for-the-source)
   * [Havuz](#considerations-for-the-sink)
   * [Serileştirme ve deserialization](#considerations-for-serialization-and-deserialization)
   * [Sıkıştırma](#considerations-for-compression)
   * [Sütun eşleme](#considerations-for-column-mapping)
   * [Diğer konular](#other-considerations)
3. **Yapılandırmayı tüm veri kümenize genişletin.** Yürütme sonuçları ndan ve performansından memnun olduğunuzda, tüm veri kümenizi kapsayacak şekilde tanım ve ardışık etkin dönemi genişletebilirsiniz.

## <a name="considerations-for-data-management-gateway"></a>Veri Yönetimi Ağ Geçidi Için Dikkat Edilecek Noktalar
**Ağ Geçidi kurulumu**: Veri Yönetimi Ağ Geçidi'ni barındırmak için özel bir makine kullanmanızı öneririz. Bkz. [Veri Yönetimi Ağ Geçidi'ni kullanmak için dikkat edilecek noktalar.](data-factory-data-management-gateway.md#considerations-for-using-gateway)

**Ağ geçidi izleme ve ölçeklendirme/çıkış**: Bir veya daha fazla ağ geçidi düğümüiçeren tek bir mantıksal ağ geçidi, aynı anda birden çok Kopyalama Etkinliği çalıştırabilir. Bir ağ geçidi makinesinde kaynak kullanımının (CPU, bellek, ağ(giriş/çıkış), vb. yanı sıra Azure portalında sınıra karşı çalışan eşzamanlı işlerin sayısını görüntüleyebilirsiniz, [bkz.](data-factory-data-management-gateway.md#monitor-gateway-in-the-portal) Çok sayıda eşzamanlı kopyalama etkinliği çalıştırAn veya kopyalanması gereken büyük miktarda veriyle karma veri hareketine büyük bir ihtiyacınız varsa, kaynağınızı daha iyi kullanmak veya kopyayı güçlendirmek için daha fazla kaynak sağlamak için [ağ geçidini ölçeklendirmeyi veya ölçeklendirmeyi](data-factory-data-management-gateway-high-availability-scalability.md#scale-considerations) düşünün.

## <a name="considerations-for-the-source"></a>Kaynak için dikkat edilecek hususlar
### <a name="general"></a>Genel
Temel veri deposunun üzerinde veya aleyhine çalışan diğer iş yükleri tarafından bunalmış olmadığından emin olun.

Microsoft veri depoları için, veri depolarına özgü [izleme ve atoalma konularına](#performance-reference) bakın ve veri depolama performansı özelliklerini anlamanıza, yanıt sürelerini en aza indirmenize ve iş hacmini en üst düzeye çıkarmanıza yardımcı olun.

Blob depolamadan SQL Veri Ambarı'na veri kopyalarsanız, performansı artırmak için **PolyBase'i** kullanmayı düşünün. Ayrıntılar [için verileri Azure SQL Veri Ambarı'na yüklemek için PolyBase'i kullanın'](data-factory-azure-sql-data-warehouse-connector.md#use-polybase-to-load-data-into-azure-sql-data-warehouse) a bakın. Kullanım örneğine sahip bir geçiş için, [Azure Veri Fabrikası ile 15 dakikanın altında Azure SQL Veri Ambarına 1 TB Yükle'ye](data-factory-load-sql-data-warehouse.md)bakın.

### <a name="file-based-data-stores"></a>Dosya tabanlı veri depoları
*(Blob depolama, Data Lake Store, Amazon S3, şirket içi dosya sistemleri ve şirket içi HDFS içerir)*

* **Ortalama dosya boyutu ve dosya sayısı**: Kopyalama Etkinliği verileri bir defada bir dosyaya aktarın. Taşınacak aynı miktarda veriyle, veriler her dosyaiçin önyükleme aşaması nedeniyle birkaç büyük dosya yerine birçok küçük dosyadan oluşuyorsa, genel iş miktarı daha düşüktür. Bu nedenle, mümkünse, daha yüksek iş elde etmek için küçük dosyaları daha büyük dosyalarda birleştirin.
* **Dosya biçimi ve sıkıştırma**: Performansı artırmanın daha fazla yolu [için, serileştirme ve deserialization için dikkat edilmesi](#considerations-for-serialization-and-deserialization) gerekenler ve sıkıştırma bölümleri için dikkat edilmesi gerekenler [bölümüne](#considerations-for-compression) bakın.
* **Veri Yönetimi Ağ Geçidi'nin** gerekli olduğu şirket içi **dosya sistemi** senaryosu için, Veri Yönetimi Ağ Geçidi için [Gerekenler bölümüne](#considerations-for-data-management-gateway) bakın.

### <a name="relational-data-stores"></a>İlişkisel veri depoları
*(SQL Veritabanı içerir; SQL Veri Ambarı; Amazon Redshift; SQL Server veritabanları; ve Oracle, MySQL, DB2, Teradata, Sybase ve PostgreSQL veritabanları, vb.)*

* **Veri deseni**: Tablo şemanız kopya verime neden olabilir. Büyük satır boyutu, aynı miktarda veriyi kopyalamak için küçük satır boyutundan daha iyi bir performans sağlar. Bunun nedeni, veritabanının daha az satır içeren daha az veri toplu işlerini daha verimli bir şekilde alabiliyor olmasıdır.
* **Sorgu veya depoyordam**: Verileri daha verimli bir şekilde getirmek için Kopyalama Etkinliği kaynağında belirttiğiniz sorgu veya depoyordamın mantığını optimize edin.
* **Veri Yönetimi Ağ Geçidi'nin**kullanımını gerektiren SQL Server ve Oracle gibi şirket **içi ilişkisel veritabanları**için Veri Yönetimi Ağ Geçidi için Gerekenler bölümüne bakın.

## <a name="considerations-for-the-sink"></a>Lavabo için dikkat edilecek hususlar
### <a name="general"></a>Genel
Temel veri deposunun üzerinde veya aleyhine çalışan diğer iş yükleri tarafından bunalmış olmadığından emin olun.

Microsoft veri depoları için, veri depolarına özgü [izleme ve aetme konularına](#performance-reference) bakın. Bu konular, veri depolama performansı özelliklerini ve yanıt sürelerini en aza indirmenize ve iş hacmini en üst düzeye çıkarmanıza yardımcı olabilir.

**Blob depolamadan** **SQL Veri Ambarı'na**veri kopyalıyorsanız, performansı artırmak için **PolyBase'i** kullanmayı düşünün. Ayrıntılar [için verileri Azure SQL Veri Ambarı'na yüklemek için PolyBase'i kullanın'](data-factory-azure-sql-data-warehouse-connector.md#use-polybase-to-load-data-into-azure-sql-data-warehouse) a bakın. Kullanım örneğine sahip bir geçiş için, [Azure Veri Fabrikası ile 15 dakikanın altında Azure SQL Veri Ambarına 1 TB Yükle'ye](data-factory-load-sql-data-warehouse.md)bakın.

### <a name="file-based-data-stores"></a>Dosya tabanlı veri depoları
*(Blob depolama, Data Lake Store, Amazon S3, şirket içi dosya sistemleri ve şirket içi HDFS içerir)*

* **Kopyalama davranışı**: Farklı bir dosya tabanlı veri deposundan veri kopyalarsanız, **Copy Activity copyBehavior** özelliği üzerinden üç seçenek vardır. Hiyerarşiyi korur, hiyerarşiyi düzleştirir veya dosyaları birleştirir. Hiyerarşiyi koruma nın veya düzleştirmek çok az performans yüküne sahiptir veya hiç performans yükü yoktur, ancak dosyaları birleştirme, performans yüküne neden olur.
* **Dosya biçimi ve sıkıştırma**: Performansı artırmak için daha fazla yol için [serileştirme ve deserialization](#considerations-for-serialization-and-deserialization) ve sıkıştırma bölümleri [için Dikkat Edilmesi Gerekenler'e](#considerations-for-compression) bakın.
* **Blob depolama**: Şu anda, Blob depolama optimize veri aktarımı ve iş tükenen için yalnızca blok blobs destekler.
* **Veri Yönetimi Ağ Geçidi'nin**kullanımını gerektiren şirket içi dosya **sistemleri** senaryoları [için, Veri Yönetimi Ağ Geçidi](#considerations-for-data-management-gateway) için Gerekenler bölümüne bakın.

### <a name="relational-data-stores"></a>İlişkisel veri depoları
*(SQL Veritabanı, SQL Veri Ambarı, SQL Server veritabanları ve Oracle veritabanlarını içerir)*

* **Kopyalama davranışı**: **sqlSink**için ayarladığınız özelliklere bağlı olarak, Copy Activity verileri hedef veritabanına farklı şekillerde yazar.
  * Varsayılan olarak, veri hareketi hizmeti en iyi performansı sağlayan ek moduna veri eklemek için Toplu Kopya API'sini kullanır.
  * Depolanan bir yordamı lavaboda yapılandırırsanız, veritabanı verileri toplu yük yerine her seferinde bir satır uygular. Performans önemli ölçüde düşer. Veri setiniz büyükse, varsa **sqlWriterCleanupScript** özelliğini kullanmaya geçmeyi düşünün.
  * Her Kopyalama Etkinliği çalışması için **sqlWriterCleanupScript** özelliğini yapılandırırsanız, hizmet komut dosyasını tetikler ve verileri eklemek için Toplu Kopya API'sini kullanırsınız. Örneğin, en son verilerle tüm tablonun üzerine yazmak için, yeni verileri kaynaktan toplu yüklemeden önce tüm kayıtları silmek için bir komut dosyası belirtebilirsiniz.
* **Veri deseni ve toplu iş boyutu**:
  * Tablo şemanız kopya iş buzunu etkiler. Aynı miktarda veriyi kopyalamak için, büyük satır boyutu küçük bir satır boyutundan daha iyi performans sağlar, çünkü veritabanı daha az veri toplu işleyebilir.
  * Etkinliği Kopyala, verileri bir dizi toplu iş halinde ekler. **WriteBatchSize** özelliğini kullanarak toplu iş teki satır sayısını ayarlayabilirsiniz. Verilerinizin küçük satırları varsa, daha düşük toplu iş yükü ve daha yüksek iş kaynağından yararlanmak için **writeBatchSize** özelliğini daha yüksek bir değerle ayarlayabilirsiniz. Verilerinizin satır boyutu büyükse, **writeBatchSize'ı**artırırken dikkatli olun. Yüksek bir değer, veritabanının aşırı yüklenmesinden kaynaklanan bir kopya hatasına neden olabilir.
* **Veri Yönetimi Ağ Geçidi'nin**kullanımını gerektiren SQL Server ve Oracle gibi şirket içi **ilişkisel veritabanları** [için, Veri Yönetimi Ağ Geçidi](#considerations-for-data-management-gateway) için Dikkat Edilmesi bölümüne bakın.

### <a name="nosql-stores"></a>NoSQL mağazaları
*(Tablo depolama ve Azure Cosmos DB içerir)*

* **Tablo depolama için:**
  * **Bölüm**: Ara bölümlere veri yazmak performansı önemli ölçüde düşürür. Kaynak verilerinizi bölüm anahtarına göre sıralayarak verilerin birbiri ardına verimli bir şekilde eklenmesini veya verileri tek bir bölüme yazmak için mantığı ayarlayın.
* **Azure Cosmos DB**için:
  * **Toplu iş boyutu**: **writeBatchSize** özelliği, belge oluşturmak için Azure Cosmos DB hizmetine paralel istek sayısını ayarlar. Azure Cosmos DB'ye daha fazla paralel istek **gönderildiğinden, writeBatchSize'ı** artırdığınızda daha iyi performans bekleyebilirsiniz. Ancak, Azure Cosmos DB'ye yazarken azaltma ya da azaltma ya da "İstek oranı büyüktür" hata iletisi) izleyin. Belge boyutu, belgelerdeki terim sayısı ve hedef koleksiyonun dizin oluşturma ilkesi gibi çeşitli etkenler daralmalara neden olabilir. Daha yüksek kopya iş elde etmek için, örneğin S3 gibi daha iyi bir koleksiyon kullanmayı düşünün.

## <a name="considerations-for-serialization-and-deserialization"></a>Serileştirme ve deserialization için dikkat edilmesi gerekenler
Giriş veri setiniz veya çıktı veri setiniz bir dosya olduğunda serileştirme ve deserialization oluşabilir. Copy Activity tarafından desteklenen dosya biçimleriyle ilgili ayrıntıları içeren [Desteklenen dosya ve sıkıştırma biçimlerine](data-factory-supported-file-and-compression-formats.md) bakın.

**Kopyalama davranışı**:

* Dosya tabanlı veri depoları arasında dosya kopyalama:
  * Giriş ve çıktı veri kümelerinin her ikisi de aynı veya hiç dosya biçimi ayarlarına sahip olduğunda, veri hareketi hizmeti herhangi bir serileştirme veya deserialization olmadan ikili kopya yürütür. Kaynak ve lavabo dosya biçimi ayarlarının birbirinden farklı olduğu senaryoya göre daha yüksek bir iş kaynağı görürsünüz.
  * Giriş ve çıktı veri kümelerinin her ikisi de metin biçiminde olduğunda ve yalnızca kodlama türü farklı olduğunda, veri hareketi hizmeti yalnızca kodlama dönüştürmesini yapar. İkili kopyaya göre bazı performans yüküne neden olan herhangi bir serileştirme ve deserialization yapmaz.
  * Giriş ve çıktı veri kümelerinin her ikisi de sınıraşanlar gibi farklı dosya biçimlerine veya farklı yapılandırmalara sahip olduğunda, veri hareketi hizmeti kaynak verilerini akış, dönüştürme ve sonra da belirttiğiniz çıktı biçimine seri hale getirmek için deserialize eder. Bu işlem, diğer senaryolara kıyasla çok daha önemli bir performans yükü yle sonuçlanır.
* Dosya tabanlı olmayan bir veri deposuna/dosya deposundan dosyaları kopyaladiğinizde (örneğin, dosya tabanlı bir depodan ilişkisel bir depoya) serileştirme veya deserialization adımı gerekir. Bu adım, önemli performans yükü ile sonuçlanır.

**Dosya biçimi**: Seçtiğiniz dosya biçimi kopyalama performansını etkileyebilir. Örneğin, Avro, meta verileri verilerle depolayan kompakt bir ikili biçimdir. Hadoop ekosisteminde işleme ve sorgulama için geniş bir desteğe sahiptir. Ancak Avro, serileştirme ve deserialization için daha pahalıdır ve bu da metin biçimine göre daha düşük kopya iş verisiyle sonuçlanır. Tümsel seli boyunca dosya biçimi seçiminizi yapın. Verilerin hangi formda depolanarak depolanmasıyla, kaynak veri depolarından veya dış sistemlerden çıkarılacak şekilde başlayın; depolama, analitik işleme ve sorgulama için en iyi biçim; ve verilerin raporlama ve görselleştirme araçları için veri martlarına hangi biçimde aktarılması gerektiği. Bazen okuma ve yazma performansı için en uygun olmayan dosya biçimi, genel analitik süreci göz önünde bulundurursak iyi bir seçim olabilir.

## <a name="considerations-for-compression"></a>Sıkıştırma için dikkat edilmesi gerekenler
Giriş veya çıktı veri kümeniz bir dosya olduğunda, hedefe veri yazarken sıkıştırma veya dekompresyon gerçekleştirecek Kopya Etkinliği'ni ayarlayabilirsiniz. Sıkıştırmayı seçtiğinizde, giriş/çıkış (I/O) ve CPU arasında bir denge kurabilirsiniz. Verilerin sıkıştırılma maliyeti bilgi işlem kaynaklarında ekstra. Ancak buna karşılık, ağ G/Ç'yi ve depolamayı azaltır. Verilerinize bağlı olarak, genel kopya verisinde bir artış görebilirsiniz.

**Codec**: Kopyalama Etkinliği gzip, bzip2 ve Deflate sıkıştırma türlerini destekler. Azure HDInsight, işleme için üç türde de tüketebilir. Her sıkıştırma codec avantajları vardır. Örneğin, bzip2 en düşük kopya iş verime sahiptir, ancak işleme için bölebilirsiniz çünkü bzip2 ile en iyi Hive sorgu performansı olsun. Gzip en dengeli seçenektir ve en sık kullanılır. Uç-uç senaryonuza en uygun codec'i seçin.

**Seviye**: Her sıkıştırma codec için iki seçenek arasından seçim yapabilirsiniz: en hızlı sıkıştırılmış ve en iyi şekilde sıkıştırılmış. En hızlı sıkıştırılmış seçenek, ortaya çıkan dosya en iyi şekilde sıkıştırılmamış olsa bile verileri mümkün olduğunca çabuk sıkıştırır. En iyi şekilde sıkıştırılmış seçenek sıkıştırma üzerinde daha fazla zaman harcar ve en az miktarda veri verir. Sizin durumunuzda daha iyi genel performans sağlayanı görmek için her iki seçeneği de test edebilirsiniz.

**Bir husus**: Şirket içi mağaza ile bulut arasında büyük miktarda veriyi kopyalamak için sıkıştırma ile geçici blob depolamayı kullanmayı düşünün. Şirket ağınızın ve Azure hizmetlerinizin bant genişliği sınırlayıcı bir faktör olduğunda ve giriş veri kümesinin ve çıktı veri kümesinin sıkıştırmamış biçimde olmasını istediğinizde, geçici depolama alanı kullanmak yararlıdır. Daha ayrıntılı olarak, tek bir kopyalama etkinliğini iki kopyalama etkinliğine ayırabilirsiniz. İlk kopyalama etkinliği kaynaktan sıkıştırılmış biçimde geçici veya evreleme blob kopyaları. İkinci kopyalama etkinliği, sıkıştırılmış verileri evrelemeden kopyalar ve lavaboya yazarken basıncı giderır.

## <a name="considerations-for-column-mapping"></a>Sütun eşleme için dikkat edilecek noktalar
Giriş sütunlarının tamamını veya bir alt kümesini çıktı sütunlarına eşlemek için Kopyalama Etkinliği'ndeki **sütun Eşlemeleri** özelliğini ayarlayabilirsiniz. Veri hareketi hizmeti kaynağından verileri okur sonra, verileri lavaboya yazmadan önce veriler üzerinde sütun eşleme gerçekleştirmesi gerekir. Bu ekstra işlem, kopyalama verime işlemini azaltır.

Kaynak veri deponuz sorgulanabilirse, örneğin, SQL Veritabanı veya SQL Server gibi ilişkisel bir depoysa veya Tablo depolama veya Azure Cosmos DB gibi bir NoSQL deposuysa, sütun filtreleme ve yeniden sıralama mantığını sütun eşleme kullanmak yerine **sorgu** özelliğine itmeyi düşünün. Bu şekilde, veri hareketi hizmeti çok daha verimli olduğu kaynak veri deposundan verileri okurken projeksiyon oluşur.

## <a name="other-considerations"></a>Diğer konular
Kopyalamak istediğiniz verilerin boyutu büyükse, Veri Fabrikası'ndaki dilimleme mekanizmasını kullanarak verileri daha fazla bölmek için iş mantığınızı ayarlayabilirsiniz. Ardından, her Kopyalama Etkinliği çalışması için veri boyutunu azaltmak için Etkinliği daha sık çalışacak şekilde zamanlayın.

Veri Fabrikası'nın aynı veri deposuna aynı anda bağlayıcı etmesini gerektiren veri kümelerinin ve kopyalama etkinliklerinin sayısı konusunda dikkatli olun. Birçok eşzamanlı kopyalama işleri bir veri deposuna gaz verebilir ve düşük performansa, iş iç yeniden denemelerini kopyalamaya ve bazı durumlarda yürütme hatalarına neden olabilir.

## <a name="sample-scenario-copy-from-an-on-premises-sql-server-to-blob-storage"></a>Örnek senaryo: Şirket içi bir SQL Server'dan Blob depolamaalanına kopyalama
**Senaryo**: Şirket içi bir SQL Server'dan CSV formatında Blob depolamasına kadar verileri kopyalamak için bir boru hattı oluşturulur. Kopyalama işini daha hızlı hale getirmek için CSV dosyaları bzip2 biçimine sıkıştırılmalıdır.

**Test ve analiz**: Kopyalama Etkinliği'nin iş bölümü 2 MBp'den azdır ve bu da performans kriterinden çok daha yavaştır.

**Performans analizi ve tuning**: Performans sorununu gidermek için verilerin nasıl işlendiğine ve taşındığına bakalım.

1. **Verileri oku**: Ağ Geçidi SQL Server'a bir bağlantı açar ve sorguyu gönderir. SQL Server intranet üzerinden Ağ Geçidi'ne veri akışı göndererek yanıt verir.
2. **Verileri serihale ve sıkıştırma**: Ağ geçidi veri akışını CSV biçimine serileştirir ve verileri bzip2 akışına sıkıştırır.
3. **Veri yazma**: Ağ geçidi internet üzerinden Blob depolama bzip2 akışı yükler.

Gördüğünüz gibi, veriler akış sıralı bir şekilde işleniyor ve taşınıyor: SQL Server > LAN > Ağ Geçidi > WAN > Blob depolama. **Genel performans, boru hattı boyunca minimum iş elde edilen iş ile kaplanır.**

![Veri akışı](./media/data-factory-copy-activity-performance/case-study-pic-1.png)

Aşağıdaki etkenlerden biri veya birkaçı performans darboğazına neden olabilir:

* **Kaynak**: SQL Server'ın kendisi ağır yükler nedeniyle düşük iş seviyesine sahiptir.
* **Veri Yönetimi Ağ Geçidi**:
  * **LAN**: Gateway, SQL Server makinesinden uzakta dır ve düşük bant genişliğine sahiptir.
  * **Ağ Geçidi**: Ağ Geçidi aşağıdaki işlemleri gerçekleştirmek için yük sınırlamalarına ulaşmıştır:
    * **Serileştirme**: Veri akışınıN CSV formatına serileştirilmesi yavaş iş akışına sahiptir.
    * **Sıkıştırma**: Yavaş sıkıştırma codec'ini seçtiniz (örneğin, Core i7 ile 2,8 MBps olan bzip2).
  * **WAN**: Kurumsal ağ ile Azure hizmetleriniz arasındaki bant genişliği düşüktür (örneğin, T1 = 1.544 kbps; T2 = 6.312 kbps).
* **Lavabo**: Blob depolama düşük iş seviyesine sahiptir. (SLA en az 60 MBps garanti ettiği için bu senaryo pek olası değildir.)

Bu durumda, bzip2 veri sıkıştırma tüm ardışık alıcı-ı sözcük teyavaşlama olabilir. Gzip sıkıştırma codec'ine geçmek bu darboğazdan daha kolay olabilir.

## <a name="sample-scenarios-use-parallel-copy"></a>Örnek senaryolar: Paralel kopya kullanın
**Senaryo I:** Şirket içi dosya sisteminden Blob depolamasına 1.000 adet 1MB dosya kopyalayın.

**Analiz ve performans atomu**: Örneğin, dört çekirdekli bir makineye ağ geçidi yüklediyseniz, Veri Fabrikası dosyaları dosya sisteminden Blob depolamasına aynı anda taşımak için 16 paralel kopya kullanır. Bu paralel yürütme yüksek iş elde neden olmalıdır. Ayrıca, paralel kopya sayısını da açıkça belirtebilirsiniz. Birçok küçük dosyayı kopyaladiğinizde, paralel kopyalar kaynakları daha etkili bir şekilde kullanarak elde edilen kaynak oluşturmayı önemli ölçüde yardımcı olur.

![Senaryo 1](./media/data-factory-copy-activity-performance/scenario-1.png)

**Senaryo II**: Blob depolamadan Data Lake Store Analytics'e kadar her biri 500 MB'lık 20 blob kopyalayın ve performansı ayarlayın.

**Analiz ve performans ayarlaması**: Bu senaryoda, Veri Fabrikası tek kopya **(paralel Kopyalar** 1 olarak ayarlanmış) ve tek bulutlu veri hareket birimleri kullanarak Blob depolamadan Data Lake Store'a kadar olan verileri kopyalar. Gözlemlediğiniz iş, [performans başvuru bölümünde](#performance-reference)açıklanana yakın olacaktır.

![Senaryo 2](./media/data-factory-copy-activity-performance/scenario-2.png)

**Senaryo III**: Tek tek dosya boyutu düzinelerce MB'den büyüktür ve toplam birim büyüktür.

**Analiz ve performans tornalama**: **Paralel Kopyaların** artırılması, tek bulutlu bir DMU'nun kaynak sınırlamaları nedeniyle daha iyi kopya performansı yla sonuçlanmaz. Bunun yerine, veri hareketini gerçekleştirmek için daha fazla kaynak elde etmek için daha fazla bulut DMU belirtmeniz gerekir. **ParallelCopy** özelliği için bir değer belirtmeyin. Veri Fabrikası sizin için paralellik işler. Bu durumda, **cloudDataMovementUnits'i** 4 olarak ayarlarsanız, yaklaşık dört kez bir iş çıkarı oluşur.

![3. Senaryo](./media/data-factory-copy-activity-performance/scenario-3.png)

## <a name="reference"></a>Başvuru
Desteklenen veri depolarından bazıları için performans izleme ve tuning başvuruları aşağıda veda edilmiştir:

* Azure Blob depolama: Blob depolama ve [Performans ve Blob depolama için ölçeklenebilirlik kontrol listesi](../../storage/blobs/storage-performance-checklist.md)için [ölçeklenebilirlik ve performans hedefleri.](../../storage/blobs/scalability-targets.md)
* Azure Tablo depolama: Tablo depolama ve Performans ve [Tablo depolama için ölçeklenebilirlik denetim listesi için](../../storage/tables/storage-performance-checklist.md) [ölçeklenebilirlik ve performans hedefleri.](../../storage/tables/scalability-targets.md)
* Azure SQL Veritabanı: [Performansı izleyebilir](../../sql-database/sql-database-single-database-monitor.md) ve veritabanı işlem birimini (DTU) kontrol edebilirsiniz
* Azure SQL Veri Ambarı: Yeteneği veri ambarı birimlerinde (DWUs) ölçülür; bkz. [Azure SQL Veri Ambarı'nda bilgi işlem gücünü yönet (Genel Bakış)](../../synapse-analytics/sql-data-warehouse/sql-data-warehouse-manage-compute-overview.md)
* Azure Cosmos DB: [Azure Cosmos DB'de performans düzeyleri](../../cosmos-db/performance-levels.md)
* Şirket içi SQL Server: [Performans için izleme ve ayarlama](https://msdn.microsoft.com/library/ms189081.aspx)
* Şirket içi dosya sunucusu: [Dosya sunucuları için performans ayarı](https://msdn.microsoft.com/library/dn567661.aspx)
