---
title: "Öğretici: Azure Active Directory ile otomatik Kullanıcı sağlama için NetSuite OneWorld 'yi yapılandırma | Microsoft Docs"
description: Azure Active Directory ve NetSuite OneWorld arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 01/26/2018
ms.author: jeedes
ms.openlocfilehash: a5b995fd273fcfa231e101bc77b11d268be728fb
ms.sourcegitcommit: 0b9fe9e23dfebf60faa9b451498951b970758103
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2020
ms.locfileid: "94359144"
---
# <a name="tutorial-configuring-netsuite-for-automatic-user-provisioning"></a>Öğretici: otomatik Kullanıcı sağlaması için NetSuite yapılandırma

Bu öğreticinin amacı, Azure AD 'den NetSuite 'e Kullanıcı hesaplarını otomatik olarak sağlamak ve devre dışı bırakmak için NetSuite OneWorld ve Azure AD 'de gerçekleştirmeniz gereken adımları gösteriyoruz.

> [!NOTE]
> Bu tümleştirme Şu anda temel kimlik doğrulaması (Kullanıcı adı ve parola) kullanarak kimlik doğrular. NetSuite, bu gereksinimden bir istisna olmadıkları takdirde müşterilerin bu tümleştirmeyi kullanmasını engelleyen bir Multi-Factor Authentication gereksinimi uygulamıştır. Bu tümleştirmeyi, bir istisna olmadan müşterileri etkinleştirmek için daha yeni bir kimlik doğrulama yöntemiyle güncelleştirmek üzere NetSuite ile çalışıyoruz. Bu belgeyi, varsa bir ETA ile güncelleştireceğiz.

Önerilen eylem: lütfen bu tümleştirme için kimlik doğrulama davranışına bir güncelleştirme yayınlıyoruz veya Multi-Factor Authentication gereksinimini bir istisna hakkında sorgulamak için NetSuite desteğine ulaşın.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide anlatılan senaryoda aşağıdakilere sahip olduğunuz kabul edilmiştir:

*   Azure Active Directory kiracısı.
*   NetSuite OneWorld aboneliği. Otomatik Kullanıcı hazırlama 'nın Şu anda yalnızca NetSuite OneWorld ile desteklendiğini unutmayın.
*   Netsuite 'te yönetici izinlerine sahip bir kullanıcı hesabı.
*   Azure AD ile tümleştirme için 2FA muafiyeti gerekir. Bu istisnayı istemek için lütfen NetSuite 'in destek ekibine başvurun.

## <a name="assigning-users-to-netsuite-oneworld"></a>Kullanıcıları NetSuite OneWorld 'e atama

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişim alacağını belirleyebilmek için "atamalar" adlı bir kavram kullanır. Otomatik Kullanıcı hesabı sağlama bağlamında, yalnızca Azure AD 'de bir uygulamaya "atanmış" olan kullanıcılar ve gruplar eşitlenir.

Sağlama hizmetini yapılandırmadan ve etkinleştirmeden önce, Azure AD 'deki hangi kullanıcıların ve/veya grupların NetSuite uygulamanıza erişmesi gereken kullanıcıları temsil ettiğini belirlemeniz gerekir. Karar verdikten sonra buradaki yönergeleri izleyerek bu kullanıcıları NetSuite uygulamanıza atayabilirsiniz:

[Kurumsal uygulamaya Kullanıcı veya Grup atama](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-netsuite-oneworld"></a>NetSuite OneWorld 'e Kullanıcı atamaya yönelik önemli ipuçları

*   Sağlama yapılandırmasını test etmek üzere NetSuite 'e tek bir Azure AD kullanıcısının atanması önerilir. Ek kullanıcılar ve/veya grupları daha sonra atanabilir.

*   Bir kullanıcıyı NetSuite 'e atarken geçerli bir kullanıcı rolü seçmeniz gerekir. "Varsayılan erişim" rolü sağlama için çalışmaz.

## <a name="enable-user-provisioning"></a>Kullanıcı sağlamayı etkinleştir

Bu bölümde, Azure AD 'nizi NetSuite 'in Kullanıcı hesabı sağlama API 'sine bağlama ve sağlama hizmeti 'ni, Azure AD 'de Kullanıcı ve grup atamasını temel alan NetSuite 'teki atanan kullanıcı hesaplarını oluşturmak, güncelleştirmek ve devre dışı bırakmak için yapılandırma işlemi kılavuzluk eder.

> [!TIP] 
> Ayrıca, [Azure Portal](https://portal.azure.com)sağlanan yönergeleri Izleyerek Netsuite için SAML tabanlı tek Sign-On de seçebilirsiniz. Çoklu oturum açma özelliği otomatik sağlanmadan bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini karmaşıdirebilirler.

### <a name="to-configure-user-account-provisioning"></a>Kullanıcı hesabı sağlamayı yapılandırmak için:

Bu bölümün amacı, Active Directory Kullanıcı hesaplarının NetSuite 'e Kullanıcı sağlamasını nasıl etkinleştireceğinizi özetler.

1. [Azure portal](https://portal.azure.com) **Azure Active Directory > Enterprise Apps > tüm uygulamalar** bölümüne gidin.

1. Zaten bir çoklu oturum açma için NetSuite yapılandırdıysanız, arama alanını kullanarak NetSuite örneğinizi arayın. Aksi takdirde, uygulama galerisinde **Netsuite** için **Ekle** ve ara ' yı seçin. Arama sonuçlarından NetSuite ' i seçin ve uygulama listenize ekleyin.

1. NetSuite örneğinizi seçin, sonra **sağlama** sekmesini seçin.

1. **Hazırlama Modu** 'nu **Otomatik** olarak ayarlayın. 

    ![Ekran görüntüsü, sağlama modu otomatik ve ayarlayabileceğiniz diğer değerlere ayarlanmış olan NetSuite sağlama sayfasını gösterir.](./media/netsuite-provisioning-tutorial/provisioning.png)

1. **Yönetici kimlik bilgileri** bölümünde aşağıdaki yapılandırma ayarlarını sağlayın:
   
    a. **Yönetici Kullanıcı adı** metin kutusuna Netsuite.com atanmış **Sistem Yöneticisi** profiline sahip bir Netsuite hesap adı yazın.
   
    b. **Yönetici parolası** metin kutusuna bu hesabın parolasını yazın.
      
1. Azure portal, Azure AD 'nin NetSuite uygulamanıza bağlanabildiğinden emin olmak için **Bağlantıyı Sına** ' ya tıklayın.

1. **Bildirim e-postası** alanına, sağlama hatası bildirimleri alması gereken kişinin veya grubun e-posta adresini girin ve onay kutusunu işaretleyin.

1. Kaydet ' e tıklayın **.**

1. Eşlemeler bölümünde **Azure Active Directory Kullanıcıları NetSuite olarak eşitler** ' ı seçin.

1. **Öznitelik eşlemeleri** bölümünde, Azure AD 'Den Netsuite 'e eşitlenen Kullanıcı özniteliklerini gözden geçirin. **Eşlenen** özellikler olarak seçilen özniteliklerin, güncelleştirme Işlemleri Için Netsuite içindeki kullanıcı hesaplarıyla eşleşecek şekilde kullanıldığını unutmayın. Değişiklikleri uygulamak için Kaydet düğmesini seçin.

1. NetSuite için Azure AD sağlama hizmetini etkinleştirmek üzere ayarlar bölümünde **sağlama durumunu** **Açık** olarak değiştirin

1. Kaydet ' e tıklayın **.**

Kullanıcılar ve Gruplar bölümünde NetSuite 'e atanan tüm Kullanıcı ve/veya grupların ilk eşitlemesini başlatır. İlk eşitlemenin daha sonra, hizmetin çalıştığı sürece yaklaşık 40 dakikada bir gerçekleşen sonraki eşitlemeler yerine gerçekleştirilmesini daha uzun sürdüğüne unutmayın. İlerleme durumunu izlemek için **eşitleme ayrıntıları** bölümünü kullanabilir ve NetSuite uygulamanızda sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklayan etkinlik günlüklerinin sağlanması için bağlantıları izleyebilirsiniz.

Azure AD sağlama günlüklerinin nasıl okunduğu hakkında daha fazla bilgi için bkz. [Otomatik Kullanıcı hesabı sağlamayı raporlama](../app-provisioning/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı hazırlamayı yönetme](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)
* [Çoklu oturum açmayı yapılandırma](netsuite-tutorial.md)
