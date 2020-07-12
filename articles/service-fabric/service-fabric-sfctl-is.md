---
title: Azure Service Fabric CLı-sfctl
description: Azure Service Fabric komut satırı arabirimi olan sfctl hakkında bilgi edinin. Altyapıyı yönetmek için komutların bir listesini içerir.
author: jeffj6123
ms.topic: reference
ms.date: 1/16/2020
ms.author: jejarry
ms.openlocfilehash: 349f70c32ea4ebb4559f053d5ef05b4b37b6480f
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2020
ms.locfileid: "86260367"
---
# <a name="sfctl-is"></a>sfctl is
Altyapı hizmetine komutları sorgulayın ve gönderin.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| command | Verilen altyapı hizmeti örneğinde bir yönetim komutu çağırır. |
| sorgu | Verilen altyapı hizmeti örneğinde salt okunurdur bir sorgu çağırır. |

## <a name="sfctl-is-command"></a>sfctl komutu
Verilen altyapı hizmeti örneğinde bir yönetim komutu çağırır.

Altyapı hizmeti 'nin yapılandırılmış bir veya daha fazla örneği olan kümeler için bu API, altyapı hizmeti 'nin belirli bir örneğine altyapıya özgü komutlar göndermek için bir yol sağlar. Kullanılabilir komutlar ve bunlara karşılık gelen yanıt biçimleri, kümenin üzerinde çalıştığı altyapıya bağlı olarak değişiklik gösterir. Bu API Service Fabric platformunu destekler; doğrudan kodunuzdan kullanılması amaçlıyordu.

### <a name="arguments"></a>Arguments

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --komut [gerekli] | Çağrılacak komutun metni. Komutun içeriği altyapıya özgüdür. |
| --hizmet kimliği | Altyapı hizmetinin kimliği. <br><br> Bu, altyapı hizmetinin ' Fabric \: ' URI düzeni olmadan tam adıdır. Bu parametre yalnızca, çalışan bir altyapı hizmeti örneğine sahip olan küme için gereklidir. |
| --timeout-t | Varsayılan \: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Hata Ayıkla | Tüm hata ayıklama günlüklerini göstermek için günlük ayrıntı düzeyini artırın. |
| --Yardım-h | Bu yardım iletisini gösterin ve çıkın. |
| --çıkış-o | Çıkış biçimi.  İzin verilen \: JSON, jsonc, tablo, TSV değerleri.  Varsayılan \: JSON. |
| --sorgu | JMESPath sorgu dizesi. \:Daha fazla bilgi ve örnek için bkz. http//jmespath.org/. |
| --ayrıntılı | Günlük ayrıntı düzeyini artırın. Tam hata ayıklama günlükleri için--Debug kullanın. |

## <a name="sfctl-is-query"></a>sfctl sorgu
Verilen altyapı hizmeti örneğinde salt okunurdur bir sorgu çağırır.

Altyapı hizmeti 'nin yapılandırılmış bir veya daha fazla örneği olan kümeler için bu API, altyapı hizmeti 'nin belirli bir örneğine altyapıya özgü sorgular göndermek için bir yol sağlar. Kullanılabilir komutlar ve bunlara karşılık gelen yanıt biçimleri, kümenin üzerinde çalıştığı altyapıya bağlı olarak değişiklik gösterir. Bu API Service Fabric platformunu destekler; doğrudan kodunuzdan kullanılması amaçlıyordu.

### <a name="arguments"></a>Arguments

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --komut [gerekli] | Çağrılacak komutun metni. Komutun içeriği altyapıya özgüdür. |
| --hizmet kimliği | Altyapı hizmetinin kimliği. <br><br> Bu, altyapı hizmetinin ' Fabric \: ' URI düzeni olmadan tam adıdır. Bu parametre yalnızca, çalışan bir altyapı hizmeti örneğine sahip olan küme için gereklidir. |
| --timeout-t | Varsayılan \: 60. |

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
