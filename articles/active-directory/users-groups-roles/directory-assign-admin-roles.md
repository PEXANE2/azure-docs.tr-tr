---
title: Yönetici rol açıklamaları ve izinleri - Azure AD | Microsoft Dokümanlar
description: Yönetici rolü kullanıcıları ekleyebilir, yönetim rolleri atayabilir, kullanıcı parolalarını sıfırlayabilir, kullanıcı lisanslarını yönetebilir veya etki alanlarını yönetebilir.
services: active-directory
author: curtand
manager: daveba
search.appverid: MET150
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: reference
ms.date: 04/14/2020
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro, fasttrack-edit
ms.collection: M365-identity-device-management
ms.openlocfilehash: bfe8aa088538663ac3e64f5913ff031e6160b045
ms.sourcegitcommit: ea006cd8e62888271b2601d5ed4ec78fb40e8427
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81382651"
---
# <a name="administrator-role-permissions-in-azure-active-directory"></a>Azure Active Directory'deki yönetici rolü izinleri

Azure Etkin Dizini 'ni (Azure AD) kullanarak, kimlik görevlerini daha az ayrıcalıklı rollerde yönetmesi için sınırlı yöneticileri atayabilirsiniz. Yöneticiler, kullanıcı ekleme veya değiştirme, yönetim rolleri atama, kullanıcı parolalarını sıfırlama, kullanıcı lisanslarını yönetme ve etki alanı adlarını yönetme gibi amaçlarla atanabilir. Varsayılan kullanıcı izinleri yalnızca Azure AD'deki kullanıcı ayarlarında değiştirilebilir.

## <a name="limit-use-of-global-administrator"></a>Global yöneticinin kullanımını sınırlandırın

Genel yönetici rolüne atanan kullanıcılar, Azure REKLAM kuruluşunuzdaki her yönetim ayarını okuyabilir ve değiştirebilir. Varsayılan olarak, Azure aboneliğine kaydolan kişiye Azure AD kuruluşu için Global yönetici rolü atanır. Yönetici rollerini yalnızca Global yöneticiler ve Ayrıcalıklı Rol yöneticileri devredebilir. İşletmeniziçin riski azaltmak için, bu rolü kuruluşunuzdaki mümkün olan en az kişiye atamanızı öneririz.

En iyi uygulama olarak, bu rolü kuruluşunuzdaki beşten az kişiye atamanızı öneririz. Kuruluşunuzdaki Genel Yönetici rolüne atanmış beşten fazla yöneticiniz varsa, bu yöneticinin kullanımını azaltmanın bazı yolları aşağıda verilmiştir.

### <a name="find-the-role-you-need"></a>İhtiyacınız olan rolü bulun

Birçok rol listesinde ihtiyacınız olan rolü bulmanız sizi hayal kırıklığına uğratıyorsa, Azure AD rol kategorilerine göre rollerin alt kümelerini gösterebilir. Yalnızca seçili türdeki rolleri göstermek için [Azure REKLAM Rolleri ve yöneticileri](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RolesAndAdministrators) için yeni **Tür** filtremize göz atın.

### <a name="a-role-exists-now-that-didnt-exist-when-you-assigned-the-global-administrator-role"></a>Global yönetici rolünü atadığınızda var olmayan bir rol artık var

Azure AD'ye bazı kullanıcıları Global yöneticiye yükselttiğinde seçenek olmayan daha ayrıntılı izinler sağlayan bir rol veya rol eklenmiş olabilir. Zaman içinde, yalnızca Genel Yönetici rolünün daha önce yapabileceği görevleri gerçekleştiren ek roller kullanıma sunacağız. Bunları aşağıdaki [Kullanılabilir rollerde](#available-roles)yansıttığını görebilirsiniz.

## <a name="assign-or-remove-administrator-roles"></a>Yönetici rollerini atama veya kaldırma

Azure Etkin Dizin'inde bir kullanıcıya yönetim rolleri nasıl ataysüreceğinizi öğrenmek için Azure [Etkin Dizin'de Görüntüle ve Yönetici rollerini atayabakın.](directory-manage-roles-portal.md)

## <a name="available-roles"></a>Kullanılabilir roller

Aşağıdaki yönetici rolleri kullanılabilir:

### <a name="application-administrator"></a>[Uygulama Yöneticisi](#application-administrator-permissions)

Bu roldeki kullanıcılar kurumsal uygulamaların, uygulama kayıtlarının ve uygulama proxy ayarlarının tüm yönlerini oluşturabilir ve yönetebilir. Bu role atanan kullanıcıların yeni uygulama kayıtları veya kurumsal uygulamalar oluştururken sahip olarak eklenmediğini unutmayın.

Uygulama Yöneticileri, uygulamanın kimliğine bürünmelerine olanak tanıyan uygulama kimlik bilgilerini yönetebilir. Bu nedenle, bu role atanan kullanıcılar yalnızca herhangi bir Azure REKLAM rolüne atanmamış uygulamaların veya yalnızca yönetici rollerine atanan uygulamaların uygulama kimlik bilgilerini yönetebilir:
* Uygulama Yöneticisi
* Uygulama Geliştirici
* Bulut Uygulaması Yöneticisi
* Dizin Okuyucular

Bir uygulama yukarıda belirtilmeyen başka bir role atanmışsa, Uygulama Yöneticisi bu uygulamanın kimlik bilgilerini yönetemez. 
 
Bu rol, Microsoft Graph API'deki izinler dışında, devredilen izinlere ve uygulama izinlerine _izin_ verme olanağı da verir.

> [!IMPORTANT]
> Bu özel durum, _diğer_ uygulamaların (örneğin, kaydettiğiniz üçüncü taraf uygulamalar veya uygulamalar) izinlerini yine de kabul edebilirsiniz, ancak Azure AD'nin kendisinden izin almadığınız anlamına gelir. _Bu_ izinleri uygulama kaydının bir parçası olarak istemeye devam edebilirsiniz, ancak bu izinleri _vermek_ (örneğin, izin vermek) bir Azure AD yöneticisi gerektirir. Bu, kötü niyetli bir kullanıcının, örneğin tüm dizinlere yazabilen bir uygulama oluşturarak ve ona rıza göstererek ve bu uygulamanın izinleri aracılığıyla kendilerini küresel bir yönetici olmak için yükselterek izinlerini kolayca yükseltemeyeceği anlamına gelir.

### <a name="application-developer"></a>[Uygulama Geliştirici](#application-developer-permissions)

Bu roldeki kullanıcılar, "Kullanıcılar uygulamaları kaydedebilir" ayarı No olarak ayarlandığında uygulama kayıtları oluşturabilir. Bu rol aynı zamanda "Kullanıcılar kendi adına şirket verilerine erişen uygulamalara izin verebilir" ayarı No olarak ayarlandığında, kişinin kendi adına rıza göstermesine izin verir. Bu role atanan kullanıcılar, yeni uygulama kayıtları veya kurumsal uygulamalar oluştururken sahip olarak eklenir.

### <a name="authentication-administrator"></a>[Kimlik Doğrulama Yöneticisi](#authentication-administrator-permissions)

Kimlik Doğrulama yöneticisi rolü şu anda genel önizlemededir. Bu role sahip kullanıcılar parola olmayan kimlik bilgilerini ayarlayabilir veya sıfırlayabilir ve tüm kullanıcılar için parolaları güncelleştirebilir. Kimlik Doğrulama Yöneticileri, kullanıcıların mevcut parola dışı kimlik bilgilerine (örneğin, MFA veya FIDO) karşı yeniden kaydolmalarını ve **aygıttaki MFA'yı iptal**etmesini gerektirebilir , bu da yönetici olmayan veya yalnızca aşağıdaki rolleratanan kullanıcıların bir sonraki oturumlarında MFA'yı ister:

* Kimlik Doğrulama Yöneticisi
* Dizin Okuyucular
* Misafir Davetçi
* İleti Merkezi Okuyucu
* Raporlar Okuyucu

> [!IMPORTANT]
> Bu role sahip kullanıcılar, Azure Active Directory içinde ve dışında hassas veya özel bilgilere veya kritik yapılandırmaya erişimi olan kişilerin kimlik bilgilerini değiştirebilir. Bir kullanıcının kimlik bilgilerini değiştirmek, kullanıcının kimliğini ve izinlerini varsayma yeteneği anlamına gelebilir. Örneğin:
>
>- Sahip oldukları uygulamaların kimlik bilgilerini yönetebilen Uygulama Kaydı ve Kurumsal Uygulama sahipleri. Bu uygulamaların Azure AD'de ayrıcalıklı izinleri olabilir ve kimlik doğrulama yöneticilerine verilmeyen başka yerlerde. Bu yol sayesinde bir Kimlik Doğrulama Yöneticisi bir uygulama sahibinin kimliğini üstlenebilir ve daha sonra uygulama için kimlik bilgilerini güncelleştirerek ayrıcalıklı bir uygulamanın kimliğini üstlenebilir.
>- Azure'da hassas veya özel bilgilere veya kritik yapılandırmaya erişebilecek azure abonelik sahipleri.
>- Grup üyeliğini yönetebilen Güvenlik Grubu ve Office 365 Grup sahipleri. Bu gruplar, Azure AD'de ve başka yerlerde hassas veya özel bilgilere veya kritik yapılandırmaya erişim sağlayabilir.
>- Exchange Online, Office Security ve Compliance Center ve insan kaynakları sistemleri gibi Azure AD dışındaki diğer hizmetlerdeki yöneticiler.
>- Hassas veya özel bilgilere erişebilen yöneticiler, hukuk müşaviri ve insan kaynakları çalışanları gibi yönetici olmayan yöneticiler.

### <a name="azure-devops-administrator"></a>[Azure Devops Yöneticisi](#azure-devops-administrator-permissions)

Bu role sahip kullanıcılar, yeni Azure DevOps organizasyon oluşturmayı bir dizi yapılandırılabilir kullanıcı veya grupla sınırlamak için Azure DevOps ilkesini yönetebilir. Bu roldeki kullanıcılar bu ilkeyi, şirketin Azure AD kuruluşunu destekleyen herhangi bir Azure DevOps kuruluşu aracılığıyla yönetebilir.

Tüm kurumsal Azure DevOps ilkeleri bu roldeki kullanıcılar tarafından yönetilebilir.

### <a name="azure-information-protection-administrator"></a>[Azure Bilgi Koruma Yöneticisi](#azure-information-protection-administrator-permissions)

Bu role sahip kullanıcılar Azure Bilgi Koruması hizmetindeki tüm izinlere sahiptir. Bu rol, Azure Bilgi Koruması ilkesi için etiketlerin yapılandırılmasına, koruma şablonlarının yönetilmesine ve korumanın etkinhale ilmesine olanak tanır. Bu rol Kimlik Koruma Merkezi, Ayrıcalıklı Kimlik Yönetimi, Monitor Office 365 Hizmet Durumu veya Office 365 Güvenlik & Uyumluluk Merkezi'nde izin vermez.

### <a name="b2c-user-flow-administrator"></a>[B2C Kullanıcı Akış Yöneticisi](#b2c-user-flow-administrator-permissions)

Bu role sahip kullanıcılar Azure portalında B2C Kullanıcı Akışları (yerleşik ilkeler olarak da adlandırılır) oluşturabilir ve yönetebilir.Kullanıcı akışları oluşturarak veya düzenleyerek, bu kullanıcılar kullanıcı deneyiminin html/CSS/javascript içeriğini değiştirebilir, kullanıcı akışı başına MFA gereksinimlerini değiştirebilir, belirteçteki talepleri değiştirebilir ve kiracıdaki tüm ilkeler için oturum ayarlarını değiştirebilir. Diğer taraftan, bu rol, kullanıcı verilerini gözden geçirme veya kiracı şemasında yer alan özniteliklerde değişiklik yapma yeteneğini içermez.Kimlik Deneyimi Çerçevesi (Özel olarak da bilinir) ilkelerindeki değişiklikler de bu rolün kapsamı dışındadır.

### <a name="b2c-user-flow-attribute-administrator"></a>[B2C Kullanıcı Akışı Öznitelik Yöneticisi](#b2c-user-flow-attribute-administrator-permissions)

Bu role sahip kullanıcılar, kiracıdaki tüm kullanıcı akışlarıiçin özel öznitelikler ekler veya siler.Bu nedenle, bu role sahip kullanıcılar son kullanıcı şemasını değiştirebilir veya yeni öğeler ekleyebilir ve tüm kullanıcı akışlarının davranışını etkileyebilir ve dolaylı olarak son kullanıcılardan hangi verilerin istenebileceği ve sonuçta uygulamalara talep olarak gönderilebilecek değişikliklere neden olabilir.Bu rol kullanıcı akışlarını kaldıramaz.

### <a name="b2c-ief-keyset-administrator"></a>[B2C IEF Anahtar Seti Yöneticisi](#b2c-ief-keyset-administrator-permissions)

Kullanıcı belirteç şifreleme, belirteç imzaları ve şifreleme/şifre çözme talebi için ilke anahtarları ve sırlarını oluşturabilir ve yönetebilir.Varolan anahtar kapsayıcılarına yeni anahtarlar ekleyerek, bu sınırlı yönetici varolan uygulamaları etkilemeden gerektiğinde sırları devredebilir.Bu kullanıcı, bu sırların tam içeriğini ve son kullanma tarihlerini oluşturulduktan sonra bile görebilir.

> [!IMPORTANT]
> Bu hassas bir rol.Anahtar seti yönetici rolü dikkatle denetlenmeli ve üretim öncesi ve üretim sırasında özenle atanmalıdır.

### <a name="b2c-ief-policy-administrator"></a>[B2C IEF İlke Yöneticisi](#b2c-ief-policy-administrator-permissions)

Bu roldeki kullanıcılar Azure AD B2C'deki tüm özel ilkeleri oluşturma, okuma, güncelleştirme ve silme yeteneğine sahiptir ve bu nedenle ilgili Azure AD B2C kiracısında Kimlik Deneyimi Çerçevesi üzerinde tam denetime sahiptir. Bu kullanıcı, ilkeleri düzenleyerek, dış kimlik sağlayıcılarıyla doğrudan federasyon kurabilir, dizin şemasını değiştirebilir, kullanıcıya yönelik tüm içeriği değiştirebilir (HTML, CSS, JavaScript), kimlik doğrulamayı tamamlamak için gereksinimleri değiştirebilir, yeni kullanıcılar oluşturabilir, kullanıcı verilerini tam geçişler de dahil olmak üzere dış sistemlere gönderebilir ve parolalar ve telefon numaraları gibi hassas alanlar da dahil olmak üzere tüm kullanıcı bilgilerini düzenleyebilir. Tersine, bu rol şifreleme anahtarlarını değiştiremez veya kiracıda federasyon için kullanılan sırları kaldıramaz.

> [!IMPORTANT]
> B2 IEF İlke Yöneticisi, üretimdeki kiracılar için çok sınırlı bir temelde atanması gereken son derece hassas bir roldür.Bu kullanıcıların faaliyetleri, özellikle üretimdeki kiracılar için yakından denetlenmelidir.

### <a name="billing-administrator"></a>[Fatura Yöneticisi](#billing-administrator-permissions)

Satın alma işlemleri yapar, abonelikleri yönetir, destek biletlerini yönetir ve hizmetin sistem durumunu izler.

### <a name="cloud-application-administrator"></a>[Bulut Uygulaması Yöneticisi](#cloud-application-administrator-permissions)

Bu roldeki kullanıcılar, uygulama proxy'sini yönetme olanağı dışında Uygulama Yöneticisi rolüyle aynı izinlere sahiptir. Bu rol, kurumsal uygulamaların ve uygulama kayıtlarının tüm yönlerini oluşturma ve yönetme olanağı sağlar. Bu rol ayrıca, temsilci verilen izinlere ve Microsoft Graph API hariç uygulama izinlerine izin verme olanağı da verir. Bu role atanan kullanıcılar, yeni uygulama kayıtları veya kurumsal uygulamalar oluştururken sahip olarak eklenmez.

Bulut Uygulama Yöneticileri, uygulamanın kimliğine bürünmelerine olanak tanıyan uygulama kimlik bilgilerini yönetebilir. Bu nedenle, bu role atanan kullanıcılar yalnızca herhangi bir Azure REKLAM rolüne atanmamış uygulamaların veya yalnızca yönetici rollerine atanan uygulamaların uygulama kimlik bilgilerini yönetebilir:
* Uygulama Geliştirici
* Bulut Uygulaması Yöneticisi
* Dizin Okuyucular

Bir uygulama yukarıda belirtilmeyen başka bir role atanmışsa, Bulut Uygulama Yöneticisi bu uygulamanın kimlik bilgilerini yönetemez.

### <a name="cloud-device-administrator"></a>[Bulut Aygıt Yöneticisi](#cloud-device-administrator-permissions)

Bu roldeki kullanıcılar Azure AD'deki aygıtları etkinleştirebilir, devre dışı edebilir ve silebilir ve Azure portalında Windows 10 BitLocker anahtarlarını (varsa) okuyabilir. Rol, aygıttaki diğer özellikleri yönetme izni vermez.

### <a name="compliance-administrator"></a>[Uyumluluk Yöneticisi](#compliance-administrator-permissions)

Bu role sahip kullanıcılar, Microsoft 365 uyumluluk merkezi, Microsoft 365 yönetici merkezi, Azure ve Office 365 Güvenlik & Uyumluluk Merkezi'nde uyumlulukla ilgili özellikleri yönetme iznine sahiptir. Devralan kişiler ayrıca Exchange yönetim merkezi ve Skype for Business yönetici merkezleri & Ekipler'deki tüm özellikleri yönetebilir ve Azure ve Microsoft 365 için destek biletleri oluşturabilir. Office [365 yönetici rolleri hakkında](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d)daha fazla bilgi edinilebilir.

İçindeki | Yapabilirim
----- | ----------
[Microsoft 365 uyumluluk merkezi](https://protection.office.com) | Microsoft 365 hizmetlerinde kuruluşunuzun verilerini koruyun ve yönetin<br>Uyumluluk uyarılarını yönetme
[Uyumluluk Yöneticisi](https://docs.microsoft.com/office365/securitycompliance/meet-data-protection-and-regulatory-reqs-using-microsoft-cloud) | Kuruluşunuzun mevzuata uygunluk etkinliklerini izleme, atama ve doğrulama
[Office 365 Güvenlik & Uyum Merkezi](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d) | Veri yönetimini yönetme<br>Yasal ve veri araştırması yapın<br>Veri Konu İsteğini Yönetme<br><br>Bu rol, Office 365 Güvenlik & Uyumluluk Merkezi rol tabanlı erişim denetiminde [Uyumluluk Yöneticisi RoleGroup](https://docs.microsoft.com/microsoft-365/security/office-365-security/permissions-in-the-security-and-compliance-center#permissions-needed-to-use-features-in-the-security--compliance-center) ile aynı izinlere sahiptir.
[Intune](https://docs.microsoft.com/intune/role-based-access-control) | Tüm Intune denetim verilerini görüntüleme
[Cloud App Security](https://docs.microsoft.com/cloud-app-security/manage-admins) | Salt okunur izinleri vardır ve uyarıları yönetebilir<br>Dosya ilkeleri oluşturabilir ve değiştirebilir ve dosya yönetimi eylemlerine izin verebilir<br>Veri Yönetimi kapsamındaki tüm yerleşik raporları görüntüleyebilir

### <a name="compliance-data-administrator"></a>[Uyumluluk Veri Yöneticisi](#compliance-data-administrator-permissions)

Bu role sahip kullanıcılar, Microsoft 365 uyumluluk merkezi, Microsoft 365 yönetici merkezi ve Azure'daki verileri izleme izinlerine sahiptir. Kullanıcılar ayrıca Exchange yönetici merkezi, Uyumluluk Yöneticisi ve Skype for Business yönetici merkezi & Ekipler içindeki uyumluluk verilerini izleyebilir ve Azure ve Microsoft 365 için destek biletleri oluşturabilir.

İçindeki | Yapabilirim
----- | ----------
[Microsoft 365 uyumluluk merkezi](https://protection.office.com) | Microsoft 365 hizmetleri genelinde uyumlulukla ilgili ilkeleri izleme<br>Uyumluluk uyarılarını yönetme
[Uyumluluk Yöneticisi](https://docs.microsoft.com/office365/securitycompliance/meet-data-protection-and-regulatory-reqs-using-microsoft-cloud) | Kuruluşunuzun mevzuata uygunluk etkinliklerini izleme, atama ve doğrulama
[Office 365 Güvenlik & Uyum Merkezi](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d) | Veri yönetimini yönetme<br>Yasal ve veri araştırması yapın<br>Veri Konu İsteğini Yönetme<br><br>Bu rol, Office 365 Güvenlik & Uyumluluk Merkezi rol tabanlı erişim denetiminde [Uyumluluk Veri Yöneticisi RoleGroup](https://docs.microsoft.com/microsoft-365/security/office-365-security/permissions-in-the-security-and-compliance-center#permissions-needed-to-use-features-in-the-security--compliance-center) ile aynı izinlere sahiptir.
[Intune](https://docs.microsoft.com/intune/role-based-access-control) | Tüm Intune denetim verilerini görüntüleme
[Cloud App Security](https://docs.microsoft.com/cloud-app-security/manage-admins) | Salt okunur izinleri vardır ve uyarıları yönetebilir<br>Dosya ilkeleri oluşturabilir ve değiştirebilir ve dosya yönetimi eylemlerine izin verebilir<br>Veri Yönetimi kapsamındaki tüm yerleşik raporları görüntüleyebilir

### <a name="conditional-access-administrator"></a>[Koşullu Erişim Yöneticisi](#conditional-access-administrator-permissions)

Bu role sahip kullanıcılar Azure Etkin Dizin Koşullu Erişim ayarlarını yönetebilir.
> [!NOTE]
> Azure'da Exchange ActiveSync Koşullu Erişim ilkesini dağıtmak için kullanıcının aynı zamanda bir Global Administrator olması gerekir.

### <a name="customer-lockbox-access-approver"></a>[Müşteri Lockbox erişim onaylayıcısı](#customer-lockbox-access-approver-permissions)

Kuruluşunuzdaki [Müşteri Kilit Kutusu isteklerini](https://docs.microsoft.com/office365/admin/manage/customer-lockbox-requests) yönetir. Müşteri Kilit Kutusu istekleri için e-posta bildirimleri alırlar ve Microsoft 365 yönetici merkezinden gelen istekleri onaylayabilir ve reddedebilir. Ayrıca Müşteri Kilit Kutusu özelliğini açabilir veya kapatabilir. Yalnızca genel yöneticiler bu role atanan kişilerin parolalarını sıfırlayabilir.

### <a name="desktop-analytics-administrator"></a>[Masaüstü Analiz Yöneticisi](#desktop-analytics-administrator-permissions)


Bu roldeki kullanıcılar Masaüstü Analizi ve Office Özelleştirme & İlke hizmetlerini yönetebilir. Masaüstü Analitiği için bu, varlık envanterini görüntüleme, dağıtım planları oluşturma, dağıtımı ve sistem durumu durumunu görüntüleme olanağını içerir. Office Özelleştirme & İlke hizmeti için bu rol, kullanıcıların Office ilkelerini yönetmesine olanak tanır.

### <a name="device-administrator"></a>[Aygıt Yöneticisi](#device-administrators-permissions)

Bu rol yalnızca [Aygıt ayarlarında](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/DevicesMenuBlade/DeviceSettings/menuId/)ek bir yerel yönetici olarak atama için kullanılabilir. Bu role sahip kullanıcılar, Azure Etkin Dizini'ne katılan tüm Windows 10 aygıtlarında yerel makine yöneticileri olur. Azure Active Directory'deki aygıt nesnelerini yönetme yeteneğine sahip değiller.

### <a name="directory-readers"></a>[Dizin Okuyucular](#directory-readers-permissions)

Bu roldeki kullanıcılar temel dizin bilgilerini okuyabilir. Bu rol için kullanılmalıdır:
* Belirli bir konuk kullanıcı kümesinin tüm konuk kullanıcılara vermek yerine erişimi okumasını sağlamak.
* "Azure AD portalına erişimi yalnızca yöneticilerle kısıtlayın" "Evet" olarak ayarlandığında, belirli bir yönetici olmayan kullanıcı kümesinin Azure portalına erişmelerine izin verilir.
* Hizmet müdürlerine Directory.Read.All'ın bir seçenek olmadığı dizine erişim izni verilmesi.

### <a name="directory-synchronization-accounts"></a>[Dizin Senkronizasyon Hesapları](#directory-synchronization-accounts-permissions)

Kullanmayın. Bu rol otomatik olarak Azure AD Connect hizmetine atanır ve başka bir kullanım için tasarlanmaz veya desteklenmez.

### <a name="directory-writers"></a>[Dizin Yazarlar](#directory-writers-permissions)

Bu, [Onay Çerçevesini](../develop/quickstart-register-app.md)desteklemeyen uygulamalara atanacak eski bir roldür. Herhangi bir kullanıcıya atanmamalıdır.

### <a name="dynamics-365-administrator--crm-administrator"></a>[Dynamics 365 yöneticisi / CRM Yöneticisi](#crm-service-administrator-permissions)

Bu role sahip kullanıcılar, hizmetin mevcut olduğu Microsoft Dynamics 365 Online'da genel izinlere ve destek biletlerini yönetme ve hizmet durumunu izleme olan genel izinlere sahiptir. Daha fazla bilgi: [Kiracınızı yönetmek için hizmet yöneticisi rolünü kullanın.](https://docs.microsoft.com/dynamics365/customer-engagement/admin/use-service-admin-role-manage-tenant)

> [!NOTE]
> Microsoft Graph API ve Azure AD PowerShell'de bu rol "Dynamics 365 Service Administrator" olarak tanımlanır. [Azure portalında](https://portal.azure.com)yer alan "Dynamics 365 Administrator" (Dynamics 365 Administrator) portalıdır.

### <a name="exchange-administrator"></a>[Exchange Yöneticisi](#exchange-service-administrator-permissions)

Bu role sahip kullanıcılar, hizmet mevcut olduğunda Microsoft Exchange Online'da genel izinlere sahiptir. Ayrıca tüm Office 365 Grupları oluşturma ve yönetme, destek biletlerini yönetme ve hizmet sağlığını izleme yeteneğine sahiptir. [Office 365 yönetici rolleri hakkında](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d)daha fazla bilgi.

> [!NOTE]
> Microsoft Graph API ve Azure AD PowerShell'de bu rol "Exchange Service Administrator" olarak tanımlanır. [Azure portalındaki](https://portal.azure.com)"Exchange Administrator"dur. [Exchange yönetici merkezindeki "Exchange](https://go.microsoft.com/fwlink/p/?LinkID=529144)Online yöneticisi"dir.

### <a name="external-identity-provider-administrator"></a>[Dış Kimlik Sağlayıcı Yöneticisi](#external-identity-provider-administrator-permissions)

Bu yönetici, Azure Etkin Dizin kiracıları ile dış kimlik sağlayıcıları arasındaki federasyonu yönetir.Bu rol ile kullanıcılar yeni kimlik sağlayıcıları ekleyebilir ve kullanılabilir tüm ayarları (örneğin kimlik doğrulama yolu, hizmet kimliği, atanan anahtar kapsayıcıları) yapılandırabilir.Bu kullanıcı, kiracının dış kimlik sağlayıcılarının kimlik doğrulamalarına güvenmesini sağlayabilir.Son kullanıcı deneyimleri üzerindeki sonuç, kiracının türüne bağlıdır:

* Çalışanlar ve iş ortakları için Azure Active Directory kiracıları: Bir federasyonun eklenmesi (örneğin Gmail ile) henüz kullanılmayan tüm konuk davetlerini hemen etkileyecektir. Bkz. [B2B konuk kullanıcıları için bir kimlik sağlayıcısı olarak Google ekleme.](https://docs.microsoft.com/azure/active-directory/b2b/google-federation)
* Azure Active Directory B2C kiracıları: Bir federasyonun eklenmesi (örneğin, Facebook veya başka bir Azure AD kuruluşuyla) kimlik sağlayıcısı kullanıcı akışına seçenek olarak eklenine (yerleşik ilke olarak da adlandırılır) son kullanıcı akışlarını hemen etkilemez. Bkz. Bir Microsoft hesabını bir örnek için [kimlik sağlayıcısı olarak yapılandırma.](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-msa-app)Kullanıcı akışlarını değiştirmek için "B2C Kullanıcı Akışı Yöneticisi"nin sınırlı rolü gereklidir.

### <a name="global-administrator--company-administrator"></a>[Global Yönetici / Şirket Yöneticisi](#company-administrator-permissions)

Bu role sahip kullanıcılar Azure Active Directory'deki tüm yönetim özelliklerine ve Microsoft 365 güvenlik merkezi, Microsoft 365 uyumluluk merkezi, Exchange Online, SharePoint Online ve Skype for Business Online gibi Azure Active Directory kimliklerini kullanan hizmetlere erişebilir. Azure Etkin Dizin kiracısı için kaydolan kişi genel bir yönetici olur. Şirketinizde birden fazla global yönetici olabilir. Genel yöneticiler, tüm kullanıcıların ve diğer tüm yöneticilerin parolalarını sıfırlayabilir.

> [!NOTE]
> Microsoft Graph API ve Azure AD PowerShell'de bu rol "Şirket Yöneticisi" olarak tanımlanır. [Azure portalındaki](https://portal.azure.com)"Global Administrator"dur.
>
>

### <a name="global-reader"></a>[Küresel Okuyucu](#global-reader-permissions)

Bu roldeki kullanıcılar Microsoft 365 hizmetlerinde ayarları ve yönetim bilgilerini okuyabilir, ancak yönetim eylemlerinde görev alamaz. Global okuyucu, Global yöneticinin salt okunur muadilidir. Planlama, denetimler veya soruşturmalar için Global yönetici yerine Global okuyucu atayın. Global Yönetici rolünü atamadan işin yapılmasını kolaylaştırmak için Exchange Administrator gibi diğer sınırlı yönetici rolleriyle birlikte Global okuyucuyu kullanın. Global okuyucu, Microsoft 365 yönetici merkezi, Exchange yönetici merkezi, SharePoint yönetici merkezi, Takımlar yönetici merkezi, Güvenlik merkezi, Uyumluluk merkezi, Azure AD yönetici merkezi ve Aygıt Yönetimi yönetici merkezi ile çalışır.

> [!NOTE]
> Küresel okuyucu rolü şu anda birkaç sınırlamaları vardır -
>
>- [OneDrive yönetici merkezi](https://admin.onedrive.com/) - OneDrive yönetici merkezi Global okuyucu rolünü desteklemez.
>- [Azure AD portalı](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/AllApps/menuId/) - Global okuyucu, kurumsal bir uygulamanın sağlama modunu okuyamaz.
>- [M365 yönetici merkezi](https://admin.microsoft.com/Adminportal/Home#/homepage) - Global okuyucu müşteri kilit kutusu isteklerini okuyamaz. M365 Yönetici Merkezi'nin sol bölmesinde **Destek** altında **Müşteri kilit kutusu istekleri** sekmesini bulamazsınız.
>- [M365 Güvenlik merkezi](https://security.microsoft.com/homepage) - Global okuyucu duyarlılık ve bekletme etiketlerini okuyamaz. M365 Güvenlik merkezinin sol bölmesinde **Duyarlılık etiketleri,** **Bekletme etiketleri**ve **Etiket analizi** sekmeleri bulamazsınız.
>- [Office Security & Uyumluluk Merkezi](https://sip.protection.office.com/homepage) - Global okuyucu SCC denetim günlüklerini okuyamaz, içerik araması yapamaz veya Güvenli Puan'ı göreemez.
>- [Takımlar yönetici merkezi](https://admin.teams.microsoft.com) - Global okuyucu **Takımlar yaşam döngüsü,** **Analytics & raporları,** **IP telefon cihaz yönetimi** ve Uygulama **kataloğu**okuyamaz.
>- [Ayrıcalıklı Erişim Yönetimi (PAM)](https://docs.microsoft.com/office365/securitycompliance/privileged-access-management-overview) Global okuyucu rolünü desteklemez.
>- [Azure Bilgi Koruması](https://docs.microsoft.com/azure/information-protection/what-is-information-protection) - Global okuyucu yalnızca [merkezi raporlama için](https://docs.microsoft.com/azure/information-protection/reports-aip) ve Azure AD kuruluşunuz birleşik etiketleme [platformunda](https://docs.microsoft.com/azure/information-protection/faqs#how-can-i-determine-if-my-tenant-is-on-the-unified-labeling-platform)olmadığında desteklenir.
>
> Bu özellikler şu anda geliştirilmektedir.
>

### <a name="groups-administrator"></a>[Gruplar Yöneticisi](#groups-administrator-permissions)

Bu roldeki kullanıcılar gruplar ve adlandırma ve son kullanma ilkeleri gibi ayarlarını oluşturabilir/yönetebilir. Bu role bir kullanıcı atamanın, onlara Outlook'a ek olarak Takımlar, SharePoint, Yammer gibi çeşitli iş yüklerinde kiracıdaki tüm grupları yönetme olanağı sağladığını anlamak önemlidir. Ayrıca kullanıcı, Microsoft Yönetici Merkezi, Azure portalı gibi çeşitli yönetici portallarında çeşitli grup ayarlarının yanı sıra Takımlar ve SharePoint Yönetici Merkezleri gibi iş yüküne özel ayarları da yönetebilecektir.

### <a name="guest-inviter"></a>[Misafir Davetçi](#guest-inviter-permissions)

Bu roldeki kullanıcılar, Üyeler kullanıcı ayarını Hayır olarak ayarladığında Azure Active Directory B2B konuk kullanıcı davetlerini **yönetebilir.** [Azure AD B2B işbirliği hakkında](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b)B2B işbirliği hakkında daha fazla bilgi. Başka izinler içermez.

### <a name="helpdesk-administrator"></a>[Yardım Masası Yöneticisi](#helpdesk-administrator-permissions)

Bu role sahip kullanıcılar parolaları değiştirebilir, yenileme belirteçlerini geçersiz kılabilir, hizmet isteklerini yönetebilir ve hizmet durumunu izleyebilir. Yenileme belirteci geçersiz kısık, kullanıcıyı yeniden oturum açmaya zorlar. Yardım Masası yöneticileri parolaları sıfırlayabilir ve yalnızca yönetici olmayan veya yalnızca aşağıdaki roller atanan diğer kullanıcıların yenilenme belirteçlerini geçersiz kılabilir:

* Dizin Okuyucular
* Misafir Davetçi
* Yardım Masası Yöneticisi
* İleti Merkezi Okuyucu
* Raporlar Okuyucu

> [!IMPORTANT]
> Bu role sahip kullanıcılar, Azure Active Directory içinde ve dışında hassas veya özel bilgilere veya kritik yapılandırmaya erişimi olan kişilerin parolalarını değiştirebilir. Bir kullanıcının parolasını değiştirmek, kullanıcının kimliğini ve izinlerini varsayma yeteneği anlamına gelebilir. Örneğin:
>
>- Sahip oldukları uygulamaların kimlik bilgilerini yönetebilen Uygulama Kaydı ve Kurumsal Uygulama sahipleri. Bu uygulamaların Azure AD'de ayrıcalıklı izinleri olabilir ve Yardım Masası Yöneticilerine verilmeyen başka yerlerde. Bu yol sayesinde bir Yardım Masası Yöneticisi bir uygulama sahibinin kimliğini üstlenebilir ve daha sonra uygulama için kimlik bilgilerini güncelleştirerek ayrıcalıklı bir uygulamanın kimliğini üstlenebilir.
>- Azure'da hassas veya özel bilgilere veya kritik yapılandırmaya erişimi olabilecek Azure abonelik sahipleri.
>- Grup üyeliğini yönetebilen Güvenlik Grubu ve Office 365 Grup sahipleri. Bu gruplar, Azure AD'de ve başka yerlerde hassas veya özel bilgilere veya kritik yapılandırmaya erişim sağlayabilir.
>- Exchange Online, Office Security ve Compliance Center ve insan kaynakları sistemleri gibi Azure AD dışındaki diğer hizmetlerdeki yöneticiler.
>- Hassas veya özel bilgilere erişebilen yöneticiler, hukuk müşaviri ve insan kaynakları çalışanları gibi yönetici olmayan yöneticiler.

Yönetim izinlerinin kullanıcı alt kümeleri üzerinden devrinve bir kullanıcı alt kümesine ilkeler [uygulanması, Yönetim Birimleri (şimdi genel önizlemede)](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-administrative-units)ile mümkündür.

Bu rol daha önce [Azure portalında](https://portal.azure.com/)"Parola Yöneticisi" olarak adlandırılıyordu. Azure AD'deki "Yardım Masası Yöneticisi" adı artık Azure AD PowerShell ve Microsoft Graph API'deki adıyla eşleşiyor.

### <a name="intune-administrator"></a>[Intune Yöneticisi](#intune-service-administrator-permissions)

Bu role sahip kullanıcılar, hizmet mevcut olduğunda Microsoft Intune Online'da genel izinlere sahiptir. Ayrıca, bu rol, ilkeyi ilişkilendirmek ve gruplar oluşturmak ve yönetmek için kullanıcıları ve aygıtları yönetme yeteneğini de içerir. [Microsoft Intune ile Rol tabanlı yönetim denetimi (RBAC) hakkında](https://docs.microsoft.com/intune/role-based-access-control)daha fazla bilgi.

Bu rol tüm güvenlik gruplarını oluşturabilir ve yönetebilir. Ancak, Intune Admin'in Office grupları üzerinde yönetici hakları yoktur. Bu, yöneticinin kiracıdaki tüm Office gruplarının sahiplerini veya üyeliklerini güncelleştiremeyeceği anlamına gelir. Ancak, kendi son kullanıcı ayrıcalıklarının bir parçası olarak gelir oluşturduğu Office grubunu yönetebilir. Bu nedenle, oluşturduğu herhangi bir Office grubu (güvenlik grubu değil) 250'lik kotasına göre sayılmalıdır.

> [!NOTE]
> Microsoft Graph API ve Azure AD PowerShell'de bu rol "Intune Service Administrator" olarak tanımlanır. [Azure portalındaki](https://portal.azure.com)"Intune Administrator"dur.

### <a name="kaizala-administrator"></a>[Kaizala Yöneticisi](#kaizala-administrator-permissions)

Bu role sahip kullanıcılar, hizmet in sayılsa da Microsoft Kaizala'daki ayarları yönetmek için genel izinlere ve destek biletlerini yönetme ve hizmet durumunu izleme olana sahiptir. Ayrıca, kullanıcı Kaizala'nın Organizasyon üyeleri tarafından benimsenmesi & kullanımına ilişkin raporlara ve Kaizala eylemleri kullanılarak oluşturulan iş raporlarına erişebilir.

### <a name="license-administrator"></a>[Lisans Yöneticisi](#license-administrator-permissions)

Bu roldeki kullanıcılar, kullanıcılara, gruplara (grup tabanlı lisanslama kullanarak) lisans atamaları ekleyebilir, kaldırabilir ve güncelleyebilir ve kullanıcılarüzerindeki kullanım konumunu yönetebilir. Bu rol, abonelikleri satın alma veya yönetme, gruplar oluşturma veya yönetme veya kullanım konumu dışında kullanıcı oluşturma veya yönetme olanağı sağlamaz. Bu rolün destek biletlerini görüntüleme, oluşturma veya yönetme erişimi yoktur.

### <a name="message-center-privacy-reader"></a>[Mesaj Merkezi Gizlilik Okuyucu](#message-center-privacy-reader-permissions)

Bu roldeki kullanıcılar, veri gizliliği iletileri de dahil olmak üzere İleti Merkezi'ndeki tüm bildirimleri izleyebilir. İleti Merkezi Gizlilik Okuyucular veri gizliliği ile ilgili olanlar da dahil olmak üzere e-posta bildirimleri almak ve İleti Merkezi Tercihleri kullanarak aboneliklerini iptal edebilirsiniz. Yalnızca Global Administrator ve İleti Merkezi Gizlilik Okuyucusu veri gizliliği iletilerini okuyabilir. Ayrıca, bu rol grupları, etki alanlarını ve abonelikleri görüntüleme özelliğini içerir. Bu rolün hizmet isteklerini görüntüleme, oluşturma veya yönetme izni yoktur.

### <a name="message-center-reader"></a>[İleti Merkezi Okuyucu](#message-center-reader-permissions)

Bu roldeki kullanıcılar, Exchange, Intune ve Microsoft Teams gibi yapılandırılmış hizmetlerde kuruluşları için [Office 365 İleti merkezinde](https://support.office.com/article/Message-center-in-Office-365-38FB3333-BFCC-4340-A37B-DEDA509C2093) bildirimleri ve danışma durumu güncelleştirmelerini izleyebilir. İleti Merkezi Okuyucuları, gönderilerin, güncelleştirmelerin haftalık e-posta özetlerini alır ve Ileti Merkezi gönderilerini Office 365'te paylaşabilir. Azure AD'de, bu role atanan kullanıcılar yalnızca kullanıcılar ve gruplar gibi Azure AD hizmetlerinde salt okunur erişime sahip olur. Bu rolün destek biletlerini görüntüleme, oluşturma veya yönetme erişimi yoktur.

### <a name="office-apps-administrator"></a>[Ofis Uygulamaları Yöneticisi](#office-apps-administrator-permissions)

Bu roldeki kullanıcılar Office 365 uygulamalarının bulut ayarlarını yönetebilir. Buna bulut ilkelerini yönetme, self servis indirme yönetimi ve Office uygulamalarıyla ilgili raporu görüntüleme olanağı dahildir. Bu rol ayrıca destek biletlerini yönetme ve ana yönetici merkezi içindeki hizmet sağlığını izleme olanağı da verir. Bu role atanan kullanıcılar, Office uygulamalarındaki yeni özelliklerin iletişimini de yönetebilir. 

### <a name="partner-tier1-support"></a>[Ortak Tier1 Desteği](#partner-tier1-support-permissions)

Kullanmayın. Bu rol amortismana kaldırıldı ve gelecekte Azure AD'den kaldırılacak. Bu rol, az sayıda Microsoft yeniden satış ortağı tarafından kullanılmak üzere tasarlanmıştır ve genel kullanım için tasarlanmamıştır.

### <a name="partner-tier2-support"></a>[Ortak Tier2 Desteği](#partner-tier2-support-permissions)

Kullanmayın. Bu rol amortismana kaldırıldı ve gelecekte Azure AD'den kaldırılacak. Bu rol, az sayıda Microsoft yeniden satış ortağı tarafından kullanılmak üzere tasarlanmıştır ve genel kullanım için tasarlanmamıştır.

### <a name="password-administrator"></a>[Şifre Yöneticisi](#password-administrator-permissions)

Bu role sahip kullanıcıların parolaları yönetme yeteneği sınırlıdır. Bu rol, hizmet isteklerini yönetme veya hizmet durumunu izleme olanağı sağlamaz. Parola yöneticileri, yalnızca aşağıdaki rollerin yöneticisi olmayan veya üyesi olmayan diğer kullanıcıların parolalarını sıfırlayabilir:

* Dizin Okuyucular
* Misafir Davetçi
* Şifre Yöneticisi

### <a name="power-bi-administrator"></a>[Güç BI Yöneticisi](#power-bi-service-administrator-permissions)

Bu role sahip kullanıcılar, hizmet in sayılsa da Microsoft Power BI'de genel izinlere ve destek biletlerini yönetme ve hizmet durumunu izleme yeteneğine sahiptir. [Power BI yönetici rolünü anlama da](https://docs.microsoft.com/power-bi/service-admin-role)daha fazla bilgi .

> [!NOTE]
> Microsoft Graph API ve Azure AD PowerShell'de bu rol "Power BI Service Administrator" olarak tanımlanır. [Azure portalında](https://portal.azure.com)"Power BI Administrator" olur.

### <a name="power-platform-administrator"></a>[Güç Platformu Yöneticisi](#power-platform-administrator-permissions)

Bu roldeki kullanıcılar ortamlar, PowerApps, Akışlar, Veri KaybıÖnleme politikalarının tüm yönlerini oluşturabilir ve yönetebilir. Ayrıca, bu role sahip kullanıcılar destek biletlerini yönetebilir ve hizmet sağlığını izleyebilir.

### <a name="privileged-authentication-administrator"></a>[Ayrıcalıklı Kimlik Doğrulama Yöneticisi](#privileged-authentication-administrator-permissions)

Bu role sahip kullanıcılar, genel yöneticiler de dahil olmak üzere tüm kullanıcılar için parola olmayan kimlik bilgilerini ayarlayabilir veya sıfırlayabilir ve tüm kullanıcılar için parolaları güncelleyebilir. Ayrıcalıklı Kimlik Doğrulama Yöneticileri, kullanıcıları mevcut parola dışı kimlik bilgilerine (örneğin MFA, FIDO) karşı yeniden kaydolmaya ve 'cihazda MFA'yı hatırlayın' iptal ini iptal etmeye zorlayarak tüm kullanıcıların bir sonraki oturumunu MFA'ya gerekettirmeyi zorlayabilir.

### <a name="privileged-role-administrator"></a>[Ayrıcalıklı Rol Yöneticisi](#privileged-role-administrator-permissions)

Bu role sahip kullanıcılar, Azure Etkin Dizini'nin yanı sıra Azure AD Ayrıcalıklı Kimlik Yönetimi'nde rol atamalarını yönetebilir. Buna ek olarak, bu rol Ayrıcalıklı Kimlik Yönetimi ve idari birimlerin tüm yönleriyle yönetimi sağlar.

> [!IMPORTANT]
> Bu rol, Genel Yönetici rolü de dahil olmak üzere tüm Azure AD rolleri için atamaları yönetme olanağı verir. Bu rol, Azure AD'de kullanıcı oluşturma veya güncelleme gibi diğer ayrıcalıklı yetenekleri içermez. Ancak, bu role atanan kullanıcılar ek roller atayarak kendilerine veya başkalarına ek ayrıcalık lar verebilir.

### <a name="reports-reader"></a>[Raporlar Okuyucu](#reports-reader-permissions)

Bu role sahip kullanıcılar, Microsoft 365 yönetici merkezinde kullanım raporlama verilerini ve rapor panosunu ve Power BI'deki benimseme bağlam paketini görüntüleyebilir. Ayrıca, rol Azure AD'deki oturum açma raporlarına ve etkinliklerine ve Microsoft Graph raporlama API'si tarafından döndürülen verilere erişim sağlar. Raporlar Okuyucu rolüne atanan bir kullanıcı yalnızca ilgili kullanım ve benimseme ölçümlerine erişebilir. Ayarları yapılandırmak veya Exchange gibi ürüne özel yönetici merkezlerine erişmek için yönetici izinleri yoktur. Bu rolün destek biletlerini görüntüleme, oluşturma veya yönetme erişimi yoktur.

### <a name="search-administrator"></a>[Arama Yöneticisi](#search-administrator-permissions)

Bu roldeki kullanıcılar, Microsoft 365 yönetici merkezindeki tüm Microsoft Arama yönetimi özelliklerine tam erişime sahiptir. Arama Yöneticileri, Arama Yöneticileri ve Arama Düzenleyicisi rollerini kullanıcılara devredebilir ve yer imleri, Q&A ve konumlar gibi içerik oluşturabilir ve yönetebilir. Ayrıca, bu kullanıcılar ileti merkezini görüntüleyebilir, hizmet durumunu izleyebilir ve hizmet istekleri oluşturabilir.

### <a name="search-editor"></a>[Arama Düzenleyicisi](#search-editor-permissions)

Bu roldeki kullanıcılar, yer imleri, Q&As ve konumlar da dahil olmak üzere Microsoft 365 yönetici merkezinde Microsoft Arama için içerik oluşturabilir, yönetebilir ve silebilir.

### <a name="security-administrator"></a>[Güvenlik Yöneticisi](#security-administrator-permissions)

Bu role sahip kullanıcılar, Microsoft 365 güvenlik merkezi, Azure Etkin Dizin Kimlik Koruması, Azure Bilgi Koruması ve Office 365 Güvenlik & Uyumluluk Merkezi'nde güvenlikle ilgili özellikleri yönetme iznine sahiptir. Office 365 izinleri hakkında daha fazla bilgiyi [Office 365 Güvenlik & Uyumluluk Merkezi'ndeki İzinler'de](https://support.office.com/article/Permissions-in-the-Office-365-Security-Compliance-Center-d10608af-7934-490a-818e-e68f17d0e9c1)bulabilirsiniz.

İçindeki | Yapabilirim
--- | ---
[Microsoft 365 güvenlik merkezi](https://protection.office.com) | Microsoft 365 hizmetleri genelinde güvenlikle ilgili ilkeleri izleme<br>Güvenlik tehditlerini ve uyarıları yönetme<br>Raporları görüntüle
Kimlik Koruma Merkezi | Güvenlik Okuyucu rolünün tüm izinleri<br>Ayrıca, parolaları sıfırlama dışında tüm Kimlik Koruma Merkezi işlemlerini gerçekleştirebilme
[Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-configure) | Güvenlik Okuyucu rolünün tüm izinleri<br>Azure AD rol atamaları veya ayarlarını **yönetemiyor**
[Office 365 Güvenlik & Uyum Merkezi](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d) | Güvenlik ilkelerini yönetme<br>Güvenlik tehditlerini görüntüleme, araştırma ve yanıt verme<br>Raporları görüntüle
Azure Gelişmiş Tehdit Koruması | Şüpheli güvenlik etkinliğini izleme ve yanıtla
Windows Defender ATP ve EDR | Rolleri atama<br>Makine gruplarını yönetme<br>Son nokta tehdit algılama ve otomatik düzeltme yapılandırma<br>Uyarıları görüntüleme, araştırma ve yanıtla
[Intune](https://docs.microsoft.com/intune/role-based-access-control) | Kullanıcıyı, cihazı, kaydı, yapılandırmayı ve uygulama bilgilerini görüntüler<br>Intune'da değişiklik yapamaz
[Cloud App Security](https://docs.microsoft.com/cloud-app-security/manage-admins) | Yönetici ekleme, ilkeler ve ayarlar ekleme, günlükleri yükleme ve yönetim eylemleri gerçekleştirme
[Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles) | Güvenlik ilkelerini görüntüleyebilir, güvenlik durumlarını görüntüleyebilir, güvenlik ilkelerini edinebilir, uyarıları ve önerileri görüntüleyebilir, uyarıları ve önerileri reddedebilir
[Office 365 hizmet sağlığı](https://docs.microsoft.com/office365/enterprise/view-service-health) | Office 365 hizmetlerinin durumunu görüntüleme

### <a name="security-operator"></a>[Güvenlik operatörü](#security-operator-permissions)

Bu role sahip kullanıcılar uyarıları yönetebilir ve Microsoft 365 güvenlik merkezi, Azure Etkin Dizini, Kimlik Koruması, Ayrıcalıklı Kimlik Yönetimi ve Office 365 Güvenlik & Uyumluluk Merkezi'ndeki tüm bilgiler de dahil olmak üzere güvenlikle ilgili özellikler de dahil olmak üzere genel bilgilere erişebilir. Office 365 izinleri hakkında daha fazla bilgiyi [Office 365 Güvenlik & Uyumluluk Merkezi'ndeki İzinler'de](https://docs.microsoft.com/office365/securitycompliance/permissions-in-the-security-and-compliance-center)bulabilirsiniz.

İçindeki | Yapabilirim
--- | ---
[Microsoft 365 güvenlik merkezi](https://protection.office.com) | Güvenlik Okuyucu rolünün tüm izinleri<br>Güvenlik tehditleri uyarılarını görüntüleme, araştırma ve yanıt verme
Kimlik Koruma Merkezi | Güvenlik Okuyucu rolünün tüm izinleri<br>Ayrıca, parolaları sıfırlama dışında tüm Kimlik Koruma Merkezi işlemlerini gerçekleştirebilme
[Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-configure) | Güvenlik Okuyucu rolünün tüm izinleri
[Office 365 Güvenlik & Uyum Merkezi](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d) | Güvenlik Okuyucu rolünün tüm izinleri<br>Güvenlik uyarılarını görüntüleme, araştırma ve yanıtla
Windows Defender ATP ve EDR | Güvenlik Okuyucu rolünün tüm izinleri<br>Güvenlik uyarılarını görüntüleme, araştırma ve yanıtla
[Intune](https://docs.microsoft.com/intune/role-based-access-control) | Güvenlik Okuyucu rolünün tüm izinleri
[Cloud App Security](https://docs.microsoft.com/cloud-app-security/manage-admins) | Güvenlik Okuyucu rolünün tüm izinleri
[Office 365 hizmet sağlığı](https://docs.microsoft.com/office365/enterprise/view-service-health) | Office 365 hizmetlerinin durumunu görüntüleme

### <a name="security-reader"></a>[Güvenlik Okuyucu](#security-reader-permissions)

Bu role sahip kullanıcılar, Microsoft 365 güvenlik merkezi, Azure Etkin Dizin, Kimlik Koruması, Ayrıcalıklı Kimlik Yönetimi ve Azure Active Directory oturum açma raporlarını ve denetim günlüklerini okuma olanağı ve Office 365 Güvenlik & Uyumluluk Merkezi'ndeki tüm bilgiler de dahil olmak üzere güvenlikle ilgili özelliklere genel olarak salt okunur erişime sahiptir. Office 365 izinleri hakkında daha fazla bilgiyi [Office 365 Güvenlik & Uyumluluk Merkezi'ndeki İzinler'de](https://support.office.com/article/Permissions-in-the-Office-365-Security-Compliance-Center-d10608af-7934-490a-818e-e68f17d0e9c1)bulabilirsiniz.

İçindeki | Yapabilirim
--- | ---
[Microsoft 365 güvenlik merkezi](https://protection.office.com) | Microsoft 365 hizmetlerinde güvenlikle ilgili ilkeleri görüntüleme<br>Güvenlik tehditlerini ve uyarılarını görüntüleme<br>Raporları görüntüle
Kimlik Koruma Merkezi | Güvenlik özellikleri için tüm güvenlik raporlarını ve ayar bilgilerini okuyun<br><ul><li>Antispam<li>Şifreleme<li>Veri kaybıönleme<li>Kötü amaçlı yazılımdan koruma<li>Gelişmiş tehdit koruması<li>Kimlik avıönleme<li>Posta akışı kuralları
[Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-configure) | Azure AD Ayrıcalıklı Kimlik Yönetimi'nde ortaya çıkan tüm bilgilere salt okunur erişim: Azure AD rol atamaları ve güvenlik incelemeleri için ilkeler ve raporlar.<br>Azure AD Ayrıcalıklı Kimlik Yönetimi'ne **kaydolamaz** veya bu yönetimde herhangi bir değişiklik yapamaz. Ayrıcalıklı Kimlik Yönetimi portalında veya PowerShell üzerinden, bu roldeki bir kişi, kullanıcı uygunsa ek rolleri (örneğin, Global Admin veya Ayrıcalıklı Rol Yöneticisi) etkinleştirebilir.
[Office 365 Güvenlik & Uyum Merkezi](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d) | Güvenlik ilkelerini görüntüleme<br>Güvenlik tehditlerini görüntüleme ve araştırma<br>Raporları görüntüle
Windows Defender ATP ve EDR | Uyarıları görüntüleyin ve araştırın. Windows Defender ATP'de rol tabanlı erişim denetimini açtığınızda, Azure AD Security okuyucu rolü gibi salt okunur izinlere sahip kullanıcılar, Windows Defender ATP rolüne atanana kadar erişimlerini kaybeder.
[Intune](https://docs.microsoft.com/intune/role-based-access-control) | Kullanıcıyı, cihazı, kaydı, yapılandırmayı ve uygulama bilgilerini görüntüler. Intune’da değişiklik yapamaz.
[Cloud App Security](https://docs.microsoft.com/cloud-app-security/manage-admins) | Salt okunur izinleri vardır ve uyarıları yönetebilir
[Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles) | Önerileri ve uyarıları görüntüleyebilir, güvenlik ilkelerini görüntüleyebilir, güvenlik durumlarını görüntüleyebilir, ancak değişiklik yapamaz
[Office 365 hizmet sağlığı](https://docs.microsoft.com/office365/enterprise/view-service-health) | Office 365 hizmetlerinin durumunu görüntüleme

### <a name="service-support-administrator"></a>[Servis Destek Yöneticisi](#service-support-administrator-permissions)

Bu role sahip kullanıcılar Azure ve Office 365 hizmetleri için Microsoft ile destek istekleri açabilir ve [Azure portalıve](https://portal.azure.com) [Microsoft 365 yönetici merkezindeki](https://admin.microsoft.com)hizmet panosunu ve ileti merkezini görüntüleyebilir. [Yönetici rolleri hakkında](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d)daha fazla bilgi .

> [!NOTE]
> Daha önce, bu rolü ["Hizmet](https://portal.azure.com) Yöneticisi" Azure portalı ve [Microsoft 365 yönetici merkezi](https://admin.microsoft.com)olarak adlandırıldı. Microsoft Graph API, Azure AD Graph API ve Azure AD PowerShell'deki exsiting adı ile uyumlu hale getirmek için adını "Service Support Administrator" olarak yeniden adlandırdık.

### <a name="sharepoint-administrator"></a>[SharePoint Yöneticisi](#sharepoint-service-administrator-permissions)

Bu role sahip kullanıcılar, hizmet in sayılsa da Microsoft SharePoint Online'da genel izinlere ve tüm Office 365 Gruplarını oluşturma ve yönetme, destek biletlerini yönetme ve hizmet durumunu izleme olan herkese sahiptir. [Yönetici rolleri hakkında](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d)daha fazla bilgi .

> [!NOTE]
> Microsoft Graph API ve Azure AD PowerShell'de bu rol "SharePoint Service Administrator" olarak tanımlanır. [Azure portalındaki](https://portal.azure.com)"SharePoint Administrator"dir.

### <a name="skype-for-business--lync-administrator"></a>[Skype for Business / Lync Yöneticisi](#lync-service-administrator-permissions)

Bu role sahip kullanıcılar, hizmet mevcut olduğunda Microsoft Skype kurumsal olarak genel izinlere sahiptir ve Azure Active Directory'de Skype'a özgü kullanıcı özniteliklerini yönetir. Ayrıca, bu rol destek biletlerini yönetme ve hizmet sağlığını izleme ve Takımlar ve Skype kurumsal yönetici merkezine erişme olanağı verir. Hesap ayrıca Takımlar için lisanslı olmalıdır veya Teams PowerShell cmdlets çalıştıramaz. [Skype for Business yönetici rolü](https://support.office.com/article/about-the-skype-for-business-admin-role-aeb35bda-93fc-49b1-ac2c-c74fbeb737b5) ve Skype for [Business ve Microsoft Teams eklenti lisanslama](https://docs.microsoft.com/skypeforbusiness/skype-for-business-and-microsoft-teams-add-on-licensing/skype-for-business-and-microsoft-teams-add-on-licensing) da Ekipler lisanslama bilgileri hakkında daha fazla bilgi

> [!NOTE]
> Microsoft Graph API ve Azure AD PowerShell'de bu rol "Lync Service Administrator" olarak tanımlanır. [Azure portalında](https://portal.azure.com/)yer alan "Skype for Business Administrator" (Kurumsal Yönetici için Skype) portalıdır.

### <a name="teams-communications-administrator"></a>[Takımlar İletişim Yöneticisi](#teams-communications-administrator-permissions)

Bu roldeki kullanıcılar, Microsoft Teams iş yükünün ses & telefonla ilgili yönlerini yönetebilir. Buna telefon numarası atama, ses ve toplantı ilkeleri ve çağrı analizi araç kümesine tam erişim için yönetim araçları dahildir.

### <a name="teams-communications-support-engineer"></a>[Takımlar İletişim Destek Mühendisi](#teams-communications-support-engineer-permissions)

Bu roldeki kullanıcılar, Microsoft Teams & Skype for Business yönetici merkezindeki kullanıcı arama sorun giderme araçlarını kullanarak Skype & Skype for Business'taki iletişim sorunlarını giderebilir. Bu roldeki kullanıcılar, ilgili tüm katılımcılar için tam çağrı kaydı bilgilerini görüntüleyebilir. Bu rolün destek biletlerini görüntüleme, oluşturma veya yönetme erişimi yoktur.

### <a name="teams-communications-support-specialist"></a>[Ekipler İletişim Destek Uzmanı](#teams-communications-support-specialist-permissions)

Bu roldeki kullanıcılar, Microsoft Teams & Skype for Business yönetici merkezindeki kullanıcı arama sorun giderme araçlarını kullanarak Skype & Skype for Business'taki iletişim sorunlarını giderebilir. Bu roldeki kullanıcılar yalnızca baktıkları belirli kullanıcının aramasında kullanıcı ayrıntılarını görüntüleyebilir. Bu rolün destek biletlerini görüntüleme, oluşturma veya yönetme erişimi yoktur.

### <a name="teams-service-administrator"></a>[Takımlar Servis Yöneticisi](#teams-service-administrator-permissions)

Bu roldeki kullanıcılar, Microsoft Teams iş yükünün tüm yönlerini Skype for Business yönetici merkezi ve ilgili PowerShell modülleri & Microsoft Ekipleri aracılığıyla yönetebilir. Buna, diğer alanların yanı sıra, telefon, mesajlaşma, toplantılar ve ekiplerin kendileri ile ilgili tüm yönetim araçları dahildir. Bu rol ayrıca tüm Office 365 Grupları oluşturma ve yönetme, destek biletlerini yönetme ve hizmet sağlığını izleme olanağı da verir.

### <a name="user-administrator"></a>[Kullanıcı Yöneticisi](#user-administrator-permissions)

Bu role sahip kullanıcılar, kullanıcılar oluşturabilir ve bazı kısıtlamalarla kullanıcıların tüm yönlerini yönetebilir (aşağıya bakın) ve parola son kullanma ilkelerini güncelleyebilir. Ayrıca, bu role sahip kullanıcılar tüm grupları oluşturabilir ve yönetebilir. Bu rol aynı zamanda kullanıcı görünümlerini oluşturma ve yönetme, destek biletlerini yönetme ve hizmet durumunu izleme yeteneğini de içerir. Kullanıcı yöneticilerinin çoğu yönetici rolündeki kullanıcılar için bazı kullanıcı özelliklerini yönetme izni yoktur. Bu role sahip kullanıcının MFA'yı yönetmek için izinleri yoktur. Bu kısıtlamanın istisnası olan roller aşağıdaki tabloda listelenir.

| | |
| --- | --- |
|Genel izinler|<p>Kullanıcı ve gruplar oluşturma</p><p>Kullanıcı görünümleri oluşturma ve yönetme</p><p>Office destek biletlerini yönetme<p>Parola sona erme ilkelerini güncelleştirme|
|<p>Tüm yöneticiler de dahil olmak üzere tüm kullanıcılarda</p>|<p>Lisansları yönetme</p><p>Kullanıcı Ana Adı dışındaki tüm kullanıcı özelliklerini yönetme</p>
|Yalnızca yönetici olmayan veya aşağıdaki sınırlı yönetici rollerinden herhangi birinde olan kullanıcılarda:<ul><li>Dizin Okuyucular<li>Misafir Davetçi<li>Yardım Masası Yöneticisi<li>İleti Merkezi Okuyucu<li>Raporlar Okuyucu<li>Kullanıcı Yöneticisi|<p>Silme ve geri yükleme</p><p>Devre dışı ve etkinleştir</p><p>Yenileme Belirteçleri'ni geçersiz kılma</p><p>Kullanıcı Ana Adı da dahil olmak üzere tüm kullanıcı özelliklerini yönetme</p><p>Parola sıfırlama</p><p>Güncelleme (FIDO) aygıt anahtarları</p>|

> [!IMPORTANT]
> Bu role sahip kullanıcılar, Azure Active Directory içinde ve dışında hassas veya özel bilgilere veya kritik yapılandırmaya erişimi olan kişilerin parolalarını değiştirebilir. Bir kullanıcının parolasını değiştirmek, kullanıcının kimliğini ve izinlerini varsayma yeteneği anlamına gelebilir. Örneğin:
>
>- Sahip oldukları uygulamaların kimlik bilgilerini yönetebilen Uygulama Kaydı ve Kurumsal Uygulama sahipleri. Bu uygulamaların Azure AD'de ayrıcalıklı izinleri olabilir ve Kullanıcı Yöneticilerine verilmeyen başka yerlerde. Bu yol sayesinde Bir Kullanıcı Yöneticisi bir uygulama sahibinin kimliğini üstlenebilir ve daha sonra uygulama için kimlik bilgilerini güncelleştirerek ayrıcalıklı bir uygulamanın kimliğini üstlenebilir.
>- Azure'da hassas veya özel bilgilere veya kritik yapılandırmaya erişebilecek azure abonelik sahipleri.
>- Grup üyeliğini yönetebilen Güvenlik Grubu ve Office 365 Grup sahipleri. Bu gruplar, Azure AD'de ve başka yerlerde hassas veya özel bilgilere veya kritik yapılandırmaya erişim sağlayabilir.
>- Exchange Online, Office Security ve Compliance Center ve insan kaynakları sistemleri gibi Azure AD dışındaki diğer hizmetlerdeki yöneticiler.
>- Hassas veya özel bilgilere erişebilen yöneticiler, hukuk müşaviri ve insan kaynakları çalışanları gibi yönetici olmayan yöneticiler.

## <a name="role-permissions"></a>Rol İzinleri

Aşağıdaki tablolar, her role verilen Azure Etkin Dizini'ndeki belirli izinleri açıklar. Bazı rollerin Azure Etkin Dizini dışında Microsoft hizmetlerinde ek izinleri olabilir.

### <a name="application-administrator-permissions"></a>Uygulama Yöneticisi izinleri

Uygulama kayıtlarının ve kurumsal uygulamaların tüm yönlerini oluşturabilir ve yönetebilirsiniz.

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.directory/Application/appProxyAuthentication/update | Azure Active Directory'deki hizmet ilkelerinde App Proxy kimlik doğrulama özelliklerini güncelleştirin. |
| microsoft.directory/Application/appProxyUrlSettings/update | Azure Active Directory'de uygulama proxy'si dahili ve harici URL'lerini güncelleştirin. |
| microsoft.directory/applications/applicationProxy/oku | Tüm App Proxy özelliklerini okuyun. |
| microsoft.directory/applications/applicationProxy/update | Tüm App Proxy özelliklerini güncelleştirin. |
| microsoft.directory/applications/audience/update | Azure Etkin Dizini'nde uygulamaları.hedef kitle özelliğini güncelleştirin. |
| microsoft.directory/applications/authentication/update | Azure Active Directory'deki uygulamalar.kimlik doğrulama özelliğini güncelleştirin. |
| microsoft.directory/applications/basic/update | Azure Etkin Dizini'ndeki uygulamalardaki temel özellikleri güncelleştirin. |
| microsoft.directory/applications/create | Azure Active Directory'de uygulamalar oluşturun. |
| microsoft.directory/applications/credentials/update | Azure Active Directory'deki applications.credentials özelliğini güncelleştirin. |
| microsoft.directory/applications/delete | Azure Etkin Dizini'ndeki uygulamaları silin. |
| microsoft.directory/applications/owners/update | Azure Active Directory'deki applications.owners özelliğini güncelleştirin. |
| microsoft.directory/applications/permissions/update | Azure Active Directory'deki uygulamalar.izinözelliğini güncelleştirin. |
| microsoft.directory/applications/policies/update | Azure Active Directory'deki applications.policies özelliğini güncelleştirin. |
| microsoft.directory/appRoleAssignments/create | Azure Etkin Dizini'nde uygulamaRol Atamaları oluşturun. |
| microsoft.directory/appRoleAssignments/read | Azure Etkin Dizini'nde uygulamaRol Atamaları'nı okuyun. |
| microsoft.directory/appRoleAssignments/update | Azure Etkin Dizini'nde uygulamaRol Atamaları'nı güncelleştirin. |
| microsoft.directory/appRoleAssignments/delete | Azure Etkin Dizini'ndeki uygulamaRol Atamaları'nı silin. |
| microsoft.directory/auditLogs/allProperties/read | Azure Etkin Dizini'ndeki auditLogs'daki tüm özellikleri (ayrıcalıklı özellikler dahil) okuyun. |
| microsoft.directory/connectorGroups/everything/read | Azure Active Directory'de uygulama proxy bağlayıcı grup özelliklerini okuyun. |
| microsoft.directory/connectorGroups/everything/update | Azure Active Directory'deki tüm uygulama proxy bağlayıcı grup özelliklerini güncelleştirin. |
| microsoft.directory/connectorGroups/create | Azure Etkin Dizini'nde uygulama proxy bağlayıcı grupları oluşturun. |
| microsoft.directory/connectorGroups/delete | Azure Etkin Dizini'ndeki uygulama proxy bağlayıcı gruplarını silin. |
| microsoft.directory/connectors/everything/read | Azure Active Directory'deki tüm uygulama proxy bağlayıcı özelliklerini okuyun. |
| microsoft.directory/connectors/create | Azure Active Directory'de uygulama proxy bağlayıcıları oluşturun. |
| microsoft.directory/policies/applicationConfiguration/basic/read | Azure Active Directory'de ilkeler.applicationConfiguration özelliğini okuyun. |
| microsoft.directory/policies/applicationConfiguration/basic/update | İlkeleri güncelleştirin.applicationConfiguration özelliğiAzure Active Directory'de. |
| microsoft.directory/policies/applicationConfiguration/create | Azure Etkin Dizini'nde ilkeler oluşturun. |
| microsoft.directory/policies/applicationConfiguration/delete | Azure Etkin Dizini'ndeki ilkeleri silin. |
| microsoft.directory/policies/applicationConfiguration/owners/read | Azure Active Directory'de ilkeler.applicationConfiguration özelliğini okuyun. |
| microsoft.directory/policies/applicationConfiguration/owners/update | İlkeleri güncelleştirin.applicationConfiguration özelliğiAzure Active Directory'de. |
| microsoft.directory/policies/applicationConfiguration/policyAppliedTo/read | Azure Active Directory'de ilkeler.applicationConfiguration özelliğini okuyun. |
| microsoft.directory/servicePrincipals/appRoleAssignedTo/update | HizmetPrincipals.appRoleAssignedTo özelliğini Azure Etkin Dizini'nde güncelleştirin. |
| microsoft.directory/servicePrincipals/appRoleAssignments/update | Azure Active Directory'de servicePrincipals.appRoleAssignments özelliğini güncelleştirin. |
| microsoft.directory/servicePrincipals/audience/update | HizmetPrincipals.audience özelliğini Azure Etkin Dizini'nde güncelleştirin. |
| microsoft.directory/servicePrincipals/authentication/update | Azure Etkin Dizini'nde hizmetPrincipals.authentication özelliğini güncelleştirin. |
| microsoft.directory/servicePrincipals/basic/update | Azure Etkin Dizini'ndeki hizmet Müdürleri'ndeki temel özellikleri güncelleştirin. |
| microsoft.directory/servicePrincipals/create | Azure Etkin Dizini'nde hizmet Müdürleri oluşturun. |
| microsoft.directory/servicePrincipals/credentials/update | Azure Etkin Dizini'nde servicePrincipals.credentials özelliğini güncelleştirin. |
| microsoft.directory/servicePrincipals/delete | Azure Etkin Dizini'ndeki hizmet Müdürlerini silin. |
| microsoft.directory/servicePrincipals/owners/update | HizmetPrincipals.owners özelliğini Azure Etkin Dizini'nde güncelleştirin. |
| microsoft.directory/servicePrincipals/permissions/update | Azure Etkin Dizini'nde hizmetPrincipals.permissions özelliğini güncelleştirin. |
| microsoft.directory/servicePrincipals/policies/update | Azure Etkin Dizini'nde hizmetPrincipals.policies özelliğini güncelleştirin. |
| microsoft.directory/signInReports/allProperties/read | Azure Active Directory'de signInReports'daki tüm özellikleri (ayrıcalıklı özellikler dahil) okuyun. |
| microsoft.azure.serviceHealth/allEntities/allGörevler | Azure Hizmet Durumu'nun okunmasını ve yapılandırılsın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure destek biletleri oluşturun ve yönetin. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nun okunmasını ve yapılandırılsın. |
| microsoft.office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |

### <a name="application-developer-permissions"></a>Uygulama Geliştiricisi izinleri

'Kullanıcılar uygulamaları kaydedebilir' ayarından bağımsız olarak uygulama kayıtları oluşturabilir.

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.directory/applications/createAsOwner | Azure Active Directory'de uygulamalar oluşturun. Oluşturan ilk sahibi olarak eklenir ve oluşturulan nesne oluşturan 250 oluşturulan nesneler kotakarşı sayar. |
| microsoft.directory/appRoleAssignments/createAsOwner | Azure Etkin Dizini'nde uygulamaRol Atamaları oluşturun. Oluşturan ilk sahibi olarak eklenir ve oluşturulan nesne oluşturan 250 oluşturulan nesneler kotakarşı sayar. |
| microsoft.directory/oAuth2PermissionGrants/createAsOwner | Azure Active Directory'de oAuth2PermissionGrants oluşturun. Oluşturan ilk sahibi olarak eklenir ve oluşturulan nesne oluşturan 250 oluşturulan nesneler kotakarşı sayar. |
| microsoft.directory/servicePrincipals/createAsOwner | Azure Etkin Dizini'nde hizmet Müdürleri oluşturun. Oluşturan ilk sahibi olarak eklenir ve oluşturulan nesne oluşturan 250 oluşturulan nesneler kotakarşı sayar. |

### <a name="authentication-administrator-permissions"></a>Kimlik Doğrulama Yöneticisi izinleri

Yönetici olmayan herhangi bir kullanıcı için kimlik doğrulama yöntemi bilgilerini görüntülemeye, ayarlamasına ve sıfırlamasına izin verilir.

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.directory/users/invalidateAllRefreshTokens | Azure Etkin Dizin'inde tüm kullanıcı yenileme belirteçlerini geçersiz kılın. |
| microsoft.directory/users/strongAuthentication/update | MFA kimlik bilgileri gibi güçlü kimlik doğrulama özelliklerini güncelleştirin. |
| microsoft.azure.serviceHealth/allEntities/allGörevler | Azure Hizmet Durumu'nun okunmasını ve yapılandırılsın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure destek biletleri oluşturun ve yönetin. |
| microsoft.office365.webPortal/allEntities/basic/read | microsoft.office365.webPortal'daki tüm kaynaklardaki temel özellikleri okuyun. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nun okunmasını ve yapılandırılsın. |
| microsoft.office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |
| microsoft.directory/users/password/update | Office 365 kuruluşundaki tüm kullanıcıların parolalarını güncelleştirin. Daha fazla ayrıntı için çevrimiçi belgelere bakın. |

### <a name="azure-devops-administrator-permissions"></a>Azure DevOps Yönetici izinleri

Azure DevOps kuruluş ilkesini ve ayarlarını yönetebilirsiniz.

> [!NOTE]
> Bu rolün Azure Etkin Dizini dışında ek izinleri vardır. Daha fazla bilgi için yukarıdaki [rol açıklamasına](#azure-devops-administrator) bakın.
>
>

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.azure.devOps/allEntities/allGörevler | Azure DevOps'leri okuyun ve yapılandırır. |

### <a name="azure-information-protection-administrator-permissions"></a>Azure Bilgi Koruma Yöneticisi izinleri

Azure Bilgi Koruması hizmetinin tüm yönlerini yönetebilirsiniz.

> [!NOTE]
> Bu rolün Azure Etkin Dizini dışında ek izinleri vardır. Daha fazla bilgi için yukarıdaki [rol açıklamasına](#) bakın.
>
>

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.azure.informationProtection/allEntities/allGörevler | Azure Bilgi Koruması'nın tüm yönlerini yönetin. |
| microsoft.azure.serviceHealth/allEntities/allGörevler | Azure Hizmet Durumu'nun okunmasını ve yapılandırılsın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure destek biletleri oluşturun ve yönetin. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nun okunmasını ve yapılandırılsın. |
| microsoft.office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |

### <a name="b2c-user-flow-administrator-permissions"></a>B2C Kullanıcı Akışı Yöneticisi izinleri

Kullanıcı akışlarının tüm yönlerini oluşturun ve yönetin.

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.b2c/userFlows/allTasks | Azure Active Directory B2C'de kullanıcı akışlarını okuyun ve yapılandırır. |

### <a name="b2c-user-flow-attribute-administrator-permissions"></a>B2C Kullanıcı Akışı Özniteliği Yönetici izinleri

Tüm kullanıcı akışları için kullanılabilir öznitelik şemasını oluşturun ve yönetin.

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.b2c/userAttributes/allTasks | Azure Active Directory B2C'de kullanıcı özniteliklerini okuyun ve yapılandırır. |

### <a name="b2c-ief-keyset-administrator-permissions"></a>B2C IEF Keyset Yöneticisi izinleri

Kimlik Deneyimi Çerçevesi'nde federasyon ve şifreleme için sırları yönetin.

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.b2c/trustFramework/keySets/allTasks | Azure Active Directory B2C'de anahtar kümelerini okuyun ve yapılandırır. |

### <a name="b2c-ief-policy-administrator-permissions"></a>B2C IEF İlke Yöneticisi izinleri

Kimlik Deneyimi Çerçevesi'nde güven çerçevesi ilkeleri oluşturun ve yönetin.

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.b2c/trustFramework/policies/allTasks | Azure Active Directory B2C'de özel ilkeleri okuyun ve yapılandırır. |

### <a name="billing-administrator-permissions"></a>Fatura Yöneticisi izinleri

Ödeme bilgilerini güncelleştirme kullanabilirsiniz gibi ortak faturalandırma ile ilgili görevleri gerçekleştirebilirsiniz.

> [!NOTE]
> Bu rolün Azure Etkin Dizini dışında ek izinleri vardır. Daha fazla bilgi için yukarıdaki rol açıklamasına bakın.
>
>

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.directory/organization/basic/update | Azure Active Directory'deki kuruluştemel özelliklerini güncelleştirin. |
| microsoft.azure.serviceHealth/allEntities/allGörevler | Azure Hizmet Durumu'nun okunmasını ve yapılandırılsın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure destek biletleri oluşturun ve yönetin. |
| microsoft.commerce.billing/allEntities/allTasks | Office 365 faturalandırmasının tüm yönlerini yönetin. |
| microsoft.office365.webPortal/allEntities/basic/read | microsoft.office365.webPortal'daki tüm kaynaklardaki temel özellikleri okuyun. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nun okunmasını ve yapılandırılsın. |
| microsoft.office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |

### <a name="cloud-application-administrator-permissions"></a>Bulut Uygulama Yöneticisi izinleri

App Proxy dışındaki uygulama kayıtlarının ve kurumsal uygulamaların tüm yönlerini oluşturabilir ve yönetebilirsiniz.

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.directory/applications/audience/update | Azure Etkin Dizini'nde uygulamaları.hedef kitle özelliğini güncelleştirin. |
| microsoft.directory/applications/authentication/update | Azure Active Directory'deki uygulamalar.kimlik doğrulama özelliğini güncelleştirin. |
| microsoft.directory/applications/basic/update | Azure Etkin Dizini'ndeki uygulamalardaki temel özellikleri güncelleştirin. |
| microsoft.directory/applications/create | Azure Active Directory'de uygulamalar oluşturun. |
| microsoft.directory/applications/credentials/update | Azure Active Directory'deki applications.credentials özelliğini güncelleştirin. |
| microsoft.directory/applications/delete | Azure Etkin Dizini'ndeki uygulamaları silin. |
| microsoft.directory/applications/owners/update | Azure Active Directory'deki applications.owners özelliğini güncelleştirin. |
| microsoft.directory/applications/permissions/update | Azure Active Directory'deki uygulamalar.izinözelliğini güncelleştirin. |
| microsoft.directory/applications/policies/update | Azure Active Directory'deki applications.policies özelliğini güncelleştirin. |
| microsoft.directory/appRoleAssignments/create | Azure Etkin Dizini'nde uygulamaRol Atamaları oluşturun. |
| microsoft.directory/appRoleAssignments/update | Azure Etkin Dizini'nde uygulamaRol Atamaları'nı güncelleştirin. |
| microsoft.directory/appRoleAssignments/delete | Azure Etkin Dizini'ndeki uygulamaRol Atamaları'nı silin. |
| microsoft.directory/auditLogs/allProperties/read | Azure Etkin Dizini'ndeki auditLogs'daki tüm özellikleri (ayrıcalıklı özellikler dahil) okuyun. |
| microsoft.directory/policies/applicationConfiguration/create | Azure Etkin Dizini'nde ilkeler oluşturun. |
| microsoft.directory/policies/applicationConfiguration/basic/read | Azure Active Directory'de ilkeler.applicationConfiguration özelliğini okuyun. |
| microsoft.directory/policies/applicationConfiguration/basic/update | İlkeleri güncelleştirin.applicationConfiguration özelliğiAzure Active Directory'de. |
| microsoft.directory/policies/applicationConfiguration/delete | Azure Etkin Dizini'ndeki ilkeleri silin. |
| microsoft.directory/policies/applicationConfiguration/owners/read | Azure Active Directory'de ilkeler.applicationConfiguration özelliğini okuyun. |
| microsoft.directory/policies/applicationConfiguration/owners/update | İlkeleri güncelleştirin.applicationConfiguration özelliğiAzure Active Directory'de. |
| microsoft.directory/policies/applicationConfiguration/policyAppliedTo/read | Azure Active Directory'de ilkeler.applicationConfiguration özelliğini okuyun. |
| microsoft.directory/servicePrincipals/appRoleAssignedTo/update | HizmetPrincipals.appRoleAssignedTo özelliğini Azure Etkin Dizini'nde güncelleştirin. |
| microsoft.directory/servicePrincipals/appRoleAssignments/update | Azure Active Directory'de servicePrincipals.appRoleAssignments özelliğini güncelleştirin. |
| microsoft.directory/servicePrincipals/audience/update | HizmetPrincipals.audience özelliğini Azure Etkin Dizini'nde güncelleştirin. |
| microsoft.directory/servicePrincipals/authentication/update | Azure Etkin Dizini'nde hizmetPrincipals.authentication özelliğini güncelleştirin. |
| microsoft.directory/servicePrincipals/basic/update | Azure Etkin Dizini'ndeki hizmet Müdürleri'ndeki temel özellikleri güncelleştirin. |
| microsoft.directory/servicePrincipals/create | Azure Etkin Dizini'nde hizmet Müdürleri oluşturun. |
| microsoft.directory/servicePrincipals/credentials/update | Azure Etkin Dizini'nde servicePrincipals.credentials özelliğini güncelleştirin. |
| microsoft.directory/servicePrincipals/delete | Azure Etkin Dizini'ndeki hizmet Müdürlerini silin. |
| microsoft.directory/servicePrincipals/owners/update | HizmetPrincipals.owners özelliğini Azure Etkin Dizini'nde güncelleştirin. |
| microsoft.directory/servicePrincipals/permissions/update | Azure Etkin Dizini'nde hizmetPrincipals.permissions özelliğini güncelleştirin. |
| microsoft.directory/servicePrincipals/policies/update | Azure Etkin Dizini'nde hizmetPrincipals.policies özelliğini güncelleştirin. |
| microsoft.directory/signInReports/allProperties/read | Azure Active Directory'de signInReports'daki tüm özellikleri (ayrıcalıklı özellikler dahil) okuyun. |
| microsoft.azure.serviceHealth/allEntities/allGörevler | Azure Hizmet Durumu'nun okunmasını ve yapılandırılsın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure destek biletleri oluşturun ve yönetin. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nun okunmasını ve yapılandırılsın. |
| microsoft.office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |

### <a name="cloud-device-administrator-permissions"></a>Bulut Aygıt Yöneticisi izinleri

Azure AD'de yönetilen aygıtlara tam erişim.

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.directory/auditLogs/allProperties/read | Azure Etkin Dizini'ndeki auditLogs'daki tüm özellikleri (ayrıcalıklı özellikler dahil) okuyun. |
| microsoft.directory/devices/bitLockerRecoveryKeys/read | Azure Active Directory'de devices.bitLockerRecoveryKeys özelliğini okuyun. |
| microsoft.directory/devices/delete | Azure Etkin Dizini'ndeki aygıtları silin. |
| microsoft.directory/devices/devre dışı | Azure Etkin Dizini'ndeki aygıtları devre dışı katın. |
| microsoft.directory/devices/enable | Azure Active Directory'de aygıtları etkinleştirin. |
| microsoft.directory/signInReports/allProperties/read | Azure Active Directory'de signInReports'daki tüm özellikleri (ayrıcalıklı özellikler dahil) okuyun. |
| microsoft.azure.serviceHealth/allEntities/allGörevler | Azure Hizmet Durumu'nun okunmasını ve yapılandırılsın. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nun okunmasını ve yapılandırılsın. |

### <a name="company-administrator-permissions"></a>Şirket Yöneticisi izinleri

Azure AD kimliklerini kullanan Azure AD ve Microsoft hizmetlerinin tüm yönlerini yönetebilirsiniz. Bu rol, Genel Yönetici rolü olarak da bilinir. 

> [!NOTE]
> Bu rolün Azure Etkin Dizini dışında ek izinleri vardır. Daha fazla bilgi için yukarıdaki rol açıklamasına bakın.
>
>

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.cloudAppSecurity/allEntities/allTasks | Microsoft.aad.cloudAppSecurity'deki tüm kaynakları oluşturun ve silin ve standart özellikleri okuyun ve güncelleyin. |
| microsoft.directory/administrativeUnits/allProperties/allTasks | Azure Active Directory'deki tüm özellikleri okuyup güncelleştirin ve yönetim birimleri oluşturun ve silin. |
| microsoft.directory/applications/allProperties/allTasks | Uygulamaları oluşturun ve silin ve Azure Active Directory'deki tüm özellikleri okuyun ve güncelleştirin. |
| microsoft.directory/appRoleAssignments/allProperties/allTasks | UygulamaRol Atamaları oluşturun ve silin ve Azure Active Directory'deki tüm özellikleri okuyun ve güncelleştirin. |
| microsoft.directory/auditLogs/allProperties/read | Azure Etkin Dizini'ndeki auditLogs'daki tüm özellikleri (ayrıcalıklı özellikler dahil) okuyun. |
| microsoft.directory/contacts/allProperties/allGörevler | Kişileri oluşturun ve silin ve Azure Active Directory'deki tüm özellikleri okuyun ve güncelleştirin. |
| microsoft.directory/contracts/allProperties/allTasks | Azure Active Directory'deki tüm özellikleri oluşturun ve silin ve güncelleştirin. |
| microsoft.directory/devices/allProperties/allGörevler | Azure Active Directory'deki tüm özellikleri oluşturun ve silin ve güncelleştirin. |
| microsoft.directory/directoryRoles/allProperties/allTasks | Azure Active Directory'deki tüm özellikleri oluşturun ve silin ve güncelleştirin. |
| microsoft.directory/directoryRoleTemplates/allProperties/allTasks | Rol Şablonları oluşturun ve silin ve Azure Etkin Dizini'ndeki tüm özellikleri okuyun ve güncelleştirin. |
| microsoft.directory/domains/allProperties/allTasks | Etki alanları oluşturun ve silin ve Azure Active Directory'deki tüm özellikleri okuyun ve güncelleştirin. |
| microsoft.directory/groups/allProperties/allTasks | Gruplar oluşturun ve silin ve Azure Active Directory'deki tüm özellikleri okuyun ve güncelleştirin. |
| microsoft.directory/groupAyarlar/allProperties/allGörevler | Grup Ayarları oluşturun ve silin ve Azure Active Directory'deki tüm özellikleri okuyun ve güncelleştirin. |
| microsoft.directory/groupSettingTemplates/allProperties/allTasks | GroupSettingTemplates oluşturun ve silin ve Azure Active Directory'deki tüm özellikleri okuyun ve güncelleştirin. |
| microsoft.directory/loginTenantBranding/allProperties/allTasks | GirişTenantBranding'i oluşturun ve silin ve Azure Active Directory'deki tüm özellikleri okuyun ve güncelleyin. |
| microsoft.directory/oAuth2PermissionGrants/allProperties/allTasks | oAuth2PermissionGrants oluşturun ve silin ve Azure Active Directory'deki tüm özellikleri okuyun ve güncelleştirin. |
| microsoft.directory/organization/allProperties/allTasks | Azure Active Directory'deki tüm özellikleri oluşturun ve silin ve güncelleştirin. |
| microsoft.directory/policies/allProperties/allTasks | İlkeler oluşturun ve silin ve Azure Active Directory'deki tüm özellikleri okuyun ve güncelleştirin. |
| microsoft.directory/roleAssignments/allProperties/allTasks | Rol Atamaları oluşturun ve silin ve Azure Active Directory'deki tüm özellikleri okuyun ve güncelleştirin. |
| microsoft.directory/roleDefinitions/allProperties/allTasks | Rol Tanımları oluşturun ve silin ve Azure Active Directory'deki tüm özellikleri okuyun ve güncelleştirin. |
| microsoft.directory/scopedRoleMemberships/allProperties/allTasks | Kapsamlı Rol Üyelikleri oluşturun ve silin ve Azure Active Directory'deki tüm özellikleri okuyun ve güncelleştirin. |
| microsoft.directory/serviceAction/activateService | Azure Etkin Dizini'nde Etkinleştirme hizmeti eylemini gerçekleştirebilir |
| microsoft.directory/serviceAction/disableDirectoryFeature | Azure Etkin Dizin'de Disabledirectoryfeature hizmet eylemini gerçekleştirebilir |
| microsoft.directory/serviceAction/enableDirectoryFeature | Azure Etkin Dizin'de Etkinleştirme Dizini özelliği eylemini gerçekleştirebilir |
| microsoft.directory/serviceAction/getAvailableExtentionProperties | Azure Etkin Dizin'de Getavailableextentionproperties hizmet eylemini gerçekleştirebilir |
| microsoft.directory/servicePrincipals/allProperties/allTasks | Hizmet Müdürler oluşturun ve silin ve Azure Etkin Dizini'ndeki tüm özellikleri okuyun ve güncelleştirin. |
| microsoft.directory/signInReports/allProperties/read | Azure Active Directory'de signInReports'daki tüm özellikleri (ayrıcalıklı özellikler dahil) okuyun. |
| microsoft.directory/subscribedSkus/allProperties/allTasks | SubscribedSkus'u oluşturun ve silin ve Azure Active Directory'deki tüm özellikleri okuyun ve güncelleştirin. |
| microsoft.directory/users/allProperties/allTasks | Azure Active Directory'deki tüm özellikleri oluşturun ve silin ve güncelleştirin. |
| microsoft.directorySync/allEntities/allGörevler | Azure AD Connect'te tüm eylemleri gerçekleştirin. |
| microsoft.aad.identityProtection/allEntities/allTasks | Microsoft.aad.identityProtection'daki tüm kaynakları oluşturun ve silin ve standart özellikleri okuyun ve güncelleyin. |
| microsoft.aad.privilegedIdentityManagement/allEntities/read | microsoft.aad.privilegedIdentityManagement adresindeki tüm kaynakları okuyun. |
| microsoft.azure.advancedThreatProtection/allEntities/read | microsoft.azure.advancedThreatProtection adresindeki tüm kaynakları okuyun. |
| microsoft.azure.informationProtection/allEntities/allGörevler | Azure Bilgi Koruması'nın tüm yönlerini yönetin. |
| microsoft.azure.serviceHealth/allEntities/allGörevler | Azure Hizmet Durumu'nun okunmasını ve yapılandırılsın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure destek biletleri oluşturun ve yönetin. |
| microsoft.commerce.billing/allEntities/allTasks | Office 365 faturalandırmasının tüm yönlerini yönetin. |
| microsoft.intune/allEntities/allTasks | Intune'un tüm yönlerini yönetin. |
| microsoft.office365.complianceManager/allEntities/allTasks | Office 365 Uyumluluk Yöneticisi'nin tüm yönlerini yönetme |
| microsoft.office365.desktopAnalytics/allEntities/allTasks | Desktop Analytics'in tüm yönlerini yönetin. |
| microsoft.office365.exchange/allEntities/allTasks | Exchange Online'ın tüm yönlerini yönetin. |
| microsoft.office365.lockbox/allEntities/allGörevler | Office 365 Müşteri Kilit Kutusu'nun tüm yönlerini yönetme |
| microsoft.office365.messageCenter/mesajlar/oku | microsoft.office365.messageCenter adresindeki iletileri okuyun. |
| microsoft.office365.messageCenter/securityMessages/read | microsoft.office365.messageCenter adresinden securityMessages'ı okuyun. |
| microsoft.office365.protectionCenter/allEntities/allTasks | Office 365 Koruma Merkezi'nin tüm yönlerini yönetin. |
| microsoft.office365.securityComplianceCenter/allEntities/allTasks | Microsoft.office365.securityComplianceCenter'daki tüm kaynakları oluşturun ve silin ve standart özellikleri okuyun ve güncelleyin. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nun okunmasını ve yapılandırılsın. |
| microsoft.office365.sharepoint/allEntities/allTasks | Microsoft.office365.sharepoint'teki tüm kaynakları oluşturun ve silin ve standart özellikleri okuyun ve güncelleyin. |
| microsoft.office365.skypeForBusiness/allEntities/allTasks | Skype for Business Online'ın tüm yönlerini yönetin. |
| microsoft.office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |
| microsoft.office365.usageReports/allEntities/read | Office 365 kullanım raporlarını okuyun. |
| microsoft.office365.webPortal/allEntities/basic/read | microsoft.office365.webPortal'daki tüm kaynaklardaki temel özellikleri okuyun. |
| microsoft.powerApps.dynamics365/allEntities/allTasks | Dynamics 365'in tüm yönlerini yönetin. |
| microsoft.powerApps.powerBI/allEntities/allTasks | Power BI'nin tüm yönlerini yönetin. |
| microsoft.windows.defenderAdvancedThreatProtection/allEntities/read | microsoft.windows.defenderAdvancedThreatProtection adresindeki tüm kaynakları okuyun. |

### <a name="compliance-administrator-permissions"></a>Uyumluluk Yöneticisi izinleri

Azure AD ve Office 365'te uyumluluk yapılandırmasını ve raporlarını okuyabilir ve yönetebilirsiniz.

> [!NOTE]
> Bu rolün Azure Etkin Dizini dışında ek izinleri vardır. Daha fazla bilgi için yukarıdaki rol açıklamasına bakın.
>
>

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allGörevler | Azure Hizmet Durumu'nun okunmasını ve yapılandırılsın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure destek biletleri oluşturun ve yönetin. |
| microsoft.office365.webPortal/allEntities/basic/read | microsoft.office365.webPortal'daki tüm kaynaklardaki temel özellikleri okuyun. |
| microsoft.office365.complianceManager/allEntities/allTasks | Office 365 Uyumluluk Yöneticisi'nin tüm yönlerini yönetme |
| microsoft.office365.exchange/allEntities/allTasks | Exchange Online'ın tüm yönlerini yönetin. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nun okunmasını ve yapılandırılsın. |
| microsoft.office365.sharepoint/allEntities/allTasks | Microsoft.office365.sharepoint'teki tüm kaynakları oluşturun ve silin ve standart özellikleri okuyun ve güncelleyin. |
| microsoft.office365.skypeForBusiness/allEntities/allTasks | Skype for Business Online'ın tüm yönlerini yönetin. |
| microsoft.office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |

### <a name="compliance-data-administrator-permissions"></a>Uyumluluk Veri Yöneticisi izinleri

Uyumluluk içeriğini oluşturur ve yönetir.

> [!NOTE]
> Bu rolün Azure Etkin Dizini dışında ek izinleri vardır. Daha fazla bilgi için yukarıdaki rol açıklamasına bakın.
>
>

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.cloudAppSecurity/allEntities/allTasks | Microsoft Cloud App Security'yi okuyun ve yapılandırır. |
| microsoft.azure.informationProtection/allEntities/allGörevler | Azure Bilgi Koruması'nın tüm yönlerini yönetin. |
| microsoft.azure.serviceHealth/allEntities/allGörevler | Azure Hizmet Durumu'nun okunmasını ve yapılandırılsın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure destek biletleri oluşturun ve yönetin. |
| microsoft.office365.webPortal/allEntities/basic/read | microsoft.office365.webPortal'daki tüm kaynaklardaki temel özellikleri okuyun. |
| microsoft.office365.complianceManager/allEntities/allTasks | Office 365 Uyumluluk Yöneticisi'nin tüm yönlerini yönetme |
| microsoft.office365.exchange/allEntities/allTasks | Exchange Online'ın tüm yönlerini yönetin. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nun okunmasını ve yapılandırılsın. |
| microsoft.office365.sharepoint/allEntities/allTasks | Microsoft.office365.sharepoint'teki tüm kaynakları oluşturun ve silin ve standart özellikleri okuyun ve güncelleyin. |
| microsoft.office365.skypeForBusiness/allEntities/allTasks | Skype for Business Online'ın tüm yönlerini yönetin. |
| microsoft.office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |

### <a name="conditional-access-administrator-permissions"></a>Koşullu Erişim Yöneticisi izinleri

Koşullu Erişim özelliklerini yönetebilir.

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.directory/policies/conditionalAccess/basic/read | Azure Active Directory'de ilkeler.conditionalAccess özelliğini okuyun. |
| microsoft.directory/policies/conditionalAccess/basic/update | İlkeleri güncelleştirin.Azure Etkin Dizini'ndeki koşulluErişim özelliği. |
| microsoft.directory/policies/conditionalAccess/create | Azure Etkin Dizini'nde ilkeler oluşturun. |
| microsoft.directory/policies/conditionalAccess/delete | Azure Etkin Dizini'ndeki ilkeleri silin. |
| microsoft.directory/policies/conditionalAccess/owners/read | Azure Active Directory'de ilkeler.conditionalAccess özelliğini okuyun. |
| microsoft.directory/policies/conditionalAccess/owners/update | İlkeleri güncelleştirin.Azure Etkin Dizini'ndeki koşulluErişim özelliği. |
| microsoft.directory/policies/conditionalAccess/policiesAppliedTo/read | Azure Active Directory'de ilkeler.conditionalAccess özelliğini okuyun. |
| microsoft.directory/policies/conditionalAccess/tenantDefault/update | İlkeleri güncelleştirin.Azure Etkin Dizini'ndeki koşulluErişim özelliği. |

### <a name="crm-service-administrator-permissions"></a>CRM Servis Yöneticisi izinleri

Dynamics 365 ürününün tüm yönlerini yönetebilirsiniz.

> [!NOTE]
> Bu rolün Azure Etkin Dizini dışında ek izinleri vardır. Daha fazla bilgi için yukarıdaki rol açıklamasına bakın.
>
>

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allGörevler | Azure Hizmet Durumu'nun okunmasını ve yapılandırılsın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure destek biletleri oluşturun ve yönetin. |
| microsoft.powerApps.dynamics365/allEntities/allTasks | Dynamics 365'in tüm yönlerini yönetin. |
| microsoft.office365.webPortal/allEntities/basic/read | microsoft.office365.webPortal'daki tüm kaynaklardaki temel özellikleri okuyun. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nun okunmasını ve yapılandırılsın. |
| microsoft.office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |

### <a name="customer-lockbox-access-approver-permissions"></a>Müşteri LockBox Erişim Onaylayıcısı izinleri

Müşteri kuruluş verilerine erişmek için Microsoft destek isteklerini onaylayabilir.

> [!NOTE]
> Bu rolün Azure Etkin Dizini dışında ek izinleri vardır. Daha fazla bilgi için yukarıdaki rol açıklamasına bakın.
>
>

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.office365.webPortal/allEntities/basic/read | microsoft.office365.webPortal'daki tüm kaynaklardaki temel özellikleri okuyun. |
| microsoft.office365.lockbox/allEntities/allGörevler | Office 365 Müşteri Kilit Kutusu'nun tüm yönlerini yönetme |

### <a name="desktop-analytics-administrator-permissions"></a>Masaüstü Analytics Yöneticisi izinleri

Masaüstü Analizi ve Office Özelleştirme & İlke hizmetlerini yönetebilir. Masaüstü Analitiği için bu, varlık envanterini görüntüleme, dağıtım planları oluşturma, dağıtımı ve sistem durumu durumunu görüntüleme olanağını içerir. Office Özelleştirme & İlke hizmeti için bu rol, kullanıcıların Office ilkelerini yönetmesine olanak tanır.

> [!NOTE]
> Bu rolün Azure Etkin Dizini dışında ek izinleri vardır. Daha fazla bilgi için yukarıdaki rol açıklamasına bakın.
>
>

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allGörevler | Azure Hizmet Durumu'nun okunmasını ve yapılandırılsın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure destek biletleri oluşturun ve yönetin. |
| microsoft.office365.webPortal/allEntities/basic/read | microsoft.office365.webPortal'daki tüm kaynaklardaki temel özellikleri okuyun. |
| microsoft.office365.desktopAnalytics/allEntities/allTasks | Desktop Analytics'in tüm yönlerini yönetin. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nun okunmasını ve yapılandırılsın. |
| microsoft.office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |

### <a name="device-administrators-permissions"></a>Aygıt Yöneticileri izinleri

Bu role atanan kullanıcılar, Azure AD'ye katılan aygıtlarda yerel yöneticiler grubuna eklenir.

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.directory/groupAyarlar/temel/okuma | Azure Etkin Dizini'nde grup Ayarları'ndaki temel özellikleri okuyun. |
| microsoft.directory/groupSettingTemplates/basic/read | Azure Etkin Dizini'nde groupSettingTemplates'teki temel özellikleri okuyun. |

### <a name="directory-readers-permissions"></a>Dizin Okuyucular izinleri
Temel dizin bilgilerini okuyabilir. Uygulamalara erişim izni vermek için, kullanıcılar için tasarlanmamıştır.

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.directory/administrativeUnits/basic/read | Azure Etkin Dizini'nde yönetimBirimleri'ndeki temel özellikleri okuyun. |
| microsoft.directory/administrativeUnits/members/read | Azure Active Directory'de administrativeUnits.members özelliğini okuyun. |
| microsoft.directory/applications/basic/read | Azure Etkin Dizini'ndeki uygulamalarla ilgili temel özellikleri okuyun. |
| microsoft.directory/applications/owners/read | Azure Active Directory'de applications.owners özelliğini okuyun. |
| microsoft.directory/applications/policies/read | Azure Active Directory'de applications.policies özelliğini okuyun. |
| microsoft.directory/contacts/basic/read | Azure Etkin Dizini'ndeki kişilerle ilgili temel özellikleri okuyun. |
| microsoft.directory/contacts/memberOf/read | kişiler.üyeAzure Active Directory'deki özelliği okuyun. |
| microsoft.directory/contracts/basic/read | Azure Active Directory'deki sözleşmelerdeki temel özellikleri okuyun. |
| microsoft.directory/devices/basic/read | Azure Active Directory'deki aygıtlarda temel özellikleri okuyun. |
| microsoft.directory/devices/memberOf/read | Devices.memberAzure Active Directory'deki özelliği okuyun. |
| microsoft.directory/devices/registeredOwners/read | Azure Active Directory'deki devices.registeredOwners özelliğini okuyun. |
| microsoft.directory/devices/registeredUsers/read | Azure Active Directory'de devices.registeredUsers özelliğini okuyun. |
| microsoft.directory/directoryRoles/basic/read | Azure Etkin Dizini'nde dizin rollerindeki temel özellikleri okuyun. |
| microsoft.directory/directoryRoles/eligibleÜyeler/oku | Azure Active Directory'de directoryRoles.eligibleMembers özelliğini okuyun. |
| microsoft.directory/directoryRoles/members/read | Azure Active Directory'de dizinRoles.members özelliğini okuyun. |
| microsoft.directory/domains/basic/read | Azure Etkin Dizin'deki etki alanlarındaki temel özellikleri okuyun. |
| microsoft.directory/groups/appRoleAssignments/read | Azure Etkin Dizini'nde groups.appRoleAssignments özelliğini okuyun. |
| microsoft.directory/groups/basic/read | Azure Etkin Dizini'ndeki gruplarla ilgili temel özellikleri okuyun. |
| microsoft.directory/groups/memberOf/read | Azure Active Directory'deki grupları okuyun.üyeÖzellik. |
| microsoft.directory/groups/members/read | Azure Etkin Dizini'nde groups.members özelliğini okuyun. |
| microsoft.directory/groups/owners/read | Azure Active Directory'de gruplar.sahipleri özelliğini okuyun. |
| microsoft.directory/groups/settings/read | Azure Etkin Dizini'nde gruplar.ayarlar özelliğini okuyun. |
| microsoft.directory/groupAyarlar/temel/okuma | Azure Etkin Dizini'nde grup Ayarları'ndaki temel özellikleri okuyun. |
| microsoft.directory/groupSettingTemplates/basic/read | Azure Etkin Dizini'nde groupSettingTemplates'teki temel özellikleri okuyun. |
| microsoft.directory/oAuth2PermissionGrants/basic/read | Azure Active Directory'deki oAuth2PermissionGrants'daki temel özellikleri okuyun. |
| microsoft.directory/organization/basic/read | Azure Active Directory'de kuruluşla ilgili temel özellikleri okuyun. |
| microsoft.directory/organization/trustedCAsForPasswordlessAuth/read | Azure Active Directory'deki organization.trustedCAsForPasswordlessAuth özelliğini okuyun. |
| microsoft.directory/roleAssignments/basic/read | Azure Etkin Dizini'nde rol Atamaları'ndaki temel özellikleri okuyun. |
| microsoft.directory/roleDefinitions/basic/read | Azure Etkin Dizini'nde roleDefinitions ile ilgili temel özellikleri okuyun. |
| microsoft.directory/servicePrincipals/appRoleAssignedTo/read | HizmetPrincipals.appRoleAssignedTo özelliğini Azure Active Directory'de okuyun. |
| microsoft.directory/servicePrincipals/appRoleAssignments/read | Azure Active Directory'de servicePrincipals.appRoleAssignments özelliğini okuyun. |
| microsoft.directory/servicePrincipals/basic/read | Azure Etkin Dizini'nde hizmet Müdürleri'ndeki temel özellikleri okuyun. |
| microsoft.directory/servicePrincipals/memberOf/read | Azure Active Directory'deki hizmetPrincipals.memberOf özelliğini okuyun. |
| microsoft.directory/servicePrincipals/oAuth2PermissionGrants/basic/read | Azure Active Directory'de hizmetPrincipals.oAuth2PermissionGrants özelliğini okuyun. |
| microsoft.directory/servicePrincipals/ownedObjects/read | HizmetPrincipals.ownedObjects özelliğini Azure Etkin Dizini'nde okuyun. |
| microsoft.directory/servicePrincipals/owners/read | Azure Active Directory'de hizmetPrincipals.owners özelliğini okuyun. |
| microsoft.directory/servicePrincipals/policies/read | Azure Etkin Dizini'nde servicePrincipals.policies özelliğini okuyun. |
| microsoft.directory/subscribedSkus/basic/read | Azure Active Directory'de abone skus'taki temel özellikleri okuyun. |
| microsoft.directory/users/appRoleAssignments/read | Azure Active Directory'de users.appRoleAssignments özelliğini okuyun. |
| microsoft.directory/users/basic/read | Azure Active Directory'deki kullanıcıların temel özelliklerini okuyun. |
| microsoft.directory/users/directReports/read | Azure Active Directory'de users.directReports özelliğini okuyun. |
| microsoft.directory/users/manager/read | Azure Active Directory'de users.manager özelliğini okuyun. |
| microsoft.directory/users/memberOf/read | Azure Active Directory'de users.memberOf özelliğini okuyun. |
| microsoft.directory/users/oAuth2PermissionGrants/basic/read | Azure Active Directory'de users.oAuth2PermissionGrants özelliğini okuyun. |
| microsoft.directory/users/ownedDevices/read | Azure Active Directory'de kullanıcıların sahip olduğu Cihazlar özelliğini okuyun. |
| microsoft.directory/users/ownedObjects/read | Azure Active Directory'de kullanıcıların sahip olduğu Objects özelliğini okuyun. |
| microsoft.directory/users/registeredDevices/read | Azure Active Directory'de users.registeredDevices özelliğini okuyun. |

### <a name="directory-synchronization-accounts-permissions"></a>Dizin Eşitleme Hesapları izinleri

Yalnızca Azure AD Connect hizmeti tarafından kullanılır.

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.directory/organization/dirSync/update | Azure Active Directory'de organization.dirSync özelliğini güncelleştirin. |
| microsoft.directory/policies/create | Azure Etkin Dizini'nde ilkeler oluşturun. |
| microsoft.directory/policies/delete | Azure Etkin Dizini'ndeki ilkeleri silin. |
| microsoft.directory/policies/basic/read | Azure Etkin Dizin'deki ilkelerle ilgili temel özellikleri okuyun. |
| microsoft.directory/policies/basic/update | Azure Etkin Dizini'ndeki ilkelerdeki temel özellikleri güncelleştirin. |
| microsoft.directory/policies/owners/read | Azure Active Directory'de ilkeler.sahipleri özelliğini okuyun. |
| microsoft.directory/policies/owners/update | İlkeleri güncelleştirin.sahipleri Azure Etkin Dizini'nde. |
| microsoft.directory/policies/policiesAppliedTo/read | İlkeleri okuyun.ilkelerAzure Active Directory'deki Uygulamalı özellik. |
| microsoft.directory/policies/tenantDefault/update | İlkeleri güncelleştirin.tenantAzure Active Directory'deki varsayılan özelliği. |
| microsoft.directory/servicePrincipals/appRoleAssignedTo/read | HizmetPrincipals.appRoleAssignedTo özelliğini Azure Active Directory'de okuyun. |
| microsoft.directory/servicePrincipals/appRoleAssignedTo/update | HizmetPrincipals.appRoleAssignedTo özelliğini Azure Etkin Dizini'nde güncelleştirin. |
| microsoft.directory/servicePrincipals/appRoleAssignments/read | Azure Active Directory'de servicePrincipals.appRoleAssignments özelliğini okuyun. |
| microsoft.directory/servicePrincipals/appRoleAssignments/update | Azure Active Directory'de servicePrincipals.appRoleAssignments özelliğini güncelleştirin. |
| microsoft.directory/servicePrincipals/audience/update | HizmetPrincipals.audience özelliğini Azure Etkin Dizini'nde güncelleştirin. |
| microsoft.directory/servicePrincipals/authentication/update | Azure Etkin Dizini'nde hizmetPrincipals.authentication özelliğini güncelleştirin. |
| microsoft.directory/servicePrincipals/basic/read | Azure Etkin Dizini'nde hizmet Müdürleri'ndeki temel özellikleri okuyun. |
| microsoft.directory/servicePrincipals/basic/update | Azure Etkin Dizini'ndeki hizmet Müdürleri'ndeki temel özellikleri güncelleştirin. |
| microsoft.directory/servicePrincipals/create | Azure Etkin Dizini'nde hizmet Müdürleri oluşturun. |
| microsoft.directory/servicePrincipals/credentials/update | Azure Etkin Dizini'nde servicePrincipals.credentials özelliğini güncelleştirin. |
| microsoft.directory/servicePrincipals/memberOf/read | Azure Active Directory'deki hizmetPrincipals.memberOf özelliğini okuyun. |
| microsoft.directory/servicePrincipals/oAuth2PermissionGrants/basic/read | Azure Active Directory'de hizmetPrincipals.oAuth2PermissionGrants özelliğini okuyun. |
| microsoft.directory/servicePrincipals/owners/read | Azure Active Directory'de hizmetPrincipals.owners özelliğini okuyun. |
| microsoft.directory/servicePrincipals/owners/update | HizmetPrincipals.owners özelliğini Azure Etkin Dizini'nde güncelleştirin. |
| microsoft.directory/servicePrincipals/ownedObjects/read | HizmetPrincipals.ownedObjects özelliğini Azure Etkin Dizini'nde okuyun. |
| microsoft.directory/servicePrincipals/permissions/update | Azure Etkin Dizini'nde hizmetPrincipals.permissions özelliğini güncelleştirin. |
| microsoft.directory/servicePrincipals/policies/read | Azure Etkin Dizini'nde servicePrincipals.policies özelliğini okuyun. |
| microsoft.directory/servicePrincipals/policies/update | Azure Etkin Dizini'nde hizmetPrincipals.policies özelliğini güncelleştirin. |
| microsoft.directorySync/allEntities/allGörevler | Azure AD Connect'te tüm eylemleri gerçekleştirin. |

### <a name="directory-writers-permissions"></a>Dizin Yazarlar izinleri

Temel dizin bilgilerini okuyabilir & yazabilirsiniz. Uygulamalara erişim izni vermek için, kullanıcılar için tasarlanmamıştır.

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.directory/groups/create | Azure Etkin Dizini'nde gruplar oluşturun. |
| microsoft.directory/groups/createAsOwner | Azure Etkin Dizini'nde gruplar oluşturun. Oluşturan ilk sahibi olarak eklenir ve oluşturulan nesne oluşturan 250 oluşturulan nesneler kotakarşı sayar. |
| microsoft.directory/groups/appRoleAssignments/update | Azure Etkin Dizini'nde grupları.appRoleAssignments özelliğini güncelleştirin. |
| microsoft.directory/groups/basic/update | Azure Etkin Dizini'ndeki gruplardaki temel özellikleri güncelleştirin. |
| microsoft.directory/groups/members/update | Grup.üyeler özelliğini Azure Etkin Dizini'nde güncelleştirin. |
| microsoft.directory/groups/owners/update | Grup.sahipleri azure Etkin Dizini'nde güncelleştirin. |
| microsoft.directory/groups/settings/update | Azure Etkin Dizini'nde groups.settings özelliğini güncelleştirin. |
| microsoft.directory/groupAyarlar/temel/güncelleştirme | Azure Etkin Dizini'nde grup Ayarları'ndaki temel özellikleri güncelleştirin. |
| microsoft.directory/groupAyarlar/oluştur | Azure Etkin Dizini'nde grup Ayarları oluşturun. |
| microsoft.directory/groupAyarlar/silme | Azure Etkin Dizini'nde grupAyarları silin. |
| microsoft.directory/users/appRoleAssignments/update | Azure Active Directory'deki kullanıcıları.appRoleAssignments özelliğini güncelleştirin. |
| microsoft.directory/users/atsignLicense | Azure Active Directory'deki kullanıcıların lisanslarını yönetin. |
| microsoft.directory/users/basic/update | Azure Active Directory'deki kullanıcıların temel özelliklerini güncelleştirin. |
| microsoft.directory/users/invalidateAllRefreshTokens | Azure Etkin Dizin'inde tüm kullanıcı yenileme belirteçlerini geçersiz kılın. |
| microsoft.directory/users/manager/update | Azure Active Directory'deki users.manager özelliğini güncelleştirin. |
| microsoft.directory/users/userPrincipalName/update | Kullanıcıları güncelleştirin.userPrincipalName özelliğini Azure Etkin Dizini'nde güncelleştirin. |

### <a name="exchange-service-administrator-permissions"></a>Exchange Service Administrator izinleri

Exchange ürününün tüm yönlerini yönetebilirsiniz.

> [!NOTE]
> Bu rolün Azure Etkin Dizini dışında ek izinleri vardır. Daha fazla bilgi için yukarıdaki rol açıklamasına bakın.
>
>

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allGörevler | Azure Hizmet Durumu'nun okunmasını ve yapılandırılsın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure destek biletleri oluşturun ve yönetin. |
| microsoft.directory/groups/unified/appRoleAssignments/update | Azure Etkin Dizin'deki grupları.birleşik özelliği güncelleştirin. |
| microsoft.directory/groups/unified/basic/update | Office 365 Grupları'nın temel özelliklerini güncelleştirin. |
| microsoft.directory/groups/unified/create | Office 365 Grupları oluşturun. |
| microsoft.directory/groups/unified/delete | Office 365 Gruplarını Silin. |
| microsoft.directory/groups/unified/members/update | Office 365 Grupları üyeliğini güncelleştirin. |
| microsoft.directory/groups/unified/owners/update | Office 365 Gruplarının sahipliğini güncelleştirin. |
| microsoft.office365.exchange/allEntities/allTasks | Exchange Online'ın tüm yönlerini yönetin. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nun okunmasını ve yapılandırılsın. |
| microsoft.office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |
| microsoft.office365.usageReports/allEntities/read | Office 365 kullanım raporlarını okuyun. |
| microsoft.office365.webPortal/allEntities/basic/read | microsoft.office365.webPortal'daki tüm kaynaklardaki temel özellikleri okuyun. |

### <a name="external-identity-provider-administrator-permissions"></a>Dış Kimlik Sağlayıcı Yönetici izinleri

Kimlik sağlayıcılarını doğrudan federasyonda kullanılmak üzere yapılandırın.

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.b2c/identityProviders/allTasks | Azure Active Directory B2C'de kimlik sağlayıcılarını okuyun ve yapılandırır. |

### <a name="global-reader-permissions"></a>Global Reader izinleri
Bir Global Yöneticinin okuyabileceği her şeyi okuyabilir, ancak hiçbir şeyi edinemez.

> [!NOTE]
> Bu rolün Azure Etkin Dizini dışında ek izinleri vardır. Daha fazla bilgi için yukarıdaki [rol açıklamasına](#global-reader) bakın.
>
>

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.commerce.billing/allEntities/read    | Office 365 faturalandırmasının tüm yönlerini okuyun. |
| microsoft.directory/administrativeUnits/basic/read    | Azure Etkin Dizini'nde yönetimBirimleri'ndeki temel özellikleri okuyun. |
| microsoft.directory/administrativeUnits/members/read    | Azure Active Directory'de administrativeUnits.members özelliğini okuyun. |
| microsoft.directory/applications/basic/read    | Azure Etkin Dizini'ndeki uygulamalarla ilgili temel özellikleri okuyun. |
| microsoft.directory/applications/owners/read    | Azure Active Directory'de applications.owners özelliğini okuyun. |
| microsoft.directory/applications/policies/read    | Azure Active Directory'de applications.policies özelliğini okuyun. |
| microsoft.directory/contacts/basic/read    | Azure Etkin Dizini'ndeki kişilerle ilgili temel özellikleri okuyun. |
| microsoft.directory/contacts/memberOf/read    | kişiler.üyeAzure Active Directory'deki özelliği okuyun. |
| microsoft.directory/contracts/basic/read    | Azure Active Directory'deki sözleşmelerdeki temel özellikleri okuyun. |
| microsoft.directory/devices/basic/read    | Azure Active Directory'deki aygıtlarda temel özellikleri okuyun. |
| microsoft.directory/devices/memberOf/read    | Devices.memberAzure Active Directory'deki özelliği okuyun. |
| microsoft.directory/devices/registeredOwners/read    | Azure Active Directory'deki devices.registeredOwners özelliğini okuyun. |
| microsoft.directory/devices/registeredUsers/read    | Azure Active Directory'de devices.registeredUsers özelliğini okuyun. |
| microsoft.directory/directoryRoles/basic/read    | Azure Etkin Dizini'nde dizin rollerindeki temel özellikleri okuyun. |
| microsoft.directory/directoryRoles/eligibleÜyeler/oku    | Azure Active Directory'de directoryRoles.eligibleMembers özelliğini okuyun. |
| microsoft.directory/directoryRoles/members/read    | Azure Active Directory'de dizinRoles.members özelliğini okuyun. |
| microsoft.directory/domains/basic/read    | Azure Etkin Dizin'deki etki alanlarındaki temel özellikleri okuyun. |
| microsoft.directory/groups/appRoleAssignments/read    | Azure Etkin Dizini'nde groups.appRoleAssignments özelliğini okuyun. |
| microsoft.directory/groups/basic/read    | Azure Etkin Dizini'ndeki gruplarla ilgili temel özellikleri okuyun. |
| microsoft.directory/groups/hiddenÜyeler/read    | Azure Etkin Dizini'nde grupları okuyun.hiddenMembers özelliği. |
| microsoft.directory/groups/memberOf/read    | Azure Active Directory'deki grupları okuyun.üyeÖzellik. |
| microsoft.directory/groups/members/read    | Azure Etkin Dizini'nde groups.members özelliğini okuyun. |
| microsoft.directory/groups/owners/read    | Azure Active Directory'de gruplar.sahipleri özelliğini okuyun. |
| microsoft.directory/groups/settings/read    | Azure Etkin Dizini'nde gruplar.ayarlar özelliğini okuyun. |
| microsoft.directory/groupAyarlar/temel/okuma    | Azure Etkin Dizini'nde grup Ayarları'ndaki temel özellikleri okuyun. |
| microsoft.directory/groupSettingTemplates/basic/read    | Azure Etkin Dizini'nde groupSettingTemplates'teki temel özellikleri okuyun. |
| microsoft.directory/oAuth2PermissionGrants/basic/read    | Azure Active Directory'deki oAuth2PermissionGrants'daki temel özellikleri okuyun. |
| microsoft.directory/organization/basic/read    | Azure Active Directory'de kuruluşla ilgili temel özellikleri okuyun. |
| microsoft.directory/organization/trustedCAsForPasswordlessAuth/read    | Azure Active Directory'deki organization.trustedCAsForPasswordlessAuth özelliğini okuyun. |
| microsoft.directory/policies/standard/read    | Azure Etkin Dizin'de standart ilkeleri okuyun. |
| microsoft.directory/roleAssignments/basic/read    | Azure Etkin Dizini'nde rol Atamaları'ndaki temel özellikleri okuyun. |
| microsoft.directory/roleDefinitions/basic/read    | Azure Etkin Dizini'nde roleDefinitions ile ilgili temel özellikleri okuyun. |
| microsoft.directory/servicePrincipals/appRoleAssignedTo/read    | HizmetPrincipals.appRoleAssignedTo özelliğini Azure Active Directory'de okuyun. |
| microsoft.directory/servicePrincipals/appRoleAssignments/read    | Azure Active Directory'de servicePrincipals.appRoleAssignments özelliğini okuyun. |
| microsoft.directory/servicePrincipals/basic/read    | Azure Etkin Dizini'nde hizmet Müdürleri'ndeki temel özellikleri okuyun. |
| microsoft.directory/servicePrincipals/memberOf/read    | Azure Active Directory'deki hizmetPrincipals.memberOf özelliğini okuyun. |
| microsoft.directory/servicePrincipals/oAuth2PermissionGrants/basic/read    | Azure Active Directory'de hizmetPrincipals.oAuth2PermissionGrants özelliğini okuyun. |
| microsoft.directory/servicePrincipals/ownedObjects/read    | HizmetPrincipals.ownedObjects özelliğini Azure Etkin Dizini'nde okuyun. |
| microsoft.directory/servicePrincipals/owners/read    | Azure Active Directory'de hizmetPrincipals.owners özelliğini okuyun. |
| microsoft.directory/servicePrincipals/policies/read    | Azure Etkin Dizini'nde servicePrincipals.policies özelliğini okuyun. |
| microsoft.directory/signInReports/allProperties/read    | Azure Active Directory'de signInReports'daki tüm özellikleri (ayrıcalıklı özellikler dahil) okuyun. |
| microsoft.directory/subscribedSkus/basic/read    | Azure Active Directory'de abone skus'taki temel özellikleri okuyun. |
| microsoft.directory/users/appRoleAssignments/read    | Azure Active Directory'de users.appRoleAssignments özelliğini okuyun. |
| microsoft.directory/users/basic/read    | Azure Active Directory'deki kullanıcıların temel özelliklerini okuyun. |
| microsoft.directory/users/directReports/read    | Azure Active Directory'de users.directReports özelliğini okuyun. |
| microsoft.directory/users/manager/read    | Azure Active Directory'de users.manager özelliğini okuyun. |
| microsoft.directory/users/memberOf/read    | Azure Active Directory'de users.memberOf özelliğini okuyun. |
| microsoft.directory/users/oAuth2PermissionGrants/basic/read    | Azure Active Directory'de users.oAuth2PermissionGrants özelliğini okuyun. |
| microsoft.directory/users/ownedDevices/read    | Azure Active Directory'de kullanıcıların sahip olduğu Cihazlar özelliğini okuyun. |
| microsoft.directory/users/ownedObjects/read    | Azure Active Directory'de kullanıcıların sahip olduğu Objects özelliğini okuyun. |
| microsoft.directory/users/registeredDevices/read    | Azure Active Directory'de users.registeredDevices özelliğini okuyun. |
| microsoft.directory/users/strongAuthentication/read    | MFA kimlik bilgileri gibi güçlü kimlik doğrulama özelliklerini okuyun. |
| microsoft.office365.exchange/allEntities/read    | Exchange Online'ın tüm yönlerini okuyun. |
| microsoft.office365.messageCenter/mesajlar/oku    | microsoft.office365.messageCenter adresindeki iletileri okuyun. |
| microsoft.office365.messageCenter/securityMessages/read    | microsoft.office365.messageCenter adresinden securityMessages'ı okuyun. |
| microsoft.office365.protectionCenter/allEntities/read    | Office 365 Koruma Merkezi'nin tüm yönlerini okuyun. |
| microsoft.office365.securityComplianceCenter/allEntities/read    | microsoft.office365.securityComplianceCenter'daki tüm standart özellikleri okuyun. |
| microsoft.office365.usageReports/allEntities/read    | Office 365 kullanım raporlarını okuyun. |
| microsoft.office365.webPortal/allEntities/standard/read    | microsoft.office365.webPortal'daki tüm kaynaklardaki standart özellikleri okuyun. |

### <a name="groups-administrator-permissions"></a>Grup Yöneticisi izinleri
Adlandırma ve son kullanma ilkeleri gibi grupların ve grup ayarlarının tüm yönlerini yönetebilirsiniz.

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.directory/groups/basic/read | Azure Etkin Dizini'ndeki Gruplar'daki standart özellikleri okuyun.  |
| microsoft.directory/groups/basic/update | Azure Etkin Dizini'ndeki gruplardaki temel özellikleri güncelleştirin. |
| microsoft.directory/groups/create | Azure Etkin Dizini'nde gruplar oluşturun. |
| microsoft.directory/groups/createAsOwner | Azure Etkin Dizini'nde gruplar oluşturun. Oluşturan ilk sahibi olarak eklenir ve oluşturulan nesne oluşturan 250 oluşturulan nesneler kotakarşı sayar. |
| microsoft.directory/groups/delete | Azure Etkin Dizini'ndeki grupları silin. |
| microsoft.directory/groups/hiddenÜyeler/read | Azure Etkin Dizini'nde grupları okuyun.hiddenMembers özelliği. |
| microsoft.directory/groups/members/update | Grup.üyeler özelliğini Azure Etkin Dizini'nde güncelleştirin. |
| microsoft.directory/groups/owners/update | Grup.sahipleri azure Etkin Dizini'nde güncelleştirin. |
| microsoft.directory/groups/geri yükleme | Azure Etkin Dizini'ndeki grupları geri yükleyin. |
| microsoft.directory/groups/settings/update | Azure Etkin Dizini'nde groups.settings özelliğini güncelleştirin. |
| microsoft.azure.serviceHealth/allEntities/allGörevler | Azure Hizmet Durumu'nun okunmasını ve yapılandırılsın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure destek biletleri oluşturun ve yönetin. |
| microsoft.office365.messageCenter/mesajlar/oku | microsoft.office365.messageCenter adresindeki iletileri okuyun. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nun okunmasını ve yapılandırılsın. |
| microsoft.office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |

### <a name="guest-inviter-permissions"></a>Misafir Davetli izinleri
'Üyeler misafir davet edebilir' ayarından bağımsız olarak konuk kullanıcıları davet edebilir.

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.directory/users/appRoleAssignments/read | Azure Active Directory'de users.appRoleAssignments özelliğini okuyun. |
| microsoft.directory/users/basic/read | Azure Active Directory'deki kullanıcıların temel özelliklerini okuyun. |
| microsoft.directory/users/directReports/read | Azure Active Directory'de users.directReports özelliğini okuyun. |
| microsoft.directory/users/inviteGuest | Azure Active Directory'deki konuk kullanıcıları davet edin. |
| microsoft.directory/users/manager/read | Azure Active Directory'de users.manager özelliğini okuyun. |
| microsoft.directory/users/memberOf/read | Azure Active Directory'de users.memberOf özelliğini okuyun. |
| microsoft.directory/users/oAuth2PermissionGrants/basic/read | Azure Active Directory'de users.oAuth2PermissionGrants özelliğini okuyun. |
| microsoft.directory/users/ownedDevices/read | Azure Active Directory'de kullanıcıların sahip olduğu Cihazlar özelliğini okuyun. |
| microsoft.directory/users/ownedObjects/read | Azure Active Directory'de kullanıcıların sahip olduğu Objects özelliğini okuyun. |
| microsoft.directory/users/registeredDevices/read | Azure Active Directory'de users.registeredDevices özelliğini okuyun. |

### <a name="helpdesk-administrator-permissions"></a>Yardım Masası Yöneticisi izinleri

Yönetici olmayanların ve Yardım Masası Yöneticilerinin parolalarını sıfırlayabilir.

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.directory/devices/bitLockerRecoveryKeys/read | Azure Active Directory'de devices.bitLockerRecoveryKeys özelliğini okuyun. |
| microsoft.directory/users/invalidateAllRefreshTokens | Azure Etkin Dizin'inde tüm kullanıcı yenileme belirteçlerini geçersiz kılın. |
| microsoft.directory/users/password/update | Azure Active Directory'deki tüm kullanıcıların parolalarını güncelleştirin. Daha fazla ayrıntı için çevrimiçi belgelere bakın. |
| microsoft.azure.serviceHealth/allEntities/allGörevler | Azure Hizmet Durumu'nun okunmasını ve yapılandırılsın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure destek biletleri oluşturun ve yönetin. |
| microsoft.office365.webPortal/allEntities/basic/read | microsoft.office365.webPortal'daki tüm kaynaklardaki temel özellikleri okuyun. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nun okunmasını ve yapılandırılsın. |
| microsoft.office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |

### <a name="intune-service-administrator-permissions"></a>Intune Service Administrator izinleri

Intune ürününün tüm yönlerini yönetebilirsiniz.

> [!NOTE]
> Bu rolün Azure Etkin Dizini dışında ek izinleri vardır. Daha fazla bilgi için yukarıdaki rol açıklamasına bakın.
>
>

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.directory/contacts/basic/update | Azure Etkin Dizini'ndeki kişilerin temel özelliklerini güncelleştirin. |
| microsoft.directory/contacts/create | Azure Etkin Dizini'nde kişiler oluşturun. |
| microsoft.directory/contacts/delete | Azure Etkin Dizini'ndeki kişileri silin. |
| microsoft.directory/devices/basic/update | Azure Etkin Dizini'ndeki aygıtların temel özelliklerini güncelleştirin. |
| microsoft.directory/devices/bitLockerRecoveryKeys/read | Azure Active Directory'de devices.bitLockerRecoveryKeys özelliğini okuyun. |
| microsoft.directory/devices/create | Azure Active Directory'de aygıtlar oluşturun. |
| microsoft.directory/devices/delete | Azure Etkin Dizini'ndeki aygıtları silin. |
| microsoft.directory/devices/registeredOwners/update | Devices.registeredOwners özelliğini Azure Active Directory'de güncelleştirin. |
| microsoft.directory/devices/registeredUsers/update | Devices.registeredUsers özelliğini Azure Active Directory'de güncelleştirin. |
| microsoft.directory/groups/appRoleAssignments/update | Azure Etkin Dizini'nde grupları.appRoleAssignments özelliğini güncelleştirin. |
| microsoft.directory/groups/basic/update | Azure Etkin Dizini'ndeki gruplardaki temel özellikleri güncelleştirin. |
| microsoft.directory/groups/create | Azure Etkin Dizini'nde gruplar oluşturun. |
| microsoft.directory/groups/createAsOwner | Azure Etkin Dizini'nde gruplar oluşturun. Oluşturan ilk sahibi olarak eklenir ve oluşturulan nesne oluşturan 250 oluşturulan nesneler kotakarşı sayar. |
| microsoft.directory/groups/delete | Azure Etkin Dizini'ndeki grupları silin. |
| microsoft.directory/groups/hiddenÜyeler/read | Azure Etkin Dizini'nde grupları okuyun.hiddenMembers özelliği. |
| microsoft.directory/groups/members/update | Grup.üyeler özelliğini Azure Etkin Dizini'nde güncelleştirin. |
| microsoft.directory/groups/owners/update | Grup.sahipleri azure Etkin Dizini'nde güncelleştirin. |
| microsoft.directory/groups/geri yükleme | Azure Etkin Dizini'ndeki grupları geri yükleyin. |
| microsoft.directory/groups/settings/update | Azure Etkin Dizini'nde groups.settings özelliğini güncelleştirin. |
| microsoft.directory/users/appRoleAssignments/update | Azure Active Directory'deki kullanıcıları.appRoleAssignments özelliğini güncelleştirin. |
| microsoft.directory/users/basic/update | Azure Active Directory'deki kullanıcıların temel özelliklerini güncelleştirin. |
| microsoft.directory/users/manager/update | Azure Active Directory'deki users.manager özelliğini güncelleştirin. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure destek biletleri oluşturun ve yönetin. |
| microsoft.intune/allEntities/allTasks | Intune'un tüm yönlerini yönetin. |
| microsoft.office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |
| microsoft.office365.webPortal/allEntities/basic/read | microsoft.office365.webPortal'daki tüm kaynaklardaki temel özellikleri okuyun. |

### <a name="kaizala-administrator-permissions"></a>Kaizala Yönetici izinleri

Microsoft Kaizala'nın ayarlarını yönetebilirsiniz.

> [!NOTE]
> Bu rolün Azure Etkin Dizini dışında ek izinleri vardır. Daha fazla bilgi için yukarıdaki rol açıklamasına bakın.
>
>

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nun okunmasını ve yapılandırılsın. |
| microsoft.office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |
| microsoft.office365.webPortal/allEntities/basic/read | Office 365 yönetici merkezini okuyun. |

### <a name="license-administrator-permissions"></a>Lisans Yöneticisi izinleri

Kullanıcılar ve gruplar üzerindeki ürün lisanslarını yönetebilirsiniz.

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.directory/users/atsignLicense | Azure Active Directory'deki kullanıcıların lisanslarını yönetin. |
| microsoft.directory/users/usageLocation/update | Kullanıcıları güncelleştirin.kullanımAzure Active Directory'deki konum özelliğini kullanın. |
| microsoft.azure.serviceHealth/allEntities/allGörevler | Azure Hizmet Durumu'nun okunmasını ve yapılandırılsın. |
| microsoft.office365.webPortal/allEntities/basic/read | microsoft.office365.webPortal'daki tüm kaynaklardaki temel özellikleri okuyun. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nun okunmasını ve yapılandırılsın. |

### <a name="lync-service-administrator-permissions"></a>Lync Hizmet Yöneticisi izinleri

Skype kurumsal ürününün tüm yönlerini yönetebilirsiniz.

> [!NOTE]
> Bu rolün Azure Etkin Dizini dışında ek izinleri vardır. Daha fazla bilgi için yukarıdaki rol açıklamasına bakın.
>
>

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allGörevler | Azure Hizmet Durumu'nun okunmasını ve yapılandırılsın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure destek biletleri oluşturun ve yönetin. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nun okunmasını ve yapılandırılsın. |
| microsoft.office365.skypeForBusiness/allEntities/allTasks | Skype for Business Online'ın tüm yönlerini yönetin. |
| microsoft.office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |
| microsoft.office365.usageReports/allEntities/read    | Office 365 kullanım raporlarını okuyun. |
| microsoft.office365.webPortal/allEntities/basic/read | microsoft.office365.webPortal'daki tüm kaynaklardaki temel özellikleri okuyun. |


### <a name="message-center-privacy-reader-permissions"></a>İleti Merkezi Gizlilik Okuyucu izinleri

İleti Merkezi gönderilerini, veri gizlilik mesajlarını, grupları, etki alanlarını ve abonelikleri okuyabilir.

> [!NOTE]
> Bu rolün Azure Etkin Dizini dışında ek izinleri vardır. Daha fazla bilgi için yukarıdaki rol açıklamasına bakın.
>
>

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.office365.webPortal/allEntities/basic/read | microsoft.office365.webPortal'daki tüm kaynaklardaki temel özellikleri okuyun. |
| microsoft.office365.messageCenter/mesajlar/oku | microsoft.office365.messageCenter adresindeki iletileri okuyun. |
| microsoft.office365.messageCenter/securityMessages/read | microsoft.office365.messageCenter adresinden securityMessages'ı okuyun. |

### <a name="message-center-reader-permissions"></a>İleti Merkezi Okuyucu izinleri
Yalnızca Office 365 İleti Merkezi'nde kuruluşlarıyla ilgili iletileri ve güncelleştirmeleri okuyabilir. 

> [!NOTE]
> Bu rolün Azure Etkin Dizini dışında ek izinleri vardır. Daha fazla bilgi için yukarıdaki rol açıklamasına bakın.
>
>

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.office365.webPortal/allEntities/basic/read | microsoft.office365.webPortal'daki tüm kaynaklardaki temel özellikleri okuyun. |
| microsoft.office365.messageCenter/mesajlar/oku | microsoft.office365.messageCenter adresindeki iletileri okuyun. |

### <a name="office-apps-administrator-permissions"></a>Office Apps Yöneticisi izinleri
İlke ve ayar yönetimi de dahil olmak üzere Office uygulamalarının bulut hizmetlerini yönetebilir ve son kullanıcının cihazlarına "yenilikler" özelliği içeriğini seçme, seçme mesuliyeti ve yayımlama yeteneğini yönetebilir.

> [!NOTE]
> Bu rolün Azure Etkin Dizini dışında ek izinleri vardır. Daha fazla bilgi için yukarıdaki rol açıklamasına bakın.
>
>

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allGörevler | Azure Hizmet Durumu'nun okunmasını ve yapılandırılsın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure destek biletleri oluşturun ve yönetin. |
| microsoft.office365.messageCenter/mesajlar/oku | microsoft.office365.messageCenter adresindeki iletileri okuyun. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nun okunmasını ve yapılandırılsın. |
| microsoft.office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |
| microsoft.office365.userCommunication/allEntities/allTasks | Yeniliklerin görünürlüğünü okuyun ve güncelleyin. |
| microsoft.office365.webPortal/allEntities/basic/read | microsoft.office365.webPortal'daki tüm kaynaklardaki temel özellikleri okuyun. |

### <a name="partner-tier1-support-permissions"></a>İş Ortağı Tier1 Destek izinleri

Genel kullanım için tasarlanmamıştır.

> [!NOTE]
> Bu rolün Azure Etkin Dizini dışında ek izinleri vardır. Daha fazla bilgi için yukarıdaki rol açıklamasına bakın.
>
>

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.directory/contacts/basic/update | Azure Etkin Dizini'ndeki kişilerin temel özelliklerini güncelleştirin. |
| microsoft.directory/contacts/create | Azure Etkin Dizini'nde kişiler oluşturun. |
| microsoft.directory/contacts/delete | Azure Etkin Dizini'ndeki kişileri silin. |
| microsoft.directory/groups/create | Azure Etkin Dizini'nde gruplar oluşturun. |
| microsoft.directory/groups/createAsOwner | Azure Etkin Dizini'nde gruplar oluşturun. Oluşturan ilk sahibi olarak eklenir ve oluşturulan nesne oluşturan 250 oluşturulan nesneler kotakarşı sayar. |
| microsoft.directory/groups/members/update | Grup.üyeler özelliğini Azure Etkin Dizini'nde güncelleştirin. |
| microsoft.directory/groups/owners/update | Grup.sahipleri azure Etkin Dizini'nde güncelleştirin. |
| microsoft.directory/users/appRoleAssignments/update | Azure Active Directory'deki kullanıcıları.appRoleAssignments özelliğini güncelleştirin. |
| microsoft.directory/users/atsignLicense | Azure Active Directory'deki kullanıcıların lisanslarını yönetin. |
| microsoft.directory/users/basic/update | Azure Active Directory'deki kullanıcıların temel özelliklerini güncelleştirin. |
| microsoft.directory/users/delete | Azure Etkin Dizini'ndeki kullanıcıları silin. |
| microsoft.directory/users/invalidateAllRefreshTokens | Azure Etkin Dizin'inde tüm kullanıcı yenileme belirteçlerini geçersiz kılın. |
| microsoft.directory/users/manager/update | Azure Active Directory'deki users.manager özelliğini güncelleştirin. |
| microsoft.directory/users/password/update | Azure Active Directory'deki tüm kullanıcıların parolalarını güncelleştirin. Daha fazla ayrıntı için çevrimiçi belgelere bakın. |
| microsoft.directory/users/restore | Azure Active Directory'de silinen kullanıcıları geri yükleyin. |
| microsoft.directory/users/userPrincipalName/update | Kullanıcıları güncelleştirin.userPrincipalName özelliğini Azure Etkin Dizini'nde güncelleştirin. |
| microsoft.azure.serviceHealth/allEntities/allGörevler | Azure Hizmet Durumu'nun okunmasını ve yapılandırılsın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure destek biletleri oluşturun ve yönetin. |
| microsoft.office365.webPortal/allEntities/basic/read | microsoft.office365.webPortal'daki tüm kaynaklardaki temel özellikleri okuyun. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nun okunmasını ve yapılandırılsın. |
| microsoft.office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |

### <a name="partner-tier2-support-permissions"></a>İş Ortağı Tier2 Destek izinleri

Genel kullanım için tasarlanmamıştır.

> [!NOTE]
> Bu rolün Azure Etkin Dizini dışında ek izinleri vardır. Daha fazla bilgi için yukarıdaki rol açıklamasına bakın.
>
>

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.directory/contacts/basic/update | Azure Etkin Dizini'ndeki kişilerin temel özelliklerini güncelleştirin. |
| microsoft.directory/contacts/create | Azure Etkin Dizini'nde kişiler oluşturun. |
| microsoft.directory/contacts/delete | Azure Etkin Dizini'ndeki kişileri silin. |
| microsoft.directory/domains/allTasks | Azure Active Directory'de etki alanları oluşturun ve silin ve standart özellikleri okuyun ve güncelleyin. |
| microsoft.directory/groups/create | Azure Etkin Dizini'nde gruplar oluşturun. |
| microsoft.directory/groups/delete | Azure Etkin Dizini'ndeki grupları silin. |
| microsoft.directory/groups/members/update | Grup.üyeler özelliğini Azure Etkin Dizini'nde güncelleştirin. |
| microsoft.directory/groups/geri yükleme | Azure Etkin Dizini'ndeki grupları geri yükleyin. |
| microsoft.directory/organization/basic/update | Azure Active Directory'deki kuruluştemel özelliklerini güncelleştirin. |
| microsoft.directory/users/appRoleAssignments/update | Azure Active Directory'deki kullanıcıları.appRoleAssignments özelliğini güncelleştirin. |
| microsoft.directory/users/atsignLicense | Azure Active Directory'deki kullanıcıların lisanslarını yönetin. |
| microsoft.directory/users/basic/update | Azure Active Directory'deki kullanıcıların temel özelliklerini güncelleştirin. |
| microsoft.directory/users/delete | Azure Etkin Dizini'ndeki kullanıcıları silin. |
| microsoft.directory/users/invalidateAllRefreshTokens | Azure Etkin Dizin'inde tüm kullanıcı yenileme belirteçlerini geçersiz kılın. |
| microsoft.directory/users/manager/update | Azure Active Directory'deki users.manager özelliğini güncelleştirin. |
| microsoft.directory/users/password/update | Azure Active Directory'deki tüm kullanıcıların parolalarını güncelleştirin. Daha fazla ayrıntı için çevrimiçi belgelere bakın. |
| microsoft.directory/users/restore | Azure Active Directory'de silinen kullanıcıları geri yükleyin. |
| microsoft.directory/users/userPrincipalName/update | Kullanıcıları güncelleştirin.userPrincipalName özelliğini Azure Etkin Dizini'nde güncelleştirin. |
| microsoft.azure.serviceHealth/allEntities/allGörevler | Azure Hizmet Durumu'nun okunmasını ve yapılandırılsın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure destek biletleri oluşturun ve yönetin. |
| microsoft.office365.webPortal/allEntities/basic/read | microsoft.office365.webPortal'daki tüm kaynaklardaki temel özellikleri okuyun. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nun okunmasını ve yapılandırılsın. |
| microsoft.office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |

### <a name="password-administrator-permissions"></a>Parola Yöneticisi izinleri

Yönetici olmayanlar ve Parola yöneticileri için parolaları sıfırlayabilir.

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.directory/users/password/update | Azure Active Directory'deki tüm kullanıcıların parolalarını güncelleştirin. Daha fazla ayrıntı için çevrimiçi belgelere bakın. |
| microsoft.office365.webPortal/allEntities/basic/read | microsoft.office365.webPortal'daki tüm kaynaklardaki temel özellikleri okuyun. |

### <a name="power-bi-service-administrator-permissions"></a>Power BI Service Administrator izinleri

Power BI ürününün tüm yönlerini yönetebilirsiniz.

> [!NOTE]
> Bu rolün Azure Etkin Dizini dışında ek izinleri vardır. Daha fazla bilgi için yukarıdaki rol açıklamasına bakın.
>
>
| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allGörevler | Azure Hizmet Durumu'nun okunmasını ve yapılandırılsın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure destek biletleri oluşturun ve yönetin. |
| microsoft.powerApps.powerBI/allEntities/allTasks | Power BI'nin tüm yönlerini yönetin. |
| microsoft.office365.webPortal/allEntities/basic/read | microsoft.office365.webPortal'daki tüm kaynaklardaki temel özellikleri okuyun. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nun okunmasını ve yapılandırılsın. |
| microsoft.office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |


### <a name="power-platform-administrator-permissions"></a>Güç Platformu Yöneticisi izinleri

Microsoft Dynamics 365, PowerApps ve Microsoft Flow'un tüm yönlerini oluşturabilir ve yönetebilir. 

> [!NOTE]
> Bu rolün Azure Etkin Dizini dışında ek izinleri vardır. Daha fazla bilgi için yukarıdaki rol açıklamasına bakın.
>
>
| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allGörevler | Azure Hizmet Durumu'nun okunmasını ve yapılandırılsın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure destek biletleri oluşturun ve yönetin. |
| microsoft.dynamics365/allEntities/allTasks | Dynamics 365'in tüm yönlerini yönetin. |
| microsoft.flow/allEntities/allTasks | Microsoft Flow'un tüm yönlerini yönetin. |
| microsoft.powerApps/allEntities/allTasks | PowerApps'ın tüm yönlerini yönetin. |
| microsoft.office365.webPortal/allEntities/basic/read | microsoft.office365.webPortal'daki tüm kaynaklardaki temel özellikleri okuyun. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nun okunmasını ve yapılandırılsın. |
| microsoft.office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |

### <a name="privileged-authentication-administrator-permissions"></a>Ayrıcalıklı Kimlik Doğrulama Yöneticisi izinleri

Herhangi bir kullanıcı (yönetici veya yönetici olmayan) için kimlik doğrulama yöntemi bilgilerini görüntülemeye, ayarlamasına ve sıfırlamasına izin verilir.

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.directory/users/invalidateAllRefreshTokens | Azure Etkin Dizin'inde tüm kullanıcı yenileme belirteçlerini geçersiz kılın. |
| microsoft.directory/users/strongAuthentication/update | MFA kimlik bilgileri gibi güçlü kimlik doğrulama özelliklerini güncelleştirin. |
| microsoft.azure.serviceHealth/allEntities/allGörevler | Azure Hizmet Durumu'nun okunmasını ve yapılandırılsın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure destek biletleri oluşturun ve yönetin. |
| microsoft.office365.webPortal/allEntities/basic/read | microsoft.office365.webPortal'daki tüm kaynaklardaki temel özellikleri okuyun. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nun okunmasını ve yapılandırılsın. |
| microsoft.office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |
| microsoft.directory/users/password/update | Office 365 kuruluşundaki tüm kullanıcıların parolalarını güncelleştirin. Daha fazla ayrıntı için çevrimiçi belgelere bakın. |

### <a name="privileged-role-administrator-permissions"></a>Ayrıcalıklı Rol Yöneticisi izinleri

Azure AD'deki rol atamalarını ve Ayrıcalıklı Kimlik Yönetimi'nin tüm yönlerini yönetebilir.

> [!NOTE]
> Bu rolün Azure Etkin Dizini dışında ek izinleri vardır. Daha fazla bilgi için yukarıdaki rol açıklamasına bakın.
>
>

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.privilegedIdentityManagement/allEntities/allTasks | Microsoft.aad.privilegedIdentityManagement'da tüm kaynakları oluşturun ve silin ve standart özellikleri okuyun ve güncelleyin. |
| microsoft.directory/servicePrincipals/appRoleAssignedTo/allTasks | Azure Active Directory'deki hizmetPrincipals.appRoleAssignedTo özelliğini okuyun ve yapılandırın. |
| microsoft.directory/servicePrincipals/oAuth2PermissionGrants/allTasks | Azure Active Directory'deki hizmetPrincipals.oAuth2PermissionGrants özelliğini okuyun ve yapılandırın. |
| microsoft.directory/administrativeUnits/allProperties/allTasks | Yönetim birimleri oluşturma ve yönetme (üyeler dahil) |
| microsoft.directory/roleAssignments/allProperties/allTasks | Rol atamaları oluşturun ve yönetin. |
| microsoft.directory/roleDefinitions/allProperties/allTasks | Rol tanımlarını oluşturun ve yönetin. |

### <a name="reports-reader-permissions"></a>Raporlar Okuyucu izinleri

Oturum açma ve denetim raporlarını okuyabilir.

> [!NOTE]
> Bu rolün Azure Etkin Dizini dışında ek izinleri vardır. Daha fazla bilgi için yukarıdaki rol açıklamasına bakın.
>
>

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.directory/auditLogs/allProperties/read | Azure Etkin Dizini'ndeki auditLogs'daki tüm özellikleri (ayrıcalıklı özellikler dahil) okuyun. |
| microsoft.directory/signInReports/allProperties/read | Azure Active Directory'de signInReports'daki tüm özellikleri (ayrıcalıklı özellikler dahil) okuyun. |
| microsoft.azure.serviceHealth/allEntities/allGörevler | Azure Hizmet Durumu'nun okunmasını ve yapılandırılsın. |
| microsoft.office365.usageReports/allEntities/read | Office 365 kullanım raporlarını okuyun. |

### <a name="search-administrator-permissions"></a>Arama Yöneticisi izinleri

Microsoft Arama ayarlarının tüm yönlerini oluşturabilir ve yönetebilir.

> [!NOTE]
> Bu rolün Azure Etkin Dizini dışında ek izinleri vardır. Daha fazla bilgi için yukarıdaki rol açıklamasına bakın.
>
>

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.office365.messageCenter/mesajlar/oku | microsoft.office365.messageCenter adresindeki iletileri okuyun. |
| microsoft.office365.search/allEntities/allProperties/allTasks | Tüm kaynakları oluşturun ve silin ve microsoft.office365.search'teki tüm özellikleri okuyun ve güncelleyin. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nun okunmasını ve yapılandırılsın. |
| microsoft.office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |
| microsoft.office365.webPortal/allEntities/basic/read | microsoft.office365.webPortal'daki tüm kaynaklardaki temel özellikleri okuyun. |

### <a name="search-editor-permissions"></a>Arama Düzenleyicisi izinleri

Yer imleri, Q ve As, konumlar, kat planı gibi editoryal içeriği oluşturabilir ve yönetebilir.

> [!NOTE]
> Bu rolün Azure Etkin Dizini dışında ek izinleri vardır. Daha fazla bilgi için yukarıdaki rol açıklamasına bakın.
>
>

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.office365.messageCenter/mesajlar/oku | microsoft.office365.messageCenter adresindeki iletileri okuyun. |
| microsoft.office365.search/content/allProperties/allTasks | Microsoft.office365.search'teki tüm özellikleri oluşturun ve silin ve güncelleyin. |

### <a name="security-administrator-permissions"></a>Güvenlik Yöneticisi izinleri

Güvenlik bilgilerini ve raporlarını okuyabilir ve Azure AD ve Office 365'te yapılandırmayı yönetebilir.

> [!NOTE]
> Bu rolün Azure Etkin Dizini dışında ek izinleri vardır. Daha fazla bilgi için yukarıdaki rol açıklamasına bakın.
>
>

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.directory/applications/policies/update | Azure Active Directory'deki applications.policies özelliğini güncelleştirin. |
| microsoft.directory/auditLogs/allProperties/read | Azure Etkin Dizini'ndeki auditLogs'daki tüm özellikleri (ayrıcalıklı özellikler dahil) okuyun. |
| microsoft.directory/devices/bitLockerRecoveryKeys/read | Azure Active Directory'de devices.bitLockerRecoveryKeys özelliğini okuyun. |
| microsoft.directory/policies/basic/update | Azure Etkin Dizini'ndeki ilkelerdeki temel özellikleri güncelleştirin. |
| microsoft.directory/policies/create | Azure Etkin Dizini'nde ilkeler oluşturun. |
| microsoft.directory/policies/delete | Azure Etkin Dizini'ndeki ilkeleri silin. |
| microsoft.directory/policies/owners/update | İlkeleri güncelleştirin.sahipleri Azure Etkin Dizini'nde. |
| microsoft.directory/policies/tenantDefault/update | İlkeleri güncelleştirin.tenantAzure Active Directory'deki varsayılan özelliği. |
| microsoft.directory/servicePrincipals/policies/update | Azure Etkin Dizini'nde hizmetPrincipals.policies özelliğini güncelleştirin. |
| microsoft.directory/signInReports/allProperties/read | Azure Active Directory'de signInReports'daki tüm özellikleri (ayrıcalıklı özellikler dahil) okuyun. |
| microsoft.aad.identityProtection/allEntities/read | microsoft.aad.identityProtection adresindeki tüm kaynakları okuyun. |
| microsoft.aad.identityProtection/allEntities/update | microsoft.aad.identityProtection'daki tüm kaynakları güncelleştirin. |
| microsoft.aad.privilegedIdentityManagement/allEntities/read | microsoft.aad.privilegedIdentityManagement adresindeki tüm kaynakları okuyun. |
| microsoft.azure.serviceHealth/allEntities/allGörevler | Azure Hizmet Durumu'nun okunmasını ve yapılandırılsın. |
| microsoft.office365.webPortal/allEntities/basic/read | microsoft.office365.webPortal'daki tüm kaynaklardaki temel özellikleri okuyun. |
| microsoft.office365.protectionCenter/allEntities/read | Office 365 Koruma Merkezi'nin tüm yönlerini okuyun. |
| microsoft.office365.protectionCenter/allEntities/update | microsoft.office365.protectionCenter'daki tüm kaynakları güncelleştirin. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nun okunmasını ve yapılandırılsın. |

### <a name="security-operator-permissions"></a>Güvenlik Operatörü izinleri

Güvenlik olayları oluşturur ve yönetir.

> [!NOTE]
> Bu rolün Azure Etkin Dizini dışında ek izinleri vardır. Daha fazla bilgi için yukarıdaki rol açıklamasına bakın.
>
>

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.cloudAppSecurity/allEntities/allTasks | Microsoft Cloud App Security'yi okuyun ve yapılandırır. |
| microsoft.aad.identityProtection/allEntities/read | microsoft.aad.identityProtection adresindeki tüm kaynakları okuyun. |
| microsoft.aad.privilegedIdentityManagement/allEntities/read | microsoft.aad.privilegedIdentityManagement adresindeki tüm kaynakları okuyun. |
| microsoft.azure.advancedThreatProtection/allEntities/read | Azure AD Gelişmiş Tehdit Koruması'nı okuyun ve yapılandırın. |
| microsoft.intune/allEntities/allTasks | Intune'un tüm yönlerini yönetin. |
| microsoft.office365.securityComplianceCenter/allEntities/allTasks | Güvenlik & Uyumluluk Merkezi'ni okuyun ve yapılandırır. |
| microsoft.windows.defenderAdvancedThreatProtection/allEntities/read | Windows Defender Gelişmiş Tehdit Koruması'nı okuyun ve yapılandırır. |

### <a name="security-reader-permissions"></a>Güvenlik Okuyucu izinleri

Azure AD ve Office 365'te güvenlik bilgilerini ve raporlarını okuyabilir.

> [!NOTE]
> Bu rolün Azure Etkin Dizini dışında ek izinleri vardır. Daha fazla bilgi için yukarıdaki rol açıklamasına bakın.
>
>

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.directory/auditLogs/allProperties/read | Azure Etkin Dizini'ndeki auditLogs'daki tüm özellikleri (ayrıcalıklı özellikler dahil) okuyun. |
| microsoft.directory/devices/bitLockerRecoveryKeys/read | Azure Active Directory'de devices.bitLockerRecoveryKeys özelliğini okuyun. |
| microsoft.directory/policies/conditionalAccess/basic/read | Azure Active Directory'de ilkeler.conditionalAccess özelliğini okuyun. |
| microsoft.directory/signInReports/allProperties/read | Azure Active Directory'de signInReports'daki tüm özellikleri (ayrıcalıklı özellikler dahil) okuyun. |
| microsoft.aad.identityProtection/allEntities/read | microsoft.aad.identityProtection adresindeki tüm kaynakları okuyun. |
| microsoft.aad.privilegedIdentityManagement/allEntities/read | microsoft.aad.privilegedIdentityManagement adresindeki tüm kaynakları okuyun. |
| microsoft.azure.serviceHealth/allEntities/allGörevler | Azure Hizmet Durumu'nun okunmasını ve yapılandırılsın. |
| microsoft.office365.webPortal/allEntities/basic/read | microsoft.office365.webPortal'daki tüm kaynaklardaki temel özellikleri okuyun. |
| microsoft.office365.protectionCenter/allEntities/read | Office 365 Koruma Merkezi'nin tüm yönlerini okuyun. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nun okunmasını ve yapılandırılsın. |

### <a name="service-support-administrator-permissions"></a>Hizmet Destek Yöneticisi izinleri

Servis sağlık bilgilerini okuyabilir ve destek biletlerini yönetebilirsiniz.

> [!NOTE]
> Bu rolün Azure Etkin Dizini dışında ek izinleri vardır. Daha fazla bilgi için yukarıdaki rol açıklamasına bakın.
>
>

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allGörevler | Azure Hizmet Durumu'nun okunmasını ve yapılandırılsın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure destek biletleri oluşturun ve yönetin. |
| microsoft.office365.webPortal/allEntities/basic/read | microsoft.office365.webPortal'daki tüm kaynaklardaki temel özellikleri okuyun. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nun okunmasını ve yapılandırılsın. |
| microsoft.office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |

### <a name="sharepoint-service-administrator-permissions"></a>SharePoint Service Administrator izinleri

SharePoint hizmetinin tüm yönlerini yönetebilirsiniz.

> [!NOTE]
> Bu rolün Azure Etkin Dizini dışında ek izinleri vardır. Daha fazla bilgi için yukarıdaki rol açıklamasına bakın.
>
>

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allGörevler | Azure Hizmet Durumu'nun okunmasını ve yapılandırılsın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure destek biletleri oluşturun ve yönetin. |
| microsoft.directory/groups/unified/appRoleAssignments/update | Azure Etkin Dizin'deki grupları.birleşik özelliği güncelleştirin. |
| microsoft.directory/groups/unified/basic/update | Office 365 Grupları'nın temel özelliklerini güncelleştirin. |
| microsoft.directory/groups/unified/create | Office 365 Grupları oluşturun. |
| microsoft.directory/groups/unified/delete | Office 365 Gruplarını Silin. |
| microsoft.directory/groups/unified/members/update | Office 365 Grupları üyeliğini güncelleştirin. |
| microsoft.directory/groups/unified/owners/update | Office 365 Gruplarının sahipliğini güncelleştirin. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nun okunmasını ve yapılandırılsın. |
| microsoft.office365.sharepoint/allEntities/allTasks | Microsoft.office365.sharepoint'teki tüm kaynakları oluşturun ve silin ve standart özellikleri okuyun ve güncelleyin. |
| microsoft.office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |
| microsoft.office365.usageReports/allEntities/read    | Office 365 kullanım raporlarını okuyun. |
| microsoft.office365.webPortal/allEntities/basic/read | microsoft.office365.webPortal'daki tüm kaynaklardaki temel özellikleri okuyun. |

### <a name="teams-communications-administrator-permissions"></a>Takımlar İletişim Yöneticisi izinleri

Microsoft Teams hizmetindeki arama ve toplantı özelliklerini yönetebilirsiniz.

> [!NOTE]
> Bu rolün Azure Etkin Dizini dışında ek izinleri vardır. Daha fazla bilgi için yukarıdaki rol açıklamasına bakın.
>
>

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allGörevler | Azure Hizmet Durumu'nun okunmasını ve yapılandırılsın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure destek biletleri oluşturun ve yönetin. |
| microsoft.office365.webPortal/allEntities/basic/read | microsoft.office365.webPortal'daki tüm kaynaklardaki temel özellikleri okuyun. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nun okunmasını ve yapılandırılsın. |
| microsoft.office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |
| microsoft.office365.usageReports/allEntities/read | Office 365 kullanım raporlarını okuyun. |

### <a name="teams-communications-support-engineer-permissions"></a>Takımlar İletişim Destek Mühendisi izinleri

Gelişmiş araçları kullanarak Takımlar içindeki iletişim sorunlarını giderebilir.

> [!NOTE]
> Bu rolün Azure Etkin Dizini dışında ek izinleri vardır. Daha fazla bilgi için yukarıdaki rol açıklamasına bakın.
>
>

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allGörevler | Azure Hizmet Durumu'nun okunmasını ve yapılandırılsın. |
| microsoft.office365.webPortal/allEntities/basic/read | microsoft.office365.webPortal'daki tüm kaynaklardaki temel özellikleri okuyun. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nun okunmasını ve yapılandırılsın. |

### <a name="teams-communications-support-specialist-permissions"></a>Ekipler İletişim Destek Uzmanı izinleri

Temel araçları kullanarak Takımlar içindeki iletişim sorunlarını giderebilir.

> [!NOTE]
> Bu rolün Azure Etkin Dizini dışında ek izinleri vardır. Daha fazla bilgi için yukarıdaki rol açıklamasına bakın.
>
>

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allGörevler | Azure Hizmet Durumu'nun okunmasını ve yapılandırılsın. |
| microsoft.office365.webPortal/allEntities/basic/read | microsoft.office365.webPortal'daki tüm kaynaklardaki temel özellikleri okuyun. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nun okunmasını ve yapılandırılsın. |

### <a name="teams-service-administrator-permissions"></a>Takımlar Servis Yöneticisi izinleri

Microsoft Teams hizmetini yönetebilirsiniz.

> [!NOTE]
> Bu rolün Azure Etkin Dizini dışında ek izinleri vardır. Daha fazla bilgi için yukarıdaki rol açıklamasına bakın.
>
>

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.directory/groups/hiddenÜyeler/read | Azure Etkin Dizini'nde grupları okuyun.hiddenMembers özelliği. |
| microsoft.directory/groups/unified/appRoleAssignments/update | Azure Etkin Dizin'deki grupları.birleşik özelliği güncelleştirin. |
| microsoft.directory/groups/unified/basic/update | Office 365 Grupları'nın temel özelliklerini güncelleştirin. |
| microsoft.directory/groups/unified/create | Office 365 Grupları oluşturun. |
| microsoft.directory/groups/unified/delete | Office 365 Gruplarını Silin. |
| microsoft.directory/groups/unified/members/update | Office 365 Grupları üyeliğini güncelleştirin. |
| microsoft.directory/groups/unified/owners/update | Office 365 Gruplarının sahipliğini güncelleştirin. |
| microsoft.azure.serviceHealth/allEntities/allGörevler | Azure Hizmet Durumu'nun okunmasını ve yapılandırılsın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure destek biletleri oluşturun ve yönetin. |
| microsoft.office365.webPortal/allEntities/basic/read | microsoft.office365.webPortal'daki tüm kaynaklardaki temel özellikleri okuyun. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nun okunmasını ve yapılandırılsın. |
| microsoft.office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |
| microsoft.office365.usageReports/allEntities/read | Office 365 kullanım raporlarını okuyun. |

### <a name="user-administrator-permissions"></a>Kullanıcı Yöneticisi izinleri
Sınırlı yöneticiler için parolaları sıfırlama da dahil olmak üzere kullanıcıların ve grupların tüm yönlerini yönetebilirsiniz.

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.directory/appRoleAssignments/create | Azure Etkin Dizini'nde uygulamaRol Atamaları oluşturun. |
| microsoft.directory/appRoleAssignments/delete | Azure Etkin Dizini'ndeki uygulamaRol Atamaları'nı silin. |
| microsoft.directory/appRoleAssignments/update | Azure Etkin Dizini'nde uygulamaRol Atamaları'nı güncelleştirin. |
| microsoft.directory/contacts/basic/update | Azure Etkin Dizini'ndeki kişilerin temel özelliklerini güncelleştirin. |
| microsoft.directory/contacts/create | Azure Etkin Dizini'nde kişiler oluşturun. |
| microsoft.directory/contacts/delete | Azure Etkin Dizini'ndeki kişileri silin. |
| microsoft.directory/groups/appRoleAssignments/update | Azure Etkin Dizini'nde grupları.appRoleAssignments özelliğini güncelleştirin. |
| microsoft.directory/groups/basic/update | Azure Etkin Dizini'ndeki gruplardaki temel özellikleri güncelleştirin. |
| microsoft.directory/groups/create | Azure Etkin Dizini'nde gruplar oluşturun. |
| microsoft.directory/groups/createAsOwner | Azure Etkin Dizini'nde gruplar oluşturun. Oluşturan ilk sahibi olarak eklenir ve oluşturulan nesne oluşturan 250 oluşturulan nesneler kotakarşı sayar. |
| microsoft.directory/groups/delete | Azure Etkin Dizini'ndeki grupları silin. |
| microsoft.directory/groups/hiddenÜyeler/read | Azure Etkin Dizini'nde grupları okuyun.hiddenMembers özelliği. |
| microsoft.directory/groups/members/update | Grup.üyeler özelliğini Azure Etkin Dizini'nde güncelleştirin. |
| microsoft.directory/groups/owners/update | Grup.sahipleri azure Etkin Dizini'nde güncelleştirin. |
| microsoft.directory/groups/geri yükleme | Azure Etkin Dizini'ndeki grupları geri yükleyin. |
| microsoft.directory/groups/settings/update | Azure Etkin Dizini'nde groups.settings özelliğini güncelleştirin. |
| microsoft.directory/users/appRoleAssignments/update | Azure Active Directory'deki kullanıcıları.appRoleAssignments özelliğini güncelleştirin. |
| microsoft.directory/users/atsignLicense | Azure Active Directory'deki kullanıcıların lisanslarını yönetin. |
| microsoft.directory/users/basic/update | Azure Active Directory'deki kullanıcıların temel özelliklerini güncelleştirin. |
| microsoft.directory/users/create | Azure Active Directory’de kullanıcılar oluşturun. |
| microsoft.directory/users/delete | Azure Etkin Dizini'ndeki kullanıcıları silin. |
| microsoft.directory/users/invalidateAllRefreshTokens | Azure Etkin Dizin'inde tüm kullanıcı yenileme belirteçlerini geçersiz kılın. |
| microsoft.directory/users/manager/update | Azure Active Directory'deki users.manager özelliğini güncelleştirin. |
| microsoft.directory/users/password/update | Azure Active Directory'deki tüm kullanıcıların parolalarını güncelleştirin. Daha fazla ayrıntı için çevrimiçi belgelere bakın. |
| microsoft.directory/users/restore | Azure Active Directory'de silinen kullanıcıları geri yükleyin. |
| microsoft.directory/users/userPrincipalName/update | Kullanıcıları güncelleştirin.userPrincipalName özelliğini Azure Etkin Dizini'nde güncelleştirin. |
| microsoft.azure.serviceHealth/allEntities/allGörevler | Azure Hizmet Durumu'nun okunmasını ve yapılandırılsın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure destek biletleri oluşturun ve yönetin. |
| microsoft.office365.webPortal/allEntities/basic/read | microsoft.office365.webPortal'daki tüm kaynaklardaki temel özellikleri okuyun. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nun okunmasını ve yapılandırılsın. |
| microsoft.office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |

## <a name="role-template-ids"></a>Rol şablonu iLikleri

Rol şablonu iAnda'ları çoğunlukla Microsoft Graph API veya PowerShell kullanıcıları tarafından kullanılır.

Grafik displayName | Azure portal ekran adı | dizinRoleTemplateId
----------------- | ------------------------- | -------------------------
Uygulama Yöneticisi | Uygulama yöneticisi | 9B895D92-2CD3-44C7-9D02-A6AC2D5EA5C3
Uygulama Geliştirici | Uygulama geliştiricisi | CF1C38E5-3621-4004-A7CB-879624DCED7C
Kimlik Doğrulama Yöneticisi | Kimlik doğrulama yöneticisi | c4e39bd9-1100-46d3-8c65-fb160da0071f
Azure Devops Yöneticisi | Azure DevOps yöneticisi | e3973bdf-4987-49ae-837a-ba8e231c7286
Azure Bilgi Koruma Yöneticisi | Azure Bilgi Koruma yöneticisi | 7495fdc4-34c4-4d15-a289-98788ce399fd
B2C Kullanıcı akışı Yöneticisi | B2C Kullanıcı akışı Yöneticisi | 6e591065-9bad-43ed-90f3-e9424366d2f0
B2C Kullanıcı Akışı Öznitelik Yöneticisi | B2C Kullanıcı Akışı Öznitelik Yöneticisi | 0f971ea-41eb-4569-a71e-57bb8a3eff1e
B2C IEF Anahtar Seti Yöneticisi | B2C IEF Anahtar Seti Yöneticisi | aaf43236-0c0d-4d5f-883a-6955382ac081
B2C IEF İlke Yöneticisi | B2C IEF İlke Yöneticisi | 3edaf663-341e-4475-9f94-5c398ef6c070
Fatura Yöneticisi | Faturalama yöneticisi | b0f54661-2d74-4c50-afa3-1ec803f12efe
Bulut Uygulaması Yöneticisi | Bulut uygulama yöneticisi | 158c047a-c907-4556-b7ef-446551a6b5f7
Bulut Aygıt Yöneticisi | Bulut aygıt yöneticisi | 7698a772-787b-4ac8-901f-60d6b08affd2
Şirket Yöneticisi | Genel yönetici | 62e90394-69f5-4237-9190-012177145e10
Uyumluluk Yöneticisi | Uyumluluk yöneticisi | 17315797-102d-40b4-93e0-432062caca18
Uyumluluk Veri Yöneticisi | Uyumluluk veri yöneticisi | e6d1a23a-da11-4be4-9570-befc86d067a7
Koşullu Erişim Yöneticisi | Koşullu Erişim yöneticisi | b1be1c3e-b65d-4f19-8427-f6fa0d97feb9
CRM Servis Yöneticisi | Dynamics 365 yöneticisi | 44367163-eba1-44c3-98af-f5787879f96a
Müşteri LockBox Erişim Onaylayıcısı | Müşteri Lockbox erişim onaylayıcısı | 5c4f9dcd-47dc-4cf7-8c9a-9e4207cbfc91
Masaüstü Analiz Yöneticisi | Masaüstü Analiz Yöneticisi | 38a96431-2bdf-4b4c-8b6e-5d3d8abac1a4
Cihaz Yöneticileri | Cihaz yöneticileri | 9f06204d-73c1-4d4c-880a-6edb90606fd8
Aygıt Birleştirme | Aygıt birleştirme | 9c094953-4995-41c8-84c8-3ebb9b32c93f
Cihaz Yöneticileri | Cihaz yöneticileri | 2b499bcd-da44-4968-8aec-78e1674fa64d
Cihaz Kullanıcıları | Cihaz kullanıcıları | d405c6df-0af8-4e3b-95e4-4d06e542189e
Dizin Okuyucular | Dizin okuyucular | 88d8e3e3-8f55-4a1e-953a-9b9898b8876b
Dizin Senkronizasyon Hesapları | Dizin eşitleme hesapları | d29b2b05-8046-44ba-8758-1e26182fcf32
Dizin Yazarlar | Dizin yazarları | 9360feb5-f418-4baa-8175-e2a00bac4301
Exchange Servis Yöneticisi | Exchange yöneticisi | 29232cdf-9323-42fd-ade2-1d097af3e4de
Dış Kimlik Sağlayıcı Yöneticisi | Dış Kimlik Sağlayıcı Yöneticisi | be2f45a1-457d-42af-a067-6ec1fa63bc45
Küresel Okuyucu | Küresel okuyucu | f2ef992c-3afb-46b9-b7cf-a126ee74c451
Gruplar Yöneticisi | Grup yöneticisi | fdd7a751-b60b-444a-984c-02652fe8fa1c 
Misafir Davetçi | Konuk davetçi | 95e79109-95c0-4d8e-aee3-d01accf2d47b
Yardım Masası Yöneticisi | Yardım Masası yöneticisi | 729827e3-9c14-49f7-bb1b-9608f156bbb8
Intune Hizmet Yöneticisi | Intune yöneticisi | 3a2c62db-5318-420d-8d74-23affee5d9d5
Kaizala Yöneticisi | Kaizala yöneticisi | 74ef975b-6605-40af-a5d2-b9539d836353
Lisans Yöneticisi | Lisans yöneticisi | 4d6ac14f-3453-41d0-bef9-a3e0c569773a
Lync Servis Yöneticisi | Skype Kurumsal yöneticisi | 75941009-915a-4869-abe7-691bff18279e
Mesaj Merkezi Gizlilik Okuyucu | İleti merkezi gizlilik okuyucu | ac16e43d-7b2d-40e0-ac05-243ff356ab5b
İleti Merkezi Okuyucu | İleti merkezi okuyucu | 790c1fb9-7f7d-4f88-86a1-ef1f95c05c1b
Ofis Uygulamaları Yöneticisi | Office uygulamaları yöneticisi | 2b745bdf-0803-4d80-aa65-822c4493daac
Ortak Tier1 Desteği | İş ortağı tier1 desteği | 4ba39ca4-527c-499a-b93d-d9b492c50246
Ortak Tier2 Desteği | İş ortağı tier2 desteği | e00e864a-17c5-4a4b-9c06-f5b95a8d5bd8
Şifre Yöneticisi | Parola yöneticisi | 966707d0-3269-4727-9be2-8c3a10f19b9d
Power BI Hizmet Yöneticisi | Güç BI yöneticisi | a9ea8996-122f-4c74-9520-8edcd192826c
Güç Platformu Yöneticisi | Güç platformu yöneticisi | 11648597-926c-4cf3-9c36-bcebb0ba8dcc
Ayrıcalıklı Kimlik Doğrulama Yöneticisi | Ayrıcalıklı kimlik doğrulama yöneticisi | 7be44c8a-adaf-4e2a-84d6-ab2649e08a13
Ayrıcalıklı Rol Yöneticisi | Ayrıcalıklı rol yöneticisi | e8611ab8-c189-46e8-94e1-60213ab1f814
Raporlar Okuyucu | Rapor okuyucu | 4a5d8f65-41da-4de4-8968-e035b65339cf
Arama Yöneticisi | Arama yöneticisi | 0964bb5e-9bdb-4d7b-ac29-58e794862a40
Arama Düzenleyicisi | Arama düzenleyicisi | 8835291a-918c-4fd7-a9ce-faa49f0cf7d9
Güvenlik Yöneticisi | Güvenlik yöneticisi | 194ae4cb-b126-40b2-bd5b-6091b380977d
Güvenlik Operatörü | Güvenlik operatörü | 5f2222b1-57c3-48ba-8ad5-d4759f1fde6f
Güvenlik Okuyucu | Güvenlik okuyucusu | 5d6b6bb7-de71-4623-b4af-96380a352509
Servis Destek Yöneticisi | Hizmet desteği yöneticisi | f023fd81-a637-4b56-95fd-791ac0226033
SharePoint Hizmet Yöneticisi | SharePoint yöneticisi | f28a1f50-f6e7-4571-818b-6a12f2af6b6c
Takımlar İletişim Yöneticisi | Takımlar İletişim Yöneticisi | baf37b3a-610e-45da-9e62-d9d1e5e8914b
Takımlar İletişim Destek Mühendisi | Takımlar İletişim Destek Mühendisi | f70938a0-fc10-4177-9e90-2178f8765737
Ekipler İletişim Destek Uzmanı | Ekipler İletişim Destek Uzmanı | fcf91098-03e3-41a9-b5ba-6f0ec8188a12
Takımlar Servis Yöneticisi | Takımlar Servis Yöneticisi | 69091246-20e8-4a56-aa4d-066075b2a7a8
Kullanıcı | Kullanıcı | a0b1b346-4d3e-4e8b-98f8-753987be4970
Kullanıcı Hesabı Yöneticisi | Kullanıcı yöneticisi | fe930be7-5e62-47db-91af-98c3a49a38b1
İşyeri Cihaz Birleştirme | İşyeri cihaz birleştirme | c34f683f-4d5a-4403-affd-6615e00e3a7f

## <a name="deprecated-roles"></a>Amortismana uyan roller

Aşağıdaki roller kullanılmamalıdır. Bunlar amortismana uğradı ve gelecekte Azure AD'den kaldırılacak.

* AdHoc Lisans Yöneticisi
* Aygıt Birleştirme
* Cihaz Yöneticileri
* Cihaz Kullanıcıları
* Doğrulanmış Kullanıcı Oluşturucuya E-posta Gönder
* Posta Kutusu Yöneticisi
* İşyeri Cihaz Birleştirme

## <a name="roles-not-shown-in-the-portal"></a>Portalda gösterilmeyen roller

PowerShell veya MS Graph API tarafından döndürülen her rol Azure portalında görünmez. Aşağıdaki tablo bu farklılıkları düzenler.

API adı | Azure portal adı | Notlar
-------- | ------------------- | -------------
Şirket Yöneticisi | Genel Yönetici | [Daha iyi netlik için ad değiştirildi](directory-assign-admin-roles.md#role-template-ids)
CRM Servis Yöneticisi | Dynamics 365 yöneticisi | [Mevcut ürün markasını yansıtır](directory-assign-admin-roles.md#role-template-ids)
Aygıt Birleştirme | Kullanım Dışı | [Amortismana karşı roller dokümantasyonu](directory-assign-admin-roles.md#deprecated-roles)
Cihaz Yöneticileri | Kullanım Dışı | [Amortismana karşı roller dokümantasyonu](directory-assign-admin-roles.md#deprecated-roles)
Cihaz Kullanıcıları | Kullanım Dışı | [Amortismana karşı roller dokümantasyonu](directory-assign-admin-roles.md#deprecated-roles)
Dizin Senkronizasyon Hesapları | Kullanılmaması gerektiği için gösterilmedi | [Dizin Senkronizasyon Hesapları dokümantasyonu](directory-assign-admin-roles.md#directory-synchronization-accounts)
Dizin Yazarlar | Kullanılmaması gerektiği için gösterilmedi | [Dizin Yazarlar dokümantasyon](directory-assign-admin-roles.md#directory-writers)
Konuk Kullanıcı | Kullanılamadığı için gösterilmedi  | NA
Lync Servis Yöneticisi | Skype Kurumsal yöneticisi | [Mevcut ürün markasını yansıtır](directory-assign-admin-roles.md#role-template-ids)
Ortak Tier 1 Desteği | Kullanılmaması gerektiği için gösterilmedi | [İş Ortağı Tier1 Destek belgeleri](directory-assign-admin-roles.md#partner-tier1-support)
Ortak Tier 2 Desteği | Kullanılmaması gerektiği için gösterilmedi | [İş Ortağı Tier2 Destek belgeleri](directory-assign-admin-roles.md#partner-tier2-support)
Yazıcı Yöneticisi | İş sürüyor | İş sürüyor
Yazıcı Teknisyeni | İş sürüyor | İş sürüyor
Kısıtlı Konuk Kullanıcı | Kullanılamadığı için gösterilmedi | NA
Kullanıcı | Kullanılamadığı için gösterilmedi | NA
İşyeri Cihaz Birleştirme | Kullanım Dışı | [Amortismana karşı roller dokümantasyonu](directory-assign-admin-roles.md#deprecated-roles)

## <a name="next-steps"></a>Sonraki adımlar

* Bir kullanıcıyı Azure aboneliğinin yöneticisi olarak nasıl atayınız hakkında daha fazla bilgi edinmek için Azure [rollerini kullanarak erişimi yönet 'e (Azure RBAC)](../../role-based-access-control/role-assignments-portal.md) bakın
* Microsoft Azure'da kaynak erişiminin nasıl denetlenir hakkında daha fazla bilgi edinmek için [bkz.](../../role-based-access-control/rbac-and-directory-admin-roles.md)
* Azure Active Directory ile Azure aboneliğinizin arasındaki ilişki hakkında bilgi için bkz. [Azure aboneliklerinin Azure Active Directory ile ilişkisi](../fundamentals/active-directory-how-subscriptions-associated-directory.md)
