---
title: Kimlik yönetimine yardımcı olan Azure güvenlik özellikleri | Microsoft Dokümanlar
description: " Bu makalede, kimlik yönetimine yardımcı olan temel Azure güvenlik özelliklerine genel bir bakış sağlanmaktadır. Microsoft kimlik ve erişim yönetimi çözümleri, BT'nin kurumsal veri merkezi genelinde ve bulutta uygulamalara ve kaynaklara erişimi korumasına yardımcı olarak Çoklu Faktör Kimlik Doğrulama ve Koşullu Erişim gibi ek doğrulama düzeyleri ne olanak sağlar Koşullarıdır. "
services: security
documentationcenter: na
author: TerryLanfear
manager: barbkess
editor: TomSh
ms.assetid: 5aa0a7ac-8f18-4ede-92a1-ae0dfe585e28
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/19/2018
ms.author: terrylan
Customer intent: As an IT Pro or decision maker I am trying to learn about identity management capabilities in Azure
ms.openlocfilehash: f61b6193a0d2082296a17128b41d7220f9b7e05f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77565902"
---
# <a name="azure-identity-management-security-overview"></a>Azure kimlik yönetimi güvenliğine genel bakış

 Kimlik yönetimi, [güvenlik ilkelerini](/windows/security/identity-protection/access-control/security-principals)doğrulama ve yetkilendirme işlemidir. Ayrıca, bu ilkeler (kimlikler) hakkındaki bilgileri kontrol etmeyi de içerir. Güvenlik ilkeleri (kimlikler) hizmetleri, uygulamaları, kullanıcıları, grupları vb. içerebilir. Microsoft kimlik ve erişim yönetimi çözümleri, BT'nin kurumsal veri merkezi ve buluttaki uygulamalara ve kaynaklara erişimi korumasına yardımcı olur. Bu tür koruma, Çok Faktörlü Kimlik Doğrulama ve Koşullu Erişim ilkeleri gibi ek doğrulama düzeyleri sağlar. Gelişmiş güvenlik raporlaması, denetim ve uyarı yoluyla şüpheli etkinlikleri izlemek olası güvenlik sorunlarını azaltmaya yardımcı olur. [Azure Active Directory Premium,](/azure/active-directory/active-directory-editions) hizmet olarak binlerce bulut yazılımına (SaaS) tek oturum açma (SSO) ve şirket içinde çalıştırdığınız web uygulamalarına erişim sağlar.
 
Azure Etkin Dizini'nin (Azure AD) güvenlik avantajlarından yararlanarak şunları yapabilirsiniz:

* Karma kuruluşunuzdaki her kullanıcı için tek bir kimlik oluşturup yönetebilir ve kullanıcıları, grupları ve cihazları eşitleyebilirsiniz. 
* Binlerce önceden entegre Edilmiş SaaS uygulaması da dahil olmak üzere uygulamalarınız için SSO erişimi sağlayın.
* Hem şirket içi hem de bulut uygulamaları için Multi-Factor Authentication tabanlı kurallar kullanarak uygulama erişimi güvenliğini sağlayabilirsiniz.
* Azure AD Application Proxy aracılığıyla şirket içi web uygulamalarına güvenli uzaktan erişim sağlama.

Bu makalenin amacı, kimlik yönetimine yardımcı olan temel Azure güvenlik özelliklerine genel bir bakış sağlamaktır. Ayrıca, daha fazla bilgi edinebilmeniz için her özelliğin ayrıntılarını veren makalelere bağlantılar da salıyoruz.  

Makale, aşağıdaki temel Azure Kimlik yönetimi yeteneklerine odaklanır:

* Çoklu oturum açma
* Ters proxy
* Multi-Factor Authentication
* Rol tabanlı erişim kontrolü (RBAC)
* Güvenlik izleme, uyarılar ve makine öğrenimi tabanlı raporlar
* Tüketici kimliği ve erişim yönetimi
* Cihaz kaydı
* Privileged Identity Management
* Kimlik koruması
* Karma kimlik yönetimi/Azure AD bağlantısı
* Azure AD erişim gözden geçirmeleri

## <a name="single-sign-on"></a>Çoklu oturum açma

SSO, tek bir kullanıcı hesabı kullanarak yalnızca bir kez oturum açarak, iş yapmanız gereken tüm uygulamalara ve kaynaklara erişebilmek anlamına gelir. Oturum açtınca, ikinci kez kimlik doğrulamanız (örneğin parola yazma) gerekkalmadan ihtiyacınız olan tüm uygulamalara erişebilirsiniz.

Birçok kuruluş, kullanıcı üretkenliği için Office 365, Box ve Salesforce gibi SaaS uygulamalarına güvenir. Tarihsel olarak, BT personelinin her SaaS uygulamasında kullanıcı hesaplarını tek tek oluşturması ve güncelleştirmesi gerekiyordu ve kullanıcılar her SaaS uygulaması için bir parola hatırlamak zorunda kaldı.

Azure AD, şirket içi Active Directory ortamlarını buluta genişleterek, kullanıcıların birincil kuruluş hesaplarını kullanarak yalnızca etki alanına katılan aygıtlarında ve şirket kaynaklarında değil, aynı zamanda ihtiyaç duydukları tüm web ve SaaS uygulamalarında oturum açmalarını sağlar işleri için.

Kullanıcılar yalnızca birden çok kullanıcı adı ve parola kümesini yönetmek zorunda kalmamakla kalmayıp, kuruluş gruplarına ve çalışan durumlarına bağlı olarak uygulama erişimini otomatik olarak sağlayabilir veya sağlamadan edebilirsiniz. Azure AD, SaaS uygulamalarında kullanıcıların erişimini merkezi olarak yönetebileceğiniz güvenlik ve erişim yönetişim denetimlerini sunar.

Daha fazla bilgi edinin:

* [Tek oturum açma genel bakışı](https://azure.microsoft.com/documentation/videos/overview-of-single-sign-on/)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../../active-directory/manage-apps/what-is-single-sign-on.md)
* [Azure Active Directory'yi SaaS uygulamalarıyla tek oturum açma ile tümleştirme](../../active-directory/manage-apps/configure-single-sign-on-non-gallery-applications.md)

## <a name="reverse-proxy"></a>Ters proxy

Azure AD Application Proxy, özel ağınızdaki [SharePoint](https://support.office.com/article/What-is-SharePoint-97b915e6-651b-43b2-827d-fb25777f446f?ui=en-US&rs=en-US&ad=US) siteleri, [Outlook Web Uygulaması](https://technet.microsoft.com/library/jj657718.aspx)ve [IIS](https://www.iis.net/)tabanlı uygulamalar gibi şirket içi uygulamaları yayımlamanızı sağlar ve ağınızın dışındaki kullanıcılara güvenli erişim sağlar. Application Proxy, Azure AD'nin desteklediği binlerce SaaS uygulamasıyla birçok şirket içi web uygulaması türü için uzaktan erişim ve SSO sağlar. Çalışanlar uygulamalarınızda kendi cihazlarıyla oturum açabilir ve bu bulut tabanlı proxy aracılığıyla kimlik doğrulaması yapabilir.

Daha fazla bilgi edinin:

* [Azure AD Uygulama Proxy'sini Etkinleştirme](/azure/active-directory/manage-apps/application-proxy-enable)
* [Azure AD Uygulama Ara Sunucusu ile uygulama yayımlama](/azure/active-directory/active-directory-application-proxy-publish)
* [Uygulama Ara Sunucusu ile çoklu oturum açma](../../active-directory/manage-apps/application-proxy-configure-single-sign-on-with-kcd.md)
* [Koşullu Erişim ile Çalışma](../../active-directory/manage-apps/application-proxy-integrate-with-sharepoint-server.md)

## <a name="multi-factor-authentication"></a>Multi-Factor Authentication

Azure Çok Faktörlü Kimlik Doğrulama, birden fazla doğrulama yöntemi nin kullanılmasını gerektiren ve kullanıcı oturum açma ve hareketlere kritik bir ikinci güvenlik katmanı ekleyen bir kimlik doğrulama yöntemidir. Çok Faktörlü Kimlik Doğrulama, basit bir oturum açma işlemi için kullanıcı talebini karşılarken verilere ve uygulamalara erişimi korumaya yardımcı olur. Telefon görüşmeleri, kısa mesajlar veya mobil uygulama bildirimleri veya doğrulama kodları ve üçüncü taraf OAuth belirteçleri gibi çeşitli doğrulama seçenekleri yle güçlü kimlik doğrulaması sağlar.

Daha fazla bilgi edinin:

* [Multi-Factor Authentication](https://azure.microsoft.com/documentation/services/multi-factor-authentication/)
* [Azure Multi-Factor Authentication nedir?](/azure/active-directory/authentication/multi-factor-authentication)
* [Azure Multi-Factor Authentication nasıl çalışır?](../../active-directory/authentication/concept-mfa-howitworks.md)

## <a name="rbac"></a>RBAC

RBAC, Azure Resource Manager'ı temel alan, Azure kaynakları için ayrıntılı erişim yönetimi sağlayan bir yetkilendirme sistemidir. RBAC, kullanıcıların sahip olduğu erişim düzeyini ayrıntılı olarak denetlemenize olanak tanır. Örneğin, bir kullanıcıyı yalnızca sanal ağları ve başka bir kullanıcıyı bir kaynak grubundaki tüm kaynakları yönetmesi için yönetecek şekilde sınırlandırabilirsiniz. Azure'da kullanabileceğiniz birçok yerleşik rol vardır. Dört temel yerleşik rol aşağıda listelenmiştir. İlk üçü tüm kaynak türleri için geçerlidir.

Daha fazla bilgi edinin:

* [Rol tabanlı erişim denetimi (RBAC) nedir?](/azure/role-based-access-control/overview)
* [Azure kaynakları için yerleşik roller](/azure/role-based-access-control/built-in-roles)

## <a name="security-monitoring-alerts-and-machine-learning-based-reports"></a>Güvenlik izleme, uyarılar ve makine öğrenimi tabanlı raporlar

Tutarsız erişim kalıplarını tanımlayan güvenlik izleme, uyarılar ve makine öğrenimi tabanlı raporlar işletmenizi korumanıza yardımcı olabilir. Kuruluşunuzun dizininin bütünlüğü ve güvenliği nde görünürlük elde etmek için Azure AD erişim ve kullanım raporlarını kullanabilirsiniz. Bu bilgilerle, dizin yöneticisi olası güvenlik risklerinin nerede olabileceğini daha iyi belirleyebilir, böylece bu riskleri yeterince azaltmayı planlayabilirler.

Azure portalında raporlar aşağıdaki kategorilere girer:

* **Anomali raporları**: Anormal olduğu tespit ettiğimiz oturum açma olaylarını içerir. Amacımız, bu tür etkinliklerden haberdar etmenizi sağlamak ve bir olayın şüpheli olup olmadığını belirlemenize olanak sağlamaktır.
* **Tümleşik Uygulama raporları**: Bulut uygulamalarının kuruluşunuzda nasıl kullanıldığına ilişkin öngörüler sağlayın. Azure AD, binlerce bulut uygulamasıyla tümleştirme sunar.
* **Hata raporları**: Dış uygulamalara hesap verdirirken oluşabilecek hataları belirtin.
* **Kullanıcıya özel raporlar**: Belirli bir kullanıcı için cihaz oturum açma etkinlik verilerini görüntüleyin.
* **Etkinlik günlükleri**: Son 24 saat, son 7 gün veya son 30 gün içinde denetlenen tüm olayların kaydını ve grup etkinlik değişikliklerini ve parola sıfırlama ve kayıt etkinliğini içerir.

Daha fazla bilgi edinin:

* [Erişim ve kullanım raporlarınızı görüntüleme](/azure/active-directory/active-directory-view-access-usage-reports)
* [Azure Active Directory raporlaması ile başlayın](/azure/active-directory/active-directory-reporting-getting-started)
* [Azure Active Directory raporlama kılavuzu](/azure/active-directory/active-directory-reporting-guide)

## <a name="consumer-identity-and-access-management"></a>Tüketici kimliği ve erişim yönetimi

Azure AD B2C, yüz milyonlarca kimlikle ölçeklendirilebilen tüketiciye yönelik uygulamalar için son derece kullanılabilir, küresel, kimlik yönetimi hizmetidir. Bu hizmet mobil platformlar ve web platformlarıyla tümleştirilebilir. Tüketicileriniz, mevcut sosyal hesaplarını kullanarak veya yeni kimlik bilgileri oluşturarak özelleştirilebilir deneyimler aracılığıyla tüm uygulamalarınızda oturum açabilir.

Geçmişte, müşterileri kaydetmek ve uygulamalarına oturum açmak isteyen uygulama geliştiricileri kendi kodlarını yazardı. Ayrıca, kullanıcı adları ile parolaları depolamak için şirket içi veritabanlarını veya sistemleri kullanırlardı. Azure AD B2C, güvenli, standartlara dayalı bir platform ve genişletilebilir geniş bir dizi ilke yardımıyla kuruluşunuzun tüketici kimlik yönetimini uygulamalara entegre etmenin daha iyi bir yolunu sunar.

Azure AD B2C'yi kullandığınızda, tüketicileriniz mevcut sosyal hesaplarını (Facebook, Google, Amazon, LinkedIn) kullanarak veya yeni kimlik bilgileri (e-posta adresi ve parola veya kullanıcı adı ve parola) oluşturarak uygulamalarınız için kaydolabilir.

Daha fazla bilgi edinin:

* [Azure Active Directory B2C nedir?](https://azure.microsoft.com/services/active-directory-b2c/)
* [Azure Active Directory B2C önizlemesi: Tüketicilerinizin uygulamanıza kaydolmalarını ve oturum açmalarını sağlama](../../active-directory-b2c/overview.md)
* [Azure Active Directory B2C Preview: Uygulama türleri](../../active-directory-b2c/application-types.md)

## <a name="device-registration"></a>Cihaz kaydı

Azure AD aygıt kaydı, aygıt tabanlı [Koşullu Erişim](/azure/active-directory/active-directory-conditional-access-device-registration-overview) senaryolarının temelidir. Bir aygıt kaydedildiğinde, Azure AD aygıt kaydı cihaza, kullanıcı giriş yaptığında aygıtın kimliğini doğrulamak için kullandığı bir kimlik sağlar. Kimlik doğrulaması yapılan aygıt ve aygıtın öznitelikleri, bulutta ve şirket içinde barındırılan uygulamalar için Koşullu Erişim ilkelerini zorlamak için kullanılabilir.

Intune gibi bir mobil aygıt yönetimi çözümüyle birleştirildiğinde, Azure AD'deki aygıt öznitelikleri aygıt hakkında ek bilgilerle güncelleştirilir. Daha sonra, güvenlik ve uyumluluk standartlarınızı karşılamak için aygıtlardan erişimi zorunlu kıkan Koşullu Erişim kuralları oluşturabilirsiniz.

Daha fazla bilgi edinin:

* [Azure AD aygıt kaydıyla başlayın](/azure/active-directory/active-directory-conditional-access-device-registration-overview)
* [Windows etki alanı yla birleştirilmiş aygıtlar için Azure AD ile otomatik aygıt kaydı](/azure/active-directory/active-directory-conditional-access-automatic-device-registration)
* [Azure AD ile Windows etki alanına bağlı aygıtların otomatik kaydını ayarlama](/azure/active-directory/active-directory-conditional-access-automatic-device-registration-setup)

## <a name="privileged-identity-management"></a>Privileged Identity Management

Azure AD Ayrıcalıklı Kimlik Yönetimi ile, Azure AD'deki ayrıcalıklı kimliklerinizi ve kaynaklara ve Office 365 ve Microsoft Intune gibi diğer Microsoft çevrimiçi hizmetlerine erişimi yönetebilir, denetleyebilir ve izleyebilirsiniz.

Kullanıcıların bazen Azure veya Office 365 kaynaklarında veya diğer SaaS uygulamalarında ayrıcalıklı işlemler gerçekleştirmeleri gerekir. Bu gereksinim genellikle kuruluşların Kullanıcılara Azure AD'de kalıcı ayrıcalıklı erişim vermesi gerektiği anlamına gelir. Kuruluşlar, kullanıcıların yönetici ayrıcalıklarıyla ne yaptıklarını yeterince izleyemediğinden, bu tür erişim bulut tarafından barındırılan kaynaklar için büyüyen bir güvenlik riskidir. Ayrıca, ayrıcalıklı erişimi olan bir kullanıcı hesabı tehlikeye girerse, bir ihlal kuruluşun genel bulut güvenliğini etkileyebilir. Azure AD Ayrıcalıklı Kimlik Yönetimi, bu riski azaltmaya yardımcı olur.

Azure AD Ayrıcalıklı Kimlik Yönetimi ile şunları yapabilirsiniz:

* Hangi kullanıcıların Azure AD yöneticisi olduğunu görün.
* Office 365 ve Intune gibi Microsoft hizmetlerine isteğe bağlı, tam zamanında (JIT) yönetim erişimi etkinleştirin.
* Yönetici erişim geçmişi ve yönetici atamalarındaki değişiklikler hakkında raporlar alın.
* Ayrıcalıklı bir role erişim konusunda uyarılar alın.

Daha fazla bilgi edinin:

* [Azure AD Privileged Identity Management nedir?](../../active-directory/privileged-identity-management/pim-configure.md)
* [PIM'de Azure AD dizini rollerini atama](../../active-directory/privileged-identity-management/pim-how-to-add-role-to-user.md)

## <a name="identity-protection"></a>Kimlik koruması

Azure AD Kimlik Koruması, kuruluşunuzun kimliklerini etkileyen risk algılamaları ve olası güvenlik açıklarına yönelik konsolide bir görünüm sağlayan bir güvenlik hizmetidir. Kimlik Koruması, Azure AD Anormal Etkinlik raporları aracılığıyla kullanılabilen mevcut Azure AD anomali algılama özelliklerinden yararlanır. Kimlik Koruma ayrıca anormallikleri gerçek zamanlı olarak algılayabilen yeni risk algılama türleri de sunar.

Daha fazla bilgi edinin:

* [Azure AD Kimlik Koruması](/azure/active-directory/identity-protection/overview)
* [Kanal 9: Azure REKLAM ve Kimlik Gösterisi: Kimlik Koruma Önizleme](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)

## <a name="hybrid-identity-managementazure-ad-connect"></a>Karma kimlik yönetimi/Azure AD bağlantısı

Microsoft’un kimlik çözümleri şirket içi ve bulut tabanlı çözümleri birleştirerek konumdan bağımsız olarak tüm kaynaklara kimlik doğrulaması ve yetkilendirme sağlamak üzere tek bir kullanıcı kimliği oluşturur. Buna hibrit kimlik denir. Azure AD Connect, Microsoft'un karma kimlik hedeflerinizi karşılamak ve gerçekleştirmek için tasarladığı araçtır. Bu sayede kullanıcılarınıza Azure AD ile tümleşik Office 365, Azure ve SaaS uygulamaları için ortak bir kimlik sağlayabilirsiniz. Aşağıdaki özellikleri sağlar:

* Eşitleme
* AD FS ve federasyon entegrasyonu
* Kimlik doğrulamayoluyla geçiş
* Sistem Durumunu İzleme

Daha fazla bilgi edinin:

* [Karma kimlik beyaz kağıt](https://download.microsoft.com/download/D/B/A/DBA9E313-B833-48EE-998A-240AA799A8AB/Hybrid_Identity_White_Paper.pdf)
* [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/)
* [Azure AD ekibi blogu](https://blogs.technet.microsoft.com/ad/)

## <a name="azure-ad-access-reviews"></a>Azure AD erişim gözden geçirmeleri

Azure Active Directory (Azure AD) erişim gözden geçirmeleri, kuruluşların grup üyeliklerini, kurumsal uygulamalara erişimi ve ayrıcalıklı rol atamalarını verimli şekilde yönetmesine olanak sağlar.

Daha fazla bilgi edinin:

* [Azure AD erişim gözden geçirmeleri](../../active-directory/governance/access-reviews-overview.md)
* [Azure AD erişim gözden geçirmeleriyle kullanıcı erişimini yönetme](../../active-directory/governance/access-reviews-overview.md)
