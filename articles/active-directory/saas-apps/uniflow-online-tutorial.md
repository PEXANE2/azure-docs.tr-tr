---
title: 'Öğretici: en düşük çevrimiçi ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ile çevrimiçi olarak çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 28313d27-638c-4d50-92ad-d093f2ae9ecf
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/02/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: f081d6b01ffb0b4fc78c23a076c20ff93c483203
ms.sourcegitcommit: 1e6c13dc1917f85983772812a3c62c265150d1e7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86166969"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-uniflow-online"></a>Öğretici: uniFLOW Online ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory

Bu öğreticide, Azure Active Directory (Azure AD) ile Ifkılow online 'ı tümleştirmeyi öğreneceksiniz. Azure AD ile Ifkılow online 'ı tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de, hiçbir Unkılow online 'a erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla çevrimiçi olmadan oturum açmasını sağlayın.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

Azure AD ile SaaS uygulaması tümleştirmesi hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* En düşük çevrimiçi Kiracı.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz.

* ıkılow Online, **SP** tarafından başlatılan SSO 'yu destekler

## <a name="adding-uniflow-online-from-the-gallery"></a>Galeriden çevrimiçi olmayan çevrimiçi ekleme

Ikılow online 'ın tümleştirmesini Azure AD 'ye göre yapılandırmak için, Galeriden, Galeri 'den yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. [Azure Portal](https://portal.azure.com) iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **çevrimiçi olmayan** ' ı yazın.
1. Sonuçlar panelinden **olmayan çevrimiçi** ' i seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-uniflow-online"></a>Azure AD çoklu oturum açmayı yapılandırma ve test etme-Iflow online

**B. Simon**adlı bir test kullanıcısı kullanarak Azure AD SSO 'Yu Unkılow Online ile yapılandırın ve test edin. SSO 'nun çalışması için bir Azure AD kullanıcısı ile ilgili Kullanıcı arasında bir bağlantı ilişkisi oluşturmanız gerekir.

Azure AD SSO 'yu yapılandırmak ve test etmek için şu yapı taşlarını doldurun:

1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
   1. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -B. Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
   1. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek için.
1. Uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için **[Unkılow ONLINE SSO 'Yu yapılandırın](#configure-uniflow-online-sso)** .
    * Uygulama tarafında Kullanıcı oturumunu test etmek için **[oluşturulan test kullanıcısı ' nı kullanarak hiç bir çevrimiçi ortamda oturum açın](#sign-in-to-uniflow-online-using-the-created-test-user)** .

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. [Azure Portal](https://portal.azure.com/), **çevrimiçi** uygulama tümleştirmesi sayfasında, **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** için Düzenle/kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. **Temel SAML yapılandırması** bölümünde, aşağıdaki alanlar için değerleri girin:

    a. **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:

    - `https://<tenant_domain_name>.eu.uniflowonline.com`
    - `https://<tenant_domain_name>.us.uniflowonline.com`
    - `https://<tenant_domain_name>.sg.uniflowonline.com`
    - `https://<tenant_domain_name>.jp.uniflowonline.com`
    - `https://<tenant_domain_name>.au.uniflowonline.com`

    b. **Tanımlayıcı (VARLıK kimliği)** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:

    - `https://<tenant_domain_name>.eu.uniflowonline.com`
    - `https://<tenant_domain_name>.us.uniflowonline.com`
    - `https://<tenant_domain_name>.sg.uniflowonline.com`
    - `https://<tenant_domain_name>.jp.uniflowonline.com`
    - `https://<tenant_domain_name>.au.uniflowonline.com`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri, gerçek oturum açma URL 'SI ve tanımlayıcısı ile güncelleştirin. Bu değerleri almak için, en [düşük çevrimiçi istemci desteği ekibine](mailto:support@nt-ware.com) başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz veya diğer bir çevrimiçi kiracınızda GÖSTERILEN yanıt URL 'sine başvurabilirsiniz.

1. En düşük çevrimiçi uygulama, SAML belirteci öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML onayları bekler. Aşağıdaki ekran görüntüsünde varsayılan özniteliklerin listesi gösterilmektedir.

    ![image](common/default-attributes.png)

1. Yukarıdakilerin yanı sıra, en düşük çevrimiçi uygulama aşağıda gösterilen SAML yanıtına daha fazla öznitelik geçirilmesini bekler. Bu öznitelikler de önceden doldurulur, ancak gereksinimlerinize göre bunları gözden geçirebilirsiniz.

    | Ad |  Kaynak özniteliği|
    | -----------| --------------- |
    | DisplayName | User. DisplayName |
    | takma ad | User. onpremisessamaccountname |

   > [!NOTE]
   > `user.onpremisessamaccountname`Özniteliği yalnızca Azure AD kullanıcılarınız yerel bir Windows Active Directory eşitlendiğinde bir değer içerir.

1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde, **uygulama Federasyon meta verileri URL 'sini** kopyalamak ve bilgisayarınıza kaydetmek için Kopyala düğmesine tıklayın.

    ![Sertifika indirme bağlantısı](common/copy-metadataurl.png)

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, B. Simon adlı Azure portal bir test kullanıcısı oluşturacaksınız.

1. Azure portal sol bölmeden **Azure Active Directory**' i seçin, **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.
1. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.
1. **Kullanıcı** özellikleri ' nde şu adımları izleyin:
   1. **Ad** alanına `B.Simon` girin.  
   1. **Kullanıcı adı** alanına, girin username@companydomain.extension . Örneğin, `B.Simon@contoso.com`.
   1. **Parolayı göster** onay kutusunu seçin ve ardından **parola** kutusunda görüntülenen değeri yazın.
   1. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Azure çoklu oturum açma özelliğini kullanarak, Ifkılow online 'a erişim vererek B. Simon 'u etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde, **çevrimiçi değil**' i seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümüne gidin ve **Kullanıcılar ve gruplar**' ı seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle**' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

   ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. SAML assertion 'da herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, Kullanıcı için listeden uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

> [!NOTE]
> Tüm kullanıcıların elle atama olmadan uygulamaya erişmesine izin vermek için, **Yönet** bölümüne gidin ve **Özellikler**' i seçin. Ardından, **Kullanıcı Ataması gerekli** parametresini **Hayır**olarak değiştirin.

## <a name="configure-uniflow-online-sso"></a>Unkılow online SSO yapılandırma

1. Farklı bir Web tarayıcısı penceresinde, yönetici olarak olmayan çevrimiçi Web sitesinde oturum açın.

1. Sol Gezinti panelinden **Kullanıcı** sekmesi ' ni seçin.

    ![En düşük çevrimiçi yapılandırma](./media/uniflow-online-tutorial/configure1.png)

1. **Kimlik sağlayıcıları**' na tıklayın.

    ![En düşük çevrimiçi yapılandırma](./media/uniflow-online-tutorial/configure2.png)

1. **Kimlik sağlayıcısı ekle**' ye tıklayın.

    ![En düşük çevrimiçi yapılandırma](./media/uniflow-online-tutorial/configure3.png)

1. **KIMLIK sağlayıcısı ekle** bölümünde aşağıdaki adımları uygulayın:


    ![En düşük çevrimiçi yapılandırma](./media/uniflow-online-tutorial/configure4.png)

    a. Ex: *Azuread SSO*görünen adını girin.

    b. **Sağlayıcı türü**için açılan listeden **WS-Besme** seçeneğini belirleyin.

    c. **WS-bestype**için açılan listeden **Azure Active Directory** seçeneğini belirleyin.

    d. **Kaydet**’e tıklayın.

1. **Genel** sekmesinde, aşağıdaki adımları uygulayın:

    ![En düşük çevrimiçi yapılandırma](./media/uniflow-online-tutorial/configure5.png)

    a. Ex: *Azuread SSO*görünen adını girin.

    b. **ADFS Federasyon meta verileri**için **Kimden URL 'si** seçeneğini belirleyin.

    c. **Federasyon meta veri URl 'si** metin kutusunda, Azure Portal kopyaladığınız **uygulama Federasyon meta veri URL 'si** değerini yapıştırın.

    d. **Kimlik sağlayıcısını** **etkin**olarak seçin.

    e. **Otomatik Kullanıcı kaydını** **etkinleştirildi**olarak seçin.

    f. **Kaydet**’e tıklayın.

### <a name="sign-in-to-uniflow-online-using-the-created-test-user"></a>Oluşturulan test kullanıcısını kullanarak Kaldırılow online 'da oturum açın

1. Farklı bir Web tarayıcısı penceresinde, kiracınız için Unkılow online URL 'sine gidin.

1. Azure AD örneğiniz aracılığıyla oturum açmak için önceden oluşturulmuş kimlik sağlayıcısını seçin.

1. Test kullanıcısını kullanarak oturum açın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile hiç çevrimiçi olmadan deneyin](https://aad.portal.azure.com/)
