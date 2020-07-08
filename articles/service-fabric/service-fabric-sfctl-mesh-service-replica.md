---
title: Azure Service Fabric CLı-sfctl kafes hizmeti-çoğaltma
description: Azure Service Fabric komut satırı arabirimi olan sfctl hakkında bilgi edinin. Uygulama kaynakları için çoğaltma ayrıntılarını alma komutlarının bir listesini içerir.
author: jeffj6123
ms.topic: reference
ms.date: 1/16/2020
ms.author: jejarry
ms.openlocfilehash: ac604ec0a854964746ef34627b054e853662265f
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "76905943"
---
# <a name="sfctl-mesh-service-replica"></a>sfctl mesh service-replica
Bir uygulama kaynağında belirli bir hizmetin çoğaltma ayrıntılarını ve liste çoğaltmalarını alma.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| list | Bir hizmetin tüm çoğaltmalarını listeler. |
| göster | Bir uygulama hizmetinin verilen çoğaltmasını alır. |

## <a name="sfctl-mesh-service-replica-list"></a>sfctl kafes hizmeti-çoğaltma listesi
Bir hizmetin tüm çoğaltmalarını listeler.

Bir hizmetin tüm çoğaltmalarıyla ilgili bilgileri alır. Bilgiler, hizmet çoğaltmasının açıklamasını ve diğer özelliklerini içerir.

### <a name="arguments"></a>Arguments

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --App-Name--uygulama-adı [gerekli] | Uygulamanın adı. |
| --hizmet-adı [gerekli] | Hizmetin adı. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Hata Ayıkla | Tüm hata ayıklama günlüklerini göstermek için günlük ayrıntı düzeyini artırın. |
| --Yardım-h | Bu yardım iletisini gösterin ve çıkın. |
| --çıkış-o | Çıkış biçimi.  İzin verilen \: JSON, jsonc, tablo, TSV değerleri.  Varsayılan \: JSON. |
| --sorgu | JMESPath sorgu dizesi. \:Daha fazla bilgi ve örnek için bkz. http//jmespath.org/. |
| --ayrıntılı | Günlük ayrıntı düzeyini artırın. Tam hata ayıklama günlükleri için--Debug kullanın. |

## <a name="sfctl-mesh-service-replica-show"></a>sfctl kafes hizmeti-çoğaltma göster
Bir uygulama hizmetinin verilen çoğaltmasını alır.

Verilen ada sahip hizmet çoğaltması hakkındaki bilgileri alır. Bilgiler, hizmet çoğaltmasının açıklamasını ve diğer özelliklerini içerir.

### <a name="arguments"></a>Arguments

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --App-Name--uygulama-adı [gerekli] | Uygulamanın adı. |
| --Name-n [gerekli] | Hizmet çoğaltmasının adı. |
| --hizmet-adı [gerekli] | Hizmetin adı. |

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