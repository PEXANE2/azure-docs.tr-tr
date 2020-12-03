---
title: IoT mimarisi için Azure Defender
description: IoT mimarisi ve bilgi akışı için Azure Defender hakkında bilgi edinin.
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
ms.date: 10/08/2020
ms.author: rkarlin
ms.openlocfilehash: a8697094a3366e3b82ca65f1b962101243b22f84
ms.sourcegitcommit: 65db02799b1f685e7eaa7e0ecf38f03866c33ad1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2020
ms.locfileid: "96548928"
---
# <a name="azure-defender-for-iot-architecture"></a>IoT mimarisi için Azure Defender

Bu makalede, IoT için Defender çözümünün işlevsel sistem mimarisi açıklanmaktadır.

## <a name="defender-for-iot-components"></a>IoT bileşenleri için Defender

IoT için Defender, hem Azure bulutuna hem de şirket içi bileşenlere bağlanır. Çözüm, birden fazla uzak konum içeren büyük ve coğrafi olarak dağıtılmış ortamlarda ölçeklenebilirlik için tasarlanmıştır. Bu çözüm, ülke, bölge, iş birimi veya bölgeye göre çok katmanlı bir dağıtılmış mimariye izin vermez. 

IoT için Azure Defender aşağıdaki bileşenleri içerir: 

**Buluta bağlı dağıtımlar**

- IoT algılayıcı VM veya gereç için Azure Defender
- Bulut yönetimi için Azure portal ve Azure Sentinel ile tümleştirme
- Yerel site yönetimi için şirket içi yönetim konsolu
- Katıştırılmış bir güvenlik Aracısı (isteğe bağlı)

**AIR-gapped (çevrimdışı) dağıtımlar**

- IoT algılayıcı VM veya gereç için Azure Defender
- Yerel site yönetimi için şirket içi yönetim konsolu


![IoT mimarisi için Defender](./media/architecture/defender-iot-security-architecture.png)

### <a name="azure-defender-for-iot-sensors"></a>IoT sensörleri için Azure Defender

IoT sensörleri için Defender, ağ cihazlarını bulur ve sürekli olarak izler. Algılayıcılar, IoT ve OT cihazlarındaki pasif (aracısız) izleme kullanan ICS ağ trafiğini toplar. 
 
Amaç-IoT ve OT ağları için tasarlanan aracısız teknoloji, IoT 'ye ayrıntılı görünürlük ve ağa bağlı dakikalar içinde risk riski sunuyor. Ağ ve ağ cihazlarında, ağ trafiği analizi (NTA) yaklaşımına bağlı olarak sıfır performans etkisi vardır. 
 
Patentli, IoT ve OT kullanan davranış analizi ve katman 7 derin paket Incelemesi (DPı) sayesinde, geleneksel imza tabanlı çözümlerin ötesinde, anormal veya yetkisiz etkinliklere dayalı olarak Gelişmiş IoT ve OT tehditleri (örneğin, çok sayıda kötü amaçlı yazılım) hemen tespit etmenizi sağlar. 
  
IoT sensörleri için Defender, bir SPAN bağlantı noktasına veya ağa DOKUNMASıNA bağlanır ve IoT ve OT ağ trafiği için hemen DPı gerçekleştirmeye başlar. 
 
Veri toplama, işleme, analiz ve uyarı doğrudan sensörde gerçekleşir. Bu, düşük bant genişliğine sahip konumlar veya yüksek gecikmeli bağlantı olan konumlara uygun hale getirir, çünkü yalnızca meta veriler yönetim konsoluna aktarılır.

Algılayıcı beş analiz algılama altyapısı içerir. Altyapılar, hem gerçek zamanlı hem de önceden kaydedilmiş trafiğin analizine dayalı olarak uyarıları tetikler. Aşağıdaki altyapılar mevcuttur: 

#### <a name="protocol-violation-detection-engine"></a>Protokol ihlali algılama altyapısı
Protokol ihlali algılama altyapısı, ICS protokol belirtimlerini ihlal eden paket yapılarının ve alan değerlerinin kullanımını tanımlar, örneğin: Modbus özel durumu ve kullanılmayan bir işlev kodu uyarılarını başlatma.

#### <a name="policy-violation-detection-engine"></a>İlke ihlali algılama altyapısı
Makine öğrenimini kullanarak, ilke ihlali algılama altyapısı, belirli işlev kodlarının yetkisiz kullanımı, belirli nesnelere erişim veya cihaz yapılandırmasındaki değişiklikler gibi taban çizgisi davranışından herhangi bir sapmanın Kullanıcı sayısını uyarır. Örneğin: DeltaV yazılım sürümü değişti ve yetkisiz PLC programlama uyarıları. Özellikle, ilke ihlali altyapısı, sektör sınırlı durum modelleme (IFSM) adlı bir patentli tekniği kullanarak, ICS ağlarını durum ve geçişlerin belirleyici dizileri olarak modeller. İlke ihlali algılama altyapısı, ICS ağlarının bir temelini oluşturur, böylece platform, ağın bir taban çizgisini oluşturmak için daha kısa bir öğrenme dönemi gerektirir. bu sayede, ağ için başlangıçta OT ağları yerine geliştirmiş olan genel matematik yaklaşımlarına veya analizinden daha kısa bir öğrenme süresi gerekir.

#### <a name="industrial-malware-detection-engine"></a>Endüstriyel kötü amaçlı yazılım algılama altyapısı
Endüstriyel kötü amaçlı yazılım algılama altyapısı, Conficker, Black Energy, Havex, WannaCry, NotPetya ve Triton gibi bilinen kötü amaçlı yazılımların varlığını belirten davranışları belirler. 

#### <a name="anomaly-detection-engine"></a>Anomali algılama altyapısı
Anomali algılama altyapısı, olağandışı makineden makineye ('U M2M) iletişimleri ve davranışları algılar. Platformlar, ICS ağlarını durum ve geçişlerin belirleyici dizileri olarak modelleyerek, genel matematik yaklaşımlarından veya orijinal olarak bu şekilde geliştirilme yerine daha kısa bir öğrenme dönemi gerektirir. Ayrıca, en az yanlış pozitif sonuç vererek anomali değerleri de algılar. Anomali algılama altyapısı uyarıları aşırı sayıda SMB oturum açma denemesi içerir ve PLC taraması uyarı algıladı.

#### <a name="operational-incident-detection"></a>İşletimsel olay algılama
İşletimsel olay algılama, donanım hatasının erken işaretlerini gösterebilen aralıklı bağlantı gibi işlemsel sorunları algılar. Örneğin, cihazın bağlantısının kesilmesi (yanıt vermemeye karşı) ve Siemens S7 stop PLC komutunun uyarı gönderilmesi şüpheli.


### <a name="management-consoles"></a>Yönetim konsolları
Karma ortamlarda IoT için Azure Defender 'ın yönetilmesi iki yönetim portalı aracılığıyla gerçekleştirilir: 
- Algılayıcı konsolu
- Şirket içi yönetim konsolu
- Azure portal

#### <a name="sensor-console"></a>Algılayıcı konsolu
Algılayıcı algılamaları, bir ağ eşlemesinde, varlık envanterinde ve çok çeşitli raporlarda, örneğin risk değerlendirmesi raporları, veri araştırma sorguları ve saldırı vektörleri gibi görüntülenebileceği, araştırılabilecek ve çözümlenebilecekleri algılayıcı konsolunda görüntülenir. Ayrıca, algılayıcı motorları tarafından algılanan tehditleri görüntülemek ve işlemek, üçüncü taraf sistemlere bilgi iletmek, kullanıcıları yönetmek ve daha fazlasını yapmak için konsolunu da kullanabilirsiniz.

![IoT algılayıcı konsolu için Defender](./media/architecture/sensor-console.png)

#### <a name="on-premises-management-console"></a>Şirket içi yönetim konsolu
Şirket içi yönetim konsolu, güvenlik işlemleri Merkezi (SOC) işleçlerinin birden çok sensörden tek bir panoda toplanan uyarıları yönetmesine ve analiz etmesine olanak sağlar ve bu da OT ağlarının sistem durumunun genel bir görünümünü sağlar.

Bu mimari, bir SOC düzeyinde ağın kapsamlı bir görünümünü, iyileştirilmiş uyarı işlemeyi ve işlemsel ağ güvenliğinin denetimini sunarak karar verme ve risk yönetiminin flavasız kalmasını sağlar.

Yönetim Konsolu, çok kiracılı, izleme, veri analizi ve merkezi algılayıcı uzaktan kumandasına ek olarak, uzak gereçlerden her biri için ek sistem bakım araçları (uyarı dışlama gibi) ve tamamen özelleştirilmiş raporlama özellikleri sağlar. Bu ölçeklenebilir mimari, her iki yerel yönetimi de bir site düzeyinde, bölge düzeyinde ve SOC içinde genel yönetime destekler.

Yönetim Konsolu, kurtarma için gereken tüm yapılandırma dosyalarının yedeklemelerini düzenli aralıklarla alan bir yedekleme konsolu sağlayan yüksek kullanılabilirliğe sahip yapılandırma için dağıtılabilir. Ana konsol başarısız olursa, yerel site yönetimi gereçleri, kesintiye uğramadan kullanılabilirliği sürdürmek için yedekleme konsolu ile eşitlenmek üzere otomatik olarak yük devreder.

#### <a name="azure-portal"></a>Azure portal

Azure 'daki IoT portalı için Defender, size yardımcı olmak için kullanılır: ·   Çözüm gereçlerini satın al ·   Yazılım yükleyip güncelleştirme ·   Azure 'a sensörlerden katılın ·   Tehdit zekası paketlerini güncelleştirme

## <a name="embedded-security-agent-built-in-mode"></a>Katıştırılmış Güvenlik Aracısı: yerleşik mod

**Yerleşik** modda, IoT Hub **güvenlik** seçeneğini etkinleştirmek istediğinizde IoT için Defender etkinleştirilir. Gerçek zamanlı izleme, öneriler ve uyarılar sunma, yerleşik mod, tek adımlı cihaz görünürlüğü ve eşleşmeyen güvenlik sunar. Derleme modu, herhangi bir cihazda aracı yüklemesi gerektirmez ve alan cihazınızı ve IoT Hub 'ınızı çözümlemek ve korumak için günlüğe kaydedilen etkinliklerde gelişmiş çözümlemeler kullanır.

## <a name="embedded-security-agent-enhanced-mode"></a>Katıştırılmış Güvenlik Aracısı: Gelişmiş mod

**Gelişmiş** modda, IoT Hub **güvenlik** seçeneğini açıp, cihazlarınızdaki IoT cihaz aracıları için Defender 'ı yükledikten sonra, aracılar cihazlarınızdan ham güvenlik olaylarını toplar, toplar ve analiz eder. Ham güvenlik olayları, IP bağlantıları, işlem oluşturma, Kullanıcı oturum açmaları ve güvenlikle ilgili diğer bilgileri içerebilir. IoT cihaz aracıları için Defender, yüksek ağ aktarım hızını önlemeye yardımcı olmak için olay toplamayı da işler. Aracılar yüksek düzeyde özelleştirilebilir ve en hızlı SLA 'da yalnızca önemli bilgileri göndermek ya da kapsamlı güvenlik bilgilerini ve bağlamı daha büyük kesimlerde toplamak, daha yüksek hizmet maliyetlerine izin vermek gibi belirli görevler için bunları kullanmanıza olanak sağlar.

Cihaz aracıları ve diğer uygulamalar, güvenlik bilgilerini Azure IoT Hub 'a göndermek için **Azure güvenlik iletisi gönder SDK 'sını** kullanır. IoT Hub bu bilgileri alır ve IoT hizmeti için Defender 'a iletir.

IoT hizmeti için Defender etkinleştirildikten sonra, iletilen verilere ek olarak, IoT Hub IoT için Defender tarafından analiz edilmek üzere tüm iç verileri de gönderir. Bu veriler cihaz bulutu işlem günlükleri, cihaz kimlikleri ve hub yapılandırması içerir. Bu bilgilerin tümü, IoT Analytics işlem hattı için Defender oluşturmaya yardımcı olur.

IoT Analytics işlem hattı için Defender, Microsoft ve Microsoft iş ortakları içindeki çeşitli kaynaklardan ek tehdit bilgileri akışları da alır. Tüm analiz işlem hattı için Defender, hizmette yapılan tüm müşteri yapılandırmaları (özel uyarılar ve güvenlik iletisi gönder SDK 'Sı) ile birlikte kullanılır.

IoT için Defender, analiz işlem hattını kullanarak, eyleme dönüştürülebilir öneriler ve uyarılar oluşturmak için tüm bilgi akışlarını birleştirir. İşlem hattı, güvenlik araştırmacıları ve uzmanlar tarafından oluşturulan özel kuralların yanı sıra standart cihaz davranışından ve risk analizinden sapma arayan makine öğrenimi modellerini içerir.

Her müşterinin Log Analytics çalışma alanına IoT önerileri ve uyarıları için Defender (analiz işlem hattı çıkışı) yazılır. Çalışma alanındaki ham olayların yanı sıra uyarı ve öneriler de dahil olmak üzere, algılanan şüpheli etkinliklerin tam ayrıntılarını kullanarak derinlemesine araştırma ve sorgular sunar.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, IoT çözümü için Defender 'ın temel mimarisi ve iş akışı hakkında bilgi edindiniz. Önkoşullar hakkında daha fazla bilgi edinmek ve IoT Hub ' de Güvenlik çözümünüzü etkinleştirmek için aşağıdaki makalelere bakın:

- [Hizmet önkoşulları](service-prerequisites.md)
- [Başlarken](getting-started.md)
- [Çözümünüzü yapılandırma](quickstart-configure-your-solution.md)
- [IoT Hub güvenliği etkinleştir](quickstart-onboard-iot-hub.md)
- [IoT için Defender SSS](resources-frequently-asked-questions.md)
- [IoT güvenlik uyarıları için Defender](concept-security-alerts.md)
