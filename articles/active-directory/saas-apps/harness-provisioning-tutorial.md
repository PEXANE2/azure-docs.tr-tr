---
title: 'Öğretici: Azure Active Directory ile otomatik Kullanıcı sağlaması için bandı yapılandırma | Microsoft Docs'
description: Kullanıcı hesaplarını otomatik olarak sağlamak ve yeniden sağlamak üzere Azure Active Directory nasıl yapılandıracağınızı öğrenin.
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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77057847"
---
# <a name="tutorial-configure-harness-for-automatic-user-provisioning"></a>Öğretici: otomatik Kullanıcı sağlaması için bandı yapılandırma

Bu makalede, kullanıcıları veya grupları otomatik olarak sağlamak ve sağlamak üzere Azure Active Directory (Azure AD) yapılandırma hakkında bilgi edineceksiniz.

> [!NOTE]
> Bu makalede, Azure AD Kullanıcı sağlama hizmeti ' nin üzerine kurulmuş bir bağlayıcı açıklanmaktadır. Bu hizmet ve sık sorulan soruların yanıtları hakkında önemli bilgiler için, bkz. [Azure Active Directory Ile SaaS uygulamalarına Kullanıcı sağlamayı ve sağlamayı kaldırmayı otomatikleştirme](../app-provisioning/user-provisioning.md).
>
> Bu bağlayıcı Şu anda önizleme aşamasındadır. Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Ön koşullar

Bu makalede özetlenen senaryo, aşağıdaki önkoşulların zaten olduğunu varsayar:

* Bir Azure AD kiracısı
* [Bir bir bir bandı kiracı](https://harness.io/pricing/)
* *Yönetici* izinlerine sahip bir kullanıcı hesabı

## <a name="assign-users-to-harness"></a>Kullanıcıları bir bandı ata

Azure Active Directory seçili uygulamalara hangi kullanıcıların erişimi alacağını belirleyen *atama* adı verilen bir kavram kullanır. Otomatik Kullanıcı sağlama bağlamında, yalnızca Azure AD 'de bir uygulamaya atanan kullanıcılar veya gruplar eşitlenir.

Otomatik Kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD 'deki hangi kullanıcıların veya grupların, her bir oturum için erişmesi gerektiğine karar verin. Daha sonra bu kullanıcıları veya grupları [bir kurumsal uygulamaya Kullanıcı veya Grup atama](../manage-apps/assign-user-or-group-access-portal.md)bölümündeki yönergeleri izleyerek, bu kullanıcılara veya gruplara atayabilirsiniz.

## <a name="important-tips-for-assigning-users-to-harness"></a>Kullanıcıları bir bandı atamaya yönelik önemli ipuçları

* Otomatik Kullanıcı sağlama yapılandırmasını test etmek için tek bir Azure AD kullanıcısı atamanız önerilir. Ek kullanıcılar veya gruplar daha sonra atanabilir.

* Bir kullanıcıyı bir kullanıcıya atadığınızda, **atama** iletişim kutusunda uygulamaya özgü geçerli herhangi bir rolü (varsa) seçmeniz gerekir. *Varsayılan erişim* rolüne sahip kullanıcılar, sağlanmasından çıkarılır.

## <a name="set-up-harness-for-provisioning"></a>Sağlama için bir bandı ayarlama

1. , Bir [Yönetici Konsolu](https://app.harness.io/#/login)'nda oturum açın ve ardından **sürekli güvenlik** > **erişimi yönetimi**' ne gidin.

    ![Yönetici Konsolu](media/harness-provisioning-tutorial/admin.png)

1. **API anahtarlarını**seçin.

    ![Anahtar API 'Sı bağlantısı](media/harness-provisioning-tutorial/apikeys.png)

1. **API anahtarı Ekle ' yi**seçin. 

    ![Ana API anahtar bağlantısı ekle](media/harness-provisioning-tutorial/addkey.png)

1. **API anahtarı Ekle** bölmesinde şunları yapın:

    ![Ana API anahtar bölmesi Ekle](media/harness-provisioning-tutorial/title.png)
   
   a. **Ad** kutusunda, anahtar için bir ad girin.  
   b. Açılan **öğeden devralınan izinler** listesinden bir seçenek belirleyin. 
   
1. **Gönder**’i seçin.

1. **Anahtarı** Bu öğreticide daha sonra kullanmak üzere kopyalayın.

    ![Bandı oluşturma belirteci](media/harness-provisioning-tutorial/token.png)

## <a name="add-harness-from-the-gallery"></a>Galeriden bir bandı ekleyin

Azure AD ile otomatik Kullanıcı sağlaması için bandı yapılandırmadan önce, Azure AD uygulama galerisindeki yönetilen SaaS uygulamaları listenize bir bandı eklemeniz gerekir.

1. [Azure Portal](https://portal.azure.com)sol bölmedeki **Azure Active Directory**' ı seçin.

    !["Azure Active Directory" düğmesi](common/select-azuread.png)

1. **Kurumsal uygulamalar** > **tüm uygulamalar**' ı seçin.

    !["Tüm uygulamalar" bağlantısı](common/enterprise-applications.png)

1. Yeni bir uygulama eklemek için bölmenin üst kısmındaki **Yeni uygulama** düğmesini seçin.

    !["Yeni uygulama" düğmesi](common/add-new-app.png)

1. Arama kutusuna, **Ekle ' ye tıklayın, sonuçlar**listesinde, ' i seçin ve ardından uygulamayı eklemek için **Ekle** **düğmesini seçin.**

    ![Sonuçlar listesinde,](common/search-new-app.png)

## <a name="configure-automatic-user-provisioning-to-harness"></a>Otomatik Kullanıcı sağlamasını otomatik olarak 

Bu bölümde Azure AD sağlama hizmeti 'ni kullanarak, Azure AD 'de Kullanıcı veya grup atamalarını temel alan Kullanıcı veya grupları oluşturmak, güncelleştirmek ve devre dışı bırakmak için adım adım kılavuzluk eder.

> [!TIP]
> Ayrıca, ara [Çoklu oturum açma öğreticisindeki](https://docs.microsoft.com/azure/active-directory/saas-apps/harness-tutorial)yönergeleri izleyerek, her bir BANDı için SAML tabanlı çoklu oturum açmayı etkinleştirmeyi de tercih edebilirsiniz. Otomatik Kullanıcı sağlamaktan bağımsız olarak çoklu oturum açmayı yapılandırabilirsiniz, ancak bu iki özellik birbirini tamamlamada.

> [!NOTE]
> Bu uç nokta uç noktası hakkında daha fazla bilgi edinmek için, bkz. ana [API anahtarları](https://docs.harness.io/article/smloyragsm-api-keys) makalesi.

Azure AD 'de otomatik Kullanıcı sağlamasını yapılandırmak için aşağıdakileri yapın:

1. [Azure Portal](https://portal.azure.com) **Kurumsal uygulamalar** > **tüm uygulamalar**' ı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

1. Uygulamalar listesinde, **bandı**' ni seçin.

    ![Uygulamalar listesindeki bir bağlantı](common/all-applications.png)

1. **Sağlamayı**seçin.

    ![Sağlama düğmesi](common/provisioning.png)

1. **Sağlama modu** aşağı açılan listesinde **Otomatik**' i seçin.

    !["Sağlama modu" açılan listesi](common/provisioning-automatic.png)

1. **Yönetici kimlik bilgileri**altında şunları yapın:

    ![Kiracı URL 'SI + belirteç](common/provisioning-testconnection-tenanturltoken.png)
 
   a. **Kiracı URL 'si** kutusuna, girin **`https://app.harness.io/gateway/api/scim/account/XCPzWkCIQ46ypIu2DeT7yw`**.  
   b. **Gizli belirteç** kutusunda, "sağlama Için bandı ayarlama" bölümünün 6. adımında kaydettiğiniz SCIM kimlik doğrulama belirteci değerini girin.  
   c. Azure AD 'nin, bir bandı bağlamak için **Bağlantıyı Sına** ' yı seçin. Bağlantı başarısız olursa, bu ortamınızda *yönetici* izinlerine sahip olduğundan emin olun ve sonra yeniden deneyin.

1. **Bildirim e-postası** kutusunda, sağlama hatası bildirimlerini alması gereken bir kişinin veya grubun e-posta adresini girin ve ardından **bir hata oluştuğunda e-posta bildirimi gönder** onay kutusunu seçin.

    !["Bildirim e-postası" kutusu](common/provisioning-notification-email.png)

1. **Kaydet**’i seçin.

1. **Eşlemeler**altında **Azure Active Directory Kullanıcıları**

    !["Kullanıcı Azure Active Directory Kullanıcıları" bağlantısı yap](media/harness-provisioning-tutorial/usermappings.png)

1. **Öznitelik eşlemeleri**altında, Azure AD 'den bandı ile eşitlenen Kullanıcı özniteliklerini gözden geçirin. *Eşleştirme* olarak seçilen öznitelikler, güncelleştirme işlemleri için, ara içindeki kullanıcı hesaplarıyla eşleştirmek için kullanılır. Değişiklikleri uygulamak için **Kaydet** ' i seçin.

    ![Kullanıcı "öznitelik eşlemeleri" bölmesi](media/harness-provisioning-tutorial/userattributes.png)

1. **Eşlemeler**' in altında, **Azure Active Directory gruplarını bir şekilde**seçin.

    !["Azure Active Directory gruplarını bir bandı ile eşitler" bağlantısı](media/harness-provisioning-tutorial/groupmappings.png)

1. **Öznitelik eşlemeleri**altında, Azure AD 'den bandı ile eşitlenen grup özniteliklerini inceleyin. *Eşleşen* özellikler olarak seçilen öznitelikler, güncelleştirme işlemleri için, ara içindeki grupları eşleştirmek için kullanılır. Değişiklikleri uygulamak için **Kaydet** ' i seçin.

    !["Öznitelik eşlemeleri" bölmesi](media/harness-provisioning-tutorial/groupattributes.png)

1. Kapsam filtrelerini yapılandırmak için, bkz. [kapsam filtreleri Ile öznitelik tabanlı uygulama sağlama](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).

1. **Ayarlar**' ın altında, ara IÇIN Azure AD sağlama hizmetini etkinleştirmek Için, **sağlama durumu** ' nu **Açık**' a değiştirin.

    ![Sağlama durumu anahtarı "on" olarak değiştirildi](common/provisioning-toggle-on.png)

1. **Ayarlar**' ın altında, **kapsam** açılan listesinde, sağlama yaptığınız kullanıcıları veya grupları nasıl eşitlemek istediğinizi seçin.

    ![Sağlama kapsamı](common/provisioning-scope.png)

1. Sağlamaya hazırsanız **Kaydet**' i seçin.

    ![Sağlama Kaydet düğmesi](common/provisioning-configuration-save.png)

Bu işlem, hazırlama yaptığınız kullanıcıların veya grupların ilk eşitlemesini başlatır. İlk eşitlemenin daha sonra gerçekleştirmesi daha uzun sürer. Eşitlemeler, Azure AD sağlama hizmeti çalıştığı sürece yaklaşık olarak her 40 dakikada bir gerçekleşir. İlerlemeyi izlemek için **eşitleme ayrıntıları** bölümüne gidin. Ayrıca, Azure AD sağlama hizmeti tarafından her türlü gerçekleştirilen tüm eylemleri içeren bir sağlama etkinliği raporunun bağlantılarını da izleyebilirsiniz.

Azure AD sağlama günlüklerinin nasıl okunduğu hakkında daha fazla bilgi için bkz. [Otomatik Kullanıcı hesabı sağlama hakkında rapor](../app-provisioning/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal uygulamalar için Kullanıcı hesabı sağlamayı yönetme](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri İnceleme ve sağlama etkinliğinde rapor alma hakkında bilgi edinin](../app-provisioning/check-status-user-account-provisioning.md)
