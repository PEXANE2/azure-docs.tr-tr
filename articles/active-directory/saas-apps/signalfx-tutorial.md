---
title: 'Öğretici Azure Active Directory: SignalFx ile çoklu oturum açma (SSO) Tümleştirmesi | Microsoft Docs'
description: Azure Active Directory ve SignalFx arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 6d5ab4b0-29bc-4b20-8536-d64db7530f32
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/10/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: ea81f0046d7f73d845ed49325a3d621e6b7735e7
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75443278"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-signalfx"></a>Öğretici: SignalFx ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory

Bu öğreticide, SignalFx 'i Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. SignalFx 'i Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de SignalFx 'e erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla SignalFx 'e otomatik olarak kaydolmalarına imkan tanıyın.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

Azure AD ile SaaS uygulaması tümleştirmesi hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* SignalFx çoklu oturum açma (SSO) etkin aboneliği.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz.

* SignalFx, **IDP** tarafından başlatılan SSO 'yu destekler
* SignalFx **, tam zamanında** Kullanıcı sağlamasını destekler

## <a name="adding-signalfx-from-the-gallery"></a>Galeriden SignalFx ekleme

SignalFx 'in Azure AD 'ye tümleştirilmesini yapılandırmak için, galerisindeki SignalFx 'i yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalda](https://portal.azure.com) oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **signalfx** yazın.
1. Sonuçlar panelinden **Signalfx** ' i seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-signalfx"></a>SignalFx için Azure AD çoklu oturum açmayı yapılandırma ve test etme

**B. Simon**adlı bir test kullanıcısı kullanarak, signalfx Ile Azure AD SSO 'yu yapılandırın ve test edin. SSO 'nun çalışması için, SignalFx içindeki bir Azure AD kullanıcısı ve ilgili Kullanıcı arasında bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO 'yu SignalFx ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını doldurun:

1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
    * Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -B. Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
    * Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek için.
1. **[SignalFx SSO 'Yu yapılandırma](#configure-signalfx-sso)** -uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
    * Signalfx **[test kullanıcısı oluşturun](#create-signalfx-test-user)** -kullanıcının Azure AD gösterimine bağlı olan Signalfx 'te B. Simon 'a karşılık gelen bir karşılığı vardır.
1. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO 'yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. [Azure Portal](https://portal.azure.com/), **signalfx** uygulama tümleştirmesi sayfasında **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** için Düzenle/kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, aşağıdaki alanlar için değerleri girin:

    a. **Tanımlayıcı** metin kutusuna bir URL yazın: `https://api.signalfx.com/v1/saml/metadata`

    b. **Yanıt URL 'si** metin kutusuna şu kalıbı kullanarak bir URL yazın: `https://api.signalfx.com/v1/saml/acs/<integration ID>`

    > [!NOTE]
    > Yukarıdaki değer gerçek değer değildir. Değeri, Öğreticinin ilerleyen kısımlarında açıklanan gerçek yanıt URL 'siyle güncelleştirin.

1. SignalFx uygulaması, SAML belirteci öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML onayları bekler. Aşağıdaki ekran görüntüsünde varsayılan özniteliklerin listesi gösterilmektedir.

    ![image](common/default-attributes.png)

1. Daha fazlasına ek olarak, SignalFx uygulaması aşağıda gösterilen SAML yanıtına daha fazla öznitelik geçirilmesini bekler. Bu öznitelikler de önceden doldurulur, ancak gereksinimlerinize göre bunları gözden geçirebilirsiniz.

    | Ad |  Kaynak özniteliği|
    | ------------------- | -------------------- |
    | Kullanıcı. FirstName  | Kullanıcı. |
    | Kullanıcı. e-posta  | Kullanıcı. Mail |
    | Personimmutableıd       | User. UserPrincipalName    |
    | User. LastName       | User. soyadı    |

1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, **SAML Imzalama sertifikası** bölümünde **sertifika bulun (base64)** ve sertifikayı indirip bilgisayarınıza kaydetmek için **İndir** ' i seçin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. **SignalFx ayarla** bölümünde, gereksiniminize göre uygun URL 'leri kopyalayın.

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

Bu bölümde, SignalFx 'e erişim vererek Azure çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde, **Signalfx**' i seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.

   !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle**' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. SAML assertion 'da herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, Kullanıcı için listeden uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

## <a name="configure-signalfx-sso"></a>SignalFx SSO 'yu yapılandırma

1. SignalFx şirket sitenizde yönetici olarak oturum açın.

1. SignalFx 'te, en üstteki tümleştirmeler sayfasını açmak için **tümleştirmelere** tıklayın.

    ![SignalFx tümleştirmesi](./media/signalfx-tutorial/tutorial_signalfx_intg.png)

1. **Oturum açma hizmetleri** bölümünde **Azure Active Directory** kutucuğa tıklayın.

    ![SignalFx SAML](./media/signalfx-tutorial/tutorial_signalfx_saml.png)

1. **Yenı tümleştirme** ' e tıklayın ve **Install** sekmesinde aşağıdaki adımları uygulayın:

    ![SignalFx samlintgsayfası](./media/signalfx-tutorial/tutorial_signalfx_azure.png)

    a. **Name** metin kutusu türünde, **OurOrgName SAML SSO**gibi yeni bir tümleştirme adı.

    b. **TÜMLEŞTIRME kimliği** değerini kopyalayın ve Azure Portal IÇINDEKI **temel SAML YAPıLANDıRMASı** bölümünün **yanıt URL** metin kutusuna `<integration ID>` yerine **yanıt URL** 'sini ekleyin.

    c. **Sertifika** metin kutusunda Azure Portal indirilen **base64 kodlu sertifikayı** karşıya yüklemek için **dosyayı karşıya yükle** ' ye tıklayın.

    d. **Veren URL** metin kutusuna, Azure Portal KOPYALADıĞıNıZ **Azure AD tanımlayıcısının**değerini yapıştırın.

    e. **Meta veri URL 'si** metin kutusunda, Azure Portal kopyaladığınız **oturum açma URL 'sini** yapıştırın.

    f. **Save (Kaydet)** düğmesine tıklayın.

### <a name="create-signalfx-test-user"></a>SignalFx test kullanıcısı oluşturma

Bu bölümün amacı, SignalFx 'de Britta Simon adlı bir Kullanıcı oluşturmaktır. SignalFx, varsayılan olarak etkinleştirilen tam zamanında sağlamayı destekler. Bu bölümde sizin için herhangi bir eylem öğesi yok. Henüz mevcut değilse, SignalFx 'e erişme girişimi sırasında yeni bir Kullanıcı oluşturulur.

Bir Kullanıcı, SAML SSO 'SU 'nden ilk kez SignalFx ' te oturum açtığında, [signalfx destek ekibi](mailto:kmazzola@signalfx.com) , kimlik doğrulaması için tıklamaları gereken bir bağlantı içeren bir e-posta gönderir. Bu, yalnızca Kullanıcı ilk kez oturum açtığında gerçekleşir; sonraki oturum açma girişimleri, e-posta doğrulaması gerektirmez.

> [!Note]
> El ile bir kullanıcı oluşturmanız gerekiyorsa, [Signalfx destek ekibine](mailto:kmazzola@signalfx.com) başvurun

## <a name="test-sso"></a>Test SSO 'SU

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde SignalFx kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız SignalFx ' de otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile SignalFx 'i deneyin](https://aad.portal.azure.com/)