---
title: Uygulama IoT Tak ve Çalıştır Önizleme modeli bulma | Microsoft Dokümanlar
description: Bir çözüm geliştiricisi olarak, ioT tak ve çalıştır model bulmanızı çözümünüzde nasıl uygulayabileceğinizhakkında bilgi edinin.
author: Philmea
ms.author: philmea
ms.date: 12/26/2019
ms.topic: conceptual
ms.custom: mvc
ms.service: iot-pnp
services: iot-pnp
manager: philmea
ms.openlocfilehash: 74eb38269a3c7fbdc6d95554a8a8cef14eb0b787
ms.sourcegitcommit: d57d2be09e67d7afed4b7565f9e3effdcc4a55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81770469"
---
# <a name="implement-iot-plug-and-play-preview-model-discovery-in-an-iot-solution"></a>IoT çözümünde IoT Tak ve Çalıştır Önizleme modeli keşfini uygulayın

Bu makalede, bir çözüm geliştiricisi olarak, bir IoT çözümünde IoT Tak ve Çalıştır Önizleme modeli keşfini nasıl uygulayabileceğiniz açıklanmaktadır.  IoT Tak ve Çalıştır modeli bulma, IoT Tak ve Çalıştır cihazlarının desteklenen yetenek modellerini ve arabirimlerini nasıl tanımladığı ve bir IoT çözümlü bu özellik modellerini ve arayüzlerini nasıl geri aldığımızdır.

IoT çözümünün iki geniş kategorisi vardır: bilinen bir IoT Tak ve Çalıştır aygıtı yla çalışan amaca yönelik çözümler ve herhangi bir IoT Tak ve Çalıştır aygıtıyla çalışan model odaklı çözümler.

Bu kavram makalesi, her iki çözüm türünde model bulmanın nasıl uygulanacağını açıklar.

## <a name="model-discovery"></a>Model keşfi

Bir IoT Tak ve Çalıştır aygıtı ilk olarak IoT hub'ınıza bağlandığında, bir model bilgi telemetrisi iletisi gönderir. Bu ileti, aygıtın uyguladığı arabirimlerin adlarını içerir. Çözümünüzün aygıtla çalışması için bu t'leri çözmesi ve her arabirim için tanımları alması gerekir.

Bir IoT Tak ve Çalıştır aygıtının bir hub'a bağlanmak için Aygıt Sağlama Hizmetini (DPS) kullandığında attığı adımlar şunlardır:

1. Aygıt açık olduğunda, DPS için genel bitiş noktasına bağlanır ve izin verilen yöntemlerden birini kullanarak kimlik doğrulaması sağlar.
1. DPS daha sonra aygıtın kimliğini doğrular ve aygıtı hangi IoT hub'ına atamasını söyleyen kuralı arar. DPS daha sonra aygıtı bu hub'a kaydeder.
1. DPS aygıta bir IoT Hub bağlantı dizesi döndürür.
1. Aygıt daha sonra IoT Hub'ınıza bir keşif telemetri iletisi gönderir. Bulma telemetri iletisi, aygıtın uyguladığı arabirimlerin dislerini içerir.
1. IoT Tak ve Çalıştır cihazı artık IoT hub'ınızı kullanan bir çözümle çalışmaya hazır.

Aygıt doğrudan IoT hub'ınıza bağlanırsa, aygıt koduna gömülü bir bağlantı dizesini kullanarak bağlanır. Aygıt daha sonra IoT Hub'ınıza bir keşif telemetri iletisi gönderir.

Model bilgileri telemetri iletisi hakkında daha fazla bilgi edinmek için [ModelInformation](concepts-common-interfaces.md) arabirimine bakın.

### <a name="purpose-built-iot-solutions"></a>Amaca yönelik IoT çözümleri

Amaca yönelik bir IoT çözümü, bilinen bir ioT Tak ve Çalıştır aygıtı özelliği modelleri ve arabirimleriyle çalışır.

Çözümünüze önceden bağlanacak aygıtlar için yetenek modeline ve arabirimlerine sahip olacaksınız. Çözümünüzü hazırlamak için aşağıdaki adımları kullanın:

1. Arabirim JSON dosyalarını, çözümünüzün okuyabileceği bir [model deposunda](./howto-manage-models.md) saklayın.
1. Beklenen IoT Tak ve Çalıştır özelliği modellerine ve arabirimine göre IoT çözümünüze mantık yazın.
1. Çözümünüzünüzün kullandığı IoT merkezinden gelen bildirimlere abone olun.

Yeni bir aygıt bağlantısı için bildirim aldığınızda aşağıdaki adımları izleyin:

1. Aygıt tarafından uygulanan yetenek modelinin ve arabirimlerinin aygıtlarının aygıtlarını ve arabirimlerinin aygıtlarını almak için bulma telemetri iletisini okuyun.
1. Yetenek modelinin kimliğini, önceden depoladığınız yetenek modellerinin kimlikleri ile karşılaştırın.
1. Artık ne tür bir aygıtın bağolduğunu biliyorsunuz. Kullanıcıların aygıtla uygun şekilde etkileşimkurmasını sağlamak için daha önce yazdığınız mantığı kullanın.

### <a name="model-driven-solutions"></a>Model odaklı çözümler

Model odaklı bir IoT çözümü herhangi bir IoT Tak ve Çalıştır aygıtıyla çalışabilir. Model odaklı bir IoT Çözümü oluşturmak daha karmaşıktır, ancak çözümünüz gelecekte eklenen cihazlarla çalışır.

Model tabanlı bir IoT çözümü oluşturmak için, IoT Tak ve Çalıştır arabirimi ilkellerine karşı mantık oluşturmanız gerekir: telemetri, özellikler ve komutlar. IoT çözümünüzün mantığı, birden çok telemetri, özellik ve komut özelliklerini birleştirerek bir aygıtı temsil eder.

Çözümünüz ayrıca kullandığı IoT hub'ından gelen bildirimlere de abone olmalıdır.

Çözümünüz yeni bir aygıt bağlantısı için bir bildirim aldığında aşağıdaki adımları izleyin:

1. Aygıt tarafından uygulanan yetenek modelinin ve arabirimlerinin aygıtlarının aygıtlarını ve arabirimlerinin aygıtlarını almak için bulma telemetri iletisini okuyun.
1. Her kimlik için, aygıtın özelliklerini bulmak için tam JSON dosyasını okuyun.
1. Çözümünüz tarafından daha önce alınan JSON dosyalarını depolamak için oluşturduğun önbelleklerde her arabirimin bulunip bulunmadığını kontrol edin.
1. Ardından, ortak model deposunda bu kimlikle bir arabirimin bulunip bulunmadığını denetleyin. Daha fazla bilgi için [Genel model deposuna](howto-manage-models.md)bakın.
1. Arabirim ortak model deposunda yoksa, çözümünüzün bildiği herhangi bir şirket modeli deposunda aramayı deneyin. Şirket modeli deposuna erişmek için bir bağlantı dizesi gerekir. Daha fazla bilgi için [Şirket modeli deposuna](howto-manage-models.md)bakın.
1. Tüm arabirimleri ortak model deposunda veya şirket modeli deposunda bulamazsanız, aygıtın arabirim tanımını sağlayıp sağlayamadığını kontrol edebilirsiniz. Aygıt, bir komutla arabirim dosyalarının nasıl alınacağı hakkında bilgi yayınlamak için standart [ModelDefinition](concepts-common-interfaces.md) arabirimini uygulayabilir.
1. Aygıt tarafından uygulanan her arabirim için JSON dosyaları bulduysanız, aygıtın yeteneklerini kaydedebilirsiniz. Kullanıcıların aygıtla etkileşim kurmasını sağlamak için daha önce yazdığınız mantığı kullanın.
1. İstediğiniz zaman, aygıt için yetenek modeli kimliği ve arayüz kimliklerini almak için dijital ikizler API'sini arayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bir IoT çözümbulma modelini öğrendiğinize göre, çözümünüz için diğer özelliklerden yararlanmak için [Azure IoT Platformu](overview-iot-plug-and-play.md) hakkında daha fazla bilgi edinin.
