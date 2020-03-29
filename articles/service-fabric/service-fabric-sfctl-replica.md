---
title: Azure Servis Kumaş CLI- sfctl çoğaltma
description: Azure Service Fabric komut satırı arabirimi sfctl hakkında bilgi edinin. Yinelemeleri yönetmek için komutların listesini içerir.
author: jeffj6123
ms.topic: reference
ms.date: 1/16/2020
ms.author: jejarry
ms.openlocfilehash: f6ad0b4c08ac8d710340fe654a068d0a3804e58f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76905815"
---
# <a name="sfctl-replica"></a>sfctl replica
Hizmet bölümlerine ait yinelemeleri yönetin.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| Dağıtılan | Service Fabric düğümünde dağıtılan çoğaltma ayrıntılarını alır. |
| dağıtılan liste | Hizmet Kumaşdüğümünde dağıtılan yinelemelerin listesini alır. |
| Sağlık | Service Fabric'in durumunu devlethizmeti yinelemesini veya devletsiz hizmet örneğini alır. |
| bilgiler | Service Fabric bölümü hakkında bilgi alır. |
| list | Service Fabric hizmet bölümü kopyaları hakkında bilgi alır. |
| remove | Düğüm üzerinde çalışan bir hizmet yinelemesi kaldırır. |
| rapor-sağlık | Service Fabric çoğaltma hakkında bir sağlık raporu gönderir. |
| restart | Düğüm üzerinde çalışan kalıcı bir hizmetin hizmet yinelemesini yeniden başlatır. |

## <a name="sfctl-replica-deployed"></a>sfctl çoğaltma dağıtıldı
Service Fabric düğümünde dağıtılan çoğaltma ayrıntılarını alır.

Hizmet Kumaş düğümünde dağıtılan yinelemenin ayrıntılarını alır. Bilgiler, hizmet türü, hizmet adı, geçerli hizmet işlemi, geçerli hizmet işlemi başlangıç tarihi, bölüm kimliği, çoğaltma/instance kimliği, bildirilen yük ve diğer bilgileri içerir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --düğüm adı [Gerekli] | Düğümün adı. |
| --partition-id [Gerekli] | Bölümün kimliği. |
| --çoğaltma-id [Gerekli] | Çoğaltmanın tanımlayıcısı. |
| --zaman ayarı -t | İşlemi saniyeler içinde gerçekleştirmek için sunucu zaman. Bu zaman sonu, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametrenin varsayılan değeri 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-replica-deployed-list"></a>sfctl çoğaltma dağıtılmış liste
Hizmet Kumaşdüğümünde dağıtılan yinelemelerin listesini alır.

Hizmet Kumaşdüğümünde dağıtılan yinelemelerle ilgili bilgileri içeren listeyi alır. Bilgiler bölüm kimliği, çoğaltma kimliği, yinelemenin durumu, hizmetin adı, hizmet türünün adı ve diğer bilgileri içerir. Dağıtılan yinelemeler hakkında bu parametreler için belirtilen değerlerle eşleşen bilgileri döndürmek için PartitionId veya ServiceManifestName sorgu parametrelerini kullanın.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --uygulama kimliği [Gerekli] | Uygulamanın kimliği. Bu genellikle 'kumaş'\:URI düzeni olmadan uygulamanın tam adıdır. Sürüm 6.0'dan başlayarak hiyerarşik adlar\~" " karakteriyle sınırlandırılır. Örneğin, uygulama adı "kumaş\:/myapp/app1" ise, uygulama kimliği 6.0+ ve önceki sürümlerde "myapp/app1" olarak "myapp\~app1" olacaktır. |
| --düğüm adı [Gerekli] | Düğümün adı. |
| --partition-id | Bölümün kimliği. |
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

## <a name="sfctl-replica-health"></a>sfctl çoğaltma sağlık
Service Fabric'in durumunu devlethizmeti yinelemesini veya devletsiz hizmet örneğini alır.

Service Fabric çoğaltmanın sağlığını alır. Yinelemede bildirilen sistem durumu durumuna göre bildirilen sistem durumu olaylarının toplanmasını filtrelemek için EventsHealthStateFilter'i kullanın.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --partition-id [Gerekli] | Bölümün kimliği. |
| --çoğaltma-id [Gerekli] | Çoğaltmanın tanımlayıcısı. |
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

## <a name="sfctl-replica-info"></a>sfctl çoğaltma bilgi
Service Fabric bölümü hakkında bilgi alır.

Yanıt, kimlik, rol, durum, sistem durumu, düğüm adı, çalışma süresi ve yinelemeyle ilgili diğer ayrıntıları içerir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --partition-id [Gerekli] | Bölümün kimliği. |
| --çoğaltma-id [Gerekli] | Çoğaltmanın tanımlayıcısı. |
| --zaman ayarı -t | İşlemi saniyeler içinde gerçekleştirmek için sunucu zaman. Bu zaman sonu, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametrenin varsayılan değeri 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-replica-list"></a>sfctl çoğaltma listesi
Service Fabric hizmet bölümü kopyaları hakkında bilgi alır.

GetReplicas bitiş noktası, belirtilen bölümün yinelemeleri hakkında bilgi verir. Yanıt, kimlik, rol, durum, sistem durumu, düğüm adı, çalışma süresi ve yinelemeyle ilgili diğer ayrıntıları içerir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --partition-id [Gerekli] | Bölümün kimliği. |
| --devam-belirteç | Devamı belirteç parametresi sonraki sonuç kümesini elde etmek için kullanılır. Sistemden elde edilen sonuçlar tek bir yanıta sığmıyorsa, BOŞ olmayan bir değere sahip bir devam belirteci API'nın yanıtına dahil edilir. Bu değer bir sonraki API çağrısına geçirildiğinde, API sonraki sonuç kümesini döndürür. Başka sonuç yoksa, devam belirteci bir değer içermez. Bu parametrenin değeri URL kodlanmış olmamalıdır. |
| --zaman ayarı -t | İşlemi saniyeler içinde gerçekleştirmek için sunucu zaman. Bu zaman sonu, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametrenin varsayılan değeri 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-replica-remove"></a>sfctl çoğaltma kaldırmak
Düğüm üzerinde çalışan bir hizmet yinelemesi kaldırır.

Bu API, bir Hizmet Kumaşı kümesinden bir yinelemekaldırarak bir Hizmet Kumaşı yineleme hatalarını simüle eder. Kaldırma yinelemeyi kapatır, yinelemeyi Yok rolüne geçişeder ve ardından yinelemenin tüm durum bilgilerini kümeden kaldırır. Bu API çoğaltma durum kaldırma yolunu sınar ve istemci API'ler aracılığıyla rapor hatası kalıcı yolu simüle eder. Uyarı - Bu API kullanıldığında güvenlik denetimi yapılmaz. Bu API'nin yanlış kullanımı, devlet tarafından sunulan hizmetler için veri kaybına neden olabilir. Buna ek olarak, forceRemove bayrağı aynı işlemde barındırılan diğer tüm yinelemeleri etkiler.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --düğüm adı [Gerekli] | Düğümün adı. |
| --partition-id [Gerekli] | Bölümün kimliği. |
| --çoğaltma-id [Gerekli] | Çoğaltmanın tanımlayıcısı. |
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

## <a name="sfctl-replica-report-health"></a>sfctl çoğaltma raporu-sağlık
Service Fabric çoğaltma hakkında bir sağlık raporu gönderir.

Belirtilen Hizmet Kumaşı çoğaltmanın sağlık durumunu bildirir. Rapor, sağlık raporunun kaynağı ve raporlandığı özellik hakkındaki bilgileri içermelidir. Rapor, sağlık deposuna ileten Bir Hizmet Dokusu ağ geçidi Çoğaltma'ya gönderilir. Rapor ağ geçidi tarafından kabul edilebilir, ancak ek doğrulama sonra sağlık deposu tarafından reddedilir. Örneğin, sistem durumu deposu, eski bir sıra numarası gibi geçersiz bir parametre nedeniyle raporu reddedebilir. Raporun sistem durumu deposunda uygulanıp uygulanmadığını görmek için çoğaltma durumunu çalıştırın ve raporun HealthEvents bölümünde görünüp görünmediğini denetleyin.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --sağlık-özellik [Gerekli] | Sağlık bilgilerinin özelliği. <br><br> Bir varlık, farklı özellikler için sistem durumu raporları olabilir. Özellik, muhabirin raporu tetikleyen durum durumunu kategorilere ayırmaesnekliğine izin vermek için sabit bir numaralandırma değil, bir dizedir. Örneğin, SourceId "LocalWatchdog" olan bir muhabir, kullanılabilir diskin bir düğümüzerindeki durumunu izleyebilir, böylece bu düğümdeki "AvailableDisk" özelliğini bildirebilir. Aynı muhabir düğüm bağlantısını izleyebilir, böylece aynı düğümüzerinde bir özellik "Bağlantı" bildirebilir. Sağlık deposunda, bu raporlar belirtilen düğüm için ayrı sağlık olayları olarak kabul edilir. Kaynak Kimliği ile birlikte, özellik benzersiz sağlık bilgilerini tanımlar. |
| --sağlık durumu [Gerekli] | Olası değerler\: arasında 'Geçersiz', 'Tamam', 'Uyarı', 'Hata', 'Bilinmiyor' sayılabilir. |
| --partition-id [Gerekli] | Bölümün kimliği. |
| --çoğaltma-id [Gerekli] | Bölümün kimliği. |
| --kaynak-id [Gerekli] | Sistem bilgilerini oluşturan istemci/izleme örgütü/sistem bileşenini tanımlayan kaynak adı. |
| --açıklama | Sağlık bilgilerinin açıklaması. <br><br> Rapor hakkında insan tarafından okunabilir bilgiler eklemek için kullanılan ücretsiz metni temsil eder. Açıklama için maksimum dize uzunluğu 4096 karakterdir. Sağlanan dize daha uzunsa, otomatik olarak kesilir. Kesildiğinde, açıklamanın son karakterleri "[Kesildi]" işareti ni içerir ve toplam dize boyutu 4096 karakterdir. İşaretçinin varlığı kullanıcılara kesilme nin oluştuğunu gösterir. Kesildiğinde, açıklamanın özgün dizedeki 4096 karakterden az olduğunu unutmayın. |
| --hemen | Raporun hemen gönderilmesi gerekip gerekmediğini belirten bir bayrak. <br><br> Sağlık raporu, sağlık deposuna ileten Bir Hizmet Dokusu ağ geçidi Uygulamasına gönderilir. Hemen doğru ayarlanırsa, rapor HTTP Ağ Geçidi Uygulaması'nın kullandığı kumaş istemci ayarlarına bakılmaksızın hemen HTTP Ağ Geçidi'nden sağlık deposuna gönderilir. Bu, en kısa sürede gönderilmesi gereken kritik raporlar için yararlıdır. Zamanlamaya ve diğer koşullara bağlı olarak, örneğin HTTP Ağ Geçidi kapalıysa veya ileti Ağ Geçidi'ne ulaşamazsa, rapor göndermek yine de başarısız olabilir. Anında yanlış olarak ayarlanırsa, rapor HTTP Ağ Geçidi'ndeki sağlık istemcisi ayarlarına göre gönderilir. Bu nedenle, HealthReportSendInterval yapılandırmasına göre toplu olarak düzenlenecektir. Bu, sistem durumu istemcisinin sağlık deposuna gelen sağlık raporlama iletilerini ve sistem durumu raporu işlemeyi optimize etmesine olanak sağladığından önerilen ayardır. Varsayılan olarak, raporlar hemen gönderilmez. |
| --kaldırma-ne zaman-süresi doldu | Raporun süresi dolduğunda sistem durumu deposundan kaldırılıp kaldırılmadığını gösteren değer. <br><br> Doğru olarak ayarlanırsa, rapor süresi dolduktan sonra sistem durumu deposundan kaldırılır. Yanlış olarak ayarlanmışsa, süresi dolduğunda rapor bir hata olarak kabul edilir. Bu özelliğin değeri varsayılan olarak yanlıştır. İstemciler düzenli aralıklarla rapor verdiğinde, RemoveWhenExpired false (varsayılan) olarak ayarlanmalıdır. Bu şekilde, muhabirin sorunları (örn. kilitlenme) vardır ve rapor edemez, sağlık raporu sona erdiğinde varlık hata olarak değerlendirilir. Bu, varlığı Hata durumu durumunda olarak gösterir. |
| --sıra numarası | Sayısal dize olarak bu sistem durumu raporunun sıra numarası. <br><br> Rapor sıra numarası, eski raporları algılamak için sistem durumu deposu tarafından kullanılır. Belirtilmemişse, rapor eklendiğinde bir sıra numarası sistem durumu istemcisi tarafından otomatik olarak oluşturulur. |
| --hizmet türü | Sağlık bildirilmekte olan hizmet çoğaltma türü (stateless veya stateful). Aşağıda olası değerler\: 'Stateless', 'Stateful'.  Varsayılan\: Durum. |
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

## <a name="sfctl-replica-restart"></a>sfctl çoğaltma yeniden başlatma
Düğüm üzerinde çalışan kalıcı bir hizmetin hizmet yinelemesini yeniden başlatır.

Düğüm üzerinde çalışan kalıcı bir hizmetin hizmet yinelemesini yeniden başlatır. Uyarı - Bu API kullanıldığında güvenlik denetimi yapılmaz. Bu API'nin yanlış kullanımı, devlet tarafından sunulan hizmetler için kullanılabilirlik kaybına neden olabilir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --düğüm adı [Gerekli] | Düğümün adı. |
| --partition-id [Gerekli] | Bölümün kimliği. |
| --çoğaltma-id [Gerekli] | Çoğaltmanın tanımlayıcısı. |
| --zaman ayarı -t | İşlemi saniyeler içinde gerçekleştirmek için sunucu zaman. Bu zaman sonu, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametrenin varsayılan değeri 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |


## <a name="next-steps"></a>Sonraki adımlar
- Servis Kumaşı CLI'yi [kur.](service-fabric-cli.md)
- [Örnek komut dosyalarını](/azure/service-fabric/scripts/sfctl-upgrade-application)kullanarak Service Fabric CLI'yi nasıl kullanacağınızı öğrenin.
