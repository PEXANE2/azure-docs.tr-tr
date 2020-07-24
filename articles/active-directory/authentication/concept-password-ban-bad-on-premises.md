---
title: Azure AD parola koruması-Azure Active Directory
description: Azure AD parola korumasını kullanarak şirket içi Active Directory Domain Services ortamlarında zayıf parolalar
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/17/2020
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: jsimmons
ms.collection: M365-identity-device-management
ms.openlocfilehash: feb7c4a4417d64e039793bd96141c965f6437414
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87050931"
---
# <a name="enforce-on-premises-azure-ad-password-protection-for-active-directory-domain-services"></a>Active Directory Domain Services için şirket içi Azure AD parola korumasını zorlayın

Azure AD parola koruması, bilinen zayıf parolaları ve bunların türevlerini algılar ve engeller ve ayrıca kuruluşunuza özgü ek zayıf terimleri engelleyebilir. Azure AD parola koruması 'nın şirket içi dağıtımı, Azure AD 'de depolanan aynı küresel ve özel yasaklanmış parola listelerini kullanır ve Azure AD 'de bulut tabanlı değişiklikler için aynı denetim, şirket içi parola değişikliklerine yönelik olarak değişir. Bu denetimler, parola değişiklikleri sırasında ve şirket içi Active Directory Domain Services (AD DS) etki alanı denetleyicilerine karşı parola sıfırlama olayları sırasında gerçekleştirilir.

## <a name="design-principles"></a>Tasarım ilkeleri

Azure AD parola koruması Aşağıdaki ilkeler göz önünde bulundurularak tasarlanmıştır:

* Etki alanı denetleyicileri (DC 'Ler) hiç Internet ile doğrudan iletişim kurmak zorunda değildir.
* DC 'lerde hiçbir yeni ağ bağlantı noktası açılmadı.
* AD DS şeması değişikliği gerekli değildir. Yazılım, mevcut AD DS *kapsayıcısını* ve *serviceConnectionPoint* şema nesnelerini kullanır.
* En az AD DS etki alanı veya orman işlev düzeyi (DFL/FFL) gerekli değildir.
* Yazılım, koruduğu AD DS etki alanlarında hesap oluşturmaz veya gerektirmez.
* Kullanıcı şifresiz-metin parolaları, parola doğrulama işlemleri sırasında veya herhangi bir zamanda etki alanı denetleyicisini hiçbir zaman bırakmaz.
* Yazılım diğer Azure AD özelliklerine bağımlı değildir. Örneğin, Azure AD Parola karması eşitleme (PHS) ilgili değildir veya Azure AD parola koruması için gerekli değildir.
* Artımlı dağıtım desteklenir, ancak parola ilkesi yalnızca etki alanı denetleyicisi aracısının (DC Aracısı) yüklendiği yerde zorlanır.

## <a name="incremental-deployment"></a>Artımlı dağıtım

Azure AD parola koruması, bir AD DS etki alanındaki DC 'lerde artımlı dağıtımı destekler. Bu gerçekten ne anlama geldiğini ve dengelerin ne olduğunu anlamak önemlidir.

Azure AD parola koruması DC Aracısı yazılımı yalnızca DC 'ye yüklendiğinde ve yalnızca bu DC 'ye gönderilen parola değişiklikleri için parolaları doğrulayabilir. Kullanıcı parolası değişikliklerini işlemek için Windows istemci makineleri tarafından hangi DC 'Lerin seçili olduğunu denetlemek mümkün değildir. Tutarlı davranış ve evrensel Azure AD parola koruması güvenlik zorlaması sağlamak için, DC Aracısı yazılımının bir etki alanındaki tüm DC 'Lere yüklenmesi gerekir.

Birçok kuruluş, tam dağıtımdan önce DC 'lerin bir alt kümesinde Azure AD parola korumasını dikkatle test etmek ister. Bu senaryoyu desteklemek için, Azure AD parola koruması kısmi dağıtımı destekler. Belirli bir DC 'deki DC Aracısı yazılımı, etki alanındaki diğer DC 'Ler DC Aracısı yazılımı yüklü olmadığında bile parolaları etkin bir şekilde doğrular. Bu türün kısmi dağıtımları güvenli değildir ve test amaçları için dışında önerilmez.

## <a name="architectural-diagram"></a>Mimari şema

Azure AD parola korumasını şirket içi AD DS ortamında dağıtmadan önce, temel alınan tasarım ve işlev kavramlarını anlamak önemlidir. Aşağıdaki diyagramda Azure AD parola koruması bileşenlerinin nasıl birlikte çalıştığı gösterilmektedir:

![Azure AD parola koruma bileşenlerinin birlikte çalışması](./media/concept-password-ban-bad-on-premises/azure-ad-password-protection.png)

* Azure AD parola koruma proxy hizmeti, geçerli AD DS ormanındaki etki alanına katılmış herhangi bir makinede çalışır. Hizmetin birincil amacı, DC 'lerden Azure AD 'ye parola ilkesi indirme isteklerini iletmektir ve ardından Azure AD 'den DC 'ye yanıtları döndürür.
* DC aracısının parola filtresi DLL 'SI, işletim sisteminden Kullanıcı parolası doğrulama isteklerini alır. Filtre, bunları DC üzerinde yerel olarak çalışan DC Aracısı Hizmetine iletir.
* Azure AD parola koruması 'nın DC Aracısı hizmeti, DC aracısının parola filtresi DLL 'sinden parola doğrulama isteklerini alır. DC Aracısı hizmeti, geçerli (yerel olarak kullanılabilir) parola ilkesini kullanarak bunları işler ve *Pass* veya *Fail*sonucunu döndürür.

## <a name="how-azure-ad-password-protection-works"></a>Azure AD parola koruması nasıl çalışacaktır?

Şirket içi Azure AD parola koruması bileşenleri aşağıdaki gibi çalışır:

1. Her Azure AD parola koruma proxy hizmeti örneği, Active Directory içinde bir *serviceConnectionPoint* nesnesi oluşturarak kendisini ormandaki DC 'lere tanıtır.

    Azure AD parola koruması için her DC Aracısı hizmeti ayrıca Active Directory bir *serviceConnectionPoint* nesnesi oluşturur. Bu nesne öncelikle raporlama ve Tanılama için kullanılır.

1. DC Aracısı hizmeti, Azure AD 'den yeni bir parola ilkesinin indirilmesini başlatmaktan sorumludur. İlk adım, proxy *serviceConnectionPoint* nesneleri için ormanı sorgulayarak BIR Azure AD parola koruma proxy hizmeti bulmaya yöneliktir.

1. Kullanılabilir bir proxy hizmeti bulunduğunda, DC Aracısı proxy hizmetine bir parola ilkesi indirme isteği gönderir. İçindeki proxy hizmeti, isteği Azure AD 'ye gönderir ve ardından DC Aracısı hizmetine yanıtı döndürür.

1. DC Aracısı hizmeti, Azure AD 'den yeni bir parola ilkesi aldıktan sonra, bu ilkeyi etki alanı *SYSVOL* klasörü paylaşımının kökündeki ayrılmış bir klasörde depolar. DC Aracısı hizmeti, yeni ilkelerin ' de etki alanındaki diğer DC Aracısı hizmetlerinden çoğaltılmasına karşı bu klasörü de izler.

1. DC Aracısı hizmeti her zaman hizmet başlangıcında yeni bir ilke ister. DC Aracısı hizmeti başlatıldıktan sonra, geçerli yerel olarak kullanılabilir ilkenin yaşını saat olarak denetler. İlke bir saatten eskiyse, DC Aracısı daha önce açıklandığı gibi proxy hizmeti aracılığıyla Azure AD 'den yeni bir ilke ister. Geçerli ilke bir saatten eski değilse, DC Aracısı bu ilkeyi kullanmaya devam eder.

1. DC tarafından parola değiştirme olayları alındığında, yeni parolanın kabul edilip edilmediğine yönelik önbelleğe alınmış ilke kullanılır.

### <a name="key-considerations-and-features"></a>Önemli konular ve Özellikler

* Bir Azure AD parola koruma parolası ilkesi indirildiğinde, bu ilke bir kiracıya özeldir. Diğer bir deyişle, parola ilkeleri her zaman Microsoft Global yasaklanmış-Password listesinin ve kiracı başına özel yasaklanmış-Password listesinin bir birleşimidir.
* DC Aracısı, TCP üzerinden RPC aracılığıyla proxy hizmeti ile iletişim kurar. Proxy hizmeti, yapılandırmaya bağlı olarak, bu çağrıları dinamik veya statik bir RPC bağlantı noktasında dinler.
* DC Aracısı, ağda kullanılabilir bir bağlantı noktasını hiçbir şekilde dinlemiyor.
* Proxy hizmeti hiçbir şekilde DC Aracısı hizmetini çağırmayın.
* Proxy hizmeti durum bilgisiz. İlkeleri veya Azure 'dan indirilen başka bir durumu hiçbir şekilde önbelleğe almaz.
* DC Aracısı hizmeti, kullanıcının parolasını değerlendirmek için her zaman en son yerel olarak kullanılabilir parola ilkesini kullanır. Yerel DC 'de parola ilkesi yoksa, parola otomatik olarak kabul edilir. Bu durumda, yöneticinin uyarmak için bir olay iletisi günlüğe kaydedilir.
* Azure AD parola koruması, gerçek zamanlı bir ilke uygulama altyapısı değildir. Azure AD 'de parola ilkesi yapılandırma değişikliği yapıldığında ve bu değişikliğin tüm DC 'lerde ne zaman uygulandığı ve zorlandığında bir gecikme olabilir.
* Azure AD parola koruması, mevcut AD DS parola ilkelerine ek olarak görev yapar. Bu, yüklenebilen diğer 3. taraf parola filtresi dll 'leri içerir. AD DS her zaman, parolayı kabul etmeden önce tüm parola doğrulama bileşenlerinin kabul edilmesini gerektirir.

## <a name="forest--tenant-binding-for-azure-ad-password-protection"></a>Azure AD parola koruması için orman/kiracı bağlama

Azure AD parola korumasının bir AD DS ormanında dağıtılması için Azure AD ile ilgili ormanın kaydettirilmesi gerekir. Dağıtılan her Proxy hizmetinin da Azure AD 'ye kayıtlı olması gerekir. Bu orman ve proxy kayıtları, kayıt sırasında kullanılan kimlik bilgileri tarafından örtük olarak belirlenen belirli bir Azure AD kiracısı ile ilişkilendirilir.

AD DS ormanı ve bir orman içindeki tüm dağıtılan proxy hizmetleri aynı kiracıya kaydedilmelidir. Bu ormanda farklı Azure AD kiracılarına Kaydolmakta olan bir AD DS ormanı veya herhangi bir proxy hizmetine sahip olmak desteklenmez. Yanlış yapılandırılmış bir dağıtımın belirtileri, parola ilkelerini indirimeme ' yı içerir.

## <a name="download"></a>İndir

Azure AD parola koruması için gereken iki aracı yükleyicisi, [Microsoft Indirme merkezi](https://www.microsoft.com/download/details.aspx?id=57071)' nden edinilebilir.

## <a name="next-steps"></a>Sonraki adımlar

Şirket içi Azure AD parola korumasını kullanmaya başlamak için aşağıdaki nasıl yapılır:

> [!div class="nextstepaction"]
> [Şirket içi Azure AD parola korumasını dağıtma](howto-password-ban-bad-on-premises-deploy.md)
