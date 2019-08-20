---
title: Azure Avustralya güvenliği açıklanıyor
description: Avustralya bölgeleri hakkında en fazla bilgi ve Avustralya kamu ilkesi, yönetmelikler ve yasalların belirli gereksinimlerini karşılayarak.
author: galey801
ms.service: azure-australia
ms.topic: conceptual
ms.date: 07/22/2019
ms.author: grgale
ms.openlocfilehash: 79f5773cf6f4906f93a5b95ce5c042da09c789fc
ms.sourcegitcommit: 5ded08785546f4a687c2f76b2b871bbe802e7dae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2019
ms.locfileid: "69575468"
---
# <a name="azure-australia-security-explained"></a>Azure Avustralya güvenliği açıklanıyor

Bu makalede, Microsoft Azure Avustralya ile araştırıp dağıtımı, tasarımı ve dağıtımı yapan Avustralya kamu kurumları hakkında yaygın soruların ve alanların bazıları ele alınmaktadır.

## <a name="irap-and-certified-cloud-services-list-documents"></a>IRAP ve sertifikalı Cloud Services listesi belgeleri

Avustralya Cyber Güvenlik Merkezi (ACSC), sertifikalı Cloud Services listesine (CCSL) eklendiğinde hizmet için bir dizi sertifika, sertifika raporu ve bir tüketici kılavuzu sağlar.

Microsoft şu anda Azure için CCSL, Office 365 ve Dynamics 365 CRM 'de listelenmiştir.

Microsoft, denetim, değerlendirme ve ACSC sertifika belgelerimizi [Microsoft hizmet güveni portalının](https://aka.ms/au-irap)Avustralya 'ya özgü bir sayfasındaki müşteriler ve iş ortakları için kullanılabilir hale getirir.

## <a name="dissemination-limiting-markers-and-protected-certification"></a>Sınırlama Işaretlerini ve KORUMALı sertifikayı yayma

Kamu kurumları tarafından kullanılmak üzere onaylanan bulut hizmetleri de dahil olmak üzere sistemlerin olma işlemi, ACSC tarafından üretilen ve yayımlanan [bilgi güvenliği el ile (ISM)](https://acsc.gov.au/infosec/ism/) tanımlanmıştır. ACSC, siber güvenlik ve bulut sertifikalarıyla sorumlu olan Avustralya sinyalleri (asd) içindeki varlıktır.

Onay süreci için iki adım vardır:

1. **Güvenlik değerlendirmesi (ıRAP)** : Kayıtlı profesyonellerin sistem, hizmet ve çözümlerin ıSM içindeki teknik denetimlere karşı değerlendirmesi ve denetimlerin etkin bir şekilde uygulanıp uygulanmadığını değerlendirmek için bir işlem. Değerlendirme Ayrıca, onay yetkilisinin çalışması için bir onay vermeden önce dikkate alınması gereken belirli riskleri de tanımlar (ATO).

1. **Çalışacak onay**: Bir devlet kurumuna ait kıdemli bir Başkan Müdürü, işlem yaptığı, depoladığı ve iletişim kurduğu bilgilere bir sistemin daha fazla riskini kabul eder. Bu işlemin bir girişi güvenlik değerlendirmesi olur.

Azure hizmetlerinin KORUMALı düzeyde değerlendirmesi, korunan ve aşağıdaki verilerin depolanması ve işlenmesi için gerekli olan güvenlik denetimlerinin uygulanmasını onaylanan ve verimli bir şekilde işlediğini belirler.

## <a name="australian-data-classification-changes"></a>Avustralya veri sınıflandırması değişiklikleri

Attorney genel bölümünün, 1 Ekim 2018 ' de, özellikle yeni bir [hassas ve sınıflandırılmış bir bilgi sistemi](https://www.protectivesecurity.gov.au/information/sensitive-classified-information/Pages/default.aspx)olan koruyucu güvenlik ilkesi çerçevesinde (Pspf) yapılan değişiklikleri genel olarak duyurmuştur.

![Düzeltilen PSPF sınıflandırmaları](media/pspf-classifications.png)

PSPF altında çalışan tüm Avustralya kurumları ve organizasyonlar bu değişikliklerden etkilenir. Geçerli ıRAP/CCSL sertifikalarımızı etkileyen birincil değişiklik, geçerli olan kısıtlama Işaretlerini (DLCI 'Ler) kullanımdan kaldırmasıdır. RESMI: Hassas işaret, hassas bilgilerin korunması için kullanılan çeşitli DLCI 'Leri değiştirir. Bu değişiklik, tüm duyarlılık ve sınıflandırma düzeylerinde tüm resmi bilgilere uygulanabilen üç bilgi yönetimi işaretleyicisi de ortaya sunmuştur. KORUNAN sınıflandırma değişmeden kalır.

"Sınıflandırılmamış" ifadesi yeni sistemden kaldırılır ve "resmi olmayan" terimi resmi olmayan bilgilere uygulanır, ancak biçimsel bir işaret gerektirmez.

## <a name="choose-an-azure-region-for-official-sensitive-and-protected-workloads"></a>RESMI için bir Azure bölgesi seçin: Gizli ve KORUMALı iş yükleri

Azure RESMI: Hassas ve KORUMALı sertifikalı hizmetler dört Avustralya veri merkezi bölgesine dağıtılır: Avustralya Doğu, Avustralya Güney Doğu, Avustralya Orta ve Avustralya Orta 2. ACSC tarafından verilen sertifika raporu, bir hizmet varsa, KORUMALı verilerin Canberde Azure Kamu bölgelerine dağıtılmasını önerir. KORUNAN sertifikalı Azure hizmetleri hakkında daha fazla bilgi için bkz. [hizmet güven portalındaki Avustralya sayfası](https://aka.ms/au-irap).

>[!NOTE]
>Microsoft, tüm sensitivities ve sınıflandırmaların kamu verilerinin, özellikle kamu ihtiyaçları için tasarlandıkları ve işledikleri için Avustralya Orta ve Avustralya Orta 2 bölgelere dağıtılması önerilir.

Azure Avustralya bölgelerinin özel doğası hakkında daha fazla bilgi için bkz. [azure Avustralya orta bölgeleri](https://azure.microsoft.com/global-infrastructure/australia/).

## <a name="how-microsoft-separates-classified-and-official-data"></a>Microsoft 'un sınıflandırılan ve resmi verileri ayıran

Microsoft, Azure ve Office 365 ' i tüm veriler hassas veya sınıflandırıldığında, bu da bu yüksek çubuğa yönelik güvenlik denetimlerimize yükselten Avustralya 'da çalışır.

Azure 'un desteklediği altyapı birden çok sınıflandırmadaki verileri sunar. Müşteri verilerinin sınıflandırıldığı varsayılmaktadır, uygun denetimler yerinde olur. Microsoft, gizli sınıflandırılan bilgileri depolamak ve işlemek için PSPF gereksinimleriyle uyumlu olan hizmetlerimiz için bir temel güvenlik duruşunu benimsemiştir.

Müşterilerin Azure 'daki bir kiracının diğer kiracılardan risk altında olmadığını güvence altına almak için, Microsoft kapsamlı savunma denetimleri uygular.

Microsoft Azure platformunda uygulanan yeteneklerin ötesinde, müşteri tarafından yönetilen anahtarlarla şifreleme, iç içe sanallaştırma ve tam zamanında yönetim erişimi gibi ek müşteri yapılandırılabilir denetimleri, riski daha da azaltabilir. Canbera 'daki Azure Kamu Avustralya bölgelerinde, yalnızca Avustralya ve Yeni Zelanda kamu ve ulusal kritik altyapı kurumları için beyaz listeye yönelik bir işlem yapılabilir. Bu topluluk bulutu, ortak Kiracı riskleri açısından önemli olan kuruluşlar için ek güvence sağlar.

Microsoft Azure KORUMALı sertifika raporu bu denetimlerin, korunan sınıflandırılan verilerin depolanması ve işlenmesi için etkili olduğunu ve bunların yalıtımlarını onaylar.

## <a name="relevance-of-irapccsl-to-state-government-and-critical-infrastructure-providers"></a>IRAP/CCSL 'nin Eyalet Kamu ve kritik altyapı sağlayıcılarının ilgisi

Birçok eyalet kamu ve kritik altyapı sağlayıcısı, Federal Kamu gereksinimlerini güvenlik ilkeleri ve güvence çerçevesiyle birleştirir. Bu kuruluşlar, resmi, RESMI olarak da işler: Hassas ve büyük miktarda korunan, verileri Federal kamu ya da kendi hakkı ile etkileşimlerinden daha fazla olabilir.

Avustralya kamu, gittikçe daha fazla odaklanan ilke ve Avustralya 'nın güvenlik ve ekonomik kullanımını temel alan kamu dışı verilerin korunmasında yasaldır. Bu nedenle, Azure Avustralya bölgeleri ve CCSL sertifikası işbu sektörlerin tümüyle ilgilidir.

![Kritik altyapı kesimleri](media/nci-sectors.png)

Microsoft sertifikaları, Azure hizmetlerinin güvenlik korumalarının yerinde, kapsamlı ve resmi bir değerlendirmesi için tabi olduğunu ve bu tür yüksek oranda duyarlı verileri işlemek için onaylanmış olduğunu gösterir.

## <a name="location-and-control-of-microsoft-data-centres"></a>Microsoft veri Centres 'nin konumu ve denetimi

Veri merkezi konumunu ve verilerini işleyen bulut hizmetleri için sahipliği açıkça bildiğiniz kamu ve kritik altyapıda zorunlu bir gereksinimdir. Microsoft, bu konumlar ve sahiplik hakkında kapsamlı bilgiler sağlayan bir hiper ölçekli bulut sağlayıcısı olarak benzersizdir.

Microsoft 'un Azure Avustralya bölgeleri (Avustralya Orta ve Avustralya Orta 2), CDC veri Centres 'nin tesislerinde çalışır. CDC veri Centres 'nin sahipliği, Infratil 'ten% 48 sahiplik ile Avustralya, 'den% 48 sahiplik (yeni bir Zelanda tabanlı, Çift Avustralya ve Yeni Zelanda stok Exchange tarafından listelenen uzun süreli bir altyapı varlık Fonu) ve% 4 Avustralya yönetimi. 

CDC veri Centres 'nin yönetimi, daha sonra sahiplik ve denetim aktarımını kısıtlayan Avustralya kamu ile ilgili olarak sözleşme sektörüne sahiptir. Microsoft 'un CDC Vercentres iş ortaklığı aracılığıyla tedarik zinciri ve sahipliğinin bu saydamlığı, [kamu öncesi barındırma stratejisinin](https://www.dta.gov.au/our-projects/whole-government-hosting-strategy) Ilkelerine ve sertifikalı bir Sogeign veri merkezinin tanımına sahiptir.

## <a name="azure-services-that-are-included-in-the-current-ccsl-certification"></a>Geçerli CCSL sertifikacıya dahil edilen Azure hizmetleri

2017 Haziran 'da, verileri sınıflandırılmayan şekilde depolama ve işleme için ACSC sertifikalı 41 Azure hizmetleri: DLCı düzeyi. 2018 Nisan 'da, bu hizmetlerin 24 ' i korunan sınıflandırılan veriler için sertifikalandırıldı.

Avustralya 'daki Azure bölgelerimizde ACSC sertifikalı Azure hizmetlerinin kullanılabilirliği aşağıda verilmiştir (kalın olarak gösterilen hizmetler KORUMALı düzeyde sertifikalıdır).

|Azure Avustralya Orta bölgeleri|Bölgesel olmayan hizmetler ve diğer bölgeler|
|---|---|
|API Management, uygulama ağ geçidi, Uygulama Hizmetleri, **Otomasyon**, **Azure Portal**, **yedekleme**, **toplu işlem**, **Cloud Services**, Cosmos DB, Event Hubs, **ExpressRoute**, HDInsight, **Key Vault**, Load Balancer, günlük Analytics, **Multi-Factor Authentication**, Redis Cache, **Kaynak Yöneticisi**, **Service Bus**, **Service Fabric**, **Site Recovery**, **SQL veritabanı**, **depolama**, Traffic Manager, **sanal Makineler**, **sanal ağ**, **VPN Gateway**|**Azure Active Directory**, CDN, Veri Kataloğu, **içeri**aktarma, **Information Protection**, **ıot hub**, Machine Learning, Media Services, **Notification Hubs**, Power BI, **Zamanlayıcı**, **Güvenlik Merkezi**, Arama, Stream Analytics|
|

Microsoft, ıRAP/CCSL 'yi içeren tüm küresel, Kamu, sektör ve bölgesel uyumluluk ve değerlendirme işlemlerine yönelik tüm kapsam içi hizmetleri listeleyen [Microsoft Azure uyumluluğun genel görünümünü](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942/file/178110/44/Microsoft%20Azure%20Compliance%20Offerings.pdf) yayımlar.

## <a name="azure-service-not-listed-or-assessed-at-a-lower-level-than-needed"></a>Azure hizmeti gerekenden düşük bir düzeyde listelenmemiş veya değerlendirilmedi

Sertifikalı veya RESMI olarak sertifikalı hizmetler: Gizli, ancak KORUMALı düzeyi değil, korunan verileri barındıran bir çözümün parçası olarak veya bu hizmetlerin bir parçası olarak kullanılabilir:

- KORUMALı verileri depolamamakta veya işlenmiyor veya
- Bir risk değerlendirmesi tamamladınız ve korunan verileri kendiniz depolamak için hizmeti onayladınız.

RESMI verileri depolamak ve işlemek için CCSL 'ye dahil olmayan bir hizmeti kullanabilirsiniz, ancak söz konusu, bir bulut hizmeti sağlayıcısı ile bir sözleşmeyi girmeden veya yenilemenize başlamadan önce yaptığınız yazarken ACSC 'yi bilgilendirmenizi ister.

KORUMALı iş yükleri için bir kurum tarafından kullanılan tüm hizmetler, [dta güvenli bulut stratejisindeki](https://www.dta.gov.au/files/cloud-strategy/secure-cloud-strategy.pdf)Ronizve acenteli tarafından yönetilen IRAP değerlendirmesi işleminde belirtilen işlemler ile birlikte güvenlik değerlendirmesi ve onaylanmış olmalıdır.

![DTA güvenli bulut stratejisi sertifika süreci](media/certification.png)

Microsoft, müşterilerimizin Kamu kamu kullanımı için güvenli ve amacına uygun olmasını sağlamak amacıyla hizmetlerimizi sürekli değerlendirir. KORUNAN düzeyde CCSL üzerinde olmayan bir hizmetle ilgili yardıma ihtiyacınız varsa Microsoft ile iletişim kurun.

Microsoft, CCSL 'nin hem sınıflandırılmamış DLCı 'ler hem de KORUMALı sınıflandırmalar üzerinde sertifikalı bir dizi hizmete sahip olduğundan, ıSM, hizmetlerimizin en az iki yılda bir ıRAP değerlendirmesini gerçekleştirmemiz gerekir. Microsoft, dikkate alınması gereken ek hizmetler de dahil olmak üzere yıllık bir değerlendirme alır.

## <a name="certified-protected-gateway-in-azure"></a>Azure 'da sertifikalı KORUMALı ağ geçidi

Microsoft, ağ geçidi birleştirme programı altında izin verilen SIGs sayısıyla ilgili kısıtlamalar nedeniyle kamu sertifikalı güvenli Internet ağ geçidini (SıG) işlemez. Ancak, bir SıG 'ın beklenen ve gerekli özellikleri Microsoft Azure içinde yapılandırılabilir.

Azure hizmetleri 'nin KORUMALı sertifikası sayesinde ACSC, Azure 'a bağlanma ve güvenlik etki alanları arasında ağ kesimlemesini uygulama (örneğin, KORUMALı ve Internet arasında) için özel önerilere sahiptir. Bu öneriler ağ güvenlik grupları, güvenlik duvarları ve sanal özel ağların kullanımını içerir. ACSC, sanal ağ geçidi gerecinin kullanımını önerir. Azure 'da, ASD tarafından değerlendirilen ürünler listesinde fiziksel eşdeğeri olan veya ortak ölçütler koruma profillerine göre değerlendirilen ve ortak ölçüt portalında listelenen birkaç sanal gereç vardır. Bu ürünler, ASD tarafından ortak ölçüt tanıma düzenlemesi için bir imza olarak birbirini dışlar.

Microsoft, farklı güvenlik etki alanları arasındaki sınırı korumak için gerekli güvenlik işlevlerini sağlayan, birleştirildiğinde, sertifikalı bir SıG ile eşdeğer olan Azure tabanlı özellikleri uygulamaya yönelik yönergeler hazırladı. Bir dizi iş ortağı, bu yeteneklerin tasarımı ve uygulamasıyla ilgili yardımcı olabilir ve aynı şekilde birçok iş ortağı çözümü bulunabilir.

## <a name="security-clearances-and-citizenship-of-microsoft-support-personnel"></a>Microsoft destek personeline yönelik güvenlik clearances ve vatandaşlık

Microsoft, araçlarımızı, ekran denetimli ve eğitilen güvenlik personeli ile küresel olarak çalıştırır. Sidney ve Melbourne 'deki tesislere yönelik fiziksel erişimi olmayan personel, Avustralya kamu temel güvenlik clearances sahiptir. Avustralya Orta ve Avustralya Orta 2 bölgelerinde çalışanların minimum negatif (NV1) clearances (gızlı veriler için uygun) vardır. Bu tasfiye gereksinimleri, veri Centres çalışan Azure 'daki personelin büyük ölçüde güvenilir olduğu müşterilere ek güvence sağlar.

Microsoft, tam zamanında bir sistem ile erişim izni verilen ve rol tabanlı erişim denetimlerine göre yeterli yönetim olan sıfır erişimli bir erişim ilkesine sahiptir. Çoğu durumda yöneticilerimiz, hizmette sorun gidermek ve bakım yapmak için müşteri verilerine erişim veya ayrıcalıklar gerektirmez. Uzaktan yürütmeye yönelik görevlerin yüksek derece ve komut dosyası kullanımı, müşteri verilerine doğrudan erişim gereksinimini ortadan kaldırmış.

Attorney genel bölümü, Microsoft 'un Azure 'daki personel güvenlik ilkelerinin ve yordamlarının, ıNFOSEC-9 ' daki bilgi sağlama işlemleri için PSPF erişimi amacına uygun olduğunu doğruladı.

## <a name="store-international-traffic-of-arms-regulations-itar-or-export-administration-regulations-ear-data"></a>Bir uluslararası trafik düzenlemeleri (ıTAR) veya dışa aktarma yönetim düzenlemeleri (EAR) verilerini depolayın

Müşterilere dışa aktarma denetimli veriler için yükümlülüklerine yardımcı olan Azure teknik denetimleri, Azure 'da genel olarak aynıdır. Daha önemlisi, dışa aktarma denetimli veriler için resmi bir değerlendirme ve sertifika çerçevesi yoktur.

Azure Kamu ve Office 365 ABD kamu 'da savunma için, müşterilerin dışa aktarma denetimlerine tabi olan müşterilere yönelik ek sözleşme ve işlem ölçüleri ekledik. Bu ek sözleşme yan tümceleri ve Azure bölgelerinin garantili ABD Ulusal desteği ve yönetimi Avustralya için yerinde değildir.

Bu, Avustralya 'daki Azure 'ın ıTAR/EAR için kullanılamayacağınız anlamına gelmez, ancak dışa aktarma lisansınız aracılığıyla size uygulanan kısıtlamaları açıkça anlamanız gerekir. Ayrıca, bu verileri depolamak için Azure kullanmadan önce bu yükümlülüklerinizi karşılamak üzere ek korumalar uygulamanız gerekir. Örneğin, şunları yapmanız gerekebilir:

- Azure Active Directory için ülke yapısını bir öznitelik olarak derleyin.
- Şifreleme kurallarını veri üzerinde zorlamak ve yalnızca ABD ile sınırlamak ve dışa aktarma lisansına dahil edilen diğer tüm ülke koşullarını kullanmak için Azure Information Protection kullanın.
- Şirket içindeki tüm verileri bir müşteri anahtarı kullanarak Azure 'da depolamadan önce veya ıTAR verileri için kendi anahtarınızı basılı tutarak şifreleyin.

ITAR, resmi bir sertifika olmadığından, dışa aktarma lisansıyla ilişkili kısıtlamaların ve sınırlamaların ne olduğunu anlamanız gerekir. Daha sonra, bu gereksinimleri karşılamak için Azure 'da yeterli denetim olup olmadığı konusunda çalışabilirsiniz. Bu durumda, en iyi şekilde göz önünde bulundurmanız gereken sorunlardan biri, ihracat lisansında onaylanan bir ülke olmaması olabilecek mühendislerimiz tarafından erişim sağlar.

## <a name="next-steps"></a>Sonraki adımlar

 Azure Avustralya ile VPN bağlantısının ıSM uyumlu yapılandırılması ve uygulanması için bkz. [azure VPN Gateway](vpn-gateway.md).
