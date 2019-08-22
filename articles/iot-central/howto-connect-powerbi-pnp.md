---
title: Azure IoT Central verilerinizi Power BI panosunda görselleştirin | Microsoft Docs
description: IoT Central verilerinizi görselleştirmek ve analiz etmek için Azure IoT Central Power BI çözümünü kullanın.
ms.service: iot-central
services: iot-central
author: viv-liu
ms.author: viviali
ms.date: 06/09/2019
ms.topic: conceptual
ms.openlocfilehash: dcfabbfe5bf807e8820fa14b6b8234d276578dcc
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/21/2019
ms.locfileid: "69880078"
---
# <a name="visualize-and-analyze-your-azure-iot-central-data-in-a-power-bi-dashboard-preview-features"></a>Azure IoT Central verilerinizi Power BI panosunda görselleştirin ve çözümleyin (Önizleme özellikleri)

[!INCLUDE [iot-central-pnp-original](../../includes/iot-central-pnp-original-note.md)]

*Bu konu, Yöneticiler için geçerlidir.*

![Power BI çözümü işlem hattı](media/howto-connect-powerbi-pnp/iot-continuous-data-export.png)

IoT cihazlarınızın performansını izlemek üzere güçlü bir Power BI panosu oluşturmak için Azure IoT Central Power BI çözümünü kullanın. Power BI panonuzda şunları yapabilirsiniz:
- Cihazların zaman içinde ne kadar veri gönderdiğini izleme
- Telemetri, durumlar ve olaylar arasındaki veri hacmini karşılaştırın
- Çok sayıda ölçümü raporlayan cihazları tanımla
- Cihaz ölçümlerinin geçmiş eğilimlerini gözlemleyin
- Çok sayıda kritik olay gönderen sorunlu cihazları tanımla

Bu çözüm, Azure Blob Depolama hesabınızdaki verileri [sürekli veri dışa aktarma](howto-export-data-blob-storage-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json)işleminden alan işlem hattını ayarlar. Bu veriler, verileri işlemek ve dönüştürmek için Azure Işlevleri, Azure Data Factory ve Azure SQL veritabanı 'na akar. Çıktı, bir PBIX dosyası olarak indirebileceğiniz bir Power BI raporunda görselleştirilir ve analiz edilebilir. Tüm bu kaynaklar Azure aboneliğinizde oluşturulur, böylece her bileşeni gereksinimlerinize uyacak şekilde özelleştirebilirsiniz.

## <a name="get-the-power-bi-solution-for-azure-iot-centralhttpsakamsiotcentralpowerbisolutiontemplate-from-microsoft-appsource"></a>Microsoft AppSource [Azure IoT Central için Power BI çözümünü](https://aka.ms/iotcentralpowerbisolutiontemplate) alın.

## <a name="prerequisites"></a>Önkoşullar
Çözümü ayarlamak için aşağıdakiler gereklidir:
- Bir Azure aboneliğine erişim
- IoT Central uygulamanızdan [sürekli veri dışa aktarma](howto-export-data-blob-storage-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) kullanarak verileri dışarı aktarın. Power BI panosundan en iyi şekilde yararlanmak için ölçümleri, cihazları ve cihaz şablonu akışlarını açmanızı öneririz.
- Power BI Desktop (en son sürüm)
- Power BI Pro (panoyu başkalarıyla paylaşmak istiyorsanız)

## <a name="reports"></a>Raporlar

İki rapor otomatik olarak oluşturulur. 

İlk rapor, cihazlar tarafından raporlanan ölçümlerin geçmiş bir görünümünü gösterir ve en yüksek ölçüm sayısını göndermiş olan farklı ölçüm türlerini ve cihazları ayırır.

![Power BI rapor sayfası 1](media/howto-connect-powerbi-pnp/template-page1-hasdata.PNG)

İkinci rapor olayları daha ayrıntılı bir şekilde gösterir ve bildirilen hataların ve uyarıların geçmiş bir görünümünü gösterir. Ayrıca, hangi cihazların en yüksek sayıda olay olduğunu ve özel olarak hata olayları ve uyarı olaylarını bildirtiğinin de gösterildiği gösterilmektedir.

![Power BI rapor sayfası 2](media/howto-connect-powerbi-pnp/template-page2-hasdata.PNG)

## <a name="architecture"></a>Mimari
Oluşturulan tüm kaynaklara Azure portal erişilebilir. Her şeyin bir kaynak grubu altında olması gerekir.

![Kaynak grubunun Azure Portal görünümü](media/howto-connect-powerbi-pnp/azure-deployment.PNG)

Her bir kaynağın özellikleri ve nasıl kullanıldığı aşağıda açıklanmaktadır.

### <a name="azure-functions"></a>Azure İşlevleri
Blob depolamaya her yeni dosya yazıldığında Azure Işlevi uygulaması tetiklenir. İşlevler her ölçüm, cihaz ve cihaz şablonu dosyasındaki alanları ayıklar ve Azure Data Factory tarafından kullanılacak birkaç ara SQL tablosunu doldurur.

### <a name="azure-data-factory"></a>Azure Data Factory
Azure Data Factory, SQL veritabanına bağlı hizmet olarak bağlanır. Verileri işleyen ve analiz tablolarında depolayan saklı yordam etkinliklerini çalıştırır.

### <a name="azure-sql-database"></a>Azure SQL Database
Bu tablolar varsayılan raporları doldurmak için otomatik olarak oluşturulur. Power BI içinde bu şemaları keşfet ve bu veriler üzerinde kendi görselleştirmelerinizi oluşturabilirsiniz.

| Tablo adı |
|------------|
|[analiz]. Ölçümün|
|[analiz]. Iletilerine|
|[aşama]. Ölçümün|
|[analiz]. Özelliklerinin|
|[analiz]. [PropertyDefinitions]|
|[analiz]. [MeasurementDefinitions]|
|[analiz]. Cihazlarınız|
|[analiz]. [DeviceTemplates]|
|[dbo]. güncel|
|[dbo]. Değişiklik izleme dosyanız|

## <a name="estimated-costs"></a>Tahmini maliyetler

Bu, dahil edilen Azure maliyetlerinin (Azure Işlevi, Data Factory, Azure SQL) tahminidir. Tüm fiyatlar ABD Doları ' dir. Fiyatların bölgeye göre değişeceğini aklınızda bulundurun. bu nedenle, gerçek fiyatları almak için her zaman bireysel hizmetlerin en son fiyatlarını aramanız gerekir.
Şablonda sizin için aşağıdaki varsayılanlar ayarlanır (bir şeyler kurulduktan sonra bunlardan herhangi birini değiştirebilirsiniz):

- Azure Işlevleri: App Service plan S1, $74.40/ay
- Azure SQL S1, ~ $30/ay

Çeşitli fiyatlandırma seçeneklerini öğrenmeniz ve gereksinimlerinize uyacak şekilde ince ayar şeyleri öğrenmeniz önerilir.

## <a name="resources"></a>Kaynaklar

[Azure IoT Central Power BI çözümünü](https://aka.ms/iotcentralpowerbisolutiontemplate)almak Için appsource 'u ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

Artık Power BI verilerinizi görselleştirmeyi öğrendiğinize göre, önerilen sonraki adım aşağıda verilmiştir:

> [!div class="nextstepaction"]
> [Cihazları yönetme](howto-manage-devices-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json)