---
title: Azure DPS aygıtları için müşteri veri isteği özellikleri
description: Azure Aygıt Sağlama Hizmeti'nde (DPS) yönetilen ve kişisel olan aygıtlar için bu makalede, yöneticilere kişisel verilerin nasıl dışa aktarılabildiğini veya sildiğini gösterilmektedir.
author: dominicbetts
ms.author: dobett
ms.date: 05/16/2018
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
ms.openlocfilehash: 1dcf1b9f62f94b8f75ef2fe77f3e237a387c53eb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73890645"
---
# <a name="summary-of-customer-data-request-features"></a>Müşteri veri seçimi özelliklerinin özeti

Azure IoT Hub Aygıt Sağlama Hizmeti, kurumsal müşterileri hedefleyen ve aygıttan başlayıp bulutla biten güvenlikle birlikte Azure IoT Hub'ına cihazların sorunsuz, otomatik sıfır dokunma sağlamasını sağlayan REST API tabanlı bir bulut hizmetidir.

[!INCLUDE [gdpr-intro-sentence](../../includes/gdpr-intro-sentence.md)]

Tek tek cihazlara bir kiracı yönetici tarafından bir kayıt kimliği ve aygıt kimliği atanır. Bu aygıtlardan gelen ve bu aygıtlar hakkındaki veriler bu t'lere dayanır. Microsoft hiçbir bilgi tutmaz ve bu aygıtların bir kişiyle ilişkikurmasını sağlayacak verilere erişimi yoktur.

Cihaz Sağlama Hizmeti'nde yönetilen cihazların çoğu kişisel cihazlar değildir, örneğin ofis termostatı veya fabrika robotu. Ancak müşteriler, bazı cihazların kişisel olarak tanımlanabilir olduğunu düşünebilir ve kendi takdirlerine bağlı olarak cihazları bireylere bağlayan kendi varlık larını veya envanter izleme yöntemlerini koruyabilirler. Aygıt Sağlama Hizmeti, cihazlarla ilişkili tüm verileri kişisel veri gibi yönetir ve depolar.

Kiracı yöneticiler, bir aygıt kimliği veya kayıt kimliğiyle ilişkili verileri dışa aktararak veya silerek bilgi isteklerini yerine getirmek için Azure portalını veya hizmetin REST API'lerini kullanabilir.

> [!NOTE]
> Aygıt Sağlama Hizmeti aracılığıyla Azure IoT Hub'ında sağlanan aygıtlar, Azure IoT Hub hizmetinde depolanan ek verilere sahiptir. Belirli bir aygıt için tam isteği tamamlamak için [Azure IoT Hub başvuru belgelerine](../iot-hub/iot-hub-customer-data-requests.md) bakın.

## <a name="deleting-customer-data"></a>Müşteri verilerini silme

Cihaz Sağlama Hizmeti, kayıtları ve kayıt kayıtlarını saklar. Kayıtlar, sağlanmasına izin verilen aygıtlar hakkında bilgi içerir ve kayıt kayıtları hangi aygıtların zaten sağlama sürecinden geçtiğini gösterir.

Kiracı yöneticiler kayıtları Azure portalından kaldırabilir ve bu da ilişkili kayıt kayıtlarını kaldırır.

Daha fazla bilgi için [aygıt kayıtlarını nasıl yönetebilirsiniz'e](how-to-manage-enrollments.md)bakın.

REST API'lerini kullanarak kayıtlar ve kayıt kayıtları için silme işlemleri gerçekleştirmek de mümkündür:

* Tek bir cihazın kayıt bilgilerini silmek için [Aygıt Kaydı - Sil](/rest/api/iot-dps/deleteindividualenrollment/deleteindividualenrollment)' i kullanabilirsiniz.
* Bir aygıt grubunun kayıt bilgilerini silmek için [Aygıt Kayıt Grubu - Sil](/rest/api/iot-dps/deleteenrollmentgroup/deleteenrollmentgroup)' i kullanabilirsiniz.
* Sağlanan aygıtlar hakkındaki bilgileri silmek için [Kayıt Durumu - Kayıt Durumunu Sil'i](/rest/api/iot-dps/deletedeviceregistrationstate/deletedeviceregistrationstate)kullanabilirsiniz.

## <a name="exporting-customer-data"></a>Müşteri verilerini dışa aktarma

Cihaz Sağlama Hizmeti, kayıtları ve kayıt kayıtlarını saklar. Kayıtlar, sağlanmasına izin verilen aygıtlar hakkında bilgi içerir ve kayıt kayıtları hangi aygıtların zaten sağlama sürecinden geçtiğini gösterir.

Kiracı yöneticiler kayıtları ve kayıt kayıtlarını Azure portalı üzerinden görüntüleyebilir ve kopyala ve yapıştırarak dışa aktarabilir.

Kayıtları yönetme hakkında daha fazla bilgi için [aygıt kayıtlarını nasıl yönetireceksiniz](how-to-manage-enrollments.md)bilgisine bakın.

Ayrıca, REST API'lerini kullanarak kayıtlar ve kayıt kayıtları için ihracat işlemleri gerçekleştirmek de mümkündür:

* Tek bir cihaz için kayıt bilgilerini dışa aktarmak için [Aygıt Kaydı '](/rest/api/iot-dps/getindividualenrollment/getindividualenrollment)nı kullanabilirsiniz - Get .
* Bir aygıt grubu için kayıt bilgilerini dışa aktarmak için [Aygıt Kayıt Grubu '](/rest/api/iot-dps/getenrollmentgroup/getenrollmentgroup)ni kullanabilirsiniz - Get .
* Daha önce verilmiş olan aygıtlar hakkında bilgi aktarmak için [Kayıt Durumu - Kayıt Durumu Al'ı](/rest/api/iot-dps/getdeviceregistrationstate/getdeviceregistrationstate)kullanabilirsiniz.

> [!NOTE]
> Microsoft'un kurumsal hizmetlerini kullandığınızda, Microsoft sistem tarafından oluşturulan günlükler olarak bilinen bazı bilgiler oluşturur. Bazı Aygıt Sağlama Hizmeti sistemi tarafından oluşturulan günlükleri kiracı yöneticiler tarafından erişilebilir veya dışa aktarılamaz. Bu günlükler, hizmet içinde gerçekleştirilen olgusal eylemleri ve tek tek aygıtlarla ilgili tanılama verilerini oluşturur.

## <a name="links-to-additional-documentation"></a>Ek belgelere bağlantılar

Cihaz Sağlama Hizmeti API'leri için [https://docs.microsoft.com/rest/api/iot-dps](https://docs.microsoft.com/rest/api/iot-dps)tam dokümantasyon .

Azure IoT Hub [müşteri veri sevesi özellikleri.](../iot-hub/iot-hub-customer-data-requests.md)