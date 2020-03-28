---
title: 'Öğretici: Mail Luck ile Azure Active Directory tek oturum açma (SSO) entegrasyonu! | Microsoft Belgeleri'
description: Azure Active Directory ve Mail Luck arasında tek oturum açma yı nasıl yapılandırıştırmayı öğrenin!.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 783ea511-0b02-4455-ac8c-7aefde4ae4df
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/24/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: daf86270224d2a76eb124e879594ac2588b46776
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "72969822"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-mail-luck"></a>Öğretici: Mail Luck ile Azure Active Directory tek oturum açma (SSO) entegrasyonu!

Bu eğitimde, Nasıl Mail Luck entegre öğreneceksiniz! Azure Etkin Dizin (Azure AD) ile. Mail Luck'ı entegre ettiğinizde! Azure AD ile şunları yapabilirsiniz:

* Mail Luck erişimi olan Azure AD'de denetim!.
* Kullanıcılarınızın Mail Luck ile otomatik olarak oturum açmış olmasını etkinleştirin! Azure AD hesaplarıyla birlikte.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Mail Luck! tek oturum açma (SSO) özellikli abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* Mail Luck! **SP** başlatılan SSO destekler

## <a name="adding-mail-luck-from-the-gallery"></a>Posta Luck ekleme! galeriden

Mail Luck entegrasyonunu yapılandırmak için! Azure AD'ye Mail Luck eklemeniz gerekir! galeriden yönetilen SaaS uygulamaları listenize kadar.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. Galeri **bölümünden Ekle** **bölümünde, Mail Luck yazın!** yazın.
1. **Posta Luck seçin!** sonuç panelinden ve sonra uygulama ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-mail-luck"></a>Mail Luck için Azure AD oturum açma işlemlerini yapılandırın ve test edin!

Azure AD SSO'u Mail Luck ile yapılandırın ve test edin! **B.Simon**adlı bir test kullanıcısı kullanarak. SSO'nun çalışması için, Bir Azure REKLAM kullanıcısı ile Mail Luck'daki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir!.

Azure AD SSO'yu Mail Luck ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    * Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    * B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[Posta Luck yapıla! SSO](#configure-mail-luck-sso)** - uygulama tarafında tek oturum açma ayarlarını yapılandırmak için.
    * **[Posta Luck oluşturun! test kullanıcı](#create-mail-luck-test-user)** - Mail Luck B.Simon bir meslektaşı olması! bu, kullanıcının Azure AD gösterimine bağlıdır.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında,](https://portal.azure.com/) **Mail Luck'da!** uygulama tümleştirme sayfasını, **Yönet** bölümünü bulun ve **tek oturum açma**seçeneğini belirleyin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde, aşağıdaki alanların değerlerini girin:

    a. URL metin kutusunda **Oturum Aç** kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://manage<UNITID>.ml-sgw.jp/<TENANT_NAME>/saml/sign_in`

    b. Tanımlayıcı **(Entity ID)** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://manage<UNITID>.ml-sgw.jp/<TENANT_NAME>/saml/`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri URL ve Tanımlayıcı'daki gerçek Oturum'la güncelleştirin. Posta Luck ile iletişime [geçin! ](https://customer.nttpc.co.jp/cgi-bin/form/inquiry_index.cgi)Bu değerleri almak için istemci destek ekibi. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

1. **SAML ile Tek Oturum** Açma sayfasında, **SAML İmza Sertifikası** bölümünde, App **Federation Metadata Url'sini** kopyalamak ve bilgisayarınıza kaydetmek için kopyala düğmesini tıklatın.

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

Bu bölümde, B.Simon'ın Mail Luck!'a erişim sağlayarak Azure tek oturum açma'yı kullanmasını sağlayacaksınız!

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde, Mail **Luck'ı seçin!**
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-mail-luck-sso"></a>Posta Luck yapıla! SSO

Mail Luck'da tek oturum açma yapılandırmak **için!** yan, Mail Luck **App Federation Metadata Url** göndermeniz [gerekir! destek ekibi](https://customer.nttpc.co.jp/cgi-bin/form/inquiry_index.cgi). Bu ayarı, SAML SSO bağlantısının her iki tarafta da düzgün bir şekilde ayarlanması için ayarlarlar.

### <a name="create-mail-luck-test-user"></a>Posta Luck oluşturun! test kullanıcısı

Bu bölümde, Mail Luck B.Simon adlı bir kullanıcı oluşturmak!. Mail Luck ile [çalışın!](https://customer.nttpc.co.jp/cgi-bin/form/inquiry_index.cgi) Platform. Tek oturum açmadan önce kullanıcılar oluşturulmalı ve etkinleştirilmelidir.

## <a name="test-sso"></a>Test SSO

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Posta Luck tıkladığınızda! Erişim Paneli'ndeki döşeme, otomatik olarak Mail Luck oturum açmış olmalıdır! bunun için SSO kurmak. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Mail Luck'ı deneyin! Azure AD ile](https://aad.portal.azure.com/)