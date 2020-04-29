---
title: Azure Service Fabric CLı-sfctl ağ secretvalue
description: Azure Service Fabric komut satırı arabirimi olan sfctl hakkında bilgi edinin. Service Fabric kafes secretvalue kaynaklarını alma ve silmeye yönelik komutların bir listesini içerir.
author: jeffj6123
ms.topic: reference
ms.date: 1/16/2020
ms.author: jejarry
ms.openlocfilehash: b2fe3a7995cdd88a8f63d0d865e72e9f161cf9d7
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "76905970"
---
# <a name="sfctl-mesh-secretvalue"></a>sfctl mesh secretvalue
Ağ secretvalue kaynaklarını alın ve silin.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| delete | Adlandırılmış gizli kaynağın belirtilen değerini siler. |
| list | Belirtilen gizli kaynağın tüm değerlerinin adlarını listeleyin. |
| göster | Gizli kaynağın belirtilen değerini listeler. |

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
Gizli kaynağın belirtilen değerini listeler.

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