---
title: Azure Servis Kumaş CLI- sfctl konteyner
description: Azure Service Fabric komut satırı arabirimi sfctl hakkında bilgi edinin. Kapsayıcılar için komutların listesini içerir.
author: jeffj6123
ms.topic: reference
ms.date: 1/16/2020
ms.author: jejarry
ms.openlocfilehash: 07861e2b67eea67740b341cbea994de978973664
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76906123"
---
# <a name="sfctl-container"></a>sfctl container
Küme düğümünde kapsayıcı ile ilgili komutları çalıştırın.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| invoke-api | Verilen kod paketi için Hizmet Kumaşdüğümünde dağıtılan bir kapsayıcıda kapsayıcı API'sini çağırın. |
| günlükler | Servis Kumaşdüğümünde dağıtılan kapsayıcının konteyner günlüklerini alır. |

## <a name="sfctl-container-invoke-api"></a>sfctl konteyner invoke-api
Verilen kod paketi için Hizmet Kumaşdüğümünde dağıtılan bir kapsayıcıda kapsayıcı API'sini çağırın.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --uygulama kimliği [Gerekli] | Uygulamanın kimliği. <br><br> Bu genellikle 'kumaş'\:URI düzeni olmadan uygulamanın tam adıdır. Sürüm 6.0'dan başlayarak hiyerarşik adlar\~" " karakteriyle sınırlandırılır. Örneğin, uygulama adı "kumaş\:/myapp/app1" ise, uygulama kimliği 6.0+ ve önceki sürümlerde "myapp/app1" olarak "myapp\~app1" olacaktır. |
| --kod-paket-instance-id [Gerekli] | Hizmet dokusu düğümünde dağıtılan bir kod paketi örneğini benzersiz olarak tanımlayan kimlik. <br><br> 'Servis kodu paket listesi' ile alınabilir. |
| --kod-paket adı [Gerekli] | Hizmet Kumaşı kümesinde uygulama türünün bir parçası olarak kayıtlı hizmet bildiriminde belirtilen kod paketinin adı. |
| --konteyner-api-uri-yolu [Gerekli] | Konteyner REST API URI yolu, konteyner adı /id yerine '{ID}' kullanın. |
| --düğüm adı [Gerekli] | Düğümün adı. |
| --hizmet bildirimi-adı [Gerekli] | Hizmet Kumaşı kümesinde uygulama türünün bir parçası olarak kaydedilmiş bir hizmet bildiriminin adı. |
| --konteyner-api-gövde | KONTEYNER REST API için HTTP istek gövdesi. |
| --konteyner-api-içerik türü | Konteyner REST API için içerik türü, varsayılan olarak 'uygulama/json'. |
| --konteyner-api-http-fiil | Konteyner REST API için HTTP fiil, GET varsayılan. |
| --zaman ayarı -t | Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-container-logs"></a>sfctl konteyner günlükleri
Servis Kumaşdüğümünde dağıtılan kapsayıcının konteyner günlüklerini alır.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --uygulama kimliği [Gerekli] | Uygulamanın kimliği. <br><br> Bu genellikle 'kumaş'\:URI düzeni olmadan uygulamanın tam adıdır. Sürüm 6.0'dan başlayarak hiyerarşik adlar\~" " karakteriyle sınırlandırılır. Örneğin, uygulama adı "kumaş\:/myapp/app1" ise, uygulama kimliği 6.0+ ve önceki sürümlerde "myapp/app1" olarak "myapp\~app1" olacaktır. |
| --kod-paket-instance-id [Gerekli] | 'Servis kodu paket listesi' ile alınabilen kod paketi örnek kimliği. |
| --kod-paket adı [Gerekli] | Hizmet Kumaşı kümesinde uygulama türünün bir parçası olarak kayıtlı hizmet bildiriminde belirtilen kod paketinin adı. |
| --düğüm adı [Gerekli] | Düğümün adı. |
| --hizmet bildirimi-adı [Gerekli] | Hizmet Kumaşı kümesinde uygulama türünün bir parçası olarak kaydedilmiş bir hizmet bildiriminin adı. |
| --kuyruk | Günlüklerin sonundan gösterilen satır sayısı. Varsayılan değer 100'dür. 'tüm' tüm günlükleri göstermek için. |
| --zaman ayarı -t | Varsayılan\: 60. |

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