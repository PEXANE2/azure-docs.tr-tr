---
title: 'Öğretici: Firmex VDR ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ile Firmex VDR arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 01/21/2020
ms.author: jeedes
ms.openlocfilehash: ab480dd8dae00cd48811473a7b335291fb6300eb
ms.sourcegitcommit: 023d10b4127f50f301995d44f2b4499cbcffb8fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88555028"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-firmex-vdr"></a>Öğretici: Firmex VDR ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory

Bu öğreticide, Firmex VDR 'yi Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. Firmex VDR 'yi Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de Firmex VDR erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Firmex VDR 'ye otomatik olarak kaydolmalarına imkan tanıyın.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

Azure AD ile SaaS uygulaması tümleştirmesi hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Firmex VDR çoklu oturum açma (SSO) aboneliği etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz.

* Firmex VDR **SP ve ıDP** tarafından başlatılan SSO 'yu destekler

* Firmex 'yı yapılandırdıktan sonra, kuruluşunuzun gizli verilerinin gerçek zamanlı olarak ayıklanmasını ve zaman korumasını koruyan oturum denetimleri uygulayabilirsiniz. Oturum denetimleri koşullu erişimden genişletilir. [Microsoft Cloud App Security ile oturum denetimini nasıl zorlayacağınızı öğrenin](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app).

## <a name="adding-firmex-vdr-from-the-gallery"></a>Galeriden Firmex VDR ekleme

Firmex VDR 'yi Azure AD 'ye tümleştirmeyi yapılandırmak için, galerisindeki Firmex VDR 'yi yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. [Azure Portal](https://portal.azure.com) iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **firmex VDR** yazın.
1. Sonuçlar panelinden **Firmex VDR** ' ı seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.


## <a name="configure-and-test-azure-ad-single-sign-on-for-firmex-vdr"></a>Firmex VDR için Azure AD çoklu oturum açmayı yapılandırma ve test etme

**B. Simon**adlı bir test kullanıcısı kullanarak FIRMEX VDR Ile Azure AD SSO 'yu yapılandırın ve test edin. SSO 'nun çalışması için, bir Azure AD kullanıcısı ve Firmex VDR içindeki ilgili Kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO 'yu Firmex VDR ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını doldurun:

1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
    * Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -B. Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
    * Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek için.
1. Uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için **[Firmex VDR SSO 'Yu yapılandırın](#configure-firmex-vdr-sso)** .
    * **[Firmex VDR test kullanıcısı oluşturma](#create-firmex-vdr-test-user)** -Kullanıcı Azure AD gösterimine bağlı olan FIRMEX VDR 'de B. Simon 'a sahip olmak için.
1. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. [Azure Portal](https://portal.azure.com/), **firmex VDR** uygulama tümleştirmesi sayfasında, **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML ile çoklu oturum açmayı ayarla** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** için Düzenle/kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. **Temel SAML yapılandırması** bölümünde, uygulama zaten Azure ile önceden tümleştirildiği için kullanıcının herhangi bir adım yapması gerekmez.

1. Uygulamayı **SP** tarafından başlatılan modda yapılandırmak Istiyorsanız **ek URL 'ler ayarla** ' ya tıklayın ve aşağıdaki adımı gerçekleştirin:

    **Oturum açma URL** 'si metin kutusuna URL 'yi yazın:`https://login.firmex.com`

1. **Kaydet**’e tıklayın.

1. Firmex VDR uygulaması, SAML belirteci öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML onayları bekliyor. Aşağıdaki ekran görüntüsünde varsayılan özniteliklerin listesi gösterilmektedir.

    ![image](common/default-attributes.png)

1. Firmex VDR uygulaması, yukarıdakine ek olarak aşağıda gösterilen SAML yanıtına daha fazla öznitelik geçirilmesini bekler. Bu öznitelikler de önceden doldurulur, ancak gereksinimlerinize göre bunları gözden geçirebilirsiniz.

    | Adı | Kaynak özniteliği|
    | ------------ | --------- |
    | e-posta | Kullanıcı. Mail |

1. **SAML ile çoklu oturum açmayı ayarla** sayfasında, **SAML imzalama sertifikası** bölümünde, **Federasyon meta verileri XML** 'i bulun ve sertifikayı indirip bilgisayarınıza kaydetmek için **İndir** ' i seçin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

1. **Kurulum Firmex VDR** bölümünde, gereksiniminize göre uygun URL 'leri kopyalayın.

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

Bu bölümde, Firmex VDR 'ye erişim izni vererek Azure çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde **Firmex VDR**öğesini seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle**' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. SAML assertion 'da herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, Kullanıcı için listeden uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

## <a name="configure-firmex-vdr-sso"></a>Firmex VDR SSO 'yu yapılandırma

### <a name="before-you-get-started"></a>Başlamadan önce

#### <a name="what-youll-need"></a>İhtiyacınız olacak

-   Etkin bir Firmex aboneliği
-   SSO hizmetiniz olarak Azure AD
-   SSO 'yu yapılandırmak için BT yöneticiniz
-   SSO etkinleştirildikten sonra, tüm şirketinizin kullanıcıları SSO kullanarak Firmex ' da oturum açması ve oturum açma/parola kullanmıyor olmalıdır.

#### <a name="how-long-will-this-take"></a>Bu işlem ne kadar sürer?

SSO uygulama birkaç dakika sürer. Firmex desteği arasında, siteniz için SSO 'yu etkinleştiren ve şirketinizin kullanıcıları SSO kullanarak kimlik doğrulayan neredeyse kapalı kalma süresi yoktur. Aşağıdaki adımları izlemeniz yeterlidir.

### <a name="step-1---identify-your-companys-domains"></a>1. adım-şirketinizin etki alanlarını tanımla

Şirketinizin kullanıcılarının oturum açtığı etki alanlarını belirler.

Örneğin:

- @firmex.com
- @firmex.ca

### <a name="step-2---contact-firmex-support-with-your-domains"></a>2. adım-etki alanlarınızda Firmex desteğiyle Iletişim kurun

Firmex destek [ekibi](mailto:support@firmex.com) e-postası veya 1888 688 4042 x. 11 ' i arayarak firmex desteğiyle konuşun. Etki alanı bilgilerinizi geçirin. Firmex desteği, etki alanlarını, **istenen etki alanları**olarak VDR 'nize ekler. Yöneticinizin artık SSO 'yu yapılandırması gerekir.

Uyarı: site yöneticiniz, istenen etki alanlarını yapılandırana kadar, şirketinizin kullanıcıları VDR 'de oturum açamaz. Şirket dışı kullanıcılar (yani, Konuk kullanıcılar) e-posta/parolalarını kullanarak oturum açabilirler. Yapılandırma birkaç dakika sürer.

### <a name="step-3---configure-the-claimed-domains"></a>3. adım-istenen etki alanlarını yapılandırma

1. Firmex ' de bir site yöneticisi olarak oturum açın.
1. Sol üst köşedeki şirket logonuz ' e tıklayın.
1. **SSO** sekmesini seçin. Sonra **SSO yapılandırması**' nı seçin. Yapılandırmak istediğiniz etki alanına tıklayın.

    ![Talep edilen etki alanları](./media/firmex-vdr-tutorial/edit-sso.png)  

1. BT yöneticinizin aşağıdaki alanları doldurmasını sağlayabilirsiniz. Alanlar, kimlik sağlayıcınızdan alınmalıdır:  

    ![SSO yapılandırması](./media/firmex-vdr-tutorial/SSO-config.png)

    a. **VARLıK kimliği** metin kutusunda, Azure Portal KOPYALADıĞıNıZ **Azure AD tanımlayıcı** değerini yapıştırın.

    b. **Kimlik sağlayıcısı URL 'si** metin kutusunda, Azure Portal kopyaladığınız **oturum açma URL 'si** değerini yapıştırın.

    c. **Ortak anahtar sertifikası**  -kimlik doğrulama amacıyla, bir SAML iletisi veren tarafından dijital olarak imzalanabilir. İletideki imzayı doğrulamak için ileti alıcısı, veren 'e ait olan bilinen bir ortak anahtar kullanır. Benzer şekilde, bir iletiyi şifrelemek için, son alıcıya ait ortak bir şifreleme anahtarı veren tarafından bilinmelidir. Her iki durumda da, imzalama ve şifreleme — güvenilen ortak anahtarların önceden paylaşılması gerekir.  Bu, **Federasyon meta VERI XML** 'Sinden alınan **X509Certificate** 'dir

    d. SSO yapılandırmasını gerçekleştirmek için **Kaydet** ' e tıklayın. Değişiklikler hemen etkili olur.

1. Bu sırada, siteniz için SSO etkinleştirilmiştir.

### <a name="create-firmex-vdr-test-user"></a>Firmex VDR test kullanıcısı oluşturma

Bu bölümde, Firmex 'de B. Simon adlı bir Kullanıcı oluşturacaksınız. Firmex platformunda kullanıcıları eklemek için [Firmex destek ekibi](mailto:support@firmex.com) ile çalışın. Çoklu oturum açma kullanılmadan önce kullanıcıların oluşturulması ve etkinleştirilmesi gerekir.

## <a name="test-sso"></a>Test SSO 'SU

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim panelinde Firmex VDR kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız Firmex VDR ' de otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

- [ SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Firmex VDR 'yi deneyin](https://aad.portal.azure.com/)

- [Microsoft Cloud App Security oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [Firmex 'yi gelişmiş görünürlük ve denetimlerle koruma](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)
