---
title: Azure Service Fabric CLı-sfctl özelliği
description: Azure Service Fabric komut satırı arabirimi olan sfctl hakkında bilgi edinin. Özellikleri depolamak ve sorgulamak için komutların bir listesini içerir.
author: jeffj6123
ms.topic: reference
ms.date: 1/16/2020
ms.author: jejarry
ms.openlocfilehash: a9bd75e0b7f8bfceb50a71ca83b60ff1e7b45508
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2020
ms.locfileid: "76905821"
---
# <a name="sfctl-property"></a>sfctl property
Service Fabric adları altında mağaza ve sorgu özellikleri.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| delete | Belirtilen Service Fabric özelliğini siler. |
| Al | Belirtilen Service Fabric özelliğini alır. |
| list | Verilen bir ad altındaki tüm Service Fabric özellikleriyle ilgili bilgileri alır. |
| konur | Service Fabric özelliği oluşturur veya güncelleştirir. |

## <a name="sfctl-property-delete"></a>sfctl özelliği silme
Belirtilen Service Fabric özelliğini siler.

Verilen bir ad altında belirtilen Service Fabric özelliğini siler. Bir özelliğin silinebilmesi için önce oluşturulması gerekir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --ad-kimliği [gerekli] | ' Fabric\:' URI düzeni olmadan Service Fabric adı. |
| --Özellik-adı [gerekli] | Alınacak özelliğin adını belirtir. |
| --timeout-t | İşlemi saniye cinsinden gerçekleştirmek için sunucu zaman aşımı. Bu zaman aşımı, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametre için varsayılan değer 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Hata Ayıkla | Tüm hata ayıklama günlüklerini göstermek için günlük ayrıntı düzeyini artırın. |
| --Yardım-h | Bu yardım iletisini gösterin ve çıkın. |
| --çıkış-o | Çıkış biçimi.  İzin verilen değerler JSON, jsonc, tablo, TSV\:.  JSON\: varsayılan. |
| --sorgu | JMESPath sorgu dizesi. Daha fazla bilgi ve örnek için bkz. http\://jmespath.org/. |
| --ayrıntılı | Günlük ayrıntı düzeyini artırın. Tam hata ayıklama günlükleri için--Debug kullanın. |

## <a name="sfctl-property-get"></a>sfctl özelliği al
Belirtilen Service Fabric özelliğini alır.

Verilen bir ad altında belirtilen Service Fabric özelliğini alır. Bu, her zaman hem değeri hem de meta verileri döndürür.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --ad-kimliği [gerekli] | ' Fabric\:' URI düzeni olmadan Service Fabric adı. |
| --Özellik-adı [gerekli] | Alınacak özelliğin adını belirtir. |
| --timeout-t | İşlemi saniye cinsinden gerçekleştirmek için sunucu zaman aşımı. Bu zaman aşımı, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametre için varsayılan değer 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Hata Ayıkla | Tüm hata ayıklama günlüklerini göstermek için günlük ayrıntı düzeyini artırın. |
| --Yardım-h | Bu yardım iletisini gösterin ve çıkın. |
| --çıkış-o | Çıkış biçimi.  İzin verilen değerler JSON, jsonc, tablo, TSV\:.  JSON\: varsayılan. |
| --sorgu | JMESPath sorgu dizesi. Daha fazla bilgi ve örnek için bkz. http\://jmespath.org/. |
| --ayrıntılı | Günlük ayrıntı düzeyini artırın. Tam hata ayıklama günlükleri için--Debug kullanın. |

## <a name="sfctl-property-list"></a>sfctl özellik listesi
Verilen bir ad altındaki tüm Service Fabric özellikleriyle ilgili bilgileri alır.

Bir Service Fabric ad, özel bilgileri depolayan bir veya daha fazla adlandırılmış özelliğe sahip olabilir. Bu işlem, bu özelliklerle ilgili bilgileri disk belleğine alınmış bir listede alır. Bilgiler, özelliklerin her biri hakkında ad, değer ve meta veriler içerir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --ad-kimliği [gerekli] | ' Fabric\:' URI düzeni olmadan Service Fabric adı. |
| --Devamlılık-belirteç | Devam belirteci parametresi, sonraki sonuç kümesini almak için kullanılır. Boş olmayan bir değere sahip devamlılık belirteci, sistemin sonuçları tek bir yanıta sığmıyor olduğunda API 'nin yanıtına dahil edilir. Bu değer sonraki API çağrısına geçirildiğinde, API sonraki sonuç kümesini döndürür. Başka sonuç yoksa devamlılık belirteci bir değer içermez. Bu parametrenin değeri, URL kodlamalı olmamalıdır. |
| --Include-Values | Döndürülen özelliklerin değerlerinin eklenip eklenmeyeceğini belirtmeye izin verir. Değerlerin meta verilerle döndürülmesi gerekiyorsa true; Yalnızca özellik meta verilerini döndürmek için false. |
| --timeout-t | İşlemi saniye cinsinden gerçekleştirmek için sunucu zaman aşımı. Bu zaman aşımı, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametre için varsayılan değer 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Hata Ayıkla | Tüm hata ayıklama günlüklerini göstermek için günlük ayrıntı düzeyini artırın. |
| --Yardım-h | Bu yardım iletisini gösterin ve çıkın. |
| --çıkış-o | Çıkış biçimi.  İzin verilen değerler JSON, jsonc, tablo, TSV\:.  JSON\: varsayılan. |
| --sorgu | JMESPath sorgu dizesi. Daha fazla bilgi ve örnek için bkz. http\://jmespath.org/. |
| --ayrıntılı | Günlük ayrıntı düzeyini artırın. Tam hata ayıklama günlükleri için--Debug kullanın. |

## <a name="sfctl-property-put"></a>sfctl özelliği koy
Service Fabric özelliği oluşturur veya güncelleştirir.

Verilen bir ad altında belirtilen Service Fabric özelliğini oluşturur veya güncelleştirir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --ad-kimliği [gerekli] | ' Fabric\:' URI düzeni olmadan Service Fabric adı. |
| --Özellik-adı [gerekli] | Service Fabric özelliğinin adı. |
| --değer [gerekli] | Service Fabric özellik değerini açıklar. Bu bir JSON dizesidir. <br><br> JSON dizesinde iki alan, verilerin ' Kind ' değeri ve değerin ' Data ' olarak girilen değeri. ' Kind ' değeri, JSON dizesinde görüntülenecek ilk öğe olmalıdır ve ' binary ', ' Int64 ', ' Double ', ' String ' veya ' Guid ' değerleri olabilir. Değer, verilen türlere serileştirilmelidir. Hem ' Kind ' hem de ' Data ' değerleri dize olarak sağlanmalıdır. |
| --Özel kimliği-tür | Özelliğin özel tür KIMLIĞI. Bu özelliği kullanarak, Kullanıcı özelliğin değerinin türünü etiketleyebilir. |
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