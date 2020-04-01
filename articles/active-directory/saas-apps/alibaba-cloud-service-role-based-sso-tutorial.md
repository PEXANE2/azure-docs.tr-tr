---
title: 'Öğretici: Alibaba Cloud Service (Role tabanlı SSO) ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Alibaba Cloud Service (Role tabanlı SSO) arasında tek oturum açma işlemlerini nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 3667841e-acfc-4490-acf5-80d9ca3e71e8
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 09/13/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: e22bec224d185d0306f2b0032aef929f627c910e
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77367928"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-alibaba-cloud-service-role-based-sso"></a>Öğretici: Alibaba Cloud Service (Rol Tabanlı SSO) ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, Alibaba Bulut Hizmeti'ni (Rol Tabanlı SSO) Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. Alibaba Bulut Hizmeti'ni (Rol Tabanlı SSO) Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Alibaba Bulut Hizmeti'ne (Rol tabanlı SSO) erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla Alibaba Bulut Hizmeti'nde (Rol Tabanlı SSO) otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Alibaba Cloud Service (Role tabanlı SSO) tek oturum açma (SSO) aboneliği ni etkinleştirdi.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* Alibaba Bulut Hizmeti (Role tabanlı SSO) **IDP** başlatılan SSO destekler

## <a name="adding-alibaba-cloud-service-role-based-sso-from-the-gallery"></a>Galeriden Alibaba Bulut Hizmeti (Rol Tabanlı SSO) ekleme

Alibaba Bulut Hizmeti'nin (Rol Tabanlı SSO) Azure AD'ye entegrasyonunu yapılandırmak için galeriden Yönetilen SaaS uygulamaları listenize Alibaba Bulut Hizmeti 'ni (Rol tabanlı SSO) eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Alibaba Bulut Hizmeti (Rol Tabanlı SSO)** yazın.
1. Sonuç panelinden **Alibaba Bulut Hizmeti'ni (Rol Tabanlı SSO)** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.
5. **Alibaba Bulut Hizmeti (Rol Tabanlı SSO)** sayfasında, sol taraftaki gezinti bölmesinde **Özellikler'i** tıklatın ve **nesne kimliğini** kopyalayın ve sonraki kullanım için bilgisayarınıza kaydedin.

    ![Özellikleri config](./media/alibaba-cloud-service-role-based-sso-tutorial/Properties.png)


## <a name="configure-and-test-azure-ad-single-sign-on-for-alibaba-cloud-service-role-based-sso"></a>Alibaba Bulut Hizmeti (Role tabanlı SSO) için Azure AD oturum açma işlemlerini yapılandırma ve test edin

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak Alibaba Bulut Hizmeti (Rol Tabanlı SSO) ile yapılandırın ve test edin. SSO'nun çalışması için, Bir Azure REKLAM kullanıcısı ile Alibaba Bulut Hizmeti'ndeki (Rol Tabanlı SSO) ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Alibaba Bulut Hizmeti (Rol Tabanlı SSO) ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    1. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    1. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
2. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Alibaba Bulut Hizmetinde Rol Tabanlı Tek Oturum](#configure-role-based-single-sign-on-in-alibaba-cloud-service)** Açma'yı yapılandırın.
    1. **[Alibaba Cloud Service (Role based SSO) SSO'yu uygulama](#configure-alibaba-cloud-service-role-based-sso-sso)** tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için yapılandırın.
    1. **[Alibaba Bulut Hizmeti (Rol Tabanlı SSO) test kullanıcısı oluşturun](#create-alibaba-cloud-service-role-based-sso-test-user)** - Kullanıcının Azure AD gösterimine bağlı Alibaba Bulut Hizmeti'nde (Rol tabanlı SSO) Britta Simon'ın bir örneğine sahip olmak için.
3. **[Yapılandırmanın](#test-sso)** çalışıp çalışmadığını doğrulamak için tek SSO'yu test edin.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında,](https://portal.azure.com/) **Alibaba Bulut Hizmeti (Role tabanlı SSO)** uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve **tek oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde, **Servis Sağlayıcı meta veri dosyanız**varsa aşağıdaki adımları gerçekleştirin:

    >[!NOTE]
    >Bu [URL'den](https://signin.alibabacloud.com/saml-role/sp-metadata.xml) Servis Sağlayıcı meta verilerini alırsınız

    a. **Meta veri dosyalarını yükle'yi**tıklatın.

    b. Meta veri dosyasını seçmek için **klasör logosuna** tıklayın ve **Yükle'yi**tıklatın.

    c. Meta veri dosyası başarıyla yüklendikten sonra, **Tanımlayıcı** ve **YanıtURL** değerleri Alibaba Bulut Hizmeti (Rol tabanlı SSO) bölümü textbox otomatik doldurulur olsun:

    > [!Note]
    > **Tanımlayıcı** ve **YanıtURL** değerleri otomatik doldurulmuyorsa, gereksiniminize göre değerleri el ile doldurun.

1. Alibaba Bulut Hizmeti (Rol Tabanlı SSO), rollerin Azure AD'de yapılandırılmasını gerektirir. Rol iddiası önceden yapılandırıldı, bu nedenle yapılandırmanız gerekmeyecek, ancak yine de bu makaleyi kullanarak Azure [AD'de](https://docs.microsoft.com/azure/active-directory/develop/active-directory-enterprise-app-role-management)oluşturmanız gerekir.

1. **SAML İmza Sertifikası** bölümünde **SAML ile tek oturum açma'da** **Federation Metadata XML'i** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

1. **Alibaba Bulut Hizmeti (Rol Tabanlı SSO) Kurulumu** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

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

Bu bölümde, B.Simon'ın Alibaba Bulut Hizmeti'ne (Rol tabanlı SSO) erişim sağlayarak Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **Alibaba Bulut Hizmeti'ni (Rol tabanlı SSO)** seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** sekmesinde, kullanıcı listesinden u2'yu seçin ve **Seç'i**tıklatın. Ardından, **Atla'yı**tıklatın.

    ![Test config](./media/alibaba-cloud-service-role-based-sso-tutorial/test01.png)

1. Atanan rolü görüntüleyin ve Alibaba Bulut Hizmeti'ni (Rol tabanlı SSO) test edin.

    ![Test config](./media/alibaba-cloud-service-role-based-sso-tutorial/test02.png)

    >[!NOTE]
    >Kullanıcıyı (u2) atadıktan sonra oluşturulan rol kullanıcıya otomatik olarak eklenir. Birden çok rol oluşturduysanız, gerektiğinde kullanıcıya uygun rolü eklemeniz gerekir. Azure AD'den birden çok Alibaba Cloud hesabına rol tabanlı SSO uygulamak istiyorsanız, önceki adımları yineleyin.

## <a name="configure-role-based-single-sign-on-in-alibaba-cloud-service"></a>Alibaba Bulut Hizmetinde Rol Tabanlı Tek Oturum Açma'yı Yapılandır

1. Hesap1'i kullanarak Alibaba Cloud [RAM konsolunda](https://account.alibabacloud.com/login/login.htm?oauth_callback=https%3A%2F%2Fram.console.aliyun.com%2F%3Fspm%3Da2c63.p38356.879954.8.7d904e167h6Yg9) oturum açın.

2. Sol taraftaki gezinti bölmesinde **SSO'yu**seçin.

3. Rol **tabanlı SSO** sekmesinde, **IdP oluştur'u**tıklatın.

4. Görüntülenen sayfada, IdP Adı alanına `AAD` girin, **Not** alanına bir açıklama girin, daha önce indirdiğiniz federasyon meta veri dosyasını yüklemek için **Yükle'yi** tıklatın ve **Tamam'ı**tıklatın.

5. IdP başarıyla oluşturulduktan **sonra, RAM Rolü Oluştur'u**tıklatın.

6. RAM **Rol Adı** alanına `AADrole`girin `AAD` , **IdP** açılır listesini seçin ve Tamam'ı tıklatın.

    >[!NOTE]
    >Gerektiğinde rol için izin verebilirsiniz. IdP'yi ve ilgili rolü oluşturduktan sonra, IdP'nin ARN'larını ve sonraki kullanım rolünü kaydetmenizi öneririz. ArN'ları IdP bilgi sayfasından ve rol bilgileri sayfasından edinebilirsiniz.

7. Alibaba Cloud RAM rolünü (AADrole) Azure AD kullanıcısıyla ilişkilendirin: RAM rolünü Azure AD kullanıcısıyla ilişkilendirmek için aşağıdaki adımları izleyerek Azure AD'de bir rol oluşturmanız gerekir:

    a. Microsoft Graph [Explorer'da](https://developer.microsoft.com/graph/graph-explorer)oturum açın.

    b. Rol oluşturmak için gerekli izinleri almak için **İzinleri değiştir'i** tıklatın.

    ![Grafik config](./media/alibaba-cloud-service-role-based-sso-tutorial/graph01.png)

    c. Listeden aşağıdaki izinleri seçin ve aşağıdaki şekilde gösterildiği gibi **İzinleri Değiştir'i**tıklatın.

    ![Grafik config](./media/alibaba-cloud-service-role-based-sso-tutorial/graph02.png)

    >[!NOTE]
    >İzinler verildikten sonra, Grafik Gezgini'nde yeniden oturum açın.

    d. Grafik Gezgini sayfasında, ilk açılan listeden **AL'ı** ve ikinci açılır listeden **beta'yı** seçin. Ardından `https://graph.microsoft.com/beta/servicePrincipals` açılan listelerin yanındaki alana girin ve **Sorguyu Çalıştır'ı**tıklatın.

    ![Grafik config](./media/alibaba-cloud-service-role-based-sso-tutorial/graph03.png)

    >[!NOTE]
    >Birden çok dizin kullanıyorsanız, `https://graph.microsoft.com/beta/contoso.com/servicePrincipals` sorgu alanına girebilirsiniz.

    e. Yanıt **Önizleme** bölümünde, sonraki kullanım için appRoles özelliğini 'Hizmet Sorumlusu'ndan ayıklayın.

    ![Grafik config](./media/alibaba-cloud-service-role-based-sso-tutorial/graph05.png)

    >[!NOTE]
    >Sorgu alanına girerek `https://graph.microsoft.com/beta/servicePrincipals/<objectID>` appRoles özelliğini bulabilirsiniz. Azure AD `objectID` **Özellikleri** sayfasından kopyaladığınız nesne kimliğinin bu olduğunu unutmayın.

    f. Graph Explorer'a geri dön, yöntemi **GET'den** **PATCH'e**değiştir, aşağıdaki içeriği **İstek Gövdesi** bölümüne yapıştır ve **Sorguyu Çalıştır'ı**tıklatın:
    ```
    { 
    "appRoles": [
        { 
        "allowedMemberTypes":[
            "User"
        ],
        "description": "msiam_access",
        "displayName": "msiam_access",
        "id": "41be2db8-48d9-4277-8e86-f6d22d35****",
        "isEnabled": true,
        "origin": "Application",
        "value": null
        },
        { "allowedMemberTypes": [
            "User"
        ],
        "description": "Admin,AzureADProd",
        "displayName": "Admin,AzureADProd",
        "id": "68adae10-8b6b-47e6-9142-6476078cdbce",
        "isEnabled": true,
        "origin": "ServicePrincipal",
        "value": "acs:ram::187125022722****:role/aadrole,acs:ram::187125022722****:saml-provider/AAD"
        }
    ]
    }
    ```
    > [!NOTE]
    > IdP'nin ARN'ları ve RAM konsolunda oluşturduğunuz `value` roldür. Burada, gerektiğinde birden çok rol ekleyebilirsiniz. Azure AD, SAML yanıtında talep değeri olarak bu rollerin değerini gönderir. Ancak, yalnızca yama işlemi `msiam_access` için parçadan sonra yeni roller ekleyebilirsiniz. Oluşturma işlemini düzeltmek için, gerçek zamanlı olarak kimlik oluşturmak için GUID Generator gibi bir kimlik jeneratörü kullanmanızı öneririz.

    g. 'Hizmet Sorumlusu' gerekli rolü yazdıktan sonra, öğreticinin Azure AD test kullanıcı bölümünü **ata** yetkilendir'in adımlarını izleyerek rolü Azure AD kullanıcısına (u2) takın.

### <a name="configure-alibaba-cloud-service-role-based-sso-sso"></a>Alibaba Bulut Hizmeti (Role tabanlı SSO) SSO'da yapılandırın

**Alibaba Bulut Hizmeti (Rol Tabanlı SSO)** tarafında tek oturum açma yapılandırmak için, indirilen **Federasyon Metadata XML'ini** ve uygun kopyalanmış URL'lerini Azure portalından [Alibaba Bulut Hizmeti (Rol tabanlı SSO) destek ekibine](https://www.aliyun.com/service/)göndermeniz gerekir. Bu ayarı, SAML SSO bağlantısının her iki tarafta da düzgün bir şekilde ayarlanması için ayarlarlar.

### <a name="create-alibaba-cloud-service-role-based-sso-test-user"></a>Alibaba Bulut Hizmeti (Rol Tabanlı SSO) test kullanıcısı oluşturma

Bu bölümde, Alibaba Bulut Hizmeti'nde (Rol tabanlı SSO) Britta Simon adında bir kullanıcı oluşturursunuz. Kullanıcıları Alibaba Bulut Hizmeti (Rol Tabanlı SSO) platformuna eklemek için [Alibaba Cloud Service (Rol Tabanlı SSO) destek ekibiyle](https://www.aliyun.com/service/) birlikte çalışın. Tek oturum açmadan önce kullanıcılar oluşturulmalı ve etkinleştirilmelidir.

## <a name="test-sso"></a>Test SSO 

Önceki yapılandırmalar tamamlandıktan sonra, Aşağıdaki adımları izleyerek Alibaba Bulut Hizmeti'ni (Rol Tabanlı SSO) test edin:

1. Azure **portalında, Alibaba Bulut Hizmeti (Rol Tabanlı SSO)** sayfasına gidin, **Tek oturum açma**'yı seçin ve **Test'i**tıklatın.

    ![Test config](./media/alibaba-cloud-service-role-based-sso-tutorial/test03.png)

2. **Geçerli kullanıcı olarak oturum aç**'a tıklayın.

    ![Test config](./media/alibaba-cloud-service-role-based-sso-tutorial/test04.png)

3. Hesap seçimi sayfasında u2'yu seçin.

    ![Test config](./media/alibaba-cloud-service-role-based-sso-tutorial/test05.png)

4. Aşağıdaki sayfa, rol tabanlı SSO'nun başarılı olduğunu gösteren görüntülenir.

    ![Test config](./media/alibaba-cloud-service-role-based-sso-tutorial/test06.png)

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Alibaba Bulut Hizmetini (Rol Tabanlı SSO) deneyin](https://aad.portal.azure.com/)

