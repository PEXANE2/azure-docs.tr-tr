---
title: 'Öğretici: birden çok hesaba bağlanmak için Amazon Web Services (AWS) ile Azure Active Directory tümleştirme | Microsoft Docs'
description: Azure AD ve birden çok Amazon Web Services (AWS) hesapları arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
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
ms.sourcegitcommit: f97f086936f2c53f439e12ccace066fca53e8dc3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/15/2020
ms.locfileid: "77368007"
---
# <a name="tutorial-azure-active-directory-integration-with-multiple-amazon-web-services-aws-accounts"></a>Öğretici: birden çok Amazon Web Services (AWS) hesabıyla Azure Active Directory tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) birden çok Amazon Web Services (AWS) hesabıyla nasıl tümleştirileceğini öğreneceksiniz.

Amazon Web Services (AWS) Azure AD ile tümleştirmek aşağıdaki avantajları sağlar:

- Amazon Web Services (AWS) erişimi olan Azure AD 'de denetim yapabilirsiniz.
- Kullanıcılarınızın Azure AD hesaplarıyla Amazon Web Services (AWS) (çoklu oturum açma) ile otomatik olarak oturum açmasını sağlayabilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek istiyorsanız, bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md).

![Sonuçlar listesinde Amazon Web Services (AWS)](./media/aws-multi-accounts-tutorial/amazonwebservice.png)

>[!NOTE]
>Lütfen bir AWS uygulamasının tüm AWS hesaplarınıza bağlanması, önerilen yaklaşımımız değildir. Bunun yerine, Azure AD 'de AWS hesabının birden çok örneğini birden çok AWS uygulamasının örneği ile yapılandırmak için [Bu](https://docs.microsoft.com/azure/active-directory/saas-apps/amazon-web-service-tutorial) yaklaşımı kullanmanızı öneririz.

**Bu yaklaşımı aşağıdaki nedenlerle kullanmayı önermiyoruz.**

* Tüm rollerin uygulamaya yama yapmak için Microsoft Graph Explorer yaklaşımını kullanmanız gerekir. Bildirim dosyası yaklaşımını kullanmanızı önermiyoruz.

* Tek bir AWS uygulaması için ~ 1200 uygulama rolü ekledikten sonra bu müşterilerin bildirdiği müşterileri gördük, uygulamadaki tüm işlemler boyutla ilgili hataları oluşturulmasına başladı. Uygulama nesnesinde sabit bir boyut sınırı vardır.

* Rol, bir değiştirme yaklaşımı olan ve sonuna eklenmemiş olan hesaplara eklenen roller için el ile güncelleştirmeniz gerekir. Ayrıca hesaplarınız büyüdükçe bu, hesaplar ve rollerle n x n ilişki haline gelir.

* Tüm AWS hesapları aynı Federasyon meta veri XML dosyasını kullanacak ve sertifika geçişi sırasında, tüm AWS hesaplarında sertifikayı aynı anda güncelleştirmek için bu çok büyük bir Alıştırmayı kullanmanız gerekir

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesini Amazon Web Services (AWS) ile yapılandırmak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Bir Azure AD ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü edinebilirsiniz
* Amazon Web Services (AWS) çoklu oturum açma etkin aboneliği

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Bir Azure AD deneme ortamınız yoksa, bir [aylık deneme sürümü](https://azure.microsoft.com/pricing/free-trial/)alabilirsiniz.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açmayı bir test ortamında yapılandırıp test edersiniz.

* Amazon Web Services (AWS) **, SP ve ıDP** tarafından başlatılan SSO 'yu destekler

## <a name="adding-amazon-web-services-aws-from-the-gallery"></a>Galeriden Amazon Web Services (AWS) ekleme

Amazon Web Services (AWS) tümleştirmesini Azure AD ile yapılandırmak için, Galeriden yönetilen SaaS uygulamaları listenize Amazon Web Services (AWS) eklemeniz gerekir.

**Galeriden Amazon Web Services (AWS) eklemek için aşağıdaki adımları uygulayın:**

1. **[Azure Portal](https://portal.azure.com)** sol gezinti panelinde **Azure Active Directory** simgesine tıklayın.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar** seçeneğini belirleyin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için, iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesine tıklayın.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Amazon Web Services (AWS)** yazın, sonuç panelinden **Amazon Web Services (AWS)** öğesini seçin ve ardından **Ekle** düğmesine tıklayarak uygulamayı ekleyin.

     ![Sonuçlar listesinde Amazon Web Services (AWS)](common/search-new-app.png)

5. Uygulama eklendikten sonra **Özellikler** sayfasına gidin ve **nesne kimliğini**kopyalayın.

    ![Sonuçlar listesinde Amazon Web Services (AWS)](./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_properties.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, Azure AD çoklu oturum açmayı, "Britta Simon" adlı bir test kullanıcısına göre Amazon Web Services (AWS) ile yapılandırıp test edersiniz.

Çoklu oturum açma 'nın çalışması için, Azure AD 'nin Amazon Web Services (AWS) içindeki karşılık gelen kullanıcının Azure AD 'deki bir kullanıcıya ne olduğunu bilmeleri gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve Amazon Web Services (AWS) içindeki ilgili Kullanıcı arasındaki bir bağlantı ilişkisinin kurulması gerekir.

Amazon Web Services (AWS) ' **de, bağlantı** ilişkisini oluşturmak için **Kullanıcı adının** değeri olarak Azure AD 'de Kullanıcı adı değerini atayın.

Amazon Web Services (AWS) ile Azure AD çoklu oturum açmayı yapılandırmak ve test etmek için aşağıdaki yapı taşlarını gerçekleştirmeniz gerekir:

1. **[Azure AD çoklu oturum açma özelliğini yapılandırarak](#configure-azure-ad-single-sign-on)** kullanıcılarınızın bu özelliği kullanmasına olanak sağlayın.
2. Uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için **[Amazon Web Services (AWS) çoklu oturum açmayı yapılandırın](#configure-amazon-web-services-aws-single-sign-on)** .
3. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[Çoklu oturum açmayı sınayın](#test-single-sign-on)** .

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure portal Azure AD çoklu oturum açma özelliğini etkinleştirir ve Amazon Web Services (AWS) uygulamanızda çoklu oturum açmayı yapılandırırsınız.

**Azure AD çoklu oturum açmayı Amazon Web Services (AWS) ile yapılandırmak için aşağıdaki adımları uygulayın:**

1. [Azure Portal](https://portal.azure.com/), **Amazon Web Services (AWS)** uygulama tümleştirmesi sayfasında, **Çoklu oturum açma**' yı seçin.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Çoklu oturum **açma yöntemi seç** iletişim kutusunda, çoklu oturum açmayı etkinleştirmek için **SAML/WS-Besme** modunu seçin.

    ![Çoklu oturum açma seçme modu](common/select-saml-option.png)

3. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **temel SAML yapılandırması** Iletişim kutusunu açmak için **Düzenle** simgesine tıklayın.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. **Temel SAML yapılandırması** bölümünde, uygulama zaten Azure ile önceden tümleştirildiği için kullanıcının herhangi bir adım yapması gerekmez.

    ![görüntü](common/preintegrated.png)

5. Amazon Web Services (AWS) uygulaması, belirli bir biçimde SAML onayları bekler. Bu uygulama için aşağıdaki talepleri yapılandırın. Bu özniteliklerin değerlerini, uygulama tümleştirme sayfasındaki **Kullanıcı öznitelikleri & talepler** bölümünde yönetebilirsiniz. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **Kullanıcı öznitelikleri & talepler** Iletişim kutusunu açmak için **Düzenle** düğmesine tıklayın.

    ![görüntü](common/edit-attribute.png)

6. **Kullanıcı öznitelikleri** Iletişim kutusundaki **Kullanıcı talepleri** bölümünde, YUKARıDAKI görüntüde gösterildiği gibi SAML belirteci özniteliğini yapılandırın ve aşağıdaki adımları gerçekleştirin:

    | Adı  | Kaynak özniteliği  | Ad alanı |
    | --------------- | --------------- | --------------- |
    | Roleoturumadı | User. UserPrincipalName | https://aws.amazon.com/SAML/Attributes |
    | Rol            | Kullanıcı. atandroles |  https://aws.amazon.com/SAML/Attributes |
    | SessionDuration             | "900 saniye (15 dakika) ila 43200 saniye (12 saat) arasında bir değer belirtin" |  https://aws.amazon.com/SAML/Attributes |

    a. **Kullanıcı taleplerini Yönet** iletişim kutusunu açmak için **yeni talep Ekle** ' ye tıklayın.

    ![görüntü](common/new-save-attribute.png)

    ![görüntü](common/new-attribute-details.png)

    b. **Ad** metin kutusuna, bu satır için gösterilen öznitelik adını yazın.

    c. **Ad alanı** metin kutusuna, bu satır Için gösterilen ad alanı değerini yazın.

    d. **Öznitelik**olarak kaynak seçin.

    e. **Kaynak özniteliği** listesinde, bu satır için gösterilen öznitelik değerini yazın.

    f. **Tamam 'a** tıklayın

    g. **Kaydet** düğmesine tıklayın.

7. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imza sertifikası** bölümünde, **Federasyon meta veri XML** 'Sini indirmek ve bilgisayarınıza kaydetmek için **İndir** ' e tıklayın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

### <a name="configure-amazon-web-services-aws-single-sign-on"></a>Amazon Web Services (AWS) çoklu oturum açmayı yapılandırma

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

    b. **Sağlayıcı adı** metin kutusuna bir sağlayıcı adı yazın (örneğin: *Waad*).

    c. İndirilen **meta veri dosyanızı** Azure Portal karşıya yüklemek Için **Dosya Seç**' e tıklayın.

    d. **Ileri adım**' a tıklayın.

6. **Sağlayıcı bilgilerini doğrula** Iletişim sayfasında **Oluştur**' a tıklayın.

    ![Çoklu oturum açmayı yapılandırma][15]

7. **Roller**' e tıklayın ve ardından **rol oluştur**' a tıklayın.

    ![Çoklu oturum açma rollerini yapılandırma][16]

8. **Rol oluştur** sayfasında, aşağıdaki adımları uygulayın:  

    ![Çoklu oturum açma güvenini yapılandırma][19]

    a. **Güvenilen varlık seçin**' ın altında **SAML 2,0 Federasyonu** ' ni seçin.

    b. **SAML 2,0 sağlayıcısı Seç bölümünde**, daha önce oluşturduğunuz **SAML sağlayıcısını** seçin (örneğin: *Waad*)

    c. **Programlı ve AWS Yönetim Konsolu erişimine Izin ver '** i seçin.
  
    d. **İleri**' ye tıklayın.

9. **Izin Ilkeleri Ekle** iletişim kutusunda, lütfen kuruluşunuza göre uygun ilkeyi ekleyin. **İleri**' ye tıklayın.  

    ![Çoklu oturum açma Ilkesini yapılandırma][33]

10. **Gözden geçirme** iletişim kutusunda, aşağıdaki adımları uygulayın:

    ![Çoklu oturum açma Incelemesini yapılandırma][34]

    a. **Rol adı** metin kutusunda, rol adınızı girin.

    b. **Rol açıklaması** metin kutusuna açıklamayı girin.

    c. **Rol oluştur**' a tıklayın.

    d. Gereken sayıda rol oluşturun ve bunları kimlik sağlayıcısına eşleyin.

11. Geçerli AWS hesabından oturumunuzu açın ve Azure AD ile çoklu oturum açmayı yapılandırmak istediğiniz diğer hesapla oturum açın.

12. Bu hesap için kurmak istediğiniz birden çok rol oluşturmak için adım 2 ' ye kadar adım-10 gerçekleştirin. İkiden fazla hesabınız varsa, bu roller için rol oluşturmak üzere tüm hesaplara yönelik aynı adımları gerçekleştirin.

13. Hesaplarda tüm roller oluşturulduktan sonra, bu hesaplar için **Roller** listesinde görünür.

    ![Rol kurulumu](./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_listofroles.png)

14. Tüm hesapların tüm rolleri için tüm rol ARN ve güvenilen varlıklarını, Azure AD uygulamasıyla el ile eşlemeniz gereken tüm roller için yakalamamız gerekir.

15. **Rol ARN** ve **güvenilen varlıklar** değerlerini kopyalamak için rollere tıklayın. Azure AD 'de oluşturmanız gereken tüm roller için bu değerlere ihtiyacınız vardır.

    ![Rol kurulumu](./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_role_summary.png)

16. Tüm hesaplardaki tüm roller için yukarıdaki adımı gerçekleştirin ve bunların tümünü bir not defteri içinde **rol ARN, güvenilen varlıklar** biçiminde depolayın.

17. [Microsoft Graph Gezginini](https://developer.microsoft.com/graph/graph-explorer) başka bir pencerede açın.

    a. Kiracınız için genel yönetici/ortak yönetici kimlik bilgilerini kullanarak Graph Explorer sitesinde oturum açın.

    b. Rolleri oluşturmak için yeterli izinlere sahip olmanız gerekir. Gerekli izinleri almak için **izinleri değiştir** ' e tıklayın.

    ![Graph Explorer iletişim kutusu](./media/aws-multi-accounts-tutorial/graph-explorer-new9.png)

    c. Listeden aşağıdaki izinleri seçin (henüz yoksa) ve "Izinleri Değiştir" e tıklayın 

    ![Graph Explorer iletişim kutusu](./media/aws-multi-accounts-tutorial/graph-explorer-new10.png)

    d. Bu, yeniden oturum açıp onayı kabul etmenizi ister. Onayı kabul ettikten sonra, Graph Explorer 'da yeniden oturum açarsınız.

    e. Sürüm açılan menüsünü **Beta**olarak değiştirin. Kiracınızdaki tüm hizmet sorumlularını getirmek için aşağıdaki sorguyu kullanın:

    `https://graph.microsoft.com/beta/servicePrincipals`

    Birden çok dizin kullanıyorsanız, üzerinde birincil etki alanınız olan aşağıdaki kalıbı kullanabilirsiniz `https://graph.microsoft.com/beta/contoso.com/servicePrincipals`

    ![Graph Explorer iletişim kutusu](./media/aws-multi-accounts-tutorial/graph-explorer-new1.png)

    f. Getirilen hizmet sorumluları listesinden, değiştirmeniz gereken bir tane alın. Ayrıca, uygulamayı listelenen tüm hizmet sorumlularından aramak için CTRL + F ' i de kullanabilirsiniz. İlgili hizmet sorumlusuna ulaşmak için Azure AD Özellikleri sayfasından kopyaladığınız **nesne kimliğini** kullanarak aşağıdaki sorguyu kullanabilirsiniz.

    `https://graph.microsoft.com/beta/servicePrincipals/<objectID>`.

    ![Graph Explorer iletişim kutusu](./media/aws-multi-accounts-tutorial/graph-explorer-new2.png)

    g. Hizmet sorumlusu nesnesinden appRoles özelliğini ayıklayın.

    ![Graph Explorer iletişim kutusu](./media/aws-multi-accounts-tutorial/graph-explorer-new3.png)

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

    j. Microsoft Graph Explorer 'a geri dönün ve yöntemi **Al** 'dan **Patch**öğesine değiştirin. Örnek bölümünde gösterilenle benzer appRoles özelliğini güncelleştirerek istenen rollere sahip olmak için hizmet sorumlusu nesnesine yama yapın. Düzeltme Eki işlemini yürütmek için **Sorguyu Çalıştır** ' a tıklayın. Başarılı iletisi, Amazon Web Services uygulamanız için rolün oluşturulmasını onaylar.

    ![Microsoft Graph Gezgini iletişim kutusu](./media/aws-multi-accounts-tutorial/graph-explorer-new11.png)

18. Hizmet sorumlusu daha fazla rolle düzeltme eki uygulandıktan sonra, ilgili rollere kullanıcılar/gruplar atayabilirsiniz. Bu, portala gidip Amazon Web Services uygulamasına gidilerek yapılabilir. Üstteki **Kullanıcılar ve gruplar** sekmesine tıklayın.

19. Bu gruptaki belirli bir rolü atayabilmeniz için her bir AWS rolü için yeni gruplar oluşturmanız önerilir. Bu, bir grup için tek bir rolün tek bir eşlemesine bir eşleme olduğunu unutmayın. Daha sonra, o gruba ait olan üyeleri ekleyebilirsiniz.

20. Gruplar oluşturulduktan sonra, grubu seçin ve uygulamaya atayın.

    ![Çoklu oturum açma eklemeyi yapılandırma](./media/aws-multi-accounts-tutorial/graph-explorer-new5.png)

    > [!Note]
    > Grupları atarken iç içe gruplar desteklenmez.

21. Rolü gruba atamak için, rolü seçin ve sayfanın altındaki **ata** düğmesine tıklayın.

    ![Çoklu oturum açma eklemeyi yapılandırma](./media/aws-multi-accounts-tutorial/graph-explorer-new6.png)

    > [!Note]
    > Yeni rolleri görmek için Azure portal oturumunuzu yenilemeniz gerektiğini lütfen unutmayın.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Amazon Web Services (AWS) kutucuğuna tıkladığınızda, rolü seçme seçeneği ile Amazon Web Services (AWS) uygulama sayfası almanız gerekir.

![Çoklu oturum açma eklemeyi yapılandırma](./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_test_screen.png)

Ayrıca, talep olarak geçirilen rolleri görmek için SAML yanıtını doğrulayabilirsiniz.

![Çoklu oturum açma eklemeyi yapılandırma](./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_test_saml.png)

Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](../active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Microsoft Graph API 'Leri kullanarak sağlamayı yapılandırma](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-configure-api)
* [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
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
