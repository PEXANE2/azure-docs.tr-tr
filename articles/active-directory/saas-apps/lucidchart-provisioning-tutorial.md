---
title: 'Öğretici: LucidChart için kullanıcı sağlama - Azure AD'
description: Azure Active Directory'yi, kullanıcı hesaplarını LucidChart'a otomatik olarak sağlamak ve sağlamadan çıkarmak için nasıl yapılandırılamayı öğrenin.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77057337"
---
# <a name="tutorial-configure-lucidchart-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için LucidChart'ı yapılandırın

Bu öğreticinin amacı, Azure AD'den LucidChart'a kullanıcı hesaplarını otomatik olarak sağlamak ve sağlamadan çıkarmak için LucidChart ve Azure AD'de gerçekleştirmeniz gereken adımları size göstermektir. 

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, zaten aşağıdaki öğelere sahip olduğunuzu varsayar:

* Azure Etkin dizin kiracı
* [Kurumsal planı](https://www.lucidchart.com/user/117598685#/subscriptionLevel) olan veya daha iyi etkin leştirilmiş bir LucidChart kiracısı
* Yönetici izinleri olan LucidChart'ta bir kullanıcı hesabı

## <a name="assigning-users-to-lucidchart"></a>Kullanıcıları LucidChart'a atama

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişmesi gerektiğini belirlemek için "atamalar" adlı bir kavram kullanır. Otomatik kullanıcı hesabı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya "atanmış" kullanıcılar ve gruplar eşitlenir.

Sağlama hizmetini yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki kullanıcıların ve/veya grupların LucidChart uygulamanıza erişilmesi gereken kullanıcıları temsil edeceğine karar vermeniz gerekir. Karar verildikten sonra, bu kullanıcıları burada talimatları izleyerek LucidChart uygulamanıza atayabilirsiniz:

[Bir kurumsal uygulamaya kullanıcı veya grup atama](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-lucidchart"></a>Kullanıcıları LucidChart'a atamak için önemli ipuçları

* Sağlama yapılandırmasını sınamak için LucidChart'a tek bir Azure AD kullanıcısı atanması önerilir. Ek kullanıcılar ve/veya gruplar daha sonra atanabilir.

* Bir kullanıcıyı LucidChart'a atarken, atama iletişim kutusunda **Kullanıcı** rolünü veya uygulamaya özgü başka bir geçerli rolü (varsa) seçmeniz gerekir. **Varsayılan Erişim** rolü sağlama için çalışmaz ve bu kullanıcılar atlanır.

## <a name="configuring-user-provisioning-to-lucidchart"></a>Kullanıcı sağlamayı LucidChart olarak yapılandırma

Bu bölüm, Azure REKLAM'ınızı LucidChart'ın kullanıcı hesabı sağlama API'sine bağlamanız ve sağlama hizmetini, Azure AD'deki kullanıcı ve grup atamasına dayalı olarak LucidChart'ta atanmış kullanıcı hesapları oluşturacak, güncelleştirecek ve devre dışı edecek şekilde yapılandırmanız konusunda size rehberlik eder.

> [!TIP]
> [Azure portalında](https://portal.azure.com)sağlanan yönergeleri izleyerek LucidChart için SAML tabanlı Tek Oturum Açma özelliğini de etkinleştirmeyi seçebilirsiniz. Tek oturum açma, otomatik sağlamadan bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini tamamlar.

### <a name="configure-automatic-user-account-provisioning-to-lucidchart-in-azure-ad"></a>Azure AD'de LucidChart'a otomatik kullanıcı hesabı sağlama yapılandırma

1. Azure [portalında,](https://portal.azure.com)Tüm uygulamalar > **Azure Active Directory > Enterprise Apps'a** göz atın.

2. LucidChart'ı zaten tek oturum açma için yapılandırıldıysanız, arama alanını kullanarak LucidChart örneğinizi arayın. Aksi takdirde, uygulama galerisinde **LucidChart'ı ekle** ve arama'yı seçin. **LucidChart** Arama sonuçlarından LucidChart'ı seçin ve uygulama listenize ekleyin.

3. LucidChart örneğini seçin ve ardından **Sağlama** sekmesini seçin.

4. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

    ![LucidChart Sağlama](./media/lucidchart-provisioning-tutorial/LucidChart1.png)

5. Yönetici **Kimlik Bilgileri** bölümü ne sin, LucidChart hesabınız tarafından oluşturulan **Gizli Belirteç'i** girdi (belirteci hesabınızın altında bulabilirsiniz: **Takım** > **Uygulaması Tümleştirme** > **SCIM).**

    ![LucidChart Sağlama](./media/lucidchart-provisioning-tutorial/LucidChart2.png)

6. Azure portalında, Azure AD'nin LucidChart uygulamanıza bağlanabilmesini sağlamak için **Test Bağlantısı'nı** tıklatın. Bağlantı başarısız olursa, LucidChart hesabınızın Yönetici izinlerine sahip olduğundan emin olun ve 5.

7. **Bildirim E-posta** alanında ödeme hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve onay kutusunu işaretleyin "Bir hata oluştuğunda e-posta bildirimi gönderin."

8. **Kaydet**'e tıklayın.

9. Eşlemeler bölümünde, **Azure Etkin Dizin Kullanıcılarını LucidChart'a Senkronize Et'i**seçin.

10. **Öznitelik Eşlemeleri** bölümünde, Azure AD'den LucidChart'a senkronize edilen kullanıcı özniteliklerini gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için LucidChart'taki kullanıcı hesaplarıyla eşleştirilmesi için kullanılır. Herhangi bir değişiklik yapmak için Kaydet düğmesini seçin.

11. LucidChart için Azure AD sağlama hizmetini etkinleştirmek **için, Ayarlar** bölümünde **KiSAma Durumunu** **Açık** olarak değiştirin

12. **Kaydet**'e tıklayın.

Bu işlem, Kullanıcılar ve Gruplar bölümünde LucidChart'a atanan kullanıcıların ve/veya grupların ilk eşitlemibaşlar. İlk eşitlemenin gerçekleştirilemi, hizmet yürütülürken yaklaşık her 40 dakikada bir meydana gelen sonraki eşitlemelerden daha uzun sürüyor. İlerlemeyi izlemek ve sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklayan sağlama etkinlik günlüklerine olan bağlantıları izlemek için **Eşitleme Ayrıntıları** bölümünü kullanabilirsiniz.

Azure AD sağlama günlüklerini nasıl okuyabilirsiniz hakkında daha fazla bilgi için [bkz.](../app-provisioning/check-status-user-account-provisioning.md)

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı sağlamanın yönetimi](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri nasıl inceleyip sağlama etkinliği yle ilgili raporları nasıl alacağınızı öğrenin](../app-provisioning/check-status-user-account-provisioning.md)
