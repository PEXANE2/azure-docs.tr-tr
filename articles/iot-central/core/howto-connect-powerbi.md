---
title: Azure IoT Central verilerinizi Power BI panosunda görselleştirin | Microsoft Docs
description: IoT Central verilerinizi görselleştirmek ve analiz etmek için Azure IoT Central Power BI çözümünü kullanın.
ms.service: iot-central
services: iot-central
author: viv-liu
ms.author: viviali
ms.date: 10/4/2019
ms.topic: conceptual
ms.openlocfilehash: a484ab2553aeefbbe8c03ae639bdd667e4d9cb8e
ms.sourcegitcommit: 5a8c8ac84c36859611158892422fc66395f808dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/10/2020
ms.locfileid: "84661197"
---
# <a name="visualize-and-analyze-your-azure-iot-central-data-in-a-power-bi-dashboard"></a>Azure IoT Central verilerinizi Power BI panosunda görselleştirin ve çözümleyin

*Bu konu, Yöneticiler ve çözüm geliştiricileri için geçerlidir.*

:::image type="content" source="media/howto-connect-powerbi/iot-continuous-data-export.png" alt-text="Power BI çözümü işlem hattı":::

IoT cihazlarınızın performansını izlemek üzere güçlü bir Power BI panosu oluşturmak için Azure IoT Central Power BI çözümünü kullanın. Power BI panonuzda şunları yapabilirsiniz:

- Cihazların zaman içinde ne kadar veri gönderdiğini izleme
- Farklı telemetri akışları arasında veri birimlerini karşılaştırın
- Belirli cihazlar tarafından gönderilen verilere filtre uygula
- Bir tabloda en son telemetri verilerini görüntüleme

Bu çözüm, [sürekli veri dışa aktarma](howto-export-data-blob-storage.md) Azure Blob Storage hesabınızdan verileri okuyan bir işlem hattı oluşturur. İşlem hattı, verileri işlemek ve dönüştürmek için Azure Işlevleri, Azure Data Factory ve Azure SQL veritabanı 'nı kullanır. verileri, bir PBIX dosyası olarak yüklediğiniz bir Power BI raporunda görselleştirebilir ve çözümleyebilirsiniz. Tüm kaynaklar Azure aboneliğinizde oluşturulur, böylece her bileşeni gereksinimlerinize uyacak şekilde özelleştirebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

Bu nasıl yapılır kılavuzundaki adımları tamamlayabilmeniz için etkin bir Azure aboneliğine ihtiyacınız vardır. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

Çözümü ayarlamak için aşağıdaki kaynaklar gereklidir:

- IoT Central uygulama. Daha fazla bilgi için bkz. [Azure IoT Central uygulaması oluşturma](./quick-deploy-iot-central.md).
- Telemetriyi, cihazları ve cihaz şablonlarını Azure Blob depolamaya aktarmak için sürekli veri dışa aktarma yapılandırıldı. Daha fazla bilgi edinmek için bkz. [Azure 'Da IoT verilerini hedeflere aktarma](howto-export-data.md).
  - Yalnızca IoT Central uygulamanızın blob kapsayıcısına veri dışarı aktarıldığınızdan emin olun.
  - [CIHAZLARıNıZıN JSON kodlu iletiler gönderilmesi gerekir](../../iot-hub/iot-hub-devguide-messages-d2c.md). Cihazların `contentType:application/JSON` `contentEncoding:utf-8` `contentEncoding:utf-16` `contentEncoding:utf-32` ileti sistemi özelliklerinde ve veya ya da belirtmesi gerekir.
- Power BI Desktop (en son sürüm). Bkz. [Power BI İndirmeleri](https://powerbi.microsoft.com/downloads/).
- Power BI Pro (panoyu başkalarıyla paylaşmak istiyorsanız).

## <a name="install"></a>Yükleme

İşlem hattını ayarlamak için **Microsoft AppSource** sitesindeki [Azure IoT Central Power BI çözümü](https://appsource.microsoft.com/product/web-apps/iot-central.power-bi-solution-iot-central) sayfasına gidin. **Şimdi al**' ı seçin ve yönergeleri izleyin.

PBIX dosyasını açtığınızda, sayfayı okuyun ve kapak sayfasındaki yönergeleri izleyin. Bu yönergeler, raporunuzu SQL veritabanınıza nasıl bağlayacağınız açıklanır.

## <a name="report"></a>Rapor

PBIX dosyasında **cihazlar ve telemetri** raporu bulunur ve cihazlar tarafından gönderilen telemetrinin geçmiş bir görünümünü gösterir. Farklı telemetri türlerinin bir dökümünü sağlar ve ayrıca cihazlar tarafından gönderilen en son Telemetriyi gösterir.

:::image type="content" source="media/howto-connect-powerbi/report.png" alt-text="Power BI cihazlar ve telemetri raporu":::

## <a name="pipeline-resources"></a>İşlem hattı kaynakları

Azure portal işlem hattını oluşturan tüm Azure kaynaklarına erişebilirsiniz. Tüm kaynaklar, işlem hattını ayarlarken oluşturduğunuz kaynak grubunda bulunur.

:::image type="content" source="media/howto-connect-powerbi/azure-deployment.png" alt-text="Kaynak grubunun Azure portal görünümü":::

Aşağıdaki listede, ardışık düzendeki her bir kaynağın rolü açıklanmaktadır:

### <a name="azure-functions"></a>Azure İşlevleri

Azure Işlevi uygulaması, BLOB depolama alanına yeni bir dosya yazdığında IoT Central her seferinde tetiklenir. İşlevler, Azure Data Factory kullandığı ara SQL tablolarını doldurmak için telemetri, cihaz ve cihaz şablonları Bloblarından veri ayıklar.

### <a name="azure-data-factory"></a>Azure Data Factory

Azure Data Factory, SQL veritabanına bağlı hizmet olarak bağlanır. Verileri işlemek ve analiz tablolarında depolamak için saklı yordamları çalıştırır.

Azure Data Factory, en son veri toplu iş kümesini SQL tablolarına (örneğin, atlayan **pencere tetikleyicisi**için en az sayı) yüklenecek şekilde dönüştürmek üzere 15 dakikada bir çalışır.

### <a name="azure-sql-database"></a>Azure SQL Veritabanı

Azure Data Factory, Power BI için bir dizi analiz tablosu oluşturur. Bu şemaları Power BI araştırabilir ve kendi görselleştirmelerinizi oluşturmak için kullanabilirsiniz.

## <a name="estimated-costs"></a>Tahmini maliyetler

Microsoft AppSource sitesindeki [Azure IoT Central için Power BI çözümü](https://appsource.microsoft.com/product/web-apps/iot-central.power-bi-solution-iot-central) , dağıttığınız kaynaklar için bir maliyet tahmin aracı bağlantısı içerir.

## <a name="next-steps"></a>Sonraki adımlar

Artık Power BI verilerinizi görselleştirmeyi öğrendiğinize göre, önerilen sonraki adım [cihazların nasıl yönetileceğini](howto-manage-devices.md)öğrenmektir.
