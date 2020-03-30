---
title: Azure'da SAP HANA'nın İşlem modeli (Büyük Örnekler) | Microsoft Dokümanlar
description: Azure'daki SAP HANA'nın işlem modeli (Büyük Örnekler).
services: virtual-machines-linux
documentationcenter: ''
author: msjuergent
manager: bburns
editor: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/04/2018
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e147e4a5f104ca4cd1a10a776c907e3f9f1d6128
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77616968"
---
# <a name="operations-model-and-responsibilities"></a>İşlem modeli ve sorumluluklar

Azure'da SAP HANA (Büyük Örnekler) ile sağlanan hizmet Azure IaaS hizmetleriyle uyumludur. SAP HANA için en iyi duruma getirilmiş yüklü bir işletim sistemine sahip HANA Büyük Örneği örneği alırsınız. Azure IaaS VM'lerde olduğu gibi, işletim sistemi sertleştirme, ek yazılım yükleme, HANA'yı yükleme, işletim sistemi ve HANA'yı çalıştırma ve işletim sistemi ve HANA'yı güncelleme görevlerinin çoğu sizin sorumluluğunuzdadır. Microsoft, işletim sistemi güncelleştirmelerini veya HANA güncelleştirmelerini sizi zorlamaz.

![SAP HANA'nın Azure'daki Sorumlulukları (Büyük Örnekler)](./media/hana-overview-architecture/image2-responsibilities.png)

Diyagramda gösterildiği gibi, Azure'daki SAP HANA (Büyük Örnekler) çok kiracılı bir IaaS teklifidir. Çoğunlukla, sorumluluk bölümü Işletim Sistemi altyapı sınırındadır. Microsoft, işletim sistemi çizgisinin altındaki hizmetin tüm yönlerinden sorumludur. Hattın üzerindeki hizmetin tüm yönlerinden siz sorumlusunuz. İşletim sistemi sizin sorumluluğunuzda. Uyumluluk, güvenlik, uygulama yönetimi, temel ve işletim sistemi yönetimi için kullanabileceğiniz en güncel şirket içi yöntemleri kullanmaya devam edebilirsiniz. Sistemler her açıdan asanızda yer alıyormuş gibi görünür.

Bu hizmet SAP HANA için optimize edilmiştir, bu nedenle en iyi sonuçlar için temel altyapı özelliklerini kullanmak için Microsoft ile birlikte çalışmanız gereken alanlar vardır.

Aşağıdaki liste, katmanların her biri ve sorumluluklarınız hakkında daha fazla ayrıntı sağlar:

**Ağ :** SAP HANA çalıştıran Büyük Örnek damgası için tüm dahili ağlar. Sorumluluğunuz, depolamaya erişim, örnekler arasındaki bağlantı (ölçeklendirme ve diğer işlevler için), yatay bağlantı ve SAP uygulama katmanının VM'lerde barındırıldığı Azure'a bağlantıiçerir. Ayrıca, olağanüstü durum kurtarma amacıyla çoğaltma için Azure Veri Merkezleri arasındaki WAN bağlantısını da içerir. Tüm ağlar kiracı tarafından bölümlere ayrılmıştır ve hizmet kalitesi uygulanır.

**Depolama**: SAP HANA sunucularının ihtiyaç duyduğu tüm birimlerin yanı sıra anlık görüntüler için de sanallaştırılmış bölümlü depolama. 

**Sunucular**: Kiracılara atanan SAP HANA DB'lerini çalıştırmak için özel fiziksel sunucular. SK'lerin Tip I sınıfının sunucuları donanım soyutlanmış. Bu tür sunucularla, sunucu yapılandırması toplanır ve bir fiziksel donanımdan başka bir fiziksel donanıma taşınabilen profillerde korunur. Bir profilin işlemlere göre böyle bir (el ile) hareketi, biraz Azure hizmet iyileştirmeile karşılaştırılabilir. Tip II sınıfı SUS'ların sunucuları böyle bir özellik sunmuyor.

**SDDC**: Veri merkezlerini yazılım tanımlı varlıklar olarak yönetmek için kullanılan yönetim yazılımı. Microsoft'un ölçek, kullanılabilirlik ve performans nedenleriyle kaynakları bir araya etmesini sağlar.

**O/S**: Seçtiğiniz işletim sistemi (SUSE Linux veya Red Hat Linux) sunucularda çalışan. Size sağlanan işletim sistemi görüntüleri, SAP HANA'yı çalıştırmak için tek tek Linux satıcısı tarafından Microsoft'a sağlanmıştır. Belirli SAP HANA için optimize edilmiş görüntü için Linux satıcısıyla aboneliğiniz olmalıdır. Görüntüleri işletim sistemi satıcısına kaydetmeksizin sorumlusunuz. 

Microsoft tarafından devri noktasından itibaren, Linux işletim sisteminin daha fazla yama sorumludur. Bu yama, başarılı bir SAP HANA yüklemesi için gerekli olabilecek ve sap HANA optimize edilmiş işletim sistemi görüntülerine belirli Linux satıcısı tarafından dahil edilmeyen ek paketler içerir. (Daha fazla bilgi için SAP'nin HANA yükleme belgelerine ve SAP Notları'na bakın.) 

Belirli sunucu donanımına göre işletim sistemi ve sürücülerinin arızalanması veya optimizasyonu nedeniyle işletim sistemi yamalarından siz sorumlusunuz. Ayrıca işletim sistemi güvenlik veya işlevsel yama sorumludur. 

Sorumluluğunuz aynı zamanda izleme ve kapasite planlamaiçerir:

- CPU kaynak tüketimi.
- Hafıza tüketimi.
- Boş alan, IOPS ve gecikme yle ilgili disk birimleri.
- HANA Büyük Örnek ve SAP uygulama katmanı arasındaki ağ hacmi trafiği.

HANA Large Instance'ın temel altyapısı, işletim sistemi hacminin yedeklenme ve geri yükleme için işlevsellik sağlar. Bu işlevselliği kullanmak da sizin sorumluluğunuzdadır.

**Middleware**: SAP HANA Örneği, öncelikle. Yönetim, operasyonlar ve izleme sizin sorumluluğunuzdadır. Sağlanan işlevselliği, yedekleme ve geri yükleme ve olağanüstü durum kurtarma amacıyla depolama anlık görüntülerini kullanmak için kullanabilirsiniz. Bu yetenekler altyapı tarafından sağlanmaktadır. Sorumluluklarınız arasında bu özelliklerle yüksek kullanılabilirlik veya olağanüstü durum kurtarma tasarlamak, bunları kullanma ve depolama anlık görüntülerini başarıyla yürütüp yürütülmediğini belirlemek için izleme de yer almaktadır.

**Veri**: Verileriniz SAP HANA tarafından yönetilir ve birim veya dosya paylaşımlarında bulunan yedekleme dosyaları gibi diğer veriler. Sorumluluklarınız arasında disk boş alanını izlemek ve birimlerdeki içeriği yönetmek yer alır. Ayrıca, disk birimlerinin ve depolama anlık görüntülerini yedeklerinin başarılı bir şekilde yürütülmesini izlemekten de siz sorumlusunuz. Veri çoğaltmanın olağanüstü durum kurtarma sitelerine başarılı bir şekilde yürütülmesi Microsoft'un sorumluluğundadır.

**Uygulamalar:** SAP uygulama örnekleri veya SAP olmayan uygulamalar durumunda, bu uygulamaların uygulama katmanı. Sorumluluklarınız bu uygulamaların dağıtımı, yönetimi, işlemleri ve izlenmesini içerir. Sanal ağlardaki CPU kaynak tüketiminin, bellek tüketiminin, Azure Depolama tüketiminin ve ağ bant genişliği tüketiminin kapasite planlamasından siz sorumlusunuz. Ayrıca, sanal ağlardan Azure'da SAP HANA'ya (Büyük Örnekler) kaynak tüketimi için kapasite planlamasından da siz sorumlusunuz.

**WANs**: İş yükleri için şirket içi ve Azure dağıtımlarına kurduğunuz bağlantılar. HANA Large Instance'a sahip tüm müşteriler bağlantı için Azure ExpressRoute'u kullanır. Bu bağlantı, Azure'da SAP HANA (Büyük Örnekler) çözümünün bir parçası değildir. Bu bağlantının kurulumundan siz sorumlusunuz.

**Arşiv**: Depolama hesaplarında kendi yöntemlerinizi kullanarak verilerin kopyalarını arşivlemeyi tercih edebilirsiniz. Arşivleme yönetimi, uyumluluk, maliyetler ve operasyonlar gerektirir. Azure'da arşiv kopyalarını ve yedeklerini oluşturmak ve uyumlu bir şekilde depolamak sizin sorumluluğunuzdadır.

[Azure'da SAP HANA için SLA'ya bakın (Büyük Örnekler)](https://azure.microsoft.com/support/legal/sla/sap-hana-large/).

**Sonraki adımlar**
- [Azure'da SAP HANA (Büyük Örnekler) mimarisine](hana-architecture.md) bakın
