---
title: PerfInsights Linux Microsoft Azure'da nasıl kullanılır? Microsoft Dokümanlar
description: Linux VM performans sorunlarını gidermek için PerfInsights'ı nasıl kullanacağınızı öğrenir.
services: virtual-machines-linux'
documentationcenter: ''
author: anandhms
manager: cshepard
editor: na
tags: ''
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.topic: troubleshooting
ms.date: 7/10/2019
ms.author: genli
ms.openlocfilehash: 19b2fcaed2c80d4ca52ada9f9f0898479e73bcf2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79266994"
---
# <a name="how-to-use-perfinsights"></a>PerfInsights’ı kullanma

[PerfInsights Linux,](https://aka.ms/perfinsightslinuxdownload) tanılama verilerini toplayan ve analiz eden ve Azure'daki Linux sanal makine performansı sorunlarını gidermeye yardımcı olacak bir rapor sağlayan bir kendi kendine yardım tanılama aracıdır. PerfInsights, desteklenen sanal makinelerde bağımsız bir araç olarak veya [Azure sanal makineleri için Performans Tanılama'yı](performance-diagnostics.md)kullanarak doğrudan portaldan çalıştırılabilir.

Destekle iletişime geçmeden önce sanal makinelerle ilgili performans sorunları yaşıyorsanız, bu aracı çalıştırın.

## <a name="supported-troubleshooting-scenarios"></a>Desteklenen sorun giderme senaryoları

PerfInsights çeşitli türde bilgileri toplayabilir ve analiz edebilir. Aşağıdaki bölümler sık karşılaşılan senaryoları kapsar.

### <a name="quick-performance-analysis"></a>Hızlı performans analizi

Bu senaryo, sanal makinenizin depolama ve donanım yapılandırması gibi temel bilgileri toplar, aşağıdakiler dahil olmak üzere çeşitli günlükleri:

- İşletim Sistemi bilgileri

- PCI cihaz bilgileri

- Genel Konuk İşletim Sistemi günlükleri

- Yapılandırma dosyaları

- Depolama bilgileri

- Azure Sanal Makine Yapılandırması [(Azure Örneği Meta veri Hizmeti](https://docs.microsoft.com/azure/virtual-machines/windows/instance-metadata-service)kullanılarak toplanır)

- Çalışan işlemler, Disk, Bellek ve CPU kullanımı listesi

- Ağ bilgileri

Bu, sistemi etkilememesi gereken pasif bir bilgi koleksiyonudur.

>[!Note]
>Hızlı performans analizi senaryosu otomatik olarak aşağıdaki senaryoların her birine dahil edilir:

### <a name="performance-analysis"></a>Performans analizi

Bu senaryo Hızlı performans çözümlemesine benzer, ancak tanılama bilgilerinin daha uzun süre yakalanmasını sağlar.

## <a name="what-kind-of-information-is-collected-by-perfinsights"></a>PerfInsights tarafından ne tür bilgiler toplanır

Linux sanal makine, işletim sistemi, blok cihazlar, yüksek kaynak tüketiciler, yapılandırma ve çeşitli günlükleri hakkında bilgi toplanır. Daha fazla ayrıntı aşağıdadır:

- İşletim sistemi
  - Linux dağıtımı ve sürümü
  - Çekirdek bilgileri
  - Sürücü bilgileri

- Donanım
  - PCI cihazları`*`[ ]

- Süreçler ve bellek
  - İşlemlistesi (görev adı, kullanılan bellek, açılan dosyalar)
  - Toplam, kullanılabilir ve ücretsiz fiziksel bellek
  - Toplam, kullanılabilir ve serbest takas belleği
  - CPU'nun ve işlemlerin CPU kullanımını 5 saniye aralığında yakalama profilleme
  - 5 saniye lik aralıklarla işlemlerbellek kullanımının profil oluşturma

- Ağ Oluşturma  
  - Bağdaştırıcı istatistiklerine sahip ağ bağdaştırıcıları listesi
  - Ağ yönlendirme tablosu
  - Açılan bağlantı noktaları ve durumu

- Depolama
  - Aygıtlistesini engelleme
  - Bölümler listesi
  - Montaj noktaları listesi
  - MDADM birim bilgileri
  - LVM birim bilgileri
  - 5 saniye lik aralıklarla tüm disklerde profil oluşturma yakalama

- Günlükler
  - /var/log/iletiler
  - /var/log/syslog
  - /var/log/kern.log
  - /var/log/cron.log
  - /var/log/boot.log
  - /var/log/yum.log
  - /var/log/dpkg.log
  - /var/log/cloud-init.log
  - /var/log/cloud-init-output.log
  - /var/log/gpu-manager.log
  - /var/log/waagent.log
  - /var/log/azure/[uzantı]/\*günlük\*
  - /var/opt/microsoft/omsconfig/omsconfig.log
  - /var/opt/microsoft/omsagent/log/omsagent.log
  - /etc/waagent.config
  - Son beş gün için journalctl çıktısı

- [Azure sanal makine örneği meta verileri](https://docs.microsoft.com/azure/virtual-machines/windows/instance-metadata-service)

>[!Note]
>[`*`] PCI bilgileri henüz Debian ve SLES dağıtımlarında toplanmadı

## <a name="run-the-perfinsights-linux-on-your-vm"></a>VM'nizde PerfInsights Linux'u çalıştırın

### <a name="what-do-i-have-to-know-before-i-run-the-tool"></a>Aracı çalıştırmadan önce bilmem gerekenler

#### <a name="tool-requirements"></a>Araç gereksinimleri

- Bu araç, performans sorunu olan VM üzerinde çalıştırılmalıdır.
- Python 2.7 VM yüklü olmalıdır

- Aşağıdaki dağıtımlar şu anda desteklenir:

    | Dağıtım               | Sürüm                                         |
    |----------------------------|-------------------------------------------------|
    | Oracle Linux Sunucusu        | 6.10`*`[ ], 7.3, 7.6, 7.5 (Oracle-Database-Ee 13.8 pazar görüntüsü)|
    | CentOS                     | 6.5`*`[ ], 7.6                                    |
    | RHEL                       | 7.2, 7.5, 8.0 [`*`]                               |
    | Ubuntu                     | 14.04, 16.04, 18.04                               |
    | Debian                     | 8, 9, 10 [ ]`*`                                    |
    | SLES                       | 12 SP4`*`[ ]                                      |
    |                            |                                                   |

>[!Note]
>[`*`] Lütfen [Bilinen sorunlar](#known-issues) bölümüne bakın

### <a name="known-issues"></a>Bilinen sorunlar

- RHEL 8 varsayılan olarak Python yüklü değildir. PerfInsights Linux'u çalıştırmak için önce Python 2.7'yi yüklemeniz gerekir

- Guest Agent bilgi toplama CentOS 6.x üzerinde başarısız olabilir

- PCI cihazları bilgileri Debian tabanlı dağıtımlarda toplanmaz

- LVM bilgileri kısmen bazı dağıtımlarda toplanır

### <a name="how-do-i-run-perfinsights"></a>PerfInsights'ı nasıl çalıştırıyorum?

Azure portalından Azure Performans Tanılama'yı yükleyerek PerfInsights'ı sanal bir makinede çalıştırabilirsiniz. Ayrıca bağımsız bir araç olarak çalıştırabilirsiniz.

>[!Note]
>PerfInsights verileri toplar ve analiz eder. Sistemde herhangi bir değişiklik yapmaz.

#### <a name="install-and-run-perfinsights-from-the-azure-portal"></a>Azure portalından PerfInsights'ı yükleyin ve çalıştırın

Bu seçenek hakkında daha fazla bilgi için [Azure Performans Tanılama'ya](performance-diagnostics.md)bakın.  

#### <a name="run-perfinsights-in-standalone-mode"></a>PerfInsights'ı bağımsız modda çalıştırın

PerfInsights aracını çalıştırmak için aşağıdaki adımları izleyin:

1. [PerfInsights.tar.gz'yi](https://aka.ms/perfinsightslinuxdownload) sanal makinenizdeki bir klasöre indirin ve aşağıdaki komutları kullanarak terminalden içindekileri ayıklayın.

   ```bash
   wget https://download.microsoft.com/download/9/F/8/9F80419C-D60D-45F1-8A98-718855F25722/PerfInsights.tar.gz
   ```

   ```bash
   tar xzvf PerfInsights.tar.gz
   ```

2. Dosya içeren `perfinsights.py` klasöre gidin ve `perfinsights.py` ardından kullanılabilir komut satırı parametrelerini görüntülemek için çalıştırın.

    ```bash
    cd <the path of PerfInsights folder>
    sudo python perfinsights.py
    ```

    ![PerfInsights Linux komut satırı çıkışının ekran görüntüsü](media/how-to-use-perfinsights-linux/perfinsights-linux-commandline.png)

    PerfInsights senaryolarını çalıştırmak için temel sözdizimi:

    ```bash
    sudo python perfinsights.py -r <ScenarioName> -d [duration]<H | M | S> [AdditionalOptions]
    ```

    Hızlı performans çözümleme senaryosunu 1 dakika çalıştırmak ve /tmp/output klasörü altında sonuçları oluşturmak için aşağıdaki örneği kullanabilirsiniz:

    ```bash
    sudo python perfinsights.py -r quick -d 1M -a -o /tmp/output
    ```

    Aşağıdaki örneği kullanarak 5 dakika boyunca performans analizi senaryosunu çalıştırabilir ve sonuç katran topını depolama hesabına yükleyebilirsiniz:

    ```bash
    sudo python perfinsights.py -r vmslow -d 300S -a -t <StorageAccountName> -k <StorageAccountKey> -i <full resource Uri of the current VM>
    ```

    >[!Note]
    >Bir senaryoyu çalıştırmadan önce, PerfInsights kullanıcıdan tanılama bilgilerini paylaşmayı ve EULA'yı kabul etmeyi kabul etmesini ister. Bu istemleri atlamak için **-a veya -accept-claimclaimer-and-share-diagnostics** seçeneğini kullanın.
    >
    >Microsoft ile etkin bir destek biletiniz varsa ve birlikte çalıştığınız destek mühendisinin isteği ne olursa olsun PerfInsights çalıştırıyorsanız, destek bilet numarasını **-s veya -destek isteği** seçeneğini kullanarak verdiğinizden emin olun.

Çalıştırma tamamlandığında, çıkış klasörü belirtilmediği sürece PerfInsights ile aynı klasörde yeni bir katran dosyası görünür. Dosyanın adı **PerformanceDiagnostics\_yyyy-MM-dd\_hh-mm-ss-fff.tar.gz.** Bu dosyayı çözümleme için destek aracısına gönderebilir veya bulguları ve önerileri gözden geçirmek için dosya içindeki raporu açabilirsiniz.

## <a name="review-the-diagnostics-report"></a>Tanılama raporunu gözden geçirme

**PerformanceDiagnostics\_yyyy-MM-dd\_hh-mm-ss-fff.tar.gz** dosyasında, PerfInsights'ın bulgularını ayrıntılarıyla gösteren bir HTML raporu bulabilirsiniz. Raporu gözden geçirmek için **\_PerformanceDiagnostics yyyy-MM-dd\_hh-mm-ss-fff.tar.gz** dosyasını genişletin ve ardından **PerfInsights Report.html** dosyasını açın.

### <a name="overview-tab"></a>Genel Bakış sekmesi

**Genel Bakış** sekmesi temel çalışma ayrıntılarını ve sanal makine bilgilerini sağlar. **Bulgular** sekmesi, PerfInsights raporunun tüm farklı bölümlerinden gelen önerilerin bir özetini görüntüler.

![PerfInsights Raporu Ekran Görüntüsü](media/how-to-use-perfinsights-linux/perfinsights-linux-overview.png)  
![PerfInsights Raporu Ekran Görüntüsü](media/how-to-use-perfinsights-linux/perfinsights-linux-findings-tab.png)

> [!NOTE]
> Yüksek olarak sınıflandırılan bulgular, performans sorunlarına neden olabilecek bilinen sorunlardır. Orta olarak sınıflandırılan bulgular, performans sorunlarına neden olmayan en iyi olmayan yapılandırmaları temsil eder. Düşük olarak kategorize edilen bulgular yalnızca bilgilendirici ifadelerdir.

Tüm yüksek ve orta bulgular için önerileri ve bağlantıları gözden geçirin. Performansı nasıl etkileyebilecekleri ve performans için en iyi yapılandırmalar hakkında bilgi edinin.

### <a name="cpu-tab"></a>CPU sekmesi

**CPU** sekmesi, PerfInsights çalışması sırasında sistem genelinde CPU tüketimi hakkında bilgi sağlar. Yüksek CPU kullanım süreleri ve en uzun süre çalışan CPU tüketicileri hakkında bilgi, CPU ile ilgili yüksek sorunları gidermek için yararlı olacaktır.

![PerfInsights Rapor CPU sekmesinin ekran görüntüsü](media/how-to-use-perfinsights-linux/perfinsights-linux-cpu-tab.png)

### <a name="storage-tab"></a>Depolama sekmesi

**Bulgular** bölümünde depolamayla ilgili çeşitli bulgular ve öneriler görüntülenir.

**Blok Aygıtları** ve **Bölümler,** **LVM**ve **MDADM** sekmeleri gibi diğer ilgili bölümler, blok aygıtlarının nasıl yapılandırıldığı ve birbiriyle nasıl ilişkili olduğunu açıklar.

![Depolama sekmesinin ekran görüntüsü](media/how-to-use-perfinsights-linux/perfinsights-linux-storage-tab.png)  
![MDADM sekmesinin ekran görüntüsü](media/how-to-use-perfinsights-linux/perfinsights-linux-mdadm-config.png)

### <a name="linux-tab"></a>Linux sekmesi

**Linux** sekmesi, VM'nizde çalışan donanım ve işletim sistemi hakkında bilgiler içerir. Ayrıntılar, çalışan işlemlerin listesini ve Konuk Aracı, PCI, CPU, Sürücüler ve LIS sürücüleri hakkındaki bilgileri içerir.

![Linux sekmesinin ekran görüntüsü](media/how-to-use-perfinsights-linux/perfinsights-linux-tab.png)

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla inceleme için tanılama günlüklerini ve raporlarını Microsoft Destek'e yükleyebilirsiniz. Microsoft Destek personeliyle çalıştığınızda, sorun giderme işlemine yardımcı olmak için PerfInsights tarafından oluşturulan çıktıyı iletmenizi isteyebilirler.

Aşağıdaki ekran görüntüsü, alabileceklerinize benzer bir ileti gösterir:

![Microsoft Desteği'nden örnek iletinin ekran görüntüsü](media/how-to-use-perfinsights-linux/support-email.png)

Dosya aktarım çalışma alanına erişmek için iletideki yönergeleri izleyin. Ek güvenlik için parolanızı ilk kullanımda değiştirmeniz gerekir.

Oturum açtıktan sonra, PerfInsights tarafından toplanan **PerformanceDiagnostics\_yyyy-MM-dd\_hh-mm-ss-fff.tar.gz** dosyasını yüklemek için bir iletişim kutusu bulacaksınız.
