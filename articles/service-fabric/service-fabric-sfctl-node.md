---
title: Azure Servis Kumaş CLI- sfctl düğüm
description: Azure Service Fabric komut satırı arabirimi sfctl hakkında bilgi edinin. Küme düğümlerini yönetmek için bir komut listesi içerir.
author: jeffj6123
ms.topic: reference
ms.date: 1/16/2020
ms.author: jejarry
ms.openlocfilehash: 5881e6485003abd4fd23a7f6d06a428e768c00fa
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76905886"
---
# <a name="sfctl-node"></a>sfctl node
Küme oluşturan düğümleri yönetin.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| add-configuration-parametre-overrides | Belirtilen düğümüzerinde yapılandırma geçersiz kılar listesini ekler. |
| Devre dışı bırakmak | Belirtilen devre dışı bırakma amacıyla Hizmet Kumaşı küme düğümünü devre dışı bırakın. |
| seçin | Şu anda devre dışı bırakılan Hizmet Kumaşı küme düğümünü etkinleştirin. |
| get-configuration-overrides | Belirtilen düğümdeki yapılandırma geçersiz kılma listesini alır. |
| Sağlık | Service Fabric düğümünün sağlığını alır. |
| bilgiler | Service Fabric kümesindeki belirli bir düğüm hakkındaki bilgileri alır. |
| list | Service Fabric kümesindeki düğümlerin listesini alır. |
| yükleme | Servis Kumaş düğümünün yük bilgilerini alır. |
| kaldırma yapılandırma-geçersiz kılar | Belirtilen düğümdeki yapılandırma geçersiz kılmalarını kaldırır. |
| kaldırma durumu | Hizmet Kumaşı'na, bir düğümüzerinde kalıcı durum kalıcı olarak kaldırıldığını veya kaybolduğunu bildirer. |
| rapor-sağlık | Hizmet Kumaş düğümü hakkında bir sağlık raporu gönderir. |
| restart | Service Fabric küme düğümlerini yeniden başlatır. |
| Geçiş | Küme düğümlerini başlatır veya durdurur. |
| geçiş durumu | StartNodeTransition kullanarak başlatılan bir işlemin ilerlemesini alır. |

## <a name="sfctl-node-add-configuration-parameter-overrides"></a>sfctl düğüm ekle-yapılandırma-parametre-overrides
Belirtilen düğümüzerinde yapılandırma geçersiz kılar listesini ekler.

Bu api, belirtilen düğümüzerinde varolan tüm yapılandırma geçersiz kılmaları eklemeye olanak sağlar.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --config-parametre-geçersiz kılma-listesi [Gerekli] | Yapılandırma geçersiz kılmalistesi eklemek için açıklama. |
| --düğüm adı [Gerekli] | Düğümün adı. |
| --kuvvet | Zorlama yapılandırma ekleme belirtilen düğümler üzerinde geçersiz kılar. |
| --zaman ayarı -t | İşlemi saniyeler içinde gerçekleştirmek için sunucu zaman. Bu zaman sonu, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametrenin varsayılan değeri 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-node-disable"></a>sfctl düğümü devre dışı
Belirtilen devre dışı bırakma amacıyla Hizmet Kumaşı küme düğümünü devre dışı bırakın.

Belirtilen devre dışı bırakma amacıyla Hizmet Kumaşı küme düğümünü devre dışı bırakın. Devre dışı bırakma devam ettikten sonra devre dışı bırakma niyeti artırılabilir, ancak azaltılamaz (örneğin, Duraklatma amacıyla devre dışı bırakılan bir düğüm Yeniden Başlat ile daha da devre dışı bırakılabilir, ancak tam tersi değil. Düğümler devre dışı bırakıldıktan sonra herhangi bir zamanda bir düğüm işlemini etkinleştir kullanılarak yeniden etkinleştirilebilir. Devre dışı bırakma tamamlanmazsa, bu devre dışı bırakma iptal eder. Devre dışı bırakılırken inen ve geri gelen bir düğümün, hizmetler düğüme yerleştirilmeden önce yeniden etkinleştirilmesi gerekir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --düğüm adı [Gerekli] | Düğümün adı. |
| --devre dışı bırakma-niyet | Düğümü devre dışı etme niyetini veya nedenini açıklar. Olası değerler takip ediyor. |
| --zaman ayarı -t | İşlemi saniyeler içinde gerçekleştirmek için sunucu zaman. Bu zaman sonu, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametrenin varsayılan değeri 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-node-enable"></a>sfctl düğüm etkinleştirmek
Şu anda devre dışı bırakılan Hizmet Kumaşı küme düğümünü etkinleştirin.

Şu anda devre dışı bırakılan Hizmet Kumaşı küme düğümünü etkinleştirir. Etkinleştirildiğinde, düğüm yeni yinelemeler yerleştirmek için yeniden uygun bir hedef haline gelir ve düğümde kalan devre dışı bırakılan yinelemeler yeniden etkinleştirilir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
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

## <a name="sfctl-node-get-configuration-overrides"></a>sfctl düğüm get-configuration-overrides
Belirtilen düğümdeki yapılandırma geçersiz kılma listesini alır.

Bu api, belirtilen düğümdeki tüm varolan yapılandırma geçersiz kılmalarını sağlar.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
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

## <a name="sfctl-node-health"></a>sfctl düğüm sağlık
Service Fabric düğümünün sağlığını alır.

Service Fabric düğümünün sağlığını alır. Sistem durumu durumuna bağlı olarak düğümde bildirilen sistem durumu olaylarının toplanmasını filtrelemek için EventsHealthStateFilter'i kullanın. Ada göre belirttiğiniz düğüm sistem durumu deposunda yoksa, bu bir hata döndürür.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --düğüm adı [Gerekli] | Düğümün adı. |
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

## <a name="sfctl-node-info"></a>sfctl düğüm bilgi
Service Fabric kümesindeki belirli bir düğüm hakkındaki bilgileri alır.

Yanıt, ad, durum, kimlik, sistem durumu, çalışma süresi ve düğümle ilgili diğer ayrıntıları içerir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
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

## <a name="sfctl-node-list"></a>sfctl düğüm listesi
Service Fabric kümesindeki düğümlerin listesini alır.

Yanıt, ad, durum, kimlik, sistem durumu, çalışma süresi ve düğümlerle ilgili diğer ayrıntıları içerir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --devam-belirteç | Devamı belirteç parametresi sonraki sonuç kümesini elde etmek için kullanılır. Sistemden elde edilen sonuçlar tek bir yanıta sığmıyorsa, BOŞ olmayan bir değere sahip bir devam belirteci API'nın yanıtına dahil edilir. Bu değer bir sonraki API çağrısına geçirildiğinde, API sonraki sonuç kümesini döndürür. Başka sonuç yoksa, devam belirteci bir değer içermez. Bu parametrenin değeri URL kodlanmış olmamalıdır. |
| --max-sonuçlar | Sayfalı sorguların bir parçası olarak döndürülecek en fazla sonuç sayısı. Bu parametre, döndürülen sonuç sayısındaki üst sınırı tanımlar. Döndürülen sonuçlar, yapılandırmada tanımlanan maksimum ileti boyutu kısıtlamalarına göre iletiye sığmazsa, belirtilen maksimum sonuçlardan daha az olabilir. Bu parametre sıfır veya belirtilmemişse, sayfalı sorgu, iade iletisindeki sığan mümkün olduğunca çok sonuç içerir. |
| --düğüm durumu-filtresi | Düğüm Durumu'na göre düğümlerin filtrelemesine izin verir. Yalnızca belirtilen filtre değeriyle eşleşen düğümler döndürülür. Filtre değeri aşağıdakilerden biri olabilir.  Varsayılan\: varsayılan değer. |
| --zaman ayarı -t | İşlemi saniyeler içinde gerçekleştirmek için sunucu zaman. Bu zaman sonu, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametrenin varsayılan değeri 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-node-load"></a>sfctl düğüm yükü
Servis Kumaş düğümünün yük bilgilerini alır.

Tanımlanan yük veya kapasiteye sahip tüm ölçümler için Hizmet Kumaş düğümünün yük bilgilerini alır.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
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

## <a name="sfctl-node-remove-configuration-overrides"></a>sfctl düğüm kaldırma-yapılandırma-overrides
Belirtilen düğümdeki yapılandırma geçersiz kılmalarını kaldırır.

Bu api, belirtilen düğümdeki varolan tüm yapılandırma geçersiz kılmalarının kaldırılmasına olanak tanır.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
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

## <a name="sfctl-node-remove-state"></a>sfctl düğüm kaldırma durumu
Hizmet Kumaşı'na, bir düğümüzerinde kalıcı durum kalıcı olarak kaldırıldığını veya kaybolduğunu bildirer.

Bu, bu düğümün kalıcı durumunu kurtarmak mümkün olmadığı anlamına gelir. Bu genellikle bir sabit disk temiz silinmişse veya sabit disk çatlarsa gerçekleşir. Bu işlemin başarılı olaması için düğümün aşağı olması gerekir. Bu işlem, Service Fabric'e bu düğümdeki yinelemelerin artık var olmadığını ve Hizmet Kumaşı'nın bu yinelemelerin geri gelmesini beklemeyi bırakması gerektiğini bilmesini sağlar. Düğümüzerindeki durum kaldırılmadıysa ve düğüm sağlam bir şekilde geri gelebiliyorsa bu cmdlet'i çalıştırmayın. Service Fabric 6.5'ten başlayarak, bu API'yi tohum düğümleri için kullanmak için lütfen tohum düğümlerini normal (tohumsuz) düğümlere çevirin ve ardından düğüm durumunu kaldırmak için bu API'yi çağırın. Küme Azure'da çalışıyorsa, tohum düğümü düştükten sonra Service Fabric onu otomatik olarak tohum olmayan bir düğümolarak değiştirmeye çalışır. Bunu gerçekleştirmek için, birincil düğüm türündeki tohumsuz düğüm sayısının Aşağı tohum düğümlerinin sayısından az olmadığından emin olun. Gerekirse, bunu başarmak için birincil düğüm türüne daha fazla düğüm ekleyin. Bağımsız küme için, Down tohum düğümünün sağlam bir şekilde geri gelmesi beklenmiyorsa, lütfen düğümü\:kümeden çıkarın, https //docs.microsoft.com/en-us/azure/service-fabric/service-fabric-cluster-windows-server-add-remove-nodes bakın.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
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

## <a name="sfctl-node-report-health"></a>sfctl düğüm raporu-sağlık
Hizmet Kumaş düğümü hakkında bir sağlık raporu gönderir.

Belirtilen Hizmet Kumaş düğümünün sağlık durumunu bildirir. Rapor, sağlık raporunun kaynağı ve raporlandığı özellik hakkındaki bilgileri içermelidir. Rapor, sağlık deposuna ileten Bir Hizmet Kumaşı ağ geçidi düğümüne gönderilir. Rapor ağ geçidi tarafından kabul edilebilir, ancak ek doğrulama sonra sağlık deposu tarafından reddedilir. Örneğin, sistem durumu deposu, eski bir sıra numarası gibi geçersiz bir parametre nedeniyle raporu reddedebilir. Raporun sağlık deposunda uygulanıp uygulanmadığını görmek için raporun HealthEvents bölümünde görünüp görünmediğini denetleyin.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --sağlık-özellik [Gerekli] | Sağlık bilgilerinin özelliği. <br><br> Bir varlık, farklı özellikler için sistem durumu raporları olabilir. Özellik, muhabirin raporu tetikleyen durum durumunu kategorilere ayırmaesnekliğine izin vermek için sabit bir numaralandırma değil, bir dizedir. Örneğin, SourceId "LocalWatchdog" olan bir muhabir, kullanılabilir diskin bir düğümüzerindeki durumunu izleyebilir, böylece bu düğümdeki "AvailableDisk" özelliğini bildirebilir. Aynı muhabir düğüm bağlantısını izleyebilir, böylece aynı düğümüzerinde bir özellik "Bağlantı" bildirebilir. Sağlık deposunda, bu raporlar belirtilen düğüm için ayrı sağlık olayları olarak kabul edilir. Kaynak Kimliği ile birlikte, özellik benzersiz sağlık bilgilerini tanımlar. |
| --sağlık durumu [Gerekli] | Olası değerler\: arasında 'Geçersiz', 'Tamam', 'Uyarı', 'Hata', 'Bilinmiyor' sayılabilir. |
| --düğüm adı [Gerekli] | Düğümün adı. |
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

## <a name="sfctl-node-restart"></a>sfctl düğüm yeniden başlatma
Service Fabric küme düğümlerini yeniden başlatır.

Zaten başlamış olan Hizmet Kumaş ı sküme düğümlerini yeniden başlatır.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --düğüm adı [Gerekli] | Düğümün adı. |
| --create-fabric-dump | Kumaş düğümü işleminin dökümesini oluşturmak için True'yu belirtin. Bu büyük/küçük harf duyarlı.  Varsayılan\: False. |
| --düğüm-instance-id | Hedef düğümün örnek kimliği. Örnek kimlik belirtilirse, düğüm yalnızca düğümün geçerli örneğiyle eşleşirse yeniden başlatılır. Varsayılan "0" değeri herhangi bir örnek kimliğiyle eşleşir. Örnek kimlik get node sorgusu kullanılarak elde edilebilir.  Varsayılan\: 0. |
| --zaman ayarı -t | İşlemi saniyeler içinde gerçekleştirmek için sunucu zaman. Bu zaman sonu, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametrenin varsayılan değeri 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-node-transition"></a>sfctl düğüm geçişi
Küme düğümlerini başlatır veya durdurur.

Küme düğümlerini başlatır veya durdurur.  Küme düğümü, işletim sistemi örneğinin kendisi değil, bir işlemdir.  Düğüm başlatmak için, NodeTransitionType parametresi için "Başlat" geçişini geçirin. Düğümü durdurmak için, NodeTransitionType parametresi için "Dur" geçişini. Bu API işlemi başlatır - API düğümü döndürdüğünde geçiş işlemi henüz tamamlanamış olabilir. Operasyonun ilerlemesini almak için getnodeTransitionProgress'i aynı OperationId ile arayın.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --düğüm-instance-id [Gerekli] | Hedef düğümün düğüm örnek kimliği. Bu GetNodeInfo API ile belirlenebilir. |
| --düğüm adı [Gerekli] | Düğümün adı. |
| --düğüm geçiş türü [Gerekli] | Gerçekleştirecek geçiş türünü gösterir.  NodeTransitionType.Start durdurulan bir düğüm başlatacak. NodeTransitionType.Stop kadar bir düğüm durdurur. |
| --operation-id [Gerekli] | Bu API'nin çağrısını tanımlayan bir GUID.  Bu, ilgili GetProgress API'sine aktarılır. |
| --stop-duration-in-seconds [Gerekli] | Düğümün durdurulması için saniyeler içinde süre.  Minimum değer 600, en büyük değer 14400'dür.  Bu süre dolduktan sonra düğüm otomatik olarak geri gelir. |
| --zaman ayarı -t | İşlemi saniyeler içinde gerçekleştirmek için sunucu zaman. Bu zaman sonu, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametrenin varsayılan değeri 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-node-transition-status"></a>sfctl düğüm geçiş durumu
StartNodeTransition kullanarak başlatılan bir işlemin ilerlemesini alır.

Sağlanan OperationId'i kullanarak StartNodeTransition ile başlatılan bir işlemin ilerlemesini alır.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --düğüm adı [Gerekli] | Düğümün adı. |
| --operation-id [Gerekli] | Bu API'nin çağrısını tanımlayan bir GUID.  Bu, ilgili GetProgress API'sine aktarılır. |
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