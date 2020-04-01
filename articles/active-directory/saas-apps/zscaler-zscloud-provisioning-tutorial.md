---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için Zscaler ZSCloud'u yapılandırın | Microsoft Dokümanlar"
description: Bu eğitimde, Azure Active Directory'yi zscaler ZSCloud'a kullanıcı hesaplarını otomatik olarak sağlamak ve yok etmek için nasıl yapılandıracağınız öğrenilir.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd-msft
ms.assetid: a752be80-d3ef-45d1-ac8f-4fb814c07b07
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/27/2019
ms.author: jeedes
ms.openlocfilehash: af9d4aa15a4ff2784d2e1ca1334d9c24a8d12251
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77062685"
---
# <a name="tutorial-configure-zscaler-zscloud-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için Zscaler ZSCloud'u yapılandırın

Bu eğitimde, kullanıcıları ve/veya grupları Zscaler ZSCloud'a otomatik olarak sağlamak ve devre dışı etmek için Azure Active Directory'yi (Azure AD) nasıl yapılandırabileceğinizi öğreneceksiniz.

> [!NOTE]
> Bu öğretici, Azure AD kullanıcı sağlama hizmetinde yerleşik bir bağlayıcıyı açıklar. Bu hizmetin ne yaptığı ve nasıl çalıştığı hakkında önemli ayrıntılar ve sık sorulan soruların yanıtları için [bkz.](../active-directory-saas-app-provisioning.md)


## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen adımları tamamlamak için aşağıdakileri yapmanız gerekir:

* Azure AD kiracısı.
* Bir Zscaler ZSCloud kiracı.
* Yönetici izinleri olan Zscaler ZSCloud'da bir kullanıcı hesabı.

> [!NOTE]
> Azure AD sağlama tümleştirmesi, Kurumsal hesaplar için kullanılabilen Zscaler ZSCloud SCIM API'sine dayanır.

## <a name="add-zscaler-zscloud-from-the-gallery"></a>Galeriden Zscaler ZSCloud ekleyin

Zscaler ZSCloud'u Azure AD ile otomatik kullanıcı sağlama için yapılandırmadan önce, Azure AD uygulama galerisinden Zscaler ZSCloud'u yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

Azure [portalında](https://portal.azure.com), sol bölmede, **Azure Etkin Dizini'ni**seçin:

![Azure Active Directory'yi seçin](common/select-azuread.png)

Kurumsal **uygulamalara** gidin ve ardından **Tüm uygulamaları**seçin:

![Kurumsal uygulamalar](common/enterprise-applications.png)

Uygulama eklemek için pencerenin üst kısmında **Yeni uygulama'yı** seçin:

![Yeni uygulama seçin](common/add-new-app.png)

Arama kutusuna **Zscaler ZSCloud**girin. Sonuçlarda **Zscaler ZSCloud'u** seçin ve ardından **Ekle'yi**seçin.

![Sonuç listesi](common/search-new-app.png)

## <a name="assign-users-to-zscaler-zscloud"></a>Kullanıcıları Zscaler ZSCloud'a atama

Azure AD kullanıcılarının kullanabilmeleri için seçili uygulamalara erişim atanması gerekir. Otomatik kullanıcı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya atanan kullanıcılar veya gruplar eşitlenir.

Otomatik kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki hangi kullanıcıların ve/veya grupların Zscaler ZSCloud'a erişmesi gerektiğine karar vermelisiniz. Buna karar verdikten sonra, [bir kullanıcıyı veya grubu kurumsal bir uygulamaya atama](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)yönergelerini izleyerek bu kullanıcıları ve grupları Zscaler ZSCloud'a atayabilirsiniz.

### <a name="important-tips-for-assigning-users-to-zscaler-zscloud"></a>Zscaler ZSCloud'a kullanıcı atamak için önemli ipuçları

* Otomatik kullanıcı sağlama yapılandırmasını test etmek için önce Zscaler ZSCloud'a tek bir Azure AD kullanıcısı atamanızı öneririz. Daha sonra daha fazla kullanıcı ve grup atayabilirsiniz.

* Bir kullanıcıyı Zscaler ZSCloud'a atadığınızda, atama iletişim kutusunda uygulamaya özgü geçerli bir rolü (varsa) seçmeniz gerekir. **Varsayılan Erişim** rolüne sahip kullanıcılar sağlama nın dışında tutulur.

## <a name="set-up-automatic-user-provisioning"></a>Otomatik kullanıcı sağlama

Bu bölüm, Azure AD'deki kullanıcı ve grup atamalarına dayalı olarak Zscaler ZSCloud'daki kullanıcıları ve grupları oluşturmak, güncellemek ve devre dışı etmek için Azure AD sağlama hizmetini yapılandırma adımları boyunca size yol göstermektedir.

> [!TIP]
> Zscaler ZSCloud için SAML tabanlı tek oturum açma'yı da etkinleştirmek isteyebilirsiniz. Bunu yaparsanız, [Zscaler ZSCloud tek oturum açma öğreticiyönergeleri](zscaler-zsCloud-tutorial.md)izleyin. Tek oturum açma, otomatik kullanıcı sağlamadan bağımsız olarak yapılandırılabilir, ancak iki özellik birbirini tamamlar.

1. [Azure portalında](https://portal.azure.com) oturum açın ve **Kurumsal uygulamaları** > seçin**Tüm uygulamalar** > **Zscaler ZSCloud**:

    ![Kurumsal uygulamalar](common/enterprise-applications.png)

2. Uygulamalar listesinde **Zscaler ZSCloud'u**seçin:

    ![Başvuru listesi](common/all-applications.png)

3. **Sağlama** sekmesini seçin:

    ![Zscaler ZSCloud Sağlama](./media/zscaler-zscloud-provisioning-tutorial/provisioningtab.png)

4. Sağlama **Modunu** **Otomatik**Olarak Ayarlayın:

    ![Sağlama Modunu Ayarlama](./media/zscaler-zscloud-provisioning-tutorial/provisioningcredentials.png)

5. Yönetici **Kimlik Bilgileri** bölümünde, bir sonraki adımda açıklandığı gibi Zscaler ZSCloud hesabınızın **Kiracı URL'sini** ve **Gizli Belirteci'ni** girin.

6. **Kiracı URL'sini** ve **Gizli Jetonu**almak için Zscaler ZSCloud portalındaki **Yönetim** > **Kimlik Doğrulama Ayarları'na** gidin ve Kimlik **Doğrulama Türü**altında **SAML'yi** seçin:

    ![Zscaler ZSCloud Kimlik Doğrulama Ayarları](./media/zscaler-zscloud-provisioning-tutorial/secrettoken1.png)

    **YapılSAML** penceresini açmak için **Yapılandırılan SAML'yi** seçin:

    ![SAML pencereyi yapılandırma](./media/zscaler-zscloud-provisioning-tutorial/secrettoken2.png)

    **SCIM Tabanlı Sağlama'yı etkinleştir'i** seçin ve **Temel URL** ve **Taşıyıcı Belirteci'ni**kopyalayın ve ardından ayarları kaydedin. Azure portalında, Temel **URL'yi** **Kiracı URL** kutusuna ve **Taşıyıcı Belirteci'ni** **Gizli Belirteç** kutusuna yapıştırın.

7. **Kiracı URL'sine** ve Gizli **Belirteç** kutularına değerleri girdikten sonra, Azure AD'nin Zscaler ZSCloud'a bağlanabilmesini sağlamak için **Test Bağlantısı'nı** seçin. Bağlantı başarısız olursa, Zscaler ZSCloud hesabınızın yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Bağlantıyı sınama](./media/zscaler-zscloud-provisioning-tutorial/testconnection.png)

8. Bildirim **E-posta** kutusuna, sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin. **Bir hata oluştuğunda e-posta gönder'i**seçin:

    ![Bildirim e-postasını ayarlama](./media/zscaler-zscloud-provisioning-tutorial/Notification.png)

9. **Kaydet'i**seçin.

10. **Eşlemeler** bölümünde, **Azure Etkin Dizin Kullanıcılarını ZscalerZSCloud ile Senkronize Et seçeneğini belirleyin:**

    ![Azure AD kullanıcılarını senkronize etme](./media/zscaler-zscloud-provisioning-tutorial/usermappings.png)

11. **Öznitelik Eşlemeleri** bölümünde Azure AD'den Zscaler ZSCloud'a senkronize edilen kullanıcı özniteliklerini gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Zscaler ZSCloud'daki kullanıcı hesaplarıyla eşleştirilmesi için kullanılır. Herhangi bir değişiklik gerçekleştirmek için **Kaydet'i** seçin.

    ![Öznitelik Eşlemeleri](./media/zscaler-zscloud-provisioning-tutorial/userattributemappings.png)

12. **Eşlemeler** bölümünde, **Azure Etkin Dizin Gruplarını ZscalerZSCloud'a Senkronize Et'i**seçin:

    ![Azure REKLAM gruplarını eşitle](./media/zscaler-zscloud-provisioning-tutorial/groupmappings.png)

13. **Öznitelik Eşlemeleri** bölümünde Azure AD'den Zscaler ZSCloud'a eşitlenen grup özniteliklerini gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Zscaler ZSCloud'daki gruplarla eşleştirilmesi için kullanılır. Herhangi bir değişiklik gerçekleştirmek için **Kaydet'i** seçin.

    ![Öznitelik Eşlemeleri](./media/zscaler-zscloud-provisioning-tutorial/groupattributemappings.png)

14. Kapsam filtrelerini yapılandırmak [için, Kapsam](./../active-directory-saas-scoping-filters.md)filtresi öğreticisindeki yönergelere bakın.

15. Zscaler ZSCloud için Azure AD sağlama hizmetini etkinleştirmek **için, Ayarlar** bölümünde **Sağlama Durumunu** **Ayarı** olarak değiştirin:

    ![Sağlama Durumu](./media/zscaler-zscloud-provisioning-tutorial/provisioningstatus.png)

16. **Ayarlar** bölümünde **Kapsam** kapsamında istediğiniz değerleri seçerek Zscaler ZSCloud'a sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın:

    ![Kapsam değerleri](./media/zscaler-zscloud-provisioning-tutorial/scoping.png)

17. Vermeye hazır olduğunuzda **Kaydet'i**seçin:

    ![Kaydet'i seçme](./media/zscaler-zscloud-provisioning-tutorial/saveprovisioning.png)

Bu işlem, **Ayarlar** bölümünde **Kapsam** altında tanımlanan tüm kullanıcıların ve grupların ilk eşitlemi başlar. Azure AD sağlama hizmeti nin çalıştırıldığı sürece, ilk eşitleme, yaklaşık her 40 dakikada bir gerçekleşen sonraki eşitlemelerden daha uzun sürer. **Eşitleme Ayrıntıları** bölümünde ilerlemeyi izleyebilirsiniz. Azure AD sağlama hizmeti tarafından Zscaler ZSCloud'da gerçekleştirilen tüm eylemleri açıklayan bir sağlama faaliyet raporuna ilişkin bağlantıları da izleyebilirsiniz.

Azure AD sağlama günlüklerini nasıl okuyabilirsiniz hakkında bilgi için bkz: [Otomatik kullanıcı hesabı sağlama hakkında Raporlama.](../active-directory-saas-provisioning-reporting.md)

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal uygulamalar için kullanıcı hesabı sağlamanın yönetimi](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri nasıl inceleyip sağlama etkinliği yle ilgili raporları nasıl alacağınızı öğrenin](../active-directory-saas-provisioning-reporting.md)

<!--Image references-->
[1]: ./media/zscaler-zscloud-provisioning-tutorial/tutorial-general-01.png
[2]: ./media/zscaler-zscloud-provisioning-tutorial/tutorial-general-02.png
[3]: ./media/zscaler-zscloud-provisioning-tutorial/tutorial-general-03.png
