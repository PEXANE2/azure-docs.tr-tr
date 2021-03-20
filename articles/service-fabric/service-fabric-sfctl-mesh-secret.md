---
title: Azure Service Fabric CLı-sfctl kafesi gizli anahtarı
description: Azure Service Fabric komut satırı arabirimi olan sfctl hakkında bilgi edinin. Service Fabric kafesi gizli kaynakları alma ve silme komutlarının bir listesini içerir.
author: jeffj6123
ms.topic: reference
ms.date: 1/16/2020
ms.author: jejarry
ms.openlocfilehash: 2c91a59e57613934c946e3cd56089b330893a51a
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "86260346"
---
# <a name="sfctl-mesh-secret"></a>sfctl mesh secret
Ağ gizli anahtarı kaynaklarını alın ve silin.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| delete | Gizli kaynağı siler. |
| list | Tüm gizli kaynakları listeler. |
| göster | Verilen ada sahip gizli kaynağı alır. |

## <a name="sfctl-mesh-secret-delete"></a>sfctl kafesi gizli silme
Gizli kaynağı siler.

Belirtilen gizli kaynağı ve tüm adlandırılmış değerlerini siler.

### <a name="arguments"></a>Bağımsız değişkenler

|Bağımsız Değişken|Description|
| --- | --- |
| --Name-n [gerekli] | Gizli kaynağın adı. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız Değişken|Description|
| --- | --- |
| --Hata Ayıkla | Tüm hata ayıklama günlüklerini göstermek için günlük ayrıntı düzeyini artırın. |
| --Yardım-h | Bu yardım iletisini gösterin ve çıkın. |
| --çıkış-o | Çıkış biçimi.  İzin verilen \: JSON, jsonc, tablo, TSV değerleri.  Varsayılan \: JSON. |
| --sorgu | JMESPath sorgu dizesi. \:Daha fazla bilgi ve örnek için bkz. http//jmespath.org/. |
| --ayrıntılı | Günlük ayrıntı düzeyini artırın. Tam hata ayıklama günlükleri için--Debug kullanın. |

## <a name="sfctl-mesh-secret-list"></a>sfctl kafesi gizli listesi
Tüm gizli kaynakları listeler.

Belirli bir kaynak grubundaki tüm gizli kaynaklarla ilgili bilgileri alır. Bilgiler, gizli dizinin açıklamasını ve diğer özelliklerini içerir.

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız Değişken|Description|
| --- | --- |
| --Hata Ayıkla | Tüm hata ayıklama günlüklerini göstermek için günlük ayrıntı düzeyini artırın. |
| --Yardım-h | Bu yardım iletisini gösterin ve çıkın. |
| --çıkış-o | Çıkış biçimi.  İzin verilen \: JSON, jsonc, tablo, TSV değerleri.  Varsayılan \: JSON. |
| --sorgu | JMESPath sorgu dizesi. \:Daha fazla bilgi ve örnek için bkz. http//jmespath.org/. |
| --ayrıntılı | Günlük ayrıntı düzeyini artırın. Tam hata ayıklama günlükleri için--Debug kullanın. |

## <a name="sfctl-mesh-secret-show"></a>sfctl kafesi gizli gösterme
Verilen ada sahip gizli kaynağı alır.

Verilen ada sahip gizli kaynakla ilgili bilgileri alır. Bilgiler, gizli dizinin açıklamasını ve diğer özelliklerini içerir.

### <a name="arguments"></a>Bağımsız değişkenler

|Bağımsız Değişken|Description|
| --- | --- |
| --Name-n [gerekli] | Gizli kaynağın adı. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız Değişken|Description|
| --- | --- |
| --Hata Ayıkla | Tüm hata ayıklama günlüklerini göstermek için günlük ayrıntı düzeyini artırın. |
| --Yardım-h | Bu yardım iletisini gösterin ve çıkın. |
| --çıkış-o | Çıkış biçimi.  İzin verilen \: JSON, jsonc, tablo, TSV değerleri.  Varsayılan \: JSON. |
| --sorgu | JMESPath sorgu dizesi. \:Daha fazla bilgi ve örnek için bkz. http//jmespath.org/. |
| --ayrıntılı | Günlük ayrıntı düzeyini artırın. Tam hata ayıklama günlükleri için--Debug kullanın. |


## <a name="next-steps"></a>Sonraki adımlar
- Service Fabric CLı 'yi [ayarlayın](service-fabric-cli.md) .
- [Örnek betikleri](./scripts/sfctl-upgrade-application.md)kullanarak Service Fabric CLI 'nın nasıl kullanılacağını öğrenin.
