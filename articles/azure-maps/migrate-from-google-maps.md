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
ms.openlocfilehash: 175625ab9fca9103bde027c3c0ea0986806ad846
ms.sourcegitcommit: 2823677304c10763c21bcb047df90f86339e476a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2020
ms.locfileid: "77208311"
---
# <a name="migrate-from-google-maps-to-azure-maps"></a>Google Maps 'tan Azure Maps 'e geçiş

Bu öğreticide Web, mobil ve sunucu tabanlı uygulamaların Google Maps 'tan Microsoft Azure haritalar platformuna nasıl geçirileceğiyle ilgili Öngörüler sunulmaktadır. Bu öğretici, Azure Maps 'a geçiş için karşılaştırılma kodu örnekleri, geçiş önerileri ve en iyi yöntemleri içerir.

## <a name="azure-maps-platform-overview"></a>Azure haritalar platformuna genel bakış

Azure haritalar, geliştiricilerin tüm sektörler açısından güçlü Jeo uzamsal yetenekler sağlar. Bu yetenekler, Web ve mobil uygulamalar için coğrafi bağlam sağlamak amacıyla düzenli olarak güncelleştirilmiş harita verileriyle paketlenmiştir. Azure haritalar 'ın Azure One API uyumlu bir REST API kümesi vardır. REST API 'Leri, haritalar Işleme, arama, yönlendirme, trafik, saat dilimi, coğrafi konum, bölge sınırlama, harita verileri, hava durumu, taşınabilirlik ve uzamsal Işlemler sunar. Hem Web hem de Android SDK 'larının yanı sıra, geliştirme işlemlerini kolay, esnek ve birden çok platformda taşınabilir hale getirebilirsiniz.

## <a name="high-level-platform-comparison"></a>Üst düzey platform karşılaştırması

Tablo, Google Maps özelliklerine karşılık gelen Azure Maps özelliklerinin üst düzey bir listesini sağlar. Bu liste Azure Maps özelliklerinin tümünü göstermez. Ek Azure haritaları özellikleri şunlardır: erişilebilirlik, bölge sınırlaması, ısodenes, uzamsal işlemler, doğrudan harita kutucuk erişimi, Batch Hizmetleri ve veri kapsamı karşılaştırmaları (yani, ımatay kapsamı).

| Google Haritalar özelliği         | Azure haritalar desteği                     |
|-----------------------------|:--------------------------------------:|
| Web SDK                     | ✓                                      |
| Android SDK                 | ✓                                      |
| iOS SDK                     | Planlandı                                |
| REST hizmeti API 'Leri           | ✓                                      |
| Yönler (Yönlendirme)        | ✓                                      |
| Uzaklık matrisi             | ✓                                      |
| Nedeniyle                   | Planlandı                                |
| Coğrafi kodlama (Ileri/ters) | ✓                                      |
| Konumu                 | Yok                                    |
| Konum arama               | ✓                                      |
| Yer ayrıntıları              | Yok – Web sitesi & telefon numarası kullanılabilir |
| Fotoğrafları yerleştir               | Yok                                    |
| Otomatik tamamlamayı yerleştir          | ✓                                      |
| Statik haritalar                 | ✓                                      |
| Statik cadde görünümü          | Yok                                    |
| Saat Dilimi                   | ✓                                      |
| Harita katıştırılmış API           | Yok                                    |
| Harita URL 'Leri                    | Yok                                    |

Google Maps temel anahtar tabanlı kimlik doğrulaması sağlar. Azure haritalar hem temel anahtar tabanlı kimlik doğrulaması hem de Azure Active Directory kimlik doğrulaması sağlar. Azure Active Directory kimlik doğrulaması, temel anahtar tabanlı kimlik doğrulamaya kıyasla daha fazla güvenlik özelliği sağlar.

## <a name="licensing-considerations"></a>Lisanslama konuları

Google Maps 'lerden Azure Maps 'a geçiş yaparken lisanslama hakkında aşağıdaki noktaları göz önünde bulundurun.

- Azure Maps, yüklü harita kutucuklarının sayısına bağlı olarak etkileşimli haritalar kullanımına yönelik ücretler ücretlendirir. Diğer yandan, Google Haritalar harita denetimini yükleme ücretini ücretlendirir. Etkileşimli Azure Haritalar SDK 'lerinde, eşleme kutucukları, geliştirme maliyetini azaltmak için otomatik olarak önbelleğe alınır. Yüklenen her 15 harita kutucuğu için bir Azure haritalar işlemi oluşturulur. Etkileşimli Azure Haritalar SDK 'Ları 512 piksellik kutucukları kullanır ve ortalama olarak sayfa görünümü başına bir veya daha az işlem oluşturur.
- Genellikle, Azure Maps web SDK ile Google Maps Web hizmetlerinden statik harita görüntülerini değiştirme maliyeti de geçerli olur. Azure Haritalar Web SDK 'Sı harita kutucukları kullanır. Kullanıcı haritayı yakınlaştırıp yakınlaşmadığı takdirde, hizmet genellikle harita yükü başına bir işlemin bir bölümünü oluşturur. Azure Haritalar Web SDK 'Sı isterseniz, kaydırma ve yakınlaştırmayı devre dışı bırakma seçenekleri vardır. Ayrıca, Azure Maps web SDK 'Sı, statik harita Web hizmetinden daha fazla sayıda görselleştirme seçeneği sağlar.
- Azure haritalar, platformdaki verilerin Azure 'da depolanmasını sağlar. Ayrıca, veriler, [kullanım koşullarına](https://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=46)göre altı aya kadar bir yerde önbelleğe alınabilir.

Azure haritalar için bazı ilgili kaynaklar aşağıda verilmiştir:

- [Azure haritalar fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/azure-maps/)
- [Azure fiyatlandırma hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/?service=azure-maps)
- [Azure Haritalar kullanım koşulları](https://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=46) (Microsoft Online Services koşulları 'nda bulunur)
- [Azure haritalar 'da doğru fiyatlandırma katmanını seçin](https://docs.microsoft.com/azure/azure-maps/choose-pricing-tier)

## <a name="suggested-migration-plan"></a>Önerilen geçiş planı

Aşağıda, üst düzey bir geçiş planı verilmiştir.

1. Uygulamanızın kullandığı Google Maps SDK 'Ları ve hizmetlerini envanterini alın. Azure haritalar 'ın alternatif SDK 'lar ve hizmetler sağladığını doğrulayın.
2. Henüz bir aboneliğiniz yoksa [https://azure.com](https://azure.com)bir Azure aboneliği oluşturun.
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

Google Maps uygulamanızı şu makalelerle nasıl geçirileceğiyle ilgili ayrıntıları öğrenin:

> [!div class="nextstepaction"]
> [Bir Web uygulamasını geçirme](migrate-from-google-maps-web-app.md)

> [!div class="nextstepaction"]
> [Android uygulamasını geçirme](migrate-from-google-maps-android-app.md)

> [!div class="nextstepaction"]
> [Web hizmetini geçirme](migrate-from-google-maps-web-services.md)
