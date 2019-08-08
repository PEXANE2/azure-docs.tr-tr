---
title: 'Öğretici: Kişilerle tümleştirme Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve kişiler arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 7c9b6202-11dd-4bb6-a679-8fb0a7a0ef4e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 08/01/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 334241683f95496ce9ea0629247bb8fd53364ee9
ms.sourcegitcommit: 3073581d81253558f89ef560ffdf71db7e0b592b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68826077"
---
# <a name="tutorial-integrate-people-with-azure-active-directory"></a>Öğretici: Kişileri Azure Active Directory ile tümleştirme

Bu öğreticide, kişileri Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. Kullanıcıları Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de kişilere erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarına sahip kişiler için otomatik olarak oturum açmalarına olanak sağlayın.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

Azure AD ile SaaS uygulaması tümleştirmesi hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Kişilerin çoklu oturum açma (SSO) aboneliği etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz.

* İnsanlar **SP** tarafından başlatılan SSO 'yu destekler
* Kişiler mobil uygulaması, artık SSO 'yu etkinleştirmek için Azure AD ile yapılandırılabilir. Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz.

## <a name="adding-people-from-the-gallery"></a>Galeriden kişi ekleme

Kişilerin Azure AD ile tümleştirilmesini yapılandırmak için galerideki kişileri yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalda](https://portal.azure.com) oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **kişiler** yazın.
1. Sonuçlar panelinden **kişiler** ' i seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.


## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

**B. Simon**adlı bir test kullanıcısı kullanarak Azure AD SSO 'Yu, kişilerle birlikte yapılandırın ve test edin. SSO 'nun çalışması için, bir Azure AD kullanıcısı ve kişiler 'de ilgili Kullanıcı arasında bir bağlantı ilişkisi oluşturmanız gerekir.

Azure AD SSO 'yu kişilerle birlikte yapılandırmak ve test etmek için aşağıdaki yapı taşlarını doldurun:

1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
2. **[Kişileri yapılandırma SSO](#configure-people-sso)** -uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -B. Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
4. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek için.
5. Kullanıcının Azure AD gösterimine bağlı olan kişilerde B. Simon 'a sahip olmak için **[kişi test kullanıcısı oluşturun](#create-people-test-user)** .
6. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO 'yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. [Azure Portal](https://portal.azure.com/), **kişiler** uygulama tümleştirmesi sayfasında, **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** için Düzenle/kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. **Temel SAML yapılandırması** bölümünde, aşağıdaki alanlar için değerleri girin:

    a. **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<company name>.peoplehr.net`

    b. **Tanımlayıcı** kutusuna bir URL yazın:`https://www.peoplehr.com`

    c. **Yanıt URL 'si** metin kutusuna aşağıdaki kalıbı kullanarak bir URL yazın:`https://<company name>.peoplehr.net/Pages/Saml/ConsumeAzureAD.aspx`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri, gerçek oturum açma URL 'SI ve yanıt URL 'SI ile güncelleştirin. Bu değerleri almak için [kişi istemci destek ekibine](mailto:customerservices@peoplehr.com) başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

4. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde, **Federasyon meta verileri XML** 'i bulun ve sertifikayı indirip bilgisayarınıza kaydetmek için **İndir** ' i seçin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. **Kişileri ayarlama** bölümünde, gereksiniminize göre uygun URL 'leri kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

### <a name="configure-people-sso"></a>Kişileri yapılandırma SSO 'SU

1. Uygulamanız için yapılandırılmış SSO 'yu almak için, kişiler kiracınızda yönetici olarak oturum açmanız gerekir.
   
2. Sol taraftaki menüde, **Ayarlar**' a tıklayın.

    ![Çoklu oturum açmayı yapılandırın](./media/people-tutorial/tutorial_people_001.png)

3. **Şirket**' e tıklayın.

    ![Çoklu oturum açmayı yapılandırın](./media/people-tutorial/tutorial_people_002.png)

4. **' Çoklu oturum açma ' SAML meta veri dosyasında karşıya yükle**' ye tıklayarak indirilen meta veri dosyasını karşıya yükleyin.

    ![Çoklu oturum açmayı yapılandırın](./media/people-tutorial/tutorial_people_003.png)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümde, B. Simon adlı Azure portal bir test kullanıcısı oluşturacaksınız.

1. Azure portal sol bölmeden **Azure Active Directory**' i seçin, **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.
1. Seçin **yeni kullanıcı** ekranın üstünde.
1. **Kullanıcı** özellikleri ' nde şu adımları izleyin:
   1. **Ad** alanına `B.Simon` girin.  
   1. **Kullanıcı adı** alanına, username@companydomain.extensiongirin. Örneğin: `B.Simon@contoso.com`.
   1. **Parolayı göster** onay kutusunu seçin ve ardından **parola** kutusunda görüntülenen değeri yazın.
   1.           **Oluştur**'a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, kişilere erişim vererek, B. Simon 'u Azure çoklu oturum açma özelliğini kullanacak şekilde etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde, **kişiler**' i seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.

   !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle**' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. SAML assertion 'da herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, Kullanıcı için listeden uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

### <a name="create-people-test-user"></a>Kişi testi kullanıcısı oluştur

Bu bölümde, kişiler 'de B. Simon adlı bir Kullanıcı oluşturacaksınız. Kullanıcıları kişiler platformunda eklemek için [kişi istemci destek ekibi](mailto:customerservices@peoplehr.com) ile çalışın. Kullanıcı oluşturulmalı ve çoklu oturum açma kullanmadan önce etkinleştirildi.

### <a name="test-sso"></a>Test SSO 'SU 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde kişiler kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız kişilerde otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="test-sso-for-people-mobile"></a>Kişiler için test SSO 'SU (mobil)

1. Kişiler mobil uygulamasını açın. Oturum aç sayfasında, **e-posta kimliğini** girin ve **Çoklu oturum açma**' ya tıklayın.

    ![Oturum açma](./media/people-tutorial/test01.png)

2. **Kuruluş Kullanıcı kimliği** ' ni girip **İleri**' ye tıklayın.

    ![E-posta](./media/people-tutorial/test02.png)

3. Son olarak, başarıyla oturum açtıktan sonra, uygulama giriş sayfası aşağıda gösterildiği gibi görüntülenir:

    ![Bir kez](./media/people-tutorial/test03.png)

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory Koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

