---
title: 'Öğretici: ThousandEyes için kullanıcı sağlama - Azure AD'
description: Azure Active Directory'yi, kullanıcı hesaplarını ThousandEyes'e otomatik olarak sağlamak ve sağlamadan çıkarmak için nasıl yapılandırılamayı öğrenin.
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
ms.openlocfilehash: d87cffce636146eac3e557670ffc4fb2fc34ae38
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77062889"
---
# <a name="tutorial-configure-thousandeyes-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için ThousandEyes'i yapılandırın

Bu öğreticinin amacı, Azure AD'den ThousandEyes'e kullanıcı hesaplarını otomatik olarak sağlamak ve sağlamadan çıkarmak için ThousandEyes ve Azure AD'de gerçekleştirmeniz gereken adımları size göstermektir. 

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, zaten aşağıdaki öğelere sahip olduğunuzu varsayar:

* Azure Etkin dizin kiracı
* [Standart planı](https://www.thousandeyes.com/pricing) olan veya daha iyi etkin leştirilmiş bir ThousandEyes kiracısı 
* Yönetici izinleri ile ThousandEyes bir kullanıcı hesabı 

> [!NOTE]
> Azure AD sağlama tümleştirmesi, ThousandEyes ekiplerinin Standart planveya daha iyisi üzerinde kullanılabilen [ThousandEyes SCIM API'sine](https://success.thousandeyes.com/PublicArticlePage?articleIdParam=kA044000000CnWrCAK)dayanır.

## <a name="assigning-users-to-thousandeyes"></a>Kullanıcıları ThousandEyes'e atama

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişmesi gerektiğini belirlemek için "atamalar" adlı bir kavram kullanır. Otomatik kullanıcı hesabı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya "atanmış" kullanıcılar ve gruplar eşitlenir. 

Sağlama hizmetini yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki kullanıcıların ve/veya grupların ThousandEyes uygulamanıza erişilmesi gereken kullanıcıları temsil edeceğine karar vermeniz gerekir. Karar verildikten sonra, bu kullanıcıları thousandeyes uygulamanıza buradaki talimatları izleyerek atayabilirsiniz:

[Bir kurumsal uygulamaya kullanıcı veya grup atama](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-thousandeyes"></a>ThousandEyes'e kullanıcı atamak için önemli ipuçları

* Sağlama yapılandırmasını sınamak için ThousandEyes'e tek bir Azure AD kullanıcısı atanması önerilir. Ek kullanıcılar ve/veya gruplar daha sonra atanabilir.

* BinEyes'e bir kullanıcı atarken, atama iletişim kutusunda **Kullanıcı** rolünü veya uygulamaya özgü başka bir geçerli rolü (varsa) seçmeniz gerekir. **Varsayılan Erişim** rolü sağlama için çalışmaz ve bu kullanıcılar atlanır.

## <a name="configuring-user-provisioning-to-thousandeyes"></a>Kullanıcı sağlamayı ThousandEyes'e yapılandırma 

Bu bölüm, Azure REKLAM'ınızı ThousandEyes'in kullanıcı hesabı sağlama API'sine bağlamanız ve Azure AD'deki kullanıcı ve grup atamasına göre ThousandEyes'te atanan kullanıcı hesaplarını oluşturmak, güncelleştirmek ve devre dışı etmek için sağlama hizmetini yapılandırmak için size yol gösteriyor .

> [!TIP]
> [Azure portalında](https://portal.azure.com)sağlanan yönergeleri izleyerek ThousandEyes için SAML tabanlı Tek Oturum Açma özelliğini de etkinleştirmeyi seçebilirsiniz. Tek oturum açma, otomatik sağlamadan bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini tamamlar.

### <a name="configure-automatic-user-account-provisioning-to-thousandeyes-in-azure-ad"></a>Azure AD'de ThousandEyes'e otomatik kullanıcı hesabı sağlama yapılandırma

1. Azure [portalında,](https://portal.azure.com)Tüm uygulamalar > **Azure Active Directory > Enterprise Apps'a** göz atın.

2. ThousandEyes'i zaten tek oturum açma için yapılandırıldıysanız, arama alanını kullanarak ThousandEyes örneğinizi arayın. Aksi takdirde, uygulama galerisinde **BinGöz** **Ekle'yi** ve aramasını seçin. Arama sonuçlarından ThousandEyes'i seçin ve uygulama listenize ekleyin.

3. ThousandEyes örneğini seçin ve ardından **Sağlama** sekmesini seçin.

4. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

    ![ThousandEyes Sağlama](./media/thousandeyes-provisioning-tutorial/ThousandEyes1.png)

5. Yönetici **Kimlik Bilgileri** bölümü altında, ThousandEyes hesabınız tarafından oluşturulan **OAuth Bearer Token'ı** girdiniz (ThousandEyes hesap **Profiliniz** bölümünüz altında bir belirteç bulabilir ve oluşturabilirsiniz).

    ![ThousandEyes Sağlama](./media/thousandeyes-provisioning-tutorial/ThousandEyes2.png)

6. Azure portalında, Azure AD'nin ThousandEyes uygulamanıza bağlanabilmesini sağlamak için **Test Bağlantısı'nı** tıklatın. Bağlantı başarısız olursa, ThousandEyes hesabınızın Yönetici izinlerine sahip olduğundan emin olun ve 5.

7. **Bildirim E-posta** alanında ödeme hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve onay kutusunu işaretleyin "Bir hata oluştuğunda e-posta bildirimi gönderin."

8. **Kaydet**'e tıklayın.

9. Eşlemeler bölümünde, **Azure Etkin Dizin Kullanıcılarını BinEyes'e Senkronize Et'i**seçin.

10. **Öznitelik Eşlemeleri** bölümünde, Azure AD'den ThousandEyes'e eşitlenen kullanıcı özniteliklerini gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için ThousandEyes'teki kullanıcı hesaplarıyla eşleştirilmesi için kullanılır. Herhangi bir değişiklik yapmak için Kaydet düğmesini seçin.

11. ThousandEyes için Azure AD sağlama hizmetini etkinleştirmek **için, Ayarlar** bölümünde **Sağlama Durumunu** **Açık** olarak değiştirin

12. **Kaydet**'e tıklayın.

Bu işlem, Kullanıcılar ve Gruplar bölümünde ThousandEyes'e atanan kullanıcıların ve/veya grupların ilk eşitlemisini başlatır. İlk eşitlemenin gerçekleştirilemi, hizmet yürütülürken yaklaşık her 40 dakikada bir meydana gelen sonraki eşitlemelerden daha uzun sürüyor. İlerlemeyi izlemek ve sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklayan sağlama etkinlik günlüklerine olan bağlantıları izlemek için **Eşitleme Ayrıntıları** bölümünü kullanabilirsiniz.

Azure AD sağlama günlüklerini nasıl okuyabilirsiniz hakkında daha fazla bilgi için [bkz.](../app-provisioning/check-status-user-account-provisioning.md)

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı sağlamanın yönetimi](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri nasıl inceleyip sağlama etkinliği yle ilgili raporları nasıl alacağınızı öğrenin](../app-provisioning/check-status-user-account-provisioning.md)
