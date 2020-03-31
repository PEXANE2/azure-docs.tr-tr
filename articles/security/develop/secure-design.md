---
title: Microsoft Azure'da güvenli uygulamalar tasarla
description: Bu makalede, web uygulama projenizin gereksinimi ve tasarım aşamalarında göz önünde bulundurulması gereken en iyi uygulamalar tartışılmaktadır.
author: TerryLanfear
manager: barbkess
ms.author: terrylan
ms.date: 06/11/2019
ms.topic: article
ms.service: security
ms.subservice: security-develop
services: azure
ms.assetid: 521180dc-2cc9-43f1-ae87-2701de7ca6b8
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.openlocfilehash: 889897cfd4dc8714ae3aea556f0924c9dbcd7825
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78299423"
---
# <a name="design-secure-applications-on-azure"></a>Azure'da güvenli uygulamalar tasarla
Bu makalede, bulut için uygulamalar tasarlarken göz önünde bulundurulması gereken güvenlik etkinlikleri ve denetimleri sıyoruz. Microsoft [Güvenlik Geliştirme Yaşam Döngüsü'nün (SDL)](https://msdn.microsoft.com/library/windows/desktop/84aed186-1d75-4366-8e61-8d258746bopq.aspx) gereksinimleri ve tasarım aşamalarında göz önünde bulundurulması gereken güvenlik soruları ve kavramlarıile birlikte eğitim kaynakları ele alınır. Amaç, daha güvenli bir uygulama tasarlamak için kullanabileceğiniz etkinlikleri ve Azure hizmetlerini tanımlamanıza yardımcı olmaktır.

Aşağıdaki SDL aşamaları bu makalede ele alınmıştır:

- Eğitim
- Gereksinimler
- Tasarım

## <a name="training"></a>Eğitim
Bulut uygulamanızı geliştirmeye başlamadan önce Azure'daki güvenliği ve gizliliği anlamak için zaman ayırın. Bu adımı atarak, uygulamanızdaki yararlanılabilir güvenlik açıklarının sayısını ve önem derecesini azaltabilirsiniz. Sürekli değişen tehdit ortamına uygun şekilde tepki vermeye daha hazır olacaksınız.

Geliştiricilerin kullanabileceği Azure hizmetlerini ve Azure'daki en iyi güvenlik uygulamalarını tanımak için eğitim aşamasında aşağıdaki kaynakları kullanın:

  - [Geliştiricinin Azure kılavuzu,](https://azure.microsoft.com/campaigns/developer-guide/) Azure ile nasıl başa çıkabileceğinizi gösterir. Kılavuz, uygulamalarınızı çalıştırmak, verilerinizi depolamak, istihbarat oluşturmak, IoT uygulamaları oluşturmak ve çözümlerinizi daha verimli ve güvenli bir şekilde dağıtmak için hangi hizmetleri kullanabileceğinizi gösterir.

  - [Azure geliştiricileri için başlangıç kılavuzu,](../../guides/developer/azure-developer-guide.md) geliştirme gereksinimleri için Azure platformlarını kullanmaya başlamak isteyen geliştiriciler için temel bilgileri sağlar.

  - [SDK'lar ve araçlar](https://docs.microsoft.com/azure/index?pivot=sdkstools) Azure'da kullanılabilen araçları açıklar.

  - [Azure DevOps Hizmetleri](https://docs.microsoft.com/azure/devops/) geliştirme işbirliği araçları sağlar. Araçlar arasında yüksek performanslı boru hatları, ücretsiz Git depoları, yapılandırılabilir Kanban panoları ve kapsamlı otomatik ve bulut tabanlı yük testi yer almaktadır.
    [DevOps Kaynak Merkezi,](https://docs.microsoft.com/azure/devops/learn/) DevOps uygulamalarını, Git sürüm denetimini, çevik yöntemleri, Microsoft'ta DevOps ile nasıl çalıştığımızı ve kendi DevOps ilerlemenizi nasıl değerlendirebileceğinizi öğrenmek için kaynaklarımızı bir araya getirmektedir.

  - [Üretime geçmeden önce göz önünde bulundurulması gereken en iyi 5 güvenlik öğesi,](https://docs.microsoft.com/learn/modules/top-5-security-items-to-consider/index?WT.mc_id=Learn-Blog-tajanca) Azure'daki web uygulamalarınızın güvenliğini sağlamaya ve uygulamalarınızı en yaygın ve tehlikeli web uygulaması saldırılarına karşı nasıl koruyacağınızı gösterir.

  - [Azure için Secure DevOps Kit,](https://azsk.azurewebsites.net/index.html) kapsamlı otomasyon kullanan DevOps ekiplerinin kapsamlı Azure aboneliği ve kaynak güvenliği gereksinimlerini karşılayan komut dosyaları, araçlar, uzantılar ve otomasyonlardan oluşan bir koleksiyondur. Azure için Güvenli DevOps Kiti, güvenliği yerel DevOps iş akışlarınıza nasıl sorunsuz bir şekilde entegre edebileceğinizi gösterebilir. Kit, geliştiricilerin güvenli kod yazmalarına ve bulut uygulamalarının güvenli yapılandırmasını kodlama ve erken geliştirme aşamalarında sınamalarına yardımcı olabilecek güvenlik doğrulama testleri (SVTs) gibi araçlara hitap eder.

  - [Azure çözümleri için en iyi güvenlik uygulamaları,](https://azure.microsoft.com/resources/security-best-practices-for-azure-solutions) Azure'u kullanarak bulut çözümlerinizi tasarlarken, dağıtırken ve yönetirken kullanılacak en iyi güvenlik uygulamaları koleksiyonu sağlar.

## <a name="requirements"></a>Gereksinimler
Gereksinimler tanım aşaması, uygulamanızın ne olduğunu ve yayımlandığında ne yapacağını tanımlamada önemli bir adımdır. Gereksinimler aşaması, uygulamanızda oluşturacağınız güvenlik denetimleri hakkında düşünme zamanıdır. Bu aşamada, güvenli bir uygulama yayımlamanızı ve dağıtmanızı sağlamak için SDL boyunca gerçekleştireceğiniz adımları da başlatırsınız.

### <a name="consider-security-and-privacy-issues"></a>Güvenlik ve gizlilik sorunlarını göz önünde bulundurun
Bu aşama, temel güvenlik ve gizlilik konularını göz önünde bulundurmak için en iyi zamandır. Projenin başlangıcında kabul edilebilir güvenlik ve gizlilik düzeylerinin tanımlanması bir takıma yardımcı olur:

- Güvenlik sorunlarıyla ilişkili riskleri anlayın.
- Geliştirme sırasında güvenlik hatalarını belirleyin ve düzeltin.
- Tüm proje boyunca yerleşik güvenlik ve gizlilik düzeylerini uygulayın.

Uygulamanız için gereksinimleri yazarken, uygulamanızı ve verilerinizin güvenliğini sağlamaya yardımcı olabilecek güvenlik denetimlerini dikkate aldığınızdan emin olun.

### <a name="ask-security-questions"></a>Güvenlik soruları sorun
Güvenlik soruları gibi sorular sorun:

  - Uygulamam hassas veriler içeriyor mu?

  - Başvurum, [Federal Finans Kurumu Sınav Konseyi (FFIEC)](../blueprints/ffiec-analytics-overview.md) veya Ödeme Kartı Endüstrisi Veri Güvenliği [Standartları (PCI DSS)](../blueprints/pcidss-analytics-overview.md)gibi endüstri standartlarına ve uyumluluk programlarına uymamı gerektiren verileri toplar mı veya saklar mı?

  - Uygulamam, tek bir kişiyi tanımlamak, iletişim kurmak veya bulmak için kendi başına veya başka bilgilerle kullanılabilecek hassas kişisel veya müşteri verilerini toplar mı veya içeriyor mu?

  - Başvurum, bir kişinin tıbbi, eğitici, mali veya istihdam bilgilerine erişmek için kullanılabilecek verileri toplar veya içeriyor mu? Gereksinimler aşamasında verilerinizin duyarlılığını belirlemek, verilerinizi sınıflandırmanıza ve uygulamanız için kullanacağınız veri koruma yöntemini belirlemenize yardımcı olur.

  - Verilerim nerede ve nasıl saklanır? Uygulamanızın beklenmeyen değişiklikler (daha yavaş yanıt süreleri gibi) için kullandığı depolama hizmetlerini nasıl izleyeceğinizi düşünün. Daha ayrıntılı veri toplamak ve bir sorunu derinlemesine analiz etmek için günlüğe kaydetmeyi etkileyebilecek misiniz?

  - Başvurum kamuya açık olacak mı (internetüzerinden) veya yalnızca dahili olarak? Uygulamanız herkese açıksa, toplanan verileri yanlış şekilde kullanılmasına karşı nasıl korursunuz? Uygulamanız yalnızca dahili olarak kullanılabilse, kuruluşunuzdaki lerin uygulamaya kimlerin erişmesi gerektiğini ve ne kadar süreyle erişmesi gerektiğini göz önünde bulundurun.

  - Başvurunuzu tasarlamaya başlamadan önce kimlik modelinizi anlıyor musunuz? Kullanıcıların söyledikleri kişiler olduğunu ve bir kullanıcının ne yapmaya yetkili olduğunu nasıl belirleyeceksiniz?

  - Uygulamam hassas veya önemli görevleri yerine getiriyor mu (para transferi, kapıların açılması veya ilaç teslimi gibi)?
    Hassas bir görevi gerçekleştiren kullanıcının görevi gerçekleştirmeyetkisine sahip olduğunu nasıl doğruladığınızı ve kişinin kim olduğunu nasıl doğruladığınızı nasıl doğruladığınızı düşünün. Yetkilendirme (AuthZ), bir şey yapmak için doğrulanmış bir güvenlik ilkesine izin verme eylemidir. Kimlik doğrulama (AuthN), bir partiye meşru kimlik bilgileri için meydan okuma eylemidir.

  - Uygulamam, kullanıcıların dosya veya diğer verileri yüklemesine veya indirmesine izin vermek gibi riskli yazılım etkinlikleri gerçekletir mi? Uygulamanız riskli etkinlikler gerçekletirse, uygulamanızın kullanıcıları kötü amaçlı dosyaları veya verileri işlemekten nasıl koruyacağını göz önünde bulundurun.

### <a name="review-owasp-top-10"></a>OWASP top 10 hakkında yorum
[<span class="underline">OWASP Top 10 Uygulama Güvenlik Risklerini</span>](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project)gözden almayı düşünün.
OWASP Top 10, web uygulamaları için kritik güvenlik risklerini giderer.
Bu güvenlik risklerinin farkında olmak, uygulamanızdaki bu riskleri en aza indiren gereksinim ve tasarım kararları belirlemenize yardımcı olabilir.

İhlalleri önlemek için güvenlik denetimleri hakkında düşünmek önemlidir.
Ancak, bir ihlalin oluşacağını [da varsaymak](https://docs.microsoft.com/azure/devops/learn/devops-at-microsoft/security-in-devops) istiyorsunuz. Bir ihlalin güvenlikle ilgili bazı önemli soruları önceden yanıtlamaya yardımcı olduğunu varsayarsak, acil durumlarda yanıtlanması gerekmez:

  - Bir saldırıyı nasıl tespit edeceğim?

  - Bir saldırı veya ihlal olduğunda ne yapacağım?

  - Veri sızdırma veya kurcalama gibi saldırıdan nasıl kurtulacağım?

## <a name="design"></a>Tasarım

Tasarım aşaması, tasarım ve işlevsel özellikler için en iyi uygulamaları oluşturmak için çok önemlidir. Ayrıca, proje boyunca güvenlik ve gizlilik sorunlarını azaltmaya yardımcı olan risk çözümlemesi için de önemlidir.

Güvenlik gereksinimleriniz varsa ve güvenli tasarım kavramlarını kullandığınızda, güvenlik kusuru için fırsatları önleyebilir veya en aza indirebilirsiniz. Güvenlik hatası, uygulamanız serbest bırakıldıktan sonra bir kullanıcının kötü amaçlı veya beklenmeyen eylemler gerçekleştirmesine izin verebilecek uygulamanın tasarımında yapılan bir denetimdir.

Tasarım aşamasında, güvenliği katmanlar halinde nasıl uygulayabileceğinizi de düşünün; bir savunma düzeyi yeterli değildir. Bir saldırgan web uygulama güvenlik duvarınızı (WAF) geçerse ne olur? Bu saldırıya karşı savunmak için başka bir güvenlik kontrolü istiyorsunuz.

Bunu göz önünde bulundurarak, güvenli uygulamalar tasarlarken ele almanız gereken aşağıdaki güvenli tasarım kavramlarını ve güvenlik denetimlerini tartışıyoruz:

- Güvenli bir kodlama kitaplığı ve yazılım çerçevesi kullanın.
- Güvenlik açığı olan bileşenleri tarar.
- Uygulama tasarımı sırasında tehdit modellemesi kullanın.
- Saldırı yüzeyinizi küçültün.
- Birincil güvenlik alanı olarak bir kimlik ilkesi benimseyin.
- Önemli hareketler için yeniden kimlik doğrulaması gerektirir.
- Anahtarları, kimlik bilgilerini ve diğer sırları güvenli hale getirmek için önemli bir yönetim çözümü kullanın.
- Hassas verileri koruyun.
- Başarısız güvenlik önlemleri uygulayın.
- Hata ve özel durum işleme avantajlarından yararlanın.
- Günlüğe kaydetme ve uyarılamayı kullanın.

### <a name="use-a-secure-coding-library-and-a-software-framework"></a>Güvenli bir kodlama kitaplığı ve yazılım çerçevesi kullanma

Geliştirme için güvenli bir kodlama kitaplığı ve güvenliği katıştirmiş bir yazılım çerçevesi kullanın. Geliştiriciler, güvenlik denetimlerini sıfırdan geliştirmek yerine varolan, kanıtlanmış özellikleri (şifreleme, giriş sanitasyonu, çıktı kodlaması, anahtarlar veya bağlantı dizeleri ve güvenlik denetimi olarak kabul edilecek diğer her şeyi) kullanabilir. Bu, güvenlikle ilgili tasarım ve uygulama kusurlarına karşı koruma sağlar.

Çerçevenizin en son sürümünü ve çerçevede kullanılabilen tüm güvenlik özelliklerini kullandığınızdan emin olun. Microsoft, bulut uygulamaları sunmak için herhangi bir platform veya dil üzerinde çalışan tüm geliştiriciler için kapsamlı bir [geliştirme araçları kümesi](https://azure.microsoft.com/product-categories/developer-tools/) sunar. Çeşitli [SDK'lardan](https://azure.microsoft.com/downloads/)seçim yaparak seçtiğiniz dile göre kodlayabilirsiniz.
Gelişmiş hata ayıklama özelliklerine ve yerleşik Azure desteğine sahip tam özellikli tümleşik geliştirme ortamlarından (IME'ler) ve düzenleyicilerden yararlanabilirsiniz.

Microsoft, Azure'da uygulama geliştirmek için kullanabileceğiniz çeşitli [diller, çerçeveler ve araçlar](https://docs.microsoft.com/azure/index?pivot=sdkstools&panel=sdkstools-all) sunar. Bir örnek [.NET ve .NET Core geliştiricileri için](https://docs.microsoft.com/dotnet/azure/)Azure'dur. Sunduğumuz her dil ve çerçeve için hızlı başlangıçlar, öğreticiler ve API referansları ile hızlı başlamanıza yardımcı olur.

Azure, web sitelerini ve web uygulamalarını barındırmak için kullanabileceğiniz çeşitli hizmetler sunar. Bu hizmetler, ister .NET, .NET Core, Java, Ruby, Node.js, PHP veya Python olsun, en sevdiğiniz dilde geliştirmenize izin verebiliyor.
[Azure App Service Web Apps](../../app-service/overview.md) (Web Apps) bu hizmetlerden biridir.

Web Apps, Microsoft Azure'un gücünü uygulamanıza ekler. Güvenlik, yük dengeleme, otomatik ölçekleme ve otomatik yönetim içerir. Ayrıca, paket yönetimi, hazırlama ortamları, özel etki alanları, SSL/TLS sertifikaları ve Azure DevOps, GitHub, Docker Hub ve diğer kaynaklardan sürekli dağıtım gibi Web Apps'taki DevOps özelliklerinden de yararlanabilirsiniz.

Azure, web sitelerini ve web uygulamalarını barındırmak için kullanabileceğiniz başka hizmetler sunar. Çoğu senaryo için Web Apps en iyi seçenektir. Mikro hizmet mimarisi için [Azure Hizmet Kumaşı'nı](https://azure.microsoft.com/documentation/services/service-fabric)düşünün.
Kodlarınızın çalıştığı sanal makineler üzerinde daha fazla denetime sahip olmanız gerekiyorsa [Azure Sanal Makineler](https://azure.microsoft.com/documentation/services/virtual-machines/)’i düşünün.
Bu Azure hizmetleri arasında nasıl seçim yapabileceğiniz hakkında daha fazla bilgi için [Azure Uygulama Hizmeti, Sanal Makineler, Hizmet Kumaşı ve Bulut Hizmetleri karşılaştırmasına](/azure/architecture/guide/technology-choices/compute-decision-tree)bakın.

### <a name="apply-updates-to-components"></a>Güncelleştirmeleri bileşenlere uygulama

Güvenlik açıklarını önlemek için, hem istemci hem de sunucu tarafındaki bileşenlerinizi (örneğin, çerçeveler ve kitaplıklar) ve bunların güncelleştirmelere olan bağımlılıklarını sürekli olarak envantere almanız gerekir. Yeni güvenlik açıkları ve güncelleştirilmiş yazılım sürümleri sürekli olarak yayımlanır. Güncelleştirmeleri veya yapılandırma değişikliklerini kullandığınız kitaplıklara ve bileşenlere izlemek, triyajlamak ve uygulamak için sürekli bir planınız olduğundan emin olun.

Araç önerileri için [bilinen güvenlik açıklarına sahip bileşenleri kullanma](https://www.owasp.org/index.php/Top_10-2017_A9-Using_Components_with_Known_Vulnerabilities) yla ilgili Açık Web Uygulama Güvenliği Projesi [(OWASP)](https://www.owasp.org/index.php/Main_Page) sayfasına bakın. Ayrıca, kullandığınız bileşenlerle ilgili güvenlik açıkları için e-posta uyarılarına abone olabilirsiniz.

### <a name="use-threat-modeling-during-application-design"></a>Uygulama tasarımı sırasında tehdit modellemesi kullanma

Tehdit modelleme, işletmenize ve uygulamanıza yönelik olası güvenlik tehditlerini belirleme ve ardından uygun azaltıcı etkenlerin uygulanmasını sağlama işlemidir. SDL, olası sorunları çözmenin nispeten kolay ve uygun maliyetli olduğu durumlarda, ekiplerin tasarım aşamasında tehdit modellemesine katılmaları gerektiğini belirtir. Tasarım aşamasında tehdit modellemekullanarak büyük ölçüde geliştirme toplam maliyetini azaltabilir.

Tehdit modelleme işlemini kolaylaştırmak için, güvenlik dışı uzmanları göz önünde bulundurarak [SDL Tehdit Modelleme Aracı'nı](threat-modeling-tool.md) tasarladık. Bu araç, tehdit modelleri oluşturma ve analiz etme hakkında net kılavuzlar sağlayarak tehdit modellemeyi tüm geliştiriciler için kolaylaştırır.

Uygulama tasarımının modelleştirilmesi ve [STRIDE](https://docs.google.com/viewer?a=v&pid=sites&srcid=ZGVmYXVsdGRvbWFpbnxzZWN1cmVwcm9ncmFtbWluZ3xneDo0MTY1MmM0ZDI0ZjQ4ZDMy) tehditlerinin sayısallaşması-Sızdırma, Tahrifat, Reddetme, Bilgi Açıklama, Hizmet Reddi ve Ayrıcalık Yükselmesi—tüm güven sınırları boyunca tasarım hatalarını erkenden yakalamanın etkili bir yolunu kanıtlamıştır. Aşağıdaki tabloDA STRIDE tehditleri listeler ve Azure tarafından sağlanan özellikleri kullanan bazı örnek azaltıcı etkenler verir. Bu hafifletmeler her durumda işe yaramaz.

| Tehdit | Güvenlik özelliği | Potansiyel Azure platformu azaltma |
| ---------------------- | --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Kandırma               | Kimlik doğrulaması        | [HTTPS bağlantıları gerektirir.](https://docs.microsoft.com/aspnet/core/security/enforcing-ssl?view=aspnetcore-2.1&tabs=visual-studio) |
| İzinsiz Değişiklik              | Bütünlük             | SSL/TLS sertifikalarını doğrulayın. SSL/TLS kullanan uygulamalar, bağlandıkları varlıkların X.509 sertifikalarını tam olarak doğrulamalıdır. [x509 sertifikalarınızı yönetmek](../../key-vault/about-keys-secrets-and-certificates.md#key-vault-certificates)için Azure Anahtar Kasası sertifikalarını kullanın. |
| Tanımadığı            | İnkar etmeme       | Azure [izleme ve tanılamayı](https://docs.microsoft.com/azure/architecture/best-practices/monitoring)etkinleştirin.|
| Bilgileri Açıklama | Gizlilik       | Hassas verileri [istirahatte](../fundamentals/encryption-atrest.md) ve [aktarımsırasında](../fundamentals/data-encryption-best-practices.md#protect-data-in-transit)şifreleyin. |
| Hizmet Reddi      | Kullanılabilirlik          | Olası hizmet reddi koşulları için performans ölçümlerini izleyin. Bağlantı filtrelerini uygulayın. [Azure DDoS koruması,](../../virtual-network/ddos-protection-overview.md#next-steps)uygulama tasarımının en iyi uygulamalarıyla birleştiğinde, DDoS saldırılarına karşı savunma sağlar.|
| Ayrıcalık Yükseltme | Yetkilendirme         | Azure Etkin Dizin <span class="underline"> </span> [Ayrıcalıklı Kimlik Yönetimi'ni](../../active-directory/privileged-identity-management/pim-configure.md)kullanın.|

### <a name="reduce-your-attack-surface"></a>Saldırı yüzeyinizi azaltın

Saldırı yüzeyi, olası güvenlik açıklarının oluşabileceği toplam yüzeydir. Bu yazıda, bir uygulamanın saldırı yüzeyine odaklanıyoruz.
Odak noktası bir uygulamayı saldırılardan korumaktır. Saldırı yüzeyinizi en aza indirmenin basit ve hızlı bir yolu, kullanılmayan kaynakları ve kodu uygulamanızdan kaldırmaktır. Uygulamanız ne kadar küçükse, saldırı yüzeyiniz de o kadar küçük tür. Örneğin, kaldırın:

- Henüz yayımlamadığınız özellikler için kod.
- Destek kodunu hata ayıklama.
- Kullanılmayan veya amortismana niçin küçümsenmiş ağ arabirimleri ve protokolleri.
- Sanal makineler ve kullanmadığınız diğer kaynaklar.

Kaynaklarınızı düzenli olarak temizlemek ve kullanılmayan kodu kaldırmanızı sağlamak, kötü amaçlı aktörlerin saldırması için daha az fırsat olduğundan emin olmanın harika yollarıdır.

Saldırı yüzeyinizi azaltmanın daha ayrıntılı ve derinlemesine bir yolu, bir saldırı yüzeyi analizini tamamlamaktır. Saldırı yüzeyi çözümlemesi, bir sistemin güvenlik açıkları açısından gözden geçirilmesi ve sınanması gereken parçaların eşlenmesine yardımcı olur.

Bir saldırı yüzey analizinin amacı, geliştiricilerin ve güvenlik uzmanlarının uygulamanın hangi bölümlerinin saldırıya açık olduğunun farkında olması için bir uygulamadaki risk alanlarını anlamaktır. Ardından, bu potansiyeli en aza indirmenin, saldırı yüzeyinin ne zaman ve nasıl değiştiğini ve bunun risk açısından ne anlama geldiğini izlemenin yollarını bulabilirsiniz.

Saldırı yüzeyi analizi şunları belirlemenize yardımcı olur:

- Güvenlik açıklarını gözden geçirmeniz ve test etmeniz gereken işlevler ve sistem parçaları.
- Derinlemesine savunma koruması gerektiren yüksek riskli kod alanları (savunmanız gereken sistemin parçaları).
- Saldırı yüzeyini değiştirdiğinizde ve bir tehdit değerlendirmesini yenilemeniz gerektiğinde.

Saldırganların olası bir zayıf nokta veya güvenlik açığından yararlanma fırsatlarını azaltmak, uygulamanızın genel saldırı yüzeyini ayrıntılı bir şekilde analiz etmenizi gerektirir. Ayrıca, sistem hizmetlerine erişimi devre dışı bırakmayı veya kısıtlamayı, en az ayrıcalık ilkesini uygulamayı ve mümkün olan her yerde katmanlı savunma lar çalıştırmayı da içerir.

SDL'nin doğrulama aşamasında [bir saldırı yüzeyi incelemesi yürütmeyi](secure-develop.md#conduct-attack-surface-review) tartışıyoruz.

> [!NOTE]
> **Tehdit modellemesi ile saldırı yüzey analizi arasındaki fark nedir?**
Tehdit modelleme, uygulamanız için olası güvenlik tehditlerini belirleme ve tehditlere karşı uygun azaltmaların yerinde olmasını sağlama işlemidir. Saldırı yüzey analizi, saldırıya açık yüksek riskli kod alanlarını tanımlar. Uygulamanızın yüksek riskli alanlarını savunmanın yollarını bulmayı ve uygulamayı dağıtmadan önce bu kod alanlarını gözden geçirmeyi ve test etmeyi içerir.

### <a name="adopt-a-policy-of-identity-as-the-primary-security-perimeter"></a>Birincil güvenlik alanı olarak bir kimlik ilkesi benimsemek

Bulut uygulamaları tasarlarken, güvenlik çevre odağınızı ağ merkezli bir yaklaşımdan kimlik merkezli bir yaklaşıma genişletmeniz önemlidir. Tarihsel olarak, birincil şirket içi güvenlik çevresi bir kuruluşun ağıydı. Şirket içi güvenlik tasarımlarının çoğu ağı birincil güvenlik pivotu olarak kullanır. Bulut uygulamaları için, kimliği birincil güvenlik alanı olarak göz önünde bulundurarak daha iyi hizmet verilir.

Web uygulamaları geliştirmek için kimlik odaklı bir yaklaşım geliştirmek için yapabileceğiniz şeyler:

- Kullanıcılar için çok faktörlü kimlik doğrulamasını uygulayın.
- Güçlü kimlik doğrulama ve yetkilendirme platformları kullanın.
- En az ayrıcalık ilkesini uygulayın.
- Tam zamanında erişim uygulayın.

#### <a name="enforce-multi-factor-authentication-for-users"></a>Kullanıcılar için çok faktörlü kimlik doğrulamayı zorlama

İki faktörlü kimlik doğrulaması kullanın. İki faktörlü kimlik doğrulama, kullanıcı adı ve parola kimlik doğrulama türlerinin doğasında olan güvenlik zayıflıklarını önlediği için geçerli kimlik doğrulama ve yetkilendirme standardıdır. Azure yönetim arabirimlerine (Azure portalı/uzak PowerShell) ve müşteriye yönelik hizmetlere erişim, [Azure Çok Faktörlü Kimlik Doğrulaması'nı](../../active-directory/authentication/concept-mfa-howitworks.md)kullanacak şekilde tasarlanmalı ve yapılandırılmalıdır.

#### <a name="use-strong-authentication-and-authorization-platforms"></a>Güçlü kimlik doğrulama ve yetkilendirme platformları kullanma

Özel kod yerine platform tarafından sağlanan kimlik doğrulama ve yetkilendirme mekanizmalarını kullanın. Bunun nedeni, özel kimlik doğrulama kodu geliştirmenin hataya yatkın olmasıdır. Ticari kod (örneğin, Microsoft'tan) genellikle güvenlik için kapsamlı bir şekilde gözden geçirilir. [Azure Active Directory (Azure AD),](../../active-directory/fundamentals/active-directory-whatis.md) kimlik ve erişim yönetimi için Azure çözümüdür. Bu Azure AD araçları ve hizmetleri güvenli geliştirme ile ilgili yardımcı olur:

- [Azure AD kimlik platformu (geliştiriciler için Azure AD),](../../active-directory/develop/about-microsoft-identity-platform.md) geliştiricilerin kullanıcıları güvenli bir şekilde oturum açan uygulamalar oluşturmak için kullandıkları bir bulut kimlik hizmetidir. Azure AD, tek kiracılı, iş yeri (LOB) uygulamaları geliştiren geliştiricilere ve çok kiracılı uygulamalar geliştirmek isteyen geliştiricilere yardımcı olur. Temel oturum açmanın yanı sıra, Azure AD kullanılarak oluşturulmuş uygulamalar Microsoft API'lerini ve Azure AD platformunda oluşturulmuş özel API'leri arayabilir. Azure AD kimlik platformu, OAuth 2.0 ve OpenID Connect gibi endüstri standardı protokolleri destekler.

- [Azure Active Directory B2C (Azure AD B2C),](../../active-directory-b2c/index.yml) müşterilerin uygulamalarınızı kullanırken profillerini nasıl kaydolduklarını, kaydolduklarını ve yönetmelerini özelleştirmek ve denetlemek için kullanabileceğiniz bir kimlik yönetimi hizmetidir. Bu, diğerlerinin yanı sıra iOS, Android ve .NET için geliştirilen uygulamaları içerir. Azure AD B2C, müşteri kimliklerini korurken bu eylemleri sağlar.

#### <a name="apply-the-principle-of-least-privilege"></a>En az ayrıcalık ilkesini uygulayın

[En az ayrıcalık](https://en.wikipedia.org/wiki/Principle_of_least_privilege) kavramı, kullanıcılara işlerini yapmak için ihtiyaç duydukları erişim ve denetim düzeyini vermek anlamına gelir.

Bir yazılım geliştiricisinin etki alanı yöneticisi haklarına ihtiyacı var mı? Bir yönetici asistanının kişisel bilgisayarındaki yönetim denetimlerine erişmeleri gerekir mi? Yazılıma erişimi değerlendirmek de farklı değildir. Kullanıcılara uygulamanızda farklı yetenekler ve yetki vermek için [rol tabanlı erişim denetimi (RBAC)](../../role-based-access-control/overview.md) kullanırsanız, herkese her şeye erişim izni vermezsiniz. Her rol için gerekenlere erişimi sınırlayarak, bir güvenlik sorununun oluşma riskini sınırlarsınız.

Uygulamanızın erişim kalıpları boyunca [en az ayrıcalık](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/security-best-practices/implementing-least-privilege-administrative-models#in-applications) uyguladığından emin olun.

> [!NOTE]
>  En az ayrıcalık kurallarının yazılıma ve yazılımı oluşturan kişilere uygulanması gerekir. Yazılım geliştiricileri çok fazla erişim verilirse BT güvenliği için büyük bir risk olabilir. Bir geliştiricinin kötü niyetli olması veya çok fazla erişim hakkı varsa sonuçları ciddi olabilir. Geliştirme yaşam döngüsü boyunca geliştiricilere en az ayrıcalık kurallarının uygulanmasını öneririz.

#### <a name="implement-just-in-time-access"></a>Tam zamanında erişim uygulayın

Ayrıcalıkların maruz kalma süresini daha da azaltmak için *tam zamanında* (JIT) erişimi uygulayın. [Azure AD Ayrıcalıklı Kimlik Yönetimi'ni](../../active-directory/users-groups-roles/directory-admin-roles-secure.md#stage-3-build-visibility-and-take-full-control-of-admin-activity) şu şekilde kullanın:

- Kullanıcılara yalnızca JIT'ye gereksinim duydukları izinleri verin.
- Ayrıcalıkların otomatik olarak iptal edildiğine güvenarak kısaltılmış bir süre için roller atayın.

### <a name="require-re-authentication-for-important-transactions"></a>Önemli işlemler için yeniden kimlik doğrulaması gerektirme

[Site içi istek sahteciliği](https://docs.microsoft.com/aspnet/core/security/anti-request-forgery?view=aspnetcore-2.1) *(XSRF* veya *CSRF*olarak da bilinir), kötü amaçlı bir web uygulamasının istemci tarayıcı ile bu tarayıcıya güvenen bir web uygulaması arasındaki etkileşimi etkilediği web tarafından barındırılan uygulamalara yönelik bir saldırıdır. Web tarayıcıları her istekle birlikte bir web sitesine otomatik olarak bazı kimlik doğrulama belirteçleri gönderdiğinden, site ler arası istek sahteciliği saldırıları mümkündür.
Bu yararlanma biçimi, saldırı kullanıcının önceden kimlik doğrulaması oturumundan yararlandığından *tek tıklamayla saldırı* veya *oturum sürme* olarak da bilinir.

Bu tür saldırılara karşı savunmanın en iyi yolu, kullanıcıdan satın alma, hesap devre dışı bırakma veya parola değişikliği gibi her önemli işlemden önce yalnızca kullanıcının sağlayabileceği bir şey istemektir. Kullanıcıdan parolasını yeniden girmesini, bir captcha'yı tamamlamasını veya yalnızca kullanıcının sahip olabileceği gizli bir belirteci göndermesini isteyebilirsiniz. En yaygın yaklaşım gizli belirteçtir.

### <a name="use-a-key-management-solution-to-secure-keys-credentials-and-other-secrets"></a>Anahtarları, kimlik bilgilerini ve diğer sırları güvenli hale getirmek için önemli bir yönetim çözümü kullanın

Anahtarları ve kimlik bilgilerini kaybetmek sık karşılaşılan bir sorundur. Anahtarlarınızı ve kimlik bilgilerinizi kaybetmekten daha kötü olan tek şey yetkisiz bir tarafın onlara erişmesidir. Saldırganlar, GitHub gibi kod depolarında depolanan anahtarları ve sırları bulmak için otomatik ve manuel tekniklerden yararlanabilirler. Anahtarları ve sırları bu genel kod depolarında veya başka bir sunucuya koymayın.

Anahtarlarınızı, sertifikalarınızı, sırlarınızı ve bağlantı dizelerinizi her zaman anahtar yönetimi çözümüne koyun. Anahtarların ve sırların donanım güvenlik modüllerinde (HSM) depolandığı merkezi bir çözüm kullanabilirsiniz. Azure, [Azure Anahtar Kasası](../../key-vault/key-vault-overview.md)ile bulutta size bir HSM sağlar.

Key Vault *gizli*bir mağazadır: uygulama sırlarını saklamak için merkezi bir bulut hizmetidir. Key Vault, uygulama sırlarını tek bir merkezi konumda tutarak ve güvenli erişim, izin kontrolü ve erişim günlüğü sağlayarak gizli verilerinizi güvende tutar.

Sırlar tek tek *kasalarda*saklanır. Her kasanın erişimi denetlemek için kendi yapılandırma ve güvenlik ilkeleri vardır. Verilerinize BIR REST API'si veya çoğu programlama dili için kullanılabilen bir istemci SDK aracılığıyla ulaşabilirsiniz.

> [!IMPORTANT]
> Azure Key Vault, sunucu uygulamaları için yapılandırma sırlarını depolamak için tasarlanmıştır. Uygulama kullanıcılarına ait verileri depolamak için tasarlanmamıştır. Bu, hizmetin performans özelliklerine, API’sine ve maliyet modeline yansımıştır.
>
> Kullanıcı verileri, Saydam Veri Şifrelemesi (TDE) içeren bir Azure SQL Veritabanı örneğinde veya Azure Depolama Hizmeti Şifrelemesi kullanan bir depolama hesabında olduğu gibi başka bir yerde depolanmalıdır. Uygulamanız tarafından bu veri depolarına erişmek için kullanılan sırlar Azure Key Vault'ta saklanabilir.

### <a name="protect-sensitive-data"></a>Hassas verileri koruma

Verileri korumak, güvenlik stratejinizin önemli bir parçasıdır.
Verilerinizi sınıflandırmak ve veri koruma gereksinimlerinizi belirlemek, uygulamanızı veri güvenliği göz önünde bulundurularak tasarlamanıza yardımcı olur. Depolanan verileri duyarlılık ve iş etkisine göre sınıflandırmak (kategorilere ayırmak), geliştiricilerin verilerle ilişkili riskleri belirlemesini sağlar.

Veri biçimlerinizi tasarlarken geçerli tüm verileri hassas olarak etiketlendirin. Uygulamanın geçerli verileri hassas olarak ele diğinden emin olun. Bu uygulamalar, hassas verilerinizi korumanıza yardımcı olabilir:

- Şifreleme kullanın.
- Anahtarlar ve parolalar gibi zor kodlama sırlarından kaçının.
- Erişim denetimlerinin ve denetimin yerinde olduğundan emin olun.

#### <a name="use-encryption"></a>Şifreleme kullanma

Verileri korumak, güvenlik stratejinizin önemli bir parçası olmalıdır.
Verileriniz bir veritabanında depolanıyorsa veya konumlar arasında ileri ve geri hareket ederse, [veri](../fundamentals/encryption-atrest.md) şifrelemesini (veritabanındayken) ve [aktarım sırasında](../fundamentals/data-encryption-best-practices.md#protect-data-in-transit) (kullanıcıya, veritabanına, API'ye veya hizmet bitiş noktasına giderken) kullanın. Veri alışverişi için her zaman SSL/TLS protokollerini kullanmanızı öneririz. Şifreleme için TLS'nin en son sürümünü kullandığınızdan emin olun (şu anda bu sürüm 1.2).

#### <a name="avoid-hard-coding"></a>Sert kodlamadan kaçının

Bazı şeyler yazılımınızda hiçbir zaman sabit kodlanmış olmamalıdır. Bazı örnekler ev sahibi adları veya IP adresleri, URL'ler, e-posta adresleri, kullanıcı adları, parolalar, depolama hesabı anahtarları ve diğer şifreleme anahtarlarıdır. Kodunuzda kodlama bölümleri de dahil olmak üzere kodunuzda kodlandırılabilen veya kodlanabilen ler ile ilgili gereksinimleri uygulamayı düşünün.

Kodunuza yorum koyduğunuzda, hassas bilgileri kaydetmediğinizden emin olun. Buna e-posta adresiniz, parolalarınız, bağlantı dizeleriniz, uygulamanız hakkında yalnızca kuruluşunuzdaki biri tarafından bilinen bilgiler ve saldırgana uygulamanıza veya kuruluşunuza saldırmada avantaj sağlayacak her şey dahildir. .

Temel olarak, geliştirme projenizdeki her şeyin dağıtıldığında herkesin bilgisi dahilinde olacağını varsayalım. Projeye her türlü hassas verileri dahil etmekten kaçının.

Daha önce Azure [Key Vault'u](../../key-vault/key-vault-overview.md)tartıştık. Anahtar Kasası'nı kullanarak anahtarları ve parolaları zor kodlamak yerine parolalar gibi sırları saklayabilirsiniz. Azure kaynakları için yönetilen kimliklerle birlikte Key Vault'u kullandığınızda, Azure web uygulamanız kaynak denetiminizde veya yapılandırmanızda herhangi bir sır depolamadan gizli yapılandırma değerlerine kolayca ve güvenli bir şekilde erişebilir. Daha fazla bilgi edinmek için [Azure Key Vault ile sunucu uygulamalarınızdaki sırları yönet'e](https://docs.microsoft.com/learn/modules/manage-secrets-with-azure-key-vault/)bakın.

### <a name="implement-fail-safe-measures"></a>Arıza güvenli önlemler uygulayın

Uygulamanız yürütme sırasında oluşan [hataları](https://docs.microsoft.com/dotnet/standard/exceptions/) tutarlı bir şekilde işleyebilmelidir. Uygulama tüm hataları yakalamak ve ya güvenli ya da kapalı başarısız olmalıdır.

Ayrıca, hataların şüpheli veya kötü amaçlı etkinliği tanımlamak için yeterli kullanıcı bağlamıyla günlüğe kaydolduğundan da emin olmalısınız. Günlükler, gecikmiş adli analize izin vermek için yeterli bir süre saklanmalıdır. Günlükler, günlük yönetimi çözümü tarafından kolayca tüketilebilen bir biçimde olmalıdır. Güvenlikle ilgili hatalar için uyarıların tetiklendirildiğından emin olun. Yetersiz günlüğe kaydetme ve izleme, saldırganların sistemlere daha fazla saldırmasına ve kalıcılığını korumasına olanak tanır.

### <a name="take-advantage-of-error-and-exception-handling"></a>Hata ve özel durum işleme avantajlarından yararlanın

Doğru hata ve [özel durum işleme](https://docs.microsoft.com/dotnet/standard/exceptions/best-practices-for-exceptions) nin uygulanması savunma kodlamasının önemli bir parçasıdır. Hata ve özel durum işleme, bir sistemi güvenilir ve güvenli hale getirmek için çok önemlidir. Hata işleme hatalarını zıddlatan saldırganlara bilgi sızdırma ve saldırganların platformunuz ve tasarımınız hakkında daha fazla bilgi edinmelerine yardımcı olmak gibi farklı türde güvenlik açıklarına neden olabilir.

Sağlamak:

- Kodda yinelenen [try/catch bloklarından](https://docs.microsoft.com/dotnet/standard/exceptions/how-to-use-the-try-catch-block-to-catch-exceptions) kaçınmak için özel durumları merkezi bir şekilde işlersiniz.

- Tüm beklenmeyen davranışlar uygulama içinde işlenir.

- Kullanıcılara görüntülenen iletiler kritik verileri sızdırmaz, ancak sorunu açıklamak için yeterli bilgi sağlar.

- İstisnalar günlüğe kaydedilir ve adli tıp veya olay müdahale ekiplerinin araştırmasına yetecek kadar bilgi sağlarlar.

[Azure Logic Apps,](../../logic-apps/logic-apps-overview.md) bağımlı sistemlerden kaynaklanan [hataları ve özel durumları işlemek](../../logic-apps/logic-apps-exception-handling.md) için birinci sınıf bir deneyim sağlar. Uygulamalar, veriler, sistemler ve hizmetleri işletmeler ve kuruluşlar arasında tümleştiren görevleri ve işlemleri otomatikleştirmek için iş akışları oluşturmak için Logic Apps'ı kullanabilirsiniz.

### <a name="use-logging-and-alerting"></a>Günlük ve uyarı kullanma

Güvenlik soruşturmaları için güvenlik sorunlarınızı [günlüğe kaydedin](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1) ve insanların sorunları zamanında bilmesini sağlamak için sorunlarla ilgili uyarıları tetikler. Tüm bileşenlerde denetim ve günlüğe kaydetmeyi etkinleştirin. Denetim günlükleri kullanıcı bağlamını yakalamalı ve tüm önemli olayları tanımlamalıdır.

Bir kullanıcının sitenize gönderdiği hassas verileri günlüğe kaydetmediğinizi kontrol edin. Hassas verilere örnek olarak şunlar verilebilir:

- Kullanıcı kimlik bilgileri
- Sosyal Güvenlik numaraları veya diğer tanımlayıcı bilgiler
- Kredi kartı numaraları veya diğer finansal bilgiler
- Sağlık bilgileri
- Şifrelenmiş bilgilerin şifresini çözmek için kullanılabilecek özel anahtarlar veya diğer veriler
- Uygulamaya daha etkili bir şekilde saldırmak için kullanılabilecek sistem veya uygulama bilgileri

Uygulamanın başarılı ve başarısız kullanıcı girişleri, parola sıfırlama, parola değişiklikleri, hesap kilitleme ve kullanıcı kaydı gibi kullanıcı yönetimi olaylarını izlediğinden emin olun. Bu olaylar için günlüğe kaydetme, şüpheli olabilecek davranışları algılamanıza ve bunlara tepki vermenize yardımcı olur. Ayrıca, uygulamaya kimlerin eriştiği gibi operasyon verilerini toplamanıza da olanak tanır.

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelerde, güvenli uygulamalar geliştirmenize ve dağıtmanıza yardımcı olabilecek güvenlik denetimleri ve etkinlikleri öneririz.

- [Güvenli uygulamalar geliştirme](secure-develop.md)
- [Güvenli uygulamaları dağıtma](secure-deploy.md)
