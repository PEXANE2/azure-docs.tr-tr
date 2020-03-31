---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için Cornerstone OnDemand'i yapılandırın | Microsoft Dokümanlar"
description: Azure Active Directory'yi, kullanıcı hesaplarını Cornerstone OnDemand'a otomatik olarak sağlamak ve yok etmek için nasıl yapılandırılamayı öğrenin.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77058456"
---
# <a name="tutorial-configure-cornerstone-ondemand-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için Cornerstone OnDemand'ı yapılandırın

Bu öğretici, Azure AD'yi kullanıcıları veya grupları Cornerstone OnDemand'a otomatik olarak sağlamak ve etkisiz leştirmek üzere yapılandırmak için Cornerstone OnDemand ve Azure Etkin Dizini'nde (Azure AD) gerçekleştirecek adımları gösterir.

> [!NOTE]
> Bu öğretici, Azure AD kullanıcı sağlama hizmetinin üzerine inşa edilmiş bir bağlayıcıyı açıklar. Bu hizmetin ne yaptığı, nasıl çalıştığı ve sık sorulan sorular hakkında bilgi için [bkz.](../app-provisioning/user-provisioning.md)

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdakilere sahip olduğunuzu varsayar:

* Azure AD kiracısı.
* Bir Cornerstone OnDemand kiracı.
* Yönetici izinleri olan Cornerstone OnDemand'deki bir kullanıcı hesabı.

> [!NOTE]
> Azure AD sağlama tümleştirmesi [Cornerstone OnDemand web hizmetine](https://help.csod.com/help/csod_0/Content/Resources/Documents/WebServices/CSOD_-_Summary_of_Web_Services_v20151106.pdf)dayanır. Bu hizmet Cornerstone OnDemand ekipleri tarafından kullanılabilir.

## <a name="add-cornerstone-ondemand-from-the-azure-marketplace"></a>Azure Marketinden Köşe Taşı OnDemand Ekleme

Azure AD ile otomatik kullanıcı sağlama için Cornerstone OnDemand'i yapılandırmadan önce, yönetilen SaaS uygulamaları listenize Market'ten Cornerstone OnDemand'i ekleyin.

Marketten Cornerstone OnDemand eklemek için aşağıdaki adımları izleyin.

1. Azure [portalında,](https://portal.azure.com)soldaki gezinti bölmesinde **Azure Etkin Dizini'ni**seçin.

    ![Azure Etkin Dizin simgesi](common/select-azuread.png)

2. Kurumsal **uygulamalara**gidin ve ardından **Tüm uygulamaları**seçin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için iletişim kutusunun üst kısmında **Yeni uygulama'yı** seçin.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda, **Cornerstone OnDemand'i** girin ve sonuç panelinden **Cornerstone OnDemand'i** seçin. Uygulamayı eklemek için **Ekle'yi**seçin.

    ![Sonuç listesinde köşe taşı OnDemand](common/search-new-app.png)

## <a name="assign-users-to-cornerstone-ondemand"></a>Kullanıcıları Cornerstone OnDemand'e atama

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişmesi gerektiğini belirlemek için *atamalar* adlı bir kavram kullanır. Otomatik kullanıcı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya atanan kullanıcılar veya gruplar eşitlenir.

Otomatik kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki hangi kullanıcıların veya grupların Cornerstone OnDemand'a erişmeye ihtiyacı olduğuna karar verin. Bu kullanıcıları veya grupları Cornerstone OnDemand'a atamak [için, bir kullanıcıyı veya grubu kurumsal bir uygulamaya atama](../manage-apps/assign-user-or-group-access-portal.md)yönergelerini izleyin.

### <a name="important-tips-for-assigning-users-to-cornerstone-ondemand"></a>Cornerstone OnDemand'a kullanıcı atamak için önemli ipuçları

* Otomatik kullanıcı sağlama yapılandırmasını test etmek için Cornerstone OnDemand'a tek bir Azure AD kullanıcısı atamanızı öneririz. Daha sonra ek kullanıcılar veya gruplar atayabilirsiniz.

* Bir kullanıcıyı Cornerstone OnDemand'a atadığınızda, atama iletişim kutusunda geçerli bir uygulamaya özgü rolü seçin. **Varsayılan Erişim** rolüne sahip kullanıcılar sağlama nın dışında tutulur.

## <a name="configure-automatic-user-provisioning-to-cornerstone-ondemand"></a>Otomatik kullanıcı sağlamayı Cornerstone OnDemand olarak yapılandırın

Bu bölüm, Azure AD sağlama hizmetini yapılandırma adımlarında size yol gösteriş eder. Azure AD'deki kullanıcı veya grup atamalarına dayalı olarak Cornerstone OnDemand'daki kullanıcıları veya grupları oluşturmak, güncelleştirmek ve devre dışı kalmak için kullanın.

Azure AD'de Cornerstone OnDemand için otomatik kullanıcı sağlama yapılandırmak için aşağıdaki adımları izleyin.

1. [Azure portalında](https://portal.azure.com)oturum açın. **Kurumsal uygulamaları** > seçin**Tüm uygulamalar** > **Cornerstone OnDemand**.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Cornerstone OnDemand'i**seçin.

    ![Uygulamalar listesindeki Cornerstone OnDemand bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Köşe Taşı OnDemand Sağlama](./media/cornerstone-ondemand-provisioning-tutorial/ProvisioningTab.png)

4. **Sağlama Modunu** **Otomatik**olarak ayarlayın.

    ![Köşe Taşı OnDemand Sağlama Modu](./media/cornerstone-ondemand-provisioning-tutorial/ProvisioningCredentials.png)

5. Yönetici **Kimlik Bilgileri** bölümünün altında, Cornerstone OnDemand hesabınızın yönetici kullanıcı adını, yönetici parolasını ve etki alanını girin:

    * Yönetici **Kullanıcı Adı** kutusunda, Cornerstone OnDemand kiracınızdaki yönetici hesabının etki alanını veya kullanıcı adını girin. Bir örnek contoso\admin olduğunu.

    * Yönetici **Parolası** kutusunda, yönetici kullanıcı adına karşılık gelen parolayı girin.

    * Etki **Alanı** kutusunda, Cornerstone OnDemand kiracısının web hizmeti URL'sini doldurun. Örneğin, hizmet `https://ws-[corpname].csod.com/feed30/clientdataservice.asmx`, ve Contoso için etki alanı `https://ws-contoso.csod.com/feed30/clientdataservice.asmx`bulunur . Web hizmeti URL'sinin nasıl alınacak sınanması hakkında daha fazla bilgi için [bu pdf'ye](https://help.csod.com/help/csod_0/Content/Resources/Documents/WebServices/CSOD_Web_Services_-_User-OU_Technical_Specification_v20160222.pdf)bakın.

6. Adım 5'te gösterilen kutuları doldurduktan sonra, Azure AD'nin Cornerstone OnDemand'a bağlanabilmesini sağlamak için **Test Bağlantısı'nı** seçin. Bağlantı başarısız olursa, Cornerstone OnDemand hesabınızın yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Köşe Taşı OnDemand Test Bağlantısı](./media/cornerstone-ondemand-provisioning-tutorial/TestConnection.png)

7. Bildirim **E-posta** kutusuna, sağlama hatası bildirimlerini almak için kişinin veya grubun e-posta adresini girin. Bir **hata olduğunda e-posta gönder bildirimini** seçin onay kutusunu seçin.

    ![Köşe Taşı OnDemand Bildirim E-posta](./media/cornerstone-ondemand-provisioning-tutorial/EmailNotification.png)

8. **Kaydet'i**seçin.

9. **Eşlemeler** bölümünde, **Azure Etkin Dizin Kullanıcılarını Cornerstone OnDemand'e Senkronize Et'i**seçin.

    ![Köşe taşı OnDemand senkronizasyonu](./media/cornerstone-ondemand-provisioning-tutorial/UserMapping.png)

10. **Öznitelik Eşlemeleri** bölümünde Azure AD'den Cornerstone OnDemand'a eşitlenen kullanıcı özniteliklerini gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Cornerstone OnDemand'deki kullanıcı hesaplarıyla eşleştirilmesi için kullanılır. Herhangi bir değişikliği kaydetmek için **Kaydet'i**seçin.

    ![Köşe Taşı OnDemand Öznitelik Eşlemeleri](./media/cornerstone-ondemand-provisioning-tutorial/UserMappingAttributes.png)

11. Kapsam filtrelerini yapılandırmak [için, kapsam filtresi](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)öğreticisindeki yönergeleri izleyin.

12. **Ayarlar** bölümünde, Cornerstone OnDemand için Azure AD sağlama hizmetini etkinleştirmek **için, Sağlama Durumunu** **A'da**olarak değiştirin.

    ![Köşe Taşı İsteği Sağlama Durumu](./media/cornerstone-ondemand-provisioning-tutorial/ProvisioningStatus.png)

13. Cornerstone OnDemand'a sağlamak istediğiniz kullanıcıları veya grupları tanımlayın. **Ayarlar** bölümünde, **Kapsam'ta**istediğiniz değerleri seçin.

    ![Köşe Taşı OnDemand Kapsamı](./media/cornerstone-ondemand-provisioning-tutorial/SyncScope.png)

14. Hükmetmeye hazır olduğunuzda **Kaydet'i**seçin.

    ![Köşe Taşı OnDemand Kaydet](./media/cornerstone-ondemand-provisioning-tutorial/Save.png)

Bu işlem, **Ayarlar** bölümünde **Kapsam'ta** tanımlanan tüm kullanıcıların veya grupların ilk eşitlemisini başlatır. İlk eşitlemenin gerçekleştirilemi daha sonraki eşitlemelerden daha uzun sürüyor. Azure AD sağlama hizmeti çalıştığı sürece yaklaşık her 40 dakikada bir oluşur. 

İlerlemeyi izlemek ve sağlama faaliyet raporuna bağlı bağlantıları izlemek için **Eşitleme Ayrıntıları** bölümünü kullanabilirsiniz. Rapor, Cornerstone OnDemand'deki Azure AD sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklar.

Azure AD sağlama günlüklerini nasıl okuyabilirsiniz hakkında bilgi için bkz: [Otomatik kullanıcı hesabı sağlama hakkında Raporlama.](../app-provisioning/check-status-user-account-provisioning.md)

## <a name="connector-limitations"></a>Konektör sınırlamaları

Cornerstone OnDemand **Position** özniteliği, Cornerstone OnDemand portalındaki rollere karşılık gelen bir değer bekler. Geçerli **Konum** değerlerinin listesini almak için, Cornerstone OnDemand portalındaki **Kullanıcı Kaydı > Organizasyon Yapısını > Konumunu Edin'e** gidin.

![Köşe Taşı OnDemand Sağlama Edit Kullanıcı Kaydı](./media/cornerstone-ondemand-provisioning-tutorial/UserEdit.png)

![Köşe Taşı İsteği Sağlama Pozisyonu](./media/cornerstone-ondemand-provisioning-tutorial/UserPosition.png)

![Köşe taşı OnDemand Sağlama pozisyon listesi](./media/cornerstone-ondemand-provisioning-tutorial/PostionId.png)

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal uygulamalar için kullanıcı hesabı sağlamayı yönetme](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri nasıl inceleyip sağlama etkinliği yle ilgili raporları nasıl alacağınızı öğrenin](../app-provisioning/check-status-user-account-provisioning.md)

<!--Image references-->
[1]: ./media/cornerstone-ondemand-provisioning-tutorial/tutorial_general_01.png
[2]: ./media/cornerstone-ondemand-provisioning-tutorial/tutorial_general_02.png
[3]: ./media/cornerstone-ondemand-provisioning-tutorial/tutorial_general_03.png
