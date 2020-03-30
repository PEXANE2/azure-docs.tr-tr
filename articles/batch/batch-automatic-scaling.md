---
title: Azure Toplu İşlem havuzunda işlem düğümlerini otomatik olarak ölçeklendirin | Microsoft Dokümanlar
description: Havuzdaki işlem düğümü sayısını dinamik olarak ayarlamak için bulut havuzunda otomatik ölçekleme olmasını etkinleştirin.
services: batch
documentationcenter: ''
author: LauraBrenner
manager: evansma
editor: ''
ms.assetid: c624cdfc-c5f2-4d13-a7d7-ae080833b779
ms.service: batch
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: multiple
ms.date: 10/24/2019
ms.author: labrenne
ms.custom: H1Hack27Feb2017,fasttrack-edit
ms.openlocfilehash: 398b6d9c3fc05a6cf164b4003f57b94ecd6c1972
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80054009"
---
# <a name="create-an-automatic-formula-for-scaling-compute-nodes-in-a-batch-pool"></a>Toplu Iş havuzunda işlem düğümlerini ölçekleme için otomatik bir formül oluşturma

Azure Toplu İşlem, tanımladığınız parametrelere göre havuzları otomatik olarak ölçeklendirebilir. Otomatik ölçekleme ile, Toplu iş, görev talepleri arttıkça bir havuza dinamik olarak düğüm ekler ve azaldıkça bilgi işlem düğümlerini kaldırır. Toplu İşlem uygulamanız tarafından kullanılan işlem düğümlerinin sayısını otomatik olarak ayarlayarak hem zamandan hem de paradan tasarruf edebilirsiniz.

Tanımladığınız otomatik *ölçeklendirme formülünün* birbiriyle ilişkilendirerek işlem düğümleri havuzunda otomatik ölçeklendirmeyi etkinleştirirsiniz. Toplu İşlem hizmeti, iş yükünüzü yürütmek için gereken bilgi işlem düğümlerinin sayısını belirlemek için otomatik ölçeklendirme formülunu kullanır. İşlem düğümleri adanmış düğümler veya [düşük öncelikli düğümler](batch-low-pri-vms.md)olabilir. Toplu iş, düzenli olarak toplanan hizmet ölçümleri verilerine yanıt verir. Bu ölçüm verilerini kullanarak, Toplu İşlem formülünüze göre ve yapılandırılabilir bir aralıkta havuzdaki işlem düğümü sayısını ayarlar.

Bir havuz oluşturulduğunda veya varolan bir havuzda otomatik ölçekleme etkinleştirebilirsiniz. Otomatik ölçekleme için yapılandırılan bir havuzda varolan formülü de değiştirebilirsiniz. Toplu işlem, formüllerinizi havuzlara atamadan önce değerlendirmenize ve otomatik ölçeklendirme çalıştırmalarının durumunu izlemenize olanak tanır.

Bu makalede, değişkenler, işleçler, işlemler ve işlevler de dahil olmak üzere otomatik ölçeklendirme formüllerinizi oluşturan çeşitli varlıklar açıklanmıştır. Toplu İşlem içinde çeşitli bilgi işlem kaynağı ve görev ölçümlerinin nasıl elde edilebildiğini tartışırız. Bu ölçümleri, kaynak kullanımı ve görev durumuna göre havuzdüğüm sayısını ayarlamak için kullanabilirsiniz. Daha sonra, hem Toplu REST hem de .NET API'lerini kullanarak bir formül oluşturmayı ve havuzda otomatik ölçeklendirmeyi nasıl etkinleştirebileceğimizi açıklarız. Son olarak, birkaç örnek formülleri ile bitirmek.

> [!IMPORTANT]
> Toplu Iş hesabı oluşturduğunuzda, havuzların Toplu Iş hizmeti aboneliğinde (varsayılan) mı yoksa kullanıcı aboneliğinizde mi tahsis edildiğini belirleyen [hesap yapılandırmasını](batch-api-basics.md#account)belirtebilirsiniz. Toplu İş hesabınızı varsayılan Toplu Hizmet yapılandırmasıyla oluşturduysanız, hesabınız işleme için kullanılabilecek maksimum çekirdek sayısıyla sınırlıdır. Toplu Iş hizmeti, hesaplama düğümlerini yalnızca bu çekirdek sınıra kadar ölçeklendiriyor. Bu nedenle, Toplu İşlem hizmeti otomatik ölçeklendirme formülü tarafından belirtilen işlem düğümlerinin hedef sayısına ulaşamayabilir. Hesap kotalarınızı görüntüleme ve artırma hakkında bilgi için [Azure Toplu İş hizmetinin Kotalarını](batch-quota-limit.md) ve sınırlarını görün.
>
>Hesabınızı Kullanıcı Aboneliği yapılandırmasıyla oluşturduysanız, hesabınız abonelik için temel kotada paylaşır. Daha fazla bilgi için [Azure aboneliği ve hizmet limitleri, kotalar ve kısıtlamalar](../azure-resource-manager/management/azure-subscription-service-limits.md) sayfasındaki [Sanal Makine limitleri](../azure-resource-manager/management/azure-subscription-service-limits.md#virtual-machines-limits) bölümüne bakın.
>
>

## <a name="automatic-scaling-formulas"></a>Otomatik ölçekleme formülleri

Otomatik ölçekleme formülü, tanımladığınız ve bir veya daha fazla deyim içeren bir dize değeridir. Otomatik ölçekformülü, havuzun otomatik [Ölçeklendirme Öğesine][rest_autoscaleformula] (Toplu REST) veya [CloudPool.AutoScaleFormula][net_cloudpool_autoscaleformula] özelliğine (Toplu .NET) atanır. Toplu İşlem hizmeti, bir sonraki işlem aralığı için havuzdaki bilgi işlem düğümlerinin hedef sayısını belirlemek için formülünüzü kullanır. Formül dizesi 8 KB'yi geçemez, yarım iki nokta yla ayrılmış en fazla 100 deyim içerebilir ve satır sonları ve açıklamalar içerebilir.

Otomatik ölçekleme formüllerini Toplu otomatik ölçeklendirme "dili" olarak düşünebilirsiniz. Formül deyimleri, hem hizmet tanımlı değişkenleri (Toplu Iş hizmeti tarafından tanımlanan değişkenler) hem de kullanıcı tanımlı değişkenleri (tanımladığınız değişkenler) içerebilen serbest biçimli ifadelerdir. Yerleşik türleri, işleçler ve işlevler kullanarak bu değerler üzerinde çeşitli işlemler gerçekleştirebilirler. Örneğin, bir deyim aşağıdaki formu alabilir:

```
$myNewVariable = function($ServiceDefinedVariable, $myCustomVariable);
```

Formüller genellikle önceki deyimlerde elde edilen değerler üzerinde işlem gerçekleştiren birden çok deyim içerir. Örneğin, önce bir değer `variable1`elde etmek, sonra doldurmak için `variable2`bir fonksiyona geçmek:

```
$variable1 = function1($ServiceDefinedVariable);
$variable2 = function2($OtherServiceDefinedVariable, $variable1);
```

Hedef işlem düğümü sayısına ulaşmak için bu ifadeleri otomatik ölçek formülünüze ekleyin. Her bir düğüm türü için bir hedef tanımlayabilmeniz için, özel düğümlerin ve düşük öncelikli düğümlerin her birinin kendi hedef ayarları vardır. Otomatik ölçeklendirme formülü, adanmış düğümler için hedef değer, düşük öncelikli düğümler için bir hedef değer veya her ikisini de içerebilir.

Düğümlerin hedef sayısı, havuzda bu türdeki geçerli düğüm sayısıyla daha yüksek, daha düşük veya aynı olabilir. Toplu işlem havuzun otomatik ölçeklendirme formüllerini belirli bir aralıkta değerlendirir (bkz. [otomatik ölçeklendirme aralıkları).](#automatic-scaling-interval) Toplu işlem, havuzdaki her düğüm türünün hedef sayısını, otomatik ölçek formülünüzün değerlendirme sırasında belirlediği sayıya ayarlar.

### <a name="sample-autoscale-formulas"></a>Örnek otomatik ölçek formülleri

Aşağıda, çoğu senaryoiçin çalışacak şekilde ayarlanabilen iki otomatik ölçeklendirme formülüne örnekler verilmiştir. Değişkenler `startingNumberOfVMs` ve `maxNumberofVMs` örnek formüller ihtiyaçlarınıza göre ayarlanabilir.

#### <a name="pending-tasks"></a>Bekleyen görevler

```
startingNumberOfVMs = 1;
maxNumberofVMs = 25;
pendingTaskSamplePercent = $PendingTasks.GetSamplePercent(180 * TimeInterval_Second);
pendingTaskSamples = pendingTaskSamplePercent < 70 ? startingNumberOfVMs : avg($PendingTasks.GetSample(180 * TimeInterval_Second));
$TargetDedicatedNodes=min(maxNumberofVMs, pendingTaskSamples);
$NodeDeallocationOption = taskcompletion;
```

Bu otomatik ölçeklendirme formülü ile havuz başlangıçta tek bir VM ile oluşturulur. Metrik, `$PendingTasks` çalışan veya sıraya alan görev sayısını tanımlar. Formül, son 180 saniyeiçinde bekleyen görevlerin ortalama `$TargetDedicatedNodes` sayısını bulur ve değişkeni buna göre ayarlar. Formül, ayrılmış düğümlerin hedef sayısının asla 25 VM'yi geçmemesini sağlar. Yeni görevler gönderildikçe, havuz otomatik olarak büyür. Görevler tamamlandıkça, VM'ler birer birer serbest kalır ve otomatik ölçekleme formülü havuzu küçültür.

Bu formül adanmış düğümleri ölçeklendirebilir, ancak düşük öncelikli düğümleri ölçeklendirmek için de uygulanacak şekilde değiştirilebilir.

#### <a name="preempted-nodes"></a>Preempted düğümler 

```
maxNumberofVMs = 25;
$TargetDedicatedNodes = min(maxNumberofVMs, $PreemptedNodeCount.GetSample(180 * TimeInterval_Second));
$TargetLowPriorityNodes = min(maxNumberofVMs , maxNumberofVMs - $TargetDedicatedNodes);
$NodeDeallocationOption = taskcompletion;
```

Bu örnek, 25 düşük öncelikli düğümle başlayan bir havuz oluşturur. Düşük öncelikli bir düğüm her önceden önce geldiğinde, özel bir düğümle değiştirilir. İlk örnekte olduğu `maxNumberofVMs` gibi, değişken havuzun 25 VM'yi aşmasını engeller. Bu örnek, düşük öncelikli VM'lerden yararlanırken, havuzun ömrü boyunca yalnızca sabit sayıda preemption oluşmasını da sağlar.

## <a name="variables"></a>Değişkenler

Otomatik ölçek formüllerinizde hem **hizmet tanımlı** hem de **kullanıcı tanımlı** değişkenler kullanabilirsiniz. Hizmet tanımlı değişkenler Toplu İşlem hizmetine dahil edilir. Bazı hizmet tanımlı değişkenler okuma-yazma ve bazıları salt okunur. Kullanıcı tanımlı değişkenler tanımladığınız değişkenlerdir. Önceki bölümde gösterilen örnek formülünde `$TargetDedicatedNodes` `$PendingTasks` hizmet tanımlı değişkenler vardır. Değişkenler `startingNumberOfVMs` `maxNumberofVMs` ve kullanıcı tanımlı değişkenlerdir.

> [!NOTE]
> Hizmet tanımlı değişkenler her zaman bir dolar işareti ($) tarafından önce gelir. Kullanıcı tanımlı değişkenler için dolar işareti isteğe bağlıdır.
>
>

Aşağıdaki tablolar, Toplu İşlem hizmeti tarafından tanımlanan hem okuma-yazma hem de salt okunur değişkenlerini gösterir.

Havuzdaki işlem düğümlerinin sayısını yönetmek için bu hizmet tanımlı değişkenlerin değerlerini alabilir ve ayarlayabilirsiniz:

| Okuma yazma hizmeti tanımlı değişkenler | Açıklama |
| --- | --- |
| $TargetDedicatedNodes |Havuz için özel işlem düğümlerinin hedef sayısı. Ayrılmış düğüm sayısı hedef olarak belirtilir, çünkü bir havuz her zaman istenilen sayıda düğümelde emeyebilir. Örneğin, ayrılmış düğümlerin hedef sayısı, havuz ilk hedefe ulaşmadan önce otomatik ölçeklendirme değerlendirmesiyle değiştirilirse, havuz hedefe ulaşamayabilir. <br /><br /> Toplu İşlem yapılandırmasıyla oluşturulan bir hesaptaki havuz, hedef bir Toplu İş hesabı düğümünü veya çekirdek kotayı aşarsa hedefine ulaşamayabilir. Hedef, abonelik için paylaşılan çekirdek kotayı aşarsa, Kullanıcı Aboneliği yapılandırmasıyla oluşturulan bir hesaptaki havuz hedefine ulaşamayabilir.|
| $TargetLowPriorityNodes |Havuz için düşük öncelikli işlem düğümlerinin hedef sayısı. Bir havuz her zaman istenilen sayıda düğüm elde olmayabilir, çünkü düşük öncelikli düğüm sayısı hedef olarak belirtilir. Örneğin, havuz ilk hedefe ulaşmadan önce düşük öncelikli düğümlerin hedef sayısı otomatik ölçeklendirme değerlendirmesiyle değiştirilirse, havuz hedefe ulaşamayabilir. Hedef bir Toplu Iş hesabı düğümünü veya çekirdek kotayı aşarsa, havuz hedefine de ulaşamayabilir. <br /><br /> Düşük öncelikli bilgi işlem düğümleri hakkında daha fazla bilgi için [bkz.](batch-low-pri-vms.md) |
| $NodeDeallocationOption |İşlem düğümleri havuzdan kaldırıldığında oluşan eylem. Olası değerler şunlardır:<ul><li>**requeue**-- Varsayılan değer. Görevleri hemen sonlandırır ve yeniden zamanlanmaları için iş kuyruğuna geri koyar. Bu eylem, düğümlerin hedef sayısına mümkün olduğunca çabuk erişilmesini sağlar, ancak çalışan görevler kesintiye uğrayacağı ve yeniden başlatılması gerektiği için daha az verimli olabilir ve daha önce yaptıkları herhangi bir işi boşa harcamış olurlar. <li>**sona erdirme**--Görevleri hemen sonlandırır ve iş kuyruğundan kaldırır.<li>**görev tamamlama**--Şu anda çalışan görevlerin tamamlanmasını bekler ve sonra düğümü havuzdan kaldırır. Görevlerin kesintiye uğramasını ve yeniden sıraya girmesini önlemek ve görevin yaptığı herhangi bir işi boşa harcamak için bu seçeneği kullanın. <li>**tutulan veriler**--Düğümdeki tüm yerel görev tarafından tutulan verilerin havuzdan çıkarmadan önce temizlenmesini bekler.</ul> |

> [!NOTE]
> Değişken `$TargetDedicatedNodes` diğer ad `$TargetDedicated`kullanılarak da belirtilebilir. Benzer şekilde, `$TargetLowPriorityNodes` değişken diğer ad `$TargetLowPriority`kullanılarak belirtilebilir. Hem tam adlandırılmış değişken hem de diğer adı formül tarafından ayarlanırsa, tam adlandırılmış değişkene atanan değer önceliklidir.
>
>

Toplu İşlem hizmetinden ölçümlere dayalı ayarlamalar yapmak için bu hizmet tanımlı değişkenlerin değerini alabilirsiniz:

| Salt okunur hizmet tanımlı değişkenler | Açıklama |
| --- | --- |
| $CPUPercent |CPU kullanımının ortalama yüzdesi. |
| $WallClockSeconds |Tüketilen saniye sayısı. |
| $MemoryBytes |Kullanılan ortalama megabayt sayısı. |
| $DiskBytes |Yerel disklerde kullanılan ortalama gigabayt sayısı. |
| $DiskReadBytes |Okunan bayt sayısı. |
| $DiskWriteBytes |Yazılan bayt sayısı. |
| $DiskReadOps |Gerçekleştirilen okuma diski işlemlerinin sayısı. |
| $DiskWriteOps |Gerçekleştirilen yazma diski işlemlerinin sayısı. |
| $NetworkInBytes |Gelen bayt sayısı. |
| $NetworkOutBytes |Giden bayt sayısı. |
| $SampleNodeCount |İşlem düğümlerinin sayısı. |
| $ActiveTasks |Yürütmeye hazır olan ancak henüz yürütülmeyen görev sayısı. $ActiveTasks sayısı, etkin durumda olan ve bağımlılıkları karşılanan tüm görevleri içerir. Etkin durumda olan ancak bağımlılıkları karşılanamayan tüm görevler $ActiveTasks sayısının dışında tutulur. Çok örnekli bir görev için $ActiveTasks, görevde ayarlanan örnek sayısını içerir.|
| $RunningTasks |Çalışan durumdaki görev sayısı. |
| $PendingTasks |$ActiveTasks ve $RunningTasks toplamı. |
| $SucceededTasks |Başarıyla tamamlanan görev sayısı. |
| $FailedTasks |Başarısız olan görev sayısı. |
| $CurrentDedicatedNodes |Adanmış işlem düğümlerinin geçerli sayısı. |
| $CurrentLowPriorityNodes |Önceden verilmiş düğümler de dahil olmak üzere, düşük öncelikli işlem düğümlerinin geçerli sayısı. |
| $PreemptedNodeCount | Önceden engellenmiş durumda olan havuzdaki düğüm sayısı. |

> [!TIP]
> Önceki tabloda gösterilen salt okunur, hizmet tanımlı değişkenler, her biriyle ilişkili verilere erişmek için çeşitli yöntemler sağlayan *nesnelerdir.* Daha fazla bilgi için [bkz.](#getsampledata)
>
>

## <a name="types"></a>Türler

Bu türler bir formülde desteklenir:

* double
* doubleVec
* doubleVecList
* string
* zaman damgası--zaman damgası aşağıdaki üyeleri içeren bileşik bir yapıdır:

  * yıl
  * ay (1-12)
  * gün (1-31)
  * hafta içi (sayı biçiminde; örneğin, Pazartesi için 1)
  * saat (24 saatlik sayı biçiminde; örneğin, 13 1PM anlamına gelir)
  * dakika (00-59)
  * ikinci (00-59)
* zaman aralığı

  * TimeInterval_Zero
  * TimeInterval_100ns
  * TimeInterval_Microsecond
  * TimeInterval_Millisecond
  * TimeInterval_Second
  * TimeInterval_Minute
  * TimeInterval_Hour
  * TimeInterval_Day
  * TimeInterval_Week
  * TimeInterval_Year

## <a name="operations"></a>İşlemler

Bu işlemler, önceki bölümde listelenen türlerde izin verilir.

| İşlem | Desteklenen operatörler | Sonuç türü |
| --- | --- | --- |
| çift *operatör* çift |+, -, *, / |double |
| çift *operatör* zaman aralığı |* |zaman aralığı |
| doubleVec *operatörü* çift |+, -, *, / |doubleVec |
| doubleVec *işleci* doubleVec |+, -, *, / |doubleVec |
| zaman aralığı *işleci* çift |*, / |zaman aralığı |
| zaman aralığı *operatör* zaman aralığı |+, - |zaman aralığı |
| zaman aralığı *operatör* zaman damgası |+ |timestamp |
| zaman damgası *operatörü* zaman aralığı |+ |timestamp |
| zaman damgası *operatörü* zaman damgası |- |zaman aralığı |
| *operatör*çift |-, ! |double |
| *operatör*zaman aralığı |- |zaman aralığı |
| çift *operatör* çift |<, <=, ==, >=, >, != |double |
| string *operatör* dizesi |<, <=, ==, >=, >, != |double |
| zaman damgası *operatörü* zaman damgası |<, <=, ==, >=, >, != |double |
| zaman aralığı *operatör* zaman aralığı |<, <=, ==, >=, >, != |double |
| çift *operatör* çift |&&, &#124;&#124; |double |

Bir üçüncül işleç ile bir`double ? statement1 : statement2`çift test ederken ( ), sıfır olmayan **doğrudur**ve sıfır **yanlıştır**.

## <a name="functions"></a>İşlevler
Bu önceden tanımlanmış **işlevler,** otomatik ölçekleme formülü tanımlamada kullanabileceğiniz bir şekilde kullanılabilir.

| İşlev | Dönüş türü | Açıklama |
| --- | --- | --- |
| avg(doubleVecList) |double |DoubleVecList'teki tüm değerlerin ortalama değerini verir. |
| len(doubleVecList) |double |DoubleVecList'ten oluşturulan vektörün uzunluğunu verir. |
| lg(çift) |double |Çiftin günlük tabanı 2'yi verir. |
| lg(doubleVecList) |doubleVec |DoubleVecList'in bileşen açısından günlük tabanı 2'yi verir. Parametre için bir vec(çift) açıkça geçirilmelidir. Aksi takdirde, çift lg (çift) versiyonu varsayılır. |
| ln(çift) |double |Çiftin doğal günlüğünü verir. |
| ln(doubleVecList) |doubleVec |Çiftin doğal günlüğünü verir. |
| günlük(çift) |double |Çiftin günlük tabanını 10'u verir. |
| log(doubleVecList) |doubleVec |DoubleVecList'in bileşen açısından günlük tabanı 10'u verir. Tek çift parametre için bir vec(çift) açıkça geçirilmelidir. Aksi takdirde, çift günlük (çift) sürümü kabul edilir. |
| max(doubleVecList) |double |DoubleVecList'teki maksimum değeri verir. |
| min(doubleVecList) |double |DoubleVecList'teki minimum değeri verir. |
| norm(doubleVecList) |double |DoubleVecList'ten oluşturulan vektörün iki normuna karşılık verir. |
| yüzdelik (doubleVec v, çift p) |double |Vektör v'nin yüzdek öğesini verir. |
| rand() |double |0,0 ile 1,0 arasında rasgele bir değer verir. |
| aralığı(doubleVecList) |double |DoubleVecList'teki min ve maksimum değerler arasındaki farkı verir. |
| std(doubleVecList) |double |DoubleVecList'teki değerlerin örnek standart sapını verir. |
| stop() | |Otomatik ölçeklendirme ifadesinin değerlendirilmesini durdurur. |
| toplamı(doubleVecList) |double |DoubleVecList'in tüm bileşenlerinin toplamını verir. |
| zaman(string dateTime="") |timestamp |Parametreler geçirilirse geçerli saatin zaman damgasını veya geçirilirse dateTime dizesinin zaman damgasını döndürür. Desteklenen dateTime biçimleri W3C-DTF ve RFC 1123'tür. |
| val(doubleVec v, double i) |double |Vektör v'deki i konumundaki öğenin değerini sıfır başlangıç indeksi ile döndürür. |

Önceki tabloda açıklanan işlevlerden bazıları bir listeyi bağımsız değişken olarak kabul edebilir. Virgülden ayrılmış liste *çift* ve *doubleVec*herhangi bir kombinasyonudur. Örnek:

`doubleVecList := ( (double | doubleVec)+(, (double | doubleVec) )* )?`

*DoubleVecList* değeri değerlendirmeden önce tek bir *doubleVec* dönüştürülür. Örneğin, eğer, `v = [1,2,3]`o `avg(v)` zaman arama `avg(1,2,3)`arama eşdeğerdir. Arama `avg(v, 7)` arama ile `avg(1,2,3,7)`eşdeğerdir.

## <a name="obtain-sample-data"></a><a name="getsampledata"></a>Örnek veri alma

Otomatik ölçekformülleri, Toplu İşlem hizmeti tarafından sağlanan ölçümler verileri (örnekler) üzerinde hareket eder. Bir formül, hizmetten elde ettiği değerlere göre havuz boyutunu büyütüyor veya küçültür. Daha önce açıklanan hizmet tanımlı değişkenler, bu nesneyle ilişkili verilere erişmek için çeşitli yöntemler sağlayan nesnelerdir. Örneğin, aşağıdaki ifade CPU kullanımının son beş dakikasını almak için bir istek gösterir:

```
$CPUPercent.GetSample(TimeInterval_Minute * 5)
```

| Yöntem | Açıklama |
| --- | --- |
| GetSample() |Yöntem, `GetSample()` veri örneklerinin bir vektörü döndürür.<br/><br/>Bir örnek 30 saniye değerinde ölçüm verisidir. Başka bir deyişle, örnekler her 30 saniyede elde edilir. Ancak aşağıda belirtildiği gibi, bir numunenin ne zaman toplandığı ile formüliçin kullanılabilir olduğu zaman arasında bir gecikme vardır. Bu nedenle, belirli bir süre için tüm örnekler bir formül le değerlendirilmek üzere kullanılamaz.<ul><li>`doubleVec GetSample(double count)`<br/>Toplanan en son örneklerden elde edilecek örnek sayısını belirtir.<br/><br/>`GetSample(1)`kullanılabilir son örneği döndürür. Ancak, örneğin ne zaman toplandığını bilmek mümkün olmadığından, bu kullanılmamalıdır. *when* `$CPUPercent` Yeni olabilir, ya da sistem sorunları nedeniyle, çok daha eski olabilir. Bu gibi durumlarda aşağıda gösterildiği gibi bir zaman aralığı kullanmak daha iyidir.<li>`doubleVec GetSample((timestamp or timeinterval) startTime [, double samplePercent])`<br/>Örnek verileri toplamak için bir zaman çerçevesi belirtir. İsteğe bağlı olarak, istenen zaman diliminde bulunması gereken örneklerin yüzdesini de belirtir.<br/><br/>`$CPUPercent.GetSample(TimeInterval_Minute * 10)`son 10 dakikadaki tüm örnekler CPUPercent geçmişinde mevcutsa 20 örnek döndürecek. Ancak tarihin son dakikası kullanılamıyorsa, yalnızca 18 örnek döndürülürdü. Bu durumda:<br/><br/>`$CPUPercent.GetSample(TimeInterval_Minute * 10, 95)`örneklerin yalnızca yüzde 90'ı kullanılabilir olduğundan başarısız olur.<br/><br/>`$CPUPercent.GetSample(TimeInterval_Minute * 10, 80)`başarılı olacaktır.<li>`doubleVec GetSample((timestamp or timeinterval) startTime, (timestamp or timeinterval) endTime [, double samplePercent])`<br/>Hem başlangıç zamanı hem de bitiş saati ile veri toplamak için bir zaman çerçevesi belirtir.<br/><br/>Yukarıda belirtildiği gibi, bir numunenin ne zaman toplandığı ile formüliçin kullanılabilir olduğu zaman arasında bir gecikme vardır. Yöntemi kullanırken bu `GetSample` gecikmeyi göz önünde bulundurun. Aşağıya `GetSamplePercent` bakın. |
| GetSamplePeriod() |Geçmiş örnek veri kümesinde alınan örneklerin dönemini verir. |
| Sayım() |Metrik geçmişindeki toplam örnek sayısını verir. |
| HistoryBeginTime() |Metrik için kullanılabilir en eski veri örneğinin zaman damgasını döndürür. |
| GetSamplePercent() |Belirli bir zaman aralığı için kullanılabilen örneklerin yüzdesini verir. Örnek:<br/><br/>`doubleVec GetSamplePercent( (timestamp or timeinterval) startTime [, (timestamp or timeinterval) endTime] )`<br/><br/>Döndürülen `GetSample` örneklerin yüzdesi `samplePercent` belirtilenden azsa yöntem başarısız olduğundan, önce yöntemi denetlemek için kullanabilirsiniz. `GetSamplePercent` Daha sonra, otomatik ölçeklendirme değerlendirmesini durdurmadan, yeterli örnek varsa alternatif bir eylem gerçekleştirebilirsiniz. |

### <a name="samples-sample-percentage-and-the-getsample-method"></a>Örnekler, numune yüzdesi ve *GetSample()* yöntemi
Otomatik ölçeklendirme formülünün temel işlemi, görev ve kaynak metrik verilerini elde etmek ve ardından bu verilere göre havuz boyutunu ayarlamaktır. Bu nedenle, otomatik ölçek formüllerinin ölçüm verileri (örnekler) ile nasıl etkileşimde olduğunu net bir şekilde anlamak önemlidir.

**Örnekler**

Toplu İşlem hizmeti, görev ve kaynak ölçümlerinin örneklerini düzenli aralıklarla alır ve bunları otomatik ölçeklendirme formüllerinizde kullanılabilir hale getirir. Bu örnekler Toplu İşlem hizmeti tarafından her 30 saniyede bir kaydedilir. Ancak, genellikle bu örneklerin kaydedildiği zaman ile otomatik ölçekformülleriniziçin kullanılabilir hale getirildikleri (ve bunlar tarafından okunabilir) arasında bir gecikme vardır. Ayrıca, ağ veya diğer altyapı sorunları gibi çeşitli etkenler nedeniyle, örnekler belirli bir aralık için kaydedilemez.

**Örnek yüzdesi**

Yönteme geçirildiğinde veya `samplePercent` `GetSamplePercent()` yöntem çağrıldığında, _yüzde,_ Toplu İşlem hizmeti tarafından kaydedilen toplam olası örnek sayısı ile otomatik ölçeklendirme formülünüzde kullanılabilen örnek sayısı arasındaki karşılaştırmayı ifade eder. `GetSample()`

Örnek olarak 10 dakikalık bir zaman ayarı bakalım. Numuneler 10 dakikalık bir zaman aralıkları içinde her 30 saniyede bir kaydedildiklerinden, Toplu İşlem tarafından kaydedilen toplam toplam örnek sayısı 20 örnek (dakikada 2) olacaktır. Ancak, raporlama mekanizmasının doğal gecikmesi ve Azure'daki diğer sorunlar nedeniyle, okuma için otomatik ölçeklendirme formülünüzde yalnızca 15 örnek kullanılabilir. Örneğin, bu 10 dakikalık süre için, kaydedilen toplam örnek sayısının yalnızca %75'i formülünüzde kullanılabilir.

**GetSample() ve örnek aralıkları**

Otomatik ölçek formülleriniz, düğüm ekleyerek veya düğümleri &mdash; kaldırarak havuzlarınızı büyütecek ve küçültecek. Düğümler size paraya mal olduğundan, formüllerinizin yeterli verilere dayalı akıllı bir çözümyöntemi kullandığından emin olmak istersiniz. Bu nedenle, formüllerinizde eğilim tipi bir çözümleme kullanmanızı öneririz. Bu tür, toplanan bir dizi örne göre havuzlarınızı büyütür ve küçültür.

Bunu yapmak için, örneklerin bir vektör dönmek için kullanın: `GetSample(interval look-back start, interval look-back end)`

```
$runningTasksSample = $RunningTasks.GetSample(1 * TimeInterval_Minute, 6 * TimeInterval_Minute);
```

Yukarıdaki satır Toplu İşlem tarafından değerlendirildiğinde, bir dizi örneği değerlerin vektörü olarak döndürür. Örnek:

```
$runningTasksSample=[1,1,1,1,1,1,1,1,1,1];
```

Örneklerin vektörü topladıktan sonra, ", " `min()` `max()`ve toplanan `avg()` aralıktan anlamlı değerler türetmek gibi işlevleri kullanabilirsiniz.

Ek güvenlik için, belirli bir zaman dilimi için belirli bir örnek yüzdeden daha az kullanılabilirse, formül değerlendirmesini başarısız olmaya zorlayabilirsiniz. Bir formül değerlendirmesini başarısız olmaya zorladığınızda, belirtilen örnekler yüzdesi kullanılamıyorsa Toplu İşlem'e formülün daha fazla değerlendirilmesini durdurmasını emre emre emre emrederim. Bu durumda, havuz boyutunda herhangi bir değişiklik yapılmaz. Değerlendirmenin başarılı olması için gerekli örneklerin yüzdesini belirtmek için, `GetSample()`üçüncü parametre olarak belirtin. Burada, örneklerin yüzde 75'inin gereksinimi belirtilir:

```
$runningTasksSample = $RunningTasks.GetSample(60 * TimeInterval_Second, 120 * TimeInterval_Second, 75);
```

Örnek kullanılabilirlikte bir gecikme olabileceğinden, her zaman bir dakikadan eski bir geri dönüş başlangıç süresiyle bir zaman aralığı belirtmek önemlidir. Örneklerin sistem de yayılması yaklaşık bir dakika sürer, bu `(0 * TimeInterval_Second, 60 * TimeInterval_Second)` nedenle aralıktaki numuneler kullanılamayabilir. Yine, belirli bir örnek yüzdesi `GetSample()` gereksinimini zorlamak için yüzde parametresini kullanabilirsiniz.

> [!IMPORTANT]
> Yalnızca otomatik ölçek **formüllerinize *only* güvenmemenizi `GetSample(1)` ** **şiddetle tavsiye** ediyoruz. Bunun nedeni `GetSample(1)` aslında Toplu servise şöyle demektedir: "Ne kadar zaman önce aldıysanız alın, bana sahip olduğunuz son örneği verin." Yalnızca tek bir örnek olduğundan ve eski bir örnek olabileceğinden, son görev veya kaynak durumunun büyük resminin temsilcisi olmayabilir. Kullanıyorsanız, `GetSample(1)`formülünüzün güvendiği tek veri noktası nın değil, daha büyük bir ifadenin parçası olduğundan emin olun.
>
>

## <a name="metrics"></a>Ölçümler

Bir formül tanımlarken hem kaynak hem de görev ölçümlerini kullanabilirsiniz. Elde ettiğiniz ve değerlendirdiğiniz ölçümler verilerine göre havuzdaki ayrılmış düğümlerin hedef sayısını ayarlarsınız. Her metrik hakkında daha fazla bilgi için yukarıdaki [Değişkenler](#variables) bölümüne bakın.

<table>
  <tr>
    <th>Ölçüm</th>
    <th>Açıklama</th>
  </tr>
  <tr>
    <td><b>Kaynak</b></td>
    <td><p>Kaynak ölçümleri CPU'yu, bant genişliğini, bilgi işlem düğümlerinin bellek kullanımını ve düğüm sayısını temel alınr.</p>
        <p> Bu hizmet tanımlı değişkenler düğüm sayısına dayalı ayarlamalar yapmak için yararlıdır:</p>
    <p><ul>
            <li>$TargetDedicatedNodes</li>
            <li>$TargetLowPriorityNodes</li>
            <li>$CurrentDedicatedNodes</li>
            <li>$CurrentLowPriorityNodes</li>
            <li>$PreemptedNodeCount</li>
            <li>$SampleNodeCount</li>
    </ul></p>
    <p>Bu hizmet tanımlı değişkenler düğüm kaynağı kullanımına dayalı ayarlamalar yapmak için yararlıdır:</p>
    <p><ul>
      <li>$CPUPercent</li>
      <li>$WallClockSeconds</li>
      <li>$MemoryBytes</li>
      <li>$DiskBytes</li>
      <li>$DiskReadBytes</li>
      <li>$DiskWriteBytes</li>
      <li>$DiskReadOps</li>
      <li>$DiskWriteOps</li>
      <li>$NetworkInBytes</li>
      <li>$NetworkOutBytes</li></ul></p>
  </tr>
  <tr>
    <td><b>Görev</b></td>
    <td><p>Görev ölçümleri, Etkin, Beklemede ve Tamamlandı gibi görevlerin durumunu temel alıntır. Aşağıdaki hizmet tanımlı değişkenler, görev ölçümlerini temel alan havuz boyutu ayarlamaları yapmak için yararlıdır:</p>
    <p><ul>
      <li>$ActiveTasks</li>
      <li>$RunningTasks</li>
      <li>$PendingTasks</li>
      <li>$SucceededTasks</li>
            <li>$FailedTasks</li></ul></p>
        </td>
  </tr>
</table>

## <a name="write-an-autoscale-formula"></a>Otomatik ölçek formülü yazma

Yukarıdaki bileşenleri kullanan deyimler oluşturarak bir otomatik ölçeklendirme formülü oluşturursunuz, ardından bu deyimleri tam bir formülde birleştirirsiniz. Bu bölümde, bazı gerçek dünya ölçekleme kararları gerçekleştirebilen örnek bir otomatik ölçeklendirme formülü oluşturuyoruz.

İlk olarak, yeni otomatik ölçeklendirme formülümüz için gereksinimleri tanımlayalım. Formül şunları yapmalı:

1. CPU kullanımı yüksekse, havuzdaki özel işlem düğümlerinin hedef sayısını artırın.
1. CPU kullanımı düşük olduğunda havuzdaki ayrılmış bilgi işlem düğümlerinin hedef sayısını azaltın.
1. Her zaman en fazla adanmış düğüm sayısını 400 ile sınırlayın.
1. Düğüm sayısını azaltırken, görevleri çalıştıran düğümleri kaldırmayın; gerekirse, düğümleri kaldırmak için görevler bitene kadar bekleyin.

Yüksek CPU kullanımı sırasında düğüm sayısını artırmak için, kullanıcı tanımlı bir değişkeni ()`$totalDedicatedNodes`geçerli hedef düğüm sayısının yüzde 110'u olan, ancak yalnızca son 10 dakikadaki minimum ortalama CPU kullanımı yüzde 70'in üzerindeyse dolduran deyimi tanımlayın. Aksi takdirde, adanmış düğümlerin geçerli sayısı için değeri kullanın.

```
$totalDedicatedNodes =
    (min($CPUPercent.GetSample(TimeInterval_Minute * 10)) > 0.7) ?
    ($CurrentDedicatedNodes * 1.1) : $CurrentDedicatedNodes;
```

Düşük CPU kullanımı sırasında adanmış düğüm sayısını *azaltmak* için, formülümüzdeki `$totalDedicatedNodes` bir sonraki deyim, son 60 dakikadaki ortalama CPU kullanımı yüzde 20'nin altında ysa, aynı değişkeni geçerli hedef düğüm sayısının yüzde 90'ına ayarlar. Aksi takdirde, yukarıdaki `$totalDedicatedNodes` ifadede dolduran geçerli değerini kullanın.

```
$totalDedicatedNodes =
    (avg($CPUPercent.GetSample(TimeInterval_Minute * 60)) < 0.2) ?
    ($CurrentDedicatedNodes * 0.9) : $totalDedicatedNodes;
```

Şimdi, özel işlem düğümlerinin hedef sayısını en fazla 400 ile sınırlandırın:

```
$TargetDedicatedNodes = min(400, $totalDedicatedNodes)
```

İşte formülün tamamı:

```
$totalDedicatedNodes =
    (min($CPUPercent.GetSample(TimeInterval_Minute * 10)) > 0.7) ?
    ($CurrentDedicatedNodes * 1.1) : $CurrentDedicatedNodes;
$totalDedicatedNodes =
    (avg($CPUPercent.GetSample(TimeInterval_Minute * 60)) < 0.2) ?
    ($CurrentDedicatedNodes * 0.9) : $totalDedicatedNodes;
$TargetDedicatedNodes = min(400, $totalDedicatedNodes)
```

## <a name="create-an-autoscale-enabled-pool-with-batch-sdks"></a>Toplu SDK'lar içeren otomatik ölçek özellikli bir havuz oluşturma

Havuz otomatiklendirme [Toplu SDKs](batch-apis-tools.md#azure-accounts-for-batch-development)herhangi kullanılarak yapılandırılabilir , [Toplu REST API](https://docs.microsoft.com/rest/api/batchservice/) [Toplu PowerShell cmdlets](batch-powershell-cmdlets-get-started.md), ve [Toplu CLI](batch-cli-get-started.md). Bu bölümde hem .NET hem de Python için örnekler görebilirsiniz.

### <a name="net"></a>.NET

.NET'te otomatik ölçekleme etkinleştirilmiş bir havuz oluşturmak için aşağıdaki adımları izleyin:

1. [BatchClient.PoolOperations.CreatePool](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.pooloperations.createpool)ile havuz oluşturun.
1. [CloudPool.AutoScaleEtkin](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.autoscaleenabled) özelliğini `true`.
1. Otomatik ölçeklendirme formülünüzle [CloudPool.AutoScaleFormula](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.autoscaleformula) özelliğini ayarlayın.
1. (İsteğe bağlı) [CloudPool.AutoScaleEvaluationInterval](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.autoscaleevaluationinterval) özelliğini ayarlayın (varsayılan değer 15 dakikadır).
1. [CloudPool.Commit](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.commit) veya [CommitAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.commitasync)ile havuzu commit.

Aşağıdaki kod snippet .NET'te otomatik ölçek etkin bir havuz oluşturur. Havuzun otomatik ölçek formülü, pazartesi günleri 5 ve haftanın her günü 1'e adanmış düğümlerin hedef sayısını ayarlar. [Otomatik ölçekleme aralığı](#automatic-scaling-interval) 30 dakika olarak ayarlanır. Bu ve bu makaledeki diğer C# parçacıklarında, `myBatchClient` [BatchClient][net_batchclient] sınıfının düzgün bir şekilde başfağa edilmiş bir örneği vardır.

```csharp
CloudPool pool = myBatchClient.PoolOperations.CreatePool(
                    poolId: "mypool",
                    virtualMachineSize: "standard_d1_v2",
                    cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "5"));    
pool.AutoScaleEnabled = true;
pool.AutoScaleFormula = "$TargetDedicatedNodes = (time().weekday == 1 ? 5:1);";
pool.AutoScaleEvaluationInterval = TimeSpan.FromMinutes(30);
await pool.CommitAsync();
```

> [!IMPORTANT]
> Otomatik ölçek etkin bir havuz oluşturduğunuzda, CreatePool için arama _hedefDedicatedNodes_ parametre veya _hedefLowPriorityNodes_ parametre belirtmeyin. **CreatePool** Bunun yerine, havuzdaki **AutoScaleEnabled** ve **AutoScaleFormula** özelliklerini belirtin. Bu özelliklerin değerleri, her düğüm türünün hedef sayısını belirler. Ayrıca, otomatik ölçekle etkinleştirilen bir havuzu el ile yeniden boyutlandırmak için (örneğin, [BatchClient.PoolOperations.ResizePoolAsync][net_poolops_resizepoolasync]ile), önce havuzda otomatik ölçeklendirmeyi **devre dışı,** sonra yeniden boyutlandırın.
>
>

#### <a name="automatic-scaling-interval"></a>Otomatik ölçekleme aralığı

Varsayılan olarak, Toplu İşlem hizmeti bir havuzun boyutunu her 15 dakikada bir otomatik ölçeklendirme formülüne göre ayarlar. Bu aralık aşağıdaki havuz özellikleri kullanılarak yapılandırılabilir:

* [CloudPool.AutoScaleEvaluationInterval][net_cloudpool_autoscaleevalinterval] (Toplu .NET)
* [autoScaleEvaluationInterval][rest_autoscaleinterval] (REST API)

Minimum aralık beş dakikadır ve en fazla 168 saattir. Bu aralığın dışında bir aralık belirtilirse, Toplu İşlem hizmeti bir Kötü İstek (400) hatası döndürür.

> [!NOTE]
> Otomatik ölçekleme şu anda değişiklikleri bir dakikadan kısa bir sürede yanıtlamak için tasarlanmamıştır, ancak daha çok bir iş yükü çalıştırdığınızda havuzunuzun boyutunu kademeli olarak ayarlamak için tasarlanmıştır.
>
>

### <a name="python"></a>Python

Benzer şekilde, Python SDK ile otomatik ölçeközellikli bir havuz yapabilirsiniz:

1. Bir havuz oluşturun ve yapılandırmasını belirtin.
1. Havuz servis istemcisine ekleyin.
1. Yazdığınız bir formülle havuzda otomatik ölçeklendirmeyi etkinleştirin.

```python
# Create a pool; specify configuration
new_pool = batch.models.PoolAddParameter(
    id="autoscale-enabled-pool",
    virtual_machine_configuration=batchmodels.VirtualMachineConfiguration(
        image_reference=batchmodels.ImageReference(
          publisher="Canonical",
          offer="UbuntuServer",
          sku="18.04-LTS",
          version="latest"
            ),
        node_agent_sku_id="batch.node.ubuntu 18.04"),
    vm_size="STANDARD_D1_v2",
    target_dedicated_nodes=0,
    target_low_priority_nodes=0
)
batch_service_client.pool.add(new_pool) # Add the pool to the service client

formula = """$curTime = time();
             $workHours = $curTime.hour >= 8 && $curTime.hour < 18; 
             $isWeekday = $curTime.weekday >= 1 && $curTime.weekday <= 5; 
             $isWorkingWeekdayHour = $workHours && $isWeekday; 
             $TargetDedicated = $isWorkingWeekdayHour ? 20:10;""";

# Enable autoscale; specify the formula
response = batch_service_client.pool.enable_auto_scale(pool_id, auto_scale_formula=formula,
                                            auto_scale_evaluation_interval=datetime.timedelta(minutes=10), 
                                            pool_enable_auto_scale_options=None, 
                                            custom_headers=None, raw=False)
```

> [!TIP]
> Python SDK'yı kullanmaya ilişkin daha fazla örnek, GitHub'daki [Toplu Python Quickstart deposunda](https://github.com/Azure-Samples/batch-python-quickstart) bulunabilir.
>
>

## <a name="enable-autoscaling-on-an-existing-pool"></a>Varolan bir havuzda otomatik ölçekleme sağlama

Her Toplu İşlem SDK otomatik ölçekleme sağlamak için bir yol sağlar. Örnek:

* [BatchClient.PoolOperations.EnableAutoScaleAsync][net_enableautoscaleasync] (Toplu .NET)
* [Havuzda otomatik ölçeklendirmeyi etkinleştirme][rest_enableautoscale] (REST API)

Varolan bir havuzda otomatik olarak otomatikleştirince, aşağıdaki noktaları aklınızda bulundurun:

* Otomatik ölçeklendirmeyi etkinleştirmek için istek yaptığınızda havuzda şu anda devre dışı bırakılmışsa, isteği verirken geçerli bir otomatik ölçeklendirme formülü belirtmeniz gerekir. İsteğe bağlı olarak bir otomatik ölçek değerlendirme aralığı belirtebilirsiniz. Bir aralık belirtmezseniz, 15 dakikalık varsayılan değer kullanılır.
* Havuzda şu anda otomatik ölçek etkinse, otomatik ölçeklendirme formülü, değerlendirme aralığı veya her ikisini de belirtebilirsiniz. Bu özelliklerden en az birini belirtmeniz gerekir.

  * Yeni bir otomatik ölçek değerlendirme aralığı belirtirseniz, varolan değerlendirme zamanlaması durdurulur ve yeni bir zamanlama başlatılır. Yeni zamanlamanın başlangıç saati, otomatik olarak otomatikleştirilmesini etkinleştirme isteğinin verildiği zamandır.
  * Otomatik ölçeklendirme formülünün veya değerlendirme aralığının atlanırsa, Toplu İşlem hizmeti bu ayarın geçerli değerini kullanmaya devam eder.

> [!NOTE]
> .NET'te havuzu oluşturduğunuzda **CreatePool** yönteminin *hedefDedicatedNodes* veya *targetLowPriorityNodes* parametreleri için değerleri belirtirseniz veya otomatik ölçekleme formülü değerlendirildiğinde bu değerler yoksayılır.
>
>

Bu C# kodu snippet varolan bir havuzda otomatik ölçekleme etkinleştirmek için [Toplu .NET][net_api] kitaplığı kullanır:

```csharp
// Define the autoscaling formula. This formula sets the target number of nodes
// to 5 on Mondays, and 1 on every other day of the week
string myAutoScaleFormula = "$TargetDedicatedNodes = (time().weekday == 1 ? 5:1);";

// Set the autoscale formula on the existing pool
await myBatchClient.PoolOperations.EnableAutoScaleAsync(
    "myexistingpool",
    autoscaleFormula: myAutoScaleFormula);
```

### <a name="update-an-autoscale-formula"></a>Otomatik ölçeklendirme formüllerini güncelleştirme

Formülü varolan otomatik ölçek etkinleştirilmiş bir havuzda güncelleştirmek için, yeni formülle yeniden otomatik ölçeklendirmeyi etkinleştirmek için işlemi arayın. Örneğin, aşağıdaki .NET kodu `myexistingpool` yürütüldüğünde otomatik ölçeklendirme zaten etkinse, otomatik ölçeklendirme formülü `myNewFormula`.

```csharp
await myBatchClient.PoolOperations.EnableAutoScaleAsync(
    "myexistingpool",
    autoscaleFormula: myNewFormula);
```

### <a name="update-the-autoscale-interval"></a>Otomatik ölçek aralığını güncelleştirme

Varolan otomatik ölçek etkinleştirilmiş bir havuzun otomatik ölçeklendirme değerlendirme aralığını güncelleştirmek için, yeni aralıkla yeniden otomatik ölçeklendirmeyi etkinleştirmek için işlemi arayın. Örneğin, .NET'te zaten otomatik ölçek etkinleştirilmiş bir havuz için otomatik ölçek değerlendirme aralığını 60 dakikaolarak ayarlamak için:

```csharp
await myBatchClient.PoolOperations.EnableAutoScaleAsync(
    "myexistingpool",
    autoscaleEvaluationInterval: TimeSpan.FromMinutes(60));
```

## <a name="evaluate-an-autoscale-formula"></a>Otomatik ölçek lendirme formüllerini değerlendirme

Bir formülü havuza uygulamadan önce değerlendirebilirsiniz. Bu şekilde, formülü üretime koymadan önce deyimlerin nasıl değerlendirilebileceğini görmek için formülü sınayabilirsiniz.

Otomatik ölçeklendirme formüllerini değerlendirmek için, önce geçerli bir formülle havuzda otomatik ölçeklendirmeyi etkinleştirmeniz gerekir. Formülü henüz otomatik ölçekleme etkinleştirilmiş olmayan bir havuzda test etmek için, `$TargetDedicatedNodes = 0` otomatik küçültmeyi ilk etkinleştirdiğinizde tek satırlık formülü kullanın. Ardından, test etmek istediğiniz formülü değerlendirmek için aşağıdakilerden birini kullanın:

* [BatchClient.PoolOperations.EvaluateAutoScale](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.pooloperations.evaluateautoscale) veya [EvaluateAutoScaleAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.pooloperations.evaluateautoscaleasync)

    Bu Toplu İşlem .NET yöntemleri, varolan bir havuzun kimliğini ve değerlendirmek için otomatik ölçek formüllerini içeren bir dize gerektirir.

* [Otomatik ölçekleme formüllerini değerlendirme](https://docs.microsoft.com/rest/api/batchservice/evaluate-an-automatic-scaling-formula)

    Bu REST API isteğinde, URI'deki havuz kimliğini ve istek gövdesinin *otomatik Ölçeklendirme formülünde otomatik ölçeklendirme* formülünün belirtin. İşlemin yanıtı, formülle ilgili olabilecek herhangi bir hata bilgisi içerir.

Bu [Toplu .NET][net_api] kod snippet'inde, otomatik ölçeklendirme formüllerini değerlendiririz. Havuzda otomatik ölçekleme etkin değilse, önce biz etkinleştiriz.

```csharp
// First obtain a reference to an existing pool
CloudPool pool = await batchClient.PoolOperations.GetPoolAsync("myExistingPool");

// If autoscaling isn't already enabled on the pool, enable it.
// You can't evaluate an autoscale formula on non-autoscale-enabled pool.
if (pool.AutoScaleEnabled == false)
{
    // We need a valid autoscale formula to enable autoscaling on the
    // pool. This formula is valid, but won't resize the pool:
    await pool.EnableAutoScaleAsync(
        autoscaleFormula: "$TargetDedicatedNodes = $CurrentDedicatedNodes;",
        autoscaleEvaluationInterval: TimeSpan.FromMinutes(5));

    // Batch limits EnableAutoScaleAsync calls to once every 30 seconds.
    // Because we want to apply our new autoscale formula below if it
    // evaluates successfully, and we *just* enabled autoscaling on
    // this pool, we pause here to ensure we pass that threshold.
    Thread.Sleep(TimeSpan.FromSeconds(31));

    // Refresh the properties of the pool so that we've got the
    // latest value for AutoScaleEnabled
    await pool.RefreshAsync();
}

// We must ensure that autoscaling is enabled on the pool prior to
// evaluating a formula
if (pool.AutoScaleEnabled == true)
{
    // The formula to evaluate - adjusts target number of nodes based on
    // day of week and time of day
    string myFormula = @"
        $curTime = time();
        $workHours = $curTime.hour >= 8 && $curTime.hour < 18;
        $isWeekday = $curTime.weekday >= 1 && $curTime.weekday <= 5;
        $isWorkingWeekdayHour = $workHours && $isWeekday;
        $TargetDedicatedNodes = $isWorkingWeekdayHour ? 20:10;
    ";

    // Perform the autoscale formula evaluation. Note that this code does not
    // actually apply the formula to the pool.
    AutoScaleRun eval =
        await batchClient.PoolOperations.EvaluateAutoScaleAsync(pool.Id, myFormula);

    if (eval.Error == null)
    {
        // Evaluation success - print the results of the AutoScaleRun.
        // This will display the values of each variable as evaluated by the
        // autoscale formula.
        Console.WriteLine("AutoScaleRun.Results: " +
            eval.Results.Replace("$", "\n    $"));

        // Apply the formula to the pool since it evaluated successfully
        await batchClient.PoolOperations.EnableAutoScaleAsync(pool.Id, myFormula);
    }
    else
    {
        // Evaluation failed, output the message associated with the error
        Console.WriteLine("AutoScaleRun.Error.Message: " +
            eval.Error.Message);
    }
}
```

Bu kod snippet gösterilen formülün başarılı değerlendirilmesi benzer sonuçlar üretir:

```
AutoScaleRun.Results:
    $TargetDedicatedNodes=10;
    $NodeDeallocationOption=requeue;
    $curTime=2016-10-13T19:18:47.805Z;
    $isWeekday=1;
    $isWorkingWeekdayHour=0;
    $workHours=0
```

## <a name="get-information-about-autoscale-runs"></a>Otomatik ölçeklendirme çalıştırmaları hakkında bilgi alın

Formülünüzün beklendiği gibi çalıştığından emin olmak için, Toplu İşlem'in havuzunuzda gerçekleştirdiği otomatik ölçekleme çalıştırmalarının sonuçlarını düzenli aralıklarla kontrol etmenizi öneririz. Bunu yapmak için havuza bir başvuru alın (veya yenileyin) ve son otomatik ölçeklendirme çalışmasının özelliklerini inceleyin.

Batch .NET'te [CloudPool.AutoScaleRun](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.autoscalerun) özelliği, havuzda gerçekleştirilen en son otomatik ölçeklendirme çalışması hakkında bilgi sağlayan çeşitli özelliklere sahiptir:

* [AutoScaleRun.Timestamp](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.autoscalerun.timestamp)
* [AutoScaleRun.Sonuçlar](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.autoscalerun.results)
* [AutoScaleRun.Hatası](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.autoscalerun.error)

REST API'de, bir havuz isteği [hakkında bilgi alın,](https://docs.microsoft.com/rest/api/batchservice/get-information-about-a-pool) [otomatik Ölçekleme](https://docs.microsoft.com/rest/api/batchservice/get-information-about-a-pool) özelliğindeki en son otomatik ölçeklendirme çalıştırma bilgilerini içeren havuz hakkında bilgi verir.

Aşağıdaki C# kodu snippet pool _myPool_üzerinde son otomatik leme çalışması hakkında bilgi yazdırmak için Toplu .NET kitaplığını kullanır:

```csharp
await Cloud pool = myBatchClient.PoolOperations.GetPoolAsync("myPool");
Console.WriteLine("Last execution: " + pool.AutoScaleRun.Timestamp);
Console.WriteLine("Result:" + pool.AutoScaleRun.Results.Replace("$", "\n  $"));
Console.WriteLine("Error: " + pool.AutoScaleRun.Error);
```

Bir önceki parçacığın örnek çıktısı:

```
Last execution: 10/14/2016 18:36:43
Result:
  $TargetDedicatedNodes=10;
  $NodeDeallocationOption=requeue;
  $curTime=2016-10-14T18:36:43.282Z;
  $isWeekday=1;
  $isWorkingWeekdayHour=0;
  $workHours=0
Error:
```

## <a name="example-autoscale-formulas"></a>Örnek otomatik ölçek formülleri

Havuzdaki işlem kaynaklarının miktarını ayarlamak için farklı yollar gösteren birkaç formüle bakalım.

### <a name="example-1-time-based-adjustment"></a>Örnek 1: Zamana dayalı ayarlama

Havuz boyutunu haftanın gününe ve günün saatine göre ayarlamak istediğinizi varsayalım. Bu örnek, havuzdaki düğüm sayısını buna göre nasıl artıracağınız veya azaltacağını gösterir.

Formül önce geçerli saati elde eder. Hafta içi (1-5) ve çalışma saatleri içinde (08:00-18:00) ise, hedef havuz boyutu 20 düğüm olarak ayarlanır. Aksi takdirde, 10 düğüm olarak ayarlanır.

```
$curTime = time();
$workHours = $curTime.hour >= 8 && $curTime.hour < 18;
$isWeekday = $curTime.weekday >= 1 && $curTime.weekday <= 5;
$isWorkingWeekdayHour = $workHours && $isWeekday;
$TargetDedicatedNodes = $isWorkingWeekdayHour ? 20:10;
$NodeDeallocationOption = taskcompletion;
```
`$curTime`ürününe `TimeZoneInterval_Hour` ve UTC mahsupunuza `time()` ekleyerek yerel saat diliminizi yansıtacak şekilde ayarlanabilir. Örneğin, Dağ `$curTime = time() + (-6 * TimeInterval_Hour);` Gün Işığı Saati (MDT) için kullanın. Ofsetin gün ışığından yararlanma saatinin başlangıcında ve sonunda (varsa) ayarlanması gerektiğini unutmayın.

### <a name="example-2-task-based-adjustment"></a>Örnek 2: Görev tabanlı ayarlama

Bu örnekte, havuz boyutu kuyruktaki görev sayısına göre ayarlanır. Formül dizelerinde hem açıklamalar hem de satır sonları kabul edilebilir.

```csharp
// Get pending tasks for the past 15 minutes.
$samples = $PendingTasks.GetSamplePercent(TimeInterval_Minute * 15);
// If we have fewer than 70 percent data points, we use the last sample point,
// otherwise we use the maximum of last sample point and the history average.
$tasks = $samples < 70 ? max(0,$PendingTasks.GetSample(1)) : max( $PendingTasks.GetSample(1), avg($PendingTasks.GetSample(TimeInterval_Minute * 15)));
// If number of pending tasks is not 0, set targetVM to pending tasks, otherwise
// half of current dedicated.
$targetVMs = $tasks > 0? $tasks:max(0, $TargetDedicatedNodes/2);
// The pool size is capped at 20, if target VM value is more than that, set it
// to 20. This value should be adjusted according to your use case.
$TargetDedicatedNodes = max(0, min($targetVMs, 20));
// Set node deallocation mode - let running tasks finish before removing a node
$NodeDeallocationOption = taskcompletion;
```

### <a name="example-3-accounting-for-parallel-tasks"></a>Örnek 3: Paralel görevlerin muhasebeleştirilmesi

Bu örnek, görev sayısına göre havuz boyutunu ayarlar. Bu formül, havuz için ayarlanan [MaxTasksPerComputeNode][net_maxtasks] değerini de dikkate alır. Bu yaklaşım, havuzunuzda [paralel görev yürütmeetkin](batch-parallel-node-tasks.md) olduğu durumlarda yararlıdır.

```csharp
// Determine whether 70 percent of the samples have been recorded in the past
// 15 minutes; if not, use last sample
$samples = $ActiveTasks.GetSamplePercent(TimeInterval_Minute * 15);
$tasks = $samples < 70 ? max(0,$ActiveTasks.GetSample(1)) : max( $ActiveTasks.GetSample(1),avg($ActiveTasks.GetSample(TimeInterval_Minute * 15)));
// Set the number of nodes to add to one-fourth the number of active tasks (the
// MaxTasksPerComputeNode property on this pool is set to 4, adjust this number
// for your use case)
$cores = $TargetDedicatedNodes * 4;
$extraVMs = (($tasks - $cores) + 3) / 4;
$targetVMs = ($TargetDedicatedNodes + $extraVMs);
// Attempt to grow the number of compute nodes to match the number of active
// tasks, with a maximum of 3
$TargetDedicatedNodes = max(0,min($targetVMs,3));
// Keep the nodes active until the tasks finish
$NodeDeallocationOption = taskcompletion;
```

### <a name="example-4-setting-an-initial-pool-size"></a>Örnek 4: Başlangıç havuzu boyutunu ayarlama

Bu örnek, havuz boyutunu başlangıç dönemi için belirli bir düğüm sayısına ayarlayan otomatik ölçek formülüne sahip bir C# kodu snippet'i gösterir. Daha sonra, ilk zaman dilimi geçtikten sonra çalışan ve etkin görevlerin sayısına göre havuz boyutunu ayarlar.

Aşağıdaki kod parçacığındaki formül:

* İlk havuz boyutunu dört düğümolarak ayarlar.
* Havuzun kullanım döngüsünün ilk 10 dakikası içinde havuz boyutunu ayarlamaz.
* 10 dakika sonra, son 60 dakika içinde çalışan ve etkin görev sayısının maksimum değerini elde eder.
  * Her iki değer de 0 ise (son 60 dakika içinde hiçbir görevin çalışmadığını veya etkin olmadığını gösterir), havuz boyutu 0 olarak ayarlanır.
  * Her iki değer de sıfırdan büyükse, değişiklik yapılmaz.

```csharp
string now = DateTime.UtcNow.ToString("r");
string formula = string.Format(@"
    $TargetDedicatedNodes = {1};
    lifespan         = time() - time(""{0}"");
    span             = TimeInterval_Minute * 60;
    startup          = TimeInterval_Minute * 10;
    ratio            = 50;

    $TargetDedicatedNodes = (lifespan > startup ? (max($RunningTasks.GetSample(span, ratio), $ActiveTasks.GetSample(span, ratio)) == 0 ? 0 : $TargetDedicatedNodes) : {1});
    ", now, 4);
```

## <a name="next-steps"></a>Sonraki adımlar

* [Eşzamanlı düğüm görevleriyle Azure Toplu İşlem kaynak kullanımını en üst düzeye](batch-parallel-node-tasks.md) çıkarın, havuzunuzdaki bilgi işlem düğümlerinde aynı anda birden çok görevi nasıl yürütebileceğinizhakkında ayrıntılar içerir. Otomatik ölçekleme nin yanı sıra, bu özellik bazı iş yükleri için iş süresini düşürmenize yardımcı olarak paradan tasarruf edebilirsiniz.
* Başka bir verimlilik artırıcı için, Toplu İşlem uygulamanızın Toplu İşlem hizmetini en uygun şekilde sorguladığından emin olun. Binlerce işlem düğümü veya görevin durumunu sorgularken telden geçen veri miktarını nasıl sınırlandırabileceğinizi öğrenmek için [Azure Toplu İşlem hizmetini sorgulayın'a](batch-efficient-list-queries.md) bakın.

[net_api]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch
[net_batchclient]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.batchclient
[net_cloudpool_autoscaleformula]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.autoscaleformula
[net_cloudpool_autoscaleevalinterval]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.autoscaleevaluationinterval
[net_enableautoscaleasync]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.pooloperations.enableautoscaleasync
[net_maxtasks]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.maxtaskspercomputenode
[net_poolops_resizepoolasync]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.pooloperations.resizepoolasync

[rest_api]: https://docs.microsoft.com/rest/api/batchservice/
[rest_autoscaleformula]: https://docs.microsoft.com/rest/api/batchservice/enable-automatic-scaling-on-a-pool
[rest_autoscaleinterval]: https://docs.microsoft.com/rest/api/batchservice/enable-automatic-scaling-on-a-pool
[rest_enableautoscale]: https://docs.microsoft.com/rest/api/batchservice/enable-automatic-scaling-on-a-pool
