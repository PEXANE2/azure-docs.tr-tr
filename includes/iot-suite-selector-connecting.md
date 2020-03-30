---
title: include dosyası
description: include dosyası
services: iot-suite
author: dominicbetts
ms.service: iot-suite
ms.topic: include
ms.date: 09/17/2018
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: ca4bd3d3b40934323bab8036f3ce72e9281f1de4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67188807"
---
> [!div class="op_single_selector"]
> * [Windows'da C](../articles/iot-accelerators/iot-accelerators-connecting-devices.md)
> * [Linux üzerinde C](../articles/iot-accelerators/iot-accelerators-connecting-devices-linux.md)
> * [Raspberry Pi üzerinde C](../articles/iot-accelerators/iot-accelerators-connecting-pi-c.md)
> * [Node.js (genel)](../articles/iot-accelerators/iot-accelerators-connecting-devices-node.md)
> * [Raspberry Pi üzerinde Node.js](../articles/iot-accelerators/iot-accelerators-connecting-pi-node.md)
> * [MXChip IoT DevKit](../articles/iot-accelerators/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2.md)

Bu eğitimde, Uzaktan İzleme [çözüm hızlandırıcısına](../articles/iot-accelerators/about-iot-accelerators.md)aşağıdaki telemetriyi gönderen bir **Chiller** cihazı uygularsınız:

* Sıcaklık
* Basınç
* Nem oranı

Basitlik için **kod, Chiller**için örnek telemetri değerleri oluşturur. Gerçek sensörleri cihazınıza bağlayarak ve gerçek telemetri göndererek numuneyi genişletebilirsiniz.

Örnek cihaz da:

* Özelliklerini açıklamak için çözüme meta veri gönderir.
* Çözümdeki **Aygıtlar** sayfasından tetiklenen eylemlere yanıt verir.
* Çözümdeki **Aygıtlar** sayfasından gönderen yapılandırma değişikliklerine yanıt verir.

Bu öğreticiyi tamamlamak için etkin bir Azure hesabınızın olması gerekir. Bir hesabınız yoksa, yalnızca birkaç dakika içinde ücretsiz bir deneme hesabı oluşturabilirsiniz. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/).

## <a name="before-you-start"></a>Başlamadan önce

Cihazınız için herhangi bir kod yazmadan önce Uzaktan İzleme çözüm hızlandırıcınızı dağıtın ve çözüme yeni bir gerçek aygıt ekleyin.

### <a name="deploy-your-remote-monitoring-solution-accelerator"></a>Uzaktan İzleme çözüm hızlandırıcınızı dağıtın

Bu öğreticide oluşturduğunuz **Chiller** aygıtı, uzaktan [izleme](../articles/iot-accelerators/quickstart-remote-monitoring-deploy.md) çözüm hızlandırıcısının bir örneğine veri gönderir. Azure hesabınızda Uzaktan İzleme çözüm hızlandırıcısını henüz sağlamadıysanız, [bkz.](../articles/iot-accelerators/quickstart-remote-monitoring-deploy.md)

Uzaktan İzleme çözümü için dağıtım işlemi sona erdiğinde, tarayıcınızdaki çözüm panosunu açmak için **Başlat'ı** tıklatın.

![Çözüm panosu](media/iot-suite-selector-connecting/dashboard.png)

### <a name="add-your-device-to-the-remote-monitoring-solution"></a>Cihazınızı Uzaktan İzleme çözümüne ekleme

> [!NOTE]
> Çözüme zaten bir aygıt eklediyseniz, bu adımı atlayabilirsiniz. Ancak, bir sonraki adım aygıtınızın bağlantı dizesini gerektirir. Bir aygıtın bağlantı dizesini [Azure portalından](https://portal.azure.com) veya [az iot](https://docs.microsoft.com/cli/azure/iot?view=azure-cli-latest) CLI aracını kullanarak alabilirsiniz.

Bir aygıtın çözüm hızlandırıcıya bağlanabilmesi için, geçerli kimlik bilgilerini kullanarak kendisini IoT Hub'a tanımlaması gerekir. Aygıtı çözüme eklediğinizde bu kimlik bilgilerini içeren aygıt bağlantı dizesini kaydetme fırsatınız vardır. Aygıt bağlantı dizesini daha sonra bu öğreticiye istemci uygulamanıza eklersiniz.

Uzaktan İzleme çözümünüze aygıt eklemek için, çözümde **Aygıt Gezgini** sayfasındaaşağıdaki adımları tamamlayın:

1. **+ Yeni aygıtı**seçin ve ardından Aygıt **türü**olarak **Real'i** seçin:

    ![Gerçek cihaz ekleme](media/iot-suite-selector-connecting/devicesprovision.png)

1. Aygıt Kimliği olarak **Fiziksel soğutucu** girin. **Simetrik Anahtar** ve **Otomatik oluşturma anahtarları** seçeneklerini seçin:

    ![Aygıt seçeneklerini seçin](media/iot-suite-selector-connecting/devicesoptions.png)

1. **Uygula**'yı seçin. Ardından **Aygıt Kimliği,** Birincil **Anahtar**ve **Bağlantı dizesi birincil anahtar** değerlerini not edin:

    ![Kimlik bilgilerini alma](media/iot-suite-selector-connecting/credentials.png)

Şimdi Uzaktan İzleme çözüm hızlandırıcısına gerçek bir aygıt eklediniz ve aygıt bağlantı dizesini kaydettiniz. Aşağıdaki bölümlerde, çözümünüze bağlanmak için aygıt bağlantı dizesini kullanan istemci uygulamasını uygularsınız.

İstemci uygulaması yerleşik **Chiller** aygıt modelini uygular. Çözüm hızlandırıcı aygıt modeli, bir aygıt hakkında aşağıdakileri belirtir:

* Aygıtın çözüme bildirdiği özellikler. Örneğin, bir **Chiller** aygıtı yazılım yazılımı ve konumu hakkında bilgi bildirir.
* Cihazın çözüme gönderdiği telemetri türleri. Örneğin, bir **Chiller** cihazı sıcaklık, nem ve basınç değerleri gönderir.
* Çözümden aygıtta çalışacak şekilde zamanlayabileceğiniz yöntemler. Örneğin, bir **Chiller** cihazı **Yeniden Başlatma,** **FirmwareUpdate,** **EmergencyValveRelease**ve **IncreasePressure** yöntemlerini uygulamalıdır.
