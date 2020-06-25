---
title: 'Azure AD Connect eşitleme: ADSync hizmet hesabını değiştirme | Microsoft Docs'
description: Bu konu belgesi, şifreleme anahtarını ve parola değiştirildikten sonra nasıl iptal edileceğini açıklar.
services: active-directory
keywords: Azure AD eşitleme hizmeti hesabı, parola
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 76b19162-8b16-4960-9e22-bd64e6675ecc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 05/02/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: cfb6e0608609a3940affd83a0aa235a2ef7c041c
ms.sourcegitcommit: f98ab5af0fa17a9bba575286c588af36ff075615
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2020
ms.locfileid: "85357571"
---
# <a name="changing-the-adsync-service-account-password"></a>ADSync hizmet hesabı parolasını değiştirme
ADSync hizmet hesabı parolasını değiştirirseniz, şifreleme anahtarını terk edinceye ve ADSync hizmet hesabı parolasını yeniden başlatana kadar eşitleme hizmeti düzgün şekilde başlayamaz. 

Azure AD Connect, Eşitleme Hizmetleri 'nin bir parçası olarak, AD DS bağlayıcı hesabının ve ADSync hizmet hesabının parolalarını depolamak için bir şifreleme anahtarı kullanır.  Bu hesaplar, veritabanında depolanmadan önce şifrelenir. 

Kullanılan şifreleme anahtarı [Windows veri koruması (DPAPI)](https://msdn.microsoft.com/library/ms995355.aspx)kullanılarak güvenli hale getirilir. DPAPI, **ADSync hizmet hesabını**kullanarak şifreleme anahtarını korur. 

Hizmet hesabı parolasını değiştirmeniz gerekiyorsa, bunu gerçekleştirmek için [ADSync hizmet hesabı şifreleme anahtarını bırakma](#abandoning-the-adsync-service-account-encryption-key) konusundaki yordamları kullanabilirsiniz.  Bu yordamlar Ayrıca, herhangi bir nedenden dolayı şifreleme anahtarını iptal etmeniz gerekiyorsa de kullanılmalıdır.

## <a name="issues-that-arise-from-changing-the-password"></a>Parolayı değiştirmekle kaynaklanan sorunlar
Hizmet hesabı parolasını değiştirdiğinizde gerçekleştirilmesi gereken iki şey vardır.

İlk olarak, Windows hizmet denetimi yöneticisi altındaki parolayı değiştirmeniz gerekir.  Bu sorun çözümlenene kadar aşağıdaki hatalar görüntülenir:


- Windows hizmet denetimi Yöneticisi 'nde eşitleme hizmetini başlatmaya çalışırsanız "**Windows, yerel bilgisayarda Microsoft Azure AD eşitleme hizmetini başlatamadı**" hatasını alıyorsunuz. **Hata 1069: hizmet bir oturum açma hatası nedeniyle başlatılmadı.**"
- Windows Olay Görüntüleyicisi altında sistem olay günlüğü, **olay kimliği 7038** ile bir hata ve "**ADSync hizmeti şu hata nedeniyle şu anda yapılandırılmış olan parolayla oturum açamadı: Kullanıcı adı veya parola yanlış.**" iletisini içerir.

İkincisi, belirli koşullar altında, parola güncelleştirilirse, eşitleme hizmeti artık DPAPI aracılığıyla şifreleme anahtarını alamaz. Şifreleme anahtarı olmadan, eşitleme hizmeti şirket içi AD ve Azure AD ile eşitlemek için gereken parolaların şifresini çözemez.
Şöyle bir hata görürsünüz:

- Windows hizmet denetimi Yöneticisi altında, eşitleme hizmetini başlatmaya çalışırsanız ve şifreleme anahtarını alamadıysanız, "<strong>windows Microsoft Azure AD eşitlemeyi yerel bilgisayarda başlatamadı hatasıyla başarısız oluyor. Daha fazla bilgi için sistem olay günlüğünü gözden geçirin. Bu Microsoft dışı bir hizmettir, hizmet satıcısına başvurun ve hizmete özgü hata kodu-21451857952 ' e başvurun</strong>.
- Windows Olay Görüntüleyicisi altında, uygulama olay günlüğü **olay kimliği 6028** ile bir hata ve *"sunucu şifreleme anahtarına erişilemiyor* " hata mesajını içerir.

Bu hataları almadığınızda emin olmak için, parolayı değiştirirken [ADSync hizmet hesabı şifreleme anahtarını bırakma](#abandoning-the-adsync-service-account-encryption-key) konusundaki yordamları izleyin.
 
## <a name="abandoning-the-adsync-service-account-encryption-key"></a>ADSync hizmet hesabı şifreleme anahtarını bırakma
>[!IMPORTANT]
>Aşağıdaki yordamlar yalnızca Azure AD Connect Build 1.1.443.0 veya üzeri için geçerlidir.

Şifreleme anahtarını bırakmak için aşağıdaki yordamları kullanın.

### <a name="what-to-do-if-you-need-to-abandon-the-encryption-key"></a>Şifreleme anahtarını iptal etmeniz gerekirse yapmanız gerekenler

Şifreleme anahtarını iptal etmeniz gerekiyorsa, bunu gerçekleştirmek için aşağıdaki yordamları kullanın.

1. [Eşitleme hizmetini durdur](#stop-the-synchronization-service)

1. [Mevcut şifreleme anahtarını bırakma](#abandon-the-existing-encryption-key)

2. [AD DS Bağlayıcısı hesabının parolasını girin](#provide-the-password-of-the-ad-ds-connector-account)

3. [ADSync hizmet hesabının parolasını yeniden başlatın](#reinitialize-the-password-of-the-adsync-service-account)

4. [Eşitleme hizmetini başlatın](#start-the-synchronization-service)

#### <a name="stop-the-synchronization-service"></a>Eşitleme hizmetini durdur
İlk olarak, Windows hizmeti Denetim Yöneticisi 'nde hizmeti durdurabilirsiniz.  Hizmetin durdurulmaya çalışılırken çalışmadığını denetleyin.  Varsa, tamamlanana kadar bekleyip uygulamayı durdurun.


1. Windows hizmet denetimi Yöneticisi 'ne gidin (→ Services 'ı başlatın).
2. **Microsoft Azure AD eşitleme** ' yi seçin ve Durdur ' a tıklayın.

#### <a name="abandon-the-existing-encryption-key"></a>Mevcut şifreleme anahtarını bırakma
Yeni şifreleme anahtarının oluşturulabilmesi için mevcut şifreleme anahtarını bırakın:

1. Azure AD Connect sunucunuzda yönetici olarak oturum açın.

2. Yeni bir PowerShell oturumu başlatın.

3. Klasöre git:`'$env:ProgramFiles\Microsoft Azure AD Sync\bin\'`

4. Şu komutu çalıştırın:`./miiskmu.exe /a`

![Azure AD Connect eşitleme şifreleme anahtarı yardımcı programı](./media/how-to-connect-sync-change-serviceacct-pass/key5.png)

#### <a name="provide-the-password-of-the-ad-ds-connector-account"></a>AD DS Bağlayıcısı hesabının parolasını girin
Veritabanında depolanan mevcut parolaların artık şifresi çözülemediğinden, AD DS bağlayıcı hesabının parolasıyla eşitleme hizmetini sağlamanız gerekir. Eşitleme hizmeti, yeni şifreleme anahtarını kullanarak parolaları şifreler:

1. Synchronization Service Manager başlatın (→ eşitleme hizmetini başlatın).
</br>![Eşitleme Service Manager](./media/how-to-connect-sync-change-serviceacct-pass/startmenu.png)  
2. **Bağlayıcılar** sekmesine gidin.
3. Şirket içi AD 'nize karşılık gelen **ad bağlayıcısını** seçin. Birden fazla AD bağlayıcınız varsa, her biri için aşağıdaki adımları yineleyin.
4. **Eylemler**altında **Özellikler**' i seçin.
5. Açılır iletişim kutusunda **Active Directory ormana Bağlan**' ı seçin:
6. **Parola** metin kutusuna AD DS hesabının parolasını girin. Parolasını bilmediğinizde, bu adımı gerçekleştirmeden önce bilinen bir değere ayarlamanız gerekir.
7. Yeni parolayı kaydetmek ve açılır iletişim kutusunu kapatmak için **Tamam** ' ı tıklatın.
![Azure AD Connect eşitleme şifreleme anahtarı yardımcı programı](./media/how-to-connect-sync-change-serviceacct-pass/key6.png)

#### <a name="reinitialize-the-password-of-the-adsync-service-account"></a>ADSync hizmet hesabının parolasını yeniden başlatın
Azure AD hizmet hesabının parolasını doğrudan eşitleme hizmetine sağlayamezsiniz. Bunun yerine, Azure AD hizmet hesabını yeniden başlatmak için **Add-ADSyncAADServiceAccount** cmdlet 'ini kullanmanız gerekir. Cmdlet, hesap parolasını sıfırlar ve eşitleme hizmeti tarafından kullanılabilir hale getirir:

1. Azure AD Connect sunucuda yeni bir PowerShell oturumu başlatın.
2. Cmdlet 'ini çalıştırın `Add-ADSyncAADServiceAccount` .
3. Açılır iletişim kutusunda Azure AD kiracınız için Azure AD Genel yönetici kimlik bilgilerini sağlayın.
![Azure AD Connect eşitleme şifreleme anahtarı yardımcı programı](./media/how-to-connect-sync-change-serviceacct-pass/key7.png)
4. Bu işlem başarılı olursa, PowerShell komut istemi ' ni görürsünüz.

#### <a name="start-the-synchronization-service"></a>Eşitleme hizmetini başlatın
Eşitleme hizmetinin şifreleme anahtarına ve gereken tüm parolalara erişimi olduğuna göre, Windows hizmeti Denetim Yöneticisi 'nde hizmeti yeniden başlatabilirsiniz:


1. Windows hizmet denetimi Yöneticisi 'ne gidin (→ Services 'ı başlatın).
2. **Microsoft Azure AD Eşitle** öğesini seçin ve yeniden Başlat ' a tıklayın.

## <a name="next-steps"></a>Sonraki adımlar
**Genel Bakış konuları**

* [Azure AD Connect eşitleme: eşitlemeyi anlama ve özelleştirme](how-to-connect-sync-whatis.md)

* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md)
