---
title: Hızlı başlangıç-Azure AD self servis parola sıfırlama
description: Bu hızlı başlangıçta, kullanıcıların kendi parolalarını sıfırlamasına ve BT departmanı yükünü azaltmasına izin vermek için Azure AD self servis parola sıfırlama 'yı nasıl yapılandıracağınızı öğreneceksiniz.
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: quickstart
ms.date: 12/10/2019
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: dd178f52665c77f03a48d87a9e73c9019390bb21
ms.sourcegitcommit: 276c1c79b814ecc9d6c1997d92a93d07aed06b84
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2020
ms.locfileid: "76154865"
---
# <a name="quickstart-configure-azure-active-directory-self-service-password-reset"></a>Hızlı başlangıç: Azure Active Directory self servis parola sıfırlamayı yapılandırma

Bu hızlı başlangıçta, kullanıcıların parolalarını sıfırlamalarını veya hesaplarının kilidini açmalarına olanak tanımak için Azure Active Directory (AD) self servis parola sıfırlama (SSPR) yapılandırabilirsiniz. SSPR ile kullanıcılar, yardım masası veya yönetici yardımı olmadan kendi kimlik bilgilerini sıfırlayabilirler. Bu özellik, kullanıcıların, ek destek beklemeden hesabına erişimini yeniden kazanmasına olanak tanır.

> [!IMPORTANT]
> Bu hızlı başlangıçta self servis parola sıfırlamayı etkinleştirme Yöneticisi gösterilir. Self servis parola sıfırlama için zaten kayıtlı bir son kullanıcı varsa ve hesabınıza geri dönmek için https://aka.ms/sspr gidin.
>
> BT ekibiniz kendi parolanızı sıfırlama özelliğini etkinleştirmediyseniz, ek yardım için yardım masasına ulaşın.

## <a name="prerequisites"></a>Ön koşullar

* En az deneme sürümü lisansı etkinleştirilmiş çalışan bir Azure AD kiracısına erişim.
    * Gerekirse, [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Genel Yönetici ayrıcalıklarına sahip olan bir hesap.
* Yönetici olmayan test kullanıcısı, sizin bildiğiniz bir parolayla, örneğin *testuser*.
    * Bir kullanıcı oluşturmanız gerekiyorsa bkz. [hızlı başlangıç: Azure Active Directory yeni kullanıcı ekleme](../add-users-azure-active-directory.md).
* Yönetici olmayan test kullanıcısının, *SSPR-test-Group*gibi bir üyesi olduğunu test etmek için bir pilot grup.
    * Bir grup oluşturmanız gerekiyorsa bkz. [Grup oluşturma ve Azure Active Directory üye ekleme](../active-directory-groups-create-azure-portal.md).

## <a name="enable-self-service-password-reset"></a>Kendi kendine parola sıfırlamayı etkinleştirme

[Bu işlemi YouTube 'da video olarak görüntüleyin](https://youtu.be/Pa0eyqjEjvQ)

1. Azure portal menüsünde veya **giriş** sayfasından **Azure Active Directory** ' i seçin ve ardından **parola sıfırlama**' yı seçin.

1. **Self servis parola sıfırlama**seçeneğinin altındaki **Özellikler** sayfasında, **Seçili**' ı seçin.
1. **Grup Seç**' i seçin ve ardından Bu makalenin, *SSPR-test-Group*gibi Önkoşullar bölümünün bir parçası olarak oluşturulan pilot grubunuzu seçin. Hazırlanıyor, **Kaydet**' i seçin.
1. **Kimlik doğrulama yöntemleri** sayfasında, aşağıdaki seçimleri yapın ve ardından **Kaydet**' i seçin:
    * Sıfırlama için gereken yöntemlerin sayısı: **1**
    * Kullanıcıların yararlanabileceği yöntemler:
        * **Mobil uygulama kodu**
        * **E-posta**

    > [!div class="mx-imgBorder"]
    > ![SSPR için kimlik doğrulama yöntemlerini seçme][Authentication]

4. **Kayıt** sayfasından aşağıdaki seçimleri yapın ve ardından **Kaydet**' i seçin:
   * Kullanıcılardan oturum açarken kaydolmalarını isteme: **Evet**
   * Kullanıcıların kimlik doğrulaması bilgilerini yeniden onaylamasını istemeden önce geçen gün sayısını ayarlama: **365**

## <a name="test-self-service-password-reset"></a>Self servis parola sıfırlamayı test etme

Şimdi, SSPR yapılandırmanızı test *etme gibi önceki*bölümde seçtiğiniz grubun parçası olan bir test kullanıcısı ile test etmenizi sağlar. Microsoft, Azure yönetici hesapları için güçlü kimlik doğrulama gereksinimleri uyguladığından, testi yönetici ayrıcalıklarına sahip bir kullanıcıyla yapmanız halinde alacağınız sonuç farklı olabilir. Yönetici parolası ilkesiyle ilgili daha fazla bilgi için [parola ilkesi](concept-sspr-policy.md) makalemize bakın.

1. InPrivate modunda veya gizli modda yeni bir tarayıcı penceresi açın ve [https://aka.ms/ssprsetup](https://aka.ms/ssprsetup) adresine gidin.
2. *Testuser*gibi yönetici olmayan bir test kullanıcısı ile oturum açın ve kimlik doğrulama telefonunuzu kaydedin.
3. Tamamlandıktan **sonra, işaretlenmiş düğmeyi** seçin ve tarayıcı penceresini kapatın.
4. InPrivate modunda veya gizli modda yeni bir tarayıcı penceresi açın ve [https://aka.ms/sspr](https://aka.ms/sspr) adresine gidin.
5. Yönetici olmayan test kullanıcılarınızın kullanıcı KIMLIĞINI ( *testuser*gibi), CAPTCHA ' dan karakterler ' i girin ve ardından **İleri**' yi seçin.
6. Parolanızı sıfırlamak için doğrulama adımlarını izleyin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Self servis parola sıfırlamayı devre dışı bırakmak için Azure portal **Azure Active Directory** arayın ve seçin. **Parola sıfırlama**' yı seçin ve ardından **self servis parola sıfırlama etkin**altında **yok** ' u seçin. Hazırlanıyor, **Kaydet**' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, yalnızca bulutta bulunan kullanıcılarınız için self servis parola sıfırlamayı yapılandırmayı öğrendiniz. Daha ayrıntılı bir dağıtım için dağıtım kılavuzuna geçin.

> [!div class="nextstepaction"]
> [Self servis parola sıfırlamayı dağıtma](howto-sspr-deployment.md)

[Authentication]: ./media/quickstart-sspr/sspr-authentication-methods.png "Kullanılabilir Azure AD kimlik doğrulama yöntemleri ve gereken miktar"

<!-- INTERNAL LINKS -->
[register-sspr]: ../user-help/active-directory-passwords-reset-register.md
[reset-password]: ../user-help/active-directory-passwords-update-your-own-password.md
