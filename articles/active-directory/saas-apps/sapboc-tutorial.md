---
title: 'Öğretici: SAP Analytics Cloud ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve SAP Analytics Cloud arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 6c5e44f0-4e52-463f-b879-834d80a55cdf
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 07/17/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1fd01c86086c7491d22f655fcba5da237286412f
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "68347790"
---
# <a name="tutorial-integrate-sap-analytics-cloud-with-azure-active-directory"></a>Öğretici: SAP Analytics Cloud'u Azure Active Directory ile tümleştirin

Bu eğitimde, SAP Analytics Cloud'u Azure Active Directory (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. SAP Analytics Cloud'u Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* SAP Analytics Cloud erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla SAP Analytics Cloud'da otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* SAP Analytics Cloud tek oturum açma (SSO) aboneliği ni etkinleştirdi.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* SAP Analytics **Cloud, SP** tarafından başlatılan SSO'ya destek verdi

## <a name="adding-sap-analytics-cloud-from-the-gallery"></a>Galeriden SAP Analytics Cloud ekleme

SAP Analytics Cloud'un Azure AD'ye entegrasyonunu yapılandırmak için galeriden SAP Analytics Cloud'u yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **SAP Analytics Cloud** yazın.
1. Sonuç panelinden **SAP Analytics Cloud'u** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.


## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

**B.Simon**adlı bir test kullanıcısı kullanarak Azure AD SSO'nu SAP Analytics Cloud ile yapılandırın ve test edin. SSO'nun çalışması için, SAP Analytics Cloud'daki bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu SAP Analytics Cloud ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
2. **[Sap Analytics Cloud SSO'yu uygulama](#configure-sap-analytics-cloud-sso)** tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için yapılandırın.
3. Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[SAP Analytics Cloud test kullanıcısını oluşturun](#create-sap-analytics-cloud-test-user)** - kullanıcının Azure AD gösterimine bağlı SAP Analytics Cloud'da B.Simon'ın bir muadili olması için.
6. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. AZURE [portalında,](https://portal.azure.com/) **SAP Analytics Cloud** uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve Tek **oturum açma'yı**seçin.
1. Tek **oturum açma yöntemi** sayfasında **SAML'yi**seçin.
1. **SAML** ile Tek Oturum Açma'da, ayarları düzenlemek için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde, aşağıdaki alanların değerlerini girin:

    a. URL metin kutusunda **Oturum Aç** kutusuna, aşağıdaki deseni kullanarak bir URL yazın:

    | |
    |-|-|
    | `https://<sub-domain>.sapanalytics.cloud/` |
    | `https://<sub-domain>.sapbusinessobjects.cloud/` |

    b. Tanımlayıcı **(Entity ID)** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:
    
    | |
    |-|-|
    | `<sub-domain>.sapbusinessobjects.cloud` |
    | `<sub-domain>.sapanalytics.cloud` |

    > [!NOTE] 
    > Bu URL'lerde değerler yalnızca gösteri içindir. Değerleri gerçek oturum açma URL'si ve tanımlayıcı URL'si ile güncelleştirin. Oturum açma URL'sini almak için [SAP Analytics Cloud Client destek ekibine](https://help.sap.com/viewer/product/SAP_BusinessObjects_Cloud/release/)başvurun. Yönetici konsolundan SAP Analytics Cloud meta verilerini indirerek tanımlayıcı URL'sini alabilirsiniz. Bu daha sonra öğretici açıklanmıştır.

4. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, Federation **Metadata XML'i** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

6. SAP **Analytics Cloud'u Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

### <a name="configure-sap-analytics-cloud-sso"></a>SAP Analytics Cloud SSO'ya yapılandır

1. Farklı bir web tarayıcısı penceresinde, SAP Analytics Cloud şirket sitenizde yönetici olarak oturum açın.

2. **Menü** > Sistem**Yönetimi'ni****System** > seçin.
    
    ![Menü'yü seçin, ardından Sistem'i ve ardından Yönetim](./media/sapboc-tutorial/config1.png)

3. **Güvenlik** **sekmesinde, Edit** (kalem) simgesini seçin.
    
    ![Güvenlik sekmesinde, Edit simgesini seçin](./media/sapboc-tutorial/config2.png)  

4. **Kimlik Doğrulama Yöntemi**için **SAML Tek Oturum Açma (SSO) seçeneğini belirleyin.**

    ![Kimlik doğrulama yöntemi için SAML Tek Oturum Açma'yı seçin](./media/sapboc-tutorial/config3.png)  

5. Servis sağlayıcı meta verilerini (Adım 1) indirmek için **İndir'i**seçin. Meta veri dosyasında **entityID** değerini bulun ve kopyalayın. Azure portalında, **Temel SAML Yapılandırma** iletişim kutusunda, Değeri **Tanımlayıcı** kutusuna yapıştırın.

    ![EntityID değerini kopyalayıp yapıştırın](./media/sapboc-tutorial/config4.png)  

6. Azure portalından indirdiğiniz dosyaya servis sağlayıcı meta verilerini (Adım 2) **Yüklemek Kimlik Sağlayıcısı meta verileri**altında yüklemek için **Yükleyin'i**seçin.  

    ![Kimlik Sağlayıcısı Yükle meta verileri altında, Yükle'yi seçin](./media/sapboc-tutorial/config5.png)

7. Kullanıcı **Özniteliği** listesinde, uygulamanız için kullanmak istediğiniz kullanıcı özniteliğini (Adım 3) seçin. Bu kullanıcı eşlemleri kimlik sağlayıcısına atfedebilir. Kullanıcının sayfasına özel bir öznitelik girmek için **Özel SAML Eşleme** seçeneğini kullanın. Veya, kullanıcı özniteliği olarak **E-posta** veya **USER ID'yi** seçebilirsiniz. Örneğimizde, Azure portalındaki **Kullanıcı Öznitelikleri & Talepler** bölümünde ki **kullanıcı ilkesi özniteliği** ile kullanıcı tanımlayıcı iddiasını eşlediğimiz için **E-posta'yı** seçtik. Bu, her başarılı SAML yanıtında SAP Analytics Cloud uygulamasına gönderilen benzersiz bir kullanıcı e-postası sağlar.

    ![Kullanıcı Özniteliği ni seçin](./media/sapboc-tutorial/config6.png)

8. Hesabı kimlik sağlayıcısına (Adım 4) doğrulamak **için, Giriş Kimlik Belgesi (E-posta)** kutusunda kullanıcının e-posta adresini girin. Ardından, **Hesabı Doğrula'yı**seçin. Sistem kullanıcı hesabına oturum açma kimlik bilgileri ekler.

    ![E-posta girin ve Hesabı Doğrula'yı seçin](./media/sapboc-tutorial/config7.png)

9. **Kaydet** simgesini seçin.

    ![Simgeyi kaydet](./media/sapboc-tutorial/save.png)

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

Bu bölümde, SAP Analytics Cloud'a erişim sağlayarak B.Simon'ın Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **SAP Analytics Cloud'u**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

### <a name="create-sap-analytics-cloud-test-user"></a>SAP Analytics Cloud test kullanıcısı oluşturun

Azure AD kullanıcılarının SAP Analytics Cloud'da oturum açabilmesi için SAP Analytics Cloud'da sağlanması gerekir. SAP Analytics Cloud'da sağlama el ile bir görevdir.

Bir kullanıcı hesabı sağlamak için:

1. SAP Analytics Cloud şirket sitenizde yönetici olarak oturum açın.

2. **Menü** > **Güvenlik** > **Kullanıcıları'nı**seçin.

    ![Çalışan Ekle](./media/sapboc-tutorial/user1.png)

3. **Kullanıcılar** sayfasında, yeni kullanıcı ayrıntıları eklemek **+** için. 

    ![Kullanıcı Ekle sayfası](./media/sapboc-tutorial/user4.png)

    Ardından, aşağıdaki adımları tamamlayın:

    a. **KULLANICI KİmLİğİ** kutusuna, Kullanıcı kimliğini **B**gibi girin.

    b. First **NAME** kutusuna, **B**gibi kullanıcının ilk adını girin.

    c. LAST **NAME** kutusuna, **Simon**gibi kullanıcının soyadını girin.

    d. DISPLAY **NAME** kutusuna, **B.Simon**gibi kullanıcının tam adını girin.

    e. E-POSTA kutusuna, kullanıcının **e-posta** adresini `b.simon@contoso.com`girin, gibi.

    f. Rolleri **Seç** sayfasında, kullanıcı için uygun rolü seçin ve ardından **Tamam'ı**seçin.

      ![Rol seç](./media/sapboc-tutorial/user3.png)

    g. **Kaydet** simgesini seçin.

### <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki SAP Analytics Cloud döşemesini tıklattığınızda, SSO'yu kurduğunuz SAP Analytics Cloud'da otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

