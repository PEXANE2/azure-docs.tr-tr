---
title: 'Öğretici: Kullanıcı sağlama - LinkedIn Sales Navigator, Azure AD'
description: Azure Active Directory'yi LinkedIn Sales Navigator'a otomatik olarak sağlama ve kullanıcı hesaplarını sağlama ve sağlamadan çıkarma için nasıl yapılandırıyarıştırmayı öğrenin.
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
ms.openlocfilehash: 48b9f2dc64d1d3ddd8253a253dcab8ef972032f9
ms.sourcegitcommit: af1cbaaa4f0faa53f91fbde4d6009ffb7662f7eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81869737"
---
# <a name="tutorial-configure-linkedin-sales-navigator-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için LinkedIn Satış Gezgini'ni yapılandırın

Bu öğreticinin amacı, Azure AD'den LinkedIn Satış Gezgini'ne otomatik olarak kullanıcı hesapları sağlamak ve sağlamadan sağlamada LinkedIn Sales Navigator ve Azure AD'de gerçekleştirmeniz gereken adımları size göstermektir.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen senaryo, zaten aşağıdaki öğelere sahip olduğunuzu varsayar:

* Azure Active Directory kiracısı
* LinkedIn Satış Gezgini kiracı 
* LinkedIn Hesap Merkezi'ne erişimi olan LinkedIn Sales Navigator'da bir yönetici hesabı

> [!NOTE]
> Azure Active Directory, [SCIM](http://www.simplecloud.info/) protokolünü kullanarak LinkedIn Sales Navigator ile tümleşir.

## <a name="assigning-users-to-linkedin-sales-navigator"></a>LinkedIn Sales Navigator'a kullanıcı atama

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişmesi gerektiğini belirlemek için "atamalar" adlı bir kavram kullanır. Otomatik kullanıcı hesabı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya "atanmış" kullanıcılar ve gruplar eşitlenir.

Sağlama hizmetini yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki kullanıcıların ve/veya grupların LinkedIn Sales Navigator'a erişilmesi gereken kullanıcıları temsil ettiğini belirlemeniz gerekir. Karar verildikten sonra, bu kullanıcıları buradaki talimatları izleyerek LinkedIn Sales Navigator'a atayabilirsiniz:

[Bir kurumsal uygulamaya kullanıcı veya grup atama](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-linkedin-sales-navigator"></a>LinkedIn Sales Navigator'a kullanıcı atamak için önemli ipuçları

* Sağlama yapılandırmasını test etmek için tek bir Azure REKLAM kullanıcısının LinkedIn Sales Navigator'a atanması önerilir. Ek kullanıcılar ve/veya gruplar daha sonra atanabilir.

* Bir kullanıcıyı LinkedIn Sales Navigator'a atarken, atama iletişim **kutusundaki Kullanıcı** rolünü seçmeniz gerekir. "Varsayılan Erişim" rolü sağlama için çalışmaz.

## <a name="configuring-user-provisioning-to-linkedin-sales-navigator"></a>Kullanıcı sağlamayı LinkedIn Sales Navigator olarak yapılandırma

Bu bölüm, Azure REKLAM'ınızı LinkedIn Sales Navigator'ın SCIM kullanıcı hesabı sağlama API'sine bağlamanız ve sağlama hizmetini LinkedIn Sales Navigator'da atanan kullanıcı hesaplarını Oluşturmak, güncelleştirmek ve devre dışı edecek şekilde yapılandırmak yoluyla size yol gösterin.

> [!TIP]
> Azure [portalında](https://portal.azure.com)sağlanan yönergeleri izleyerek LinkedIn Sales Navigator için SAML tabanlı Tek Oturum Açma özelliğini de seçebilirsiniz. Tek oturum açma, otomatik sağlamadan bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini tamamlar.

### <a name="to-configure-automatic-user-account-provisioning-to-linkedin-sales-navigator-in-azure-ad"></a>Azure AD'deki LinkedIn Sales Navigator için otomatik kullanıcı hesabı sağlama yapılandırmak için:

İlk adım LinkedIn erişim jetonu almaktır. Bir Kurumsal yöneticiyseniz, bir erişim jetonuna kendi kendine sağlayabilirsiniz. Hesap merkezinizde **Ayarlar &gt; Global Ayarları'na** gidin ve **SCIM Kurulum** panelini açın.

> [!NOTE]
> Hesap merkezine bir bağlantı yerine doğrudan erişiyorsanız, aşağıdaki adımları kullanarak bu merkeze ulaşabilirsiniz.

1. Hesap Merkezi'nde oturum açın.

2. **Yönetici &gt; Yönetici Ayarlarını** Seçin.

3. Sol kenar çubuğunda **Gelişmiş Tümleştirmeler'i** tıklatın. Hesap merkezine yönlendirilirsiniz.

4. **+ Yeni SCIM yapılandırması ekleyin** ve her alanı doldurarak yordamı izleyin.

    > [!NOTE]
    > Otomatik atama lisansları etkinleştirilemediğinde, yalnızca kullanıcı verilerinin eşitlenmiş olduğu anlamına gelir.

    ![LinkedIn Satış Navigator Provizyon](./media/linkedinsalesnavigator-provisioning-tutorial/linkedin_1.PNG)

    > [!NOTE]
    > Otomatik lisans ataması etkinleştirildiğinde, uygulama örneğini ve lisans türünü not almanız gerekir. Lisanslar, tüm lisanslar alınana kadar ilk gelene, önce hizmet esasına göre atanır.

    ![LinkedIn Satış Navigator Provizyon](./media/linkedinsalesnavigator-provisioning-tutorial/linkedin_2.PNG)

5. **Belirteç Oluştur'u**tıklatın. **Access belirteci** alanının altında erişim belirteci ekranınızı görmeniz gerekir.

6. Sayfadan çıkmadan önce erişim belirtecinizi panonuza veya bilgisayarınıza kaydedin.

7. Ardından, [Azure portalında](https://portal.azure.com)oturum açın ve **Azure Active Directory > Kurumsal Uygulamalar > Tüm uygulamalar** bölümüne göz atın.

8. LinkedIn Sales Navigator'ı tek oturum açma için zaten yapılandırıldıysanız, arama alanını kullanarak LinkedIn Sales Navigator örneğini arayın. Aksi takdirde, uygulama galerisinde **LinkedIn Sales Navigator'ı** **ekle** ve arama yapın'ı seçin. Arama sonuçlarından LinkedIn Satış Gezgini'ni seçin ve uygulama listenize ekleyin.

9. LinkedIn Satış Gezgini örneğini seçin ve ardından **Sağlama** sekmesini seçin.

10. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

    ![LinkedIn Satış Navigator Provizyon](./media/linkedinsalesnavigator-provisioning-tutorial/linkedin_3.PNG)

11. **Yönetici Kimlik Bilgileri** altında aşağıdaki alanları doldurun:

    * Kiracı **URL** alanına girin. https://developer.linkedin.com

    * Gizli **Belirteç** alanında, adım 1'de oluşturduğunuz erişim jetonunu girin ve **Test Bağlantısı'nı** tıklatın.

    * Portalınızın sağ üst tarafında bir başarı bildirimi görmeniz gerekir.

12. **Bildirim E-posta** alanında ödeme hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve aşağıdaki onay kutusunu işaretleyin.

13. **Kaydet**’e tıklayın.

14. **Öznitelik Eşlemeleri** bölümünde, Azure AD'den LinkedIn Satış Gezgini'ne eşitlenecek kullanıcı ve grup özniteliklerini gözden geçirin. **Eşleştirme** özellikleri olarak seçilen özniteliklerin, güncelleştirme işlemleri için LinkedIn Sales Navigator'daki kullanıcı hesapları ve gruplarıyla eşleşecek şekilde kullanılacağını unutmayın. Herhangi bir değişiklik yapmak için Kaydet düğmesini seçin.

    ![LinkedIn Satış Navigator Provizyon](./media/linkedinsalesnavigator-provisioning-tutorial/linkedin_4.PNG)

15. LinkedIn Sales Navigator için Azure AD sağlama hizmetini etkinleştirmek **On** **için, Ayarlar** bölümünde Ki **Sağlama Durumunu**

16. **Kaydet**’e tıklayın.

Bu, Kullanıcılar ve Gruplar bölümünde LinkedIn Sales Navigator'a atanan kullanıcıların ve/veya grupların ilk senkronizasyonunu başlatır. İlk eşitlemenin, hizmet çalışırken yaklaşık her 40 dakikada bir oluşan sonraki eşitlemelerden daha uzun süreceğini unutmayın. LinkedIn Sales Navigator uygulamanızda sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklayan ilerlemeyi izlemek ve sağlama etkinlik günlüklerine olan bağlantıları izlemek için **Eşitleme Ayrıntıları** bölümünü kullanabilirsiniz.

Azure AD sağlama günlüklerini nasıl okuyabilirsiniz hakkında daha fazla bilgi için [bkz.](../app-provisioning/check-status-user-account-provisioning.md)

## <a name="additional-resources"></a>Ek Kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı sağlamanın yönetimi](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)
