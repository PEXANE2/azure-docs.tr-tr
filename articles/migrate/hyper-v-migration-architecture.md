---
title: Azure Geçişi'nde Hyper-V geçişi nasıl çalışır?
description: Azure Geçişi ile Hyper-V geçişi hakkında bilgi edinin
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 11/19/2019
ms.author: raynew
ms.openlocfilehash: 8bca88fc63a7fc04a22d2a68adbe59259b07f50e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74185872"
---
# <a name="how-does-hyper-v-replication-work"></a>Hyper-V çoğaltma nasıl çalışır?

Bu makalede, Azure Geçiş Sunucusu Geçişi aracıyla Hyper-V VV'leri geçirdiğinizde kullanılan mimari ve işlemlere genel bir bakış sağlanmaktadır.

[Azure Geçiş,](migrate-services-overview.md) şirket içi uygulamalarınızın ve iş yüklerinizin ve özel/genel bulut VM'lerinizin Azure'a keşfini, değerlendirmesini ve geçişini izlemek için merkezi bir hub sağlar. Hub, değerlendirme ve geçiş için Azure Geçiş araçlarının yanı sıra üçüncü taraf bağımsız yazılım satıcısı (ISV) teklifleri sağlar.

## <a name="agentless-migration"></a>Aracısız geçiş

Azure Geçir Sunucusu Geçişi aracı, Hyper-V için en iyi duruma getirilmiş bir geçiş iş akışını kullanarak şirket içi Hyper-V V VM'ler için aracısız çoğaltma sağlar. Yalnızca Hyper-V ana bilgisayarlarına veya küme düğümlerine bir yazılım aracısı yüklersiniz. Hyper-V VM'lere hiçbir şeyin yüklenmesi gerekmesin.

## <a name="server-migration-and-azure-site-recovery"></a>Sunucu Geçişi ve Azure Site Kurtarma

Azure Geçir Sunucusu Geçişi, şirket içi iş yüklerini ve bulut tabanlı VM'leri Azure'a geçirmek için bir araçtır. Site Kurtarma bir felaket kurtarma aracıdır. Araçlar, veri çoğaltma için kullanılan bazı yaygın teknoloji bileşenlerini paylaşır, ancak farklı amaçlara hizmet eder. 


## <a name="architectural-components"></a>Mimari bileşenler

![Mimari](./media/hyper-v-replication-architecture/architecture.png)



**Bileşen** | **Dağıtım** | 
--- | --- 
**Çoğaltma sağlayıcısı** | Microsoft Azure Site Kurtarma sağlayıcısı Hyper-V ana bilgisayarlarına yüklenir ve Azure Geçiş Sunucusu Geçişi'ne kaydedilir.<br/> Sağlayıcı Hyper-V VM'ler için çoğaltmayı yönetir.
**Kurtarma Hizmetleri aracısı** | Microsoft Azure Kurtarma Hizmeti aracısı veri çoğaltma işlemini işler. Hyper-V VM'lerden Azure'a verileri çoğaltmak için sağlayıcıyla birlikte çalışır.<br/> Çoğaltılan veriler Azure aboneliğinizdeki bir depolama hesabına yüklenir. Sunucu Geçişi aracı çoğaltılan verileri işler ve abonelikteki yineleme disklerine uygular. Yineleme diskleri, geçiş yaptığınızda Azure VM'lerini oluşturmak için kullanılır.

- Bileşenler, portaldaki Azure Geçir Sunucusu Geçişi'nden indirilen tek bir kurulum dosyası tarafından yüklenir.
- Sağlayıcı ve cihaz, Azure Geçiş Sunucusu Geçişi ile iletişim kurmak için giden HTTPS bağlantı noktası 443 bağlantı noktasını kullanır.
- Sağlayıcı ve aracının iletişimleri güvenli ve şifrelenir.


## <a name="replication-process"></a>Çoğaltma işlemi

1. Hyper-V VM için çoğaltmayı etkinleştirdiğinizde, ilk çoğaltma başlar.
2. Hyper-V VM anlık görüntüsü alınır.
3. VM'deki VHD'ler, tümü Azure'a kopyalanana kadar tektek çoğaltılır. İlk çoğaltma süresi VM boyutuna ve ağ bant genişliğine bağlıdır.
4. İlk çoğaltma sırasında oluşan disk değişiklikleri Hyper-V Replica kullanılarak izlenir ve günlük dosyalarında (hrl dosyaları) depolanır.
    - Günlük dosyaları disklerle aynı klasördedir.
    - Her diskin ikincil depolama alanına gönderilen ilişkili bir hrl dosyası vardır.
    - İlk çoğaltma sırasında anlık görüntü ve günlük dosyaları disk kaynaklarını kullanır.
4. İlk çoğaltma bittikten sonra VM anlık görüntüsü silinir ve delta çoğaltma başlar.
5. Artımlı disk değişiklikleri hrl dosyalarında izlenir. Çoğaltma günlükleri, Kurtarma Hizmetleri aracısı tarafından düzenli olarak bir Azure depolama hesabına yüklenir.


## <a name="performance-and-scaling"></a>Performans ve ölçeklenebilirlik

Hyper-V için çoğaltma performansı, VM boyutu, VM'lerin veri değiştirme hızı (karmaşası), günlük dosyası depolama için Hyper-V ana bilgisayarda kullanılabilir alan, çoğaltma verileri için bant genişliği yükleme ve Azure'da hedef depolama gibi etkenlerden etkilenir.

- Aynı anda birden çok makineyi çoğaltıyorsanız, çoğaltmayı en iyi duruma getirmek için Hyper-V için [Azure Site Kurtarma Dağıtım Planlayıcısını](../site-recovery/hyper-v-deployment-planner-overview.md) kullanın.
- Hyper-V çoğaltmanızı planlayın ve çoğaltmayı kapasiteye uygun olarak Azure depolama hesapları üzerinden dağıtın.

### <a name="control-upload-throughput"></a>Yükleme iş buzunu kontrol edin

Her Hyper-V ana bilgisayarda Azure'a veri yüklemek için kullanılan bant genişliği miktarını sınırlayabilirsiniz. Dikkat et. Değerleri çok düşük ayarlarsanız, çoğaltmayı olumsuz etkiler ve geçişi geciktirir.


1. Hyper-V ana bilgisayar veya küme düğümünde oturum açın.
2. Windows Azure Yedekleme MMC snap-in'i açmak için **C:\Program Files\Microsoft Azure Kurtarma Hizmetleri Aracısı\bin\wabadmin.msc'yi**çalıştırın.
3. Snap-in'de **Özellikleri Değiştir'i**seçin.
4. **Azaltma'da,** yedekleme **işlemleri için internet bant genişliği kullanımını azaltmayı etkinleştir'i**seçin. Çalışma ve çalışma dışı saatler için sınırları belirleyin. Geçerli aralıklar 512 Kbps ile 1.023 Mbps arasındadır.
I

### <a name="influence-upload-efficiency"></a>Yükleme verimliliğini etkileme

Çoğaltma için yedek bant genişliğiniz varsa ve yüklemeleri artırmak istiyorsanız, yükleme görevi için ayrılan iş parçacığı sayısını aşağıdaki gibi artırabilirsiniz:

1. Kayıt defterini Regedit ile açın.
2. anahtar HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Yedekleme\Çoğaltma\UploadThreadsPerVM gidin
3. Her çoğaltma VM için veri yüklemeiçin kullanılan iş parçacığı sayısı nın değerini artırın. Varsayılan değer 4, en büyük değer 32'dir. 




## <a name="next-steps"></a>Sonraki adımlar

Azure Geçiş Sunucusu Geçişi'ni kullanarak [Hyper-V geçişini](tutorial-migrate-hyper-v.md) deneyin.
