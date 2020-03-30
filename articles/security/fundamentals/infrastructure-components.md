---
title: Azure bilgi sistemi bileşenleri ve sınırları
description: Bu makalede, Microsoft Azure mimarisi ve yönetimi genel bir açıklamasını sağlar.
services: security
documentationcenter: na
author: TerryLanfear
manager: barbkess
editor: TomSh
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2018
ms.author: terrylan
ms.openlocfilehash: 68535f70507e7a81d217f4148314a3d76ec832ea
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68727208"
---
# <a name="azure-information-system-components-and-boundaries"></a>Azure bilgi sistemi bileşenleri ve sınırları
Bu makalede, Azure mimarisi ve yönetimi genel bir açıklamasını sağlar. Azure sistem ortamı aşağıdaki ağlardan oluşur:

- Microsoft Azure üretim ağı (Azure ağı)
- Microsoft kurumsal ağ (corpnet)

Bu ağların işletilmesi nden ve bakımından ayrı BT ekipleri sorumludur.

## <a name="azure-architecture"></a>Azure mimarisi
Azure, bir veri merkezi ağı aracılığıyla uygulama ve hizmetleri oluşturmak, dağıtmak ve yönetmek için bir bulut bilgi işlem platformu ve altyapısıdır. Microsoft bu veri merkezlerini yönetir. Azure, belirttiğiniz kaynak sayısına bağlı olarak kaynak gereksinimine göre sanal makineler (VM' ler) oluşturur. Bu VM'ler, bulutta kullanılmak üzere tasarlanmış ve herkese açık olmayan bir Azure hipervizöründe çalışır.

Her Azure fiziksel sunucu düğümünde, doğrudan donanımın üzerinde çalışan bir hipervizör vardır. Hipervizör bir düğümü değişken sayıda konuk VM'ye böler. Her düğüm de ana bilgisayar işletim sistemini çalıştıran bir kök VM vardır. Her VM'de Windows Güvenlik Duvarı etkindir. Hizmet tanımı dosyasını yapılandırarak hangi bağlantı noktalarının adreslenebilir olduğunu tanımlarsınız. Bu bağlantı noktaları, dahili veya harici olarak açık ve adreslenebilir olan lardır. Tüm trafik ve disk ve ağa erişim hipervizör ve kök işletim sistemi tarafından aracılık edilir.

Ana bilgisayar katmanında Azure VM'ler en son Windows Server'ın özelleştirilmiş ve sertleştirilmiş bir sürümünü çalıştırın. Azure, Windows Server'ın yalnızca VM'leri barındırmak için gereken bileşenleri içeren bir sürümünü kullanır. Bu, performansı artırır ve saldırı yüzeyini azaltır. Makine sınırları, işletim sistemi güvenliğine bağlı olmayan hipervizör tarafından uygulanır.

### <a name="azure-management-by-fabric-controllers"></a>Kumaş denetleyicileri tarafından Azure yönetimi

Azure'da, fiziksel sunucularda (bıçaklar/düğümler) çalışan VM'ler yaklaşık 1000 küme halinde gruplandırılır. VM'ler, kumaş denetleyicisi (FC) adı verilen ölçeklenmiş ve gereksiz platform yazılım bileşeni tarafından bağımsız olarak yönetilir.

Her FC kendi kümesinde çalışan uygulamaların yaşam döngüsünü yönetir ve kendi kontrolü altındaki donanımın durumunu hükümler ve izler. Bir sunucunun başarısız olduğunu belirlediğinde, vm örneklerini sağlıklı sunucularda reenkarne etmek gibi otonom işlemler çalıştırıyor. FC ayrıca uygulamaları dağıtma, güncelleştirme ve ölçekleme gibi uygulama yönetimi işlemleri de gerçekleştirir.

Veri merkezi kümelere ayrılmıştır. Kümeler FC düzeyinde hataları yalıttır ve belirli hata sınıflarının sunucuları oluştukları kümenin ötesinde etkilemesini engeller. Belirli bir Azure kümesine hizmet veren FC'ler bir FC kümesinde gruplandırılır.

### <a name="hardware-inventory"></a>Donanım envanteri

FC, bootstrap yapılandırma işlemi sırasında Azure donanım ve ağ aygıtlarının envanterini hazırlar. Azure üretim ortamına giren tüm yeni donanım ve ağ bileşenleri, önyükleme yapılandırma işlemini izlemelidir. FC, datacenter.xml yapılandırma dosyasında listelenen tüm envanterin yönetiminden sorumludur.

### <a name="fc-managed-operating-system-images"></a>FC tarafından yönetilen işletim sistemi görüntüleri

İşletim sistemi ekibi, Azure üretim ortamındaki tüm ana bilgisayar ve konuk VM'lerde dağıtılan Sanal Sabit Diskler şeklinde görüntüler sağlar. Takım, bu temel görüntüleri otomatik bir çevrimdışı oluşturma işlemi yle oluşturur. Temel görüntü, çekirdek ve diğer temel bileşenlerin Azure ortamını desteklemek üzere değiştirildiği ve optimize edildiği işletim sisteminin bir sürümüdür.

Kumaş la yönetilen işletim sistemi görüntüleri üç türü vardır:

- Ana bilgisayar: Ana bilgisayar VM'lerde çalışan özelleştirilmiş bir işletim sistemi.
- Yerel: Kiracılarla çalışan yerel bir işletim sistemi (örneğin, Azure Depolama). Bu işletim sisteminin hipervizörü yoktur.
- Misafir: Konuk VM'ler üzerinde çalışan bir konuk işletim sistemi.

Ana bilgisayar ve yerel FC tarafından yönetilen işletim sistemleri bulutta kullanılmak üzere tasarlanmıştır ve genel olarak erişilemez.

#### <a name="host-and-native-operating-systems"></a>Ana bilgisayar ve yerel işletim sistemleri

Ana bilgisayar ve yerel, kumaş aracılarını barındıran ve bir işlem düğümüüzerinde çalışan (düğümde ilk VM olarak çalışır) ve depolama düğümleri üzerinde çalışan sertleştirilmiş işletim sistemi görüntüleridir. Ana bilgisayar ve yerel en iyi duruma getirilmiş temel görüntüleri kullanmanın yararı, API'ler veya kullanılmayan bileşenler tarafından maruz kalan yüzey alanını azaltmasın. Bunlar yüksek güvenlik riskleri sunabilir ve işletim sisteminin ayak izini artırabilir. Azaltılmış ayak izi işletim sistemleri yalnızca Azure için gerekli bileşenleri içerir.

#### <a name="guest-operating-system"></a>Konuk işletim sistemi

Konuk işletim sistemi VM'lerinde çalışan Azure dahili bileşenlerinin Uzak Masaüstü Protokolü'nü çalıştırma fırsatı yoktur. Temel yapılandırma ayarlarında yapılan tüm değişikliklerin değişiklik ve sürüm yönetimi sürecinden geçmesi gerekir.

## <a name="azure-datacenters"></a>Azure veri merkezleri
Microsoft Bulut Altyapısı ve İşlemleri (MCIO) ekibi, tüm Microsoft çevrimiçi hizmetleri için fiziksel altyapıyı ve veri merkezi olanaklarını yönetir. MCIO, öncelikle veri merkezleri içindeki fiziksel ve çevresel denetimlerin yönetilmesinden ve dış çevre ağ aygıtlarını (kenar yönlendiricileri ve veri merkezi yönlendiricileri gibi) yönetmek ve desteklemekten sorumludur. MCIO ayrıca veri merkezindeki raflarda minimum sunucu donanımını kurmakla da sorumludur. Müşterilerin Azure ile doğrudan etkileşimi yoktur.

## <a name="service-management-and-service-teams"></a>Servis yönetimi ve servis ekipleri
Hizmet ekipleri olarak bilinen çeşitli mühendislik grupları Azure hizmetinin desteğini yönetir. Her hizmet ekibi Azure için bir destek alanından sorumludur. Her servis ekibi, hizmetteki hataları araştırmak ve çözmek için bir mühendisi 24x7'ye hazır hale getirmelidir. Hizmet ekipleri varsayılan olarak Azure'da çalışan donanıma fiziksel erişime sahip değildir.

Servis ekipleri şunlardır:

- Uygulama Platformu
- Azure Active Directory
- Azure İşlem
- Azure Net
- Bulut Mühendisliği Hizmetleri
- ISSD: Güvenlik
- Çok Faktörlü Kimlik Doğrulama
- SQL Database
- Depolama

## <a name="types-of-users"></a>Kullanıcı türleri
Microsoft çalışanları (veya yüklenicileri) dahili kullanıcı olarak kabul edilir. Diğer tüm kullanıcılar dış kullanıcı olarak kabul edilir. Tüm Azure dahili kullanıcılarının çalışan durumları, müşteri verilerine erişimlerini (erişim veya erişim yok) tanımlayan bir duyarlılık düzeyiyle sınıflandırılır. Azure'daki kullanıcı ayrıcalıkları (kimlik doğrulama gerçekleştikten sonra yetkilendirme izni) aşağıdaki tabloda açıklanmıştır:

| Rol | Dahili veya harici | Duyarlılık düzeyi | Gerçekleştirilen yetkili ayrıcalıklar ve işlevler | Erişim türü
| --- | --- | --- | --- | --- |
| Azure veri merkezi mühendisi | İç | Müşteri verilerine erişim yok | Binaların fiziksel güvenliğini yönetin. Veri merkezine girip çıkmak için devriye gezin ve tüm giriş noktalarını izleyin. Veri merkezine giriş ve çıkışta, veri merkezi içinde genel hizmetler (yemek veya temizlik gibi) veya BT çalışmaları sağlayan temizlenmeden bazı personele eşlik edin. Ağ donanımının rutin izleme ve bakımını gerçekleştirin. Çeşitli araçlar kullanarak olay yönetimi ve kesme düzeltme çalışması gerçekleştirin. Veri merkezlerinde fiziksel donanımın rutin izleme ve bakımını gerçekleştirin. Mülk sahiplerinden talep üzerine çevreye erişim. Adli soruşturma ları gerçekleştirebilir, olay raporlarını kaydedebilecek ve zorunlu güvenlik eğitimi ve politika gereksinimleri gerektirebilir. Tarayıcılar ve günlük toplama gibi kritik güvenlik araçlarının operasyonel mülkiyeti ve bakımı. | Ortama kalıcı erişim. |
| Azure olay triajı (hızlı yanıt mühendisleri) | İç | Müşteri verilerine erişim | MCIO, destek ve mühendislik ekipleri arasındaki iletişimi yönetin. Platform olaylarını, dağıtım sorunlarını ve hizmet isteklerini üçleme. | Müşteri olmayan sistemlere sınırlı kalıcı erişimle ortama tam zamanında erişim. |
| Azure dağıtım mühendisleri | İç | Müşteri verilerine erişim | Azure'u desteklemek için platform bileşenlerini, yazılımları ve zamanlanmış yapılandırma değişikliklerini dağıtın ve yükseltin. | Müşteri olmayan sistemlere sınırlı kalıcı erişimle ortama tam zamanında erişim. |
| Azure müşteri kesintisi desteği (kiracı) | İç | Müşteri verilerine erişim | Tek tek işlem kiracıları ve Azure hesapları için platform kesintilerini ve hatalarını hata ayıklama ve tanılama. Hataları analiz edin. Kritik düzeltmeleri platforma veya müşteriye yönlendirin ve destek genelinde teknik iyileştirmeler geliştirin. | Müşteri olmayan sistemlere sınırlı kalıcı erişimle ortama tam zamanında erişim. |
| Azure canlı site mühendisleri (izleme mühendisleri) ve olay | İç | Müşteri verilerine erişim | Tanılama araçlarını kullanarak platform sağlığını tanılar ve azla. Toplu sürücü düzeltmeleri, kesintilerden kaynaklanan öğeleri onarmak ve kesinti geri yükleme eylemlerine yardımcı olun. | Müşteri olmayan sistemlere sınırlı kalıcı erişimle ortama tam zamanında erişim. |
|Azure müşterileri | Dış | Yok | Yok | Yok |

Azure, kuruluş kullanıcılarının ve müşterilerinin (veya kuruluş kullanıcıları adına hareket eden işlemlerin) kimliğini doğrulamak için benzersiz tanımlayıcılar kullanır. Bu, Azure ortamının bir parçası olan tüm varlıklar ve aygıtlar için geçerlidir.

### <a name="azure-internal-authentication"></a>Azure dahili kimlik doğrulaması

Azure dahili bileşenleri arasındaki iletişim TLS şifreleme ile korunur. Çoğu durumda, X.509 sertifikaları kendi imzalı. Azure ağı dışından erişilebilen bağlantılara sahip sertifikalar ve FC'ler için sertifikalar bir istisnadır. FC'lerin, güvenilir bir ca tarafından desteklenen bir Microsoft Yetki Sertifikası (CA) tarafından verilen sertifikaları vardır. Bu, FC ortak anahtarlarının kolayca devredilmesine olanak tanır. Ayrıca, Microsoft geliştirici araçları FC ortak anahtarlarını kullanır. Geliştiriciler yeni uygulama görüntüleri gönderdiğinde, görüntüler katıştılı sırları korumak için bir FC ortak anahtarıyla şifrelenir.

### <a name="azure-hardware-device-authentication"></a>Azure donanım aygıtı kimlik doğrulaması

FC, kendi kontrolü altındaki çeşitli donanım aygıtlarına kimlik doğrulamak için kullanılan bir kimlik bilgileri kümesini (anahtarlar ve/veya parolalar) tutar. Microsoft, bu kimlik bilgilerine erişimi engellemek için bir sistem kullanır. Özellikle, bu kimlik bilgilerinin taşınması, kalıcılığı ve kullanımı Azure geliştiricilerinin, yöneticilerinin ve yedekleme hizmetlerinin ve personelin hassas, gizli veya özel bilgilere erişimini önlemek için tasarlanmıştır.

Microsoft, FC'nin ana kimlik ortak anahtarına dayalı şifreleme kullanır. Bu, ağ donanım aygıtlarına erişmek için kullanılan kimlik bilgilerini aktarmak için FC kurulum ve FC yeniden yapılandırma zamanlarında oluşur. FC kimlik bilgilerine ihtiyaç duyduğunda, FC kimlik bilgilerini alır ve şifresini çözer.

### <a name="network-devices"></a>Ağ aygıtları

Azure ağ ekibi, ağ hizmeti hesaplarını, bir Azure istemcisinin ağ aygıtlarına (yönlendiriciler, anahtarlar ve yük bakiyeleri) kimlik doğrulaması yapabilmesini sağlayacak şekilde yapılandırır.

## <a name="secure-service-administration"></a>Güvenli hizmet yönetimi
Azure işlemleri personelinin güvenli yönetici iş istasyonlarını (SAWs) kullanması gerekir. Müşteriler ayrıcalıklı erişim iş istasyonlarını kullanarak benzer denetimleri uygulayabilir. SAWs ile, yönetim personeli kullanıcının standart kullanıcı hesabından ayrı ayrı atanmış bir yönetim hesabı kullanır. SAW, bu hassas hesaplar için güvenilir bir iş istasyonu sağlayarak bu hesap ayırma uygulaması üzerine inşa eder.

## <a name="next-steps"></a>Sonraki adımlar
Microsoft'un Azure altyapısının güvenliğini sağlamaya yardımcı olmak için neler yaptığı hakkında daha fazla bilgi edinmek için bkz:

- [Azure tesisleri, tesisler ve fiziksel güvenlik](physical-security.md)
- [Azure altyapısı kullanılabilirliği](infrastructure-availability.md)
- [Azure ağ mimarisi](infrastructure-network.md)
- [Azure üretim ağı](production-network.md)
- [Azure SQL Veritabanı güvenlik özellikleri](infrastructure-sql.md)
- [Azure üretim işlemleri ve yönetimi](infrastructure-operations.md)
- [Azure altyapı izleme](infrastructure-monitoring.md)
- [Azure altyapı bütünlüğü](infrastructure-integrity.md)
- [Azure müşteri veri koruması](protection-customer-data.md)
