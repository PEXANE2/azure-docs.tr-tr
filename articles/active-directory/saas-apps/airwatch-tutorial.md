---
title: 'Öğretici: AirWatch ile tümleştirme Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve AirWatch arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 96a3bb1c-96c6-40dc-8ea0-060b0c2a62e5
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 07/11/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 772b37816b83c275bae927d825434dc3ca76a35c
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "74231983"
---
# <a name="tutorial-integrate-airwatch-with-azure-active-directory"></a>Öğretici: AirWatch 'u Azure Active Directory tümleştirin

Bu öğreticide, AirWatch 'u Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. AirWatch 'u Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de AirWatch 'a erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla AirWatch 'ta otomatik olarak oturum açmalarına olanak sağlayın.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

Azure AD ile SaaS uygulaması tümleştirmesi hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/)bir aylık ücretsiz deneme sürümü edinebilirsiniz.
* AirWatch çoklu oturum açma (SSO) etkin aboneliği.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz. AirWatch, **SP** tarafından başlatılan SSO 'yu destekler.

## <a name="adding-airwatch-from-the-gallery"></a>Galeriden AirWatch ekleme

AirWatch 'un Azure AD ile tümleştirilmesini yapılandırmak için, Galeriden yönetim SaaS uygulamaları listenize AirWatch eklemeniz gerekir.

1. [Azure Portal](https://portal.azure.com) iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, ara kutusuna **AirWatch** yazın.
1. Sonuçlar panelinden **AirWatch** ' ı seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma ve test etme

**B. Simon**adlı bir test kullanıcısı kullanarak AirWatch Ile Azure AD SSO 'yu yapılandırın ve test edin. SSO 'nun çalışması için, bir Azure AD kullanıcısı ile AirWatch 'daki ilgili Kullanıcı arasında bir bağlantı ilişkisi oluşturmanız gerekir.

Azure AD SSO 'yu AirWatch ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını doldurun:

1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
2. **[AirWatch SSO 'Yu yapılandırma](#configure-airwatch-sso)** -uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -Britta Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
4. AirWatch **[test kullanıcısı oluşturun](#create-airwatch-test-user)** -kullanıcının Azure AD gösterimine bağlı olan uçak Watch 'Da Britta Simon 'ın bir karşılığı olmalıdır.
5. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanarak Britta Simon 'u etkinleştirin.
6. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. [Azure Portal](https://portal.azure.com/), **AirWatch** uygulama tümleştirmesi sayfasında **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** için Düzenle/kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. **Temel SAML yapılandırması** sayfasında, aşağıdaki alanlar için değerleri girin:

    1. **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<subdomain>.awmdm.com/AirWatch/Login?gid=companycode`

    1. **Tanımlayıcı (VARLıK kimliği)** metin kutusuna değeri şöyle yazın:`AirWatch`

    > [!NOTE]
    > Bu değer gerçek değil. Bu değeri, gerçek oturum açma URL 'siyle güncelleştirin. Bu değeri almak için [AirWatch istemci destek ekibine](https://www.air-watch.com/company/contact-us/) başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

1. AirWatch uygulaması, SAML onaylamalarını belirli bir biçimde bekliyor. Bu uygulama için aşağıdaki talepleri yapılandırın. Bu özniteliklerin değerlerini, uygulama tümleştirme sayfasındaki **Kullanıcı öznitelikleri** bölümünden yönetebilirsiniz. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **Kullanıcı öznitelikleri** Iletişim kutusunu açmak için **Düzenle** düğmesine tıklayın.

    ![image](common/edit-attribute.png)

1. **Kullanıcı öznitelikleri** Iletişim kutusundaki **Kullanıcı talepleri** bölümünde, yukarıdaki görüntüde gösterildiği gibi, **Düzen simgesini** kullanarak talepleri DÜZENLEYIN veya aşağıdaki resimde gösterildiği gibi SAML belirteci özniteliğini yapılandırmak için **yeni talep Ekle** ' yi kullanarak talepleri ekleyin ve aşağıdaki adımları gerçekleştirin:

    | Adı |  Kaynak özniteliği|
    |---------------|----------------|
    | 'SINI | User. UserPrincipalName |
    | | |

    a. **Kullanıcı taleplerini Yönet** iletişim kutusunu açmak için **yeni talep Ekle** ' ye tıklayın.

    b. **Ad** metin kutusuna, bu satır için gösterilen öznitelik adını yazın.

    c. **Ad alanını** boş bırakın.

    d. **Öznitelik**olarak kaynak seçin.

    e. **Kaynak özniteliği** listesinde, bu satır için gösterilen öznitelik değerini yazın.

    f. **Tamam 'a** tıklayın

    g. **Kaydet**’e tıklayın.

1. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde, **Federasyon meta verileri XML** 'i bulun ve meta veri XML 'Sini indirmek ve bilgisayarınıza kaydetmek için **İndir** ' i seçin.

   ![Sertifika indirme bağlantısı](common/metadataxml.png)

1. **AirWatch 'U ayarla** bölümünde, gereksiniminize göre uygun URL 'leri kopyalayın.

   ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

### <a name="configure-airwatch-sso"></a>AirWatch SSO 'yu yapılandırma

1. Farklı bir Web tarayıcısı penceresinde, AirWatch şirket sitenizde yönetici olarak oturum açın.

1. Ayarlar sayfasında. **> dizin hizmetleri kurumsal tümleştirme ayarları >** seçin.

   ![Ayarlar](./media/airwatch-tutorial/ic791921.png "Ayarlar")

1. **Kullanıcı** sekmesine tıklayın, **Taban DN** metin kutusunda, etki alanı adınızı yazın ve ardından **Kaydet**' e tıklayın.

   ![Kullanıcı](./media/airwatch-tutorial/ic791922.png "Kullanıcı")

1. **Sunucu** sekmesine tıklayın.

   ![Sunucu](./media/airwatch-tutorial/ic791923.png "Sunucu")

1. **LDAP** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Karşıya yükle](./media/airwatch-tutorial/ic791924.png "LDAP")   

    a. **Dizin türü**olarak, **hiçbiri**' ni seçin.

    b. **Kimlik doğrulaması IÇIN SAML kullan**' ı seçin.

1. **SAML 2,0** bölümünde, indirilen sertifikayı karşıya yüklemek Için **karşıya yükle**' ye tıklayın.

    ![Karşıya yükle](./media/airwatch-tutorial/ic791932.png "Karşıya Yükle")

1. **İstek** bölümünde aşağıdaki adımları uygulayın:

    ![İstek](./media/airwatch-tutorial/ic791925.png "İstek")  

    a. **Istek bağlama türü**olarak **gönderi**' ı seçin.

    b. Azure portal, **AirWatch 'da çoklu oturum açmayı Yapılandır** iletişim sayfasında, **oturum açma URL 'si** değerini kopyalayın ve ardından **kimlik sağlayıcısı çoklu oturum açma URL 'si** metin kutusuna yapıştırın.

    c. **NameID biçimi**olarak **e-posta adresi**' ni seçin.

    d. **Kimlik doğrulama Isteği güvenliği**olarak **hiçbiri**' ni seçin.

    e. **Kaydet**’e tıklayın.

1. **Kullanıcı** sekmesine tekrar tıklayın.

    ![Kullanıcı](./media/airwatch-tutorial/ic791926.png "Kullanıcı")

1. **Öznitelik** bölümünde aşağıdaki adımları uygulayın:

    ![Öznitelik](./media/airwatch-tutorial/ic791927.png "Öznitelik")

    a. **Nesne tanımlayıcısı** metin kutusuna yazın `http://schemas.microsoft.com/identity/claims/objectidentifier`.

    b. **Kullanıcı adı** metin kutusuna yazın `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.

    c. **Görünen ad** metin kutusuna yazın `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.

    d. **Ilk ad** metin kutusuna yazın `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.

    e. **Soyadı** metin kutusuna yazın `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.

    f. **E-posta** metin kutusuna yazın `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.

    g. **Kaydet**’e tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, B. Simon adlı Azure portal bir test kullanıcısı oluşturacaksınız.

1. Azure portal sol bölmeden **Azure Active Directory**' i seçin, **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.
1. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.
1. **Kullanıcı** özellikleri ' nde şu adımları izleyin:
   1. **Ad** alanına `B.Simon` girin.  
   1. **Kullanıcı adı** alanına, username@companydomain.extensiongirin. Örneğin, `B.Simon@contoso.com`.
   1. **Parolayı göster** onay kutusunu seçin ve ardından **parola** kutusunda görüntülenen değeri yazın.
   1. **Oluştur**' a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, AirWatch 'a erişim vererek Azure çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde **AirWatch**' u seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle**' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. SAML assertion 'da herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, Kullanıcı için listeden uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

### <a name="create-airwatch-test-user"></a>AirWatch test kullanıcısı oluşturma

Azure AD kullanıcılarının AirWatch 'da oturum açmasını sağlamak için, bu kullanıcıların AirWatch 'da sağlanması gerekir. AirWatch durumunda, sağlama el ile gerçekleştirilen bir görevdir.

**Kullanıcı sağlamayı yapılandırmak için aşağıdaki adımları uygulayın:**

1. **AirWatch** şirket sitenizde yönetici olarak oturum açın.

2. Sol taraftaki Gezinti bölmesinde **hesaplar**' a ve ardından **Kullanıcılar**' a tıklayın.
  
   ![Kullanıcılar](./media/airwatch-tutorial/ic791929.png "Kullanıcılar")

3. **Kullanıcılar** menüsünde **liste görünümü**' ne ve ardından **Kullanıcı Ekle > Ekle**' ye tıklayın.
  
   ![Kullanıcı Ekle](./media/airwatch-tutorial/ic791930.png "Kullanıcı Ekleme")

4. **Kullanıcı Ekle/Düzenle** iletişim kutusunda aşağıdaki adımları gerçekleştirin:

   ![Kullanıcı Ekle](./media/airwatch-tutorial/ic791931.png "Kullanıcı Ekleme")

   a. İlgili metin kutularına sağlamak istediğiniz geçerli bir Azure Active Directory hesabının **Kullanıcı**adı, **parola**, **parolayı onaylayın**, **adı**, **Soyadı**, **e-posta adresini** yazın.

   b. **Kaydet**’e tıklayın.

> [!NOTE]
> Azure AD Kullanıcı hesapları sağlamak için AirWatch tarafından sunulan diğer AirWatch Kullanıcı hesabı oluşturma araçlarını veya API 'Leri kullanabilirsiniz.

### <a name="test-sso"></a>Test SSO 'SU

Erişim panelinde AirWatch kutucuğunu seçtiğinizde, SSO 'yu ayarladığınız AirWatch 'da otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
