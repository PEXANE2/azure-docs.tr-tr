---
title: StorSimple yerel olarak sabitlenmiş hacimler SSS| Microsoft Dokümanlar
description: StorSimple yerel olarak sabitlenmiş ciltler hakkında sık sorulan soruların yanıtlarını sağlar.
services: storsimple
documentationcenter: NA
author: manuaery
manager: syadav
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/26/2017
ms.author: manuaery
ms.openlocfilehash: aa69d8b07d31b5cf0386e34c113475cbf4191891
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60319556"
---
# <a name="storsimple-locally-pinned-volumes-frequently-asked-questions-faq"></a>StorSimple yerel olarak sabitlenmiş ciltler: sık sorulan sorular (SSS)
## <a name="overview"></a>Genel Bakış
Aşağıda, StorSimple yerel olarak sabitlenmiş bir birim oluşturduğunuzda, katmanlı bir birimi yerel olarak sabitlenmiş bir topluya dönüştürdüğünüzde (veya tam tersi) veya yerel olarak sabitlenmiş bir birimi yedekleyip geri yüklediğinizde sahip olabileceğiniz sorular ve yanıtlar ve yanıtlar vecevaplarınız verebilirsiniz.

Sorular ve cevaplar aşağıdaki kategorilerde düzenlenir

* Yerel olarak sabitlenmiş bir birim oluşturma
* Yerel olarak sabitlenmiş bir yedekleme
* Katmanlı bir birimi yerel olarak sabitlenmiş bir birime dönüştürme
* Yerel olarak sabitlenmiş bir ses düzeyini geri alma
* Yerel olarak sabitlenmiş bir birim üzerinde başarısız

## <a name="questions-about-creating-a-locally-pinned-volume"></a>Yerel olarak sabitlenmiş bir birim oluşturma yla ilgili sorular
**S.** 8000 serisi cihazlarda oluşturabileceğim yerel olarak sabitlenmiş bir birimin maksimum boyutu nedir?

**A** A StorSimple 8000 Serisi Güncelleme 3.0 çalıştıran cihazlarda, 8100 aygıtında yerel olarak sabitlenmiş hacimleri 8,5 TB'a kadar veya katmanlı hacimlerde 200 TB'a kadar sağlayabilirsiniz. Daha büyük olan 8600 cihazında 22,5 TB'a kadar yerel olarak sabitlenmiş birim ya da 500 TB'a kadar katmanlı birim sağlayabilirsiniz.

**S.** Geçenlerde Güncelleme 3.0 benim 8100 cihaz yükseltilmiş ve yerel olarak sabitlenmiş bir hacim oluşturmaya çalıştığınızda, maksimum kullanılabilir boyutu sadece 6 TB değil, 8,5 TB olduğunu. Neden 8,5 TB hacim oluşturamıyorum?

**A** Cihazınız güncelleştirme 3.0 çalıştırıyorsa, 8100 aygıtında yerel olarak sabitlenmiş 8,5 TB VEYA katmanlı hacimler sağlayabilirsiniz. Cihazınızzaten katmanlı birimlere sahipse, yerel olarak sabitlenmiş bir birim oluşturmak için kullanılabilen alan bu maksimum sınırdan orantılı olarak daha düşük olacaktır. Örneğin, 8100 aygıtınızda yaklaşık 106 TB katmanlı birim sağlanmışsa (katmanlı kapasitenin yarısıdır), 8100 aygıtında oluşturabileceğiniz yerel bir birimin maksimum boyutu buna karşılık 4 TB'ye düşürülür (kabaca maksimum yerel olarak sabitlenmiş hacim kapasitesinin yarısı).

Aygıttaki bazı yerel alan katmanlı birimlerin çalışma kümesini barındırmak için kullanıldığından, aygıt katmanlı birimlere sahipse, yerel olarak sabitlenmiş bir birim oluşturmak için kullanılabilir alan azalır. Tersine, yerel olarak sabitlenmiş bir birim oluşturmak, katmanlı birimler için kullanılabilir alanı orantılı olarak azaltır. Aşağıdaki tablolar, yerel olarak sabitlenmiş birimler oluşturulduğunda 8100 ve 8600 aygıtlarında kullanılabilir katmanlı kapasiteyi özetler.

#### <a name="update-30"></a>Güncelleme 3.0 

| Yerel olarak sabitlenmiş hacimler sağlanan kapasite | Katmanlı hacimler için sağlanacak kullanılabilir kapasite - 8100 | Katmanlı hacimler için sağlanacak kullanılabilir kapasite - 8600 |
| --- | --- | --- |
| 0 |200 TB |500 TB |
| 1 TB |176,5 TB |477.8 TB |
| 4 TB |105.9 TB |411.1 TB |
| 8.5 TB |0 TB |311.1 TB |
| 10 TB |NA |277.8 TB |
| 15 TB |NA |166.7 TB |
| 22,5 TB |NA |0 TB |

**S.** Neden yerel olarak sabitlenmiş ses oluşturma uzun süren bir işlemdir?

**A.** Yerel olarak sabitlenmiş hacimler kalın olarak karşılanır. Aygıtın yerel katmanlarında alan oluşturmak için, sağlama işlemi sırasında varolan katmanlı birimlerden bazı veriler buluta itilebilir. Bu, sağlanan birimin boyutuna, cihazınızdaki varolan verilere ve buluta sunulan bant genişliğine bağlı olduğundan, yerel bir birim oluşturmak için geçen süre birkaç saat olabilir.

**S.** Yerel olarak sabitlenmiş bir birim oluşturmak ne kadar sürer?

**A.** Yerel olarak sabitlenmiş birimler kalın olarak sağlandığı için, katmanlı birimlerden gelen bazı varolan veriler sağlama işlemi sırasında buluta itilebilir. Bu nedenle, yerel olarak sabitlenmiş bir birim oluşturmak için geçen süre, ses düzeyinin boyutu, cihazınızdaki veriler ve kullanılabilir bant genişliği gibi birden çok etkene bağlıdır. Birimleri olmayan yeni yüklenmiş bir aygıtta, yerel olarak sabitlenmiş bir birim oluşturma süresi terabayt başına yaklaşık 10 dakikadır. Ancak, yerel birimlerin oluşturulması, kullanılmakta olan bir aygıtta yukarıda açıklanan etkenlere bağlı olarak birkaç saat sürebilir.

**S.** Yerel olarak sabitlenmiş bir birim oluşturmak istiyorum. Farkında olmam gereken en iyi uygulamalar var mı?

**A.** Yerel olarak sabitlenmiş birimler, her zaman yerel veri garantisi gerektiren ve bulut gecikmelerine karşı hassas olan iş yükleri için uygundur. İş yüklerinizden herhangi biri için yerel birimlerin kullanımını düşünürken, lütfen aşağıdakilere dikkat edin:

* Yerel olarak sabitlenmiş birimler kalın olarak karşılanır ve yerel birimler oluşturmak katmanlı birimler için kullanılabilir alanı etkiler. Bu nedenle, depolama gereksiniminiz arttıkça daha küçük hacimlerle başlamanızı ve ölçeklendirmenizi öneririz.
* Yerel birimlerin sağlanması, varolan verileri katmanlı birimlerden buluta itmeyi gerektirebilecek uzun süren bir işlemdir. Sonuç olarak, bu birimlerde düşük performans yaşayabilirsiniz.
* Yerel birimlerin sağlanması zaman alan bir işlemdir. Söz konusu gerçek süre birden çok etkene bağlıdır: sağlanan birimin boyutu, cihazınızdaki veriler ve kullanılabilir bant genişliği. Varolan birimlerinizi buluta yedeklemediyseniz, birim oluşturma daha yavaştır. Yerel bir birim sağlamadan önce varolan birimlerinizin bulut anlık görüntülerini almanızı öneririz.
* Varolan katmanlı birimleri yerel olarak sabitlenmiş birimlere dönüştürebilirsiniz ve bu dönüştürme, elde edilen yerel olarak sabitlenmiş birim için aygıtta alan sağlanmasını içerir (varsa katmanlı verileri buluttan aşağı indirebilir). Yine, bu yukarıda tartıştık faktörlere bağlı olarak uzun süren bir işlemdir. Varolan birimler yedeklenmezse işlem daha da yavaş olacağından, dönüşümden önce varolan birimlerinizi yedeklemenizi öneririz. Cihazınız da bu işlem sırasında daha düşük performans yaşayabilir.

[Yerel olarak sabitlenmiş bir birim oluşturma](storsimple-8000-manage-volumes-u2.md#add-a-volume) hakkında daha fazla bilgi

**S.** Aynı anda birden çok yerel olarak sabitlenmiş birim oluşturabilir miyim?

**A.** Evet, ancak yerel olarak sabitlenmiş birim oluşturma ve genişletme işleri sırayla işlenir.

Yerel olarak sabitlenmiş birimler kalın bir şekilde karşılanır ve bu aygıtta yerel alan oluşturulmasını gerektirir (bu da katmanlı birimlerden gelen varolan verilerin sağlama işlemi sırasında buluta itilmesine neden olabilir). Bu nedenle, bir sağlama işi devam ediyorsa, diğer yerel birim oluşturma işleri bu iş tamamlanana kadar sıraya alınır.

Benzer şekilde, varolan bir yerel birim genişletiliyorsa veya katmanlı bir birim yerel olarak sabitlenmiş bir derme dönüştürülürse, önceki iş tamamlanana kadar yerel olarak sabitlenmiş yeni bir birim oluşturulması sıralanır. Yerel olarak sabitlenmiş bir birimin boyutunu genişletmek, o birim için varolan yerel alanın genişletilmesini içerir. Katmanlı bir birimden yerel olarak sabitlenmiş bir birimden dönüştürme, ortaya çıkan yerel olarak sabitlenmiş birim için yerel alan oluşturulmasını da içerir. Bu operasyonların her ikisinde de, yerel alanın oluşturulması veya genişletilmesi uzun süren bir iştir.

Bu işleri StorSimple Device Manager hizmetinin **İşler** bıçak bölümünde görüntüleyebilirsiniz. Etkin olarak işlenen iş, alan sağlamanın ilerlemesini yansıtacak şekilde sürekli olarak güncelleştirilir. Kalan yerel olarak sabitlenmiş birim işleri çalışan olarak işaretlenir, ancak ilerlemeleri durdurulur ve sıraya alındıkları sırada seçilir.

**S.** Yerel olarak sabitlenmiş bir ses düzeyini sildim. Yeni bir hacim oluşturmaya çalıştığımda neden geri alınan alanın kullanılabilir alana yansıdığını göremiyorum?

**A.** Yerel olarak sabitlenmiş bir birimi silerseniz, yeni birimler için kullanılabilir alan hemen güncelleştirilemeyebilir. StorSimple Aygıt Yöneticisi Hizmeti, yaklaşık her saat kullanılabilen yerel alanı güncelleştirir. Yeni bir birim oluşturmaya çalışmadan önce bir saat beklemenizi öneririz.

**S.** Bulut cihazında yerel olarak sabitlenmiş birimler desteklenir mi?

**A.** Yerel olarak sabitlenmiş birimler bulut aygıtında desteklenmez (eskiden StorSimple sanal aygıt olarak adlandırılan 8010 ve 8020 aygıtlar).

**S.** Azure PowerShell cmdlets'i yerel olarak sabitlenmiş hacimler oluşturmak ve yönetmek için kullanabilir miyim?

**A.** Hayır, Azure PowerShell cmdlets üzerinden yerel olarak sabitlenmiş hacimler oluşturamazsınız (Azure PowerShell üzerinden oluşturduğunuz tüm birimler katmanlıdır). Ayrıca, ses düzeyini katmanlı olarak değiştirmenin istenmeyen etkisi olacağından, azure PowerShell cmdlets'i yerel olarak sabitlenmiş bir birimin özelliklerini değiştirmek için kullanmamanızı öneririz.

## <a name="questions-about-backing-up-a-locally-pinned-volume"></a>Yerel olarak sabitlenmiş bir birimi yedekleme yle ilgili sorular
**S.** Yerel olarak sabitlenmiş birimlerin yerel anlık görüntüleri desteklendi mi?

**A.** Evet, yerel olarak sabitlenmiş birimlerinizin yerel anlık görüntülerini alabilirsiniz. Ancak, verilerinizin bir felaket durumunda korunduğundan emin olmak için yerel olarak sabitlenmiş birimlerinizi bulut anlık görüntüleriyle düzenli olarak yedeklemenizi öneririz.

Yerel olarak sabitlenmiş birimlerin yerel anlık görüntülerinin de buluta katmanlanabildiği ve aygıtın yerel katmanında kalacağı garanti edilmez.

**S.** Yerel olarak sabitlenmiş birimler için yerel anlık görüntüleri yönetmek için herhangi bir yönerge var mı?

**A.** Yerel olarak sabitlenmiş birimdeki yüksek veri karmaşasının yanı sıra sık sık yerel anlık görüntüler, aygıttaki yerel alanın hızlı bir şekilde tüketilmesine ve katmanlı birimlerden gelen verilerin buluta itilmesine neden olabilir. Bu nedenle, yerel anlık görüntü sayısını en aza indirmenizi öneririz.

**S.** Yerel olarak sabitlenmiş birimlerin yerel anlık görüntülerinin geçersiz kılınabileceğini belirten bir uyarı aldım. Bu ne zaman olabilir?

**A.** Yerel olarak sabitlenmiş birimdeki yüksek veri karmaşasının yanı sıra sık sık yerel anlık görüntüler, aygıttaki yerel alanın hızlı bir şekilde tüketilmesine neden olabilir. Aygıtın yerel katmanları yoğun olarak kullanılıyorsa, genişletilmiş bir bulut kesintisi aygıtın dolmasına neden olabilir ve gelen yazılar anlık görüntünün geçersiz sayılmasına neden olabilir (anlık görüntüleri eski bloklara başvurmak için enstantaneleri güncelleştirmek için yer olmadığı için üzerine yazılmış veriler). Böyle bir durumda, ses eyazı servis olmaya devam edecektir, ancak yerel anlık görüntüler geçersiz olabilir. Varolan bulut anlık görüntüleriniz üzerinde hiçbir etkisi yoktur.

Uyarı uyarısı, böyle bir durumun ortaya çıkabileceğini size bildirmek ve yerel anlık görüntü zamanlamalarınızı gözden geçirerek veya artık gerekli olmayan eski yerel anlık görüntüleri silerek aynı adresi zamanında ele aldığınızdan emin olmaktır.

Yerel anlık görüntüler geçersiz kılındıysa, belirli yedekleme ilkesinin yerel anlık görüntülerinin geçersiz kılındığını bildiren bir bilgi uyarısı alırsınız. Bu anlık görüntüler otomatik olarak silinir ve azure portalındaki **Yedek Kataloglar** bıtır'da artık görüntüleyebilirsiniz.

## <a name="questions-about-converting-a-tiered-volume-to-a-locally-pinned-volume"></a>Katmanlı bir birimi yerel olarak sabitlenmiş bir birime dönüştürme yle ilgili sorular
**S.** Katmanlı bir sesi yerel olarak sabitlenmiş bir ses hacmine dönüştürürken cihazdaki bazı yavaşlıkları gözlemliyorum. Bu neden oluyor?

**A.** Dönüştürme işlemi iki adım içerir:

1. Yakında dönüştürülecek olan yerel olarak sabitlenmiş birim için cihazda yer sağlanması.
2. Yerel garantiler sağlamak için buluttan herhangi bir katmanlı veri indirme.

Bu adımların her ikisi de dönüştürülen birimin boyutuna, aygıttaki verilere ve kullanılabilir bant genişliğine bağlı olarak uzun süren işlemlerdir. Varolan katmanlı birimlerden elde edilen bazı veriler sağlama işleminin bir parçası olarak buluta dökülebileceğinden, cihazınız bu süre içinde daha düşük performans yaşayabilir. Buna ek olarak, aşağıdaki durumlarda dönüştürme işlemi daha yavaş olabilir:

* Varolan birimler buluta yedeklenmedi; bu nedenle, bir dönüşüm başlatmadan önce birimlerinizi yedeklemenizi öneririz.
* Kullanılabilir bant genişliğini buluta sınırlandırabilecek bant genişliği azaltma ilkeleri uygulanmıştır; bu nedenle buluta özel bir 40 Mbps veya daha fazla bağlantınız olmasını öneririz.
* Dönüştürme işlemi, yukarıda açıklanan birden fazla etken nedeniyle birkaç saat sürebilir; bu nedenle, son tüketiciler üzerindeki etkiyi önlemek için bu işlemi zirve dışı zamanlarda veya bir hafta sonu gerçekleştirmenizi öneririz.

[Katmanlı bir birimin yerel olarak sabitlenmiş bir birime nasıl dönüştürüldüğü](storsimple-8000-manage-volumes-u2.md#change-the-volume-type) hakkında daha fazla bilgi

**S.** Birim dönüştürme işlemini iptal edebilir miyim?

**A.** Hayır, başlatıldıktan sonra dönüşüm işlemini iptal edemezsiniz. Önceki soruda da belirtildiği gibi, lütfen işlem sırasında karşılaşabileceğiniz olası performans sorunlarının farkında olun ve dönüşümünüzü planlarken yukarıda listelenen en iyi uygulamaları izleyin.

**S.** Dönüşüm işlemi başarısız olursa sesime ne olur?

**A.** Bulut bağlantısı sorunları nedeniyle birim dönüştürme başarısız olabilir. Aygıt, buluttan katmanlı verileri düşürmek için yapılan bir dizi başarısız denemeden sonra dönüşüm işlemini durdurabilir. Böyle bir senaryoda, birim türü dönüştürmeden önce kaynak birim türü olmaya devam eder ve:

* Birim dönüştürme hatasını size bildirmek için kritik bir uyarı yükseltilir. [Yerel olarak sabitlenmiş ciltlerle ilgili uyarılar](storsimple-8000-manage-alerts.md#locally-pinned-volume-alerts) hakkında daha fazla bilgi
* Katmanlı bir birimi yerel olarak sabitlenmiş bir biri dönüştürüyorsanız, veriler bulutta hala ikamet edebileceğinden, birim katmanlı bir birimin özelliklerini göstermeye devam eder. Bağlantı sorunlarını çözmenizi ve ardından dönüştürme işlemini yeniden denemenizi öneririz.
* Benzer şekilde, yerel olarak sabitlenmiş bir birimden katmanlı bir biri için dönüştürme başarısız olduğunda, birim yerel olarak sabitlenmiş bir birim olarak işaretlenmiş olsa da, katmanlı bir birim olarak işlev görür (çünkü veriler buluta dökülmüş olabilir). Ancak, aygıtın yerel katmanlarında yer kaplamaya devam eder. Bu alan, diğer yerel olarak sabitlenmiş birimler için kullanılamaz. Birim dönüştürmeişleminin tamamlandığından ve aygıttaki yerel alanın geri alınabilmesi için bu işlemi yeniden denemenizi öneririz.

## <a name="questions-about-restoring-a-locally-pinned-volume"></a>Yerel olarak sabitlenmiş bir birimi geri alma yla ilgili sorular
**S.** Yerel olarak sabitlenmiş birimler anında geri yüklenir mi?

**A.** Evet, yerel olarak sabitlenmiş birimler anında geri yüklenir. Geri yükleme işleminin bir parçası olarak birim için meta veri bilgileri buluttan çekilir çekilmez, birim çevrimiçi duruma getirilir ve ana bilgisayar tarafından erişilebilir. Ancak, tüm veriler buluttan indirilene kadar birim verilerinyerel garantileri bulunmaz ve geri yükleme süresince bu birimlerde düşük performans karşılaşabilirsiniz.

**S.** Yerel olarak sabitlenmiş bir birimi geri yüklemek ne kadar sürer?

**A.** Yerel olarak sabitlenmiş birimler anında geri yüklenir ve birim meta veri bilgileri buluttan alınır alınmaz çevrimiçi duruma getirilirken, birim verileri arka planda indirilmeye devam eder. Geri yükleme işleminin bu ikinci bölümü --birim verilerin yerel garantilerini geri almak- uzun süren bir işlemdir ve tüm verilerin yeniden yerel hale getirilmesi birkaç saat sürebilir. Aynı şeyi tamamlamak için geçen süre, geri yüklenen ses düzeyinin boyutu ve kullanılabilir bant genişliği gibi birden çok etkene bağlıdır. Geri yüklenen özgün birim silinmişse, geri yükleme işleminin bir parçası olarak aygıtta yerel alan oluşturmak için ek süre alınır.

**S.** Varolan yerel olarak sabitlenmiş sesimi eski bir anlık görüntüye geri yüklemem gerekir (ses seviyesi katmanlı yken alınır). Bu durumda ses seviyesi katmanlı olarak geri yüklenecek mi?

**A.** Hayır, birim yerel olarak sabitlenmiş bir birim olarak geri yüklenir. Anlık görüntü, birimin katmanlandırılma zamanıyla tarihlendirilmiş olsa da, varolan birimleri geri getirirken, StorSimple her zaman şu anda var olduğu gibi diskteki birim türünü kullanır.

**S.** Ben son zamanlarda benim yerel olarak sabitlenmiş ses genişletilmiş, ama şimdi hacmi boyutu daha küçük olduğu bir zamana verileri geri yüklemem gerekir. Geçerli birimi yeniden boyutlandırmak geri yüklenecek ve geri yükleme tamamlandıktan sonra ses boyutunu genişletmek gerekir?

**A.** Evet, geri yükleme birimi yeniden boyutlandıracak ve geri yükleme tamamlandıktan sonra birimin boyutunu genişletmeniz gerekir.

**S.** Geri yükleme sırasında ses düzeyini değiştirebilir miyim?

**A.** Hayır, geri yükleme sırasında ses düzeyini değiştiremezsiniz.

* Silinen birimler anlık görüntüde depolanan tür olarak geri yüklenir.
* Varolan birimler, anlık görüntüde depolanan türe bakılmaksızın geçerli türüne göre geri yüklenir (önceki iki soruya bakın).

**S.** Yerel olarak sabitlenmiş sesimi geri yüklemem gerekiyor, ancak zaman anlık görüntüsünde yanlış bir nokta seçtim. Geçerli geri yükleme işlemini iptal edebilir miyim?

**A.** Evet, devam etme işlemi iptal edebilirsiniz. Ses düzeyi, geri yüklemenin başlangıcında duruma geri alınır. Ancak, geri yükleme devam ederken sese yapılan tüm yazmalar kaybolur.

**S.** Yerel olarak sabitlenmiş birimlerimden birinde bir geri yükleme işlemi başlattım ve şimdi biriktirme listesi kataloğumda oluşturduğunu hatırlayamadığım bir anlık görüntü görüyorum. Bu ne için kullanılır?

**A.** Bu, geri yükleme işleminden önce oluşturulan geçici anlık görüntüdür ve geri yüklemenin iptal edilmesi veya başarısız olması durumunda geri alma için kullanılır. Bu anlık görüntüsünü silmeyin; geri yükleme tamamlandığında otomatik olarak silinir. Geri yükleme işinizyalnızca yerel olarak sabitlenmiş birimlere veya yerel olarak sabitlenmiş ve katmanlı birimlerin bir karışımına sahipse, bu davranış oluşabilir. Geri yükleme işi yalnızca katmanlı birimleri içeriyorsa, bu davranış oluşmaz.

**S.** Yerel olarak sabitlenmiş bir ses düzeyini klonlayabilir miyim?

**A.** Evet, bunu yapabilirsiniz. Ancak, yerel olarak sabitlenmiş birim varsayılan olarak katmanlı bir birim olarak klonlanır. [Yerel olarak sabitlenmiş bir hacmin](storsimple-8000-clone-volume-u2.md) nasıl klonlanabildiğini hakkında daha fazla bilgi

## <a name="questions-about-failing-over-a-locally-pinned-volume"></a>Yerel olarak sabitlenmiş bir birim üzerinde başarısız hakkında sorular
**S.** Cihazım da başka bir fiziksel cihazda başarısız olmak zorunda. Yerel olarak sabitlenmiş birimlerim yerel olarak sabitlenmiş veya katmanlı olarak başarısız olacak mı?

**A.** Hedef aygıt StorSimple 8000 serisi güncelleştirme3 veya daha yüksek bir süredir çalışıyorsa, yerel olarak sabitlenmiş olan yerel olarak sabitlenmiş birimler üzerinde başarısız olur.

[Sürümler arasında yerel olarak sabitlenmiş birimlerin failover ve DR](storsimple-8000-device-failover-disaster-recovery.md#device-failover-across-software-versions) hakkında daha fazla bilgi

**S.** Olağanüstü durum kurtarma (DR) sırasında yerel olarak sabitlenmiş birimler anında geri yüklenir mi?

**A.** Evet, yerel olarak sabitlenmiş birimler, başarısız lık sırasında anında geri yüklenir. Hata üzerinde çalışma işleminin bir parçası olarak birimiçin meta veri bilgileri buluttan çekilir çekilmez, birim hedef aygıtta çevrimiçi duruma getirilir ve ana bilgisayar tarafından erişilebilir. Bu arada, birim verileri arka planda karşıdan yükmeye devam eder ve bu birimlerde başarısızlık süresince düşük performans la karşılaşabilirsiniz.

**S.** Başarısız işin tamamlandığını görüyorum, hedef aygıtta geri yüklenen yerel olarak sabitlenmiş hacmin ilerlemesini nasıl izleyebilirim?

**A.** Bir arıza işlemi sırasında, başarısız setteki tüm birimler anında geri yüklendikten ve hedef aygıtta çevrimiçi duruma getirildikten sonra başarısız iş tamamlandı olarak işaretlenir. Bu, üzerinde başarısız olmuş olabilecek yerel olarak sabitlenmiş tüm birimleri içerir; ancak, verilerin yerel garantileri yalnızca birimiçin tüm veriler karşıdan yüklendiğinde kullanılabilir olacaktır. Başarısız ın bir parçası olarak oluşturulan karşılık gelen geri yükleme işlerini izleyerek başarısız olan her yerel olarak sabitlenmiş birim için bu ilerlemeyi izleyebilirsiniz. Bu tek tek geri yükleme işleri yalnızca yerel olarak sabitlenmiş birimler için oluşturulur.

**S.** Başarısız olurken ses düzeyini değiştirebilir miyim?

**A.** Hayır, bir hata sırasında ses düzeyini değiştiremezsiniz. StorSimple 8000 serisi güncelleştirme 3'u çalıştıran başka bir fiziksel aygıtta başarısız oluyorsanız, ses birimleri anlık görüntüde depolanan ses türüne bağlı olarak başarısız olur.

**S.** Bulut cihazına yerel olarak sabitlenmiş hacimleri olan bir birim kapsayıcı üzerinde başarısız olabilir miyim?

**A.** Evet, bunu yapabilirsiniz. Yerel olarak sabitlenmiş birimler katmanlı birimler olarak başarısız olur. [Sürümler arasında yerel olarak sabitlenmiş birimlerin failover ve DR](storsimple-8000-device-failover-disaster-recovery.md#common-considerations-for-device-failover) hakkında daha fazla bilgi

