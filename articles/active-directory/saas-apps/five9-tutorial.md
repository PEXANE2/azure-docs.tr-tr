---
title: 'Öğretici: Five9 Plus Adaptörü (CTI, İletişim Merkezi Aracıları) ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Five9 Plus Adaptörü (CTI, İletişim Merkezi Aracıları) arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 88dc82ab-be0b-4017-8335-c47d00775d7b
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/04/2019
ms.author: jeedes
ms.openlocfilehash: dd4eab2d8e877cac468097c0a6ed6aa3cfb750b9
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "67102462"
---
# <a name="tutorial-azure-active-directory-integration-with-five9-plus-adapter-cti-contact-center-agents"></a>Öğretici: Five9 Plus Adaptörü (CTI, İletişim Merkezi Aracıları) ile Azure Active Directory entegrasyonu

Bu eğitimde, Five9 Plus Adaptör'ü (CTI, İletişim Merkezi Aracıları) ile Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
Five9 Plus Adaptör'ü (CTI, İletişim Merkezi Aracıları) Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* Five9 Plus Adaptörüne (CTI, İletişim Merkezi Aracıları) erişimi olan Azure AD'de kontrol edebilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla Five9 Plus Adaptörü (CTI, İletişim Merkezi Aracıları) (Tek Oturum Açma) ile otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Five9 Plus Adaptörü (CTI, İletişim Merkezi Aracıları) ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız [yoksa, ücretsiz](https://azure.microsoft.com/free/)bir hesap alabilirsiniz.
* Five9 Plus Adaptörü (CTI, İletişim Merkezi Aracıları) tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* Five9 Plus Adaptörü (CTI, İletişim Merkezi Aracıları) **IDP** tarafından başlatılan SSO'ya destek veriyor

## <a name="adding-five9-plus-adapter-cti-contact-center-agents-from-the-gallery"></a>Galeriden Five9 Plus Adaptörü (CTI, İletişim Merkezi Aracıları) ekleme

Five9 Plus Adaptör'ün (CTI, İletişim Merkezi Aracıları) Azure AD'ye entegrasyonunu yapılandırmak için galeriden yönetilen SaaS uygulamaları listenize Five9 Plus Adapter'ı (CTI, İletişim Merkezi Aracıları) eklemeniz gerekir.

**Galeriden Five9 Plus Adaptörü (CTI, İletişim Merkezi Aracıları) eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda, **Beş9 Plus Adaptörü (CTI, İletişim Merkezi Aracıları)** yazın, sonuç panelinden **Five9 Plus Adaptörü 'ni (CTI, İletişim Merkezi Aracıları)** seçin ve ardından uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![Sonuç listesinde Five9 Plus Adaptörü (CTI, İletişim Merkezi Aracıları)](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, **Britta Simon**adlı bir test kullanıcısına dayalı olarak Azure AD tek oturum açma işlemini Five9 Plus Adaptörü (CTI, İletişim Merkezi Aracıları) ile yapılandırıp test emmiş oluyorsunuz.
Tek oturum açmanın işe yaraması için, Bir Azure AD kullanıcısı ile Five9 Plus Adaptörü'ndeki (CTI, İletişim Merkezi Aracıları) ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Azure AD oturum açma işlemlerini Five9 Plus Adaptör (CTI, İletişim Merkezi Aracıları) ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. **[Beş9 Plus Bağdaştırıcısını (CTI, İletişim Merkezi Aracıları) Tek Oturum Açma](#configure-five9-plus-adapter-cti-contact-center-agents-single-sign-on)** - uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için yapılandırın.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Five9 Plus Adaptörü (CTI, İletişim Merkezi Aracıları) test kullanıcısı oluşturun](#create-five9-plus-adapter-cti-contact-center-agents-test-user)** - kullanıcının Azure AD gösterimine bağlı Five9 Plus Adaptörü 'nde (CTI, İletişim Merkezi Aracıları) Britta Simon'ın bir örneğine sahip olun.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturum açma işlemlerini Five9 Plus Adaptörü (CTI, İletişim Merkezi Aracıları) ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında,](https://portal.azure.com/) **Five9 Plus Adaptörü (CTI, İletişim Merkezi Aracıları)** uygulama tümleştirme sayfasında **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. **SAML sayfasıyla Tek Oturum Açma'da** aşağıdaki adımları gerçekleştirin:

    ![Five9 Plus Adaptörü (CTI, İletişim Merkezi Aracıları) Etki Alanı ve URL'ler tek oturum açma bilgileri](common/idp-intiated.png)

    a. **Tanımlayıcı** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:
    
    |    Ortam      |       URL'si      |
    | :-- | :-- |
    | "Microsoft Dynamics CRM için Five9 Plus Adaptörü" için | `https://app.five9.com/appsvcs/saml/metadata/alias/msdc` |
    | "Zendesk için Five9 Plus Adaptörü" için | `https://app.five9.com/appsvcs/saml/metadata/alias/zd` |
    | "Agent Desktop Toolkit için Five9 Plus Adaptörü" için | `https://app.five9.com/appsvcs/saml/metadata/alias/adt` |

    b. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:

    |      Ortam     |      URL'si      |
    | :--                  | :--           |
    | "Microsoft Dynamics CRM için Five9 Plus Adaptörü" için | `https://app.five9.com/appsvcs/saml/SSO/alias/msdc` |
    | "Zendesk için Five9 Plus Adaptörü" için | `https://app.five9.com/appsvcs/saml/SSO/alias/zd` |
    | "Agent Desktop Toolkit için Five9 Plus Adaptörü" için | `https://app.five9.com/appsvcs/saml/SSO/alias/adt` |

6. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, sertifikayı **(Base64)** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

7. **Five9 Plus Adaptörü (CTI, İletişim Merkezi Aracıları)** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-five9-plus-adapter-cti-contact-center-agents-single-sign-on"></a>Beş9 Plus Adaptörü (CTI, İletişim Merkezi Aracıları) Tek Oturum Açma

1. **Five9 Plus Adaptörü (CTI, İletişim Merkezi Aracıları)** tarafında tek oturum açma yapılandırmak için, indirilen **Sertifikayı (Base64)** ve uygun kopyalanmış URL'yi [Five9 Plus Adaptörü'ne (CTI, İletişim Merkezi Aracıları) destek ekibine](https://www.five9.com/about/contact)göndermeniz gerekir. Ayrıca, SSO'yu daha da yapılandırmak için lütfen bağdaştırıcıya göre aşağıdaki adımları izleyin:

    a. "Temsilci Masaüstü Araç Seti için Five9 Plus Adaptörü" Yönetici Kılavuzu:[https://webapps.five9.com/assets/files/for_customers/documentation/integrations/agent-desktop-toolkit/plus-agent-desktop-toolkit-administrators-guide.pdf](https://webapps.five9.com/assets/files/for_customers/documentation/integrations/agent-desktop-toolkit/plus-agent-desktop-toolkit-administrators-guide.pdf)
    
    b. "Microsoft Dynamics CRM için Five9 Plus Adaptörü" Yönetici Kılavuzu:[https://webapps.five9.com/assets/files/for_customers/documentation/integrations/microsoft/microsoft-administrators-guide.pdf](https://webapps.five9.com/assets/files/for_customers/documentation/integrations/microsoft/microsoft-administrators-guide.pdf)
    
    c. "Zendesk için Five9 Plus Adaptör" Yönetici Kılavuzu:[https://webapps.five9.com/assets/files/for_customers/documentation/integrations/zendesk/zendesk-plus-administrators-guide.pdf](https://webapps.five9.com/assets/files/for_customers/documentation/integrations/zendesk/zendesk-plus-administrators-guide.pdf)

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Azure portalında Britta Simon adında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede **Azure Etkin Dizini'ni**seçin, **Kullanıcılar'ı**seçin ve ardından **Tüm Kullanıcıları**seçin.

    !["Kullanıcılar ve gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Ekranın üst kısmında **Yeni kullanıcı** yı seçin.

    ![Yeni kullanıcı Düğmesi](common/new-user.png)

3. Kullanıcı özelliklerinde aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanında **BrittaSimon**girin.
  
    b. Kullanıcı **adı** alanı `brittasimon@yourcompanydomain.extension`türünde. Örneğin, BrittaSimon@contoso.com

    c. Parola onay kutusunu **göster'i** seçin ve ardından Parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur'u**tıklatın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Britta Simon'ın Five9 Plus Adaptörüne (CTI, İletişim Merkezi Aracıları) erişim sağlayarak Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin, ardından **Five9 Plus Adaptör'ü (CTI, İletişim Merkezi Aracıları)** seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Five9 Plus Adaptörü 'ni (CTI, İletişim Merkezi Aracıları)** seçin.

    ![Uygulamalar listesindeki Five9 Plus Adaptörü (CTI, İletişim Merkezi Aracıları) bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-five9-plus-adapter-cti-contact-center-agents-test-user"></a>Five9 Plus Adaptörü (CTI, İletişim Merkezi Aracıları) test kullanıcısı oluşturun

Bu bölümde, Five9 Plus Adaptörü (CTI, İletişim Merkezi Aracıları) britta Simon adlı bir kullanıcı oluşturun. Five9 [Plus Adaptörü (CTI, İletişim Merkezi Aracıları) destek ekibiyle](https://www.five9.com/about/contact) çalışarak kullanıcıları Five9 Plus Adaptör (CTI, Contact Center Agents) platformuna ekleyin. Tek oturum açmadan önce kullanıcılar oluşturulmalı ve etkinleştirilmelidir. 

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Panelindeki Five9 Plus Adaptörü'ni (CTI, İletişim Merkezi Aracıları döşemesi) tıklattığınızda, SSO'yu kurduğunuz Five9 Plus Adaptöründe (CTI, İletişim Merkezi Aracıları) otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

