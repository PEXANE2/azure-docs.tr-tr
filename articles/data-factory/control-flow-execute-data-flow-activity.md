---
title: Veri Akışı etkinliği
description: Bir veri fabrikası boru hattı içinden veri akışları yürütmek için nasıl.
services: data-factory
documentationcenter: ''
author: kromerm
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.author: makromer
ms.date: 03/16/2020
ms.openlocfilehash: 32088dd712cd0c70fc01de48add17a0b6a828dc8
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81415325"
---
# <a name="data-flow-activity-in-azure-data-factory"></a>Azure Veri Fabrikası'nda Veri Akışı etkinliği

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Veri akışlarını eşleme yoluyla verileri dönüştürmek ve taşımak için Veri Akışı etkinliğini kullanın. Veri akışlarına yeniyseniz, [Bkz. Veri Akışı Haritalama](concepts-data-flow-overview.md)

## <a name="syntax"></a>Sözdizimi

```json
{
    "name": "MyDataFlowActivity",
    "type": "ExecuteDataFlow",
    "typeProperties": {
      "dataflow": {
         "referenceName": "MyDataFlow",
         "type": "DataFlowReference"
      },
      "compute": {
         "coreCount": 8,
         "computeType": "General"
      },
      "staging": {
          "linkedService": {
              "referenceName": "MyStagingLinkedService",
              "type": "LinkedServiceReference"
          },
          "folderPath": "my-container/my-folder"
      },
      "integrationRuntime": {
          "referenceName": "MyDataFlowIntegrationRuntime",
          "type": "IntegrationRuntimeReference"
      }
}

```

## <a name="type-properties"></a>Tür özellikleri

Özellik | Açıklama | İzin verilen değerler | Gerekli
-------- | ----------- | -------------- | --------
Dataflow | Yürütülen Veri Akışı na başvuru | DataFlowReference | Evet
entegrasyonRuntime | Veri akışının çalıştığı bilgi işlem ortamı. Belirtilmemişse, Azure Tümleştirme çalışma süresi otomatik olarak çözülür | IntegrationRuntimeReference | Hayır
compute.coreCount | Kıvılcım kümesinde kullanılan çekirdek sayısı. Yalnızca otomatik çözümleme Azure Tümleştirme çalışma süresi kullanılırsa belirtilebilir | 8, 16, 32, 48, 80, 144, 272 | Hayır
compute.computeType | Kıvılcım kümesinde kullanılan işlem türü. Yalnızca otomatik çözümleme Azure Tümleştirme çalışma süresi kullanılırsa belirtilebilir | "Genel", "ComputeOptimized", "MemoryOptimized" | Hayır
evreleme.linkedService | Bir SQL DW kaynağı veya lavabo kullanıyorsanız, PolyBase evreleme için kullanılan depolama hesabı | LinkedServiceReference | Yalnızca veri akışı bir SQL DW'yi okur veya yazarsa
evreleme.folderPath | Bir SQL DW kaynağı veya lavabo kullanıyorsanız, Blob depolama hesabındaki klasör yolu PolyBase evreleme için kullanılır | Dize | Yalnızca veri akışı bir SQL DW'yi okur veya yazarsa

![Veri Akışını Yürütme](media/data-flow/activity-data-flow.png "Veri Akışını Yürütme")

### <a name="dynamically-size-data-flow-compute-at-runtime"></a>Çalışma zamanında dinamik boyut veri akışı bilgi işlem

Çekirdek Sayısı ve İşlem Türü özellikleri, gelen kaynak verilerinizin çalışma zamanındaki boyutuna göre ayarlanacak şekilde dinamik olarak ayarlanabilir. Kaynak veri kümesi verilerinin boyutunu bulmak için Arama veya Meta Verilerini Al gibi ardışık iş aktarım etkinliklerini kullanın. Ardından, Veri Akışı etkinlik özelliklerinde Dinamik İçerik Ekle'yi kullanın.

![Dinamik Veri Akışı](media/data-flow/dyna1.png "Dinamik veri akışı")

[İşte bu tekniği açıklayan kısa bir video eğitimi](https://www.youtube.com/watch?v=jWSkJdtiJNM)

### <a name="data-flow-integration-runtime"></a>Veri Akışı tümleştirme çalışma süresi

Veri Akışı etkinlik yürütmeniz için hangi Tümleştirme Runtime'ını kullanacağınızı seçin. Varsayılan olarak, Veri Fabrikası dört alt çekirdekve yaşamak için zaman (TTL) ile otomatik çözümleme Azure Tümleştirme çalışma süresini kullanır. Bu IR genel bir amaç hesaplama türüne sahiptir ve fabrikanızla aynı bölgede çalışır. Veri akışı etkinliği yürütmeniz için belirli bölgeleri, bilgi işlem türünü, çekirdek sayılarını ve TTL'yi tanımlayan kendi Azure Tümleştirme Run zamanlarınızı oluşturabilirsiniz.

Pipeline yürütmeleri için küme, yürütme başlamadan önce başlatılması birkaç dakika süren bir iş kümesidir. TTL belirtilmemişse, her ardışık işlem de bu başlatma süresi gereklidir. Bir TTL belirtirseniz, sıcak küme havuzu son yürütmeden sonra belirtilen süre boyunca etkin kalır ve bu da daha kısa başlangıç süreleri ile sonuçlanır. Örneğin, 60 dakikalık bir TTL'niz varsa ve üzerinde saatte bir veri akışı çalıştırılırsa, küme havuzu etkin kalır. Daha fazla bilgi için Azure [tümleştirme çalışma zamanı'na](concepts-integration-runtime.md)bakın.

![Azure Tümleştirme Çalışma Süresi](media/data-flow/ir-new.png "Azure Tümleştirme Çalışma Süresi")

> [!NOTE]
> Veri Akışı etkinliğindeki Tümleştirme Çalışma Zamanı seçimi yalnızca ardışık alanınızın *tetiklenen yürütmeleri* için geçerlidir. Veri ayıklama ile ardışık alma hata ayıklama, hata ayıklama oturumunda belirtilen küme üzerinde çalışır.

### <a name="polybase"></a>PolyBase

Bir Azure SQL Veri Ambarı'nı lavabo veya kaynak olarak kullanıyorsanız, PolyBase toplu yükünüz için bir hazırlama konumu seçmeniz gerekir. PolyBase, verileri satır satır yüklemek yerine toplu olarak toplu yüklemeye olanak tanır. PolyBase, SQL DW'ye yükleme süresini önemli ölçüde azaltır.

## <a name="parameterizing-data-flows"></a>Veri Akışlarını Parametreleme

### <a name="parameterized-datasets"></a>Parametreli veri kümeleri

Veri akışınız parametreli veri kümelerini kullanıyorsa, **Ayarlar** sekmesindeki parametre değerlerini ayarlayın.

![Veri Akış Parametrelerini Çalıştırma](media/data-flow/params.png "Parametreler")

### <a name="parameterized-data-flows"></a>Parametreli veri akışları

Veri akışınız parametrelendirilmişse, **Parametreler** sekmesindeki veri akışı parametrelerinin dinamik değerlerini ayarlayın. Dinamik veya gerçek parametre değerleri atamak için ADF ardışık ifade dilini (yalnızca String türleri için) veya veri akışı ifade dilini kullanabilirsiniz. Daha fazla bilgi için [Bkz. Veri Akış Parametreleri.](parameters-data-flow.md)

![Veri Akış Parametre Örneği Yürütme](media/data-flow/parameter-example.png "Parametre Örneği")

### <a name="parameterized-compute-properties"></a>Parametreli işlem özellikleri.

Azure Tümleştirme çalışma zamanını otomatik çözümleme ve compute.coreCount ve compute.computeType değerlerini belirtirseniz, çekirdek sayımı veya işlem türünü parametrenize alabilirsiniz.

![Veri Akış Parametre Örneği Yürütme](media/data-flow/parameterize-compute.png "Parametre Örneği")

## <a name="pipeline-debug-of-data-flow-activity"></a>Veri Akışı etkinliğinin ardışık hatlar ayıklama

Veri Akışı etkinliği yle bir hata ayıklama ardışık etki veri hattı çalıştırmak için, üst çubuğunda **Veri Akışı Hata Ayıklama** kaydırıcısı üzerinden veri akışı hata ayıklama modunu açmanız gerekir. Hata ayıklama modu, veri akışını etkin bir Spark kümesine karşı çalıştırmanızı sağlar. Daha fazla bilgi için [Hata Ayıklama Modu'na](concepts-data-flow-debug-mode.md)bakın.

![Hata ayıklama düğmesi](media/data-flow/debugbutton.png "Hata ayıklama düğmesi")

Hata ayıklama ardışık işlem, Veri Akışı etkinlik ayarlarında belirtilen tümleştirme çalışma zamanı ortamına değil, etkin hata ayıklama kümesine karşı çalışır. Hata ayıklama modunu başlatırken hata ayıklama ortamını seçebilirsiniz.

## <a name="monitoring-the-data-flow-activity"></a>Veri Akışı etkinliğini izleme

Veri Akışı etkinliği, bölümleme, aşama süresi ve veri soy bilgilerini görüntüleyebileceğiniz özel bir izleme deneyimine sahiptir. **Eylemler**altında gözlük simgesi üzerinden izleme bölmesini açın. Daha fazla bilgi için [bkz.](concepts-data-flow-monitoring.md)

### <a name="use-data-flow-activity-results-in-a-subsequent-activity"></a>Sonraki bir etkinlikte Veri Akışı etkinlik sonuçlarını kullanma

Veri akışı etkinliği, her bir lavaboya yazılan satır sayısıve her kaynaktan okunan satır sayısıyla ilgili ölçümleri doğuruyor. Bu sonuçlar, etkinlik `output` çalıştırma sonucu bölümünde döndürülür. Döndürülen ölçümler aşağıdaki json biçimindedir.

``` json
{
    "runStatus": {
        "metrics": {
            "<your sink name1>": {
                "rowsWritten": <number of rows written>,
                "sinkProcessingTime": <sink processing time in ms>,
                "sources": {
                    "<your source name1>": {
                        "rowsRead": <number of rows read>
                    },
                    "<your source name2>": {
                        "rowsRead": <number of rows read>
                    },
                    ...
                }
            },
            "<your sink name2>": {
                ...
            },
            ...
        }
    }
}
```

Örneğin, 'dataflowActivity' adlı bir etkinlikte 'sink1' adlı bir lavaboya yazılan satır `@activity('dataflowActivity').output.runStatus.metrics.sink1.rowsWritten`sayısına ulaşmak için.

Bu lavaboda kullanılan 'source1' adlı bir kaynaktan okunan satır `@activity('dataflowActivity').output.runStatus.metrics.sink1.sources.source1.rowsRead`sayısını almak için.

> [!NOTE]
> Bir lavaboda sıfır satır yazılmışsa, ölçümlerde gösterilmez. Varoluş `contains` işlevi kullanılarak doğrulanabilir. Örneğin, `contains(activity('dataflowActivity').output.runStatus.metrics, 'sink1')` herhangi bir satır lavabo1 için yazılmış olup olmadığını denetler.

## <a name="next-steps"></a>Sonraki adımlar

Veri Fabrikası tarafından desteklenen denetim akışı etkinliklerine bakın: 

- [If Condition Etkinliği](control-flow-if-condition-activity.md)
- [İşlem Hattı Çalıştırma Etkinliği](control-flow-execute-pipeline-activity.md)
- [Her Bir Etkinlik için](control-flow-for-each-activity.md)
- [Meta Veri Alma Etkinliği](control-flow-get-metadata-activity.md)
- [Arama Etkinliği](control-flow-lookup-activity.md)
- [Web Etkinliği](control-flow-web-activity.md)
- [Until Etkinliği](control-flow-until-activity.md)
