---
title: Azure Service Fabric CLı-sfctl deposu | Microsoft Docs
description: Service Fabric CLı sfctl depolama komutlarını açıklar.
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
ms.openlocfilehash: ccd9e8323f6e0de7b81c7600e7828e4858c51201
ms.sourcegitcommit: 18061d0ea18ce2c2ac10652685323c6728fe8d5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/15/2019
ms.locfileid: "69035853"
---
# <a name="sfctl-store"></a>sfctl store
Küme görüntü deposunda temel dosya düzeyi işlemleri gerçekleştirin.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| sil | Mevcut görüntü deposu içeriğini siler. |
| kök-bilgi | Görüntü deposunun kökündeki içerik bilgilerini alır. |
| istatistik | Görüntü deposu içerik bilgilerini alır. |

## <a name="sfctl-store-delete"></a>sfctl deposu silme
Mevcut görüntü deposu içeriğini siler.

Verilen görüntü deposu göreli yolu içinde bulunan mevcut görüntü deposu içeriğini siler. Bu komut, yüklenen uygulama paketlerini sağlandıktan sonra silmek için kullanılabilir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Content-Path [gerekli] | Görüntü deposundaki dosya veya klasörün kök yolundan göreli yolu. |
| --timeout-t | Saniye cinsinden sunucu zaman aşımı.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Hata Ayıkla | Tüm hata ayıklama günlüklerini göstermek için günlük ayrıntı düzeyini artırın. |
| --Yardım-h | Bu yardım iletisini gösterin ve çıkın. |
| --çıkış-o | Çıkış biçimi.  İzin verilen\: JSON, jsonc, tablo, TSV değerleri.  Varsayılan\: JSON. |
| --sorgu | JMESPath sorgu dizesi. Daha fazla\:bilgi ve örnek için bkz. http//jmespath.org/. |
| --ayrıntılı | Günlük ayrıntı düzeyini artırın. Tam hata ayıklama günlükleri için--Debug kullanın. |

## <a name="sfctl-store-root-info"></a>sfctl deposu kök-bilgi
Görüntü deposunun kökündeki içerik bilgilerini alır.

Görüntü deposunun kökündeki görüntü deposu içeriğiyle ilgili bilgileri döndürür.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --timeout-t | Saniye cinsinden sunucu zaman aşımı.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Hata Ayıkla | Tüm hata ayıklama günlüklerini göstermek için günlük ayrıntı düzeyini artırın. |
| --Yardım-h | Bu yardım iletisini gösterin ve çıkın. |
| --çıkış-o | Çıkış biçimi.  İzin verilen\: JSON, jsonc, tablo, TSV değerleri.  Varsayılan\: JSON. |
| --sorgu | JMESPath sorgu dizesi. Daha fazla\:bilgi ve örnek için bkz. http//jmespath.org/. |
| --ayrıntılı | Günlük ayrıntı düzeyini artırın. Tam hata ayıklama günlükleri için--Debug kullanın. |

## <a name="sfctl-store-stat"></a>sfctl mağaza stat
Görüntü deposu içerik bilgilerini alır.

Belirtilen contentPath konumundaki görüntü deposu içeriğiyle ilgili bilgileri döndürür. ContentPath, görüntü deposunun köküne göredir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Content-Path [gerekli] | Görüntü deposundaki dosya veya klasörün kök yolundan göreli yolu. |
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