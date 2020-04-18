---
title: 'Öğretici: Birden çok hesabı bağlamak için Amazon Web Hizmetleri (AWS) ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure AD ve Amazon Web Hizmetleri (AWS) (Eski Öğretici) arasında tek oturum açma işlemlerini nasıl yapılandırabileceğinizi öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 7561c20b-2325-4d97-887f-693aa383c7be
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 04/16/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 51be98654950ba290fa83f77eccdae4d6f549891
ms.sourcegitcommit: 5e49f45571aeb1232a3e0bd44725cc17c06d1452
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81603837"
---
# <a name="tutorial-azure-active-directory-integration-with-amazon-web-services-aws-legacy-tutorial"></a>Öğretici: Amazon Web Hizmetleri (AWS) ile Azure Active Directory entegrasyonu (Eski Öğretici)

Bu öğreticide, Azure Active Directory'yi (Azure AD) ile Amazon Web Hizmetleri (AWS) (Eski Öğretici) ile nasıl entegre acağınızı öğrenirsiniz.

Amazon Web Hizmetlerini (AWS) Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

- Amazon Web Hizmetleri'ne (AWS) erişimi olan Azure AD'da denetim yapabilirsiniz.
- Kullanıcılarınızın Azure AD hesaplarıyla Amazon Web Hizmetleri'nde (AWS) (Tek Oturum Açma) otomatik olarak oturum açmalarını sağlayabilirsiniz.
- Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişiminin ve tek oturum açmanın ne olduğunu](../manage-apps/what-is-single-sign-on.md)görün.

![Amazon Web Services (AWS) sonuç listesinde](./media/aws-multi-accounts-tutorial/amazonwebservice.png)

> [!NOTE]
> Lütfen bir AWS uygulamasını tüm AWS hesaplarınıza bağlamanın tavsiye edilen yaklaşımımız olmadığını unutmayın. Bunun [yerine,](https://docs.microsoft.com/azure/active-directory/saas-apps/amazon-web-service-tutorial) Birden çok AWS hesabı örneğini Azure AD'deki Birden çok AWS uygulaması örneğine yapılandırmak için bu yaklaşımı kullanmanızı öneririz. Bu yaklaşımı yalnızca içinde birkaç AWS Hesabı ve Rolü varsa kullanmalısınız, bu model, AWS hesapları ve bu hesapların içindeki roller büyüdükçe ölçeklenebilir değildir. Bu yaklaşım, Azure AD Kullanıcı Sağlama'yı kullanarak AWS Role alma işlevini kullanmaz, bu nedenle rolleri el ile eklemeniz/güncelleştirmeniz/silmeniz gerekir. Bu yaklaşımla ilgili diğer sınırlamalar için lütfen aşağıdaki ayrıntılara bakın.

**Bu yaklaşımı aşağıdaki nedenlerle kullanmanızı önermediğimizi lütfen unutmayın:**

* Uygulamanın tüm rollerini yamalamak için Microsoft Graph Explorer yaklaşımını kullanmanız gerekir. Bildirim dosyası yaklaşımını kullanmanızı önermiyoruz.

* Müşterilerin, tek bir AWS uygulaması için ~1200 uygulama rollerini ekledikten sonra, uygulamadaki herhangi bir işlemin boyutla ilgili hataları atmaya başladığını bildiren müşteriler gördük. Uygulama nesnesi üzerinde sabit bir boyut sınırı vardır.

* Roller hesaplardan herhangi birinde eklendikçe rolü el ile güncelleştirmeniz gerekir, bu da ne yazık ki Append değil, değiştir yaklaşımıdır. Ayrıca hesaplarınız büyüyorsa, bu hesaplar ve rollerle n x n ilişkisi olur.

* Tüm AWS hesapları aynı Federasyon Metadata XML dosyasını kullanacak ve sertifika devri sırasında aynı anda tüm AWS hesaplarında Sertifikayı güncelleştirmek için bu büyük alıştırmayı yapmak zorundasınız

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Amazon Web Hizmetleri (AWS) ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü alabilirsiniz
* Amazon Web Services (AWS) tek oturum açma özellikli abonelik

> [!NOTE]
> Bu öğreticideki adımları sınamak için bir üretim ortamı kullanmanızı önermiyoruz.

Bu öğreticideki adımları sınamak için aşağıdaki önerileri izlemeniz gerekir:

- Gerekli olmadıkça üretim ortamınızı kullanmayın.
- Azure AD deneme ortamınız yoksa, bir [aylık deneme sürümü alabilirsiniz.](https://azure.microsoft.com/pricing/free-trial/)

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* Amazon Web Services (AWS) **SP ve IDP** SSO başlatılan destekler
* Amazon Web Hizmetlerini (AWS) yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak sızma ve sızmalarını koruyan Oturum Denetimi'ni uygulayabilirsiniz. Oturum Denetimi Koşullu Erişim'den genişletir. [Microsoft Cloud App Security ile oturum denetimini nasıl uygulayacağınızı öğrenin](https://docs.microsoft.com/cloud-app-security/proxy-deployment-aad)

## <a name="adding-amazon-web-services-aws-from-the-gallery"></a>Galeriden Amazon Web Hizmetleri (AWS) ekleme

Amazon Web Hizmetleri'nin (AWS) Azure AD'ye entegrasyonunu yapılandırmak için, galeriden yönetilen SaaS uygulamaları listenize Amazon Web Hizmetleri 'ni (AWS) eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Amazon Web Hizmetleri (AWS)** yazın.
1. Sonuç panelinden **Amazon Web Services'ı (AWS)** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

1. Uygulama eklendikten sonra **Özellikler** sayfasına gidin ve **Nesne Kimliğini**kopyalayın.

    ![Amazon Web Services (AWS) sonuç listesinde](./media/aws-multi-accounts-tutorial/tutorial-amazonwebservices-properties.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Azure AD'nin "Britta Simon" adlı bir test kullanıcısına göre Amazon Web Hizmetleri (AWS) ile tek oturum açma işlemini yapılandırıp sınayabilirsiniz.

Tek oturum açmanın işe yaraması için Azure AD'nin Amazon Web Hizmetleri'ndeki (AWS) karşı kullanıcının Azure AD'deki bir kullanıcıya ne olduğunu bilmesi gerekir. Başka bir deyişle, Bir Azure AD kullanıcısı ile Amazon Web Hizmetleri'ndeki (AWS) ilgili kullanıcı arasında bir bağlantı ilişkisi nin kurulması gerekir.

Amazon Web Services'ta (AWS), bağlantı ilişkisini kurmak için Azure AD'deki **kullanıcı adının** değerini **Kullanıcı adı** değeri olarak atayın.

Azure AD oturumlarını Amazon Web Services (AWS) ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. **[Amazon Web Services (AWS) Tek Oturum Açma](#configure-amazon-web-services-aws-single-sign-on)** -uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için yapılandırın.
3. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturumunu etkinleştirin ve Amazon Web Hizmetleri (AWS) uygulamanızda tek oturum açma'yı yapılandırırsınız.

**Azure AD oturumlarını Amazon Web Hizmetleri (AWS) ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure [portalında,](https://portal.azure.com/) **Amazon Web Services (AWS)** uygulama tümleştirme sayfasında Tek oturum **açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde, uygulama Azure ile önceden entegre edilmiş olduğundan kullanıcının herhangi bir adım gerçekleştirmesine gerek yoktur ve **Kaydet'i**tıklatın.

5. Amazon Web Services (AWS) uygulaması belirli bir biçimde SAML iddiaları bekliyor. Bu uygulama için aşağıdaki talepleri yapılandırın. Bu özniteliklerin değerlerini, uygulama tümleştirme sayfasındaki **Kullanıcı Öznitelikleri & Talepler** bölümünden yönetebilirsiniz. **SAML sayfasıyla Tek Oturum** Açma'da, **Kullanıcı Öznitelikleri & Talepler** iletişim kutusunu açmak için **Edit** düğmesini tıklatın.

    ![image](common/edit-attribute.png)

6. **Kullanıcı Öznitelikleri** iletişim kutusundaki **Kullanıcı Talepleri** bölümünde, saml belirteç özniteliğini yukarıdaki resimde gösterildiği gibi yapılandırın ve aşağıdaki adımları gerçekleştirin:

    | Adı  | Kaynak Özniteliği  | Ad Alanı |
    | --------------- | --------------- | --------------- |
    | RoleSessionName | user.userprincipalname | `https://aws.amazon.com/SAML/Attributes` |
    | Rol            | user.assignedroles |  `https://aws.amazon.com/SAML/Attributes`|
    | Oturum Süresi             | "900 saniye (15 dakika) ile 43200 saniye (12 saat) arasında bir değer sağlar" |  `https://aws.amazon.com/SAML/Attributes` |

    a. **Kullanıcı taleplerini yönet** iletişim kutusunu açmak için yeni **talep ekle'yi** tıklatın.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. **Ad** metin kutusunda, bu satır için gösterilen öznitelik adını yazın.

    c. Ad **Alanı** metin kutusunda, bu satır için gösterilen Ad Alanı değerini yazın.

    d. **Kaynak'ı Öznitelik**olarak seçin.

    e. Kaynak **öznitelik** listesinden, bu satır için gösterilen öznitelik değerini yazın.

    f. **Tamam'ı** tıklatın

    g. **Kaydet**’e tıklayın.

7. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, Federasyon **Metadata XML'i** indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

### <a name="configure-amazon-web-services-aws-single-sign-on"></a>Yapılandırma Amazon Web Services (AWS) Tek Oturum Açma

1. Farklı bir tarayıcı penceresinde, Amazon Web Services (AWS) şirket sitenizi yönetici olarak oturum açın.

1. **AWS Ana Sayfa'yı**tıklatın.

    ![Tek Oturum Açma ev yapılandırma][11]

1. **Kimlik ve Erişim Yönetimi'ni**tıklatın.

    ![Tek Oturum Açma Kimliğini Yapılandırma][12]

1. **Kimlik Sağlayıcıları'nı**tıklatın ve ardından **Sağlayıcı Oluştur'u**tıklatın.

    ![Tek Oturum Açma Sağlayıcısını Yapılandırma][13]

1. **Sağlayıcıyı Yapılandırıyorum** iletişim sayfasında aşağıdaki adımları gerçekleştirin:

    ![Tek Oturum Açma iletişim kutusunu yapılandırma][14]

    a. **Sağlayıcı Türü**olarak, **SAML'yi**seçin.

    b. Sağlayıcı **Adı** metin kutusunda bir sağlayıcı adı yazın (örneğin: *WAAD).*

    c. İndirdiğiniz meta **veri dosyanızı** Azure portalından yüklemek için **Dosya yı seç'i**tıklatın.

    d. **Sonraki Adım'ı**tıklatın.

1. Sağlayıcı **Bilgilerini Doğrula** iletişim sayfasında **Oluştur'u**tıklatın.

    ![Tek İşaret-On Doğrula'yı Yapılandır][15]

1. **Roller'i**tıklatın ve ardından **Rol Oluştur'u**tıklatın.

    ![Tek Oturum Açma Rollerini Yapılandırma][16]

1. Rol **Oluştur** sayfasında aşağıdaki adımları gerçekleştirin:  

    ![Tek Oturum Açma Güvenini Yapılandırma][19]

    a. **Güvenilir varlık türü**altında **SAML 2.0 federasyonu** seçin.

    b. **SAML 2.0 Sağlayıcı bölümünü seçin,** daha önce oluşturduğunuz **SAML sağlayıcısını** seçin (örneğin: *WAAD*)

    c. **Programatik ve AWS Yönetim Konsoluna Erişime İzin Ver'i**seçin.
  
    d. **Sonraki'yi tıklatın: İzinler**.

1. Arama çubuğunda **Yönetici Erişimi'ni** arayın ve **AdministratorAccess** onay kutusunu seçin ve **ardından Sonraki: Etiketler'i**tıklatın.

    ![Yönetici Erişimi'ni seçin](./media/aws-multi-accounts-tutorial/administrator-access.png)

1. **Etiketlerekle (isteğe bağlı)** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Yönetici Erişimi'ni seçin](./media/aws-multi-accounts-tutorial/config2.png)

    a. **Anahtar** metin kutusuna eski sevgilinin anahtar adını girin: Azureadtest.

    b. Değer **(isteğe bağlı)** textbox'a, aşağıdaki `accountname-aws-admin`biçimi kullanarak anahtar değerini girin. Hesap adı tüm küçük harfle olmalıdır.

    c. Sonraki ni **tıklatın: Gözden geçirin.**

1. Gözden **Geçirme** iletişim kutusunda aşağıdaki adımları gerçekleştirin:

    ![Tek İşaretLi İncelemeyi Yapılandır][34]

    a. Rol **adı** metin kutusuna, aşağıdaki desendeki `accountname-aws-admin`değeri girin.

    b. Rol **açıklaması** metin kutusuna, rol adı için kullandığınız değeri girin.

    c. **Rol Oluştur'u**tıklatın.

    d. Gerektiği kadar rol oluşturun ve bunları Kimlik Sağlayıcısı ile eşleyin.

    > [!NOTE]
    > Benzer şekilde, hesap adı-finans-admin, hesap adı-salt-user, accountname-devops-user, accountname-tpm-user gibi farklı ilkeler eklenecek kalan diğer rolleri oluşturun. Daha sonra da bu rol ilkeleri AWS hesabı başına gereksinimleri ne göre değiştirilebilir, ancak Her zaman AWS hesapları arasında her rol için aynı ilkeleri tutmak için daha iyidir.

1. Aşağıda vurgulandığı gibi EC2 özelliklerinden veya IAM panosundan bu AWS hesabı için lütfen hesap kimliği notu alın:

    ![Yönetici Erişimi'ni seçin](./media/aws-multi-accounts-tutorial/aws-accountid.png)

1. Şimdi [Azure portalında](https://portal.azure.com/) oturum açın ve **Gruplar'a**gidin.

1. Daha önce oluşturulan IAM Rolleri ile aynı ada sahip yeni gruplar oluşturun ve bu yeni grupların **Nesne Adlarını** not edin.

    ![Yönetici Erişimi'ni seçin](./media/aws-multi-accounts-tutorial/copy-objectids.png)

1. Geçerli AWS hesabından oturum açın ve Azure AD ile tek oturum açmak istediğiniz diğer hesapla oturum açın.

1. Tüm roller hesaplarda oluşturulduktan sonra, bu hesapların **Roller** listesinde gösterirler.

    ![Roller kurulumu](./media/aws-multi-accounts-tutorial/tutorial-amazonwebservices-listofroles.png)

1. Azure AD uygulamasıyla el ile haritalamamız gereken tüm hesaplardaki tüm roller için tüm Role ARN ve Trusted Ntities'ı yakalamamız gerekir.

1. **Role ARN** ve **Trusted Ntities** values değerlerini kopyalamak için rolleri tıklatın. Azure AD'de oluşturmanız gereken tüm roller için bu değerlere ihtiyacınız var.

    ![Roller kurulumu](./media/aws-multi-accounts-tutorial/tutorial-amazonwebservices-role-summary.png)

1. Tüm hesaplardaki tüm roller için yukarıdaki adımı gerçekleştirin ve bunların hepsini **Role ARN,Trusted varlıklar** biçiminde bir not defterinde saklayın.

1. [Microsoft Graph Explorer'ı](https://developer.microsoft.com/graph/graph-explorer) başka bir pencerede açın.

    a. Kiracınız için Global Admin/Co-admin kimlik bilgilerini kullanarak Microsoft Graph Explorer sitesinde oturum açın.

    b. Rolleri oluşturmak için yeterli izinlere sahip olmanız gerekir. Gerekli **izinleri** almak için izinleri değiştir'i tıklatın.

    ![Microsoft Graph Explorer iletişim kutusu](./media/aws-multi-accounts-tutorial/graph-explorer-new9.png)

    c. Listeden aşağıdaki izinleri seçin (bunlar zaten yoksa) ve "İzinleri Değiştir"i tıklayın 

    ![Microsoft Graph Explorer iletişim kutusu](./media/aws-multi-accounts-tutorial/graph-explorer-new10.png)

    d. Bu, tekrar oturum açmanızı ve onayı kabul etmenizi isteyecektir. Onayı kabul ettikten sonra, Microsoft Graph Explorer'da yeniden oturum açmış olursunuz.

    e. Sürüm açılır sürümünü **beta**olarak değiştirin. Tüm Hizmet Müdürlerini kiracınızdan almak için aşağıdaki sorguyu kullanın:

    `https://graph.microsoft.com/beta/servicePrincipals`

    Birden çok dizin kullanıyorsanız, birincil etki alanınız içinde bulunan aşağıdaki deseni kullanabilirsiniz`https://graph.microsoft.com/beta/contoso.com/servicePrincipals`

    ![Microsoft Graph Explorer iletişim kutusu](./media/aws-multi-accounts-tutorial/graph-explorer-new1.png)

    f. Getirilen Hizmet Müdürleri listesinden değiştirmeniz gerekeni alın. Ayrıca, listelenen tüm ServicePrincipals'dan uygulamayı aramak için Ctrl+F'yi de kullanabilirsiniz. İlgili Hizmet Sorumlusuna ulaşmak için Azure AD Özellikleri sayfasından kopyalamış olduğunuz **Nesne Kimliği'ni** kullanarak sorguyu aşağıdaki leri kullanabilirsiniz.

    `https://graph.microsoft.com/beta/servicePrincipals/<objectID>`.

    ![Microsoft Graph Explorer iletişim kutusu](./media/aws-multi-accounts-tutorial/graph-explorer-new2.png)

    g. Hizmet ana nesnesinden appRoles özelliğini ayıklayın.

    ![Microsoft Graph Explorer iletişim kutusu](./media/aws-multi-accounts-tutorial/graph-explorer-new3.png)

    h. Artık uygulamanız için yeni roller oluşturmanız gerekir. 

    i. JSON'un altında appRoles nesnesinin bir örneği yer almaktadır. Uygulamanız için istediğiniz rolleri eklemek için benzer bir nesne oluşturun.

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
    > Yalnızca yama işlemi için **msiam_access** sonra yeni roller ekleyebilirsiniz. Ayrıca, Kuruluşunuz gereksiniminize göre istediğiniz kadar rol ekleyebilirsiniz. Azure AD, SAML yanıtında talep değeri olarak bu rollerin **değerini** gönderir.

    j. Microsoft Graph Explorer'ınıza geri dön ve yöntemi **GET'den** **PATCH'e**değiştirin. Örnekte yukarıda gösterilene benzer appRoles özelliğini güncelleyerek Hizmet Müdürü nesnesini istenen rollere sahip olacak şekilde yama. Yama işlemini yürütmek için **Sorguyu Çalıştır'ı** tıklatın. Bir başarı mesajı Amazon Web Hizmetleri uygulamanız için rolün oluşturulmasını onaylar.

    ![Microsoft Graph Explorer iletişim kutusu](./media/aws-multi-accounts-tutorial/graph-explorer-new11.png)

1. Hizmet Sorumlusu daha fazla rolle yamalı hale geldikten sonra, Kullanıcıları/Grupları ilgili rollere atayabilirsiniz. Bu portal a gidip Amazon Web Services uygulamasına gezinerek yapılabilir. Üstteki **Kullanıcılar ve Gruplar** sekmesine tıklayın.

1. Söz konusu gruptaki belirli rolü atayabilmeniz için her AWS rolü için yeni gruplar oluşturmanızı öneririz. Bunun bir gruba bir rol için bire bir eşleme olduğunu unutmayın. Daha sonra bu gruba ait üyeleri ekleyebilirsiniz.

1. Gruplar oluşturulduktan sonra grubu seçin ve uygulamaya atayın.

    ![Tek İşaret Eklemeyi Yapılandır](./media/aws-multi-accounts-tutorial/graph-explorer-new5.png)

    > [!Note]
    > İç içe gruplar grup atarken desteklenmez.

1. Rolü gruba atamak için rolü seçin ve sayfanın altındaki **Atla düğmesini** tıklatın.

    ![Tek İşaret Eklemeyi Yapılandır](./media/aws-multi-accounts-tutorial/graph-explorer-new6.png)

    > [!Note]
    > Yeni roller görmek için Azure portalındaki oturumunuzu yenilemeniz gerektiğini lütfen unutmayın.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki Amazon Web Hizmetleri (AWS) döşemesini tıklattığınızda, rolü seçme seçeneğiyle Birlikte Amazon Web Hizmetleri (AWS) uygulama sayfasını almalısınız.

![Tek İşaret Eklemeyi Yapılandır](./media/aws-multi-accounts-tutorial/tutorial-amazonwebservices-test-screen.png)

Ayrıca, rollerin talep olarak geçirildiğini görmek için SAML yanıtını da doğrulayabilirsiniz.

![Tek İşaret Eklemeyi Yapılandır](./media/aws-multi-accounts-tutorial/tutorial-amazonwebservices-test-saml.png)

Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](../active-directory-saas-access-panel-introduction.md)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

* [MS Graph API'leri kullanarak sağlama yapılandırma nasıl yapılır?](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-configure-api)
* [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)
* [Microsoft Cloud App Security'de oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)
* [Amazon Web Services (AWS) gelişmiş görünürlük ve kontroller ile nasıl korunur?](https://docs.microsoft.com/cloud-app-security/protect-aws)

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