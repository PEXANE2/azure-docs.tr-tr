---
title: Azure Active Directory kullanarak talep üzerine Kullanıcı sağlama
description: Eşitlemeyi zorla
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-provisioning
ms.workload: identity
ms.topic: how-to
ms.date: 06/23/2020
ms.author: mimart
ms.reviewer: arvinh
ms.openlocfilehash: 7799e873afb117481cebafd982df59a3267f4405
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87051589"
---
# <a name="on-demand-provisioning"></a>İsteğe bağlı sağlama
İsteğe bağlı sağlama, bir kullanıcıyı birkaç saniye içinde uygulamanıza olanak tanır. Yapılandırma sorunlarını hızla gidermek, tanımladığınız ifadeleri doğrulamak, test kapsamı filtrelerini ve çok daha fazlasını yapmak için bu özelliği kullanabilirsiniz. 

## <a name="how-to-use-on-demand-provisioning"></a>İsteğe bağlı sağlama kullanımı 

1. **Azure Portal**oturum açın.
2. **Kurumsal uygulamalar**'a gidin.
3. Uygulamanızı seçin ve sağlama yapılandırma sayfasına gidin.
4. Yönetici kimlik bilgilerinizi sağlayarak sağlamayı yapılandırın.
5. **İsteğe bağlı sağlama**' ya tıklayın.
6. Kullanıcı adını adı, soyadı, görünen ad, Kullanıcı asıl adı veya e-posta ile arayın.
7. Sayfanın alt kısmındaki sağla ' yı seçin.

:::image type="content" source="media/provision-on-demand/on-demand-provision-user.jpg" alt-text="İsteğe bağlı bir Kullanıcı sağlayın.":::

## <a name="understanding-the-provisioning-steps"></a>Sağlama adımlarını anlama
İsteğe bağlı sağlama özelliği, bir Kullanıcı sağlanırken sağlama hizmetinin aldığı adımları göstermeye çalışır. Genellikle bir kullanıcı sağlamak için beş adım vardır ve aşağıdaki adımlardan biri veya birkaçı, isteğe bağlı sağlama deneyiminde gösterilir.

### <a name="step-1-test-connection"></a>1. Adım: bağlantıyı test etme
Sağlama Hizmeti, "test kullanıcısı" için bir istek yaparak hedef uygulamaya erişim yetkisi kurmaya çalışır. Sağlama Hizmeti, sağlama adımlarıyla devam etmek için yetkilendirildiğini belirten bir yanıt bekler. Bu adım yalnızca adımda bir hata olduğunda gösterilir. Adım başarılı olduğunda isteğe bağlı sağlama deneyiminde gösterilmez. 

**Sorun giderme ipuçları**
* Hedef uygulamaya, gizli belirteç ve kiracı URL 'SI gibi geçerli kimlik bilgileri sağladığınızdan emin olun. Gereken kimlik bilgileri uygulamaya göre farklılık gösterir. Ayrıntılı yapılandırma öğreticilerini [burada](https://docs.microsoft.com/azure/active-directory/saas-apps/tutorial-list)bulabilirsiniz. 
* Hedef uygulamanın, öznitelik eşlemeleri dikey penceresinde tanımlanan eşleşen öznitelikler üzerinde filtrelemeyi desteklediğinden emin olun. Destekledikleri filtreleri anlamak için uygulama geliştiricisi tarafından sunulan API belgelerini denetlemeniz gerekebilir.  
* SCıM uygulamaları için, Azure AD sağlama hizmeti beklediği için uygulamanın yetkilendirme isteklerine yanıt vermesini sağlamak üzere Postman gibi bir araç kullanabilirsiniz. [Burada](https://docs.microsoft.com/azure/active-directory/app-provisioning/use-scim-to-provision-users-and-groups#request-3)bir örnek istek bulunabilir.

### <a name="step-2-import-user"></a>2. Adım: Kullanıcı Içeri aktarma
Ardından, sağlama hizmeti kullanıcıyı kaynak sistemden alır. Hizmetin aldığı Kullanıcı öznitelikleri daha sonra kullanıcının sağlama kapsamında olup olmadığını değerlendirmek, mevcut bir kullanıcı için hedef sistemi denetlemek ve hedef sisteme hangi kullanıcı özniteliklerinin dışarı aktarılacağını belirlemek için kullanılır. 

**Ayrıntıları görüntüle**

Ayrıntıları görüntüle bölümü, kaynak sistemden içeri aktarılan kullanıcının özelliklerini gösterir (örneğin, Azure AD).

**Sorun giderme ipuçları**
* Kaynak sistemdeki kullanıcı nesnesinde eşleşen özniteliği eksik olduğunda Kullanıcı içeri aktarma başarısız olabilir. Kullanıcı nesnesini eşleşen öznitelik için bir değerle güncelleştirerek veya sağlama yapılandırmanızda eşleşen özniteliği değiştirerek bu hatayı çözebilirsiniz.  
* Beklediğiniz bir öznitelik, içeri aktarılan listede yoksa, özniteliğin kaynak sistemdeki kullanıcı nesnesinde bir değere sahip olduğundan emin olun. Sağlama hizmeti şu anda null öznitelikleri sağlamayı desteklemiyor. 
* Sağlama yapılandırma özniteliği eşleme sayfanızın beklediğiniz özniteliği içerdiğinden emin olun. 

### <a name="step-3-determine-if-user-is-in-scope"></a>3. Adım: kullanıcının kapsamda olup olmadığını belirleme
Ardından, sağlama hizmeti kullanıcının sağlama [kapsamında](https://docs.microsoft.com/azure/active-directory/app-provisioning/how-provisioning-works#scoping) olup olmadığını belirler. Hizmet, kullanıcının uygulamaya atanıp atanmadığını, kapsamın eşitleme atanan veya eşitleme olarak ayarlanmış olup olmadığını ve sağlama yapılandırmanızda tanımlanan kapsam filtrelerini göz önünde bulunduracaktır.  

**Ayrıntıları görüntüle**

Ayrıntıları görüntüle bölümü değerlendirilen kapsam koşullarını gösterir. Aşağıdaki özelliklerden birini görebilirsiniz:
* **Kaynak sistemde etkin** özelliği, kullanıcının bir özelliğin etkin olduğunu, Azure AD 'de doğru olarak ayarlandığını gösterir.
* **Uygulamaya atanan** , kullanıcının Azure AD 'de uygulamaya atandığını belirtir
* **Kapsam eşitleme** , kapsam ayarının Kiracıdaki tüm kullanıcılara ve gruplara izin verdiğini belirtir.
* **Kullanıcının gerekli rolü** , kullanıcının uygulamaya sağlanması için gerekli rollerin olduğunu gösterir. 
* Uygulamanız için kapsam filtrelerini tanımlamış olmanız durumunda **kapsam filtreleri** de gösterilir. Filtre şu biçimde görüntülenir-{kapsam filtresi başlığı} {kapsam Filtresi özniteliği} {kapsam filtre işleci} {kapsam filtresi değeri}. 

**Sorun giderme ipuçları**
* Geçerli bir kapsam rolü tanımlamış olduğunuzdan emin olun. Örneğin, tamsayı olmayan bir değerle ["büyüktür"](https://docs.microsoft.com/azure/active-directory/app-provisioning/define-conditional-rules-for-provisioning-user-accounts#create-a-scoping-filter) işlecini kullanmaktan kaçının. 
* Kullanıcının gerekli rolü yoksa, [burada](https://docs.microsoft.com/azure/active-directory/app-provisioning/application-provisioning-config-problem-no-users-provisioned#provisioning-users-assigned-to-the-default-access-role)açıklanan ipuçlarını gözden geçirin. 

### <a name="step-4-match-user-between-source-and-target"></a>4. Adım: kullanıcıdan kaynak ve hedef arasında eşleşme
Bu adımda. hizmet, içeri aktarma adımında alınan kullanıcıyla hedef sistemdeki bir kullanıcıyla eşleştirmeye çalışır. 

**Ayrıntıları görüntüle**

Ayrıntıları görüntüle sayfaları, hedef sistemde eşleşen kullanıcı (ler) i özelliklerini gösterir. Bağlam bölmesinde gördüğünüz Özellikler aşağıdaki gibi değişir:
* Hedef sistemde eşleşen hiçbir kullanıcı yoksa, herhangi bir özellik görmezsiniz.
* Hedef sistemde eşleşen bir kullanıcı varsa, hedef sistemden bu eşleşen kullanıcının özelliklerini görürsünüz.
* Birden çok Kullanıcı eşleşirse, eşleşen kullanıcıların özelliklerini görürsünüz.
* Birden çok eşleşen öznitelik öznitelik eşlemelerinizin parçasıysa, eşleşen her öznitelik sırayla değerlendirilir ve eşleşen kullanıcılar gösterilir. 

**Sorun giderme ayrıntıları**
* Sağlama Hizmeti, kaynaktaki bir kullanıcıyla hedefteki bir kullanıcıyla benzersiz bir şekilde eşleşemez. Bu, eşleşen özniteliğin benzersiz olmasını sağlayarak çözülebilir. 
* Hedef uygulamanın, eşleşen öznitelik olarak tanımlanan öznitelik üzerinde filtrelemeyi desteklediğinden emin olun.  

### <a name="step-5-perform-action"></a>5. Adım: eylemi gerçekleştirme
Son olarak, sağlama hizmeti kullanıcı oluşturma, güncelleştirme, silme veya atlama gibi bir eylem gerçekleştirir. 

:::image type="content" source="media/provision-on-demand/success-on-demand-provision.jpg" alt-text="Kullanıcının başarılı bir şekilde sağlanması.":::

**Ayrıntıları görüntüle**

Ayrıntıları görüntüle bölümü, hedef uygulamada değiştirilen öznitelikleri görüntüler. Bu, sağlama hizmeti etkinliğinin son çıkışını ve aktarılmış öznitelikleri temsil eder. Bu adım başarısız olursa, görüntülenen öznitelikler sağlama hizmetinin değiştirme girişiminde bulunduğu öznitelikleri temsil eder.  

**Sorun giderme ipuçları**
* Değişiklikleri dışarı aktarma sorunları büyük ölçüde farklılık gösterebilir. Genel hatalara yönelik sağlama günlükleri [belgelerini](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-provisioning-logs#error-codes) inceleyin.


## <a name="frequently-asked-questions"></a>Sık sorulan sorular
**İsteğe bağlı sağlama kullanmak için sağlama özelliğini açmanız gerekiyor mu?** Yetkilendirme için uzun süreli bir taşıyıcı belirteci veya Kullanıcı adı ve parola kullanan uygulamalar için ek bir adım gerekmez. Yetkilendirme için OAuth kullanan uygulamalar şu anda sağlama işinin isteğe bağlı sağlama kullanılmadan önce durdurulmasını gerektirir. G Suite, Box, Facebook tarafından çalışma alanı ve bolluk bu kategoriye girer. Sağlama işlemini durdurmak zorunda kalmadan tüm uygulamalar için isteğe bağlı sağlama çalıştırmaya izin veren iş devam ediyor. 

**İsteğe bağlı sağlama ne kadar sürer?** Genellikle 30 saniyeden daha az sürer. 

## <a name="known-limitations"></a>Bilinen Sınırlamalar
Günümüzde bazı bilinen sınırlamalar vardır. Daha sonra yapılacak geliştirmeleri daha iyi önceliklendirmemiz için lütfen [Kullanıcı seslerinde](https://aka.ms/appprovisioningfeaturerequest) gönderin. Bu sınırlamaların isteğe bağlı sağlama özelliğine özgü olduğunu unutmayın. bir uygulamanın sağlama gruplarını, silmeleri, vb. özelliklerini destekleyip desteklemediğini öğrenmek için uygulama öğreticisini kontrol edin. 

* Uygulamalar Workday, AWS ve başarılı faktörleri isteğe bağlı sağlamayı desteklemez.
* İsteğe bağlı sağlama grupları ve rolleri desteklenmez.
* İsteğe bağlı sağlama, uygulamadan atanmamış kullanıcıların devre dışı bırakılmasını destekler, ancak Azure Active Directory devre dışı bırakılmış veya silinmiş kullanıcıların devre dışı bırakılmasını veya silinmesini desteklemez (Bu kullanıcılar bir Kullanıcı ararken görünmez).

## <a name="next-steps"></a>Sonraki Adımlar

* [Sağlama sorunlarını giderme](https://docs.microsoft.com/azure/active-directory/app-provisioning/application-provisioning-config-problem)
