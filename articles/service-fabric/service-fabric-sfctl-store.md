---
title: Azure Servis Kumaş CLI- sfctl mağazası
description: Azure Service Fabric komut satırı arabirimi sfctl hakkında bilgi edinin. Küme görüntü deposunda dosya düzeyi işlemleri gerçekleştirmek için komutların listesini içerir.
author: jeffj6123
ms.topic: reference
ms.date: 1/16/2020
ms.author: jejarry
ms.openlocfilehash: 75c62b54ff3aa7f3af344aa3e1ca81d431ae0ab2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76905617"
---
# <a name="sfctl-store"></a>sfctl store
Küme görüntü deposunda temel dosya düzeyi işlemlerini gerçekleştirin.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| delete | Varolan resim deposu içeriğini siler. |
| kök bilgisi | İçerik bilgilerini görüntü deposunun kökünden alır. |
| Stat | Görüntü deposu içerik bilgilerini alır. |

## <a name="sfctl-store-delete"></a>sfctl mağaza silme
Varolan resim deposu içeriğini siler.

Verilen resim deposu göreli yol içinde bulunan varolan görüntü deposu içeriğini siler. Bu komut, yüklendikleri uygulama paketlerini yüklendikten sonra silmek için kullanılabilir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --içerik yolu [Gerekli] | Görüntü deposundaki dosya veya klasöre göreli yol kökünden. |
| --zaman ayarı -t | İşlemi saniyeler içinde gerçekleştirmek için sunucu zaman. Bu zaman sonu, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametrenin varsayılan değeri 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-store-root-info"></a>sfctl mağaza kök-bilgi
İçerik bilgilerini görüntü deposunun kökünden alır.

Görüntü deposunun kökündeki görüntü deposu içeriğihakkındaki bilgileri verir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --zaman ayarı -t | İşlemi saniyeler içinde gerçekleştirmek için sunucu zaman. Bu zaman sonu, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametrenin varsayılan değeri 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-store-stat"></a>sfctl mağaza stat
Görüntü deposu içerik bilgilerini alır.

Belirtilen içerikPath'te görüntü deposu içeriği hakkındaki bilgileri verir. ContentPath görüntü deposunun köküne göredir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --içerik yolu [Gerekli] | Görüntü deposundaki dosya veya klasöre göreli yol kökünden. |
| --zaman ayarı -t | İşlemi saniyeler içinde gerçekleştirmek için sunucu zaman. Bu zaman sonu, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametrenin varsayılan değeri 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |


## <a name="next-steps"></a>Sonraki adımlar
- Servis Kumaşı CLI'yi [kur.](service-fabric-cli.md)
- [Örnek komut dosyalarını](/azure/service-fabric/scripts/sfctl-upgrade-application)kullanarak Service Fabric CLI'yi nasıl kullanacağınızı öğrenin.