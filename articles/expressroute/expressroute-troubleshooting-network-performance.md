---
title: 'Sorun giderme ağ bağlantısı performansı: Azure'
description: Bu sayfa, Azure ağ bağlantısı performansını test etmek için standartlaştırılmış bir yöntem sağlar.
services: expressroute
author: tracsman
ms.service: expressroute
ms.topic: article
ms.date: 12/20/2017
ms.author: jonor
ms.custom: seodec18
ms.openlocfilehash: bb68919fba731caa32dcca3f4c991b8881afc6f9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74869655"
---
# <a name="troubleshooting-network-performance"></a>Ağ performansı sorunlarını giderme
## <a name="overview"></a>Genel Bakış
Azure şirket içi ağınızdan Azure’a bağlanmak için kararlı ve hızlı yollar sağlar. Siteler Arası VPN ve ExpressRoute, büyük ve küçük müşteriler tarafından Azure’da işlerini yürütmek için başarıyla kullanılır. Ama performans beklentinizi veya önceki deneyiminizi karşılamadığında ne olur? Bu belge, belirli ortamınızı test ve temellendirme şeklinizi standartlaştırmanıza yardımcı olabilir.

Bu belge, iki ana bilgisayar arasında ağ gecikmesi ve bant genişliğini nasıl kolayca ve tutarlı bir şekilde test edebileceğinizi gösterir. Bu belge ayrıca Azure ağına bakmanın ve sorun noktalarını yalıtmaya yardımcı olmanın yolları hakkında bazı öneriler de sağlar. Tartışılan PowerShell komut dosyası ve araçları ağda iki ana bilgisayar gerektirir (bağlantının her iki ucunda test ediliyor). Bir ana bilgisayar Windows Server veya Masaüstü olmalıdır, diğer veya Windows veya Linux olabilir. 

>[!NOTE]
>Sorun giderme yaklaşımı, araçlar ve kullanılan yöntemler kişisel tercihlerdir. Bu belge, sık sık almak yaklaşım ve araçları açıklar. Yaklaşımınız muhtemelen farklı olacak, problem çözme için farklı yaklaşımlar ile yanlış bir şey yok. Ancak, yerleşik bir yaklaşımınız yoksa, bu belge ağ sorunlarını giderme için kendi yöntemlerinizi, araçlarınızı ve tercihlerinizi oluşturma yolunda çalışmaya başlamanızı sağlayabilir.
>
>

## <a name="network-components"></a>Ağ bileşenleri
Sorun giderme içine kazma önce, bazı ortak terimler ve bileşenleri tartışalım. Bu tartışma, Azure'da bağlantı sağlayan uçlardan uca zincirdeki her bileşeni düşünmemizi sağlar.
![1][1]

En üst düzeyde, üç büyük ağ yönlendirme etki alanlarını tanımlıyorum;

- Azure ağı (sağdaki mavi bulut)
- Internet veya WAN (merkezde yeşil bulut)
- Corporate Network (solda şeftali bulutu)

Diyagrama sağdan sola baktığımızda, her bileşeni kısaca tartışalım:
 - **Sanal Makine** - Sunucu, statik yolları, varsayılan yolları ve İşletim Sistemi ayarları nın istediğiniz gibi trafik gönderip aldığından emin olmak için birden çok NIC'e sahip olabilir. Ayrıca, her VM SKU bir bant genişliği kısıtlaması vardır. Daha küçük bir VM SKU kullanıyorsanız, trafiğiniz NIC'nin kullanabileceği bant genişliğiyle sınırlıdır. Ben genellikle vm yeterli bant genişliği sağlamak için test etmek için bir DS5v2 kullanın (ve sonra bir kez para kazanmak için test ile yapılır silin) .
 - **NIC** - Söz konusu NIC'e atanan özel IP'yi bildiğinizden emin olun.
 - **NIC NSG** - NIC düzeyinde uygulanan belirli NSG'ler olabilir, NSG kural kümesinin geçmeye çalıştığınız trafiğe uygun olduğundan emin olun. Örneğin, test trafiğinin geçmesine izin vermek için iPerf için 5201, RDP için 3389 veya SSH için 22 bağlantı noktasının açık olduğundan emin olun.
 - **VNet Subnet** - NIC belirli bir alt ağa atanır, hangisinin ve bu alt ağla ilişkili kuralları bildiğinizden emin olun.
 - **Subnet NSG** - Nic gibi, NSG'ler de alt ağda uygulanabilir. NSG kural kümesinin geçmeye çalıştığınız trafiğe uygun olduğundan emin olun. (NIC'e gelen trafik için alt ağ NSG önce geçerlidir, sonra NIC NSG, tam tersine VM'den giden trafik için NIC NSG önce uygulanır, sonra Subnet NSG devreye girer).
 - **Subnet UDR** - Kullanıcı Tanımlı Rotalar trafiği bir ara atlamaya (güvenlik duvarı veya yük dengeleyicisi gibi) yönlendirebilir. Eğer trafik için yerinde bir UDR olup olmadığını ve eğer öyleyse nereye gider ve bir sonraki atlama trafik için ne yapacağını bildiğinizden emin olun. (örneğin, bir güvenlik duvarı bazı trafiği geçebilir ve aynı iki ana bilgisayar arasındaki diğer trafiği reddedebilir).
 - **Ağ geçidi alt ağı / NSG / UDR** - VM alt ağı gibi, ağ geçidi alt ağı nsg'leri ve ÜD'leri olabilir. Orada olup olmadıklarını ve trafiğinizi nasıl etkilediklerini bildiğinizden emin olun.
 - **VNet Ağ Geçidi (ExpressRoute)** - Bir kez bakan (ExpressRoute) veya VPN etkinleştirildikten sonra, trafik yollarının nasıl veya olup olmadığını etkileyebilecek çok fazla ayar yoktur. Aynı VNet Ağ Geçidi'ne bağlı birden çok ExpressRoute devreniz veya VPN tüneliniz varsa, bu ayar bağlantı tercihini etkilediği ve trafiğinizin izlediği yolu etkilediği için bağlantı ağırlığı ayarlarını biliyor olmalısınız.
 - **Rota Filtresi** (Gösterilmez) - Bir rota filtresi yalnızca ExpressRoute'daki Microsoft Peering için geçerlidir, ancak Microsoft Peering'de beklediğiniz yolları görüp görmediğiniz konusunda kontrol etmek çok önemlidir. 

Bu noktada, bağlantının WAN bölümündesiniz. Bu yönlendirme etki alanı servis sağlayıcınız, şirketiniz WAN veya Internet olabilir. Bu bağlantılarla ilgili birçok atlama, teknoloji ve şirket sorun gidermede biraz zorolabilir. Genellikle, bu şirket ve atlama koleksiyonuna atlamadan önce hem Azure'u hem de Kurumsal Ağlarınızı ekarte etmek için çalışırsınız.

Bir önceki diyagramda, en solda şirket ağınız yer alıyor. Şirketinizin boyutuna bağlı olarak, bu yönlendirme etki alanı siz ve WAN arasında birkaç ağ aygıtı veya bir kampüs/kuruluş ağındaki birden çok aygıt katmanı olabilir.

Bu üç farklı üst düzey ağ ortamının karmaşıklığı göz önüne alındığında, genellikle kenarlardan başlamak ve performansın nerede iyi olduğunu ve nerede bozulduğunu göstermeye çalışmak en uygun uyrur. Bu yaklaşım, üç ünün sorunlu yönlendirme etki alanını belirlemeye ve sorun giderme nizi bu belirli ortama odaklamaya yardımcı olabilir.

## <a name="tools"></a>Araçlar
Ağ sorunlarının çoğu ping ve izleme yolu gibi temel araçlar kullanılarak analiz edilebilir ve yalıtılabilir. Wireshark gibi bir paket analizi kadar derine inmeniz çok nadirdir. Sorun gidermeye yardımcı olmak için, Azure Bağlantı Araç Seti (AzureCT), bu araçlardan bazılarını kolay bir pakete koymak için geliştirilmiştir. Performans testi için, iPerf ve PSPing kullanmak istiyorum. iPerf yaygın olarak kullanılan bir araçtır ve çoğu işletim sisteminde çalışır. iPerf temel performans testleri için iyidir ve kullanımı oldukça kolaydır. PSPing SysInternals tarafından geliştirilen bir ping aracıdır. PSPing, ICMP ve TCP ping'lerini tek bir komutla gerçekleştirmenin kolay bir yoludur. Bu araçların her ikisi de hafif ve sadece ana bilgisayarda bir dizin dosyaları başa çıkararak "yüklü".

Tüm bu araçları ve yöntemleri yükleyebileceğiniz ve kullanabileceğiniz bir PowerShell modülüne (AzureCT) sardım.

### <a name="azurect---the-azure-connectivity-toolkit"></a>AzureCT - Azure Bağlantı Araç Seti
AzureCT PowerShell modülünde [kullanılabilirlik testi][Availability Doc] ve [performans testi][Performance Doc]olmak için iki bileşen vardır. Bu belge yalnızca Performans testi ile ilgilidir, bu nedenle bu PowerShell modülündeki iki Bağlantı Performansı komutuna odaklanmanızı sağlar.

Performans testi için bu araç kitini kullanmak için üç temel adım vardır. 1) PowerShell modüllerini kurun, 2) Destekleyici uygulamaları iPerf ve PSPing 3) Performans testini çalıştırın.

1. PowerShell Modülünün Kurulumu

    ```powershell
    (new-object Net.WebClient).DownloadString("https://aka.ms/AzureCT") | Invoke-Expression
    
    ```

    Bu komut PowerShell modüllerini indirir ve yerel olarak yükler.

2. Destekleyici uygulamaları yükleme
    ```powershell
    Install-LinkPerformance
    ```
    Bu AzureCT komutu iPerf ve PSPing'i yeni bir dizine yükler "C:\ACTTools", ICMP ve port 5201 (iPerf) trafiğine izin vermek için Windows Güvenlik Duvarı bağlantı noktalarını da açar.

3. Performans testini çalıştırma

    İlk olarak, uzak ana bilgisayarda iPerf'i sunucu modunda yüklemeniz ve çalıştırmanız gerekir. Ayrıca, uzak ana bilgisayar3389 (Windows için RDP) veya 22 (Linux için SSH) üzerinde dinleme ve iPerf için port 5201 trafiğine izin emin olun. Uzak ana bilgisayar windows'sa, iPerf'i ve iPerf'i sunucu modunda başarıyla başlatmak için gereken güvenlik duvarı kurallarını ayarlamak için AzureCT'u yükleyin ve Install-LinkPerformance komutunu çalıştırın. 
    
    Uzak makine hazır olduğunda, PowerShell'i yerel makinede açın ve testi başlatın:
    ```powershell
    Get-LinkPerformance -RemoteHost 10.0.0.1 -TestSeconds 10
    ```

    Bu komut, ağ bağlantınızın bant genişliği kapasitesini ve gecikme süresini tahmin etmeye yardımcı olmak için bir dizi eşzamanlı yük ve gecikme sonu testi çalıştırAr.

4. Testlerin çıktısını gözden geçirin

    PowerShell çıkış biçimi aşağıdakilere benzer:

    ![4][4]

    Tüm iPerf ve PSPing testlerinin ayrıntılı sonuçları, "C:\ACTTools" düzeyindeki AzureCT araçları dizinindeki tek tek metin dosyalarında yer alır.

## <a name="troubleshooting"></a>Sorun giderme
Performans testi size beklenen sonuçları vermiyorsa, neden aşamalı bir adım adım işlem olması gerektiğini bulmak. Yoldaki bileşenlerin sayısı göz önüne alındığında, sistematik bir yaklaşım genellikle etrafında atlama ve potansiyel olarak aynı testi birden çok kez yapıyor daha çözünürlüğe daha hızlı bir yol sağlar.

>[!NOTE]
>Buradaki senaryo bir performans sorunudur, bağlantı sorunu değil. Trafik hiç geçmiyor olsaydı adımlar farklı olurdu.
>
>

İlk olarak, varsayımlarınıza meydan okuyun. Beklentiniz makul mü? Örneğin, 1 Gbps ExpressRoute devreniz ve 100 ms gecikmeniz varsa, tcp'nin yüksek gecikme liyaka bağlantıları üzerinden performans özellikleri göz önüne alındığında tam 1 Gbps trafiğini beklemek mantıksızdır. Performans varsayımları hakkında daha fazla şey için [Başvurular bölümüne](#references) bakın.

Ardından, yönlendirme etki alanları arasındaki kenarlardan başlamanızı ve sorunu tek bir ana yönlendirme etki alanına yalıtmayı denemenizi öneririm; Kurumsal Ağ, WAN veya Azure Ağı. İnsanlar genellikle yolda "kara kutu" suçlamak, kara kutu suçlayarak yapmak kolaydır, özellikle sorun aslında değişiklik yapma yeteneğine sahip bir alanda ise çözünürlüğü önemli ölçüde geciktirebilir. Servis sağlayıcınıza veya ISS'ye teslim etmeden önce durum tespitinizi yaptığınızdan emin olun.

Sorunu içeren ana yönlendirme etki alanını tanımladıktan sonra, söz konusu alanın bir diyagramını oluşturmanız gerekir. Bir beyaz tahta, not defteri veya Visio bir diyagram olarak daha fazla sorunu izole etmek için metodik bir yaklaşım sağlamak için somut bir "savaş haritası" sağlar. Test noktaları planlayabilir ve alanları temizledikçe veya test ilerledikçe daha derine indikçe haritayı güncelleyebilirsiniz.

Artık bir diyagramınuz olduğuna göre, ağı bölümlere bölmeye başlayın ve sorunu daraltın. Nerede çalışıp çalışmadığını öğren. Test noktalarınızı rahatsız edici bileşene yalıtmak için taşımaya devam edin.

Ayrıca, OSI modelinin diğer katmanlarına bakmayı unutmayın. Ağa ve 1 -3 katmanlarına (Fiziksel, Veri ve Ağ katmanları) odaklanmak kolaydır, ancak sorunlar uygulama katmanındaki Katman 7'de de olabilir. Açık fikirli ol ve varsayımları doğrula.

## <a name="advanced-expressroute-troubleshooting"></a>Gelişmiş ExpressRoute sorun giderme
Bulutun kenarının gerçekte nerede olduğundan emin değilseniz, Azure bileşenlerini yalıtmak zor olabilir. ExpressRoute kullanıldığında, kenar Microsoft Enterprise Edge (MSEE) adı verilen bir ağ bileşenidir. **ExpressRoute kullanırken,** MSEE Microsoft'un ağına ilk temas noktası ve Microsoft ağı bırakarak son atlama. VNet ağ geçidiniz ile ExpressRoute devresi arasında bir bağlantı nesnesi oluşturduğunuzda, aslında MSEE'ye bir bağlantı kurarsınız. MSEE'yi ilk veya son atlayış olarak tanımak (hangi yöne gittiğinize bağlı olarak) Azure Ağı sorunlarını yada sorunun Azure'da veya WAN veya Corporate Network'te daha aşağıda olduğunu kanıtlamak için yalıtmak için çok önemlidir. 

![2][2]

>[!NOTE]
> MSEE'nin Azure bulutu içinde olmadığına dikkat edin. ExpressRoute aslında Azure'da değil, Microsoft ağının kenarındadır. ExpressRoute'a bir MSEE'ye bağlandıktan sonra Microsoft'un ağına bağlanabilirsiniz ve buradan Office 365 (Microsoft Peering ile) veya Azure (Özel ve/veya Microsoft Peering ile) gibi bulut hizmetlerinden herhangi biri için gidebilirsiniz.
>
>

İki VNets (diyagramdaki VNets A ve B) **aynı** ExpressRoute devresine bağlıysa, sorunu Azure'da yalıtmak (veya Azure'da olmadığını kanıtlamak) için bir dizi test gerçekleştirebilirsiniz
 
### <a name="test-plan"></a>Test planı
1. VM1 ve VM2 arasındaki Get-LinkPerformance testini çalıştırın. Bu test, sorunun yerel olup olmadığını anlamasağlar. Bu test kabul edilebilir gecikme sonu ve bant genişliği sonuçları üretirse, yerel VNet ağını iyi olarak işaretleyebilirsiniz.
2. Yerel VNet trafiğinin iyi olduğunu varsayarsak, VM1 ve VM3 arasındaki Get-LinkPerformance testini çalıştırın. Bu test, Microsoft ağı üzerinden MSEE'ye ve Azure'a olan bağlantıyı çalıştırZ. Bu test kabul edilebilir gecikme sonu ve bant genişliği sonuçları üretirse, Azure ağını iyi olarak işaretleyebilirsiniz.
3. Azure ekarte edilirse, Kurumsal Ağınızda benzer bir test dizisi gerçekleştirebilirsiniz. Bu da iyi test ederse, WAN bağlantınızı tanılamak için servis sağlayıcınızla veya ISS ile çalışma zamanı dır. Örnek: Bu testi iki şube ofisi veya masanız ile veri merkezi sunucusu arasında çalıştırın. Neyi test ettiğinize bağlı olarak, bu yolu çalıştırabilecek uç noktaları (sunucular, bilgisayarlar, vb.) bulun.

>[!IMPORTANT]
> Her test için testi çalıştırdığınız günün saatini işaretlemeniz ve sonuçları ortak bir konuma kaydetmeniz çok önemlidir (OneNote veya Excel'i seviyorum). Sonuç verileri test çalıştırmaları arasında karşılaştırabilmeniz ve verilerde "boşluklar" olmaması için her test çalışmasının aynı çıktıya sahip olması gerekir. Birden çok testte tutarlılık, sorun giderme için AzureCT'u kullanmamın birincil nedenidir. Sihirli ben çalıştırmak tam yük senaryoları değil, ama bunun yerine *sihirli* ben tutarlı bir *test ve her testten veri çıkışı* almak gerçektir. Daha sonra sorunun düzensiz olduğunu bulursanız, zamanı kaydetmek ve her seferinde tutarlı verilere sahip olmak özellikle yararlıdır. Ön veri toplama ile gayretli olun ve aynı senaryoları yeniden test saat önlemek olacak (Ben bu zor yolu yıllar önce öğrendim).
>
>

## <a name="the-problem-is-isolated-now-what"></a>Sorun izole edilmiş, şimdi ne?
Sorunu ne kadar çok yalıtabilirseniz, sorun giderme ile daha derin veya daha ileri gidemezsiniz noktasına genellikle ulaşırsınız. Örnek: Servis sağlayıcınızın Avrupa'da hop'ları gezdiren bağlantınızı görürsünüz, ancak beklenen yolun uzadı. Bu noktada yardım için uzanmak gerekir. Sorduğunuz kişi, sorunu yalıttırdığınız yönlendirme etki alanına bağlıdır veya belirli bir bileşene daraltabilirseniz daha da iyidir.

Kurumsal ağ sorunları için, ağınızı destekleyen dahili BT departmanınız veya servis sağlayıcınız (donanım üreticisi olabilir) aygıt yapılandırması veya donanım onarımı konusunda yardımcı olabilir.

WAN için, test sonuçlarınızı Servis Sağlayıcınız veya ISS ile paylaşmak, test ettiğiniz zeminin bazılarını kapatmaya ve bunların başlatılmasına yardımcı olabilir. Ancak, sonuçlarınızı doğrulamak isterlerse gücenmeyin. "Güven ama doğrulamak" diğer kişilerin bildirilen sonuçlarına dayalı sorun giderme iyi bir slogandır.

Azure ile, sorunu mümkün olduğunca ayrıntılı olarak yalıttıktan sonra, [Azure Ağ Dokümantasyon'u][Network Docs] gözden geçirme nin ve gerekirse [bir destek bileti açmanın][Ticket Link]zamanı geldi.

## <a name="references"></a>Başvurular
### <a name="latencybandwidth-expectations"></a>Gecikme/bant genişliği beklentileri
>[!TIP]
> Test ettiğiniz bitiş noktaları arasındaki coğrafi gecikme (kilometre veya kilometre) gecikmenin açık ara en büyük bileşenidir. Ekipman gecikmesi (fiziksel ve sanal bileşenler, atlama sayısı, vb.) söz konusu olmakla birlikte, coğrafya nın WAN bağlantılarıyla uğraşırken genel gecikmenin en büyük bileşeni olduğu kanıtlanmıştır. Ayrıca bu mesafe lif mesafe değil düz çizgi veya yol haritası mesafe çalıştırmak olduğunu unutmayın. Bu mesafeherhangi bir doğruluk ile elde etmek inanılmaz derecede zordur. Sonuç olarak, ben genellikle internet üzerinde bir şehir mesafe hesap makinesi kullanmak ve bu yöntem in brüt yanlış bir ölçü olduğunu biliyorum, ama genel bir beklenti ayarlamak için yeterlidir.
>
>

Seattle'da, Washington'da bir ExpressRoute kurulumum var. Aşağıdaki tablo, çeşitli Azure konumlarında test edilen gecikme yi ve bant genişliğini gösterir. Testin her iki sonu arasındaki coğrafi mesafeyi tahmin ettim.

Test kurulumu:
 - Windows Server 2016'yı 10 Gbps NIC ile çalıştıran ve ExpressRoute devresine bağlı fiziksel bir sunucu.
 - Özel Peering ile tanımlanan konumda 10Gbps Premium ExpressRoute devresi etkin.
 - Belirtilen bölgede UltraPerformance ağ geçidi olan bir Azure VNet.
 - VNet'te Windows Server 2016 çalıştıran bir DS5v2 VM. VM, AzureCT yüklü varsayılan Azure görüntüsünden (optimizasyon veya özelleştirme yok) oluşturulmuş, etki alanı dışında birleştirilmişti.
 - Tüm testler, altı test çalışmasının her biri için 5 dakikalık bir yükleme testi yle AzureCT Get-LinkPerformance komutunu kullanıyordu. Örnek:

    ```powershell
    Get-LinkPerformance -RemoteHost 10.0.0.1 -TestSeconds 300
    ```
 - Her testiçin veri akışı, şirket içi fiziksel sunucudan (Seattle'daki iPerf istemcisi) Azure VM'ye (listelenen Azure bölgesindeki iPerf sunucusu) kadar akan yükü taşıyordu.
 - "Gecikme" sütun verileri Yük Yok testinden (iPerf çalışmayan bir TCP gecikme testi) gelir.
 - "Max Bant Genişliği" sütun verileri 1-Mb pencere boyutuile 16 TCP akış yük testinden gelir.

![3][3]

### <a name="latencybandwidth-results"></a>Gecikme/bant genişliği sonuçları
>[!IMPORTANT]
> Bu numaralar yalnızca genel başvuru içindir. Birçok etken gecikme süresini etkiler ve bu değerler genellikle zaman içinde tutarlı olsa da, Azure veya Hizmet Sağlayıcıları ağındaki koşullar trafiği herhangi bir zamanda farklı yollardan gönderebilir, böylece gecikme süresi ve bant genişliği etkilenebilir. Genellikle, bu değişikliklerin etkileri önemli farklılıklara neden olmaz.
>
>

| | | | | | |
|-|-|-|-|-|-|
|ExpressRoute<br/>Konum|Azure<br/>Bölge|Tahmini<br/>Mesafe (km)|Gecikme süresi|1 Oturum<br/>Bant genişliği|Maksimum<br/>Bant genişliği|
| Seattle | Batı ABD 2        |    191 km |   5 ms | 262.0 Mbits/sn |  3,74 Gbits/sn |
| Seattle | Batı ABD          |  1.094 km |  18 ms |  82.3 Mbits/sn |  3,70 Gbits/sn |
| Seattle | Orta ABD       |  2.357 km |  40 ms |  38.8 Mbits/sn |  2,55 Gbits/sn |
| Seattle | Orta Güney ABD |  2.877 km |  51 ms |  30.6 Mbits/sn |  2,49 Gbits/sn |
| Seattle | Orta Kuzey ABD |  2.792 km |  55 ms |  27.7 Mbits/sn |  2,19 Gbits/sn |
| Seattle | Doğu ABD 2        |  3,769 km |  73 ms |  21.3 Mbits/sn |  1.79 Gbits/sn |
| Seattle | Doğu ABD          |  3.699 km |  74 ms |  21.1 Mbits/sn |  1,78 Gbits/sn |
| Seattle | Doğu Japonya       |  7.705 km | 106 ms |  14.6 Mbits/sn |  1.22 Gbits/sn |
| Seattle | Güney Birleşik Krallık         |  7.708 km | 146 ms |  10.6 Mbits/sn |   896 Mbits/sn |
| Seattle | Batı Avrupa      |  7.834 km | 153 ms |  10.2 Mbits/sn |   761 Mbits/sn |
| Seattle | Doğu Avustralya   | 12.484 km | 165 ms |   9.4 Mbits/sn |   794 Mbits/sn |
| Seattle | Güneydoğu Asya   | 12.989 km | 170 ms |   9.2 Mbits/sn |   756 Mbits/sn |
| Seattle | Brezilya Güney *   | 10.930 km | 189 ms |   8.2 Mbits/sn |   699 Mbits/sn |
| Seattle | Güney Hindistan      | 12.918 km | 202 ms |   7.7 Mbits/sn |   634 Mbits/sn |

\*Brezilya'ya gecikme, düz çizgi mesafesinin lif çalışma mesafesinden önemli ölçüde farklı olduğu iyi bir örnektir. Ben gecikme 160 ms mahallede olacağını tahmin ediyorum, ama aslında 189 ms. Benim beklenti karşı bu fark bir yerde bir ağ sorunu gösterebilir, ama fiber çalıştırmak düz bir çizgide Brezilya gitmez ve seattle Brezilya'ya almak için ekstra bir 1.000 km ya da seyahat kadar büyük olasılıkla.

## <a name="next-steps"></a>Sonraki adımlar
1. Azure Bağlantı Araç Kiti'ni GitHub'dan indirin[https://aka.ms/AzCT][ACT]
2. Bağlantı performans [testi][Performance Doc] için yönergeleri izleyin

<!--Image References-->
[1]: ./media/expressroute-troubleshooting-network-performance/network-components.png "Azure Ağ Bileşenleri"
[2]: ./media/expressroute-troubleshooting-network-performance/expressroute-troubleshooting.png "ExpressRoute Sorun Giderme"
[3]: ./media/expressroute-troubleshooting-network-performance/test-diagram.png "Perf Test Ortamı"
[4]: ./media/expressroute-troubleshooting-network-performance/powershell-output.png "PowerShell Çıkışı"

<!--Link References-->
[Performance Doc]: https://github.com/Azure/NetworkMonitoring/blob/master/AzureCT/PerformanceTesting.md
[Availability Doc]: https://github.com/Azure/NetworkMonitoring/blob/master/AzureCT/AvailabilityTesting.md
[Network Docs]: https://docs.microsoft.com/azure/index
[Ticket Link]: https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview
[ACT]: https://aka.ms/AzCT
