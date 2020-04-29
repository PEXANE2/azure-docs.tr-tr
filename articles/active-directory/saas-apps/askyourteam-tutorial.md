---
title: 'Öğretici: AskYourTeam ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve AskYourTeam arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 1cef6764-de54-4920-b0ad-e560c214c72e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 02/28/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 403f39de1b7d226d9feeb33388c32f63271fdcd6
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "78968630"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-askyourteam"></a>Öğretici: AskYourTeam ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory

Bu öğreticide, Askyourekibini Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. Azure AD ile Askyourekibini tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de, AskYourTeam 'e erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla şirket içinde otomatik olarak oturum açmalarına olanak sağlayın.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

Azure AD ile SaaS uygulaması tümleştirmesi hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on).

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* AskYourTeam çoklu oturum açma (SSO) aboneliği etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz.

* AskYourTeam **SP ve ıDP** tarafından başlatılan SSO 'yu destekler.
* AskYourTeam 'i yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak ayıklanmasını ve zaman korumasını koruyan oturum denetimini zorunlu kılabilirsiniz. Oturum denetimi koşullu erişimden genişletiliyor. [Microsoft Cloud App Security ile oturum denetimini nasıl zorlayacağınızı öğrenin](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app).

## <a name="adding-askyourteam-from-the-gallery"></a>Galeriden AskYourTeam ekleme

AskYourTeam 'in tümleştirmesini Azure AD ile yapılandırmak için, Galeriden yönetilen SaaS uygulamaları listenize AskYourTeam eklemeniz gerekir.

1. [Azure Portal](https://portal.azure.com) iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Askyourteam** yazın.
1. Sonuçlar panelinden **Askyourteam** ' i seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-askyourteam"></a>AskYourTeam için Azure AD çoklu oturum açmayı yapılandırma ve test etme

**B. Simon**adlı bir test kullanıcısı kullanarak Azure AD SSO 'Yu AskYourTeam ile yapılandırın ve test edin. SSO 'nun çalışması için, bir Azure AD kullanıcısı ve AskYourTeam 'teki ilgili Kullanıcı arasında bir bağlantı ilişkisi oluşturmanız gerekir.

Azure AD SSO 'yu AskYourTeam ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını doldurun:

1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
    * Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -B. Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
    * Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek için.
1. **[AskYourTeam SSO 'Yu yapılandırma](#configure-askyourteam-sso)** -uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
    * **[Askyourteam test kullanıcısı oluşturun](#create-askyourteam-test-user)** -bu, kullanıcının Azure AD gösterimine bağlı olan Askyourteam 'te B. Simon 'ın bir karşılığı olacak şekilde.
1. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. [Azure Portal](https://portal.azure.com/), **Askyourteam** uygulama tümleştirmesi sayfasında **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** için Düzenle/kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. **Temel SAML yapılandırması** bölümünde, **IDP** tarafından başlatılan modda uygulamayı yapılandırmak istiyorsanız aşağıdaki alanlar için değerleri girin:

    **Yanıt URL 'si** metin kutusuna aşağıdaki kalıbı kullanarak bir URL yazın:`https://<COMPANY>.app.askyourteam.com/users/auth/saml/callback`

1. Uygulamayı **SP** tarafından başlatılan modda yapılandırmak Istiyorsanız **ek URL 'ler ayarla** ' ya tıklayın ve aşağıdaki adımı gerçekleştirin:

    **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<COMPANY>.app.askyourteam.com/login`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri, öğreticide daha sonra açıklanan gerçek yanıt URL 'SI ve oturum açma URL 'SI değerleriyle güncelleştirin.

1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, **SAML Imzalama sertifikası** bölümünde **sertifika bulun (base64)** ve sertifikayı indirip bilgisayarınıza kaydetmek için **İndir** ' i seçin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. **AskYourTeam 'ı ayarla** bölümünde, gereksiniminize göre uygun URL 'leri kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

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

Bu bölümde, Askyourekibine erişim vererek Azure çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde, **Askyourteam**' i seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle**' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. SAML assertion 'da herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, Kullanıcı için listeden uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

## <a name="configure-askyourteam-sso"></a>AskYourTeam SSO 'yu yapılandırma

1. Farklı bir Web tarayıcısı penceresinde, yönetici olarak AskYourTeam Web sitesinde oturum açın.

1. **Kuruluşumun**üzerine tıklayın.

    ![AskYourTeam yapılandırması](./media/askyourteam-tutorial/user1.png)

1. **Tümleştirmeler**' e tıklayın.

    ![AskYourTeam yapılandırması](./media/askyourteam-tutorial/configure1.png)

1. **Ayarları Düzenle**' ye tıklayın.

    ![AskYourTeam yapılandırması](./media/askyourteam-tutorial/configure2.png)

1. **Çoklu oturum açma tümleştirmesi düzenleme** sayfasında, aşağıdaki adımları uygulayın: 

    ![AskYourTeam yapılandırması](./media/askyourteam-tutorial/configure3.png)

    a. **SAML çoklu oturum açma hizmeti URL 'si** metin kutusunda, Azure Portal kopyaladığınız **oturum açma URL 'si** değerini yapıştırın.

    b. **SAML VARLıK kimliği** metin kutusunda, Azure Portal KOPYALADıĞıNıZ **Azure AD tanımlayıcı** değerini yapıştırın.

    c. Oturum kapatma **URL 'si** metin kutusunda, Azure Portal KOPYALADıĞıNıZ **Logout URL** değerini yapıştırın.

    d. İndirilen **sertifikayı (base64)** Azure Portal Not defteri ' nden açın ve Içeriği **SAML Imzalama sertifikası-Base64** metin kutusuna yapıştırın.

    > [!NOTE]
    > Alternatif olarak, **Dosya Seç** seçeneğine tıklayarak **Federasyon meta veri XML** dosyasını da karşıya yükleyebilirsiniz.

    e. **Yanıt URL 'si (onaylama tüketici hizmeti URL 'si)** değerini Kopyala, bu değeri Azure Portal **temel SAML yapılandırması** bölümündeki **yanıt URL 'si** metin kutusuna yapıştırın.

    f. **Oturum açma URL 'si** değerini Kopyala, bu değeri Azure Portal **temel SAML yapılandırması** bölümünde yer **alan URL 'yi aç** metin kutusuna yapıştırın.

    g. **Kaydet**’e tıklayın.

### <a name="create-askyourteam-test-user"></a>AskYourTeam test kullanıcısı oluşturma

1. Farklı bir Web tarayıcısı penceresinde, yönetici olarak AskYourTeam Web sitesinde oturum açın.

1. **Kuruluşumun**üzerine tıklayın.

    ![AskYourTeam yapılandırması](./media/askyourteam-tutorial/user1.png)

1. **Kullanıcılar** ' a tıklayın ve **Yeni Kullanıcı**' yı seçin.

    ![AskYourTeam yapılandırması](./media/askyourteam-tutorial/user2.png)

1. **Yeni Kullanıcı** bölümünde aşağıdaki adımları uygulayın:

    ![AskYourTeam yapılandırması](./media/askyourteam-tutorial/user3.png)

    1. **İlk ad** metin kutusuna kullanıcının adını girin.

    1. **Son ad** metin kutusuna kullanıcının soyadını girin.

    1. **E-posta** metin kutusuna, gibi B.Simon@contoso.comkullanıcının EMAA adını girin.

    1. Kuruluşunuzun gereksinimlerine göre Kullanıcı için **rol** seçin.

    1. **Kaydet**’e tıklayın.

## <a name="test-sso"></a>Test SSO 'SU 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim panelinde AskYourTeam kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız Askyourekibine otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile AskYourTeam 'i deneyin](https://aad.portal.azure.com/)

- [Microsoft Cloud App Security oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)
