---
title: Ağ gereksinimleri
description: Optimum deneyim için ağ gereksinimleri ve en iyi ağ uygulamaları
author: florianborn71
ms.author: flborn
ms.date: 02/13/2020
ms.topic: reference
ms.openlocfilehash: 73bbfad4c0535fa00b1aa53764eb52acb83124f8
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80680381"
---
# <a name="network-requirements"></a>Ağ gereksinimleri

Azure veri merkezine kararlı, düşük gecikmeli ağ bağlantısı, Azure Uzaktan İşleme'de iyi bir kullanıcı deneyimi için çok önemlidir. Kötü ağ koşulları, bağlantıların düşmesine, kararsız, gergin veya 'atlama' hologramlarına ve sunucu tarafındaki sahne grafiğini güncellerken fark edilebilir gecikmeye neden olabilir.

## <a name="guidelines-for-network-connectivity"></a>Ağ bağlantısı için yönergeler

Tam ağ gereksinimleri, uzak sahne grafiğindeki değişikliklerin sayısı ve sıklığı nın yanı sıra işlenen görünümün karmaşıklığı gibi özel kullanım durumunuza bağlıdır, ancak deneyiminizin mümkün olduğunca iyi olduğundan emin olmak için bir dizi yönerge vardır:

* Ağda rakip trafik olmadığını varsayarak, internet bağlantınızın azure Uzaktan İşlem'in tek bir kullanıcı oturumu için en az **50 Mbps akış** aşağı ve **10 Mbps upstream'i** tutarlı bir şekilde desteklemesi gerekir. Daha iyi deneyimler için daha yüksek fiyatlar öneriyoruz. Aynı ağdaki daha fazla kullanıcıyla, bu gereksinimler buna bağlı olarak ölçeklendirilir.
* **5-GHz Wi-Fi bandı** kullanarak genellikle 2.4-GHz Wi-Fi bandı daha iyi sonuçlar üretecek, her ikisi de çalışması gerekir rağmen.
* Yakınlarda başka Wi-Fi ağları varsa, bu diğer ağlar tarafından kullanılan Wi-Fi kanallarını kullanmaktan kaçının. Wi-Fi ağınızın kullandığı kanalların rakip trafikten uzak olup olmadığını doğrulamak için [WifiInfoView](https://www.nirsoft.net/utils/wifi_information_view.html) gibi ağ tarama araçlarını kullanabilirsiniz.
* **Wi-Fi tekrarlayıcıları** veya LAN-over-powerline iletme kullanmaktan kesinlikle kaçının.
* Aynı Wi-Fi ağında, video veya oyun akışı gibi **bant genişliği yoğun trafikten kaçının.**
* **İyi Wi-Fi sinyal gücüne** sahip olmak esastır. Mümkünse Wi-Fi erişim noktanıza yakın durun ve istemci aygıtınızla erişim noktaları arasındaki engelleri önleyebilirsiniz.
* [Bölgeniz](regions.md)için her zaman **en yakın Azure veri merkezine** bağlandığınızdan emin olun. Veri merkezi ne kadar yakınsa, ağ gecikmesi de o kadar düşük olur ve bu da hologram kararlılığı üzerinde büyük bir etkiye sahiptir.

## <a name="network-performance-tests"></a>Ağ performans testleri

Ağ bağlantınızın kalitesinin Azure Uzaktan İşleme'yi çalıştırmak için yeterli olup olmadığını anlamak istiyorsanız, kullanabileceğiniz mevcut çevrimiçi araçlar vardır. Azure Uzaktan İşlem istemcisi uygulamanızı çalıştırmayı planladığınız aygıtla aynı Wi-Fi'a bağlı oldukça güçlü bir dizüstü bilgisayardan bu çevrimiçi araçları çalıştırmanızı şiddetle öneririz. Düşük güçlü uç nokta cihazlarında önemli farklılıklar gösterdiği kanıtlanan testlerin bir cep telefonu veya HoloLens2 üzerinde çalıştırılan sonuçları genellikle daha az yararlıdır. Dizüstü bilgisayarı yerleştirdiğiniz konum kabaca Azure Uzaktan İşlem istemcisi uygulamanızı çalıştıran aygıtı kullanmayı beklediğiniz yerde olmalıdır.

Ağ bağlantınızın hızlı bir şekilde sınanmasına yönelik birkaç basit adım şunlardır:

1. **Ağ bağlantınızın genel gecikme sağlığı ve yukarı/alt bant width hakkında veri almak için www.speedtest.net gibi bir ağ test aracını çalıştırın.**
Size en yakın sunucuseçin ve testi çalıştırın. Sunucu Azure Uzaktan İşleme'nin bağlayacağı Azure veri merkezi olmasa da, ortaya çıkan veriler internet bağlantınızın ve Wi-Fi'ınızın performansını anlamak için yine de yararlıdır.
   * Azure Uzaktan İşleme için **minimum gereksinim:** Yaklaşık 40 Mbps aşağı akış ve 5 Mbps yukarı akış.
   * Azure Uzaktan İşleme için **önerilir:** Yaklaşık 100 Mbps aşağı akış ve 10 Mbps yukarı akış.
Testi birden çok kez çalıştırmanızı ve en kötü sonuçları almanızı öneririz.
1. **Azure veri merkezlerine gecikmeyi ölçen www.azurespeed.com gibi bir araç kullanın.** Size en yakın Azure Uzaktan İşleme tarafından desteklenen Azure veri merkezini seçin [(desteklenen bölgelere](regions.md)bakın) ve bir **gecikme testi**çalıştırın. Gördüğünüz sayılarda bir değişiklik varsa, sonuçları stabilize etmek için biraz zaman verin.
   * Azure Uzaktan İşleme için **minimum gereksinim:** Gecikme sürekli olarak 100 ms'den az olmalıdır.
   * Azure Uzaktan İşleme için **önerilir:** Gecikme sürekli olarak 70 ms'den az olmalıdır.

Düşük gecikme cezası, Azure Uzaktan İşleme'nin ağımızda iyi çalışacağının garantisi olmasa da, genellikle bu testlerin başarılı bir şekilde geçtiği durumlarda iyi performans gösterdiğini gördük.
Azure Uzaktan İşleme'yi çalıştırırken kararsız, gergin veya atlama hologramları gibi yapılarla karşılaşıyorsanız, [sorun giderme kılavuzuna](../resources/troubleshoot.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar

* [Quickstart: Unity ile bir model oluşturma](../quickstarts/render-model.md)
