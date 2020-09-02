---
title: 'Öğretici: FortiGate SSL VPN ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve FortiGate SSL VPN arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 18a3d9d5-d81c-478c-be7e-ef38b574cb88
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 08/11/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: ecb53d661b1171f9c1b18d37d0bb35952645ba7e
ms.sourcegitcommit: 58d3b3314df4ba3cabd4d4a6016b22fa5264f05a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89299737"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-fortigate-ssl-vpn"></a>Öğretici: FortiGate SSL VPN ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory

Bu öğreticide, FortiGate SSL VPN 'i Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. FortiGate SSL VPN 'yi Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de, FortiGate SSL VPN 'ye erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla SSL VPN 'yi yasakladığından otomatik olarak oturum açmalarına olanak sağlayın.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

Azure AD ile SaaS uygulaması tümleştirmesi hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on).

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* FortiGate SSL VPN çoklu oturum açma (SSO) etkin aboneliği.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz.

* FortiGate SSL VPN, **SP** tarafından başlatılan SSO 'yu destekler
* FortiGate SSL VPN 'yi yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak ayıklanmasını ve zaman korumasını koruyan oturum denetimini zorunlu kılabilirsiniz. Oturum denetimi koşullu erişimden genişletiliyor. [Microsoft Cloud App Security ile oturum denetimini nasıl zorlayacağınızı öğrenin](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app).

## <a name="adding-fortigate-ssl-vpn-from-the-gallery"></a>Galeriden FortiGate SSL VPN ekleme

FortiGate SSL VPN 'nin tümleştirmesini Azure AD ile yapılandırmak için, Galeriden yönetilen SaaS uygulamaları listenize FortiGate SSL VPN eklemeniz gerekir.

1. [Azure Portal](https://portal.azure.com) iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **FORTIGATE SSL VPN** yazın.
1. Sonuçlar panelinden **Fortigate SSL VPN** ' i seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-sso-for-fortigate-ssl-vpn"></a>FortiGate SSL VPN için Azure AD SSO 'yu yapılandırma ve test etme

**B. Simon**adlı bir test kullanıcısı kullanarak, FortiGate SSL VPN Ile Azure AD SSO 'yu yapılandırın ve test edin. SSO 'nun çalışması için, FortiGate SSL VPN içindeki bir Azure AD kullanıcısı ve ilgili Kullanıcı arasında bir bağlantı ilişkisi oluşturmanız gerekir.

FortiGate SSL VPN ile Azure AD SSO 'yu yapılandırmak ve test etmek için aşağıdaki yapı taşlarını doldurun:

1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
    1. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -B. Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
    1. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek için.
1. Uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için **[FortiGate SSL VPN SSO 'Yu yapılandırın](#configure-fortigate-ssl-vpn-sso)** .
    1. User 'ın Azure AD gösterimine bağlı olan FortiGate SSL VPN 'de B. Simon 'ya sahip olmak için **FortiGate SSL VPN test kullanıcısı oluşturun** .
1. **[Test SSO](#test-single-sign-on)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. [Azure Portal](https://portal.azure.com/), **FORTIGATE SSL VPN** uygulama tümleştirmesi sayfasında, **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** için Düzenle/kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, aşağıdaki alanlar için değerleri girin:

    a. **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<FQDN>/remote/login`

    b. **Tanımlayıcı** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<FQDN>/remote/saml/metadata`

    c. **Yanıt URL 'si** metin kutusuna aşağıdaki kalıbı kullanarak bir URL yazın:`https://><FQDN/remote/saml/login`

    d. **Oturum kapatma URL** 'si metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<FQDN>/remote/saml/logout`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek oturum açma URL 'SI, tanımlayıcı, yanıt URL 'si ve oturum kapatma URL 'SI ile güncelleştirin. Bu değerleri almak için [Fortigate SSL VPN istemci desteği ekibine](mailto:tac_amer@fortinet.com) başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

1. Fortıgate SSL VPN uygulaması, SAML belirteci öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML onayları bekliyor. Aşağıdaki ekran görüntüsünde varsayılan özniteliklerin listesi gösterilmektedir.

    ![image](common/default-attributes.png)

1. FortiGate SSL VPN uygulaması, yukarıdakine ek olarak aşağıda gösterilen SAML yanıtına daha fazla öznitelik geçirilmesini bekler. Bu öznitelikler de önceden doldurulur, ancak gereksinimlerinize göre bunları gözden geçirebilirsiniz.
    
    | Name |  Kaynak özniteliği|
    | ------------ | --------- |
    | username | User. UserPrincipalName |
    | group | Kullanıcı. gruplar |

1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, **SAML Imzalama sertifikası** bölümünde **sertifika bulun (base64)** ve sertifikayı indirip bilgisayarınıza kaydetmek için **İndir** ' i seçin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. **FortiGate SSL VPN ayarla** bölümünde, gereksiniminize göre uygun URL 'leri kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, B. Simon adlı Azure portal bir test kullanıcısı oluşturacaksınız.

1. Azure portal sol bölmeden **Azure Active Directory**' i seçin, **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.
1. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.
1. **Kullanıcı** özellikleri ' nde şu adımları izleyin:
   1. **Ad** alanına `B.Simon` girin.  
   1. **Kullanıcı adı** alanına, girin username@companydomain.extension . Örneğin, `B.Simon@contoso.com`.
   1. **Parolayı göster** onay kutusunu seçin ve ardından **parola** kutusunda görüntülenen değeri yazın.
   1. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, FortiGate SSL VPN 'ye erişim izni vererek Azure çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde **Fortigate SSL VPN**' yi seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle**' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. SAML assertion 'da herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, Kullanıcı için listeden uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

### <a name="create-a-security-group-for-the-test-user"></a>Test kullanıcısı için bir güvenlik grubu oluşturma

Bu bölümde, test kullanıcısı için Azure Active Directory bir güvenlik grubu oluşturacaksınız. Bu güvenlik grubu, VPN üzerinden Kullanıcı ağ erişimi sağlamak için FortiGate tarafından kullanılacaktır.

1. Azure portal sol bölmeden **Azure Active Directory**' i seçin ve ardından **gruplar**' ı seçin.
1. Ekranın üst kısmındaki **Yeni Grup** ' u seçin.
1. **Yeni Grup** özelliklerinde şu adımları izleyin:
   1. **Grup türü** alanında **güvenlik**' i seçin.
   1. **Ad** alanına `FortiGateAccess` girin.
   1. **Grup açıklaması** alanına, girin `Group for granting FortiGate VPN access` .
   1. **Azure AD rolleri grup (Önizleme) ayarlarına atanabilir** , **Hayır**' ı seçin.
   1. **Üyelik türü** alanında, **atandı**' ı seçin.
   1. **Üyeler**altında, **Seçili üye yok**' u seçin.
   1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
   1. **Oluştur**’u seçin.
1. Azure Active Directory içindeki **gruplar** dikey penceresine geri döntikten sonra, **Fortigate erişim** grubunu bulun ve daha sonra kullanmak üzere **nesne kimliğini** aklınızda olun.

## <a name="configure-fortigate-ssl-vpn-sso"></a>FortiGate SSL VPN SSO 'yu yapılandırma

### <a name="upload-the-base64-saml-certificate-to-the-fortigate-appliance"></a>FortiGate gerecine Base64 SAML sertifikası yükleme

Kiracınızdaki FortiGate uygulamasının SAML yapılandırmasını tamamladıktan sonra Base64 kodlamalı SAML sertifikasını indirdiniz. Bu, FortiGate gerecine yüklenmelidir:

1. FortiGate gerecinizin yönetim portalında oturum açın.
1. Sol taraftaki menüde **sistem**' e tıklayın.
1. **Sistem**altında **Sertifikalar**' a tıklayın.
1. Uzak **sertifikayı içeri aktar**' a tıklayın  ->  **Remote Certificate**.
1. Azure kiracısındaki FortiGate uygulama dağıtımından indirilen sertifikaya gidin, seçin ve **Tamam 'a** tıklayın

Sertifika karşıya yüklendikten sonra, **sistem**  >  **sertifikaları**  >  **uzak sertifikası**altında adını göz önünde atın. Varsayılan olarak, REMOTE_Cert_**n** olarak adlandırılır; burada **n** , bir tamsayı değeridir.

### <a name="perform-fortigate-command-line-configuration"></a>FortiGate komut satırı yapılandırmasını gerçekleştirme

Aşağıdaki adımlarda Azure oturum kapatma URL 'sinin yapılandırılması gerekir. Bu URL bir soru işareti (?) içeriyor. Bu karakteri başarıyla göndermek için özel adımlar gerekir. Adımlar FortiGate CLı konsolundan gerçekleştirilemez. Bunun yerine, PuTTY gibi bir araç kullanarak FortiGate Applicance için bir SSH oturumu kurun. FortiGate gereci bir Azure sanal makinesi ise, Azure sanal makine seri konsolundan aşağıdaki adımları gerçekleştirebilirsiniz.

Bu adımları gerçekleştirmek için, daha önce kaydedilen değerlere ihtiyacınız olacaktır:

- Varlık KIMLIĞI
- Yanıt URL'si
- Oturum kapatma URL 'SI
- Azure oturum açma URL 'SI
- Azure AD tanımlayıcısı
- Azure oturum kapatma URL 'SI
- Base64 SAML sertifikası adı (REMOTE_Cert_N)

1. FortiGate Applicance için bir SSH oturumu oluşturun ve bir FortiGate yönetici hesabıyla oturum açın.
1. Aşağıdaki komutları gerçekleştirin:

   ```console
    config user saml
    edit azure
    set entity-id <Entity ID>
    set single-sign-on-url <Reply URL>
    set single-logout-url <Logout URL>
    set idp-single-sign-on-url <Azure Login URL>
    set idp-entity-id <Azure AD Identifier>
    set idp-single-logout-url <Azure Logout URL>
    set idp-cert <Base64 SAML Certificate Name>
    set user-name username
    set group-name group
    end

   ```

   > [!NOTE]
   > **Azure oturum kapatma URL 'si** bir `?` karakter içeriyor. FortiGate seri konsolunun URL 'sini doğru şekilde sağlamak için özel bir anahtar sırası girmeniz gerekir. URL genellikle olur `https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.0` .
   >
   > Seri konsoluna Azure Logout URL 'sini girmek için girin `set idp-single-logout-url https://login.microsoftonline.com/common/wsfederation` .
   > 
   > Ardından, CTRL + V ' yi seçin ve satırı tamamladıktan sonra URL 'nin geri kalanını yapıştırın: `set idp-single-logout-url https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.0` .

### <a name="configure-fortigate-for-group-matching"></a>Grup eşleştirme için FortiGate yapılandırma

Bu bölümde, FortiGate ' i test kullanıcısının bulunduğu güvenlik grubunun nesne kimliğini tanıyacak şekilde yapılandıracaksınız. Bu, FortiGate 'in bu grup üyeliğine göre erişim kararları almasına izin verir.

Bu adımları gerçekleştirmek için, daha önce oluşturulmuş olan **Fortigateaccess** güvenlik grubunun nesne kimliği gerekir

1. FortiGate Applicance için bir SSH oturumu kurun ve bir FortiGate yönetici hesabıyla oturum açın.
1. Aşağıdaki komutları gerçekleştirin:

   ```
    config user group
    edit FortiGateAccess
    set member azure
    config match
    edit 1
    set server-name azure
    set group-name <Object Id>
    next
    end
    next
    end
   ```

### <a name="create-fortigate-vpn-portals-and-firewall-policy"></a>FortiGate VPN portalları ve güvenlik duvarı Ilkesi oluşturma

Bu bölümde, yukarıda oluşturulan **Fortigateaccess** güvenlik grubuna erişim Izni veren FortiGate VPN portallarını ve güvenlik duvarı ilkesini yapılandırırsınız.

Fortigate VPN platformuna VPN portalları ve güvenlik duvarı Ilkesi eklemek için [Fortigate destek ekibi](mailto:tac_amer@fortinet.com) ile çalışın. Çoklu oturum açma kullanılmadan önce bu adımların tamamlanmış olması gerekir.

## <a name="test-single-sign-on"></a>Çoklu oturum açmayı sına 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim panelinde FortiGate SSL VPN kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız FortiGate SSL VPN ' de otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

Microsoft ve FortiGate, en iyi Son Kullanıcı deneyimi için, Fortinet VPN istemcisini FortiClient kullanmanızı önerir.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile SSL VPN 'yi deneyin](https://aad.portal.azure.com/)

- [Microsoft Cloud App Security oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)
