---
title: Öğretici - Azure AD Connect bulut sağlamayı kullanarak varolan bir ormanı ve yeni bir ormanı tek bir Azure AD kiracısıyla tümleştirin.
description: Öğretici.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: overview
ms.date: 12/05/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: aa612ad30ae0faa42071613be15c1d91fb96b8f6
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80332275"
---
# <a name="integrate-an-existing-forest-and-a-new-forest-with-a-single-azure-ad-tenant"></a>Mevcut bir ormanı ve yeni bir ormanı tek bir Azure AD kiracısı ile tümleştirme

Bu öğretici, varolan bir karma kimlik ortamına bulut sağlama ekleme konusunda size yol kateder. 

![Oluşturma](media/tutorial-existing-forest/existing-forest-new-forest.png)

Bu öğreticide oluşturduğunuz ortamı test etmek veya karma kimliğin nasıl çalıştığını daha iyi tanımak için kullanabilirsiniz. 

Bu senaryoda, Azure AD Connect eşitlemi kullanılarak azure AD kiracısına eşitlenen varolan bir orman vardır. Ayrıca, aynı Azure AD kiracısına eşitlemek istediğiniz yeni bir ormana sahipsiniz. Yeni orman için bulut sağlama yı kuracaksın. 

## <a name="prerequisites"></a>Ön koşullar
### <a name="in-the-azure-active-directory-admin-center"></a>Azure Etkin Dizin yönetici merkezinde

1. Azure AD kiracınızda yalnızca buluta özel bir global yönetici hesabı oluşturun. Bu şekilde, şirket içi hizmetleriniz başarısız olursa veya kullanılamıyorsa kiracınızın yapılandırmasını yönetebilirsiniz. Yalnızca [buluta özel genel yönetici hesabı ekleme](../active-directory-users-create-azure-portal.md)hakkında bilgi edinin. Bu adımı tamamlamak, kiracınızın kilitsiz kaldığınızdan emin olmak için çok önemlidir.
2. Azure AD kiracınıza bir veya daha fazla [özel alan adı](../active-directory-domains-add-azure-portal.md) ekleyin. Kullanıcılarınız bu alan adlarından biriyle oturum açabilir.

### <a name="in-your-on-premises-environment"></a>Şirket içi ortamınızda

1. En az 4 GB RAM ve .NET 4.7.1+ çalışma süresiyle Windows Server 2012 R2 veya daha fazla çalıştıran etki alanı birleştirilmiş ana bilgisayar sunucusunu kimlikle 

2. Sunucularınız ile Azure AD arasında bir güvenlik duvarı varsa, aşağıdaki öğeleri yapılandırın:
   - Aracıların Aşağıdaki bağlantı noktaları üzerinden Azure AD'ye *giden* isteklerde bulunabilmesini sağlayın:

     | Bağlantı noktası numarası | Nasıl kullanılır? |
     | --- | --- |
     | **80** | TLS/SSL sertifikasını doğrularken sertifika iptal listelerini (CRLs) karşıdan yükler |
     | **443** | Hizmetle ilgili tüm giden iletişimi işler |
     | **8080** (isteğe bağlı) | Aracılar, bağlantı noktası 443 kullanılamıyorsa, 8080 portu üzerinden her 10 dakikada bir durumlarını bildirir. Bu durum Azure AD portalında görüntülenir. |
     
     Güvenlik duvarınız kuralları oluşturan kullanıcılara göre zorluyorsa, bu bağlantı noktalarını ağ hizmeti olarak çalışan Windows hizmetlerinden gelen trafiğe açın.
   - Güvenlik duvarınız veya proxy'niz güvenli sonekler belirtmenize izin ** \*veriyorsa, 0,msappproxy.net** ve ** \*.servicebus.windows.net**bağlantılarını ekleyin. Değilse, haftalık olarak güncelleştirilen [Azure veri merkezi IP aralıklarına](https://www.microsoft.com/download/details.aspx?id=41653)erişin.
   - Acentelerinizin ilk kayıt için **login.windows.net** ve **login.microsoftonline.com** erişimi ne kadar gerekir. Bu URL'ler için de güvenlik duvarınızı açın.
   - Sertifika doğrulama için aşağıdaki URL'lerin engelini kaldırın: **mscrl.microsoft.com:80,** **crl.microsoft.com:80,** **ocsp.msocsp.com:80**ve **\.www microsoft.com:80.** Bu URL'ler diğer Microsoft ürünleriyle sertifika doğrulama için kullanıldığından, bu URL'lerin engelini kaldırmış olabilirsiniz.

## <a name="install-the-azure-ad-connect-provisioning-agent"></a>Azure AD Connect sağlama aracısını yükleme
1. Birleştirilmiş etki alanında oturum açın.  [Temel AD ve Azure ortamı](tutorial-basic-ad-azure.md) öğreticisini kullanıyorsanız, bu DURUM DC1 olacaktır.
2. Yalnızca buluta özel genel yönetici kimlik bilgilerini kullanarak Azure portalında oturum açın.
3. Solda Azure **Etkin Dizin'i**seçin, **Azure AD Bağlantısı'nı** tıklatın ve merkezde **sağlamayı yönet 'i (önizleme)** seçin.</br>
![Azure portalda](media/how-to-install/install6.png)</br>
4. "Download agent" üzerine tıklayın
5. Azure AD Connect sağlama aracısını çalıştırma
6. Sıçrama ekranında, lisans koşullarını **kabul edin** ve **Yükle'yi**tıklatın.</br>
![Hoş Geldiniz ekranı](media/how-to-install/install1.png)</br>

7. Bu işlem tamamlandığında yapılandırma sihirbazı başlatılacaktır.  Azure AD global yönetici hesabınızla oturum açın.  IE gelişmiş güvenlik etkin varsa bu oturum açma engelleyecek unutmayın.  Bu durumda, yüklemeyi kapatın, Sunucu Yöneticisi'nde IE gelişmiş güvenliği devre dışı kaldırın ve yüklemeyi yeniden başlatmak için **AAD Connect Provisioning Agent Wizard'ı** tıklatın.
8. Etkin **Dizin Ekle** ekranında **Dizini Ekle'yi** tıklatın ve ardından Active Directory etki alanı yöneticihesabınızla oturum açın.  NOT: Etki alanı yöneticisi hesabının parola değiştirme gereksinimleri olmamalıdır. Parolanın süresinin dolması veya değiştirilmesi durumunda, aracıyı yeni kimlik bilgileriyle yeniden yapılandırmanız gerekir. Bu işlem şirket içi dizininizi de ekleyecektir.  **İleri**'ye tıklayın.</br>
![Hoş Geldiniz ekranı](media/how-to-install/install3.png)</br>

9. Yapılandırma **tam** ekranında, **Onayla'yı**tıklatın.  Bu işlem aracıyı kaydeder ve yeniden başlatacaktır.</br>
![Hoş Geldiniz ekranı](media/how-to-install/install4.png)</br>

10. Bu işlem tamamlandıktan sonra bir uyarı görmeniz gerekir: **Aracı yapılandırmanız başarıyla doğrulandı.**  **Çıkış'ı**tıklatabilirsiniz.</br>
![Hoş Geldiniz ekranı](media/how-to-install/install5.png)</br>
11. İlk sıçrama ekranını hala **görüyorsanız, Kapat'ı**tıklatın.


## <a name="verify-agent-installation"></a>Aracı yüklemeyi doğrula
Aracı doğrulaması Azure portalında ve aracıyı çalıştıran yerel sunucuda gerçekleşir.

### <a name="azure-portal-agent-verification"></a>Azure portal aracısı doğrulaması
Aracının Azure tarafından görüldüğünü doğrulamak için aşağıdaki adımları izleyin:

1. Azure Portal’da oturum açın.
2. Solda Azure **Etkin Dizin'i**seçin, **Azure AD Bağlantısı'nı** tıklatın ve merkezde **sağlamayı yönet 'i (önizleme)** seçin.</br>
![Azure portalda](media/how-to-install/install6.png)</br>

3.  Azure **AD Sağlama (önizleme)** ekranında **tüm aracıları gözden geçir'i**tıklatın.
![Azure REKLAM Sağlama](media/how-to-install/install7.png)</br>
 
4. Şirket **içi sağlama aracıları ekranında** yüklediğiniz aracıları göreceksiniz.  Söz konusu aracının orada olduğunu ve **etkin**olarak işaretlendiğini doğrulayın.
![Sağlama ajanları](media/how-to-install/verify1.png)</br>

### <a name="on-the-local-server"></a>Yerel sunucuda
Aracının çalıştığını doğrulamak için aşağıdaki adımları izleyin:

1.  Yönetici hesabıyla sunucuda oturum açma
2.  **Hizmetleri** başlatarak veya Başlat/Çalıştır/Services.msc'ye giderek açın.
3.  **Hizmetler**altında, **Microsoft Azure AD Connect Agent Updater** ve **Microsoft Azure AD Connect Provisioning Agent'ın** bulunduğundan ve durumun **çalışmadığından**emin olun.
![Hizmetler](media/how-to-troubleshoot/troubleshoot1.png)

## <a name="configure-azure-ad-connect-cloud-provisioning"></a>Azure AD Connect bulut sağlama yı yapılandırma
 Sağlamayı yapılandırmak için aşağıdaki adımları kullanın

1.  Azure AD portalında oturum açın.
2.  **Azure Etkin Dizini'ni** tıklatın
3.  **Azure AD Bağlantısı'nı** tıklatın
4.  **Hükmü Yönet'i seçin (Önizleme)**
![](media/how-to-configure/manage1.png)
5.  **Yeni Yapılandırma'yı** tıklatın
![](media/tutorial-single-forest/configure1.png)
7.  Yapılandırma ekranında, bir **Bildirim e-postası**girin, seçiciyi **Etkinleştir'e** taşıyın ve **Kaydet'e**tıklayın.
![](media/tutorial-single-forest/configure2.png)
1.  Yapılandırma durumu artık **Sağlıklı**olmalıdır.
![](media/how-to-configure/manage4.png)

## <a name="verify-users-are-created-and-synchronization-is-occurring"></a>Kullanıcıların oluşturulduğunu ve eşitlemenin oluştuğunu doğrulayın
Artık şirket içi dizinimizde bulunan kullanıcıların senkronize edildiğini ve artık Azure AD kiracımızda bulunduğunu doğrulayaceksiniz.  Bunun tamamlanmasının birkaç saat sürebileceğini unutmayın.  Kullanıcıların senkronize olduğunu doğrulamak için aşağıdakileri yapın.


1. [Azure portalına](https://portal.azure.com) gidip Azure aboneliği olan bir hesapla oturum açın.
2. Solda Azure **Etkin Dizini'ni** seçin
3. **Yönet** bölümünde **Kullanıcılar**’ı seçin.
4. Kiracımızda yeni kullanıcıları gördüğünüzden doğrulama</br>
![Synch](media/tutorial-single-forest/synchronize1.png)</br>

## <a name="test-signing-in-with-one-of-our-users"></a>Kullanıcılarımızdan biriyle oturum açma testini test edin

1. Göz atın[https://myapps.microsoft.com](https://myapps.microsoft.com)
2. Yeni kiracımızda oluşturulan bir kullanıcı hesabıyla oturum açın.  Aşağıdaki biçimi kullanarak oturum açmanız gerekir: (user@domain.onmicrosoft.com). Kullanıcının şirket içinde oturum açmada kullandığı parolayı kullanın.</br>
   ![Doğrulama](media/tutorial-single-forest/verify1.png)</br>

Azure'un sunduklarını sınamak ve tanımak için kullanabileceğiniz karma bir kimlik ortamını başarıyla ayarladınız.

## <a name="next-steps"></a>Sonraki adımlar 

- [Sağlama nedir?](what-is-provisioning.md)
- [Azure AD Connect bulut sağlama nedir?](what-is-cloud-provisioning.md)
