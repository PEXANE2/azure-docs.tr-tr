---
title: 'Öğretici: TimeOffManager ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve TimeOffManager arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 3685912f-d5aa-4730-ab58-35a088fc1cc3
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/10/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 06aa2ddf3e7168147ec091ef6fb9826025f23364
ms.sourcegitcommit: 5925df3bcc362c8463b76af3f57c254148ac63e3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/31/2019
ms.locfileid: "75561822"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-timeoffmanager"></a>Öğretici: TimeOffManager ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory

Bu öğreticide, TimeOffManager 'ı Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. TimeOffManager 'ı Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de, TimeOffManager 'a erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla zaman aşımına uğradı ve otomatik olarak oturum açmalarına olanak sağlayın.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

Azure AD ile SaaS uygulaması tümleştirmesi hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* TimeOffManager çoklu oturum açma (SSO) etkin aboneliği.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz.


* TimeOffManager, **IDP** tarafından başlatılan SSO 'yu destekliyor

* TimeOffManager **, tam zamanında** Kullanıcı sağlamayı destekliyor

> [!NOTE]
> Bu uygulamanın tanımlayıcısı, tek bir kiracıda yalnızca bir örneğin yapılandırılabilmesini sağlamak için sabit bir dize değeridir.


## <a name="adding-timeoffmanager-from-the-gallery"></a>Galeriden TimeOffManager ekleme

TimeOffManager 'ın Azure AD ile tümleştirilmesini yapılandırmak için, Galeriden TimeOffManager 'ı yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalda](https://portal.azure.com) oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **TimeOffManager** yazın.
1. Sonuçlar panelinden **TimeOffManager** ' ı seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.


## <a name="configure-and-test-azure-ad-single-sign-on-for-timeoffmanager"></a>TimeOffManager için Azure AD çoklu oturum açmayı yapılandırma ve test etme

**B. Simon**adlı bir test kullanıcısı kullanarak, TimeOffManager Ile Azure AD SSO 'yu yapılandırın ve test edin. SSO 'nun çalışması için, TimeOffManager 'da bir Azure AD kullanıcısı ve ilgili Kullanıcı arasında bir bağlantı ilişkisi oluşturmanız gerekir.

Azure AD SSO 'yu TimeOffManager ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını doldurun:

1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
    1. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -B. Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
    1. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek için.
1. **[TimeOffManager SSO 'Yu yapılandırma](#configure-timeoffmanager-sso)** -uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
    1. **[TimeOffManager test kullanıcısı oluşturun](#create-timeoffmanager-test-user)** -kullanıcının Azure AD gösterimine bağlı olan, TimeOffManager 'da B. Simon 'a karşılık gelen bir karşılığı elde edin.
1. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO 'yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. [Azure Portal](https://portal.azure.com/), **TimeOffManager** uygulama tümleştirmesi sayfasında, **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** için Düzenle/kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. **Temel SAML yapılandırması** bölümünde, aşağıdaki alanlar için değerleri girin:

    **Yanıt URL 'si** metin kutusuna şu kalıbı kullanarak bir URL yazın: `https://www.timeoffmanager.com/cpanel/sso/consume.aspx?company_id=<companyid>`

    > [!NOTE]
    > Bu değer gerçek değil. Bu değeri gerçek yanıt URL 'siyle güncelleştirin. Bu değeri, öğreticide daha sonra açıklanan **Çoklu oturum açma ayarları sayfasından** alabilir veya [TimeOffManager destek ekibine](https://www.purelyhr.com/contact-us)başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

1. TimeOffManager uygulaması, SAML belirteci öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML onayları bekliyor. Aşağıdaki ekran görüntüsünde varsayılan özniteliklerin listesi gösterilmektedir.

    ![image](common/edit-attribute.png)

1. Ayrıca, TimeOffManager uygulaması, daha fazla özniteliğin aşağıda gösterilen SAML yanıtına geri geçirilmesini bekler. Bu öznitelikler de önceden doldurulur, ancak gereksiniminize göre bunları gözden geçirebilirsiniz.

    | Ad | Kaynak özniteliği|
    | --- | --- |
    | FirstName |Kullanıcı. |
    | Soyadı |User. soyadı |
    | E-posta |Kullanıcı. Mail |

1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, **SAML Imzalama sertifikası** bölümünde **sertifika bulun (base64)** ve sertifikayı indirip bilgisayarınıza kaydetmek için **İndir** ' i seçin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. **TimeOffManager ayarlama** bölümünde, gereksiniminize göre uygun URL 'leri kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümde, B. Simon adlı Azure portal bir test kullanıcısı oluşturacaksınız.

1. Azure portal sol bölmeden **Azure Active Directory**' i seçin, **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.
1. Seçin **yeni kullanıcı** ekranın üstünde.
1. **Kullanıcı** özellikleri ' nde şu adımları izleyin:
   1. **Ad** alanına `B.Simon` girin.  
   1. **Kullanıcı adı** alanına username@companydomain.extensiongirin. Örneğin, `B.Simon@contoso.com`.
   1. **Parolayı göster** onay kutusunu seçin ve ardından **parola** kutusunda görüntülenen değeri yazın.
   1. **Oluştur**'a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, TimeOffManager erişimi vererek Azure çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde, **TimeOffManager**' ı seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.

   !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle**' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. SAML assertion 'da herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, Kullanıcı için listeden uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

## <a name="configure-timeoffmanager-sso"></a>TimeOffManager SSO 'yu yapılandırma

1. Farklı bir Web tarayıcısı penceresinde, TimeOffManager şirket sitenizde yönetici olarak oturum açın.

2. **Çoklu oturum açma ayarları \> hesap \> hesap seçenekleri**' ne gidin.
   
    ![Çoklu oturum açma ayarları](./media/timeoffmanager-tutorial/ic795917.png "Çoklu Oturum Açma Ayarları")

3. **Çoklu oturum açma ayarları** bölümünde aşağıdaki adımları uygulayın:
   
    ![Çoklu oturum açma ayarları](./media/timeoffmanager-tutorial/ic795918.png "Çoklu Oturum Açma Ayarları")
   
    a. Base-64 kodlu sertifikanızı Not defteri 'nde açın, bu içeriği panonuza kopyalayın ve ardından tüm sertifikayı **X. 509.440 sertifikası** metin kutusuna yapıştırın.
   
    b. **IDP veren** metin kutusunda, Azure Portal KOPYALADıĞıNıZ **Azure AD tanımlayıcısının** değerini yapıştırın.
   
    c. **IDP uç nokta URL 'si** metin kutusunda, Azure Portal kopyaladığınız **oturum açma URL 'si** değerini yapıştırın.
   
    d. **SAML zorlama**olarak **Hayır**' ı seçin.
   
    e. **Kullanıcıları otomatik oluştur**olarak **Evet**' i seçin.
   
    f. **Logout URL** metin kutusuna, Azure Portal kopyaladığınız **Logout URL 'si** değerini yapıştırın.
   
    g. **Değişiklikleri Kaydet**' e tıklayın.

4. **Çoklu oturum açma ayarları** sayfasında, **onaylama tüketici hizmeti URL 'si** değerini kopyalayın ve Azure Portal içindeki **temel SAML yapılandırması** bölümündeki **yanıt URL** 'si metin kutusuna yapıştırın. 

      ![Çoklu oturum açma ayarları](./media/timeoffmanager-tutorial/ic795915.png "Çoklu Oturum Açma Ayarları")

### <a name="create-timeoffmanager-test-user"></a>TimeOffManager test kullanıcısı oluşturma

Bu bölümde, TimeOffManager 'da Britta Simon adlı bir Kullanıcı oluşturulur. TimeOffManager, varsayılan olarak etkinleştirilen tam zamanında Kullanıcı sağlamayı destekler. Bu bölümde sizin için herhangi bir eylem öğesi yok. Kullanıcı TimeOffManager 'da zaten mevcut değilse, kimlik doğrulamasından sonra yeni bir tane oluşturulur.

>[!NOTE]
>Azure AD Kullanıcı hesapları sağlamak için TimeOffManager tarafından sunulan diğer bir TimeOffManager Kullanıcı hesabı oluşturma aracını veya API 'Leri kullanabilirsiniz.

## <a name="test-sso"></a>Test SSO 'SU 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde TimeOffManager kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız TimeOffManager 'da otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile TimeOffManager 'ı deneyin](https://aad.portal.azure.com/)

