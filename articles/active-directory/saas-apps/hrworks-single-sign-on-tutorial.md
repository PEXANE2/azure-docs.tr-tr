---
title: 'Öğretici: Hrçalışmalar çoklu oturum açma ile Azure Active Directory tümleştirme | Microsoft Docs'
description: Azure Active Directory ve Hrçalışmalerde çoklu oturum açma arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: c4c5d434-3f8a-411e-83a5-c3d5276ddc0a
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/25/2019
ms.author: jeedes
ms.openlocfilehash: a36266c14531f935779266829402392dc4a03411
ms.sourcegitcommit: c69c8c5c783db26c19e885f10b94d77ad625d8b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74706007"
---
# <a name="tutorial-azure-active-directory-integration-with-hrworks-single-sign-on"></a>Öğretici: Hrçalışmalar çoklu oturum açma ile tümleştirme Azure Active Directory

Bu öğreticide, Hrçalışmaları çoklu oturum açmayı Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz.
Hrçalışmaları çoklu oturum açmayı Azure AD ile tümleştirmek aşağıdaki avantajları sağlar:

* Hrçalışmalar çoklu oturum açma 'ya erişimi olan Azure AD 'de denetim yapabilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla HR, çoklu oturum açma (çoklu oturum açma) ile otomatik olarak oturum açmasını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz-Azure portal.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek istiyorsanız, bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesini Hrçalışmaçoklu oturum açma ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Bir Azure AD aboneliği. Bir Azure AD ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü edinebilirsiniz
* Hrçalışmaktadır çoklu oturum açma çoklu oturum açma aboneliği

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açmayı bir test ortamında yapılandırıp test edersiniz.

* Hrçalışmalar çoklu oturum açma, **SP** tarafından başlatılan SSO 'yu destekler

## <a name="adding-hrworks-single-sign-on-from-the-gallery"></a>Galeriden Hrçalışmalar çoklu oturum açma ekleme

HR, çoklu oturum açma 'nın Azure AD 'de tümleştirilmesini yapılandırmak için, Galeriden, yönetilen SaaS uygulamaları listenize hrçalışmalardan çoklu oturum açma eklemeniz gerekir.

**Galeriden Hrçalışmalardan çoklu oturum açma eklemek için aşağıdaki adımları uygulayın:**

1. **[Azure Portal](https://portal.azure.com)** sol gezinti panelinde **Azure Active Directory** simgesine tıklayın.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar** seçeneğini belirleyin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için, iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesine tıklayın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Hrçalışmaçoklu oturum açma**yazın, sonuç panelinden **Hrçalışmaçoklu oturum açma** ' yı seçin ve sonra uygulamayı eklemek için düğme **Ekle** ' ye tıklayın.

     ![HR, sonuçlar listesinde çoklu oturum açma](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma ve test etme

Bu bölümde, Azure AD çoklu oturum açmayı, **Britta Simon**adlı bir test kullanıcısına göre HR, çoklu oturum açma ile yapılandırıp test edersiniz.
Çoklu oturum açma için, bir Azure AD kullanıcısı ve HR, tek oturum açma içindeki ilgili Kullanıcı arasındaki bağlantı ilişkisinin kurulması gerekir.

Azure AD çoklu oturum açmayı, Hrçalışmalarla çoklu oturum açma ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını gerçekleştirmeniz gerekir:

1. **[Azure AD çoklu oturum açma özelliğini yapılandırarak](#configure-azure-ad-single-sign-on)** kullanıcılarınızın bu özelliği kullanmasına olanak sağlayın.
2. **[Hrçalışmaları çoklu oturum açma çoklu](#configure-hrworks-single-sign-on-single-sign-on)** oturum açma ayarlarını, uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için yapılandırın.
3. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -Britta Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
4. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanarak Britta Simon 'u etkinleştirin.
5. Hrçalışmalar **[Çoklu oturum açma testi kullanıcısı oluşturun](#create-hrworks-single-sign-on-test-user)** . HRG, kullanıcının Azure AD gösterimine bağlı olan Hrmem çoklu oturum açma bölümünde Britta Simon 'a sahip olacak.
6. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[Çoklu oturum açmayı sınayın](#test-single-sign-on)** .

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure portal Azure AD çoklu oturum açma özelliğini etkinleştirirsiniz.

Azure AD çoklu oturum açmayı HR, çoklu oturum açma ile yapılandırmak için aşağıdaki adımları uygulayın:

1. [Azure Portal](https://portal.azure.com/), **hrçalışmaçoklu oturum açma** uygulama tümleştirmesi sayfasında, **Çoklu oturum**açma ' yı seçin.

    ![Çoklu oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Çoklu oturum **açma yöntemi seç** iletişim kutusunda, çoklu oturum açmayı etkinleştirmek için **SAML/WS-Besme** modunu seçin.

    ![Çoklu oturum açma seçme modu](common/select-saml-option.png)

3. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **temel SAML yapılandırması** Iletişim kutusunu açmak için **Düzenle** simgesine tıklayın.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. **Temel SAML yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Hrçalışmaktadır çoklu oturum açma etki alanı ve URL 'Ler çoklu oturum açma bilgileri](common/sp-signonurl.png)

    **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın: `https://login.hrworks.de/?companyId=<companyId>&directssologin=true`

    > [!NOTE]
    > Değer gerçek değil. Değeri, gerçek oturum açma URL 'SI ile güncelleştirin. Değeri almak için [Hrçalışmaçoklu oturum açma istemci desteği ekibine](https://www.hrworks.de/dienstleistungen/support/) başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

5. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imza sertifikası** bölümünde, **Federasyon meta veri XML** 'sini gereksiniminize göre belirtilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir** ' e tıklayın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

6. **Hrçalışmalarca çoklu oturum açma ayarla** bölümünde, uygun URL 'leri gereksiniminize göre kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

    a. Oturum açma URL 'SI

    b. Azure AD tanımlayıcısı

    c. Oturum kapatma URL 'SI

### <a name="configure-hrworks-single-sign-on-single-sign-on"></a>Hrçalışmaçoklu oturum açma çoklu oturum açmayı yapılandırma

1. Farklı bir Web tarayıcısı penceresinde, hr, çoklu oturum açma sırasında yönetici olarak oturum açın.

2. **Yönetici** > , menü çubuğunun sol tarafındaki **güvenlik** > **Çoklu oturum açma** > **temelleri** ve aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırma](./media/hrworks-single-sign-on-tutorial/configure01.png)

    a. **Çoklu oturum açma kullan** kutusunu işaretleyin.

    b. **Meta veri giriş yöntemi**olarak **XML meta** verilerini seçin.

    c. **NameID Için** **bağımsız bir NameID tanımlayıcı** değeri seçin.

    d. Not defteri 'nde, Azure portal indirdiğiniz meta veri XML dosyasını açın, içeriğini kopyalayın ve sonra **meta veri** metin kutusuna yapıştırın.

    e. **Kaydet** düğmesine tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Azure portal Britta Simon adlı bir test kullanıcısı oluşturmaktır.

1. Azure portal, sol bölmedeki **Azure Active Directory**' i seçin, **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.

    !["Kullanıcılar ve gruplar" ve "tüm kullanıcılar" bağlantıları](common/users.png)

2. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.

    ![Yeni Kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı Özellikleri ' nde aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanına **Brittasıon**yazın.
  
    b. **Kullanıcı adı** alanına BrittaSimon@contoso.comgibi kullanıcı adını yazın.

    c. **Parolayı göster** onay kutusunu seçin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, hr, çoklu oturum açma 'ya erişim vererek Azure çoklu oturum açma özelliğini kullanmak için Britta Simon 'u etkinleştirin.

1. Azure portal **Kurumsal uygulamalar**' ı seçin, **tüm uygulamalar**' ı seçin ve ardından **hrçalışmaçoklu oturum açma**' yı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde, **Hrçalışmaçoklu oturum açma**' yı seçin.

    ![Uygulama listesindeki Hrçalışmalerde çoklu oturum açma bağlantısı](common/all-applications.png)

3. Soldaki menüde **Kullanıcılar ve gruplar**' ı seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. **Kullanıcı Ekle** düğmesine tıklayın, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. **Kullanıcılar ve gruplar** Iletişim kutusunda kullanıcılar listesinde **Britta Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

6. SAML onaylama işlemi içinde herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, listeden Kullanıcı için uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

7. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

### <a name="create-hrworks-single-sign-on-test-user"></a>Hrçalışmalar çoklu oturum açma test kullanıcısı oluşturma

Azure AD kullanıcılarını etkinleştirmek için, hr, çoklu oturum açma 'da oturum açın, hr, çoklu oturum açma ' ya sağlanması gerekir. Hrçalışmalerde çoklu oturum açma bölümünde, sağlama el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Hrçalışmalerde yönetici olarak çoklu oturum açma oturumu açın.

2. **Yönetici** > **kişiler** > , menü çubuğunun sol tarafındaki **kişiler** > **Yeni kişi** ' ye tıklayın.

     ![Çoklu oturum açmayı yapılandırma](./media/hrworks-single-sign-on-tutorial/configure02.png)

3. Açılır pencerede **İleri**' ye tıklayın.

    ![Çoklu oturum açmayı yapılandırma](./media/hrworks-single-sign-on-tutorial/configure03.png)

4. **Yasal koşullar için ülke ile yeni kişi oluştur** açılır penceresinde, **ad**, **Soyadı** gibi ilgili ayrıntıları girin ve **Oluştur**' a tıklayın.
    
    ![Çoklu oturum açmayı yapılandırma](./media/hrworks-single-sign-on-tutorial/configure04.png)

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim panelinde Hrçalışmaçoklu oturum açma kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız HR, çoklu oturum açma için otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory Koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

