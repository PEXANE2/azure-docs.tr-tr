---
title: 'Öğretici: Azure Active Directory ile otomatik Kullanıcı sağlama için MerchLogix yapılandırma | Microsoft Docs'
description: Kullanıcı hesaplarını MerchLogix 'e otomatik olarak sağlamak ve sağlamak üzere Azure Active Directory nasıl yapılandıracağınızı öğrenin.
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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77061344"
---
# <a name="tutorial-configure-merchlogix-for-automatic-user-provisioning"></a>Öğretici: otomatik Kullanıcı sağlama için MerchLogix yapılandırma

Bu öğreticinin amacı, Azure AD 'yi kullanıcıları ve/veya grupları MerchLogix 'e otomatik olarak sağlamak ve devre dışı bırakmak üzere yapılandırmak için MerchLogix ve Azure Active Directory (Azure AD) içinde gerçekleştirilecek adımları göstermektir.

> [!NOTE]
> Bu öğreticide, Azure AD Kullanıcı sağlama hizmeti ' nin üzerine oluşturulmuş bir bağlayıcı açıklanmaktadır. Bu hizmetin ne yaptığını, nasıl çalıştığını ve sık sorulan soruları hakkında önemli ayrıntılar için bkz. [Azure Active Directory Ile SaaS uygulamalarına Kullanıcı sağlamayı ve sağlamayı kaldırmayı otomatikleştirme](../app-provisioning/user-provisioning.md).

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdaki önkoşulların zaten olduğunu varsayar:

* Bir Azure AD kiracısı
* MerchLogix kiracısı
* MerchLogix 'de, Kullanıcı sağlaması için gereken SCıM uç nokta URL 'SI ve gizli belirteç sağlayabilen teknik bir iletişim

## <a name="adding-merchlogix-from-the-gallery"></a>Galeriden MerchLogix ekleme

Azure AD ile otomatik Kullanıcı sağlama için MerchLogix ' i yapılandırmadan önce Azure AD uygulama galerisindeki yönetilen SaaS uygulamaları listenize MerchLogix eklemeniz gerekir.

**Azure AD uygulama galerisinden MerchLogix eklemek için aşağıdaki adımları uygulayın:**

1. **[Azure Portal](https://portal.azure.com)** sol gezinti panelinde **Azure Active Directory** simgesine tıklayın. 

    ![Azure Active Directory düğmesi][1]

2. **Kurumsal uygulamalar** > **tüm uygulamalar**' a gidin.

    ![Kurumsal uygulamalar bölümü][2]

3. MerchLogix eklemek için, iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesine tıklayın.

    ![Yeni uygulama düğmesi][3]

4. Arama kutusuna **Merchlogix**yazın.

5. Sonuçlar panelinde, **Merchlogix**' i seçin ve sonra SaaS uygulamaları listenize MerchLogix eklemek için **Ekle** düğmesine tıklayın.

    ![MerchLogix sağlama][4]

## <a name="assigning-users-to-merchlogix"></a>MerchLogix 'e Kullanıcı atama

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişim alacağını belirleyebilmek için "atamalar" adlı bir kavram kullanır. Otomatik Kullanıcı sağlama bağlamında, yalnızca Azure AD 'de bir uygulamaya "atanmış" olan kullanıcılar ve/veya gruplar eşitlenir. 

Otomatik Kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD 'deki hangi kullanıcıların ve/veya grupların MerchLogix erişimine ihtiyacı olduğuna karar vermeniz gerekir. Karar verdikten sonra buradaki yönergeleri izleyerek bu kullanıcıları ve/veya grupları MerchLogix 'e atayabilirsiniz:

* [Kurumsal uygulamaya Kullanıcı veya Grup atama](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-merchlogix"></a>MerchLogix 'e Kullanıcı atamaya yönelik önemli ipuçları

* İlk otomatik Kullanıcı sağlama yapılandırmanızı test etmek için MerchLogix öğesine tek bir Azure AD kullanıcısının atanması önerilir. Testler başarılı olduktan sonra ek kullanıcılar ve/veya gruplar daha sonra atanabilir.

* Bir kullanıcıyı MerchLogix 'e atarken, atama iletişim kutusunda uygulamaya özgü geçerli herhangi bir rolü (varsa) seçmeniz gerekir. **Varsayılan erişim** rolüne sahip kullanıcılar, sağlanmasından çıkarılır.

## <a name="configuring-automatic-user-provisioning-to-merchlogix"></a>MerchLogix için otomatik Kullanıcı sağlamayı yapılandırma 

Bu bölümde Azure AD sağlama hizmeti 'ni kullanarak Azure AD 'de Kullanıcı ve/veya grup atamalarını temel alan MerchLogix içindeki kullanıcıları ve/veya grupları oluşturma, güncelleştirme ve devre dışı bırakma adımları adım adım kılavuzluk eder.

> [!TIP]
> Merchlogix için SAML tabanlı çoklu oturum açmayı etkinleştirmeyi de tercih edebilirsiniz. Bu, [merchlogix çoklu oturum açma öğreticisinde](merchlogix-tutorial.md)sunulan yönergeleri izleyerek. Çoklu oturum açma, otomatik Kullanıcı sağlamasından bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini karmaşıdirebilirler.

### <a name="to-configure-automatic-user-provisioning-for-merchlogix-in-azure-ad"></a>Azure AD 'de MerchLogix için otomatik Kullanıcı sağlamayı yapılandırmak için:

1. [Azure Portal](https://portal.azure.com) oturum açın ve **tüm uygulamalar > > kurumsal uygulamalara Azure Active Directory**gidin.

2. SaaS uygulamaları listenizden MerchLogix ' i seçin.

3. **Sağlama** sekmesini seçin.

4. **Sağlama modunu** **Otomatik**olarak ayarlayın.

    ![MerchLogix sağlama](./media/merchlogix-provisioning-tutorial/Merchlogix1.png)

5. **Yönetici kimlik bilgileri** bölümünde:

    * **Kiracı URL 'si** alanında, MerchLogix Technical Contact 'niz tarafından sunulan SCIM uç nokta URL 'sini girin.

    * **Gizli belirteç** alanına, MerchLogix Technical Contact 'niz tarafından sunulan gizli belirteç girin.

6. 5. adımda gösterilen alanları doldurarak Azure AD ' ın MerchLogix ' e bağlanabildiğinden emin olmak için **Bağlantıyı Sına** ' ya tıklayın. Bağlantı başarısız olursa, MerchLogix hesabınızın yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

7. **Bildirim e-postası** alanına, sağlama hatası bildirimlerini alması gereken bir kişinin veya grubun e-posta adresini girin ve hata oluştuğunda onay kutusu- **e-posta bildirimi gönder**' i işaretleyin.

8. **Kaydet**’e tıklayın.

9. **Eşlemeler** bölümünde **Azure Active Directory Kullanıcıları MerchLogix ile eşitler**' ı seçin.

10. **Öznitelik eşleme** bölümünde Azure AD 'Den MerchLogix 'e eşitlenen Kullanıcı özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemleri Için MerchLogix içindeki kullanıcı hesaplarıyla eşleştirmek için kullanılır. Değişiklikleri uygulamak için **Kaydet** düğmesini seçin.

11. **Eşlemeler** bölümünde **Azure Active Directory gruplarını MerchLogix olarak eşitler**' ı seçin.

12. **Öznitelik eşleme** bölümünde Azure AD 'Den MerchLogix 'e eşitlenen grup özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemleri Için MerchLogix içindeki grupları eşleştirmek için kullanılır. Değişiklikleri uygulamak için **Kaydet** düğmesini seçin.

13. MerchLogix için Azure AD sağlama hizmetini etkinleştirmek üzere **Ayarlar** bölümünde **sağlama durumunu** **Açık** olarak değiştirin.

14. Sağlamaya hazırsanız **Kaydet**' e tıklayın.

Bu işlem, **Ayarlar** bölümünde **kapsam** içinde tanımlanan tüm kullanıcılar ve/veya grupların ilk eşitlemesini başlatır. İlk eşitlemenin daha sonra, Azure AD sağlama hizmeti çalıştığı sürece yaklaşık 40 dakikada bir oluşan sonraki eşitlemeler yerine gerçekleştirilmesi daha uzun sürer. **Eşitleme ayrıntıları** bölümünü Izleyip, MerchLogix ÜZERINDE Azure AD sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklayan sağlama etkinliği raporunu kullanabilirsiniz.

Azure AD sağlama günlüklerinin nasıl okunduğu hakkında daha fazla bilgi için bkz. [Otomatik Kullanıcı hesabı sağlamayı raporlama](../app-provisioning/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal uygulamalar için Kullanıcı hesabı sağlamayı yönetme](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri İnceleme ve sağlama etkinliğinde rapor alma hakkında bilgi edinin](../app-provisioning/check-status-user-account-provisioning.md)

<!--Image references-->
[1]: common/select-azuread.png
[2]: common/enterprise-applications.png
[3]: common/add-new-app.png
[4]: common/search-new-app.png
