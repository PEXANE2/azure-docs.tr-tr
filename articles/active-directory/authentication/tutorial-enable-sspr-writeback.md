---
title: Azure Active Directory parola sını yeniden yazmayı etkinleştirme
description: Bu eğitimde, değişiklikleri şirket içi Active Directory Etki Alanı Hizmetleri ortamına senkronize etmek için Azure AD Connect'i kullanarak Azure AD self servis parola sıfırlama yı etkinleştirmeyi öğrenirsiniz.
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: tutorial
ms.date: 02/18/2020
ms.author: iainfou
author: iainfoulds
ms.reviewer: rhicock
ms.collection: M365-identity-device-management
ms.openlocfilehash: f3578cb1326ebd701c3f00618c19a501a1476372
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80332134"
---
# <a name="tutorial-enable-azure-active-directory-self-service-password-reset-writeback-to-an-on-premises-environment"></a>Öğretici: Azure Active Directory self servis parola sıfırlama yı şirket içi ortama geri ayarlamayı etkinleştirme

Azure Active Directory (Azure AD) self servis parola sıfırlama (SSPR) ile kullanıcılar parolalarını güncelleyebilir veya bir web tarayıcısı kullanarak hesaplarının kilidini açabilir. Azure AD'nin şirket içi Active Directory Domain Services (AD DS) ortamına bağlı olduğu karma bir ortamda, bu senaryo parolaların iki dizin arasında farklı olmasını sağlayabilir.

Parola geri ödeme, Azure AD'deki parola değişikliklerini şirket içi AD DS ortamınıza senkronize etmek için kullanılabilir. Azure AD Connect, bu parola değişikliklerini Azure AD'den varolan bir şirket içi dizine geri göndermek için güvenli bir mekanizma sağlar.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Parola yazma için gerekli izinleri yapılandırma
> * Azure AD Connect'te parola yazma seçeneğini etkinleştirme
> * Azure AD SSPR'de parola yazmayı etkinleştirme

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdaki kaynaklara ve ayrıcalıklara ihtiyacınız vardır:

* En az deneme sürümü lisansı etkinleştirilmiş çalışan bir Azure AD kiracısına erişim.
    * Gerekirse, [ücretsiz bir oluşturun.](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
    * Daha fazla bilgi için [Azure AD SSPR için Lisanslama gereksinimlerine](concept-sspr-licensing.md)bakın.
* *Genel yönetici* ayrıcalıklarına sahip bir hesap.
* Azure AD self servis parola sıfırlama için yapılandırıldı.
    * Gerekirse, [Azure AD SSPR'ı etkinleştirmek için önceki öğreticiyi tamamlayın.](tutorial-enable-sspr.md)
* Azure AD Connect'in geçerli sürümüyle yapılandırılan mevcut şirket içi AD DS ortamı.
    * Gerekirse, [Express](../hybrid/how-to-connect-install-express.md) veya [Custom](../hybrid/how-to-connect-install-custom.md) ayarlarını kullanarak Azure AD Connect'i yapılandırın.
    * Parola Yazma Geri Kullanma'yı kullanmak için Etki Alanı Denetleyicilerinizwindows Server 2008 R2 veya sonraki olmalıdır.

## <a name="configure-account-permissions-for-azure-ad-connect"></a>Azure AD Connect için hesap izinlerini yapılandırma

Azure AD Connect, kullanıcıları, grupları ve kimlik bilgileriyle şirket içi AD DS ortamı ile Azure AD arasında eşitleme yapmanızı sağlar. Azure AD Connect'i genellikle, şirket içi AD DS etki alanına katılan bir Windows Server 2012 veya daha sonraki bir bilgisayara yüklersiniz.

SSPR geri ödemeyle doğru çalışabilmek için Azure AD Connect'te belirtilen hesabın uygun izinlere ve seçeneklere sahip olması gerekir. Şu anda hangi hesabın kullanımda olduğundan emin değilseniz, Azure AD Connect'i açın ve **geçerli yapılandırmayı görüntüle** seçeneğini belirleyin. İzin eklemeniz gereken hesap **Eşitlenmiş Dizinler**altında listelenir. Aşağıdaki izinler ve seçenekler hesapta ayarlanmalıdır:

* **Parola sıfırlama**
* **İzinleri** üzerine yazma`lockoutTime`
* **İzinleri** üzerine yazma`pwdLastSet`
* "Süresi Dolmayan Parola" için **genişletilmiş haklar:**
   * Bu ormandaki *her etki alanının* kök nesnesi
   * SSPR kapsamında olmak istediğiniz kullanıcı organizasyon birimleri (OUs)

Bu izinleri atamazsak, yazma doğru yapılandırılmış gibi görünür, ancak kullanıcılar şirket içi parolalarını buluttan yönetirken hatalarla karşılaşır.

Parola yazma nın gerçekleşmesi için uygun izinleri ayarlamak için aşağıdaki adımları tamamlayın:

1. Şirket içi AD DS ortamınızda, uygun *etki alanı yöneticisi* izinlerine sahip bir hesapla Etkin **Dizin Kullanıcıları ve Bilgisayarları** açın.
1. **Görünüm** menüsünden **Gelişmiş özelliklerin** açık olduğundan emin olun.
1. Sol panelde, etki alanının kökünü temsil eden nesneyi sağ seçin ve **Gelişmiş Özellikler** > **Güvenlik'i** > **seçin.**
1. **İzinler** sekmesinden **Ekle'yi**seçin.
1. **Principal**için, izinlerin uygulanması gereken hesabı (Azure AD Connect tarafından kullanılan hesap) seçin.
1. Açılan liste **için geçerli dir,** **Soyundan Gelen Kullanıcı nesneleri'ni**seçin.
1. *İzinler*altında, aşağıdaki seçenekler için kutuları seçin:
    * **Parola sıfırlama**
1. *Özellikler*altında, aşağıdaki seçenekler için kutuları seçin. Varsayılan olarak zaten ayarlanmış olabilecek bu seçenekleri bulmak için listede gezinmeniz gerekir:
    * **Lokavt YazmaTime**
    * **pwdLastSet yaz**

    [![](media/tutorial-enable-sspr-writeback/set-ad-ds-permissions-cropped.png "Set the appropriate permissions in Active Users and Computers for the account that is used by Azure AD Connect")](media/tutorial-enable-sspr-writeback/set-ad-ds-permissions.png#lightbox)

1. Hazır olduğunuzda, değişiklikleri uygulamak için **Uygula / Tamam'ı** seçin ve açık iletişim kutularını çıkın.

İzinleri güncellediğinizde, bu izinlerin dizininizdeki tüm nesnelere çoğaltılması bir saat veya daha uzun sürebilir.

Şirket içi AD DS ortamındaki parola ilkeleri parola sıfırlamaişlemlerinin doğru şekilde işlenmesini engelleyebilir. Parola yazmanın en verimli şekilde çalışabilmesi için *Minimum parola yaşı* için grup ilkesinin 0 olarak ayarlanması gerekir. Bu **ayar, Bilgisayar Yapılandırma > İlkeleri > Windows Ayarları > Güvenlik Ayarları > Hesap İlkeleri** içinde `gpedit.msc`bulunabilir. 

Grup ilkesini güncellerseniz, güncelleştirilmiş ilkenin çoğaltılmasını bekleyin veya komutu `gpupdate /force` kullanın.

> [!Note]
> Parolaların hemen değiştirilebilmesi için parola yazma nın 0 olarak ayarlanması gerekir. Ancak, kullanıcılar şirket içi ilkelere uyarsa ve *Minimum parola yaşı* sıfırdan büyük bir değere ayarlanırsa, şirket içi ilkeler değerlendirildikten sonra parola yazma yine de çalışır. 

## <a name="enable-password-writeback-in-azure-ad-connect"></a>Azure AD Connect'te parola yazmayı etkinleştirme

Azure AD Connect'teki yapılandırma seçeneklerinden biri parola yazma dır. Bu seçenek etkinleştirildiğinde, parola değiştirme olayları Azure AD Connect'in güncelleştirilmiş kimlik bilgilerini şirket içi AD DS ortamına eşitlemasına neden olur.

Self servis parola sıfırlama yazma yı etkinleştirmek için önce Azure AD Connect'te geri yazma seçeneğini etkinleştirin. Azure AD Connect sunucunuzdan aşağıdaki adımları tamamlayın:

1. Azure AD Connect sunucunuzda oturum açın ve **Azure AD Connect** yapılandırma sihirbazını başlatın.
1. **Hoş Geldiniz** sayfasında, **Yapılandır**’ı seçin.
1. **Ek görevler** sayfasında **Eşitleme seçeneklerini özelleştirme**'yi ve ardından **İleri**'yi seçin.
1. Azure **AD'ye Bağlan** sayfasında, Azure kiracınız için genel bir yönetici kimlik bilgisi girin ve ardından **İleri'yi**seçin.
1. **Dizinleri bağla** ve **Etki Alanı/OU** filtreleme sayfalarında **İleri**'yi seçin.
1. **İsteğe bağlı özellikler** sayfasında **Parola geri yazma** özelliğinin yanındaki kutuyu işaretleyin ve **İleri**'yi seçin.

    ![Parola geri yazmak için Azure AD Connect'i yapılandırma](media/tutorial-enable-sspr-writeback/enable-password-writeback.png)

1. **Yapılandırma için hazır** sayfasında **Yapılandır**'ı seçin ve işlemin tamamlanmasını bekleyin.
1. Yapılandırma tamamlandığında **Çıkış**'ı seçin.

## <a name="enable-password-writeback-for-sspr"></a>SSPR için parola yazma yı etkinleştirme

Azure AD Connect'te parola yazma özelliği etkinleştirildiğinde, artık yeniden yazmak için Azure AD SSPR'yi yapılandırın. SSPR'nin parola yazmayı kullanmasını etkinleştirdiğinizde, parolalarını değiştiren veya sıfırlayan kullanıcılar, güncelleştirilmiş parolayı şirket içi AD DS ortamına da senkronize eder.

SSPR'de parola yazmayı etkinleştirmek için aşağıdaki adımları tamamlayın:

1. Genel bir yönetici hesabı kullanarak [Azure portalında](https://portal.azure.com) oturum açın.
1. Azure Etkin **Dizini'ni**arayın ve seçin, **Parola sıfırlama'yı**seçin ve ardından **şirket içi tümleştirmeyi**seçin.
1. **Şirket dizininize parolaları geri yazma** seçeneğini *Evet'e*ayarlayın.
1. **Kullanıcıların parolalarını sıfırlamadan hesapların kilidini açmalarına** izin *Yes*verme seçeneğini belirleyin?

    ![Parola yazma için Azure AD self servis parola sıfırlamayı etkinleştirme](media/tutorial-enable-sspr-writeback/enable-sspr-writeback.png)

1. Hazır olduğunda **Kaydet'i**seçin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticinin bir parçası olarak yapılandırdığınız SSPR geri yazma işlevini artık kullanmak istemiyorsanız, aşağıdaki adımları tamamlayın:

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Azure Etkin **Dizini'ni**arayın ve seçin, **Parola sıfırlama'yı**seçin ve ardından **şirket içi tümleştirmeyi**seçin.
1. **Şirket dizininize parolaları geri yazma** seçeneğini no olarak *No*ayarlayın.
1. **Kullanıcıların parolalarını sıfırlamadan hesapların kilidini açmalarına** izin *No*verme seçeneğini ayarlayın?

Artık herhangi bir parola işlevselliği kullanmak istemiyorsanız, Azure AD Connect sunucunuzdan aşağıdaki adımları tamamlayın:

1. Azure AD Connect sunucunuzda oturum açın ve **Azure AD Connect** yapılandırma sihirbazını başlatın.
1. **Hoş Geldiniz** sayfasında, **Yapılandır**’ı seçin.
1. **Ek görevler** sayfasında **Eşitleme seçeneklerini özelleştirme**'yi ve ardından **İleri**'yi seçin.
1. Azure **AD'ye Bağlan** sayfasında, Azure kiracınız için genel bir yönetici kimlik bilgisi girin ve ardından **İleri'yi**seçin.
1. **Dizinleri bağla** ve **Etki Alanı/OU** filtreleme sayfalarında **İleri**'yi seçin.
1. **İsteğe Bağlı özellikler** sayfasında, Parola **yazma** nın yanındaki kutuyu seçin ve **İleri'yi**seçin.
1. **Yapılandırma için hazır** sayfasında **Yapılandır**'ı seçin ve işlemin tamamlanmasını bekleyin.
1. Yapılandırma tamamlandığında **Çıkış**'ı seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, Azure AD SSPR'nin şirket içi AD DS ortamına geri yazmasını sağladınız. Şunları öğrendiniz:

> [!div class="checklist"]
> * Parola yazma için gerekli izinleri yapılandırma
> * Azure AD Connect'te parola yazma seçeneğini etkinleştirme
> * Azure AD SSPR'de parola yazmayı etkinleştirme

> [!div class="nextstepaction"]
> [Oturum açma sırasında risk değerlendirmesi yapma](tutorial-risk-based-sspr-mfa.md)
