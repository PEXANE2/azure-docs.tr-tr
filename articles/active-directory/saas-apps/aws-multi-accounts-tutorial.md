---
title: 'Öğretici: birden çok hesaba bağlanmak için Amazon Web Services (AWS) ile Azure Active Directory tümleştirme | Microsoft Docs'
description: Azure AD ile Amazon Web Services (AWS) (eski öğretici) arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: article
ms.date: 08/07/2020
ms.author: jeedes
ms.openlocfilehash: 20674f5a793267c3a9e2fa078f95cbf96624df13
ms.sourcegitcommit: 023d10b4127f50f301995d44f2b4499cbcffb8fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88550186"
---
# <a name="tutorial-azure-active-directory-integration-with-amazon-web-services-aws-legacy-tutorial"></a>Öğretici: Amazon Web Services (AWS) ile Azure Active Directory tümleştirme (eski öğretici)

Bu öğreticide, Azure Active Directory (Azure AD) Amazon Web Services (AWS) (eski öğretici) ile tümleştirmeyi öğreneceksiniz.

Amazon Web Services (AWS) Azure AD ile tümleştirmek aşağıdaki avantajları sağlar:

- Amazon Web Services (AWS) erişimi olan Azure AD 'de denetim yapabilirsiniz.
- Kullanıcılarınızın Azure AD hesaplarıyla Amazon Web Services (AWS) (çoklu oturum açma) ile otomatik olarak oturum açmasını sağlayabilirsiniz.
- Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz-Azure portal.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek istiyorsanız, bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md).

![Sonuçlar listesinde Amazon Web Services (AWS)](./media/aws-multi-accounts-tutorial/amazonwebservice.png)

> [!NOTE]
> Lütfen bir AWS uygulamasının tüm AWS hesaplarınıza bağlanması, önerilen yaklaşımımız değildir. Bunun yerine, Azure AD 'de AWS hesabının birden çok örneğini birden çok AWS uygulamasının örneği ile yapılandırmak için [Bu](https://docs.microsoft.com/azure/active-directory/saas-apps/amazon-web-service-tutorial) yaklaşımı kullanmanızı öneririz. Bu yaklaşımı yalnızca birkaç AWS hesabı ve rolüne sahipseniz kullanmanız gerekir, AWS hesapları ve bu hesapların içindeki roller büyüdükçe bu model ölçeklenebilir değildir. Bu yaklaşım, Azure AD Kullanıcı sağlaması kullanılarak AWS rolü içeri aktarma işlevini kullanmaz, bu nedenle rolleri el ile eklemeniz/güncelleştirmeniz/silmeniz gerekir. Bu yaklaşım hakkında diğer sınırlamalar için lütfen aşağıdaki ayrıntılara bakın.

**Bu yaklaşımı aşağıdaki nedenlerle kullanmayı önermiyoruz.**

* Tüm rollerin uygulamaya yama yapmak için Microsoft Graph Explorer yaklaşımını kullanmanız gerekir. Bildirim dosyası yaklaşımını kullanmanızı önermiyoruz.

* Tek bir AWS uygulaması için ~ 1200 uygulama rolü ekledikten sonra bu müşterilerin bildirdiği müşterileri gördük, uygulamadaki tüm işlemler boyutla ilgili hataları oluşturulmasına başladı. Uygulama nesnesinde sabit bir boyut sınırı vardır.

* Rol, bir değiştirme yaklaşımı olan ve sonuna eklenmemiş olan hesaplara eklenen roller için el ile güncelleştirmeniz gerekir. Ayrıca hesaplarınız büyüdükçe bu, hesaplar ve rollerle n x n ilişki haline gelir.

* Tüm AWS hesapları aynı Federasyon meta veri XML dosyasını kullanacak ve sertifika geçişi sırasında, tüm AWS hesaplarında sertifikayı aynı anda güncelleştirmek için bu çok büyük bir Alıştırmayı kullanmanız gerekir

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Amazon Web Services (AWS) ile yapılandırmak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Bir Azure AD ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü edinebilirsiniz
* Amazon Web Services (AWS) çoklu oturum açma etkin aboneliği

> [!NOTE]
> Bu öğreticideki adımları test etmek için, üretim ortamının kullanılmasını önermiyoruz.

Bu öğreticideki adımları test etmek için aşağıdaki önerileri izlemeniz gerekir:

- Gerekli olmadığı takdirde üretim ortamınızı kullanmayın.
- Bir Azure AD deneme ortamınız yoksa, bir [aylık deneme sürümü](https://azure.microsoft.com/pricing/free-trial/)alabilirsiniz.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açmayı bir test ortamında yapılandırıp test edersiniz.

* Amazon Web Services (AWS) **, SP ve ıDP** tarafından başlatılan SSO 'yu destekler
* Amazon Web Services (AWS) yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak ayıklanmasını ve zaman korumasını koruyan oturum denetimini zorunlu kılabilirsiniz. Oturum denetimi koşullu erişimden genişletiliyor. [Microsoft Cloud App Security ile oturum denetimini nasıl zorlayacağınızı öğrenin](https://docs.microsoft.com/cloud-app-security/proxy-deployment-aad)

## <a name="adding-amazon-web-services-aws-from-the-gallery"></a>Galeriden Amazon Web Services (AWS) ekleme

Amazon Web Services (AWS) tümleştirmesini Azure AD ile yapılandırmak için, Galeriden yönetilen SaaS uygulamaları listenize Amazon Web Services (AWS) eklemeniz gerekir.

1. [Azure Portal](https://portal.azure.com) iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Amazon Web Services (AWS)** yazın.
1. Sonuçlar panelinden **Amazon Web Services (AWS)** öğesini seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

1. Uygulama eklendikten sonra **Özellikler** sayfasına gidin ve **nesne kimliğini**kopyalayın.

    ![Sonuçlar listesinde Amazon Web Services (AWS)](./media/aws-multi-accounts-tutorial/tutorial-amazonwebservices-properties.png)

## <a name="configure-and-test-azure-ad-sso"></a>Azure AD SSO 'yu yapılandırma ve test etme

Bu bölümde, Azure AD çoklu oturum açmayı, "Britta Simon" adlı bir test kullanıcısına göre Amazon Web Services (AWS) ile yapılandırıp test edersiniz.

Çoklu oturum açma 'nın çalışması için, Azure AD 'nin Amazon Web Services (AWS) içindeki karşılık gelen kullanıcının Azure AD 'deki bir kullanıcıya ne olduğunu bilmeleri gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve Amazon Web Services (AWS) içindeki ilgili Kullanıcı arasındaki bir bağlantı ilişkisinin kurulması gerekir.

Amazon Web Services (AWS) ' **de, bağlantı** ilişkisini oluşturmak için **Kullanıcı adının** değeri olarak Azure AD 'de Kullanıcı adı değerini atayın.

Amazon Web Services (AWS) ile Azure AD çoklu oturum açmayı yapılandırmak ve test etmek için aşağıdaki yapı taşlarını gerçekleştirmeniz gerekir:

1. **[Azure AD çoklu oturum açma özelliğini yapılandırarak](#configure-azure-ad-single-sign-on)** kullanıcılarınızın bu özelliği kullanmasına olanak sağlayın.
2. Uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için **[Amazon Web Services (AWS) çoklu oturum açmayı yapılandırın](#configure-amazon-web-services-aws-single-sign-on)** .
3. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[Çoklu oturum açmayı sınayın](#test-single-sign-on)** .

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure portal Azure AD çoklu oturum açma özelliğini etkinleştirir ve Amazon Web Services (AWS) uygulamanızda çoklu oturum açmayı yapılandırırsınız.

**Azure AD çoklu oturum açmayı Amazon Web Services (AWS) ile yapılandırmak için aşağıdaki adımları uygulayın:**

1. [Azure Portal](https://portal.azure.com/), **Amazon Web Services (AWS)** uygulama tümleştirmesi sayfasında, **Çoklu oturum açma**' yı seçin.

    ![Çoklu oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Çoklu oturum **açma yöntemi seç** iletişim kutusunda, çoklu oturum açmayı etkinleştirmek için **SAML/WS-Besme** modunu seçin.

    ![Çoklu oturum açma seçme modu](common/select-saml-option.png)

3. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **temel SAML yapılandırması** Iletişim kutusunu açmak için **Düzenle** simgesine tıklayın.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. **Temel SAML yapılandırması** bölümünde, uygulama zaten Azure ile önceden tümleştirilmiş olduğundan ve **Kaydet**' e tıkladığınızda, kullanıcının herhangi bir adım yapması gerekmez.

5. Amazon Web Services (AWS) uygulaması, belirli bir biçimde SAML onayları bekler. Bu uygulama için aşağıdaki talepleri yapılandırın. Bu özniteliklerin değerlerini, uygulama tümleştirme sayfasındaki **Kullanıcı öznitelikleri & talepler** bölümünde yönetebilirsiniz. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **Kullanıcı öznitelikleri & talepler** Iletişim kutusunu açmak için **Düzenle** düğmesine tıklayın.

    ![image](common/edit-attribute.png)

6. **Kullanıcı öznitelikleri** Iletişim kutusundaki **Kullanıcı talepleri** bölümünde, YUKARıDAKI görüntüde gösterildiği gibi SAML belirteci özniteliğini yapılandırın ve aşağıdaki adımları gerçekleştirin:

    | Adı  | Kaynak özniteliği  | Ad Alanı |
    | --------------- | --------------- | --------------- |
    | Roleoturumadı | User. UserPrincipalName | `https://aws.amazon.com/SAML/Attributes` |
    | Rol            | Kullanıcı. atandroles |  `https://aws.amazon.com/SAML/Attributes`|
    | SessionDuration             | "900 saniye (15 dakika) ila 43200 saniye (12 saat) arasında bir değer belirtin" |  `https://aws.amazon.com/SAML/Attributes` |

    a. **Kullanıcı taleplerini Yönet** iletişim kutusunu açmak için **yeni talep Ekle** ' ye tıklayın.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. **Ad** metin kutusuna, bu satır için gösterilen öznitelik adını yazın.

    c. **Ad alanı** metin kutusuna, bu satır Için gösterilen ad alanı değerini yazın.

    d. **Öznitelik**olarak kaynak seçin.

    e. **Kaynak özniteliği** listesinde, bu satır için gösterilen öznitelik değerini yazın.

    f. **Tamam 'a** tıklayın

    örneğin: **Kaydet**’e tıklayın.

7. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imza sertifikası** bölümünde, **Federasyon meta veri XML** 'Sini indirmek ve bilgisayarınıza kaydetmek için **İndir** ' e tıklayın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

### <a name="configure-amazon-web-services-aws-single-sign-on"></a>Amazon Web Services (AWS) çoklu oturum açmayı yapılandırma

1. Farklı bir tarayıcı penceresinde, Amazon Web Services (AWS) Şirket sitenizde yönetici olarak oturum açın.

1. **AWS giriş**' e tıklayın.

    ![Çoklu oturum açmayı yapılandırma][11]

1. **Kimlik ve erişim yönetimi**' ne tıklayın.

    ![Çoklu oturum açma kimliğini yapılandırma][12]

1. **Kimlik sağlayıcıları**' na ve ardından **sağlayıcı oluştur**' a tıklayın.

    ![Çoklu oturum açma sağlayıcısını yapılandırma][13]

1. **Sağlayıcıyı yapılandır** iletişim sayfasında, aşağıdaki adımları uygulayın:

    ![Çoklu oturum açma iletişim kutusunu yapılandırma][14]

    a. **Sağlayıcı türü**olarak **SAML**' yi seçin.

    b. **Sağlayıcı adı** metin kutusuna bir sağlayıcı adı yazın (örneğin: *Waad*).

    c. İndirilen **meta veri dosyanızı** Azure Portal karşıya yüklemek Için **Dosya Seç**' e tıklayın.

    d. **Ileri adım**' a tıklayın.

1. **Sağlayıcı bilgilerini doğrula** Iletişim sayfasında **Oluştur**' a tıklayın.

    ![Çoklu oturum açmayı yapılandırma][15]

1. **Roller**' e tıklayın ve ardından **rol oluştur**' a tıklayın.

    ![Çoklu oturum açma rollerini yapılandırma][16]

1. **Rol oluştur** sayfasında, aşağıdaki adımları uygulayın:  

    ![Çoklu oturum açma güvenini yapılandırma][19]

    a. **Güvenilen varlık seçin**' ın altında **SAML 2,0 Federasyonu** ' ni seçin.

    b. **SAML 2,0 sağlayıcısı Seç bölümünde**, daha önce oluşturduğunuz **SAML sağlayıcısını** seçin (örneğin: *Waad*)

    c. **Programlı ve AWS Yönetim Konsolu erişimine Izin ver '** i seçin.
  
    d. **İleri**' ye tıklayın.

1. Arama çubuğunda **yönetici erişimi** arayın ve **Yönetimtoraccess** onay kutusunu seçin ve ardından **İleri: Etiketler**' e tıklayın.

    ![Yönetici erişimini seçin](./media/aws-multi-accounts-tutorial/administrator-access.png)

1. **Etiket ekle (isteğe bağlı)** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Yönetici erişimini seçin](./media/aws-multi-accounts-tutorial/config2.png)

    a. **Anahtar** metin kutusuna, Ex: Azureadtest için anahtar adını girin.

    b. **Değer (isteğe bağlı)** metin kutusuna aşağıdaki biçimi kullanarak anahtar değerini girin `accountname-aws-admin` . Hesap adı tamamen küçük harfle yazılmalıdır.

    c. **İleri**' ye tıklayın.

1. **Gözden geçirme** iletişim kutusunda, aşağıdaki adımları uygulayın:

    ![Çoklu oturum açma Incelemesini yapılandırma][34]

    a. **Rol adı** metin kutusuna aşağıdaki düzende bulunan değeri girin `accountname-aws-admin` .

    b. **Rol açıklaması** metin kutusunda, rol adı için kullandığınız değeri girin.

    c. **Rol oluştur**' a tıklayın.

    d. Gereken sayıda rol oluşturun ve bunları kimlik sağlayıcısına eşleyin.

    > [!NOTE]
    > Benzer şekilde, AccountName-finans-admin, AccountName-Read-Only-User, AccountName-DevOps-User, AccountName-TPM-eklenecek farklı ilkelere sahip diğer diğer rolleri oluşturun. Ayrıca, bu rol ilkeleri AWS hesabı başına gereksinimlere göre değiştirilebilir, ancak her bir rol için AWS hesaplarında aynı ilkelerin tutulması her zaman daha iyidir.

1. Lütfen bu AWS hesabının hesap KIMLIĞINI EC2 özelliklerinden veya ıAM panosundan aşağıda vurgulanan şekilde bir yere göz önüne alın:

    ![Yönetici erişimini seçin](./media/aws-multi-accounts-tutorial/aws-accountid.png)

1. Şimdi [Azure Portal](https://portal.azure.com/) oturum açın ve **gruplar**'a gidin.

1. Daha önce oluşturulan ıAM rolleriyle aynı ada sahip yeni gruplar oluşturun ve bu yeni grupların **nesne kimliklerini** unutmayın.

    ![Yönetici erişimini seçin](./media/aws-multi-accounts-tutorial/copy-objectids.png)

1. Geçerli AWS hesabından oturumunuzu açın ve Azure AD ile çoklu oturum açmayı yapılandırmak istediğiniz diğer hesapla oturum açın.

1. Hesaplarda tüm roller oluşturulduktan sonra, bu hesaplar için **Roller** listesinde görünür.

    ![Rol kurulumu](./media/aws-multi-accounts-tutorial/tutorial-amazonwebservices-listofroles.png)

1. Tüm hesapların tüm rolleri için tüm rol ARN ve güvenilen varlıklarını, Azure AD uygulamasıyla el ile eşlemeniz gereken tüm roller için yakalamamız gerekir.

1. **Rol ARN** ve **güvenilen varlıklar** değerlerini kopyalamak için rollere tıklayın. Azure AD 'de oluşturmanız gereken tüm roller için bu değerlere ihtiyacınız vardır.

    ![Rol kurulumu](./media/aws-multi-accounts-tutorial/tutorial-amazonwebservices-role-summary.png)

1. Tüm hesaplardaki tüm roller için yukarıdaki adımı gerçekleştirin ve bunların tümünü bir not defteri içinde **rol ARN, güvenilen varlıklar** biçiminde depolayın.

1. [Microsoft Graph Gezginini](https://developer.microsoft.com/graph/graph-explorer) başka bir pencerede açın.

    a. Kiracınız için genel yönetici/ortak yönetici kimlik bilgilerini kullanarak Microsoft Graph gezgin sitesinde oturum açın.

    b. Rolleri oluşturmak için yeterli izinlere sahip olmanız gerekir. Gerekli izinleri almak için **izinleri değiştir** ' e tıklayın.

    ![Microsoft Graph Gezgini iletişim kutusu](./media/aws-multi-accounts-tutorial/graph-explorer-new9.png)

    c. Listeden aşağıdaki izinleri seçin (henüz yoksa) ve "Izinleri Değiştir" e tıklayın 

    ![Microsoft Graph Gezgini iletişim kutusu](./media/aws-multi-accounts-tutorial/graph-explorer-new10.png)

    d. Bu, yeniden oturum açıp onayı kabul etmenizi ister. Onayı kabul ettikten sonra, Microsoft Graph Explorer 'da yeniden oturum açarsınız.

    e. Sürüm açılan menüsünü **Beta**olarak değiştirin. Kiracınızdaki tüm hizmet sorumlularını getirmek için aşağıdaki sorguyu kullanın:

    `https://graph.microsoft.com/beta/servicePrincipals`

    Birden çok dizin kullanıyorsanız, üzerinde birincil etki alanınız bulunan aşağıdaki kalıbı kullanabilirsiniz  `https://graph.microsoft.com/beta/contoso.com/servicePrincipals`

    ![Microsoft Graph Gezgini iletişim kutusu](./media/aws-multi-accounts-tutorial/graph-explorer-new1.png)

    f. Getirilen hizmet sorumluları listesinden, değiştirmeniz gereken bir tane alın. Ayrıca, uygulamayı listelenen tüm hizmet sorumlularından aramak için CTRL + F ' i de kullanabilirsiniz. İlgili hizmet sorumlusuna ulaşmak için Azure AD Özellikleri sayfasından kopyaladığınız **hizmet sorumlusu nesne kimliğini** kullanarak aşağıdaki sorguyu kullanabilirsiniz.

    `https://graph.microsoft.com/beta/servicePrincipals/<objectID>`.

    ![Microsoft Graph Gezgini iletişim kutusu](./media/aws-multi-accounts-tutorial/graph-explorer-new2.png)

    örneğin: Hizmet sorumlusu nesnesinden appRoles özelliğini ayıklayın.

    ![Microsoft Graph Gezgini iletişim kutusu](./media/aws-multi-accounts-tutorial/graph-explorer-new3.png)

    h. Artık uygulamanız için yeni roller oluşturmanız gerekir. 

    i. Aşağıdaki JSON, appRoles nesnesine bir örnektir. Uygulamanız için istediğiniz rolleri eklemek için benzer bir nesne oluşturun.

    ```
    {
    "appRoles": [
        {
            "allowedMemberTypes": [
                "User"
            ],
            "description": "msiam_access",
            "displayName": "msiam_access",
            "id": "7dfd756e-8c27-4472-b2b7-38c17fc5de5e",
            "isEnabled": true,
            "origin": "Application",
            "value": null
        },
        {
            "allowedMemberTypes": [
                "User"
            ],
            "description": "Admin,WAAD",
            "displayName": "Admin,WAAD",
            "id": "4aacf5a4-f38b-4861-b909-bae023e88dde",
            "isEnabled": true,
            "origin": "ServicePrincipal",
            "value": "arn:aws:iam::12345:role/Admin,arn:aws:iam::12345:saml-provider/WAAD"
        },
        {
            "allowedMemberTypes": [
                "User"
            ],
            "description": "Auditors,WAAD",
            "displayName": "Auditors,WAAD",
            "id": "bcad6926-67ec-445a-80f8-578032504c09",
            "isEnabled": true,
            "origin": "ServicePrincipal",
            "value": "arn:aws:iam::12345:role/Auditors,arn:aws:iam::12345:saml-provider/WAAD"
        }    ]
    }
    ```

    > [!Note]
    > Düzeltme Eki işlemi için **msiam_access** sonrasında yalnızca yeni roller ekleyebilirsiniz. Ayrıca, kuruluşunuzun ihtiyaç duyduğu kadar istediğiniz sayıda rol ekleyebilirsiniz. Azure AD, bu rollerin **DEĞERINI** SAML yanıtında talep değeri olarak gönderir.

    j. Microsoft Graph Gezginine dönün ve yöntemi **Al** 'dan **Patch**öğesine değiştirin. Örnek bölümünde gösterilenle benzer appRoles özelliğini güncelleştirerek istenen rollere sahip olmak için hizmet sorumlusu nesnesine yama yapın. Düzeltme Eki işlemini yürütmek için **Sorguyu Çalıştır** ' a tıklayın. Başarılı iletisi, Amazon Web Services uygulamanız için rolün oluşturulmasını onaylar.

    ![Microsoft Graph Gezgini iletişim kutusu](./media/aws-multi-accounts-tutorial/graph-explorer-new11.png)

1. Hizmet sorumlusu daha fazla rolle düzeltme eki uygulandıktan sonra, ilgili rollere kullanıcılar/gruplar atayabilirsiniz. Bu, portala gidip Amazon Web Services uygulamasına gidilerek yapılabilir. Üstteki **Kullanıcılar ve gruplar** sekmesine tıklayın.

1. Bu gruptaki belirli bir rolü atayabilmeniz için her bir AWS rolü için yeni gruplar oluşturmanız önerilir. Bu, bir grup için tek bir rolün tek bir eşlemesine bir eşleme olduğunu unutmayın. Daha sonra, o gruba ait olan üyeleri ekleyebilirsiniz.

1. Gruplar oluşturulduktan sonra, grubu seçin ve uygulamaya atayın.

    ![Çoklu oturum açma eklemeyi yapılandırma](./media/aws-multi-accounts-tutorial/graph-explorer-new5.png)

    > [!Note]
    > Grupları atarken iç içe gruplar desteklenmez.

1. Rolü gruba atamak için, rolü seçin ve sayfanın altındaki **ata** düğmesine tıklayın.

    ![Çoklu oturum açma eklemeyi yapılandırma](./media/aws-multi-accounts-tutorial/graph-explorer-new6.png)

    > [!Note]
    > Yeni rolleri görmek için Azure portal oturumunuzu yenilemeniz gerektiğini lütfen unutmayın.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim panelinde Amazon Web Services (AWS) kutucuğuna tıkladığınızda, rolü seçme seçeneği ile Amazon Web Services (AWS) uygulama sayfası almanız gerekir.

![Çoklu oturum açma eklemeyi yapılandırma](./media/aws-multi-accounts-tutorial/tutorial-amazonwebservices-test-screen.png)

Ayrıca, talep olarak geçirilen rolleri görmek için SAML yanıtını doğrulayabilirsiniz.

![Çoklu oturum açma eklemeyi yapılandırma](./media/aws-multi-accounts-tutorial/tutorial-amazonwebservices-test-saml.png)

Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](../active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [MS Graph API 'Leri kullanarak sağlamayı yapılandırma](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-configure-api)
* [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)
* [Microsoft Cloud App Security oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)
* [Gelişmiş görünürlük ve denetimlerle Amazon Web Services (AWS) koruma](https://docs.microsoft.com/cloud-app-security/protect-aws)

<!--Image references-->

[11]: ./media/aws-multi-accounts-tutorial/ic795031.png
[12]: ./media/aws-multi-accounts-tutorial/ic795032.png
[13]: ./media/aws-multi-accounts-tutorial/ic795033.png
[14]: ./media/aws-multi-accounts-tutorial/ic795034.png
[15]: ./media/aws-multi-accounts-tutorial/ic795035.png
[16]: ./media/aws-multi-accounts-tutorial/ic795022.png
[17]: ./media/aws-multi-accounts-tutorial/ic795023.png
[18]: ./media/aws-multi-accounts-tutorial/ic795024.png
[19]: ./media/aws-multi-accounts-tutorial/ic795025.png
[32]: ./media/aws-multi-accounts-tutorial/ic7950251.png
[33]: ./media/aws-multi-accounts-tutorial/ic7950252.png
[35]: ./media/aws-multi-accounts-tutorial/tutorial-amazonwebservices-provisioning.png
[34]: ./media/aws-multi-accounts-tutorial/config3.png
[36]: ./media/aws-multi-accounts-tutorial/tutorial-amazonwebservices-securitycredentials.png
[37]: ./media/aws-multi-accounts-tutorial/tutorial-amazonwebservices-securitycredentials-continue.png
[38]: ./media/aws-multi-accounts-tutorial/tutorial-amazonwebservices-createnewaccesskey.png
[39]: ./media/aws-multi-accounts-tutorial/tutorial-amazonwebservices-provisioning-automatic.png
[40]: ./media/aws-multi-accounts-tutorial/tutorial-amazonwebservices-provisioning-testconnection.png
[41]: ./media/aws-multi-accounts-tutorial/