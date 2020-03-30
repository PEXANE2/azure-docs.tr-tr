---
title: B2B işbirliğinde davet kullanımı - Azure AD
description: Azure AD B2B işbirliği daveti kullanım deneyimini, gizlilik koşulları anlaşması da dahil olmak üzere son kullanıcılar için açıklar.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 03/19/2020
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: elisol
ms.collection: M365-identity-device-management
ms.openlocfilehash: 043e0f3a0ff2c1c642c63a387c571b575f77cf7d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80050831"
---
# <a name="azure-active-directory-b2b-collaboration-invitation-redemption"></a>Azure Active Directory B2B işbirliği daveti ödeme

Bu makalede, konuk kullanıcıların kaynaklarınıza nasıl erişebileceği ve karşılaşacakları onay süreci açıklanmaktadır. Davete davet e-postası gönderirseniz, davet, konuğun uygulamanıza veya portalınıza erişmek için kullanabileceği bir bağlantı içerir. Davet e-postası, konukların kaynaklarınıza erişebildiği yollardan sadece biridir. Alternatif olarak, rehberinize konuk ekleyebilir ve paylaşmak istediğiniz portal veya uygulamaya doğrudan bağlantı verebilirsiniz. Kullandıkları yöntemne bakılmaksızın, konuklar a.b.d. Bu işlem, misafirlerinizin gizlilik koşullarını kabul etmesini ve ayarladığınız [tüm kullanım koşullarını](https://docs.microsoft.com/azure/active-directory/governance/active-directory-tou) kabul etmesini sağlar.

Dizininize bir konuk kullanıcı eklediğinizde, konuk kullanıcı hesabının başlangıçta **BeklemeKabul**olarak ayarlanmış bir onay durumu (PowerShell'de görüntülenebilir). Bu ayar, konuk davetinizi kabul edene ve gizlilik politikanızı ve kullanım koşullarınızı kabul edene kadar kalır. Bundan sonra, onay durumu **Kabul edilir**ve onay sayfaları artık misafire sunulmaz.

   > [!IMPORTANT]
   > **31 Mart 2021'den itibaren Microsoft,** B2B işbirliği senaryoları için yönetilmeyen Azure AD hesapları ve kiracılar oluşturarak artık davetlerin itfasını desteklemez. Hazırlık olarak, müşterilerin tek [seferlik parola kimlik doğrulaması e-postasını](one-time-passcode.md)seçmelerini öneririz. Bu genel önizleme özelliği hakkındaki görüşlerinizi memnuniyetle karşılıyoruz ve işbirliği yapmak için daha fazla yol oluşturmaktan heyecan duyuyoruz.

## <a name="redemption-through-the-invitation-email"></a>Davet e-postası aracılığıyla kullanım

[Azure portalını kullanarak](https://docs.microsoft.com/azure/active-directory/b2b/b2b-quickstart-add-guest-users-portal)dizininize bir konuk kullanıcı eklediğinizde, bu süreçte misafire bir davet e-postası gönderilir. Ayrıca, dizininize konuk kullanıcı eklemek için [PowerShell'i kullanırken](https://docs.microsoft.com/azure/active-directory/b2b/b2b-quickstart-invite-powershell) davet e-postaları göndermeyi de seçebilirsiniz. E-postadaki bağlantıyı kullandıklarında konuğun deneyiminin bir açıklaması aşağıda veda edebilirsiniz.

1. Konuk, **Microsoft Invitations'tan**gönderilen bir davet [e-postası](https://docs.microsoft.com/azure/active-directory/b2b/invitation-email-elements) alır.
2. Konuk e-postada **Başlat'ı** seçer.
3. Konuğun federe bir kuruluşta Azure AD hesabı, Microsoft Hesabı (MSA) veya e-posta hesabı yoksa, msa oluşturmaları istenir [(bir kerelik geçiş kodu](https://docs.microsoft.com/azure/active-directory/b2b/one-time-passcode) özelliği etkinleştirilmediği sürece ve MSA gerektirmez).
4. Konuk aşağıda açıklanan [rıza deneyimi](#consent-experience-for-the-guest) ile yönlendirilir.

## <a name="redemption-through-a-direct-link"></a>Doğrudan bağlantı üzerinden kullanım

Davet e-postasına alternatif olarak, bir misafire uygulamanıza veya portalınıza doğrudan bağlantı verebilirsiniz. Öncelikle Azure [portalı](https://docs.microsoft.com/azure/active-directory/b2b/b2b-quickstart-add-guest-users-portal) veya [PowerShell](https://docs.microsoft.com/azure/active-directory/b2b/b2b-quickstart-invite-powershell)üzerinden konuk kullanıcıyı dizininize eklemeniz gerekir. Ardından, uygulamaları doğrudan oturum açma bağlantıları da dahil olmak üzere [kullanıcılara dağıtmanın özelleştirilebilir yollarından](https://docs.microsoft.com/azure/active-directory/manage-apps/end-user-experiences)herhangi birini kullanabilirsiniz. Bir konuk davet e-postası yerine doğrudan bir bağlantı kullandığında, yine de ilk kez izin deneyimi boyunca yönlendirilir.

> [!IMPORTANT]
> Doğrudan bağlantı kiracıya özel olmalıdır. Başka bir deyişle, konuğun paylaşılan uygulamanın bulunduğu kiracınızda kimliğinin doğrulanabilmesi için kiracı kimliği veya doğrulanmış etki alanı içermelidir. Kimlik doğrulaması https://myapps.microsoft.com için ev kiracısına yönlendireceği için, bu url gibi ortak bir URL bir konuk için çalışmaz. Kiracı bağlamıyla doğrudan bağlantıya bazı örnekler aşağıda verilmiştir:
 > - Uygulamalar erişim https://myapps.microsoft.com/?tenantid=&ltpaneli: ;kiracı kimliği&gt; 
 > - Doğrulanmış bir etki alanı https://myapps.microsoft.com/&ltiçin uygulamalar erişim paneli: ;doğrulanmış etki alanı&gt;
 > - Azure portalı: https://portal.azure.com/&lt;kiracı kimliği&gt;
 > - Bireysel uygulama: [doğrudan oturum açma bağlantısını](../manage-apps/end-user-experiences.md#direct-sign-on-links) nasıl kullanacağız

Davet e-postasının doğrudan bir bağlantı üzerinden tavsiye edildiği bazı durumlar vardır. Bu özel durumlar kuruluşunuz için önemliyse, davet e-postasını hala gönderen yöntemleri kullanarak kullanıcıları davet etmenizi öneririz:
 - Kullanıcının federe bir kuruluşta Azure AD hesabı, MSA'sı veya e-posta hesabı yoktur. Tek seferlik parola özelliğini kullanmıyorsanız, konuğun MSA oluşturmak için adımlarda yönlendirilmesi için davet e-postasını kullanması gerekir.
 - Bazen davet edilen kullanıcı nesnesi, ilgili kişi nesnesiyle (örneğin, Outlook kişi nesnesi) çakışması nedeniyle e-posta adresine sahip olmayabilir. Bu durumda, kullanıcının davet e-postasındakullanım URL'sini tıklatması gerekir.
 - Kullanıcı, davet edilen e-posta adresinin takma adı ile oturum açabilir. (Takma ad, bir e-posta hesabıyla ilişkili ek bir e-posta adresidir.) Bu durumda, kullanıcının davet e-postasındakullanım URL'sini tıklatması gerekir.

## <a name="consent-experience-for-the-guest"></a>Konuk için rıza deneyimi

Bir konuk, bir ortak kuruluştaki kaynaklara erişmek için ilk kez giriş yaptığında, aşağıdaki sayfalarda yönlendirilir. 

1. Konuk, davet eden kuruluşun gizlilik bildirimini açıklayan **İnceleme izinleri** sayfasını inceler. Bir kullanıcı, bilgilerinin davet edilen kuruluşun gizlilik politikalarına uygun olarak kullanımını **kabul** etmelidir.

   ![İnceleme izinleri sayfasını gösteren ekran görüntüsü](media/redemption-experience/review-permissions.png) 

   > [!NOTE]
   > Kiracı yönetici olarak kuruluşunuzun gizlilik bildirimine nasıl bağlantı kurabileceğiniz hakkında bilgi için [bkz.](https://aka.ms/adprivacystatement)

2. Kullanım koşulları yapılandırılırsa, konuk açılışını yapıp kullanım koşullarını gözden geçirir ve sonra **Kabul Et'i**seçer. 

   ![Yeni kullanım koşullarını gösteren ekran görüntüsü](media/redemption-experience/terms-of-use-accept.png) 

   > [!NOTE]
   > **Kuruluş**ilişkileri >  **Yönetme** > Kullanım [Koşullarını](../governance/active-directory-tou.md) **görebilirsiniz.**

3. Aksi belirtilmedikçe, konuk, konuğun erişebileceği uygulamaları listeleyen Uygulamalar erişim paneline yönlendirilir.

   ![Uygulamalar erişim panelini gösteren ekran görüntüsü](media/redemption-experience/myapps.png) 

Dizininizde, konuğun **Daveti** kabul edilen değer değişiklikleri **Evet**olarak değiştirilmiştir. Bir MSA oluşturulduysa, konuğun **Kaynağı** **Microsoft Hesabı'nı**gösterir. Konuk kullanıcı hesabı özellikleri hakkında daha fazla bilgi için Azure [AD B2B işbirliği kullanıcısının özellikleri](user-properties.md)bölümüne bakın. 

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD B2B işbirliği nedir?](what-is-b2b.md)
- [Azure portalında Azure Active Directory B2B işbirliği kullanıcıları ekleme](add-users-administrator.md)
- [Bilgi çalışanları Azure Active Directory'ye B2B işbirliği kullanıcılarını nasıl ekler?](add-users-information-worker.md)
- [PowerShell'i kullanarak Azure Active Directory B2B işbirliği kullanıcıları ekleme](customize-invitation-api.md#powershell)
- [Bir kuruluşu konuk kullanıcı olarak bırakma](leave-the-organization.md)
