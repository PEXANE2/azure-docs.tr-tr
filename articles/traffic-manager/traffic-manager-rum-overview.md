---
title: Azure Trafik Yöneticisi'nde Gerçek Kullanıcı Ölçümleri
description: Bu girişte, Azure Trafik Yöneticisi Gerçek Kullanıcı Ölçümlerini öğrenin.
services: traffic-manager
documentationcenter: traffic-manager
author: rohinkoul
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 03/16/2018
ms.author: rohink
ms.custom: ''
ms.openlocfilehash: e38e1255b1a84ab5d3fd37e16eb65c76001dbfa1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76938443"
---
# <a name="traffic-manager-real-user-measurements-overview"></a>Traffic Manager Gerçek Kullanıcı Ölçümleri’ne genel bakış

Performans yönlendirme yöntemini kullanmak üzere bir Trafik Yöneticisi profili ayarladığınızda, hizmet DNS sorgu isteklerinin nereden geldiğini bakar ve bu istekleri en düşük gecikme süresini veren Azure bölgesine yönlendirmek için yönlendirme kararları verir. Bu, Trafik Yöneticisi'nin farklı son kullanıcı ağları için koruduğu ağ gecikme syon zekasıkullanılarak gerçekleştirilir.

Gerçek Kullanıcı Ölçümleri, ağ gecikme süresi ölçümlerini son kullanıcılarınızın kullandığı istemci uygulamalarından Azure bölgelerine ölçmenize olanak tanır ve Yönlendirme kararları verirken Trafik Yöneticisi'nin de bu bilgileri dikkate almalarını sağlar. Gerçek Kullanıcı Ölçümlerini kullanmayı seçerek, son kullanıcılarınızın bulunduğu ağlardan gelen istekler için yönlendirmenin doğruluğunu artırabilirsiniz. 

## <a name="how-real-user-measurements-work"></a>Gerçek Kullanıcı Ölçümleri nasıl çalışır?

Gerçek Kullanıcı Ölçümleri, istemci uygulamalarınızın kullanıldığı son kullanıcı ağlarından görüldüğü gibi Azure bölgelerine gecikme süresiölçmesini sağlayarak çalışır. Örneğin, farklı konumlardaki kullanıcılar tarafından erişilen bir web sayfanız varsa (örneğin, Kuzey Amerika bölgelerinde), bunları sunucunuzun bulunduğu en iyi Azure bölgesine ulaşmak için performans yönlendirme yöntemiyle Gerçek Kullanıcı Ölçümleri'ni kullanabilirsiniz uygulama barındırılır.

Web sayfalarınıza Azure sağlanan Bir JavaScript (içinde benzersiz bir anahtar bulunan) katıştırarak başlar. Bu yapıldıktan sonra, bir kullanıcı bu web sayfasını ziyaret ettiğinde, JavaScript, ölçmesi gereken Azure bölgeleri hakkında bilgi almak için Trafik Yöneticisi'ni sorgular. Hizmet, bu Azure bölgelerinde barındırılan tek bir piksel görüntüsünü indirerek ve isteğin gönderildiği saat ile ilk baytın alındığı saat arasındaki gecikmeyi belirterek bu bölgeleri art arda ölçen komut dosyasına bir dizi uç nokta döndürür . Bu ölçümler daha sonra Trafik Yöneticisi servisine geri bildirilir.

Zaman içinde bu, birçok kez ve Trafik Yöneticisi'nin son kullanıcılarınızın bulunduğu ağların gecikme özellikleri hakkında daha doğru bilgi edinmesine yol açan birçok ağda gerçekleşir. Bu bilgiler Trafik Yöneticisi tarafından alınan yönlendirme kararlarına dahil olmaya başlar. Sonuç olarak, gönderilen Gerçek Kullanıcı Ölçümlerine dayalı olarak bu kararlarda doğruluğun artmasına neden olur.

Gerçek Kullanıcı Ölçümleri kullandığınızda, Trafik Yöneticisi'ne gönderilen ölçüm sayısına göre faturalandırılırsınız. Fiyatlandırma hakkında daha fazla bilgi için [Trafik Yöneticisi fiyatlandırma sayfasını](https://azure.microsoft.com/pricing/details/traffic-manager/)ziyaret edin.

## <a name="faqs"></a>SSS

* [Gerçek Kullanıcı Ölçümlerini kullanmanın faydaları nelerdir?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#what-are-the-benefits-of-using-real-user-measurements)

* [Azure olmayan bölgelerde Gerçek Kullanıcı Ölçümleri kullanabilir miyim?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#can-i-use-real-user-measurements-with-non-azure-regions)

* [Gerçek Kullanıcı Ölçümlerinden hangi yönlendirme yöntemi yararlanır?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#which-routing-method-benefits-from-real-user-measurements)

* [Her profili ayrı ayrı Gerçek Kullanıcı Ölçümleri'ne etkinleştirmem gerekiyor mu?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#do-i-need-to-enable-real-user-measurements-each-profile-separately)

* [Aboneliğim için Gerçek Kullanıcı Ölçümlerini nasıl kapatırım?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#how-do-i-turn-off-real-user-measurements-for-my-subscription)

* [Gerçek Kullanıcı Ölçümlerini web sayfaları dışındaki istemci uygulamalarıyla kullanabilir miyim?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#can-i-use-real-user-measurements-with-client-applications-other-than-web-pages)

* [Gerçek Kullanıcı Ölçümlerim etkinleştirildiğinde web sayfam da kaç ölçüm yapılır?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#how-many-measurements-are-made-each-time-my-real-user-measurements-enabled-web-page-is-rendered)

* [Gerçek Kullanıcı Ölçümleri komut dosyası web sayfamda çalıştırmadan önce bir gecikme var mı?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#is-there-a-delay-before-real-user-measurements-script-runs-in-my-webpage)

* [Gerçek Kullanıcı Ölçümlerini yalnızca ölçmek istediğim Azure bölgeleriyle kullanabilir miyim?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#can-i-use-real-user-measurements-with-only-the-azure-regions-i-want-to-measure)

* [Yapılan ölçüm sayısını belirli bir sayıyla sınırlayabilir miyim?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#can-i-limit-the-number-of-measurements-made-to-a-specific-number)

* [Müşteri başvurum tarafından yapılan ölçümleri Gerçek Kullanıcı Ölçümleri kapsamında görebilir miyim?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#can-i-see-the-measurements-taken-by-my-client-application-as-part-of-real-user-measurements)

* [Trafik Yöneticisi tarafından sağlanan ölçüm komut dosyasını değiştirebilir miyim?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#can-i-modify-the-measurement-script-provided-by-traffic-manager)

* [Başkalarının Gerçek Kullanıcı Ölçümleri ile kullandığım anahtarı görmesi mümkün olacak mı?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#will-it-be-possible-for-others-to-see-the-key-i-use-with-real-user-measurements)

* [Başkaları rum anahtarımı kötüye kullanabilir mi?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#can-others-abuse-my-rum-key)

* [Tüm web sayfalarıma javascript ölçümü koymam gerekiyor mu?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#do-i-need-to-put-the-measurement-javascript-in-all-my-web-pages)

* [Gerçek Kullanıcı Ölçümleri kullanırsam son kullanıcılarla ilgili bilgiler Trafik Yöneticisi tarafından tanımlanabilir mi?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#can-information-about-my-end-users-be-identified-by-traffic-manager-if-i-use-real-user-measurements)

* [Gerçek Kullanıcı Ölçümlerini ölçen web sayfasının yönlendirme için Trafik Yöneticisi'ni kullanması gerekiyor mu?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#does-the-webpage-measuring-real-user-measurements-need-to-be-using-traffic-manager-for-routing)

* [Gerçek Kullanıcı Ölçümleri ile kullanmak için Azure bölgelerinde herhangi bir hizmeti barındırmam gerekiyor mu?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#do-i-need-to-host-any-service-on-azure-regions-to-use-with-real-user-measurements)

* [Gerçek Kullanıcı Ölçümleri'ni kullandığımda Azure bant genişliği kullanımım artar mı?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#will-my-azure-bandwidth-usage-increase-when-i-use-real-user-measurements)

## <a name="next-steps"></a>Sonraki adımlar
- Web sayfaları [ile Gerçek Kullanıcı Ölçümlerini](traffic-manager-create-rum-web-pages.md) nasıl kullanacağınızı öğrenin
- [Trafik Yöneticisi'nin nasıl çalıştığını](traffic-manager-overview.md) öğrenin
- [Mobile Center](https://docs.microsoft.com/mobile-center/) hakkında daha fazla bilgi edinin
- Trafik Yöneticisi tarafından desteklenen [trafik yönlendirme yöntemleri](traffic-manager-routing-methods.md) hakkında daha fazla bilgi edinin
- Trafik Yöneticisi profilini nasıl [oluşturabilirsiniz](traffic-manager-create-profile.md) öğrenin

