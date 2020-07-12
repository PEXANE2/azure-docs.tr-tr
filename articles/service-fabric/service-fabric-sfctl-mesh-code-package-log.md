---
title: Azure Service Fabric CLı-sfctl ağ kodu-paket-günlüğü
description: Azure Service Fabric komut satırı arabirimi olan sfctl hakkında bilgi edinin. Belirtilen kod paketi için günlük alma komutlarının bir listesini içerir.
author: jeffj6123
ms.topic: reference
ms.date: 1/16/2020
ms.author: jejarry
ms.openlocfilehash: 9ac1d85a1a498f9f6fcd0a03f8f819d1cdfcac33
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2020
ms.locfileid: "86257304"
---
# <a name="sfctl-mesh-code-package-log"></a>sfctl mesh code-package-log
Verilen hizmet çoğaltması için belirtilen kod paketinin kapsayıcısına ait günlükleri al.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| get | Kapsayıcıdan günlükleri alır. |

## <a name="sfctl-mesh-code-package-log-get"></a>sfctl ağ kodu-paket-günlük al
Kapsayıcıdan günlükleri alır.

Hizmet çoğaltmasının belirtilen kod paketinin kapsayıcısına ait günlükleri alır.

### <a name="arguments"></a>Arguments

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --App-Name--uygulama-adı [gerekli] | Uygulamanın adı. |
| --Code-Package-Name [gerekli] | Hizmetin kod paketinin adı. |
| --çoğaltma-adı [gerekli] | Service Fabric çoğaltma adı. |
| --hizmet-adı [gerekli] | Hizmetin adı. |
| --Tail | Günlüklerin sonundan gösterilecek satır sayısı. Varsayılan değer 100 ' dir. Tüm günlükleri göstermek için ' All '. |

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
- [Örnek betikleri](./scripts/sfctl-upgrade-application.md)kullanarak Service Fabric CLI 'nın nasıl kullanılacağını öğrenin.
