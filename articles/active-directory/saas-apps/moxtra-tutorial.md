---
title: 'Öğretici: Moxtra ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory Microsoft Docs'
description: Azure Active Directory ve Moxtra arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 12/05/2019
ms.author: jeedes
ms.openlocfilehash: 7fffe58dfdb63da28edc19c19b56b576f4fbadd3
ms.sourcegitcommit: 023d10b4127f50f301995d44f2b4499cbcffb8fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88544052"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-moxtra"></a>Öğretici: Moxtra ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory

Bu öğreticide, Moxtra 'ın Azure Active Directory (Azure AD) ile nasıl tümleştirileceğini öğreneceksiniz. Moxtra 'ı Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de Moxtra 'a erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla birlikte otomatik olarak oturum açmalarına olanak tanıyın.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

Azure AD ile SaaS uygulaması tümleştirmesi hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Moxtra çoklu oturum açma (SSO) aboneliği etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz.

* Moxtra, **SP** tarafından başlatılan SSO 'yu destekler

> [!NOTE]
> Bu uygulamanın tanımlayıcısı, tek bir kiracıda yalnızca bir örneğin yapılandırılabilmesini sağlamak için sabit bir dize değeridir.

## <a name="adding-moxtra-from-the-gallery"></a>Galeriden Moxtra ekleme

Moxtra 'ın Azure AD ile tümleştirilmesini yapılandırmak için Galeri 'den yönetilen SaaS uygulamaları listenize Moxtra eklemeniz gerekir.

1. [Azure Portal](https://portal.azure.com) iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Moxtra** yazın.
1. Sonuçlar panelinden **Moxtra** ' ı seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.


## <a name="configure-and-test-azure-ad-single-sign-on-for-moxtra"></a>Moxtra için Azure AD çoklu oturum açmayı yapılandırma ve test etme

**B. Simon**adlı bir test kullanıcısı kullanarak Moxtra Ile Azure AD SSO 'yu yapılandırın ve test edin. SSO 'nun çalışması için, Moxtra içindeki bir Azure AD kullanıcısı ve ilgili Kullanıcı arasında bir bağlantı ilişkisi oluşturmanız gerekir.

Azure AD SSO 'yu Moxtra ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını doldurun:

1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
    1. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -B. Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
    1. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek için.
1. **[Moxtra SSO 'Yu yapılandırma](#configure-moxtra-sso)** -uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
    1. **[Moxtra test kullanıcısı oluşturun](#create-moxtra-test-user)** ve bu, kullanıcının Azure AD gösterimine bağlı olan Moxtra 'de B. Simon 'a karşılık gelen bir karşılığı vardır.
1. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. [Azure Portal](https://portal.azure.com/), **Moxtra** uygulama tümleştirmesi sayfasında, **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** için Düzenle/kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. **Temel SAML yapılandırması** bölümünde, aşağıdaki alanlar için değerleri girin:

    **Oturum açma URL 'si** metin kutusuna bir URL yazın:`https://www.moxtra.com/service/#login`

1. Moxtra uygulaması, SAML belirteci öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML onayları bekliyor. Aşağıdaki ekran görüntüsünde varsayılan özniteliklerin listesi gösterilmektedir. Kullanıcı öznitelikleri iletişim kutusunu açmak için **Düzenle** simgesine tıklayın.

    ![image](common/edit-attribute.png)

1. Moxtra uygulaması, yukarıdakine ek olarak, SAML yanıtına daha fazla özniteliğin geri geçirilmesini bekler. Kullanıcı öznitelikleri iletişim kutusundaki Kullanıcı talepleri bölümünde aşağıdaki tabloda gösterildiği gibi SAML belirteci özniteliği eklemek için aşağıdaki adımları gerçekleştirin:

    | Adı | Kaynak özniteliği|
    | ------------------- | -------------------- |    
    | FirstName | Kullanıcı. |
    | Soyadı | User. soyadı |
    | ıdpid    | Azure AD tanımlayıcısını < >

    > [!Note]
    > **Idpid** özniteliği değeri gerçek değil. 5. adımdaki **Moxtra** bölümünden gerçek değeri alabilirsiniz. 

    1. **Kullanıcı taleplerini Yönet** iletişim kutusunu açmak için **yeni talep Ekle** ' ye tıklayın.

    1. **Ad** metin kutusuna, bu satır için gösterilen öznitelik adını yazın.

    1. **Ad alanını** boş bırakın.

    1. **Öznitelik**olarak kaynak seçin.

    1. **Kaynak özniteliği** listesinde, bu satır için gösterilen öznitelik değerini yazın.

    1. **Tamam 'a** tıklayın

    1. **Kaydet**’e tıklayın.

1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, **SAML Imzalama sertifikası** bölümünde **sertifika bulun (base64)** ve sertifikayı indirip bilgisayarınıza kaydetmek için **İndir** ' i seçin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. **Moxtra ayarla** bölümünde, gereksiniminize göre uygun URL 'leri kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

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

Bu bölümde, Moxtra 'a erişim vererek Azure çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde **Moxtra**' ı seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle**' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. SAML assertion 'da herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, Kullanıcı için listeden uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

## <a name="configure-moxtra-sso"></a>Moxtra SSO 'yu yapılandırma

1. Başka bir tarayıcı penceresinde, Moxtra şirket sitenizde yönetici olarak oturum açın.

2. Soldaki araç çubuğunda, **yönetici konsolu > SAML çoklu oturum açma**' ya tıklayın ve ardından **Yeni**' ye tıklayın.
   
    ![Çoklu oturum açmayı yapılandırma](./media/moxtra-tutorial/tutorial_moxtra_06.png) 

3. **SAML** sayfasında, aşağıdaki adımları uygulayın:
   
    ![Çoklu oturum açmayı yapılandırma](./media/moxtra-tutorial/tutorial_moxtra_08.png)   
 
    a. **Ad** metin kutusuna yapılandırmanız için bir ad yazın (örneğin: *SAML*). 
  
    b. **IDP VARLıK kimliği** metin kutusunda, Azure Portal KOPYALADıĞıNıZ **Azure AD tanımlayıcısının** değerini yapıştırın. 
 
    c. **Oturum açma URL 'si** metin kutusunda, Azure Portal kopyaladığınız **oturum açma URL 'si** değerini yapıştırın. 
 
    d. **Authncontextclassref** metin kutusunda **urn: oasu: adlar: TC: SAML: 2.0: AC: sınıflar: Password**yazın. 
 
    e. **NameID biçimi** metin kutusuna **urn: oassıs: names: TC: SAML: 1.1: NameID-Format: emapostaadı**yazın. 
 
    f. Not defteri 'nde Azure portal indirdiğiniz sertifikayı açın, içeriği kopyalayın ve sonra **sertifika** metin kutusuna yapıştırın.    
 
    örneğin: SAML e-posta etki alanı metin kutusuna SAML e-posta etki alanınızı yazın.    
  
    >[!NOTE]
    >Etki alanını doğrulama adımlarını görmek için**aşağıdaki "i" düğmesine**tıklayın.

    h. **Güncelleştir**’e tıklayın.

### <a name="create-moxtra-test-user"></a>Moxtra test kullanıcısı oluşturma

Bu bölümün amacı, Moxtra içinde B. Simon adlı bir Kullanıcı oluşturmaktır.

**Moxtra 'de B. Simon adlı bir kullanıcı oluşturmak için aşağıdaki adımları uygulayın:**

1. Moxtra şirket sitenizde yönetici olarak oturum açın.

1. Soldaki araç çubuğunda **yönetici konsolu > Kullanıcı yönetimi**' ne ve ardından **Kullanıcı Ekle**' ye tıklayın.
   
    ![Çoklu oturum açmayı yapılandırma](./media/moxtra-tutorial/tutorial_moxtra_10.png) 

1. **Kullanıcı Ekle** iletişim kutusunda aşağıdaki adımları gerçekleştirin:
  
    a. **Ad** metin kutusuna **B**yazın.
  
    b. **Soyadı** metin kutusuna **Simon**yazın.
  
    c. **E-posta** metin kutusuna, Azure Portal ile aynı e-posta adresini yazın.
  
    d. **Bölüm** metin kutusuna **dev**yazın.
  
    e. **Bölüm** metin **kutusuna yazın.**
  
    f. **Yönetici**' yi seçin.
  
    örneğin: **Ekle**'ye tıklayın.

## <a name="test-sso"></a>Test SSO 'SU 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim panelinde Moxtra kutucuğunu tıklattığınızda, SSO 'yu ayarladığınız Moxtra ' de otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

- [ SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Moxtra 'ı deneyin](https://aad.portal.azure.com/)

