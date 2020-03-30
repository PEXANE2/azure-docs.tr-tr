---
title: Azure Servis Kumaş CLI- sfctl örgü servisi
description: Azure Service Fabric komut satırı arabirimi sfctl hakkında bilgi edinin. Bir uygulama kaynağı için hizmet ayrıntılarını almak için komutların listesini içerir.
author: jeffj6123
ms.topic: reference
ms.date: 1/16/2020
ms.author: jejarry
ms.openlocfilehash: 2b9f16062b02d7a1a3403d396643a7989b8976b7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76905929"
---
# <a name="sfctl-mesh-service"></a>sfctl mesh service
Bir uygulama kaynağının hizmet ayrıntılarını ve liste hizmetlerini alın.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| list | Tüm hizmet kaynaklarını listeler. |
| göster | Verilen adla Hizmet kaynağını alır. |

## <a name="sfctl-mesh-service-list"></a>sfctl örgü servis listesi
Tüm hizmet kaynaklarını listeler.

Bir uygulama kaynağının tüm hizmetleri hakkında bilgi alır. Bilgiler, Hizmetin açıklamasını ve diğer özelliklerini içerir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --uygulama adı --uygulama adı [Gerekli] | Uygulamanın adı. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-mesh-service-show"></a>sfctl örgü servis gösterisi
Verilen adla Hizmet kaynağını alır.

Verilen adla Hizmet kaynağı hakkındaki bilgileri alır. Bilgiler, Hizmetin açıklamasını ve diğer özelliklerini içerir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --uygulama adı --uygulama adı [Gerekli] | Uygulamanın adı. |
| --isim -n [Gerekli] | Hizmetin adı. |

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