---
title: 'Öğretici: Pega Systems ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Bu eğitimde, Azure Active Directory ve Pega Systems arasında tek oturum açma işlemlerini nasıl yapılandırabileceğinizi öğreneceksiniz.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 31acf80f-1f4b-41f1-956f-a9fbae77ee69
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/26/2019
ms.author: jeedes
ms.openlocfilehash: bd54466e054055ff84cd5bb2b28c5cc074ac0017
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "72026813"
---
# <a name="tutorial-azure-active-directory-integration-with-pega-systems"></a>Öğretici: Pega Systems ile Azure Active Directory entegrasyonu

Bu eğitimde, Pega Systems'i Azure Active Directory (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.

Bu tümleştirme şu avantajları sağlar:

* Pega Systems'e kimlerin erişebileceğini denetlemek için Azure AD'yi kullanabilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla Pega Systems'de (tek oturum açma) otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz: Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek [için Azure Active Directory'deki uygulamalarda tek](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)oturum açma'ya bakın.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Pega Systems ile yapılandırmak için şunları yapmanız gerekir:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [bir aylık deneme](https://azure.microsoft.com/pricing/free-trial/)sürümüne kaydolabilirsiniz.
* Tek oturum açma özelliği etkin olan bir Pega Systems aboneliği.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD oturum açma işlemlerini bir test ortamında yapılandıracak ve sınayacaksınız.

* Pega Systems, SP tarafından başlatılan ve IdP tarafından başlatılan SSO'ya destek vermektedir.

## <a name="add-pega-systems-from-the-gallery"></a>Galeriden Pega Sistemleri ekleyin

Pega Systems'in Azure AD'ye entegrasyonunu ayarlamak için, galeriden Pega Systems'i yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com), sol bölmede, **Azure Etkin Dizini'ni**seçin:

    ![Azure Active Directory'yi seçin](common/select-azuread.png)

2. Kurumsal **uygulamalara** > git**Tüm uygulamalar**.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

3. Uygulama eklemek için pencerenin üst kısmında **Yeni uygulama'yı** seçin:

    ![Yeni uygulama seçin](common/add-new-app.png)

4. Arama **kutusuna Pega Systems'i**girin. Arama sonuçlarında **Pega Systems'i** seçin ve sonra **Ekle'yi**seçin.

     ![Arama sonuçları](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Britta Simon adlı bir test kullanıcısı kullanarak Azure AD oturumaçma işlemini Pega Systems ile yapılandıracak ve sınayacaksınız.
Tek oturum açmayı etkinleştirmek için, Bir Azure REKLAM kullanıcısı ile Pega Systems'deki ilgili kullanıcı arasında bir ilişki kurmanız gerekir.

Azure AD oturumlarını Pega Systems ile yapılandırmak ve test etmek için şu adımları tamamlamanız gerekir:

1. Azure AD oturumunu, kullanıcılarınız için özelliği etkinleştirmek için **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. Uygulama tarafında **[Pega Systems'i tek oturum](#configure-pega-systems-single-sign-on)** aç'ı yapılandırın.
3. Azure AD tek oturum açma'yı test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Azure AD test kullanıcısını, kullanıcı için Azure AD oturum açmayı etkinleştirmek için **[atayın.](#assign-the-azure-ad-test-user)**
5. Kullanıcının Azure AD gösterimine bağlı **[bir Pega Systems test kullanıcısı oluşturun.](#create-a-pega-systems-test-user)**
6. Yapılandırmanın çalıştığını doğrulamak için **[tek oturum](#test-single-sign-on)** açma yı test edin.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturumaçmayı Pega Systems ile yapılandırmak için şu adımları izleyin:

1. Azure [portalında](https://portal.azure.com/), **Pega Systems** uygulama tümleştirme sayfasında **Tek oturum açma**seçeneğini belirleyin:

    ![Tek oturum açma'yı seçin](common/select-sso.png)

2. Tek **oturum** açma yöntemi iletişim kutusunu seç'te, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin:

    ![Tek bir oturum açma yöntemi seçin](common/select-saml-option.png)

3. **SAML** ile Tek Oturum Açma'da, **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini seçin:

    ![Düzenle simgesi](common/edit-urls.png)

4. Temel **SAML Yapılandırma** iletişim kutusunda, uygulamayı IdP tarafından başlatılan modda yapılandırmak istiyorsanız, aşağıdaki adımları tamamlayın.

    ![Temel SAML Yapılandırma iletişim kutusu](common/idp-intiated.png)

    1. **Tanımlayıcı** kutusuna, bu desene bir URL girin:

       `https://<customername>.pegacloud.io:443/prweb/sp/<instanceID>`

    1. **Yanıtla URL** kutusuna, bu desene bir URL girin:

       `https://<customername>.pegacloud.io:443/prweb/PRRestService/WebSSO/SAML/AssertionConsumerService`

5. Uygulamayı SP tarafından başlatılan modda yapılandırmak istiyorsanız, **ek URL'ler** ayarla'yı seçin ve aşağıdaki adımları tamamlayın.

    ![Pega Systems Domain ve URL'ler tek oturum açma bilgileri](common/both-advanced-urls.png)

    1. **URL'de Oturum Aç** kutusuna, URL değerindeki işareti girin.

    1. **Röle Durumu** kutusuna, bu desene bir URL girin:`https://<customername>.pegacloud.io/prweb/sso`

    > [!NOTE]
    > Burada sağlanan değerler yer tutuculardır. Gerçek tanımlayıcıyı kullanmanız, URL'yi yanıtlamanız, URL'de oturum açmanız ve durum URL'sini aktarmanız gerekir. Bu öğreticide daha sonra açıklandığı gibi, tanımlayıcıyı ve url değerlerini bir Pega uygulamasından yanıtlayabilirsiniz. Röle durumu değerini almak için [Pega Systems destek ekibine](https://www.pega.com/contact-us)başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

6. Pega Systems uygulamasının belirli bir formatta olması için SAML iddialarına ihtiyacı vardır. Bunları doğru biçimde almak için SAML belirteç öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemeniz gerekir. Aşağıdaki ekran görüntüsü varsayılan öznitelikleri gösterir. **Kullanıcı Öznitelikleri** iletişim kutusunu açmak için **Edit** simgesini seçin:

    ![Kullanıcı Öznitelikleri](common/edit-attribute.png)

7. Pega Systems uygulaması, önceki ekran görüntüsünde gösterilen özniteliklere ek olarak, SAML yanıtında birkaç özniteliğin daha geçirilmesini gerektirir. Kullanıcı **Öznitelikleri** iletişim kutusunun **Kullanıcı** talepleri bölümünde, bu SAML belirteç özniteliklerieklemek için aşağıdaki adımları tamamlayın:

    
   - `uid`
   - `cn`
   - `mail`
   - `accessgroup`  
   - `organization`  
   - `orgdivision`
   - `orgunit`
   - `workgroup`  
   - `Phone`

    > [!NOTE]
    > Bu değerler kuruluşunuza özgüdür. Uygun değerleri sağlayın.

    1. **Kullanıcı taleplerini yönet** iletişim kutusunu açmak için yeni talep **ekle'yi** seçin:

    ![Yeni talep ekle'yi seçin](common/new-save-attribute.png)

    ![Kullanıcı talepleri iletişim kutusunu yönetme](common/new-attribute-details.png)

    1. **Ad** kutusuna, bu satır için gösterilen öznitelik adını girin.

    1. Ad **Alanı** kutusunu boş bırakın.

    1. **Kaynak**için, **Öznitelik'i**seçin.

    1. Kaynak **öznitelik** listesinde, bu satır için gösterilen öznitelik değerini seçin.

    1. **Tamam**’ı seçin.

    1. **Kaydet'i**seçin.

8. **SAML Ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, gereksinimlerinize göre Federation **Metadata XML'nin**yanındaki **İndir** bağlantısını seçin ve sertifikayı bilgisayarınıza kaydedin:

    ![Sertifika indirme linki](common/metadataxml.png)

9. **Pega Systems'i Kur** bölümünde, gereksinimlerinize göre uygun URL'leri kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    1. **Giriş URL'si**.

    1. **Azure AD Tanımlayıcısı**.

    1. **Çıkış URL'si**.

### <a name="configure-pega-systems-single-sign-on"></a>Pega Systems'i yapılandırma tek oturum açma

1. **Pega Systems** tarafında tek oturum açma yapılandırmak için, başka bir tarayıcı penceresinde bir yönetici hesabıyla Pega Portal'da oturum açın.

2. **SysAdmin** > **Kimlik Doğrulama Hizmeti** **Oluştur'u** > seçin:

    ![Kimlik Doğrulama Hizmeti'ni seçin](./media/pegasystems-tutorial/tutorial_pegasystems_admin.png)
    
3. Kimlik Doğrulama Hizmeti **Oluştur** ekranında aşağıdaki adımları tamamlayın.

    ![Kimlik Doğrulama Hizmeti ekranı oluşturma](./media/pegasystems-tutorial/tutorial_pegasystems_admin1.png)

    1. **Tür** listesinde **SAML 2.0'ı**seçin.

    1. **Ad** kutusuna herhangi bir ad girin (örneğin, **Azure AD SSO).**

    1. Kısa **açıklama** kutusuna bir açıklama girin.  

    1. **Oluştur'u seçin ve açın.**
    
4. Kimlik **Sağlayıcısı (IdP) bilgileri** bölümünde, **IdP meta verilerini içe aktar'ı** seçin ve Azure portalından indirdiğiniz meta veri dosyasına göz atın. Meta verileri yüklemek için **Gönder'i** tıklatın:

    ![Kimlik Sağlayıcı (IdP) bilgi bölümü](./media/pegasystems-tutorial/tutorial_pegasystems_admin2.png)
    
    Alma, IdP verilerini burada gösterildiği gibi dolduracaktır:

    ![İçe aktarılan IdP verileri](./media/pegasystems-tutorial/tutorial_pegasystems_admin3.png)
    
6. **Servis Sağlayıcı (SP) ayarları** bölümünde aşağıdaki adımları tamamlayın.

    ![Servis sağlayıcı ayarları](./media/pegasystems-tutorial/tutorial_pegasystems_admin4.png)

    1. Varlık **Tanımlama** değerini kopyalayın ve Azure portalındaki **Temel SAML Yapılandırması** bölümündeki **Tanımlayıcı** kutusuna yapıştırın.

    1. İddia **Tüketici Hizmeti (ACS) konum** değerini kopyalayın ve Azure portalındaki **Temel SAML Yapılandırması** bölümündeki **YanıtLA URL** kutusuna yapıştırın.

    1. **İstek imzalamayı devre dışı tonuyla**seçin.

7. **Kaydet'i**seçin.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, Azure portalında Britta Simon adında bir test kullanıcısı oluşturursunuz.

1. Azure portalında, sol bölmede **Azure Etkin Dizini'ni** seçin, **Kullanıcılar'ı**seçin ve ardından **Tüm Kullanıcıları**seçin:

    ![Tüm kullanıcılar'ı seçin](common/users.png)

2. Ekranın üst kısmında **Yeni kullanıcı** seçin:

    ![Yeni kullanıcıyı seçin](common/new-user.png)

3. **Kullanıcı** iletişim kutusunda aşağıdaki adımları tamamlayın.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** **kutusuna BrittaSimon**girin.
  
    b. Kullanıcı **adı** kutusuna, **\<şirketinizin alan\< brittasimon@> girin. uzantısı>**. (Örneğin, BrittaSimon@contoso.com.)

    c. **Parolayı Göster'i**seçin ve ardından **Parola** kutusundaki değeri yazın.

    d. **Oluştur'u**seçin.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde Britta Simon'ın Pega Systems'e erişimini sağlayarak Azure tek oturum açma işlemini kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal uygulamaları**seçin, **Tüm uygulamaları**seçin ve ardından **Pega Systems'i**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Pega Systems'i**seçin.

    ![Başvuru listesi](common/all-applications.png)

3. Sol **bölmede, Kullanıcıları ve grupları**seçin:

    ![Kullanıcı ve gruplar'ı seçin](common/users-groups-blade.png)

4. **Kullanıcı Ekle'yi**seçin ve ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı ekle seçeneğini belirleme](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, kullanıcı listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında, **Rolü Seç** iletişim kutusunda bir rol değeri bekliyorsanız, listeden kullanıcı için uygun rolü seçin. Ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atama'yı**seçin.

### <a name="create-a-pega-systems-test-user"></a>Pega Systems test kullanıcısı oluşturma

Sonra, Pega Systems Britta Simon adında bir kullanıcı oluşturmanız gerekir. Kullanıcılar oluşturmak için [Pega Systems destek ekibiyle](https://www.pega.com/contact-us) birlikte çalışın.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Artık Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı test etmeniz gerekir.

Access Paneli'ndeki Pega Sistemleri döşemesini seçtiğinizde, SSO'yu kurduğunuz Pega Systems örneğinde otomatik olarak oturum açmalısınız. Daha fazla bilgi için [Access'e bakın ve Uygulamalarım portalındaki uygulamaları kullanın.](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme öğreticileri](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)