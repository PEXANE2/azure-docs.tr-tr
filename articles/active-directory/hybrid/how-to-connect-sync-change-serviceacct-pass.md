---
title: 'Azure AD Connect eşitlemi: ADSync hizmet hesabını değiştirme | Microsoft Dokümanlar'
description: Bu konu belgesi, şifreleme anahtarını ve parola değiştirildikten sonra nasıl terk edilebildiğini açıklar.
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
ms.topic: conceptual
ms.date: 05/02/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9b2a0d0b77b6db481b13785907a1359d2bbe3e9b
ms.sourcegitcommit: 7d8158fcdcc25107dfda98a355bf4ee6343c0f5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2020
ms.locfileid: "80984513"
---
# <a name="changing-the-adsync-service-account-password"></a>ADSync servis hesabı parolasını değiştirme
ADSync hizmet hesabı parolasını değiştirirseniz, şifreleme anahtarını bırakıp ADSync hizmet hesabı parolasını yeniden başlatana kadar Eşitleme Hizmeti doğru şekilde başlatılamaz. 

Azure AD Connect, Eşitleme Hizmetlerinin bir parçası olarak AD DS Bağlayıcı hesabının ve ADSync hizmet hesabının parolalarını depolamak için bir şifreleme anahtarı kullanır.  Bu hesaplar veritabanında depolandırılmadan önce şifrelenir. 

Kullanılan şifreleme anahtarı Windows [Veri Koruması (DPAPI)](https://msdn.microsoft.com/library/ms995355.aspx)kullanılarak korunur. DPAPI, **ADSync hizmet hesabını**kullanarak şifreleme anahtarını korur. 

Servis hesabı parolasını değiştirmeniz gerekiyorsa, bunu gerçekleştirmek için [ADSync hizmet hesabı şifreleme anahtarını terk etme](#abandoning-the-adsync-service-account-encryption-key) yordamlarını kullanabilirsiniz.  Şifreleme anahtarını herhangi bir nedenle bırakmanız gerekiyorsa, bu yordamlar da kullanılmalıdır.

## <a name="issues-that-arise-from-changing-the-password"></a>Parolanın değiştirilmesinden kaynaklanan sorunlar
Hizmet hesabı parolasını değiştirdiğinizde yapılması gereken iki şey vardır.

Öncelikle, Windows Service Control Manager altında parolayı değiştirmeniz gerekir.  Bu sorun çözülene kadar aşağıdaki hataları görürsünüz:


- Windows Hizmet Denetim Yöneticisi'nde Eşitleme Hizmetini başlatmaya çalışırsanız, "**Windows, Yerel Bilgisayar'da Microsoft Azure AD Eşitleme hizmetini başlatamadı**" hatasını alırsınız. **Hata 1069: Hizmet bir oturum açma hatası nedeniyle başlamadı.**"
- Windows Olay Görüntüleyicisi altında, sistem olay günlüğü **Olay Kimliği 7038** ile ilgili bir hata içerir ve **"ADSync hizmeti aşağıdaki hata nedeniyle şu anda yapılandırılan parolada olduğu gibi oturum açamadı: Kullanıcı adı veya parola yanlıştır.**"

İkinci olarak, belirli koşullar altında, parola güncelleştirilirse, Eşitleme Hizmeti artık DPAPI üzerinden şifreleme anahtarını alamaz. Şifreleme anahtarı olmadan, Eşitleme Hizmeti şirket içi AD ve Azure AD'ye/şirket içi AD'ye eşitlemek için gereken parolaların şifresini çözemez.
Şu gibi hatalar görürsünüz:

- Windows Service Control Manager altında, Eşitleme Hizmetini başlatmaya çalışırsanız ve şifreleme anahtarını alamıyorsa, hatayla başarısız olur "<strong>Windows, Microsoft Azure AD Eşitlemeyi'ni Yerel Bilgisayarda başlatamaz. Daha fazla bilgi için Sistem Olayı günlüğünü inceleyin. Bu Microsoft'a ait olmayan bir hizmetse, hizmet satıcısına başvurun ve hizmete özgü hata kodu -21451857952'ye başvurun."</strong>
- Windows Olay Görüntüleyicisi altında, uygulama olay günlüğü **Olay Kimliği 6028** ve hata iletisi *"Sunucu şifreleme anahtarına erişilemiyor"* hatası içerir.

Bu hataları almadığınızı sağlamak için, parolayı değiştirirken [ADSync hizmet hesabı şifreleme anahtarını terk etme](#abandoning-the-adsync-service-account-encryption-key) yordamlarını izleyin.
 
## <a name="abandoning-the-adsync-service-account-encryption-key"></a>ADSync hizmet hesabı şifreleme anahtarını terk etme
>[!IMPORTANT]
>Aşağıdaki yordamlar yalnızca Azure AD Connect build 1.1.443.0 veya daha büyük için geçerlidir.

Şifreleme anahtarını terk etmek için aşağıdaki yordamları kullanın.

### <a name="what-to-do-if-you-need-to-abandon-the-encryption-key"></a>Şifreleme anahtarını bırakmanız gerekiyorsa ne yapmalısınız?

Şifreleme anahtarını bırakmanız gerekiyorsa, bunu gerçekleştirmek için aşağıdaki yordamları kullanın.

1. [Eşitleme Hizmetini Durdur](#stop-the-synchronization-service)

1. [Varolan şifreleme anahtarını terk etme](#abandon-the-existing-encryption-key)

2. [AD DS Bağlayıcıhesabının parolasını sağlama](#provide-the-password-of-the-ad-ds-connector-account)

3. [ADSync hizmet hesabının parolasını yeniden başlatma](#reinitialize-the-password-of-the-adsync-service-account)

4. [Eşitleme Hizmetini Başlat](#start-the-synchronization-service)

#### <a name="stop-the-synchronization-service"></a>Eşitleme Hizmetini Durdur
Önce Windows Service Control Manager'da hizmeti durdurabilirsiniz.  Hizmeti durdurmaya çalışırken çalışmıyorolduğundan emin olun.  Eğer öyleyse, tamamlanana kadar bekleyin ve sonra durdurun.


1. Windows Service Control Manager'a gidin (START → Hizmetler).
2. **Microsoft Azure AD Eşitlemesini** seçin ve Durdur'u tıklatın.

#### <a name="abandon-the-existing-encryption-key"></a>Varolan şifreleme anahtarını terk etme
Yeni şifreleme anahtarının oluşturulabilmesi için varolan şifreleme anahtarından vazgeçin:

1. Azure AD Connect Server'ınızda yönetici olarak oturum açın.

2. Yeni bir PowerShell oturumu başlatın.

3. Klasöre gidin:`'$env:ProgramFiles\Microsoft Azure AD Sync\bin\'`

4. Komutu çalıştırın:`./miiskmu.exe /a`

![Azure AD Connect Sync Şifreleme Anahtarı Yardımcı Programı](./media/how-to-connect-sync-change-serviceacct-pass/key5.png)

#### <a name="provide-the-password-of-the-ad-ds-connector-account"></a>AD DS Bağlayıcıhesabının parolasını sağlama
Veritabanında depolanan varolan parolaların şifresi artık çözülemeyeceğinden, Eşitleme Hizmeti'ni AD DS Bağlayıcı hesabının parolasıyla sağlamanız gerekir. Eşitleme Hizmeti, parolaları yeni şifreleme anahtarını kullanarak şifreler:

1. Eşitleme Hizmet Yöneticisi'ni başlatın (START → Eşitleme Hizmeti).
</br>![Eşitleme Servis Yöneticisi](./media/how-to-connect-sync-change-serviceacct-pass/startmenu.png)  
2. **Bağlayıcılar** sekmesine gidin.
3. Şirket içi AD'nize karşılık gelen **AD Bağlayıcısı'nı** seçin. Birden fazla AD bağlayıcınız varsa, her biri için aşağıdaki adımları yineleyin.
4. **Eylemler**altında, **Özellikleri**seçin.
5. Açılan iletişim kutusunda Etkin **Dizin Ormanı'na Bağlan'ı**seçin:
6. **Parola** metin kutusuna AD DS hesabının parolasını girin. Parolasını bilmiyorsanız, bu adımı gerçekleştirmeden önce parolayı bilinen bir değere ayarlamanız gerekir.
7. Yeni parolayı kaydetmek ve açılır pencere iletişim kutusunu kapatmak için **Tamam'ı** tıklatın.
![Azure AD Connect Sync Şifreleme Anahtarı Yardımcı Programı](./media/how-to-connect-sync-change-serviceacct-pass/key6.png)

#### <a name="reinitialize-the-password-of-the-adsync-service-account"></a>ADSync hizmet hesabının parolasını yeniden başlatma
Azure AD hizmet hesabının parolasını doğrudan Eşitleme Hizmeti'ne sağlayamazsınız. Bunun yerine, Azure AD hizmet hesabını yeniden başlatmanız için cmdlet **Add-ADSyncAADServiceAccount'i** kullanmanız gerekir. Cmdlet hesap parolasını sıfırlar ve Eşitleme Hizmeti'ne verir:

1. Azure AD Connect sunucusunda yeni bir PowerShell oturumu başlatın.
2. Cmdlet `Add-ADSyncAADServiceAccount`çalıştırın.
3. Açılır iletişim kutusunda, Azure AD kiracınız için Azure AD Global yönetici kimlik bilgilerini sağlayın.
![Azure AD Connect Sync Şifreleme Anahtarı Yardımcı Programı](./media/how-to-connect-sync-change-serviceacct-pass/key7.png)
4. Başarılı olursa, PowerShell komut istemini görürsünüz.

#### <a name="start-the-synchronization-service"></a>Eşitleme Hizmetini Başlat
Eşitleme Hizmeti şifreleme anahtarına ve ihtiyacı olan tüm parolalara erişebildiği için, Hizmeti Windows Service Control Manager'da yeniden başlatabilirsiniz:


1. Windows Service Control Manager'a gidin (START → Hizmetler).
2. **Microsoft Azure AD Eşitlemeyi'ni** seçin ve Yeniden Başlat'ı tıklatın.

## <a name="next-steps"></a>Sonraki adımlar
**Genel bakış konuları**

* [Azure AD Connect eşitlemesi: Eşitlemeyi anlama ve özelleştirme](how-to-connect-sync-whatis.md)

* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md)
