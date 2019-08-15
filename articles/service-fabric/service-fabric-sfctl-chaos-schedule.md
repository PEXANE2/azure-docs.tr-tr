---
title: Azure Service Fabric CLı-sfctl Chaos zamanlaması | Microsoft Docs
description: CLı sfctl Chaos zamanlama komutlarını Service Fabric açıklar.
services: service-fabric
documentationcenter: na
author: Christina-Kang
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 12/06/2018
ms.author: bikang
ms.openlocfilehash: 1664978110b7c700906cbf4e6c80806ac70f1f05
ms.sourcegitcommit: 18061d0ea18ce2c2ac10652685323c6728fe8d5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/15/2019
ms.locfileid: "69036527"
---
# <a name="sfctl-chaos-schedule"></a>sfctl chaos schedule
Chaos zamanlamasını alın ve ayarlayın.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| Al | Chaos 'ın ne zaman ve nasıl çalıştırılacağını tanımlayan Chaos zamanlamasını alın. |
| set | Chaos tarafından kullanılan zamanlamayı ayarlayın. |

## <a name="sfctl-chaos-schedule-get"></a>sfctl Chaos zamanlaması al
Chaos 'ın ne zaman ve nasıl çalıştırılacağını tanımlayan Chaos zamanlamasını alın.

Kullanımdaki Chaos zamanlamasının sürümünü ve Chaos 'ın ne zaman ve nasıl çalıştırılacağını tanımlayan Chaos zamanlamasını alır.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --timeout-t | Saniye cinsinden sunucu zaman aşımı.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Hata Ayıkla | Tüm hata ayıklama günlüklerini göstermek için günlük ayrıntı düzeyini artırın. |
| --Yardım-h | Bu yardım iletisini gösterin ve çıkın. |
| --çıkış-o | Çıkış biçimi.  İzin verilen\: JSON, jsonc, tablo, TSV değerleri.  Varsayılan\: JSON. |
| --sorgu | JMESPath sorgu dizesi. Daha fazla\:bilgi ve örnek için bkz. http//jmespath.org/. |
| --ayrıntılı | Günlük ayrıntı düzeyini artırın. Tam hata ayıklama günlükleri için--Debug kullanın. |

## <a name="sfctl-chaos-schedule-set"></a>sfctl Chaos zamanlama kümesi
Chaos tarafından kullanılan zamanlamayı ayarlayın.

Chaos, Chaos zamanlaması temelinde otomatik olarak çalıştırmalar zamanlar. Belirtilen giriş zamanlamadaki sürümün sunucudaki Chaos zamanlaması sürümü ile eşleşmesi gerekir. Belirtilen sürüm, sunucudaki sürümle eşleşmezse, Chaos zamanlaması güncellenmez. Belirtilen sürüm, sunucudaki sürümle eşleşiyorsa, Chaos zamanlaması güncelleştirilir ve sunucudaki Chaos zamanlamasının sürümü bir artırılır ve 2.147.483.647 sonrasında 0 ' a geri sarar. Bu çağrı yapıldığında Chaos çalışıyorsa, çağrı başarısız olur.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Chaos-Parameters-sözlük | Iş tarafından kullanılacak bir dize adları ile ChaosParameters eşlemesini temsil eden JSON kodlu liste. |
| --süre sonu-tarih-UTC | Chaos zamanlamak için zamanlamanın ne zaman durdurulacağını durdurulacağı tarih ve saat.  Varsayılan\: 9999-12-31t23\:59\:59.999 z. |
| --işler | Chaos 'ın ne zaman çalıştırılacağını temsil eden ve ile Chaos 'in hangi parametreleri çalıştıracağınızı temsil eden JSON kodlu bir liste. |
| --başlangıç-tarih-UTC | Chaos zamanlamak için zamanlamayı kullanmanın bitiş tarihi ve saati.  Varsayılan\: 1601-01-01T00\:00\:00.000 z. |
| --timeout-t | Saniye cinsinden sunucu zaman aşımı.  Varsayılan\: 60. |
| --sürüm | Zamanlamanın sürüm numarası. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Hata Ayıkla | Tüm hata ayıklama günlüklerini göstermek için günlük ayrıntı düzeyini artırın. |
| --Yardım-h | Bu yardım iletisini gösterin ve çıkın. |
| --çıkış-o | Çıkış biçimi.  İzin verilen\: JSON, jsonc, tablo, TSV değerleri.  Varsayılan\: JSON. |
| --sorgu | JMESPath sorgu dizesi. Daha fazla\:bilgi ve örnek için bkz. http//jmespath.org/. |
| --ayrıntılı | Günlük ayrıntı düzeyini artırın. Tam hata ayıklama günlükleri için--Debug kullanın. |

### <a name="examples"></a>Örnekler

Aşağıdaki komut, bir zamanlamayı (geçerli zamanlamanın 0 ' dır), 2016-01-01 tarihinde başlayan ve gün 24 saat, haftanın 7 günü çalışan 2038-01-01 tarihinde sona erecek şekilde ayarlar. Chaos bu süre için kümede zamanlanır.

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


## <a name="next-steps"></a>Sonraki adımlar
- Service Fabric CLı 'yi [ayarlayın](service-fabric-cli.md) .
- [Örnek betikleri](/azure/service-fabric/scripts/sfctl-upgrade-application)kullanarak Service Fabric CLI 'nın nasıl kullanılacağını öğrenin.
