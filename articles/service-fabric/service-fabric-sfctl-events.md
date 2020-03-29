---
title: Azure Servis Kumaş CLI- sfctl etkinlikleri
description: Hizmet Kumaşı CLI sfctl olay komutlarını açıklar.
author: jeffj6123
ms.topic: reference
ms.date: 1/16/2020
ms.author: jejarry
ms.openlocfilehash: 635bddef1a98d6ed2b112662d8c9c44fe65e9e32
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76906097"
---
# <a name="sfctl-events"></a>sfctl olaylar
Olaylar mağazasından olayları alın (EventStore hizmeti zaten yüklüyse).

EventStore sistem hizmeti, =6,4 >çalışan herhangi bir SFRP kümesine config yükseltmesi yoluyla eklenebilir. Lütfen aşağıdaki https\: \://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-eventstore url'sini kontrol ediniz.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| tüm uygulamalar listesi | Uygulamalarla ilgili tüm olayları alır. |
| tüm düğümler listesi | Düğümlerle ilgili tüm Olayları alır. |
| tüm bölümler listesi | Bölümlerle ilgili tüm olayları alır. |
| tüm hizmetler listesi | Hizmetlerle ilgili tüm etkinlikleri alır. |
| uygulama listesi | Uygulama yla ilgili olayları alır. |
| küme listesi | Kümeyle ilgili tüm olayları alır. |
| düğüm listesi | Düğümle ilgili olaylar alır. |
| bölüm-tüm yinelemeler-listesi | Bir Bölüm için Yinelemelerle ilgili tüm olayları alır. |
| bölüm listesi | Bölümle ilgili olaylar alır. |
| bölüm çoğaltma-listesi | Bölüm Çoğaltma ile ilgili olaylar alır. |
| hizmet listesi | Hizmetle ilgili etkinlikler alır. |

## <a name="sfctl-events-all-applications-list"></a>sfctl olaylar tüm uygulamalar-listesi
Uygulamalarla ilgili tüm olayları alır.

Yanıt, ApplicationEvent nesnelerinin listesidir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --bitiş-zaman-utc [Gerekli] | ISO UTC yyyy-MM-ddTHH\:mm\:ssZ'deki bir arama sorgusunun bitiş saati. |
| --başlangıç-zaman-utc [Gerekli] | ISO UTC yyyy-MM-ddTHH\:mm\:ssZ'de bir arama sorgusunun başlangıç saati. |
| --olaylar-türleri-filtre | Bu, yalnızca yanıta eklenmesi gereken FabricEvents türlerini belirten virgülle ayrılmış bir dizedir. |
| --dışlama-analiz-olaylar | Bu param, doğru geçirilirse AnalysisEvents'in alınmasını devre dışı kılabilir. |
| --skip-correlation-lookup | Bu param, doğru geçirilirse CorrelatedEvents bilgilerinin aranmasını devre dışı kılabilir. aksi takdirde Korelasyon Etkinlikleri işlenir ve her FabricEvent'deki HasCorrelatedEvents alanı doldurulur. |
| --zaman ayarı -t | İşlemi saniyeler içinde gerçekleştirmek için sunucu zaman. Bu zaman sonu, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametrenin varsayılan değeri 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-events-all-nodes-list"></a>sfctl olaylar all-düğümler-listesi
Düğümlerle ilgili tüm Olayları alır.

Yanıt, NodeEvent nesnelerinin listesidir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --bitiş-zaman-utc [Gerekli] | ISO UTC yyyy-MM-ddTHH\:mm\:ssZ'deki bir arama sorgusunun bitiş saati. |
| --başlangıç-zaman-utc [Gerekli] | ISO UTC yyyy-MM-ddTHH\:mm\:ssZ'de bir arama sorgusunun başlangıç saati. |
| --olaylar-türleri-filtre | Bu, yalnızca yanıta eklenmesi gereken FabricEvents türlerini belirten virgülle ayrılmış bir dizedir. |
| --dışlama-analiz-olaylar | Bu param, doğru geçirilirse AnalysisEvents'in alınmasını devre dışı kılabilir. |
| --skip-correlation-lookup | Bu param, doğru geçirilirse CorrelatedEvents bilgilerinin aranmasını devre dışı kılabilir. aksi takdirde Korelasyon Etkinlikleri işlenir ve her FabricEvent'deki HasCorrelatedEvents alanı doldurulur. |
| --zaman ayarı -t | İşlemi saniyeler içinde gerçekleştirmek için sunucu zaman. Bu zaman sonu, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametrenin varsayılan değeri 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-events-all-partitions-list"></a>sfctl olaylar tüm bölümler-listesi
Bölümlerle ilgili tüm olayları alır.

Yanıt PartitionEvent nesnelerinin listesidir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --bitiş-zaman-utc [Gerekli] | ISO UTC yyyy-MM-ddTHH\:mm\:ssZ'deki bir arama sorgusunun bitiş saati. |
| --başlangıç-zaman-utc [Gerekli] | ISO UTC yyyy-MM-ddTHH\:mm\:ssZ'de bir arama sorgusunun başlangıç saati. |
| --olaylar-türleri-filtre | Bu, yalnızca yanıta eklenmesi gereken FabricEvents türlerini belirten virgülle ayrılmış bir dizedir. |
| --dışlama-analiz-olaylar | Bu param, doğru geçirilirse AnalysisEvents'in alınmasını devre dışı kılabilir. |
| --skip-correlation-lookup | Bu param, doğru geçirilirse CorrelatedEvents bilgilerinin aranmasını devre dışı kılabilir. aksi takdirde Korelasyon Etkinlikleri işlenir ve her FabricEvent'deki HasCorrelatedEvents alanı doldurulur. |
| --zaman ayarı -t | İşlemi saniyeler içinde gerçekleştirmek için sunucu zaman. Bu zaman sonu, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametrenin varsayılan değeri 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-events-all-services-list"></a>sfctl olaylar tüm hizmetler listesi
Hizmetlerle ilgili tüm etkinlikleri alır.

Yanıt ServiceEvent nesnelerinin listesidir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --bitiş-zaman-utc [Gerekli] | ISO UTC yyyy-MM-ddTHH\:mm\:ssZ'deki bir arama sorgusunun bitiş saati. |
| --başlangıç-zaman-utc [Gerekli] | ISO UTC yyyy-MM-ddTHH\:mm\:ssZ'de bir arama sorgusunun başlangıç saati. |
| --olaylar-türleri-filtre | Bu, yalnızca yanıta eklenmesi gereken FabricEvents türlerini belirten virgülle ayrılmış bir dizedir. |
| --dışlama-analiz-olaylar | Bu param, doğru geçirilirse AnalysisEvents'in alınmasını devre dışı kılabilir. |
| --skip-correlation-lookup | Bu param, doğru geçirilirse CorrelatedEvents bilgilerinin aranmasını devre dışı kılabilir. aksi takdirde Korelasyon Etkinlikleri işlenir ve her FabricEvent'deki HasCorrelatedEvents alanı doldurulur. |
| --zaman ayarı -t | İşlemi saniyeler içinde gerçekleştirmek için sunucu zaman. Bu zaman sonu, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametrenin varsayılan değeri 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-events-application-list"></a>sfctl olaylar uygulama listesi
Uygulama yla ilgili olayları alır.

Yanıt, ApplicationEvent nesnelerinin listesidir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --uygulama kimliği [Gerekli] | Uygulamanın kimliği. Bu genellikle 'kumaş'\:URI düzeni olmadan uygulamanın tam adıdır. Sürüm 6.0'dan başlayarak hiyerarşik adlar\~" " karakteriyle sınırlandırılır. Örneğin, uygulama adı "kumaş\:/myapp/app1" ise, uygulama kimliği 6.0+ ve önceki sürümlerde "myapp/app1" olarak "myapp\~app1" olacaktır. |
| --bitiş-zaman-utc [Gerekli] | ISO UTC yyyy-MM-ddTHH\:mm\:ssZ'deki bir arama sorgusunun bitiş saati. |
| --başlangıç-zaman-utc [Gerekli] | ISO UTC yyyy-MM-ddTHH\:mm\:ssZ'de bir arama sorgusunun başlangıç saati. |
| --olaylar-türleri-filtre | Bu, yalnızca yanıta eklenmesi gereken FabricEvents türlerini belirten virgülle ayrılmış bir dizedir. |
| --dışlama-analiz-olaylar | Bu param, doğru geçirilirse AnalysisEvents'in alınmasını devre dışı kılabilir. |
| --skip-correlation-lookup | Bu param, doğru geçirilirse CorrelatedEvents bilgilerinin aranmasını devre dışı kılabilir. aksi takdirde Korelasyon Etkinlikleri işlenir ve her FabricEvent'deki HasCorrelatedEvents alanı doldurulur. |
| --zaman ayarı -t | İşlemi saniyeler içinde gerçekleştirmek için sunucu zaman. Bu zaman sonu, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametrenin varsayılan değeri 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-events-cluster-list"></a>sfctl olaylar küme-listesi
Kümeyle ilgili tüm olayları alır.

Yanıt ClusterEvent nesnelerinin listesidir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --bitiş-zaman-utc [Gerekli] | ISO UTC yyyy-MM-ddTHH\:mm\:ssZ'deki bir arama sorgusunun bitiş saati. |
| --başlangıç-zaman-utc [Gerekli] | ISO UTC yyyy-MM-ddTHH\:mm\:ssZ'de bir arama sorgusunun başlangıç saati. |
| --olaylar-türleri-filtre | Bu, yalnızca yanıta eklenmesi gereken FabricEvents türlerini belirten virgülle ayrılmış bir dizedir. |
| --dışlama-analiz-olaylar | Bu param, doğru geçirilirse AnalysisEvents'in alınmasını devre dışı kılabilir. |
| --skip-correlation-lookup | Bu param, doğru geçirilirse CorrelatedEvents bilgilerinin aranmasını devre dışı kılabilir. aksi takdirde Korelasyon Etkinlikleri işlenir ve her FabricEvent'deki HasCorrelatedEvents alanı doldurulur. |
| --zaman ayarı -t | İşlemi saniyeler içinde gerçekleştirmek için sunucu zaman. Bu zaman sonu, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametrenin varsayılan değeri 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-events-node-list"></a>sfctl olaylar düğüm listesi
Düğümle ilgili olaylar alır.

Yanıt, NodeEvent nesnelerinin listesidir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --bitiş-zaman-utc [Gerekli] | ISO UTC yyyy-MM-ddTHH\:mm\:ssZ'deki bir arama sorgusunun bitiş saati. |
| --düğüm adı [Gerekli] | Düğümün adı. |
| --başlangıç-zaman-utc [Gerekli] | ISO UTC yyyy-MM-ddTHH\:mm\:ssZ'de bir arama sorgusunun başlangıç saati. |
| --olaylar-türleri-filtre | Bu, yalnızca yanıta eklenmesi gereken FabricEvents türlerini belirten virgülle ayrılmış bir dizedir. |
| --dışlama-analiz-olaylar | Bu param, doğru geçirilirse AnalysisEvents'in alınmasını devre dışı kılabilir. |
| --skip-correlation-lookup | Bu param, doğru geçirilirse CorrelatedEvents bilgilerinin aranmasını devre dışı kılabilir. aksi takdirde Korelasyon Etkinlikleri işlenir ve her FabricEvent'deki HasCorrelatedEvents alanı doldurulur. |
| --zaman ayarı -t | İşlemi saniyeler içinde gerçekleştirmek için sunucu zaman. Bu zaman sonu, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametrenin varsayılan değeri 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-events-partition-all-replicas-list"></a>sfctl olaylar bölüm-tüm yinelemeler-listesi
Bir Bölüm için Yinelemelerle ilgili tüm olayları alır.

Yanıt, Çoğaltma Olayı nesnelerinin listesidir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --bitiş-zaman-utc [Gerekli] | ISO UTC yyyy-MM-ddTHH\:mm\:ssZ'deki bir arama sorgusunun bitiş saati. |
| --partition-id [Gerekli] | Bölümün kimliği. |
| --başlangıç-zaman-utc [Gerekli] | ISO UTC yyyy-MM-ddTHH\:mm\:ssZ'de bir arama sorgusunun başlangıç saati. |
| --olaylar-türleri-filtre | Bu, yalnızca yanıta eklenmesi gereken FabricEvents türlerini belirten virgülle ayrılmış bir dizedir. |
| --dışlama-analiz-olaylar | Bu param, doğru geçirilirse AnalysisEvents'in alınmasını devre dışı kılabilir. |
| --skip-correlation-lookup | Bu param, doğru geçirilirse CorrelatedEvents bilgilerinin aranmasını devre dışı kılabilir. aksi takdirde Korelasyon Etkinlikleri işlenir ve her FabricEvent'deki HasCorrelatedEvents alanı doldurulur. |
| --zaman ayarı -t | İşlemi saniyeler içinde gerçekleştirmek için sunucu zaman. Bu zaman sonu, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametrenin varsayılan değeri 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-events-partition-list"></a>sfctl olaylar bölüm-listesi
Bölümle ilgili olaylar alır.

Yanıt PartitionEvent nesnelerinin listesidir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --bitiş-zaman-utc [Gerekli] | ISO UTC yyyy-MM-ddTHH\:mm\:ssZ'deki bir arama sorgusunun bitiş saati. |
| --partition-id [Gerekli] | Bölümün kimliği. |
| --başlangıç-zaman-utc [Gerekli] | ISO UTC yyyy-MM-ddTHH\:mm\:ssZ'de bir arama sorgusunun başlangıç saati. |
| --olaylar-türleri-filtre | Bu, yalnızca yanıta eklenmesi gereken FabricEvents türlerini belirten virgülle ayrılmış bir dizedir. |
| --dışlama-analiz-olaylar | Bu param, doğru geçirilirse AnalysisEvents'in alınmasını devre dışı kılabilir. |
| --skip-correlation-lookup | Bu param, doğru geçirilirse CorrelatedEvents bilgilerinin aranmasını devre dışı kılabilir. aksi takdirde Korelasyon Etkinlikleri işlenir ve her FabricEvent'deki HasCorrelatedEvents alanı doldurulur. |
| --zaman ayarı -t | İşlemi saniyeler içinde gerçekleştirmek için sunucu zaman. Bu zaman sonu, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametrenin varsayılan değeri 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-events-partition-replica-list"></a>sfctl olaylar bölüm-çoğaltma-listesi
Bölüm Çoğaltma ile ilgili olaylar alır.

Yanıt, Çoğaltma Olayı nesnelerinin listesidir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --bitiş-zaman-utc [Gerekli] | ISO UTC yyyy-MM-ddTHH\:mm\:ssZ'deki bir arama sorgusunun bitiş saati. |
| --partition-id [Gerekli] | Bölümün kimliği. |
| --çoğaltma-id [Gerekli] | Çoğaltmanın tanımlayıcısı. |
| --başlangıç-zaman-utc [Gerekli] | ISO UTC yyyy-MM-ddTHH\:mm\:ssZ'de bir arama sorgusunun başlangıç saati. |
| --olaylar-türleri-filtre | Bu, yalnızca yanıta eklenmesi gereken FabricEvents türlerini belirten virgülle ayrılmış bir dizedir. |
| --dışlama-analiz-olaylar | Bu param, doğru geçirilirse AnalysisEvents'in alınmasını devre dışı kılabilir. |
| --skip-correlation-lookup | Bu param, doğru geçirilirse CorrelatedEvents bilgilerinin aranmasını devre dışı kılabilir. aksi takdirde Korelasyon Etkinlikleri işlenir ve her FabricEvent'deki HasCorrelatedEvents alanı doldurulur. |
| --zaman ayarı -t | İşlemi saniyeler içinde gerçekleştirmek için sunucu zaman. Bu zaman sonu, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametrenin varsayılan değeri 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-events-service-list"></a>sfctl olaylar hizmet listesi
Hizmetle ilgili etkinlikler alır.

Yanıt ServiceEvent nesnelerinin listesidir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --bitiş-zaman-utc [Gerekli] | ISO UTC yyyy-MM-ddTHH\:mm\:ssZ'deki bir arama sorgusunun bitiş saati. |
| --service-id [Gerekli] | Hizmetin kimliği. Bu kimlik genellikle 'kumaş'\:URI düzeni olmadan hizmetin tam adıdır. Sürüm 6.0'dan başlayarak hiyerarşik adlar\~" " karakteriyle sınırlandırılır. Örneğin, hizmet adı "kumaş\:/myapp/app1/svc1" ise, hizmet kimliği 6.0+ ve önceki sürümlerde "myapp/app1/svc1"\~\~olacaktır. |
| --başlangıç-zaman-utc [Gerekli] | ISO UTC yyyy-MM-ddTHH\:mm\:ssZ'de bir arama sorgusunun başlangıç saati. |
| --olaylar-türleri-filtre | Bu, yalnızca yanıta eklenmesi gereken FabricEvents türlerini belirten virgülle ayrılmış bir dizedir. |
| --dışlama-analiz-olaylar | Bu param, doğru geçirilirse AnalysisEvents'in alınmasını devre dışı kılabilir. |
| --skip-correlation-lookup | Bu param, doğru geçirilirse CorrelatedEvents bilgilerinin aranmasını devre dışı kılabilir. aksi takdirde Korelasyon Etkinlikleri işlenir ve her FabricEvent'deki HasCorrelatedEvents alanı doldurulur. |
| --zaman ayarı -t | İşlemi saniyeler içinde gerçekleştirmek için sunucu zaman. Bu zaman sonu, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametrenin varsayılan değeri 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

