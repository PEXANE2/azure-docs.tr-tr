---
title: 'Öğretici: artışlarını devralarak LMS ile tümleştirme Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve artışlarını devralarak LMS arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: ba9f1b3d-a4a0-4ff7-b0e7-428e0ed92142
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/02/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: d3d5dbd5c831b4e3cf2ab63796165dc6bad005a5
ms.sourcegitcommit: 4e5560887b8f10539d7564eedaff4316adb27e2c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87905851"
---
# <a name="tutorial-azure-active-directory-integration-with-absorb-lms"></a>Öğretici: artışlarını devralarak LMS ile tümleştirme Azure Active Directory

Bu öğreticide, artışlarını devralarak LMS 'yi Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz.
Artışlarını devralarak LMS 'yi Azure AD ile tümleştirmek aşağıdaki avantajları sağlar:

* Artışlarını devralarak LMS erişimi olan Azure AD 'de denetim yapabilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla artışlarını devralarak LMS (çoklu oturum açma) ile otomatik olarak oturum açmasını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz-Azure portal.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek istiyorsanız, bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesini artışlarını devralarak LMS ile yapılandırmak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Bir Azure AD ortamınız yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/) alabilirsiniz
* Artışlarını devralarak LMS çoklu oturum açma etkin abonelik

> [!NOTE]
> Bu tümleştirme Ayrıca Azure AD ABD kamu bulut ortamından kullanılabilir. Bu uygulamayı Azure AD ABD kamu bulutu uygulama galerisinde bulabilir ve bunu ortak buluttan yaptığınız şekilde yapılandırabilirsiniz.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açmayı bir test ortamında yapılandırıp test edersiniz.

* Artışlarını devralarak LMS, **IDP** tarafından başlatılan SSO 'yu destekler

## <a name="adding-absorb-lms-from-the-gallery"></a>Galeriden artışlarını devralarak LMS ekleme

Artışlarını devralarak LMS 'nin tümleştirmesini Azure AD 'ye göre yapılandırmak için galerideki artışlarını devralarak LMS 'yi yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Galeriden artışlarını devralarak LMS eklemek için aşağıdaki adımları uygulayın:**

1. **[Azure Portal](https://portal.azure.com)** sol gezinti panelinde **Azure Active Directory** simgesine tıklayın.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar** seçeneğini belirleyin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için, iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesine tıklayın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **ARTıŞLARıNı DEVRALARAK LMS**yazın, sonuç PANELINDEN **artışlarını devralarak LMS** ' yi seçin ve ardından **Ekle** düğmesine tıklayarak uygulamayı ekleyin.

     ![Sonuç listesinde artışlarını devralarak LMS](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma ve test etme

Bu bölümde, **Britta Simon**adlı bir test kullanıcısına göre artışlarını devralarak LMS Ile Azure AD çoklu oturum açmayı yapılandırıp test edersiniz.
Çoklu oturum açma için, bir Azure AD kullanıcısı ve artışlarını devralarak LMS içindeki ilgili Kullanıcı arasındaki bağlantı ilişkisinin kurulması gerekir.

Azure AD çoklu oturum açmayı artışlarını devralarak LMS ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını gerçekleştirmeniz gerekir:

1. **[Azure AD çoklu oturum açma özelliğini yapılandırarak](#configure-azure-ad-single-sign-on)** kullanıcılarınızın bu özelliği kullanmasına olanak sağlayın.
2. Uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için **[ARTıŞLARıNı DEVRALARAK LMS çoklu oturum açmayı yapılandırın](#configure-absorb-lms-single-sign-on)** .
3. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -Britta Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
4. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanarak Britta Simon 'u etkinleştirin.
5. Kullanıcının Azure AD gösterimine bağlı olan artışlarını devralarak LMS 'de Britta Simon 'un bir karşılığı olacak şekilde **[ARTıŞLARıNı DEVRALARAK LMS test kullanıcısı oluşturun](#create-absorb-lms-test-user)** .
6. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[Çoklu oturum açmayı sınayın](#test-single-sign-on)** .

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure portal Azure AD çoklu oturum açma özelliğini etkinleştirirsiniz.

Azure AD çoklu oturum açmayı artışlarını devralarak LMS ile yapılandırmak için aşağıdaki adımları uygulayın:

1. [Azure Portal](https://portal.azure.com/), **artışlarını devralarak LMS** uygulama tümleştirmesi sayfasında, **Çoklu oturum açma**' yı seçin.

    ![Çoklu oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Çoklu oturum **açma yöntemi seç** iletişim kutusunda, çoklu oturum açmayı etkinleştirmek için **SAML/WS-Besme** modunu seçin.

    ![Çoklu oturum açma seçme modu](common/select-saml-option.png)

3. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **temel SAML yapılandırması** Iletişim kutusunu açmak için **Düzenle** simgesine tıklayın.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **temel SAML yapılandırması** Iletişim kutusunu açmak için **Düzenle** düğmesine tıklayın.

    ![Artışlarını devralarak LMS etki alanı ve URL 'Ler çoklu oturum açma bilgileri](common/idp-intiated.png)

    **Artışlarını devralarak 5 kullanıyorsanız-Kullanıcı arabirimi** aşağıdaki yapılandırmayı kullanın:

    a. **Tanımlayıcı** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://company.myabsorb.com/account/saml`

    b. **Yanıt URL 'si** metin kutusuna aşağıdaki kalıbı kullanarak bir URL yazın:`https://company.myabsorb.com/account/saml`

    **Artışlarını devralarak 5 kullanıyorsanız-yeni Learner deneyimi** aşağıdaki yapılandırmayı kullanın:

    a. **Tanımlayıcı** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://company.myabsorb.com/api/rest/v2/authentication/saml`

    b. **Yanıt URL 'si** metin kutusuna aşağıdaki kalıbı kullanarak bir URL yazın:`https://company.myabsorb.com/api/rest/v2/authentication/saml`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek tanımlayıcı ve yanıt URL 'siyle güncelleştirin. Bu değerleri almak için [ARTıŞLARıNı DEVRALARAK LMS istemci desteği ekibine](https://support.absorblms.com/hc/) başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

5. Aşağıdaki ekran görüntüsünde, **NameIdentifier** 'ın **User. UserPrincipalName**ile eşlendiği varsayılan özniteliklerin listesi gösterilmektedir.

    ![image](common/edit-attribute.png)

6. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imza sertifikası** bölümünde, **Federasyon meta veri XML** 'sini gereksiniminize göre belirtilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir** ' e tıklayın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

7. **ARTıŞLARıNı DEVRALARAK LMS 'Yi ayarla** bölümünde, uygun URL 'leri gereksiniminize göre kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD tanımlayıcısı

    c. Oturum kapatma URL 'SI

### <a name="configure-absorb-lms-single-sign-on"></a>Artışlarını devralarak LMS çoklu oturum açmayı yapılandırma

1. Yeni bir Web tarayıcısı penceresinde, artışlarını devralarak LMS şirket sitenizde yönetici olarak oturum açın.

2. Sağ üst taraftaki **Hesap** düğmesini seçin.

    ![Hesap düğmesi](./media/absorblms-tutorial/1.png)

3. Hesap bölmesinde **Portal Ayarları**' nı seçin.

    ![Portal ayarları bağlantısı](./media/absorblms-tutorial/2.png)

4. **SSO ayarlarını yönet** sekmesini seçin.

    ![Kullanıcılar sekmesi](./media/absorblms-tutorial/managesso.png)

5. **Çoklu oturum açma ayarlarını yönet** sayfasında, şunları yapın:

    ![Çoklu oturum açma yapılandırma sayfası](./media/absorblms-tutorial/settings.png)

    a. **Ad** metin kutusuna Azure AD marketi SSO 'su gibi bir ad girin.

    b. **Yöntem**olarak **SAML** ' yi seçin.

    c. Not defteri 'nde, Azure portal indirdiğiniz sertifikayı açın. **---Başlangıç sertifikası---** ve **---son sertifika---** etiketlerini kaldırın. Ardından, **anahtar** kutusunda kalan içeriği yapıştırın.

    d. **Mod** kutusunda **kimlik sağlayıcısı başlatıldı**' ı seçin.

    e. **Kimlik özellik** kutusunda, Azure AD 'de Kullanıcı tanımlayıcısı olarak yapılandırdığınız özniteliği seçin. Örneğin, Azure AD 'de *NameIdentifier* seçilirse **Kullanıcı adı**' nı seçin.

    f. **Imza türü**olarak **SHA256** öğesini seçin.

    örneğin: **Oturum açma URL** 'si kutusunda, Azure Portal uygulamanın **Özellikler** sayfasından **Kullanıcı erişim URL** 'sini yapıştırın.

    h. Oturum **kapatma URL**'si içinde, Azure Portal oturum açma **URL 'si** değerini, ' nin oturum açma **yapılandırma** penceresinden yapıştırın.

    i. Öğesini **otomatik olarak** **Açık**olarak değiştirin.

6. Kaydet ' i seçin **.**

    ![Yalnızca SSO oturumu açma geçişine Izin ver](./media/absorblms-tutorial/save.png)

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Azure portal Britta Simon adlı bir test kullanıcısı oluşturmaktır.

1. Azure portal, sol bölmedeki **Azure Active Directory**' i seçin, **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.

    !["Kullanıcılar ve gruplar" ve "tüm kullanıcılar" bağlantıları](common/users.png)

2. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.

    ![Yeni Kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı Özellikleri ' nde aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanına **Brittasıon**girin.
  
    b. **Kullanıcı adı** alan türü`brittasimon\@yourcompanydomain.extension`  
    Örneğin, BrittaSimon@contoso.com

    c. **Parolayı göster** onay kutusunu seçin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, artışlarını devralarak LMS 'ye erişim izni vererek Azure çoklu oturum açma özelliğini kullanmak için Britta Simon 'u etkinleştirin.

1. Azure portal **Kurumsal uygulamalar**' ı seçin, **tüm uygulamalar**' ı SEÇIN ve ardından **artışlarını devralarak LMS**' yi seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **ARTıŞLARıNı DEVRALARAK LMS**yazın ve seçin.

    ![Uygulamalar listesindeki artışlarını devralarak LMS bağlantısı](common/all-applications.png)

3. Soldaki menüde **Kullanıcılar ve gruplar**' ı seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. **Kullanıcı Ekle** düğmesine tıklayın, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. **Kullanıcılar ve gruplar** Iletişim kutusunda kullanıcılar listesinde **Britta Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

6. SAML onaylama işlemi içinde herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, listeden Kullanıcı için uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

7. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

### <a name="create-absorb-lms-test-user"></a>Artışlarını devralarak LMS test kullanıcısı oluşturma

Azure AD kullanıcılarının artışlarını devralarak LMS 'de oturum açması için, artışlarını devralarak LMS 'de ayarlanması gerekir. Artışlarını devralarak LMS söz konusu olduğunda, sağlama el ile gerçekleştirilen bir görevdir.

**Kullanıcı sağlamayı yapılandırmak için aşağıdaki adımları uygulayın:**

1. Artışlarını devralarak LMS şirket sitenizde yönetici olarak oturum açın.

2. **Kullanıcılar** bölmesinde, **Kullanıcılar**' ı seçin.

    ![Kullanıcılar bağlantısı](./media/absorblms-tutorial/absorblms_userssub.png)

3. **Kullanıcı** sekmesini seçin.

    ![Yeni açılan liste ekle](./media/absorblms-tutorial/absorblms_createuser.png)

4. **Kullanıcı Ekle** sayfasında, aşağıdakileri yapın:

    ![Kullanıcı Ekle sayfası](./media/absorblms-tutorial/user.png)

    a. **Ad** kutusuna ilk adı yazın (örneğin, **Britta**).

    b. **Son ad** kutusunda, **Simon**gibi soyadı yazın.

    c. **Kullanıcı adı** kutusuna, **Britta Simon**gibi bir tam ad yazın.

    d. **Parola** kutusuna kullanıcı parolası yazın.

    e. **Parolayı Onayla** kutusuna parolayı yeniden yazın.

    f. **Etkin** **' e geçiş etkin '** i ayarlayın.

5. Kaydet ' i seçin **.**

    ![Yalnızca SSO oturumu açma geçişine Izin ver](./media/absorblms-tutorial/save.png)

    > [!NOTE]
    > Varsayılan olarak, Kullanıcı sağlama SSO 'da etkinleştirilmemiştir. Müşteri bu özelliği etkinleştirmek isterse, [Bu](https://support.absorblms.com/hc/en-us/articles/360014083294-Incoming-SAML-2-0-SSO-Account-Provisioning) belgede belirtilen şekilde ayarlanmaları gerekir. Ayrıca, kullanıcının artışlarını devralarak 5 ' te kullanılabilir olduğunu, ACS URL 'SI ile **Yeni Learner deneyimini** de unutmayın.`https://company.myabsorb.com/api/rest/v2/authentication/saml`

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim panelinde artışlarını devralarak LMS kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız artışlarını devralarak LMS 'de otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory Koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
