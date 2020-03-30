---
title: 'Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için Yakınlaştırmayı yapılandırın | Microsoft Dokümanlar'
description: Azure AD'den Yakınlaştırma'ya kullanıcı hesaplarını otomatik olarak nasıl sağlayıp geçici olarak sağdatmayı öğrenin.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: na
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/3/2019
ms.author: Zhchia
ms.openlocfilehash: 94c261da0c935cb7a41dde768069099b4e5ed251
ms.sourcegitcommit: e040ab443f10e975954d41def759b1e9d96cdade
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2020
ms.locfileid: "80384084"
---
# <a name="tutorial-configure-zoom-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için Yakınlaştırmayı yapılandırın

Bu öğretici, otomatik kullanıcı sağlama yapılandırmak için hem Yakınlaştırma hem de Azure Etkin Dizin (Azure AD) işlemlerigerçekleştirmeniz gereken adımları açıklar. Azure AD, yapılandırıldığınızda, kullanıcıları ve grupları Azure REKLAM Sağlama hizmetini kullanarak [Yakınlaştırma'ya](https://zoom.us/pricing/) otomatik olarak hükümler ve hükümler denkiştir. Bu hizmetin ne yaptığı, nasıl çalıştığı ve sık sorulan sorular hakkında önemli ayrıntılar [için](../manage-apps/user-provisioning.md)bkz. 


## <a name="capabilities-supported"></a>Desteklenen yetenekler
> [!div class="checklist"]
> * Yakınlaştırma'da kullanıcı oluşturma
> * Artık erişim gerektirmediklerinde kullanıcıları Yakınlaştırma'da kaldırma
> * Azure AD ve Yakınlaştırma arasında kullanıcı özniteliklerini eşitlenmiş tutma
> * Zoom için [tek oturum](https://docs.microsoft.com/azure/active-directory/saas-apps/zoom-tutorial) açma (önerilir)

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdaki ön koşullara sahip olduğunuzu varsayar:

* [Azure AD kiracı.](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant)
* Sağlama yapılandırma [izniyle](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) Azure AD'deki bir kullanıcı hesabı (örn. Uygulama Yöneticisi, Bulut Uygulama yöneticisi, Uygulama Sahibi veya Genel Yönetici). 
* [Bir Zoom kiracı](https://zoom.us/pricing).
* Yönetici izinleriyle Yakınlaştırma'da bir kullanıcı hesabı.

## <a name="step-1-plan-your-provisioning-deployment"></a>1. Adım. Sağlama dağıtımınızı planlayın
1. Sağlama [hizmetinin nasıl çalıştığı](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning)hakkında bilgi edinin.
2. [Kimler in provizyon kapsamına](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)alınacağını belirleyin.
3. Azure AD ve Yakınlaştırma arasında hangi verilerin [eşlenere](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)verilen leri belirleyin. 

## <a name="step-2-configure-zoom-to-support-provisioning-with-azure-ad"></a>2. Adım Azure AD ile sağlamayı destekleyecek Yakınlaştırma'yı yapılandırın

1. Zoom Admin [Konsolunuzda](https://zoom.us/signin)oturum açın. Sol gezinti bölmesinde **Geliştiriciler için Gelişmiş > Yakınlaştırma'ya** gidin.

    ![Yakınlaştırma Tümleştirmeleri](media/zoom-provisioning-tutorial/zoom01.png)

2. Sayfanın sağ üst köşesinde **Yönet'e** gidin. 

    ![Yakınlaştırma Yükle](media/zoom-provisioning-tutorial/zoom02.png)

3. Oluşturduğunuz Azure AD uygulamasına gidin. 
    
    ![Yakınlaştırma Uygulaması](media/zoom-provisioning-tutorial/zoom03.png)

4. Sol gezinti bölmesinde **Uygulama Kimlik Bilgileri'ni** seçin.

    ![Yakınlaştırma Uygulaması](media/zoom-provisioning-tutorial/zoom04.png)

5. Kopyalayın ve **JWT Belirteci**kaydedin. Bu değer, Azure portalındaki Yakınlaştırma uygulamanızın Sağlama sekmesindeki **Gizli Belirteç** alanına girilir. Süresi dolmayan yeni bir belirteç gerekiyorsa, otomatik olarak yeni bir belirteç oluşturacak son kullanma süresini yeniden yapılandırmanız gerekir. 

    ![Yakınlaştırma Yükle](media/zoom-provisioning-tutorial/zoom05.png)

## <a name="step-3-add-zoom-from-the-azure-ad-application-gallery"></a>3. Adım Azure AD uygulama galerisinden Yakınlaştırma ekleme

Yakınlaştırma'ya sağlamayı yönetmeye başlamak için Azure AD uygulama galerisinden Yakınlaştırma'yı ekleyin. Daha önce SSO için Zoom kurulumu varsa, aynı uygulamayı kullanabilirsiniz. Ancak, başlangıçta tümleştirmeyi test ederken ayrı bir uygulama oluşturmanız önerilir. [Burada](https://docs.microsoft.com/azure/active-directory/manage-apps/add-gallery-app)galeriden bir uygulama ekleme hakkında daha fazla bilgi edinin. 

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>4. Adım. Tedarik kapsamına kimlerde olacağını tanımlama 

Azure AD sağlama hizmeti, uygulamaya yapılan atamaya ve kullanıcının/ grubun özniteliklerine göre kimin sağlanacak kapsamını kapsamanızı sağlar. Atamaya göre uygulamanız için kimlerin sağlanacak kapsamını seçerseniz, uygulamayı zedelektirler ve kullanıcıları ve grupları uygulamaya atamak için aşağıdaki [adımları](../manage-apps/assign-user-or-group-access-portal.md) kullanabilirsiniz. Yalnızca kullanıcı nın veya grubun özelliklerine göre kimlerin sağlanacak kapsamını seçerseniz, [burada](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)açıklandığı gibi bir kapsam filtresi kullanabilirsiniz. 

* Kullanıcıları ve grupları Yakınlaştırma'ya atarken, Varsayılan **Erişim**dışında bir rol seçmeniz gerekir. Varsayılan Erişim rolüne sahip kullanıcılar sağlama nın dışında tutulur ve sağlama günlüklerinde etkin bir şekilde hak sahibi olmadığı şeklinde işaretlenir. Uygulamada kullanılabilen tek rol varsayılan erişim rolüyse, ek roller eklemek için [uygulama bildirimini](https://docs.microsoft.com/azure/active-directory/develop/howto-add-app-roles-in-azure-ad-apps) güncelleştirebilirsiniz. 

* Küçük başla. Herkese kullanıma başlamadan önce küçük bir kullanıcı ve grup kümesiyle test edin. Sağlama kapsamı atanmış kullanıcılara ve gruplara ayarlandığında, uygulamaya bir veya iki kullanıcı veya grup atayarak bunu denetleyebilirsiniz. Kapsam tüm kullanıcılar ve gruplar için ayarlandığında, [öznitelik tabanlı kapsam filtresi](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)belirtebilirsiniz. 


## <a name="step-5-configure-automatic-user-provisioning-to-zoom"></a>5. Adım. Otomatik kullanıcı sağlamayı Yakınlaştırma olarak yapılandırın 

Bu bölüm, Azure AD'deki kullanıcı ve/veya grup atamalarına dayalı olarak TestApp'teki kullanıcıları ve/veya grupları oluşturmak, güncellemek ve devre dışı etmek için Azure AD sağlama hizmetini yapılandırma adımları boyunca size yol göstermektedir.

### <a name="to-configure-automatic-user-provisioning-for-zoom-in-azure-ad"></a>Azure AD'de Yakınlaştırma için otomatik kullanıcı sağlama yapılandırmak için:

1. [Azure portalında](https://portal.azure.com)oturum açın. **Kurumsal Uygulamaları**seçin, ardından **Tüm uygulamaları**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Yakınlaştırma'yı**seçin.

    ![Uygulamalar listesindeki Yakınlaştırma bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. Yönetici **Kimlik Bilgileri** bölümüne Kiracı `https://api.zoom.us/scim` **URL'sini**girin. **Gizli Belirteç'te**daha önce alınan **JWT Belirteci** değerini girdi. Azure AD'nin Yakınlaştırma'ya bağlanabilmesini sağlamak için **Test Bağlantısı'nı** tıklatın. Bağlantı başarısız olursa, Yakınlaştırma hesabınızda Yönetici izinleri olduğundan emin olun ve yeniden deneyin.

    ![Yakınlaştırma sağlama](./media/zoom-provisioning-tutorial/provisioning.png)

6. Bildirim **E-postası** alanında, sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve **bir hata olduğunda e-posta bildirimi gönder'i** seçin.

    ![Bildirim E-postası](common/provisioning-notification-email.png)

7. **Kaydet'i**seçin.

8. **Eşlemeler** bölümünde, **Yakınlaştırmak için Azure Etkin Dizin Kullanıcılarını Senkronize Et'i**seçin.

9. Azure AD'den **Öznitelik-Eşleme** bölümünde Yakınlaştırma'ya eşitlenen kullanıcı özniteliklerini gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Yakınlaştırma'daki kullanıcı hesaplarıyla eşleştirilmesi için kullanılır. [Eşleşen hedef özniteliği](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)değiştirmeyi seçerseniz, Yakınlaştırma API'sinin bu özniteliğe bağlı olarak kullanıcıları filtrelemeyi desteklediğinden emin olmanız gerekir. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

   |Öznitelik|Tür|
   |---|---|
   |userName|Dize|
   |Etkin|Boole|
   |name.givenName|Dize|
   |name.familyName|Dize|
   |e-postalar[eq "iş yazın"]|Dize|
   |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:department|Dize|

10. Kapsam filtrelerini yapılandırmak [için, Kapsam](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md)filtresi öğreticisinde sağlanan aşağıdaki yönergelere bakın.

11. Yakınlaştırma için Azure AD sağlama hizmetini etkinleştirmek **için, Ayarlar** bölümünde **Kisama Durumunu** **Ayarı** olarak değiştirin.

    ![Geçiş Yapılan Sağlama Durumu](common/provisioning-toggle-on.png)

12. **Ayarlar** bölümünde **Kapsam'ta** istenen değerleri seçerek Yakınlaştırma'ya sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Sağlama Kapsamı](common/provisioning-scope.png)

13. Hükmetmeye hazır olduğunuzda **Kaydet'i**tıklatın.

    ![Tasarruf Sağlama Yapılandırması](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **Kapsam'ta** tanımlanan tüm kullanıcıların ve grupların ilk eşitleme döngüsünü başlatır. Azure AD sağlama hizmeti nin çalıştırıldığı sürece yaklaşık her 40 dakikada bir gerçekleşen sonraki döngülere göre ilk çevrimin gerçekleşmesi daha uzun sürer. 

## <a name="step-6-monitor-your-deployment"></a>6. Adım. Dağıtımınızı izleme
Sağlamayı yapılandırdıktan sonra, dağıtımınızı izlemek için aşağıdaki kaynakları kullanın:

1. Hangi kullanıcıların başarılı veya başarısız bir şekilde sağlandığını belirlemek için [sağlama günlüklerini](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-provisioning-logs) kullanma
2. Sağlama döngüsünün durumunu ve tamamlanmasına ne kadar yakın olduğunu görmek için [ilerleme çubuğunu](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-when-will-provisioning-finish-specific-user) kontrol edin
3. Sağlama yapılandırması sağlıksız bir durumda gibi görünüyorsa, uygulama karantinaya alınır. Karantina durumları hakkında daha fazla bilgi [için burada.](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-quarantine-status)  

## <a name="connector-limitations"></a>Konektör sınırlamaları
* Zoom sadece 9.999 temel kullanıcı bugün maksimum sağlar.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı sağlamanın yönetimi](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri nasıl inceleyip sağlama etkinliği yle ilgili raporları nasıl alacağınızı öğrenin](../manage-apps/check-status-user-account-provisioning.md)
