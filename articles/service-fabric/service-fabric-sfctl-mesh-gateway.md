---
title: Azure Service Fabric CLı-sfctl ağı ağ geçidi | Microsoft Docs
description: Service Fabric CLı sfctl ağı ağ geçidi komutlarını açıklar.
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
ms.openlocfilehash: b8acb23b274e6062ae3c50ab80220ab93297037e
ms.sourcegitcommit: 18061d0ea18ce2c2ac10652685323c6728fe8d5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/15/2019
ms.locfileid: "69036422"
---
# <a name="sfctl-mesh-gateway"></a>sfctl mesh gateway
Ağ Geçidi kaynaklarını alın ve silin.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| sil | Ağ Geçidi kaynağını siler. |
| list | Tüm ağ geçidi kaynaklarını listeler. |
| göster | Belirtilen ada sahip ağ geçidi kaynağını alır. |

## <a name="sfctl-mesh-gateway-delete"></a>sfctl ağı ağ geçidi silme
Ağ Geçidi kaynağını siler.

Ad tarafından tanımlanan ağ geçidi kaynağını siler.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Name-n [gerekli] | Ağ Geçidi kaynağının adı. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Hata Ayıkla | Tüm hata ayıklama günlüklerini göstermek için günlük ayrıntı düzeyini artırın. |
| --Yardım-h | Bu yardım iletisini gösterin ve çıkın. |
| --çıkış-o | Çıkış biçimi.  İzin verilen\: JSON, jsonc, tablo, TSV değerleri.  Varsayılan\: JSON. |
| --sorgu | JMESPath sorgu dizesi. Daha fazla\:bilgi ve örnek için bkz. http//jmespath.org/. |
| --ayrıntılı | Günlük ayrıntı düzeyini artırın. Tam hata ayıklama günlükleri için--Debug kullanın. |

## <a name="sfctl-mesh-gateway-list"></a>sfctl ağı ağ geçidi listesi
Tüm ağ geçidi kaynaklarını listeler.

Belirli bir kaynak grubundaki tüm ağ geçidi kaynaklarıyla ilgili bilgileri alır. Bilgiler, ağ geçidinin açıklamasını ve diğer özelliklerini içerir.

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Hata Ayıkla | Tüm hata ayıklama günlüklerini göstermek için günlük ayrıntı düzeyini artırın. |
| --Yardım-h | Bu yardım iletisini gösterin ve çıkın. |
| --çıkış-o | Çıkış biçimi.  İzin verilen\: JSON, jsonc, tablo, TSV değerleri.  Varsayılan\: JSON. |
| --sorgu | JMESPath sorgu dizesi. Daha fazla\:bilgi ve örnek için bkz. http//jmespath.org/. |
| --ayrıntılı | Günlük ayrıntı düzeyini artırın. Tam hata ayıklama günlükleri için--Debug kullanın. |

## <a name="sfctl-mesh-gateway-show"></a>sfctl ağı ağ geçidi Show
Belirtilen ada sahip ağ geçidi kaynağını alır.

Belirtilen ada sahip ağ geçidi kaynağıyla ilgili bilgileri alır. Bilgiler, ağ geçidinin açıklamasını ve diğer özelliklerini içerir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Name-n [gerekli] | Ağ Geçidi kaynağının adı. |

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