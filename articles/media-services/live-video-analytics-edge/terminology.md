---
title: IoT EDG terimlerinde canlı video analizi-Azure
description: Bu makalede, IoT Edge terminolojisine ilişkin canlı video analizlerine genel bakış sunulmaktadır.
ms.topic: conceptual
ms.date: 05/30/2020
ms.openlocfilehash: 5d4eff506b2a6f51b9803f827379b9ba0c2b2ff6
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87011507"
---
# <a name="terminology"></a>Terminoloji

Bu makalede, [IoT Edge üzerindeki canlı video analizi](overview.md)ile ilgili terimlere genel bakış sunulmaktadır.

## <a name="azure-media-services"></a>Azure Media Services

Azure Media Services, medya çözümleri oluşturmanıza olanak sağlayan bir bulut medya platformudur. [Azure Media Services](../latest/media-services-overview.md) belgelerinde hakkında daha fazla bilgi edinebilirsiniz.

## <a name="asset"></a>Varlık

[Varlık](../latest/assets-concept.md) , bir Media Services hesabına eklenen Azure depolama hesabındaki bir blob kapsayıcısına eşlenen Azure Media Services bir varlıktır. Bir varlıkla ilişkili tüm dosyalar, varlık ile ilişkili meta verileri (örneğin, bir ad, açıklama, oluşturma saati) taşıdığı Media Services sürece bu kapsayıcıda blob olarak depolanır.

IoT Edge üzerindeki canlı video analizi, varlık oluşturabilir ve/veya mevcut varlıklara veri ekleyebilir. Bu, sürekli ve olay tabanlı video kaydı ve kayıttan yürütme senaryolarına (Edge cihazında video yakalama, Azure Media Services kaydetme ve mevcut Azure Media Services akış özellikleri aracılığıyla kayıttan yürütme ile) olanak sağlar.

## <a name="streaming"></a>Akış

Netflix, YouTube ve diğer hizmetler gibi hizmetlerden bir mobil cihazda video izleniyor varsa, akış videosu ile karşılaşmış olursunuz. Kayıttan yürütme, "Oynat" düğmesine (yeterli bant genişliğine sahipseniz) kadar ilerledikten sonra videonun zaman çizelgesinde geri ve ileriye doğru arama yapabilirsiniz. Akışla, fikir yalnızca izlenmekte olan videonun yalnızca bir kısmını sunmanız ve verilerin bir sunucudan kayıttan yürütme istemcisine aktarılmakta olması halinde görüntüleyicisinin videoyu yürütmeye başlamasını sağlar. Azure Media Services bağlamında, [akış](https://en.wikipedia.org/wiki/Streaming_media) [Azure Media Services](../azure-media-player/azure-media-player-overview.md) bir akış istemcisine medya sunma işlemini (örneğin, Azure Media Player) ifade eder. Azure Media Services kullanarak, [http canlı akışı (HLS)](https://developer.apple.com/streaming/) ve [MPEG-Dash](https://dashif.org/about/)gıbı sektör standardı, HTTP tabanlı medya akışı protokollerini kullanarak müşterilere video akışını sağlayabilirsiniz. HLS, Azure Media Player ve [JW oynatıcı](https://www.jwplayer.com/), [hls.js](https://github.com/video-dev/hls.js/), [Videojs](https://videojs.com/), [Google 'ın Shaka Player](https://github.com/google/shaka-player)gibi Web oyuncularını ve Android 'in [exoplayer](https://github.com/google/ExoPlayer) ve iOS 'un [av Foundation](https://developer.apple.com/av-foundation/)ile mobil uygulamalarda yerel olarak işleyebilmeniz için desteklenir. MPEG-DASH aynı şekilde Azure Media Player tarafından desteklenir, [Bu sayfadaki istemcilerin bir listesini bulur](https://dashif.org/clients/). 

Videoları Azure Media Services bir varlığa kaydetmek için [medya graflarını](#media-graph)kullanarak, HLS ve Dash 'de video akışları teslim etmek üzere Media Services akış özelliğinden yararlanabilirsiniz. [Video kayıttan yürütme](video-playback-concept.md) makalesinde bunun hakkında daha fazla bilgi edinebilirsiniz.

## <a name="recording"></a>Kayıt

Güvenlik kameraları için bir video yönetim sistemi bağlamında, video kaydı, kameralardan video yakalama ve mobil ve tarayıcı uygulamaları aracılığıyla sonraki görüntülemeye yönelik bir dosyaya (veya dosyalara) depolama sürecini ifade eder. Video kaydı, [sürekli video kaydı](continuous-video-recording-concept.md) ve [olay tabanlı video kaydı](event-based-video-recording-concept.md)halinde kategorilere ayrılabilir. Bunlar, [video kaydetme](video-recording-concept.md) kavramı sayfasında daha ayrıntılı olarak açıklanmıştır.

## <a name="media-graph"></a>Medya grafiği

[Medya grafiği](media-graph-concept.md) , medyanın nerede yakalanabileceğini, nasıl işleneceğini ve sonuçların nereye teslim edileceğini tanımlamanızı sağlar. Kaynakları, işlemcileri ve havuz düğümlerini içeren bir grafik tanımlamanızı sağlar ve bu sayede canlı video analizi uygulamaları oluşturmanıza olanak sağlar. Medya grafiği, medya grafiği kavram sayfasında ayrıntılı olarak ele alınmıştır.

## <a name="rtsp"></a>RTSP

[RTSP](https://tools.ietf.org/html/rfc2326) gerçek zamanlı akış protokolüne başvurur. Gerçek zamanlı özelliklerle veri tesliminde denetim için uygulama düzeyi bir protokoldür. RTSP, ses ve video gibi gerçek zamanlı verilerin denetimli, isteğe bağlı teslimatını etkinleştirmek için genişletilebilir bir çerçeve sağlar. 

## <a name="vms"></a>VM 'ler

[VM 'ler](https://en.wikipedia.org/wiki/Video_management_system) bir video yönetim sistemine başvurur. Bu tür sistemler CCTV kameralarını yapılandırmak ve denetlemek, Videoları yakalamak ve bunlara kaydetmek için kullanılır. Bu sistemler Ayrıca, istemci uygulamalarının kayıtlı videoyu kayıttan yürütmesine olanak sağlar

## <a name="next-steps"></a>Sonraki adımlar

[Medya grafikleri](media-graph-concept.md)
