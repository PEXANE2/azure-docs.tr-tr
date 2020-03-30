---
title: Azure Trafik Yöneticisi - SSS
description: Bu makalede Trafik Yöneticisi hakkında sık sorulan soruların yanıtları sağlar
services: traffic-manager
documentationcenter: ''
author: rohinkoul
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/26/2019
ms.author: rohink
ms.openlocfilehash: acdac6e3eafc5251ebd31a34bcb9a4db34f0ebbe
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79254371"
---
# <a name="traffic-manager-frequently-asked-questions-faq"></a>Trafik Yöneticisi Sık Sorulan Sorular (SSS)

## <a name="traffic-manager-basics"></a>Trafik Yöneticisi temelleri

### <a name="what-ip-address-does-traffic-manager-use"></a>Trafik Yöneticisi hangi IP adresini kullanır?

[Trafik Yöneticisi Nasıl Çalışır](../traffic-manager/traffic-manager-how-it-works.md)açıklandığı gibi, Trafik Yöneticisi DNS düzeyinde çalışır. İstemcileri uygun hizmet bitiş noktasına yönlendirmek için DNS yanıtları gönderir. İstemciler daha sonra Servis Yöneticisi aracılığıyla değil, doğrudan hizmet bitiş noktasına bağlanır.

Bu nedenle, Trafik Yöneticisi istemcilerin bağlanabilmek için bir bitiş noktası veya IP adresi sağlamaz. Hizmetiniz için statik IP adresi istiyorsanız, bu adres Trafik Yöneticisi'nde değil, hizmette yapılandırılmalıdır.

### <a name="what-types-of-traffic-can-be-routed-using-traffic-manager"></a>Trafik Yöneticisi kullanılarak ne tür trafik yönlendirilebilir?
[Trafik Yöneticisi Nasıl Çalışır'da](../traffic-manager/traffic-manager-how-it-works.md)açıklandığı gibi, Trafik Yöneticisi bitiş noktası Azure'un içinde veya dışında barındırılan herhangi bir internet eki hizmet olabilir. Bu nedenle, Trafik Yöneticisi de internet karşı karşıya olan uç noktaları bir dizi kamu internet kaynaklanan trafik yönlendirebilirsiniz. Özel bir ağda bulunan uç noktalarınız (örneğin, [Azure Yük Bakiyesi'nin](../load-balancer/concepts-limitations.md#internalloadbalancer)dahili sürümü) varsa veya kullanıcıların bu tür dahili ağlardan DNS istekleri yapması varsa, bu trafiği yönlendirmek için Trafik Yöneticisi'ni kullanamazsınız.

### <a name="does-traffic-manager-support-sticky-sessions"></a>Trafik Yöneticisi "yapışkan" oturumları destekliyor mu?

[Trafik Yöneticisi Nasıl Çalışır](../traffic-manager/traffic-manager-how-it-works.md)açıklandığı gibi, Trafik Yöneticisi DNS düzeyinde çalışır. İstemcileri uygun hizmet bitiş noktasına yönlendirmek için DNS yanıtlarını kullanır. İstemciler, Servis Yöneticisi aracılığıyla değil, doğrudan servis bitiş noktasına bağlanır. Bu nedenle, Trafik Yöneticisi istemci ve sunucu arasındaki HTTP trafiğini görmez.

Ayrıca, Trafik Yöneticisi tarafından alınan DNS sorgusunun kaynak IP adresi istemciye değil, özyinelemeli DNS hizmetine aittir. Bu nedenle, Trafik Yöneticisi tek tek istemcileri izlemek için bir yolu yoktur ve 'yapışkan' oturumları uygulayamaz. Bu sınırlama tüm DNS tabanlı trafik yönetim sistemlerinde yaygındır ve Trafik Yöneticisi'ne özgü değildir.

### <a name="why-am-i-seeing-an-http-error-when-using-traffic-manager"></a>Trafik Yöneticisi'ni kullanırken neden bir HTTP hatası görüyorum?

[Trafik Yöneticisi Nasıl Çalışır](../traffic-manager/traffic-manager-how-it-works.md)açıklandığı gibi, Trafik Yöneticisi DNS düzeyinde çalışır. İstemcileri uygun hizmet bitiş noktasına yönlendirmek için DNS yanıtlarını kullanır. İstemciler daha sonra Servis Yöneticisi aracılığıyla değil, doğrudan hizmet bitiş noktasına bağlanır. Trafik Yöneticisi istemci ve sunucu arasındaki HTTP trafiğini görmez. Bu nedenle, gördüğünüz herhangi bir HTTP hatası uygulamanızdan geliyor olmalıdır. İstemcinin uygulamaya bağlanması için tüm DNS çözüm adımları tamamlanır. Trafik Yöneticisi'nin uygulama trafiği akışı yla ilgili herhangi bir etkileşimi içerir.

Daha fazla araştırma bu nedenle uygulama üzerinde durulmalıdır.

İstemcinin tarayıcısından gönderilen HTTP ana bilgisayar üstbilgisi en yaygın sorun kaynağıdır. Uygulamanın, kullanmakta olduğunuz etki alanı adı için doğru ana bilgisayar üstbilgisini kabul edecek şekilde yapılandırıldığından emin olun. Azure Uygulama Hizmeti'ni kullanan son noktalar [için, Trafik Yöneticisi'ni kullanarak Azure Uygulama Hizmeti'ndeki bir web uygulaması için özel bir etki alanı adı yapılandırmaya](../app-service/configure-domain-traffic-manager.md)bakın.

### <a name="what-is-the-performance-impact-of-using-traffic-manager"></a>Trafik Yöneticisi'ni kullanmanın performans etkisi nedir?

[Trafik Yöneticisi Nasıl Çalışır](../traffic-manager/traffic-manager-how-it-works.md)açıklandığı gibi, Trafik Yöneticisi DNS düzeyinde çalışır. İstemciler doğrudan hizmet uç noktalarınıza bağlandığından, bağlantı kurulduktan sonra Trafik Yöneticisi'ni kullanırken oluşan performans etkisi olmaz.

Traffic Manager DNS düzeyindeki uygulamalarla tümleştiğinden, DNS çözüm zincirine ek bir DNS araması eklenmesi gerekir. Trafik Yöneticisi'nin DNS çözünürlük süresi üzerindeki etkisi çok azdır. Traffic Manager, ad sunucularından oluşan genel bir ağ kullanır ve DNS sorgularının her zaman kullanılabilir en yakın ad sunucusuna yönlendirilmesini sağlamak için [anycast](https://en.wikipedia.org/wiki/Anycast) ağlarını kullanır. Buna ek olarak, DNS yanıtlarının önbelleğe aldaması, Trafik Yöneticisi kullanılarak ortaya çıkan ek DNS gecikmesinin yalnızca oturumların bir kısmı için geçerli olduğu anlamına gelir.

Performans yöntemi trafiği kullanılabilir en yakın bitiş noktasına yönlendirir. Net sonuç, bu yöntemle ilişkili genel performans etkisinin en az olması gerektiğidir. DNS gecikmesindeki herhangi bir artış, bitiş noktasına daha düşük ağ gecikmesi ile dengelenmelidir.

### <a name="what-application-protocols-can-i-use-with-traffic-manager"></a>Trafik Yöneticisi ile hangi uygulama protokollerini kullanabilirim?

[Trafik Yöneticisi Nasıl Çalışır](../traffic-manager/traffic-manager-how-it-works.md)açıklandığı gibi, Trafik Yöneticisi DNS düzeyinde çalışır. DNS araması tamamlandıktan sonra, istemciler uygulama bitiş noktasına Trafik Yöneticisi aracılığıyla değil, doğrudan bağlanır. Bu nedenle, bağlantı herhangi bir uygulama protokolü kullanabilirsiniz. İzleme protokolü olarak TCP'yi seçerseniz, Trafik Yöneticisi'nin uç nokta durumu izlemesi herhangi bir uygulama protokolü kullanmadan yapılabilir. Bir uygulama protokolü kullanarak sistem durumunun doğrulanmasını seçerseniz, bitiş noktasının HTTP veya HTTPS GET isteklerine yanıt verebilmesi gerekir.

### <a name="can-i-use-traffic-manager-with-a-naked-domain-name"></a>Trafik Yöneticisi'ni "çıplak" alan adı ile kullanabilir miyim?

Evet. Azure Trafik Yöneticisi profiline başvurmak için alan adınız için bir diğer ad kaydının nasıl oluşturuldestekleyeceğini öğrenmek için, [Trafik Yöneticisi ile apeks alan adlarını desteklemek için bir takma ad kaydı yapılandır'a](../dns/tutorial-alias-tm.md)bakın.

### <a name="does-traffic-manager-consider-the-client-subnet-address-when-handling-dns-queries"></a>Trafik Yöneticisi, DNS sorgularını işlerken istemci alt net adresini dikkate alır mı? 

Evet, aldığı DNS sorgusunun kaynak IP adresine (genellikle DNS çözümleyicisinin IP adresidir) ek olarak, Coğrafi, Performans ve Subnet yönlendirme yöntemleriiçin arama lar gerçekleştirirken, trafik yöneticisi istemci alt net adresini de dikkate alır son kullanıcı adına istekte bulunan çözümleyici tarafından sorguya dahil edilir.  
Özellikle, [DNS Sorguları'ndaki RFC 7871 – Client Subnet,](https://tools.ietf.org/html/rfc7871) istemci alt net adresini destekleyen çözücülerden geçirebilen [DNS (EDNS0) için](https://tools.ietf.org/html/rfc2671) bir Uzantı Mekanizması sağlar.

### <a name="what-is-dns-ttl-and-how-does-it-impact-my-users"></a>DNS TTL nedir ve kullanıcılarımı nasıl etkiler?

Bir DNS sorgusu Trafik Yöneticisi'ne düştüğünde, zaman-to-live (TTL) adlı yanıtta bir değer ayarlar. Birimi saniye cinsinden olan bu değer, bu yanıtı ne kadar süreyle önbelleğe almak için DNS çözümleyicilerine aşağı doğru olduğunu gösterir. DNS çözümleyicilerinin bu sonucu önbelleğe almaları garanti edilmezken, önbelleğe alma, Trafik Yöneticisi DNS sunucularına gitmek yerine önbellekten sonraki sorgulara yanıt vermelerini sağlar. Bu, yanıtları aşağıdaki gibi etkiler:

- daha yüksek bir TTL, Traffic Manager DNS sunucularına inen sorgu sayısını azaltır, bu da sunulan sorgu sayısı faturalandırılabilir bir kullanım olduğundan müşterinin maliyetini azaltabilir.
- daha yüksek bir TTL, DNS araması yapmak için gereken süreyi potansiyel olarak azaltabilir.
- daha yüksek bir TTL, verilerinizin Trafik Yöneticisi'nin sondalama aracıları aracılığıyla elde ettiği en son sağlık bilgilerini yansıtmadığı anlamına da gelir.

### <a name="how-high-or-low-can-i-set-the-ttl-for-traffic-manager-responses"></a>Trafik Yöneticisi yanıtları için TTL'yi ne kadar yüksek veya düşük ayarlayabilirim?

Profil başına DNS TTL'yi 0 saniye ve 2.147.483.647 saniye [(RFC-1035](https://www.ietf.org/rfc/rfc1035.txt )ile uyumlu maksimum aralık) kadar düşük olarak ayarlayabilirsiniz. 0 TTL, akış aşağı DNS çözümleyicilerinin sorgu yanıtlarını önbelleğe almaması ve tüm sorguların çözüm için Trafik Yöneticisi DNS sunucularına ulaşmasının beklendiği anlamına gelir.

### <a name="how-can-i-understand-the-volume-of-queries-coming-to-my-profile"></a>Profilime gelen sorguların hacmini nasıl anlayabilirim? 

Trafik Yöneticisi tarafından sağlanan ölçümlerden biri, bir profil tarafından yanıtlanan sorgu sayısıdır. Bu bilgileri profil düzeyinde toplama dan alabilirsiniz veya belirli uç noktalarıdöndürüldüğü sorguların hacmini görmek için daha fazla bölebilirsiniz. Ayrıca, sorgu yanıt hacmi belirlediğiniz koşulları geçerse sizi bilgilendirmek için uyarılar ayarlayabilirsiniz. Daha fazla bilgi [için, Trafik Yöneticisi ölçümleri ve uyarıları](traffic-manager-metrics-alerts.md).

## <a name="traffic-manager-geographic-traffic-routing-method"></a>Trafik Yöneticisi Coğrafi trafik yönlendirme yöntemi

### <a name="what-are-some-use-cases-where-geographic-routing-is-useful"></a>Coğrafi yönlendirmenin yararlı olduğu bazı kullanım durumları nelerdir?

Coğrafi yönlendirme türü, Azure müşterisinin kullanıcılarını coğrafi bölgelere göre ayırt etmesi gereken her senaryoda kullanılabilir. Örneğin, Coğrafi trafik yönlendirme yöntemini kullanarak, belirli bölgelerdeki kullanıcılara diğer bölgelerdekikullanıcılardan farklı bir kullanıcı deneyimi verebilirsiniz. Başka bir örnek, belirli bir bölgedeki kullanıcılara yalnızca o bölgedeki uç noktalarla hizmet sunulmasını gerektiren yerel veri egemenlik zorunluluklarına uymaktır.

### <a name="how-do-i-decide-if-i-should-use-performance-routing-method-or-geographic-routing-method"></a>Performans yönlendirme yöntemini mi yoksa Coğrafi yönlendirme yöntemini mi kullanmam gerektiğine nasıl karar veririm?

Bu iki popüler yönlendirme yöntemi arasındaki temel fark, Performans yönlendirme yönteminde birincil amacınızın trafiği arayanın en düşük gecikme sini sağlayabilecek bitiş noktasına göndermek olmasıdır, oysa Coğrafi yönlendirmede birincil hedef bir coğrafi arayanlarınız için çit, böylece onları kasten belirli bir bitiş noktasına yönlendirebilirsiniz. Bu her zaman doğru olmasa da, coğrafi yakınlık ve düşük gecikme arasında bir korelasyon olduğundan çakışma gerçekleşir. Arayan için daha iyi bir gecikme deneyimi sağlayabilecek farklı bir coğrafyada bir bitiş noktası olabilir ve bu durumda Performans yönlendirmesi kullanıcıyı bu uç noktaya gönderir, ancak Coğrafi yönlendirme her zaman onları eşlediğiniz bitiş noktasına gönderir coğrafi bölge. Daha da açık olmak için, aşağıdaki örneği göz önünde bulundurun - Coğrafi yönlendirme ile Asya'dan ABD'deki uç noktalara tüm trafiği göndermek ve Asya'daki tüm ABD trafiğini son noktalara göndermek gibi nadir olmayan eşlemeler yapabilirsiniz. Bu durumda, Coğrafi yönlendirme kasıtlı olarak tam olarak ne yapmak ve performans optimizasyonu yapmak için yapılandırdığınız şeyi yapacak bir göz değildir. 
>[!NOTE]
>Bu senaryolar iç içe profiller için hem performans hem de coğrafi yönlendirme yeteneklerine ihtiyaç duyabileceğiniz senaryolar olabilir. Örneğin, Kuzey Amerika'daki tüm trafiği ABD'de uç noktaları olan iç içe yönelik bir profile gönderdiğiniz coğrafi yönlendirmeiçeren bir üst profil ayarlayabilir ve bu trafiği bu kümedeki en iyi bitiş noktasına göndermek için performans yönlendirmesini kullanabilirsiniz. 

### <a name="what-are-the-regions-that-are-supported-by-traffic-manager-for-geographic-routing"></a>Coğrafi yönlendirme için Trafik Yöneticisi tarafından desteklenen bölgeler nelerdir?

Trafik Yöneticisi tarafından kullanılan ülke/bölge hiyerarşisini [burada](traffic-manager-geographic-regions.md)bulabilirsiniz. Bu sayfa herhangi bir değişiklikle güncel tutulurken, [Azure Trafik Yöneticisi REST API'yi](https://docs.microsoft.com/rest/api/trafficmanager/)kullanarak aynı bilgileri programlı olarak da alabilirsiniz. 

### <a name="how-does-traffic-manager-determine-where-a-user-is-querying-from"></a>Trafik yöneticisi, bir kullanıcının nereden sorgu yaptığını nasıl belirler?

Trafik Yöneticisi sorgunun kaynağı IP bakar (bu büyük olasılıkla kullanıcı adına sorgu yapan yerel bir DNS çözümleyici) ve konumu belirlemek için bölge haritası için bir iç IP kullanır. Bu harita, internetteki değişiklikleri hesaba katmak için sürekli olarak güncelleştirilir. 

### <a name="is-it-guaranteed-that-traffic-manager-can-correctly-determine-the-exact-geographic-location-of-the-user-in-every-case"></a>Trafik Yöneticisi'nin her durumda kullanıcının tam coğrafi konumunu doğru bir şekilde belirleyebileceği garanti midir?

Hayır, Trafik Yöneticisi, bir DNS sorgusunun kaynak IP adresinden çıkardığımız coğrafi bölgenin aşağıdaki nedenlerden dolayı her zaman kullanıcının konumuna karşılık olacağını garanti edemez:

- İlk olarak, önceki SSS açıklandığı gibi, gördüğümüz kaynak IP adresi kullanıcı adına arama yapan bir DNS çözümleyici olduğunu. DNS çözümleyicisinin coğrafi konumu kullanıcının coğrafi konumu için iyi bir proxy olsa da, DNS çözümleyici hizmetinin ayak izine ve müşterinin kullanmayı seçtiği belirli DNS çözümleyici hizmetine bağlı olarak da farklı olabilir. Örnek olarak, Malezya'da bulunan bir müşteri, cihazlarının ayarlarını belirtebilir ve Singapur'daki DNS sunucusu söz kullanıcı/aygıt için sorgu çözümleme çözümünü işlemek üzere seçilebilen bir DNS çözümleyici hizmeti kullanabilir. Bu durumda, Trafik Yöneticisi yalnızca Çözümleyicinin Singapur konumuna karşılık gelen IP adresini görebilir. Ayrıca, bu sayfada istemci alt net adresi desteği ile ilgili önceki SSS bakın.

- İkinci olarak, Trafik Yöneticisi coğrafi bölge çevirisi için IP adresini yapmak için dahili bir harita kullanır. Bu harita doğruluğunu artırmak ve internetin gelişen doğası için hesap sürekli olarak doğrulanmış ve güncellenir iken, bilgilerimizin tüm IP coğrafi konumunun tam bir temsili olmaması hala olasılığı vardır Adres.

###  <a name="does-an-endpoint-need-to-be-physically-located-in-the-same-region-as-the-one-it-is-configured-with-for-geographic-routing"></a>Coğrafi yönlendirme için yapılandırıldığı noktayla aynı bölgede bir uç noktanın fiziksel olarak bulunması gerekir mi?

Hayır, bitiş noktasının konumu, hangi bölgelerin eşlenebileceğine herhangi bir kısıtlama getirmez. Örneğin, ABD-Orta Azure bölgesindeki bir bitiş noktası, Hindistan'daki tüm kullanıcıların bu bölgeye yönlendirilen olmasını sağlayabilir.

### <a name="can-i-assign-geographic-regions-to-endpoints-in-a-profile-that-is-not-configured-to-do-geographic-routing"></a>Coğrafi yönlendirme yapmak üzere yapılandırılmamış bir profildeki uç noktalara coğrafi bölgeler atayabilir miyim?

Evet, bir profilin yönlendirme yöntemi coğrafi değilse, bu profildeki uç noktalara coğrafi bölgeler atamak için [Azure Trafik Yöneticisi REST API'yi](https://docs.microsoft.com/rest/api/trafficmanager/) kullanabilirsiniz. Coğrafi olmayan yönlendirme türü profilleri durumunda, bu yapılandırma yoksayılır. Böyle bir profili daha sonra coğrafi yönlendirme türüyle değiştirirseniz, Trafik Yöneticisi bu eşlemeleri kullanabilir.

### <a name="why-am-i-getting-an-error-when-i-try-to-change-the-routing-method-of-an-existing-profile-to-geographic"></a>Varolan bir profilin yönlendirme yöntemini Coğrafi olarak değiştirmeye çalıştığımda neden hata alıyorum?

Coğrafi yönlendirmeiçeren bir profilin altındaki tüm uç noktaların en az bir bölgenin eşlenmiş olması gerekir. Varolan bir profili coğrafi yönlendirme türüne dönüştürmek için, yönlendirme türünü coğrafi olarak değiştirmeden önce Azure [Trafik Yöneticisi REST API'yi](https://docs.microsoft.com/rest/api/trafficmanager/) kullanarak coğrafi bölgeleri tüm uç noktalarıyla ilişkilendirmeniz gerekir. Portalı kullanıyorsanız, önce uç noktaları silin, profilin yönlendirme yöntemini coğrafi olarak değiştirin ve ardından uç noktaları coğrafi bölge eşlemeleriyle birlikte ekleyin.

### <a name="why-is-it-strongly-recommended-that-customers-create-nested-profiles-instead-of-endpoints-under-a-profile-with-geographic-routing-enabled"></a>Neden müşterilerin coğrafi yönlendirme etkin leştirilmiş bir profilin altında uç nokta yerine iç içe profiller oluşturması şiddetle tavsiye edilir?

Coğrafi yönlendirme yöntemini kullanıyorsa, bölge profil içinde yalnızca bir bitiş noktasına atanabilir. Bu uç nokta, alt profili eklenmiş iç içe bir tür değilse, bu bitiş noktası sağlıksız gidiyorsa, Trafik Yöneticisi herhangi bir trafik göndermeme alternatifi daha iyi olmadığından, trafik yöneticisi ona trafik göndermeye devam ediyor. Trafik Yöneticisi başka bir bitiş noktasına kadar başarısız olmaz, atanan bölge sağlıksız giden bitiş noktasına atanan bölgenin bir "ebeveyni" olsa bile (örneğin, İspanya bölgesi olan bir bitiş noktası sağlıksız giderse, başka bir bitiş noktasına başarısız olmayız bölge Avrupa ona atanmış) vardır. Bu, Trafik Yöneticisi'nin müşterinin profilinde kurduğu coğrafi sınırlara saygı göstermesini sağlamak için yapılır. Bir bitiş noktası sağlıksız gittiğinde başka bir uç noktaya geçmeavantajını elde etmek için, coğrafi bölgelerin tek tek uç noktaları yerine içinde birden çok uç nokta bulunan iç içe dönük profillere atanması önerilir. Bu şekilde, iç içe geçen alt profildeki bir bitiş noktası başarısız olursa, trafik aynı iç içe geçen alt profilin içindeki başka bir bitiş noktasına geçemez.

### <a name="are-there-any-restrictions-on-the-api-version-that-supports-this-routing-type"></a>Bu yönlendirme türünü destekleyen API sürümünde herhangi bir kısıtlama var mı?

Evet, yalnızca 2017-03-01 VE yeni sürüm Coğrafi yönlendirme türünü destekler. Daha eski API sürümleri, Coğrafi yönlendirme türüprofillerini oluşturmada veya coğrafi bölgeleri uç noktalara atamak için kullanılamaz. Azure aboneliğinden profilleri almak için eski bir API sürümü kullanılıyorsa, Coğrafi yönlendirme türüne ait herhangi bir profil döndürülmez. Ayrıca, eski API sürümlerini kullanırken, coğrafi bölge ataması olan uç noktaları olan herhangi bir profilin coğrafi bölge ataması gösterilmez.

## <a name="traffic-manager-subnet-traffic-routing-method"></a>Trafik Yöneticisi Subnet trafik yönlendirme yöntemi

### <a name="what-are-some-use-cases-where-subnet-routing-is-useful"></a>Alt ağ yönlendirmenin yararlı olduğu bazı kullanım durumları nelerdir?

Alt ağ yönlendirme, DNS istekleri IP adresinin kaynak IP'si tarafından tanımlanan belirli kullanıcı kümeleri için sağladığınız deneyimi ayırt etmenizi sağlar. Kullanıcılar şirket merkezinizden bir web sitesine bağlanıyorsa, bir örnek farklı içerik göstermek tir. Bir diğeri, belirli ISS'lerden gelen kullanıcıları yalnızca IPv6 kullanıldığında alt par performansına sahipse yalnızca IPv4 bağlantılarını destekleyen uç noktalara erişmekiçin kısıtlamak olacaktır.
Subnet yönlendirme yöntemini kullanmanın bir diğer nedeni de iç içe geçen profil kümesindeki diğer profillerle birlikte olmasıdır. Örneğin, kullanıcılarınızı coğrafi eskrim için Coğrafi yönlendirme yöntemini kullanmak, ancak belirli bir ISS için farklı bir yönlendirme yöntemi yapmak istiyorsanız, ana profil olarak bir profil ile Subnet yönlendirme yöntemine sahip olabilirsiniz ve iss'in belirli bir alt öğeyi kullanmasını geçersiz kılabilirsiniz profili ne olursa o kadar çok kişi var ve herkes için standart Coğrafi profile sahip.

### <a name="how-does-traffic-manager-know-the-ip-address-of-the-end-user"></a>Trafik Yöneticisi son kullanıcının IP adresini nasıl biliyor?

Son kullanıcı aygıtları genellikle dns aramasını kendi adlarına yapmak için bir DNS çözümleyicisi kullanır. Bu tür çözücülerin giden IP'si, Trafik Yöneticisi'nin kaynak IP olarak gördüğü şeydir. Buna ek olarak, Subnet yönlendirme yöntemi de istek ile geçirilen EDNS0 Genişletilmiş İstemci Subnet (ECS) bilgi olup olmadığını görmek için görünüyor. ECS bilgileri varsa, yönlendirmeyi belirlemek için kullanılan adres tir. ECS bilgilerinin yokluğunda, sorgunun kaynak IP'si yönlendirme amacıyla kullanılır.

### <a name="how-can-i-specify-ip-addresses-when-using-subnet-routing"></a>Subnet yönlendirmekullanırken IP adreslerini nasıl belirtebilirim?

Bitiş noktasıyla ilişkilendirilecek IP adresleri iki şekilde belirtilebilir. İlk olarak, aralığı belirtmek için (örneğin, 1.2.3.4-5.6.7.8 veya 3.4.5.6-3.4.5.6) bir başlangıç ve bitiş adresleri ile dört noktalı ondalık sekizli gösterimi kullanabilirsiniz. İkinci olarak, aralığı belirtmek için CIDR gösterimini kullanabilirsiniz (örneğin, 1.2.3.0/24). Birden çok aralık belirtebilir ve aralık kümesinde her iki gösterim türünü de kullanabilirsiniz. Birkaç kısıtlama uygulanır.

-    Her IP'nin yalnızca tek bir bitiş noktasına eşlemesi gerektiğinden, adres aralıklarının çakışması olamaz
-    Başlangıç adresi bitiş adresinden fazla olamaz
-    CIDR gösterimi durumunda, '/' öncesi IP adresi bu aralığın başlangıç adresi olmalıdır (örneğin, 1.2.3.0/24 geçerlidir ancak 1.2.3.4.4/24 geçerli DeğİlDİr)

### <a name="how-can-i-specify-a-fallback-endpoint-when-using-subnet-routing"></a>Subnet yönlendirmekullanırken bir geri dönüş bitiş noktasını nasıl belirtebilirim?

Subnet yönlendirmesi olan bir profilde, üzerinde alt ağ eşlenmemiş bir bitiş noktanız varsa, diğer uç noktalarla eşleşmeyen tüm istek buraya yönlendirilir. Trafik Yöneticisi bir istek gelirse ve herhangi bir uç noktaya eşlenmez veya bir bitiş noktasına eşlenir, ancak bu bitiş noktası sağlıksız sayılsa, profilinizde böyle bir geri dönüş bitiş noktası olması önerilir.

### <a name="what-happens-if-an-endpoint-is-disabled-in-a-subnet-routing-type-profile"></a>Subnet yönlendirme türü profilinde bir bitiş noktası devre dışı bırakılırsa ne olur?

Subnet yönlendirmesi olan bir profilde, bu devre dışı bırakılmış bir bitiş noktanız varsa, Trafik Yöneticisi bu uç nokta ve sahip olduğu alt net eşlemeleri yokmuş gibi çalışır. IP adresi eşlemesiyle eşleşen bir sorgu alınırsa ve bitiş noktası devre dışı bırakılırsa, Trafik Yöneticisi bir geri dönüş bitiş noktası (eşlemesi olmayan bir sorgu) döndürecek veya böyle bir bitiş noktası yoksa, bir NXDOMAIN yanıtı döndürecektir.

## <a name="traffic-manager-multivalue-traffic-routing-method"></a>Trafik Yöneticisi MultiValue trafik yönlendirme yöntemi

### <a name="what-are-some-use-cases-where-multivalue-routing-is-useful"></a>MultiValue yönlendirmenin yararlı olduğu bazı kullanım örnekleri nelerdir?

MultiValue yönlendirme, tek bir sorgu yanıtında birden çok sağlıklı uç noktasını döndürür. Bunun en büyük avantajı, bir bitiş noktası sağlıksız ise, istemci başka bir DNS araması yapmadan yeniden denemek için daha fazla seçenek olmasıdır (bir yukarı önbellekten aynı değeri döndürebilir). Bu, kapalı kalma süresini en aza indirmek isteyen kullanılabilirlik duyarlı uygulamalar için geçerlidir.
MultiValue yönlendirme yöntemiiçin başka bir kullanım bir bitiş noktası hem IPv4 ve IPv6 adresleri için "çift ev sahibi" ve bitiş noktasına bir bağlantı başlattığınızda seçim için arayan her iki seçenek vermek istiyorum.

### <a name="how-many-endpoints-are-returned-when-multivalue-routing-is-used"></a>MultiValue yönlendirmesi kullanıldığında kaç uç nokta döndürülür?

Döndürülecek en fazla uç nokta sayısını belirtebilirsiniz ve bir sorgu alındığı zaman MultiValue bu kadar çok sağlıklı uç noktadan daha fazla döndürmez. Bu yapılandırma için mümkün olan en yüksek değer 10'dur.

### <a name="will-i-get-the-same-set-of-endpoints-when-multivalue-routing-is-used"></a>MultiValue yönlendirmesi kullanıldığında aynı uç nokta kümesini alacak mıyım?

Her sorguda aynı uç nokta kümesinin döndürüleceğini garanti edemeyiz. Bu da bazı uç noktaların sağlıksız gidebilir ve bu noktada yanıta dahil edilmeyecektir gerçeğinden de etkilenir

## <a name="real-user-measurements"></a>Gerçek Kullanıcı Ölçümleri

### <a name="what-are-the-benefits-of-using-real-user-measurements"></a>Gerçek Kullanıcı Ölçümlerini kullanmanın faydaları nelerdir?

Performans yönlendirme yöntemini kullandığınızda, Traffic Manager, kaynak IP ve EDNS İstemci Alt Net'ini inceleyerek (aktarılırsa) ve hizmetin koruduğu ağ gecikme zekasıyla karşılaştırarak son kullanıcınızın bağlanması gereken en iyi Azure bölgesini seçer. Gerçek Kullanıcı Ölçümleri, deneyimlerinin bu gecikme tablosuna katkıda bulunmasını ve bu tablonun son kullanıcı larınızın Azure'a bağlandığı son kullanıcı ağlarını yeterli şekilde kapsadığını sağlayarak bunu son kullanıcı tabanınız için geliştirir. Bu, son kullanıcınızın yönlendirmesinde daha yüksek bir doğruluk sağlar.

### <a name="can-i-use-real-user-measurements-with-non-azure-regions"></a>Azure olmayan bölgelerde Gerçek Kullanıcı Ölçümleri kullanabilir miyim?

Gerçek Kullanıcı Ölçümleri, Azure bölgelerine ulaşmak için yalnızca gecikme gecikmesini ölçer ve raporlar. Azure olmayan bölgelerde barındırılan uç noktalarıyla performans tabanlı yönlendirme kullanıyorsanız, bu bitiş noktasıyla ilişkilendirilmeye yönelik olarak seçtiğiniz temsili Azure bölgesi hakkında daha fazla gecikme sonu bilgisi ne olursa olsun bu özellikten yararlanmaya devam edebilirsiniz.

### <a name="which-routing-method-benefits-from-real-user-measurements"></a>Gerçek Kullanıcı Ölçümlerinden hangi yönlendirme yöntemi yararlanır?

Gerçek Kullanıcı Ölçümleri ile elde edilen ek bilgiler yalnızca performans yönlendirme yöntemini kullanan profiller için geçerlidir. Gerçek Kullanıcı Ölçümleri bağlantısı, Azure portalı üzerinden görüntülediğinizde tüm profillerden edinilebilir.

### <a name="do-i-need-to-enable-real-user-measurements-each-profile-separately"></a>Her profili ayrı ayrı Gerçek Kullanıcı Ölçümleri'ne etkinleştirmem gerekiyor mu?

Hayır, abonelik başına yalnızca bir kez etkinleştirmeniz gerekir ve ölçülen ve bildirilen tüm gecikme bilgileri tüm profiller tarafından kullanılabilir.

### <a name="how-do-i-turn-off-real-user-measurements-for-my-subscription"></a>Aboneliğim için Gerçek Kullanıcı Ölçümlerini nasıl kapatırım?

Müşteri uygulamanızdan gecikme süresi ölçümleri toplamayı ve geri göndermeyi bıraktığınızda Gerçek Kullanıcı Ölçümleri ile ilgili ücret tahakkuk etmeyi durdurabilirsiniz. Örneğin, web sayfalarına katıştırılmış javascript ölçümü yaparken, JavaScript'i kaldırarak veya sayfa işlendiğinde çağrısını kapatarak bu özelliği kullanmayı durdurabilirsiniz.

Ayrıca anahtarınızı silerek Gerçek Kullanıcı Ölçümleri'ni kapatabilirsiniz. Anahtarı sildikten sonra, bu anahtarla Trafik Yöneticisi'ne gönderilen ölçümler atılır.

### <a name="can-i-use-real-user-measurements-with-client-applications-other-than-web-pages"></a>Gerçek Kullanıcı Ölçümlerini web sayfaları dışındaki istemci uygulamalarıyla kullanabilir miyim?

Evet, Gerçek Kullanıcı Ölçümleri, farklı türde son kullanıcı istemcileri aracılığıyla toplanan verileri almak üzere tasarlanmıştır. Bu SSS, yeni istemci uygulama türleri desteklendikçe güncelleştirilir.

### <a name="how-many-measurements-are-made-each-time-my-real-user-measurements-enabled-web-page-is-rendered"></a>Gerçek Kullanıcı Ölçümlerim etkinleştirildiğinde web sayfam da kaç ölçüm yapılır?

JavaScript'in sağladığı ölçümle Gerçek Kullanıcı Ölçümleri kullanıldığında, her sayfa nın işlenmesi altı ölçüm alınmasıyla sonuçlanır. Bunlar daha sonra Trafik Yöneticisi hizmetine geri bildirilir. Trafik Yöneticisi hizmetine bildirilen ölçüm sayısına bağlı olarak bu özellik için ücretlendirilirsiniz. Örneğin, ölçümler alınırken ancak raporedilmeden önce kullanıcı web sayfanızdan uzaklaşıyorsa, bu ölçümler faturalandırma amacıyla dikkate alınmaz.

### <a name="is-there-a-delay-before-real-user-measurements-script-runs-in-my-webpage"></a>Gerçek Kullanıcı Ölçümleri komut dosyası web sayfamda çalıştırmadan önce bir gecikme var mı?

Hayır, komut dosyası çağrılmadan önce programlanmış bir gecikme yok.

### <a name="can-i-use-real-user-measurements-with-only-the-azure-regions-i-want-to-measure"></a>Gerçek Kullanıcı Ölçümlerini yalnızca ölçmek istediğim Azure bölgeleriyle kullanabilir miyim?

Hayır, her çağrıldığınızda, Gerçek Kullanıcı Ölçümleri komut dosyası, hizmet tarafından belirlenen altı Azure bölgesi kümesini ölçer. Bu küme, farklı çağırmalar arasında değişiklik yapılır ve bu tür çağrıların çok sayıda gerçekleştiğinde, ölçüm kapsamı farklı Azure bölgeleri arasında yayılır.

### <a name="can-i-limit-the-number-of-measurements-made-to-a-specific-number"></a>Yapılan ölçüm sayısını belirli bir sayıyla sınırlayabilir miyim?

JavaScript ölçümü web sayfanıza gömülüdür ve ne zaman başlatıp kullanmayı durdurabileceğiniz üzerinde tam kontrole devam elabilirsiniz. Trafik Yöneticisi hizmeti ölçülecek Azure bölgelerinin listesi için bir istek aldığı sürece, bir dizi bölge döndürülür.

### <a name="can-i-see-the-measurements-taken-by-my-client-application-as-part-of-real-user-measurements"></a>Müşteri başvurum tarafından yapılan ölçümleri Gerçek Kullanıcı Ölçümleri kapsamında görebilir miyim?

Ölçüm mantığı istemci uygulamanızdan çalıştırıladığından, gecikme ölçümlerini görmek de dahil olmak üzere neler olacağını tam olarak kontrol esiniz. Trafik Yöneticisi, aboneliğinize bağlı anahtar altında alınan ölçümlerin toplu görünümünü bildirmez.

### <a name="can-i-modify-the-measurement-script-provided-by-traffic-manager"></a>Trafik Yöneticisi tarafından sağlanan ölçüm komut dosyasını değiştirebilir miyim?

Web sayfanızda gömülü olan ları kontrol ederken, gecikmeleri doğru bir şekilde ölçtürünve raporladığından emin olmak için ölçüm komut dosyasında herhangi bir değişiklik yapmanızı şiddetle tavsiye ediyoruz.

### <a name="will-it-be-possible-for-others-to-see-the-key-i-use-with-real-user-measurements"></a>Başkalarının Gerçek Kullanıcı Ölçümleri ile kullandığım anahtarı görmesi mümkün olacak mı?

Ölçüm komut dosyasını bir web sayfasına katıştırdığınızda, başkalarının komut dosyasını ve Gerçek Kullanıcı Ölçümleri (RUM) anahtarınızı görmesi mümkün olacaktır. Ancak bu anahtarın abonelik kimliğinizden farklı olduğunu ve Trafik Yöneticisi tarafından yalnızca bu amaçla kullanılmak üzere oluşturulduğunu bilmek önemlidir. RUM anahtarınızı bilmek Azure hesap güvenliğinizi tehlikeye atmayacaktır.

### <a name="can-others-abuse-my-rum-key"></a>Başkaları rum anahtarımı kötüye kullanabilir mi?

Başkalarının azure'a yanlış bilgi göndermek için anahtarınızı kullanması mümkün olsa da, aldığımız diğer ölçümlerle birlikte dikkate alındığından birkaç yanlış ölçüm yönlendirmeyi değiştirmez. Anahtarlarınızı değiştirmeniz gerekiyorsa, eski anahtarın atıldığı noktada anahtarı yeniden oluşturabilirsiniz.

### <a name="do-i-need-to-put-the-measurement-javascript-in-all-my-web-pages"></a>Tüm web sayfalarıma javascript ölçümü koymam gerekiyor mu?

Gerçek Kullanıcı Ölçümleri, ölçüm sayısı arttıkça daha fazla değer sağlar. Bunu söyledikten sonra, tüm web sayfalarınıza mı yoksa seçilmiş birkaç sayfanıza mı koymanız gerektiğine karar vermek sizin kararınızdır. Tavsiyemiz, kullanıcının o sayfada beş saniye veya daha uzun süre kalması beklenen en çok ziyaret edilen sayfanıza koyarak başlamaktır.

### <a name="can-information-about-my-end-users-be-identified-by-traffic-manager-if-i-use-real-user-measurements"></a>Gerçek Kullanıcı Ölçümleri kullanırsam son kullanıcılarla ilgili bilgiler Trafik Yöneticisi tarafından tanımlanabilir mi?

Sağlanan ölçüm JavaScript kullanıldığında, Trafik Yöneticisi son kullanıcının istemci IP adresi ve kullandıkları yerel DNS çözümleyicisinin kaynak IP adresi ne görünürlük olacaktır. Trafik Yöneticisi, istemci IP adresini yalnızca ölçümleri gönderen son kullanıcıyı tanımlayabilmek için kesildikten sonra kullanır.

### <a name="does-the-webpage-measuring-real-user-measurements-need-to-be-using-traffic-manager-for-routing"></a>Gerçek Kullanıcı Ölçümlerini ölçen web sayfasının yönlendirme için Trafik Yöneticisi'ni kullanması gerekiyor mu?

Hayır, Trafik Yöneticisi'ni kullanmasına gerek yok. Trafik Yöneticisi yönlendirme tarafı Gerçek Kullanıcı Ölçümü bölümünden ayrı olarak çalışır ve her ikisi de aynı web özelliği olması harika bir fikir olsa da, onlar olması gerekmez.

### <a name="do-i-need-to-host-any-service-on-azure-regions-to-use-with-real-user-measurements"></a>Gerçek Kullanıcı Ölçümleri ile kullanmak için Azure bölgelerinde herhangi bir hizmeti barındırmam gerekiyor mu?

Hayır, gerçek kullanıcı ölçümleri için Azure'da sunucu tarafındaki herhangi bir bileşeni barındırmanız gerekmez. JavaScript ölçümü ve farklı Azure bölgelerinde çalıştıran hizmet tarafından indirilen tek piksellik görüntü Azure tarafından barındırılır ve yönetilir. 

### <a name="will-my-azure-bandwidth-usage-increase-when-i-use-real-user-measurements"></a>Gerçek Kullanıcı Ölçümleri'ni kullandığımda Azure bant genişliği kullanımım artar mı?

Önceki yanıtta da belirtildiği gibi, Gerçek Kullanıcı Ölçümleri'nin sunucu tarafındaki bileşenleri Azure'a aittir ve azure tarafından yönetilir. Bu, Gerçek Kullanıcı Ölçümleri kullandığınız için Azure bant genişliği kullanımınızın artmayacağı anlamına gelir. Bu, Azure'un ücretleri dışında herhangi bir bant genişliği kullanımını içermez. Bir Azure bölgesinin gecikme süresini ölçmek için yalnızca tek bir piksel görüntü indirerek kullanılan bant genişliğini en aza indiririz. 

## <a name="traffic-view"></a>Trafik Görünümü

### <a name="what-does-traffic-view-do"></a>Trafik Görünümü ne işe yarar?

Traffic View, Traffic Manager'ın kullanıcılarınız ve deneyimlerinin nasıl olduğunu daha iyi anlamanıza yardımcı olan bir özelliğidir. Trafik Yöneticisi tarafından alınan sorguları ve hizmetin size aşağıdakileri sağlamak için koruduğu ağ gecikme sayıltma tabloları kullanır:

- Kullanıcılarınızın Azure'daki uç noktalarınıza bağlandığı bölgeler.
- Bu bölgelerden bağlanan kullanıcıların hacmi.
- Yönlendirildikleri Azure bölgeleri.
- Bu Azure bölgelerine gecikme deneyimi.

Bu bilgiler, portaldaki coğrafi harita kaplaması ve tabular görünümler aracılığıyla ve indirebildiğiniz ham veri olarak kullanılabilmenize ek olarak, bunları tüketmeniz için kullanılabilir.

### <a name="how-can-i-benefit-from-using-traffic-view"></a>Trafik Görünümü'nden nasıl yararlanabilirim?

Trafik Görünümü, Trafik Yöneticisi profillerinizin aldığı trafiğin genel görünümünü sağlar. Özellikle, kullanıcı tabanınızın nereden bağlandığıve aynı derecede önemli ölçüde ortalama gecikme deneyiminin ne olduğunu anlamak için kullanılabilir. Daha sonra bu bilgileri, örneğin Azure ayak izinizi bu kullanıcılara daha düşük gecikme süreleriyle hizmet verebilecek bir bölgeye genişleterek odaklanmanız gereken alanları bulmak için kullanabilirsiniz. Trafik Görünümü'ni kullanarak elde edebilirsiniz başka bir bakış açısı, sırayla bu bölgelerde icat artan veya azalan karar vermenize yardımcı olabilir farklı bölgelere trafik desenleri görmektir.

### <a name="how-is-traffic-view-different-from-the-traffic-manager-metrics-available-through-azure-monitor"></a>Trafik Görünümü, Azure monitörü aracılığıyla kullanılabilen Trafik Yöneticisi ölçümlerinden nasıl farklı?

Azure Monitör, profiliniz ve uç noktaları tarafından alınan trafiği toplu bir düzeyde anlamak için kullanılabilir. Ayrıca, sağlık kontrolü sonuçlarını ortaya çıkararak uç noktaların sistem durumunu izlemenizi sağlar. Bunların ötesine geçmeniz ve son kullanıcınızın Azure'a bölgesel düzeyde bağlanma deneyimini anlamanız gerektiğinde, Trafik Görünümü bunu başarmak için kullanılabilir.

### <a name="does-traffic-view-use-edns-client-subnet-information"></a>Trafik Görünümü EDNS İstemci Alt net bilgilerini kullanıyor mu?

Azure Trafik Yöneticisi tarafından sunulan DNS sorguları, yönlendirmenin doğruluğunu artırmak için ECS bilgilerini dikkate alır. Ancak, kullanıcıların nereden bağladığını gösteren veri kümesini oluştururken, Trafik Görünümü yalnızca DNS çözümleyicisinin IP adresini kullanır.

### <a name="how-many-days-of-data-does-traffic-view-use"></a>Trafik Görünümü kaç günlük veri kullanır?

Trafik Görünümü, sizin tarafından görüntülendiğinde, önceki günden önceki yedi gündeki verileri işleyerek çıktısını oluşturur. Bu hareketli bir penceredir ve her ziyaretinizde en son veriler kullanılır.

### <a name="how-does-traffic-view-handle-external-endpoints"></a>Trafik Görünümü dış uç noktaları nasıl işler?

Trafik Yöneticisi profilinde Azure bölgeleri dışında barındırılan dış uç noktaları kullandığınızda, gecikme sonu özellikleriiçin bir proxy olan bir Azure bölgesine eşleninmeyi seçebilirsiniz (performans yönlendirme yöntemini kullanıyorsanız bunun gerekli olması gerekir). Bu Azure bölge eşleciliği varsa, Trafik Görünümü çıktısı oluşturulurken Azure bölgesinin gecikme ölçütleri kullanılır. Azure bölgesi belirtilmemişse, gecikme bilgileri bu dış uç noktaların verilerinde boş kalır.

### <a name="do-i-need-to-enable-traffic-view-for-each-profile-in-my-subscription"></a>Aboneliğimdeki her profil için Trafik Görünümü'ni etkinleştirmem gerekiyor mu?

Önizleme döneminde, Trafik Görünümü abonelik düzeyinde etkinleştirildi. Genel kullanılabilirlik ten önce yaptığımız iyileştirmelerin bir parçası olarak, artık trafik görünümünü profil düzeyinde etkinleştirerek bu özelliğin daha ayrıntılı olmasını sağlayabilirsiniz. Varsayılan olarak, Trafik Görünümü bir profil için devre dışı bırakılır.

>[!NOTE]
>Önizleme süresi boyunca bir abonelik düzeyinde Trafik Görünümü'ni etkinleştirdiyseniz, artık bu aboneliğin altındaki profillerin her biri için yeniden etkinleştirmeniz gerekir.
 
### <a name="how-can-i-turn-off-traffic-view"></a>Trafik Görünümü'ni nasıl kapatabilirim?

Portal veya REST API'yi kullanarak herhangi bir profil için Trafik Görünümü'nu kapatabilirsiniz. 

### <a name="how-does-traffic-view-billing-work"></a>Trafik Görünümü faturalandırma nasıl çalışır?

Trafik Görünümü fiyatlandırması, çıktıyı oluşturmak için kullanılan veri noktalarının sayısını temel alır. Şu anda desteklenen tek veri türü profilinizin aldığı sorgulardır. Ayrıca, yalnızca Trafik Görünümü etkinleştirildiğinde yapılan işlem için faturalandırılırsınız. Bu, Trafik Görünümü'ni bir ay içinde bir süre etkinleştirir ve diğer zamanlarda kapatırsanız, yalnızca faturanıza yönelik özelliği etkinleştirilmiş olarak işlenen veri noktaları nın işlenir.

## <a name="traffic-manager-endpoints"></a>Traffic Manager uç noktaları

### <a name="can-i-use-traffic-manager-with-endpoints-from-multiple-subscriptions"></a>Trafik Yöneticisi'ni birden çok abonelikten son puanla kullanabilir miyim?

Azure Web Apps ile birden çok abonelikten uç noktaları kullanmak mümkün değildir. Azure Web Apps, Web Apps ile kullanılan özel alan adının yalnızca tek bir abonelik içinde kullanılmasını gerektirir. Web Apps'ı aynı alan adı taşıyan birden çok abonelikten kullanmak mümkün değildir.

Diğer uç nokta türleri için, trafik yöneticisini birden fazla abonelikten uç noktalarıyla kullanmak mümkündür. Kaynak Yöneticisi'nde, Trafik Yöneticisi profilini yapılandıran kişi bitiş noktasına erişimi okuduğu sürece, herhangi bir abonelikten uç noktalar Trafik Yöneticisi'ne eklenebilir. Bu izinler [Azure Kaynak Yöneticisi rol tabanlı erişim denetimi (RBAC)](../role-based-access-control/role-assignments-portal.md)kullanılarak verilebilir.

### <a name="can-i-use-traffic-manager-with-cloud-service-staging-slots"></a>Trafik Yöneticisi'ni Bulut Hizmeti 'Evreleme' yuvalarıyla kullanabilir miyim?

Evet. Bulut Hizmeti 'evreleme' yuvaları Trafik Yöneticisi'nde Dış uç nokta olarak yapılandırılabilir. Sistem durumu denetimleri hala Azure Uç Noktaları oranında ücretlendirilir.

### <a name="does-traffic-manager-support-ipv6-endpoints"></a>Trafik Yöneticisi IPv6 uç noktalarını destekliyor mu?

Trafik Yöneticisi şu anda IPv6 adreslenebilir ad sunucuları sağlamaz. Ancak, Trafik Yöneticisi hala IPv6 uç noktalarına bağlanan IPv6 istemcileri tarafından kullanılabilir. İstemci DNS isteklerini doğrudan Trafik Yöneticisi'ne yapmaz. Bunun yerine, istemci özyinelemeli bir DNS hizmeti kullanır. Yalnızca IPv6 istemcisi, iPv6 üzerinden özyinelemeli DNS hizmetine istek gönderir. Daha sonra özyinelemeli hizmet IPv4 kullanarak Trafik Yöneticisi ad sunucuları na başvurabilmeli.

Trafik Yöneticisi bitiş noktasının DNS adı veya IP adresi ile yanıt verir. IPv6 bitiş noktasını desteklemek için iki seçenek vardır. Bitiş noktasını ilişkili bir AAAA kaydı olan bir DNS adı olarak ekleyebilirsiniz ve Trafik Yöneticisi bu bitiş noktasını sağlık denetimine eder ve sorgu yanıtında CNAME kayıt türü olarak döndürür. Ayrıca, iPv6 adresini kullanarak bu bitiş noktasını doğrudan ekleyebilirsiniz ve Trafik Yöneticisi sorgu yanıtında bir AAAA türü kaydı döndürecektir.

### <a name="can-i-use-traffic-manager-with-more-than-one-web-app-in-the-same-region"></a>Trafik Yöneticisi'ni aynı bölgede birden fazla Web Uygulamasıyla kullanabilir miyim?

Genellikle, Trafik Yöneticisi trafiği farklı bölgelerde dağıtılan uygulamalara yönlendirmek için kullanılır. Ancak, bir uygulamanın aynı bölgede birden fazla dağıtımı olduğunda da kullanılabilir. Trafik Yöneticisi Azure bitiş noktaları, aynı Azure bölgesinden birden fazla Web Uygulaması bitiş noktasının aynı Trafik Yöneticisi profiline eklenmesine izin vermez.

### <a name="how-do-i-move-my-traffic-manager-profiles-azure-endpoints-to-a-different-resource-group-or-subscription"></a>Trafik Yöneticisi profilimin Azure uç noktalarını farklı bir kaynak grubuna veya aboneye nasıl taşıyabilirim?

Trafik Yöneticisi profiliyle ilişkili Azure uç noktaları, kaynak kimliklerini kullanarak izlenir. Son nokta olarak kullanılmakta olan bir Azure kaynağı (örneğin, Genel IP, Klasik Bulut Hizmeti, WebApp veya iç içe doğru kullanılan başka bir Trafik Yöneticisi profili) farklı bir kaynak grubuna veya aboneliğe taşındığında, kaynak kimliği değişir. Bu senaryoda, şu anda, önce silerek ve sonra profilin uç noktalarını geri ekleyerek Trafik Yöneticisi profilini güncelleştirmeniz gerekir.

## <a name="traffic-manager-endpoint-monitoring"></a>Traffic Manager uç nokta izleme

### <a name="is-traffic-manager-resilient-to-azure-region-failures"></a>Trafik Yöneticisi Azure bölge hatalarına karşı dirençli mi?

Traffic Manager, Azure'da yüksek oranda kullanılabilen uygulamaların tesliminin önemli bir bileşenidir.
Yüksek kullanılabilirlik sağlamak için, Trafik Yöneticisi'nin olağanüstü yüksek düzeyde kullanılabilirlik düzeyine sahip olması ve bölgesel arızalara karşı dirençli olması gerekir.

Tasarım gereği, Traffic Manager bileşenleri herhangi bir Azure bölgesinin tamamen arızalanmasına karşı dayanıklıdır. Bu esneklik tüm Trafik Yöneticisi bileşenleri için geçerlidir: DNS ad sunucuları, API, depolama katmanı ve bitiş noktası izleme hizmeti.

Tüm Azure bölgesinde olası bir kesinti durumunda, Trafik Yöneticisi'nin normal şekilde çalışmaya devam etmesi beklenir. Birden çok Azure bölgesinde dağıtılan uygulamalar, trafiği uygulamalarının kullanılabilir bir örneğine yönlendirmek için Trafik Yöneticisi'ne güvenebilir.

### <a name="how-does-the-choice-of-resource-group-location-affect-traffic-manager"></a>Kaynak grubu konumu seçimi Trafik Yöneticisi'ni nasıl etkiler?

Trafik Yöneticisi tek ve küresel bir hizmettir. Bölgesel değil. Kaynak grubu konumunun seçimi, bu kaynak grubunda dağıtılan Trafik Yöneticisi profilleri için hiçbir fark yaratmaz.

Azure Kaynak Yöneticisi, tüm kaynak gruplarının, bu kaynak grubunda dağıtılan kaynaklar için varsayılan konumu belirleyen bir konum belirlemesini gerektirir. Bir Trafik Yöneticisi profili oluşturduğunuzda, bir kaynak grubunda oluşturulur. Tüm Trafik Yöneticisi profilleri, kaynak grubu varsayılanını geçersiz kılarak konum olarak **genel** kullanır.

### <a name="how-do-i-determine-the-current-health-of-each-endpoint"></a>Her bitiş noktasının geçerli durumunu nasıl belirleyebilirim?

Genel profile ek olarak her bitiş noktasının geçerli izleme durumu Azure portalında görüntülenir. Bu bilgiler ayrıca Traffic Monitor [REST API,](https://msdn.microsoft.com/library/azure/mt163667.aspx) [PowerShell cmdlets](https://docs.microsoft.com/powershell/module/az.trafficmanager)ve [çapraz platform Azure CLI](../cli-install-nodejs.md)üzerinden de kullanılabilir.

Ayrıca, uç noktalarınızın sistem durumunu izlemek ve bunların görsel bir temsilini görmek için Azure Monitor'u da kullanabilirsiniz. Azure Monitör'ü kullanma hakkında daha fazla bilgi için [Azure İzleme belgelerine](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-metrics)bakın.

### <a name="can-i-monitor-https-endpoints"></a>HTTPS uç noktalarını izleyebilir miyim?

Evet. Trafik Yöneticisi HTTPS üzerinde sondalama destekler. **Https'yi** izleme yapılandırmasındaprotokol olarak yapılandırın.

Trafik yöneticisi, şunları dahil olmak üzere herhangi bir sertifika doğrulaması sağlayamaz:

* Sunucu tarafındaki sertifikalar doğrulanmadı
* SNI sunucu tarafındaki sertifikalar doğrulanmadı
* İstemci sertifikaları desteklenmiyor

### <a name="do-i-use-an-ip-address-or-a-dns-name-when-adding-an-endpoint"></a>Bitiş noktası eklerken IP adresi veya DNS adı kullanıyor muyum?

Trafik Yöneticisi, bunları başvurmak için üç yol kullanarak uç noktaları eklemeyi destekler – dns adı olarak, IPv4 adresi olarak ve IPv6 adresi olarak. Bitiş noktası IPv4 veya IPv6 adresi olarak eklenirse, sorgu yanıtı sırasıyla A veya AAAA kayıt türünde olacaktır. Bitiş noktası DNS adı olarak eklendiyse, sorgu yanıtı cname türünde olacaktır. Uç noktalarıi IPv4 veya IPv6 adresi olarak eklemeye yalnızca bitiş noktası **Harici**türündeyse izin verilir.
Tüm yönlendirme yöntemleri ve izleme ayarları üç uç nokta adresleme türleri tarafından desteklenir.

### <a name="what-types-of-ip-addresses-can-i-use-when-adding-an-endpoint"></a>Bitiş noktası eklerken ne tür IP adresleri kullanabilirim?

Trafik Yöneticisi uç noktaları belirtmek için IPv4 veya IPv6 adreslerini kullanmanıza izin verir. Aşağıda listelenen birkaç kısıtlama vardır:

- Ayrılmış özel IP adresi alanlarına karşılık gelen adreslere izin verilmez. Bu adresler RFC 1918, RFC 6890, RFC 5737, RFC 3068, RFC 2544 ve RFC 5771'de sesverilenleri içerir.
- Adres herhangi bir bağlantı noktası numarası içermemelidir (profil yapılandırma ayarlarında kullanılacak bağlantı noktalarını belirtebilirsiniz)
- Aynı profildeki hiçbir iki uç nokta aynı hedef IP adresine sahip olamaz

### <a name="can-i-use-different-endpoint-addressing-types-within-a-single-profile"></a>Tek bir profildeki farklı uç nokta adresleme türlerini kullanabilir miyim?

Hayır, Trafik Yöneticisi, IPv4 ve IPv6 adresleme türlerini karıştırabileceğiniz MultiValue yönlendirme türüne sahip bir profil durumu dışında, profildeki uç nokta adresleme türlerini karıştırmanıza izin vermez

### <a name="what-happens-when-an-incoming-querys-record-type-is-different-from-the-record-type-associated-with-the-addressing-type-of-the-endpoints"></a>Gelen bir sorgunun kayıt türü, uç noktaların adresleme türüyle ilişkili kayıt türünden farklı olduğunda ne olur?

Bir profil karşı bir sorgu aldığında, Trafik Yöneticisi ilk olarak belirtilen yönlendirme yöntemine ve bitiş noktalarının sistem durumu durumuna göre döndürülmesi gereken bitiş noktasını bulur. Ardından, aşağıdaki tabloya dayalı bir yanıt döndürmeden önce gelen sorguda istenen kayıt türüne ve bitiş noktasıyla ilişkili kayıt türüne bakar.

MultiValue dışında herhangi bir yönlendirme yöntemi olan profiller için:

|Gelen sorgu isteği|     Uç nokta türü|     Yanıt Sağlandı|
|--|--|--|
|HERHANGI BIR |    A / AAAA / CNAME |    Hedef Bitiş Noktası| 
|A |    A / CNAME |    Hedef Bitiş Noktası|
|A |    AAAA |    NODATA |
|AAAA |    AAAA / CNAME |    Hedef Bitiş Noktası|
|AAAA |    A |    NODATA |
|CNAME |    CNAME |    Hedef Bitiş Noktası|
|CNAME     |A / AAAA |    NODATA |
|

Yönlendirme yöntemi multivalue olarak ayarlanmış profiller için:

|Gelen sorgu isteği|     Uç nokta türü |    Yanıt Sağlandı|
|--|--|--|
|HERHANGI BIR |    A ve AAAA Karışımı |    Hedef Uç Noktaları|
|A |    A ve AAAA Karışımı |    A türünün yalnızca Hedef Uç Noktaları|
|AAAA    |A ve AAAA Karışımı|     Aaaa türünün yalnızca Hedef Uç Noktaları|
|CNAME |    A ve AAAA Karışımı |    NODATA |

### <a name="can-i-use-a-profile-with-ipv4--ipv6-addressed-endpoints-in-a-nested-profile"></a>İç içe bir profilde IPv4 / IPv6 adresli uç noktaları olan bir profil kullanabilir miyim?

Evet, multivalue türünden bir profilin iç içe geçen profil kümesinde bir üst profil olamayacağı dışında yapabilirsiniz.

### <a name="i-stopped-an-web-application-endpoint-in-my-traffic-manager-profile-but-i-am-not-receiving-any-traffic-even-after-i-restarted-it-how-can-i-fix-this"></a>Trafik Yöneticisi profilimde bir web uygulaması bitiş noktası nı durdurdum, ancak yeniden başlattıktan sonra bile herhangi bir trafik alamıyorum. Bunu nasıl düzeltebilirim?

Bir Azure web uygulaması bitiş noktası durdurulduğunda Trafik Yöneticisi sistem durumunu denetlemeyi durdurur ve sistem durumu denetimlerini yalnızca bitiş noktasının yeniden başlatıldığında niçin algıladığını algıladıktan sonra yeniden başlatır. Bu gecikmeyi önlemek için, bitiş noktasını yeniden başlattıktan sonra Trafik Yöneticisi profilindeki bitiş noktasını devre dışı edin ve yeniden etkinleştirin.

### <a name="can-i-use-traffic-manager-even-if-my-application-does-not-have-support-for-http-or-https"></a>UygulamamDA HTTP veya HTTPS desteği olmasa bile Trafik Yöneticisi'ni kullanabilir miyim?

Evet. İzleme protokolü olarak TCP belirtebilirsiniz ve Trafik Yöneticisi bir TCP bağlantısı başlatabilir ve bitiş noktasından bir yanıt bekleyebilir. Bitiş noktası, bağlantı kurmak için bir yanıtla bağlantı isteğini zaman ası psonu süresi içinde yanıtlarsa, bu bitiş noktası sağlıklı olarak işaretlenir.

### <a name="what-specific-responses-are-required-from-the-endpoint-when-using-tcp-monitoring"></a>TCP izleme kullanırken bitiş noktasından hangi özel yanıtlar gereklidir?

TCP izleme kullanıldığında, Trafik Yöneticisi belirtilen bağlantı noktasında bitiş noktasına bir SYN isteği göndererek üç yönlü bir TCP el sıkışması başlatır. Daha sonra bir süre için bitiş noktasından bir SYN-ACK yanıtı bekler (zaman ayarı ayarlarında belirtilir).

- Syn-ACK yanıtı izleme ayarlarında belirtilen zaman sonu süresi içinde alınırsa, bu bitiş noktası sağlıklı kabul edilir. Bir FIN veya FIN-ACK, bir soketi düzenli olarak sonlandırdığinde Trafik Yöneticisi'nden beklenen yanıttır.
- Belirtilen zaman anından sonra bir SYN-ACK yanıtı alınırsa, Trafik Yöneticisi bağlantıyı sıfırlamak için bir RST ile yanıt verir.

### <a name="how-fast-does-traffic-manager-move-my-users-away-from-an-unhealthy-endpoint"></a>Trafik Yöneticisi kullanıcılarımı sağlıksız bir bitiş noktasından ne kadar hızlı uzaklaştırıyor?

Trafik Yöneticisi, Trafik Yöneticisi profilinizin başarısız davranışını aşağıdaki gibi denetlemenize yardımcı olabilecek birden çok ayar sağlar:

- Sondalama Aralığını 10 saniye olarak ayarlayarak Trafik Yöneticisi'nin uç noktaları daha sık incelediğini belirtebilirsiniz. Bu, sağlıksız giden herhangi bir bitiş noktasının mümkün olan en kısa sürede algılanabilmesini sağlar. 
- sağlık kontrolü isteğinin saatler bitmeden önce ne kadar bekleyeceğinizi belirtebilirsiniz (minimum zaman çıkış değeri 5 sn'dir).
- bitiş noktası sağlıksız olarak işaretlendirilmeden önce kaç hata oluşabileceğini belirtebilirsiniz. Bu değer 0 olarak düşük olabilir ve bu durumda bitiş noktası ilk sistem durumu denetiminde başarısız olur olmaz sağlıksız olarak işaretlenir. Ancak, tolere edilen hata sayısı için en az 0 değerinin kullanılması, sondalama sırasında oluşabilecek geçici sorunlar nedeniyle uç noktaların döndürme dışına alınmasına neden olabilir.
- DNS yanıtı için canlı süreyi (TTL) 0'a kadar düşürebilirsiniz. Bunu yapmak, DNS çözümleyicilerinin yanıtı önbelleğe alamadığı ve her yeni sorgunun Trafik Yöneticisi'nin sahip olduğu en güncel sistem durumu bilgilerini içeren bir yanıt alacağı anlamına gelir.

Trafik Yöneticisi bu ayarları kullanarak, bir bitiş noktası sağlıksız hale geçtikten ve ilgili profile karşı bir DNS sorgusu yapıldıktan sonra 10 saniyenin altında arızalar sağlayabilir.

### <a name="how-can-i-specify-different-monitoring-settings-for-different-endpoints-in-a-profile"></a>Bir profildeki farklı uç noktalar için farklı izleme ayarlarını nasıl belirtebilirim?

Trafik Yöneticisi izleme ayarları profil başına bir düzeydedir. Yalnızca bir bitiş noktası için farklı bir izleme ayarı kullanmanız gerekiyorsa, izleme ayarları üst profilden farklı olan [iç içe bir profil](traffic-manager-nested-profiles.md) olarak bu bitiş noktasına sahip olarak yapılabilir.

### <a name="how-can-i-assign-http-headers-to-the-traffic-manager-health-checks-to-my-endpoints"></a>HTTP üstbilgilerini Trafik Yöneticisi sağlık kontrollerine uç noktalarıma nasıl atayabilirim?

Trafik Yöneticisi, uç noktalarınıza başlattığı HTTP(S) sağlık denetimlerinde özel üstbilgi belirtmenize olanak tanır. Özel bir üstbilgi belirtmek isterseniz, bunu profil düzeyinde (tüm uç noktalara uygulanabilir) yapabilir veya bitiş noktası düzeyinde belirtebilirsiniz. Üstbilgi her iki düzeyde tanımlanırsa, bitiş noktası düzeyinde belirtilen bir profil düzeyi bir geçersiz kılar.
Bunun için yaygın kullanım örneklerinden biri, Trafik Yöneticisi isteklerinin çok kiracılı bir ortamda barındırılan bir uç noktaya doğru yönlendirilebilmeleri için ana bilgisayar üstbilgilerini belirtmektir. Bunun bir diğer kullanım örneği de, bir uç noktanın HTTP(S) istek günlüklerinden Trafik Yöneticisi isteklerini tanımlamaktır

### <a name="what-host-header-do-endpoint-health-checks-use"></a>Uç nokta sağlık denetimleri ne ana bilgisayar üstbilgi kullanır?

Özel ana bilgisayar üstbilgia ayarı sağlanmadıysa, Traffic Manager tarafından kullanılan ana bilgisayar üstbilgisi, varsa profilde yapılandırılan bitiş noktası hedefinin DNS adıdır.

### <a name="what-are-the-ip-addresses-from-which-the-health-checks-originate"></a>Sağlık kontrollerinin kaynaklandığı IP adresleri nelerdir?

Trafik Yöneticisi sağlık denetimlerinin kaynaklanabileceği IP adreslerini listeleyen JSON dosyasını görüntülemek için [burayı](https://azuretrafficmanagerdata.blob.core.windows.net/probes/azure/probe-ip-ranges.json) tıklatın. Bu IP adreslerinden gelen bağlantılara son noktalarda sistem durumunu denetlemek için izin verilen lerden emin olmak için JSON dosyasında listelenen IP'leri gözden geçirin.

### <a name="how-many-health-checks-to-my-endpoint-can-i-expect-from-traffic-manager"></a>Trafik Yöneticisi'nden son noktama kaç sağlık denetimi bekleyebilirim?

Bitiş noktanıza ulaşan Trafik Yöneticisi sağlık kontrollerinin sayısı aşağıdakilere bağlıdır:

- izleme aralığı için belirlediğiniz değer (daha küçük aralık, belirli bir zaman diliminde bitiş noktanıza daha fazla istek inişi anlamına gelir).
- sistem durumu kontrollerinin kaynaklandığı yerin sayısı (bu çeklerin beklendiği yerdeki IP adresleri önceki SSS'de listelenir).

### <a name="how-can-i-get-notified-if-one-of-my-endpoints-goes-down"></a>Uç noktalarımdan biri düşerse nasıl bilgilendirilirim?

Trafik Yöneticisi tarafından sağlanan ölçümlerden biri, profildeki uç noktaların sistem durumudur. Bunu bir profildeki tüm uç noktaların toplamı olarak (örneğin, uç noktalarınızın %75'i sağlıklıdır) veya bitiş noktası düzeyinde görebilirsiniz. Trafik Yöneticisi ölçümleri Azure Monitor aracılığıyla ortaya çıkar ve bitiş noktanızın sistem durumu yla ilgili bir değişiklik olduğunda bildirim almak için [uyarı özelliklerini](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md) kullanabilirsiniz. Daha fazla ayrıntı için [Trafik Yöneticisi ölçümlerine ve uyarılarına](traffic-manager-metrics-alerts.md)bakın.  

## <a name="traffic-manager-nested-profiles"></a>Trafik Yöneticisi iç içe profiller

### <a name="how-do-i-configure-nested-profiles"></a>İç içe profilleri nasıl yapılandırıyorum?

İç içe trafik yöneticisi profilleri hem Azure Kaynak Yöneticisi hem de klasik Azure REST API'leri, Azure PowerShell cmdlets'leri ve çapraz platform Azure CLI komutları kullanılarak yapılandırılabilir. Ayrıca yeni Azure portalı üzerinden de desteklenirler.

### <a name="how-many-layers-of-nesting-does-traffic-manger-support"></a>Trafik Yöneticisi kaç kat iç içe geçmeyi destekler?

10 düzeye kadar olan profilleri yuvalayabilirsiniz. 'Döngülere' izin verilmez.

### <a name="can-i-mix-other-endpoint-types-with-nested-child-profiles-in-the-same-traffic-manager-profile"></a>Aynı Trafik Yöneticisi profilinde iç içe geçen alt profillerle diğer uç nokta türlerini karıştırabilir miyim?

Evet. Profildeki farklı türdeki uç noktaları birleştirme konusunda herhangi bir kısıtlama yoktur.

### <a name="how-does-the-billing-model-apply-for-nested-profiles"></a>İç içe profiller için faturalandırma modeli nasıl uygulanır?

İç içe profilkullanmanın olumsuz bir fiyatlandırma etkisi yoktur.

Trafik Yöneticisi faturalandırmanın iki bileşeni vardır: uç nokta sağlık denetimleri ve milyonlarca DNS sorgusu

* Bitiş noktası sistem durumu denetimleri: Üst profilde bitiş noktası olarak yapılandırıldığında alt profil için ücret alınmaz. Alt profildeki uç noktaların izlenmesi her zamanki gibi faturalandırılır.
* DNS sorguları: Her sorgu yalnızca bir kez sayılır. Alt profilden bitiş noktası döndüren bir üst profile karşı bir sorgu yalnızca üst profile sayılır.

Tüm ayrıntılar için [Trafik Yöneticisi fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/traffic-manager/)bakın.

### <a name="is-there-a-performance-impact-for-nested-profiles"></a>İç içe profiller için bir performans etkisi var mı?

Hayır. İç içe profiller kullanırken oluşan performans etkisi yoktur.

Trafik Yöneticisi ad sunucuları, her DNS sorgusunu işlerken profil hiyerarşisini dahili olarak geçer. Üst profildeki bir DNS sorgusu, alt profilden bitiş noktası olan bir DNS yanıtı alabilir. Tek bir cname kaydı, ister tek bir profil ister iç içe profiller kullanıyor olun kullanılır. Hiyerarşideki her profil için cname kaydı oluşturmaya gerek yoktur.

### <a name="how-does-traffic-manager-compute-the-health-of-a-nested-endpoint-in-a-parent-profile"></a>Trafik Yöneticisi, üst profilde iç içe bir uç noktanın durumunu nasıl hesaplar?

Üst profil, doğrudan çocuk üzerinde sağlık denetimi yapmaz. Bunun yerine, alt profilin son noktalarının durumu, alt profilin genel durumunu hesaplamak için kullanılır. Bu bilgiler iç içe kalmış bitiş noktasının durumunu belirlemek için iç içe geçen profil hiyerarşisini yayıştırılır. Üst profil, trafiğin çocuğa yönlendirilip yönlendirilemeyeceğini belirlemek için bu toplu durumu kullanır.

Aşağıdaki tabloda, iç içe bir uç nokta için Trafik Yöneticisi sağlık denetimlerinin davranışı açıklanmaktadır.

| Alt Profil Monitörü durumu | Üst Uç Nokta Monitörü durumu | Notlar |
| --- | --- | --- |
| Devre dışı. Alt profil devre dışı bırakıldı. |Durduruldu |Üst uç nokta durumu Durduruldu, Devre Dışı bırakıldı. Devre Dışı Bırakma durumu, üst profildeki bitiş noktasını devre dışı ettiğinizi belirtmek için ayrılmıştır. |
| Bozulmuş. En az bir alt profil bitiş noktası bozulmuş durumdadır. |Çevrimiçi: Alt profildeki Çevrimiçi uç noktaların sayısı en azından MinChildEndpoints değeridir.<BR>CheckEndpoint: Alt profildeki Çevrimiçi artı CheckEndpoint uç noktalarının sayısı en azından MinChildEndpoints değeridir.<BR>Bozulmuş: aksi takdirde. |Trafik, durum DenetimiBitiş Noktası'nın bitiş noktasına yönlendirilir. MinChildEndpoints çok yüksek ayarlanırsa, bitiş noktası her zaman bozulur. |
| Çevrimiçi. En az bir alt profil bitiş noktası çevrimiçi durumdur. Bozulmuş durumda hiçbir bitiş noktası yoktur. |Yukarıya bakın. | |
| Kontrol Bitiş Noktaları. En az bir alt profil bitiş noktası 'CheckEndpoint'tir. Hiçbir uç nokta 'Çevrimiçi' veya 'Bozulmuş' değildir |Yukarıdakiyle aynı. | |
| Etkin olmayan. Tüm alt profil bitiş noktaları devre dışı bırakılır veya durdurulur veya bu profilin bitiş noktası yoktur. |Durduruldu | |

## <a name="next-steps"></a>Sonraki adımlar:

- Trafik Yöneticisi [uç nokta izleme ve otomatik arıza](../traffic-manager/traffic-manager-monitoring.md)hakkında daha fazla bilgi edinin.
- Trafik Yöneticisi [trafik yönlendirme yöntemleri](../traffic-manager/traffic-manager-routing-methods.md)hakkında daha fazla bilgi edinin.
