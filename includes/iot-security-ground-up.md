---
title: include dosyası
description: include dosyası
services: iot-fundamentals
author: robinsh
ms.service: iot-fundamentals
ms.topic: include
ms.date: 04/24/2018
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: 128a2e8b200f1323b88aad635f27c1b686ecbed2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72789709"
---
Nesnelerin İnterneti (IoT), dünya çapındaki işletmeler için benzersiz güvenlik, gizlilik ve uyumluluk zorlukları oluşturmaktadır. Bu konuların yazılım ve nasıl uygulandığı etrafında döndüğü geleneksel siber teknolojinin aksine, IoT siber ve fiziksel dünyalar birleştiğinde ne olacağıyla ilgilidir. IoT çözümlerinin korunması, cihazların güvenli bir şekilde sağlanmasını, bu aygıtlar ve bulut arasında güvenli bağlantı sağlanmasını ve işleme ve depolama sırasında bulutta veri korumasının güvenli olmasını gerektirir. Ancak, bu tür işlevlere karşı çalışmak, kaynak kısıtlamalı aygıtlar, dağıtımların coğrafi dağılımı ve çözüm içindeki çok sayıda aygıttır.

Bu makalede, IoT çözüm hızlandırıcılarının güvenli ve özel Nesnelerin İnterneti bulut çözümü nasıl sağlandığını inceleyeceğimiz. Çözüm hızlandırıcıları, güvenlik sıfırdan her aşamada yerleşik olarak eksiksiz bir uçtan uca çözüm sunar. Microsoft'ta, güvenli yazılım geliştirmek, Microsoft'un güvenli yazılım geliştirme deki on yıllardır süren deneyimine dayanan yazılım mühendisliği uygulamalarının bir parçasıdır. Bunu sağlamak için, Güvenlik Geliştirme Yaşam Döngüsü (SDL), Operasyonel Güvenlik Güvencesi (OSA) ve Microsoft Dijital Suçlar Birimi, Microsoft gibi altyapı düzeyinde güvenlik hizmetleri bir dizi ile birleştiğinde temel geliştirme metodolojisidir Güvenlik Yanıt Merkezi ve Microsoft Kötü Amaçlı YazılımLarı Koruma Merkezi.

Çözüm hızlandırıcıları, IoT aygıtlarından veri sağlamayı, bağlanmayı ve depolamayı kolay, şeffaf ve en önemlisi güvenli hale getiren benzersiz özellikler sunar. Bu makalede, güvenlik, gizlilik ve uyumluluk zorluklarının ele alınmasını sağlamak için Azure IoT çözüm hızlandırıcıları güvenlik özelliklerini ve dağıtım stratejilerini inceler.

## <a name="introduction"></a>Giriş

Nesnelerin İnterneti (IoT), işletmelere maliyetleri azaltmak, gelirlerini artırmak ve işlerini dönüştürmek için anında ve gerçek dünya fırsatları sunan geleceğin dalgasıdır. Ancak birçok işletme, güvenlik, gizlilik ve uyumluluk la ilgili endişeler nedeniyle IoT'yi kuruluşlarına dağıtmakta tereddüt etmektedir. Önemli bir endişe noktası, siber ve fiziksel dünyaları birleştiren ve bu iki dünyada doğal olarak bireysel riskleri birleştiren IoT altyapısının benzersizliğinden kaynaklanmıyor. IoT'nin güvenliği, aygıtlarda çalışan kodun bütünlüğünü sağlamak, cihaz ve kullanıcı kimlik doğrulaması sağlamak, cihazların net mülkiyetini (ve bu aygıtlar tarafından oluşturulan verileri) tanımlamaya ve siber ve fiziksel saldırılara karşı dirençli olmakla ilgilidir.

Bir de mahremiyet sorunu var. Şirketler, toplanan ve neden toplandığı, kimlerin görebileceği, erişimi kimlerin kontrol ettiği gibi veri toplama konusunda şeffaflık isterler. Son olarak, ekipmanın genel güvenlik sorunları ve bunları çalıştıran kişiler ve endüstri standartlarının uyumluluğu koruma sorunları vardır.

Güvenlik, gizlilik, şeffaflık ve uyumluluk endişeleri göz önüne alındığında, doğru IoT çözüm sağlayıcısını seçmek zor olmaya devam etmektedir. Çeşitli satıcılar tarafından sağlanan IoT yazılım ve hizmetlerinin tek tek parçalarını bir araya getirmek, düzeltmek şöyle dursun, algılaması zor olabilecek güvenlik, gizlilik, şeffaflık ve uyumluluk boşluklarını ortaya getirir. Doğru IoT yazılım ve servis sağlayıcısının seçimi, dikey ve coğrafyalara yayılan, ancak aynı zamanda güvenli ve şeffaf bir şekilde ölçeklendirebilen geniş bir hizmet çalıştırma deneyimine sahip sağlayıcılar bulmaya dayanır. Benzer şekilde, seçilen sağlayıcının dünya çapında milyarlarca makinede çalışan güvenli yazılım lar geliştirme konusunda onlarca yıllık deneyime sahip olmasına ve Nesnelerin İnterneti'nin bu yeni dünyasının yarattığı tehdit ortamını takdir etme yeteneğine sahip olmasına yardımcı olur.

## <a name="secure-infrastructure-from-the-ground-up"></a>Sıfırdan güvenli altyapı

[Microsoft Cloud](https://azure.microsoft.com) altyapısı, 127 ülkede/bölgede bir milyardan fazla müşteriyi destekler. Microsoft'un kurumsal yazılım oluşturma ve dünyanın en büyük çevrimiçi hizmetlerinden bazılarını çalıştırma deneyiminden yararlanan Microsoft Cloud, daha yüksek düzeyde gelişmiş güvenlik, gizlilik, uyumluluk ve tehdit azaltma uygulamaları sağlar çoğu müşteri kendi başına elde edebilirim daha.

[Güvenlik Geliştirme Yaşam Döngüsü (SDL),](https://www.microsoft.com/sdl/) güvenlik gereksinimlerini tüm yazılım yaşam döngüsüne yerleştiren şirket çapında zorunlu bir geliştirme süreci sağlar. Operasyonel faaliyetlerin aynı güvenlik uygulamalarını izlemesini sağlamak için SDL, Microsoft'un Operasyonel Güvenlik Güvencesi (OSA) sürecinde belirtilen sıkı güvenlik yönergelerini kullanır. Microsoft ayrıca uyumluluk yükümlülüklerini yerine getirdiğini sürekli doğrulama için üçüncü taraf denetim firmalarıyla birlikte çalışır ve Microsoft, Microsoft Dijital Suçlar Birimi de dahil olmak üzere mükemmellik merkezleri nin oluşturulması yoluyla geniş güvenlik çabalarını yürütür, Microsoft Güvenlik Yanıt Merkezi ve Microsoft Kötü Amaçlı Yazılımları Koruma Merkezi.

## <a name="microsoft-azure---secure-iot-infrastructure-for-your-business"></a>Microsoft Azure - işletmeniz için güvenli IoT altyapısı

Microsoft Azure, sürekli büyüyen entegre bulut hizmetleri koleksiyonunu (analitik, makine öğrenimi, depolama, güvenlik, ağ ve web) koruma ve endüstri lideri bir taahhütle birleştiren eksiksiz bir bulut çözümü sunar verilerinizin gizliliği. Microsoft'un [ihlal](https://azure.microsoft.com/blog/red-teaming-using-cutting-edge-threat-simulation-to-harden-the-microsoft-enterprise-cloud/) stratejisi, saldırıları simüle eden, Azure'un ortaya çıkan tehditleri algılama, koruma ve ihlallerden kurtarma yeteneğini test eden özel bir yazılım güvenliği uzmanı *ekibi* kullanır. Microsoft'un [genel olay yanıt](https://www.microsoft.com/en-us/TrustCenter/Security/DesignOpSecurity) ekibi, saldırıların ve kötü amaçlı etkinliklerin etkilerini azaltmak için 24 saat çalışır. Ekip, olay yönetimi, iletişim ve kurtarma için belirlenmiş yordamları izler ve dahili ve harici ortaklarla keşfedilebilir ve öngörülebilir arabirimleri kullanır.

Microsoft'un sistemleri, tehditleri belirlemeye ve azaltmaya yardımcı olan sürekli izinsiz giriş algılama ve önleme, hizmet saldırısı önleme, düzenli nüfuz testi ve adli araçlar sağlar. [Çok faktörlü kimlik doğrulama,](../articles/active-directory/authentication/multi-factor-authentication.md) son kullanıcıların ağa erişebilmek için fazladan bir güvenlik katmanı sağlar. Uygulama ve ana bilgisayar sağlayıcısı için Microsoft, erişim denetimi, izleme, kötü amaçlı yazılımdan koruma, güvenlik açığı taraması, düzeltme ve yapılandırma yönetimi sunar.

Çözüm hızlandırıcıları, tüm Microsoft yazılımlarının güvenli bir şekilde geliştirilmesi ve işletilmesi için SDL ve OSA süreçlerinin yanı sıra Azure platformunda yerleşik güvenlik ve gizlilikten yararlanır. Bu yordamlar, altyapı koruması, ağ koruması ve kimlik ve yönetim özellikleri herhangi bir çözümün güvenliği için temel özellikleri sağlar.

[IoT çözüm hızlandırıcıları](../articles/iot-fundamentals/iot-introduction.md) içindeki [Azure IoT](../articles/iot-hub/about-iot-hub.md) Hub'ı, cihaz başına güvenlik kimlik bilgileri ve erişim denetimi kullanarak IoT aygıtları ile [Azure Machine Learning](../articles/machine-learning/studio/what-is-machine-learning.md) ve Azure Akış [Analitiği](../articles/stream-analytics/stream-analytics-introduction.md) gibi Azure hizmetleri arasında güvenilir ve güvenli çift yönlü iletişim sağlayan tam yönetilen bir hizmet sunar.

Azure IoT çözüm hızlandırıcılarında yerleşik güvenlik ve gizlilik özelliklerini en iyi şekilde iletmek için, bu makale paketi üç birincil güvenlik alanına ayırır.

![Azure IoT çözüm hızlandırıcıları](media/iot-security-ground-up/securing-iot-ground-up-fig3.png)

### <a name="secure-device-provisioning-and-authentication"></a>Güvenli aygıt sağlama ve kimlik doğrulama

Çözüm hızlandırıcıları, ioT altyapısı tarafından cihazla çalışırken iletişim kurmak için kullanılabilen her cihaz için benzersiz bir kimlik anahtarı sağlayarak, sahadayken cihazları güvenli hale getirmektedir. İşlem hızlı ve kolay ayarlanır. Kullanıcı tarafından seçilen aygıt kimliğiyle oluşturulan anahtar, aygıt ve Azure IoT Hub arasındaki tüm iletişimde kullanılan bir belirteci temeline oluşturur.

Aygıt kimlikleri üretim sırasında bir aygıtla ilişkilendirilebilir (diğer bir deyişle, donanım güven modülünde yanıp söner) veya varolan bir sabit kimliği proxy olarak kullanabilir (örneğin CPU seri numaraları). Aygıttaki bu tanımlayıcı bilgileri değiştirmek basit olmadığından, altta yatan aygıt donanımı değişir, ancak mantıksal aygıt aynı kalır durumda mantıksal aygıt kimliklerini tanıtmak önemlidir. Bazı durumlarda, aygıt kimliğinin ilişkilendirilmesi aygıt dağıtım zamanında gerçekleşebilir (örneğin, kimlik doğrulaması yapılmış bir alan mühendisi çözüm arka uçla iletişim kurarken yeni bir aygıtı fiziksel olarak yapılandırır). [Azure IoT Hub kimlik kayıt defteri,](../articles/iot-hub/iot-hub-devguide.md) bir çözüm için aygıt kimliklerinin ve güvenlik anahtarlarının güvenli bir şekilde depolanmasını sağlar. Tek tek veya aygıt kimlik grupları izin listesine veya aygıt erişimi üzerinde tam denetim sağlayan bir engelleme listesine eklenebilir.

Buluttaki Azure IoT Hub erişim denetim ilkeleri etkinleştirilmesini ve herhangi bir aygıt kimliğini devre dışı bırakmayı sağlayarak aygıtı gerektiğinde IoT dağıtımından ayırmanın bir yolunu sağlar. Aygıtların bu ilişkisi ve ayrıştırma her aygıt kimliğine dayanır.

Ek aygıt güvenlik özellikleri şunlardır:

* Aygıtlar istenmeyen ağ bağlantılarını kabul etmez. Tüm bağlantıları ve yolları yalnızca giden bir şekilde kurarlar. Bir aygıtın arka uçtan bir komut alabilmesi için, aygıtın bekleyen komutların işlenmesini denetlemek için bir bağlantı başlatması gerekir. Aygıt ve IoT Hub arasında bir bağlantı güvenli bir şekilde kurulduktan sonra, buluttan aygıta ve aygıta iletiler saydam olarak gönderilebilir.

* Aygıtlar yalnızca Azure IoT Hub'ı gibi iyi baktıkları tanınmış hizmetlere bağlanır veya bunlara giden yollar oluşturur.

* Sistem düzeyinde yetkilendirme ve kimlik doğrulama, erişim kimlik bilgilerini ve izinleri anında geri alınabilir hale getirerek aygıt başına kimlik kullanımı sağlar.

### <a name="secure-connectivity"></a>Güvenli bağlantı

Mesajlaşmanın dayanıklılığı herhangi bir IoT çözümünün önemli bir özelliğidir. Komutları güvenilir bir şekilde teslim etme ve/veya aygıtlardan veri alma gereksinimi, IoT aygıtlarının Internet üzerinden bağlı olması veya güvenilmez olabilecek diğer benzer ağlar la vurgulanır. Azure IoT Hub, iletilere yanıt olarak bir bildirim sistemi aracılığıyla bulut ve aygıtlar arasında mesajlaşmanın dayanıklılığını sunar. İleti için ek dayanıklılık, ioT Hub'daki iletilerin telemetri için yedi güne kadar, komutlar için iki güne kadar önbelleğe alınarak sağlanır.

Verimlilik, kaynakların korunmasını ve kaynak kısıtlı bir ortamda işletilmesini sağlamak için önemlidir. Popüler http protokolünün endüstri standardı güvenli sürümü HTTPS (HTTP Secure), Azure IoT Hub tarafından desteklenerek verimli iletişim sağlar. Azure IoT Hub tarafından desteklenen Gelişmiş İleti Kuyruk Protokolü (AMQP) ve Message Queuing Telemetri Transport (MQTT), yalnızca kaynak kullanımı açısından verimlilik için değil, aynı zamanda güvenilir ileti teslimi için de tasarlanmıştır. 

Ölçeklenebilirlik, çok çeşitli cihazlarla güvenli bir şekilde birlikte çalışma olanağı gerektirir. Azure IoT hub'ı hem IP etkin hem de IP etkin olmayan aygıtlara güvenli bağlantı sağlar. IP özellikli aygıtlar, güvenli bir bağlantı üzerinden IoT Hub ile doğrudan bağlantı kurabilir ve iletişim kurabilir. IP özellikli olmayan aygıtlar kaynak kısıtlaması sağlar ve yalnızca Zwave, ZigBee ve Bluetooth gibi kısa mesafeiletişim protokolleri üzerinden bağlanır. Bu aygıtları toplamak için bir alan ağ geçidi kullanılır ve bulutla güvenli çift yönlü iletişim sağlamak için protokol çevirisi gerçekleştirir.

Ek bağlantı güvenliği özellikleri şunlardır:

* Aygıtlar ve Azure IoT Hub arasındaki veya ağ geçitleri ile Azure IoT Hub arasındaki iletişim yolu, X.509 protokolü kullanılarak kimlik doğrulaması yapılan Azure IoT Hub ile endüstri standardı Aktarım Katmanı Güvenliği (TLS) kullanılarak güvenli hale verilir.

* Aygıtları istenmeyen gelen bağlantılara karşı korumak için Azure IoT Hub aygıta herhangi bir bağlantı açmaz. Aygıt tüm bağlantıları başlatır.

* Azure IoT Hub, aygıtlar için iletileri kalıcı olarak saklar ve aygıtın bağlanmasını bekler. Bu komutlar iki gün boyunca saklanır ve güç veya bağlantı sorunları nedeniyle ara sıra bağlanan aygıtların bu komutları almasını sağlar. Azure IoT Hub, her aygıt için aygıt başına bir kuyruk tutar.

### <a name="secure-processing-and-storage-in-the-cloud"></a>Bulutta güvenli işleme ve depolama

Çözüm hızlandırıcıları, şifreli iletişimden buluttaki verileri işlemeye kadar verilerin güvenliğini sağlamaya yardımcı olur. Ek şifreleme ve güvenlik anahtarları nın yönetimini uygulamak için esneklik sağlar.

Azure IoT çözüm hızlandırıcıları, kullanıcı kimlik doğrulaması ve yetkilendirmesi için Azure Active Directory 'i (AAD) kullanarak buluttaki veriler için ilke tabanlı bir yetkilendirme modeli sağlayarak denetlenebilir ve gözden geçirilebilen kolay erişim yönetimi sağlayabilir. Bu model ayrıca buluttaki verilere ve Azure IoT çözüm hızlandırıcılarına bağlı aygıtlara erişimin neredeyse anında iptal edilmesini de sağlar.

Veriler buluta yüklendikten sonra, kullanıcı tarafından tanımlanan herhangi bir iş akışında işlenebilir ve depolanabilir. Verilerin her bölümüne erişim, kullanılan depolama hizmetine bağlı olarak Azure Active Directory ile denetlenir.

IoT altyapısı tarafından kullanılan tüm anahtarlar bulutta güvenli depolamada saklanır ve anahtarların yeniden sağlanması gerektiğinde devrilme özelliği de vardır. Veriler [Azure Cosmos DB'de](../articles/cosmos-db/introduction.md) veya [SQL veritabanlarında](../articles/sql-database/sql-database-faq.md)depolanabilir ve böylece istenen güvenlik düzeyinin tanımlanmasını sağlar. Ayrıca Azure, herhangi bir izinsiz giriş veya yetkisiz erişim konusunda sizi uyarmak için verilerinize tüm erişimi izlemek ve denetlemek için bir yol sağlar.

## <a name="conclusion"></a>Sonuç

Nesnelerin İnterneti, işletmeler için en önemli şeyler olan şeylerle başlar. IoT, maliyetleri azaltarak, geliri artırarak ve işletmeyi dönüştürerek bir işletmeye inanılmaz bir değer sunabilir. Bu dönüşümün başarısı büyük ölçüde doğru IoT yazılım ve servis sağlayıcısının seçilmesine bağlıdır. Bu, yalnızca iş gereksinimlerini ve gereksinimleri anlayarak bu dönüşümü hızlandıran bir sağlayıcı bulmak la birlikte, aynı zamanda güvenlik, gizlilik, şeffaflık ve uyumlulukla birlikte temel tasarım konuları olarak oluşturulmuş hizmetler ve yazılımlar da sağlar. Microsoft, güvenli yazılım ve hizmetler geliştirme ve dağıtma konusunda geniş deneyime sahiptir ve nesnelerin internetinin bu yeni çağında lider olmaya devam etmektedir.

Çözüm hızlandırıcıları, verimliliği artırmak, inovasyonu etkinleştirmek için operasyonel performansı artırmak ve işletmeleri dönüştürmek için gelişmiş veri analitiği kullanmak için varlıkların güvenli bir şekilde izlenmesine olanak sağlayarak, tasarım gereği güvenlik önlemleri oluşturur. Güvenlik, birden çok güvenlik özelliği ve tasarım desenleri için katmanlı yaklaşımıyla çözüm hızlandırıcıları, herhangi bir işletmeyi dönüştürmek için güvenilen bir altyapının dağıtılmasına yardımcı olur.

## <a name="additional-information"></a>Ek bilgiler

Her çözüm hızlandırıcısı, azure hizmetlerinin örnekleri oluşturur:

* [**Azure IoT Hub'ı**](https://azure.microsoft.com/services/iot-hub/): Bulutu aygıtlara bağlayan ağ geçidiniz. Hub başına milyonlarca bağlantıya ölçeklendirebilir ve çözümünüzü güvence altına almanıza yardımcı olan cihaz başına kimlik doğrulama desteğiyle büyük hacimli verileri işleyebilirsiniz.

* [**Azure Cosmos DB**](https://azure.microsoft.com/services/cosmos-db/): Öznitelikler, yapılandırma ve güvenlik özellikleri gibi sağladığınız aygıtlar için meta verileri yöneten yarı yapılandırılmış veriler için ölçeklenebilir, tam dizine sahip veritabanı hizmeti. Azure Cosmos DB, yüksek performanslı ve yüksek iş işlemcisi işleme, verilerin şema-agnostik dizini ve zengin bir SQL sorgu arabirimi sunar.

* [**Azure Akış Analizi**](https://azure.microsoft.com/services/stream-analytics/): Cihazlardan, sensörlerden, altyapıdan ve uygulamalardan gerçek zamanlı öngörüler ortaya çıkarmak için düşük maliyetli bir analitik çözüm geliştirmenizi ve dağıtmanızı sağlayan bulutta gerçek zamanlı akış işleme. Bu tam olarak yönetilen hizmetten elde edilen veriler, yüksek iş hacmi, düşük gecikme gecikmesi ve esneklik elde ederken herhangi bir hacme ölçeklenebilir.

* [**Azure Uygulama Hizmetleri**](https://azure.microsoft.com/services/app-service/): Her yerde verilere bağlanan güçlü web ve mobil uygulamalar oluşturmak için bir bulut platformu; bulutta veya şirket içinde. iOS, Android ve Windows için ilgi çekici mobil uygulamalar oluşturun. Hizmet Olarak Yazılımınızla (SaaS) ve kurumsal uygulamalarınızla, düzinelerce bulut tabanlı hizmet ve kurumsal uygulamayla güncel olmayan bağlantıyla tümleştirin. Web uygulamaları ve API'leri her zamankinden daha hızlı oluşturmak için en sevdiğiniz dilde ve IDE—.NET, Node.js, PHP, Python veya Java'da kodlayın.

* [**Logic Apps**](https://azure.microsoft.com/services/app-service/logic/): Azure Uygulama Hizmetinin Mantıksal Uygulamalar özelliği, IoT çözümünüzü mevcut iş hattı sistemlerinize entegre etmeye ve iş akışı süreçlerini otomatikleştirmenize yardımcı olur. Logic Apps, geliştiricilerin tetikleyiciden başlayan iş akışlarını tasarlamasına ve ardından iş süreçlerinizle tümleştirmek için güçlü bağlayıcılar kullanan bir dizi adım-kural ve eylem yürütmesine olanak tanır. Logic Apps, SaaS, bulut tabanlı ve şirket içi uygulamalardan oluşan geniş bir ekosisteme güncel bağlantılar sunar.

* [**Azure Blob depolama**](https://azure.microsoft.com/services/storage/): Aygıtlarınızın buluta gönderdiği veriler için güvenilir ve ekonomik bulut depolama.