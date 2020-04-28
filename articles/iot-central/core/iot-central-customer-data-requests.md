---
title: Azure IoT Central müşteri verileri isteği özellikleri | Microsoft Docs
description: Bu makalede, Azure IoT Central uygulamasındaki müşteri verilerini tanımlama, silme ve dışarı aktarma işlemi açıklanmaktadır.
author: dominicbetts
ms.author: dobett
ms.date: 08/23/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: timlt
ms.openlocfilehash: 3cf88b4d3c4c74493235d2997db4d464bb055b81
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77023761"
---
# <a name="summary-of-customer-data-request-features"></a>Müşteri verileri istek özelliklerinin Özeti

Azure IoT Central, IoT varlıklarınızı ölçeklendirerek kolayca bağlamayı, izlemenizi ve yönetmenizi, IoT verilerinizden ayrıntılı Öngörüler oluşturmayı ve bilinçli bir işlem yapmayı kolaylaştıran, tam olarak Nesnelerin İnterneti yönetilen bir hizmet olarak yazılım çözümüdür.

[!INCLUDE [gdpr-intro-sentence](../../../includes/gdpr-intro-sentence.md)]

## <a name="identifying-customer-data"></a>Müşteri verilerini tanımlama

Azure Active Directory nesne kimlikleri, kullanıcıları tanımlamak ve rolleri atamak için kullanılır. Azure IoT Central portalı, rol atamaları için kullanıcı e-posta adreslerini görüntüler, ancak yalnızca Azure Active Directory nesne KIMLIĞI depolanır, e-posta adresi Azure Active Directory tarafından dinamik olarak sorgulanır. Azure IoT Central yöneticileri, Azure IoT Central uygulamasının Kullanıcı Yönetimi bölümünde uygulama kullanıcılarını görüntüleyebilir, dışarı aktarabilir ve silebilir.

Uygulama içinde e-posta adresleri, uyarıları alacak şekilde yapılandırılabilir. Bu durumda, e-posta adresleri IoT Central içinde depolanır ve uygulama içi hesap yönetimi sayfasından yönetilmelidir.

Microsoft, cihazlarla ilgili bilgi içermez ve cihazın Kullanıcı bağıntısını sağlayan verilere erişimi yoktur. Azure IoT Central 'de yönetilen cihazların birçoğu, bir havalandırma makinesi veya kahve Oluşturucu gibi kişisel cihazlar değildir. Ancak müşteriler, bazı cihazları kişisel olarak tanımlanabilir ve bu yandan cihazları kişilere bağlayan kendi varlığını veya envanter izleme sistemlerini de koruyabilir. Azure IoT Central, cihazlarla ilişkili tüm verileri kişisel veriler gibi yönetir ve depolar.

Microsoft Enterprise Services kullandığınızda, Microsoft, sistem tarafından oluşturulan Günlükler olarak bilinen bazı bilgiler üretir. Bu Günlükler, hizmet içinde yürütülen ve tek tek cihazlarla ilgili tanılama verileri ve Kullanıcı etkinliğiyle ilgili olmayan işlemleri oluşturur. Azure IoT Central sistem tarafından oluşturulan günlüklere, uygulama yöneticileri tarafından erişilebilir veya dışarı aktarılabilir.

## <a name="deleting-customer-data"></a>Müşteri verilerini silme

Kullanıcı verilerini silme özelliği yalnızca IoT Central yönetim sayfası aracılığıyla sağlanır. Uygulama yöneticileri silinecek kullanıcıyı seçebilir ve kaydı silmek için uygulamanın sağ üst köşesindeki **Sil** ' i seçin. Uygulama yöneticileri, artık söz konusu uygulamayla ilişkili olmayan bireysel hesapları da kaldırabilir.

Bir Kullanıcı silindikten sonra, bunlara başka bir uyarı e-posta ile gönderilmeyecektir. Ancak, e-posta adresleri, yapılandırılan uyarılardan ayrı ayrı kaldırılmalıdır.

## <a name="exporting-customer-data"></a>Müşteri verilerini dışa aktarma

Verileri dışarı aktarma özelliği yalnızca IoT Central yönetim sayfası aracılığıyla sağlanır. Atanan roller de dahil olmak üzere müşteri verileri, bir uygulama Yöneticisi tarafından seçilebilir, kopyalanabilir ve yapıştırılabilir.

## <a name="links-to-additional-documentation"></a>Ek belgelere bağlantılar

Rol tanımları dahil hesap yönetimi hakkında daha fazla bilgi için bkz. [uygulamanızı yönetme](howto-administer.md).
