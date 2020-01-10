---
title: Azure Service Fabric CLı-sfctl kapsayıcısı
description: Azure Service Fabric komut satırı arabirimi olan sfctl hakkında bilgi edinin. Kapsayıcılar için komutların bir listesini içerir.
author: jeffj6123
ms.topic: reference
ms.date: 9/17/2019
ms.author: jejarry
ms.openlocfilehash: 316715e65af07176b3dc276d47979a5fa3dc1d6f
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2020
ms.locfileid: "75639148"
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

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Uygulama kimliği [gerekli] | Uygulamanın kimliği. <br><br> Bu genellikle uygulamanın ' Fabric\:' URI düzeni olmadan tam adıdır. 6,0 sürümünden başlayarak, hiyerarşik adlar "\~" karakteriyle sınırlandırılmıştır. Örneğin, uygulama adı "Fabric\:/MyApp/APP1" ise, uygulama kimliği önceki sürümlerde 6.0 + ve "MyApp/APP1" içinde "MyApp\~APP1" olacaktır. |
| --Code-Package-Instance-id [gerekli] | Service Fabric düğümüne dağıtılan bir kod paketi örneğini benzersiz bir şekilde tanımlayan KIMLIK. <br><br> ' Service Code-Package-List ' tarafından alınabilir. |
| --Code-Package-Name [gerekli] | Hizmet bildiriminde belirtilen kod paketinin adı Service Fabric kümesindeki bir uygulama türünün bir parçası olarak kaydedilir. |
| --Container-api-Uri-Path [gerekli] | Kapsayıcı REST API URI yolu, kapsayıcı adı/kimliği yerine ' {ID} ' kullanın. |
| --Node-Name [gerekli] | Düğümün adı. |
| --hizmet-bildirim-adı [gerekli] | Service Fabric kümesinde uygulama türünün bir parçası olarak kaydedilmiş bir hizmet bildiriminin adı. |
| --Container-api-Body | Kapsayıcı REST API için HTTP istek gövdesi. |
| --Container-api-Content-Type | Kapsayıcı REST API için içerik türü, varsayılan olarak ' Application/JSON ' olarak belirlenmiştir. |
| --Container-api-http-fiil | Kapsayıcı REST API için HTTP fiili, varsayılan olarak al. |
| --timeout-t | Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Hata Ayıkla | Tüm hata ayıklama günlüklerini göstermek için günlük ayrıntı düzeyini artırın. |
| --Yardım-h | Bu yardım iletisini gösterin ve çıkın. |
| --çıkış-o | Çıkış biçimi.  İzin verilen değerler JSON, jsonc, tablo, TSV\:.  JSON\: varsayılan. |
| --sorgu | JMESPath sorgu dizesi. Daha fazla bilgi ve örnek için bkz. http\://jmespath.org/. |
| --ayrıntılı | Günlük ayrıntı düzeyini artırın. Tam hata ayıklama günlükleri için--Debug kullanın. |

## <a name="sfctl-container-logs"></a>sfctl kapsayıcı günlükleri
Service Fabric düğümüne dağıtılan kapsayıcının kapsayıcı günlüklerini alır.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Uygulama kimliği [gerekli] | Uygulamanın kimliği. <br><br> Bu genellikle uygulamanın ' Fabric\:' URI düzeni olmadan tam adıdır. 6,0 sürümünden başlayarak, hiyerarşik adlar "\~" karakteriyle sınırlandırılmıştır. Örneğin, uygulama adı "Fabric\:/MyApp/APP1" ise, uygulama kimliği önceki sürümlerde 6.0 + ve "MyApp/APP1" içinde "MyApp\~APP1" olacaktır. |
| --Code-Package-Instance-id [gerekli] | ' Hizmet kodu-paket-listesi ' tarafından alınabilecek kod paketi örnek KIMLIĞI. |
| --Code-Package-Name [gerekli] | Hizmet bildiriminde belirtilen kod paketinin adı Service Fabric kümesindeki bir uygulama türünün bir parçası olarak kaydedilir. |
| --Node-Name [gerekli] | Düğümün adı. |
| --hizmet-bildirim-adı [gerekli] | Service Fabric kümesinde uygulama türünün bir parçası olarak kaydedilmiş bir hizmet bildiriminin adı. |
| --Tail | Günlüklerin sonundan gösterilecek satır sayısı. Varsayılan değer 100 ' dir. Tüm günlükleri göstermek için ' All '. |
| --timeout-t | Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Hata Ayıkla | Tüm hata ayıklama günlüklerini göstermek için günlük ayrıntı düzeyini artırın. |
| --Yardım-h | Bu yardım iletisini gösterin ve çıkın. |
| --çıkış-o | Çıkış biçimi.  İzin verilen değerler JSON, jsonc, tablo, TSV\:.  JSON\: varsayılan. |
| --sorgu | JMESPath sorgu dizesi. Daha fazla bilgi ve örnek için bkz. http\://jmespath.org/. |
| --ayrıntılı | Günlük ayrıntı düzeyini artırın. Tam hata ayıklama günlükleri için--Debug kullanın. |


## <a name="next-steps"></a>Sonraki adımlar
- Service Fabric CLı 'yi [ayarlayın](service-fabric-cli.md) .
- [Örnek betikleri](/azure/service-fabric/scripts/sfctl-upgrade-application)kullanarak Service Fabric CLI 'nın nasıl kullanılacağını öğrenin.