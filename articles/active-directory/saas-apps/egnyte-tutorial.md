---
title: 'Öğretici: Egnyut ile tümleştirme Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve Egnde arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 8c2101d4-1779-4b36-8464-5c1ff780da18
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 01/31/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7f8eb0f0d566d656436da11141ea7f6c521b7b82
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "76983733"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-egnyte"></a>Öğretici: Egnyut ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory

Bu öğreticide, Azure Active Directory (Azure AD) ile nasıl tümleştirileceğini öğreneceksiniz. Azure AD ile Egnylei tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de, Egnde erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla otomatik olarak oturum açmalarına olanak sağlar.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

Azure AD ile SaaS uygulaması tümleştirmesi hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* EGN, çoklu oturum açma (SSO) etkin abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açmayı bir test ortamında yapılandırıp test edersiniz.

* Egnse, **SP** tarafından başlatılan SSO 'yu destekler
* Egnorut 'ları yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin bir kısmını gerçek zamanlı olarak koruyan oturum denetimini zorunlu kılabilirsiniz. Oturum denetimi koşullu erişimden genişletilir. [Microsoft Cloud App Security ile oturum denetimini nasıl zorlayacağınızı öğrenin](https://docs.microsoft.com/cloud-app-security/proxy-deployment-aad)

## <a name="adding-egnyte-from-the-gallery"></a>Galeriden Egnklerden ekleme

Egnyleyler 'in Azure AD ile tümleştirilmesini yapılandırmak için, Galeriden yönetilen SaaS uygulamaları listenize Egnler eklemeniz gerekir.

1. [Azure Portal](https://portal.azure.com) iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **egnlele,** yazın.
1. Sonuçlar panelinden **Egnut** ' ı seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma ve test etme

Bu bölümde, Azure AD çoklu oturum açma 'yı, **Britta Simon**adlı bir test kullanıcısına bağlı olarak, egnylele yapılandırıp test edersiniz.
Çoklu oturum açma 'nın çalışması için, bir Azure AD kullanıcısı ile ilgili Kullanıcı arasındaki bağlantı ilişkisinin kurulması gerekir.

Azure AD çoklu oturum açma 'yı Egnler ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını gerçekleştirmeniz gerekir:

Azure AD SSO 'yu yapılandırmak ve test etmek için şu yapı taşlarını doldurun:

1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
    1. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -B. Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
    1. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek için.
1. Uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için **[EGN, SSO 'Yu yapılandırın](#configure-egnyte-sso)** .
    1. **[Egnlarca test kullanıcısı oluşturun](#create-egnyte-test-user)** -bu, kullanıcının Azure AD gösterimine bağlı olan, Egnlarca 'da B. Simon 'ın bir karşılığı olacak şekilde.
1. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Bu bölümde, Azure portal Azure AD çoklu oturum açma özelliğini etkinleştirirsiniz.

Azure AD çoklu oturum açma 'yı Egnde ile yapılandırmak için aşağıdaki adımları uygulayın:

1. [Azure Portal](https://portal.azure.com/), **EGN,** uygulama tümleştirmesi sayfasında, **Çoklu oturum açma**' yı seçin.

    ![Çoklu oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Çoklu oturum **açma yöntemi seç** iletişim kutusunda, çoklu oturum açmayı etkinleştirmek için **SAML/WS-Besme** modunu seçin.

    ![Çoklu oturum açma seçme modu](common/select-saml-option.png)

3. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **temel SAML yapılandırması** Iletişim kutusunu açmak için **Düzenle** simgesine tıklayın.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. **Temel SAML yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![EGN, etki alanı ve URL 'Ler çoklu oturum açma bilgileri](common/sp-signonurl.png)

    **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<companyname>.egnyte.com`

    > [!NOTE]
    > Değer gerçek değil. Değeri, gerçek oturum açma URL 'SI ile güncelleştirin. Değeri almak için [EGN, istemci destek ekibine](https://www.egnyte.com/corp/contact_egnyte.html) başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

4. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde, **sertifika (base64)** ' i gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir** ' e tıklayın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

5. **Egnlarca ayarla** bölümünde uygun URL 'leri gereksiniminize göre kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD tanımlayıcısı

    c. Oturum kapatma URL 'SI

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Azure portal Britta Simon adlı bir test kullanıcısı oluşturmaktır.

1. Azure portal, sol bölmedeki **Azure Active Directory**' i seçin, **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.

    !["Kullanıcılar ve gruplar" ve "tüm kullanıcılar" bağlantıları](common/users.png)

2. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.

    ![Yeni Kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı Özellikleri ' nde aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanına **Brittasıon**girin.
  
    b. **Kullanıcı adı** alanına **bricompansıon\@yourcompanydomain. Extension** yazın  
    Örneğin, BrittaSimon@contoso.com

    c. **Parolayı göster** onay kutusunu seçin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**' a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, erişim izni vererek Azure çoklu oturum açma özelliğini kullanmak için Britta Simon 'u etkinleştirin.

1. Azure portal **Kurumsal uygulamalar**' ı seçin, **tüm uygulamalar**' ı seçin ve ardından **egnde**' yi seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde, **Egnde**' yi seçin.

    ![Uygulamalar listesindeki Egnbir bağlantı](common/all-applications.png)

3. Soldaki menüde **Kullanıcılar ve gruplar**' ı seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. **Kullanıcı Ekle** düğmesine tıklayın, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. **Kullanıcılar ve gruplar** Iletişim kutusunda kullanıcılar listesinde **Britta Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

6. SAML onaylama işlemi içinde herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, listeden Kullanıcı için uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

7. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

## <a name="configure-egnyte-sso"></a>EGN, SSO 'yu yapılandırma

1. Farklı bir Web tarayıcısı penceresinde, Egnyut şirket sitenizde yönetici olarak oturum açın.

2. **Ayarlar**'a tıklayın.
   
    ![Ayarlar](./media/egnyte-tutorial/ic787819.png "Ayarlar")

3. Menüsünde, **Ayarlar**' a tıklayın.

    ![Ayarlar](./media/egnyte-tutorial/ic787820.png "Ayarlar")

4. **Yapılandırma** sekmesine tıklayın ve ardından **güvenlik**' e tıklayın.

    ![Güvenlik](./media/egnyte-tutorial/ic787821.png "Güvenlik")

5. **Çoklu oturum açma kimlik doğrulaması** bölümünde aşağıdaki adımları uygulayın:

    ![Çoklu oturum açma kimlik doğrulaması](./media/egnyte-tutorial/ic787822.png "Çoklu oturum açma kimlik doğrulaması")   
    
    a. **Çoklu oturum açma kimlik doğrulaması**olarak **SAML 2,0**' i seçin.
   
    b. **Kimlik sağlayıcısı**olarak **azuread**' i seçin.
   
    c. Azure portal 'den kopyalanmış **oturum açma URL** 'sini **kimlik sağlayıcısı oturum açma URL 'si** metin kutusuna kopyalayın.
   
    d. Azure portal kopyaladığınız **Azure ad tanımlayıcısını** **KIMLIK sağlayıcısı varlık kimliği** metin kutusuna yapıştırın.
      
    e. Azure portal 'den indirilen Not defteri 'nde Base-64 kodlu sertifikanızı açın, içeriğini panonuza kopyalayın ve ardından **kimlik sağlayıcısı sertifikası** metin kutusuna yapıştırın.
   
    f. **Varsayılan kullanıcı eşlemesi**olarak **e-posta adresi**' ni seçin.
   
    g. **Etki alanına özgü veren değerini kullanmak**için **devre dışı**' yı seçin.
   
    h. **Kaydet**’e tıklayın.

### <a name="create-egnyte-test-user"></a>EGN, test kullanıcısı oluşturma

Azure AD kullanıcılarının, Egnler 'de oturum açmasını sağlamak için, bunların Egnda sağlanması gerekir. Bu durumda, sağlama işlemi el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. **Egnyi** şirket sitenizde yönetici olarak oturum açın.

2. **Ayarlar \> kullanıcılar & gruplar**' a gidin.

3. **Yeni Kullanıcı Ekle**' ye tıklayın ve sonra eklemek istediğiniz kullanıcı türünü seçin.
   
    ![Kullanıcılar](./media/egnyte-tutorial/ic787824.png "Kullanıcılar")

4. **Yeni Power User** bölümünde aşağıdaki adımları uygulayın:
    
    ![Yeni standart Kullanıcı](./media/egnyte-tutorial/ic787825.png "Yeni standart Kullanıcı")   

    a. **E-posta** metin kutusuna, **\@brittasıon contoso.com**gibi kullanıcının e-postasını girin.

    b. Kullanıcı **adı** metin kutusuna, **Brittasıon**gibi kullanıcının Kullanıcı adını girin.

    c. **Kimlik doğrulama türü**olarak **Çoklu oturum açma** seçeneğini belirleyin.
   
    d. **Kaydet**’e tıklayın.
    
    >[!NOTE]
    >Azure Active Directory hesap sahibi bir bildirim e-postası alır.
    >

>[!NOTE]
>Azure AD Kullanıcı hesaplarını sağlamak için, EGNX 'Ler tarafından sunulan diğer tüm Kullanıcı hesabı oluşturma araçlarını veya API 'Leri kullanabilirsiniz.
>

### <a name="test-sso"></a>Test SSO 'SU

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim paneli ' nde Egnayttan Kutucuğa tıkladığınızda, SSO 'yu ayarladığınız erişimde otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory Koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Microsoft Cloud App Security oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)
