---
title: "Öğretici: Bing Haritalar 'dan Azure Maps 'e geçiş | Microsoft Azure haritaları"
description: Bing Haritalar 'dan Microsoft Azure haritalarını geçirmeye yönelik bir öğretici. Yönergeler, Azure Maps API 'Lerine ve SDK 'larına nasıl geçlenebileceğine kılavuzluk eder.
author: rbrundritt
ms.author: richbrun
ms.date: 9/10/2020
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: cpendle
ms.custom: ''
ms.openlocfilehash: 643e49bdba76051c873ed549d5f6c21487f34056
ms.sourcegitcommit: 07166a1ff8bd23f5e1c49d4fd12badbca5ebd19c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2020
ms.locfileid: "90108923"
---
# <a name="migrate-from-bing-maps-to-azure-maps"></a>Bing Haritalar 'dan Azure Maps 'e geçiş

Bu kılavuzda, Web, mobil ve sunucu tabanlı uygulamaların Bing Haritalar 'dan Azure haritalar platformuna nasıl geçirileceğiyle ilgili Öngörüler sunulmaktadır. Bu kılavuzda, Azure Maps 'e geçiş için karşılaştırılma kodu örnekleri, geçiş önerileri ve en iyi uygulamalar yer almaktadır.

## <a name="azure-maps-platform-overview"></a>Azure haritalar platformuna genel bakış

Azure haritalar, geliştiricilerin Web ve mobil uygulamalar için coğrafi bağlam sağlamak üzere sunulan en güncel eşleme verileriyle paketlenmiş, tüm sektörlerin güçlü Jeo-uzamsal yeteneklerini sunmaktadır. Azure haritalar; haritalar, arama, yönlendirme, trafik, saat dilimleri, bölge sınırlaması, harita verileri, hava durumu verileri ve birden çok platformda daha kolay, esnek ve taşınabilir hale getirmek için hem Web hem de Android SDK 'Lar tarafından birlikte bir Azure tek API uyumlu REST API kümesidir. [Azure haritalar da Power BI de mevcuttur](power-bi-visual-getting-started.md).

## <a name="high-level-platform-comparison"></a>Üst düzey platform karşılaştırması

Aşağıdaki tablo, Bing Haritalar özelliklerinin üst düzey bir listesini ve Azure Maps 'taki bu özellikler için göreli desteği sağlar. Bu liste erişilebilirlik, bölge sınırlaması API 'Leri, Trafik Hizmetleri, uzamsal işlemler, doğrudan harita kutucuğu erişimi ve Batch hizmetleri gibi ek Azure Maps özellikleri içermez.

| Bing Haritalar özelliği                     | Azure haritalar desteği |
|---------------------------------------|:------------------:|
| Web SDK’sı                               | ✓                  |
| Android SDK                           | ✓                  |
| iOS SDK                               | Planlandı            |
| UWP SDK 'Sı                               | Planlandı            |
| WPF SDK 'Sı                               | Planlandı            |
| REST hizmeti API 'Leri                     | ✓                  |
| Otomatik öneri                           | ✓                  |
| Yönergeler (kamyon dahil)          | ✓                  |
| Uzaklık matrisi                       | ✓                  |
| Yükseltmeleri                            | Planlandı            |
| Imagery – statik eşleme                  | ✓                  |
| Imagery meta verileri                      | ✓                  |
| İzokron                            | ✓                  |
| Yerel Öngörüler                        | ✓                  |
| Yerel arama                          | ✓                  |
| Konum tanıma                  | ✓                  |
| Konumlar (ileri/ters coğrafi kodlama) | ✓                  |
| İyileştirilmiş yolculuk yolları            | Planlandı            |
| Yollara yapış                         | ✓                  |
| Uzamsal veri Hizmetleri (SDS)           | Kısmi            |
| Saat Dilimi                             | ✓                  |
| Trafik olayları                     | ✓                  |
| Yapılandırma temelli haritalar             | Yok                |

Bing Haritalar temel anahtar tabanlı kimlik doğrulaması sağlar. Azure haritalar, temel anahtar tabanlı kimlik doğrulamasının yanı sıra yüksek düzeyde güvenli Azure Active Directory kimlik doğrulaması sağlar.

## <a name="licensing-considerations"></a>Lisanslama konuları

Bing Haritalar 'dan Azure Maps 'a geçiş yaparken, lisanslanması gereken, aşağıdakiler göz önünde bulundurulmalıdır.

-   Azure Maps, yüklenen harita kutucuklarının sayısına göre etkileşimli haritalar kullanımı için ücretlendirirken, Bing Haritalar harita denetimi 'nin (oturumlar) yüklemesi için ücretlendirir. Azure haritalar 'da, geliştirici maliyetini azaltmak için harita kutucukları otomatik olarak önbelleğe alınır. Yüklenen her 15 harita kutucuğu için bir Azure haritalar işlemi oluşturulur. Etkileşimli Azure Haritalar SDK 'Ları 512 piksellik kutucukları kullanır ve ortalama üzerinde sayfa görünümü başına bir veya daha az işlem oluşturur.

-   Azure haritalar, platformdaki verilerin Azure 'da depolanmasını sağlar. Ayrıca [kullanım koşullarına](https://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31)göre altı aya kadar bir yerde önbelleğe alınabilir.

Azure haritalar için lisanslama ile ilgili bazı kaynaklar aşağıda verilmiştir:

-   [Azure haritalar fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/azure-maps/)
-   [Azure fiyatlandırma hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/?service=azure-maps)
-   [Azure Haritalar kullanım koşulları](https://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) (Microsoft Online Services koşulları 'nda bulunur)
-   [Azure haritalar 'da doğru fiyatlandırma katmanını seçin](https://docs.microsoft.com/azure/azure-maps/choose-pricing-tier)

## <a name="suggested-migration-plan"></a>Önerilen geçiş planı

Aşağıda, üst düzey bir geçiş planı verilmiştir.

1.  Uygulamanızın kullandığı Bing Haritalar SDK 'Ları ve hizmetleri hakkında daha fazla işlem yapın ve Azure Maps 'e geçiş yapmanız için alternatif SDK 'lar ve hizmetler sağladığını doğrulayın.
2.  Bir Azure aboneliği oluşturun (henüz bir tane yoksa) <https://azure.com> .
3.  Azure haritalar hesabı ([Belgeler](https://docs.microsoft.com/azure/azure-maps/how-to-manage-account-keys)) ve kimlik doğrulama anahtarı veya Azure Active Directory ([Belgeler](https://docs.microsoft.com/azure/azure-maps/how-to-manage-authentication)) oluşturun.
4.  Uygulama kodunuzu geçirin.
5.  Geçirilen uygulamanızı test edin.
6.  Geçirilen uygulamanızı üretime dağıtın.

## <a name="azure-maps-technical-resources"></a>Azure haritalar teknik kaynakları

Azure haritalar için yararlı teknik kaynakların listesi aşağıda verilmiştir.

-   Genel Bakış: https://azure.com/maps
-   Belgelerle <https://aka.ms/AzureMapsDocs>
-   Web SDK kodu örnekleri: <https://aka.ms/AzureMapsSamples>
-   Geliştirici forumları: <https://aka.ms/AzureMapsForums>
-   Larınız <https://aka.ms/AzureMapsVideos>
-   Lenemeyen <https://aka.ms/AzureMapsBlog>
-   Azure haritalar geri bildirimi (UserVoice): <https://aka.ms/AzureMapsFeedback>

## <a name="migration-support"></a>Geçiş desteği

Geliştiriciler, [Forum](https://aka.ms/AzureMapsForums) aracılığıyla veya birçok Azure destek seçeneğinden biri aracılığıyla geçiş desteği arayabilir: <https://azure.microsoft.com/support/options/>

## <a name="new-terminology"></a>Yeni terminoloji 

Aşağıda, Azure Maps 'ta farklı bir terim tarafından bilinen yaygın Bing Haritalar koşullarının bir listesi verilmiştir.

| Bing Haritalar terimi                    | Azure haritalar terimi                                                |
|-----------------------------------|----------------------------------------------------------------|
| Havadan                            | Uydu veya havadan                                            |
| Yönergeler                        | Yönlendirme olarak da adlandırılabilir                             |
| Varlıklar                          | Geometriler veya Özellikler                                         |
| `EntityCollection`                | Veri kaynağı veya katmanı                                           |
| `Geopoint`                        | Konum                                                       |
| `GeoXML`                          | Uzamsal GÇ modülündeki XML dosyaları                             |
| Zemin kaplama                    | Görüntü katmanı                                                    |
| Karma (eşleme türüne başvuru olarak) | Yollar ile uydu                                           |
| Kapat                           | Açılan Pencere                                                          |
| Konum                          | Konum                                                       |
| `LocationRect`                    | Sınırlayıcı kutusu                                                   |
| Eşleme Türü                          | Harita stili                                                      |
| Gezinti çubuğu                    | Harita stil Seçicisi, yakınlaştırma denetimi, aralıklı denetim, pusula denetimi |
| Raptiye                           | Kabarcık katmanı, simge katmanı veya HTML Işaretçisi                      |

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makalelerle Bing Haritalar uygulamanızı nasıl geçirileceğiyle ilgili ayrıntıları öğrenin:

> [!div class="nextstepaction"]
> [Web uygulamasını geçirme](migrate-from-bing-maps-web-app.md)

> [!div class="nextstepaction"]
> [Web hizmetini geçirme](migrate-from-bing-maps-web-services.md)
