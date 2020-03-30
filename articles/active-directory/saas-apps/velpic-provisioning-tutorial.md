---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için Velpic'i yapılandırma | Microsoft Dokümanlar"
description: Azure Active Directory'yi, kullanıcı hesaplarını Velpic'e otomatik olarak sağlamak ve sağlamadan çıkarmak için nasıl yapılandırılamayı öğrenin.
services: active-directory
documentationcenter: ''
author: zhchia
writer: zhchia
manager: beatrizd-msft
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2019
ms.author: zhchia
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9b7a6c2c9b7ecb0b160f7481d95f7682f3f7a109
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77064130"
---
# <a name="tutorial-configuring-velpic-for-automatic-user-provisioning"></a>Öğretici: Otomatik Kullanıcı Sağlama için Velpic Yapılandırma

Bu öğreticinin amacı, Azure AD'den Velpic'e kullanıcı hesaplarını otomatik olarak sağlamak ve sağlamadan sağlamak için Velpic ve Azure AD'de gerçekleştirmeniz gereken adımları size göstermektir.

> [!NOTE]
> Bu öğretici, Azure AD Kullanıcı Sağlama Hizmeti'nin üzerine inşa edilmiş bir bağlayıcıyı açıklar. Bu hizmetin ne yaptığı, nasıl çalıştığı ve sık sorulan sorular hakkında önemli ayrıntılar [için](../app-provisioning/user-provisioning.md)bkz.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, zaten aşağıdaki öğelere sahip olduğunuzu varsayar:

* Azure Active Directory kiracısı
* [Kurumsal planı](https://www.velpic.com/pricing.html) olan veya daha iyi etkin leştirilmiş bir Velpic kiracı
* Yönetici izinleri olan Velpic'te bir kullanıcı hesabı

## <a name="assigning-users-to-velpic"></a>Velpic'e kullanıcı atama

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişmesi gerektiğini belirlemek için "atamalar" adlı bir kavram kullanır. Otomatik kullanıcı hesabı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya "atanmış" kullanıcılar ve gruplar eşitlenir. 

Sağlama hizmetini yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki kullanıcıların ve/veya grupların Velpic uygulamanıza erişilmesi gereken kullanıcıları temsil edeceğine karar vermeniz gerekir. Karar verildikten sonra, bu kullanıcıları buradaki talimatları izleyerek Velpic uygulamanıza atayabilirsiniz:

[Bir kurumsal uygulamaya kullanıcı veya grup atama](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-velpic"></a>Velpic'e kullanıcı atamak için önemli ipuçları

* Sağlama yapılandırmasını sınamak için Tek bir Azure AD kullanıcısının Velpic'e atanması önerilir. Ek kullanıcılar ve/veya gruplar daha sonra atanabilir.

* Velpic'e bir kullanıcı atarken, atama iletişim kutusunda **Kullanıcı** rolünü veya uygulamaya özgü başka bir geçerli rolü (varsa) seçmeniz gerekir. **Varsayılan Erişim** rolünün sağlama için çalışmadığını ve bu kullanıcıların atlanacağını unutmayın.

## <a name="configuring-user-provisioning-to-velpic"></a>Kullanıcı sağlamayı Velpic'e yapılandırma

Bu bölüm, Azure REKLAM'ınızı Velpic'in kullanıcı hesabı sağlama API'sine bağlamanız ve sağlama hizmetini Azure AD'deki kullanıcı ve grup atamasına göre Velpic'te atanmış kullanıcı hesapları oluşturacak, güncelleştirecek ve devre dışı edecek şekilde yapılandırmanız konusunda size rehberlik eder.

> [!TIP]
> [Azure portalında](https://portal.azure.com)sağlanan yönergeleri izleyerek Velpic için SAML tabanlı Tek Oturum Açma özelliğini de etkinleştirmeyi seçebilirsiniz. Tek oturum açma, otomatik sağlamadan bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini tamamlar.

### <a name="to-configure-automatic-user-account-provisioning-to-velpic-in-azure-ad"></a>Azure AD'de Otomatik kullanıcı hesabı sağlamayı Velpic'e yapılandırmak için:

1. Azure [portalında,](https://portal.azure.com)Tüm uygulamalar > **Azure Active Directory > Enterprise Apps'a** göz atın.

2. Velpic'i tek oturum açma için zaten yapılandırıldıysanız, arama alanını kullanarak Velpic örneğini arayın. Aksi takdirde, uygulama galerisinde **Velpic'i** **ekle** ve arama'yı seçin. Arama sonuçlarından Velpic'i seçin ve uygulama listenize ekleyin.

3. Velpic örneğini seçin ve ardından **Sağlama** sekmesini seçin.

4. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

    ![Velpic Provizyon](./media/velpic-provisioning-tutorial/Velpic1.png)

5. Yönetici **Kimlik Bilgileri** bölümüne, Kiracı URL'si **&Velpic Gizli Belirteci'ni** girdi. (Bu değerleri Velpic hesabınızaltında bulabilirsiniz: > **Tümleştirme** > **Eklentisini** >  **YÖNET)****SCIM**

    ![Yetki Değerleri](./media/velpic-provisioning-tutorial/Velpic2.png)

6. Azure portalında, Azure AD'nin Velpic uygulamanıza bağlanabilmesini sağlamak için **Test Bağlantısı'nı** tıklatın. Bağlantı başarısız olursa, Velpic hesabınızda Yönetici izinleri olduğundan emin olun ve 5.

7. **Bildirim E-posta** alanında ödeme hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve aşağıdaki onay kutusunu işaretleyin.

8. **Kaydet**'e tıklayın.

9. Eşlemeler bölümünde, **Azure Etkin Dizin Kullanıcılarını Velpic'e Senkronize Et'i**seçin.

10. **Öznitelik Eşlemeleri** bölümünde, Azure AD'den Velpic'e eşitlenecek kullanıcı özniteliklerini gözden geçirin. **Eşleşme** özellikleri olarak seçilen özniteliklerin, güncelleştirme işlümi şuandırıları için Velpic' teki kullanıcı hesaplarıyla çalışmak için kullanıla Herhangi bir değişiklik yapmak için Kaydet düğmesini seçin.

11. Velpic için Azure AD sağlama hizmetini etkinleştirmek **için, Ayarlar** bölümünde **Sağlama Durumunu** **Ayarı'nda**

12. **Kaydet**'e tıklayın.

Bu, Kullanıcılar ve Gruplar bölümünde Velpic'e atanan kullanıcıların ve/veya grupların ilk senkronizasyonunu başlatır. İlk eşitlemenin, hizmet çalışırken yaklaşık her 40 dakikada bir oluşan sonraki eşitlemelerden daha uzun süreceğini unutmayın. İlerlemeyi izlemek ve sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklayan sağlama faaliyet raporlarının bağlantılarını izlemek için **Eşitleme Ayrıntıları** bölümünü kullanabilirsiniz.

Azure AD sağlama günlüklerini nasıl okuyabilirsiniz hakkında daha fazla bilgi için [bkz.](../app-provisioning/check-status-user-account-provisioning.md)

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı sağlamanın yönetimi](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri nasıl inceleyip sağlama etkinliği yle ilgili raporları nasıl alacağınızı öğrenin](../app-provisioning/check-status-user-account-provisioning.md)