---
title: 'Öğretici: Azure Active Directory tek oturum açma (SSO) İnsanlarla tümleştirme | Microsoft Dokümanlar'
description: Azure Etkin Dizin ve Kişiler arasında tek oturum açma yı nasıl yapılandırıştırmayı öğrenin.
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
ms.date: 08/27/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3a9b8f08a54c978d81a8d33c61ab3d5f5fc7271f
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "70164227"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-people"></a>Öğretici: Azure Active Directory tek oturum açma (SSO) İnsanlarla tümleştirme

Bu eğitimde, Kişileri Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. Kişileri Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Kişiler'e erişimi olan Azure AD'da denetim.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla Kişiler'de otomatik olarak oturum açmasını etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Kişiler tek oturum açma (SSO) aboneliği ni etkinleştirildi.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* İnsanlar **SP** başlatılan SSO destekler
* İnsanlar Mobil uygulaması artık SSO'yu etkinleştirmek için Azure AD ile yapılandırılabilir. Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

>[!NOTE]
>Bu uygulamanın tanımlayıcısı sabit bir dize değeridir, bu nedenle yalnızca bir örnek bir kiracıda yapılandırılabilir.

## <a name="adding-people-from-the-gallery"></a>Galeriden Kişi Ekleme

Kişiler'in Azure AD'ye entegrasyonunu yapılandırmak için, galerideki Kişileri yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Kişiler** yazın.
1. Sonuçlar panelinden **Kişiler'i** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-people"></a>Azure AD oturumaçmayı yapılandırır ve test edin

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak Kişilerle yapılandırma ve test edin. SSO'nun çalışması için, Bir Azure REKLAM kullanıcısı ile Kişiler'deki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Kişilerle yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    1. Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    1. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
2. Uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için **[Kişi SSO'yu yapılandırın.](#configure-people-sso)**
    1. **[Kullanıcının](#create-people-test-user)** Azure AD gösterimine bağlı Kişiler'deki B.Simon'ın bir örneğine sahip olmak için Kişiler test kullanıcısı oluşturun.
6. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında](https://portal.azure.com/), **Kişiler** uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve **Tek oturum açma'yı**seçin.
1. Tek **oturum açma yöntemi** sayfasında **SAML'yi**seçin.
1. **SAML** ile Tek Oturum Açma'da, ayarları düzenlemek için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde, aşağıdaki alanların değerlerini girin:

    a. Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://<company name>.peoplehr.net`

    b. **Tanımlayıcı** kutusuna bir URL yazın:`https://www.peoplehr.com`

    c. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<company name>.peoplehr.net/Pages/Saml/ConsumeAzureAD.aspx`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek Oturum Açma URL'si ve YanıtURL'i ile güncelleştirin. Bu değerleri almak için [Kişiler İstemci destek ekibine](mailto:customerservices@peoplehr.com) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

4. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, Federation **Metadata XML'i** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. Kişileri **Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

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

Bu bölümde, B.Simon'ın Kişiler'e erişim izni vererek Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **Kişiler'i**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-people-sso"></a>Yapılandırılan Kişiler SSO

1. Kişiler içindeki yapılandırmayı otomatikleştirmek için, **uzantıyı**yükleyi'ni tıklatarak **Uygulamalarım Güvenli Oturum Açma tarayıcı uzantısını** yüklemeniz gerekir.

    ![Uygulamalar uzantım](common/install-myappssecure-extension.png)

2. Tarayıcıya uzantı ekledikten **sonra, Kurulum Kişileri'ni** tıklatın ve sizi Kişiler uygulamasına yönlendirecektir. Buradan, Kişiler'de oturum açabilmek için yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı uygulamayı sizin için otomatik olarak yapılandıracak ve 3-6 adımlarını otomatikleştirecektir.

    ![Kurulum yapılandırması](common/setup-sso.png)

3. Kişiler'i el ile kurmak istiyorsanız, yeni bir web tarayıcıpenceresi açın ve Kişiler şirket sitenizde yönetici olarak oturum açın ve aşağıdaki adımları gerçekleştirin:
   
4. Sol taraftaki menüde **Ayarlar'ı**tıklatın.

    ![Tek İşaret-On'u Yapılandır](./media/people-tutorial/tutorial_people_001.png)

5. **Şirket'i**tıklatın.

    ![Tek İşaret-On'u Yapılandır](./media/people-tutorial/tutorial_people_002.png)

6. Upload **'Tek İşaret Açma' SAML meta-veri dosyasında,** indirilen meta veri dosyasını yüklemek için **Gözat'ı** tıklatın.

    ![Tek İşaret-On'u Yapılandır](./media/people-tutorial/tutorial_people_003.png)

### <a name="create-people-test-user"></a>Kişiler test kullanıcısı oluşturma

Bu bölümde, B.Simon in People adlı bir kullanıcı oluşturursunuz. Kullanıcıları Kişiler platformuna eklemek için [People Client destek ekibiyle](mailto:customerservices@peoplehr.com) birlikte çalışın. Tek oturum açmadan önce kullanıcılar oluşturulmalı ve etkinleştirilmelidir.

## <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki Kişiler döşemesini tıklattığınızda, SSO'yu kurduğunuz Kişiler'de otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="test-sso-for-people-mobile"></a>İnsanlar için Test SSO (Mobil)

1. Açık İnsanlar Mobil uygulaması. Oturum açma sayfasında, **E-posta Kimliğini** girin ve ardından **Tek Oturum Aç'a**tıklayın.

    ![Oturum açma](./media/people-tutorial/test01.png)

2. **Kuruluş UserID'yi** girin ve **İleri'yi**tıklatın.

    ![E-posta](./media/people-tutorial/test02.png)

3. Son olarak başarılı oturum açmadan sonra, uygulama ana sayfası aşağıda gösterilmiştir:

    ![Bir kez](./media/people-tutorial/test03.png)

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD'li Kişileri Deneyin](https://aad.portal.azure.com)
