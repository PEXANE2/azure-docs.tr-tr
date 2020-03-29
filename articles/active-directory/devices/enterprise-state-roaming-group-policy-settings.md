---
title: ESR için Grup İlkesi ve MDM ayarları - Azure Active Directory
description: Kurumsal Durum Dolaşımı için yönetim ayarları
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: troubleshooting
ms.date: 02/12/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: na
ms.collection: M365-identity-device-management
ms.openlocfilehash: bdffbc3a140bd13dcd6d352db8c192803d39b03e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78672361"
---
# <a name="group-policy-and-mdm-settings"></a>Grup İlkesi ve MDM ayarları

Bu ilkeler kullanıcının tüm aygıtına uygulandığından, bu grup ilkesini ve mobil cihaz yönetimi (MDM) ayarlarını yalnızca şirkete ait cihazlarda kullanın. Kişisel, kullanıcıya ait bir cihaz için ayarlar eşitlemesini devre dışı kılabilir bir MDM ilkesi uygulamak, söz sahibi aygıtın kullanımını olumsuz etkiler. Ayrıca, aygıttaki diğer kullanıcı hesapları da ilkeden etkilenir.

Kişisel (yönetilmeyen) aygıtlar için dolaşımı yönetmek isteyen işletmeler, Grup İlkesi veya MDM kullanmak yerine dolaşımı etkinleştirmek veya devre dışı etmek için Azure portalını kullanabilir.
Aşağıdaki tablolarda kullanılabilir ilke ayarları açıklayınız.

> [!NOTE]
> Bu makale, Temmuz 2015'te Windows 10 ile başlatılan Microsoft Edge Legacy HTML tabanlı tarayıcı için geçerlidir. Makale, 15 Ocak 2020'de yayımlanan yeni Microsoft Edge Krom tabanlı tarayıcı için geçerli değildir. Yeni Microsoft Edge için Eşitleme davranışı hakkında daha fazla bilgi için [Microsoft Edge Eşitleme](/deployedge/microsoft-edge-enterprise-sync)makalesine bakın.

## <a name="mdm-settings"></a>MDM ayarları

MDM ilke ayarları hem Windows 10 hem de Windows 10 Mobile için geçerlidir.  Windows 10 Mobile desteği yalnızca Microsoft hesabı tabanlı kullanıcının OneDrive hesabı üzerinden dolaşım için vardır. Azure AD tabanlı eşitleme için hangi aygıtların desteklendirilip desteklendirilene ilişkin ayrıntılar için [Aygıtlar ve uç noktalara](enterprise-state-roaming-windows-settings-reference.md) bakın.

| Adı | Açıklama |
| --- | --- |
| Microsoft Hesap Bağlantısına İzin Ver |Kullanıcıların aygıtta bir Microsoft hesabı kullanarak kimlik doğrulaması yapmanızı sağlar |
| Ayarlarımı Eşitle'ye Izin Ver |Kullanıcıların Windows ayarlarında ve uygulama verilerinde gezinmelerine izin verir; Bu politikayı devre dışı bırakmak, mobil cihazlardaki yedeklemelerin yanı sıra eşitlemeyi de devre dışı bırakacaktır |

## <a name="group-policy-settings"></a>Grup ilkesi ayarları

Grup ilkesi ayarları, Etkin Dizin etki alanına birleştirilmiş Windows 10 aygıtları için geçerlidir. Tablo ayrıca eşitleme ayarlarını yönetmek için görünen, ancak windows 10 için Enterprise State Roaming için çalışmayan ve açıklamata 'Kullanmayın' ile birlikte belirtilen eski ayarları da içerir.

Bu ayarlar şu adreste bulunur:`Computer Configuration > Administrative Templates > Windows Components > Sync your settings` 

| Adı | Açıklama |
| --- | --- |
| Hesaplar: Microsoft Hesaplarını Engelle |Bu ilke ayarı, kullanıcıların bu bilgisayara yeni Microsoft hesapları eklemesini engeller |
| Eşitleme yapma |Kullanıcıların Windows ayarlarında ve uygulama verilerinde gezinmesini önler |
| Kişiselleştirme |Temalar grubunun eşitlemeyi devre dışı |
| Tarayıcı ayarlarını senkronize etmeyin |Internet Explorer grubunun eşitlemeyi devre dışı |
| Parolaları eşitleme |Parolalar grubunun eşitlemeyi devre dışı |
| Diğer Windows ayarlarını eşitleme |Diğer Windows ayarları grubunun eşitlemeyi devre dışı |
| Masaüstü kişiselleştirmeyi senkronize etmeyin |Kullanmayın; hiçbir etkisi yoktur |
| Tarifeli bağlantılarda eşitleme yapmayın |Hücresel 3G gibi tarifeli bağlantılarda dolaşımı devre dışı |
| Uygulamaları senkronize etmeyin |Kullanmayın; hiçbir etkisi yoktur |
| Uygulama ayarlarını senkronize etmeyin |Uygulama verilerinin dolaşımını devre dışı kılabilir |
| Başlangıç ayarlarını eşitleme |Kullanmayın; hiçbir etkisi yoktur |

## <a name="next-steps"></a>Sonraki adımlar

Genel bakış için [kurumsal Devlet Dolaşımıgenel görünümüne](enterprise-state-roaming-overview.md)bakın.
