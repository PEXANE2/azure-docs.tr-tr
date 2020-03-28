---
title: 'Öğretici: Zscaler Beta ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Zscaler Beta arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 56b846ae-a1e7-45ae-a79d-992a87f075ba
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/24/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 07b0f8112f724c857ffb46378f7aa7ef605b9bbb
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "68943282"
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-beta"></a>Öğretici: Zscaler Beta ile Azure Active Directory entegrasyonu

Bu eğitimde, Zscaler Beta'yı Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
Zscaler Beta'yı Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD'de Zscaler Beta erişimi olan denetim.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla Zscaler Beta'da otomatik olarak oturum açabilmelerine izin verin. Bu erişim denetimine tek oturum açma (SSO) adı verilir.
* Azure portalını kullanarak hesaplarınızı tek bir merkezi konumda yönetin.

Azure AD ile hizmet olarak yazılım (SaaS) uygulama tümleştirmesi hakkında daha fazla bilgi [için](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bkz.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Zscaler Beta ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız [yoksa, ücretsiz](https://azure.microsoft.com/free/)bir hesap alabilirsiniz.
* Tek oturum açma kullanan bir Zscaler Beta aboneliği.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* Zscaler Beta, SP tarafından başlatılan SSO'ları destekler.
* Zscaler Beta tam zamanında kullanıcı sağlamayı destekler.

## <a name="add-zscaler-beta-from-the-azure-marketplace"></a>Azure Marketinden Zscaler Beta ekleyin

Zscaler Beta'nın Azure AD'ye entegrasyonunu yapılandırmak için Azure Marketi'nden Zscaler Beta'yı yönetilen SaaS uygulamaları listenize ekleyin.

Azure Marketi'nden Zscaler Beta eklemek için aşağıdaki adımları izleyin.

1. Sol daki gezinti bölmesinde [bulunan Azure portalında](https://portal.azure.com) **Azure Etkin Dizin'i**seçin.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **uygulamalara**gidin ve ardından **Tüm uygulamaları**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için iletişim kutusunun üst kısmında **Yeni uygulama'yı** seçin.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Zscaler Beta**girin. Sonuç panelinden **Zscaler Beta'yı** seçin ve sonra **Ekle'yi**seçin.

     ![Sonuç listesinde Zscaler Beta](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Test kullanıcısı Britta Simon'a göre Azure AD oturum açma işlemini Zscaler Beta ile yapılandırıp test etersiniz.
Tek oturum açma nın çalışması için, Bir Azure AD kullanıcısı ile Zscaler Beta'daki ilgili kullanıcı arasında bir bağlantı ilişkisi kurun.

Azure AD oturumaçmayı Zscaler Beta ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

- Kullanıcılarınızın bu özelliği kullanmasını sağlamak için [Azure AD oturumunu yapılandırın.](#configure-azure-ad-single-sign-on)
- [Zscaler Beta tek oturum](#configure-zscaler-beta-single-sign-on) açma işlemlerini uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için yapılandırın.
- Britta Simon ile Azure AD tek oturum açma işlemini test etmek için [bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)
- Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için [Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)
- Zscaler Beta'daki Britta Simon'ın, kullanıcının Azure AD gösterimine bağlı bir muadili olması için [bir Zscaler Beta test kullanıcısı oluşturun.](#create-a-zscaler-beta-test-user)
- Yapılandırmanın çalışıp çalışmadığını doğrulamak için [tek oturum](#test-single-sign-on) açma yı test edin.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturum açma işlemlerini Zscaler Beta ile yapılandırmak için aşağıdaki adımları izleyin.

1. Azure [portalında](https://portal.azure.com/), **Zscaler Beta** uygulama tümleştirme sayfasında Tek **oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum** açma yöntemi iletişim kutusunu seç'te, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme'yi** seçin.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımı izleyin:

    ![Zscaler Beta etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-intiated.png)

    - **URL'de Oturum Aç** kutusuna, zscaler Beta uygulamanızda oturum açabilmek için kullanıcılarınızın kullandığı URL'yi girin.

    > [!NOTE]
    > Değer gerçek değil. Değeri URL değerindeki gerçek Oturum'la güncelleştirin. Değeri almak için [Zscaler Beta istemci destek ekibine](https://www.zscaler.com/company/contact)başvurun.

5. Zscaler Beta uygulaması, SAML iddialarını belirli bir biçimde bekler. SAML belirteç öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemeniz gerekir. Aşağıdaki ekran görüntüsü varsayılan özniteliklerin listesini gösterir. **Kullanıcı Öznitelikleri** iletişim kutusunu açmak için **Edit'i** seçin.

    ![Kullanıcı Öznitelikleri iletişim kutusu](common/edit-attribute.png)

6. Zscaler Beta uygulaması, SAML yanıtında birkaç özniteliğin daha geçirilmesini bekler. Kullanıcı **Öznitelikleri** iletişim kutusundaki **Kullanıcı** talepleri bölümünde, aşağıdaki tabloda gösterildiği gibi SAML belirteç özniteliğini eklemek için aşağıdaki adımları izleyin.
    
    | Adı | Kaynak özniteliği | 
    | ---------------| --------------- |
    | Memberof  | user.assignedroles |

    a. **Kullanıcı taleplerini yönet** iletişim kutusunu açmak için yeni talep **ekle'yi** seçin.

    ![Kullanıcı talepleri iletişim kutusu](common/new-save-attribute.png)

    ![Kullanıcı talepleri iletişim kutusunu yönetme](common/new-attribute-details.png)

    b. **Ad** kutusuna, bu satır için gösterilen öznitelik adını girin.

    c. Ad **Alanı** kutusunu boş bırakın.

    d. **Kaynak**için, **Öznitelik'i**seçin.

    e. Kaynak **öznitelik** listesinden, bu satır için gösterilen öznitelik değerini girin.

    f. **Tamam'ı**seçin.

    g. **Kaydet'i**seçin.

    > [!NOTE]
    > Azure AD'deki rolleri nasıl yapılandırıştırılabildiğini öğrenmek için [bkz.](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-app-role-management)

7. **SAML** ile Tek Oturum Açma sayfasında, **SAML İmza Sertifikası** bölümünde Sertifikayı indirmek için **İndir'i (Base64)** seçin. **Download** Bilgisayarınıza kaydedin.

    ![Sertifika indirme linki](common/certificatebase64.png)

8. **Zscaler Beta'yı Ayarla** bölümünde, gereksinimleriniz için ihtiyacınız olan URL'leri kopyalayın:

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    - Oturum Açma URL’si
    - Azure AD Tanımlayıcısı
    - Giriş URL'si

### <a name="configure-zscaler-beta-single-sign-on"></a>Zscaler Beta tek oturum açma yı yapılandır

1. Zscaler Beta içindeki yapılandırmayı otomatikleştirmek için, uzantıyı yükleyin'i seçerek **Uygulamalarım Güvenli Oturum Açma tarayıcı uzantısını** **yükleyin.**

    ![Uygulamalarım uzantısı](common/install-myappssecure-extension.png)

2. Uzantıyı tarayıcıya ekledikten sonra, **Zscaler Beta'yı** ayarla'yı seçerek sizi Zscaler Beta uygulamasına yönlendirir. Buradan, Zscaler Beta'da oturum açacak yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı uygulamayı sizin için otomatik olarak yapılandırır ve 3'ten 6'ya kadar olan adımları otomatikleştirir.

    ![Kurulum yapılandırması](common/setup-sso.png)

3. Zscaler Beta'yı el ile ayarlamak için yeni bir web tarayıcısı penceresi açın. Zscaler Beta şirket sitenizde yönetici olarak oturum açın ve aşağıdaki adımları izleyin.

4. **Yönetim** > **Kimlik Doğrulama** > **Kimlik Doğrulama Ayarları'na**gidin ve aşağıdaki adımları izleyin.
   
    ![Yönetim](./media/zscaler-beta-tutorial/ic800206.png "Yönetim")

    a. **Kimlik Doğrulama Türü**altında **SAML'yi**seçin.

    b. **SAML'yi Yapılandır'ı**seçin.

5. **SAML'yi Edit** penceresinde aşağıdaki adımları izleyin: 
            
    ![Kullanıcıları & Kimlik Doğrulamayı Yönetin](./media/zscaler-beta-tutorial/ic800208.png "Kullanıcıları & Kimlik Doğrulamayı Yönetin")
    
    a. **SAML Portal URL** kutusuna, Azure portalından kopyaladığınız **Giriş URL'sini** yapıştırın.

    b. Giriş **Adı Özniteliği** kutusuna **NameID'i**girin.

    c. Herkese **Açık SSL Sertifikası** kutusunda, Azure portalından indirdiğiniz Azure SAML imza sertifikasını yüklemek için **Yükle'yi** seçin.

    d. GEÇIŞ **Etkinleştir MESLEK Otomatik Sağlama.**

    e. Kullanıcı **Görüntülü Reklam Adı Öznitelik** kutusuna, displayName öznitelikleri için SAML otomatik sağlamayı etkinleştirmek istiyorsanız **displayName'yi** girin.

    f. Grup **Adı Özniteliği** kutusuna, üyeOf öznitelikleri için SAML otomatik sağlamayı etkinleştirmek istiyorsanız **üyeOf** girin.

    g. Bölüm **Adı Özniteliği** kutusuna, bölüm öznitelikleri için SAML otomatik sağlamayı etkinleştirmek istiyorsanız **bölümü** girin.

    h. **Kaydet'i**seçin.

6. Kullanıcı **Kimlik Doğrulaması'nı Yapılandırma** iletişim sayfasında aşağıdaki adımları izleyin:

    ![Etkinleştirme menüsü ve Etkinleştir düğmesi](./media/zscaler-beta-tutorial/ic800207.png)

    a. Sol alttaki **Etkinleştirme** menüsünün üzerine titreyin.

    b. **Etkinleştir'i**seçin.

## <a name="configure-proxy-settings"></a>Ara sunucu ayarlarını yapılandırma
Internet Explorer'daki proxy ayarlarını yapılandırmak için aşağıdaki adımları izleyin.

1. **Internet Explorer'ı başlatın.**

2. **Internet Seçenekleri** iletişim kutusunu açmak için **Araçlar** menüsünden **Internet seçeneklerini** seçin. 
    
     ![Internet Seçenekleri iletişim kutusu](./media/zscaler-beta-tutorial/ic769492.png "İnternet Seçenekleri")

3. **Bağlantılar** sekmesini seçin. 
  
     ![Bağlantılar sekmesi](./media/zscaler-beta-tutorial/ic769493.png "Bağlantılar")

4. Yerel Alan Ağı **(LAN) Ayarları** iletişim kutusunu açmak için LAN **ayarlarını** seçin.

5. Proxy **sunucusu** bölümünde aşağıdaki adımları izleyin: 
   
    ![Proxy sunucu bölümü](./media/zscaler-beta-tutorial/ic769494.png "Proxy sunucu")

    a. LAN onay **kutunuz için proxy sunucusu kullan'ı** seçin.

    b. **Adres** kutusuna **ağ geçidini girin. Zscaler Beta.net**.

    c. Bağlantı **Noktası** kutusuna **80**girin.

    d. Yerel adresler onay kutusu **için Bypass proxy sunucusunu** seçin.

    e. **Yerel Alan Ağı (LAN) Ayarları** iletişim kutusunu kapatmak için **Tamam'ı** seçin.

6. **Internet Seçenekleri** iletişim kutusunu kapatmak için **Tamam'ı** seçin.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma 

Azure portalında Britta Simon adında bir test kullanıcısı oluşturun.

1. Azure portalında, sol bölmede **Azure Etkin Dizin** > **Kullanıcıları** > **Tüm kullanıcıları**seçin.

    ![Kullanıcılar ve Tüm kullanıcı bağlantıları](common/users.png)

2. Ekranın üst kısmında **Yeni kullanıcı** yı seçin.

    ![Yeni kullanıcı düğmesi](common/new-user.png)

3. **Kullanıcı** iletişim kutusunda aşağıdaki adımları izleyin:

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** **kutusuna BrittaSimon**girin.
  
    b. **Kullanıcı adı** kutusuna `brittasimon@yourcompanydomain.extension` girin. BrittaSimon@contoso.com bunun bir örneğidir.

    c. **Parolayı göster** onay kutusunu seçin. **Parola** kutusuna görünen değeri yazın.

    d. **Oluştur'u**seçin.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Britta Simon'ın Zscaler Beta'ya erişim sağlayarak Azure tek oturum açma işlemini kullanmasını etkinleştirin.

1. Azure portalında Kurumsal **uygulamaları** > seçin**Tüm uygulamalar** > **Zscaler Beta.**

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesine **Zscaler Beta'yı**girin ve seçin.

    ![Uygulamalar listesindeZscaler Beta bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    ![Kullanıcılar ve gruplar bağlantısı](common/users-groups-blade.png)

4. **Kullanıcı Ekle'yi**seçin. Atama **Ekle** iletişim **kutusunda, Kullanıcılar ve gruplar**seçin.

    ![Kullanıcı düğmesi ekle](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, listeden **Britta Simon** gibi kullanıcıyı seçin. Ardından ekranın alt kısmında **Seç'i** seçin.

    ![Kullanıcılar ve gruplar iletişim kutusu](./media/zscaler-beta-tutorial/tutorial_zscalerbeta_users.png)

6. Rolü **Seç** iletişim kutusunda, listedeki uygun kullanıcı rolünü seçin. Ardından ekranın alt kısmında **Seç'i** seçin.

    ![Rol iletişim kutusunu seçin](./media/zscaler-beta-tutorial/tutorial_zscalerbeta_roles.png)

7. Atama **Ekle** iletişim kutusunda **Atama'yı**seçin.

    ![Atama iletişim kutusu ekle](./media/zscaler-beta-tutorial/tutorial_zscalerbeta_assign.png)

### <a name="create-a-zscaler-beta-test-user"></a>Zscaler Beta test kullanıcısı oluşturma

Bu bölümde, kullanıcı Britta Simon Zscaler Beta oluşturulur. Zscaler Beta, varsayılan olarak etkinleştirilen **tam zamanında kullanıcı sağlamayı**destekler. Bu bölümde yapacak bir şey yok. Zscaler Beta'da bir kullanıcı zaten yoksa, kimlik doğrulamadan sonra yeni bir kullanıcı oluşturulur.

>[!Note]
>Bir kullanıcıyı el ile oluşturmak için [Zscaler Beta destek ekibine](https://www.zscaler.com/company/contact)başvurun.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı test edin.

Erişim Paneli'ndeki Zscaler Beta döşemesini seçtiğinizde, SSO'yu kurduğunuz Zscaler Beta'da otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamalarının Azure Active Directory ile nasıl entegre edilebildiğini anlatan öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)
- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)
- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

