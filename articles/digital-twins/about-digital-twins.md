---
title: Genel Bakış - Azure Dijital İkizler | Microsoft Dokümanlar
description: Uzamsal zekaya yönelik bir Azuer IoT çözümü olan Azure Digital Twins hakkında daha fazla bilgi edinin.
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.date: 12/30/2019
ms.topic: overview
ms.service: digital-twins
services: digital-twins
ms.custom: mvc
ms.openlocfilehash: 91386eaf7753f2aefada2d1423a11b366c3ee924
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79370908"
---
# <a name="overview-of-azure-digital-twins-preview"></a>Azure Dijital İkizler Önizleme'ye Genel Bakış

[!INCLUDE [digital-twins-preview-limit-alert](../../includes/digital-twins-preview-limit-alert.md)]

Azure Digital Twins Preview, fiziksel ortamın kapsamlı modellerini oluşturan bir Azure IoT hizmetidir. İnsanlar, boşluklar ve aygıtlar arasındaki ilişkileri ve etkileşimleri modellemek için uzamsal zeka grafikleri oluşturabilir.

Azure Digital Twins ile verileri birbirinden farklı birçok sensör yerine fiziksel bir alandan sorgulayabilirsiniz. Bu hizmet, dijital ve fiziksel dünyada veri akışı bağlantı yeniden kullanılabilir, yüksek ölçeklenebilir, mekansal farkında deneyimler oluşturmanıza yardımcı olur. Uygulamalarınız bu benzersiz alakalı bağlamsal özelliklerle geliştirilmiştir. 

Azure Dijital İkizler, depolar, ofisler, okullar, hastaneler ve bankalar gibi her türlü ortam için geçerlidir. Hatta stadyumlar, fabrikalar, otoparklar, parklar, akıllı ızgaralar ve şehirler için kullanılabilir. Azure Dijital İkizler'in yararlı olabileceği bazı senaryolar şunlardır:

- Bir fabrikanın bakım ihtiyaçlarını tahmin edin.
- Bir elektrik şebekesi için gerçek zamanlı enerji gereksinimlerini analiz edin.
- Bir ofis için kullanılabilir alanın kullanımını optimize edin.
- Çeşitli eyaletlerde günlük sıcaklığı izleyin.
- Yoğun insansız hava aracı yollarını izleyin.
- Otonom araçları tanımlayın.
- Bir bina için doluluk seviyelerini analiz edin.
- Mağazanızdaki en işlek yazar kasayı bulun.

Gerçek iş senaryonuz ne olursa olsun, azure digital twins aracılığıyla ilgili bir dijital örnek kullanılabilir.

Aşağıdaki video Azure Digital Twins'e daha yakından bakılıyor.

> [!VIDEO https://www.youtube.com/embed/TvN_NxpgyzQ]

## <a name="key-capabilities"></a>Temel işlevler

Azure Digital Twins aşağıdaki temel özelliklere sahiptir.

### <a name="spatial-intelligence-graph"></a>Uzamsal zeka grafı

[*Uzamsal zeka grafiği*](./concepts-objectmodel-spatialgraph.md#spatial-intelligence-graph)veya *uzamsal grafik,* fiziksel ortamın sanal bir temsilidir. Kişiler, yerler ve aygıtlar arasındaki ilişkileri modellemek için kullanabilirsiniz.

Bir mahalleye bağlı birkaç elektrik kullanım metre içeren akıllı bir yardımcı program uygulaması düşünün. Akıllı hizmet şirketi doğru izlemek ve elektrik kullanımı ve fatura tahmin gerekir. Her cihaz ve sensör, konumu ve faturalandırılacak müşteri yle ilgili bağlamla modellenmelidir. Bu tür karmaşık ilişkileri modellemek için uzamsal zeka grafiğini kullanabilirsiniz.

### <a name="digital-twin-object-models"></a>Dijital ikiz nesne modelleri

[Dijital ikiz nesne modelleri](./concepts-objectmodel-spatialgraph.md#digital-twins-object-models) önceden tanımlanmış aygıt protokolleri ve veri şemasıdır. Geliştirmeyi hızlandırmak ve kolaylaştırmak için çözümünüzün etki alanına özel gereksinimlerini hizalarlar.

Örneğin, bir oda doluluk uygulaması kampüs, bina, zemin ve oda gibi önceden tanımlanmış alan türlerini kullanabilir.

### <a name="multiple-and-nested-tenants"></a>Birden çok ve iç içe yerleştirilmiş kiracılar

Güvenli bir şekilde ölçeklendirilebilen ve birden çok kiracı için yeniden kullanılabilen çözümler oluşturabilirsiniz. Ayrıca, yalıtılmış ve güvenli bir şekilde erişilebilen ve kullanılabilen birden çok alt kiracı da oluşturabilirsiniz.

Buna örnek olarak, kiracının verilerini tek bir binadaki diğer kiracıların verilerinden yalıtmak üzere yapılandırılan bir alan kullanım uygulaması örnektir. Veya uygulama çok sayıda bina ile tek bir kiracı için verileri birleştirmek için kullanılır.

### <a name="advanced-compute-capabilities"></a>Gelişmiş işlem özellikleri

[Kullanıcı tanımlı işlevlerle,](./concepts-user-defined-functions.md)önceden tanımlanmış uç noktalara sinyal göndermek için gelen [aygıt verilerine](./concepts-device-ingress.md) karşı özel işlevler tanımlayabilir ve çalıştırabilirsiniz. Bu gelişmiş özellik, aygıt görevlerinin özelleştirmesini ve otomasyonunu geliştirir.

Buna örnek olarak, toprak nem sensörü okumalarını ve hava tahminini değerlendirmek için kullanıcı tanımlı bir işlev içeren akıllı bir tarım uygulaması örnek tir. Uygulama daha sonra sulama ihtiyaçları hakkında sinyaller gönderir.

### <a name="built-in-access-control"></a>Yerleşik erişim denetimi

[Rol tabanlı erişim denetimi](./security-role-based-access-control.md) ve Azure Etkin [Dizin](./security-authenticating-apis.md)gibi erişim ve kimlik yönetimi özelliklerini kullanarak, bireyler ve aygıtlar için erişimi güvenli bir şekilde denetleyebilirsiniz.

Bir örnek, bir odanın sakinlerinin sıcaklığı belirli bir aralıkta ayarlamasına olanak sağlayacak şekilde yapılandırılan bir tesis yönetimi uygulamasıdır. Tesis yöneticileri herhangi bir odadaki sıcaklığı herhangi bir değere göre ayarlayabın.

### <a name="ecosystem"></a>Ekosistem

Azure Dijital İkizler örneğini birçok güçlü Azure hizmetine bağlayabilirsiniz. Bu hizmetler arasında Azure Akış Analizi, Azure AI ve Azure Depolama yer almaktadır. Ayrıca Azure Haritalar, Microsoft Mixed Reality, Dynamics 365 veya Office 365'i de içerir.

Buna örnek olarak, birçok katta bulunan ekipleri ve cihazları temsil etmek için Azure Digital Twins kullanan akıllı bir ofis oluşturma uygulaması örnek tir. Aygıtlar sağlanan Dijital İkiz örneğine canlı veri akışı sağlarken, Akış Analizi işlem edilebilir anahtar öngörüler sağlamak için bu verileri işler. Veriler Azure Depolama'da depolanır ve paylaşılabilir dosya biçimine dönüştürülür. Dosya, Office 365 kullanılarak tüm kuruluşa dağıtılır.

## <a name="solutions-that-benefit-from-azure-digital-twins"></a>Azure Digital Twins'den yararlanan çözümler

Azure Digital Twins, fiziksel dünyayı ve birçok ilişkilerini temsil etmek için yararlıdır. IoT modelleme, veri işleme, olay işleme ve aygıt izlemeyi kolaylaştırır. Çeşitli sektörlerde aşağıdaki senaryolardan sadece birkaçını göz önünde bulundurun. Onlar kullanımından yararlanır:

* Bir emlak yönetim şirketine, ofis binasını yapılandırmanın en iyi yolları hakkında bilgi edinmek için zaman içinde bir alanın doluluk düzeylerini gösterin.
* Mobil uygulama için iş emri biletlerini tetikle. Güvenlik görevlilerini göndermek ve bir perakende alanı veya spor alanında hademe ve diğer hizmetleri zamanlamak için kullanın.
* Bir bina sakinine hangi odaların bir binada gerçek zamanlı olarak oturduğuni gösterin. Daha sonra, kendi ihtiyaçlarına uygun yolcu rezerv çalışma alanları yardımcı olur.
* Varlıkların bir boşluk içinde nerede bulunduğunu izleyin.
* Kullanıcı tercihlerini ve enerji şebekesi kısıtlamalarını modelleyerek elektrikli araç şarjlarını optimize edin.

## <a name="azure-digital-twins-in-the-context-of-other-iot-services"></a>Diğer IoT hizmetleri bağlamında Azure Dijital İkizler

Azure Digital Twins, verileri fiziksel dünyayla eşitlemek için Azure IoT Hub'ı kullanarak IoT cihazlarına ve sensörlerine bağlanır. Aşağıdaki diyagram, Azure Dijital İkizler'in diğer Azure IoT hizmetleriyle nasıl ilişkili olduğunu gösterir.

[![Azure Digital Twins, Azure IoT Hub üzerinde geliştirilmiş olan bir hizmettir](media/overview/azure-digital-twins-in-iot-ecosystem.png)](media/overview/azure-digital-twins-in-iot-ecosystem.png#lightbox)

IoT hakkında daha fazla bilgi için [Azure IoT teknolojilerini ve çözümlerini](../iot-fundamentals/iot-services-and-technologies.md)okuyun.

## <a name="next-steps"></a>Sonraki adımlar

Azure Digital Twins hakkında kısa bir demoya gidin:

>[!div class="nextstepaction"]
>[Hızlı başlangıç: Azure Digital Twins kullanarak kullanılabilir odaları bulun](./quickstart-view-occupancy-dotnet.md)

Azure Digital Twins'i kullanarak bir tesis yönetimi uygulamasına yakından bakın:

>[!div class="nextstepaction"]
>[Öğretici: Azure Digital Twins'i dağıtma ve uzamsal graf yapılandırma](./tutorial-facilities-setup.md)

Temel Azure Digital Twins kavramları hakkında bilgi edinin:

>[!div class="nextstepaction"]
>[Dijital İkizler nesne modelini ve uzamsal zeka grafiğini anlayın](./concepts-objectmodel-spatialgraph.md)