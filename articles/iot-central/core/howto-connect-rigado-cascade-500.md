---
title: Azure IoT Central'da Rigado Cascade 500'e bağlayın | Microsoft Dokümanlar
description: Rigado Cascade 500 ağ geçidi cihazını IoT Central uygulamanıza nasıl bağlayabilirsiniz öğrenin.
services: iot-central
ms.service: iot-central
ms.topic: how-to
ms.custom:
- iot-storeAnalytics-conditionMonitor
- iot-p0-scenario
ms.author: avneets
author: avneet723
ms.date: 11/27/2019
ms.openlocfilehash: 3e6026e683d46ca9062c5c139fcd7febd7df9bd1
ms.sourcegitcommit: 31e9f369e5ff4dd4dda6cf05edf71046b33164d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81758253"
---
# <a name="connect-a-rigado-cascade-500-gateway-device-to-your-azure-iot-central-application"></a>Rigado Cascade 500 ağ geçidi aygıtını Azure IoT Merkezi uygulamanıza bağlayın

*Bu makale, çözüm oluşturucular ve aygıt geliştiricileri için geçerlidir.*

Bu makalede, bir çözüm oluşturucu olarak, bir Rigado Cascade 500 ağ geçidi aygıtını Microsoft Azure IoT Merkezi uygulamanıza nasıl bağlayabileceğiniz açıklanmaktadır. 

## <a name="what-is-cascade-500"></a>Cascade 500 nedir?

Cascade 500 IoT ağ geçidi, Rigado'nun Cascade Edge-as-a-Service çözümünün bir parçası olarak yer alan bir donanım teklifidir. Ticari IoT proje ve ürün ekiplerine esnek kenar bilgi işlem gücü, sağlam bir konteyner uygulama ortamı ve Bluetooth 5, LTE, & Wi-Fi gibi çok çeşitli kablosuz aygıt bağlantısı seçenekleri sunar.

Cascade 500, Azure IoT Tak ve Çalıştır (önizleme) için önceden sertifikalandırılır ve çözüm oluşturucularımızın cihazda uçtan uca çözümlerine kolayca binmelerine olanak tanır. Basamaklı ağ geçidi, ağ geçidi aygıtına yakın olan çeşitli durum izleme sensörlerine kablosuz olarak bağlanmanızı sağlar. Bu sensörler ağ geçidi cihazı ile IoT Central'a binilebilir.

## <a name="prerequisites"></a>Ön koşullar
Bu nasıl yapılacağını zinde kılavuzunda adım atmak için aşağıdaki kaynaklara ihtiyacınız var:

* Rigado Cascade 500 cihazı. Daha fazla bilgi için lütfen [Rigado'yu](https://www.rigado.com/)ziyaret edin.
* Azure IoT Merkezi uygulaması. Daha fazla bilgi için [yeni bir uygulama oluşturmaya](./quick-deploy-iot-central.md)bakın.

## <a name="add-a-device-template"></a>Cihaz şablonu ekleme

Azure IoT Merkezi uygulama örneğinize cascade 500 ağ geçidi aygıtında binebilmek için, uygulamanızda ilgili bir aygıt şablonu yapılandırmanız gerekir.

Basamaklı 500 aygıt şablonu eklemek için: 

1. Sol bölmedeki ***Aygıt Şablonları*** sekmesine gidin, select ![+ **Yeni**: Yeni aygıt şablonu oluşturma](./media/howto-connect-rigado-cascade-500/device-template-new.png)
1. Sayfa, size özel ***bir şablon oluşturma*** veya önceden ***yapılandırılmış bir aygıt şablonu kullanma*** seçeneği sunar
1. Aşağıda gösterildiği gibi önceden yapılandırılmış aygıt şablonları listesinden C500 ![aygıt şablonu seçin: C500 aygıt şablonu seçin](./media/howto-connect-rigado-cascade-500/device-template-preconfigured.png)
1. ***Sonraki'ni seçin:*** Bir sonraki adıma devam etmek için özelleştirin. 
1. Bir sonraki ekranda, IoT Central uygulamanızda C500 cihaz şablonuna binecek şekilde ***Oluştur'u*** seçin.

## <a name="retrieve-application-connection-details"></a>Uygulama bağlantı ayrıntılarını alma

Şimdi, Cascade 500 aygıtını bağlamak için Azure IoT Central uygulamanızın **Kapsam Kimliği** ve **Birincil anahtarını** almanız gerekir. 

1. Sol bölmede **İdare'ye** gidin ve **Aygıt bağlantısına**tıklayın. 
2. IoT Central uygulamanız için **Kapsam Kimliği'ni** not edin.
![Uygulama Kapsam Kimliği](./media/howto-connect-rigado-cascade-500/app-scope-id.png)
3. Şimdi **Görünüm Tuşları'na** tıklayın ve **Birincil Anahtar**
![Birincil Anahtarı not edin](./media/howto-connect-rigado-cascade-500/primary-key-sas.png)  

## <a name="contact-rigado-to-connect-the-gateway"></a>Ağ geçidini bağlamak için Rigado ile iletişime geçin 

Cascade 500 cihazını IoT Central uygulamanıza bağlamak için Rigado ile iletişime geçmeniz ve yukarıdaki adımlardaki uygulama bağlantı bilgilerini sağlamanız gerekmektedir. 

Cihaz internete bağlandıktan sonra Rigado, güvenli bir kanal üzerinden Cascade 500 ağ geçidi cihazına bir yapılandırma güncellemesi indirebilecek. 

Bu güncelleştirme, Cascade 500 aygıtındaki IoT Merkezi bağlantı ayrıntılarını uygular ve aygıtlar listenizde görünür. 

![Birincil Anahtar](./media/howto-connect-rigado-cascade-500/devices-list-c500.png)  

C500 cihazınızı IoT Central uygulamanızda kullanmaya hazırsınız!

## <a name="next-steps"></a>Sonraki adımlar

Aygıt geliştiricisiyseniz, önerilen bazı sonraki adımlar şunlardır:

- Azure [IoT Central'da Aygıt bağlantısı](./concepts-get-connected.md) hakkında bilgi edinin
- [Azure CLI'yi kullanarak aygıt bağlantısını](./howto-monitor-devices-azure-cli.md) nasıl izleyeceğinizi öğrenin
