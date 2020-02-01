---
title: Azure Service Fabric CLı-sfctl deposu
description: Azure Service Fabric komut satırı arabirimi olan sfctl hakkında bilgi edinin. Küme görüntü deposunda dosya düzeyi işlemleri gerçekleştirmeye yönelik komutların bir listesini içerir.
author: jeffj6123
ms.topic: reference
ms.date: 1/16/2020
ms.author: jejarry
ms.openlocfilehash: 75c62b54ff3aa7f3af344aa3e1ca81d431ae0ab2
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2020
ms.locfileid: "76905617"
---
# <a name="sfctl-store"></a>sfctl store
Küme görüntü deposunda temel dosya düzeyi işlemleri gerçekleştirin.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| delete | Mevcut görüntü deposu içeriğini siler. |
| kök-bilgi | Görüntü deposunun kökündeki içerik bilgilerini alır. |
| istatistik | Görüntü deposu içerik bilgilerini alır. |

## <a name="sfctl-store-delete"></a>sfctl deposu silme
Mevcut görüntü deposu içeriğini siler.

Verilen görüntü deposu göreli yolu içinde bulunan mevcut görüntü deposu içeriğini siler. Bu komut, yüklenen uygulama paketlerini sağlandıktan sonra silmek için kullanılabilir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Content-Path [gerekli] | Görüntü deposundaki dosya veya klasörün kök yolundan göreli yolu. |
| --timeout-t | İşlemi saniye cinsinden gerçekleştirmek için sunucu zaman aşımı. Bu zaman aşımı, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametre için varsayılan değer 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Hata Ayıkla | Tüm hata ayıklama günlüklerini göstermek için günlük ayrıntı düzeyini artırın. |
| --Yardım-h | Bu yardım iletisini gösterin ve çıkın. |
| --çıkış-o | Çıkış biçimi.  İzin verilen değerler JSON, jsonc, tablo, TSV\:.  JSON\: varsayılan. |
| --sorgu | JMESPath sorgu dizesi. Daha fazla bilgi ve örnek için bkz. http\://jmespath.org/. |
| --ayrıntılı | Günlük ayrıntı düzeyini artırın. Tam hata ayıklama günlükleri için--Debug kullanın. |

## <a name="sfctl-store-root-info"></a>sfctl deposu kök-bilgi
Görüntü deposunun kökündeki içerik bilgilerini alır.

Görüntü deposunun kökündeki görüntü deposu içeriğiyle ilgili bilgileri döndürür.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --timeout-t | İşlemi saniye cinsinden gerçekleştirmek için sunucu zaman aşımı. Bu zaman aşımı, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametre için varsayılan değer 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Hata Ayıkla | Tüm hata ayıklama günlüklerini göstermek için günlük ayrıntı düzeyini artırın. |
| --Yardım-h | Bu yardım iletisini gösterin ve çıkın. |
| --çıkış-o | Çıkış biçimi.  İzin verilen değerler JSON, jsonc, tablo, TSV\:.  JSON\: varsayılan. |
| --sorgu | JMESPath sorgu dizesi. Daha fazla bilgi ve örnek için bkz. http\://jmespath.org/. |
| --ayrıntılı | Günlük ayrıntı düzeyini artırın. Tam hata ayıklama günlükleri için--Debug kullanın. |

## <a name="sfctl-store-stat"></a>sfctl mağaza stat
Görüntü deposu içerik bilgilerini alır.

Belirtilen contentPath konumundaki görüntü deposu içeriğiyle ilgili bilgileri döndürür. ContentPath, görüntü deposunun köküne göredir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Content-Path [gerekli] | Görüntü deposundaki dosya veya klasörün kök yolundan göreli yolu. |
| --timeout-t | İşlemi saniye cinsinden gerçekleştirmek için sunucu zaman aşımı. Bu zaman aşımı, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametre için varsayılan değer 60 saniyedir.  Varsayılan\: 60. |

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