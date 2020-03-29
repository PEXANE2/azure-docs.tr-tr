---
title: Azure Servis Kumaş CLI- sfctl kaos
description: Azure Service Fabric komut satırı arabirimi sfctl hakkında bilgi edinin. Kaosu yönetmek için komutların listesini içerir.
author: jeffj6123
ms.topic: reference
ms.date: 1/16/2020
ms.author: jejarry
ms.openlocfilehash: 6668446363361fbc6d24afc3d11a36a0b786667d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76906172"
---
# <a name="sfctl-chaos"></a>sfctl chaos
Kaos test hizmetini başlatın, durdurun ve rapor edin.

## <a name="subgroups"></a>Alt gruplar
|Alt|Açıklama|
| --- | --- |
| [Zamanlama](service-fabric-sfctl-chaos-schedule.md) | Alın ve kaos programını ayarlayın. |
## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| etkinlikler | Devam belirteci veya zaman aralığına dayalı Kaos olaylarının bir sonraki bölümünü alır. |
| get | Kaos'un statüsünü al. |
| start | Kümede Kaos başlar. |
| Dur | Kümede çalışıyorsa Kaos'u durdurur ve Kaos Çizelgesi'ni durdurulmuş duruma koyar. |

## <a name="sfctl-chaos-events"></a>sfctl kaos olaylar
Devam belirteci veya zaman aralığına dayalı Kaos olaylarının bir sonraki bölümünü alır.

Kaos olaylarının bir sonraki bölümünü almak için ContinuationToken'ı belirtebilirsiniz. Kaos olaylarının yeni bir bölümünün başlangıcını almak için, StartTimeUtc ve EndTimeUtc ile zaman aralığını belirtebilirsiniz. ContinuationToken'ı ve zaman aralığını aynı çağrıda belirtemezsiniz. 100'den fazla Kaos olayı olduğunda, Kaos olayları bir segmentin en fazla 100 Kaos olayı içerdiği birden çok segmentte döndürülür ve bir sonraki segmenti almak için devamı belirteciyle bu API'yi ararsınız.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --devam-belirteç | Devamı belirteç parametresi sonraki sonuç kümesini elde etmek için kullanılır. Sistemden elde edilen sonuçlar tek bir yanıta sığmıyorsa, BOŞ olmayan bir değere sahip bir devam belirteci API'nın yanıtına dahil edilir. Bu değer bir sonraki API çağrısına geçirildiğinde, API sonraki sonuç kümesini döndürür. Başka sonuç yoksa, devam belirteci bir değer içermez. Bu parametrenin değeri URL kodlanmış olmamalıdır. |
| --bitiş zamanı-utc | Kaos raporunun oluşturulabilmek için oluşturulacak zaman aralığının bitiş saatini temsil eden Windows dosya zamanı. Ayrıntılar için [DateTime.ToFileTimeUtc Yöntemi'ne](https\://msdn.microsoft.com/library/system.datetime.tofiletimeutc(v=vs.110).aspx) başvurun. |
| --max-sonuçlar | Sayfalı sorguların bir parçası olarak döndürülecek en fazla sonuç sayısı. Bu parametre, döndürülen sonuç sayısındaki üst sınırı tanımlar. Döndürülen sonuçlar, yapılandırmada tanımlanan maksimum ileti boyutu kısıtlamalarına göre iletiye sığmazsa, belirtilen maksimum sonuçlardan daha az olabilir. Bu parametre sıfır veya belirtilmemişse, sayfalı sorgu, iade iletisindeki sığan mümkün olduğunca çok sonuç içerir. |
| --başlangıç-zaman-utc | Kaos raporunun oluşturulabilmek için oluşturulacak zaman aralığının başlangıç saatini temsil eden Windows dosya zamanı. Ayrıntılar için [DateTime.ToFileTimeUtc Yöntemi'ne](https\://msdn.microsoft.com/library/system.datetime.tofiletimeutc(v=vs.110).aspx) başvurun. |
| --zaman ayarı -t | İşlemi saniyeler içinde gerçekleştirmek için sunucu zaman. Bu zaman sonu, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametrenin varsayılan değeri 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-chaos-get"></a>sfctl kaos olsun
Kaos'un statüsünü al.

Kaos'un çalışıp çalışmadığını, Kaos'u çalıştırmak için kullanılan Kaos parametrelerini ve Kaos Çizelgesi'nin durumunu belirten Kaos durumunu alın.

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

## <a name="sfctl-chaos-start"></a>sfctl kaos başlangıç
Kümede Kaos başlar.

Kaos kümede zaten çalışmıyorsa, Kaos parametrelerinde geçirilen kaosla başlar. Bu arama yapıldığında Kaos zaten çalışıyorsa, arama hata kodu FABRIC_E_CHAOS_ALREADY_RUNNING ile başarısız olur. Daha fazla bilgi için [Hizmet Kumaş kümelerinde Kontrollü Kaos'u uyarın](https\://docs.microsoft.com/azure/service-fabric/service-fabric-controlled-chaos) makaleye bakın.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --uygulama türü-sağlık-ilke-haritası | Belirli uygulama türleri için maksimum yüzde sağlıksız uygulamalar ile JSON kodlanmış liste. Her giriş, bir anahtar olarak uygulama türü adı ve belirtilen uygulama türü uygulamaları değerlendirmek için kullanılan MaxPercentUnhealthyApplications yüzdesini temsil eden bir değer tamsayı olarak belirtir. <br><br> Belirli uygulama türleri için maksimum yüzde sağlıksız uygulamalariçeren bir harita tanımlar. Her giriş, uygulama türü adı anahtar olarak ve belirtilen uygulama türü uygulamalarını değerlendirmek için kullanılan MaxPercentUnhealthyApplications yüzdesini temsil eden bir toplam değer olarak belirtir. Uygulama türü sistem durumu ilkesi eşlemi, özel uygulama türlerini tanımlamak için küme durumu değerlendirmesi sırasında kullanılabilir. Haritada yer alan uygulama türleri, küme durumu ilkesinde tanımlanan genel MaxPercentUnhealthyApplications ile değil, haritada belirtilen yüzdeye göre değerlendirilir. Haritada belirtilen uygulama türlerinin uygulamaları genel uygulama havuzuna dahil edilmez. Örneğin, bir türdeki bazı uygulamalar kritikse, küme yöneticisi bu uygulama türü için eşmeye bir giriş ekleyebilir ve %0'lık bir değer atayabilir (diğer bir süre, herhangi bir hataya tolerans göstermeyin). Diğer tüm uygulamalar, binlerce uygulama örneğinin bazı başarısızlıklarını tolere etmek için %20'ye ayarlanmış MaxPercentUnhealthyApplications ile değerlendirilebilir. Uygulama türü sistem durumu ilkesi eşlemi, yalnızca küme bildirimi, HealthManager/EnableApplicationTypeHealthEvaluation yapılandırma girişini kullanarak uygulama türü sistem durumu değerlendirmesini mümkün kılarsa kullanılır. |
| --kaos-hedef filtresi | JSON iki dize türü tuşları ile sözlük kodlu. İki anahtar NodeTypeInclusionList ve ApplicationInclusionList vardır. Bu anahtarların her ikisi için de değerler dize listesidir. chaos_target_filter, hedeflenen Kaos hataları için tüm filtreleri tanımlar, örneğin yalnızca belirli düğüm türlerini faylama veya yalnızca belirli uygulamaları hatalama. <br><br> chaos_target_filter kullanılmazsa, Kaos tüm küme varlıklarını hatalar. chaos_target_filter kullanılırsa, Kaos yalnızca chaos_target_filter belirtimini karşılayan varlıkları hatalaştırır. NodeTypeInclusionList ve ApplicationInclusionList yalnızca birleşim semantikine izin verir. NodeTypeInclusionList ve ApplicationInclusionList kesişimini belirtmek mümkün değildir. Örneğin, "bu uygulamayı yalnızca bu düğüm türünde yken hata" belirtmek mümkün değildir. Bir varlık NodeTypeInclusionList veya ApplicationInclusionList'e dahil edildikten sonra, bu varlık ChaosTargetFilter kullanılarak dışlanamaz. ApplicationInclusionList'te applicationX görünmese bile, bazı Kaos yineleme uygulamalarındaX, NodeTypeI'de yer alan bir düğümde olduğu için hatalı olabilir. Hem NodeTypeInclusionList hem de ApplicationInclusionList boşsa, Bir ArgumentException atılır. Bu düğüm türlerinin düğümleri için tüm hata türleri (düğümyeniden başlatma, kod paketini yeniden başlatma, yinelemeyi kaldırma, yinelemeyi yeniden başlatma, birincil taşıma ve ikincil taşıma) etkinleştirilir. Düğüm türü (NodeTypeX deyin) NodeTypeInclusionList'te görünmüyorsa, düğüm düzeyi hataları (NodeRestart gibi) NodeTypeX düğümleri için hiçbir zaman etkinleştirilmez, ancak bir uygulama da ApplicationInclusionList, NodeTypeX düğümünde yer alır. Bu sayıyı artırmak için en fazla 100 düğüm türü adı bu listeye eklenebilir, MaxNumberOfNodeTypesInChaosEntityFilter yapılandırması için bir config yükseltmesi gerekir. Bu uygulamaların hizmetlerine ait tüm yinelemeler, Kaos tarafından yineleme hataları (yinelemeyi yeniden başlatma, çoğaltmayı kaldırma, birincil taşıma ve ikincil taşıma) için uygundur. Kaos, yalnızca kod paketi yalnızca bu uygulamaların yinelemelerini barındırıyorsa bir kod paketini yeniden başlatabilir. Bir uygulama bu listede görünmüyorsa, uygulama NodeTypeInclusionList'e dahil edilmiş bir düğüm düğümünde biterse, bazı Kaos yinelemesinde yine de hatalı olabilir. Ancak applicationX yerleşim kısıtlamaları yoluyla nodeTypeY'ye bağlıysa ve applicationX ApplicationInclusionList'te yoksa ve nodeTypeY NodeTypeInclusionList'te yoksa, applicationX hiçbir zaman hata yapılmaz. En fazla 1000 uygulama adları bu listeye dahil edilebilir, bu sayıyı artırmak için, bir config yükseltme MaxNumberOfApplicationsInChaosEntityFilter yapılandırması için gereklidir. |
| --bağlam | JSON kodlanmış harita (string, string) türü anahtar değeri çiftleri. Harita, Kaos koşusu hakkındaki bilgileri kaydetmek için kullanılabilir. Bu tür 100'den fazla çift olamaz ve her dize (anahtar veya değer) en fazla 4095 karakter uzunluğunda olabilir. Bu harita, belirli çalıştırmayla ilgili bağlamı isteğe bağlı olarak depolamak için Kaos çalıştırın başlatıcısı tarafından ayarlanır. |
| --devre dışı-taşıma-çoğaltma-hataları | Birincil taşımayı devre dışı bırakıp ikincil hataları taşır. |
| --max-cluster-stabilizasyon | Tüm küme varlıklarının kararlı ve sağlıklı hale gelmesini beklemek için gereken maksimum süre.  Varsayılan\: 60. <br><br> Kaos yinelemelerde yürütür ve her yinelemenin başlangıcında küme varlıklarının durumunu doğrular. Doğrulama sırasında bir küme varlığı MaxClusterStabilizationTimeoutInSeconds içinde kararlı ve sağlıklı değilse, Kaos bir doğrulama başarısız olay oluşturur. |
| --max-eşzamanlı hatalar | Yineleme başına indüklenen en fazla eşzamanlı hata sayısı. Yinelemelerde kaos yürütülür ve art arda iki yineleme doğrulama aşamasıyla ayrılır. Eşzamanlılık ne kadar yüksekse, hataların enjeksiyonu o kadar agresif olur ve hataları ortaya çıkarmak için daha karmaşık bir dizi durum ortaya çıkar. Öneri, 2 veya 3 değeriyle başlamak ve yukarı çıkarken dikkatli olmaktır.  Varsayılan\: 1. |
| --maksimum yüzde-sağlıksız-uygulamalar | Kaos sırasında küme durumu değerlendirilirken, bir hata bildirmeden önce izin verilen en yüksek sağlıksız uygulamaların yüzdesi. <br><br> Bir hatayı bildirmeden önce izin verilen en yüksek sağlıksız uygulamaların yüzdesi. Örneğin, uygulamaların %10'unun sağlıksız olmasını sağlamak için bu değer 10 olacaktır. Bu yüzde, küme hata olarak kabul edilmeden önce sağlıksız olabilecek uygulamaların en yüksek tolere edilen yüzdesini temsil eder. Yüzdeye saygı duyulur ancak en az bir sağlıksız uygulama varsa, sistem durumu Uyarı olarak değerlendirilir. Bu, ApplicationTypeHealthPolicyMap'e dahil edilen uygulama türlerinin uygulamaları hariç olmak üzere, sağlıksız uygulama sayısının kümedeki toplam uygulama örneği sayısına bölünmesiyle hesaplanır. Hesaplama, az sayıda uygulamada bir başarısızlığa tolerans göstermek için yuvarlar. Varsayılan yüzde sıfırdır. |
| --maksimum yüzde-sağlıksız düğümler | Kaos sırasında küme durumu değerlendirilirken, bir hata bildirmeden önce izin verilen en fazla sağlıksız düğüm yüzdesi. <br><br> Bir hatayı bildirmeden önce izin verilen en yüksek sağlıksız düğüm yüzdesi. Örneğin, düğümlerin %10'unun sağlıksız olmasını sağlamak için bu değer 10 olacaktır. Yüzde, küme hata olarak kabul edilmeden önce sağlıksız olabilecek düğümlerin en yüksek tolere edilen yüzdesini temsil eder. Yüzdeye saygı duyulur ancak en az bir sağlıksız düğüm varsa, sistem durumu Uyarı olarak değerlendirilir. Yüzde, kümedeki toplam düğüm sayısına sağlıksız düğüm sayısı bölünerek hesaplanır. Hesaplama, küçük sayıda düğümdeki bir başarısızlığa tolerans göstermek için yuvarlar. Varsayılan yüzde sıfırdır. Büyük kümelerde, bazı düğümler her zaman aşağı veya onarım için dışarı olacak, bu nedenle bu yüzde bunu tolere etmek için yapılandırılmalıdır. |
| --çalışma süresi | Chaos'un otomatik olarak durmadan önce çalışacağı toplam süre (saniye cinsinden). İzin verilen maksimum değer 4,294,967,295 'dir (System.UInt32.MaxValue).  Varsayılan\: 4294967295. |
| --zaman ayarı -t | Varsayılan\: 60. |
| --bekleme-zaman-arasında-hatalar | Tek bir yineleme içinde ardışık hatalar arasındaki bekleme süresi (saniye cinsinden).  Varsayılan\: 20. <br><br> Değer ne kadar büyükse, hatalar arasındaki çakışma o kadar düşük olur ve kümenin geçtiği durum geçişleri sırası da o kadar basittir. Öneri, 1 ile 5 arasında bir değerle başlamak ve yukarı çıkarken dikkatli olmaktır. |
| --bekleme-zaman-arasında yinelemeler | Kaos'un art arda iki yinelemesi arasında zaman ayrımı (saniye cinsinden). Değeri ne kadar büyükse, hata enjeksiyon oranı da o kadar düşüktür.  Varsayılan\: 30. |
| --uyarı-as-hata | Uyarıların hatalarla aynı önemle davranılıp işitilen olmadığını gösterir. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-chaos-stop"></a>sfctl kaos durdurmak
Kümede çalışıyorsa Kaos'u durdurur ve Kaos Çizelgesi'ni durdurulmuş duruma koyar.

Kaos'un yeni hataları yürütmeyi durdurması. Uçuş hataları tamamlanana kadar çalışmaya devam edecektir. Geçerli Kaos Zamanlaması durdurulmuş bir duruma konur. Bir zamanlama durdurulduktan sonra, durdurulan durumda kalır ve Kaos Zamanlaması'nın yeni çalışmalarından kullanılmaz. Zamanlamaya devam etmek için yeni bir Kaos Zamanlaması ayarlanmalıdır.

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


## <a name="next-steps"></a>Sonraki adımlar
- Servis Kumaşı CLI'yi [kur.](service-fabric-cli.md)
- [Örnek komut dosyalarını](/azure/service-fabric/scripts/sfctl-upgrade-application)kullanarak Service Fabric CLI'yi nasıl kullanacağınızı öğrenin.