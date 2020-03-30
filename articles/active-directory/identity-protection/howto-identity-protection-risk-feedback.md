---
title: Azure Active Directory Identity Protection'da risk geri bildirimi sağlayın
description: Kimlik Koruması risk algılamaları hakkında nasıl ve neden geri bildirim de sağlamalısınız.
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: conceptual
ms.date: 11/21/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4be8290f12d64f0c556100c63ec159bd414c6fcb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74382089"
---
# <a name="how-to-give-risk-feedback-in-azure-ad-identity-protection"></a>Nasıl Olunması: Azure AD Kimlik Koruması'nda risk geri bildirimi nde

Azure AD Kimlik Koruması, risk değerlendirmesi hakkında geri bildirimde Aşağıdaki belge, Azure AD Identity Protection'ın risk değerlendirmesi hakkında geri bildirimde bulunan senaryoları ve bunları nasıl dahil ettiğimizi listeler.

## <a name="what-is-a-detection"></a>Algılama nedir?

Kimlik Koruması algılaması, kimlik riski açısından şüpheli etkinliğin bir göstergesidir. Bu şüpheli faaliyetlere risk algılamaları denir. Bu kimlik tabanlı algılamalar buluşsal, makine öğrenimine veya ortak ürünlerden gelebilir. Bu tespitler oturum açma riskini ve kullanıcı riskini belirlemek için kullanılır,

* Kullanıcı riski, bir kimliğin tehlikeye girme olasılığını temsil eder.
* Oturum açma riski, oturum açma olasılığını temsil eder (örneğin, oturum açma kimlik sahibi tarafından yetkilendirilmez).

## <a name="why-should-i-give-risk-feedback-to-azure-ads-risk-assessments"></a>Azure AD'nin risk değerlendirmelerine neden risk geri bildirimi vermeliyim? 

Azure AD risk geri bildirimi vermenizin birkaç nedeni vardır:

- **Azure AD'nin kullanıcısını veya oturum açma risk değerlendirmesini yanlış buldunuz.** Örneğin, 'Riskli oturum açma' raporunda gösterilen bir oturum açma iyi huylu idi ve oturum açma daki tüm tespitler yanlış pozitifti.
- **Azure AD'nin kullanıcısının veya oturum açma risk değerlendirmesinin doğru olduğunu doğruladınız.** Örneğin, 'Riskli oturum açma' raporunda gösterilen bir oturum açma gerçekten kötü amaçlıydı ve Azure AD'nin oturum açma yla ilgili tüm algılamaların gerçek olumlu olduğunu bilmesini istiyorsunuz.
- **Azure AD Kimlik Koruması dışındaki kullanıcı üzerindeki riski düzelttin** ve kullanıcının risk düzeyinin güncelleştirilmesini istiyorsunuz.

## <a name="how-does-azure-ad-use-my-risk-feedback"></a>Azure AD risk geri bildirimimi nasıl kullanır?

Azure AD, geri bildiriminizi, temel kullanıcının riskini ve/veya oturum açma riskini ve bu olayların doğruluğunu güncellemek için kullanır. Bu geri bildirim, son kullanıcının güvenliğini sağlamaya yardımcı olur. Örneğin, oturum açmanın tehlikeye girildiğini onayladığınızda, Azure AD kullanıcının riskini ve oturum açma riskini (gerçek zamanlı risk değil) Hemen Yüksek'e yükseltir. Bu kullanıcı, Yüksek riskli kullanıcıları parolalarını güvenli bir şekilde sıfırlamaya zorlamak için kullanıcı risk politikanıza dahil edilirse, kullanıcı bir sonraki oturum açmada kendisini otomatik olarak düzeltecektir.

## <a name="how-should-i-give-risk-feedback-and-what-happens-under-the-hood"></a>Nasıl risk geribildirim vermeliyim ve başlık altında ne olur?

Azure AD'ye risk geri bildirimi nde yer alan senaryolar ve mekanizmalar aşağıda verilmiştir.

| Senaryo | Nasıl geribildirim vermek için? | Kaputun altında ne oluyor? | Notlar |
| --- | --- | --- | --- |
| **Oturum açma (Yanlış pozitif)** <br> 'Riskli oturum açma' raporu risk altında oturum açma [Risk durumu = Risk altında] gösterir, ancak oturum açma nın tehlikeye girmemesi gerekir. | Oturum açma seçeneğini seçin ve 'Oturum açma güvenlisini onaylayın'a tıklayın. | Azure AD, oturum açmanın toplam riskini hiçbir edoğru taşımaz [Risk durumu = Onaylandı güvenli; Risk düzeyi (Toplam) = -] ve kullanıcı riski üzerindeki etkisini tersine çevirecektir. | Şu anda, 'Oturum açma güvenlisini onaylayın' seçeneği yalnızca 'Riskli oturum açma' raporunda kullanılabilir. |
| **Oturum açma (Gerçek pozitif)** <br> 'Riskli oturum açma' raporu, risk altındaki oturum açmanın [Risk durumu = Risk Altında] düşük risk [Risk düzeyi (Toplam) = Düşük] olduğunu ve oturum açmanın gerçekten tehlikeye düştüğünü gösterir. | Oturum açma'yı seçin ve 'Oturum açmanın tehlikeye atını onaylayın'a tıklayın. | Azure AD, oturum açma nın toplam riskini ve kullanıcı riskini Yüksek [Risk durumu = Onaylandı gizliliği ihlal edildi; Risk düzeyi = Yüksek]. | Şu anda, 'Oturum açma nın gizliliğini zedelenini onaylayın' seçeneği yalnızca 'Riskli oturum açma' raporunda kullanılabilir. |
| **Kullanıcı tehlikeye (True pozitif)** <br> 'Riskli kullanıcılar' raporu, risk altındaki bir kullanıcıyı [Risk durumu = Risk altında] düşük risk [Risk düzeyi = Düşük] ve bu kullanıcının gerçekten tehlikeye düştüğünü gösterir. | Kullanıcıyı seçin ve 'Kullanıcının gizliliğini niçin onayladığını onayla'yı tıklayın. | Azure AD, kullanıcı riskini Yüksek [Risk durumu = Onaylandı gizliliği ihlal edildi; Risk düzeyi = Yüksek] ve yeni bir algılama 'Yönetici kullanıcı tehlikeye doğruladı' ekler. | Şu anda, 'Kullanıcının gizliliğini ihlal onaylayın' seçeneği yalnızca 'Riskli kullanıcılar' raporunda kullanılabilir. <br> 'Yönetici, kullanıcının tehlikeye attırdığını doğruladı' tespiti, 'Riskli kullanıcılar' raporunda 'Risk algılamaları oturum açmayla bağlantılı değildir' sekmesinde gösterilir. |
| **Azure AD Kimlik Koruması dışında düzeltilen kullanıcı (True pozitif + Düzeltilmiştir)** <br> 'Riskli kullanıcılar' raporu risk altındaki bir kullanıcıyı gösterir ve daha sonra kullanıcıyı Azure AD Kimlik Koruması dışında düzeltimişimdir. | 1. Kullanıcıyı seçin ve 'Kullanıcının gizliliğini zedele' seçeneğini tıklayın. (Bu işlem Azure AD'ye kullanıcının gerçekten gizliliğinin ihlal edildiğini doğrular.) <br> 2. Kullanıcının 'Risk düzeyi'nin Yüksek'e gitmesini bekleyin. (Bu süre Azure AD'ye yukarıdaki geri bildirimi risk motoruna götürmesi için gereken zamanı verir.) <br> 3. Kullanıcıyı seçin ve 'Kullanıcı riskini düşürün' seçeneğini tıklayın. (Bu işlem Azure AD'ye kullanıcının artık gizliliğinin ihlal edilemeyeceğini doğrular.) |  Azure AD, kullanıcı riskini hiçbir şekilde taşıyamaz [Risk durumu = Görevden Alındı; Risk düzeyi = -] ve aktif risk olan tüm mevcut oturum açma riskini kapatır. | 'Kullanıcı riskini düşür' seçeneğini tıklatmak, kullanıcı ve geçmiş oturum açma riskleri üzerindeki tüm riskleri kapatır. Bu eylem geri alınamaz. |
| **Kullanıcı tehlikeye değil (Yanlış pozitif)** <br> 'Riskli kullanıcılar' raporu risk altındaki kullanıcıyı gösterir, ancak kullanıcı tehlikeye girmez. | Kullanıcıyı seçin ve 'Kullanıcı riskini kapat' seçeneğini tıklayın. (Bu işlem Azure AD'ye kullanıcının gizliliğinin ihlal edilmediğini doğrular.) | Azure AD, kullanıcı riskini hiçbir şekilde taşıyamaz [Risk durumu = Görevden Alındı; Risk düzeyi = -]. | 'Kullanıcı riskini düşür' seçeneğini tıklatmak, kullanıcı ve geçmiş oturum açma riskleri üzerindeki tüm riskleri kapatır. Bu eylem geri alınamaz. |
| Ben kullanıcı riskini kapatmak istiyorum ama kullanıcı / güvenli tehlikeye olup olmadığından emin değilim. | Kullanıcıyı seçin ve 'Kullanıcı riskini kapat' seçeneğini tıklayın. (Bu işlem Azure AD'ye kullanıcının artık gizliliğinin ihlal edilemeyeceğini doğrular.) | Azure AD, kullanıcı riskini hiçbir şekilde taşıyamaz [Risk durumu = Görevden Alındı; Risk düzeyi = -]. | 'Kullanıcı riskini düşür' seçeneğini tıklatmak, kullanıcı ve geçmiş oturum açma riskleri üzerindeki tüm riskleri kapatır. Bu eylem geri alınamaz. 'Parolayı sıfırla'yı tıklatarak kullanıcıyı düzeltmenizi veya kullanıcıdan kimlik bilgilerini güvenli bir şekilde sıfırlamasını/değiştirmesini istemenizi öneririz. |

Kimlik Koruması'ndaki kullanıcı risk algılamaları hakkındaki geri bildirimler çevrimdışı işlenir ve güncellenmesi biraz zaman alabilir. Risk işleme durumu sütunu, geri bildirim işlemenin geçerli durumunu sağlar.

![Riskli kullanıcı raporu için risk işleme durumu](./media/howto-identity-protection-risk-feedback/risky-users-provide-feedback.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Active Directory Identity Protection risk algılamaları başvurusu](risk-events-reference.md)
