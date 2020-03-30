---
title: Azure IoT Central'daki müşteri veri isteği özellikleri | Microsoft Dokümanlar
description: Bu makalede, Azure IoT Central uygulamasında müşteri verilerinin tanımlanması, silmesi ve dışa aktarılması açıklanmaktadır.
author: dominicbetts
ms.author: dobett
ms.date: 08/23/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: timlt
ms.openlocfilehash: 3cf88b4d3c4c74493235d2997db4d464bb055b81
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77023761"
---
# <a name="summary-of-customer-data-request-features"></a>Müşteri veri seçimi özelliklerinin özeti

Azure IoT Central, IoT varlıklarınızı ölçekte bağlamayı, izlemeyi ve yönetmeyi, IoT verilerinizden derin öngörüler oluşturmayı ve bilinçli eylemde bulunmayı kolaylaştıran, hizmet olarak yazılım olarak tamamen yönetilen bir Internet (IoT) yazılımıdır.

[!INCLUDE [gdpr-intro-sentence](../../../includes/gdpr-intro-sentence.md)]

## <a name="identifying-customer-data"></a>Müşteri verilerinin tanımlanması

Azure Active Directory Object-DIsi, kullanıcıları tanımlamak ve rolleri atamak için kullanılır. Azure IoT Central portalı rol atamaları için kullanıcı e-posta adreslerini görüntüler, ancak yalnızca Azure Etkin Dizin Nesne Kimliği saklanır, e-posta adresi Azure Active Directory'den dinamik olarak sorgulanır. Azure IoT Merkezi yöneticileri, bir Azure IoT Merkezi uygulamasının kullanıcı yönetimi bölümündeki uygulama kullanıcı kullanıcılarını görüntüleyebilir, dışa aktarabilir ve silebilir.

Uygulama içinde, e-posta adresleri uyarıları almak için yapılandırılabilir. Bu durumda, e-posta adresleri IoT Central içinde saklanır ve uygulama içi hesap yönetimi sayfasından yönetilmelidir.

Cihazlarla ilgili olarak, Microsoft hiçbir bilgi tutmaz ve aygıtın kullanıcı korelasyonuna ulaşmasını sağlayan verilere erişimi yoktur. Azure IoT Central'da yönetilen aygıtların çoğu kişisel aygıtlar değildir, örneğin otomat veya kahve makinesi. Ancak müşteriler, bazı cihazların kişisel olarak tanımlanabilir olduğunu düşünebilir ve kendi takdirlerine bağlı olarak cihazları bireylere bağlayan kendi varlık veya envanter izleme sistemlerini koruyabilirler. Azure IoT Central, cihazlarla ilişkili tüm verileri kişisel veri gibi yönetir ve depolar.

Microsoft kurumsal hizmetlerini kullandığınızda, Microsoft sistem tarafından oluşturulan günlükler olarak bilinen bazı bilgiler oluşturur. Bu günlükler, hizmet içinde gerçekleştirilen olgusal eylemleri ve tek tek aygıtlarla ilgili tanılama verilerini oluşturur ve kullanıcı etkinliğiyle ilgili değildir. Azure IoT Merkezi sistem tarafından oluşturulan günlüklere uygulama yöneticileri tarafından erişilemez veya dışa aktarılamaz.

## <a name="deleting-customer-data"></a>Müşteri verilerini silme

Kullanıcı verilerini silme olanağı yalnızca IoT Merkezi yönetim sayfası üzerinden sağlanır. Uygulama yöneticileri silinecek kullanıcıyı seçebilir ve kaydı silmek için uygulamanın sağ üst köşesindesil'i **seçebilir.** Uygulama yöneticileri, söz konusu uygulamayla artık ilişkili olmayan tek tek hesapları da kaldırabilir.

Bir kullanıcı silindikten sonra, onlara başka uyarı e-postayla gönderilir. Ancak, e-posta adresleri yapılandırılan her uyarıdan ayrı ayrı kaldırılmalıdır.

## <a name="exporting-customer-data"></a>Müşteri verilerini dışa aktarma

Veri ihracı yeteneği yalnızca IoT Merkezi yönetim sayfası üzerinden sağlanır. Atanan roller de dahil olmak üzere müşteri verileri bir uygulama yöneticisi tarafından seçilebilir, kopyalanabilir ve yapıştırılabilir.

## <a name="links-to-additional-documentation"></a>Ek belgelere bağlantılar

Rol tanımları da dahil olmak üzere hesap yönetimi hakkında daha fazla bilgi için [başvurunuzu nasıl yönetisiniz](howto-administer.md)ebakını görün.
