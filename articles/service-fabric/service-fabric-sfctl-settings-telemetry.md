---
title: Azure Service Fabric CLı-sfctl ayarları telemetrisi
description: Azure Service Fabric komut satırı arabirimi olan sfctl hakkında bilgi edinin. Sfctl telemetrisini yapılandırmaya yönelik komutların bir listesini içerir.
author: jeffj6123
ms.topic: reference
ms.date: 1/16/2020
ms.author: jejarry
ms.openlocfilehash: ef720a14617b4131474d50875701d0ef27df4151
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2020
ms.locfileid: "86245525"
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

### <a name="arguments"></a>Arguments

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --kapalı | Telemetriyi devre dışı bırakın. |
| --Açık | Telemetriyi açın. Varsayılan değer budur. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Hata Ayıkla | Tüm hata ayıklama günlüklerini göstermek için günlük ayrıntı düzeyini artırın. |
| --Yardım-h | Bu yardım iletisini gösterin ve çıkın. |
| --çıkış-o | Çıkış biçimi.  İzin verilen \: JSON, jsonc, tablo, TSV değerleri.  Varsayılan \: JSON. |
| --sorgu | JMESPath sorgu dizesi. \:Daha fazla bilgi ve örnek için bkz. http//jmespath.org/. |
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
- [Örnek betikleri](./scripts/sfctl-upgrade-application.md)kullanarak Service Fabric CLI 'nın nasıl kullanılacağını öğrenin.
