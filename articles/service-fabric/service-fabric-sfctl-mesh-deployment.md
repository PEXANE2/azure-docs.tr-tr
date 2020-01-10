---
title: Azure Service Fabric CLı-sfctl kafes dağıtımı
description: Azure Service Fabric komut satırı arabirimi olan sfctl hakkında bilgi edinin. Service Fabric kafes kaynakları oluşturmak için komutların bir listesini içerir.
author: jeffj6123
ms.topic: reference
ms.date: 9/17/2019
ms.author: jejarry
ms.openlocfilehash: 6512cce075906fc8708a39fa0747ec27be33e961
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2020
ms.locfileid: "75645370"
---
# <a name="sfctl-mesh-deployment"></a>sfctl mesh deployment
Service Fabric kafes kaynakları oluşturun.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| oluşturmaya | Service Fabric kafes kaynakları dağıtımı oluşturur. |

## <a name="sfctl-mesh-deployment-create"></a>sfctl kafes dağıtımı oluşturma
Service Fabric kafes kaynakları dağıtımı oluşturur.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Input-YAML-Files [gerekli] | Tüm YAML dosyalarının virgülle ayrılmış göreli/mutlak dosya yolları veya YAML dosyaları içeren dizinin (özyinelemeli) göreli/mutlak yolu. |
| --Parametreler | YAML dosyasına yönelik göreli/mutlak yol veya geçersiz kılınabilmesi gereken parametreleri içeren bir JSON nesnesi. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Hata Ayıkla | Tüm hata ayıklama günlüklerini göstermek için günlük ayrıntı düzeyini artırın. |
| --Yardım-h | Bu yardım iletisini gösterin ve çıkın. |
| --çıkış-o | Çıkış biçimi.  İzin verilen değerler JSON, jsonc, tablo, TSV\:.  JSON\: varsayılan. |
| --sorgu | JMESPath sorgu dizesi. Daha fazla bilgi ve örnek için bkz. http\://jmespath.org/. |
| --ayrıntılı | Günlük ayrıntı düzeyini artırın. Tam hata ayıklama günlükleri için--Debug kullanın. |

### <a name="examples"></a>Örnekler

YAML dosyasında bahsedilen parametreleri geçersiz kılarak kümeleyerek tüm kaynakları birleştirir ve dağıtır
``` 
sfctl mesh deployment create --input-yaml-files ./app.yaml,./network.yaml --parameters  
./param.yaml    
```

YAML dosyasında bahsedilen parametreleri geçersiz kılarak kümeleyerek bir dizindeki tüm kaynakları birleştirir ve dağıtır

``` 
sfctl mesh deployment create --input-yaml-files ./resources --parameters ./param.yaml
```

Doğrudan JSON nesnesi olarak geçirilen parametreleri geçersiz kılarak kümelemek için bir dizindeki tüm kaynakları birleştirir ve dağıtır
``` 
sfctl mesh deployment create --input-yaml-files ./resources --parameters "{ 'my_param' :    
{'value' : 'my_value'} }"   
```

## <a name="next-steps"></a>Sonraki adımlar
- Service Fabric CLı 'yi [ayarlayın](service-fabric-cli.md) .
- [Örnek betikleri](/azure/service-fabric/scripts/sfctl-upgrade-application)kullanarak Service Fabric CLI 'nın nasıl kullanılacağını öğrenin.