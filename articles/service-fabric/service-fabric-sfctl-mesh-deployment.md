---
title: Azure Servis Kumaş CLI- sfctl örgü dağıtım
description: Azure Hizmet Kumaşı komut satırı arabirimi sfctl hakkında bilgi edinin. Service Fabric Mesh kaynakları oluşturmak için komutların listesini içerir.
author: jeffj6123
ms.topic: reference
ms.date: 1/16/2020
ms.author: jejarry
ms.openlocfilehash: 108389407221779ed20e81310f084b7b5c23b8c7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76906032"
---
# <a name="sfctl-mesh-deployment"></a>sfctl mesh deployment
Hizmet Kumaş Kafeskaynakları oluşturun.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| oluşturmaya | Hizmet Kumaş ı Sağı Kaynaklarının dağıtımını oluşturur. |

## <a name="sfctl-mesh-deployment-create"></a>sfctl örgü dağıtım oluşturma
Hizmet Kumaş ı Sağı Kaynaklarının dağıtımını oluşturur.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --input-yaml-files [Gerekli] | Virgülden ayrılmış göreli veya mutlak dosya yolları tüm yaml dosyaları veya yaml dosyaları içeren dizinin göreli veya mutlak yolu (özyinelemeli). |
| --parametreler | Bir yaml dosyasına veya geçersiz kılınması gereken parametreleri içeren bir json nesnesine göreli veya mutlak bir yol. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

### <a name="examples"></a>Örnekler

Yaml dosyasında belirtilen parametreleri geçersiz kılarak tüm kaynakları kümelemek için birleştirir ve dağır
``` 
sfctl mesh deployment create --input-yaml-files ./app.yaml,./network.yaml --parameters  
./param.yaml    
```

Yaml dosyasında belirtilen parametreleri geçersiz kılarak tüm kaynakları kümelemek için bir dizindeki tüm kaynakları birleştirir ve dağıtir

``` 
sfctl mesh deployment create --input-yaml-files ./resources --parameters ./param.yaml
```

Json nesnesi olarak doğrudan geçirilen parametreleri geçersiz kılarak kümelemek için dizindeki tüm kaynakları birleştirir ve dağıtir
``` 
sfctl mesh deployment create --input-yaml-files ./resources --parameters "{ 'my_param' :    
{'value' : 'my_value'} }"   
```

## <a name="next-steps"></a>Sonraki adımlar
- Servis Kumaş ı CLI'yi [ayarlayın.](service-fabric-cli.md)
- [Örnek komut dosyalarını](/azure/service-fabric/scripts/sfctl-upgrade-application)kullanarak Service Fabric CLI'yi nasıl kullanacağınızı öğrenin.
