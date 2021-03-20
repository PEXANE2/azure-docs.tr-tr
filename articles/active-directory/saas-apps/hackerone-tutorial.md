---
title: 'Öğretici: HackerOne ile tümleştirme Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve HackerOne arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 02/15/2019
ms.author: jeedes
ms.openlocfilehash: ac8bdc37bc2572b580deebc35931e324ed107d87
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "92446661"
---
# <a name="tutorial-azure-active-directory-integration-with-hackerone"></a>Öğretici: HackerOne ile tümleştirme Azure Active Directory

Bu öğreticide, Azure Active Directory (Azure AD) ile HackerOne tümleştirme hakkında bilgi edineceksiniz.
Azure AD ile HackerOne tümleştirme aşağıdaki avantajları sağlar:

* Azure AD 'de, HackerOne 'a erişimi olan denetimi yapabilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla HackerOne (çoklu oturum açma) için otomatik olarak oturum açmasını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz-Azure portal.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek istiyorsanız, bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesini HackerOne ile yapılandırmak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Bir Azure AD ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü edinebilirsiniz
* HackerOne çoklu oturum açma etkin abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açmayı bir test ortamında yapılandırıp test edersiniz.

* HackerOne, **SP** tarafından başlatılan SSO 'yu destekler
* HackerOne **, yalnızca zamanında** Kullanıcı sağlamayı destekler

## <a name="adding-hackerone-from-the-gallery"></a>Galeriden HackerOne ekleme

HackerOne 'ın Azure AD 'ye tümleştirilmesini yapılandırmak için, galerinizden yönetilen SaaS uygulamaları listenize HackerOne eklemeniz gerekir.

**Galeriden HackerOne eklemek için aşağıdaki adımları uygulayın:**

1. **[Azure Portal](https://portal.azure.com)** sol gezinti panelinde **Azure Active Directory** simgesine tıklayın.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar** seçeneğini belirleyin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için, iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesine tıklayın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **HackerOne** yazın, sonuç panelinden **HackerOne** ' ı seçin ve sonra uygulamayı eklemek için **Ekle** düğmesine tıklayın.

     ![Sonuçlar listesinde HackerOne](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma ve test etme

Bu bölümde, Azure AD çoklu oturum açmayı, **Britta Simon** adlı bir test kullanıcısına göre HackerOne ile yapılandırıp test edersiniz.
Çoklu oturum açma için, bir Azure AD kullanıcısı ve HackerOne içindeki ilgili Kullanıcı arasındaki bağlantı ilişkisinin oluşturulması gerekir.

Azure AD çoklu oturum açma 'yı HackerOne ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını gerçekleştirmeniz gerekir:

1. **[Azure AD çoklu oturum açma özelliğini yapılandırarak](#configure-azure-ad-single-sign-on)** kullanıcılarınızın bu özelliği kullanmasına olanak sağlayın.
2. **[HackerOne çoklu oturum açmayı yapılandırma](#configure-hackerone-single-sign-on)** -uygulama tarafında tek Sign-On ayarlarını yapılandırmak için.
3. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -Britta Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
4. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanarak Britta Simon 'u etkinleştirin.
5. **[HackerOne test kullanıcısı oluşturun](#create-hackerone-test-user)** -HackerOne 'Da kullanıcının Azure AD gösterimine bağlanmış Britta Simon 'un bir karşılığı vardır.
6. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[Çoklu oturum açmayı sınayın](#test-single-sign-on)** .

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure portal Azure AD çoklu oturum açma özelliğini etkinleştirirsiniz.

Azure AD çoklu oturum açmayı HackerOne ile yapılandırmak için aşağıdaki adımları uygulayın:

1. [Azure Portal](https://portal.azure.com/), **HackerOne** uygulama tümleştirmesi sayfasında, **Çoklu oturum açma**' yı seçin.

    ![Çoklu oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Çoklu oturum **açma yöntemi seç** iletişim kutusunda, çoklu oturum açmayı etkinleştirmek için **SAML/WS-Besme** modunu seçin.

    ![Çoklu oturum açma seçme modu](common/select-saml-option.png)

3. **SAML Ile tek Sign-On ayarlama** sayfasında, **temel SAML yapılandırması** Iletişim kutusunu açmak için **Düzenle** simgesine tıklayın.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. **Temel SAML yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![HackerOne etki alanı ve URL 'Ler çoklu oturum açma bilgileri](common/sp-identifier.png)

    a. **Oturum açma URL 'si** metin kutusunda, aşağıdakileri girin:`https://hackerone.com/users/saml/sign_in?email=<configured domain>`

    b. **Tanımlayıcı (VARLıK kimliği)** metin kutusuna şunu girin:`hackerone.com`

5. **SAML Ile tek Sign-On ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde, **sertifika (base64)** ' i gereksiniminize göre ve bilgisayarınıza kaydetmek için **İndir** ' e tıklayın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. **HackerOne ayarla** bölümünde uygun URL 'leri gereksiniminize göre kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD tanımlayıcısı

    c. Oturum kapatma URL 'SI

### <a name="configure-hackerone-single-sign-on"></a>HackerOne tek Sign-On yapılandırma

1. HackerOne kiracınızda yönetici olarak oturum açın.

2. Üstteki menüde, **Ayarlar**' a tıklayın.

    ![Menüde "Ayarlar" seçili olduğunu gösteren ekran görüntüsü.](./media/hackerone-tutorial/tutorial_hackerone_001.png)

3. **Kimlik doğrulaması** ' na gıdın ve **SAML ayarları ekle**' ye tıklayın.

    !["Kimlik doğrulama ayarları" sayfasını "S A M L ayarları ekle" düğmesi seçiliyken gösteren ekran görüntüsü.](./media/hackerone-tutorial/tutorial_hackerone_003.png)

4. **SAML ayarları** iletişim kutusunda, aşağıdaki adımları uygulayın:

    ![Tek Sign-On yapılandırma](./media/hackerone-tutorial/tutorial_hackerone_004.png)

    a. **E-posta etki alanı** metin kutusuna kayıtlı bir etki alanı yazın.

    b. **Çoklu oturum açma URL 'si** metin kutularına, Azure Portal kopyaladığınız **oturum açma URL 'si** değerini yapıştırın.

    c. İndirilen **sertifika dosyanızı** Azure Portal Not defteri ' nden açın, bu içeriği panonuza kopyalayın ve ardından **x509 sertifika**  metin kutusuna yapıştırın.

    d. **Kaydet**’e tıklayın.

5. Kimlik doğrulama ayarları iletişim kutusunda, aşağıdaki adımları uygulayın:

    !["Test Çalıştır" düğmesinin seçili olduğu "kimlik doğrulama ayarları" iletişim kutusunun gösterildiği ekran görüntüsü.](./media/hackerone-tutorial/tutorial_hackerone_005.png)

    a. **Test Çalıştır** öğesine tıklayın.

6. Sınama başarıyla tamamlandığında ve **durum** alanı **son test durumu: başarılı**' i gösteriyorsa, bir onay için HackerOne 'A göndermek üzere **doğrulama iste** düğmesini seçin.

    ![Bir onay için HackerOne 'a gönderme](./media/hackerone-tutorial/tutorial-hackerone-006.png)

7. HackerOne ayarları onayladıktan sonra, tüm kullanıcılar için SSO kimlik doğrulaması gerektirmek için **kullanıcıları geçir** düğmesini seçebilirsiniz.

    ![SAML 'yi etkinleştir](./media/hackerone-tutorial/tutorial-hackerone-007.png)

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Azure portal Britta Simon adlı bir test kullanıcısı oluşturmaktır.

1. Azure portal, sol bölmedeki **Azure Active Directory**' i seçin, **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.

    !["Kullanıcılar ve gruplar" ve "tüm kullanıcılar" bağlantıları](common/users.png)

2. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.

    ![Yeni Kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı Özellikleri ' nde aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanına **Brittasıon** girin.

    b. **Kullanıcı adı** alanına **\@ bricompansıon yourcompanydomain. Extension** yazın  
    Örneğin, BrittaSimon@contoso.com

    c. **Parolayı göster** onay kutusunu seçin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, HackerOne 'a erişim vererek Azure çoklu oturum açma özelliğini kullanmak için Britta Simon 'u etkinleştirin.

1. Azure portal **Kurumsal uygulamalar**' ı seçin, **tüm uygulamalar**' ı seçin ve ardından **HackerOne**' ı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde, **HackerOne**' ı seçin.

    ![Uygulamalar listesindeki HackerOne bağlantısı](common/all-applications.png)

3. Soldaki menüde **Kullanıcılar ve gruplar**' ı seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. **Kullanıcı Ekle** düğmesine tıklayın, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. **Kullanıcılar ve gruplar** Iletişim kutusunda kullanıcılar listesinde **Britta Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

6. SAML onaylama işlemi içinde herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, listeden Kullanıcı için uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

7. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

### <a name="create-hackerone-test-user"></a>HackerOne test kullanıcısı oluşturma

Bu bölümde, HackerOne 'da Britta Simon adlı bir Kullanıcı oluşturulur. HackerOne, varsayılan olarak etkinleştirilen tam zamanında Kullanıcı sağlamayı destekler. Bu bölümde sizin için herhangi bir eylem öğesi yok. Bir Kullanıcı HackerOne 'da zaten mevcut değilse, kimlik doğrulamasından sonra yeni bir tane oluşturulur.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim panelinde HackerOne kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız HackerOne 'da otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](../user-help/my-apps-portal-end-user-access.md).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](./tutorial-list.md)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

- [Azure Active Directory Koşullu erişim nedir?](../conditional-access/overview.md)