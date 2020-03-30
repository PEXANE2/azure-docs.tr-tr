---
title: Kapsam filtreleri ile sağlama uygulamaları | Microsoft Dokümanlar
description: Bir nesne iş gereksinimlerinizi karşılamazsa, otomatik kullanıcı sağlamayı destekleyen uygulamalardaki nesnelerin sağlanmasını önlemek için kapsam filtrelerini nasıl kullanacağınızı öğrenin.
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
ms.date: 09/11/2018
ms.author: mimart
ms.custom: H1Hack27Feb2017
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2c667409f2abb9f1cf89ae3b34f08e0f9eec067e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79138544"
---
# <a name="attribute-based-application-provisioning-with-scoping-filters"></a>Kapsam filtreleri ile öznitelik tabanlı uygulama sağlama
Bu makalenin amacı, hangi kullanıcıların bir uygulamaya sağlanmış olduğunu belirleyen öznitelik tabanlı kuralları tanımlamak için kapsam filtrelerinin nasıl kullanılacağını açıklamaktır.

## <a name="scoping-filter-use-cases"></a>Kapsam filtresi kullanım kılıfları

Kapsam filtresi, Azure Etkin Dizin (Azure AD) sağlama hizmetinin belirli bir değerle eşleşen bir özelliğe sahip kullanıcıları eklemesine veya hariç tutmasına olanak tanır. Örneğin, kullanıcıları Azure AD'den bir satış ekibi tarafından kullanılan bir SaaS uygulamasına sağlarken, yalnızca "Satış" özelliğine sahip "Departman" özelliğine sahip kullanıcıların sağlama kapsamında olması gerektiğini belirtebilirsiniz.

Kapsam filtreleri, sağlayan bağlayıcının türüne bağlı olarak farklı şekilde kullanılabilir:

* **Azure AD'den SaaS uygulamalarına giden sağlama.** Azure AD kaynak sistem olduğunda, [kullanıcı ve grup atamaları,](../manage-apps/assign-user-or-group-access-portal.md) hangi kullanıcıların sağlama kapsamında olduğunu belirlemek için en yaygın yöntemdir. Bu atamalar, tek oturum açmayı etkinleştirmek ve erişimi ve sağlamayı yönetmek için tek bir yöntem sağlamak için de kullanılır. Kapsam filtreleri, atamalara ek olarak veya bunların yerine, kullanıcıları öznitelik değerlerine göre filtrelemek için isteğe bağlı olarak kullanılabilir.

    >[!TIP]
    > **Tüm kullanıcıları ve grupları eşitlemek**için sağlama ayarları altında [Kapsam](../app-provisioning/user-provisioning.md#how-do-i-set-up-automatic-provisioning-to-an-application) menüsündeki ayarları değiştirerek, kurumsal uygulama için atamalara dayalı sağlamayı devre dışı kullanabilirsiniz. Bu seçeneğin yanı sıra öznitelik tabanlı kapsam filtreleri kullanmak, grup tabanlı atamaları kullanmaktan daha hızlı performans sağlar.  

* **HCM uygulamalarından Azure AD ve Active Directory'ye gelen sağlama.** İş Günü gibi bir [HCM uygulaması](../saas-apps/workday-tutorial.md) kaynak sistem olduğunda, kapsam filtreleri, HCM uygulamasından Active Directory veya Azure AD'ye hangi kullanıcıların sağlanması gerektiğini belirlemek için birincil yöntemdir.

Varsayılan olarak, Azure AD sağlama bağlayıcılarının yapılandırılan öznitelik tabanlı kapsam filtreleri yoktur. 

## <a name="scoping-filter-construction"></a>Kapsam filtresi yapımı

Kapsam filtresi bir veya daha fazla *yan tümceden*oluşur. Yan tümceler, her kullanıcının özniteliklerini değerlendirerek hangi kullanıcıların kapsam filtresinden geçmesine izin verilebilenleri belirler. Örneğin, bir kullanıcının "Eyalet" özniteliğinin "New York" özelliğine eşit olduğunu, bu nedenle uygulamaya yalnızca New York kullanıcılarının girmesini gerektiren bir yan tümceniz olabilir. 

Tek bir tümce, tek bir öznitelik değeri için tek bir koşul tanımlar. Tek bir kapsam filtresinde birden çok yan tümce oluşturulursa, "AND" mantığı kullanılarak birlikte değerlendirilirler. Bu, bir kullanıcının sağlanması için tüm yan tümcelerin "doğru" olarak değerlendirilmesi gerektiği anlamına gelir.

Son olarak, tek bir uygulama için birden çok kapsam filtresi oluşturulabilir. Birden çok kapsam filtresi varsa, "VEYA" mantığı kullanılarak birlikte değerlendirilir. Bu, yapılandırılan kapsam filtrelerinden herhangi birinde yer alan tüm yan tümcelerin "doğru" olarak değerlendirildiğini, kullanıcının sağlanmış olduğu anlamına gelir.

Azure AD sağlama hizmeti tarafından işlenen her kullanıcı veya grup, her kapsam filtresine göre her zaman ayrı ayrı değerlendirilir.

Örnek olarak, aşağıdaki kapsam filtresini göz önünde bulundurun:

![Kapsam filtresi](./media/define-conditional-rules-for-provisioning-user-accounts/scoping-filter.PNG) 

Bu kapsam filtresine göre, kullanıcıların sağlanacak aşağıdaki kriterleri karşılaması gerekir:

* New York'ta olmalılar.
* Mühendislik bölümünde çalışmak zorundalar.
* Şirket çalışan larının kimliği 1.000.000 ile 2.000.000 arasında olmalıdır.
* Onların iş unvanı boş veya boş olmamalıdır.

## <a name="create-scoping-filters"></a>Kapsam filtreleri oluşturma
Kapsam filtreleri, her Azure AD kullanıcı sağlama bağlayıcısı için öznitelik eşlemelerinin bir parçası olarak yapılandırılır. Aşağıdaki yordam, [desteklenen uygulamalardan biri](../saas-apps/tutorial-list.md) için otomatik sağlama zaten ayarladığınızı ve bu uygulamaya bir kapsam filtresi eklediğinizi varsayar.

### <a name="create-a-scoping-filter"></a>Kapsam filtresi oluşturma
1. Azure [portalında,](https://portal.azure.com)Azure **Active Directory** > **Enterprise Applications** > **Tüm uygulamalar** bölümüne gidin.

2. Otomatik sağlama yı yapılandırdığınız uygulamayı seçin: örneğin, "ServiceNow".

3. **Sağlama** sekmesini seçin.

4. **Eşlemeler** bölümünde, kapsam landırma filtresini yapılandırmak istediğiniz eşleciliği seçin: örneğin, "Azure Etkin Dizin Kullanıcılarını ServiceNow ile senkronize edin".

5. Kaynak **nesne kapsamı** menüsünü seçin.

6. **Kapsam filtresi ekle'yi**seçin.

7. Bir kaynak **Öznitelik Adı,** **Işleç**ve eşleşecek bir **Öznitelik Değeri** seçerek bir yan tümce tanımlayın. Aşağıdaki işleçler desteklenir:

   a. **EŞLer**. Değerlendirilen öznitelik giriş dize değeriyle tam olarak eşleşirse (büyük/küçük harf duyarlı) "true" yan tümcesi döndürür.

   b. **EŞIT Değİl .** Değerlendirilen öznitelik giriş dize değeri (büyük/küçük harf duyarlı) eşleşmiyorsa yan tümce "true" döndürür.

   c. **DOĞRUDUR.** Değerlendirilen öznitelik gerçek bir Boolean değeri içeriyorsa yan tümce "true" döndürür.

   d. **YANLıŞ MıDıR.** Değerlendirilen öznitelik false boolean değeri içeriyorsa yan tümce "true" döndürür.

   e. **NULL OLDUĞUNU**. Değerlendirilen öznitelik boşsa yan tümce "true" döndürür.

   f. **NULL DEĞILDIR.** Değerlendirilen öznitelik boş değilse yan tümce "true" döndürür.

   g. **REGEX MAÇ**. Değerlendirilen öznitelik normal bir ifade deseniyle eşleşiyorsa, yan tümce "true" döndürür. Örneğin: ([1-9][0-9]) 10 ile 99 arasındaki sayılarla eşleşir.

   h. **REGEX EŞLEŞMESI DEĞIL.** Değerlendirilen öznitelik normal bir ifade deseniyle eşleşmiyorsa, yan tümce "true" döndürür.
   
   i. **Greater_Than.** Değerlendirilen öznitelik değerden büyükse yan tümce "true" verir. Kapsam filtresinde belirtilen değer bir tamsayı olmalı ve kullanıcıdaki öznitelik bir tamsayı olmalıdır [0,1,2,...]. 
   
   j. **Greater_Than_OR_EQUALS.** Değerlendirilen öznitelik değerden büyük veya eşitse yan tümce "true" döndürür. Kapsam filtresinde belirtilen değer bir tamsayı olmalı ve kullanıcıdaki öznitelik bir tamsayı olmalıdır [0,1,2,...]. 
   
   k. **Içerir.** Değerlendirilen öznitelik [burada](https://docs.microsoft.com/dotnet/api/system.string.contains?view=netframework-4.8)açıklandığı gibi dize değerini (büyük/küçük harf duyarlı) içeriyorsa yan tümce "true" döndürür. 


>[!IMPORTANT] 
> - IsMemberOf filtresi şu anda desteklenmiyor.
> - EQUALS ve NOT EQUALS çok değerli öznitelikler için desteklenmez

9. İsteğe bağlı olarak, daha fazla kapsam altanlama yan tümcesi eklemek için 7-8 adımlarını yineleyin.

10. **Kapsam Filtresi Başlığı'nda,** kapsam filtreniz için bir ad ekleyin.

11. **Tamam'ı**seçin.

12. **Kapsam Filtreleri** ekranında tekrar **Tamam'ı** seçin. İsteğe bağlı olarak, başka bir kapsam filtresi eklemek için 6-11 adımlarını yineleyin.

13. **Öznitelik Eşleme** ekranında **Kaydet'i** seçin. 

>[!IMPORTANT] 
> Yeni bir kapsam filtresi nin kaydedilmesi, kaynak sistemdeki tüm kullanıcıların yeni kapsam filtresine göre yeniden değerlendirildiği uygulama için yeni bir tam eşitleme tetikler. Uygulamadaki bir kullanıcı daha önce sağlama kapsamındaysa, ancak kapsam dışına çıkarsa, hesabı devre dışı bırakılır veya uygulamada hükmü bozulabilir. Bu varsayılan davranışı geçersiz kılmak [için, kapsam dışına çıkan kullanıcı hesapları için silmeyi atla'ya](../app-provisioning/skip-out-of-scope-deletions.md)bakın.


## <a name="common-scoping-filters"></a>Yaygın kapsam filtreleri
| Hedef Öznitelik| İşleç | Değer | Açıklama|
|----|----|----|----|
|userPrincipalName|REGEX MAÇI|.\*@domain.com |KullanıcıPrensibi olan tüm kullanıcılar, @domain.com etki alanına sahip|
|userPrincipalName|DEĞIL REGEX MAÇ|.\*@domain.com|KullanıcıPrensibi olan tüm kullanıcılar, @domain.com etki alanına sahip olan tüm kullanıcılar,|
|bölüm|Eşit -tir|Satış|Satış departmanındaki tüm kullanıcılar,|
|işçi Kimliği|REGEX MAÇI|(1[0-9][0-9][0-9][0-9][0-9][0-9])| 1000000 ile 2000000 arasında işçi liği olan tüm çalışanlar tedarik kapsamındadır.|

## <a name="related-articles"></a>İlgili makaleler:
* [SaaS uygulamalarında kullanıcı sağlama ve sağlamanın otomatikleştirilmesi](../app-provisioning/user-provisioning.md)
* [Kullanıcı sağlama için öznitelik eşlemelerini özelleştirme](../app-provisioning/customize-application-attributes.md)
* [Öznitelik eşlemeleri için ifadeler yazma](functions-for-customizing-application-data.md)
* [Hesap sağlama bildirimleri](../app-provisioning/user-provisioning.md)
* [Azure Active Directory'den uygulamalara kullanıcıların ve grupların otomatik olarak sağlanmasını etkinleştirmek için SCIM'i kullanın](../app-provisioning/use-scim-to-provision-users-and-groups.md)
* [SaaS uygulamalarının nasıl entegre edilebildiğini anlatan öğreticiler listesi](../saas-apps/tutorial-list.md)

