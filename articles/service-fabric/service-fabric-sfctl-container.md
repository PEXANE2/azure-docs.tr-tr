---
title: Azure Service Fabric CLı-sfctl kapsayıcısı
description: Azure Service Fabric komut satırı arabirimi olan sfctl hakkında bilgi edinin. Kapsayıcılar için komutların bir listesini içerir.
author: jeffj6123
ms.topic: reference
ms.date: 1/16/2020
ms.author: jejarry
ms.openlocfilehash: 07861e2b67eea67740b341cbea994de978973664
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "76906123"
---
# <a name="sfctl-container"></a>sfctl container
Kapsayıcı ile ilgili komutları bir küme düğümünde çalıştırın.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| Invoke-API | Verilen kod paketi için bir Service Fabric düğümüne dağıtılan bir kapsayıcıda kapsayıcı API 'sini çağırma. |
| günlükler | Service Fabric düğümüne dağıtılan kapsayıcının kapsayıcı günlüklerini alır. |

## <a name="sfctl-container-invoke-api"></a>sfctl kapsayıcısı çağırma-API
Verilen kod paketi için bir Service Fabric düğümüne dağıtılan bir kapsayıcıda kapsayıcı API 'sini çağırma.

### <a name="arguments"></a>Arguments

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Uygulama kimliği [gerekli] | Uygulamanın kimliği. <br><br> Bu genellikle uygulamanın ' Fabric \: ' URI şeması olmadan tam adıdır. Sürüm 6,0 ' den başlayarak, hiyerarşik adlar " \~ " karakteriyle sınırlandırılmıştır. Örneğin, uygulama adı "Fabric \: /MyApp/APP1" ise, uygulama kimliği \~ önceki sürümlerde "MyApp APP1" ve 6.0 + "MyApp/APP1" şeklinde olur. |
| --Code-Package-Instance-id [gerekli] | Service Fabric düğümüne dağıtılan bir kod paketi örneğini benzersiz bir şekilde tanımlayan KIMLIK. <br><br> ' Service Code-Package-List ' tarafından alınabilir. |
| --Code-Package-Name [gerekli] | Hizmet bildiriminde belirtilen kod paketinin adı Service Fabric kümesindeki bir uygulama türünün bir parçası olarak kaydedilir. |
| --Container-api-Uri-Path [gerekli] | Kapsayıcı REST API URI yolu, kapsayıcı adı/kimliği yerine ' {ID} ' kullanın. |
| --Node-Name [gerekli] | Düğümün adı. |
| --hizmet-bildirim-adı [gerekli] | Service Fabric kümesinde uygulama türünün bir parçası olarak kaydedilmiş bir hizmet bildiriminin adı. |
| --Container-api-Body | Kapsayıcı REST API için HTTP istek gövdesi. |
| --Container-api-Content-Type | Kapsayıcı REST API için içerik türü, varsayılan olarak ' Application/JSON ' olarak belirlenmiştir. |
| --Container-api-http-fiil | Kapsayıcı REST API için HTTP fiili, varsayılan olarak al. |
| --timeout-t | Varsayılan \: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Hata Ayıkla | Tüm hata ayıklama günlüklerini göstermek için günlük ayrıntı düzeyini artırın. |
| --Yardım-h | Bu yardım iletisini gösterin ve çıkın. |
| --çıkış-o | Çıkış biçimi.  İzin verilen \: JSON, jsonc, tablo, TSV değerleri.  Varsayılan \: JSON. |
| --sorgu | JMESPath sorgu dizesi. \:Daha fazla bilgi ve örnek için bkz. http//jmespath.org/. |
| --ayrıntılı | Günlük ayrıntı düzeyini artırın. Tam hata ayıklama günlükleri için--Debug kullanın. |

## <a name="sfctl-container-logs"></a>sfctl kapsayıcı günlükleri
Service Fabric düğümüne dağıtılan kapsayıcının kapsayıcı günlüklerini alır.

### <a name="arguments"></a>Arguments

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Uygulama kimliği [gerekli] | Uygulamanın kimliği. <br><br> Bu genellikle uygulamanın ' Fabric \: ' URI şeması olmadan tam adıdır. Sürüm 6,0 ' den başlayarak, hiyerarşik adlar " \~ " karakteriyle sınırlandırılmıştır. Örneğin, uygulama adı "Fabric \: /MyApp/APP1" ise, uygulama kimliği \~ önceki sürümlerde "MyApp APP1" ve 6.0 + "MyApp/APP1" şeklinde olur. |
| --Code-Package-Instance-id [gerekli] | ' Hizmet kodu-paket-listesi ' tarafından alınabilecek kod paketi örnek KIMLIĞI. |
| --Code-Package-Name [gerekli] | Hizmet bildiriminde belirtilen kod paketinin adı Service Fabric kümesindeki bir uygulama türünün bir parçası olarak kaydedilir. |
| --Node-Name [gerekli] | Düğümün adı. |
| --hizmet-bildirim-adı [gerekli] | Service Fabric kümesinde uygulama türünün bir parçası olarak kaydedilmiş bir hizmet bildiriminin adı. |
| --Tail | Günlüklerin sonundan gösterilecek satır sayısı. Varsayılan değer 100 ' dir. Tüm günlükleri göstermek için ' All '. |
| --timeout-t | Varsayılan \: 60. |

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
- [Örnek betikleri](/azure/service-fabric/scripts/sfctl-upgrade-application)kullanarak Service Fabric CLI 'nın nasıl kullanılacağını öğrenin.