---
title: 'Azure AD Connect: Eşitleme sırasında hata giderme | Microsoft Dokümanlar'
description: Azure AD Connect ile eşitleme sırasında karşılaşılan hataları nasıl gidereceklerini açıklar.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
ms.assetid: 2209d5ce-0a64-447b-be3a-6f06d47995f8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/29/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 745ddcc95bb91e61478307265aec1ac8a7ebba54
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75609205"
---
# <a name="troubleshooting-errors-during-synchronization"></a>Eşitleme sırasında karşılaşılan Hataları giderme
Kimlik verileri Windows Server Active Directory 'den (AD DS) Azure Active Directory 'ye (Azure AD) eşitlendiğinde hatalar oluşabilir. Bu makalede, eşitleme hataları farklı türleri, bu hataları ve hataları düzeltmek için olası yollar neden olası senaryoların bazıları genel bir bakış sağlar. Bu makalede, yaygın hata türlerini içerir ve tüm olası hataları kapsamayabilir.

 Bu makalede, okuyucunun Azure AD ve Azure AD Connect'in temel [tasarım kavramlarına](plan-connect-design-concepts.md)aşina olduğu varsayar.

Azure AD Connect'in \(Ağustos 2016\)veya daha yüksek sürümüyle, eşitlemek için Azure AD Bağlantı Durumu'nun bir parçası olarak [Azure portalında](https://aka.ms/aadconnecthealth) Eşitleme Hataları raporu mevcuttur.

1 Eylül 2016 tarihinden itibaren [Azure Active Directory Yinelenen Öznitelik Esneklik](how-to-connect-syncservice-duplicate-attribute-resiliency.md) özelliği varsayılan olarak tüm *yeni* Azure Active Directory Kiracıları için etkinleştirilir. Bu özellik, önümüzdeki aylarda mevcut kiracılar için otomatik olarak etkinleştirilecektir.

Azure AD Connect, senkronize tuttuğu dizinlerden üç tür işlem gerçekleştirir: Alma, Eşitleme ve Dışa Aktarma. Hatalar tüm işlemlerde yer alabilir. Bu makale, ağırlıklı olarak Azure AD'ye Dışa Aktarma sırasındaki hatalara odaklanır.

## <a name="errors-during-export-to-azure-ad"></a>Azure AD'ye Dışa Aktarma sırasında hatalar
Aşağıdaki bölümde, Azure AD bağlayıcısını kullanarak Azure AD'ye dışa aktarma işlemi sırasında oluşabilecek farklı türde eşitleme hataları açıklanır. Bu bağlayıcı " contoso" ad biçimi ile tanımlanabilir. *onmicrosoft.com*".
Azure AD'ye Dışa Aktarma \(sırasındaki hatalar,\) Azure Etkin Dizini'nde Azure AD Connect \(Sync Engine\) tarafından denenen ekleme, güncelleştirme, silme vb. işlemin başarısız olduğunu gösterir.

![Dışa Aktarma Hatalarına Genel Bakış](./media/tshoot-connect-sync-errors/Export_Errors_Overview_01.png)

## <a name="data-mismatch-errors"></a>Veri Uyuşmazlığı Hataları
### <a name="invalidsoftmatch"></a>GeçersizSoftMatch
#### <a name="description"></a>Açıklama
* Azure AD \(Connect\) eşitleme motoru Azure Active Directory'ye nesne eklemesini veya güncelleştirmesini söylediğinde, Azure AD, gelen nesneyle gelir, **kaynak Çapa** özniteliğini kullanarak Azure AD'deki nesnelerin **değişmez Özniteliğiyle** eşleşir. Bu maç **bir Sabit Maç**denir.
* Azure AD, yeni bir nesne sağlamadan önce, gelen nesnenin **kaynakÇapa** özniteliği ile **değişmezId** özniteliği eşleşen herhangi bir nesne **bulamazsa,** bir eşleşme bulmak için ProxyAdresleri ve UserPrincipalName özniteliklerini kullanmak için geri düşer. Bu eşleşmeye **Yumuşak Eşleşme**denir. Yumuşak Eşleme, Azure AD'de mevcut olan (Azure AD'de bulunan) nesneleri, eşzamanlı olarak aynı varlığı (kullanıcılar, gruplar) temsil eden eşitleme sırasında eklenen/güncelleştirilen yeni nesnelerle eşleşecek şekilde tasarlanmıştır.
* **InvalidSoftMatch** hatası, sabit eşleşme eşleşen bir nesne bulamazsa oluşur **ve** yumuşak eşleşme eşleşen bir nesne bulur, ancak bu nesne, gelen nesnenin *SourceAnchor'ından*farklı bir *değişmezId* değerine sahiptir ve eşleşen nesnenin şirket içinde bulunan Active Directory'den başka bir nesneyle senkronize edildiğini düşündürmektedir.

Başka bir deyişle, yumuşak eşleşmenin çalışabilmesi için, yumuşak eşleşecek nesnenin *değişmezId*için herhangi bir değeri olmamalıdır. Değeri sabit olan *herhangi* bir nesne zor eşleşmeyi başarısız lığa uğratıyor sayılsa da yumuşak eşleşme ölçütlerini karşılıyorsa, işlem Bir GeçersizSoftMatch eşitleme hatasına neden olur.

Azure Active Directory şeması, iki veya daha fazla nesnenin aşağıdaki özniteliklerle aynı değere sahip olmasını sağlamaz. \(Bu kapsamlı bir liste değil.\)

* ProxyAddresses
* UserPrincipalName
* onPremisesSecurityIdentifier
* ObjectId

> [!NOTE]
> [Azure AD Atfamacı Yinelenen Öznitelik Esneklik](how-to-connect-syncservice-duplicate-attribute-resiliency.md) özelliği de Azure Etkin Dizinin varsayılan davranışı olarak kullanıma sunuluyor.  Bu, Azure AD Connect tarafından görülen eşitleme hatalarının (ve diğer eşitleme istemcilerinin) sayısını, Azure AD'yi, yinelenen Proxy Adresleri ve UserPrincipalName özniteliklerini şirket içinde bulunan AD ortamlarında işleme şeklinde daha esnek hale getirerek azaltır. Bu özellik yineleme hatalarını düzeltmez. Yani verilerin hala düzeltilmesi gerekiyor. Ancak, Azure AD'de yinelenen değerler nedeniyle başka türlü engellenen yeni nesnelerin sağlanmasına olanak tanır. Bu, eşitleme istemcisine döndürülen eşitleme hatalarının sayısını da azaltır.
> Bu özellik Kiracınız için etkinleştirilirse, yeni nesnelerin sağlanması sırasında görülen Geçersiz SoftMatch eşitleme hatalarını görmezsiniz.
>
>

#### <a name="example-scenarios-for-invalidsoftmatch"></a>InvalidSoftMatch için Örnek Senaryolar
1. ProxyAddresses özniteliği için aynı değere sahip iki veya daha fazla nesne şirket içi Active Directory'de bulunur. Yalnızca biri Azure AD'de sağlanıyor.
2. UserPrincipalName özniteliği için aynı değere sahip iki veya daha fazla nesne şirket içi Active Directory'de bulunur. Yalnızca biri Azure AD'de sağlanıyor.
3. Şirket içi Active Directory'ye, Azure Etkin Dizini'ndeki varolan bir nesneyle proxy adresleri özniteliğiyle aynı değere sahip bir nesne eklendi. Şirket içinde eklenen nesne Azure Etkin Dizini'nde sağlanmıyor.
4. Şirket içinde, Azure Active Directory'deki bir hesapla kullanıcıPrincipalName özniteliği yle aynı değere sahip bir nesne dahili Active Directory eklendi. Nesne Azure Etkin Dizini'nde sağlanmıyor.
5. Senkronize edilmiş bir hesap, SourceAnchor'ı hesaplamak için ObjectGUID özniteliğini kullanarak Orman A'dan Orman B. Azure AD Connect'e (eşitleme motoru) taşındı. Orman hareket ettikten sonra, SourceAnchor değeri farklıdır. Yeni nesne (Orman B'den) Azure AD'deki varolan nesneyle eşitleneme de başaramaz.
6. Senkronize edilmiş bir nesne yanlışlıkla şirket içinde Active Directory'den silindi ve Azure Etkin Dizini'ndeki hesabı silmeden aynı varlık (kullanıcı gibi) için Active Directory'de yeni bir nesne oluşturuldu. Yeni hesap, varolan Azure AD nesnesiyle eşitleme de başarısız olur.
7. Azure AD Connect kaldırıldı ve yeniden yüklendi. Yeniden yükleme sırasında SourceAnchor olarak farklı bir öznitelik seçildi. Daha önce eşitlenmiş olan tüm nesneler InvalidSoftMatch hatasıyla eşitlenmeyi durdurdu.

#### <a name="example-case"></a>Örnek durum:
1. **Bob Smith,** Azure Active Directory'de şirket içi Active Directory of *contoso.com*
2. Bob Smith'in **UserPrincipalName** **bobs\@contoso.com**olarak ayarlanır.
3. **"abcdefghijklmnopqrstuv==",** Azure ACTIVE Directory'de Bob Smith'in **objectGUID'ini** kullanarak Azure **immutableId** AD Connect tarafından hesaplanan **SourceAnchor'dır.**
4. Bob da **proxyAddresses** özniteliği için aşağıdaki değerlere sahiptir:
   * Smtp:bobs@contoso.com
   * Smtp:bob.smith@contoso.com
   * **smtp:\@bob contoso.com**
5. Yeni bir kullanıcı, **Bob Taylor**, şirket içinde Active Directory eklenir.
6. Bob Taylor **KullanıcıPrincipalName** **\@bobt contoso.com**olarak ayarlanır.
7. **"abcdefghijkl0123456789==""** Azure AD Connect tarafından Bob Taylor'ın **objectGUID'ini** kullanarak hesaplanan **kaynaktır** Active Directory. Bob Taylor'ın nesnesi henüz Azure Active Directory ile senkronize edilmedi.
8. Bob Taylor proxyAddresses özniteliği için aşağıdaki değerlere sahiptir
   * Smtp:bobt@contoso.com
   * Smtp:bob.taylor@contoso.com
   * **smtp:\@bob contoso.com**
9. Eşitleme sırasında Azure AD Connect, Bob Taylor'ın şirket içi Active Directory'ye eklenmesini tanır ve Azure AD'den de aynı değişikliği yapmasını ister.
10. Azure AD ilk olarak zor eşleşmeyi gerçekleştirecektir. Yani, "abcdefghijkl0123456789="ye eşit değişmez id'li herhangi bir nesne olup olmadığını araştıracaktır. Azure AD'deki başka hiçbir nesne bu değişmezId'e sahip olmayacağından, Sabit Eşleşme başarısız olur.
11. Azure AD daha sonra Bob Taylor'ı yumuşak bir şekilde eşleştirmeye çalışır. Diğer bir zamanda, proxyAdresleri ile smtp dahil olmak üzere üç değere eşit herhangi bir nesne olup olmadığını arayacaktır:bob@contoso.com
12. Azure AD, Bob Smith'in nesnesini yumuşak eşleştirme ölçütlerine uyacak şekilde bulur. Ama bu nesne değişmezId değerine sahiptir = "abcdefghijklmnopqrstuv==". bu nesnenin başka bir nesneden şirket içinde Active Directory'den eşitlandığını gösterir. Bu nedenle, Azure AD bu nesneleri yumuşak bir şekilde eşleştiremez ve **Bir GeçersizSoftMatch** eşitleme hatasıyla sonuçlanır.

#### <a name="how-to-fix-invalidsoftmatch-error"></a>InvalidSoftMatch hatası nasıl düzeltilir?
GeçersizSoftMatch hatasının en yaygın nedeni, farklı SourceAnchor \(immutableId'e\) sahip iki nesnenin Azure AD'deki yumuşak eşleştirme işlemi sırasında kullanılan ProxyAdresleri ve/veya UserPrincipalName öznitelikleri için aynı değere sahip olmasıdır. Geçersiz Yumuşak Eşleşme'yi düzeltmek için

1. Hataya neden olan yinelenen proxyAdresleri, userPrincipalName veya diğer öznitelik değerini tanımlayın. Ayrıca, çatışmada hangi iki \(veya daha fazla\) nesnenin yer aldığını belirleyin. Azure AD Connect Health tarafından [eşitleme için](https://aka.ms/aadchsyncerrors) oluşturulan rapor, iki nesneyi belirlemenize yardımcı olabilir.
2. Yinelenen değere sahip olmaya devam etmesi gereken nesnenin hangisi olmaması gerektiğini belirleyin.
3. Yinelenen değeri bu değere sahip olmaması gereken nesneden kaldırın. Nesnenin kaynağının bulunduğu dizinde değişiklik yapmalısınız. Bazı durumlarda, çakışan nesnelerden birini silmeniz gerekebilir.
4. Şirket içi AD'de değişiklik yaptıysanız, Azure AD Connect'in değişikliği senkronize edin.

Eşitleme için Azure AD Connect Health içindeki eşitleme hata raporları her 30 dakikada bir güncellenir ve en son eşitleme denemesindeki hataları içerir.

> [!NOTE]
> ImmutableId, tanımı gereği, nesnenin ömrü içinde değişmemelidir. Azure AD Connect yukarıdaki listeden bazı senaryolar göz önünde bulundurularak yapılandırılmamışsa, Azure AD Connect'in, kullanmaya devam etmek istediğiniz varolan bir Azure AD Nesnesi olan aynı varlığı (aynı kullanıcı/grup/kişi vb.) temsil eden AD nesnesi için Kaynak Çapa'nın farklı bir değerini hesapladığı bir duruma kalabilirsiniz.
>
>

#### <a name="related-articles"></a>İlgili Makaleler
* [Yinelenen veya geçersiz öznitelikler, Office 365'te dizin eşitlemesini önler](https://support.microsoft.com/kb/2647098)

### <a name="objecttypemismatch"></a>ObjectTypeMismatch
#### <a name="description"></a>Açıklama
Azure AD iki nesneyi yumuşak eşleştirmeye çalıştığında, farklı "nesne türü" iki nesnenin (Kullanıcı, Grup, İlgili Kişi vb.) yumuşak eşleşmeyi gerçekleştirmek için kullanılan öznitelikler için aynı değerlere sahip olması mümkündür. Bu özniteliklerin çoğaltılmasına Azure AD'de izin verilmediğinden, işletmek "ObjectTypeMismatch" senkronizasyon hatasına neden olabilir.

#### <a name="example-scenarios-for-objecttypemismatch-error"></a>ObjectTypeMismatch hatası için örnek Senaryolar
* Office 365'te posta etkin leştirilmiş bir güvenlik grubu oluşturulur. Yönetici, Proxy Adresleri özniteliği için Office 365 grubuyla aynı değere sahip yeni bir kullanıcı veya ilgili kişi şirket içinde AD (henüz Azure AD'ye eşitlenmemiştir) ekler.

#### <a name="example-case"></a>Örnek durum
1. Yönetici, Vergi departmanı için Office 365'te yeni bir posta etkin tax@contoso.comgüvenlik grubu oluşturur ve e-posta adresi olarak . Bu gruba **\@smtp'nin** ProxyAdresleri öznitelik değeri atanır: vergi contoso.com
2. Contoso.com yeni bir kullanıcı katılır ve proxyAddress ile şirket içinde kullanıcı için bir hesap **oluşturulur: vergi\@contoso.com**
3. Azure AD Connect yeni kullanıcı hesabını eşitlediğinde "ObjectTypeMismatch" hatası alır.

#### <a name="how-to-fix-objecttypemismatch-error"></a>ObjectTypeMismatch hatası nasıl düzeltilir?
ObjectTypeMismatch hatasının en yaygın nedeni, proxyadresleri özniteliği için farklı türde iki nesnenin (Kullanıcı, Grup, Kişi vb.) aynı değere sahip olmasıdır. ObjectTypeMismatch düzeltmek için:

1. Hataya neden olan yinelenen proxyAdresleri (veya diğer öznitelik) değerini tanımlayın. Ayrıca, çatışmada hangi iki \(veya daha fazla\) nesnenin yer aldığını belirleyin. Azure AD Connect Health tarafından [eşitleme için](https://aka.ms/aadchsyncerrors) oluşturulan rapor, iki nesneyi belirlemenize yardımcı olabilir.
2. Yinelenen değere sahip olmaya devam etmesi gereken nesnenin hangisi olmaması gerektiğini belirleyin.
3. Yinelenen değeri bu değere sahip olmaması gereken nesneden kaldırın. Nesnenin kaynağının bulunduğu dizinde değişiklik yapmanız gerektiğini unutmayın. Bazı durumlarda, çakışan nesnelerden birini silmeniz gerekebilir.
4. Şirket içi AD'de değişiklik yaptıysanız, Azure AD Connect'in değişikliği senkronize edin. Eşitleme için Azure AD Connect Health içindeki eşitleme hata raporu her 30 dakikada bir güncellenir ve en son eşitleme denemesindeki hataları içerir.

## <a name="duplicate-attributes"></a>Yinelenen Öznitelikler
### <a name="attributevaluemustbeunique"></a>ÖznitelikValueMustBeUnique
#### <a name="description"></a>Açıklama
Azure Active Directory şeması, iki veya daha fazla nesnenin aşağıdaki özniteliklerle aynı değere sahip olmasını sağlamaz. Azure AD'deki her nesne, belirli bir örnekte bu özniteliklerin benzersiz bir değerine sahip olmaya zorlanır.

* ProxyAddresses
* UserPrincipalName

Azure AD Connect, Azure Etkin Dizini'nde başka bir nesneye atanmış olan yukarıdaki öznitelikler için bir değere sahip yeni bir nesne eklemeye veya varolan bir nesneyi güncelleştirmeye çalışırsa, işlem "AttributeValueMustBeUnique" eşitleme hatasıyla sonuçlanır.

#### <a name="possible-scenarios"></a>Olası Senaryolar:
1. Yinelenen değer, eşitlenmiş başka bir nesneyle çakışan zaten eşitlenmiş bir nesneye atanır.

#### <a name="example-case"></a>Örnek durum:
1. **Bob Smith,** Azure Active Directory'de şirket içi Active Directory of contoso.com
2. Bob Smith'in **UserPrincipalName** tesislerinde **bobs\@contoso.com**olarak ayarlanır.
3. Bob da **proxyAddresses** özniteliği için aşağıdaki değerlere sahiptir:
   * Smtp:bobs@contoso.com
   * Smtp:bob.smith@contoso.com
   * **smtp:\@bob contoso.com**
4. Yeni bir kullanıcı, **Bob Taylor**, şirket içinde Active Directory eklenir.
5. Bob Taylor **KullanıcıPrincipalName** **\@bobt contoso.com**olarak ayarlanır.
6. **Bob Taylor** **ProxyAddresses** özniteliği i için aşağıdaki değerlere sahiptir. smtp: bobt@contoso.com ii. Smtp:bob.taylor@contoso.com
7. Bob Taylor'ın nesnesi Azure AD ile başarıyla senkronize edilir.
8. Admin Aşağıdaki değeri ile Bob Taylor'S **ProxyAdresleri** özniteliği güncellemeye karar verdi: i. **smtp:\@bob contoso.com**
9. Azure AD, Azure AD'deki Bob Taylor nesnesini yukarıdaki değerle güncelleştirmeyi dener, ancak ProxyAdresleri değeri Zaten Bob Smith'e atandığı için bu işlem başarısız olur ve bu da "AttributeValueMustBeUnique" hatasına neden olur.

#### <a name="how-to-fix-attributevaluemustbeunique-error"></a>AttributeValueMustBeUnique hatası nasıl düzeltilir
AttributeValueMustBeUnique hatasının en yaygın nedeni, farklı SourceAnchor \(immutableId'e\) sahip iki nesnenin ProxyAdresleri ve/veya UserPrincipalName öznitelikleri için aynı değere sahip olmasıdır. AttributeValueMustBeUnique hata düzeltmek için

1. Hataya neden olan yinelenen proxyAdresleri, userPrincipalName veya diğer öznitelik değerini tanımlayın. Ayrıca, çatışmada hangi iki \(veya daha fazla\) nesnenin yer aldığını belirleyin. Azure AD Connect Health tarafından [eşitleme için](https://aka.ms/aadchsyncerrors) oluşturulan rapor, iki nesneyi belirlemenize yardımcı olabilir.
2. Yinelenen değere sahip olmaya devam etmesi gereken nesnenin hangisi olmaması gerektiğini belirleyin.
3. Yinelenen değeri bu değere sahip olmaması gereken nesneden kaldırın. Nesnenin kaynağının bulunduğu dizinde değişiklik yapmanız gerektiğini unutmayın. Bazı durumlarda, çakışan nesnelerden birini silmeniz gerekebilir.
4. Şirket içi AD'de değişiklik yaptıysanız, Azure AD Connect'in hatanın düzeltilmesi için değişikliği eşitlemasına izin verin.

#### <a name="related-articles"></a>İlgili Makaleler
-[Yinelenen veya geçersiz öznitelikler, Office 365'te dizin eşitlemesini önler](https://support.microsoft.com/kb/2647098)

## <a name="data-validation-failures"></a>Veri Doğrulama Hataları
### <a name="identitydatavalidationfailed"></a>IdentityDataValidationFailed
#### <a name="description"></a>Açıklama
Azure Etkin Dizin, bu verilerin dizine yazılmasına izin vermeden önce verilerin kendisi üzerinde çeşitli kısıtlamalar uygular. Bu kısıtlamalar, son kullanıcıların bu verilere bağlı uygulamaları kullanırken mümkün olan en iyi deneyimleri elde etmesini sağlamak içindir.

#### <a name="scenarios"></a>Senaryolar
a. UserPrincipalName öznitelik değeri geçersiz/desteklenmeyen karakterlere sahiptir.
b. UserPrincipalName özniteliği gerekli biçimi izlemez.

#### <a name="how-to-fix-identitydatavalidationfailed-error"></a>IdentityDataValidationFailed hatası nasıl düzeltilir?
a. userPrincipalName özniteliğinin desteklenen karakterlere ve gerekli biçime sahip olduğundan emin olun.

#### <a name="related-articles"></a>İlgili Makaleler
* [Office 365'e dizin eşitlemesi yoluyla kullanıcıları sağlama](https://support.office.com/article/Prepare-to-provision-users-through-directory-synchronization-to-Office-365-01920974-9e6f-4331-a370-13aea4e82b3e)

### <a name="federateddomainchangeerror"></a>FedereDomainChangeError
#### <a name="description"></a>Açıklama
Bu durum, bir kullanıcının UserPrincipalName'sinin sonekinin federe bir etki alanından başka bir federe etki alanına değiştirilmesiyle **"FedereEtkiChangeError"** eşitleme hatasıyla sonuçlanır.

#### <a name="scenarios"></a>Senaryolar
Senkronize bir kullanıcı için UserPrincipalName soneki, bir federe etki alanından şirket içinde başka bir federe etki alanına değiştirildi. Örneğin, *UserPrincipalName =\@bob contoso.com* *UserPrincipalName\@= bob fabrikam.com*olarak değiştirildi.

#### <a name="example"></a>Örnek
1. Bob Smith, Contoso.com için bir hesap, UserPrincipalName ile Active Directory yeni bir kullanıcı olarak eklenirbob@contoso.com
2. Bob, Fabrikam.com adı verilen Contoso.com farklı bir bölümüne taşınır ve UserPrincipalNamebob@fabrikam.com
3. Hem contoso.com hem de fabrikam.com etki alanları Azure Active Directory ile federe etki alanlarıdır.
4. Bob kullanıcısıPrincipalName güncellenmez ve "FedereDomainChangeError" eşitleme hatasıyla sonuçlanır.

#### <a name="how-to-fix"></a>Nasıl düzeltilir
Bir kullanıcının UserPrincipalName soneki, hem**contoso.com** contoso.com\@hem de **contoso.com** **fabrikam.com** **federe etki alanlarının**olduğu**fabrikam.com**contoso.com bob@ contoso.com güncelleştirildiyse, eşitleme hatasını düzeltmek için aşağıdaki adımları izleyin

1. Azure AD'de kullanıcının UserPrincipalName'sini bob@contoso.com bob@contoso.onmicrosoft.comgüncelleştirin. Azure AD PowerShell Modülü ile aşağıdaki PowerShell komutunu kullanabilirsiniz:`Set-MsolUserPrincipalName -UserPrincipalName bob@contoso.com -NewUserPrincipalName bob@contoso.onmicrosoft.com`
2. Eşitleme denemesi için bir sonraki eşitleme döngüsünün izin verin. Bu kez eşitleme başarılı olacak ve beklendiği bob@fabrikam.com gibi Bob Kullanıcı PrincipalName güncelleştirecektir.

#### <a name="related-articles"></a>İlgili Makaleler
* [Farklı bir federe etki alanı kullanmak için bir kullanıcı hesabının UPN'sini değiştirdikten sonra değişiklikler Azure Etkin Dizin Eşitleme aracı tarafından eşitlenmez](https://support.microsoft.com/help/2669550/changes-aren-t-synced-by-the-azure-active-directory-sync-tool-after-you-change-the-upn-of-a-user-account-to-use-a-different-federated-domain)

## <a name="largeobject"></a>Büyük Nesne
### <a name="description"></a>Açıklama
Bir öznitelik Azure Active Directory şeması tarafından ayarlanan izin verilen boyut sınırını, uzunluk sınırını veya sayım sınırını aştığında, eşitleme işlemi **Büyük Nesne** veya **ExceededAllowedLength** eşitleme hatasıyla sonuçlanır. Genellikle bu hata aşağıdaki öznitelikler için oluşur

* userCertificate
* userSMIMECertificate
* thumbnailPhoto
* proxyAddresses

### <a name="possible-scenarios"></a>Olası Senaryolar
1. Bob'un userCertificate özniteliği, Bob'a atanan çok fazla sertifika yı kakıyor. Bunlar eski, süresi dolmuş sertifikalar içerebilir. Sabit sınır 15 sertifikadır. UserCertificate özniteliği ile Büyük Nesne hatalarının nasıl işleyeceğiniz hakkında daha fazla bilgi için lütfen [userCertificate özniteliğinin neden olduğu Büyük Nesne hatalarını işleme](tshoot-connect-largeobjecterror-usercertificate.md)makalesine bakın.
2. Bob'un userSMIMECertificate özniteliği, Bob'a atanan çok fazla sertifika yı kakıyor. Bunlar eski, süresi dolmuş sertifikalar içerebilir. Sabit sınır 15 sertifikadır.
3. Bob'un Active Directory'deki küçük resmiAzure AD'de eşitlenemeyecek kadar büyüktür.
4. Active Directory'deki ProxyAdresleri özniteliğinin otomatik popülasyonu sırasında, bir nesnenin çok fazla ProxyAdresi atanmış olması vardır.

### <a name="how-to-fix"></a>Nasıl düzeltilir
1. Hataya neden olan özniteliğin izin verilen sınırlama içinde olduğundan emin olun.

## <a name="existing-admin-role-conflict"></a>Varolan Yönetici Rol Çakışması

### <a name="description"></a>Açıklama
Eşitleme sırasında bir kullanıcı nesnesi üzerinde varolan yönetici **rol çakışması** oluşur:

- idari izinler ve
- varolan bir Azure AD nesnesi ile aynı UserPrincipalName

Azure AD Connect'in, şirket içi AD'deki bir kullanıcı nesnesiyle Azure AD'de kendisine atanmış bir yönetim rolü olan bir kullanıcı nesnesiyle yumuşak eşleşmesine izin verilmez.  Daha fazla bilgi için azure [AD UserPrincipalName popülasyonuna](plan-connect-userprincipalname.md) bakın

![Varolan Yönetici](media/tshoot-connect-sync-errors/existingadmin.png)


### <a name="how-to-fix"></a>Nasıl düzeltilir
Bu sorunu çözmek için aşağıdakileri yapın:

1. Azure AD hesabını (sahibini) tüm yönetici rollerinden kaldırın. 
2. Hard Buluttaki Karantina nesnesini **silin.** 
3. Bir sonraki eşitleme döngüsü, şirket içi kullanıcıyı bulut hesabıyla yumuşak eşleştirmeyi sağlayacaktır (bulut kullanıcısı artık küresel bir GA olmadığından). 
4. Sahibi için rol üyelikleri geri yükleyin. 

>[!NOTE]
>Şirket içi kullanıcı nesnesi ile Azure AD kullanıcı nesnesi arasındaki yumuşak eşleşme tamamlandıktan sonra yönetim rolünü varolan kullanıcı nesnesine yeniden atayabilirsiniz.

## <a name="related-links"></a>İlgili bağlantılar
* [Active Directory Administrative Center'da Etkin Dizin Nesnelerini Bulma](https://technet.microsoft.com/library/dd560661.aspx)
* [Azure Active Directory PowerShell'i kullanarak bir nesne için Azure Etkin Dizin sorgulanma](https://msdn.microsoft.com/library/azure/jj151815.aspx)
