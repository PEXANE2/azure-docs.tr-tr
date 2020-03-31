---
title: Hadoop Kovan etkinliğini kullanarak verileri dönüştürme
description: İsteğe bağlı/kendi HDInsight kümenizde Hive sorgularını çalıştırmak için Bir Azure veri fabrikasındaHive Etkinliğini nasıl kullanabileceğinizi öğrenin.
services: data-factory
ms.service: data-factory
ms.workload: data-services
ms.devlang: na
ms.topic: conceptual
author: nabhishek
ms.author: abnarain
manager: anandsub
ms.custom: seo-lt-2019
ms.date: 01/15/2019
ms.openlocfilehash: b4af3f897a12c71d73962735d3fe68d95f138cef
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74912919"
---
# <a name="transform-data-using-hadoop-hive-activity-in-azure-data-factory"></a>Azure Veri Fabrikası'ndaki Hadoop Hive etkinliğini kullanarak verileri dönüştürme

> [!div class="op_single_selector" title1="Kullandığınız Veri Fabrikası hizmetisürümünü seçin:"]
> * [Sürüm 1](v1/data-factory-hive-activity.md)
> * [Geçerli sürüm](transform-data-using-hadoop-hive.md)

Veri Fabrikası [ardışık işletme](concepts-pipelines-activities.md) işlonundaki HDInsight Hive etkinliği, Kovan sorgularını kendi veya [isteğe bağlı](compute-linked-services.md#azure-hdinsight-on-demand-linked-service) HDInsight [kümenizde](compute-linked-services.md#azure-hdinsight-linked-service) yürütür. Bu makalede, veri dönüşümü ve desteklenen dönüşüm faaliyetlerine genel bir genel bakış sunan [veri dönüştürme etkinlikleri](transform-data.md) makalesi temel almaktadır.

Azure Veri Fabrikası'nda yeniyseniz, [Azure Veri Fabrikası'na Giriş'i](introduction.md) okuyun ve Öğretici'yi yapın: Bu makaleyi okumadan önce verileri [dönüştürün.](tutorial-transform-data-spark-powershell.md) 

## <a name="syntax"></a>Sözdizimi

```json
{
    "name": "Hive Activity",
    "description": "description",
    "type": "HDInsightHive",
    "linkedServiceName": {
        "referenceName": "MyHDInsightLinkedService",
        "type": "LinkedServiceReference"
    },
    "typeProperties": {
        "scriptLinkedService": {
            "referenceName": "MyAzureStorageLinkedService",
            "type": "LinkedServiceReference"
        },
        "scriptPath": "MyAzureStorage\\HiveScripts\\MyHiveSript.hql",
        "getDebugInfo": "Failure",
        "arguments": [
            "SampleHadoopJobArgument1"
        ],
        "defines": {
            "param1": "param1Value"
        }
    }
}
```
## <a name="syntax-details"></a>Sözdizimi ayrıntıları
| Özellik            | Açıklama                                                  | Gerekli |
| ------------------- | ------------------------------------------------------------ | -------- |
| ad                | Etkinliğin adı                                         | Evet      |
| açıklama         | Etkinliğin ne için kullanıldığını açıklayan metin                | Hayır       |
| type                | Kovan Aktivitesi için etkinlik türü HDinsightHive'dir        | Evet      |
| linkedServiceName   | Veri Fabrikası'nda bağlantılı hizmet olarak kayıtlı HDInsight kümesine başvuru. Bu bağlantılı hizmet hakkında bilgi edinmek için [Bkz. Compute bağlantılı hizmetler](compute-linked-services.md) makalesine bakın. | Evet      |
| komut dosyasıLinkedService | Yürütülecek Hive komut dosyasını depolamak için kullanılan Azure Depolama Bağlantılı Hizmetine başvuru. Bu Bağlantılı Hizmeti belirtmezseniz, HDInsight Bağlantılı Hizmeti'nde tanımlanan Azure Depolama Bağlantılı Hizmeti kullanılır. | Hayır       |
| scriptPath          | ScriptLinkedService tarafından başvurulan Azure Depolama'da depolanan komut dosyası dosyasına giden yolu sağlayın. Dosya adı büyük/küçük harf duyarlıdır. | Evet      |
| getDebugInfo        | Günlük dosyalarının, scriptLinkedService tarafından belirtilen HDInsight kümesi (veya) tarafından kullanılan Azure Depolama alanına kopyalandığında belirtir. İzin verilen değerler: Yok, Her Zaman veya Hata. Varsayılan değer: Hiçbiri. | Hayır       |
| Bağımsız değişken           | Hadoop işi için bir dizi bağımsız değişken belirtir. Bağımsız değişkenler her göreve komut satırı bağımsız değişkenleri olarak geçirilir. | Hayır       |
| Tanım -lar             | Hive komut dosyası içinde başvurmak için parametreleri anahtar/değer çiftleri olarak belirtin. | Hayır       |
| Querytimeout        | Zaman önce ödeme değerini sorgula (dakika içinde). HDInsight kümesi Kurumsal Güvenlik Paketi etkinolduğunda uygulanabilir. | Hayır       |

## <a name="next-steps"></a>Sonraki adımlar
Verileri başka şekillerde nasıl dönüştüreceklerini açıklayan aşağıdaki makalelere bakın: 

* [U-SQL etkinliği](transform-data-using-data-lake-analytics.md)
* [Domuz aktivitesi](transform-data-using-hadoop-pig.md)
* [MapAz etkinliği](transform-data-using-hadoop-map-reduce.md)
* [Hadoop Akış etkinliği](transform-data-using-hadoop-streaming.md)
* [Kıvılcım etkinliği](transform-data-using-spark.md)
* [.NET özel etkinliği](transform-data-using-dotnet-custom-activity.md)
* [Makine Öğrenimi Toplu Yürütme Etkinliği](transform-data-using-machine-learning.md)
* [Depolanan yordam etkinliği](transform-data-using-stored-procedure.md)
