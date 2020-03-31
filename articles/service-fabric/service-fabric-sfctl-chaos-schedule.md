---
title: Azure Servis Kumaş CLI- sfctl kaos programı
description: Azure Service Fabric komut satırı arabirimi sfctl hakkında bilgi edinin. Kaos zamanlama için komutların listesini içerir.
author: jeffj6123
ms.topic: reference
ms.date: 1/16/2020
ms.author: jejarry
ms.openlocfilehash: 29b365c48e405830e238945f1d94156f477c15b4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76906176"
---
# <a name="sfctl-chaos-schedule"></a>sfctl chaos schedule
Alın ve kaos programını ayarlayın.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| get | Kaos'u ne zaman ve nasıl çalıştırılanın tanımladığı kaos çizelgesini alın. |
| set | Chaos tarafından kullanılan zamanlamayı ayarlayın. |

## <a name="sfctl-chaos-schedule-get"></a>sfctl kaos programı olsun
Kaos'u ne zaman ve nasıl çalıştırılanın tanımladığı kaos çizelgesini alın.

Kaos Zamanlaması'nın sürümünü ve Kaos'un ne zaman ve nasıl çalıştırılabildiğini tanımlayan Kaos Çizelgesi'ni alır.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --zaman ayarı -t | İşlemi saniyeler içinde gerçekleştirmek için sunucu zaman. Bu zaman sonu, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametrenin varsayılan değeri 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-chaos-schedule-set"></a>sfctl kaos programı seti
Chaos tarafından kullanılan zamanlamayı ayarlayın.

Kaos, çalıştırmaları Kaos Çizelgesi'ne göre otomatik olarak zamanlar. Sağlanan sürüm sunucudaki sürümle eşleşirse Kaos Çizelgesi güncelleştirilir. Kaos Çizelgesi güncelleştirilirken, sunucudaki sürüm 1 ile yükseltilir. Sunucudaki sürüm, çok sayıdana ulaştıktan sonra 0'a geri döner. Bu arama yapıldığında Kaos çalışıyorsa, arama başarısız olur.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --kaos-parametreler-sözlük | JSON, Jobs tarafından kullanılacak ChaosParameters için dize adlarının eşlemini temsil eden kodlanmış liste. |
| --son kullanma tarihi-utc | Kaos'u zamanlamak için Zamanlama'yı kullanmanın ne zaman durdurulması için tarih ve saat.  Varsayılan\: 9999-12-31T23\:\:59 59.999Z. |
| --işler | JSON, ChaosScheduleJobs'un ne zaman çalıştırılmasını ve Chaos'u hangi parametrelerle çalıştıracakparametrelerle yönetileceklerini gösteren listesini kodladı. |
| --başlangıç-tarih-utc | Karmaşayı zamanlamak için Zamanlama'yı kullanmaya başlamatarihi ve saati.  Varsayılan\: 1601-01-01T00\:\:00 00.000Z. |
| --zaman ayarı -t | Varsayılan\: 60. |
| --sürüm | Zamanlama'nın sürüm numarası. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

### <a name="examples"></a>Örnekler

Aşağıdaki komut, 2016-01-01'de başlayan ve 2038-01-01 tarihinde sona eren ve haftanın 7 günü Kaos'u çalıştıran bir zamanlama (geçerli zamanlamanın sürüm 0'a sahip olduğunu varsayarak) ayarlar.
Kaos o zaman için küme üzerinde zamanlanmış olacak.
```
sfctl chaos schedule set --version 0 --start-date-utc "2016-01-01T00:00:00.000Z" --expiry-date-utc "2038-01-01T00:00:00.000Z"
    --chaos-parameters-dictionary
    [
    {
        "Key":"adhoc",
        "Value":{
            "MaxConcurrentFaults":3,
            "EnableMoveReplicaFaults":true,
            "ChaosTargetFilter":{
                "NodeTypeInclusionList":[
                "N0010Ref",
                "N0020Ref",
                "N0030Ref",
                "N0040Ref",
                "N0050Ref"
                ]
            },
            "MaxClusterStabilizationTimeoutInSeconds":60,
            "WaitTimeBetweenIterationsInSeconds":15,
            "WaitTimeBetweenFaultsInSeconds":30,
            "TimeToRunInSeconds":"600",
            "Context":{
                "Map":{
                "test":"value"
                }
            },
            "ClusterHealthPolicy":{
                "MaxPercentUnhealthyNodes":0,
                "ConsiderWarningAsError":true,
                "MaxPercentUnhealthyApplications":0
            }
        }
    }
    ]
    --jobs
    [
    {
        "ChaosParameters":"adhoc",
        "Days":{
            "Sunday":true,
            "Monday":true,
            "Tuesday":true,
            "Wednesday":true,
            "Thursday":true,
            "Friday":true,
            "Saturday":true
        },
        "Times":[
            {
                "StartTime":{
                "Hour":0,
                "Minute":0
                },
                "EndTime":{
                "Hour":23,
                "Minute":59
                }
            }
        ]
    }
    ]
```



## <a name="next-steps"></a>Sonraki adımlar
- Servis Kumaş ı CLI'yi [ayarlayın.](service-fabric-cli.md)
- [Örnek komut dosyalarını](/azure/service-fabric/scripts/sfctl-upgrade-application)kullanarak Service Fabric CLI'yi nasıl kullanacağınızı öğrenin.
