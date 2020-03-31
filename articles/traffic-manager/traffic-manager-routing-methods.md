---
title: Azure Trafik Yöneticisi - trafik yönlendirme yöntemleri
description: Bu makaleler, Trafik Yöneticisi tarafından kullanılan farklı trafik yönlendirme yöntemlerini anlamanıza yardımcı olur
services: traffic-manager
author: rohinkoul
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/17/2018
ms.author: rohink
ms.openlocfilehash: 4a035506943eeffa2c3fc4fec27c47da4136683b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79250913"
---
# <a name="traffic-manager-routing-methods"></a>Traffic Manager yönlendirme yöntemleri

Azure Trafik Yöneticisi, ağ trafiğini çeşitli hizmet uç noktalarına nasıl yönlendireceklerini belirlemek için altı trafik yönlendirme yöntemini destekler. Herhangi bir profil için Trafik Yöneticisi, bu profille ilişkili trafik yönlendirme yöntemini aldığı her DNS sorgusuna uygular. Trafik yönlendirme yöntemi, DNS yanıtında hangi bitiş noktasının döndürüldİğİni belirler.

Aşağıdaki trafik yönlendirme yöntemleri Trafik Yöneticisi mevcuttur:

* **[Öncelik](#priority-traffic-routing-method):** Tüm trafik için birincil hizmet bitiş noktası kullanmak istediğinizde **Önceliği** seçin ve birincil veya yedek uç noktalarının kullanılamaması durumunda yedekler sağlayın.
* **[Ağırlıklı](#weighted):** Trafiği bitiş noktaları kümesine eşit olarak veya tanımladığınız ağırlıklara göre dağıtmak istediğinizde **Ağırlıklı'yı** seçin.
* **[Performans](#performance):** Farklı coğrafi konumlarda uç noktanız olduğunda ve son kullanıcıların en düşük ağ gecikmesi açısından "en yakın" bitiş noktasını kullanmasını istediğinizde **Performans'ı** seçin.
* **[Coğrafi](#geographic):** Kullanıcıların DNS sorgularının hangi coğrafi konumdan kaynaklandığına bağlı olarak belirli uç noktalara (Azure, Harici veya İç Içe) yönlendirilen **Coğrafi** konumu seçin. Bu, Trafik Yöneticisi müşterilerinin, bir kullanıcının coğrafi bölgesini bilmenin ve bunları buna göre yönlendirmenin önemli olduğu senaryoları etkinleştirme lerini sağlar. Örnekler arasında veri egemenliği zorunluluklarına uymak, içeriğin yerelleştirilmesi & kullanıcı deneyimi ve farklı bölgelerden gelen trafiğin ölçülmesi verilebilir.
* **[Çok değer](#multivalue):** Yalnızca IPv4/IPv6 adresi uç nokta olarak sahip olabilecek Trafik Yöneticisi profilleri için **MultiValue'ı** seçin. Bu profil için bir sorgu alındığı zaman, tüm sağlıklı uç noktaları döndürülür.
* **[Alt Ağ](#subnet):** Son kullanıcı IP adresi aralıklarının kümelerini Trafik Yöneticisi profilinde belirli bir uç noktayla eşlemek için **Subnet** trafik yönlendirme yöntemini seçin. Bir istek alındığı zaman, döndürülen bitiş noktası, isteğin kaynak IP adresi için eşlenen nokta olacaktır. 


Tüm Trafik Yöneticisi profilleri uç nokta sağlık ve otomatik bitiş noktası failover izleme içerir. Daha fazla bilgi için [Trafik Yöneticisi Uç Nokta İzleme'ye](traffic-manager-monitoring.md)bakın. Tek bir Trafik Yöneticisi profili yalnızca bir trafik yönlendirme yöntemini kullanabilir. Profiliniz için istediğiniz zaman farklı bir trafik yönlendirme yöntemi seçebilirsiniz. Değişiklikler bir dakika içinde uygulanır ve hiçbir kapalı kalma süresi tahakkuk etmez. Trafik yönlendirme yöntemleri iç içe trafik yöneticisi profilleri kullanılarak birleştirilebilir. İç içe geçme, daha büyük ve karmaşık uygulamaların gereksinimlerini karşılayan gelişmiş ve esnek trafik yönlendirme yapılandırmaları sağlar. Daha fazla bilgi için [iç içe olan Trafik Yöneticisi profillerine](traffic-manager-nested-profiles.md)bakın.

## <a name="priority-traffic-routing-method"></a>Öncelikli trafik yönlendirme yöntemi

Genellikle bir kuruluş, birincil hizmetlerinin düşmesi durumunda bir veya daha fazla yedekleme hizmeti dağıtarak hizmetleri için güvenilirlik sağlamak ister. 'Öncelik' trafik yönlendirme yöntemi, Azure müşterilerinin bu hata değiştirme desenini kolayca uygulamasına olanak tanır.

![Azure Trafik Yöneticisi 'Öncelik' trafik yönlendirme yöntemi](media/traffic-manager-routing-methods/priority.png)

Traffic Manager profili, önceliği belirlenmiş hizmet uç noktalarının bir listesini içerir. Traffic Manager varsayılan olarak tüm trafiği birincil (en yüksek öncelikli) uç noktaya gönderir. Birincil uç noktanın kullanılabilir olmaması halinde Traffic Manager, trafiği ikinci uç noktaya yönlendirir. Hem birincil hem de ikincil uç noktaların kullanılabilir olmaması durumunda trafik üçüncüye gider ve bu böyle devam eder. Uç noktanın kullanılabilirliği, yapılandırılan duruma (etkin veya devre dışı) ve devam eden uç nokta izleme durumuna göre belirlenir.

### <a name="configuring-endpoints"></a>Uç noktaları yapılandırma

Azure Kaynak Yöneticisi ile, her bitiş noktası için 'öncelik' özelliğini kullanarak bitiş noktası önceliğini açıkça yapılandırırsınız. Bu özellik 1 ile 1000 arasında bir değerdir. Daha düşük değerler daha yüksek bir önceliği temsil ediyor. Uç noktalar öncelik değerlerini paylaşamaz. Özelliği ayarlamak isteğe bağlıdır. Atlandığında, uç nokta sırasına dayalı varsayılan bir öncelik kullanılır.

## <a name="weighted-traffic-routing-method"></a><a name = "weighted"></a>Ağırlıklı trafik yönlendirme yöntemi
'Ağırlıklı' trafik yönlendirme yöntemi, trafiği eşit olarak dağıtmanıza veya önceden tanımlanmış bir ağırlıklandırma kullanmanıza olanak tanır.

![Azure Trafik Yöneticisi 'Ağırlıklı' trafik yönlendirme yöntemi](media/traffic-manager-routing-methods/weighted.png)

Ağırlıklı trafik yönlendirme yönteminde, Trafik Yöneticisi profil yapılandırmasındaki her bitiş noktasına bir ağırlık atarsınız. Ağırlık 1-1000 arasında bir tamsayıdır. Bu parametre isteğe bağlıdır. Atlanırsa, Trafik Yöneticileri varsayılan '1' ağırlığı kullanır. Daha yüksek ağırlık, yüksek öncelik.

Traffic Manager, alınan her DNS sorgusu için rastgele olarak kullanılabilir durumdaki bir uç noktayı seçer. Bir uç noktanın seçilme ihtimali, kullanılabilir durumdaki tüm uç noktalara atanmış olan ağırlıklara bağlıdır. Tüm uç noktalarda aynı ağırlığı kullanmak eşit bir trafik dağılımına neden olur. Belirli uç noktalarda daha yüksek veya daha düşük ağırlıkların kullanılması, bu uç noktaların DNS yanıtlarında daha sık veya daha az döndürülmesine neden olur.

Ağırlıklı yöntem bazı yararlı senaryolar sağlar:

* Aşamalı uygulama yükseltmesi: Yeni bir bitiş noktasına yönlendirilen bir trafik yüzdesini ayırın ve zaman içinde trafiği kademeli olarak %100'e yükseltin.
* Azure'a uygulama geçişi: Hem Azure hem de dış uç noktaları olan bir profil oluşturun. Yeni uç noktaları tercih etmek için uç noktaların ağırlığını ayarlayın.
* Ek kapasite için bulut patlaması: Trafik Yöneticisi profilinin arkasına koyarak şirket içi dağıtımı buluta hızla genişletin. Bulutta fazladan kapasiteye ihtiyacınız olduğunda, daha fazla uç nokta ekleyebilir veya etkinleştirebilir ve trafiğin her bitiş noktasına hangi bölümünün gittiğini belirtebilirsiniz.

Azure portalını kullanmanın yanı sıra, Azure PowerShell, CLI ve REST API'lerini kullanarak ağırlıkları yapılandırabilirsiniz.

DNS yanıtlarının istemciler ve istemcilerin DNS adlarını çözmek için kullandıkları özyinelemeli DNS sunucuları tarafından önbelleğe alınmış olduğunu anlamak önemlidir. Bu önbelleğe alma ağırlıklı trafik dağılımları üzerinde bir etkisi olabilir. İstemci ve özyinelemeli DNS sunucularının sayısı büyükolduğunda, trafik dağıtımı beklendiği gibi çalışır. Ancak, istemci veya özyinelemeli DNS sunucularının sayısı küçükse, önbelleğe alma trafik dağılımını önemli ölçüde çarpıtabilir.

Yaygın kullanım örnekleri şunlardır:

* Geliştirme ve test ortamları
* Uygulama-uygulama iletişimi
* Ortak özyinelemeli DNS altyapılarını paylaşan dar bir kullanıcı tabanına yönelik uygulamalar (örneğin, şirket çalışanları bir proxy üzerinden bağlanır)

Bu DNS önbelleğe alma efektleri, sadece Azure Trafik Yöneticisi'nin değil, tüm DNS tabanlı trafik yönlendirme sistemlerinde yaygındır. Bazı durumlarda, DNS önbelleğinin açıkça temizlenmesi geçici çözüm sağlayabilir. Diğer durumlarda, alternatif bir trafik yönlendirme yöntemi daha uygun olabilir.

## <a name="performance-traffic-routing-method"></a><a name = "performance"></a>Performans trafiği yönlendirme yöntemi

Uç noktaları dünya çapında iki veya daha fazla konumda dağıtmak, trafiği size 'en yakın' konuma yönlendirme yaparak birçok uygulamanın yanıt verme yeteneğini artırabilir. 'Performans' trafik yönlendirme yöntemi bu özelliği sağlar.

![Azure Trafik Yöneticisi 'Performans' trafik yönlendirme yöntemi](media/traffic-manager-routing-methods/performance.png)

Coğrafi uzaklıkla ölçülen en yakın uç nokta mutlaka en yakın değildir. Bunun yerine, 'Performans' trafik yönlendirme yöntemi ağ gecikmesini ölçerek en yakın bitiş noktasını belirler. Traffic Manager, IP adres aralıkları ile her Azure veri merkezi arasındaki gidiş-dönüş süresini izlemek için bir Internet Gecikme Tablosu tutar.

Trafik Yöneticisi, Internet Gecikme Tablosu'nda gelen DNS isteğinin kaynak IP adresini arar. Trafik Yöneticisi daha sonra Azure veri merkezinde, bu IP adresi aralığı için en düşük gecikme süreye sahip kullanılabilir bir bitiş noktası seçer ve DNS yanıtındaki bitiş noktasını döndürür.

[Trafik Yöneticisi Nasıl Çalışır'da](traffic-manager-how-it-works.md)açıklandığı gibi, Trafik Yöneticisi doğrudan istemcilerden DNS sorguları almaz. Bunun yerine, DNS sorguları istemcilerin kullanmak üzere yapılandırılan özyinelemeli DNS hizmetinden gelir. Bu nedenle, 'en yakın' bitiş noktasını belirlemek için kullanılan IP adresi istemcinin IP adresi değil, özyinelemeli DNS hizmetinin IP adresidir. Uygulamada, bu IP adresi istemci için iyi bir proxy'dir.


Trafik Yöneticisi, genel Internet ve yeni Azure bölgelerindeki değişiklikleri hesaba katmak için Internet Gecikme Tablosu'nu düzenli olarak güncelleştirir. Ancak, uygulama performansı Internet'teki gerçek zamanlı yük değişimlerine bağlı olarak değişir. Performans trafiği yönlendirmesi, belirli bir hizmet bitiş noktasındaki yükü izlemez. Ancak, bir bitiş noktası kullanılamıyorsa, Trafik Yöneticisi bunu DNS sorgu yanıtlarına eklemez.


Dikkat edilmesi gereken noktalar:

* Profiliniz aynı Azure bölgesinde birden çok uç nokta içeriyorsa, Trafik Yöneticisi trafiği o bölgedeki kullanılabilir uç noktalara eşit olarak dağıtır. Bir bölge içinde farklı bir trafik dağılımı nı tercih ederseniz, [iç içe olan Trafik Yöneticisi profillerini](traffic-manager-nested-profiles.md)kullanabilirsiniz.
* En yakın Azure bölgesindeki tüm etkin uç noktalar bozulursa, Trafik Yöneticisi trafiği sonraki en yakın Azure bölgesindeki bitiş noktalarına taşır. Tercih edilen bir başarısızlık dizisi tanımlamak istiyorsanız, [iç içe trafik yöneticisi profillerini](traffic-manager-nested-profiles.md)kullanın.
* Dış uç noktaları veya iç içe uç noktaları ile Performans trafik yönlendirme yöntemini kullanırken, bu uç noktaların konumunu belirtmeniz gerekir. Dağıtımınıza en yakın Azure bölgesini seçin. Bu konumlar, Internet Gecikme Tablosu tarafından desteklenen değerlerdir.
* Bitiş noktasını seçen algoritma belirleyicidir. Aynı istemciden yinelenen DNS sorguları aynı bitiş noktasına yönlendirilir. Genellikle, istemciler seyahat ederken farklı özyinelemeli DNS sunucuları kullanır. İstemci farklı bir bitiş noktasına yönlendirilebilir. Yönlendirme, Internet Gecikme Tablosu'ndaki güncelleştirmelerden de etkilenebilir. Bu nedenle, Performans trafiği yönlendirme yöntemi istemcinin her zaman aynı bitiş noktasına yönlendirildiğini garanti etmez.
* Internet Gecikme Tablosu değiştiğinde, bazı istemcilerin farklı bir bitiş noktasına yönlendirilmiş olduğunu fark edebilirsiniz. Bu yönlendirme değişikliği, geçerli gecikme sonu verilerine göre daha doğrudur. Bu güncelleştirmeler, Internet sürekli geliştikçe Performans trafiği yönlendirmesinin doğruluğunu korumak için gereklidir.

## <a name="geographic-traffic-routing-method"></a><a name = "geographic"></a>Coğrafi trafik yönlendirme yöntemi

Trafik Yöneticisi profilleri, kullanıcıların DNS sorgularının hangi coğrafi konumdan kaynaklandığına bağlı olarak belirli uç noktalara (Azure, Dış veya İç Içe) yönlendirilen Coğrafi yönlendirme yöntemini kullanacak şekilde yapılandırılabilir. Bu, Trafik Yöneticisi müşterilerinin, bir kullanıcının coğrafi bölgesini bilmenin ve bunları buna göre yönlendirmenin önemli olduğu senaryoları etkinleştirme lerini sağlar. Örnekler arasında veri egemenliği zorunluluklarına uymak, içeriğin yerelleştirilmesi & kullanıcı deneyimi ve farklı bölgelerden gelen trafiğin ölçülmesi verilebilir.
Bir profil coğrafi yönlendirme için yapılandırıldığınızda, bu profille ilişkili her bitiş noktasının, profilin atanmış bir coğrafi bölgeleri kümesine sahip olması gerekir. Bir coğrafi bölge parçalılık aşağıdaki düzeylerde olabilir 
- Dünya- herhangi bir bölge
- Bölgesel Gruplama – örneğin, Afrika, Orta Doğu, Avustralya/Pasifik vb. 
- Ülke / Bölge - örneğin, İrlanda, Peru, Hong Kong SAR vb. 
- Eyalet/Eyalet – örneğin, ABD-Kaliforniya, Avustralya-Queensland, Kanada-Alberta vb.(not: bu parçalılık düzeyi yalnızca Avustralya, Kanada ve ABD'deki eyaletler/ eyaletler için desteklenir).

Bir bölge veya bölge kümesi bitiş noktasına atandığında, bu bölgelerden gelen istekler yalnızca bu bitiş noktasına yönlendirilir. Trafik Yöneticisi, bir kullanıcının sorguladığı bölgeyi belirlemek için DNS sorgusunun kaynak IP adresini kullanır – genellikle bu, kullanıcı adına sorgu yapan yerel DNS çözümleyicisinin IP adresidir.  

![Azure Trafik Yöneticisi 'Coğrafi' trafik yönlendirme yöntemi](./media/traffic-manager-routing-methods/geographic.png)

Trafik Yöneticisi, DNS sorgusunun kaynak IP adresini okur ve hangi coğrafi bölgeden geldiğine karar verir. Daha sonra bu coğrafi bölge eşlenen bir bitiş noktası olup olmadığını görmek için görünüyor. Bu arama en düşük parçalılık düzeyinde başlar (Devlet / Eyalet nerede desteklenir, Diğer Ülke / Bölge düzeyinde) ve **dünya**olan en yüksek seviyeye kadar tüm yol gider. Bu geçiş kullanılarak bulunan ilk eşleşme, sorgu yanıtında döndürülmek için bitiş noktası olarak belirlenir. İç içe bir tür bitiş noktasıyla eşleştirildiğinde, yönlendirme yöntemine göre bu alt profildeki bir uç nokta döndürülür. Aşağıdaki noktalar bu davranış için geçerlidir:

- Yönlendirme türü Coğrafi Yönlendirme olduğunda, coğrafi bölge Trafik Yöneticisi profilinde yalnızca bir bitiş noktasına eşlenebilir. Bu, kullanıcıların yönlendirmesinin belirleyici olmasını sağlar ve müşteriler kesin coğrafi sınırlar gerektiren senaryoları etkinleştirebilir.
- Bir kullanıcının bölgesi iki farklı uç noktanın coğrafi eşlemesi altında bulunuyorsa, Trafik Yöneticisi bitiş noktasını en düşük parçalı olan noktayı seçer ve o bölgeden diğer uç noktaya yönlendirme isteklerini dikkate almaz. Örneğin, iki uç noktası olan bir Coğrafi Yönlendirme türü profilini düşünün - Endpoint1 ve Endpoint2. Endpoint1 İrlanda'dan trafik almak için yapılandırılmıştır ve Endpoint2 Avrupa'dan trafik almak için yapılandırılmıştır. Bir istek İrlanda'dan geliyorsa, her zaman Endpoint1'e yönlendirilir.
- Bir bölge yalnızca bir bitiş noktasına eşlenebileceğinden, Trafik Yöneticisi bitiş noktasının sağlıklı olup olmadığına bakılmaksızın onu döndürür.

    >[!IMPORTANT]
    >Coğrafi yönlendirme yöntemini kullanan müşterilerin, her birinde en az iki uç nokta içeren alt profilleri olan İç Içe Geçmiş tür uç noktalarıyla ilişkilendirmeleri önerilir.
- Bir bitiş noktası eşleşmesi bulunursa ve bu bitiş noktası **Durduruldu** durumundaysa, Trafik Yöneticisi bir NODATA yanıtı döndürür. Bu durumda, coğrafi bölge hiyerarşisinde daha yüksek arama yapılmaz. Bu davranış, alt profil Durduruldu veya **Devre Dışı** **bırakılmış** durumdayken iç içe geçen uç nokta türleri için de geçerlidir.
- Bir bitiş noktası **Devre Dışı bırakılmış** durumu görüntülerse, bölge eşleştirme işlemine dahil olmaz. Bu davranış, son nokta **Devre Dışı bırakma** durumunda iç içe geçen uç nokta türleri için de geçerlidir.
- Bu profilde eşleme olmayan bir coğrafi bölgeden bir sorgu geliyorsa, Trafik Yöneticisi bir NODATA yanıtı döndürür. Bu nedenle, müşterilerin coğrafi yönlendirmeyi, ideal olarak alt profil içinde en az iki uç noktayla iç içe geçmiş bir türle, **dünya** bölgesinin ona atanmış olduğu bir uç noktayla kullanmaları kuvvetle önerilir. Bu, bir bölgeye eş olmayan IP adreslerinin de işlenmesini sağlar.

[Trafik Yöneticisi Nasıl Çalışır'da](traffic-manager-how-it-works.md)açıklandığı gibi, Trafik Yöneticisi doğrudan istemcilerden DNS sorguları almaz. Bunun yerine, DNS sorguları istemcilerin kullanmak üzere yapılandırılan özyinelemeli DNS hizmetinden gelir. Bu nedenle, bölgeyi belirlemek için kullanılan IP adresi istemcinin IP adresi değil, özyinelemeli DNS hizmetinin IP adresidir. Uygulamada, bu IP adresi istemci için iyi bir proxy'dir.

### <a name="faqs"></a>SSS

* [Coğrafi yönlendirmenin yararlı olduğu bazı kullanım durumları nelerdir?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#what-are-some-use-cases-where-geographic-routing-is-useful)

* [Performans yönlendirme yöntemini mi yoksa Coğrafi yönlendirme yöntemini mi kullanmam gerektiğine nasıl karar veririm?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#how-do-i-decide-if-i-should-use-performance-routing-method-or-geographic-routing-method)

* [Coğrafi yönlendirme için Trafik Yöneticisi tarafından desteklenen bölgeler nelerdir?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#what-are-the-regions-that-are-supported-by-traffic-manager-for-geographic-routing)

* [Trafik yöneticisi, bir kullanıcının nereden sorgu yaptığını nasıl belirler?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#how-does-traffic-manager-determine-where-a-user-is-querying-from)

* [Trafik Yöneticisi'nin her durumda kullanıcının tam coğrafi konumunu doğru bir şekilde belirleyebileceği garanti midir?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#is-it-guaranteed-that-traffic-manager-can-correctly-determine-the-exact-geographic-location-of-the-user-in-every-case)

* [Coğrafi yönlendirme için yapılandırıldığı noktayla aynı bölgede bir uç noktanın fiziksel olarak bulunması gerekir mi?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#does-an-endpoint-need-to-be-physically-located-in-the-same-region-as-the-one-it-is-configured-with-for-geographic-routing)

* [Coğrafi yönlendirme yapmak üzere yapılandırılmamış bir profildeki uç noktalara coğrafi bölgeler atayabilir miyim?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#can-i-assign-geographic-regions-to-endpoints-in-a-profile-that-is-not-configured-to-do-geographic-routing)

* [Varolan bir profilin yönlendirme yöntemini Coğrafi olarak değiştirmeye çalıştığımda neden hata alıyorum?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#why-am-i-getting-an-error-when-i-try-to-change-the-routing-method-of-an-existing-profile-to-geographic)

* [Neden müşterilerin coğrafi yönlendirme etkin leştirilmiş bir profilin altında uç nokta yerine iç içe profiller oluşturması şiddetle tavsiye edilir?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#why-is-it-strongly-recommended-that-customers-create-nested-profiles-instead-of-endpoints-under-a-profile-with-geographic-routing-enabled)

* [Bu yönlendirme türünü destekleyen API sürümünde herhangi bir kısıtlama var mı?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#are-there-any-restrictions-on-the-api-version-that-supports-this-routing-type)

## <a name="multivalue-traffic-routing-method"></a><a name = "multivalue"></a>Çok değerli trafik yönlendirme yöntemi
**Çok değerli** trafik yönlendirme yöntemi, tek bir DNS sorgu yanıtında birden çok sağlıklı uç nokta elde etmenizi sağlar. Bu, döndürülen bir uç noktanın yanıt vermemiş olması durumunda, arayanın istemci tarafı yeniden denemelerini diğer uç noktalarla yapmasını sağlar. Bu düzen bir hizmetin kullanılabilirliğini artırabilir ve iyi durumdaki bir uç noktayı alma amacıyla yapılacak yeni DNS sorgusundan kaynaklanan gecikme süresini azaltabilir. MultiValue yönlendirme yöntemi yalnızca 'Harici' türünün tüm uç noktaları ve IPv4 veya IPv6 adresleri olarak belirtilirse çalışır. Bu profil için bir sorgu alındığı zaman, tüm sağlıklı uç noktalar döndürülür ve yapılandırılabilir maksimum iade sayısına tabi tutulur.

### <a name="faqs"></a>SSS

* [MultiValue yönlendirmenin yararlı olduğu bazı kullanım örnekleri nelerdir?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#what-are-some-use-cases-where-multivalue-routing-is-useful)

* [MultiValue yönlendirmesi kullanıldığında kaç uç nokta döndürülür?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#how-many-endpoints-are-returned-when-multivalue-routing-is-used)

* [MultiValue yönlendirmesi kullanıldığında aynı uç nokta kümesini alacak mıyım?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#will-i-get-the-same-set-of-endpoints-when-multivalue-routing-is-used)

## <a name="subnet-traffic-routing-method"></a><a name = "subnet"></a>Subnet trafik yönlendirme yöntemi
**Subnet** trafik yönlendirme yöntemi, bir profildeki belirli uç noktalarla bir dizi son kullanıcı IP adresi aralığını eşlemenize olanak tanır. Bundan sonra, Trafik Yöneticisi bu profil için bir DNS sorgusu alırsa, bu isteğin kaynak IP adresini inceler (çoğu durumda bu, arayan tarafından kullanılan DNS çözümleyicisinin giden IP adresi olacaktır), eşlendiğini ve döndürüleceğini belirler sorgu yanıtında bu bitiş noktası. 

Bir uç noktaya eşlenecek IP adresi CIDR aralıkları (örn. 1.2.3.0/24) veya adres aralığı (örn. 1.2.3.4-5.6.7.8) olarak belirtilebilir. Bir bitiş noktasıyla ilişkili IP aralıklarının bu profil içinde benzersiz olması gerekir ve aynı profildeki farklı bir bitiş noktasının IP adresi kümesiyle çakışmaz.
Adres aralığı olmayan bir bitiş noktası tanımlarsanız, bu bir geri dönüş işlevi görür ve kalan alt ağlardan trafik alır. Geri dönüş bitiş noktası dahil değilse, Trafik Yöneticisi tanımlanmamış aralıklar için bir NODATA yanıtı gönderir. Bu nedenle, bir geri dönüş bitiş noktası tanımlamanız veya olası tüm IP aralıklarının uç noktalarınızda belirtildiğinden emin olmak önerilir.

Alt ağ yönlendirmesi, belirli bir IP alanından bağlanan kullanıcılar için farklı bir deneyim sunmak için kullanılabilir. Örneğin bir müşteri alt ağ yönlendirme seçeneğini kullanarak kurumsal ofislerinden yapılan tüm isteklerin, uygulamanın yalnızca şirket içi kullanıma açık bir sürümünü test ettikleri farklı bir uç noktaya yönlendirilmesini sağlayabilir. Başka bir senaryo da belirli bir ISP'den bağlanan kullanıcılara farklı bir deneyim sağlamak olabilir (belirli bir ISP'den bağlanan kullanıcıları engellemek gibi).

### <a name="faqs"></a>SSS

* [Alt ağ yönlendirmenin yararlı olduğu bazı kullanım durumları nelerdir?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#what-are-some-use-cases-where-subnet-routing-is-useful)

* [Trafik Yöneticisi son kullanıcının IP adresini nasıl biliyor?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#how-does-traffic-manager-know-the-ip-address-of-the-end-user)

* [Subnet yönlendirmekullanırken IP adreslerini nasıl belirtebilirim?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#how-can-i-specify-ip-addresses-when-using-subnet-routing)

* [Subnet yönlendirmekullanırken bir geri dönüş bitiş noktasını nasıl belirtebilirim?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#how-can-i-specify-a-fallback-endpoint-when-using-subnet-routing)

* [Subnet yönlendirme türü profilinde bir bitiş noktası devre dışı bırakılırsa ne olur?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#what-happens-if-an-endpoint-is-disabled-in-a-subnet-routing-type-profile)


## <a name="next-steps"></a>Sonraki adımlar

[Trafik Yöneticisi uç nokta izlemesini](traffic-manager-monitoring.md) kullanarak yüksek kullanılabilirlik uygulamalarının nasıl geliştirilenöğrenin




