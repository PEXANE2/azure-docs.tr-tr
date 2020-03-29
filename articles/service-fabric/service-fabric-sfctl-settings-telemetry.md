---
title: Azure Servis Kumaş CLI- sfctl ayarları telemetri
description: Azure Service Fabric komut satırı arabirimi sfctl hakkında bilgi edinin. sfctl telemetri yapılandırmak için komutların listesini içerir.
author: jeffj6123
ms.topic: reference
ms.date: 1/16/2020
ms.author: jejarry
ms.openlocfilehash: 6af5fa944ef399756f9e890ddd77a7f5f32e2bfb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76903021"
---
# <a name="sfctl-settings-telemetry"></a>sfctl settings telemetry
Telemetri ayarlarını bu sfctl örneğine yerel olarak yapılandırın.

Sfctl telemetri parametreleri veya değerleri, sfctl sürümü, işletim sistemi türü, python sürümü, komutun başarı veya hatası olmadan komut adı toplar, hata iletisi döndürülür.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| set-telemetri | Telemetriyi açın veya kapatın. |

## <a name="sfctl-settings-telemetry-set-telemetry"></a>sfctl ayarları telemetri set-telemetri
Telemetriyi açın veya kapatın.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --kapalı | Telemetriyi kapatın. |
| --üzerinde | Telemetriyi aç. Varsayılan değer budur. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

### <a name="examples"></a>Örnekler

Telemetriyi kapatın.

```
sfctl settings telemetry set_telemetry --off
```

Telemetriyi aç.

```
sfctl settings telemetry set_telemetry --on
```


## <a name="next-steps"></a>Sonraki adımlar
- Servis Kumaş ı CLI'yi [ayarlayın.](service-fabric-cli.md)
- [Örnek komut dosyalarını](/azure/service-fabric/scripts/sfctl-upgrade-application)kullanarak Service Fabric CLI'yi nasıl kullanacağınızı öğrenin.