---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için MerchLogix'i yapılandırın | Microsoft Dokümanlar"
description: Azure Active Directory'yi, kullanıcı hesaplarını MerchLogix'e otomatik olarak sağlamak ve sağlamadan çıkarmak için nasıl yapılandırılamayı öğrenin.
services: active-directory
documentationcenter: ''
author: zhchia
writer: zhchia
manager: beatrizd-msft
ms.assetid: 9df4c7c5-9a58-478e-93b7-2f77aae12807
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2019
ms.author: zhchia
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4aa60fb565552961a3c85346c39c318a90c8adc0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77061344"
---
# <a name="tutorial-configure-merchlogix-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için MerchLogix'i yapılandırın

Bu öğreticinin amacı, Azure AD'yi kullanıcıları ve/veya grupları MerchLogix'e otomatik olarak sağlayacak ve geçici olarak sağlayacak şekilde yapılandırmak için MerchLogix ve Azure Active Directory 'de (Azure AD) gerçekleştirilecek adımları göstermektir.

> [!NOTE]
> Bu öğretici, Azure AD Kullanıcı Sağlama Hizmeti'nin üzerine inşa edilmiş bir bağlayıcıyı açıklar. Bu hizmetin ne yaptığı, nasıl çalıştığı ve sık sorulan sorular hakkında önemli ayrıntılar [için](../app-provisioning/user-provisioning.md)bkz.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdaki ön koşullara sahip olduğunuzu varsayar:

* Azure AD kiracı
* Bir MerchLogix kiracı
* MerchLogix'te, SCIM uç noktası URL'sini ve kullanıcı sağlama için gerekli gizli belirteci sağlayabilecek teknik bir iletişim

## <a name="adding-merchlogix-from-the-gallery"></a>Galeriden MerchLogix ekleme

Azure AD ile otomatik kullanıcı sağlama için MerchLogix'i yapılandırmadan önce, Azure AD uygulama galerisinden MerchLogix'i yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Azure AD uygulama galerisinden MerchLogix eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Etkin Dizin** simgesine tıklayın. 

    ![Azure Etkin Dizin düğmesi][1]

2. Kurumsal **uygulamalara** > gidin**Tüm uygulamalar**.

    ![Kurumsal uygulamalar Bölümü][2]

3. MerchLogix eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi][3]

4. Arama kutusuna **MerchLogix**yazın.

5. Sonuç panelinde **MerchLogix'i**seçin ve ardından SaaS uygulamaları listenize MerchLogix eklemek için **Ekle** düğmesini tıklayın.

    ![MerchLogix Sağlama][4]

## <a name="assigning-users-to-merchlogix"></a>Kullanıcıları MerchLogix'e atama

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişmesi gerektiğini belirlemek için "atamalar" adlı bir kavram kullanır. Otomatik kullanıcı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya "atanmış" kullanıcılar ve/veya gruplar eşitlenir. 

Otomatik kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki hangi kullanıcıların ve/veya grupların MerchLogix'e erişmesi gerektiğine karar vermelisiniz. Karar verildikten sonra, bu kullanıcıları ve/veya grupları buradaki talimatları izleyerek MerchLogix'e atayabilirsiniz:

* [Bir kurumsal uygulamaya kullanıcı veya grup atama](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-merchlogix"></a>MerchLogix'e kullanıcı atamak için önemli ipuçları

* İlk otomatik kullanıcı sağlama yapılandırmanızı test etmek için MerchLogix'e tek bir Azure AD kullanıcısı atanması önerilir. Testler başarılı olduktan sonra ek kullanıcılar ve/veya gruplar atanabilir.

* Bir kullanıcıyı MerchLogix'e atarken, atama iletişim kutusunda uygulamaya özgü geçerli bir rolü (varsa) seçmeniz gerekir. **Varsayılan Erişim** rolüne sahip kullanıcılar sağlama nın dışında tutulur.

## <a name="configuring-automatic-user-provisioning-to-merchlogix"></a>MerchLogix için otomatik kullanıcı sağlama yapılandırma 

Bu bölüm, Azure AD'deki kullanıcı ve/veya grup atamalarına dayalı olarak MerchLogix'teki kullanıcıları ve/veya grupları oluşturmak, güncellemek ve devre dışı etmek için Azure AD sağlama hizmetini yapılandırma adımları boyunca size yol açar.

> [!TIP]
> [Ayrıca MerchLogix tek oturum](merchlogix-tutorial.md)açma öğreticisinde verilen talimatları izleyerek MerchLogix için SAML tabanlı tek oturum açmayı etkinleştirmeyi de seçebilirsiniz. Tek oturum açma, otomatik kullanıcı sağlamadan bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini tamamlar.

### <a name="to-configure-automatic-user-provisioning-for-merchlogix-in-azure-ad"></a>Azure AD'de MerchLogix için otomatik kullanıcı sağlama yapılandırmak için:

1. [Azure portalında](https://portal.azure.com) oturum açın ve **Azure Active Directory > Enterprise uygulamalarına > tüm uygulamalara**göz atın.

2. SaaS uygulamaları listenizden MerchLogix'i seçin.

3. **Sağlama** sekmesini seçin.

4. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

    ![MerchLogix Sağlama](./media/merchlogix-provisioning-tutorial/Merchlogix1.png)

5. Yönetici **Kimlik Bilgileri** bölümünde:

    * Kiracı **URL** alanına, MerchLogix teknik ilgili kişiniz tarafından sağlanan SCIM uç nokta URL'sini girin.

    * Gizli **Belirteç** alanında, MerchLogix teknik bağlantınız tarafından sağlanan gizli jetonu girin.

6. Adım 5'te gösterilen alanları doldurarak, Azure AD'nin MerchLogix'e bağlanabilmesini sağlamak için **Test Bağlantısı'nı** tıklatın. Bağlantı başarısız olursa, MerchLogix hesabınızın Yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

7. Bildirim **E-postası** alanında, sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve onay kutusunu işaretleyin - **Bir hata oluştuğunda e-posta bildirimi gönderin.**

8. **Kaydet**'e tıklayın.

9. **Eşlemeler** bölümünde, **Azure Etkin Dizin Kullanıcılarını MerchLogix**ile Senkronize Et'i seçin.

10. Azure AD'den MerchLogix'e eşitlenen kullanıcı özniteliklerini **Öznitelik Eşleme** bölümünde gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için MerchLogix'teki kullanıcı hesaplarıyla eşleştirilmesi için kullanılır. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

11. **Eşlemeler** bölümünde, **Azure Etkin Dizin Gruplarını MerchLogix'e Senkronize Et'i**seçin.

12. Azure AD'den MerchLogix'e eşitlenen grup özniteliklerini **Öznitelik Eşleme** bölümünde gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için MerchLogix'teki gruplarla eşleştirilmesi için kullanılır. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

13. MerchLogix için Azure AD sağlama hizmetini etkinleştirmek **için, Ayarlar** bölümünde **Sağlama Durumunu** **Ayarı** olarak değiştirin.

14. Hükmetmeye hazır olduğunuzda **Kaydet'i**tıklatın.

Bu işlem, **Ayarlar** bölümünde **Kapsam'ta** tanımlanan tüm kullanıcıların ve/veya grupların ilk eşitlemisini başlatır. Azure AD sağlama hizmeti nin çalıştırıldığı sürece yaklaşık her 40 dakikada bir gerçekleşen sonraki eşitlemelerden daha uzun süren ilk eşitlemenin gerçeklemi daha uzun sürer. MerchLogix'teki Azure AD sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklayan ilerlemeyi izlemek ve sağlama etkinlik raporuna olan bağlantıları izlemek için **Eşitleme Ayrıntıları** bölümünü kullanabilirsiniz.

Azure AD sağlama günlüklerini nasıl okuyabilirsiniz hakkında daha fazla bilgi için [bkz.](../app-provisioning/check-status-user-account-provisioning.md)

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı sağlamanın yönetimi](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri nasıl inceleyip sağlama etkinliği yle ilgili raporları nasıl alacağınızı öğrenin](../app-provisioning/check-status-user-account-provisioning.md)

<!--Image references-->
[1]: common/select-azuread.png
[2]: common/enterprise-applications.png
[3]: common/add-new-app.png
[4]: common/search-new-app.png
