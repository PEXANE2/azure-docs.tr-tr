---
title: Azure IoT Hub ileti zenginleştirmelerine genel bakış
description: Bu makalede, IoT Hub'a iletiler belirlenen bitiş noktasına gönderilmeden önce iletileri ek bilgilerle damgalayabilme olanağı veren ileti zenginleştirmeleri gösterilmektedir.
author: robinsh
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 05/10/2019
ms.author: robinsh
ms.openlocfilehash: c3dbd01faf61c164c88f09b0da03c07be4abd187
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75429116"
---
# <a name="message-enrichments-for-device-to-cloud-iot-hub-messages"></a>Aygıttan buluta IoT Hub iletileri için ileti zenginleştirmeleri

*İleti zenginleştirmeleri,* IoT Hub'ının iletiler belirlenen bitiş noktasına gönderilmeden önce iletileri ek bilgilerle *damgalayabilme* yeteneğidir. İleti zenginleştirmelerini kullanmanın bir nedeni, akış aşağı işlemini kolaylaştırmak için kullanılabilecek verileri eklemektir. Örneğin, aygıt telemetri iletilerini aygıt ikiz etiketiyle zenginleştirmek, bu bilgiler için aygıt ikiz API çağrıları yapmak için müşterilerüzerindeki yükü azaltabilir.

![İleti zenginleştirmeleri akışı](./media/iot-hub-message-enrichments-overview/message-enrichments-flow.png)

İleti zenginleştirme nin üç temel öğesi vardır:

* Zenginleştirme adı veya anahtarı

* Bir değer

* Zenginleştirmenin uygulanması gereken bir veya daha fazla [uç nokta.](iot-hub-devguide-endpoints.md)

Anahtar **key** bir dize. Bir anahtar yalnızca alfasayısal karakterler veya bu özel karakterleri içerebilir: tire`-``_`( ),`.`alt çizgi ( ), ve dönem ( ).

**Değer** aşağıdaki örneklerden biri olabilir:

* Herhangi bir statik dize. Koşullar, mantık, işlemler ve işlevler gibi dinamik değerlere izin verilmez. Örneğin, birkaç müşteri tarafından kullanılan bir SaaS uygulaması geliştirirseniz, her müşteriye bir tanımlayıcı atayabilir ve bu tanımlayıcıyı uygulamada kullanılabilir hale getirebilirsiniz. Uygulama çalıştığında, IoT Hub aygıt telemetri iletilerini müşterinin tanımlayıcısıyla damgalayacak ve iletilerin her müşteri için farklı şekilde işlenmesini mümkün kılar.

* İletiyi gönderen IoT hub'ının adı. Bu değer *$iothubname.*

* Yol gibi aygıt ikizinden gelen bilgiler. Örnekler *$twin.tags.field* ve *$twin.tags.latitude*olacaktır.

   > [!NOTE]
   > Şu anda, ileti zenginleştirme için yalnızca $iothubname, $twin.etiketleri, $twin.properties.desired ve $twin.properties.reported desteklenen değişkenlerdir.

İleti Zenginleştirmeleri, seçilen uç nokta(lar)a gönderilen iletilere uygulama özellikleri olarak eklenir.  

## <a name="applying-enrichments"></a>Zenginleştirmelerin uygulanması

İletiler, aşağıdaki örnekler de dahil olmak üzere [IoT Hub ileti yönlendirmesi](iot-hub-devguide-messages-d2c.md)tarafından desteklenen herhangi bir veri kaynağından gelebilir:

* sıcaklık veya basınç gibi cihaz telemetrisi
* cihaz ikiz değiştirme bildirimleri - aygıt ikiz değişiklikleri
* aygıtın oluşturulması veya silinmesi gibi aygıt yaşam döngüsü olayları

Bir IoT Hub'ının yerleşik bitiş noktasına giden iletilere veya Azure Blob depolama, Hizmet Veri Yolu sırası veya Hizmet Veri Yolu konusu gibi özel uç noktalara yönlendirilen iletilere zenginleştirmeler ekleyebilirsiniz.

Bitiş noktasını Olay Izgarası olarak seçerek Olay Izgarası'na yayımlanmakta olan iletilere zenginleştirmeler ekleyebilirsiniz. IoT Hub'da Olay Ağı aboneliğinize dayalı olarak aygıt telemetrisine varsayılan bir rota oluştururuz. Bu tek rota, tüm Olay Izgara aboneliklerinizi işleyebilir. Aygıt telemetrisine olay ızgarası aboneliğini oluşturduktan sonra olay ızgarası bitiş noktası için zenginleştirmeleri yapılandırabilirsiniz. Daha fazla bilgi için [Iot Hub ve Olay Izgara'ya](iot-hub-event-grid.md)bakın.

Zenginleştirmeler uç nokta başına uygulanır. Belirli bir bitiş noktası için damgalanacak beş zenginleştirme belirtirseniz, bu bitiş noktasına giden tüm iletiler aynı beş zenginleştirmeyle damgalanır.

Zenginleştirmeler aşağıdaki yöntemlerkullanılarak yapılandırılabilir:

| **Yöntem** | **Komut** |
| ----- | -----| 
| Portal | [Azure portalında](https://portal.azure.com) | İleti [zenginleştirme ler öğretici](tutorial-message-enrichments.md) bakın | 
| Azure CLI   | [az iot hub mesaj zenginleştirme](https://docs.microsoft.com/cli/azure/iot/hub/message-enrichment?view=azure-cli-latest) |
| Azure PowerShell | [Add-AzIotHubMessageEnrichment](https://docs.microsoft.com/powershell/module/az.iothub/add-aziothubmessageenrichment?view=azps-2.8.0) |

İleti zenginleştirmeleri eklemek, ileti yönlendirmesine gecikme eklemez.

İleti zenginleştirmelerini denemek için [ileti zenginleştirmeleri öğreticisine](tutorial-message-enrichments.md) bakın

## <a name="limitations"></a>Sınırlamalar

* Standart veya temel katmandaki hub'lar için IoT Hub başına en fazla 10 zenginleştirme ekleyebilirsiniz. Ücretsiz katmandaki IoT Hub'ları için en fazla 2 zenginleştirme ekleyebilirsiniz.

* Bazı durumlarda, aygıt ikizinde bir etikete veya özellik tesibe ayarlanmış bir değerle zenginleştirme uyguluyorsanız, değer dize değeri olarak damgalanır. Örneğin, bir zenginleştirme değeri $twin.tags.field olarak ayarlanırsa, iletiler ikizden bu alanın değeri yerine "$twin.tags.field" dizesi ile damgalanır. Bu, aşağıdaki durumlarda olur:

   * IoT Hub'ınız temel katmandadır. Temel katman IoT hub'ları aygıt ikizlerini desteklemez.

   * IoT Hub'ınız standart katmandadır, ancak iletiyi gönderen aygıtın aygıt ikizi yoktur.

   * IoT Hub'ınız standart katmandadır, ancak zenginleştirme değeri için kullanılan aygıt ikiz yolu yoktur. Örneğin, zenginleştirme değeri $twin.tags.location olarak ayarlanmışsa ve aygıt ikizinin etiketlerin altında bir konum özelliği yoksa, ileti "$twin.tags.location" dizesiyle damgalanır. 

* Aygıt ikizine yapılan güncelleştirmelerin ilgili zenginleştirme değerine yansıtılması beş dakika kadar sürebilir.

* Zenginleştirmeler de dahil olmak üzere toplam ileti boyutu 256 KB'yi geçemez. İleti boyutu 256 KB'yi aşarsa, IoT Hub iletiyi bırakır. İletiler bırakıldığında hataları tanımlamak ve hata ayıklamak için [IoT Hub ölçümlerini](iot-hub-metrics.md) kullanabilirsiniz. Örneğin, d2c.telemetry.egress.geçersiz izleyebilirsiniz.

* İleti zenginleştirmeleri dijital ikiz değiştirme etkinlikleri için geçerli değildir [(IoT Tak ve Genel Önizlemesini Çalıştır'ın](../iot-pnp/overview-iot-plug-and-play.md)bir parçası).

## <a name="pricing"></a>Fiyatlandırma

İleti zenginleştirmeleri ek ücret ödemeden kullanılabilir. Şu anda, bir IoT Hub'ına ileti gönderdiğinde ücretlendirilirsiniz. İleti birden çok uç noktaya gitse bile, bu ileti için yalnızca bir kez ücretlendirilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

İletileri Bir IoT Hub'ına yönlendirme hakkında daha fazla bilgi için bu makalelere göz atın:

* [İleti zenginleştirmeler öğretici](tutorial-message-enrichments.md)

* [Aygıttan buluta iletileri farklı uç noktalara göndermek için IoT Hub ileti yönlendirmesini kullanma](iot-hub-devguide-messages-d2c.md)

* [Öğretici: IoT Hub yönlendirme](tutorial-routing.md)
