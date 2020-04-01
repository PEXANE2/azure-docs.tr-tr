---
title: 'Öğretici: Dropbox for Business ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Dropbox for Business arasında tek oturum açma işlemlerini nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 63502412-758b-4b46-a580-0e8e130791a1
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 01/31/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: df7fc366c5087e66c3022c212870397d77e6e34d
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77046754"
---
# <a name="tutorial-integrate-dropbox-for-business-with-azure-active-directory"></a>Öğretici: Dropbox for Business'ı Azure Active Directory ile tümleştirin

Bu eğitimde, Dropbox for Business'ı Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. Dropbox for Business'ı Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* İş Için Dropbox erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Otomatik olarak Dropbox'ta İş Yeri oturum açmasını etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/)bir aylık ücretsiz deneme sürümü alabilirsiniz.
* Business için Dropbox tek oturum açma (SSO) aboneliği ni etkinleştirildi.

## <a name="scenario-description"></a>Senaryo açıklaması

* Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz. Dropbox for Business, **SP** tarafından başlatılan SSO'yu destekliyor

* Dropbox for Business [Otomatik kullanıcı sağlama ve deprovisioning](dropboxforbusiness-tutorial.md) destekler
* Dropbox'ı yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak sızma ve sızmalarını koruyan Oturum Denetimi'ni uygulayabilirsiniz. Oturum Denetimi Koşullu Erişim'den genişletir. [Microsoft Cloud App Security ile oturum denetimini nasıl uygulayacağınızı öğrenin](https://docs.microsoft.com/cloud-app-security/proxy-deployment-aad)

## <a name="adding-dropbox-for-business-from-the-gallery"></a>Galeriden İşletmeler için Dropbox Ekleme

Dropbox for Business'ın Azure AD'ye entegrasyonunu yapılandırmak için, galeriden yönetilen SaaS uygulamaları listenize Dropbox for Business'ı eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **İşletmeler için Dropbox** yazın.
1. Sonuç panelinden **Dropbox for Business'ı** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Azure AD SSO'yu Dropbox for Business ile **Britta Simon**adlı bir test kullanıcısı kullanarak yapılandırın ve test edin. SSO'nun çalışması için, Bir Azure AD kullanıcısı ile Dropbox for Business'taki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Dropbox for Business ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**    
    1. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    1. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[İş SSO için Dropbox'ı yapılandırın](#configure-dropbox-for-business-sso)** - uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için.
    1. **[Dropbox for Business test kullanıcısı için Dropbox'ı oluşturun](#create-dropbox-for-business-test-user)** - Kullanıcının Azure AD gösterimine bağlı Dropbox for Business'ta Britta Simon'ın bir örneğine sahip olmak için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında,](https://portal.azure.com/) **Dropbox for Business** uygulaması tümleştirme sayfasında, **Yönet** bölümünü bulun ve Tek **oturum açma'yı**seçin.
1. Tek **oturum açma yöntemi** sayfasında **SAML'yi**seçin.
1. **SAML** ile Tek Oturum Açma'da, ayarları düzenlemek için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırma** sayfasında, aşağıdaki alanların değerlerini girin:

    a. URL metin kutusunda **Oturum Aç** kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://www.dropbox.com/sso/<id>`

    b. Tanımlayıcı **(Entity ID)** metin kutusuna bir değer yazın:`Dropbox`

    > [!NOTE]
    > Önceki Oturum Açma URL değeri gerçek değer değildir. Değeri, daha sonra öğreticide açıklanan gerçek Oturum Açma URL'si ile güncelleştireceksiniz.

1. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, sertifikayı **(Base64)** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. İşletmeler **için Dropbox'ı Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD Tanımlayıcısı

    c. Giriş URL'si


### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, Azure portalında Britta Simon adında bir test kullanıcısı oluşturursunuz.

1. Azure portalındaki sol bölmeden **Azure Etkin Dizini'ni**seçin, **Kullanıcılar'ı**seçin ve ardından **Tüm Kullanıcıları**seçin.
1. Ekranın üst kısmında **Yeni kullanıcı** yı seçin.
1. **Kullanıcı** özelliklerinde aşağıdaki adımları izleyin:
   1. **Ad** alanına `Britta Simon` girin.  
   1. Kullanıcı **adı** alanına. username@companydomain.extension Örneğin, `BrittaSimon@contoso.com`.
   1. **Parolayı Göster** onay kutusunu seçin ve ardından **Parola** kutusunda görüntülenen değeri yazın.
   1. **Oluştur'u**tıklatın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Britta Simon'ın Dropbox for Business'a erişim sağlayarak Azure tek oturum açma işlemini kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde, İşletmeler **için Dropbox'ı**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-dropbox-for-business-sso"></a>Business SSO için Dropbox'ı yapılandır

1. Dropbox for Business içindeki yapılandırmayı otomatikleştirmek için, **uzantıyı**yükleyin'i tıklatarak **Uygulamalarım Güvenli Oturum Açma tarayıcı uzantısını** yüklemeniz gerekir.

    ![Uygulamalar uzantım](common/install-myappssecure-extension.png)

2. Tarayıcıya uzantı ekledikten sonra, **İş için Kurulum Dropbox'a** tıklayın ve sizi İş için Dropbox uygulamasına yönlendirecektir. Buradan, Dropbox for Business'ta oturum açabilmek için yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı uygulamayı sizin için otomatik olarak yapılandıracak ve 3-8 adımlarını otomatikleştirecektir.

    ![Kurulum yapılandırması](common/setup-sso.png)

3. Dropbox for Business'ı el ile kurmak istiyorsanız, yeni bir web tarayıcısı penceresi açın ve Business kiracıiçin Dropbox'ınıza gidin ve iş kiracınız için Dropbox'ınızda oturum açın. ve aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırma](./media/dropboxforbusiness-tutorial/ic769509.png "Çoklu oturum açmayı yapılandırma")

4. **Kullanıcı Simgesi'ne** tıklayın ve **Ayarlar** sekmesini seçin.

    ![Çoklu oturum açmayı yapılandırma](./media/dropboxforbusiness-tutorial/configure1.png "Çoklu oturum açmayı yapılandırma")

5. Sol taraftaki gezinti bölmesinde **Yönetici konsoluna**tıklayın.

    ![Çoklu oturum açmayı yapılandırma](./media/dropboxforbusiness-tutorial/configure2.png "Çoklu oturum açmayı yapılandırma")

6. Yönetici **konsolunda,** sol daki gezinti bölmesinde **Ayarlar'ı** tıklatın.

    ![Çoklu oturum açmayı yapılandırma](./media/dropboxforbusiness-tutorial/configure3.png "Çoklu oturum açmayı yapılandırma")

7. **Kimlik Doğrulama** bölümünün altında Tek **oturum açma** seçeneğini seçin.

    ![Çoklu oturum açmayı yapılandırma](./media/dropboxforbusiness-tutorial/configure4.png "Çoklu oturum açmayı yapılandırma")

8. Tek **oturum açma** bölümünde aşağıdaki adımları gerçekleştirin:  

    ![Çoklu oturum açmayı yapılandırma](./media/dropboxforbusiness-tutorial/configure5.png "Çoklu oturum açmayı yapılandırma")

    a. **Tek oturum açma**için açılır dosyadan seçenek olarak **Gerekli'yi** seçin.

    b. Oturum **açma URL'sini ekle'ye** tıklayın ve **Kimlik sağlayıcı oturum açma URL** metin kutusuna tıklayın, Azure portalından kopyaladığınız Giriş **URL** değerini yapıştırın ve **ardından Bitti'yi**seçin.

    ![Çoklu oturum açmayı yapılandırma](./media/dropboxforbusiness-tutorial/configure6.png "Çoklu oturum açmayı yapılandırma")

    c. **Sertifikayükle'yi**tıklatın ve ardından Azure portalından indirdiğiniz **Base64 kodlanmış sertifika dosyanıza** göz atın.

    d. **Kopyala bağlantısını** tıklatın ve kopyalanan değeri Azure portalındaki **Dropbox for Business Domain ve URL'ler** bölümüne **oturum aç'ın URL** metin kutusuna yapıştırın.

    e. **Kaydet**'e tıklayın.

### <a name="create-dropbox-for-business-test-user"></a>İş testi kullanıcısı için Dropbox oluşturma

Bu bölümde, Dropbox for Business'ta Britta Simon adında bir kullanıcı oluşturulur. Dropbox for Business, varsayılan olarak etkinleştirilen tam zamanında kullanıcı sağlamayı destekler. Bu bölümde sizin için bir eylem öğesi yoktur. Bir kullanıcı İş Için Dropbox'ta zaten yoksa, kimlik doğrulamadan sonra yeni bir kullanıcı oluşturulur.

>[!Note]
>El ile bir kullanıcı oluşturmanız gerekiyorsa, [İş İstemci destek ekibi için Dropbox](https://www.dropbox.com/business/contact) ile iletişime geçin

### <a name="test-sso"></a>Test SSO

Erişim Paneli'ndeki Dropbox for Business kutularını seçtiğinizde, SSO'yu kurduğunuz İş Için Dropbox'ta otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [Dropbox'ı gelişmiş görünürlük ve kontrollerle nasıl koruyabilirsiniz?](https://docs.microsoft.com/cloud-app-security/protect-dropbox)