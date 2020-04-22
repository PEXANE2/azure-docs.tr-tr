---
title: Azure Media Player özellik listesi
description: Azure Media Player için bir özellik başvurusu.
author: IngridAtMicrosoft
ms.author: inhenkel
ms.service: media-services
ms.topic: reference
ms.date: 04/20/2020
ms.openlocfilehash: e5595620a2f888b06ad5b35d2e8a008f23861463
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81727235"
---
# <a name="feature-list"></a>Özellik listesi #
Test edilmiş özelliklerin ve desteklenmeyen özelliklerin listesi aşağıda verilmiştir:

|                                         | Test | KıSMEN TEST EDİlMİş | Denenme | Desteklenme -yen | NOTLAR                                                                                                                |
|:----------------------------------------|--------|------------------|----------|-------------|:---------------------------------------------------------------------------------------------------------------------|
| Kayıttan yürütme                                |        |                  |          |             |                                                                                                                      |
| Temel İsteğe Bağlı Oynatma                | X      |                  |          |             | Yalnızca Azure Medya Hizmetleri'nden gelen akışları destekler                                                                      |
| Temel Canlı Oynatma                     | X      |                  |          |             | Yalnızca Azure Medya Hizmetleri'nden gelen akışları destekler                                                                      |
| AES                                     | X      |                  |          |             | Azure Medya Hizmetleri Anahtar Teslim Hizmetini Destekler                                                                   |
| Çoklu DRM                               |        | X                |          |             |                                                                                                                      |
| PlayReady                               | X      |                  |          |             | Azure Medya Hizmetleri Anahtar Teslim Hizmetini Destekler                                                                   |
| Widevine                                |        | X                |          |             | Manifest'te özetlenen Widevine PSSH kutularını destekler                                                                    |
| FairPlay                                |        | X                |          |             | Azure Medya Hizmetleri Anahtar Teslim Hizmetini Destekler                                                                   |
| Teknisyen                                   |        |                  |          |             |                                                                                                                      |
| MSE/EME (AzureHtml5JS)                  | X      |                  |          |             |                                                                                                                      |
| Flash Backback (FlashSS)                | X      |                  |          |             | Tüm özellikler bu teknolojide mevcut değildir.                                                                         |
| Silverlight Geri Dönüş SilverlightSS      | X      |                  |          |             | Tüm özellikler bu teknolojide mevcut değildir.                                                                         |
| Yerli HLS Geçiş (Html5)         |        | X                |          |             | Platform kısıtlamaları nedeniyle tüm özellikler bu teknolojide bulunmaz.                                            |
| Özellikler                                |        |                  |          |             |                                                                                                                      |
| API Desteği                             | X      |                  |          |             | Bilinen sorunlar listesine bakın                                                                                                |
| Temel UI                                | X      |                  |          |                                                                                                                                    |
| JavaScript ile başlatma       | X      |                  |          |             |                                                                                                                      |
| Video etiketi yle başlatma        |        | X                |          |             |                                                                                                                      |
| Segment adresleme - ZamanA Dayalı         | X      |                  |          |             |                                                                                                                      |
| Segment adresleme - Dizin Tabanlı        |        |                  |          | X           |                                                                                                                      |
| Segment adresleme - Bayt Tabanlı         |        |                  |          | X           |                                                                                                                      |
| Azure Medya Hizmetleri URL yeniden yazar       |        | X                |          |             |                                                                                                                      |
| Erişilebilirlik - Altyazılar ve Altyazılar  |        | X                |          |             |  WebVTT isteğe bağlı desteklenen, canlı CEA 708 kısmen test                                                       |
| Erişilebilirlik - Anahtarlar                 | X      |                  |          |             |                                                                                                                      |
| Erişilebilirlik - Yüksek Karşıtlık           |        | X                |          |             |                                                                                                                      |
| Erişilebilirlik - Sekme Odak               |        | X                |          |             |                                                                                                                      |
| Hata İletisi                         |        | X                |          |             | Hata iletileri teknisyenler arasında tutarsız                                                                         |
| Olay Tetikleyici                        | X      |                  |          |             |                                                                                                                      |
| Tanılama                             |        | X                |          |             | Tanılama bilgileri yalnızca AzureHtml5JS teknolojisinde ve kısmen silverlightss teknolojisinde kullanılabilir. |
| Özelleştirilebilir Teknik Sipariş                 |        | X                |          |             |                                                                                                                      |
| Sezgisel - Temel                      | X      |                  |          |             |                                                                                                                      |
| Sezgisel - Özelleştirme              |        |                  | X        |             | Özelleştirme yalnızca AzureHtml5JS teknolojisiyle kullanılabilir.                                                          |
| Süreksizlikler                         | X      |                  |          |             |                                                                                                                      |
| Bitrate'yi seçin                          | X      |                  |          |             | Bu API yalnızca AzureHtml5JS ve FlashSS teknisyenlerinde kullanılabilir.                                                    |
| Çoklu Ses Akışı                      |        | X                |          |             | Programlı ses anahtarı AzureHtml5JS ve FlashSS teknisyenlerinde desteklenir ve AzureHtml5JS, FlashSS ve yerel Html5 (Safari'de) üzerinden ui seçimi yoluyla kullanılabilir.  Çoğu platform ses akışlarını değiştirmek için aynı codec özel verileri gerektirir (aynı codec, kanal, örnekleme hızı, vb.). |
| UI Yerelleştirme                         |        | X                |          |             |                                                                                                                      |
| Çok örnekli Oynatma                 |        |                  |          | X           | Bu senaryo bazı teknisyenler için çalışabilir, ancak şu anda desteklenmeyen ve test edilmemiştir. Ayrıca iframes kullanarak çalışmak için bu alabilirsiniz |
| Reklam Desteği                             |        | x                |          |             | AMP, AzureHtml5JS teknolojisinde VOD için VAST uyumlu reklam sunucularından orta ve post-roll doğrusal reklamlar eklemeyi destekler |
| Analiz                               |        | X                |          |             | AMP, seçtiğiniz bir Analytics arka ucuna göndermek için analitik ve tanıolaylarını dinleme olanağı sağlar.  Platform sınırlamaları nedeniyle tüm etkinlikler ve özellikler teknisyenler arasında kullanılamaz.                                                                            |
| Özel Kaplamalar                            |        |                  | X        |             | Ayarı ayar denetimlerini AMP'de ve kendi HTML ve CSS'nizi kullanarak false'a çevirin.           |
| Bar Temizleme Ara                      |        |                  |          | X           |                                                                                                                      |
| Hile-Oyun                              |        |                  |          | X           |                                                                                                                      |
| Yalnızca Ses                              |        |                  |          | X           | Uyarlanabilir Akış için bazı teknisyenlerde çalışabilir, ancak şu anda desteklenmemektedir ve AzureHtml5JS'de çalışmaz. Platform destekliyorsa, aşamalı MP3 oynatma HTML5 teknolojisi ile çalışabilir.                                                                                                        |
| Yalnızca Video                              |        |                  |          | X           | Uyarlanabilir Akış için bazı teknisyenlerde çalışabilir, ancak şu anda desteklenmemektedir ve AzureHtml5JS'de çalışmaz.      |
| Çok Dönemli Sunum               |        |                  |          | X                                                                                                                                  |
| Birden fazla kamera açısı                  |        |                  |          | X           |                                                                                                                      |
| Oynatma Hızı                          |        | X                |          |             | Chrome'daki kısmi bir hata nedeniyle mobil durum dışında çoğu senaryoda oynatma hızı desteklenir                 |

## <a name="next-steps"></a>Sonraki adımlar ##
- [Azure Media Player Quickstart](azure-media-player-quickstart.md)