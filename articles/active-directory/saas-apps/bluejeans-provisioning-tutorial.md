---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için BlueJeans'i yapılandırın | Microsoft Dokümanlar"
description: Azure Active Directory'yi bluejeans'e otomatik olarak sağlama ve kullanıcı hesaplarını sağlama ve sağlamadan çıkarma için nasıl yapılandırılamayı öğrenin.
services: active-directory
documentationcenter: ''
author: zhchia
writer: zhchia
manager: beatrizd-msft
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: fcb3fe009a6482c8e512899a952694beaed361a7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77059112"
---
# <a name="tutorial-configure-bluejeans-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için BlueJeans yapılandırın

Bu öğreticinin amacı, Azure AD'yi kullanıcıları ve/veya grupları BlueJeans'e otomatik olarak sağlamak ve sağlamadan çıkarmak üzere yapılandırmak için BlueJeans ve Azure Active Directory 'de (Azure AD) gerçekleştirilecek adımları göstermektir.

> [!NOTE]
> Bu öğretici, Azure AD Kullanıcı Sağlama Hizmeti'nin üzerine inşa edilmiş bir bağlayıcıyı açıklar. Bu hizmetin ne yaptığı, nasıl çalıştığı ve sık sorulan sorular hakkında önemli ayrıntılar [için](../app-provisioning/user-provisioning.md)bkz.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, zaten aşağıdakilere sahip olduğunuzu varsayar:

* Azure AD kiracı
* [Şirketim](https://www.BlueJeans.com/pricing) planı olan veya daha iyi etkin leştirilmiş bir BlueJeans kiracısı
* Yönetici izinleri olan BlueJeans'de bir kullanıcı hesabı

> [!NOTE]
> Azure AD sağlama tümleştirmesi, BlueJeans ekiplerinin Standart planda veya daha iyisi üzerinde kullanılabilen [BlueJeans API'sine](https://BlueJeans.github.io/developer)dayanır.

## <a name="adding-bluejeans-from-the-gallery"></a>Galeriden BlueJeans ekleme

Azure AD ile otomatik kullanıcı sağlama için BlueJeans'i yapılandırmadan önce, Azure AD uygulama galerisinden yönetilen SaaS uygulamaları listenize BlueJeans eklemeniz gerekir.

**Azure AD uygulama galerisinden BlueJeans eklemek için aşağıdaki adımları gerçekleştirin:**

1. Azure **[portalında,](https://portal.azure.com)** soldaki gezinti panelinde **Azure Etkin Dizin'i**seçin.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **uygulamalara**gidin ve ardından **Tüm uygulamaları**seçin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için bölmenin üst kısmındaki **Yeni uygulama** düğmesini seçin.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **BlueJeans**girin, sonuç panelinde **BlueJeans'i** seçin ve ardından uygulamayı eklemek için **Ekle** düğmesini seçin.

    ![Sonuç listesinde BlueJeans](common/search-new-app.png)

## <a name="assigning-users-to-bluejeans"></a>Kullanıcıları BlueJeans'e atama

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişmesi gerektiğini belirlemek için "atamalar" adlı bir kavram kullanır. Otomatik kullanıcı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya "atanmış" kullanıcılar ve/veya gruplar eşitlenir.

Otomatik kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki hangi kullanıcıların ve/veya grupların BlueJeans'e erişmesi gerektiğine karar vermelisiniz. Karar verildikten sonra, bu kullanıcıları ve/veya grupları burada talimatları izleyerek BlueJeans'e atayabilirsiniz:

* [Bir kurumsal uygulamaya kullanıcı veya grup atama](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-bluejeans"></a>BlueJeans'e kullanıcı atamak için önemli ipuçları

* Otomatik kullanıcı sağlama yapılandırmasını sınamak için BlueJeans'e tek bir Azure AD kullanıcısı atanması önerilir. Ek kullanıcılar ve/veya gruplar daha sonra atanabilir.

* Bir kullanıcıyı BlueJeans'e atarken, atama iletişim kutusunda uygulamaya özgü geçerli bir rolü (varsa) seçmeniz gerekir. **Varsayılan Erişim** rolüne sahip kullanıcılar sağlama nın dışında tutulur.

## <a name="configuring-automatic-user-provisioning-to-bluejeans"></a>BlueJeans için otomatik kullanıcı sağlama yapılandırma

Bu bölüm, Azure AD'deki kullanıcı ve/veya grup atamalarına dayalı olarak BlueJeans'deki kullanıcıları ve/veya grupları oluşturmak, güncellemek ve devre dışı etmek için Azure AD sağlama hizmetini yapılandırma adımları boyunca size yol göstermektedir.

> [!TIP]
> Ayrıca [BlueJeans tek oturum](bluejeans-tutorial.md)açma öğreticisinde verilen talimatları izleyerek BlueJeans için SAML tabanlı tek oturum açmayı etkinleştirmeyi de seçebilirsiniz. Tek oturum açma, otomatik kullanıcı sağlamadan bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini tamamlar.

### <a name="to-configure-automatic-user-provisioning-for-bluejeans-in-azure-ad"></a>Azure AD'de BlueJeans için otomatik kullanıcı sağlama yapılandırmak için:

1. [Azure portalında](https://portal.azure.com) oturum açın ve **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin, ardından **BlueJeans'i**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **BlueJeans'i**seçin.

    ![Uygulamalar listesindeki BlueJeans bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![BlueJeans Sağlama](./media/bluejeans-provisioning-tutorial/BluejeansProvisioningTab.png)

4. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

    ![BlueJeans Sağlama](./media/bluejeans-provisioning-tutorial/Bluejeans1.png)

5. Yönetici **Kimlik Bilgileri** bölümü ne göre, BlueJeans hesabınızın **Yönetici Kullanıcı Adı**ve Yönetici **Şifresi'ni** girin. Bu değerlere örnek olarak şunlar verilebilir:

   * Yönetici **Kullanıcı Adı** alanında, BlueJeans kiracınızda yönetici hesabının kullanıcı adını doldurun. Örnek: admin@contoso.com.

   * Yönetici **Parolası** alanında, yönetici kullanıcı adına karşılık gelen parolayı doldurun.

6. Adım 5'te gösterilen alanları doldurarak, Azure AD'nin BlueJeans'e bağlanabilmesini sağlamak için **Test Bağlantısı'nı** tıklatın. Bağlantı başarısız olursa, BlueJeans hesabınızın Yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![BlueJeans Sağlama](./media/bluejeans-provisioning-tutorial/BluejeansTestConnection.png)

7. Bildirim **E-postası** alanında, sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve onay kutusunu işaretleyin - **Bir hata oluştuğunda e-posta bildirimi gönderin.**

    ![BlueJeans Sağlama](./media/bluejeans-provisioning-tutorial/BluejeansNotificationEmail.png)

8. **Kaydet**'e tıklayın.

9. **Eşlemeler** bölümünde, **Azure Etkin Dizin Kullanıcılarını BlueJeans**ile Senkronize Et'i seçin.

    ![BlueJeans Sağlama](./media/bluejeans-provisioning-tutorial/BluejeansMapping.png)

10. **Öznitelik Eşleme** bölümünde Azure AD'den BlueJeans'e senkronize edilen kullanıcı özniteliklerini gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için BlueJeans'deki kullanıcı hesaplarıyla eşleştirilmesi için kullanılır. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

    ![BlueJeans Sağlama](./media/bluejeans-provisioning-tutorial/BluejeansUserMappingAtrributes.png)

11. Kapsam filtrelerini yapılandırmak [için, Kapsam](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)filtresi öğreticisinde sağlanan aşağıdaki yönergelere bakın.

12. BlueJeans için Azure AD sağlama hizmetini etkinleştirmek **için, Ayarlar** bölümünde **KiSama Durumunu** **Ayarı** olarak değiştirin.

    ![BlueJeans Sağlama](./media/bluejeans-provisioning-tutorial/BluejeansProvisioningStatus.png)

13. **Ayarlar** bölümünde **Kapsam'ta** istenen değerleri seçerek BlueJeans'e sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![BlueJeans Sağlama](./media/bluejeans-provisioning-tutorial/UserGroupSelection.png)

14. Hükmetmeye hazır olduğunuzda **Kaydet'i**tıklatın.

    ![BlueJeans Sağlama](./media/bluejeans-provisioning-tutorial/SaveProvisioning.png)

Bu işlem, **Ayarlar** bölümünde **Kapsam'ta** tanımlanan tüm kullanıcıların ve/veya grupların ilk eşitlemisini başlatır. Azure AD sağlama hizmeti nin çalıştırıldığı sürece yaklaşık her 40 dakikada bir gerçekleşen sonraki eşitlemelerden daha uzun süren ilk eşitlemenin gerçeklemi daha uzun sürer. Azure AD sağlama hizmeti tarafından BlueJeans'de gerçekleştirilen tüm eylemleri açıklayan ilerlemeyi izlemek ve sağlama etkinlik raporuna bağlantılar izlemek için **Eşitleme Ayrıntıları** bölümünü kullanabilirsiniz.

Azure AD sağlama günlüklerini nasıl okuyabilirsiniz hakkında daha fazla bilgi için [bkz.](../app-provisioning/check-status-user-account-provisioning.md)

## <a name="connector-limitations"></a>Bağlayıcı Sınırlamaları

* Bluejeans 30 karakteri aşan kullanıcı adlarına izin vermez.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı sağlamanın yönetimi](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri nasıl inceleyip sağlama etkinliği yle ilgili raporları nasıl alacağınızı öğrenin](../app-provisioning/check-status-user-account-provisioning.md)

<!--Image references-->

[1]: ./media/bluejeans-provisioning-tutorial/tutorial_general_01.png
[2]: ./media/bluejeans-tutorial/tutorial_general_02.png
[3]: ./media/bluejeans-tutorial/tutorial_general_03.png
