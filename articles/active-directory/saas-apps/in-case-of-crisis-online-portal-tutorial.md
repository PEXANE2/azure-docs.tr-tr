---
title: 'Öğretici: Azure Active Directory tek oturum açma (SSO) entegrasyonu Kriz Durumunda - Online Portal | Microsoft Dokümanlar'
description: Azure Active Directory ile Kriz Durumunda - Çevrimiçi Portal arasında tek oturum açma yı nasıl yapılandırıştırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: e7864de1-017c-4bf2-a734-3d0daec648a4
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 02/04/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: aa06f63141933427d0c225b590cdeebdfb618554
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77050287"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-in-case-of-crisis---online-portal"></a>Öğretici: Kriz Durumunda Azure Active Directory tek oturum açma (SSO) entegrasyonu - Online Portal

Bu eğitimde, Kriz Durumunda - Azure Active Directory (Azure AD) ile Çevrimiçi Portalı nasıl entegre edeceğinizi öğreneceksiniz. Kriz Durumunda - Çevrimiçi Portalı Azure AD ile entegre ettiğinizde şunları yapabilirsiniz:

* Kriz Durumunda Erişebilen Azure AD'de denetim - Çevrimiçi Portal.
* Kullanıcılarınızın Azure AD hesaplarıyla Kriz Durumunda - Çevrimiçi Portal'da otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Kriz Durumunda - Online Portal tek oturum açma (SSO) aboneliği ni etkinleştirdi.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* Kriz Durumunda - Online Portal **IDP** başlatılan SSO destekler
* Kriz Durumunda - Çevrimiçi Portal'ı yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak sızma ve sızmalarını koruyan oturum denetimlerini uygulayabilirsiniz. Oturum denetimleri Koşullu Erişim'den itibaren genişletir. [Microsoft Cloud App Security ile oturum denetimini nasıl uygulayacağınızı öğrenin.](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app)

## <a name="adding-in-case-of-crisis---online-portal-from-the-gallery"></a>Kriz Durumunda Ekleme - Galeriden Online Portal

Kriz Durumunda - Çevrimiçi Portal'ın Azure AD'ye entegrasyonunu yapılandırmak için, Galeriden Yönetilen SaaS uygulamaları listenize In Case - Online Portal eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeriden Ekle** bölümünde, Arama kutusuna **Kriz Durumunda - Çevrimiçi Portal** yazın.
1. Sonuç panelinden **Kriz Durumunda - Çevrimiçi Portal'ı** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.


## <a name="configure-and-test-azure-ad-single-sign-on-for-in-case-of-crisis---online-portal"></a>Kriz Durumunda Azure AD oturum açma işlemlerini yapılandırın ve test edin - Çevrimiçi Portal

Azure AD SSO'nu Kriz Durumunda - Çevrimiçi Portal'ı **B.Simon**adlı bir test kullanıcısı kullanarak yapılandırın ve test edin. SSO'nun çalışması için, Bir Azure AD kullanıcısı ile Ilgili Kullanıcı Arasında Kriz Durumunda - Çevrimiçi Portal'da bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Kriz Durumunda - Çevrimiçi Portal ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    * Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    * B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[Kriz Durumunda Online Portal SSO'yu yapılandırın](#configure-in-case-of-crisis-online-portal-sso)** - uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için.
    * **[Kriz Durumunda Çevrimiçi Portal test kullanıcısını oluşturun](#create-in-case-of-crisis-online-portal-test-user)** - Kriz Durumunda B.Simon'Un bir muadili olması - Kullanıcının Azure AD temsiline bağlı Çevrimiçi Portal.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında](https://portal.azure.com/), **Kriz Durumunda - Çevrimiçi Portal** uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve tek **oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde, uygulama önceden yapılandırılmıştır ve gerekli URL'ler Azure ile önceden doldurulmuştür. Kullanıcının **Kaydet** düğmesini tıklatarak yapılandırmayı kaydetmesi gerekir.


1. **SAML** Ile Tek Oturum Açma sayfasında, **SAML İmza Sertifikası** bölümünde **Sertifika 'yı (Base64)** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. Kriz **Durumunda Kurulum - Çevrimiçi Portal** bölümünde, gereksiniminize göre uygun URL(ler) kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, Azure portalında B.Simon adında bir test kullanıcısı oluşturursunuz.

1. Azure portalındaki sol bölmeden **Azure Etkin Dizini'ni**seçin, **Kullanıcılar'ı**seçin ve ardından **Tüm Kullanıcıları**seçin.
1. Ekranın üst kısmında **Yeni kullanıcı** yı seçin.
1. **Kullanıcı** özelliklerinde aşağıdaki adımları izleyin:
   1. **Ad** alanına `B.Simon` girin.  
   1. Kullanıcı **adı** alanına. username@companydomain.extension Örneğin, `B.Simon@contoso.com`.
   1. **Parolayı Göster** onay kutusunu seçin ve ardından **Parola** kutusunda görüntülenen değeri yazın.
   1. **Oluştur'u**tıklatın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, B.Simon'ın Kriz Durumunda - Çevrimiçi Portal'a erişim sağlayarak Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulama listesinde, Kriz **Durumunda - Online Portal'ı**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-in-case-of-crisis-online-portal-sso"></a>Kriz Durumunda Yapılandırma Online Portal SSO

Kriz Durumunda Tek Oturum Açma **- Çevrimiçi Portal** tarafında tek oturum açma yı yapılandırmak için, indirilen **Sertifikayı (Base64)** ve uygun kopyalanmış URL'leri Azure portalından [Kriz Durumunda - Çevrimiçi Portal destek ekibine](mailto:support@rockdovesolutions.com)göndermeniz gerekir. Bu ayarı, SAML SSO bağlantısının her iki tarafta da düzgün bir şekilde ayarlanması için ayarlarlar.

### <a name="create-in-case-of-crisis-online-portal-test-user"></a>Kriz Durumunda Oluştur Online Portal test kullanıcısı

Bu bölümde, Kriz Durumunda B.Simon adında bir kullanıcı oluşturursunuz - Online Portal. Kriz [Durumunda - Online Portal destek ekibi](mailto:support@rockdovesolutions.com) ile çalışarak Kullanıcıları Kriz Durumunda - Online Portal platformuna ekleyin. Tek oturum açmadan önce kullanıcılar oluşturulmalı ve etkinleştirilmelidir.

## <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki In Crisis - Online Portal döşemesini tıklattığınızda, SSO'yu kurduğunuz Çevrimiçi Portal'da otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Kriz Durumunda Deneyin - Azure AD ile Çevrimiçi Portal](https://aad.portal.azure.com/)

- [Microsoft Cloud App Security'de oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)