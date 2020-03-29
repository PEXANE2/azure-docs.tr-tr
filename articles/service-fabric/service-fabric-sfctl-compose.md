---
title: Azure Servis Kumaş CLI- sfctl oluşturmak
description: Azure Service Fabric komut satırı arabirimi sfctl hakkında bilgi edinin. Docker Compose uygulamaları için komutların listesini içerir.
author: jeffj6123
ms.topic: reference
ms.date: 1/16/2020
ms.author: jejarry
ms.openlocfilehash: 1e40ca4e3c5ec8b7566646aa7ef723bd4c9e45a1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76906138"
---
# <a name="sfctl-compose"></a>sfctl compose
Docker Comcreate uygulamaları oluşturun, silin ve yönetin.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| oluşturmaya | Hizmet Kumaşı oluşturma dağıtımı oluşturur. |
| list | Hizmet Kumaşı kümesinde oluşturulan oluşturma dağıtımlarının listesini alır. |
| remove | Varolan bir Hizmet Kumaşı kümeden dağıtım oluşturur. |
| durum | Hizmet Kumaşı oluşturma dağıtımı hakkında bilgi alır. |
| yükseltme | Hizmet Kumaşı kümesinde bir oluşturma dağıtımını yükseltmeye başlar. |
| yükseltme-geri alma | Hizmet Kumaşı kümesinde bir oluşturma dağıtım yükseltmesini geri alma işlemi başlatılır. |
| yükseltme durumu | Bu Hizmet Kumaşı oluşturma dağıtımında gerçekleştirilen en son yükseltmenin ayrıntılarını alır. |

## <a name="sfctl-compose-create"></a>sfctl oluşturmak oluşturmak oluşturmak oluşturmak
Hizmet Kumaşı oluşturma dağıtımı oluşturur.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --dağıtım adı [Gerekli] | Dağıtımın adı. |
| --dosya yolu [Gerekli] | Hedef Docker Oluştur dosyasına giden yol. |
| --şifreli geçiş | Kapsayıcı kayıt defteri parolası için istekte almak yerine, zaten şifrelenmiş bir geçiş tümceciği kullanın. |
| --has-pass | Kapsayıcı kayıt defteriiçin bir parola ister. |
| --zaman ayarı -t | Varsayılan\: 60. |
| --kullanıcı | Konteyner kayıt defterine bağlanmak için kullanıcı adı. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-compose-list"></a>sfctl oluşturma listesi
Hizmet Kumaşı kümesinde oluşturulan oluşturma dağıtımlarının listesini alır.

Hizmet Kumaşı kümesinde oluşturulan veya oluşturulma sürecinde oluşturulan dağıtımları oluşturma durumu alır. Yanıt, dağıtımoluşturmayla ilgili adı, durumu ve diğer ayrıntıları içerir. Dağıtım listesi bir sayfaya sığmazsa, bir sayfa sonuç ve sonraki sayfayı almak için kullanılabilecek bir devam belirteci döndürülür.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --devam-belirteç | Devamı belirteç parametresi sonraki sonuç kümesini elde etmek için kullanılır. Sistemden elde edilen sonuçlar tek bir yanıta sığmıyorsa, BOŞ olmayan bir değere sahip bir devam belirteci API'nın yanıtına dahil edilir. Bu değer bir sonraki API çağrısına geçirildiğinde, API sonraki sonuç kümesini döndürür. Başka sonuç yoksa, devam belirteci bir değer içermez. Bu parametrenin değeri URL kodlanmış olmamalıdır. |
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

## <a name="sfctl-compose-remove"></a>sfctl kaldırmak oluşturmak
Varolan bir Hizmet Kumaşı kümeden dağıtım oluşturur.

Varolan bir Hizmet Kumaşı oluşturma dağıtımını siler.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --dağıtım adı [Gerekli] | Dağıtımın kimliği. |
| --zaman ayarı -t | İşlemi saniyeler içinde gerçekleştirmek için sunucu zaman. Bu zaman sonu, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametrenin varsayılan değeri 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-compose-status"></a>sfctl oluşturma durumu
Hizmet Kumaşı oluşturma dağıtımı hakkında bilgi alır.

Hizmet Kumaşı kümesinde oluşturulan ve adı parametre olarak belirtilenle eşleşen oluşturma işlemi sırasında oluşturulan veya oluşturulan oluşturma dağıtımının durumunu döndürür. Yanıt, dağıtımla ilgili adı, durumu ve diğer ayrıntıları içerir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --dağıtım adı [Gerekli] | Dağıtımın kimliği. |
| --zaman ayarı -t | İşlemi saniyeler içinde gerçekleştirmek için sunucu zaman. Bu zaman sonu, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametrenin varsayılan değeri 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-compose-upgrade"></a>sfctl yükseltme oluşturmak
Hizmet Kumaşı kümesinde bir oluşturma dağıtımını yükseltmeye başlar.

Sağlanan yükseltme parametrelerini doğrular ve parametreler geçerliyse dağıtımı yükseltmeye başlar.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --dağıtım adı [Gerekli] | Dağıtımın adı. |
| --dosya yolu [Gerekli] | Hedef Docker oluşturma dosyasına giden yol. |
| --varsayılan-svc-tipi-sağlık-harita | Hizmetlerin sağlığını değerlendirmek için kullanılan sağlık ilkesini açıklayan JSON kodlu sözlük. |
| --şifreli geçiş | Kapsayıcı kayıt defteri parolası için istekte almak yerine, zaten şifrelenmiş bir geçiş tümceciği kullanın. |
| --hata-eylem | Olası değerler\: arasında 'Geçersiz', 'Geri Alma', 'El Kitabı' sayılabilir. |
| --kuvvet yeniden başlatma | Kod sürümü değişmemiş olsa bile, işlemler yükseltme sırasında zorla yeniden başlatılır. <br><br> Yükseltme yalnızca yapılandırmayı veya verileri değiştirir. |
| --has-pass | Kapsayıcı kayıt defteriiçin bir parola ister. |
| --sağlık-kontrol-yeniden deneme | Uygulama veya küme sağlıklı değilse, sistem durumu denetimleri gerçekleştirme girişimleri arasındaki süre. |
| --sağlık-kontrol-kararlı | Yükseltme bir sonraki yükseltme etki alanına geçmeden önce uygulamanın veya kümenin sağlıklı kalması gereken süre. <br><br> İlk olarak ISO 8601 süresini temsil eden bir dize olarak yorumlanır. Bu başarısız olursa, o zaman milisaniye toplam sayısını temsil eden bir sayı olarak yorumlanır. |
| --sağlık-check-bekle | Sistem durumu denetimleri işlemini başlatmadan önce bir yükseltme etki alanını tamamladıktan sonra bekleme süresi. |
| --çoğaltma-ayar-denetimi | Yükseltme etki alanının işlenmesini engellemek ve beklenmeyen sorunlar olduğunda kullanılabilirlik kaybını önlemek için en yüksek süre. <br><br> Bu zaman aşımı sona erdiğinde, kullanılabilirlik kaybı sorunları ne olursa olsun yükseltme etki alanının işlenmesi devam eder. Zaman anına sıfırlama, her yükseltme etki alanının başında sıfırlanır. Geçerli değerler 0 ile 42949672925 arasındadır. |
| --svc tipi-sağlık-harita | JSON, farklı hizmet türlerinin durumunu değerlendirmek için kullanılan sistem durumu ilkelerini açıklayan nesnelerin listesini kodladı. |
| --zaman ayarı -t | Varsayılan\: 60. |
| --sağlıksız-uygulama | Bir hatayı bildirmeden önce izin verilen en yüksek sağlıksız uygulamaların yüzdesi. <br><br> Örneğin, uygulamaların %10'unun sağlıksız olmasını sağlamak için bu değer 10 olacaktır. Bu yüzde, küme hata olarak kabul edilmeden önce sağlıksız olabilecek uygulamaların en yüksek tolere edilen yüzdesini temsil eder. Yüzdeye saygı duyulur ancak en az bir sağlıksız uygulama varsa, sistem durumu Uyarı olarak değerlendirilir. Bu, kümedeki toplam uygulama örneği sayısına sağlıksız uygulama sayısı bölünerek hesaplanır. |
| --upgrade-etki alanı-zaman ayarı | FailureAction yürütülmeden önce her yükseltme etki alanının tamamlanması gereken süre. <br><br> İlk olarak ISO 8601 süresini temsil eden bir dize olarak yorumlanır. Bu başarısız olursa, o zaman milisaniye toplam sayısını temsil eden bir sayı olarak yorumlanır. |
| --yükseltme türü | Varsayılan\: Yuvarlama. |
| --yükseltme modu | Olası değerler\: arasında 'Geçersiz', 'İzlenmeyen Otomatik', 'İzlenmeyen Kılavuz', 'İzlenen'.  Varsayılan\: UnmonitoredAuto. |
| --yükseltme-zaman ayarı | FailureAction yürütülmeden önce genel yükseltmenin tamamlanması gereken süre. <br><br> İlk olarak ISO 8601 süresini temsil eden bir dize olarak yorumlanır. Bu başarısız olursa, o zaman milisaniye toplam sayısını temsil eden bir sayı olarak yorumlanır. |
| --kullanıcı | Konteyner kayıt defterine bağlanmak için kullanıcı adı. |
| --uyarı-as-hata | Uyarıların hatalarla aynı önemle davranılıp işitilen olmadığını gösterir. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-compose-upgrade-rollback"></a>sfctl yükseltme-geri alma oluşturmak
Hizmet Kumaşı kümesinde bir oluşturma dağıtım yükseltmesini geri alma işlemi başlatılır.

Bir hizmet dokusunun dağıtım yükseltmesi oluşturması.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --dağıtım adı [Gerekli] | Dağıtımın kimliği. |
| --zaman ayarı -t | İşlemi saniyeler içinde gerçekleştirmek için sunucu zaman. Bu zaman sonu, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametrenin varsayılan değeri 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-compose-upgrade-status"></a>sfctl oluşturma yükseltme durumu
Bu Hizmet Kumaşı oluşturma dağıtımında gerçekleştirilen en son yükseltmenin ayrıntılarını alır.

Hata ayıklama uygulaması sistem durumu sorunlarına yardımcı olmak için ayrıntıların yanı sıra oluşturma dağıtım yükseltmesinin durumu hakkındaki bilgileri verir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --dağıtım adı [Gerekli] | Dağıtımın kimliği. |
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
- Servis Kumaş ı CLI'yi [ayarlayın.](service-fabric-cli.md)
- [Örnek komut dosyalarını](/azure/service-fabric/scripts/sfctl-upgrade-application)kullanarak Service Fabric CLI'yi nasıl kullanacağınızı öğrenin.