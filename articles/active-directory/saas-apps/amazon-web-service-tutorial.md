---
title: 'Öğretici: Azure Active Directory tek oturum açma (SSO) Amazon Web Hizmetleri (AWS) ile entegrasyon | Microsoft Dokümanlar'
description: Azure Active Directory ve Amazon Web Hizmetleri (AWS) arasında tek oturum açma işlemlerini nasıl yapılandırabileceğinizi öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 7561c20b-2325-4d97-887f-693aa383c7be
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 01/31/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 40fd8217285643aa7d706d194d7f78ba0634dd32
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80048956"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-amazon-web-services-aws"></a>Öğretici: Azure Active Directory tek oturum açma (SSO) Tümleştirmesi ile Amazon Web Services (AWS)

Bu eğitimde, Amazon Web Hizmetleri'ni (AWS) Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. Amazon Web Services'ı (AWS) Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Amazon Web Hizmetleri'ne (AWS) erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Amazon Web Hizmetleri'nde (AWS) otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

![Azure AD ve AWS ilişkisi diyagramı](./media/amazon-web-service-tutorial/tutorial_amazonwebservices_image.png)

Birden çok örnek için birden çok tanımlayıcıyı yapılandırabilirsiniz. Örnek:

* `https://signin.aws.amazon.com/saml#1`

* `https://signin.aws.amazon.com/saml#2`

Bu değerlerle Azure AD, **#** SAML belirtecindeki `https://signin.aws.amazon.com/saml` hedef kitle URL'si olarak doğru değeri kaldırır ve doğru değeri gönderir.

Bu yaklaşımı aşağıdaki nedenlerle öneririz:

- Her uygulama size benzersiz bir X509 sertifikası sağlar. Bir AWS uygulama örneğinin her örneğinin, tek tek AWS hesabı bazında yönetilebilen farklı bir sertifika son kullanma tarihi olabilir. Bu durumda genel sertifika devri daha kolaydır.

- Azure AD'de bir AWS uygulamasıyla kullanıcı sağlamayı etkinleştirebilirsiniz ve ardından hizmetimiz söz konusu AWS hesabından tüm rolleri getirir. Uygulamadaki AWS rollerini el ile eklemeniz veya güncellemeniz gerekmez.

- Uygulama sahibini uygulama için ayrı ayrı atayabilirsiniz. Bu kişi uygulamayı doğrudan Azure AD'de yönetebilir.

> [!Note]
> Yalnızca bir galeri uygulaması kullandığınızdan emin olun.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* AWS tek oturum açma (SSO) özellikli abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* Amazon Web Services (AWS) **SP ve IDP** SSO başlatılan destekler
* Amazon Web Hizmetlerini (AWS) yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak sızma ve sızmalarını koruyan Oturum Denetimi'ni uygulayabilirsiniz. Oturum Denetimi Koşullu Erişim'den genişletir. [Microsoft Cloud App Security ile oturum denetimini nasıl uygulayacağınızı öğrenin](https://docs.microsoft.com/cloud-app-security/proxy-deployment-aad)

> [!NOTE]
> Bu uygulamanın tanımlayıcısı sabit bir dize değeridir, bu nedenle yalnızca bir örnek bir kiracıda yapılandırılabilir.

## <a name="adding-amazon-web-services-aws-from-the-gallery"></a>Galeriden Amazon Web Hizmetleri (AWS) ekleme

Amazon Web Hizmetleri'nin (AWS) Azure AD'ye entegrasyonunu yapılandırmak için, galeriden yönetilen SaaS uygulamaları listenize Amazon Web Hizmetleri 'ni (AWS) eklemeniz gerekir.

1. Bir iş hesabı, okul hesabı veya kişisel Microsoft hesabı kullanarak [Azure portalında](https://portal.azure.com) oturum açın.
1. Azure portalında Azure Etkin **Dizini'ni**arayın ve seçin.
1. Azure Active Directory genel bakış menüsünde **Kurumsal Uygulamalar** > **Tüm uygulamaları**seçin.
1. Uygulama eklemek için **Yeni uygulama'yı** seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Amazon Web Hizmetleri (AWS)** yazın.
1. Sonuç panelinden **Amazon Web Services'ı (AWS)** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-amazon-web-services-aws"></a>Amazon Web Hizmetleri (AWS) için Azure AD oturum açma işlemlerini yapılandırma ve test edin

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak Amazon Web Hizmetleri (AWS) ile yapılandırın ve test edin. SSO'nun çalışması için, bir Azure REKLAM kullanıcısı ile Amazon Web Hizmetleri'ndeki (AWS) ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Amazon Web Hizmetleri (AWS) ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    1. Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    1. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[Amazon Web Services (AWS) SSO'yu uygulama](#configure-amazon-web-services-aws-sso)** tarafındaki tek oturum açma ayarlarını yapılandırmak için yapılandırın.
    1. Kullanıcının Azure AD gösterimine bağlı Amazon Web Hizmetleri 'nde (AWS) B.Simon'ın bir muadili olması için **[Amazon Web Services (AWS) test kullanıcısı](#create-amazon-web-services-aws-test-user)** oluşturun.
    1. **[Amazon Web Services (AWS)'de rol sağlama nasıl yapılandırılır?](#how-to-configure-role-provisioning-in-amazon-web-services-aws)**
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında,](https://portal.azure.com/) **Amazon Web Services (AWS)** uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve tek **oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde, uygulama önceden yapılandırılmıştır ve gerekli URL'ler Azure ile önceden doldurulmuştür. Kullanıcının **Kaydet'i**seçerek yapılandırmayı kaydetmesi gerekiyor.

1. Birden fazla örneği yapılandırıyorsanız, tanımlayıcı değeri sağlayın. İkinci örnekten itibaren, benzersiz bir SPN **#** değeri belirtmek için bir işaret de dahil olmak üzere aşağıdaki biçimi kullanın.

    `https://signin.aws.amazon.com/saml#2`

1. **SAML İmza Sertifikası** bölümünde **SAML ile tek oturum açma'da** **Federation Metadata XML'i** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

1. Amazon **Web Services (AWS) Kurulumu** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, Azure portalında B.Simon adında bir test kullanıcısı oluşturursunuz.

1. Azure portalında Azure Etkin **Dizini'ni**arayın ve seçin.
1. Azure Active Directory genel bakış menüsünde, **Tüm** > **Kullanıcıları**Seçin.
1. Ekranın üst kısmında **Yeni kullanıcı** yı seçin.
1. **Kullanıcı** özelliklerinde aşağıdaki adımları izleyin:
   1. **Ad** alanına `B.Simon` girin.  
   1. Kullanıcı **adı** alanına. username@companydomain.extension Örneğin, `B.Simon@contoso.com`.
   1. **Parolayı Göster** onay kutusunu seçin ve ardından **Parola** kutusunda görüntülenen değeri yazın.
   1. **Oluştur'u**tıklatın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, B.Simon'ın Amazon Web Hizmetleri'ne (AWS) erişim sağlayarak Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında Azure Etkin **Dizini'ni**arayın ve seçin.
1. Azure Active Directory genel bakış menüsünde **Kurumsal Uygulamalar** > **Tüm uygulamaları**seçin.
1. Uygulama **listesinde, Amazon Web Services (AWS) seçin.**
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-amazon-web-services-aws-sso"></a>Yapılandırma Amazon Web Services (AWS) SSO

1. Farklı bir tarayıcı penceresinde, Yönetici olarak AWS şirket sitenize oturum açın.

2. **AWS Ana Sayfa'yı**seçin.

    ![AWS Home simgesi vurgulanmış AWS şirket sitesinin ekran görüntüsü][11]

3. **Kimlik ve Erişim Yönetimi'ni**seçin.

    ![IAM vurgulanmış AWS hizmetleri sayfasının ekran görüntüsü][12]

4. **Kimlik Sağlayıcılar** > **Oluştur Sağlayıcı'yı**seçin.

    ![Kimlik Sağlayıcılar ve Create Sağlayıcı vurgulanmış iAM sayfasının ekran görüntüsü][13]

5. **Sağlayıcıyı Yapılandır** ı sayfasında aşağıdaki adımları gerçekleştirin:

    ![Yapılandırma Sağlayıcısıekran Görüntüsü][14]

    a. **Sağlayıcı Türü**için **SAML'yi**seçin.

    b. **Sağlayıcı Adı**için, bir sağlayıcı adı yazın (örneğin: *WAAD).*

    c. İndirdiğiniz meta **veri dosyanızı** Azure portalından yüklemek için **Dosya Seç'i**seçin.

    d. **Sonraki Adımı**seçin.

6. Sağlayıcı **Bilgilerini Doğrula** sayfasında **Oluştur'u**seçin.

    ![Create vurgulanmış sağlayıcı bilgilerini doğrula ekran görüntüsü][15]

7. **Roller** > **Oluştur'u**seçin.

    ![Roller sayfasının ekran görüntüsü][16]

8. Rol **Oluştur** sayfasında aşağıdaki adımları gerçekleştirin:  

    ![Rol Oluştur sayfasının ekran görüntüsü][19]

    a. **Güvenilir varlığın Seç türü**altında, **SAML 2.0 federasyonu'ni**seçin.

    b. **Saml 2.0 Sağlayıcısı'nın**altında, daha önce oluşturduğunuz **SAML sağlayıcısını** seçin (örneğin: *WAAD).*

    c. **Programatik ve AWS Yönetim Konsoluna Erişime İzin Ver'i**seçin.
  
    d. **Sonraki'ni Seçin: İzinler.**

9. **İzinleri Ekle ilkeleri** iletişim kutusunda, kuruluşunuz başına uygun ilkeyi takın. Sonra **Sonraki seçin: Gözden geçirin.**  

    ![İzinleri Ekle ilkesi iletişim kutusunun ekran görüntüsü][33]

10. Gözden **Geçir** iletişim kutusunda aşağıdaki adımları gerçekleştirin:

    ![Gözden Geçir iletişim kutusunun ekran görüntüsü][34]

    a. **Rol adına,** rol adınızı girin.

    b. **Rol açıklamasında,** açıklamayı girin.

    c. **Rol Oluştur'u**seçin.

    d. Gerektiği kadar rol oluşturun ve bunları kimlik sağlayıcısıyla eşleyin.

11. Azure AD kullanıcı sağlamada rolleri AWS hesabından almak için AWS hizmet hesabı kimlik bilgilerini kullanın. Bunun için AWS konsolu eve açın.

12. **Hizmetleri**Seçin. **Güvenlik altında, Kimlik & Uyumluluk**, **IAM**seçin.

    ![Hizmetler ve IAM vurgulanmış AWS konsol uvere sinin ekran görüntüsü](./media/amazon-web-service-tutorial/fetchingrole1.png)

13. IAM bölümünde **İlkeler'i**seçin.

    ![İlkeler vurgulanmış IAM bölümünün ekran görüntüsü](./media/amazon-web-service-tutorial/fetchingrole2.png)

14. Azure AD kullanıcı sağlamada rolleri AWS hesabından almak için **oluştur ilkesini** seçerek yeni bir ilke oluşturun.

    ![Oluşturma ilkesi vurgulanmış olan rol sayfası oluştur ekran görüntüsü](./media/amazon-web-service-tutorial/fetchingrole3.png)

15. AWS hesaplarındaki tüm rolleri almak için kendi ilkenizi oluşturun.

    ![JSON vurgulandığı politika sayfası oluşturma ekran görüntüsü](./media/amazon-web-service-tutorial/policy1.png)

    a. **Oluştur ilkesinde** **JSON** sekmesini seçin.

    b. İlke belgesinde aşağıdaki JSON'u ekleyin:

    ```json
    {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Effect": "Allow",
                "Action": [
                "iam:ListRoles"
                ],
                "Resource": "*"
            }
        ]
    }
    ```

    c. İlkeyi doğrulamak için **Gözden Geçir ilkesini** seçin.

    ![Politika oluştur sayfasının ekran görüntüsü](./media/amazon-web-service-tutorial/policy5.png)

16. Yeni ilkeyi tanımlayın.

    ![Ad ve Açıklama alanları vurgulanmış ilke sayfası oluştur ekran görüntüsü](./media/amazon-web-service-tutorial/policy2.png)

    a. **Ad**için , **AzureAD_SSOUserRole_Policy**girin.

    b. **Açıklama**için, **Bu ilke AWS hesaplarından rolleri getirmek için izin verir**girin.

    c. **İlke oluştur**'u seçin.

17. AWS IAM hizmetinde yeni bir kullanıcı hesabı oluşturun.

    a. AWS IAM **konsolunda, Kullanıcılar'ı**seçin.

    ![AWS IAM konsolunun ekran görüntüsü, Kullanıcılar vurgulanır](./media/amazon-web-service-tutorial/policy3.png)

    b. Yeni bir kullanıcı oluşturmak için **kullanıcı ekle'yi**seçin.

    ![Kullanıcı ekle düğmesinin ekran görüntüsü](./media/amazon-web-service-tutorial/policy4.png)

    c. Kullanıcı **Ekle** bölümünde:

    ![Kullanıcı adı ve Erişim türü vurgulanmış kullanıcı sayfası ekle ekran görüntüsü](./media/amazon-web-service-tutorial/adduser1.png)

    * Kullanıcı adını **AzureADRoleManager**olarak girin.

    * Erişim türü için **Programlı erişimi**seçin. Bu şekilde, kullanıcı API'leri çağırabilir ve rolleri AWS hesabından alabilir.

    * **Sonraki İzinleri**seçin.

18. Bu kullanıcı için yeni bir ilke oluşturun.

    ![Kullanıcı Ekle ekran görüntüsü](./media/amazon-web-service-tutorial/adduser2.png)

    a. **Varolan ilkeleri doğrudan ekle'yi**seçin.

    b. Filtre bölümünde yeni oluşturulan **ilkeyi AzureAD_SSOUserRole_Policy.**

    c. İlkeyi seçin ve sonra **Sonraki: Gözden Geçir'i**seçin.

19. İlkeyi bağlı kullanıcıya gözden geçirin.

    ![Kullanıcı ekle sayfasının ekran görüntüsü, Kullanıcı yı oluştur vurgulanır](./media/amazon-web-service-tutorial/adduser3.png)

    a. Kullanıcıyla eşlenen kullanıcı adını, erişim türünü ve ilkeyi gözden geçirin.

    b. **Kullanıcı Oluştur'u**seçin.

20. Bir kullanıcının kullanıcı kimlik bilgilerini indirin.

    ![Kullanıcı Ekle ekran görüntüsü](./media/amazon-web-service-tutorial/adduser4.png)

    a. Kullanıcı **Erişim anahtar kimliğini** ve Gizli erişim **anahtarını**kopyalayın.

    b. Rolleri AWS konsolundan almak için bu kimlik bilgilerini Azure AD kullanıcı sağlama bölümüne girin.

    c. **Kapat**'ı seçin.

### <a name="how-to-configure-role-provisioning-in-amazon-web-services-aws"></a>Amazon Web Services (AWS)'de rol sağlama nasıl yapılandırılır?

1. Azure AD yönetim portalında, AWS **uygulamasında, Sağlama'ya**gidin.

    ![AWS uygulamasının ekran görüntüsü, Sağlama vurgulanır](./media/amazon-web-service-tutorial/provisioning.png)

2. Sırasıyla **istemci gizli** ve Gizli **Belirteç** alanlarına erişim anahtarını ve sırrıgirin.

    ![Yönetici Kimlik Bilgileri iletişim kutusunun ekran görüntüsü](./media/amazon-web-service-tutorial/provisioning1.png)

    a. **Istemci gizli** alanında AWS kullanıcı erişim anahtarını girin.

    b. **Gizli Belirteç** alanına AWS kullanıcı sırrını girin.

    c. **Test Bağlantısı'nı**seçin.

    d. Kaydet'i seçerek ayarı **kaydet.**

3. **Ayarlar** bölümünde, **Provisioning Status**için, **On'u**seçin. Ardından **Kaydet'i**seçin.

    ![Açık vurgulanmış ayarlar bölümünün ekran görüntüsü](./media/amazon-web-service-tutorial/provisioning2.png)

> [!NOTE]
> Sağlama hizmeti rolleri yalnızca AWS'den Azure AD'ye aktarım. Hizmet, Azure AD'den AWS'ye kullanıcılar ve gruplar sağlamaz.

> [!NOTE]
> Sağlama kimlik bilgilerini kurtardıktan sonra, ilk eşitleme döngüsünün çalışmasını beklemeniz gerekir. Eşitlemenin tamamlanması genellikle yaklaşık 40 dakika sürer. Geçerlilik sayfasının alt kısmında, **Provisioning** **Geçerli Durum**altında durumu görebilirsiniz.

### <a name="create-amazon-web-services-aws-test-user"></a>Amazon Web Services (AWS) test kullanıcısı oluşturma

Bu bölümün amacı Amazon Web Services (AWS) B.Simon adlı bir kullanıcı oluşturmaktır. Amazon Web Services (AWS) SSO için kendi sisteminde oluşturulacak bir kullanıcı gerekmez, bu yüzden burada herhangi bir eylem gerçekleştirmek gerekmez.

## <a name="test-sso"></a>Test SSO

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki Amazon Web Hizmetleri (AWS) döşemesini tıklattığınızda, SSO'yu kurduğunuz Amazon Web Hizmetleri'nde (AWS) otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="known-issues"></a>Bilinen sorunlar

 * **Sağlama** bölümünde, **Haritalamalar** alt bölümünde "Yükleme..." iletin ve öznitelik eşlemelerini asla görüntülemez. Bugün desteklenen tek sağlama iş akışı, bir kullanıcı veya grup ataması sırasında seçim için AWS'den Azure AD'ye rollerin içe aktarılmasıdır. Bunun öznitelik eşlemeleri önceden belirlenmiştir ve yapılandırılamaz.

 * **Sağlama** bölümü, aynı anda yalnızca bir AWS kiracısı için bir kimlik bilgileri kümesi girmeyi destekler. Tüm içe aktarılan roller, AWS kiracısı için Azure AD `appRoles` [ `servicePrincipal` nesnesinin](https://docs.microsoft.com/graph/api/resources/serviceprincipal?view=graph-rest-beta) özelliğine yazılır.

   Birden çok AWS kiracı `servicePrincipals`(temsil) sağlama için galeriden Azure AD eklenebilir. Ancak, SSO için kullanılan tek `servicePrincipals` `servicePrincipal` ekibe sağlama için kullanılan birden çok AWS'den alınan tüm rolleri otomatik olarak yazamamak gibi bilinen bir sorun vardır.

   Geçici çözüm olarak, sağlamanın yapılandırıldığı her AWS'ye `appRoles` `servicePrincipal` alınan tüm aws'yi ayıklamak için Microsoft Graph [API'sini](https://docs.microsoft.com/graph/api/resources/serviceprincipal?view=graph-rest-beta) kullanabilirsiniz. Daha sonra bu rol dizelerini SSO'nun yapılandırıldığı AWS'ye `servicePrincipal` ekleyebilirsiniz.

* Rollerin AWS'den Azure AD'ye aktarılabilmek için aşağıdaki gereksinimleri karşılaması gerekir:

  * Rollerin AWS'de tam olarak bir saml sağlayıcısı tanımlanmış olması gerekir

  * Alınan bir rol için ROL ARN ve saml sağlayıcısı ARN'nin toplam uzunluğu 119 karakter veya daha az olmalıdır

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Amazon Web Hizmetlerini (AWS) deneyin](https://aad.portal.azure.com/)

- [Microsoft Cloud App Security'de oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [Amazon Web Services (AWS) gelişmiş görünürlük ve kontroller ile nasıl korunur?](https://docs.microsoft.com/cloud-app-security/protect-aws)

[11]: ./media/amazon-web-service-tutorial/ic795031.png
[12]: ./media/amazon-web-service-tutorial/ic795032.png
[13]: ./media/amazon-web-service-tutorial/ic795033.png
[14]: ./media/amazon-web-service-tutorial/ic795034.png
[15]: ./media/amazon-web-service-tutorial/ic795035.png
[16]: ./media/amazon-web-service-tutorial/ic795022.png
[17]: ./media/amazon-web-service-tutorial/ic795023.png
[18]: ./media/amazon-web-service-tutorial/ic795024.png
[19]: ./media/amazon-web-service-tutorial/ic795025.png
[32]: ./media/amazon-web-service-tutorial/ic7950251.png
[33]: ./media/amazon-web-service-tutorial/ic7950252.png
[35]: ./media/amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning.png
[34]: ./media/amazon-web-service-tutorial/ic7950253.png
[36]: ./media/amazon-web-service-tutorial/tutorial_amazonwebservices_securitycredentials.png
[37]: ./media/amazon-web-service-tutorial/tutorial_amazonwebservices_securitycredentials_continue.png
[38]: ./media/amazon-web-service-tutorial/tutorial_amazonwebservices_createnewaccesskey.png
[39]: ./media/amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_automatic.png
[40]: ./media/amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_testconnection.png
[41]: ./media/amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_on.png
