---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için Zscaler Three'ü yapılandırın | Microsoft Dokümanlar"
description: Bu eğitimde, Azure Active Directory'yi zscaler Three'e kullanıcı hesaplarını otomatik olarak sağlamak ve yok etmek için nasıl yapılandıracağınız öğrenilir.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd-msft
ms.assetid: 385a1153-0f47-4e41-8f44-da1b49d7629e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/27/2019
ms.author: jeedes
ms.openlocfilehash: 177bc34162c2b5e4dadc54e1166c5f6061068bae
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77064113"
---
# <a name="tutorial-configure-zscaler-three-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için Zscaler Üç'ü yapılandırın

Bu eğitimde, kullanıcıları ve/veya grupları Zscaler Three'e otomatik olarak sağlamak ve devre dışı etmek için Azure Active Directory'yi (Azure AD) nasıl yapılandırabileceğinizi öğreneceksiniz.

> [!NOTE]
> Bu öğretici, Azure AD kullanıcı sağlama hizmetinde yerleşik bir bağlayıcıyı açıklar. Bu hizmetin ne yaptığı ve nasıl çalıştığı hakkında önemli ayrıntılar ve sık sorulan soruların yanıtları için [bkz.](../active-directory-saas-app-provisioning.md)

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen adımları tamamlamak için aşağıdakileri yapmanız gerekir:

* Azure AD kiracısı.
* Bir Zscaler Üç kiracı.
* Yönetici izinleri olan Zscaler Three'de bir kullanıcı hesabı.

> [!NOTE]
> Azure AD sağlama tümleştirmesi, Kurumsal hesaplar için kullanılabilen Zscaler ZSCloud SCIM API'sine dayanır.

## <a name="adding-zscaler-three-from-the-gallery"></a>Galeriden Zscaler Üç ekleme

Azure AD ile otomatik kullanıcı sağlama için Zscaler Three'ü yapılandırmadan önce, Zscaler Three'ü Azure AD uygulama galerisinden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

Azure [portalında](https://portal.azure.com), sol bölmede, **Azure Etkin Dizini'ni**seçin:

![Azure Active Directory'yi seçin](common/select-azuread.png)

Kurumsal **uygulamalara** gidin ve ardından **Tüm uygulamaları**seçin:

![Kurumsal uygulamalar](common/enterprise-applications.png)

Uygulama eklemek için pencerenin üst kısmında **Yeni uygulama'yı** seçin:

![Yeni uygulama seçin](common/add-new-app.png)

Arama kutusuna **Zscaler Üç'ü**girin. Sonuçlarda **Zscaler Three'ü** seçin ve sonra **Ekle'yi**seçin.

![Sonuç listesi](common/search-new-app.png)

## <a name="assign-users-to-zscaler-three"></a>Kullanıcıları Zscaler Three'e atama

Azure AD kullanıcılarının kullanabilmeleri için seçili uygulamalara erişim atanması gerekir. Otomatik kullanıcı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya atanan kullanıcılar veya gruplar eşitlenir.

Otomatik kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki hangi kullanıcıların ve/veya grupların Zscaler Three'e erişmesi gerektiğine karar vermelisiniz. Buna karar verdikten sonra, [bir kullanıcıyı veya grubu kurumsal bir uygulamaya atama](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)yönergelerini izleyerek bu kullanıcıları ve grupları Zscaler Three'e atayabilirsiniz.

### <a name="important-tips-for-assigning-users-to-zscaler-three"></a>Zscaler Three'e kullanıcı atamak için önemli ipuçları

* Otomatik kullanıcı sağlama yapılandırmasını test etmek için önce Zscaler Three'e tek bir Azure AD kullanıcısı atamanızı tavsiye ettik. Daha sonra daha fazla kullanıcı ve grup atayabilirsiniz.

* Bir kullanıcıyı Zscaler Three'e atadığınızda, atama iletişim kutusunda geçerli bir uygulamaya özgü rolü (varsa) seçmeniz gerekir. **Varsayılan Erişim** rolüne sahip kullanıcılar sağlama nın dışında tutulur.

## <a name="set-up-automatic-user-provisioning"></a>Otomatik kullanıcı sağlama

Bu bölüm, Azure AD'deki kullanıcı ve grup atamalarına dayalı olarak Zscaler Three'deki kullanıcıları ve grupları oluşturmak, güncelleştirmek ve devre dışı etmek için Azure AD sağlama hizmetini yapılandırma adımları boyunca size yol göstermektedir.

> [!TIP]
> Ayrıca, Zscaler Three için SAML tabanlı tek oturum açmayı da etkinleştirmek isteyebilirsiniz. Bunu yaparsanız, [Zscaler Üç tek oturum açma öğretici](zscaler-three-tutorial.md)yönergeleri izleyin. Tek oturum açma, otomatik kullanıcı sağlamadan bağımsız olarak yapılandırılabilir, ancak iki özellik birbirini tamamlar.

1. [Azure portalında](https://portal.azure.com) oturum açın ve **Kurumsal uygulamaları** > seçin**Tüm uygulamalar** > **Zscaler Three**:

    ![Kurumsal uygulamalar](common/enterprise-applications.png)

2. Uygulamalar listesinde **Zscaler Three'u**seçin:

    ![Başvuru listesi](common/all-applications.png)

3. **Sağlama** sekmesini seçin:

    ![Zscaler Üç Sağlama](./media/zscaler-three-provisioning-tutorial/provisioning-tab.png)

4. Sağlama **Modunu** **Otomatik**Olarak Ayarlayın:

    ![Sağlama Modunu Ayarlama](./media/zscaler-three-provisioning-tutorial/provisioning-credentials.png)

5. Yönetici **Kimlik Bilgileri** bölümünde, bir sonraki adımda açıklandığı gibi Zscaler Three hesabınızın **Kiracı URL'sini** ve **Gizli Belirteci'ni** girin.

6. **Kiracı URL'sini** ve **Gizli Jetonu**almak için Zscaler Üç portalındaki **Yönetim** > **Kimlik Doğrulama Ayarları'na** gidin ve Kimlik **Doğrulama Türü**altında **SAML'yi** seçin:

    ![Zscaler Üç Kimlik Doğrulama Ayarları](./media/zscaler-three-provisioning-tutorial/secret-token-1.png)

    **YapılSAML** penceresini açmak için **Yapılandırılan SAML'yi** seçin:

    ![SAML pencereyi yapılandırma](./media/zscaler-three-provisioning-tutorial/secret-token-2.png)

    **SCIM Tabanlı Sağlama'yı etkinleştir'i** seçin ve **Temel URL** ve **Taşıyıcı Belirteci'ni**kopyalayın ve ardından ayarları kaydedin. Azure portalında, Temel **URL'yi** **Kiracı URL** kutusuna ve **Taşıyıcı Belirteci'ni** **Gizli Belirteç** kutusuna yapıştırın.

7. **Kiracı URL'sine** ve Gizli **Belirteç** kutularına değerleri girdikten sonra, Azure AD'nin Zscaler Three'e bağlanabilmesini sağlamak için **Test Bağlantısı'nı** seçin. Bağlantı başarısız olursa, Zscaler Three hesabınızın yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Bağlantıyı sınama](./media/zscaler-three-provisioning-tutorial/test-connection.png)

8. Bildirim **E-posta** kutusuna, sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin. **Bir hata oluştuğunda e-posta gönder'i**seçin:

    ![Bildirim e-postasını ayarlama](./media/zscaler-three-provisioning-tutorial/notification.png)

9. **Kaydet'i**seçin.

10. **Eşlemeler** bölümünde, **Azure Etkin Dizin Kullanıcılarını ZscalerThree ile Senkronize**Et:'yi seçin:

    ![Azure AD kullanıcılarını senkronize etme](./media/zscaler-three-provisioning-tutorial/user-mappings.png)

11. **Öznitelik Eşlemeleri** bölümünde Azure AD'den Zscaler Three'e eşitlenen kullanıcı özniteliklerini gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Zscaler Three'deki kullanıcı hesaplarıyla eşleştirilmesi için kullanılır. Herhangi bir değişiklik gerçekleştirmek için **Kaydet'i** seçin.

    ![Öznitelik Eşlemeleri](./media/zscaler-three-provisioning-tutorial/user-attribute-mappings.png)

12. **Eşlemeler** bölümünde, **Azure Etkin Dizin Gruplarını ZscalerThree**ile Senkronize Et:'yi seçin:

    ![Azure REKLAM gruplarını eşitle](./media/zscaler-three-provisioning-tutorial/group-mappings.png)

13. **Öznitelik Eşlemeleri** bölümünde Azure AD'den Zscaler Three'e eşitlenen grup özniteliklerini gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Zscaler Three'deki gruplarla eşleştirilmesi için kullanılır. Herhangi bir değişiklik gerçekleştirmek için **Kaydet'i** seçin.

    ![Öznitelik Eşlemeleri](./media/zscaler-three-provisioning-tutorial/group-attribute-mappings.png)

14. Kapsam filtrelerini yapılandırmak [için, Kapsam](./../active-directory-saas-scoping-filters.md)filtresi öğreticisindeki yönergelere bakın.

15. Zscaler Three için Azure AD sağlama hizmetini etkinleştirmek **için, Ayarlar** bölümünde **Sağlama Durumunu** **Ayarı** olarak değiştirin:

    ![Sağlama Durumu](./media/zscaler-three-provisioning-tutorial/provisioning-status.png)

16. **Ayarlar** bölümünde **Kapsam** kapsamında istediğiniz değerleri seçerek Zscaler Three'e sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın:

    ![Kapsam değerleri](./media/zscaler-three-provisioning-tutorial/scoping.png)

17. Vermeye hazır olduğunuzda **Kaydet'i**seçin:

    ![Kaydet'i seçme](./media/zscaler-three-provisioning-tutorial/save-provisioning.png)

Bu işlem, **Ayarlar** bölümünde **Kapsam** altında tanımlanan tüm kullanıcıların ve grupların ilk eşitlemi başlar. Azure AD sağlama hizmeti nin çalıştırıldığı sürece, ilk eşitleme, yaklaşık her 40 dakikada bir gerçekleşen sonraki eşitlemelerden daha uzun sürer. **Eşitleme Ayrıntıları** bölümünde ilerlemeyi izleyebilirsiniz. Azure AD sağlama hizmeti tarafından Zscaler Three'de gerçekleştirilen tüm eylemleri açıklayan bir sağlama faaliyet raporuna ilişkin bağlantıları da izleyebilirsiniz.

Azure AD sağlama günlüklerini nasıl okuyabilirsiniz hakkında bilgi için bkz: [Otomatik kullanıcı hesabı sağlama hakkında Raporlama.](../active-directory-saas-provisioning-reporting.md)

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal uygulamalar için kullanıcı hesabı sağlamanın yönetimi](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri nasıl inceleyip sağlama etkinliği yle ilgili raporları nasıl alacağınızı öğrenin](../active-directory-saas-provisioning-reporting.md)

<!--Image references-->
[1]: ./media/zscaler-three-provisioning-tutorial/tutorial-general-01.png
[2]: ./media/zscaler-three-provisioning-tutorial/tutorial-general-02.png
[3]: ./media/zscaler-three-provisioning-tutorial/tutorial-general-03.png
