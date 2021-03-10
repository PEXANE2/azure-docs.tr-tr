---
title: Azure Active Directory ile Microsoft ekiplerine, SharePoint 'e ve OneDrive 'a dış erişimi güvenli hale getirme
description: Genel dış erişim güvenlerinizin bir parçası olarak Microsoft 365 hizmetlerine güvenli erişim.
services: active-directory
author: BarbaraSelden
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 12/18/2020
ms.author: baselden
ms.reviewer: ajburnle
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: f373c849cd542769fec7a10f20dad04e742c9591
ms.sourcegitcommit: 7edadd4bf8f354abca0b253b3af98836212edd93
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/10/2021
ms.locfileid: "102565147"
---
# <a name="secure-external-access-to-microsoft-teams-sharepoint-and-onedrive-for-business"></a>Microsoft ekiplerine, SharePoint 'e ve OneDrive Iş 'e güvenli dış erişim 

Microsoft ekipleri, SharePoint ve OneDrive Iş, dış kullanıcılarla işbirliği yapmak ve içerik paylaşmak için en çok kullanılan yöntemlere yöneliktir. "Onaylanan" yöntemler çok kısıtlayıcıysa, kullanıcılar içerik göndererek veya kişisel DropBox veya OneDrive gibi güvenli olmayan dış süreçler ve uygulamalar ayarlayarak onaylanan Yöntemler dışında kalır. Amacınız, güvenlik gereksinimlerinizin işbirliği kolaylığıyla dengelenmesi sağlamaktır.

Bu makalede, Microsoft ekipleri ve SharePoint kullanarak iş hedeflerinizi karşılamak üzere dış işbirliğini belirleme ve yapılandırma yönergeleri yer almaktadır.

## <a name="governance-begins-in-azure-active-directory"></a>İdare Azure Active Directory ' de başlar

Microsoft 365 paylaşım [dış kimliklerin kapsamında Azure Active Directory 'de dış işbirliği ayarları](https://aad.portal.azure.com/) (Azure AD). Azure AD 'de dış paylaşım devre dışıysa veya kısıtlanmışsa, Microsoft 365 ' de yapılandırılan tüm paylaşım ayarlarını geçersiz kılar. Bunun bir özel durumu, Azure AD B2B tümleştirmesi etkinleştirilmemişse, SharePoint ve OneDrive, tek seferlik geçiş kodları (OTP) aracılığıyla geçici paylaşımı destekleyecek şekilde yapılandırılabilirler.

![Dış işbirliği ayarlarının ekran görüntüsü](media/secure-external-access/9-external-collaboration-settings.png)

### <a name="guest-user-access"></a>Konuk kullanıcı erişimi

Konuk Kullanıcı erişimi için, davet edildikten sonra hangi konuk kullanıcıların görebileceklerini denetleyen üç seçenek vardır. 

Konuk kullanıcıların diğer konuk kullanıcılarının ayrıntılarını görmesini engellemek ve grup üyeliğini numaralandırabilmek için, konuk kullanıcıların, dizin nesnelerinin özelliklerine ve üyeliklerine sınırlı erişimi vardır.

### <a name="guest-invite-settings"></a>Konuk davet ayarları

Bu ayarlar, kimlerin konukları davet edebilir ve bu konuklara nasıl davet edilerişebileceğini belirlemektir. Bu ayarlar yalnızca B2B tümleştirmesi etkinse etkindir.

Konuk davet eden rolündeki yöneticilerin ve kullanıcıların davet edilmesini öneririz. Bu ayar, aşağıdaki örnekte olduğu gibi denetimli işbirliği işlemlerinin ayarlanmasını sağlar.

* Takım sahibi, konuk davet eden rolüne atanacak bir bilet gönderir ve

   * Tüm konuk davetlerden sorumlu olur.

   * Doğrudan temel SharePoint 'e Kullanıcı eklememe kabul eder

   * , Düzenli erişim İncelemeleri gerçekleştirmek ve erişimi uygun şekilde iptal etmek için muhasebeci tablosudur.

* Merkezi aşağıdaki

   * Eğitim tamamlandıktan sonra istenen rolü vererek dış paylaşımı etkinleştirilir.

   * Erişim incelemelerini etkinleştirmek için Microsoft 365 Grubu sahibine Azure AD P2 lisansı atar.
   * Microsoft 365 grubu erişim incelemesi oluşturur.

   * Erişim incelemelerinin gerçekleştiğini onaylar.

   * Temel alınan SharePoint 'e doğrudan eklenen kullanıcıları kaldırır.

 **Ziyaretçi Için tek seferlik parolaları etkinleştir (Önizleme) ve Kullanıcı akışları aracılığıyla konuk self servis Imzasını etkinleştir** ayarını **Evet** olarak belirleyin. Bu ayar, Azure AD dış işbirliği ayarlarıyla tümleştirmeden yararlanır.

### <a name="collaboration-restrictions"></a>İşbirliği kısıtlamaları

İşbirliği kısıtlamaları altında üç seçenek vardır. İş gereksinimleriniz şunları seçmenizi ister.

* **Herhangi bir etki alanına davetlerin gönderilmesine Izin ver** , herhangi bir kullanıcının işbirliği yapmaya davet edilmesi anlamına gelir.

* **Belirtilen etki alanlarına yapılan davetleri reddetme** , bunların dışındaki tüm kullanıcıların işbirliği için davet edilmeyeceği anlamına gelir.

* **Yalnızca belirtilen etki alanlarına davetlere Izin ver** belirtilen etki alanlarının dışındaki herhangi bir kullanıcının davet edilmeyeceği anlamına gelir. 

## <a name="govern-access-in-teams"></a>Ekiplere erişimi yönetir

[Takımlar harici kullanıcılar (kuruluşunuzun dışındaki herkes) ve Konuk kullanıcılar (Konuk hesapları olan) arasında farklılık](/microsoftteams/communicate-with-users-from-other-organizations?WT.mc_id=TeamsAdminCenterCSH%e2%80%8b)gösterir. Kurumsal çapta ayarlar altında [takımlar yönetici portalında](https://admin.teams.microsoft.com/company-wide-settings/external-communications) işbirliği ayarını yönetirsiniz. 

> [!NOTE]
> Dış kimlikler işbirliği ayarları Azure Active Directory etkin izinleri denetler. Ekipteki kısıtlamaları artırabilir, ancak bunları Azure AD 'de ayarlandıklarından azaltamazsınız.

* **Dış erişim ayarları**. Varsayılan olarak, takımlar dış erişime izin verir, bu da kuruluşun tüm dış etki alanlarıyla iletişim kurabildiği anlamına gelir. Yalnızca takımlar için belirli etki alanlarını kısıtlamak veya izin vermek istiyorsanız, bunu burada yapabilirsiniz.

* **Konuk erişimi**. Konuk erişimi, konuk kullanıcıların ekiplerde neler yapabileceğini denetler.

Ekiplerde dış erişimin yönetilmesi hakkında daha fazla bilgi edinmek için aşağıdaki kaynaklara bakın.

* [Microsoft ekiplerinde dış erişimi yönetme](/microsoftteams/manage-external-access)

* [Microsoft 365 kimlik modelleri ve Azure Active Directory](/microsoft-365/enterprise/about-microsoft-365-identity)

* [Microsoft ekipleri için kimlik modelleri ve kimlik doğrulaması](/MicrosoftTeams/identify-models-authentication)

* [Microsoft ekipleri için duyarlılık etiketleri](/MicrosoftTeams/sensitivity-labels)

## <a name="govern-access-in-sharepoint-and-onedrive"></a>SharePoint ve OneDrive 'da erişimi yönetir

SharePoint yöneticileri işbirliği için birçok ayarı vardır. Şirket genelinde ayarlar SharePoint Yönetim merkezinden yönetilir. Ayarlar, her bir SharePoint sitesi için ayarlanabilir. Kuruluşunuzun genelindeki ayarlarınızı gereken en düşük güvenlik düzeylerinde olmasını ve gerektiğinde belirli sitelerde güvenliği artırmanız önerilir. Örneğin, yüksek riskli bir proje için kullanıcıları belirli etki alanlarına kısıtlamak ve üyelerin konukları davet etme yeteneğini devre dışı bırakmak isteyebilirsiniz.

### <a name="integrating-sharepoint-and-one-drive-with-azure-ad-b2b"></a>Azure AD B2B ile SharePoint ve tek sürücülü tümleştirme

Dış işbirliğini yönetmek için genel stratejinizin bir parçası olarak, [SharePoint ve OneDrive tümleştirmesinin Azure AD B2B Ile önizlemesini etkinleştirmenizi](/sharepoint/sharepoint-azureb2b-integration-preview) öneririz.

Azure AD B2B, konuk kullanıcıların kimlik doğrulaması ve yönetimi sağlar. SharePoint ve OneDrive tümleştirmesi sayesinde, dosyaların, klasörlerin, liste öğelerinin, belge kitaplıklarının ve sitelerin dışarıdan paylaşılması için [Azure AD B2B tek seferlik geçiş kodlarını](../external-identities/one-time-passcode.md) kullanılır. Bu özellik, var olan [güvenli dış paylaşım alıcısı deneyiminden](/sharepoint/what-s-new-in-sharing-in-targeted-release)yükseltilen bir deneyim sağlar.

> [!NOTE]
> Azure AD B2B tümleştirmesi için Önizlemeyi etkinleştirirseniz, SharePoint ve OneDrive paylaşımı, **Üyeler davet edebilir** ve **konuklarınız davet** edebilir.

### <a name="sharing-policies"></a>İlkeleri paylaşma

*Dış paylaşım* , hem SharePoint hem de OneDrive için ayarlanabilir. OneDrive kısıtlamaları, SharePoint ayarlarından daha fazla izin veremez.

 ![SharePoint ve OneDrive 'daki dış paylaşım ayarlarının ekran görüntüsü](media/secure-external-access/9-sharepoint-settings.png)

Azure AD B2B ile SharePoint tümleştirmesi, denetimlerin hesaplarla nasıl etkileşime gireceğini değiştirir.

* **Herkes**. Önerilmez

   * Tümleştirme durumundan bağımsız olarak, herhangi bir bağlantının etkinleştirilmesi, bu tür bir bağlantı kullanıldığında hiçbir Azure ilkesinin uygulanmayacağı anlamına gelir. 

   * Bir yönetilen işbirliği senaryosunda, bu işlevselliği etkinleştirmeyin. 
   > [!NOTE]
   > Belirli bir site için bu ayarı etkinleştirmeniz gereken bir senaryo bulabilirsiniz, bu durumda burada etkinleştirmeniz ve ayrı sitelerde daha fazla kısıtlama ayarlamanız gerekir.

* **Yeni ve mevcut konuklar**. Tümleştirme etkinse önerilir.

   * **Azure AD B2B tümleştirmesinin etkin olduğu** , yeni ve mevcut KONUKLARıN Azure AD ilkeleriyle yönetilebilen BIR Azure AD B2B Konuk hesabı olacaktır.

   * **Azure AD B2B tümleştirmesi etkin olmadan** , yeni konukların oluşturulmuş BIR Azure AD B2B hesabı yoktur ve bunlar Azure AD 'den yönetilemez. Mevcut konukların bir Azure AD B2B hesabı olup olmadığı, konuğun nasıl oluşturulduğuna bağlıdır.

* **Mevcut konuklar**. Tümleştirme özelliği etkinleştirilmemişse önerilir.

   * Bu etkinken, kullanıcılar yalnızca dizininizde bulunan diğer kullanıcılarla paylaşabilir.

* **Yalnızca kuruluşunuzdaki kişiler**. Dış kullanıcılarla işbirliği yapmanız gerektiğinde önerilmez.

   * Tümleştirme durumundan bağımsız olarak, kullanıcılar yalnızca kuruluşunuzdaki kullanıcılarla paylaşabilir. 

* **Etki alanı ile dış paylaşımı sınırlayın**. Varsayılan olarak, SharePoint dış erişime izin verir, yani tüm dış etki alanlarıyla ilgili paylaşıma izin verilir. Yalnızca SharePoint için belirli etki alanlarını kısıtlamak veya buna izin vermek istiyorsanız, bunu burada yapabilirsiniz.

* **Yalnızca belirli güvenlik gruplarındaki kullanıcıların dışarıdan paylaşmasına Izin ver**.  Bu ayar, SharePoint ve OneDrive 'da içeriği paylaşabileceği, ancak Azure AD 'deki ayar tüm uygulamalar için geçerlidir. Kullanıcılarınızın güvenli bir şekilde paylaşım hakkında bir eğitim geçirmesine gerek olmasını sağlamak istiyorsanız, kimlerin paylaşılacağını kısıtlamak yararlı olabilir, sonra bunları onaylanan paylaşım güvenlik grubuna ekleyin. Bu ayar seçilirse ve kullanıcılar "onaylı paylaşan" olarak erişim elde etmek için bir yol yoksa, bunun yerine paylaşmak için onaylanmamış yollar bulabilir. 

* **Konukların sahip olmadıkları öğeleri paylaşmasına Izin verin**. Bunu devre dışı bırakmanızı öneririz.

* **Doğrulama kodu kullanan kişilerin bu kadar gün sonra yeniden doğrulanması gerekir (varsayılan değer 30 ' dur)**. Bu ayarın etkinleştirilmesini öneririz.

### <a name="access-controls"></a>Erişim denetimleri

Erişim denetimleri ayarı, kuruluşunuzdaki tüm kullanıcıları etkiler. Dış kullanıcıların uyumlu cihazlara sahip olup olmadığını denetleyemeyebilirsiniz, bu denetimleri burada ele vermeyecektir. 

* **Boştaki oturum oturumu kapatma**. Bu denetimi etkinleştirmenizi ve bir süre etkinlik dışı kaldıktan sonra kullanıcıları yönetilmeyen cihazlarda uyarmasını ve oturum açmanızı sağlayan bu denetimi etkinleştirmenizi öneririz. İşlem yapılmadan ve uyarının süresini yapılandırabilirsiniz. 

* **Ağ konumu**. Bu denetimi ayarlamak, yalnızca kuruluşunuzun sahip olduğu IP adreslerini erişime izin vermek için kullanabileceğiniz anlamına gelir. Dış işbirliği senaryolarında bunu yalnızca tüm dış iş ortaklarınızın ağınız içindeki kaynaklara ya da VPN 'niz aracılığıyla erişebileceği şekilde ayarlayın.

### <a name="file-and-folder-links"></a>Dosya ve klasör bağlantıları

SharePoint Yönetim merkezinde, dosya ve klasör bağlantılarının nasıl paylaşılacağını da ayarlayabilirsiniz. Ayrıca, bu ayarları her site için yapılandırabilirsiniz. 

 ![Dosya ve klasör bağlantısı ayarlarının ekran görüntüsü](media/secure-external-access/9-file-folder-links.png)

Azure AD B2B ile tümleştirmeyi etkinleştirdiyseniz dosya ve klasörlerin kuruluş dışından paylaşılması, dosyalar ve klasör paylaşıldığında bir B2B kullanıcısının oluşturulması ile sonuçlanır.

Varsayılan bağlantı türünü **yalnızca kuruluşunuzdaki insanlarla** ve **düzenleme** için varsayılan izinlerle ayarlamayı öneririz. Bunun yapılması, öğelerin daha dar bir şekilde paylaşılmasını sağlar. Daha sonra bu ayarı, belirli işbirliği ihtiyaçlarını karşılayan site başına varsayılan için özelleştirebilirsiniz.

### <a name="anyone-links"></a>Herkes bağlantı

Herhangi bir bağlantıyı etkinleştirmenizi önermeyiz. Bunu yaparsanız, bir süre sonu ayarlamayı öneririz ve izinleri görüntülemek için kısıtlamayı düşünün. Dosyalar veya klasörler için yalnızca görünüm izinlerini seçerseniz, kullanıcılar düzenleme ayrıcalıklarını dahil etmek için herhangi bir bağlantıyı değiştiremezler.

SharePoint 'e dış erişimi yönetmek hakkında daha fazla bilgi edinmek için aşağıdakilere bakın:

* [SharePoint dış paylaşımına genel bakış](/sharepoint/external-sharing-overview)

* [Azure AD B2B ile SharePoint ve OneDrive tümleştirmesi](/sharepoint/sharepoint-azureb2b-integration-preview)

#### <a name="next-steps"></a>Sonraki adımlar

Kaynaklara dışarıdan erişimi güvenli hale getirmek için aşağıdaki makalelere bakın. Eylemleri listelenen sırayla yapmanızı öneririz.

1. [Dış erişim için güvenlik durunuzu belirleme](1-secure-access-posture.md)

2. [Geçerli durumlarınızı bulma](2-secure-access-current-state.md)

3. [İdare planı oluşturma](3-secure-access-plan.md)

4. [Güvenlik için grupları kullanma](4-secure-access-groups.md)

5. [Azure AD B2B 'ye geçiş](5-secure-access-b2b.md)

6. [Yetkilendirme yönetimiyle güvenli erişim](6-secure-access-entitlement-managment.md)

7. [Koşullu erişim ilkeleriyle güvenli erişim](7-secure-access-conditional-access.md)

8. [Duyarlılık etiketleriyle güvenli erişim](8-secure-access-sensitivity-labels.md)

9. [Microsoft ekipleri, OneDrive ve SharePoint 'e güvenli erişim](9-secure-access-teams-sharepoint.md) (burada bulabilirsiniz.)