---
title: 'Öğretici: Amazon Web Services ile tümleştirme Azure Active Directory (AWS) | Microsoft Docs'
description: Azure Active Directory ve Amazon Web Services (AWS) arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: 7561c20b-2325-4d97-887f-693aa383c7be
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 07/30/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: d0ed4cd677392a6a48038eed80c36664ccef9494
ms.sourcegitcommit: aa042d4341054f437f3190da7c8a718729eb675e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68879875"
---
# <a name="tutorial-integrate-amazon-web-services-aws-with-azure-active-directory"></a>Öğretici: Amazon Web Services (AWS) Azure Active Directory ile tümleştirin

Bu öğreticide, Amazon Web Services (AWS) Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. AWS 'yi Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* AWS 'ye erişimi olan Azure AD 'de denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla AWS 'ye otomatik olarak oturum açmalarına olanak sağlayın.
* Hesaplarınızı tek bir merkezi konumda yönetin, Azure portal.

Azure AD ile SaaS uygulaması tümleştirmesi hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

![Azure AD ve AWS ilişkisi diyagramı](./media/amazon-web-service-tutorial/tutorial_amazonwebservices_image.png)

Birden çok örnek için birden çok tanımlayıcı yapılandırabilirsiniz. Örneğin:

* `https://signin.aws.amazon.com/saml#1`

* `https://signin.aws.amazon.com/saml#2`

Bu değerlerle Azure AD **#** , değerini kaldırır ve SAML belirtecindeki hedef kitle URL 'si olarak doğru değeri `https://signin.aws.amazon.com/saml` gönderir.

Bu yaklaşım aşağıdaki nedenlerle önerilir:

- Her uygulama size benzersiz bir x509 sertifikası sağlar. AWS uygulama örneğinin her bir örneği daha sonra ayrı bir AWS hesabı temelinde yönetilebilen farklı bir sertifika sona erme tarihine sahip olabilir. Bu durumda genel sertifika geçişi daha kolay.

- Azure AD 'de bir AWS uygulamasıyla Kullanıcı sağlamayı etkinleştirebilir ve ardından hizmetimiz bu AWS hesabındaki tüm rolleri getirir. Uygulama üzerinde AWS rollerini el ile eklemeniz veya güncelleştirmeniz gerekmez.

- Uygulama sahibini uygulama için ayrı ayrı atayabilirsiniz. Bu kişi, uygulamayı doğrudan Azure AD 'de yönetebilir.

> [!Note]
> Yalnızca bir galeri uygulaması kullandığınızdan emin olun.

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* AWS çoklu oturum açma (SSO) aboneliği etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz. AWS **, SP ve ıDP** tarafından başlatılan SSO 'yu destekler.

## <a name="add-aws-from-the-gallery"></a>Galeriden AWS ekleme

AWS 'nin tümleştirmesini Azure AD ile yapılandırmak için galerisinden AWS 'yi yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Bir iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak [Azure Portal](https://portal.azure.com) oturum açın.
1. Sol bölmede **Azure Active Directory** hizmeti seçin.
1. **Kurumsal uygulamalar**' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Amazon Web Services (AWS)** yazın.
1. Sonuçlar panelinden **Amazon Web Services (AWS)** öğesini seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

**B. Simon**adlı bir test kullanıcısı kullanarak AWS Ile Azure AD SSO 'yu yapılandırın ve test edin. SSO 'nun çalışması için, AWS 'de bir Azure AD kullanıcısı ve ilgili Kullanıcı arasında bağlı bir ilişki kurmanız gerekir.

AWS ile Azure AD SSO 'yu yapılandırmak ve test etmek için aşağıdaki yapı taşlarını doldurun:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **Azure AD SSO 'Yu yapılandırın** .
2. **AWS** 'yi, uygulama tarafında SSO ayarlarını yapılandırmak için yapılandırın.
3. Azure AD SSO 'yu B. Simon ile test etmek için **bir Azure AD test kullanıcısı oluşturun** .
4. Azure AD SSO 'yu kullanmak için B. Simon 'u etkinleştirmek üzere **Azure AD test kullanıcısını atayın** .
5. AWS **testi kullanıcısını** , kullanıcının Azure AD gösterimine bağlı olan AWS 'de B. Simon 'a sahip olacak şekilde oluşturun.
6. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **test SSO 'su** .

### <a name="configure-azure-ad-sso"></a>Azure AD SSO 'yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. [Azure Portal](https://portal.azure.com/), **Amazon Web Services (AWS)** uygulama tümleştirmesi sayfasında, **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** kalem simgesini seçin.

   ![Kalem simgesi vurgulanmış şekilde SAML sayfası ile çoklu oturum açmayı ayarlama ekran görüntüsü](common/edit-urls.png)

4. **Temel SAML yapılandırması** bölümünde, uygulama önceden yapılandırılmıştır ve gerekli URL 'ler Azure ile önceden doldurulmuştur. Kullanıcının **Kaydet**' i seçerek yapılandırmayı kaydetmesi gerekir.

5. Birden fazla örnek yapılandırırken bir tanımlayıcı değeri sağlayın. İkinci örnekten başlayarak, bir **#** işaret da dahil olmak üzere benzersiz bir SPN değeri belirtmek için aşağıdaki biçimi kullanın.

    `https://signin.aws.amazon.com/saml#2`

6. AWS uygulaması, SAML belirteci öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML onayları bekliyor. Aşağıdaki ekran görüntüsünde varsayılan özniteliklerin listesi gösterilmektedir. Kullanıcı öznitelikleri iletişim kutusunu açmak için kalem simgesini seçin.

    ![Kalem simgesi vurgulanmış şekilde Kullanıcı öznitelikleri iletişim kutusunun ekran görüntüsü](common/edit-attribute.png)

7. Önceki özniteliklere ek olarak AWS uygulaması, SAML yanıtına daha fazla öznitelik geçirilmesini bekler. **Kullanıcı öznitelikleri** Iletişim kutusundaki **Kullanıcı TALEPLERI** bölümünde, SAML belirteci özniteliğini eklemek için aşağıdaki adımları gerçekleştirin.

    | Ad  | Kaynak özniteliği  | Ad Alanı |
    | --------------- | --------------- | --------------- |
    | Roleoturumadı | User. UserPrincipalName | https://aws.amazon.com/SAML/Attributes |
    | Role            | Kullanıcı. atandroles |  https://aws.amazon.com/SAML/Attributes |
    | SessionDuration             | "900 saniye (15 dakika) ila 43200 saniye (12 saat) arasında bir değer belirtin" |  https://aws.amazon.com/SAML/Attributes |

    a. **Kullanıcı taleplerini Yönet** iletişim kutusunu açmak için **yeni talep Ekle** ' yi seçin.

    ![Kullanıcı talepleri bölümünün ekran görüntüsü, yeni talep Ekle ve vurgulanmış olarak kaydet](common/new-save-attribute.png)

    ![Kullanıcı taleplerini Yönet iletişim kutusunun ekran görüntüsü](common/new-attribute-details.png)

    b. **Ad**alanına, bu satır için gösterilen öznitelik adını yazın.

    c. **Ad alanı**' nda, bu satır için gösterilen ad alanı değerini yazın.

    d. **Kaynak**bölümünde **öznitelik**' i seçin.

    e. **Kaynak özniteliği** listesinde, bu satır için gösterilen öznitelik değerini yazın.

    f. **Tamam**’ı seçin.

    g. **Kaydet**’i seçin.

1. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde, **Federasyon meta verileri XML**'i bulun. Sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir** ' i seçin.

   ![Yükleme vurgulanmış olarak SAML Imzalama sertifikası bölümünün ekran görüntüsü](common/metadataxml.png)

1. **Amazon Web Services ayarla (AWS)** bölümünde, gereksiniminize göre uygun URL 'leri kopyalayın.

   ![Yapılandırma URL 'Leri vurgulanmış şekilde ayarlama Amazon Web Services (AWS) bölümünün ekran görüntüsü](common/copy-configuration-urls.png)

### <a name="configure-aws"></a>AWS 'yi yapılandırma

1. Farklı bir tarayıcı penceresinde, AWS şirket sitenizde yönetici olarak oturum açın.

2. **AWS giriş**seçeneğini belirleyin.

    ![AWS şirket sitesinin, AWS giriş simgesi vurgulanmış ekran görüntüsü][11]

3. **Kimlik ve erişim yönetimi '** ni seçin.

    ![AWS Hizmetleri sayfasının ekran görüntüsü, ıAM vurgulanmış][12]

4. **Kimlik sağlayıcıları** > **sağlayıcı oluştur**' u seçin.

    ![Kimlik sağlayıcıları ve sağlayıcı oluşturma vurgulanmış olarak ıAM sayfasının ekran görüntüsü][13]

5. **Sağlayıcıyı yapılandır** sayfasında, aşağıdaki adımları uygulayın:

    ![Yapılandırma sağlayıcısının ekran görüntüsü][14]

    a. **Sağlayıcı türü**için **SAML**' yi seçin.

    b. **Sağlayıcı adı**için bir sağlayıcı adı yazın (örneğin: *WAAD*).

    c. İndirilen **meta veri dosyanızı** Azure Portal yüklemek Için **Dosya Seç**' i seçin.

    d. **Sonraki adımı**seçin.

6. **Sağlayıcı bilgilerini doğrula** sayfasında **Oluştur**' u seçin.

    ![Create vurgulanan ile sağlayıcı bilgilerini doğrulama ekran görüntüsü][15]

7. Rol oluşturma**rolü**' nü seçin. > 

    ![Roller sayfasının ekran görüntüsü][16]

8. **Rol oluştur** sayfasında, aşağıdaki adımları uygulayın:  

    ![Rol Oluştur sayfasının ekran görüntüsü][19]

    a. **Güvenilen varlık türünü seçin**altında **SAML 2,0 Federasyonu**' ni seçin.

    b. **SAML 2,0 sağlayıcısı seçin**altında, daha önce oluşturduğunuz **SAML sağlayıcısını** seçin (örneğin: *WAAD*).

    c. **Programlı ve AWS Yönetim Konsolu erişimine Izin ver '** i seçin.
  
    d. İleri **' yi seçin: İzinler**.

9. **İzin Ilkeleri Ekle** iletişim kutusunda, uygun ilkeyi kuruluşunuza göre ekleyin. Ardından İleri **' yi seçin: Gözden**geçirin.  

    ![İzin ilkesi Ekle iletişim kutusunun ekran görüntüsü][33]

10. **Gözden geçirme** iletişim kutusunda, aşağıdaki adımları uygulayın:

    ![Inceleme iletişim kutusunun ekran görüntüsü][34]

    a. **Rol adı**alanına rol adınızı girin.

    b. **Rol açıklaması**' nda, açıklamayı girin.

    c. **Rol oluştur**' u seçin.

    d. Gereken sayıda rol oluşturun ve bunları kimlik sağlayıcısına eşleyin.

11. Azure AD Kullanıcı sağlaması içindeki AWS hesabından rolleri getirmek için AWS hizmet hesabı kimlik bilgilerini kullanın. Bu için AWS konsolu ana öğesini açın.

12. **Hizmetler**' i seçin. **Güvenlik, kimlik & uyumluluğu**altında, **IAM**' i seçin.

    ![Hizmet ve ıAM ile AWS konsol giriş ekranının ekran görüntüsü](./media/amazon-web-service-tutorial/fetchingrole1.png)

13. IAM bölümünde **ilkeler**' i seçin.

    ![Ilke vurgulanmış şekilde ıAM bölümünün ekran görüntüsü](./media/amazon-web-service-tutorial/fetchingrole2.png)

14. Azure AD Kullanıcı sağlaması içindeki AWS hesabından rolleri getirmeye yönelik **Ilke oluştur** ' a tıklayarak yeni bir ilke oluşturun.

    ![Rol oluştur seçeneğinin, ilke oluştur vurgulanarak ekran görüntüsü](./media/amazon-web-service-tutorial/fetchingrole3.png)

15. AWS hesaplarından tüm rolleri getirmek için kendi ilkenizi oluşturun.

    ![JSON vurgulanmış şekilde ilke oluştur sayfasının ekran görüntüsü](./media/amazon-web-service-tutorial/policy1.png)

    a. **Ilke oluştur**bölümünde **JSON** sekmesini seçin.

    b. İlke belgesinde aşağıdaki JSON 'u ekleyin:

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

    c. İlkeyi doğrulamak için **Ilkeyi gözden geçir** ' i seçin.

    ![İlke Oluştur sayfasının ekran görüntüsü](./media/amazon-web-service-tutorial/policy5.png)

16. Yeni ilkeyi tanımlayın.

    ![Ad ve açıklama alanları vurgulanmış şekilde ilke oluştur sayfasının ekran görüntüsü](./media/amazon-web-service-tutorial/policy2.png)

    a. **Ad**için **AzureAD_SSOUserRole_Policy**girin.

    b. **Açıklama**için **Bu ilkenin, AWS hesaplarından rolleri getirmeye izin verilecek şekilde**girin.

    c. **Ilke oluştur**' u seçin.

17. AWS ıAM hizmetinde yeni bir kullanıcı hesabı oluşturun.

    a. AWS ıAM konsolunda **Kullanıcılar**' ı seçin.

    ![AWS ıAM konsolunun, kullanıcıların vurgulandığı ekran görüntüsü](./media/amazon-web-service-tutorial/policy3.png)

    b. Yeni bir kullanıcı oluşturmak için **Kullanıcı Ekle**' yi seçin.

    ![Kullanıcı Ekle düğmesinin ekran görüntüsü](./media/amazon-web-service-tutorial/policy4.png)

    c. **Kullanıcı Ekle** bölümünde:

    ![Kullanıcı adı ve erişim türü vurgulanmış şekilde Kullanıcı ekleme sayfasının ekran görüntüsü](./media/amazon-web-service-tutorial/adduser1.png)

    * Kullanıcı adını **Azureadrolemanager**olarak girin.

    * Erişim türü için **programlı erişim**' i seçin. Bu şekilde, Kullanıcı API 'Leri çağırabilir ve AWS hesabından rolleri alabilir.

    * **Sonraki izinleri**seçin.

18. Bu Kullanıcı için yeni bir ilke oluşturun.

    ![Kullanıcı ekleme ekranının ekran görüntüsü](./media/amazon-web-service-tutorial/adduser2.png)

    a. **Mevcut ilkeleri doğrudan Ekle**' yi seçin.

    b. Yeni oluşturulan ilkeyi arama filtresi bölümünde **AzureAD_SSOUserRole_Policy**.

    c. İlkeyi seçin ve ardından İleri ' yi **seçin: Gözden**geçirin.

19. Ekli Kullanıcı için ilkeyi gözden geçirin.

    ![Kullanıcı Ekle seçeneğinin vurgulandığı Kullanıcı ekleme sayfasının ekran görüntüsü](./media/amazon-web-service-tutorial/adduser3.png)

    a. Kullanıcı adını, erişim türünü ve kullanıcıyla eşlenen ilkeyi gözden geçirin.

    b. **Kullanıcı oluştur**' u seçin.

20. Kullanıcının Kullanıcı kimlik bilgilerini indirin.

    ![Kullanıcı ekleme ekranının ekran görüntüsü](./media/amazon-web-service-tutorial/adduser4.png)

    a. Kullanıcı **erişim anahtarı kimliğini** ve gizli dizi **erişim anahtarını**kopyalayın.

    b. AWS konsolundan rolleri getirmek için bu kimlik bilgilerini Azure AD Kullanıcı sağlama bölümüne girin.

    c. **Kapat**' ı seçin.

21. Azure AD yönetim portalında AWS uygulamasında **sağlama**' ya gidin.

    ![Sağlama vurgulanmış şekilde AWS uygulamasının ekran görüntüsü](./media/amazon-web-service-tutorial/provisioning.png)

22. Erişim anahtarını ve gizli anahtarı, sırasıyla **ClientSecret** ve **gizli belirteç** alanlarına girin.

    ![Yönetici kimlik bilgileri iletişim kutusunun ekran görüntüsü](./media/amazon-web-service-tutorial/provisioning1.png)

    a. **ClientSecret** alanına AWS Kullanıcı erişimi anahtarını girin.

    b. **Gizli belirteç** alanına AWS Kullanıcı gizli anahtarını girin.

    c. **Bağlantıyı Sına**' yı seçin.

    d. **Kaydet**' i seçerek ayarı kaydedin.

23. **Ayarlar** bölümünde, **sağlama durumu**için **Açık**' ı seçin. Daha sonra **Kaydet**’e tıklayın.

    ![Vurgulanan Ayarlar bölümünün ekran görüntüsü](./media/amazon-web-service-tutorial/provisioning2.png)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümde, Azure portal bir test kullanıcısı oluşturun, B. Simon.

1. Azure Portal sol bölmeden,**Kullanıcılar** >  **Azure Active Directory** > **tüm kullanıcılar**' ı seçin.
1. Seçin **yeni kullanıcı** ekranın üstünde.
1. **Kullanıcı** özellikleri ' nde şu adımları izleyin:
   
   a. **Ad** alanına `B.Simon` girin.  
   b. **Kullanıcı adı** alanına, username@companydomain.extensiongirin. Örneğin: `B.Simon@contoso.com`.   
   c. **Parolayı göster**' i seçin ve yazın. Ardından **Oluştur**’u seçin.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, AWS 'ye erişim vererek B. Simon 'u Azure SSO 'yu kullanmaya etkinleştirirsiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde **Amazon Web Services (AWS)** öğesini seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.

   ![Kullanıcılar ve gruplar vurgulanmış şekilde Yönet bölümünün ekran görüntüsü](common/users-groups-blade.png)

1. **Kullanıcı Ekle**' yi seçin. **Atama Ekle** Iletişim kutusunda **Kullanıcılar ve gruplar**' ı seçin.

    ![Kullanıcı ekleme ekranının ekran görüntüsü](common/add-assign-user.png)

1. **Kullanıcılar ve gruplar** Iletişim kutusunda **B. Simon**öğesini seçin. Sonra **Seç**' i seçin.
1. SAML onaylama işlemi içinde herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, Kullanıcı için listeden uygun rolü seçin. Sonra **Seç**' i seçin.
1. **Atama Ekle** Iletişim kutusunda **ata**' yı seçin.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Erişim panelinde AWS kutucuğunu seçtiğinizde, SSO 'yu ayarladığınız AWS 'de otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="known-issues"></a>Bilinen sorunlar

 * **Sağlama** bölümünde, **eşlemeler** alt bölümünde "yükleniyor..." görüntülenir ileti eşleştirmez ve hiçbir şekilde öznitelik eşlemelerini görüntülemez. Bugün desteklenen tek sağlama iş akışı, bir kullanıcı veya grup ataması sırasında AWS 'den Azure AD 'ye roller için içeri aktarmaktır. Bunun için öznitelik eşlemeleri önceden belirlenir ve yapılandırılamaz.
 
 * **Sağlama** bölümü yalnızca bir AWS kiracısı için tek seferde bir kimlik bilgileri kümesi girmeyi destekler. İçeri aktarılan tüm roller AWS kiracısı `appRoles` için Azure AD [ `servicePrincipal` nesnesinin](https://docs.microsoft.com/graph/api/resources/serviceprincipal?view=graph-rest-beta) özelliğine yazılır. 
 
   Azure AD 'ye, sağlama için Galeriden `servicePrincipals`birden çok AWS kiracısı (tarafından temsil edilir) eklenebilir. Bununla birlikte, tüm içeri aktarılan rollerin, SSO için kullanılmak üzere tek başına `servicePrincipals` `servicePrincipal` sağlanması için kullanılan birden fazla AWS 'den otomatik olarak yazamayacak bilinen bir sorun vardır. 
   
   Geçici bir çözüm olarak, sağlama yapılandırılmış her bir `appRoles` AWS `servicePrincipal` ' de içeri aktarılan her bir ı ayıklamak için [Microsoft Graph API](https://docs.microsoft.com/graph/api/resources/serviceprincipal?view=graph-rest-beta) 'sini kullanabilirsiniz. Daha sonra, bu rol dizelerini SSO 'nun yapılandırıldığı AWS `servicePrincipal` 'ye ekleyebilirsiniz.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory Koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

<!--Image references-->

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
