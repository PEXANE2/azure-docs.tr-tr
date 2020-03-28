---
title: 'Öğretici: ServiceNow ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve ServiceNow arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: a5a1a264-7497-47e7-b129-a1b5b1ebff5b
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 01/31/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: c18613233d6dec59c76db120ed7f089dfbb5fbac
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77046731"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-servicenow"></a>Öğretici: ServiceNow ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde ServiceNow'u Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. ServiceNow'u Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* ServiceNow erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla ServiceNow'da otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin: Azure portalı.

Azure AD ile hizmet olarak yazılım (SaaS) uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* ServiceNow tek oturum açma (SSO) özellikli abonelik.
* ServiceNow için ServiceNow, Calgary sürümü veya daha sonra bir örnek veya kiracı.
* ServiceNow Express, ServiceNow Express, Helsinki sürümü veya daha sonra bir örneği için.
* ServiceNow kiracısı, [Eklentide Birden Çok Sağlayıcı Tek İşareti](https://wiki.servicenow.com/index.php?title=Multiple_Provider_Single_Sign-On#gsc.tab=0) etkin olmalıdır. Bunu [bir hizmet isteği göndererek](https://hi.service-now.com)yapabilirsiniz.
* Otomatik yapılandırma için ServiceNow için çoklu sağlayıcı eklentisini etkinleştirin.
* ServiceNow Classic (Mobile) uygulamasını yüklemek için uygun mağazaya gidin ve ServiceNow Classic uygulamasını arayın. O zaman indir.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz. 

* **ServiceNow, SP** tarafından başlatılan SSO'yı destekler.

* ServiceNow [Otomatik kullanıcı sağlama](servicenow-provisioning-tutorial.md)destekler.

* ServiceNow'u yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak sızma ve sızmalarını koruyan Oturum Denetimi'ni uygulayabilirsiniz. Oturum Denetimi Koşullu Erişim'den genişletir. [Microsoft Cloud App Security ile oturum denetimini nasıl uygulayacağınızı öğrenin](https://docs.microsoft.com/cloud-app-security/proxy-deployment-aad)

* SSO'yu etkinleştirmek için ServiceNow Classic (Mobil) uygulamasını Azure AD ile yapılandırabilirsiniz. Hem Android hem de iOS kullanıcılarını destekler. Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

## <a name="add-servicenow-from-the-gallery"></a>Galeriden ServiceNow ekle

ServiceNow'un Azure AD'ye entegrasyonunu yapılandırmak için, galeriden yönetilen SaaS uygulamaları listenize ServiceNow eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabı kullanarak veya kişisel bir Microsoft hesabı kullanarak oturum açın.
1. Sol bölmede **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a**gidin ve **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **ServiceNow'u** girin.
1. Sonuç panelinden **ServiceNow'u** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-servicenow"></a>ServiceNow için Azure AD oturum açma işlemlerini yapılandırın ve test edin

Azure AD SSO'nu ServiceNow ile **B.Simon**adlı bir test kullanıcısı kullanarak yapılandırın ve test edin. SSO'nun çalışması için, ServiceNow'daki bir Azure REKLAM kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu ServiceNow ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için [Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)
    1. Azure AD oturum açma'yı B.Simon ile test etmek için [bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)
    1. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için [Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)
    1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak [için ServiceNow Express için Azure AD SSO'su](#configure-azure-ad-sso-for-servicenow-express) yapılandırın.
2. Uygulama tarafındaki SSO ayarlarını yapılandırmak için [ServiceNow'u yapılandırın.](#configure-servicenow)
    1. ServiceNow'da B.Simon'ın bir örneğine sahip olmak için [bir ServiceNow test kullanıcısı oluşturun](#create-servicenow-test-user) ve kullanıcının Azure AD gösterimine bağlanın.
    1. Uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için [ServiceNow Express SSO'yu yapılandırın.](#configure-servicenow-express-sso)  
3. Yapılandırmanın çalışıp çalışmadığını doğrulamak için [SSO'yu test](#test-sso) edin.
4. Yapılandırmanın çalışıp çalışmadığını doğrulamak [için ServiceNow Classic (Mobile) için SSO'yu test](#test-sso-for-servicenow-classic-mobile) edin.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında](https://portal.azure.com/), **ServiceNow** uygulama tümleştirme sayfasında **Yönet** bölümünü bulun. **Tek oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için kalem simgesini seçin.

   ![Kalem simgesi vurgulanmış SAML sayfası ile Tek Oturum Açma'yı ayarlama ekran görüntüsü](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    a. **URL'de Oturum**Aç'ta, aşağıdaki deseni kullanan bir URL girin:`https://<instance-name>.service-now.com/navpage.do`

    b. **Tanımlayıcıda (Entity ID)** aşağıdaki deseni kullanan bir URL girin:`https://<instance-name>.service-now.com`

    > [!NOTE]
    > Bu değerler gerçek değil. Bu değerleri, daha sonra öğreticide açıklanan gerçek oturum açma URL'si ve tanımlayıcısıyla güncelleştirmeniz gerekir. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

1. **SAML sayfasıyla tek oturum** açma'da, **SAML İmza Sertifikası** bölümünde **Sertifikayı (Base64)** bulun. 

   ![SamL İmzaSertifikası bölümünün ekran görüntüsü, Download vurgulanmış](common/certificatebase64.png)

   a. **App Federation Metadata Url'sini**kopyalamak için kopyala düğmesini seçin ve Not Defteri'ne yapıştırın. Bu URL daha sonra öğreticide kullanılacaktır.

    b. Sertifikayı indirmek için **İndir'i(Base64)** seçin ve ardından sertifika dosyasını bilgisayarınıza kaydedin. **Download**

1. **ServiceNow'u ayarla** bölümünde, gereksiniminize göre uygun URL'leri kopyalayın.

   ![URL'ler vurgulanmış ServiceNow bölümünün ekran görüntüsü](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, Azure portalında B.Simon adında bir test kullanıcısı oluşturursunuz.

1. Azure portalındaki sol bölmeden **Azure Etkin Dizin** > **Kullanıcıları** > **Tüm kullanıcıları**seçin.
1. Ekranın üst kısmında **Yeni kullanıcı** yı seçin.
1. **Kullanıcı** özelliklerinde aşağıdaki adımları izleyin:
   1. **Ad**için `B.Simon`, girin .  
   1. **Kullanıcı adı**için username@companydomain.extension, . Örneğin, `B.Simon@contoso.com`.
   1. **Parolayı Göster'i**seçin ve ardından **Parola** kutusunda gösterilen değeri yazın.
   1. **Oluştur'u**seçin.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, B.Simon'ın ServiceNow'a erişim sağlayarak Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar** > **Tüm uygulamaları**seçin.
1. Uygulamalar listesinde **ServiceNow'u**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   ![Kullanıcılar ve gruplar vurgulanırken Yönet bölümünün ekran görüntüsü](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin. Atama **Ekle** iletişim **kutusunda, Kullanıcılar ve gruplar**seçin.

    ![Kullanıcı ekle vurgulanan kullanıcılar ve grupların ekran görüntüsü](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B.Simon'ı** seçin ve sonra **Seç'i**seçin.
1. SAML iddiasında, **Rolü Seç** iletişim kutusunda herhangi bir rol değeri bekliyorsanız, listeden kullanıcı için uygun rolü seçin. Ardından **Seç** seçeneğini belirleyin.
1. Atama **Ekle** iletişim kutusunda **Atama'yı**seçin.

### <a name="configure-azure-ad-sso-for-servicenow-express"></a>ServiceNow Express için Azure AD SSO'su yapılandırın

1. Azure [portalında](https://portal.azure.com/), **ServiceNow** uygulama tümleştirme sayfasında **tek oturum açma'yı**seçin.

    ![ServiceNow uygulama tümleştirme sayfasının ekran görüntüsü, tek oturum açma vurgulu](common/select-sso.png)

2. Tek **oturum** açma yöntemi iletişim kutusunu seç'te, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![SAML vurgulandığı tek bir oturum açma yöntemini seç ekran görüntüsü](common/select-saml-option.png)

3. **SAML sayfasıyla tek oturum açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için kalem simgesini seçin.

    ![KALEM simgesi vurgulanmış SAML sayfasıyla tek oturum açma ekranı](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    a. **URL'de Oturum Aç**için, aşağıdaki deseni kullanan bir URL girin:`https://<instance-name>.service-now.com/navpage.do`

    b. **Tanımlayıcı (Entity ID)** için, aşağıdaki deseni kullanan bir URL girin:`https://<instance-name>.service-now.com`

    > [!NOTE]
    > Bu değerler gerçek değil. Bu değerleri, daha sonra öğreticide açıklanan gerçek oturum açma URL'si ve tanımlayıcısıyla güncelleştirmeniz gerekir. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

5. **SAML** ile Tek Oturum Açma sayfasında, **SAML İmza Sertifikası** bölümünde, gereksiniminize göre belirtilen seçeneklerden **Sertifikayı (Base64)** indirmek için **İndir'i** seçin. Bilgisayarınıza kaydedin.

    ![SamL İmzalama Sertifikası bölümünün ekran görüntüsü, İndir vurgulanmış](common/certificatebase64.png)

6. Azure AD'nin SAML tabanlı kimlik doğrulaması için ServiceNow'u otomatik olarak yapılandırması olabilir. Bu hizmeti etkinleştirmek için **ServiceNow'u Ayarla** bölümüne gidin ve Oturum açma penceresini **yapılandırmak** için **adım adım yönergeleri görüntüle'yi** seçin.

    ![ServiceNow'u ayarla bölümünün ekran görüntüsü, adım adım yönergeleri nizve etti](./media/servicenow-tutorial/tutorial_servicenow_configure.png)

7. Yapılandırma **oturum açma** formuna ServiceNow örnek adınızı, yönetici kullanıcı adınızı ve yönetici parolanızı girin. **Şimdi Yapılandır'ı**seçin. Sağlanan yönetici kullanıcı adı, bunun çalışması için ServiceNow'da atanmış **security_admin** rolüne sahip olmalıdır. Aksi takdirde, ServiceNow'u Azure AD'yi SAML Identity Provider olarak kullanacak şekilde el ile yapılandırmak için, **tek oturum açma'yı El ile yapılandır'ı**seçin. Hızlı Başvuru bölümünden **Giriş URL'sini, Azure AD Tanımlayıcısını ve Giriş URL'sini** kopyalayın.

    ![Yapılandırma Şimdi vurgulanmış olan Oturum Açma formunun ekran görüntüsü](./media/servicenow-tutorial/configure.png "Uygulama URL'sini yapılandırma")

## <a name="configure-servicenow"></a>Hizmeti Yapılandır

1. ServiceNow uygulamanızda yönetici olarak oturum açın.

2. **Tümleştirmeyi etkinleştirin - Aşağıdaki** adımları izleyerek Birden Çok Sağlayıcı tek oturum açma Yükleyici eklentisi:

    a. Sol bölmede, arama kutusundan **Sistem Tanımı** bölümünü arayın ve ardından **Eklentileri**seçin.

    ![Sistem Tanımı ve Eklentileri vurgulanmış Sistem Tanımı bölümünün ekran görüntüsü](./media/servicenow-tutorial/tutorial_servicenow_03.png "Eklentiyi etkinleştirme")

    b. Tümleştirme Ara **- Birden Çok Sağlayıcı tek oturum açma Yükleyici.**

     ![Sistem Eklentileri sayfasının ekran görüntüsü, Entegrasyon ile - Çoklu Sağlayıcı Tek Sign-On Installer vurgulanır](./media/servicenow-tutorial/tutorial_servicenow_04.png "Eklentiyi etkinleştirme")

    c. Eklentiyi seçin. Sağ tıklatın ve **Etkinleştir/Yükselt'i**seçin.

     ![Tak-in sağ tıklama menüsünün ekran görüntüsü, Etkinleştir/Yükseltme vurgulanır](./media/servicenow-tutorial/tutorial_activate.png "Eklentiyi etkinleştirme")

    d. **Etkinleştir'i**seçin.

     ![Etkinle Eklenti iletişim kutusunun ekran görüntüsü, vurgulanmış etkinleştir](./media/servicenow-tutorial/tutorial_activate1.png "Eklentiyi etkinleştirme")

3. Sol bölmede, arama çubuğundan **Çok Sağlayıcılı SSO** bölümünü arayın ve ardından **Özellikler'i**seçin.

    ![Multi-Provider SSO bölümünün ekran görüntüsü, Multi-Provider SSO ve Özellikleri vurgulanır](./media/servicenow-tutorial/tutorial_servicenow_06.png "Uygulama URL'sini yapılandırma")

4. Birden **Çok Sağlayıcı SSO Özellikleri** iletişim kutusunda aşağıdaki adımları gerçekleştirin:

    ![Çoklu Sağlayıcı SSO Özellikleri iletişim kutusunun ekran görüntüsü](./media/servicenow-tutorial/ic7694981.png "Uygulama URL'sini yapılandırma")

    * **Birden çok sağlayıcı SSO'ya etkinleştiriçin** **Evet'i**seçin.
  
    * **Tüm kimlik sağlayıcılardan kullanıcıların kullanıcı tablosuna Otomatik Olarak Aktarılmasını Etkinleştiriçin** **Evet'i**seçin.

    * **Birden çok sağlayıcı SSO tümleştirmesi için hata ayıklama günlemesini etkinleştir**için **Evet'i**seçin.

    * **Kullanıcı tablosundaki alan için...**, **user_name**girin .
  
    * **Kaydet'i**seçin.

6. ServiceNow'u otomatik veya el ile yapılandırabilirsiniz. ServiceNow'u otomatik olarak yapılandırmak için aşağıdaki adımları izleyin:

    1. Azure portalındaki **ServiceNow** tek oturum açma sayfasına geri dönün.

    1. ServiceNow için tek tıklamayla yapılandırma hizmeti sağlanır. Bu hizmeti etkinleştirmek için **ServiceNow Yapılandırma** bölümüne gidin ve Oturum açma penceresini **yapılandırmak** için **ServiceNow'u Yapılandır'ı** seçin.

        ![ServiceNow'u ayarla'nın ekran görüntüsü, adım adım yönergeleri nizve etti](./media/servicenow-tutorial/tutorial_servicenow_configure.png)

    1. Yapılandırma **oturum açma** formuna ServiceNow örnek adınızı, yönetici kullanıcı adınızı ve yönetici parolanızı girin. **Şimdi Yapılandır'ı**seçin. Sağlanan yönetici kullanıcı adı, bunun çalışması için ServiceNow'da atanmış **security_admin** rolüne sahip olmalıdır. Aksi takdirde, ServiceNow'u Azure AD'yi SAML Identity Provider olarak kullanacak şekilde el ile yapılandırmak için, **tek oturum açma'yı El ile yapılandır'ı**seçin. Hızlı Başvuru bölümünden **Oturum Açma URL'sini, SAML Entity Id'i ve SAML tek oturum açma Hizmeti URL'sini** kopyalayın.

        ![Yapılandırma Şimdi vurgulanmış olan Oturum Açma formunun ekran görüntüsü](./media/servicenow-tutorial/configure.png "Uygulama URL'sini yapılandırma")

    1. ServiceNow uygulamanızda yönetici olarak oturum açın.

       * Otomatik yapılandırmada, gerekli tüm ayarlar **ServiceNow** tarafında yapılandırılır, ancak **X.509 Sertifikası** varsayılan olarak etkinleştirilir. ServiceNow'da kimlik sağlayıcınızla elle eşleniz gerekir. Şu adımları uygulayın:

         1. Sol bölmede, arama kutusundan **Çoklu SağlayıcıSSO** bölümünü arayın ve **Kimlik Sağlayıcıları'nı**seçin.

            ![Kimlik Sağlayıcılar vurgulanırken, Çoklu Sağlayıcılı SSO bölümünün ekran görüntüsü](./media/servicenow-tutorial/tutorial_servicenow_07.png "Çoklu oturum açmayı yapılandırma")

         1. Otomatik olarak oluşturulan kimlik sağlayıcısını seçin.

            ![Otomatik olarak oluşturulan kimlik sağlayıcısı vurgulanmış kimlik sağlayıcılarının ekran görüntüsü](./media/servicenow-tutorial/tutorial_servicenow_08.png "Çoklu oturum açmayı yapılandırma")

         1.  Kimlik **Sağlayıcı** lar bölümünde aşağıdaki adımları gerçekleştirin:

             ![Kimlik Sağlayıcı bölümünün ekran görüntüsü](./media/servicenow-tutorial/automatic_config.png "Çoklu oturum açmayı yapılandırma")

               * **Ad**için, yapılandırmanız için bir ad girin (örneğin, **Microsoft Azure Federe tek oturum açma).**

               * Doldurulan **Kimlik Sağlayıcısı'nın SingleLogoutRequest** değerini metin kutusundan kaldırın.

               * **ServiceNow Ana Sayfa** değerini kopyalayın ve Azure portalının **ServiceNow Basic SAML Yapılandırma** bölümünde Oturum Aç **URL'ye** yapıştırın.

                  > [!NOTE]
                  > ServiceNow örnek ana sayfası **ServiceNow kiracı** URL'nizin ve **/navpage.do** `https://fabrikam.service-now.com/navpage.do`(örneğin: ).

              * Varlık **Kimliği / Veren** değerini kopyalayın ve Azure portalının **ServiceNow Basic SAML Yapılandırma** bölümündeki **Tanımlayıcı'ya** yapıştırın.

              * **NameID İlkesi'nin** değer olarak `urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified` ayarlendiğini doğrulayın. 

         1. **X.509 Sertifikası** bölümüne gidin ve **Edit'i**seçin.

             ![X.509 Sertifika bölümünün ekran görüntüsü, Edit vurgulanmış](./media/servicenow-tutorial/tutorial_servicenow_09.png "Çoklu oturum açmayı yapılandırma")

         1. Sertifikayı seçin ve sertifikayı eklemek için sağ ok simgesini seçin

            ![Sertifika ve sağ ok simgesi vurgulanmış Koleksiyonun ekran görüntüsü](./media/servicenow-tutorial/tutorial_servicenow_11.png "Çoklu oturum açmayı yapılandırma")

          1. **Kaydet'i**seçin.

          1. Sayfanın sağ üst köşesinde Test **Bağlantısı'nı**seçin.

             ![Test Bağlantısı vurgulanmış sayfanın ekran görüntüsü](./media/servicenow-tutorial/tutorial_activate2.png "Eklentiyi etkinleştirme")

          1. Kimlik bilgileriniz sorulduğunda, bunları girin. Aşağıdaki sayfayı görürsünüz. **SSO Oturum Açma Test Sonuçları** hatası bekleniyor. Hatayı yoksay ve **Etkinleştir'i**seçin.

             ![Test Sonuçları sayfasının ekran görüntüsü](./media/servicenow-tutorial/servicenowactivate.png "Çoklu oturum açmayı yapılandırma")
  
6. **ServiceNow'u** el ile yapılandırmak için aşağıdaki adımları izleyin:

    1. ServiceNow uygulamanızda yönetici olarak oturum açın.

    1. Sol bölmede Kimlik **Sağlayıcıları'nı**seçin.

        ![Çoklu SağlayıcıSSO ekran görüntüsü, Kimlik Sağlayıcıları vurgulanır](./media/servicenow-tutorial/tutorial_servicenow_07.png "Çoklu oturum açmayı yapılandırma")

    1. Kimlik **Sağlayıcıları** iletişim kutusunda **Yeni'yi**seçin.

        ![Kimlik Sağlayıcıları iletişim kutusunun ekran görüntüsü, Yeni vurgulanmış](./media/servicenow-tutorial/ic7694977.png "Çoklu oturum açmayı yapılandırma")

    1. Kimlik **Sağlayıcıları** iletişim kutusunda **SAML'yi**seçin.

        ![SAML vurgulanmış Kimlik Sağlayıcıları iletişim kutusunun ekran görüntüsü](./media/servicenow-tutorial/ic7694978.png "Çoklu oturum açmayı yapılandırma")

    1. **Alma Kimlik Sağlayıcısı Metaverilerinde**aşağıdaki adımları gerçekleştirin:

        ![URL ve Alma vurgulanmış Olan Alma Kimlik Sağlayıcısı Meta verilerinin ekran görüntüsü](./media/servicenow-tutorial/idp.png "Çoklu oturum açmayı yapılandırma")

        1. Azure portalından kopyaladığınız **Uygulama Federasyonu Metaveri Url'sini** girin.

        1. **İçeri aktar**'ı seçin.

    1. IdP meta veri URL'sini okur ve tüm alan bilgilerini doldurur.

        ![Kimlik Sağlayıcı ekran görüntüsü](./media/servicenow-tutorial/ic7694982.png "Çoklu oturum açmayı yapılandırma")

        * **Ad**için, yapılandırmanız için bir ad girin (örneğin, **Microsoft Azure Federe tek oturum açma).**

        * Doldurulan **Kimlik Sağlayıcısı'nın SingleLogoutRequest** değerini metin kutusundan kaldırın.

        * **ServiceNow Ana Sayfa** değerini kopyalayın. Azure portalının **ServiceNow Basic SAML Yapılandırması** bölümünde **Oturum Aç URL'ye** yapıştırın.

            > [!NOTE]
            > ServiceNow örnek ana sayfası **ServiceNow kiracı** URL'nizin ve **/navpage.do** `https://fabrikam.service-now.com/navpage.do`(örneğin: ).

        * Varlık **Kimliğini / Veren** değerini kopyalayın. Azure portalının **ServiceNow Basic SAML Yapılandırma** sı **bölümündei Tanımlayıcı'ya** yapıştırın.

        * **NameID İlkesi'nin** değer olarak `urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified` ayarlendiğini doğrulayın.

        * **Gelişmiş**'i seçin. **Kullanıcı Alanı'nda,** ServiceNow dağıtımınızdaki kullanıcıları benzersiz olarak tanımlamak için hangi alanın kullanıldığına bağlı olarak **e-posta** veya **user_name**girin.

            > [!NOTE]
            > Azure AD'yi, Azure AD kullanıcı kimliği (kullanıcı adı) veya e-posta adresini SAML belirtecindeki benzersiz tanımlayıcı olarak yayarak yapılandırabilirsiniz. Bunu, Azure portalının **ServiceNow** > **Öznitelikleri** > **Tek oturum** açma bölümüne giderek ve istenen alanı **namedentifier** özniteliğiyle eşleyerek yapın. Azure AD'de seçili öznitelik için depolanan değerin (örneğin, kullanıcı adı adı) girilen alan için ServiceNow'da depolanan değerle eşleşmesi gerekir (örneğin, user_name).

        * Sayfanın sağ üst köşesindeki **Test Bağlantısı'nı** seçin.

        * Kimlik bilgileriniz sorulduğunda, bunları girin. Aşağıdaki sayfayı görürsünüz. **SSO Oturum Açma Test Sonuçları** hatası bekleniyor. Hatayı yoksay ve **Etkinleştir'i**seçin.

          ![Test Sonuçları sayfasının ekran görüntüsü](./media/servicenow-tutorial/servicenowactivate.png "Çoklu oturum açmayı yapılandırma")

### <a name="create-servicenow-test-user"></a>ServiceNow test kullanıcısı oluştur

Bu bölümün amacı ServiceNow B.Simon adlı bir kullanıcı oluşturmaktır. ServiceNow varsayılan olarak etkinleştirilen otomatik kullanıcı sağlamayı destekler.

> [!NOTE]
> El ile bir kullanıcı oluşturmanız gerekiyorsa [ServiceNow Client destek ekibine](https://www.servicenow.com/support/contact-support.html)başvurun.

### <a name="configure-servicenow-express-sso"></a>ServiceNow Express SSO'da Yapılandır

1. ServiceNow Express başvurunuza yönetici olarak oturum açın.

2. Sol **bölmede, Tek Oturum Açma'yı**seçin.

    ![ServiceNow Express uygulamasının ekran görüntüsü, Tek Oturum Açma vurgulu](./media/servicenow-tutorial/ic7694980ex.png "Uygulama URL'sini yapılandırma")

3. Tek **Oturum Açma** iletişim kutusunda, sağ üstteki yapılandırma simgesini seçin ve aşağıdaki özellikleri ayarlayın:

    ![Tek Oturum Açma iletişim kutusunun ekran görüntüsü](./media/servicenow-tutorial/ic7694981ex.png "Uygulama URL'sini yapılandırma")

    a. Geçiş **Sağdaki birden çok sağlayıcı SSO'yu etkinleştirin.**

    b. Geçiş, **birden çok sağlayıcı SSO tümleştirmesi için sağa hata ayıklama günlüğe kaydetmeyi etkinleştir.**

    c. **Kullanıcı tablosundaki alana...**, **user_name**girin .

4. Tek **Oturum Açma** iletişim kutusunda **Yeni Sertifika Ekle'yi**seçin.

    ![Yeni Sertifika Ekle vurgulanmış tek oturum açma iletişim kutusunun ekran görüntüsü](./media/servicenow-tutorial/ic7694973ex.png "Çoklu oturum açmayı yapılandırma")

5. **X.509 Sertifikaları** iletişim kutusunda aşağıdaki adımları gerçekleştirin:

    ![X.509 Sertifikaları iletişim kutusunun ekran görüntüsü](./media/servicenow-tutorial/ic7694975.png "Çoklu oturum açmayı yapılandırma")

    a. **Ad**için, yapılandırmanız için bir ad girin (örneğin: **TestSAML2.0).**

    b. **Etkin'i**seçin.

    c. **Biçim**için **PEM'i**seçin.

    d. **Türü**için, **Güven Deposu Cert**seçin.

    e. Notepad'de Azure portalından indirilen Base64 kodlu sertifikanızı açın. İçeriğini panonuza kopyalayın ve **PEM Sertifikası** metin kutusuna yapıştırın.

    f. **Güncelleştir'i** Seçin

6. Tek **Oturum Açma** iletişim kutusunda **Yeni IdP ekle'yi**seçin.

    ![Yeni IdP Ekle vurgulanmış tek oturum açma iletişim kutusunun ekran görüntüsü](./media/servicenow-tutorial/ic7694976ex.png "Çoklu oturum açmayı yapılandırma")

7. Kimlik **SağlayıcısıNı Yapılandır'ın**altında **yeni kimlik sağlayıcı** ekle iletişim kutusunda aşağıdaki adımları gerçekleştirin:

    ![Yeni Kimlik Sağlayıcı Ekle iletişim kutusunun ekran görüntüsü](./media/servicenow-tutorial/ic7694982ex.png "Çoklu oturum açmayı yapılandırma")

    a. **Ad**için, yapılandırmanız için bir ad girin (örneğin: **SAML 2.0).**

    b. **Kimlik Sağlayıcı URL'si**için, Azure portalından kopyaladığınız kimlik sağlayıcı kimliğinin değerini yapıştırın.

    c. **Kimlik Sağlayıcı'nın AuthnRequest'i**için Azure portalından kopyaladığınız kimlik doğrulama isteği URL'sinin değerini yapıştırın.

    d. **Kimlik Sağlayıcısı'nın SingleLogoutRequest'i**için Azure portalından kopyaladığınız çıkış URL'sinin değerini yapıştırın.

    e. **Kimlik Sağlayıcı Sertifikası**için, önceki adımda oluşturduğunuz sertifikayı seçin.

8. **Gelişmiş Ayarlar'ı**seçin. **Ek Kimlik Sağlayıcı Özellikleri**altında, aşağıdaki adımları gerçekleştirin:

    ![Gelişmiş Ayarlar vurgulanmış yeni kimlik sağlayıcı iletişim kutusunun ekran görüntüsü](./media/servicenow-tutorial/ic7694983ex.png "Çoklu oturum açmayı yapılandırma")

    a. **IDP'nin SingleLogoutRequest için Protokol Bağlama için**, **urn girin:oasis:names:tc:SAML:2.0:bindings:HTTP-Yönlendirme**.

    b. **NameID Politikası**için, **vazo girin:oasis:names:tc:SAML:1.1:nameid-format:belirtilmemiş**.

    c. **AuthnContextClassRef Yöntemi**için `http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password`, girin .

    d. **AuthnContextClass oluşturmak**için, (seçili olmayan) için değiştirin.

9. **Ek Hizmet Sağlayıcı Özellikleri**altında, aşağıdaki adımları gerçekleştirin:

    ![Çeşitli özellikleri vurgulanmış Yeni Kimlik Sağlayıcı iletişim kutusu nun ekran görüntüsü](./media/servicenow-tutorial/ic7694984ex.png "Çoklu oturum açmayı yapılandırma")

    a. **ServiceNow Ana Sayfası**için ServiceNow örnek ana sayfanızın URL'sini girin.

    > [!NOTE]
    > ServiceNow örnek ana sayfası **ServiceNow kiracı** URL'nizin ve **/navpage.do** `https://fabrikam.service-now.com/navpage.do`(örneğin: ).

    b. **Entity ID / Veren**için ServiceNow kiracınızın URL'sini girin.

    c. **Hedef Kitle URI**için ServiceNow kiracınızın URL'sini girin.

    d. **Saat Skew**için, **60**girin.

    e. **Kullanıcı Alanı**için, ServiceNow dağıtımınızdaki kullanıcıları benzersiz olarak tanımlamak için hangi alanın kullanıldığına bağlı olarak **e-posta** veya **user_name**girin.

    > [!NOTE]
    > Azure AD'yi, Azure AD kullanıcı kimliği (kullanıcı adı) veya e-posta adresini SAML belirtecindeki benzersiz tanımlayıcı olarak yayarak yapılandırabilirsiniz. Bunu, Azure portalının **ServiceNow** > **Öznitelikleri** > **Tek oturum** açma bölümüne giderek ve istenen alanı **namedentifier** özniteliğiyle eşleyerek yapın. Azure AD'de seçili öznitelik için depolanan değerin (örneğin, kullanıcı adı adı) girilen alan için ServiceNow'da depolanan değerle eşleşmesi gerekir (örneğin, user_name).

    f. **Kaydet'i**seçin.

## <a name="test-sso"></a>Test SSO

Access Paneli'ndeki ServiceNow döşemesini seçtiğinizde, SSO'yu kurduğunuz ServiceNow'da otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="test-sso-for-servicenow-classic-mobile"></a>ServiceNow Classic (Mobil) için Test SSO

1. **ServiceNow Classic (Mobil)** uygulamanızı açın ve aşağıdaki adımları gerçekleştirin:

    a. Sağ alt köşedeki artı işaretini seçin.

    ![ServiceNow Classic uygulamasının ekran görüntüsü, artı işareti vurgulanmış](./media/servicenow-tutorial/test03.png)

    b. ServiceNow örnek adınızı girin ve **Devam et'i**seçin.

    ![Örnek Ekle sayfasının ekran görüntüsü, Devam vurgulanmış](./media/servicenow-tutorial/test04.png)

    c. **Giriş** sayfasında aşağıdaki adımları gerçekleştirin:

    ![Giriş sayfasının ekran görüntüsü, Dış giriş özelliği vurgulanmış](./media/servicenow-tutorial/test01.png)

    *  **Kullanıcı Adını**girin B.simon@contoso.com, gibi .

    *  **KULLAN Harici Gİrİş'İ**SEÇIN. Oturum açma için Azure REKLAM sayfasına yönlendirilirsiniz.
    
    *  Kimlik bilgilerinizi girin. Herhangi bir üçüncü taraf kimlik doğrulaması veya başka bir güvenlik özelliği etkinse, kullanıcı buna göre yanıt vermelidir. Uygulama **Giriş sayfası** görüntülenir.

        ![Uygulama giriş sayfasının ekran görüntüsü](./media/servicenow-tutorial/test02.png)

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile nasıl entegre edilebildiğini anlatan öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Kullanıcı sağlamayı yapılandırma](servicenow-provisioning-tutorial.md)

- [Azure AD ile ServiceNow'u deneyin](https://aad.portal.azure.com)

- [Microsoft Cloud App Security'de oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [Gelişmiş görünürlük ve kontrollerle ServiceNow nasıl korunur?](https://docs.microsoft.com/cloud-app-security/protect-servicenow)