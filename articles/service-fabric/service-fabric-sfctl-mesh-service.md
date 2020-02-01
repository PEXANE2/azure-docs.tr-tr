---
title: Azure Service Fabric CLı-sfctl kafes hizmeti
description: Azure Service Fabric komut satırı arabirimi olan sfctl hakkında bilgi edinin. Uygulama kaynağı için hizmet ayrıntıları almaya yönelik komutların bir listesini içerir.
author: jeffj6123
ms.topic: reference
ms.date: 1/16/2020
ms.author: jejarry
ms.openlocfilehash: 2b9f16062b02d7a1a3403d396643a7989b8976b7
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2020
ms.locfileid: "76905929"
---
# <a name="sfctl-mesh-service"></a>sfctl mesh service
Hizmet Ayrıntıları ve uygulama kaynağının listeleme hizmetleri alın.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| list | Tüm hizmet kaynaklarını listeler. |
| göster | Verilen ada sahip hizmet kaynağını alır. |

## <a name="sfctl-mesh-service-list"></a>sfctl ağ hizmeti listesi
Tüm hizmet kaynaklarını listeler.

Bir uygulama kaynağının tüm hizmetleriyle ilgili bilgileri alır. Bilgiler, hizmetin açıklamasını ve diğer özelliklerini içerir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --App-Name--uygulama-adı [gerekli] | Uygulamanın adı. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Hata Ayıkla | Tüm hata ayıklama günlüklerini göstermek için günlük ayrıntı düzeyini artırın. |
| --Yardım-h | Bu yardım iletisini gösterin ve çıkın. |
| --çıkış-o | Çıkış biçimi.  İzin verilen değerler JSON, jsonc, tablo, TSV\:.  JSON\: varsayılan. |
| --sorgu | JMESPath sorgu dizesi. Daha fazla bilgi ve örnek için bkz. http\://jmespath.org/. |
| --ayrıntılı | Günlük ayrıntı düzeyini artırın. Tam hata ayıklama günlükleri için--Debug kullanın. |

## <a name="sfctl-mesh-service-show"></a>sfctl kafes hizmeti Show
Verilen ada sahip hizmet kaynağını alır.

Verilen ada sahip hizmet kaynağıyla ilgili bilgileri alır. Bilgiler, hizmetin açıklamasını ve diğer özelliklerini içerir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --App-Name--uygulama-adı [gerekli] | Uygulamanın adı. |
| --Name-n [gerekli] | Hizmetin adı. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Hata Ayıkla | Tüm hata ayıklama günlüklerini göstermek için günlük ayrıntı düzeyini artırın. |
| --Yardım-h | Bu yardım iletisini gösterin ve çıkın. |
| --çıkış-o | Çıkış biçimi.  İzin verilen değerler JSON, jsonc, tablo, TSV\:.  JSON\: varsayılan. |
| --sorgu | JMESPath sorgu dizesi. Daha fazla bilgi ve örnek için bkz. http\://jmespath.org/. |
| --ayrıntılı | Günlük ayrıntı düzeyini artırın. Tam hata ayıklama günlükleri için--Debug kullanın. |


## <a name="next-steps"></a>Sonraki adımlar
- Service Fabric CLı 'yi [ayarlayın](service-fabric-cli.md) .
- [Örnek betikleri](/azure/service-fabric/scripts/sfctl-upgrade-application)kullanarak Service Fabric CLI 'nın nasıl kullanılacağını öğrenin.