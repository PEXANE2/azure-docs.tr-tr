---
title: 'Öğretici: Bolluk için kullanıcı sağlama - Azure AD'
description: Azure Active Directory'yi, kullanıcı hesaplarını Slack'e otomatik olarak sağlamak ve sağlamadan çıkarmak için nasıl yapılandırılamayı öğrenin.
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
ms.openlocfilehash: cdc912c2df435f9b7e591d7c5475e126e6b0aeb7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77062838"
---
# <a name="tutorial-configure-slack-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için Slack'i yapılandır

Bu öğreticinin amacı, Azure AD'den Slack'e kullanıcı hesaplarını otomatik olarak sağlamak ve sağlamadan çıkarmak için Slack ve Azure AD'de gerçekleştirmeniz gereken adımları size göstermektir.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, zaten aşağıdaki öğelere sahip olduğunuzu varsayar:

* Azure Active Directory kiracısı
* [Plus planına](https://aadsyncfabric.slack.com/pricing) sahip veya daha iyi etkin leştirilmiş bir Bolluk kiracısı
* Ekip Yöneticisi izinleri ile Slack'te bir kullanıcı hesabı

Not: Azure AD sağlama tümleştirmesi, Plus planında veya daha iyisi üzerinde Slack ekiplerinin kullanabileceği [Slack SCIM API'sine](https://api.slack.com/scim)dayanır.

## <a name="assigning-users-to-slack"></a>Kullanıcıları Slack'e atama

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişmesi gerektiğini belirlemek için "atamalar" adlı bir kavram kullanır. Otomatik kullanıcı hesabı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya "atanmış" kullanıcılar ve gruplar eşitlenir.

Sağlama hizmetini yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki kullanıcıların ve/veya grupların Slack uygulamanıza erişilmesi gereken kullanıcıları temsil edeceğine karar vermeniz gerekir. Karar verildikten sonra, bu kullanıcıları buradaki talimatları izleyerek Slack uygulamanıza atayabilirsiniz:

[Bir kurumsal uygulamaya kullanıcı veya grup atama](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-slack"></a>Kullanıcıları Slack'e atamak için önemli ipuçları

* Sağlama yapılandırmasını sınamak için tek bir Azure AD kullanıcısının Slack'e atanması önerilir. Ek kullanıcılar ve/veya gruplar daha sonra atanabilir.

* Bir kullanıcıyı Slack'e atarken, atama iletişim kutusunda **Kullanıcı** veya "Grup" rolünü seçmeniz gerekir. "Varsayılan Erişim" rolü sağlama için çalışmaz.

## <a name="configuring-user-provisioning-to-slack"></a>Kullanıcı sağlamayı Slack'e yapılandırma 

Bu bölüm, Azure REKLAM'ınızı Slack'in kullanıcı hesabı sağlama API'sine bağlamanız ve sağlama hizmetini Azure AD'deki kullanıcı ve grup atamasına göre Slack'te atanmış kullanıcı hesapları oluşturacak, güncelleştirecek ve devre dışı edecek şekilde yapılandırmanız konusunda size rehberlik eder.

**İpucu:** [Azure portalında](https://portal.azure.com)sağlanan yönergeleri izleyerek, Slack için SAML tabanlı Tek Oturum Açma özelliğini etkinleştirmeyi de seçebilirsiniz. Tek oturum açma, otomatik sağlamadan bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini tamamlar.

### <a name="to-configure-automatic-user-account-provisioning-to-slack-in-azure-ad"></a>Azure AD'de otomatik kullanıcı hesabı sağlamayı Slack'e yapılandırmak için:

1. Azure [portalında,](https://portal.azure.com)Tüm uygulamalar > **Azure Active Directory > Enterprise Apps'a** göz atın.

2. Tek oturum açma için Slack'i zaten yapılandırmışsanız, arama alanını kullanarak Slack örneğinizi arayın. Aksi takdirde, **ekle'yi** seçin ve uygulama galerisinde **Bolluk'u** arayın. Arama sonuçlarından Slack'i seçin ve uygulama listenize ekleyin.

3. Bolluk örneğini seçin ve ardından **Sağlama** sekmesini seçin.

4. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

   ![Bolluk Sağlama](./media/slack-provisioning-tutorial/slack1.png)

5. Yönetici **Kimlik Bilgileri** bölümünde **Yetkiver'i**tıklatın. Bu, yeni bir tarayıcı penceresinde bir Slack yetkilendirme iletişim kutusunu açar.

6. Yeni pencerede, Team Admin hesabınızı kullanarak Slack'te oturum açın. elde edilen yetkilendirme iletişim kutusunda, sağlama sağlamak istediğiniz Slack takımını seçin ve ardından **Yetkilendir'i**seçin. Tamamlandıktan sonra, sağlama yapılandırmasını tamamlamak için Azure portalına dönün.

    ![Yetkilendirme İletişim Kutusu](./media/slack-provisioning-tutorial/slackauthorize.png)

7. Azure portalında, Azure AD'nin Slack uygulamanıza bağlanabilmesini sağlamak için **Test Bağlantısı'nı** tıklatın. Bağlantı başarısız olursa, Slack hesabınızda Team Admin izinleri olduğundan emin olun ve "Yetki" adımını yeniden deneyin.

8. **Bildirim E-posta** alanında ödeme hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve aşağıdaki onay kutusunu işaretleyin.

9. **Kaydet**'e tıklayın.

10. Eşlemeler bölümünde, **Azure Etkin Dizin Kullanıcılarını Slack'e Senkronize Et'i**seçin.

11. **Öznitelik Eşlemeleri** bölümünde, Azure AD'den Slack'e eşitlenecek kullanıcı özniteliklerini gözden geçirin. **Eşleşme** özellikleri olarak seçilen özniteliklerin, sürücü işlemleri için Slack' teki kullanıcı hesaplarıyla çalışmak için kullanıla Herhangi bir değişiklik yapmak için Kaydet düğmesini seçin.

12. Slack için Azure AD sağlama hizmetini etkinleştirmek **için, Ayarlar** bölümünde **On** **KiSama Durumunu**

13. **Kaydet**'e tıklayın.

Bu, Kullanıcılar ve Gruplar bölümünde Slack'e atanan kullanıcıların ve/veya grupların ilk eşitlenmesine başlar. İlk eşitlemenin, hizmet çalışırken yaklaşık her 10 dakikada bir oluşan sonraki eşitlemelerden daha uzun süreceğini unutmayın. İlerlemeyi izlemek ve Slack uygulamanızda sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklayan sağlama faaliyet raporlarına yönelik bağlantıları izlemek için **Eşitleme Ayrıntıları** bölümünü kullanabilirsiniz.

## <a name="optional-configuring-group-object-provisioning-to-slack"></a>[İsteğe bağlı] Grup nesnesi sağlamayı Slack'e yapılandırma

İsteğe bağlı olarak, Grup nesnelerinin Azure AD'den Slack'e sağlanmasını etkinleştirebilirsiniz. Bu, üyelerine ek olarak gerçek grup nesnesinin Azure AD'den Slack'e kopyalanması nedeniyle "kullanıcı grupları atama"dan farklıdır. Örneğin, Azure AD'de "Grubum" adında bir grubunuzun varsa, Slack içinde "Grubum" adlı özdeş bir grup oluşturulur.

### <a name="to-enable-provisioning-of-group-objects"></a>Grup nesnelerinin sağlanmasını etkinleştirmek için:

1. Eşlemeler bölümünde, **Azure Etkin Dizin Gruplarını Slack'e Senkronize Et'i**seçin.

2. Öznitelik Eşleme bıçağında, Etkin'den Evet'e ayarlayın.

3. **Öznitelik Eşlemeleri** bölümünde, Azure AD'den Slack'e eşitlenecek grup özniteliklerini gözden geçirin. **Eşleşme** özellikleri olarak seçilen özniteliklerin, sürül işlemleri için Slack' teki gruplarla aynı ş 

4. **Kaydet**'e tıklayın.

Bu, **Kullanıcılar ve Gruplar** bölümünde Slack'e atanan tüm grup nesnelerinin Azure AD'den Slack'e tam olarak eşitlenmesine neden olur. İlerlemeyi izlemek ve Slack uygulamanızdaki sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklayan sağlama etkinlik günlüklerine olan bağlantıları izlemek için **Eşitleme Ayrıntıları** bölümünü kullanabilirsiniz.

Azure AD sağlama günlüklerini nasıl okuyabilirsiniz hakkında daha fazla bilgi için [bkz.](../app-provisioning/check-status-user-account-provisioning.md)

## <a name="connector-limitations"></a>Konektör sınırlamaları

* Slack'in **displayName** özniteliğini yapılandırırken aşağıdaki davranışlara dikkat edin:

  * Değerler tamamen benzersiz değildir (örn. 2 kullanıcı aynı ekran adına sahip olabilir)

  * İngilizce olmayan karakterleri, boşlukları, büyük harfleri destekler. 
  
  * İzin verilen noktalama işaretleri dönemleri, alt çizgileri, tireleri, kesitleri, parantezleri (örn. **[ { } ] ) )** ve ayırıcıları (örn. / / **;**) içerir.
  
  * Yalnızca bu iki ayar Slack'in işyerinde/kuruluşunda yapılandırılırsa güncelleştirmeler - **Profil eşitleme etkindir** ve **Kullanıcılar görüntü adlarını değiştiremez.**
  
* Slack'in **kullanıcı Adı** özniteliği 21 karakterin altında olmalı ve benzersiz bir değere sahip olmalıdır.

* Bolluk sadece kullanıcı **Adı** ve **e-posta**öznitelikleri ile eşleşen sağlar.  

## <a name="additional-resources"></a>Ek Kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı sağlamanın yönetimi](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)
