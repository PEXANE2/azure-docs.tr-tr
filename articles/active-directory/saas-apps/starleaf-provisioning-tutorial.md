---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için StarLeaf'i yapılandırın | Microsoft Dokümanlar"
description: Azure Active Directory'yi, kullanıcı hesaplarını StarLeaf'e otomatik olarak sağlamak ve sağlamadan çıkarmak için nasıl yapılandırılamayı öğrenin.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: na
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2019
ms.author: zhchia
ms.openlocfilehash: 520373fc6a05bcaada973273e3553f9da623c669
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77064315"
---
# <a name="tutorial-configure-starleaf-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için StarLeaf'i yapılandır

Bu öğreticinin amacı, Azure AD'yi kullanıcıları ve/veya grupları StarLeaf'e otomatik olarak sağlamak ve sağlamadan çıkarmak üzere yapılandırmak için StarLeaf ve Azure Etkin Dizini'nde (Azure AD) gerçekleştirilecek adımları göstermektir.

> [!NOTE]
>  Bu öğretici, Azure AD Kullanıcı Sağlama Hizmeti'nin üzerine inşa edilmiş bir bağlayıcıyı açıklar. Bu hizmetin ne yaptığı, nasıl çalıştığı ve sık sorulan sorular hakkında önemli ayrıntılar [için](../app-provisioning/user-provisioning.md)bkz.
>
> Bu bağlayıcı şu anda Önizleme'de. Önizleme özellikleri için genel Microsoft Azure kullanım koşulları hakkında daha fazla bilgi için, [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdaki ön koşullara sahip olduğunuzu varsayar:

* Azure AD kiracısı.
* [Bir StarLeaf kiracı.](https://www.starleaf.com/solutions/)
* Yönetici izinli StarLeaf'teki bir kullanıcı hesabı.

## <a name="assign-users-to-starleaf"></a>Kullanıcıları StarLeaf'e atama
Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişmesi gerektiğini belirlemek için atamalar adlı bir kavram kullanır. Otomatik kullanıcı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya atanan kullanıcılar ve/veya gruplar eşitlenir.

Otomatik kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki hangi kullanıcıların ve grupların StarLeaf'e erişmesi gerektiğine karar vermelisiniz. Daha sonra bu yönergeleri izleyerek kullanıcıları ve grupları [StarLeaf'e](../manage-apps/assign-user-or-group-access-portal.md)atayabilirsiniz.

## <a name="important-tips-for-assigning-users-to-starleaf"></a>Kullanıcıları StarLeaf'e atamak için önemli ipuçları

* Otomatik kullanıcı sağlama yapılandırmasını sınamak için StarLeaf'e tek bir Azure AD kullanıcısı atanması önerilir. Ek kullanıcılar ve gruplar daha sonra atanabilir.

* Bir kullanıcıyı StarLeaf'e atadığınızda, atama iletişim kutusunda uygulamaya özgü geçerli bir rolü (varsa) seçmeniz gerekir. Varsayılan Erişim rolüne sahip kullanıcılar sağlama nın dışında tutulur.

## <a name="set-up-starleaf-for-provisioning"></a>Tedarik için StarLeaf'i ayarlama

Azure AD ile otomatik kullanıcı sağlama için StarLeaf'i yapılandırmadan önce, SCIM sağlamayı StarLeaf'te yapılandırmanız gerekir:

1. [StarLeaf Yönetici Konsolunuzda](https://portal.starleaf.com/#page=login)oturum açın. **Tümleştirmelere** > Git**Tümleştirme ekle.**

    ![StarLeaf SCIM ekle](media/starleaf-provisioning-tutorial/image00.png)

2. Microsoft Azure Etkin Dizini olacak **Türü** seçin. **Ad'a**uygun bir ad girin. **Uygula**’ya tıklayın.

    ![StarLeaf SCIM ekle](media/starleaf-provisioning-tutorial/image01.png)

3.  Daha sonra **SCIM temel** URL'si ve **Access belirteç** değerleri görüntülenir. Bu değerler, Azure portalındaki StarLeaf uygulamanızın Sağlama sekmesindeki **Kiracı URL'sine** ve **Gizli Belirteç** alanlarına girilir. 

    ![StarLeaf Oluştur Belirteci](media/starleaf-provisioning-tutorial/image02.png)

## <a name="add-starleaf-from-the-gallery"></a>Galeriden StarLeaf ekleyin

Azure AD ile otomatik kullanıcı sağlama için StarLeaf'i yapılandırmak için, Yönetilen SaaS uygulamaları listenize Azure AD uygulama galerisinden StarLeaf eklemeniz gerekir.

**Azure AD uygulama galerisinden StarLeaf eklemek için aşağıdaki adımları gerçekleştirin:**

1. Azure **[portalında,](https://portal.azure.com)** soldaki gezinti panelinde **Azure Etkin Dizin'i**seçin.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **uygulamalara**gidin ve ardından **Tüm uygulamaları**seçin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için bölmenin üst kısmındaki **Yeni uygulama** düğmesini seçin.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **StarLeaf**girin, sonuç panelinde **StarLeaf'i** seçin.
    ![Sonuç listesinde StarLeaf](common/search-new-app.png)

## <a name="configure-automatic-user-provisioning-to-starleaf"></a>Otomatik kullanıcı sağlamayı StarLeaf'e yapılandırma

Bu bölüm, Azure AD'deki kullanıcı ve/veya grup atamalarına dayalı olarak StarLeaf'teki kullanıcıları ve/veya grupları oluşturmak, güncellemek ve devre dışı etmek için Azure AD sağlama hizmetini yapılandırma adımları boyunca size yol göstermektedir.

1. [Azure portalında](https://portal.azure.com)oturum açın. **Kurumsal Uygulamaları**seçin, ardından **Tüm uygulamaları**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **StarLeaf'i**seçin.

    ![Uygulamalar listesindeki StarLeaf bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. Yönetici Kimlik Bilgileri bölümü altında, sırasıyla **Kiracı URL'sinde** ve **Gizli Belirteç'te** daha önce alınan **SCIM Base URL** ve Access **Belirteci** değerlerini girin. Azure AD'nin StarLeaf'e bağlanabilmesini sağlamak için **Test Bağlantısı'nı** tıklatın. Bağlantı başarısız olursa, StarLeaf hesabınızın Yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Kiracı URL + Belirteç](common/provisioning-testconnection-tenanturltoken.png)

6. Bildirim **E-postası** alanında, sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve **bir hata olduğunda e-posta bildirimi gönder'i** denetleyin.

    ![Bildirim E-postası](common/provisioning-notification-email.png)

7. **Kaydet**'e tıklayın.

8. **Eşlemeler** bölümünde, **Azure Etkin Dizin Kullanıcılarını StarLeaf'e Senkronize Et'i**seçin.

    ![StarLeaf Oluştur Belirteci](media/starleaf-provisioning-tutorial/usermapping.png)

9. **Öznitelik Eşleme** bölümünde Azure AD'den StarLeaf'e eşitlenen kullanıcı özniteliklerini gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için StarLeaf'teki kullanıcı hesaplarıyla eşleştirilmesi için kullanılır. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

    ![StarLeaf Oluştur Belirteci](media/starleaf-provisioning-tutorial/userattribute.png)


10. Kapsam filtrelerini yapılandırmak [için, Kapsam](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)filtresi öğreticisinde sağlanan aşağıdaki yönergelere bakın.


11. StarLeaf için Azure AD sağlama hizmetini etkinleştirmek **için, Ayarlar** bölümünde **KiSama Durumunu** **Açık** olarak değiştirin.

    ![Geçiş Yapılan Sağlama Durumu](common/provisioning-toggle-on.png)

12. **Ayarlar** bölümünde **Kapsam'ta** istenen değerleri seçerek StarLeaf'e sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Sağlama Kapsamı](common/provisioning-scope.png)

13. Hükmetmeye hazır olduğunuzda **Kaydet'i**tıklatın.

    ![Tasarruf Sağlama Yapılandırması](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **Kapsam'ta** tanımlanan tüm kullanıcıların ve/veya grupların ilk eşitlemisini başlatır. Azure AD sağlama hizmeti nin çalıştırıldığı sürece yaklaşık her 40 dakikada bir gerçekleşen sonraki eşitlemelerden daha uzun süren ilk eşitlemenin gerçeklemi daha uzun sürer. İlerlemeyi izlemek ve StarLeaf'teki Azure AD sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklayan sağlama faaliyet raporuna bağlı bağlantıları izlemek için **Eşitleme Ayrıntıları** bölümünü kullanabilirsiniz.

Azure AD sağlama günlüklerini nasıl okuyabilirsiniz hakkında daha fazla bilgi için bkz: [Otomatik kullanıcı hesabı sağlama hakkında Raporlama](../app-provisioning/check-status-user-account-provisioning.md)

## <a name="connector-limitations"></a>Konektör sınırlamaları

* StarLeaf şu anda grup sağlamayı desteklemiyor. 
* **StarLeaf, e-posta** ve **kullanıcı Adı** değerlerinin aynı kaynak değerine sahip olmasını gerektirir.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal uygulamalar için kullanıcı hesabı sağlamayı yönetme](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* Günlükleri nasıl [inceleyip sağlama etkinliği yle ilgili raporlar alacağınızı](../app-provisioning/check-status-user-account-provisioning.md)öğrenin.
