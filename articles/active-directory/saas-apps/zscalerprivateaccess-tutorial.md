---
title: 'Öğretici: Zscaler özel erişimi ile Azure Active Directory tümleştirme (ZPA) | Microsoft Docs'
description: Azure Active Directory ve Zscaler özel erişimi (ZPA) arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 05/23/2019
ms.author: jeedes
ms.openlocfilehash: 8c7347aabb3aa6f122ea82a46ad8a09f53e271bf
ms.sourcegitcommit: 023d10b4127f50f301995d44f2b4499cbcffb8fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88545701"
---
# <a name="tutorial-integrate-zscaler-private-access-zpa-with-azure-active-directory"></a>Öğretici: Zscaler özel erişimini (ZPA) Azure Active Directory ile tümleştirin

Bu öğreticide, Zscaler özel erişimini (ZPA) Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. Zscaler özel erişimini (ZPA) Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de Zscaler özel erişimine (ZPA) erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Zscaler özel erişimi 'ne (ZPA) otomatik olarak oturum açmalarına olanak sağlayın.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

Azure AD ile SaaS uygulaması tümleştirmesi hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Zscaler özel erişimi (ZPA) çoklu oturum açma (SSO) etkin aboneliği.

> [!NOTE]
> Bu tümleştirme Ayrıca Azure AD ABD kamu bulut ortamından kullanılabilir. Bu uygulamayı Azure AD ABD kamu bulutu uygulama galerisinde bulabilir ve bunu ortak buluttan yaptığınız şekilde yapılandırabilirsiniz.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz. Zscaler özel erişimi (ZPA), **SP** tarafından başlatılan SSO 'yu destekler.

## <a name="adding-zscaler-private-access-zpa-from-the-gallery"></a>Galeriden Zscaler özel erişimi (ZPA) ekleme

Zscaler özel erişiminin (ZPA) tümleştirmesini Azure AD ile yapılandırmak için, Galeriden Zscaler özel erişimi (ZPA) ' i yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. [Azure Portal](https://portal.azure.com) iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Zscaler özel erişimi (ZPA)** yazın.
1. Sonuçlar panelinden **Zscaler özel erişimi (ZPA)** öğesini seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma ve test etme

**Britta Simon**adlı bir test kullanıcısı kullanarak Zscaler özel erişimi (ZPA) Ile Azure AD SSO 'yu yapılandırın ve test edin. SSO 'nun çalışması için, Zscaler özel erişimi (ZPA) içindeki bir Azure AD kullanıcısı ve ilgili Kullanıcı arasında bir bağlantı ilişkisi oluşturmanız gerekir.

Azure AD SSO 'yu Zscaler özel erişimi (ZPA) ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını doldurun:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** .
2. Uygulama tarafında SSO ayarlarını yapılandırmak için **[Zscaler özel erişimini (ZPA) yapılandırın](#configure-zscaler-private-access-zpa)** .
3. Britta Simon ile Azure AD çoklu oturum açma sınamasını test etmek için **[bir Azure AD test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** .
4. Azure AD 'de çoklu oturum açma özelliğini kullanarak Britta Simon 'u etkinleştirmek için **[Azure AD test kullanıcısını atayın](#assign-the-azure-ad-test-user)** .
5. **[Zscaler özel erişim (ZPA) test kullanıcısını](#create-zscaler-private-access-zpa-test-user)** , kullanıcının Azure AD gösterimine bağlı Zscaler özel erişimi (ZPA) Içinde Britta Simon 'a sahip olacak şekilde oluşturun.
6. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[test SSO 'su](#test-sso)** .

### <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. [Azure Portal](https://portal.azure.com/), **Zscaler özel erişim (ZPA)** uygulama tümleştirmesi sayfasında, **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** için Düzenle/kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. **Temel SAML yapılandırması** sayfasında, aşağıdaki alanlar için değerleri girin:

    1. **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://samlsp.private.zscaler.com/auth/login?domain=<your-domain-name>`

    1. **Tanımlayıcı (VARLıK kimliği)** metin kutusuna bir URL yazın:`https://samlsp.private.zscaler.com/auth/metadata`

    > [!NOTE]
    > **Oturum açma URL 'si** değeri gerçek değil. Değeri, gerçek oturum açma URL 'SI ile güncelleştirin. Değeri almak için [Zscaler özel erişim (ZPA) istemci desteği ekibine](https://help.zscaler.com/zpa-submit-ticket) başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

1. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde, **Federasyon meta verileri XML** 'i bulun ve sertifikayı indirip bilgisayarınıza kaydetmek için **İndir** ' i seçin.

   ![Sertifika indirme bağlantısı](common/metadataxml.png)

1. **Zscaler özel erişimi (ZPA) ayarla** bölümünde, gereksiniminize göre uygun URL 'leri kopyalayın.

   ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

### <a name="configure-zscaler-private-access-zpa"></a>Zscaler özel erişimini yapılandırma (ZPA)

1. Zscaler özel erişimi (ZPA) içindeki yapılandırmayı otomatik hale getirmek için, **uzantıyı yüklemeniz**' ne tıklayarak **uygulamalarım güvenli oturum açma tarayıcı uzantısını** yüklemeniz gerekir.

    ![Uygulamalarım uzantısı](common/install-myappssecure-extension.png)

2. Tarayıcıya Uzantı eklendikten sonra, bu **özel erişim (ZPA) kurulum** 'a tıkladığınızda sizi Zscaler özel erişim (ZPA) uygulamasına yönlendirirsiniz. Buradan, Zscaler özel erişimi 'nde (ZPA) oturum açmak için yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı, uygulamayı sizin için otomatik olarak yapılandırır ve 3-6 adımlarını otomatikleştirecektir.

    ![Kurulum yapılandırması](common/setup-sso.png)

3. Zscaler özel erişimi 'ni (ZPA) el ile ayarlamak istiyorsanız yeni bir Web tarayıcı penceresi açın ve Zscaler özel erişim (ZPA) Şirket sitenizde yönetici olarak oturum açın ve aşağıdaki adımları gerçekleştirin:

4. Menünün sol tarafında **Yönetim** ' e tıklayın ve **kimlik doğrulama** bölümüne gidin **IDP yapılandırması**' na tıklayın.

    ![Zscaler özel erişim Yöneticisi yönetimi](./media/zscalerprivateaccess-tutorial/tutorial-zscaler-private-access-administration.png)

5. Sağ üst köşede **IDP Yapılandırması Ekle**' ye tıklayın. 

    ![Zscaler özel erişim Yöneticisi IDP](./media/zscalerprivateaccess-tutorial/tutorial-zscaler-private-access-idp.png)

6. **IDP Yapılandırması Ekle** sayfasında, aşağıdaki adımları uygulayın:
 
    ![Zscaler özel erişim Yöneticisi seçme](./media/zscalerprivateaccess-tutorial/tutorial-zscaler-private-access-select.png)

    a. **IDP meta veri dosyası yükleme** alanına Indirilen meta veri dosyasını Azure AD 'den karşıya yüklemek Için **Dosya Seç** ' e tıklayın.

    b. **IDP meta verilerini** Azure AD 'den okur ve tüm alan bilgilerini aşağıda gösterildiği gibi doldurur.

    ![Zscaler özel erişim Yöneticisi yapılandırması](./media/zscalerprivateaccess-tutorial/config.png)

    c. Etki **alanları** alanından etki alanınızı seçin.
    
    d. **Kaydet**’e tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, Britta Simon adlı Azure portal bir test kullanıcısı oluşturacaksınız.

1. Azure portal sol bölmeden **Azure Active Directory**' i seçin, **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.
1. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.
1. **Kullanıcı** özellikleri ' nde şu adımları izleyin:
   1. **Ad** alanına `Britta Simon` girin.  
   1. **Kullanıcı adı** alanına, girin username@companydomain.extension . Örneğin, `BrittaSimon@contoso.com`.
   1. **Parolayı göster** onay kutusunu seçin ve ardından **parola** kutusunda görüntülenen değeri yazın.
   1. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Zscaler özel erişim (ZPA) erişimi vererek Azure çoklu oturum açma özelliğini kullanmak için Britta Simon 'u etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde **Zscaler özel erişimi (ZPA)** öğesini seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle**' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. **Kullanıcılar ve gruplar** Iletişim kutusunda kullanıcılar listesinden **Britta Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. SAML assertion 'da herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, Kullanıcı için listeden uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

### <a name="create-zscaler-private-access-zpa-test-user"></a>Zscaler özel erişim (ZPA) test kullanıcısı oluşturma

Bu bölümde, Zscaler özel erişimi (ZPA) içinde Britta Simon adlı bir Kullanıcı oluşturacaksınız. Zscaler özel erişim (ZPA) platformunda kullanıcıları eklemek için lütfen [Zscaler özel erişim (ZPA) destek ekibi](https://help.zscaler.com/zpa-submit-ticket) ile çalışın.

### <a name="test-sso"></a>Test SSO 'SU

Erişim panelinde Zscaler özel erişim (ZPA) kutucuğunu seçtiğinizde, SSO 'yu ayarladığınız Zscaler özel erişiminin (ZPA) otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory Koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)