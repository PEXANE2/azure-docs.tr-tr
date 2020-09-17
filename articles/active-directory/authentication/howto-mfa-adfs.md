---
title: Azure MFA ve ADFS ile kaynakları güvenli hale getirme-Azure Active Directory
description: Bu, bulutta nasıl Azure MFA ve AD FS kullanmaya başlayacağınızı açıklayan Azure Multi-Factor Authentication sayfasıdır.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: how-to
ms.date: 07/11/2018
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: bf57bea87fcb5e1d1f1bde4eada5a79d2fef52c8
ms.sourcegitcommit: 7374b41bb1469f2e3ef119ffaf735f03f5fad484
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/16/2020
ms.locfileid: "90706328"
---
# <a name="securing-cloud-resources-with-azure-multi-factor-authentication-and-ad-fs"></a>Azure Multi-Factor Authentication ve AD FS ile bulut kaynaklarını güvenli hale getirme

Kuruluşunuz Azure Active Directory ile birleştiriliyorsa, Azure AD’nin erişebildiği kaynakları güvenli hale getirmek için Azure Multi-Factor Authentication ya da Active Directory Federation Services (AD FS) kullanın. Azure Active Directory kaynaklarını Azure Multi-Factor Authentication ya da Active Directory Federasyon Hizmetleri ile güvenli hale getirmek için aşağıdaki yordamları kullanın.

## <a name="secure-azure-ad-resources-using-ad-fs"></a>AD FS kullanarak Azure AD kaynaklarını güvenli hale getirme

Bulut kaynağınızın güvenliğini sağlamak için, kullanıcı iki adımlı doğrulamayı başarıyla gerçekleştirdiğinde Active Directory Federation Services tarafından multipleauthn talebinin yayılması için bir talep kuralı oluşturun. Bu talep Azure AD'ye aktarılır. İlerlemek için bu yordamı izleyin:

1. AD FS Yönetimi'ni açın.
2. Solda, **Bağlı Olan Taraf Güvenleri**’ni seçin.
3. **Microsoft Office 365 kimlik platformu** ' na sağ tıklayın ve **talep kurallarını Düzenle**' yi seçin.

   ![ADFS konsolu-bağlı olan taraf güvenleri](./media/howto-mfa-adfs/trustedip1.png)

4. Verme dönüştürme kurallarında **Kural Ekle**' ye tıklayın.

   ![Verme dönüştürme kurallarını Düzenle](./media/howto-mfa-adfs/trustedip2.png)

5. Dönüştürme Kuralı Ekleme Sihirbazı’nda, açılır menüde **Gelen Talep için Geçiş ya da Filtre**’yi seçin ve **İleri**’ye tıklayın.

   ![Dönüşüm Talep Kuralı Ekleme Sihirbazı](./media/howto-mfa-adfs/trustedip3.png)

6. Kuralınıza bir ad verin. 
7. Gelen talep türü olarak **Kimlik Doğrulama Yöntemleri Başvuruları**’nı seçin.
8. **Tüm talep değerlerini geçir**’i seçin.
    ![Dönüşüm Talep Kuralı Ekleme Sihirbazı](./media/howto-mfa-adfs/configurewizard.png)
9. **Son**'a tıklayın. AD FS Yönetim Konsolu'nu kapatın.

## <a name="trusted-ips-for-federated-users"></a>Federasyon kullanıcıları için Güvenilen IP'ler

Güvenilen IP'ler yöneticilerin, belirli IP adresleri ya da kendi intranetlerinden kaynaklanan taleplere sahip federasyon kullanıcıları için iki aşamalı doğrulamayı atlamasına izin verir. Aşağıdaki bölümlerde, federasyon kullanıcıları intranetinden gelen talepler için Azure Multi-Factor Authentication Güvenilen IP’lerinin federasyon kullanıcılarıyla yapılandırılması ve iki aşamalı doğrulamanın atlanması açıklanmıştır. Bu, AD FS’nin bir geçiş kullanacak ya da Kurumsal Ağ İçinde talep türü ile gelen bir talep şablonunu filtreleyecek şekilde yapılandırılmasıyla gerçekleştirilir.

Bu örnek, bağlı olan taraf Güvenlerimiz için Microsoft 365 kullanır.

### <a name="configure-the-ad-fs-claims-rules"></a>AD FS talep kurallarını yapılandırma

Yapmamız gereken ilk şey, AD FS taleplerini yapılandırmaktır. Biri Kurumsal Ağ İçinde talep türü için, diğeriyse kullanıcıların oturumunu açık şekilde tutmak için olmak üzere iki talep kuralı oluşturun.

1. AD FS Yönetimi'ni açın.
2. Solda, **Bağlı Olan Taraf Güvenleri**’ni seçin.
3. **Microsoft Office 365 kimlik platformu** ' na sağ tıklayın ve **talep kurallarını Düzenle..** 
    ![ . seçeneğini belirleyin. ADFS konsolu-talep kurallarını Düzenle](./media/howto-mfa-adfs/trustedip1.png)
4. Verme dönüştürme kurallarında **Kural Ekle** ' ye tıklayın. 
    ![ Talep kuralı ekleme](./media/howto-mfa-adfs/trustedip2.png)
5. Dönüştürme Kuralı Ekleme Sihirbazı’nda, açılır menüde **Gelen Talep için Geçiş ya da Filtre**’yi seçin ve **İleri**’ye tıklayın.
   ![Dönüşüm Talep Kuralı Ekleme Sihirbazı](./media/howto-mfa-adfs/trustedip3.png)
6. Talep kuralı adının yanındaki kutuda kuralınıza bir ad verin. Örneğin: InsideCorpNet.
7. Gelen talep türü’nün yanındaki açılır menüde, **Kurumsal Ağ İçinde** seçeneğini belirleyin.
   ![Şirket ağı talebi Içine ekleniyor](./media/howto-mfa-adfs/trustedip4.png)
8. **Son**'a tıklayın.
9. Verme dönüştürme kurallarında **Kural Ekle**' ye tıklayın.
10. Dönüştürme Kuralı Ekleme Sihirbazı’nda, açılır menüden **Talepleri Özel Bir Kural Kullanarak Gönder**’i seçin ve **İleri**’ye tıklayın.
11. Talep kuralı adı: altındaki kutuya *Kullanıcıların Oturumlarını Açık Tut* ifadesini girin.
12. Özel kural kutusuna şunu girin:

```ad-fs-claim-rule
        c:[Type == "http://schemas.microsoft.com/2014/03/psso"]
            => issue(claim = c);
    ![Create custom claim to keep users signed in](./media/howto-mfa-adfs/trustedip5.png)
```

13. **Son**'a tıklayın.
14. **Uygula**’ya tıklayın.
15. **Tamam**’a tıklayın.
16. AD FS Yönetimi'ni kapatın.

### <a name="configure-azure-multi-factor-authentication-trusted-ips-with-federated-users"></a>Azure Multi-Factor Authentication Güvenilen IP’leri Federasyon Kullanıcıları ile Yapılandırma

Talepler yapıldığına göre, artık güvenilen IP’leri yapılandırabiliriz.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. **Azure Active Directory**  >  **güvenlik**  >  **koşullu erişimi**olarak  >  **adlandırılan konumları**seçin.
3. **Koşullu erişim-adlandırılmış konumlar** DIKEY penceresinde **MFA güvenilir IP 'lerini Yapılandır** ' ı seçin.

   ![Azure AD koşullu erişim adlandırılmış konumlar MFA güvenilir IP 'Leri yapılandırma](./media/howto-mfa-adfs/trustedip6.png)

4. Hizmet Ayarları sayfasındaki **güvenilen IP'ler** altında bulunan **İntranetimde bulunan şirket dışındaki kullanıcıların istekleri için çok öğeli kimlik doğrulamayı atla** seçeneğini belirleyin.  
5. **Kaydet**' e tıklayın.

İşte bu kadar! Bu noktada, Federal Microsoft 365 kullanıcıların yalnızca bir talebin şirket intraneti dışından geldiği durumlarda MFA kullanması gerekir.
