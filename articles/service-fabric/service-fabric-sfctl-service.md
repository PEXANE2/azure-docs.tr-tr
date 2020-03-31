---
title: Azure Servis Kumaş CLI- sfctl hizmeti
description: Azure Service Fabric komut satırı arabirimi sfctl hakkında bilgi edinin. Hizmetleri, hizmet türlerini ve hizmet paketlerini yönetmekomutlarının listesini içerir.
author: jeffj6123
ms.topic: reference
ms.date: 1/16/2020
ms.author: jejarry
ms.openlocfilehash: 696de713129ca71dd7f2451501a7cc9eca0ee9b9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76906227"
---
# <a name="sfctl-service"></a>sfctl service
Hizmet, hizmet türleri ve hizmet paketleri oluşturun, silin ve yönetin.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| uygulama adı | Hizmet için Service Fabric uygulamasının adını alır. |
| kod paketi-listesi | Hizmet Kumaşdüğümünde dağıtılan kod paketlerinin listesini alır. |
| oluşturmaya | Belirtilen Service Fabric hizmetini oluşturur. |
| delete | Varolan bir Hizmet Kumaşı hizmetini siler. |
| dağıtılan tür | Hizmet Kumaşı kümesindeki bir düğümüzerinde dağıtılan uygulamanın belirli bir hizmet türü hakkındaki bilgileri alır. |
| dağıtılan tür listesi | Hizmet Kumaşı kümesinde düğümüzerinde dağıtılan uygulamalardan hizmet türleri hakkındaki bilgileri içeren listeyi alır. |
| açıklama | Varolan bir Service Fabric hizmetinin açıklamasını alır. |
| get-konteyner günlükleri | Servis Kumaşdüğümünde dağıtılan kapsayıcının konteyner günlüklerini alır. |
| Sağlık | Belirtilen Servis Kumaşı hizmetinin sağlığını alır. |
| bilgiler | Service Fabric uygulamasına ait özel hizmet hakkında bilgi alır. |
| list | Uygulama kimliği ile belirtilen uygulamaya ait tüm hizmetler hakkında bilgi alır. |
| bildirim | Bir hizmet türünü açıklayan bildirimi alır. |
| paket dağıtma | Belirtilen hizmetle ilişkili paketleri, belirtilen düğümdeki görüntü önbelleğine indirir. |
| paket sağlığı | Hizmet Kumaş düğümü ve uygulaması için dağıtılan belirli bir uygulama için bir hizmet paketinin durumu yla ilgili bilgileri alır. |
| paket bilgileri | Hizmet Kumaşı düğümünde dağıtılan hizmet paketlerinin listesini tam olarak belirtilen ada uygun hale alır. |
| paket listesi | Hizmet Kumaşdüğümünde dağıtılan hizmet paketlerinin listesini alır. |
| kurtarma | Hizmet Kumaşı kümesine, şu anda çoğunluk kaybına bağlı olan belirtilen hizmeti kurtarmaya çalışması gerektiğini gösterir. |
| rapor-sağlık | Service Fabric hizmeti hakkında bir sağlık raporu gönderir. |
| Gidermek | Hizmet Kumaşı bölmeyi çözümle. |
| tür listesi | Hizmet Kumaşı kümesinde sağlanan bir uygulama türü tarafından desteklenen hizmet türleri hakkındaki bilgileri içeren listeyi alır. |
| update | Verilen güncelleştirme açıklamasını kullanarak belirtilen hizmeti güncelleştirir. |

## <a name="sfctl-service-app-name"></a>sfctl hizmet uygulama adı
Hizmet için Service Fabric uygulamasının adını alır.

Belirtilen hizmet için uygulamanın adını alır. Sağlanan hizmet kimliğine sahip bir hizmet yoksa, 404 FABRIC_E_SERVICE_DOES_NOT_EXIST hatası döndürülür.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --service-id [Gerekli] | Hizmetin kimliği. Bu kimlik genellikle 'kumaş'\:URI düzeni olmadan hizmetin tam adıdır. Sürüm 6.0'dan başlayarak hiyerarşik adlar\~" " karakteriyle sınırlandırılır. Örneğin, hizmet adı "kumaş\:/myapp/app1/svc1" ise, hizmet kimliği 6.0+ ve önceki sürümlerde "myapp/app1/svc1"\~\~olacaktır. |
| --zaman ayarı -t | İşlemi saniyeler içinde gerçekleştirmek için sunucu zaman. Bu zaman sonu, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametrenin varsayılan değeri 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-service-code-package-list"></a>sfctl hizmet kodu-paket listesi
Hizmet Kumaşdüğümünde dağıtılan kod paketlerinin listesini alır.

Verilen uygulama için Hizmet Kumaşdüğümünde dağıtılan kod paketlerinin listesini alır.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --uygulama kimliği [Gerekli] | Uygulamanın kimliği. Bu genellikle 'kumaş'\:URI düzeni olmadan uygulamanın tam adıdır. Sürüm 6.0'dan başlayarak hiyerarşik adlar\~" " karakteriyle sınırlandırılır. Örneğin, uygulama adı "kumaş\:/myapp/app1" ise, uygulama kimliği 6.0+ ve önceki sürümlerde "myapp/app1" olarak "myapp\~app1" olacaktır. |
| --düğüm adı [Gerekli] | Düğümün adı. |
| --kod-paket adı | Hizmet Kumaşı kümesinde uygulama türünün bir parçası olarak kayıtlı hizmet bildiriminde belirtilen kod paketinin adı. |
| --hizmet bildirimi-adı | Hizmet Kumaşı kümesinde uygulama türünün bir parçası olarak kaydedilmiş bir hizmet bildiriminin adı. |
| --zaman ayarı -t | İşlemi saniyeler içinde gerçekleştirmek için sunucu zaman. Bu zaman sonu, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametrenin varsayılan değeri 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-service-create"></a>sfctl hizmet oluşturmak
Belirtilen Service Fabric hizmetini oluşturur.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --app-id [Gerekli] | Uygulamanın kimliği. Bu genellikle 'kumaş'\:URI düzeni olmadan uygulamanın tam adıdır. Sürüm 6.0'dan başlayarak hiyerarşik adlar\~' ' karakteriyle sınırlandırılır. Örneğin, uygulama adı 'kumaş\:/myapp/app1' ise, uygulama kimliği 6.0+ ve önceki sürümlerde 'myapp/app1' olarak 'myapp/app1'\~olacaktır. |
| --isim [Gerekli] | Hizmetin adı. Bu, uygulama kimliğinin bir alt olmalıdır. Bu URI de dahil `fabric\:` olmak üzere tam adıdır. Örneğin hizmet `fabric\:/A/B` bir uygulama `fabric\:/A`nın alt. |
| --hizmet türü [Gerekli] | Hizmet türünün adı. |
| --aktivasyon modu | Servis paketi için etkinleştirme modu. |
| --kısıtlamalar | Bir dize olarak yerleşim kısıtlamaları. Yerleşim kısıtlamaları düğüm özellikleri üzerinde boolean ifadeler ve hizmet gereksinimlerine göre belirli düğümleri bir hizmet kısıtlamak için izin verir. Örneğin, Düğüm Tipi'nin mavi olduğu düğümlere bir hizmet\:yerleştirmek için aşağıdaki "NodeColor == mavi" belirtin. |
| --koreilişkili hizmet | İlişkilendirilmesi hedef hizmetin adı. |
| --korelasyon | Bir hizalama afinitesini kullanarak hizmeti varolan bir hizmetle ilişkilendirin. |
| --dns-adı | Oluşturulacak hizmetin DNS adı. Bu ayar için Service Fabric DNS sistem hizmeti etkinleştirilmelidir. |
| --örnek sayısı | Örnek sayısı. Bu yalnızca devletsiz hizmetler için geçerlidir. |
| --int-şeması | Hizmetin imzasız tamsayılar aralığına eşit olarak bölünmesi gerektiğini gösterir. |
| --int-şema-sayısı | Tek tip bir tamsayı bölüm düzeni kullanıyorsanız, oluşturmak için tamsayı anahtar aralığındaki bölüm sayısı. |
| --int-şeması-yüksek | Tek tip bir arameger bölme düzeni kullanıyorsanız, anahtar arayı sonu. |
| --int-şeması-düşük | Tek tip bir arameger bölme düzeni kullanıyorsanız, anahtar arayı başlangıcı. |
| --yük ölçümleri | JSON, düğümler arasında yük dengeleme hizmetleri kullanılırken kullanılan ölçümlerin listesini kodladı. |
| --min-çoğaltma-set-boyutu | En az yineleme bir sayı olarak boyut kümesi. Bu yalnızca devlet hizmetleri için geçerlidir. |
| --taşıma maliyeti | Hizmetin taşıma maliyetini belirtir. Olası değerler\: 'Sıfır', 'Düşük', 'Orta', 'Yüksek', 'Çok Yüksek'tir. |
| --adlı şema | Hizmetin birden çok adlandırılmış bölümü olması gerektiğini gösterir. |
| --adlı şema-listesi | JSON, adlandırılmış bölüm düzenini kullanıyorsa, hizmeti tüm servise bölmek için ad listesini kodladı. |
| --devam sız-durum | Doğruysa, bu, hizmetin yerel diskte depolanan kalıcı bir durum olmadığını veya yalnızca durumu bellekte depoladığını gösterir. |
| --yerleşim ilkesi listesi | JSON, hizmetiçin yerleşim ilkeleri ve ilişkili alan adları listesini kodladı. İlkeler bir veya daha\: `NonPartiallyPlaceService`fazla `PreferPrimaryDomain` `RequireDomain`olabilir `RequireDomainDistribution`, , . |
| --çoğunluk-kayıp-bekleme | Bir bölümün çoğunluk kaybı durumunda olması için izin verilen en uzun süre, saniye cinsinden. Bu yalnızca devlet hizmetleri için geçerlidir. |
| --çoğaltma-yeniden başlatma-bekleme | Bir yinelemenin aşağı inip yeni bir yinelemenin oluşturulması arasındaki süre saniye cinsinden. Bu yalnızca devlet hizmetleri için geçerlidir. |
| --ölçekleme ilkeleri | JSON, bu hizmet için ölçekleme ilkelerinin listesini kodladı. |
| --hizmet yerleştirme süresi | Kopyaların, bu yapıyı bildirmeden önce InBuild'te kalabilecekleri süre sıkışmıştır. Bu yalnızca devlet hizmetleri için geçerlidir. |
| --singleton şeması | Hizmetin tek bir bölümü olması veya bölümlenmemiş bir hizmet olması gerektiğini gösterir. |
| --stand-by-çoğaltma-tutmak | StandBy yinelemelerinin kaldırılmadan önce tutulacağı maksimum süre saniye cinsinden. Bu yalnızca devlet hizmetleri için geçerlidir. |
| --devletli | Hizmetin devlet hizmeti olduğunu gösterir. |
| --devletsiz | Hizmetin devletsiz bir hizmet olduğunu gösterir. |
| --hedef-çoğaltma-ayar-boyutu | Hedef çoğaltma boyutu bir sayı olarak ayarlayın. Bu yalnızca devlet hizmetleri için geçerlidir. |
| --zaman ayarı -t | Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-service-delete"></a>sfctl hizmet silme
Varolan bir Hizmet Kumaşı hizmetini siler.

Bir hizmetin silinemeden önce oluşturulması gerekir. Varsayılan olarak, Service Fabric hizmet yinelemelerini zarif bir şekilde kapatmaya ve ardından hizmeti silmeye çalışır. Ancak, hizmet incelikli çoğaltma kapatma sorunları yaşıyorsanız, silme işlemi uzun zaman alabilir veya sıkışmış olsun. Zarif kapanış sırasını atlamak ve hizmeti zorla silmek için isteğe bağlı ForceRemove bayrağını kullanın.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --service-id [Gerekli] | Hizmetin kimliği. Bu kimlik genellikle 'kumaş'\:URI düzeni olmadan hizmetin tam adıdır. Sürüm 6.0'dan başlayarak hiyerarşik adlar\~" " karakteriyle sınırlandırılır. Örneğin, hizmet adı "kumaş\:/myapp/app1/svc1" ise, hizmet kimliği 6.0+ ve önceki sürümlerde "myapp/app1/svc1"\~\~olacaktır. |
| --kuvvet kaldırma | Zarif kapatma sırasını geçmeden bir Hizmet Kumaşı uygulamasını veya hizmetini zorla kaldırın. Bu parametre, hizmet kodunda yinelemelerin zarif bir şekilde kapanmasını engelleyen sorunlar nedeniyle silmenin zamanlaması olan bir uygulamayı veya hizmeti zorla silmek için kullanılabilir. |
| --zaman ayarı -t | İşlemi saniyeler içinde gerçekleştirmek için sunucu zaman. Bu zaman sonu, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametrenin varsayılan değeri 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-service-deployed-type"></a>sfctl hizmet dağıtılmış türü
Hizmet Kumaşı kümesindeki bir düğümüzerinde dağıtılan uygulamanın belirli bir hizmet türü hakkındaki bilgileri alır.

Hizmet Kumaşı kümesindeki bir düğümüzerinde dağıtılan uygulamalardan belirli bir hizmet türüyle ilgili bilgileri içeren listeyi alır. Yanıt, hizmet türünün adını, kayıt durumunu, onu kaydeden kod paketini ve hizmet paketinin etkinleştirme kimliğini içerir. Her giriş, etkinleştirme kimliğiyle farklılaştırılan bir hizmet türünün etkinleştirmesini temsil eder.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --uygulama kimliği [Gerekli] | Uygulamanın kimliği. Bu genellikle 'kumaş'\:URI düzeni olmadan uygulamanın tam adıdır. Sürüm 6.0'dan başlayarak hiyerarşik adlar\~" " karakteriyle sınırlandırılır. Örneğin, uygulama adı "kumaş\:/myapp/app1" ise, uygulama kimliği 6.0+ ve önceki sürümlerde "myapp/app1" olarak "myapp\~app1" olacaktır. |
| --düğüm adı [Gerekli] | Düğümün adı. |
| --hizmet türü adı [Gerekli] | Service Fabric servis türünün adını belirtir. |
| --hizmet bildirimi-adı | Dağıtılan hizmet türü bilgilerinin listesini filtrelemek için hizmet bildiriminin adı. Belirtilirse, yanıt yalnızca bu hizmet bildiriminde tanımlanan hizmet türleri hakkındaki bilgileri içerir. |
| --zaman ayarı -t | İşlemi saniyeler içinde gerçekleştirmek için sunucu zaman. Bu zaman sonu, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametrenin varsayılan değeri 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-service-deployed-type-list"></a>sfctl hizmet dağıtılmış tür listesi
Hizmet Kumaşı kümesinde düğümüzerinde dağıtılan uygulamalardan hizmet türleri hakkındaki bilgileri içeren listeyi alır.

Hizmet Kumaşı kümesinde düğümüzerinde dağıtılan uygulamalardan hizmet türleri hakkındaki bilgileri içeren listeyi alır. Yanıt, hizmet türünün adını, kayıt durumunu, onu kaydeden kod paketini ve hizmet paketinin etkinleştirme kimliğini içerir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --uygulama kimliği [Gerekli] | Uygulamanın kimliği. Bu genellikle 'kumaş'\:URI düzeni olmadan uygulamanın tam adıdır. Sürüm 6.0'dan başlayarak hiyerarşik adlar\~" " karakteriyle sınırlandırılır. Örneğin, uygulama adı "kumaş\:/myapp/app1" ise, uygulama kimliği 6.0+ ve önceki sürümlerde "myapp/app1" olarak "myapp\~app1" olacaktır. |
| --düğüm adı [Gerekli] | Düğümün adı. |
| --hizmet bildirimi-adı | Dağıtılan hizmet türü bilgilerinin listesini filtrelemek için hizmet bildiriminin adı. Belirtilirse, yanıt yalnızca bu hizmet bildiriminde tanımlanan hizmet türleri hakkındaki bilgileri içerir. |
| --zaman ayarı -t | İşlemi saniyeler içinde gerçekleştirmek için sunucu zaman. Bu zaman sonu, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametrenin varsayılan değeri 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-service-description"></a>sfctl servis açıklaması
Varolan bir Service Fabric hizmetinin açıklamasını alır.

Varolan bir Service Fabric hizmetinin açıklamasını alır. Açıklama nın alınabilmesi için bir hizmet oluşturulmalıdır.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --service-id [Gerekli] | Hizmetin kimliği. Bu kimlik genellikle 'kumaş'\:URI düzeni olmadan hizmetin tam adıdır. Sürüm 6.0'dan başlayarak hiyerarşik adlar\~" " karakteriyle sınırlandırılır. Örneğin, hizmet adı "kumaş\:/myapp/app1/svc1" ise, hizmet kimliği 6.0+ ve önceki sürümlerde "myapp/app1/svc1"\~\~olacaktır. |
| --zaman ayarı -t | İşlemi saniyeler içinde gerçekleştirmek için sunucu zaman. Bu zaman sonu, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametrenin varsayılan değeri 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-service-get-container-logs"></a>sfctl servis get-konteyner-günlükleri
Servis Kumaşdüğümünde dağıtılan kapsayıcının konteyner günlüklerini alır.

Verilen kod paketi için Servis Kumaşdüğümünde dağıtılan kapsayıcının konteyner günlüklerini alır.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --uygulama kimliği [Gerekli] | Uygulamanın kimliği. Bu genellikle 'kumaş'\:URI düzeni olmadan uygulamanın tam adıdır. Sürüm 6.0'dan başlayarak hiyerarşik adlar\~" " karakteriyle sınırlandırılır. Örneğin, uygulama adı "kumaş\:/myapp/app1" ise, uygulama kimliği 6.0+ ve önceki sürümlerde "myapp/app1" olarak "myapp\~app1" olacaktır. |
| --kod-paket adı [Gerekli] | Hizmet Kumaşı kümesinde uygulama türünün bir parçası olarak kayıtlı hizmet bildiriminde belirtilen kod paketinin adı. |
| --düğüm adı [Gerekli] | Düğümün adı. |
| --hizmet bildirimi-adı [Gerekli] | Hizmet Kumaşı kümesinde uygulama türünün bir parçası olarak kaydedilmiş bir hizmet bildiriminin adı. |
| --önceki | Kod paketi örneğinin çıkış/ölü kapsayıcılarından kapsayıcı günlüklerinin alınıp alınmayacağını belirtir. |
| --kuyruk | Günlüklerin sonundan gösterilen satır sayısı. Varsayılan değer 100'dür. 'tüm' tüm günlükleri göstermek için. |
| --zaman ayarı -t | İşlemi saniyeler içinde gerçekleştirmek için sunucu zaman. Bu zaman sonu, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametrenin varsayılan değeri 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-service-health"></a>sfctl hizmet sağlığı
Belirtilen Servis Kumaşı hizmetinin sağlığını alır.

Belirtilen hizmetin sağlık bilgilerini alır. Hizmette bildirilen sistem durumu durumuna göre bildirilen sistem durumu olaylarının toplanmasını filtrelemek için EventsHealthStateFilter'i kullanın. Döndürülen bölümlerin koleksiyonunu filtrelemek için PartitionsHealthStateFilter'i kullanın. Sistem durumu deposunda bulunmayan bir hizmet belirtirseniz, bu istek bir hata döndürür.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --service-id [Gerekli] | Hizmetin kimliği. Bu kimlik genellikle 'kumaş'\:URI düzeni olmadan hizmetin tam adıdır. Sürüm 6.0'dan başlayarak hiyerarşik adlar\~" " karakteriyle sınırlandırılır. Örneğin, hizmet adı "kumaş\:/myapp/app1/svc1" ise, hizmet kimliği 6.0+ ve önceki sürümlerde "myapp/app1/svc1"\~\~olacaktır. |
| --olaylar-sağlık-durum-filtre | HealthEvent nesnelerinin toplanmasının sistem durumu durumuna göre filtrelemesine olanak tanır. Bu parametre için olası değerler, aşağıdaki sistem durumu durumlarından birinin tamsayı değerini içerir. Yalnızca filtreyle eşleşen olaylar döndürülür. Tüm olaylar toplu sağlık durumunu değerlendirmek için kullanılır. Belirtilmemişse, tüm girişler döndürülür. Durum değerleri bayrak tabanlı numaralandırmadır, bu nedenle değer bitwise 'VEYA' işleci kullanılarak elde edilen bu değerlerin bir birleşimi olabilir. Örneğin, sağlanan değer 6 ise, Ok (2) ve Warning (4) HealthState değerine sahip tüm olaylar döndürülür.  <br> - Varsayılan - Varsayılan değer. Herhangi bir HealthState ile eşleşir. Değer sıfırdır.  <br> - Yok - Herhangi bir HealthState değeriyle eşleşmeyen filtre. Belirli bir durum koleksiyonunda sonuç vermek için kullanılır. Değeri 1'dir.  <br> - Tamam - HealthState değeri Ok ile giriş eşleşen filtre. Değeri 2'dir.  <br> - Uyarı - HealthState değer Uyarısı ile girdi eşleşen filtre. Değeri 4'dür.  <br> - Hata - HealthState değer Hatası ile giriş eşleşen filtre. Değeri 8'dir.  <br> - Tümü - Herhangi bir HealthState değeriyle girişle eşleşen filtre. Değeri 65535' tir. |
| --dışlama-sağlık-istatistik | Sistem durumu istatistiklerinin sorgu sonucunun bir parçası olarak döndürülüp döndürülmemesi gerektiğini gösterir. Varsayılan olarak false. İstatistikler, ok, uyarı ve hata durumu sağlık durumundaki çocuk varlıkların sayısını gösterir. |
| --bölümler-sağlık-durum-filtre | Sistem durumu durumuna göre hizmet durumu sorgusu sonucunda döndürülen bölümlerin sistem durumu durumu nesnelerinin filtrelemesine izin verir. Bu parametre için olası değerler, aşağıdaki sistem durumu durumlarından birinin tamsayı değerini içerir. Yalnızca filtreyle eşleşen bölümler döndürülür. Tüm bölümler, toplu sistem durumunu değerlendirmek için kullanılır. Belirtilmemişse, tüm girişler döndürülür. Durum değerleri bayrak tabanlı numaralandırmadır, bu nedenle değer bitwise 'VEYA' işleci kullanılarak elde edilen bu değerin bir birleşimi olabilir. Örneğin, sağlanan değer 6 ise, Ok (2) ve Warning (4) değerine sahip bölümlerin sistem durumu döndürülür.  <br> - Varsayılan - Varsayılan değer. Herhangi bir HealthState ile eşleşir. Değer sıfırdır.  <br> - Yok - Herhangi bir HealthState değeriyle eşleşmeyen filtre. Belirli bir durum koleksiyonunda sonuç vermek için kullanılır. Değeri 1'dir.  <br> - Tamam - HealthState değeri Ok ile giriş eşleşen filtre. Değeri 2'dir.  <br> - Uyarı - HealthState değer Uyarısı ile girdi eşleşen filtre. Değeri 4'dür.  <br> - Hata - HealthState değer Hatası ile giriş eşleşen filtre. Değeri 8'dir.  <br> - Tümü - Herhangi bir HealthState değeriyle girişle eşleşen filtre. Değeri 65535' tir. |
| --zaman ayarı -t | İşlemi saniyeler içinde gerçekleştirmek için sunucu zaman. Bu zaman sonu, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametrenin varsayılan değeri 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-service-info"></a>sfctl servis bilgileri
Service Fabric uygulamasına ait özel hizmet hakkında bilgi alır.

Belirtilen Hizmet Kumaşı uygulamasına ait belirtilen hizmetle ilgili bilgileri verir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --uygulama kimliği [Gerekli] | Uygulamanın kimliği. Bu genellikle 'kumaş'\:URI düzeni olmadan uygulamanın tam adıdır. Sürüm 6.0'dan başlayarak hiyerarşik adlar\~" " karakteriyle sınırlandırılır. Örneğin, uygulama adı "kumaş\:/myapp/app1" ise, uygulama kimliği 6.0+ ve önceki sürümlerde "myapp/app1" olarak "myapp\~app1" olacaktır. |
| --service-id [Gerekli] | Hizmetin kimliği. Bu kimlik genellikle 'kumaş'\:URI düzeni olmadan hizmetin tam adıdır. Sürüm 6.0'dan başlayarak hiyerarşik adlar\~" " karakteriyle sınırlandırılır. Örneğin, hizmet adı "kumaş\:/myapp/app1/svc1" ise, hizmet kimliği 6.0+ ve önceki sürümlerde "myapp/app1/svc1"\~\~olacaktır. |
| --zaman ayarı -t | İşlemi saniyeler içinde gerçekleştirmek için sunucu zaman. Bu zaman sonu, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametrenin varsayılan değeri 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-service-list"></a>sfctl hizmet listesi
Uygulama kimliği ile belirtilen uygulamaya ait tüm hizmetler hakkında bilgi alır.

Uygulama kimliğinde belirtilen uygulamaya ait tüm hizmetlerle ilgili bilgileri verir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --uygulama kimliği [Gerekli] | Uygulamanın kimliği. Bu genellikle 'kumaş'\:URI düzeni olmadan uygulamanın tam adıdır. Sürüm 6.0'dan başlayarak hiyerarşik adlar\~" " karakteriyle sınırlandırılır. Örneğin, uygulama adı "kumaş\:/myapp/app1" ise, uygulama kimliği 6.0+ ve önceki sürümlerde "myapp/app1" olarak "myapp\~app1" olacaktır. |
| --devam-belirteç | Devamı belirteç parametresi sonraki sonuç kümesini elde etmek için kullanılır. Sistemden elde edilen sonuçlar tek bir yanıta sığmıyorsa, BOŞ olmayan bir değere sahip bir devam belirteci API'nın yanıtına dahil edilir. Bu değer bir sonraki API çağrısına geçirildiğinde, API sonraki sonuç kümesini döndürür. Başka sonuç yoksa, devam belirteci bir değer içermez. Bu parametrenin değeri URL kodlanmış olmamalıdır. |
| --hizmet türü-adı | Sorgulanacak hizmetleri filtrelemek için kullanılan hizmet türü adı. |
| --zaman ayarı -t | İşlemi saniyeler içinde gerçekleştirmek için sunucu zaman. Bu zaman sonu, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametrenin varsayılan değeri 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-service-manifest"></a>sfctl hizmet bildirimi
Bir hizmet türünü açıklayan bildirimi alır.

Bir hizmet türünü açıklayan bildirimi alır. Yanıt, xml'i dize olarak hizmet bildirimini içerir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --uygulama türü adı [Gerekli] | Uygulama türünün adı. |
| --uygulama türü sürümü [Gerekli] | Uygulama türünün sürümü. |
| --hizmet bildirimi-adı [Gerekli] | Hizmet Kumaşı kümesinde uygulama türünün bir parçası olarak kaydedilmiş bir hizmet bildiriminin adı. |
| --zaman ayarı -t | İşlemi saniyeler içinde gerçekleştirmek için sunucu zaman. Bu zaman sonu, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametrenin varsayılan değeri 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-service-package-deploy"></a>sfctl hizmet paketi-dağıtma
Belirtilen hizmetle ilişkili paketleri, belirtilen düğümdeki görüntü önbelleğine indirir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --uygulama türü adı [Gerekli] | İlgili talep edilen hizmet bildirimi için başvuru bildiriminin adı. |
| --uygulama türü sürümü [Gerekli] | İlgili istenen hizmet bildirimi için başvuru bildiriminin sürümü. |
| --düğüm adı [Gerekli] | Düğümün adı. |
| --hizmet bildirimi-adı [Gerekli] | İndirilecek paketlerle ilişkili hizmet bildiriminin adı. |
| --pay-politikası | JSON, paylaşım politikalarının listesini kodladı. Her paylaşım ilkesi öğesi bir 'ad' ve 'kapsam'tan oluşur. Ad, paylaşılacak kodun, yapılandırmanın veya veri paketinin adına karşılık gelir. Kapsam 'Yok', 'Tümü', 'Kod', 'Config' veya 'Veri' olabilir. |
| --zaman ayarı -t | Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-service-package-health"></a>sfctl servis paketi-sağlık
Hizmet Kumaş düğümü ve uygulaması için dağıtılan belirli bir uygulama için bir hizmet paketinin durumu yla ilgili bilgileri alır.

Hizmet Kumaş düğümünde dağıtılan belirli bir uygulama için bir hizmet paketinin durumu yla ilgili bilgileri alır. Dağıtılan hizmet paketinde sistem durumu durumuna göre bildirilen HealthEvent nesnelerinin toplanması için isteğe bağlı olarak filtre uygulama yapmak için EventsHealthStateFilter'i kullanın.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --uygulama kimliği [Gerekli] | Uygulamanın kimliği. Bu genellikle 'kumaş'\:URI düzeni olmadan uygulamanın tam adıdır. Sürüm 6.0'dan başlayarak hiyerarşik adlar\~" " karakteriyle sınırlandırılır. Örneğin, uygulama adı "kumaş\:/myapp/app1" ise, uygulama kimliği 6.0+ ve önceki sürümlerde "myapp/app1" olarak "myapp\~app1" olacaktır. |
| --düğüm adı [Gerekli] | Düğümün adı. |
| --hizmet-paket adı [Gerekli] | Servis paketinin adı. |
| --olaylar-sağlık-durum-filtre | HealthEvent nesnelerinin toplanmasının sistem durumu durumuna göre filtrelemesine olanak tanır. Bu parametre için olası değerler, aşağıdaki sistem durumu durumlarından birinin tamsayı değerini içerir. Yalnızca filtreyle eşleşen olaylar döndürülür. Tüm olaylar toplu sağlık durumunu değerlendirmek için kullanılır. Belirtilmemişse, tüm girişler döndürülür. Durum değerleri bayrak tabanlı numaralandırmadır, bu nedenle değer bitwise 'VEYA' işleci kullanılarak elde edilen bu değerlerin bir birleşimi olabilir. Örneğin, sağlanan değer 6 ise, Ok (2) ve Warning (4) HealthState değerine sahip tüm olaylar döndürülür.  <br> - Varsayılan - Varsayılan değer. Herhangi bir HealthState ile eşleşir. Değer sıfırdır.  <br> - Yok - Herhangi bir HealthState değeriyle eşleşmeyen filtre. Belirli bir durum koleksiyonunda sonuç vermek için kullanılır. Değeri 1'dir.  <br> - Tamam - HealthState değeri Ok ile giriş eşleşen filtre. Değeri 2'dir.  <br> - Uyarı - HealthState değer Uyarısı ile girdi eşleşen filtre. Değeri 4'dür.  <br> - Hata - HealthState değer Hatası ile giriş eşleşen filtre. Değeri 8'dir.  <br> - Tümü - Herhangi bir HealthState değeriyle girişle eşleşen filtre. Değeri 65535' tir. |
| --zaman ayarı -t | İşlemi saniyeler içinde gerçekleştirmek için sunucu zaman. Bu zaman sonu, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametrenin varsayılan değeri 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-service-package-info"></a>sfctl servis paketi-bilgi
Hizmet Kumaşı düğümünde dağıtılan hizmet paketlerinin listesini tam olarak belirtilen ada uygun hale alır.

Verilen uygulama için Hizmet Kumaşdüğümünde dağıtılan hizmet paketleri hakkındaki bilgileri verir. Bu sonuçlar, adı parametre olarak belirtilen servis paketi adı ile tam olarak eşleşen hizmet paketleridir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --uygulama kimliği [Gerekli] | Uygulamanın kimliği. Bu genellikle 'kumaş'\:URI düzeni olmadan uygulamanın tam adıdır. Sürüm 6.0'dan başlayarak hiyerarşik adlar\~" " karakteriyle sınırlandırılır. Örneğin, uygulama adı "kumaş\:/myapp/app1" ise, uygulama kimliği 6.0+ ve önceki sürümlerde "myapp/app1" olarak "myapp\~app1" olacaktır. |
| --düğüm adı [Gerekli] | Düğümün adı. |
| --hizmet-paket adı [Gerekli] | Servis paketinin adı. |
| --zaman ayarı -t | İşlemi saniyeler içinde gerçekleştirmek için sunucu zaman. Bu zaman sonu, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametrenin varsayılan değeri 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-service-package-list"></a>sfctl servis paketi listesi
Hizmet Kumaşdüğümünde dağıtılan hizmet paketlerinin listesini alır.

Verilen uygulama için Hizmet Kumaşdüğümünde dağıtılan hizmet paketleri hakkındaki bilgileri verir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --uygulama kimliği [Gerekli] | Uygulamanın kimliği. Bu genellikle 'kumaş'\:URI düzeni olmadan uygulamanın tam adıdır. Sürüm 6.0'dan başlayarak hiyerarşik adlar\~" " karakteriyle sınırlandırılır. Örneğin, uygulama adı "kumaş\:/myapp/app1" ise, uygulama kimliği 6.0+ ve önceki sürümlerde "myapp/app1" olarak "myapp\~app1" olacaktır. |
| --düğüm adı [Gerekli] | Düğümün adı. |
| --zaman ayarı -t | İşlemi saniyeler içinde gerçekleştirmek için sunucu zaman. Bu zaman sonu, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametrenin varsayılan değeri 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-service-recover"></a>sfctl hizmet kurtarma
Hizmet Kumaşı kümesine, şu anda çoğunluk kaybına bağlı olan belirtilen hizmeti kurtarmaya çalışması gerektiğini gösterir.

Hizmet Kumaşı kümesine, şu anda çoğunluk kaybına bağlı olan belirtilen hizmeti kurtarmaya çalışması gerektiğini gösterir. Bu işlem yalnızca aşağı olan yinelemelerin kurtarılamayacağı biliniyorsa gerçekleştirilmelidir. Bu API'nin yanlış kullanımı olası veri kaybına neden olabilir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --service-id [Gerekli] | Hizmetin kimliği. Bu kimlik genellikle 'kumaş'\:URI düzeni olmadan hizmetin tam adıdır. Sürüm 6.0'dan başlayarak hiyerarşik adlar\~" " karakteriyle sınırlandırılır. Örneğin, hizmet adı "kumaş\:/myapp/app1/svc1" ise, hizmet kimliği 6.0+ ve önceki sürümlerde "myapp/app1/svc1"\~\~olacaktır. |
| --zaman ayarı -t | İşlemi saniyeler içinde gerçekleştirmek için sunucu zaman. Bu zaman sonu, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametrenin varsayılan değeri 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-service-report-health"></a>sfctl hizmet raporu-sağlık
Service Fabric hizmeti hakkında bir sağlık raporu gönderir.

Belirtilen Service Fabric hizmetinin sağlık durumunu bildirir. Rapor, sağlık raporunun kaynağı ve raporlandığı özellik hakkındaki bilgileri içermelidir. Rapor, sağlık deposuna ileten bir Hizmet Kumaşağ Geçidi Hizmeti'ne gönderilir. Rapor ağ geçidi tarafından kabul edilebilir, ancak ek doğrulama sonra sağlık deposu tarafından reddedilir. Örneğin, sistem durumu deposu, eski bir sıra numarası gibi geçersiz bir parametre nedeniyle raporu reddedebilir. Raporun sağlık deposunda uygulanıp uygulanmadığını görmek için, raporun hizmetin sağlık olaylarında görünüp görünmediğini denetleyin.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --sağlık-özellik [Gerekli] | Sağlık bilgilerinin özelliği. <br><br> Bir varlık, farklı özellikler için sistem durumu raporları olabilir. Özellik, muhabirin raporu tetikleyen durum durumunu kategorilere ayırmaesnekliğine izin vermek için sabit bir numaralandırma değil, bir dizedir. Örneğin, SourceId "LocalWatchdog" olan bir muhabir, kullanılabilir diskin bir düğümüzerindeki durumunu izleyebilir, böylece bu düğümdeki "AvailableDisk" özelliğini bildirebilir. Aynı muhabir düğüm bağlantısını izleyebilir, böylece aynı düğümüzerinde bir özellik "Bağlantı" bildirebilir. Sağlık deposunda, bu raporlar belirtilen düğüm için ayrı sağlık olayları olarak kabul edilir. Kaynak Kimliği ile birlikte, özellik benzersiz sağlık bilgilerini tanımlar. |
| --sağlık durumu [Gerekli] | Olası değerler\: arasında 'Geçersiz', 'Tamam', 'Uyarı', 'Hata', 'Bilinmiyor' sayılabilir. |
| --service-id [Gerekli] | Hizmetin kimliği. <br><br> Bu genellikle 'kumaş'\:URI düzeni olmadan hizmetin tam adıdır. Sürüm 6.0'dan başlayarak hiyerarşik adlar\~' ' karakteriyle sınırlandırılır. Örneğin, hizmet adı 'kumaş\:/myapp/app1/svc1' ise, hizmet kimliği önceki\~sürümlerde 6.0+ 'myapp app1\~svc1' ve 'myapp/app1/svc1' olacaktır. |
| --kaynak-id [Gerekli] | Sistem bilgilerini oluşturan istemci/izleme örgütü/sistem bileşenini tanımlayan kaynak adı. |
| --açıklama | Sağlık bilgilerinin açıklaması. <br><br> Rapor hakkında insan tarafından okunabilir bilgiler eklemek için kullanılan ücretsiz metni temsil eder. Açıklama için maksimum dize uzunluğu 4096 karakterdir. Sağlanan dize daha uzunsa, otomatik olarak kesilir. Kesildiğinde, açıklamanın son karakterleri "[Kesildi]" işareti ni içerir ve toplam dize boyutu 4096 karakterdir. İşaretçinin varlığı kullanıcılara kesilme nin oluştuğunu gösterir. Kesildiğinde, açıklamanın özgün dizedeki 4096 karakterden az olduğunu unutmayın. |
| --hemen | Raporun hemen gönderilmesi gerekip gerekmediğini belirten bir bayrak. <br><br> Sağlık raporu, sağlık deposuna ileten Bir Hizmet Dokusu ağ geçidi Uygulamasına gönderilir. Hemen doğru ayarlanırsa, rapor HTTP Ağ Geçidi Uygulaması'nın kullandığı kumaş istemci ayarlarına bakılmaksızın hemen HTTP Ağ Geçidi'nden sağlık deposuna gönderilir. Bu, en kısa sürede gönderilmesi gereken kritik raporlar için yararlıdır. Zamanlamaya ve diğer koşullara bağlı olarak, örneğin HTTP Ağ Geçidi kapalıysa veya ileti Ağ Geçidi'ne ulaşamazsa, rapor göndermek yine de başarısız olabilir. Anında yanlış olarak ayarlanırsa, rapor HTTP Ağ Geçidi'ndeki sağlık istemcisi ayarlarına göre gönderilir. Bu nedenle, HealthReportSendInterval yapılandırmasına göre toplu olarak düzenlenecektir. Bu, sistem durumu istemcisinin sağlık deposuna gelen sağlık raporlama iletilerini ve sistem durumu raporu işlemeyi optimize etmesine olanak sağladığından önerilen ayardır. Varsayılan olarak, raporlar hemen gönderilmez. |
| --kaldırma-ne zaman-süresi doldu | Raporun süresi dolduğunda sistem durumu deposundan kaldırılıp kaldırılmadığını gösteren değer. <br><br> Doğru olarak ayarlanırsa, rapor süresi dolduktan sonra sistem durumu deposundan kaldırılır. Yanlış olarak ayarlanmışsa, süresi dolduğunda rapor bir hata olarak kabul edilir. Bu özelliğin değeri varsayılan olarak yanlıştır. İstemciler düzenli aralıklarla rapor verdiğinde, RemoveWhenExpired false (varsayılan) olarak ayarlanmalıdır. Bu şekilde, muhabirin sorunları (örn. kilitlenme) vardır ve rapor edemez, sağlık raporu sona erdiğinde varlık hata olarak değerlendirilir. Bu, varlığı Hata durumu durumunda olarak gösterir. |
| --sıra numarası | Sayısal dize olarak bu sistem durumu raporunun sıra numarası. <br><br> Rapor sıra numarası, eski raporları algılamak için sistem durumu deposu tarafından kullanılır. Belirtilmemişse, rapor eklendiğinde bir sıra numarası sistem durumu istemcisi tarafından otomatik olarak oluşturulur. |
| --zaman ayarı -t | Varsayılan\: 60. |
| --ttl | Bu sağlık raporunun geçerli olduğu süre. Bu alan, süreyi belirtmek için ISO8601 biçimini kullanır. <br><br> İstemciler düzenli olarak rapor verdiğinde, yaşamak için zamandan daha yüksek sıklıkta raporlar göndermelidir. İstemciler geçiş hakkında rapor verirlerse, zamanı sonsuz alete ayarlayabilirler. Yaşam süresi dolduğunda, sağlık bilgilerini içeren sağlık olayı, RemoveWhenExpired doğruysa, sağlık deposundan kaldırılır veya hata yla değerlendirilir, RemoveWhenExpired false ise. Belirtilmemişse, sonsuz değerde varsayılan olarak yaşama zamanı. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-service-resolve"></a>sfctl hizmet çözümle
Hizmet Kumaşı bölmeyi çözümle.

Hizmet yinelemelerinin uç noktalarını almak için Hizmet Kumaşı hizmet bölmesini çözümle.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --service-id [Gerekli] | Hizmetin kimliği. Bu kimlik genellikle 'kumaş'\:URI düzeni olmadan hizmetin tam adıdır. Sürüm 6.0'dan başlayarak hiyerarşik adlar\~" " karakteriyle sınırlandırılır. Örneğin, hizmet adı "kumaş\:/myapp/app1/svc1" ise, hizmet kimliği 6.0+ ve önceki sürümlerde "myapp/app1/svc1"\~\~olacaktır. |
| --partition-key-type | Bölüm için anahtar türü. Hizmetin bölüm düzeni Int64Range veya Named ise bu parametre gereklidir. Olası değerler takip ediyor. - Yok (1) - PartitionKeyValue parametresinin belirtilmediğini gösterir. Bu Singleton olarak bölümleme düzeni ile bölümler için geçerlidir. Varsayılan değer budur. Değeri 1'dir. - Int64Range (2) - PartitionKeyValue parametresi int64 bölüm anahtarı olduğunu gösterir. Bu Int64Range olarak bölümleme düzeni ile bölümler için geçerlidir. Değeri 2'dir. - Adlandırılmış (3) - PartitionKeyValue parametresinin bölümün adı olduğunu gösterir. Bu, Adlandırılmış olarak bölümleme düzeni olan bölümler için geçerlidir. Değeri 3'dür. |
| --partition-key-value | Bölme anahtarı. Hizmetin bölüm düzeni Int64Range veya Named ise bu gereklidir. Bu bölüm kimliği değil, tamsayı anahtar değeri veya bölüm kimliği adıdır. Örneğin, hizmetiniz 0 ile 10 arasında değişen bölümler kullanıyorsa, bunlar PartitionKeyValue bu aralıkta bir arayla bir sonda olacaktır. Aralığı veya adı görmek için servis açıklamasını sorgula. |
| --önceki rsp sürümü | Daha önce alınan yanıtın Sürüm alanındaki değer. Kullanıcı daha önce elde edilen sonucun bayat olduğunu biliyorsa bu gereklidir. |
| --zaman ayarı -t | İşlemi saniyeler içinde gerçekleştirmek için sunucu zaman. Bu zaman sonu, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametrenin varsayılan değeri 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-service-type-list"></a>sfctl hizmet türü listesi
Hizmet Kumaşı kümesinde sağlanan bir uygulama türü tarafından desteklenen hizmet türleri hakkındaki bilgileri içeren listeyi alır.

Hizmet Kumaşı kümesinde sağlanan bir uygulama türü tarafından desteklenen hizmet türleri hakkındaki bilgileri içeren listeyi alır. Sağlanan uygulama türü olmalıdır. Aksi takdirde, 404 durumu döndürülür.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --uygulama türü adı [Gerekli] | Uygulama türünün adı. |
| --uygulama türü sürümü [Gerekli] | Uygulama türünün sürümü. |
| --zaman ayarı -t | İşlemi saniyeler içinde gerçekleştirmek için sunucu zaman. Bu zaman sonu, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametrenin varsayılan değeri 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-service-update"></a>sfctl hizmet güncelleştirmesi
Verilen güncelleştirme açıklamasını kullanarak belirtilen hizmeti güncelleştirir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --service-id [Gerekli] | Hizmetin kimliği. Bu genellikle 'kumaş'\:URI düzeni olmadan hizmetin tam adıdır. Sürüm 6.0'dan başlayarak hiyerarşik adlar\~" " karakteriyle sınırlandırılır. Örneğin, hizmet adı 'kumaş\:/myapp/app1/svc1' ise, hizmet kimliği önceki\~sürümlerde 6.0+ 'myapp app1\~svc1' ve 'myapp/app1/svc1' olacaktır. |
| --kısıtlamalar | Bir dize olarak yerleşim kısıtlamaları. Yerleşim kısıtlamaları düğüm özellikleri üzerinde boolean ifadeler ve hizmet gereksinimlerine göre belirli düğümleri bir hizmet kısıtlamak için izin verir. Örneğin, Düğüm Tipi'nin mavi olduğu düğümlere bir hizmet\: yerleştirmek için aşağıdaki "NodeColor == mavi" belirtin. |
| --koreilişkili hizmet | İlişkilendirilmesi hedef hizmetin adı. |
| --korelasyon | Bir hizalama afinitesini kullanarak hizmeti varolan bir hizmetle ilişkilendirin. |
| --örnek sayısı | Örnek sayısı. Bu yalnızca devletsiz hizmetler için geçerlidir. |
| --yük ölçümleri | JSON, düğümler arasında yük dengelemesi yaparken kullanılan ölçümlerin listesini kodladı. |
| --min-çoğaltma-set-boyutu | En az yineleme bir sayı olarak boyut kümesi. Bu yalnızca devlet hizmetleri için geçerlidir. |
| --taşıma maliyeti | Hizmetin taşıma maliyetini belirtir. Olası değerler\: 'Sıfır', 'Düşük', 'Orta', 'Yüksek', 'Çok Yüksek'tir. |
| --yerleşim ilkesi listesi | JSON, hizmetiçin yerleşim ilkeleri ve ilişkili alan adları listesini kodladı. İlkeler bir veya daha\: `NonPartiallyPlaceService`fazla `PreferPrimaryDomain` `RequireDomain`olabilir `RequireDomainDistribution`, , . |
| --çoğunluk-kayıp-bekleme | Bir bölümün çoğunluk kaybı durumunda olması için izin verilen en uzun süre, saniye cinsinden. Bu yalnızca devlet hizmetleri için geçerlidir. |
| --çoğaltma-yeniden başlatma-bekleme | Bir yinelemenin aşağı inip yeni bir yinelemenin oluşturulması arasındaki süre saniye cinsinden. Bu yalnızca devlet hizmetleri için geçerlidir. |
| --ölçekleme ilkeleri | JSON, bu hizmet için ölçekleme ilkelerinin listesini kodladı. |
| --hizmet yerleştirme süresi | Kopyaların, bu yapıyı bildirmeden önce InBuild'te kalabilecekleri süre sıkışmıştır. Bu yalnızca devlet hizmetleri için geçerlidir. |
| --stand-by-çoğaltma-tutmak | StandBy yinelemelerinin kaldırılmadan önce tutulacağı maksimum süre saniye cinsinden. Bu yalnızca devlet hizmetleri için geçerlidir. |
| --devletli | Hedef hizmetin devlet hizmeti olduğunu gösterir. |
| --devletsiz | Hedef hizmetin devletsiz bir hizmet olduğunu gösterir. |
| --hedef-çoğaltma-ayar-boyutu | Hedef çoğaltma boyutu bir sayı olarak ayarlayın. Bu yalnızca devlet hizmetleri için geçerlidir. |
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
