---
title: Azure IoT Central'da bir RuuviTag'ı bağlayın | Microsoft Dokümanlar
description: Bir RuuviTag ortam sensörünü IoT Central uygulamanıza nasıl bağlayabilirsiniz öğrenin.
services: iot-central
ms.service: iot-central
ms.topic: how-to
ms.custom:
- iot-storeAnalytics-conditionMonitor
- iot-p0-scenario
ms.author: avneets
author: avneet723
ms.date: 11/27/2019
ms.openlocfilehash: e8d1c4a605e8db2e9753bb80c9712dd6c2be7b59
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80158274"
---
# <a name="connect-a-ruuvitag-sensor-to-your-azure-iot-central-application"></a>Azure IoT Merkezi uygulamanıza bir RuuviTag sensörü bağlayın

Bu makalede, bir çözüm oluşturucu olarak, bir RuuviTag sensörünü Microsoft Azure IoT Central uygulamanıza nasıl bağlayabileceğiniz açıklanmaktadır.

Ruuvi etiketi nedir?

RuuviTag, iş müşterilerinin, geliştiricilerin, üreticilerin, öğrencilerin ve hobi cilerin ihtiyaçlarını karşılamak üzere tasarlanmış gelişmiş bir açık kaynak sensör işaret platformudur. Cihaz, kutusundan çıkarır çıkarmaz çalışacak şekilde ayarlanmıştır ve ihtiyacınız olan yere dağıtmanız için hazırdır. Bir ortam sensörü ve ivmeölçer dahili bir Bluetooth LE işaret bulunuyor.

RuuviTag, BLE (Bluetooth Düşük Enerji) üzerinden iletişim kurar ve Azure IoT Central ile konuşmak için bir ağ geçidi aygıtı gerektirir. Bir RuuviTag'ın IoT Central'a bağlanmasını sağlamak için Rigado Cascade 500 gibi bir ağ geçidi aygıtınız olduğundan emin olun.

Rigado Cascade 500 ağ geçidi cihazı kurmak istiyorsanız lütfen [buradaki talimatları](./howto-connect-rigado-cascade-500.md) uygulayın.

## <a name="prerequisites"></a>Ön koşullar

RuuviTag sensörlerini bağlamak için aşağıdaki kaynaklara ihtiyacınız vardır:

* Bir RuuviTag sensörü. Daha fazla bilgi için lütfen [RuuviTag](https://ruuvi.com/)adresini ziyaret edin.
* Rigado Cascade 500 cihazı veya başka bir BLE ağ geçidi. Daha fazla bilgi için lütfen [Rigado'yu](https://www.rigado.com/)ziyaret edin.
* Azure IoT Merkezi uygulaması. Daha fazla bilgi için [yeni bir uygulama oluşturmaya](./quick-deploy-iot-central.md)bakın.

## <a name="add-a-ruuvitag-device-template"></a>RuuviTag aygıt şablonu ekleme

Azure IoT Merkezi uygulama örneğinize bir RuuviTag sensörü nde yer almak için, uygulamanızda ilgili bir aygıt şablonu yapılandırmanız gerekir.

RuuviTag aygıt şablonu eklemek için:

1. Sol bölmede ***Aygıt Şablonları*** sekmesine gidin, seçin **+ Yeni**: ![Yeni aygıt](./media/howto-connect-ruuvi/devicetemplate-new.png) şablonu oluşturma ***Sayfa, size özel bir şablon oluşturma*** veya önceden yapılandırılmış bir aygıt ***şablonu kullanma*** seçeneği sunar
1. Aşağıda gösterildiği gibi önceden yapılandırılmış aygıt şablonları listesinden RuuviTag ![aygıt şablonu seçin: RuuviTag aygıt şablonu seçin](./media/howto-connect-ruuvi/devicetemplate-preconfigured.png)
1. ***Sonraki'ni seçin:*** Bir sonraki adıma devam etmek için özelleştirin.
1. Bir sonraki ekranda, IoT Central uygulamanızda C500 cihaz şablonuna binecek şekilde ***Oluştur'u*** seçin.

## <a name="connect-a-ruuvitag-sensor"></a>RuuviTag sensörünü bağlayın

Daha önce de belirtildiği gibi, RuuviTag'ı IoT Central uygulamanızla bağlamak için bir ağ geçidi aygıtı kurmanız gerekir. Aşağıdaki adımlar, bir Rigado Cascade 500 ağ geçidi aygıtı ayarladığınızvarsa.  

1. Rigado Cascade 500 cihazınızı güçlandırın ve ağ bağlantınıza bağlayın (Ethernet veya kablosuz)
1. RuuviTag'ın kapağını kapatın ve pil ile bağlantıyı korumak için plastik sekmeyi çekin.
1. RuuviTag'ı, IoT Central uygulamanızda zaten yapılandırılmış olan Rigado Cascade 500 ağ geçidine yakın yerleştirin.
1. Sadece birkaç saniye içinde, RuuviTag'ınız IoT Central'daki aygıtlar listenizde görünmelidir.  
    ![RuuviTag Cihaz Listesi](./media/howto-connect-ruuvi/ruuvi-devicelist.png)

Artık bu RuuviTag'ı IoT Central uygulamanızda kullanabilirsiniz.  

## <a name="create-a-simulated-ruuvitag"></a>Simüle edilmiş Bir RuuviTag oluşturma

Fiziksel bir RuuviTag aygıtınız yoksa, Azure IoT Central uygulamanızda test etmek için kullanmak üzere simüle edilmiş bir RuuviTag sensörü oluşturabilirsiniz.

Simüle Edilmiş Bir RuuviTag oluşturmak için:

1. **RuuviTag > Cihazları**seçin.
1. Seçin **+ Yeni**.
1. Benzersiz bir **Aygıt Kimliği** ve uygun bir **Aygıt adı**belirtin.  
1. **Benzetim** li ayarı etkinleştirin.
1. **Oluştur'u**seçin.  

## <a name="next-steps"></a>Sonraki Adımlar

Bir RuuviTag'ı Azure IoT Central uygulamanıza nasıl bağlayabileceğinizi öğrendiğinize göre, önerilen bir sonraki adım, uçtan uca bir çözüm oluşturmak için [IoT Central uygulamanızı nasıl özelleştireceğimize](../retail/tutorial-in-store-analytics-customize-dashboard-pnp.md) öğrenmektir.
