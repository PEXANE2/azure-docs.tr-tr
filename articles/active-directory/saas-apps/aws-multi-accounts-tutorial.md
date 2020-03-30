---
title: 'Öğretici: Birden çok hesabı bağlamak için Amazon Web Hizmetleri (AWS) ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure AD ile Amazon Web Hizmetleri'nin (AWS) birden çok hesabı arasında tek oturum açma yı nasıl yapılandırabileceğinizi öğrenin.
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
ms.devlang: na
ms.topic: article
ms.date: 01/07/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: edd54352b1328c95ae2c3e466003b64eaa0fcfde
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77368007"
---
# <a name="tutorial-azure-active-directory-integration-with-multiple-amazon-web-services-aws-accounts"></a>Öğretici: Birden çok Amazon Web Hizmetleri (AWS) hesabıyla Azure Active Directory entegrasyonu

Bu eğitimde, Azure Etkin Dizinini (Azure AD) ile Amazon Web Hizmetleri 'nin (AWS) birden çok hesabıyla nasıl entegre acağınızı öğrenirsiniz.

Amazon Web Hizmetlerini (AWS) Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

- Amazon Web Hizmetleri'ne (AWS) erişimi olan Azure AD'da denetim yapabilirsiniz.
- Kullanıcılarınızın Azure AD hesaplarıyla Amazon Web Hizmetleri'nde (AWS) (Tek Oturum Açma) otomatik olarak oturum açmalarını sağlayabilirsiniz.
- Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişiminin ve tek oturum açmanın ne olduğunu](../manage-apps/what-is-single-sign-on.md)görün.

![Amazon Web Services (AWS) sonuç listesinde](./media/aws-multi-accounts-tutorial/amazonwebservice.png)

>[!NOTE]
>Lütfen bir AWS uygulamasını tüm AWS hesaplarınıza bağlamanın tavsiye edilen yaklaşımımız olmadığını unutmayın. Bunun [yerine,](https://docs.microsoft.com/azure/active-directory/saas-apps/amazon-web-service-tutorial) Birden çok AWS hesabı örneğini Azure AD'deki Birden çok AWS uygulaması örneğine yapılandırmak için bu yaklaşımı kullanmanızı öneririz.

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

## <a name="adding-amazon-web-services-aws-from-the-gallery"></a>Galeriden Amazon Web Hizmetleri (AWS) ekleme

Amazon Web Hizmetleri'nin (AWS) Azure AD'ye entegrasyonunu yapılandırmak için, galeriden yönetilen SaaS uygulamaları listenize Amazon Web Hizmetleri 'ni (AWS) eklemeniz gerekir.

**Galeriden Amazon Web Hizmetleri (AWS) eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda, **Amazon Web Services (AWS)** yazın, sonuç panelinden Amazon Web Services **(AWS)** seçeneğini seçin ve uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![Amazon Web Services (AWS) sonuç listesinde](common/search-new-app.png)

5. Uygulama eklendikten sonra **Özellikler** sayfasına gidin ve **Nesne Kimliğini**kopyalayın.

    ![Amazon Web Services (AWS) sonuç listesinde](./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_properties.png)

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

4. Temel **SAML Yapılandırması** bölümünde, uygulama Azure ile önceden entegre edilmiş olduğundan, kullanıcının herhangi bir adım gerçekleştirmesi gerekmez.

    ![image](common/preintegrated.png)

5. Amazon Web Services (AWS) uygulaması belirli bir biçimde SAML iddiaları bekliyor. Bu uygulama için aşağıdaki talepleri yapılandırın. Bu özniteliklerin değerlerini, uygulama tümleştirme sayfasındaki **Kullanıcı Öznitelikleri & Talepler** bölümünden yönetebilirsiniz. **SAML sayfasıyla Tek Oturum** Açma'da, **Kullanıcı Öznitelikleri & Talepler** iletişim kutusunu açmak için **Edit** düğmesini tıklatın.

    ![image](common/edit-attribute.png)

6. **Kullanıcı Öznitelikleri** iletişim kutusundaki **Kullanıcı Talepleri** bölümünde, saml belirteç özniteliğini yukarıdaki resimde gösterildiği gibi yapılandırın ve aşağıdaki adımları gerçekleştirin:

    | Adı  | Kaynak Özniteliği  | Ad Alanı |
    | --------------- | --------------- | --------------- |
    | RoleSessionName | user.userprincipalname | https://aws.amazon.com/SAML/Attributes |
    | Rol            | user.assignedroles |  https://aws.amazon.com/SAML/Attributes |
    | Oturum Süresi             | "900 saniye (15 dakika) ile 43200 saniye (12 saat) arasında bir değer sağlar" |  https://aws.amazon.com/SAML/Attributes |

    a. **Kullanıcı taleplerini yönet** iletişim kutusunu açmak için yeni **talep ekle'yi** tıklatın.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. **Ad** metin kutusunda, bu satır için gösterilen öznitelik adını yazın.

    c. Ad **Alanı** metin kutusunda, bu satır için gösterilen Ad Alanı değerini yazın.

    d. **Kaynak'ı Öznitelik**olarak seçin.

    e. Kaynak **öznitelik** listesinden, bu satır için gösterilen öznitelik değerini yazın.

    f. **Tamam'ı** tıklatın

    g. **Kaydet**'e tıklayın.

7. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, Federasyon **Metadata XML'i** indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

### <a name="configure-amazon-web-services-aws-single-sign-on"></a>Yapılandırma Amazon Web Services (AWS) Tek Oturum Açma

1. Farklı bir tarayıcı penceresinde, Amazon Web Services (AWS) şirket sitenizi yönetici olarak oturum açın.

2. **AWS Ana Sayfa'yı**tıklatın.

    ![Tek Oturum Açma ev yapılandırma][11]

3. **Kimlik ve Erişim Yönetimi'ni**tıklatın.

    ![Tek Oturum Açma Kimliğini Yapılandırma][12]

4. **Kimlik Sağlayıcıları'nı**tıklatın ve ardından **Sağlayıcı Oluştur'u**tıklatın.

    ![Tek Oturum Açma Sağlayıcısını Yapılandırma][13]

5. **Sağlayıcıyı Yapılandırıyorum** iletişim sayfasında aşağıdaki adımları gerçekleştirin:

    ![Tek Oturum Açma iletişim kutusunu yapılandırma][14]

    a. **Sağlayıcı Türü**olarak, **SAML'yi**seçin.

    b. Sağlayıcı **Adı** metin kutusunda bir sağlayıcı adı yazın (örneğin: *WAAD).*

    c. İndirdiğiniz meta **veri dosyanızı** Azure portalından yüklemek için **Dosya yı seç'i**tıklatın.

    d. **Sonraki Adım'ı**tıklatın.

6. Sağlayıcı **Bilgilerini Doğrula** iletişim sayfasında **Oluştur'u**tıklatın.

    ![Tek İşaret-On Doğrula'yı Yapılandır][15]

7. **Roller'i**tıklatın ve ardından **Rol Oluştur'u**tıklatın.

    ![Tek Oturum Açma Rollerini Yapılandırma][16]

8. Rol **Oluştur** sayfasında aşağıdaki adımları gerçekleştirin:  

    ![Tek Oturum Açma Güvenini Yapılandırma][19]

    a. **Güvenilir varlık türü**altında **SAML 2.0 federasyonu** seçin.

    b. **SAML 2.0 Sağlayıcı bölümünü seçin,** daha önce oluşturduğunuz **SAML sağlayıcısını** seçin (örneğin: *WAAD*)

    c. **Programatik ve AWS Yönetim Konsoluna Erişime İzin Ver'i**seçin.
  
    d. **Sonraki'yi tıklatın: İzinler**.

9. İzin **İlkeleri Ekle** iletişim kutusunda, lütfen kuruluşunuz için uygun ilke ekleyin. **Sonraki'yi tıklatın: Gözden Geçirin.**  

    ![Tek Oturum Açma İlkesi'ni Yapılandır][33]

10. Gözden **Geçirme** iletişim kutusunda aşağıdaki adımları gerçekleştirin:

    ![Tek İşaretLi İncelemeyi Yapılandır][34]

    a. Rol **adı** metin kutusuna Rol adınızı girin.

    b. Rol **açıklaması** textbox'ına açıklamayı girin.

    c. **Rol Oluştur'u**tıklatın.

    d. Gerektiği kadar rol oluşturun ve bunları Kimlik Sağlayıcısı ile eşleyin.

11. Geçerli AWS hesabından oturum açın ve Azure AD ile tek oturum açmak istediğiniz diğer hesapla oturum açın.

12. Bu hesap için kurmak istediğiniz birden çok rol oluşturmak için adım-2'den adım 10'a gerçekleştirin. İkiden fazla hesabınız varsa, lütfen tüm hesapların onlar için rol oluşturması için aynı adımları gerçekleştirin.

13. Tüm roller hesaplarda oluşturulduktan sonra, bu hesapların **Roller** listesinde gösterirler.

    ![Roller kurulumu](./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_listofroles.png)

14. Azure AD uygulamasıyla el ile haritalamamız gereken tüm hesaplardaki tüm roller için tüm Role ARN ve Trusted Ntities'ı yakalamamız gerekir.

15. **Role ARN** ve **Trusted Ntities** values değerlerini kopyalamak için rolleri tıklatın. Azure AD'de oluşturmanız gereken tüm roller için bu değerlere ihtiyacınız var.

    ![Roller kurulumu](./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_role_summary.png)

16. Tüm hesaplardaki tüm roller için yukarıdaki adımı gerçekleştirin ve bunların hepsini **Role ARN,Trusted varlıklar** biçiminde bir not defterinde saklayın.

17. [Microsoft Graph Explorer'ı](https://developer.microsoft.com/graph/graph-explorer) başka bir pencerede açın.

    a. Kiracınız için Global Admin/Co-admin kimlik bilgilerini kullanarak Graph Explorer sitesinde oturum açın.

    b. Rolleri oluşturmak için yeterli izinlere sahip olmanız gerekir. Gerekli **izinleri** almak için izinleri değiştir'i tıklatın.

    ![Grafik gezgini iletişim kutusu](./media/aws-multi-accounts-tutorial/graph-explorer-new9.png)

    c. Listeden aşağıdaki izinleri seçin (bunlar zaten yoksa) ve "İzinleri Değiştir"i tıklayın 

    ![Grafik gezgini iletişim kutusu](./media/aws-multi-accounts-tutorial/graph-explorer-new10.png)

    d. Bu, tekrar oturum açmanızı ve onayı kabul etmenizi isteyecektir. Onayı kabul ettikten sonra, Grafik Gezgini'ne yeniden giriş yapmış olursunuz.

    e. Sürüm açılır sürümünü **beta**olarak değiştirin. Tüm Hizmet Müdürlerini kiracınızdan almak için aşağıdaki sorguyu kullanın:

    `https://graph.microsoft.com/beta/servicePrincipals`

    Birden çok dizin kullanıyorsanız, birincil etki alanınız içinde bulunan aşağıdaki deseni kullanabilirsiniz`https://graph.microsoft.com/beta/contoso.com/servicePrincipals`

    ![Grafik gezgini iletişim kutusu](./media/aws-multi-accounts-tutorial/graph-explorer-new1.png)

    f. Getirilen Hizmet Müdürleri listesinden değiştirmeniz gerekeni alın. Ayrıca, listelenen tüm ServicePrincipals'dan uygulamayı aramak için Ctrl+F'yi de kullanabilirsiniz. İlgili Hizmet Sorumlusuna ulaşmak için Azure AD Özellikleri sayfasından kopyalamış olduğunuz **Object id'i** kullanarak sorguyu aşağıdaki leri kullanabilirsiniz.

    `https://graph.microsoft.com/beta/servicePrincipals/<objectID>`.

    ![Grafik gezgini iletişim kutusu](./media/aws-multi-accounts-tutorial/graph-explorer-new2.png)

    g. Hizmet ana nesnesinden appRoles özelliğini ayıklayın.

    ![Grafik gezgini iletişim kutusu](./media/aws-multi-accounts-tutorial/graph-explorer-new3.png)

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

    j. Microsoft Graph Explorer'a geri dön ve yöntemi **GET'den** **PATCH'e**değiştirin. Örnekte yukarıda gösterilene benzer appRoles özelliğini güncelleyerek Hizmet Müdürü nesnesini istenen rollere sahip olacak şekilde yama. Yama işlemini yürütmek için **Sorguyu Çalıştır'ı** tıklatın. Bir başarı mesajı Amazon Web Hizmetleri uygulamanız için rolün oluşturulmasını onaylar.

    ![Microsoft Graph explorer iletişim kutusu](./media/aws-multi-accounts-tutorial/graph-explorer-new11.png)

18. Hizmet Sorumlusu daha fazla rolle yamalı hale geldikten sonra, Kullanıcıları/Grupları ilgili rollere atayabilirsiniz. Bu portal a gidip Amazon Web Services uygulamasına gezinerek yapılabilir. Üstteki **Kullanıcılar ve Gruplar** sekmesine tıklayın.

19. Söz konusu gruptaki belirli rolü atayabilmeniz için her AWS rolü için yeni gruplar oluşturmanızı öneririz. Bunun bir gruba bir rol için bire bir eşleme olduğunu unutmayın. Daha sonra bu gruba ait üyeleri ekleyebilirsiniz.

20. Gruplar oluşturulduktan sonra grubu seçin ve uygulamaya atayın.

    ![Tek İşaret Eklemeyi Yapılandır](./media/aws-multi-accounts-tutorial/graph-explorer-new5.png)

    > [!Note]
    > İç içe gruplar grup atarken desteklenmez.

21. Rolü gruba atamak için rolü seçin ve sayfanın altındaki **Atla düğmesini** tıklatın.

    ![Tek İşaret Eklemeyi Yapılandır](./media/aws-multi-accounts-tutorial/graph-explorer-new6.png)

    > [!Note]
    > Yeni roller görmek için Azure portalındaki oturumunuzu yenilemeniz gerektiğini lütfen unutmayın.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki Amazon Web Hizmetleri (AWS) döşemesini tıklattığınızda, rolü seçme seçeneğiyle Birlikte Amazon Web Hizmetleri (AWS) uygulama sayfasını almalısınız.

![Tek İşaret Eklemeyi Yapılandır](./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_test_screen.png)

Ayrıca, rollerin talep olarak geçirildiğini görmek için SAML yanıtını da doğrulayabilirsiniz.

![Tek İşaret Eklemeyi Yapılandır](./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_test_saml.png)

Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](../active-directory-saas-access-panel-introduction.md)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

* [Microsoft Graph API'lerini kullanarak sağlama yapılandırma nasıl yapılır?](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-configure-api)
* [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

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
[35]: ./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices_provisioning.png
[34]: ./media/aws-multi-accounts-tutorial/ic7950253.png
[36]: ./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices_securitycredentials.png
[37]: ./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices_securitycredentials_continue.png
[38]: ./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices_createnewaccesskey.png
[39]: ./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices_provisioning_automatic.png
[40]: ./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices_provisioning_testconnection.png
[41]: ./media/aws-multi-accounts-tutorial/
