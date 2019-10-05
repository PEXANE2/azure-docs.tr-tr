---
title: Azure dijital TWINS 'e genel bakış | Microsoft Docs
description: Uzamsal zeka için Azure IoT çözümü olan Azure dijital TWINS hakkında daha fazla bilgi edinin.
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.date: 09/17/2019
ms.topic: overview
ms.service: digital-twins
services: digital-twins
ms.custom: mvc
ms.openlocfilehash: 2194afe4784d1b59dc5fd5628e3a9223d13f92be
ms.sourcegitcommit: 4f7dce56b6e3e3c901ce91115e0c8b7aab26fb72
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2019
ms.locfileid: "71949298"
---
# <a name="overview-of-azure-digital-twins"></a>Azure dijital TWINS 'e genel bakış

Azure dijital TWINS önizlemesi, fiziksel ortamın kapsamlı modellerini oluşturan bir Azure IoT hizmetidir. Kişiler, boşluklar ve cihazlar arasındaki ilişkileri ve etkileşimleri modellemek için uzamsal zeka grafikleri oluşturabilir.

Azure dijital TWINS sayesinde, birçok farklı sensör yerine fiziksel bir alandan verileri sorgulayabilirsiniz. Bu hizmet, akış verilerini dijital ve fiziksel bir dünya genelinde bağlayan, yeniden kullanılabilir, yüksek düzeyde ölçeklenebilir, istenmeyen ve dağınık bir deneyim oluşturmanıza yardımcı olur. Uygulamalarınız bu benzersiz ilgili bağlamsal Özellikler tarafından geliştirilmiştir. 

Azure dijital TWINS, ambarlar, ofisler, okullar, hastaneler ve bankalar gibi tüm ortam türleri için geçerlidir. Bu, Stadiums, fabrika, Park lotları, Park, Akıllı Kılavuzlar ve şehirlerde bile kullanılabilir. Azure dijital TWINS 'in yararlı olabilecek bazı senaryolar aşağıda verilmiştir:

- Bir fabrika için bakım ihtiyaçlarını tahmin edin.
- Elektrik Kılavuzu için gerçek zamanlı enerji gereksinimlerini çözümleyin.
- Bir ofis için kullanılabilir alan kullanımını iyileştirin.
- Çeşitli durumlarda günlük sıcaklığını izleyin.
- Meşgul drone yollarını izleyin.
- Otonom Araçlar 'ı tanımla.
- Bir bina için doluluk düzeylerini analiz edin.
- Deponuzdaki en zorlu nakit kaydını bulun.

Gerçek dünyada iş senaryolarınız ne olursa olsun, buna karşılık gelen dijital bir örnek Azure dijital TWINS aracılığıyla sağlanabilir.

Aşağıdaki videoda Azure dijital TWINS 'e daha yakından göz atalım.

> [!VIDEO https://www.youtube.com/embed/TvN_NxpgyzQ]

## <a name="key-capabilities"></a>Anahtar özellikleri

Azure dijital TWINS 'in aşağıdaki önemli özellikleri vardır.

### <a name="spatial-intelligence-graph"></a>Uzamsal zeka grafiği

[*Uzamsal zeka grafiği*](./concepts-objectmodel-spatialgraph.md#spatial-intelligence-graph)veya *uzamsal grafik*, fiziksel ortamın sanal bir gösterimidir. Kişiler, Yerlerim ve cihazlar arasındaki ilişkileri modellemek için kullanabilirsiniz.

Bir komşuları genelinde bağlanmış birkaç elektrik kullanımı ölçümünü içeren bir akıllı yardımcı program uygulaması düşünün. Akıllı yardımcı program şirketi, elektrik kullanımını ve faturalandırmayı doğru şekilde izleyip tahmin etmelidir. Her bir cihaz ve algılayıcı, Faturalanacak konum ve müşteri hakkındaki bağlamla modellenmelidir. Bu karmaşık ilişki türlerini modellemek için uzamsal zeka grafiğini kullanabilirsiniz.

### <a name="digital-twin-object-models"></a>Dijital ikizi nesne modelleri

[Dijital ikizi nesne modelleri](./concepts-objectmodel-spatialgraph.md#digital-twins-object-models) önceden tanımlanmış cihaz protokolleri ve veri şemadır. Bu kişiler, uygulamanızın etki alanına özgü ihtiyaçlarını geliştirmeyi hızlandırmaya ve basitleştirmeye yönelik olarak hizalar.

Örneğin, bir oda doluluk uygulaması kampüs, bina, kat ve oda gibi önceden tanımlanmış alan türlerini kullanabilir.

### <a name="multiple-and-nested-tenants"></a>Birden çok ve iç içe kiracı

Güvenli bir şekilde ölçeklenen ve birden çok kiracı için yeniden kullanılabilecek çözümler oluşturabilirsiniz. Ayrıca, yalıtılmış ve güvenli bir şekilde erişilebilen ve kullanılabilecek birden fazla alt kiracı oluşturabilirsiniz.

Bir örnek, kiracının verilerini tek bir bina içindeki verileri diğer kiracılardan yalıtmak üzere yapılandırılmış bir alan kullanım uygulamasıdır. Ya da uygulama, çok sayıda binasıyla tek bir kiracıya yönelik verileri birleştirmek için kullanılır.

### <a name="advanced-compute-capabilities"></a>Gelişmiş işlem özellikleri

[Kullanıcı tanımlı işlevlerle](./concepts-user-defined-functions.md), önceden tanımlanmış uç noktalara sinyaller göndermek için gelen [cihaz verileriyle](./concepts-device-ingress.md) özel işlevler tanımlayabilir ve çalıştırabilirsiniz. Bu gelişmiş özellik, cihaz görevlerinin özelleştirmesini ve otomasyonunu geliştirir.

Bu örnek, SOIL nemi algılayıcı okumaları ve hava durumu tahminini değerlendirmek için Kullanıcı tanımlı bir işlev içeren akıllı bir tarım uygulamasıdır. Uygulama daha sonra, sulama ihtiyaçları hakkında sinyaller gönderir.

### <a name="built-in-access-control"></a>Yerleşik erişim denetimi

[Rol tabanlı erişim denetimi](./security-role-based-access-control.md) ve [Azure Active Directory](./security-authenticating-apis.md)gibi erişim ve kimlik yönetimi özelliklerini kullanarak bireyler ve cihazlar için erişimi güvenli bir şekilde denetleyebilirsiniz.

Örnek, bir odanın, belirtilen bir aralıktaki sıcaklığın belirtilmesine izin verecek şekilde yapılandırılmış bir tesis yönetimi uygulamasıdır. Tesis yöneticileri, herhangi bir odada bulunan sıcaklığı herhangi bir değere ayarlamanıza izin verilir.

### <a name="ecosystem"></a>Ekosistem

Azure dijital TWINS örneğini birçok güçlü Azure hizmetine bağlayabilirsiniz. Bu hizmetler Azure Stream Analytics, Azure AI ve Azure depolama 'yı içerir. Azure haritalar, Microsoft Mixed Reality, Dynamics 365 veya Office 365 de bulunur.

Birçok katlarda bulunan ekipleri ve cihazları göstermek için Azure dijital TWINS kullanan bir akıllı ofis oluşturma uygulamasıdır. Cihazlar, sağlanan dijital Ikizi örneğine canlı veri akışı yaparken, bu verileri uygulanabilir temel Öngörüler sağlamak için Stream Analytics işler. Veriler Azure Storage 'da depolanır ve paylaşılabilir bir dosya biçimine dönüştürülür. Dosya, Office 365 kullanılarak tüm kuruluş genelinde dağıtılır.

## <a name="solutions-that-benefit-from-azure-digital-twins"></a>Azure dijital TWINS 'den faydalanabilir çözümler

Azure dijital TWINS, fiziksel dünyanın ve çok sayıda ilişkilerinin temsil edilmesi için yararlıdır. IoT modelleme, veri işleme, olay işleme ve cihaz izlemeyi basitleştirir. Birkaç sektörde aşağıdaki senaryolardan yalnızca birkaçını göz önünde bulundurun. Kullanım avantajlarından faydalanır:

* Bir özellik Yönetimi şirketini, Office oluşturmayı yapılandırmanın en iyi yolları hakkında bilgi sahibi olmak için zaman içinde bir alanın doluluk düzeylerini gösterir.
* Bir mobil uygulama için iş emri biletlerini tetikleyin. Bu hizmeti kullanarak, bir perakende alanında veya spor alanında güvenlik koruyucuları ve diğer hizmetleri zamanlayın.
* Gerçek zamanlı olarak bir binada yer aldığı bir bina işant 'yi gösterir. Daha sonra, işant 'nin ihtiyaçlarına uygun çalışma alanlarını ayırmalarına yardımcı olun.
* Varlıkların bir boşluk içinde bulunduğu yeri izleyin.
* Kullanıcı tercihleri ve enerji Kılavuzu kısıtlamalarını modelleyerek elektrik araç şarjını iyileştirin.

## <a name="azure-digital-twins-in-the-context-of-other-iot-services"></a>Diğer IoT Hizmetleri bağlamında Azure dijital TWINS

Azure dijital TWINS, her şeyi fiziksel dünyanın güncel kalmasını sağlayan IoT cihazlarını ve Sensörlerinizi bağlamak için Azure IoT Hub kullanır. Aşağıdaki diyagramda Azure Digital TWINS 'in diğer Azure IoT hizmetleriyle nasıl ilişkili olduğu gösterilmektedir.

[![Azure dijital TWINS, Azure IoT Hub üzerinde oluşturulmuş bir hizmettir](media/overview/azure-digital-twins-in-iot-ecosystem.png)](media/overview/azure-digital-twins-in-iot-ecosystem.png#lightbox)

IoT hakkında daha fazla bilgi için bkz. [Azure IoT teknolojileri ve çözümleri](../iot-fundamentals/iot-services-and-technologies.md).

## <a name="next-steps"></a>Sonraki adımlar

Azure dijital TWINS hakkında kısa bir tanıtım 'e gidin:

>[!div class="nextstepaction"]
>[Hızlı başlangıç: Azure dijital TWINS kullanarak kullanılabilir odaları bulma](./quickstart-view-occupancy-dotnet.md)

Azure dijital TWINS kullanarak bir tesis yönetimi uygulamasına yakından bakış:

>[!div class="nextstepaction"]
>[Öğretici: Azure dijital TWINS dağıtma ve uzamsal bir grafik yapılandırma](./tutorial-facilities-setup.md)

Çekirdek Azure dijital TWINS kavramları hakkında bilgi edinin:

>[!div class="nextstepaction"]
>[Dijital TWINS nesne modelini ve uzamsal zeka grafiğini anlayın](./concepts-objectmodel-spatialgraph.md)