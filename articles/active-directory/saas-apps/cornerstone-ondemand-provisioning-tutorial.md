---
title: 'Öğretici: Azure Active Directory ile otomatik Kullanıcı sağlama için köşeli temel taş OnDemand yapılandırma | Microsoft Docs'
description: Kullanıcı hesaplarını otomatik olarak sağlamak ve sağlaması için Azure Active Directory nasıl yapılandırılacağını öğrenin.
services: active-directory
documentationcenter: ''
author: zhchia
writer: zhchia
manager: beatrizd
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
ms.openlocfilehash: 6212e74ecbf8327d3939138de2e92868f29b0f1a
ms.sourcegitcommit: db2d402883035150f4f89d94ef79219b1604c5ba
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2020
ms.locfileid: "77058456"
---
# <a name="tutorial-configure-cornerstone-ondemand-for-automatic-user-provisioning"></a>Öğretici: otomatik Kullanıcı hazırlama için temel taş OnDemand yapılandırma

Bu öğreticide, Azure AD 'yi, Kullanıcı veya grupları otomatik olarak sağlamak ve sağlaması için, temel bir kullanıcı veya grup sağlamak üzere yapılandırmak üzere, temel Stone OnDemand ve Azure Active Directory (Azure AD) adımları gösterilmektedir.

> [!NOTE]
> Bu öğreticide, Azure AD Kullanıcı sağlama hizmeti 'nin üzerine kurulmuş bir bağlayıcı açıklanmaktadır. Bu hizmetin ne yaptığını, nasıl çalıştığını ve sık sorulan soruları öğrenmek için bkz. [Azure Active Directory ile hizmet olarak yazılım (SaaS) uygulamalarına Kullanıcı sağlamayı ve sağlamayı kaldırmayı otomatikleştirme](../app-provisioning/user-provisioning.md).

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen senaryo şunları olduğunu varsayar:

* Azure AD kiracısı.
* Bir temel taş OnDemand kiracısı.
* Temel bir kullanıcı hesabı ile birlikte yönetici izinleri.

> [!NOTE]
> Azure AD sağlama tümleştirmesi, temel bir [Stone OnDemand Web hizmetini](https://help.csod.com/help/csod_0/Content/Resources/Documents/WebServices/CSOD_-_Summary_of_Web_Services_v20151106.pdf)kullanır. Bu hizmet, temel bir Stone Ondemandekipleri tarafından kullanılabilir.

## <a name="add-cornerstone-ondemand-from-the-azure-marketplace"></a>Azure Marketi 'nden temel pul OnDemand ekleme

Azure AD ile otomatik Kullanıcı sağlama için köşeli temel taş OnDemand ' i yapılandırmadan önce Market 'ten yönetilen SaaS uygulamaları listenize temel bir kenar ekleyin.

Market 'ten temel taş OnDemand eklemek için aşağıdaki adımları izleyin.

1. [Azure Portal](https://portal.azure.com)sol taraftaki gezinti bölmesinde **Azure Active Directory**' i seçin.

    ![Azure Active Directory simgesi](common/select-azuread.png)

2. **Kurumsal uygulamalar**' a gidin ve **tüm uygulamalar**' ı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** ' yı seçin.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna, **köşeli kenar** kutucuğuna girin ve sonuç panelinden **köşeli kenar** kutucuğuna tıklayın. Uygulamayı eklemek için **Ekle**' yi seçin.

    ![Sonuç listesinde temel taş OnDemand](common/search-new-app.png)

## <a name="assign-users-to-cornerstone-ondemand"></a>Kullanıcıları, temel aşağı taşı OnDemand ata

Azure Active Directory seçili uygulamalara hangi kullanıcıların erişimi alacağını belirleyen *atama* adı verilen bir kavram kullanır. Otomatik Kullanıcı sağlama bağlamında, yalnızca Azure AD 'de bir uygulamaya atanan kullanıcılar veya gruplar eşitlenir.

Otomatik Kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD 'deki hangi kullanıcıların veya grupların, köşeli temel taşıdır OnDemand erişimine ihtiyacı olduğuna karar verin. Bu kullanıcıları veya grupları, [bir, bir kurumsal uygulamaya Kullanıcı veya Grup atama](../manage-apps/assign-user-or-group-access-portal.md)bölümündeki yönergeleri izleyin.

### <a name="important-tips-for-assigning-users-to-cornerstone-ondemand"></a>Temel olarak Kullanıcı atamaya yönelik önemli ipuçları

* Otomatik Kullanıcı sağlama yapılandırmasını test etmek için, bir Azure AD kullanıcısını en köşeli bir Azure AD kullanıcısı atamanız önerilir. Daha sonra ek kullanıcı veya grup atayabilirsiniz.

* Bir kullanıcıyı bir temel aşağı taşı OnDemand atadığınızda atama iletişim kutusunda varsa uygulamaya özgü geçerli herhangi bir rolü seçin. **Varsayılan erişim** rolüne sahip kullanıcılar, sağlanmasından çıkarılır.

## <a name="configure-automatic-user-provisioning-to-cornerstone-ondemand"></a>Otomatik Kullanıcı sağlamayı, köşeli temel bir OnDemand yapılandırma

Bu bölüm, Azure AD sağlama hizmetini yapılandırma adımlarında size rehberlik eder. Azure AD 'de Kullanıcı veya grup atamalarına bağlı olarak, temel bir kullanıcı veya grupları oluşturmak, güncelleştirmek ve devre dışı bırakmak için kullanın.

Azure AD 'de temel pul OnDemand için otomatik Kullanıcı sağlamayı yapılandırmak üzere aşağıdaki adımları izleyin.

1. [Azure Portal](https://portal.azure.com) oturum açın. **Tüm uygulamalar** > **Kurumsal uygulamalar** > **köşeli**seçim seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde, **köşeli kenar taşı**' yı seçin.

    ![Uygulamalar listesindeki temel taş OnDemand bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Temel taş OnDemand sağlama](./media/cornerstone-ondemand-provisioning-tutorial/ProvisioningTab.png)

4. **Sağlama modunu** **Otomatik**olarak ayarlayın.

    ![Temel taş OnDemand sağlama modu](./media/cornerstone-ondemand-provisioning-tutorial/ProvisioningCredentials.png)

5. **Yönetici kimlik bilgileri** bölümüne ait yönetici kullanıcı adını, yönetici parolasını ve etki alanını girin:

    * **Yönetici Kullanıcı adı** kutusunda, temel taş OnDemand kiracınızdaki yönetici hesabının etki alanını veya Kullanıcı adını girin. Contoso adminbir örnektir.

    * **Yönetici parolası** kutusuna yönetici kullanıcı adına karşılık gelen parolayı girin.

    * **Etki alanı** kutusunda, temel bir kutucuğuna ait Web hizmeti URL 'sini girin. Örneğin, hizmet `https://ws-[corpname].csod.com/feed30/clientdataservice.asmx`konumunda bulunur ve contoso için etki alanı `https://ws-contoso.csod.com/feed30/clientdataservice.asmx`. Web hizmeti URL 'sini alma hakkında daha fazla bilgi için [Bu PDF 'ye](https://help.csod.com/help/csod_0/Content/Resources/Documents/WebServices/CSOD_Web_Services_-_User-OU_Technical_Specification_v20160222.pdf)bakın.

6. Adım 5 ' te gösterilen kutuları doldurduktan sonra, Azure AD 'nin köşeli-açı OnDemand bağlantı kuramadığından emin olmak için **Bağlantıyı Sına** ' yı seçin. Bağlantı başarısız olursa, temel Stone OnDemand hesabınızın yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Temel taş OnDemand test bağlantısı](./media/cornerstone-ondemand-provisioning-tutorial/TestConnection.png)

7. **Bildirim e-postası** kutusunda, sağlama hatası bildirimlerini alacak kişinin veya grubun e-posta adresini girin. **Bir hata oluştuğunda e-posta bildirimi gönder** onay kutusunu seçin.

    ![Köşeli kenar, Ondemandnotification e-postası](./media/cornerstone-ondemand-provisioning-tutorial/EmailNotification.png)

8. **Kaydet**’i seçin.

9. **Eşlemeler** bölümü altında, Azure Active Directory kullanıcılarını, temel aşağı **Taşı OnDemand olacak şekilde eşitler**' ı seçin.

    ![Köşeli kenar, Ondemandsynchronization](./media/cornerstone-ondemand-provisioning-tutorial/UserMapping.png)

10. **Öznitelik eşlemeleri** bölümünde, Azure AD 'den temel alınan kullanıcı özniteliklerini bir şekilde gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme işlemleri Için temel taş OnDemand Kullanıcı hesaplarını eşleştirmek için kullanılır. Değişiklikleri kaydetmek için **Kaydet**' i seçin.

    ![Köşeli kenar, Ondemandattribute eşlemeleri](./media/cornerstone-ondemand-provisioning-tutorial/UserMappingAttributes.png)

11. Kapsam filtrelerini yapılandırmak için [kapsam filtresi öğreticisindeki](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)yönergeleri izleyin.

12. Azure AD sağlama hizmeti ' ni temel taş OnDemand için etkinleştirmek üzere, **Ayarlar** bölümünde **sağlama durumunu** **Açık**olarak değiştirin.

    ![Temel taş OnDemand sağlama durumu](./media/cornerstone-ondemand-provisioning-tutorial/ProvisioningStatus.png)

13. Temel pul OnDemand sağlamak istediğiniz kullanıcıları veya grupları tanımlayın. **Ayarlar** bölümünde, **kapsam**içinde istediğiniz değerleri seçin.

    ![Köşeli kenar, Ondemandscope](./media/cornerstone-ondemand-provisioning-tutorial/SyncScope.png)

14. Sağlamaya hazırsanız **Kaydet**' i seçin.

    ![Temel taş OnDemand Kaydet](./media/cornerstone-ondemand-provisioning-tutorial/Save.png)

Bu işlem, **Ayarlar** bölümünde **kapsamda** tanımlanan tüm kullanıcıların veya grupların ilk eşitlemesini başlatır. İlk eşitlemenin daha sonra eşitlenmesi daha uzun sürer. Azure AD sağlama hizmeti çalıştığı sürece yaklaşık 40 dakikada bir gerçekleşir. 

İlerleme durumunu izlemek ve sağlama etkinliği raporunun bağlantılarını izlemek için **eşitleme ayrıntıları** bölümünü kullanabilirsiniz. Rapor, Azure AD sağlama hizmeti tarafından, köşeli temel Stone OnDemand üzerinde gerçekleştirilen tüm eylemleri açıklar.

Azure AD sağlama günlüklerini okuma hakkında daha fazla bilgi için bkz. [Otomatik Kullanıcı hesabı sağlama hakkında raporlama](../app-provisioning/check-status-user-account-provisioning.md).

## <a name="connector-limitations"></a>Bağlayıcı sınırlamaları

Temel taş OnDemand **konum** özniteliği, temel taş OnDemand portalındaki rollere karşılık gelen bir değer bekler. Geçerli **konum** değerlerinin bir listesini almak için, temel gider OnDemand portalındaki **> kuruluş yapısı > Kullanıcı kaydını Düzenle** ' ye gidin.

![Temel taş OnDemand sağlama Kullanıcı kaydını düzenleme](./media/cornerstone-ondemand-provisioning-tutorial/UserEdit.png)

![Temel taş OnDemand sağlama konumu](./media/cornerstone-ondemand-provisioning-tutorial/UserPosition.png)

![Temel taş OnDemand sağlama konumu listesi](./media/cornerstone-ondemand-provisioning-tutorial/PostionId.png)

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal uygulamalar için Kullanıcı hesabı sağlamayı yönetme](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri İnceleme ve sağlama etkinliğinde rapor alma hakkında bilgi edinin](../app-provisioning/check-status-user-account-provisioning.md)

<!--Image references-->
[1]: ./media/cornerstone-ondemand-provisioning-tutorial/tutorial_general_01.png
[2]: ./media/cornerstone-ondemand-provisioning-tutorial/tutorial_general_02.png
[3]: ./media/cornerstone-ondemand-provisioning-tutorial/tutorial_general_03.png
