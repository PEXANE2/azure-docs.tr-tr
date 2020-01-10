---
title: Azure Service Fabric CLı-sfctl ayarları telemetrisi
description: Azure Service Fabric komut satırı arabirimi olan sfctl hakkında bilgi edinin. Sfctl telemetrisini yapılandırmaya yönelik komutların bir listesini içerir.
author: jeffj6123
ms.topic: reference
ms.date: 9/17/2019
ms.author: jejarry
ms.openlocfilehash: cdb4a44c8f19b31c164e2ba3ea5e16b7a09e743e
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2020
ms.locfileid: "75645285"
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
| --çıkış-o | Çıkış biçimi.  İzin verilen değerler JSON, jsonc, tablo, TSV\:.  JSON\: varsayılan. |
| --sorgu | JMESPath sorgu dizesi. Daha fazla bilgi ve örnek için bkz. http\://jmespath.org/. |
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