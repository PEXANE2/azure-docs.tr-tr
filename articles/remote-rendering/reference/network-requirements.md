---
title: Ağ gereksinimleri
description: En iyi deneyim için ağ gereksinimleri ve en iyi ağ uygulamaları
author: florianborn71
ms.author: flborn
ms.date: 02/13/2020
ms.topic: reference
ms.openlocfilehash: 9bb8818d2bb91c14eafc272774e0b633c51e58e1
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81617889"
---
# <a name="network-requirements"></a>Ağ gereksinimleri

Azure uzaktan Işlemede iyi bir kullanıcı deneyimi için, Azure veri merkezine yönelik kararlı, düşük gecikmeli bir ağ bağlantısı kritik öneme sahiptir. Kötü ağ koşulları, bağlantı, dengesiz, jaray veya ' atlama ' holograflarını ve sunucu tarafı sahne grafiğini güncelleştirirken dikkat çekici gecikme oluşmasına neden olabilir.

## <a name="guidelines-for-network-connectivity"></a>Ağ bağlantısı için yönergeler

Tam ağ gereksinimleri, uzaktan sahne grafiğinde yapılan değişikliklerin sayısı ve sıklığı ile işlenmiş görünümün karmaşıklığı gibi belirli bir kullanım örneğine bağlıdır, ancak deneyiminizin mümkün olduğunca iyi olduğundan emin olmak için çeşitli yönergeler vardır:

* Internet bağlantınızın, ağda rekabet eden bir trafik olmadığı varsayılarak, Azure uzaktan oluşturma işleminin tek bir kullanıcı oturumunda en az **50 Mbps 'lik aşağı akış** ve **10 Mbps 'lik yukarı akış** desteği sağlaması gerekir. Daha iyi deneyimler için daha yüksek ücretler öneririz. Aynı ağda daha fazla kullanıcı varsa, bu gereksinimlerin ölçeği aynı şekilde ölçeklendirilir.
* **5 GHz Wi-Fi bandı** kullanmak genellikle 2,4 GHz Wi-Fi bandından daha iyi sonuçlar üretir, ancak her ikisi de çalışır.
* Yakında başka Wi-Fi ağları varsa, bu diğer ağlar tarafından kullanılan Wi-Fi kanallarını kullanmaktan kaçının. Wi-Fi ağınızın kullandığı kanalların, rekabet eden trafiğin ücretsizdir olup olmadığını doğrulamak için [WifiInfoView](https://www.nirsoft.net/utils/wifi_information_view.html) gibi ağ tarama araçlarını kullanabilirsiniz.
* **Wi-Fi repeaters** veya ağ üzerinden-Powerline iletmeyi kullanmaktan kesinlikle kaçının.
* Aynı Wi-Fi ağında video veya oyun akışı gibi **rekabet bant genişliği güçlü trafiğinden kaçının** .
* **Iyi Wi-Fi sinyal gücüne** sahip olmak önemlidir. Mümkünse, Wi-Fi erişim noktalarınıza yakın kalın ve istemci cihazınız ile erişim noktaları arasında engelleri önleyin.
* [Bölgeniz](regions.md)için her zaman **en yakın Azure veri merkezine** bağlandığınızdan emin olun. Veri merkezine yaklaşarak, hologram kararlılığı üzerinde büyük bir etkiye sahip olan ağ gecikmesi düşüktür.

> [!NOTE]
> Aşağı akış bant genişliği çoğunlukla video akışı tarafından tüketilecektir. Bu, sırayla renk ve derinlik bilgileri (60 Hz, stereo) arasında bölünür.

## <a name="network-performance-tests"></a>Ağ performans testleri

Ağ bağlantınızın kalitesinin Azure uzaktan Işleme çalıştırmak için yeterli olup olmadığını anlamak istiyorsanız, kullanabileceğiniz mevcut çevrimiçi araçlar vardır. Azure uzaktan Işleme istemci uygulamanızı çalıştırmayı planladığınız cihazla, bu çevrimiçi araçların aynı Wi-Fi ' y e bağlı olan makul bir dizüstü bilgisayardan çalıştırılmasını önemle öneririz. Bir cep telefonunda veya HoloLens2 testlerin çalıştırılması sonucunda elde edilen sonuçlar, düşük güç destekli uç nokta cihazlarında önemli bir çeşitleme göstermek için kanıtlanmış bir şekilde daha az yararlıdır. Dizüstü bilgisayarı yerleştirdiğiniz konum kabaca, Azure uzaktan Işleme istemci uygulamanızı çalıştıran cihazı kullanmayı düşündüğünüz yerde kabaca olmalıdır.

Ağ bağlantınızın hızlı bir testi için birkaç basit adım aşağıda verilmiştir:

1. **Ağ bağlantınızın genel gecikme süresi ve yukarı akış/aşağı akış bant genişliği hakkında veri almak için www.speedtest.net gibi bir ağ testi aracı çalıştırın.**
Size en yakın sunucuyu seçin ve testi çalıştırın. Sunucu, Azure uzaktan Işlemenin bağlanacağı Azure veri merkezi olmayacak olsa da, sonuçta elde edilen veriler Internet bağlantınızın ve Wi-Fi 'nizin performansını anlamak için hala yararlıdır.
   * Azure uzaktan Işleme için **En düşük gereksinim** : yaklaşık 40 Mbps aşağı akış ve 5 Mbps yukarı akış.
   * Azure uzaktan Işleme için **önerilir** : yaklaşık 100 Mbps aşağı akış ve 10 Mbps yukarı akış.
Testi birden çok kez çalıştırmayı ve en kötü sonuçları almayı öneririz.
1. **Azure veri merkezlerine gecikme süresi ölçen www.azurespeed.com gibi bir araç kullanın**. Size en yakın Azure uzaktan Işleme tarafından desteklenen Azure veri merkezini seçin ( [desteklenen bölgelere](regions.md)bakın) ve bir **gecikme süresi testi**çalıştırın. Gördüğünüz sayılarda çeşitleme varsa, sonuçların sabitlemesini biraz zaman sunun.
   * Azure uzaktan Işleme için **En düşük gereksinim** : gecikme süresi sürekli olarak 100 ms 'den az olmalıdır.
   * Azure uzaktan Işleme için **önerilir** : gecikme süresi sürekli olarak 70 MS 'den az olmalıdır.

Düşük gecikme süresi, Azure uzaktan Işlemenin ağınızda iyi çalışacağı garantisi olmadığından, genellikle bu testlerin başarıyla geçirildiği durumlarda iyi bir şekilde karşılaştık.
Azure uzaktan Işleme çalıştırırken dengesiz, jtery veya atlama gibi yapıtlardan karşılaşdıysanız, [sorun giderme kılavuzuna](../resources/troubleshoot.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar

* [Hızlı başlangıç: Unity ile model Işleme](../quickstarts/render-model.md)
