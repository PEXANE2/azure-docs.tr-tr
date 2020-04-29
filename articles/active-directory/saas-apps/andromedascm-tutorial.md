---
title: 'Öğretici: Andromeda ile tümleştirme Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve Andromeda arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 7a142c86-ca0c-4915-b1d8-124c08c3e3d8
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/16/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 68fa570ecfbafe2000bfa6eb9fa159dff48219a6
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "67107081"
---
# <a name="tutorial-azure-active-directory-integration-with-andromeda"></a>Öğretici: Andromeda ile tümleştirme Azure Active Directory

Bu öğreticide, Andromeda 'i Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz.
Andromeda Azure AD ile tümleştirmek aşağıdaki avantajları sağlar:

* Andromeda 'e erişimi olan Azure AD 'de denetim yapabilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla Andromeda (çoklu oturum açma) ile otomatik olarak oturum açmasını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz-Azure portal.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek istiyorsanız, bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/) .

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Andromeda ile yapılandırmak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Bir Azure AD ortamınız yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/) alabilirsiniz
* Andromeda çoklu oturum açma etkin aboneliği

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açmayı bir test ortamında yapılandırıp test edersiniz.

* Andromeda **SP ve ıDP** tarafından başlatılan SSO 'yu destekler
* Andromeda **, tam zamanında** Kullanıcı sağlamayı destekler

## <a name="adding-andromeda-from-the-gallery"></a>Galeriden Andromeda ekleme

Andromeda tümleştirmesini Azure AD 'ye göre yapılandırmak için, Galeriden Andromeda yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Andromeda eklemek için aşağıdaki adımları uygulayın:**

1. **[Azure Portal](https://portal.azure.com)** sol gezinti panelinde **Azure Active Directory** simgesine tıklayın.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar** seçeneğini belirleyin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için, iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesine tıklayın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Andromeda**yazın, sonuç panelinden **Andromeda** ' yi seçin ve ardından **Ekle** düğmesine tıklayarak uygulamayı ekleyin.

    ![Sonuç listesinde Andromeda](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma ve test etme

Bu bölümde, Azure AD çoklu oturum açmayı, **Britta Simon**adlı bir test kullanıcısına göre Andromeda ile yapılandırıp test edersiniz.
Çoklu oturum açma için, bir Azure AD kullanıcısı ve Andromeda 'deki ilgili Kullanıcı arasındaki bağlantı ilişkisinin kurulması gerekir.

Azure AD çoklu oturum açmayı Andromeda ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını gerçekleştirmeniz gerekir:

1. **[Azure AD çoklu oturum açma özelliğini yapılandırarak](#configure-azure-ad-single-sign-on)** kullanıcılarınızın bu özelliği kullanmasına olanak sağlayın.
2. Uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için **[Andromeda çoklu oturum açmayı yapılandırın](#configure-andromeda-single-sign-on)** .
3. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -Britta Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
4. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanarak Britta Simon 'u etkinleştirin.
5. Kullanıcının Azure AD gösterimine bağlı olan Andromeda 'de Britta Simon 'ın bir karşılığı olacak şekilde **[Andromeda test kullanıcısı oluşturun](#create-andromeda-test-user)** .
6. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[Çoklu oturum açmayı sınayın](#test-single-sign-on)** .

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure portal Azure AD çoklu oturum açma özelliğini etkinleştirirsiniz.

Azure AD çoklu oturum açmayı Andromeda ile yapılandırmak için aşağıdaki adımları uygulayın:

1. [Azure Portal](https://portal.azure.com/), **Andromeda** uygulama tümleştirmesi sayfasında, **Çoklu oturum açma**' yı seçin.

    ![Çoklu oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Çoklu oturum **açma yöntemi seç** iletişim kutusunda, çoklu oturum açmayı etkinleştirmek için **SAML/WS-Besme** modunu seçin.

    ![Çoklu oturum açma seçme modu](common/select-saml-option.png)

3. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **temel SAML yapılandırması** Iletişim kutusunu açmak için **Düzenle** simgesine tıklayın.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. **Temel SAML yapılandırması** bölümünde, uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız aşağıdaki adımları uygulayın:

    ![Andromeda etki alanı ve URL 'Ler çoklu oturum açma bilgileri](common/idp-intiated.png)

    a. **Tanımlayıcı** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<tenantURL>.ngcxpress.com/`

    b. **Yanıt URL 'si** metin kutusuna aşağıdaki kalıbı kullanarak bir URL yazın:`https://<tenantURL>.ngcxpress.com/SAMLConsumer.aspx`

5. Uygulamayı **SP** tarafından başlatılan modda yapılandırmak Istiyorsanız **ek URL 'ler ayarla** ' ya tıklayın ve aşağıdaki adımı gerçekleştirin:

    ![Andromeda etki alanı ve URL 'Ler çoklu oturum açma bilgileri](common/metadata-upload-additional-signon.png)

    **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<tenantURL>.ngcxpress.com/SAMLLogon.aspx`

    > [!NOTE]
    > Bu değerler gerçek değildir. Değeri gerçek tanımlayıcı, yanıt URL 'SI ve Öğreticinin ilerleyen kısımlarında açıklanan oturum açma URL 'SI ile güncelleşceksiniz.

6. Andromeda uygulaması, SAML onaylamalarını belirli bir biçimde bekliyor. Bu uygulama için aşağıdaki talepleri yapılandırın. Bu özniteliklerin değerlerini, uygulama tümleştirme sayfasındaki **Kullanıcı öznitelikleri** bölümünden yönetebilirsiniz. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **Kullanıcı öznitelikleri** Iletişim kutusunu açmak için **Düzenle** düğmesine tıklayın.

    ![image](common/edit-attribute.png)

    > [!Important]
    > Bu ayarları ayarlarken ad alanı tanımlarını temizleyin.

7. **Kullanıcı öznitelikleri** Iletişim kutusundaki **Kullanıcı talepleri** bölümünde, yukarıdaki görüntüde gösterildiği gibi, **Düzen simgesini** kullanarak talepleri DÜZENLEYIN veya aşağıdaki resimde gösterildiği gibi SAML belirteci özniteliğini yapılandırmak için **yeni talep Ekle** ' yi kullanarak talepleri ekleyin ve aşağıdaki adımları gerçekleştirin: 

    | Adı | Kaynak özniteliği|
    | ------ | -----------|
    | rol        | Uygulamaya özel rol |
    | type        | Uygulama Türü |
    | şirket       | CompanyName |

    > [!NOTE]
    > Gerçek değer yok. Bu değerler yalnızca tanıtım amaçlı amaçlıdır, lütfen kuruluş rollerinizi kullanın.

    a. **Kullanıcı taleplerini Yönet** iletişim kutusunu açmak için **yeni talep Ekle** ' ye tıklayın.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. **Ad** metin kutusuna, bu satır için gösterilen öznitelik adını yazın.

    c. **Ad alanını** boş bırakın.

    d. **Öznitelik**olarak kaynak seçin.

    e. **Kaynak özniteliği** listesinde, bu satır için gösterilen öznitelik değerini yazın.

    f. **Tamam 'a** tıklayın

    g. **Kaydet**’e tıklayın.

8. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde, **sertifika (base64)** ' i gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir** ' e tıklayın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

9. **Andromeda ayarla** bölümünde, uygun URL 'leri gereksiniminize göre kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD tanımlayıcısı

    c. Oturum kapatma URL 'SI

### <a name="configure-andromeda-single-sign-on"></a>Andromeda çoklu oturum açmayı yapılandırma

1. Andromeda şirket sitenizde yönetici olarak oturum açın.

2. Menü çubuğu üst kısmında **yönetici** ' ye tıklayıp **Yönetim**' e gidin.

    ![Andromeda Yöneticisi](./media/andromedascm-tutorial/tutorial_andromedascm_admin.png)

3. Araç çubuğunun sol tarafında, **arabirimler** bölümünde **SAML yapılandırması**' na tıklayın.

    ![Andromeda SAML](./media/andromedascm-tutorial/tutorial_andromedascm_saml.png)

4. **SAML yapılandırması** bölüm sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Andromeda yapılandırması](./media/andromedascm-tutorial/tutorial_andromedascm_config.png)

    a. **SAML Ile SSO 'Yu etkinleştir**' i işaretleyin.

    b. **Andromeda bilgileri** bölümünde, **SP kimlik** DEĞERINI kopyalayın ve **temel SAML yapılandırması** bölümünün **tanımlayıcı** metin kutusuna yapıştırın.

    c. **Tüketici URL 'si** değerini kopyalayın ve **temel SAML YAPıLANDıRMASı** bölümünün **yanıt URL** metin kutusuna yapıştırın.

    d. **Oturum açma URL 'si** değerini kopyalayın ve **temel SAML yapılandırması** bölümünün **oturum açma URL 'si** metin kutusuna yapıştırın.

    e. **SAML kimlik sağlayıcısı** bölümünde IDP adınızı yazın.

    f. **Çoklu oturum açma uç noktası** metin kutusunda, Azure Portal kopyaladığınız **oturum açma URL 'si** değerini yapıştırın.

    g. İndirilen **Base64 kodlamalı sertifikayı** Not defteri 'ndeki Azure Portal açın, **X 509 Certificate** metin kutusuna yapıştırın.
    
    h. Azure AD 'den SSO oturumu açmayı kolaylaştırmak için aşağıdaki öznitelikleri ilgili değerle eşleyin. Oturum açmak için **Kullanıcı kimliği** özniteliği gereklidir. Sağlama, **e-posta**, **Şirket**, **Kullanıcı türü**ve **rol** için gereklidir. Bu bölümde, Azure portal içinde tanımlananlarla bağıntılı öznitelik eşlemesini (ad ve değerler) tanımlayacağız

    ![Andromeda attbmap](./media/andromedascm-tutorial/tutorial_andromedascm_attbmap.png)

    i. **Kaydet**’e tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Azure portal Britta Simon adlı bir test kullanıcısı oluşturmaktır.

1. Azure portal, sol bölmedeki **Azure Active Directory**' i seçin, **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.

    !["Kullanıcılar ve gruplar" ve "tüm kullanıcılar" bağlantıları](common/users.png)

2. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.

    ![Yeni Kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı Özellikleri ' nde aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanına **Brittasıon**girin.
  
    b. **Kullanıcı adı** alanına yazın `brittasimon@yourcompanydomain.extension`. Örneğin, BrittaSimon@contoso.com

    c. **Parolayı göster** onay kutusunu seçin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**' a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Andromeda 'e erişim vererek Azure çoklu oturum açma özelliğini kullanmak için Britta Simon özelliğini etkinleştirin.

1. Azure portal **Kurumsal uygulamalar**' ı seçin, **tüm uygulamalar**' ı seçin ve ardından **Andromeda**' yi seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Andromeda**' yi seçin.

    ![Uygulamalar listesindeki Andromeda bağlantısı](common/all-applications.png)

3. Soldaki menüde **Kullanıcılar ve gruplar**' ı seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. **Kullanıcı Ekle** düğmesine tıklayın, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. **Kullanıcılar ve gruplar** Iletişim kutusunda kullanıcılar listesinde **Britta Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

6. SAML onaylama işlemi içinde herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, listeden Kullanıcı için uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

7. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

### <a name="create-andromeda-test-user"></a>Andromeda test kullanıcısı oluştur

Bu bölümde, Andromeda içinde Britta Simon adlı bir Kullanıcı oluşturulur. Andromeda, varsayılan olarak etkinleştirilen tam zamanında Kullanıcı sağlamayı destekler. Bu bölümde sizin için herhangi bir eylem öğesi yok. Bir Kullanıcı Andromeda içinde zaten mevcut değilse, kimlik doğrulamasından sonra yeni bir tane oluşturulur. Bir kullanıcıyı el ile oluşturmanız gerekiyorsa, [Andromeda istemci desteği ekibine](https://www.ngcsoftware.com/support/)başvurun.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim panelinde Andromeda kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız Andromeda için otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory Koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)