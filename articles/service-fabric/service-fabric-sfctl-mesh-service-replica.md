---
title: Azure Service Fabric CLı-sfctl kafes hizmeti-çoğaltma
description: Azure Service Fabric komut satırı arabirimi olan sfctl hakkında bilgi edinin. Uygulama kaynakları için çoğaltma ayrıntılarını alma komutlarının bir listesini içerir.
author: jeffj6123
ms.topic: reference
ms.date: 1/16/2020
ms.author: jejarry
ms.openlocfilehash: cbfdba30663e2aa531ab1db955b0e035a0588709
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "86245730"
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

### <a name="arguments"></a>Bağımsız değişkenler

|Bağımsız Değişken|Description|
| --- | --- |
| --App-Name--uygulama-adı [gerekli] | Uygulamanın adı. |
| --hizmet-adı [gerekli] | Hizmetin adı. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız Değişken|Description|
| --- | --- |
| --Hata Ayıkla | Tüm hata ayıklama günlüklerini göstermek için günlük ayrıntı düzeyini artırın. |
| --Yardım-h | Bu yardım iletisini gösterin ve çıkın. |
| --çıkış-o | Çıkış biçimi.  İzin verilen \: JSON, jsonc, tablo, TSV değerleri.  Varsayılan \: JSON. |
| --sorgu | JMESPath sorgu dizesi. \:Daha fazla bilgi ve örnek için bkz. http//jmespath.org/. |
| --ayrıntılı | Günlük ayrıntı düzeyini artırın. Tam hata ayıklama günlükleri için--Debug kullanın. |

## <a name="sfctl-mesh-service-replica-show"></a>sfctl kafes hizmeti-çoğaltma göster
Bir uygulama hizmetinin verilen çoğaltmasını alır.

Verilen ada sahip hizmet çoğaltması hakkındaki bilgileri alır. Bilgiler, hizmet çoğaltmasının açıklamasını ve diğer özelliklerini içerir.

### <a name="arguments"></a>Bağımsız değişkenler

|Bağımsız Değişken|Description|
| --- | --- |
| --App-Name--uygulama-adı [gerekli] | Uygulamanın adı. |
| --Name-n [gerekli] | Hizmet çoğaltmasının adı. |
| --hizmet-adı [gerekli] | Hizmetin adı. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız Değişken|Description|
| --- | --- |
| --Hata Ayıkla | Tüm hata ayıklama günlüklerini göstermek için günlük ayrıntı düzeyini artırın. |
| --Yardım-h | Bu yardım iletisini gösterin ve çıkın. |
| --çıkış-o | Çıkış biçimi.  İzin verilen \: JSON, jsonc, tablo, TSV değerleri.  Varsayılan \: JSON. |
| --sorgu | JMESPath sorgu dizesi. \:Daha fazla bilgi ve örnek için bkz. http//jmespath.org/. |
| --ayrıntılı | Günlük ayrıntı düzeyini artırın. Tam hata ayıklama günlükleri için--Debug kullanın. |


## <a name="next-steps"></a>Sonraki adımlar
- Service Fabric CLı 'yi [ayarlayın](service-fabric-cli.md) .
- [Örnek betikleri](./scripts/sfctl-upgrade-application.md)kullanarak Service Fabric CLI 'nın nasıl kullanılacağını öğrenin.
