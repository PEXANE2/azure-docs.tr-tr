---
title: Azure Servis Kumaş CLI- sfctl bölümü
description: Azure Service Fabric komut satırı arabirimi sfctl hakkında bilgi edinin. Bir hizmetin bölümlerini yönetmek için bir komut listesi içerir.
author: jeffj6123
ms.topic: reference
ms.date: 1/16/2020
ms.author: jejarry
ms.openlocfilehash: c038ef3266a727bf6984a5bd88ca540a589380db
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76905843"
---
# <a name="sfctl-partition"></a>sfctl partition
Herhangi bir hizmet için bölümleri sorgula ve yönet.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| veri kaybı | Bu API, belirtilen bölüm için veri kaybına neden olur. |
| veri kaybı-durumu | StartDataLoss API'sini kullanarak bir bölüm veri kaybı işleminin ilerlemesini alır. |
| Sağlık | Belirtilen Hizmet Kumaşı bölümü ne olursa, sistem durumunu alır. |
| bilgiler | Service Fabric bölümü hakkında bilgi alır. |
| list | Service Fabric hizmetinin bölümlerinin listesini alır. |
| yükleme | Belirtilen Service Fabric bölümü yük bilgilerini alır. |
| yük sıfırlama | Hizmet Kumaşı bölümü ndeki geçerli yükü sıfırlar. |
| çoğunluk-kayıp | Belirli bir devlet hizmeti bölümü için çoğunluk kaybına neden olur. |
| çoğunluk-kayıp-durum | StartQuorumLoss API'sini kullanmaya başlayan bir bölüm üzerinde çoğunluk kaybı işleminin ilerlemesini alır. |
| kurtarma | Hizmet Kumaşı kümesine, şu anda çoğunluk kaybına sıkışmış belirli bir bölümü kurtarmaya çalışması gerektiğini gösterir. |
| recover-all | Hizmet Kumaşı kümesine, şu anda çoğunluk kaybına bağlı olan hizmetleri (sistem hizmetleri dahil) kurtarmaya çalışması gerektiğini belirtir. |
| rapor-sağlık | Hizmet Kumaşı bölümü yle ilgili bir sağlık raporu gönderir. |
| restart | Bu API, belirtilen bölümün bazı veya tüm yinelemelerini veya örneklerini yeniden başlatacaktır. |
| yeniden başlatma durumu | Bir PartitionRestart işleminin ilerlemesini alır StartPartitionRestart kullanarak başladı. |
| svc adı | Bir bölüm için Service Fabric hizmetinin adını alır. |

## <a name="sfctl-partition-data-loss"></a>sfctl bölüm veri kaybı
Bu API, belirtilen bölüm için veri kaybına neden olur.

Bu bölümün OnDataLossAsync API bir çağrı tetikler.  Bu API, belirtilen bölüm için veri kaybına neden olur. Bu bölümün OnDataLoss API bir çağrı tetikler. Gerçek veri kaybı belirtilen DataLossMode bağlıdır.
- PartialDataLoss: Yalnızca bir çok çoğaltma kaldırılır ve OnDataLoss bölüm için tetiklenir, ancak gerçek veri kaybı uçuş içi çoğaltma nın varlığına bağlıdır.  
- FullDataLoss: Tüm yinelemeler kaldırılır, böylece tüm veriler kaybolur ve OnDataLoss tetiklenir. Bu API yalnızca hedef olarak devlet hizmeti ile çağrılmalıdır. Hedef olarak bu API'yi bir sistem hizmetiyle çağırmak tavsiye edilmez.

> [!NOTE]   
> Bu API çağrıldıktan sonra geri alınamaz. CancelOperation'ı aramak yalnızca yürütmeyi durdurur ve dahili sistem durumunu temizler. Komut veri kaybına neden olacak kadar ilerlediyse verileri geri yüklemez. Bu API ile başlatılan işlem hakkında bilgi döndürmek için aynı OperationId ile GetDataLossProgress API'yi arayın.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --veri kaybı-modu [Gerekli] | Bu enum, ne tür bir veri kaybına neden olacaklarını belirtmek için StartDataLoss API'sine aktarılır. |
| --operation-id [Gerekli] | Bu API'nin çağrısını tanımlayan bir GUID.  Bu, ilgili GetProgress API'sine aktarılır. |
| --partition-id [Gerekli] | Bölümün kimliği. |
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

## <a name="sfctl-partition-data-loss-status"></a>sfctl bölüm veri kaybı-durum
StartDataLoss API'sini kullanarak bir bölüm veri kaybı işleminin ilerlemesini alır.

İşlemKimliği'ni kullanarak StartDataLoss ile bir veri kaybı işleminin ilerlemesini başlatın.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --operation-id [Gerekli] | Bu API'nin çağrısını tanımlayan bir GUID.  Bu, ilgili GetProgress API'sine aktarılır. |
| --partition-id [Gerekli] | Bölümün kimliği. |
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

## <a name="sfctl-partition-health"></a>sfctl bölüm sağlık
Belirtilen Hizmet Kumaşı bölümü ne olursa, sistem durumunu alır.

Hizmette bildirilen sistem durumu durumuna göre bildirilen sistem durumu olaylarının toplanmasını filtrelemek için EventsHealthStateFilter'i kullanın. ÇoğaltmaSağlıkDurumu nesnelerinin toplanmasını bölmek için ReplicasHealthStateFilter'i kullanın. Sistem durumu deposunda olmayan bir bölüm belirtirseniz, bu istek bir hata döndürür.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --partition-id [Gerekli] | Bölümün kimliği. |
| --olaylar-sağlık-durum-filtre | HealthEvent nesnelerinin toplanmasının sistem durumu durumuna göre filtrelemesine olanak tanır. Bu parametre için olası değerler, aşağıdaki sistem durumu durumlarından birinin tamsayı değerini içerir. Yalnızca filtreyle eşleşen olaylar döndürülür. Tüm olaylar toplu sağlık durumunu değerlendirmek için kullanılır. Belirtilmemişse, tüm girişler döndürülür. Durum değerleri bayrak tabanlı numaralandırmadır, bu nedenle değer bitwise 'VEYA' işleci kullanılarak elde edilen bu değerlerin bir birleşimi olabilir. Örneğin, sağlanan değer 6 ise, Ok (2) ve Warning (4) HealthState değerine sahip tüm olaylar döndürülür.  <br> - Varsayılan - Varsayılan değer. Herhangi bir HealthState ile eşleşir. Değer sıfırdır.  <br> - Yok - Herhangi bir HealthState değeriyle eşleşmeyen filtre. Belirli bir durum koleksiyonunda sonuç vermek için kullanılır. Değeri 1'dir.  <br> - Tamam - HealthState değeri Ok ile giriş eşleşen filtre. Değeri 2'dir.  <br> - Uyarı - HealthState değer Uyarısı ile girdi eşleşen filtre. Değeri 4'dür.  <br> - Hata - HealthState değer Hatası ile giriş eşleşen filtre. Değeri 8'dir.  <br> - Tümü - Herhangi bir HealthState değeriyle girişle eşleşen filtre. Değeri 65535' tir. |
| --dışlama-sağlık-istatistik | Sistem durumu istatistiklerinin sorgu sonucunun bir parçası olarak döndürülüp döndürülmemesi gerektiğini gösterir. Varsayılan olarak false. İstatistikler, ok, uyarı ve hata durumu sağlık durumundaki çocuk varlıkların sayısını gösterir. |
| --yinelemeler-sağlık-durum-filtre | Partition üzerinde ReplicaHealthState nesnelerin toplama filtreleme sağlar. Değer, HealthStateFilter üyeleri nezdinde üyelerden veya bitwise işlemlerden elde edilebilir. Yalnızca filtreyle eşleşen yinelemeler döndürülür. Tüm yinelemeler, toplu sağlık durumunu değerlendirmek için kullanılır. Belirtilmemişse, tüm girişler döndürülür. Durum değerleri bayrak tabanlı numaralandırmadır, bu nedenle değer bitwise 'VEYA' işleci kullanılarak elde edilen bu değerlerin bir birleşimi olabilir. Örneğin, sağlanan değer 6 ise, Ok (2) ve Warning (4) healthState değerine sahip tüm olaylar döndürülür. Bu parametre için olası değerler, aşağıdaki sistem durumu durumlarından birinin tamsayı değerini içerir.  <br> - Varsayılan - Varsayılan değer. Herhangi bir HealthState ile eşleşir. Değer sıfırdır.  <br> - Yok - Herhangi bir HealthState değeriyle eşleşmeyen filtre. Belirli bir durum koleksiyonunda sonuç vermek için kullanılır. Değeri 1'dir.  <br> - Tamam - HealthState değeri Ok ile giriş eşleşen filtre. Değeri 2'dir.  <br> - Uyarı - HealthState değer Uyarısı ile girdi eşleşen filtre. Değeri 4'dür.  <br> - Hata - HealthState değer Hatası ile giriş eşleşen filtre. Değeri 8'dir.  <br> - Tümü - Herhangi bir HealthState değeriyle girişle eşleşen filtre. Değeri 65535' tir. |
| --zaman ayarı -t | İşlemi saniyeler içinde gerçekleştirmek için sunucu zaman. Bu zaman sonu, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametrenin varsayılan değeri 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-partition-info"></a>sfctl bölüm bilgi
Service Fabric bölümü hakkında bilgi alır.

Belirtilen bölüm hakkında bilgi alır. Yanıt, bölümleme kimliği, bölümleme düzeni bilgileri, bölüm, durum, sistem durumu ve bölümle ilgili diğer ayrıntılar tarafından desteklenen anahtarları içerir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --partition-id [Gerekli] | Bölümün kimliği. |
| --zaman ayarı -t | İşlemi saniyeler içinde gerçekleştirmek için sunucu zaman. Bu zaman sonu, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametrenin varsayılan değeri 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-partition-list"></a>sfctl bölüm listesi
Service Fabric hizmetinin bölümlerinin listesini alır.

Yanıt, bölümleme kimliği, bölümleme düzeni bilgileri, bölüm, durum, sistem durumu ve bölümle ilgili diğer ayrıntılar tarafından desteklenen anahtarları içerir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --service-id [Gerekli] | Hizmetin kimliği. Bu kimlik genellikle 'kumaş'\:URI düzeni olmadan hizmetin tam adıdır. Sürüm 6.0'dan başlayarak hiyerarşik adlar\~" " karakteriyle sınırlandırılır. Örneğin, hizmet adı "kumaş\:/myapp/app1/svc1" ise, hizmet kimliği 6.0+ ve önceki sürümlerde "myapp/app1/svc1"\~\~olacaktır. |
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

## <a name="sfctl-partition-load"></a>sfctl bölüm yükü
Belirtilen Service Fabric bölümü yük bilgilerini alır.

Belirtilen bir bölümün yükü hakkında bilgi verir. Yanıt, Hizmet Kumaşı bölümü için yük raporlarının listesini içerir. Her rapor, UTC'de yük metrik adını, değerini ve en son bildirilen zamanı içerir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --partition-id [Gerekli] | Bölümün kimliği. |
| --zaman ayarı -t | İşlemi saniyeler içinde gerçekleştirmek için sunucu zaman. Bu zaman sonu, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametrenin varsayılan değeri 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-partition-load-reset"></a>sfctl bölüm yük sıfırlama
Hizmet Kumaşı bölümü ndeki geçerli yükü sıfırlar.

Hizmet Kumaşı bölümü ndeki geçerli yükü hizmet için varsayılan yüke sıfırlar.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --partition-id [Gerekli] | Bölümün kimliği. |
| --zaman ayarı -t | İşlemi saniyeler içinde gerçekleştirmek için sunucu zaman. Bu zaman sonu, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametrenin varsayılan değeri 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-partition-quorum-loss"></a>sfctl bölüm çoğunluk-kayıp
Belirli bir devlet hizmeti bölümü için çoğunluk kaybına neden olur.

Bu API, hizmetinizde geçici bir çoğunluk kaybı durumu için yararlıdır. Bu API ile başlatılan işlem hakkında bilgi döndürmek için aynı OperationId ile GetQuorumLossProgress API'yi arayın. Bu yalnızca durum lu kalıcı (HasPersistedState==true) hizmetlerine çağrılabilir.  Bu API'yi devletsiz hizmetlerde veya yalnızca bellek tesbitindeki hizmetlerde kullanmayın.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --operation-id [Gerekli] | Bu API'nin çağrısını tanımlayan bir GUID.  Bu, ilgili GetProgress API'sine aktarılır. |
| --partition-id [Gerekli] | Bölümün kimliği. |
| --çoğunluk-kayıp-süresi [Gerekli] | Bölünmenin çoğunluk kaybında tutulacağı süre.  Bu saniye cinsinden belirtilmelidir. |
| --çoğunluk-kayıp-modu [Gerekli] | Bu enum StartQuorumLoss API için ne tür çoğunluk kaybı neden belirtmek için geçirilir. |
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

## <a name="sfctl-partition-quorum-loss-status"></a>sfctl bölüm çoğunluk-kayıp-durum
StartQuorumLoss API'sini kullanmaya başlayan bir bölüm üzerinde çoğunluk kaybı işleminin ilerlemesini alır.

Sağlanan OperationId'i kullanarak StartQuorumLoss ile başlayan bir çoğunluk kaybı işleminin ilerlemesini sağlar.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --operation-id [Gerekli] | Bu API'nin çağrısını tanımlayan bir GUID.  Bu, ilgili GetProgress API'sine aktarılır. |
| --partition-id [Gerekli] | Bölümün kimliği. |
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

## <a name="sfctl-partition-recover"></a>sfctl bölüm kurtarma
Hizmet Kumaşı kümesine, şu anda çoğunluk kaybına sıkışmış belirli bir bölümü kurtarmaya çalışması gerektiğini gösterir.

Bu işlem yalnızca aşağı olan yinelemelerin kurtarılamayacağı biliniyorsa gerçekleştirilmelidir. Bu API'nin yanlış kullanımı olası veri kaybına neden olabilir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --partition-id [Gerekli] | Bölümün kimliği. |
| --zaman ayarı -t | İşlemi saniyeler içinde gerçekleştirmek için sunucu zaman. Bu zaman sonu, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametrenin varsayılan değeri 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-partition-recover-all"></a>sfctl bölüm recover-all
Hizmet Kumaşı kümesine, şu anda çoğunluk kaybına bağlı olan hizmetleri (sistem hizmetleri dahil) kurtarmaya çalışması gerektiğini belirtir.

Bu işlem yalnızca aşağı olan yinelemelerin kurtarılamayacağı biliniyorsa gerçekleştirilmelidir. Bu API'nin yanlış kullanımı olası veri kaybına neden olabilir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --zaman ayarı -t | İşlemi saniyeler içinde gerçekleştirmek için sunucu zaman. Bu zaman sonu, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametrenin varsayılan değeri 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-partition-report-health"></a>sfctl bölüm raporu-sağlık
Hizmet Kumaşı bölümü yle ilgili bir sağlık raporu gönderir.

Belirtilen Hizmet Kumaşı bölümü sağlık durumunu bildirir. Rapor, sağlık raporunun kaynağı ve raporlandığı özellik hakkındaki bilgileri içermelidir. Rapor, sağlık deposuna ileten Bir Hizmet Dokusu ağ geçidi Bölümü'ne gönderilir. Rapor ağ geçidi tarafından kabul edilebilir, ancak ek doğrulama sonra sağlık deposu tarafından reddedilir. Örneğin, sistem durumu deposu, eski bir sıra numarası gibi geçersiz bir parametre nedeniyle raporu reddedebilir. Raporun sağlık deposunda uygulanıp uygulanmadığını görmek için, raporun olaylar bölümünde görünüp görünmediğini denetleyin.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --sağlık-özellik [Gerekli] | Sağlık bilgilerinin özelliği. <br><br> Bir varlık, farklı özellikler için sistem durumu raporları olabilir. Özellik, muhabirin raporu tetikleyen durum durumunu kategorilere ayırmaesnekliğine izin vermek için sabit bir numaralandırma değil, bir dizedir. Örneğin, SourceId "LocalWatchdog" olan bir muhabir, kullanılabilir diskin bir düğümüzerindeki durumunu izleyebilir, böylece bu düğümdeki "AvailableDisk" özelliğini bildirebilir. Aynı muhabir düğüm bağlantısını izleyebilir, böylece aynı düğümüzerinde bir özellik "Bağlantı" bildirebilir. Sağlık deposunda, bu raporlar belirtilen düğüm için ayrı sağlık olayları olarak kabul edilir. Kaynak Kimliği ile birlikte, özellik benzersiz sağlık bilgilerini tanımlar. |
| --sağlık durumu [Gerekli] | Olası değerler\: arasında 'Geçersiz', 'Tamam', 'Uyarı', 'Hata', 'Bilinmiyor' sayılabilir. |
| --partition-id [Gerekli] | Bölümün kimliği. |
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

## <a name="sfctl-partition-restart"></a>sfctl bölüm yeniden başlatma
Bu API, belirtilen bölümün bazı veya tüm yinelemelerini veya örneklerini yeniden başlatacaktır.

Bu API, başarısız olup olmadığını sınaması için yararlıdır. Devletsiz bir hizmet bölümü hedeflemek için kullanılırsa, RestartPartitionMode AllReplicasOrInstances olmalıdır. İlerlemeyi elde etmek için aynı OperationId'i kullanarak GetPartitionRestartProgress API'yi arayın.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --operation-id [Gerekli] | Bu API'nin çağrısını tanımlayan bir GUID.  Bu, ilgili GetProgress API'sine aktarılır. |
| --partition-id [Gerekli] | Bölümün kimliği. |
| --yeniden başlatma-partition-mode [Gerekli] | Hangi bölümlerin yeniden başlatılmasını açıklayın. |
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

## <a name="sfctl-partition-restart-status"></a>sfctl bölüm yeniden başlatma durumu
Bir PartitionRestart işleminin ilerlemesini alır StartPartitionRestart kullanarak başladı.

Sağlanan OperationId kullanarak StartPartitionRestart ile başlatılan bir PartitionRestart ilerleme alır.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --operation-id [Gerekli] | Bu API'nin çağrısını tanımlayan bir GUID.  Bu, ilgili GetProgress API'sine aktarılır. |
| --partition-id [Gerekli] | Bölümün kimliği. |
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

## <a name="sfctl-partition-svc-name"></a>sfctl bölüm svc-name
Bir bölüm için Service Fabric hizmetinin adını alır.

Belirtilen bölüm için hizmetin adını alır. Kümede bölüm kimliği yoksa 404 hatası döndürülür.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --partition-id [Gerekli] | Bölümün kimliği. |
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
