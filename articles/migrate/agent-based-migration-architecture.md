---
title: Azure Geçiş Sunucusu Geçişi'nde aracı tabanlı geçiş
description: Azure Geçişi'nde aracı tabanlı VMware VM geçişine genel bir bakış sağlar.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 02/17/2020
ms.author: raynew
ms.openlocfilehash: d345d707cbf58f48466c3bd830d93250d13397c6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77425875"
---
# <a name="agent-based-migration-architecture"></a>Aracı tabanlı geçiş mimarisi

Bu makalede, VMware VM'lerin [Azure Geçişi: Sunucu Geçişi](migrate-services-overview.md#azure-migrate-server-assessment-tool) aracıyla aracı tabanlı çoğaltılmasında kullanılan mimari ve işlemlere genel bir bakış sağlanmaktadır.

Azure Geçiş: Sunucu Geçişi'ni kullanarak VMware VM'leri birkaç seçenekle çoğaltabilirsiniz:

- Bu makalede açıklandığı gibi, aracı tabanlı çoğaltma kullanarak VM'leri geçirin.
- Aracısız çoğaltma kullanarak VMware VM'leri geçirin. Bu, üzerlerine herhangi bir şey yüklemeye gerek kalmadan VM'leri geçirin.

VMware VM'ler için geçiş yöntemlerini [seçme ve karşılaştırma](server-migrate-overview.md) hakkında daha fazla bilgi edinin. 


## <a name="agent-based-migration"></a>Aracı tabanlı geçiş

Aracı tabanlı geçiş, şirket içi VMware VM'leri ve fiziksel sunucuları Azure'a geçirmek için kullanılır. Ayrıca, diğer şirket içi sanallaştırılmış sunucuların yanı sıra AWS örnekleri ve GCP VM'leri de dahil olmak üzere özel ve genel bulut VM'leri geçirmek için de kullanılabilir. Azure Geçişi'ndeki aracı tabanlı geçiş, Azure [Site Kurtarma](../site-recovery/site-recovery-overview.md) hizmetinden bazı arka uç işlevleri kullanır.


## <a name="architectural-components"></a>Mimari bileşenler

Diyagram, aracı tabanlı geçişte yer alan bileşenleri göstermektedir.

![Mimari](./media/agent-based-replication-architecture/architecture.png)

Tablo, aracı tabanlı geçiş için kullanılan bileşenleri özetler.

**Bileşen** | **Şey** | **Yükleme**
--- | --- | ---
**Çoğaltma cihazı** | Çoğaltma cihazı (configuration server/process server), şirket içi ortam ve Sunucu Geçişi arasında köprü görevi gören şirket içi bir makinedir. Cihaz, Sunucu Geçişi'nin çoğaltma ve geçiş düzenleyebilmeleri için şirket içi makine envanterini keşfeder. Cihazın iki bileşeni vardır:<br/><br/> **Configuration server**: Sunucu Geçişi'ne bağlanır ve çoğaltmayı koordine eder.<br/> **İşlem sunucusu**: Veri çoğaltma işlemlerini işler. İşlem sunucusu makine verilerini alır, sıkıştırır ve şifreler ve Azure'a gönderir. Azure'da, Sunucu Geçişi verileri yönetilen disklere yazar. | Varsayılan olarak işlem sunucusu çoğaltma cihazındaki yapılandırma sunucusuyla birlikte yüklenir.
**Mobilite hizmeti** | Mobilite hizmeti çoğaltmak ve geçiş yapmak istediğiniz her makineye yüklenen bir aracıdır. Makineden çoğaltma verilerini işlem sunucusuna gönderir. | Mobilite hizmetinin farklı sürümleri için yükleme dosyaları çoğaltma cihazında bulunur. İhtiyacınız olan aracıyı işletim sistemine ve çoğaltmak istediğiniz makinenin sürümüne uygun olarak indirip yüklersiniz.

## <a name="mobility-service-installation"></a>Mobility hizmeti yüklemesi

Mobilite Hizmetini aşağıdaki yöntemleri kullanarak dağıtabilirsiniz:

- **Push installation**: Bir makine için koruma sağladığında Mobilite hizmeti işlem sunucusu tarafından yüklenir. 
- **El ile yükle**: Mobilite hizmetini her makineye UI veya komut istemi aracılığıyla el ile yükleyebilirsiniz.

Mobilite hizmeti, çoğaltma cihazı ve çoğaltılmış makinelerle iletişim kurar. Çoğaltma cihazında, işlem sunucularında veya çoğaltılan makinelerde çalışan virüsten koruma yazılımınız varsa, aşağıdaki klasörler tarama dışında tutulmalıdır:


- C:\Program Files\Microsoft Azure Kurtarma Hizmetleri Aracısı
- C:\ProgramData\ASR
- C:\ProgramData\ASRLogs
- C:\ProgramData\ASRSetupLogs
- C:\ProgramData\LogUploadServiceLogs
- C:\ProgramData\Microsoft Azure Site Kurtarma
- C:\Program Dosyaları (x86)\Microsoft Azure Site Kurtarma
- C:\ProgramData\ASR\agent (Mobilite hizmeti yüklü Windows makinelerde)

## <a name="replication-process"></a>Çoğaltma işlemi

1. Bir makine için çoğaltmayı etkinleştirdiğinizde, Azure'a ilk çoğaltma başlar.
2. İlk çoğaltma sırasında, Mobilite hizmeti makine disklerinden gelen verileri okur ve işlem sunucusuna gönderir.
3. Bu veriler, Azure aboneliğinizde diskin bir kopyasını tohumlamak için kullanılır. 
4. İlk çoğaltma işlemi bittikten sonra, Delta değişikliklerinin Azure'da çoğaltılması başlar. Çoğaltma blok düzeyinde ve neredeyse süreklidir.
4. Mobilite hizmeti, işletim sisteminin depolama alt sistemiyle tümleştirerek disk belleğine yazmayı keser. Bu yöntem, artımlı çoğaltma için çoğaltma makinesinde disk G/Ç işlemlerini önler. 
5. Bir makine için izlenen değişiklikler gelen bağlantı noktası HTTPS 9443'teki işlem sunucusuna gönderilir. Bu bağlantı noktası değiştirilebilir. İşlem sunucusu sıkıştırır ve şifreler ve Azure'a gönderir. 

## <a name="ports"></a>Bağlantı Noktaları

**Cihaz** | **Bağlantı**
--- | --- 
**Çoğaltma makineleri** | VM'lerde çalışan Mobilite hizmeti, çoğaltma yönetimi için HTTPS 443 gelen bağlantı noktasındaki şirket içi çoğaltma cihazıyla iletişim kurar.<br/><br/> Makineler, https 9443 bağlantı noktasındaki işlem sunucusuna çoğaltma verileri gönderir. Bu bağlantı noktası değiştirilebilir.
**Çoğaltma cihazı** | Çoğaltma cihazı, https 443 giden bağlantı noktası üzerinden Azure ile çoğaltmayı yönetir.
**İşlem sunucusu** | İşlem sunucusu çoğaltma verilerini alır, en iyi duruma getirir ve şifreler ve 443 bağlantı noktası üzerinden Azure depolamasına gönderir.


## <a name="performance-and-scaling"></a>Performans ve ölçeklenebilirlik

Varsayılan olarak, hem yapılandırma sunucusunu hem de işlem sunucusunu çalıştıran tek bir çoğaltma cihazı dağısınız. Yalnızca birkaç makineyi kopyalayorsanız, bu dağıtım yeterlidir. Ancak, yüzlerce makineyi çoğaltıyor ve geçiş iyorsanız, tek bir işlem sunucusu tüm çoğaltma trafiğini işleyebilir. Bu durumda, ek, ölçeklendirme işlem sunucuları dağıtabilirsiniz.

### <a name="plan-vmware-deployment"></a>VMware dağıtım Planı

VMware VM'leri çoğaltırsanız, günlük veri değiştirme oranı ve ihtiyacınız olan işlem sunucuları da dahil olmak üzere performans gereksinimlerini belirlemeye yardımcı olmak [için VMware için Site Kurtarma Dağıtım Planlayıcısını](../site-recovery/site-recovery-deployment-planner.md)kullanabilirsiniz.

### <a name="replication-appliance-capacity"></a>Çoğaltma cihazı kapasitesi

Dağıtımınızda ek bir işlem sunucusuna ihtiyacınız olup olmadığını anlamak için bu tablodaki değerleri kullanın.

- Günlük değişim oranınız (karmaşa hızı) 2 TB'nin üzerindeyse, ek bir işlem sunucusu dağıtın.
- 200'den fazla makineyi çoğaltıyorsanız, ek bir çoğaltma cihazı dağıtın.

**CPU** | **Bellek** | **Boş alan veri önbelleğe alma** | **Çalkalama oranı** | **Çoğaltma sınırları**
--- | --- | --- | --- | ---
8 vCPUs (2 soket \@ * 4 çekirdek 2.5 GHz) | 16 GB | 300 GB | 500 GB veya daha az | < 100 makine 
12 vCPUs (2 soket \@ * 6 çekirdek 2.5 GHz) | 18 GB | 600 GB | 501 GB - 1 TB | 100-150 makine.
16 vCPUs (2 soket \@ * 8 çekirdek 2.5 GHz) | 32 G1 |  1 TB | 1 TB - 2 TB | 151-200 makine.

### <a name="sizing-scale-out-process-servers"></a>Ölçeklendirme işlem sunucularını boyutlandırma

Bir ölçeklendirme işlem sunucusu dağıtmanız gerekiyorsa, sunucu boyutlandırmasını bulmak için bu tabloyu kullanın.

**İşlem sunucusu** | **Veri önbelleğe alma için boş alan** | **Çalkalama oranı** | **Çoğaltma sınırları**
--- | --- | --- | --- 
4 vCPUs (2 soket \@ * 2 çekirdek 2,5 GHz), 8 GB bellek | 300 GB | 250 GB veya daha az | 85 makineye kadar 
8 vCPUs (2 soket \@ * 4 çekirdek 2,5 GHz), 12 GB bellek | 600 GB | 251 GB - 1 TB    | 86-150 makine.
12 vCPUs (2 soket \@ * 6 çekirdek 2,5 GHz), 24 GB bellek | 1 TB | 1-2 TB | 151-225 makine.

## <a name="throttle-upload-bandwidth"></a>Yükleme bant genişliğini daralt.

Azure'a çoğalan VMware trafiği belirli bir işlem sunucusundan geçer. İşlem sunucusu olarak çalışan makinelerde bant genişliğini daraltarak yükleme iş buzunu sınırlandırabilirsiniz. Bu kayıt defteri anahtarını kullanarak bant genişliğini etkileyebilirsiniz:

- HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Yedekleme\Çoğaltma\UploadThreadsPerVM kayıt defteri değeri, bir diskin veri aktarımı (ilk veya delta çoğaltma) için kullanılan iş parçacığı sayısını belirtir. Daha yüksek bir değer çoğaltma için kullanılan ağ bant genişliğini artırır. Varsayılan değer dörteder. Maksimum değer 32'dir. Değeri iyileştirmek için trafiği izleyin.
- Buna ek olarak, işlem sunucusu makinesinde bant genişliğini aşağıdaki gibi azaltabilirsiniz:

    1. İşlem sunucusu makinesinde Azure Yedekleme MMC snap-in'ini açın. Masaüstünde veya C:\Program Files\Microsoft Azure Kurtarma Hizmetleri Aracısı\bin klasöründe bir kısayol vardır. 
    2. Snap-in'de **Özellikleri Değiştir'i**seçin.
    3. **Azaltma'da,** yedekleme **işlemleri için internet bant genişliği kullanımını azaltmayı etkinleştir'i**seçin. Çalışma ve çalışma dışı saatler için sınırları belirleyin. Geçerli aralıklar 512 Kbps ile 1.023 Mbps arasındadır.


## <a name="next-steps"></a>Sonraki adımlar

[VMware](tutorial-migrate-vmware-agent.md) veya [fiziksel sunucular](tutorial-migrate-physical-virtual-machines.md)için [aracı tabanlı geçiş](tutorial-migrate-vmware-agent.md) deneyin.
