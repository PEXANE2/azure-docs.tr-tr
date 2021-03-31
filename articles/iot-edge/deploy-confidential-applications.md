---
title: Azure IoT Edge modüller olarak gizli uygulamalar
description: Gizli uygulamalar yazmak ve bunları gizli bilgi işlem için IoT Edge modülleri olarak dağıtmak üzere açık şifreleme SDK 'sını ve API 'sini kullanın
author: kgremban
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 01/27/2021
ms.author: kgremban
ms.openlocfilehash: f9dff1b4c6b2489edd3cd685e3546618961d9757
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2021
ms.locfileid: "103487726"
---
# <a name="confidential-computing-at-the-edge"></a>Kenarda gizli bilgi işlem

[!INCLUDE [iot-edge-version-all-supported](../../includes/iot-edge-version-all-supported.md)]

Azure IoT Edge, cihazda güvenli şifreleme kapsamında çalışan gizli uygulamaları destekler. Şifreleme, aktarım sırasında veya bekleyen sırada veriler için güvenlik sağlar, ancak şifreleme, kullanımda olan veriler ve iş yükleri için güvenlik sağlar. IoT Edge, gizli uygulamalar geliştirmek için standart olarak açık şifreleme desteği destekler.

Güvenlik, genellikle IoT cihazlarının bir özel tesis içinde güvenli hale getirilse de genellikle dünya genelinde kullanıldığı için Nesnelerin İnterneti (IoT) ' nin önemli bir odasına sahiptir. Bu pozlama, cihazları izinsiz değişikliklere karşı risk altına alır, çünkü bunlar, hatalı aktör için fiziksel olarak erişilebilirdir. IoT Edge cihazların, önemli iş yüklerinin kenarda çalışmasına izin verdiklerinden güven ve bütünlüğü daha da fazla olması gerekir. Yaygın sensörlerden ve kullanılcılardan farklı olarak, bu akıllı sınır cihazları, daha önce yalnızca korunan bulut veya şirket içi ortamlarda çalışan hassas iş yüklerini açığa çıkarmakta olabilir.

[IoT Edge Güvenlik Yöneticisi](iot-edge-security-manager.md) , gizli bilgi işlem Challenge 'ın bir parçasını ele alınmaktadır. Güvenlik Yöneticisi, kimlik iş yüklerini ve bir IoT Edge cihazının devam eden süreçlerini korumak için bir donanım güvenlik modülü (HSM) kullanır.

Gizli bilgi işlem kullanımının başka bir yönü de kullanılan verileri kenarda koruyor. *Güvenilir bir yürütme ortamı* (t), bir işlemcide güvenli, yalıtılmış bir ortamdır ve bazen bir *şifreleme* olarak adlandırılır. *Gizli bir uygulama* , bir kuşatma içinde çalışan bir uygulamadır. Şifreleme doğası nedeniyle, gizli uygulamalar, ana işlemcide veya t 'de çalışan diğer uygulamalardan korunur.

## <a name="confidential-applications-on-iot-edge"></a>IoT Edge üzerinde gizli uygulamalar

Gizli uygulamalar aktarım sırasında ve bekleyen ' de şifrelenir ve yalnızca güvenilir bir yürütme ortamında çalışmak üzere şifresi çözülür. Bu standart, IoT Edge modüller olarak dağıtılan gizli uygulamalar için doğru bir tutar.

Geliştirici gizli uygulamayı oluşturur ve bir IoT Edge modülü olarak paketler. Uygulama, kapsayıcı kayıt defterine gönderilmeden önce şifrelenir. Uygulama, IoT Edge cihazında modül başlatılana kadar IoT Edge dağıtım işlemi boyunca şifreli olarak kalır. Gizli uygulama cihazın t 'nin içindeyse, şifresi çözülür ve yürütülmeye başlayabilir.

![Diyagram-gizli olmayan uygulamalar, güvenli şifreleme içine dağıtılana kadar IoT Edge modüller içinde şifrelenir](./media/deploy-confidential-applications/confidential-applications-encrypted.png)

IoT Edge üzerindeki gizli uygulamalar, [Azure gizli bilgi işlem](../confidential-computing/overview.md)'nin mantıksal uzantısıdır. Buluttaki güvenli şifreleme dahilinde çalışan iş yükleri, uç tarafında güvenli kuşışları içinde çalışacak şekilde de dağıtılabilir.

## <a name="open-enclave"></a>Açık şifreleme

[Açık şifreleme SDK 'sı](https://openenclave.io/sdk/) , geliştiricilerin birden çok platform ve ortam için gizli uygulamalar oluşturmalarına yardımcı olan açık kaynaklı bir projem. Açık şifreleme SDK 'Sı, bir cihazın güvenilir yürütme ortamında çalışır, ancak açık şifreleme API 'SI, t ve t olmayan işlem ortamı arasında bir arabirim görevi görür.

Açık şifreleme birden çok donanım platformunu destekler. Şifreleme için IoT Edge desteği şu anda açık taşınabilir t işletim sistemi (OP-t OS) gerektirir. Daha fazla bilgi için bkz. [op-t OS Için açık ŞIFRELEME SDK](https://github.com/openenclave/openenclave/blob/master/docs/GettingStartedDocs/OP-TEE/Introduction.md).

Açık şifreleme deposu, geliştiricilerin başlamanıza yardımcı olacak örnekler de içerir. Daha fazla bilgi için, giriş makalelerinden birini seçin:

* [Linux üzerinde açık şifreleme SDK örnekleri oluşturma](https://github.com/openenclave/openenclave/blob/master/samples/BuildSamplesLinux.md)
* [Windows üzerinde açık şifreleme SDK örnekleri oluşturma](https://github.com/openenclave/openenclave/blob/master/samples/BuildSamplesWindows.md)

## <a name="hardware"></a>Donanım

Şu anda, [Scalys tarafından yapılan Trustbox](https://scalys.com/trustbox-industrial/) , IoT Edge modüller olarak gizli uygulamalar dağıtmaya yönelik üretici hizmeti sözleşmeleri ile desteklenen tek cihazdır. Trustbox Edge ve TrustBox EdgeXL cihazlarında, hem açık şifreleme, hem de Azure IoT Edge birlikte önceden yüklenmiş olarak sunulur.

Daha fazla bilgi için bkz. [Scalys TrustBox Için açık şifreleme ile çalışmaya](https://aka.ms/scalys-trustbox-edge-get-started)başlama.

## <a name="develop-and-deploy"></a>Geliştirme ve dağıtma

Gizli uygulamanızı geliştirmeye ve dağıtmaya hazırsanız, Visual Studio Code için [Microsoft açık şifreleme](https://marketplace.visualstudio.com/items?itemName=ms-iot.msiot-vscode-openenclave) uzantısı yardımcı olabilir. TrustBox için modüller geliştirmek üzere, geliştirme makineniz olarak Linux veya Windows 'u kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[Visual Studio Code Için açık şifreleme uzantısı](https://github.com/openenclave/openenclave/tree/master/devex/vscode-extension) ile IoT Edge modüller olarak gizli uygulamalar geliştirmeye nasıl başlayacağınızı öğrenin
