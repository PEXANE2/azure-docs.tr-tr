---
title: 'Öğretici: ThousandEyes ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve ThousandEyes arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 10/04/2019
ms.author: jeedes
ms.openlocfilehash: 2b99d5ea33693431fb3811af50385891684a366e
ms.sourcegitcommit: 023d10b4127f50f301995d44f2b4499cbcffb8fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88546466"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-thousandeyes"></a>Öğretici: ThousandEyes ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory

Bu öğreticide, ThousandEyes 'i Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. ThousandEyes 'i Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de ThousandEyes 'e erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla ThousandEyes otomatik olarak oturum açmalarına olanak tanıyın.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

Azure AD ile SaaS uygulaması tümleştirmesi hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* ThousandEyes çoklu oturum açma (SSO) etkin abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz.

* ThousandEyes **SP ve ıDP** tarafından başlatılan SSO 'yu destekler
* ThousandEyes [ **Otomatik** Kullanıcı sağlamayı destekler](https://docs.microsoft.com/azure/active-directory/saas-apps/thousandeyes-provisioning-tutorial)

> [!NOTE]
> Bu uygulamanın tanımlayıcısı, tek bir kiracıda yalnızca bir örneğin yapılandırılabilmesini sağlamak için sabit bir dize değeridir.

## <a name="adding-thousandeyes-from-the-gallery"></a>Galeriden ThousandEyes ekleme

ThousandEyes tümleştirmesini Azure AD 'ye göre yapılandırmak için, Galeriden ThousandEyes yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

1. [Azure Portal](https://portal.azure.com) iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **ThousandEyes** yazın.
1. Sonuçlar panelinden **ThousandEyes** ' i seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-thousandeyes"></a>ThousandEyes için Azure AD çoklu oturum açmayı yapılandırma ve test etme

**B. Simon**adlı bir test kullanıcısı kullanarak Azure AD SSO 'yu ThousandEyes ile yapılandırın ve test edin. SSO 'nun çalışması için, ThousandEyes içinde bir Azure AD kullanıcısı ve ilgili Kullanıcı arasında bir bağlantı ilişkisi oluşturmanız gerekir.

Azure AD SSO 'yu ThousandEyes ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını doldurun:

1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
    * Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -B. Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
    * Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek için.
1. Uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için **[THOUSANDEYES SSO 'Yu yapılandırın](#configure-thousandeyes-sso)** .
    * Kullanıcının Azure AD gösterimine bağlı olan ThousandEyes 'de B. Simon 'ya karşılık gelen bir **[ThousandEyes test kullanıcısı oluşturun](#create-thousandeyes-test-user)** .
1. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. [Azure Portal](https://portal.azure.com/), **ThousandEyes** uygulama tümleştirmesi sayfasında **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** için Düzenle/kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. **Temel SAML yapılandırması** bölümünde, uygulama önceden yapılandırılmıştır ve gerekli URL 'ler Azure ile önceden doldurulmuştur. Kullanıcının **Kaydet** düğmesine tıklayarak yapılandırmayı kaydetmesi gerekir.

1. Uygulamayı **SP** tarafından başlatılan modda yapılandırmak Istiyorsanız **ek URL 'ler ayarla** ' ya tıklayın ve aşağıdaki adımı gerçekleştirin:

    **Oturum açma URL 'si** metin kutusuna bir URL yazın:`https://app.thousandeyes.com/login/sso`

1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, **SAML Imzalama sertifikası** bölümünde **sertifika bulun (base64)** ve sertifikayı indirip bilgisayarınıza kaydetmek için **İndir** ' i seçin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. **ThousandEyes ayarla** bölümünde, gereksiniminize göre uygun URL 'leri kopyalayın.

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

Bu bölümde, ThousandEyes 'e erişim vererek Azure çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde **ThousandEyes**' yi seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle**' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. SAML assertion 'da herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, Kullanıcı için listeden uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

## <a name="configure-thousandeyes-sso"></a>ThousandEyes SSO 'yu yapılandırma

1. Farklı bir Web tarayıcısı penceresinde, **ThousandEyes** şirket sitenizde yönetici olarak oturum açın.

2. Üstteki menüde, **Ayarlar**' a tıklayın.

    ![Ayarlar](./media/thousandeyes-tutorial/ic790066.png "Ayarlar")

3. **Hesap** öğesine tıklayın

    ![Hesap](./media/thousandeyes-tutorial/ic790067.png "Hesap")

4. **Güvenlik & kimlik doğrulaması** sekmesine tıklayın.

    ![Güvenlik & kimlik doğrulaması](./media/thousandeyes-tutorial/ic790068.png "Güvenlik & kimlik doğrulaması")

5. **Çoklu oturum açma kurulumu** bölümünde aşağıdaki adımları uygulayın:

    ![Çoklu oturum açma kurulumu](./media/thousandeyes-tutorial/ic790069.png "Çoklu oturum açma kurulumu")

    a. **Çoklu oturum açmayı etkinleştir '** i seçin.

    b. **Oturum açma sayfası URL 'si** metin kutusunda, Azure Portal kopyaladığınız **oturum açma URL 'sini**yapıştırın.

    c. **Oturum kapatma sayfası URL 'si** metin kutusunda, Azure Portal kopyaladığınız **oturum kapatma URL 'sini**yapıştırın.

    d. **Kimlik sağlayıcısı veren** metin kutusu, Azure Portal KOPYALADıĞıNıZ **Azure ad tanımlayıcısını**yapıştırın.

    e. **Doğrulama sertifikası**' nda **Dosya Seç**' e tıklayın ve ardından Azure Portal indirdiğiniz sertifikayı karşıya yükleyin.

    f. **Kaydet**’e tıklayın.

### <a name="create-thousandeyes-test-user"></a>ThousandEyes test kullanıcısı oluştur

Bu bölümün amacı, ThousandEyes ' de Britta Simon adlı bir Kullanıcı oluşturmaktır. ThousandEyes, varsayılan olarak etkinleştirilen Otomatik Kullanıcı sağlamasını destekler. Otomatik Kullanıcı sağlamayı yapılandırma hakkında daha [fazla ayrıntı bulabilirsiniz](thousandeyes-provisioning-tutorial.md) .

**Kullanıcı el ile oluşturmanız gerekiyorsa aşağıdaki adımları gerçekleştirin:**

1. ThousandEyes şirket sitenizde yönetici olarak oturum açın.

2. **Ayarlar**'a tıklayın.

    ![Ayarlar](./media/thousandeyes-tutorial/IC790066.png "Ayarlar")

3. **Hesap**' a tıklayın.

    ![Hesap](./media/thousandeyes-tutorial/IC790067.png "Hesap")

4. **Hesaplar & kullanıcılar** sekmesine tıklayın.

    ![Kullanıcı & hesaplar](./media/thousandeyes-tutorial/IC790073.png "Kullanıcı & hesaplar")

5. **Kullanıcı & hesaplarını Ekle** bölümünde aşağıdaki adımları uygulayın:

    ![Kullanıcı hesapları ekleme](./media/thousandeyes-tutorial/IC790074.png "Kullanıcı hesapları ekleme")

    a. **Ad** metin kutusuna **B. Simon**gibi kullanıcının adını yazın.

    b. **E-posta** metin kutusuna kullanıcının e-postasını yazın b.simon@contoso.com .

    b. **Hesaba Yeni Kullanıcı Ekle**' ye tıklayın.

    > [!NOTE]
    > Azure Active Directory hesap sahibi, hesabı onaylamaya ve etkinleştirmeye yönelik bir bağlantı içeren bir e-posta alır.

> [!NOTE]
> Azure Active Directory Kullanıcı hesapları sağlamak için, ThousandEyes tarafından sunulan diğer tüm ThousandEyes Kullanıcı hesabı oluşturma araçlarını veya API 'Leri kullanabilirsiniz.


## <a name="test-sso"></a>Test SSO 'SU 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim panelinde ThousandEyes kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız ThousandEyes için otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

- [ SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile ThousandEyes deneyin](https://aad.portal.azure.com/)

- [Kullanıcı sağlamayı yapılandırma](https://docs.microsoft.com/azure/active-directory/saas-apps/thousandeyes-provisioning-tutorial)