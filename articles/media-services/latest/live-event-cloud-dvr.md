---
title: İsteğe bağlı video oynatma oluşturmak için zaman kaydırma ve Canlı Çıktılar kullanma
titleSuffix: Azure Media Services
description: Bu makalede, Canlı Akışları kaydetmek ve isteğe bağlı oynatma oluşturmak için zaman kaydırma ve Canlı Çıktılar'ın nasıl kullanılacağı açıklanmaktadır.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 08/27/2019
ms.author: juliako
ms.openlocfilehash: 4c7618b60e5fd86a9b8b3f22fb3333c00cfdfa61
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74899790"
---
# <a name="use-time-shifting-and-live-outputs-to-create-on-demand-video-playback"></a>İsteğe bağlı video oynatma oluşturmak için zaman kaydırma ve Canlı Çıktılar kullanma

Azure Medya Hizmetleri'nde, [Canlı Çıktı](https://docs.microsoft.com/rest/api/media/liveoutputs) nesnesi, canlı akışınızı Medya Hizmetleri hesabınızdaki bir varlığa yakalayıp kaydedecek dijital video kaydedici gibidir. Kaydedilen [içerik, Varlık](https://docs.microsoft.com/rest/api/media/assets) kaynağı tarafından tanımlanan kapsayıcıda kalıcıdır (kapsayıcı hesabınıza bağlı Azure Depolama hesabındadır). Canlı Çıkış ayrıca, akışın ne kadarının arşiv kaydında tutulduğu (örneğin, bulut DVR'ın kapasitesi) veya görüntüleyenlerin canlı akışı izlemeye ne zaman başlabildiği gibi giden canlı akışın bazı özelliklerini kontrol etmenizi sağlar. Diskteki arşiv, yalnızca Live **Output'un archiveWindowLength** özelliğinde belirtilen içerik miktarını tutan dairesel bir arşiv "penceresi"dir. Bu pencerenin dışına düşen içerik otomatik olarak depolama kabından atılır ve kurtarılamaz. ArchiveWindowLength değeri, DVR'ın kapasitesini belirten bir ISO-8601 zaman dilimi süresini (örneğin, PTHH:MM:SS) temsil eder. Değer en az üç dakikadan en fazla 25 saate ayarlanabilir.

Bir Live Event ve Live Outputs arasındaki ilişki geleneksel TV yayınına benzer, çünkü bir kanal (Live Event) sabit bir video akışını temsil eder ve bir kayıt (Canlı Çıkış) belirli bir zaman segmentine (örneğin, akşam haberleri 18:30 ile 19:00 arası). Akışı Live Event'e akttİlke çıkardıktan sonra, bir varlık, Canlı Çıktı ve akış bulucu oluşturarak akış olayına başlayabilirsiniz. Canlı Çıktı akışı arşivleyecek ve [Akış Bitiş Noktası](https://docs.microsoft.com/rest/api/media/streamingendpoints)aracılığıyla görüntüleyenlerin kullanımına sunacak. Farklı arşiv uzunlukları ve ayarlarıyla bir Live Event'de birden çok Live Output (en fazla üç) oluşturabilirsiniz. Canlı akış iş akışı hakkında bilgi için [genel adımlar](live-streaming-overview.md#general-steps) bölümüne bakın.

## <a name="using-a-dvr-during-an-event"></a>Etkinlik sırasında DVR kullanma

Bu bölümde, akışın hangi bölümlerinin 'geri sarma' için kullanılabilen bir olay sırasında Nasıl DVR kullanılacağı açıklanır.

Değer, `archiveWindowLength` bir görüntüleyenin geçerli canlı konumdan ne kadar geriye gidebileceğini belirler. Değer `archiveWindowLength` ayrıca istemci bildirimlerinin ne kadar büyüyebileceğini de belirler.

Bir futbol oyunu yayın yaptığınızı ve `ArchiveWindowLength` sadece 30 dakikası olduğunu varsayalım. Oyun başladıktan 45 dakika sonra etkinliğinizi izlemeye başlayan bir izleyici en fazla 15 dakikalık hedefe geri dönebilir. Oyun için Canlı Çıktılarınız, Canlı Etkinlik durdurulana kadar devam eder. ArchiveWindowLength'un dışına düşen içerik sürekli olarak depodan atılır ve kurtarılamaz. Bu örnekte, olayın başlangıcı ile 15 dakikalık işaret arasındaki video, DVR'ınızdan ve kıymet için blob depolamasındaki kapsayıcıdan temizlenmiş olacaktır. Arşiv kurtarılamaz ve Azure blob depolama sındaki kapsayıcıdan kaldırılır.

Canlı Etkinlik, aynı anda çalışan en fazla üç Live Output'u destekler (aynı anda bir canlı akıştan en fazla 3 kayıt/arşiv oluşturabilirsiniz). Bu destek, bir olayın farklı bölümlerini gerektiği gibi yayımlamanızı ve arşivlemenizi sağlar. 24x7 canlı doğrusal bir yayın yayınlamanız ve müşterilere izleme için isteğe bağlı içerik olarak sunmak için gün boyunca farklı programların "kayıtlarını" oluşturmanız gerektiğini varsayalım. Bu senaryo için, ilk olarak 1 saat veya daha kısa bir arşiv penceresi olan birincil Canlı Çıktı oluşturursunuz– bu, görüntüleyenlerinizin ayaryapacağı birincil canlı akıştır. Bu Canlı Çıktı için bir Akış Bulucu oluşturur ve "Canlı" akışı olarak uygulamanızda veya web sitenizde yayımlarsınız. Canlı Etkinlik çalışırken, program başlangıcında programlanabilir şekilde ikinci bir eşzamanlı Canlı Çıktı oluşturabilirsiniz (veya daha sonra kırpmak için bazı tutamaçları sağlamak için 5 dakika erken). Bu ikinci Canlı Çıktı, program sona erdikten 5 dakika sonra silinebilir. Bu ikinci varlıkla, bu programı uygulamanızın kataloğunda isteğe bağlı bir varlık olarak yayımlamak için yeni bir Akış Bulucu oluşturabilirsiniz. Bu işlemi, ilk Canlı Çıktı'dan gelen "Canlı" akışı doğrusal akışı yayınlamaya devam ederken, isteğe bağlı videolar olarak paylaşmak istediğiniz diğer program sınırları veya vurgular için birden çok kez yineleyebilirsiniz.

## <a name="creating-an-archive-for-on-demand-playback"></a>İsteğe bağlı oynatma için arşiv oluşturma

Canlı Çıktı silindiğinde, Canlı Çıktının otomatik olarak isteğe bağlı bir varlık haline gelmesi için arşivlediği varlık. Canlı Etkinlik durdurulmadan önce tüm Canlı Çıktıları silmeniz gerekir. İsteğe bağlı bir bayrak [removeOutputsOnStop](https://docs.microsoft.com/rest/api/media/liveevents/stop#request-body) kullanarak Canlı Çıktıları durdurun.

Etkinliği durdurup sildikten sonra bile, kullanıcılar arşivlenmiş içeriğinizi, varlığı silmediğiniz sürece isteğe bağlı olarak video olarak aktarabilirler. Bir varlık bir olay tarafından kullanılıyorsa silinmemelidir; olay önce silinmelidir.

Canlı Çıktınızın kıymetini bir akış bulucusu kullanarak yayımladıysanız, Akış Etkinliği (DVR pencere uzunluğuna kadar) akış bulucusu sona erene veya silinene (hangisi önce gelirse) kadar görüntülenebilir olmaya devam eder.

Daha fazla bilgi için bkz.

- [Canlı akışa genel bakış](live-streaming-overview.md)
- [Canlı akış eğitimi](stream-live-tutorial-with-api.md)

> [!NOTE]
> Canlı Çıktıyı sildiğinizde, varlıktaki dayanak varlığı ve içeriği silmezsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Videolarınızı alt klip.](subclip-video-rest-howto.md)
* [Varlıklarınız için filtreler tanımlayın.](filters-dynamic-manifest-rest-howto.md)
