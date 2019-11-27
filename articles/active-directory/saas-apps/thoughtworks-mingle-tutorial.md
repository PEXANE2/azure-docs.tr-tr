---
title: 'Öğretici: En Iyi şekilde çalışmak için Mingle ile tümleştirmeyi Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ile, En Iyi şekilde çalışması için en az bir oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 69d859d9-b7f7-4c42-bc8c-8036138be586
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/27/2019
ms.author: jeedes
ms.openlocfilehash: a12d4dca61734275ef0e56dfe2a800c64dc52540
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74233299"
---
# <a name="tutorial-azure-active-directory-integration-with-thoughtworks-mingle"></a>Öğretici: En Iyi şekilde çalışmak için Mingle ile tümleştirme Azure Active Directory

Bu öğreticide, Azure Active Directory (Azure AD) ile en iyi şekilde, en iyi çalışmalarla nasıl tümleştirileceğini öğreneceksiniz.
Azure AD ile En Iyi şekilde yararlanmak için, aşağıdaki avantajları sağlar:

* Azure AD 'de, En Iyi şekilde çalışan bir Mingle erişimi olan denetim yapabilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla, En Iyi şekilde (çoklu oturum açma) Mini olarak oturum açmasını sağlayabilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek istiyorsanız, bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesini, En Iyi şekilde kullanmanız gereken Mingle ile yapılandırmak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Bir Azure AD ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü edinebilirsiniz
* Düşünte, Mingle çoklu oturum açma etkin aboneliği

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açmayı bir test ortamında yapılandırıp test edersiniz.

* En iyi yöntem, **SP** tarafından başlatılan SSO 'yu destekler

## <a name="adding-thoughtworks-mingle-from-the-gallery"></a>Galeriden En Iyi şekilde çalışma

Azure AD 'de, Düşünfile sorunsuz çalışma Mingle tümleştirmesini yapılandırmak için, galerinizden yönetilen SaaS uygulamaları listenize, En Iyi şekilde kullanmanız gerekir.

**Galerinin En Iyi şekilde çalışması için, aşağıdaki adımları uygulayın:**

1. **[Azure Portal](https://portal.azure.com)** sol gezinti panelinde **Azure Active Directory** simgesine tıklayın.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar** seçeneğini belirleyin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için, iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesine tıklayın.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna, en **iyi**sonuçları yazın, sonuç panelinden, daha sonra **Ekle** düğmesine tıklayarak uygulamayı ekleyin.

     ![Düşünte, sonuçlar listesinde Mingle ile çalışmaktadır](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, Azure AD çoklu oturum açmayı, **Britta Simon**adlı bir test kullanıcısına bağlı olarak, bir test kullanıcısına göre daha iyi bir şekilde yapılandırır ve test edersiniz.
Çoklu oturum açma için, bir Azure AD kullanıcısı ile ilgili Kullanıcı arasındaki bir bağlantı ilişkisinin, En Iyi durumda çalışan Mingle üzerinde oluşturulması gerekir.

Azure AD çoklu oturum açmayı, daha Iyi çalışmalarla birlikte yapılandırmak ve test etmek için aşağıdaki yapı taşlarını gerçekleştirmeniz gerekir:

1. **[Azure AD çoklu oturum açma özelliğini yapılandırarak](#configure-azure-ad-single-sign-on)** kullanıcılarınızın bu özelliği kullanmasına olanak sağlayın.
2. , Uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için, en **[az oturum açma, En Iyi oturum açma ayarlarını yapılandırın](#configure-thoughtworks-mingle-single-sign-on)** .
3. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -Britta Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
4. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanarak Britta Simon 'u etkinleştirin.
5. Bir kullanıcının Azure AD gösterimine bağlı olan, en iyi durumda, düşünmem için en az bir Kullanıcı **[test kullanıcısı oluşturun](#create-thoughtworks-mingle-test-user)** .
6. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[Çoklu oturum açmayı sınayın](#test-single-sign-on)** .

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure portal Azure AD çoklu oturum açma özelliğini etkinleştirirsiniz.

Azure AD çoklu oturum açmayı, En Iyi şekilde dikkatli bir şekilde yapılandırmak için, aşağıdaki adımları uygulayın:

1. [Azure Portal](https://portal.azure.com/), en **iyi yöntem** uygulama tümleştirmesi sayfasında, **Çoklu oturum açma**' yı seçin.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Çoklu oturum **açma yöntemi seç** iletişim kutusunda, çoklu oturum açmayı etkinleştirmek için **SAML/WS-Besme** modunu seçin.

    ![Çoklu oturum açma seçme modu](common/select-saml-option.png)

3. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **temel SAML yapılandırması** Iletişim kutusunu açmak için **Düzenle** simgesine tıklayın.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. **Temel SAML yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Düşünte, Mingle etki alanı ve URL 'Lerde çoklu oturum açma bilgileri çalışmaktadır](common/sp-signonurl.png)

    **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın: `https://<companyname>.mingle.thoughtworks.com`

    > [!NOTE]
    > Değer gerçek değil. Değeri, gerçek oturum açma URL 'SI ile güncelleştirin. Değeri almak için, En Iyi şekilde dikkatli olmak üzere [Mingle istemci destek ekibine](https://support.thoughtworks.com/hc/categories/201743486-Mingle-Community-Support) başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

5. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imza sertifikası** bölümünde, **Federasyon meta veri XML** 'sini gereksiniminize göre belirtilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir** ' e tıklayın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

6. En uygun URL 'leri **Ayarla** bölümünde, gereksiniminize göre uygun URL 'leri kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-thoughtworks-mingle-single-sign-on"></a>En Iyi şekilde çalışma için en az oturum açmayı yapılandırma

1. Bir yönetici olarak, en **iyi şekilde Mingle** şirket sitenizde oturum açın.

2. **Yönetici** sekmesine tıklayın ve ardından **SSO yapılandırması**' na tıklayın.
   
    ![Yönetici sekmesi](./media/thoughtworks-mingle-tutorial/ic785157.png "SSO yapılandırması")

3. **SSO yapılandırma** bölümünde aşağıdaki adımları uygulayın:
   
    ![SSO yapılandırması](./media/thoughtworks-mingle-tutorial/ic785158.png "SSO yapılandırması")
    
    a. Meta veri dosyasını karşıya yüklemek için **Dosya Seç**' e tıklayın. 

    b. **Değişiklikleri Kaydet**' e tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portal, sol bölmedeki **Azure Active Directory**' i seçin, **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.

    ![Yeni Kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanına **Brittasıon**girin.
  
    b. **Kullanıcı adı** alanına brittasimon@yourcompanydomain.extensionyazın. Örneğin, BrittaSimon@contoso.com

    c. **Parolayı göster** onay kutusunu seçin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**'a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, bir Azure çoklu oturum açma özelliğini kullanarak, En Iyi şekilde kullanmanız gerekir.

1. Azure portal **Kurumsal uygulamalar**' ı seçin, **tüm uygulamalar**' **ı seçin ve**ardından, daha sonra, daha sonra, daha sonra,

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde, en **iyi**durumda, ' ı seçin.

    ![En Iyi uygulama listesi, uygulamalar listesinde Mingle bağlantısı](common/all-applications.png)

3. Soldaki menüde **Kullanıcılar ve gruplar**' ı seçin.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. **Kullanıcı Ekle** düğmesine tıklayın, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. **Kullanıcılar ve gruplar** Iletişim kutusunda kullanıcılar listesinde **Britta Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

6. SAML onaylama işlemi içinde herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, listeden Kullanıcı için uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

7. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

### <a name="create-thoughtworks-mingle-test-user"></a>En Iyi şekilde test eden Mini test kullanıcısı oluşturma

Azure AD kullanıcılarının oturum açabilmeleri için, Azure Active Directory Kullanıcı adlarını kullanarak bu kişilerin, En Iyi şekilde kullanılması gerekir. Düşünmeme durumunda Mingle çalışıyor durumunda, sağlama el ile gerçekleştirilen bir görevdir.

**Kullanıcı sağlamayı yapılandırmak için aşağıdaki adımları uygulayın:**

1. Bir yönetici olarak, En Iyi şekilde Mingle şirket sitenizde oturum açın.

2. **Profil**' e tıklayın.
   
    ![Ilk projeniz](./media/thoughtworks-mingle-tutorial/ic785160.png "Ilk projeniz")

3. **Yönetici** sekmesine tıklayın ve ardından **Kullanıcılar**' a tıklayın.
   
    ![Kullanıcılar](./media/thoughtworks-mingle-tutorial/ic785161.png "Kullanıcılar")

4. **Yeni Kullanıcı**' ya tıklayın.
   
    ![Yeni Kullanıcı](./media/thoughtworks-mingle-tutorial/ic785162.png "Yeni Kullanıcı")

5. **Yeni Kullanıcı** iletişim sayfasında, aşağıdaki adımları uygulayın:
   
    ![Yeni Kullanıcı iletişim kutusu](./media/thoughtworks-mingle-tutorial/ic785163.png "Yeni Kullanıcı")  
 
    a. **Oturum açma adı**, **görünen ad**, **parola Seç**, ilgili metin KUTULARıNA sağlamak istediğiniz geçerli bir Azure AD hesabının **parolasını onaylayın** . 

    b. **Kullanıcı türü**olarak, **tam Kullanıcı**' yı seçin.

    c. **Bu profili oluştur**' a tıklayın.

>[!NOTE]
>Azure AD Kullanıcı hesapları sağlamak için, daha iyi bir kullanıcı hesabı oluşturma araçları veya En Iyi şekilde kullanmanız gereken mini kullanılan API 'Leri kullanabilirsiniz.
> 

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde, en iyi şekilde çalışır durumda, SSO 'yu ayarladığınız ınfetle için otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory Koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

