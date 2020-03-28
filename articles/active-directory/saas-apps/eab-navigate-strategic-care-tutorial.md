---
title: 'Öğretici: EAB Ile Azure Active Directory tek oturum açma (SSO) entegrasyonu Stratejik Bakımda Gezinme | Microsoft Dokümanlar'
description: Azure Active Directory ve EAB Arasında Tek Oturum Açma'yı nasıl yapılandıracak larını öğrenin Stratejik Bakımda Gezinme.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 0c61fcfc-b89b-4f89-9b81-27098047cddb
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 08/20/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1185217d6b12d3efd5dedc9faf903f1b365481db
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "70014072"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-eab-navigate-strategic-care"></a>Öğretici: EAB Ile Azure Active Directory tek oturum açma (SSO) entegrasyonu Stratejik Bakımda Gezinme

Bu eğitimde, EAB'nin Stratejik Bakımda Gezinme'yi Azure Etkin Dizini (Azure AD) ile nasıl entegre edeceğinizi öğreneceksiniz. EAB Gezinme Stratejik Bakım'ı Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* EAB'nin Stratejik Bakımda Gezinme'sine erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla EAB'de Otomatik Olarak Oturum Aç'a kaydolmasını etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* EAB Navigate Strategic Care tek oturum açma (SSO) aboneliği etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* EAB Navigate Stratejik Bakım **SP** başlatılan SSO destekler

## <a name="adding-eab-navigate-strategic-care-from-the-gallery"></a>Galeriden EAB Gezinme Stratejik Bakım ekleme

EAB Navigate Strategic Care'in Azure AD'ye entegrasyonunu yapılandırmak için, galeriden Yönetilen SaaS uygulamaları listenize EAB Navigate Stratejik Bakım'ı eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Stratejik Bakımda Gezinme'yi** yazın.
1. Sonuçlar panelinden **Stratejik Bakım'da Gezinme'yi** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.


## <a name="configure-and-test-azure-ad-single-sign-on-for-eab-navigate-strategic-care"></a>EAB Stratejik Bakımda Gezinmek için Azure AD oturum açma işlemlerini yapılandırın ve test edin

Azure AD SSO'nu EAB Ile **B.Simon**adlı bir test kullanıcısı kullanarak Stratejik Bakımda gezinme ile yapılandırın ve test edin. SSO'nun çalışması için, bir Azure AD kullanıcısı ile EAB'deki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu EAB Gezinme Stratejik Bakım ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    1. Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    1. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[EAB Navigate Strategic Care SSO'yu yapılandırır](#configure-eab-navigate-strategic-care-sso)** - uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için.
    1. **[EAB Navigate Strategic Care test kullanıcısını oluşturun](#create-eab-navigate-strategic-care-test-user)** - EAB'deki B.Simon'ın, kullanıcının Azure AD temsiline bağlı Stratejik Bakımda Gezinme'deki bir muadili olması için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında,](https://portal.azure.com/) **EAB'nin Stratejik Bakım** uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve tek **oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde, aşağıdaki alanların değerlerini girin:

    Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://<CUSTOMERURL>.eab.com`

    > [!NOTE]
    > Değer gerçek değil. Değeri gerçek Oturum Açma URL'si ile güncelleştirin. Değeri almak için [EAB'nin Stratejik Bakım İstemci destek ekibine](mailto:tech@gradesfirst.com) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

1. **SAML ile Tek Oturum** Açma sayfasında, **SAML İmza Sertifikası** bölümünde, App Federation **Metadata Url'sini** kopyalamak ve bilgisayarınıza kaydetmek için kopyala düğmesini tıklatın.

    ![Sertifika indirme bağlantısı](common/copy-metadataurl.png)

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

Bu bölümde, B.Simon'ın EAB'nin Stratejik Bakımda Gezinme'ye erişim izni vererek Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde, **EAB'nin Stratejik Bakımda Gezinme'yi**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-eab-navigate-strategic-care-sso"></a>EAB'yi Yapılandırın Stratejik Bakım SSO'su gezin

**EAB Navigate Strategic Care** tarafında tek oturum açma yapılandırmak için, [EAB Navigate Stratejik Bakım destek ekibine](mailto:tech@gradesfirst.com) **App Federation Metadata Url'sini** göndermeniz gerekir. Bu ayarı, SAML SSO bağlantısının her iki tarafta da düzgün bir şekilde ayarlanması için ayarlarlar.

### <a name="create-eab-navigate-strategic-care-test-user"></a>EAB'yi Stratejik Bakım testi kullanıcısında gezinme oluşturma

Bu bölümde, EAB'de Stratejik Bakımda Gezinme'de B.Simon adında bir kullanıcı oluşturursunuz. Kullanıcıları EAB'nin Stratejik Bakımda Gezinme platformuna eklemek için [EAB'de Gezinme Stratejik Bakım destek ekibiyle](mailto:tech@gradesfirst.com) birlikte çalışın. Tek oturum açmadan önce kullanıcılar oluşturulmalı ve etkinleştirilmelidir.

## <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki EAB Navigate Stratejik Bakım döşemesini tıklattığınızda, SSO'yu kurduğunuz EAB Navigate Stratejik Bakım'da otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile EAB'nin Stratejik Bakımda Gezinmeyi deneyin](https://aad.portal.azure.com/)

