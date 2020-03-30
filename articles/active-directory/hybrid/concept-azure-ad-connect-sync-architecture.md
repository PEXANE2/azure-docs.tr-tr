---
title: 'Azure AD Connect eşitlemi: Mimariyi anlama - Azure'
description: Bu konu, Azure AD Connect eşitlememimarisini açıklar ve kullanılan terimleri açıklar.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 465bcbe9-3bdd-4769-a8ca-f8905abf426d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/13/2017
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: fac0f9143918d3f273812e53abfb88d6a56f7a71
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79261625"
---
# <a name="azure-ad-connect-sync-understanding-the-architecture"></a>Azure AD Connect eşitlemi: Mimariyi anlama
Bu konu, Azure AD Connect eşitlemi için temel mimariyi kapsar. Birçok açıdan, selefleri MIIS 2003, ILM 2007 ve FIM 2010 benzer. Azure AD Connect eşitleme, bu teknolojilerin evrimidir. Bu önceki teknolojilerden herhangi birini biliyorsanız, bu konunun içeriği de size tanıdık gelecektir. Senkronizasyon için yeni iseniz, o zaman bu konu sizin için. Ancak, Azure AD Connect eşitlemesinde (bu konuda eşitleme altyapısı olarak adlandırılır) özelleştirmelerde başarılı olmak için bu konunun ayrıntılarını bilmek gerekli değildir.

## <a name="architecture"></a>Mimari
Eşitleme altyapısı, birden çok bağlı veri kaynağında depolanan nesnelerin tümleşik görünümünü oluşturur ve bu veri kaynaklarındaki kimlik bilgilerini yönetir. Bu tümleşik görünüm, bağlı veri kaynaklarından alınan kimlik bilgileri ve bu bilgilerin nasıl işlenirse nasıl işlenirbelirleyecek bir dizi kural tarafından belirlenir.

### <a name="connected-data-sources-and-connectors"></a>Bağlı Veri Kaynakları ve Bağlayıcılar
Eşitleme motoru, Active Directory veya SQL Server veritabanı gibi farklı veri depolarından kimlik bilgilerini işler. Verilerini veritabanı benzeri bir biçimde düzenleyen ve standart veri erişim yöntemleri sağlayan her veri deposu eşitleme altyapısı için potansiyel bir veri kaynağı adayıdır. Eşitleme altyapısı tarafından senkronize edilen veri depolarına **bağlı veri kaynakları** veya bağlı **dizinler** (CD) denir.

Eşitleme motoru, **bağlayıcı**adı verilen bir modül içinde bağlı bir veri kaynağıyla etkileşimi kapsüller. Bağlı veri kaynağının her türünde belirli bir Bağlayıcı vardır. Bağlayıcı, gerekli bir işlemi bağlı veri kaynağının anladığı biçime çevirir.

Bağlayıcılar, bağlı bir veri kaynağıyla kimlik bilgilerini (hem okuma hem de yazma) alışverişi yapmak için API çağrıları yapar. Genişletilebilir bağlantı çerçevesini kullanarak özel bir Bağlayıcı eklemek de mümkündür. Aşağıdaki resimde, bağlayıcının bağlı bir veri kaynağını eşitleme motoruna nasıl bağlandığı gösterilmektedir.

![Kemer1](./media/concept-azure-ad-connect-sync-architecture/arch1.png)

Veriler her iki yönde de akabilir, ancak aynı anda her iki yönde de akamaz. Başka bir deyişle, bağlayıcı, verilerin bağlı veri kaynağından eşitleme motoruna veya eşitleme altyapısından bağlı veri kaynağına akmasına izin verecek şekilde yapılandırılabilir, ancak bu işlemlerden yalnızca biri tek bir nesne ve öznitelik için herhangi bir anda oluşabilir. Yön farklı nesneler ve farklı öznitelikler için farklı olabilir.

Bağlayıcıyı yapılandırmak için, eşitlemek istediğiniz nesne türlerini belirtirsiniz. Nesne türlerini belirtmek, eşitleme işlemine dahil edilen nesnelerin kapsamını tanımlar. Bir sonraki adım, öznitelik ekleme listesi olarak bilinen eşitlemek için öznitelikleri seçmektir. Bu ayarlar, iş kurallarınızda yapılan değişikliklere yanıt olarak her zaman değiştirilebilir. Azure AD Connect yükleme sihirbazını kullandığınızda, bu ayarlar sizin için yapılandırılır.

Nesneleri bağlı bir veri kaynağına aktarmak için öznitelik ekleme listesi, bağlı bir veri kaynağında belirli bir nesne türü oluşturmak için gereken en az öznitelikleri içermelidir. Örneğin, Active Directory'deki tüm kullanıcı nesnelerinin bir **sAMAccountName** özniteliği tanımlanmış olması gerektiğinden, bir kullanıcı nesnesini Active Directory'ye aktarmak için **sAMAccountName** özniteliği öznitelik ekleme listesine eklenmelidir. Yine, yükleme sihirbazı sizin için bu yapılandırma yapar.

Bağlı veri kaynağı nesneleri düzenlemek için bölümler veya kapsayıcılar gibi yapısal bileşenler kullanıyorsa, bağlı veri kaynağındaki belirli bir çözüm için kullanılan alanları sınırlayabilirsiniz.

### <a name="internal-structure-of-the-sync-engine-namespace"></a>Eşitleme motoru ad alanının iç yapısı
Eşitleme motoru ad alanının tamamı, kimlik bilgilerini depolayan iki ad alanından oluşur. İki ad alanı şunlardır:

* Konektör alanı (CS)
* Metaverse (MV)

**Bağlayıcı alanı,** bağlı bir veri kaynağından atanan nesnelerin gösterimlerini ve öznitelik ekleme listesinde belirtilen öznitelikleri içeren bir hazırlama alanıdır. Eşitleme motoru, bağlı veri kaynağında nelerin değiştiğini belirlemek ve gelen değişiklikleri sahnelemek için bağlayıcı alanı kullanır. Eşitleme altyapısı, bağlı veri kaynağına dışa aktarma için giden değişiklikleri sahnelemek için bağlayıcı alanını da kullanır. Eşitleme motoru, her Bağlayıcı için bir hazırlama alanı olarak ayrı bir konektör alanı tutar.

Bir evreleme alanı kullanarak, eşitleme altyapısı bağlı veri kaynaklarından bağımsız kalır ve kullanılabilirlikve erişilebilirliklerinden etkilenmez. Sonuç olarak, evreleme alanındaki verileri kullanarak istediğiniz zaman kimlik bilgilerini işleyebilirsiniz. Eşitleme altyapısı, son iletişim oturumu sona erdiğinden beri yalnızca bağlı veri kaynağı içinde yapılan değişiklikleri isteyebilir veya yalnızca bağlı veri kaynağının henüz almadığı kimlik bilgilerindeki değişiklikleri dışarı itebilir ve bu da ağı azaltır eşitleme motoru ile bağlı veri kaynağı arasındaki trafik.

Ayrıca, eşitleme motoru, konektör alanında aşamalı olarak sahnelenebilen tüm nesnelerle ilgili durum bilgilerini depolar. Yeni veriler alındığı zaman, eşitleme altyapısı her zaman verilerin zaten eşitlenip eşitlenmediğini değerlendirir.

**Metaverse,** birden çok bağlı veri kaynağından toplanan kimlik bilgilerini içeren ve tüm birleştirilmiş nesnelerin tek bir küresel, tümleşik görünümünü sağlayan bir depolama alanıdır. Metaverse nesneler, bağlı veri kaynaklarından alınan kimlik bilgilerine ve eşitleme işlemini özelleştirmenize olanak tanıyan bir dizi kurala göre oluşturulur.

Aşağıdaki resimde, eşitleme motoru içindeki konektör alanı ad alanı ve metaverse ad alanı gösterilmektedir.

![Kemer2](./media/concept-azure-ad-connect-sync-architecture/arch2.png)

## <a name="sync-engine-identity-objects"></a>Motor kimlik nesnelerini eşitleme
Eşitleme altyapısındaki nesneler, bağlı veri kaynağındaki nesnelerin veya eşitleme altyapısının bu nesnelere sahip olduğu tümleşik görünümün gösterimleridir. Her eşitleme motoru nesnesi, genel olarak benzersiz bir tanımlayıcıya (GUID) sahip olmalıdır. GUI'ler veri bütünlüğü sağlar ve nesneler arasındaki ilişkileri ifade eder.

### <a name="connector-space-objects"></a>Bağlayıcı alan nesneleri
Eşitleme motoru bağlı bir veri kaynağıyla iletişim kurduğunda, bağlı veri kaynağındaki kimlik bilgilerini okur ve bağlayıcı uzayında kimlik nesnesinin bir temsilini oluşturmak için bu bilgileri kullanır. Bu nesneleri tek tek oluşturamaz veya silemezsiniz. Ancak, bağlayıcı alandaki tüm nesneleri el ile silebilirsiniz.

Bağlayıcı alanındaki tüm nesnelerin iki özelliği vardır:

* Genel olarak benzersiz tanımlayıcı (GUID)
* Ayırt edici bir ad (DN olarak da bilinir)

Bağlı veri kaynağı nesneye benzersiz bir öznitelik atarsa, bağlayıcı alanındaki nesnelerin de bir bağlantı özniteliği olabilir. Bağlantı özniteliği, bağlı veri kaynağındaki bir nesneyi benzersiz olarak tanımlar. Eşitleme motoru bağlı veri kaynağında bu nesnenin karşılık gelen temsilbulmak için çapa kullanır. Eşitleme motoru, bir nesnenin çapasının nesnenin ömrü boyunca asla değişmediğini varsayar.

Bağlayıcıların çoğu, içe aktarıldığında her nesne için otomatik olarak bir bağlantı oluşturmak için bilinen benzersiz bir tanımlayıcı kullanır. Örneğin, Etkin Dizin Bağlayıcısı bir çapa için **objectGUID** özniteliğini kullanır. Açıkça tanımlanmış benzersiz bir tanımlayıcı sağlamayan bağlı veri kaynakları için Bağlantı Oluşturma'yı Bağlayıcı yapılandırmasının bir parçası olarak belirtebilirsiniz.

Bu durumda, bağlantı, ikisi de değişmeyen ve bağlayıcı alanındaki nesneyi (örneğin çalışan numarası veya kullanıcı kimliği) benzersiz olarak tanımlayan bir nesne türünün bir veya daha fazla benzersiz özniteliklerinden oluşturulur.

Bağlayıcı uzay nesnesi aşağıdakilerden biri olabilir:

* Bir evreleme nesnesi
* Yer tutucu

### <a name="staging-objects"></a>Evreleme Nesneleri
Evreleme nesnesi, bağlı veri kaynağından atanan nesne türlerinin bir örneğini temsil eder. GUID ve ayırt edici ada ek olarak, bir evreleme nesnesi her zaman nesne türünü gösteren bir değere sahiptir.

İçe aktarılan evreleme nesnelerinin her zaman bağlantı özniteliği için bir değeri vardır. Eşitleme altyapısı tarafından yeni sağlanan ve bağlı veri kaynağında oluşturulma sürecinde olan evreleme nesnelerinin bağlantı özniteliği için bir değeri yoktur.

Evreleme nesneleri de iş özniteliklerinin geçerli değerlerini ve eşitleme işlemini gerçekleştirmek için eşitleme altyapısı tarafından gereken operasyonel bilgileri taşır. İşletim bilgileri, evreleme nesnesinde sahnelenen güncelleştirme türünü gösteren bayraklar içerir. Bir evreleme nesnesi henüz işlenmemiş bağlı veri kaynağından yeni kimlik bilgileri aldıysa, nesne **bekleyen içe aktarma**olarak işaretlenir. Bir evreleme nesnesi henüz bağlı veri kaynağına dışa aktarılmadı yeni kimlik bilgileri varsa, **bekleyen dışa aktarma**olarak işaretlenir.

Bir evreleme nesnesi bir alma nesnesi veya dışa aktarma nesnesi olabilir. Eşitleme altyapısı, bağlı veri kaynağından alınan nesne bilgilerini kullanarak bir alma nesnesi oluşturur. Eşitleme motoru Bağlayıcı'da seçilen nesne türlerinden biriyle eşleşen yeni bir nesnenin varlığı hakkında bilgi aldığında, bağlayıcı boşlukta bağlı veri kaynağındaki nesnenin temsili olarak bir alma nesnesi oluşturur.

Aşağıdaki resimde, bağlı veri kaynağındaki bir nesneyi temsil eden bir alma nesnesi gösterilmektedir.

![Kemer3](./media/concept-azure-ad-connect-sync-architecture/arch3.png)

Eşitleme altyapısı, metaverse nesne bilgilerini kullanarak bir dışa aktarma nesnesi oluşturur. Dışa aktarma nesneleri bir sonraki iletişim oturumunda bağlı veri kaynağına dışa aktarılır. Eşitleme altyapısı açısından, dışa aktarma nesneleri henüz bağlı veri kaynağında yok. Bu nedenle, bir dışa aktarma nesnesi için bağlantı özniteliği kullanılamaz. Nesneyi eşitleme altyapısından aldıktan sonra, bağlı veri kaynağı nesnenin bağlantı özniteliği için benzersiz bir değer oluşturur.

Aşağıdaki resimde, metaverse kimlik bilgileri kullanılarak bir dışa aktarma nesnesi nasıl oluşturulur gösterir.

![Arch4](./media/concept-azure-ad-connect-sync-architecture/arch4.png)

Eşitleme altyapısı, nesneyi bağlı veri kaynağından yeniden içe aktararak nesnenin dışa aktarılmasını onaylar. Dışa aktarma nesneleri, eşitleme altyapısı bağlı veri kaynağından sonraki alma sırasında bunları aldığında içe aktarma nesneleri olur.

### <a name="placeholders"></a>Yer Tutucular
Eşitleme altyapısı nesneleri depolamak için düz bir ad alanı kullanır. Ancak, Etkin Dizin gibi bazı bağlı veri kaynakları hiyerarşik bir ad alanı kullanır. Hiyerarşik ad alanından bilgileri düz bir ad alanına dönüştürmek için eşitleme altyapısı hiyerarşiyi korumak için yer tutucular kullanır.

Her yer tutucu, bir nesnenin hiyerarşik adının eşitleme altyapısına alınmamış, ancak hiyerarşik adı oluşturmak için gereken bir bileşenini (örneğin, bir kuruluş birimi) temsil eder. Bağlı veri kaynağındaki başvurular tarafından oluşturulan boşlukları bağlayıcı uzayında nesneleri evreleme olmayan nesnelere doldururlar.

Eşitleme motoru, henüz alınmamış başvurulan nesneleri depolamak için yer tutucular da kullanır. Örneğin, eşitleme *Abbie Spencer* nesnesi için yönetici özniteliği içerecek şekilde yapılandırılır ve alınan değer *cn=Lee Sperry,CN=Users,DC=fabrikam,DC=com*gibi henüz alınmamış bir nesneyse, yönetici bilgileri bağlayıcı alanında yer tutucu olarak depolanır. Yönetici nesnesi daha sonra içe aktarılırsa, yer tutucu nesne, yöneticiyi temsil eden evreleme nesnesi tarafından üzerine yazılır.

### <a name="metaverse-objects"></a>Metaverse nesneler
Metaverse nesne, eşitleme altyapısının konektör uzayında evreleme nesnelerinin sahip olduğu toplu görünümü içerir. Eşitleme altyapısı, alma nesnelerindeki bilgileri kullanarak metaverse nesneler oluşturur. Birkaç bağlayıcı uzay nesnesi tek bir metaverse nesneye bağlanabilir, ancak bağlayıcı uzay nesnesi birden fazla metaverse nesneye bağlanamaz.

Metaverse nesneler el ile oluşturulamaz veya silinemez. Eşitleme motoru, bağlayıcı uzasındaki herhangi bir bağlayıcı uzay nesnesine bağlantısı olmayan metaverse nesneleri otomatik olarak siler.

Bağlı bir veri kaynağı içindeki nesneleri metaverse içinde karşılık gelen bir nesne türüne eşlemek için eşitleme altyapısı, önceden tanımlanmış nesne türleri ve ilişkili öznitelikler kümesiyle genişletilebilir bir şema sağlar. Metaverse nesneler için yeni nesne türleri ve öznitelikleri oluşturabilirsiniz. Öznitelikler tek değerli veya çok değerli olabilir ve öznitelik türleri dizeleri, başvurular, sayılar ve Boolean değerleri olabilir.

### <a name="relationships-between-staging-objects-and-metaverse-objects"></a>Evreleme nesneleri ve metaverse nesneler arasındaki ilişkiler
Eşitleme motoru ad alanı içinde, veri akışı evreleme nesneleri ve metaverse nesneler arasındaki bağlantı ilişkisi tarafından etkindir. Metaverse nesnesine bağlı bir evreleme nesnesine **birleştirilmiş nesne** (veya **bağlayıcı nesne)** denir. Metaverse nesnesine bağlı olmayan bir evreleme **nesnesine, birleştirilmiş nesne** (veya **bağlantı kesecisi nesne)** denir. Birleştirilmiş ve birleşen terimler, bağlı bir dizinden veri alma ve dışa aktarmadan sorumlu Bağlayıcılarla karıştırılmamak için tercih edilir.

Yer tutucular hiçbir zaman metaverse nesnesine bağlanmaz

Birleştirilmiş nesne, bir evreleme nesnesi ve onun tek bir metaverse nesneyle olan ilişkisini içerir. Birleştirilmiş nesneler, bağlayıcı uzay nesnesi ile metaverse nesnesi arasındaki öznitelik değerlerini eşitlemek için kullanılır.

Bir evreleme nesnesi eşitleme sırasında birleştirilmiş nesne olduğunda, öznitelikler evreleme nesnesi ile metaverse nesnesi arasında akabilir. Öznitelik akışı çift yönlüdür ve alma öznitelik kuralları ve dışa aktarım özniteliği kuralları kullanılarak yapılandırılır.

Tek bir bağlayıcı uzay nesnesi yalnızca bir metaverse nesneye bağlanabilir. Ancak, her metaverse nesne, aşağıdaki resimde gösterildiği gibi, aynı veya farklı bağlayıcı alanlarda birden çok bağlayıcı uzay nesnelerine bağlanabilir.

![Kemer5](./media/concept-azure-ad-connect-sync-architecture/arch5.png)

Evreleme nesnesi ile metaverse nesnesi arasındaki bağlantılı ilişki kalıcıdır ve yalnızca belirttiğiniz kurallarla kaldırılabilir.

Birleştirilmiş nesne, herhangi bir metaverse nesneye bağlı olmayan bir evreleme nesnesidir. Birleştirilmiş nesnenin öznitelik değerleri metaverse içinde daha fazla işlenmez. Bağlı veri kaynağında karşılık gelen nesnenin öznitelik değerleri eşitleme altyapısı tarafından güncelleştirilmez.

Bire birleştirilmiş nesneleri kullanarak, kimlik bilgilerini eşitleme altyapısında depolayabilir ve daha sonra işleyebilirsiniz. Bir evreleme nesnesinin bağlayıcı alanda birleştirilmiş nesne olarak tutulmasının birçok avantajı vardır. Sistem bu nesne hakkında gerekli bilgileri zaten aşamalı olduğundan, bağlı veri kaynağından bir sonraki aktarım sırasında bu nesnenin bir temsilini yeniden oluşturmak gerekli değildir. Bu şekilde, eşitleme altyapısı, bağlı veri kaynağına geçerli bir bağlantı olmasa bile, her zaman bağlı veri kaynağının tam bir anlık görüntüsüne sahiptir. Birebir edilen nesneler, belirttiğiniz kurallara bağlı olarak birleştirilmiş nesnelere dönüştürülebilir ve bunun tersi de olabilir.

Bir içe aktarma nesnesi, birebir nesne olarak oluşturulur. Dışa aktarma nesnesi birleştirilmiş bir nesne olmalıdır. Sistem mantığı bu kuralı zorlar ve birleştirilmiş nesne olmayan her dışa aktarma nesnesini siler.

## <a name="sync-engine-identity-management-process"></a>Eşitleme motoru kimlik yönetimi süreci
Kimlik yönetimi işlemi, kimlik bilgilerinin farklı bağlı veri kaynakları arasında nasıl güncelleştirilebildiğini denetler. Kimlik yönetimi üç süreçte gerçekleşir:

* İçeri Aktarma
* Eşitleme
* Dışarı Aktarma

Alma işlemi sırasında, eşitleme motoru bağlı bir veri kaynağından gelen kimlik bilgilerini değerlendirir. Değişiklikler algılandığında, yeni evreleme nesneleri oluşturur veya eşitleme için bağlayıcı alanda varolan evreleme nesnelerini güncelleştirir.

Eşitleme işlemi sırasında, eşitleme motoru konektör alanında meydana gelen değişiklikleri yansıtacak şekilde metaverse güncelleştirir ve metaverse meydana gelen değişiklikleri yansıtacak şekilde bağlayıcı alanı güncelleştirir.

Dışa aktarma işlemi sırasında, eşitleme motoru, evreleme nesnelerinde sahnelenen ve bekleyen dışa aktarma olarak işaretlenen değişiklikleri dışarı iter.

Aşağıdaki resimde, kimlik bilgileri bağlı bir veri kaynağından diğerine akarken, işlemlerin her birinin nerede oluştuğu gösterilmektedir.

![Arch6](./media/concept-azure-ad-connect-sync-architecture/arch6.png)

### <a name="import-process"></a>İthalat süreci
Alma işlemi sırasında, eşitleme motoru kimlik bilgilerinin güncelleştirmelerini değerlendirir. Eşitleme altyapısı, bağlı veri kaynağından alınan kimlik bilgilerini bir evreleme nesnesi hakkındaki kimlik bilgileriyle karşılaştırır ve evreleme nesnesinin güncelleştirmegerekip gerekmediğini belirler. Evreleme nesnesini yeni verilerle güncelleştirmek gerekiyorsa, evreleme nesnesi bekleyen içe aktarma olarak işaretlenir.

Eşitlemeden önce bağlayıcı alanda nesneleri evreleme, eşitleme motoru yalnızca değiştirilen kimlik bilgilerini işleyebilir. Bu işlem aşağıdaki yararları sağlar:

* **Verimli eşitleme.** Eşitleme sırasında işlenen veri miktarı en aza indirilir.
* **Verimli yeniden senkronizasyon.** Eşitleme motorunun kimlik bilgilerini veri kaynağına yeniden bağlamadan nasıl işlediğini değiştirebilirsiniz.
* **Eşitleme önizleme fırsatı.** Kimlik yönetimi işlemi yle ilgili varsayımlarınızın doğru olduğunu doğrulamak için eşitlemayı önizleyebilirsiniz.

Bağlayıcıda belirtilen her nesne için, eşitleme motoru önce Bağlayıcının bağlayıcı alanında nesnenin bir temsilini bulmaya çalışır. Eşitleme motoru bağlayıcı alanındaki tüm evreleme nesnelerini inceler ve eşleşen bir çapa özelliğine sahip karşılık gelen bir evreleme nesnesi bulmaya çalışır. Varolan hiçbir evreleme nesnesi eşleşen bir bağlantı özelliği ne varsa, eşitleme motoru aynı ayırt edici ada sahip karşılık gelen bir evreleme nesnesi bulmaya çalışır.

Eşitleme altyapısı, çapayla değil, ayırt edici ada göre eşleşen bir evreleme nesnesi bulduğunda, aşağıdaki özel davranış oluşur:

* Bağlayıcı alanında bulunan nesnenin çapası yoksa, eşitleme motoru bu nesneyi bağlayıcı uzayından kaldırır ve bir **sonraki eşitleme çalışmasında yeniden deneolarak**bağlı olduğu metaverse nesneyi işaretler. Sonra yeni alma nesnesi oluşturur.
* Bağlayıcı alanında bulunan nesnenin bir çapası varsa, eşitleme motoru bu nesnenin bağlı dizinde yeniden adlandırıldığını veya silindiğini varsayar. Gelen nesneyi sahneleyebilmek için bağlayıcı alan nesnesi için geçici, yeni bir ayırt edici ad atar. Eski nesne daha sonra **geçici**olur, Konektör'ün durumu gidermek için yeniden adlandırmaveya silme işlemini almasını bekler.

Eşitleme altyapısı Bağlayıcı'da belirtilen nesneye karşılık gelen bir evreleme nesnesi bulursa, ne tür değişiklikler uygulanacağı belirler. Örneğin, eşitleme altyapısı bağlı veri kaynağındaki nesneyi yeniden adlandırabilir veya silebilir veya yalnızca nesnenin öznitelik değerlerini güncelleyebilir.

Güncelleştirilmiş verilerle nesneleri evreleme bekleyen alma olarak işaretlenir. Bekleyen içeri alma ların farklı türleri mevcuttur. Alma işleminin sonucuna bağlı olarak, bağlayıcı alanındaki bir evreleme nesnesi aşağıdaki bekleyen alma türlerinden birine sahiptir:

* **Hiç yok.** Evreleme nesnesinin özniteliklerinin hiçbirinde değişiklik yok. Eşitleme altyapısı bu türü bekleyen alma olarak işaretlemez.
* **Ekle**. Evreleme nesnesi bağlayıcı alanda yeni bir alma nesnesidir. Eşitleme motoru metaverse ek işleme için bekleyen içe aktarma olarak bu türü bayraklar.
* **Güncelleştirin.** Eşitleme altyapısı bağlayıcı alanında karşılık gelen bir evreleme nesnesi bulur ve özniteliklere güncelleştirmelerin metaverse işlenebilir, böylece bekleyen alma olarak bu türü bayraklar. Güncelleştirmeler nesne yeniden adlandırma içerir.
* **Sil.** Eşitleme altyapısı bağlayıcı alanında karşılık gelen bir evreleme nesnesi bulur ve birleşen nesnenin silinebilmeleri için bu türü bekleyen alma olarak işaretler.
* **Sil/Ekle.** Eşitleme motoru bağlayıcı alanında karşılık gelen bir evreleme nesnesi bulur, ancak nesne türleri eşleşmiyor. Bu durumda, bir silme-ekle değişikliği sahnelenir. Sil-ekle değişikliği eşitleme motoruna, nesne türü değiştiğinde bu nesneye farklı kural kümeleri uygulandığından, bu nesnenin tam bir yeniden eşitlemesi nin gerçekleşmesi gerektiğini gösterir.

Bir evreleme nesnesinin bekleyen içe aktarma durumunu ayarlayarak, eşitleme sırasında işlenen veri miktarını önemli ölçüde azaltmak mümkündür, çünkü bunu yapmak sistemin yalnızca verileri güncelleştirmiş nesneleri işlemesine olanak tanır.

### <a name="synchronization-process"></a>Eşitleme işlemi
Eşitleme iki ilgili işlemden oluşur:

* Gelen senkronizasyon, metaverse içeriği bağlayıcı alanında veri kullanılarak güncelleştirildiğinde.
* Giden senkronizasyon, bağlayıcı alanının içeriği metaverse verileri kullanılarak güncelleştirildiğinde.

Gelen eşitleme işlemi, bağlayıcı alanda sahnelenen bilgileri kullanarak, metaverse bağlı veri kaynaklarında depolanan verilerin tümleşik görünümünü oluşturur. Tüm evreleme nesneleri veya yalnızca bekleyen alma bilgileri ne kadar süreyle kurallara göre yapılandırıldığına bağlı olarak toplanır.

Giden eşitleme işlemi, metaverse nesneler değiştiğinde dışa aktarma nesneleri güncelleştirir.

Gelen eşitleme, bağlı veri kaynaklarından alınan kimlik bilgilerinin metaverse tümleşik görünümünü oluşturur. Eşitleme motoru, bağlı veri kaynağından aldığı en son kimlik bilgilerini kullanarak kimlik bilgilerini herhangi bir zamanda işleyebilir.

**Gelen eşitleme**

Gelen eşitleme aşağıdaki işlemleri içerir:

* **Hüküm** (bu işlemi giden eşitleme sağlamadan ayırmak önemliyse **Projeksiyon** olarak da adlandırılır). Eşitleme altyapısı, bir evreleme nesnesini temel alan yeni bir metaverse nesne oluşturur ve bunları bağlar. Hüküm nesne düzeyinde bir işlemdir.
* **Katılın.** Eşitleme motoru, bir evreleme nesnesini varolan bir metaverse nesneye bağlar. Birleştirme nesne düzeyinde bir işlemdir.
* **Öznitelik akışını alma.** Eşitleme motoru, metaverse nesnenin öznitelik akışı olarak adlandırılan öznitelik değerlerini güncelleştirir. Alma öznitelik akışı, bir evreleme nesnesi ile metaverse nesnesi arasında bağlantı gerektiren öznitelik düzeyinde bir işlemdir.

Sağlama, metaverse nesneleri oluşturan tek işlemdir. Sağlama yalnızca birebir çevrilmiş nesneleri alma nesnelerini etkiler. Sağlama sırasında, eşitleme altyapısı alma nesnesinin nesne türüne karşılık gelen bir metaverse nesne oluşturur ve her iki nesne arasında bir bağlantı oluşturarak birleştirilmiş nesne oluşturur.

Birleştirme işlemi, alma nesneleri ile metaverse nesnesi arasında da bir bağlantı kurar. Birleştirme ve sağlama arasındaki fark, birleştirme işleminin alma nesnesinin, sağlama işleminin yeni bir metaverse nesnesi oluşturduğu varolan bir metaverse nesneye bağlanmasını gerektirmesidir.

Eşitleme altyapısı Eşitleme Kuralı yapılandırmasında belirtilen ölçütleri kullanarak bir metaverse nesneye alma nesnesi katılmaya çalışır.

Sağlama ve birleştirme işlemleri sırasında, eşitleme motoru birleştirilmiş nesneyi metaverse nesneye bağlar ve bu da onları birleştirir. Bu nesne düzeyindeki işlemler tamamlandıktan sonra, eşitleme altyapısı ilişkili metaverse nesnenin öznitelik değerlerini güncelleştirebilirsiniz. Bu işleme alma öznitelik akışı denir.

Alma özniteliği akışı, yeni veri taşıyan ve metaverse nesnesine bağlı tüm içe aktarma nesnelerinde oluşur.

**Giden eşitleme**

Giden eşitleme, metaverse nesnesi değiştiğinde ancak silinmediğinde dışa aktarma nesnelerini güncelleştirir. Giden eşitlemenin amacı, metaverse nesnelerdeki değişikliklerin bağlayıcı boşluklardaki nesneleri evreleme güncelleştirmelerini gerektirip gerektirmediğini değerlendirmektir. Bazı durumlarda, değişiklikler tüm bağlayıcı alanlarda ki nesneleri hazırlama nın güncelleştirilmesini gerektirebilir. Değiştirilen nesneleri evreleme bekleyen dışa aktarma olarak işaretlenir ve bu da nesneleri dışa aktarmalarını sağlamaz. Bu dışa aktarma nesneleri daha sonra dışa aktarma işlemi sırasında bağlı veri kaynağına itilir.

Giden eşitlemenin üç işlemi vardır:

* **Sağlanıyor**
* **Deprovisioning**
* **Dışa aktarma öznitelik akışı**

Sağlama ve deprovisioning nesne düzeyinde işlemleri her ikisi de vardır. Sadece sağlama başlatabilir, çünkü deprovisioning sağlanması bağlıdır. Sağlama, metaverse nesnesi ile dışa aktarma nesnesi arasındaki bağlantıyı kaldırdığında deprovisioning tetiklenir.

Metaverse nesnelere değişiklikler uygulandığında sağlama her zaman tetiklenir. Metaverse nesnelerde değişiklikler yapıldığında, eşitleme altyapısı sağlama işleminin bir parçası olarak aşağıdaki görevlerden herhangi birini gerçekleştirebilir:

* Metaverse nesnenin yeni oluşturulan dışa aktarma nesnesine bağlı olduğu birleştirilmiş nesneler oluşturun.
* Birleştirilmiş nesneyi yeniden adlandırın.
* Bir metaverse nesne ve evreleme nesneleri arasındaki bağlantıları birleştirmek, birleştirilmiş bir nesne oluşturma.

Yeni bir bağlayıcı nesne oluşturmak için eşitleme altyapısı gerekiyorsa, nesne bağlı veri kaynağında henüz bulunmadığından, metaverse nesnesinin bağlı olduğu evreleme nesnesi her zaman bir dışa aktarma nesnesidir.

Sağlama, birleştirilmiş bir nesneye katılmak için eşitleme altyapısı gerektiriyorsa, birleştirilmiş bir nesne oluşturmak, deprovisioning tetiklenir. Deprovisioning işlemi nesneyi siler.

Deprovisioning sırasında, dışa aktarma nesnesi silme nesneyi fiziksel olarak silmez. Nesne **silinmiş**olarak işaretlenir, bu da silme işleminin nesne üzerinde aşamalı olduğu anlamına gelir.

Dışa aktarma özniteliği akışı, giden eşitleme işlemi sırasında, içe aktarma öznitelik akışının gelen eşitleme sırasında oluşuruna benzer şekilde de oluşur. Dışa aktarım yalnızca metaverse ve birleşen dışa aktarma nesneleri arasında oluşur.

### <a name="export-process"></a>Dışa aktarma süreci
Dışa aktarma işlemi sırasında, eşitleme altyapısı bağlayıcı alanında bekleyen dışa aktarma olarak işaretlenen tüm dışa aktarma nesnelerini inceler ve ardından bağlı veri kaynağına güncelleştirmeler gönderir.

Eşitleme altyapısı bir dışa aktarmanın başarısını belirleyebilir, ancak kimlik yönetim işleminin tamamlandığını yeterince belirleyemez. Bağlı veri kaynağındaki nesneler her zaman diğer işlemler tarafından değiştirilebilir. Eşitleme altyapısı bağlı veri kaynağına kalıcı bir bağlantısı olmadığından, yalnızca başarılı bir dışa aktarım bildirimine dayalı olarak bağlı veri kaynağındaki bir nesnenin özellikleri hakkında varsayımlarda bulunmak yeterli değildir.

Örneğin, bağlı veri kaynağındaki bir işlem nesnenin özniteliklerini özgün değerlerine geri değiştirebilir (diğer bir deyişle, bağlı veri kaynağı, veriler eşitleme altyapısı tarafından dışarı itildikten ve başarıyla uygulandıktan hemen sonra değerlerin üzerine yazabilir bağlı veri kaynağı).

Eşitleme motoru, her evreleme nesnesi hakkında dışa aktarma ve alma durumu bilgilerini depolar. Öznitelik ekleme listesinde belirtilen özniteliklerin değerleri son dışaaklıktan sonra değiştiyse, alma ve dışa aktarma durumunun depolanması eşitleme altyapısının uygun şekilde tepki göstermesini sağlar. Eşitleme altyapısı, bağlı veri kaynağına dışa aktarılan öznitelik değerlerini onaylamak için alma işlemini kullanır. Aktarılan ve dışa aktarılan bilgiler arasında yapılan karşılaştırma, aşağıdaki resimde gösterildiği gibi, eşitleme altyapısının dışa aktarmanın başarılı olup olmadığını veya yineedilmesi gerekip gerekmediğini belirlemesini sağlar.

![Arch7](./media/concept-azure-ad-connect-sync-architecture/arch7.png)

Örneğin, eşitleme motoru bağlı bir veri kaynağına 5 değeri olan C özniteliği varsa, dışa aktarma durumu belleğinde C=5 depolar. Bu nesnedeki her ek dışa aktarım, c=5'i bağlı veri kaynağına yeniden dışa aktarma çabasıyla sonuçlanır, çünkü eşitleme altyapısı bu değerin nesneye kalıcı olarak uygulanmadığını varsayar (diğer bir deyişle, son zamanlarda farklı bir değer bağlı veri kaynağı). C=5 nesne üzerinde bir alma işlemi sırasında alındığı zaman dışa aktarma belleği temizlenir.

## <a name="next-steps"></a>Sonraki adımlar
Azure AD [Connect eşitleme](how-to-connect-sync-whatis.md) yapılandırması hakkında daha fazla bilgi edinin.

[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md) hakkında daha fazla bilgi edinin.

