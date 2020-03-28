---
title: 'Öğretici: Salesforce Sandbox ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Salesforce Sandbox arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: ee54c39e-ce20-42a4-8531-da7b5f40f57c
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 01/16/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: b9d6d11b6b56483f954049fdc1858db31f35c14a
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "76290012"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-salesforce-sandbox"></a>Öğretici: Salesforce Sandbox ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, Salesforce Sandbox'ı Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. Salesforce Sandbox'ı Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Salesforce Sandbox erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Salesforce Sandbox'ta otomatik olarak oturum açmasını etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Salesforce Sandbox tek oturum açma (SSO) aboneliği ni etkinleştirildi.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* Salesforce Sandbox **SP ve IDP** sso başlatılan destekler
* Salesforce Sandbox **Just In Time** kullanıcı sağlama destekler
* Salesforce Sandbox [ **Otomatik** kullanıcı sağlama destekler](salesforce-sandbox-provisioning-tutorial.md)
* Salesforce Sandbox'ı yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak sızma ve sızmalarını koruyan oturum denetimlerini uygulayabilirsiniz. Oturum denetimleri Koşullu Erişim'den itibaren genişletir. [Microsoft Cloud App Security ile oturum denetimini nasıl uygulayacağınızı öğrenin](https://docs.microsoft.com/cloud-app-security/proxy-deployment-aad)

## <a name="adding-salesforce-sandbox-from-the-gallery"></a>Galeriden Salesforce Sandbox ekleme

Salesforce Sandbox'ın Azure AD'ye entegrasyonunu yapılandırmak için, galeriden Yönetilen SaaS uygulamaları listenize Salesforce Sandbox'ı eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Salesforce Sandbox** yazın.
1. Sonuç panelinden **Salesforce Sandbox'ı** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.


## <a name="configure-and-test-azure-ad-single-sign-on-for-salesforce-sandbox"></a>Salesforce Sandbox için Azure AD oturum açma işlemlerini yapılandırma ve test edin

Azure AD SSO'yu Salesforce Sandbox ile **B.Simon**adlı bir test kullanıcısı kullanarak yapılandırın ve test edin. SSO'nun çalışması için, Salesforce Sandbox'taki bir Azure REKLAM kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Salesforce Sandbox ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    * Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    * B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[Salesforce Sandbox SSO'yu yapılandırır](#configure-salesforce-sandbox-sso)** - uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için.
    * **[Salesforce Sandbox test kullanıcısını oluşturun](#create-salesforce-sandbox-test-user)** - Kullanıcının Azure AD gösterimine bağlı Salesforce Sandbox'ta B.Simon'ın bir muadili olması için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında](https://portal.azure.com/) **Salesforce Sandbox** uygulama tümleştirme sayfasında Yönet **bölümünü** bulun ve **tek oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırma** sı bölümünde, **Servis Sağlayıcı meta veri dosyanız** varsa ve **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin:

    a. **Meta veri dosyalarını yükle'yi**tıklatın.

    ![Meta veri dosyalarını yükleme](common/upload-metadata.png)

    b. Meta veri dosyasını seçmek için **klasör logosuna** tıklayın ve **Yükle'yi**tıklatın.

    ![meta veri dosyasini seçin](common/browse-upload-metadata.png)

    > [!NOTE]
    > Hizmet sağlayıcı meta veri dosyasını salesforce Sandbox yönetici portalından alırsınız ve bu dosya daha sonra öğreticide açıklanır.

    c. Meta veri dosyası başarıyla yüklendikten **sonra, Yanıtla URL** değeri **Yanıtla URL** metin kutusunda otomatik olarak doldurulur.

    ![image](common/both-replyurl.png)

    > [!Note]
    > **YanıtURL** değeri otomatik olarak elde edilmezse, gereksiniminize göre değeri el ile doldurun.

5. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, **metadata XML'yi** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

6. **Salesforce Sandbox'ı Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD Tanımlayıcısı

    c. Giriş URL'si

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

Bu bölümde, Salesforce Sandbox'a erişim sağlayarak B.Simon'Un Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **Salesforce Sandbox'ı**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-salesforce-sandbox-sso"></a>Salesforce Sandbox SSO'yu yapılandırın

1. Tarayıcınızda yeni bir sekme açın ve Salesforce Sandbox yönetici hesabınızda oturum açın.

2. Sayfanın sağ üst köşesindeki **ayarlar simgesinin** altındaki **Kurulum** simgesine tıklayın.

    ![Tek İşaret-On'u Yapılandır](./media/salesforce-sandbox-tutorial/configure1.png)

3. Sol gezinti bölmesindeki **AYARLAR'a** gidin, ilgili bölümü genişletmek için **Kimlik'i** tıklatın. Ardından **Tek Oturum Açma Ayarları'nı**tıklatın.

    ![Tek İşaret-On'u Yapılandır](./media/salesforce-sandbox-tutorial/sf-admin-sso.png)

4. Tek **Oturum Açma Ayarları** **sayfasında, Düzelt** düğmesini tıklatın.

    ![Tek İşaret-On'u Yapılandır](./media/salesforce-sandbox-tutorial/configure3.png)

5. **SAML Etkin'i**seçin ve ardından **Kaydet'i**tıklatın.

    ![Tek İşaret-On'u Yapılandır](./media/salesforce-sandbox-tutorial/sf-enable-saml.png)

6. SAML tek oturum açma ayarlarınızı yapılandırmak için **Meta veri dosyasından Yeni'yi**tıklatın.

    ![Tek İşaret-On'u Yapılandır](./media/salesforce-sandbox-tutorial/sf-admin-sso-new.png)

7. Azure portalından indirdiğiniz meta data XML dosyasını yüklemek için **Dosyayı Seç'i** tıklatın ve **Oluştur'u**tıklatın.

    ![Tek İşaret-On'u Yapılandır](./media/salesforce-sandbox-tutorial/xmlchoose.png)

8. **SAML Tek Oturum Açma Ayarları** sayfasında, alanlar otomatik olarak doldurulur ve kaydet'i tıklatın.

    ![Tek İşaret-On'u Yapılandır](./media/salesforce-sandbox-tutorial/salesforcexml.png)

9. Tek **Oturum Açma Ayarları** sayfasında, servis sağlayıcı meta veri dosyasını indirmek için **Meta Veri İndir** düğmesini tıklatın. Yukarıda açıklandığı gibi gerekli URL'leri yapılandırmak için azure portalındaki **Temel SAML Yapılandırma** bölümünde bu dosyayı kullanın.

    ![Tek İşaret-On'u Yapılandır](./media/salesforce-sandbox-tutorial/configure4.png)

10. Uygulamayı **SP** tarafından başlatılan modda yapılandırmak isterseniz, bunun ön koşulları şunlardır:

    a. Doğrulanmış bir etki alanınız olmalıdır.

    b. Salesforce Sandbox üzerinde etki alanınızı yapılandırmanız ve etkinleştirmeniz gerekir, bunun için adımlar daha sonra bu öğreticide açıklanmıştır.

    c. Azure portalında, **Temel SAML Yapılandırması** **bölümünde, ek URL'ler ayarla'yı** tıklatın ve aşağıdaki adımı gerçekleştirin:
  
    ![Salesforce Sandbox Domain ve URL'ler tek oturum açma bilgileri](common/both-signonurl.png)

    Oturum **Açma URL** metin kutusuna, aşağıdaki deseni kullanarak değeri yazın:`https://<instancename>--Sandbox.<entityid>.my.salesforce.com`

    > [!NOTE]
    > Bu değer, etki alanını etkinleştirdikten sonra Salesforce Sandbox portalından kopyalanmalıdır.

11. **SAML İmzaSertifikası** **bölümünde, Federasyon Metadata XML'i** tıklatın ve ardından xml dosyasını bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

12. Tarayıcınızda yeni bir sekme açın ve Salesforce Sandbox yönetici hesabınızda oturum açın.

13. Sayfanın sağ üst köşesindeki **ayarlar simgesinin** altındaki **Kurulum** simgesine tıklayın.

    ![Tek İşaret-On'u Yapılandır](./media/salesforce-sandbox-tutorial/configure1.png)

14. Sol gezinti bölmesindeki **AYARLAR'a** gidin, ilgili bölümü genişletmek için **Kimlik'i** tıklatın. Ardından **Tek Oturum Açma Ayarları'nı**tıklatın.

    ![Tek İşaret-On'u Yapılandır](./media/salesforce-sandbox-tutorial/sf-admin-sso.png)

15. Tek **Oturum Açma Ayarları** **sayfasında, Düzelt** düğmesini tıklatın.

    ![Tek İşaret-On'u Yapılandır](./media/salesforce-sandbox-tutorial/configure3.png)

16. **SAML Etkin'i**seçin ve ardından **Kaydet'i**tıklatın.

    ![Tek İşaret-On'u Yapılandır](./media/salesforce-sandbox-tutorial/sf-enable-saml.png)

17. SAML tek oturum açma ayarlarınızı yapılandırmak için **Meta veri dosyasından Yeni'yi**tıklatın.

    ![Tek İşaret-On'u Yapılandır](./media/salesforce-sandbox-tutorial/sf-admin-sso-new.png)

18. meta data XML dosyasını yüklemek için **Dosyayı Seçin'i** tıklatın ve **Oluştur'u**tıklatın.

    ![Tek İşaret-On'u Yapılandır](./media/salesforce-sandbox-tutorial/xmlchoose.png)

19. **SAML Tek Oturum Açma Ayarları** sayfasında, alanlar otomatik olarak doldurulur, yapılandırmanın adını yazın (örneğin: *SPSSOWAAD_Test*), **Ad** metin kutusuna ve kaydet'i tıklatın.

    ![Tek İşaret-On'u Yapılandır](./media/salesforce-sandbox-tutorial/sf-saml-config.png)

20. Etki alanınızın Salesforce Sandbox'ta etkinleştirilmesini sağlamak için aşağıdaki adımları gerçekleştirin:

    > [!NOTE]
    > Etki alanını etkinleştirmeden önce Salesforce Sandbox'ta aynı sıyrıkları oluşturmanız gerekir. Daha fazla bilgi için [bkz.](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_define.htm&language=en_US) Etki alanı oluşturulduktan sonra, lütfen doğru şekilde yapılandırıldığından emin olun.

21. Salesforce Sandbox'taki sol gezinti bölmesinde, ilgili bölümü genişletmek için **Şirket Ayarları'nı** tıklatın ve ardından **Etki Alanım'ı**tıklatın.

    ![Tek İşaret-On'u Yapılandır](./media/salesforce-sandbox-tutorial/sf-my-domain.png)

22. Kimlik **Doğrulama Yapılandırması** **bölümünde, Düzenleme'yi**tıklatın.

    ![Tek İşaret-On'u Yapılandır](./media/salesforce-sandbox-tutorial/sf-edit-auth-config.png)

23. Kimlik **Doğrulama Yapılandırması** bölümünde, **Kimlik Doğrulama Hizmeti**olarak Salesforce Sandbox'ta SSO yapılandırması sırasında ayarlamış olduğunuz SAML Tek Oturum Açma Ayarı'nın adını seçin ve **Kaydet'i**tıklatın.

    ![Tek İşaret-On'u Yapılandır](./media/salesforce-sandbox-tutorial/configure2.png)

### <a name="create-salesforce-sandbox-test-user"></a>Salesforce Sandbox test kullanıcısı oluşturma

Bu bölümde, Salesforce Sandbox'ta Britta Simon adında bir kullanıcı oluşturulur. Salesforce Sandbox, varsayılan olarak etkinleştirilen tam zamanında sağlamayı destekler. Bu bölümde sizin için bir eylem öğesi yoktur. Salesforce Sandbox'ta bir kullanıcı zaten yoksa, Salesforce Sandbox'a erişmeye çalıştığınızda yeni bir kullanıcı oluşturulur. Salesforce Sandbox da otomatik kullanıcı sağlama destekler, otomatik kullanıcı sağlama yapılandırmak için nasıl daha fazla bilgi [burada](salesforce-sandbox-provisioning-tutorial.md) bulabilirsiniz.

## <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki Salesforce Sandbox kutucuğuna tıkladığınızda, SSO'yu kurduğunuz Salesforce Sandbox'ta otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Salesforce Sandbox'ı deneyin](https://aad.portal.azure.com/)

- [Microsoft Cloud App Security'de oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/protect-salesforce)

- [Kullanıcı Sağlama'yı Yapılandır](salesforce-sandbox-provisioning-tutorial.md)

- [Salesforce Sandbox'ı gelişmiş görünürlük ve kontrollerle nasıl koruyabilirsiniz?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)
