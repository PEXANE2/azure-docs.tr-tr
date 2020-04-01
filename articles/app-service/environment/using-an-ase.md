---
title: Uygulama Hizmeti Ortamını kullanma ve yönetme
description: Uygulama Hizmeti Ortamında uygulamaları nasıl oluşturup yayınlayacağınızı ve ölçeklendireceklerini öğrenin. Bu makaledeki tüm ortak görevleri bulun.
author: ccompy
ms.assetid: a22450c4-9b8b-41d4-9568-c4646f4cf66b
ms.topic: article
ms.date: 01/01/2020
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: 8a73c1998203a8696b67a5e7eb3af23898239265
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80477627"
---
# <a name="use-an-app-service-environment"></a>App Service Ortamını kullanma

Uygulama Hizmet Ortamı (ASE), Azure Uygulama Hizmeti'nin müşterinin Azure Sanal Ağ örneğinde bir alt ağa dağıtımıdır. Bir ASE oluşur:

- **Ön uçlar**: Http veya HTTPS'nin Bir Uygulama Hizmet Ortamında sona erdiği yer.
- **Çalışanlar**: Uygulamalarınızı barındıran kaynaklar.
- **Veritabanı**: Ortamı tanımlayan bilgileri tutar.
- **Depolama**: Müşteri tarafından yayınlanan uygulamaları barındırmak için kullanılır.

Uygulama erişimi için harici veya dahili sanal IP (VIP) içeren bir ASE dağıtabilirsiniz. Harici BIR VIP ile dağıtım genellikle *Harici ASE*denir. Dahili yük dengeleyici (ILB) kullandığından, dahili VIP'li bir dağıtıma *ILB ASE* denir. ILB ASE hakkında daha fazla bilgi edinmek için [bkz.][MakeILBASE]

## <a name="create-an-app-in-an-ase"></a>ASE'de uygulama oluşturma

BIR ASE'de bir uygulama oluşturmak için, normalde bir uygulama oluşturduğunuzda, ancak birkaç küçük farkla aynı işlemi kullanırsınız. Yeni bir Uygulama Hizmeti planı oluşturduğunuzda:

- Uygulamanızı dağıtabileceğiniz bir coğrafi konum seçmek yerine, konumunuz olarak bir ASE seçersiniz.
- BIR ASE'de oluşturulan tüm Uygulama Hizmeti planları yalnızca Yalıtılmış bir fiyatlandırma katmanında olabilir.

BIR ASE'niz yoksa, [Uygulama Hizmeti Ortamı Oluştur'daki][MakeExternalASE]yönergeleri izleyerek bir ase oluşturabilirsiniz.

ASE'de bir uygulama oluşturmak için:

1. **Kaynak** > Web +**Mobil** > Web**Uygulaması**Oluştur'u seçin.

1. Uygulama için bir ad girin. Bir ASE'de zaten bir Uygulama Hizmeti planı seçtiyseniz, uygulamanın alan adı ASE'nin etki alanı adını yansıtır:

    ![Uygulama adı seçimi][1]

1. Bir abonelik seçin.

1. Yeni bir kaynak grubu için bir ad girin veya **varolan kullan'ı** seçin ve açılan listeden birini seçin.

1. İşletim sisteminizi seçin.

1. ASE'nizde varolan bir Uygulama Hizmeti planı seçin veya aşağıdaki adımları izleyerek yeni bir plan oluşturun:

    a. Azure portalı sol taraftaki **menüsünden, Kaynak Oluştur > Web Uygulaması'nı**seçin.

    b. Aboneliği seçin.

    c. Kaynak grubunu seçin veya oluşturun.

    d. Web uygulamanızın adını girin.

    e. **Kod** veya **DockerContainer'ı**seçin.

    f. Çalışma zamanı yığınını seçin.

    g. **Linux** veya **Windows'u**seçin. 

    h. **Bölge** açılır listesinde ASE'nizi seçin. 

    i. Yeni bir Uygulama Hizmeti planı seçin veya oluşturun. Yeni bir Uygulama Hizmeti planı oluşturuyorsanız, uygun **İzole** SKU boyutunu seçin.

    ![Yalıtılmış fiyatlandırma katmanları][2]

    > [!NOTE]
    > Linux uygulamaları ve Windows uygulamaları aynı Uygulama Hizmeti planında olamaz, ancak aynı Uygulama Hizmeti Ortamında olabilir.
    >

1. **Gözden Geçir + oluştur'** seçeneğini seçin, bilgilerin doğru olduğundan emin olun ve sonra **Oluştur'u**seçin.

## <a name="how-scale-works"></a>Ölçek nasıl çalışır?

Her App Service uygulaması bir Uygulama Hizmeti planında çalışır. Uygulama Hizmet Ortamları Uygulama Hizmeti planlarını ve Uygulama Hizmeti planlarını tutar. Bir uygulamayı ölçeklendirdiğinizde, Uygulama Hizmeti planını ve aynı plandaki tüm uygulamaları da ölçeklendirin.

Bir Uygulama Hizmeti planını ölçeklendirdiğinizde, gerekli altyapı otomatik olarak eklenir. Altyapı eklenirken operasyonları ölçeklendirmek için bir zaman gecikmesi var. Sırayla birkaç ölçek işlemi yaparsanız, ilk altyapı ölçeği isteği üzerinde hareket edilir ve diğerleri sıraya alınır. İlk ölçek çalışması bittiğinde, diğer altyapı isteklerinin tümü birlikte çalışır. Altyapı eklendiğinde, Uygulama Hizmeti planları uygun şekilde atanır. Ek donanım istediği için yeni bir Uygulama Hizmeti planı oluşturmak bir ölçek işlemidir.

Çok kiracılı Uygulama Hizmeti'nde, bir kaynak havuzu bunu desteklemeye hazır olduğundan ölçekleme hemen gerçekleşir. Bir ASE'de böyle bir arabellek yoktur ve kaynaklar ihtiyaca göre ayrılır.

Bir ASE'de, bir Uygulama Hizmeti planını 100 örneğe kadar ölçeklendirebilirsiniz. Bir ASE, o ASE'deki tüm Uygulama Hizmeti planlarında 201'e kadar toplam örne sahip olabilir.

## <a name="ip-addresses"></a>IP adresleri

Uygulama Hizmeti bir uygulamaya özel bir IP adresi tahsis edebilir. Bu özellik, Bind'de açıklandığı gibi, IP tabanlı SSL'yi Azure [Uygulama Hizmeti'ne bind'de][ConfigureSSL]yapılandırdıktan sonra kullanılabilir. ILB ASE'de, IP tabanlı SSL için kullanılacak daha fazla IP adresi ekemezsiniz.

Harici BIR ASE ile, uygulamanız için IP tabanlı SSL'yi çok kiracılı Uygulama Hizmeti'nde olduğu gibi yapılandırabilirsiniz. ASE'de her zaman bir yedek adres, en fazla 30 IP adresi vardır. Birini her kullandığınızda, bir adresin her zaman hazır olması için başka bir adres eklenir. Başka bir IP adresi ayırmak için bir zaman gecikmesi gerekir. Bu gecikme, IP adreslerinin hızlı bir şekilde art arda eklenmesini önler.

## <a name="front-end-scaling"></a>Ön uç ölçekleme

Uygulama Hizmeti planlarınızı ölçeklendirdiğinizde, çalışanlar bunları desteklemek için otomatik olarak eklenir. Her ASE iki ön uçile oluşturulur. Ön uçlar, her 15 Uygulama Hizmeti planı örneği kümesi için otomatik olarak bir ön uç oranında ölçeklendirilir. Örneğin, her biri beş örneği olan üç Uygulama Hizmeti planınız varsa, toplam 15 örnek ve üç ön uç tan tantananız olur. Toplam 30 örnekle ölçeklendirseniz, dört ön ucunuz olur. Bu desen, siz ölçeklendikçe devam eder.

Varsayılan olarak ayrılan ön uç sayısı orta düzeyde bir yük için iyidir. Her beş örnek için oranı bir ön uca kadar düşürebilirsiniz. Ayrıca ön uçların boyutunu değiştirebilirsiniz. Varsayılan olarak, tek çekirdekli. Azure portalında, bunların boyutunu iki veya dört çekirdek olarak değiştirebilirsiniz.

Oranı veya ön uç boyutlarını değiştirmek için bir ücret var. Daha fazla bilgi için Azure [Uygulama Hizmeti fiyatlandırması'na][Pricing]bakın. ASE'nizin yük kapasitesini artırmak istiyorsanız, ölçek oranını ayarlamadan önce ilk ölçeklendirmeyi iki çekirdekli ön uçlara ölçeklendirerek daha fazla iyileştirme elde elabilirsiniz. Ön uçlarınızın çekirdek boyutunu değiştirmek ASE'nizin yükseltilmesine neden olur ve normal çalışma saatleri dışında yapılmalıdır.

Ön uç kaynakları ASE için HTTP/HTTPS bitiş noktasıdır. Varsayılan ön uç yapılandırması ile, ön uç başına bellek kullanımı sürekli olarak yüzde 60 civarındadır. Ön uçlarınızı ölçeklendirmenin birincil nedeni, öncelikle HTTPS trafiği tarafından yönlendirilen CPU kullanımıdır.

## <a name="app-access"></a>Uygulama erişimi

Harici BIR ASE'de, uygulama oluşturma için kullanılan etki alanı soneki *.&lt; asename&gt;.p.azurewebsites.net*. ASE'niz _in dış-ase_ olarak adlandırılır ve o ASE'de _contoso_ adlı bir uygulama barındırıyorsanız, bu URL'lerden ulaşırsınız:

- contoso.external-ase.p.azurewebsites.net
- contoso.scm.external-ase.p.azurewebsites.net

Harici ASE nasıl oluşturulacak hakkında bilgi [için][MakeExternalASE]bkz.

ILB ASE'de, uygulama oluşturma için kullanılan etki alanı soneki *.&lt; asename&gt;.appserviceenvironment.net*. ASE'niz _ilb-ase_ olarak adlandırılırve o ASE'de _contoso_ adı verilen bir uygulama barındırıyorsanız, bu URL'lerden ulaşırsınız:

- contoso.ilb-ase.appserviceenvironment.net
- contoso.scm.ilb-ase.appserviceenvironment.net

ILB ASE nasıl oluşturulacak hakkında bilgi [için][MakeILBASE]bkz.

SCM URL, Kudu konsoluna erişmek veya Web Dağıtımı'nı kullanarak uygulamanızı yayınlamak için kullanılır. Kudu konsolu hakkında daha fazla bilgi için [Azure Uygulama Hizmeti için Kudu konsoluna][Kudu]bakın. Kudu konsolu hata ayıklama, dosya yükleme, dosyaları düzenleme ve çok daha fazlası için bir web arabirimi verir.

## <a name="publishing"></a>Yayımlama

Bir ASE'de, çok kiracılı Uygulama Hizmetinde olduğu gibi, şu yöntemlerle yayınlayabilirsiniz:

- Web dağıtımı
- FTP
- Sürekli entegrasyon (CI)
- Kudu konsolunda sürükleyin ve bırakın
- Visual Studio, Eclipse veya IntelliJ IDEA gibi bir IDE

Harici ASE ile bu yayımlama seçeneklerinin tümü aynı şekilde çalışır. Daha fazla bilgi için Azure [Uygulama Hizmetinde Dağıtım'a][AppDeploy]bakın.

Yayımlama, yayımlama uç noktalarının yalnızca ILB aracılığıyla kullanılabildiği bir ILB ASE ile önemli ölçüde farklıdır. ILB, sanal ağdaki ASE alt ağında ki özel bir IP üzerindedir. ILB'ye ağ erişiminiz yoksa, bu ASE'de herhangi bir uygulama yayınlayamazsınız. [ILB ASE oluştur ve kullan,][MakeILBASE]sistemdeki uygulamalar için DNS yapılandırmanız gerekir. Bu gereksinim SCM bitiş noktasını içerir. Uç noktalar düzgün tanımlanmamışsa, yayımlayamazsınız. IDA'larınızın doğrudan yayınlamak için ILB'ye ağ erişimine de sahip olması gerekir.

Ek değişiklikler olmadan, GitHub ve Azure DevOps gibi Internet tabanlı CI sistemleri, yayın bitiş noktası Internet'e erişilemediği için ILB ASE ile çalışmaz. ILB ASE içeren sanal ağa kendi kendine barındırılan bir sürüm aracısı yükleyerek Azure DevOps'ten bir ILB ASE'ye yayımlamayı etkinleştirebilirsiniz. Alternatif olarak, Dropbox gibi çekme modeli kullanan bir CI sistemi de kullanabilirsiniz.

Bir ILB ASE’deki uygulamalar için yayımlama uç noktaları, ILB ASE oluşturulurken kullanılan etki alanını kullanır. Bunu uygulamanın yayın profilinde ve uygulamanın portal bölmesinde **(Genel Bakış** > **Essentials** ve ayrıca **Özellikler'de)** görebilirsiniz.

## <a name="storage"></a>Depolama

Bir ASE, ASE'deki tüm uygulamalar için 1 TB depolama alanına sahiptir. Yalıtılmış fiyatlandırma SKU bir Uygulama Hizmeti planı varsayılan olarak 250 GB sınırı vardır. Beş veya daha fazla Uygulama Hizmeti planınız varsa, ASE'nin 1-TB sınırını aşmamaya dikkat edin. Bir Uygulama Hizmeti planında 250 GB sınırından fazlasına ihtiyacınız varsa, Uygulama Hizmeti planı sınırını en fazla 1 TB olarak ayarlamak için desteğe başvurun. Plan sınırı ayarlandığında, ASE'deki tüm Uygulama Hizmeti planlarında hala 1 TB sınırı vardır.

## <a name="logging"></a>Günlüğe Kaydetme

ASE ile ilgili günlükleri Azure Depolama, Azure Etkinlik Hub'ları veya Günlük Analitiği'ne göndermek için ASE'nizi Azure Monitörü ile tümleştirebilirsiniz. Bu öğeler bugün günlüğe kaydedilir:

| Durum | İleti |
|---------|----------|
| ASE sağlıksız | Belirtilen ASE geçersiz bir sanal ağ yapılandırması nedeniyle sağlıksız. Sağlıksız durum devam ederse ASE askıya alınacak. Burada tanımlanan yönergelerin izninizlediğinden emin olun: https://docs.microsoft.com/azure/app-service/environment/network-info. |
| ASE alt ağı neredeyse uzay dışında | Belirtilen ASE neredeyse uzay dışında bir alt net bulunmaktadır. Kalan {0} adresler var. Bu adresler tükendiğinde, ASE ölçeklendiremez.  |
| ASE toplam örnek sınırına yaklaşıyor | Belirtilen ASE, ASE'nin toplam örnek sınırına yaklaşıyor. Şu anda en fazla 201 örnekapp Servis Planı örnekleri içerir. {0} |
| ASE bir bağımlıya ulaşamıyor | Belirtilen ASE'ye ulaşamıyor. {0}  Burada tanımlanan yönergelerin izninizlediğinden emin olun: https://docs.microsoft.com/azure/app-service/environment/network-info. |
| ASE askıya alındı | Belirtilen ASE askıya alındı. ASE süspansiyonu bir hesap açığından veya geçersiz bir sanal ağ yapılandırmasına bağlı olabilir. Temel nedeni çözümleyin ve hizmet vermeye devam etmek için ASE'yi devam ettirin. |
| ASE yükseltmesi başladı | Belirtilen ASE için bir platform yükseltmesi başladı. Ölçekleme işlemlerinde gecikmeler bekleyin. |
| ASE yükseltmesi tamamlandı | Belirtilen ASE'ye bir platform yükseltmesi tamamlandı. |
| Ölçek işlemleri başladı | Bir Uygulama Hizmeti{0}planı ( ) ölçekleme başladı. İstenilen durum: {1} Ben{2} işçiler.
| Ölçek işlemleri tamamlandı | Bir Uygulama Hizmeti{0}planı ( ) ölçekleme tamamlandı. Şu anki {1} {2} durum: Ben işçiler. |
| Ölçek işlemleri başarısız oldu | Bir Uygulama Hizmeti{0}planı ( ) ölçeklendirmeyi başaramadı. Şu anki {1} {2} durum: Ben işçiler. |

ASE'nizde oturum açmayı etkinleştirmek için:

1. Portalda, **Tanılama ayarlarına**gidin.
1. **Tanıayar ayarı ekle'yi**seçin.
1. Günlük tümleştirmesi için bir ad sağlayın.
1. İstediğiniz günlük hedeflerini seçin ve yapılandırın.
1. **AppServiceEnvironmentPlatformLogs'u**seçin.

![ASE tanılama günlüğü ayarları][4]

Log Analytics ile tümleşirseniz, ASE portalından **Günlükler** seçerek ve **AppServiceEnvironmentPlatformLogs'a**karşı bir sorgu oluşturarak günlükleri görebilirsiniz.

## <a name="upgrade-preference"></a>Yükseltme tercihi

Birden çok ASE'niz varsa, bazı ASE'lerin diğerlerinden önce yükseltilmesini isteyebilirsiniz. ASE **HostingEnvironment Resource Manager** nesnesi içinde **yükseltme Tercihi**için bir değer ayarlayabilirsiniz. **yükseltmeTercih** ayarı bir şablon, ARMClient veya https://resources.azure.com. Üç olası değer şunlardır:

- **Yok**: Azure, ASE'nizi belirli bir toplu iş halinde yükseltmez. Bu varsayılan değerdir.
- **Erken**: ASE'niz, Uygulama Hizmeti yükseltmelerinin ilk yarısında yükseltilir.
- **Geç**: ASE'niz, Uygulama Hizmeti yükseltmelerinin ikinci yarısında yükseltilir.

Kullanıyorsanız, https://resources.azure.com **yükseltmeTercihleri** değerini ayarlamak için aşağıdaki adımları izleyin:

1. resources.azure.com gidin ve Azure hesabınızla oturum açın.
1. \/\[Abonelikler abonelik\]\/adı kaynak\/\[Gruplar kaynak grup\]\/\/adı sağlayıcıları\/Microsoft.Web hostingOrtamlar\/\[ASE adı\]için kaynaklar üzerinden gidin.
1. En üstte **Oku/Yaz'ı** seçin.
1. **Edit'i**seçin.
1. **YükseltmeTercihini** istediğiniz üç değerden hangisine ayarlayın.
1. **Yama'yı**seçin.

![kaynaklar azure com ekran][5]

**UpgradePreferences** özelliği, birden fazla "Erken" KıÇ'ınız "Geç" KıÇ'ınızdan önce yükseltilecektir, çünkü birden fazla KıÇ'ınız olduğunda en anlamlı özelliktir. Birden fazla ASE'niz olduğunda, geliştirme nizi ve test ETTIĞINIZ imar hanenizi "Erken" ve üretim ASE'lerinizi "Geç" olarak ayarlamalısınız.

## <a name="pricing"></a>Fiyatlandırma

SKU'nun *İzole* olarak adlandırdığı fiyatlandırma yalnızca AS'lerde kullanım içindir. ASE'de barındırılan tüm Uygulama Hizmeti planları İzole fiyatlandırma SKU'dadır. Uygulama Hizmeti planları için yalıtılmış fiyatlar bölgeye göre değişebilir.

Uygulama Hizmeti planlarınızın fiyatına ek olarak, ASE'nin kendisi için sabit bir fiyat vardır. Sabit oran ASE boyutu ile değişmez. Ase altyapısı için her 15 App Service planı örneği için bir ek ön uç varsayılan ölçek hızında ödeme yapar.

Her 15 Uygulama Hizmeti planı örneği için bir ön uç varsayılan ölçek oranı yeterince hızlı değilse, ön uçların eklenme oranını veya ön uçların boyutunu ayarlayabilirsiniz. Oran veya boyutu ayarladığınızda, varsayılan olarak eklenmez ön uç çekirdekleri için ödeme.

Örneğin, ölçek oranını 10 olarak ayarlarsanız, Uygulama Hizmeti planlarınızdaki her 10 örnek için bir ön uç eklenir. Sabit ücret, her 15 örnek için bir ön uç ölçek oranını kapsar. 10 ölçek oranıyla, 10 Uygulama Hizmeti planı örnekleri için eklenen üçüncü ön uç için bir ücret ödenebilirsiniz. Otomatik olarak eklendiğinden, 15 örneğe ulaştığınızda bunun için ödeme yapmanız gerekmez.

Ön uçların boyutunu iki çekirdekle ayarlarsanız ancak oranı ayarlamazsanız, ekstra çekirdekler için ödeme yapamazsınız. Bir ASE iki ön uçları ile oluşturulur, bu yüzden otomatik ölçekleme eşiğinin altında bile iki çekirdek için iki ekstra çekirdek için ödeme yapacak eğer iki çekirdekli ön uçlara boyutu arttı.

Daha fazla bilgi için Azure [Uygulama Hizmeti fiyatlandırması'na][Pricing]bakın.

## <a name="delete-an-ase"></a>BIR ASE silme

BIR ASE'yi silmek için:

1. **App Service Environment** bölmesinin üst kısmında **Sil'i** seçin.

1. Silmek istediğinizi doğrulamak için ASE'nizin adını girin. Bir ASE'yi sildiğinizde, içindeki tüm içeriği de silersiniz.

    ![ASE silme][3]

1. **Tamam'ı**seçin.

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
