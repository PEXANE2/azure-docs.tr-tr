---
title: Azure Service Fabric CLı-sfctl
description: Azure Service Fabric komut satırı arabirimi olan sfctl hakkında bilgi edinin. Altyapıyı yönetmek için komutların bir listesini içerir.
author: jeffj6123
ms.topic: reference
ms.date: 1/16/2020
ms.author: jejarry
ms.openlocfilehash: a64aaca97c9df61d795c82d64e6048d6f9ae032f
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "76906081"
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

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --komut [gerekli] | Çağrılacak komutun metni. Komutun içeriği altyapıya özgüdür. |
| --hizmet kimliği | Altyapı hizmetinin kimliği. <br><br> Bu, altyapı hizmetinin ' Fabric\:' URI düzeni olmadan tam adıdır. Bu parametre yalnızca, çalışan bir altyapı hizmeti örneğine sahip olan küme için gereklidir. |
| --timeout-t | Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Hata Ayıkla | Tüm hata ayıklama günlüklerini göstermek için günlük ayrıntı düzeyini artırın. |
| --Yardım-h | Bu yardım iletisini gösterin ve çıkın. |
| --çıkış-o | Çıkış biçimi.  İzin verilen\: JSON, jsonc, tablo, TSV değerleri.  Varsayılan\: JSON. |
| --sorgu | JMESPath sorgu dizesi. Daha fazla\:bilgi ve örnek için bkz. http//jmespath.org/. |
| --ayrıntılı | Günlük ayrıntı düzeyini artırın. Tam hata ayıklama günlükleri için--Debug kullanın. |

## <a name="sfctl-is-query"></a>sfctl sorgu
Verilen altyapı hizmeti örneğinde salt okunurdur bir sorgu çağırır.

Altyapı hizmeti 'nin yapılandırılmış bir veya daha fazla örneği olan kümeler için bu API, altyapı hizmeti 'nin belirli bir örneğine altyapıya özgü sorgular göndermek için bir yol sağlar. Kullanılabilir komutlar ve bunlara karşılık gelen yanıt biçimleri, kümenin üzerinde çalıştığı altyapıya bağlı olarak değişiklik gösterir. Bu API Service Fabric platformunu destekler; doğrudan kodunuzdan kullanılması amaçlıyordu.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --komut [gerekli] | Çağrılacak komutun metni. Komutun içeriği altyapıya özgüdür. |
| --hizmet kimliği | Altyapı hizmetinin kimliği. <br><br> Bu, altyapı hizmetinin ' Fabric\:' URI düzeni olmadan tam adıdır. Bu parametre yalnızca, çalışan bir altyapı hizmeti örneğine sahip olan küme için gereklidir. |
| --timeout-t | Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Hata Ayıkla | Tüm hata ayıklama günlüklerini göstermek için günlük ayrıntı düzeyini artırın. |
| --Yardım-h | Bu yardım iletisini gösterin ve çıkın. |
| --çıkış-o | Çıkış biçimi.  İzin verilen\: JSON, jsonc, tablo, TSV değerleri.  Varsayılan\: JSON. |
| --sorgu | JMESPath sorgu dizesi. Daha fazla\:bilgi ve örnek için bkz. http//jmespath.org/. |
| --ayrıntılı | Günlük ayrıntı düzeyini artırın. Tam hata ayıklama günlükleri için--Debug kullanın. |


## <a name="next-steps"></a>Sonraki adımlar
- Service Fabric CLı 'yi [ayarlayın](service-fabric-cli.md) .
- [Örnek betikleri](/azure/service-fabric/scripts/sfctl-upgrade-application)kullanarak Service Fabric CLI 'nın nasıl kullanılacağını öğrenin.
