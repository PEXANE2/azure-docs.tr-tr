---
title: Risk nedir? Azure AD Kimlik Koruması
description: Azure AD Kimlik Koruması'nda riski açıklama
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: conceptual
ms.date: 10/18/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 775ff6b3ba003bed22ccd5a42cb4da005c4dbb69
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79253695"
---
# <a name="what-is-risk"></a>Risk nedir?

Azure AD Kimlik Koruması'ndaki risk algılamaları, dizindeki kullanıcı hesaplarıyla ilgili tanımlanmış şüpheli eylemleri içerir.

Kimlik Koruması, kuruluşların bu şüpheli eylemleri hızlı bir şekilde görmek ve bunlara yanıt vermek için güçlü kaynaklara erişmelerini sağlar. 

![Riskli kullanıcıları ve oturum açmaları gösteren güvenliğe genel bakış](./media/concept-identity-protection-risks/identity-protection-security-overview.png)

## <a name="risk-types-and-detection"></a>Risk türleri ve algılama

İki tür risk vardır **Kullanıcı** ve **Oturum Açma** ve iki tür algılama veya hesaplama **Gerçek zamanlı** ve **Çevrimdışı.**

### <a name="user-risk"></a>Kullanıcı riski

Kullanıcı riski, belirli bir kimliğin veya hesabın tehlikeye girme olasılığını temsil eder. 

Bu riskler, güvenlik araştırmacıları, emniyet uzmanları, Microsoft'taki güvenlik ekipleri ve diğer güvenilen kaynaklar da dahil olmak üzere Microsoft'un dahili ve harici tehdit istihbarat kaynakları kullanılarak çevrimdışı olarak hesaplanır.

| Risk tespiti | Açıklama |
| --- | --- |
| Sızdırılan kimlik bilgileri | Bu risk algılama türü, kullanıcının geçerli kimlik bilgilerinin sızdırıldığını gösterir. Siber suçlular yasal kullanıcıların geçerli parolalarını tehlikeye attıklarında, genellikle bu kimlik bilgilerini paylaşırlar. Bu paylaşım genellikle karanlık web, yapıştırma siteleri, kamuya yayın veya ticaret ve karaborsada kimlik bilgileri satarak yapılır. Microsoft sızdırılan kimlik bilgileri hizmeti karanlık web, yapıştırma sitelerinden veya diğer kaynaklardan kullanıcı kimlik bilgileri aldığında, geçerli eşleşmeleri bulmak için Azure AD kullanıcılarının geçerli kimlik bilgileriyle karşılaştırılır. |
| Azure AD tehdit zekası | Bu risk algılama türü, verilen kullanıcı için alışılmadık olan veya Microsoft'un iç ve dış tehdit istihbarat kaynaklarına dayalı olarak bilinen saldırı kalıplarıyla tutarlı olan kullanıcı etkinliğini gösterir. |

### <a name="sign-in-risk"></a>Oturum açma riski

Oturum açma riski, belirli bir kimlik doğrulama isteğinin kimlik sahibi tarafından yetkilendirilmeyen olasılığını gösterir. 

Bu riskler, güvenlik araştırmacıları, emniyet güçleri uzmanları, Microsoft'taki güvenlik ekipleri ve diğer güvenilen kaynaklar da dahil olmak üzere Microsoft'un dahili ve harici tehdit istihbarat kaynakları kullanılarak gerçek zamanlı olarak veya çevrimdışı olarak hesaplanabilir.

| Risk tespiti | Algılama türü | Açıklama |
| --- | --- | --- |
| Anonim IP adresi | Gerçek zamanlı | Bu risk algılama türü, anonim bir IP adresinden (örneğin, Tor tarayıcısı veya anonim VPN) oturum açma ları gösterir. Bu IP adresleri genellikle giriş telemetrilerini (IP adresi, konum, aygıt, vb.) kötü amaçlı olarak gizlemek isteyen aktörler tarafından kullanılır. |
| Atipik seyahat | Çevrimdışı | Bu risk algılama türü, geçmiş davranışlar göz önüne alındığında, konumlardan en az birinin kullanıcı için atipik olabileceği coğrafi olarak uzak konumlardan kaynaklanan iki oturum açma yı tanımlar. Diğer bazı faktörlerin yanı sıra, bu makine öğrenme algoritması, iki oturum açma arasındaki süreyi ve kullanıcının ilk konumdan ikinci ye seyahat etmesi için gereken süreyi dikkate alır ve farklı bir kullanıcının aynı Kimlik bilgi -leri. <br><br> Algoritma, VPN'ler ve kuruluştaki diğer kullanıcılar tarafından düzenli olarak kullanılan konumlar gibi imkansız seyahat koşullarına katkıda bulunan bariz "yanlış pozitifleri" yok sayar. Sistem, 14 gün veya 10 giriş en erken bir başlangıç öğrenme süresi vardır, bu süre boyunca yeni bir kullanıcının oturum açma davranışını öğrenir. |
| Kötü amaçlı yazılım bağlantılı IP adresi | Çevrimdışı | Bu risk algılama türü, bir bot sunucusuyla etkin olarak iletişim kurduğu bilinen kötü amaçlı yazılımla enfekte olan IP adreslerinden oturum açma ları gösterir. Bu algılama, kullanıcının cihazının IP adreslerinin, bot sunucusu etkinken bir bot sunucusuyla temas halinde olan IP adresleriyle ilişkilendirilerek belirlenir. |
| Bilmediğiniz oturum açma özellikleri | Gerçek zamanlı | Bu risk algılama türü, anormal işaret-ins aramak için geçmiş oturum geçmişi (IP, Enlem / Boylam ve ASN) dikkate alır. Sistem, kullanıcı tarafından kullanılan önceki konumlar la ilgili bilgileri depolar ve bu "tanıdık" konumları dikkate alır. Oturum açma, tanıdık konumlar listesinde zaten olmayan bir konumdan gerçekleştiğinde risk algılama tetiklenir. Yeni oluşturulan kullanıcılar, algoritmalarımız kullanıcının davranışını öğrenirken, yabancı oturum açma özelliklerinin risk tespitlerinin kapatılacağı bir süre için "öğrenme modunda" olacaktır. Öğrenme modu süresi dinamiktir ve algoritmanın kullanıcının oturum açma desenleri hakkında yeterli bilgi toplamasının ne kadar zaman aldığına bağlıdır. Minimum süre beş gündür. Bir kullanıcı uzun bir hareketsizlik döneminden sonra öğrenme moduna geri dönebilir. Sistem ayrıca, tanıdık aygıtlardan gelen oturum açma ları ve coğrafi olarak tanıdık bir konuma yakın olan konumları da yok sayar. <br><br> Bu algılamayı temel kimlik doğrulama (veya eski protokoller) için de çalıştırıyoruz. Bu protokoller istemci kimliği gibi modern özelliklere sahip olmadığından, yanlış pozitifleri azaltmak için sınırlı bir telemetri vardır. Müşterilerimizin modern kimlik doğrulamasına geçmelerini öneririz. |
| Yönetici doğruladı kullanıcı tehlikeye | Çevrimdışı | Bu algılama, bir yöneticinin Riskli kullanıcı Kullanıcı UI'sinde 'Kullanıcının gizliliğini bozun' veya riskli Kullanıcı API'sini kullanarak 'Kullanıcının gizliliğini bozun' seçtiğini gösterir. Hangi yöneticinin bu kullanıcının gizliliğini ihlal ettiğine dair onayını almak için, kullanıcının risk geçmişini kontrol edin (Kullanıcı arabirimi veya API üzerinden). |
| Kötü amaçlı IP adresi | Çevrimdışı | Bu algılama, kötü amaçlı bir IP adresinden oturum açmayı gösterir. IP adresi, IP adresinden veya diğer IP itibar kaynaklarından alınan geçersiz kimlik bilgileri nedeniyle yüksek hata oranlarına bağlı olarak kötü amaçlı olarak kabul edilir. |
| Şüpheli gelen kutusu düzenleme kuralları | Çevrimdışı | Bu algılama Microsoft [Cloud App Security (MCAS)](/cloud-app-security/anomaly-detection-policy#suspicious-inbox-manipulation-rules)tarafından keşfedilmiştir. Bu algılama, ortamınızın profilini koyar ve iletileri veya klasörleri kullanıcının gelen kutusunda silen veya hareket ettiren şüpheli kurallar ayarlandığında uyarıları tetikler. Bu, kullanıcının hesabının gizliliğinin ihlal edildiğini, iletilerin kasıtlı olarak gizlendiğini ve posta kutusunun kuruluşunuzdaki istenmeyen postaları veya kötü amaçlı yazılımları dağıtmak için kullanıldığını gösterebilir. |
| Mümkün olmayan seyahat | Çevrimdışı | Bu algılama Microsoft [Cloud App Security (MCAS)](/cloud-app-security/anomaly-detection-policy#impossible-travel)tarafından keşfedilmiştir. Bu algılama, kullanıcının ilk konumdan ikinci konuma seyahat etmesini gerektiren süreden daha kısa bir süre içinde coğrafi olarak uzak yerlerden kaynaklanan iki kullanıcı faaliyetini (tek veya birden çok oturumdur) tanımlar ve farklı bir kullanıcıaynı kimlik bilgilerini kullanıyor. |

### <a name="other-risk-detections"></a>Diğer risk tespitleri

| Risk tespiti | Algılama türü | Açıklama |
| --- | --- | --- |
| Ek risk algılandı | Gerçek zamanlı veya Çevrimdışı | Bu algılama, yukarıdaki premium algılamalardan birinin algılandığını gösterir. Premium algılamalar yalnızca Azure AD Premium P2 müşterileri tarafından görülebildiğinden, Azure AD Premium P2 lisansı olmayan müşteriler için "ek risk algılanır". |

## <a name="next-steps"></a>Sonraki adımlar

- [Riskleri azaltmak için kullanılabilen politikalar](concept-identity-protection-policies.md)

- [Güvenliğe genel bakış](concept-identity-protection-security-overview.md)
