---
title: 'Öğretici: Sectigo Sertifika Yöneticisi ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Sectigo Sertifika Yöneticisi arasında tek oturum açma işlemlerini nasıl yapılandırıştırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 62cd6987-3373-4b58-b1ff-589f4a3d70a9
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/15/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0447a8dd464363ae7e076dde2520565005d7c0a5
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "67588245"
---
# <a name="tutorial-azure-active-directory-integration-with-sectigo-certificate-manager"></a>Öğretici: Sectigo Sertifika Yöneticisi ile Azure Active Directory entegrasyonu

Bu eğitimde, Sectigo Sertifika Yöneticisi'ni Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.

Sectigo Sertifika Yöneticisi'ni Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* Sectigo Sertifika Yöneticisi'ne kimlerin erişebileceğini denetlemek için Azure AD'yi kullanabilirsiniz.
* Kullanıcılar, Azure AD hesaplarıyla (tek oturum açma) Sectigo Sertifika Yöneticisi'nde otomatik olarak oturum açabilir.
* Hesaplarınızı tek bir merkezi konumda, Yani Azure portalında yönetebilirsiniz.

Azure AD ile hizmet olarak yazılım (SaaS) uygulama tümleştirmesi hakkında daha fazla bilgi için [Azure Active Directory'deki uygulamalarda tek oturum açma'ya](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Sectigo Sertifika Yöneticisi ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure AD aboneliğiniz yoksa, başlamadan önce ücretsiz bir [hesap](https://azure.microsoft.com/free/) oluşturun.
* Sectigo Sertifika Yöneticisi aboneliği tek oturum açma özelliğine sahip.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayın ve Sectigo Sertifika Yöneticisi'ni Azure AD ile tümleştirirsiniz.

Sectigo Sertifika Yöneticisi aşağıdaki özellikleri destekler:

* **SP tarafından başlatılan tek oturum açma**
* **IDP tarafından başlatılan tek oturum açma**

## <a name="add-sectigo-certificate-manager-in-the-azure-portal"></a>Azure portalına Sectigo Sertifika Yöneticisi ekleme

Sectigo Sertifika Yöneticisi'ni Azure AD ile tümleştirmek için, yönetilen SaaS uygulamaları listenize Sectigo Sertifika Yöneticisi'ni eklemeniz gerekir.

1. [Azure portalında](https://portal.azure.com)oturum açın.

1. Sol menüde **Azure Etkin Dizin'i**seçin.

    ![Azure Etkin Dizin seçeneği](common/select-azuread.png)

1. **Kurumsal uygulamaları** > seçin**Tüm uygulamalar**.

    ![Kurumsal uygulamalar bölmesi](common/enterprise-applications.png)

1. Uygulama eklemek için **Yeni uygulama'yı**seçin.

    ![Yeni uygulama seçeneği](common/add-new-app.png)

1. Arama kutusuna **Sectigo Sertifika Yöneticisi'ni**girin. Arama sonuçlarında **Sectigo Sertifika Yöneticisi'ni**seçin ve sonra **Ekle'yi**seçin.

    ![Sonuç listesinde Sectigo Sertifika Yöneticisi](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Azure AD tek oturum açma işlemini **Britta Simon**adlı bir test kullanıcısına göre Sectigo Sertifika Yöneticisi ile yapılandırıp test esinizsiniz. Tek oturum açmanın çalışması için, Bir Azure AD kullanıcısı ile Sectigo Sertifika Yöneticisi'ndeki ilgili kullanıcı arasında bağlantılı bir ilişki kurmanız gerekir.

Azure AD oturumaçma işlemlerini Sectigo Sertifika Yöneticisi ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

| Görev | Açıklama |
| --- | --- |
| **[Azure AD'yi tek oturum açma yapılandırma](#configure-azure-ad-single-sign-on)** | Kullanıcılarınızın bu özelliği kullanmasını sağlar. |
| **[Sectigo Sertifika Yöneticisi'ni tek oturum açma yapılandırma](#configure-sectigo-certificate-manager-single-sign-on)** | Uygulamadaki tek oturum açma ayarlarını yapılandırır. |
| **[Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)** | Britta Simon adlı bir kullanıcı için Azure AD tek oturum açma testlerini. |
| **[Azure AD test kullanıcısını atama](#assign-the-azure-ad-test-user)** | Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlar. |
| **[Sectigo Sertifika Yöneticisi test kullanıcısı oluşturma](#create-a-sectigo-certificate-manager-test-user)** | Sectigo Sertifika Yöneticisi'nde Britta Simon'ın, kullanıcının Azure AD gösterimine bağlı bir örneğini oluşturur. |
| **[Çoklu oturum açma testi](#test-single-sign-on)** | Yapılandırmanın çalıştığını doğrular. |

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Sectigo Sertifika Yöneticisi ile Azure AD oturum açma işlemlerini yapılandırırsınız.

1. Azure [portalında,](https://portal.azure.com/) **Sectigo Sertifika Yöneticisi** uygulama tümleştirme bölmesinde, **Tek oturum açma'yı**seçin.

    ![Tek oturum açma seçeneğini yapılandırma](common/select-sso.png)

1. Tek **oturum açma yöntemi** bölmesinde, tek oturum açmayı etkinleştirmek için SAML veya **SAML/WS-Fed** modunu seçin. **SAML**

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

1. **SAML bölmeli Tek Oturum Açma'da** **Temel SAML Yapılandırma** bölmesini açmak için **Düzenleme** (kalem simgesi) seçeneğini belirleyin.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırma** bölmesinde, *IDP tarafından başlatılan modu*yapılandırmak için aşağıdaki adımları tamamlayın:

    1. **Tanımlayıcı** kutusuna aşağıdaki URL'lerden birini girin:
       * https:\//cert-manager.com/shibboleth
       * https:\//hard.cert-manager.com/shibboleth

    1. **Yanıtla URL** kutusuna aşağıdaki URL'lerden birini girin:
        * https:\//cert-manager.com/Shibboleth.sso/SAML2/POST
        * https:\//hard.cert-manager.com/Shibboleth.sso/SAML2/POST

    1. **Ek URL'ler Ayarla'yı**seçin.

    1. **Röle Durumu** kutusuna, aşağıdaki URL'lerden birini girin:
       * https:\//cert-manager.com/customer/SSLSupport/idp
       * https:\//hard.cert-manager.com/customer/SSLSupport/idp

    ![Sectigo Sertifika Yöneticisi etki alanı ve URL'ler tek oturum açma bilgileri](common/idp-relay.png)

1.  Uygulamayı SP tarafından *başlatılan modda*yapılandırmak için aşağıdaki adımları tamamlayın:

    * **URL'de Oturum Aç** kutusuna şu URL'lerden birini girin:
      * https:\//cert-manager.com/Shibboleth.sso/Login
      * https:\//hard.cert-manager.com/Shibboleth.sso/Login

      ![Sectigo Sertifika Yöneticisi etki alanı ve URL'ler tek oturum açma bilgileri](common/both-signonurl.png)

1. **SAML bölmeli Tek Oturum** Açma'da, **SAML İmza Sertifikası** bölümünde Sertifikanın yanında **İndir'i (Base64)** seçin. **Download** Gereksinimlerinize göre bir indirme seçeneği seçin. Sertifikayı bilgisayarınıza kaydedin.

    ![Sertifika (Base64) indirme seçeneği](common/certificatebase64.png)

1. **Sectigo Sertifika Yöneticisi'ni Ayarla** bölümünde, gereksinimlerinize göre aşağıdaki URL'leri kopyalayın:

    * Oturum Açma URL’si
    * Azure AD Tanımlayıcısı
    * Giriş URL'si

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

### <a name="configure-sectigo-certificate-manager-single-sign-on"></a>Sectigo Sertifika Yöneticisi'ni tek oturum açma yapılandırma

Sectigo Sertifika Yöneticisi tarafında tek oturum açma yapılandırmak için, indirilen Sertifika (Base64) dosyasını ve Azure portalından kopyaladığınız ilgili URL'leri [Sectigo Sertifika Yöneticisi destek ekibine](https://sectigo.com/support)gönderin. Sectigo Sertifika Yöneticisi destek ekibi, SAML tek oturum açma bağlantısının her iki tarafta da düzgün bir şekilde ayarlandığından emin olmak için onlara gönderdiğiniz bilgileri kullanır.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma 

Bu bölümde, Azure portalında Britta Simon adında bir test kullanıcısı oluşturursunuz.

1. Azure portalında Azure **Etkin Dizin** > **Kullanıcıları** > Tüm**kullanıcıları**seçin.

    ![Kullanıcılar ve Tüm Kullanıcı Seçenekleri](common/users.png)

1. **Yeni kullanıcıyı**seçin.

    ![Yeni kullanıcı seçeneği](common/new-user.png)

1. **Kullanıcı** bölmesinde aşağıdaki adımları tamamlayın:

    1. **Ad** **kutusuna BrittaSimon**girin.
  
    1. Kullanıcı **adı** kutusuna, **şirketinizin\@\<etki alanı>\< brittasimon girin. uzantısı\>**. Örneğin, **brittasimon\@contoso.com.**

    1. **Parolayı göster** onay kutusunu seçin. **Parola** kutusunda görüntülenen değeri yazın.

    1. **Oluştur'u**seçin.

    ![Kullanıcı bölmesi](common/user-properties.png)

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Britta Simon'a Sectigo Sertifika Yöneticisi'ne erişim izni vererek Azure tek oturum açma işlemini kullanabiliyor.

1. Azure portalında Kurumsal **uygulamaları** > seçin**Tüm uygulamalar** > **Sectigo Sertifika Yöneticisi.**

    ![Kurumsal uygulamalar bölmesi](common/enterprise-applications.png)

1. Başvuru listesinde **Sectigo Sertifika Yöneticisi'ni**seçin.

    ![Uygulama listesinde Sectigo Sertifika Yöneticisi](common/all-applications.png)

1. **Menüde, Kullanıcıları ve grupları**seçin.

    ![Kullanıcılar ve gruplar seçeneği](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin. Ardından, **Atama Ekle** bölmesinde, Kullanıcılar **ve gruplar**seçin.

    ![Atama ekle bölmesi](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** bölmesinde, kullanıcı listesinde **Britta Simon'ı** seçin. **Seç**’i seçin.

1. SAML iddiasında bir rol değeri bekliyorsanız, **Select rol** bölmesinde, listeden kullanıcı için ilgili rolü seçin. **Seç**’i seçin.

1. Atama **Ekle** bölmesinde **Atama'yı**seçin.

### <a name="create-a-sectigo-certificate-manager-test-user"></a>Sectigo Sertifika Yöneticisi test kullanıcısı oluşturma

Bu bölümde, Sectigo Sertifika Yöneticisi Britta Simon adında bir kullanıcı oluşturun. Kullanıcıyı Sectigo Sertifika Yöneticisi platformuna eklemek için [Sectigo Sertifika Yöneticisi destek ekibiyle](https://sectigo.com/support) birlikte çalışın. Tek oturum açmadan önce kullanıcılar oluşturulmalı ve etkinleştirilmelidir.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, Azure AD tek oturum açma yapılandırmanızı Uygulamalarım portalını kullanarak test edeyim.

Tek oturum açtıktan sonra, Uygulamalarım portalında **Sectigo Sertifika Yöneticisi'ni** seçtiğinizde, otomatik olarak Sectigo Sertifika Yöneticisi ile oturum açarsınız. Uygulamalarım portalı hakkında daha fazla bilgi [için, Uygulamalarım portalındaki Access'e](../user-help/my-apps-portal-end-user-access.md)bakın ve uygulamaları kullanın.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinmek için şu makaleleri inceleyin:

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme yle ilgili öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)
- [Azure Active Directory'deki uygulamalarda tek oturum açma](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)
- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)


