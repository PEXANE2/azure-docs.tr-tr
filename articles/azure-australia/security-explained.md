---
title: Azure Avustralya güvenliği açıklanıyor
description: Avustralya bölgeleri hakkında en fazla bilgi ve Avustralya kamu ilkesi, yönetmelikler ve yasalların belirli gereksinimlerini karşılayarak.
author: galey801
ms.service: azure-australia
ms.topic: conceptual
ms.date: 07/22/2019
ms.author: grgale
ms.openlocfilehash: 212d14e31c152e50c216f2f34fb225c29fe054b5
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68571438"
---
# <a name="azure-australia-security-explained"></a>Azure Avustralya güvenliği açıklanıyor

Bu makalede, Avustralya devlet kurumlarının Microsoft Azure Avustralya 'da araştırmasını, tasarlamayı ve dağıtılmasını sağlayan bazı yaygın soruların ve alanların bazıları ele alınmaktadır.

## <a name="irap-and-certified-cloud-services-list-ccsl-documents"></a>IRAP ve sertifikalı Cloud Services listesi (CCSL) belgeleri

Avustralya Cyber Güvenlik Merkezi (ACSC), CCSL 'ye eklendiğinde hizmet için bir dizi sertifika, sertifika raporu ve bir tüketici kılavuzu sağlar.

Microsoft şu anda Azure için CCSL, Office 365 ve Dynamics 365 CRM 'de listelenmiştir.

Microsoft, denetim, değerlendirme ve ACSC sertifika belgelerimizi [Microsoft hizmet güveni portalının](https://aka.ms/au-irap)Avustralya 'ya özgü bir sayfasındaki müşteriler ve iş ortakları için kullanılabilir hale getirir.

## <a name="dissemination-limiting-markers-dlm-and-protected-certification"></a>Sınırlama Işaretlerini (DLD) ve KORUMALı sertifikayı yayma

Kamu kurumları tarafından kullanılmak üzere onaylanan, bulut hizmetleri de dahil olmak üzere sistemlerin bulunduğu işlem, Avustralya Cyber Güvenlik Merkezi (ACSC) tarafından oluşturulan ve yayımlanan [bilgi güvenliği el ile (ISM)](https://acsc.gov.au/infosec/ism/) tanımlanmıştır. Avustralya Siber Güvenlik Merkezi (ACSC), bu, siber güvenlik ve bulut sertifikasıdır.

Onay süreci için iki adım vardır:

1. Güvenlik değerlendirmesi (ıRAP)-kayıtlı uzmanlarının sistem, hizmet ve çözümlerin ıSM içindeki teknik denetimlere karşı değerlendirmesi ve denetimlerin etkin bir şekilde uygulanıp uygulanmadığını değerlendirmek için bir süreç. Değerlendirme Ayrıca, onay yetkilisinin çalışması için bir onay vermeden önce dikkate alınması gereken belirli riskleri de tanımlar (ATO).

1. Çalışmaya onay: bir devlet kurumu Kıdemli Müdür 'nin bir sistem tarafından tanınan, depoladığı ve iletişim kurduğu bilgilere daha fazla risk kabul ettiği süreç.  Bu işlemin bir girişi güvenlik değerlendirmesi olur.

Azure hizmetlerinin KORUMALı düzeyde değerlendirmesi, korunan ve aşağıdaki verilerin depolanması ve işlenmesi için gerekli olan güvenlik denetimlerinin uygulanması onaylanan ve etkin bir şekilde doğrulandığını belirler.

## <a name="australian-data-classification-changes"></a>Avustralya veri sınıflandırması değişiklikleri

1 Ekim 2018 ' de, Attorney genel bölümü, özellikle yeni bir [hassas ve sınıflandırılmış bir bilgi sistemi](https://www.protectivesecurity.gov.au/information/sensitive-classified-information/Pages/default.aspx)olan koruyucu güvenlik ilkesi çerçevesi 'NDE (Pspf) yapılan değişiklikleri genel olarak duyurmuştur.

![Düzeltilen PSPF sınıflandırmaları](media/pspf-classifications.png)

PSPF altında çalışan tüm Avustralya kurumları ve organizasyonlar bu değişikliklerden etkilenir. Geçerli ıRAP/CCSL sertifikalarımızı etkileyen birincil değişiklik, geçerli yayma sınırlaması Işaretlerini (DLM) devre dışı bırakıldı. Resmi işaretleme **: Hassas** bilgilerin korunması için kullanılan çeşitli DLCI 'ler hassas şekilde değiştirilir. Bu değişiklik, tüm duyarlılık ve sınıflandırma düzeylerinde tüm resmi bilgilere uygulanabilen üç bilgi yönetimi işaretleyicisi de ortaya sunmuştur. **Korunan** sınıflandırma değişmeden kalır.

Sınıflandırılmamış terimi yeni sistemden kaldırılır ve resmi olmayan bilgiler, resmi bir işaret gerektirmese de kamu dışı bilgilere uygulanır.

## <a name="choosing-an-azure-region-for-my-official-sensitive-and-protected-workloads"></a>RESMI olarak Azure bölgesi seçme: Gizli ve KORUMALı iş yükleri

Azure **resmi: Hassas** ve **korumalı** sertifikalı hizmetler dört Avustralya veri merkezi bölgesine dağıtılır (Avustralya Doğu, Avustralya Güney Doğu, Avustralya orta ve Avustralya Orta 2); ancak, tarafından verilen sertifika raporu ACSC, bir hizmet varsa, **korumalı** verilerin Canberde Azure Kamu bölgelerine dağıtılmasını önerir. **Korumalı** sertifikalı Azure hizmetleri hakkında daha ayrıntılı bilgi IÇIN, [STP sayfasındaki Avustralya sayfasından](https://aka.ms/au-irap)ulaşılabilir.

>[!NOTE]
>Microsoft, tüm sensitivities kamu verilerini önerir ve sınıflandırmaların, özellikle kamu ihtiyaçları için tasarlandıkları ve işledikleri için Avustralya Orta ve Avustralya Orta 2 bölgelere dağıtılır.

Azure Avustralya bölgelerinin özel doğası hakkında daha fazla bilgiyi [Azure Avustralya orta bölgeleri](https://azure.microsoft.com/global-infrastructure/australia/)adresinden bulabilirsiniz.

## <a name="how-microsoft-separates-classified-and-official-data"></a>Microsoft 'un sınıflandırılan ve resmi verileri ayıran

Microsoft, Azure ve Office 365 ' i tüm veriler hassas ve/veya sınıflandırıldığında, bu yüksek çubuğa yönelik güvenlik denetimlerimizi oluşturan Avustralya 'da çalışır.

Azure destekleyen altyapı, çok sayıda sınıflandırmadaki verileri de görüyor olabilir.  Ancak varsayımızın müşteri verilerinin sınıflandırıldığından, uygun denetimlerin de olduğu gibi yer aldığı varsayılır. Microsoft, **gizli sınıflandırılan** bilgileri depolamak ve işlemek için Pspf gereksinimleriyle uyumlu olan hizmetlerimiz için bir temel güvenlik duruşunu benimsemiştir.

Müşterilerin Azure 'daki bir kiracının diğer kiracılardan risk altında olmadığından emin olmak için, Microsoft kapsamlı savunma denetimleri uygular.

Microsoft Azure platformunda uygulanan yeteneklerin ötesinde, müşteri tarafından yönetilen anahtarlarla şifreleme, iç içe sanallaştırma ve tam zamanında yönetim erişimi gibi ek müşteri yapılandırılabilir denetimleri, riski daha da azaltabilir. Canbera 'daki Azure Kamu Avustralya bölgelerinde, yalnızca Yeni Zelanda kamu ve ulusal kritik altyapı kuruluşları & resmi bir kez beyaz listeye almak için bir işlem gerçekleştirilir. Bu topluluk bulutu, ortak Kiracı riskleri açısından önemli olan kuruluşlar için ek güvence sağlar.

Microsoft Azure **korumalı** sertifika raporu bu denetimlerin, **korunan** sınıflandırılan verilerin depolanması ve işlenmesi için etkili olduğunu ve bunların yalıtımlarını onaylar.

## <a name="relevance-of-irapccsl-to-state-government-and-critical-infrastructure-providers"></a>IRAP/CCSL 'nin Eyalet Kamu ve kritik altyapı sağlayıcılarının ilgisi

Birçok eyalet kamu ve kritik altyapı sağlayıcısı, Federal Kamu gereksinimlerini güvenlik ilkeleri ve güvence çerçevesiyle birleştirir. Bu **kuruluşlar, resmi**, resmi olarak **da işler: Federal** kamu veya kendi hakkı ile etkileşimlerinden hassas ve **korunan** , sınıflandırılmış verilerin miktarı.

Avustralya kamu, gittikçe daha fazla odaklanan ilke ve Avustralya 'nın güvenlik ve ekonomik kullanımını temel alan kamu dışı verilerin korunmasında yasaldır. Bu nedenle, Azure Avustralya bölgeleri ve CCSL sertifikası işbu sektörlerin tümüyle ilgilidir.

![Kritik altyapı kesimleri](media/nci-sectors.png)

Microsoft sertifikaları, Azure hizmetlerinin güvenlik korumalarının yerinde, kapsamlı ve resmi bir değerlendirmesi için tabi olduğunu ve bu yüksek oranda duyarlı verileri işlemeye yönelik onaylanmış olduğunu gösterir.

## <a name="location-and-control-of-microsoft-data-centres"></a>Microsoft veri Centres 'nin konumu ve denetimi

Veri merkezi konumunu ve verilerini işleyen bulut hizmetlerinin sahipliğini açıkça bilen bir kamu ve kritik altyapı gereksinimi zorunludur.  Microsoft, bu konumlar ve sahiplik hakkında kapsamlı bilgiler sağlayan bir hiper ölçekli bulut sağlayıcısı olarak benzersizdir.

Microsoft 'un Azure Avustralya bölgeleri (Avustralya Orta ve Avustralya Orta 2), CDC veri Centres 'in tesislerinde çalıştırılır.  CDC veri Centres 'nin sahipliği, Infratil 'den% 48 sahiplik ile Avustralya, 'tan% 48 sahiplik (NZ tabanlı, Çift Avustralya ve Yeni Zelanda stok Exchange tarafından listelenen uzun vadeli altyapı varlık Fonu) ve% 4 Avustralya yönetimi.  

CDC veri Centres 'nin yönetimi, devlet kurumlarının, daha sonra sahiplik ve denetim aktarımını kısıtlayan Avustralya kamu ile ilgili olarak sözleşme sahibi olduğunu bildiriyor. Microsoft 'un CDC Vercentres ortaklığı aracılığıyla tedarik zinciri ve sahipliğinin bu saydamlığı, [kamu barındırma stratejisinin tamamının](https://www.dta.gov.au/our-projects/whole-government-hosting-strategy) ve **sertifikalı bir Sogeign veri**merkezinin tanımıyla birlikte olacaktır.

## <a name="the-azure-services-that-are-included-in-the-current-ccsl-certification"></a>Geçerli CCSL sertifikacıya dahil edilen Azure hizmetleri

2017 Haziran 'da, verileri **sınıflandırılmayan şekilde depolama ve işleme için ACSC sertifikalı 41 Azure hizmetleri:**  DLCI düzeyi. 2018 Nisan 'da, bu hizmetlerin 24 ' i **korunan** sınıflandırılan veriler için sertifikalandırıldı.

Avustralya 'daki Azure Bölgelerimiz genelinde ACSC sertifikalı Azure hizmetlerinin kullanılabilirliği şu şekildedir:

|Azure Avustralya Merkez Bölgeleri|Bölgesel olmayan hizmetler ve diğer bölgeler|
|---|---|
|API Management, uygulama ağ geçidi, Uygulama Hizmetleri, **Otomasyon, Azure Portal, yedekleme, Batch, Cloud Services**, Cosmos DB, Event Hubs, **ExpressRoute**, hdınsight, **Key Vault**, Load Balancer, Log Analytics, **Multi-Factor Kimlik doğrulaması**, Redis Cache, **Kaynak Yöneticisi, Service Bus, Service Fabric, SITE Recovery, SQL veritabanı, depolama**, Traffic Manager, **sanal makineler, sanal ağ, VPN Gateway**|**Azure Active Directory**, CDN, Veri Kataloğu, **içeri aktarma, Information Protection, ıot hub**, Machine Learning, Media Services, **Notification Hubs**, Power BI, **güvenlik merkezi, Zamanlayıcı**, arama, Stream Analytics|
|

Microsoft, ıRAP/CCSL dahil olmak üzere tüm küresel, Kamu, sektör ve bölgesel uyumluluk ve değerlendirme işlemlerine yönelik tüm kapsam içi hizmetleri listeleyen [Microsoft Azure uyumluluğuna genel bakış](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942/file/178110/44/Microsoft%20Azure%20Compliance%20Offerings.pdf) yayımlar.

## <a name="azure-service-not-listed-or-assessed-at-a-lower-level-than-i-need"></a>Azure hizmeti, gereksinimden daha düşük bir düzeyde listelenmemiş veya değerlendirilmedi

Sertifikalı veya **resmi olarak sertifikalı hizmetler: Hassas** ancak **korunmuyor**, **korunan** verileri barındıran bir çözümün parçası olarak veya bu hizmetlerin bir parçası olarak kullanılabilir:
1. **korumalı** verileri depolamamakta veya işlenmiyor veya
1. bir risk değerlendirmesi tamamladınız ve **korunan** verileri kendiniz depolamak için hizmeti onayladınız.

**Resmi** verileri depolamak ve işlemek için CCSL 'ye dahil olmayan bir hizmet kullanmak istiyorsanız, bir bulut hizmeti sağlayıcısı ile bir sözleşmeyi girmeden veya yenilemeden önce, bunu yaptığınız gibi ACSC 'ye bildirimde bulunmasını istemeniz gerekir.

**Korumalı** iş yükleri için bir kurum tarafından kullanılan herhangi bir hizmetin, bir güvenlik değerlendirmesi ve bir arada olması gerekir. Bu, [dta](https://www.dta.gov.au/files/cloud-strategy/secure-cloud-strategy.pdf)ve

![DTA güvenli bulut stratejisi sertifika süreci](media/certification.png)

Microsoft, müşterilerimizin Avustralya kamu kullanımı açısından güvenli ve amacına uygun olmasını sağlamak için hizmetleri sürekli olarak değerlendirir, bu nedenle şu anda **korunan**CCSL üzerinde olmayan bir hizmetle ilgili yardıma Ihtiyacınız varsa Microsoft 'a ulaşın.

Microsoft, CCSL 'nin hem **SıNıFLANDıRıLMAMıŞ DLCı** 'ler hem de **korumalı** sınıflandırmalar üzerinde sertifikalı bir dizi HIZMETE sahip olduğundan, ISM, hizmetlerimizin en az ıkı yılda bir IRAP değerlendirmesini gerçekleştirmemiz gerekir. Microsoft, bir yıllık değerlendirme alır ve bu da ek hizmetleri dikkate almak için fırsat sağlar.

## <a name="certified-protected-gateway-in-azure"></a>Azure 'da sertifikalı KORUMALı ağ geçidi

Microsoft, ağ geçidi birleştirme programı altında izin verilen SIGs sayısıyla ilgili kısıtlamalar nedeniyle, kamu sertifikalı güvenli Internet ağ geçidini (SıG) işlemez.  Ancak, bir SıG 'ın beklenen ve gerekli özellikleri Microsoft Azure içinde yapılandırılabilir.

Azure hizmetleri 'nin **korumalı** sertifikası sayesinde ACSC, Azure 'a bağlanmak için kurumları ve güvenlik etki alanları arasında ağ kesimlemesini uygularken, örneğin **korumalı** ve arasındaki belirli önerilere sahiptir. Internet. Bu öneriler ağ güvenlik grupları, güvenlik duvarları ve sanal özel ağların kullanımını içerir.  ACSC, sanal ağ geçidi gerecinin kullanımını önerir. Azure 'da, ASD tarafından değerlendirilen ürünler listesinde fiziksel eşdeğeri olan veya ortak ölçütler koruma profillerine göre değerlendirilen ve ortak ölçüt portalında listelenen birkaç sanal gereç vardır. Bu ürünler, ASD 'nin ortak ölçüt tanıma düzenlemesi (CCRA) olarak kabul edilir.

Microsoft, birleştirildiğinde sertifikalı bir SıG ile eşdeğer olan farklı güvenlik etki alanları arasındaki sınırı korumak için gereken güvenlik işlevlerini sağlayan Azure tabanlı özellikleri uygulamaya yönelik yönergeler hazırladı. Bu yeteneklerin tasarımına ve uygulanmasına ve aynı zamanda sağlanan birçok iş ortağı uygulamasına yardımcı olabilecek çeşitli iş ortakları vardır.

## <a name="security-clearances-and-citizenship-of-microsoft-support-personnel"></a>Microsoft destek personeline yönelik güvenlik clearances ve vatandaşlık

Microsoft, araçlarımızı, ekran denetimli ve eğitilen güvenlik personeli ile küresel olarak çalıştırır.  Sidney ve Melbourne 'deki tesislere yönelik fiziksel erişimi olmayan personel, Avustralya kamu temel güvenlik clearances sahiptir. Avustralya Orta ve Avustralya Orta 2 bölgelerinde çalışanların minimum negatif (NV1) clearances ( **gizli** veriler için uygun) vardır. Bu, veri Centres çalışan Azure 'daki personelin büyük ölçüde güvenilir olduğu müşterilere ek güvence sağlar.

Microsoft, tam zamanında ve rol tabanlı erişim denetimlerine göre yalnızca yeterli yönetim içeren bir sistemle erişim izni verilen sıfır erişimli bir erişim ilkesine sahiptir. Çoğu durumda yöneticilerimiz, hizmette sorun gidermek ve bakım yapmak için müşteri verilerine erişim veya ayrıcalıklar gerektirmez.  Uzaktan yürütmeye yönelik görevlerin yüksek derece ve komut dosyası kullanımı, müşteri verilerine doğrudan erişim gereksinimini ortadan kaldırmış.

Attorney-genel ' in Bakanlığı, Microsoft 'un Azure 'daki personel güvenlik ilkelerinin ve yordamlarının, ıNFOSEC-9 ' da bilgi sağlama amacıyla PSPF erişimi amacına uygun olduğunu doğruladı.

## <a name="storing-international-traffic-of-arms-regulations-itar-or-export-administration-regulations-ear-data"></a>Uluslararası veri düzenlemeleri (ıTAR) veya dışa aktarma yönetim düzenlemeleri (EAR) verilerini depolama

Müşterilere dışa aktarma denetimli veriler için yükümlülüklerine yardımcı olan Azure teknik denetimleri, Azure 'da genel olarak aynıdır. Önemli bir değerlendirme ve dışa aktarma denetimli veriler için sertifika çerçevesi olmadığından, önemli bir "ıTAR/EAR" kutusu yoktur.

Azure Kamu ve Office 365 ABD kamu 'da savunma için, müşterilerin dışa aktarma denetimlerine tabi olarak müşterilere daha fazla sözleşme ve işlem ölçümleri koyduk. Bu ek sözleşme yan tümceleri ve Azure bölgelerinin garantili ABD Ulusal desteği ve yönetimi Avustralya için yerinde değildir.

Bu, Avustralya 'daki Azure 'ın ıTAR/EAR için kullanılamayacağını, ancak dışa aktarma lisansınıza uygulanan kısıtlamaları açıkça anlamanız ve Azure 'u depolamak üzere kullanmadan önce bu yükümlülüklerinizi karşılamak için ek korumalar uygulamanız gerekir. Bu veriler. Örneğin, Azure Active Directory bir öznitelik olarak ülke yapısını oluşturmanız gerekebilir, verileri veri üzerinde zorlamak ve yalnızca BIZIMLE ve dışa aktarma lisansına dahil edilen diğer herhangi bir ülke ile sınırlamak için Azure Information Protection kullanabilirsiniz. Azure 'da depolamadan önce Şirket içindeki tüm verileri şifreleyin, müşteri anahtarını kullanarak veya ıTAR verileri için kendi anahtarınızı basılı tutarak, liste açık......

ITAR, resmi bir sertifika olmadığından, dışarı aktarma lisansıyla ilişkili kısıtlamaların ve sınırlamaların ne olduğunu anlamanız ve ardından Azure 'da bu gereksinimleri karşılamak için yeterli denetimler olup olmadığı hakkında bilgi almanız gerekir. Bu durumda, yakından göz önünde bulundurmanız gereken sorunlardan biri, ihracat lisansında onaylanan bir ülke olmaması olabilecek mühendislerimiz tarafından erişim sağlar.

## <a name="next-steps"></a>Sonraki adımlar

Azure Avustralya 'ya yönelik olarak Azure VPN Gateway ile ilgili olarak VPN bağlantısı sağlamak için [azure](vpn-gateway.md) makalesini gözden geçirin
