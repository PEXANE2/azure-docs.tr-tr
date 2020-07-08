---
title: Azure Service Fabric CLı-sfctl rpm
description: Azure Service Fabric komut satırı arabirimi olan sfctl hakkında bilgi edinin. Onarım Yöneticisi hizmeti için komutların bir listesini içerir.
author: jeffj6123
ms.topic: reference
ms.date: 1/16/2020
ms.author: jejarry
ms.openlocfilehash: 1787c0c8a8d5994ce1f1e9e4c55f155bbca6934c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "76904950"
---
# <a name="sfctl-rpm"></a>sfctl rpm
Komutları onarma Yöneticisi hizmetine sorgula ve gönder.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| Onayla-zorla | Verilen onarım görevinin onayını zorlar. |
| delete | Tamamlanmış bir onarım görevini siler. |
| list | Verilen filtrelerle eşleşen Onarım görevlerinin listesini alır. |

## <a name="sfctl-rpm-approve-force"></a>sfctl RPM onaylama-zorlama
Verilen onarım görevinin onayını zorlar.

Bu API Service Fabric platformunu destekler; doğrudan kodunuzdan kullanılması amaçlıyordu.

### <a name="arguments"></a>Arguments

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --görev-kimliği [gerekli] | Onarım görevinin KIMLIĞI. |
| --sürüm | Onarım görevinin geçerli sürüm numarası. Sıfır olmayan bir değer varsa, istek yalnızca, onarım görevinin gerçek geçerli sürümüyle eşleşiyorsa başarılı olur. Sıfır ise, sürüm denetimi gerçekleştirilmez. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Hata Ayıkla | Tüm hata ayıklama günlüklerini göstermek için günlük ayrıntı düzeyini artırın. |
| --Yardım-h | Bu yardım iletisini gösterin ve çıkın. |
| --çıkış-o | Çıkış biçimi.  İzin verilen \: JSON, jsonc, tablo, TSV değerleri.  Varsayılan \: JSON. |
| --sorgu | JMESPath sorgu dizesi. \:Daha fazla bilgi ve örnek için bkz. http//jmespath.org/. |
| --ayrıntılı | Günlük ayrıntı düzeyini artırın. Tam hata ayıklama günlükleri için--Debug kullanın. |

## <a name="sfctl-rpm-delete"></a>sfctl RPM silme
Tamamlanmış bir onarım görevini siler.

Bu API Service Fabric platformunu destekler; doğrudan kodunuzdan kullanılması amaçlıyordu.

### <a name="arguments"></a>Arguments

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --görev-kimliği [gerekli] | Silinecek tamamlanmış onarım görevinin KIMLIĞI. |
| --sürüm | Onarım görevinin geçerli sürüm numarası. Sıfır olmayan bir değer varsa, istek yalnızca, onarım görevinin gerçek geçerli sürümüyle eşleşiyorsa başarılı olur. Sıfır ise, sürüm denetimi gerçekleştirilmez. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Hata Ayıkla | Tüm hata ayıklama günlüklerini göstermek için günlük ayrıntı düzeyini artırın. |
| --Yardım-h | Bu yardım iletisini gösterin ve çıkın. |
| --çıkış-o | Çıkış biçimi.  İzin verilen \: JSON, jsonc, tablo, TSV değerleri.  Varsayılan \: JSON. |
| --sorgu | JMESPath sorgu dizesi. \:Daha fazla bilgi ve örnek için bkz. http//jmespath.org/. |
| --ayrıntılı | Günlük ayrıntı düzeyini artırın. Tam hata ayıklama günlükleri için--Debug kullanın. |

## <a name="sfctl-rpm-list"></a>sfctl RPM listesi
Verilen filtrelerle eşleşen Onarım görevlerinin listesini alır.

Bu API Service Fabric platformunu destekler; doğrudan kodunuzdan kullanılması amaçlıyordu.

### <a name="arguments"></a>Arguments

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Yürütücü-filtre | İstenen görevler listede yer almalıdır onarım yürütücüsü adı. |
| --durum-filtre | Sonuç listesine hangi görev durumlarının ekleneceğini belirten, aşağıdaki değerlerden oluşan bit düzeyinde OR. <ul><li>1-oluşturuldu</li><li>2-talep edilen</li><li>4-hazırlama</li><li>8-onaylanan</li><li>16-yürütülüyor</li><li>32-geri yükleme</li><li>64-tamamlandı</li></ul>
| --görev kimliği-filtre | Eşleştirilecek onarım görev KIMLIĞI ön eki. |

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
