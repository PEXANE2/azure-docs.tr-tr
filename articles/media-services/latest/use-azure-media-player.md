---
title: Azure Media Player ile Oynatma - Azure | Microsoft Dokümanlar
description: Azure Media Player, Microsoft Azure Media Services'in medya içeriğini çok çeşitli tarayıcılarda ve cihazlarda oynatmak için oluşturulmuş bir web video oynatıcıdır.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 07/17/2019
ms.author: juliako
ms.openlocfilehash: d9e42e809443a2dd6cdeb989f692b96d63269f79
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78673570"
---
# <a name="azure-media-player-overview"></a>Azure Media Player'a genel bakış

Azure Media Player, Microsoft Azure Media Services'in medya içeriğini çok çeşitli tarayıcılarda ve cihazlarda oynatmak için oluşturulmuş bir web video oynatıcıdır. Azure Media Player, zenginleştirilmiş bir uyarlamalı akış deneyimi sağlamak için HTML5, Media Source Extensions (MSE) ve Şifreli Medya Uzantıları (EME) gibi endüstri standartlarını kullanır. Bu standartlar bir aygıtta veya tarayıcıda kullanılamadığında, Azure Media Player geri dönüş teknolojisi olarak Flash ve Silverlight'ı kullanır. Kullanılan oynatma teknolojisine bakılmaksızın, geliştiriciler API'lere erişmek için birleşik bir JavaScript arabirimine sahip olacaktır. Bu, Azure Medya Hizmetleri tarafından sunulan içeriğin ekstra bir çaba sarf edilmeden çok çeşitli aygıtlarda ve tarayıcılarda oynatılmasına olanak tanır.

Microsoft Azure Media Services, içeriği oynatmak için içeriğihls, DASH, Düzgün Akış akış biçimleriyle birlikte sunulmasını sağlar. Azure Media Player bu çeşitli biçimleri dikkate alır ve platform/tarayıcı özelliklerine göre otomatik olarak en iyi bağlantıyı çalar. Medya Hizmetleri ayrıca PlayReady şifreleme veya AES-128 bit zarf şifreleme ile varlıkların dinamik şifreleme sağlar. Azure Media Player, uygun şekilde yapılandırıldığında PlayReady ve AES-128 bit şifreli içeriğin şifresini çözmenize olanak tanır. 

> [!NOTE]
> Widevine şifreli içerik için HTTPS oynatma gereklidir.

## <a name="use-azure-media-player-demo-page"></a>Azure Media Player demo sayfasını kullanma

### <a name="start-using"></a>Kullanmaya başlayın

Azure Media [Player demo sayfasını](https://aka.ms/azuremediaplayer) Azure Medya Hizmetleri örneklerini veya kendi akışınızı çalmak için kullanabilirsiniz.  

Yeni bir video oynatmak için farklı bir URL yapıştırın ve **Güncelleştir'e**basın.

Çeşitli oynatma seçeneklerini (örneğin, teknoloji, dil veya şifreleme) yapılandırmak için **Gelişmiş Seçenekler'e**basın.

![Azure Media Player](./media/azure-media-player/home-page.png)

### <a name="monitor-diagnostics-of-a-video-stream"></a>Video akışının tanılamalarını izleme

Bir video akışının tanılamalarını izlemek için [Azure Media Player demo sayfasını](https://aka.ms/azuremediaplayer) kullanabilirsiniz. 

![Azure Media Player tanılama](./media/azure-media-player/diagnostics.png)

## <a name="set-up-azure-media-player-in-your-html"></a>HTML'nizde Azure Media Player'ı ayarlama

Azure Media Player'ın kurulumu kolaydır. Medya Hizmetleri hesabınızdan medya içeriğinin temel olarak oynatmasını almak yalnızca birkaç dakika nızı alır. Azure Media Player'ı nasıl ayarlayıp yapılandırılabildiğini öğrenmek için [Azure Media Player belgelerine](https://aka.ms/ampdocs) bakın. 

## <a name="additional-notes"></a>Ek notlar

* Widevine, Google Inc. tarafından sağlanan ve Google, Inc.'in hizmet koşullarına ve Gizlilik Politikasına tabi olan bir hizmettir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Media Player belgeleri](https://aka.ms/ampdocs)
- [Azure Media Player örnekleri](https://aka.ms/ampsamples)
