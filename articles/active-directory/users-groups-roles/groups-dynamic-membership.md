---
title: Dinamik olarak doldurulan grup üyeliği için kurallar - Azure AD | Microsoft Dokümanlar
description: Grupları otomatik olarak doldurmak için üyelik kuralları ve kural başvurusu oluşturma.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 11/27/2019
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: d6f8237ac13744e56baa8551f8cced12b2785a48
ms.sourcegitcommit: fb23286d4769442631079c7ed5da1ed14afdd5fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2020
ms.locfileid: "81114734"
---
# <a name="dynamic-membership-rules-for-groups-in-azure-active-directory"></a>Azure Etkin Dizini'ndeki gruplar için dinamik üyelik kuralları

Azure Etkin Dizini'nde (Azure AD), gruplar için dinamik üyelikleri etkinleştirmek için karmaşık öznitelik tabanlı kurallar oluşturabilirsiniz. Dinamik grup üyeliği, kullanıcı ekleme ve kaldırma yönetim ek yükü azaltır. Bu makalede, kullanıcılar veya aygıtlar için dinamik üyelik kuralları oluşturmak için özellikleri ve sözdizimi ayrıntıları. Güvenlik gruplarında veya Office 365 gruplarında dinamik üyelik için bir kural ayarlayabilirsiniz.

Bir kullanıcının veya aygıtın herhangi bir öznitelikleri değiştiğinde, sistem değişikliğin herhangi bir grubu tetiklemeyeceğini veya kaldıracağını görmek için dizindeki tüm dinamik grup kurallarını değerlendirir. Bir kullanıcı veya aygıt bir grup üzerindeki kuralı karşılarsa, bu grup üyesi olarak eklenir. Artık kuralı yerine getirmezlerse, kaldırılırlar. Dinamik bir grubun üyesini el ile ekleyip kaldıramam.

- Aygıtlar veya kullanıcılar için dinamik bir grup oluşturabilirsiniz, ancak hem kullanıcıları hem de aygıtları içeren bir kural oluşturamazsınız.
- Aygıt sahiplerinin özniteliklerine dayalı bir aygıt grubu oluşturamazsınız. Aygıt üyelik kuralları yalnızca aygıt özniteliklerine başvurur.

> [!NOTE]
> Bu özellik, bir veya daha fazla dinamik grubun üyesi olan her benzersiz kullanıcı için bir Azure AD Premium P1 lisansı gerektirir. Dinamik grupların üyesi olmaları için kullanıcılara lisans atamanız gerekmemektedir, ancak bu tür tüm kullanıcıları kapsayacak şekilde kiracıda en az sayıda lisans olması gerekir. Örneğin, kiracınızdaki tüm dinamik gruplarda toplam 1.000 benzersiz kullanıcınız varsa, lisans gereksinimini karşılamak için Azure AD Premium P1 için en az 1.000 lisans gerekir.
> Dinamik bir aygıt grubunun üyesi olan aygıtlar için lisans gerekmez.

## <a name="rule-builder-in-the-azure-portal"></a>Azure portalında kural oluşturucu

Azure AD, önemli kurallarınızı daha hızlı oluşturmak ve güncelleştirmek için bir kural oluşturucu sağlar. Kural oluşturucu en fazla beş ifade inşaat destekler. Kural oluşturucu, birkaç basit ifadeyle bir kural oluşturmayı kolaylaştırır, ancak her kuralı çoğaltmak için kullanılamaz. Kural oluşturucu oluşturmak istediğiniz kuralı desteklemiyorsa, metin kutusunu kullanabilirsiniz.

Metin kutusunu kullanarak oluşturmanızı önerdiğimiz gelişmiş kurallara veya sözdizimine bazı örnekler aşağıda verilmiştir:

- Beşten fazla ifade içeren kural
- Doğrudan raporlar kuralı
- Ayar [operatörü önceliği](groups-dynamic-membership.md#operator-precedence)
- [Karmaşık ifadeler içeren kurallar;](groups-dynamic-membership.md#rules-with-complex-expressions) örneğin`(user.proxyAddresses -any (_ -contains "contoso"))`

> [!NOTE]
> Kural oluşturucu, metin kutusunda oluşturulmuş bazı kuralları görüntüleyemeyebilir. Kural oluşturucu kuralı görüntüleyemediğinde bir ileti görebilirsiniz. Kural oluşturucu, desteklenen sözdizimini, doğrulamayı veya dinamik grup kurallarının işlenmesini hiçbir şekilde değiştirmez.

Daha fazla adım adım yönergeiçin [dinamik bir grup oluştur veya güncelleştir'e](groups-create-rule.md)bakın.

![Dinamik bir grup için üyelik kuralı ekleme](./media/groups-dynamic-membership/update-dynamic-group-rule.png)

### <a name="rule-syntax-for-a-single-expression"></a>Tek bir ifade için kural sözdizimi

Tek bir ifade, üyelik kuralının en basit biçimidir ve yalnızca yukarıda belirtilen üç bölümden oluşur. Tek bir ifadeye sahip bir `Property Operator Value`kural şuna benzer: , özelliğin sözdizimi object.property'in adıdır.

Aşağıda, tek bir ifadeyle düzgün bir şekilde oluşturulmuş bir üyelik kuralı örneği verilmiştir:

```
user.department -eq "Sales"
```

Parantezler tek bir ifade için isteğe bağlıdır. Üyelik kuralınızın toplam uzunluğu 2048 karakteri geçemez.

## <a name="constructing-the-body-of-a-membership-rule"></a>Üyelik kuralının gövdesinin oluşturulması

Bir grubu kullanıcılar veya aygıtlarla otomatik olarak dolduran bir üyelik kuralı, doğru veya yanlış bir sonuçla sonuçlanan ikili bir ifadedir. Basit bir kuralın üç bölümü şunlardır:

- Özellik
- İşleç
- Değer

Sözdizimi hatalarını önlemek için bir ifade içindeki parçaların sırası önemlidir.

## <a name="supported-properties"></a>Desteklenen özellikler

Üyelik kuralı oluşturmak için kullanılabilecek üç tür özellik vardır.

- Boole
- Dize
- String koleksiyonu

Aşağıda, tek bir ifade oluşturmak için kullanabileceğiniz kullanıcı özellikleri verilmiştir.

### <a name="properties-of-type-boolean"></a>Tip boolean özellikleri

| Özellikler | İzin verilen değerler | Kullanım |
| --- | --- | --- |
| hesapEtkin |gerçek yanlış |user.accountEtkin -eq doğru |
| dirSyncEnabled |gerçek yanlış |user.dirSyncEnabled -eq doğru |

### <a name="properties-of-type-string"></a>Tür dizesinin özellikleri

| Özellikler | İzin verilen değerler | Kullanım |
| --- | --- | --- |
| city |Herhangi bir dize değeri veya *null* |(user.city -eq "değer") |
| ülke |Herhangi bir dize değeri veya *null* |(user.country -eq "değer") |
| Şirketadı | Herhangi bir dize değeri veya *null* | (user.companyName -eq "değer") |
| bölüm |Herhangi bir dize değeri veya *null* |(user.department -eq "değer") |
| displayName |Herhangi bir dize değeri |(user.displayName -eq "değer") |
| Employeeıd |Herhangi bir dize değeri |(user.employeeId -eq "değer")<br>(user.employeeId -ne *null*) |
| faksTelefon Numarası |Herhangi bir dize değeri veya *null* |(user.facsimileTelephoneNumber -eq "değer") |
| givenName |Herhangi bir dize değeri veya *null* |(user.givenName -eq "değer") |
| jobTitle |Herhangi bir dize değeri veya *null* |(user.jobTitle -eq "değer") |
| posta |Herhangi bir dize değeri veya *null* (kullanıcının SMTP adresi) |(user.mail -eq "değer") |
| mailNickName |Herhangi bir dize değeri (kullanıcının posta diğer adı) |(user.mailNickName -eq "değer") |
| mobil |Herhangi bir dize değeri veya *null* |(user.mobile -eq "değer") |
| Objectıd |Kullanıcı nesnesinin GUID'i |(user.objectId -eq "11111111-1111-1111111111111111111111111111111111) |
| onPremisesSecurityIdentifier | Şirket içi buluta senkronize edilen kullanıcılar için şirket içi güvenlik tanımlayıcısı (SID). |(user.onPremisesSecurityIdentifier -eq "S-1-1-11-11111111-111111111-11111111111111111111111111111111111111111111111111111111111111111111111) |
| passwordPolitikalar |Yok DisableStrongPassword Devre Dışı Şifre Sona ErdirmePasswordExpiration, DisableStrongPassword |(user.passwordPolicies -eq "DisableStrongPassword") |
| physicalDeliveryOfficeName |Herhangi bir dize değeri veya *null* |(user.physicalDeliveryOfficeName -eq "değer") |
| Postakodu |Herhangi bir dize değeri veya *null* |(user.postalCode -eq "değer") |
| tercihDil |ISO 639-1 kodu |(user.preferredLanguage -eq "en-US") |
| sipProxyAddress |Herhangi bir dize değeri veya *null* |(user.sipProxyAddress -eq "değer") |
| durum |Herhangi bir dize değeri veya *null* |(user.state -eq "değer") |
| Streetaddress |Herhangi bir dize değeri veya *null* |(user.streetAddress -eq "değer") |
| surname |Herhangi bir dize değeri veya *null* |(user.surname -eq "değer") |
| Telephonenumber |Herhangi bir dize değeri veya *null* |(user.telephoneNumber -eq "değer") |
| kullanımKonum |İki harfli ülke kodu |(user.usageLocation -eq "US") |
| userPrincipalName |Herhangi bir dize değeri |(user.userPrincipalName -eqalias@domain" ") |
| Usertype |üye konuk *null* |(user.userType -eq "Üye") |

### <a name="properties-of-type-string-collection"></a>Tür dize koleksiyonunun özellikleri

| Özellikler | İzin verilen değerler | Kullanım |
| --- | --- | --- |
| otherMails |Herhangi bir dize değeri |(user.otherMails -içeriralias@domain" ") |
| proxyAddresses |SMTP: alias@domain smtp:alias@domain |(user.proxyAdresleri -içerir "SMTP: alias@domain") |

Aygıt kuralları için kullanılan özellikler [için aygıtlar için Kurallar'a](#rules-for-devices)bakın.

## <a name="supported-expression-operators"></a>Desteklenen ifade işleçleri

Aşağıdaki tablo, desteklenen tüm işleçleri ve tek bir ifade için sözdizimini listeler. İşleçler tire (-) öneki ile veya olmadan kullanılabilir.

| İşleç | Sözdizimi |
| --- | --- |
| Eşit Değil |-ne |
| Eşittir |-eq |
| Ile Başlamaz |-notStartsWith |
| Ile Başlar |-startsWith |
| Içermez |-notContains |
| Contains |-içerir |
| Eşleşmez |-notMatch |
| Eşleştirme |-maç |
| İçindeki | -içinde |
| Içinde Değil | -notIn |

### <a name="using-the--in-and--notin-operators"></a>-in ve -notIn operatörlerini kullanma

Bir kullanıcı özniteliğinin değerini bir dizi farklı değerle karşılaştırmak istiyorsanız,-in veya -notIn işleçlerini kullanabilirsiniz. Değerler listesini başlatmak ve bitirmek için "[" ve "]" köşeli ayraç sembollerini kullanın.

 Aşağıdaki örnekte, user.department değeri listedeki değerlerden herhangi biri eşitse, ifade doğruyu değerlendirir:

```
   user.department -in ["50001","50002","50003","50005","50006","50007","50008","50016","50020","50024","50038","50039","51100"]
```


### <a name="using-the--match-operator"></a>-match işlecinin kullanılması 
**-match** işleci herhangi bir normal ifade eşleştirmek için kullanılır. Örnekler:

```
user.displayName -match "Da.*"   
```
Da, Dav, David doğru değerlendirmek, aDa yanlış değerlendirir.

```
user.displayName -match ".*vid"
```
David doğru değerlendirir, Da yanlış değerlendirir.

## <a name="supported-values"></a>Desteklenen değerler

Bir ifadede kullanılan değerler, aşağıdakiler dahil olmak üzere birkaç türden oluşabilir:

* Dizeler
* Boolean – doğru, yanlış
* Numaralar
* Diziler – sayı dizisi, dize dizisi

Bir ifade içinde bir değer belirtirken, hataları önlemek için doğru sözdizimini kullanmak önemlidir. Bazı sözdizimi ipuçları şunlardır:

* Çift tırnak değeri bir dize olmadığı sürece isteğe bağlıdır.
* String ve regex işlemleri büyük/küçük harf duyarlı değildir.
* Bir dize değeri çift tırnak içeriyorsa, \` her iki tırnak da karakter \`kullanılarak\`kaçmış olmalıdır, örneğin, user.department -eq "Satış " "Satış" değeri olduğunda uygun sözdizimidir.
* Null denetimlerini, örneğin bir değer olarak null `user.department -eq null`kullanarak da gerçekleştirebilirsiniz.

### <a name="use-of-null-values"></a>Null değerlerinin kullanımı

Bir kuralda null değeri belirtmek *için, null* değerini kullanabilirsiniz. 

* Bir ifadedeki *null* değerini karşılaştırırken -eq veya -ne kullanın.
* Yalnızca gerçek bir dize değeri olarak yorumlanması istiyorsanız *null* sözcüğünün etrafındaki tırnak tırnaklarını kullanın.
* -değil işleci null için karşılaştırmalı bir operatör olarak kullanılamaz. Kullanırsanız, null veya $null kullanın olsun bir hata alırsınız.

Null değeri başvuru doğru yolu aşağıdaki gibidir:

```
   user.mail –ne null
```

## <a name="rules-with-multiple-expressions"></a>Birden çok ifade içeren kurallar

Grup üyeliği kuralı, mantıksal işleçler tarafından bağlanan birden fazla tek ifadeden oluşabilir. Mantıksal işleçler de birlikte kullanılabilir. 

Aşağıda, birden çok ifadeiçeren düzgün şekilde oluşturulmuş üyelik kurallarıörnekleri verilmiştir:

```
(user.department -eq "Sales") -or (user.department -eq "Marketing")
(user.department -eq "Sales") -and -not (user.jobTitle -contains "SDE")
```

### <a name="operator-precedence"></a>İşleç önceliği

Tüm işleçler en yüksekten en aşağıya doğru öncelik sırasına göre aşağıda listelenmiştir. Aynı hattaki işleçler eşit önceliğe dayanır:

```
-eq -ne -startsWith -notStartsWith -contains -notContains -match –notMatch -in -notIn
-not
-and
-or
-any -all
```

Aşağıda, kullanıcı için iki ifadenin değerlendirildiği işleç önceliği ne reisi verilmiştir:

```
   user.department –eq "Marketing" –and user.country –eq "US"
```

Parantezler yalnızca öncelik gereksinimlerinizi karşılamadığında gereklidir. Örneğin, önce bölümün değerlendirilmesini istiyorsanız, aşağıdaki parantezlerin düzeni belirlemek için nasıl kullanılabileceğini gösterir:

```
   user.country –eq "US" –and (user.department –eq "Marketing" –or user.department –eq "Sales")
```

## <a name="rules-with-complex-expressions"></a>Karmaşık ifadeler içeren kurallar

Üyelik kuralı, özelliklerin, işleçlerin ve değerlerin daha karmaşık biçimlere sahip olduğu karmaşık ifadelerden oluşabilir. Aşağıdakilerden herhangi biri doğru olduğunda ifadeler karmaşık olarak kabul edilir:

* Özellik, değerler koleksiyonundan oluşur; özellikle, çok değerli özellikleri
* İfadeler -any ve -all işleçlerini kullanır
* İfadenin değeri bir veya daha fazla ifade olabilir

## <a name="multi-value-properties"></a>Çok değerli özellikler

Çok değerli özellikler aynı türdeki nesnelerin koleksiyonlarıdır. Bunlar -any ve-all mantıksal işleçleri kullanarak üyelik kuralları oluşturmak için kullanılabilir.

| Özellikler | Değerler | Kullanım |
| --- | --- | --- |
| atananPlanlar | Koleksiyondaki her nesne aşağıdaki dize özelliklerini ortaya çıkarır: capabilityStatus, service, servicePlanId |user.assignedPlans -any (assignedPlan.servicePlanId -eq "efb87545-963c-4e0d-99df-69c6916d9eb0" -ve assignedPlan.capabilityStatus -eq "Enabled") |
| proxyAddresses| SMTP: alias@domain smtp:alias@domain | (user.proxyAdresleri -any\_ ( -içerir "contoso")) |

### <a name="using-the--any-and--all-operators"></a>-any ve -all operatörlerini kullanma

Koleksiyondaki öğelerden birine veya tümüne sırasıyla bir koşul uygulamak için -herhangi bir ve-tüm işleçleri kullanabilirsiniz.

* -herhangi bir (koleksiyondaki en az bir öğe koşulla eşleştiğinde memnun)
* -tüm (koleksiyondaki tüm öğeler koşulla eşleştiğinde memnun)

#### <a name="example-1"></a>Örnek 1

assignedPlans, kullanıcıya atanan tüm hizmet planlarını listeleyen çok değerli bir özelliktir. Aşağıdaki ifade, Etkin durumunda olan Exchange Online (Plan 2) hizmet planına (GUID değeri olarak) sahip kullanıcıları seçer:

```
user.assignedPlans -any (assignedPlan.servicePlanId -eq "efb87545-963c-4e0d-99df-69c6916d9eb0" -and assignedPlan.capabilityStatus -eq "Enabled")
```

Bunun gibi bir kural, Office 365 (veya diğer Microsoft Çevrimiçi Hizmeti) özelliğinin etkin olduğu tüm kullanıcıları gruplandırmak için kullanılabilir. Daha sonra gruba bir dizi ilke ile uygulayabilirsiniz.

#### <a name="example-2"></a>Örnek 2

Aşağıdaki ifade, Intune hizmetiyle ilişkili herhangi bir hizmet planı olan (hizmet adı "SCO" ile tanımlanan) tüm kullanıcıları seçer:

```
user.assignedPlans -any (assignedPlan.service -eq "SCO" -and assignedPlan.capabilityStatus -eq "Enabled")
```

### <a name="using-the-underscore-_-syntax"></a>Alt sözden gelen ( )\_sözdizimini kullanma

Alt nokta\_( ) sözdizimi, kullanıcıları veya aygıtları dinamik bir gruba eklemek için çok değerli dize toplama özelliklerinden birinde belirli bir değerin oluşumlarını eşleşir. Bu -herhangi veya -tüm operatörler ile kullanılır.

Aşağıda, user.proxyAddress'e dayalı\_üye eklemek için bir kuraldaki alt alt puanı kullanma ( ) örneği verilmiştir (user.otherMails için de aynı şekilde çalışır). Bu kural, gruba "contoso" içeren proxy adresi olan herhangi bir kullanıcı ekler.

```
(user.proxyAddresses -any (_ -contains "contoso"))
```

## <a name="other-properties-and-common-rules"></a>Diğer özellikler ve genel kurallar

### <a name="create-a-direct-reports-rule"></a>"Doğrudan raporlar" kuralı oluşturma

Bir yöneticinin tüm doğrudan raporlarını içeren bir grup oluşturabilirsiniz. Yöneticinin doğrudan raporları gelecekte değiştiğinde, grubun üyeliği otomatik olarak ayarlanır.

Doğrudan raporlar kuralı aşağıdaki sözdizimi kullanılarak oluşturulur:

```
Direct Reports for "{objectID_of_manager}"
```

Burada geçerli bir kural örneği nerede "62e19b97-8b3d-4d4a-a106-4ce66896a863" yöneticinin objectID olduğunu:

```
Direct Reports for "62e19b97-8b3d-4d4a-a106-4ce66896a863"
```

Aşağıdaki ipuçları kuralı düzgün kullanmanıza yardımcı olabilir.

- **Yönetici kimliği,** yöneticinin nesne kimliğidir. Yöneticinin **Profili'nde**bulunabilir.
- Kuralın çalışması için **Yönetici** özelliğinin kiracınızdaki kullanıcılar için doğru ayarlandıklarına emin olun. Kullanıcının **Profili'ndeki**geçerli değeri kontrol edebilirsiniz.
- Bu kural yalnızca yöneticinin doğrudan raporlarını destekler. Başka bir deyişle, yöneticinin doğrudan raporları *ve* raporlarıyla bir grup oluşturamazsınız.
- Bu kural diğer üyelik kurallarıyla birleştirilemez.

### <a name="create-an-all-users-rule"></a>"Tüm kullanıcılar" kuralı oluşturma

Üyelik kuralını kullanarak kiracı içindeki tüm kullanıcıları içeren bir grup oluşturabilirsiniz. Kullanıcılar gelecekte kiracıya eklendiğinde veya kaldırıldığında, grubun üyeliği otomatik olarak ayarlanır.

"Tüm kullanıcılar" kuralı -ne işleci ve null değeri kullanılarak tek ifade kullanılarak oluşturulur. Bu kural, B2B konuk kullanıcılarının yanı sıra üye kullanıcıları da gruba ekler.

```
user.objectId -ne null
```
Grubunuzun konuk kullanıcıları hariç tutmasını ve yalnızca kiracınızın üyelerini içermesini istiyorsanız, aşağıdaki sözdizimini kullanabilirsiniz:

```
(user.objectId -ne null) -and (user.userType -eq "Member")
```

### <a name="create-an-all-devices-rule"></a>"Tüm aygıtlar" kuralı nı oluşturma

Üyelik kuralını kullanarak kiracı içindeki tüm aygıtları içeren bir grup oluşturabilirsiniz. Aygıtlar gelecekte kiracıya eklendiğinde veya kaldırıldığında, grubun üyeliği otomatik olarak ayarlanır.

"Tüm Aygıtlar" kuralı -ne işleci ve null değeri kullanılarak tek ifade kullanılarak oluşturulur:

```
device.objectId -ne null
```

## <a name="extension-properties-and-custom-extension-properties"></a>Uzantı özellikleri ve özel uzatma özellikleri

Uzantı öznitelikleri ve özel uzantı özellikleri dinamik üyelik kurallarında dize özellikleri olarak desteklenir. [Uzantı öznitelikleri](https://docs.microsoft.com/graph/api/resources/onpremisesextensionattributes?view=graph-rest-1.0) şirket içi Window Server AD'den eşitlenir ve X'in 1 - 15'e eşit olduğu "ExtensionAttributeX" biçimini alır. Özellik olarak uzantı özniteliği kullanan bir kural örneği aşağıda verilmiştir:

```
(user.extensionAttribute15 -eq "Marketing")
```

[Özel uzantı özellikleri](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-sync-feature-directory-extensions) şirket içi Windows Server AD'den veya bağlı bir SaaS uygulamasından `user.extension_[GUID]_[Attribute]`senkronize edilir ve aşağıdakilerin biçimindedir:

* [GUID], Azure AD'de özelliği oluşturan uygulama için Azure AD'deki benzersiz tanımlayıcıdır
* [Öznitelik] oluşturulduğu gibi özelliğin adıdır

Özel uzantı özelliği kullanan bir kural örneği:

```
user.extension_c272a57b722d4eb29bfe327874ae79cb_OfficeNumber -eq "123"
```

Özel özellik adı, Grafik Gezgini'ni kullanarak bir kullanıcının özelliğini sorgulayarak ve özellik adını arayarak dizinde bulunabilir. Ayrıca, artık dinamik bir uygulama kimliği girmek ve dinamik bir üyelik kuralı oluştururken kullanılacak özel uzantı özelliklerinin tam listesini almak için dinamik kullanıcı grubu kural oluşturucusundaki **Özel uzantı özellikleri** bağlantısını al'ı seçebilirsiniz. Bu liste, bu uygulama için yeni özel uzantı özellikleri almak için de yenilenebilir.

## <a name="rules-for-devices"></a>Cihazlar için kurallar

Ayrıca, bir gruba üyelik için aygıt nesnelerini seçen bir kural da oluşturabilirsiniz. Hem kullanıcılara hem de aygıtlara grup üyesi olamazsınız. 

> [!NOTE]
> **OrganizationalUnit** özniteliği artık listelenmez ve kullanılmamalıdır. Bu dize Belirli durumlarda Intune tarafından ayarlanır, ancak Azure AD tarafından tanınmaz, bu nedenle bu özniteliğe dayalı gruplara hiçbir aygıt eklenmez.

> [!NOTE]
> systemlabels, Intune ile ayarlanamayan salt okunur bir özniteliktir.
>
> Windows 10 için deviceOSVersion özniteliğinin doğru biçimi aşağıdaki gibidir: (device.deviceOSVersion -eq "10.0.17763"). Biçimlendirme Get-MsolDevice PowerShell cmdlet ile doğrulanabilir.

Aşağıdaki aygıt öznitelikleri kullanılabilir.

 Aygıt özniteliği  | Değerler | Örnek
 ----- | ----- | ----------------
 hesapEtkin | gerçek yanlış | (device.accountEtkin -eq true)
 displayName | herhangi bir dize değeri |(device.displayName -eq "Rob iPhone")
 deviceOSType | herhangi bir dize değeri | (device.deviceOSType -eq "iPad") -veya (device.deviceOSType -eq "iPhone")<br>(device.deviceOSType -"AndroidEnterprise" içerir)<br>(device.deviceOSType -eq "AndroidForWork")
 deviceOSVersion | herhangi bir dize değeri | (device.deviceOSVersion -eq "9.1")
 Cihazlar | geçerli bir cihaz kategori adı | (device.deviceKategori -eq "BYOD")
 cihazÜretici | herhangi bir dize değeri | (device.deviceÜretici -eq "Samsung")
 deviceModel | herhangi bir dize değeri | (device.deviceModel -eq "iPad Air")
 deviceOwnership | Kişisel, Şirket, Bilinmeyen | (device.deviceOwnership -eq "Şirket")
 kayıtProfileName | Apple Cihazı Kayıt Profili, Cihaz kaydı - Kurumsal cihaz tanımlayıcıları (Android - Kiosk) veya Windows Otomatik Pilot profil adı | (device.enrollmentProfileName -eq "DEP iPhone")
 isRooted | gerçek yanlış | (device.isRooted -eq true)
 managementType | MDM (mobil cihazlar için)<br>PC (Intune PC aracısı tarafından yönetilen bilgisayarlar için) | (device.managementType -eq "MDM")
 deviceId | geçerli bir Azure AD aygıt kimliği | (device.deviceId -eq "d4fe7726-5966-431c-b3b8-cddc8fdb717d")
 Objectıd | geçerli bir Azure AD nesne kimliği |  (device.objectId -eq "76ad43c9-32c5-45e8-a272-7b58b58f596d")
 cihazPhysicalIds | Tüm Otomatik Pilot aygıtları, OrderID veya PurchaseOrderID gibi Otomatik Pilot tarafından kullanılan herhangi bir dize değeri  | (device.devicePhysicalIDs -any _ -içerir "[ZTDId]") (device.devicePhysicalIds -any _ -eq "[OrderID]:179887111881") (device.devicePhysicalIds -any _ -eq "[PurchaseOrderId]:76222342342")
 sistemEtiketler | Modern İşyeri aygıtlarını etiketlemek için Intune aygıt özelliğiyle eşleşen herhangi bir dize | (device.systemLabels -içerir "M365Yönetilen")

> [!Note]  
> Aygıtlar için Dinamik Gruplar oluştururken aygıtOwnership için değeri "Şirket" olarak eşit ayarlamanız gerekir. Intune'da aygıt sahipliği şirket olarak temsil edilir. Daha fazla bilgi için [OwnerTypes'a](https://docs.microsoft.com/intune/reports-ref-devices#ownertypes) bakın. 

## <a name="next-steps"></a>Sonraki adımlar

Bu makaleler, Azure Etkin Dizini'ndeki gruplar hakkında ek bilgiler sağlar.

- [Var olan grupları görme](../fundamentals/active-directory-groups-view-azure-portal.md)
- [Yeni grup oluşturma ve üye ekleme](../fundamentals/active-directory-groups-create-azure-portal.md)
- [Bir grubun ayarlarını yönetme](../fundamentals/active-directory-groups-settings-azure-portal.md)
- [Bir grubun üyeliklerini yönetme](../fundamentals/active-directory-groups-membership-azure-portal.md)
- [Bir gruptaki kullanıcılar için dinamik kuralları yönetme](groups-create-rule.md)
