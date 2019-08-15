---
title: Azure Service Fabric CLı-sfctl ağ kodu-paket-günlük | Microsoft Docs
description: CLı sfctl ağ kodu-paket-günlük komutlarını Service Fabric açıklar.
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
ms.openlocfilehash: b1949f87dcdb1e3d9fe8e7fd08d8d8ba3b8203a0
ms.sourcegitcommit: 18061d0ea18ce2c2ac10652685323c6728fe8d5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/15/2019
ms.locfileid: "69036433"
---
# <a name="sfctl-mesh-code-package-log"></a>sfctl mesh code-package-log
Verilen hizmet çoğaltması için belirtilen kod paketinin kapsayıcısına ait günlükleri al.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| Al | Kapsayıcıdan günlükleri alır. |

## <a name="sfctl-mesh-code-package-log-get"></a>sfctl ağ kodu-paket-günlük al
Kapsayıcıdan günlükleri alır.

Hizmet çoğaltmasının belirtilen kod paketinin kapsayıcısına ait günlükleri alır.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --App-Name--uygulama-adı [gerekli] | Uygulamanın adı. |
| --Code-Package-Name [gerekli] | Hizmetin kod paketinin adı. |
| --çoğaltma-adı [gerekli] | Service Fabric çoğaltma adı. |
| --hizmet-adı [gerekli] | Hizmetin adı. |
| --Tail | Günlüklerin sonundan gösterilecek satır sayısı. Varsayılan değer 100 ' dir. Tüm günlükleri göstermek için ' All '. |

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