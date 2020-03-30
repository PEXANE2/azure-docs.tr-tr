---
title: Azure Servis Kumaş CLI- sfctl uygulaması
description: Azure Service Fabric komut satırı arabirimi sfctl hakkında bilgi edinin. Uygulamaları yönetmek için komutların listesini içerir.
author: jeffj6123
ms.topic: reference
ms.date: 1/16/2020
ms.author: jejarry
ms.openlocfilehash: b4e1066bba1db387c9dc0600bc55522f0b5fe897
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76906203"
---
# <a name="sfctl-application"></a>sfctl application
Uygulamaları ve uygulama türlerini oluşturun, silin ve yönetin.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| oluşturmaya | Belirtilen açıklamayı kullanarak Bir Hizmet Kumaşı uygulaması oluşturur. |
| delete | Varolan bir Service Fabric uygulamasını siler. |
| Dağıtılan | Hizmet Kumaş düğümünde dağıtılan bir uygulama hakkındaki bilgileri alır. |
| dağıtılan-sağlık | Hizmet Kumaş düğümünde dağıtılan bir uygulamanın sistem durumu yla ilgili bilgileri alır. |
| dağıtılan liste | Hizmet Kumaşdüğümünde dağıtılan uygulamaların listesini alır. |
| Sağlık | Servis kumaşı uygulamasının sağlığını alır. |
| bilgiler | Service Fabric uygulaması hakkında bilgi alır. |
| list | Belirtilen filtrelerle eşleşen Hizmet Dokusu kümesinde oluşturulan uygulamaların listesini alır. |
| yükleme | Service Fabric uygulaması hakkında yük bilgileri alır. |
| bildirim | Bir uygulama türünü açıklayan bildirimi alır. |
| Sağlanması | Harici mağazadaki '.sfpkg' paketini veya görüntü deposundaki uygulama paketini kullanarak bir Hizmet Kumaşı uygulama türünü kümeye göre hükümler veya kaydeder. |
| rapor-sağlık | Service Fabric uygulaması hakkında bir sağlık raporu gönderir. |
| type | Hizmet Kumaşı kümesindeki uygulama türlerinin listesini tam olarak belirtilen ada eşleşen alır. |
| tür listesi | Hizmet Kumaşı kümesindeki uygulama türlerinin listesini alır. |
| hükümsüz | Hizmet Kumaşı uygulama türünü kümeden kaldırır veya kaydeder. |
| yükseltme | Service Fabric kümesinde bir uygulamayı yükseltmeye başlar. |
| yükseltme-özgeçmiş | Hizmet Kumaşı kümesindeki bir uygulamayı yükseltmeye devam eder. |
| yükseltme-geri alma | Service Fabric kümesinde bir uygulamanın şu anda devam eden yükseltmesini geri alma başlar. |
| yükseltme durumu | Bu uygulamada gerçekleştirilen en son yükseltme için ayrıntıları alır. |
| karşıya yükle | Hizmet Kumaşı uygulama paketini görüntü deposuna kopyalayın. |

## <a name="sfctl-application-create"></a>sfctl uygulama oluşturmak
Belirtilen açıklamayı kullanarak Bir Hizmet Kumaşı uygulaması oluşturur.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --app-name [Gerekli] | 'Kumaş'\:URI şeması da dahil olmak üzere uygulamanın adı. |
| --uygulama türü [Gerekli] | Uygulama bildiriminde bulunan uygulama türü adı. |
| --uygulama sürümü [Gerekli] | Uygulama bildiriminde tanımlanan uygulama türünün sürümü. |
| --maksimum düğüm sayısı | Service Fabric'in bu uygulama için kapasite ayıracağı maksimum düğüm sayısı. Bunun, bu uygulamanın hizmetlerinin tüm düğümlere yerleştirileceği anlamına gelmediğini unutmayın. |
| --ölçümler | JSON kodlanmış uygulama kapasitesi metrik açıklamaları listesi. Bir metrik, uygulamanın var olduğu her düğüm için kapasite kümesiyle ilişkili bir ad olarak tanımlanır. |
| --min-düğüm sayısı | Service Fabric'in bu uygulama için kapasite ayıracağı minimum düğüm sayısı. Bunun, bu uygulamanın hizmetlerinin tüm düğümlere yerleştirileceği anlamına gelmediğini unutmayın. |
| --parametreler | Uygulama oluşturulurken uygulanacak uygulama parametresi geçersiz kılmalarının JSON kodlanmış listesi. |
| --zaman ayarı -t | Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-application-delete"></a>sfctl uygulama silme
Varolan bir Service Fabric uygulamasını siler.

Bir uygulama silinebilir önce oluşturulmalıdır. Bir uygulamayı silmek, bu uygulamanın parçası olan tüm hizmetleri siler. Varsayılan olarak, Service Fabric hizmet yinelemelerini zarif bir şekilde kapatmaya ve ardından hizmeti silmeye çalışır. Ancak, bir hizmet yinelemeyi incelikle kapatma da sorun yaşıyorsa, silme işlemi uzun sürebilir veya takılıp kalabilir. Zarif kapanış sırasını atlamak ve uygulamayı ve tüm hizmetlerini zorla silmek için isteğe bağlı ForceRemove bayrağını kullanın.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --uygulama kimliği [Gerekli] | Uygulamanın kimliği. Bu genellikle 'kumaş'\:URI düzeni olmadan uygulamanın tam adıdır. Sürüm 6.0'dan başlayarak hiyerarşik adlar\~" " karakteriyle sınırlandırılır. Örneğin, uygulama adı "kumaş\:/myapp/app1" ise, uygulama kimliği 6.0+ ve önceki sürümlerde "myapp/app1" olarak "myapp\~app1" olacaktır. |
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

## <a name="sfctl-application-deployed"></a>sfctl uygulaması dağıtıldı
Hizmet Kumaş düğümünde dağıtılan bir uygulama hakkındaki bilgileri alır.

Bu sorgu, sağlanan başvuru kimliği sistem uygulaması içinse sistem başvuru bilgilerini verir. Sonuçlar, etkin, etkin leştirilmiş ve indirilmiş durumlarda dağıtılan uygulamaları kapsar. Bu sorgu, düğüm adının kümedeki bir düğüme karşılık geldiğini gerektirir. Sağlanan düğüm adı kümedeki etkin Hizmet Dokusu düğümlerini işaret etmiyorsa sorgu başarısız olur.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --uygulama kimliği [Gerekli] | Uygulamanın kimliği. Bu genellikle 'kumaş'\:URI düzeni olmadan uygulamanın tam adıdır. Sürüm 6.0'dan başlayarak hiyerarşik adlar\~" " karakteriyle sınırlandırılır. Örneğin, uygulama adı "kumaş\:/myapp/app1" ise, uygulama kimliği 6.0+ ve önceki sürümlerde "myapp/app1" olarak "myapp\~app1" olacaktır. |
| --düğüm adı [Gerekli] | Düğümün adı. |
| --include-health-state | Bir varlığın sistem durumu ekleyin. Bu parametre yanlış sayılsa veya belirtilmemişse, döndürülen sistem durumu "Bilinmiyor" olur. Doğru olarak ayarlandığında, sorgu, sonuçlar birleştirilmeden önce düğüme ve sistem hizmetine paralel olarak gider. Sonuç olarak, sorgu daha pahalıdır ve daha uzun sürebilir. |
| --zaman ayarı -t | İşlemi saniyeler içinde gerçekleştirmek için sunucu zaman. Bu zaman sonu, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametrenin varsayılan değeri 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-application-deployed-health"></a>sfctl uygulaması dağıtıldı-sağlık
Hizmet Kumaş düğümünde dağıtılan bir uygulamanın sistem durumu yla ilgili bilgileri alır.

Hizmet Kumaş düğümünde dağıtılan bir uygulamanın sistem durumu yla ilgili bilgileri alır. Dağıtılan uygulamada bildirilen HealthEvent nesnelerinin toplanmasını sistem durumu durumuna göre isteğe bağlı olarak filtrelemek için EventsHealthStateFilter'i kullanın. Sağlık durumuna göre DeployedServicePackageHealth çocuklar için isteğe bağlı olarak filtre lemek için DeployedServicePackagesHealthStateFilter kullanın.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --uygulama kimliği [Gerekli] | Uygulamanın kimliği. Bu genellikle 'kumaş'\:URI düzeni olmadan uygulamanın tam adıdır. Sürüm 6.0'dan başlayarak hiyerarşik adlar\~" " karakteriyle sınırlandırılır. Örneğin, uygulama adı "kumaş\:/myapp/app1" ise, uygulama kimliği 6.0+ ve önceki sürümlerde "myapp/app1" olarak "myapp\~app1" olacaktır. |
| --düğüm adı [Gerekli] | Düğümün adı. |
| --dağıtılan-hizmet paketleri-sağlık-durum filtresi | Dağıtılan hizmet paketi sistem durumu nesnelerinin, sistem durumu durumuna göre dağıtılan uygulama durumu sorgusu sonucunda döndürülen nesnelerin filtrelemesine izin verir. Bu parametre için olası değerler, aşağıdaki sistem durumu durumlarından birinin tamsayı değerini içerir. Yalnızca filtreyle eşleşen dağıtılmış hizmet paketleri döndürülür. Dağıtılan tüm hizmet paketleri, dağıtılan uygulamanın toplu sistem durumunu değerlendirmek için kullanılır. Belirtilmemişse, tüm girişler döndürülür. Durum değerleri bayrak tabanlı numaralandırmadır, bu nedenle değer bitwise 'VEYA' işleci kullanılarak elde edilen bu değerlerin bir birleşimi olabilir. Örneğin, sağlanan değer 6 ise, Ok (2) ve Uyarı (4) değerine sahip Sağlık Durumu hizmet paketleri döndürülür.  <br> - Varsayılan - Varsayılan değer. Herhangi bir HealthState ile eşleşir. Değer sıfırdır.  <br> - Yok - Herhangi bir HealthState değeriyle eşleşmeyen filtre. Belirli bir durum koleksiyonunda sonuç vermek için kullanılır. Değeri 1'dir.  <br> - Tamam - HealthState değeri Ok ile giriş eşleşen filtre. Değeri 2'dir.  <br> - Uyarı - HealthState değer Uyarısı ile girdi eşleşen filtre. Değeri 4'dür.  <br> - Hata - HealthState değer Hatası ile giriş eşleşen filtre. Değeri 8'dir.  <br> - Tümü - Herhangi bir HealthState değeriyle girişle eşleşen filtre. Değeri 65535' tir. |
| --olaylar-sağlık-durum-filtre | HealthEvent nesnelerinin toplanmasının sistem durumu durumuna göre filtrelemesine olanak tanır. Bu parametre için olası değerler, aşağıdaki sistem durumu durumlarından birinin tamsayı değerini içerir. Yalnızca filtreyle eşleşen olaylar döndürülür. Tüm olaylar toplu sağlık durumunu değerlendirmek için kullanılır. Belirtilmemişse, tüm girişler döndürülür. Durum değerleri bayrak tabanlı numaralandırmadır, bu nedenle değer bitwise 'VEYA' işleci kullanılarak elde edilen bu değerlerin bir birleşimi olabilir. Örneğin, sağlanan değer 6 ise, Ok (2) ve Warning (4) HealthState değerine sahip tüm olaylar döndürülür.  <br> - Varsayılan - Varsayılan değer. Herhangi bir HealthState ile eşleşir. Değer sıfırdır.  <br> - Yok - Herhangi bir HealthState değeriyle eşleşmeyen filtre. Belirli bir durum koleksiyonunda sonuç vermek için kullanılır. Değeri 1'dir.  <br> - Tamam - HealthState değeri Ok ile giriş eşleşen filtre. Değeri 2'dir.  <br> - Uyarı - HealthState değer Uyarısı ile girdi eşleşen filtre. Değeri 4'dür.  <br> - Hata - HealthState değer Hatası ile giriş eşleşen filtre. Değeri 8'dir.  <br> - Tümü - Herhangi bir HealthState değeriyle girişle eşleşen filtre. Değeri 65535' tir. |
| --dışlama-sağlık-istatistik | Sistem durumu istatistiklerinin sorgu sonucunun bir parçası olarak döndürülüp döndürülmemesi gerektiğini gösterir. Varsayılan olarak false. İstatistikler, ok, uyarı ve hata durumu sağlık durumundaki çocuk varlıkların sayısını gösterir. |
| --zaman ayarı -t | İşlemi saniyeler içinde gerçekleştirmek için sunucu zaman. Bu zaman sonu, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametrenin varsayılan değeri 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-application-deployed-list"></a>sfctl uygulaması dağıtılmış liste
Hizmet Kumaşdüğümünde dağıtılan uygulamaların listesini alır.

Hizmet Kumaşdüğümünde dağıtılan uygulamaların listesini alır. Sonuçlar, kimlikle açıkça sorgulanmadıkça dağıtılan sistem uygulamaları hakkında bilgi içermez. Sonuçlar, etkin, etkin leştirilmiş ve indirilmiş durumlarda dağıtılan uygulamaları kapsar. Bu sorgu, düğüm adının kümedeki bir düğüme karşılık geldiğini gerektirir. Sağlanan düğüm adı kümedeki etkin Hizmet Dokusu düğümlerini işaret etmiyorsa sorgu başarısız olur.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --düğüm adı [Gerekli] | Düğümün adı. |
| --devam-belirteç | Devamı belirteç parametresi sonraki sonuç kümesini elde etmek için kullanılır. Sistemden elde edilen sonuçlar tek bir yanıta sığmıyorsa, BOŞ olmayan bir değere sahip bir devam belirteci API'nın yanıtına dahil edilir. Bu değer bir sonraki API çağrısına geçirildiğinde, API sonraki sonuç kümesini döndürür. Başka sonuç yoksa, devam belirteci bir değer içermez. Bu parametrenin değeri URL kodlanmış olmamalıdır. |
| --include-health-state | Bir varlığın sistem durumu ekleyin. Bu parametre yanlış sayılsa veya belirtilmemişse, döndürülen sistem durumu "Bilinmiyor" olur. Doğru olarak ayarlandığında, sorgu, sonuçlar birleştirilmeden önce düğüme ve sistem hizmetine paralel olarak gider. Sonuç olarak, sorgu daha pahalıdır ve daha uzun sürebilir. |
| --max-sonuçlar | Sayfalı sorguların bir parçası olarak döndürülecek en fazla sonuç sayısı. Bu parametre, döndürülen sonuç sayısındaki üst sınırı tanımlar. Döndürülen sonuçlar, yapılandırmada tanımlanan maksimum ileti boyutu kısıtlamalarına göre iletiye sığmazsa, belirtilen maksimum sonuçlardan daha az olabilir. Bu parametre sıfır veya belirtilmemişse, sayfalı sorgu, iade iletisindeki sığan mümkün olduğunca çok sonuç içerir. |
| --zaman ayarı -t | İşlemi saniyeler içinde gerçekleştirmek için sunucu zaman. Bu zaman sonu, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametrenin varsayılan değeri 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-application-health"></a>sfctl uygulama sağlık
Servis kumaşı uygulamasının sağlığını alır.

Servis kumaşı uygulamasının sağlık durumunu verir. Yanıt, Tamam, Hata veya Uyarı durumu bildirir. Varlık sistem durumu deposunda bulunmazsa, Hata döndürecektir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --uygulama kimliği [Gerekli] | Uygulamanın kimliği. Bu genellikle 'kumaş'\:URI düzeni olmadan uygulamanın tam adıdır. Sürüm 6.0'dan başlayarak hiyerarşik adlar\~" " karakteriyle sınırlandırılır. Örneğin, uygulama adı "kumaş\:/myapp/app1" ise, uygulama kimliği 6.0+ ve önceki sürümlerde "myapp/app1" olarak "myapp\~app1" olacaktır. |
| --dağıtılmış uygulamalar-sağlık-durum-filtre | Dağıtılan uygulamaların filtrelemesine izin verir sistem durumu nesneleri, sistem durumu durumuna göre uygulama durumu sorgusu sonucunda döndürülür. Bu parametre için olası değerler, aşağıdaki sistem durumu durumlarından birinin tamsayı değerini içerir. Yalnızca filtreyle eşleşen dağıtılmış uygulamalar döndürülür. Dağıtılan tüm uygulamalar, toplu sistem durumunu değerlendirmek için kullanılır. Belirtilmemişse, tüm girişler döndürülür. Durum değerleri bayrak tabanlı numaralandırmadır, bu nedenle değer bitwise 'VEYA' işleci kullanılarak elde edilen bu değerlerin bir birleşimi olabilir. Örneğin, sağlanan değer 6 ise, Ok (2) ve Warning (4) HealthState değerine sahip dağıtılan uygulamaların sistem durumu döndürülür.  <br> - Varsayılan - Varsayılan değer. Herhangi bir HealthState ile eşleşir. Değer sıfırdır.  <br> - Yok - Herhangi bir HealthState değeriyle eşleşmeyen filtre. Belirli bir durum koleksiyonunda sonuç vermek için kullanılır. Değeri 1'dir.  <br> - Tamam - HealthState değeri Ok ile giriş eşleşen filtre. Değeri 2'dir.  <br> - Uyarı - HealthState değer Uyarısı ile girdi eşleşen filtre. Değeri 4'dür.  <br> - Hata - HealthState değer Hatası ile giriş eşleşen filtre. Değeri 8'dir.  <br> - Tümü - Herhangi bir HealthState değeriyle girişle eşleşen filtre. Değeri 65535' tir. |
| --olaylar-sağlık-durum-filtre | HealthEvent nesnelerinin toplanmasının sistem durumu durumuna göre filtrelemesine olanak tanır. Bu parametre için olası değerler, aşağıdaki sistem durumu durumlarından birinin tamsayı değerini içerir. Yalnızca filtreyle eşleşen olaylar döndürülür. Tüm olaylar toplu sağlık durumunu değerlendirmek için kullanılır. Belirtilmemişse, tüm girişler döndürülür. Durum değerleri bayrak tabanlı numaralandırmadır, bu nedenle değer bitwise 'VEYA' işleci kullanılarak elde edilen bu değerlerin bir birleşimi olabilir. Örneğin, sağlanan değer 6 ise, Ok (2) ve Warning (4) HealthState değerine sahip tüm olaylar döndürülür.  <br> - Varsayılan - Varsayılan değer. Herhangi bir HealthState ile eşleşir. Değer sıfırdır.  <br> - Yok - Herhangi bir HealthState değeriyle eşleşmeyen filtre. Belirli bir durum koleksiyonunda sonuç vermek için kullanılır. Değeri 1'dir.  <br> - Tamam - HealthState değeri Ok ile giriş eşleşen filtre. Değeri 2'dir.  <br> - Uyarı - HealthState değer Uyarısı ile girdi eşleşen filtre. Değeri 4'dür.  <br> - Hata - HealthState değer Hatası ile giriş eşleşen filtre. Değeri 8'dir.  <br> - Tümü - Herhangi bir HealthState değeriyle girişle eşleşen filtre. Değeri 65535' tir. |
| --dışlama-sağlık-istatistik | Sistem durumu istatistiklerinin sorgu sonucunun bir parçası olarak döndürülüp döndürülmemesi gerektiğini gösterir. Varsayılan olarak false. İstatistikler, ok, uyarı ve hata durumu sağlık durumundaki çocuk varlıkların sayısını gösterir. |
| --hizmetler-sağlık-durum filtresi | Hizmetler sağlık durumu durum nesnelerinin filtrelemesine izin verir, sağlık durumuna göre hizmet durumu sorgusu sonucunda döndürülen nesneler. Bu parametre için olası değerler, aşağıdaki sistem durumu durumlarından birinin tamsayı değerini içerir. Yalnızca filtreyle eşleşen hizmetler döndürülür. Tüm hizmetler, toplu sağlık durumunu değerlendirmek için kullanılır. Belirtilmemişse, tüm girişler döndürülür. Durum değerleri bayrak tabanlı numaralandırmadır, bu nedenle değer bitwise 'VEYA' işleci kullanılarak elde edilen bu değerlerin bir birleşimi olabilir. Örneğin, sağlanan değer 6 ise, Ok (2) ve Uyarı (4) HealthState değerine sahip hizmetlerin sağlık durumu döndürülür.  <br> - Varsayılan - Varsayılan değer. Herhangi bir HealthState ile eşleşir. Değer sıfırdır.  <br> - Yok - Herhangi bir HealthState değeriyle eşleşmeyen filtre. Belirli bir durum koleksiyonunda sonuç vermek için kullanılır. Değeri 1'dir.  <br> - Tamam - HealthState değeri Ok ile giriş eşleşen filtre. Değeri 2'dir.  <br> - Uyarı - HealthState değer Uyarısı ile girdi eşleşen filtre. Değeri 4'dür.  <br> - Hata - HealthState değer Hatası ile giriş eşleşen filtre. Değeri 8'dir.  <br> - Tümü - Herhangi bir HealthState değeriyle girişle eşleşen filtre. Değeri 65535' tir. |
| --zaman ayarı -t | İşlemi saniyeler içinde gerçekleştirmek için sunucu zaman. Bu zaman sonu, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametrenin varsayılan değeri 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-application-info"></a>sfctl uygulama bilgileri
Service Fabric uygulaması hakkında bilgi alır.

Hizmet Kumaşı kümesinde oluşturulan ve adı parametre olarak belirtilen uygulamayla eşleşen uygulama yla ilgili bilgileri verir. Yanıt, uygulamayla ilgili adı, türü, durumu, parametreleri ve diğer ayrıntıları içerir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --uygulama kimliği [Gerekli] | Uygulamanın kimliği. Bu genellikle 'kumaş'\:URI düzeni olmadan uygulamanın tam adıdır. Sürüm 6.0'dan başlayarak hiyerarşik adlar\~" " karakteriyle sınırlandırılır. Örneğin, uygulama adı "kumaş\:/myapp/app1" ise, uygulama kimliği 6.0+ ve önceki sürümlerde "myapp/app1" olarak "myapp\~app1" olacaktır. |
| --dışlama-uygulama-parametreleri | Uygulama parametrelerinin sonucun dışında tutulup tutulmayacağını belirten bayrak. |
| --zaman ayarı -t | İşlemi saniyeler içinde gerçekleştirmek için sunucu zaman. Bu zaman sonu, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametrenin varsayılan değeri 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-application-list"></a>sfctl uygulama listesi
Belirtilen filtrelerle eşleşen Hizmet Dokusu kümesinde oluşturulan uygulamaların listesini alır.

Hizmet Kumaşı kümesinde oluşturulan veya oluşturulma sürecinde oluşturulan uygulamalar la ilgili bilgileri alır ve belirtilen filtrelerle eşleşir. Yanıt, uygulamayla ilgili adı, türü, durumu, parametreleri ve diğer ayrıntıları içerir. Uygulamalar bir sayfaya sığmazsa, bir sayfa sonuç döndürülür ve bir sonraki sayfayı almak için kullanılabilecek bir devam belirteci döndürülür. Filtreler ApplicationTypeName ve ApplicationDefinitionKindFilter aynı anda belirtilemez.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --uygulama tanımı-tür-filtre | Service Fabric uygulamasını tanımlamak için kullanılan mekanizma olan ApplicationDefinitionKind'i filtrelemek için kullanılır.  <br> - Varsayılan - "Tümü" seçmekle aynı işlevi gerçekleştiren varsayılan değer. Değer 0'dır.  <br> - Tüm - Girişle herhangi bir ApplicationDefinitionKind değeriyle eşleşen filtre. Değeri 65535' tir.  <br> - ServiceFabricApplicationDescription - ApplicationDefinitionKind değeri ServiceFabricApplicationDescription ile girdi eşleşen filtre. Değeri 1'dir.  <br> - Oluştur - Girişle ApplicationDefinitionKind değeri Oluştur ile eşleşen filtre. Değeri 2'dir. |
| --uygulama türü-adı | Sorgulanacak uygulamaları filtrelemek için kullanılan uygulama türü adı. Bu değer uygulama türü sürümünü içermemelidir. |
| --devam-belirteç | Devamı belirteç parametresi sonraki sonuç kümesini elde etmek için kullanılır. Sistemden elde edilen sonuçlar tek bir yanıta sığmıyorsa, BOŞ olmayan bir değere sahip bir devam belirteci API'nın yanıtına dahil edilir. Bu değer bir sonraki API çağrısına geçirildiğinde, API sonraki sonuç kümesini döndürür. Başka sonuç yoksa, devam belirteci bir değer içermez. Bu parametrenin değeri URL kodlanmış olmamalıdır. |
| --dışlama-uygulama-parametreleri | Uygulama parametrelerinin sonucun dışında tutulup tutulmayacağını belirten bayrak. |
| --max-sonuçlar | Sayfalı sorguların bir parçası olarak döndürülecek en fazla sonuç sayısı. Bu parametre, döndürülen sonuç sayısındaki üst sınırı tanımlar. Döndürülen sonuçlar, yapılandırmada tanımlanan maksimum ileti boyutu kısıtlamalarına göre iletiye sığmazsa, belirtilen maksimum sonuçlardan daha az olabilir. Bu parametre sıfır veya belirtilmemişse, sayfalı sorgu, iade iletisindeki sığan mümkün olduğunca çok sonuç içerir. |
| --zaman ayarı -t | İşlemi saniyeler içinde gerçekleştirmek için sunucu zaman. Bu zaman sonu, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametrenin varsayılan değeri 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-application-load"></a>sfctl uygulama yükü
Service Fabric uygulaması hakkında yük bilgileri alır.

Hizmet Kumaşı kümesinde oluşturulan ve adı parametre olarak belirtilen uygulamayla eşleşen uygulama yla ilgili yük bilgilerini verir. Yanıt, adı, minimum düğümleri, maksimum düğümleri, uygulamanın şu anda işgal etmekte olduğu düğüm sayısını ve uygulama yla ilgili uygulama yükü metrik bilgilerini içerir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --uygulama kimliği [Gerekli] | Uygulamanın kimliği. Bu genellikle 'kumaş'\:URI düzeni olmadan uygulamanın tam adıdır. Sürüm 6.0'dan başlayarak hiyerarşik adlar\~" " karakteriyle sınırlandırılır. Örneğin, uygulama adı "kumaş\:/myapp/app1" ise, uygulama kimliği 6.0+ ve önceki sürümlerde "myapp/app1" olarak "myapp\~app1" olacaktır. |
| --zaman ayarı -t | İşlemi saniyeler içinde gerçekleştirmek için sunucu zaman. Bu zaman sonu, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametrenin varsayılan değeri 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-application-manifest"></a>sfctl başvuru bildirimi
Bir uygulama türünü açıklayan bildirimi alır.

Yanıt, uygulama bildirimi XML'i dize olarak içerir.

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

## <a name="sfctl-application-provision"></a>sfctl uygulama hükmü
Harici mağazadaki '.sfpkg' paketini veya görüntü deposundaki uygulama paketini kullanarak bir Hizmet Kumaşı uygulama türünü kümeye göre hükümler veya kaydeder.

Kümeile hizmet kumaşı uygulama türünü hükümler. Yeni uygulamaların anlık olarak yapılabilmesi için bu hükmün verilmesi gerekmektedir. Provizyon işlemi, relativePathInImageStore tarafından belirtilen uygulama paketinde veya harici '.sfpkg' URI kullanılarak gerçekleştirilebilir. --dış hüküm ayarlılmadığı sürece, bu komut görüntü deposu hükmü bekler.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --uygulama-paket-download-uri | Uygulama paketinin HTTP veya HTTPS protokolleri kullanılarak indirilebileceği '.sfpkg' uygulama paketine giden yol. <br><br> Sadece tedarik tür dış mağaza için. Uygulama paketi, dosyayı indirmek için GET işlemi sağlayan harici bir depoda saklanabilir. Desteklenen protokoller HTTP ve HTTPS'dir ve yol READ erişimine izin vermelidir. |
| --uygulama türü-yapı-yolu | Sadece provision kind görüntü deposu için. Önceki yükleme işlemi sırasında belirtilen görüntü deposundaki uygulama paketinin göreli yolu. |
| --uygulama türü-adı | Sadece tedarik tür dış mağaza için. Uygulama türü adı, uygulama bildiriminde bulunan uygulama türünün adını gösterir. |
| --uygulama türü-sürüm | Sadece tedarik tür dış mağaza için. Uygulama türü sürümü, uygulama bildiriminde bulunan uygulama türünün sürümünü temsil eder. |
| --dış-karşılık | Uygulama paketinin kaydedilebildiği veya sağlanabileceği yer. Hükmün daha önce harici bir mağazaya yüklenen bir uygulama paketi için olduğunu gösterir. Uygulama paketi *.sfpkg uzantısı ile sona erer. |
| --bekleme | Sağlamanın eşzamanlı olarak gerçekleşip gerçekleşmemesi gerektiğini gösterir. <br><br> Doğru ayarlandığında, istek sistem tarafından kabul edildiğinde provizyon işlemi geri döner ve sağlama işlemi herhangi bir zaman önce sınırı olmadan devam eder. Varsayılan değer false'tur. Büyük uygulama paketleri için, değeri doğru olarak ayarlamanızı öneririz. |
| --zaman ayarı -t | Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-application-report-health"></a>sfctl uygulama raporu-sağlık
Service Fabric uygulaması hakkında bir sağlık raporu gönderir.

Belirtilen Service Fabric uygulamasının sağlık durumunu bildirir. Rapor, sağlık raporunun kaynağı ve raporlandığı özellik hakkındaki bilgileri içermelidir. Rapor, sağlık deposuna ileten Bir Hizmet Kumaşı ağ geçidi Uygulamasına gönderilir. Rapor ağ geçidi tarafından kabul edilebilir, ancak ek doğrulama sonra sağlık deposu tarafından reddedilir. Örneğin, sistem durumu deposu, eski bir sıra numarası gibi geçersiz bir parametre nedeniyle raporu reddedebilir. Raporun sağlık deposunda uygulanıp uygulanmadığını görmek için uygulama durumunu alın ve raporun görünüp görünmediğini kontrol edin.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --uygulama kimliği [Gerekli] | Uygulamanın kimliği. <br><br> Bu genellikle 'kumaş'\:URI düzeni olmadan uygulamanın tam adıdır. Sürüm 6.0'dan başlayarak hiyerarşik adlar\~' ' karakteriyle sınırlandırılır. Örneğin, uygulama adı 'kumaş\:/myapp/app1' ise, uygulama kimliği 6.0+ ve önceki sürümlerde 'myapp/app1' olarak 'myapp/app1'\~olacaktır. |
| --sağlık-özellik [Gerekli] | Sağlık bilgilerinin özelliği. <br><br> Bir varlık, farklı özellikler için sistem durumu raporları olabilir. Özellik, muhabirin raporu tetikleyen durum durumunu kategorilere ayırmaesnekliğine izin vermek için sabit bir numaralandırma değil, bir dizedir. Örneğin, SourceId "LocalWatchdog" olan bir muhabir, kullanılabilir diskin bir düğümüzerindeki durumunu izleyebilir, böylece bu düğümdeki "AvailableDisk" özelliğini bildirebilir. Aynı muhabir düğüm bağlantısını izleyebilir, böylece aynı düğümüzerinde bir özellik "Bağlantı" bildirebilir. Sağlık deposunda, bu raporlar belirtilen düğüm için ayrı sağlık olayları olarak kabul edilir. Kaynak Kimliği ile birlikte, özellik benzersiz sağlık bilgilerini tanımlar. |
| --sağlık durumu [Gerekli] | Olası değerler\: arasında 'Geçersiz', 'Tamam', 'Uyarı', 'Hata', 'Bilinmiyor' sayılabilir. |
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

## <a name="sfctl-application-type"></a>sfctl uygulama türü
Hizmet Kumaşı kümesindeki uygulama türlerinin listesini tam olarak belirtilen ada eşleşen alır.

Hizmet Kumaşı kümesinde sağlanan veya verilen uygulama türleri hakkındaki bilgileri verir. Bu sonuçlar, adı parametre olarak tam olarak belirtilenle eşleşen ve verilen sorgu parametrelerine uyan uygulama türleridir. Uygulama türü adı ile eşleşen uygulama türünün tüm sürümleri döndürülür ve her sürüm tek bir uygulama türü olarak döndürülür. Yanıt, uygulama türüyle ilgili adı, sürümü, durumu ve diğer ayrıntıları içerir. Bu, bir sayfaya sığmazsa, bir sayfanın yanı sıra bir sonraki sayfayı almak için kullanılabilecek bir devam belirteci döndürülen tüm uygulama türlerinin yanı sıra, sayfalı bir sorgudur. Örneğin, 10 uygulama türü varsa, ancak bir sayfa yalnızca ilk üç uygulama türüne uyuyorsa veya max sonuçları 3 olarak ayarlanmışsa, üç döndürülür. Sonuçların geri kalanına erişmek için sonraki sorguda döndürülen devam belirteci kullanarak sonraki sayfaları alın. Sonraki sayfalar yoksa boş bir devam belirteci döndürülür.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --uygulama türü adı [Gerekli] | Uygulama türünün adı. |
| --uygulama türü-sürüm | Uygulama türünün sürümü. |
| --devam-belirteç | Devamı belirteç parametresi sonraki sonuç kümesini elde etmek için kullanılır. Sistemden elde edilen sonuçlar tek bir yanıta sığmıyorsa, BOŞ olmayan bir değere sahip bir devam belirteci API'nın yanıtına dahil edilir. Bu değer bir sonraki API çağrısına geçirildiğinde, API sonraki sonuç kümesini döndürür. Başka sonuç yoksa, devam belirteci bir değer içermez. Bu parametrenin değeri URL kodlanmış olmamalıdır. |
| --dışlama-uygulama-parametreleri | Uygulama parametrelerinin sonucun dışında tutulup tutulmayacağını belirten bayrak. |
| --max-sonuçlar | Sayfalı sorguların bir parçası olarak döndürülecek en fazla sonuç sayısı. Bu parametre, döndürülen sonuç sayısındaki üst sınırı tanımlar. Döndürülen sonuçlar, yapılandırmada tanımlanan maksimum ileti boyutu kısıtlamalarına göre iletiye sığmazsa, belirtilen maksimum sonuçlardan daha az olabilir. Bu parametre sıfır veya belirtilmemişse, sayfalı sorgu, iade iletisindeki sığan mümkün olduğunca çok sonuç içerir. |
| --zaman ayarı -t | İşlemi saniyeler içinde gerçekleştirmek için sunucu zaman. Bu zaman sonu, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametrenin varsayılan değeri 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-application-type-list"></a>sfctl uygulama türü listesi
Hizmet Kumaşı kümesindeki uygulama türlerinin listesini alır.

Hizmet Kumaşı kümesinde sağlanan veya verilen uygulama türleri hakkındaki bilgileri verir. Uygulama türünün her sürümü bir uygulama türü olarak döndürülür. Yanıt, uygulama türüyle ilgili adı, sürümü, durumu ve diğer ayrıntıları içerir. Bu, bir sayfaya sığmazsa, bir sayfanın yanı sıra bir sonraki sayfayı almak için kullanılabilecek bir devam belirteci döndürülen tüm uygulama türlerinin yanı sıra, sayfalı bir sorgudur. Örneğin, 10 uygulama türü varsa, ancak bir sayfa yalnızca ilk üç uygulama türüne uyuyorsa veya max sonuçları 3 olarak ayarlanmışsa, üç döndürülür. Sonuçların geri kalanına erişmek için sonraki sorguda döndürülen devam belirteci kullanarak sonraki sayfaları alın. Sonraki sayfalar yoksa boş bir devam belirteci döndürülür.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --uygulama türü-tanım-tür-filtre | Service Fabric uygulama türünü tanımlamak için kullanılan mekanizma olan ApplicationTypeDefinitionKind'i filtrelemek için kullanılır.  <br> - Varsayılan - "Tümü" seçmekle aynı işlevi gerçekleştiren varsayılan değer. Değer 0'dır.  <br> - Tüm - Herhangi bir ApplicationTypeDefinitionKind değeri ile giriş eşleşen filtre. Değeri 65535' tir.  <br> - ServiceFabricApplicationPackage - ApplicationTypeDefinitionKind değeri ServiceFabricApplicationPackage ile girdi eşleşen filtre. Değeri 1'dir.  <br> - Oluştur - Girişle ApplicationTypeDefinitionKind değeri Oluştur ile eşleşen filtre. Değeri 2'dir. |
| --devam-belirteç | Devamı belirteç parametresi sonraki sonuç kümesini elde etmek için kullanılır. Sistemden elde edilen sonuçlar tek bir yanıta sığmıyorsa, BOŞ olmayan bir değere sahip bir devam belirteci API'nın yanıtına dahil edilir. Bu değer bir sonraki API çağrısına geçirildiğinde, API sonraki sonuç kümesini döndürür. Başka sonuç yoksa, devam belirteci bir değer içermez. Bu parametrenin değeri URL kodlanmış olmamalıdır. |
| --dışlama-uygulama-parametreleri | Uygulama parametrelerinin sonucun dışında tutulup tutulmayacağını belirten bayrak. |
| --max-sonuçlar | Sayfalı sorguların bir parçası olarak döndürülecek en fazla sonuç sayısı. Bu parametre, döndürülen sonuç sayısındaki üst sınırı tanımlar. Döndürülen sonuçlar, yapılandırmada tanımlanan maksimum ileti boyutu kısıtlamalarına göre iletiye sığmazsa, belirtilen maksimum sonuçlardan daha az olabilir. Bu parametre sıfır veya belirtilmemişse, sayfalı sorgu, iade iletisindeki sığan mümkün olduğunca çok sonuç içerir. |
| --zaman ayarı -t | İşlemi saniyeler içinde gerçekleştirmek için sunucu zaman. Bu zaman sonu, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametrenin varsayılan değeri 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-application-unprovision"></a>sfctl uygulaması unprovision
Hizmet Kumaşı uygulama türünü kümeden kaldırır veya kaydeder.

Bu işlem yalnızca uygulama türündeki tüm uygulama örnekleri silinmişse gerçekleştirilebilir. Uygulama türü kayıtsız kaldıktan sonra, bu özel uygulama türü için yeni uygulama örnekleri oluşturulamaz.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --uygulama türü adı [Gerekli] | Uygulama türünün adı. |
| --uygulama türü sürümü [Gerekli] | Uygulama bildiriminde tanımlanan uygulama türünün sürümü. |
| --async-parametre | Hükümsüzlük olup olmadığını belirten bayrak, eşzamanlı olarak gerçekleşir. Doğru ayarlandığında, istek sistem tarafından kabul edildiğinde, karşılıksız alma işlemi döner ve hükümsüz işlem herhangi bir zaman önce sınırı olmadan devam eder. Varsayılan değer false'tur. Ancak, sağlanan büyük uygulama paketleri için doğru ayarlamanızı öneririz. |
| --zaman ayarı -t | İşlemi saniyeler içinde gerçekleştirmek için sunucu zaman. Bu zaman sonu, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametrenin varsayılan değeri 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-application-upgrade"></a>sfctl uygulama yükseltme
Service Fabric kümesinde bir uygulamayı yükseltmeye başlar.

Sağlanan uygulama yükseltme parametrelerini doğrular ve parametreler geçerliyse uygulamayı yükseltmeye başlar. Yükseltme açıklamasının varolan uygulama açıklamasının yerini alacağına dikkat edin. Bu, parametreler belirtilmemişse, uygulamalardaki varolan parametrelerin boş parametreler listesiyle birlikte üzerine yazılması anlamına gelir. Bu, uygulama bildirimindeki parametrelerin varsayılan değerini kullanarak uygulamayla sonuçlanır.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --uygulama kimliği [Gerekli] | Uygulamanın kimliği. <br><br> Bu genellikle 'kumaş'\:URI düzeni olmadan uygulamanın tam adıdır. Sürüm 6.0'dan başlayarak hiyerarşik adlar\~" " karakteriyle sınırlandırılır. Örneğin, uygulama adı "kumaş\:/myapp/app1" ise, uygulama kimliği 6.0+ ve önceki sürümlerde "myapp/app1" olarak "myapp\~app1" olacaktır. |
| --uygulama sürümü [Gerekli] | Uygulama yükseltmesi için hedef uygulama türü sürümü (uygulama bildiriminde bulunan). |
| --parametreler [Gerekli] | Uygulamayı yükseltirken uygulanacak uygulama parametresi geçersiz kılmalarının JSON kodlanmış listesi. |
| --varsayılan hizmet-sağlık ilkesi | JSON, bir hizmet türünün durumunu değerlendirmek için varsayılan olarak kullanılan sistem durumu ilkesinin belirtimini kodladı. |
| --hata-eylem | İzlenen bir yükseltme izleme ilkesi veya sistem durumu ilkesi ihlalleriyle karşılaştığında gerçekleştirecek eylem. |
| --kuvvet yeniden başlatma | Kod sürümü değişmemiş olsa bile yükseltme sırasında işlemleri zorla yeniden başlatın. |
| --sağlık-check-retry-zaman- zaman | Uygulama veya küme sağlıklı değilse, sistem durumu denetimleri gerçekleştirme girişimleri arasındaki süre.  Varsayılan\: PT0H10M0S. |
| --sağlık-kontrol-kararlı-süresi | Yükseltme bir sonraki yükseltme etki alanına geçmeden önce uygulamanın veya kümenin sağlıklı kalması gereken süre.  Varsayılan\: PT0H2M0S. <br><br> İlk olarak ISO 8601 süresini temsil eden bir dize olarak yorumlanır. Bu başarısız olursa, o zaman milisaniye toplam sayısını temsil eden bir sayı olarak yorumlanır. |
| --sağlık-kontrol-bekleme süresi | Sistem durumu denetimleri işlemini başlatmadan önce bir yükseltme etki alanını tamamladıktan sonra bekleme süresi.  Varsayılan\: 0. |
| --max-sağlıksız-uygulamalar | Sağlıksız dağıtılan uygulamaların izin verilen maksimum yüzdesi. 0 ile 100 arasında bir sayı olarak temsil edilir. |
| --mod | Yuvarlama yükseltmesi sırasında durumu izlemek için kullanılan mod.  Varsayılan\: UnmonitoredAuto. |
| --çoğaltma-ayar-çek-zaman dilimi | Yükseltme etki alanının işlenmesini engellemek ve beklenmeyen sorunlar olduğunda kullanılabilirlik kaybını önlemek için en yüksek süre. Saniyecinsinden ölçülür. |
| --hizmet-sağlık politikası | JSON, hizmet türü adı başına hizmet türü sağlık ilkesine sahip haritayı kodladı. Harita boş varsayılan olmak. |
| --zaman ayarı -t | Varsayılan\: 60. |
| --upgrade-etki alanı-zaman ayarı | FailureAction yürütülmeden önce her yükseltme etki alanının tamamlanması gereken süre.  Varsayılan\: P1067519DT02H48M05.4775807S. <br><br> İlk olarak ISO 8601 süresini temsil eden bir dize olarak yorumlanır. Bu başarısız olursa, o zaman milisaniye toplam sayısını temsil eden bir sayı olarak yorumlanır. |
| --yükseltme-zaman ayarı | FailureAction yürütülmeden önce genel yükseltmenin tamamlanması gereken süre.  Varsayılan\: P1067519DT02H48M05.4775807S. <br><br> İlk olarak ISO 8601 süresini temsil eden bir dize olarak yorumlanır. Bu başarısız olursa, o zaman milisaniye toplam sayısını temsil eden bir sayı olarak yorumlanır. |
| --uyarı-as-hata | Uyarıların hatalarla aynı önemle davranılıp işitilen olmadığını gösterir. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-application-upgrade-resume"></a>sfctl uygulama yükseltme-özgeçmiş
Hizmet Kumaşı kümesindeki bir uygulamayı yükseltmeye devam eder.

İzlenmeyen manuel Servis Kumaşı uygulama yükseltmesi devam eder. Service Fabric, aynı anda bir yükseltme etki alanını yükseltir. İzlenmeyen manuel yükseltmeler için, Service Fabric bir yükseltme etki alanını tamamladıktan sonra, bir sonraki yükseltme etki alanına geçmeden önce bu API'yi aramanızı bekler.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --uygulama kimliği [Gerekli] | Uygulamanın kimliği. Bu genellikle 'kumaş'\:URI düzeni olmadan uygulamanın tam adıdır. Sürüm 6.0'dan başlayarak hiyerarşik adlar\~" " karakteriyle sınırlandırılır. Örneğin, uygulama adı "kumaş\:/myapp/app1" ise, uygulama kimliği 6.0+ ve önceki sürümlerde "myapp/app1" olarak "myapp\~app1" olacaktır. |
| --upgrade-etki alanı adı [Gerekli] | Yükseltmeye devam etmek için yükseltme etki alanının adı. |
| --zaman ayarı -t | İşlemi saniyeler içinde gerçekleştirmek için sunucu zaman. Bu zaman sonu, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametrenin varsayılan değeri 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-application-upgrade-rollback"></a>sfctl uygulama yükseltme-geri alma
Service Fabric kümesinde bir uygulamanın şu anda devam eden yükseltmesini geri alma başlar.

Geçerli uygulama yükseltmesini önceki sürüme geri almaya başlar. Bu API yalnızca yeni sürüme doğru ilerleyen geçerli devam eden yükseltmeyi geri almak için kullanılabilir. Uygulama şu anda yükseltilmiyorsa, önceki sürüme geri dönmek de dahil olmak üzere istenen sürüme yükseltmek için StartApplicationUpgrade API'yi kullanın.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --uygulama kimliği [Gerekli] | Uygulamanın kimliği. Bu genellikle 'kumaş'\:URI düzeni olmadan uygulamanın tam adıdır. Sürüm 6.0'dan başlayarak hiyerarşik adlar\~" " karakteriyle sınırlandırılır. Örneğin, uygulama adı "kumaş\:/myapp/app1" ise, uygulama kimliği 6.0+ ve önceki sürümlerde "myapp/app1" olarak "myapp\~app1" olacaktır. |
| --zaman ayarı -t | İşlemi saniyeler içinde gerçekleştirmek için sunucu zaman. Bu zaman sonu, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametrenin varsayılan değeri 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-application-upgrade-status"></a>sfctl uygulama yükseltme durumu
Bu uygulamada gerçekleştirilen en son yükseltme için ayrıntıları alır.

Hata ayıklama uygulaması sağlık sorunlarına yardımcı olmak için ayrıntıları ile birlikte en son uygulama yükseltme durumu hakkında bilgi verir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --uygulama kimliği [Gerekli] | Uygulamanın kimliği. Bu genellikle 'kumaş'\:URI düzeni olmadan uygulamanın tam adıdır. Sürüm 6.0'dan başlayarak hiyerarşik adlar\~" " karakteriyle sınırlandırılır. Örneğin, uygulama adı "kumaş\:/myapp/app1" ise, uygulama kimliği 6.0+ ve önceki sürümlerde "myapp/app1" olarak "myapp\~app1" olacaktır. |
| --zaman ayarı -t | İşlemi saniyeler içinde gerçekleştirmek için sunucu zaman. Bu zaman sonu, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametrenin varsayılan değeri 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-application-upload"></a>sfctl uygulama yükleme
Hizmet Kumaşı uygulama paketini görüntü deposuna kopyalayın.

Paketteki her dosya için yükleme ilerlemesini isteğe bağlı olarak görüntüleyin. Yükleme ilerleme `stderr`gönderilir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --yol [Gerekli] | Yerel uygulama paketine giden yol. |
| --sıkıştırmak | Yalnızca Service Fabric uygulama paketleri için geçerlidir. Sıkıştırılmış uygulama paketini içeren yeni bir klasör oluşturun veya varsayılan konuma veya sıkıştırılmış konum parametresi tarafından belirtilen konuma ve ardından yeni oluşturulan klasörü yükleyin. <br><br> Sfctl tarafından zaten sıkıştırılmış bir dosya varsa, bu bayrak ayarlanırsa üzerine yazılır. Dizin bir uygulama paketi değilse bir hata döndürülür. Zaten sıkıştırılmış bir uygulama paketi ise, klasör olduğu gibi üzerine kopyalanır. Varsayılan olarak, yeni oluşturulan sıkıştırılmış uygulama paketi başarılı bir yüklemeden sonra silinir. Yükleme başarılı olmazsa, lütfen sıkıştırılmış paketi gerektiği gibi el ile temizleyin. Sıkıştırılmış konum parametresi var olmayan dizinlere başvuruyorsa, silme işlemi oluşturulmuş olabilecek boş kirleri kaldırmaz. |
| --sıkıştırılmış konum | Sıkıştırılmış uygulama paketini koyacak konum. <br><br> Konum sağlanmazsa, sıkıştırılmış paket yol argümanında belirtilen ana dizinin altında sfctl_compressed_temp adlı yeni oluşturulan bir klasörün altına yerleştirilir. Örneğin, yol bağımsız değişkeninin\:C /FolderA/AppPkg değeri varsa, sıkıştırılmış\:paket C /FolderA/sfctl_compressed_temp/AppPkg'a eklenir. |
| --imagestore-string | Başvuru paketini yüklemek için hedef resim mağazası.  Varsayılan\: \:kumaş ImageStore. <br><br> Bir dosya konumuna yüklemek için bu parametreyi 'dosya'\:ile başlatın. Aksi takdirde değer, varsayılan değer gibi görüntü deposu bağlantı dizesi olmalıdır. |
| --sıkıştırılmış tut | Oluşturulan sıkıştırılmış paketi başarılı yükleme tamamlamada tutup tutmamak. <br><br> Ayarlanmazsa, başarılı bir şekilde tamamlandığında sıkıştırılmış uygulama paketleri silinir. Yükleme başarılı olmadıysa, uygulama paketi her zaman yeniden yüklemek için çıktı dizininde tutulur. |
| --ilerleme göster | Büyük paketler için dosya yükleme ilerlemeyi gösterin. |
| --zaman ayarı -t | Saniye cinsinden toplam zaman dilimi. Yükleme, yükleme zaman dışarısı geçtikten sonra hata yapmaz ve hatayı döndürer. Bu zaman tüm uygulama paketi için geçerlidir ve tek tek dosya zaman ları kalan zaman anına eşit olacaktır. Zaman alakart, uygulama paketini sıkıştırmak için gereken süreyi içermez.  Varsayılan\: 300. |

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
