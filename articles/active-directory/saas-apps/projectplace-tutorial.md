---
title: 'Öğretici: Projectplace ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Etkin Dizin ve Projectplace arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: 298059ca-b652-4577-916a-c31393d53d7a
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/29/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 179721075484c35c5ebbb3d936b83bc407b75a8d
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "67093533"
---
# <a name="tutorial-integrate-projectplace-with-azure-active-directory"></a>Öğretici: Projectplace'i Azure Active Directory ile tümleştirin

Bu eğitimde, Projectplace'i Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. Projectplace'i Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Projectplace erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla Projectplace'de otomatik olarak oturum açmasını etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.
* Kullanıcılar Projectplace'de otomatik olarak kullanılabilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Projectplace tek oturum açma (SSO) özellikli abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz. **Projectplace, SP ve IDP** tarafından başlatılan SSO'nun ve **Just In Time** kullanıcı sağlamanın desteklenir.

## <a name="adding-projectplace-from-the-gallery"></a>Galeriden Proje Yeri Ekleme

Projectplace'in Azure AD'ye entegrasyonunu yapılandırmak için, Projectplace'i galeriden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeribölümünden Ekle** bölümünde, arama kutusuna **Proje Yeri** yazın.
1. Sonuç panelinden **Projectplace'i** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Azure AD SSO'nu **B. Simon**adlı bir test kullanıcısı kullanarak Projectplace ile yapılandırın ve test edin. SSO'nun çalışması için, Projectplace'deki bir Azure REKLAM kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Projectplace ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
2. Uygulama tarafındaki SSO ayarlarını yapılandırmak için **[Projectplace'i yapılandırın.](#configure-projectplace)**
3. Azure AD oturum açma'yı B. Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. B. Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. Projectplace'de B. Simon'ın bir örneğine sahip olması için **[Projectplace test kullanıcısını oluşturun](#create-projectplace-test-user)** ve bu kullanıcının Azure REKLAM gösterimine bağlı.
6. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[SSO'yu test](#test-sso)** edin.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında,](https://portal.azure.com/) **Projectplace** uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve **Tek oturum açma'yı**seçin.
1. Tek **oturum açma yöntemi** sayfasında **SAML'yi**seçin.
1. **SAML** ile Tek Oturum Açma'da, ayarları düzenlemek için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde, uygulamayı **IDP** tarafından başlatılan modda yapılandırmak isterseniz, uygulama önceden yapılandırılmıştır ve gerekli URL'ler azure ile önceden doldurulmuş durumdadır. Kullanıcının **Kaydet** düğmesini tıklatarak yapılandırmayı kaydetmesi gerekir.

1. Uygulamayı **SP** başlatılan modda yapılandırmak istiyorsanız **ek URL'ler ayarla'yı** tıklatın ve aşağıdaki adımı gerçekleştirin:

    Oturum **Açma URL** metin kutusuna bir URL yazın:`https://service.projectplace.com`

1. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, gereksiniminize göre App Federation **Metadata Url'sini**kopyalamak ve Not Defteri'ne kaydetmek için kopya **simgesine** tıklayın.

   ![Sertifika indirme bağlantısı](common/copy-metadataurl.png)

1. **Projectplace'i Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

   ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

### <a name="configure-projectplace"></a>Proje Yerini Yapılandır

**Projectplace** tarafında tek oturum açma yapılandırmak için, kopyalanan **Uygulama Federasyonu Meta veri Url'sini** Azure portalından [Projectplace destek ekibine](https://success.planview.com/Projectplace/Support)göndermeniz gerekir. Bu ekip, SAML SSO bağlantısının her iki tarafta da düzgün bir şekilde ayarlanınmasını sağlar.

>[!NOTE]
>Tek oturum açma yapılandırması [Projectplace destek ekibi](https://success.planview.com/Projectplace/Support)tarafından gerçekleştirilmelidir. Yapılandırma tamamlanır tamamlanmaz bir bildirim alırsınız. 

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, Azure portalında B. Simon adında bir test kullanıcısı oluşturursunuz.

1. Azure portalındaki sol bölmeden **Azure Etkin Dizini'ni**seçin, **Kullanıcılar'ı**seçin ve ardından **Tüm Kullanıcıları**seçin.
1. Ekranın üst kısmında **Yeni kullanıcı** yı seçin.
1. **Kullanıcı** özelliklerinde aşağıdaki adımları izleyin:
   1. **Ad** alanına `B. Simon` girin.  
   1. Kullanıcı **adı** alanına. username@companydomain.extension Örneğin, `BrittaSimon@contoso.com`.
   1. **Parolayı Göster** onay kutusunu seçin ve ardından **Parola** kutusunda görüntülenen değeri yazın.
   1. **Oluştur'u**tıklatın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, B. Simon'ın Projectplace'e erişim izni vererek Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **Projectplace'i**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B. Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

### <a name="create-projectplace-test-user"></a>Projectplace test kullanıcısı oluşturma

>[!NOTE]
>Projectplace'de sağlama etkinse bu adımı atlayabilirsiniz. Proje yeri destek ekibinden, ilk oturum açma sırasında [Projectplace'de](https://success.planview.com/Projectplace/Support) kullanıcılar oluşturulduktan sonra, ön görenebilmeyi etkinleştirmesini isteyebilirsiniz.

Azure AD kullanıcılarının Projectplace'de oturum açmasını sağlamak için bunları Projectplace'e eklemeniz gerekir. Bunları el ile eklemeniz gerekir.

**Bir kullanıcı hesabı oluşturmak için şu adımları izleyin:**

1. **Projeyeri** şirket sitenizde yönetici olarak oturum açın.

2. **Kişiler'e**gidin ve ardından **Üyeler'i**seçin:
   
    ![Kişiler'e gidin ve ardından Üyeler'i seçin](./media/projectplace-tutorial/ic790228.png "People")

3. **Üye Ekle'yi**Seçin :
   
    ![Üye Ekle'yi Seçin](./media/projectplace-tutorial/ic790232.png "Üye Ekle")

4. Üye **Ekle** bölümünde aşağıdaki adımları izleyin.
   
    ![Üye ekle bölümü](./media/projectplace-tutorial/ic790233.png "Yeni Üyeler")
   
    1. Yeni **Üyeler** kutusuna, eklemek istediğiniz geçerli bir Azure REKLAM hesabının e-posta adresini girin.
   
    1. **Gönder**’i seçin.

   Etkin hale gelmeden önce hesabı onaylamak için bir bağlantı içeren bir e-posta Azure REKLAM hesap sahibine gönderilir.

>[!NOTE]
>Azure AD kullanıcı hesapları eklemek için Projectplace tarafından sağlanan diğer kullanıcı hesabı oluşturma aracını veya API'yi de kullanabilirsiniz.


### <a name="test-sso"></a>Test SSO

Access Paneli'ndeki Projeyeri döşemesini seçtiğinizde, SSO'yu kurduğunuz Proje Alanı'nda otomatik olarak oturum açmış olmanız gerekir. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)