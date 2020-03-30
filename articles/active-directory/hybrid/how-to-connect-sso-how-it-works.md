---
title: 'Azure AD Connect: Sorunsuz Tek Oturum Açma - Nasıl Çalışır | Microsoft Dokümanlar'
description: Bu makalede, Azure Etkin Dizin Sorunsuz Tek Oturum Açma özelliğinin nasıl çalıştığı açıklanmaktadır.
services: active-directory
keywords: Azure AD Connect nedir, Active Directory'yi yükler, Azure AD, SSO, Tek Oturum Açma için gerekli bileşenler
documentationcenter: ''
author: billmath
manager: daveba
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/16/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: bd4743bc38c3b2b4b9495b33535b4b73f48d1372
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71176679"
---
# <a name="azure-active-directory-seamless-single-sign-on-technical-deep-dive"></a>Azure Active Directory Seamless Tek İşaret- On: Teknik derin dalış

Bu makalede, Azure Active Directory Seamless Tek Oturum Açma (Sorunsuz SSO) özelliğinin nasıl çalıştığına ilişkin teknik ayrıntılar verebilirsiniz.

## <a name="how-does-seamless-sso-work"></a>Sorunsuz SSO nasıl çalışır?

Bu bölümde üç bölüm vardır:

1. Kesintisiz SSO özelliğinin kurulumu.
2. Bir web tarayıcısında tek bir kullanıcı oturum açma işlemi Sorunsuz SSO ile nasıl çalışır.
3. Yerel istemcide tek bir kullanıcı oturum açma işleminin Sorunsuz SSO ile nasıl çalıştığı.

### <a name="how-does-set-up-work"></a>Kurulumu nasıl çalışır?

[Burada](how-to-connect-sso-quick-start.md)gösterildiği gibi Azure AD Connect kullanılarak sorunsuz SSO etkinleştirilir. Özelliği etkinleştirirken aşağıdaki adımlar oluşur:

- Azure AD'ye senkronize ettiğiniz her AD ormanındaki şirket içi Active Directory 'nizde (AD) bir bilgisayar hesabı`AZUREADSSOACC`oluşturulur (Azure AD Connect'i kullanarak).
- Ayrıca, Azure AD oturum açma işlemi sırasında kullanılmak üzere bir dizi Kerberos hizmet temel adı (SPN) oluşturulur.
- Bilgisayar hesabının Kerberos şifre çözme anahtarı Azure AD ile güvenli bir şekilde paylaşılır. Birden çok AD ormanı varsa, her bilgisayar hesabının kendine özgü Kerberos şifre çözme anahtarı olacaktır.

>[!IMPORTANT]
> Bilgisayar `AZUREADSSOACC` hesabının güvenlik nedenleriyle güçlü bir şekilde korunması gerekir. Bilgisayar hesabını yalnızca Etki Alanı Yöneticileri yönetebilir. Bilgisayar hesabındaki Kerberos delegasyonunun devre dışı bırakıldığından ve Active Directory'deki başka hiçbir hesabın `AZUREADSSOACC` bilgisayar hesabında delegasyon izinleri olmadığından emin olun... Bilgisayar hesabını, yanlışlıkla silmelere karşı güvende oldukları ve yalnızca Etki Alanı Yöneticilerinin erişebildiği bir Kuruluş Birimi'nde (OU) depolayın. Bilgisayar hesabındaki Kerberos şifre çözme anahtarı da hassas olarak ele alınmalıdır. Bilgisayar [hesabının Kerberos şifre çözme anahtarını](how-to-connect-sso-faq.md) en az 30 günde bir teslim almanızı şiddetle öneririz. `AZUREADSSOACC`

Kurulum tamamlandıktan sonra, Seamless SSO, Tümleşik Windows Kimlik Doğrulaması (IWA) kullanan diğer oturum açma ile aynı şekilde çalışır.

### <a name="how-does-sign-in-on-a-web-browser-with-seamless-sso-work"></a>Sorunsuz SSO ile bir web tarayıcısında oturum açma nasıl çalışır?

Bir web tarayıcısında oturum açma akışı aşağıdaki gibidir:

1. Kullanıcı, şirket ağınızdaki etki alanı birleştirilmiş kurumsal https://outlook.office365.com/owa/) cihazdan bir web uygulamasına (örneğin, Outlook Web Uygulaması- erişmeye çalışır.
2. Kullanıcı zaten oturum açmamışsa, kullanıcı Azure AD oturum açma sayfasına yönlendirilir.
3. Kullanıcı adını Azure AD oturum açma sayfasına adlarını tipler.

   >[!NOTE]
   >[Bazı uygulamalar](./how-to-connect-sso-faq.md)için, 2 & 3 adım atlanır.

4. Arka planda JavaScript'i kullanan Azure AD, 401 Yetkisiz yanıt yoluyla tarayıcıya Kerberos bileti sağlamak için meydan okuyor.
5. Tarayıcı da bilgisayar hesabı için Active Directory'den bir bilet ister (Azure AD'yi `AZUREADSSOACC` temsil eder).
6. Active Directory bilgisayar hesabını bulur ve bilgisayar hesabının sırrıyla şifrelenmiş tarayıcıya bir Kerberos bileti döndürür.
7. Tarayıcı, Active Directory'den satın aldığı Kerberos biletini Azure AD'ye iletir.
8. Azure AD, daha önce paylaşılan anahtarı kullanarak kurumsal aygıtta oturum açan kullanıcının kimliğini içeren Kerberos biletinin şifresini çözer.
9. Değerlendirmeden sonra Azure AD, bir belirteçle uygulamayı geri döndürür veya kullanıcıdan Çok Faktörlü Kimlik Doğrulama gibi ek provalar gerçekleştirmesini ister.
10. Kullanıcı oturum açma başarılı olursa, kullanıcı uygulamaya erişebilir.

Aşağıdaki diyagram, ilgili tüm bileşenleri ve adımları gösterir.

![Sorunsuz Tek Oturum Açma - Web uygulaması akışı](./media/how-to-connect-sso-how-it-works/sso2.png)

Sorunsuz SSO fırsatçıdır, yani başarısız olursa, oturum açma deneyimi normal davranışına geri döner - yani kullanıcının oturum açabilmesi için parolasını girmesi gerekir.

### <a name="how-does-sign-in-on-a-native-client-with-seamless-sso-work"></a>Sorunsuz SSO'lu yerel bir istemcide oturum açma nasıl çalışır?

Yerel istemcide oturum açma akışı aşağıdaki gibidir:

1. Kullanıcı, şirket ağınızdaki etki alanına katılan bir şirket aygıtından yerel bir uygulamaya (örneğin, Outlook istemcisi) erişmeye çalışır.
2. Kullanıcı zaten oturum açmış değilse, yerel uygulama kullanıcının kullanıcı adını aygıtın Windows oturumundan alır.
3. Uygulama kullanıcı adını Azure AD'ye gönderir ve kiracınızın WS-Trust MEX bitiş noktasını alır. Bu WS-Trust bitiş noktası yalnızca Sorunsuz SSO özelliği tarafından kullanılır ve Azure AD'deki WS-Trust protokolünün genel bir uygulaması değildir.
4. Uygulama daha sonra tümleşik kimlik doğrulama bitiş noktası nın kullanılabilir olup olmadığını görmek için WS-Trust MEX bitiş noktasını sorgular. Tümleşik kimlik doğrulama bitiş noktası yalnızca Dikişsiz SSO özelliği tarafından kullanılır.
5. Adım 4 başarılı olursa, bir Kerberos meydan verilir.
6. Uygulama Kerberos biletini alabiliyorsa, bu bileti Azure AD'nin tümleşik kimlik doğrulama bitiş noktasına iletir.
7. Azure AD, Kerberos biletinin şifresini çözer ve doğrular.
8. Azure AD kullanıcıyı imzalar ve uygulamaya bir SAML belirteci yayınlar.
9. Uygulama daha sonra SAML belirteci'ni Azure AD'nin OAuth2 belirteci bitiş noktasına gönderir.
10. Azure AD, SAML belirteci ve uygulamanın erişimini ve belirtilen kaynak için yenibir belirteci ve kimlik belirteci sorunlarını doğrular.
11. Kullanıcı uygulamanın kaynağına erişebilir.

Aşağıdaki diyagram, ilgili tüm bileşenleri ve adımları gösterir.

![Sorunsuz Tek İşaret Açma - Yerel uygulama akışı](./media/how-to-connect-sso-how-it-works/sso14.png)

## <a name="next-steps"></a>Sonraki adımlar

- [**Hızlı Başlangıç**](how-to-connect-sso-quick-start.md) - Azure REKLAM Sorunsuz SSO'ya başlayın ve çalıştırın.
- [**Sık Sorulan Sorular**](how-to-connect-sso-faq.md) - Sık sorulan soruların yanıtları.
- [**Sorun Giderme**](tshoot-connect-sso.md) - Özellik ile ilgili sık karşılaşılan sorunları nasıl çözeceğinizi öğrenin.
- [**UserVoice**](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - Yeni özellik istekleri dosyalama için.
