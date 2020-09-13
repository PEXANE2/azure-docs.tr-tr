---
title: 'Ağ bağlantısı performansının sorunlarını giderme: Azure'
description: Bu sayfa, Azure ağ bağlantısı performansını test etmek için standartlaştırılmış bir yöntem sağlar.
services: expressroute
author: duongau
ms.service: expressroute
ms.topic: troubleshooting
ms.date: 12/20/2017
ms.author: duau
ms.custom: seodec18
ms.openlocfilehash: 6b9a951787df6775b5159433c7172e767ff955b2
ms.sourcegitcommit: d0541eccc35549db6381fa762cd17bc8e72b3423
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/09/2020
ms.locfileid: "89566084"
---
# <a name="troubleshooting-network-performance"></a>Ağ performansı sorunlarını giderme
## <a name="overview"></a>Genel Bakış
Azure şirket içi ağınızdan Azure’a bağlanmak için kararlı ve hızlı yollar sağlar. Siteler Arası VPN ve ExpressRoute, büyük ve küçük müşteriler tarafından Azure’da işlerini yürütmek için başarıyla kullanılır. Ancak performans beklentilerinizi veya önceki deneyiminizi karşılamıyorsa ne olur? Bu belge, belirli ortamınızı test etme ve temel getirme şeklini standartlaşmanız için yardımcı olabilir.

Bu belgede, iki ana bilgisayar arasında ağ gecikmesini ve bant genişliğini nasıl kolayca ve tutarlı bir şekilde test edebileceğiniz gösterilmektedir. Bu belge Ayrıca, Azure ağı 'na bakmak ve sorun noktalarını yalıtmak için kullanabileceğiniz bazı öneriler sağlar. Ele alınan PowerShell betiği ve araçları ağ üzerinde iki ana bilgisayar gerektirir (test edilmekte olan bağlantının sonunda). Bir konağın Windows Server veya masaüstü olması gerekir, diğeri Windows veya Linux olabilir. 

>[!NOTE]
>Sorun giderme yaklaşımı, Araçlar ve Yöntemler kişisel tercihlerdir. Bu belgede, sıklıkla karşılaşdığım yaklaşım ve araçlar açıklanmaktadır. Yaklaşımınız farklı olabilir, sorun çözmeye yönelik farklı yaklaşımlar ile hiçbir şey yok. Ancak, bir yaklaşım yoksa, bu belge, ağ sorunlarını gidermek için kendi yöntemlerinizi, araçlarınızı ve tercihlerinizi oluşturmaya yönelik yol üzerinde başlamanızı sağlayabilir.
>
>

## <a name="network-components"></a>Ağ bileşenleri
Sorun gidermeye başlamadan önce bazı yaygın hüküm ve bileşenleri tartışalım. Bu tartışma, Azure 'da bağlantı sağlayan uçtan uca zincirde her bir bileşen hakkında düşünmekten emin olmanızı sağlar.
![1][1]

En yüksek düzeyde, üç ana ağ yönlendirme etki alanı açıklıyorum;

- Azure ağı (sağ tarafta mavi bulut)
- Internet veya WAN (merkezinde yeşil bulut)
- Şirket ağı (sol taraftaki pon bulut)

Sağ taraftaki diyagrama bakarak her bir bileşeni kısaca ele alalım:
 - **Sanal makine** -sunucuda birden çok NIC olabilir, herhangi bir statik yolun, varsayılan yolların ve işletim sistemi ayarlarının, olduğunu düşündüğünüz şekilde trafik göndermesini ve aldığını doğrulayın. Ayrıca, her sanal makine SKU 'SU bir bant genişliği kısıtlamasına sahiptir. Daha küçük bir VM SKU 'SU kullanıyorsanız, trafiğiniz NIC için kullanılabilen bant genişliği ile sınırlıdır. VM 'de yeterli bant genişliği sağlamak için genellikle test için bir DS5v2 kullandım (ve ardından para tasarrufu için test ile bir kez sildikten sonra).
 - **NIC** -söz konusu NIC 'ye atanan özel IP 'yi öğrendiğinizden emin olun.
 - **NIC NSG** -NIC düzeyinde uygulanan belirli NSG 'ler olabilir, NSG kural kümesinin geçirmeye çalıştığınız trafiğe uygun olduğundan emin olun. Örneğin, test trafiğinin geçmesine izin vermek için, Iperf için 5201 bağlantı noktası, RDP için 3389 veya SSH için 22 açık olduğundan emin olun.
 - **VNET alt ağı** -NIC belirli bir alt ağa atanır, bu alt ağla ilişkili bir ve kuralın hangisi olduğunu bilmenizi sağlar.
 - **Alt ağ NSG** -NIC gibi, NSG 'ler de alt ağda uygulanabilir. NSG kural kümesinin geçirmeye çalıştığınız trafiğe uygun olduğundan emin olun. (NIC 'ye gelen trafik için önce alt ağ NSG 'si geçerlidir, sonra VM 'den giden trafik için de bunun aksine NIC NSG, bu durumda, NSG alt ağı Play 'e gelir).
 - **Alt ağ UDR** -Kullanıcı tanımlı yollar, trafiği bir ara atlamaya (bir güvenlik duvarı veya yük dengeleyici gibi) yönlendirebilir. Trafiğiniz için bir UDR olup olmadığını ve bu durumda nerede giteceğimizi ve trafiğiniz için bir sonraki atlamanın ne yapacağından emin olun. (örneğin, bir güvenlik duvarı bazı trafiği geçirebilir ve aynı iki ana bilgisayar arasındaki diğer trafiği reddedebilir).
 - **Ağ geçidi alt ağı/NSG/UDR** -VM alt ağında olduğu gibi, ağ geçidi alt ağında NSG 'Ler ve udrs olabilir. Burada olup olmadığını ve trafiğiniz üzerinde sahip oldukları etkileri öğrendiğinizden emin olun.
 - **VNET Gateway (ExpressRoute)** -bir eşleme (ExpressRoute) veya VPN etkinleştirildikten sonra trafik yollarının nasıl veya ne şekilde yönlendirileceğini etkileyebilecek birçok ayar yoktur. Aynı VNet ağ geçidine bağlı birden fazla ExpressRoute devresine veya VPN tüneline sahipseniz, bu ayar bağlantı tercihini etkilediği ve trafiğinizin aldığı yolu etkilediği için bağlantı ağırlığı ayarlarını bilmelisiniz.
 - **Yol filtresi** (gösterilmez)-bir yol filtresi yalnızca ExpressRoute üzerinde Microsoft eşlemesi için geçerlidir, ancak Microsoft eşlemesi 'nde beklediğinizi denetlemek için kritik öneme sahiptir. 

Bu noktada, bağlantının WAN bölümü olursunuz. Bu yönlendirme etki alanı, hizmet sağlayıcınız, kurumsal WAN veya Internet olabilir. Bu bağlantılarla ilgili birçok sıçrama, teknoloji ve şirket, sorun gidermeyi zorlaştırır. Genellikle, bu şirket ve atlama koleksiyonuna geçmeden önce hem Azure hem de kurumsal ağlarınızı bir kez kurala göre çalışır.

Yukarıdaki diyagramda, en solda şirket ağınız bulunur. Şirketinizin boyutuna bağlı olarak, bu yönlendirme etki alanı, bir kampüs/kuruluş ağında sizin ve WAN veya birden çok cihaz katmanı arasında birkaç ağ aygıtı olabilir.

Bu üç farklı üst düzey ağ ortamının karmaşıklıkları verildiğinde, genellikle kenarlarından başlamak ve performansın iyi olduğunu ve nerede düşmekte olduğunu göstermek için en iyi seçenektir. Bu yaklaşım, üçüncü olarak sorun yönlendirme etki alanının belirlenmesini ve ardından sorun gidermeyi ilgili ortama odaklanmaya yardımcı olabilir.

## <a name="tools"></a>Araçlar
Çoğu ağ sorunu, PING ve izleme işlemi gibi temel araçlar kullanılarak analiz edilebilir ve yalıtılabilir. Wireshark gibi bir paket analizine kadar derin gitmeniz gerekir. Sorun gidermeye yardımcı olmak için Azure bağlantı araç seti (AzureCT), bu araçların bazılarını kolay bir pakette yerleştirmek için geliştirilmiştir. Performans testi için Iperf ve PSPing kullanmak istiyorum. Iperf yaygın olarak kullanılan bir araçtır ve çoğu işletim sisteminde çalışır. Iperf, temel performanons testleri için uygundur ve kullanımı oldukça kolaydır. PSPing, SysInternals tarafından geliştirilen bir ping aracıdır. PSPing, aynı zamanda kullanımı kolay bir şekilde ıCMP ve TCP pingleri gerçekleştirmenin kolay bir yoludur. Bu araçların her ikisi de hafif olur ve yalnızca dosyaları konaktaki bir dizine kaydederek "yüklenir".

Bu araçların ve yöntemlerin tümünü, yükleyip kullanabileceğiniz bir PowerShell modülüne (AzureCT) sarmalıyorum.

### <a name="azurect---the-azure-connectivity-toolkit"></a>AzureCT-Azure bağlantı araç seti
AzureCT PowerShell modülünde iki bileşen [Kullanılabilirlik testi][Availability Doc] ve [performans testi][Performance Doc]vardır. Bu belge yalnızca performans testi ile ilgilendiğinden, bu PowerShell modülündeki iki bağlantı performans komutlarına odaklanılmasını sağlar.

Performans testi için bu araç takımını kullanmanın üç temel adımı vardır. 1) PowerShell modülünü yüklemesi, 2) destekleyici uygulamaları Install Iperf ve PSPing 3) performans testini çalıştırın.

1. PowerShell modülünü yükleme

    ```powershell
    (new-object Net.WebClient).DownloadString("https://aka.ms/AzureCT") | Invoke-Expression
    
    ```

    Bu komut, PowerShell modülünü indirir ve yerel olarak yükler.

2. Destekleyici uygulamaları yükler
    ```powershell
    Install-LinkPerformance
    ```
    Bu AzureCT komutu, "C:\ACTTools" adlı yeni bir dizinde Iperf ve PSPing 'yi yüklüyor ve ayrıca, ıCMP ve bağlantı noktası 5201 (Iperf) trafiğine izin vermek için Windows Güvenlik Duvarı bağlantı noktalarını açar.

3. Performans testini çalıştırma

    İlk olarak, uzak ana bilgisayarda Iperf 'yi sunucu modunda yüklemeli ve çalıştırmanız gerekir. Ayrıca, uzak konağın 3389 (Windows için RDP) veya 22 (Linux için SSH) üzerinde dinleme yaptığını ve Iperf için bağlantı noktası 5201 ' de trafiğe izin vermesini sağlayın. Uzak ana bilgisayar Windows ise, AzureCT 'i yükleyip, Iperf 'yi ve sunucu modunda Iperf 'yi başlatmak için gereken güvenlik duvarı kurallarını ayarlamak için Install-LinkPerformance komutunu çalıştırın. 
    
    Uzak makine çalışmaya başladıktan sonra yerel makinede PowerShell 'i açın ve testi başlatın:
    ```powershell
    Get-LinkPerformance -RemoteHost 10.0.0.1 -TestSeconds 10
    ```

    Bu komut, ağ bağlantılarınızın bant genişliği kapasitesini ve gecikme süresini tahmin etmeye yardımcı olmak için bir dizi eşzamanlı yük ve gecikme süresi testi çalıştırır.

4. Testlerin çıkışını gözden geçirin

    PowerShell çıkış biçimi şuna benzer:

    ![4][4]

    Tüm Iperf ve PSPing testlerinin ayrıntılı sonuçları, "C:\ACTTools." adresindeki AzureCT araçları dizininde bulunan bireysel metin dosyalarıdır.

## <a name="troubleshooting"></a>Sorun giderme
Performans testi size beklenen sonuçlar vermekten dolayı neden bir aşamalı adım adım işlem olmalıdır. Yoldaki bileşenlerin sayısı verildiğinde, sistematik bir yaklaşım genellikle çözünürlükten daha hızlı bir yol sağlar ve aynı testi birden çok kez daha sorunsuz bir şekilde yapıyor olabilir.

>[!NOTE]
>Bu senaryo, bağlantı sorunu değil bir performans sorunudur. Trafik hiç geçirilmediyse, adımlar farklı olur.
>
>

Öncelikle varsayımlarınızı zorluk. Beklentisi makul mi? Örneğin, 1 GB/sn ExpressRoute devreniz varsa ve 100 gecikme süresi varsa, yüksek gecikme süresi bağlantıları üzerinden TCP 'nin performans özelliklerine verilen trafik için 1 GB/sn 'lik trafiği beklemeniz mantıklıdır. Performans varsayımları hakkında daha fazla bilgi için [başvurular bölümüne](#references) bakın.

Daha sonra, yönlendirme etki alanları arasındaki kenarlarından başlayıp sorunu tek bir ana yönlendirme etki alanında yalıtmak için kullanmayı öneririz; Şirket ağı, WAN veya Azure ağı. Kullanıcılar genellikle yoldaki "siyah kutusu" den daha kolay hale geiyor, ancak siyah kutunun kullanımı kolaydır, bu, özellikle de sorun değişiklik yapma yeteneğiniz olan bir alanda olduğunda çözümü önemli ölçüde erteleyebilir. Hizmet sağlayıcınıza veya ISS 'nize teslim etmeden önce, süresi dolan bir değer elde ettiğinizden emin olun.

Sorunu içeren ana yönlendirme etki alanını tanımladıktan sonra, söz konusu alanın bir diyagramını oluşturmalısınız. Bir beyaz tahta, Not defteri veya Visio 'da diyagram olarak, sorunu daha da yalıtmak için bir methodical yaklaşımına izin vermek üzere somut bir "Savaşı Map" sağlar. Test noktalarını planlayabilir ve daha derin olan veya test ilerledikçe eşlemeyi güncelleştirebilirsiniz.

Artık bir diyagramınızın olduğuna göre, ağı segmentlere bölmek ve sorunu daraltmak için başlangıç yapın. Nerede çalıştığını ve nerede çalışmadığını öğrenin. Test noktalarınızı, sorunlu bileşene ayırmak için taşımayı sürdürün.

Ayrıca, OSı modelinin diğer katmanlarına bakmaya de unutmayın. Ağ ve katmanlara 1-3 (fiziksel, veri ve ağ katmanları) odaklanmak kolaydır, ancak sorunlar uygulama katmanındaki katman 7 ' de da bulunabilir. Açık bir fikir bulundurun ve varsayımları doğrulayın.

## <a name="advanced-expressroute-troubleshooting"></a>Gelişmiş ExpressRoute sorunlarını giderme
Bulutun kenarının gerçekten olduğu konusunda emin değilseniz, Azure bileşenlerini yalıtmak bir zorluk olabilir. ExpressRoute kullanıldığında, Edge Microsoft Enterprise Edge (MSEE) adlı bir ağ bileşenidir. **ExpressRoute kullanılırken**MSEE, Microsoft 'un ağı olan ilk iletişim noktasıdır ve Microsoft ağını geçen son Atlaymadır. VNet ağ geçidiniz ile ExpressRoute bağlantı hattı arasında bir bağlantı nesnesi oluşturduğunuzda, aslında MSEE ile bağlantı kurma. İlk veya son atlama olarak MSEE 'yi (hangi yöne gittiğinize bağlı olarak), sorunu Azure 'da veya WAN veya şirket ağında daha açık bir şekilde kanıtlamak üzere Azure ağ sorunlarını yalıtmak için çok önemlidir. 

![2][2]

>[!NOTE]
> MSEE 'ın Azure bulutu 'nda olmadığına dikkat edin. ExpressRoute aslında yalnızca Azure 'da olmayan Microsoft ağının kenarında bulunur. Bir MSEE ExpressRoute ile bağlandıktan sonra, Microsoft 'un ağına bağlanırsınız, buradan Microsoft 365 (Microsoft eşlemesiyle) veya Azure (özel ve/veya Microsoft eşlemesiyle) gibi bulut hizmetlerinden birine gidebilirsiniz.
>
>

İki sanal ağ (diyagramdaki A ve B 'ler) **aynı** ExpressRoute devresine bağlıysa, sorunu Azure 'da yalıtmak için bir dizi test gerçekleştirebilirsiniz (veya Azure 'da olmadığını kanıtlayabilirsiniz)
 
### <a name="test-plan"></a>Test planı
1. VM1 ve VM2 arasında Get-LinkPerformance testini çalıştırın. Bu test, sorunun yerel olup olmadığı konusunda öngörü sağlar. Bu sınama kabul edilebilir gecikme ve bant genişliği sonuçları üretirse, yerel VNet ağını iyi şekilde işaretleyebilirsiniz.
2. Yerel VNet trafiğinin iyi olduğu varsayıldığında, VM1 ve VM3 arasında Get-LinkPerformance testini çalıştırın. Bu test, bağlantıyı Microsoft ağı üzerinden MSEE 'a ve Azure 'a yeniden sağlar. Bu sınama kabul edilebilir gecikme ve bant genişliği sonuçları üretirse, Azure ağını uygun şekilde işaretleyebilirsiniz.
3. Azure kullanıma hazır değilse, kurumsal ağınızda benzer bir test dizisi gerçekleştirebilirsiniz. Ayrıca, bu da test eder, WAN bağlantınızı tanılamak için hizmet sağlayıcınız veya ISS 'niz ile çalışmanız zaman alabilir. Örnek: Bu testi iki şube ofisi arasında ya da masa ile bir veri merkezi sunucusu arasında çalıştırın. Test ettiğinize bağlı olarak, bu yolu uygulayabilir uç noktalar (sunucular, bilgisayarlar vb.) bulun.

>[!IMPORTANT]
> Her test için, testi çalıştırdığınız günün saatini işaretlemenizi ve sonuçları ortak bir konuma (OneNote veya Excel gibi) kaydetmeniz önemlidir. Her test çalıştırmasının aynı çıkış olması gerekir, böylece sonuç verileri test çalıştırmaları genelinde karşılaştırabilir ve verilerde "delik" yoktur. Birden çok test arasında tutarlılık, sorun giderme için AzureCT 'i kullanmanın birincil nedenidir. Magic, çalıştırdığım tam yükleme senaryolarında değil, ancak her bir test ve *magic* her testten *tutarlı bir test ve veri çıktısı* aldım. Her seferinde bir kez kaydetme ve tutarlı veri sahibi olma, daha sonra sorunun tek bir biçimde olduğunu fark ediyorsanız kullanışlıdır. Veri koleksiyonunuzun önüne göz aşabilirsiniz ve aynı senaryolara yeniden test etme saatlerinizi (Bu sabit bir süre önce öğrendim) göz önüne almanız gerekir.
>
>

## <a name="the-problem-is-isolated-now-what"></a>Sorun yalıtılmış, şimdi ne?
Sorunu çözmek daha kolay olur, ancak genellikle sorun gidermenize daha ayrıntılı veya daha fazla gidebileceğiniz noktaya ulaşabilirsiniz. Örnek: Avrupa üzerinden atlama gerçekleştirerek hizmet sağlayıcınızdaki bağlantıyı görürsünüz, ancak beklediğiniz yolun hepsi Asya 'da. Bu nokta, yardım için ne zaman ulaşmalısınız. Sorun sizi, sorunu yalıtmakta olduğunuz yönlendirme etki alanına, hatta belirli bir bileşene daraltacaksanız daha iyi bir şekilde değişir.

Kurumsal ağ sorunları için, ağınızı destekleyen iç BT departmanınız veya hizmet sağlayıcınız (donanım üreticisi olabilir) cihaz yapılandırması veya donanım onarımı konusunda yardımcı olabilir.

WAN için test sonuçlarınızı hizmet sağlayıcınız veya ISS 'niz ile paylaşmak, başlamanıza yardımcı olabilir ve daha önce test ettiğiniz aynı temelden bazılarını kapsamaktan kaçınabilirsiniz. Ancak, sonuçlarınızın kendilerini doğrulamak istiyorlarsa, bu işlemi sonlandırmayın. "Güven ancak Doğrula", diğer kişilerin bildirilen sonuçlarına göre sorun giderirken iyi bir alışkanlıktır.

Azure sayesinde, sorunu mümkün olduğunca detaylı bir şekilde yalıtdıktan sonra, [Azure ağ belgelerinin][Network Docs] incelenmesi ve hala gerekliyse [bir destek bileti açmanız][Ticket Link]gerekir.

## <a name="references"></a>Başvurular
### <a name="latencybandwidth-expectations"></a>Gecikme süresi/bant genişliği beklentileri
>[!TIP]
> Test ettiğiniz bitiş noktaları arasındaki coğrafi gecikme süresi (mil veya kilometre), gecikme süresinin en büyük bileşenidir. Ekipman gecikme süresi (fiziksel ve sanal bileşenler, atlama sayısı vb.) olsa da Coğrafya, WAN bağlantılarıyla ilgilenirken genel gecikme süresinin en büyük bileşeni olarak kendini kanıtlamış. Aynı zamanda, şehirlerarası 'in, normal hat veya yol haritası mesafesi değil, fiber çalıştırmanın uzaklığı olduğunu unutmamak önemlidir. Bu mesafe, inanılmaz zor bir değer alabilir. Sonuç olarak, genellikle internet üzerinde bir şehir uzaklığı Hesaplayıcısı kullandım ve bu yöntemin farklı bir ölçü olduğunu, ancak genel bir beklentisi ayarlamak için yeterince olduğunu biliyoruz.
>
>

ABD 'de Seattle, Washington 'da bir ExpressRoute kurulumuna sahibim. Aşağıdaki tabloda, çeşitli Azure konumlarına test etmeyi kullandığım gecikme süresi ve bant genişliği gösterilmektedir. Testin her bitişi arasındaki coğrafi mesafeyi tahmin ediyorum.

Test kurulumu:
 - Bir ExpressRoute devresine bağlı 10 Gbps NIC ile Windows Server 2016 çalıştıran bir fiziksel sunucu.
 - Özel eşleme etkinken belirlenen konumda 10 Gbps Premium ExpressRoute devresi.
 - Belirtilen bölgede UltraPerformance ağ geçidine sahip bir Azure sanal ağı.
 - VNet üzerinde Windows Server 2016 çalıştıran bir DS5v2 VM. VM, varsayılan Azure görüntüsünden (iyileştirme veya özelleştirme olmadan) AzureCT yüklü olarak oluşturulan etki alanına katılmamış.
 - Tüm testler, altı test çalıştırmalarının her biri için 5 dakikalık bir yük testi ile AzureCT Get-LinkPerformance komutunu kullanıyor. Örneğin:

    ```powershell
    Get-LinkPerformance -RemoteHost 10.0.0.1 -TestSeconds 300
    ```
 - Her test için veri akışı, şirket içi fiziksel sunucudan (Seattle 'daki Iperf istemcisi) Azure VM 'ye kadar (listelenen Azure bölgesindeki Iperf sunucusu) yük akışını içeriyordu.
 - "Gecikme süresi" sütun verileri yük testi değil (Iperf çalıştıran bir TCP gecikme testi).
 - "Maks. bant genişliği" sütun verileri, 16 TCP akışı yük testinin 1 MB 'Lık bir pencere boyutuyla aynıdır.

![3][3]

### <a name="latencybandwidth-results"></a>Gecikme/bant genişliği sonuçları
>[!IMPORTANT]
> Bu sayılar yalnızca genel başvuru amaçlıdır. Birçok etken gecikme süresini etkiler ve bu değerler zaman içinde genel olarak tutarlı olsa da, Azure veya hizmet sağlayıcıları ağı içindeki koşullar herhangi bir zamanda farklı yollar aracılığıyla trafik gönderebilir, bu nedenle gecikme süresi ve bant genişliği etkilenebilir. Genellikle, bu değişikliklerin etkileri önemli farklılıklar oluşmasına neden olmaz.
>
>

| ExpressRoute<br/>Konum|Azure<br/>Bölge | Edilen<br/>Uzaklık (km) | Gecikme süresi|1 oturum<br/>Bant genişliği | Maksimum<br/>Bant genişliği |
| ------------------------------------------ | --------------------------- |  - | - | - | - |
| Seattle | Batı ABD 2        |    191 km |   5 MS | 262,0 Mbits/sn |  3,74 Gbit/sn |
| Seattle | Batı ABD          |  1.094 km |  18 MS |  82,3 Mbits/sn |  3,70 Gbit/sn |
| Seattle | Central US       |  2.357 km |  40 ms |  38,8 Mbits/sn |  2,55 Gbit/sn |
| Seattle | Orta Güney ABD |  2.877 km |  51 MS |  30,6 Mbits/sn |  2,49 Gbit/sn |
| Seattle | Orta Kuzey ABD |  2.792 km |  55 MS |  27,7 Mbits/sn |  2,19 Gbit/sn |
| Seattle | Doğu ABD 2        |  3.769 km |  73 MS |  21,3 Mbits/sn |  1,79 Gbit/sn |
| Seattle | Doğu ABD          |  3.699 km |  74 MS |  21,1 Mbits/sn |  1,78 Gbit/sn |
| Seattle | Doğu Japonya       |  7.705 km | 106 MS |  14,6 Mbits/sn |  1,22 Gbit/sn |
| Seattle | Güney Birleşik Krallık         |  7.708 km | 146 MS |  10,6 Mbits/sn |   896 Mbits/sn |
| Seattle | West Europe      |  7.834 km | 153 MS |  10,2 Mbits/sn |   761 Mbits/sn |
| Seattle | Doğu Avustralya   | 12.484 km | 165 MS |   9,4 Mbits/sn |   794 Mbits/sn |
| Seattle | Güneydoğu Asya   | 12.989 km | 170 MS |   9,2 Mbits/sn |   756 Mbits/sn |
| Seattle | Brezilya Güney *   | 10.930 km | 189 MS |   8,2 Mbits/sn |   699 Mbits/sn |
| Seattle | Güney Hindistan      | 12.918 km | 202 MS |   7,7 Mbits/sn |   634 Mbits/sn |

\* Brezilya gecikmesi, düz çizgili mesafenin fiber çalışma uzaklığına göre önemli ölçüde farklı olduğu iyi bir örnektir. Gecikme süresinin 160 ms 'in komşuları içinde olmasını bekler, ancak gerçekten 189 MS olur. Beklentilerimin bu farkı, bir ağ sorununun bir yerde olduğunu gösteriyor olabilir, ancak büyük olasılıkla fiber çalıştırmanın Brezilya 'ye doğrudan bir satırda gitmemesi ve Seattle 'dan Brezilya 'ye gitmesi için fazladan 1.000 km veya buna karşılık gelen bir yolculuğu vardır.

## <a name="next-steps"></a>Sonraki adımlar
1. Azure bağlantı araç setini GitHub 'dan yükleme [https://aka.ms/AzCT][ACT]
2. [Bağlantı performansı testi][Performance Doc] için yönergeleri izleyin

<!--Image References-->
[1]: ./media/expressroute-troubleshooting-network-performance/network-components.png "Azure ağ bileşenleri"
[2]: ./media/expressroute-troubleshooting-network-performance/expressroute-troubleshooting.png "ExpressRoute sorunlarını giderme"
[3]: ./media/expressroute-troubleshooting-network-performance/test-diagram.png "Perf test ortamı"
[4]: ./media/expressroute-troubleshooting-network-performance/powershell-output.png "PowerShell çıkışı"

<!--Link References-->
[Performance Doc]: https://github.com/Azure/NetworkMonitoring/blob/master/AzureCT/PerformanceTesting.md
[Availability Doc]: https://github.com/Azure/NetworkMonitoring/blob/master/AzureCT/AvailabilityTesting.md
[Network Docs]: https://docs.microsoft.com/azure/index
[Ticket Link]: https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview
[ACT]: https://aka.ms/AzCT
