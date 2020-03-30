---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için Insight4GRC'yi yapılandırın | Microsoft Dokümanlar"
description: Azure AD'den Insight4GRC'ye kullanıcı hesaplarını otomatik olarak nasıl sağlayıp geçici olarak sağlayışınızı öğrenin.
services: active-directory
documentationcenter: ''
author: Zhchia
writer: Zhchia
manager: beatrizd
ms.assetid: d0eab8a0-571b-4609-96b1-bdbc761a25de
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/04/2020
ms.author: Zhchia
ms.openlocfilehash: 1404854e054c8fc4967ba863486969b8a87db526
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77621525"
---
# <a name="tutorial-configure-insight4grc-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için Insight4GRC'yi yapılandırın

Bu öğretici, otomatik kullanıcı sağlama yapılandırmak için hem Insight4GRC hem de Azure Active Directory (Azure AD) işlemlerinde gerçekleştirmeniz gereken adımları açıklar. Azure AD, yapılandırıldığınızda, Azure AD Sağlama hizmetini kullanarak kullanıcıları ve grupları [Insight4GRC'ye](https://www.rsmuk.com/) otomatik olarak hükümler ve hükümlerden arındırma sağlar. Bu hizmetin ne yaptığı, nasıl çalıştığı ve sık sorulan sorular hakkında önemli ayrıntılar [için](../manage-apps/user-provisioning.md)bkz. 


## <a name="capabilities-supported"></a>Desteklenen yetenekler
> [!div class="checklist"]
> * Insight4GRC'de kullanıcı oluşturma
> * Artık erişim gerektirmeyen kullanıcıları Insight4GRC'de kaldırın
> * Kullanıcı özniteliklerini Azure AD ve Insight4GRC arasında senkronize tutma
> * Insight4GRC'de sağlama grupları ve grup üyelikleri
> * Insight4GRC için [tek oturum açma](https://docs.microsoft.com/azure/active-directory/saas-apps/insight4grc-tutorial) (önerilir)

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdaki ön koşullara sahip olduğunuzu varsayar:

* [Azure AD kiracı](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant) 
* Sağlama yapılandırma [izniyle](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) Azure AD'deki bir kullanıcı hesabı (örn. Uygulama Yöneticisi, Bulut Uygulama yöneticisi, Uygulama Sahibi veya Genel Yönetici). 
* Yönetici izinleri olan Insight4GRC'de bir kullanıcı hesabı.

## <a name="step-1-plan-your-provisioning-deployment"></a>1. Adım. Sağlama dağıtımınızı planlayın
1. Sağlama [hizmetinin nasıl çalıştığı](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning)hakkında bilgi edinin.
2. [Kimler in provizyon kapsamına](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)alınacağını belirleyin.
3. Azure AD [ve Insight4GRC arasında hangi](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)verilerin eşlenemeye devam edin. 

## <a name="step-2-configure-insight4grc-to-support-provisioning-with-azure-ad"></a>2. Adım Insight4GRC'yi Azure AD ile sağlamayı destekleyecek şekilde yapılandırın

Azure AD ile otomatik kullanıcı sağlama için Insight4GRC'yi yapılandırmadan önce, Insight4GRC'de SCIM sağlamayı etkinleştirmeniz gerekir.

1. Taşıyıcı belirteci elde etmek için, son müşteri [destek ekibine](mailto:support.ss@rsmuk.com)başvurması gerekir.
2. SCIM uç nokta URL'sini elde etmek için, SCIM uç nokta URL'nizi oluşturmak için kullanılacağı için Insight4GRC alan adınızın hazır olması gerekir. Insight4GRC ile ilk yazılım satın alma işleminin bir parçası olarak Insight4GRC alan adınızı alabilirsiniz.

## <a name="step-3-add-insight4grc-from-the-azure-ad-application-gallery"></a>3. Adım Azure AD uygulama galerisinden Insight4GRC ekleyin

Insight4GRC'ye sağlama yı yönetmeye başlamak için Azure AD uygulama galerisinden Insight4GRC ekleyin. SSO için daha önce Insight4GRC kurulumu varsa, aynı uygulamayı kullanabilirsiniz. Ancak, başlangıçta tümleştirmeyi test ederken ayrı bir uygulama oluşturmanız önerilir. [Burada](https://docs.microsoft.com/azure/active-directory/manage-apps/add-gallery-app)galeriden bir uygulama ekleme hakkında daha fazla bilgi edinin. 

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>4. Adım. Tedarik kapsamına kimlerde olacağını tanımlama 

Azure AD sağlama hizmeti, uygulamaya yapılan atamaya ve kullanıcının/ grubun özniteliklerine göre kimin sağlanacak kapsamını kapsamanızı sağlar. Atamaya göre uygulamanız için kimlerin sağlanacak kapsamını seçerseniz, uygulamayı zedelektirler ve kullanıcıları ve grupları uygulamaya atamak için aşağıdaki [adımları](../manage-apps/assign-user-or-group-access-portal.md) kullanabilirsiniz. Yalnızca kullanıcı nın veya grubun özelliklerine göre kimlerin sağlanacak kapsamını seçerseniz, [burada](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)açıklandığı gibi bir kapsam filtresi kullanabilirsiniz. 

* Kullanıcıları ve grupları Insight4GRC'ye atarken, **Varsayılan Erişim**dışında bir rol seçmeniz gerekir. Varsayılan Erişim rolüne sahip kullanıcılar sağlama nın dışında tutulur ve sağlama günlüklerinde etkin bir şekilde hak sahibi olmadığı şeklinde işaretlenir. Uygulamada kullanılabilen tek rol varsayılan erişim rolüyse, ek roller eklemek için [uygulama bildirimini](https://docs.microsoft.com/azure/active-directory/develop/howto-add-app-roles-in-azure-ad-apps) güncelleştirebilirsiniz. 

* Küçük başla. Herkese kullanıma başlamadan önce küçük bir kullanıcı ve grup kümesiyle test edin. Sağlama kapsamı atanmış kullanıcılara ve gruplara ayarlandığında, uygulamaya bir veya iki kullanıcı veya grup atayarak bunu denetleyebilirsiniz. Kapsam tüm kullanıcılar ve gruplar için ayarlandığında, [öznitelik tabanlı kapsam filtresi](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)belirtebilirsiniz. 


## <a name="step-5-configure-automatic-user-provisioning-to-insight4grc"></a>5. Adım. Otomatik kullanıcı sağlamayı Insight4GRC olarak yapılandırın 

Bu bölüm, Azure AD'deki kullanıcı ve/veya grup atamalarına dayalı olarak TestApp'teki kullanıcıları ve/veya grupları oluşturmak, güncellemek ve devre dışı etmek için Azure AD sağlama hizmetini yapılandırma adımları boyunca size yol göstermektedir.

### <a name="to-configure-automatic-user-provisioning-for-insight4grc-in-azure-ad"></a>Azure AD'de Insight4GRC için otomatik kullanıcı sağlama yapılandırmak için:

1. [Azure portalında](https://portal.azure.com)oturum açın. **Kurumsal Uygulamaları**seçin, ardından **Tüm uygulamaları**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Insight4GRC'yi**seçin.

    ![Uygulamalar listesindeki Insight4GRC bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. Yönetici **Kimlik Bilgileri** bölümüne, **Kiracı URL'sinde**SCIM bitiş noktası URL'sini girdi. Enpoint URL, `https://<Insight4GRC Domain Name>.insight4grc.com/public/api/scim/v2 ` **Insight4GRC Alan Adı'nın** önceki adımlarda alınan değer olduğu biçimde olmalıdır. Daha önce **Gizli Belirteç'te**alınan taşıyıcı belirteci değerini girdi. Azure AD'nin Insight4GRC'ye bağlanabilmesini sağlamak için **Test Bağlantısı'nı** tıklatın. Bağlantı başarısız olursa, Insight4GRC hesabınızın Yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Sağlama](./media/insight4grc-provisioning-tutorial/provisioning.png)

6. Bildirim **E-postası** alanında, sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve **bir hata olduğunda e-posta bildirimi gönder'i** seçin.

    ![Bildirim E-postası](common/provisioning-notification-email.png)

7. **Kaydet'i**seçin.

8. **Eşlemeler** bölümünde, **Azure Etkin Dizin Kullanıcılarını Insight4GRC**ile Senkronize Et'i seçin.

9. Azure AD'den Insight4GRC'ye, **Öznitelik-Eşleme** bölümünde senkronize edilen kullanıcı özniteliklerini gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Insight4GRC'deki kullanıcı hesaplarıyla eşleştirilmesi için kullanılır. [Eşleşen hedef özniteliği](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)değiştirmeyi seçerseniz, Insight4GRC API'sinin bu özniteliğe göre kullanıcıları filtrelemeyi desteklediğinden emin olmanız gerekir. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

   |Öznitelik|Tür|
   |---|---|
   |userName|Dize|
   |externalId|Dize|
   |Etkin|Boole|
   |başlık|Dize|
   |name.givenName|Dize|
   |name.familyName|Dize|
   |e-postalar[yazın eq "iş"].value|Dize|
   |phoneNumbers[yazın eq "iş"].value|Dize|

10. **Eşlemeler** bölümünde, **Azure Etkin Dizin Gruplarını Insight4GRC'ye Senkronize Et'i**seçin.

11. Azure AD'den Insight4GRC'ye, **Öznitelik-Eşleme** bölümünde senkronize edilen grup özniteliklerini gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Insight4GRC'deki gruplarla eşleştirilmesi için kullanılır. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

      |Öznitelik|Tür|
      |---|---|
      |displayName|Dize|
      |externalId|Dize|
      |üyeler|Başvuru|

10. Kapsam filtrelerini yapılandırmak [için, Kapsam](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)filtresi öğreticisinde sağlanan aşağıdaki yönergelere bakın.

13. Insight4GRC için Azure AD sağlama hizmetini etkinleştirmek **için, Ayarlar** bölümünde **Sağlama Durumunu** **Ayarı** olarak değiştirin.

    ![Geçiş Yapılan Sağlama Durumu](common/provisioning-toggle-on.png)

14. **Ayarlar** bölümünde **Kapsam'ta** istenen değerleri seçerek Insight4GRC'ye sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Sağlama Kapsamı](common/provisioning-scope.png)

15. Hükmetmeye hazır olduğunuzda **Kaydet'i**tıklatın.

    ![Tasarruf Sağlama Yapılandırması](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **Kapsam'ta** tanımlanan tüm kullanıcıların ve grupların ilk eşitleme döngüsünü başlatır. Azure AD sağlama hizmeti nin çalıştırıldığı sürece yaklaşık her 40 dakikada bir gerçekleşen sonraki döngülere göre ilk çevrimin gerçekleşmesi daha uzun sürer. 

## <a name="step-6-monitor-your-deployment"></a>6. Adım. Dağıtımınızı izleme
Sağlamayı yapılandırdıktan sonra, dağıtımınızı izlemek için aşağıdaki kaynakları kullanın:

* Hangi kullanıcıların başarılı veya başarısız bir şekilde sağlandığını belirlemek için [sağlama günlüklerini](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-provisioning-logs) kullanın.
* Sağlama döngüsünün durumunu ve tamamlanmasına ne kadar yakın olduğunu görmek için [ilerleme çubuğunu](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-when-will-provisioning-finish-specific-user) denetleyin.
* Sağlama yapılandırması sağlıksız bir durumda gibi görünüyorsa, uygulama karantinaya alınır. Karantina durumları hakkında daha fazla bilgi [için burada.](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-quarantine-status)

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı sağlamayı yönetme.](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri nasıl inceleyip sağlama etkinliği yle ilgili raporlar alacağınızı öğrenin.](../app-provisioning/check-status-user-account-provisioning.md)
