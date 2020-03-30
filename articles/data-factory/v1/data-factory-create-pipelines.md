---
title: Veri Fabrikası'nda Boru Hatları Oluşturma/Zamanlama, Zincir Leme Faaliyetleri
description: Verileri taşımak ve dönüştürmek için Azure Veri Fabrikası'nda bir veri ardışık alanı oluşturmayı öğrenin. Kullanıma hazır bilgiler üretmek için veri odaklı bir iş akışı oluşturun.
services: data-factory
documentationcenter: ''
author: djpmsft
ms.author: daperlov
manager: jroth
ms.reviewer: maghan
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/10/2018
ms.openlocfilehash: f93bea240ee3f139c9be84199d116f9f3f231261
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79281528"
---
# <a name="pipelines-and-activities-in-azure-data-factory"></a>Azure Veri Fabrikası'nda Boru Hatları ve Aktiviteler
> [!div class="op_single_selector" title1="Kullandığınız Veri Fabrikası hizmetisürümünü seçin:"]
> * [Sürüm 1](data-factory-create-pipelines.md)
> * [Sürüm 2 (geçerli sürüm)](../concepts-pipelines-activities.md)

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Veri Fabrikası hizmetinin geçerli sürümünü kullanıyorsanız, [V2'deki Boru Hatları bölümüne](../concepts-pipelines-activities.md)bakın.

Bu makale, Azure Data Factory’de işlem hatlarını ve etkinlikleri anlamanıza ve veri hareketi ile veri işleme senaryolarınız için uçtan uca veri odaklı iş akışları oluşturmak amacıyla bunları nasıl kullanacağınızı anlamanıza yardımcı olur.

> [!NOTE]
> Bu makalede, Azure Veri [Fabrikası'na Giriş'ten](data-factory-introduction.md)geçtiğinizi varsayar. Veri fabrikaları oluşturma konusunda uygulamalı deneyiminiz yoksa, veri [dönüştürme öğreticisi](data-factory-build-your-first-pipeline.md) ve/veya [veri hareketi öğreticisi](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) bu makaleyi daha iyi anlamanıza yardımcı olacaktır.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="overview"></a>Genel Bakış
Bir veri fabrikasında bir veya daha fazla işlem hattı olabilir. İşlem hattı, bir araya geldiğinde bir görev gerçekleştiren mantıksal etkinlik grubudur. Bir işlem hattındaki etkinlikler, verilerinizde gerçekleştirilecek eylemleri tanımlar. Örneğin, kopyalama etkinliğini kullanarak şirket içi SQL Server’dan Azure Blob Depolama’ya veri kopyalayabilirsiniz. Ardından bir Azure HDInsight kümesinde Hive betiği çalıştıran bir Hive etkinliği kullanıp blob depolamadaki verileri işleyerek/dönüştürerek çıktı verileri üretebilirsiniz. Son olarak, ikinci bir kopyalama etkinliği kullanarak üzerinde iş zekası (BI) raporlama çözümlerinin oluşturulduğu bir Azure SQL Veri Ambarı’na çıktı verilerini kopyalayabilirsiniz.

Bir etkinliğin sıfır veya sıfırdan çok giriş [veri kümesi](data-factory-create-datasets.md) olabilir ve her etkinlik bir veya birden çok çıkış [veri kümesi](data-factory-create-datasets.md) oluşturabilir. Aşağıdaki diyagramda, Data Factory içindeki işlem hattı, etkinlik ve veri kümesi arasındaki ilişki gösterilmektedir:

![Ardışık hatlar, etkinlik ve veri kümesi arasındaki ilişki](media/data-factory-create-pipelines/relationship-pipeline-activity-dataset.png)

Bir ardışık iş, etkinlikleri her biri yerine ayrı ayrı bir küme olarak yönetmenize olanak tanır. Örneğin, ardışık işlemle bağımsız olarak uğraşmak yerine bir ardışık hattı dağıtabilir, zamanlayabilir, askıya alabilir ve devam ettirebilirsiniz.

Data Factory iki tür etkinliği destekler: veri taşıma etkinlikleri ve veri dönüştürme etkinlikleri. Her etkinlik sıfır veya daha fazla giriş [veri kümesine](data-factory-create-datasets.md) sahip olabilir ve bir veya daha fazla çıktı veri kümesi üretebilir.

Giriş veri kümesi, veri işlem hattındaki bir etkinlik için girişi ve çıktı veri kümesi, etkinliğin çıktısını temsil eder. Veri kümeleri tablolar, dosyalar, klasörler ve belgeler gibi farklı veri depolarındaki verileri tanımlar. Bir veri kümesi oluşturduktan sonra, bu kümeyi bir işlem hattındaki etkinliklerle birlikte kullanabilirsiniz. Örneğin, veri kümesi bir Kopyalama Etkinliğinin veya HDInsightHive Etkinliğinin giriş/çıkış veri kümesi olabilir. Veri kümeleri hakkında daha fazla bilgi için [Azure Data Factory'de Veri Kümeleri](data-factory-create-datasets.md) makalesine bakın.

### <a name="data-movement-activities"></a>Veri taşıma etkinlikleri
Data Factory’deki Kopyalama Etkinliği bir kaynak veri deposundan havuz veri deposuna verileri kopyalar. Data Factory aşağıdaki veri depolarını destekler. Herhangi bir kaynaktan gelen veriler herhangi bir havuza yazılabilir. Bir depoya veya depodan veri kopyalama hakkında bilgi edinmek için veri deposuna tıklayın.

[!INCLUDE [data-factory-supported-data-stores](../../../includes/data-factory-supported-data-stores.md)]

> [!NOTE]
> * taşıyan veri depoları şirket içi veya Azure IaaS üzerinde olabilir ve bir şirket içi/Azure IaaS makinesine [Veri Yönetimi Ağ Geçidi](data-factory-data-management-gateway.md) yüklemenizi gerektirir.

Daha fazla bilgi için [Veri Taşıma Etkinlikleri](data-factory-data-movement-activities.md) makalesine bakın.

### <a name="data-transformation-activities"></a>Veri dönüştürme etkinlikleri
[!INCLUDE [data-factory-transformation-activities](../../../includes/data-factory-transformation-activities.md)]

Daha fazla bilgi için [Veri Dönüştürme Etkinlikleri](data-factory-data-transformation-activities.md) makalesine bakın.

### <a name="custom-net-activities"></a>Özel .NET etkinlikleri
Kopya Etkinliği'nin desteklemediği bir veri deposuna/veri deposundan veri taşımanız veya kendi mantığınızı kullanarak verileri dönüştürmeniz gerekiyorsa, özel bir **.NET etkinliği**oluşturun. Özel bir etkinlik oluşturma ve kullanma hakkında ayrıntılı bilgi için bkz. [Azure Data Factory işlem hattında özel etkinlikler kullanma](data-factory-use-custom-activities.md).

## <a name="schedule-pipelines"></a>Ardışık hatlar zamanla
Bir ardışık ardışık hat, yalnızca **başlangıç** zamanı ile **bitiş** saati arasında etkindir. Başlangıç saatinden önce veya bitiş saatinden sonra yürütülmez. Ardışık alan, başlangıç ve bitiş saatine bakılmaksızın yürütülmez. Bir boru hattının çalışması için duraklatmamalıdır. Azure Veri [Fabrikası'nda](data-factory-scheduling-and-execution.md) zamanlama ve yürütmenin nasıl çalıştığını anlamak için Zamanlama ve Yürütme'ye bakın.

## <a name="pipeline-json"></a>İşlem Hattı JSON
Bir işlem hattının JSON biçiminde nasıl tanımlandığına daha yakından bakalım. Bir işlem hattının genel yapısı şu şekilde görünür:

```json
{
    "name": "PipelineName",
    "properties":
    {
        "description" : "pipeline description",
        "activities":
        [

        ],
        "start": "<start date-time>",
        "end": "<end date-time>",
        "isPaused": true/false,
        "pipelineMode": "scheduled/onetime",
        "expirationTime": "15.00:00:00",
        "datasets":
        [
        ]
    }
}
```

| Etiket | Açıklama | Gerekli |
| --- | --- | --- |
| ad |İşlem hattının adı. İşlem hattının gerçekleştirdiği eylemi temsil eden bir ad belirtin. <br/><ul><li>En fazla karakter sayısı: 260</li><li>Bir harf numarası veya bir alt\_puan ile başlamanız gerekir ( )</li><li>Aşağıdaki karakterlere izin verilmez: ".", "+", "?", "/",\*"<", ">",\\"","",""&", ""</li></ul> |Evet |
| açıklama | İşlem hattının ne için kullanıldığını açıklayan metni belirtin. |Evet |
| etkinlikler | **Etkinlikler** bölümünde tanımlanmış bir veya daha fazla etkinlik olabilir. Etkinlikler JSON öğesi hakkında ayrıntılar için sonraki bölüme bakın. | Evet |
| start | Boru hattı için tarih-saat başlatın. [ISO formatında](https://en.wikipedia.org/wiki/ISO_8601)olmalıdır. Örneğin: `2016-10-14T16:32:41Z`. <br/><br/>Yerel bir saat belirtmek mümkündür, örneğin bir EST zamanı. İşte bir örnek: `2016-02-27T06:00:00-05:00`", 06:00 EST olduğunu.<br/><br/>Başlangıç ve bitiş özellikleri birlikte ardışık hatlar için etkin dönemi belirtir. Çıkış dilimleri sadece bu etkin dönemde üretilir. |Hayır<br/><br/>Son özellik için bir değer belirtirseniz, başlangıç özelliği için değer belirtmeniz gerekir.<br/><br/>Başlangıç ve bitiş saatleri, bir ardışık hat lar oluşturmak için boş olabilir. Ardışık hattın çalışması için etkin bir dönem ayarlamak için her iki değeri de belirtmeniz gerekir. Bir ardışık hatlar oluştururken başlangıç ve bitiş saatlerini belirtmezseniz, bunları Daha sonra Set-AzDataFactoryPipelineActivePeriod cmdlet kullanarak ayarlayabilirsiniz. |
| end | Boru hattı için bitiş tarihi. Belirtilen iso formatında olmalıdır. Örneğin, `2016-10-14T17:32:41Z` <br/><br/>Yerel bir saat belirtmek mümkündür, örneğin bir EST zamanı. İşte bir örnek: `2016-02-27T06:00:00-05:00`, 06 EST olduğunu.<br/><br/>İşlem hattını süresiz olarak çalıştırmak için end özelliği değerini 9999-09-09 olarak ayarlayın. <br/><br/> Bir ardışık ardışık hat, yalnızca başlangıç zamanı ile bitiş saati arasında etkindir. Başlangıç saatinden önce veya bitiş saatinden sonra yürütülmez. Ardışık alan, başlangıç ve bitiş saatine bakılmaksızın yürütülmez. Bir boru hattının çalışması için duraklatmamalıdır. Azure Veri [Fabrikası'nda](data-factory-scheduling-and-execution.md) zamanlama ve yürütmenin nasıl çalıştığını anlamak için Zamanlama ve Yürütme'ye bakın. |Hayır <br/><br/>Başlangıç özelliği için bir değer belirtirseniz, bitiş özelliği için değer belirtmeniz gerekir.<br/><br/>**Başlangıç** özelliği için notlara bakın. |
| isPaused | Doğru olarak ayarlanırsa, ardışık hatlar çalışmaz. Duraklatılmış durumda. Varsayılan değer = false. Bu özelliği, bir ardışık hattı etkinleştirmek veya devre dışı kullanabilirsiniz. |Hayır |
| pipelineMode | Zamanlama yöntemi, ardışık hatlar için çalışır. İzin verilen değerler şunlardır: zamanlanmış (varsayılan), tek seferlik.<br/><br/>'Zamanlanmış' ardışık ardışık ardışık ardışık etkin döneme (başlangıç ve bitiş saati) göre belirli bir zaman aralığında çalıştığını gösterir. 'Onetime' boru hattının yalnızca bir kez çalıştığını gösterir. Bir kez oluşturulan tek seferlik ardışık ardışık lar şu anda değiştirilemez/güncelleştirilemez. Tek seferlik ayar la ilgili ayrıntılar için [Onetime ardışık hattına](#onetime-pipeline) bakın. |Hayır |
| sona ermeZamanı | [Tek seferlik boru hattının](#onetime-pipeline) geçerli olduğu ve sağlanması gereken yaratılıştan sonraki süre. Etkin, başarısız veya bekleyen çalışır yoksa, son kullanma süresine ulaştığında ardışık alan otomatik olarak silinir. Varsayılan değer:`"expirationTime": "3.00:00:00"`|Hayır |
| veri kümeleri |Ardışık alanda tanımlanan etkinlikler tarafından kullanılacak veri kümelerinin listesi. Bu özellik, bu ardışık almaya özgü ve veri fabrikası içinde tanımlanmamış veri kümelerini tanımlamak için kullanılabilir. Bu ardışık ardışık ardışık ardışık ardışık ardışık aygıtlar yalnızca bu ardışık işlem tarafından kullanılabilir ve paylaşılamaz. Ayrıntılar için [Kapsamlı veri kümelerini](data-factory-create-datasets.md#scoped-datasets) görün. |Hayır |

## <a name="activity-json"></a>Etkinlik JSON
**Etkinlikler** bölümünde tanımlanmış bir veya daha fazla etkinlik olabilir. Her etkinlik aşağıdaki üst düzey yapıya sahiptir:

```json
{
    "name": "ActivityName",
    "description": "description",
    "type": "<ActivityType>",
    "inputs": "[]",
    "outputs": "[]",
    "linkedServiceName": "MyLinkedService",
    "typeProperties":
    {

    },
    "policy":
    {
    },
    "scheduler":
    {
    }
}
```

Aşağıdaki tabloda, etkinlik JSON tanımındaki özellikler açıklamaktadır:

| Etiket | Açıklama | Gerekli |
| --- | --- | --- |
| ad | Etkinliğin adı. Etkinliğin gerçekleştirdiği eylemi temsil eden bir ad belirtin. <br/><ul><li>En fazla karakter sayısı: 260</li><li>Bir harf numarası veya bir alt\_puan ile başlamanız gerekir ( )</li><li>Aşağıdaki karakterlere izin verilmez: ".", "+", "?", "/", "<",">","","","","%","&",":","\\"</li></ul> |Evet |
| açıklama | Etkinliğin ne olduğunu veya ne için kullanıldığını açıklayan metin |Evet |
| type | Etkinliğin türü. Farklı türde etkinlikler için [Veri Hareketi Etkinlikleri](#data-movement-activities) ve Veri Dönüştürme [Etkinlikleri](#data-transformation-activities) bölümlerine bakın. |Evet |
| Giriş |Etkinlik tarafından kullanılan giriş tabloları<br/><br/>`// one input table`<br/>`"inputs":  [ { "name": "inputtable1"  } ],`<br/><br/>`// two input tables` <br/>`"inputs":  [ { "name": "inputtable1"  }, { "name": "inputtable2"  } ],` |Evet |
| Çıkış |Etkinlik tarafından kullanılan çıktı tabloları.<br/><br/>`// one output table`<br/>`"outputs":  [ { "name": "outputtable1" } ],`<br/><br/>`//two output tables`<br/>`"outputs":  [ { "name": "outputtable1" }, { "name": "outputtable2" }  ],` |Evet |
| linkedServiceName |Etkinlik tarafından kullanılan bağlı hizmetin adı. <br/><br/>Bir etkinlik için gerekli işlem ortamına bağlanan bağlı hizmeti belirtmeniz gerekebilir. |HDInsight Etkinliği ve Azure Machine Learning Toplu Puanlama Etkinliği için evet <br/><br/>Diğer tümü için hayır |
| typeProperties |**TypeProperties** bölümündeki özellikler etkinliğin türüne bağlıdır. Bir etkinliğin tür özelliklerini görmek için önceki bölümde verilen etkinlik bağlantılarına tıklayın. | Hayır |
| ilke |Etkinliğin çalışma zamanı davranışını etkileyen ilkeler. Belirtilmemişse, varsayılan ilkeler kullanılır. |Hayır |
| scheduler | Etkinlik için istenen zamanlamayı tanımlamak için "zamanlayıcı" özelliği kullanılır. Alt özellikleri, [bir veri kümesindeki kullanılabilirlik özelliğindeki özelliklerle](data-factory-create-datasets.md#dataset-availability)aynıdır. |Hayır |

### <a name="policies"></a>İlkeler
İlkeler, özellikle bir tablonun dilimi işlendiğinde, bir etkinliğin çalışma zamanı davranışını etkiler. Aşağıdaki tablo ayrıntıları sağlar.

| Özellik | İzin verilen değerler | Varsayılan Değer | Açıklama |
| --- | --- | --- | --- |
| eşzamanlılık |Tamsayı <br/><br/>Maksimum değer: 10 |1 |Etkinliğin eşzamanlı yürütme sayısı.<br/><br/>Farklı dilimlerde gerçekleşebilecek paralel etkinlik yürütmelerinin sayısını belirler. Örneğin, bir etkinliğin büyük bir kullanılabilir veri kümesinden geçmesi gerekiyorsa, daha büyük bir eşzamanlılık değerine sahip olmak veri işlemeyi hızlandırİr. |
| yürütmeÖncelikli Sipariş |En Yeni İlk<br/><br/>En Eskiİlk |En Eskiİlk |İşlenen veri dilimlerinin sırasını belirler.<br/><br/>Örneğin, 2 dilim (biri 16:00'da, diğeri 17:00'de oluyor) varsa ve her ikisi de yürütmeyi bekliyorsa. YürütmePriorityOrder'ı NewestFirst olarak ayarlarsanız, ilk olarak saat 17:00'deki dilim işlenir. Benzer şekilde, executionPriorityORder'ı En Eski FIrst olarak ayarlarsanız, saat 16:00'daki dilim işlenir. |
| retry |Tamsayı<br/><br/>Maksimum değer 10 olabilir |0 |Dilimiçin veri işleme önce yeniden deneme sayısı Hata olarak işaretlenir. Bir veri dilimiiçin etkinlik yürütme belirtilen yeniden deneme sayısına kadar yeniden denendir. Yeniden deneme, arızadan sonra mümkün olan en kısa sürede yapılır. |
| timeout |TimeSpan |00:00:00 |Etkinlik için zaman dilimi. Örnek: 00:10:00 (zaman 10 dakika anlamına gelir)<br/><br/>Bir değer belirtilmemişse veya 0 ise, zaman acısı sonsuzdur.<br/><br/>Bir dilimdeki veri işleme süresi zaman aşımı değerini aşarsa, iptal edilir ve sistem işlemeyi yeniden denemeye çalışır. Yeniden deneme sayısı yeniden deneme özelliğine bağlıdır. Zaman anına kadar zaman dilimi oluştuğunda, durum TimedOut olarak ayarlanır. |
| Gecikme |TimeSpan |00:00:00 |Dilimin veri işlemesi başlamadan önce gecikmeyi belirtin.<br/><br/>Bir veri dilimi için etkinliğin yürütülmesi, Gecikme beklenen yürütme süresini geçtikten sonra başlatılır.<br/><br/>Örnek: 00:10:00 (10 dakika gecikme anlamına gelir) |
| longRetry |Tamsayı<br/><br/>Maksimum değer: 10 |1 |Dilim yürütme başarısız olmadan önce uzun yeniden deneme denemelerinin sayısı.<br/><br/>longRetry girişimleri longRetryInterval tarafından aralıklı. Bu nedenle, yeniden deneme denemeleri arasında bir süre belirtmeniz gerekiyorsa, longRetry'yi kullanın. Hem Retry ve longRetry belirtilirse, her longRetry girişimi Retry girişimleri içerir ve girişimleri maksimum sayıda Retry * longRetry olduğunu.<br/><br/>Örneğin, etkinlik ilkesinde aşağıdaki ayarlar varsa:<br/>Yeniden deneme sayısı: 3<br/>longRetry: 2<br/>longRetryInterval: 01:00:00<br/><br/>Yürütülecek tek bir dilim olduğunu varsayalım (durum Bekliyor) ve etkinlik yürütmesi her seferinde başarısız oluyor. Başlangıçta art arda 3 infaz girişimi olacaktır. Her denemeden sonra, dilim durumu Yeniden Deneme olacaktır. İlk 3 deneme bittikten sonra, dilim durumu LongRetry olacaktır.<br/><br/>Bir saat sonra (yani, longRetryInteval değeri), 3 ardışık yürütme girişimleri başka bir dizi olacaktır. Bundan sonra, dilim durumu Başarısız olur ve başka yeniden deneme denilmeye çalışılacak. Bu nedenle genel 6 girişimleri yapıldı.<br/><br/>Herhangi bir yürütme başarılı olursa, dilim durumu Hazır olur ve başka yeniden deneme denenir.<br/><br/>longRetry, bağımlı verilerin deterministik olmayan zamanlarda geldiği veya genel ortamın veri işlemenin gerçekleştiği durumlarda kullanılabilir. Bu gibi durumlarda, birbiri ardına yeniden denemeler yapmak yardımcı olmayabilir ve bunu bir süre sonra yapmak istenilen çıktıile sonuçlanır.<br/><br/>Dikkat Word: longRetry veya longRetryInterval için yüksek değerler ayarlamayın. Genellikle, daha yüksek değerler diğer sistemik sorunları ima. |
| longRetryInterval |TimeSpan |00:00:00 |Uzun yeniden deneme denemeleri arasındaki gecikme |

## <a name="sample-copy-pipeline"></a>Örnek kopyalama işlem hattı
Aşağıdaki örnek işlem hattında, **Etkinlikler** bölümünde **Kopyalama** türünde olan bir etkinlik vardır. Bu örnekte [kopyalama etkinliği](data-factory-data-movement-activities.md), verileri Azure Blob depolama alanından Azure SQL veritabanına kopyalar.

```json
{
  "name": "CopyPipeline",
  "properties": {
    "description": "Copy data from a blob to Azure SQL table",
    "activities": [
      {
        "name": "CopyFromBlobToSQL",
        "type": "Copy",
        "inputs": [
          {
            "name": "InputDataset"
          }
        ],
        "outputs": [
          {
            "name": "OutputDataset"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "BlobSource"
          },
          "sink": {
            "type": "SqlSink",
            "writeBatchSize": 10000,
            "writeBatchTimeout": "60:00:00"
          }
        },
        "Policy": {
          "concurrency": 1,
          "executionPriorityOrder": "NewestFirst",
          "retry": 0,
          "timeout": "01:00:00"
        }
      }
    ],
    "start": "2016-07-12T00:00:00Z",
    "end": "2016-07-13T00:00:00Z"
  }
}
```

Aşağıdaki noktalara dikkat edin:

* Etkinlikler bölümünde, **türü****Copy** olarak ayarlanmış yalnızca bir etkinlik vardır.
* Etkinlik girdisi **InputDataset** olarak, etkinlik çıktısı ise **OutputDataset** olarak ayarlanmıştır. JSON biçiminde veri kümeleri tanımlamak için [Veri Kümeleri](data-factory-create-datasets.md) makalesine bakın.
* **typeProperties** bölümünde **BlobSource** kaynak türü, **SqlSink** de havuz türü olarak belirtilir. Veri [hareketi etkinlikleri](#data-movement-activities) bölümünde, veri depolamaya/bu veri deposundan veri taşıma hakkında daha fazla bilgi edinmek için kaynak veya lavabo olarak kullanmak istediğiniz veri deposunu tıklatın.

Bu ardışık alanı oluşturmanın tam bir bölümü için [Bkz. Öğretici: Blob Depolama'dan SQL Veritabanı'na veri kopyalama.](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)

## <a name="sample-transformation-pipeline"></a>Örnek dönüştürme işlem hattı
Aşağıdaki örnek işlem hattında, **etkinlikler** bölümünde **HDInsightHive** türünde olan bir etkinlik vardır. Bu örnekte [HDInsight Hive etkinliği](data-factory-hive-activity.md), bir Azure HDInsight Hadoop kümesinde Hive betik dosyası çalıştırarak verileri bir Azure Blob depolamadan dönüştürür.

```json
{
    "name": "TransformPipeline",
    "properties": {
        "description": "My first Azure Data Factory pipeline",
        "activities": [
            {
                "type": "HDInsightHive",
                "typeProperties": {
                    "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                    "scriptLinkedService": "AzureStorageLinkedService",
                    "defines": {
                        "inputtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/inputdata",
                        "partitionedtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/partitioneddata"
                    }
                },
                "inputs": [
                    {
                        "name": "AzureBlobInput"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobOutput"
                    }
                ],
                "policy": {
                    "concurrency": 1,
                    "retry": 3
                },
                "scheduler": {
                    "frequency": "Month",
                    "interval": 1
                },
                "name": "RunSampleHiveActivity",
                "linkedServiceName": "HDInsightOnDemandLinkedService"
            }
        ],
        "start": "2016-04-01T00:00:00Z",
        "end": "2016-04-02T00:00:00Z",
        "isPaused": false
    }
}
```

Aşağıdaki noktalara dikkat edin:

* Etkinlikler bölümünde **türü****HDInsightHive** olarak ayarlanmış yalnızca bir etkinlik vardır.
* **partitionweblogs.hql** Hive betik dosyası Azure depolama hesabında (scriptLinkedService tarafından belirtilen **AzureStorageLinkedService** adıyla) ve **adfgetstarted** kapsayıcısındaki **betik** klasöründe depolanır.
* Bu `defines` bölüm, kovan komut dosyasına hive yapılandırma değerleri (örn. `${hiveconf:inputtable}` `${hiveconf:partitionedtable}`

**TypeProperties** bölümü her bir dönüştürme etkinliği için farklıdır. Dönüşüm etkinliği için desteklenen tür özellikleri hakkında bilgi edinmek için [Veri dönüştürme etkinlikleri](#data-transformation-activities) tablosundaki dönüşüm etkinliğini tıklatın.

Bu ardışık yapıyı oluşturmanın tam bir bölümü için [Bkz. Öğretici: Hadoop kümesini kullanarak verileri işlemek için ilk ardışık lığınızı oluşturun.](data-factory-build-your-first-pipeline.md)

## <a name="multiple-activities-in-a-pipeline"></a>Bir işlem hattında birden çok etkinlik
Önceki iki örnekte işlem hatları yalnızca bir etkinlik içeriyordu. Bir işlem hattında birden fazla etkinliğiniz olabilir.

Bir ardışık ardışık alanda birden çok etkinliğiniz varsa ve bir etkinliğin çıktısı başka bir etkinliğin girişi değilse, etkinlikleriçin giriş veri dilimleri hazırsa, etkinlikler paralel olarak çalışabilir.

Bir etkinliğin çıktı veri kümesini diğer etkinliğin giriş veri kümesi olarak alarak iki etkinliği zincirleyebilirsiniz. İkinci etkinlik yalnızca ilki başarıyla tamamlandığında yürütülür.

![Aynı boru hattında zincirleme faaliyetleri](./media/data-factory-create-pipelines/chaining-one-pipeline.png)

Bu örnekte, ardışık boru hattının iki faaliyeti vardır: Etkinlik1 ve Etkinlik2. Activity1, Dataset1'i giriş olarak alır ve bir çıktı Dataset2 üretir. Etkinlik, Dataset2'yi girdi olarak alır ve bir çıktı Dataset3 üretir. Activity1 (Dataset2) çıktısı Activity2'nin girişi olduğundan, Etkinlik2 yalnızca Etkinlik başarıyla tamamlandıktan ve Dataset2 dilimini yaptıktan sonra çalışır. Etkinlik1 bazı nedenlerden dolayı başarısız olursa ve Dataset2 dilimini üretmezse, Etkinlik 2 bu dilim için çalışmaz (örneğin: 9 ila 10 AM).

Ayrıca, farklı ardışık hatlar halindeki etkinlikleri zincirleyebilirsiniz.

![İki boru hattında zincirleme faaliyetleri](./media/data-factory-create-pipelines/chaining-two-pipelines.png)

Bu örnekte, Pipeline1'in Dataset1'i girdi olarak alan ve veri kümesi2'yi çıktı olarak üreten tek bir etkinliği vardır. Pipeline2 ayrıca Dataset2'yi girdi olarak ve Dataset3'ü çıktı olarak alan tek bir faaliyete sahiptir.

Daha fazla bilgi için [zamanlama ve yürütme](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline)ye bakın.
## <a name="create-and-monitor-pipelines"></a>Boru hatları oluşturma ve izleme
Bu araçlardan veya SDK'lardan birini kullanarak ardışık hatlar oluşturabilirsiniz.

- Kopyalama Sihirbazı
- Visual Studio
- Azure PowerShell
- Azure Resource Manager şablonu
- REST API
- .NET API’si

Bu araçlardan veya SDK'lardan birini kullanarak ardışık hatlar oluşturmak için adım adım talimatlar için aşağıdaki öğreticilere bakın.

- [Veri dönüştürme etkinliğine sahip işlem hattı oluşturma](data-factory-build-your-first-pipeline.md)
- [Veri hareketi etkinliği içeren bir ardışık hatlar oluşturma](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)

Bir ardışık ardışık hatlar oluşturulduktan/dağıtıldıktan sonra, Azure portal bıçaklarını veya İzleme ve Yönetme Uygulamasını kullanarak ardışık hatlarınızı yönetebilir ve izleyebilirsiniz. Adım adım talimatlar için aşağıdaki konulara bakın.

- [Azure portal bıçaklarını kullanarak boru hatlarını izleyin ve yönetin.](data-factory-monitor-manage-pipelines.md)
- [Monitör ve Uygulama'yı kullanarak boru hatlarını izleme ve yönetme](data-factory-monitor-manage-app.md)

## <a name="onetime-pipeline"></a>Tek seferlik boru hattı
Boru hattı tanımında belirttiğiniz başlangıç ve bitiş saatlerinde düzenli aralıklarla (örneğin: saatlik veya günlük) çalışacak bir ardışık hatlar oluşturabilir ve zamanlayabilirsiniz. Ayrıntılar için Zamanlama etkinliklerine bakın. Ayrıca yalnızca bir kez çalışan bir ardışık hatlar oluşturabilirsiniz. Bunu yapmak için, **pipelineMode** özelliğini aşağıdaki JSON örneğinde gösterildiği gibi **bir defaya** bir kez ayarlarsınız. Bu özelliğin varsayılan değeri **zamanlanır.**

```json
{
    "name": "CopyPipeline",
    "properties": {
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource",
                        "recursive": false
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "InputDataset"
                    }
                ],
                "outputs": [
                    {
                        "name": "OutputDataset"
                    }
                ],
                "name": "CopyActivity-0"
            }
        ],
        "pipelineMode": "OneTime"
    }
}
```

Şunlara dikkat edin:

* Ardışık iş tonu için **başlangıç** ve **bitiş** saatleri belirtilmemiştir.
* Veri Fabrikası değerleri kullanmasa bile, giriş ve çıktı veri kümelerinin **kullanılabilirliği** **(sıklık** ve **aralık)** belirtilir.
* Diyagram görünümü bir kerelik ardışık lıkları göstermez. Bu davranış tasarım gereğidir.
* Tek seferlik ardışık hatlar güncelleştirilemez. Bir kerelik bir ardışık lığı klonlayabilir, yeniden adlandırabilir, özellikleri güncelleştirebilir ve başka bir ardışıklandırma yapabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
- Veri kümeleri hakkında daha fazla bilgi için [bkz.](data-factory-create-datasets.md)
- Boru hatlarının nasıl zamanlanıp yürütüldedildiği hakkında daha fazla bilgi için [Azure Veri Fabrikası makalesinde Zamanlama ve yürütme](data-factory-scheduling-and-execution.md) ye bakın.
