---
title: Azure Site Kurtarma ile ikincil bir siteye VMware VM'lerin/fiziksel sunucuların olağanüstü kurtarma
description: Azure Site Kurtarma ile ikincil bir siteye VMware VM'lerin veya Windows ve Linux fiziksel sunucularının olağanüstü durum kurtarmasını nasıl ayarlayacağınızı öğrenin.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
services: site-recovery
ms.topic: conceptual
ms.date: 11/05/2019
ms.author: raynew
ms.openlocfilehash: 71d230c9fea25edfbf0ca4ea40f15b69779ad060
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79256815"
---
# <a name="set-up-disaster-recovery-of-on-premises-vmware-virtual-machines-or-physical-servers-to-a-secondary-site"></a>Şirket içi VMware sanal makinelerinde veya fiziksel sunucularda ikincil siteye olağanüstü durum kurtarmayı ayarlama

[Azure Site Kurtarma'daki](site-recovery-overview.md) InMage Scout, şirket içi VMware siteleri arasında gerçek zamanlı çoğaltma sağlar. InMage Scout, Azure Site Kurtarma hizmet aboneliklerine dahildir.

## <a name="end-of-support-announcement"></a>Destek sonu duyurusu

Şirket içi VMware veya fiziksel veri merkezleri arasında çoğaltma için Azure Site Kurtarma senaryosu destek sonu geliyor.

-   Ağustos 2018'den itibaren, senaryo Kurtarma Hizmetleri kasasında yapılandırılamaz ve InMage Scout yazılımı kasadan indirici olamaz. Varolan dağıtımlar desteklenecektir. 
-   31 Aralık 2020 tarihinden itibaren senaryo desteklenmeyen.
- Varolan iş ortakları, destek sona erene kadar senaryoya yeni müşteriler verebilir.

2018 ve 2019 yıllarında iki güncelleme yayınlanacaktır: 

-   Güncelleme 7: Ağ yapılandırması ve uyumluluk sorunlarını giderir ve TLS 1.2 desteği sağlar.
-   Güncelleme 8: Linux işletim sistemleri RHEL/CentOS 7.3/7.4/7.5 desteği ekler ve SUSE 12 için

Güncelleştirme 8'den sonra başka güncelleştirme yayınlanmayacaktır. Güncelleştirme 8'e eklenen işletim sistemleri ve en iyi çabaya dayalı hata düzeltmeleri için sınırlı düzeltme desteği olacaktır.

Azure Site Kurtarma, VMware ve Hyper-V müşterilerine olağanüstü durum kurtarma sitesi olarak Azure ile sorunsuz ve sınıfının en iyisi Bir DRaaS çözümü sunarak yenilik yapmaya devam ediyor. Microsoft, mevcut InMage / ASR Scout müşterilerinin iş süreklilik gereksinimleri için Azure Site Kurtarma'nın VMware'ini Azure senaryosuna kullanmayı düşünmelerini önerir. Azure Site Kurtarma'nın Azure'a VMware'i senaryosu, RPO ve RTO dakikaları, çoklu VM uygulama çoğaltma ve kurtarma desteği, sorunsuz biniş, kapsamlı izleme ve VMware uygulamaları için kurumsal sınıf bir DR çözümüdür önemli TCO avantajı.

### <a name="scenario-migration"></a>Senaryo geçişi
Alternatif olarak, şirket içi VMware VM'ler ve fiziksel makineler için olağanüstü durum kurtarma yı ayarlamanızı öneririz. Bunu şu şekilde yapabilirsiniz:

1.  Aşağıdaki hızlı karşılaştırmayı gözden geçirin. Şirket içi makineleri çoğaltmadan önce, Azure'da çoğaltma [gereksinimlerini](./vmware-physical-azure-support-matrix.md#replicated-machines) karşılayıp karşılamadıklarını kontrol etseniz gerek. VMware VM'leri çoğaltırsanız, [kapasite planlama yönergelerini](./site-recovery-plan-capacity-vmware.md)gözden geçirmenizi ve [Dağıtım Planlayıcısı aracını](./site-recovery-deployment-planner.md) kimlik kapasitesi gereksinimlerine göre çalıştırmanızı ve uyumluluğu doğrulamanızı öneririz.
2.  Dağıtım Planlayıcısını çalıştırdıktan sonra çoğaltma yı ayarlayabilirsiniz: o VMware VM'ler için, [Azure'u hazırlamak](./tutorial-prepare-azure.md)için bu öğreticileri takip edin, [şirket içi VMware ortamınızı hazırlayın](./vmware-azure-tutorial-prepare-on-premises.md)ve olağanüstü durum [kurtarmayı ayarlayabilirsiniz.](./vmware-azure-tutorial-prepare-on-premises.md)
o Fiziksel makineler için, bu [öğretici](./physical-azure-disaster-recovery.md)izleyin.
3.  Makineler Azure'a çoğaldıktan sonra, her şeyin beklendiği gibi çalıştığından emin olmak için bir [olağanüstü durum kurtarma matkabı](./site-recovery-test-failover-to-azure.md) çalıştırabilirsiniz.

### <a name="quick-comparison"></a>Hızlı karşılaştırma

**Özellik** | **Azure’a çoğaltma** |**VMware veri merkezleri arasında çoğaltma**
--|--|--
**Gerekli bileşenler** |Çoğaltılan makinelerde mobilite hizmeti. Şirket içi yapılandırma sunucusu, işlem sunucusu, ana hedef sunucu. Başarısız geri dönüş için Azure'da geçici işlem sunucusu.|Mobilite hizmeti, İşlem Sunucusu, Configuration Server ve Ana Hedef
**Yapılandırma ve orkestrasyon** |Azure portalında Kurtarma Hizmetleri kasası | vContinuum kullanma 
**Çoğaltılmış** |Disk (Windows ve Linux) |Ses-Windows<br> Disk-Linux
**Paylaşılan disk kümesi** |Desteklenmiyor|Destekleniyor
**Veri karmaşası sınırları (ortalama)** |Disk başına 10 MB/sn veri<br> VM başına 25MB/sveri<br> [Daha fazlasını öğrenin](./site-recovery-vmware-deployment-planner-analyze-report.md#azure-site-recovery-limits) | > 10 MB/sn veri  <br> vm başına 25 MB/sn veri >
**İzleme** |Azure portalından|Kaynak: CX (Configuration Server)
**Destek Matrisi** | [Detaylar için tıklayınız](./vmware-physical-azure-support-matrix.md)|[ASR Scout uyumlu matrisindir](https://aka.ms/asr-scout-cm)


## <a name="prerequisites"></a>Ön koşullar
Bu öğreticiyi tamamlamak için:

- Tüm bileşenler için destek gereksinimlerini [gözden geçirin.](vmware-physical-secondary-support-matrix.md)
- Çoğaltmak istediğiniz makinelerin [çoğaltılan makine desteğine](vmware-physical-secondary-support-matrix.md#replicated-vm-support)uyduğundan emin olun.


## <a name="download-and-install-component-updates"></a>Bileşen güncelleştirmelerini karşıdan yükleme ve yükleme

 En son [güncelleştirmeleri](#updates)gözden geçirin ve yükleyin. Güncelleştirmeler sunucularda aşağıdaki sırayla yüklenmelidir:

1. RX sunucusu (varsa)
2. Yapılandırma sunucuları
3. İşlem sunucuları
4. Ana Hedef sunucuları
5. vContinuum sunucuları
6. Kaynak sunucu (hem Windows hem de Linux Sunucuları)

Güncelleştirmeleri aşağıdaki gibi yükleyin:

> [!NOTE]
>Tüm Scout bileşenlerinin dosya güncelleştirme sürümü güncelleştirme .zip dosyasında aynı olmayabilir. Eski sürüm, bu güncelleştirmenin önceki güncelleştirmesinden beri bileşende bir değişiklik olmadığını gösterir.

[Update](https://aka.ms/asr-scout-update7) .zip dosyasını ve [MySQL ve PHP yükseltme](https://aka.ms/asr-scout-u7-mysql-php-manualupgrade) yapılandırma dosyalarını indirin. Update .zip dosyası, aşağıdaki bileşenlerin tüm temel ikililerini ve kümülatif yükseltme ikililerini içerir: 
- InMage_ScoutCloud_RX_8.0.1.0_RHEL6-64_GA_02Mar2015.tar.gz
- RX_8.0.7.0_GA_Update_7_2965621_28Dec18.tar.gz
- InMage_CX_8.0.1.0_Windows_GA_26Feb2015_release.exe
- InMage_CX_TP_8.0.1.0_Windows_GA_26Feb2015_release.exe
- CX_Windows_8.0.7.0_GA_Update_7_2965621_28Dec18.exe
- InMage_PI_8.0.1.0_Windows_GA_26Feb2015_release.exe
- InMage_Scout_vContinuum_MT_8.0.7.0_Windows_GA_27Dec2018_release.exe
- InMage_UA_8.0.7.0_Windows_GA_27Dec2018_release.exe
- InMage_UA_8.0.7.0_OL5-32_GA_03Dec2018_release.tar.gz
- InMage_UA_8.0.7.0_OL5-64_GA_03Dec2018_release.tar.gz
- InMage_UA_8.0.7.0_OL6-32_GA_03Dec2018_release.tar.gz
- InMage_UA_8.0.7.0_OL6-64_GA_03Dec2018_release.tar.gz
- InMage_UA_8.0.7.0_RHEL5-32_GA_03Dec2018_release.tar.gz
- InMage_UA_8.0.7.0_RHEL5-64_GA_03Dec2018_release.tar.gz
- InMage_UA_8.0.7.0_RHEL6-32_GA_03Dec2018_release.tar.gz
- InMage_UA_8.0.7.0_RHEL6-64_GA_03Dec2018_release.tar.gz
- InMage_UA_8.0.7.0_RHEL7-64_GA_03Dec2018_release.tar.gz
- InMage_UA_8.0.7.0_SLES10-32_GA_03Dec2018_release.tar.gz
- InMage_UA_8.0.7.0_SLES10-64_GA_03Dec2018_release.tar.gz
- InMage_UA_8.0.7.0_SLES10-SP1-32_GA_03Dec2018_release.tar.gz
- InMage_UA_8.0.7.0_SLES10-SP1-64_GA_03Dec2018_release.tar.gz
- InMage_UA_8.0.7.0_SLES10-SP2-32_GA_03Dec2018_release.tar.gz
- InMage_UA_8.0.7.0_SLES10-SP2-64_GA_03Dec2018_release.tar.gz
- InMage_UA_8.0.7.0_SLES10-SP3-32_GA_03Dec2018_release.tar.gz
- InMage_UA_8.0.7.0_SLES10-SP3-64_GA_03Dec2018_release.tar.gz
- InMage_UA_8.0.7.0_SLES10-SP4-32_GA_03Dec2018_release.tar.gz
- InMage_UA_8.0.7.0_SLES10-SP4-64_GA_03Dec2018_release.tar.gz
- InMage_UA_8.0.7.0_SLES11-32_GA_03Dec2018_release.tar.gz
- InMage_UA_8.0.7.0_SLES11-64_GA_04Dec2018_release.tar.gz
- InMage_UA_8.0.7.0_SLES11-SP1-32_GA_03Dec2018_release.tar.gz
- InMage_UA_8.0.7.0_SLES11-SP1-64_GA_04Dec2018_release.tar.gz
- InMage_UA_8.0.7.0_SLES11-SP2-32_GA_03Dec2018_release.tar.gz
- InMage_UA_8.0.7.0_SLES11-SP2-64_GA_03Dec2018_release.tar.gz
- InMage_UA_8.0.7.0_SLES11-SP3-32_GA_03Dec2018_release.tar.gz
- InMage_UA_8.0.7.0_SLES11-SP3-64_GA_03Dec2018_release.tar.gz
- InMage_UA_8.0.7.0_SLES11-SP4-64_GA_03Dec2018_release.tar.gz
  1. .zip dosyalarını ayıklayın.
  2. **RX sunucusu**: rx sunucusuna **RX_8.0.7.0_GA_Update_7_2965621_28Dec18.tar.gz** kopyalayın ve ayıklayın. Çıkarılan klasörde çalıştır **/Yükle.**
  3. **Configuration server and process server**: **CX_Windows_8.0.7.0_GA_Update_7_2965621_28Dec18.exe'yi** yapılandırma sunucusuna ve işlem sunucusuna kopyalayın. Çalıştırmak için çift tıklatın.<br>
  4. **Windows Master Target sunucusu**: Birleşik aracıyı güncelleştirmek için **InMage_UA_8.0.7.0_Windows_GA_27Dec2018_release.exe'yi** sunucuya kopyalayın. Çalıştırmak için çift tıklatın. Aynı dosya yeni yükleme için de kullanılabilir. Aynı birleşik aracı güncelleştirmesi kaynak sunucu için de geçerlidir.
  Bu tüm son değişiklikler ile yeni GA yükleyici olduğu gibi güncelleştirme **InMage_Scout_vContinuum_MT_8.0.7.0_Windows_GA_27Dec2018_release.exe** ile hazırlanan Master hedef uygulamak gerekmez.
  5. **vContinuum server**: **InMage_Scout_vContinuum_MT_8.0.7.0_Windows_GA_27Dec2018_release.exe'yi** sunucuya kopyalayın.  vContinuum sihirbazını kapattığınızdan emin olun. Çalıştırmak için dosyaya çift tıklayın.
  6. **Linux ana hedef sunucusu**: Birleşik aracıyı güncellemek **için, InMage_UA_8.0.7.0_RHEL6-64_GA_03Dec2018_release.tar.gz'yi** Linux Master Target sunucusuna kopyalayın ve ayıklayın. Çıkarılan klasörde çalıştır **/Yükle.**
  7. **Windows kaynak sunucusu**: Birleşik aracıyı güncelleştirmek için kaynak sunucuya **InMage_UA_8.0.7.0_Windows_GA_27Dec2018_release.exe** kopyalayın. Çalıştırmak için dosyaya çift tıklayın. 
  8. **Linux kaynak sunucusu**: Birleşik aracıyı güncellemek için, birleşik aracı dosyasının ilgili sürümünü Linux sunucusuna kopyalayın ve ayıklayın. Çıkarılan klasörde çalıştır **/Yükle.**  Örnek: RHEL 6.7 64 bit sunucusu için **InMage_UA_8.0.7.0_RHEL6-64_GA_03Dec2018_release.tar.gz'yi** sunucuya kopyalayın ve ayıklayın. Çıkarılan klasörde çalıştır **/Yükle.**
  9. Configuration Server, Process Server ve RX sunucusunu yukarıda belirtilen yükleyiciler ile yükselttikten sonra, PHP ve MySQL kitaplıklarının [hızlı kurulum kılavuzunun](https://aka.ms/asr-scout-quick-install-guide)7.4 bölümünde belirtilen adımlarla el ile yükseltilmesi gerekmektedir.

## <a name="enable-replication"></a>Çoğaltmayı etkinleştirme

1. Kaynak ve hedef VMware siteleri arasında çoğaltma ayarlayın.
2. Yükleme, koruma ve kurtarma hakkında daha fazla bilgi edinmek için aşağıdaki belgelere bakın:

   * [Sürüm notları](https://aka.ms/asr-scout-release-notes)
   * [Uyumluluk matrisi](https://aka.ms/asr-scout-cm)
   * [Kullanım kılavuzu](https://aka.ms/asr-scout-user-guide)
   * [RX kullanım kılavuzu](https://aka.ms/asr-scout-rx-user-guide)
   * [Hızlı kurulum kılavuzu](https://aka.ms/asr-scout-quick-install-guide)
   * [MYSQL ve PHP kitaplıklarını yükseltme](https://aka.ms/asr-scout-u7-mysql-php-manualupgrade)

## <a name="updates"></a>Güncelleştirmeler

### <a name="site-recovery-scout-801-update-7"></a>Site Kurtarma Scout 8.0.1 Güncelleme 7 
Güncelleme: 31 Aralık 2018 [Download Scout güncellemesi 7](https://aka.ms/asr-scout-update7).
Scout Update 7, yeni kurulum için kullanılabilen ve önceki güncelleştirmeleri (Güncelleme 1'den 6'ya) mevcut aracıları/MT'yi yükseltmek için kullanılabilen tam bir yükleyicidir. Güncelleştirme 1'den Güncelleme 6'ya kadar tüm düzeltmeleri ve aşağıda açıklanan yeni düzeltmeleri ve geliştirmeleri içerir.
 
#### <a name="new-features"></a>Yeni özellikler
* PCI uyumluluğu
* TLS v1.2 Desteği

#### <a name="bug-and-security-fixes"></a>Hata ve Güvenlik Düzeltmeleri
* Sabit: Windows Cluster/Bağımsız Makineler kurtarma/DR-Drill üzerine yanlış IP yapılandırmalarına sahiptir.
* Sabit: Bazen Ekle disk işlemi V2V küme için başarısız olur.
* Sabit: Ana Hedef Windows Server 2016 ise vContinuum Sihirbazı kurtarma aşamasında takılıp kalır
* Düzeltildi: MySQL güvenlik sorunları, MySQL'i sürüm 5.7.23'e yükselterek azaltılır

#### <a name="manual-upgrade-for-php-and-mysql-on-csps-and-rx"></a>CS, PS ve RX'te PHP ve MySQL için Manuel Yükseltme
PHP komut dosyası platformu Configuration Server, Process Server ve RX Server sürüm 7.2.10'a yükseltilmelidir.
MySQL veritabanı yönetim sistemi Configuration Server, Process Server ve RX Server sürüm 5.7.23'e yükseltilmelidir.
PHP ve MySQL sürümlerini yükseltmek için [lütfen Hızlı yükleme kılavuzunda](https://aka.ms/asr-scout-quick-install-guide) verilen manuel adımları izleyin.

### <a name="site-recovery-scout-801-update-6"></a>Site Kurtarma Scout 8.0.1 Güncelleme 6 
Güncelleme: 12 Ekim 2017

[Scout güncelleme 6 indirin](https://aka.ms/asr-scout-update6).

Scout Update 6 kümülatif bir güncelleştirmedir. Güncelleştirme 1'den Güncelleme 5'e kadar olan tüm düzeltmeleri ve aşağıda açıklanan yeni düzeltmeleri ve geliştirmeleri içerir. 

#### <a name="new-platform-support"></a>Yeni platform desteği
* Source Windows Server 2016 için destek eklendi
* Aşağıdaki Linux işletim sistemleri için destek eklendi:
    - Kırmızı Şapka Kurumsal Linux (RHEL) 6,9
    - CentOS 6.9
    - Oracle Linux 5.11
    - Oracle Linux 6.8
* VMware Center 6.5 için destek eklendi

Güncelleştirmeleri aşağıdaki gibi yükleyin:

> [!NOTE]
>Tüm Scout bileşenlerinin dosya güncelleştirme sürümü güncelleştirme .zip dosyasında aynı olmayabilir. Eski sürüm, bu güncelleştirmenin önceki güncelleştirmesinden beri bileşende bir değişiklik olmadığını gösterir.

Update .zip dosyasını [indirin.](https://aka.ms/asr-scout-update6) Dosya aşağıdaki bileşenleri içerir: 
- RX_8.0.4.0_GA_Update_4_8725872_16Sep16.tar.gz
- CX_Windows_8.0.6.0_GA_Update_6_13746667_18Sep17.exe
- UA_Windows_8.0.5.0_GA_Update_5_11525802_20Apr17.exe
- UA_RHEL6-64_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz
- vCon_Windows_8.0.6.0_GA_Update_6_11525767_21Sep17.exe
- RHEL5, OL5, OL6, SUSE 10, SUSE 11 için\<UA güncelleme4 bit: UA_ Linux OS>_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz
  1. .zip dosyalarını ayıklayın.
  2. **RX sunucusu**: rx sunucusuna **RX_8.0.4.0_GA_Update_4_8725872_16Sep16.tar.gz** kopyalayın ve ayıklayın. Çıkarılan klasörde çalıştır **/Yükle.**
  3. **Configuration server and process server**: **CX_Windows_8.0.6.0_GA_Update_6_13746667_18Sep17.exe'yi** yapılandırma sunucusuna ve işlem sunucusuna kopyalayın. Çalıştırmak için çift tıklatın.<br>
  4. **Windows Master Target server**: Birleşik aracıyı güncelleştirmek için **UA_Windows_8.0.5.0_GA_Update_5_11525802_20Apr17.exe'yi** sunucuya kopyalayın. Çalıştırmak için çift tıklatın. Aynı birleşik aracı güncelleştirmesi kaynak sunucu için de geçerlidir. Kaynak Güncelleştirme 4'e güncelleştirilmemişse, birleştirilmiş aracıyı güncelleştirmeniz gerekir.
  Bu tüm son değişiklikler ile yeni GA yükleyici olduğu gibi güncelleştirme **InMage_Scout_vContinuum_MT_8.0.1.0_Windows_GA_10Oct2017_release.exe** ile hazırlanan Master hedef uygulamak gerekmez.
  5. **vContinuum server**: **sunucuya vCon_Windows_8.0.6.0_GA_Update_6_11525767_21Sep17.exe** kopyalayın.  vContinuum sihirbazını kapattığınızdan emin olun. Çalıştırmak için dosyaya çift tıklayın.
  Bu tüm son değişiklikler ile yeni GA yükleyici olduğu gibi güncelleştirme **InMage_Scout_vContinuum_MT_8.0.1.0_Windows_GA_10Oct2017_release.exe** ile hazırlanan Ana Hedef uygulamak gerekmez.
  6. **Linux ana hedef sunucusu**: Birleşik aracıyı güncellemek **için, UA_RHEL6-64_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz'yi** ana hedef sunucuya kopyalayın ve ayıklayın. Çıkarılan klasörde çalıştır **/Yükle.**
  7. **Windows kaynak sunucusu**: Birleşik aracıyı güncelleştirmek için kaynak sunucuya **UA_Windows_8.0.5.0_GA_Update_5_11525802_20Apr17.exe** kopyalayın. Çalıştırmak için dosyaya çift tıklayın. 
  Güncelleştirme 4'e zaten güncelleştirilmişse veya kaynak aracı en son base installer **InMage_UA_8.0.1.0_Windows_GA_28Sep2017_release.exe**yüklüyse, Kaynak sunucuya Update 5 aracısını yüklemeniz gerekmez.
  8. **Linux kaynak sunucusu**: Birleşik aracıyı güncellemek için, birleşik aracı dosyasının ilgili sürümünü Linux sunucusuna kopyalayın ve ayıklayın. Çıkarılan klasörde çalıştır **/Yükle.**  Örnek: RHEL 6.7 64 bit sunucusu için **UA_RHEL6-64_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz'yi** sunucuya kopyalayın ve ayıklayın. Çıkarılan klasörde çalıştır **/Yükle.**


> [!NOTE]
> * Windows Için Temel Birleşik Aracı(UA) yükleyici, Windows Server 2016'yı desteklemek için yenilendi. Yeni yükleyici **InMage_UA_8.0.1.0_Windows_GA_28Sep2017_release.exe** temel Scout GA paketi **(InMage_Scout_Standard_8.0.1 GA-Oct17.zip) ile paketlenmiştir.** Aynı yükleyici, desteklenen tüm Windows sürümü için kullanılır. 
> * Temel Windows vContinuum & Master Target yükleyici, Windows Server 2016'yı desteklemek için yenilendi. Yeni yükleyici **InMage_Scout_vContinuum_MT_8.0.1.0_Windows_GA_10Oct2017_release.exe** temel Scout GA paketi ile paketlenir **(InMage_Scout_Standard_8.0.1 GA-Oct17.zip).** Aynı yükleyici, Windows 2016 Master Target ve Windows 2012R2 Master Target'ı dağıtmak için kullanılacaktır.
> * Fiziksel sunucudaki Windows server 2016 ASR Scout tarafından desteklenmez. Yalnızca Windows Server 2016 VMware VM'yi destekler. 
>

#### <a name="bug-fixes-and-enhancements"></a>Hata düzeltmeleri ve geliştirmeleri
- Failback koruması Linux VM için başarısız olur ve çoğaltılacak disklerin listesi config'in sonunda boştur.

### <a name="site-recovery-scout-801-update-5"></a>Site Kurtarma Scout 8.0.1 Güncelleme 5
Scout Update 5 kümülatif bir güncelleştirmedir. Güncelleştirme 1'den 4'e kadar tüm düzeltmeleri ve aşağıda açıklanan yeni düzeltmeleri içerir.
- Site Kurtarma Scout Update 4'ten Güncelleme 5'e düzeltmeler özellikle ana hedef ve vContinuum bileşenleri içindir.
- Kaynak sunucular, ana hedef, yapılandırma, işlem ve RX sunucuları güncelleştirme 4'i zaten çalıştırıyorsa, yalnızca ana hedef sunucuya uygulayın. 

#### <a name="new-platform-support"></a>Yeni platform desteği
* SUSE Linux Enterprise Server 11 Servis Paketi 4(SP4)
* SLES 11 SP4 64 bit **InMage_UA_8.0.1.0_SLES11-SP4-64_GA_13Apr2017_release.tar.gz** temel Scout GA paketi ile paketlenir **(InMage_Scout_Standard_8.0.1 GA.zip).** Bir tonoz oluşturmak açıklandığı gibi, portaldan GA paketini indirin.


#### <a name="bug-fixes-and-enhancements"></a>Hata düzeltmeleri ve geliştirmeleri

* Artırılmış Windows küme desteği güvenilirliği için düzeltmeler:
    * Sabit- Bazı P2V MSCS küme diskleri kurtarma sonra RAW olur.
    * Sabit- P2V MSCS küme kurtarma bir disk sipariş uyuşmazlığı nedeniyle başarısız olur.
    * Sabit- Disk eklemek için MSCS kümeleme işlemi bir disk boyutu uyuşmazlığı hatası ile başarısız olur.
    * Sabit- RDM LUNs eşlemeli kaynak MSCS kümesi için hazırlık denetimi boyut doğrulamasında başarısız olur.
    * Sabit- Tek düğüm küme koruması, SCSI uyumsuzluğu sorunu nedeniyle başarısız olur. 
    * Sabit- Hedef küme diskleri varsa P2V Windows küme sunucusunun yeniden korunması başarısız olur. 
    
* Sabit: Failback koruması sırasında, seçili ana hedef sunucu korumalı kaynak makineyle aynı ESXi sunucusunda değilse (ileri koruma sırasında), vContinuum başarısız kurtarma sırasında yanlış ana hedef sunucuyu alır ve kurtarma işlem başarısız olur.

> [!NOTE]
> * P2V küme düzeltmeleri yalnızca Site Kurtarma İzci Güncelleştirmesi 5 ile yeni korunan fiziksel MSCS kümeleri için geçerlidir. Küme düzeltmelerini eski güncelleştirmelerle korunan P2V MSCS kümelerine yüklemek için, Site Kurtarma [İzci Yayın Notları'nın](https://aka.ms/asr-scout-release-notes)bölüm 12'sinde belirtilen yükseltme adımlarını izleyin.
> * yeniden koruma sırasında, küme düğümlerinin her birinde başlangıçta korunduğunda olduğu gibi aynı disk kümesi etkinse, fiziksel bir MSCS kümesinin yeniden korunması yalnızca varolan hedef diskleri yeniden kullanabilir. Değilse, yeniden koruma sırasında yeniden kullanmak için hedef yan diskleri doğru veri deposu yoluna taşımak için [Site Kurtarma İzci Yayın Notları](https://aka.ms/asr-scout-release-notes)bölüm 12'deki manuel adımları kullanın. Yükseltme adımlarını takip etmeden P2V modunda MSCS kümesini yeniden korursanız, hedef ESXi sunucusunda yeni bir disk oluşturur. Eski diskleri veri deposundan el ile silmeniz gerekir.
> * Bir kaynak SLES11 veya SLES11 (herhangi bir hizmet paketiyle) sunucu düzgün bir şekilde yeniden başlatıldığında, yeniden eşitleme için **kök** disk çoğaltma çiftlerini el ile işaretleyin. CX arabiriminde bildirim yok.Yeniden eşitleme için kök diski işaretlemezseniz, veri bütünlüğü sorunları fark edebilirsiniz.


### <a name="azure-site-recovery-scout-801-update-4"></a>Azure Site Kurtarma İzleyicisi 8.0.1 Güncelleme 4
Scout Update 4 kümülatif bir güncelleştirmedir. Güncelleştirme 1'den Güncelleştirme 3'e kadar tüm düzeltmeleri ve aşağıda açıklanan yeni düzeltmeleri içerir.

#### <a name="new-platform-support"></a>Yeni platform desteği

* vCenter/vSphere 6.0, 6.1 ve 6.2 için destek eklendi
* Bu Linux işletim sistemleri için destek eklendi:
  * Red Hat Enterprise Linux (RHEL) 7.0, 7.1 ve 7.2
  * CentOS 7.0, 7.1 ve 7.2
  * Kırmızı Şapka Kurumsal Linux (RHEL) 6,8
  * CentOS 6.8

> [!NOTE]
> RHEL/CentOS 7 64 bit **InMage_UA_8.0.1.0_RHEL7-64_GA_06Oct2016_release.tar.gz** temel Scout GA paketi ile paketlenir **InMage_Scout_Standard_8.0.1 GA.zip**. Scout GA paketini bir kasa oluşturmakta açıklandığı gibi portaldan indirin.

#### <a name="bug-fixes-and-enhancements"></a>Hata düzeltmeleri ve geliştirmeleri

* İstenmeyen yeniden senkronizasyon sorunlarını önlemek için aşağıdaki Linux işletim sistemleri ve klonlar için geliştirilmiş kapatma işleme:
    * Kırmızı Şapka Enterprise Linux (RHEL) 6.x
    * Oracle Linux (OL) 6.x
* Linux için, birleştirilmiş aracı yükleme dizinindeki tüm klasör erişim izinleri artık yalnızca yerel kullanıcıyla sınırlıdır.
* Windows'da, SQL Server ve Share Point kümeleri gibi yüklü dağıtılmış uygulamalarda yaygın dağıtılmış tutarlılık yer imleri verilirken oluşan zamanlama sorunu için bir düzeltme.
* Yapılandırma sunucusu temel yükleyicisinde günlükle ilgili düzeltme.
* Windows ana hedef taban yükleyicisine VMware vCLI 6.0 indirme bağlantısı eklendi.
* Failover ve olağanüstü durum kurtarma matkapları sırasında ağ yapılandırma değişiklikleri için ek denetimler ve günlükler eklendi.
* Bekletme bilgilerinin yapılandırma sunucusuna bildirilemelerine neden olan bir sorun düzeltmesi.  
* Fiziksel kümeler için, kaynak birimi küçülterken vContinuum sihirbazında birim yeniden boyutlandırmanın başarısız olmasıyla ilgili bir sorun düzeltmesi.
* Küme diski PRDM diskolduğunda, "Disk imzasını bulamayınca" hatayla başarısız olan küme koruma sorunu için düzeltme.
* Kapsama alanı dışında bir özel durum nedeniyle bir cxps aktarım sunucusu kilitlenme için bir düzeltme.
* Sunucu adı ve IP adresi sütunları artık vContinuum sihirbazının **Push Yükleme** sayfasında yeniden boyutlandırılabilir.
* RX API geliştirmeleri:
  * Mevcut en son beş ortak tutarlılık noktası artık kullanılabilir (yalnızca garantili etiketler).
  * Tüm korumalı aygıtlar için kapasite ve boş alan ayrıntıları görüntülenir.
  * Kaynak sunucuda Scout sürücü durumu kullanılabilir.

> [!NOTE]
> * **InMage_Scout_Standard_8.0.1_GA.zip** baz paketi vardır:
>     * Güncelleştirilmiş bir yapılandırma sunucusu temel yükleyicisi (**InMage_CX_8.0.1.0_Windows_GA_26Feb2015_release.exe**)
>     * Bir Windows ana hedef temel yükleyici (**InMage_Scout_vContinuum_MT_8.0.1.0_Windows_GA_26Feb2015_release.exe**).
>     * Tüm yeni yüklemeler için yeni yapılandırma sunucusunu ve Windows ana hedef GA bitlerini kullanın.
> * Güncelleme 4 doğrudan 8.0.1 GA uygulanabilir.
> * Yapılandırma sunucusu ve RX güncelleştirmeleri uygulandıktan sonra geri alınamıyor.


### <a name="azure-site-recovery-scout-801-update-3"></a>Azure Site Kurtarma İzleyicisi 8.0.1 Güncelleme 3

Tüm Site Kurtarma güncelleştirmeleri birikmeli. Güncelleştirme 3, Güncelleştirme 1 ve Güncelleştirme 2'deki tüm düzeltmeleri içerir. Güncelleme 3 doğrudan 8.0.1 GA uygulanabilir. Yapılandırma sunucusu ve RX güncelleştirmeleri uygulandıktan sonra geri alınamıyor.

#### <a name="bug-fixes-and-enhancements"></a>Hata düzeltmeleri ve geliştirmeleri
Güncelleme 3 aşağıdaki sorunları giderir:

* Yapılandırma sunucusu ve RX, proxy'nin arkasında olduklarında kasada kayıtlı değildir.
* Kurtarma noktası hedefine (RPO) ulaşılamayan saat sayısı sağlık raporunda güncelleştirilmez.
* ESX donanım ayrıntıları veya ağ ayrıntıları UTF-8 karakterleri içerdiğinde yapılandırma sunucusu RX ile eşitleme yapmıyor.
* Windows Server 2008 R2 etki alanı denetleyicileri kurtarma dan sonra başlamaz.
* Çevrimdışı eşitleme beklendiği gibi çalışmıyor.
* VM başarısız olduktan sonra, çoğaltma çifti silme yapılandırma sunucu konsolunda uzun süre ilerlemez. Kullanıcılar geri başarısız mı tamamlayabilir veya işlemleri sürdüremez.
* SQL Server istemcileri gibi uygulama bağlantı kesilmelerini azaltmaya yardımcı olmak için tutarlılık işine göre genel anlık görüntü işlemleri en iyi duruma getirilmiştir.
* Tutarlılık aracı (VACP.exe) performansı geliştirildi. Windows'da anlık görüntü oluşturmak için gereken bellek kullanımı azaltıldı.
* Parola 16 karakterden büyük olduğunda itme yükleme hizmeti çöker.
* vContinuum, kimlik bilgileri değiştirildiğinde yeni vCenter kimlik bilgilerini denetlemez ve istemetmez.
* Linux'ta, ana hedef önbellek yöneticisi (cachemgr) işlem sunucusundan dosya indirmiyor. Bu çoğaltma çifti azaltma ile sonuçlanır.
* Fiziksel failover kümesi (MSCS) disk sırası tüm düğümlerde aynı değilse, çoğaltma küme birimlerinin bazıları için ayarlı değildir. Küme, bu düzeltmeden yararlanmak için yeniden korunmalıdır.  
* RX Scout 7.1'den Scout 8.0.1'e yükseltildikten sonra SMTP işlevselliği beklendiği gibi çalışmıyor.
* Tamamlamak için gereken süreyi izlemek için geri alma işlemi için günlükte daha fazla istatistik eklendi.
* Destek kaynak sunucuda Linux işletim sistemleri için eklenmiştir:
  * Red Hat Enterprise Linux (RHEL) 6 güncelleme 7
  * CentOS 6 güncelleme 7
* Yapılandırma sunucusu ve RX konsolları artık bitmap moduna giren çift için bildirimler gösterir.
* RX'te aşağıdaki güvenlik düzeltmeleri eklendi:
    * Parametre tahrifatı ile yetkilendirme bypass: Geçerli olmayan kullanıcılara sınırlı erişim.
    * Site içi istek sahteciliği: Sayfa belirteç kavramı uygulandı ve her sayfa için rastgele üretiyor. Bu, aynı kullanıcı için yalnızca tek bir oturum açma örneği olduğu ve sayfa yenilemenin çalışmadığı anlamına gelir. Bunun yerine, panoya yönlendirir.
    * Kötü amaçlı dosya yükleme: Dosyalar belirli uzantılarla sınırlıdır: z, aiff, asf, avi, bmp, csv, doc, docx, fla, flv, gif, gz, gzip, jpeg, jpg, log, mid, mov, mp3, mp4, mpc, mpeg, mpg, ods, odt, pdf, png, ppt, pptx, pxd, qt, ram, rar, rm, rmi, rm, rm, rm, rm, rm, rm, rm, rm, mov, mp3, mp4, mpc, mpeg, mpg, ods, odt, pdf, png, ppt, pptx, pxd, qt, ram, rar, rm, rmi, rm, rtm, , sdc, sitd, swf, sxc, sxw, tar, tgz, tif, tiff, txt, vsd, wav, wma, wmv, xls, xlsx, xml ve zip.
    * Kalıcı site arası komut dosyası: Giriş doğrulamaları eklendi.

### <a name="azure-site-recovery-scout-801-update-2-update-03dec15"></a>Azure Site Kurtarma İzleyicisi 8.0.1 Güncelleme 2 (Güncelleme 03Dec15)

Güncelleştirme 2'deki düzeltmeler şunlardır:

* **Yapılandırma sunucusu**: Yapılandırma sunucusu Azure Site Kurtarma kasasına kaydedildiğinde, 31 günlük ücretsiz ölçüm özelliğinin beklendiği gibi çalışmasını engelleyen sorunlar.
* **Unified agent**: Güncelleştirme 1'de, sürüm 8.0'dan 8.0.1'e yükseltme sırasında güncelleştirmenin ana hedef sunucuya yüklenmemesi ile sonuçlanan bir sorunu düzeltme.

### <a name="azure-site-recovery-scout-801-update-1"></a>Azure Site Kurtarma İzleyicisi 8.0.1 Güncelleme 1
Güncelleştirme 1 aşağıdaki hata düzeltmeleri ve yeni özellikleri içerir:

* Sunucu örneği başına 31 günlük ücretsiz koruma. Bu, işlevselliği sınamamanızı veya kavram kanıtı ayarlamanızı sağlar.
* Failover ve failback dahil olmak üzere sunucudaki tüm işlemler ilk 31 gün ücretsizdir. Bir sunucu ilk Site Kurtarma Scout ile korunduğunda zaman başlar. 32. günden itibaren, her korumalı sunucu, Site Kurtarma koruması için standart örnek fiyatıüzerinden müşteriye ait bir siteye ücretlendirilir.
* İstediğin zaman, şu anda şarj edilmekte olan korumalı sunucuların sayısı kasadaki **Pano'da** kullanılabilir.
* vSphere Command-Line Interface (vCLI) 5.5 Update 2 için destek eklendi.
* Destek kaynak sunucuda bu Linux işletim sistemleri için eklendi:
    * RHEL 6 Güncelleme 6
    * RHEL 5 Güncelleme 11
    * CentOS 6 Güncelleme 6
    * CentOS 5 Güncelleme 11
* Hata aşağıdaki sorunları gidermek için giderir:
  * Yapılandırma sunucusu veya RX sunucusu için kasa kaydı başarısız olur.
  * Kümelenmiş VM'ler devam ettikçe yeniden korunduğunda küme birimleri beklendiği gibi görünmez.
  * Ana hedef sunucu şirket içi üretim VM'lerinden farklı bir ESXi sunucusunda barındırıldığında failgeri başarısız olur.
  * Yapılandırma dosyası izinleri 8.0.1'e yükselttiğinde değiştirilir. Bu değişiklik koruma ve işlemleri etkiler.
  * Yeniden eşitleme eşiği beklendiği gibi zorlanmaz ve tutarsız çoğaltma davranışına neden olur.
  * RPO ayarları yapılandırma sunucusu konsolunda doğru görünmez. Sıkıştırılmamış veri değeri sıkıştırılmış değeri yanlış gösterir.
  * Kaldır işlemi vContinuum sihirbazında beklendiği gibi silinmez ve çoğaltma yapılandırma sunucusu konsolundan silinmez.
  * vContinuum sihirbazında, MSCS VM'lerinin korunması sırasında disk görünümündeki **Ayrıntılar'ı** tıklattığınızda disk otomatik olarak seçilmez.
  * Fiziksel-sanal (P2V) senaryosunda, gerekli HP hizmetleri (CIMnotify ve CqMgHost gibi) VM kurtarma kılavuzuna taşınmaz. Bu sorun ek önyükleme süresi ile sonuçlanır.
  * Ana hedef sunucuda 26'dan fazla disk olduğunda Linux VM koruması başarısız olur.

