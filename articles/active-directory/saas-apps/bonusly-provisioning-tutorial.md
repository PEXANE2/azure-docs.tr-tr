---
title: 'Öğretici: Azure Active Directory ile otomatik Kullanıcı sağlama için primi yapılandırma | Microsoft Docs'
description: Kullanıcı hesaplarını otomatik olarak sağlamak ve yeniden sağlamak üzere Azure Active Directory nasıl yapılandırılacağını öğrenin.
services: active-directory
author: zchia
writer: zchia
manager: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 03/27/2019
ms.author: jeedes
ms.openlocfilehash: d8c3f64e5cb5269bfe7e555615f874ac3443c6eb
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "94357836"
---
# <a name="tutorial-configure-bonusly-for-automatic-user-provisioning"></a>Öğretici: otomatik Kullanıcı sağlaması için priminin yapılandırın

Bu öğreticinin amacı, Azure AD 'yi, kullanıcıları ve/veya grupları otomatik olarak sağlamak ve devre dışı bırakmak üzere yapılandırmak için Bonor ve Azure Active Directory (Azure AD) içinde gerçekleştirilecek adımları göstermektir.

> [!NOTE]
> Bu öğreticide, Azure AD Kullanıcı sağlama hizmeti ' nin üzerine oluşturulmuş bir bağlayıcı açıklanmaktadır. Hizmetin işlevleri ve çalışma şekli hakkında daha fazla bilgi edinmek ve sık sorulan soruları incelemek için bkz. [Azure Active Directory ile SaaS uygulamalarına kullanıcı hazırlama ve kaldırma işlemlerini otomatik hale getirme](../app-provisioning/user-provisioning.md).

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen senaryo, zaten aşağıdakilere sahip olduğunuzu varsayar:

* Bir Azure AD kiracısı
* [Primi kiracı](https://bonus.ly/pricing)
* Yönetici izinlerine sahip olan bir kullanıcı hesabı

> [!NOTE]
> Azure AD sağlama tümleştirmesi, pribilen geliştiriciler tarafından kullanılabilen [primi REST API](https://konghq.com/solutions/gateway/)'sine dayanır.

## <a name="adding-bonusly-from-the-gallery"></a>Galeriden primi ekleme

Azure AD ile otomatik Kullanıcı sağlama için priliksiz yapılandırmadan önce Azure AD uygulama galerisindeki yönetilen SaaS uygulamaları listenize primi eklemeniz gerekir.

**Azure AD uygulama galerisinden primi eklemek için aşağıdaki adımları uygulayın:**

1. **[Azure Portal](https://portal.azure.com)** sol gezinti panelinde **Azure Active Directory** simgesine tıklayın.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar** seçeneğini belirleyin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için, iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesine tıklayın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **bonikisi** yazın, sonuç panelinden **primi** ' yi seçin, sonra da uygulamayı eklemek için düğme **Ekle** ' ye tıklayın.

    ![Sonuçlar listesinde](common/search-new-app.png)

## <a name="assigning-users-to-bonusly"></a>Kullanıcıları primi atama

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişim alacağını belirleyebilmek için "atamalar" adlı bir kavram kullanır. Otomatik Kullanıcı sağlama bağlamında, yalnızca Azure AD 'de bir uygulamaya "atanmış" olan kullanıcılar ve/veya gruplar eşitlenir. 

Otomatik Kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD 'deki hangi kullanıcıların ve/veya grupların Bonor 'a erişmesi gerektiğine karar vermeniz gerekir. Karar verdikten sonra buradaki yönergeleri izleyerek bu kullanıcıları ve/veya grupları primi atayabilirsiniz:

* [Kurumsal uygulamaya Kullanıcı veya Grup atama](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-bonusly"></a>Kullanıcıları Priye atamaya yönelik önemli ipuçları

* Otomatik Kullanıcı sağlama yapılandırmasını test etmek için tek bir Azure AD kullanıcısının Pride bir şekilde atanması önerilir. Ek kullanıcılar ve/veya grupları daha sonra atanabilir.

* Bir kullanıcıyı Pril 'e atarken atama iletişim kutusunda uygulamaya özgü geçerli herhangi bir rolü (varsa) seçmeniz gerekir. **Varsayılan erişim** rolüne sahip kullanıcılar, sağlanmasından çıkarılır.

## <a name="configuring-automatic-user-provisioning-to-bonusly"></a>Otomatik Kullanıcı sağlamayı primi olarak yapılandırma

Bu bölümde, Azure AD sağlama hizmeti 'ni kullanarak Kullanıcı ve/veya grupları Azure AD 'de Kullanıcı ve/veya Grup atamaları temelinde oluşturma, güncelleştirme ve devre dışı bırakma adımları adım adım kılavuzluk eder.

> [!TIP]
> Ayrıca, primi [Çoklu oturum açma öğreticisinde](bonus-tutorial.md)sunulan yönergeleri izleyerek, her bononun için SAML tabanlı çoklu oturum açmayı etkinleştirmeyi de tercih edebilirsiniz. Çoklu oturum açma, otomatik Kullanıcı sağlamasından bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini karmaşıdirebilirler.

### <a name="to-configure-automatic-user-provisioning-for-bonusly-in-azure-ad"></a>Azure AD 'de otomatik Kullanıcı sağlamasını yapılandırmak için:

1. [Azure Portal](https://portal.azure.com) oturum açın ve **Kurumsal uygulamalar**' ı seçin, **tüm uygulamalar**' ı seçin, sonra da **primi**' yi seçin

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde, **primi**' yi seçin.

    ![Uygulamalar listesindeki primi bağlantısı](common/all-applications.png)

3. **Hazırlama** sekmesini seçin.

    :::image type="content" source="./media/bonusly-provisioning-tutorial/ProvisioningTab.png" alt-text="Primi sağlama sekmesinin ekran görüntüsü. Yönet ' in altında sağlama vurgulanır." border="false":::

4. **Hazırlama Modu**'nu **Otomatik** olarak ayarlayın.

    :::image type="content" source="./media/bonusly-provisioning-tutorial/ProvisioningCredentials.png" alt-text="Otomatik olarak seçili ve vurgulanmış bir sağlama modu liste kutusu gösteren ekran görüntüsü." border="false":::

5. **Yönetici kimlik bilgileri** bölümünde, adım 6 ' da açıklandığı gibi, primi hesabınızın **gizli belirtecini** girin.

    :::image type="content" source="./media/bonusly-provisioning-tutorial/secrettoken.png" alt-text="Yönetici kimlik bilgileri bölümünün ekran görüntüsü. Gizli dizi belirteci kutusu boştur, ancak kutu vurgulanır." border="false":::

6. Primi hesabınız için **gizli anahtar** , **yönetici > şirket > tümleştirmelerle** bulunur. **Kod eklemek** istiyorsanız, yeni bir gizli belirteç oluşturmak için **API > yeni API erişim belirteci oluştur** ' a tıklayın.

    :::image type="content" source="./media/bonusly-provisioning-tutorial/BonuslyIntegrations.png" alt-text="Primi menüsünün ekran görüntüsü. Yönetici altında şirket vurgulanır. Şirket altında, tümleştirmeler vurgulanır." border="false":::

    :::image type="content" source="./media/bonusly-provisioning-tutorial/BonsulyRestApi.png" alt-text="Primi sitesinin vurgulandığı bir P vurgulanarak kod yapmak istiyorsanız, öğesinin ekran görüntüsü." border="false":::

    :::image type="content" source="./media/bonusly-provisioning-tutorial/CreateToken.png" alt-text="Primi sitesinin ekran görüntüsü. Hizmetler sekmesi açıktır. P 'm erişim Belirteçleriniz altında, yeni bir P. erişim belirteci oluştur vurgulanacaktır." border="false":::

7. Aşağıdaki ekranda, erişim belirteci için, girilen metin kutusuna bir ad yazın ve ardından **API anahtarı oluştur**' a basın. Yeni erişim belirteci açılan pencerede birkaç saniye görünür.

    :::image type="content" source="./media/bonusly-provisioning-tutorial/Token01.png" alt-text="Primi sitesinin yeni erişim belirteci sayfasının ekran görüntüsü. Etiketli olmayan bir kutu belirtemi içerir ve P + anahtar oluştur düğmesi vurgulanır." border="false":::

    :::image type="content" source="./media/bonusly-provisioning-tutorial/Token02.png" alt-text="Primi sitesinin ekran görüntüsü. Oluşturulan yeni erişim belirtecini görüntüleyen bir bildirim görünür ve ardından şifresi ayrılabilir bir belirteç gelir." border="false":::

8. 5. adımda gösterilen alanları doldurulmaya yönelik olarak, Azure AD 'nin primi 'ya bağlanabildiğinden emin olmak için **Bağlantıyı Sına** ' ya tıklayın. Bağlantı başarısız olursa, primi hesabınızın yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    :::image type="content" source="./media/bonusly-provisioning-tutorial/TestConnection.png" alt-text="Azure portal yönetici kimlik bilgileri bölümünün ekran görüntüsü. Metin bağlantısı düğmesi vurgulanır." border="false":::

9. **Bildirim e-postası** alanına, sağlama hatası bildirimlerini alması gereken kişinin veya grubun e-posta adresini girin ve **bir hata oluştuğunda e-posta bildirimi gönder** onay kutusunu işaretleyin.

    :::image type="content" source="./media/bonusly-provisioning-tutorial/EmailNotification.png" alt-text="Boş bir bildirim e-posta kutusunu gösteren ekran görüntüsü. Bir hata oluştuğunda e-posta bildirimi gönder etiketli bir seçenek görünür." border="false":::

10. **Kaydet**’e tıklayın.

11. **Eşlemeler** bölümünde, **kullanıcıları Azure Active Directory eşitler**' ı seçin.

    :::image type="content" source="./media/bonusly-provisioning-tutorial/UserMappings.png" alt-text="Eşlemeler bölümünün ekran görüntüsü. Ad ' ın altında, Azure Active Directory Kullanıcıları primi olarak eşitlemeniz vurgulanır." border="false":::

12. **Öznitelik eşleme** bölümünde Azure AD 'den pribir şekilde eşitlenen Kullanıcı özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemleri için primi 'daki Kullanıcı hesaplarını eşleştirmek için kullanılır. Değişiklikleri uygulamak için **Kaydet** düğmesini seçin.

    :::image type="content" source="./media/bonusly-provisioning-tutorial/UserAttributeMapping.png" alt-text="Öznitelik eşlemeleri sayfasının ekran görüntüsü. Bir tablo Azure Active Directory öznitelikleri, karşılık gelen primi özniteliklerini ve eşleşen durumu listeler." border="false":::

13. Kapsam belirleme filtrelerini yapılandırmak için [Kapsam belirleme filtresi öğreticisi](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md) ile sunulan yönergeleri izleyin.

14. Azure AD sağlama hizmetini primi için etkinleştirmek üzere **Ayarlar** bölümünde **sağlama durumunu** **Açık** olarak değiştirin.

    :::image type="content" source="./media/bonusly-provisioning-tutorial/ProvisioningStatus.png" alt-text="Ayarlar bölümünün ekran görüntüsü. Sağlama durumu geçişi kapalı olarak ayarlanır." border="false":::

15. **Ayarlar** bölümünde **kapsam** içinde istenen değerleri seçerek, sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    :::image type="content" source="./media/bonusly-provisioning-tutorial/ScopeSync.png" alt-text="Kapsam liste kutusunu gösteren ekran görüntüsü. Yalnızca atanan kullanıcıları ve grupları Eşitle kutusunda seçilir." border="false":::

16. Hazırlama işlemini başlatmak için **Kaydet**'e tıklayın.

    :::image type="content" source="./media/bonusly-provisioning-tutorial/SaveProvisioning.png" alt-text="Kaydet düğmesi vurgulanmış şekilde, primi sağlama sayfasının ekran görüntüsü." border="false":::

Bu işlem, **Ayarlar** bölümünde **kapsam** içinde tanımlanan tüm kullanıcılar ve/veya grupların ilk eşitlemesini başlatır. İlk eşitlemenin daha sonra, Azure AD sağlama hizmeti çalıştığı sürece yaklaşık 40 dakikada bir oluşan sonraki eşitlemeler yerine gerçekleştirilmesi daha uzun sürer. İlerleme durumunu izlemek için **eşitleme ayrıntıları** bölümünü kullanabilir ve Azure AD sağlama hizmeti tarafından gerçekleştirilen tüm işlemleri ayrıntılarıyla açıklayan etkinlik raporuna yönelik bağlantıları izleyebilirsiniz.

Azure AD sağlama günlüklerinin nasıl okunduğu hakkında daha fazla bilgi için bkz. [Otomatik Kullanıcı hesabı sağlamayı raporlama](../app-provisioning/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı hazırlamayı yönetme](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Hazırlama etkinliği günlüklerini incelemeyi ve rapor oluşturmayı öğrenin](../app-provisioning/check-status-user-account-provisioning.md)

<!--Image references-->
[1]: ./media/bonusly-provisioning-tutorial/tutorial_general_01.png
[2]: ./media/bonusly-provisioning-tutorial/tutorial_general_02.png
[3]: ./media/bonusly-provisioning-tutorial/tutorial_general_03.png