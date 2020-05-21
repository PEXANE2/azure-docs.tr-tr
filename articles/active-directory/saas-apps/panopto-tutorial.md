---
title: 'Öğretici: Panopto ile tümleştirme Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve Panopto arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 89c88e23-93ce-4970-9baa-1104c4e8fe4a
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/17/2019
ms.author: jeedes
ms.openlocfilehash: a1d4836586bac359c3208147ec7bf59061cf0928
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83648600"
---
# <a name="tutorial-azure-active-directory-integration-with-panopto"></a>Öğretici: Panopto ile tümleştirme Azure Active Directory

Bu öğreticide, Panopto 'ı Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz.
Panopto 'ı Azure AD ile tümleştirmek aşağıdaki avantajları sağlar:

* Panopto 'a erişimi olan Azure AD 'de denetim yapabilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla Panopto (çoklu oturum açma) ile otomatik olarak oturum açmasını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz-Azure portal.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek istiyorsanız, bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/) .

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Panopto ile yapılandırmak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Bir Azure AD ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü edinebilirsiniz
* Panopto çoklu oturum açma etkin abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açmayı bir test ortamında yapılandırıp test edersiniz.

* Panopto **SP** tarafından başlatılan SSO 'yu destekler

* Panopto 'ın **tam zamanında** Kullanıcı sağlamasını destekliyor

## <a name="adding-panopto-from-the-gallery"></a>Galeriden Panopto ekleme

Panopto 'ın Azure AD Ile tümleşmesini yapılandırmak için Galeri 'den yönetilen SaaS uygulamaları listenize Panopto 'A eklemeniz gerekir.

**Galeriden Panopto eklemek için aşağıdaki adımları uygulayın:**

1. **[Azure Portal](https://portal.azure.com)** sol gezinti panelinde **Azure Active Directory** simgesine tıklayın.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar** seçeneğini belirleyin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için, iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesine tıklayın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Panopto**yazın, sonuç panelinden **Panopto** ' ı seçin ve sonra uygulamayı eklemek için **Ekle** düğmesine tıklayın.

     ![Sonuçlar listesinde Panopto](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma ve test etme

Bu bölümde, **Britta Simon**adlı bir test kullanıcısına göre Panopto Ile Azure AD çoklu oturum açmayı yapılandırıp test edersiniz.
Çoklu oturum açma için, bir Azure AD kullanıcısı ve Panopto 'daki ilgili Kullanıcı arasındaki bağlantı ilişkisinin oluşturulması gerekir.

Azure AD çoklu oturum açma 'yı Panopto ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını gerçekleştirmeniz gerekir:

1. **[Azure AD çoklu oturum açma özelliğini yapılandırarak](#configure-azure-ad-single-sign-on)** kullanıcılarınızın bu özelliği kullanmasına olanak sağlayın.
2. Uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için **[Panopto çoklu oturum açmayı yapılandırın](#configure-panopto-single-sign-on)** .
3. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -Britta Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
4. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanarak Britta Simon 'u etkinleştirin.
5. Kullanıcının Azure AD gösterimine bağlı olan Panetto 'da Britta Simon 'ın bir karşılığı olacak şekilde **[kullanıcıyı test etmek Için Panopto oluşturma](#create-panopto-test-user)** .
6. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[Çoklu oturum açmayı sınayın](#test-single-sign-on)** .

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure portal Azure AD çoklu oturum açma özelliğini etkinleştirirsiniz.

Azure AD çoklu oturum açmayı Panopto ile yapılandırmak için aşağıdaki adımları uygulayın:

1. [Azure Portal](https://portal.azure.com/), **Panopto** uygulama tümleştirmesi sayfasında, **Çoklu oturum açma**' yı seçin.

    ![Çoklu oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Çoklu oturum **açma yöntemi seç** iletişim kutusunda, çoklu oturum açmayı etkinleştirmek için **SAML/WS-Besme** modunu seçin.

    ![Çoklu oturum açma seçme modu](common/select-saml-option.png)

3. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **temel SAML yapılandırması** Iletişim kutusunu açmak için **Düzenle** simgesine tıklayın.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. **Temel SAML yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Panopto etki alanı ve URL 'Ler çoklu oturum açma bilgileri](common/sp-signonurl.png)

    **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<tenant-name>.panopto.com`

    > [!NOTE]
    > Değer gerçek değil. Değeri, gerçek oturum açma URL 'SI ile güncelleştirin. Değeri almak için [Panopto istemci destek ekibine](mailto:support@panopto.com) başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

5. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imza sertifikası** bölümünde, **Federasyon meta veri XML** 'sini gereksiniminize göre belirtilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir** ' e tıklayın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

6. **Panetto ayarla** bölümünde uygun URL 'leri gereksiniminize göre kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD tanımlayıcısı

    c. Oturum kapatma URL 'SI

### <a name="configure-panopto-single-sign-on"></a>Panopto çoklu oturum açmayı yapılandırma

1. Farklı bir Web tarayıcısı penceresinde, Panopto şirket sitenizde yönetici olarak oturum açın.

2. Soldaki araç çubuğunda, **sistem**' e ve ardından **kimlik sağlayıcıları**' na tıklayın.
   
    ![Sistem](./media/panopto-tutorial/ic777670.png "Sistem")

3. **Sağlayıcı Ekle**' ye tıklayın.
   
    ![Kimlik Sağlayıcıları](./media/panopto-tutorial/ic777671.png "Kimlik Sağlayıcıları")
   
4. SAML sağlayıcısı bölümünde aşağıdaki adımları uygulayın:
   
    ![SaaS yapılandırması](./media/panopto-tutorial/ic777672.png "SaaS yapılandırması")
    
    a. **Sağlayıcı türü** listesinden **SAML20**öğesini seçin.    
    
    b. **Örnek adı** metin kutusuna örnek için bir ad yazın.

    c. **Kolay açıklama** metin kutusunda kolay bir açıklama yazın.
    
    d. **Sıçrama sayfası URL 'si** metin kutusunda, Azure Portal kopyaladığınız **oturum açma URL 'si**değerini yapıştırın.

    e. **Veren** metin kutusunda, Azure Portal KOPYALADıĞıNıZ **Azure AD tanımlayıcısının**değerini yapıştırın.

    f. Azure portal indirdiğiniz temel 64 kodlu sertifikanızı açın, içindeki içeriğini panonuza kopyalayın ve sonra **ortak anahtar** metin kutusuna yapıştırın.

5. **Kaydet**’e tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Azure portal Britta Simon adlı bir test kullanıcısı oluşturmaktır.

1. Azure portal, sol bölmedeki **Azure Active Directory**' i seçin, **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.

    !["Kullanıcılar ve gruplar" ve "tüm kullanıcılar" bağlantıları](common/users.png)

2. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.

    ![Yeni Kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı Özellikleri ' nde aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanına **Brittasıon**girin.
  
    b. **Kullanıcı adı** alan türü**brittasimon@yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

    c. **Parolayı göster** onay kutusunu seçin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**' a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Panetto 'a erişim vererek Azure çoklu oturum açma özelliğini kullanmak için Britta Simon 'u etkinleştirin.

1. Azure portal **Kurumsal uygulamalar**' ı seçin, **tüm uygulamalar**' ı seçin ve ardından **Panopto**' ı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde, **Panopto**' ı seçin.

    ![Uygulamalar listesinde Panopto bağlantısı](common/all-applications.png)

3. Soldaki menüde **Kullanıcılar ve gruplar**' ı seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. **Kullanıcı Ekle** düğmesine tıklayın, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. **Kullanıcılar ve gruplar** Iletişim kutusunda kullanıcılar listesinde **Britta Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

6. SAML onaylama işlemi içinde herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, listeden Kullanıcı için uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

7. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

### <a name="create-panopto-test-user"></a>Kullanıcıyı test etmek için Panopopi oluştur

Bu bölümde, Britta Simon adlı bir Kullanıcı Panopto 'da oluşturulur. Panopto, varsayılan olarak etkinleştirilen tam zamanında Kullanıcı sağlamayı destekler. Bu bölümde sizin için herhangi bir eylem öğesi yok. Bir Kullanıcı Panopto 'da zaten mevcut değilse, kimlik doğrulamasından sonra yeni bir tane oluşturulur.

>[!NOTE]
>Azure AD Kullanıcı hesapları sağlamak için Panopto tarafından sunulan diğer bir panopya Kullanıcı hesabı oluşturma araçlarını veya API 'Leri kullanabilirsiniz.
>

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim panelinde Panopto kutucuğunu tıklattığınızda, SSO 'yu ayarladığınız Panopto 'da otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory Koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

