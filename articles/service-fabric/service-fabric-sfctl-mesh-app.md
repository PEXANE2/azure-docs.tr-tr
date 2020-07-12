---
title: Azure Service Fabric CLı-sfctl kafes uygulaması
description: Azure Service Fabric komut satırı arabirimi olan sfctl hakkında bilgi edinin. Service Fabric mesh uygulama kaynaklarını yönetmeye yönelik komutların bir listesini içerir.
author: jeffj6123
ms.topic: reference
ms.date: 1/16/2020
ms.author: jejarry
ms.openlocfilehash: 835369116b07b74c666fba271476f1cba5a708b8
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2020
ms.locfileid: "86259944"
---
# <a name="sfctl-mesh-app"></a>sfctl mesh app
Uygulama kaynaklarını alın ve silin.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| silme | Uygulama kaynağını siler. |
| list | Tüm uygulama kaynaklarını listeler. |
| göster | Verilen ada sahip uygulama kaynağını alır. |

## <a name="sfctl-mesh-app-delete"></a>sfctl kafes uygulaması silme
Uygulama kaynağını siler.

Ad tarafından tanımlanan uygulama kaynağını siler.

### <a name="arguments"></a>Arguments

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Name-n [gerekli] | Uygulamanın adı. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Hata Ayıkla | Tüm hata ayıklama günlüklerini göstermek için günlük ayrıntı düzeyini artırın. |
| --Yardım-h | Bu yardım iletisini gösterin ve çıkın. |
| --çıkış-o | Çıkış biçimi.  İzin verilen \: JSON, jsonc, tablo, TSV değerleri.  Varsayılan \: JSON. |
| --sorgu | JMESPath sorgu dizesi. \:Daha fazla bilgi ve örnek için bkz. http//jmespath.org/. |
| --ayrıntılı | Günlük ayrıntı düzeyini artırın. Tam hata ayıklama günlükleri için--Debug kullanın. |

## <a name="sfctl-mesh-app-list"></a>sfctl kafes uygulama listesi
Tüm uygulama kaynaklarını listeler.

Belirli bir kaynak grubundaki tüm uygulama kaynaklarıyla ilgili bilgileri alır. Bilgiler, uygulamanın açıklamasını ve diğer özelliklerini içerir.

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Hata Ayıkla | Tüm hata ayıklama günlüklerini göstermek için günlük ayrıntı düzeyini artırın. |
| --Yardım-h | Bu yardım iletisini gösterin ve çıkın. |
| --çıkış-o | Çıkış biçimi.  İzin verilen \: JSON, jsonc, tablo, TSV değerleri.  Varsayılan \: JSON. |
| --sorgu | JMESPath sorgu dizesi. \:Daha fazla bilgi ve örnek için bkz. http//jmespath.org/. |
| --ayrıntılı | Günlük ayrıntı düzeyini artırın. Tam hata ayıklama günlükleri için--Debug kullanın. |

## <a name="sfctl-mesh-app-show"></a>sfctl kafes uygulaması göster
Verilen ada sahip uygulama kaynağını alır.

Verilen ada sahip uygulama kaynağıyla ilgili bilgileri alır. Bilgiler, uygulamanın açıklamasını ve diğer özelliklerini içerir.

### <a name="arguments"></a>Arguments

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Name-n [gerekli] | Uygulamanın adı. |

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
- [Örnek betikleri](./scripts/sfctl-upgrade-application.md)kullanarak Service Fabric CLI 'nın nasıl kullanılacağını öğrenin.
