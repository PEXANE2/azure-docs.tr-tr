---
title: B2B işbirliği ile davet kullanım-Azure AD
description: Son kullanıcılar için, gizlilik koşullarına yönelik anlaşma dahil olmak üzere Azure AD B2B işbirliği daveti kullanım deneyimini açıklar.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 05/11/2020
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: elisol
ms.collection: M365-identity-device-management
ms.openlocfilehash: d6bf5d40262c5991504d3dc62490fb50f6a20592
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/25/2020
ms.locfileid: "83826103"
---
# <a name="azure-active-directory-b2b-collaboration-invitation-redemption"></a>Azure Active Directory B2B işbirliği daveti kullanım

Bu makalede, konuk kullanıcıların kaynaklarınızda ve iletişim kurdukları onay sürecinde erişebileceği yollar açıklanmaktadır. Konuğa bir davet e-postası gönderirseniz, davet, konuğun uygulamanıza veya portala erişim sağlamak için yararlanana bir bağlantı içerir. Davet e-postası, konuklarınızın kaynaklarınıza erişebileceği yollarla yalnızca biridir. Alternatif olarak, dizininize Konuk ekleyebilir ve paylaşmak istediğiniz portalın ya da uygulamanın doğrudan bağlantısını sağlayabilirsiniz. Konukların, kullandıkları yöntemden bağımsız olarak, bir ilk kez onay süreci aracılığıyla yapılır. Bu işlem, konukların gizlilik koşullarını kabul etmesini ve ayarlamış olduğunuz tüm [kullanım koşullarını](https://docs.microsoft.com/azure/active-directory/governance/active-directory-tou) kabul etmesini sağlar.

Dizininize bir Konuk kullanıcı eklediğinizde, Konuk Kullanıcı hesabının başlangıçta **Pendingkabulünü**olarak ayarlanan bir izin durumu (PowerShell 'de görüntülenebilir) vardır. Bu ayar, Konuk davetinizi kabul edene ve gizlilik ilkenize ve kullanım koşullarınıza karşı kabul edilene kadar kalır. Bundan sonra, onay durumu **kabul edildi**olarak değişir ve izin sayfaları artık konuğa sunulmaz.

   > [!IMPORTANT]
   > Microsoft, **31 mart 2021**' den ıtıbaren, B2B işbirliği senaryoları Için YÖNETILMEYEN Azure AD hesapları ve kiracılar oluşturarak artık davetlerin kullanımını desteklememektedir. Hazırlık aşamasında, müşterilerin [e-posta bir kerelik geçiş kodu kimlik doğrulamasını](one-time-passcode.md)kabul etmelerini öneririz. Bu genel önizleme özelliğiyle ilgili geri bildirimlerinize hoş geldiniz ve işbirliği yapmak için daha fazla yol oluşturmak heyecanlıyız.

## <a name="redemption-through-the-invitation-email"></a>Davet e-postası üzerinden ödeme

[Azure Portal kullanarak](https://docs.microsoft.com/azure/active-directory/b2b/b2b-quickstart-add-guest-users-portal)dizininize bir Konuk kullanıcı eklediğinizde, işlemde konuğa bir davet e-postası gönderilir. Ayrıca, [PowerShell kullanırken](https://docs.microsoft.com/azure/active-directory/b2b/b2b-quickstart-invite-powershell) , dizininize Konuk kullanıcıları eklemek için davet e-postaları gönderilmesini de tercih edebilirsiniz. İşte e-postadaki bağlantıyı kullandıklarında konuğun deneyiminden oluşan bir açıklama.

1. Konuk, **Microsoft davetlerden**gönderilen bir [davet e-postası](https://docs.microsoft.com/azure/active-directory/b2b/invitation-email-elements) alır.
2. Konuk, e-postadaki **daveti kabul et** ' i seçer.
3. Konuk, dizininizde oturum açmak için kendi kimlik bilgilerini kullanır. Konukta dizininizden federe olabilecek bir hesap yoksa, [e-posta bir kerelik geçiş kodu (OTP)](https://docs.microsoft.com/azure/active-directory/b2b/one-time-passcode) özelliği etkin değildir; konuğa bir kişisel [MSA](https://support.microsoft.com/help/4026324/microsoft-account-how-to-create) veya BIR [Azure AD Self Servis hesabı](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-self-service-signup)oluşturması istenir. Ayrıntılar için [davet kullanım akışına](#invitation-redemption-flow) bakın.
4. Konuk, aşağıda açıklanan [onay deneyimine](#consent-experience-for-the-guest) göre yapılır.

## <a name="redemption-through-a-direct-link"></a>Doğrudan bir bağlantı üzerinden ödeme

Davet e-postasına alternatif olarak, bir konuğa uygulamanız veya portalınızın doğrudan bağlantısını sağlayabilirsiniz. Önce [Azure Portal](https://docs.microsoft.com/azure/active-directory/b2b/b2b-quickstart-add-guest-users-portal) veya [PowerShell](https://docs.microsoft.com/azure/active-directory/b2b/b2b-quickstart-invite-powershell)aracılığıyla konuk kullanıcıyı dizininize eklemeniz gerekir. Ardından doğrudan oturum açma bağlantıları dahil olmak üzere [kullanıcılara uygulama dağıtmak için özelleştirilebilen bir yol](https://docs.microsoft.com/azure/active-directory/manage-apps/end-user-experiences)kullanabilirsiniz. Bir konuk davet e-postası yerine doğrudan bağlantı kullandığında, yine de ilk kez onay deneyiminde gezinir.

> [!IMPORTANT]
> Doğrudan bağlantı, kiracıya özgü olmalıdır. Diğer bir deyişle, bir kiracı KIMLIĞI veya doğrulanmış etki alanı içermesi gerekir, bu sayede Konuk, paylaşılan uygulamanın bulunduğu kiracınızda kimlik doğrulaması yapılabilir. https://myapps.microsoft.comKimlik doğrulaması için kendi giriş kiracısına yönlendirireceği için, benzer ortak BIR URL, konuk için çalışmaz. Kiracı bağlamıyla doğrudan bağlantı örnekleri aşağıda verilmiştir:
 > - Uygulamalar erişim paneli:`https://myapps.microsoft.com/?tenantid=<tenant id>`
 > - Doğrulanmış bir etki alanı için uygulamalar erişim paneli:`https://myapps.microsoft.com/<;verified domain>`
 > - Azure portal:`https://portal.azure.com/<tenant id>`
 > - Tek uygulama: bkz. [doğrudan oturum açma bağlantısı](../manage-apps/end-user-experiences.md#direct-sign-on-links) kullanma

Davet e-postasında doğrudan bağlantı üzerinden önerilen bazı durumlar vardır. Bu özel durumlar kuruluşunuz için önemliyse, davet e-postasını hala gönderen yöntemleri kullanarak kullanıcıları davet etmenizi öneririz:
 - Kullanıcının Federasyon kuruluşunda bir Azure AD hesabı, MSA veya e-posta hesabı yoktur. Tek seferlik geçiş kodu özelliğini kullanmıyorsanız, bir MSA oluşturma adımlarında gezinmek için konuğun davet e-postasını sağlaması gerekir.
 - Bazen davet eden Kullanıcı nesnesinin bir e-posta adresi olmayabilir (örneğin, bir Outlook ilgili kişisi nesnesi). Bu durumda, Kullanıcı davet e-postasında kullanım URL 'sini tıklamalıdır.
 - Kullanıcı, davet edilen e-posta adresinin diğer adıyla oturum açabilir. (Diğer ad, bir e-posta hesabıyla ilişkili ek bir e-posta adresidir.) Bu durumda, Kullanıcı davet e-postasında kullanım URL 'sini tıklamalıdır.

## <a name="invitation-redemption-flow"></a>Davet teminat akışı

Bir Kullanıcı [davet e-postasında](invitation-email-elements.md) **daveti kabul et** BAĞLANTıSıNA tıkladığında, Azure AD aşağıda gösterildiği gibi kullanım akışına göre daveti otomatik olarak alır:

![Kullanım akışı diyagramını gösteren ekran görüntüsü](media/redemption-experience/invitation-redemption-flow.png)

**Kullanıcının Kullanıcı asıl adı (UPN) hem var olan bir Azure AD hem de kişisel MSA hesabıyla eşleşiyorsa, kullanıcıdan hangi hesabı kullanmak istediğini seçmesi istenir.*

1. Azure AD, kullanıcının [mevcut bir Azure AD kiracısında](https://docs.microsoft.com/azure/active-directory/b2b/what-is-b2b#easily-add-guest-users-in-the-azure-ad-portal)bulunup bulunmadığını belirlemede Kullanıcı tabanlı bulma işlemini gerçekleştirir.

2. Yönetici [doğrudan Federasyonu](https://docs.microsoft.com/azure/active-directory/b2b/direct-federation)etkinleştirmişse, Azure AD kullanıcının etki alanı son ekinin YAPıLANDıRıLMıŞ bir SAML/WS-beslik kimlik sağlayıcısının etki alanıyla eşleşip eşleşmediğini denetler ve kullanıcıyı önceden yapılandırılmış kimlik sağlayıcısına yönlendirir.

3. Bir yönetici [Google Federasyonu](https://docs.microsoft.com/azure/active-directory/b2b/google-federation)etkinleştirmişse, Azure AD kullanıcının etki alanı sonekinin gmail.com veya googlemail.com olup olmadığını denetler ve kullanıcıyı Google 'a yönlendirir.

4. Kullanım süreci, kullanıcının mevcut bir kişisel [Microsoft hesabı (MSA)](https://support.microsoft.com/help/4026324/microsoft-account-how-to-create)olup olmadığını denetler.

5. Kullanıcının **giriş dizini** tanımlandıktan sonra, Kullanıcı oturum açmak için ilgili kimlik sağlayıcısına gönderilir.  

6. Davet edilen kullanıcı için 1 ile 4 arasındaki adımlar başarısız olursa, Azure AD, davet eden kiracının guests için [bir kerelik geçiş kodu (OTP)](https://docs.microsoft.com/azure/active-directory/b2b/one-time-passcode) özelliğinin etkinleştirilip etkinleştirilmediğini belirler.

7. [Konuklar için bir kerelik geçiş kodu etkinleştirilirse](https://docs.microsoft.com/azure/active-directory/b2b/one-time-passcode#when-does-a-guest-user-get-a-one-time-passcode), davet edilen e-posta aracılığıyla kullanıcıya bir geçiş kodu gönderilir. Kullanıcı Azure AD oturum açma sayfasında bu geçiş kodunu alır ve girer.

8. Konuklarınız için bir kerelik geçiş kodu devre dışıysa Azure AD, bir tüketici hesabına ait olup olmadığını öğrenmek için etki alanı sonekini denetler. Öyleyse, kullanıcıdan kişisel [Microsoft hesabı](https://support.microsoft.com/help/4026324/microsoft-account-how-to-create)oluşturması istenir. Aksi takdirde, kullanıcıdan bir [Azure AD Self Servis hesabı](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-self-service-signup)oluşturması istenir.

9. Azure AD, e-postaya erişimi doğrulayarak bir [Azure AD Self Servis hesabı](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-self-service-signup) oluşturmaya çalışır. Hesap, e-postaya bir kod göndererek ve kullanıcının bunu alıp Azure AD 'ye göndermesini sağlayarak yapılır. Ancak, davet edilen kullanıcının kiracısı federe ise veya davet edilen kullanıcının kiracısında AllowEmailVerifiedUsers alanı false olarak ayarlandıysa, Kullanıcı kullanım alanını tamamlayamaz ve Flow bir hatayla sonuçlanır. Daha fazla bilgi için bkz. [B2B işbirliği Azure Active Directory sorun giderme](https://docs.microsoft.com/azure/active-directory/b2b/troubleshoot#the-user-that-i-invited-is-receiving-an-error-during-redemption).

10. Kullanıcıdan bir kişisel [Microsoft hesabı (MSA)](https://support.microsoft.com/help/4026324/microsoft-account-how-to-create)oluşturması istenir.

11. Doğru kimlik sağlayıcısında kimlik doğrulamasından geçtikten sonra, Kullanıcı [onay deneyimini](https://docs.microsoft.com/azure/active-directory/b2b/redemption-experience#consent-experience-for-the-guest)tamamlamaya YÖNELIK Azure AD 'ye yeniden yönlendirilir.  

Tam zamanında (JıT) kampanyaya için, satın alma, kiralanan bir uygulama bağlantısı aracılığıyla olduğunda, 8 ile 10 arasındaki adımlar kullanılamaz. Bir Kullanıcı 6. adıma ulaşırsa ve tek seferlik geçiş kodu özelliği etkinleştirilmemişse, Kullanıcı bir hata iletisi alır ve daveti kullanmayabilir. Bu hatayı engellemek için, yöneticiler e- [posta tek seferlik geçiş kodunu etkinleşmelidir](https://docs.microsoft.com/azure/active-directory/b2b/one-time-passcode#when-does-a-guest-user-get-a-one-time-passcode) veya kullanıcının bir davet bağlantısına tıklamasını sağlar.

## <a name="consent-experience-for-the-guest"></a>Konuk için onay deneyimi

Bir konuk, bir iş ortağı kuruluşundaki kaynaklara ilk kez erişmek üzere oturum açtığında, bunlar aşağıdaki sayfalara kılavuzluk ederler. 

1. Konuk, davet eden kuruluşun gizlilik bildirimini açıklayan **Izinleri gözden geçirme** sayfasını inceler. Bir kullanıcının devam etmesi için bilgilerin kullanımını davet eden kuruluşun gizlilik ilkelerine uygun olarak **kabul etmesi** gerekir.

   ![İzinleri gözden geçir sayfasını gösteren ekran görüntüsü](media/redemption-experience/review-permissions.png) 

   > [!NOTE]
   > Kiracı Yöneticisi olarak kuruluşunuzun gizlilik bildirimine nasıl bağlayabileceğiniz hakkında bilgi için bkz. [nasıl yapılır: kuruluşunuzun gizlilik bilgilerini Azure Active Directory ekleme](https://aka.ms/adprivacystatement).

2. Kullanım koşulları yapılandırılırsa, Konuk açılıp kullanım koşullarını inceler ve **kabul et**' i seçer. 

   ![Yeni kullanım koşullarını gösteren ekran görüntüsü](media/redemption-experience/terms-of-use-accept.png) 

   **Dış kimliklerden** [kullanım koşullarını](../governance/active-directory-tou.md) yapılandırabilirsiniz  >  **kullanım koşulları**.

3. Aksi belirtilmediği takdirde, Konuk, konuk tarafından erişebilen uygulamaları listeleyen uygulamalar erişim paneline yönlendirilir.

   ![Uygulamalar erişim panelini gösteren ekran görüntüsü](media/redemption-experience/myapps.png) 

Dizininizde, Konuk **davetinin kabul edilebilir** değeri **Evet**olarak değişir. Bir MSA oluşturulduysa, Konuk **kaynağı** **Microsoft hesabını**gösterir. Konuk Kullanıcı hesabı özellikleri hakkında daha fazla bilgi için bkz. [Azure AD B2B işbirliği kullanıcısının özellikleri](user-properties.md). 

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD B2B işbirliği nedir?](what-is-b2b.md)
- [Azure portal Azure Active Directory B2B işbirliği kullanıcıları ekleyin](add-users-administrator.md)
- [Bilgi çalışanları, Azure Active Directory B2B işbirliği kullanıcılarını nasıl ekler?](add-users-information-worker.md)
- [PowerShell kullanarak Azure Active Directory B2B işbirliği kullanıcıları ekleme](customize-invitation-api.md#powershell)
- [Bir kuruluştan Konuk Kullanıcı olarak ayrılın](leave-the-organization.md)
