---
title: İşletim sistemi işlevi
description: Windows'daki Azure Uygulama Hizmeti'nde işletim sistemi işlevselliği hakkında bilgi edinin. Uygulamanızın hangi dosya, ağ ve kayıt defteri erişimine eriştüğüni öğrenin.
ms.assetid: 39d5514f-0139-453a-b52e-4a1c06d8d914
ms.topic: article
ms.date: 10/30/2018
ms.custom: seodec18
ms.openlocfilehash: ed84cb2b0cb8d98b12fe787e49c400ba47e4e38a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74671618"
---
# <a name="operating-system-functionality-on-azure-app-service"></a>Azure Uygulama Hizmeti'nde işletim sistemi işlevselliği
Bu makalede, [Azure Uygulama Hizmeti'nde](https://go.microsoft.com/fwlink/?LinkId=529714)çalışan tüm Windows uygulamaları için kullanılabilen ortak temel işletim sistemi işlevselliği açıklanmaktadır. Bu işlevsellik dosya, ağ ve kayıt defteri erişimi ve tanılama günlükleri ve olayları içerir. 

> [!NOTE] 
> App Service'deki [Linux uygulamaları](containers/app-service-linux-intro.md) kendi kaplarında çalışır. Ana bilgisayar işletim sistemine erişime izin verilmez, kapsayıcıya kök erişiminiz var. Aynı şekilde, [Windows kapsayıcılarında çalışan uygulamalar](app-service-web-get-started-windows-container.md)için kapsayıcıya yönetimerişiminiz vardır, ancak ana bilgisayar işletim sistemine erişiminiz yoktur. 
>

<a id="tiers"></a>

## <a name="app-service-plan-tiers"></a>Uygulama Hizmeti planı katmanları
App Service, müşteri uygulamalarını çok kiracılı bir barındırma ortamında çalıştırZ. **Ücretsiz** ve **Paylaşılan** katmanlarda dağıtılan uygulamalar paylaşılan sanal makinelerde çalışan işlemlerde çalışırken, **Standart** ve **Premium** katmanlarda dağıtılan uygulamalar tek bir müşteriyle ilişkili uygulamalara özel sanal makinelerde çalışır.

[!INCLUDE [app-service-dev-test-note](../../includes/app-service-dev-test-note.md)]

Uygulama Hizmeti farklı katmanlar arasında sorunsuz ölçekleme deneyimini desteklediğinden, App Service uygulamaları için uygulanan güvenlik yapılandırması aynı kalır. Bu, Uygulama Hizmeti planı bir katmandan diğerine geçtiğinde uygulamaların aniden farklı davranmamasını ve beklenmeyen şekillerde başarısız olmasını sağlar.

<a id="developmentframeworks"></a>

## <a name="development-frameworks"></a>Geliştirme çerçeveleri
Uygulama Hizmeti fiyatlandırma katmanları, uygulamaların kullanabileceği bilgi işlem kaynaklarının (CPU, disk depolama, bellek ve ağ çıkış) miktarını denetler. Ancak, ölçekleme katmanlarından bağımsız olarak, uygulamaların kullanabileceği çerçeve işlevselliğinin genişliği aynı kalır.

App Service, ASP.NET, klasik ASP, node.js, PHP ve Python dahil olmak üzere çeşitli geliştirme çerçevelerini destekler ve bunların hepsi IIS içinde uzantı olarak çalışır. Güvenlik yapılandırmasını basitleştirmek ve normalleştirmek için, App Service uygulamaları genellikle varsayılan ayarlarıyla çeşitli geliştirme çerçevelerini çalıştırAr. Uygulamaları yapılandırmaya yönelik bir yaklaşım, her bir geliştirme çerçevesi için API yüzey alanını ve işlevselliğini özelleştirmek olabilir. Uygulama Hizmeti bunun yerine, bir uygulamanın geliştirme çerçevesine bakılmaksızın işletim sistemi işlevselliğinin ortak bir temelini etkinleştirerek daha genel bir yaklaşım benimser.

Aşağıdaki bölümler, App Service uygulamalarının kullanabileceği genel işletim sistemi işlevselliği türlerini özetleyin.

<a id="FileAccess"></a>

## <a name="file-access"></a>Dosya erişimi
Uygulama Hizmeti'nde yerel sürücüler ve ağ sürücüleri de dahil olmak üzere çeşitli sürücüler bulunur.

<a id="LocalDrives"></a>

### <a name="local-drives"></a>Yerel sürücüler
App Service, özünde Azure PaaS (hizmet olarak platform) altyapısının üzerinde çalışan bir hizmettir. Sonuç olarak, sanal bir makineye "bağlı" olan yerel sürücüler, Azure'da çalışan herhangi bir çalışan rolün kullanabileceği sürücü türleriile aynıdır. Buna aşağıdakiler dahildir:

- Bir işletim sistemi sürücüsü (D:\ sürücü)
- Yalnızca Uygulama Hizmeti tarafından kullanılan Azure Paketi cspkg dosyalarını içeren (ve müşteriler tarafından erişilemeyen) bir uygulama sürücüsü
- Bir "kullanıcı" sürücüsü (C:\ sürücü), boyutu VM boyutuna bağlı olarak değişir. 

Uygulamanız büyüdükçe disk kullanımınızı izlemek önemlidir. Disk kotasına ulaşılırsa, uygulamanız üzerinde olumsuz etkileri olabilir. Örnek: 

- Uygulama, diskte yeterli alan olmadığını belirten bir hata yapabilir.
- Kudu konsoluna göz atarken disk hataları görebilirsiniz.
- Azure DevOps veya Visual Studio'dan dağıtım `ERROR_NOT_ENOUGH_DISK_SPACE: Web deployment task failed. (Web Deploy detected insufficient space on disk)`başarısız olabilir.
- Uygulamanız yavaş performansa neden olabilir.

<a id="NetworkDrives"></a>

### <a name="network-drives-aka-unc-shares"></a>Ağ sürücüleri (aka UNC hisseleri)
Uygulama Hizmeti'nin uygulama dağıtımını ve bakımını kolaylaştıran benzersiz yönlerinden biri, tüm kullanıcı içeriğinin bir dizi UNC paylaşımında depolanmasıdır. Bu model, birden çok yük dengeli sunucuya sahip şirket içi web barındırma ortamları tarafından kullanılan ortak içerik depolama desenine iyi eşler. 

App Service içinde, her veri merkezinde oluşturulan bir dizi UNC paylaşımı vardır. Her veri merkezindeki tüm müşteriler için kullanıcı içeriğinin yüzdesi her UNC paylaşımına ayrılır. Ayrıca, tek bir müşterinin aboneliğine ait tüm dosya içeriği her zaman aynı UNC payına yerleştirilir. 

Azure hizmetlerinin çalışma şekli nedeniyle, UNC paylaşımını barındırmak üzere sorumlu belirli sanal makine zaman içinde değişecektir. UNC hisselerinin, Azure işlemlerinin normal seyri sırasında yukarı ve aşağı getirildiği nde farklı sanal makineler tarafından monte edilmesi garanti edilir. Bu nedenle, uygulamalar hiçbir zaman UNC dosya yolundaki makine bilgilerinin zaman içinde sabit kalacağına dair sabit kodlanmış varsayımlarda bulunmamalıdır. Bunun yerine, Uygulama Hizmeti'nin sağladığı uygun *sahte* mutlak yolu Kullanmaları gerekir **D:\home\site.** Bu sahte mutlak yol, kişinin kendi uygulamasına atıfta bulunmanın taşınabilir, uygulama ve kullanıcı-agnostik bir yöntemini sağlar. **D:\home\site**kullanarak, her aktarım için yeni bir mutlak yol yapılandırmak zorunda kalmadan paylaşılan dosyaları uygulamadan uygulamaya aktarabilirsiniz.

<a id="TypesOfFileAccess"></a>

### <a name="types-of-file-access-granted-to-an-app"></a>Bir uygulamaya verilen dosya erişim türleri
Her müşterinin aboneliği, bir veri merkezi içinde belirli bir UNC paylaşımı üzerinde ayrılmış bir dizin yapısına sahiptir. Bir müşterinin belirli bir veri merkezi içinde oluşturulan birden çok uygulaması olabilir, bu nedenle tek bir müşteri aboneliğine ait tüm dizinler aynı UNC paylaşımında oluşturulur. Paylaşım, içerik, hata ve tanılama günlükleri gibi dizinleri ve kaynak denetimi tarafından oluşturulan uygulamanın önceki sürümlerini içerebilir. Beklendiği gibi, bir müşterinin uygulama dizinleri uygulamanın uygulama kodu tarafından çalışma zamanında okuma ve yazma erişimi için kullanılabilir.

Bir uygulamayı çalıştıran sanal makineye bağlı yerel sürücülerde, Uygulama Hizmeti C:\ uygulamaya özel geçici yerel depolama için sürücü. Bir uygulama kendi geçici yerel depolama alanına tam okuma/yazma erişimine sahip olsa da, bu depolama alanı gerçekten doğrudan uygulama kodu tarafından kullanılmak üzere tasarlanmamıştır. Bunun yerine, amaç IIS ve web uygulama çerçeveleri için geçici dosya depolama sağlamaktır. Uygulama Hizmeti ayrıca, tek tek uygulamaların aşırı miktarda yerel dosya depolama alanı tüketmesini önlemek için her uygulamaiçin kullanılabilir geçici yerel depolama miktarını da sınırlar.

Uygulama Hizmeti'nin geçici yerel depolamayı nasıl kullandığına iki örnek, geçici ASP.NET dosyalarının dizini ve IIS sıkıştırılmış dosyaların dizinidir. ASP.NET derleme sistemi geçici derleme önbellek konumu olarak "Geçici ASP.NET Dosyaları" dizini kullanır. IIS, sıkıştırılmış yanıt çıktısını depolamak için "IIS Geçici Sıkıştırılmış Dosyalar" dizini kullanır. Bu tür dosya kullanımının her ikisi de (ve diğerleri) Uygulama Hizmeti'nde uygulama başına geçici yerel depolama alanına yeniden eşlenir. Bu yeniden eşleme, işlevselliğin beklendiği gibi devam edilmesini sağlar.

App Service'deki her uygulama, burada açıklanan "uygulama havuzu kimliği" adı verilen rasgele [https://www.iis.net/learn/manage/configuring-security/application-pool-identities](https://www.iis.net/learn/manage/configuring-security/application-pool-identities)benzersiz, düşük ayrıcalıklı bir işçi süreci kimliği olarak çalışır: . Uygulama kodu, işletim sistemi sürücüsüne temel salt okunur erişim için bu kimliği kullanır (D:\ sürücü). Bu, uygulama kodunun ortak dizin yapılarını listeleyip işletim sistemi sürücüsündeki ortak dosyaları okuyabileceği anlamına gelir. Bu biraz geniş bir erişim düzeyi gibi görünse de, Azure barındırılan bir hizmette bir çalışan rolü sağlamadığınızda ve sürücü içeriğini okuduğunuzda aynı dizinlere ve dosyalara erişilebilir. 

<a name="multipleinstances"></a>

### <a name="file-access-across-multiple-instances"></a>Birden çok örnekte dosya erişimi
Ev dizini bir uygulamanın içeriğini içerir ve uygulama kodu bu içeriğe yazabilir. Bir uygulama birden çok örnekte çalışırsa, tüm örneklerin aynı dizini görmesi için ev dizini tüm örnekler arasında paylaşılır. Bu nedenle, örneğin, bir uygulama yüklenen dosyaları ev dizinine kaydederse, bu dosyalar tüm örnekler için hemen kullanılabilir. 

<a id="NetworkAccess"></a>

## <a name="network-access"></a>Ağ erişimi
Uygulama kodu, harici hizmetleri ortaya çıkaran Internet'e giden ağ bağlantılarını kullanıma açmak için TCP/IP ve UDP tabanlı protokolleri kullanabilir. Uygulamalar, örneğin SQL Veritabanı'na HTTPS bağlantıları kurarak Azure içindeki hizmetlere bağlanmak için aynı protokolleri kullanabilir.

Uygulamaların tek bir yerel geri dönüş bağlantısı kurması ve bir uygulamanın bu yerel geri dönüş soketi üzerinde dinlemesi için sınırlı bir yeteneği de vardır. Bu özellik, işlevselliklerinin bir parçası olarak yerel döngü yuvalarında dinleyen uygulamaların etkinleştirilmesini sağlamak için öncelikle vardır. Her uygulama bir "özel" döngü bağlantısı görür. "A" uygulaması "B" uygulaması tarafından kurulan yerel bir geri dönüş soketi dinleyemez.

Adlandırılmış borular, bir uygulamayı toplu olarak çalıştıran farklı işlemler arasında süreçler arası iletişim (IPC) mekanizması olarak da desteklenir. Örneğin, IIS FastCGI modülü PHP sayfalarını çalıştıran tek tek işlemleri koordine etmek için adlandırılmış borulara dayanır.

<a id="Code"></a>

## <a name="code-execution-processes-and-memory"></a>Kod yürütme, işlemler ve bellek
Daha önce de belirtildiği gibi, uygulamalar rasgele bir uygulama havuzu kimliği kullanarak düşük ayrıcalıklı işçi işlemleri içinde çalışır. Uygulama kodu, alt işlemle ilişkili bellek alanına ve CGI işlemleri veya diğer uygulamalar tarafından ortaya çıkabilecek alt işlemlere erişebilir. Ancak, bir uygulama aynı sanal makinede olsa bile başka bir uygulamanın bellene veya verilerine erişemez.

Uygulamalar, desteklenen web geliştirme çerçeveleriyle yazılmış komut dosyalarını veya sayfaları çalıştırabilir. Uygulama Hizmeti, web çerçevesi ayarlarını daha kısıtlı modlara yapılandırmaz. Örneğin, App Service'te çalışan ASP.NET uygulamalar, daha kısıtlı bir güven modunun aksine "tam" güvenle çalışır. Hem klasik ASP hem de ASP.NET dahil olmak üzere web çerçeveleri, Windows işletim sisteminde varsayılan olarak kayıtlı olan ADO (ActiveX Veri Nesneleri) gibi işlem içi COM bileşenlerini (işlem COM bileşenlerinin dışında değil) arayabilir.

Uygulamalar rasgele kod lar yumurtlayabilir ve çalıştırabilir. Bir uygulamanın komut kabuğu nu yontmak veya PowerShell komut dosyası çalıştırmak gibi şeyler yapmasına izin verilir. Ancak, bir uygulamadan rasgele kod ve işlemler çıkarılabilir olsa da, yürütülebilir programlar ve komut dosyaları yine de üst uygulama havuzuna verilen ayrıcalıklarla sınırlıdır. Örneğin, bir uygulama giden http araması yapan bir yürütülebilir yumurtlayabilir, ancak aynı yürütülebilir, sanal bir makinenin IP adresini NIC'sinden çıkarmaya çalışamaz. Giden ağ çağrısı yapmak düşük ayrıcalıklı kodiçin izin verilir, ancak sanal bir makinede ağ ayarlarını yeniden yapılandırmaya çalışmak yönetim ayrıcalıkları gerektirir.

<a id="Diagnostics"></a>

## <a name="diagnostics-logs-and-events"></a>Tanılama günlükleri ve olayları
Günlük bilgileri, bazı uygulamaların erişmeye çalıştığı başka bir veri kümesidir. App Service'de çalışan kodiçin kullanılabilen günlük bilgileri türleri, uygulama tarafından kolayca erişilebilen bir uygulama tarafından oluşturulan tanılama ve günlük bilgilerini içerir. 

Örneğin, etkin bir uygulama tarafından oluşturulan W3C HTTP günlükleri, uygulama için oluşturulan ağ paylaşım konumundaki bir günlük dizininde veya bir müşteri W3C'nin depolama alanına günlüğe kaydedilmesini ayarlamışsa blob depolama alanında kullanılabilir. İkinci seçenek, ağ paylaşımıyla ilişkili dosya depolama sınırlarını aşma riski olmadan büyük miktarda günlük lerin toplanmasını sağlar.

Benzer bir şekilde ,NET uygulamalarından gelen gerçek zamanlı tanılama bilgileri ,.NET izleme ve tanılama altyapısı kullanılarak, izleme bilgilerini uygulamanın ağ paylaşımına veya alternatif olarak bir blob depolama konumuna yazma seçenekleriyle de günlüğe kaydedilebilir.

Uygulamalar için kullanılamayan tanılama günlüğe kaydetme ve izleme alanları Windows ETW olayları ve yaygın Windows olay günlükleridir (örneğin, Sistem, Uygulama ve Güvenlik olay günlükleri). ETW izleme bilgileri makine çapında görüntülenebilir (doğru Aç'larla) olabileceğinden, ETW olaylarına okuma ve yazma erişimi engellenir. Geliştiriciler, ETW olaylarını okumak ve yazmak için API çağrılarının ve yaygın Windows olay günlüklerinin işe yaradığını fark edebilir, ancak bunun nedeni Uygulama Hizmeti'nin çağrıları "taklit etmesi" ve böylece başarılı olduklarının görünmesidir. Gerçekte, uygulama kodunun bu olay verilerine erişimi yoktur.

<a id="RegistryAccess"></a>

## <a name="registry-access"></a>Kayıt defteri erişimi
Uygulamalar, üzerinde çalıştıkları sanal makinenin kayıt defterinin çok kısmına (hepsi olmasa da) salt okunur erişime sahiptir. Uygulamada bu, yerel Kullanıcılar grubuna salt okunur erişime izin veren kayıt defteri anahtarlarına uygulamalar tarafından erişilebildiği anlamına gelir. Kayıt defterinin şu anda okuma veya yazma erişimi için desteklenmeyen bir alanı HKEY\_CURRENT\_USER kovanıdır.

Kayıt defterine yazma erişimi, kullanıcı başına kayıt defteri anahtarlarına erişim de dahil olmak üzere engellenir. Uygulamanın bakış açısından, uygulamalar farklı sanal makinelere geçebildiği (ve yapabildiği) için kayıt defterine yazma erişimine azure ortamında asla güvenilmemelidir. Bir uygulamaya bağlı olabilecek tek kalıcı yazılabilir depolama alanı, App Service UNC paylaşımlarında depolanan uygulama başına içerik dizini yapısıdır. 

## <a name="remote-desktop-access"></a>Uzak masaüstü erişimi

Uygulama Hizmeti, VM örneklerine uzak masaüstü erişimi sağlamaz.

## <a name="more-information"></a>Daha fazla bilgi

[Azure App Service sandbox](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox) - Uygulama Hizmeti'nin yürütme ortamı hakkında en güncel bilgiler. Bu sayfa doğrudan App Service geliştirme ekibi tarafından tutulur.

