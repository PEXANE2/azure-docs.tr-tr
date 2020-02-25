---
title: App Service Ortamı kullanma ve yönetme
description: Azure App Service ortamında uygulama oluşturma, yayımlama ve ölçekleme. Ortak görevleri tek bir belgede bulabilirsiniz.
author: ccompy
ms.assetid: a22450c4-9b8b-41d4-9568-c4646f4cf66b
ms.topic: article
ms.date: 01/01/2020
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: 7be1676c8949cd30d5e1fe93a73afd75a5a9b67f
ms.sourcegitcommit: f27b045f7425d1d639cf0ff4bcf4752bf4d962d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/23/2020
ms.locfileid: "77565706"
---
# <a name="use-an-app-service-environment"></a>App Service ortamı kullanma #

App Service Ortamı (Ao), bir müşterinin Azure sanal ağındaki bir alt ağa Azure App Service bir dağıtımdır. Bir asa aşağıdakilerden oluşur:

- **Ön uçlar**: ön uçlar, http/https 'nin bir App Service ortamında sonlandırmakta olduğu yerdir.
- **Çalışanlar**: uygulamalarınızı barındıran kaynaklar.
- **Veritabanı**: veritabanı ortamı tanımlayan bilgileri barındırır.
- **Depolama**: depolama alanı, müşteri tarafından yayımlanan uygulamaları barındırmak için kullanılır.

Uygulama erişimi için bir dış veya iç VIP ile ATıCı dağıtabilirsiniz. Dış VIP ile dağıtım genellikle dış Ao olarak adlandırılır. İç sürüm, iç yük dengeleyici (ıLB) kullandığından ıLB aşırı olarak adlandırılır. ILB Ao hakkında daha fazla bilgi edinmek için bkz. [ILB Ai oluşturma ve kullanma][MakeILBASE].

## <a name="create-an-app-in-an-ase"></a>AS 'de uygulama oluşturma ##

Ao 'da bir uygulama oluşturmak için, normal olarak oluşturduğunuz ve birkaç küçük farklılık ile aynı süreci kullanırsınız. Yeni bir App Service planı (ASP) oluşturduğunuzda:

- Uygulamanızın dağıtılacağı coğrafi bir konum seçmek yerine konumunuz olarak bir AO seçin.
- Ao 'da oluşturulan tüm App Service planları yalnızca yalıtılmış bir fiyatlandırma katmanında yer alabilir.

Ao 'a sahip değilseniz, [App Service ortamı oluşturma][MakeExternalASE]bölümündeki yönergeleri izleyerek bir tane oluşturabilirsiniz.

AS 'de bir uygulama oluşturmak için:

1. **Web uygulaması** > **Web ve mobil** > **kaynak oluştur** ' u seçin.

2. Uygulama için bir ad girin. Ao 'da zaten bir App Service planı seçtiyseniz, uygulamanın etki alanı adı Ao 'nun etki alanı adını yansıtır.

    ![Uygulama adı seçimi][1]

1. Bir abonelik seçin.

1. Yeni bir kaynak grubu için bir ad girin veya **var olanı kullan** ' ı seçin ve açılan listeden birini seçin.

1. İşletim sisteminizi seçin. 

1. AŞIRDE mevcut bir App Service planı seçin veya aşağıdaki adımları izleyerek yeni bir tane oluşturun:

    a. Azure portal sol taraftaki menüden **bir kaynak oluştur > Web uygulaması**' nı seçin.

    b. Aboneliği seçin.
    
    c. Kaynak grubunu seçin veya oluşturun.
    
    d. Web uygulamanızın adını belirtin.
    
    e. Kod veya DockerContainer seçin.
    
    f. Çalışma zamanı yığınını seçin.
    
    g. Linux veya Windows 'u seçin. 
    
    h. **Bölge** açılır listesinden Ao 'nizi seçin. 
    
    i. Yeni bir App Service planı seçin veya oluşturun. Yeni bir App Service planı oluşturuyorsanız uygun **yalıtılmış** SKU boyutunu seçin.
    
    ![Yalıtılmış fiyatlandırma katmanları][2]

    > [!NOTE]
    > Linux uygulamaları ve Windows uygulamaları aynı App Service planında olamaz, ancak aynı App Service Ortamı olabilir. 
    >

2. **Gözden geçir + oluştur** ' u seçin ve bilgiler doğruysa **Oluştur** ' u seçin.

## <a name="how-scale-works"></a>Ölçeklendirmenin çalışması ##

Her App Service uygulaması bir App Service planında çalışır. App Service ortamlar, App Service planlarını ve App Service planlar uygulamaları tutar. Bir uygulamayı ölçeklendirirseniz, App Service planı ölçeklendirebilirsiniz ve böylece aynı plandaki tüm uygulamaları ölçeklendirebilirsiniz.

Bir App Service planını ölçeklendirirseniz, gerekli altyapı otomatik olarak eklenir. Altyapı eklenirken, işlemleri ölçeklendirmek için zaman gecikmesi vardır. Sırayla birkaç ölçeklendirme işlemi yaparsanız, ilk altyapı ölçeği isteği üzerinde işlem yapılır ve diğerleri kuyruğa alır. İlk ölçeklendirme işlemi tamamlandığında, diğer altyapı istekleri birlikte çalışır. Altyapı eklendiğinde App Service planları uygun şekilde atanır. Yeni bir App Service planının oluşturulması, ek donanım istediğinde bir ölçeklendirme işlemidir. 

Çok kiracılı App Service, bir kaynak havuzu bunu destekleyecek şekilde kullanıma hazır olduğundan ölçekleme anında gerçekleşir. Bir Ao 'da bu tür bir arabellek yoktur ve kaynaklar ihtiyaç duymak üzere ayrılır.

Bir Ao 'da, bir App Service planı 100 örneğe kadar ölçeklendirebilirsiniz. ATıCı, Ao 'da bulunan tüm App Service planlarında en fazla 201 toplam örneğe sahip olabilir. 

## <a name="ip-addresses"></a>IP adresleri ##

App Service bir uygulamaya adanmış bir IP adresi ayırabilme olanağı vardır. Bir uygulamaya adanmış bir IP ayırma özelliği, [var olan bir özel SSL sertifikasını Azure App Service bağlama][ConfigureSSL]bölümünde açıklandığı gıbı, IP tabanlı bir SSL yapılandırıldıktan sonra kullanılabilir. Bir ıLB Ao 'da, IP tabanlı SSL için kullanılacak ek IP adresleri ekleyemezsiniz.

Dış bir AO ile, uygulamanız için IP tabanlı SSL 'yi çok kiracılı App Service yaptığınız şekilde yapılandırabilirsiniz. En fazla 30 IP adresine sahip her zaman bir yedek adres vardır. Her birini kullandığınızda bir adres, her zaman kullanıma hazır olacak şekilde eklenir. Başka bir IP adresi ayırmak için zaman gecikmesi gerekir, bu da IP adreslerini hızlı bir şekilde art arda eklemeyi önler.

## <a name="front-end-scaling"></a>Ön uç ölçeklendirme ##

App Service planlarınızı ölçeklendirirseniz, çalışanlar onları destekleyecek şekilde otomatik olarak eklenir. Her Ao, iki ön uç ile oluşturulur. Ön uçlar, her toplam 15 App Service plan örneği için bir ön uç hızında otomatik olarak ölçeklendirilir. Beş örnek içeren üç App Service planınız varsa, toplam 15 örneğe ve üç ön uca sahip olursunuz. Toplam 30 örneğe ölçeklendirirseniz, dört ön Uçmış olursunuz ve bu şekilde devam edersiniz. 

Varsayılan olarak ayrılan ön uçların sayısı, orta düzeyde yük için uygundur. Her beş örnek için, oranı bir ön ucu olarak düşük olarak değiştirebilirsiniz. Ön uçların boyutunu da değiştirebilirsiniz. Varsayılan olarak tek çekirdeklerdir. Portalda ön uçların boyutunu iki veya dört temel boyuta dönüştürebilirsiniz. Oran veya ön uç boyutlarının değiştirilmesi için bir ücret uygulanır. Daha fazla bilgi için bkz. [Azure App Service fiyatlandırması][Pricing]. AP 'nizin yük kapasitesini artırmak istiyorsanız, ölçek oranını ayarlamadan önce iki çekirdekli ön uca ölçeklendirerek daha fazla geliştirme alacaksınız. Ön uçlarınızın çekirdek boyutunu değiştirmek AP 'nizin yükseltmesine neden olur ve normal iş saatleri dışında yapılmalıdır.

Ön uçlar kaynakları ASE 'nin HTTP/HTTPS uç noktasıdır. Varsayılan ön uç yapılandırmasıyla, ön uç başına bellek kullanımı yaklaşık olarak yüzde 60 ' dir. Ön uçlarınızın ölçeklendirilmesi için birincil neden, genellikle HTTPS trafiği tarafından yönetilen bir CPU olur.

## <a name="app-access"></a>Uygulama erişimi ##

Bir dış ate, uygulama oluşturma için kullanılan etki alanı soneki *.&lt;asename&gt;. p.azurewebsites.net*. Ao 'niz _External-Ao_ olarak adlandırılmışsa ve _contoso_ ADLı bir uygulamayı bu Ao 'da barındırdıysanız, bu URL 'ye aşağıdaki URL 'lerde ulaşabilirsiniz:

- contoso.external-ase.p.azurewebsites.net
- contoso.scm.external-ase.p.azurewebsites.net

Dış Ao oluşturma hakkında daha fazla bilgi için bkz. [App Service ortam oluşturma][MakeExternalASE]

ILB Ao 'da, uygulama oluşturma için kullanılan etki alanı soneki *.&lt;asename&gt;. appserviceenvironment.net*. Ao 'niz _ILB-Ao_ olarak adlandırılmışsa ve _contoso_ ADLı bir uygulamayı bu Ao 'da barındırdıysanız, bu URL 'ye Şu URL 'lerden ulaşabilirsiniz:

- contoso.ilb-ase.appserviceenvironment.net
- contoso.scm.ilb-ase.appserviceenvironment.net

ILB ATıCı oluşturma hakkında daha fazla bilgi için, bkz. [ILB Ai oluşturma ve kullanma][MakeILBASE]. 

SCM URL 'SI, kudu konsoluna erişmek veya uygulamanızı Web dağıtımı kullanarak yayımlamak için kullanılır. Kudu Konsolu hakkında daha fazla bilgi için bkz. [Azure App Service kudu konsolu][Kudu]. Kudu konsolu, hata ayıklama, dosyaları karşıya yükleme, dosyaları düzenlemeyle ve çok daha fazlası için bir Web Kullanıcı arabirimi sağlar.

## <a name="publishing"></a>Yayımlama ##

Çok kiracılı App Service gibi, bir AO 'da ile yayımlayabilirsiniz:

- Web dağıtımı.
- FTP.
- Sürekli tümleştirme.
- Kudu konsoluna sürükleyip bırakın.
- Visual Studio, tutulma veya IntelliJ fıkrı gibi bir IDE.

Bir dış Ao ile, bu yayımlama seçeneklerinin hepsi aynı şekilde davranır. Daha fazla bilgi için bkz. [Azure App Service dağıtım][AppDeploy]. 

Yayımlamayla ilgili önemli fark, bir ıLB Ao 'ya göre belirlenir. ILB Ao ile, yayımlama uç noktaları yalnızca ıLB ile kullanılabilir. ILB, sanal ağdaki Ao alt ağında özel bir IP üzerinde yer alır. ILB 'ye ağ erişiminiz yoksa, o Ao 'da herhangi bir uygulama yayımlayamazsınız. [BIR ıLB Ai oluşturma ve kullanma][MakeILBASE]bölümünde belirtildiği gibi, sistemdeki uygulamalar için DNS 'yi yapılandırmanız gerekir. Bu, SCM uç noktasını içerir. Bunlar düzgün tanımlanmamışsa yayımlanamıyor. Ayrıca, doğrudan kendisine yayımlamak için Ides 'in ıLB 'ye ağ erişimi olması gerekir.

Kullanıma hazır olmayan, GitHub ve Azure DevOps gibi Internet tabanlı CI sistemleri, yayımlama uç noktası Internet 'e erişilemediğinden bir ıLB Ao ile çalışmaz. ILB ASE 'yi içeren VNet 'e şirket içinde barındırılan bir yayın Aracısı yükleyerek Azure DevOps 'dan bir ıLB ASE 'ye yayımlamayı etkinleştirebilirsiniz. Alternatif olarak, Dropbox gibi bir çekme modeli kullanan bir CI sistemini de kullanabilirsiniz.

Bir ILB ASE’deki uygulamalar için yayımlama uç noktaları, ILB ASE oluşturulurken kullanılan etki alanını kullanır. Uygulamanın yayımlama profilinde ve uygulamanın Portal dikey penceresinde ( **genel bakış** > **Essentials** ve ayrıca **Özellikler**bölümünde) görebilirsiniz. 

## <a name="storage"></a>Depolama

ATıCı, Ao 'daki tüm uygulamalar için 1 TB depolama alanı içerir. Yalıtılmış bir SKU App Service planı varsayılan olarak 250 GB 'lik bir sınıra sahiptir. Beş veya daha fazla App Service planınız varsa, Ao 'nun 1 TB sınırını aşmamaya dikkat etmeniz gerekir. Bir App Service planında 250 GB sınırından daha fazlasına ihtiyacınız varsa, App Service planı limitini en fazla 1 TB olarak ayarlamak için desteğe başvurun. Plan limiti ayarlandığında, Ao 'daki tüm App Service planlarında hala 1 TB sınırı vardır. 

## <a name="logging"></a>Günlüğe kaydetme ##

Aşirinizi depolama, Olay Hub 'ı veya Log Analytics ile ilgili günlükleri göndermek için Azure Izleyici ile tümleştirebilirsiniz. Bugün günlüğe kaydedilen öğeler şunlardır:

| Olanını | İleti |
|---------|----------|
| Ao uygun değil | Belirtilen Ao, geçersiz bir sanal ağ yapılandırması nedeniyle uygun değil. Sağlıksız durum devam ederse Ao askıya alınır. Burada tanımlanan yönergelerin izlendiğinden emin olun: https://docs.microsoft.com/azure/app-service/environment/network-info |
| Ao alt ağında neredeyse yer kalmadı | Belirtilen Ao, neredeyse boş alan olmayan bir alt ağda. Kalan {0} adres var. Bu adresler tükendiğinde Ao, ölçeklendirmeyecektir  |
| ATıCı toplam örnek sınırına yaklaşıyor | Belirtilen Ao, Ao 'nun toplam örnek sınırına yaklaşıyor. Şu anda en fazla 201 örnek {0} App Service planı örnekleri içerir. |
| ATıCı bir bağımlılığa ulaşamıyor | Belirtilen ATıCı {0}ulaşamıyor.  Burada tanımlanan yönergelerin izlendiğinden emin olun: https://docs.microsoft.com/azure/app-service/environment/network-info |
| ATıCı askıya alındı | Belirtilen Ao, askıya alındı. Ao askıya alma, bir hesap kısaltıcı veya geçersiz bir sanal ağ yapılandırmasından kaynaklanıyor olabilir. Ana neden çözümleyin ve trafiğe hizmet vermeye devam etmek için Ao 'yu sürdürün |
| ALUPGRADE yükseltmesi başlatıldı | Belirtilen asa 'ya bir platform yükseltmesi başladı. Ölçeklendirme işlemlerinde gecikme süreleri bekliyor |
| ALUPGRADE yükseltmesi tamamlandı | Belirtilen asa 'ya platform yükseltmesi tamamlandı |
| Ölçeklendirme işlemleri başlatıldı | App Service planı ({0}) ölçeklendirmeye başladı. İstenen durum: {1} I{2} çalışanlar 
| Ölçeklendirme işlemleri tamamlandı | App Service planı ({0}) ölçeklendirmeyi bitirdi. Geçerli durum: {1} I{2} çalışanlar |
| Ölçeklendirme işlemleri başarısız oldu | App Service planı ({0}) ölçeklendirilemedi. Geçerli durum: {1} I{2} çalışanlar |

AŞIRDE günlüğe kaydetmeyi etkinleştirmek için: 

1. Portalda Tanılama Ayarları ' na gidin.  
1. Tanılama ayarı Ekle ' yi seçin.
1. Günlük tümleştirmesi için bir ad sağlayın
1. İstenen günlük hedeflerini denetleyin ve yapılandırın. 
1. AppServiceEnvironmentPlatformLogs 'u denetleyin

![ATıCı tanılama günlüğü ayarları][4]

Log Analytics ile tümleştirirseniz, asa portalından Günlükler seçerek ve AppServiceEnvironmentPlatformLogs 'a yönelik bir sorgu oluşturarak günlükleri görebilirsiniz. 

## <a name="upgrade-preference"></a>Yükseltme tercihi ##

Birden çok ASE varsa, diğerlerinden önce bazı ASE 'ler yükseltilmek isteyebilirsiniz. ATıCı HostingEnvironment Kaynak Yöneticisi nesnesi içinde, UpgradePreference için bir değer ayarlayabilirsiniz. UpgradePreference ayarı şablon, ARMClient veya https://resources.azure.comaracılığıyla yapılandırılabilir.  Üç değer seçeneği şunlardır:

* None-none varsayılandır ve Azure, Ao 'nizi belirli bir toplu iş olmadan yükseltecektir
* Erken erken, ATıCı 'nizin App Service yükseltmelerin ilk yarısında yükseltileceğini gösterir
* Geç geç, ATıCı 'nizin App Service yükseltmelerin ikinci yarısında yükseltileceğini gösterir

https://resources.azure.comkullanıyorsanız, upgradePreferences değerini şu şekilde ayarlayabilirsiniz:

1. Azure hesabınızla resources.azure.com ve oturum açma ile çalışmaya devam edin
1. Abonelik\/\[abonelik adı\]\/resourceGroups\/\[kaynak grubu adı\]\/sağlayıcıları\/Microsoft. Web\/hostingEnvironments\/\[Ao adı\]
1. Üstte okuma/yazma seçme
1. Düzenle 'yi seçin
1. UpgradePreference değerini, üç seçenekten istediğiniz şekilde değiştirin.
1. Düzeltme Eki Seç

![Kaynak Azure com görüntülemesi][5]

Yükseltme tercihleri özelliği, "erken" yükseltilmiş ASE 'lerin "geç" Atoklarından önce yükseltilene kadar çok zaman sahip olduğunuzda en mantıklı şekilde yararlanacaktır. Birden çok ASE 'niz varsa, geliştirme/test ASEs larınızın "erken" ve üretim Alarınızın "geç" olarak ayarlanması gerekir.

## <a name="pricing"></a>Fiyatlandırma ##

**Yalıtılmış** olarak adlandırılan fiyatlandırma SKU 'SU yalnızca Ao ile kullanım içindir. ASA 'da barındırılan tüm App Service planları yalıtılmış fiyatlandırma SKU 'sunda. Yalıtılmış App Service plan ücretleri bölge başına farklılık gösterebilir. 

App Service planlarınızın fiyatına ek olarak, ASE 'nin kendisi için düz bir ücret vardır. Sabit Fiyat, asa 'nın boyutuyla değişmez ve her 15 App Service plan örneği için bir ek ön uçtaki varsayılan ölçeklendirme hızında Ao altyapısı için ödeme yapar.  

Her 15 App Service plan örneği için bir ön uçların varsayılan ölçek oranı yeterince hızlı değilse, ön uçların eklenme oranını veya ön uçların boyutunu ayarlayabilirsiniz.  Oranı veya boyutu ayarlarken, varsayılan olarak eklenmemelidir ön uç çekirdekleri için ödeme yaparsınız.  

Örneğin, ölçek oranını 10 olarak ayarlarsanız App Service planlarınızda her 10 örnek için bir ön uç eklenir. Düz ücret, her 15 örnek için bir ön ucun ölçek oranını içerir. 10 ' un ölçek oranıyla 10 App Service plan örnekleri için eklenen üçüncü ön uç için ücret ödersiniz. Otomatik olarak eklendiğinden, 15 örneğe ulaştığınızda bu ücret için ödeme yapmanız gerekmez.

Ön uçların boyutunu iki çekirdeğe ayarlarsanız ancak oranı ayarlamadıysanız ek çekirdekler için ödeme yaparsınız.  İki ön uç ile bir ASE oluşturulur. bu nedenle, otomatik ölçeklendirme eşiğinin altında bile, boyutu iki çekirdekli ön uçlara yükselseniz iki ek çekirdek için ödeme yaparsınız.

Daha fazla bilgi için bkz. [Azure App Service fiyatlandırması][Pricing].

## <a name="delete-an-ase"></a>AS 'yi silme ##

ATıCı 'yi silmek için: 

1. **App Service ortamı** dikey penceresinin üstündeki **Sil** ' i kullanın. 

1. Silmek istediğinizi onaylamak için ATıCı 'nizin adını girin. Bir AI 'yi sildiğinizde, içindeki içeriğin tümünü de silersiniz. 

    ![ATıCı silme][3]

<!--Image references-->
[1]: ./media/using_an_app_service_environment/usingase-appcreate.png
[2]: ./media/using_an_app_service_environment/usingase-pricingtiers.png
[3]: ./media/using_an_app_service_environment/usingase-delete.png
[4]: ./media/using_an_app_service_environment/usingase-logsetup.png
[4]: ./media/using_an_app_service_environment/usingase-logs.png
[5]: ./media/using_an_app_service_environment/usingase-upgradepref.png


<!--Links-->
[Intro]: ./intro.md
[MakeExternalASE]: ./create-external-ase.md
[MakeASEfromTemplate]: ./create-from-template.md
[MakeILBASE]: ./create-ilb-ase.md
[ASENetwork]: ./network-info.md
[UsingASE]: ./using-an-ase.md
[UDRs]: ../../virtual-network/virtual-networks-udr-overview.md
[NSGs]: ../../virtual-network/security-overview.md
[ConfigureASEv1]: app-service-web-configure-an-app-service-environment.md
[ASEv1Intro]: app-service-app-service-environment-intro.md
[Functions]: ../../azure-functions/index.yml
[Pricing]: https://azure.microsoft.com/pricing/details/app-service/
[ARMOverview]: ../../azure-resource-manager/management/overview.md
[ConfigureSSL]: ../configure-ssl-certificate.md
[Kudu]: https://azure.microsoft.com/resources/videos/super-secret-kudu-debug-console-for-azure-web-sites/
[AppDeploy]: ../deploy-local-git.md
[ASEWAF]: app-service-app-service-environment-web-application-firewall.md
[AppGW]: ../../application-gateway/application-gateway-web-application-firewall-overview.md
