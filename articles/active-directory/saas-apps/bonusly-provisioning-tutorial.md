---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için Bonus'u yapılandırın | Microsoft Dokümanlar"
description: Azure Active Directory'yi otomatik olarak sağlama ve kullanıcı hesaplarını Bonusly'ye sağlama ve ödemeden çıkarma için nasıl yapılandırılamayı öğrenin.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd-msft
ms.assetid: 879b0ee9-042a-441b-90a7-8c364d62426a
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7694e441a59680a9b9544d3479100c1f779964ff
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77058964"
---
# <a name="tutorial-configure-bonusly-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için Bonusly'yi yapılandırın

Bu öğreticinin amacı, Azure AD'yi kullanıcıları ve/veya grupları Bonusly'ye otomatik olarak sağlamak ve sağlamadan çıkarmak üzere yapılandırmak için Bonusly ve Azure Etkin Dizini'nde (Azure AD) gerçekleştirilecek adımları göstermektir.

> [!NOTE]
> Bu öğretici, Azure AD Kullanıcı Sağlama Hizmeti'nin üzerine inşa edilmiş bir bağlayıcıyı açıklar. Bu hizmetin ne yaptığı, nasıl çalıştığı ve sık sorulan sorular hakkında önemli ayrıntılar [için](../app-provisioning/user-provisioning.md)bkz.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, zaten aşağıdakilere sahip olduğunuzu varsayar:

* Azure AD kiracı
* Bonus [ly kiracı](https://bonus.ly/pricing)
* Yönetici izinleri ile Bonusly bir kullanıcı hesabı

> [!NOTE]
> Azure AD sağlama tümleştirmesi, Bonusly geliştiricileri tarafından kullanılabilen [Bonusly Rest API'sine](https://konghq.com/solutions/gateway/)dayanır.

## <a name="adding-bonusly-from-the-gallery"></a>Galeriden Bonusly ekleme

Azure AD ile otomatik kullanıcı sağlama için Bonus'u yapılandırmadan önce, Azure AD uygulama galerisinden yönetilen SaaS uygulamaları listenize Bonus olarak eklemeniz gerekir.

**Azure AD uygulama galerisinden Bonus eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Bonusly**yazın , sonuç panelinden **Bonusly'i** seçin ve uygulamayı eklemek için **Ekle** düğmesini tıklatın.

    ![Sonuç listesinde bonus olarak](common/search-new-app.png)

## <a name="assigning-users-to-bonusly"></a>Bonusly'ye kullanıcı atama

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişmesi gerektiğini belirlemek için "atamalar" adlı bir kavram kullanır. Otomatik kullanıcı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya "atanmış" kullanıcılar ve/veya gruplar eşitlenir. 

Otomatik kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki hangi kullanıcıların ve/veya grupların Bonusly'ye erişmesi gerektiğine karar vermelisiniz. Karar verildikten sonra, bu kullanıcıları ve/veya grupları bonus olarak buradaki talimatları izleyerek atayabilirsiniz:

* [Bir kurumsal uygulamaya kullanıcı veya grup atama](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-bonusly"></a>Bonusly'ye kullanıcı atamak için önemli ipuçları

* Otomatik kullanıcı sağlama yapılandırmasını sınamak için bonusa tek bir Azure AD kullanıcısıatanması önerilir. Ek kullanıcılar ve/veya gruplar daha sonra atanabilir.

* Bonusly'e bir kullanıcı atarken, atama iletişim kutusunda uygulamaya özgü geçerli bir rolü (varsa) seçmeniz gerekir. **Varsayılan Erişim** rolüne sahip kullanıcılar sağlama nın dışında tutulur.

## <a name="configuring-automatic-user-provisioning-to-bonusly"></a>Otomatik kullanıcı sağlamanın Bonusly olarak yapılandırılma

Bu bölüm, Azure AD'deki kullanıcı ve/veya grup atamalarını temel alarak Bonus'taki kullanıcıları ve/veya grupları oluşturmak, güncellemek ve devre dışı etmek için Azure AD sağlama hizmetini yapılandırma adımları boyunca size yol göstermektedir.

> [!TIP]
> Ayrıca Bonusly tek oturum açma [öğreticisinde](bonus-tutorial.md)verilen talimatları izleyerek Bonusly için SAML tabanlı tek oturum açma özelliğini etkinleştirmeyi de seçebilirsiniz. Tek oturum açma, otomatik kullanıcı sağlamadan bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini tamamlar.

### <a name="to-configure-automatic-user-provisioning-for-bonusly-in-azure-ad"></a>Azure AD'de Bonusly için otomatik kullanıcı sağlama yapılandırmak için:

1. [Azure portalında](https://portal.azure.com) oturum açın ve **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin, ardından **Bonusly'yi**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Başvuru listesinde **Bonusly'i**seçin.

    ![Uygulamalar listesindeki Bonus bağlantı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![İkramiye Sağlama](./media/bonusly-provisioning-tutorial/ProvisioningTab.png)

4. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

    ![İkramiye Sağlama](./media/bonusly-provisioning-tutorial/ProvisioningCredentials.png)

5. Yönetici **Kimlik Bilgileri** bölümüne, Adım 6'da açıklandığı gibi Bonus hesabınızın **Gizli Belirteci'ni** giriş leyin.

    ![İkramiye Sağlama](./media/bonusly-provisioning-tutorial/secrettoken.png)

6. Bonus hesabınız için **Gizli Belirteç,** **Yönetici > Şirketi > Entegrasyonları'nda**bulunmaktadır. Kod **oluşturmak istiyorsanız,** yeni bir Gizli Belirteç oluşturmak için **API > Yeni API Erişim Belirteci Oluştur'u** tıklatın.

    ![İkramiye Sağlama](./media/bonusly-provisioning-tutorial/BonuslyIntegrations.png)

    ![İkramiye Sağlama](./media/bonusly-provisioning-tutorial/BonsulyRestApi.png)

    ![İkramiye Sağlama](./media/bonusly-provisioning-tutorial/CreateToken.png)

7. Aşağıdaki ekranda, sağlanan metin kutusuna erişim belirteci için bir ad yazın ve ardından **Api Tuşu Oluştur'a**basın. Yeni erişim belirteci bir açılır pencerede birkaç saniye görünür.

    ![İkramiye Sağlama](./media/bonusly-provisioning-tutorial/Token01.png)

    ![İkramiye Sağlama](./media/bonusly-provisioning-tutorial/Token02.png)

8. Adım 5'te gösterilen alanları doldurarak, Azure AD'nin Bonusly'ye bağlanabilmesini sağlamak için **Test Bağlantısı'nı** tıklatın. Bağlantı başarısız olursa, Bonus hesabınızda Yönetici izinleri olduğundan emin olun ve yeniden deneyin.

    ![İkramiye Sağlama](./media/bonusly-provisioning-tutorial/TestConnection.png)

9. Bildirim **E-postası** alanında, sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve **bir hata oluştuğunda e-posta bildirimi gönder**onay kutusunu işaretleyin.

    ![İkramiye Sağlama](./media/bonusly-provisioning-tutorial/EmailNotification.png)

10. **Kaydet**'e tıklayın.

11. **Eşlemeler** bölümünde, **Azure Etkin Dizin Kullanıcılarını Bonus olarak senkronize et'i**seçin.

    ![İkramiye Sağlama](./media/bonusly-provisioning-tutorial/UserMappings.png)

12. Azure AD'den Bonus'a eşitlenen kullanıcı özniteliklerini **Öznitelik Eşleme** bölümünde gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Bonus'taki kullanıcı hesaplarıyla eşleştirilmesi için kullanılır. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

    ![İkramiye Sağlama](./media/bonusly-provisioning-tutorial/UserAttributeMapping.png)

13. Kapsam filtrelerini yapılandırmak [için, Kapsam](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)filtresi öğreticisinde sağlanan aşağıdaki yönergelere bakın.

14. Azure AD sağlama hizmetini Bonus için etkinleştirmek **için, Ayarlar** bölümünde **KiSama Durumunu** **Ayarı** olarak değiştirin.

    ![İkramiye Sağlama](./media/bonusly-provisioning-tutorial/ProvisioningStatus.png)

15. **Ayarlar** bölümünde **Kapsam'ta** istenen değerleri seçerek Bonus olarak sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![İkramiye Sağlama](./media/bonusly-provisioning-tutorial/ScopeSync.png)

16. Hükmetmeye hazır olduğunuzda **Kaydet'i**tıklatın.

    ![İkramiye Sağlama](./media/bonusly-provisioning-tutorial/SaveProvisioning.png)

Bu işlem, **Ayarlar** bölümünde **Kapsam'ta** tanımlanan tüm kullanıcıların ve/veya grupların ilk eşitlemisini başlatır. Azure AD sağlama hizmeti nin çalıştırıldığı sürece yaklaşık her 40 dakikada bir gerçekleşen sonraki eşitlemelerden daha uzun süren ilk eşitlemenin gerçeklemi daha uzun sürer. Azure AD sağlama hizmeti tarafından Bonus ly'de gerçekleştirilen tüm eylemleri açıklayan ilerlemeyi izlemek ve sağlama etkinlik raporuna bağlantılar izlemek için **Eşitleme Ayrıntıları** bölümünü kullanabilirsiniz.

Azure AD sağlama günlüklerini nasıl okuyabilirsiniz hakkında daha fazla bilgi için [bkz.](../app-provisioning/check-status-user-account-provisioning.md)

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı sağlamanın yönetimi](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri nasıl inceleyip sağlama etkinliği yle ilgili raporları nasıl alacağınızı öğrenin](../app-provisioning/check-status-user-account-provisioning.md)

<!--Image references-->
[1]: ./media/bonusly-provisioning-tutorial/tutorial_general_01.png
[2]: ./media/bonusly-provisioning-tutorial/tutorial_general_02.png
[3]: ./media/bonusly-provisioning-tutorial/tutorial_general_03.png
