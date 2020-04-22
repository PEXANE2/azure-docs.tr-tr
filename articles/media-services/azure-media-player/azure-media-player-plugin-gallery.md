---
title: Azure Media Player Eklentigalerisi
description: Bu makalede, Azure Media Player için kullanılabilir eklentilerin bir listesi yer almaktadır.
author: IngridAtMicrosoft
ms.author: inhenkel
ms.service: media-services
ms.topic: overview
ms.date: 04/20/2020
ms.openlocfilehash: 206200f3ba0757cd738439b58c8b94874cf5a938
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81726483"
---
# <a name="azure-media-player-plugin-gallery"></a>Azure Media Player Eklentigalerisi #

## <a name="plugins"></a>Eklentiler ##

| Eklenti Adı                         | Demo URL                    | Kaynak Kodu                | Açıklama    |
|-------------------------------------|-----------------------------|----------------------------|----------------|
| Ek Özellikler                 | | | |
| **Yeni!** AMP360Video                | [Tanıtım](http://www.babylonjs.com/demos/amp360video/)                        | [GitHub](https://github.com/BabylonJS/Extensions/tree/master/Amp360Video)                     | Eklenti, 360 videoyu masaüstünüzde veya VR uyumlu cihazlarda Amp'te görselleştirmenizi sağlar. Tam dokümantasyon [burada](https://doc\.babylonjs\.com/extensions/amp360video)mevcuttur: |
|  Sprite İpucu                         | [Tanıtım](http://www.skymedia.tv/asset/sprite)                        | [GitHub](https://github.com/RickShahid/SpriteTip)                    | Azure Media Services (AMS) Media Encoder Standard (MES) tarafından oluşturulan video küçük resim görüntüsünün zaman çizelgesi oluşturma sıcağı için Azure Media Player (AMP) eklentisi. |
| Tanılama Bindirme                 | [Tanıtım](https://openidconnectweb.azurewebsites.net/Diagnoverlay.html)                        | [GitHub](https://github.com/willzhan/diagnoverlay)                     | Bu eklenti görüntüler: Tüm anahtar parametreler, video istatistikleri, video oynatma yaşam döngüsündeki tüm olaylar ve korunduğunda anahtar kimliği, lisans edinme URL'leri gibi DRM koruma bilgileri.                                                                                                                                                                      |
| Çerçeve hızı ve Zaman Kodu hesaplayıcısı | [Tanıtım](http://mconvertitest001.blob.core.windows.net/public/example.html)                        | [GitHub](https://github.com/mconverti/media-services-javascript-azure-media-player-framerate-timecode-calculator-plugin)                     | Bu eklenti, ilk MPEG-DASH video `tfhd` / `trun` parçasının MP4 kutularını temel alan videonun kare hızını hesaplar, MPEG-DASH istemci bildiriminden zaman ölçeği değerini ayrıştırır ve aynı zamanda oynatıcıdan belirli bir mutlak süre için zaman kodu oluşturmanın bir yolunu sağlar (ayrıca oyuncuya zaman kodu verilen mutlak zamanı sağlar) |
| <strike>Oynatma Hızı</strike>                      | [Tanıtım](https://azure-samples.github.io/media-services-javascript-Azure-Media-Player-playback-rate-plugin/)                        | [GitHub](https://github.com/Azure-Samples/media-services-javascript-azure-media-player-time-tip-plugin)                     | Bu eklenti, görüntüleyenlerin videonun hangi hızda olduğunu denetlemesini sağlar. *Not, bu işlevsellik otomatik olarak SÜRÜM AMP v2.0.0+ kullanılabilir ancak varsayılan olarak devre dışı bırakılır.* Nasıl etkinleştirin, buradan [örneklere](https://github.com/Azure-Samples/azure-media-player-samples) göz atın |
| Hover Zaman İpucu                      | [Tanıtım](http://sr-test.azurewebsites.net/Tests/Plugin%20Gallery/plugins/timetip/example.html)                        | [GitHub](https://github.com/Azure-Samples/media-services-javascript-azure-media-player-time-tip-plugin)                     | Zaman doğru arama için fare gezinme üzerinde ilerleme çubuğu üzerinde bir zaman ucu görüntüler. *Not: Bu eklenti zaten AMP entegre edilmiştir,* ancak nasıl programlandığını görmek istiyorsanız bir göz atmakta çekinmeyin.                                                                                                                       |
| Başlık Bindirme                       | [Tanıtım](https://azure-samples.github.io/media-services-javascript-azure-media-player-title-overlay-plugin/")                        | [GitHub](https://github.com/Azure-Samples/media-services-javascript-azure-media-player-title-overlay-plugin)                     | Ekran üzerinde yapılandırılabilir video başlığı nı yerle bir eder |
| Zaman Çizelgesi İşaretleyicileri                    | [Tanıtım](http://sr-test.azurewebsites.net/Tests/Plugin%20Gallery/plugins/timelinemarkers/example.html)                        | [GitHub](https://github.com/Azure-Samples/media-services-javascript-azure-media-player-timeline-markers-plugin)                     | Bu eklenti bir dizi kez alır ve o zamanlarda ilerleme çubuğu üzerinde küçük işaretleri yeralır. |
| Analiz                           | | | |
| Application Insights                | [Blog Yazısı](https://azure.microsoft.com/blog/player-analytics-azure-media-player-plugin/)                   | [GitHub](https://github.com/Azure-Samples/media-services-javascript-azure-media-player-application-insights-plugin)                     | Görüntüleyenlerinizin oyuncu deneyiminin sezgisel grafik gösterimi için oyuncu ölçümlerinizi izleyen ve Power BI'ye bağlantı sağlayan eklenti. |
| Google Analytics                    | Yok                         | [GitHub](https://github.com/Azure-Samples/media-services-javascript-azure-media-player-google-analytics-plugin)                     | Azure Media Player için Google Analytics eklentisi |
| Tanılama                         | | | |
| Tanılama Çıktısı                  | [Tanıtım](http://sr-test.azurewebsites.net/Tests/Plugin%20Gallery/plugins/diagnosticslogger/example.html)                        | [GitHub](https://github.com/Azure-Samples/media-services-javascript-azure-media-player-diagnostic-logger-plugin)                     | Bu eklenti, oyuncunuzdan bir dizi tanılama çıkarır, bunu hareket halinde görmek için demo bağlantısına gidin ve JavaScript konsolunuzu açın. |
| Erişim Kolaylığı                      | | | |
| Yakınlaştırma                             | [Tanıtım](http://sr-test.azurewebsites.net/Tests/Plugin%20Gallery/plugins/zoom/example.html)                        | [GitHub](https://github.com/Azure-Samples/media-services-javascript-azure-media-player-zoom-plugin)                     | Bu eklenti, görüntüleyenlerin içeriğinizi yakınlaştırabilmesi için oynatılabilen ekranda sürülebilen bir yakınlaştırma ölçeği görüntüler |
| Canlı Altyazılar                       | [Azure Blog Gönderisi](https://azure.microsoft.com/blog/live-real-time-captions-with-azure-media-services-and-player/),[SubPly Post](http://www.subply.com/en/Products/AzureLiveCaptions.htm) | Yok | *Daha fazla bilgi için gönderiye bakın.* Azure Media Player için yerleşik eklenti için tasarlanmış sondan uca iş akışı, SubPly'nin sitesine gitmek ve çözüm hakkında daha fazla bilgi edinmek için soldaki en lin'e tıklayın |
| Sıcak Tuşlar                            | <strike>[Tanıtım](http://sr-test.azurewebsites.net/Tests/Plugin%20Gallery/plugins/hotkeys/example.html)</strike>                        | <strike>[GitHub](https://github.com/Azure-Samples/media-services-javascript-azure-media-player-hot-keys-plugin)</strike>                     | Sıcak tuşlar eklentisi, izleyicilerin tam ekran için F, sessiz için M ve ilerleme çubuğu kontrolü için ok tuşları gibi genel eklenti kombinasyonları ile oyuncunun çeşitli yönlerini kontrol etmesini sağlar. *Not: Bu eklenti zaten AMP entegre edilmiş ancak bir kaynak olarak kullanmak için çekinmeyin* |
| Sosyal                              | | | |
| Paylaş                               | [Tanıtım](http://sr-test.azurewebsites.net/Tests/Plugin%20Gallery/plugins/share/example.html)                        | [GitHub](https://github.com/Azure-Samples/media-services-javascript-azure-media-player-social-share-plugin)                     | Bu eklenti, görüntüleyenlerinizin izledikleri videoyu Facebook, Twitter veya Linkedin üzerinden arkadaşlarıyla paylaşabilmeleri için oyuncunun kontrol çubuğuna bir paylaşım düğmesi ekler. |

## <a name="next-steps"></a>Sonraki adımlar ##

- [Azure Media Player Quickstart](azure-media-player-quickstart.md)