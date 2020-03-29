---
title: 'Azure AD Connect: Bildirimsel Sağlamayı Anlama | Microsoft Dokümanlar'
description: Azure AD Connect'teki bildirimsel sağlama yapılandırma modelini açıklar.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: cfbb870d-be7d-47b3-ba01-9e78121f0067
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/13/2017
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 543c1a6706f794b81c4f93fc6fff3a61ed3fb9e3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60246273"
---
# <a name="azure-ad-connect-sync-understanding-declarative-provisioning"></a>Azure AD Connect eşitlemi: Bildirimsel Sağlama'yı Anlama
Bu konu, Azure AD Connect'teki yapılandırma modelini açıklar. Model Bildirimsel Sağlama denir ve kolaylıkla bir yapılandırma değişikliği yapmanızı sağlar. Bu konuda açıklanan birçok şey gelişmiştir ve çoğu müşteri senaryosu için gerekli değildir.

## <a name="overview"></a>Genel Bakış
Bildirimsel sağlama, kaynağa bağlı bir dizinden gelen nesneleri işliyor ve nesnenin ve özniteliklerin kaynaktan hedefe nasıl dönüştürülmesi gerektiğini belirler. Nesne eşitleme ardışık bir şekilde işlenir ve ardışık işlem, gelen ve giden kurallar için aynıdır. Gelen kural bir bağlayıcı alanından metaverse ve giden kural metaverse bir bağlayıcı alanı içindir.

![Eşitleme ardışık](./media/concept-azure-ad-connect-sync-declarative-provisioning/sync1.png)  

Boru hattının birkaç farklı modülü vardır. Her biri nesne senkronizasyonunda bir kavramdan sorumludur.

![Eşitleme ardışık](./media/concept-azure-ad-connect-sync-declarative-provisioning/pipeline.png)  

* Kaynak, Kaynak nesne
* [Kapsam](#scope), Kapsamda bulunan tüm eşitleme kurallarını bulur
* [Birleştirme](#join), bağlayıcı boşluk ve metaverse arasındaki ilişkiyi belirler
* Dönüştürme, özniteliklerin nasıl dönüştürülmesi ve akış la ilgili hesaplar
* [Öncelik](#precedence), Çakışan öznitelik katkılarını çözer
* Hedef, Hedef nesne

## <a name="scope"></a>Kapsam
Kapsam modülü bir nesneyi değerlendiriyor ve kapsamda olan ve işleme dahil edilmesi gereken kuralları belirler. Nesnedeki özniteliklere bağlı olarak, farklı eşitleme kuralları kapsamda olmak üzere değerlendirilir. Örneğin, Exchange posta kutusu olmayan engelli bir kullanıcının, posta kutusu olan etkin bir kullanıcıdan farklı kuralları vardır.  
![Kapsam](./media/concept-azure-ad-connect-sync-declarative-provisioning/scope1.png)  

Kapsam gruplar ve yan tümceler olarak tanımlanır. Yan tümceler bir grubun içinde. Mantıksal VE bir gruptaki tüm yan tümceler arasında kullanılır. Örneğin, (bölüm =IT AND ülke = Danimarka). Gruplar arasında mantıksal bir OR kullanılır.

![Kapsam](./media/concept-azure-ad-connect-sync-declarative-provisioning/scope2.png)  
Bu resimdeki kapsam (department = IT AND country = Danimarka) VEYA (ülke=İsveç) olarak okunmalıdır. Grup 1 veya grup 2 doğru değerlendirilirse, kural kapsam içindedir.

Kapsam modülü aşağıdaki işlemleri destekler.

| İşlem | Açıklama |
| --- | --- |
| EŞIT, EŞIT OLMAYAN |Değer öznitelikteki değere eşitse değerlendiren bir dize karşılaştırın. Çok değerli öznitelikler için ISIN ve ISNOTIN'e bakın. |
| AZ, LESSTHAN_OR_EQUAL |Değer öznitelikteki değerden daha az sayılsa bile değerlendiren bir dize karşılaştırın. |
| Içerir, IÇERMEZ |Öznitelikte değerin içinde bir yerde bulunabiliyorsa değerlendiren bir dize karşılaştırılır. |
| ILE BAŞLAR, NOTSTARTSWITH |Öznitelikteki değerin başında olup olmadığını değerlendiren bir dize karşılaştırın. |
| ENDSWITH, NOTENDS WITH |Öznitelikteki değerin sonunda değer olup olmadığını değerlendiren bir dize karşılaştırın. |
| GREATERTHAN, GREATERTHAN_OR_EQUAL |Değer öznitelikteki değerden daha büyükse değerlendiren bir dize karşılaştırın. |
| ISNULL, ISNOTNULL |Öznitelik nesnede yoksa değerlendirir. Öznitelik mevcut değilse ve bu nedenle null, o zaman kural kapsamı içindedir. |
| ISIN, ISNOTIN |Tanımlanan öznitelikte değer in ipi varsa değerlendirir. Bu işlem EQUAL ve NOTEQUAL'ın çok değerli varyasyonudur. Öznitelik çok değerli bir öznitelik olması gerekiyordu ve değer öznitelik değerlerinden herhangi birinde bulunabilir, o zaman kural kapsamı içindedir. |
| ISBITSET, ISNOTBITSET |Belirli bir bit in ayarlı olup olmadığını değerlendirir. Örneğin, kullanıcının etkin veya devre dışı bırakıldığını görmek için userAccountControl'deki bitleri değerlendirmek için kullanılabilir. |
| ISMEMBEROF, ISNOTMEMBEROF |Değer, bağlayıcı alanındaki bir gruba bir DN içermelidir. Nesne belirtilen grubun bir üyesiyse, kural kapsamdadır. |

## <a name="join"></a>Birleştir
Eşitleme ardışık ardışık modülü, kaynaktaki nesne ile hedefteki bir nesne arasındaki ilişkiyi bulmaktan sorumludur. Gelen bir kuralda, bu ilişki, bağlayıcı boşluktaki bir nesne nin metaverse'deki bir nesneyle ilişkisini bulması olacaktır.  
![cs ve mv arasında birleştirme](./media/concept-azure-ad-connect-sync-declarative-provisioning/join1.png)  
Amaç, metaverse zaten başka bir Bağlayıcı tarafından oluşturulan bir nesne olup olmadığını görmekiçin, bu ile ilişkili olmalıdır. Örneğin, bir hesap kaynağı ormanında, hesap ormanındaki kullanıcı, kaynak ormanındaki kullanıcıyla birleştirilmesi gerekir.

Birleştirmeler çoğunlukla gelen kurallarda, bağlayıcı alan nesnelerini aynı metaverse nesneyle birleştirmek için kullanılır.

Birleştirmeler bir veya daha fazla grup olarak tanımlanır. Bir grubun içinde yan tümceler vardır. Mantıksal VE bir gruptaki tüm yan tümceler arasında kullanılır. Gruplar arasında mantıksal bir OR kullanılır. Gruplar yukarıdan aşağıya doğru sırayla işlenir. Bir grup hedefteki bir nesneyle tam olarak bir eşleşme bulduğunda, başka birleştirme kuralları değerlendirilmez. Sıfır veya birden fazla nesne bulunursa, işleme sonraki kural grubuna devam edilir. Bu nedenle, kurallar sonunda en açık ilk ve daha bulanık sırasına göre oluşturulmalıdır.  
![Tanıma katılma](./media/concept-azure-ad-connect-sync-declarative-provisioning/join2.png)  
Bu resimdeki birleştirmeler yukarıdan aşağıya doğru işlenir. Önce eşitleme ardışık düzeni employeeID üzerinde bir eşleşme olup olmadığını görür. Değilse, ikinci kural hesap adının nesneleri biraraya getirmek için kullanılıp kullanılamayacağıgörür. Bu da bir eşleşme değilse, üçüncü ve son kural kullanıcı nın adını kullanarak daha bulanık bir eşleşmedir.

Tüm birleştirme kuralları değerlendirildiyse ve tam olarak bir eşleşme yoksa, **Açıklama** sayfasındaki **Bağlantı Türü** kullanılır. Bu seçenek **Provision**olarak ayarlanmışsa, hedefte yeni bir nesne oluşturulur.  
![Sağlama veya katılma](./media/concept-azure-ad-connect-sync-declarative-provisioning/join3.png)  

Bir nesnenin kapsamda birleştirme kuralları olan tek bir eşitleme kuralı olmalıdır. Birleştirme tanımlandığı birden çok eşitleme kuralı varsa, bir hata oluşur. Öncelik, birleştirme çakışmalarını çözmek için kullanılmaz. Özniteliklerin aynı gelen/giden yönde akması için bir nesnenin kapsamda bir birleştirme kuralı olması gerekir. Aynı nesneye hem gelen hem de giden öznitelikleri akması gerekiyorsa, birleştirme ile hem gelen hem de giden eşitleme kuralına sahip olmanız gerekir.

Giden birleştirme, hedef bağlayıcı alana bir nesne sağlamaya çalıştığında özel bir davranışa sahiptir. DN özniteliği ilk önce bir ters birleştirme denemek için kullanılır. Hedef bağlayıcı alanında aynı DN'ye sahip bir nesne varsa, nesneler birleştirilir.

Birleştirme modülü yalnızca yeni bir eşitleme kuralı kapsamına girdiğinde değerlendirilir. Bir nesne birleştiğinde, birleştirme ölçütleri artık tatmin olmasa bile ayrılmaz. Bir nesneye katılmak istiyorsanız, nesneleri birleştiren eşitleme kuralı kapsam dışına çıkmalıdır.

### <a name="metaverse-delete"></a>Metaverse silme
Bir metaverse **nesne, Provision** veya **StickyJoin**için ayarlanmış **Bağlantı Türü** ile kapsamda bir eşitleme kuralı olduğu sürece kalır. StickyJoin, bir Bağlayıcı'nın metaverse'e yeni bir nesne sağlamasına izin verilmediğinde kullanılır, ancak birleştiğinde metaverse nesnesi silinmeden önce kaynakta silinmesi gerekir.

Bir metaverse nesne sildiğinde, **hükmü** için işaretlenmiş giden eşitleme kuralı ile ilişkili tüm nesneler bir silme için işaretlenir.

## <a name="transformations"></a>Dönüşümler
Dönüşümler, özniteliklerin kaynaktan hedefe nasıl akması gerektiğini tanımlamak için kullanılır. Akışlar aşağıdaki **akış türlerinden**birine sahip olabilir: Doğrudan, Sabit veya İfade. Doğrudan akış, ek dönüşümler ile bir öznitelik değeri olarak akar. Sabit değer belirtilen değeri ayarlar. Bir ifade, dönüşümün nasıl olması gerektiğini ifade etmek için bildirimsel sağlama ifadesini kullanır. İfade dilinin [ayrıntılarını, ifade dili](concept-azure-ad-connect-sync-declarative-provisioning-expressions.md) konusunu anlama da bulunabilir.

![Sağlama veya katılma](./media/concept-azure-ad-connect-sync-declarative-provisioning/transformations1.png)  

**Uygula** onay kutusu, özniteliğin yalnızca nesne ilk oluşturulduğunda ayarlandığını tanımlar. Örneğin, bu yapılandırma yeni bir kullanıcı nesnesi için ilk parolayı ayarlamak için kullanılabilir.

### <a name="merging-attribute-values"></a>Öznitelik değerlerini birleştirme
Öznitelik akışlarında, çok değerli özniteliklerin birkaç farklı Bağlayıcıdan birleştirilmesi gerekip gerekmeden önce belirlenip birleştirilmemesi gerektiğini belirlemek için bir ayar vardır. Varsayılan değer, **Update**en yüksek önceliğe sahip eşitleme kuralının kazanması gerektiğini belirten Güncelleştirme'dir.

![Birleştirme Türleri](./media/concept-azure-ad-connect-sync-declarative-provisioning/mergetype.png)  

**Birleştirme** ve **BirleştirmeCaseInsensitive**de vardır. Bu seçenekler, farklı kaynaklardan gelen değerleri birleştirmenize olanak sağlar. Örneğin, üye veya proxyAdres özniteliğini birkaç farklı ormandan birleştirmek için kullanılabilir. Bu seçeneği kullandığınızda, bir nesne için kapsamdaki tüm eşitleme kurallarının aynı birleştirme türünü kullanması gerekir. Bir Bağlayıcıdan **Güncelleştirme** ve diğerinden **Birleştirme'yi** tanımlayamazsınız. Denerseniz, bir hata alırsınız.

**Birleştirme** ve **BirleştirmeCaseInsensitive** arasındaki fark, yinelenen öznitelik değerlerinin nasıl işlenir olduğudur. Eşitleme altyapısı, yinelenen değerlerin hedef özniteliğine eklenmemesini sağlar. **MergeCaseInsensitive**ile, yalnızca büyük bir fark ile yinelenen değerler mevcut olmayacaktır. Örneğin, hedef özyeğinde " veSMTP:bob@contoso.com"smtp:bob@contoso.comher ikisini de görmemelisiniz. **Birleştirme** yalnızca yalnızca bir durum farkı nın mevcut olabileceği tam değerlere ve birden çok değere bakıyor.

**Değiştir** seçeneği **Güncelleştirme**ile aynıdır, ancak kullanılmaz.

### <a name="control-the-attribute-flow-process"></a>Öznitelik akış sürecini denetleme
Birden çok gelen eşitleme kuralları aynı metaverse özniteliğe katkıda bulunmak üzere yapılandırılırsa, kazananı belirlemek için öncelik kullanılır. En yüksek önceliğe (en düşük sayısal değere) sahip eşitleme kuralı değere katkıda bulunacaktır. Aynı giden kurallar için de geçerlidir. En yüksek önceliğe sahip eşitleme kuralı kazanır ve bağlı dizine değer katar.

Bazı durumlarda, bir değer katkıda bulunmak yerine, eşitleme kuralı diğer kuralların nasıl olması gerektiğini belirlemelidir. Bu dava için kullanılan bazı özel edebi vardır.

Gelen Eşitleme Kuralları için, akış katkıda bulunmak için hiçbir değeri olduğunu belirtmek için gerçek **NULL** kullanılabilir. Daha düşük önceliğe sahip başka bir kural bir değer ekleyebilirsiniz. Hiçbir kural bir değer katkıda bulundu, o zaman metaverse özniteliği kaldırılır. Giden bir kural için, tüm eşitleme kuralları işlendikten sonra **NULL** son değerse, değer bağlı dizinde kaldırılır.

Literal **AuthoritativeNull** **NULL** benzer ama hiçbir düşük öncelik kuralları bir değer katkıda bulunabilir farkı ile.

Bir öznitelik akışı da **IgnoreThisFlow**kullanabilirsiniz. Bu katkıda bulunacak bir şey olmadığını gösterir anlamında NULL benzer. Fark, hedefte zaten varolan bir değeri kaldırmamasıdır. Bu öznitelik akışı orada hiç olmamıştı gibi.

Örnek aşağıda verilmiştir:

*Out to AD - User Exchange hibritinde* aşağıdaki akış bulunabilir:  
`IIF([cloudSOAExchMailbox] = True,[cloudMSExchSafeSendersHash],IgnoreThisFlow)`  
Bu ifade şu şekilde okunmalıdır: Kullanıcı posta kutusu Azure AD'de bulunuyorsa, özniteliği Azure AD'den AD'ye aktın. Değilse, Active Directory'ye hiçbir şey akmayın. Bu durumda, VAROlan DEĞERI AD'de tutar.

### <a name="importedvalue"></a>İthal Değer
Öznitelik adı kare ayraçyerine tırnak içinde eklenmelidir gerekir bu yana ImportedValue işlevi diğer tüm işlevlerden farklıdır:  
`ImportedValue("proxyAddresses")`.

Genellikle eşitleme sırasında bir öznitelik, henüz dışa aktarılmamış veya dışa aktarma sırasında bir hata alınmış olsa bile beklenen değeri kullanır ("kulenin üst kısmı"). Gelen eşitleme, henüz bağlı bir dizine ulaşmamış bir özniteliğin sonunda bu özneye ulaştığını varsayar. Bazı durumlarda, yalnızca bağlı dizin ("hologram ve delta alma kulesi") tarafından onaylanan bir değeri eşitlemek önemlidir.

Bu işlevin bir örneği, ad'dan kutudan çıkan Eşitleme Kuralı'nda bulunabilir *– Exchange'den Kullanıcı Ortak.* Hybrid Exchange'de, Exchange tarafından çevrimiçi olarak eklenen değer, yalnızca değerin başarıyla dışa aktarıldığı doğrulandığında eşitlenmelidir:  
`proxyAddresses` <- `RemoveDuplicates(Trim(ImportedValue("proxyAddresses")))`

## <a name="precedence"></a>Önceliği
Birkaç eşitleme kuralı hedefe aynı öznitelik değeri ne zaman katkıda bulunmaya çalışırsa, öncelik değeri kazananı belirlemek için kullanılır. En yüksek önceliğe, en düşük sayısal değere sahip kural, bir çakışma özniteliğine katkıda bulunacaktır.

![Birleştirme Türleri](./media/concept-azure-ad-connect-sync-declarative-provisioning/precedence1.png)  

Bu sıralama, nesnelerin küçük bir alt kümesi için daha kesin öznitelik akışları tanımlamak için kullanılabilir. Örneğin, kutu dışı kurallar, etkin bir hesaptaki özniteliklerin **(Kullanıcı Hesabı Etkin)** diğer hesaplardan önceolduğundan emin olun.

Öncelik Bağlayıcılar arasında tanımlanabilir. Bu, daha iyi veriye sahip Bağlayıcıların önce değerlere katkıda bulunmasını sağlar.

### <a name="multiple-objects-from-the-same-connector-space"></a>Aynı bağlayıcı alanından birden çok nesne
Aynı bağlayıcı alanda aynı metaverse nesneye birleştirilmiş birden fazla nesne varsa, öncelik ayarlanmalıdır. Birkaç nesne aynı eşitleme kuralı kapsamında ise, eşitleme altyapısı önceliği belirlemek mümkün değildir. Hangi kaynak nesnenin metaverse değeri katkıda bulunması gerektiği belirsizdir. Kaynaktaki öznitelikler aynı değere sahip olsa bile, bu yapılandırma belirsiz olarak bildirilir.  
![Aynı mv nesnesine birden çok nesne katıldı](./media/concept-azure-ad-connect-sync-declarative-provisioning/multiple1.png)  

Bu senaryo için, kaynak nesnelerin kapsamda farklı eşitleme kuralları olması için eşitleme kurallarının kapsamını değiştirmeniz gerekir. Bu, farklı bir öncelik tanımlamanızı sağlar.  
![Aynı mv nesnesine birden çok nesne katıldı](./media/concept-azure-ad-connect-sync-declarative-provisioning/multiple2.png)  

## <a name="next-steps"></a>Sonraki adımlar
* [Bildirimsel Hüküm Veren İfadeleri Anlama'da](concept-azure-ad-connect-sync-declarative-provisioning-expressions.md)ifade dili hakkında daha fazla bilgi edinin.
* [Varsayılan yapılandırmayı anlamada](concept-azure-ad-connect-sync-default-configuration.md)bildirimsel sağlamanın kullanıma hazır olarak nasıl kullanıldığını görün.
* [Varsayılan yapılandırmada değişiklik yapma](how-to-connect-sync-change-the-configuration.md)konusunda bildirimsel hükmü kullanarak pratik bir değişiklik yapma şekline bakın.
* Kullanıcıları ve [Kişileri Anlama'da](concept-azure-ad-connect-sync-user-and-contacts.md)kullanıcıların ve kişilerin birlikte nasıl çalıştığını okumaya devam edin.

**Genel bakış konuları**

* [Azure AD Connect eşitlemesi: Eşitlemeyi anlama ve özelleştirme](how-to-connect-sync-whatis.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md)

**Başvuru konuları**

* [Azure AD Connect eşitlemi: İşlevler Başvurusu](reference-connect-sync-functions-reference.md)
