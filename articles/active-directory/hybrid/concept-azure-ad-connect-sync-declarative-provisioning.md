---
title: 'Azure AD Connect: bildirime dayalı sağlamayı anlama | Microsoft Docs'
description: Azure AD Connect bildirim temelli sağlama yapılandırma modelini açıklar.
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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "60246273"
---
# <a name="azure-ad-connect-sync-understanding-declarative-provisioning"></a>Azure AD Connect eşitleme: bildirime dayalı sağlamayı anlama
Bu konuda yapılandırma modeli Azure AD Connect açıklanmaktadır. Model, bildirim temelli sağlama olarak adlandırılır ve kolayca bir yapılandırma değişikliği yapmanıza olanak sağlar. Bu konuda açıklanan pek çok şey gelişmiş ve çoğu müşteri senaryosunda gerekli değildir.

## <a name="overview"></a>Genel Bakış
Bildirim temelli sağlama, kaynak bağlantılı dizinden gelen nesneleri işleme ve nesne ve özniteliklerin bir kaynaktan hedefe nasıl dönüştürülmesi gerektiğini belirler. Bir nesne, eşitleme ardışık düzeninde işlenir ve işlem hattı gelen ve giden kuralları için aynıdır. Bir gelen kuralı, bir bağlayıcı alanından meta veri deposuna ve bir giden kuralı metadize 'den bir bağlayıcı alanına kadar olur.

![Eşitleme işlem hattı](./media/concept-azure-ad-connect-sync-declarative-provisioning/sync1.png)  

İşlem hattının birkaç farklı modülü vardır. Her biri, nesne eşitlemede bir kavramdan sorumludur.

![Eşitleme işlem hattı](./media/concept-azure-ad-connect-sync-declarative-provisioning/pipeline.png)  

* Kaynak, kaynak nesne
* [Kapsam](#scope), kapsamdaki tüm eşitleme kurallarını bulur
* [Birleştir](#join), bağlayıcı alanı ile meta veri deposu arasındaki ilişkiyi belirler
* Dönüştür, özniteliklerin nasıl dönüştürülmesi gerektiğini ve akışını hesaplar
* [Öncelik](#precedence), çakışan öznitelik katkıları çözümleniyor
* Hedef nesne

## <a name="scope"></a>Kapsam
Kapsam modülü bir nesneyi değerlendiriyor ve kapsamdaki kuralları belirler ve işleme dahil edilmelidir. Nesnedeki öznitelik değerlerine bağlı olarak, farklı eşitleme kuralları kapsam içinde olacak şekilde değerlendirilir. Örneğin, Exchange posta kutusu olmayan devre dışı bırakılmış bir kullanıcının, bir posta kutusu olan etkin bir kullanıcıdan farklı kuralları vardır.  
![Kapsam](./media/concept-azure-ad-connect-sync-declarative-provisioning/scope1.png)  

Kapsam, gruplar ve yan tümceler olarak tanımlanır. Yan tümceler bir grup içindedir. Bir gruptaki tüm yan tümceler arasında bir mantıksal ve kullanılır. Örneğin, (departman = It ve Country = Danimarka). Gruplar arasında bir mantıksal OR kullanılır.

![Kapsam](./media/concept-azure-ad-connect-sync-declarative-provisioning/scope2.png)  
Bu resimdeki kapsam (departman = IT ve ülke = Danimarka) veya (ülke = Isveç) olarak okunmalıdır. Grup 1 veya Grup 2 doğru olarak değerlendiriliyorsa kural kapsamdadır.

Kapsam modülü aşağıdaki işlemleri destekler.

| İşlem | Açıklama |
| --- | --- |
| EŞITTIR, NOTEQUAL |Değerin öznitelikteki değere eşit olup olmadığını değerlendiren bir dize karşılaştırması. Birden çok değerli öznitelikler için bkz. ııN ve ıSNOTıN. |
| LESSTHAN, LESSTHAN_OR_EQUAL |Değerin öznitelikteki değerden küçük olup olmadığını değerlendiren bir dize karşılaştırması. |
| CONTAINS, NOTCONTAINS |Öznitelik içinde değer içinde bir yerde bir yerde bulunursa değerlendirilen bir dize karşılaştırması. |
| STARTSWITH, NOTSTARTSWITH |Değer, öznitelikteki değerin başında ise değeri değerlendiren bir dize karşılaştırması. |
| ENDSWITH, NOTENDSWITH |Değer, öznitelikteki değerin sonunda olup olmadığını değerlendiren bir dize karşılaştırması. |
| GREATERTHAN, GREATERTHAN_OR_EQUAL |Değerin öznitelikteki değerden daha büyük olup olmadığını değerlendiren bir dize karşılaştırması. |
| ıSNULL, ISNOTNULL |Özniteliğin nesneden eksik olup olmadığını değerlendirir. Öznitelik yoksa ve bu nedenle null ise, kural kapsamdadır. |
| ISIN, ıSNOTıN |Değerin tanımlı öznitelikte mevcut olup olmadığını değerlendirir. Bu işlem, EŞITTIR ve NOTQUAL ' ın çok değerli çeşitçeşididir. Özniteliğin birden çok değerli bir öznitelik olması gerekir ve değer herhangi bir öznitelik değerinde bulunursa, kural kapsam içinde yer alabilir. |
| ISBITSET, ISNOTBITSET |Belirli bir bitin ayarlanmış olup olmadığını değerlendirir. Örneğin, bir kullanıcının etkin veya devre dışı olup olmadığını görmek için userAccountControl içindeki bitleri değerlendirmek üzere kullanılabilir. |
| ıMEMBEROF, ISNOTMEMBEROF |Değer, bağlayıcı alanındaki bir gruba bir DN içermelidir. Nesne belirtilen grubun üyesiyse, kural kapsamdadır. |

## <a name="join"></a>Birleştir
Eşitleme işlem hattındaki JOIN modülü, kaynak içindeki nesne ve hedefteki bir nesne arasındaki ilişkiyi bulmaktan sorumludur. Gelen kuralında, bu ilişki, meta veri deposundaki bir nesneyle ilişki bulan bağlayıcı alanındaki bir nesne olur.  
![CS ve MV arasında Birleştir](./media/concept-azure-ad-connect-sync-declarative-provisioning/join1.png)  
Amaç, meta veri deposunda zaten başka bir bağlayıcı tarafından oluşturulmuş bir nesne olup olmadığını görebilmelidir. Örneğin, bir hesap-kaynak ormanında, hesap ormanındaki kullanıcının, kaynak ormandan Kullanıcı ile katılması gerekir.

Birleşimler, bağlayıcı alanı nesnelerini aynı meta veri deposu nesnesi ile birlikte birleştirmek için çoğunlukla gelen kurallarında kullanılır.

Birleşimler bir veya daha fazla grup olarak tanımlanır. Bir grup içinde yan tümceler vardır. Bir gruptaki tüm yan tümceler arasında bir mantıksal ve kullanılır. Gruplar arasında bir mantıksal OR kullanılır. Gruplar yukarıdan aşağıya doğru sırayla işlenir. Bir grup, hedefteki bir nesneyle tam olarak bir eşleşme bulmışsa, başka bir JOIN kuralı değerlendirilmez. Sıfır veya daha fazla nesne bulunursa, işleme bir sonraki kurallar grubuna devam eder. Bu nedenle, kurallar son olarak en açık ve daha belirsiz sırada oluşturulmalıdır.  
![Birleştir tanımı](./media/concept-azure-ad-connect-sync-declarative-provisioning/join2.png)  
Bu resimdeki birleştirmeler üstten alta işlenir. İlk olarak eşitleme işlem hattı, ÇalışanNo üzerinde bir eşleşme olup olmadığını görür. Aksi takdirde, ikinci kural, hesap adının nesnelere birlikte katılması için kullanılabilir olup olmadığını görür. Bu bir eşleşme değilse, üçüncü ve son kural Kullanıcı adı kullanılarak daha belirsiz bir eşleşmedir.

Tüm JOIN kuralları değerlendiriliyorsa ve tam olarak bir eşleşme yoksa, **Açıklama** sayfasındaki **bağlantı türü** kullanılır. Bu seçenek **sağlama**olarak ayarlandıysa, hedefteki yeni bir nesne oluşturulur.  
![Sağlama veya katma](./media/concept-azure-ad-connect-sync-declarative-provisioning/join3.png)  

Bir nesne, kapsamdaki JOIN kurallarına sahip tek bir eşitleme kuralına sahip olmalıdır. Birleştirmenin tanımlandığı birden fazla eşitleme kuralı varsa, bir hata oluşur. Öncelik, JOIN çakışmalarını çözmek için kullanılmaz. Bir nesnenin aynı gelen/giden yönle akacak özniteliklerin kapsamında bir JOIN kuralı olmalıdır. Hem gelen hem de giden özniteliklerini aynı nesneye akıtmak gerekirse, JOIN ile hem gelen hem de giden eşitleme kuralına sahip olmanız gerekir.

Giden birleşimin, hedef bağlayıcı alanına bir nesne sağlamayı denediğinde özel bir davranışı vardır. DN özniteliği öncelikle bir ters birleştirmeyi denemek için kullanılır. Hedef bağlayıcı alanında aynı DN 'ye sahip bir nesne zaten varsa, nesneler birleştirilir.

JOIN modülü, yeni bir eşitleme kuralı kapsama geldiğinde yalnızca bir kez değerlendirilir. Bir nesne katıldığında, birleştirme ölçütü artık karşılanmasa bile bu, katılmaz. Bir nesnenin katılmasını sağlamak istiyorsanız, nesneleri birleştiren eşitleme kuralının kapsam dışında olması gerekir.

### <a name="metaverse-delete"></a>Meta veri deposu silme
Bir meta veri deposu nesnesi, **bağlantı türü** **sağlama** veya **StickyJoin**olarak ayarlanan kapsamda tek bir eşitleme kuralı olduğu sürece kalır. Bir bağlayıcının meta veri deposuna yeni bir nesne sağlamasına izin verilmediği, ancak katıldığı zaman, meta veri deposu nesnesi silinmeden önce kaynakta silinmesi gerekir.

Meta veri deposu nesnesi silindiğinde, **sağlama** için işaretlenen bir giden eşitleme kuralıyla ilişkili tüm nesneler silme için işaretlenir.

## <a name="transformations"></a>Dönüşümler
Dönüşümler, özniteliklerin kaynaktan hedefe nasıl akacağınızı tanımlamak için kullanılır. Akışlar şu **akış türlerinden**birine sahip olabilir: doğrudan, sabit veya ifade. Doğrudan akış, öznitelik değerini ek dönüşümlere sahip olmayan olarak akar. Sabit değer belirtilen değeri ayarlar. Bir ifade, dönüştürmenin nasıl olması gerektiğini ifade etmek için bildirim temelli sağlama ifade dilini kullanır. İfade dilinin ayrıntıları, [bildirime dayalı sağlama ifade dilini anlama](concept-azure-ad-connect-sync-declarative-provisioning-expressions.md) konusunda bulunabilir.

![Sağlama veya katma](./media/concept-azure-ad-connect-sync-declarative-provisioning/transformations1.png)  

**Bir kez Uygula** onay kutusu, özniteliğin yalnızca nesne başlangıçta oluşturulduğunda ayarlanabileceğini tanımlar. Örneğin, bu yapılandırma yeni bir kullanıcı nesnesi için ilk parolayı ayarlamak üzere kullanılabilir.

### <a name="merging-attribute-values"></a>Öznitelik değerleri birleştiriliyor
Öznitelik akışlarında, birden çok değerli özniteliklerin farklı bağlayıcılardan birleştirilmesi gerekip gerekmediğini belirleme ayarı vardır. Varsayılan değer **Güncelleştir**' dir ve bu, en yüksek önceliğe sahip eşitleme kuralının kazandığını gösterir.

![Birleştirme türleri](./media/concept-azure-ad-connect-sync-declarative-provisioning/mergetype.png)  

Ayrıca **merge** ve **mergecaseduyarsızdır**. Bu seçenekler, farklı kaynaklardaki değerleri birleştirmenizi sağlar. Örneğin, çeşitli farklı ormanlardan üye veya proxyAddresses özniteliğini birleştirmek için kullanılabilir. Bu seçeneği kullandığınızda, bir nesnenin kapsamındaki tüm eşitleme kurallarının aynı birleştirme türünü kullanması gerekir. Bir bağlayıcıdan **güncelleştirme** tanımlayamazsınız ve başka bir bağlayıcıya **birleştiremezsiniz** . Deneme yaparsanız bir hata alırsınız.

**Merge** ve **mergecaseduyarsız** arasındaki fark, yinelenen öznitelik değerlerini işleme işlemidir. Eşitleme altyapısı, yinelenen değerlerin hedef özniteliğe eklenmediğinden emin olmanızı sağlar. **Mergecaseduyarsız**ile yinelenen değerler yalnızca büyük/küçük harf farklılığı olmayacak. Örneğin, hedef özniteliğinde hem "SMTP:bob@contoso.com" hem de "smtp:bob@contoso.com" öğesini görmemelisiniz. **Birleştirme** yalnızca tam değerlere ve yalnızca büyük/küçük harfli bir farklılık olduğunu fark eden birden fazla değere bakıyor.

**Replace** seçeneği **güncelleştirmesiyle**aynıdır, ancak kullanılmaz.

### <a name="control-the-attribute-flow-process"></a>Öznitelik akışı işlemini denetleme
Birden çok gelen eşitleme kuralı aynı meta veri deposu özniteliğine katkıda bulunmak üzere yapılandırıldığında, kazanan kişiyi tespit etmek için öncelik kullanılır. En yüksek önceliğe sahip eşitleme kuralı (en düşük sayısal değer) değeri katkıda bulunacak. Giden kuralları için de aynı olur. En yüksek önceliğe sahip eşitleme kuralı kazanır ve değeri bağlı dizine katkıda bulunun.

Bazı durumlarda, bir değer katkıda bulunmak yerine, eşitleme kuralı diğer kuralların nasıl davranması gerektiğini belirlemelidir. Bu durum için kullanılan bazı özel sabit değerler vardır.

Gelen eşitleme kuralları için, **null** değeri akışın katkıda bulunmak için bir değer olmadığını belirtmek için kullanılabilir. Daha düşük önceliğe sahip başka bir kural bir değer katkıda bulunabilir. Bir kural bir değere katkıda bulunulmadığı takdirde, meta veri deposu özniteliği kaldırılır. Giden bir kural için, tüm eşitleme kuralları işlendikten sonra son değer **null** ise, bu değer bağlı dizinde kaldırılır.

**AuthoritativeNull** değişmez değeri **null** ile benzerdir, ancak daha düşük öncelikli kuralların bir değer katkıda bulunabilmesine neden olan farkla aynıdır.

Öznitelik akışı, **ıgnorethisflow**da kullanabilir. Katkıda bulunmak için bir şey olmadığını belirten, NULL değere benzer. Fark, hedefte zaten varolan bir değeri kaldırmıyor. Öznitelik akışının hiç olmadığı anlaşılıyor.

Örnek aşağıda verilmiştir:

*Ad-Kullanıcı Exchange hibrit için* aşağıdaki akış bulunabilir:  
`IIF([cloudSOAExchMailbox] = True,[cloudMSExchSafeSendersHash],IgnoreThisFlow)`  
Bu ifade şöyle okunmalıdır: Kullanıcı posta kutusu Azure AD 'de bulunuyorsa, özniteliği Azure AD 'den AD 'ye Flow. Aksi takdirde, Active Directory bir şeyi geri Flow. Bu durumda, AD 'de mevcut değeri saklar.

### <a name="importedvalue"></a>Importedvalue
Öznitelik adının köşeli parantezler yerine tırnak içine alınması gerektiğinden, ımportedvalue işlevi tüm diğer işlevlerden farklı.  
`ImportedValue("proxyAddresses")`.

Genellikle eşitleme sırasında bir öznitelik beklenen değeri, henüz dışarı aktarılmamış olsa bile veya dışa aktarma sırasında ("Tower 'ın üstü") bir hata alındı olarak kullanır. Bir gelen eşitleme, bağlı bir dizine henüz ulaşan bir özniteliğin o zaman ona ulaştığını varsayar. Bazı durumlarda, yalnızca bağlı dizin ("hologram ve Delta içeri aktarma Kulesi") tarafından onaylanan bir değeri eşitlenmesi önemlidir.

Bu işleve örnek *olarak, ad – kullanıcının Exchange 'de ortak olduğu, içindeki*kullanıma hazır eşitleme kuralında bulabilirsiniz. Karma Exchange 'de, Exchange Online tarafından eklenen değerin yalnızca değerin başarıyla verildiğini teyit edildiğinde eşitlenmesi gerekir:  
`proxyAddresses` <- `RemoveDuplicates(Trim(ImportedValue("proxyAddresses")))`

## <a name="precedence"></a>Önceliği
Birkaç eşitleme kuralı hedefe aynı öznitelik değerini katkıda bulmaya çalıştığında, kazanan kişiyi tespit etmek için öncelik değeri kullanılır. En yüksek önceliğe sahip kural, en düşük sayısal değer, özniteliği bir çakışmada katkıda bulunur.

![Birleştirme türleri](./media/concept-azure-ad-connect-sync-declarative-provisioning/precedence1.png)  

Bu sıralama, küçük bir nesne alt kümesi için daha kesin öznitelik akışları tanımlamak üzere kullanılabilir. Örneğin, kutudan çıkış kuralları etkin bir hesaptan (**Kullanıcı muhasebeci**) özniteliklerin diğer hesaplardan öncelikli olduğundan emin olur.

Öncelik, bağlayıcılar arasında tanımlanabilir. Bu, öncelikle değerlerin katkıda bulunmak için daha iyi veri içeren bağlayıcılara izin verir

### <a name="multiple-objects-from-the-same-connector-space"></a>Aynı bağlayıcı alanından birden çok nesne
Aynı bağlayıcı alanında aynı meta veri deposu nesnesine katılmış birkaç nesneniz varsa, öncelik ayarlanmalıdır. Birden fazla nesne aynı eşitleme kuralının kapsamındeyse, eşitleme altyapısı önceliği belirleyemez. Bu değer, meta veri deposuna değeri hangi kaynak nesnenin katkıda bulunduğunu belirsizdir. Kaynaktaki öznitelikler aynı değere sahip olsa bile bu yapılandırma belirsiz olarak bildirilir.  
![Aynı MV nesnesine katılmış birden çok nesne](./media/concept-azure-ad-connect-sync-declarative-provisioning/multiple1.png)  

Bu senaryo için, kaynak nesnelerin kapsamda farklı eşitleme kurallarına sahip olması için eşitleme kurallarının kapsamını değiştirmeniz gerekir. Bu, farklı öncelik tanımlamanızı sağlar.  
![Aynı MV nesnesine katılmış birden çok nesne](./media/concept-azure-ad-connect-sync-declarative-provisioning/multiple2.png)  

## <a name="next-steps"></a>Sonraki adımlar
* [Bildirim temelli sağlama Ifadelerini anlama](concept-azure-ad-connect-sync-declarative-provisioning-expressions.md)bölümünde ifade dili hakkında daha fazla bilgi edinin.
* [Varsayılan yapılandırmayı anlamak için](concept-azure-ad-connect-sync-default-configuration.md)bildirim temelli sağlama 'nın nasıl kullanıldığını görün.
* [Varsayılan yapılandırmada değişiklik yapma konusunda](how-to-connect-sync-change-the-configuration.md)bildirim temelli sağlama kullanarak pratik bir değişiklik yapma konusuna bakın.
* Kullanıcıları ve kişileri [anlamak](concept-azure-ad-connect-sync-user-and-contacts.md)için kullanıcıların ve kişilerin birlikte nasıl çalıştığını okumaya devam edin.

**Genel Bakış konuları**

* [Azure AD Connect eşitleme: eşitlemeyi anlama ve özelleştirme](how-to-connect-sync-whatis.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md)

**Başvuru konuları**

* [Azure AD Connect Sync: Işlevler başvurusu](reference-connect-sync-functions-reference.md)
