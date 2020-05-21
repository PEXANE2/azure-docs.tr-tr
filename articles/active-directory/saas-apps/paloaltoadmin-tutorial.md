---
title: 'Öğretici: Palo Alto Networks-yönetici kullanıcı arabirimi ile Azure Active Directory tümleştirme | Microsoft Docs'
description: Azure Active Directory ile Palo Alto Networks-admin kullanıcı arabirimi arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
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
ms.openlocfilehash: 9804a44a29f4540c28ec4e1eb6927e65af70218c
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2020
ms.locfileid: "83682956"
---
# <a name="tutorial-azure-active-directory-integration-with-palo-alto-networks---admin-ui"></a>Öğretici: Palo Alto Networks-admin kullanıcı arabirimi ile Azure Active Directory tümleştirme

Bu öğreticide, Palo Alto Networks-admin kullanıcı arabirimini Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz.
Azure AD ile Palo Alto Networks-admin kullanıcı arabirimine tümleştirme, aşağıdaki avantajları sağlar:

* Azure AD 'de, Palo Alto Networks-admin kullanıcı arabirimine erişimi olan bir denetim yapabilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla Networks-admin kullanıcı arabirimine (çoklu oturum açma) otomatik olarak oturum açmasını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz-Azure portal.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek istiyorsanız, bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/) .

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Palo Alto Networks-admin kullanıcı arabirimine göre yapılandırmak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Bir Azure AD ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü edinebilirsiniz
* Palo Alto Networks-admin UI çoklu oturum açma etkin abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açmayı bir test ortamında yapılandırıp test edersiniz.

* Palo Alto Networks-yönetici kullanıcı arabirimi **SP** tarafından başlatılan SSO 'yu destekler
* Palo Alto ağları-yönetici kullanıcı ARABIRIMI **, tam zamanında** Kullanıcı sağlamasını destekler

## <a name="adding-palo-alto-networks---admin-ui-from-the-gallery"></a>Galeriden Palo Alto Networks-admin kullanıcı arabirimine ekleme

Palo Alto Networks-admin kullanıcı arabirimini Azure AD 'ye tümleştirmeyi yapılandırmak için, galerisindeki Palo Alto Networks-admin kullanıcı arabirimini yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. [Azure Portal](https://portal.azure.com) iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Palo Alto Networks-admin kullanıcı arabirimini** yazın.
1. Sonuçlar panelinden **Palo Alto Networks-admin kullanıcı arabirimini** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma ve test etme

Bu bölümde, **B. Simon**adlı bir test kullanıcısına bağlı olarak, Palo Alto Networks-admin kullanıcı ARABIRIMINDEKI Azure AD çoklu oturum açmayı yapılandırıp test edersiniz.
Çoklu oturum açma için, bir Azure AD kullanıcısı ve Palo Alto Networks-admin kullanıcı ARABIRIMINDEKI ilgili Kullanıcı arasındaki bağlantı ilişkisinin kurulması gerekir.

Azure AD çoklu oturum açmayı, Palo Alto Networks-admin UI ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını gerçekleştirmeniz gerekir:

1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
    * Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -B. Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
    * Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek için.
1. Uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için, **[Palo Alto Networks-ADMIN UI SSO](#configure-palo-alto-networks---admin-ui-sso)** -öğesini yapılandırın.
    * **[Palo Alto Networks-ADMIN UI Test User](#create-palo-alto-networks---admin-ui-test-user)** -kullanıcının Azure AD gösterimine bağlı olan Networks-ADMIN Kullanıcı arabiriminde B. Simon 'un bir karşılığı olacak şekilde.
1. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Bu bölümde, Azure portal Azure AD çoklu oturum açma özelliğini etkinleştirirsiniz.

Azure AD çoklu oturum açmayı Palo Alto Networks-admin kullanıcı arabirimine göre yapılandırmak için aşağıdaki adımları uygulayın:

1. [Azure Portal](https://portal.azure.com/), **Palo Alto Networks-admin kullanıcı arabirimi** uygulama tümleştirmesi sayfasında, **Çoklu oturum açma**' yı seçin.

    ![Çoklu oturum açma bağlantısını yapılandırma](common/select-sso.png)

1. Çoklu oturum **açma yöntemi seç** iletişim kutusunda, çoklu oturum açmayı etkinleştirmek için **SAML/WS-Besme** modunu seçin.

    ![Çoklu oturum açma seçme modu](common/select-saml-option.png)

1. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **temel SAML yapılandırması** Iletişim kutusunu açmak için **Düzenle** simgesine tıklayın.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. **Temel SAML yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    a. **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<Customer Firewall FQDN>/php/login.php`

    b. **Tanımlayıcı** kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<Customer Firewall FQDN>:443/SAML20/SP`

    c. **Yanıt URL** 'si metin kutusunda, onaylama tüketici HIZMETI (ACS) URL 'sini aşağıdaki biçimde yazın:`https://<Customer Firewall FQDN>:443/SAML20/SP/ACS`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek oturum açma URL 'SI, tanımlayıcı ve yanıt URL 'siyle güncelleştirin. Bu değerleri almak için [Palo Alto Networks-ADMIN UI Client support ekibine](https://support.paloaltonetworks.com/support) başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.
    >
    > **Tanımlayıcı** üzerinde 443 numaralı bağlantı noktası ve **yanıt URL 'si** , bu değerler Palo Alto güvenlik duvarına kodlandığı için gereklidir. Bağlantı noktası numarasının kaldırılması, kaldırıldığında oturum açma sırasında hata oluşmasına neden olur.

    > **Tanımlayıcı** üzerinde 443 numaralı bağlantı noktası ve **yanıt URL 'si** , bu değerler Palo Alto güvenlik duvarına kodlandığı için gereklidir. Bağlantı noktası numarasının kaldırılması, kaldırıldığında oturum açma sırasında hata oluşmasına neden olur.

1. Genesys uygulamasına göre Purecyüksek, SAML belirteci öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML onayları bekler. Aşağıdaki ekran görüntüsünde varsayılan özniteliklerin listesi gösterilmektedir.

    ![image](common/default-attributes.png)

   > [!NOTE]
   > Öznitelik değerleri yalnızca örnekler olduğundan, *Kullanıcı adı* ve *adminrole*için uygun değerleri eşleyin. Güvenlik duvarındaki belirli sanal sistemlere yönetici erişimini kısıtlamak için kullanılan başka bir isteğe bağlı öznitelik olan *accessdomain*.

1. Genesys uygulamasına göre Purecsesli, daha fazla özniteliğin aşağıda gösterilen SAML yanıtına geri geçirilmesini bekler. Bu öznitelikler de önceden doldurulur, ancak gereksinimlerinize göre bunları gözden geçirebilirsiniz.

    | Name |  Kaynak özniteliği|
    | --- | --- |
    | kullanıcı adı | User. UserPrincipalName |
    | adminrole | customadmin |
    | | |

    > [!NOTE]
    > Öznitelikler hakkında daha fazla bilgi için aşağıdaki makalelere bakın:
    > * [Yönetici Kullanıcı arabirimi için yönetim rolü profili (adminrole)](https://www.paloaltonetworks.com/documentation/80/pan-os/pan-os/firewall-administration/manage-firewall-administrators/configure-an-admin-role-profile)
    > * [Yönetici Kullanıcı arabirimi (accessdomain) için cihaz erişimi etki alanı](https://docs.paloaltonetworks.com/pan-os/8-0/pan-os-web-interface-help/device/device-access-domain.html)

1. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imza sertifikası** bölümünde, **Federasyon meta veri XML** 'sini gereksiniminize göre belirtilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir** ' e tıklayın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

1. **Palo Alto Networks-admin kullanıcı arabirimini ayarlama** bölümünde uygun URL 'leri gereksiniminize göre kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD tanımlayıcısı

    c. Oturum kapatma URL 'SI

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, B. Simon adlı Azure portal bir test kullanıcısı oluşturacaksınız.

1. Azure portal sol bölmeden **Azure Active Directory**' i seçin, **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.
1. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.
1. **Kullanıcı** özellikleri ' nde şu adımları izleyin:
   1. **Ad** alanına `B.Simon` girin.  
   1. **Kullanıcı adı** alanına, girin username@companydomain.extension . Örneğin, `B.Simon@contoso.com`.
   1. **Parolayı göster** onay kutusunu seçin ve ardından **parola** kutusunda görüntülenen değeri yazın.
   1. **Oluştur**' a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Azure çoklu oturum açmayı kullanarak, Palo Alto Networks-admin kullanıcı arabirimine erişim vererek B. Simon 'u etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde, **Palo Alto Networks-admin kullanıcı arabirimi**' ni seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle**' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. SAML assertion 'da herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, Kullanıcı için listeden uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

### <a name="configure-palo-alto-networks---admin-ui-sso"></a>Palo Alto Networks-admin UI SSO 'yu yapılandırma

1. Yeni bir pencerede bir yönetici olarak Palo Alto Networks güvenlik duvarı yönetici kullanıcı arabirimini açın.

2. **Cihaz** sekmesini seçin.

    ![Cihaz sekmesi](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_admin1.png)

3. Sol bölmede, **SAML kimlik sağlayıcısı**' nı seçin ve ardından **içeri aktar** ' ı seçerek meta veri dosyasını içeri aktarın.

    ![Meta veri dosyasını Içeri aktar düğmesi](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_admin2.png)

4. **SAML sağlayıcı sunucu profili Içeri aktarmayı tanımla** bölümünde aşağıdakileri yapın:

    !["SAML kimlik sağlayıcısı sunucu profili Içeri aktarmayı tanımla" penceresi](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_idp.png)

    a. **Profil adı** kutusuna bir ad girin (örneğin, **AZUREAD admin kullanıcı arabirimi**).

    b. **Kimlik sağlayıcısı meta verileri**altında, **Araştır**' ı seçin ve daha önce Azure Portal indirdiğiniz Metadata. xml dosyasını seçin.

    c. **Kimlik sağlayıcısı sertifikasını doğrula** onay kutusunu temizleyin.

    d. **Tamam**’ı seçin.

    e. Güvenlik duvarında yapılandırmaların yürütülmesi için, **Yürüt**' ü seçin.

5. Sol bölmede, **SAML kimlik sağlayıcısı**' nı seçin ve ardından önceki ADıMDA oluşturduğunuz SAML kimlik sağlayıcı profilini (örneğin, **AZUREAD admin UI**) seçin.

    ![SAML kimlik sağlayıcısı profili](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_idp_select.png)

6. **SAML kimlik sağlayıcısı sunucu profili** penceresinde şunları yapın:

    !["SAML kimlik sağlayıcısı sunucu profili" penceresi](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_slo.png)
  
    a. **Kimlik sağlayıcısı SLO URL 'si** kutusunda, önceden Içe AKTARıLAN SLO URL 'SINI Şu URL ile değiştirin:`https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.0`
  
    b. **Tamam**’ı seçin.

7. Palo Alto Networks güvenlik duvarının Yönetici Kullanıcı arabiriminde **cihaz**' ı seçin ve ardından **yönetici rolleri**' ni seçin.

8. **Ekle** düğmesini seçin.

9. **Yönetici rolü profili** penceresinde, **ad** kutusunda, yönetici rolü için bir ad sağlayın (örneğin, **fwadmin**). Yönetici rolü adı, kimlik sağlayıcısı tarafından gönderilen SAML yönetici rolü özniteliği adıyla eşleşmelidir. Yönetici rolü adı ve değeri, Azure portal **Kullanıcı öznitelikleri** bölümünde oluşturulmuştur.

    ![Palo Alto Networks yönetici rolünü yapılandırma](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_adminrole.png)
  
10. Güvenlik duvarının Yönetici Kullanıcı arabiriminde **cihaz**' ı seçin ve ardından **kimlik doğrulama profili**' ni seçin.

11. **Ekle** düğmesini seçin.

12. **Kimlik doğrulama profili** penceresinde şunları yapın: 

    !["Kimlik doğrulama profili" penceresi](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_authentication_profile.png)

    a. **Ad** kutusunda bir ad sağlayın (örneğin, **AzureSAML_Admin_AuthProfile**).

    b. **Tür** açılan listesinde **SAML**' yi seçin. 

    c. **IDP sunucu profili** açılan listesinde, uygun SAML kimlik sağlayıcısı sunucu profilini (örneğin, **AZUREAD admin kullanıcı arabirimi**) seçin.

    c. **Tek Logout etkinleştir** onay kutusunu seçin.

    d. **Yönetici rolü öznitelik** kutusuna öznitelik adını (örneğin, **adminrole**) girin.

    e. **Gelişmiş** sekmesini seçin ve ardından **Izin verilenler listesinde** **Ekle**' yi seçin.

    ![Gelişmiş sekmesindeki Ekle düğmesi](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_allowlist.png)

    f. **Tümü** onay kutusunu seçin veya bu profille kimlik doğrulayabilecek kullanıcıları ve grupları seçin.  
    Bir kullanıcı kimliğini doğruladığında, güvenlik duvarı ilgili Kullanıcı adı veya grupla bu listedeki girdilerle eşleşir. Girdi eklememeniz durumunda hiçbir kullanıcının kimlik doğrulaması yapabilmeleri gerekmez.

    g. **Tamam**’ı seçin.

13. Yöneticilerin Azure kullanarak SAML SSO 'yu kullanmasını sağlamak için **cihaz**  >  **kurulumu**' nu seçin. **Kurulum** bölmesinde, **Yönetim** sekmesini seçin ve ardından **kimlik doğrulama ayarları**altında **Ayarlar** ("dişli") düğmesini seçin.

    ![Ayarlar düğmesi](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_authsetup.png)

14. Kimlik doğrulama profili penceresinde oluşturduğunuz SAML kimlik doğrulaması profilini seçin (örneğin, **AzureSAML_Admin_AuthProfile**).

    ![Kimlik doğrulama profili alanı](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_authsettings.png)

15. **Tamam**’ı seçin.

16. Yapılandırmayı yürütmek için **Yürüt**' ü seçin.

### <a name="create-palo-alto-networks---admin-ui-test-user"></a>Palo Alto ağları-yönetici UI test kullanıcısı oluşturma

Palo Alto Networks-yönetici kullanıcı ARABIRIMI, tam zamanında Kullanıcı sağlamayı destekler. Bir kullanıcı zaten mevcut değilse, başarılı bir kimlik doğrulamasından sonra otomatik olarak sistemde oluşturulur. Kullanıcı oluşturmanız için herhangi bir işlem yapmanız gerekmez.

### <a name="test-sso"></a>Test SSO 'SU

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim panelinde Palo Alto Networks-admin kullanıcı arabirimi kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız Palo Alto Networks-admin kullanıcı arabirimine otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)

- [Azure Active Directory Koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Palo Alto Networks-admin kullanıcı arabirimini deneyin](https://aad.portal.azure.com/)

- [Microsoft Cloud App Security oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [Gelişmiş görünürlük ve denetimlerle Palo Alto ağları-yönetici kullanıcı arabirimini koruma](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)