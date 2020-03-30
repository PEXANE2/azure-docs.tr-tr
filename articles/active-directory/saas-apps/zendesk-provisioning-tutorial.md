---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için Zendesk'i yapılandırın | Microsoft Dokümanlar"
description: Azure Active Directory'yi, kullanıcı hesaplarını Zendesk'e otomatik olarak sağlamak ve yok etmek için nasıl yapılandırılamayı öğrenin.
services: active-directory
documentationcenter: ''
author: zhchia
writer: zhchia
manager: beatrizd-msft
ms.assetid: 01d5e4d5-d856-42c4-a504-96fa554baf66
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/06/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: a480119ee88521b920be88669f6d80e3754d24d3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77062764"
---
# <a name="tutorial-configure-zendesk-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için Zendesk'i yapılandırın

Bu öğretici, Azure AD'yi kullanıcıları ve grupları Zendesk'e otomatik olarak sağlamak ve etkisiz leştirmek üzere yapılandırmak için Zendesk ve Azure Etkin Dizini'nde (Azure AD) gerçekleştirecek adımları gösterir.

> [!NOTE]
> Bu öğretici, Azure AD kullanıcı sağlama hizmetinin üzerine inşa edilmiş bir bağlayıcıyı açıklar. Bu hizmetin ne yaptığı, nasıl çalıştığı ve sık sorulan sorular hakkında bilgi için [bkz.](../app-provisioning/user-provisioning.md)

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdakilere sahip olduğunuzu varsayar:

* Azure AD kiracısı.
* Profesyonel planı veya daha iyi etkin bir Zendesk kiracı.
* Zendesk'te yönetici izinleri olan bir kullanıcı hesabı.

## <a name="add-zendesk-from-the-azure-marketplace"></a>Azure Marketinden Zendesk Ekle

Azure AD ile otomatik kullanıcı sağlama için Zendesk'i yapılandırmadan önce, Azure Marketi'nden Yönetilen SaaS uygulamaları listenize Zendesk'i ekleyin.

Pazar Yeri'nden Zendesk eklemek için aşağıdaki adımları izleyin.

1. Azure [portalında,](https://portal.azure.com)soldaki gezinti bölmesinde **Azure Etkin Dizini'ni**seçin.

    ![Azure Etkin Dizin simgesi](common/select-azuread.png)

2. Kurumsal **uygulamalara**gidin ve ardından **Tüm uygulamaları**seçin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için iletişim kutusunun üst kısmında **Yeni uygulama'yı** seçin.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Zendesk'e** girin ve sonuç panelinden **Zendesk'i** seçin. Uygulamayı eklemek için **Ekle'yi**seçin.

    ![Zendesk sonuç listesinde](common/search-new-app.png)

## <a name="assign-users-to-zendesk"></a>Kullanıcıları Zendesk'e atama

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişmesi gerektiğini belirlemek için *atamalar* adlı bir kavram kullanır. Otomatik kullanıcı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya atanan kullanıcılar veya gruplar eşitlenir.

Otomatik kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki hangi kullanıcıların veya grupların Zendesk'e erişmeye ihtiyacı olduğuna karar verin. Bu kullanıcıları veya grupları Zendesk'e atamak için, [bir kullanıcıyı veya grubu kurumsal bir uygulamaya atama](../manage-apps/assign-user-or-group-access-portal.md)yönergelerini izleyin.

### <a name="important-tips-for-assigning-users-to-zendesk"></a>Zendesk'e kullanıcı atamak için önemli ipuçları

* Bugün, Zendesk rolleri Azure portalı ui'sinde otomatik olarak ve dinamik olarak doldurulur. Kullanıcılara Zendesk rolleri atamadan önce, Zendesk kiracınızdaki en son rolleri almak için Zendesk'e karşı ilk eşitlemenin tamamlandığından emin olun.

* İlk otomatik kullanıcı sağlama yapılandırmanızı test etmek için Zendesk'e tek bir Azure REKLAM kullanıcısı atamanızı öneririz. Testler başarılı olduktan sonra ek kullanıcılar veya gruplar atayabilirsiniz.

* Bir kullanıcıyı Zendesk'e atadığınızda, atama iletişim kutusunda geçerli bir uygulamaya özgü rolü seçin. **Varsayılan Erişim** rolüne sahip kullanıcılar sağlama nın dışında tutulur.

## <a name="configure-automatic-user-provisioning-to-zendesk"></a>Otomatik kullanıcı sağlamayı Zendesk'e yapılandırma 

Bu bölüm, Azure AD sağlama hizmetini yapılandırma adımlarında size yol gösteriş eder. Azure AD'deki kullanıcı veya grup atamalarına göre Zendesk'teki kullanıcıları veya grupları oluşturmak, güncellemek ve devre dışı kalmak için kullanın.

> [!TIP]
> Ayrıca Zendesk için SAML tabanlı tek oturum açma yı da etkinleştirebilirsiniz. [Zendesk tek oturum açma öğretici](zendesk-tutorial.md)yönergeleri izleyin. Tek oturum açma, bu iki özellik birbirini tamamlasa da, otomatik kullanıcı sağlamadan bağımsız olarak yapılandırılabilir.

### <a name="configure-automatic-user-provisioning-for-zendesk-in-azure-ad"></a>Azure AD'de Zendesk için otomatik kullanıcı sağlama yapılandırma

1. [Azure portalında](https://portal.azure.com)oturum açın. **Kurumsal uygulamaları** > seçin**Tüm uygulamalar** > **Zendesk**.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulama listesinde **Zendesk'i**seçin.

    ![Uygulamalar listesindeki Zendesk bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Zendesk Temini](./media/zendesk-provisioning-tutorial/ZenDesk16.png)

4. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

    ![Zendesk Sağlama Modu](./media/zendesk-provisioning-tutorial/ZenDesk1.png)

5. Yönetici **Kimlik Bilgileri** bölümü ne si bölümünde, Zendesk hesabınızın yönetici kullanıcı adını, gizli belirteci ve etki alanını girin. Bu değerlere örnek olarak şunlar verilebilir:

   * Yönetici **Kullanıcı Adı** kutusunda, Zendesk kiracınızdaki yönetici hesabının kullanıcı adını girin. admin@contoso.com bunun bir örneğidir.

   * Gizli **Belirteç** kutusunda, Adım 6'da açıklandığı gibi gizli belirteci doldurun.

   * Etki **Alanı** kutusunda, Zendesk kiracınızın alt etki alanını doldurun. Örneğin, kiracı URL'si `https://my-tenant.zendesk.com`olan bir hesap için alt etki alanınız benim **kiracımdır.**

6. Zendesk hesabınızın gizli belirteci **Yönetici** > **API** > **Ayarları'nda**yer alır. **Token Access'in** **Etkin**olarak ayarlandıklarına emin olun.

    ![Zendesk yönetici ayarları](./media/zendesk-provisioning-tutorial/ZenDesk4.png)

    ![Zendesk gizli belirteç](./media/zendesk-provisioning-tutorial/ZenDesk2.png)

7. Adım 5'te gösterilen kutuları doldurduktan sonra, Azure AD'nin Zendesk'e bağlanabilmesini sağlamak için **Test Bağlantısı'nı** seçin. Bağlantı başarısız olursa, Zendesk hesabınızın yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Zendesk Test Bağlantısı](./media/zendesk-provisioning-tutorial/ZenDesk19.png)

8. Bildirim **E-posta** kutusuna, sağlama hatası bildirimlerini almak için kişinin veya grubun e-posta adresini girin. Bir **hata olduğunda e-posta gönder bildirimini** seçin onay kutusunu seçin.

    ![Zendesk Bildirim E-posta](./media/zendesk-provisioning-tutorial/ZenDesk9.png)

9. **Kaydet'i**seçin.

10. **Eşlemeler** bölümünde, **Azure Etkin Dizin Kullanıcılarını Zendesk'e Senkronize Et'i**seçin.

    ![Zendesk kullanıcı senkronizasyonu](./media/zendesk-provisioning-tutorial/ZenDesk10.png)

11. Azure AD'den Zendesk'e eşitlenen kullanıcı özniteliklerini **Öznitelik Eşlemeleri** bölümünde gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Zendesk'teki kullanıcı hesaplarıyla eşleştirilmesi için kullanılır. Herhangi bir değişikliği kaydetmek için **Kaydet'i**seçin.

    ![Zendesk eşleşen kullanıcı öznitelikleri](./media/zendesk-provisioning-tutorial/ZenDesk11.png)

12. **Eşlemeler** bölümünde, **Azure Etkin Dizin Gruplarını Zendesk'e Senkronize Et'i**seçin.

    ![Zendesk grup senkronizasyonu](./media/zendesk-provisioning-tutorial/ZenDesk12.png)

13. Azure AD'den Zendesk'e eşitlenen grup özniteliklerini **Öznitelik Eşlemeleri** bölümünde gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Zendesk'teki gruplarla eşleşmek için kullanılır. Herhangi bir değişikliği kaydetmek için **Kaydet'i**seçin.

    ![Zendesk eşleştirme grup öznitelikleri](./media/zendesk-provisioning-tutorial/ZenDesk13.png)

14. Kapsam filtrelerini yapılandırmak [için, kapsam filtresi](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)öğreticisindeki yönergeleri izleyin.

15. **Ayarlar** bölümünde, Zendesk için Azure AD sağlama hizmetini etkinleştirmek **için, Sağlama Durumunu** **A'da**olarak değiştirin.

    ![Zendesk Sağlama Durumu](./media/zendesk-provisioning-tutorial/ZenDesk14.png)

16. Zendesk'e sağlamak istediğiniz kullanıcıları veya grupları tanımlayın. **Ayarlar** bölümünde, **Kapsam'ta**istediğiniz değerleri seçin.

    ![Zendesk Kapsamı](./media/zendesk-provisioning-tutorial/ZenDesk15.png)

17. Hükmetmeye hazır olduğunuzda **Kaydet'i**seçin.

    ![Zendesk Kaydet](./media/zendesk-provisioning-tutorial/ZenDesk18.png)

Bu işlem, **Ayarlar** bölümünde **Kapsam'ta** tanımlanan tüm kullanıcıların veya grupların ilk eşitlemisini başlatır. İlk eşitlemenin gerçekleştirilemi daha sonraki eşitlemelerden daha uzun sürüyor. Azure AD sağlama hizmeti çalıştığı sürece yaklaşık her 40 dakikada bir oluşur. 

İlerlemeyi izlemek ve sağlama faaliyet raporuna bağlı bağlantıları izlemek için **Eşitleme Ayrıntıları** bölümünü kullanabilirsiniz. Rapor, Azure AD sağlama hizmeti tarafından Zendesk'te gerçekleştirilen tüm eylemleri açıklar.

Azure AD sağlama günlüklerini nasıl okuyabilirsiniz hakkında bilgi için bkz: [Otomatik kullanıcı hesabı sağlama hakkında Raporlama.](../app-provisioning/check-status-user-account-provisioning.md)

## <a name="connector-limitations"></a>Konektör sınırlamaları

* Zendesk, yalnızca **Agent** rolleri olan kullanıcılar için grupların kullanımını destekler. Daha fazla bilgi için [Zendesk belgelerine](https://support.zendesk.com/hc/en-us/articles/203661966-Creating-managing-and-using-groups)bakın.

* Bir kullanıcıya veya gruba özel bir rol atandığında, Azure AD otomatik kullanıcı sağlama hizmeti de varsayılan rol **Aracısını**atar. Yalnızca Aracılar özel bir rol atanabilir. Daha fazla bilgi için [Zendesk API belgelerine](https://developer.zendesk.com/rest_api/docs/support/users#json-format-for-agent-or-admin-requests)bakın. 

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal uygulamalar için kullanıcı hesabı sağlamayı yönetme](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri nasıl inceleyip sağlama etkinliği yle ilgili raporları nasıl alacağınızı öğrenin](../app-provisioning/check-status-user-account-provisioning.md)

<!--Image references-->
[1]: ./media/zendesk-tutorial/tutorial_general_01.png
[2]: ./media/zendesk-tutorial/tutorial_general_02.png
[3]: ./media/zendesk-tutorial/tutorial_general_03.png
