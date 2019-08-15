---
title: Azure Service Fabric CLı-sfctl rpm | Microsoft Docs
description: Service Fabric CLı sfctl RPM komutlarını açıklar.
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
ms.openlocfilehash: 3f40451087aba5af5b02625ad3ac1ca6231d976c
ms.sourcegitcommit: 18061d0ea18ce2c2ac10652685323c6728fe8d5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/15/2019
ms.locfileid: "69035881"
---
# <a name="sfctl-rpm"></a>sfctl rpm
Komutları onarma Yöneticisi hizmetine sorgula ve gönder.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| Onayla-zorla | Verilen onarım görevinin onayını zorlar. |
| sil | Tamamlanmış bir onarım görevini siler. |
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
| --çıkış-o | Çıkış biçimi.  İzin verilen\: JSON, jsonc, tablo, TSV değerleri.  Varsayılan\: JSON. |
| --sorgu | JMESPath sorgu dizesi. Daha fazla\:bilgi ve örnek için bkz. http//jmespath.org/. |
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
| --çıkış-o | Çıkış biçimi.  İzin verilen\: JSON, jsonc, tablo, TSV değerleri.  Varsayılan\: JSON. |
| --sorgu | JMESPath sorgu dizesi. Daha fazla\:bilgi ve örnek için bkz. http//jmespath.org/. |
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
| --çıkış-o | Çıkış biçimi.  İzin verilen\: JSON, jsonc, tablo, TSV değerleri.  Varsayılan\: JSON. |
| --sorgu | JMESPath sorgu dizesi. Daha fazla\:bilgi ve örnek için bkz. http//jmespath.org/. |
| --ayrıntılı | Günlük ayrıntı düzeyini artırın. Tam hata ayıklama günlükleri için--Debug kullanın. |


## <a name="next-steps"></a>Sonraki adımlar
- Service Fabric CLı 'yi [ayarlayın](service-fabric-cli.md) .
- [Örnek betikleri](/azure/service-fabric/scripts/sfctl-upgrade-application)kullanarak Service Fabric CLI 'nın nasıl kullanılacağını öğrenin.