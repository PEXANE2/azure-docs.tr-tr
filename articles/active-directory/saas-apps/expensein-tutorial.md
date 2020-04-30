---
title: "Öğretici: ' de Expenseile tümleştirme Azure Active Directory | Microsoft Docs"
description: Azure Active Directory ve Expensearasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: 6ac8053b-a216-45d8-bf5e-ecd37d808e57
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 06/11/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7c09542013dff3a18965d1070216a938c26a144e
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "67102853"
---
# <a name="tutorial-integrate-expensein-with-azure-active-directory"></a>Öğretici: Expensewith Azure Active Directory ile tümleştirin

Bu öğreticide, Azure Active Directory (Azure AD) ile Expense'in nasıl tümleştirileceğini öğreneceksiniz. Expensewith Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de, ' de Expense'e erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla birlikte Expenseto 'da otomatik olarak oturum açmalarına olanak sağlayın.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

Azure AD ile SaaS uygulaması tümleştirmesi hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Çoklu oturum açma (SSO) özellikli abonelikte Expense.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz. Expenseın, **SP ve ıDP** tarafından başlatılan SSO 'yu destekler.

## <a name="adding-expensein-from-the-gallery"></a>Galeriden Expensefrom ekleme

' Deki expense'in tümleştirmesini Azure AD olarak yapılandırmak için, galerideki Expenseın ' u yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. [Azure Portal](https://portal.azure.com) iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **expenseyazın** .
1. Sonuçlar panelinden **expensefrom '** ı seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma ve test etme

**B. Simon**adlı bir test kullanıcısı kullanarak Azure AD SSO 'yu expenseile yapılandırın ve test edin. SSO 'nun çalışması için, bir Azure AD kullanıcısı ve ' de Expenseiçindeki ilgili Kullanıcı arasında bir bağlantı ilişkisi oluşturmanız gerekir.

Azure AD SSO 'yu ' de Expensewith yapılandırmak ve test etmek için aşağıdaki yapı taşlarını doldurun:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** .
2. Uygulama tarafında SSO ayarlarını yapılandırmak için **[Expenseın ' i yapılandırın](#configure-expensein)** .
3. B. Simon ile Azure AD çoklu oturum açma sınamasını test etmek için **[bir Azure AD test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** .
4. Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek üzere **[Azure AD test kullanıcısını atayın](#assign-the-azure-ad-test-user)** .
5. Kullanıcının Azure AD gösterimine bağlı olan, ' de Expenseon 'da B. Simon 'a sahip olmak için, **[Test kullanıcısına Expensecreate](#create-expensein-test-user)** .
6. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[test SSO 'su](#test-sso)** .

### <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. [Azure Portal](https://portal.azure.com/), uygulama tümleştirmesinde **Expenseın** sayfasında **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** için Düzenle/kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. **Temel SAML yapılandırması** bölümünde, uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız aşağıdaki adımı uygulayın:

    **Yanıt URL** 'si metın kutusuna URL 'den birini yazın:

    | |
    |--|
    | `https://app.expensein.com/samlcallback` |
    | `https://mobileapi.expensein.com/identity/samlcallback` |

5. Uygulamayı **SP** tarafından başlatılan modda yapılandırmak Istiyorsanız **ek URL 'ler ayarla** ' ya tıklayın ve aşağıdaki adımı gerçekleştirin:

    **Oturum açma URL 'si** metin kutusuna bir URL yazın:`https://app.expensein.com/saml`

1. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde, Kopyala düğmesine tıklayarak **uygulama Federasyon meta verileri URL 'sini** kopyalayın ve sertifikayı indirmek Için **İndir** ' e tıklayın **(base64)** ve bilgisayarınıza kaydedin.

   ![Sertifika indirme bağlantısı](./media/expensein-tutorial/copy-metdataurl-certificate.png)

1. **Expenseon ' u ayarla** bölümünde, gereksiniminize göre uygun URL 'leri kopyalayın.

   ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

### <a name="configure-expensein"></a>Expenseın yapılandırma

1. ' De Expenseın içindeki yapılandırmayı otomatikleştirmek için, **uzantıyı yüklemek**üzere **uygulamalarımı güvenli oturum açma tarayıcı uzantısı** ' nı yüklemeniz gerekir.

    ![Uygulamalarım uzantısı](common/install-myappssecure-extension.png)

2. Tarayıcıya Uzantı eklendikten sonra, ' **ın** ' a tıklayın. Buradan, adresinden Expenseın üzerinde oturum açmak için yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı, uygulamayı sizin için otomatik olarak yapılandırır ve 3-5 adımlarını otomatikleştirecektir.

    ![Kurulum yapılandırması](common/setup-sso.png)

3. Expenseas 'yi el ile ayarlamak istiyorsanız yeni bir Web tarayıcısı penceresi açın ve bir yönetici olarak şirket sitesinde Expenseoturumunuzu açın ve aşağıdaki adımları gerçekleştirin:

4. Sayfanın üst kısmındaki **yönetici** ' ye tıklayın, ardından **Çoklu oturum açma** ' ya gidin ve **Sağlayıcı Ekle**' ye tıklayın.

     ![Yapılandırmada Expense](./media/expenseIn-tutorial/config01.png)

5. **Yeni kimlik sağlayıcısı** açılır penceresinde aşağıdaki adımları uygulayın:

    ![Yapılandırmada Expense](./media/expenseIn-tutorial/config02.png)

    a. **Sağlayıcı adı** metin kutusuna, Ex: Azure gibi bir ad yazın.

    b. **Sağlayıcı tarafından önceden oturum açma Izni ver '** i seçerek **Evet** ' i seçin.

    c. **Hedef URL** metin kutusunda, Azure Portal kopyaladığınız **oturum açma URL 'si**değerini yapıştırın.

    d. **Veren** metin kutusunda, Azure Portal KOPYALADıĞıNıZ **Azure AD tanımlayıcısının**değerini yapıştırın.

    e. Sertifika (base64) Not defteri 'nde açın, içeriğini kopyalayın ve **sertifika** metin kutusuna yapıştırın.

    f. **Oluştur**' a tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, B. Simon adlı Azure portal bir test kullanıcısı oluşturacaksınız.

1. Azure portal sol bölmeden **Azure Active Directory**' i seçin, **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.
1. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.
1. **Kullanıcı** özellikleri ' nde şu adımları izleyin:
   1. **Ad** alanına `B.Simon` girin.  
   1. **Kullanıcı adı** alanına, username@companydomain.extensiongirin. Örneğin, `B.Simon@contoso.com`.
   1. **Parolayı göster** onay kutusunu seçin ve ardından **parola** kutusunda görüntülenen değeri yazın.
   1. **Oluştur**' a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, ' de Expense'e erişim izni vererek Azure çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde, **Içinde expense'** i seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle**' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. SAML assertion 'da herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, Kullanıcı için listeden uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

### <a name="create-expensein-test-user"></a>Test kullanıcısına Expenseoluştur

Azure AD kullanıcılarının ' de Expenseöğesinde oturum açmasını sağlamak için, ' de Expense'e sağlanması gerekir. Expenseın içinde, sağlama el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Bir yönetici olarak ' de Expenseöğesinde oturum açın.

2. Sayfanın üst kısmındaki **yönetici** ' ye tıklayın, ardından **Kullanıcılar** ' a gidin ve **Yeni Kullanıcı**' ya tıklayın.

     ![Yapılandırmada Expense](./media/expenseIn-tutorial/config03.png)

3. **Ayrıntılar** açılır penceresinde aşağıdaki adımları uygulayın:

    ![Yapılandırmada Expense](./media/expenseIn-tutorial/config04.png)

    a. **Ad** metin kutusuna **B**gibi kullanıcının adını girin.

    b. **Soyadı** metin kutusuna, **Simon**gibi kullanıcı adının soyadını girin.

    c. **E-posta** metin kutusuna kullanıcının e-postasını girin `B.Simon@contoso.com`.

    d. **Oluştur**' a tıklayın.

### <a name="test-sso"></a>Test SSO 'SU

Erişim panelinde Expenseın kutucuğunu seçtiğinizde, SSO 'yu ayarladığınız Expenseın ' de otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory Koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
