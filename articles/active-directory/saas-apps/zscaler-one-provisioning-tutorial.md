---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için Zscaler One'ı yapılandırın | Microsoft Dokümanlar"
description: Azure Active Directory'yi, kullanıcı hesaplarını Zscaler One'a otomatik olarak sağlamak ve yok etmek için nasıl yapılandırılamayı öğrenin.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd-msft
ms.assetid: 72f6ba2b-73ed-420a-863a-aff672f26fa3
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2019
ms.author: jeedes
ms.openlocfilehash: 87413932acee576934ee50b59546371b03ceaf7e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77064181"
---
# <a name="tutorial-configure-zscaler-one-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için Zscaler One'ı yapılandırın

Bu öğretici, Azure AD'yi kullanıcıları ve grupları Zscaler One'a otomatik olarak sağlamak ve etkisiz katmak üzere yapılandırmak için Zscaler One ve Azure Etkin Dizini'nde (Azure AD) gerçekleştirecek adımları gösterir.

> [!NOTE]
> Bu öğretici, Azure AD kullanıcı sağlama hizmetinin üzerine inşa edilmiş bir bağlayıcıyı açıklar. Bu hizmetin ne yaptığı, nasıl çalıştığı ve sık sorulan sorular hakkında bilgi için [bkz.](../active-directory-saas-app-provisioning.md)


## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdakilere sahip olduğunuzu varsayar:

* Azure AD kiracısı.
* Bir Zscaler Bir kiracı.
* Yönetici izinleri olan Zscaler One'da bir kullanıcı hesabı.

> [!NOTE]
> Azure AD sağlama tümleştirmesi Zscaler One SCIM API'ye dayanır. Bu API, Kurumsal pakete sahip hesaplar için Zscaler One geliştiricileri tarafından kullanılabilir.

## <a name="add-zscaler-one-from-the-azure-marketplace"></a>Azure Marketinden Zscaler Bir Ekle

Azure AD ile otomatik kullanıcı sağlama için Zscaler One'ı yapılandırmadan önce, Azure Marketi'nden Zscaler One'ı yönetilen SaaS uygulamaları listenize ekleyin.

Pazar Yeri'nden Zscaler One eklemek için aşağıdaki adımları izleyin.

1. Azure [portalında,](https://portal.azure.com)soldaki gezinti bölmesinde **Azure Etkin Dizini'ni**seçin.

    ![Azure Etkin Dizin simgesi](common/select-azuread.png)

2. Kurumsal **uygulamalara**gidin ve ardından **Tüm uygulamaları**seçin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için iletişim kutusunun üst kısmında **Yeni uygulama'yı** seçin.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Zscaler One** girin ve sonuç panelinden **Zscaler One'ı** seçin. Uygulamayı eklemek için **Ekle'yi**seçin.

    ![Zscaler Bir sonuç listesinde](common/search-new-app.png)

## <a name="assign-users-to-zscaler-one"></a>Kullanıcıları Zscaler One'a atama

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişmesi gerektiğini belirlemek için *atamalar* adlı bir kavram kullanır. Otomatik kullanıcı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya atanan kullanıcılar veya gruplar eşitlenir.

Otomatik kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki hangi kullanıcıların veya grupların Zscaler One'a erişmeye ihtiyacı olduğuna karar verin. Bu kullanıcıları veya grupları Zscaler One'a atamak için, [bir kullanıcıyı veya grubu kurumsal bir uygulamaya atama](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)yönergelerini izleyin.

### <a name="important-tips-for-assigning-users-to-zscaler-one"></a>Zscaler One'a kullanıcı atamak için önemli ipuçları

* Otomatik kullanıcı sağlama yapılandırmasını test etmek için Zscaler One'a tek bir Azure AD kullanıcısı atamanızı öneririz. Daha sonra ek kullanıcılar veya gruplar atayabilirsiniz.

* Bir kullanıcıyı Zscaler One'a atadığınızda, atama iletişim kutusunda geçerli bir uygulamaya özgü rolü seçin. **Varsayılan Erişim** rolüne sahip kullanıcılar sağlama nın dışında tutulur.

## <a name="configure-automatic-user-provisioning-to-zscaler-one"></a>Otomatik kullanıcı sağlamayı Zscaler One olarak yapılandırın

Bu bölüm, Azure AD sağlama hizmetini yapılandırma adımlarında size yol gösteriş eder. Azure AD'deki kullanıcı veya grup atamalarına dayalı olarak Zscaler One'daki kullanıcıları veya grupları oluşturmak, güncelleştirmek ve devre dışı kalmak için kullanın.

> [!TIP]
> Ayrıca Zscaler One için SAML tabanlı tek oturum açmayı da etkinleştirebilirsiniz. [Zscaler One tek oturum açma öğreticiyönergeleri](zscaler-One-tutorial.md)izleyin. Tek oturum açma, bu iki özellik birbirini tamamlasa da, otomatik kullanıcı sağlamadan bağımsız olarak yapılandırılabilir.

### <a name="configure-automatic-user-provisioning-for-zscaler-one-in-azure-ad"></a>Azure AD'de Zscaler One için otomatik kullanıcı sağlama yapılandırma

1. [Azure portalında](https://portal.azure.com)oturum açın. **Kurumsal uygulamaları** > seçin**Tüm uygulamalar** > **Zscaler Bir**.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Zscaler One'ı**seçin.

    ![Uygulamalar listesindeki Zscaler Bir bağlantı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Zscaler Bir Sağlama](./media/zscaler-one-provisioning-tutorial/provisioning-tab.png)

4. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

    ![Zscaler Bir Sağlama Modu](./media/zscaler-one-provisioning-tutorial/provisioning-credentials.png)

5. Yönetici **Kimlik Bilgileri** bölümünde, **Kiracı URL'sini** ve **Gizli Belirteç** kutularını Adım 6'da açıklandığı gibi Zscaler One hesabınızın ayarlarıyla doldurun.

6. Kiracı URL'sini ve gizli jetonu elde etmek için Zscaler One portalı Kullanıcı Arabirimi'ndeki **Yönetim** > **Kimlik Doğrulama Ayarları'na** gidin. **Kimlik Doğrulama Türü**altında **SAML'yi**seçin.

    ![Zscaler One Kimlik Doğrulama Ayarları](./media/zscaler-one-provisioning-tutorial/secret-token-1.png)

    a. YapılSAML seçeneklerini açmak için **YAPıya'yı** **SAML'yi** seçin.

    ![Zscaler Bir Yapılandırılan SAML](./media/zscaler-one-provisioning-tutorial/secret-token-2.png)

    b. **Temel URL** ve **Taşıyıcı Belirteç'teki**ayarları almak için **SCIM Tabanlı Sağlama'yı etkinleştir'i** seçin. Ardından ayarları kaydedin. Azure portalında **Temel URL** ayarını **Kiracı URL'sine** kopyalayın. Taşıyıcı **Belirteci** ayarını Azure portalında **Gizli Belirteç** olarak kopyalayın.

7. Adım 5'te gösterilen kutuları doldurduktan sonra, Azure AD'nin Zscaler One'a bağlanabilmesini sağlamak için **Test Bağlantısı'nı** seçin. Bağlantı başarısız olursa, Zscaler One hesabınızın yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Zscaler Bir Test Bağlantısı](./media/zscaler-one-provisioning-tutorial/test-connection.png)

8. Bildirim **E-posta** kutusuna, sağlama hatası bildirimlerini almak için kişinin veya grubun e-posta adresini girin. Bir **hata olduğunda e-posta gönder bildirimini** seçin onay kutusunu seçin.

    ![Zscaler Bir Bildirim E-posta](./media/zscaler-one-provisioning-tutorial/notification.png)

9. **Kaydet'i**seçin.

10. **Eşlemeler** bölümünde, **Azure Etkin Dizin Kullanıcılarını Zscaler One'a Senkronize Et'i**seçin.

    ![Zscaler Bir kullanıcı senkronizasyonu](./media/zscaler-one-provisioning-tutorial/user-mappings.png)

11. **Öznitelik Eşlemeleri** bölümünde Azure AD'den Zscaler One'a eşitlenen kullanıcı özniteliklerini gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Zscaler One'daki kullanıcı hesaplarıyla eşleştirilmesi için kullanılır. Herhangi bir değişikliği kaydetmek için **Kaydet'i**seçin.

    ![Zscaler Bir eşleşen kullanıcı öznitelikleri](./media/zscaler-one-provisioning-tutorial/user-attribute-mappings.png)

12. **Eşlemeler** bölümünde, **Azure Etkin Dizin Gruplarını Zscaler One'a Senkronize Et'i**seçin.

    ![Zscaler Bir grup senkronizasyonu](./media/zscaler-one-provisioning-tutorial/group-mappings.png)

13. **Öznitelik Eşlemeleri** bölümünde Azure AD'den Zscaler One'a eşitlenen grup özniteliklerini gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Zscaler One'daki gruplarla eşleştirilmesi için kullanılır. Herhangi bir değişikliği kaydetmek için **Kaydet'i**seçin.

    ![Zscaler Bir eşleşen grup öznitelikleri](./media/zscaler-one-provisioning-tutorial/group-attribute-mappings.png)

14. Kapsam filtrelerini yapılandırmak [için, kapsam filtresi](./../active-directory-saas-scoping-filters.md)öğreticisindeki yönergeleri izleyin.

15. **Ayarlar** bölümündeZscaler One için Azure AD sağlama hizmetini etkinleştirmek **için, Sağlama Durumunu** **A'da**olarak değiştirin.

    ![Zscaler Bir Sağlama Durumu](./media/zscaler-one-provisioning-tutorial/provisioning-status.png)

16. Zscaler One'a sağlamak istediğiniz kullanıcıları veya grupları tanımlayın. **Ayarlar** bölümünde, **Kapsam'ta**istediğiniz değerleri seçin.

    ![Zscaler Bir Kapsam](./media/zscaler-one-provisioning-tutorial/scoping.png)

17. Hükmetmeye hazır olduğunuzda **Kaydet'i**seçin.

    ![Zscaler Bir Kaydet](./media/zscaler-one-provisioning-tutorial/save-provisioning.png)

Bu işlem, **Ayarlar** bölümünde **Kapsam'ta** tanımlanan tüm kullanıcıların veya grupların ilk eşitlemisini başlatır. İlk eşitlemenin gerçekleştirilemi daha sonraki eşitlemelerden daha uzun sürüyor. Azure AD sağlama hizmeti çalıştığı sürece yaklaşık her 40 dakikada bir oluşur. 

İlerlemeyi izlemek ve sağlama faaliyet raporuna bağlı bağlantıları izlemek için **Eşitleme Ayrıntıları** bölümünü kullanabilirsiniz. Rapor, Azure AD sağlama hizmeti tarafından Zscaler One'da gerçekleştirilen tüm eylemleri açıklar.

Azure AD sağlama günlüklerini nasıl okuyabilirsiniz hakkında bilgi için bkz: [Otomatik kullanıcı hesabı sağlama hakkında Raporlama.](../active-directory-saas-provisioning-reporting.md)

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal uygulamalar için kullanıcı hesabı sağlamayı yönetme](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri nasıl inceleyip sağlama etkinliği yle ilgili raporları nasıl alacağınızı öğrenin](../active-directory-saas-provisioning-reporting.md)

<!--Image references-->
[1]: ./media/zscaler-one-provisioning-tutorial/tutorial-general-01.png
[2]: ./media/zscaler-one-provisioning-tutorial/tutorial-general-02.png
[3]: ./media/zscaler-one-provisioning-tutorial/tutorial-general-03.png
