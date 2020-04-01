---
title: 'Öğretici: Kontiki ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Kontiki arasında tek oturum açma işlemlerini nasıl yapılandıracağınızı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 8d5e5413-da4c-40d8-b1d0-f03ecfef030b
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/14/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: bfb154bce34b2ceda99b82c7ca3534b8a8ee0a1f
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "67098500"
---
# <a name="tutorial-azure-active-directory-integration-with-kontiki"></a>Öğretici: Kontiki ile Azure Active Directory entegrasyonu

Bu eğitimde, Kontiki'yi Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.

Kontiki'yi Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* Kontiki'ye kimlerin erişebileceğini denetlemek için Azure AD'yi kullanabilirsiniz.
* Kullanıcılar Azure AD hesaplarıyla (tek oturum açma) Kontiki'de otomatik olarak oturum açabilir.
* Hesaplarınızı tek bir merkezi konumda, Yani Azure portalında yönetebilirsiniz.

Azure AD ile hizmet olarak yazılım (SaaS) uygulama tümleştirmesi hakkında daha fazla bilgi için [Azure Active Directory'deki uygulamalarda tek oturum açma'ya](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Kontiki ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure AD aboneliğiniz yoksa, başlamadan önce ücretsiz bir [hesap](https://azure.microsoft.com/free/) oluşturun.
* Tek oturum açma özelliğine sahip kontiki aboneliği.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Bir test ortamında Azure AD tek oturum açma işlemlerini yapılandırıp sınayın ve Kontiki'yi Azure AD ile tümleştirirsiniz.

Kontiki aşağıdaki özellikleri destekler:

* **SP tarafından başlatılan tek oturum açma**
* **Tam zamanında kullanıcı sağlama**

## <a name="add-kontiki-in-the-azure-portal"></a>Azure portalına Kontiki ekleme

Kontiki'yi Azure AD ile tümleştirmek için, Kontiki'yi yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. [Azure portalında](https://portal.azure.com)oturum açın.

1. Sol menüde **Azure Etkin Dizin'i**seçin.

    ![Azure Etkin Dizin seçeneği](common/select-azuread.png)

1. **Kurumsal uygulamaları** > seçin**Tüm uygulamalar**.

    ![Kurumsal uygulamalar bölmesi](common/enterprise-applications.png)

1. Uygulama eklemek için **Yeni uygulama'yı**seçin.

    ![Yeni uygulama seçeneği](common/add-new-app.png)

1. Arama kutusuna **Kontiki**girin. Arama sonuçlarında **Kontiki'yi**seçin ve sonra **Ekle'yi**seçin.

    ![Kontiki sonuç listesinde](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Azure AD tek oturum açma işlemini **Britta Simon**adlı bir test kullanıcısına göre yapılandırır ve test etirsiniz. Tek oturum açmanın işe yaraması için, Kontiki'deki bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bağlantılı bir ilişki kurmanız gerekir.

Azure AD oturumlarını Kontiki ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

| Görev | Açıklama |
| --- | --- |
| **[Azure AD'yi tek oturum açma yapılandırma](#configure-azure-ad-single-sign-on)** | Kullanıcılarınızın bu özelliği kullanmasını sağlar. |
| **[Kontiki'yi tek oturum açma yı yapılandır](#configure-kontiki-single-sign-on)** | Uygulamadaki tek oturum açma ayarlarını yapılandırır. |
| **[Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)** | Britta Simon adlı bir kullanıcı için Azure AD tek oturum açma testlerini. |
| **[Azure AD test kullanıcısını atama](#assign-the-azure-ad-test-user)** | Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlar. |
| **[Kontiki test kullanıcısı oluşturma](#create-a-kontiki-test-user)** | Kullanıcının Azure AD gösterimine bağlı olan Kontiki'deki Britta Simon'ın bir örneğini oluşturur. |
| **[Çoklu oturum açma testi](#test-single-sign-on)** | Yapılandırmanın çalıştığını doğrular. |

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Kontiki ile Azure AD oturum açma işlemlerini yapılandırırsınız.

1. Azure [portalında](https://portal.azure.com/) **Kontiki** uygulama tümleştirme bölmesinde **Tek oturum açma'yı**seçin.

    ![Tek oturum açma seçeneğini yapılandırma](common/select-sso.png)

1. Tek **oturum açma yöntemi** bölmesinde, tek oturum açmayı etkinleştirmek için SAML veya **SAML/WS-Fed** modunu seçin. **SAML**

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

1. **SAML bölmeli Tek Oturum Açma'da** **Temel SAML Yapılandırma** bölmesini açmak için **Düzenleme** (kalem simgesi) seçeneğini belirleyin.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırma** bölmesine, **URL** metin kutusunda Oturum Aç kutusuna aşağıdaki deseni içeren bir URL girin:`https://<companyname>.mc.eval.kontiki.com`

    ![Kontiki Domain ve URL'ler tek oturum açma bilgileri](common/sp-signonurl.png)

    > [!NOTE]
    > Kullanılacak doğru değeri elde etmek için [Kontiki Client destek ekibine](https://customersupport.kontiki.com/enterprise/contactsupport.html) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

1. **SAML bölmesiyle Tek Oturum** Açma'da, **SAML İmza Sertifikası** **bölümünde, Federasyon Metadata XML'nin**yanındaki **Karşıdan Yükleme'yi** seçin. Gereksinimlerinize göre bir indirme seçeneği seçin. Sertifikayı bilgisayarınıza kaydedin.

    ![Federasyon Metadata XML sertifika indirme seçeneği](common/metadataxml.png)

1. **Kontiki'yi Ayarla** bölümünde, gereksinimlerinize göre aşağıdaki URL'leri kopyalayın:

    * Oturum Açma URL’si
    * Azure AD Tanımlayıcısı
    * Giriş URL'si

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

### <a name="configure-kontiki-single-sign-on"></a>Kontiki'yi tek oturum açma yı yapılandır

Kontiki tarafında tek oturum açma yapılandırmak için, indirilen Federasyon Metadata XML dosyasını ve Azure portalından kopyaladığınız ilgili URL'leri [Kontiki destek ekibine](https://customersupport.kontiki.com/enterprise/contactsupport.html)gönderin. Kontiki destek ekibi, SAML tek oturum açma bağlantısının her iki tarafta da düzgün bir şekilde ayarlandığından emin olmak için onlara gönderdiğiniz bilgileri kullanır.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma 

Bu bölümde, Azure portalında Britta Simon adında bir test kullanıcısı oluşturursunuz.

1. Azure portalında Azure **Etkin Dizin** > **Kullanıcıları** > Tüm**kullanıcıları**seçin.

    ![Kullanıcılar ve Tüm Kullanıcı Seçenekleri](common/users.png)

1. **Yeni kullanıcıyı**seçin.

    ![Yeni kullanıcı seçeneği](common/new-user.png)

1. **Kullanıcı** bölmesinde aşağıdaki adımları tamamlayın:

    1. **Ad** **kutusuna BrittaSimon**girin.
  
    1. Kullanıcı **adı** kutusuna, **şirketinizin\@\<etki alanı>\< brittasimon girin. uzantısı>**. Örneğin, **brittasimon\@contoso.com.**

    1. **Parolayı göster** onay kutusunu seçin. **Parola** kutusunda görüntülenen değeri yazın.

    1. **Oluştur'u**seçin.

    ![Kullanıcı bölmesi](common/user-properties.png)

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Britta Simon'a Kontiki'ye erişim izni vererek Azure tek oturum açma işlemini kullanabiliyor.

1. Azure portalında **Kurumsal uygulamaları** > seçin**Tüm uygulamalar** > **Kontiki.**

    ![Kurumsal uygulamalar bölmesi](common/enterprise-applications.png)

1. Uygulamalar listesinde **Kontiki'yi**seçin.

    ![Kontiki başvuru listesinde](common/all-applications.png)

1. **Menüde, Kullanıcıları ve grupları**seçin.

    ![Kullanıcılar ve gruplar seçeneği](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin. Ardından, **Atama Ekle** bölmesinde, Kullanıcılar **ve gruplar**seçin.

    ![Atama ekle bölmesi](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** bölmesinde, kullanıcı listesinde **Britta Simon'ı** seçin. **Seç**’i seçin.

1. SAML iddiasında bir rol değeri bekliyorsanız, **Select rol** bölmesinde, listeden kullanıcı için ilgili rolü seçin. **Seç**’i seçin.

1. Atama **Ekle** bölmesinde **Atama'yı**seçin.

### <a name="create-a-kontiki-test-user"></a>Kontiki test kullanıcısı oluşturma

Kontiki'de kullanıcı sağlamayı yapılandırmanız için bir eylem öğesi yoktur. Atanan bir kullanıcı Uygulamalarım portalını kullanarak Kontiki'de oturum açmaya çalıştığında Kontiki, kullanıcının var olup olmadığını denetler. Kullanıcı hesabı bulunmazsa, Kontiki otomatik olarak kullanıcı hesabını oluşturur.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, Azure AD tek oturum açma yapılandırmanızı Uygulamalarım portalını kullanarak test edeyim.

Tek oturum açtıktan sonra, Uygulamalarım portalında **Kontiki'yi** seçtiğinizde, kontiki ile otomatik olarak oturum açarsınız. Uygulamalarım portalı hakkında daha fazla bilgi [için, Uygulamalarım portalındaki Access'e](../user-help/my-apps-portal-end-user-access.md)bakın ve uygulamaları kullanın.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinmek için şu makaleleri inceleyin:

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme yle ilgili öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)
- [Azure Active Directory'deki uygulamalarda tek oturum açma](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)
- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
