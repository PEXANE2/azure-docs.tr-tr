---
title: Azure Trafik Yöneticisi uç nokta izleme | Microsoft Dokümanlar
description: Bu makale, Trafik Yöneticisi'nin Azure müşterilerinin yüksek kullanılabilirlik uygulamaları dağıtmasına yardımcı olmak için uç nokta izleme ve otomatik bitiş noktası başarısızlığı nasıl kullandığını anlamanıza yardımcı olabilir
services: traffic-manager
author: rohinkoul
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/04/2018
ms.author: rohink
ms.openlocfilehash: 61aafbe8cb12e93d72f5efd01155f06fb3ec0c28
ms.sourcegitcommit: 441db70765ff9042db87c60f4aa3c51df2afae2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80757267"
---
# <a name="traffic-manager-endpoint-monitoring"></a>Traffic Manager uç nokta izleme

Azure Trafik Yöneticisi yerleşik uç nokta izleme ve otomatik bitiş noktası başarısıziçerir. Bu özellik, Azure bölge hataları da dahil olmak üzere uç nokta hatalarına dayanıklı yüksek kullanılabilirlik uygulamaları sunmanıza yardımcı olur.

## <a name="configure-endpoint-monitoring"></a>Son nokta izlemeyi yapılandırma

Uç nokta izlemeyi yapılandırmak için Trafik Yöneticisi profilinizde aşağıdaki ayarları belirtmeniz gerekir:

* **Protokol.** Trafik Yöneticisi'nin sistem durumunu kontrol etmek için bitiş noktanızı araştırırken kullandığı protokol olarak HTTP, HTTPS veya TCP'yi seçin. HTTPS izleme tls/SSL sertifikanızın geçerli olup olmadığını doğrulamaz, yalnızca sertifikanın mevcut olup olmadığını denetler.
* **Bağlantı noktası**. İstek için kullanılan bağlantı noktasını seçin.
* **Yol**. Bu yapılandırma ayarı yalnızca yol ayarını belirtmenin gerekli olduğu HTTP ve HTTPS protokolleri için geçerlidir. TCP izleme protokolü için bu ayarı sağlamak bir hata yla sonuçlanır. HTTP ve HTTPS protokolü için, göreli yolu ve web sayfasının veya izlemenin eriştikleri dosyanın adını verin. İleri eğik çizgi (/) göreli yol için geçerli bir giriştir. Bu değer, dosyanın kök dizinde (varsayılan) olduğu anlamına gelir.
* **Özel üstbilgi ayarları** Bu yapılandırma ayarı, Trafik Yöneticisi'nin profil altındaki uç noktalara gönderdiği sistem durumu denetimlerine belirli BIRDURUM üstbilgilerini eklemenize yardımcı olur. Özel üstbilgi, bu profildeki tüm uç noktalar için geçerli olacak bir profil düzeyinde ve/veya yalnızca bu bitiş noktasına uygulanabilir bir bitiş noktası düzeyinde belirtilebilir. Çok kiracılı bir ortamda son noktaların ana bilgisayar üstbilgisini belirterek hedeflerine doğru şekilde yönlendirilmesi için sistem durumu denetimlerinin olması için özel üstbilgi kullanabilirsiniz. Trafik Yöneticisi kaynaklı HTTP(S) isteklerini tanımlamak ve bunları farklı şekilde işler belirlemek için kullanılabilecek benzersiz üstbilgi ekleyerek de bu ayarı kullanabilirsiniz. En fazla sekiz üstbilgi belirtebilirsiniz: bir virgülle ayrılmış değer çiftleri. Örneğin, "header1:value1,header2:value2". 
* **Beklenen durum kodu aralıkları** Bu ayar, 200-299, 301-301 biçiminde birden çok başarı kodu aralığı belirtmenizi sağlar. Bu durum kodları, sistem durumu denetimi başlatıldığında bir son noktadan yanıt olarak alınırsa, Trafik Yöneticisi bu uç noktaları sağlıklı olarak işaretler. En fazla 8 durum kodu aralığı belirtebilirsiniz. Bu ayar yalnızca HTTP ve HTTPS protokolü ve tüm uç noktalar için geçerlidir. Bu ayar Trafik Yöneticisi profil düzeyindedir ve varsayılan olarak 200 değeri başarı durum kodu olarak tanımlanır.
* **Sonlama aralığı.** Bu değer, bir son noktanın bir Trafik Yöneticisi sondalama aracısından durumu için ne sıklıkta denetlendiğini belirtir. Burada iki değer belirtebilirsiniz: 30 saniye (normal sondalama) ve 10 saniye (hızlı sondalama). Hiçbir değer sağlanmadıysa, profil varsayılan değeri 30 saniye olarak ayarlar. Hızlı sondalama fiyatlandırması hakkında daha fazla bilgi edinmek için [Trafik Yöneticisi Fiyatlandırma](https://azure.microsoft.com/pricing/details/traffic-manager) sayfasını ziyaret edin.
* **Tolere edilen hata sayısı.** Bu değer, bir Trafik Yöneticisi sondalama aracısı sağlıksız olarak bu bitiş noktası işaretlemeden önce tolere kaç hataları belirtir. Değeri 0 ile 9 arasında değişebilir. 0 değeri, tek bir izleme hatasının bu bitiş noktasının sağlıksız olarak işaretlenmesine neden olabileceği anlamına gelir. Değer belirtilmemişse, varsayılan değeri 3 kullanır.
* **Sonda zaman dilimi**. Bu özellik, trafik yöneticisi sondalama aracısının, son noktaya bir sağlık denetimi sondası gönderildiğinde bu denetimi denetlemeyi düşünmeden önce beklemesi gereken süreyi belirtir. Sondalama Aralığı 30 saniye olarak ayarlanmışsa, Zaman Aralığı değerini 5 ile 10 saniye arasında ayarlayabilirsiniz. Değer belirtilmemişse, varsayılan değeri 10 saniye kullanır. Sondalama Aralığı 10 saniye olarak ayarlanmışsa, Zaman Aralığı değerini 5 ile 9 saniye arasında ayarlayabilirsiniz. Zaman Önce si belirtilmemişse, varsayılan değeri 9 saniye kullanır.

    ![Traffic Manager uç nokta izleme](./media/traffic-manager-monitoring/endpoint-monitoring-settings.png)

    **Şekil: Trafik Yöneticisi uç nokta izleme**

## <a name="how-endpoint-monitoring-works"></a>Uç nokta izleme nasıl çalışır?

İzleme protokolü HTTP veya HTTPS olarak ayarlanırsa, Trafik Yöneticisi sondalama aracısı verilen protokol, bağlantı noktası ve göreli yolu kullanarak bitiş noktasına get isteğinde bulunur. 200-Ok yanıtını geri alırsa veya **Beklenen durum \*kodu aralıklarında**yapılandırılan yanıtlardan herhangi biri varsa, bu bitiş noktası sağlıklı kabul edilir. Yanıt farklı bir değerse veya belirtilen zaman dışında yanıt alınmazsa, Trafik Yöneticisi sondalama aracısı Tolere Edilen Hata Sayısı ayarına göre yeniden çalışır (bu ayar 0 ise yeniden deneme yapılmaz). Ardışık hataların sayısı Tolere Edilen Hata Sayısı ayarından daha yüksekse, bu bitiş noktası sağlıksız olarak işaretlenir. 

İzleme protokolü TCP ise, Trafik Yöneticisi sondalama aracısı belirtilen bağlantı noktasını kullanarak bir TCP bağlantı isteği başlatır. Bitiş noktası bağlantıyı kurmak için bir yanıtile isteğe yanıt verirse, bu sistem durumu denetimi bir başarı olarak işaretlenir ve Trafik Yöneticisi sondalama aracısı TCP bağlantısını sıfırlar. Yanıt farklı bir değerse veya belirtilen zaman dışında yanıt alınmazsa, Trafik Yöneticisi sondalama aracısı Tolere Edilen Hata Sayısı ayarına göre yeniden çalışır (bu ayar 0 ise yeniden deneme yapılmaz). Ardışık hataların sayısı Tolere Edilen Hata Sayısı ayarından daha yüksekse, bu bitiş noktası sağlıksız olarak işaretlenir.

Her durumda, Trafik Yöneticisi birden çok konumdan sondalar ve ardışık hata belirleme her bölge içinde olur. Bu aynı zamanda uç noktaların Sondalama Aralığı için kullanılan ayarından daha yüksek bir sıklıkta Trafik Yöneticisi'nden sistem durumu sondaları aldığı anlamına gelir.

>[!NOTE]
>HTTP veya HTTPS izleme protokolü için, bitiş noktası tarafında yaygın bir uygulama uygulamanızda özel bir sayfa uygulamaktır - örneğin, /health.aspx. İzleme için bu yolu kullanarak, performans sayaçlarını denetleme veya veritabanı kullanılabilirliğini doğrulama gibi uygulamaya özgü denetimler gerçekleştirebilirsiniz. Bu özel denetimlere bağlı olarak, sayfa uygun bir HTTP durum kodu döndürür.

Trafik Yöneticisi profilindeki tüm uç noktalar izleme ayarlarını paylaşır. Farklı uç noktalar için farklı izleme ayarları kullanmanız gerekiyorsa, [iç içe trafik yöneticisi profilleri](traffic-manager-nested-profiles.md#example-5-per-endpoint-monitoring-settings)oluşturabilirsiniz.

## <a name="endpoint-and-profile-status"></a>Bitiş noktası ve profil durumu

Trafik Yöneticisi profillerini ve uç noktalarını etkinleştirebilir ve devre dışı kullanabilirsiniz. Ancak, Trafik Yöneticisi otomatik ayarları ve işlemleri sonucunda uç nokta durumunda bir değişiklik de oluşabilir.

### <a name="endpoint-status"></a>Bitiş noktası durumu

Belirli bir bitiş noktasını etkinleştirebilir veya devre dışı kullanabilirsiniz. Hala sağlıklı olabilecek temel hizmet etkilenmez. Bitiş noktası durumunun değiştirilmesi, Trafik Yöneticisi profilindeki bitiş noktasının kullanılabilirliğini denetler. Bir bitiş noktası durumu devre dışı bırakıldığında, Trafik Yöneticisi sistem durumunu denetlemez ve bitiş noktası DNS yanıtına dahil edilmez.

### <a name="profile-status"></a>Profil durumu

Profil durum ayarını kullanarak, belirli bir profili etkinleştirebilir veya devre dışı kullanabilirsiniz. Uç nokta durumu tek bir bitiş noktasını etkilerken, profil durumu tüm uç noktaları da dahil olmak üzere tüm profili etkiler. Bir profili devre dışı dışı aldığınızda, uç noktalar sistem durumu için denetlenmez ve DNS yanıtına uç nokta dahil edilmez. DNS sorgusu için [bir NXDOMAIN](https://tools.ietf.org/html/rfc2308) yanıt kodu döndürülür.

### <a name="endpoint-monitor-status"></a>Uç nokta monitör durumu

Uç nokta izleme durumu, bitiş noktasının durumunu gösteren Trafik Yöneticisi tarafından oluşturulan bir değerdir. Bu ayarı el ile değiştiremezsiniz. Uç nokta izleme durumu, bitiş noktası izleme sonuçları ve yapılandırılan uç nokta durumunun bir birleşimidir. Uç nokta monitördurumunun olası değerleri aşağıdaki tabloda gösterilmiştir:

| Profil durumu | Bitiş noktası durumu | Uç nokta monitör durumu | Notlar |
| --- | --- | --- | --- |
| Devre dışı |Etkin |Etkin değil |Profil devre dışı bırakıldı. Uç nokta durumu Etkin olsa da, profil durumu (Devre dışı bırakılmış) öncegelir. Devre dışı bırakılan profillerde uç noktalar izlenmez. DNS sorgusu için bir NXDOMAIN yanıt kodu döndürülür. |
| &lt;tümü&gt; |Devre dışı |Devre dışı |Bitiş noktası devre dışı bırakıldı. Devre dışı bırakılan uç noktalar izlenmez. Bitiş noktası DNS yanıtlarına dahil edilmez, bu nedenle trafik almaz. |
| Etkin |Etkin |Çevrimiçi |Bitiş noktası izlenir ve sağlıklıdır. DNS yanıtlarına dahildir ve trafik alabilir. |
| Etkin |Etkin |Düzeyi düşürüldü |Endpoint izleme sistem denetimleri başarısız oluyor. Bitiş noktası DNS yanıtlarına dahil edilmez ve trafik almaz. <br>Bunun bir istisnası, tüm uç noktaların bozulması ve bu durumda hepsinin sorgu yanıtında döndürülmüş olarak kabul edilmesidir).</br>|
| Etkin |Etkin |Kontrol Bitiş Noktası |Bitiş noktası izlenir, ancak ilk sondanın sonuçları henüz alınmamıştır. CheckEndpoint genellikle profilde bir bitiş noktası ekledikten veya etkinleştirdikten hemen sonra oluşan geçici bir durumdur. Bu durumdaki bir bitiş noktası DNS yanıtlarına dahildir ve trafik alabilir. |
| Etkin |Etkin |Durduruldu |Bitiş noktasının işaret ettiği web uygulaması çalışmıyor. Web uygulaması ayarlarını kontrol edin. Bu, uç nokta iç içe uç nokta türündeyse ve alt profil devre dışı ysa veya etkin değilse de olabilir. <br>Durduruldu durumu olan bir bitiş noktası izlenmez. DNS yanıtlarına dahil edilmez ve trafik almaz. Bunun bir istisnası, tüm uç noktaların bozulması ve bu durumda hepsinin sorgu yanıtında döndürülmüş olarak kabul edilmesidir.</br>|

İç içe uç noktalar için uç nokta izleme durumunun nasıl hesaplandığıyla ilgili ayrıntılar için [iç içe olan Trafik Yöneticisi profillerine](traffic-manager-nested-profiles.md)bakın.

>[!NOTE]
> Web uygulamanız Standart katmanda veya üzerinde çalışmıyorsa, Uygulama Hizmeti'nde Durdurulan Bitiş Noktası izleme durumu gerçekleşebilir. Daha fazla bilgi [için, Uygulama Hizmeti ile Trafik Yöneticisi tümleştirmesi'ne](/azure/app-service/web-sites-traffic-manager)bakın.

### <a name="profile-monitor-status"></a>Profil monitörü durumu

Profil izleme durumu, yapılandırılan profil durumu ve tüm uç noktalar için uç nokta izleme durum değerlerinin bir birleşimidir. Olası değerler aşağıdaki tabloda açıklanmıştır:

| Profil durumu (yapılandırıldı) | Uç nokta monitör durumu | Profil monitörü durumu | Notlar |
| --- | --- | --- | --- |
| Devre dışı |&lt;tanımlanmış&gt; uç noktaları olmayan herhangi bir profil veya profil. |Devre dışı |Profil devre dışı bırakıldı. |
| Etkin |En az bir bitiş noktasının durumu bozulur. |Düzeyi düşürüldü |Hangi uç noktaların daha fazla dikkat gerektirdiğini belirlemek için tek tek uç nokta durum değerlerini gözden geçirin. |
| Etkin |En az bir bitiş noktasının durumu Çevrimiçi'dir. Hiçbir uç noktanın bozulmuş bir durumu yoktur. |Çevrimiçi |Hizmet trafiği kabul ediyor. Başka bir eylem gerekli değildir. |
| Etkin |En az bir bitiş noktasının durumu CheckEndpoint'tir. Çevrimiçi veya Bozulmuş durumda uç nokta yok. |KontrolUçnoktaları |Bu geçiş durumu, bir profil oluşturulduğunda veya etkinleştirildiğinde oluşur. Bitiş noktası durumu ilk kez kontrol ediliyor. |
| Etkin |Profildeki tüm uç noktaların durumları Devre Dışı bırakılmış veya Durdurulmuş veya profilin tanımlı uç noktaları yoktur. |Etkin değil |Hiçbir uç nokta etkin değil, ancak profil hala etkin. |

## <a name="endpoint-failover-and-recovery"></a>Endpoint failover ve kurtarma

Trafik Yöneticisi, sağlıksız uç noktalar da dahil olmak üzere her uç noktanın durumunu düzenli olarak denetler. Trafik Yöneticisi, bir bitiş noktasının ne zaman sağlıklı hale geldiğini algılar ve onu döndürmeye geri getirir.

Aşağıdaki olaylardan herhangi biri oluştuğunda bitiş noktası sağlıksızdır:

- İzleme protokolü HTTP veya HTTPS ise:
    - 200 olmayan bir yanıt veya **Beklenen durum kodu aralıkları** ayarında belirtilen durum aralığını içermeyen bir yanıt alınır (farklı bir 2xx kodu veya 301/302 yeniden yönlendirmesi dahil).
- İzleme protokolü TCP ise: 
    - ACK veya SYN-ACK dışındaki bir yanıt, Trafik Yöneticisi tarafından bir bağlantı kurulması girişiminde bulunmak üzere gönderilen SYN isteğine yanıt olarak alınır.
- Zaman aşımı. 
- Bitiş noktasının erişilemememesi ile sonuçlanan diğer bağlantı sorunu.

Sorun giderme başarısız denetimleri hakkında daha fazla bilgi için Azure [Trafik Yöneticisi'nde Sorun Giderme Durumu'na](traffic-manager-troubleshooting-degraded.md)bakın. 

Aşağıdaki şekildeki zaman çizelgesi, Trafik Yöneticisi bitiş noktasının aşağıdaki ayarları içeren izleme sürecinin ayrıntılı bir açıklamasıdır: izleme protokolü HTTP' dir, sondalama aralığı 30 saniyedir, tolere edilen hata sayısı 3, zaman aralığı değeri 10 saniye ve DNS TTL 30 saniyedir.

![Trafik Yöneticisi bitiş noktası failover ve failback dizisi](./media/traffic-manager-monitoring/timeline.png)

**Şekil: Trafik yöneticisi bitiş noktası failover ve kurtarma sırası**

1. **AL.** Her bitiş noktası için, Trafik Yöneticisi izleme sistemi izleme ayarlarında belirtilen yolda bir GET isteği gerçekleştirir.
2. **200 Ok veya özel kod aralığı trafik yöneticisi profil izleme ayarları belirtilen** . İzleme sistemi, http 200 Ok veya belirtilen özel kod aralığının Trafik Yöneticisi profil izleme ayarları iletisinin 10 saniye içinde döndürülmesini bekler. Bu yanıtı aldığında, hizmetin kullanılabilir olduğunu algılar.
3. **Çekler arasında 30 saniye.** Uç nokta sağlık kontrolü her 30 saniyede bir tekrarlanır.
4. **Hizmet kullanılamıyor.** Hizmet kullanılamaz hale gelir. Trafik Müdürü bir sonraki sağlık kontrolüne kadar bilmeyecek.
5. **İzleme yoluna erişmeye çalışır.** İzleme sistemi get isteği gerçekleştirir, ancak 10 saniyelik zaman dilimi içinde yanıt almaz (alternatif olarak, 200 olmayan bir yanıt alınabilir). Daha sonra 30 saniyelik aralıklarla üç kez daha dener. Denemelerden biri başarılı olursa, deneme sayısı sıfırlanır.
6. **Durum Bozulmuş olarak ayarlandı.** Art arda dördüncü bir hatadan sonra, izleme sistemi kullanılamayan uç nokta durumunu Bozulmuş olarak işaretler.
7. **Trafik diğer uç noktalara yönlendirilir.** Trafik Yöneticisi DNS ad sunucuları güncelleştirilir ve Trafik Yöneticisi artık DNS sorgularına yanıt olarak bitiş noktasını döndürmez. Yeni bağlantılar diğer kullanılabilir uç noktalara yönlendirilir. Ancak, bu bitiş noktasını içeren önceki DNS yanıtları yine de özyinelemeli DNS sunucuları ve DNS istemcileri tarafından önbelleğe alınabilir. İstemciler, DNS önbelleği sona erene kadar bitiş noktasını kullanmaya devam eder. DNS önbelleği sona erdiğinde, istemciler yeni DNS sorguları yapar ve farklı uç noktalara yönlendirilir. Önbellek süresi, Örneğin Trafik Yöneticisi profilindeki TTL ayarı tarafından 30 saniye olarak denetlenir.
8. **Sağlık kontrolleri devam ediyor.** Trafik Yöneticisi, bozulmuş bir durumu varken bitiş noktasının durumunu denetlemeye devam ediyor. Trafik Yöneticisi, bitiş noktasının ne zaman sağlığa döndüğünü algılar.
9. **Hizmet tekrar çevrimiçi geliyor.** Hizmet kullanılabilir hale gelir. Bitiş noktası, izleme sistemi bir sonraki sistem denetimini gerçekleştirene kadar Trafik Yöneticisi'ndeki bozulmuş durumunu korur.
10. **Hizmete trafik devam eder.** Trafik Yöneticisi bir GET isteği gönderir ve 200 Ok durum yanıtı alır. Hizmet sağlıklı bir duruma geri döndü. Trafik Yöneticisi ad sunucuları güncelleştirilir ve dns yanıtlarında hizmetin DNS adını dağıtmaya başlar. Diğer uç noktaları döndüren önbelleğe alınmış DNS yanıtları sona erdiğinde ve diğer uç noktalara varolan bağlantılar sonlandırıldıkça trafik bitiş noktasına döner.

    > [!NOTE]
    > Trafik Yöneticisi DNS düzeyinde çalıştığından, varolan bağlantıları herhangi bir uç noktaya etkileyemez. Trafik Yöneticisi, bitiş noktaları arasındaki trafiği (profil ayarları değiştirildi veya başarısız veya geri dönüş sırasında) yönlendirdiğinde, yeni bağlantıları kullanılabilir uç noktalara yönlendirir. Ancak, diğer uç noktalar, bu oturumlar sonlandırılana kadar varolan bağlantılar üzerinden trafik almaya devam edebilir. Trafiğin varolan bağlantılardan akmasını sağlamak için, uygulamaların her bitiş noktasıyla kullanılan oturum süresini sınırlaması gerekir.

## <a name="traffic-routing-methods"></a>Trafik yönlendirme yöntemleri

Bir uç noktası bozulmuş bir duruma sahipse, artık DNS sorgularına yanıt olarak döndürülür. Bunun yerine, alternatif bir bitiş noktası seçilir ve döndürülür. Profilde yapılandırılan trafik yönlendirme yöntemi, alternatif bitiş noktasının nasıl seçileceği belirlenir.

* **Öncelik**. Uç noktalar öncelik sıralı bir liste oluşturur. Listedeki ilk kullanılabilir bitiş noktası her zaman döndürülür. Bir uç nokta durumu bozulursa, bir sonraki kullanılabilir uç nokta döndürülür.
* **Ağırlıklı**. Kullanılabilir uç nokta, atanan ağırlıklarına ve diğer kullanılabilir uç noktaların ağırlıklarına göre rastgele seçilir.
* **Performans**. Son kullanıcıya en yakın bitiş noktası döndürülür. Bu bitiş noktası kullanılamıyorsa, Trafik Yöneticisi trafiği sonraki en yakın Azure bölgesindeki bitiş noktalarına taşır. İç [içe trafik yöneticisi profillerini](traffic-manager-nested-profiles.md#example-4-controlling-performance-traffic-routing-between-multiple-endpoints-in-the-same-region)kullanarak performans trafiği yönlendirmesi için alternatif başarısız planları yapılandırabilirsiniz.
* **Coğrafi .** Sorgu isteği IP'lerine göre coğrafi konuma hizmet etmek için eşlenen bitiş noktası döndürülür. Bu bitiş noktası kullanılamıyorsa, coğrafi bir konum profilde yalnızca bir bitiş noktasına eşlenebileceğinden (daha fazla ayrıntı [SSS'de](traffic-manager-FAQs.md#traffic-manager-geographic-traffic-routing-method)olduğundan) başka bir bitiş noktası başarısız olmak için seçilmez. En iyi uygulama olarak, coğrafi yönlendirmeyi kullanırken, müşterilerin profilin bitiş noktaları olarak birden fazla uç noktası olan iç içe geçmeli Trafik Yöneticisi profillerini kullanmalarını öneririz.
* **Çoklu Değer** IPv4/IPv6 adreslerine eşlenen birden çok uç nokta döndürülür. Bu profil için bir sorgu alındığı zaman, belirttiğiniz yanıt değerindeki **Maksimum kayıt sayısına** göre sağlıklı uç noktalar döndürülür. Varsayılan yanıt sayısı iki uç noktadır.
* **Alt ağ** Bir IP adresi aralıkları kümesine eşlenen bitiş noktası döndürülür. Bu IP adresinden bir istek alındığı zaman, döndürülen uç nokta, bu IP adresi için eşlenen noktadır. 

Daha fazla bilgi için [Trafik Yöneticisi trafik yönlendirme yöntemlerine](traffic-manager-routing-methods.md)bakın.

> [!NOTE]
> Tüm uygun uç noktaları bozulmuş bir duruma sahip olduğunda normal trafik yönlendirme davranışıiçin bir özel durum oluşur. Trafik Yöneticisi bir "en iyi çaba" girişimi yapar ve *tüm Bozulmuş durum bitiş noktaları aslında bir çevrimiçi durumda sanki yanıt verir.* Bu davranış, DNS yanıtında herhangi bir bitiş noktası döndürmemek olacak alternatife tercih edilir. Devre Dışı bırakılan veya Durdurulan uç noktalar izlenmez, bu nedenle trafik için uygun kabul edilmezler.
>
> Bu durum genellikle aşağıdakiler gibi hizmetin yanlış yapılandırması nedeniyle kaynaklanır:
>
> * Trafik Yöneticisi sağlık denetimlerini engelleyen bir erişim denetim listesi [ACL] .
> * Trafik yöneticisi profilindeki izleme bağlantı noktasının veya protokolün yanlış bir yapılandırması.
>
> Bu davranışın sonucu, Trafik Yöneticisi sağlık denetimleri doğru yapılandırılmamışsa, trafik yönlendirmesinden Trafik Yöneticisi düzgün *çalışıyormuş* gibi görünebilir. Ancak, bu durumda, genel uygulama kullanılabilirliğini etkileyen bitiş noktası asıması gerçekleşemez. Profilin bozulmuş bir durum değil, Çevrimiçi durumu gösterip göstermediğini denetlemek önemlidir. Çevrimiçi durum, Trafik Yöneticisi'nin sağlık denetimlerinin beklendiği gibi çalıştığını gösterir.

Sorun giderme başarısız sistem durumu denetimleri hakkında daha fazla bilgi için Azure [Trafik Yöneticisi'nde Sorun Giderme Durumu'na](traffic-manager-troubleshooting-degraded.md)bakın.

## <a name="faqs"></a>SSS

* [Trafik Yöneticisi Azure bölge hatalarına karşı dirençli mi?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#is-traffic-manager-resilient-to-azure-region-failures)

* [Kaynak grubu konumu seçimi Trafik Yöneticisi'ni nasıl etkiler?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#how-does-the-choice-of-resource-group-location-affect-traffic-manager)

* [Her bitiş noktasının geçerli durumunu nasıl belirleyebilirim?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#how-do-i-determine-the-current-health-of-each-endpoint)

* [HTTPS uç noktalarını izleyebilir miyim?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#can-i-monitor-https-endpoints)

* [Bitiş noktası eklerken IP adresi veya DNS adı kullanıyor muyum?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#do-i-use-an-ip-address-or-a-dns-name-when-adding-an-endpoint)

* [Bitiş noktası eklerken ne tür IP adresleri kullanabilirim?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#what-types-of-ip-addresses-can-i-use-when-adding-an-endpoint)

* [Tek bir profildeki farklı uç nokta adresleme türlerini kullanabilir miyim?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#can-i-use-different-endpoint-addressing-types-within-a-single-profile)

* [Gelen bir sorgunun kayıt türü, uç noktaların adresleme türüyle ilişkili kayıt türünden farklı olduğunda ne olur?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#what-happens-when-an-incoming-querys-record-type-is-different-from-the-record-type-associated-with-the-addressing-type-of-the-endpoints)

* [İç içe bir profilde IPv4 / IPv6 adresli uç noktaları olan bir profil kullanabilir miyim?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#can-i-use-a-profile-with-ipv4--ipv6-addressed-endpoints-in-a-nested-profile)

* [Trafik Yöneticisi profilimde bir web uygulaması bitiş noktası nı durdurdum, ancak yeniden başlattıktan sonra bile herhangi bir trafik alamıyorum. Bunu nasıl düzeltebilirim?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#i-stopped-an-web-application-endpoint-in-my-traffic-manager-profile-but-i-am-not-receiving-any-traffic-even-after-i-restarted-it-how-can-i-fix-this)

* [UygulamamDA HTTP veya HTTPS desteği olmasa bile Trafik Yöneticisi'ni kullanabilir miyim?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#can-i-use-traffic-manager-even-if-my-application-does-not-have-support-for-http-or-https)

* [TCP izleme kullanırken bitiş noktasından hangi özel yanıtlar gereklidir?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#what-specific-responses-are-required-from-the-endpoint-when-using-tcp-monitoring)

* [Trafik Yöneticisi kullanıcılarımı sağlıksız bir bitiş noktasından ne kadar hızlı uzaklaştırıyor?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#how-fast-does-traffic-manager-move-my-users-away-from-an-unhealthy-endpoint)

* [Bir profildeki farklı uç noktalar için farklı izleme ayarlarını nasıl belirtebilirim?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#how-can-i-specify-different-monitoring-settings-for-different-endpoints-in-a-profile)

* [HTTP üstbilgilerini Trafik Yöneticisi sağlık kontrollerine uç noktalarıma nasıl atayabilirim?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#how-can-i-assign-http-headers-to-the-traffic-manager-health-checks-to-my-endpoints)

* [Uç nokta sağlık denetimleri ne ana bilgisayar üstbilgi kullanır?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#what-host-header-do-endpoint-health-checks-use)

* [Sağlık kontrollerinin kaynaklandığı IP adresleri nelerdir?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#what-are-the-ip-addresses-from-which-the-health-checks-originate)

* [Trafik Yöneticisi'nden son noktama kaç sağlık denetimi bekleyebilirim?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#how-many-health-checks-to-my-endpoint-can-i-expect-from-traffic-manager)

* [Uç noktalarımdan biri düşerse nasıl bilgilendirilirim?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#how-can-i-get-notified-if-one-of-my-endpoints-goes-down)

## <a name="next-steps"></a>Sonraki adımlar

[Trafik Yöneticisi'nin nasıl çalıştığını](traffic-manager-how-it-works.md) öğrenin

Trafik Yöneticisi tarafından desteklenen [trafik yönlendirme yöntemleri](traffic-manager-routing-methods.md) hakkında daha fazla bilgi edinin

Trafik Yöneticisi profilini nasıl [oluşturabilirsiniz](traffic-manager-manage-profiles.md) öğrenin

Trafik Yöneticisi bitiş noktasında [ki bozulmuş durumu giderme](traffic-manager-troubleshooting-degraded.md)
