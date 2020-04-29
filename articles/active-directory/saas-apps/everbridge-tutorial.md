---
title: 'Öğretici: her yaprak Köprüsü ile Azure Active Directory tümleştirme | Microsoft Docs'
description: Azure Active Directory ve tek köprü arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 58d7cd22-98c0-4606-9ce5-8bdb22ee8b3e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/18/2019
ms.author: jeedes
ms.openlocfilehash: 60463a00c6864bed7b3a18e816ef0143d3573782
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "67103251"
---
# <a name="tutorial-azure-active-directory-integration-with-everbridge"></a>Öğretici: her yaprak Köprüsü ile Azure Active Directory tümleştirme

Bu öğreticide, yaprak Köprüsü Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz.
Her bir Azure AD ile bir yaprak Köprüsü tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de her bir yaprak köprüye erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla bir yaprak 'ta otomatik olarak oturum açmaya izin verin. Bu erişim denetimine çoklu oturum açma (SSO) adı verilir.
* Azure portal kullanarak hesaplarınızı tek bir merkezi konumda yönetin.
Azure AD ile hizmet olarak yazılım (SaaS) uygulama tümleştirmesi hakkında daha fazla bilgi için bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/) .

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini her bir yaprak Köprüle yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Bir Azure AD aboneliği. Azure AD ortamınız yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Çoklu oturum açma kullanan bir yaprak Köprüsü aboneliği.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açmayı bir test ortamında yapılandırıp test edersiniz.

* Yaprak Köprüsü ıDP tarafından başlatılan SSO 'yu destekler.

## <a name="add-everbridge-from-the-azure-marketplace"></a>Azure Marketi 'nden yaprak Köprüsü ekleme

Her yaprak Köprüsü 'nün Azure AD ile tümleştirilmesini yapılandırmak için, Azure Marketi 'nden yönetilen SaaS uygulamaları listenize yaprak Köprüsü ekleyin.

Azure Marketi 'nden bir yaprak Köprüsü eklemek için aşağıdaki adımları izleyin.

1. [Azure Portal](https://portal.azure.com), sol gezinti bölmesinde **Azure Active Directory**' i seçin.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. **Kurumsal uygulamalar**' a gidin ve **tüm uygulamalar**' ı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** ' yı seçin.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna, **yaprak Köprüsü**girin. Sonuç panelinden her bir **yaprak Köprüsü** seçin ve **Ekle**' yi seçin.

     ![Sonuçlar listesinde yaprak Köprüsü](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma ve test etme

Bu bölümde, Azure AD çoklu oturum açmayı, bu Kullanıcı test kullanıcısına göre her ne kadar bas.
Çoklu oturum açma için, tek Köprü ' de bir Azure AD kullanıcısı ve ilgili Kullanıcı arasında bağlantı ilişkisi oluşturun.

Azure AD çoklu oturum açmayı tek köprü ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını doldurun:

- Kullanıcılarınızın bu özelliği kullanmasını sağlamak için [Azure AD çoklu oturum açmayı yapılandırın](#configure-azure-ad-single-sign-on) .
- Uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için her bir [yaprak köprü Yöneticisi portalı çoklu oturum açma olarak tek köprü yapılandırın](#configure-everbridge-as-everbridge-manager-portal-single-sign-on) .
- Uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için her bir [yaprak Köprüsü üye portalı çoklu oturum açma olarak yapılandırın](#configure-everbridge-as-everbridge-member-portal-single-sign-on) .
- Britta Simon ile Azure AD çoklu oturum açma sınamasını test etmek için [bir Azure AD test kullanıcısı oluşturun](#create-an-azure-ad-test-user) .
- Azure AD 'de çoklu oturum açma özelliğini kullanarak Britta Simon 'u etkinleştirmek için [Azure AD test kullanıcısını atayın](#assign-the-azure-ad-test-user) .
- Kullanıcının Azure AD gösterimine bağlı olan her bir yaprak Köprüsü içinde Britta Simon 'a sahip olmak için bir [yaprak Köprüsü test kullanıcısı oluşturun](#create-an-everbridge-test-user) .
- Yapılandırmanın çalışıp çalışmadığını doğrulamak için [Çoklu oturum açmayı test](#test-single-sign-on) edin.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure portal Azure AD çoklu oturum açma özelliğini etkinleştirirsiniz.

Azure AD çoklu oturum açma 'yı tek köprü ile yapılandırmak için aşağıdaki adımları izleyin.

1. [Azure Portal](https://portal.azure.com/), tek **köprü** uygulama tümleştirmesi sayfasında, **Çoklu oturum açma**' yı seçin.

    ![Çoklu oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. **Çoklu oturum açma yöntemi seç** iletişim kutusunda, çoklu oturum açmayı etkinleştirmek için **SAML/WS-Besme** modunu seçin.

    ![Çoklu oturum açma seçme modu](common/select-saml-option.png)

3. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **temel SAML yapılandırması** Iletişim kutusunu açmak için **Düzenle** ' yi seçin.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

    >[!NOTE]
    >Uygulamayı Yönetici portalı olarak *veya* hem Azure Portal hem de tek tek köprü portalında üye portalı olarak yapılandırın.

4. **Yaprak Köprüsü** uygulamasını her bir **yaprak Köprüsü Yönetici portalı**olarak YAPıLANDıRMAK Için, **temel SAML yapılandırması** bölümünde şu adımları izleyin:

    ![Yaprak Köprüsü etki alanı ve URL çoklu oturum açma bilgileri](common/idp-intiated.png)

    a. **Tanımlayıcı** kutusunda, kalıbı IZLEYEN bir URL girin`https://sso.everbridge.net/<API_Name>`

    b. **Yanıt URL 'si** kutusuna, kalıbı IZLEYEN bir URL girin`https://manager.everbridge.net/saml/SSO/<API_Name>/alias/defaultAlias`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek tanımlayıcı ve yanıt URL 'SI değerleriyle güncelleştirin. Bu değerleri almak için, [yaprak Köprüsü destek ekibine](mailto:support@everbridge.com)başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

5. **Yaprak** Köprüsü uygulamasını her **yaprak Köprüsü üye portalı**olarak YAPıLANDıRMAK Için, **temel SAML yapılandırması** bölümünde şu adımları izleyin:

  * Uygulamayı ıDP tarafından başlatılan modda yapılandırmak istiyorsanız aşağıdaki adımları izleyin:

     ![Her yaprak için etki alanı ve URL 'Ler ıDP tarafından başlatılan mod için çoklu oturum açma bilgileri](common/idp-intiated.png)

    a. **Tanımlayıcı** kutusunda, kalıbı IZLEYEN bir URL girin`https://sso.everbridge.net/<API_Name>/<Organization_ID>`

    b. **Yanıt URL 'si** kutusuna, kalıbı IZLEYEN bir URL girin`https://member.everbridge.net/saml/SSO/<API_Name>/<Organization_ID>/alias/defaultAlias`

   * Uygulamayı SP tarafından başlatılan modda yapılandırmak istiyorsanız **ek URL 'Ler ayarla** ' yı seçin ve bu adımı izleyin:

     ![Tek tek köprü etki alanı ve URL 'Ler SP tarafından başlatılan mod için çoklu oturum açma bilgileri](common/both-signonurl.png)

     a. **Oturum açma URL 'si** kutusuna, kalıbı IZLEYEN bir URL girin`https://member.everbridge.net/saml/login/<API_Name>/<Organization_ID>/alias/defaultAlias?disco=true`

     > [!NOTE]
     > Bu değerler gerçek değildir. Bu değerleri gerçek tanımlayıcı, yanıt URL 'si ve oturum açma URL 'SI değerleriyle güncelleştirin. Bu değerleri almak için, [yaprak Köprüsü destek ekibine](mailto:support@everbridge.com)başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

6. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imza sertifikası** bölümünde, **Federasyon meta veri XML**'sini indirmek için **İndir** ' i seçin. Bu dosyayı bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

7. Her bir **yaprak kümesi ayarla** bölümünde, gereksinimleriniz için Ihtiyaç duyduğunuz URL 'leri kopyalayın:

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

    - Oturum Açma URL’si
    - Azure AD tanımlayıcısı
    - Oturum kapatma URL 'SI

### <a name="configure-everbridge-as-everbridge-manager-portal-single-sign-on"></a>Her bir yaprak için tek köprü Yönetici portalı çoklu oturum açma olarak yapılandırma

Her bir **yaprak ÜZERINDE SSO** 'Yu bir **yaprak Köprüsü Yöneticisi Portal** uygulaması olarak yapılandırmak için aşağıdaki adımları izleyin.
 
1. Farklı bir Web tarayıcısı penceresinde, bir yönetici olarak yaprak olarak oturum açın.

1. Üstteki menüde **Ayarlar** sekmesini seçin. **Güvenlik**bölümünde **Çoklu oturum açma**seçeneğini belirleyin.
   
     ![Çoklu oturum açmayı yapılandırma](./media/everbridge-tutorial/tutorial_everbridge_002.png)
   
     a. **Ad** kutusuna, tanımlayıcı sağlayıcının adını girin. Şirketinizin adı bir örnektir.
   
     b. **API adı** kutusuna API 'nin adını girin.
   
     c. Azure portal indirdiğiniz meta veri dosyasını karşıya yüklemek için **Dosya Seç** ' i seçin.
   
     d. **SAML kimlik konumu**Için, **Konu ifadesinin NameIdentifier öğesinde kimlik**' i seçin.
   
     e. **Kimlik sağlayıcısı oturum açma URL 'si** kutusunda, Azure Portal kopyaladığınız **oturum açma URL 'si** değerini yapıştırın.
   
     f. **Hizmet sağlayıcı tarafından başlatılan Istek bağlama**Için **http yeniden yönlendirme**' yi seçin.

     g. **Kaydet**’i seçin.

### <a name="configure-everbridge-as-everbridge-member-portal-single-sign-on"></a>Her bir yaprak Köprüsü üye portalı çoklu oturum açma olarak yapılandırma

Her **bir yaprak Köprüsü** **üye portalı**olarak tek tek oturum açma 'yı yapılandırmak Için, INDIRILEN **Federasyon meta verileri XML** 'i tek [köprü desteği ekibine](mailto:support@everbridge.com)gönderin. Bu ayar, SAML SSO bağlantısının her iki tarafında da düzgün bir şekilde ayarlanmasını sağlamak üzere ayarlanmıştır.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma 

Azure portal test kullanıcısını, için bu adımları izleyin.

1. Azure Portal sol bölmedeki **Azure Active Directory** > **Kullanıcılar** > **tüm kullanıcılar**' ı seçin.

    ![Kullanıcılar ve tüm kullanıcılar bağlantıları](common/users.png)

2. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.

    ![Yeni Kullanıcı düğmesi](common/new-user.png)

3. **Kullanıcı** iletişim kutusunda, aşağıdaki adımları izleyin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** kutusuna **Brittasıon**yazın.
  
    b. **Kullanıcı adı** kutusuna `brittasimon@yourcompanydomain.extension` girin. BrittaSimon@contoso.com bunun bir örneğidir.

    c. **Parolayı göster** onay kutusunu seçin. **Parola** kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’u seçin.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Her bir yaprak köprüye erişim vererek Azure çoklu oturum açma özelliğini kullanmak için Britta Simon 'u etkinleştirin.

1. Azure Portal **Kurumsal uygulamalar** > **tüm uygulamalar** >tek**köprü**' i seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde, **yaprak Köprüsü**' nü seçin.

    ![Uygulamalar listesinde yaprak köprüsü bağlantısı](common/all-applications.png)

3. Soldaki menüde **Kullanıcılar ve gruplar**' ı seçin.

    ![Kullanıcılar ve gruplar bağlantısı](common/users-groups-blade.png)

4. **Kullanıcı Ekle**' yi seçin. **Atama Ekle** Iletişim kutusunda **Kullanıcılar ve gruplar**' ı seçin.

    ![Atama Ekle iletişim kutusu](common/add-assign-user.png)

5. **Kullanıcılar ve gruplar** iletişim kutusunda kullanıcılar listesinde **Britta Simon** ' u seçin. Ekranın alt kısmında **Seç ' i** seçin.

6. SAML onaylama 'da herhangi bir rol değeri bekleliyorsanız, **Rol Seç** iletişim kutusunda, Kullanıcı için listeden uygun rolü seçin. Ekranın alt kısmında **Seç ' i** seçin.

7. **Atama Ekle** Iletişim kutusunda **ata**' yı seçin.

### <a name="create-an-everbridge-test-user"></a>Bir yaprak Köprüsü test kullanıcısı oluşturma

Bu bölümde, her bir yaprak için Kullanıcı, Britta Simon ' u oluşturun. Her yaprak Köprüsü platformunda Kullanıcı eklemek için, her bir [yaprak Köprüsü destek ekibi](mailto:support@everbridge.com)ile çalışın. Çoklu oturum açma kullanılmadan önce kullanıcıların tek köprü içinde oluşturulması ve etkinleştirilmesi gerekir. 

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde her bir yaprak Köprüsü kutucuğunu seçtiğinizde, SSO 'yu ayarladığınız yaprak Köprüsü hesabında otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)
- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)
- [Azure Active Directory Koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

