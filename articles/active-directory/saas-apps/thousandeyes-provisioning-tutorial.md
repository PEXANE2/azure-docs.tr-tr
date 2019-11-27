---
title: 'Öğretici: ThousandEyes için Kullanıcı hazırlama-Azure AD'
description: ThousandEyes 'ye Kullanıcı hesaplarını otomatik olarak sağlamak ve sağlamak üzere Azure Active Directory yapılandırmayı öğrenin.
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
ms.date: 03/28/2019
ms.author: arvinh
ms.collection: M365-identity-device-management
ms.openlocfilehash: eaf019303c311519c4b7d483d8f9193f432b8385
ms.sourcegitcommit: 653e9f61b24940561061bd65b2486e232e41ead4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74278828"
---
# <a name="tutorial-configure-thousandeyes-for-automatic-user-provisioning"></a>Öğretici: otomatik Kullanıcı hazırlama için ThousandEyes yapılandırma

Bu öğreticinin amacı, Azure AD 'den ThousandEyes 'ye Kullanıcı hesaplarını otomatik olarak sağlamak ve devre dışı bırakmak için ThousandEyes ve Azure AD 'de gerçekleştirmeniz gereken adımları gösteriyoruz. 

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen senaryo, aşağıdaki öğelerin zaten olduğunu varsayar:

* Azure Active Directory kiracısı
* [Standart plan](https://www.thousandeyes.com/pricing) veya daha iyi etkinleştirilmiş bir ThousandEyes kiracısı 
* Yönetici izinlerine sahip ThousandEyes 'de bir kullanıcı hesabı 

> [!NOTE]
> Azure AD sağlama tümleştirmesi, standart plandaki veya daha iyi ThousandEyes takımlar tarafından kullanılabilen [ThousandEyes SCIM API](https://success.thousandeyes.com/PublicArticlePage?articleIdParam=kA044000000CnWrCAK)'sini kullanır.

## <a name="assigning-users-to-thousandeyes"></a>ThousandEyes 'e Kullanıcı atama

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişim alacağını belirleyebilmek için "atamalar" adlı bir kavram kullanır. Otomatik Kullanıcı hesabı sağlama bağlamında, yalnızca Azure AD 'de bir uygulamaya "atanmış" olan kullanıcılar ve gruplar eşitlenir. 

Sağlama hizmetini yapılandırmadan ve etkinleştirmeden önce, Azure AD 'deki hangi kullanıcıların ve/veya grupların ThousandEyes uygulamanıza erişmesi gereken kullanıcıları temsil ettiğini belirlemeniz gerekir. Karar verdikten sonra buradaki yönergeleri izleyerek bu kullanıcıları ThousandEyes uygulamanıza atayabilirsiniz:

[Kurumsal uygulamaya Kullanıcı veya Grup atama](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-thousandeyes"></a>ThousandEyes 'e Kullanıcı atamaya yönelik önemli ipuçları

* Sağlama yapılandırmasını test etmek için ThousandEyes 'e tek bir Azure AD kullanıcısının atanması önerilir. Ek kullanıcılar ve/veya grupları daha sonra atanabilir.

* Bir kullanıcıyı ThousandEyes 'e atarken, atama iletişim kutusunda **Kullanıcı** rolünü veya geçerli uygulamaya özgü bir rolü (varsa) seçmeniz gerekir. **Varsayılan erişim** rolü sağlama için çalışmaz ve bu kullanıcılar atlanır.

## <a name="configuring-user-provisioning-to-thousandeyes"></a>ThousandEyes için Kullanıcı sağlamayı yapılandırma 

Bu bölüm, Azure AD 'nizi ThousandEyes 'un Kullanıcı hesabı sağlama API 'sine bağlama ve sağlama hizmetini, Azure AD 'de Kullanıcı ve grup atamasını temel alan ThousandEyes içindeki atanmış kullanıcı hesaplarını oluşturmak, güncelleştirmek ve devre dışı bırakmak için yapılandırmanıza kılavuzluk eder. .

> [!TIP]
> Ayrıca, [Azure Portal](https://portal.azure.com)' de sağlanan yönergeleri Izleyerek, THOUSANDEYES için SAML tabanlı çoklu oturum açmayı da tercih edebilirsiniz. Çoklu oturum açma özelliği otomatik sağlanmadan bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini karmaşıdirebilirler.

### <a name="configure-automatic-user-account-provisioning-to-thousandeyes-in-azure-ad"></a>Azure AD 'de ThousandEyes 'e otomatik Kullanıcı hesabı sağlamayı yapılandırma

1. [Azure portal](https://portal.azure.com) **Azure Active Directory > Enterprise Apps > tüm uygulamalar** bölümüne gidin.

2. Çoklu oturum açma için ThousandEyes zaten yapılandırdıysanız, arama alanını kullanarak ThousandEyes örneğinizi arayın. Aksi takdirde, **Ekle** ' yi seçin ve uygulama galerisinde **ThousandEyes** için arama yapın. Arama sonuçlarından ThousandEyes ' ı seçin ve uygulama listenize ekleyin.

3. ThousandEyes örneğinizi seçin, sonra **sağlama** sekmesini seçin.

4. **Sağlama modunu** **Otomatik**olarak ayarlayın.

    ![ThousandEyes sağlama](./media/thousandeyes-provisioning-tutorial/ThousandEyes1.png)

5. **Yönetici kimlik bilgileri** bölümünde, ThousandEyes hesabı tarafından oluşturulan **OAuth taşıyıcı belirtecini** girin (ThousandEyes hesabı **profiliniz** bölümünde bir belirteç bulabilir ve oluşturabilirsiniz).

    ![ThousandEyes sağlama](./media/thousandeyes-provisioning-tutorial/ThousandEyes2.png)

6. Azure portal, Azure AD 'nin ThousandEyes uygulamanıza bağlanabildiğinden emin olmak için **Bağlantıyı Sına** ' ya tıklayın. Bağlantı başarısız olursa, ThousandEyes hesabınızın yönetici izinlerine sahip olduğundan emin olun ve 5. adımı yeniden deneyin.

7. **Bildirim e-postası** alanında sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve "bir hata oluştuğunda e-posta bildirimi gönder" onay kutusunu işaretleyin.

8. **Save (Kaydet)** düğmesine tıklayın.

9. Eşlemeler bölümünde **Azure Active Directory Kullanıcıları ThousandEyes olarak eşitler**' ı seçin.

10. **Öznitelik eşlemeleri** bölümünde, Azure AD 'den ThousandEyes 'e eşitlenen Kullanıcı özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemleri için ThousandEyes içindeki kullanıcı hesaplarını eşleştirmek için kullanılır. Değişiklikleri kaydetmek için Kaydet düğmesini seçin.

11. ThousandEyes için Azure AD sağlama hizmetini etkinleştirmek üzere **Ayarlar** bölümünde **sağlama durumunu** **Açık** olarak değiştirin

12. **Save (Kaydet)** düğmesine tıklayın.

Bu işlem, kullanıcılar ve Gruplar bölümünde ThousandEyes 'e atanan tüm Kullanıcı ve/veya grupların ilk eşitlemesini başlatır. İlk eşitleme hizmeti çalışıyor sürece yaklaşık 40 dakikada oluşan sonraki eşitlemeler uzun sürer. İlerleme durumunu izlemek ve sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklayan etkinlik günlüklerinin sağlanması için bağlantıları izlemek üzere **eşitleme ayrıntıları** bölümünü kullanabilirsiniz.

Azure AD sağlama günlüklerinin nasıl okunduğu hakkında daha fazla bilgi için bkz. [Otomatik Kullanıcı hesabı sağlamayı raporlama](../manage-apps/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal uygulamalar için Kullanıcı hesabı sağlamayı yönetme](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri İnceleme ve sağlama etkinliğinde rapor alma hakkında bilgi edinin](../manage-apps/check-status-user-account-provisioning.md)
