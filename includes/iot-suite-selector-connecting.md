---
title: dosya dahil etme
description: dosya dahil etme
services: iot-suite
author: dominicbetts
ms.service: iot-suite
ms.topic: include
ms.date: 09/17/2018
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: ca4bd3d3b40934323bab8036f3ce72e9281f1de4
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "67188807"
---
> [!div class="op_single_selector"]
> * [Windows üzerinde C](../articles/iot-accelerators/iot-accelerators-connecting-devices.md)
> * [Linux üzerinde C](../articles/iot-accelerators/iot-accelerators-connecting-devices-linux.md)
> * [Raspberry Pi üzerinde C](../articles/iot-accelerators/iot-accelerators-connecting-pi-c.md)
> * [Node.js (genel)](../articles/iot-accelerators/iot-accelerators-connecting-devices-node.md)
> * [Raspberry Pi üzerinde Node.js](../articles/iot-accelerators/iot-accelerators-connecting-pi-node.md)
> * [MXChip IoT DevKit](../articles/iot-accelerators/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2.md)

Bu öğreticide, uzaktan Izleme [çözüm hızlandırıcısına](../articles/iot-accelerators/about-iot-accelerators.md)aşağıdaki Telemetriyi gönderen bir **Chiller** cihazı uygulamalısınız:

* Sıcaklık
* Gereksiniminin
* Nem oranı

Kolaylık olması için kod, **Chiller**için örnek telemetri değerleri üretir. Gerçek Sensörlerinizi cihazınıza bağlayarak ve gerçek telemetri göndererek örneği genişletebilirsiniz.

Örnek cihaz ayrıca:

* Özelliklerini anlatmak için çözüme meta veriler gönderir.
* Çözümdeki **cihazlar** sayfasından tetiklenen eylemlere yanıt verir.
* Çözümdeki **aygıtlar** sayfasından gönderilen yapılandırma değişikliklerine yanıt verir.

Bu öğreticiyi tamamlamak için etkin bir Azure hesabınızın olması gerekir. Bir hesabınız yoksa, yalnızca birkaç dakika içinde ücretsiz bir deneme hesabı oluşturabilirsiniz. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/).

## <a name="before-you-start"></a>Başlamadan önce

Cihazınız için kod yazmadan önce, uzaktan Izleme çözüm hızlandırıcıyı dağıtın ve çözüme yeni bir gerçek cihaz ekleyin.

### <a name="deploy-your-remote-monitoring-solution-accelerator"></a>Uzaktan Izleme çözümü hızlandırıcıyı dağıtın

Bu öğreticide oluşturduğunuz **chilaygıtı** , verileri [Uzaktan izleme](../articles/iot-accelerators/quickstart-remote-monitoring-deploy.md) çözüm hızlandırıcının bir örneğine gönderir. Azure hesabınızda zaten uzaktan Izleme çözüm Hızlandırıcısını sağladıysanız, bkz [. uzaktan izleme çözüm Hızlandırıcısını dağıtma](../articles/iot-accelerators/quickstart-remote-monitoring-deploy.md)

Uzaktan Izleme çözümünün dağıtım işlemi tamamlandığında, tarayıcınızda çözüm panosunu açmak için **Başlat** ' a tıklayın.

![Çözüm panosu](media/iot-suite-selector-connecting/dashboard.png)

### <a name="add-your-device-to-the-remote-monitoring-solution"></a>Cihazınızı uzaktan Izleme çözümüne ekleme

> [!NOTE]
> Çözümünüze zaten bir cihaz eklediyseniz, bu adımı atlayabilirsiniz. Ancak, sonraki adım cihaz Bağlantı dizenizi gerektirir. Bir cihazın bağlantı dizesini [Azure Portal](https://portal.azure.com) veya [az IoT](https://docs.microsoft.com/cli/azure/iot?view=azure-cli-latest) CLI aracını kullanarak alabilirsiniz.

Bir cihazın çözüm hızlandırıcısına bağlanması için, geçerli kimlik bilgilerini kullanarak IoT Hub kendisini tanımlaması gerekir. Cihazı çözüme eklerken bu kimlik bilgilerini içeren cihaz bağlantı dizesini kaydetme şansınız vardır. Bu öğreticide daha sonra istemci uygulamanıza cihaz bağlantı dizesini dahil edersiniz.

Uzaktan Izleme çözümünüze bir cihaz eklemek için, çözümdeki **Device Explorer** sayfasında aşağıdaki adımları izleyin:

1. **+ Yeni cihaz**' ı seçin ve ardından **cihaz türü**olarak **Gerçek** öğesini seçin:

    ![Gerçek cihaz ekleme](media/iot-suite-selector-connecting/devicesprovision.png)

1. Cihaz KIMLIĞI olarak **fiziksel-Chiller** girin. **Simetrik anahtar** seçin ve **anahtarları otomatik oluştur** seçeneklerini belirleyin:

    ![Cihaz seçeneklerini belirleyin](media/iot-suite-selector-connecting/devicesoptions.png)

1. **Uygula**'yı seçin. Ardından **CIHAZ kimliği**, **birincil anahtar**ve **bağlantı dizesi birincil anahtar** değerlerini bir yere unutmayın:

    ![Kimlik bilgilerini al](media/iot-suite-selector-connecting/credentials.png)

Artık, uzaktan Izleme çözüm hızlandırıcısına gerçek bir cihaz eklediniz ve cihaz bağlantı dizesini not etmiştiniz. Aşağıdaki bölümlerde, çözümünüze bağlanmak için cihaz bağlantı dizesini kullanan istemci uygulamasını uygulayacağınızı unutmayın.

İstemci uygulaması, yerleşik **chilcihaz** modelini uygular. Bir çözüm Hızlandırıcısı cihaz modeli, bir cihaz hakkında şunları belirtir:

* Cihazın çözüme rapor bildirdiği Özellikler. Örneğin, bir **chilcihazı** üretici yazılımı ve konumuyla ilgili bilgileri raporlar.
* Cihazın çözüme gönderdiği telemetri türleri. Örneğin, bir **chilcihazı** sıcaklık, nem ve basınç değerlerini gönderir.
* Çözümden, cihazda çalıştırmak için zamanlayabilirsiniz. Örneğin, bir **chilcihazı** , **reboot**, **firmwareupdate**, **acil Gencyvalverelease**ve **ıncreasebasınç** yöntemleri uygulamalıdır.
