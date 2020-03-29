---
title: Azure Servis Kumaş CLI- sfctl örgü secretvalue
description: Azure Service Fabric komut satırı arabirimi sfctl hakkında bilgi edinin. Hizmet Fabric Mesh gizli değer kaynakları almak ve silen komutların listesini içerir.
author: jeffj6123
ms.topic: reference
ms.date: 1/16/2020
ms.author: jejarry
ms.openlocfilehash: b2fe3a7995cdd88a8f63d0d865e72e9f161cf9d7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76905970"
---
# <a name="sfctl-mesh-secretvalue"></a>sfctl mesh secretvalue
Mesh secretvalue kaynaklarını alın ve silin.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| delete | Adlandırılmış gizli kaynağın belirtilen değerini siler. |
| list | Belirtilen gizli kaynağın tüm değerlerinin adlarını listele. |
| göster | Gizli kaynağın belirtilen değerini listeler. |

## <a name="sfctl-mesh-secretvalue-delete"></a>sfctl örgü secretvalue silme
Adlandırılmış gizli kaynağın belirtilen değerini siler.

Adla tanımlanan gizli değer kaynağını siler. Kaynağın adı genellikle bu değerle ilişkili sürümdür. Belirtilen değer kullanılıyorsa silme işlemi başarısız olur.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --gizli ad -n [Gerekli] | Gizli kaynağın adı. |
| --sürüm -v [Gerekli] | Gizli versiyonun adı. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-mesh-secretvalue-list"></a>sfctl örgü secretvalue listesi
Belirtilen gizli kaynağın tüm değerlerinin adlarını listele.

Belirtilen gizli kaynağın tüm gizli değer kaynakları hakkında bilgi alır. Bilgiler, gizli değer kaynaklarının adlarını içerir, ancak gerçek değerleri içermez.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --gizli ad -n [Gerekli] | Gizli kaynağın adı. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-mesh-secretvalue-show"></a>sfctl örgü secretvalue gösterisi
Gizli kaynağın belirtilen değerini listeler.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --gizli ad -n [Gerekli] | Gizli kaynağın adı. |
| --sürüm -v [Gerekli] | Gizli versiyonun adı. |
| --göster değeri | Gizli sürümün gerçek değerini gösterin. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |


## <a name="next-steps"></a>Sonraki adımlar
- Servis Kumaş ı CLI'yi [ayarlayın.](service-fabric-cli.md)
- [Örnek komut dosyalarını](/azure/service-fabric/scripts/sfctl-upgrade-application)kullanarak Service Fabric CLI'yi nasıl kullanacağınızı öğrenin.