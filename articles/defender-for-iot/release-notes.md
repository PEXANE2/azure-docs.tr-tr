---
title: IoT için Azure Defender 'daki yenilikler
description: Bu makale, IoT için Defender 'ın en son sürümündeki yenilikleri öğrenmenizi sağlar.
services: defender-for-iot
ms.service: defender-for-iot
documentationcenter: na
author: shhazam-ms
manager: rkarlin
editor: ''
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/14/2021
ms.author: shhazam
ms.openlocfilehash: ef72be60b6294ad4e1fca2ce9c0e3c66b64ac687
ms.sourcegitcommit: 4bda786435578ec7d6d94c72ca8642ce47ac628a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2021
ms.locfileid: "103493976"
---
# <a name="whats-new-in-azure-defender-for-iot"></a>IoT için Azure Defender 'daki yenilikler nelerdir?

Bu makalede, IoT için Defender için yeni özellikler ve özellik geliştirmeleri listelenmektedir.

Belirtilen özellikler ÖNIZLEMEDEDIR. [Azure önizleme ek koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) , Beta, önizleme veya henüz genel kullanıma sunulmayan Azure özelliklerine uygulanan ek yasal koşulları içerir.
## <a name="march-2021"></a>Mart 2021

### <a name="sensor---enhanced-custom-alert-rules-public-preview"></a>Algılayıcı gelişmiş özel uyarı kuralları (Genel Önizleme)

Artık güne, gün grubuna ve zaman aralığı ağ etkinliğine göre özel uyarı kuralları oluşturabilirsiniz.  Gün ve saat kuralı koşullarıyla çalışma, örneğin uyarı önem derecesi, uyarı olayının gerçekleştiği zamana göre elde edilen durumlarda faydalıdır. Örneğin, bir hafta sonu veya akşam 'da ağ etkinliği algılandığında yüksek önem derecesine sahip bir uyarı tetikleyen özel bir kural oluşturun.

Bu özellik, sürüm 10,2 sürümü ile sensörde mevcuttur.

### <a name="on-premises-management-console---export-alerts-public-preview"></a>Şirket içi yönetim konsolu-uyarıları dışarı aktarma (Genel Önizleme)

Uyarı bilgileri artık şirket içi yönetim konsolundan bir. csv dosyasına aktarılabilir. Filtrelenmiş görünüme göre algılanan tüm uyarıların veya dışarı aktarma bilgilerinin bilgilerini dışarı aktarabilirsiniz.

Bu özellik, sürüm 10,2 sürümü ile şirket içi yönetim konsolunda kullanılabilir.

### <a name="add-second-network-interface-to-on-premises-management-console-public-preview"></a>Şirket içi yönetim konsoluna ikinci ağ arabirimi ekleme (Genel Önizleme)

Artık şirket içi yönetim konsoluna ikinci bir ağ arabirimi ekleyerek dağıtımınızın güvenliğini geliştirebilirsiniz. Bu özellik, şirket içi yönetimin tek bir güvenli ağ üzerinde bağlı algılayıcıların olmasına olanak sağlarken, kullanıcılarınızın şirket içi yönetim konsoluna ikinci ayrı bir ağ arabirimi aracılığıyla erişmesini sağlar.

Bu özellik, sürüm 10,2 sürümü ile şirket içi yönetim konsolunda kullanılabilir.
### <a name="device-builder---new-micro-agent-public-preview"></a>Cihaz Oluşturucu-yeni mikro Aracı (Genel Önizleme)

Yeni bir cihaz Oluşturucu modülü var. Mikro aracı olarak adlandırılan modül şunları sağlar:

- **IoT için azure IoT Hub ve Azure Defender Ile tümleştirme** -bunu hem Azure IoT Hub hem de IoT Için Azure Defender tarafından sunulan izleme seçeneği ile tümleştirerek IoT cihazlarınızda doğrudan daha güçlü uç nokta güvenliği oluşturun.
- **Standart IoT işletim sistemleri desteğiyle esnek dağıtım seçenekleri** -Linux ve Azure RTOS gibi standart IoT işletim sistemleri desteğiyle ikili bir paket ya da değiştirilebilir kaynak kodu olarak dağıtılabilir.
- **İşletim sistemi çekirdeği bağımlılıkları olmayan en az kaynak gereksinimleri** -küçük ayak izi, düşük CPU tüketimi ve işletim sistemi çekirdeği bağımlılıkları yoktur.
- **Güvenlik durure yönetimi** – IoT cihazlarınızın güvenlik duruşunu önceden izleyin.
- **Sürekli, gerçek zamanlı IoT/ot tehdit algılama** -botağları, deneme yanılma girişimleri, şifre Miners ve şüpheli ağ etkinliği gibi tehditleri algılayın

Kullanım dışı bırakılan Defender-IoT-Micro-Agent belgeleri, *Klasik klasör>cihaz oluşturucular Için aracı tabanlı çözüme* taşınır.

Bu özellik kümesi, geçerli genel önizleme bulutu sürümü ile kullanılabilir.

## <a name="january-2021"></a>Ocak 2021

- [Güvenlik](#security)
- [Ekleme](#onboarding)
- [Stillerin](#usability)
- [Diğer güncelleştirmeler](#other-updates)
### <a name="security"></a>Güvenlik

Bu sürümde sertifika ve parola kurtarma geliştirmeleri yapılmıştır.

#### <a name="certificates"></a>Sertifikalar
  
Bu sürüm şunları yapmanızı sağlar:

- SSL sertifikalarını doğrudan sensörlerden ve şirket içi yönetim konsollarına yükleyin.
- Şirket içi yönetim konsolu ve bağlı sensörler arasında ve bir yönetim konsolu ve yüksek oranda kullanılabilir bir yönetim konsolu arasında doğrulama gerçekleştirin. Doğrulama, sona erme tarihlerini, kök CA özgünlük ve sertifika Iptal listelerini temel alır.  Doğrulama başarısız olursa, oturum devam etmez.

Yükseltmeler için:

- Yükseltme sırasında SSL sertifikası veya doğrulama işlevselliğinde değişiklik yapılmaz.
- Yükseltmeden sonra, algılayıcı ve şirket içi yönetim konsolu yönetici kullanıcıları, SSL sertifikalarını değiştirebilir veya sistem ayarları, SSL sertifikası penceresinden SSL sertifikası doğrulamasını etkinleştirebilir.  

Yeni yüklemeler için:

- İlk kez oturum açma sırasında, kullanıcıların bir SSL sertifikası (önerilen) veya yerel olarak oluşturulmuş otomatik olarak imzalanan bir sertifika kullanması gerekir (önerilmez)
- Sertifika doğrulama, yeni yüklemeler için varsayılan olarak açıktır.

#### <a name="password-recovery"></a>Parola kurtarma
  
Algılayıcı ve şirket içi yönetim konsolu yönetici kullanıcıları artık IoT portalı için Azure Defender 'dan parolaları kurtarabilir. Daha önce parola kurtarma, destek ekibi tarafından gerektirilir.

### <a name="onboarding"></a>Ekleme

#### <a name="on-premises-management-console---committed-devices"></a>Şirket içi yönetim konsolu ile kaydedilmiş cihazlar

Şirket içi yönetim konsolunda ilk oturum açma sonrasında, kullanıcılardan bir etkinleştirme dosyasını karşıya yüklemesi gerekir. Dosya, kuruluş ağında izlenecek toplam cihaz sayısını içerir. Bu sayı, işlenen cihaz sayısı olarak adlandırılır.
Kaydedilen cihazlar, etkinleştirme dosyasının oluşturulduğu IoT portalı için Azure Defender 'daki ekleme işlemi sırasında tanımlanmıştır.
Etkinleştirme dosyasını karşıya yüklemek için ilk kullanıcıların ve kullanıcıların yükseltilme yapması gerekir.
İlk etkinleştirmeden sonra, ağda algılanan cihazların sayısı, taahhüt edilen cihaz sayısını aşmış olabilir. Bu olay, örneğin, yönetim konsoluna daha fazla algılayıcı bağladığınızda meydana gelebilir. Algılanan cihaz sayısıyla kaydedilmiş cihazların sayısı arasında bir tutarsızlık varsa, yönetim konsolunda bir uyarı görüntülenir. Bu olay oluşursa, yeni bir etkinleştirme dosyası yüklemeniz gerekir.

#### <a name="pricing-page-options"></a>Fiyatlandırma sayfası seçenekleri

Fiyatlandırma sayfası, IoT için Azure Defender 'a yeni abonelikler eklemenizi ve ağınızdaki kaydedilmiş cihazları tanımlamanızı sağlar.  
Ayrıca, fiyatlandırma sayfası artık bir algılayıcı ile ilişkili mevcut abonelikleri yönetmenize ve cihaz taahhüdünü güncelleştirmenize olanak tanır.

#### <a name="view-and-manage-onboarded-sensors"></a>Eklendi sensörleri görüntüleme ve yönetme

Yeni bir site ve algılayıcı portalı sayfası şunları yapmanızı sağlar:

- Algılayıcı hakkında açıklayıcı bilgi ekleyin. Örneğin, sensörle ilişkili bir bölge veya serbest metin etiketleri.
- Algılayıcı bilgilerini görüntüleyin ve filtreleyin. Örneğin, bulut bağlı veya yerel olarak yönetilen sensörlerle ilgili ayrıntıları görüntüleyin veya belirli bir bölgedeki sensörlerle ilgili bilgileri görüntüleyin.  

### <a name="usability"></a>Kullanılabilirlik

#### <a name="azure-sentinel-new-connector-page"></a>Azure Sentinel yeni bağlayıcı sayfası

Azure Sentinel 'te IoT veri bağlayıcı için Azure Defender sayfası yeniden tasarlanmıştır. Veri Bağlayıcısı artık IoT Hub 'Ları yerine aboneliklerine dayalıdır; Müşterilerin Azure Sentinel ile yapılandırma bağlantılarını daha iyi yönetmesine izin verme.

#### <a name="azure-portal-permission-updates"></a>Azure portal izin güncelleştirmeleri  

Güvenlik okuyucusu ve güvenlik yöneticisi desteği eklenmiştir.

### <a name="other-updates"></a>Diğer güncelleştirmeler

#### <a name="access-group---zone-permissions"></a>Erişim grubu-bölge izinleri
  
Şirket içi yönetim konsolu erişim grubu kuralları, belirli bir bölgeye erişim verme seçeneğini içermez. Siteleri, bölgeleri ve iş birimlerini kullanan kuralların tanımlanmasıyla ilgili hiçbir değişiklik yoktur.   Yükseltmenin ardından, belirli bölgelere erişime izin veren kuralların bulunduğu erişim grupları, tüm bölgeleri dahil olmak üzere üst sitesine erişime izin verecek şekilde değiştirilir.

#### <a name="terminology-changes"></a>Terminoloji değişiklikleri

Varlık terimi, algılayıcı ve şirket içi yönetim konsolunda, raporlarda ve diğer çözüm arabirimlerinde cihaz olarak yeniden adlandırıldı.
Algılayıcı ve şirket içi yönetim konsolu uyarıları ' nda, bu olayı yönetme terimi düzeltme adımları olarak adlandırılmıştır.

## <a name="next-steps"></a>Sonraki adımlar

[IoT için Defender 'ı kullanmaya başlama](getting-started.md)
