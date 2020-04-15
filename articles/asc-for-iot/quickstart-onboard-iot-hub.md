---
title: 'Quickstart: Hizmeti etkinleştirme'
description: Azure IoT Hub'ınızda IoT güvenlik hizmeti için Azure Güvenlik Merkezi'nde nasıl bindirilip etkinleştirin öğrenin.
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
ms.sourcegitcommit: 7e04a51363de29322de08d2c5024d97506937a60
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81310747"
---
# <a name="quickstart-onboard-azure-security-center-for-iot-service-in-iot-hub"></a>Hızlı başlangıç: IoT Hub'da IoT hizmeti için Yerleşik Azure Güvenlik Merkezi

Bu makalede, mevcut IoT Hub'ınızda Azure Güvenlik Merkezi IoT hizmetinin nasıl etkinleştirilen bir açıklama sağlar. Şu anda bir IoT Hub'ınız yoksa, başlamak için [Azure portalını kullanarak Bir IoT Hub'ı Oluştur'a](https://docs.microsoft.com/azure/iot-hub/iot-hub-create-through-portal) bakın.

> [!NOTE]
> IoT için Azure Güvenlik Merkezi şu anda yalnızca standart katman IoT Hub'larını destekler.

## <a name="prerequisites-for-enabling-the-service"></a>Hizmeti etkinleştirmek için ön koşullar

- Log Analytics çalışma alanı
  - IoT için Azure Güvenlik Merkezi tarafından Günlük Analizi çalışma alanınızda varsayılan olarak iki tür bilgi depolanır; **güvenlik uyarıları** ve **öneriler.**
  - Ek bir bilgi türü, **ham olaylar**depolama eklemeyi seçebilirsiniz. **Ham olayları** Log Analytics'te depolamanın ek depolama maliyetleri taşıdığını unutmayın.
- IoT Hub (standart katman)
- Tüm [hizmet ön koşullarıyla](service-prerequisites.md) tanışın

## <a name="enable-azure-security-center-for-iot-on-your-iot-hub"></a>IoT Hub'ınızda IoT için Azure Güvenlik Merkezi'ni etkinleştirme

IoT Hub'ınızda güvenliği sağlamak için:

1. Azure portalında **IoT Hub'ınızı** açın.
1. **Güvenlik** menüsü **altında, IoT çözümünüzü güvenli olarak**sabitleyin'i tıklatın.

Tebrikler! IoT Hub'ınızda IoT için Azure Güvenlik Merkezi'ni etkinleştirme nizi tamamladınız.

### <a name="geolocation-and-ip-address-handling"></a>Coğrafi konum ve IP adresi işleme

IoT çözümünüzü güvence altına almak için, IoT aygıtlarınıza gelen ve giden bağlantıların IP adresleri, IoT Edge ve IoT Hub(lar) varsayılan olarak toplanır ve saklanır. Bu bilgiler, şüpheli IP kaynaklarından anormal bağlantı algılamak için gereklidir. Örneğin, bilinen bir botnet'in IP kaynağından veya coğrafi konumunuz uzun bir ip kaynağından bağlantı kurmaya yönelik girişimlerde bulunulduğunda. Azure Güvenlik Merkezi IoT hizmeti, IP adresi verilerinin toplanmasını her zaman etkinleştirme ve devre dışı bırakma esnekliği sunar.

IP adresi verilerinin toplanmasını etkinleştirmek veya devre dışı etmek için:

1. IoT Hub'ınızı açın ve **ardından Güvenlik** menüsünden **Genel Bakış'ı** seçin.
1. **Ayarlar** ekranını seçin ve coğrafi konum ve/veya IP işleme ayarlarını istediğiniz gibi değiştirin.

### <a name="log-analytics-creation"></a>Günlük Analytics oluşturma

IoT için Azure Güvenlik Merkezi açıkolduğunda, IoT aygıtlarınız, IoT Edge ve IoT Hub'ınız için ham güvenlik olaylarını, uyarıları ve önerileri depolamak için varsayılan bir Azure Günlük Analizi çalışma alanı oluşturulur. Her ay, Azure Log Analytics hizmetine müşteri başına alınan ilk beş (5) GB veri ücretsizdir. Azure Log Analytics çalışma alanınızda alınan her GB veri ilk 31 gün boyunca ücretsiz olarak saklanır. [Log Analytics](https://azure.microsoft.com/pricing/details/monitor/) fiyatlandırması hakkında daha fazla bilgi edinin.

Log Analytics'in çalışma alanı yapılandırmasını değiştirmek için:

1. IoT Hub'ınızı açın ve **ardından Güvenlik** menüsünden **Genel Bakış'ı** seçin.
1. **Ayarlar** ekranını seçin ve Log Analytics ayarlarının çalışma alanı yapılandırmasını istediğiniz gibi değiştirin.

### <a name="customize-your-iot-security-solution"></a>IoT güvenlik çözümünüzü özelleştirin

Varsayılan olarak, Azure Güvenlik Merkezi'ni açmak ioT çözümü için Azure aboneliğiniz altındaki tüm IoT Hub'larını otomatik olarak güvence altına alır.

Azure Güvenlik Merkezi'ni IoT hizmeti için belirli bir IoT Hub'ında açıp kapatmak için:

1. IoT Hub'ınızı açın ve **ardından Güvenlik** menüsünden **Genel Bakış'ı** seçin.
1. **Ayarlar** ekranını seçin ve Azure aboneliğinizdeki herhangi bir IoT hub'ının güvenlik ayarlarını istediğiniz gibi değiştirin.

## <a name="next-steps"></a>Sonraki adımlar

Çözümünüzü yapılandırmak için bir sonraki makaleye ilerleyin...

> [!div class="nextstepaction"]
> [Çözümünüzü yapılandırma](quickstart-configure-your-solution.md)
