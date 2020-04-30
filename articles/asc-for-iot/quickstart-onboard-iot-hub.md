---
title: 'Hızlı başlangıç: hizmeti etkinleştirme'
description: Azure IoT Hub IoT güvenlik hizmeti için Azure Güvenlik Merkezi 'ni ekleme ve etkinleştirme hakkında bilgi edinin.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: 670e6d2b-e168-4b14-a9bf-51a33c2a9aad
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/08/2019
ms.author: mlottner
ms.openlocfilehash: 0d3e4ad63baf6883aa8fadaca5b1f2d28fa14881
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81310747"
---
# <a name="quickstart-onboard-azure-security-center-for-iot-service-in-iot-hub"></a>Hızlı başlangıç: IoT Hub 'da IoT hizmeti için Azure Güvenlik Merkezi 'Ni ekleme

Bu makalede, mevcut IoT Hub IoT hizmeti için Azure Güvenlik Merkezi 'nin nasıl etkinleştirileceği hakkında bir açıklama sunulmaktadır. Şu anda bir IoT Hub yoksa, kullanmaya başlamak için [Azure Portal kullanarak IoT Hub oluşturma](https://docs.microsoft.com/azure/iot-hub/iot-hub-create-through-portal) makalesine bakın.

> [!NOTE]
> IoT için Azure Güvenlik Merkezi şu anda yalnızca Standart katman IoT Hub 'Larını desteklemektedir.

## <a name="prerequisites-for-enabling-the-service"></a>Hizmeti etkinleştirme önkoşulları

- Log Analytics çalışma alanı
  - İki tür bilgi, IoT için Azure Güvenlik Merkezi tarafından Log Analytics çalışma alanınızda varsayılan olarak saklanır; **güvenlik uyarıları** ve **önerileri**.
  - Ek bilgi türü, **Ham olaylar**için depolama alanını eklemeyi seçebilirsiniz. **Ham olayların** Log Analytics daha fazla depolama maliyeti taşıdığına göz önünde unutmayın.
- IoT Hub (Standart katman)
- Tüm [hizmet önkoşullarını](service-prerequisites.md) karşılayın

## <a name="enable-azure-security-center-for-iot-on-your-iot-hub"></a>IoT Hub IoT için Azure Güvenlik Merkezi 'ni etkinleştirin

IoT Hub güvenliği sağlamak için:

1. **IoT Hub** Azure Portal açın.
1. **Güvenlik** menüsünde, **IoT çözümünüzün güvenliğini sağla**' yı tıklatın.

Tebrikler! IoT Hub IoT için Azure Güvenlik Merkezi 'Ni etkinleştirmeyi tamamladınız.

### <a name="geolocation-and-ip-address-handling"></a>Coğrafi konum ve IP adresi işleme

IoT çözümünüzü güvenli hale getirmek için IoT cihazlarınıza gelen ve giden bağlantıların IP adresleri, IoT Edge ve IoT Hub, varsayılan olarak toplanır ve depolanır. Bu bilgiler şüpheli IP kaynaklarından anormal bağlantıları algılamak için gereklidir. Örneğin, bilinen bir botnet 'in IP kaynağından veya coğrafi konum dışında bir IP kaynağından bağlantı kurmak için denemeler yapılır. IoT hizmeti için Azure Güvenlik Merkezi, IP adresi verilerinin toplanmasını istediğiniz zaman etkinleştirme ve devre dışı bırakma esnekliği sunar.

IP adresi verilerini toplamayı etkinleştirmek veya devre dışı bırakmak için:

1. IoT Hub açın ve **güvenlik** menüsünden **genel bakış** ' ı seçin.
1. **Ayarlar** ekranını seçin ve coğrafi konum ve/veya IP işleme ayarlarını istediğiniz gibi değiştirin.

### <a name="log-analytics-creation"></a>Log Analytics oluşturma

IoT için Azure Güvenlik Merkezi açıldığında, IoT cihazlarınıza, IoT Edge ve IoT Hub yönelik ham güvenlik olaylarını, uyarıları ve önerilerini depolamak için varsayılan bir Azure Log Analytics çalışma alanı oluşturulur. Her ay, Azure Log Analytics hizmetine müşteri başına alınan ilk beş (5) veri giriş ücretsizdir. Azure Log Analytics çalışma alanınıza alınan her GB veri, ilk 31 gün boyunca ücretsiz olarak tutulur. [Log Analytics](https://azure.microsoft.com/pricing/details/monitor/) fiyatlandırması hakkında daha fazla bilgi edinin.

Log Analytics çalışma alanı yapılandırmasını değiştirmek için:

1. IoT Hub açın ve **güvenlik** menüsünden **genel bakış** ' ı seçin.
1. **Ayarlar** ekranını seçin ve Log Analytics ayarların çalışma alanı yapılandırmasını istediğiniz gibi değiştirin.

### <a name="customize-your-iot-security-solution"></a>IoT Güvenlik çözümünüzü özelleştirme

Varsayılan olarak, IoT çözümü için Azure Güvenlik Merkezi 'ni açmak, Azure aboneliğinizdeki tüm IoT Hub 'Larını otomatik olarak korur.

Belirli bir IoT Hub IoT hizmeti için Azure Güvenlik Merkezi 'ni etkinleştirmek veya devre dışı bırakmak için:

1. IoT Hub açın ve **güvenlik** menüsünden **genel bakış** ' ı seçin.
1. **Ayarlar** ekranını seçin ve Azure aboneliğinizdeki herhangi bir IoT Hub 'ının güvenlik ayarlarını istediğiniz gibi değiştirin.

## <a name="next-steps"></a>Sonraki adımlar

Çözümünüzü yapılandırmak için sonraki makaleye ilerleyin...

> [!div class="nextstepaction"]
> [Çözümünüzü yapılandırma](quickstart-configure-your-solution.md)
