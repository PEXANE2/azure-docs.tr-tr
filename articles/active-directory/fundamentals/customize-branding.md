---
title: Kuruluşunuzun oturum açma sayfasına marka ekleme - Azure AD
description: Kuruluşunuzun markalarını Azure Active Directory oturum açma sayfasına nasıl ekleyeceğiniz le ilgili talimatlar.
services: active-directory
author: msaburnley
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 09/18/2018
ms.author: ajburnle
ms.reviewer: kexia
ms.custom: it-pro, seodec18, fasttrack-edit
ms.collection: M365-identity-device-management
ms.openlocfilehash: 441fdd14cc2c734b6ce532f3ad1d30663b2f56c5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80049805"
---
# <a name="add-branding-to-your-organizations-azure-active-directory-sign-in-page"></a>Kuruluşunuzun Azure Active Directory oturum açma sayfasına marka ekleme
Azure Etkin Dizin (Azure AD) oturum açma sayfalarınızda tutarlı bir görünüm sağlamak için kuruluşunuzun logosunu ve özel renk şemalarını kullanın. Oturum açma sayfalarınız, kullanıcılar kuruluşunuzun kimlik sağlayıcınız olarak Azure AD'yi kullanan Office 365 gibi web tabanlı uygulamalarında oturum açtıklarında görünür.

>[!Note]
>Özel marka ekleme, Azure Active Directory Premium 1, Premium 2 veya Basic sürümlerini kullanmanızı veya Office 365 lisansına sahip olduğunuzu gerektirir. Lisanslama ve sürümler hakkında daha fazla bilgi için Azure [AD Premium'a kaydolun'](active-directory-get-started-premium.md)a bakın.<br><br>Azure AD Premium ve Temel sürümleri, Azure Active Directory'nin dünya çapındaki örneğini kullanan Çin'deki müşterilerin kullanımına sunulmuştur. Azure AD Premium ve Temel sürümleri, şu anda Çin’de 21Vianet tarafından işletilen Azure hizmeti kapsamında desteklenmemektedir. Daha fazla bilgi için [Azure Active Directory Forumu](https://feedback.azure.com/forums/169401-azure-active-directory/)’nu kullanarak bizimle görüşün.

## <a name="customize-your-azure-ad-sign-in-page"></a>Azure AD oturum açma sayfanızı özelleştirin
Kullanıcılar kuruluşunuzun kiracıya özel uygulamalarında oturum açtığında `https://outlook.com/contoso.com`(örneğin, bir etki alanı değişkenini geçerken veya . `https://passwordreset.microsoftonline.com/?whr=contoso.com`

Kullanıcılarınız www\.office.com gibi sitelere gittiğinde özel markanız hemen görünmez. Bunun yerine, özelleştirilmiş markanız görünmeden önce kullanıcının oturum açması gerekiyor. Kullanıcı oturum imzaladıktan sonra, markanın görünmesi 15 dakika veya daha uzun sürebilir. 

> [!NOTE]
> Tüm marka öğeleri isteğe bağlıdır. Örneğin, arka plan resmi olmayan bir banner logosu belirtirseniz, oturum açma sayfası logonuzu hedef siteden varsayılan arka plan resmiyle birlikte gösterir (örneğin, Office 365).<br><br>Ayrıca, oturum açma sayfası markası kişisel Microsoft hesaplarına taşımaz. Kullanıcılarınız veya iş konuklarınız kişisel bir Microsoft hesabı kullanarak oturum açıyorsa, oturum açma sayfası kuruluşunuzun markasını yansıtmaz.

### <a name="to-customize-your-branding"></a>Markanızı özelleştirmek için
1. Dizin için bir Genel yönetici hesabı kullanarak [Azure portalda](https://portal.azure.com/) oturum açın.

2. **Azure Etkin Dizini'ni**seçin ve ardından **Şirket markasını**seçin ve sonra **Yapıla'yı**seçin.

    ![Contoso - Şirket marka sayfası, Yapılandırma seçeneği vurgulanır](media/customize-branding/company-branding-configure-button.png)

3. **Yapılandırılan şirket markalama** sayfasında, aşağıdaki bilgilerden herhangi birini veya tümlerini sağlayın.

    >[!Important]
    >Bu sayfaya eklediğiniz tüm özel görüntülerin görüntü boyutu (piksel) ve dosya boyutu (KB) kısıtlamaları vardır. Bu kısıtlamalar nedeniyle, büyük olasılıkla doğru boyutlu görüntüler oluşturmak için bir fotoğraf düzenleyicisi kullanmanız gerekir.

    - **Genel ayarlar**

        ![Genel ayarları tamamlanan şirket markalama sayfasını yapılandırın](media/customize-branding/configure-company-branding-general-settings.png)

        - **Dil.** Dil otomatik olarak varsayılan olarak ayarlanır ve değiştirilemez.
        
        - **Oturum açma sayfası arka plan görüntüsü.** Oturum açma sayfalarınızın arka planı olarak görünmesi için bir .png veya .jpg resim dosyası seçin. 
        
            Görüntü 1920x1080 pikselden daha büyük olamaz ve 300 KB'den az dosya boyutuna sahip olmalıdır.

        - **Afiş logosu.** Kullanıcı bir kullanıcı adı girdikten sonra ve **Uygulamalarım** portalı sayfasında oturum açma sayfasında görünmek için logonuzun bir .png veya .jpg sürümünü seçin.
            
            Görüntü 60 pikselden daha uzun veya 280 pikselden daha geniş olamaz. Arka plan logo arka planınızla eşleşmeyebileceğinden saydam bir görüntü kullanmanızı öneririz. Ayrıca resmin etrafına dolgu eklememenizi öneririz veya logonuz küçük görünmesini sağlayabilir.

        - **Kullanıcı adı ipucu.** Kullanıcı adlarını unuturlarsa kullanıcılara görünen ipucu metnini yazın. Bu metin Unicode olmalıdır, bağlantılar veya kod olmadan ve 64 karakter geçemez. Konuklar uygulamanızda oturum açsa, bu ipucunu eklememenizi öneririz.

        - **Oturum açma sayfası metni.** Oturum açma sayfasının alt kısmında görünen metni yazın. Bu metni, telefon numarası gibi ek bilgileri yardım masanıza veya yasal bir bildirime iletmek için kullanabilirsiniz. Bu metin Unicode olmalıdır ve 256 karakteri geçmemelidir. Ayrıca bağlantılar veya HTML etiketleri dahil edilmemenizi öneririz.

    - **Gelişmiş ayarlar**
            
        ![Gelişmiş ayarlar tamamlanmış olan şirket markalama sayfasını yapılandırın](media/customize-branding/configure-company-branding-advanced-settings.png)   

        - **Oturum açma sayfası arka plan rengi.** Düşük bant genişliği bağlantı durumlarında arka plan görüntünüzün yerine görünecek hexadecimal rengi (örneğin, beyaz #FFFFFF) belirtin. Banner logonuzun birincil rengini veya kuruluş renginizi kullanmanızı öneririz.

        - **Kare logo görüntüsü.** Yeni Windows 10 Enterprise cihazlarının kurulum işlemi sırasında kullanıcılara görünmesi için kuruluşunuzun logosunun bir .png (tercih edilen) veya .jpg görüntüsünü seçin. Bu resim yalnızca Windows kimlik doğrulamaiçin kullanılır ve yalnızca dağıtım için [Windows Otomatik Pilot'u]( https://docs.microsoft.com/windows/deployment/windows-autopilot/windows-10-autopilot) kullanan kiracılarda veya diğer Windows 10 deneyimlerindeki parola giriş sayfalarında görünür. Bazı durumlarda, onay iletişim kutusunda da görünebilir.
        
            Görüntü 240x240 pikselden büyük boyutta olamaz ve dosya boyutu 10 KB'den az olmalıdır. Arka plan logo arka planınızla eşleşmeyebileceğinden saydam bir görüntü kullanmanızı öneririz. Ayrıca resmin etrafına dolgu eklememenizi öneririz veya logonuz küçük görünmesini sağlayabilir.
    
        - **Kare logo görüntüsü, koyu renk teması.** Yukarıdaki kare logo görüntüsüyle aynı. Bu logo görüntüsü, kutudan çıkma deneyimi (OOBE) sırasında Windows 10 Azure AD'nin katıldığı ekranlarda olduğu gibi koyu bir arka planla kullanıldığında kare logo görüntüsünün yerini alır.  Logonuz beyaz, koyu mavi ve siyah arka planlarda iyi görünüyorsa, bu resmi eklemeniz gerekmez. 
        
        - **Oturum da kalma seçeneğini göster.** Açıkça oturumunuzu silene kadar kullanıcılarınızın Azure AD'da oturum açmalarına izin vermeyi seçebilirsiniz. **Hayır'ı**seçerseniz, bu seçenek gizlenir ve tarayıcı her kapatıldığında ve yeniden açıldığında kullanıcıların oturum açması gerekir.
        
            >[!Note]
            >SharePoint Online ve Office 2010’un bazı özellikleri kullanıcıların oturumun açık kalmasını seçebilmesine bağlıdır. Bu ayarı **Hayır** olarak ayarlarsanız kullanıcılarınız oturum açmaya yönelik ek ve beklenmeyen istemler görebilir.
   

3. Markanızı eklemeyi bitirdikten sonra **Kaydet'i**seçin.

    Bu işlem ilk özel marka yapılandırmanızı oluşturursa, kiracınız için varsayılan olur. Ek yapılandırmalarınız varsa, varsayılan yapılandırmanızı seçebilirsiniz.
    
    >[!Important]
    >Kiracınıza daha fazla kurumsal marka yapılandırması eklemek için **Contoso - Company marka sayfasında** Yeni **dili** seçmeniz gerekir. Bu, yukarıdaki adımları izleyebileceğiniz **Yapılandırılmış şirket markalandırma** sayfasını açar.

## <a name="update-your-custom-branding"></a>Özel markanızı güncelleyin
Özel markanızı oluşturduktan sonra, geri dönüp istediğiniz her şeyi değiştirebilirsiniz.

### <a name="to-edit-your-custom-branding"></a>Özel markanızı yeniden yapmak için
1. Dizin için bir Genel yönetici hesabı kullanarak [Azure portalda](https://portal.azure.com/) oturum açın.

2. **Azure Etkin Dizini'ni**seçin ve ardından **Şirket markasını**seçin ve sonra **Yapıla'yı**seçin.

    ![Contoso - Şirket markalama sayfası, varsayılan yapılandırma gösterilen](media/customize-branding/company-branding-default-config.png)

3. **Yapılandırılan şirket markalandırma** sayfasında, bu makalenin [Azure REKLAM oturum açma sayfanızı özelleştir'deki](#customize-your-azure-ad-sign-in-page) açıklamalara bağlı olarak, bilgilerden herhangi birini ekleyin, kaldırın veya değiştirin.

4. **Kaydet'i**seçin.

   Oturum açma sayfası markasında yaptığınız değişikliklerin görünmesi bir saate kadar sürebilir.

## <a name="add-language-specific-company-branding-to-your-directory"></a>Dizininize dile özgü şirket markası ekleme
Özgün yapılandırmanızın dilini varsayılan dilinizden değiştiremezsiniz. Ancak, farklı bir dilde yapılandırma gerekiyorsa, yeni bir yapılandırma oluşturabilirsiniz.

### <a name="to-add-a-language-specific-branding-configuration"></a>Dile özel marka yapılandırması eklemek için

1. Dizin için bir Genel yönetici hesabı kullanarak [Azure portalda](https://portal.azure.com/) oturum açın.

2. **Azure Etkin Dizini'ni**seçin ve ardından **Şirket markasını**seçin ve ardından **Yeni dil'i**seçin.

    ![Contoso - Yeni dil seçeneği vurgulanan şirket markasayfası](media/customize-branding/company-branding-new-language.png)

3. Şirket **markalandırma** sayfasında, dilinizi seçin (örneğin, Fransızca) ve bu makalenin Azure AD oturum [açma sayfanızı Özelleştir](#customize-your-azure-ad-sign-in-page) bölümündeki açıklamalara göre çevrilmiş bilgilerinizi ekleyin.

4. **Kaydet'i**seçin.

    **Contoso – Yeni** Fransızca yapılandırmanızı göstermek için şirket markalama sayfası güncellemeleri.

    ![Contoso - Şirket markalama sayfası, varsayılan yapılandırma gösterilen](media/customize-branding/company-branding-french-config.png)

## <a name="add-your-custom-branding-to-pages"></a>Özel markanızı sayfalara ekleme
URL'nin sonunu metinle değiştirerek sayfalara özel markanızı `?whr=yourdomainname`ekleyin. Bu değişiklik, Çok Faktörlü Kimlik Doğrulama (MFA) kurulum sayfası, Self servis Parola Sıfırlama (SSPR) kurulum sayfası ve oturum açma sayfası dahil olmak üzere çeşitli sayfalarda çalışır.

**Örnekler:**

**Orijinal URL:**https://aka.ms/MFASetup<br>
**Özel URL:**`https://account.activedirectory.windowsazure.com/proofup.aspx?whr=contoso.com`

**Orijinal URL:**https://aka.ms/SSPR<br>
**Özel URL:**`https://passwordreset.microsoftonline.com/?whr=contoso.com`

 
