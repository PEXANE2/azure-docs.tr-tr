---
title: Azure Akışı Analizi önizleme özellikleri
description: Bu makalede, şu anda önizlemede olan Azure Akış Analizi özellikleri listelenir.
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 3/9/2020
ms.openlocfilehash: 4c265665be26dcc6868ea9a303b0c12c52dfe05b
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80878315"
---
# <a name="azure-stream-analytics-preview-features"></a>Azure Akışı Analizi önizleme özellikleri

Bu makalede, Şu anda Azure Akış Analizi için önizlemede olan tüm özellikler özetlenmiştir. Önizleme özelliklerinin üretim ortamında kullanılması önerilmez.

## <a name="public-previews"></a>Genel önizlemeler

Aşağıdaki özellikler genel önizlemede yer alıyor. Bu özelliklerden bugün yararlanabilirsiniz, ancak bunları üretim ortamınızda kullanmayın.

### <a name="online-scaling"></a>Çevrimiçi ölçekleme

Çevrimiçi ölçekleme ile, SU tahsisatını değiştirmeniz gerekiyorsa işinizi durdurmanız gerekmez. Çalışan bir işin SU kapasitesini durdurmak zorunda kalmadan artırabilir veya azaltabilirsiniz. Bu, Stream Analytics'in bugün sunduğu uzun soluklu görev açısından kritik boru hatlarının müşteri vaadine dayanmaktadır. Daha fazla bilgi için [bkz.](stream-analytics-streaming-unit-consumption.md#configure-stream-analytics-streaming-units-sus)

### <a name="c-custom-de-serializers"></a>C# özel de-serializers
Geliştiriciler, Protobuf, XML veya herhangi bir özel biçimde verileri işlemek için Azure Akış Analizi'nin gücünden yararlanabilir. Azure Akış Analizi tarafından alınan olayları seri olmaktan çıkarmak için kullanılabilecek [Özel Seri İzleyicileri](custom-deserializer-examples.md) C#'da uygulayabilirsiniz.

### <a name="extensibility-with-c-custom-code"></a>C# özel kodu ile genişletilebilirlik

Bulutta veya IoT Edge'de Stream Analytics modülleri oluşturan geliştiriciler özel C# işlevlerini yazabilir veya yeniden kullanabilir ve [bunları kullanıcı tarafından tanımlanan işlevler](stream-analytics-edge-csharp-udf-methods.md)aracılığıyla doğrudan sorguda çağırabilir.


### <a name="debug-query-steps-in-visual-studio"></a>Visual Studio'da hata ayıklama sorgu adımları

Visual Studio için Azure Akış Analizi araçlarında yerel test yaparken veri diyagramında ayarlanan ara satırı kolayca önizleyebilirsiniz. 

### <a name="local-testing-with-live-data-in-visual-studio-code"></a>Visual Studio Code'da canlı verilerle yerel test

İşi Azure'a göndermeden önce sorgularınızı yerel makinenizdeki canlı verilere karşı test edebilirsiniz. Her test yinelemesi ortalama 2-3 saniyeden az sürer ve bu da çok verimli bir geliştirme süreciyle sonuçlanır.

### <a name="visual-studio-code-for-azure-stream-analytics"></a>Azure Akış Analizi için Görsel Stüdyo Kodu

Azure Akış Analizi işleri Visual Studio Code'da yazılabilir. VS [Kodumuz öğretici başlarken](https://docs.microsoft.com/azure/stream-analytics/quick-create-vs-code)bakın.


### <a name="real-time-high-performance-scoring-with-custom-ml-models-managed-by-azure-machine-learning"></a>Azure Machine Learning tarafından yönetilen özel ML modelleri ile gerçek zamanlı yüksek performans puanlama

Azure Akış Analizi, Azure Machine Learning tarafından yönetilen ve kod yazmanızı gerektirmeyen bir iş akışını kullanarak Azure Kubernetes Hizmeti (AKS) veya Azure Kapsayıcı Örnekleri'nde (ACI) barındırılan özel önceden eğitilmiş Machine Learning modellerinden yararlanarak yüksek performanslı, gerçek zamanlı puanlamayı destekler. Önizleme için [kaydolun](https://aka.ms/asapreview1)


### <a name="live-data-testing-in-visual-studio"></a>Visual Studio'da canlı veri testi

Azure Akış Analitiği için Visual Studio araçları, sizi Event Hub veya IoT hub gibi bulut kaynaklarından gelen canlı etkinlik akışlarına karşı sorgularınızı test etmenizi sağlayan yerel test özelliğini geliştirir. [Visual Studio için Azure Akış Analizi araçlarını kullanarak canlı verileri yerel olarak](stream-analytics-live-data-local-testing.md)nasıl test edebilirsiniz öğrenin.


### <a name="net-user-defined-functions-on-iot-edge"></a>.NET kullanıcı tanımlı fonksiyonlar IoT Edge

.NET standart kullanıcı tanımlı işlevlerle, akış ardınızın bir parçası olarak .NET Standart kodunu çalıştırabilirsiniz. Basit C# sınıfları oluşturabilir veya tam proje ve kitaplıklar içe aktarabilirsiniz. Visual Studio'da tam yazma ve hata ayıklama deneyimi desteklenir. Daha fazla bilgi için [Azure Akış Analitiği Kenarı işleri için Gelişmiş .NET Standart kullanıcı tanımlı işlevleri](stream-analytics-edge-csharp-udf-methods.md)ni ziyaret edin.

## <a name="other-previews"></a>Diğer önizlemeler

Aşağıdaki özellikler istek üzerine önizlemede de kullanılabilir.

### <a name="support-for-azure-stack"></a>Azure Yığını desteği
Azure IoT Edge çalışma zamanında etkinleştirilen bu özellik, Azure Yığını'nda çalışan yerel giriş ve çıktılar için yerel destek (örneğin Olay Hub'ları, IoT Hub'ı, Blob Depolama) gibi özel Azure Yığını özelliklerinden yararlanır. Bu yeni tümleştirme, verilerinizi oluşturulduğu yere yakın analiz ederek gecikmeyi azaltan ve öngörüleri en üst düzeye çıkaran karma mimariler oluşturmanıza olanak tanır.
Bu özellik, verilerinizi oluşturulduğu yere yakın analiz ederek gecikmeyi azaltan ve öngörüleri en üst düzeye çıkarabilen karma mimariler oluşturmanıza olanak tanır. Bu önizleme için [kaydolmalısınız.](https://aka.ms/asapreview1)
