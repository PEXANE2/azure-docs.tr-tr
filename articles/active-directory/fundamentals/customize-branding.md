---
title: Kuruluşunuzun oturum açma sayfasına marka ekleme-Azure AD
description: Azure Active Directory oturum açma sayfasına kuruluşunuzun markasını ekleme hakkında yönergeler.
services: active-directory
author: msaburnley
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 06/24/2020
ms.author: ajburnle
ms.reviewer: kexia
ms.custom: it-pro, seodec18, fasttrack-edit
ms.collection: M365-identity-device-management
ms.openlocfilehash: 388c40867c21390c3de8a033a1bfa08eb95bd602
ms.sourcegitcommit: bf8c447dada2b4c8af017ba7ca8bfd80f943d508
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2020
ms.locfileid: "85367419"
---
# <a name="add-branding-to-your-organizations-azure-active-directory-sign-in-page"></a>Kuruluşunuzun Azure Active Directory oturum açma sayfasına marka ekleyin
Azure Active Directory (Azure AD) oturum açma sayfalarınızda tutarlı bir görünüm sağlamak için kuruluşunuzun logosu ve özel renk düzenlerini kullanın. Kullanıcılar, kimlik sağlayıcınız olarak Azure AD kullanan Office 365 gibi, kuruluşunuzun Web tabanlı uygulamalarında oturum açtığında oturum açma sayfalarınız görüntülenir.

>[!NOTE]
>Özel marka eklemek için Azure Active Directory Premium 1, Premium 2 veya Basic sürümleri kullanmanız ya da bir Office 365 lisansına sahip olmanız gerekir. Lisanslama ve sürümler hakkında daha fazla bilgi için bkz. [Azure AD Premium kaydolma](active-directory-get-started-premium.md).<br><br>Azure AD Premium ve Temel sürümleri, Azure Active Directory'nin dünya çapındaki örneğini kullanan Çin'deki müşterilerin kullanımına sunulmuştur. Azure AD Premium ve Temel sürümleri, şu anda Çin’de 21Vianet tarafından işletilen Azure hizmeti kapsamında desteklenmemektedir. Daha fazla bilgi için [Azure Active Directory Forumu](https://feedback.azure.com/forums/169401-azure-active-directory/)’nu kullanarak bizimle görüşün.

## <a name="customize-your-azure-ad-sign-in-page"></a>Azure AD oturum açma sayfanızı özelleştirin
Kullanıcılar kuruluşunuzun kiracıya özgü uygulamalarında oturum açtığında `https://outlook.com/contoso.com` veya gibi bir etki alanı değişkeni geçirirken görüntülenen Azure AD oturum açma sayfalarınızı özelleştirebilirsiniz `https://passwordreset.microsoftonline.com/?whr=contoso.com` .

Kullanıcılarınızın www office.com gibi sitelere gitmesi durumunda özel markası hemen görünmez \. . Bunun yerine, kullanıcının özelleştirilmiş marka görüntülenmeden önce oturum açması gerekir. Kullanıcı oturum açtıktan sonra, marka 15 dakika veya daha uzun sürebilir. 

> [!NOTE]
> Tüm marka öğeleri isteğe bağlıdır. Örneğin, arka plan resmi olmayan bir başlık logosu belirtirseniz, oturum açma sayfası logonuzu hedef siteden varsayılan bir arka plan görüntüsü ile gösterir (örneğin, Office 365).<br><br>Ayrıca, oturum açma sayfası markalaması kişisel Microsoft hesaplarının üzerine taşınmaz. Kullanıcılarınız veya iş konuklarınız kişisel bir Microsoft hesabı oturum açtığında, oturum açma sayfası kuruluşunuzun markasını yansıtmaz.

### <a name="to-customize-your-branding"></a>Markanızı özelleştirmek için
1. Dizin için bir Genel yönetici hesabı kullanarak [Azure portalda](https://portal.azure.com/) oturum açın.

2. **Azure Active Directory**' yi seçin ve ardından **Şirket markası**' ni seçin ve ardından **Yapılandır**' ı seçin.

    ![Contoso-Şirket markası sayfası, seçeneği vurgulanmış şekilde yapılandırın](media/customize-branding/company-branding-configure-button.png)

3. **Şirket markasını Yapılandır** sayfasında, aşağıdaki bilgilerin herhangi birini veya tümünü sağlayın.

    >[!IMPORTANT]
    >Bu sayfaya eklediğiniz tüm özel görüntülerin görüntü boyutu (piksel) ve olası dosya boyutu (KB), kısıtlamalar vardır. Bu kısıtlamalar nedeniyle, genellikle doğru boyutlu görüntüleri oluşturmak için büyük olasılıkla bir fotoğraf düzenleyicisi kullanmanız gerekir.

    - **Genel ayarlar**

        ![Genel ayarlar tamamlandığında şirket markasını yapılandırma sayfası](media/customize-branding/configure-company-branding-general-settings.png)

        - **Dildir.** Dil, varsayılan olarak otomatik olarak ayarlanır ve değiştirilemez.
        
        - **Oturum açma sayfası arka plan resmi.** Oturum açma sayfalarınızın arka planı olarak görüntülenecek bir. png veya. jpg görüntü dosyası seçin. Görüntü tarayıcının merkezine bağlanır ve görüntülenebilir alanın boyutuna ölçeklendirecektir. Boyutu 1920x1080 pikselden daha büyük olan veya 300 KB 'tan fazla dosya boyutu olan bir görüntü seçemezsiniz.
        
            Büyük bir konu odağı olmadan görüntülerin kullanılması önerilir, örneğin, ekranın ortasında donuk bir beyaz kutu görünür ve görüntülenen alanın boyutlarına bağlı olarak görüntünün herhangi bir parçasını kapsayabilir.

        - **Başlık logosu.** Kullanıcı bir Kullanıcı adı girdikten ve **uygulamalarım** portalı sayfasında, oturum açma sayfasında görünmesi için logonuzun bir. png veya. jpg sürümü seçin.
            
            Görüntü 60 pikselden veya 280 pikselden daha büyük olamaz. Arka plan, logo arka planıyla eşleşmediğinden saydam bir görüntü kullanmanızı öneririz. Ayrıca, görüntünün etrafına doldurma eklememeyi önermiyoruz veya logonuzu küçük bir görünüme alabilir.

        - **Kullanıcı adı ipucu.** Kullanıcı Kullanıcı adını unutduklarında görüntülenen ipucu metnini yazın. Bu metin, bağlantı veya kod olmadan Unicode olmalıdır ve 64 karakteri aşamaz. Konuklarınız uygulamanızda oturum açtığında, bu ipucunu eklememeyi öneririz.

        - **Oturum açma sayfası metni ve biçimlendirmesi.** Oturum açma sayfasının alt kısmında görüntülenen metni yazın. Bu metni, telefon numarası gibi ek bilgileri, yardım masasına veya yasal bir bildirime iletmek için kullanabilirsiniz. Bu metin Unicode olmalıdır ve 1024 karakteri aşmamalıdır.

           Girdiğiniz oturum açma sayfası metnini özelleştirebilirsiniz. Yeni bir paragrafa başlamak için Enter tuşunu iki kez kullanın. Ayrıca metin biçimlendirmesini kalın, italik, altı çizili veya tıklatılabilir bir bağlantı içerecek şekilde değiştirebilirsiniz. Metne biçimlendirme eklemek için aşağıdaki sözdizimini kullanın: 

          > Bağlanan```[text](link)``` 
          
          > Kalın: ``` **text** ``` veya``` __text__ ``` 
          
          > İtalik: ``` *text* ``` veya``` _text_ ``` 
          
          > Altçizgi``` ++text++ ``` 

    - **Gelişmiş ayarlar**
            
        ![Gelişmiş ayarlar tamamlandığında, şirket markasını yapılandırma sayfası](media/customize-branding/configure-company-branding-advanced-settings.png)   

        - **Oturum açma sayfası arka plan rengi.** Düşük bant genişliğine sahip bağlantı durumlarında arka plan resminizin yerini alacak olan onaltılık rengi (örneğin, beyaz #FFFFFF) belirtin. Başlık logonuzun veya kuruluşunuzun renginin birincil rengini kullanmanızı öneririz.

        - **Kare logosu resmi.** Yeni Windows 10 Enterprise cihazları için kurulum işlemi sırasında kullanıcılara görünmesi için kuruluşunuzun logosunun bir. png (tercih edilen) veya. jpg görüntüsünü seçin. Bu görüntü yalnızca Windows kimlik doğrulaması için kullanılır ve yalnızca Windows [Autopilot]( https://docs.microsoft.com/windows/deployment/windows-autopilot/windows-10-autopilot) for Deployment veya diğer Windows 10 deneyimlerinde parola giriş sayfaları kullanan kiracılar üzerinde görünür. Bazı durumlarda onay iletişim kutusunda da görünebilir.
        
            Görüntü, boyutu 240x240 pikselden daha büyük olamaz ve 10 KB 'tan küçük bir dosya boyutuna sahip olmalıdır. Arka plan, logo arka planıyla eşleşmediğinden saydam bir görüntü kullanmanızı öneririz. Ayrıca, görüntünün etrafına doldurma eklememeyi önermiyoruz veya logonuzu küçük bir görünüme alabilir.
    
        - **Kare logosu resmi, Koyu tema.** Yukarıdaki kare logosu görüntüsüyle aynı. Bu logo resmi, kutu dışı deneyim (OOBE) sırasında Windows 10 Azure AD 'ye katılmış ekranları gibi koyu bir arka planla birlikte kullanıldığında kare logosu görüntüsünün yerini alır.  Logonuz beyaz, koyu mavi ve siyah arka planlar üzerinde iyi görünüyorsa, bu görüntüyü eklemeniz gerekmez. 
        
        - **Oturum açmış durumda kalma seçeneğini göster.** Kullanıcılarınızın açık olarak oturumu açana kadar Azure AD 'de oturum açmış kalmasına izin verebilirsiniz. **Hayır**' ı seçerseniz, bu seçenek gizlidir ve kullanıcıların tarayıcının her kapatılışında ve yeniden açıldığı her seferinde oturum açması gerekir.

            Oturum açmış durumda kalma seçeneğini yapılandırma ve sorun giderme hakkında daha fazla bilgi edinmek için bkz. [Azure AD hesapları için ' oturum açmış durumda kal? ' Istemi yapılandırma](keep-me-signed-in.md)
        
            >[!NOTE]
            >SharePoint Online ve Office 2010’un bazı özellikleri kullanıcıların oturumun açık kalmasını seçebilmesine bağlıdır. Bu ayarı **Hayır** olarak ayarlarsanız kullanıcılarınız oturum açmaya yönelik ek ve beklenmeyen istemler görebilir.
   

3. Markanızı eklemeyi bitirdikten sonra **Kaydet**' i seçin.

    Bu işlem ilk özel marka yapılandırmanızı oluşturursa, kiracınız için varsayılan değer olur. Ek yapılandırmalar varsa, varsayılan yapılandırmanızı seçebileceksiniz.
    
    >[!IMPORTANT]
    >Kiracınıza daha fazla kurumsal marka yapılandırması eklemek için **contoso-Company marka** sayfasında **Yeni dil** ' i seçmeniz gerekir. Bu, yukarıdaki adımları izleyerek **Şirket markasını Yapılandır** sayfasını açar.

## <a name="update-your-custom-branding"></a>Özel markanızı güncelleştirme
Özel markanızı oluşturduktan sonra geri dönüp istediğiniz herhangi bir şeyi değiştirebilirsiniz.

### <a name="to-edit-your-custom-branding"></a>Özel markanızı düzenlemek için
1. Dizin için bir Genel yönetici hesabı kullanarak [Azure portalda](https://portal.azure.com/) oturum açın.

2. **Azure Active Directory**' yi seçin ve ardından **Şirket markası**' ni seçin ve ardından **Yapılandır**' ı seçin.

    ![Contoso-Şirket markası sayfası, varsayılan yapılandırma gösteriliyor](media/customize-branding/company-branding-default-config.png)

3. **Şirket markasını Yapılandır** sayfasında, bu MAKALENIN [Azure AD oturum açma sayfasını özelleştirme](#customize-your-azure-ad-sign-in-page) bölümünde yer alan açıklamalara göre bilgileri ekleyin, kaldırın veya değiştirin.

4. **Kaydet**’i seçin.

   Oturum açma sayfası markasında yaptığınız değişikliklerin görünmesi bir saate kadar sürebilir.

## <a name="add-language-specific-company-branding-to-your-directory"></a>Dizininize dile özgü şirket markası ekleme
Özgün yapılandırmanızın dilini varsayılan dilinizden değiştiremezsiniz. Ancak, farklı bir dilde bir yapılandırmaya ihtiyacınız varsa yeni bir yapılandırma oluşturabilirsiniz.

### <a name="to-add-a-language-specific-branding-configuration"></a>Dile özgü marka yapılandırması eklemek için

1. Dizin için bir Genel yönetici hesabı kullanarak [Azure portalda](https://portal.azure.com/) oturum açın.

2. **Azure Active Directory**' yi seçin ve ardından **Şirket markası**' ni seçin ve ardından **Yeni dil**' i seçin.

    ![Contoso-Şirket marka sayfası, yeni dil seçeneği vurgulanmış](media/customize-branding/company-branding-new-language.png)

3. **Şirket markasını Yapılandır** sayfasında, dilinizi (örneğin, Fransızca) seçin ve ardından bu MAKALENIN [Azure AD oturum açma sayfasını özelleştirme](#customize-your-azure-ad-sign-in-page) bölümündeki açıklamalara göre çevrilmiş bilgilerinizi ekleyin.

4. **Kaydet**’i seçin.

    **Contoso – Company marka** sayfası, yeni Fransızca yapılandırmanızı gösterecek şekilde güncelleştirilir.

    ![Contoso-Şirket markası sayfası, varsayılan yapılandırma gösteriliyor](media/customize-branding/company-branding-french-config.png)

## <a name="add-your-custom-branding-to-pages"></a>Sayfalarınıza özel markalarınızı ekleme
URL 'nin sonunu bir metinle değiştirerek özel markanızı sayfalara ekleyin `?whr=yourdomainname` . Bu değişiklik Multi-Factor Authentication (MFA) kurulum sayfası, self servis parola sıfırlama (SSPR) kurulum sayfası ve oturum açma sayfası dahil olmak üzere çeşitli sayfalarda çalışmaktadır.

**Örnekler**

**Özgün URL:**https://aka.ms/MFASetup<br>
**Özel URL:**`https://account.activedirectory.windowsazure.com/proofup.aspx?whr=contoso.com`

**Özgün URL:**https://aka.ms/SSPR<br>
**Özel URL:**`https://passwordreset.microsoftonline.com/?whr=contoso.com`
