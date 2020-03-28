---
title: 'Öğretici: Percolate ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Bu eğitimde, Azure Active Directory ve Percolate arasında tek oturum açma yı nasıl yapılandıracağınız öğrenilir.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 355f9659-b378-44c9-aa88-236e9b529a53
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/01/2019
ms.author: jeedes
ms.openlocfilehash: a6c1f893757baf1e6c85420b31997a5073cff684
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "67094608"
---
# <a name="tutorial-azure-active-directory-integration-with-percolate"></a>Öğretici: Percolate ile Azure Active Directory entegrasyonu

Bu eğitimde, Percolate'ı Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.

Bu tümleştirme şu avantajları sağlar:

* Percolate'a kimlerin erişebileceğini denetlemek için Azure AD'yi kullanabilirsiniz.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla Percolate'da (tek oturum açma) otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz: Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek [için Azure Active Directory'deki uygulamalarda tek](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)oturum açma'ya bakın.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Percolate ile yapılandırmak için şunları yapmanız gerekir:

* Azure AD aboneliği. Azure REKLAM ortamınız [yoksa, ücretsiz](https://azure.microsoft.com/free/)bir hesap alabilirsiniz.
* Tek oturum açma özelliği etkin olan bir Percolate aboneliği.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD oturum açma işlemlerini bir test ortamında yapılandıracak ve sınayacaksınız.

* Percolate, SP tarafından başlatılan ve IdP tarafından başlatılan SSO'ları destekler.

## <a name="add-percolate-from-the-gallery"></a>Galeriden Percolate ekle

Percolate'ın Azure AD'ye entegrasyonunu yapılandırmak için, percolate'ı galeriden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com), sol bölmede, **Azure Etkin Dizini'ni**seçin:

    ![Azure Active Directory'yi seçin](common/select-azuread.png)

2. Kurumsal **uygulamalara** > git**Tüm uygulamalar:**

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Uygulama eklemek için pencerenin üst kısmında **Yeni uygulama'yı** seçin:

    ![Yeni uygulama seçin](common/add-new-app.png)

4. Arama kutusuna **Percolate**girin. Arama sonuçlarında **Percolate'ı** seçin ve ardından **Ekle'yi**seçin.

     ![Arama sonuçları](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Britta Simon adlı bir test kullanıcısı kullanarak Azure AD'nin Percolate ile tek oturum açma işlemini yapılandıracak ve sınayacaksınız.
Tek oturum açmayı etkinleştirmek için, Bir Azure REKLAM kullanıcısı ile Percolate'daki ilgili kullanıcı arasında bir ilişki kurmanız gerekir.

Azure AD oturumlarını Percolate ile yapılandırmak ve test etmek için şu adımları tamamlamanız gerekir:

1. Azure AD oturumunu, kullanıcılarınız için özelliği etkinleştirmek için **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. Uygulama tarafında **[Percolate tek oturum](#configure-percolate-single-sign-on)** açArak yapılandırın.
3. Azure AD tek oturum açma'yı test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Azure AD test kullanıcısını, kullanıcı için Azure AD oturum açmayı etkinleştirmek için **[atayın.](#assign-the-azure-ad-test-user)**
5. Kullanıcının Azure AD gösterimine bağlı **[bir Percolate test kullanıcısı oluşturun.](#create-a-percolate-test-user)**
6. Yapılandırmanın çalıştığını doğrulamak için **[tek oturum](#test-single-sign-on)** açma yı test edin.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturumaçmayı Percolate ile yapılandırmak için şu adımları izleyin:

1. Azure [portalında,](https://portal.azure.com/) **Percolate** uygulama tümleştirme sayfasında **Tek oturum açma**seçeneğini belirleyin:

    ![Tek oturum açma'yı seçin](common/select-sso.png)

2. Tek **oturum** açma yöntemi iletişim kutusunu seç'te, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin:

    ![Tek bir oturum açma yöntemi seçin](common/select-saml-option.png)

3. **SAML** ile Tek Oturum Açma'da, **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini seçin:

    ![Düzenle simgesi](common/edit-urls.png)

4. Temel **SAML Yapılandırma** iletişim kutusunda, uygulamayı IdP tarafından başlatılan modda yapılandırmak için herhangi bir işlem yapmanız gerekmez. Uygulama zaten Azure ile entegre edilmiştir.

    ![Percolate Domain ve URL'ler tek oturum açma bilgileri](common/preintegrated.png)

5. Uygulamayı SP tarafından başlatılan modda yapılandırmak istiyorsanız, **ek URL'ler** ayarla'yı seçin ve **URL'de Oturum Aç** kutusuna girin: **https://percolate.com/app/login**

   ![Percolate Domain ve URL'ler tek oturum açma bilgileri](common/metadata-upload-additional-signon.png)
6. **SAML** ile Tek Oturum Açma sayfasında, **SAML İmza Sertifikası** bölümünde, Uygulama Federasyonu **Metaveri Url'sini**kopyalamak için **Kopyala** simgesini seçin. Bu URL'yi kaydedin.

    ![Uygulama Federasyonu Meta veri URL'sini kopyalama](common/copy-metadataurl.png)

7. **Percolate'ı Ayarla** bölümünde, gereksinimlerinize göre uygun URL'leri kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    1. **Giriş URL'si**.

    1. **Azure AD Tanımlayıcısı**.

    1. **Çıkış URL'si**.

### <a name="configure-percolate-single-sign-on"></a>Percolate tek oturum açma yı yapılandır

1. Yeni bir web tarayıcısı penceresinde, percolate'da yönetici olarak oturum açın.

2. Ana sayfanın sol tarafında **Ayarlar'ı**seçin:
    
    ![Ayarlar'ı seçin](./media/percolate-tutorial/configure01.png)

3. Sol bölmede, **Organizasyon**altında **SSO'yu** seçin:

    ![Organizasyon altında SSO'u seçin](./media/percolate-tutorial/configure02.png)

    1. Giriş **URL** kutusuna Azure portalından kopyaladığınız **Giriş URL** değerini yapıştırın.

    1. Varlık **Kimliği** kutusuna, Azure portalından kopyaladığınız **Azure AD Tanımlayıcı** değerini yapıştırın.

    1. Not Defteri'nde, Azure portalından indirdiğiniz base-64 kodlu sertifikayı açın. İçeriğini kopyalayın ve **x509 sertifikalar** kutusuna yapıştırın.

    1. **E-posta öznitelik** **kutusuna, e-posta adresini**girin.

    1. **Kimlik sağlayıcı meta data URL** kutusu isteğe bağlı bir alandır. Azure portalından bir **Uygulama Federasyonu Meta veri url'sini** kopyaladıysanız, bu kutuya yapıştırabilirsiniz.

    1. **AuthNRequests imzalanmalı mı?** **No**

    1. **SSO otomatik sağlama** yı etkinleştir listesinde **No'yu**seçin.

    1. **Kaydet'i**seçin.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, Azure portalında Britta Simon adında bir test kullanıcısı oluşturursunuz.

1. Azure portalında, sol bölmede **Azure Etkin Dizini'ni** seçin, **Kullanıcılar'ı**seçin ve ardından **Tüm Kullanıcıları**seçin:

    ![Tüm kullanıcılar'ı seçin](common/users.png)

2. Ekranın üst kısmında **Yeni kullanıcı** seçin:

    ![Yeni kullanıcıyı seçin](common/new-user.png)

3. **Kullanıcı** iletişim kutusunda aşağıdaki adımları izleyin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    1. **Ad** **kutusuna BrittaSimon**girin.
  
    1. Kullanıcı **adı** kutusuna, **\<şirketinizin alan\< BrittaSimon@> girin. uzantısı>**. (Örneğin, BrittaSimon@contoso.com.)

    1. **Parolayı Göster'i**seçin ve ardından **Parola** kutusundaki değeri yazın.

    1. **Oluştur'u**seçin.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Britta Simon'ın Percolate'a erişimini sağlayarak Azure AD oturumunu tek oturum açma işlemini kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal uygulamaları**seçin, **Tüm uygulamaları**seçin ve ardından **Percolate'ı**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Percolate'ı**seçin.

    ![Başvuru listesi](common/all-applications.png)

3. Sol **bölmede, Kullanıcıları ve grupları**seçin:

    ![Kullanıcı ve gruplar'ı seçin](common/users-groups-blade.png)

4. **Kullanıcı Ekle'yi**seçin ve ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı ve gruplar'ı seçin](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, kullanıcı listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında, **Rolü Seç** iletişim kutusunda bir rol değeri bekliyorsanız, listeden kullanıcı için uygun rolü seçin. Ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atama'yı**seçin.

### <a name="create-a-percolate-test-user"></a>Percolate test kullanıcısı oluşturma

Azure AD kullanıcılarının Percolate'da oturum açmasını sağlamak için bunları Percolate'a eklemeniz gerekir. Bunları el ile eklemeniz gerekir.

Bir kullanıcı hesabı oluşturmak için şu adımları izleyin:

1. Percolate'da yönetici olarak oturum açın.

2. Sol bölmede, **Kuruluş**altındaki **Kullanıcıları** seçin. **Yeni kullanıcılar**seçin:

    ![Yeni kullanıcılar seçin](./media/percolate-tutorial/configure03.png)

3. Kullanıcı **Oluştur** sayfasında aşağıdaki adımları izleyin.

    ![Kullanıcılar sayfası oluşturma](./media/percolate-tutorial/configure04.png)

    1. **E-posta** kutusuna kullanıcının e-posta adresini girin. Örneğin, brittasimon@contoso.com.

    1. Tam **ad** kutusuna kullanıcının adını girin. Örneğin, **Brittasimon.**

    1. **Kullanıcıları Oluştur'u**seçin.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Artık Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı test etmeniz gerekir.

Erişim Paneli'ndeki Percolate karo'yu seçtiğinizde, SSO'yu kurduğunuz Percolate örneğinde otomatik olarak oturum açmalısınız. Daha fazla bilgi için [Access'e bakın ve Uygulamalarım portalındaki uygulamaları kullanın.](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme öğreticileri](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)