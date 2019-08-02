---
title: Nesnelerin İnterneti En Iyi güvenlik uygulamaları | Microsoft Docs
description: Makale, Microsoft Nesnelerin İnterneti En Iyi güvenlik uygulamaları ve genel önerilerin seçkin bir listesini sağlar.
services: security
documentationcenter: na
author: barclayn
manager: barbkess
editor: TomSh
ms.assetid: 2d5598c5-4c30-481d-b8f4-51ee024ea9a7
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/26/2018
ms.author: barclayn
ms.openlocfilehash: 14f1905b1acd60acdf7b3dad0686e3fa34629953
ms.sourcegitcommit: 85b3973b104111f536dc5eccf8026749084d8789
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2019
ms.locfileid: "68727047"
---
# <a name="internet-of-things-security-best-practices"></a>Nesnelerin İnterneti En Iyi güvenlik uygulamaları

Nesnelerin İnterneti (IoT) altyapısının güvenliğini sağlamak, IoT çözümlerine katılan herkese karşı kritik öneme sahiptir. Söz konusu cihazların sayısı ve bu cihazların dağıtılmış doğası nedeniyle, Milyonlarca IoT cihazıdır. bir güvenlik olayının etkisi önemsiz değildir ve çok daha geniş bir etkiye sahip olabilir.

Bu nedenle, IoT Security 'nin derinlemesine bir güvenlik yaklaşımı olması gerekir. Verilerin bulutta güvenli olması gerekir ve özel ve ortak ağların üzerine taşınıyor. IoT cihazlarının kendilerine güvenli bir şekilde sağlanması için yöntemlerin yerinde olması gerekir. Cihazdan ve ağdan buluta arka uca kadar olan her katmanın güçlü güvenlik kesintileri olması gerekir.

IoT en iyi uygulamaları aşağıdaki şekilde kategorize edilebilir:

* IoT donanım üreticisi veya tümleştirici
* IoT çözüm geliştiricisi
* IoT çözüm dağıtıcı
* IoT çözüm operatörü

Bu makalede [En Iyi güvenlik uygulamaları nesnelerin interneti](/azure/iot-fundamentals/iot-security-best-practices)özetlenmektedir. Daha ayrıntılı bilgi için lütfen bu makaleye bakın.

## <a name="iot-hardware-manufacturer-or-integrator"></a>IoT donanım üreticisi veya tümleştirici

IoT donanım üretimi veya donanım tümleştirici kullanıyorsanız aşağıdaki en iyi uygulamaları izleyin:

* **En düşük gereksinimlere sahip donanım kapsamı**: donanım tasarımı, donanımın çalışması için gereken en düşük özellikleri ve başka hiçbir şeyi içermelidir. 
* **Donanım üzerinde oynama sağlaması yapın**: cihaz kapağını açma, cihazın bir kısmını kaldırma, vb. gibi fiziksel donanım kullanımını algılamak için mekanizmalarda derleme yapın. 
* **Güvenli donanım etrafında oluşturun**: [SMM](https://en.wikipedia.org/wiki/Cost_of_goods_sold) izin veriyorsa, güvenli ve şifreli depolama ve Güvenilir Platform Modülü (TPM) tabanlı önyükleme işlevselliği gibi güvenlik özellikleri oluşturun.
* **Yükseltmeleri güvenli yapın**: cihazın kullanım ömrü boyunca üretici yazılımının yükseltilmesi, çok daha fazla tablodur.

## <a name="iot-solution-developer"></a>IoT çözüm geliştiricisi

IoT çözüm geliştiricisiyseniz aşağıdaki en iyi uygulamaları izleyin:

* **Güvenli yazılım geliştirme metodolojisini takip edin**: güvenli yazılım geliştirme, projenin uygulama, test ve dağıtımına kadar her şey için bir güvenlik hakkında bilgi sahibi olmanızı gerektirir.
* **Açık kaynak yazılım**' i seçin: açık kaynak yazılım, çözümleri hızlı bir şekilde geliştirmeye yönelik bir fırsat sağlar.
* **Dikkatli tümleştirin**: kitaplıkların ve API 'lerin sınırında birçok yazılım güvenlik kusurı vardır. 

## <a name="iot-solution-deployer"></a>IoT çözüm dağıtıcı

IoT çözümü dağıtıcı kullanıyorsanız aşağıdaki en iyi uygulamaları izleyin:

* **Donanımı güvenli bir şekilde dağıtın**: IoT dağıtımları, donanımın genel boşluklar veya denetimli yerel ayarlarda olduğu gibi güvenli olmayan konumlara dağıtılması gerekebilir.
* **Kimlik doğrulama anahtarlarını güvenli tut**: dağıtım sırasında her cihaz, bulut hizmeti tarafından oluşturulan cihaz kimliklerini ve ilişkili kimlik doğrulama anahtarlarını gerektirir. Dağıtımdan sonra bile bu anahtarları fiziksel olarak güvende tutun. Kötü amaçlı bir cihaz, var olan bir cihaz olarak maske sağlamak için kullanılabilir.

## <a name="iot-solution-operator"></a>IoT çözüm operatörü

IoT çözüm operatörünüz varsa aşağıdaki en iyi uygulamaları izleyin:

* **Sistemleri güncel tutun**: cihaz işletim sistemlerinin ve tüm cihaz sürücülerinin en son sürümlere güncelleştirildiğinden emin olun. 
* **Kötü amaçlı etkinliğe karşı koruma**: işletim sistemi izin veriyorsa, her cihaz işletim sistemine en son virüsten koruma ve kötü amaçlı yazılımdan koruma özelliklerini yerleştirin. 
* **Denetim genellikle**: güvenlik olaylarına yanıt verme sırasında güvenlik ile ilgili sorunlar için IoT altyapısını denetleme anahtarıdır.
* **IoT altyapısını fiziksel olarak koruyun**: IoT altyapısına karşı en kötü güvenlik saldırıları cihazlara fiziksel erişim kullanılarak başlatılır.
* **Bulut kimlik bilgilerini koruma**: IoT dağıtımını yapılandırmak ve çalıştırmak için kullanılan bulut kimlik doğrulama kimlik bilgileri, büyük olasılıkla bir IoT sistemine erişmek ve bu sisteme zarar vermek için en kolay yoldur. 

