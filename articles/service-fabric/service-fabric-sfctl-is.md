---
title: Azure Servis Kumaş ı CLI- sfctl olduğunu
description: Azure Hizmet Kumaşı komut satırı arabirimi sfctl hakkında bilgi edinin. Altyapıyı yönetmek için komutların listesini içerir.
author: jeffj6123
ms.topic: reference
ms.date: 1/16/2020
ms.author: jejarry
ms.openlocfilehash: a64aaca97c9df61d795c82d64e6048d6f9ae032f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76906081"
---
# <a name="sfctl-is"></a>sfctl is
Komutları altyapı hizmetine sorgula ve gönder.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| command | Verilen Altyapı Hizmeti örneğinde bir yönetim komutu çağırır. |
| sorgu | Verilen altyapı hizmeti örneğinde salt okunur sorgusunu çağırır. |

## <a name="sfctl-is-command"></a>sfctl komutudur
Verilen Altyapı Hizmeti örneğinde bir yönetim komutu çağırır.

Altyapı Hizmetinin bir veya daha fazla örneğini yapılandırılan kümeler için bu API, Altyapı Hizmeti'nin belirli bir örneğine altyapıya özgü komutlar göndermenin bir yolunu sağlar. Kullanılabilir komutlar ve karşılık gelen yanıt biçimleri kümenin çalıştırıldığı altyapıya bağlı olarak değişir. Bu API, Service Fabric platformını destekler; doğrudan kodunuzdan kullanılmak üzere değildir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --komut [Gerekli] | Çağrılacak komutun metni. Komutun içeriği altyapıya özgüdür. |
| --hizmet-id | Altyapı hizmetinin kimliği. <br><br> Bu 'kumaş\:' URI düzeni olmadan altyapı hizmetitam adıdır. Bu parametre yalnızca birden fazla altyapı hizmeti çalıştıran küme için gereklidir. |
| --zaman ayarı -t | Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-is-query"></a>sfctl sorgudur
Verilen altyapı hizmeti örneğinde salt okunur sorgusunu çağırır.

Altyapı Hizmeti'nin bir veya daha fazla örneği yapılandırılan kümeler için bu API, Altyapı Hizmeti'nin belirli bir örneğine altyapıya özgü sorgular göndermenin bir yolunu sağlar. Kullanılabilir komutlar ve karşılık gelen yanıt biçimleri kümenin çalıştırıldığı altyapıya bağlı olarak değişir. Bu API, Service Fabric platformını destekler; doğrudan kodunuzdan kullanılmak üzere değildir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --komut [Gerekli] | Çağrılacak komutun metni. Komutun içeriği altyapıya özgüdür. |
| --hizmet-id | Altyapı hizmetinin kimliği. <br><br> Bu 'kumaş\:' URI düzeni olmadan altyapı hizmetitam adıdır. Bu parametre yalnızca birden fazla altyapı hizmeti çalıştıran küme için gereklidir. |
| --zaman ayarı -t | Varsayılan\: 60. |

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
