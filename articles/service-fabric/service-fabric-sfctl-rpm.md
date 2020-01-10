---
title: Azure Service Fabric CLı-sfctl rpm
description: Azure Service Fabric komut satırı arabirimi olan sfctl hakkında bilgi edinin. Onarım Yöneticisi hizmeti için komutların bir listesini içerir.
author: jeffj6123
ms.topic: reference
ms.date: 9/17/2019
ms.author: jejarry
ms.openlocfilehash: 674970276046034d13801db7c1bb4ab5175385fb
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2020
ms.locfileid: "75639097"
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

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --görev-kimliği [gerekli] | Onarım görevinin KIMLIĞI. |
| --sürüm | Onarım görevinin geçerli sürüm numarası. Sıfır olmayan bir değer varsa, istek yalnızca, onarım görevinin gerçek geçerli sürümüyle eşleşiyorsa başarılı olur. Sıfır ise, sürüm denetimi gerçekleştirilmez. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Hata Ayıkla | Tüm hata ayıklama günlüklerini göstermek için günlük ayrıntı düzeyini artırın. |
| --Yardım-h | Bu yardım iletisini gösterin ve çıkın. |
| --çıkış-o | Çıkış biçimi.  İzin verilen değerler JSON, jsonc, tablo, TSV\:.  JSON\: varsayılan. |
| --sorgu | JMESPath sorgu dizesi. Daha fazla bilgi ve örnek için bkz. http\://jmespath.org/. |
| --ayrıntılı | Günlük ayrıntı düzeyini artırın. Tam hata ayıklama günlükleri için--Debug kullanın. |

## <a name="sfctl-rpm-delete"></a>sfctl RPM silme
Tamamlanmış bir onarım görevini siler.

Bu API Service Fabric platformunu destekler; doğrudan kodunuzdan kullanılması amaçlıyordu.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --görev-kimliği [gerekli] | Silinecek tamamlanmış onarım görevinin KIMLIĞI. |
| --sürüm | Onarım görevinin geçerli sürüm numarası. Sıfır olmayan bir değer varsa, istek yalnızca, onarım görevinin gerçek geçerli sürümüyle eşleşiyorsa başarılı olur. Sıfır ise, sürüm denetimi gerçekleştirilmez. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Hata Ayıkla | Tüm hata ayıklama günlüklerini göstermek için günlük ayrıntı düzeyini artırın. |
| --Yardım-h | Bu yardım iletisini gösterin ve çıkın. |
| --çıkış-o | Çıkış biçimi.  İzin verilen değerler JSON, jsonc, tablo, TSV\:.  JSON\: varsayılan. |
| --sorgu | JMESPath sorgu dizesi. Daha fazla bilgi ve örnek için bkz. http\://jmespath.org/. |
| --ayrıntılı | Günlük ayrıntı düzeyini artırın. Tam hata ayıklama günlükleri için--Debug kullanın. |

## <a name="sfctl-rpm-list"></a>sfctl RPM listesi
Verilen filtrelerle eşleşen Onarım görevlerinin listesini alır.

Bu API Service Fabric platformunu destekler; doğrudan kodunuzdan kullanılması amaçlıyordu.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Yürütücü-filtre | İstenen görevler listede yer almalıdır onarım yürütücüsü adı. |
| --durum-filtre | Sonuç listesine hangi görev durumlarının ekleneceğini belirten, aşağıdaki değerlerden oluşan bit düzeyinde OR. <br> 1-oluşturuldu <br>2-talep edilen  <br>4-hazırlama  <br>8-onaylanan  <br>16-yürütülüyor  <br>32-geri yükleme  <br>64-tamamlandı |
| --görev kimliği-filtre | Eşleştirilecek onarım görev KIMLIĞI ön eki. |

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
- Learn how to use the Service Fabric CLI using the [sample scripts](/azure/service-fabric/scripts/sfctl-upgrade-application).