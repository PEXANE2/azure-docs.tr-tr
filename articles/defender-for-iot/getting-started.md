---
title: Dağıtım seçenekleri
description: IoT özellikleri ve hizmeti için Defender 'ın temel iş akışını anlamak için kullanmaya başlayın.
services: defender-for-iot
ms.service: defender-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/09/2020
ms.author: mlottner
ms.openlocfilehash: e3de253ee6f45f9296d6b09189fe4bc488be36ad
ms.sourcegitcommit: 30505c01d43ef71dac08138a960903c2b53f2499
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2020
ms.locfileid: "92090072"
---
# <a name="getting-started-with-azure-defender-for-iot"></a>IoT için Azure Defender ile çalışmaya başlama

Bu makalede, IoT için Azure Defender 'ı çalıştırmak için gereken dağıtım ve ekleme işlemlerinin açıklanmaktadır. Ek adımlar da gereklidir. Bu adımları anlamanız ve eşlik eden belgelerdeki bilgileri öğrenmeniz önerilir.

Tüm adımları tamamladıktan sonra IoT sensörleri için Azure Defender ağınızı izlecektir. Çözümünüzü nasıl ayarladığınıza bağlı olarak, algılamalar da şirket içi yönetim konsoluna veya IoT Hub gönderilebilir.

IoT için Azure Defender 'ı çalışır duruma getirmek için aşağıdaki adımları izleyin.

## <a name="1-set-up-azure"></a>1. Azure 'ı ayarlama

- Bir Azure hesabı ayarlayın. Daha fazla bilgi için bkz. [Azure hesabı oluşturma](https://docs.microsoft.com/learn/modules/create-an-azure-account/).

- Güvenlik duvarı veya proxy: belirli bağlantılara izin vermek üzere yapılandırılmış bir güvenlik duvarınız veya benzer bir araya giren ağ cihazınız varsa, *. azure-devices.net:443 ' nin güvenlik duvarı veya proxy 'ye açıldığını doğrulayın. Joker karakterler desteklenmiyorsa veya daha fazla denetim istiyorsanız, belirli IoT Hub FQDN 'SI ILT veya proxy 'niz içinde açılmalıdır. Daha fazla bilgi için bkz. [Reference-IoT Hub uç noktaları](/azure/iot-hub/iot-hub-devguide-endpoints).

## <a name="2-deploy-hardware-software-and-onboard-to-sensor"></a>2. donanım, yazılım dağıtma ve sensöre ekleme

- Algılayıcı donanımı satın alın ve yazılımı yükler. Burada özetlenen adımları izleyin ve daha fazla bilgi için bu makaleye ve [IoT donanım Kılavuzu Için Defender](https://aka.ms/AzureDefenderforIoTBareMetalAppliance) ve [Yükleme Kılavuzu](https://aka.ms/AzureDefenderforIoTInstallSensorISO)' na bakın.

  - Sensörinizi yükledikten sonra, algılayıcı oturum açma kimlik bilgilerini güvenli bir şekilde kaydedin. Etkinleştirme dosyasını sensöre yüklemek için kimlik bilgileri gerekir.

  - Yerel olarak yönetilen sensörlerle çalışıyorsanız, algılayıcının IP adresini veya yüklemede tanımlanan algılayıcı adını güvenli bir şekilde kaydedin. Bir algılayıcı adı oluştururken IoT portalı için Defender 'da algılayıcı kaydı sırasında bunu kullanmak isteyebilirsiniz. IoT portalında Azure Defender 'daki kayıt adı ve algılayıcı konsolunda görünen dağıtılan algılayıcının IP adresi arasında daha kolay izleme ve tutarlı adlandırma sağlamak için bunları daha sonra kullanabilirsiniz.

- Algılayıcısı, IoT portalı için Defender ile kaydedin ve bir algılayıcı etkinleştirme dosyası indirin.

- Etkinleştirme dosyasını sensöre yükleyin.

## <a name="3-perform-network-setup-for-sensor-monitoring-and-management"></a>3. algılayıcı izlemesi ve yönetimi için ağ kurulumu gerçekleştirin

- Sensörinizi ağa bağlayın. [Ağ kurulum kılavuzunda](https://aka.ms/AzureDefenderForIoTNetworkSetup)açıklanmıştır.

## <a name="4-start-discovering-your-network"></a>4. ağınızı bulmaya başlayın

- Algılayıcı konsolundaki sistem ayarlarını ince ayar.

- Algılayıcıları şirket içi yönetim konsoluna bağlayın.

Daha fazla bilgi için bkz. [IoT algılayıcısı Için Azure Defender Kullanıcı Kılavuzu](https://aka.ms/AzureDefenderforIoTUserGuide) ve [IoT şirket içi yönetim konsolu Kullanıcı Kılavuzu](https://aka.ms/DefenderForIoTManagementConsole).

## <a name="5-populate-azure-sentinel-with-alert-information"></a>5. Azure Sentinel 'i uyarı bilgileriyle doldurma

- Azure Sentinel 'e uyarı bilgileri göndermek için Azure Sentinel 'i yapılandırın: [IoT Için Defender 'daki verilerinizi Azure Sentinel 'e bağlayın](how-to-configure-with-sentinel.md).
 

## <a name="next-steps"></a>Sonraki adımlar

- [IoT Için Defender 'ı](quickstart-onboard-iot-hub.md) etkinleştirin
- [Çözümünüzü](quickstart-configure-your-solution.md) yapılandırın
- [Güvenlik modülleri oluştur](quickstart-create-security-twin.md)
- [Özel uyarıları](quickstart-create-custom-alerts.md) yapılandırma
- [Güvenlik aracısı dağıtma](how-to-deploy-agent.md)
