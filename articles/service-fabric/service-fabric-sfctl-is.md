---
title: Azure Service Fabric CLı-sfctl | Microsoft Docs
description: CLı sfctl komutlarının Service Fabric açıklar.
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
ms.openlocfilehash: 998a94350250402d4face64f64e12f32cf5b2a36
ms.sourcegitcommit: 18061d0ea18ce2c2ac10652685323c6728fe8d5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/15/2019
ms.locfileid: "69036497"
---
# <a name="sfctl-is"></a>sfctl is
Altyapı hizmetine komutları sorgulayın ve gönderin.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| Komutu | Verilen altyapı hizmeti örneğinde bir yönetim komutu çağırır. |
| query | Verilen altyapı hizmeti örneğinde salt okunurdur bir sorgu çağırır. |

## <a name="sfctl-is-command"></a>sfctl komutu
Verilen altyapı hizmeti örneğinde bir yönetim komutu çağırır.

Altyapı hizmeti 'nin yapılandırılmış bir veya daha fazla örneği olan kümeler için bu API, altyapı hizmeti 'nin belirli bir örneğine altyapıya özgü komutlar göndermek için bir yol sağlar. Kullanılabilir komutlar ve bunlara karşılık gelen yanıt biçimleri, kümenin üzerinde çalıştığı altyapıya bağlı olarak değişiklik gösterir. Bu API Service Fabric platformunu destekler; doğrudan kodunuzdan kullanılması amaçlıyordu.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --komut [gerekli] | Çağrılacak komutun metni. Komutun içeriği altyapıya özgüdür. |
| --hizmet kimliği | Altyapı hizmetinin kimliği. <br><br> Bu, altyapı hizmetinin ' Fabric\:' URI düzeni olmadan tam adıdır. Bu parametre yalnızca, çalışan bir altyapı hizmeti örneğine sahip olan küme için gereklidir. |
| --timeout-t | Saniye cinsinden sunucu zaman aşımı.  Varsayılan\: 60. |

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
| --timeout-t | Saniye cinsinden sunucu zaman aşımı.  Varsayılan\: 60. |

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