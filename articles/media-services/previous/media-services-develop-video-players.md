---
title: Video oynatıcı uygulamaları geliştirme
description: Bu konu, Media Services 'den akış medyası kullanabilen kendi istemci uygulamalarınızı geliştirmek için kullanabileceğiniz oynatıcı çerçeveleri ve eklentileri için bağlantılar sağlar.
author: Juliako
manager: femila
editor: ''
services: media-services
documentationcenter: ''
ms.assetid: 55e419fc-4c39-4902-9c62-f41cfcd86c6c
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/18/2019
ms.author: juliako
ms.openlocfilehash: 39459267919747ed49e9fa3f05746294eaf741dc
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2020
ms.locfileid: "76906833"
---
# <a name="develop-video-player-applications"></a>Video oynatıcı uygulamaları geliştirme
## <a name="overview"></a>Genel Bakış
Azure Media Services, şunlar dahil olmak üzere çoğu platform için zengin ve dinamik istemci oynatıcı uygulamaları oluştururken ihtiyacınız olan araçları sağlar: iOS Cihazları, Android Cihazları, Windows, Windows Phone, Xbox ve Alıcı kutuları. Bu konu ayrıca, Azure Media Services 'den akış medyası kullanabilen kendi istemci uygulamalarınızı geliştirmek için kullanabileceğiniz SDK 'lar ve oynatıcı çerçevelerine bağlantılar sağlar.

>[!NOTE]
>AMS hesabınız oluşturulduğunda hesabınıza **Durdurulmuş** durumda bir **varsayılan** akış uç noktası eklenir. İçerik akışını başlatmak ve dinamik paketleme ile dinamik şifrelemeden yararlanmak için içerik akışı yapmak istediğiniz akış uç noktasının **Çalışıyor** durumda olması gerekir. 
 
## <a name="azure-media-player"></a>Azure Media Player
[Azure Media Player](https://aka.ms/ampinfo) , medya içeriğini çok çeşitli tarayıcılarda ve cihazlarda Microsoft Azure Media Services çalmak için oluşturulmuş bir web video oyuncusu. Azure Media Player HTML5, medya kaynağı uzantıları (MSE) ve şifreli Medya Uzantıları (EME) gibi sektör standartlarını, zenginleştirilmiş bir uyarlamalı akış deneyimi sağlamak için kullanır. Bu standartlar bir cihazda veya bir tarayıcıda kullanılabilir olmadığında, Azure Media Player Flash ve Silverlight 'ı geri dönüş teknolojisi olarak kullanır. Kullanılan kayıttan yürütme teknolojisinden bağımsız olarak, geliştiricilerin API 'Lere erişmek için birleştirilmiş bir JavaScript arabirimi olacaktır. Bu, Azure Media Services tarafından sunulan içeriğin ek bir çaba olmadan çok sayıda cihaz ve tarayıcıda yürütülebilmesini sağlar.

Microsoft Azure Media Services, içeriğin kayıttan yürütülmesi için DASH, Kesintisiz Akış ve HLS akış biçimleriyle birlikte sunulmasını sağlar. Azure Media Player, bu çeşitli biçimleri dikkate alır ve platform/tarayıcı özelliklerine göre en iyi bağlantıyı otomatik olarak yürütür. Microsoft Azure Media Services Ayrıca, PlayReady şifrelemesi veya AES-128 bit zarfı Şifrelemeli varlıkların dinamik şifrelemesine olanak tanır. Azure Media Player, uygun şekilde yapılandırıldığında PlayReady ve AES-128 bit şifreli içeriğin şifresini çözmeye izin verir. 

Daha fazla bilgi için:

* [Azure Media Player](https://aka.ms/ampinfo)
* [Azure Media Player belgeleri](https://aka.ms/ampdocs) 
* [Azure Media Player kullanmaya başlama blogu](https://azure.microsoft.com/blog/2015/04/15/announcing-azure-media-player/)
* [En son Azure Media Player güncel kalmak için kaydolun](https://aka.ms/ampsignup)
* [Yeni özellik istekleri, fikirler ve geri bildirim ekleyin](https://aka.ms/ampuservoice) 

## <a name="other-tools-for-creating-player-applications"></a>Oynatıcı uygulamaları oluşturmaya yönelik diğer araçlar
Aşağıdaki SDK 'Lardan herhangi birini de kullanabilirsiniz:

* [Kesintisiz Akış Istemci SDK 'Sı](https://www.iis.net/downloads/microsoft/smooth-streaming) 
* [Windows Mağazası uygulamasını Kesintisiz Akış](media-services-build-smooth-streaming-apps.md)
* [Microsoft medya platformu: oynatıcı çatısı](https://playerframework.codeplex.com/) 
* [HTML5 Player Çerçevesi belgeleri](https://playerframework.codeplex.com/wikipage?title=HTML5%20Player&referringTitle=Documentation) 
* [OSMF için Microsoft Kesintisiz Akış eklentisi](https://www.microsoft.com/download/details.aspx?id=36057) 
* [Microsoft® Kesintisiz Akış Istemci taşıma setini lisanslama](https://aka.ms/sspk) 
* [XBOX video uygulaması geliştirme](https://www.xbox.com/en-US/developers) 

## <a name="advertising"></a>Reklam
Azure Media Services, Windows Media platformu: oynatıcı çerçeveleri aracılığıyla ad ekleme desteği sağlar. Windows 8, Silverlight, Windows Phone 8 ve iOS cihazlarda ad desteğine sahip oynatıcı çerçeveleri mevcuttur. Her Player çerçevesi, oynatıcı uygulamasının nasıl uygulanacağını gösteren örnek kod içerir. Medyanıza ekleyebileceğiniz üç farklı tür reklamları vardır:

Doğrusal – ana videoyu duraklatarak tam çerçeve reklamları

Doğrusal olmayan – ana video olarak görüntülenen kaplama reklamları, genellikle bir logo veya Player içinde yer alan başka bir statik görüntü yürütüyor.

Yardımcı – oyuncu dışında görüntülenen reklamlar

Reklamlar, ana videonun zaman satırındaki herhangi bir noktaya yerleştirilebilir. Player 'ın ad 'yi oynatacak ve hangi reklamları oynatacak hakkında söylemeniz gerekir. Bu, standart bir XML tabanlı dosyalar kümesi kullanılarak yapılır: video ad hizmeti şablonu (büyük), dijital video birden çok ad çalma listesi (VMAP), medya soyut sıralama şablonu (MAST) ve dijital video oynatıcı ad arabirimi tanımı (VÜCRETLI). Büyük dosyalar hangi reklamların gösterileceğini belirtir. VMAP dosyaları, çeşitli reklamları ne zaman oynatacak ve çok sayıda XML içermektir. MAST dosyaları, büyük bir XML de içerebilen reklamları sıralamadaki başka bir yoldur. VÜCRETLI dosyalar, video oynatıcı ile ad veya ad sunucusu arasında bir arabirim tanımlar. Daha fazla bilgi için bkz. [reklamları ekleme](https://msdn.microsoft.com/library/dn387398.aspx).

Canlı akış videolarında kapalı açıklamalı altyazı ve ads desteği hakkında daha fazla bilgi için bkz. [desteklenen kapalı açıklamalı altyazı ve ad ekleme standartları](https://msdn.microsoft.com/library/c49e0b4d-357e-4cca-95e5-2288924d1ff3#caption_ad).

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirim sağlayın
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Ayrıca Bkz.
[DASH.js ile bir MPEG-DASH Uyarlamalı Akış Videosunu bir HTML5 Uygulamasına ekleme](media-services-embed-mpeg-dash-in-html5.md)

[GitHub Dash. js deposu](https://github.com/Dash-Industry-Forum/dash.js)

