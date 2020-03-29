---
title: Azure Servis Kumaş CLI- sfctl rpm
description: Azure Service Fabric komut satırı arabirimi sfctl hakkında bilgi edinin. Onarım yöneticisi hizmeti için komutların listesini içerir.
author: jeffj6123
ms.topic: reference
ms.date: 1/16/2020
ms.author: jejarry
ms.openlocfilehash: 1787c0c8a8d5994ce1f1e9e4c55f155bbca6934c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76904950"
---
# <a name="sfctl-rpm"></a>sfctl rpm
Komutları onarım yöneticisi hizmetine sorgula ve gönder.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| onay kuvveti | Verilen onarım görevinin onayını zorlar. |
| delete | Tamamlanmış onarım görevini siler. |
| list | Verilen filtrelerle eşleşen onarım görevlerinin listesini alır. |

## <a name="sfctl-rpm-approve-force"></a>sfctl rpm onay-kuvvet
Verilen onarım görevinin onayını zorlar.

Bu API, Service Fabric platformını destekler; doğrudan kodunuzdan kullanılmak üzere değildir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --görev-id [Gerekli] | Onarım görevinin kimliği. |
| --sürüm | Onarım görevinin geçerli sürüm numarası. Sıfır değilse, istek yalnızca bu değer onarım görevinin geçerli sürümüyle eşleşirse başarılı olur. Sıfır ise, sürüm denetimi yapılmaz. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-rpm-delete"></a>sfctl rpm silme
Tamamlanmış onarım görevini siler.

Bu API, Service Fabric platformını destekler; doğrudan kodunuzdan kullanılmak üzere değildir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --görev-id [Gerekli] | Tamamlanan onarım görevinin kimliği silinecek. |
| --sürüm | Onarım görevinin geçerli sürüm numarası. Sıfır değilse, istek yalnızca bu değer onarım görevinin geçerli sürümüyle eşleşirse başarılı olur. Sıfır ise, sürüm denetimi yapılmaz. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-rpm-list"></a>sfctl rpm listesi
Verilen filtrelerle eşleşen onarım görevlerinin listesini alır.

Bu API, Service Fabric platformını destekler; doğrudan kodunuzdan kullanılmak üzere değildir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --executor-filtre | Talep edilen görevleri listeye dahil edilmesi gereken onarım uygulayıcısının adı. |
| --durum filtresi | Sonuç listesine hangi görev durumlarının eklenmesi gerektiğini belirten aşağıdaki değerlerin bit-wise-OR'u. <ul><li>1 - Oluşturuldu</li><li>2 - İddia</li><li>4 - Hazırlama</li><li>8 - Onaylı</li><li>16 - Yürütme</li><li>32 - Geri</li><li>64 - Tamamlandı</li></ul>
| --görev-id-filtre | Eşleşecek onarım görev kimliği öneki. |

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
