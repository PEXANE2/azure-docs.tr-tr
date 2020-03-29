---
title: Bulut hizmeti nasıl güncellenir | Microsoft Dokümanlar
description: Azure'da bulut hizmetlerini nasıl güncelleştirin öğrenin. Kullanılabilirliği sağlamak için bulut hizmetiyle ilgili bir güncelleştirmenin nasıl ilerlediğini öğrenin.
services: cloud-services
author: tgore03
ms.service: cloud-services
ms.topic: article
ms.date: 04/19/2017
ms.author: tagore
ms.openlocfilehash: 731f4e8cc8a93f33d6887f44fc8d09585e92a75a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75360353"
---
# <a name="how-to-update-a-cloud-service"></a>Bulut hizmetini güncelleştirme

Hem rolleri hem de konuk işletim sistemi de dahil olmak üzere bir bulut hizmetini güncelleştirmek üç adımlı bir işlemdir. İlk olarak, yeni bulut hizmeti veya işletim sistemi sürümü için ikili dosyalar ve yapılandırma dosyaları yüklenmelidir. Ardından, Azure, bulut hizmeti için yeni bulut hizmeti sürümünün gereksinimlerine bağlı olarak hesaplama ve ağ kaynaklarını ayırır. Son olarak Azure, kullanılabilirliğinizi korurken kiracıyı yeni sürüme veya konuk işletim sistemiyle kademeli olarak güncelleştirmek için bir yükseltme gerçekleştirir. Bu makalede, bu son adımın ayrıntıları tartışılır - haddeleme yükseltme.

## <a name="update-an-azure-service"></a>Azure Hizmetini Güncelleştirme
Azure, rol örneklerinizi yükseltme etki alanları (UD) adı verilen mantıksal gruplandırmalar halinde düzenler. Yükseltme etki alanları (UD), grup olarak güncelleştirilen mantıksal rol örnekleri kümeleridir.  Azure, bulut hizmetini her seferinde bir UD olarak güncelleştirir ve bu da diğer UD'lerde bulunan örneklerin trafiğe hizmet vermeye devam etmesine olanak tanır.

Yükseltme etki alanlarının varsayılan sayısı 5'tir. Hizmetin tanım dosyasına (.csdef) yükseltmeAlanSayısı özniteliğini ekleyerek farklı sayıda yükseltme etki alanı belirtebilirsiniz. YükseltmeDomainCount özniteliği hakkında daha fazla bilgi için [Azure Bulut Hizmetleri Tanımı Şeması (.csdef Dosyası)](https://docs.microsoft.com/azure/cloud-services/schema-csdef-file)konusuna bakın.

Hizmetinizde bir veya daha fazla rolün yerinde güncelleştirmesini gerçekleştirdiğinizde, Azure rol örneklerini ait oldukları yükseltme etki alanına göre ayarlar. Azure, belirli bir yükseltme etki alanında tüm örnekleri güncelleştirir ( onları durdurmak, güncellemek, tekrar çevrimiçi hale getirmek- sonraki etki alanına geçer. Azure, yalnızca geçerli yükseltme etki alanında çalışan örnekleri durdurarak, bir güncelleştirmenin çalışan hizmet üzerinde mümkün olan en az etkiye sahip olmasını sağlar. Daha fazla bilgi için güncelleştirmenin bu makalede nasıl [devam ettiğine](#howanupgradeproceeds) bakın.

> [!NOTE]
> Terimler **güncelleştirme** ve **yükseltme** bağlamında Azure'da biraz farklı bir anlam ifade etse de, bu belgedeki özelliklerin işlemleri ve açıklamaları için birbirinin yerine kullanılabilir.
>
>

Hizmetinizin, bu rolün kapalı kalma süresi olmadan yerinde güncelleştirilemesi için rolün en az iki örneğini tanımlaması gerekir. Hizmet yalnızca bir rol örneğinden oluşuyorsa, yerinde güncelleştirme tamamlanana kadar hizmetiniz kullanılamaz.

Bu konu, Azure güncelleştirmeleri hakkında aşağıdaki bilgileri kapsar:

* [Güncelleştirme sırasında izin verilen hizmet değişiklikleri](#AllowedChanges)
* [Yükseltme nasıl devam eder?](#howanupgradeproceeds)
* [Güncelleştirmenin geri alma](#RollbackofanUpdate)
* [Devam eden bir dağıtımda birden çok mutasyona uğrama işlemi başlatma](#multiplemutatingoperations)
* [Rollerin yükseltme etki alanları arasında dağılımı](#distributiondfroles)

<a name="AllowedChanges"></a>

## <a name="allowed-service-changes-during-an-update"></a>Güncelleştirme sırasında izin verilen hizmet değişiklikleri
Aşağıdaki tablo, güncelleştirme sırasında bir hizmette izin verilen değişiklikleri gösterir:

| Barındırma, hizmetler ve rollere izin verilen değişiklikler | Yerinde güncelleştirme | Sahnelenen (VIP takas) | Silme ve yeniden dağıtma |
| --- | --- | --- | --- |
| İşletim sistemi sürümü |Evet |Evet |Evet |
| .NET güven düzeyi |Evet |Evet |Evet |
| Sanal makine boyutu<sup>1</sup> |Evet<sup>2</sup> |Evet |Evet |
| Yerel depolama ayarları |Yalnızca<sup>2'yi</sup> artırın |Evet |Evet |
| Bir hizmetteki rolleri ekleme veya kaldırma |Evet |Evet |Evet |
| Belirli bir rolün örneklerinin sayısı |Evet |Evet |Evet |
| Bir hizmetiçin uç nokta sayısı veya türü |Evet<sup>2</sup> |Hayır |Evet |
| Yapılandırma ayarlarının adları ve değerleri |Evet |Evet |Evet |
| Yapılandırma ayarlarının değerleri (ancak adları değil) |Evet |Evet |Evet |
| Yeni sertifikalar ekleme |Evet |Evet |Evet |
| Varolan sertifikaları değiştirme |Evet |Evet |Evet |
| Yeni kod dağıtma |Evet |Evet |Evet |

<sup>1</sup> Bulut hizmeti için kullanılabilen boyut alt kümesiyle sınırlı boyut değişikliği.

<sup>2</sup> Azure SDK 1.5 veya sonraki sürümleri gerektirir.

> [!WARNING]
> Sanal makine boyutunudeğiştirmek yerel verileri yok eder.
>
>

Güncelleştirme sırasında aşağıdaki öğeler desteklenmez:

* Bir rolün adını değiştirmek. Yeni adla rolü kaldırın ve ekleyin.
* Yükseltme Etki Alanı sayısının değiştirilmesi.
* Yerel kaynakların boyutunu azaltma.

Yerel kaynağın boyutunu küçültmek gibi hizmettanımınızda başka güncelleştirmeler yapıyorsanız, bunun yerine bir VIP takas güncelleştirmesi gerçekleştirmeniz gerekir. Daha fazla bilgi için [Swap Dağıtım'a](/previous-versions/azure/reference/ee460814(v=azure.100))bakın.

<a name="howanupgradeproceeds"></a>

## <a name="how-an-upgrade-proceeds"></a>Yükseltme nasıl devam eder?
Hizmetinizdeki tüm rolleri mi yoksa hizmetteki tek bir rolü güncelleştirmek mi istediğinize karar verebilirsiniz. Her iki durumda da, yükseltilmekte olan ve ilk yükseltme alanına ait olan her rolün tüm örnekleri durdurulur, yükseltilir ve çevrimiçi olarak geri getirilir. Çevrimiçi olduktan sonra, ikinci yükseltme etki alanındaki örnekler durdurulur, yükseltilir ve tekrar çevrimiçi duruma getirilir. Bir bulut hizmeti, aynı anda en fazla bir yükseltme etkin olabilir. Yükseltme her zaman bulut hizmetinin en son sürümüne karşı gerçekleştirilir.

Aşağıdaki diyagram, hizmetteki tüm rolleri yükseltiyorsanız yükseltmenin nasıl devam ettiğini gösterir:

![Yükseltme hizmeti](media/cloud-services-update-azure-service/IC345879.png "Yükseltme hizmeti")

Bir sonraki diyagram, yalnızca tek bir rolü yükseltiyorsanız güncelleştirmenin nasıl devam ettiğini gösterir:

![Yükseltme rolü](media/cloud-services-update-azure-service/IC345880.png "Yükseltme rolü")  

Otomatik güncelleştirme sırasında Azure Kumaş Denetleyicisi, bir sonraki UD'de ne zaman güvenli olduğunu belirlemek için bulut hizmetinin durumunu düzenli aralıklarla değerlendirir. Bu sistem durumu değerlendirmesi her rol bazında gerçekleştirilir ve yalnızca en son sürümdeki örnekleri (örneğin, zaten yürütülmüş olan UD'lerden örnekler) dikkate alır. Her rol için en az sayıda rol örneği tatmin edici bir terminal durumu elde ettiğini doğrular.

### <a name="role-instance-start-timeout"></a>Rol Örneği Başlangıç Zaman Azamanı
Kumaş Denetleyicisi, her rol örneğinin Başlangıç durumuna ulaşması için 30 dakika bekler. Zaman aşımı süresi dolursa, Kumaş Denetleyicisi bir sonraki rol örneğine yürümeye devam eder.

### <a name="impact-to-drive-data-during-cloud-service-upgrades"></a>Bulut Hizmeti yükseltmeleri sırasında verileri yönlendirme etkisi

Bir hizmeti tek bir örnekten birden çok örne yükseltirken, yükseltme Azure yükseltme hizmetleri nedeniyle gerçekleştirilirken hizmetiniz indirilir. Hizmet kullanılabilirliğini garanti eden hizmet düzeyi sözleşmesi yalnızca birden fazla örnekle dağıtılan hizmetler için geçerlidir. Aşağıdaki liste, her sürücüdeki verilerin her Azure hizmet yükseltme senaryosundan nasıl etkilendiğini açıklar:

|Senaryo|C Sürücü|D Sürücü|E Sürücü|
|--------|-------|-------|-------|
|VM yeniden başlatma|Korunmuş|Korunmuş|Korunmuş|
|Portal yeniden başlatma|Korunmuş|Korunmuş|Yok|
|Portal reimage|Korunmuş|Yok|Yok|
|Yerinde Yükseltme|Korunmuş|Korunmuş|Yok|
|Düğüm geçişi|Yok|Yok|Yok|

Yukarıdaki listede, E: sürücü rolün kök sürücüsünu temsil eder ve sabit kodlanmamalıdır. Bunun yerine, sürücüyü temsil etmek için **%RoleRoot%** ortamı değişkenini kullanın.

Tek örnekli bir hizmeti yükseltirken kapalı kalma süresini en aza indirmek için, evreleme sunucusuna yeni bir çok örnekli hizmet dağıtın ve VIP değiştirme gerçekleştirin.

<a name="RollbackofanUpdate"></a>

## <a name="rollback-of-an-update"></a>Güncelleştirmenin geri alma
Azure, ilk güncelleştirme isteği Azure Kumaş Denetleyicisi tarafından kabul edildikten sonra bir hizmette ek işlemler başlatmanıza izin vererek bir güncelleştirme sırasında hizmetleri yönetmede esneklik sağlar. Geri alma işlemi yalnızca dağıtımda devam **eden** durum sırasında bir güncelleştirme (yapılandırma değişikliği) veya yükseltme olduğunda gerçekleştirilebilir. Hizmetin henüz yeni sürüme güncelleştirilmemiş en az bir örneği olduğu sürece, güncelleştirme veya yükseltme devam ediyor olarak kabul edilir. Geri alma işlemlerine izin verilip verilmediğini sınamak için, Dağıtımı Al ve Bulut [Hizmeti Özelliklerini](/previous-versions/azure/reference/ee460806(v=azure.100)) [Al](/previous-versions/azure/reference/ee460804(v=azure.100)) işlemleri tarafından döndürülen RollbackAllowed bayrağının değerini kontrol edin, doğru ayarlanır.

> [!NOTE]
> VIP takas yükseltmeleri hizmetinizin tüm çalışan örneğini başka bir şeyle değiştirmeyi içerdiğinden, rollback'i **yerinde** bir güncelleme veya yükseltmeyle aramak mantıklıdır.
>
>

Devam eden bir güncelleştirmenin geri alma geri alma işlemi, dağıtım üzerinde aşağıdaki etkilere sahiptir:

* Henüz güncelleştirilemeyen veya yeni sürüme yükseltilemeyen rol örnekleri güncelleştirilmez veya yükseltilmez, çünkü bu örnekler zaten hizmetin hedef sürümünü çalıştırMaktadır.
* Hizmet paketinin (.cspkg)\*dosyasının veya hizmet yapılandırmasının (.cscfg)\*dosyasının (veya her iki dosyanın) yeni sürümüne zaten güncelleştirilen veya yükseltilmiş olan tüm rol örnekleri bu dosyaların ön yükseltme sürümüne döndürülür.

Bu işlevsel olarak aşağıdaki özellikler tarafından sağlanır:

* Hizmette henüz yeni sürüme güncelleştirilmemiş en az bir örnek olduğu sürece, bir yapılandırma güncelleştirmesi [(Dağıtım Yapılandırmasını Değiştir'i](/previous-versions/azure/reference/ee460809(v=azure.100))çağırarak tetiklenen) veya yükseltme [(Yükseltme Dağıtımı](/previous-versions/azure/reference/ee460793(v=azure.100))çağırılarak tetiklenen) çağrılabilir [Rollback Update veya Yükseltme](/previous-versions/azure/reference/hh403977(v=azure.100)) işlemi.
* Dağıtımı Al ve [Bulut Hizmeti Özelliklerini](/previous-versions/azure/reference/ee460806(v=azure.100)) [Al](/previous-versions/azure/reference/ee460804(v=azure.100)) işlemlerinin yanıt gövdesinin bir parçası olarak döndürülen Kilitli öğe ve RollbackAllowed öğesi:

  1. Kilitli öğe, belirli bir dağıtımda bir mutasyon işleminin ne zaman çağrılabileceğini algılamanızı sağlar.
  2. RollbackAllowed öğesi, [Rollback Update veya Yükseltme](/previous-versions/azure/reference/hh403977(v=azure.100)) işleminin belirli bir dağıtımda ne zaman çağrılabileceğini algılamanızı sağlar.

  Geri alma gerçekleştirmek için hem Kilitli hem de RollbackAllowed öğelerini denetlemeniz gerekmez. Bu RollbackAllowed doğru ayarlanmış olduğunu onaylamak için yeterlidir. Bu öğeler yalnızca istek üstbilgikümesi "x-ms sürümü: 2011-10-01" veya daha sonraki bir sürüm kullanılarak çağrılırsa döndürülür. Sürüm üstbilgileri hakkında daha fazla bilgi için [Service Management Versioning'e](/previous-versions/azure/gg592580(v=azure.100))bakın.

Güncelleştirme veya yükseltmenin geri alma nın desteklenmediğini belirten bazı durumlar şunlardır:

* Yerel kaynaklarda azalma - Güncelleştirme, Azure platformunun geri alınmasına izin vermeyen bir rol için yerel kaynakları artırırsa.
* Kota sınırlamaları - Güncelleştirme bir ölçek küçültme işlemiyse, geri alma işlemini tamamlamak için artık yeterli bilgi işlem kotasına sahip olmayabilirsiniz. Her Azure aboneliğinin, bu aboneye ait tüm barındırılan hizmetler tarafından tüketilebilen maksimum çekirdek sayısını belirten bir kotası vardır. Belirli bir güncelleştirmenin geri alınması, aboneliğinizi kotanın üzerine koyarsa, geri alma etkinleştirilmez.
* Yarış koşulu - İlk güncelleştirme tamamlandıysa, geri alma mümkün değildir.

Bir güncelleştirmenin geri alınmasının ne zaman yararlı olabileceğine bir örnek, Azure barındırılan hizmetinize büyük bir yerinde yükseltme nin gerçekleşme hızını denetlemek için El ile modda [Yükseltme Dağıtım](/previous-versions/azure/reference/ee460793(v=azure.100)) işlemini kullanıyorsanız.

Yükseltmenin kullanıma sunulması sırasında [Yükseltme Dağıtım'ı](/previous-versions/azure/reference/ee460793(v=azure.100)) manuel modda çağırır sınız ve yükseltme etki alanlarında yürümeye başlarsınız. Bir noktada, yükseltmeyi izlerken, incelediğiniz ilk yükseltme etki alanlarındaki bazı rol örneklerinin yanıt vermediğini fark ederseniz, dağıtımda [Rollback Update veya Yükseltme](/previous-versions/azure/reference/hh403977(v=azure.100)) işlemini arayabilirsiniz, bu da henüz yükseltilmemiş örnekleri ve önceki hizmet paketi ve yapılandırmasına yükseltilmiş örnekleri geri alma işlemine dokunulmaz bırakacaktır.

<a name="multiplemutatingoperations"></a>

## <a name="initiating-multiple-mutating-operations-on-an-ongoing-deployment"></a>Devam eden bir dağıtımda birden çok mutasyona uğrama işlemi başlatma
Bazı durumlarda, devam eden bir dağıtımda birden çok eşzamanlı mutasyon işlemleri başlatmak isteyebilirsiniz. Örneğin, bir hizmet güncelleştirmesi gerçekleştirebilirsiniz ve bu güncelleştirme hizmetinizde kullanıma sunulurken, güncelleştirmeyi geri almak, farklı bir güncelleştirme uygulamak ve dağıtımı silmek gibi bazı değişiklikler yapmak isteyebilirsiniz. Bunun gerekli olabileceği bir durum, bir hizmet yükseltmesi, yükseltilmiş bir rol örneğinin tekrar tekrar çökmesine neden olan buggy kodu içeriyorsa. Bu durumda, yükseltilmiş etki alanında yetersiz sayıda örnek sağlıklı olduğundan, Azure Kumaş Denetleyicisi bu yükseltmeyi uygularken ilerleme kaydedemeyecektir. Bu durum *sıkışmış dağıtım*olarak adlandırılır. Güncelleştirmeyi geri alarak veya başarısız olanın üstüne yeni bir güncelleştirme uygulayarak dağıtımı geri alabilirsiniz.

Hizmeti güncelleştirmek veya yükseltmek için ilk istek Azure Kumaş Denetleyicisi tarafından alındıktan sonra, sonraki mutasyon işlemlerine başlayabilirsiniz. Diğer bir tarihte, başka bir mutasyon işlemine başlamadan önce ilk işlemin tamamlanmasını beklemeniz gerekmez.

İlk güncelleştirme devam ederken ikinci bir güncelleştirme işlemi başlatılması, geri alma işlemine benzer bir performans sergiler. İkinci güncelleştirme otomatik moddaysa, ilk yükseltme etki alanı hemen yükseltilir ve bu da büyük olasılıkla birden çok yükseltme etki alanından aynı anda çevrimdışı olma örneklerine yol açacaktır.

Mutasyon işlemleri aşağıdaki gibidir: [Dağıtım Yapılandırmasını Değiştir,](/previous-versions/azure/reference/ee460809(v=azure.100)) [Dağıtım Dağıtımını Yükseltme,](/previous-versions/azure/reference/ee460793(v=azure.100)) [Dağıtım Durumunu Güncelleştir,](/previous-versions/azure/reference/ee460808(v=azure.100)) [Dağıtımı Sil](/previous-versions/azure/reference/ee460815(v=azure.100))ve Geri Alma [Güncelleştirme si veya Yükseltme](/previous-versions/azure/reference/hh403977(v=azure.100)).

İki işlem, Dağıtım al ve [Bulut Hizmeti Özelliklerini](/previous-versions/azure/reference/ee460806(v=azure.100)) [Al](/previous-versions/azure/reference/ee460804(v=azure.100)) , belirli bir dağıtımda mutasyona uğratılan bir işlemin çağrılup çağrılamayacağını belirlemek için incelenebilen Kilitli bayrağı döndürün.

Kilitli bayrağı döndüren bu yöntemlerin sürümünü aramak için istek üstbilgisini "x-ms-version: 2011-10-01" veya daha sonra olarak ayarlamanız gerekir. Sürüm üstbilgileri hakkında daha fazla bilgi için [Service Management Versioning'e](/previous-versions/azure/gg592580(v=azure.100))bakın.

<a name="distributiondfroles"></a>

## <a name="distribution-of-roles-across-upgrade-domains"></a>Rollerin yükseltme etki alanları arasında dağılımı
Azure, bir rolün örneklerini, hizmet tanımı (.csdef) dosyasının bir parçası olarak yapılandırılabilen belirli sayıda yükseltme etki alanına eşit olarak dağıtır. Yükseltme etki alanlarının maksimum sayısı 20'dir ve varsayılan değer 5'tir. Hizmet tanımı dosyasının nasıl değiştirilen hakkında daha fazla bilgi için [Azure Hizmet Tanımı Şeması 'na (.csdef Dosyası)](cloud-services-model-and-package.md#csdef)bakın.

Örneğin, rolünüzde on örnek varsa, varsayılan olarak her yükseltme etki alanı iki örnek içerir. Rolünüzde 14 örnek varsa, yükseltme etki alanlarından dördü üç örnek içerir ve beşinci bir etki alanı iki örnek içerir.

Yükseltme etki alanları sıfır tabanlı bir dizinle tanımlanır: ilk yükseltme etki alanı 0 kimliğine sahiptir ve ikinci yükseltme etki alanı 1 ve benzeri bir kimliği vardır.

Aşağıdaki diyagram, iki rol içeren bir hizmetin iki yükseltme etki alanı tanımladığında nasıl dağıtıldığını gösterir. Hizmet, web rolünün sekiz örneğini ve işçi rolünün dokuz örneğini çalıştırıyor.

![Yükseltme Etki Alanlarının Dağıtımı](media/cloud-services-update-azure-service/IC345533.png "Yükseltme Etki Alanlarının Dağıtımı")

> [!NOTE]
> Azure'un, yükseltme etki alanlarında örneklerin nasıl tahsis edildiğini denetlediğini unutmayın. Hangi örneklerin hangi etki alanına tahsis edildiğini belirtmek mümkün değildir.
>
>

## <a name="next-steps"></a>Sonraki adımlar
[Bulut Hizmetleri Nasıl Yönetilir?](cloud-services-how-to-manage-portal.md)  
[Bulut Hizmetleri Nasıl İzlenir?](cloud-services-how-to-monitor.md)  
[Cloud Services’ı Yapılandırma](cloud-services-how-to-configure-portal.md)  



