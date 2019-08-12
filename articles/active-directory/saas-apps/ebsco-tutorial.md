---
title: 'Öğretici: EBSCO ile tümleştirme Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve EBSCO arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 144f7f65-69e9-4016-a151-fe1104fd6ba8
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/01/2019
ms.author: jeedes
ms.openlocfilehash: 35cb408473da8c6397c5034ae20ac0a50b0953ea
ms.sourcegitcommit: 124c3112b94c951535e0be20a751150b79289594
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2019
ms.locfileid: "68944713"
---
# <a name="tutorial-azure-active-directory-integration-with-ebsco"></a>Öğretici: EBSCO ile tümleştirme Azure Active Directory

Bu öğreticide, EBSCO 'yı Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz.
EBSCO 'yi Azure AD ile tümleştirmek aşağıdaki avantajları sağlar:

* Azure AD 'de, EBSCO 'ya erişimi olan bir denetim yapabilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla otomatik olarak EBSCO (çoklu oturum açma) ile oturum açmasını sağlayabilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek istiyorsanız, bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesini EBSCO ile yapılandırmak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Bir Azure AD ortamınız yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/) alabilirsiniz
* EBSCO çoklu oturum açma etkin aboneliği

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açmayı bir test ortamında yapılandırıp test edersiniz.

* EBSCO, **SP** ve **IDP** tarafından başlatılan SSO 'yu destekler

* EBSCO **, tam zamanında** Kullanıcı sağlamayı destekliyor

## <a name="adding-ebsco-from-the-gallery"></a>Galeriden EBSCO ekleme

EBSCO 'nın tümleştirmesini Azure AD 'ye göre yapılandırmak için, galerinizden yönetilen SaaS uygulamaları listenize EBSCO eklemeniz gerekir.

**Galeriden EBSCO eklemek için aşağıdaki adımları uygulayın:**

1. **[Azure Portal](https://portal.azure.com)** sol gezinti panelinde **Azure Active Directory** simgesine tıklayın.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar** seçeneğini belirleyin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için, iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesine tıklayın.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **EBSCO**yazın, sonuç panelinden **EBSCO** ' ı seçin, sonra da uygulamayı eklemek için **Ekle** düğmesine tıklayın.

     ![Sonuç listesinde EBSCO](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, Azure AD çoklu oturum açmayı, **Britta Simon**adlı bir test kullanıcısına bağlı olarak EBSCO ile yapılandırıp test edersiniz.
Çoklu oturum açma 'nın çalışması için, bir Azure AD kullanıcısı ile ilgili Kullanıcı arasındaki bir bağlantı ilişkisinin kurulması gerekir.

Azure AD çoklu oturum açma 'yı EBSCO ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını gerçekleştirmeniz gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[EBSCO çoklu oturum açmayı yapılandırma](#configure-ebsco-single-sign-on)** -uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[EBSCO test kullanıcısı oluşturun](#create-ebsco-test-user)** ; bu, kullanıcının Azure AD gösterimine bağlı olan EBSCO 'Da Britta Simon 'a sahip olmak için.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure portal Azure AD çoklu oturum açma özelliğini etkinleştirirsiniz.

Azure AD çoklu oturum açmayı EBSCO ile yapılandırmak için aşağıdaki adımları uygulayın:

1. [Azure Portal](https://portal.azure.com/), **EBSCO** uygulama tümleştirmesi sayfasında, **Çoklu oturum açma**' yı seçin.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Çoklu oturum **açma yöntemi seç** iletişim kutusunda, çoklu oturum açmayı etkinleştirmek için **SAML/WS-Besme** modunu seçin.

    ![Çoklu oturum açma seçme modu](common/select-saml-option.png)

3. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **temel SAML yapılandırması** Iletişim kutusunu açmak için **Düzenle** simgesine tıklayın.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. **Temel SAML yapılandırması** bölümünde, uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız aşağıdaki adımı uygulayın:

    ![EBSCO etki alanı ve URL 'Ler çoklu oturum açma bilgileri](common/idp-identifier.png)

    **Tanımlayıcı** metin kutusuna bir URL yazın:`pingsso.ebscohost.com`

5. Uygulamayı **SP** tarafından başlatılan modda yapılandırmak Istiyorsanız **ek URL 'ler ayarla** ' ya tıklayın ve aşağıdaki adımı gerçekleştirin:

    ![image](common/both-preintegrated-signon.png)

    **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://search.ebscohost.com/login.aspx?authtype=sso&custid=<unique EBSCO customer ID>&profile=<profile ID>`

    > [!NOTE]
    > Oturum açma URL 'SI değeri gerçek değil. Değeri, gerçek oturum açma URL 'SI ile güncelleştirin. Bu değerleri almak için [EBSCO istemci destek ekibine](mailto:sso@ebsco.com) başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

    o **benzersiz öğeler:**  

    o **CustId** = benzersiz EBSCO müşteri kimliğini girin 

    o **profile** = istemciler, kullanıcıları belirli bir profile (EBSCO 'dan satın aldıklarına bağlı olarak) yönlendirebilir. Belirli bir profil KIMLIĞI girebilirler. Ana kimlikler EDS (EBSCO bulma hizmeti) ve ehost (EBSOCOhost veritabanları) ' dir. Aynı yönergeler [burada](https://help.ebsco.com/interfaces/EBSCOhost/EBSCOhost_FAQs/How_do_I_set_up_direct_links_to_EBSCOhost_profiles_and_or_databases#profile)verilmiştir.

6. EBSCO uygulaması, SAML belirteci öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML onayları bekliyor. Aşağıdaki ekran görüntüsünde varsayılan özniteliklerin listesi gösterilmektedir. **Kullanıcı öznitelikleri** iletişim kutusunu açmak için **Düzenle** simgesine tıklayın.

    ![image](common/edit-attribute.png)

     > [!Note]
    > **Name** özniteliği zorunludur ve EBSCO uygulamasındaki **ad tanımlayıcı değeriyle** eşlenir. Bu, varsayılan olarak eklenerek el ile eklemeniz gerekmez.

7. Daha fazlasına ek olarak, EBSCO uygulaması SAML yanıtına daha fazla özniteliğin geri geçirilmesini bekler. **Kullanıcı öznitelikleri** Iletişim kutusundaki **Kullanıcı talepleri** bölümünde AŞAĞıDAKI tabloda gösterildiği gibi SAML belirteci özniteliği eklemek için aşağıdaki adımları gerçekleştirin: 

    | Ad | Kaynak özniteliği|
    | ---------------| --------------- |    
    | FirstName   | Kullanıcı. |
    | LastName   | User. soyadı |
    | Email   | Kullanıcı. Mail |

    a. **Kullanıcı taleplerini Yönet** iletişim kutusunu açmak için **yeni talep Ekle** ' ye tıklayın.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. **Ad** metin kutusuna, bu satır için gösterilen öznitelik adını yazın.

    c. **Ad alanını** boş bırakın.

    d. **Öznitelik**olarak kaynak seçin.

    e. **Kaynak özniteliği** listesinde, bu satır için gösterilen öznitelik değerini yazın.

    f. **Kaydet**’e tıklayın.

8. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imza sertifikası** bölümünde, **Federasyon meta veri XML** 'sini gereksiniminize göre belirtilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir** ' e tıklayın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

9. **EBSCO ayarla** bölümünde uygun URL 'leri gereksiniminize göre kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-ebsco-single-sign-on"></a>EBSCO çoklu oturum açmayı yapılandırma

**EBSCO** tarafında çoklu oturum açmayı yapılandırmak için, Indirilen **meta veri XML** 'Sini ve uygun kopyalanmış URL 'Leri Azure Portal ' den [EBSCO destek ekibine](mailto:sso@ebsco.com)göndermeniz gerekir. Bunlar, her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Yeni Kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanına **Brittasıon**girin.
  
    b. **Kullanıcı adı** alanına yazın brittasimon@yourcompanydomain.extension. Örneğin, BrittaSimon@contoso.com

    c. **Parolayı göster** onay kutusunu seçin ve ardından parola kutusunda görüntülenen değeri yazın.

    d.           **Oluştur**'a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, EBSCO 'ya erişim vererek Azure çoklu oturum açma özelliğini kullanmak için Britta Simon 'u etkinleştirin.

1. Azure portal **Kurumsal uygulamalar**' ı seçin, **tüm uygulamalar**' ı seçin ve ardından **EBSCO**' yi seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde, **EBSCO**' ı seçin.

    ![Uygulamalar listesindeki EBSCO bağlantısı](common/all-applications.png)

3. Soldaki menüde **Kullanıcılar ve gruplar**' ı seçin.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. **Kullanıcı Ekle** düğmesine tıklayın, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylama işlemi içinde herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, listeden Kullanıcı için uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

7. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

### <a name="create-ebsco-test-user"></a>EBSCO test kullanıcısı oluşturma

EBSCO durumunda, Kullanıcı sağlama otomatiktir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

Azure AD, gerekli verileri EBSCO uygulamasına geçirir. EBSCO 'nın Kullanıcı sağlaması otomatik olabilir veya tek seferlik bir form gerektirebilir. Bu, istemcinin kişisel ayarları kayıtlı olan çok sayıda daha önceden var olan EBSCOhost hesabına sahip olup olmamasına bağlıdır. Aynı şekilde, uygulama sırasında [EBSCO destek ekibi](mailto:sso@ebsco.com) ile de tartışılmış olabilir. Her iki durumda da, istemci test etmeden önce herhangi bir EBSCOhost hesabı oluşturmak zorunda değildir.

   >[!Note]
   >EBSCOhost Kullanıcı sağlama/kişiselleştirmesini otomatik hale getirebilirsiniz. Tam zamanında Kullanıcı hazırlama hakkında [EBSCO destek ekibine](mailto:sso@ebsco.com) başvurun. 

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

1. Erişim panelinde EBSCO kutucuğuna tıkladığınızda, EBSCO uygulamanızda otomatik olarak oturum açmış olmanız gerekir.
Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](../user-help/active-directory-saas-access-panel-introduction.md).

2. Uygulamada oturum açtıktan sonra sağ üst köşedeki **oturum aç** düğmesine tıklayın.

    ![Uygulamalar listesinde EBSCO oturum açma](./media/ebsco-tutorial/tutorial_ebsco_signin.png)
 
3. **Mevcut MyEBSCOhost hesabınızı şimdi kurum hesabınıza bağlamak** veya **Yeni bir Myebscohost hesabı oluşturmak ve bunu kurum hesabınıza bağlamak**için tek seferlik bir istem alacaksınız. Hesap, EBSCOhost uygulamasında kişiselleştirme için kullanılır. **Yeni hesap oluştur** seçeneğini belirleyin ve aşağıdaki ekran görüntüsünde gösterildiği gibi, kişiselleştirme formunun SAML yanıtından alınan değerlerle önceden tamamlandığını görürsünüz. Bu seçimi kaydetmek için **' devam '** düğmesine tıklayın.
    
     ![Uygulamalar listesindeki EBSCO kullanıcısı](./media/ebsco-tutorial/tutorial_ebsco_user.png)

1. Yukarıdaki kurulumu tamamladıktan sonra, tanımlama bilgileri/önbellek seçimini kaldırın ve yeniden oturum açın. El ile yeniden oturum açmanız gerekmez ve kişiselleştirme ayarları hatırlanır

## <a name="additional-sesources"></a>Ek sesources

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory Koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

