---
title: Secure resources with Azure MFA and ADFS - Azure Active Directory
description: Bu, bulutta nasıl Azure MFA ve AD FS kullanmaya başlayacağınızı açıklayan Azure Multi-Factor Authentication sayfasıdır.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 20e9f0b9a993d2a8af9b9d91399fdd49f9a06eaa
ms.sourcegitcommit: f523c8a8557ade6c4db6be12d7a01e535ff32f32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/22/2019
ms.locfileid: "74381842"
---
# <a name="securing-cloud-resources-with-azure-multi-factor-authentication-and-ad-fs"></a>Azure Multi-Factor Authentication ve AD FS ile bulut kaynaklarını güvenli hale getirme

Kuruluşunuz Azure Active Directory ile birleştiriliyorsa, Azure AD’nin erişebildiği kaynakları güvenli hale getirmek için Azure Multi-Factor Authentication ya da Active Directory Federation Services (AD FS) kullanın. Azure Active Directory kaynaklarını Azure Multi-Factor Authentication ya da Active Directory Federasyon Hizmetleri ile güvenli hale getirmek için aşağıdaki yordamları kullanın.

## <a name="secure-azure-ad-resources-using-ad-fs"></a>AD FS kullanarak Azure AD kaynaklarını güvenli hale getirme

Bulut kaynağınızın güvenliğini sağlamak için, kullanıcı iki adımlı doğrulamayı başarıyla gerçekleştirdiğinde Active Directory Federation Services tarafından multipleauthn talebinin yayılması için bir talep kuralı oluşturun. Bu talep Azure AD'ye aktarılır. İlerlemek için bu yordamı izleyin:

1. AD FS Yönetimi'ni açın.
2. Solda, **Bağlı Olan Taraf Güvenleri**’ni seçin.
3. **Microsoft Office 365 Kimlik Platformu**'na sağ tıklayın ve **Talep Kurallarını Düzenle**'yi seçin.

   ![ADFS Console - Relying Party Trusts](./media/howto-mfa-adfs/trustedip1.png)

4. Verme Dönüştürme Kuralları’nda **Kural Ekle**’ye tıklayın.

   ![Editing Issuance Transform Rules](./media/howto-mfa-adfs/trustedip2.png)

5. Dönüştürme Kuralı Ekleme Sihirbazı’nda, açılır menüde **Gelen Talep için Geçiş ya da Filtre**’yi seçin ve **İleri**’ye tıklayın.

   ![Add Transform Claim Rule Wizard](./media/howto-mfa-adfs/trustedip3.png)

6. Kuralınıza bir ad verin. 
7. Gelen talep türü olarak **Kimlik Doğrulama Yöntemleri Başvuruları**’nı seçin.
8. **Tüm talep değerlerini geçir**’i seçin.
    ![Dönüşüm Talep Kuralı Ekleme Sihirbazı](./media/howto-mfa-adfs/configurewizard.png)
9. **Son**'a tıklayın. AD FS Yönetim Konsolu'nu kapatın.

## <a name="trusted-ips-for-federated-users"></a>Federasyon kullanıcıları için Güvenilen IP'ler

Güvenilen IP'ler yöneticilerin, belirli IP adresleri ya da kendi intranetlerinden kaynaklanan taleplere sahip federasyon kullanıcıları için iki aşamalı doğrulamayı atlamasına izin verir. Aşağıdaki bölümlerde, federasyon kullanıcıları intranetinden gelen talepler için Azure Multi-Factor Authentication Güvenilen IP’lerinin federasyon kullanıcılarıyla yapılandırılması ve iki aşamalı doğrulamanın atlanması açıklanmıştır. Bu, AD FS’nin bir geçiş kullanacak ya da Kurumsal Ağ İçinde talep türü ile gelen bir talep şablonunu filtreleyecek şekilde yapılandırılmasıyla gerçekleştirilir.

Bu örnekte Güvenilen Taraf Güvenlerimiz için Office 365 kullanılmıştır.

### <a name="configure-the-ad-fs-claims-rules"></a>AD FS talep kurallarını yapılandırma

Yapmamız gereken ilk şey, AD FS taleplerini yapılandırmaktır. Biri Kurumsal Ağ İçinde talep türü için, diğeriyse kullanıcıların oturumunu açık şekilde tutmak için olmak üzere iki talep kuralı oluşturun.

1. AD FS Yönetimi'ni açın.
2. Solda, **Bağlı Olan Taraf Güvenleri**’ni seçin.
3. Right-click on **Microsoft Office 365 Identity Platform** and select **Edit Claim Rules…** 
   ![ADFS Console - Edit Claim Rules](./media/howto-mfa-adfs/trustedip1.png)
4. On Issuance Transform Rules, click **Add Rule.** 
   ![Adding a Claim Rule](./media/howto-mfa-adfs/trustedip2.png)
5. Dönüştürme Kuralı Ekleme Sihirbazı’nda, açılır menüde **Gelen Talep için Geçiş ya da Filtre**’yi seçin ve **İleri**’ye tıklayın.
   ![Dönüşüm Talep Kuralı Ekleme Sihirbazı](./media/howto-mfa-adfs/trustedip3.png)
6. Talep kuralı adının yanındaki kutuda kuralınıza bir ad verin. Örneğin: InsideCorpNet.
7. Gelen talep türü’nün yanındaki açılır menüde, **Kurumsal Ağ İçinde** seçeneğini belirleyin.
   ![Adding Inside Corporate Network claim](./media/howto-mfa-adfs/trustedip4.png)
8. **Son**'a tıklayın.
9. Verme Dönüştürme Kuralları’nda **Kural Ekle**’ye tıklayın.
10. Dönüştürme Kuralı Ekleme Sihirbazı’nda, açılır menüden **Talepleri Özel Bir Kural Kullanarak Gönder**’i seçin ve **İleri**’ye tıklayın.
11. Talep kuralı adı: altındaki kutuya *Kullanıcıların Oturumlarını Açık Tut* ifadesini girin.
12. Özel kural kutusuna şunu girin:

        c:[Type == "http://schemas.microsoft.com/2014/03/psso"]
            => issue(claim = c);
    ![Create custom claim to keep users signed in](./media/howto-mfa-adfs/trustedip5.png)
13. **Son**'a tıklayın.
14. **Apply (Uygula)** düğmesine tıklayın.
15. **Tamam**’a tıklayın.
16. AD FS Yönetimi'ni kapatın.

### <a name="configure-azure-multi-factor-authentication-trusted-ips-with-federated-users"></a>Azure Multi-Factor Authentication Güvenilen IP’leri Federasyon Kullanıcıları ile Yapılandırma

Talepler yapıldığına göre, artık güvenilen IP’leri yapılandırabiliriz.

1. [Azure Portal](https://portal.azure.com)’ında oturum açın.
2. Select **Azure Active Directory** > **Conditional Access** > **Named locations**.
3. From the **Conditional Access - Named locations** blade, select **Configure MFA trusted IPs**

   ![Azure AD Conditional Access named locations Configure MFA trusted IPs](./media/howto-mfa-adfs/trustedip6.png)

4. Hizmet Ayarları sayfasındaki **güvenilen IP'ler** altında bulunan **İntranetimde bulunan şirket dışındaki kullanıcıların istekleri için çok öğeli kimlik doğrulamayı atla** seçeneğini belirleyin.  
5. **Kaydet**’e tıklayın.

Bu kadar! Bu noktada, birleştirilmiş Office 365 kullanıcıları yalnızca talep kurumsal intranet dışından kaynaklandığı zaman MFA kullanmalıdır.
