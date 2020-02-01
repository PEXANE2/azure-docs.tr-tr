---
title: Azure AD Connect performansını etkileyen faktörler
description: Bu belgede, çeşitli faktörlerin Azure AD Connect sağlama altyapısını nasıl etkilediği açıklanmaktadır. Bu faktörler, kuruluşların eşitleme gereksinimlerini karşıladığından emin olmak için Azure AD Connect dağıtımını planlamalarına yardımcı olur.
services: active-directory
author: billmath
manager: daveba
tags: azuread
ms.service: active-directory
ms.subservice: hybrid
ms.topic: conceptual
ms.workload: identity
ms.date: 10/06/2018
ms.reviewer: martincoetzer
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: a5518d516848ba7c006827faa41ff76bbca35d0c
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2020
ms.locfileid: "76897063"
---
# <a name="factors-influencing-the-performance-of-azure-ad-connect"></a>Azure AD Connect performansını etkileyen faktörler

Azure AD Connect Active Directory Azure AD 'ye eşitler. Bu sunucu, kullanıcı kimliklerinizi buluta taşımaya yönelik kritik bir bileşendir. Azure AD Connect performansını etkileyen birincil faktörler şunlardır:

| **Tasarım faktörü**| **Tanım** |
|:-|-|
| Topoloji| Uç noktaların ve bileşenlerin dağıtımı Azure AD Connect ağ üzerinde yönetilmesi gerekir. |
| Ölçeklendirme| Azure AD Connect tarafından yönetilecek kullanıcılar, gruplar ve OU 'Lar gibi nesnelerin sayısı. |
| Donanım| Azure AD Connect için donanım (fiziksel veya sanal) ve CPU, bellek, ağ ve sabit sürücü yapılandırması gibi her bir donanım bileşeninin bağımlı performans kapasitesi. |
| Yapılandırma| Azure AD Connect dizinleri ve bilgileri nasıl işler. |
| Yükle| Nesne değişikliklerinin sıklığı. Yükleme bir saat, gün veya hafta boyunca farklılık gösterebilir. Bileşene bağlı olarak, yoğun yük veya ortalama yük için tasarım yapmanız gerekebilir. |

Bu belgenin amacı, Azure AD Connect sağlama altyapısının performansını etkileyen faktörleri anlatmaktadır. Büyük veya karmaşık kuruluşlar (100.000 'den fazla nesne sağlama), burada özetlenen performans sorunlarıyla karşılaşmaları halinde Azure AD Connect uygulamasını iyileştirmek için önerileri kullanabilir. [Azure AD Connect sistem durumu](how-to-connect-health-agent-install.md) ve aracılar gibi diğer Azure AD Connect bileşenleri burada kapsanmaz.

> [!IMPORTANT]
> Microsoft, resmi olarak belgelenen eylemlerin dışında değiştirme veya çalıştırma Azure AD Connect desteklemez. Bu eylemlerden herhangi biri tutarsız veya Azure AD Connect eşitlemeye neden olabilecek bir durum oluşabilir. Sonuç olarak, Microsoft bu tür dağıtımlar için teknik destek sağlayamaz.

## <a name="azure-ad-connect-component-factors"></a>Azure AD Connect bileşen faktörleri

Aşağıdaki diyagramda, birden çok orman desteklenmesine karşın, tek bir ormana bağlanan bir sağlama altyapısının üst düzey mimarisi gösterilmektedir. Bu mimari çeşitli bileşenlerin birbirleriyle nasıl etkileşime gireceğini gösterir.

![Azureadconnentınternal](media/plan-connect-performance-factors/AzureADConnentInternal.png)

Sağlama altyapısı her bir Active Directory ormanına ve Azure AD 'ye bağlanır. Her dizinden bilgi okuma işlemi Içeri aktarma olarak adlandırılır. Dışarı aktarma işlemi, dizinleri sağlama altyapısından güncelleştirmek anlamına gelir. Eşitleme, nesnelerin sağlama altyapısının içinde nasıl akacağı kurallarını değerlendirir. Daha ayrıntılı bir bakış için, [Azure AD Connect eşitleme: mimariyi anlama](https://docs.microsoft.com/azure/active-directory/hybrid/concept-azure-ad-connect-sync-architecture)bölümüne başvurabilirsiniz.

Azure AD Connect, Active Directory Azure AD 'ye eşitlemeye izin vermek için aşağıdaki hazırlama bölgelerini, kuralları ve süreçlerini kullanır:

* **Bağlayıcı alanı (CS)** -her bağlı DIZINDEKI (CD), gerçek dizinlerin nesneleri, sağlama altyapısı tarafından işlenebilmeleri için önce burada hazırlanır. Azure AD 'nin kendi CS ve bağlandığınız her ormanın kendine ait CS vardır.
* **Meta veri deposu (MV)** -eşitlenmesi gereken nesneler, eşitleme kurallarına göre burada oluşturulur. Nesnelerin, nesneleri ve öznitelikleri diğer bağlı dizinlere doldurmasına izin vermeden önce, nesneler MV içinde bulunmalıdır. Yalnızca bir MV vardır.
* **Eşitleme kuralları** -hangi nesnelerin (yansıtılmış) veya MV 'daki nesnelere bağlı (birleştirilmiş) olduğuna karar verir. Eşitleme kuralları ayrıca hangi öznitelik değerlerinin dizine ve dizinlerden/veya bu dizinlere dönüştürüleceğini de karar verir.
* **Çalıştırma profilleri** -, hazırlama alanı ve bağlı dizinler arasındaki eşitleme kurallarına göre nesneleri ve bunların öznitelik değerlerini kopyalamanın işlem adımlarını sağlar.

Sağlama altyapısının performansını iyileştirmek için farklı çalıştırma profilleri vardır. Çoğu kuruluş, varsayılan zamanlamaları kullanır ve normal işlemler için profilleri çalıştırır, ancak bazı kuruluşların yaygın olmayan durumlar için [zamanlamayı değiştirmek](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-sync-feature-scheduler) veya diğer çalıştırma profillerini kullanmak zorunda olması gerekebilir. Aşağıdaki çalıştırma profilleri kullanılabilir:

### <a name="initial-sync-profile"></a>İlk eşitleme profili

İlk eşitleme profili, Active Directory ormanı gibi bağlı dizinleri ilk kez okuma işlemidir. Ardından, eşitleme altyapısı veritabanındaki tüm girişlerde bir analiz yapar. İlk zaman, Azure AD 'de yeni nesneler oluşturacak ve Active Directory ormanlarınız büyükse daha fazla zaman alacak. İlk eşitleme aşağıdaki adımları içerir:

1. Tüm bağlayıcılarda tam içeri aktarma
2. Tüm bağlayıcılarda tam eşitleme
3. Tüm bağlayıcılarda dışarı aktar

### <a name="delta-sync-profile"></a>Delta eşitleme profili

Eşitleme işlemini iyileştirmek için bu çalıştırma profili, son eşitleme işleminden bu yana bağlantılı dizinlerinizdeki nesnelerin yalnızca değişikliklerini (oluşturur, siler ve güncelleştirmeler) işler. Varsayılan olarak, Delta eşitleme profili her 30 dakikada bir çalışır. Kuruluşların, Azure AD 'nin güncel olduğundan emin olmak için 30 dakikadan daha kısa sürede sürme süresini üstlenmelidir. Azure AD Connect durumunu izlemek için, işlemle ilgili herhangi bir sorunu görmek için [sistem durumu izleme aracısını](how-to-connect-health-sync.md) kullanın. Delta eşitleme profili aşağıdaki adımları içerir:

1. Tüm bağlayıcılarda Delta içeri aktarma
2. Tüm bağlayıcılarda Delta eşitleme
3. Tüm bağlayıcılarda dışarı aktar

Tipik bir kurumsal kuruluş Delta eşitleme senaryosu şunlardır:

- ~ nesnelerin %1 ' i silindi
- ~ nesnelerin %1 ' i oluşturuldu
- ~ nesnelerin %5 ' i değiştirildi

Değişiklik hızlarınız, kuruluşunuzun Active Directory kullanıcıları ne sıklıkta güncelleştirdiğine bağlı olarak değişebilir. Örneğin, iş zorlamaya ve azaltmaya yönelik mevsimsellik sayesinde daha yüksek bir değişiklik ücretleri meydana gelebilir.

### <a name="full-sync-profile"></a>Tam eşitleme profili

Aşağıdaki yapılandırma değişikliklerinden birini yaptıysanız tam bir eşitleme çevrimi gereklidir:



- Bağlı dizinlerden içeri aktarılacak nesne veya özniteliklerin kapsamı arttırılmıştır. Örneğin, içeri aktarma kapsamınızda bir etki alanı veya OU eklediğinizde.
- Eşitleme kurallarında değişiklikler yapıldı. Örneğin, Azure AD 'de bir kullanıcının başlığını Active Directory extension_attribute3 olarak doldurmak için yeni bir kural oluşturduğunuzda. Bu güncelleştirme, sağlama altyapısının, tüm mevcut kullanıcıları yeniden inceleyerek, başlıkları öne çıkan değişikliği uygulayacak şekilde güncelleştirir.

Aşağıdaki işlemler tam eşitleme döngüsüne dahildir:

1. Tüm bağlayıcılarda tam içeri aktarma
2. Tüm bağlayıcılarda tam/Delta eşitlemesi
3. Tüm bağlayıcılarda dışarı aktar

> [!NOTE]
> Active Directory veya Azure AD 'de çok sayıda nesneye toplu güncelleştirmeler yaparken dikkatli bir planlama yapmanız gerekir. Çok sayıda nesne değiştiğinden, toplu güncelleştirmeler içeri aktarma sırasında Delta eşitleme işleminin daha uzun sürmesine neden olur. Toplu güncelleştirme eşitleme işlemini etkilemese bile uzun içeri aktarmalar gerçekleşebilir. Örneğin, Azure AD 'de birçok kullanıcıya lisans atamak, Azure AD 'den uzun bir içeri aktarma döngüsüne neden olur, ancak Active Directory hiçbir öznitelik değişikliğine neden olmaz.

### <a name="synchronization"></a>Eşitleme

Eşitleme işlemi çalışma zamanı, aşağıdaki performans özelliklerine sahiptir:

* Eşitleme tek iş parçacıklı, yani sağlama altyapısı bağlı dizinlerin, nesnelerin veya özniteliklerin çalıştırma profillerinin hiçbir paralel işlemesini yapmaz.
* İçeri aktarma süresi, eşitlenen nesne sayısıyla doğrusal olarak artar. Örneğin, 10.000 nesnelerinin içeri aktarılması 10 dakika alıyorsa, 20.000 nesne aynı sunucuda yaklaşık 20 dakika sürer.
* Dışarı aktarma da doğrusal bir şekilde yapılır.
* Eşitleme, diğer nesnelere başvuru içeren nesne sayısına göre üstel olarak artar. Grup üyelikleri ve iç içe gruplar, üyeleri Kullanıcı nesnelerine veya diğer gruplara başvurduğundan ana performans etkisine sahiptir. Eşitleme döngüsünü tamamlayabilmeniz için bu başvuruların bulunması ve MV içindeki gerçek nesnelere başvurulması gerekir.

### <a name="filtering"></a>Filtreleme

İçeri aktarmak istediğiniz Active Directory topolojisinin boyutu, performansı etkileyen bir faktörün ve sağlama altyapısının iç bileşenlerinin zaman aldığı sayıdır.

Nesneleri eşitlenecek şekilde azaltmak için [filtreleme](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-sync-configure-filtering) kullanılmalıdır. Bu, gereksiz nesnelerin işlenmesini ve Azure AD 'ye verilmesini engeller. Tercih sırasına göre, filtreleme için aşağıdaki teknikler kullanılabilir:



- **Etki alanı tabanlı filtreleme** – Azure AD ile eşitlenecek belirli etki alanlarını seçmek için bu seçeneği kullanın. Azure AD Connect eşitlemesini yükledikten sonra şirket içi altyapınızda değişiklik yaptığınızda, eşitleme altyapısı yapılandırmasına etki alanı eklemeniz ve çıkarmanız gerekir.
- **Kuruluş birimi (OU) filtreleme** -Azure AD 'ye sağlamak üzere Active Directory etki alanlarında belirli nesneleri hedeflemek Için kuruluş birimleri kullanır. OU filtrelemesi, en az bir nesne alt kümesini Active Directory içeri aktarmak için basit LDAP kapsam sorguları kullandığından, ikinci önerilen filtreleme mekanizmasıdır.
- **Nesne başına öznitelik filtreleme** -Active Directory içindeki belirli bir NESNENIN Azure AD 'de sağlanmasının gerekip gerekmediğine karar vermek için nesnelerdeki öznitelik değerlerini kullanır. Etki alanı ve OU filtrelemesi belirli filtreleme gereksinimlerini karşılamadığında, filtrelerinizin ince ayar yapmak için öznitelik filtrelemesi çok uygundur. Öznitelik filtrelemesi içeri aktarma süresini azaltmaz, ancak eşitleme ve dışarı aktarma sürelerini azaltabilir.
- **Grup tabanlı filtreleme** -NESNELERIN Azure AD 'de sağlanması gerekip gerekmediğine karar vermek için Grup üyeliğini kullanır. Grup tabanlı filtreleme, eşitleme çevrimi sırasında grup üyeliğini denetlemek için gereken ek yük nedeniyle, yalnızca test durumları için uygundur ve üretim için önerilmez.

Active Directory CS 'deki pek çok kalıcı [ayırıcı](concept-azure-ad-connect-sync-architecture.md#relationships-between-staging-objects-and-metaverse-objects) nesne, eşitleme döngüsünün olası bağlantı için her bir ayırıcı nesnesini yeniden değerlendirmesine yol açacağından, eşitleme süreleri daha uzun olabilir. Bu sorunu aşmak için aşağıdaki önerilerden birini göz önünde bulundurun:



- Etki alanı veya OU filtrelemesini kullanarak içeri aktarma için, ayırıcı nesnelerini kapsam dışına yerleştirin.
- Azure AD CS 'de bu nesnelerin sağlanmasını engellemek için, nesneleri MV 'a ekleyin ve [Cloudfiltrelenmiş](how-to-connect-sync-configure-filtering.md#negative-filtering-do-not-sync-these) özniteliğini true değerine ayarlayın.

> [!NOTE]
> Çok fazla nesne filtrelendiğinde, kullanıcılar kafa karışıyor veya uygulama izinleri sorunları ortaya çıkabilir. Örneğin, karma bir Exchange Online uygulamasında, şirket içi posta kutularına sahip kullanıcılar, Exchange Online 'da posta kutusu olan kullanıcılardan, genel adres listesinde daha fazla Kullanıcı görecektir. Diğer durumlarda, bir Kullanıcı, bir bulut uygulamasında filtrelenmiş nesne kümesinin kapsamına dahil olmayan başka bir kullanıcıya erişim izni vermek isteyebilir.

### <a name="attribute-flows"></a>Öznitelik akışları

Öznitelik akışları, nesnelerin öznitelik değerlerini bir bağlı dizinden başka bir bağlı dizine kopyalama veya dönüştürme işlemidir. Eşitleme kurallarının bir parçası olarak tanımlanırlar. Örneğin, bir kullanıcının telefon numarası Active Directory değiştirildiğinde, Azure AD 'deki telefon numarası güncelleştirilecektir. Kuruluşlar, çeşitli gereksinimleri paketleyebileceği [öznitelik akışlarını değiştirebilir](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-sync-change-the-configuration) . Mevcut öznitelik akışlarını değiştirmeden önce kopyalamanız önerilir.

Bir öznitelik değerinin farklı bir özniteliğe akmasını sağlamak gibi basit yeniden yönlendirmeler, malzeme performansı olumsuz bir etkiye sahip değildir. Azure AD 'de telefon numarası Active Directory bir yeniden yönlendirmeye örnek olarak bir cep telefonu numarası akar.

Öznitelik değerlerinin dönüştürülmesi, eşitleme işlemi üzerinde bir performans etkisine sahip olabilir. Öznitelik değerlerini dönüştürme, özniteliklerin değerlerini değiştirme, yeniden biçimlendirme, bitiştirme veya çıkarma içerir.

Kuruluşlar bazı özniteliklerin Azure AD 'ye akmasını engelleyebilir, ancak sağlama altyapısının performansını etkilemez.

> [!NOTE]
> Eşitleme kurallarınızda istenmeyen öznitelik akışlarını silmeyin. Azure AD Connect yükseltmeleri sırasında silinen kurallar yeniden oluşturulduğundan, bunları devre dışı bırakmanız önerilir.

## <a name="azure-ad-connect-dependency-factors"></a>Azure AD Connect bağımlılık faktörleri

Azure AD Connect performansı, içeri aktardığı ve dışa aktardığı bağlı dizinlerin performansına bağlıdır. Örneğin, içeri aktarılması gereken Active Directory boyutu veya Azure AD hizmetine ağ gecikmesi. Sağlama altyapısının kullandığı SQL veritabanı, eşitleme döngüsünün genel performansını da etkiler.

### <a name="active-directory-factors"></a>Active Directory faktörleri

Daha önce belirtildiği gibi, alınacak nesne sayısı performansı önemli ölçüde etkiler. [Azure AD Connect donanım ve önkoşulları](how-to-connect-install-prerequisites.md) , dağıtımınızın boyutuna bağlı olarak belirli donanım katmanlarını özetler. Azure AD Connect, yalnızca [Azure AD Connect topolojilerde](plan-connect-topologies.md)belirtilen topolojileri destekler. Desteklenmeyen topolojiler için performans iyileştirmesi ve öneriler yoktur.

Azure AD Connect sunucunuzun, içeri aktarmak istediğiniz Active Directory boyutuna bağlı olarak donanım gereksinimlerini karşıladığından emin olun. Azure AD Connect sunucusu ile Active Directory etki alanı denetleyicileriniz arasında bozuk veya yavaş ağ bağlantısı, içeri aktarmanızı yavaşlatabilir.

### <a name="azure-ad-factors"></a>Azure AD faktörleri

Azure AD, bulut hizmetini hizmet reddi (DoS) saldırılarına karşı korumak için azaltma kullanır. Şu anda Azure AD 'nin 5 dakika başına 7.000 yazma sınırlaması vardır (saat başına 84.000). Örneğin, aşağıdaki işlemler kısıtlanabilir:



- Azure AD Connect Azure AD 'ye aktarın.
- Dinamik grup üyelikleri gibi doğrudan arka planda bile Azure AD 'yi güncelleştiren PowerShell betikleri veya uygulamaları.
- Kullanıcılar MFA veya SSPR 'ye kaydolma (self servis parola sıfırlama) gibi kendi kimlik kayıtlarını güncelliyor.
- Grafik Kullanıcı arabirimindeki işlemler.

Azure AD Connect eşitleme döngünüzün azaltma limitlerinin etkilenmediğinden emin olmak için dağıtım ve bakım görevlerini planlayın. Örneğin, binlerce Kullanıcı kimliği oluşturduğunuz büyük bir işe alma dalgasına sahipseniz, dinamik grup üyelikleri, lisanslama atamaları ve self servis parola sıfırlama kayıtlarında güncelleştirmelere neden olabilir. Bu yazmaları birkaç saat veya birkaç gün içinde yaymak daha iyidir.

### <a name="sql-database-factors"></a>SQL veritabanı faktörleri

Kaynak Active Directory topolojinizin boyutu SQL veritabanı performansınızı etkiler. SQL Server veritabanı için [donanım gereksinimlerini](how-to-connect-install-prerequisites.md) izleyin ve aşağıdaki önerileri göz önünde bulundurun:



- 100.000 ' den fazla kullanıcısı olan kuruluşlar, SQL veritabanını ve sağlama altyapısını aynı sunucuda birleştirerek ağ gecikmelerinin azaltılmasına yönelik olabilir.
- Eşitleme işleminin yüksek disk girişi ve çıkış (g/ç) gereksinimleri nedeniyle, en iyi sonuçlar için sağlama altyapısının SQL veritabanı için katı hal sürücüleri (SSD) kullanın, mümkün değilse RAID 0 veya RAID 1 konfigürasyonları değerlendirin.
- Tam eşitleme ön önleyici yapmayın; gereksiz karmaşıklık ve daha yavaş yanıt sürelerine neden olur.

## <a name="conclusion"></a>Sonuç

Azure AD Connect uygulamanızın performansını iyileştirmek için aşağıdaki önerileri göz önünde bulundurun:



- Azure AD Connect sunucusu için uygulama boyutunuzu temel alarak [Önerilen donanım yapılandırmasını](how-to-connect-install-prerequisites.md) kullanın.
- Büyük ölçekli dağıtımlarda Azure AD Connect yükseltirken, en az kapalı kalma süresi ve en iyi güvenilirlik olduğundan emin olmak için [esnek geçiş yöntemini](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-upgrade-previous-version#swing-migration)kullanmayı düşünün. 
- En iyi yazma performansı için SQL veritabanı için SSD kullanın.
- Active Directory kapsamını yalnızca Azure AD 'de, etki alanı, OU veya öznitelik filtrelemesi kullanılarak sağlanması gereken nesneleri içerecek şekilde filtreleyin.
- Varsayılan öznitelik akış kurallarını değiştirmeniz gerekiyorsa, önce kuralı kopyalayın, sonra kopyayı değiştirin ve özgün kuralı devre dışı bırakın. Tam eşitlemeyi yeniden çalıştırmayı unutmayın.
- İlk tam eşitleme çalıştırma profili için yeterli zaman planlayın.
- 30 dakika içinde Delta eşitleme döngüsünü tamamlamaya çalışır. Delta eşitleme profili 30 dakika içinde tamamlanmazsa, varsayılan eşitleme sıklığını tam bir Delta eşitleme döngüsünü içerecek şekilde değiştirin.
- Azure AD 'de [Azure AD Connect eşitleme sistem durumunu](how-to-connect-health-agent-install.md) izleyin.

## <a name="next-steps"></a>Sonraki adımlar
[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md) hakkında daha fazla bilgi edinin.
