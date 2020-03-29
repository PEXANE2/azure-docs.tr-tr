---
title: Azure Servis Kumaş CLI- sfctl kümesi
description: Azure Service Fabric komut satırı arabirimi sfctl hakkında bilgi edinin. Kümeleri yönetmek için komutların listesini içerir.
author: jeffj6123
ms.topic: reference
ms.date: 1/16/2020
ms.author: jejarry
ms.openlocfilehash: 007ad6f59f0ce304db579f4faa1bb95611a93a37
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76906143"
---
# <a name="sfctl-cluster"></a>sfctl cluster
Service Fabric kümelerini seçin, yönetin ve çalıştırın.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| kod sürümleri | Service Fabric kümesinde sağlanan kumaş kodu sürümlerinin listesini alır. |
| config sürümleri | Service Fabric kümesinde bulunan kumaş config sürümlerinin listesini alır. |
| Sağlık | Service Fabric kümesinin durumunu alır. |
| bildirim | Service Fabric küme bildirimini alın. |
| işlem iptal | Kullanıcı kaynaklı bir hata işlemini iptal eder. |
| operasyon listesi | Sağlanan girdi tarafından filtre uygulanmış kullanıcı kaynaklı hata işlemlerinin listesini alır. |
| Sağlanması | Hizmet Kumaşı kümesinin kod veya yapılandırma paketlerini sağlama. |
| kurtarma sistemi | Hizmet Kumaşı kümesine, şu anda çoğunluk kaybına bağlı olan sistem hizmetlerini kurtarmaya çalışması gerektiğini gösterir. |
| rapor-sağlık | Hizmet Kumaşı kümesi hakkında bir sistem durumu raporu gönderir. |
| Seçin | Service Fabric kümesi bitiş noktasına bağlanır. |
| show-bağlantı | Bu sfctl örneğinin bağlı olduğu Hizmet Kumaşı kümesini gösterin. |
| hükümsüz | Hizmet Kumaşı kümesinin kod veya yapılandırma paketlerini sağlama. |
| yükseltme | Service Fabric kümesinin kod veya yapılandırma sürümünü yükseltmeye başlayın. |
| yükseltme-özgeçmiş | Küme yükseltmesinin bir sonraki yükseltme etki alanına taşınmasını sağla. |
| yükseltme-geri alma | Service Fabric kümesinin yükseltmesini geri getirin. |
| yükseltme durumu | Geçerli küme yükseltmesinin ilerlemesini alır. |
| yükseltme-güncelleme | Service Fabric küme yükseltmesinin yükseltme parametrelerini güncelleştirin. |

## <a name="sfctl-cluster-code-versions"></a>sfctl küme kod sürümleri
Service Fabric kümesinde sağlanan kumaş kodu sürümlerinin listesini alır.

Kümede sağlanan kumaş kodu sürümleri yle ilgili bilgilerin listesini alır. Parametre CodeVersion isteğe bağlı olarak yalnızca belirli bir sürüme çıktı filtrelemek için kullanılabilir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --kod sürümü | Service Fabric ürün sürümü. |
| --zaman ayarı -t | İşlemi saniyeler içinde gerçekleştirmek için sunucu zaman. Bu zaman sonu, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametrenin varsayılan değeri 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-cluster-config-versions"></a>sfctl küme config-versiyonları
Service Fabric kümesinde bulunan kumaş config sürümlerinin listesini alır.

Kümede bulunan kumaş config sürümleri hakkında bir bilgi listesi alır. ConfigVersion parametresi, çıktıyı yalnızca belirli bir sürüme isteğe bağlı olarak filtrelemek için kullanılabilir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --config-version | Service Fabric config sürümü. |
| --zaman ayarı -t | İşlemi saniyeler içinde gerçekleştirmek için sunucu zaman. Bu zaman sonu, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametrenin varsayılan değeri 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-cluster-health"></a>sfctl küme sağlık
Service Fabric kümesinin durumunu alır.

Sistem durumu durumuna göre kümede bildirilen sistem durumu olaylarının toplanmasını filtrelemek için EventsHealthStateFilter'i kullanın. Benzer şekilde, düğümlerin ve uygulamaların toplanmış sistem durumu temel alınarak döndürülen düğümlerin ve uygulamaların toplanmasını filtrelemek için DüğümlerHealthStateFilter ve ApplicationsHealthStateFilter'i kullanın.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --uygulamalar-sağlık-durum-filtre | Sistem durumu durumuna göre küme durumu sorgusu sonucunda döndürülen uygulama durumu durumu nesnelerinin filtrelemesine izin verir. Bu parametre için olası değerler, üyelerden elde edilen tamsayı değerini veya HealthStateFilter numaralandırma üyelerinezdinde bitwise işlemleri içerir. Yalnızca filtreyle eşleşen uygulamalar döndürülür. Tüm uygulamalar, toplu sağlık durumunu değerlendirmek için kullanılır. Belirtilmemişse, tüm girişler döndürülür. Durum değerleri bayrak tabanlı numaralandırmadır, bu nedenle değer bitwise 'VEYA' işleci kullanılarak elde edilen bu değerlerin bir birleşimi olabilir. Örneğin, sağlanan değer 6 ise, Ok (2) ve Uyarı (4) HealthState değerine sahip uygulamaların sistem durumu döndürülür.  <br> - Varsayılan - Varsayılan değer. Herhangi bir HealthState ile eşleşir. Değer sıfırdır.  <br> - Yok - Herhangi bir HealthState değeriyle eşleşmeyen filtre. Belirli bir durum koleksiyonunda sonuç vermek için kullanılır. Değeri 1'dir.  <br> - Tamam - HealthState değeri Ok ile giriş eşleşen filtre. Değeri 2'dir.  <br> - Uyarı - HealthState değer Uyarısı ile girdi eşleşen filtre. Değeri 4'dür.  <br> - Hata - HealthState değer Hatası ile giriş eşleşen filtre. Değeri 8'dir.  <br> - Tümü - Herhangi bir HealthState değeriyle girişle eşleşen filtre. Değeri 65535' tir. |
| --olaylar-sağlık-durum-filtre | HealthEvent nesnelerinin toplanmasının sistem durumu durumuna göre filtrelemesine olanak tanır. Bu parametre için olası değerler, aşağıdaki sistem durumu durumlarından birinin tamsayı değerini içerir. Yalnızca filtreyle eşleşen olaylar döndürülür. Tüm olaylar toplu sağlık durumunu değerlendirmek için kullanılır. Belirtilmemişse, tüm girişler döndürülür. Durum değerleri bayrak tabanlı numaralandırmadır, bu nedenle değer bitwise 'VEYA' işleci kullanılarak elde edilen bu değerlerin bir birleşimi olabilir. Örneğin, sağlanan değer 6 ise, Ok (2) ve Warning (4) HealthState değerine sahip tüm olaylar döndürülür.  <br> - Varsayılan - Varsayılan değer. Herhangi bir HealthState ile eşleşir. Değer sıfırdır.  <br> - Yok - Herhangi bir HealthState değeriyle eşleşmeyen filtre. Belirli bir durum koleksiyonunda sonuç vermek için kullanılır. Değeri 1'dir.  <br> - Tamam - HealthState değeri Ok ile giriş eşleşen filtre. Değeri 2'dir.  <br> - Uyarı - HealthState değer Uyarısı ile girdi eşleşen filtre. Değeri 4'dür.  <br> - Hata - HealthState değer Hatası ile giriş eşleşen filtre. Değeri 8'dir.  <br> - Tümü - Herhangi bir HealthState değeriyle girişle eşleşen filtre. Değeri 65535' tir. |
| --dışlama-sağlık-istatistik | Sistem durumu istatistiklerinin sorgu sonucunun bir parçası olarak döndürülüp döndürülmemesi gerektiğini gösterir. Varsayılan olarak false. İstatistikler, ok, uyarı ve hata durumu sağlık durumundaki çocuk varlıkların sayısını gösterir. |
| --dahil-sistem-uygulama-sağlık-istatistik | Sistem istatistiklerinin kumaş\:/Sistem uygulaması sistem sistem istatistiklerini içerip içermemesi gerektiğini gösterir. Varsayılan olarak false. IncludeSystemApplicationHealthStatistics doğru ayarlanmışsa, sistem istatistikleri kumaş\:/ Sistem uygulamasına ait varlıkları içerir. Aksi takdirde, sorgu sonucu yalnızca kullanıcı uygulamaları için sistem durumu istatistiklerini içerir. Bu parametrenin uygulanabilmesi için sistem durumu istatistikleri sorgu sonucuna dahil edilmelidir. |
| --düğümler-sağlık-durum-filtre | Küme durumu sorgusu nun sonucu olarak döndürülen düğüm durumu durumu nesnelerinin sistem durumu durumuna göre filtrelemesine izin verir. Bu parametre için olası değerler, aşağıdaki sistem durumu durumlarından birinin tamsayı değerini içerir. Yalnızca filtreyle eşleşen düğümler döndürülür. Tüm düğümler, birleştirilmiş sistem durumunu değerlendirmek için kullanılır. Belirtilmemişse, tüm girişler döndürülür. Durum değerleri bayrak tabanlı numaralandırmadır, bu nedenle değer bitwise 'VEYA' işleci kullanılarak elde edilen bu değerlerin bir birleşimi olabilir. Örneğin, sağlanan değer 6 ise, Ok (2) ve Warning (4) HealthState değerine sahip düğümlerin sistem durumu döndürülür.  <br> - Varsayılan - Varsayılan değer. Herhangi bir HealthState ile eşleşir. Değer sıfırdır.  <br> - Yok - Herhangi bir HealthState değeriyle eşleşmeyen filtre. Belirli bir durum koleksiyonunda sonuç vermek için kullanılır. Değeri 1'dir.  <br> - Tamam - HealthState değeri Ok ile giriş eşleşen filtre. Değeri 2'dir.  <br> - Uyarı - HealthState değer Uyarısı ile girdi eşleşen filtre. Değeri 4'dür.  <br> - Hata - HealthState değer Hatası ile giriş eşleşen filtre. Değeri 8'dir.  <br> - Tümü - Herhangi bir HealthState değeriyle girişle eşleşen filtre. Değeri 65535' tir. |
| --zaman ayarı -t | İşlemi saniyeler içinde gerçekleştirmek için sunucu zaman. Bu zaman sonu, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametrenin varsayılan değeri 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-cluster-manifest"></a>sfctl küme manifestosu
Service Fabric küme bildirimini alın.

Service Fabric küme bildirimini alın. Küme bildirimi küme, güvenlik yapılandırmaları, hata ve yükseltme etki alanı topolojileri, vb farklı düğüm türleri içeren küme özelliklerini içerir. Bu özellikler, tek başına bir küme dağıtılırken ClusterConfig.JSON dosyasının bir parçası olarak belirtilir. Ancak, küme bildirimindeki bilgilerin çoğu, diğer dağıtım senaryolarında (örneğin Azure portalı kullanırken) küme dağıtımı sırasında hizmet dokusu tarafından dahili olarak oluşturulur. Küme manifestosunun içeriği yalnızca bilgilendirme amaçlıdır ve kullanıcıların dosya içeriğinin biçimine veya yorumlanmasına bağımlı olması beklenmez.

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

## <a name="sfctl-cluster-operation-cancel"></a>sfctl küme işlemi-iptal
Kullanıcı kaynaklı bir hata işlemini iptal eder.

Aşağıdaki API'ler CancelOperation\: StartDataLoss, StartQuorumLoss, StartPartitionRestart, StartNodeTransition kullanılarak iptal edilebilecek hata işlemlerini başlatır. Kuvvet yanlışsa, kullanıcı tarafından başlatılan işlem incelikle durdurulur ve temizlenir.  Eğer kuvvet doğruysa, komut iptal edilir ve bazı iç durum geride bırakılabilir.  Kuvvetin doğru olarak belirtilmesi özenle kullanılmalıdır. Bu API'yi doğru ayarlanmış bir güçle çağırmak, bu API'nin önce yanlış olarak ayarlanmış kuvvetle aynı test komutuna çağrılmasına veya test komutunun zaten OperationState.RollingBack'e ait bir OperationState.The OperationState'e sahip olmadığı sürece izin verilmez. Açıklama\: OperationState.RollingBack sistem olacak / komut yürütme kaynaklanan iç sistem durumu temizliyor anlamına gelir.  Test komutu veri kaybına neden olacaksa verileri geri yüklemez.  Örneğin, StartDataLoss'u arayarak bu API'yi ararsanız, sistem yalnızca iç durumu komutu çalıştırarak temizler. Komut veri kaybına neden olacak kadar ilerlediyse, hedef bölümün verilerini geri yüklemez. Bu\: API kuvvetle çağrılması durumunda önemli not==doğru, iç durum geride bırakılabilir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --operation-id [Gerekli] | Bu API'nin çağrısını tanımlayan bir GUID.  Bu, ilgili GetProgress API'sine aktarılır. |
| --kuvvet | Kullanıcı kaynaklı işlemi yürüterek değiştirilen dahili sistem durumunu zarif bir şekilde geri alıp temizlemeyeceklerini gösterir. |
| --zaman ayarı -t | İşlemi saniyeler içinde gerçekleştirmek için sunucu zaman. Bu zaman sonu, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametrenin varsayılan değeri 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-cluster-operation-list"></a>sfctl küme işlem listesi
Sağlanan girdi tarafından filtre uygulanmış kullanıcı kaynaklı hata işlemlerinin listesini alır.

Sağlanan girdi tarafından filtre uygulanarak kullanıcı kaynaklı hata işlemlerinin listesini alır.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --durum filtresi | Kullanıcı kaynaklı işlemler için OperationState'infiltresini filtrelemek için kullanılır. - 65535 - Select All - 1 - Select Running - 2 - Select RollingBack - 8 - select Completed - 16 - select Faulted - 32 - Select Canceled - 64 - forcecancelled'i seçin.  Varsayılan\: 65535. |
| --zaman ayarı -t | İşlemi saniyeler içinde gerçekleştirmek için sunucu zaman. Bu zaman sonu, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametrenin varsayılan değeri 60 saniyedir.  Varsayılan\: 60. |
| --tip filtresi | Kullanıcı kaynaklı işlemler için OperationType'ı filtrelemek için kullanılır. - 65535 - tümünü seçin - 1 - PartitionDataLoss'u seçin. - 2 - PartitionQuorumLoss seçin. - 4 - PartitionRestart'ı seçin. - 8 - Düğüm Geçişi'ni seçin.  Varsayılan\: 65535. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-cluster-provision"></a>sfctl küme hükmü
Hizmet Kumaşı kümesinin kod veya yapılandırma paketlerini sağlama.

Hizmet Kumaşı kümesinin kod veya yapılandırma paketlerini doğrulayın ve sağlayın.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --küme-manifest-dosya-yol | Küme bildirimi dosya yolu. |
| --kod-dosya-yol | Küme kodu paketi dosya yolu. |
| --zaman ayarı -t | İşlemi saniyeler içinde gerçekleştirmek için sunucu zaman. Bu zaman sonu, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametrenin varsayılan değeri 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-cluster-recover-system"></a>sfctl küme kurtarma sistemi
Hizmet Kumaşı kümesine, şu anda çoğunluk kaybına bağlı olan sistem hizmetlerini kurtarmaya çalışması gerektiğini gösterir.

Hizmet Kumaşı kümesine, şu anda çoğunluk kaybına bağlı olan sistem hizmetlerini kurtarmaya çalışması gerektiğini gösterir. Bu işlem yalnızca aşağı olan yinelemelerin kurtarılamayacağı biliniyorsa gerçekleştirilmelidir. Bu API'nin yanlış kullanımı olası veri kaybına neden olabilir.

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

## <a name="sfctl-cluster-report-health"></a>sfctl küme rapor-sağlık
Hizmet Kumaşı kümesi hakkında bir sistem durumu raporu gönderir.

Hizmet Kumaşı kümesi nde bir sistem durumu raporu gönderir. Rapor, sağlık raporunun kaynağı ve raporlandığı özellik hakkındaki bilgileri içermelidir. Rapor, sağlık deposuna ileten Bir Hizmet Kumaşı ağ geçidi düğümüne gönderilir. Rapor ağ geçidi tarafından kabul edilebilir, ancak ek doğrulama sonra sağlık deposu tarafından reddedilir. Örneğin, sistem durumu deposu, eski bir sıra numarası gibi geçersiz bir parametre nedeniyle raporu reddedebilir. Raporun sistem durumu deposunda uygulanıp uygulanmadığını görmek için GetClusterHealth'i çalıştırın ve raporun HealthEvents bölümünde görünüp görünmediğini denetleyin.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
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

## <a name="sfctl-cluster-select"></a>sfctl küme seçin
Service Fabric kümesi bitiş noktasına bağlanır.

Güvenli kümeye bağlanıyorsanız, bir cert (.crt) ve anahtar dosyasına (.key) veya her ikisine (.pem) sahip tek bir dosyaya mutlak bir yol belirtin. Her ikisini de belirtmeyin. İsteğe bağlı olarak, güvenli bir kümeye bağlanıyorsanız, ca paketi dosyasına veya güvenilir CA sertifika dizinine mutlak bir yol da belirtin.  Bu komutu önce çalıştırmadan bir kümeyle bağlantı yoktur, localhost bağlantısı da dahil. Ancak, yerel bir kümeye bağlanmak için açık bir uç nokta gerekmez.  Kendi imzalı bir sertifika veya iyi bilinen bir CA tarafından imzalanmayan başka bir sertifika kullanıyorsanız, doğrulamanın geçtiğinden emin olmak için --ca parametresini geçirin. Bir üretim kümesinde değilse, istemci tarafı doğrulamasını atlamak için (kendi imzası olan veya iyi bilinen CA imzalı için kullanışlı), --doğrulama seçeneğini kullanın. Mümkün olsa da, üretim kümeleri için önerilmez. Bir sertifika doğrulama hatası aksi neden olabilir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --aad | Kimlik doğrulaması için Azure Etkin Dizini'ni kullanın. |
| --ca | Geçerli veya CA paket dosyası olarak ele almak için CA sertifikadizini için mutlak yol. OpenSSL tarafından sağlanan CA `c_rehash <directory>` sertifikalarının bir dizinini kullanıyorsanız, önce sertifika kalıplarını hesaplamak ve uygun sembolik bağlantılar oluşturmak için çalıştırılmalıdır. Bu, küme tarafından döndürülen sertifikanın geçerli olduğunu doğrulamak için kullanılır. |
| --cert | İstemci sertifikası dosyasına giden mutlak yol. |
| --uç nokta | Bağlantı noktası ve HTTP veya HTTPS öneki de dahil olmak üzere uç nokta URL'yi kümele. Genellikle, bitiş noktası https\://<url'niz 19080>\:gibi görünür. Bitiş noktası verilmezse, varsayılan olarak\:http //localhost\:19080 olur.  Varsayılan\: \:http //localhost\:19080. |
| --anahtar | İstemci sertifikası anahtar dosyasına mutlak yol. |
| --doğrulama | HTTPS kullanırken sertifikalar için doğrulamayı\: devre dışı, bunun güvenli olmayan bir seçenek olduğunu ve üretim ortamları için kullanılmaması gerektiğini unutmayın. |
| --pem | .pem dosyası olarak istemci sertifikasına giden mutlak yol. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-cluster-show-connection"></a>sfctl küme show-bağlantısı
Bu sfctl örneğinin bağlı olduğu Hizmet Kumaşı kümesini gösterin.

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-cluster-unprovision"></a>sfctl küme unprovision
Hizmet Kumaşı kümesinin kod veya yapılandırma paketlerini sağlama.

Ayrı ayrı hükümsüz kod ve yapılandırma için desteklenir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --kod sürümü | Küme kodu paket sürümü. |
| --config-version | Küme manifesto sürümü. |
| --zaman ayarı -t | İşlemi saniyeler içinde gerçekleştirmek için sunucu zaman. Bu zaman sonu, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametrenin varsayılan değeri 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-cluster-upgrade"></a>sfctl küme yükseltme
Service Fabric kümesinin kod veya yapılandırma sürümünü yükseltmeye başlayın.

Sağlanan yükseltme parametrelerini doğrulayın ve parametreler geçerliyse Service Fabric kümesinin kod veya yapılandırma sürümünü yükseltmeye başlayın.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --app-health-map | JSON, hata yükseltmeden önce uygulama adı ve maksimum yüzde sağlıksız çiftleri sözlüğü kodladı. |
| --uygulama türü-sağlık-harita | JSON, hata yükseltmeden önce uygulama türü adı ve maksimum yüzde sağlıksız çiftleri sözlüğü kodlanır. |
| --kod sürümü | Küme kodu sürümü. |
| --config-version | Küme yapılandırma sürümü. |
| --delta-sağlık-değerlendirme | Her yükseltme etki alanı tamamlandıktan sonra mutlak sağlık değerlendirmesi yerine delta sağlık değerlendirmesi sağlar. |
| --delta-sağlıksız düğümler | Küme yükseltmeleri sırasında izin verilen düğümlerin sistem durumu bozulmasının izin verilen maksimum yüzdesi.  Varsayılan\: 10. <br><br> Delta yükseltme başında düğümlerin durumu ve sağlık değerlendirmesi sırasında düğümlerin durumu arasında ölçülür. Denetim, kümenin genel durumunun tolere edilen sınırlar içinde olduğundan emin olmak için her yükseltme etki alanı yükseltmesi tamamlandıktan sonra gerçekleştirilir. |
| --hata-eylem | Olası değerler\: arasında 'Geçersiz', 'Geri Alma', 'El Kitabı' sayılabilir. |
| --kuvvet yeniden başlatma | Kod sürümü değişmemiş olsa bile, işlemler yükseltme sırasında zorla yeniden başlatılır. <br><br> Yükseltme yalnızca yapılandırmayı veya verileri değiştirir. |
| --sağlık-kontrol-yeniden deneme | Uygulama veya küme sağlıklı değilse, sistem durumu denetimleri gerçekleştirme girişimleri arasındaki süre. |
| --sağlık-kontrol-kararlı | Yükseltme bir sonraki yükseltme etki alanına geçmeden önce uygulamanın veya kümenin sağlıklı kalması gereken süre. <br><br> İlk olarak ISO 8601 süresini temsil eden bir dize olarak yorumlanır. Bu başarısız olursa, o zaman milisaniye toplam sayısını temsil eden bir sayı olarak yorumlanır. |
| --sağlık-check-bekle | Sistem durumu denetimleri işlemini başlatmadan önce bir yükseltme etki alanını tamamladıktan sonra bekleme süresi. |
| --çoğaltma-ayar-çek-zaman dilimi | Yükseltme etki alanının işlenmesini engellemek ve beklenmeyen sorunlar olduğunda kullanılabilirlik kaybını önlemek için en yüksek süre. <br><br> Bu zaman aşımı sona erdiğinde, kullanılabilirlik kaybı sorunları ne olursa olsun yükseltme etki alanının işlenmesi devam eder. Zaman anına sıfırlama, her yükseltme etki alanının başında sıfırlanır. Geçerli değerler 0 ile 42949672925 arasındadır. |
| --yuvarlanma-yükseltme modu | Olası değerler\: arasında 'Geçersiz', 'İzlenmeyen Otomatik', 'İzlenmeyen Kılavuz', 'İzlenen'.  Varsayılan\: UnmonitoredAuto. |
| --zaman ayarı -t | Varsayılan\: 60. |
| --sağlıksız uygulamalar | Bir hatayı bildirmeden önce izin verilen en yüksek sağlıksız uygulamaların yüzdesi. <br><br> Örneğin, uygulamaların %10'unun sağlıksız olmasını sağlamak için bu değer 10 olacaktır. Bu yüzde, küme hata olarak kabul edilmeden önce sağlıksız olabilecek uygulamaların en yüksek tolere edilen yüzdesini temsil eder. Yüzdeye saygı duyulur ancak en az bir sağlıksız uygulama varsa, sistem durumu Uyarı olarak değerlendirilir. Bu, ApplicationTypeHealthPolicyMap'e dahil edilen uygulama türlerinin uygulamaları hariç olmak üzere, sağlıksız uygulama sayısının kümedeki toplam uygulama örneği sayısına bölünmesiyle hesaplanır. Hesaplama, az sayıda uygulamada bir başarısızlığa tolerans göstermek için yuvarlar. |
| --sağlıksız düğümler | Bir hatayı bildirmeden önce izin verilen en yüksek sağlıksız düğüm yüzdesi. <br><br> Örneğin, düğümlerin %10'unun sağlıksız olmasını sağlamak için bu değer 10 olacaktır. Yüzde, küme hata olarak kabul edilmeden önce sağlıksız olabilecek düğümlerin en yüksek tolere edilen yüzdesini temsil eder. Yüzdeye saygı duyulur ancak en az bir sağlıksız düğüm varsa, sistem durumu Uyarı olarak değerlendirilir. Yüzde, kümedeki toplam düğüm sayısına sağlıksız düğüm sayısı bölünerek hesaplanır. Hesaplama, küçük sayıda düğümdeki bir başarısızlığa tolerans göstermek için yuvarlar. Büyük kümelerde, bazı düğümler her zaman aşağı veya onarım için dışarı olacak, bu nedenle bu yüzde bunu tolere etmek için yapılandırılmalıdır. |
| --upgrade-etki alanı-delta-sağlıksız düğümler | Küme yükseltmeleri sırasında izin verilen yükseltme etki alanı düğümlerinin en fazla izin verilen yüzdesi.  Varsayılan\: 15. <br><br> Delta, yükseltmenin başındaki yükseltme etki alanı düğümlerinin durumu ile sistem durumu değerlendirmesi sırasında yükseltme etki alanı düğümlerinin durumu arasında ölçülür. Denetim, yükseltme etki alanlarının durumunun tolere edilen sınırlar içinde olduğundan emin olmak için tamamlanan tüm yükseltme etki alanları için her yükseltme etki alanı yükseltmesi tamamlandıktan sonra gerçekleştirilir. |
| --upgrade-etki alanı-zaman ayarı | FailureAction yürütülmeden önce her yükseltme etki alanının tamamlanması gereken süre. <br><br> İlk olarak ISO 8601 süresini temsil eden bir dize olarak yorumlanır. Bu başarısız olursa, o zaman milisaniye toplam sayısını temsil eden bir sayı olarak yorumlanır. |
| --yükseltme-zaman ayarı | FailureAction yürütülmeden önce genel yükseltmenin tamamlanması gereken süre. <br><br> İlk olarak ISO 8601 süresini temsil eden bir dize olarak yorumlanır. Bu başarısız olursa, o zaman milisaniye toplam sayısını temsil eden bir sayı olarak yorumlanır. |
| --uyarı-as-hata | Uyarıların hatalarla aynı önemle davranılıp işitilen olmadığını gösterir. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-cluster-upgrade-resume"></a>sfctl küme yükseltme-özgeçmiş
Küme yükseltmesinin bir sonraki yükseltme etki alanına taşınmasını sağla.

Küme kodunu veya yapılandırma yükseltmesini uygunsa bir sonraki yükseltme etki alanına taşıyın.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --upgrade-etki alanı [Gerekli] | Bu küme yükseltmesi için bir sonraki yükseltme etki alanı. |
| --zaman ayarı -t | İşlemi saniyeler içinde gerçekleştirmek için sunucu zaman. Bu zaman sonu, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametrenin varsayılan değeri 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-cluster-upgrade-rollback"></a>sfctl küme yükseltme-rollback
Service Fabric kümesinin yükseltmesini geri getirin.

Service Fabric kümesinin kodu veya yapılandırma yükseltmesini geri al.

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

## <a name="sfctl-cluster-upgrade-status"></a>sfctl küme yükseltme durumu
Geçerli küme yükseltmesinin ilerlemesini alır.

Devam eden küme yükseltmesinin geçerli ilerlemesini alır. Şu anda yükseltme devam ediyorsa, önceki küme yükseltmesinin son durumunu alın.

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

## <a name="sfctl-cluster-upgrade-update"></a>sfctl küme yükseltme-güncelleme
Service Fabric küme yükseltmesinin yükseltme parametrelerini güncelleştirin.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --app-health-map | JSON, hata yükseltmeden önce uygulama adı ve maksimum yüzde sağlıksız çiftleri sözlüğü kodladı. |
| --uygulama türü-sağlık-harita | JSON, hata yükseltmeden önce uygulama türü adı ve maksimum yüzde sağlıksız çiftleri sözlüğü kodlanır. |
| --delta-sağlık-değerlendirme | Her yükseltme etki alanı tamamlandıktan sonra mutlak sağlık değerlendirmesi yerine delta sağlık değerlendirmesi sağlar. |
| --delta-sağlıksız düğümler | Küme yükseltmeleri sırasında izin verilen düğümlerin sistem durumu bozulmasının izin verilen maksimum yüzdesi.  Varsayılan\: 10. <br><br> Delta yükseltme başında düğümlerin durumu ve sağlık değerlendirmesi sırasında düğümlerin durumu arasında ölçülür. Denetim, kümenin genel durumunun tolere edilen sınırlar içinde olduğundan emin olmak için her yükseltme etki alanı yükseltmesi tamamlandıktan sonra gerçekleştirilir. |
| --hata-eylem | Olası değerler\: arasında 'Geçersiz', 'Geri Alma', 'El Kitabı' sayılabilir. |
| --kuvvet yeniden başlatma | Kod sürümü değişmemiş olsa bile, işlemler yükseltme sırasında zorla yeniden başlatılır. <br><br> Yükseltme yalnızca yapılandırmayı veya verileri değiştirir. |
| --sağlık-kontrol-yeniden deneme | Uygulama veya küme sağlıklı değilse, sistem durumu denetimleri gerçekleştirme girişimleri arasındaki süre. |
| --sağlık-kontrol-kararlı | Yükseltme bir sonraki yükseltme etki alanına geçmeden önce uygulamanın veya kümenin sağlıklı kalması gereken süre. <br><br> İlk olarak ISO 8601 süresini temsil eden bir dize olarak yorumlanır. Bu başarısız olursa, o zaman milisaniye toplam sayısını temsil eden bir sayı olarak yorumlanır. |
| --sağlık-check-bekle | Sistem durumu denetimleri işlemini başlatmadan önce bir yükseltme etki alanını tamamladıktan sonra bekleme süresi. |
| --çoğaltma-ayar-çek-zaman dilimi | Yükseltme etki alanının işlenmesini engellemek ve beklenmeyen sorunlar olduğunda kullanılabilirlik kaybını önlemek için en yüksek süre. <br><br> Bu zaman aşımı sona erdiğinde, kullanılabilirlik kaybı sorunları ne olursa olsun yükseltme etki alanının işlenmesi devam eder. Zaman anına sıfırlama, her yükseltme etki alanının başında sıfırlanır. Geçerli değerler 0 ile 42949672925 arasındadır. |
| --yuvarlanma-yükseltme modu | Olası değerler\: arasında 'Geçersiz', 'İzlenmeyen Otomatik', 'İzlenmeyen Kılavuz', 'İzlenen'.  Varsayılan\: UnmonitoredAuto. |
| --zaman ayarı -t | Varsayılan\: 60. |
| --sağlıksız uygulamalar | Bir hatayı bildirmeden önce izin verilen en yüksek sağlıksız uygulamaların yüzdesi. <br><br> Örneğin, uygulamaların %10'unun sağlıksız olmasını sağlamak için bu değer 10 olacaktır. Bu yüzde, küme hata olarak kabul edilmeden önce sağlıksız olabilecek uygulamaların en yüksek tolere edilen yüzdesini temsil eder. Yüzdeye saygı duyulur ancak en az bir sağlıksız uygulama varsa, sistem durumu Uyarı olarak değerlendirilir. Bu, ApplicationTypeHealthPolicyMap'e dahil edilen uygulama türlerinin uygulamaları hariç olmak üzere, sağlıksız uygulama sayısının kümedeki toplam uygulama örneği sayısına bölünmesiyle hesaplanır. Hesaplama, az sayıda uygulamada bir başarısızlığa tolerans göstermek için yuvarlar. |
| --sağlıksız düğümler | Bir hatayı bildirmeden önce izin verilen en yüksek sağlıksız düğüm yüzdesi. <br><br> Örneğin, düğümlerin %10'unun sağlıksız olmasını sağlamak için bu değer 10 olacaktır. Yüzde, küme hata olarak kabul edilmeden önce sağlıksız olabilecek düğümlerin en yüksek tolere edilen yüzdesini temsil eder. Yüzdeye saygı duyulur ancak en az bir sağlıksız düğüm varsa, sistem durumu Uyarı olarak değerlendirilir. Yüzde, kümedeki toplam düğüm sayısına sağlıksız düğüm sayısı bölünerek hesaplanır. Hesaplama, küçük sayıda düğümdeki bir başarısızlığa tolerans göstermek için yuvarlar. Büyük kümelerde, bazı düğümler her zaman aşağı veya onarım için dışarı olacak, bu nedenle bu yüzde bunu tolere etmek için yapılandırılmalıdır. |
| --upgrade-etki alanı-delta-sağlıksız düğümler | Küme yükseltmeleri sırasında izin verilen yükseltme etki alanı düğümlerinin en fazla izin verilen yüzdesi.  Varsayılan\: 15. <br><br> Delta, yükseltmenin başındaki yükseltme etki alanı düğümlerinin durumu ile sistem durumu değerlendirmesi sırasında yükseltme etki alanı düğümlerinin durumu arasında ölçülür. Denetim, yükseltme etki alanlarının durumunun tolere edilen sınırlar içinde olduğundan emin olmak için tamamlanan tüm yükseltme etki alanları için her yükseltme etki alanı yükseltmesi tamamlandıktan sonra gerçekleştirilir. |
| --upgrade-etki alanı-zaman ayarı | FailureAction yürütülmeden önce her yükseltme etki alanının tamamlanması gereken süre. <br><br> İlk olarak ISO 8601 süresini temsil eden bir dize olarak yorumlanır. Bu başarısız olursa, o zaman milisaniye toplam sayısını temsil eden bir sayı olarak yorumlanır. |
| --yükseltme türü | Olası değerler\: arasında 'Geçersiz', 'Yuvarlanma', 'Rolling_ForceRestart' sayılabilir.  Varsayılan\: Yuvarlama. |
| --yükseltme-zaman ayarı | FailureAction yürütülmeden önce genel yükseltmenin tamamlanması gereken süre. <br><br> İlk olarak ISO 8601 süresini temsil eden bir dize olarak yorumlanır. Bu başarısız olursa, o zaman milisaniye toplam sayısını temsil eden bir sayı olarak yorumlanır. |
| --uyarı-as-hata | Uyarıların hatalarla aynı önemle davranılıp işitilen olmadığını gösterir. |

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