---
title: Azure Servis Kumaş CLI- sfctl örgü kod-paket-log
description: Azure Service Fabric komut satırı arabirimi sfctl hakkında bilgi edinin. Belirli bir kod paketi için günlükleri almak için komutların listesini içerir.
author: jeffj6123
ms.topic: reference
ms.date: 1/16/2020
ms.author: jejarry
ms.openlocfilehash: f8665e75e4c921a3305c9965601e2ee0825c8995
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76906054"
---
# <a name="sfctl-mesh-code-package-log"></a>sfctl mesh code-package-log
Verilen hizmet çoğaltma için belirtilen kod paketinin kapsayıcısının günlüklerini alın.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| get | Konteynırdan günlükleri alır. |

## <a name="sfctl-mesh-code-package-log-get"></a>sfctl örgü kod-paket-günlük olsun
Konteynırdan günlükleri alır.

Hizmet çoğaltma belirtilen kod paketinin kapsayıcı için günlükleri alır.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --uygulama adı --uygulama adı [Gerekli] | Uygulamanın adı. |
| --kod-paket adı [Gerekli] | Hizmetin kod paketinin adı. |
| --çoğaltma adı [Gerekli] | Hizmet Kumaş çoğaltma adı. |
| --hizmet adı [Gerekli] | Hizmetin adı. |
| --kuyruk | Günlüklerin sonundan gösterilen satır sayısı. Varsayılan değer 100'dür. 'tüm' tüm günlükleri göstermek için. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |


## <a name="next-steps"></a>Sonraki adımlar
- Servis Kumaş ı CLI'yi [ayarlayın.](service-fabric-cli.md)
- [Örnek komut dosyalarını](/azure/service-fabric/scripts/sfctl-upgrade-application)kullanarak Service Fabric CLI'yi nasıl kullanacağınızı öğrenin.