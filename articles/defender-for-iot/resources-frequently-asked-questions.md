---
title: IoT için Defender hakkında sık sorulan sorular
description: IoT özellikleri ve hizmeti için Azure Defender hakkında en sık sorulan soruların yanıtlarını bulun.
services: defender-for-iot
ms.service: defender-for-iot
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/07/2020
ms.author: rkarlin
ms.openlocfilehash: 5e7eabd44ea8c56fbb102f9e48812745a31de62a
ms.sourcegitcommit: 30505c01d43ef71dac08138a960903c2b53f2499
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2020
ms.locfileid: "92089205"
---
# <a name="azure-defender-for-iot-frequently-asked-questions"></a>IoT için Azure Defender hakkında sık sorulan sorular

Bu makalede, IoT için Defender ile ilgili sık sorulan soruların ve yanıtların bir listesi sunulmaktadır.

## <a name="what-is-azures-unique-value-proposition-for-iot-security"></a>Azure 'un IoT güvenliği için benzersiz değer teklifi nedir?

IoT için Defender, kuruluşların mevcut siber güvenlik görünümünü tüm IoT çözümüne genişletmelerini sağlar. Azure, iş çözümünüzün uçtan uca bir görünümünü sunarak kurumsal güvenlik duruşunuzu ve toplanan verileri temel alarak işle ilgili eylemler ve kararlar almanıza olanak sağlar. Azure IoT, Azure IoT Edge ve Azure Güvenlik Merkezi kullanılarak birleştirilmiş güvenlik, ihtiyacınız olan güvenlik ile istediğiniz çözümü oluşturmanızı sağlar.

## <a name="our-organization-uses-proprietary-non-standard-industrial-protocols-are-they-supported"></a>Kuruluşumuz standart olmayan bir endüstriyel protokol kullanıyor. Bunlar destekleniyor mu? 

IoT için Azure Defender kapsamlı protokol desteği sağlar. Gömülü protokol desteğine ek olarak, özel ve özel protokoller çalıştıran IoT ve OT cihazlarının yanı sıra herhangi bir standarda göre farklılık gösteren protokolleri güvenli hale getirebilirsiniz. Geliştiriciler, ufuk açık geliştirme ortamı (ODE) SDK 'sını kullanarak, tanımlı protokollere göre ağ trafiğinin kodunu çözen dissektör eklentileri oluşturabilir. Trafik, tüm izleme, uyarı ve raporlama sağlamak için hizmetler tarafından çözümlenir. Ufku kullanarak:
- Yeni sürümlere yükseltmeye gerek olmadan görünürlük ve denetim ' i genişletin.
- Şirket içinde bir dış eklenti oluşturarak özel bilgileri güvenli hale getirin. 
- Uyarılar, olaylar ve protokol parametreleri için metni yerelleştirin.

Eklentileri eklenti olarak geliştirmeye yönelik bu benzersiz çözüm, yeni bir protokolü desteklemek için adanmış geliştirici takımları veya sürüm sürümleri gerektirmez. Geliştiriciler, iş ortakları ve müşteriler ufku kullanarak protokolleri güvenle geliştirebilir ve Öngörüler ve bilgi paylaşabilir. 

## <a name="do-i-have-to-purchase-hardware-appliances-from-microsoft-partners"></a>Microsoft iş ortaklarından donanım gereçlerini satın almam gerekiyor mu?
IoT algılayıcısı için Azure Defender, [donanım belirtimleri Kılavuzu](https://aka.ms/AzureDefenderforIoTBareMetalAppliance)' nda açıklandığı gibi belirli donanım özellikleri üzerinde çalışır, müşteriler Microsoft iş ortaklarından sertifikalı donanımlar satın alabilir veya sağlanan ürün REÇETELERINI (BOM) kullanabilir ve kendi kendinize satın alabilir. 

Sertifikalı donanım, laboratuvarlarımızda sürücü kararlılığı, paket bırakılanlar ve ağ boyutlandırma için test edilmiştir.


## <a name="regulation-does-not-allow-us-to-connect-our-system-to-the-internet-can-we-still-utilize-defender-for-iot"></a>Yönetmeliği, sistemimizi Internet 'e bağlayamamıza izin vermiyor. Hala IoT için Defender 'ı kullanabilir miyim?

Evet! IoT Platformu için Azure Defender çözümü, ağ trafiğini (SPAN, RSPAN veya TAP aracılığıyla), BT ve IoT ağlarını analiz etmek, keşfetmeyi ve sürekli olarak izlemeyi sağlayan bir fiziksel veya sanal algılayıcı gereci olarak dağıtılır. Daha büyük kuruluşlar için birden çok algılayıcı, verilerini şirket içi yönetim konsoluna toplayabilirler.

## <a name="where-in-the-network-should-i-connect-monitoring-ports"></a>Ağ nerede izleme bağlantı noktalarına bağlanmalıyım?

IoT için Azure Defender algılayıcısı, bir SPAN bağlantı noktasına veya ağa DOKUNMASıNA bağlanır ve doğrudan pasif (aracısız) izleme aracılığıyla ICS ağ trafiğini toplamaya başlar. Bu, veri yoluna yerleştirilmediğinden ve cihazları etkin bir şekilde taramazsa OT ağları üzerinde sıfır etkiye sahiptir.

Örnek:
- Tek bir gereç (fiziksel sanal), atölye DMZ katmanında bulunabilir ve bu katmana tüm atölye hücre trafiği yönlendirilir.
- Alternatif olarak, atölye DMZ katmanında yer alacak bulut ya da yerel yönetim ile her bir mağaza zemin hücresindeki küçük mini sensörlerini bulun. Başka bir gereç (sanal veya fiziksel), atölye DMZ katmanındaki trafiği izleyebilir (SCADA, Histora veya MES için).

## <a name="how-does-defender-for-iot-compare-to-the-competition"></a>IoT için Defender, yarışmaya nasıl Karşılaştır?

Diğer çözümler müşterilerin kendi çözümlerini oluşturmalarına izin veren bir dizi özellik sağlarken, IoT için Defender, ilgili tüm Azure kaynaklarınızın güvenliği genelinde geniş bir görünüm sağlayan benzersiz bir uçtan uca IoT güvenlik çözümü sağlar. Azure, mevcut cihaz yönetim araçlarıyla kolay tümleştirme için IoT Hub Module TWINS ile hızlı dağıtım ve tam tümleştirme imkanı sunar.


## <a name="do-i-have-to-be-an-azure-iot-customer"></a>Azure IoT müşterisi olmam gerekiyor mu?

Evet. Bulut bağlantılı dağıtımlar için IoT için Azure Defender, Azure IoT bağlantısı ve altyapısına bağımlıdır.
## <a name="can-i-create-my-own-alerts"></a>Kendi uyarılarımı oluşturabilir miyim?

Evet. IP adresi ve açık bağlantı noktaları gibi, önceden belirlenmiş davranış kümesinde özelleştirilmiş bir uyarı ayarlayabilirsiniz. Özel uyarılar ve bunların nasıl yapılacağı hakkında daha fazla bilgi edinmek için bkz. [özel uyarılar oluşturma](quickstart-create-custom-alerts.md) .

## <a name="where-can-i-see-logs-can-i-customize-logs"></a>Günlükleri nereden görebilirim? Günlükleri özelleştirebilir miyim?

- Bağlı Log Analytics çalışma alanınızı kullanarak uyarıları ve önerileri görüntüleyin. Çalışma alanındaki depolama boyutunu ve süreyi yapılandırın.

- Güvenlik aracılarınızın ham verileri de Log Analytics hesabınızda depolanabilir. Bu seçeneğin yapılandırmasını değiştirmeden önce boyut, süre, depolama gereksinimleri ve ilişkili maliyetleri göz önünde bulundurun.



## <a name="what-happens-when-the-internet-connection-stops-working"></a>Internet bağlantısı çalışmayı kestiğinde ne olur?

Algılayıcılar ve aracılar, cihaz çalıştığı sürece verileri çalıştırmaya ve depolamaya devam eder. Veriler, boyut yapılandırmasına göre güvenlik iletisi önbelleğinde depolanır. Cihazın bağlantısı yapıldığında, güvenlik iletileri göndermeye sürdürülür.





## <a name="next-steps"></a>Sonraki adımlar

IoT için Defender 'ı kullanmaya başlama hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- IoT için Defender 'ı okuyun [genel bakış](overview.md)
- [Hizmet önkoşullarını](service-prerequisites.md) doğrulama
- [Kullanmaya başlama](getting-started.md) hakkında daha fazla bilgi edinin
- [IoT güvenlik uyarıları Için Defender 'ı](concept-security-alerts.md) anlama
