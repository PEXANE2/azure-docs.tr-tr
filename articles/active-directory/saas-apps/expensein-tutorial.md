---
title: 'Öğretici: ExpenseIn ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve ExpenseIn arasında tek oturum açma yı nasıl yapılandırıştırmayı öğrenin.
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
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "67102853"
---
# <a name="tutorial-integrate-expensein-with-azure-active-directory"></a>Öğretici: ExpenseIn'i Azure Active Directory ile tümleştir

Bu eğitimde, ExpenseIn'i Azure Etkin Dizini (Azure AD) ile nasıl tümleştireceğinizi öğreneceksiniz. ExpenseIn'i Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD'de ExpenseIn erişimi olan denetim.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla ExpenseIn'de otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* ExpenseIn tek oturum açma (SSO) özellikli abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz. **ExpenseIn, SP ve IDP'nin** başlattığı SSO'yı destekler.

## <a name="adding-expensein-from-the-gallery"></a>Galeriden ExpenseIn Ekleme

ExpenseIn'in Azure AD'ye entegrasyonunu yapılandırmak için, galeriden yönetilen SaaS uygulamaları listenize ExpenseIn eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. Galeri **bölümünden Ekle** bölümünde, arama kutusuna **GiderIn** yazın.
1. Sonuç panelinden **ExpenseIn'i** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak ExpenseIn ile yapılandırın ve test edin. SSO'nun çalışması için, Bir Azure REKLAM kullanıcısı ile ExpenseIn'deki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu ExpenseIn ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
2. Uygulama tarafındaki SSO ayarlarını yapılandırmak için **[ExpenseIn'i yapılandırın.](#configure-expensein)**
3. Azure AD oturum açma'yı B.Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[HarcamaTest kullanıcısı](#create-expensein-test-user)** oluşturun, Kullanıcının Azure AD gösterimine bağlı ExpenseIn'de B.Simon'ın bir örneğine sahip olun.
6. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[SSO'yu test](#test-sso)** edin.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında,](https://portal.azure.com/) **Gider Uygulama** tümleştirme sayfasında, **Yönet** bölümünü bulun ve Tek **oturum açma'yı**seçin.
1. Tek **oturum açma yöntemi** sayfasında **SAML'yi**seçin.
1. **SAML** ile Tek Oturum Açma'da, ayarları düzenlemek için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırma** bölümünde, uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız, aşağıdaki adımı gerçekleştirin:

    **Url'yi Yanıtla** metin kutusuna URL'den herhangi birini yazın:

    | |
    |--|
    | `https://app.expensein.com/samlcallback` |
    | `https://mobileapi.expensein.com/identity/samlcallback` |

5. Uygulamayı **SP** başlatılan modda yapılandırmak istiyorsanız **ek URL'ler ayarla'yı** tıklatın ve aşağıdaki adımı gerçekleştirin:

    Oturum **Açma URL** metin kutusuna bir URL yazın:`https://app.expensein.com/saml`

1. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, App **Federation Metadata Url'sini** kopyalamak için kopyala düğmesini tıklatın ve **Sertifikayı (Base64)** indirip bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

   ![Sertifika indirme bağlantısı](./media/expensein-tutorial/copy-metdataurl-certificate.png)

1. **GiderI Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

   ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

### <a name="configure-expensein"></a>Harcamayı Yapılandır

1. ExpenseIn içindeki yapılandırmayı otomatikleştirmek için, **uzantıyı yükle'yi**tıklatarak **Uygulamalarım Güvenli Oturum Açma tarayıcı uzantısını** yüklemeniz gerekir.

    ![Uygulamalar uzantım](common/install-myappssecure-extension.png)

2. Tarayıcıya uzantı ekledikten sonra **Kurulum Giderin'e** tıklayın ve sizi ExpenseIn uygulamasına yönlendirecektir. Buradan, ExpenseIn'de oturum açmaları için yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı uygulamayı sizin için otomatik olarak yapılandıracak ve 3-5 adımlarını otomatikleştirecektir.

    ![Kurulum yapılandırması](common/setup-sso.png)

3. ExpenseIn'i el ile kurmak istiyorsanız, yeni bir web tarayıcıpenceresi açın ve ExpenseIn şirket sitenizde yönetici olarak oturum açın ve aşağıdaki adımları gerçekleştirin:

4. Sayfanın üst kısmında **Admin'e** tıklayın ve **ardından Tek Oturum Aç'a** gidin ve **Sağlayıcı Ekle'yi**tıklatın.

     ![HarcamaYapılandırmada](./media/expenseIn-tutorial/config01.png)

5. Yeni **Kimlik Sağlayıcısı** açılır pencerede aşağıdaki adımları gerçekleştirin:

    ![HarcamaYapılandırmada](./media/expenseIn-tutorial/config02.png)

    a. Sağlayıcı **Adı** metin kutusuna, adı ex:Azure gibi yazın.

    b. **Sağlayıcı Intitated Sign-On'a İzin Ver**olarak **Evet'i** seçin.

    c. Hedef **Url** metin kutusuna, Azure portalından kopyalamış olduğunuz **Giriş URL'sinin**değerini yapıştırın.

    d. **Veren** metin kutusuna, Azure portalından kopyaladığınız **Azure AD Tanımlayıcısı'nın**değerini yapıştırın.

    e. Sertifikayı (Base64) Not Defteri'nde açın, içeriğini kopyalayın ve **Sertifika** metin kutusuna yapıştırın.

    f. **Oluştur'u**tıklatın.

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

Bu bölümde, B.Simon'ın ExpenseIn'e erişim sağlayarak Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **ExpenseIn'i**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

### <a name="create-expensein-test-user"></a>Harcama OluşturmaTest kullanıcı

Azure AD kullanıcılarının ExpenseIn'de oturum açabilmeleri için ExpenseIn'de oturum açmaları gerekir. ExpenseIn'de, sağlama el ile bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Yönetici olarak ExpenseIn'de oturum açın.

2. Sayfanın üst kısmında **Admin'e** tıklayın ve **ardından Kullanıcılar'a** gidin ve **Yeni Kullanıcı'yı**tıklatın.

     ![HarcamaYapılandırmada](./media/expenseIn-tutorial/config03.png)

3. **Ayrıntılar** açılır pencerede aşağıdaki adımları gerçekleştirin:

    ![HarcamaYapılandırmada](./media/expenseIn-tutorial/config04.png)

    a. **Ad metin** kutusuna, **B**gibi kullanıcının ilk adını girin.

    b. **Soyadı** metin kutusuna, **Simon**gibi kullanıcının soyadını girin.

    c. **E-posta** metin kutusuna, gibi `B.Simon@contoso.com`kullanıcının e-posta girin.

    d. **Oluştur'u**tıklatın.

### <a name="test-sso"></a>Test SSO

Access Paneli'ndeki Gider Karosu'nu seçtiğinizde, SSO'yu kurduğunuz ExpenseIn'de otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
