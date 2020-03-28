---
title: 'Öğretici: Palo Alto Networks ile Azure Active Directory entegrasyonu - Yönetici UI | Microsoft Dokümanlar'
description: Azure Active Directory ve Palo Alto Networks - Yönetici UI arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: a826eaec-15af-4c85-8855-8a3374d1efb9
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 03/12/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: f2fc9a23a8b0ef69760867dd5768510397f8570e
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80129783"
---
# <a name="tutorial-azure-active-directory-integration-with-palo-alto-networks---admin-ui"></a>Öğretici: Palo Alto Networks ile Azure Active Directory entegrasyonu - Yönetici UI

Bu eğitimde, Palo Alto Networks - Yönetici UI'yi Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
Palo Alto Ağlarını - Yönetici Kullanıcı Arabirimi'ni Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* Palo Alto Networks - Yönetici UI erişimi olan Azure AD'de kontrol edebilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla Palo Alto Networks - Yönetici Kullanıcı Arabirimi (Tek Oturum Açma) ile otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Palo Alto Networks - Yönetici Arabirimi ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü alabilirsiniz
* Palo Alto Networks - Yönetici UI tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* Palo Alto Networks - Yönetici UI **SP** başlatılan SSO destekler
* Palo Alto Networks - Yönetici UI **Just In Time** kullanıcı sağlama destekler

## <a name="adding-palo-alto-networks---admin-ui-from-the-gallery"></a>Palo Alto Networks ekleme - Yönetici UI galeriden

Palo Alto Networks -Yönetici UI) ile Azure AD entegrasyonunu yapılandırmak için galeriden Palo Alto Networks - Yönetici UI'yi yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. Galeri **bölümünden Ekle** bölümünde, arama **kutusuna Palo Alto Networks - Yönetici UI** yazın.
1. Sonuçlar panelinden **Palo Alto Networks - Yönetici UI'yi** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Azure AD oturum açma işlemlerini Palo Alto Networks - Yönetici Kullanıcı Arabirimi'ni **B.Simon**adlı bir test kullanıcısına göre yapılandırıp test emzebilirsiniz.
Tek oturum açmanın işe yaraması için, Palo Alto Networks'teki bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişkisi - Yönetici Kullanıcı Arabirimi'nin kurulması gerekir.

Azure AD oturumaçmaişlemlerini Palo Alto Networks - Yönetici Arabirimi ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    * Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    * B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[Palo Alto Networks 'ü (Yönetici UI SSO)](#configure-palo-alto-networks---admin-ui-sso)** uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için yapılandırın.
    * **[Palo Alto Networks 'te](#create-palo-alto-networks---admin-ui-test-user)** B.Simon'ın bir muadili olan Palo Alto Ağları -Yönetici Kullanıcı Arabirimi- Kullanıcının Azure AD gösterimine bağlı Yönetici Kullanıcı Arabirimi' ni oluşturun.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturum açma işlemlerini Palo Alto Networks - Yönetici Arabirimi ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında,](https://portal.azure.com/) **Palo Alto Networks - Yönetici Kullanıcı Arabirimi** uygulama tümleştirme sayfasında **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

1. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

1. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    a. Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://<Customer Firewall FQDN>/php/login.php`

    b. **Tanımlayıcı** kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<Customer Firewall FQDN>:443/SAML20/SP`

    c. **Yanıtla URL** metin kutusuna, Assertion Consumer Service (ACS) URL'sini aşağıdaki biçimde yazın:`https://<Customer Firewall FQDN>:443/SAML20/SP/ACS`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek Oturum Açma URL'si, Tanımlayıcı ve Yanıt URL'si ile güncelleştirin. Bu değerleri almak için [Palo Alto Networks - Yönetici UI İstemci destek ekibiyle](https://support.paloaltonetworks.com/support) iletişime geçin. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.
    >
    > Bu değerler Palo **Alto** Güvenlik Duvarı'na kodlanmış olduğundan Bağlantı Noktası 443 Tanımlayıcı ve **Yanıt URL'si** için gereklidir. Bağlantı noktası numarasının kaldırılması, kaldırılırsa oturum açma sırasında bir hataya neden olur.

    > Bu değerler Palo **Alto** Güvenlik Duvarı'na kodlanmış olduğundan Bağlantı Noktası 443 Tanımlayıcı ve **Yanıt URL'si** için gereklidir. Bağlantı noktası numarasının kaldırılması, kaldırılırsa oturum açma sırasında bir hataya neden olur.

1. Genesys uygulaması tarafından PureCloud, SAML belirteç öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML iddialarını bekler. Aşağıdaki ekran görüntüsü varsayılan özniteliklerin listesini gösterir.

    ![image](common/default-attributes.png)

   > [!NOTE]
   > Öznitelik değerleri yalnızca örnekler olduğundan, kullanıcı *adı* ve *yönetici rolü*için uygun değerleri eşle. Güvenlik duvarındaki belirli sanal sistemlere yönetici erişimini kısıtlamak için kullanılan başka bir isteğe bağlı öznitelik, *accessdomain*vardır.

1. Yukarıdakilere ek olarak, Genesys uygulaması tarafından PureCloud aşağıda gösterilen SAML yanıtı geri geçirilmesi için birkaç daha fazla öznitelikleri bekliyor. Bu öznitelikler de önceden doldurulur, ancak gereksinimlerinize göre bunları gözden geçirebilirsiniz.

    | Adı |  Kaynak Özniteliği|
    | --- | --- |
    | kullanıcı adı | user.userprincipalname |
    | adminrole | customadmin |
    | | |

    > [!NOTE]
    > Öznitelikler hakkında daha fazla bilgi için aşağıdaki makalelere bakın:
    > * [Yönetici AraBirimi için yönetim rol profili (yönetici rolü)](https://www.paloaltonetworks.com/documentation/80/pan-os/pan-os/firewall-administration/manage-firewall-administrators/configure-an-admin-role-profile)
    > * [Yönetici Arabirimi (accessdomain) için aygıt erişim etki alanı](https://www.paloaltonetworks.com/documentation/80/pan-os/web-interface-help/device/device-access-domain)

1. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, Federasyon **Metadata XML'ini** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

1. **Palo Alto Networks - Admin UI** bölümünü ayarla bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure Reklam Tanımlayıcısı

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

Bu bölümde, Palo Alto Ağları - Yönetici UI erişimi sağlayarak B.Simon'Un Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **Palo Alto Networks - Yönetici Kullanıcı Arama Bilgi Formu'nu**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

### <a name="configure-palo-alto-networks---admin-ui-sso"></a>Palo Alto Ağlarını Yapılandır - Yönetici UI SSO

1. Palo Alto Networks Güvenlik Duvarı Yönetici UI'sini yeni bir pencerede yönetici olarak açın.

2. **Aygıt** sekmesini seçin.

    ![Aygıt sekmesi](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_admin1.png)

3. Sol bölmede **SAML Identity Provider'ı**seçin ve meta veri dosyasını almak için **Içe Aktar'ı** seçin.

    ![Meta veri dosyasını içe aktar düğmesi](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_admin2.png)

4. **SAML Sağlayıcı Sunucu Profil Iİşlemet** penceresinde aşağıdakileri yapın:

    !["SAML Sağlayıcı Sunucu ProfilI Alma" penceresi](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_idp.png)

    a. Profil **Adı** kutusunda bir ad sağlayın (örneğin, **AzureAD Yönetici Kullanıcı Arabirimi).**

    b. **Kimlik Sağlayıcısı Metadata** **altında, Gözat'ı**seçin ve Azure portalından daha önce indirdiğiniz metadata.xml dosyasını seçin.

    c. Kimlik **Sağlayıcı Sertifikası** onay kutusunu temizleyin.

    d. **Tamam'ı**seçin.

    e. Güvenlik duvarındaki yapılandırmaları işlemek için **Commit'i**seçin.

5. Sol bölmede **SAML Identity Provider'ı**seçin ve ardından önceki adımda oluşturduğunuz SAML Identity Provider Profilini (örneğin, **AzureAD Yönetici UI)** seçin.

    ![SAML Kimlik Sağlayıcı Profili](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_idp_select.png)

6. **SAML Identity Provider Server Profile** penceresinde aşağıdakileri yapın:

    !["SAML Identity Provider Server Profile" penceresi](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_slo.png)
  
    a. Kimlik **Sağlayıcısı SLO URL** kutusunda, daha önce içe aktarılan SLO URL'yi aşağıdaki URL ile değiştirin:`https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.0`
  
    b. **Tamam'ı**seçin.

7. Palo Alto Networks Firewall'S Admin UI'de **Aygıt'ı**seçin ve ardından **Yönetici Rolleri'ni**seçin.

8. **Ekle** düğmesini seçin.

9. Yönetici **Rol Profili** penceresinde, **Ad** kutusunda yönetici rolü için bir ad sağlayın (örneğin, **fwadmin).** Yönetici rol adı, Kimlik Sağlayıcısı tarafından gönderilen SAML Yönetici Rolü özniteliği adıyla eşleşmelidir. Yönetici rol adı ve değeri Azure portalındaki **Kullanıcı Öznitelikleri** bölümünde oluşturuldu.

    ![Palo Alto Networks Yönetici Rolünü Yapılandır](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_adminrole.png)
  
10. Güvenlik Duvarı'nın Yönetici Arama İyisu'nda **Aygıt'ı**seçin ve ardından **Kimlik Doğrulama Profili'ni**seçin.

11. **Ekle** düğmesini seçin.

12. Kimlik **Doğrulama Profili** penceresinde aşağıdakileri yapın: 

    !["Kimlik Doğrulama Profili" penceresi](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_authentication_profile.png)

    a. **Ad** kutusunda bir ad sağlayın (örneğin, **AzureSAML_Admin_AuthProfile).**

    b. **Tür** açılır listesinde **SAML'yi**seçin. 

    c. **IdP Server Profili** açılır listesinde, uygun SAML Identity Provider Server profilini (örneğin, **AzureAD Yönetici UI)** seçin.

    c. Tek **Oturum Açma** yı etkinleştir onay kutusunu seçin.

    d. Yönetici **Rolü Özniteliği** kutusuna öznitelik adını girin (örneğin, **yönetici rolü).**

    e. **Gelişmiş** sekmesini seçin ve sonra, **İzin Listesi'nin**altında **Ekle'yi**seçin.

    ![Gelişmiş sekmesinde Ekle düğmesi](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_allowlist.png)

    f. **Tümü** onay kutusunu seçin veya bu profille kimlik doğrulaması yapabilecek kullanıcıları ve grupları seçin.  
    Bir kullanıcı kimlik doğruladığında, güvenlik duvarı ilişkili kullanıcı adı veya grubu bu listedeki girişlere göre eşleşir. Giriş eklemezseniz, hiçbir kullanıcı kimlik doğrulaması yapamaz.

    g. **Tamam'ı**seçin.

13. Yöneticilerin Azure'u kullanarak SAML SSO'su kullanmasını sağlamak için **Aygıt** > **Kurulumu'u**seçin. **Kurulum** bölmesinde **Yönetim** sekmesini seçin ve ardından Kimlik **Doğrulama Ayarları'nın**altında **Ayarlar** ("vites") düğmesini seçin.

    ![Ayarlar düğmesi](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_authsetup.png)

14. Kimlik Doğrulama Profili penceresinde oluşturduğunuz SAML Kimlik Doğrulama profilini seçin (örneğin, **AzureSAML_Admin_AuthProfile).**

    ![Kimlik Doğrulama Profili alanı](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_authsettings.png)

15. **Tamam'ı**seçin.

16. Yapılandırmayı işlemek için **Commit'i**seçin.

### <a name="create-palo-alto-networks---admin-ui-test-user"></a>Palo Alto Networks oluşturma - Yönetici UI test kullanıcısı

Palo Alto Networks - Yönetici Kullanıcı Arabirimi tam zamanında kullanıcı sağlamayı destekler. Bir kullanıcı zaten yoksa, başarılı bir kimlik doğrulamadan sonra sistemde otomatik olarak oluşturulur. Kullanıcıyı oluşturmak için sizden herhangi bir işlem gerekmez.

### <a name="test-sso"></a>Test SSO

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Panelindeki Palo Alto Networks - Yönetici UI döşemesini tıklattığınızda, SSO'yu kurduğunuz Palo Alto Networks - Admin UI'de otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Palo Alto Networks'ü deneyin - Azure AD ile Yönetici UI](https://aad.portal.azure.com/)

- [Microsoft Cloud App Security'de oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [Palo Alto Ağları nasıl korunur - Gelişmiş görünürlük ve kontrollerle Yönetici UI](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)