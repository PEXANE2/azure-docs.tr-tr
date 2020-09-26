---
title: Azure IoT Edge modüller olarak güvenilir uygulamalar
description: Açık şifreleme SDK 'sını ve API 'yi kullanarak güvenilir uygulamalar yazın ve bunları gizli bilgi işlem için IoT Edge modüller olarak dağıtın
author: kgremban
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 09/25/2020
ms.author: kgremban
ms.openlocfilehash: d81cc6c94c04c683362fd12cd6777c304a4b0a84
ms.sourcegitcommit: d95cab0514dd0956c13b9d64d98fdae2bc3569a0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91361985"
---
# <a name="confidential-computing-at-the-edge"></a>Kenarda gizli bilgi işlem

Azure IoT Edge, cihazda güvenli şifreleme kapsamında çalışan güvenilen uygulamaları destekler. Şifreleme, aktarım sırasında veya bekleyen sırada veriler için güvenlik sağlar, ancak şifreleme, kullanımda olan veriler ve iş yükleri için güvenlik sağlar. IoT Edge, güvenilir uygulamalar geliştirmeye yönelik standart olarak açık şifreleme desteği destekler.

Güvenlik, genellikle IoT cihazlarının bir özel tesis içinde güvenli hale getirilse de genellikle dünya genelinde kullanıldığı için Nesnelerin İnterneti (IoT) ' nin önemli bir odasına sahiptir. Bu pozlama, cihazları izinsiz değişikliklere karşı risk altına alır, çünkü bunlar, hatalı aktör için fiziksel olarak erişilebilirdir. IoT Edge cihazların, önemli iş yüklerinin kenarda çalışmasına izin verdiklerinden güven ve bütünlüğü daha da fazla olması gerekir. Yaygın sensörlerden ve kullanılcılardan farklı olarak, bu akıllı sınır cihazları, daha önce yalnızca korunan bulut veya şirket içi ortamlarda çalışan hassas iş yüklerini açığa çıkarmakta olabilir.

[IoT Edge Güvenlik Yöneticisi](iot-edge-security-manager.md) , gizli bilgi işlem Challenge 'ın bir parçasını ele alınmaktadır. Güvenlik Yöneticisi, kimlik iş yüklerini ve bir IoT Edge cihazının devam eden süreçlerini korumak için bir donanım güvenlik modülü (HSM) kullanır.

Gizli bilgi işlem kullanımının başka bir yönü de kullanılan verileri kenarda koruyor. *Güvenilir bir yürütme ortamı* (t), bir işlemcide güvenli, yalıtılmış bir ortamdır ve bazen bir *şifreleme*olarak adlandırılır. *Güvenilir bir uygulama* , bir kuşatma içinde çalışan bir uygulamadır. Şifreleme doğası nedeniyle, güvenilen uygulamalar, ana işlemcide veya t 'de çalışan diğer uygulamalardan korunur.

## <a name="trusted-applications-on-iot-edge"></a>IoT Edge güvenilir uygulamalar

Güvenilen uygulamalar aktarım sırasında ve bekleyen ' de şifrelenir ve yalnızca güvenilir bir yürütme ortamında çalışmak üzere şifresi çözülür. Bu standart, IoT Edge modüller olarak dağıtılan güvenilir uygulamalar için doğru bir tutar.

Geliştirici güvenilen uygulamayı oluşturur ve bir IoT Edge modülü olarak paketler. Uygulama, kapsayıcı kayıt defterine gönderilmeden önce şifrelenir. Uygulama, IoT Edge cihazında modül başlatılana kadar IoT Edge dağıtım işlemi boyunca şifreli olarak kalır. Güvenilen uygulama cihazın t 'nin içindeyse, şifresi çözülür ve yürütülmeye başlayabilir.

![Diyagram-güvenilir uygulamalar, güvenli kuşyana dağıtılana kadar IoT Edge modüller içinde şifrelenir](./media/deploy-trusted-applications/trusted-applications-encrypted.png)

IoT Edge güvenilir uygulamalar, [Azure gizli bilgi işlem](../confidential-computing/overview.md)'nin mantıksal uzantısıdır. Buluttaki güvenli şifreleme dahilinde çalışan iş yükleri, uç tarafında güvenli kuşışları içinde çalışacak şekilde de dağıtılabilir.

## <a name="open-enclave"></a>Açık şifreleme

[Açık şifreleme SDK 'sı](https://openenclave.io/sdk/) , geliştiricilerin birden çok platform ve ortam için güvenilir uygulamalar oluşturmalarına yardımcı olan açık kaynaklı bir projem. Açık şifreleme SDK 'Sı, bir cihazın güvenilir yürütme ortamında çalışır, ancak açık şifreleme API 'SI, t ve t olmayan işlem ortamı arasında bir arabirim görevi görür.

Açık şifreleme birden çok donanım platformunu destekler. Şifreleme için IoT Edge desteği şu anda açık taşınabilir t işletim sistemi (OP-t OS) gerektirir. Daha fazla bilgi için bkz. [op-t OS Için açık ŞIFRELEME SDK](https://github.com/openenclave/openenclave/blob/master/docs/GettingStartedDocs/OP-TEE/Introduction.md).

Açık şifreleme deposu, geliştiricilerin başlamanıza yardımcı olacak örnekler de içerir. Daha fazla bilgi için, giriş makalelerinden birini seçin:

* [Linux üzerinde açık şifreleme SDK örnekleri oluşturma](https://github.com/openenclave/openenclave/blob/master/samples/BuildSamplesLinux.md)
* [Windows üzerinde açık şifreleme SDK örnekleri oluşturma](https://github.com/openenclave/openenclave/blob/master/samples/BuildSamplesWindows.md)

## <a name="hardware"></a>Donanım

Şu anda, [Scalys tarafından yapılan Trustbox](https://scalys.com/trustbox-industrial/) , güvenilen uygulamaları IoT Edge modülleri olarak dağıtmaya yönelik üretici hizmeti sözleşmeleri ile desteklenen tek cihazdır. Trustbox Edge ve TrustBox EdgeXL cihazlarında, hem açık şifreleme, hem de Azure IoT Edge birlikte önceden yüklenmiş olarak sunulur.

Daha fazla bilgi için bkz. [Scalys TrustBox Için açık şifreleme ile çalışmaya](https://aka.ms/scalys-trustbox-edge-get-started)başlama.

## <a name="develop-and-deploy"></a>Geliştirme ve dağıtma

Güvenilen uygulamanızı geliştirmeye ve dağıtmaya hazırsanız, Visual Studio Code için [Microsoft açık şifreleme](https://marketplace.visualstudio.com/items?itemName=ms-iot.msiot-vscode-openenclave) uzantısı yardımcı olabilir. TrustBox için modüller geliştirmek üzere, geliştirme makineniz olarak Linux veya Windows 'u kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[Visual Studio Code Için açık şifreleme uzantısı](https://github.com/openenclave/openenclave/tree/master/devex/vscode-extension) ile IoT Edge modüller olarak güvenilir uygulamalar geliştirmeye nasıl başlayacağınızı öğrenin
