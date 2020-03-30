---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için Zscaler Beta'yı yapılandırın | Microsoft Dokümanlar"
description: Azure Active Directory'yi zscaler Beta'ya otomatik olarak sağlama ve kullanıcı hesaplarını sağlama ve azaltma işlemlerini yapmak üzere nasıl yapılandırılayarılamayı öğrenin.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd-msft
ms.assetid: 83db6b8d-503b-48f3-b918-f9fba1369d53
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2019
ms.author: jeedes
ms.openlocfilehash: 505cd7a3350c937885f0f03268ef326a4f784258
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77062761"
---
# <a name="tutorial-configure-zscaler-beta-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için Zscaler Beta'yı yapılandırın

Bu öğreticinin amacı, Azure AD'yi zscaler Beta'ya ve/veya grupları Zscaler Beta'ya otomatik olarak sağlamak ve sağlamadan çıkarmak üzere yapılandırmak için Zscaler Beta ve Azure Active Directory 'de (Azure AD) gerçekleştirilecek adımları göstermektir.

> [!NOTE]
> Bu öğretici, Azure AD Kullanıcı Sağlama Hizmeti'nin üzerine inşa edilmiş bir bağlayıcıyı açıklar. Bu hizmetin ne yaptığı, nasıl çalıştığı ve sık sorulan sorular hakkında önemli ayrıntılar [için](../active-directory-saas-app-provisioning.md)bkz.
>


## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, zaten aşağıdakilere sahip olduğunuzu varsayar:

* Azure AD kiracı
* Bir Zscaler Beta kiracı
* Yönetici izinleri olan Zscaler Beta'da bir kullanıcı hesabı

> [!NOTE]
> Azure AD sağlama tümleştirmesi, Kurumsal pakete sahip hesaplar için Zscaler Beta geliştiricilerin kullanabileceği Zscaler Beta SCIM API'sine dayanır.

## <a name="adding-zscaler-beta-from-the-gallery"></a>Galeriden Zscaler Beta ekleme

Azure AD ile otomatik kullanıcı sağlama için Zscaler Beta'yı yapılandırmadan önce, Azure AD uygulama galerisinden Zscaler Beta'yı yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Azure AD uygulama galerisinden Zscaler Beta eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda, **Zscaler Beta**yazın, sonuç panelinden **Zscaler Beta'yı** seçin ve uygulamayı eklemek için **Ekle** düğmesini tıklatın.

    ![Sonuç listesinde Zscaler Beta](common/search-new-app.png)

## <a name="assigning-users-to-zscaler-beta"></a>Kullanıcıları Zscaler Beta'ya atama

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişmesi gerektiğini belirlemek için "atamalar" adlı bir kavram kullanır. Otomatik kullanıcı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya "atanmış" kullanıcılar ve/veya gruplar eşitlenir.

Otomatik kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki hangi kullanıcıların ve/veya grupların Zscaler Beta'ya erişmesi gerektiğine karar vermelisiniz. Karar verildikten sonra, bu kullanıcıları ve/veya grupları buradaki talimatları izleyerek Zscaler Beta'ya atayabilirsiniz:

* [Bir kurumsal uygulamaya kullanıcı veya grup atama](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-zscaler-beta"></a>Kullanıcıları Zscaler Beta'ya atamak için önemli ipuçları

* Otomatik kullanıcı sağlama yapılandırmasını sınamak için zscaler Beta'ya tek bir Azure AD kullanıcısı atanması önerilir. Ek kullanıcılar ve/veya gruplar daha sonra atanabilir.

* Bir kullanıcıyı Zscaler Beta'ya atarken, atama iletişim kutusunda uygulamaya özgü geçerli bir rolü (varsa) seçmeniz gerekir. **Varsayılan Erişim** rolüne sahip kullanıcılar sağlama nın dışında tutulur.

## <a name="configuring-automatic-user-provisioning-to-zscaler-beta"></a>Otomatik kullanıcı sağlamanın Zscaler Beta'ya yapılandırılması

Bu bölüm, Azure AD'deki kullanıcı ve/veya grup atamalarına dayalı olarak Zscaler Beta'daki kullanıcıları ve/veya grupları oluşturmak, güncellemek ve devre dışı etmek için Azure AD sağlama hizmetini yapılandırma adımları boyunca size yol göstermektedir.

> [!TIP]
> Ayrıca Zscaler Beta tek oturum açma [öğreticisinde](zscaler-beta-tutorial.md)verilen talimatları izleyerek Zscaler Beta için SAML tabanlı tek oturum açmayı etkinleştirmeyi de seçebilirsiniz. Tek oturum açma, otomatik kullanıcı sağlamadan bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini tamamlar.

### <a name="to-configure-automatic-user-provisioning-for-zscaler-beta-in-azure-ad"></a>Azure AD'de Zscaler Beta için otomatik kullanıcı sağlama yapılandırmak için:

1. [Azure portalında](https://portal.azure.com) oturum açın ve **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin, ardından **Zscaler Beta'yı**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Zscaler Beta'yı**seçin.

    ![Uygulamalar listesindeki Zscaler Beta bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Zscaler Beta Sağlama](./media/zscaler-beta-provisioning-tutorial/provisioning-tab.png)

4. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

    ![Zscaler Beta Sağlama](./media/zscaler-beta-provisioning-tutorial/provisioning-credentials.png)

5. Yönetici **Kimlik Bilgileri** bölümü ne şrbölümünde, Adım 6'da açıklandığı şekilde Zscaler Beta hesabınızın **Kiracı URL'sini** ve **Gizli Jetonunu** girdiniz.

6. **Kiracı URL'sini** ve **Gizli Jetonu**almak için Zscaler Beta portal kullanıcı arabirimindeki **Yönetim > Kimlik Doğrulama Ayarları'na** gidin ve Kimlik **Doğrulama Türü**altında **SAML'yi** tıklatın.

    ![Zscaler Beta Sağlama](./media/zscaler-beta-provisioning-tutorial/secret-token-1.png)

    **Yapılandırma SAML** seçeneklerini açmak için **Yapıya SAML'yi** tıklatın.

    ![Zscaler Beta Sağlama](./media/zscaler-beta-provisioning-tutorial/secret-token-2.png)

    **Temel URL** ve **Taşıyıcı Belirteci'ni**almak için **SCIM Tabanlı Sağlama'yı etkinleştir'i** seçin ve ardından ayarları kaydedin. Temel **URL'yi** **Kiracı URL'sine**ve **Taşıyıcı Belirteci'ni** Azure portalında **Gizli Belirteç'e** kopyalayın.

7. Adım 5'te gösterilen alanları doldurarak, Azure AD'nin Zscaler Beta'ya bağlanabilmesini sağlamak için **Test Bağlantısı'nı** tıklatın. Bağlantı başarısız olursa, Zscaler Beta hesabınızın Yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Zscaler Beta Sağlama](./media/zscaler-beta-provisioning-tutorial/test-connection.png)

8. Bildirim **E-postası** alanında, sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve **bir hata oluştuğunda e-posta bildirimi gönder**onay kutusunu işaretleyin.

    ![Zscaler Beta Sağlama](./media/zscaler-beta-provisioning-tutorial/notification.png)

9. **Kaydet**'e tıklayın.

10. **Eşlemeler** bölümünde, **Azure Etkin Dizin Kullanıcılarını Zscaler Beta**ile Senkronize Et'i seçin.

    ![Zscaler Beta Sağlama](./media/zscaler-beta-provisioning-tutorial/user-mappings.png)

11. Azure AD'den Zscaler Beta'ya senkronize edilen kullanıcı özniteliklerini **Öznitelik Eşleme** bölümünde gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Zscaler Beta'daki kullanıcı hesaplarıyla eşleştirilmesi için kullanılır. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

    ![Zscaler Beta Sağlama](./media/zscaler-beta-provisioning-tutorial/user-attribute-mappings.png)

12. **Eşlemeler** bölümünde, **Azure Etkin Dizin Gruplarını Zscaler Beta'ya Senkronize Et'i**seçin.

    ![Zscaler Beta Sağlama](./media/zscaler-beta-provisioning-tutorial/group-mappings.png)

13. Azure AD'den Zscaler Beta'ya eşitlenen grup özniteliklerini **Öznitelik Eşleme** bölümünde gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Zscaler Beta'daki gruplarla eşleştirilmesi için kullanılır. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

    ![Zscaler Beta Sağlama](./media/zscaler-beta-provisioning-tutorial/group-attribute-mappings.png)

14. Kapsam filtrelerini yapılandırmak [için, Kapsam](./../active-directory-saas-scoping-filters.md)filtresi öğreticisinde sağlanan aşağıdaki yönergelere bakın.

15. Zscaler Beta için Azure AD sağlama hizmetini etkinleştirmek **için, Ayarlar** bölümünde **Sağlama Durumunu** **Ayarı** olarak değiştirin.

    ![Zscaler Beta Sağlama](./media/zscaler-beta-provisioning-tutorial/provisioning-status.png)

16. **Ayarlar** bölümünde **Kapsam'ta** istenen değerleri seçerek Zscaler Beta'ya sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Zscaler Beta Sağlama](./media/zscaler-beta-provisioning-tutorial/scoping.png)

17. Hükmetmeye hazır olduğunuzda **Kaydet'i**tıklatın.

    ![Zscaler Beta Sağlama](./media/zscaler-beta-provisioning-tutorial/save-provisioning.png)

Bu işlem, **Ayarlar** bölümünde **Kapsam'ta** tanımlanan tüm kullanıcıların ve/veya grupların ilk eşitlemisini başlatır. Azure AD sağlama hizmeti nin çalıştırıldığı sürece yaklaşık her 40 dakikada bir gerçekleşen sonraki eşitlemelerden daha uzun süren ilk eşitlemenin gerçeklemi daha uzun sürer. Zscaler Beta'daki Azure AD sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklayan ilerlemeyi izlemek ve sağlama etkinlik raporuna olan bağlantıları izlemek için **Eşitleme Ayrıntıları** bölümünü kullanabilirsiniz.

Azure AD sağlama günlüklerini nasıl okuyabilirsiniz hakkında daha fazla bilgi için [bkz.](../active-directory-saas-provisioning-reporting.md)

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı sağlamanın yönetimi](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri nasıl inceleyip sağlama etkinliği yle ilgili raporları nasıl alacağınızı öğrenin](../active-directory-saas-provisioning-reporting.md)

<!--Image references-->
[1]: ./media/zscaler-beta-provisioning-tutorial/tutorial-general-01.png
[2]: ./media/zscaler-beta-provisioning-tutorial/tutorial-general-02.png
[3]: ./media/zscaler-beta-provisioning-tutorial/tutorial-general-03.png
