---
title: Azure AD uygulama galerisinden bir OpenID/OAuth uygulamasını yapılandırma | Microsoft Dokümanlar
description: Azure AD uygulama galerisinden bir OpenID/OAuth uygulamasını yapılandırma adımları.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: eedebb76-e78c-428f-9cf0-5891852e79fb
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/30/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: f8a2c962c69ead28c4e79b663010eab77a499f5c
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80048425"
---
# <a name="configure-an-openidoauth-application-from-the-azure-ad-app-gallery"></a>Azure AD uygulama galerisinden bir OpenID/OAuth uygulamasını yapılandırma

## <a name="process-of-adding-an-openid-application-from-the-gallery"></a>Galeriden OpenID uygulaması ekleme işlemi

1. Azure [portalında,](https://portal.azure.com)sol bölmede **Azure Etkin Dizini'ni**seçin. 

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png))

2. Kurumsal **uygulamalara** > git**Tüm uygulamalar**.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. İletişim kutusunun üst kısmında **Yeni uygulama'yı** seçin.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna uygulama adını yazın. Sonuç panelinden istediğiniz uygulamayı seçin ve uygulamaya kaydolun.

    ![Sonuç listesinde Openid](common/search-new-app.png)

    > [!NOTE]
    > OpenID Connect ve OAuth uygulamaları için **Ekle** düğmesi varsayılan olarak devre dışı bırakılır. Burada kiracı yönetici kayıt düğmesini seçmeli ve uygulamaya onay vermelidir. Uygulama daha sonra yapılandırmaları yapabileceğiniz müşteri kiracısına eklenir. Uygulamayı açıkça eklemenize gerek yoktur.

    ![Ekle düğmesi](./media/openidoauth-tutorial/addbutton.png)

5. Kaydolma bağlantısını seçtiğinizde, oturum açma kimlik bilgileri için Azure Etkin Dizin (Azure AD) sayfasına yönlendirilirsiniz.

6. Başarılı bir kimlik doğrulaması ndan sonra, onay sayfasından onayı kabul elabilirsiniz. Bundan sonra, uygulama giriş sayfası görüntülenir.

    > [!NOTE]
    > Uygulamanın yalnızca bir örneğini ekleyebilirsiniz. Zaten bir tane eklediyseniz ve onayı yeniden sağlamaya çalıştıysanız, kiracıya tekrar eklenmez. Yani mantıksal olarak, kiracıyalnızca bir uygulama örneği kullanabilirsiniz.

## <a name="authentication-flow-using-openid-connect"></a>OpenID Connect kullanarak kimlik doğrulama akışı

En temel oturum açma akışı aşağıdaki adımları içerir:

![OpenID Connect kullanarak kimlik doğrulama akışı](./media/openidoauth-tutorial/authenticationflow.png)

### <a name="multitenant-application"></a>Çok kiracılı uygulama 
Çok kiracılı bir uygulama, tek bir kuruluşta değil, birçok kuruluşta kullanılmak üzere tasarlanmıştır. Bunlar genellikle bağımsız bir yazılım satıcısı (ISV) tarafından yazılmış hizmet olarak yazılım (SaaS) uygulamalarıdır. 

Çok kiracılı uygulamaların kullanılacağı her dizinde sağlanması gerekir. Bunları kaydetmek için kullanıcı veya yöneticinin onayı gerekir. Bu onay işlemi, bir uygulama dizine kaydedildiğinde başlar ve Grafik API'sine veya belki de başka bir web API'sine erişim verilir. Uygulamayı kullanmak için farklı bir kuruluştan bir kullanıcı veya yönetici kaydolduğunda, bir iletişim kutusu uygulamanın ihtiyaç duyduğu izinleri görüntüler. 

Kullanıcı veya yönetici daha sonra uygulamayı onaylayabilir. Onay, başvuruya belirtilen verilere erişim sağlar ve son olarak uygulamayı dizine kaydeder.

> [!NOTE]
> Uygulamanızı birden çok dizindeki kullanıcıların kullanımına savuruyorsanız, hangi kiracıda olduklarını belirlemek için bir mekanizmaya ihtiyacınız vardır. Tek kiracılı bir uygulamanın yalnızca bir kullanıcı için kendi dizininde görünmesi gerekir. Çok kiracılı bir uygulamanın Azure AD'deki tüm dizinlerden belirli bir kullanıcıyı tanımlaması gerekir.
> 
> Bu görevi gerçekleştirmek için Azure AD, kiracıya özgü bir bitiş noktası yerine herhangi bir çok kiracılı uygulamanın oturum açma isteklerini yönlendirebileceği ortak bir kimlik doğrulama bitiş noktası sağlar. Bu bitiş `https://login.microsoftonline.com/common` noktası, Azure AD'deki tüm dizinler içindir. Kiracıya özgü bir bitiş `https://login.microsoftonline.com/contoso.onmicrosoft.com`noktası olabilir. 
>
> Uygulamanızı geliştirirken göz önünde bulundurulması gereken ortak bitiş noktası önemlidir. Oturum açma, imzalama ve belirteç doğrulaması sırasında birden çok kiracıyı işlemek için gerekli mantığa ihtiyacınız vardır.

Varsayılan olarak, Azure AD çok kiracılı uygulamaları teşvik eder. Kuruluşlar arasında kolayca erişilebiliyor ve siz onayı kabul ettikten sonra kullanımı kolaydır.

## <a name="consent-framework"></a>Onay çerçevesi

Çok kiracılı web ve yerel istemci uygulamaları geliştirmek için Azure AD onay çerçevesini kullanabilirsiniz. Bu uygulamalar, uygulamanın kayıtlı olduğu yerden farklı olarak, bir Azure AD kiracısının kullanıcı hesapları tarafından oturum açmalarına izin verir. Ayrıca şu gibi web API'lerine erişmeleri gerekebilir:
- Microsoft Graph API, Azure AD, Intune ve Office 365'teki hizmetlere erişmek için. 
- Diğer Microsoft hizmetlerinin API'leri.
- Kendi web API'leriniz. 

Çerçeve, dizinine kaydolmak isteyen bir uygulamaya onay veren bir kullanıcıya veya yöneticiye dayanır. Kayıt, dizin verilerine erişmeyi içerebilir. Onay verildikten sonra, istemci uygulaması kullanıcı adına Microsoft Graph API'yi arayabilir ve gerekli bilgileri kullanabilir.

[Microsoft Graph API,](https://developer.microsoft.com/graph/) Office 365'teki verilere aşağıdakigibi erişim sağlar:

- Exchange'den takvimler ve mesajlar.
- SharePoint'ten siteler ve listeler.
- OneDrive'dan belgeler.
- OneNote'un not defterleri.
- Planlayıcıdan görevler.
- Excel'den çalışma kitapları.

Grafik API ayrıca Azure AD'den ve diğer veri nesnelerinden kullanıcılara ve gruplara daha fazla Microsoft bulut hizmetinden erişim sağlar.

Aşağıdaki adımlar, uygulama geliştiricisi ve kullanıcısı için onay deneyiminin nasıl çalıştığını gösterir:

1. Bir kaynağa veya API'ye erişmek için belirli izinler istemesi gereken bir web istemcisi uygulamanız olduğunu varsayalım. Azure portalı, yapılandırma zamanında izin isteklerini bildirmek için kullanılır. Diğer yapılandırma ayarları gibi, uygulamanın Azure AD kayıtlarının bir parçası olurlar. İzin isteği yolu için aşağıdaki adımları izlemeniz gerekir:

    a. Menünün sol tarafındaki **Uygulama kayıtlarına** tıklayın ve arama kutusuna uygulama adını yazarak başvurunuzu açın.

    ![Graph API](./media/openidoauth-tutorial/application.png)

    b. **API İzinlerini Görüntüle'yi**tıklatın.

    ![Graph API](./media/openidoauth-tutorial/api-permission.png)

    c. İzin **Ekle'ye**tıklayın.

    ![Graph API](./media/openidoauth-tutorial/add-permission.png)

    d. Microsoft **Graph'a**tıklayın.

    ![Graph API](./media/openidoauth-tutorial/microsoft-graph.png)

    e. **Temsilcilik izni** ve Uygulama **İzinleri'nden**gerekli seçenekleri seçin.

    ![Graph API](./media/openidoauth-tutorial/graphapi.png)

2. Uygulamanızın izinlerinin güncelleştirildiğini düşünün. Uygulama çalışıyor ve bir kullanıcı ilk kez kullanmak üzere. Öncelikle uygulamanın Azure AD /authorize bitiş noktasından bir yetkilendirme kodu alması gerekir. Yetkilendirme kodu daha sonra yeni bir erişim elde etmek ve belirteci yenilemek için kullanılabilir.

3. Kullanıcının kimliği zaten doğrulanmazsa, Azure AD /authorize bitiş noktası oturum açma için istemde kalır.

    ![Kimlik doğrulaması](./media/openidoauth-tutorial/authentication.png)

4. Kullanıcı oturum açsın sonra, Azure AD kullanıcıya bir onay sayfası gösterilmesi gerekip gerekmediğini belirler. Bu belirleme, kullanıcının (veya kuruluşun yöneticisinin) uygulama iznini zaten verip vermediğini temel eder.

   İzin verilmediyse, Azure AD kullanıcıdan izin ister ve çalışması için gereken izinleri görüntüler. İzin iletişim kutusunda görüntülenen izinler, Azure portalında temsilci izinlerinde seçilenlerle eşleşir.

    ![İzin sayfası](./media/openidoauth-tutorial/consentpage.png)

Normal bir kullanıcı bazı izinleri onaylayabilir. Diğer izinler için kiracı yöneticinin onayı gerekir.

## <a name="difference-between-admin-consent-and-user-consent"></a>Yönetici onayı ile kullanıcı onayı arasındaki fark

Yönetici olarak, kiracınızdaki tüm kullanıcılar adına bir uygulamanın temsilciizinlerini de kabul edebilirsiniz. İdari onay, kiracıdaki her kullanıcı için onay iletişim kutusunun görünmesini engeller. Yönetici rolüne sahip kullanıcılar Azure portalında onay sağlayabilir. Uygulamanızın **Ayarlar** sayfasından **Gerekli İzinleri** > Hibe admin**onayı'nı**seçin.

![İzinleri Ver düğmesi](./media/openidoauth-tutorial/grantpermission.png)

> [!NOTE]
> ADAL.js kullanan tek sayfalı uygulamalar (SPA'lar) için **Grant admin onay** düğmesini kullanarak açık onay verilmesi artık gereklidir. Aksi takdirde, erişim belirteci istendiğinde uygulama başarısız olur.

Yalnızca uygulama izinleri her zaman kiracı yöneticinin onayGerektirir. Uygulamanız yalnızca uygulama izni isterse ve bir kullanıcı uygulamada oturum açmaya çalışırsa, bir hata iletisi görüntülenir. İleti, kullanıcının onay veremediğini söylüyor.

Uygulamanız yönetici onayı gerektiren izinler kullanıyorsa, yöneticinin eylemi başlatabileceği bir düğme veya bağlantı gibi bir hareket iniz gerekir. Başvurunuzun bu eylem için gönderdiği istek, her zamanki OAuth2/OpenID Connect yetkilendirme isteğidir. Bu istek *istemi=admin_consent* sorgu dize parametresini içerir. 

Yönetici nin onayı alındıktan ve hizmet yöneticisi müşterinin kiracısında oluşturulduktan sonra, daha sonra oturum açma *isteklerinin istem=admin_consent* parametresi gerekmez. Yönetici istenen izinlerin kabul edilebilir olduğuna karar verdiğinden, kiracıdaki diğer kullanıcılardan bu noktadan sonra onay istenmez.

Kiracı yönetici, normal kullanıcıların uygulamalara onay verme yeteneğini devre dışı kılabilir. Bu özellik devre dışı bırakılırsa, uygulamanın kiracıda kullanılması için her zaman yönetici onayı gerekir. Son kullanıcı onayı devre dışı bırakılmış olarak uygulamanızı test etmek istiyorsanız, yapılandırma anahtarını [Azure portalında](https://portal.azure.com/)bulabilirsiniz. Kurumsal uygulamalar altında [Kullanıcı ayarları](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/UserSettings/menuId/) **bölümünde.**

*Komut istemi=admin_consent* parametresi, yönetici onayı gerektirmeyen izinleri isteyen uygulamalar tarafından da kullanılabilir. Buna örnek olarak, kiracı yöneticinin bir kez "kaydolduğu" ve bu noktadan itibaren başka hiçbir kullanıcıdan izin istenmediği bir deneyim gerektiren bir uygulama örnektir.

Bir uygulamanın yönetici onayı gerektirdiğini ve bir yöneticinin *istem=admin_consent* parametresi gönderilmeden imzaladığını düşünün. Yönetici uygulamayı başarıyla kabul ettiğinde, bu yalnızca kullanıcı hesabı için geçerlidir. Normal kullanıcılar yine de oturum açamaz veya uygulamayı onaylayamaz. Bu özellik, kiracı yöneticiye diğer kullanıcıların erişimine izin vermeden önce uygulamanızı keşfetme olanağı vermek istiyorsanız yararlıdır.
