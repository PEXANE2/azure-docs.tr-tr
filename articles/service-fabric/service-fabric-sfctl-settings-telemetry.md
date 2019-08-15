---
title: Azure Service Fabric CLı-sfctl ayarları telemetrisi | Microsoft Docs
description: Service Fabric CLı sfctl ayarları telemetri komutlarını açıklar.
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
ms.openlocfilehash: cf5ebbeb4d9b4757e0c55eeb1a9268065efb2c7c
ms.sourcegitcommit: 18061d0ea18ce2c2ac10652685323c6728fe8d5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/15/2019
ms.locfileid: "69035198"
---
# <a name="sfctl-settings-telemetry"></a>sfctl settings telemetry
Bu sfctl örneğine yerelde telemetri ayarlarını yapılandırın.

Sfctl telemetrisi parametresiz komut adı ya da değerleri, Sfctl sürümü, işletim sistemi türü, Python sürümü, komutun başarısı veya başarısızlığı, döndürülen hata iletisi.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| Set-telemetri | Telemetriyi açın veya kapatın. |

## <a name="sfctl-settings-telemetry-set-telemetry"></a>sfctl ayarları telemetri kümesi-telemetri
Telemetriyi açın veya kapatın.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --kapalı | Telemetriyi devre dışı bırakın. |
| --Açık | Telemetriyi açın. Varsayılan değer budur. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Hata Ayıkla | Tüm hata ayıklama günlüklerini göstermek için günlük ayrıntı düzeyini artırın. |
| --Yardım-h | Bu yardım iletisini gösterin ve çıkın. |
| --çıkış-o | Çıkış biçimi.  İzin verilen\: JSON, jsonc, tablo, TSV değerleri.  Varsayılan\: JSON. |
| --sorgu | JMESPath sorgu dizesi. Daha fazla\:bilgi ve örnek için bkz. http//jmespath.org/. |
| --ayrıntılı | Günlük ayrıntı düzeyini artırın. Tam hata ayıklama günlükleri için--Debug kullanın. |

### <a name="examples"></a>Örnekler

Telemetriyi devre dışı bırakın.

```
sfctl settings telemetry set_telemetry --off
```

Telemetriyi açın.

```
sfctl settings telemetry set_telemetry --on
```


## <a name="next-steps"></a>Sonraki adımlar
- Service Fabric CLı 'yi [ayarlayın](service-fabric-cli.md) .
- [Örnek betikleri](/azure/service-fabric/scripts/sfctl-upgrade-application)kullanarak Service Fabric CLI 'nın nasıl kullanılacağını öğrenin.