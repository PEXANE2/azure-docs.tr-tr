---
title: Azure Servis Kumaş CLI- sfctl özelliği
description: Azure Service Fabric komut satırı arabirimi sfctl hakkında bilgi edinin. Özellikleri depolamak ve sorgulamak için komutların listesini içerir.
author: jeffj6123
ms.topic: reference
ms.date: 1/16/2020
ms.author: jejarry
ms.openlocfilehash: a9bd75e0b7f8bfceb50a71ca83b60ff1e7b45508
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76905821"
---
# <a name="sfctl-property"></a>sfctl property
Hizmet Kumaşı adları altında özellikleri depolayın ve sorgulayın.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| delete | Belirtilen Hizmet Kumaşı özelliğini siler. |
| get | Belirtilen Service Fabric özelliğini alır. |
| list | Belirli bir ad altında tüm Service Fabric özellikleri hakkında bilgi alır. |
| Koymak | Bir Service Fabric özelliği oluşturur veya güncelleştirir. |

## <a name="sfctl-property-delete"></a>sfctl özellik silme
Belirtilen Hizmet Kumaşı özelliğini siler.

Belirtilen Hizmet Kumaşı özelliğini belirli bir ad altında siler. Bir özelliğin silinemeden önce oluşturulması gerekir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --isim-id [Gerekli] | Hizmet Kumaş adı, 'kumaş'\:URI düzeni olmadan. |
| --özellik adı [Gerekli] | Almak için özelliğin adını belirtir. |
| --zaman ayarı -t | İşlemi saniyeler içinde gerçekleştirmek için sunucu zaman. Bu zaman sonu, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametrenin varsayılan değeri 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-property-get"></a>sfctl özellik olsun
Belirtilen Service Fabric özelliğini alır.

Belirtilen Service Fabric özelliğini belirli bir ad altında alır. Bu her zaman hem değer hem de meta veri döndürecektir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --isim-id [Gerekli] | Hizmet Kumaş adı, 'kumaş'\:URI düzeni olmadan. |
| --özellik adı [Gerekli] | Almak için özelliğin adını belirtir. |
| --zaman ayarı -t | İşlemi saniyeler içinde gerçekleştirmek için sunucu zaman. Bu zaman sonu, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametrenin varsayılan değeri 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-property-list"></a>sfctl özellik listesi
Belirli bir ad altında tüm Service Fabric özellikleri hakkında bilgi alır.

Hizmet Kumaşı adında özel bilgiler depolayan bir veya daha fazla adlandırılmış özellik olabilir. Bu işlem, bu özellikler le ilgili bilgileri sayfalı bir listede alır. Bilgiler, özelliklerin her biri hakkında ad, değer ve meta verileri içerir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --isim-id [Gerekli] | Hizmet Kumaş adı, 'kumaş'\:URI düzeni olmadan. |
| --devam-belirteç | Devamı belirteç parametresi sonraki sonuç kümesini elde etmek için kullanılır. Sistemden elde edilen sonuçlar tek bir yanıta sığmıyorsa, BOŞ olmayan bir değere sahip bir devam belirteci API'nın yanıtına dahil edilir. Bu değer bir sonraki API çağrısına geçirildiğinde, API sonraki sonuç kümesini döndürür. Başka sonuç yoksa, devam belirteci bir değer içermez. Bu parametrenin değeri URL kodlanmış olmamalıdır. |
| --dahil-değerler | Döndürülen özelliklerin değerlerinin dahil edilip edilemeyeceğini belirtmeyi sağlar. Değerler meta verilerle döndürülecekse doğru; Yalnızca özellik meta verilerini döndürmek için yanlış. |
| --zaman ayarı -t | İşlemi saniyeler içinde gerçekleştirmek için sunucu zaman. Bu zaman sonu, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametrenin varsayılan değeri 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-property-put"></a>sfctl özellik koymak
Bir Service Fabric özelliği oluşturur veya güncelleştirir.

Belirtilen Service Fabric özelliğini belirli bir ad altında oluşturur veya güncelleştirir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --isim-id [Gerekli] | Hizmet Kumaş adı, 'kumaş'\:URI düzeni olmadan. |
| --özellik adı [Gerekli] | Service Fabric özelliğinin adı. |
| --değer [Gerekli] | Hizmet Kumaşı özellik değerini açıklar. Bu bir JSON ipi. <br><br> json dizesi, verilerin 'Türü' ve verilerin 'Veri' olarak girilen değeri olmak üzere iki alana sahiptir. 'Tür' değeri JSON dizesi'nde görünen ilk öğe olmalıdır ve 'İkili', 'Int64', 'Double', 'String' veya 'Guid' değerleri olabilir. Değer, verilen türlere serileştirilebilen olmalıdır. Hem 'Tür' hem de 'Veri' değerleri dizeleri olarak sağlanmalıdır. |
| --özel id türü | Tesisin özel tip kimliği. Bu özelliği kullanarak, kullanıcı özelliğinin değeri türünü etiketlemek mümkün. |
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