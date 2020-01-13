---
title: "Öğretici: Google Maps 'tan Azure Maps 'e geçiş | Microsoft Azure haritaları"
description: Google Maps 'tan Microsoft Azure Maps 'a geçirmeye yönelik bir öğretici. Yönergeler, Azure Maps API 'Lerine ve SDK 'larına nasıl geçlenebileceğine kılavuzluk eder.
author: rbrundritt
ms.author: richbrun
ms.date: 12/17/2019
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: cpendle
ms.custom: ''
ms.openlocfilehash: 0e841b1f386d45ddb4af8598855d8e739750307e
ms.sourcegitcommit: f9601bbccddfccddb6f577d6febf7b2b12988911
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2020
ms.locfileid: "75910734"
---
# <a name="migrate-from-google-maps-to-azure-maps"></a>Google Maps 'tan Azure Maps 'e geçiş

Bu öğreticide Web, mobil ve sunucu tabanlı uygulamaların Google Maps 'tan Microsoft Azure haritalar platformuna nasıl geçirileceğiyle ilgili Öngörüler sunulmaktadır. Bu öğretici, Azure Maps 'a geçiş için karşılaştırılma kodu örnekleri, geçiş önerileri ve en iyi yöntemleri içerir.

## <a name="azure-maps-platform-overview"></a>Azure haritalar platformuna genel bakış

Azure Maps, geliştiricilerin Web ve mobil uygulamalara coğrafi bağlam sağlamak amacıyla düzenli olarak güncelleştirilmiş harita verileriyle paketlenmiş tüm sektörlerin güçlü Jeo-uzamsal özelliklerine sahip geliştiriciler sağlar. Azure haritalar, geliştirme kolaylığı, arama, yönlendirme, trafik, saat dilimleri, coğrafi konum, bölge sınırlama, harita verileri, hava durumu, taşınabilirlik ve uzamsal Işlemler için Web ve Android SDK 'Ları ile birlikte daha kolay, esnek ve birden çok platformda taşınabilir.

## <a name="high-level-platform-comparison"></a>Üst düzey platform karşılaştırması

Aşağıdaki tabloda, Google Maps özelliklerinin üst düzey bir listesi ve Azure Maps 'ta bu özellikler için göreli destek sağlanmaktadır. Bu liste erişilebilirlik, bölge sınırlaması API 'Leri, Iselees, uzamsal işlemler, doğrudan harita kutucuğu erişimi, Batch Hizmetleri ve veri kapsamı karşılaştırmaları (örn. Imagery kapsamı) gibi ek Azure Maps özellikleri içermez.

| Google Haritalar özelliği         | Azure haritalar desteği                     |
|-----------------------------|:--------------------------------------:|
| Web SDK                     | ✓                                      |
| Android SDK                 | ✓                                      |
| iOS SDK                     | Planlandı                                |
| REST hizmeti API 'Leri           | ✓                                      |
| Yönler (Yönlendirme)        | ✓                                      |
| Mesafe Matrisi             | ✓                                      |
| Nedeniyle                   | Planlandı                                |
| Coğrafi kodlama (Ileri/ters) | ✓                                      |
| Coğrafi Konum                 | Yok                                    |
| Konum arama               | ✓                                      |
| Yer ayrıntıları              | Yok – Web sitesi & telefon numarası kullanılabilir |
| Fotoğrafları yerleştir               | Yok                                    |
| Otomatik tamamlamayı yerleştir          | ✓                                      |
| Statik haritalar                 | ✓                                      |
| Statik cadde görünümü          | Yok                                    |
| Saat Dilimi                   | ✓                                      |
| Harita katıştırılmış API           | Yok                                    |
| Harita URL 'Leri                    | Yok                                    |

Google Maps temel anahtar tabanlı kimlik doğrulaması sağlar. Azure haritalar, temel anahtar tabanlı kimlik doğrulamasının yanı sıra yüksek düzeyde güvenli Azure Active Directory kimlik doğrulaması sağlar.

## <a name="licensing-considerations"></a>Lisanslama konuları

Google Maps ' den Azure Maps 'a geçiş yaparken, lisanslanması gereken noktaların aşağıdaki noktaları dikkate alınmalıdır.

- Azure haritalar, yüklenen harita kutucuklarının sayısına göre etkileşimli haritalar kullanımı için ücretlendirirken, Google Haritalar harita denetimi yüklemesi için ücretlendirir. Etkileşimli Azure Haritalar SDK 'lerinde, geliştirici maliyetini azaltmak için harita kutucukları otomatik olarak önbelleğe alınır. Yüklenen her 15 harita kutucuğu için bir Azure haritalar işlemi oluşturulur. Etkileşimli Azure Haritalar SDK 'Ları 512 piksellik kutucukları kullanır ve ortalama üzerinde sayfa görünümü başına bir veya daha az işlem oluşturur.
- Bu, eşleme kutucukları kullandığından ve Kullanıcı haritayı yakınlaştırıp yakınlaşmadığı sürece, genellikle bir işlemin yalnızca bir kısmını bir kez oluşturacak şekilde, bu harita, Google Maps Web hizmetlerinden gelen statik harita görüntülerini Azure Maps web SDK 'Sı ile değiştirmek için çok daha düşük maliyetli bir işlemdir. Azure Haritalar Web SDK 'Sı, kaydırma ve yakınlaştırmasını devre dışı bırakmaya yönelik seçeneklere sahiptir. Ayrıca, Azure Maps web SDK 'Sı, statik bir harita Web hizmetinden çok daha fazla görselleştirme seçeneği sağlar.
- Azure haritalar, platformdaki verilerin Azure 'da depolanmasını sağlar. Ayrıca [kullanım koşullarına](https://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=46)göre altı aya kadar bir yerde önbelleğe alınabilir.

Azure haritalar için bazı ilgili kaynaklar aşağıda verilmiştir:

- [Azure haritalar fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/azure-maps/)
- [Azure fiyatlandırma hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/?service=azure-maps)
- [Azure Haritalar kullanım koşulları](https://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=46) (Microsoft Online Services koşulları 'nda bulunur)
- [Azure haritalar 'da doğru fiyatlandırma katmanını seçin](https://docs.microsoft.com/azure/azure-maps/choose-pricing-tier)

## <a name="suggested-migration-plan"></a>Önerilen geçiş planı

Aşağıda, üst düzey bir geçiş planı verilmiştir.

1. Azure haritalar 'ın, uygulamanızın kullandığı SDK 'Ları ve Hizmetleri Eşlediklerinin envanterini alın ve Azure Maps ' a geçiş yapmanız için alternatif SDK 'lar ve hizmetler sağladığını doğrulayın.
2. [https://azure.com](https://azure.com)' de bir Azure aboneliği oluşturun (henüz yoksa).
3. Azure haritalar hesabı ([Belgeler](https://docs.microsoft.com/azure/azure-maps/how-to-manage-account-keys)) ve kimlik doğrulama anahtarı veya Azure Active Directory ([Belgeler](https://docs.microsoft.com/azure/azure-maps/how-to-manage-authentication)) oluşturun.
4. Uygulama kodunuzu geçirin.
5. Geçirilen uygulamanızı test edin.
6. Geçirilen uygulamanızı üretime dağıtın.

## <a name="azure-maps-technical-resources"></a>Azure haritalar teknik kaynakları

Azure haritalar için yararlı teknik kaynakların listesi aşağıda verilmiştir.

- Genel Bakış: [https://azure.com/maps](https://azure.com/maps)
- Belgeler: [https://aka.ms/AzureMapsDocs](https://aka.ms/AzureMapsDocs)
- Web SDK kodu örnekleri: [https://aka.ms/AzureMapsSamples](https://aka.ms/AzureMapsSamples)
- Geliştirici forumları: [https://aka.ms/AzureMapsForums](https://aka.ms/AzureMapsForums)
- Videolar: [https://aka.ms/AzureMapsVideos](https://aka.ms/AzureMapsVideos)
- Blog: [https://aka.ms/AzureMapsBlog](https://aka.ms/AzureMapsBlog)
- Azure haritalar geri bildirimi (UserVoice): [https://aka.ms/AzureMapsFeedback](https://aka.ms/AzureMapsFeedback)

## <a name="migration-support"></a>Geçiş desteği

Geliştiriciler, [Forum](https://aka.ms/AzureMapsForums) aracılığıyla veya birçok Azure destek seçeneğinden biri aracılığıyla geçiş desteği arayabilir: [https://azure.microsoft.com/support/options](https://azure.microsoft.com/support/options)

## <a name="next-steps"></a>Sonraki adımlar

Google Maps uygulamanızı şu makalelerle geçirmeyi öğrenin:

> [!div class="nextstepaction"]
> [Bir Web uygulamasını geçirme](migrate-from-google-maps-web-app.md)

> [!div class="nextstepaction"]
> [Android uygulamasını geçirme](migrate-from-google-maps-android-app.md)

> [!div class="nextstepaction"]
> [Web hizmetini geçirme](migrate-from-google-maps-web-services.md)
