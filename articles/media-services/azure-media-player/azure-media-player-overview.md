---
title: Azure Media Player'a Genel Bakış
description: Azure Media Player hakkında daha fazla bilgi edinin.
author: IngridAtMicrosoft
ms.author: inhenkel
ms.service: media-services
ms.topic: overview
ms.date: 04/20/2020
ms.openlocfilehash: b6d30aebd4de272ba98fce87f23701b129eacb02
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81726525"
---
# <a name="azure-media-player-overview"></a>Azure Media Player'a genel bakış #

Azure Media Player, [Microsoft Azure Media Services'in](https://azure.microsoft.com/services/media-services/) medya içeriğini çok çeşitli tarayıcılarda ve cihazlarda oynatmak için oluşturulmuş bir web video oynatıcıdır. Azure Media Player, zenginleştirilmiş bir uyarlamalı akış deneyimi sağlamak için HTML5, Media Source Extensions (MSE) ve Şifreli Medya Uzantıları (EME) gibi endüstri standartlarını kullanır.  Bu standartlar bir aygıtta veya tarayıcıda kullanılamadığında, Azure Media Player geri dönüş teknolojisi olarak Flash ve Silverlight'ı kullanır. Kullanılan oynatma teknolojisine bakılmaksızın, geliştiriciler API'lere erişmek için birleşik bir JavaScript arabirimine sahip olacaktır.  Bu, Azure Medya Hizmetleri tarafından sunulan içeriğin ekstra bir çaba sarf edilmeden çok çeşitli aygıtlarda ve tarayıcılarda oynatılmasına olanak tanır.

Microsoft Azure Media Services, içeriği oynatmak için DASH, Smooth Streaming ve HLS akış biçimleriyle içeriğin sunulmasını sağlar. Azure Media Player bu çeşitli biçimleri dikkate alır ve platform/tarayıcı özelliklerine göre otomatik olarak en iyi bağlantıyı çalar. Microsoft Azure Medya Hizmetleri, ortak şifreleme (PlayReady veya Widevine) veya AES-128 bit zarf şifrelemesi olan varlıkların dinamik şifrelemesine de izin verir. Azure Media Player, uygun şekilde yapılandırıldığında PlayReady ve AES-128 bit şifreli içeriğin şifresini çözmenize olanak tanır.  Bunu yapılandırmak için [Korumalı İçerik](azure-media-player-protected-content.md) bölümüne bakın.

Yeni özellikler istemek, fikir veya geri bildirim sağlamak için lütfen [Azure Media Player için UserVoice'a](https://aka.ms/ampuservoice)gönderin. Eğer ve belirli sorunlar, sorular veya herhangi bir hata ampinfo@microsoft.combulmak varsa, bize bir satır bırakın .

Bir sürümü asla kaçırmamak ve Azure Media Player'ın sunduğu en son sürümlerle güncel kalmak için [kaydolun.](https://aka.ms/ampsignup)

> [!NOTE]
> Azure Media Player'ın yalnızca Azure Medya Hizmetleri'nden medya akışlarını desteklediğini lütfen unutmayın.

## <a name="license"></a>Lisans ##

Azure Media Player lisanslıdır ve Azure Media Player için Microsoft Yazılım Lisans Koşulları'nda belirtilen koşullara tabidir. Tam koşullar için [lisans dosyasına](azure-media-player-license.md) bakın. Daha fazla bilgi için [Gizlilik Bildirimi'ne](https://www.microsoft.com/en-us/privacystatement/default.aspx) bakın.

Telif Hakkı 2015 Microsoft Corporation.

## <a name="next-steps"></a>Sonraki adımlar ##

- [Azure Media Player Quickstart](azure-media-player-quickstart.md)