---
title: 'Öğretici: Kullanıcı hazırlama-LinkedIn Sales Navigator, Azure AD'
description: LinkedIn Sales Navigator 'a Kullanıcı hesaplarını otomatik olarak sağlamak ve serbest bırakmak için Azure Active Directory yapılandırmayı öğrenin.
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
ms.openlocfilehash: dbf8923d62b49ed5341776aef03bc1bc2dabeaa9
ms.sourcegitcommit: 653e9f61b24940561061bd65b2486e232e41ead4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74276815"
---
# <a name="tutorial-configure-linkedin-sales-navigator-for-automatic-user-provisioning"></a>Öğretici: otomatik Kullanıcı sağlaması için LinkedIn Sales Navigator 'ı yapılandırma

Bu öğreticinin amacı, Azure AD 'den LinkedIn Sales Navigator 'a Kullanıcı hesaplarını otomatik olarak sağlamak ve devre dışı bırakmak için LinkedIn Sales Navigator ve Azure AD 'de gerçekleştirmeniz gereken adımları gösteriyoruz.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen senaryo, aşağıdaki öğelerin zaten olduğunu varsayar:

* Azure Active Directory kiracısı
* LinkedIn Sales Navigator kiracısı 
* LinkedIn hesap merkezine erişimi olan LinkedIn Sales Navigator 'da yönetici hesabı

> [!NOTE]
> Azure Active Directory, [SCIM](http://www.simplecloud.info/) protokolünü kullanarak LinkedIn Sales Navigator ile tümleşir.

## <a name="assigning-users-to-linkedin-sales-navigator"></a>LinkedIn Sales Navigator 'a Kullanıcı atama

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişim alacağını belirleyebilmek için "atamalar" adlı bir kavram kullanır. Otomatik Kullanıcı hesabı sağlama bağlamında, yalnızca Azure AD 'de bir uygulamaya "atanmış" olan kullanıcılar ve gruplar eşitlenir.

Sağlama hizmetini yapılandırmadan ve etkinleştirmeden önce, Azure AD 'deki hangi kullanıcıların ve/veya grupların LinkedIn Sales Navigator 'a erişmesi gereken kullanıcıları temsil ettiğini belirlemeniz gerekir. Karar verdikten sonra buradaki yönergeleri izleyerek bu kullanıcıları LinkedIn Sales Navigator 'a atayabilirsiniz:

[Kurumsal uygulamaya Kullanıcı veya Grup atama](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-linkedin-sales-navigator"></a>LinkedIn Sales Navigator 'a Kullanıcı atamaya yönelik önemli ipuçları

* Sağlama yapılandırmasını test etmek için LinkedIn Sales Navigator 'a tek bir Azure AD kullanıcısının atanması önerilir. Ek kullanıcılar ve/veya grupları daha sonra atanabilir.

* LinkedIn Sales Navigator 'a bir Kullanıcı atarken, atama iletişim kutusunda **Kullanıcı** rolünü seçmeniz gerekir. "Varsayılan erişim" rolü sağlama için çalışmaz.

## <a name="configuring-user-provisioning-to-linkedin-sales-navigator"></a>LinkedIn Sales Navigator 'a Kullanıcı sağlamayı yapılandırma

Bu bölümde, Azure AD 'nizi LinkedIn Sales Navigator 'ın SCıM Kullanıcı hesabı sağlama API 'sine bağlama ve sağlama hizmeti 'ni Kullanıcı tabanlı LinkedIn Sales Navigator 'daki atanan kullanıcı hesaplarını oluşturmak, güncelleştirmek ve devre dışı bırakmak için yapılandırma işlemi kılavuzluk eder. Azure AD 'de grup ataması.

> [!TIP]
> Ayrıca, [Azure Portal](https://portal.azure.com)' de sağlanan yönergeleri Izleyerek LinkedIn Sales NAVIGATOR için SAML tabanlı çoklu oturum açma 'yı da etkinleştirebilirsiniz. Çoklu oturum açma özelliği otomatik sağlanmadan bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini tamamlayabilse de.

### <a name="to-configure-automatic-user-account-provisioning-to-linkedin-sales-navigator-in-azure-ad"></a>Azure AD 'de LinkedIn Sales Navigator 'a otomatik Kullanıcı hesabı sağlamayı yapılandırmak için:

İlk adım, LinkedIn erişim belirtecinizi almak için kullanılır. Kuruluş yöneticisiyseniz, kendi kendine erişim belirteci sağlayabilirsiniz. Hesap merkezinizde **ayarlar &gt; genel ayarlar** ' a gidin ve **SCIM kurulum** panelini açın.

> [!NOTE]
> Hesap merkezine bir bağlantı yerine doğrudan erişiyorsanız, aşağıdaki adımları kullanarak buna ulaşabilirsiniz.

1. Hesap Merkezi 'nde oturum açın.

2. Yönetici **&gt; yönetici ayarları** ' nı seçin.

3. Sol kenar çubuğunda **Gelişmiş tümleştirmeler** ' e tıklayın. Hesap merkezine yönlendirilirsiniz.

4. **+ Yenı SCIM Yapılandırması Ekle** ' ye tıklayın ve her alanı doldurarak yordamı izleyin.

    > [!NOTE]
    > Yeniden atama lisansları etkin olmadığında, yalnızca Kullanıcı verilerinin eşitlendiği anlamına gelir.

    ![LinkedIn Sales Navigator sağlama](./media/linkedinsalesnavigator-provisioning-tutorial/linkedin_1.PNG)

    > [!NOTE]
    > Oto lisans ataması etkinleştirildiğinde, uygulama örneği ve lisans türünü not etmeniz gerekir. Lisanslar ilk olarak atanır, ilk olarak tüm lisanslar alınana kadar temel alınır.

    ![LinkedIn Sales Navigator sağlama](./media/linkedinsalesnavigator-provisioning-tutorial/linkedin_2.PNG)

5. **Belirteç oluştur**' a tıklayın. Erişim belirteci, erişim **belirteci** alanının altında görüntülenir.

6. Sayfadan çıkmadan önce erişim belirtecinizi panonuza veya bilgisayarınıza kaydedin.

7. Ardından [Azure Portal](https://portal.azure.com)oturum açın ve **Azure Active Directory > Enterprise Apps > tüm uygulamalar** bölümüne gidin.

8. Çoklu oturum açma için LinkedIn Sales Navigator 'ı zaten yapılandırdıysanız, arama alanını kullanarak LinkedIn Sales Navigator örneğinizi arayın. Aksi takdirde, uygulama galerisinde **LinkedIn Sales Navigator** **Ekle** ve ara ' yı seçin. Arama sonuçlarından LinkedIn Sales Navigator ' ı seçin ve uygulama listenize ekleyin.

9. LinkedIn Sales Navigator örneğinizi seçin ve **sağlama** sekmesini seçin.

10. **Sağlama modunu** **Otomatik**olarak ayarlayın.

    ![LinkedIn Sales Navigator sağlama](./media/linkedinsalesnavigator-provisioning-tutorial/linkedin_3.PNG)

11. **Yönetici kimlik bilgileri** altında aşağıdaki alanları girin:

    * **Kiracı URL 'si** alanında https://api.linkedin.comgirin.

    * **Gizli belirteç** alanına, adım 1 ' de oluşturduğunuz erişim belirtecini girin ve **Bağlantıyı Sına** ' yı tıklatın.

    * Portalınızın sağ tarafında bir başarı bildirimi görmeniz gerekir.

12. **Bildirim e-postası** alanında sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve aşağıdaki onay kutusunu işaretleyin.

13. **Save (Kaydet)** düğmesine tıklayın.

14. **Öznitelik eşlemeleri** bölümünde, Azure AD 'Den LinkedIn Sales Navigator 'a eşitlenecek Kullanıcı ve grup özniteliklerini gözden geçirin. Güncelleştirme işlemleri için LinkedIn Sales Navigator 'daki Kullanıcı hesaplarını ve grupları eşleştirmek için, **eşleşen** özellikler olarak seçilen özniteliklerin kullanılacağını unutmayın. Değişiklikleri kaydetmek için Kaydet düğmesini seçin.

    ![LinkedIn Sales Navigator sağlama](./media/linkedinsalesnavigator-provisioning-tutorial/linkedin_4.PNG)

15. LinkedIn Sales Navigator için Azure AD sağlama hizmeti 'ni etkinleştirmek üzere **Ayarlar** bölümünde **sağlama durumunu** **Açık** olarak değiştirin

16. **Save (Kaydet)** düğmesine tıklayın.

Bu, kullanıcılar ve Gruplar bölümünde LinkedIn Sales Navigator 'a atanan tüm Kullanıcı ve/veya grupların ilk eşitlemesini başlatır. İlk eşitlemenin daha sonra, hizmetin çalıştığı sürece yaklaşık 40 dakikada bir gerçekleşen sonraki eşitlemeler yerine daha uzun süreceğini unutmayın. **Eşitleme ayrıntıları** bölümünü Izleyip, LinkedIn Sales Navigator uygulamanızda sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklayan etkinlik günlüklerinin sağlanması için kullanılacak bağlantıları izleyebilirsiniz.

Azure AD sağlama günlüklerinin nasıl okunduğu hakkında daha fazla bilgi için bkz. [Otomatik Kullanıcı hesabı sağlamayı raporlama](../manage-apps/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Ek Kaynaklar

* [Kurumsal uygulamalar için Kullanıcı hesabı sağlamayı yönetme](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)
