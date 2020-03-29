---
title: Güvenlik çerçevesi - Azure IoT Edge | Microsoft Dokümanlar
description: Azure IoT Edge'i geliştirmek için kullanılan ve çözümünüzü tasarladığınız da göz önünde bulundurulması gereken güvenlik, kimlik doğrulama ve yetkilendirme standartları hakkında bilgi edinin
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 08/30/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 3212493963805de3c8845ec494d87fc92d72998a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76760122"
---
# <a name="security-standards-for-azure-iot-edge"></a>Azure IoT Edge için güvenlik standartları

Azure IoT Edge, verilerinizi ve analitiğinizi akıllı kenara taşırken doğasında olan riskleri giderir. IoT Edge güvenlik standartları, tüm Azure hizmetlerinden beklediğiniz korumayla farklı dağıtım senaryoları için esnekliği dengeler.

IoT Edge çeşitli donanım modelleri üzerinde çalışır, çeşitli işletim sistemlerini destekler ve çeşitli dağıtım senaryoları için geçerlidir. IoT Edge, belirli senaryolar için somut çözümler sunmak yerine, ölçek için tasarlanmış iyi temelli ilkelere dayanan genişletilebilir bir güvenlik çerçevesidir. Dağıtım senaryosunun riski, aşağıdakiler dahil olmak üzere birçok etkene bağlıdır:

* Çözüm sahipliği
* Dağıtım coğrafyası
* Veri duyarlılığı
* Gizlilik
* Uygulama dikey
* Mevzuat gereklilikleri

Bu makalede, IoT Edge güvenlik çerçevesine genel bir bakış sağlar. Daha fazla bilgi için bkz: [Akıllı kenarı güvence altına alma.](https://azure.microsoft.com/blog/securing-the-intelligent-edge/)

## <a name="standards"></a>Standartlar

Standartlar, her ikisi de güvenliğin ayırt edici özellikleri olan inceleme ve uygulama kolaylığını teşvik etmektedir. Bir güvenlik çözümü güven oluşturmak için değerlendirme altında inceleme kendini ödünç ve dağıtım için bir engel olmamalıdır. Azure IoT Edge'i güvence altına alacak çerçevenin tasarımı, aşinalık ve yeniden kullanım için zaman içinde test edilmiş ve endüstride kanıtlanmış güvenlik protokollerine dayanır.

## <a name="authentication"></a>Kimlik doğrulaması

Bir IoT çözümünüzü dağıttığınızda, yalnızca güvenilen aktörlerin, aygıtların ve modüllerin çözümünüze erişebildiğini bilmeniz gerekir. Sertifika tabanlı kimlik doğrulama, Azure IoT Edge platformuiçin özgündoğrulama için birincil mekanizmadır. Bu mekanizma, Internet Engineering Task Force (IETF) tarafından Kamu Anahtar Altyapısı (PKiX) yöneten bir dizi standarttan türetilmiştir.

Azure IoT Edge aygıtıyla etkileşimde olan tüm aygıtlar, modüller ve aktörlerin benzersiz sertifika kimlikleri olmalıdır. Bu kılavuz, etkileşimlerin fiziksel olup olmadığı veya bir ağ bağlantısı aracılığıyla geçerlidir. Her senaryo veya bileşen sertifika tabanlı kimlik doğrulamasına kendini ödünç veremedi, bu nedenle güvenlik çerçevesinin genişletilebilirliği güvenli alternatifler sunar.

Daha fazla bilgi için Azure [IoT Edge sertifika kullanımı](iot-edge-certs.md)na bakın.

## <a name="authorization"></a>Yetkilendirme

En az ayrıcalık ilkesi, bir sistemin kullanıcılarının ve bileşenlerinin yalnızca rollerini gerçekleştirmek için gereken en az kaynak ve veri kümesine erişmesi gerektiğini söyler. Aygıtlar, modüller ve aktörler yalnızca izin kapsamındaki kaynaklara ve verilere erişmelidir ve yalnızca mimari olarak izin verildiğinde. Bazı izinler yeterli ayrıcalıklarla yapılandırılabilir ve diğerleri mimari olarak zorlanır. Örneğin, bazı modüllerin Azure IoT Hub'ına bağlanma yetkisi verilebilir. Ancak, bir IoT Edge aygıtındaki bir modülün başka bir IoT Edge aygıtındaki bir modülün ikizine erişmesi için hiçbir neden yoktur.

Diğer yetkilendirme şemaları arasında sertifika imzalama hakları ve rol tabanlı erişim denetimi (RBAC) yer alır.

## <a name="attestation"></a>Tasdik

Attestation, kötü amaçlı yazılımları tespit etmek ve önlemek için önemli olan yazılım bitlerinin bütünlüğünü sağlar. Azure IoT Edge güvenlik çerçevesi attestation'ı üç ana kategori altında sınıflandırır:

* Statik attestation
* Çalışma süresi attestation
* Yazılım attestation

### <a name="static-attestation"></a>Statik attestation

Statik attestation, işletim sistemi, tüm çalışma süreleri ve yapılandırma bilgileri de dahil olmak üzere, güç-up sırasında bir aygıttaki tüm yazılımların bütünlüğünü doğrular. Statik attestation güç-up sırasında meydana geldiğinden, genellikle güvenli önyükleme olarak adlandırılır. IoT Edge aygıtlarının güvenlik çerçevesi üreticileri genişletir ve statik attestation işlemlerini güvence altına alan güvenli donanım özelliklerini içerir. Bu işlemler güvenli önyükleme ve güvenli firmware yükseltme içerir. Silikon satıcıları ile yakın işbirliği içinde çalışmak gereksiz firmware katmanları ortadan kaldırır, böylece tehdit yüzeyini en aza indirir.

### <a name="runtime-attestation"></a>Çalışma süresi attestation

Bir sistem güvenli bir önyükleme işlemini tamamladıktan sonra, iyi tasarlanmış sistemler kötü amaçlı yazılım enjekte etme girişimlerini algılamalı ve uygun karşı önlemler almalıdır. Kötü amaçlı yazılım saldırıları sistemin bağlantı noktalarını ve arabirimlerini hedef leyebilir. Kötü niyetli aktörlerin bir aygıta fiziksel erişimi varsa, aygıtın kendisini kurcalayabilir veya erişim elde etmek için yan kanal saldırılarını kullanabilirler. Kötü amaçlı yazılım veya yetkisiz yapılandırma değişiklikleri olsun, bu tür hoşnutsuzluklar, önyükleme işleminden sonra enjekte edildiği için statik attestation tarafından algılanamaz. Bu tür tehditleri savuşturmak için aygıtın donanımı tarafından sunulan veya uygulanan karşı önlemler yardımcı olur. IoT Edge'in güvenlik çerçevesi, çalışma zamanı tehditlerle mücadele eden uzantılar için açıkça çağrıda bulunur.  

### <a name="software-attestation"></a>Yazılım attestation

Akıllı kenar sistemleri de dahil olmak üzere tüm sağlıklı sistemlerin yamalar alabına ve yükseltmelere ihtiyacı vardır. Güvenlik güncelleştirme işlemleri için önemlidir, aksi takdirde potansiyel tehdit vektörleri olabilir. IoT Edge'in güvenlik çerçevesi, paketlerin kaynağını doğrulamak ve bütünlüğünü sağlamak için ölçülen ve imzalanmış paketler aracılığıyla güncelleştirmeler için çağrıda bulunur. Bu standart tüm işletim sistemleri ve uygulama yazılımı bitleri için geçerlidir.

## <a name="hardware-root-of-trust"></a>Güvenin donanım kökü

Birçok akıllı kenar aygıtı için, özellikle de olası kötü amaçlı aktörler tarafından fiziksel olarak erişilebilen aygıtlar için donanım güvenliği, koruma için son savunmadır. Kurcalamaya dayanıklı donanım, bu tür dağıtımlar için çok önemlidir. Azure IoT Edge, güvenli silikon donanım satıcılarının çeşitli risk profillerini ve dağıtım senaryolarını karşılamak için farklı donanım güven köklerini sunmaya teşvik eder. Donanım güveni, Güvenilir Platform Modülü (ISO/IEC 11889) ve Trusted Computing Group'un Aygıt Tanımlayıcı Kompozisyon Motoru (DICE) gibi ortak güvenlik protokolü standartlarından gelebilir. TrustZones ve Software Guard Extensions (SGX) gibi güvenli enklav teknolojileri de donanım güveni sağlar.

## <a name="certification"></a>Sertifika

Müşterilerin dağıtımları için Azure IoT Edge aygıtları satın alırken bilinçli kararlar almalarına yardımcı olmak için, IoT Edge çerçevesi sertifika gereksinimlerini içerir. Bu gereksinimlerin temelinde, güvenlik uygulamalarının doğrulanmasıyla ilgili güvenlik talepleri ve sertifikalarla ilgili sertifikalar yer alır. Örneğin, bir güvenlik talebi sertifikası, IoT Edge aygıtının önyükleme saldırılarına karşı koymak için bilinen güvenli donanım kullandığı anlamına gelir. Doğrulama sertifikası, güvenli donanımın aygıtta bu değeri sunmak için düzgün şekilde uygulandığı anlamına gelir. Sadelik prensibine uygun olarak, çerçeve belgelendirme yükünü minimumda tutmaya çalışır.

## <a name="extensibility"></a>Genişletilebilirlik

IoT teknolojisi farklı iş dönüşümleri itici ile, güvenlik ortaya çıkan senaryoları ele paralel olarak gelişmelidir. Azure IoT Edge güvenlik çerçevesi, genişletilebilirlik te sartını içerdiği sağlam bir temelle başlar:

* Azure IoT Hub için Aygıt Sağlama Hizmeti gibi birinci taraf güvenlik hizmetleri.
* Zengin bir ağ üzerinden farklı uygulama dikeyleri (endüstriyel veya sağlık hizmetleri gibi) veya teknoloji odağı (kafes ağlarda güvenlik izleme veya silikon donanım attestation hizmetleri gibi) için yönetilen güvenlik hizmetleri gibi üçüncü taraf hizmetler Ortak.
* Kimlik doğrulama ve kimlik yönetimi sertifikaları dışındaki güvenli teknolojiyi kullanmak gibi alternatif güvenlik stratejileriyle güçlendirmeyi içeren eski sistemler.
* Yeni ortaya çıkan güvenli donanım teknolojilerinin ve silikon iş ortağı katkılarının benimsenmesi için güvenli donanım.

Sonuç olarak, akıllı kenarı güvence altına almak, IoT'nin güvenliğini sağlamak için ortak çıkarlar tarafından yönlendirilen açık bir topluluğun ortak katkıları gerektirir. Bu katkılar güvenli teknolojiler veya hizmetler şeklinde olabilir. Azure IoT Edge güvenlik çerçevesi, maksimum kapsama alanı için Azure bulutuyla aynı düzeyde güven ve bütünlük sunmak için genişletilebilir bir güvenlik temeli sunar.  

## <a name="next-steps"></a>Sonraki adımlar

Azure IoT Edge'in [akıllı kenarı](https://azure.microsoft.com/blog/securing-the-intelligent-edge/)nasıl güvence altına alabildiğini hakkında daha fazla bilgi edinin.
