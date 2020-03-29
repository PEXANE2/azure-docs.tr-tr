---
title: Azure Servis Kumaş CLI- sfctl örgü hizmet-çoğaltma
description: Azure Service Fabric komut satırı arabirimi sfctl hakkında bilgi edinin. Uygulama kaynakları için yineleme ayrıntıları almak için komutların listesini içerir.
author: jeffj6123
ms.topic: reference
ms.date: 1/16/2020
ms.author: jejarry
ms.openlocfilehash: ac604ec0a854964746ef34627b054e853662265f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76905943"
---
# <a name="sfctl-mesh-service-replica"></a>sfctl mesh service-replica
Bir uygulama kaynağında çoğaltma ayrıntılarını ve belirli bir hizmetin yinelemelerini listele.'yi alın.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| list | Bir hizmetin tüm kopyalarını listeler. |
| göster | Bir uygulamanın hizmetin verilen kopyasını alır. |

## <a name="sfctl-mesh-service-replica-list"></a>sfctl örgü hizmet çoğaltma listesi
Bir hizmetin tüm kopyalarını listeler.

Bir hizmetin tüm kopyaları hakkında bilgi alır. Bilgiler, hizmet yinelemesinin açıklamasını ve diğer özelliklerini içerir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --uygulama adı --uygulama adı [Gerekli] | Uygulamanın adı. |
| --hizmet adı [Gerekli] | Hizmetin adı. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-mesh-service-replica-show"></a>sfctl örgü hizmet-çoğaltma gösterisi
Bir uygulamanın hizmetin verilen kopyasını alır.

Verilen adla hizmet çoğaltma hakkında bilgi alır. Bilgiler, hizmet yinelemesinin açıklamasını ve diğer özelliklerini içerir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --uygulama adı --uygulama adı [Gerekli] | Uygulamanın adı. |
| --isim -n [Gerekli] | Hizmet çoğaltmaadı. |
| --hizmet adı [Gerekli] | Hizmetin adı. |

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