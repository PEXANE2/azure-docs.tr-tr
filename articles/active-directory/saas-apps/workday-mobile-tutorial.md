---
title: 'Öğretici: Workday mobil uygulamasıyla çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ile Workday mobil uygulaması arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 08/31/2020
ms.author: jeedes
ms.openlocfilehash: 256da169761da486d8ac064a2f58a59be43bb5df
ms.sourcegitcommit: 8c7f47cc301ca07e7901d95b5fb81f08e6577550
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2020
ms.locfileid: "92754736"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-workday-mobile-application"></a>Öğretici: Workday mobil uygulamasıyla çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory

Bu öğreticide, Azure Active Directory (Azure AD), koşullu erişim ve Intune 'U Workday Mobile Apps ile tümleştirmeyi öğreneceksiniz. Workday 'nin Mobile Apps Microsoft ile tümleştirdiğinizde şunları yapabilirsiniz:

* Oturum açmak için önce cihazların ilkelerinizle uyumlu olduğundan emin olun.
* Kullanıcıların şirket verilerine güvenli bir şekilde erişmesini sağlamak için Workday 'in uygulamasına denetimler ekleyin. 
* Azure AD 'de Workday 'e erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Workday 'e otomatik olarak oturum açmalarına olanak sağlayın.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Workday 'yi Azure AD ile tümleştirme
* Öğretici: [Workday ile çoklu oturum açma (SSO) tümleştirmesi Azure Active Directory](https://docs.microsoft.com/azure/active-directory/saas-apps/workday-tutorial)

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Microsoft 'un koşullu erişim Ilkelerini ve Intune 'U Workday 'nin mobil uygulamalarıyla yapılandırıp test edersiniz.

* Workday Federasyon uygulaması, artık SSO 'yu etkinleştirmek için Azure AD ile yapılandırılabilir. ' Nin nasıl yapılandırılacağı hakkında daha fazla bilgi için lütfen [Bu](workday-tutorial.md) bağlantıyı izleyin.

> [!NOTE] 
> Workday, Intune 'un uygulama koruma Ilkelerini desteklemez. Koşullu erişimi kullanmak için mobil cihaz yönetimi kullanmanız gerekir.


## <a name="ensure-users-have-access-to-the-workday-mobile-app"></a>Kullanıcıların Workday mobil uygulamasına erişimi olduğundan emin olun:

Workday 'yi, mobil uygulama tekliflerinden erişime izin verecek şekilde yapılandırın. Mobil için aşağıdaki ilkeleri yapılandırmanız gerekir:

Bu yönergeleri izleyerek şunları yapılandırabilirsiniz:

1. Işlevsel alan raporu için etki alanı güvenlik Ilkelerine erişin.
2. Bir güvenlik ilkesi seçin.
    * Mobil kullanım-Android
    * Mobil kullanım-iPad
    * Mobil kullanım-iPhone
3. Izinleri Düzenle ' ye tıklayın.
4. Güvenlik gruplarına rapor veya görev güvenliği sağlanabilir öğelerine erişim izni vermek için görünüm veya değiştirme onay kutusunu seçin.
5. Güvenlik gruplarına tümleştirme ve rapor veya görev güvenliği sağlanabilir eylemlerine erişim izni vermek için Al veya koy onay kutusunu seçin.

Bekleyen **Güvenlik Ilkesi değişikliklerini etkinleştir** görevini çalıştırarak, bekleyen güvenlik ilkesi değişikliklerini etkinleştirin.

## <a name="open-workday-login-page-in-mobile-browser"></a>Workday oturum açma sayfasını mobil tarayıcıda aç:

Workday 'in mobil uygulamasına koşullu erişim uygulamak için, uygulamanın bir dış tarayıcıda açılması gerekir. Bu işlem, **kiracı kurulumunu Düzenle-güvenlik** bölümünde **yerel uygulamalar için mobil tarayıcı SSO 'su etkinleştir** onay kutusunu işaretleyerek yapılabilir. Bu, iOS için cihazda ve Android için Iş profilinde Intune onaylı bir tarayıcının yüklü olmasını gerektirir.

![Mobil tarayıcı oturumu açma](./media/workday-tutorial/mobile-browser.png)

## <a name="setup-conditional-access-policy"></a>Koşullu erişim Ilkesi kurulumu:

Bu ilke, yalnızca bir iOS veya Android cihazında oturum açmayı etkiler. Tüm platformlara genişletmek istiyorsanız, yalnızca **herhangi bir cihaz** seçmeniz yeterlidir. Bu ilke, cihazın ilkeyle uyumlu olmasını gerektirir ve bunu Microsoft Intune aracılığıyla doğrular. Android Iş profillerinin bulunduğu için, bu, Iş profilinde oturum açtıklarında ve uygulamayı Intune Şirket Portalı aracılığıyla yüklemedikleri takdirde, tüm kullanıcıların Workday 'de (Web veya uygulama) oturum açmasını engellemelidir. Aynı durumun aynı durumda olduğundan emin olmak için iOS için bir ek adım vardır. Koşullu erişim kurulumunun bazı ekran görüntüleri aşağıda verilmiştir.

**Workday aşağıdaki erişim denetimlerini destekler:**
* Multi-Factor Authentication gerektir
* Cihazın uyumlu olarak işaretlenmesini gerektir

**Workday uygulaması aşağıdakileri desteklemez:**
* Onaylanan istemci uygulaması gerektir
* Uygulama koruma ilkesi gerektir (Önizleme)

**Workday** 'Yi **yönetilen cihaz** olarak ayarlamak için aşağıdaki adımları gerçekleştirin:

![Koşullu erişim Ilkesini ayarla](./media/workday-tutorial/managed-devices-only.png)

1. **> Microsoft Intune > koşullu Access-Policies yalnızca yönetilen cihazları >** 

1. **Yalnızca yönetilen cihazlar** sayfasında, **ad** alanı değerini olarak verin `Managed Devices Only` ve **bulut uygulamaları veya eylemleri** ' ne tıklayın.

1. **Bulut uygulamaları veya eylemleri** için aşağıdaki adımları gerçekleştirin.

    a. Bu ilkenin **bulut uygulamaları** olarak **ne uygulanacağını seçin** .

    b. Ekle ' de, **Uygulama Seç** ' e tıklayın.

    c. Seçim listesinden **Workday** ' i seçin.

    d. **Bitti** ’ye tıklayın.

1. **Ilkeyi etkinleştir** ' i açın.

1. **Kaydet** ’e tıklayın.

Erişim **verme** için aşağıdaki adımları gerçekleştirin:

![Workday kurulumu koşullu erişim Ilkesi](./media/workday-tutorial/managed-devices-only-2.png)

1. **> Microsoft Intune > koşullu Access-Policies yalnızca yönetilen cihazları >** 

1. **Yalnızca yönetilen cihazlar** sayfasında, **ad** alanı değerini olarak verin `Managed Devices Only` ve **izin ver > erişim denetimleri** ' ne tıklayın.

1. **Verme** sayfasında aşağıdaki adımları gerçekleştirin.

    a. **Erişim izni** olarak uygulanacak denetimleri seçin.

    b. **Cihazın uyumlu olarak Işaretlenmesini gerektir** kutusunu işaretleyin.

    c. **Seçili denetimlerden birini gerektir** ' i seçin.

    d. **Seç** ' e tıklayın.

1. **Ilkeyi etkinleştir** ' i açın.

1. **Kaydet** ’e tıklayın

## <a name="set-up-device-compliance-policy"></a>Cihaz uyumluluk Ilkesini ayarlama:

İOS cihazlarının yalnızca MDM tarafından yönetilen bir Workday uygulaması aracılığıyla oturum açabildiğinden emin olmak için, kısıtlanmış uygulamalar listesine **com. Workday. workdayapp** ekleyerek App Store uygulamasını engellemeniz gerekir. Bu, yalnızca şirket portalı aracılığıyla iş günü uygulamasına yüklenmiş cihazların Workday 'e erişebildiğinden emin olur. Tarayıcı için yalnızca cihaz Intune tarafından yönetilmiyorsa ve yönetilen bir tarayıcı kullanıyorsa Workday 'ye erişebilecekler.

![Workday kurulumu cihaz uyumluluk Ilkesi](./media/workday-tutorial/ios-policy.png)

## <a name="set-up-microsoft-intune-app-configuration-policies"></a>Microsoft Intune uygulama yapılandırma Ilkelerini ayarlama:

| Senaryo | Anahtar değer çiftleri |
|----------------------------------------------------------------------------------------   |-----------|
| Kiracı ve Web adresi alanlarını otomatik olarak doldur:<br>Android for Work profillerini etkinleştirdiğinizde Android üzerinde ● Workday.<br>İPad ve iPhone üzerinde ● Workday.     | Kiracınızı yapılandırmak için şu değerleri kullanın: <br>● Yapılandırma anahtarı = UserGroupCode<br>● Değer türü = dize <br>● Yapılandırma değeri = kiracı adınız. Örnek: GMS<br>Web adresinizi yapılandırmak için şu değerleri kullanın:<br>● Yapılandırma anahtarı = AppServiceHost<br>● Değer türü = dize<br>● Yapılandırma değeri = kiracınızın temel URL 'SI. Örnek: https://www.myworkday.com                              |   |
| İPad ve iPhone üzerinde Workday için bu eylemleri devre dışı bırakın:<br>● Kes, Kopyala ve Yapıştır<br>● Yazdır                       | İşlevselliği devre dışı bırakmak için bu anahtarlarda değeri (Boolean) false olarak ayarlayın:<br>● AllowCutCopyPaste<br>● AllowPrint  |
| Android 'de Workday için ekran görüntülerini devre dışı bırakın. |İşlevselliği devre dışı bırakmak için Allowekran anahtarındaki değeri (Boolean) false olarak ayarlayın.|
| Kullanıcılarınız için önerilen güncelleştirmeleri devre dışı bırakın.|İşlevselliği devre dışı bırakmak için Allowmülatedupdates anahtarındaki değeri (Boolean) false olarak ayarlayın.|
|Mobil kullanıcıları tercih ettiğiniz uygulama deposuna yönlendirmek için uygulama mağazası URL 'sini özelleştirin.|Uygulama Mağazası URL 'sini değiştirmek için şu değerleri kullanın:<br>● Yapılandırma anahtarı = AppUpdateURL<br>● Değer türü = dize<br> ● Yapılandırma değeri = App Store URL 'SI|
|       |


## <a name="ios-configuration-policies"></a>iOS yapılandırma Ilkeleri:

1. Git https://portal.azure.com/ ve oturum aç
2. **Intune 'u** arayın veya listeden pencere öğesine tıklayın.
3. **Istemci uygulamaları-> uygulamalar-> uygulama yapılandırma ilkeleri-> + > yönetilen cihazlar** ' a gidin
4. Bir ad girin.
5. **Platform** altında **IOS/ıpados** ' ı seçin.
6. **Ilişkili uygulama** altında, eklediğiniz iOS uygulaması için Workday 'yi seçin
7. **Yapılandırma ayarları** ' na tıklayın ve **yapılandırma ayarları biçimi** altında **XML verisi gir** ' i seçin
8. Örnek bir XML dosyası aşağıda verilmiştir. Uygulamak istediğiniz konfigürasyonları ekleyin. **STRING_VALUE** , kullanmak istediğiniz dize ile değiştirin. Veya `<true />` `<false />` ile değiştirin `<true />`  `<false />` . Bir yapılandırma eklememezseniz, doğru olarak ayarlandığı gibi çalışır.

    ```
    <dict>
    <key>UserGroupCode</key>
    <string>STRING_VALUE</string>
    <key>AppServiceHost</key>
    <string>STRING_VALUE</string>
    <key>AllowCutCopyPaste</key>
    <true /> or <false />
    <key>AllowPrint</key>
    <true /> or <false />
    <key>AllowSuggestedUpdates</key>
    <true /> or <false />
    <key>AppUpdateURL</key>
    <string>STRING_VALUE</string>
    </dict>

    ```
9. Ekle'ye tıklayın.
10. Sayfayı yenileyin ve yeni oluşturulan ilkeye tıklayın.
11. Atamalar ' a tıklayın ve uygulamanın uygulanmasını istediğinizi seçin.
12. Kaydet’e tıklayın.

## <a name="android-configuration-policies"></a>Android yapılandırma Ilkeleri:

1. `https://portal.azure.com/` adresine gidin ve oturum açın.
2. **Intune 'u** arayın veya listeden pencere öğesine tıklayın.
3. **Istemci uygulamaları-> uygulamalar-> uygulama yapılandırma ilkeleri-> + > yönetilen cihazlar** ' a gidin
5. Bir ad girin. 
6. **Platform** altında **Android** ' i seçin.
7. **Ilişkili uygulama** altında, eklediğiniz Android uygulaması için Workday 'yi seçin
8. **Yapılandırma ayarları** ' na tıklayın ve **yapılandırma ayarları biçimi** altında **JSON verisi gir** ' i seçin

