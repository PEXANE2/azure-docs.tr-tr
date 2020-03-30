---
title: 'Azure AD Connect: Karma Azure REKLAMı yapılandırma sonrası görevlere katılır | Microsoft Dokümanlar'
description: Bu belge, Karma Azure AD birleştirmesini tamamlamak için gereken yapılandırma görevlerini nakletme ayrıntılarını
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: billmath
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/10/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: ffb8243041bb93ba8be6a65bb83df6f84affaee3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80049658"
---
# <a name="post-configuration-tasks-for-hybrid-azure-ad-join"></a>Hibrit Azure AD’ye katılmada yapılandırma sonrası görevler

Kuruluşunuzkarma Azure AD join için yapılandırmak için Azure AD Connect'i çalıştırdıktan sonra, bu kurulumu tamamlamak için tamamlamanız gereken birkaç ek adım vardır.  Yalnızca aygıtlarınız için geçerli olan adımları uygulayın.

## <a name="1-configure-controlled-rollout-optional"></a>1. Kontrollü kullanıma yapılandırın (İsteğe bağlı)
Tüm yapılandırma adımları tamamlandıktan sonra Windows 10 ve Windows Server 2016 çalıştıran tüm etki alanı birleştirilmiş aygıtlar otomatik olarak Azure AD'ye kaydolur. Bu otomatik kayıt yerine kontrollü bir kullanıma sunulmasını tercih ederseniz, otomatik kullanıma sunulmasını seçicisi olarak etkinleştirmek veya devre dışı katmak için grup ilkesini kullanabilirsiniz.  Bu grup ilkesi, diğer yapılandırma adımlarını başlatmadan önce ayarlanmalıdır:
* Etkin Dizininizde bir grup ilkesi nesnesi oluşturun.
* Ad it (eski- Karma Azure AD join).
* Düzenleme ve gitme: Bilgisayar Yapılandırma > İlkeleri > Windows Bileşenleri > Aygıt Kaydı > Yönetim Şablonları.

>[!NOTE]
>2012R2 için ilke ayarları **Bilgisayar Yapılandırma > İlkeleri > Yönetim Şablonları > Windows Bileşenleri > İşyeri Otomatik olarak işyeri birleştirme > istemci bilgisayarlara katılma**

* Bu ayarı etkinleştirin: Etki alanına birleştirilmiş bilgisayarları aygıt olarak kaydedin.
* Uygula ve Tamam'ı tıklatın.
* GPO'yu seçtiğiniz konuma (kuruluş birimi, güvenlik grubu veya tüm aygıtlar için etki alanına) bağlayın.

## <a name="2-configure-network-with-device-registration-endpoints"></a>2. Cihazı cihaz kayıt bitiş noktalarıyla yapılandırma
Azure AD'ye kaydolmak için kuruluş ağınızdaki bilgisayarlardan aşağıdaki URL'lere erişilebildiğinden emin olun:

* `https://enterpriseregistration.windows.net`
* `https://login.microsoftonline.com`
* `https://device.login.microsoftonline.com` 

## <a name="3-implement-wpad-for-windows-10-devices"></a>3. Windows 10 cihazları için WPAD uygulayın
Kuruluşunuz giden bir proxy aracılığıyla Internet'e eriştiyse, Windows 10 bilgisayarlarının Azure AD'ye kaydolmasını sağlamak için Web Proxy Otomatik Bulma (WPAD) uygulayın.

## <a name="4-configure-the-scp-in-any-forests-that-were-not-configured-by-azure-ad-connect"></a>4. Azure AD Connect tarafından yapılandırılmamış ormanlarda SCP'yi yapılandırın 

Hizmet bağlantı noktası (SCP), otomatik kayıt için aygıtlarınız tarafından kullanılacak Azure AD kiracı bilgilerinizi içerir.  Azure AD Connect'ten indirdiğiniz PowerShell komut dosyasını, ConfigureSCP.ps1'i çalıştırın.

## <a name="5-configure-any-federation-service-that-was-not-configured-by-azure-ad-connect"></a>5. Azure AD Connect tarafından yapılandırılmamış herhangi bir federasyon hizmetini yapılandırma

Kuruluşunuz Azure AD'de oturum açtıracak bir federasyon hizmeti kullanıyorsa, Azure AD güvenerek taraf güveninizdeki talep kuralları aygıt kimlik doğrulaması için izin vermelidir. AD FS ile federasyon kullanıyorsanız, talep kurallarını oluşturmak için [AD FS Yardım'a](https://aka.ms/aadrptclaimrules) gidin. Microsoft federasyonu olmayan bir çözüm kullanıyorsanız, rehberlik için bu sağlayıcıya başvurun.  

>[!NOTE]
>Windows alt düzey aygıtlarınız varsa, hizmetin Azure AD güvenine istek alırken kimlik doğrulama yönteminin ve wiaormultiauthn taleplerinin verilmesine destek vermesi gerekir. AD FS'de, kimlik doğrulama yönteminden geçen bir verme dönüştürme kuralı eklemeniz gerekir.

## <a name="6-enable-azure-ad-seamless-sso-for-windows-down-level-devices"></a>6. Windows alt düzey cihazlar için Azure REKLAM Sorunsuz SSO'ya olanak sağlama

Kuruluşunuz Azure AD'de oturum açabilmek için Parola Karma Eşitleme veya Geçiş Kimlik Doğrulaması kullanıyorsa, Windows'un alt düzey https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-ssoaygıtlarının kimliğini doğrulamak için bu oturum açma yöntemiyle Azure AD Sorunsuz SSO'ya olanak sağlayın: . 

## <a name="7-set-azure-ad-policy-for-windows-down-level-devices"></a>7. Windows alt düzey aygıtlar için Azure REKLAM ilkesini ayarlama

Windows'un alt düzey aygıtlarını kaydetmek için Azure REKLAM ilkesinin kullanıcıların aygıtları kaydetmesine izin verdiğinden emin olmanız gerekir. 

* Azure portalında hesabınızda oturum açın.
* Şuna git: Azure Active Directory > Cihazlar > Aygıt ayarları
* "Kullanıcılar cihazlarını Azure AD ile kaydedebilir" olarak ayarlayın.
* Kaydet’e tıklayın.

## <a name="8-add-azure-ad-endpoint-to-windows-down-level-devices"></a>8. Windows alt düzey aygıtlara Azure AD bitiş noktası ekleme

Aygıtların kimlik doğruluğunu yaparken sertifika istemlerinden kaçınmak için Windows alt düzey aygıtlarınızdaki yerel Intranet bölgelerine Azure AD aygıtı kimlik doğrulaması bitiş noktasını ekleyin:`https://device.login.microsoftonline.com` 

[Sorunsuz SSO](how-to-connect-sso.md)kullanıyorsanız, o bölgede "Komut dosyası üzerinden durum çubuğu güncelleştirmelerine izin ver"i etkinleştirin ve aşağıdaki bitiş noktasını ekleyin:`https://autologon.microsoftazuread-sso.com` 

## <a name="9-install-microsoft-workplace-join-on-windows-down-level-devices"></a>9. Windows alt düzey cihazlara Microsoft İşyeri Birleştirme'yi yükleme

Bu yükleyici, kullanıcıbağlamında çalışan aygıt sisteminde zamanlanmış bir görev oluşturur. Kullanıcı Windows'da kaydolduğunda görev tetiklenir. Görev, Tümleşik Windows Kimlik Doğrulaması'nı kullanarak kimlik doğrulaması yaptıktan sonra azure AD ile aygıtı sessizce kullanıcı kimlik bilgileriyle birleştirir. İndirme merkezi https://www.microsoft.com/download/details.aspx?id=53554. 

## <a name="10-configure-group-policy-to-allow-device-registration"></a>10. Aygıt kaydına izin verecek şekilde grup ilkesini yapılandırın

* Active Directory'nizde bir grup ilkesi nesnesi oluşturun- zaten oluşturulmamadıysa.
* Ad it (eski- Karma Azure AD join).
* & düzenleme: Bilgisayar Yapılandırma > İlkeleri > Windows Bileşenleri > Aygıt Kaydı > Yönetim Şablonları
* Etkinleştir: Etki alanına birleştirilmiş bilgisayarları aygıt olarak kaydetme
* Uygula ve Tamam'ı tıklatın.
* GPO'yu seçtiğiniz konuma (kuruluş birimi, güvenlik grubu veya tüm aygıtlar için etki alanına) bağlayın.

>[!NOTE]
>2012R2 için ilke ayarları **Bilgisayar Yapılandırma > İlkeleri > Yönetim Şablonları > Windows Bileşenleri > İşyeri Otomatik olarak işyeri birleştirme > istemci bilgisayarlara katılma**

## <a name="next-steps"></a>Sonraki adımlar
[Aygıt yazmayı yapılandırma](how-to-connect-device-writeback.md)
