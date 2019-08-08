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
ms.openlocfilehash: f6640708905abc266b07b7b66f5da09aeb890f01
ms.sourcegitcommit: 3073581d81253558f89ef560ffdf71db7e0b592b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68823886"
---
# <a name="tutorial-integrate-amazon-web-services-aws-with-azure-active-directory"></a>Öğretici: Amazon Web Services (AWS) Azure Active Directory ile tümleştirin

Bu öğreticide, Amazon Web Services (AWS) Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. Amazon Web Services (AWS) Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de Amazon Web Services erişimi olan denetim (AWS).
* Kullanıcılarınızın Azure AD hesaplarıyla Amazon Web Services (AWS) için otomatik olarak oturum açmalarına olanak sağlayın.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

Azure AD ile SaaS uygulaması tümleştirmesi hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

![Amazon Web Services (AWS)](./media/amazon-web-service-tutorial/tutorial_amazonwebservices_image.png)

Aşağıdaki gibi birden çok örnek için birden fazla tanımlayıcı yapılandırabilirsiniz.

* `https://signin.aws.amazon.com/saml#1`

* `https://signin.aws.amazon.com/saml#2`

Bu değerler sayesinde, Azure AD değerini **#** kaldırır ve SAML belirtecindeki hedef kitle URL 'si olarak doğru değeri `https://signin.aws.amazon.com/saml` gönderir.

**Aşağıdaki nedenlerden dolayı bu yaklaşımı kullanmanızı öneririz:**

a. Her uygulama size benzersiz bir x509 sertifikası sağlayacaktır. AWS uygulama örneğinin her örneği, tek bir AWS hesabı temelinde yönetilebilen farklı bir sertifika sona erme tarihine sahip olabilir. Bu durumda genel sertifika geçişi daha kolay olacaktır.

b. Azure AD 'de AWS uygulamasıyla Kullanıcı sağlamayı etkinleştirebilir ve ardından hizmetimiz bu AWS hesabındaki tüm rolleri getirir. Uygulama üzerinde AWS rollerini el ile eklemeniz veya güncelleştirmeniz gerekmez.

c. Uygulamayı doğrudan Azure AD 'de yönetebilen uygulama için uygulama sahibini ayrı ayrı atayabilirsiniz.

> [!Note]
> Yalnızca Galeri uygulamasını kullandığınızdan emin olun

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Amazon Web Services (AWS) çoklu oturum açma (SSO) etkin aboneliği.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz. Amazon Web Services (AWS) **, SP ve ıDP** tarafından başlatılan SSO 'yu destekler.

## <a name="adding-amazon-web-services-aws-from-the-gallery"></a>Galeriden Amazon Web Services (AWS) ekleme

Amazon Web Services (AWS) tümleştirmesini Azure AD ile yapılandırmak için, Galeriden yönetilen SaaS uygulamaları listenize Amazon Web Services (AWS) eklemeniz gerekir.

1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalda](https://portal.azure.com) oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Amazon Web Services (AWS)** yazın.
1. Sonuçlar panelinden **Amazon Web Services (AWS)** öğesini seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

**B. Simon**adlı bir test kullanıcısı kullanarak Azure AD SSO 'yu Amazon Web Services (AWS) ile yapılandırın ve test edin. SSO 'nun çalışması için, Amazon Web Services (AWS) ' de bir Azure AD kullanıcısı ve ilgili Kullanıcı arasında bağlantı ilişkisi oluşturmanız gerekir.

Azure AD SSO 'yu Amazon Web Services (AWS) ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını doldurun:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** .
2. Uygulama tarafında SSO ayarlarını yapılandırmak için **[Amazon Web Services (AWS) öğesini yapılandırın](#configure-amazon-web-services-aws)** .
3. B. Simon ile Azure AD çoklu oturum açma sınamasını test etmek için **[bir Azure AD test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** .
4. Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek üzere **[Azure AD test kullanıcısını atayın](#assign-the-azure-ad-test-user)** .
5. Kullanıcının Azure AD gösterimine bağlı olan Amazon Web Services (AWS) için B. Simon 'a sahip olmak üzere **[Amazon Web Services (AWS) test kullanıcısı oluşturun](#create-amazon-web-services-aws-test-user)** .
6. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[test SSO 'su](#test-sso)** .

### <a name="configure-azure-ad-sso"></a>Azure AD SSO 'yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. [Azure Portal](https://portal.azure.com/), **Amazon Web Services (AWS)** uygulama tümleştirmesi sayfasında, **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** için Düzenle/kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. **Temel SAML yapılandırması** bölümünde, uygulama önceden yapılandırılmıştır ve gerekli URL 'ler Azure ile önceden doldurulmuştur. Kullanıcının **Kaydet** düğmesine tıklayarak yapılandırmayı kaydetmesi gerekir.

5. Birden fazla örnek yapılandırırken lütfen tanımlayıcı değeri belirtin. İkinci örnekten başlayarak, lütfen aşağıdaki biçimde tanımlayıcı değeri sağlayın. Lütfen benzersiz bir **#** SPN değeri belirtmek için bir işaret kullanın.

    `https://signin.aws.amazon.com/saml#2`

6. Amazon Web Services (AWS) uygulaması, SAML belirteci öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML onayları bekler. Aşağıdaki ekran görüntüsünde varsayılan özniteliklerin listesi gösterilmektedir. Kullanıcı öznitelikleri iletişim kutusunu açmak için **Düzenle** simgesine tıklayın.

    ![image](common/edit-attribute.png)

7. Amazon Web Services (AWS) uygulaması, yukarıdakine ek olarak, SAML yanıtına daha fazla özniteliğin geri geçirilmesini bekler. **Kullanıcı öznitelikleri** Iletişim kutusundaki **Kullanıcı talepleri** bölümünde AŞAĞıDAKI tabloda gösterildiği gibi SAML belirteci özniteliği eklemek için aşağıdaki adımları gerçekleştirin:

    | Ad  | Kaynak özniteliği  | Ad Alanı |
    | --------------- | --------------- | --------------- |
    | Roleoturumadı | User. UserPrincipalName | https://aws.amazon.com/SAML/Attributes |
    | Role            | Kullanıcı. atandroles |  https://aws.amazon.com/SAML/Attributes |
    | SessionDuration             | "900 saniye (15 dakika) ila 43200 saniye (12 saat) arasında bir değer belirtin" |  https://aws.amazon.com/SAML/Attributes |

    a. **Kullanıcı taleplerini Yönet** iletişim kutusunu açmak için **yeni talep Ekle** ' ye tıklayın.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. **Ad** metin kutusuna, bu satır için gösterilen öznitelik adını yazın.

    c. **Ad alanı** metin kutusuna, bu satır Için gösterilen ad alanı değerini yazın.

    d. **Öznitelik**olarak kaynak seçin.

    e. **Kaynak özniteliği** listesinde, bu satır için gösterilen öznitelik değerini yazın.

    f. Tıklayın **Tamam**

    g. **Kaydet**’e tıklayın.

1. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde, **Federasyon meta verileri XML** 'i bulun ve sertifikayı indirip bilgisayarınıza kaydetmek için **İndir** ' i seçin.

   ![Sertifika indirme bağlantısı](common/metadataxml.png)

1. **Amazon Web Services ayarla (AWS)** bölümünde, gereksiniminize göre uygun URL 'leri kopyalayın.

   ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

### <a name="configure-amazon-web-services-aws"></a>Amazon Web Services (AWS) yapılandırma

1. Farklı bir tarayıcı penceresinde, Amazon Web Services (AWS) Şirket sitenizde yönetici olarak oturum açın.

2. **AWS giriş**' e tıklayın.

    ![Çoklu oturum açmayı yapılandırma][11]

3. **Kimlik ve erişim yönetimi**' ne tıklayın.

    ![Çoklu oturum açma kimliğini yapılandırma][12]

4. **Kimlik sağlayıcıları**' na ve ardından **sağlayıcı oluştur**' a tıklayın.

    ![Çoklu oturum açma sağlayıcısını yapılandırma][13]

5. **Sağlayıcıyı yapılandır** iletişim sayfasında, aşağıdaki adımları uygulayın:

    ![Çoklu oturum açma iletişim kutusunu yapılandırma][14]

    a. **Sağlayıcı türü**olarak **SAML**' yi seçin.

    b. **Sağlayıcı adı** metin kutusuna bir sağlayıcı adı yazın (örneğin: *WAAD*).

    c. İndirilen **meta veri dosyanızı** Azure Portal karşıya yüklemek Için **Dosya Seç**' e tıklayın.

    d. **Ileri adım**' a tıklayın.

6. **Sağlayıcı bilgilerini doğrula** Iletişim sayfasında **Oluştur**' a tıklayın.

    ![Çoklu oturum açmayı yapılandırma][15]

7. **Roller**' e tıklayın ve ardından **rol oluştur**' a tıklayın.

    ![Çoklu oturum açma rollerini yapılandırma][16]

8. **Rol oluştur** sayfasında, aşağıdaki adımları uygulayın:  

    ![Çoklu oturum açma güvenini yapılandırma][19]

    a. **Güvenilen varlık seçin**' ın altında **SAML 2,0 Federasyonu** ' ni seçin.

    b. **SAML 2,0 sağlayıcısı Seç bölümünde**, daha önce oluşturduğunuz **SAML sağlayıcısını** seçin (örneğin: *WAAD*)

    c. **Programlı ve AWS Yönetim Konsolu erişimine Izin ver '** i seçin.
  
    d. İleri **' ye tıklayın: İzinler**.

9. **Izin Ilkeleri Ekle** iletişim kutusunda, lütfen kuruluşunuza göre uygun ilkeyi ekleyin. İleri **' ye tıklayın: Gözden**geçirin.  

    ![Çoklu oturum açma Ilkesini yapılandırma][33]

10. **Gözden geçirme** iletişim kutusunda, aşağıdaki adımları uygulayın:

    ![Çoklu oturum açma Incelemesini yapılandırma][34]

    a. **Rol adı** metin kutusunda, rol adınızı girin.

    b. **Rol açıklaması** metin kutusuna açıklamayı girin.

    c. **Rol oluştur**' a tıklayın.

    d. Gereken sayıda rol oluşturun ve bunları kimlik sağlayıcısına eşleyin.

11. Azure AD Kullanıcı sağlaması içindeki AWS hesabından rolleri getirmek için AWS hizmet hesabı kimlik bilgilerini kullanın. Bu için AWS konsolu ana öğesini açın.

12.  ->  **Hizmetler** -> **güvenlik, kimlik & uyumluluk** **IAM**' e tıklayın.

    ![AWS hesabından roller getiriliyor](./media/amazon-web-service-tutorial/fetchingrole1.png)

13. IAM bölümünde **ilkeler** sekmesini seçin.

    ![AWS hesabından roller getiriliyor](./media/amazon-web-service-tutorial/fetchingrole2.png)

14. Azure AD Kullanıcı sağlaması içindeki AWS hesabından rolleri getirmek için **Ilke oluştur** ' a tıklayarak yeni bir ilke oluşturun.

    ![Yeni ilke oluşturuluyor](./media/amazon-web-service-tutorial/fetchingrole3.png)

15. Aşağıdaki adımları gerçekleştirerek AWS hesaplarından tüm rolleri getirmek için kendi ilkenizi oluşturun:

    ![Yeni ilke oluşturuluyor](./media/amazon-web-service-tutorial/policy1.png)

    a. **"Ilke oluştur"** bölümünde **"JSON"** sekmesine tıklayın.

    b. İlke belgesinde aşağıdaki JSON 'u ekleyin.

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

    c. İlkeyi doğrulamak için **Ilkeyi gözden geçir düğmesine** tıklayın.

    ![Yeni ilkeyi tanımlama](./media/amazon-web-service-tutorial/policy5.png)

16. Aşağıdaki adımları gerçekleştirerek **Yeni ilkeyi** tanımlayın:

    ![Yeni ilkeyi tanımlama](./media/amazon-web-service-tutorial/policy2.png)

    a. **Ilke adını** **AzureAD_SSOUserRole_Policy**olarak belirtin.

    b. **Bu ilke AWS hesaplarından rolleri getirmeye izin verolacağı**Için ilkeye **Açıklama** sağlayabilirsiniz.

    c. **"Ilke oluştur"** düğmesine tıklayın.

17. Aşağıdaki adımları gerçekleştirerek AWS ıAM hizmetinde yeni bir kullanıcı hesabı oluşturun:

    a. AWS ıAM konsolundaki **Kullanıcılar** gezintisi ' ne tıklayın.

    ![Yeni ilkeyi tanımlama](./media/amazon-web-service-tutorial/policy3.png)

    b. Yeni bir kullanıcı oluşturmak için **Kullanıcı Ekle** düğmesine tıklayın.

    ![Kullanıcı ekle](./media/amazon-web-service-tutorial/policy4.png)

    c. **Kullanıcı Ekle** bölümünde aşağıdaki adımları uygulayın:

    ![Kullanıcı ekle](./media/amazon-web-service-tutorial/adduser1.png)

    * Kullanıcı adını **Azureadrolemanager**olarak girin.

    * Erişim türü ' nde, **programlı erişim** seçeneğini belirleyin. Bu şekilde Kullanıcı API 'Leri çağırabilir ve AWS hesabından rolleri alabilir.

    * Sağ alt köşedeki **sonraki izinler** düğmesine tıklayın.

18. Şimdi aşağıdaki adımları gerçekleştirerek bu kullanıcı için yeni bir ilke oluşturun:

    ![Kullanıcı ekle](./media/amazon-web-service-tutorial/adduser2.png)

    a. **Mevcut ilkeleri doğrudan Ekle** düğmesine tıklayın.

    b. Yeni oluşturulan ilkeyi arama filtresi bölümünde **AzureAD_SSOUserRole_Policy**.

    c. **İlkeyi** seçin ve ardından **ileri ' ye tıklayın: Gözden** geçir düğmesi.

19. Aşağıdaki adımları gerçekleştirerek, eklenen kullanıcıya ilkeyi gözden geçirin:

    ![Kullanıcı ekle](./media/amazon-web-service-tutorial/adduser3.png)

    a. Kullanıcı adını, erişim türünü ve kullanıcıyla eşlenen ilkeyi gözden geçirin.

    b. Kullanıcıyı oluşturmak için sağ alt köşedeki **Kullanıcı oluştur** düğmesine tıklayın.

20. Aşağıdaki adımları gerçekleştirerek bir kullanıcının Kullanıcı kimlik bilgilerini indirin:

    ![Kullanıcı ekle](./media/amazon-web-service-tutorial/adduser4.png)

    a. Kullanıcı **erişim anahtarı kimliğini** ve gizli dizi **erişim anahtarını**kopyalayın.

    b. AWS konsolundan rolleri getirmek için bu kimlik bilgilerini Azure AD Kullanıcı sağlama bölümüne girin.

    c. Alttaki **Kapat** düğmesine tıklayın.

21. Azure AD Yönetim Portalı 'de Amazon Web Services uygulamasının **Kullanıcı sağlama** bölümüne gidin.

    ![Kullanıcı ekle](./media/amazon-web-service-tutorial/provisioning.png)

22. **Erişim anahtarını** ve **parolayı** sırasıyla **Istemci gizli** anahtarı ve **gizli belirteç** alanına girin.

    ![Kullanıcı ekle](./media/amazon-web-service-tutorial/provisioning1.png)

    a. **ClientSecret** alanına AWS Kullanıcı erişimi anahtarını girin.

    b. **Gizli belirteç** alanına AWS Kullanıcı gizli anahtarını girin.

    c. **Bağlantıyı Sına** düğmesine tıklayın ve bu bağlantıyı başarıyla sınayabileceksiniz.

    d. Üstteki **Kaydet** düğmesine tıklayarak ayarı kaydedin.

23. Şimdi, anahtarı açık yapıp üstteki **Kaydet** düğmesine tıklayarak ayarlar bölümünde sağlama durumunu etkinleştirdiğinizden emin olun.

    ![Kullanıcı ekle](./media/amazon-web-service-tutorial/provisioning2.png)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümde, B. Simon adlı Azure portal bir test kullanıcısı oluşturacaksınız.

1. Azure portal sol bölmeden **Azure Active Directory**' i seçin, **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.
1. Seçin **yeni kullanıcı** ekranın üstünde.
1. **Kullanıcı** özellikleri ' nde şu adımları izleyin:
   1. **Ad** alanına `B.Simon` girin.  
   1. **Kullanıcı adı** alanına, username@companydomain.extensiongirin. Örneğin: `B.Simon@contoso.com`.
   1. **Parolayı göster** onay kutusunu seçin ve ardından **parola** kutusunda görüntülenen değeri yazın.
   1.           **Oluştur**'a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Amazon Web Services (AWS) erişimi vererek Azure çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde **Amazon Web Services (AWS)** öğesini seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.

   !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle**' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. SAML assertion 'da herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, Kullanıcı için listeden uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

### <a name="create-amazon-web-services-aws-test-user"></a>Amazon Web Services (AWS) test kullanıcısı oluştur

Bu bölümün amacı, Amazon Web Services (AWS) içinde B. Simon adlı bir Kullanıcı oluşturmaktır. Amazon Web Services (AWS), bir kullanıcının SSO için sisteminde oluşturulmasına gerek kalmaz, bu nedenle burada herhangi bir işlem gerçekleştirmeniz gerekmez.

### <a name="test-sso"></a>Test SSO 'SU

Erişim panelinde Amazon Web Services (AWS) kutucuğunu seçtiğinizde, SSO 'yu ayarladığınız Amazon Web Services (AWS) otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="known-issues"></a>Bilinen sorunlar

 * **Sağlama** bölümünde, **eşlemeler** alt bölümünde "yükleniyor..." gösterilir ileti eşlemelerini hiçbir daha gösterme. Bugün desteklenen tek sağlama iş akışı, Kullanıcı/Grup ataması sırasında yalnızca AWS 'den Azure AD 'ye roller içeri aktarılmaktır. Bunun için öznitelik eşlemeleri önceden belirlenir ve yapılandırılamaz.
 
 * **Sağlama** bölümü yalnızca bir AWS kiracısı için tek seferde bir kimlik bilgileri kümesi girmeyi destekler. İçeri aktarılan tüm roller AWS kiracısı için Azure AD [ServicePrincipal nesnesinin](https://docs.microsoft.com/graph/api/resources/serviceprincipal?view=graph-rest-beta) approles özelliğine yazılır. Birden çok AWS kiracısı (Servicesorumlularını temsil eden) Azure AD 'ye, sağlama galerisindeki eklenebilir, ancak şu şekilde kullanılan birden fazla AWS Servicesorumlularını içeri aktarılan rollerin tümünü otomatik olarak yazamayacak bilinen bir sorun var Çoklu oturum açma için kullanılan tek servicePrincipal 'ın sağlanması. Geçici bir çözüm olarak, [Microsoft Graph API 'si](https://docs.microsoft.com/graph/api/resources/serviceprincipal?view=graph-rest-beta) , sağlama yapılandırıldığı her BIR AWS ServicePrincipal öğesine içeri aktarılan tüm appro'leri ayıklamak için kullanılabilir. Bu rol dizeleri daha sonra çoklu oturum açma 'nın yapılandırıldığı AWS servicePrincipal 'a eklenebilir.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

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
