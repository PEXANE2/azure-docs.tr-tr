---
title: Otomatik ölçeklendirme için en iyi uygulamalar
description: Web Uygulamaları, Sanal Makine Ölçeği kümeleri ve Bulut Hizmetleri için Azure'da otomatik ölçeklendirme desenleri
ms.topic: conceptual
ms.date: 07/07/2017
ms.subservice: autoscale
ms.openlocfilehash: a05cf87e660cc6c388ea2055bb174c47b99da4a3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79248924"
---
# <a name="best-practices-for-autoscale"></a>Otomatik ölçeklendirme için en iyi uygulamalar
Azure Monitör otomatik ölçeklendirmesi yalnızca [Sanal Makine Ölçeği Kümeleri,](https://azure.microsoft.com/services/virtual-machine-scale-sets/) [Bulut Hizmetleri,](https://azure.microsoft.com/services/cloud-services/) [Uygulama Hizmeti - Web Uygulamaları](https://azure.microsoft.com/services/app-service/web/)ve [API Yönetimi hizmetleri](https://docs.microsoft.com/azure/api-management/api-management-key-concepts)için geçerlidir.

## <a name="autoscale-concepts"></a>Otomatik ölçeklendirme kavramları

* Bir kaynağın yalnızca *bir* otomatik ölçek lendirme ayarı olabilir
* Otomatik ölçeklendirme ayarı bir veya daha fazla profile sahip olabilir ve her profilin bir veya daha fazla otomatik ölçek kuralı olabilir.
* Otomatik ölçeklendirme ayarı, örnekleri artırarak *ve* örnek sayısını azaltarak *çıkan* örnekleri yatay olarak ölçeklendiriyor.
  Otomatik ölçeklendirme ayarı, örneklerin en büyük, en küçük ve varsayılan değerine sahiptir.
* Otomatik ölçeklendirme işi her zaman ölçeklendirme veya ölçeklendirme için yapılandırılan eşiği geçip geçilmediğini denetleyerek, ölçeklendirme için ilişkili ölçütleri okur. [Azure Monitor'da otomatik](autoscale-common-metrics.md)ölçeklendirme yle ölçeklendirilebilen ölçümlerin listesini görüntüleyebilirsiniz.
* Tüm eşikler örnek düzeyinde hesaplanır. Örneğin, "örnek sayısı 2 olduğunda ortalama CPU > %80 olduğunda bir örneğe göre ölçeklendirin", tüm örneklerdeki ortalama CPU %80'den büyük olduğunda ölçeklendirme anlamına gelir.
* Tüm otomatik ölçek hataları Etkinlik Günlüğü'ne kaydedilir. Daha sonra, otomatik ölçeklendirme hatası olduğunda e-posta, SMS veya webhooks yoluyla bilgilendirilebilmeniz için bir [etkinlik günlüğü uyarısı](./../../azure-monitor/platform/activity-log-alerts.md) yapılandırabilirsiniz.
* Benzer şekilde, tüm başarılı ölçek eylemleri Etkinlik Günlüğü'ne nakledilir. Daha sonra başarılı bir otomatik ölçeklendirme eylemi olduğunda e-posta, SMS veya webhooks yoluyla haberdar edilebilir, böylece bir etkinlik günlüğü uyarısı yapılandırabilirsiniz. Otomatik ölçek ayarındaki bildirimler sekmesi aracılığıyla başarılı ölçekli eylemler için bildirim almak için e-posta veya webhook bildirimlerini de yapılandırabilirsiniz.

## <a name="autoscale-best-practices"></a>Otomatik ölçeklendirme en iyi uygulamaları

Otomatik ölçeklendirme kullanırken aşağıdaki en iyi uygulamaları kullanın.

### <a name="ensure-the-maximum-and-minimum-values-are-different-and-have-an-adequate-margin-between-them"></a>En yüksek ve en düşük değerlerin farklı olduğundan emin olun ve bunların arasında yeterli bir boşluk bırakın

En az=2, maksimum=2 ve geçerli örnek sayısı 2 olan bir ayar varsa, ölçek eylemi gerçekleşemez. Kapsayıcı maksimum ve minimum örnek sayıları arasında yeterli bir kenar boşluğu tutun. Otomatik ölçekher zaman bu sınırlar arasında ölçekler.

### <a name="manual-scaling-is-reset-by-autoscale-min-and-max"></a>El ile ölçeklendirme, otomatik ölçeklendirmenin en düşük ve en yüksek değerleriyle sıfırlanır

Örnek sayısını maksimumun üstünde veya altındaki bir değere el ile güncellerseniz, otomatik ölçeklendirme motoru otomatik olarak en aza (aşağıdaysa) veya maksimuma (yukarıdaysa) geri ölçeklenir. Örneğin, aralığı 3 ile 6 arasında ayarlarsınız. Çalışan bir örneğinvarsa, otomatik ölçeklendirme motoru bir sonraki çalışmasında üç örneğine ölçekler. Aynı şekilde, ölçeği el ile sekiz örneğe ayarlarsanız, bir sonraki çalıştırmaotomatik ölçeğinde, bir sonraki çalışmasında altı örneğine geri ölçeklendirin.  Otomatik ölçeklendirme kurallarını da sıfırlamadığınız sürece manuel ölçeklendirme geçicidir.

### <a name="always-use-a-scale-out-and-scale-in-rule-combination-that-performs-an-increase-and-decrease"></a>Her zaman bir artış ve azalma gerçekleştiren bir ölçeklendirme ve ölçek-in kural kombinasyonu kullanın
Kombinasyonun yalnızca bir bölümünü kullanırsanız, otomatik ölçek yalnızca profilde tanımlanan maksimum veya minimum örnek sayısına ulaşana kadar tek bir yönde (ölçeklendirme veya içinde) işlem yapacaktır. Bu en uygun değildir, ideal olarak kullanılabilirliği sağlamak için kaynağınızın yüksek kullanım zamanlarında ölçeklendirmesini istersiniz. Benzer şekilde, düşük kullanım zamanlarında kaynağınızın küçültülmesini istersiniz, böylece maliyet tasarrufu gerçekleştirebilirsiniz.

### <a name="choose-the-appropriate-statistic-for-your-diagnostics-metric"></a>Tanılama ölçümünüz için uygun istatistiği seçin
Tanılama ölçümleri için, ölçeklendirilecek bir metrik olarak *Ortalama,* *Minimum,* *Maksimum* ve *Toplam* arasında seçim yapabilirsiniz. En yaygın istatistik *Average*Ortalama'dır.

### <a name="choose-the-thresholds-carefully-for-all-metric-types"></a>Tüm ölçüm türlerine yönelik eşikleri dikkatle seçin
Pratik durumlara göre ölçeklendirme ve ölçeklendirme için farklı eşikleri dikkatle seçmenizi öneririz.

Aşağıdaki örneklerdeki gibi otomatik ölçeklendirme ayarlarını, çıkış ve koşullar için aynı veya benzer eşik değerlerine sahip olarak *önermiyoruz:*

* İş Parçacığı Sayısı >= 600 olduğunda örnekleri 1 sayıya göre artırın
* İş Parçacığı Sayısı <= 600 olduğunda örnekleri 1 sayıya göre azaltın

Kafa karıştırıcı görünen bir davranışa neyin yol açabileceğine dair bir örneğe bakalım. Aşağıdaki sırayı göz önünde bulundurun.

1. Başlangıçta iki örnek olduğunu ve ardından her örnek teki iş parçacığı sayısının 625'e kadar büyüdüğünü varsayalım.
2. Otomatik ölçeklendirme üçüncü bir örnek ekleyerek ölçekler.
3. Ardından, örnek teki ortalama iş parçacığı sayısının 575'e düştüğünü varsayalım.
4. Ölçeklendirmeden önce, otomatik ölçeklendirme, ölçeklendirildiyse son durum ne olacağını tahmin etmeye çalışır. Örneğin, 575 x 3 (geçerli örnek sayısı) = 1,725 / 2 (küçültüldüğünde son örnek sayısı) = 862,5 iş parçacığı. Bu, ortalama iş parçacığı sayısı aynı kalırsa veya yalnızca küçük bir miktar düşerse, otomatik ölçeklendirmenin ölçeklendirildikten sonra bile hemen yeniden ölçeklendirmesi gerekeceği anlamına gelir. Ancak, tekrar ölçeklendirilirse, tüm işlem yinelenir ve sonsuz bir döngüye yol açarak.
5. Bu durumu önlemek için ("çırpma" olarak adlandırılır), otomatik ölçek hiç küçülmez. Bunun yerine, hizmetin işi bir sonraki yürütülmede durumu atlar ve yeniden değerlendirir. Otomatik ölçek, ortalama iş parçacığı sayısı 575 olduğunda çalışmadığından, çırpma durumu birçok kişinin kafasını karıştırabilir.

Bir ölçeklendirme sırasında tahmin , ölçeklendirme ve ölçeklendirme eylemlerinin sürekli olarak ileri geri gittiği "kanat çırpma" durumlarından kaçınmak için tasarlanmıştır. Ölçeklendirme ve giriş için aynı eşikleri seçerken bu davranışı aklınızda bulundurun.

Ölçeklendirme ve eşikler arasında yeterli bir kenar boşluğu seçmenizi öneririz. Örnek olarak, aşağıdaki daha iyi kural kombinasyonunu göz önünde bulundurun.

* CPU% >= 80 olduğunda örnekleri 1 sayısına göre artırın
* CPU% <= 60 olduğunda örnekleri 1 sayısına göre azaltın

Bu durumda  

1. Başlamak için 2 örnek olduğunu varsayalım.
2. Örnekler arasında ortalama CPU%80'e çıkarsa, otomatik ölçeklendirme üçüncü bir örnek ekleyerek ölçeklendirin.
3. Şimdi cpu% 60 düşüyor zaman içinde varsayalım.
4. Otomatik ölçeklendirme kuralı, ölçeklendirme için son durumu tahmin eder. Örneğin, 60 x 3 (geçerli örnek sayısı) = 180 / 2 (küçültüldüğünde son örnek sayısı) = 90. Bu yüzden otomatik ölçeklendirme ölçeklendirme yapmaz, çünkü hemen yeniden ölçeklendirmesi gerekir. Bunun yerine, ölçekleme aşağı atlar.
5. Bir sonraki otomatik ölçeklendirme denetiminde, CPU 50'ye düşmeye devam eder. Tekrar tahmin - 50 x 3 örnek = 150 / 2 örnekleri = 75, hangi 80 ölçek-out eşik altındadır, bu yüzden başarıyla 2 örnek ölçekler.

### <a name="considerations-for-scaling-threshold-values-for-special-metrics"></a>Özel ölçümler için eşik değerleri ölçeklendirme konusunda dikkat edilmesi gerekenler
 Depolama veya Servis Veri Yolu Sırası uzunluk ölçümü gibi özel ölçümler için eşik, geçerli örnek sayısı başına kullanılabilen ortalama ileti sayısıdır. Bu ölçüm için eşik değerini dikkatle seçin.

Davranışı daha iyi anladığınızdan emin olmak için bunu bir örnekle gösterelim.

* Depolama Sırası ileti sayısı >= 50 olduğunda örnekleri 1 sayısına göre artırma
* Depolama Sırası ileti sayısı <= 10 olduğunda örnekleri 1 sayısına göre azaltın

Aşağıdaki sırayı göz önünde bulundurun:

1. İki depolama sırası örneği vardır.
2. İletiler gelmeye devam ediyor ve depolama sırasını gözden geçirdiğinizde toplam sayı 50'yi okur. Otomatik ölçeklendirmenin bir ölçeklendirme eylemi başlatması gerektiğini varsayabilirsiniz. Ancak, yine de örnek başına 50/2 = 25 ileti olduğunu unutmayın. Yani, ölçeklendirme oluşmaz. İlk ölçeklendirmenin gerçekleşmesi için depolama kuyruğundaki toplam ileti sayısı 100 olmalıdır.
3. Ardından, toplam ileti sayısının 100'e ulaştığını varsayalım.
4. Ölçeklendirme eylemi nedeniyle üçüncü bir depolama sırası örneği eklenir.  150/3 = 50 olduğundan, kuyruktaki toplam ileti sayısı 150'ye ulaşana kadar bir sonraki ölçeklendirme eylemi gerçekleşmez.
5. Artık kuyruktaki ileti sayısı azalır. Üç örnekle, ilk ölçeklendirme eylemi, tüm kuyruklarda toplam iletiler 30'a kadar eklediğinde gerçekleşir, çünkü örnek başına 30/3 = 10 ileti, yani ölçek-giriş eşiğidir.

### <a name="considerations-for-scaling-when-multiple-profiles-are-configured-in-an-autoscale-setting"></a>Birden fazla profil otomatik ölçeklendirme ayarında yapılandırıldığında ölçeklendirme konusunda dikkat edilmesi gerekenler
Otomatik ölçeklendirme ayarında, zamanlamaya veya saate herhangi bir bağımlılık olmadan her zaman uygulanan varsayılan bir profil seçebilir veya tarih ve saat aralığı olan belirli bir dönem için yinelenen bir profil veya profil seçebilirsiniz.

Otomatik ölçeklendirme hizmeti bunları işlerken, her zaman aşağıdaki sırayla denetler:

1. Sabit Tarih profili
2. Yinelenen profil
3. Varsayılan ("Her zaman") profili

Bir profil koşulu karşılanırsa, otomatik ölçek altındaki sonraki profil koşulunu denetlemez. Otomatik ölçeklendirme aynı anda yalnızca bir profili işler. Bu, daha düşük katmanlı bir profilden bir işleme koşulu da eklemek istiyorsanız, bu kuralları geçerli profile de eklemeniz gerektiği anlamına gelir.

Bir örnek kullanarak gözden geçirelim:

Aşağıdaki resimde varsayılan profili en az olan bir otomatik ölçek ayarı = 2 ve en büyük örnek = 10' dur. Bu örnekte, kuyruktaki ileti sayısı 10'dan büyük olduğunda ve kuyruktaki ileti sayısı üçten az olduğunda ölçeklendirme küçülene kural landırılır. Yani şimdi kaynak iki ve on örnek arasında ölçeklendirebilirsiniz.

Buna ek olarak, Pazartesi için yinelenen bir profil kümesi vardır. Minimum örnekler = 3 ve en büyük örnekler = 10 olarak ayarlanır. Bu, Pazartesi günü, bu durum için ilk kez otomatik ölçeklendirme denetler, örnek sayısı iki ise, üç yeni en az ölçekler anlamına gelir. Otomatik ölçek, bu profil koşulunu (Pazartesi) eşleştirmeye devam ettiği sürece, yalnızca bu profil için yapılandırılan CPU tabanlı ölçeklendirme/in kurallarını işler. Şu anda, sıra uzunluğunu denetlemez. Ancak, sıra uzunluğu koşulunun da denetlenmesini istiyorsanız, bu kuralları varsayılan profilden de Pazartesi profilinize eklemeniz gerekir.

Benzer şekilde, otomatik ölçek varsayılan profile geri döndüğünde, minimum ve maksimum koşullar karşılanıp karşılanmamasını ilk olarak denetler. O anda örnek sayısı 12 ise, varsayılan profil için izin verilen en fazla olan 10'a ölçeklenir.

![otomatik ölçeklendirme ayarları](./media/autoscale-best-practices/insights-autoscale-best-practices-2.png)

### <a name="considerations-for-scaling-when-multiple-rules-are-configured-in-a-profile"></a>Birden fazla kural bir profilde yapılandırıldığında ölçeklendirme konusunda dikkat edilmesi gerekenler

Bir profilde birden çok kural belirlemeniz gerekebilecek durumlar vardır. Birden çok kural ayarlandığında, aşağıdaki otomatik ölçeklendirme kuralları otomatik ölçeklendirme altyapısı tarafından kullanılır.

*Ölçeklendirmede,* herhangi bir kural yerine getirilirse otomatik ölçek çalışır.
*Ölçek-in,* otomatik ölçek tüm kuralların karşılanmasını gerektirir.

Göstermek için, aşağıdaki dört otomatik ölçek kuralına sahip olduğunuzu varsayalım:

* CPU %30 <, ölçek-in 1
* Bellek %50 <, 1 ölçeklendir
* CPU %75 >, 1 ölçeklendir
* Bellek %75 >, ölçeklendirme 1

Sonra aşağıdaki oluşur:

* CPU %76 ve Bellek %50 ise, ölçeklendirin.
* CPU %50 ve Bellek %76 ise, ölçeklendirin.

Öte yandan, CPU% 25 ve bellek% 51 otomatik ölçeklendirme ise ölçeklendirme **yapmaz.** Ölçeklendirme yapabilmek için CPU'nun %29 ve Bellek %49 olması gerekir.

### <a name="always-select-a-safe-default-instance-count"></a>Her zaman güvenli bir varsayılan örnek sayısı seçin
Otomatik ölçek, ölçümler kullanılamadığında hizmetinizi bu sayıya ölçeklendirdiği için varsayılan örnek sayısı önemlidir. Bu nedenle, iş yüklerin için güvenli olan varsayılan örnek sayısını seçin.

### <a name="configure-autoscale-notifications"></a>Otomatik ölçeklendirme bildirimlerini yapılandırma
Aşağıdaki koşullardan herhangi biri oluşursa Otomatik Ölçek Etkinlik Günlüğü'ne yayınlanacaktır:

* Otomatik ölçeklendirme bir ölçek işlemi sorunları.
* Otomatik ölçeklendirme hizmeti bir ölçek eylemini başarıyla tamamlar.
* Otomatik ölçeklendirme hizmeti bir ölçek eylemi yapmak için başarısız olur.
* Otomatik ölçeklendirme hizmetinin ölçek kararı vermek için ölçümler kullanılamaz.
* Ölçümler yeniden ölçek kararı vermek için kullanılabilir (kurtarma).

Otomatik ölçeklendirme motorunun durumunu izlemek için etkinlik günlüğü uyarısı da kullanabilirsiniz. Aboneliğinizdeki [tüm otomatik ölçeklendirme motor işlemlerini izlemek](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-alert) veya [aboneliğinizdeki tüm başarısız otomatik ölçek ölçeklendirme/ölçeklendirme işlemlerini izlemek için bir Etkinlik Günlüğü Uyarısı oluşturmak için](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-failed-alert)bir Etkinlik Günlüğü Uyarısı oluşturmak için örnekler verilmiştir.

Etkinlik günlüğü uyarılarını kullanmanın yanı sıra, otomatik ölçek ayarındaki bildirimler sekmesi aracılığıyla başarılı ölçekli eylemler için bildirim almak için e-posta veya webhook bildirimlerini de yapılandırabilirsiniz.

## <a name="next-steps"></a>Sonraki Adımlar
- [Aboneliğinizdeki tüm otomatik ölçeklendirme motor işlemlerini izlemek için bir Etkinlik Günlüğü Uyarısı oluşturun.](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-alert)
- [Aboneliğinizdeki tüm başarısız otomatik ölçek giriş/ölçeklendirme işlemlerini izlemek için Etkinlik Günlüğü Uyarısı oluşturun](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-failed-alert)

