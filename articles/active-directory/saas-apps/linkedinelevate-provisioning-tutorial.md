---
title: 'Öğretici: LinkedIn Yükseltme için kullanıcı sağlama - Azure AD'
description: Azure Active Directory'yi LinkedIn Elevate'e otomatik olarak sağlama ve kullanıcı hesaplarını sağlama ve sağlamadan çıkarma için nasıl yapılandırılamayı öğrenin.
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
ms.openlocfilehash: fa0a26eaeac431ed2c78c5bd938bbbe7dff14e0e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77057422"
---
# <a name="tutorial-configure-linkedin-elevate-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için LinkedIn Yükseltme'yi yapılandırın

Bu öğreticinin amacı, Azure AD'den LinkedIn Yükseltme'ye otomatik olarak kullanıcı hesapları sağlamak ve sağlamadan sağlamak için LinkedIn Yükseltme ve Azure AD'de gerçekleştirmeniz gereken adımları size göstermektir.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, zaten aşağıdaki öğelere sahip olduğunuzu varsayar:

* Azure Active Directory kiracısı
* LinkedIn Yükseltme kiracısı
* LinkedIn Hesap Merkezi'ne erişimi olan LinkedIn Elevate'deki bir yönetici hesabı

> [!NOTE]
> Azure Active Directory, [SCIM](http://www.simplecloud.info/) protokolünü kullanarak LinkedIn Yükseltme ile tümleşir.

## <a name="assigning-users-to-linkedin-elevate"></a>LinkedIn Yükseltme'ye kullanıcı atama

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişmesi gerektiğini belirlemek için "atamalar" adlı bir kavram kullanır. Otomatik kullanıcı hesabı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya "atanmış" kullanıcılar ve gruplar eşitlenir.

Sağlama hizmetini yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki kullanıcıların ve/veya grupların LinkedIn Yükseltmes'e erişilmesi gereken kullanıcıları temsil edeceğine karar vermeniz gerekir. Karar verildikten sonra, bu kullanıcıları buradaki yönergeleri izleyerek LinkedIn Yükseltme'ye atayabilirsiniz:

[Bir kurumsal uygulamaya kullanıcı veya grup atama](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-linkedin-elevate"></a>LinkedIn Elevate'e kullanıcı atamak için önemli ipuçları

* Sağlama yapılandırmasını sınamak için tek bir Azure REKLAM kullanıcısının LinkedIn Yükseltme'ye atanması önerilir. Ek kullanıcılar ve/veya gruplar daha sonra atanabilir.

* LinkedIn Yükseltme'ye bir kullanıcı atarken, atama iletişim **kutusundaki Kullanıcı** rolünü seçmeniz gerekir. "Varsayılan Erişim" rolü sağlama için çalışmaz.

## <a name="configuring-user-provisioning-to-linkedin-elevate"></a>Kullanıcı sağlamayı LinkedIn Yükseltme olarak yapılandırma

Bu bölüm, Azure REKLAM'ınızı LinkedIn Elevate'in SCIM kullanıcı hesabı sağlama API'sine bağlamanız ve kullanıcı ve grup atamasına bağlı olarak LinkedIn Elevate'de atanan kullanıcı hesaplarını oluşturacak, güncelleştirecek ve devre dışı edecek şekilde sağlama hizmetini yapılandırmanız konusunda size yol gösteriyor Azure AD'de.

**İpucu:** [Azure portalında](https://portal.azure.com)sağlanan yönergeleri izleyerek LinkedIn Yükseltmesi için SAML tabanlı Tek Oturum Açma özelliğini de etkinleştirmeyi seçebilirsiniz. Tek oturum açma, otomatik sağlamadan bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini tamamlar.

### <a name="to-configure-automatic-user-account-provisioning-to-linkedin-elevate-in-azure-ad"></a>Azure AD'de LinkedIn Yükseltme için otomatik kullanıcı hesabı sağlama yapılandırmak için:

İlk adım LinkedIn erişim jetonu almaktır. Bir Kurumsal yöneticiyseniz, bir erişim jetonuna kendi kendine sağlayabilirsiniz. Hesap merkezinizde **Ayarlar &gt; Global Ayarları'na** gidin ve **SCIM Kurulum** panelini açın.

> [!NOTE]
> Hesap merkezine bir bağlantı yerine doğrudan erişiyorsanız, aşağıdaki adımları kullanarak bu merkeze ulaşabilirsiniz.

1. Hesap Merkezi'nde oturum açın.

2. **Yönetici &gt; Yönetici Ayarlarını** Seçin.

3. Sol kenar çubuğunda **Gelişmiş Tümleştirmeler'i** tıklatın. Hesap merkezine yönlendirilirsiniz.

4. **+ Yeni SCIM yapılandırması ekleyin** ve her alanı doldurarak yordamı izleyin.

    > [!NOTE]
    > Otomatik atama lisansları etkinleştirilemediğinde, yalnızca kullanıcı verilerinin eşitlenmiş olduğu anlamına gelir.

    ![LinkedIn Yükseltme Sağlama](./media/linkedinelevate-provisioning-tutorial/linkedin_elevate1.PNG)

    > [!NOTE]
    > Otomatik lisans ataması etkinleştirildiğinde, uygulama örneğini ve lisans türünü not almanız gerekir. Lisanslar, tüm lisanslar alınana kadar ilk gelene, önce hizmet esasına göre atanır.

    ![LinkedIn Yükseltme Sağlama](./media/linkedinelevate-provisioning-tutorial/linkedin_elevate2.PNG)

5. **Belirteç Oluştur'u**tıklatın. **Access belirteci** alanının altında erişim belirteci ekranınızı görmeniz gerekir.

6. Sayfadan çıkmadan önce erişim belirtecinizi panonuza veya bilgisayarınıza kaydedin.

7. Ardından, [Azure portalında](https://portal.azure.com)oturum açın ve **Azure Active Directory > Kurumsal Uygulamalar > Tüm uygulamalar** bölümüne göz atın.

8. LinkedIn Yükseltme'yi tek oturum açma için zaten yapılandırıldıysanız, arama alanını kullanarak LinkedIn Yükseltme örneğinizi arayın. Aksi takdirde, uygulama galerisinde **LinkedIn** **Yükseltme** ekle ve arama yı seçin. Arama sonuçlarından LinkedIn Yükseltme'yi seçin ve uygulama listenize ekleyin.

9. LinkedIn Yükseltme örneğini seçin ve ardından **Sağlama** sekmesini seçin.

10. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

    ![LinkedIn Yükseltme Sağlama](./media/linkedinelevate-provisioning-tutorial/linkedin_elevate3.PNG)

11. **Yönetici Kimlik Bilgileri** altında aşağıdaki alanları doldurun:

    * Kiracı **URL** alanına girin. `https://api.linkedin.com`

    * Gizli **Belirteç** alanında, adım 1'de oluşturduğunuz erişim jetonunu girin ve **Test Bağlantısı'nı** tıklatın.

    * Portalınızın sağ üst tarafında bir başarı bildirimi görmeniz gerekir.

12. **Bildirim E-posta** alanında ödeme hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve aşağıdaki onay kutusunu işaretleyin.

13. **Kaydet**'e tıklayın.

14. **Öznitelik Eşlemeleri** bölümünde, Azure AD'den LinkedIn Yükseltme'ye eşitlenecek kullanıcı ve grup özniteliklerini gözden geçirin. **Eşleştirme** özellikleri olarak seçilen özniteliklerin, güncelleştirme işlemleri için LinkedIn Yükseltme'deki kullanıcı hesapları ve gruplarıyla eşleştirmek için kullanılacağını unutmayın. Herhangi bir değişiklik yapmak için Kaydet düğmesini seçin.

    ![LinkedIn Yükseltme Sağlama](./media/linkedinelevate-provisioning-tutorial/linkedin_elevate4.PNG)

15. LinkedIn Yükseltmes için Azure AD sağlama hizmetini etkinleştirmek için, **ıstaşlama Durumunu** **Ayarlar** bölümünde açık **On**

16. **Kaydet**'e tıklayın.

Bu, LinkedIn Yükseltme'ye atanan kullanıcıların ve/veya grupların Kullanıcılar ve Gruplar bölümünde ilk eşitlenmesine başlar. İlk eşitlemenin, hizmet çalışırken yaklaşık her 40 dakikada bir oluşan sonraki eşitlemelerden daha uzun süreceğini unutmayın. LinkedIn Yükseltme uygulamanızda sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklayan ilerlemeyi izlemek ve sağlama etkinlik günlüklerine olan bağlantıları izlemek için **Eşitleme Ayrıntıları** bölümünü kullanabilirsiniz.

Azure AD sağlama günlüklerini nasıl okuyabilirsiniz hakkında daha fazla bilgi için [bkz.](../app-provisioning/check-status-user-account-provisioning.md)

## <a name="additional-resources"></a>Ek Kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı sağlamanın yönetimi](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)
