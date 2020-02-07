---
title: 'Öğretici: Azure AD veri Bağlayıcısı başına Beekeeile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory | Microsoft Docs'
description: Azure AD veri Bağlayıcısı başına Azure Active Directory ve Beekeearasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 26c8789a-1bfe-4371-94d5-febe34f0b0e4
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 02/04/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7484d71c6032b97341537a0564d8d52b3996cc8e
ms.sourcegitcommit: 57669c5ae1abdb6bac3b1e816ea822e3dbf5b3e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/06/2020
ms.locfileid: "77050329"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-beekeeper-azure-ad-data-connector"></a>Öğretici: Azure AD veri Bağlayıcısı başına Beekeeile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory

Bu öğreticide, Azure AD veri Bağlayıcısı başına Beekeeto Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. Azure ad veri Bağlayıcısı başına Beekeeı tümleştirdiğinizde, şunları yapabilirsiniz:

* Azure AD 'de, Azure AD veri Bağlayıcısı başına Beekee'e erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Azure AD veri Bağlayıcısı başına Beekeena otomatik olarak kaydolmalarına imkan tanıyın.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

Azure AD ile SaaS uygulaması tümleştirmesi hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Azure AD veri Bağlayıcısı çoklu oturum açma (SSO) özellikli abonelik başına beekee.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz.

* Azure AD veri Bağlayıcısı başına beekeesp **ve ıDP** başlatılan SSO 'yu destekler
* Azure AD veri Bağlayıcısı başına beekee, **tam zamanında** Kullanıcı sağlamasını destekler
* Azure AD veri Bağlayıcısı başına Beekeeyapılandırıldıktan sonra, kuruluşunuzun hassas verilerinin boyutunu gerçek zamanlı olarak koruyan oturum denetimlerini zorunlu kılabilirsiniz. Oturum denetimleri koşullu erişimden genişletilir. [Microsoft Cloud App Security ile oturum denetimini nasıl zorlayacağınızı öğrenin](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app).

## <a name="adding-beekeeper-azure-ad-data-connector-from-the-gallery"></a>Galeriden Azure AD veri Bağlayıcısı başına Beekeeekleme

Azure ad veri Bağlayıcısı başına Beekeeto Azure AD ile tümleştirmeyi yapılandırmak için, galerideki Azure AD veri bağlayıcısını yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalda](https://portal.azure.com) oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Azure AD Data Connector başına beekeeyazın** .
1. Sonuçlar panelinden **Azure ad veri Bağlayıcısı başına Beekeeı** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.


## <a name="configure-and-test-azure-ad-single-sign-on-for-beekeeper-azure-ad-data-connector"></a>Azure ad veri Bağlayıcısı başına Beekeeiçin Azure AD çoklu oturum açmayı yapılandırma ve test etme

**B. Simon**adlı bir test kullanıcısı kullanarak Azure AD SSO 'Su başına Beekeeile Azure ad veri bağlayıcısını yapılandırın ve test edin. SSO 'nun çalışması için bir Azure AD kullanıcısı ve Azure AD veri Bağlayıcısı başına Beekeeiçindeki ilgili Kullanıcı arasında bir bağlantı ilişkisi oluşturmanız gerekir.

Azure ad veri Bağlayıcısı başına Beekeeile Azure AD SSO 'yu yapılandırmak ve test etmek için aşağıdaki yapı taşlarını doldurun:

1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
    * Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -B. Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
    * Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek için.
1. Uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için **[Azure AD Data Connector SSO 'Su başına Beekeeconfigure 'U yapılandırın](#configure-beekeeper-azure-ad-data-connector-sso)** .
    * Kullanıcının Azure AD gösterimine bağlı olan Azure ad veri Bağlayıcısı başına beekeeto 'da B. Simon 'a sahip olmak için **[Beekeefor Azure AD Data Connector test kullanıcısı oluşturun](#create-beekeeper-azure-ad-data-connector-test-user)** .
1. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO 'yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. [Azure Portal](https://portal.azure.com/) **Azure ad veri Bağlayıcısı** uygulama tümleştirmesi sayfasında, **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** için Düzenle/kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. **Temel SAML yapılandırması** bölümünde, **hizmet sağlayıcısı meta verileri dosyanız** varsa ve **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin:

    a. **Meta veri dosyasını karşıya yükle**' ye tıklayın.

    ![Meta veri dosyasını karşıya yükle](common/upload-metadata.png)

    b. Meta veri dosyasını seçmek için **klasör logosu** ' na tıklayın ve **karşıya yükle**' ye tıklayın.

    ![meta veri dosyası seçin](common/browse-upload-metadata.png)

    c. Meta veri dosyası başarıyla karşıya yüklendikten sonra, **tanımlayıcı** ve **yanıt URL** değerleri temel SAML yapılandırması bölümünde otomatik olarak doldurulur.

    ![image](common/idp-intiated.png)

    > [!Note]
    > **Tanımlayıcı** ve **yanıt URL 'si** değerleri otomatik olarak alamazsanız, değerleri gereksinimlerinize göre el ile girin.

1. Uygulamayı **SP** tarafından başlatılan modda yapılandırmak Istiyorsanız **ek URL 'ler ayarla** ' ya tıklayın ve aşağıdaki adımı gerçekleştirin:

    **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın: `https://<your_company>.beekeeper.io/login`

    > [!NOTE]
    > Oturum açma URL 'SI değeri gerçek değil. Bu değeri, gerçek oturum açma URL 'siyle güncelleştirin. Bu değeri almak için [Azure ad veri Bağlayıcısı istemci destek ekibi başına Beekeeile](mailto:support@beekeeper.io) iletişime geçin. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

1. Azure AD veri bağlayıcı uygulaması başına beekee, SAML belirteci öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML onayları bekler. Aşağıdaki ekran görüntüsünde varsayılan özniteliklerin listesi gösterilmektedir.

    ![image](common/default-attributes.png)

1. Yukarıdaki ' a ek olarak, Azure AD veri Bağlayıcısı uygulaması başına Beekee, daha fazla özniteliğin aşağıda gösterilen SAML yanıtına geri geçirilmesini bekler. Bu öznitelikler de önceden doldurulur, ancak gereksinimlerinize göre bunları gözden geçirebilirsiniz.

    | Adı | Kaynak özniteliği|
    | ------------ | --------- |
    | FirstName | Kullanıcı. |
    | Soyadı | User. soyadı |
    | e-posta | Kullanıcı. Mail |
    | kullanıcı adı | User. PrincipalName |
    | konum | user.jobtitle |

1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde, **Federasyon meta verileri XML** 'i bulun ve sertifikayı indirip bilgisayarınıza kaydetmek için **İndir** ' i seçin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

1. **Azure ad veri Bağlayıcısı başına Beekeeayarla** bölümünde, gereksiniminize göre uygun URL 'leri kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümde, B. Simon adlı Azure portal bir test kullanıcısı oluşturacaksınız.

1. Azure portal sol bölmeden **Azure Active Directory**' i seçin, **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.
1. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.
1. **Kullanıcı** özellikleri ' nde şu adımları izleyin:
   1. **Ad** alanına `B.Simon` girin.  
   1. **Kullanıcı adı** alanına username@companydomain.extensiongirin. Örneğin, `B.Simon@contoso.com`.
   1. **Parolayı göster** onay kutusunu seçin ve ardından **parola** kutusunda görüntülenen değeri yazın.
   1. **Oluştur**'a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure AD veri Bağlayıcısı başına Beekeeerişimi vererek Azure çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde, **Azure ad veri Bağlayıcısı başına Beekeeu**seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.

   !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle**' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. SAML assertion 'da herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, Kullanıcı için listeden uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

## <a name="configure-beekeeper-azure-ad-data-connector-sso"></a>Azure AD veri Bağlayıcısı SSO 'SU başına Beekeeyapılandırma

**Azure ad veri Bağlayıcısı tarafında Beekeeon** 'da çoklu oturum açmayı yapılandırmak için, Indirilen **Federasyon meta verileri XML** 'sini ve Azure Portal ' den uygun KOPYALANMıŞ URL 'Leri [Azure ad veri Bağlayıcısı destek ekibi başına beekeeye](mailto:support@beekeeper.io)göndermeniz gerekir. Bunlar, her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

### <a name="create-beekeeper-azure-ad-data-connector-test-user"></a>Azure AD veri Bağlayıcısı test kullanıcısı başına Beekeeoluştur

Bu bölümde, Azure AD veri Bağlayıcısı başına Beekee, Britta Simon adlı bir Kullanıcı oluşturulur. Azure AD veri Bağlayıcısı başına beekee, varsayılan olarak etkinleştirilen tam zamanında Kullanıcı sağlamayı destekler. Bu bölümde sizin için herhangi bir eylem öğesi yok. Bir Kullanıcı Azure AD veri Bağlayıcısı başına Beekeehenüz yoksa, kimlik doğrulamasından sonra yeni bir tane oluşturulur.

## <a name="test-sso"></a>Test SSO 'SU 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Azure AD veri bağlayıcı kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız Azure AD veri Bağlayıcısı başına beekeede otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory Koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Azure AD veri Bağlayıcısı başına Beekeedene](https://aad.portal.azure.com/)

- [Microsoft Cloud App Security oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)
