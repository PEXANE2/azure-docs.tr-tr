---
title: Azure AD parola koruması - Azure Active Directory
description: Azure AD parola koruması kullanarak şirket içi Active Directory'de zayıf parolaları yasaklama
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 02/18/2018
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: jsimmons
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1b9d07099f8de996181948921330ef6744b302a8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74848655"
---
# <a name="enforce-azure-ad-password-protection-for-windows-server-active-directory"></a>Windows Server Active Directory için Azure AD parola korumasını zorunlu tutma

Azure AD parola koruması, bir kuruluştaki parola ilkelerini geliştiren bir özelliktir. Parola korumasının şirket içi dağıtımı, Azure AD'de depolanan genel ve özel yasaklı parola listelerini kullanır. Azure AD'nin bulut tabanlı değişiklikler için yaptığı denetimlerin aynısını şirket içinde yapar. Bu denetimler parola değişiklikleri ve parola sıfırlama senaryoları sırasında gerçekleştirilir.

## <a name="design-principles"></a>Tasarım ilkeleri

Azure AD parola koruması aşağıdaki ilkeler göz önünde bulundurularak tasarlanmıştır:

* Etki alanı denetleyicileri internet ile doğrudan iletişim kurmak zorunda asla.
* Etki alanı denetleyicilerinde yeni ağ bağlantı noktaları açılmaz.
* Etkin Dizin şeması değişikliği gerekmez. Yazılım varolan Active Directory **kapsayıcı** ve **serviceConnectionPoint** şema nesneleri kullanır.
* Minimum Active Directory etki alanı veya orman işlevsel düzeyi (DFL/FFL) gerekmez.
* Yazılım, koruduğu Active Directory etki alanlarında hesap oluşturmaz veya gerektirmez.
* Kullanıcı açık metin parolaları, parola doğrulama işlemleri sırasında veya başka bir zamanda etki alanı denetleyicisinden asla ayrılmaz.
* Yazılım diğer Azure AD özelliklerine bağlı değildir; örneğin Azure AD parolası karma eşitlemesi ilişkili değildir ve Azure AD parola korumasının çalışması için gerekli değildir.
* Artımlı dağıtım desteklenir, ancak parola ilkesi yalnızca Etki Alanı Denetleyiciaracısı (DC Agent) yüklendiğinde uygulanır. Daha fazla bilgi için sonraki konuya bakın.

## <a name="incremental-deployment"></a>Artımlı dağıtım

Azure AD parola koruması, Etkin Dizin etki alanında etki alanı denetleyicileri arasında artımlı dağıtımı destekler, ancak bunun gerçekte ne anlama geldiğini ve dengelerin ne olduğunu anlamak önemlidir.

Azure AD parola koruma DC aracı yazılımı parolaları yalnızca etki alanı denetleyicisine yüklendiğinde ve yalnızca bu etki alanı denetleyicisine gönderilen parola değişiklikleri için doğrulayabilir. Kullanıcı parolası değişikliklerini işlemek için Windows istemci makineleri tarafından hangi etki alanı denetleyicilerinin seçildiğini denetlemek mümkün değildir. Tutarlı davranışı ve evrensel parola koruma güvenlik zorlamasını garanti etmek için DC aracı yazılımı bir etki alanındaki tüm etki alanı denetleyicilerine yüklenmelidir.

Birçok kuruluş, tam dağıtım yapmadan önce etki alanı denetleyicilerinin bir alt kümesinde Azure AD parola korumasının dikkatli bir şekilde test edilmesini ister. Azure AD parola koruması kısmi dağıtımı destekler, yani belirli bir DC'deki DC aracı yazılımı, etki alanında bulunan diğer DC'ler DC aracı yazılımı yüklü olmasa bile parolaları etkin bir şekilde doğrular. Bu tür kısmi dağıtımlar güvenli DeğİlDir ve sınama amaçları dışında tavsiye edilmez.

## <a name="architectural-diagram"></a>Mimari şema

Azure AD parola korumasını şirket içi Active Directory ortamında dağıtmadan önce temel tasarım ve işlev kavramlarını anlamak önemlidir. Aşağıdaki diyagram, parola koruma bileşenlerinin birlikte nasıl çalıştığını gösterir:

![Azure AD parola koruma bileşenleri birlikte nasıl çalışır?](./media/concept-password-ban-bad-on-premises/azure-ad-password-protection.png)

* Azure AD Parola Koruma Proxy hizmeti, geçerli Active Directory ormanındaki etki alanına bağlı herhangi bir makinede çalışır. Birincil amacı, etki alanı denetleyicilerinden parola ilkesi indirme isteklerini Azure AD'ye iletmektir. Ardından Azure AD'deki yanıtları etki alanı denetleyicisine döndürür.
* DC Agent'ın parola filtresi DLL işletim sisteminden kullanıcı parola doğrulama istekleri alır. Bunları etki alanı denetleyicisi üzerinde yerel olarak çalışan DC Agent hizmetine iletir.
* Parola korumasının DC Agent hizmeti, DC Agent'ın parola filtresi DLL'den parola doğrulama isteklerini alır. Geçerli (yerel olarak kullanılabilir) parola ilkesini kullanarak bunları işler ve sonucu döndürür: *geçmek* veya *başarısız.*

## <a name="how-password-protection-works"></a>Parola koruması nasıl çalışır?

Her Azure AD Parola Koruma Proxy hizmeti örneği, Active Directory'de bir **serviceConnectionPoint** nesnesi oluşturarak kendisini ormandaki etki alanı denetleyicilerine tanıtıyor.

Parola koruması için her DC Agent hizmeti, Active Directory'de bir **serviceConnectionPoint** nesnesi de oluşturur. Bu nesne öncelikle raporlama ve tanılama için kullanılır.

DC Agent hizmeti, Azure AD'den yeni bir parola ilkesinin karşıdan yüklenemeye başlanmasından sorumludur. İlk adım, proxy **hizmetiConnectionPoint** nesneleri için orman sorgulayarak bir Azure AD Parola Koruma Proxy hizmeti bulmaktır. Kullanılabilir bir proxy hizmeti bulunduğunda, DC Aracısı proxy hizmetine bir parola ilkesi indirme isteği gönderir. Proxy hizmeti de isteği Azure AD'ye gönderir. Proxy hizmeti daha sonra yanıtı DC Agent hizmetine döndürür.

DC Agent hizmeti Azure AD'den yeni bir parola ilkesi aldıktan sonra, hizmet ilkeyi etki alanı *sysvol* klasörü paylaşımının kökünde özel bir klasörde saklar. DC Agent hizmeti, etki alanında ki diğer DC Agent hizmetlerinden yeni ilkelerin çoğaltılması durumunda bu klasörü de izler.

DC Agent hizmeti, hizmet başlangıç hizmetinde her zaman yeni bir ilke ister. DC Agent hizmeti başlatıldıktan sonra, geçerli yerel kullanılabilir ilkenin yaşını saatlik olarak denetler. İlke bir saatten eskiyse, DC Aracısı daha önce açıklandığı gibi proxy hizmeti aracılığıyla Azure AD'den yeni bir ilke ister. Geçerli ilke bir saatten eski değilse, DC Agent bu ilkeyi kullanmaya devam ediyor.

Azure AD parola koruma parola ilkesi indirildiğinde, bu ilke kiracıya özgüdür. Başka bir deyişle, parola ilkeleri her zaman Microsoft genel yasaklı parola listesi ve kiracı başına özel yasaklı parola listesinin bir birleşimidir.

DC Agent TCP üzerinden RPC üzerinden proxy hizmeti ile iletişim kurar. Proxy hizmeti, yapılandırmaya bağlı olarak dinamik veya statik bir RPC bağlantı noktasında bu çağrıları dinler.

DC Agent asla ağ kullanılabilir bir bağlantı noktasında dinlemez.

Proxy hizmeti hiçbir zaman DC Agent hizmetini aramaz.

Proxy hizmeti devletsizdir. Azure'dan indirilen ilkeleri veya başka bir durumu asla önbelleğe almaz.

DC Agent hizmeti, bir kullanıcının parolasını değerlendirmek için her zaman en son yerel olarak kullanılabilen parola ilkesini kullanır. Yerel DC'de parola ilkesi yoksa, parola otomatik olarak kabul edilir. Bu durumda, yöneticiyi uyarmak için bir olay iletisi günlüğe kaydedilir.

Azure AD parola koruması gerçek zamanlı bir ilke uygulama altyapısı değildir. Azure AD'de parola ilkesi yapılandırma sıcağa geçiş insa edilmesiyle bu değişikliğin tüm etki alanı denetleyicilerine ulaştığı ve uygulandığı arasında bir gecikme olabilir.

Azure AD parola koruması, bir yedek değil, varolan Active Directory parola ilkelerine ek olarak hareket eder. Bu, yüklenmiş olabilecek diğer üçüncü taraf parola filtresi dll'lerini içerir. Etkin Dizin her zaman parola kabul etmeden önce tüm parola doğrulama bileşenlerinin aynı fikirde olduğunu gerektirir.

## <a name="foresttenant-binding-for-password-protection"></a>Parola koruması için orman/kiracı bağlama

Azure AD parola korumasının Etkin Dizin ormanında dağıtılması için bu ormanın Azure AD ile kaydedilmesi gerekir. Dağıtılan her proxy hizmetinin Azure AD'ye de kaydedilmesi gerekir. Bu orman ve proxy kayıtları, kayıt sırasında kullanılan kimlik bilgileri tarafından dolaylı olarak tanımlanan belirli bir Azure AD kiracısıyla ilişkilidir.

Etkin Dizin ormanı ve bir orman içinde dağıtılan tüm proxy hizmetleri aynı kiracıya kaydedilmelidir. Etkin Dizin ormanı veya bu ormanda farklı Azure AD kiracılarına kayıtlı herhangi bir proxy hizmeti nin olması desteklenmez. Bu tür yanlış yapılandırılmış bir dağıtımın belirtileri arasında parola ilkelerinin karşıdan yüklenememe leri yer almaktadır.

## <a name="download"></a>İndirme

Azure AD parola koruması için gerekli iki aracı yükleyicimicrosoft [Download Center'dan](https://www.microsoft.com/download/details.aspx?id=57071)edinilebilir.

## <a name="next-steps"></a>Sonraki adımlar
[Azure AD parola korumasını dağıtma](howto-password-ban-bad-on-premises-deploy.md)
