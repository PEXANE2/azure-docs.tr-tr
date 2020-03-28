---
title: Özel Azure Etkin Dizin parola koruma listelerini yapılandırma
description: Bu eğitimde, ortamınızdaki yaygın sözcükleri kısıtlamak için Azure Active Directory için özel yasaklı parola koruma listelerini nasıl yapılandırabileceğinizi öğrenirsiniz.
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: tutorial
ms.date: 02/27/2020
ms.author: iainfou
author: iainfoulds
ms.reviewer: rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4abb15462689470c87e9cf5ba8d5be8af2e45bfd
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "78252848"
---
# <a name="tutorial-configure-custom-banned-passwords-for-azure-active-directory-password-protection"></a>Öğretici: Azure Active Directory parola koruması için özel yasaklı parolaları yapılandırma

Kullanıcılar genellikle okul, spor takımı veya ünlü kişi gibi yaygın yerel sözcükleri kullanan parolalar oluşturur. Bu parolaları tahmin etmek kolaydır ve sözlük tabanlı saldırılara karşı zayıftır. Kuruluşunuzdaki güçlü parolaları zorlamak için Azure Active Directory (Azure AD) özel yasaklı parola listesi, değerlendirmek ve engellemek için belirli dizeler eklemenize izin verebiliyor. Özel yasaklı parola listesinde eşleşme varsa parola değiştirme isteği başarısız olur.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Özel yasaklı parolaları etkinleştirme
> * Özel yasaklı parola listesine giriş ekleme
> * Yasaklı bir parolayla parola değişikliklerini test edin

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdaki kaynaklara ve ayrıcalıklara ihtiyacınız vardır:

* En az deneme sürümü lisansı etkinleştirilmiş çalışan bir Azure AD kiracısına erişim.
    * Gerekirse, [ücretsiz bir oluşturun.](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
* *Genel yönetici* ayrıcalıklarına sahip bir hesap.
* *Testuser*gibi bildiğiniz bir parolaya sahip yönetici olmayan bir kullanıcı. Bu öğreticide bu hesabı kullanarak bir parola değiştirme olayını sınarsınız.
    * Bir kullanıcı oluşturmanız gerekiyorsa, [bkz.](../add-users-azure-active-directory.md)
    * Parola değiştirme işlemini yasaklı bir parola kullanarak sınamak için Azure AD kiracısının [self servis parola sıfırlama için yapılandırılması](tutorial-enable-sspr.md)gerekir.

## <a name="what-are-banned-password-lists"></a>Yasaklı parola listeleri nelerdir?

Azure AD, genel olarak yasaklanmış bir parola listesi içerir. Genel olarak yasaklı parola listesinin içeriği herhangi bir dış veri kaynağına dayanmaz. Bunun yerine, genel yasaklı parola listesi Azure AD güvenlik telemetrisi ve analizinin devam eden sonuçlarına dayanır. Bir kullanıcı veya yönetici kimlik bilgilerini değiştirmeye veya sıfırlamaya çalıştığında, istenen parola yasaklı parolalar listesiyle karşılaştırılır. Genel olarak yasaklı parola listesinde bir eşleşme varsa parola değiştirme isteği başarısız olur.

Hangi parolalara izin verildiği nde esneklik sağlamak için, özel yasaklı parola listesi de tanımlayabilirsiniz. Özel yasaklı parola listesi, kuruluşunuzdaki güçlü parolaları zorlamak için genel olarak yasaklanan parola listesiyle birlikte çalışır. Kuruluşa özgü terimler, aşağıdaki örnekler gibi özel yasaklı parola listesine eklenebilir:

* Marka adları
* Ürün adları
* Şirket merkezi gibi konumlar
* Şirkete özel iç terimler
* Belirli şirket anlamı olan kısaltmalar

Bir kullanıcı, genel veya özel yasaklı parola listesinde ki bir şeyin parolasını sıfırlamaya çalıştığında, aşağıdaki hata iletilerinden birini görür:

* *Ne yazık ki, parolanız parolanızı kolayca tahmin edilebilir kılan bir sözcük, tümcecik veya desen içerir. Lütfen farklı bir parola ile yeniden deneyin.*
* *Ne yazık ki, yöneticiniz tarafından engellenen sözcükler veya karakterler içerdiğinden bu parolayı kullanamazsınız. Lütfen farklı bir parola ile yeniden deneyin.*

Özel yasaklı parola listesi en fazla 1000 terimle sınırlıdır. Büyük parola listelerini engellemek için tasarlanmaz. Özel yasaklı parola listesinin avantajlarını en üst düzeye çıkarmak için, [özel yasaklı parola listesi kavramlarını](concept-password-ban-bad.md#custom-banned-password-list) ve parola değerlendirme [algoritmasına genel bakışı](concept-password-ban-bad.md#how-are-passwords-evaluated)gözden geçirin.

## <a name="configure-custom-banned-passwords"></a>Özel yasaklı parolaları yapılandırma

Özel yasaklı parola listesini etkinleştirelim ve bazı girişler ekleyelim. Özel yasaklı parola listesine istediğiniz zaman ek girişler ekleyebilirsiniz.

Özel yasaklı parola listesini etkinleştirmek ve girişeklemek için aşağıdaki adımları tamamlayın:

1. *Genel yönetici* izinlerine sahip bir hesabı kullanarak [Azure portalında](https://portal.azure.com) oturum açın.
1. Azure Active **Directory'yi**arayın ve seçin, ardından sol taraftaki menüden **Güvenlik'i** seçin.
1. **Yönet** menüsü üstbilginin altında Kimlik **Doğrulama yöntemlerini**seçin, ardından **Parola koruması.**
1. **Özel listeyi Uygula** seçeneğini *Evet*olarak ayarlayın.
1. **Özel yasaklı parola listesine**dizeleri ekleyin, satır başına bir dize. Aşağıdaki hususlar ve sınırlamalar özel yasaklı parola listesi için geçerlidir:

    * Özel yasaklı parola listesi en fazla 1000 terim içerebilir.
    * Özel yasaklı parola listesi büyük/küçük harf duyarsızdır.
    * Özel yasaklı parola listesi, "o" ve "0" veya "a" ve "@" gibi yaygın karakter değiştirmesini dikkate alır.
    * En düşük dize uzunluğu dört karakterdir ve en fazla 16 karakterdir.

    Aşağıdaki örnekte gösterildiği gibi, yasaklamak için kendi özel parolalarınızı belirtin

    [![](media/tutorial-configure-custom-password-protection/enable-configure-custom-banned-passwords-cropped.png "Modify the custom banned password list under Authentication Methods in the Azure portal")](media/tutorial-configure-custom-password-protection/enable-configure-custom-banned-passwords.png#lightbox)

1. Windows Server **Active Directory'de parola korumayı etkinleştir** seçeneğini *Hayır'a*bırakın.
1. Özel yasaklı parolaları ve girişlerinizi etkinleştirmek için **Kaydet'i**seçin.

Özel yasaklı parola listesindeki güncelleştirmelerin uygulanması birkaç saat sürebilir.

Karma bir ortam için, [Azure AD parola korumasını şirket içi ortama](howto-password-ban-bad-on-premises-deploy.md)da dağıtabilirsiniz. Aynı genel ve özel yasaklı parola listeleri hem bulut hem de ön hazırlık parola değişikliği istekleri için kullanılır.

## <a name="test-custom-banned-password-list"></a>Özel yasaklı parola listesini test edin

Özel yasaklı parola listesini iş başında görmek için, parolayı önceki bölümde eklediğiniz bir parolayla değiştirmeyi deneyin. Azure AD parola değişikliğini işlemeye çalıştığında, parola özel yasaklı parola listesindeki bir girişle eşleşir. Daha sonra kullanıcıya bir hata görüntülenir.

> [!NOTE]
> Bir kullanıcının parolasını web tabanlı portalda sıfırlayabilmesi için, Azure AD kiracısının [self servis parola sıfırlama için yapılandırılması](tutorial-enable-sspr.md)gerekir.

1. [https://myapps.microsoft.com](https://myapps.microsoft.com) **'de Uygulamalarım** sayfasına gidin.
1. Sağ üst köşede adınızı seçin ve açılan menüden **Profil'i** seçin.

    ![Profil seçme](media/tutorial-configure-custom-password-protection/myapps-profile.png)

1. **Profil** sayfasında **parolayı değiştir'i**seçin.
1. **Parolayı Değiştir** sayfasında, varolan (eski) parolayı girin. Önceki bölümde tanımladığınız özel yasaklı parola listesinde bulunan yeni bir parola girin ve onaylayın, ardından **Gönder'i**seçin.
1. Aşağıdaki örnekte gösterildiği gibi, parolanın yönetici tarafından engellendiğini belirten bir hata iletisi döndürülür:

    ![Özel yasaklı parola listesinin bir parçası olan bir parolayı kullanmaya çalıştığınızda görüntülenen hata iletisi](media/tutorial-configure-custom-password-protection/password-change-error.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticinin bir parçası olarak yapılandırdığınız özel yasaklı parola listesini artık kullanmak istemiyorsanız, aşağıdaki adımları tamamlayın:

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Azure Active **Directory'yi**arayın ve seçin, ardından sol taraftaki menüden **Güvenlik'i** seçin.
1. **Yönet** menüsü üstbilginin altında Kimlik **Doğrulama yöntemlerini**seçin, ardından **Parola koruması.**
1. **Özel listeyi Uygula** seçeneğini *No*olarak ayarlayın.
1. Özel yasaklı parola yapılandırmasını güncelleştirmek için **Kaydet'i**seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, Azure AD için özel parola koruma listelerini etkinleştirdiniz ve yapılandırıldınız. Şunları öğrendiniz:

> [!div class="checklist"]
> * Özel yasaklı parolaları etkinleştirme
> * Özel yasaklı parola listesine giriş ekleme
> * Yasaklı bir parolayla parola değişikliklerini test edin

> [!div class="nextstepaction"]
> [Risk tabanlı Azure Multi-Factor Authentication'ı etkinleştirme](tutorial-mfa-applications.md)
