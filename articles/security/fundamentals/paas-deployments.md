---
title: Güvenli PaaS dağıtımları için en iyi uygulamalar - Microsoft Azure
description: Azure'da güvenli bulut uygulamaları tasarlamak, oluşturmak ve yönetmek için en iyi uygulamaları öğrenin ve PaaS ile diğer bulut hizmeti modellerinin güvenlik avantajlarını anlayın.
services: security
documentationcenter: na
author: TerryLanfear
manager: barbkess
editor: techlake
ms.assetid: ''
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/28/2019
ms.author: terrylan
ms.openlocfilehash: 9adbe7b03283a00f78222ffdc77dca7aaadcbda0
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81461710"
---
# <a name="securing-paas-deployments"></a>PaaS dağıtımlarının güvenliğini sağlama

Bu makalede, size yardımcı olan bilgiler sağlar:

- Uygulamaları bulutta barındırmanın güvenlik avantajlarını anlama
- Platformun güvenlik avantajlarını hizmet (PaaS) ile diğer bulut hizmeti modelleri olarak değerlendirin
- Güvenlik odağınızı ağ merkezli bir ağ dan kimlik merkezli çevre güvenlik yaklaşımına değiştirme
- Genel PaaS güvenlik en iyi uygulama önerilerini uygulayın

[Azure'da güvenli uygulamalar geliştirmek,](abstract-develop-secure-apps.md) bulut uygulamaları geliştirirken yazılım geliştirme yaşam döngüsünün her aşamasında göz önünde bulundurmanız gereken güvenlik soruları ve denetimleri için genel bir kılavuzdur.

## <a name="cloud-security-advantages"></a>Bulut güvenliği avantajları
Siz ve Microsoft arasındaki [sorumluluk bölümünü](shared-responsibility.md) anlamak önemlidir. Şirket içinde, tüm yığının sahibisiniz, ancak buluta geçerken bazı sorumluluklar Microsoft'a aktarılır.

[Bulutta olmanın güvenlik avantajları](shared-responsibility.md#cloud-security-advantages)vardır. Şirket içi bir ortamda, kuruluşların karşılanmamış sorumlulukları ve güvenliğe yatırım yapmak için kullanabileceği sınırlı kaynaklara sahip olması, saldırganların tüm katmanlarda güvenlik açıklarından yararlanabildiği bir ortam oluşturur.

Kuruluşlar, bir sağlayıcının bulut tabanlı güvenlik özelliklerini ve bulut zekasını kullanarak tehdit algılama ve yanıt sürelerini geliştirebilir.  Kuruluşlar, sorumluluklarını bulut sağlayıcısına kaydırarak, güvenlik kaynaklarını ve bütçeyi diğer iş önceliklerine göre yeniden tahsis etmelerini sağlayan daha fazla güvenlik kapsamı elde edebilir.

## <a name="security-advantages-of-a-paas-cloud-service-model"></a>PaaS bulut hizmeti modelinin güvenlik avantajları
Azure PaaS dağıtımının şirket içi karşı güvenlik avantajlarına bakalım.

![PaaS'ın güvenlik avantajları](./media/paas-deployments/advantages-of-paas.png)

Microsoft, yığının en altından başlayarak ortak riskleri ve sorumlulukları azaltır. Microsoft bulutu Microsoft tarafından sürekli olarak izlendiği için saldırmak zordur. Bir saldırganın Microsoft bulutu hedef olarak takip etmesi mantıklı değildir. Saldırganın çok parası ve kaynağı yoksa, saldırganın başka bir hedefe geçme olasılığı yüksektir.  

Yığının ortasında, PaaS dağıtımı ile şirket içi arasında hiçbir fark yoktur. Uygulama katmanında, hesap ve erişim yönetimi katmanında benzer riskler esahipsiniz. Bu makalenin sonraki adımlar bölümünde, bu riskleri ortadan kaldırmak veya en aza indirmek için en iyi uygulamalar için size rehberlik edecektir.

Yığının en üstünde, veri yönetimi ve hak yönetimi, anahtar yönetimi tarafından azaltılabilir bir risk alırsınız. (Anahtar yönetimi en iyi uygulamalarda ele alınmıştır.) Anahtar yönetimi ek bir sorumluluk olsa da, kaynakları anahtar yönetimine kaydırabilmek için artık yönetmek zorunda olmadığınız bir PaaS dağıtımında alanlarınız vardır.

Azure platformu, çeşitli ağ tabanlı teknolojileri kullanarak güçlü DDoS koruması da sağlar. Ancak, ağ tabanlı DDoS koruma yöntemlerinin her türlü sýnýrlarý, baýlanti ve veri merkezi bazýnda sýnýrlarýna sahiptir. Büyük DDoS saldırılarının etkisini önlemeye yardımcı olmak için, Azure'un DDoS saldırılarına karşı savunmak için hızlı ve otomatik olarak ölçeklendirme yapmanızı sağlayan temel bulut yeteneğinden yararlanabilirsiniz. Önerilen uygulamalar makalelerinde bunu nasıl yapabileceğiniz konusunda daha fazla ayrıntıya gireceğiz.

## <a name="modernizing-the-defenders-mindset"></a>Savunucu zihniyetinin modernizasyonu
PaaS dağıtımları ile güvenlik için genel yaklaşım bir değişiklik geliyor. Her şeyi kendiniz kontrol etme gereksiniminden microsoft ile sorumluluğu paylaşmaya geçersiniz.

PaaS ve geleneksel şirket içi dağıtımlar arasındaki bir diğer önemli fark, birincil güvenlik çevresini tanımlayan yeni bir görünümdür. Tarihsel olarak, birincil şirket içi güvenlik alanı ağınızdır ve şirket içi güvenlik tasarımlarının çoğu ağı birincil güvenlik pivotu olarak kullanır. PaaS dağıtımları için, kimliğin birincil güvenlik alanı olduğunu göz önünde bulundurarak size daha iyi hizmet verilir.

## <a name="adopt-a-policy-of-identity-as-the-primary-security-perimeter"></a>Birincil güvenlik alanı olarak bir kimlik ilkesi benimsemek
Bulut bilgi işlemin beş temel özelliğinden biri, ağ merkezli düşünmeyi daha az alakalı kılan geniş ağ erişimidir. Bulut bilgi işlemin büyük bir kısmının amacı, kullanıcıların konumlarından bağımsız olarak kaynaklara erişmesine izin vermektir. Çoğu kullanıcı için, konumları Internet'te bir yerde olacak.

Aşağıdaki şekil, güvenlik çemberinin ağ çevresinden kimlik çevresine nasıl evrildiğini gösterir. Güvenlik, ağınızı savunmaktan çok, verilerinizi savunmanın yanı sıra uygulamalarınızın ve kullanıcılarınızın güvenliğini yönetme konusunda daha fazla bilgi edinirken olur. Önemli fark, güvenliği şirketiniz için önemli olana daha da yakınlaştırmak istemenizdir.

![Yeni güvenlik çemberi olarak kimlik](./media/paas-deployments/identity-perimeter.png)

Başlangıçta, Azure PaaS hizmetleri (örneğin, web rolleri ve Azure SQL) çok az veya hiç geleneksel ağ çevre savunması sağladı. Öğenin amacının Internet'e (web rolü) maruz kalmak olduğu ve kimlik doğrulamasının yeni çevreyi (örneğin, BLOB veya Azure SQL) sağladığı anlaşılmıştır.

Modern güvenlik uygulamaları, düşmanın ağ çevresini ihlal ettiğini varsayıyor. Bu nedenle, modern savunma uygulamaları kimlik taşındı. Kuruluşlar, güçlü kimlik doğrulama ve yetkilendirme hijyeni (en iyi uygulamalar) içeren kimlik tabanlı bir güvenlik çemberi oluşturmalıdır.

Ağ çevresi için ilkeler ve desenler yıllardır mevcuttur. Buna karşılık, endüstri birincil güvenlik alanı olarak kimlik kullanma konusunda nispeten daha az deneyime sahiptir. Bununla birlikte, alanında kanıtlanmış ve hemen hemen tüm PaaS hizmetleri için geçerli bazı genel öneriler sağlamak için yeterli deneyim birikmiş olduğunu söyledi.

Aşağıda kimlik çevresini yönetmek için en iyi uygulamalar veremistir.

**En iyi uygulama**: PaaS dağıtımınızı güvence altına almak için anahtarlarınızı ve kimlik bilgilerinizi güvence altına alanın.   
**Ayrıntı**: Anahtar ve kimlik bilgilerini kaybetmek sık karşılaşılan bir sorundur. Anahtarların ve sırların donanım güvenlik modüllerinde (HSM) depolanabileceği merkezi bir çözüm kullanabilirsiniz. [Azure Key Vault,](../../key-vault/general/overview.md) HSM'ler tarafından korunan anahtarları kullanarak kimlik doğrulama anahtarlarını, depolama hesap anahtarlarını, veri şifreleme anahtarlarını, .pfx dosyalarını ve şifreleri şifreleyerek anahtarlarınızı ve sırlarınızı korur.

**En iyi uygulama**: Kaynak koduveya GitHub kimlik bilgileri ve diğer sırları koymayın.   
**Ayrıntı**: Anahtarlarınızı ve kimlik bilgilerinizi kaybetmekten daha kötü olan tek şey, yetkisiz bir tarafın bunlara erişmehakkı sağlamasıdır. Saldırganlar, GitHub gibi kod depolarında depolanan anahtarları ve sırları bulmak için bot teknolojilerinden yararlanabilirler. Bu genel kod depolarında anahtar ve sırları koymayın.

**En iyi uygulama**: Bu VM'leri uzaktan doğrudan yönetmenize olanak tanıyan bir yönetim arabirimi kullanarak hibrit PaaS ve IaaS hizmetlerinde VM yönetim arayüzlerinizi koruyun.   
**Detay**: [SSH,](https://en.wikipedia.org/wiki/Secure_Shell) [RDP](https://support.microsoft.com/kb/186607)ve [PowerShell remoting](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.core/enable-psremoting) gibi uzaktan yönetim protokolleri kullanılabilir. Genel olarak, Internet'ten VM'lere doğrudan uzaktan erişime izin vermemenizi öneririz.

Mümkünse, azure sanal ağında sanal özel ağları kullanmak gibi alternatif yaklaşımlar kullanın. Alternatif yaklaşımlar kullanılamıyorsa, karmaşık parolalar ve iki faktörlü kimlik doğrulama [(Azure Çok Faktörlü Kimlik Doğrulaması](/azure/active-directory/authentication/multi-factor-authentication)gibi) kullandığınızdan emin olun.

**En iyi uygulama**: Güçlü kimlik doğrulama ve yetkilendirme platformları kullanın.   
**Ayrıntı**: Azure AD'de özel kullanıcı mağazaları yerine federe kimlikler kullanın. Federe kimlikler kullandığınızda, platform tabanlı bir yaklaşımdan yararlanır ve yetkili kimliklerin yönetimini ortaklarınıza devretmiş olursunuz. Çalışanların işine son verildiğinde ve bilgilerin birden fazla kimlik ve yetkilendirme sistemi aracılığıyla yansıtılması gerektiğinde federe kimlik yaklaşımı özellikle önemlidir.

Özel kod yerine platform tarafından sağlanan kimlik doğrulama ve yetkilendirme mekanizmalarını kullanın. Bunun nedeni, özel kimlik doğrulama kodu geliştirmehata eğilimli olabilir. Geliştiricilerinizin çoğu güvenlik uzmanı değildir ve kimlik doğrulama ve yetkilendirmedeki en son gelişmelerin farkında olma olasılığı düşüktür. Ticari kod (örneğin, Microsoft'tan) genellikle kapsamlı güvenlik gözden geçirilir.

İki faktörlü kimlik doğrulaması kullanın. İki faktörlü kimlik doğrulama, kullanıcı adı ve parola kimlik doğrulama türlerinin doğasında bulunan güvenlik zayıflıklarını önlediği için geçerli kimlik doğrulama ve yetkilendirme standardıdır. Hem Azure yönetimi (portal/uzak PowerShell) arabirimlerine hem de müşteriye yönelik hizmetlere [erişim, Azure Çok Faktörlü Kimlik Doğrulaması'nı](/azure/active-directory/authentication/multi-factor-authentication)kullanacak şekilde tasarlanmalı ve yapılandırılmalıdır.

OAuth2 ve Kerberos gibi standart kimlik doğrulama protokollerini kullanın. Bu protokoller kapsamlı bir şekilde gözden geçirilmiştir ve büyük olasılıkla kimlik doğrulama ve yetkilendirme için platform kitaplıklarınızın bir parçası olarak uygulanmaktadır.

## <a name="use-threat-modeling-during-application-design"></a>Uygulama tasarımı sırasında tehdit modellemesi kullanma
Microsoft [Güvenlik Geliştirme Yaşam Döngüsü,](https://www.microsoft.com/en-us/sdl) ekiplerin tasarım aşamasında tehdit modelleme adı verilen bir sürece dahil olması gerektiğini belirtir. Bu işlemi kolaylaştırmak için, Microsoft [SDL Tehdit Modelleme Aracı](/azure/security/azure-security-threat-modeling-tool)oluşturdu. Uygulama tasarımının modelilmesi ve [STRIDE](https://docs.google.com/viewer?a=v&pid=sites&srcid=ZGVmYXVsdGRvbWFpbnxzZWN1cmVwcm9ncmFtbWluZ3xneDo0MTY1MmM0ZDI0ZjQ4ZDMy) tehditlerinin tüm güven sınırları içinde sayısalanın tasarım hatalarını erkenden yakalayabilir.

Aşağıdaki tabloDA STRIDE tehditleri listeler ve Azure özelliklerini kullanan bazı örnek azaltıcı etkenler verir. Bu hafifletmeler her durumda işe yaramaz.

| Tehdit | Güvenlik özelliği | Olası Azure platformu azaltımları |
| --- | --- | --- |
| Kandırma | Kimlik Doğrulaması | HTTPS bağlantıları gerektirir. |
| İzinsiz Değişiklik | Bütünlük | TLS/SSL sertifikalarını doğrulayın. |
| Tanımadığı | İnkar etmeme | Azure [izleme ve tanılamayı](/azure/architecture/best-practices/monitoring)etkinleştirin. |
| Bilgi nin açıklanması | Gizlilik | [Hizmet sertifikalarını](/rest/api/appservice/certificates)kullanarak hassas verileri istirahatte şifreleyin. |
| Hizmet reddi | Kullanılabilirlik | Olası hizmet reddi koşulları için performans ölçümlerini izleyin. Bağlantı filtrelerini uygulayın. |
| Ayrıcalık yükselmesi | Yetkilendirme | [Ayrıcalıklı Kimlik Yönetimi'ni](/azure/active-directory/privileged-identity-management/subscription-requirements)kullanın. |

## <a name="develop-on-azure-app-service"></a>Azure Uygulama Hizmetinde Geliştirin
[Azure Uygulama Hizmeti,](/azure/app-service/overview) herhangi bir platform veya cihaz için web ve mobil uygulamalar oluşturmanıza ve her yerde, bulutta veya şirket içinde verilere bağlanmanıza olanak tanıyan bir PaaS teklifidir. Uygulama Hizmeti, daha önce Azure Web Siteleri ve Azure Mobil Hizmetleri olarak ayrı olarak teslim edilen web ve mobil özellikleri içerir. Ayrıca iş süreçlerini otomatikleştirmek ve bulut API'leri barındırmak için yeni özellikler içerir. Tek bir entegre hizmet olarak, App Service web, mobil ve tümleştirme senaryolarına zengin bir yetenek kümesi getirir.

Aşağıda Uygulama Hizmetini kullanmak için en iyi uygulamalar veremeyiz.

**En iyi uygulama**: [Azure Active Directory ile kimlik doğrulaması](/azure/app-service/overview-authentication-authorization)yapın.   
**Ayrıntı**: App Service, kimlik sağlayıcınız için bir OAuth 2.0 hizmeti sağlar. OAuth 2.0, web uygulamaları, masaüstü uygulamaları ve cep telefonları için özel yetkilendirme akışları sağlarken istemci geliştirici basitliğine odaklanır. Azure AD, mobil ve web uygulamalarına erişimi yetkilendirmenizi sağlamak için OAuth 2.0'ı kullanır.

**En iyi uygulama**: Bilinmesi gerekene ve en az ayrıcalık lı güvenlik ilkelerine göre erişimi kısıtlayın.   
**Ayrıntı**: Veri erişimi için güvenlik ilkelerini uygulamak isteyen kuruluşlar için erişimi kısıtlamak zorunludur. RBAC'ı, belirli bir kapsamdaki kullanıcılara, gruplara ve uygulamalara izin atamak için kullanabilirsiniz. Kullanıcılara uygulamalara erişim izni verme hakkında daha fazla bilgi edinmek için erişim [yönetimine başlayın'](/azure/role-based-access-control/overview)a bakın.

**En iyi uygulama**: Anahtarlarınızı koruyun.   
**Ayrıntı**: Azure Key Vault, bulut uygulamalarının ve hizmetlerinin kullandığı şifreleme anahtarlarının ve sırların korunmasına yardımcı olur. Key Vault ile anahtarları ve sırları (kimlik doğrulama anahtarları, depolama hesap anahtarları, veri şifreleme anahtarları gibi) şifreleyebilirsiniz. PFX dosyaları ve parolaları) donanım güvenlik modülleri (HSM) tarafından korunan anahtarları kullanarak. Ek güvenlik için HSM'lerde anahtarları içeri aktarabilir veya oluşturabilirsiniz. Daha fazla bilgi edinmek için [Azure Key Vault'a](/azure/key-vault/key-vault-overview) bakın. TLS sertifikalarınızı otomatik yenileme ile yönetmek için Key Vault'u da kullanabilirsiniz.

**En iyi yöntem**: Gelen kaynak IP adreslerini kısıtlayın.   
**Ayrıntı**: [Uygulama Hizmet Ortamı,](/azure/app-service/environment/intro) gelen kaynak IP adreslerini ağ güvenlik grupları aracılığıyla kısıtlamanıza yardımcı olan bir sanal ağ tümleştirme özelliğine sahiptir. Sanal ağlar, Azure kaynaklarını erişimi kontrol ettiğiniz internet olmayan, rahatsız edilebilen bir ağa yerleştirmenize olanak tanır. Daha fazla bilgi için bkz. [uygulamanızı bir Azure sanal ağıyla tümleştirin.](/azure/app-service/web-sites-integrate-with-vnet)

**En iyi uygulama**: Uygulama Hizmeti ortamlarınızın güvenlik durumunu izleyin.   
**Ayrıntı**: Uygulama Hizmeti ortamlarınızı izlemek için Azure Güvenlik Merkezi'ni kullanın. Güvenlik Merkezi olası güvenlik açıklarını tanımladığında, gerekli denetimleri yapılandırma sürecinde size yol gösteren [öneriler](../../security-center/security-center-virtual-machine-protection.md) oluşturur.

> [!NOTE]
> İzleme Uygulaması Hizmeti önizlemededir ve yalnızca Güvenlik Merkezi'nin [Standart katmanında](/azure/security-center/security-center-pricing) kullanılabilir.
>
>

## <a name="install-a-web-application-firewall"></a>Web uygulaması güvenlik duvarı yükleme
Web uygulamaları, bilinen yaygın güvenlik açıklarından yararlanan kötü amaçlı saldırıların giderek daha fazla hedefi olmaktadır. Bu açıklardan yararlanma örnekleri arasında SQL ekleme saldırıları, siteler arası komut dosyası saldırıları yaygındır. Uygulama kodunda bu tür saldırıların önlenmesi zor olabilir ve uygulama topolojisinin birçok katmanında ayrıntılı bakım, düzeltme eki uygulama ve izleme işlemleri gerektirebilir. Merkezi bir web uygulaması güvenlik duvarı, güvenlik yönetimini çok daha kolay hale getirir ve yetkisiz erişim ya da izinsiz giriş tehditlerine karşı uygulama yöneticilerine daha iyi güvence verir. Bir WAF çözümü, bilinen bir güvenlik açığına merkezi bir konumda düzeltme eki uygulayarak güvenlik tehdidine karşı, web uygulamalarının her birinin güvenliğini sağlamaya göre daha hızlı tepki verebilir. Var olan uygulama ağ geçitleri, web uygulaması güvenlik duvarı bulunan bir uygulama ağ geçidine kolaylıkla dönüştürülebilir.

[Web uygulaması güvenlik duvarı (WAF),](/azure/frontdoor/waf-overview) web uygulamalarınızın yaygın açıklardan ve güvenlik açıklarına karşı merkezi koruma sağlayan Uygulama Ağ Geçidi'nin bir özelliğidir. WAF, Open Web [Application Security Project (OWASP) temel kuralı](https://www.owasp.org/index.php/Category:OWASP_ModSecurity_Core_Rule_Set_Project) 3.0 veya 2.2.9 ayarlanan kurallara dayanır.

## <a name="monitor-the-performance-of-your-applications"></a>Uygulamalarınızın performansını izleme
İzleme, uygulamanızın performansını, durumunu ve kullanılabilirliğini belirlemek için veri toplama ve çözümleme eylemidir. Etkili bir izleme stratejisi, uygulamanız için bileşenlerin ayrıntılı işlemini anlamanıza yardımcı olur. Kritik sorunları size bildirerek çalışma sürenizi artırmanıza yardımcı olur, böylece sorun haline gelmeden önce bunları çözebilirsiniz. Ayrıca, güvenlikle ilgili olabilecek anormallikleri tespit emenize de yardımcı olur.

İster bulutta ister şirket içinde barındırılan uygulamanızın kullanılabilirliğini, performansını ve kullanımını izlemek için [Azure Uygulama Öngörüleri'ni](https://azure.microsoft.com/documentation/services/application-insights) kullanın. Uygulama Öngörüleri'ni kullanarak, bir kullanıcının bunları bildirmesini beklemeden uygulamanızdaki hataları hızla tanımlayabilir ve tanılayabilirsiniz. Topladığınız bilgilerle, uygulamanızın bakımı ve iyileştirmeleri ile ilgili bilinçli kararlar alabilirsiniz.

Application Insights, topladığı verilerle etkileşim kurmak için kapsamlı araçlara sahiptir. Application Insights, verilerini genel bir depoda saklar. Kusto sorgu diliyle uyarılar, panolar ve derin analiz gibi paylaşılan işlevsellikten yararlanabilir.

## <a name="perform-security-penetration-testing"></a>Güvenlik penetrasyon testi gerçekleştirin
Güvenlik savunmalarını doğrulamak, diğer işlevleri sınatarmak kadar önemlidir. [Penetrasyon testini](pen-testing.md) oluşturma ve dağıtım işleminizin standart bir parçası haline getirin. Dağıtılan uygulamalarda düzenli güvenlik testleri ve güvenlik açığı taraması zamanlayın ve açık bağlantı noktaları, uç noktalar ve saldırılar için izlemeyi izleyin.

Fuzz testi, bu verileri ayrıştıran ve tüketen program arabirimlerine (giriş noktaları) hatalı bilgilendirilmiş giriş verileri sağlayarak program hatalarını (kod hataları) bulma yöntemidir. [Microsoft Güvenlik Risk Algılama,](https://www.microsoft.com/en-us/security-risk-detection/) yazılımınızı Azure'a dağıtmadan önce hataları ve diğer güvenlik açıklarını aramak için kullanabileceğiniz bulut tabanlı bir araçtır. Araç, bir hatayı düzeltmeden önce güvenlik açıklarını yakalamak, böylece hatayla başa çıkmak, çökmelerle başa çıkmak veya yazılım yayımlandıktan sonra bir saldırıya yanıt vermek zorunda kalmamak için tasarlanmıştır.


## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, bir Azure PaaS dağıtımının güvenlik avantajları na ve bulut uygulamaları için en iyi güvenlik uygulamalarına odaklandık. Ardından, belirli Azure hizmetlerini kullanarak PaaS web ve mobil çözümlerinizi güvence altına almak için önerilen uygulamaları öğrenin. Azure Uygulama Hizmeti, Azure SQL Veritabanı ve Azure SQL Veri Ambarı ve Azure Depolama ile başlayacağız. Diğer Azure hizmetleri için önerilen uygulamalarla ilgili makaleler kullanıma sunulduğunda, bağlantılar aşağıdaki listede sağlanacaktır:

- [Azure App Service](paas-applications-using-app-services.md)
- [Azure SQL Veritabanı ve Azure SQL Veri Ambarı](paas-applications-using-sql.md)
- [Azure Depolama](paas-applications-using-storage.md)
- Redis için Azure Önbelleği
- Azure Service Bus
- Web Uygulama Güvenlik Duvarları

Bulut uygulamaları geliştirirken yazılım geliştirme yaşam döngüsünün her aşamasında göz önünde bulundurmanız gereken güvenlik soruları ve [denetimleri](abstract-develop-secure-apps.md) için Azure'da güvenli uygulamalar geliştirme bölümüne bakın.

Azure'u kullanarak bulut çözümlerinizi tasarlarken, dağıtırken ve yönetirken kullanabileceğiniz daha fazla güvenlik en iyi uygulamaları için Azure güvenlik en iyi uygulamaları ve [desenleri](best-practices-and-patterns.md) görün.

Azure güvenliği ve ilgili Microsoft hizmetleri hakkında daha genel bilgi sağlamak için aşağıdaki kaynaklar kullanılabilir:
* [Azure Güvenlik Ekibi Blogu](https://blogs.msdn.microsoft.com/azuresecurity/) - Azure Güvenlik'teki en son bilgiler için
* [Microsoft Güvenlik Yanıt Merkezi](https://technet.microsoft.com/library/dn440717.aspx) - Azure ile ilgili sorunlar da dahil olmak üzere Microsoft güvenlik açıklarının rapor edilebildiği veya e-posta yoluylasecure@microsoft.com
