---
title: Kurumsal Azure AD uygulamaları için rol iddiasını yapılandırma | Azure
titleSuffix: Microsoft identity platform
description: Azure Etkin Dizini'ndeki kurumsal uygulamalar için SAML belirtecinde verilen rol iddiasını nasıl yapılandırabilirsiniz öğrenin
services: active-directory
author: jeevansd
manager: CelesteDG
ms.assetid: eb2b3741-3cde-45c8-b639-a636f3df3b74
ms.service: active-directory
ms.subservice: develop
ms.custom: aaddev
ms.workload: identity
ms.topic: conceptual
ms.date: 04/22/2019
ms.author: jeedes
ms.openlocfilehash: a70abd1cddb866037926bbbc881682d50599366b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76699266"
---
# <a name="how-to-configure-the-role-claim-issued-in-the-saml-token-for-enterprise-applications"></a>Nasıl verilir: Kurumsal uygulamalar için SAML belirteci verilen rol talebi yapılandırma

Azure Etkin Dizini 'ni (Azure AD) kullanarak, bir uygulamayı yetkilendirmeden sonra aldığınız yanıt jetonundaki rol talebi için talep türünü özelleştirebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

- Dizin kurulumu na sahip bir Azure AD aboneliği.
- Tek oturum açma (SSO) etkinleştirilmiş bir abonelik. SSO'yu uygulamanızla yapılandırmanız gerekir.

## <a name="when-to-use-this-feature"></a>Bu özelliğin ne zaman kullanılacağı

Uygulamanız özel rollerin SAML yanıtında geçirilmesini bekliyorsa, bu özelliği kullanmanız gerekir. Azure AD'den uygulamanıza geri geçirilmeniz için gereken sayıda rol oluşturabilirsiniz.

## <a name="create-roles-for-an-application"></a>Uygulama için roller oluşturma

1. Azure [portalında](https://portal.azure.com), sol bölmede, **Azure Etkin Dizin** simgesini seçin.

    ![Azure Etkin Dizin simgesi][1]

2. **Kurumsal uygulamaları**seçin. Ardından **Tüm uygulamaları**seçin.

    ![Kurumsal uygulamalar bölmesi][2]

3. Yeni bir uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini seçin.

    !["Yeni uygulama" düğmesi][3]

4. Arama kutusuna, uygulamanızın adını yazın ve sonuç panelinden uygulamanızı seçin. Uygulamayı eklemek için **Ekle** düğmesini seçin.

    ![Sonuç listesindeki uygulama](./media/active-directory-enterprise-app-role-management/tutorial_app_addfromgallery.png)

5. Uygulama eklendikten sonra **Özellikler** sayfasına gidin ve nesne kimliğini kopyalayın.

    ![Özellikler Sayfası](./media/active-directory-enterprise-app-role-management/tutorial_app_properties.png)

6. [Microsoft Graph Explorer'ı](https://developer.microsoft.com/graph/graph-explorer) başka bir pencerede açın ve aşağıdaki adımları izleyin:

    a. Kiracınız için genel yönetici veya coadmin kimlik bilgilerini kullanarak Graph Explorer sitesinde oturum açın.

    b. Rolleri oluşturmak için yeterli izinlere ihtiyacınız vardır. İzinleri almak için **izinleri değiştir'i** seçin.

      !["İzinleri değiştir" düğmesi](./media/active-directory-enterprise-app-role-management/graph-explorer-new9.png)

    c. Listeden aşağıdaki izinleri seçin (bunlar zaten yoksa) ve **İzinleri Değiştir'i**seçin.

      ![İzinlistesi ve "İzinleri Değiştir" düğmesi](./media/active-directory-enterprise-app-role-management/graph-explorer-new10.png)

    > [!Note]
    > Dizin Okuma ve Yazma için Global Yönetici izinlerine ihtiyacımız olduğu için Bulut Uygulaması Yöneticisi ve Uygulama Yöneticisi rolü bu senaryoda çalışmaz.

    d. İzin kabul et. Sisteme tekrar giriş yapmışsın.

    e. Sürümü **beta**olarak değiştirin ve aşağıdaki sorguyu kullanarak hizmet ilkelerinin listesini kiracınızdan getirin:

     `https://graph.microsoft.com/beta/servicePrincipals`

      Birden çok dizin kullanıyorsanız, aşağıdaki deseni izleyin:`https://graph.microsoft.com/beta/contoso.com/servicePrincipals`

      ![Hizmet ilkelerini alma sorgusuyla birlikte Grafik Gezgini iletişim kutusu](./media/active-directory-enterprise-app-role-management/graph-explorer-new1.png)

      > [!Note]
      > Müşterilerin hizmette bazı aksaklıklar görebilmeleri için API'leri yükseltme sürecindeyiz.

    f. Getirilen hizmet ilkeleri listesinden değiştirmeniz gerekeni alın. Ayrıca, listelenen tüm hizmet ilkelerinden uygulamayı aramak için Ctrl+F'yi de kullanabilirsiniz. **Özellikler** sayfasından kopyaladığınız nesne kimliğini arayın ve hizmet ilkesine ulaşmak için aşağıdaki sorguyu kullanın:

      `https://graph.microsoft.com/beta/servicePrincipals/<objectID>`

      ![Değiştirmeniz gereken hizmet ilkesini almak için sorgu](./media/active-directory-enterprise-app-role-management/graph-explorer-new2.png)

    g. Hizmet ana nesnesinden **appRoles** özelliğini ayıklayın.

      ![appRoles özelliğinin ayrıntıları](./media/active-directory-enterprise-app-role-management/graph-explorer-new3.png)

      > [!Note]
      > Özel uygulamayı (Azure Marketi uygulaması değil) kullanıyorsanız, iki varsayılan rol görürsünüz: kullanıcı ve msiam_access. Market uygulaması için msiam_access tek varsayılan roldür. Varsayılan rollerde herhangi bir değişiklik yapmanız gerekmez.

    h. Uygulamanız için yeni roller oluşturun.

      Aşağıdaki JSON **appRoles** nesnesinin bir örneğidir. Uygulamanız için istediğiniz rolleri eklemek için benzer bir nesne oluşturun.

      ```
      {
         "appRoles": [
          {
              "allowedMemberTypes": [
                  "User"
              ],
              "description": "msiam_access",
              "displayName": "msiam_access",
              "id": "b9632174-c057-4f7e-951b-be3adc52bfe6",
              "isEnabled": true,
              "origin": "Application",
              "value": null
          },
          {
              "allowedMemberTypes": [
                  "User"
              ],
              "description": "Administrators Only",
              "displayName": "Admin",
              "id": "4f8f8640-f081-492d-97a0-caf24e9bc134",
              "isEnabled": true,
              "origin": "ServicePrincipal",
              "value": "Administrator"
          }
      ]
      }
      ```

      > [!Note]
      > Yalnızca yama işlemi için msiam_access sonra yeni roller ekleyebilirsiniz. Ayrıca, kuruluşunuzun ihtiyacı kadar rol ekleyebilirsiniz. Azure AD, saml yanıtında talep değeri olarak bu rollerin değerini gönderir. Yeni rollerin kimliği için GUID değerlerini oluşturmak için web araçlarını [bu](https://www.guidgenerator.com/) gibi kullanın

    i. Graph Explorer'a geri dön ve yöntemi **GET'den** **PATCH'e**değiştirin. Önceki örnekte gösterildiği gibi **appRoles** özelliğini güncelleştirerek istenen rollere sahip olmak için hizmet ana nesnesini yama. Yama işlemini yürütmek için **Sorguyu Çalıştır'ı** seçin. Başarı iletisi rolün oluşturulmasını onaylar.

      ![Başarı mesajı ile yama işlemi](./media/active-directory-enterprise-app-role-management/graph-explorer-new11.png)

7. Hizmet ilkesi daha fazla rolle yamalı hale geldikten sonra, kullanıcıları ilgili rollere atayabilirsiniz. Portala giderek ve uygulamaya göz atarak kullanıcıları atayabilirsiniz. Kullanıcılar **ve gruplar** sekmesini seçin. Bu sekme, uygulamaya zaten atanmış tüm kullanıcıları ve grupları listeler. Yeni rollere yeni kullanıcılar ekleyebilirsiniz. Ayrıca varolan bir kullanıcı yı seçebilir ve rolü değiştirmek için **Edit'i** seçebilirsiniz.

    !["Kullanıcılar ve gruplar" sekmesi](./media/active-directory-enterprise-app-role-management/graph-explorer-new5.png)

    Rolü herhangi bir kullanıcıya atamak için yeni rolü seçin ve sayfanın altındaki **Atama** düğmesini seçin.

    !["Atamayı Edit" bölmesi ve "Rol Seç" bölmesi](./media/active-directory-enterprise-app-role-management/graph-explorer-new6.png)

    > [!Note]
    > Yeni roller görmek için oturumunuzu Azure portalında yenilemeniz gerekir.

8. Rol iddiasının özelleştirilmiş eşleştirini tanımlamak için **Öznitelikler** tablosunu yük

9. **Kullanıcı Öznitelikleri** iletişim kutusundaki **Kullanıcı Talepleri** bölümünde, aşağıdaki tabloda gösterildiği gibi SAML belirteç özniteliği eklemek için aşağıdaki adımları gerçekleştirin:

    | Öznitelik adı | Öznitelik değeri |
    | -------------- | ----------------|
    | Rol adı  | user.assignedroles |

    >[!NOTE]
    >Rol talep değeri null ise, Azure AD bu değeri belirteç olarak göndermez ve bu tasarım başına varsayılandır.

    a. **Kullanıcı Öznitelikleri & Talepler** iletişim kutusunu açmak için **Edit** simgesini tıklatın.

      !["Öznitelik ekle" düğmesi](./media/active-directory-enterprise-app-role-management/editattribute.png)

    b. Kullanıcı **taleplerini yönet** iletişim kutusunda, **yeni talep ekle'yi**tıklatarak SAML belirteç özniteliğini ekleyin.

      !["Öznitelik ekle" düğmesi](./media/active-directory-enterprise-app-role-management/tutorial_attribute_04.png)

      !["Öznitelik Ekle" bölmesi](./media/active-directory-enterprise-app-role-management/tutorial_attribute_05.png)

    c. **Ad** kutusuna, gerektiğinde öznitelik adını yazın. Bu örnek, talep adı olarak **Rol Adı** kullanır.

    d. Ad **Alanı** kutusunu boş bırakın.

    e. Kaynak **öznitelik** listesinden, bu satır için gösterilen öznitelik değerini yazın.

    f. **Kaydet'i**seçin.

10. Başvurunuzu bir kimlik sağlayıcısı tarafından başlatılan tek bir oturum açma işleminde test etmek için [Access Panel'de](https://myapps.microsoft.com) oturum açın ve uygulama döşemenizi seçin. SAML belirtecinde, kullanıcı için atanan tüm rolleri, vermiş olduğunuz talep adı yla görmeniz gerekir.

## <a name="update-an-existing-role"></a>Varolan bir rolü güncelleştirme

Varolan bir rolü güncelleştirmek için aşağıdaki adımları gerçekleştirin:

1. [Microsoft Graph Explorer'ı](https://developer.microsoft.com/graph/graph-explorer)açın.

2. Kiracınız için genel yönetici veya coadmin kimlik bilgilerini kullanarak Graph Explorer sitesinde oturum açın.

3. Sürümü **beta**olarak değiştirin ve aşağıdaki sorguyu kullanarak hizmet ilkelerinin listesini kiracınızdan getirin:

    `https://graph.microsoft.com/beta/servicePrincipals`

    Birden çok dizin kullanıyorsanız, aşağıdaki deseni izleyin:`https://graph.microsoft.com/beta/contoso.com/servicePrincipals`

    ![Hizmet ilkelerini alma sorgusuyla birlikte Grafik Gezgini iletişim kutusu](./media/active-directory-enterprise-app-role-management/graph-explorer-new1.png)

4. Getirilen hizmet ilkeleri listesinden değiştirmeniz gerekeni alın. Ayrıca, listelenen tüm hizmet ilkelerinden uygulamayı aramak için Ctrl+F'yi de kullanabilirsiniz. **Özellikler** sayfasından kopyaladığınız nesne kimliğini arayın ve hizmet ilkesine ulaşmak için aşağıdaki sorguyu kullanın:

    `https://graph.microsoft.com/beta/servicePrincipals/<objectID>`

    ![Değiştirmeniz gereken hizmet ilkesini almak için sorgu](./media/active-directory-enterprise-app-role-management/graph-explorer-new2.png)

5. Hizmet ana nesnesinden **appRoles** özelliğini ayıklayın.

    ![appRoles özelliğinin ayrıntıları](./media/active-directory-enterprise-app-role-management/graph-explorer-new3.png)

6. Varolan rolü güncelleştirmek için aşağıdaki adımları kullanın.

    !["Açıklama" ve "displayname" vurgulanmış "PATCH" için istek gövdesi](./media/active-directory-enterprise-app-role-management/graph-explorer-patchupdate.png)

    a. Yöntemi **GET'den** **PATCH'e**değiştirin.

    b. Varolan rolleri kopyalayın ve **İstek Gövdesi**altında yapıştırın.

    c. Gerektiğinde rol açıklamasını, rol değerini veya rol görüntüleme adını güncelleştirerek rolün değerini güncelleştirin.

    d. Gerekli tüm rolleri güncelledikten sonra **Sorguyu Çalıştır'ı**seçin.

## <a name="delete-an-existing-role"></a>Varolan bir rolü silme

Varolan bir rolü silmek için aşağıdaki adımları gerçekleştirin:

1. [Microsoft Graph Explorer'ı](https://developer.microsoft.com/graph/graph-explorer) başka bir pencerede açın.

2. Kiracınız için genel yönetici veya coadmin kimlik bilgilerini kullanarak Graph Explorer sitesinde oturum açın.

3. Sürümü **beta**olarak değiştirin ve aşağıdaki sorguyu kullanarak hizmet ilkelerinin listesini kiracınızdan getirin:

    `https://graph.microsoft.com/beta/servicePrincipals`

    Birden çok dizin kullanıyorsanız, aşağıdaki deseni izleyin:`https://graph.microsoft.com/beta/contoso.com/servicePrincipals`

    ![Hizmet ilkelerinin listesini almak için sorgu ile Grafik Explorer iletişim kutusu](./media/active-directory-enterprise-app-role-management/graph-explorer-new1.png)

4. Getirilen hizmet ilkeleri listesinden değiştirmeniz gerekeni alın. Ayrıca, listelenen tüm hizmet ilkelerinden uygulamayı aramak için Ctrl+F'yi de kullanabilirsiniz. **Özellikler** sayfasından kopyaladığınız nesne kimliğini arayın ve hizmet ilkesine ulaşmak için aşağıdaki sorguyu kullanın:

    `https://graph.microsoft.com/beta/servicePrincipals/<objectID>`

    ![Değiştirmeniz gereken hizmet ilkesini almak için sorgu](./media/active-directory-enterprise-app-role-management/graph-explorer-new2.png)

5. Hizmet ana nesnesinden **appRoles** özelliğini ayıklayın.

    ![Hizmet ana nesnesinden appRoles özelliğinin ayrıntıları](./media/active-directory-enterprise-app-role-management/graph-explorer-new7.png)

6. Varolan rolü silmek için aşağıdaki adımları kullanın.

    ![IsEnabled yanlış olarak ayarlanmış "PATCH" için istek gövdesi](./media/active-directory-enterprise-app-role-management/graph-explorer-new8.png)

    a. Yöntemi **GET'den** **PATCH'e**değiştirin.

    b. Uygulamadan varolan rolleri kopyalayın ve **İstek Gövdesi**altında yapıştırın.

    c. Silmek istediğiniz rol için **IsEnabled** değerini **false** olarak ayarlayın.

    d. **Sorgu Çalıştır**'ı seçin.

    > [!NOTE]
    > msiam_access rolüne sahip olduğundan ve kimliğin oluşturulan rolde eşleştirdiğinden emin olun.

7. Rol devre dışı bırakıldıktan sonra, bu rol bloğunu **appRoles** bölümünden silin. Yöntemi **YAMA**olarak tutun ve **Sorguyu Çalıştır'ı**seçin.

8. Sorguyu çalıştırdıktan sonra, rol silinir.

    > [!NOTE]
    > Rolün kaldırılabilmesi için devre dışı bırakılması gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Ek adımlar için [uygulama belgelerine](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)bakın.

<!--Image references-->
<!--Image references-->

[1]: ./media/active-directory-enterprise-app-role-management/tutorial_general_01.png
[2]: ./media/active-directory-enterprise-app-role-management/tutorial_general_02.png
[3]: ./media/active-directory-enterprise-app-role-management/tutorial_general_03.png
[4]: ./media/active-directory-enterprise-app-role-management/tutorial_general_04.png
