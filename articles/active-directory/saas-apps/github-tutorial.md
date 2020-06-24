---
title: 'Öğretici: GitHub ile tümleştirme Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve GitHub arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 8761f5ca-c57c-4a7e-bf14-ac0421bd3b5e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 06/17/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1ade9e3200909c781dc00cf4e3713395f55f173d
ms.sourcegitcommit: bf99428d2562a70f42b5a04021dde6ef26c3ec3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85253774"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-github"></a>Öğretici: GitHub ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory

Bu öğreticide, GitHub 'ı Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. GitHub 'ı Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de GitHub Kurumsal bulut kuruluşunuza erişimi olan denetim.
* GitHub Enterprise Cloud kuruluşunuza erişimi tek bir merkezi konumda yönetin-Azure portal.

Azure AD ile SaaS uygulaması tümleştirmesi hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini GitHub ile yapılandırmak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Bir Azure AD ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü edinebilirsiniz
* GitHub Enterprise [Cloud](https://help.github.com/articles/github-s-products/#github-enterprise)'da oluşturulmuş GitHub [Enterprise faturalandırma planını](https://help.github.com/articles/github-s-billing-plans/#billing-plans-for-organizations) gerektiren bir GitHub kuruluşu

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açmayı bir test ortamında yapılandırıp test edersiniz.

* GitHub, **SP** tarafından başlatılan SSO 'yu destekler

* GitHub [ **Otomatik** Kullanıcı sağlamayı (kuruluş davetleri) destekler](github-provisioning-tutorial.md)
* GitHub 'ı yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak ayıklanmasını ve ne zaman korumasını koruyan oturum denetimini zorunlu kılabilirsiniz. Oturum denetimi koşullu erişimden genişletilir. [Microsoft Cloud App Security ile oturum denetimini nasıl zorlayacağınızı öğrenin](https://docs.microsoft.com/cloud-app-security/proxy-deployment-aad)

## <a name="adding-github-from-the-gallery"></a>Galeriden GitHub ekleme

GitHub 'ın Azure AD 'ye tümleştirmesini yapılandırmak için, Galeriden GitHub 'dan yönetilen SaaS uygulamaları listenize GitHub eklemeniz gerekir.

1. [Azure Portal](https://portal.azure.com) iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **GitHub** yazın.
1. Sonuçlar panelinden **GitHub** ' ı seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-github"></a>GitHub için Azure AD çoklu oturum açmayı yapılandırma ve test etme

**B. Simon**adlı bir test kullanıcısı kullanarak Azure AD SSO 'yu GitHub ile yapılandırın ve test edin. SSO 'nun çalışması için, GitHub 'daki bir Azure AD kullanıcısı ve ilgili Kullanıcı arasında bir bağlantı ilişkisi oluşturmanız gerekir.

Azure AD SSO 'yu GitHub ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını doldurun:

1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
    * Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -B. Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
    * Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek için.
1. **[GITHUB SSO 'Yu yapılandırma](#configure-github-sso)** -uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
    * GitHub 'da kullanıcının Azure AD gösterimine bağlı olan, GitHub 'da B. Simon 'a karşılık gelen bir buna sahip olmak için **[GitHub test kullanıcısı oluşturun](#create-github-test-user)** .
1. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. [Azure Portal](https://portal.azure.com/), **GitHub** uygulama tümleştirmesi sayfasında **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** için Düzenle/kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. **Temel SAML yapılandırması** bölümünde, aşağıdaki alanlar için değerleri girin:

   a. **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://github.com/orgs/<Organization ID>/sso`

    b. **Tanımlayıcı (VARLıK kimliği)** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://github.com/orgs/<Organization ID>`

    c. **Yanıt URL 'si** textox ' de, aşağıdaki kalıbı kullanarak bir URL yazın:`https://github.com/orgs/<Organization ID>/saml/consume`


    > [!NOTE]
    > Bunların gerçek değerler olduğunu lütfen unutmayın. Bu değerleri gerçek oturum açma URL 'SI, tanımlayıcı ve yanıt URL 'siyle güncelleştirmeniz gerekir. Burada, Tanımlayıcıdaki benzersiz dize değerini kullanmanızı öneririz. Bu değerleri almak için GitHub Yöneticisi bölümüne gidin.

5. GitHub uygulamanız, SAML belirteci öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML onayları bekliyor. Aşağıdaki ekran görüntüsünde, **benzersiz kullanıcı tanımlayıcısının (ad kimliği)** **User. UserPrincipalName**ile eşlendiği varsayılan özniteliklerin listesi gösterilmektedir. GitHub uygulaması, **benzersiz kullanıcı tanımlayıcısının (ad kimliği)** **Kullanıcı. Mail**ile eşlenmesini bekler, bu nedenle, **Düzenle** simgesine tıklayarak ve öznitelik eşlemesini değiştirerek öznitelik eşlemesini düzenlemeniz gerekir.

    ![image](common/edit-attribute.png)

6. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde, **sertifika (base64)** ' i gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir** ' e tıklayın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

7. GitHub 'ı **Ayarla** bölümünde uygun URL 'leri gereksiniminize göre kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD tanımlayıcısı

    c. Oturum kapatma URL 'SI

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, B. Simon adlı Azure portal bir test kullanıcısı oluşturacaksınız.

1. Azure portal sol bölmeden **Azure Active Directory**' i seçin, **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.
1. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.
1. **Kullanıcı** özellikleri ' nde şu adımları izleyin:
   1. **Ad** alanına `B.Simon` girin.  
   1. **Kullanıcı adı** alanına, girin username@companydomain.extension . Örneğin, `B.Simon@contoso.com`.
   1. **Parolayı göster** onay kutusunu seçin ve ardından **parola** kutusunda görüntülenen değeri yazın.
   1. **Oluştur**'a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, GitHub 'a erişim vererek Azure çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde **GitHub**' ı seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle**' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. SAML assertion 'da herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, Kullanıcı için listeden uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

## <a name="configure-github-sso"></a>GitHub SSO 'yu yapılandırma

1. Farklı bir Web tarayıcısı penceresinde, GitHub kuruluş sitenizde yönetici olarak oturum açın.

2. **Ayarlar** ' a gidin ve **güvenlik** ' e tıklayın.

    ![Ayarlar](./media/github-tutorial/tutorial_github_config_github_03.png)

3. Çoklu oturum açma yapılandırma alanlarını işaret eden **SAML kimlik doğrulamasını etkinleştir** kutusunu işaretleyin. aşağıdaki adımları uygulayın:

    ![Ayarlar](./media/github-tutorial/tutorial_github_config_github_13.png)

    a. **Çoklu oturum açma URL 'si** değerini kopyalayın ve bu değeri Azure Portal **temel SAML yapılandırmasındaki** **URL 'yi aç** metin kutusuna yapıştırın.
    
    b. **Onaylama tüketici hizmeti URL 'si** değerini kopyalayın ve bu değeri Azure Portal **temel SAML yapılandırmasındaki** **yanıt URL 'si** metin kutusuna yapıştırın.

4. Aşağıdaki alanları yapılandırın:

    ![Ayarlar](./media/github-tutorial/tutorial_github_config_github_051.png)

    a. Oturum açma **URL 'si** metin kutusunda, Azure Portal kopyaladığınız **oturum açma URL 'si** değerini yapıştırın.

    b. **Veren** metin kutusunda, Azure Portal KOPYALADıĞıNıZ **Azure AD tanımlayıcı** değerini yapıştırın.

    c. İndirilen sertifikayı not defteri 'nde Azure portal açın, içeriği **ortak sertifika** metin kutusuna yapıştırın.

    d. Şu şekilde gösterildiği gibi **RSA-SHA1** ve **SHA1** ile **RSA-SHA256** ve **SHA256** arasındaki **imza yöntemini** ve **Özet yöntemini** düzenlemek için **Düzenle** simgesine tıklayın.
    
    e. GitHub 'daki URL 'nin Azure uygulama kaydı 'ndaki URL ile eşleşmesi için, **onaylama tüketici hizmeti URL 'sini (yanıt URL 'si)** varsayılan URL 'den güncelleştirin.

    ![image](./media/github-tutorial/tutorial_github_sha.png)

5. SSO sırasında doğrulama hatası veya hata olmadığını doğrulamak için **Test SAML yapılandırması** ' na tıklayın.

    ![Ayarlar](./media/github-tutorial/tutorial_github_config_github_06.png)

6. **Kaydet** 'e tıklayın

> [!NOTE]
> GitHub 'daki çoklu oturum açma, GitHub 'daki belirli bir kuruluşun kimliğini doğrular ve GitHub 'ın kimlik doğrulamasını değiştirmez. Bu nedenle, kullanıcının github.com oturumunun süresi dolmuşsa, çoklu oturum açma işlemi sırasında GitHub 'ın KIMLIĞI/parolasıyla kimlik doğrulaması yapmanız istenebilir.

### <a name="create-github-test-user"></a>GitHub test kullanıcısı oluşturma

Bu bölümün amacı, GitHub 'da Britta Simon adlı bir Kullanıcı oluşturmaktır. GitHub, varsayılan olarak etkinleştirilen Otomatik Kullanıcı sağlamayı destekler. Otomatik Kullanıcı sağlamayı yapılandırma hakkında daha [fazla ayrıntı bulabilirsiniz](github-provisioning-tutorial.md) .

**Kullanıcı el ile oluşturmanız gerekiyorsa aşağıdaki adımları gerçekleştirin:**

1. GitHub şirket sitenizde yönetici olarak oturum açın.

2. **Kişiler**' e tıklayın.

    ![People](./media/github-tutorial/tutorial_github_config_github_08.png "People")

3. **Üyeyi davet et**' e tıklayın.

    ![Kullanıcıları davet et](./media/github-tutorial/tutorial_github_config_github_09.png "Kullanıcıları davet et")

4. **Üye davet et** iletişim sayfasında, aşağıdaki adımları uygulayın:

    a. **E-posta** metin kutusunda Britta Simon hesabının e-posta adresini yazın.

    ![Kişileri davet et](./media/github-tutorial/tutorial_github_config_github_10.png "Kişileri davet et")

    b. **Davet gönder**' e tıklayın.

    ![Kişileri davet et](./media/github-tutorial/tutorial_github_config_github_11.png "Kişileri davet et")

    > [!NOTE]
    > Azure Active Directory hesap sahibi bir e-posta alır ve etkin hale gelmeden önce hesaplarını onaylamak için bir bağlantıyı izler.

## <a name="test-sso"></a>Test SSO 'SU 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim panelinde GitHub kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız GitHub 'da otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile GitHub 'ı deneyin](https://aad.portal.azure.com/)

- [Microsoft Cloud App Security oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)