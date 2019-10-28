---
title: Azure IoT Central bir RuuviTag bağlantısı | Microsoft Docs
description: IoT Central uygulamanıza bir RuuviTag ortam algılayıcısı bağlamayı öğrenin.
services: iot-central
ms.service: iot-central
ms.topic: conceptual
ms.custom:
- iot-storeAnalytics-conditionMonitor
- iot-p0-scenario
ms.author: avneets
author: avneet723
ms.date: 10/19/2019
ms.openlocfilehash: 0a93bedb697a0d5a514fddab55c79b969b2bd77b
ms.sourcegitcommit: 4c3d6c2657ae714f4a042f2c078cf1b0ad20b3a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2019
ms.locfileid: "72954233"
---
# <a name="connect-a-ruuvitag-sensor-to-your-azure-iot-central-application"></a>Azure IoT Central uygulamanıza bir RuuviTag algılayıcısı bağlama

Bu makalede, bir çözüm Oluşturucusu olarak, bir RuuviTag algılayıcısı Microsoft Azure IoT Central uygulamanıza nasıl bağlayabileceğinizi açıklanmaktadır.

Ruuvi etiketi nedir?

RuuviTag, iş müşterilerinin, geliştiricilerin, mekanizmalarının, öğrencilerin ihtiyaçlarını karşılamak için tasarlanan ve kendi evinizde ve kişisel endeavors bir parçası olarak bile kullanılabilen, gelişmiş bir açık kaynaklı algılayıcı işaret platformudur. Cihaz, kutudan çıkar almaz ve ihtiyacınız olan yere dağıtılmaya hazır hale geldiğinde çalışır. Ortam algılayıcısı ve hızlandırma için yerleşik olarak bulunan bir Bluetooth LE işaret. 

RuuviTag, BLE üzerinden iletişim kurar (Bluetooth düşük enerji) ve bu nedenle Azure IoT Central ile iletişim kurmak için bir ağ geçidi cihazı gerektirir. Lütfen Rigado Cascade 500 gibi bir ağ geçidi cihazına sahip olduğunuzdan emin olun. IoT Central için bir RuuviTag bağlantısı kurabiliyor. 

Bir Rigado basamaklı 500 ağ geçidi cihazı kurmak istiyorsanız lütfen [buradaki yönergeleri](./howto-connect-rigado-cascade-500-pnp.md) izleyin.

## <a name="prerequisites"></a>Önkoşullar
RuuviTag sensörlerinden bağlantı kurmak için aşağıdaki kaynaklara ihtiyacınız vardır:

* RuuviTag algılayıcısı. Daha fazla bilgi için lütfen [Ruuvitag](https://ruuvi.com/)adresini ziyaret edin. 
* Bir Rigado Cascade 500 cihazı veya başka bir BLE ağ geçidi. Daha fazla bilgi için lütfen [Rigado](https://www.rigado.com/)adresini ziyaret edin.
* Önizleme uygulaması şablonlarından birinden oluşturulmuş bir Azure IoT Central uygulaması. Daha fazla bilgi için [Yeni uygulama oluşturma](https://docs.microsoft.com/azure/iot-central/quick-deploy-iot-central-pnp?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json)bölümüne bakın.

## <a name="add-a-ruuvitag-device-template"></a>RuuviTag cihaz şablonu ekleme

Azure IoT Central uygulama örneğinize bir RuuviTag algılayıcısı eklemek için, uygulamanız içinde karşılık gelen bir cihaz şablonu yapılandırmanız gerekecektir.

Bir RuuviTag cihaz şablonu eklemek için: 

1. Sol bölmedeki ***cihaz şablonları*** sekmesine giderek **+ Yeni**' yi seçin ![yeni cihaz şablonu oluştur ' a tıklayın](./media/howto-connect-ruuvi/devicetemplate-new.png) sayfa size ***özel şablon oluşturma*** veya ***önceden yapılandırılmış bir cihaz şablonu kullanma*** seçeneği sunar.
1. Aşağıda gösterildiği gibi önceden yapılandırılmış cihaz şablonları listesinden RuuviTag cihaz şablonunu seçin: ![RuuviTag cihaz şablonunu seçin](./media/howto-connect-ruuvi/devicetemplate-preconfigured.png)
1. Sonraki adıma devam etmek için Ileri 'yi seçin ***: Özelleştir*** . 
1. Bir sonraki ekranda, C500 cihaz şablonunu IoT Central uygulamanıza eklemek için ***Oluştur*** ' u seçin.

## <a name="connect-a-ruuvitag-sensor"></a>RuuviTag algılayıcısı bağlama

Yukarıda belirtildiği gibi, RuuviTag 'i IoT Central uygulamanızla bağlamak için bir ağ geçidi cihazı oluşturmanız gerekir. Aşağıdaki adımlarda, bir Rigado basamaklı 500 ağ geçidi cihazı oluşturduğunuzu varsayacağız.  

1. Rigado Cascade 500 cihazınızda güç yapın ve ağ bağlantınıza bağlayın (Ethernet veya kablosuz aracılığıyla)
1. RuuviTag 'in kapağını kapatın ve pille bağlantıyı güvenli hale getirmek için plastik sekmesini çekin. 
1. RuuviTag ' i daha önce IoT Central uygulamanızla yapılandırılmış olan Rigado Cascade 500 ağ geçidinize yakın bir yere yerleştirin. 
1. Yalnızca birkaç saniye içinde, RuuviTag 'niz IoT Central içindeki cihazlar listenizde görünmelidir.  
![Ruuvıtag cihaz listesi](./media/howto-connect-ruuvi/ruuvi-devicelist.png) artık IoT Central uygulamanızda bu RuuviTag ' i kullanabilirsiniz.  

## <a name="create-a-simulated-ruuvitag"></a>Benzetimli RuuviTag oluştur
Fiziksel bir RuuviTag size kullanışlı değilse, Azure IoT Central uygulamanızda test için kullanılacak bir benzetimli Ruuvıtag algılayıcısı oluşturabilirsiniz.

Benzetimli RuuviTag oluşturmak için:

1. **Cihaz > RuuviTag**' ı seçin. 
1. **+ Yeni**seçeneğini belirleyin. 
1. Benzersiz bir **CIHAZ kimliği** ve kolay bir **Cihaz adı**belirtin.  
1. **Benzetimli** ayarı etkinleştirin.
1. **Oluştur**'u seçin.  

## <a name="next-steps"></a>Sonraki Adımlar

Azure IoT Central uygulamanıza bir RuuviTag bağlamayı öğrendiğinize göre, önerilen sonraki adım bir uçtan uca çözüm oluşturmak için [IoT Central uygulamanızı nasıl özelleştireceğinizi](../retail/tutorial-in-store-analytics-customize-dashboard-pnp.md) öğrenmektedir. 