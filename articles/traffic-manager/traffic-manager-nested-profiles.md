---
title: Azure'da İç Içe Trafik Yöneticisi Profilleri
titleSuffix: Azure Traffic Manager
description: Bu makalede, Azure Trafik Yöneticisi'nin 'İç Içe Profiller' özelliği açıklanmaktadır
services: traffic-manager
documentationcenter: ''
author: rohinkoul
manager: twooley
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/22/2018
ms.author: rohink
ms.openlocfilehash: 282099cb274c1ea872a0df9c2753a939ef31421f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76938566"
---
# <a name="nested-traffic-manager-profiles"></a>İç İçe Traffic Manager profilleri

Trafik Yöneticisi, Trafik Yöneticisi'nin son kullanıcıdan hangi bitiş noktasının trafik alması gerektiğini nasıl seçeceğini denetlemenize olanak tanıyan bir dizi trafik yönlendirme yöntemi içerir. Daha fazla bilgi için [Trafik Yöneticisi trafik yönlendirme yöntemlerine](traffic-manager-routing-methods.md)bakın.

Her Trafik Yöneticisi profili nde tek bir trafik yönlendirme yöntemi belirtilir. Ancak, tek bir Trafik Yöneticisi profili tarafından sağlanan yönlendirmedaha karmaşık trafik yönlendirme gerektiren senaryolar vardır. Birden fazla trafik yönlendirme yönteminin avantajlarını birleştirmek için Trafik Yöneticisi profillerini iç içe çekebilirsiniz. İç içe profiller, daha büyük ve daha karmaşık uygulama dağıtımlarını desteklemek için varsayılan Trafik Yöneticisi davranışını geçersiz kılmanızı sağlar.

Aşağıdaki örnekler, iç içe gelişmiş Trafik Yöneticisi profillerinin çeşitli senaryolarda nasıl kullanılacağını göstermektedir.

## <a name="example-1-combining-performance-and-weighted-traffic-routing"></a>Örnek 1: 'Performans' ve 'Ağırlıklı' trafik yönlendirmesi birleştirme

Bir uygulamayı aşağıdaki Azure bölgelerinde dağıtdığınızı varsayalım: Batı ABD, Batı Avrupa ve Doğu Asya. Trafiği kullanıcıya en yakın bölgeye dağıtmak için Trafik Yöneticisi'nin 'Performans' trafik yönlendirme yöntemini kullanırsınız.

![Tek Trafik Yöneticisi profili][4]

Şimdi, bir güncelleştirmeyi daha yaygın olarak kullanıma sunmadan önce hizmetinizle sınamak istediğinizi varsayalım. Küçük bir trafik yüzdesini test dağıtımınıza yönlendirmek için 'ağırlıklı' trafik yönlendirme yöntemini kullanmak istiyorsunuz. Test dağıtımını Batı Avrupa'daki varolan üretim dağıtımının yanında ayarlarsınız.

Hem 'Ağırlıklı' hem de 'Performans trafiği yönlendirmeyi tek bir profilde birleştiremezsiniz. Bu senaryoyu desteklemek için, iki Batı Avrupa uç noktasını ve 'Ağırlıklı' trafik yönlendirme yöntemini kullanarak bir Trafik Yöneticisi profili oluşturursunuz. Ardından, bu 'alt' profili 'üst' profiline bir bitiş noktası olarak eklersiniz. Üst profil hala Performans trafiği yönlendirme yöntemini kullanır ve bitiş noktası olarak diğer genel dağıtımları içerir.

Aşağıdaki diyagramda bu örnek gösteriş verilmiştir:

![İç İçe Traffic Manager profilleri][2]

Bu yapılandırmada, üst profil üzerinden yönlendirilen trafik, trafiği normal olarak bölgeler arasında dağıtır. Batı Avrupa'da iç içe geçen profil, trafiği belirlenen ağırlıklara göre üretim ve test bitiş noktalarına dağıtır.

Üst profil 'Performans' trafik yönlendirme yöntemini kullandığında, her bitiş noktasına bir konum atanması gerekir. Bitiş noktasını yapılandırdığınızda konum atanır. Dağıtımınıza en yakın Azure bölgesini seçin. Azure bölgeleri, Internet Gecikme Tablosu tarafından desteklenen konum değerleridir. Daha fazla bilgi için [Trafik Yöneticisi 'Performans' trafik yönlendirme yöntemine](traffic-manager-routing-methods.md#performance)bakın.

## <a name="example-2-endpoint-monitoring-in-nested-profiles"></a>Örnek 2: İç Içe Profillerde Uç Nokta izleme

Trafik Yöneticisi, her hizmet bitiş noktasının durumunu etkin bir şekilde izler. Bir bitiş noktası sağlıksızsa, Traffic Manager hizmetinizin kullanılabilirliğini korumak için kullanıcıları alternatif uç noktalara yönlendirir. Bu uç nokta izleme ve başarısız davranış tüm trafik yönlendirme yöntemleri için geçerlidir. Daha fazla bilgi için [Trafik Yöneticisi Uç Nokta İzleme'ye](traffic-manager-monitoring.md)bakın. Uç nokta izleme iç içe profiller için farklı çalışır. İç içe profillerde, üst profil doğrudan çocuk üzerinde sistem durumu denetimleri yapmaz. Bunun yerine, alt profilin son noktalarının durumu, alt profilin genel durumunu hesaplamak için kullanılır. Bu sistem durumu bilgileri iç içe profil hiyerarşisini yayıştırılır. Üst profil, trafiği alt profile yönlendirip yönlendirmeyeceğini belirlemek için bu toplu durumu kullanır. İç içe profillerin sistem durumu izleme yle ilgili tüm ayrıntılar için [SSS](traffic-manager-FAQs.md#traffic-manager-nested-profiles) bölümüne bakın.

Önceki örneğe dönersek, Batı Avrupa'daki üretim dağıtımının başarısız olduğunu varsayalım. Varsayılan olarak, 'alt' profili tüm trafiği test dağıtımına yönlendirir. Sınama dağıtımı da başarısız olursa, üst profil tüm alt uç noktaları sağlıksız olduğundan alt profilin trafik almamasını belirler. Ardından, üst profil trafiği diğer bölgelere dağıtır.

![İç içe profil failover (varsayılan davranış)][3]

Bu anlaşmadan memnun olabilirsin. Ya da Batı Avrupa'daki tüm trafiğin artık sınırlı bir alt ayar trafiği yerine test dağıtımına gideceğinden endişe duyabilirsiniz. Test dağıtımının durumu ne olursa olsun, Batı Avrupa'daki üretim dağıtımı başarısız olduğunda diğer bölgelere gitmek te başarısız olmak istersiniz. Bu başarısıznoktayı etkinleştirmek için, alt profili üst profilde bir bitiş noktası olarak yapılandırırken 'MinChildEndpoints' parametresini belirtebilirsiniz. Parametre, alt profildeki kullanılabilir uç noktaların en az sayısını belirler. Varsayılan değer '1'dir. Bu senaryo için MinChildEndpoints değerini 2 olarak ayarlarsınız. Bu eşiğin altında, üst profil tüm alt profilin kullanılamaz olduğunu kabul eder ve trafiği diğer uç noktalara yönlendirir.

Aşağıdaki şekil bu yapılandırmayı göstermektedir:

!['MinChildEndpoints' = 2 ile İç Içe Profil failover][4]

> [!NOTE]
> 'Öncelik' trafik yönlendirme yöntemi tüm trafiği tek bir bitiş noktasına dağıtır. Bu nedenle, minChildEndpoints ayarında çocuk profili için '1' dışında çok az amaç vardır.

## <a name="example-3-prioritized-failover-regions-in-performance-traffic-routing"></a>Örnek 3: 'Performans' trafik yönlendirmesinde öncelikli başarısız bölgeler

'Performans' trafik yönlendirme yöntemi için varsayılan davranış, farklı coğrafi konumlarda uç noktaları nız olduğunda, son kullanıcıların en düşük ağ gecikmesi açısından "en yakın" bitiş noktasına yönlendirilmesidir.

Ancak, Batı Avrupa trafiğinin Batı ABD'ye devrinin başarısız olmasını ve trafiği yalnızca her iki uç nokta kullanılamıyorsa diğer bölgelere yönlendirdiğinizi varsayalım. Bu çözümü, 'Öncelik' trafik yönlendirme yöntemine sahip bir alt profil kullanarak oluşturabilirsiniz.

![Tercihli arıza ile 'Performans' trafik yönlendirme][6]

Batı Avrupa bitiş noktası Batı ABD bitiş noktasından daha yüksek önceliğe sahip olduğundan, her iki uç nokta da çevrimiçi olduğunda tüm trafik Batı Avrupa bitiş noktasına gönderilir. Batı Avrupa başarısız olursa, trafik Batı ABD'ye yönlendirilir. İç içe profille, trafik doğu Asya'ya yönlendirilir ancak hem Batı Avrupa hem de Batı ABD başarısız olduğunda.

Bu deseni tüm bölgeler için yineleyebilirsiniz. Üst profildeki üç uç noktayı da her biri öncelikli bir hata dizisi sağlayan üç alt profille değiştirin.

## <a name="example-4-controlling-performance-traffic-routing-between-multiple-endpoints-in-the-same-region"></a>Örnek 4: Aynı bölgedeki birden çok uç nokta arasında 'Performans' trafik yönlendirmesini denetleme

'Performans' trafik yönlendirme yönteminin belirli bir bölgede birden fazla bitiş noktası olan bir profilde kullanıldığını varsayalım. Varsayılan olarak, o bölgeye yönlendirilen trafik, o bölgedeki tüm kullanılabilir uç noktalara eşit olarak dağıtılır.

!['Performans' trafik yönlendirme bölge içi trafik dağılımı (varsayılan davranış)][7]

Batı Avrupa'da birden çok uç nokta eklemek yerine, bu uç noktalar ayrı bir alt profilde eklenir. Alt profil, Batı Avrupa'daki tek bitiş noktası olarak ebeveyne eklenir. Alt profildeki ayarlar, o bölgede öncelik tabanlı veya ağırlıklı trafik yönlendirmesini etkinleştirerek Batı Avrupa ile trafik dağılımını denetleyebilir.

![Özel bölge içi trafik dağılımı ile 'Performans' trafik yönlendirmesi][8]

## <a name="example-5-per-endpoint-monitoring-settings"></a>Örnek 5: Uç nokta başına izleme ayarları

Trafiği eski bir şirket içi web sitesinden Azure'da barındırılan bulut tabanlı yeni bir sürüme sorunsuz bir şekilde geçirmek için Trafik Yöneticisi'ni kullandığınızı varsayalım. Eski site için, site durumunu izlemek için ana sayfa URI'yi kullanmak istiyorsunuz. Ancak yeni Bulut tabanlı sürüm için, ek denetimler içeren özel bir izleme sayfası (yol '/monitor.aspx') uyguluyorsunuz.

![Trafik Yöneticisi uç nokta izleme (varsayılan davranış)][9]

Trafik Yöneticisi profilindeki izleme ayarları, tek bir profildeki tüm uç noktalara uygulanır. İç içe profillerde, farklı izleme ayarlarını tanımlamak için site başına farklı bir alt profil kullanırsınız.

![Uç nokta ayarları başına trafik yöneticisi uç nokta izleme][10]

## <a name="faqs"></a>SSS

* [İç içe profilleri nasıl yapılandırıyorum?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#traffic-manager-endpoint-monitoring)

* [Trafik Yöneticisi kaç kat iç içe geçmeyi destekler?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#how-many-layers-of-nesting-does-traffic-manger-support)

* [Aynı Trafik Yöneticisi profilinde iç içe geçen alt profillerle diğer uç nokta türlerini karıştırabilir miyim?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#can-i-mix-other-endpoint-types-with-nested-child-profiles-in-the-same-traffic-manager-profile)

* [İç içe profiller için faturalandırma modeli nasıl uygulanır?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#how-does-the-billing-model-apply-for-nested-profiles)

* [İç içe profiller için bir performans etkisi var mı?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#is-there-a-performance-impact-for-nested-profiles)

* [Trafik Yöneticisi, üst profilde iç içe bir uç noktanın durumunu nasıl hesaplar?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#how-does-traffic-manager-compute-the-health-of-a-nested-endpoint-in-a-parent-profile)

## <a name="next-steps"></a>Sonraki adımlar

[Trafik Yöneticisi profilleri](traffic-manager-overview.md) hakkında daha fazla bilgi edinin

Trafik Yöneticisi profilini nasıl [oluşturabilirsiniz](traffic-manager-create-profile.md) öğrenin

<!--Image references-->
[1]: ./media/traffic-manager-nested-profiles/figure-1.png
[2]: ./media/traffic-manager-nested-profiles/figure-2.png
[3]: ./media/traffic-manager-nested-profiles/figure-3.png
[4]: ./media/traffic-manager-nested-profiles/figure-4.png
[5]: ./media/traffic-manager-nested-profiles/figure-5.png
[6]: ./media/traffic-manager-nested-profiles/figure-6.png
[7]: ./media/traffic-manager-nested-profiles/figure-7.png
[8]: ./media/traffic-manager-nested-profiles/figure-8.png
[9]: ./media/traffic-manager-nested-profiles/figure-9.png
[10]: ./media/traffic-manager-nested-profiles/figure-10.png
