---
title: Azure AD öznitelik eşlemelerini özelleştirme | Microsoft Docs
description: Azure Active Directory ' deki SaaS uygulamalarına yönelik öznitelik eşleştirmelerinin, iş gereksinimlerinizi karşılamak üzere bunları nasıl değiştirebileceğiniz hakkında bilgi edinin.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/03/2019
ms.author: mimart
ms.collection: M365-identity-device-management
ms.openlocfilehash: ef3d6a47986056925f9964638c9c7192341ca5f9
ms.sourcegitcommit: 824e3d971490b0272e06f2b8b3fe98bbf7bfcb7f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2019
ms.locfileid: "72241001"
---
# <a name="customizing-user-provisioning-attribute-mappings-for-saas-applications-in-azure-active-directory"></a>Kullanıcı hazırlama özniteliğini özelleştirme-Azure Active Directory SaaS uygulamaları için eşlemeler

Microsoft Azure AD Salesforce, G Suite ve diğerleri gibi üçüncü taraf SaaS uygulamalarına Kullanıcı hazırlama desteği sağlar. Bir üçüncü taraf SaaS uygulaması için Kullanıcı sağlamayı etkinleştirirseniz Azure portal öznitelik değerlerini öznitelik eşlemeleriyle denetler.

Azure AD Kullanıcı nesneleri ve her SaaS uygulamasının Kullanıcı nesneleri arasında önceden yapılandırılmış bir öznitelikler ve öznitelik eşlemeleri kümesi vardır. Bazı uygulamalar, gruplar gibi kullanıcılarla birlikte diğer nesne türlerini de yönetir.

Varsayılan öznitelik eşlemelerini iş gereksinimlerinize göre özelleştirebilirsiniz. Bu nedenle, var olan öznitelik eşlemelerini değiştirebilir veya silebilir veya yeni öznitelik eşlemeleri oluşturabilirsiniz.

## <a name="editing-user-attribute-mappings"></a>Kullanıcı özniteliği düzenleniyor-eşlemeler

Kullanıcı hazırlama 'nin **eşlemeler** özelliğine erişmek için şu adımları izleyin:

1. [Azure Active Directory portalında](https://aad.portal.azure.com)oturum açın.
1. Sol bölmeden **Kurumsal uygulamalar** ' ı seçin. Galeriden eklenen uygulamalar dahil olmak üzere, yapılandırılan tüm uygulamaların bir listesi gösterilir.
1. Raporları görüntüleyebileceğiniz ve uygulama ayarlarını yönetebileceğiniz uygulama yönetimi bölmesini yüklemek için herhangi bir uygulamayı seçin.
1. Seçili uygulama için Kullanıcı hesabı sağlama ayarlarını yönetmek üzere **sağlamayı** seçin.
1. Azure AD ile hedef uygulama arasında akan Kullanıcı özniteliklerini görüntülemek ve düzenlemek için **eşlemeler** ' i genişletin. Hedef uygulama destekliyorsa, bu bölüm, isteğe bağlı olarak grupların ve Kullanıcı hesaplarının sağlanması yapılandırmanızı sağlar.

   ![Kullanıcı özniteliklerini görüntülemek ve düzenlemek için eşlemeleri kullanın](./media/customize-application-attributes/21.png)

1. İlgili **öznitelik eşleme** ekranını açmak Için bir **eşleme** yapılandırması seçin. Bir SaaS uygulamasının doğru çalışması için bazı öznitelik eşlemeleri gereklidir. Gerekli öznitelikler için, **silme** özelliği kullanılamaz.

   ![Uygulamalar için öznitelik eşlemelerini yapılandırmak üzere öznitelik eşlemesini kullanın](./media/customize-application-attributes/22.png)

   Bu ekran görüntüsünde, Salesforce 'ta yönetilen bir nesnenin **Kullanıcı adı** özniteliğinin, bağlantılı Azure Active Directory nesnesinin **userPrincipalName** değeriyle doldurulduğunu görebilirsiniz.

1. **Özniteliği Düzenle** ekranını açmak için varolan bir **öznitelik eşlemesi** seçin. Burada Azure AD ile hedef uygulama arasında akan Kullanıcı özniteliklerini düzenleyebilirsiniz.

   ![Kullanıcı özniteliklerini düzenlemek için özniteliği Düzenle özelliğini kullanın](./media/customize-application-attributes/23.png)

### <a name="understanding-attribute-mapping-types"></a>Öznitelik eşleme türlerini anlama

Öznitelik eşlemeleriyle, özniteliklerin bir üçüncü taraf SaaS uygulamasında nasıl doldurulduğunu kontrol edersiniz.
Dört farklı eşleme türü desteklenir:

- **Doğrudan** – Target özniteliği, Azure AD 'deki bağlantılı nesnenin bir özniteliğinin değeri ile doldurulur.
- **Sabit** – Target özniteliği belirttiğiniz belirli bir dizeyle doldurulur.
- **İfade** -Target özniteliği, komut dosyası benzeri bir ifadenin sonucuna göre doldurulur.
  Daha fazla bilgi için bkz. [Azure Active Directory öznitelik eşlemeleri Için Ifadeler yazma](functions-for-customizing-application-data.md).
- **Hiçbiri** -hedef öznitelik değiştirilmemiş olarak bırakıldı. Ancak, hedef özniteliği boşsa, belirttiğiniz varsayılan değerle doldurulur.

Bu dört temel tür ile birlikte özel öznitelik eşlemeleri, isteğe bağlı **varsayılan** değer atama kavramını destekler. Varsayılan değer atama, Azure AD 'de veya hedef nesnede bir değer olmadığında bir hedef özniteliğin bir değer ile doldurulmasını sağlar. En yaygın yapılandırma bu boş bırakılmamalıdır.

### <a name="understanding-attribute-mapping-properties"></a>Öznitelik eşleme özelliklerini anlama

Önceki bölümde, öznitelik eşleme türü özelliğine zaten sunuldu.
Bu özellik ile birlikte, öznitelik eşlemeleri de aşağıdaki öznitelikleri destekler:

- **Source özniteliği** -kaynak sistemden Kullanıcı özniteliği (örnek: Azure Active Directory).
- **Target özniteliği** – hedef sistemdeki kullanıcı özniteliği (örnek: ServiceNow).
- **Bu özniteliği kullanarak nesneleri Eşleştir** – bu eşlemenin, kaynak ve hedef sistemler arasında kullanıcıları benzersiz şekilde tanımlamak için kullanılması gerekip gerekmediğini belirtir. Genellikle, Azure AD 'deki userPrincipalName veya mail özniteliğinde ayarlanır; Bu, genellikle hedef uygulamadaki bir Kullanıcı adı alanına eşlenir.
- **Eşleşen öncelik** – birden çok eşleşen öznitelik ayarlanabilir. Birden çok olduğunda, bu alan tarafından tanımlanan sıraya göre değerlendirilir. Bir eşleşme bulunur başlamaz, başka eşleşen öznitelikler değerlendirilir.
- **Bu eşlemeyi Uygula**
  - **Her zaman** : Bu eşlemeyi hem Kullanıcı oluşturma hem de güncelleştirme eylemlerinde uygulayın.
  - **Yalnızca oluşturma sırasında** -bu eşlemeyi yalnızca Kullanıcı oluşturma eylemlerinde uygulayın.

## <a name="editing-group-attribute-mappings"></a>Grup özniteliği düzenleniyor-eşlemeler

ServiceNow, Box ve G Suite gibi seçilen sayıda uygulama, Grup nesneleri ve Kullanıcı nesneleri sağlama yeteneğini destekler. Grup nesneleri, Grup üyeleri ile birlikte görünen adlar ve e-posta diğer adları gibi grup özellikleri içerebilir.

![Örnek, sağlanan grup ve Kullanıcı nesneleri ile ServiceNow 'ı gösterir](./media/customize-application-attributes/24.png)

Grup sağlama, **eşlemeler**altında Grup eşlemesi seçilerek etkinleştirilebilir veya devre dışı bırakılabilir ve bu ayar, **öznitelik eşleme** ekranında istediğiniz **seçeneğe ayarlanabilir.**

Grup nesnelerinin bir parçası olarak sağlanan öznitelikler, daha önce açıklanan Kullanıcı nesneleriyle aynı şekilde özelleştirilebilir. 

> [!TIP]
> Grup nesnelerinin sağlanması (Özellikler ve Üyeler), bir uygulamaya [grupları atamanın](assign-user-or-group-access-portal.md) ayrı bir kavramıdır. Bir uygulamaya grup atamak mümkündür, ancak yalnızca grupta bulunan kullanıcı nesnelerini temin edin. Atamaları içinde grupları kullanmak için tam grup nesnelerinin sağlanması gerekmez.

## <a name="editing-the-list-of-supported-attributes"></a>Desteklenen özniteliklerin listesini düzenle

Belirli bir uygulama için desteklenen Kullanıcı öznitelikleri önceden yapılandırılmıştır. Çoğu uygulamanın kullanıcı yönetimi API 'Leri şema bulmayı desteklemez. Bu nedenle, Azure AD sağlama hizmeti uygulamaya çağrılar yaparak desteklenen özniteliklerin listesini dinamik olarak oluşturamayacak.

Ancak, bazı uygulamalar özel öznitelikleri destekler ve Azure AD sağlama hizmeti özel öznitelikleri okuyup yazabilir. Azure portal tanımlarını girmek için, **öznitelik eşleme** ekranının alt kısmındaki **Gelişmiş seçenekleri göster** onay kutusunu işaretleyin ve ardından uygulamanızın **öznitelik listesini düzenle** ' yi seçin.

Öznitelik listesini özelleştirmeyi destekleyen uygulamalar ve sistemler şunlardır:

- Satış
- ServiceNow
- Günü
- Azure Active Directory ([Azure AD Graph API varsayılan öznitelikleri](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity) ve özel dizin uzantıları desteklenir)
- [Çekirdek şemasında](https://tools.ietf.org/html/rfc7643) tanımlanan özniteliklerin eklenmesi gereken [SCIM 2,0](https://tools.ietf.org/html/rfc7643)' ı destekleyen uygulamalar

> [!NOTE]
> Desteklenen özniteliklerin listesinin düzenlenmesiyle yalnızca, uygulamalarının ve sistemlerinin şemasını özelleştiren Yöneticiler için önerilir ve özel özniteliklerinin nasıl tanımlandıkları hakkında ilk bilgiye sahip olmanız gerekir. Bu, bazen bir uygulama veya sistem tarafından sunulan API 'Ler ve geliştirici araçları hakkında daha fazla benzerlik gerektirir.

Desteklenen özniteliklerin listesi düzenlenirken aşağıdaki özellikler sağlanır:

- **Ad** -hedef nesnenin şemasında tanımlanan şekilde özniteliğin sistem adı.
- **Tür** : öznitelik depoladığı verilerin türü, hedef nesnenin şemasında tanımlandığı şekilde, aşağıdaki türlerden biri olabilir:
  - *Binary* özniteliği ikili veri içeriyor.
  - *Boolean* -öznitelik, doğru veya yanlış bir değer içeriyor.
  - *DateTime* -öznitelik bir tarih dizesi içerir.
  - *Integer* özniteliği bir tamsayı içeriyor.
  - *Reference* -Attribute, Hedef uygulamadaki başka bir tabloda depolanan bir değere başvuruda bulunan bir kimlik içeriyor.
  - *String* özniteliği bir metin dizesi içerir.
- **Birincil anahtar?** -Özniteliğin hedef nesnenin şemasında birincil anahtar alanı olarak tanımlanıp tanımlanmayacağı.
- **Gerekli?** -Özniteliğin hedef uygulamada veya sistemde doldurulması gerekip gerekmediği.
- **Çoklu değer?** -Özniteliğin birden çok değeri destekleyip desteklemediğini belirtir.
- **Tam durum?** -Öznitelik değerlerinin büyük/küçük harfe duyarlı bir şekilde değerlendirilip değerlendirilmediğini belirtir.
- **API ifadesi** -bunu, belirli bir sağlama Bağlayıcısı (Workday gibi) için belgeler tarafından istenmedikçe kullanmayın.
- **Başvurulan nesne özniteliği** -Eğer bir başvuru türü özniteliği ise, bu menü, öznitelik ile ilişkili değeri içeren hedef uygulamadaki tablo ve özniteliği seçmenizi sağlar. Örneğin, saklı değeri ayrı bir "departmanlar" tablosunda bir nesneye başvuran "Departman" adlı bir özniteliğe sahipseniz, "Departments.Name" seçeneğini belirleyin. Başvuru tabloları ve belirli bir uygulama için desteklenen birincil KIMLIK alanları önceden yapılandırılmıştır ve şu anda Azure portal kullanılarak düzenlenemiyor, ancak [Graph API](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/synchronization-configure-with-custom-target-attributes)kullanılarak düzenlenebilirler.

Yeni bir öznitelik eklemek için desteklenen özniteliklerin listesinin sonuna kaydırın, yukarıdaki girişleri kullanarak yukarıdaki alanları doldurun ve **öznitelik Ekle**' yi seçin. Öznitelik ekleme bittiğinde **Kaydet** ' i seçin. Ardından, yeni özniteliklerin öznitelik eşleme düzenleyicisinde kullanılabilir olması için **sağlama** sekmesini yeniden yüklemeniz gerekir.

## <a name="restoring-the-default-attributes-and-attribute-mappings"></a>Varsayılan öznitelikleri ve öznitelik eşlemelerini geri yükleme

Var olan eşlemelerinizi yeniden başlatmanız ve sıfırlamanız gerekir, varsayılan **eşlemeleri geri yükle** onay kutusunu seçip yapılandırmayı kaydedebilirsiniz. Bunun yapılması, uygulamanın uygulama galerisinden Azure AD kiracınıza yeni eklenmiş gibi tüm eşlemeleri ayarlar.

Bu seçeneğin belirlenmesi, sağlama hizmeti çalışırken tüm kullanıcıların yeniden eşitleme işlemini etkili bir şekilde zorlar.

> [!IMPORTANT]
> Bu seçeneği çağırmadan önce **sağlama durumunun** **kapalı** olarak ayarlanması önemle önerilir.

## <a name="what-you-should-know"></a>Bilmeniz gerekenler

- Microsoft Azure AD, eşitleme işleminin verimli bir uygulamasını sağlar. Başlatılmış bir ortamda, eşitleme çevrimi sırasında yalnızca güncelleştirme gerektiren nesneler işlenir.
- Özniteliği güncelleştirme eşleme döngüsünün performansına etkisi vardır. Öznitelik eşleme yapılandırmasına yönelik bir güncelleştirme, tüm yönetilen nesnelerin yeniden değerlendirilmesinin yapılmasını gerektirir.
- En iyi önerilen yöntem, öznitelik eşlemelerinizde yapılan ardışık değişikliklerin sayısını minimum olarak tutacağız.
- Fotoğrafı eşitleme biçimini belirtmediğinden, bir uygulamaya sağlanacak bir fotoğraf özniteliği eklemek bugün desteklenmez. Özelliği [Kullanıcı seste](https://feedback.azure.com/forums/169401-azure-active-directory) isteyebilirsiniz
- Isofdeleted özniteliği, genellikle bir uygulama için varsayılan eşlemelerin bir parçasıdır. Isofdeleted, dört senaryodan birinde doğru olabilir (Kullanıcı, uygulamadan atanmamış olması nedeniyle kapsam dışında, Kullanıcı kapsam dışı olduğundan, kapsam bir filtrenin yükseltilmediği, Kullanıcı Azure AD 'de geçici olarak silindiğinden veya AccountEnabled özelliği false olarak ayarlandığından  Kullanıcı). 
- Azure AD sağlama hizmeti null değerler sağlamayı desteklemiyor

## <a name="next-steps"></a>Sonraki adımlar

- [SaaS uygulamalarına Kullanıcı sağlamasını/sağlamayı kaldırmayı otomatikleştirme](user-provisioning.md)
- [Öznitelik eşlemeleri için Ifadeler yazma](functions-for-customizing-application-data.md)
- [Kullanıcı hazırlama için kapsam filtreleri](define-conditional-rules-for-provisioning-user-accounts.md)
- [Kullanıcıları ve grupları Azure Active Directory uygulamalara otomatik olarak sağlamayı etkinleştirmek için SCıM kullanma](use-scim-to-provision-users-and-groups.md)
- [SaaS uygulamalarının nasıl tümleştirileceği hakkında öğreticiler listesi](../saas-apps/tutorial-list.md)
