---
title: 'Öğretici: Five9 Plus bağdaştırıcısıyla Azure Active Directory tümleştirme (CTı, kişi merkezi aracıları) | Microsoft Docs'
description: Azure Active Directory ve Five9 Plus bağdaştırıcısı (CTı, kişi merkezi aracıları) arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 04/04/2019
ms.author: jeedes
ms.openlocfilehash: 2ab03aecf8226662eeb03a5bb3f8adc85013375d
ms.sourcegitcommit: 023d10b4127f50f301995d44f2b4499cbcffb8fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88555068"
---
# <a name="tutorial-azure-active-directory-integration-with-five9-plus-adapter-cti-contact-center-agents"></a>Öğretici: Five9 Plus bağdaştırıcısıyla Azure Active Directory tümleştirme (CTı, kişi merkezi aracıları)

Bu öğreticide, Five9 Plus bağdaştırıcısını (CTı, Iletişim merkezi aracıları) Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz.
Azure AD ile Five9 Plus bağdaştırıcısını (CTı, Ilgili kişi merkezi aracıları) tümleştirmek aşağıdaki avantajları sağlar:

* Azure AD 'de Five9 Plus bağdaştırıcısına erişimi olan (CTı, kişi merkezi aracıları) denetim yapabilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla Five9 Plus bağdaştırıcısı (CTı, Ilgili merkezi aracıları) (çoklu oturum açma) ile otomatik olarak oturum açmasını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz-Azure portal.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek istiyorsanız, bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Five9 Plus bağdaştırıcısı (CTı, kişi merkezi aracıları) ile yapılandırmak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Azure AD ortamınız yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Five9 Plus bağdaştırıcısı (CTı, kişi merkezi aracıları) çoklu oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açmayı bir test ortamında yapılandırıp test edersiniz.

* Five9 Plus bağdaştırıcısı (CTı, kişi merkezi aracıları) **IDP** tarafından başlatılan SSO 'yu destekler

## <a name="adding-five9-plus-adapter-cti-contact-center-agents-from-the-gallery"></a>Galeriden Five9 Plus bağdaştırıcısı (CTı, kişi merkezi aracıları) ekleme

Five9 Plus bağdaştırıcısının (CTı, Ilgili kişi merkezi aracıları) tümleştirmesini Azure AD ile yapılandırmak için, galerideki Five9 Plus bağdaştırıcısı (CTı, Iletişim merkezi aracıları) öğesini yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Galeriden Five9 Plus bağdaştırıcısı (CTı, kişi merkezi aracıları) eklemek için aşağıdaki adımları uygulayın:**

1. **[Azure Portal](https://portal.azure.com)** sol gezinti panelinde **Azure Active Directory** simgesine tıklayın.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar** seçeneğini belirleyin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için, iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesine tıklayın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Five9 Plus bağdaştırıcı (CTI, kişi merkezi aracıları)** yazın, sonuç panelinden **Five9 Plus bağdaştırıcısı (CTI, kişi merkezi aracıları)** öğesini seçin ve ardından uygulamayı eklemek için **Ekle** düğmesine tıklayın.

     ![Sonuç listesindeki Five9 Plus bağdaştırıcısı (CTı, kişi merkezi aracıları)](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma ve test etme

Bu bölümde, Azure AD çoklu oturum açmayı, **Britta Simon**adlı bir test kullanıcısına göre Five9 Plus BAĞDAŞTıRıCıSı (CTI, kişi merkezi aracıları) ile yapılandırıp test edersiniz.
Çoklu oturum açma için, bir Azure AD kullanıcısı ve Five9 Plus bağdaştırıcısı (CTı, kişi merkezi aracıları) içindeki ilgili Kullanıcı arasındaki bağlantı ilişkisinin kurulması gerekir.

Azure AD çoklu oturum açmayı Five9 Plus bağdaştırıcısı (CTı, kişi merkezi aracıları) ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını gerçekleştirmeniz gerekir:

1. **[Azure AD çoklu oturum açma özelliğini yapılandırarak](#configure-azure-ad-single-sign-on)** kullanıcılarınızın bu özelliği kullanmasına olanak sağlayın.
2. Uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için **[Five9 Plus bağdaştırıcısını (CTI, kişi merkezi aracıları) çoklu oturum açma 'Yı yapılandırın](#configure-five9-plus-adapter-cti-contact-center-agents-single-sign-on)** .
3. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -Britta Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
4. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanarak Britta Simon 'u etkinleştirin.
5. Kullanıcının Azure AD gösterimine bağlı olan Five9 Plus bağdaştırıcısında (CTı, Ilgili merkezi aracılarında) Britta Simon 'a sahip olmak için **[Five9 Plus bağdaştırıcısı (CTI, kişi merkezi aracıları) oluşturun](#create-five9-plus-adapter-cti-contact-center-agents-test-user)** .
6. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[Çoklu oturum açmayı sınayın](#test-single-sign-on)** .

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure portal Azure AD çoklu oturum açma özelliğini etkinleştirirsiniz.

Azure AD çoklu oturum açmayı Five9 Plus bağdaştırıcısı (CTı, kişi merkezi aracıları) ile yapılandırmak için aşağıdaki adımları uygulayın:

1. [Azure Portal](https://portal.azure.com/), **Five9 Plus bağdaştırıcısı (CTI, kişi merkezi aracıları)** uygulama tümleştirmesi sayfasında, **Çoklu oturum açma**' yı seçin.

    ![Çoklu oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Çoklu oturum **açma yöntemi seç** iletişim kutusunda, çoklu oturum açmayı etkinleştirmek için **SAML/WS-Besme** modunu seçin.

    ![Çoklu oturum açma seçme modu](common/select-saml-option.png)

3. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **temel SAML yapılandırması** Iletişim kutusunu açmak için **Düzenle** simgesine tıklayın.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, aşağıdaki adımları uygulayın:

    ![Five9 Plus bağdaştırıcısı (CTı, kişi merkezi aracıları) etki alanı ve URL 'Ler çoklu oturum açma bilgileri](common/idp-intiated.png)

    a. **Tanımlayıcı** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:
    
    |    Ortam      |       URL      |
    | :-- | :-- |
    | "Microsoft Dynamics CRM için Five9 Plus bağdaştırıcısı" için | `https://app.five9.com/appsvcs/saml/metadata/alias/msdc` |
    | "Zendesk için Five9 Plus bağdaştırıcısı" için | `https://app.five9.com/appsvcs/saml/metadata/alias/zd` |
    | "Aracı masaüstü araç seti için Five9 Plus bağdaştırıcısı" | `https://app.five9.com/appsvcs/saml/metadata/alias/adt` |

    b. **Yanıt URL 'si** metin kutusuna aşağıdaki kalıbı kullanarak bir URL yazın:

    |      Ortam     |      URL      |
    | :--                  | :--           |
    | "Microsoft Dynamics CRM için Five9 Plus bağdaştırıcısı" için | `https://app.five9.com/appsvcs/saml/SSO/alias/msdc` |
    | "Zendesk için Five9 Plus bağdaştırıcısı" için | `https://app.five9.com/appsvcs/saml/SSO/alias/zd` |
    | "Aracı masaüstü araç seti için Five9 Plus bağdaştırıcısı" | `https://app.five9.com/appsvcs/saml/SSO/alias/adt` |

6. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde, **sertifika (base64)** ' i gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir** ' e tıklayın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

7. **Five9 Plus bağdaştırıcısı ayarlama (CTI, kişi merkezi aracıları)** bölümünde, uygun URL 'leri gereksiniminize göre kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD tanımlayıcısı

    c. Oturum kapatma URL 'SI

### <a name="configure-five9-plus-adapter-cti-contact-center-agents-single-sign-on"></a>Five9 Plus bağdaştırıcısını yapılandırma (CTı, kişi merkezi aracıları) çoklu oturum açma

1. **Five9 Plus bağdaştırıcısı (CTI, kişi merkezi aracıları)** tarafında çoklu oturum açmayı yapılandırmak için, indirilen **sertifikayı (base64)** ve uygun kopyalanmış URL 'leri [Five9 Plus bağdaştırıcı (CTI, bağlantı merkezi aracıları) destek ekibine](https://www.five9.com/about/contact)göndermeniz gerekir. Ayrıca, SSO 'yu daha fazla yapılandırmak için lütfen bağdaştırıcıya göre aşağıdaki adımları izleyin:

    a. "Aracı masaüstü araç seti için Five9 Plus bağdaştırıcısı" Yönetici Kılavuzu: [https://webapps.five9.com/assets/files/for_customers/documentation/integrations/agent-desktop-toolkit/plus-agent-desktop-toolkit-administrators-guide.pdf](https://webapps.five9.com/assets/files/for_customers/documentation/integrations/agent-desktop-toolkit/plus-agent-desktop-toolkit-administrators-guide.pdf)
    
    b. "Microsoft Dynamics CRM için Five9 Plus bağdaştırıcısı" Yönetici Kılavuzu: [https://webapps.five9.com/assets/files/for_customers/documentation/integrations/microsoft/microsoft-administrators-guide.pdf](https://webapps.five9.com/assets/files/for_customers/documentation/integrations/microsoft/microsoft-administrators-guide.pdf)
    
    c. "Zendesk için Five9 Plus bağdaştırıcısı" Yönetici Kılavuzu: [https://webapps.five9.com/assets/files/for_customers/documentation/integrations/zendesk/zendesk-plus-administrators-guide.pdf](https://webapps.five9.com/assets/files/for_customers/documentation/integrations/zendesk/zendesk-plus-administrators-guide.pdf)

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Azure portal Britta Simon adlı bir test kullanıcısı oluşturmaktır.

1. Azure portal, sol bölmedeki **Azure Active Directory**' i seçin, **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.

    !["Kullanıcılar ve gruplar" ve "tüm kullanıcılar" bağlantıları](common/users.png)

2. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.

    ![Yeni Kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı Özellikleri ' nde aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanına **Brittasıon**girin.
  
    b. **Kullanıcı adı** alanına yazın `brittasimon@yourcompanydomain.extension` . Örneğin, BrittaSimon@contoso.com

    c. **Parolayı göster** onay kutusunu seçin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Five9 Plus bağdaştırıcısına (CTı, kişi merkezi aracıları) erişim izni vererek Azure çoklu oturum açma özelliğini kullanmak için Britta Simon özelliğini etkinleştirirsiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin, **tüm uygulamalar**' ı seçin, ardından **Five9 Plus bağdaştırıcısı ' nı (CTI, kişi merkezi aracıları)** seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Five9 Plus bağdaştırıcısı (CTI, kişi merkezi aracıları)** öğesini seçin.

    ![Uygulamalar listesindeki Five9 Plus bağdaştırıcısı (CTı, kişi merkezi aracıları) bağlantısı](common/all-applications.png)

3. Soldaki menüde **Kullanıcılar ve gruplar**' ı seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. **Kullanıcı Ekle** düğmesine tıklayın, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. **Kullanıcılar ve gruplar** Iletişim kutusunda kullanıcılar listesinde **Britta Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

6. SAML onaylama işlemi içinde herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, listeden Kullanıcı için uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

7. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

### <a name="create-five9-plus-adapter-cti-contact-center-agents-test-user"></a>Five9 Plus bağdaştırıcısı oluşturma (CTı, kişi merkezi aracıları) test kullanıcısı

Bu bölümde, Five9 Plus bağdaştırıcısında (CTı, kişi merkezi aracıları) Britta Simon adlı bir Kullanıcı oluşturacaksınız. Kullanıcıları Five9 Plus bağdaştırıcı (CTı, Ilgili merkezi aracıları) platformunda eklemek için [Five9 Plus bağdaştırıcısı (CTI, kişi merkezi aracıları) destek ekibi](https://www.five9.com/about/contact) ile çalışın. Çoklu oturum açma kullanılmadan önce kullanıcıların oluşturulması ve etkinleştirilmesi gerekir. 

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim panelinde Five9 Plus bağdaştırıcısı (CTı, kişi merkezi aracıları kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız Five9 Plus bağdaştırıcısında (CTı, Ilgili kişi merkezi aracıları) otomatik olarak oturum açmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory Koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

