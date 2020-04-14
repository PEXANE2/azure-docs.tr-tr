---
title: 'Öğretici: Zaman Dilimi Çözümleri ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Timetabling Solutions arasında tek oturum açma yı nasıl yapılandırılayarıştırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: d171564f-1acf-4d87-afe9-9d91c2c13e8e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 04/10/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 10c1db9aa0452ae70564d2025f34d05629d3bc72
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81265583"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-timetabling-solutions"></a>Öğretici: Zaman Dilimi Çözümleri ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, Zaman Dilimi Çözümlerini Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. Zaman Dilimi Çözümlerini Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Zaman Dilimi Çözümleri'ne erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Timetabling Çözümleri'nde otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Timetabling Solutions tek oturum açma (SSO) aboneliği sağladı.

> [!NOTE]
> Bu uygulamanın tanımlayıcısı sabit bir dize değeridir, bu nedenle yalnızca bir örnek bir kiracıda yapılandırılabilir.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* Timetabling **Solutions, SP** tarafından başlatılan SSO'ya destek veriyor
* Zaman Dilimi Çözümlerini yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak sızma ve sızmalarını koruyan oturum denetimini uygulayabilirsiniz. Oturum denetimi Koşullu Erişim'den genişletir. [Microsoft Cloud App Security ile oturum denetimini nasıl uygulayacağınızı öğrenin.](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app)

## <a name="adding-timetabling-solutions-from-the-gallery"></a>Galeriden Zaman Tabling Çözümleri Ekleme

Zaman Dilimi Çözümlerinin Azure AD'ye entegrasyonunu yapılandırmak için, galeriden yönetilen SaaS uygulamaları listenize Zaman Tabling Çözümleri eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Zaman Tabling Çözümleri** yazın.
1. Sonuç panelinden **Zaman Tabling Çözümleri'ni** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-timetabling-solutions"></a>Zaman Dilimi Çözümleri için Azure AD oturum açma işlemlerini yapılandırın ve test edin

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak TimeTabling Çözümleri ile yapılandırın ve test edin. SSO'nun çalışması için, Timetabling Solutions'da bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Zaman Tabling Çözümleri ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    1. Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    1. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. Uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için **[Zaman Dilimi Çözümlerini SSO'yu yapılandırın.](#configure-timetabling-solutions-sso)**
    1. **[Zaman Tabling Çözümleri test kullanıcı oluşturun](#create-timetabling-solutions-test-user)** - kullanıcının Azure AD gösterimi ile bağlantılı Timetabling Çözümleri B.Simon bir meslektaşı olması.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında,](https://portal.azure.com/) **Zaman Tabling Çözümleri** uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve tek **oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    Oturum **Açma URL** metin kutusuna bir URL yazın:`https://auth.timetabling.education/login`

1. **SAML İmzalama Sertifikası** bölümünde, **SAML İmzalama Sertifikası** iletişim kutusunu açmak için **Edit** düğmesini tıklatın.

    ![SAML İmza Sertifikasını Edin](common/edit-certificate.png)

1. **SAML İmza Sertifikası** **bölümünde, Parmak İzi Değerini** kopyalayın ve bilgisayarınıza kaydedin.

    ![Parmak Izi değerini kopyala](common/copy-thumbprint.png)

1. Zaman **Dilimi Nidaçözümleri Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

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

Bu bölümde, B.Simon'ın Zaman Dilimi Çözümleri'ne erişim sağlayarak Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **Timetabling Solutions'ı**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-timetabling-solutions-sso"></a>Zaman Dilimi Çözümlerini Yapılandırma SSO

**Timetabling Solutions** tarafında tek oturum açma yapılandırmak için, Azure portalından **Thumbprint Value** ve uygun kopyalanmış URL'leri [Timetabling Solutions destek ekibine](https://www.timetabling.com.au/contact-us/)göndermeniz gerekir. Bu ayarı, SAML SSO bağlantısının her iki tarafta da düzgün bir şekilde ayarlanması için ayarlarlar.

### <a name="create-timetabling-solutions-test-user"></a>TimeTabling Solutions test kullanıcısı oluşturma

Bu bölümde, Timetabling Solutions'da Britta Simon adında bir kullanıcı oluşturursunuz. Timetabling Solutions platformundaki kullanıcıları eklemek için [Timetabling Solutions destek ekibiyle](https://www.timetabling.com.au/contact-us/) çalışın. Tek oturum açmadan önce kullanıcılar oluşturulmalı ve etkinleştirilmelidir.

## <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki Zaman Tabling Çözümleri döşemesini tıklattığınızda, SSO'yu ayarladığınız Zaman Lama Çözümleri'nde otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Zaman Dilimi Çözümlerini Deneyin](https://aad.portal.azure.com/)

- [Microsoft Cloud App Security'de oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [Gelişmiş görünürlük ve kontrollerle Zaman Dilimi Çözümlerini nasıl koruyabilirsiniz?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

