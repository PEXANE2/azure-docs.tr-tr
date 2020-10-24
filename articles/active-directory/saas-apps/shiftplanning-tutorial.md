---
title: 'Öğretici: ınsanlarla Azure Active Directory tümleştirme | Microsoft Docs'
description: Azure Active Directory ve Insandaki çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 02/15/2019
ms.author: jeedes
ms.openlocfilehash: e1099f271291c6d86610237454e08e06b5fe48c7
ms.sourcegitcommit: 59f506857abb1ed3328fda34d37800b55159c91d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2020
ms.locfileid: "92522176"
---
# <a name="tutorial-azure-active-directory-integration-with-humanity"></a>Öğretici: ınsanlarla Azure Active Directory tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile ınsanın nasıl tümleştirileceğini öğreneceksiniz.
Insanların Azure AD ile tümleştirilmesi aşağıdaki avantajları sağlar:

* Azure AD 'de ınsana erişimi olan denetim yapabilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla ınsanda (çoklu oturum açma) otomatik olarak oturum açmasını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz-Azure portal.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek istiyorsanız, bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesini ınsanlarla yapılandırmak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Bir Azure AD ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü edinebilirsiniz
* İnsandaki çoklu oturum açma etkin abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açmayı bir test ortamında yapılandırıp test edersiniz.

* İnsansızlık, **SP** tarafından başlatılan SSO 'yu destekler

## <a name="adding-humanity-from-the-gallery"></a>Galeriden Insanlar ekleme

Azure AD 'de insanın tümleştirmesini yapılandırmak için, Galeriden yönetilen SaaS uygulamaları listenize ınsana eklemeniz gerekir.

**Galeriden Insanlar eklemek için aşağıdaki adımları uygulayın:**

1. **[Azure Portal](https://portal.azure.com)** sol gezinti panelinde **Azure Active Directory** simgesine tıklayın.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar** seçeneğini belirleyin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için, iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesine tıklayın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **insana**yazın, sonuç panelinden **insanlar** ' ı seçin ve sonra uygulamayı eklemek için **Ekle** düğmesine tıklayın.

     ![Sonuçlar listesindeki insanlar](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma ve test etme

Bu bölümde, Azure AD çoklu oturum açmayı, **Britta Simon**adlı bir test kullanıcısına göre insanla yapılandırıp test edersiniz.
Çoklu oturum açma için, bir Azure AD kullanıcısı ve Insandaki ilgili Kullanıcı arasındaki bağlantı ilişkisinin oluşturulması gerekir.

Azure AD çoklu oturum açma 'yı ınsanla yapılandırmak ve test etmek için aşağıdaki yapı taşlarını gerçekleştirmeniz gerekir:

1. **[Azure AD çoklu oturum açma özelliğini yapılandırarak](#configure-azure-ad-single-sign-on)** kullanıcılarınızın bu özelliği kullanmasına olanak sağlayın.
2. **[İnsandaki çoklu oturum açmayı yapılandırma](#configure-humanity-single-sign-on)** -uygulama tarafında tek Sign-On ayarlarını yapılandırmak için.
3. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -Britta Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
4. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanarak Britta Simon 'u etkinleştirin.
5. Kullanıcının Azure AD gösterimine bağlı olan insandaki Britta Simon 'un bir karşılığı olan **[insanın bir test kullanıcısı oluşturun](#create-humanity-test-user)** .
6. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[Çoklu oturum açmayı sınayın](#test-single-sign-on)** .

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure portal Azure AD çoklu oturum açma özelliğini etkinleştirirsiniz.

Azure AD çoklu oturum açma 'yı ınsanlarla yapılandırmak için aşağıdaki adımları uygulayın:

1. [Azure Portal](https://portal.azure.com/), **insanın** uygulama tümleştirmesi sayfasında, **Çoklu oturum açma**' yı seçin.

    ![Çoklu oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Çoklu oturum **açma yöntemi seç** iletişim kutusunda, çoklu oturum açmayı etkinleştirmek için **SAML/WS-Besme** modunu seçin.

    ![Çoklu oturum açma seçme modu](common/select-saml-option.png)

3. **SAML Ile tek Sign-On ayarlama** sayfasında, **temel SAML yapılandırması** Iletişim kutusunu açmak için **Düzenle** simgesine tıklayın.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. **Temel SAML yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![İnsandaki etki alanı ve URL 'Ler çoklu oturum açma bilgileri](common/sp-identifier.png)

    a. **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://company.humanity.com/includes/saml/`

    b. **Tanımlayıcı (VARLıK kimliği)** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://company.humanity.com/app/`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri, gerçek oturum açma URL 'SI ve tanımlayıcısı ile güncelleştirin. Bu değerleri almak için [Humanity istemci destek ekibine](https://www.humanity.com/support/) başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

4. **SAML Ile tek Sign-On ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde, **sertifika (base64)** ' i gereksiniminize göre ve bilgisayarınıza kaydetmek için **İndir** ' e tıklayın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. **Insanlığı ayarla** bölümünde uygun URL 'leri gereksiniminize göre kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD tanımlayıcısı

    c. Oturum kapatma URL 'SI

### <a name="configure-humanity-single-sign-on"></a>Insandaki tek Sign-On yapılandırma

1. Farklı bir Web tarayıcısı penceresinde, **insandaki** şirket sitenizde yönetici olarak oturum açın.

2. Üstteki menüden **yönetici**' ye tıklayın.

    ![Yönetici](./media/shiftplanning-tutorial/iC786619.png "Yönetici")
3. **Tümleştirme**altında **Çoklu oturum açma**' ya tıklayın.

    ![Ekran görüntüsü, tümleştirme menüsünden Seçili tek Sign-On gösterir.](./media/shiftplanning-tutorial/iC786620.png "Çoklu Oturum Açma")

4. **Çoklu oturum açma** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Ekran görüntüsü, açıklanan değerleri girebileceğiniz tek Sign-On bölümünü gösterir.](./media/shiftplanning-tutorial/iC786905.png "Çoklu Oturum Açma")

    a. **SAML etkin**' i seçin.

    b. **Parola oturum açmaya Izin ver**' i seçin.

    c. **SAML veren URL** metin kutusunda, Azure Portal kopyaladığınız **oturum açma URL 'si** değerini yapıştırın.

    d. **Uzaktan oturum kapatma URL 'si** metin kutusunda, Azure Portal kopyaladığınız **oturum kapatma URL 'si** değerini yapıştırın.

    e. Base-64 kodlu sertifikanızı Not defteri 'nde açın, içeriğini panonuza kopyalayın ve **X. 509.440 sertifikası** metin kutusuna yapıştırın.

    f. **Ayarları Kaydet**' e tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Azure portal Britta Simon adlı bir test kullanıcısı oluşturmaktır.

1. Azure portal, sol bölmedeki **Azure Active Directory**' i seçin, **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.

    !["Kullanıcılar ve gruplar" ve "tüm kullanıcılar" bağlantıları](common/users.png)

2. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.

    ![Yeni Kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı Özellikleri ' nde aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanına **Brittasıon**girin.
  
    b. **Kullanıcı adı** alanına ** \@ bricompansıon yourcompanydomain. Extension** yazın  
    Örneğin, BrittaSimon@contoso.com

    c. **Parolayı göster** onay kutusunu seçin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, ınsana erişim sağlayarak Azure çoklu oturum açma özelliğini kullanmak için Britta Simon 'u etkinleştirin.

1. Azure portal **Kurumsal uygulamalar**' ı seçin, **tüm uygulamalar**' ı seçin, sonra da **insanlar**' ı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **insanlar**' ı seçin.

    ![Uygulamalar listesindeki Insandaki bağlantı](common/all-applications.png)

3. Soldaki menüde **Kullanıcılar ve gruplar**' ı seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. **Kullanıcı Ekle** düğmesine tıklayın, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. **Kullanıcılar ve gruplar** Iletişim kutusunda kullanıcılar listesinde **Britta Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

6. SAML onaylama işlemi içinde herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, listeden Kullanıcı için uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

7. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

### <a name="create-humanity-test-user"></a>Insandaki bir test kullanıcısı oluşturun

Azure AD kullanıcılarının, ınsanda oturum açmasını sağlamak için, bunların insanda sağlanması gerekir. Insanın olması durumunda, sağlama el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. **İnsandaki** şirket sitenizde yönetici olarak oturum açın.

2. **Yönetici**' ye tıklayın.

    ![Yönetici](./media/shiftplanning-tutorial/iC786619.png "Yönetici")

3. **Personel**' e tıklayın.

    ![Personeli](./media/shiftplanning-tutorial/ic786623.png "Personeli")

4. **Eylemler**altında, **çalışan Ekle**' ye tıklayın.

    ![Çalışan Ekle](./media/shiftplanning-tutorial/iC786624.png "Çalışan Ekle")

5. **Çalışanları Ekle** bölümünde aşağıdaki adımları uygulayın:

    ![Çalışanları Kaydet](./media/shiftplanning-tutorial/iC786625.png "Çalışanları Kaydet")

    a. İlgili metin kutularına sağlamak istediğiniz geçerli bir Azure AD hesabının **adını**, **soyadını**ve **e-postasını** yazın.

    b. **Çalışanları kaydet**' e tıklayın.

> [!NOTE]
> Azure AD Kullanıcı hesapları sağlamak için Insanlar tarafından sunulan diğer Insansız Kullanıcı hesabı oluşturma araçlarını veya API 'Leri kullanabilirsiniz.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim panelinde Insandaki bir kutucuğa tıkladığınızda, SSO 'yu ayarladığınız ınsanda otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](../user-help/my-apps-portal-end-user-access.md).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](./tutorial-list.md)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

- [Azure Active Directory Koşullu erişim nedir?](../conditional-access/overview.md)