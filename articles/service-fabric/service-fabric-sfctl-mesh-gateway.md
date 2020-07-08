---
title: Azure Service Fabric CLı-sfctl ağı ağ geçidi
description: Azure Service Fabric komut satırı arabirimi olan sfctl hakkında bilgi edinin. Service Fabric ağ geçidi kaynaklarını alma ve silmeye yönelik komutların bir listesini içerir.
author: jeffj6123
ms.topic: reference
ms.date: 1/16/2020
ms.author: jejarry
ms.openlocfilehash: 3e398ff8a1a0a28fd9d0650a7c9d18ba04817d33
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "76906008"
---
# <a name="sfctl-mesh-gateway"></a>sfctl mesh gateway
Ağ Geçidi kaynaklarını alın ve silin.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| delete | Ağ Geçidi kaynağını siler. |
| list | Tüm ağ geçidi kaynaklarını listeler. |
| göster | Belirtilen ada sahip ağ geçidi kaynağını alır. |

## <a name="sfctl-mesh-gateway-delete"></a>sfctl ağı ağ geçidi silme
Ağ Geçidi kaynağını siler.

Ad tarafından tanımlanan ağ geçidi kaynağını siler.

### <a name="arguments"></a>Arguments

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Name-n [gerekli] | Ağ Geçidi kaynağının adı. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Hata Ayıkla | Tüm hata ayıklama günlüklerini göstermek için günlük ayrıntı düzeyini artırın. |
| --Yardım-h | Bu yardım iletisini gösterin ve çıkın. |
| --çıkış-o | Çıkış biçimi.  İzin verilen \: JSON, jsonc, tablo, TSV değerleri.  Varsayılan \: JSON. |
| --sorgu | JMESPath sorgu dizesi. \:Daha fazla bilgi ve örnek için bkz. http//jmespath.org/. |
| --ayrıntılı | Günlük ayrıntı düzeyini artırın. Tam hata ayıklama günlükleri için--Debug kullanın. |

## <a name="sfctl-mesh-gateway-list"></a>sfctl ağı ağ geçidi listesi
Tüm ağ geçidi kaynaklarını listeler.

Belirli bir kaynak grubundaki tüm ağ geçidi kaynaklarıyla ilgili bilgileri alır. Bilgiler, ağ geçidinin açıklamasını ve diğer özelliklerini içerir.

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Hata Ayıkla | Tüm hata ayıklama günlüklerini göstermek için günlük ayrıntı düzeyini artırın. |
| --Yardım-h | Bu yardım iletisini gösterin ve çıkın. |
| --çıkış-o | Çıkış biçimi.  İzin verilen \: JSON, jsonc, tablo, TSV değerleri.  Varsayılan \: JSON. |
| --sorgu | JMESPath sorgu dizesi. \:Daha fazla bilgi ve örnek için bkz. http//jmespath.org/. |
| --ayrıntılı | Günlük ayrıntı düzeyini artırın. Tam hata ayıklama günlükleri için--Debug kullanın. |

## <a name="sfctl-mesh-gateway-show"></a>sfctl ağı ağ geçidi Show
Belirtilen ada sahip ağ geçidi kaynağını alır.

Belirtilen ada sahip ağ geçidi kaynağıyla ilgili bilgileri alır. Bilgiler, ağ geçidinin açıklamasını ve diğer özelliklerini içerir.

### <a name="arguments"></a>Arguments

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Name-n [gerekli] | Ağ Geçidi kaynağının adı. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Hata Ayıkla | Tüm hata ayıklama günlüklerini göstermek için günlük ayrıntı düzeyini artırın. |
| --Yardım-h | Bu yardım iletisini gösterin ve çıkın. |
| --çıkış-o | Çıkış biçimi.  İzin verilen \: JSON, jsonc, tablo, TSV değerleri.  Varsayılan \: JSON. |
| --sorgu | JMESPath sorgu dizesi. \:Daha fazla bilgi ve örnek için bkz. http//jmespath.org/. |
| --ayrıntılı | Günlük ayrıntı düzeyini artırın. Tam hata ayıklama günlükleri için--Debug kullanın. |


## <a name="next-steps"></a>Sonraki adımlar
- Service Fabric CLı 'yi [ayarlayın](service-fabric-cli.md) .
- [Örnek betikleri](/azure/service-fabric/scripts/sfctl-upgrade-application)kullanarak Service Fabric CLI 'nın nasıl kullanılacağını öğrenin.