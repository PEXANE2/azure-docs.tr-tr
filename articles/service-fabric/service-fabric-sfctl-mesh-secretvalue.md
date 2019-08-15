---
title: Azure Service Fabric CLı-sfctl ağ secretvalue | Microsoft Docs
description: Service Fabric CLı sfctl ağ secretvalue komutlarını açıklar.
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
ms.openlocfilehash: 5200c40afe62ce3a236a3a16b59b64deb8d24ced
ms.sourcegitcommit: 18061d0ea18ce2c2ac10652685323c6728fe8d5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/15/2019
ms.locfileid: "69035935"
---
# <a name="sfctl-mesh-secretvalue"></a>sfctl mesh secretvalue
Ağ secretvalue kaynaklarını alın ve silin.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| sil | Adlandırılmış gizli kaynağın belirtilen değerini siler. |
| list | Belirtilen gizli kaynağın tüm değerlerinin adlarını listeleyin. |
| göster | Gizli bir kaynağın belirtilen sürümünün değerini alır. |

## <a name="sfctl-mesh-secretvalue-delete"></a>sfctl ağ secretvalue Delete
Adlandırılmış gizli kaynağın belirtilen değerini siler.

Ad tarafından tanımlanan gizli değer kaynağını siler. Kaynağın adı genellikle bu değerle ilişkili sürümdür. Belirtilen değer kullanımda olduğunda silme işlemi başarısız olur.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Secret-adı-n [gerekli] | Gizli kaynağın adı. |
| --sürüm-v [gerekli] | Gizli dizi sürümünün adı. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Hata Ayıkla | Tüm hata ayıklama günlüklerini göstermek için günlük ayrıntı düzeyini artırın. |
| --Yardım-h | Bu yardım iletisini gösterin ve çıkın. |
| --çıkış-o | Çıkış biçimi.  İzin verilen\: JSON, jsonc, tablo, TSV değerleri.  Varsayılan\: JSON. |
| --sorgu | JMESPath sorgu dizesi. Daha fazla\:bilgi ve örnek için bkz. http//jmespath.org/. |
| --ayrıntılı | Günlük ayrıntı düzeyini artırın. Tam hata ayıklama günlükleri için--Debug kullanın. |

## <a name="sfctl-mesh-secretvalue-list"></a>sfctl ağ secretvalue listesi
Belirtilen gizli kaynağın tüm değerlerinin adlarını listeleyin.

Belirtilen gizli kaynağın tüm gizli değer kaynakları hakkında bilgi alır. Bilgiler, gizli değer kaynaklarının adlarını içerir, ancak gerçek değerleri içermez.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Secret-adı-n [gerekli] | Gizli kaynağın adı. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Hata Ayıkla | Tüm hata ayıklama günlüklerini göstermek için günlük ayrıntı düzeyini artırın. |
| --Yardım-h | Bu yardım iletisini gösterin ve çıkın. |
| --çıkış-o | Çıkış biçimi.  İzin verilen\: JSON, jsonc, tablo, TSV değerleri.  Varsayılan\: JSON. |
| --sorgu | JMESPath sorgu dizesi. Daha fazla\:bilgi ve örnek için bkz. http//jmespath.org/. |
| --ayrıntılı | Günlük ayrıntı düzeyini artırın. Tam hata ayıklama günlükleri için--Debug kullanın. |

## <a name="sfctl-mesh-secretvalue-show"></a>sfctl ağı secretvalue Show
Gizli bir kaynağın belirtilen sürümünün değerini alır.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Secret-adı-n [gerekli] | Gizli kaynağın adı. |
| --sürüm-v [gerekli] | Gizli dizi sürümünün adı. |
| --değer göster | Gizli sürümün gerçek değerini gösterir. |

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