---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için Zscaler Two'yı yapılandırın | Microsoft Dokümanlar"
description: Bu eğitimde, Azure Active Directory'yi zscaler Two'ya kullanıcı hesaplarını otomatik olarak sağlamak ve yok etmek için nasıl yapılandıracağınız öğrenilir.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd-msft
ms.assetid: 0a250fcd-6ca1-47c2-a780-7a6278186a69
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/27/2019
ms.author: jeedes
ms.openlocfilehash: 364b106e7c1f01269ac02b0c2851f8824ea0f58c
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77062704"
---
# <a name="tutorial-configure-zscaler-two-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için Zscaler İki'yi yapılandırın

Bu eğitimde, kullanıcıları ve/veya grupları Zscaler Two'ya otomatik olarak sağlamak ve devre dışı etmek için Azure Active Directory'yi (Azure AD) nasıl yapılandırabileceğinizi öğreneceksiniz.

> [!NOTE]
> Bu öğretici, Azure AD kullanıcı sağlama hizmetinde yerleşik bir bağlayıcıyı açıklar. Bu hizmetin ne yaptığı ve nasıl çalıştığı hakkında önemli ayrıntılar ve sık sorulan soruların yanıtları için [bkz.](../active-directory-saas-app-provisioning.md)

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen adımları tamamlamak için aşağıdakileri yapmanız gerekir:

* Azure AD kiracısı.
* Bir Zscaler İki kiracı.
* Yönetici izinleri olan Zscaler Two'da bir kullanıcı hesabı.

> [!NOTE]
> Azure AD sağlama tümleştirmesi, Kurumsal hesaplar için kullanılabilen Zscaler Two SCIM API'ye dayanır.

## <a name="add-zscaler-two-from-the-gallery"></a>Galeriden Zscaler İki ekle

Azure AD ile otomatik kullanıcı sağlama için Zscaler Two'yu yapılandırmadan önce, Zscaler Two'yu Azure AD uygulama galerisinden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

Azure [portalında](https://portal.azure.com), sol bölmede, **Azure Etkin Dizini'ni**seçin:

![Azure Active Directory'yi seçin](common/select-azuread.png)

Kurumsal **uygulamalara** gidin ve ardından **Tüm uygulamaları**seçin:

![Kurumsal uygulamalar](common/enterprise-applications.png)

Uygulama eklemek için pencerenin üst kısmında **Yeni uygulama'yı** seçin:

![Yeni uygulama seçin](common/add-new-app.png)

Arama kutusuna **Zscaler İki'yi**girin. Sonuçlarda **Zscaler Two'yu** seçin ve sonra **Ekle'yi**seçin.

![Sonuç listesi](common/search-new-app.png)

## <a name="assign-users-to-zscaler-two"></a>Kullanıcıları Zscaler Two'ya atama

Azure AD kullanıcılarının kullanabilmeleri için seçili uygulamalara erişim atanması gerekir. Otomatik kullanıcı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya atanan kullanıcılar veya gruplar eşitlenir.

Otomatik kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki hangi kullanıcıların ve/veya grupların Zscaler Two'ya erişmesi gerektiğine karar vermelisiniz. Buna karar verdikten sonra, [bir kullanıcıyı veya grubu kurumsal bir uygulamaya atama](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)yönergelerini izleyerek bu kullanıcıları ve grupları Zscaler Two'ya atayabilirsiniz.

### <a name="important-tips-for-assigning-users-to-zscaler-two"></a>Zscaler Two'ya kullanıcı atamak için önemli ipuçları

* Otomatik kullanıcı sağlama yapılandırmasını test etmek için önce Zscaler Two'ya tek bir Azure AD kullanıcısı atamanızı öneririz. Daha sonra daha fazla kullanıcı ve grup atayabilirsiniz.

* Bir kullanıcıyı Zscaler Two'ya atadığınızda, atama iletişim kutusunda geçerli bir uygulamaya özgü rolü (varsa) seçmeniz gerekir. **Varsayılan Erişim** rolüne sahip kullanıcılar sağlama nın dışında tutulur.

## <a name="set-up-automatic-user-provisioning"></a>Otomatik kullanıcı sağlama

Bu bölüm, Azure AD'deki kullanıcı ve grup atamalarına dayalı olarak Zscaler Two'daki kullanıcıları ve grupları oluşturmak, güncelleştirmek ve devre dışı etmek için Azure AD sağlama hizmetini yapılandırma adımları boyunca size yol göstermektedir.

> [!TIP]
> Zscaler Two için SAML tabanlı tek oturum açma'yı da etkinleştirmek isteyebilirsiniz. Bunu yaparsanız, [Zscaler İki tek oturum açma öğretici](zscaler-two-tutorial.md)yönergeleri izleyin. Tek oturum açma, otomatik kullanıcı sağlamadan bağımsız olarak yapılandırılabilir, ancak iki özellik birbirini tamamlar.

1. [Azure portalında](https://portal.azure.com) oturum açın ve **Kurumsal uygulamaları** > seçin**Tüm uygulamalar** > **Zscaler İki**:

    ![Kurumsal uygulamalar](common/enterprise-applications.png)

2. Uygulamalar listesinde **Zscaler Two'yu**seçin:

    ![Başvuru listesi](common/all-applications.png)

3. **Sağlama** sekmesini seçin:

    ![Zscaler İki Sağlama](./media/zscaler-two-provisioning-tutorial/provisioning-tab.png)

4. Sağlama **Modunu** **Otomatik**Olarak Ayarlayın:

    ![Sağlama Modunu Ayarlama](./media/zscaler-two-provisioning-tutorial/provisioning-credentials.png)

5. Yönetici **Kimlik Bilgileri** bölümünde, bir sonraki adımda açıklandığı gibi Zscaler Two hesabınızın **Kiracı URL'sini** ve **Gizli Belirteci'ni** girin.

6. **Kiracı URL'sini** ve **Gizli Jetonu**almak için Zscaler Two portalındaki **Yönetim** > **Kimlik Doğrulama Ayarları'na** gidin ve Kimlik **Doğrulama Türü**altında **SAML'yi** seçin:

    ![Zscaler İki Kimlik Doğrulama Ayarı](./media/zscaler-two-provisioning-tutorial/secret-token-1.png)

    **YapılSAML** penceresini açmak için **Yapılandırılan SAML'yi** seçin:

    ![SAML pencereyi yapılandırma](./media/zscaler-two-provisioning-tutorial/secret-token-2.png)

    **SCIM Tabanlı Sağlama'yı etkinleştir'i** seçin ve **Temel URL** ve **Taşıyıcı Belirteci'ni**kopyalayın ve ardından ayarları kaydedin. Azure portalında, Temel **URL'yi** **Kiracı URL** kutusuna ve **Taşıyıcı Belirteci'ni** **Gizli Belirteç** kutusuna yapıştırın.

7. **Kiracı URL'sine** ve Gizli **Belirteç** kutularına değerleri girdikten sonra, Azure AD'nin Zscaler Two'ya bağlanabilmesini sağlamak için **Test Bağlantısı'nı** seçin. Bağlantı başarısız olursa, Zscaler Two hesabınızın yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Bağlantıyı sınama](./media/zscaler-two-provisioning-tutorial/test-connection.png)

8. Bildirim **E-posta** kutusuna, sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin. **Bir hata oluştuğunda e-posta gönder'i**seçin:

    ![Bildirim e-postasını ayarlama](./media/zscaler-two-provisioning-tutorial/notification.png)

9. **Kaydet'i**seçin.

10. **Eşlemeler** bölümünde, **Azure Etkin Dizin Kullanıcılarını ZscalerTwo ile Senkronize Et seçeneğini belirleyin:**

    ![Azure AD kullanıcılarını senkronize etme](./media/zscaler-two-provisioning-tutorial/user-mappings.png)

11. **Öznitelik Eşlemeleri** bölümünde Azure AD'den Zscaler Two'ya eşitlenen kullanıcı özniteliklerini gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Zscaler Two'daki kullanıcı hesaplarıyla eşleştirilmesi için kullanılır. Herhangi bir değişiklik gerçekleştirmek için **Kaydet'i** seçin.

    ![Öznitelik Eşlemeleri](./media/zscaler-two-provisioning-tutorial/user-attribute-mappings.png)

12. **Eşlemeler** bölümünde, **Azure Etkin Dizin Gruplarını ZscalerTwo ile Senkronize Et'i**seçin:

    ![Azure REKLAM gruplarını eşitle](./media/zscaler-two-provisioning-tutorial/group-mappings.png)

13. **Öznitelik Eşlemeleri** bölümünde Azure AD'den Zscaler Two'ya eşitlenen grup özniteliklerini gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Zscaler Two'daki gruplarla eşleştirilmesi için kullanılır. Herhangi bir değişiklik gerçekleştirmek için **Kaydet'i** seçin.

    ![Öznitelik Eşlemeleri](./media/zscaler-two-provisioning-tutorial/group-attribute-mappings.png)

14. Kapsam filtrelerini yapılandırmak [için, Kapsam](./../active-directory-saas-scoping-filters.md)filtresi öğreticisindeki yönergelere bakın.

15. Zscaler Two için Azure AD sağlama hizmetini etkinleştirmek **için, Ayarlar** bölümünde **Sağlama Durumunu** **Ayarı** olarak değiştirin:

    ![Sağlama Durumu](./media/zscaler-two-provisioning-tutorial/provisioning-status.png)

16. **Ayarlar** bölümünde **Kapsam** altında istediğiniz değerleri seçerek Zscaler Two'ya sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın:

    ![Kapsam değerleri](./media/zscaler-two-provisioning-tutorial/scoping.png)

17. Vermeye hazır olduğunuzda **Kaydet'i**seçin:

    ![Kaydet'i seçme](./media/zscaler-two-provisioning-tutorial/save-provisioning.png)

Bu işlem, **Ayarlar** bölümünde **Kapsam** altında tanımlanan tüm kullanıcıların ve grupların ilk eşitlemi başlar. Azure AD sağlama hizmeti nin çalıştırıldığı sürece, ilk eşitleme, yaklaşık her 40 dakikada bir gerçekleşen sonraki eşitlemelerden daha uzun sürer. **Eşitleme Ayrıntıları** bölümünde ilerlemeyi izleyebilirsiniz. Azure AD sağlama hizmeti tarafından Zscaler Two'da gerçekleştirilen tüm eylemleri açıklayan bir sağlama faaliyet raporuna ilişkin bağlantıları da izleyebilirsiniz.

Azure AD sağlama günlüklerini nasıl okuyabilirsiniz hakkında bilgi için bkz: [Otomatik kullanıcı hesabı sağlama hakkında Raporlama.](../active-directory-saas-provisioning-reporting.md)

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal uygulamalar için kullanıcı hesabı sağlamanın yönetimi](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri nasıl inceleyip sağlama etkinliği yle ilgili raporları nasıl alacağınızı öğrenin](../active-directory-saas-provisioning-reporting.md)

<!--Image references-->
[1]: ./media/zscaler-two-provisioning-tutorial/tutorial-general-01.png
[2]: ./media/zscaler-two-provisioning-tutorial/tutorial-general-02.png
[3]: ./media/zscaler-two-provisioning-tutorial/tutorial-general-03.png
