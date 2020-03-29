---
title: Azure AD Connect performansını etkileyen faktörler
description: Bu belge, çeşitli faktörlerin Azure AD Connect sağlama altyapısını nasıl etkilediğini açıklar. Bu etkenler, kuruluşların eşitleme gereksinimlerini karşıladığından emin olmak için Azure AD Connect dağıtımlarını planlamalarına yardımcı olur.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76897063"
---
# <a name="factors-influencing-the-performance-of-azure-ad-connect"></a>Azure AD Connect performansını etkileyen faktörler

Azure AD Connect, Etkin Dizininizi Azure AD ile eşitler. Bu sunucu, kullanıcı kimliklerinizi buluta taşımanın önemli bir bileşenidir. Azure AD Bağlantısının performansını etkileyen başlıca etkenler şunlardır:

| **Tasarım faktörü**| **Tanım** |
|:-|-|
| Topoloji| Azure AD Connect'in ağda yönetilmesi gereken uç noktaların ve bileşenlerin dağıtımı. |
| Ölçek| Azure AD Connect tarafından yönetilecek kullanıcılar, gruplar ve OSB gibi nesnelerin sayısı. |
| Donanım| Azure AD Connect'in donanımı (fiziksel veya sanal) ve CPU, bellek, ağ ve sabit disk yapılandırması dahil olmak üzere her donanım bileşeninin bağımlı performans kapasitesi. |
| Yapılandırma| Azure AD Connect dizinleri ve bilgileri nasıl işler? |
| Yükleme| Nesne değişikliklerinin sıklığı. Yükler bir saat, gün veya hafta boyunca değişebilir. Bileşene bağlı olarak, en yüksek yük veya ortalama yük için tasarım yapmak zorunda klabilirsiniz. |

Bu belgenin amacı, Azure AD Connect sağlama altyapısının performansını etkileyen faktörleri açıklamaktır. Büyük veya karmaşık kuruluşlar (100.000'den fazla nesne sağlayan kuruluşlar) burada özetlenen herhangi bir performans sorunuyla karşılaşırsa, Azure AD Connect uygulamalarını optimize etmek için önerileri kullanabilir. Azure AD Connect'in Azure [AD Connect sistem durumu](how-to-connect-health-agent-install.md) ve aracılar gibi diğer bileşenleri burada kapsam almaz.

> [!IMPORTANT]
> Microsoft, Azure AD Connect'in resmi olarak belgelenen eylemlerin dışında değiştirilmesini veya çalıştırını desteklemez. Bu eylemlerden herhangi biri, Azure AD Connect eşitlemesinde tutarsız veya desteklenmeyen bir duruma neden olabilir. Sonuç olarak, Microsoft bu tür dağıtımlar için teknik destek sağlayamaz.

## <a name="azure-ad-connect-component-factors"></a>Azure AD Connect bileşen faktörleri

Aşağıdaki diyagram, birden çok orman desteklenmiş olsa da, tek bir ormana bağlanan sağlama motorunun üst düzey mimarisini gösterir. Bu mimari, çeşitli bileşenlerin birbiriyle nasıl etkileşimde olduğunu gösterir.

![AzureADConnentDahil](media/plan-connect-performance-factors/AzureADConnentInternal.png)

Sağlama altyapısı her Active Directory ormanına ve Azure AD'ye bağlanır. Her dizindeki bilgileri okuma işlemine Alma denir. Dışa aktarma, sağlama motorundaki dizinlerin güncelleştirilmesi anlamına gelir. Eşitleme, nesnelerin sağlama motorunun içinde nasıl akacağına ilişkin kuralları değerlendirir. Daha derin bir dalış için [Azure AD Connect eşitlemesine başvurabilirsiniz: Mimariyi anlama.](https://docs.microsoft.com/azure/active-directory/hybrid/concept-azure-ad-connect-sync-architecture)

Azure AD Connect, Etkin Dizin'den Azure AD'ye eşitleme nin izin vermek için aşağıdaki hazırlama alanlarını, kurallarını ve işlemlerini kullanır:

* **Bağlayıcı Alanı (CS)** - Gerçek dizinler olan her bağlı dizindeki (CD) nesneler, sağlama altyapısı tarafından işlenmeden önce burada ilk kez sahnelenir. Azure AD'nin kendi CS'si vardır ve bağlandığınız her ormanın kendi CS'si vardır.
* **Metaverse (MV)** - Eşitlenmesi gereken nesneler eşitleme kurallarına göre burada oluşturulur. Nesneleri ve öznitelikleri diğer bağlı dizinlere doldurmak için önce MV nesneleri var olmalıdır. Sadece bir MV var.
* **Eşitleme kuralları** - MV'deki nesnelere hangi nesnelerin oluşturulacağına (yansıtılacağına) veya bağlanacağına (birleşeceğine) karar verirler. Eşitleme kuralları ayrıca hangi öznitelik değerlerinin kopyalanacağına veya dizinlere ve dizinlere dönüştürüleceğine de karar verir.
* **Profili çalıştır** - Nesneleri ve öznitelik değerlerini ayırma alanları ve bağlı dizinler arasındaki eşitleme kurallarına göre kopyalama işlem adımlarını paketler.

Sağlama altyapısının performansını optimize etmek için farklı çalışma profilleri vardır. Çoğu kuruluş varsayılan zamanlamaları kullanır ve normal işlemler için profilleri çalıştırın, ancak bazı kuruluşların sık karşılaşılan durumlara hitap etmek için [zamanlamayı değiştirmesi](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-sync-feature-scheduler) veya diğer çalışma profillerini tetiklemesi gerekebilir. Aşağıdaki çalışma profilleri kullanılabilir:

### <a name="initial-sync-profile"></a>İlk eşitleme profili

İlk eşitleme profili, bağlı dizinleri ilk kez Etkin Dizin ormanı gibi okuma işlemidir. Daha sonra eşitleme motoru veritabanında tüm girişleri bir analiz yapar. İlk döngü Azure AD'de yeni nesneler oluşturur ve Active Directory ormanlarınız büyükse tamamlanması ekstra zaman alır. İlk eşitleme aşağıdaki adımları içerir:

1. Tüm konektörlerde tam alma
2. Tüm konektörlerde tam eşitleme
3. Tüm konektörlere dışa aktarma

### <a name="delta-sync-profile"></a>Delta senkronizasyon profili

Eşitleme işlemini en iyi duruma getirmek için bu çalıştırma profili yalnızca bağlı dizinlerinizdeki nesnelerin değişikliklerini (oluşturur, siler ve güncelleştirmeler) son eşitleme işleminden bu yana işlemeyi sağlar. Varsayılan olarak, delta eşitleme profili her 30 dakikada bir çalışır. Kuruluşlar, Azure REKLAM'ın güncel olduğundan emin olmak için gereken süreyi 30 dakikanın altında tutmaya çalışmalıdır. Azure AD Connect'in durumunu izlemek için, işlemle ilgili sorunları görmek için [sistem durumu izleme aracısını](how-to-connect-health-sync.md) kullanın. Delta eşitleme profili aşağıdaki adımları içerir:

1. Tüm konektörlerde Delta alma
2. Tüm konektörlerde delta senkronizasyonu
3. Tüm konektörlere dışa aktarma

Tipik bir kurumsal kuruluş delta eşitleme senaryosu:

- Nesnelerin ~%1'i silinir
- ~1% nesnelerin oluşturulur
- Nesnelerin ~%5'i değiştirildi

Değişiklik oranınız, kuruluşunuzun Etkin Dizininizdeki kullanıcıları ne sıklıkta güncellebileceğine bağlı olarak değişebilir. Örneğin, işe alma ve iş gücünün azaltılması mevsimsellik ile daha yüksek değişim oranları oluşabilir.

### <a name="full-sync-profile"></a>Tam eşitleme profili

Aşağıdaki yapılandırma değişikliklerinden herhangi birini yaptıysanız tam eşitleme döngüsü gereklidir:



- Bağlı dizinlerden alınacak nesnelerin veya özniteliklerin kapsamı artırıldı. Örneğin, alma kapsamınıza bir etki alanı veya OU eklediğinizde.
- Eşitleme kurallarında değişiklik yapıldı. Örneğin, Bir kullanıcının unvanını Azure AD'de Etkin Dizini'ndeki extension_attribute3 doldurmak için yeni bir kural oluşturduğunuzda. Bu güncelleştirme, sağlama altyapısının, ileriye dönük değişikliği uygulamak için başlıklarını güncelleştirmeleri için varolan tüm kullanıcıları yeniden incelemesini gerektirir.

Aşağıdaki işlemler tam eşitleme döngüsüne dahildir:

1. Tüm konektörlerde tam alma
2. Tüm konektörlerde tam/Delta senkronizasyonu
3. Tüm konektörlere dışa aktarma

> [!NOTE]
> Etkin Dizinveya Azure AD'nizdeki birçok nesneiçin toplu güncelleştirmeler yaparken dikkatli planlama gereklidir. Toplu güncelleştirmeler, çok sayıda nesne değiştiğinden, içe aktarma işlemi nin daha uzun sürmesine neden olur. Toplu güncelleştirme eşitleme işlemini etkilemese bile uzun içe almalar gerçekleşebilir. Örneğin, Azure AD'deki birçok kullanıcıya lisans atamak Azure AD'den uzun bir alma döngüsüne neden olur, ancak Active Directory'de herhangi bir öznitelik değişikliğine neden olmaz.

### <a name="synchronization"></a>Eşitleme

Eşitleme işlemi çalışma süresi aşağıdaki performans özelliklerine sahiptir:

* Eşitleme tek iş parçacığı, yani sağlama altyapısı bağlı dizinlerin, nesnelerin veya özniteliklerin çalışma profillerinin paralel bir işlemesini yapmaz.
* Alma süresi, eşitlenen nesne sayısıyla doğrusal olarak büyür. Örneğin, 10.000 nesnenin içe aktarılaması 10 dakika sürüyorsa, 20.000 nesne aynı sunucuda yaklaşık 20 dakika sürer.
* Dışa aktarma da doğrusal.
* Eşitleme, diğer nesnelere başvuruolan nesnelerin sayısına bağlı olarak katlanarak büyür. Üyeleri kullanıcı nesnelerine veya diğer gruplara atıfta bulunduğundan, grup üyelikleri ve iç içe geçen gruplar ana performans etkisine sahiptir. Eşitleme döngüsünü tamamlamak için bu başvurular bulunmalıdır ve MV'deki gerçek nesnelere başvurulmalıdır.

### <a name="filtering"></a>Filtreleme

Almak istediğiniz Active Directory topolojisinin boyutu, sağlama altyapısının iç bileşenlerinin alacağı performansı ve genel süreyi etkileyen bir numaralı faktördür.

[Filtreleme,](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-sync-configure-filtering) nesneleri eşitlenene düşürmek için kullanılmalıdır. Gereksiz nesnelerin işlenmesini ve Azure AD'ye dışa aktarılmasını önler. Tercih sırasına göre, aşağıdaki filtreleme teknikleri mevcuttur:



- **Etki alanı tabanlı filtreleme** – Azure AD ile eşitlemek için belirli etki alanlarını seçmek için bu seçeneği kullanın. Azure AD Connect eşitlemeyi yükledikten sonra şirket içi altyapınızda değişiklik yaptığınızda eşitleme motoru yapılandırmasından etki alanları eklemeniz ve kaldırmanız gerekir.
- **Kuruluş Birimi (OU) filtreleme** - Azure AD'ye sağlama için Active Directory etki alanlarındaki belirli nesneleri hedeflemek için OS B'leri kullanır. Ou filtreleme, Active Directory'den daha küçük bir nesne alt kümesini almak için basit LDAP kapsam sorguları kullandığından, önerilen ikinci filtreleme mekanizmasıdır.
- **Nesne başına öznitelik filtreleme** - Etkin Dizin'deki belirli nesnenin Azure AD'de sağlanıp sağlanmayacağına karar vermek için nesnelerdeki öznitelik değerlerini kullanır. Öznitelik filtreleme, etki alanı ve OU filtreleme nin belirli filtreleme gereksinimlerini karşılamadığı nda, filtrelerinizi hassas ayarlaya kadar mükemmeldir. Öznitelik filtreleme alma süresini azaltmaz, eşitleme ve dışa aktarma sürelerini azaltabilir.
- **Grup tabanlı filtreleme** - nesnelerin Azure AD'de sağlanıp sağlanmayacağına karar vermek için grup üyeliğini kullanır. Grup tabanlı filtreleme yalnızca test durumları için uygundur ve eşitleme döngüsü sırasında grup üyeliğini denetlemek için gereken ek ek yükü nedeniyle üretim için önerilmez.

Etkin Dizin CS'nizdeki birçok kalıcı [bağlantı kesiği nesnesi](concept-azure-ad-connect-sync-architecture.md#relationships-between-staging-objects-and-metaverse-objects) daha uzun eşitleme sürelerine neden olabilir, çünkü sağlama altyapısı eşitleme döngüsünde olası bağlantı için her bağlantı kesilemesi nesnesini yeniden değerlendirmek zorundadır. Bu sorunun üstesinden gelmek için aşağıdaki önerilerden birini göz önünde bulundurun:



- Bağlantı keseci nesneleri etki alanı veya OU filtreleme kullanarak alma için kapsam dışına yerleştirin.
- Azure AD CS'de bu nesnelerin sağlanmasını önlemek için nesneleri MV'ye projelendi/katılın ve [bulutFiltreleştirilmiş](how-to-connect-sync-configure-filtering.md#negative-filtering-do-not-sync-these) özniteliği True'ya eşit olarak ayarlayın.

> [!NOTE]
> Çok fazla nesne filtrelendiğinde, kullanıcıların kafası karışabilir veya uygulama izinleri sorunları oluşabilir. Örneğin, karma Exchange çevrimiçi uygulamasında, şirket içi posta kutuları olan kullanıcılar, global adres listelerinde Exchange çevrimiçi posta kutuları olan kullanıcılardan daha fazla kullanıcı görür. Diğer durumlarda, bir kullanıcı bir bulut uygulamasında filtre uygulanmış nesne kümesinin kapsamının bir parçası olmayan başka bir kullanıcıya erişim izni vermek isteyebilir.

### <a name="attribute-flows"></a>Öznitelik akışları

Öznitelik akışları, nesnelerin öznitelik değerlerini bir bağlı dizinden başka bir bağlı dizine kopyalama veya dönüştürme işlemidir. Eşitleme kurallarının bir parçası olarak tanımlanırlar. Örneğin, Etkin Dizininizde bir kullanıcının telefon numarası değiştirildiğinde, Azure AD'deki telefon numarası güncelleştirilir. Kuruluşlar [öznitelik akışlarını](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-sync-change-the-configuration) çeşitli gereksinimleri süitlere göre değiştirebilir. Varolan öznitelik akışlarını değiştirmeden önce kopyalamanız önerilir.

Bir öznitelik değerini farklı bir özniteliğe akış gibi basit yönlendirmelerin malzeme performans etkisi yoktur. Yeniden yönlendirmenin bir örneği, Active Directory'deki bir cep telefonu numarasını Azure AD'deki ofis telefon numarasına akmasıdır.

Öznitelik değerlerini dönüştürmenin eşitleme işlemi üzerinde bir performans etkisi olabilir. Öznitelik değerlerini dönüştürme, özniteliklerin değerlerini değiştirme, yeniden biçimlendirme, birliş bağlama veya çıkarma içerir.

Kuruluşlar belirli özniteliklerin Azure AD'ye akmasını engelleyebilir, ancak sağlama altyapısının performansını etkilemez.

> [!NOTE]
> Eşitleme kurallarınızda istenmeyen öznitelik akışlarını silmeyin. Silinen kurallar Azure AD Connect yükseltmeleri sırasında yeniden oluşturulduğundan, bunları devre dışı kaldırmanız önerilir.

## <a name="azure-ad-connect-dependency-factors"></a>Azure AD Bağlantı bağımlılık faktörleri

Azure AD Connect'in performansı, içe aktettiği ve dışa aktettiği bağlı dizinlerin performansına bağlıdır. Örneğin, içe aktarması gereken Etkin Dizin boyutu veya Azure AD hizmetine ağ gecikmesi. Sağlama altyapısının kullandığı SQL veritabanı, eşitleme döngüsünün genel performansını da etkiler.

### <a name="active-directory-factors"></a>Aktif Dizin faktörleri

Daha önce de belirtildiği gibi, alınacak nesne sayısı performansı önemli ölçüde etkiler. [Azure AD Connect'in donanımı ve ön koşulları,](how-to-connect-install-prerequisites.md) dağıtımınızın boyutuna bağlı olarak belirli donanım katmanlarını ana hatlar. Azure AD Connect yalnızca Azure AD [Connect için Topolojilerde](plan-connect-topologies.md)belirtildiği gibi belirli topolojileri destekler. Desteklenmeyen topolojiler için performans optimizasyonu ve öneriler yoktur.

Azure AD Connect sunucunuzun, içe aktarmak istediğiniz Etkin Dizin boyutuna bağlı olarak donanım gereksinimlerini karşıladığından emin olun. Azure AD Connect sunucusu ile Active Directory etki alanı denetleyicileriniz arasındaki kötü veya yavaş ağ bağlantısı alma işleminizi yavaşlatabilir.

### <a name="azure-ad-factors"></a>Azure REKLAM faktörleri

Azure AD, bulut hizmetini hizmet reddi (DoS) saldırılarından korumak için azaltma kullanır. Şu anda Azure AD'nin 5 dakikada 7.000 yazma (saatte 84.000) yazma azaltma sınırı vardır. Örneğin, aşağıdaki işlemler azaltılabilir:



- Azure AD Connect dışa aktarma sı Azure AD'ye.
- PowerShell komut dosyaları veya Dinamik grup üyelikleri gibi azure AD'yi arka planda bile doğrudan güncelleyen uygulamalar.
- Kullanıcılar MFA veya SSPR (self servis şifre sıfırlama) için kayıt gibi kendi kimlik kayıtlarını güncellerler.
- Grafik kullanıcı arabirimi içindeki işlemler.

Azure AD Connect eşitleme döngünüzün azaltma sınırlarını etkilemediğinden emin olmak için dağıtım ve bakım görevleri için plan yapın. Örneğin, binlerce kullanıcı kimliği oluşturduğunuz büyük bir işe alma dalganız varsa, dinamik grup üyeliklerinde güncelleştirmelere, lisans atamalarına ve self servis parola sıfırlama kayıtlarına neden olabilir. Bu yazıları birkaç saat veya birkaç gün içinde yaymak daha iyidir.

### <a name="sql-database-factors"></a>SQL veritabanı faktörleri

Kaynak Active Directory topolojinizin boyutu SQL veritabanı performansınızı etkiler. SQL sunucu veritabanı için [donanım gereksinimlerini](how-to-connect-install-prerequisites.md) izleyin ve aşağıdaki önerileri göz önünde bulundurun:



- 100.000'den fazla kullanıcısı olan kuruluşlar, SQL veritabanını ve sağlama altyapısını aynı sunucuda saklayarak ağ gecikmelerini azaltabilir.
- Eşitleme işleminin yüksek disk giriş ve çıktı (G/Ç) gereksinimleri nedeniyle, en iyi sonuçlar için sağlama altyapısının SQL veritabanı için Solid State Drives (SSD) kullanın, mümkün değilse RAID 0 veya RAID 1 yapılandırmalarını düşünün.
- Tam bir senkronizasyon yapmayın önceden boşbir şekilde; gereksiz çalkalama ve daha yavaş yanıt sürelerine neden olur.

## <a name="conclusion"></a>Sonuç

Azure AD Connect uygulamanızın performansını optimize etmek için aşağıdaki önerileri göz önünde bulundurun:



- Azure AD Connect sunucusu için uygulama boyutunuza göre [önerilen donanım yapılandırmasını](how-to-connect-install-prerequisites.md) kullanın.
- Azure AD Connect'i büyük ölçekli dağıtımlarda yükseltirken, en az kapalı kalma süresine ve en iyi güvenilirliğe sahip olduğunuzdan emin olmak için [salıncak geçiş yöntemini](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-upgrade-previous-version#swing-migration)kullanmayı düşünün. 
- En iyi yazma performansı için SQL veritabanı için SSD'yi kullanın.
- Etkin Dizin kapsamını yalnızca etki alanı, OU veya öznitelik filtreleme kullanarak Azure AD'de sağlanması gereken nesneleri içerecek şekilde filtreleyin.
- Varsayılan öznitelik akışı kurallarını değiştirmeniz gerekiyorsa, önce kuralı kopyalayın, sonra kopyayı değiştirin ve özgün kuralı devre dışı bırakın. Tam eşitlemeyi yeniden çalıştırmayı unutmayın.
- İlk tam eşitleme çalıştırma profili için yeterli zamanı planlayın.
- Delta senkronizasyon döngüsünü 30 dakikada tamamlamaya gayret edin. Delta eşitleme profili 30 dakika içinde tamamlanmazsa, varsayılan eşitleme sıklığını tam bir delta eşitleme döngüsü içerecek şekilde değiştirin.
- Azure [AD Connect eşitleme sistemizini](how-to-connect-health-agent-install.md) Azure AD'de izleyin.

## <a name="next-steps"></a>Sonraki adımlar
[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md) hakkında daha fazla bilgi edinin.
