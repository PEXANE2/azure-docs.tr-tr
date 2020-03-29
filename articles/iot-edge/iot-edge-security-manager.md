---
title: Azure IoT Edge güvenlik yöneticisi - Azure IoT Edge
description: IoT Edge aygıt güvenlik duruşunu ve güvenlik hizmetlerinin bütünlüğünü yönetir.
services: iot-edge
keywords: güvenlik, güvenli eleman, enklav, TEE, IoT Edge
author: eustacea
manager: philmea
ms.author: eustacea
ms.date: 08/30/2019
ms.topic: conceptual
ms.service: iot-edge
ms.openlocfilehash: d5cfa16196a8815b711fd5277a80f6eb67d3a388
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76548705"
---
# <a name="azure-iot-edge-security-manager"></a>Azure IoT Edge güvenlik yöneticisi

Azure IoT Edge güvenlik yöneticisi, güvenli silikon donanımı soyutlayarak IoT Edge aygıtını ve tüm bileşenlerini korumak için iyi sınırlanmış bir güvenlik çekirdeğidir. Güvenlik yöneticisi, güvenlik sertleştirmesinin odak noktasıdır ve orijinal ekipman üreticilerine (OEM) teknoloji entegrasyon noktası sağlar.

![Azure IoT Edge güvenlik yöneticisi](media/edge-security-manager/iot-edge-security-manager.png)

IoT Edge güvenlik yöneticisi, IoT Edge aygıtının ve tüm doğal yazılım işlemlerinin bütünlüğünü savunmayı amaçlar. Güvenlik yöneticisi, Güven'i altta yatan donanım donanımı kökünden (varsa) IoT Edge çalışma saatini önyüklemeye ve devam eden işlemleri izlemeye geçiş etir.  IoT Edge güvenlik yöneticisi, mümkün olan en yüksek güvenlik güvencesini sağlamaya yardımcı olmak için güvenli silikon donanımla (varsa) birlikte çalışan bir yazılımdır.  

IoT Edge güvenlik yöneticisinin sorumlulukları şunlardır, ancak bunlarla sınırlı değildir:

* Azure IoT Edge aygıtının emniyetli ve ölçülü önyüklemesi.
* Cihaz kimlik sağlama ve gerektiğinde güven geçişi.
* Aygıt Sağlama Hizmeti gibi bulut hizmetlerinin aygıt bileşenlerini barındırın ve koruyun.
* Benzersiz kimliklere sahip IoT Edge modüllerini güvenli bir şekilde sağlama.
* Gatekeeper noter hizmetleri aracılığıyla güven cihaz donanım kök.
* Çalışma zamanında IoT Edge işlemlerinin bütünlüğünü izleyin.

IoT Edge güvenlik yöneticisi üç bileşeniçerir:

* IoT Edge güvenlik daemon.
* Donanım güvenlik modülü platformu soyutlama Katmanı (HSM PAL).
* İsteğe bağlı ama son derece tavsiye donanım silikon güven veya HSM kökü.

## <a name="the-iot-edge-security-daemon"></a>IoT Edge güvenlik daemon

IoT Edge güvenlik daemon IoT Edge güvenlik yöneticisinin mantıksal işlemlerinden sorumludur. IoT Edge aygıtının güvenilir bilgi işlem tabanının önemli bir bölümünü temsil eder.

### <a name="design-principles"></a>Tasarım ilkeleri

IoT Edge güvenlik daemon iki temel ilkeler izler: operasyonel bütünlüğü en üst düzeye çıkarmak ve şişkinlik ve karmaşa en aza indirmek.

#### <a name="maximize-operational-integrity"></a>Operasyonel bütünlüğü en üst düzeye çıkarın

IoT Edge güvenlik daemon güven donanım herhangi bir kök savunma yeteneği içinde mümkün olan en yüksek bütünlük ile çalışır. Uygun entegrasyon ile, güven donanımı kökü ölçüler ve güvenlik daemon statik ve çalışma zamanında kurcalamaya karşı izler.

Fiziksel erişim her zaman IoT aygıtları için bir tehdittir. Güven donanım kökü IoT Edge güvenlik daemon bütünlüğünü savunmada önemli bir rol oynar.  Güven donanım kökü iki çeşidi gelir:

* sırlar ve şifreleme anahtarları gibi hassas bilgilerin korunması için güvenli öğeler.
* anahtarlar ve ölçüm ve faturalama gibi hassas iş yüklerinin korunması için güvenli enklavlar.

Donanım güven kökünü kullanmak için iki tür yürütme ortamı vardır:

* Hassas bilgileri korumak için güvenli öğelerin kullanımına dayanan standart veya zengin yürütme ortamı (REE).
* Hassas bilgileri korumak ve yazılım yürütmeye koruma sağlamak için güvenli enklav teknolojisinin kullanımına dayanan güvenilir yürütme ortamı (TEE).

Güven donanım kökü olarak güvenli enklavlar kullanan cihazlar için, IoT Edge güvenlik daemon içinde hassas mantık enklav içinde olmalıdır.  Güvenlik daemonunun hassas olmayan kısımları TEE dışında olabilir.  Her durumda, orijinal tasarım üreticileri (ODM) ve orijinal ekipman üreticileri (OEM), önyükleme ve çalışma zamanında IoT Edge güvenlik daemon'unun bütünlüğünü ölçmek ve savunmak için HSM'lerinden güven duymalıdır.

#### <a name="minimize-bloat-and-churn"></a>Şişkinlik ve çalkalama en aza indirmek

IoT Edge güvenlik daemon için başka bir temel ilke churn en aza indirmektir.  En yüksek güven düzeyi için, IoT Edge güvenlik daemon sıkıca güven cihaz donanım kökü ile çift ve yerel kod olarak çalışabilir.  Bu tür gerçekleşmelerin, daemon yazılımını güvenin güvenli güncelleştirme yollarının donanım kökü üzerinden (işletim sistemi sağlanan güncelleştirme mekanizmalarının aksine) güncelleştirilmesi yaygındır ve bu da bazı senaryolarda zorlayıcı olabilir.  IoT aygıtları için güvenlik yenilemesi önerilirken, aşırı güncelleştirme gereksinimleri veya büyük güncelleştirme yükleri tehdit yüzeyini birçok yönden genişletebilir.  Bunlara örnek olarak, büyük güncelleştirme yüklerini işlemek için çok kısıtlı olan operasyonel kullanılabilirliği veya güven donanımı kökünü en üst düzeye çıkarmak için güncelleştirmelerin atlanması verilebilir.  Bu nedenle, IoT Edge güvenlik daemon tasarımı ayak izi ve güvenilir bilgi işlem tabanı küçük tutmak ve güncelleştirme gereksinimlerini en aza indirmek için kısa.

### <a name="architecture-of-iot-edge-security-daemon"></a>IoT Edge güvenlik daemon mimarisi

![Azure IoT Edge güvenlik daemon](media/edge-security-manager/iot-edge-security-daemon.png)

IoT Edge güvenlik daemon güvenlik sertleştirme için güven teknolojisi mevcut donanım kök yararlanır.  Ayrıca, donanım teknolojileri güvenilir yürütme ortamları sunduğunda standart/zengin yürütme ortamı (REE) ile güvenilir bir yürütme ortamı (TEE) arasında bölünmüş dünya çalışmasına olanak tanır. Role özel arabirimler, IoT Edge aygıtının bütünlüğünü ve işlemlerini güvence altına almak için IoT Edge'in ana bileşenlerine olanak tanır.

#### <a name="cloud-interface"></a>Bulut arabirimi

Bulut arabirimi, IoT Edge güvenlik daemonunun güvenlik yenileme gibi aygıt güvenliğine bulut övgüleri gibi bulut hizmetlerine erişmesine olanak tanır.  Örneğin, IoT Edge güvenlik daemon şu anda aygıt kimlik yaşam döngüsü yönetimi için Azure IoT Hub [Aygıt Sağlama Hizmeti'ne](https://docs.microsoft.com/azure/iot-dps/) erişmek için bu arabirimi kullanır.  

#### <a name="management-api"></a>Yönetim API'si

IoT Edge güvenlik daemon, bir IoT Edge modülü oluştururken/başlatırken/durdururken/kaldırırken IoT Edge aracısı tarafından çağrılan bir yönetim API'si sunar. Güvenlik daemon tüm aktif modüller için "kayıtları" depolar. Bu kayıtlar, modülün kimliğini modülün bazı özelliklerine göre eşler. Örneğin, bu modül özellikleri, kapsayıcıda çalışan işlemin proses tanımlayıcısını (pid) ve docker konteynerinin içeriğinin karmasını içerir.

Bu özellikler, arayanın bir eylem için yetkili olduğunu doğrulamak için iş yükü API 'si (aşağıda açıklanan) tarafından kullanılır.

Yönetim API'si ayrıcalıklı bir API'dir ve yalnızca IoT Edge aracısından çağrılabilir.  IoT Edge güvenlik daemon bootstraps ve IoT Edge aracısını başlattığından, IoT Edge aracısının kurcalanmadığını doğrular, o zaman IoT Edge aracısı için örtülü bir kayıt oluşturabilir. İş yükü API kullandığı aynı attestation işlemi de yönetim API erişimi yalnızca IoT Edge aracısı ile kısıtlar.

#### <a name="container-api"></a>Konteyner API'si

Konteyner API'si, Moby veya Docker gibi modül yönetimi için kullanılan konteyner sistemiyle etkileşime girerek kullanılır.

#### <a name="workload-api"></a>İş Yükü API'si

İş yükü API'sine tüm modüller erişebilir. HSM köklü imzalı belirteci veya X509 sertifikası ve bir modüle karşılık gelen güven paketi olarak kimlik kanıtı sağlar. Güven paketi, modüllerin güvenmesi gereken diğer tüm sunucular için CA sertifikaları içerir.

IoT Edge güvenlik daemon bu API korumak için bir attestation işlemi kullanır. Bir modül bu API'yi aradığında, güvenlik daemon'u kimlik kaydı nı bulmaya çalışır. Başarılı olursa, modülü ölçmek için kaydın özelliklerini kullanır. Ölçüm işleminin sonucu kayıtla eşleşirse, yeni bir kimlik kanıtı oluşturulur. İlgili CA sertifikaları (güven paketi) modüle döndürülür.  Modül, IoT Hub'a, diğer modüllere bağlanmak veya bir sunucu başlatmak için bu sertifikayı kullanır. İmzalanan belirteç veya sertifikanın süresi dolmak üzereyken, yeni bir sertifika istemek modülün sorumluluğundadır.

### <a name="integration-and-maintenance"></a>Entegrasyon ve bakım

Microsoft [GitHub Üzerinde IoT Edge güvenlik daemon](https://github.com/Azure/iotedge/tree/master/edgelet)için ana kod tabanını korur.

#### <a name="installation-and-updates"></a>Kurulum ve güncellemeler

IoT Edge güvenlik daemon'unun kurulumu ve güncelleştirmeleri işletim sisteminin paket yönetim sistemi üzerinden yönetilir. Donanım güven köküne sahip IoT Edge aygıtları, güvenli önyükleme ve güncelleme yönetim sistemleri aracılığıyla yaşam döngüsünü yöneterek daemon'un bütünlüğüne ek sertleştirme sağlamalıdır. Cihaz üreticileri bu yolları kendi cihaz yeteneklerine göre araştırmalıdır.

#### <a name="versioning"></a>Sürüm Oluşturma

IoT Edge çalışma zamanı, IoT Edge güvenlik daemonunun sürümünü izler ve raporlar. Sürüm, IoT Edge aracı modülünün *runtime.platform.version* özniteliği olarak bildirilmiştir.

### <a name="hardware-security-module-platform-abstraction-layer-hsm-pal"></a>Donanım güvenlik modülü platformu soyutlama katmanı (HSM PAL)

HSM PAL, IoT Edge geliştiricisini veya kullanıcısını karmaşıklıklarından ayırmak için tüm güven donanımı kökünü soyutlar.  Uygulama programlama arabirimi (API) ve etki alanı iletişim yordamlarının bir birleşimini içerir, örneğin standart bir yürütme ortamı ile güvenli bir yerleşim birimi arasındaki iletişim.  HSM PAL'ın gerçek uygulaması, kullanılan belirli güvenli donanıma bağlıdır. Varlığı hemen hemen her güvenli silikon donanım kullanımını sağlar.

## <a name="secure-silicon-root-of-trust-hardware"></a>Güven donanımının güvenli silikon kökü

IoT Edge cihaz donanımına güven bağlamak için güvenli silikon gereklidir.  Güvenli silikon, Güvenilir Platform Modülü (TPM), gömülü Secure Element (eSE), ARM TrustZone, Intel SGX ve özel güvenli silikon teknolojilerini içerecek şekilde çeşitli şekillerde gelir.  IoT aygıtlarının fiziksel erişilebilirliğiyle ilişkili tehditler göz önüne alındığında, cihazlarda güvenli silikon güven kökünün kullanılması önerilir.

## <a name="iot-edge-security-manager-integration-and-maintenance"></a>IoT Edge güvenlik yöneticisi entegrasyonu ve bakımı

IoT Edge güvenlik yöneticisi, azure IoT Edge platformunun güvenliğini ve bütünlüğünü özel sertleştirme için koruyan bileşenleri tanımlamayı ve yalıtmayı amaçlar. Aygıt üreticileri gibi üçüncü taraflar, aygıt donanımlarıyla özel güvenlik özelliklerinden yararlanmalıdır.  Linux ve Windows platformlarında Güvenilir Platform Modülü (TPM) ile Azure IoT güvenlik yöneticisinin nasıl sertleşiş gösteriş yapılacağını gösteren bağlantılar için sonraki adımlar bölümüne bakın. Bu örnekler yazılım veya sanal TPM'ler kullanır, ancak ayrık TPM aygıtları kullanmak için doğrudan geçerlidir.  

## <a name="next-steps"></a>Sonraki adımlar

Akıllı kenarı [güvence altına alma](https://azure.microsoft.com/blog/securing-the-intelligent-edge/)blogunu okuyun.

Linux sanal [makinede sanal TPM içeren bir IoT Edge cihazı](how-to-auto-provision-simulated-device-linux.md)oluşturun ve tedarik edin.

Windows'da [simüle edilmiş bir TPM içeren bir IoT Edge aygıtı](how-to-auto-provision-simulated-device-windows.md)oluşturun ve sağlama.
