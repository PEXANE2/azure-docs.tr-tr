---
title: Sanal makineyi başka bir bölgeye taşıma (Azure Site Kurtarma)
description: Azure'da SQL Server sanal makinenizi bir bölgeden diğerine nasıl geçirebileceğinizi öğrenin.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: jroth
tags: azure-resource-manager
ms.assetid: aa5bf144-37a3-4781-892d-e0e300913d03
ms.service: virtual-machines-sql
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 07/30/2019
ms.author: mathoma
ms.reviewer: jroth
ms.custom: seo-lt-2019
ms.openlocfilehash: 3b84119cdcc1bb7e8603de64e3d23c69dac70cc3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74022302"
---
# <a name="move-sql-server-vm-to-another-region-within-azure-with-azure-site-recovery-services"></a>Azure Site Kurtarma hizmetleriyle SQL Server VM'yi Azure içindeki başka bir bölgeye taşıyın

Bu makalede, SQL Server sanal makinenizi (VM) Azure'da bir bölgeden diğerine geçirmek için Azure Site Kurtarma'yı nasıl kullanacağınızı öğretilir. 

BIR SQL Server VM'nin farklı bir bölgeye taşınması için aşağıdakileri yapmak gerekmektedir:
1. [**Hazırlama**](#prepare-to-move): Hem kaynak SQL Server VM'nizin hem de hedef bölgenizin taşınmaya uygun şekilde hazırlandığını doğrulayın. 
1. [**Yapılandırma**](#configure-azure-site-recovery-vault): SQL Server VM'nizi taşımak, Azure Site Kurtarma kasasında çoğaltılmış bir nesne olmasını gerektirir. SQL Server VM'nizi Azure Site Kurtarma kasasına eklemeniz gerekir. 
1. [**Sınama**](#test-move-process): SQL Server VM'nin geçirilmesi, kaynak bölgeden çoğaltılan hedef bölgeye geçirilemeyi gerektirir. Taşıma işleminin başarılı olmasını sağlamak için öncelikle SQL Server VM'nizin hedef bölgeye başarılı bir şekilde geçebileceğini test etmeniz gerekir. Bu, herhangi bir sorunu ortaya çıkarmave gerçek hareketi gerçekleştirirken bunlardan kaçınmaya yardımcı olur. 
1. [**Taşıma**](#move-the-sql-server-vm): Test başarısızlığı geçtikten ve SQL Server VM'nizi geçirebileceğinizi bildiğinizde, VM'nin hedef bölgeye hareketini gerçekleştirebilirsiniz. 
1. [**Temizleme**](#clean-up-source-resources): Faturalama ücretlerinden kaçınmak için SQL Server VM'yi kasadan ve kaynak grubunda kalan gereksiz kaynakları kaldırın. 

## <a name="verify-prerequisites"></a>Önkoşulları doğrulama 

- Kaynak bölgenizden hedef bölgenize geçmenin [desteklendigini](../../../site-recovery/azure-to-azure-support-matrix.md#region-support)onaylayın.  
- Senaryo [mimarisini ve bileşenlerini](../../../site-recovery/azure-to-azure-architecture.md) ve [destek sınırlamalarını ve gereksinimlerini](../../../site-recovery/azure-to-azure-support-matrix.md)gözden geçirin. 
- Hesap izinlerini doğrulayın. Ücretsiz Azure hesabınızı oluşturduysanız, aboneliğinizin yöneticisisizsiniz. Abonelik yöneticisi değilseniz, ihtiyacınız olan izinleri atamak için yöneticiyle birlikte çalışın. Bir VM için çoğaltmayı etkinleştirmek ve Azure Site Kurtarma'yı kullanarak verileri kopyalamak için şunları sahip olmalısınız: 
    - VM oluşturma izinleri. *Sanal Makine Katılımcısı* yerleşik rolü, aşağıdakileri içeren şu izinlere sahiptir: 
        - Seçili kaynak grubunda VM oluşturma izinleri. 
        - Seçili sanal ağda VM oluşturma izinleri. 
        - Seçili depolama hesabına yazma izinleri. 
      - Azure Site Kurtarma işlemlerini yönetme izinleri. *Site Kurtarma Katılımcısı* rolü, Kurtarma Hizmetleri kasasında Site Kurtarma işlemlerini yönetmek için gereken tüm izinlere sahiptir.  

## <a name="prepare-to-move"></a>Hareket etmeye hazırlanın
Hareket için hem kaynak SQL Server VM'yi hem de hedef bölgeyi hazırlayın. 

### <a name="prepare-the-source-sql-server-vm"></a>Kaynak SQL Server VM'yi hazırlama

- Taşımak istediğiniz en son kök sertifikaların tümünü SQL Server VM'de olduğundan emin olun. En son kök sertifikalar yoksa, güvenlik kısıtlamaları hedef bölgeye veri kopyalanmasını engeller. 
- Windows VM'ler için, tüm güvenilir kök sertifikaların makinede olması için VM'deki en son Windows güncelleştirmelerinin tümünü yükleyin. Bağlantısı kesilen bir ortamda, kuruluşunuz için standart Windows UPdate ve sertifika güncelleştirme işlemini izleyin. 
- Linux VM'leri için, VM'deki en son güvenilir kök sertifikalarını ve sertifika iptal listesini almak için Linux dağıtımcınız tarafından sağlanan kılavuzu izleyin. 
- Taşımak istediğiniz VM'lerin ağ bağlantısını denetlemek için kimlik doğrulama proxy'si kullanmadığınızdan emin olun. 
- Taşımaya çalıştığınız VM'nin Internet erişimi yoksa veya giden erişimi denetlemek için bir güvenlik duvarı proxy'si kullanıyorsa, gereksinimleri denetleyin. 
- Kaynak ağ düzenini ve şu anda kullanmakta olduğunuz tüm kaynakları tanımlayın. Bu, yük dengeleyicileri, ağ güvenlik gruplarını (NSG'ler) ve genel IP'leri içerir, ancak bunlarla sınırlı değildir. 

### <a name="prepare-the-target-region"></a>Hedef bölgeyi hazırlama

- Azure aboneliğinizin, olağanüstü durum kurtarma için kullanılan hedef bölgede VM'ler oluşturmanıza izin verdiğini doğrulayın. Gerekli kotayı sağlamak için desteğe başvurun. 
- Aboneliğinizin kaynak VM'lerinizle eşleşen büyük lüğe sahip VM'leri desteklemek için yeterli kaynağa sahip olduğundan emin olun. Verileri hedefe kopyalamak için Site Kurtarma'yı kullanıyorsanız, Site Kurtarma aynı boyutu veya hedef VM için mümkün olan en yakın boyutu seçer. 
- Kaynak ağ düzeninde tanımlanan her bileşen için bir hedef kaynak oluşturduğunuzdan emin olun. Bu adım, VM'lerinizin kaynak bölgede bulunan hedef bölgedeki tüm işlevselliğe ve özelliklere sahip olduğundan emin olmak için önemlidir. 
    - Azure Site Kurtarma, kaynak VM için çoğaltmayı etkinleştirdiğinizde otomatik olarak sanal bir ağ keşfeder ve oluşturur. Ayrıca, çoğaltmayı etkinleştirmek için bir ağı önceden oluşturabilir ve kullanıcı akışındaki VM'ye atayabilirsiniz. Hedef bölgedeki diğer kaynakları el ile oluşturmanız gerekir.
- Kaynak VM yapılandırmasına dayalı olarak sizin için alakalı en yaygın kullanılan ağ kaynaklarını oluşturmak için aşağıdaki belgelere bakın: 
    - [Ağ güvenlik grupları](../../../virtual-network/tutorial-filter-network-traffic.md) 
    - [Yük dengeleyici](../../../load-balancer/tutorial-load-balancer-basic-internal-portal.md)
    - [Genel IP adresi](../../../virtual-network/virtual-network-public-ip-address.md)
    - Ek ağ bileşenleri için [ağ belgelerine](../../../virtual-network/virtual-networks-overview.md)bakın.
- Hedef bölgeye son hareketi gerçekleştirmeden önce yapılandırmayı sınamak istiyorsanız, hedef bölgede üretim dışı bir ağ el ile oluşturun. Üretim ağına minimum müdahale sağladığından bu adımı öneririz. 

## <a name="configure-azure-site-recovery-vault"></a>Azure Site Kurtarma kasası yapılandırma

Aşağıdaki adımlar, verileri hedef bölgeye kopyalamak için Azure Site Kurtarma'yı nasıl kullanacağınızı gösterir. Kurtarma Hizmetleri kasasını kaynak bölge dışındaki herhangi bir bölgede oluşturun. 

1. [Azure portalında](https://portal.azure.com)oturum açın. 
1. Gezinti bölmesinin sol üst köşesinden **kaynak oluşturmayı** seçin. 
1. **BT & Yönetimi araçlarını** seçin ve ardından Yedekleme ve Site **Kurtarma'yı**seçin. 
1. Temel **Bilgiler** sekmesinde, **Proje ayrıntıları**altında, hedef bölgede yeni bir kaynak grubu oluşturun veya hedef bölgede varolan bir kaynak grubu seçin. 
1. **Örnek Ayrıntıları**altında, kasanız için bir ad belirtin ve ardından açılan yerden hedef **Bölgenizi** seçin. 
1. Kurtarma Hizmetleri kasanızı oluşturmak için **Gözden Geçir + Oluştur'u** seçin. 
1. Gezinti **All services** bölmesinin sol üst köşesinden ve arama kutusu türünden `recovery services`tüm hizmetleri seçin. 
1. (İsteğe bağlı olarak) Hızlı gezinme çubuğuna eklemek için **Kurtarma Hizmetleri kasalarının** yanındaki yıldızı seçin. 
1. **Kurtarma hizmetleri kasalarını** seçin ve ardından oluşturduğunuz Kurtarma Hizmetleri kasasını seçin. 
1. Genel **Bakış** bölmesine, **Çoğaltma'yı**seçin. 

   ![Çoğaltmayı yapılandırma](media/virtual-machines-windows-sql-move-to-new-region/configure-replication.png)

1. **Kaynak'ı** seçin ve ardından kaynak olarak **Azure'u** seçin. Kaynak VM'lerinizin konumu gibi diğer açılır alanlar için uygun değerleri seçin. **Kaynak kaynak grubu** alanında yalnızca Kaynak **konum** bölgesinde bulunan kaynak grupları görünür. 
1. **Sanal makineleri** seçin ve sonra geçirmek istediğiniz sanal makineleri seçin. VM seçiminizi kaydetmek için **Tamam'ı** seçin. 
1. **Ayarlar'ı**seçin ve açılan konumdan **Hedef konumunuzu** seçin. Bu, daha önce hazırladığınız kaynak grubu olmalıdır. 
1. Çoğaltmayı özelleştirdikten sonra, yeni konumdaki kaynakları oluşturmak için **hedef kaynakları** oluştur'u seçin. 
1. Kaynak oluşturma tamamlandıktan sonra, KAYNAKTAN hedef bölgeye SQL Server VM'nizin çoğaltılmasını başlatmak için **çoğaltmayı etkinleştir'i** seçin.
1. Kurtarma tonozunuza gidip **Çoğaltılan öğeleri** seçerek ve SQL Server VM'nizin **durumunu** görüntüleyerek çoğaltma durumunu kontrol edebilirsiniz. **Korumalı** bir durum çoğaltma tamamlandığını gösterir. 

   ![Çoğaltma durumunu doğrulama](media/virtual-machines-windows-sql-move-to-new-region/check-replication-status.png)

## <a name="test-move-process"></a>Test taşıma işlemi
Aşağıdaki adımlar, taşıma işlemini test etmek için Azure Site Kurtarma'yı nasıl kullanacağınızı gösterir. 

1. [Azure portalındaki](https://portal.azure.com) **Kurtarma Hizmetleri kasanıza** gidin ve **Çoğaltılmış öğeleri**seçin. 
1. Taşımak istediğiniz SQL Server VM'yi seçin, **Çoğaltma** **Durumu'nun Sağlıklı** olarak göründüğünü doğrulayın ve ardından **Failover Test'i**seçin. 

   ![VM'niz için test başarısız](media/virtual-machines-windows-sql-move-to-new-region/test-failover-of-replicated-vm.png)

1. Test **Failover** sayfasında, SQL Server veri tutarlılığını garanti eden tek anlık görüntü türü olduğundan, başarısız olmak için kullanılacak **en son uygulama tutarlı** kurtarma noktasını seçin. 
1. **Azure sanal ağı** altında sanal ağı seçin ve ardından başarısız olmayı test etmek için **Tamam'ı** seçin. 
   
   >[!IMPORTANT]
   > Başarısızlık testi için ayrı bir Azure VM ağı kullanmanızı öneririz. Çoğaltmayı etkinleştirdiğinizde ayarlanan ve VM'lerinizi eninde sonunda taşımak istediğiniz üretim ağını kullanmayın. 

1. İlerlemeyi izlemek için kasanıza gidin, **İzleme**altında **Site Kurtarma işlerini** seçin ve ardından devam eden Test başarısız **işini** seçin.

   ![Başarısız test ilerlemesini izleme](media/virtual-machines-windows-sql-move-to-new-region/monitor-failover-test-job.png)

1. Test tamamlandıktan sonra, portaldaki **Sanal makinelere** gidin ve yeni oluşturulan sanal makineyi gözden geçirin. SQL Server VM'nin çalışırken, uygun boyutlandırılmış olduğundan ve uygun ağa bağlı olduğundan emin olun. 
1. **Failover** seçeneği, başarısız test kaynakları temizlenene kadar gri renkte olacağından, testin bir parçası olarak oluşturulan VM'yi silin. Kasaya geri gidin, **Çoğaltılan öğeleri**seçin, SQL Server VM'yi seçin ve ardından **Temizleme testi başarısız olmasını**seçin. **Notlar** bölümünde testle ilişkili gözlemleri kaydedin ve kaydedin ve Test'in yanındaki onay kutusunu seçin **tamamlandı. Sanal makineler üzerinden test failover silin.** Testten sonra kaynakları temizlemek için **Tamam'ı** seçin. 

   ![başarısız testten sonra öğeleri temizleme](media/virtual-machines-windows-sql-move-to-new-region/cleanup-test-items.png)

## <a name="move-the-sql-server-vm"></a>SQL Server VM'yi taşıma 
Aşağıdaki adımlar, SQL Server VM'yi kaynak bölgenizden hedef bölgenize nasıl taşıyabileceğinizi gösterir. 

1. **Kurtarma Hizmetleri** kasasına gidin, **Çoğaltılan öğeleri**seçin, VM'yi seçin ve ardından **Failover'ı**seçin. 

   ![Başarısızı başlatma](media/virtual-machines-windows-sql-move-to-new-region/initiate-failover.png)

1. **Recovery Point**altında en son **uygulama tutarlı** kurtarma noktasını seçin. 
1. Başarısız olmadan önce **makineyi kapatın'ın**yanındaki onay kutusunu seçin. Site Kurtarma failover tetiklemeden önce kaynak VM kapatmaya çalışır. Kapatma başarısız olsa bile failover devam edecektir. 
1. Başarısız olmayı başlatmak için **Tamam'ı** seçin.
1. Önceki bölümde ki başarısızlık testini izlerken görüntülediğiniz aynı **Site Kurtarma işleri** sayfasından hata devam etme işlemini izleyebilirsiniz. 
1. İş tamamlandıktan sonra, SQL Server VM'nin beklendiği gibi hedef bölgede görünün. 
1. Kasaya geri gidin, **Çoğaltılan Öğeler'i**seçin, SQL Server VM'yi seçin ve hedef bölgeye taşıma işlemini tamamlamak için **Commit'i** seçin. Taahhüt işi bitene kadar bekleyin. 
1. Sql Server VM'inizi SQL VM kaynak sağlayıcısına kaydedin ve Azure portalında ve kaynak sağlayıcısıyla ilişkili özelliklerde **SQL sanal makine** yönetilebilirliğini etkinleştirin. Daha fazla bilgi için sql [VM kaynak sağlayıcısıile SQL Server VM'ye kaydolun.](virtual-machines-windows-sql-register-with-rp.md) 

  > [!WARNING]
  > SQL Server veri tutarlılığı yalnızca uygulama tutarlı anlık görüntüleri ile garanti edilir. En **son işlenen** anlık görüntü, bir kilitlenme kurtarma anlık görüntüsü SQL Server veri tutarlılığını garanti edemez gibi SQL Server failover için kullanılamaz. 

## <a name="clean-up-source-resources"></a>Kaynak kaynaklarını temizleme
Faturalama ücretlerini önlemek için SQL Server VM'yi kasadan kaldırın ve gereksiz ilişkili kaynakları silin. 

1. **Site Kurtarma** kasasına geri gidin, **Çoğaltılan öğeleri**seçin ve SQL Server VM'yi seçin. 
1. **Çoğaltmayı Devre Dışı'** yı seçin. Korumayı devre dışı bırakmak için bir neden seçin ve ardından çoğaltmayı devre dışı bırakmak için **Tamam'ı** seçin. 

   >[!IMPORTANT]
   > Azure Site Kurtarma çoğaltma için ücretlendirilmekten kaçınmak için bu adımı gerçekleştirmek önemlidir. 

1. Kaynak bölgedeki kaynaklardan herhangi birini yeniden kullanmayı planlıyorsanız, ilgili tüm ağ kaynaklarını ve ilgili depolama hesaplarını silin. 


## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki makalelere bakın: 

* [Windows VM'de SQL Server'a Genel Bakış](virtual-machines-windows-sql-server-iaas-overview.md)
* [Windows VM SSS'de SQL Server](virtual-machines-windows-sql-server-iaas-faq.md)
* [Windows VM fiyatlandırma kılavuzunda SQL Server](virtual-machines-windows-sql-server-pricing-guidance.md)
* [Windows VM sürüm notlarında SQL Server](virtual-machines-windows-sql-server-iaas-release-notes.md)


