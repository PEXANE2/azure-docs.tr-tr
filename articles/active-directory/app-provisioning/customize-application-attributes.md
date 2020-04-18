---
title: Azure AD öznitelik eşlemelerini özelleştirme | Microsoft Dokümanlar
description: Azure Active Directory'deki SaaS uygulamaları için hangi öznitelik eşlemelerinin iş gereksinimlerinize yönelik olarak bunları nasıl değiştirebileceğinizi öğrenin.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-provisioning
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/03/2019
ms.author: mimart
ms.collection: M365-identity-device-management
ms.openlocfilehash: bfee19e9cfd1def71ebad82c2210ffc10146c896
ms.sourcegitcommit: d791f8f3261f7019220dd4c2dbd3e9b5a5f0ceaf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2020
ms.locfileid: "81639754"
---
# <a name="customizing-user-provisioning-attribute-mappings-for-saas-applications-in-azure-active-directory"></a>Azure Active Directory'deki SaaS uygulamaları için kullanıcı sağlama öznitelik eşlemelerini özelleştirme

Microsoft Azure AD, Salesforce, G Suite ve diğerleri gibi üçüncü taraf SaaS uygulamalarına kullanıcı sağlama desteği sağlar. Üçüncü taraf bir SaaS uygulaması için kullanıcı sağlamayı etkinleştiriyorsanız, Azure portalı öznitelik eşlemeleri aracılığıyla öznitelik değerlerini denetler.

Azure AD kullanıcı nesneleri ile her SaaS uygulamasının kullanıcı nesneleri arasında önceden yapılandırılmış öznitelikler ve öznitelik eşlemeleri kümesi vardır. Bazı uygulamalar, Kullanıcılar la birlikte Gruplar gibi diğer nesne türlerini yönetir.

Varsayılan öznitelik eşlemelerini işletme gereksinimlerinize göre özelleştirebilirsiniz. Bu nedenle, varolan öznitelik eşlemelerini değiştirebilir veya silebilir veya yeni öznitelik eşlemeleri oluşturabilirsiniz.

## <a name="editing-user-attribute-mappings"></a>Kullanıcı özniteliği eşlemelerini düzenleme

Kullanıcı sağlamanın **Eşlemeler** özelliğine erişmek için aşağıdaki adımları izleyin:

1. Azure Active [Directory portalında](https://aad.portal.azure.com)oturum açın.
1. Sol bölmeden **Enterprise uygulamalarını** seçin. Galeriden eklenen uygulamalar da dahil olmak üzere, yapılandırılan tüm uygulamaların listesi gösterilir.
1. Raporları görüntüleyebileceğiniz ve uygulama ayarlarını yönetebileceğiniz uygulama yönetimi bölmesini yüklemek için herhangi bir uygulamayı seçin.
1. Seçili uygulama için kullanıcı hesabı sağlama ayarlarını yönetmek için **Sağlama'yı** seçin.
1. Azure AD ile hedef uygulama arasında akan kullanıcı özniteliklerini görüntülemek ve bunlarla görüntülemek için **Eşlemeleri** genişletin. Hedef uygulama destekliyorsa, bu bölüm isteğe bağlı olarak grupların ve kullanıcı hesaplarının sağlanmasını yapılandırmanızı sağlar.

   ![Kullanıcı özniteliklerini görüntülemek ve bunları yeniden kullanmak için Eşlemeleri kullanma](./media/customize-application-attributes/21.png)

1. İlgili **Öznitelik Eşleme** ekranını açmak için **Eşlemeyapılandırmasını** seçin. Bazı öznitelik eşlemeleri bir SaaS uygulaması tarafından doğru çalışması için gereklidir. Gerekli öznitelikler için **Sil** özelliği kullanılamıyor.

   ![Uygulamalar için öznitelik eşlemelerini yapılandırmak için Öznitelik Eşleme'yi kullanma](./media/customize-application-attributes/22.png)

   Bu ekran görüntüsünde, Salesforce'ta yönetilen bir nesnenin **Kullanıcı Adı** özniteliğinin, bağlı Azure Etkin Dizin Nesnesinin **kullanıcıAdı** adı değeriyle dolduruldugını görebilirsiniz.

1. **Atnitelik Atfetme** ekranını açmak için varolan bir **Öznitelik Eşlemi'ni** seçin. Burada Azure AD ile hedef uygulama arasında akan kullanıcı özniteliklerini edinebilirsiniz.

   ![Kullanıcı özniteliklerini yeniden güncellemek için Özniteliği Ediyi kullanma](./media/customize-application-attributes/23.png)

### <a name="understanding-attribute-mapping-types"></a>Öznitelik-eşleme türlerini anlama

Öznitelik eşlemeleri ile özniteliklerin üçüncü taraf bir SaaS uygulamasında nasıl doldurulur denetlersiniz.
Desteklenen dört farklı eşleme türü vardır:

- **Doğrudan** – hedef öznitelik, Azure AD'de bağlı nesnenin özniteliğinin değeriyle doldurulur.
- **Sabit** – hedef özniteliği belirttiğiniz belirli bir dize ile doldurulur.
- **İfade** - hedef özniteliği, komut dosyası benzeri bir ifadenin sonucuna göre doldurulur.
  Daha fazla bilgi için bkz: [Azure Etkin Dizini'nde Öznitelik-Eşlemeler için İfadeler Yazma.](../app-provisioning/functions-for-customizing-application-data.md)
- **Yok** - hedef özniteliği değiştirilmemiş bırakılır. Ancak, hedef öznitelik hiç boşsa, belirttiğiniz Varsayılan değerle doldurulur.

Bu dört temel türle birlikte, özel öznitelik eşlemeleri isteğe bağlı **varsayılan** değer ataması kavramını destekler. Varsayılan değer ataması, Azure AD'de veya hedef nesnede bir değer yoksa, hedef özniteliğinin bir değerle doldurulmasını sağlar. En yaygın yapılandırma bu boş bırakmaktır.

### <a name="understanding-attribute-mapping-properties"></a>Öznitelik eşleme özelliklerini anlama

Önceki bölümde, öznitelik eşleme türü özelliği ne tanıtıldınız.
Bu özelliğin yanı sıra, öznitelik eşlemeleri de aşağıdaki öznitelikleri destekler:

- **Kaynak özniteliği** - Kaynak sistemden kullanıcı özniteliği (örnek: Azure Etkin Dizini).
- **Hedef öznitelik** – Hedef sistemdeki kullanıcı özniteliği (örnek: ServiceNow).
- **Null (isteğe bağlı) -** Kaynak özniteliği null ise hedef sisteme geçirilecek değer. Bu değer yalnızca bir kullanıcı oluşturulduğunda sağlanacaktır. Varolan bir kullanıcı güncellenirken "null zaman varsayılan değer" sağlanmaz. Örneğin, hedef sistemdeki tüm varolan kullanıcıları belirli bir İş Başlığıyla (kaynak sistemde null olduğunda) sağlamak istiyorsanız, aşağıdaki [ifadeyi](../app-provisioning/functions-for-customizing-application-data.md)kullanabilirsiniz: Switch(IsPresent([jobTitle]), "DefaultValue", "True", [jobTitle]). Kaynak sistemde null olduğunda "Varsayılan Değer"i sağlamak istediğiniz şeyle değiştirdiğinden emin olun. 
- **Bu özniteliği kullanarak nesneleri eşleştirin** – Bu eşlemenin kaynak ve hedef sistemler arasındaki kullanıcıları benzersiz olarak tanımlamak için kullanılıp kullanılmaması gerektiği. Genellikle Azure AD'deki userPrincipalName veya posta özniteliği üzerinde ayarlanır ve bu özellik genellikle hedef bir uygulamada bir kullanıcı adı alanına eşlenir.
- **Eşleşen öncelik** – Birden çok eşleşen öznitelik ayarlanabilir. Birden fazla olduğunda, bu alan tarafından tanımlanan sırada değerlendirilir. Eşleşme bulunur bulunmaz, başka eşleşen öznitelikler değerlendirilmez.
- **Bu eşlemi uygulayın**
  - **Her zaman** – Bu eşlemi hem kullanıcı oluşturma hem de güncelleştirme eylemlerine uygulayın.
  - **Yalnızca oluşturma sırasında** - Bu eşlemi yalnızca kullanıcı oluşturma eylemlerine uygulayın.

## <a name="matching-users-in-the-source-and-target--systems"></a>Kaynak ve hedef sistemlerdeki kullanıcıları eşleştirme
Azure AD sağlama hizmeti hem "yeşil alan" senaryolarında (kullanıcıların hedef sistemde çıkmadığı) hem de "brownfield" senaryolarında (kullanıcıların hedef sistemde zaten var olduğu) dağıtılabilir. Her iki senaryoyu desteklemek için, sağlama hizmeti öznitelikleri eşleştirme kavramını kullanır. Eşleşen öznitelikler, kaynaktaki bir kullanıcıyı nasıl benzersiz bir şekilde tanımlayacağınızı ve hedefteki kullanıcıyla nasıl eşleşip eşleşip eşleştireceklerini belirlemenize olanak sağlar. Dağıtımınızı planlamanın bir parçası olarak, kaynak ve hedef sistemlerdeki bir kullanıcıyı benzersiz olarak tanımlamak için kullanılabilecek özniteliği tanımlayın. Unutulmaması gerekenler:

- **Eşleşen öznitelikler benzersiz olmalıdır:** Müşteriler genellikle kullanıcıPrincipalName, posta veya nesne kimliği gibi öznitelikleri eşleşen öznitelik olarak kullanır.
- **Birden çok öznitelik eşleşen öznitelikleri olarak kullanılabilir:** Kullanıcılarla eşleşirken ve bunların değerlendirildiği sıra (UI'de eşleşen öncelik olarak tanımlanır) değerlendirilecek birden çok özniteliği tanımlayabilirsiniz. Örneğin, üç özniteliği eşleşen öznitelikler olarak tanımlarsanız ve bir kullanıcı ilk iki özniteliği değerlendirdikten sonra benzersiz bir şekilde eşleşirse, hizmet üçüncü özniteliği değerlendirmez. Hizmet, belirtilen sırada eşleşen öznitelikleri değerlendirir ve eşleşme bulunduğunda değerlendirmeyi durdurur.  
- **Kaynaktaki ve hedefteki değerin tam olarak eşleşmesi gerekmez:** Hedefteki değer, kaynaktaki değerin basit bir işlevi olabilir. Bu nedenle, kaynakta bir emailAddress özniteliği ve hedefteki userPrincipalName olabilir ve bazı karakterlerin yerine sabit bir değer le gelen e-postaAdresi özniteliğinin bir işleviile eşleşebilir.  
- **Özniteliklerin birleşimine dayalı eşleştirme desteklenmez:** Çoğu uygulama iki özelliğe dayanarak sorgulamayı desteklemez. Bu nedenle, özniteliklerin bir leşimine göre eşleşmek mümkün değildir. Tek özellikleri birbiri ardına değerlendirmek mümkündür.
- **Tüm kullanıcıların en az bir eşleşen öznitelik için bir değeri olmalıdır:** Eşleşen bir öznitelik tanımlarsanız, tüm kullanıcıların kaynak sistemde bu özniteliğe sahip olması gerekir. Örneğin, userPrincipalName'i eşleşen öznitelik olarak tanımlarsanız, tüm kullanıcıların bir userPrincipalName'e sahip olması gerekir. Birden çok eşleşen öznitelik (örneğin uzantıAttribute1 ve posta) tanımlarsanız, tüm kullanıcıların aynı eşleşen özniteliğe sahip olması gerekmez. Başka bir kullanıcı posta olabilir ama hiçbir uzantısıAttribute1 iken bir kullanıcı bir uzantısıAttribute1 olabilir. 
- **Hedef uygulama eşleşen öznitelik üzerinde filtreleme desteklemesi gerekir:** Uygulama geliştiricileri, kullanıcı veya grup API'lerindeki özniteliklerin bir alt kümesi için filtreleme sağlar. Galerideki uygulamalar için, varsayılan öznitelik eşlemenin hedef uygulamanın API'sinin filtrelemesini desteklediği bir öznitelik için olduğundan emin oluruz. Hedef uygulama için varsayılan eşleme özniteliğini değiştirirken, özniteliğin filtreuygulanabileceğinden emin olmak için üçüncü taraf API belgelerini denetleyin.  

## <a name="editing-group-attribute-mappings"></a>Grup öznitelik eşlemelerini düzenleme

ServiceNow, Box ve G Suite gibi seçili sayıda uygulama, Grup nesnelerini ve Kullanıcı nesnelerini sağlama yeteneğini destekler. Grup nesneleri, grup üyeleriyle birlikte görüntü adları ve e-posta takma adları gibi grup özellikleri içerebilir.

![Örnek, serviceNow'u sağlanan Grup ve Kullanıcı nesneleri ile gösterir](./media/customize-application-attributes/24.png)

Grup sağlama, **Eşlemeler**altında grup eşleciliğini seçerek ve **Atrektif Eşleme** ekranında istediğiniz seçeneğe **Etkinleştirilerek** isteğe bağlı olarak etkinleştirilebilir veya devre dışı edilebilir.

Grup nesnelerinin bir parçası olarak sağlanan öznitelikler, daha önce açıklanan Kullanıcı nesneleriyle aynı şekilde özelleştirilebilir. 

> [!TIP]
> Grup nesnelerinin (özellikler ve üyeler) sağlanması, bir uygulamaya [grup atamaktan](../manage-apps/assign-user-or-group-access-portal.md) farklı bir kavramdır. Bir uygulamaya bir grup atamak mümkündür, ancak yalnızca grupta bulunan kullanıcı nesnelerini sağlar. Tam grup nesnelerinsağlanması atamalarında grupları kullanmak için gerekli değildir.

## <a name="editing-the-list-of-supported-attributes"></a>Desteklenen özniteliklerlistesini düzenleme

Belirli bir uygulama için desteklenen kullanıcı öznitelikleri önceden yapılandırılmıştır. Çoğu uygulamanın kullanıcı yönetimi API'leri şema keşfini desteklemez. Bu nedenle, Azure AD sağlama hizmeti, uygulamaya çağrı lar yaparak desteklenen öznitelikler listesini dinamik olarak oluşturamaz.

Ancak, bazı uygulamalar özel öznitelikleri destekler ve Azure AD sağlama hizmeti özel öznitelikleri okuyabilir ve yazabilir. Tanımlarını Azure portalına girmek için, **Attribute Mapping** ekranının altındaki **Gelişmiş seçenekleri göster** onay kutusunu seçin ve ardından uygulamanız için **öznitelik listesini edin'i** seçin.

Öznitelik listesinin özelleştirilmesini destekleyen uygulamalar ve sistemler şunlardır:

- Salesforce
- ServiceNow
- Workday
- Azure Active Directory[(Microsoft Graph REST API v1.0 başvuru](https://docs.microsoft.com/graph/api/overview?view=graph-rest-1.0) ve özel dizin uzantıları desteklenir)
- [SCIM 2.0'ı](https://tools.ietf.org/html/rfc7643)destekleyen, temel [şemada](https://tools.ietf.org/html/rfc7643) tanımlanan özniteliklerin eklenmesi gereken uygulamalar

> [!NOTE]
> Desteklenen öznitelikler listesini düzenlemek yalnızca uygulamalarının ve sistemlerinin şemalarını özelleştiren ve özel özniteliklerinin nasıl tanımlandığı hakkında ilk elden bilgiye sahip yöneticiler için önerilir. Bu bazen bir uygulama veya sistem tarafından sağlanan API'ler ve geliştirici araçları ile aşinalık gerektirir.

Desteklenen öznitelikler in listesini düzenlerken aşağıdaki özellikler sağlanır:

- **Ad** - Hedef nesnenin şemasında tanımlandığı gibi özniteliğin sistem adı.
- **Tür** - Aşağıdaki türlerden biri olabilecek hedef nesnenin şemasında tanımlandığı gibi öznitelik depoladığı veri türü:
  - *İkili* - Öznitelik ikili veri içerir.
  - *Boolean* - Öznitelik Doğru veya Yanlış değeri içerir.
  - *DateTime* - Öznitelik bir tarih dizeiçerir.
  - *Sonda* - Öznitelik bir karşıcı içerir.
  - *Başvuru* - Öznitelik, hedef uygulamada başka bir tabloda depolanan bir değere başvuran bir kimlik içerir.
  - *String* - Öznitelik bir metin dizesi içerir.
- **Birincil Anahtar mı?** - Özniteliğin hedef nesnenin şemasında birincil anahtar alan olarak tanımlanıp tanımlanmadığı.
- **Gerekli?** - Özniteliğin hedef uygulamada veya sistemde doldurulması gerekip gerekmediği.
- **Çok değerli mi?** - Özniteliğin birden çok değeri destekleyip desteklemediği.
- **Tam vaka mı?** - Özniteliklerin değerlerinin büyük/küçük harf duyarlı bir şekilde değerlendirilip değerlendirilmediği.
- **API İfadesi** - Belirli bir sağlama bağlayıcısı (İş Günü gibi) için belgeler tarafından talimat edilmedikçe kullanmayın.
- **Başvurulan Nesne Özniteliği** - Başvuru türü özniteliği ise, bu menü, öznitelikile ilişkili değeri içeren hedef uygulamada tabloyu ve özniteliği seçmenize olanak tanır. Örneğin, "Departman" adlı bir özniteseniz ve depolanan değer ayrı bir "Bölümler" tablosunda bir nesneye başvuruyorsa, "Departments.Name" seçeneğini seçersiniz. Başvuru tabloları ve belirli bir uygulama için desteklenen birincil kimlik alanları önceden yapılandırılmıştır ve şu anda Azure portalı kullanılarak düzenlenemez, ancak [Microsoft Graph API](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/synchronization-configure-with-custom-target-attributes)kullanılarak düzenlenebilir.

#### <a name="provisioning-a-custom-extension-attribute-to-a-scim-compliant-application"></a>SCIM uyumlu bir uygulamaya özel bir uzantı özniteliği sağlama
SCIM RFC bir çekirdek kullanıcı ve grup şeması tanımlarken, aynı zamanda uygulamanızın gereksinimlerini karşılamak için şemaya uzantılar için izin verir. SCIM uygulamasına özel bir öznitelik eklemek için:
   1. Azure Active [Directory portalında](https://aad.portal.azure.com)oturum açın, **Kurumsal Uygulamalar'ı**seçin, uygulamanızı seçin ve ardından **Sağlama'yı**seçin.
   2. **Eşlemeler**altında, özel bir öznitelik eklemek istediğiniz nesneyi (kullanıcı veya grup) seçin.
   3. Sayfanın alt kısmında **gelişmiş seçenekleri göster'i**seçin.
   4. **AppName için Öznitelik Listesini Edit'i**seçin.
   5. Öznitelik listesinin en altında, sağlanan alanlara özel öznitelik hakkında bilgi girin. Ardından **Öznitelik Ekle'yi**seçin.

SCIM uygulamaları için öznitelik adı aşağıdaki örnekte gösterilen deseni izlemeli. "CustomExtensionName" ve "CustomAttribute" örneğin, uygulamanızın gereksinimlerine göre özelleştirilebilir:  
 * urn:ietf:params:scim:schemas:extension:CustomExtensionName:2.0:User:CustomAttribute 
 * urn:ietf:params:scim:schemas:extension:2.0:CustomExtensionName:CustomAttribute  
 * urn:ietf:params:scim:schemas:extension:CustomExtensionName:2.0:User.CustomAttributeName:value

Bu yönergeler yalnızca SCIM özellikli uygulamalar için geçerlidir. ServiceNow ve Salesforce gibi uygulamalar SCIM kullanılarak Azure AD ile tümleşik değildir ve bu nedenle özel bir öznitelik eklerken bu özel ad alanını gerektirmezler.

Özel öznitelikler başvurusal öznitelikler veya çok değerli öznitelikler olamaz. Özel çok değerli uzantı öznitelikleri şu anda yalnızca galerideki uygulamalar için desteklenir.  
 
**Uzantı özniteliği olan bir kullanıcının örnek gösterimi:**

```json
   {
     "schemas": ["urn:ietf:params:scim:schemas:core:2.0:User",
      "urn:ietf:params:scim:schemas:extension:enterprise:2.0:User",
      "urn:ietf:params:scim:schemas:extension:CustomExtensionName:2.0:User"],
     "userName":"bjensen",
     "externalId":"bjensen",
     "name":{
       "formatted":"Ms. Barbara J Jensen III",
       "familyName":"Jensen",
       "givenName":"Barbara"
     },
     "urn:ietf:params:scim:schemas:extension:enterprise:2.0:User": {
     "employeeNumber": "701984",
     "costCenter": "4130",
     "organization": "Universal Studios",
     "division": "Theme Park",
     "department": "Tour Operations",
     "manager": {
       "value": "26118915-6090-4610-87e4-49d8ca9f808d",
       "$ref": "../Users/26118915-6090-4610-87e4-49d8ca9f808d",
       "displayName": "John Smith"
     }
   },
     "urn:ietf:params:scim:schemas:extension:CustomExtensionName:2.0:CustomAttribute:User": {
     "CustomAttribute": "701984",
   },
   "meta": {
     "resourceType": "User",
     "created": "2010-01-23T04:56:22Z",
     "lastModified": "2011-05-13T04:42:34Z",
     "version": "W\/\"3694e05e9dff591\"",
     "location":
 "https://example.com/v2/Users/2819c223-7f76-453a-919d-413861904646"
   }
 }
```


## <a name="provisioning-a-role-to-a-scim-app"></a>BIR SCIM uygulamasına rol sağlama
Uygulamanız için bir kullanıcı için rolleri sağlamak için aşağıdaki adımları kullanın. Aşağıdaki açıklamanın özel SCIM uygulamalarına özgü olduğunu unutmayın. Salesforce ve ServiceNow gibi galeri uygulamaları için önceden tanımlanmış rol eşlemelerini kullanın. Aşağıdaki madde işaretleri, Uygulama Atamaları özniteliğinin uygulamanızın beklediği biçime nasıl dönüştürüldüğünü açıklar.

- Azure AD'deki bir uygulamaRol Atamasını uygulamanızdaki bir rolile eşlemeniz, özniteliği bir [ifade](../app-provisioning/functions-for-customizing-application-data.md)kullanarak dönüştürmenizi gerektirir. AppRoleAssignment özniteliği, rol ayrıntılarını ayrışdırmak için bir ifade kullanmadan doğrudan bir rol özniteliğine **eşlenmemelidir.** 

- **SingleAppRoleAssignment** 
  - **Ne zaman kullanılır:** Bir kullanıcı için tek bir rol sağlamak ve birincil rolü belirtmek için SingleAppRoleAssignment ifadesini kullanın. 
  - **Nasıl yapılandırın:** Öznitelik eşlemeleri sayfasına gitmek için yukarıda açıklanan adımları kullanın ve rollerözle eşlemek için SingleAppRoleAssignment ifadesini kullanın. Seçilecek üç rol özniteliği vardır: (roller[birincil eq "True"].display, roles[primary eq "True].type, ve roles[primary eq "True"]].value). Eşlemelerinize rol özniteliklerinden herhangi birini veya tümünün dahil etmeyi seçebilirsiniz. Birden fazla eklemek istiyorsanız, yeni bir eşleme ekleyin ve hedef öznitelik olarak ekleyin.  
  
  ![SingleAppRoleAssignment ekle](./media/customize-application-attributes/edit-attribute-singleapproleassignment.png)
  - **Göz önünde bulundurulması gerekenler**
    - Birden çok rolün bir kullanıcıya atanmadığını sağlayın. Hangi rolün verişolacağını garanti edemeyiz.
    
  - **Örnek çıktı** 

   ```json
    {
      "schemas": [
          "urn:ietf:params:scim:schemas:core:2.0:User"
      ],
      "externalId": "alias",
      "userName": "alias@contoso.OnMicrosoft.com",
      "active": true,
      "displayName": "First Name Last Name",
      "meta": {
           "resourceType": "User"
      },
      "roles": [
         {
               "primary": true,
               "type": "WindowsAzureActiveDirectoryRole",
               "value": "Admin"
         }
      ]
   }
   ```
  
- **AppRoleAssignmentsComplex** 
  - **Ne zaman kullanılır:** Bir kullanıcı için birden çok rol sağlamak için AppRoleAssignmentsComplex ifadesini kullanın. 
  - **Nasıl yapılandırın:** Roller için yeni bir öznitelik içerecek şekilde yukarıda açıklandığı gibi desteklenen özniteliklerin listesini edin: 
  
    ![Rolleri ekleme](./media/customize-application-attributes/add-roles.png)<br>

    Ardından AppRoleAssignmentsComplex ifadesini kullanarak aşağıdaki resimde gösterildiği gibi özel rol özniteliğini eşleyin:

    ![AppRoleAssignmentsComplex ekle](./media/customize-application-attributes/edit-attribute-approleassignmentscomplex.png)<br>
  - **Göz önünde bulundurulması gerekenler**
    - Tüm roller birincil = false olarak sağlanacaktır.
    - POST rol türünü içerir. PATCH isteği türü içermez. Hem POST hem de PATCH isteklerinde türü göndermeye çalışıyoruz.
    
  - **Örnek çıktı** 
  
   ```json
   {
       "schemas": [
           "urn:ietf:params:scim:schemas:core:2.0:User"
      ],
      "externalId": "alias",
      "userName": "alias@contoso.OnMicrosoft.com",
      "active": true,
      "displayName": "First Name Last Name",
      "meta": {
           "resourceType": "User"
      },
      "roles": [
         {
               "primary": false,
               "type": "WindowsAzureActiveDirectoryRole",
               "display": "Admin",
               "value": "Admin"
         },
         {
               "primary": false,
               "type": "WindowsAzureActiveDirectoryRole",
               "display": "User",
             "value": "User"
         }
      ]
   }
   ```

  


## <a name="provisioning-a-multi-value-attribute"></a>Çok değerli bir öznitelik sağlama
phoneNumbers ve e-postalar gibi belirli öznitelikler, farklı telefon numaraları veya e-posta türleri belirtmeniz gerekebileceği çok değerli özniteliklerdir. Çok değerli öznitelikler için aşağıdaki ifadeyi kullanın. Değer için ilgili Azure AD kullanıcı özniteliğine göre öznitelik türünü belirtmenize ve eşlediğinizi belirtmenize olanak tanır. 

* phoneNumbers[yazın eq "iş"].value
* phoneNumbers[eq yazın "mobil"].value
* phoneNumbers[eq yazın "faks"].value

   ```json
   "phoneNumbers": [
       {
         "value": "555-555-5555",
         "type": "work"
      },
      {
         "value": "555-555-5555",
         "type": "mobile"
      },
      {
         "value": "555-555-5555",
         "type": "fax"
      }
   ]
   ```

## <a name="restoring-the-default-attributes-and-attribute-mappings"></a>Varsayılan öznitelikleri ve öznitelik eşlemelerini geri alma

Yeniden başlayıp varolan eşlemelerinizi varsayılan durumlarına sıfırlamanız gerekirse, **varsayılan eşleciliği Geri Yükle** onay kutusunu seçebilir ve yapılandırmayı kaydedebilirsiniz. Bunu yaparken, uygulama uygulama galerisinden Azure AD kiracınıza yeni eklenmiştir gibi tüm eşlemeleri ve kapsam filtreleri ayarlar.

Bu seçeneğin seçilmesi, sağlama hizmeti çalışırken tüm kullanıcıların yeniden senkronize olmasına etkin bir şekilde neden olur.

> [!IMPORTANT]
> Bu seçeneği kapatmadan önce **Sağlama durumunun** **Kapalı** olarak ayarlanmasına şiddetle tavsiye ediyoruz.

## <a name="what-you-should-know"></a>Bilmeniz gerekenler

- Microsoft Azure AD, eşitleme işleminin verimli bir şekilde uygulanmasını sağlar. Başlatılan bir ortamda, yalnızca güncelleştirme gerektiren nesneler eşitleme döngüsü sırasında işlenir.
- Öznitelik eşlemelerinin güncelleştirilmesi, eşitleme döngüsünün performansı üzerinde bir etkiye sahiptir. Öznitelik eşleme yapılandırmasına yapılan bir güncelleştirme, yönetilen tüm nesnelerin yeniden değerlendirilmesini gerektirir.
- Önerilen en iyi yöntem, öznitelik haritalamalarınızda ardışık değişikliklerin sayısını en az düzeyde tutmaktır.
- Bir uygulamaya sağlanacak bir fotoğraf özniteliği eklemek, fotoğrafı eşitlemek için biçimini belirtemediğiniz için bugün desteklenmez. [Kullanıcı Sesi'nde](https://feedback.azure.com/forums/169401-azure-active-directory) bu özelliği talep edebilirsiniz
- Öznitelik IsSoftDeleted genellikle bir uygulama için varsayılan eşlemelerin bir parçasıdır. IsSoftdeleted dört senaryodan birinde doğru olabilir (kullanıcı uygulamadan atanmadı nedeniyle kapsam dışındadır, kullanıcı kapsam dışında dır, kapsam dışıdır, kapsam dışıdır, kullanıcı Azure AD'de yumuşak silinmiş veya AccountEnabled özelliği kullanıcı üzerinde yanlış olarak ayarlanmıştır). 
- Azure AD sağlama hizmeti, null değerleri sağlamayı desteklemez

## <a name="next-steps"></a>Sonraki adımlar

- [SaaS Uygulamalarına Kullanıcı Sağlama/Sağlama sağlama](user-provisioning.md)
- [Öznitelik-Eşlemeler için İfadeler Yazma](../app-provisioning/functions-for-customizing-application-data.md)
- [Kullanıcı Sağlama için Kapsam Filtreleri](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)
- [Kullanıcıların ve grupların Azure Active Directory'den uygulamalara otomatik olarak hazırlanmasını etkinleştirmek için SCIM'yi kullanma](use-scim-to-provision-users-and-groups.md)
- [SaaS Uygulamalarını Tümleştirme Hakkında Öğreticiler Listesi](../saas-apps/tutorial-list.md)
