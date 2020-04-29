---
title: 'Öğretici: LucidChart için Kullanıcı hazırlama-Azure AD'
description: LucidChart 'ye Kullanıcı hesaplarını otomatik olarak sağlamak ve sağlamak üzere Azure Active Directory yapılandırmayı öğrenin.
services: active-directory
documentationcenter: ''
author: ArvindHarinder1
manager: CelesteDG
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2019
ms.author: arvinh
ms.collection: M365-identity-device-management
ms.openlocfilehash: c5d946c6e257c7676178f9bc3c234f66ba6fe622
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77057337"
---
# <a name="tutorial-configure-lucidchart-for-automatic-user-provisioning"></a>Öğretici: otomatik Kullanıcı hazırlama için LucidChart yapılandırma

Bu öğreticinin amacı, Azure AD 'den LucidChart 'ye Kullanıcı hesaplarını otomatik olarak sağlamak ve devre dışı bırakmak için LucidChart ve Azure AD 'de gerçekleştirmeniz gereken adımları gösteriyoruz. 

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdaki öğelerin zaten olduğunu varsayar:

* Azure Active Directory kiracısı
* [Kurumsal plana](https://www.lucidchart.com/user/117598685#/subscriptionLevel) sahip bir Lucidchart kiracısı veya daha iyi etkin
* Yönetici izinlerine sahip LucidChart 'de bir kullanıcı hesabı

## <a name="assigning-users-to-lucidchart"></a>LucidChart 'e Kullanıcı atama

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişim alacağını belirleyebilmek için "atamalar" adlı bir kavram kullanır. Otomatik Kullanıcı hesabı sağlama bağlamında, yalnızca Azure AD 'de bir uygulamaya "atanmış" olan kullanıcılar ve gruplar eşitlenir.

Sağlama hizmetini yapılandırmadan ve etkinleştirmeden önce, Azure AD 'deki hangi kullanıcıların ve/veya grupların LucidChart uygulamanıza erişmesi gereken kullanıcıları temsil ettiğini belirlemeniz gerekir. Karar verdikten sonra buradaki yönergeleri izleyerek bu kullanıcıları LucidChart uygulamanıza atayabilirsiniz:

[Kurumsal uygulamaya Kullanıcı veya Grup atama](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-lucidchart"></a>LucidChart 'e Kullanıcı atamaya yönelik önemli ipuçları

* Sağlama yapılandırmasını test etmek için LucidChart 'e tek bir Azure AD kullanıcısının atanması önerilir. Ek kullanıcılar ve/veya grupları daha sonra atanabilir.

* Bir kullanıcıyı LucidChart 'e atarken, atama iletişim kutusunda **Kullanıcı** rolünü veya geçerli uygulamaya özgü bir rolü (varsa) seçmeniz gerekir. **Varsayılan erişim** rolü sağlama için çalışmaz ve bu kullanıcılar atlanır.

## <a name="configuring-user-provisioning-to-lucidchart"></a>LucidChart için Kullanıcı sağlamayı yapılandırma

Bu bölümde, Azure AD 'nizi LucidChart 'in Kullanıcı hesabı sağlama API 'sine bağlama ve sağlama hizmetini, Azure AD 'de Kullanıcı ve grup atamasını temel alan LucidChart içindeki atanmış kullanıcı hesaplarını oluşturmak, güncelleştirmek ve devre dışı bırakmak için yapılandırma adımları kılavuzluk eder.

> [!TIP]
> Ayrıca, [Azure Portal](https://portal.azure.com)' de sağlanan yönergeleri Izleyerek, LUCIDCHART için SAML tabanlı çoklu oturum açmayı da tercih edebilirsiniz. Çoklu oturum açma özelliği otomatik sağlanmadan bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini karmaşıdirebilirler.

### <a name="configure-automatic-user-account-provisioning-to-lucidchart-in-azure-ad"></a>Azure AD 'de LucidChart 'e otomatik Kullanıcı hesabı sağlamayı yapılandırma

1. [Azure portal](https://portal.azure.com) **Azure Active Directory > Enterprise Apps > tüm uygulamalar** bölümüne gidin.

2. Çoklu oturum açma için LucidChart zaten yapılandırdıysanız, arama alanını kullanarak LucidChart örneğinizi arayın. Aksi takdirde, **Ekle** ' yi seçin ve uygulama galerisinde **Lucidchart** için arama yapın. Arama sonuçlarından LucidChart ' ı seçin ve uygulama listenize ekleyin.

3. LucidChart örneğinizi seçin, sonra **sağlama** sekmesini seçin.

4. **Sağlama modunu** **Otomatik**olarak ayarlayın.

    ![LucidChart sağlama](./media/lucidchart-provisioning-tutorial/LucidChart1.png)

5. **Yönetici kimlik bilgileri** bölümünde, Lucidchart hesabı tarafından **oluşturulan gizli dizi belirtecini** girin (bu belirteci hesabınız altında bulabilirsiniz: **Team** > **App Integration** > **SCIM**).

    ![LucidChart sağlama](./media/lucidchart-provisioning-tutorial/LucidChart2.png)

6. Azure portal, Azure AD 'nin LucidChart uygulamanıza bağlanabildiğinden emin olmak için **Bağlantıyı Sına** ' ya tıklayın. Bağlantı başarısız olursa, LucidChart hesabınızın yönetici izinlerine sahip olduğundan emin olun ve 5. adımı yeniden deneyin.

7. **Bildirim e-postası** alanında sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve "bir hata oluştuğunda e-posta bildirimi gönder" onay kutusunu işaretleyin.

8. **Kaydet**’e tıklayın.

9. Eşlemeler bölümünde **Azure Active Directory Kullanıcıları Lucidchart olarak eşitler**' ı seçin.

10. **Öznitelik eşlemeleri** bölümünde, Azure AD 'den Lucidchart 'e eşitlenen Kullanıcı özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemleri için Lucidchart içindeki kullanıcı hesaplarını eşleştirmek için kullanılır. Değişiklikleri uygulamak için Kaydet düğmesini seçin.

11. LucidChart için Azure AD sağlama hizmetini etkinleştirmek üzere **Ayarlar** bölümünde **sağlama durumunu** **Açık** olarak değiştirin

12. **Kaydet**’e tıklayın.

Bu işlem, kullanıcılar ve Gruplar bölümünde LucidChart 'e atanan tüm Kullanıcı ve/veya grupların ilk eşitlemesini başlatır. İlk eşitlemenin daha sonra, hizmetin çalıştığı sürece yaklaşık 40 dakikada bir oluşan sonraki eşitlemeler yerine gerçekleştirilmesi daha uzun sürer. İlerleme durumunu izlemek ve sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklayan etkinlik günlüklerinin sağlanması için bağlantıları izlemek üzere **eşitleme ayrıntıları** bölümünü kullanabilirsiniz.

Azure AD sağlama günlüklerinin nasıl okunduğu hakkında daha fazla bilgi için bkz. [Otomatik Kullanıcı hesabı sağlamayı raporlama](../app-provisioning/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal uygulamalar için Kullanıcı hesabı sağlamayı yönetme](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri İnceleme ve sağlama etkinliğinde rapor alma hakkında bilgi edinin](../app-provisioning/check-status-user-account-provisioning.md)
