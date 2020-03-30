---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için Harness'i yapılandırın | Microsoft Dokümanlar"
description: Kullanıcı hesaplarını Harness'e otomatik olarak sağlamak ve devre dışı etmek için Azure Active Directory'yi nasıl yapılandıracak larını öğrenin.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: 0cdb970b-440b-4e7c-9118-2f03baab6a20
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/29/2019
ms.author: Zhchia
ms.openlocfilehash: 518d86fff04a23f1c1e63c44c53485b99f30637d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77057847"
---
# <a name="tutorial-configure-harness-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için Koşum'u yapılandırın

Bu makalede, kullanıcıları veya grupları Harness'e otomatik olarak sağlamak ve devre dışı etmek için Azure Etkin Dizinini (Azure AD) nasıl yapılandırabileceğinizi öğreneceksiniz.

> [!NOTE]
> Bu makalede, Azure AD kullanıcı sağlama hizmetinin üzerine inşa edilmiş bir bağlayıcı açıklanmaktadır. Bu hizmet hakkında önemli bilgiler ve sık sorulan soruların yanıtları [için](../app-provisioning/user-provisioning.md)bkz.
>
> Bu bağlayıcı şu anda önizlemede. Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

## <a name="prerequisites"></a>Ön koşullar

Bu makalede özetlenen senaryo, zaten aşağıdaki ön koşullara sahip olduğunu varsayar:

* Azure AD kiracı
* [Bir Koşum kiracı](https://harness.io/pricing/)
* *Yönetici* izinleri ile Harness'te bir kullanıcı hesabı

## <a name="assign-users-to-harness"></a>Kullanıcıları Harness'e atama

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişmesi gerektiğini belirlemek için *atamalar* adlı bir kavram kullanır. Otomatik kullanıcı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya atanan kullanıcılar veya gruplar eşitlenir.

Otomatik kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki hangi kullanıcıların veya grupların Harness'e erişmeye ihtiyacı olduğuna karar verin. Daha [sonra, bir kullanıcıyı veya grubu kurumsal bir uygulamaya atama](../manage-apps/assign-user-or-group-access-portal.md)yönergelerini izleyerek bu kullanıcıları veya grupları Harness'e atayabilirsiniz.

## <a name="important-tips-for-assigning-users-to-harness"></a>Kullanıcıları Harness'e atamak için önemli ipuçları

* Otomatik kullanıcı sağlama yapılandırmasını test etmek için Harness'e tek bir Azure AD kullanıcısı atamanızı tavsiye ettik. Ek kullanıcılar veya gruplar daha sonra atanabilir.

* Bir kullanıcıyı Harness'e atadığınızda, **Atama** iletişim kutusunda uygulamaya özgü geçerli bir rolü (varsa) seçmeniz gerekir. *Varsayılan Erişim* rolüne sahip kullanıcılar sağlama nın dışında tutulur.

## <a name="set-up-harness-for-provisioning"></a>Sağlama için Koşum'u ayarlama

1. Harness Yönetici [Konsolunuzda](https://app.harness.io/#/login)oturum açın ve **ardından Sürekli Güvenlik** > **Erişim Yönetimi'ne**gidin.

    ![Harness Yönetici Konsolu](media/harness-provisioning-tutorial/admin.png)

1. **API Tuşlarını**seçin.

    ![Harness API Tuşları bağlantısı](media/harness-provisioning-tutorial/apikeys.png)

1. **API Tuşu Ekle'yi**seçin. 

    ![Harness API Anahtar bağlantısı ekle](media/harness-provisioning-tutorial/addkey.png)

1. **Api Anahtarı Ekle** bölmesinde aşağıdakileri yapın:

    ![Koşum Api Anahtar bölmesi ekle](media/harness-provisioning-tutorial/title.png)
   
   a. **Ad** kutusunda, anahtar için bir ad sağlayın.  
   b. Açılan listeden **Devralınan İzinler'de** bir seçenek seçin. 
   
1. **Gönder**’i seçin.

1. Bu öğreticide daha sonra kullanmak üzere **Anahtarı** kopyalayın.

    ![Harness Oluşturma Belirteci](media/harness-provisioning-tutorial/token.png)

## <a name="add-harness-from-the-gallery"></a>Galeriden Koşum Ekle

Azure AD ile otomatik kullanıcı sağlama için Harness'i yapılandırmadan önce, Azure AD uygulama galerisinden Harness'i yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Azure [portalında,](https://portal.azure.com)sol bölmede **Azure Etkin Dizini'ni**seçin.

    !["Azure Etkin Dizin" düğmesi](common/select-azuread.png)

1. **Kurumsal uygulamaları** > seçin**Tüm uygulamalar**.

    !["Tüm uygulamalar" bağlantısı](common/enterprise-applications.png)

1. Yeni bir uygulama eklemek için bölmenin üst kısmındaki **Yeni uygulama** düğmesini seçin.

    !["Yeni uygulama" düğmesi](common/add-new-app.png)

1. Arama kutusuna **Harness'i**girin , sonuç listesinde **Harness'i** seçin ve ardından uygulamayı eklemek için **Ekle** düğmesini seçin.

    ![Sonuç listesinde koşum](common/search-new-app.png)

## <a name="configure-automatic-user-provisioning-to-harness"></a>Otomatik kullanıcı sağlamayı Harness olarak yapılandırın 

Bu bölüm, Azure AD'deki kullanıcı veya grup atamalarına dayalı olarak Harness'teki kullanıcıları veya grupları oluşturmak, güncelleştirmek ve devre dışı etmek için Azure AD sağlama hizmetini yapılandırma adımları boyunca size rehberlik eder.

> [!TIP]
> Ayrıca, [Harness tek oturum](https://docs.microsoft.com/azure/active-directory/saas-apps/harness-tutorial)açma öğreticisindeki talimatları izleyerek Koşum için SAML tabanlı tek oturum açmayı etkinleştirmeyi de seçebilirsiniz. Bu iki özellik birbirini tamamlasa da, otomatik kullanıcı sağlamadan bağımsız olarak tek oturum açma yapılandırabilirsiniz.

> [!NOTE]
> Harness SCIM bitiş noktası hakkında daha fazla bilgi edinmek için Harness [API Keys](https://docs.harness.io/article/smloyragsm-api-keys) makalesine bakın.

Azure AD'de Harness için otomatik kullanıcı sağlama yı yapılandırmak için aşağıdakileri yapın:

1. Azure [portalında](https://portal.azure.com)Kurumsal **Uygulamalar** > **Tüm uygulamaları**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

1. Uygulamalar listesinde **Harness'i**seçin.

    ![Uygulamalar listesindeki Harness bağlantısı](common/all-applications.png)

1. **Sağlama'yı**seçin.

    ![Sağlama düğmesi](common/provisioning.png)

1. Sağlama **Modu** açılır listesinde **Otomatik'i**seçin.

    !["Sağlama Modu" açılır listesi](common/provisioning-automatic.png)

1. **Yönetici Kimlik Bilgileri**altında aşağıdakileri yapın:

    ![Kiracı URL + Belirteç](common/provisioning-testconnection-tenanturltoken.png)
 
   a. Kiracı **URL** kutusuna **`https://app.harness.io/gateway/api/scim/account/XCPzWkCIQ46ypIu2DeT7yw`** girin.  
   b. Gizli **Belirteç** kutusuna, "Sağlama için Koşum'u ayarla" bölümünün 6.  
   c. Azure AD'nin Harness'e bağlanabilmesini sağlamak için **Test Bağlantısı'nı** seçin. Bağlantı başarısız olursa, Harness hesabınızın *Yönetici* izinlerine sahip olduğundan emin olun ve sonra yeniden deneyin.

1. Bildirim **E-posta** kutusuna, sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve **ardından bir hata olduğunda e-posta bildirimi gönder'i** seçin.

    !["Bildirim E-postası" kutusu](common/provisioning-notification-email.png)

1. **Kaydet'i**seçin.

1. **Eşlemeler**altında, **Yararlanışyapmak için Azure Etkin Dizin Kullanıcılarını Senkronize Et'i**seçin.

    !["Azure Active Directory Kullanıcılarını Yararlanmak için Senkronize Edin" bağlantısını kullanma](media/harness-provisioning-tutorial/usermappings.png)

1. **Öznitelik Eşlemeleri**altında, Azure AD'den Harness'e eşitlenen kullanıcı özniteliklerini gözden geçirin. *Eşleme* olarak seçilen öznitelikler, güncelleştirme işlemleri için Harness'teki kullanıcı hesaplarıyla eşleştirilmesi için kullanılır. Herhangi bir değişiklik gerçekleştirmek için **Kaydet'i** seçin.

    ![Harness kullanıcı "Öznitelik Eşlemeleri" bölmesi](media/harness-provisioning-tutorial/userattributes.png)

1. **Eşlemeler**altında, **Yararlanılmak için Azure Etkin Dizin Gruplarını Eşitle'yi**seçin.

    !["Azure Active Directory Groups'u Kullanmak Için Senkronize Et" bağlantısını kullanma](media/harness-provisioning-tutorial/groupmappings.png)

1. **Öznitelik Eşlemeleri**altında, Azure AD'den Harness'e eşitlenen grup özniteliklerini gözden geçirin. *Eşleştirme* özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Harness'teki gruplarla eşleştirilmesi için kullanılır. Herhangi bir değişiklik gerçekleştirmek için **Kaydet'i** seçin.

    ![Koşum grubu "Atrit Eşlemeleri" bölmesi](media/harness-provisioning-tutorial/groupattributes.png)

1. Kapsam filtrelerini yapılandırmak için, [kapsam filtreleri ile Birlikte Öznitelik tabanlı uygulama sağlama](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)konusuna bakın.

1. **Ayarlar**altında, Harness için Azure AD sağlama hizmetini etkinleştirmek **için, Sağlama Durumu** anahtarını **Açık'a**geçirin.

    ![Sağlama Durumu anahtarı "Açık" olarak değiştirildi](common/provisioning-toggle-on.png)

1. **Ayarlar**altında, **Kapsam** açılır listesinde, Harness'e sağlamakta olduğunuz kullanıcıları veya grupları nasıl eşitlemek istediğinizi seçin.

    ![Sağlama Kapsamı](common/provisioning-scope.png)

1. Hükmetmeye hazır olduğunuzda **Kaydet'i**seçin.

    ![Kaydet düğmesi](common/provisioning-configuration-save.png)

Bu işlem, sağlama yaptığınız kullanıcıların veya grupların ilk eşitlemeyi başlatır. İlk eşitlemenin daha sonrakisine göre gerçekleştirmesi daha uzun sürer. Azure AD sağlama hizmeti çalışmaya devam ediyorsa, eşitlemeler yaklaşık olarak her 40 dakikada bir gerçekleşir. İlerlemeyi izlemek için **Eşitleme Ayrıntıları** bölümüne gidin. Harness'teki Azure AD sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklayan bir sağlama faaliyet raporuna ilişkin bağlantıları da izleyebilirsiniz.

Azure AD sağlama günlüklerini nasıl okuyabilirsiniz hakkında daha fazla bilgi için [otomatik kullanıcı hesabı sağlama raporu'na](../app-provisioning/check-status-user-account-provisioning.md)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal uygulamalar için kullanıcı hesabı sağlamayı yönetme](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri nasıl inceleyip sağlama etkinliği yle ilgili raporları nasıl alacağınızı öğrenin](../app-provisioning/check-status-user-account-provisioning.md)
