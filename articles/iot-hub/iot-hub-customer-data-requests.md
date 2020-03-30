---
title: Azure IoT Hub aygıtları için müşteri veri istekleri
description: Azure IoT Hub'da yönetilen aygıtların çoğu kişisel değildir, ancak bazıları kişiseldir. Bu makalede, yöneticilerin bir aygıttan kişisel verileri dışa aktarabilme veya silebilme hakkında konuşmalar yer almak.
author: robinsh
ms.author: robinsh
ms.date: 05/16/2018
ms.topic: conceptual
ms.service: iot-hub
services: iot-hub
ms.openlocfilehash: a05fbf6e1908f88014cd8da99fafb875de033f45
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79499322"
---
# <a name="summary-of-customer-data-request-features"></a>Müşteri veri seçimi özelliklerinin özeti

Azure IoT Hub, milyonlarca cihaz ve bölümlenmiş Azure hizmeti arasında güvenli, çift yönlü iletişim sağlayan kurumsal müşterileri hedefleyen REST API tabanlı bir bulut hizmetidir.

[!INCLUDE [gdpr-related guidance](../../includes/gdpr-intro-sentence.md)]

Tek tek aygıtlar, kiracı bir yönetici tarafından bir aygıt tanımlayıcısı (aygıt kimliği) atanır. Aygıt verileri atanan aygıt kimliğine dayanır. Microsoft hiçbir bilgi tutmaz ve aygıt kimliğinin kullanıcı bağındırıntısı için izin verecek verilere erişimi yoktur.

Azure IoT Hub'da yönetilen cihazların çoğu kişisel aygıtlar değildir, örneğin ofis termostatı veya fabrika robotu. Ancak müşteriler, bazı cihazların kişisel olarak tanımlanabilir olduğunu düşünebilir ve kendi takdirlerine bağlı olarak cihazları bireylere bağlayan kendi varlık larını veya envanter izleme yöntemlerini koruyabilirler. Azure IoT Hub, cihazlarla ilişkili tüm verileri kişisel veri gibi yönetir ve depolar.

Kiracı yöneticiler, bir aygıt kimliğiyle ilişkili verileri dışa aktararak veya silerek bilgi isteklerini yerine getirmek için Azure portalını veya hizmetin REST API'lerini kullanabilir.

Aygıt iletilerini diğer hizmetlere iletmek için Azure IoT Hub hizmetinin yönlendirme özelliğini kullanıyorsanız, belirli bir aygıt için tam isteği tamamlamak için her yönlendirme bitiş noktası için veri isteklerinin kiracı yönetici tarafından gerçekleştirilmesi gerekir. Daha fazla ayrıntı için her bitiş noktası için başvuru belgelerine bakın. Desteklenen uç noktalar hakkında daha fazla bilgi için [Başvuru - IoT Hub uç noktalarına](iot-hub-devguide-endpoints.md)bakın.

Azure IoT Hub hizmetinin Azure Olay Ağı tümleştirme özelliğini kullanıyorsanız, bu olayların her bir abonesi için veri isteklerinin kiracı yönetici tarafından gerçekleştirilmesi gerekir. Daha fazla bilgi için [Olay Grid'i kullanarak IoT Hub olaylarına tepki](iot-hub-event-grid.md)verme konusuna bakın.

Tanılama günlükleri oluşturmak için Azure IoT Hub hizmetinin Azure Monitor tümleştirme özelliğini kullanıyorsanız, veri isteklerinin kiracı yönetici tarafından depolanan günlüklere karşı gerçekleştirilmesi gerekir. Daha fazla bilgi için Azure [IoT Hub'ının sistem durumunu izleyin'](iot-hub-monitor-resource-health.md)e bakın.

## <a name="deleting-customer-data"></a>Müşteri verilerini silme

Kiracı yöneticiler, Söz sözle ilişkili verileri silen bir aygıtı silmek için Azure portalındaki Azure IoT Hub uzantısının IoT aygıtları bıçaklarını kullanabilir.

REST API'leri kullanan aygıtlar için silme işlemleri gerçekleştirmek de mümkündür. Daha fazla bilgi için [Hizmet - Cihazı Sil'](/rest/api/iothub/service/registrymanager/deletedevice)e bakın.

## <a name="exporting-customer-data"></a>Müşteri verilerini dışa aktarma

Kiracı yöneticiler, bir aygıtla ilişkili verileri dışa aktarmak için Azure portalındaki Azure IoT Hub uzantısının IoT aygıtlar bölmesinde kopyalayıp yapıştırabilir.

REST API'leri kullanan aygıtlar için dışa aktarma işlemleri gerçekleştirmek de mümkündür. Daha fazla bilgi için [Servis - Cihazı Al'](/rest/api/iothub/service/registrymanager/getdevice)a bakın.

> [!NOTE]
> Microsoft'un kurumsal hizmetlerini kullandığınızda, Microsoft sistem tarafından oluşturulan günlükler olarak bilinen bazı bilgiler oluşturur. Bazı Azure IoT Hub sistem tarafından oluşturulan günlüklere kiracı yöneticiler tarafından erişilemez veya dışa aktarılamaz. Bu günlükler, hizmet içinde gerçekleştirilen olgusal eylemleri ve tek tek aygıtlarla ilgili tanılama verilerini oluşturur.

## <a name="links-to-additional-documentation"></a>Ek belgelere bağlantılar

Azure IoT Hub Hizmeti API'leri için tam dokümantasyon [IoT Hub Hizmeti API'larında](https://docs.microsoft.com/rest/api/iothub/service/configuration)bulunur.
