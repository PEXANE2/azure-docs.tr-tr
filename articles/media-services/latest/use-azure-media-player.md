---
title: Azure Media Player-Azure ile kayıttan yürütme | Microsoft Docs
description: Azure Media Player, medya içeriğini çok çeşitli tarayıcılarda ve cihazlarda Microsoft Azure Media Services çalmak için oluşturulmuş bir web video oyuncusu.
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
ms.openlocfilehash: bfc22862bc40679644d7dc9eddedf45fbacccab1
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87292896"
---
# <a name="playback-with-azure-media-player"></a>Azure Media Player ile kayıttan yürütme

Azure Media Player, medya içeriğini çok çeşitli tarayıcılarda ve cihazlarda Microsoft Azure Media Services çalmak için oluşturulmuş bir web video oyuncusu. Azure Media Player HTML5, medya kaynağı uzantıları (MSE) ve şifreli Medya Uzantıları (EME) gibi sektör standartlarını, zenginleştirilmiş bir uyarlamalı akış deneyimi sağlamak için kullanır. Bu standartlar bir cihazda veya bir tarayıcıda kullanılabilir olmadığında, Azure Media Player Flash ve Silverlight 'ı geri dönüş teknolojisi olarak kullanır. Kullanılan kayıttan yürütme teknolojisinden bağımsız olarak, geliştiricilerin API 'Lere erişmek için birleştirilmiş bir JavaScript arabirimi olacaktır. Bu, Azure Media Services tarafından sunulan içeriğin ek bir çaba olmadan çok sayıda cihaz ve tarayıcıda yürütülebilmesini sağlar.

Microsoft Azure Media Services, içeriğin kayıttan yürütülmesi için HLS, DASH, Kesintisiz Akış akış biçimleriyle birlikte sunulmasını sağlar. Azure Media Player, bu çeşitli biçimleri dikkate alır ve platform/tarayıcı özelliklerine göre en iyi bağlantıyı otomatik olarak yürütür. Media Services Ayrıca, PlayReady şifrelemesi veya AES-128 bit zarfı Şifrelemeli varlıkların dinamik şifrelemesine olanak tanır. Azure Media Player, uygun şekilde yapılandırıldığında PlayReady ve AES-128 bit şifreli içeriğin şifresini çözmeye izin verir.

> [!NOTE]
> Wdevine şifrelenmiş içerik için HTTPS kayıttan yürütme gereklidir.

## <a name="use-azure-media-player-demo-page"></a>Azure Media Player demo sayfası kullanma

### <a name="start-using"></a>Kullanmaya başlayın

[Azure Media Player Demo sayfasını](https://aka.ms/azuremediaplayer) kullanarak Azure Media Services örnekleri veya kendi akışınızı oynatabilirsiniz.  

Yeni bir video oynatmak için, farklı bir URL yapıştırın ve **Güncelleştir**' e basın.

Çeşitli kayıttan yürütme seçeneklerini yapılandırmak için (örneğin, teknik, dil veya şifreleme), **Gelişmiş Seçenekler**' e basın.

![Azure Media Player](./media/azure-media-player/home-page.png)

### <a name="monitor-diagnostics-of-a-video-stream"></a>Video akışının tanılamalarını izleme

Video akışının tanılamalarını izlemek için [Azure Media Player Demo sayfasını](https://aka.ms/azuremediaplayer) kullanabilirsiniz.

![Azure Media Player tanılama](./media/azure-media-player/diagnostics.png)

## <a name="set-up-azure-media-player-in-your-html"></a>HTML 'inizdeki Azure Media Player ayarlama

Azure Media Player kolayca ayarlanabilir. Media Services hesabınızdan medya içeriğinin temel oynatımını almak yalnızca birkaç dakika sürer. Azure Media Player ayarlama ve yapılandırma hakkında ayrıntılı bilgi için bkz. [Azure Media Player belgeleri](https://docs.microsoft.com/azure/media-services/azure-media-player/azure-media-player-overview) .

## <a name="additional-notes"></a>Ek notlar

* Widevine, Google Inc. tarafından sunulan bir hizmettir ve Google, Inc 'nin hizmet koşullarına ve gizlilik Ilkesine tabidir.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Media Player belgeleri](https://docs.microsoft.com/azure/media-services/azure-media-player/azure-media-player-overview)
* [Azure Media Player örnekleri](https://github.com/Azure-Samples/azure-media-player-samples)
