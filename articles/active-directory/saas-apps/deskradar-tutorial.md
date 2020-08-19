---
title: 'Öğretici: Deskradar ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve Deskradar arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 10/24/2019
ms.author: jeedes
ms.openlocfilehash: a9004577fa8d946281df88284139e2e507e485b7
ms.sourcegitcommit: 023d10b4127f50f301995d44f2b4499cbcffb8fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88536491"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-deskradar"></a>Öğretici: Deskradar ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory

Bu öğreticide, Azure Active Directory (Azure AD) ile Deskradar tümleştirme hakkında bilgi edineceksiniz. Deskradar 'i Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de Deskradar 'e erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Deskradar 'e otomatik olarak oturum açmalarına olanak sağlayın.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

Azure AD ile SaaS uygulaması tümleştirmesi hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Deskradar çoklu oturum açma (SSO) etkin aboneliği.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz.



* Deskradar **SP ve ıDP** tarafından başlatılan SSO 'yu destekler



## <a name="adding-deskradar-from-the-gallery"></a>Galeriden Deskradar ekleme

Deskradar 'in Azure AD 'ye tümleştirilmesini yapılandırmak için Galeriden, yönetilen SaaS uygulamaları listenize Deskradar eklemeniz gerekir.

1. [Azure Portal](https://portal.azure.com) iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **deskradar** yazın.
1. Sonuçlar panelinden **Deskradar** ' i seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.


## <a name="configure-and-test-azure-ad-single-sign-on-for-deskradar"></a>Deskradar için Azure AD çoklu oturum açmayı yapılandırma ve test etme

**B. Simon**adlı bir test kullanıcısı kullanarak Azure AD SSO 'Yu deskradar ile yapılandırın ve test edin. SSO 'nun çalışması için, bir Azure AD kullanıcısı ve Deskradar içindeki ilgili Kullanıcı arasında bağlantı ilişkisi oluşturmanız gerekir.

Azure AD SSO 'yu Deskradar ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını doldurun:

1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
    1. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -B. Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
    1. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek için.
1. Uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için **[Deskradar SSO 'Yu yapılandırın](#configure-deskradar-sso)** .
    1. Kullanıcının Azure AD gösterimine bağlı olan Deskradar 'de B. Simon 'a karşılık gelen, **[deskradar test kullanıcısı oluşturun](#create-deskradar-test-user)** .
1. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. [Azure Portal](https://portal.azure.com/), **deskradar** uygulama tümleştirmesi sayfasında, **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** için Düzenle/kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. **Temel SAML yapılandırması** bölümünde, **IDP** tarafından başlatılan modda uygulamayı yapılandırmak istiyorsanız aşağıdaki alanlar için değerleri girin:

    a. **Tanımlayıcı** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<YOURDOMAIN>.deskradar.cloud`

    b. **Yanıt URL 'si** metin kutusuna aşağıdaki kalıbı kullanarak bir URL yazın:`https://<YOURDOMAIN>.deskradar.cloud/auth/sso/saml/consume`

1. Uygulamayı **SP** tarafından başlatılan modda yapılandırmak Istiyorsanız **ek URL 'ler ayarla** ' ya tıklayın ve aşağıdaki adımı gerçekleştirin:

    **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<YOURDOMAIN>.deskradar.cloud/auth/sso/saml/login`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek tanımlayıcı, yanıt URL 'SI ve oturum açma URL 'SI ile güncelleştirin. **Yourdomain** öğesini deskradar örnek etki alanınız ile değiştirin. Bu değerleri almak için [Deskradar istemci desteği ekibine](mailto:support@deskradar.com) başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

1. Deskradar uygulaması, SAML belirteci öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML onayları bekler. Aşağıdaki ekran görüntüsünde varsayılan özniteliklerin listesi gösterilmektedir.

    ![image](common/edit-attribute.png)

1. Deskradar uygulaması, yukarıdakine ek olarak aşağıda gösterilen SAML yanıtına daha fazla öznitelik geçirilmesini bekler. Bu öznitelikler de önceden doldurulur, ancak gereksiniminize göre bunları gözden geçirebilirsiniz.

    | Adı | Kaynak özniteliği|
    | ---------------| --------- |
    | FirstName | Kullanıcı. |
    | LastName | User. soyadı |
    | E-posta | User. UserPrincipalName |
    | | |

1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, **SAML Imzalama sertifikası** bölümünde **sertifika bulun (base64)** ve sertifikayı indirip bilgisayarınıza kaydetmek için **İndir** ' i seçin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. **Deskradar ayarla** bölümünde, gereksiniminize göre uygun URL 'leri kopyalayın.

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

Bu bölümde, Deskradar 'e erişim vererek Azure çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde, **Deskradar**' i seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle**' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. SAML assertion 'da herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, Kullanıcı için listeden uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

### <a name="configure-deskradar-sso"></a>Deskradar SSO 'yu yapılandırma

1. Deskradar içindeki yapılandırmayı otomatik hale getirmek için, **uzantıyı yüklemek**üzere **uygulamalarımı güvenli oturum açma tarayıcı uzantısı** ' nı yüklemeniz gerekir.

    ![Uygulamalarım uzantısı](common/install-myappssecure-extension.png)

1. Tarayıcıya uzantı ekledikten sonra, **Kurulum deskradar** ' e tıklayarak sizi deskradar uygulamasına yönlendirebilirsiniz. Buradan, Deskradar 'de oturum açmak için yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı, uygulamayı sizin için otomatik olarak yapılandırır ve 3-7 adımlarını otomatikleştirecektir.

    ![Kurulum yapılandırması](common/setup-sso.png)

1. Deskradar 'i el ile ayarlamak istiyorsanız yeni bir Web tarayıcı penceresi açın ve Deskradar şirket sitenizde yönetici olarak oturum açın ve aşağıdaki adımları gerçekleştirin:

1. Kenar çubuğundaki simgeye tıklayarak **ekip** masasını açın.

1. **Kimlik doğrulama** sekmesine geçin.

1. **SAML 2,0** sekmesinde aşağıdaki adımları gerçekleştirin:

    ![Deskradar yapılandırması](./media/deskradar-tutorial/14-paste-urls.jpg)

    a. **SAML** kimlik doğrulama yöntemini etkinleştirin.

    b. **SAML SSO URL** metin kutusuna, Azure Portal kopyaladığınız **oturum açma URL 'si** değerini girin.

    c. **Kimlik sağlayıcısı veren** metin kutusuna, Azure Portal KOPYALADıĞıNıZ **Azure AD tanımlayıcı** değerini girin.

1. İndirilen **sertifika (base64)** dosyasını bir metin düzenleyicisiyle açın ve Içeriği kopyalayıp deskradar Içindeki **ortak sertifika** alanına yapıştırın.

    ![Deskradar yapılandırması](./media/deskradar-tutorial/15-paste-certificate.jpg)

### <a name="create-deskradar-test-user"></a>Deskradar test kullanıcısı oluştur

Bu bölümde, Deskradar 'de B. Simon adlı bir Kullanıcı oluşturacaksınız. Deskradar platformunda kullanıcıları eklemek için [Deskradar istemci desteği ekibi](mailto:support@deskradar.com) ile çalışın. Çoklu oturum açma kullanılmadan önce kullanıcıların oluşturulması ve etkinleştirilmesi gerekir.

## <a name="test-sso"></a>Test SSO 'SU 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim panelinde Deskradar kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız Deskradar için otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

- [ SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Deskradar 'i deneyin](https://aad.portal.azure.com/)

