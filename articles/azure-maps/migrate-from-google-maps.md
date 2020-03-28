---
title: "Öğretici: Google Haritalar'dan Azure Haritalar'a geçiş | Microsoft Azure Haritaları"
description: Google Haritalar'dan Microsoft Azure Haritalar'a nasıl geçirilir hakkında bir öğretici. Rehberlik, Azure Haritalar API'larına ve SDK'larına nasıl geçilebilirsiniz size yol gösterir.
author: rbrundritt
ms.author: richbrun
ms.date: 12/17/2019
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: cpendle
ms.custom: ''
ms.openlocfilehash: 7b73923b7fc32ae83bfc8405d074835c02031a63
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77913710"
---
# <a name="migrate-from-google-maps-to-azure-maps"></a>Google Haritalar'dan Azure Haritalar'a geçiş

Bu öğretici, Web, mobil ve sunucu tabanlı uygulamaları Google Haritalar'dan Microsoft Azure Haritalar platformuna nasıl geçirilir hakkında öngörüler sağlar. Bu öğretici, karşılaştırmalı kod örneklerini, geçiş önerilerini ve Azure Haritalar'a geçiş için en iyi uygulamaları içerir.

## <a name="azure-maps-platform-overview"></a>Azure Haritalar platformuna genel bakış

Azure Haritalar, tüm sektörlerden geliştiricilere güçlü coğrafi yetenekler sağlar. Bu özellikler, web ve mobil uygulamalar için coğrafi bağlam sağlamak için düzenli olarak güncellenen harita verileriyle doludur. Azure Haritalar'da Azure One API uyumlu REST API seti vardır. REST API'leri Haritalar Oluşturma, Arama, Yönlendirme, Trafik, Saat Dilimleri, Coğrafi Konum, Geofencing, Harita Verileri, Hava Durumu, Mobilite ve Mekansal İşlemler sunar. Geliştirmeyi kolaylaştırmak, esnek ve taşınabilir hale getirmek için operasyonlara hem Web hem de Android SDK'lar eşlik eder.

## <a name="high-level-platform-comparison"></a>Üst düzey platform karşılaştırması

Tablo, Google Haritalar özelliklerine karşılık gelen Azure Haritalar özelliklerinin üst düzey bir listesini sağlar. Bu liste, Azure Haritalar özelliklerinin tümünü göstermez. Ek Azure Haritalar özellikleri şunlardır: erişilebilirlik, geofencing, isochrones, mekansal işlemler, doğrudan harita döşeme erişimi, toplu iş hizmetleri ve veri kapsamı karşılaştırmaları (diğer bir süre önce görüntü kapsamı).

| Google Haritalar özelliği         | Azure Haritalar desteği                     |
|-----------------------------|:--------------------------------------:|
| Web SDK                     | ✓                                      |
| Android SDK                 | ✓                                      |
| iOS SDK                     | Planlandı                                |
| REST Servis API'leri           | ✓                                      |
| Yol Tarifi (Yönlendirme)        | ✓                                      |
| Mesafe Matrisi             | ✓                                      |
| Yükseklik                   | Planlandı                                |
| Coğrafi kodlama (İleri/Geri) | ✓                                      |
| Coğrafi Konum                 | Yok                                    |
| En Yakın Yollar               | ✓                                      |
| Yerler Arama               | ✓                                      |
| Yerler Detayları              | N/A – web sitesi & telefon numarası mevcuttur |
| Yerler Fotoğraflar               | Yok                                    |
| Otomatik Tamamlama yı yerleştir          | ✓                                      |
| Yola Yapış                | ✓                                      |
| Hız Limitleri                | ✓                                      |
| Statik Haritalar                 | ✓                                      |
| Statik Sokak Görünümü          | Yok                                    |
| Saat Dilimi                   | ✓                                      |
| Haritalar Gömülü API           | Yok                                    |
| Harita URL'leri                    | Yok                                    |

Google Haritalar temel anahtar tabanlı kimlik doğrulaması sağlar. Azure Haritalar hem temel anahtar tabanlı kimlik doğrulaması hem de Azure Etkin Dizin kimlik doğrulaması sağlar. Azure Etkin Dizin kimlik doğrulaması, temel anahtar tabanlı kimlik doğrulamasına kıyasla daha fazla güvenlik özelliği sağlar.

## <a name="licensing-considerations"></a>Lisanslama hususları

Google Haritalar'dan Azure Haritalar'a geçiş yaparken, lisanslama yla ilgili aşağıdaki noktaları göz önünde bulundurun.

- Azure Haritalar, yüklenen harita kutucuklarının sayısını temel alan etkileşimli haritaların kullanımı için ücret lendirmektedir. Öte yandan, Google Haritalar harita denetimini yüklemek için ücret alır. Etkileşimli Azure Haritalar SDK'larında, geliştirme maliyetini azaltmak için harita kutucukları otomatik olarak önbelleğe alınır. Yüklenen her 15 harita kutucisi için bir Azure Haritalar hareketi oluşturulur. Etkileşimli Azure Haritalar SDK'ları 512 piksel kutucuk kullanır ve ortalama olarak sayfa görünümü başına bir veya daha az işlem oluşturur.
- Genellikle, Google Haritalar web hizmetlerinden statik harita görüntülerini Azure Haritalar Web SDK ile değiştirmek için daha uygun maliyetlidir. Azure Haritalar Web SDK harita kutucuklarını kullanır. Kullanıcı haritayı kaydırıp yakınlaştırmadıkça, hizmet genellikle harita yükü başına bir işlemin yalnızca bir kısmını oluşturur. Azure Haritalar web SDK'nın, istenirse kaydırma ve yakınlaştırmayı devre dışı bırakma seçenekleri vardır. Ayrıca, Azure Haritalar web SDK statik harita web hizmetiçok daha fazla görselleştirme seçenekleri sağlar.
- Azure Haritalar, platformundaki verilerin Azure'da depolanmasına olanak tanır. Ayrıca, veriler [kullanım koşullarına](https://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=46)göre altı aya kadar başka bir yerde önbelleğe alınabilir.

Azure Haritalar için bazı ilgili kaynaklar şunlardır:

- [Azure Haritalar fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/azure-maps/)
- [Azure fiyatlandırma hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/?service=azure-maps)
- [Azure Haritalar kullanım süresi](https://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=46) (Microsoft Çevrimiçi Hizmetler Koşulları'na dahildir)
- [Azure Haritalar'da doğru fiyatlandırma katmanını seçin](https://docs.microsoft.com/azure/azure-maps/choose-pricing-tier)

## <a name="suggested-migration-plan"></a>Önerilen geçiş planı

Aşağıda üst düzey bir geçiş planı verem.

1. Uygulamanızın kullandığı Google Haritalar SDK'larının ve hizmetlerinin envanterini alın. Azure Haritalar'ın alternatif SDK'lar ve hizmetler sağladığını doğrulayın.
2. Zaten bir aboneliğiniz yoksa, 'den [https://azure.com](https://azure.com)bir Azure aboneliği oluşturun.
3. Bir Azure Haritalar hesabı[(belgeler)](https://docs.microsoft.com/azure/azure-maps/how-to-manage-account-keys)ve kimlik doğrulama anahtarı veya Azure Etkin Dizini[(belgeler)](https://docs.microsoft.com/azure/azure-maps/how-to-manage-authentication)oluşturun.
4. Uygulama kodunuzu geçirin.
5. Geçirilen uygulamanızı test edin.
6. Geçirilen uygulamanızı üretime dağıtın.

## <a name="azure-maps-technical-resources"></a>Azure Haritalar teknik kaynakları

Azure Haritalar için yararlı teknik kaynakların bir listesi aşağıda veda edilmiştir.

- Genel bakış:[https://azure.com/maps](https://azure.com/maps)
- Belge:[https://aka.ms/AzureMapsDocs](https://aka.ms/AzureMapsDocs)
- Web SDK Kod Örnekleri:[https://aka.ms/AzureMapsSamples](https://aka.ms/AzureMapsSamples)
- Geliştirici Forumları:[https://aka.ms/AzureMapsForums](https://aka.ms/AzureMapsForums)
- Video:[https://aka.ms/AzureMapsVideos](https://aka.ms/AzureMapsVideos)
- Blog:[https://aka.ms/AzureMapsBlog](https://aka.ms/AzureMapsBlog)
- Teknik Blog:[https://aka.ms/AzureMapsTechBlog](https://aka.ms/AzureMapsTechBlog)
- Azure Haritalar Geri Bildirimi (UserVoice):[https://aka.ms/AzureMapsFeedback](https://aka.ms/AzureMapsFeedback)
- [Azure Haritalar Jupyter Notebook] [https://github.com/Azure-Samples/Azure-Maps-Jupyter-Notebook]

## <a name="migration-support"></a>Geçiş desteği

Geliştiriciler [forumlar](https://aka.ms/AzureMapsForums) aracılığıyla veya azure destek seçeneklerinden biri aracılığıyla geçiş desteği arayabilirler:[https://azure.microsoft.com/support/options](https://azure.microsoft.com/support/options)

## <a name="next-steps"></a>Sonraki adımlar

Google Haritalar uygulamanızı aşağıdaki makalelerle nasıl geçirebilirsiniz ayrıntılarını öğrenin:

> [!div class="nextstepaction"]
> [Web uygulamasını geçirme](migrate-from-google-maps-web-app.md)

> [!div class="nextstepaction"]
> [Android uygulamasını geçirme](migrate-from-google-maps-android-app.md)

> [!div class="nextstepaction"]
> [Web hizmetini geçirme](migrate-from-google-maps-web-services.md)
