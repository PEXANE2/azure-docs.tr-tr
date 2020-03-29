---
title: Site Kurtarma ile fiziksel sunucular için failover ve failback ayarlama
description: Azure'daki fiziksel sunucular üzerinde nasıl başarısız olunacağınızı ve Azure Site Kurtarma ile olağanüstü durum kurtarma için şirket içi siteye geri dönmeyi öğrenin
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 12/17/2019
ms.author: raynew
ms.openlocfilehash: ea5893f45962d67f4b6f3e9a261c65aa0ec926bf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75497865"
---
# <a name="fail-over-and-fail-back-physical-servers-replicated-to-azure"></a>Azure'a çoğaltılan fiziksel sunucuları başarısız ve başarısız

Bu öğretici, [Azure Site Kurtarma](site-recovery-overview.md)ile Azure'a çoğalan şirket içi fiziksel sunucular üzerinde nasıl başarısız olunacağı açıklanmaktadır. Başarısız olduktan sonra, kullanılabilir olduğunda Azure'dan şirket içi sitenize geri dönmezsiniz.

## <a name="before-you-start"></a>Başlamadan önce

- Olağanüstü durum kurtarma daki başarısızlık süreci hakkında [bilgi edinin.](failover-failback-overview.md)
- Birden çok makinede başarısız olmak istiyorsanız, kurtarma planında makineleri nasıl bir araya topladığınızı [öğrenin.](recovery-plan-overview.md)
- Tam bir arıza yapmadan önce, her şeyin beklendiği gibi çalıştığından emin olmak için bir [olağanüstü durum kurtarma tatbikatı](site-recovery-test-failover-to-azure.md) çalıştırın.
- Başarısız olduktan sonra Azure VM'lere bağlanmaya hazırlanmak için [bu yönergeleri](site-recovery-failover.md#prepare-to-connect-after-failover) izleyin.



## <a name="run-a-failover"></a>Yük devretme çalıştırma

### <a name="verify-server-properties"></a>Sunucu özelliklerini doğrulama

Sunucu özelliklerini doğrulayın ve Azure VM'leri için [Azure gereksinimlerine](vmware-physical-azure-support-matrix.md#replicated-machines) uyduğundan emin olun.

1. **Korumalı**Öğeler'de, **Çoğaltılan Öğeler'i**tıklatın ve makineyi seçin.
2. Yinelenen **öğe** bölmesinde, makine bilgilerinin, sistem durumu durumunun ve kullanılabilir en son kurtarma noktalarının bir özeti bulunur. Daha fazla ayrıntı görüntülemek için **Özellikler**’e tıklayın.
3. **İşlem ve Ağ** bölümünde Azure adını, kaynak grubunu, hedef boyutunu, [kullanılabilirlik kümesini](../virtual-machines/windows/tutorial-availability-sets.md) ve yönetilen disk ayarlarını değiştirebilirsiniz
4. Ağ ayalarını, yük devretmeden sonra Azure VM’nin yerleştirileceği ağ/alt ağ ve VM’ye atanacak IP adresi dahil olmak üzere görüntüleyebilir ve değiştirebilirsiniz.
5. **Diskler'de,** makine işletim sistemi ve veri diskleri hakkındaki bilgileri görebilirsiniz.

### <a name="fail-over-to-azure"></a>Azure'a yük devretme

1. **Ayarlar** > **Çoğaltılan öğeler** de Makine > **Failover**tıklayın.
2. **Yük devretme**’de yük devretmenin yapılacağı bir **Kurtarma Noktası** seçin. Aşağıdaki seçeneklerden birini kullanabilirsiniz:
   - **Varsayılan**: Bu seçenekte öncelikle Site Recovery’ye gönderilen tüm veriler işlenir. Yük devretmeden sonra oluşturulan Azure VM, yük devretme tetiklendiğinde Site Recovery’ye çoğaltılan tüm verilere sahip olduğundan en düşük RPO (Kurtarma Noktası Hedefi) sağlanır.
   - **En son işlenen**: Bu seçenek, Site Kurtarma tarafından işlenen en son kurtarma noktasına makine üzerinde başarısız olur. İşlenmemiş verileri işlemek için zaman harcanmadığından bu seçenekte düşük bir RTO (Kurtarma Süresi Hedefi) sağlanır.
   - **En son uygulama tutarlılığı**: Bu seçenek, Site Kurtarma tarafından işlenen en son uygulama tutarlı kurtarma noktasına makine üzerinde başarısız olur.
   - **Özel**: Bir kurtarma noktası belirtin.

3. Site Kurtarma'nın başarısız olmayı tetiklemeden önce kaynak makineyi kapatmayı denemesini istiyorsanız, başarısız olmadan **önce kapatma makinesini** seçin. Kapatma işlemi başarısız olsa bile yük devretme devam eder. Yük devretme işleminin ilerleme durumunu **İşler** sayfasında takip edebilirsiniz.
4. Azure VM’ye bağlanmaya hazırsanız, yük devretmeden sonra VM’yi doğrulamak için bağlanın.
5. Doğruladıktan sonra yük devretmeyi **Yürütün**. Bu, tüm kullanılabilir kurtarma noktalarını siler.

> [!WARNING]
> Devam etmekte olan bir başarısızı iptal etmeyin. Failover başlamadan önce, makine çoğaltma durur. Başarısız ı iptal ederseniz, durur, ancak makine tekrar kopyalamaz.
> Fiziksel sunucular için, ek hata işlemenin tamamlanması yaklaşık sekiz ila on dakika sürebilir.

## <a name="automate-actions-during-failover"></a>Başarısız olurken eylemleri otomatikleştirin

Başarısız lık sırasında eylemleri otomatikleştirmek isteyebilirsiniz. Bunu yapmak için, kurtarma planlarında komut dosyalarını veya Azure otomasyon runbook'larını kullanabilirsiniz.

- Komut dosyaları ekleme de dahil olmak üzere kurtarma planları oluşturma ve özelleştirme hakkında [bilgi edinin.](site-recovery-create-recovery-plans.md)
- Kurtarma planlarına Azure Otomasyon runbook'ları eklemeyi [öğrenin.](site-recovery-runbook-automation.md)

## <a name="configure-settings-after-failover"></a>Başarısız olduktan sonra ayarları yapılandır

Başarısız olduktan sonra, yinelenen Azure VM'lerine bağlanmak için [Azure ayarlarını yapılandırmanız](site-recovery-failover.md#prepare-in-azure-to-connect-after-failover) gerekir. Buna ek olarak, [dahili ve genel](site-recovery-failover.md#set-up-ip-addressing) IP adresleme ayarlayın.

## <a name="prepare-for-reprotection-and-failback"></a>Yeniden koruma ve geri dönüş için hazırlanın

Azure'da başarısız olduktan sonra, Azure VM'leri şirket içi siteye kopyalayarak yeniden korursunuz. Çoğaldıktan sonra, Azure'dan şirket içi sitenize bir arıza çalıştırarak bunları şirket içi olarak geri alabilirsiniz.

1. Site Kurtarma'yı kullanarak Azure'a çoğaltılan fiziksel sunucular yalnızca VMware VM'ler olarak geri başarısız olabilir. Geri başarısız olmak için bir VMware altyapısıgerekir. Azure'da bir işlem sunucusu ve şirket içi ana hedef sunucu oluşturma ve siteden siteye VPN veya ExpressRoute özel eşlemesini hata geri almak için yapılandırmak da dahil olmak üzere yeniden koruma ve geri dönüşe hazırlanmak için [bu makaledeki](vmware-azure-prepare-failback.md) adımları izleyin.
2. Şirket içi yapılandırma sunucusunun çalışır durumda olduğundan ve Azure'a bağlı olduğundan emin olun. Azure'a geçerken şirket içi siteye erişilemeyebilir ve yapılandırma sunucusu kullanılamayabilir veya kapatılabilir. Geri hata sırasında, VM yapılandırma sunucusu veritabanında bulunmalıdır. Aksi takdirde, failback başarısız olur.
3. Şirket içi ana hedef sunucudaki anlık görüntüleri silin. Anlık görüntüler varsa yeniden koruma çalışmaz.  VM'deki anlık görüntüler, yeniden koruma işi sırasında otomatik olarak birleştirilir.
4. Çoklu VM tutarlılığı için bir çoğaltma grubuna toplanan VM'leri yeniden koruyorsanız, hepsinin aynı işletim sistemine (Windows veya Linux) sahip olduğundan emin olun ve dağıttığınız ana hedef sunucunun aynı işletim sistemi türüne sahip olduğundan emin olun. Çoğaltma grubundaki tüm VM'ler aynı ana hedef sunucuyu kullanmalıdır.
5. Geri dönüş için [gerekli bağlantı noktalarını](vmware-azure-prepare-failback.md#ports-for-reprotectionfailback) açın.
6. Başarısız olmadan önce vCenter Server'ın bağlı olduğundan emin olun. Aksi takdirde, disklerin bağlantısını kesmek ve sanal makineye takmak başarısız olur.
7. Bir vCenter sunucusu başarısız olduğunuz VM'leri yönetiyorsa, gerekli izinlere sahip olduğundan emin olun. Salt okunur kullanıcı vCenter bulma gerçekleştirin ve sanal makineleri korumak, koruma başarılı olur ve başarısız çalışır. Ancak, yeniden koruma sırasında, veri depoları bulunamadığından ve yeniden koruma sırasında listelenmediği için başarısız olur. Bu sorunu gidermek için, vCenter kimlik bilgilerini uygun bir [hesap/izinle](vmware-azure-tutorial-prepare-on-premises.md#prepare-an-account-for-automatic-discovery)güncelleştirebilir ve ardından işi yeniden deneyebilirsiniz. 
8. Sanal makinelerinizi oluşturmak için bir şablon kullandıysanız, her VM'nin diskler için kendi UUID'si olduğundan emin olun. Her ikisi de aynı şablondan oluşturulduğu için şirket içi VM UUID ana hedef sunucunun UUID'si ile çakışıyorsa, yeniden koruma başarısız olur. Farklı bir şablondan dağıtın.
9. Alternatif bir vCenter Server'a geri dönülmeye çalışıyorsanız, yeni vCenter Server ve ana hedef sunucunun bulunduğundan emin olun. Genellikle veri depoları erişilemiyorsa veya **Reprotect'de**görülemiyorsa.
10. Başarısız olamadığınız aşağıdaki senaryoları doğrulayın:
    - ESXi 5.5 ücretsiz sürümü veya vSphere 6 Hypervisor ücretsiz sürümünü kullanıyorsanız. Farklı bir sürüme yükseltin.
    - Windows Server 2008 R2 SP1 fiziksel sunucunuz varsa.
    - [Geçirilen VM'ler.](migrate-overview.md#what-do-we-mean-by-migration)
    - Başka bir kaynak grubuna taşınmış bir VM.
    - Silinmiş bir yineleme Azure VM.
    - Korunmayan bir yineleme Azure VM (şirket içi siteye çoğaltma).
10. Kullanabileceğiniz [geri dönüş türlerini gözden geçirin](concepts-types-of-failback.md) - özgün konum kurtarma ve alternatif konum kurtarma.


## <a name="reprotect-azure-vms-to-an-alternate-location"></a>Azure VM'leri alternatif bir konuma yeniden koruma

Bu yordam, şirket içi VM'nin kullanılmadığını varsayar.

1. Tonoz > **Ayarlar** > **Çoğaltılan öğeler**, Yeniden **Koruma**> üzerinde başarısız olan makineye sağ tıklayın.
2. **Yeniden koru** bölümünde **Azure’dan Şirket içine** seçeneğinin belirlendiğinden emin olun.
3. Şirket içi ana hedef sunucuyu ve işlem sunucusunu belirtin.
4. **Veri deposu**’nda şirket içi diskleri kurtarmak istediğiniz ana hedef veri deposunu seçin.
       - Şirket içi VM silinmişse veya yoksa ve yeni diskler oluşturmanız gerekiyorsa bu seçeneği kullanın.
       - Diskler zaten varsa, bu ayar yoksayılır, ancak bir değer belirtmeniz gerekir.
5. Ana hedef bekletme sürücüsünü seçin. Yeniden çalışma ilkesi otomatik olarak seçilir.
6. Yeniden korumaya başlamak için **Tamam**’a tıklayın. Bir iş, Azure VM'yi şirket içi siteye çoğaltmaya başlar. **İşler** sekmesinde ilerleme durumunu izleyebilirsiniz.

> [!NOTE]
> Azure VM’sini mevcut bir şirket içi VM’ye kurtarmak istiyorsanız, şirket içi sanal makinenin veri deposunu okuma/yazma erişimiyle ana hedef sunucunun ESXi ana bilgisayarına bağlayın.


## <a name="fail-back-from-azure"></a>Azure’dan yeniden çalışma

Yük devretmeyi şu şekilde çalıştırın:

1. **Çoğaltılan Öğeler** sayfasında makineye sağ tıklayın ve **Planlanmamış Yük Devretme** seçeneğini belirleyin.
2. **Yük Devretmeyi Onayla** bölümünde yük devretme yönünün Azure’dan olduğundan emin olun.
3.Başarısız olmak için kullanmak istediğiniz kurtarma noktasını seçin.
    - **En Son** kurtarma noktasını kullanmanızı öneririz. Uygulama tutarlı noktası, zaman içinde en son noktanın gerisinde dir ve bazı veri kaybına neden olur.
    - **En sonuncusu,** çarpışma tutarlı bir kurtarma noktasıdır.
    - Yük devretme çalıştığında Site Recovery, Azure VM’lerini kapatır ve şirket içi VM’yi başlatır. Hizmet bir süre kapalı kalacağından uygun bir zaman seçin.
4. Makineye sağ tıklayın ve **Yürüt**’e tıklayın. Bu, Azure VM’lerini kaldıran bir işi tetikler.
5. Azure VM’lerinin beklenen şekilde kapatıldığından emin olun.


## <a name="reprotect-on-premises-machines-to-azure"></a>Şirket içi makineleri Azure’da yeniden koruma

Veriler artık şirket içi sitenizde tekrar yerleştirilir, ancak Azure’a çoğaltılmaz. Tekrar Azure’a çoğaltmaya başlamak için şunları yapabilirsiniz:

1. Kasa > **Ayarlar** >**Çoğaltılan Öğeler** bölümünde yeniden çalışan VM’leri seçin ve **Yeniden Koru**’ya tıklayın.
2. Çoğaltılan verileri Azure’a göndermek için kullanılan işlem sunucusunu seçin ve **Tamam**’a tıklayın.


## <a name="next-steps"></a>Sonraki adımlar

Yeniden koruma işi bittikten sonra şirket içi VM Azure'a çoğalıyor. Gerektiğinde Azure'a [başka bir hata yapabilirsiniz.](site-recovery-failover.md)
