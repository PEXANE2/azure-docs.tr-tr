---
title: Azure IoT Edge nedir | Microsoft Docs
description: Azure IoT Edge hizmetine genel bakış
author: kgremban
manager: philmea
ms.reviewer: chipalost
ms.service: iot-edge
services: iot-edge
ms.topic: overview
ms.date: 10/28/2019
ms.author: kgremban
ms.custom: mvc
ms.openlocfilehash: 1ba133acda414d9779e2fb10150bbdd57285e9a5
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "76511221"
---
# <a name="what-is-azure-iot-edge"></a>Azure IoT Edge nedir?

Azure IoT Edge bulut analizini ve özel iş mantığını cihazlara taşıyarak kuruluşunuzun veri yönetimi yerine iş öngörülerine odaklanmasını sağlar. İş mantığınızı standart kaplara paketleyerek IoT çözümünüzü ölçeklendirin, ardından bu kapları aygıtlarınızdan herhangi biri için dağıtabilir ve buluttan izleyebilirsiniz.

Analizler IoT çözümlerinin iş değerini artırır, aman tüm analizlerin bulutta olması gerekmez. Acil durumlara mümkün olduğunca çabuk yanıt vermek istiyorsanız, anomali algılama iş yüklerini en uçta çalıştırabilirsiniz. Bant genişliği maliyetlerini azaltmak ve terabaytlarca ham veri aktarmaktan kaçınmak istiyorsanız, verileri yerel olarak temizleyebilir ve toplayabilir, ardından istatistikleri yalnızca analiz için buluta gönderebilirsiniz.

Azure IoT Edge üç bileşenden oluşur:

* **IoT Edge modülleri,** Azure hizmetlerini, üçüncü taraf hizmetlerini veya kendi kodunuzu çalıştıran kapsayıcılardır. Modüller IoT Edge cihazlarına dağıtılır ve bu cihazlarda yerel olarak yürütülür.
* **IoT Edge çalışma süresi** her IoT Edge aygıtında çalışır ve her cihaza dağıtılan modülleri yönetir.
* **Bulut tabanlı bir arayüz,** IoT Edge aygıtlarını uzaktan izlemenizi ve yönetmeniz sağlar.

>[!NOTE]
>Azure IoT Edge, IoT Hub’ın ücretsiz ve standart katmanında kullanılabilir. Ücretsiz katman yalnızca test etme ve değerlendirme içindir. Temel ve standart katmanlar hakkında daha fazla bilgi için bkz. [Doğru IoT Hub katmanını seçme](../iot-hub/iot-hub-scaling.md).

## <a name="iot-edge-modules"></a>IoT Edge modülleri

IoT Edge modülleri, iş mantığınızı sınırda çalıştıran Docker uyumlu kapsayıcılar olarak uygulanan yürütme birimleridir. Birbiriyle iletişim kuracak birden çok modül yapılandırabilir ve böylelikle veri işleme için bir işlem hattı oluşturabilirsiniz. Çevrimdışı ve uçta öngörüler sağlayan kendi modüllerinizi geliştirebilir veya bazı Azure hizmetlerini modüller halinde paketleyebilirsiniz.

### <a name="artificial-intelligence-at-the-edge"></a>Kenarda yapay zeka

Azure IoT Edge, karmaşık olay işleme, makine öğrenimi, görüntü tanıma ve diğer yüksek değerli yapay aI'yi şirket içinde yazmadan dağıtmanıza olanak tanır. Azure İşlevleri, Azure Akış Analizi ve Azure Machine Learning gibi Azure hizmetlerinin tümü Azure IoT Edge üzerinden şirket içinde çalıştırılabilir. Ancak Azure hizmetleriyle sınırlı değilsiniz. Herkes AI modülleri oluşturabilir ve Azure Marketi aracılığıyla kullanılmak üzere topluluk için kullanılabilir hale getirebilir.

### <a name="bring-your-own-code"></a>Kendi kodunuzu getirin

Cihazlarınıza kendi kodunuzu dağıtmak istediğinizde, Azure IoT Edge bunu da destekler. Azure IoT Edge, diğer Azure IoT hizmetleriyle alnı programlama modeline sahiptir. Aynı kodu bir aygıtta veya bulutta çalıştırabilirsiniz. Azure IoT Edge hem Linux'ı hem de Windows'u desteklediğinden, kendi seçtiğiniz platform için kod yazabilirsiniz. Java, .NET Core 2.0, Node.js, C ve Python'u destekler, böylece geliştiricileriniz zaten bildikleri bir dilde kodlayabilir ve varolan iş mantığını kullanabilir.

## <a name="iot-edge-runtime"></a>IoT Edge çalışma zamanı

Azure IoT Edge çalışma zamanı, IoT Edge cihazlarında özel mantığa ve bulut mantığına olanak tanır. Çalışma süresi IoT Edge aygıtında yer alıyor ve yönetim ve iletişim işlemlerini gerçekleştirir. Çalışma zamanı çeşitli işlevler gerçekleştirir:

* Aygıta iş yüklerini yükler ve günceller.
* Cihazda Azure IoT Edge güvenlik standartlarını korur.
* IoT Edge modüllerinin her zaman çalıştırılmasını güvence altına alır.
* Uzaktan izleme için modül durumunu buluta bildirir.
* Akış aşağı yaprak aygıtları ile IoT Edge aygıtı arasındaki, Bir IoT Edge aygıtındaki modüller arasındaki ve bir IoT Edge aygıtı ile bulut arasındaki iletişimi yönetir.

![IoT Edge çalışma zamanı öngörüleri ve raporları IoT Hub'ına gönderir](./media/about-iot-edge/runtime.png)

Azure IoT Edge cihazını nasıl kullanacağınız size kalmış. Çalışma süresi genellikle diğer şirket içi aygıtlardan veri toplayan ve işleyen ağ geçidi aygıtlarına AI dağıtmak için kullanılır, ancak bu dağıtım modeli yalnızca bir seçenektir.

Azure IoT Edge çalışma süresi, çok çeşitli şekillerde kullanılmasını sağlayan çok çeşitli IoT aygıtları kümesinde çalışır. Hem Linux hem de Windows işletim sistemlerini ve donanım ayrıntılarını özetler. Çok fazla veri işlemetmiyorsanız Raspberry Pi 3'ten daha küçük bir aygıt kullanın veya kaynak yoğun iş yüklerini çalıştırmak için endüstriyel bir sunucu kullanın.

## <a name="iot-edge-cloud-interface"></a>IoT Edge bulut arabirimi

Genellikle farklı yapar ve modelleri veya coğrafi dağınık IoT cihazlar milyonlarca yazılım yaşam döngüsü yönetmek zordur. Belirli bir cihaz türü için iş yüklerinin oluşturulması ve yapılandırılması, tüm cihazlarınıza dağıtılması ve hatalı davranan cihazları yakalamak için izleme yapılması gerekir. Bu cihazlar tek tek cihazlar temelinde yapılamaz, belirli bir ölçekte yapılmalıdır.

Azure IoT Edge, Azure IoT çözüm hızlandırıcıları ile rahatça tümleştirildiğinden, çözümünüzün gereksinimlerini karşılayacak tek bir denetim düzlemi sağlar. Bulut hizmetleri şunları sağlar:

* Belirli bir tür cihaz üzerinde çalıştırılacak bir iş yükü oluşturabilir ve yapılandırılabilir.
* İş yükünü bir dizi cihaza gönderebilir.
* Sahadaki cihazlarda çalıştırılan iş yüklerini izleyebilir.

![Cihaz telemetrisi ve eylemleri bulutla koordine edilir](./media/about-iot-edge/cloud-interface.png)

## <a name="next-steps"></a>Sonraki adımlar

[Bir sanal cihaza IoT Edge dağıtımı yaparak](quickstart.md) bu kavramları deneyin.
