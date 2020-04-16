---
title: Azure Site Kurtarma ile VMware/fiziksel olağanüstü durum kurtarmayı ölçeklendirin
description: Azure Site Kurtarma ile çok sayıda şirket içi VMware VM'si veya fiziksel sunucu için Azure'da olağanüstü durum kurtarma yı nasıl ayarlayacağınızı öğrenin.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 11/14/2019
ms.author: raynew
ms.openlocfilehash: a3a2317554f02dc1f1198d8019bbfdb50e3cc71c
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81409762"
---
# <a name="set-up-disaster-recovery-at-scale-for-vmware-vmsphysical-servers"></a>VMware VM'ler/fiziksel sunucular için ölçekte olağanüstü durum kurtarma yı ayarlama

Bu makalede, [Azure Site Kurtarma](site-recovery-overview.md) hizmetini kullanarak, üretim ortamınızdaki çok sayıda (1000 >) büyük sayılar için Azure'da olağanüstü durum kurtarma nın nasıl ayarlanır.


## <a name="define-your-bcdr-strategy"></a>BCDR stratejinizi tanımlayın

İş sürekliliği ve olağanüstü durum kurtarma (BCDR) stratejinizin bir parçası olarak, iş uygulamalarınız ve iş yükleriniz için kurtarma noktası hedeflerini (RDO' lar) ve kurtarma süresi hedeflerini (RTO' lar) tanımlarsınız. RTO, süreklilik sorunlarını önlemek için bir iş uygulamasının veya işleminin geri yüklenmesi ve kullanılabilir olması gereken süreyi ve hizmet düzeyini ölçer.
- Site Kurtarma VMware VM'ler ve fiziksel sunucular için sürekli çoğaltma ve RTO için bir [SLA](https://azure.microsoft.com/support/legal/sla/site-recovery/v1_2/) sağlar.
- VMware VM'ler için büyük ölçekli olağanüstü durum kurtarma planı ve ihtiyacınız olan Azure kaynaklarını bulmak için, kapasite hesaplamaları için kullanılacak bir RTO değeri belirtebilirsiniz.


## <a name="best-practices"></a>En iyi uygulamalar

Büyük ölçekli olağanüstü durum kurtarma için bazı genel en iyi uygulamalar. Bu en iyi uygulamalar belgenin sonraki bölümlerinde daha ayrıntılı olarak ele alınmıştır.

- **Hedef gereksinimleri belirleyin:** Olağanüstü durum kurtarma yı ayarlamadan önce Azure'daki kapasite ve kaynak gereksinimlerini tahmin edin.
- **Site Kurtarma bileşenleri için plan**: Tahmini kapasitenizi karşılamak için hangi Site Kurtarma bileşenlerini (yapılandırma sunucusu, işlem sunucuları) bulmanız gerektiğini öğrenin.
- **Bir veya daha fazla ölçeklendirme işlem sunucusu ayarlama**: Yapılandırma sunucusunda varsayılan olarak çalışan işlem sunucusunu kullanmayın. 
- **En son güncelleştirmeleri çalıştırın**: Site Kurtarma ekibi, Site Kurtarma bileşenlerinin yeni sürümlerini düzenli olarak yayımlar ve en son sürümleri çalıştırdığınızdan emin olmalısınız. Bu soruna yardımcı olmak için, güncelleştirmelerin [yenilikleri](site-recovery-whats-new.md) izleyin ve güncelleştirmeleri yayımladıkları nda [etkinleştirin ve yükleyin.](service-updates-how-to.md)
- **Proaktif olarak izleyin**: Olağanüstü durum kurtarma yı pratisye ve çalıştırırken, çoğaltılan makinelerin durumunu ve durumunu ve altyapı kaynaklarını proaktif olarak izlemelisiniz.
- **Olağanüstü durum kurtarma matkapları**: Olağanüstü durum kurtarma matkaplarını düzenli olarak çalıştırmalısınız. Bunlar üretim ortamınızı etkilemez, ancak Azure'a geçememesi gerektiğinde beklendiği gibi çalışacağından emin olun.



## <a name="gather-capacity-planning-information"></a>Kapasite planlama bilgilerini toplama

Hedef (Azure) kapasite gereksinimlerinizi değerlendirmenize ve tahmin etmeye yardımcı olmak için şirket ortamınız hakkında bilgi toplayın.
- VMware için, bunu yapmak için VMware VM'ler için Dağıtım Planlayıcısını çalıştırın.
- Fiziksel sunucular için bilgileri el ile toplayın.

### <a name="run-the-deployment-planner-for-vmware-vms"></a>VMware VM'ler için Dağıtım Planlayıcısını Çalıştırma

Dağıtım Planlayıcısı, VMware şirket ortamınız hakkında bilgi toplamanıza yardımcı olur.

- Dağıtım Planlayıcısını, VM'leriniz için tipik karmaşayı temsil eden bir dönemde çalıştırın. Bu daha doğru tahminler ve öneriler üretecektir.
- Planlayıcı, üzerinde çalıştığını sunucudan elde edilen iş verisini hesapladığı için Dağıtım Planlayıcısını yapılandırma sunucusu makinesinde çalıştırmanızı öneririz. Iş tükeni ölçme hakkında [daha fazla bilgi edinin.](site-recovery-vmware-deployment-planner-run.md#get-throughput)
- Henüz bir yapılandırma sunucunuz yoksa:
    - Site Kurtarma bileşenlerine [genel bakış alın.](vmware-physical-azure-config-process-server-overview.md)
    - Dağıtım Planlayıcısını çalıştırmak için [bir yapılandırma sunucusu ayarlayın.](vmware-azure-deploy-configuration-server.md)

Ardından Planlayıcıyı aşağıdaki gibi çalıştırın:

1. Dağıtım Planlayıcısı [hakkında bilgi edinin.](site-recovery-deployment-planner.md) Portaldan en son sürümü indirebilir veya [doğrudan indirebilirsiniz.](https://aka.ms/asr-deployment-planner)
2. Dağıtım Planlayıcısı için ön koşulları ve [en son güncelleştirmeleri](site-recovery-deployment-planner-history.md) gözden geçirin ve aracı [indirip ayıklayın.](site-recovery-deployment-planner.md#download-and-extract-the-deployment-planner-tool) [prerequisites](site-recovery-deployment-planner.md#prerequisites)
3. [Dağıtım Planlayıcısını](site-recovery-vmware-deployment-planner-run.md) yapılandırma sunucusunda çalıştırın.
4. Tahminleri ve önerileri özetlemek için [bir rapor oluşturun.](site-recovery-vmware-deployment-planner-run.md#generate-report)
5. Rapor [önerilerini](site-recovery-vmware-deployment-planner-analyze-report.md) ve [maliyet tahminlerini](site-recovery-vmware-deployment-planner-cost-estimation.md)analiz edin.

>[!NOTE]
> Varsayılan olarak, araç profile göre yapılandırılır ve 1000 VM'ye kadar rapor oluşturur. ASRDeploymentPlanner.exe.config dosyasındaki MaxVMsDestekli anahtar değerini artırarak bu sınırı değiştirebilirsiniz.

## <a name="plan-target-azure-requirements-and-capacity"></a>Hedef (Azure) gereksinimlerini ve kapasitesini planlama

Toplanan tahminlerinizi ve önerilerinizi kullanarak, hedef kaynakları ve kapasiteyi planlayabilirsiniz. VMware VM'ler için Dağıtım Planlayıcısını çalıştırıyorsanız, size yardımcı olmak için bir dizi [rapor önerisi](site-recovery-vmware-deployment-planner-analyze-report.md#recommendations) kullanabilirsiniz.

- **Uyumlu VM'ler**: Azure'da olağanüstü durum kurtarma için hazır vm sayısını belirlemek için bu numarayı kullanın. Ağ bant genişliği ve Azure çekirdekleri hakkındaki öneriler bu sayıya dayanır.
- **Gerekli ağ bant genişliği**: Uyumlu VM'lerin delta çoğaltması için ihtiyacınız olan bant genişliğini not edin. 
    - Planlayıcıyı çalıştırdığınızda istediğiniz RPO'yu dakikalar içinde belirtirsiniz. Öneriler, rpo%100 ve %90 zaman adedikarşılamak için gereken bant genişliğini gösterir. 
    - Ağ bant genişliği önerileri, Planlayıcı'da önerilen toplam yapılandırma sunucusu ve işlem sunucusu sayısı için gereken bant genişliğini dikkate alır.
- **Gerekli Azure çekirdekleri**: Uyumlu VM sayısına bağlı olarak hedef Azure bölgesinde gereksinim duyduğunuz çekirdek sayısına dikkat edin. Yeterli çekirdeğiniz yoksa, başarısız site kurtarma sırasında gerekli Azure VM'lerini oluşturamazsınız.
- **Önerilen VM toplu iş boyutu**: Önerilen toplu iş boyutu, %100'lük bir RPO'yu karşılarken, varsayılan olarak 72 saat içinde toplu iş için ilk çoğaltmayı tamamlama yeteneğine dayanır. Saat değeri değiştirilebilir.

Bu önerileri Azure kaynakları, ağ bant genişliği ve VM toplu işlemesini planlamak için kullanabilirsiniz.

## <a name="plan-azure-subscriptions-and-quotas"></a>Azure aboneliklerini ve kotalarını planlama

Hedef abonelikteki kullanılabilir kotaların başarısızlığı işlemek için yeterli olduğundan emin olmak istiyoruz.

**Görev** | **Şey** | **Eylem**
--- | --- | ---
**Çekirdekleri kontrol edin** | Kullanılabilir kotadaki çekirdekler, başarısız olduğu anda toplam hedef sayısına eşit veya aşmazsa, başarısız olur. | VMware VM'ler için, hedef abonelikte Dağıtım Planlayıcısı temel önerisini karşılayacak yeterli çekirdek olup yok.<br/><br/> Fiziksel sunucular için Azure çekirdeklerinin el ile ilgili tahminlerinize uygun olup olmadığını kontrol edin.<br/><br/> Kotaları kontrol etmek için Azure portalında **> Abonelik**'i tıklatın , Kullanım **+ kotalar.**<br/><br/> Kotaları artırma hakkında [daha fazla bilgi edinin.](https://docs.microsoft.com/azure/azure-portal/supportability/resource-manager-core-quotas-request)
**Başarısız geçme sınırlarını denetleme** | Başarısız ların sayısı Site Kurtarma hata sınırlarını aşmamalıdır. |  Başarısız lar sınırları aşarsa, abonelik ekleyebilir ve birden çok abonelikte başarısız olabilir veya abonelik kotasını artırabilirsiniz. 


### <a name="failover-limits"></a>Failover sınırları

Sınırlar, makine başına üç disk varsayılarak, Site Kurtarma tarafından bir saat içinde desteklenen başarısız ların sayısını gösterir.

Uymak ne anlama gelir? Azure VM'yi başlatmak için Azure bazı sürücülerin önyükleme başlangıç durumunda olmasını ve DHCP gibi hizmetlerin otomatik olarak başlatılacak şekilde ayarlanmasını gerektirir.
- Uyumlu makineler de bu ayarları zaten yerinde olacaktır.
- Windows çalıştıran makineler için, uyumluluğu proaktif olarak denetleyebilir ve gerekirse uyumlu hale getirebilirsiniz. [Daha fazla bilgi edinin](site-recovery-failover-to-azure-troubleshoot.md#failover-failed-with-error-id-170010).
- Linux makineleri yalnızca başarısız olduğu anda uyumlu olarak getirilir.

**Makine Azure ile uyumlu mu?** | **Azure VM sınırları (yönetilen disk başarısız)**
--- | --- 
Evet | 2000
Hayır | 1000

- Sınırlar, abonelik için hedef bölgede en az diğer işlerin devam ettiğini varsayar.
- Bazı Azure bölgeleri daha küçüktür ve biraz daha düşük sınırlara sahip olabilir.

## <a name="plan-infrastructure-and-vm-connectivity"></a>Altyapı yı ve VM bağlantısını planlayın

Azure'a geçemedikten sonra iş yüklerinizin şirket içinde olduğu gibi çalışması ve kullanıcıların Azure VM'lerinde çalışan iş yüklerine erişmesini sağlamanız gerekir.

- Etkin Dizin veya DNS şirket içi altyapınız üzerinden Azure'da başarısız olmak hakkında [daha fazla bilgi edinin.](site-recovery-active-directory.md#test-failover-considerations)
- Başarısız olduktan sonra Azure VM'lere bağlanmaya hazırlanma hakkında [daha fazla bilgi edinin.](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover)



## <a name="plan-for-source-capacity-and-requirements"></a>Kaynak kapasitesi ve gereksinimleri için plan

Kapasite gereksinimlerini karşılamak için yeterli yapılandırma sunucunuz ve ölçeklendirme işlem sunucunuz olması önemlidir. Büyük ölçekli dağıtımınızı başlattığınızda, tek bir yapılandırma sunucusu ve tek bir ölçeklendirme işlem sunucusuyla başlayın. Öngörülen sınırlara ulaştığınızda, ek sunucular ekleyin.

>[!NOTE]
> VMware VM'ler için Dağıtım Planlayıcısı, gereksinim duyduğunuz yapılandırma ve işlem sunucuları hakkında bazı önerilerde bulunur. Dağıtım Planlayıcısı önerisini takip etmek yerine aşağıdaki yordamlarda yer alan tabloları kullanmanızı öneririz. 


## <a name="set-up-a-configuration-server"></a>Yapılandırma sunucusu ayarlama
 
Yapılandırma sunucusu kapasitesi, veri karmaşası hızından değil, çoğaltma makinesi sayısından etkilenir. Ek yapılandırma sunucularına ihtiyacınız olup olmadığını anlamak için bu tanımlanmış VM sınırlarını kullanın.

**CPU** | **Bellek** | **Önbellek diski** | **Çoğaltılan makine sınırı**
 --- | --- | --- | ---
8 vCPUs<br> 2 soket * 4 çekirdek @ 2.5 Ghz | 16 GB | 600 GB | 550'ye kadar makine<br> Her makinenin her biri 100 GB'lık üç diski olduğunu varsayar.

- Bu sınırlar, OVF şablonu kullanılarak ayarlanan bir yapılandırma sunucusuna dayanır.
- Sınırlar, yapılandırma sunucusunda varsayılan olarak çalışan işlem sunucusunu kullanmadığınızı varsayar.

Yeni bir yapılandırma sunucusu eklemeniz gerekiyorsa, aşağıdaki yönergeleri izleyin:

- OVF şablonu kullanarak VMware VM olağanüstü durum kurtarma için [bir yapılandırma sunucusu ayarlayın.](vmware-azure-deploy-configuration-server.md)
- Fiziksel sunucular veya OVF şablonu kullanabilen VMware dağıtımları için el ile [bir yapılandırma sunucusu ayarlayın.](physical-azure-set-up-source.md)

Bir yapılandırma sunucusu ayarlarken şunları unutmayın:

- Bir yapılandırma sunucusu ayarladığınızda, bu sunucular kurulumdan sonra değiştirilmemesi gerektiğinden, içinde bulunduğu abonelik ve kasayı göz önünde bulundurmanız önemlidir. Kasayı değiştirmeniz gerekiyorsa, yapılandırma sunucusunu kasadan ayırmanız ve yeniden kaydetmeniz gerekir. Bu, kasadaki VM'lerin çoğaltılmasıyla durur.
- Birden çok ağ bağdaştırıcısı içeren bir yapılandırma sunucusu kurmak istiyorsanız, bunu ayarlama sırasında yapmanız gerekir. Bunu, yapılandırma sunucusunu kasaya kaydettirdikten sonra yapamazsınız.

## <a name="set-up-a-process-server"></a>İşlem sunucusu ayarlama

İşlem sunucusu kapasitesi, çoğaltma için etkinleştirilen makine sayısından değil, veri karmaşası hızlarından etkilenir.

- Büyük dağıtımlar için her zaman en az bir ölçeklendirme işlem sunucusu olmalıdır.
- Ek sunuculara ihtiyacınız olup olmadığını anlamak için aşağıdaki tabloyu kullanın.
- En yüksek özelliklere sahip bir sunucu eklemenizi öneririz. 


**CPU** | **Bellek** | **Önbellek diski** | **Çalkalama oranı**
 --- | --- | --- | --- 
12 vCPUs<br> 2 soket*6 çekirdek @ 2.5 Ghz | 24 GB | 1 GB | Günde en fazla 2 TB

İşlem sunucusunu aşağıdaki gibi ayarlayın:

1. Ön [koşulları](vmware-azure-set-up-process-server-scale.md#prerequisites)gözden geçirin.
2. Sunucuyu [portala](vmware-azure-set-up-process-server-scale.md#install-from-the-ui)veya komut [satırına](vmware-azure-set-up-process-server-scale.md#install-from-the-command-line)yükleyin.
3. Çoğaltılan makineleri yeni sunucuyu kullanmak üzere yapılandırın. Zaten çoğalan makineleriniz varsa:
    - Tüm işlem sunucusu iş yükünü yeni işlem sunucusuna [taşıyabilirsiniz.](vmware-azure-manage-process-server.md#switch-an-entire-workload-to-another-process-server)
    - Alternatif olarak, belirli VM'leri yeni işlem sunucusuna [taşıyabilirsiniz.](vmware-azure-manage-process-server.md#move-vms-to-balance-the-process-server-load)



## <a name="enable-large-scale-replication"></a>Büyük ölçekli çoğaltmayı etkinleştirme

Kapasiteyi planladıktan ve gerekli bileşenleri ve altyapıyı dağıttıktan sonra, çok sayıda VM için çoğaltmayı etkinleştirin.

1. Makineleri gruplar halinde sıralayın. Toplu iş içindeki VM'ler için çoğaltmayı etkinleştirin ve ardından bir sonraki toplu iş partisine geçin.

    - VMware VM'ler için Dağıtım Planlayıcısı raporunda [önerilen VM toplu iş boyutunu](site-recovery-vmware-deployment-planner-analyze-report.md#recommended-vm-batch-size-for-initial-replication) kullanabilirsiniz.
    - Fiziksel makineler için, benzer boyut ve veri miktarına sahip makinelere ve kullanılabilir ağ verisi üzerinde toplu iş tanımlamanızı öneririz. Amaç, ilk çoğaltmalarını aynı süre içinde tamamlamaolasılığı yüksek olan toplu makinelerde yapmaktır.
    
2. Bir makine için disk karmaşası yüksekse veya Dağıtım thePlanner'da sınırları aşıyorsa, çoğaltmanız gerekmeyen kritik olmayan dosyaları (günlük dökümleri veya geçici dosyalar gibi) makineden taşıyabilirsiniz. VMware VM'ler için bu dosyaları ayrı bir diske taşıyabilir ve ardından [bu diski](vmware-azure-exclude-disk.md) çoğaltmadan dışlayabilirsiniz.
3. Çoğaltmayı etkinleştirmeden önce, makinelerin [çoğaltma gereksinimlerini](vmware-physical-azure-support-matrix.md#replicated-machines)karşıladığını kontrol edin.
4. [VMware VM'ler](vmware-azure-set-up-replication.md#create-a-policy) veya fiziksel [sunucular](physical-azure-disaster-recovery.md#create-a-replication-policy)için bir çoğaltma ilkesi ni yapılandırın.
5. [VMware VM'ler](vmware-azure-enable-replication.md) veya [fiziksel sunucular](physical-azure-disaster-recovery.md#enable-replication)için çoğaltmayı etkinleştirin. Bu, seçili makinelerin ilk çoğaltmasını başlatıyor.

## <a name="monitor-your-deployment"></a>Dağıtımınızı izleme

İlk VM grubu için çoğaltma işlemini başlattıktan sonra, dağıtımınızı aşağıdaki gibi izlemeye başlayın:  

1. Çoğaltılan makinelerin sistem durumu durumunu izlemek için bir olağanüstü durum kurtarma yöneticisi atayın.
2. Çoğaltılan öğeler ve altyapı için [olayları izleyin.](site-recovery-monitor-and-troubleshoot.md)
3. Ölçeklendirme işlem sunucularınızın [durumunu izleyin.](vmware-physical-azure-monitor-process-server.md)
4. Etkinlikler için [e-posta bildirimleri](https://docs.microsoft.com/azure/site-recovery/site-recovery-monitor-and-troubleshoot#subscribe-to-email-notifications) almak, daha kolay izleme için kaydolun.
5. Her şeyin beklendiği gibi çalıştığından emin olmak için düzenli [afet kurtarma tatbikatları](site-recovery-test-failover-to-azure.md)gerçekleştirin.


## <a name="plan-for-large-scale-failovers"></a>Büyük ölçekli arızalar için plan

Bir felaket durumunda, Azure'a çok sayıda makine/iş yükü üzerinde başarısız olmanız gerekebilir. Aşağıdaki gibi olay bu tür için hazırlanın.

Aşağıdaki gibi başarısız olmak için önceden hazırlayabilirsiniz:

- [Altyapınızı ve VM'lerinizi,](#plan-infrastructure-and-vm-connectivity) iş yüklerinizin başarısız olduktan sonra kullanılabilir hale gelecek şekilde ve kullanıcıların Azure VM'lerine erişebilmeleri için hazırlayın.
- Bu belgede daha önceki [başarısızlık sınırlarını](#failover-limits) not edin. Başarısızlarınızın bu sınırlara denk geleceğinden emin olun.
- Düzenli [afet kurtarma matkapları](site-recovery-test-failover-to-azure.md)çalıştırın. Matkaplar yardımcı olur:
    - Başarısız olmadan önce dağıtımınızdaki boşlukları bulun.
    - Uygulamalarınız için uçuça RTO'u tahmin edin.
    - İş yüklerin için uçlardan uca RPO'nuzu tahmin edin.
    - IP adresi aralığı çakışmalarını tanımlayın.
    - Matkapları çalıştırırken, üretim ağlarını matkaplar için kullanmamanızı, üretim ve test ağlarında aynı alt ağ adlarını kullanmaktan kaçınmanızı ve her alıştırmadan sonra test arızalarını temizlemenizi öneririz.

Büyük ölçekli bir başarısızlık çalıştırmak için aşağıdakileri öneririz:

1. İş yükü başarısızlığına yönelik kurtarma planları oluşturun.
    - Her kurtarma planı en fazla 100 makinenin arızasını tetikleyebilir.
    - Kurtarma planları hakkında [daha fazla bilgi edinin.](recovery-plan-overview.md)
2. Azure'daki tüm el ile görevleri otomatikleştirmek için kurtarma planlarına Azure Otomasyon runbook komut dosyaları ekleyin. Tipik görevler arasında yük dengeleyicilerinin yapılandırılması, DNS'nin güncellenmesi vb. yer alır. [Daha fazlasını öğrenin](site-recovery-runbook-automation.md)
2. Başarısız olmadan önce, Windows makinelerini Azure ortamına uyacak şekilde hazırlayın. [Arıza sınırları,](#plan-azure-subscriptions-and-quotas) uyumlu makineler için daha yüksektir. Runbook'lar hakkında [daha fazla bilgi edinin.](site-recovery-failover-to-azure-troubleshoot.md#failover-failed-with-error-id-170010)
4.  [Start-AzRecoveryServicesAsrPlannedFailoverJob](https://docs.microsoft.com/powershell/module/az.recoveryservices/start-azrecoveryservicesasrplannedfailoverjob?view=azps-2.0.0&viewFallbackFrom=azps-1.1.0) PowerShell cmdlet ile birlikte bir kurtarma planı ile başarısız tetikleme.



## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Site Recovery’yi izleme](site-recovery-monitor-and-troubleshoot.md)
