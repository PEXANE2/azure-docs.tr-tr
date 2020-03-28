---
title: 'Öğretici: iLMS ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve iLMS arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: d6e11639-6cea-48c9-b008-246cf686e726
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/14/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 50097aec1c4a003d3494029e8f25bb13b564f207
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "68944014"
---
# <a name="tutorial-integrate-ilms-with-azure-active-directory"></a>Öğretici: iLMS'leri Azure Active Directory ile tümleştirin

Bu eğitimde, iLMS'leri Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. iLMS'leri Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* azure AD'de iLMS erişimi olan denetimi denetler.
* Kullanıcılarınızın Azure AD hesaplarıyla iLMS'de otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/)bir aylık ücretsiz deneme sürümü alabilirsiniz.
* iLMS tek oturum açma (SSO) özellikli abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz. iLMS, **SP ve IDP** tarafından başlatılan SSO'ya destek veriyor

## <a name="adding-ilms-from-the-gallery"></a>Galeriden iLMS ekleme

iLMS'lerin Azure AD'ye entegrasyonunu yapılandırmak için galerideki iLMS'leri yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeribölümünden Ekle** bölümünde, arama kutusuna **iLMS** yazın.
1. Sonuç panelinden **iLMS'i** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Azure AD SSO'nu **britta Simon**adlı bir test kullanıcısı kullanarak iLMS ile yapılandırın ve test edin. SSO'nun çalışması için, bir Azure AD kullanıcısı ile iLMS'deki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu iLMS ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
2. Uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için **[iLMS SSO'yu yapılandırın.](#configure-ilms-sso)**
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[ILMS test kullanıcısını oluşturun](#create-ilms-test-user)** - kullanıcının Azure AD gösterimine bağlı iLMS'deki Britta Simon'ın bir örneğine sahip olmak için.
6. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında](https://portal.azure.com/), **iLMS** uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve **Tek oturum açma'yı**seçin.
1. Tek **oturum açma yöntemi** sayfasında **SAML'yi**seçin.
1. **SAML** ile Tek Oturum Açma'da, ayarları düzenlemek için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırma** sayfasında, uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız, aşağıdaki alanların değerlerini girin:

    a. **Tanımlayıcı** metin kutusuna, iLMS yönetici portalında SAML ayarlarının **Servis Sağlayıcı** bölümünden kopyaladığınız Tanımlayıcı değerini yapıştırın. **Identifier**

    b. **Yanıtla URL** metin kutusuna, aşağıdaki deseni sahip iLMS yönetici portalında SAML ayarlarının **Servis Sağlayıcı** bölümünden kopyaladığınız Bitiş **Noktası (URL)** değerini yapıştırın`https://www.inspiredlms.com/Login/<instanceName>/consumer.aspx`

1. Uygulamayı **SP** başlatılan modda yapılandırmak istiyorsanız **ek URL'ler ayarla'yı** tıklatın ve aşağıdaki adımı gerçekleştirin:

    Oturum **Açma URL** metin kutusuna, iLMS yönetici portalındaki SAML ayarlarının **Servis Sağlayıcı** bölümünden kopyaladığınız Bitiş **Noktası (URL)** değerini yapıştırın`https://www.inspiredlms.com/Login/<instanceName>/consumer.aspx`

1. JIT sağlamayı etkinleştirmek için, iLMS uygulamanız SAML belirteç öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML iddiaları bekler. Aşağıdaki ekran görüntüsü varsayılan özniteliklerin listesini gösterir. Kullanıcı Öznitelikleri iletişim kutusunu açmak için **Edit** simgesini tıklatın.

    > [!NOTE]
    > Bu öznitelikleri eşlemek için iLMS'de **tanınmayan Kullanıcı Hesabı Oluşturma'yı** etkinleştirmeniz gerekir. Öznitelikler yapılandırması hakkında bir fikir almak için [buradaki](https://support.inspiredelearning.com/help/adding-updating-and-managing-users#just-in-time-provisioning-with-saml-single-signon) yönergeleri izleyin.

1. Yukarıdakilere ek olarak, iLMS uygulaması SAML yanıtında birkaç özniteliğin daha geçirilmesini bekler. **Kullanıcı Öznitelikleri** iletişim kutusundaki **Kullanıcı Talepleri** bölümünde, aşağıdaki tabloda gösterildiği gibi SAML belirteç özniteliği eklemek için aşağıdaki adımları gerçekleştirin:

    | Adı | Kaynak Özniteliği|
    | --------|------------- |
    | Bölümü | user.department |
    | region | user.state |
    | bölüm | user.jobtitle |

    a. **Kullanıcı taleplerini yönet** iletişim kutusunu açmak için yeni **talep ekle'yi** tıklatın.

    b. **Ad** metin kutusunda, bu satır için gösterilen öznitelik adını yazın.

    c. Ad **alanını** boş bırakın.

    d. **Kaynak'ı Öznitelik**olarak seçin.

    e. Kaynak **öznitelik** listesinden, bu satır için gösterilen öznitelik değerini yazın.

    f. **Tamam'ı** tıklatın

    g. **Kaydet**'e tıklayın.

1. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, Federasyon **Metadata XML'ini** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

1. **iLMS'yi Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-ilms-sso"></a>iLMS SSO'da yapılandırın

1. Farklı bir web tarayıcısı penceresinde, yönetici olarak **iLMS yönetici portalınızda** oturum açın.

2. SAML ayarlarını açmak ve aşağıdaki adımları gerçekleştirmek için **Ayarlar** sekmesinin altındaki **SSO:SAML'yi** tıklatın:

    ![Tek İşaret-On'u Yapılandır](./media/ilms-tutorial/1.png)

3. Hizmet **Sağlayıcısı** bölümünü genişletin ve **Tanımlayıcı** ve **Bitiş Noktası (URL)** değerini kopyalayın.

    ![Tek İşaret-On'u Yapılandır](./media/ilms-tutorial/2.png) 

4. **Kimlik Sağlayıcı** lar bölümünde Meta verilerini **içe aktar'ı**tıklatın.

5. **SAML İmzaLama Sertifikası** bölümünden Azure portalından indirilen **Federasyon Meta veri** dosyasını seçin.

    ![Tek İşaret-On'u Yapılandır](./media/ilms-tutorial/tutorial_ilms_ssoconfig1.png)

6. Tanımadığınız kullanıcılar için iLMS hesapları oluşturmak için JIT sağlamayı etkinleştirmek istiyorsanız, aşağıdaki adımları izleyin:

    a. **Tanınmayan Kullanıcı Hesabı Oluştur'u**denetleyin.

    ![Tek İşaret-On'u Yapılandır](./media/ilms-tutorial/tutorial_ilms_ssoconfig2.png)

    b. Azure AD'deki öznitelikleri iLMS'deki özniteliklerle eşle. Öznitelik sütununda özniteliklerin adını veya varsayılan değerini belirtin.

    c. İş **Kuralları** sekmesine gidin ve aşağıdaki adımları gerçekleştirin:

    ![Tek İşaret-On'u Yapılandır](./media/ilms-tutorial/5.png)

    d. Tek Oturum Açma sırasında zaten var olmayan Bölgeler, Bölümler ve Bölümler oluşturmak için **tanınmayan Bölgeler, Bölümler** ve Bölümler Oluşturma'yı denetleyin.

    e. **Oturum Açma sırasında Kullanıcı Profilini Güncelleştir'i** kontrol edin ve kullanıcıprofilinin her Tek Oturum Açma ile güncelleştirilip güncelleştirilmediğini belirtin.

    f. Kullanıcı **Profili'ndeki Zorunlu Olmayan Alanlar için Boş Değerleri Güncelleştir** seçeneği işaretlenirse, oturum açken boş olan isteğe bağlı profil alanları da kullanıcının iLMS profilinin bu alanlar için boş değerler içermesi ne neden olur.

    g. **Hata Bildirimi E-postası Gönder'i** kontrol edin ve hata bildirimi e-postasını almak istediğiniz kullanıcının e-postasını girin.

7. Ayarları kaydetmek için **Kaydet** düğmesini tıklatın.

    ![Tek İşaret-On'u Yapılandır](./media/ilms-tutorial/save.png)

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

Bu bölümde, Britta Simon'ın iLMS'ye erişim sağlayarak Azure tek oturum açma işlemini kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **iLMS'yi**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

### <a name="create-ilms-test-user"></a>iLMS test kullanıcısı oluşturma

Uygulama tam zamanında kullanıcı sağlama ve kimlik doğrulama kullanıcıları otomatik olarak uygulamada oluşturulur sonra destekler. JIT, iLMS yönetici portalında SAML yapılandırma ayarı sırasında **tanınmayan Kullanıcı Hesabı Oluştur** onay kutusunu tıklattıysanız çalışır.

Bir kullanıcıyı el ile oluşturmanız gerekiyorsa, aşağıdaki adımları izleyin:

1. yönetici olarak iLMS şirket sitenizde oturum açın.

2. KullanıcıYı Kaydedin sayfasını açmak için **Kullanıcı** sekmesi altında **Kullanıcıyı Kaydedin'i** tıklatın. **Register User**

   ![Çalışan Ekle](./media/ilms-tutorial/3.png)

3. **Kullanıcıyı Kaydol** sayfasında aşağıdaki adımları gerçekleştirin.

    ![Çalışan Ekle](./media/ilms-tutorial/create_testuser_add.png)

    a. Ad **metin** kutusuna, Britta gibi ilk adı yazın.

    b. **Soyadı** metin kutusuna, Simon gibi soyadını yazın.

    c. **E-posta Kimliği** metin kutusuna, kullanıcının BrittaSimon@contoso.come-posta adresini şöyle yazın.

    d. **Bölge** açılır düşüşünde, bölge değerini seçin.

    e. **Bölüm** açılır düşüşünde, bölüm değerini seçin.

    f. **Bölüm** açılır bölümünde, bölüm değerini seçin.

    g. **Kaydet**'e tıklayın.

    > [!NOTE]
    > **Kayıt Postası Gönder** onay kutusunu seçerek kullanıcıya kayıt postası gönderebilirsiniz.

### <a name="test-sso"></a>Test SSO

Access Paneli'ndeki iLMS döşemesini seçtiğinizde, SSO'yu kurduğunuz iLMS'de otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
