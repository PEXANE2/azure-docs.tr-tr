---
title: Azure Machine Learning ardışık hatlarını çalıştırma
description: Azure Machine Learning ardışık alanlarınızı Azure Veri Fabrikası ardışık hatlarınızda nasıl çalıştırılacınız öğrenin.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.author: daperlov
author: djpmsft
manager: anandsub
ms.date: 10/10/2019
ms.openlocfilehash: b54504cf8ca7b32bf14bd4b7e0c561ffd56d4098
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76155172"
---
# <a name="execute-azure-machine-learning-pipelines-in-azure-data-factory"></a>Azure Veri Fabrikası'nda Azure Machine Learning ardışık hatlarını çalıştırma

Azure Machine Learning ardışık hatlarınızı Azure Veri Fabrikası ardışık hatlarınızda bir adım olarak çalıştırın. Machine Learning Execute Pipeline etkinliği, olası kredi varsayılanlarını tanımlama, duyarlılığı belirleme ve müşteri davranış modellerini çözümleme gibi toplu tahmin senaryolarına olanak tanır.

Aşağıdaki videoda bu özelliğin altı dakikalık tanıtımı ve gösterimi yer almaktadır.

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/How-to-execute-Azure-Machine-Learning-service-pipelines-in-Azure-Data-Factory/player]

## <a name="syntax"></a>Sözdizimi

```json
{
    "name": "Machine Learning Execute Pipeline",
    "type": "AzureMLExecutePipeline",
    "linkedServiceName": {
        "referenceName": "AzureMLService",
        "type": "LinkedServiceReference"
    },
    "typeProperties": {
        "mlPipelineId": "machine learning pipeline ID",
        "experimentName": "experimentName",
        "mlPipelineParameters": {
            "mlParameterName": "mlParameterValue"
        }
    }
}

```

## <a name="type-properties"></a>Tür özellikleri

Özellik | Açıklama | İzin verilen değerler | Gerekli
-------- | ----------- | -------------- | --------
ad | Boru hattındaki etkinliğin adı | Dize | Evet
type | Etkinlik türü 'AzureMLExecutePipeline' | Dize | Evet
linkedServiceName | Azure Machine Learning'e Bağlantılı Hizmet | Bağlantılı hizmet başvurusu | Evet
mlPipelineId | Yayınlanan Azure Machine Learning ardışık hattının kimliği | String (veya sonuç string Türü ile ifade) | Evet
denemeAdı | Machine Learning ardışık boru hattının geçmiş deneme adını çalıştırma | String (veya sonuç string Türü ile ifade) | Hayır
mlPipelineParametreler | Anahtar, Yayınlanan Azure Machine Learning ardışık nokta uç noktasına geçirilecek değer çiftleri. Anahtarlar, yayınlanan Machine Learning ardışık boru hattında tanımlanan boru hattı parametrelerinin adlarıyla eşleşmelidir | Anahtar değer çiftleri olan nesne (veya resultType nesnesi ile İfade) | Hayır
mlParentRunId | Üst Azure Machine Learning ardışık çalışma kimliği | String (veya sonuç string Türü ile ifade) | Hayır
continueOnStepFailure | Bir adım başarısız olursa Makine Öğrenimi ardışık işakindeki diğer adımların yürütülmesine devam edilip edilemeyeceği | boole | Hayır

## <a name="next-steps"></a>Sonraki adımlar
Verileri başka şekillerde nasıl dönüştüreceklerini açıklayan aşağıdaki makalelere bakın:

* [Veri Akışı etkinliğini yürütme](control-flow-execute-data-flow-activity.md)
* [U-SQL etkinliği](transform-data-using-data-lake-analytics.md)
* [Kovan aktivitesi](transform-data-using-hadoop-hive.md)
* [Domuz aktivitesi](transform-data-using-hadoop-pig.md)
* [MapAz etkinliği](transform-data-using-hadoop-map-reduce.md)
* [Hadoop Akış etkinliği](transform-data-using-hadoop-streaming.md)
* [Kıvılcım etkinliği](transform-data-using-spark.md)
* [.NET özel etkinliği](transform-data-using-dotnet-custom-activity.md)
* [Depolanan yordam etkinliği](transform-data-using-stored-procedure.md)
