---
title: Windows Server/System Center VMM 2012 R2'yi Windows Server 2016-Azure Site Kurtarma'ya yükseltin
description: Azure Site Kurtarma hizmetiyle Azure Yığın VM'leri için Azure'da olağanüstü durum kurtarma yı nasıl ayarlayamanızı öğrenin.
services: site-recovery
author: rajani-janaki-ram
manager: rochakm
ms.topic: conceptual
ms.service: site-recovery
ms.date: 12/03/2018
ms.author: rajanaki
ms.openlocfilehash: 1d94935db542a0e64754ab8769996fe906f88b46
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73954416"
---
# <a name="upgrade-windows-server-serversystem-center-2012-r2-vmm-to-windows-servervmm-2016"></a>Windows Server Server/System Center 2012 R2 VMM'yi Windows Server/VMM 2016'ya yükseltin 

Bu makale, Azure Site Kurtarma ile yapılandırılan Windows Server 2012 R2 ana bilgisayarlarını & Nasıl Windows Server 2012 R2 ana bilgisayarlarını Windows Server 2016 & SCVMM 2016'ya nasıl yükselteceklerinizi gösterir

Site Kurtarma, iş sürekliliği nize ve olağanüstü durum kurtarma (BCDR) stratejinize katkıda bulunur. Hizmet, beklenen ve beklenmeyen kesintiler oluştuğunda VM iş yüklerinizin kullanılabilir kalmasını sağlar.

> [!IMPORTANT]
> Azure Site Kurtarma ile çoğaltma için zaten yapılandırılmış olan Windows Server 2012 R2 ana bilgisayarlarını yükselttiğinde, bu belgede belirtilen adımları izlemeniz gerekir. Yükseltme için seçilen herhangi bir alternatif yol desteklenmeyen durumlara neden olabilir ve çoğaltmada bir kopmaya veya başarısız olma yeteneğine neden olabilir.


Bu makalede, ortamınızda aşağıdaki yapılandırmaları nasıl yükselteceklerinizi öğreneceksiniz:

> [!div class="checklist"]
> * **SCVMM tarafından yönetilmeyen Windows Server 2012 R2 ana bilgisayarları** 
> * **Windows Server 2012 Bağımsız Bir SCVMM 2012 R2 sunucusu tarafından yönetilen R2 ana bilgisayarlar** 
> * **Windows Server 2012 R2 ana bilgisayarlar, son derece kullanılabilir SCVMM 2012 R2 sunucusu tarafından yönetilir**


## <a name="prerequisites--factors-to-consider"></a>Göz önünde bulundurulması gereken & faktörlerönkoşullar

Yükseltmeden önce aşağıdakileri not edin:-

- Windows Server 2012 R2 ana bilgisayarlar, SCVMM tarafından yönetilmeyen ve tek başına bir ortam kurulumu ysa, yükseltmeyi gerçekleştirmeye çalışırsanız çoğaltmada bir ara olur.
- SCVMM 2012 R2'yi yüklerken "*Anahtarlarımı Dağıtılmış Anahtar Yönetimi altında Active Directory'de*saklamayın" seçtiyseniz, yükseltmeler başarıyla tamamlanmaz.

- System Center 2012 R2 VMM kullanıyorsanız, 

    - VMM: **VMM konsol** -> **ayarları** -> **Genel** -> Veritabanı**bağlantısı** veritabanı bilgilerini kontrol edin
    - System Center Virtual Machine Manager Agent hizmeti için kullanılan hizmet hesaplarını kontrol edin
    - VMM Veritabanı'nın yedeğinin olduğundan emin olun.
    - İlgili SCVMM sunucularının veritabanı adını not edin. Bu **VMM konsol** -> **Ayarları** -> **Genel** -> Veritabanı**bağlantısı** navigasyon tarafından yapılabilir
    - Hem 2012R2 birincil hem de kurtarma VMM sunucularının VMM Kimliğini not edin. VMM KIMLIĞI "HKLM:\SOFTWARE\Microsoft\Microsoft\Microsoft System Center Virtual Machine Manager Server\Setup" kayıt defterinden bulunabilir.
    - Kümeye eklediğiniz yeni SCVM'lerin önceki adlarla aynı adlara sahip olduğundan emin olun. 

- Her iki tarafta da SCVMM'ler tarafından yönetilen iki siteniz arasında çoğalıyorsanız, birincil tarafı yükseltmeden önce kurtarma tarafınızı yükselttinizden emin olun.
  > [!WARNING]
  > Dağıtılmış Anahtar Yönetimi altında SCVMM 2012 R2'yi yükseltirken, **şifreleme anahtarlarını Active Directory'de depolamayı**seçin. Servis hesabının ayarlarını seçin ve anahtar yönetimini dikkatle dağıtın. Seçiminizi temel alan şablonlar daki parolalar gibi şifrelenmiş veriler yükseltmeden sonra kullanılamayabilir ve Azure Site Kurtarma ile çoğaltmayı etkileyebilir

> [!IMPORTANT]
> Lütfen [ön koşulların](https://docs.microsoft.com/system-center/vmm/upgrade-vmm?view=sc-vmm-2016#requirements-and-limitations) ayrıntılı SCVMM belgelerine bakın

## <a name="windows-server-2012-r2-hosts-which-arent-managed-by-scvmm"></a>SCVMM tarafından yönetilmeyen Windows Server 2012 R2 ana bilgisayarları 
Aşağıda belirtilen adımların listesi, bu [öğreticiyi](https://docs.microsoft.com/azure/site-recovery/hyper-v-prepare-on-premises-tutorial) izleyerek [Hyper-V ana bilgisayarlarından Azure'a](https://docs.microsoft.com/azure/site-recovery/hyper-v-azure-architecture) kullanıcı yapılandırması için geçerlidir

> [!WARNING]
> Ön koşullarda belirtildiği gibi, bu adımlar yalnızca kümelenmiş bir ortam senaryosu için geçerlidir ve tek başına hyper-V ana bilgisayar yapılandırmasında geçerli değildir.

1. [Yuvarlanma küme yükseltmegerçekleştirmek](https://docs.microsoft.com/windows-server/failover-clustering/cluster-operating-system-rolling-upgrade#cluster-os-rolling-upgrade-process) için adımları izleyin. yuvarlanma küme yükseltme işlemini yürütmek için.
2. Kümede tanıtılan her yeni Windows Server 2016 ana bilgisayarıyla, söz konusu adımları [burada] izleyerek Bir Windows Server 2012 R2 ana bilgisayarının başvurularını Azure Site Kurtarma'dan kaldırın. Bu, kümeden boşaltmak & ayırmayı seçtiğiniz ana bilgisayar olmalıdır.
3. Tüm sanal makineler için *Update-VMVersion* komutu çalıştırıldıktan sonra yükseltmeler tamamlanır. 
4. Yeni Windows Server 2016 ana bilgisayarını Azure Site Kurtarma'ya kaydetmek için [burada](https://docs.microsoft.com/azure/site-recovery/hyper-v-azure-tutorial#set-up-the-source-environment) belirtilen adımları kullanın. Hyper-V sitesinin zaten etkin olduğunu ve kümedeki yeni ana bilgisayarı kaydetmeniz gerektiğini lütfen unutmayın. 
5.  Azure portalına gidin ve Kurtarma Hizmetleri içinde çoğaltılan sistem durumunu doğrulayın

## <a name="upgrade-windows-server-2012-r2-hosts-managed-by-stand-alone-scvmm-2012-r2-server"></a>Windows Server 2012 R2 ana bilgisayarlarını tek başına SCVMM 2012 R2 sunucusu tarafından yönetilen yükseltme
Windows Sever 2012 R2 ana bilgisayarlarınızı yükseltmeden önce SCVMM 2012 R2'yi SCVMM 2016'ya yükseltmeniz gerekir. Aşağıdaki adımları izleyin:-

**Bağımsız SCVMM 2012 R2'yi SCVMM 2016'ya yükseltin**

1.  Microsoft Azure Site Kurtarma'>Programları ve Özellikleri > Denetim Masası -> Programlara ve Özelliklere giderek ASR sağlayıcısını kaldırın ve Kaldır'a tıklayın
2. [SCVMM veritabanını koruyun ve işletim sistemini yükseltin](https://docs.microsoft.com/system-center/vmm/upgrade-vmm?view=sc-vmm-2016#back-up-and-upgrade-the-operating-system)
3. **Kaldırma Programları Ekle'de**VMM**Kaldır'ı** **VMM** > seçin. b. **Özellikleri Kaldır'ı**seçin ve ardından V MM yönetim sunucusu**ve VMM Konsolu'nü**seçin. c. **Veritabanı**Seçenekleri'nde, **Veritabanını Koru'u**seçin. d. Özeti gözden geçirin ve **Kaldır'ı**tıklatın.

4. [VMM 2016'yı yükleyin](https://docs.microsoft.com/system-center/vmm/upgrade-vmm?view=sc-vmm-2016#install-vmm-2016)
5. SCVMM'yi başlatın ve **Kumaşlar** sekmesi altında her ana bilgisayar için durum kontrol edin. En son durumu almak için **Yenile'yi** tıklatın. Durumu "Dikkat Edilmesi Gerekenler" olarak görmelisiniz. 
17. SCVMM'deki en son [Microsoft Azure Site Kurtarma Sağlayıcısı'nı](https://aka.ms/downloaddra) yükleyin.
16. Kümenin her ana bilgisayarına en son [Microsoft Azure Kurtarma Hizmeti (MARS) aracısını](https://aka.ms/latestmarsagent) yükleyin. SCVMM'nin ana bilgisayarları başarılı bir şekilde sorgulayabilmesini sağlamak için yenileyin.

**Windows Server 2012 R2 ana bilgisayarlarını Windows Server 2016'ya yükseltin**

1. Yuvarlanma kümesi yükseltme işlemini yürütmek için [burada](https://docs.microsoft.com/windows-server/failover-clustering/cluster-operating-system-rolling-upgrade#cluster-os-rolling-upgrade-process) belirtilen adımları izleyin. 
2. Kümeye yeni ana bilgisayar ekledikten sonra, vmm aracısını bu güncelleştirilmiş ana bilgisayara yüklemek için ana bilgisayarı SCVMM konsolundan yenileyin.
3. Sanal makinelerin VM sürümlerini güncelleştirmek için *Update-VMVersion'u* çalıştırın. 
4.  Azure portalına gidin ve Kurtarma Hizmetleri Kasası'ndaki sanal makinelerin çoğaltılan sistem durumunu doğrulayın. 

## <a name="upgrade-windows-server-2012-r2-hosts-are-managed-by-highly-available-scvmm-2012-r2-server"></a>Yükseltme Windows Server 2012 R2 ana bilgisayarlar, yüksek kullanılabilirSe'si olan SCVMM 2012 R2 sunucusu tarafından yönetilir
Windows Sever 2012 R2 ana bilgisayarlarınızı yükseltmeden önce SCVMM 2012 R2'yi SCVMM 2016'ya yükseltmeniz gerekir. Azure Site Kurtarma ile yapılandırılan SCVMM 2012 R2 sunucuları yükseltirken aşağıdaki yükseltme modları desteklenir - Ek VMM sunucuları olmayan karışık mod & Ek VMM sunucuları ile karma mod.

**SCVMM 2012 R2'yi SCVMM 2016'ya yükseltin**

1.  Microsoft Azure Site Kurtarma'>Programları ve Özellikleri > Denetim Masası -> Programlara ve Özelliklere giderek ASR sağlayıcısını kaldırın ve Kaldır'a tıklayın
2. Yürütmek istediğiniz yükseltme moduna göre [burada](https://docs.microsoft.com/system-center/vmm/upgrade-vmm?view=sc-vmm-2016#upgrade-a-standalone-vmm-server) belirtilen adımları izleyin.
3. SCVMM konsolu başlatın ve **Kumaşlar** sekmesi altında her ana bilgisayar durumunu kontrol edin. En son durumu almak için **Yenile'yi** tıklatın. Durumu "Dikkat Edilmesi Gerekenler" olarak görmelisiniz.
4. SCVMM'deki en son [Microsoft Azure Site Kurtarma Sağlayıcısı'nı](https://aka.ms/downloaddra) yükleyin.
5. Kümenin her ana bilgisayarındaki en son [Microsoft Azure Kurtarma Hizmeti (MARS) aracısını](https://aka.ms/latestmarsagent) güncelleştirin. SC VMM'nin ana bilgisayarları başarılı bir şekilde sorgulayabilmesini sağlamak için yenileyin.


**Windows Server 2012 R2 ana bilgisayarlarını Windows Server 2016'ya yükseltin**

1. Yuvarlanma kümesi yükseltme işlemini yürütmek için [burada](https://docs.microsoft.com/windows-server/failover-clustering/cluster-operating-system-rolling-upgrade#cluster-os-rolling-upgrade-process) belirtilen adımları izleyin.
2. Kümeye yeni ana bilgisayar ekledikten sonra, vmm aracısını bu güncelleştirilmiş ana bilgisayara yüklemek için ana bilgisayarı SCVMM konsolundan yenileyin.
3. Sanal makinelerin VM sürümlerini güncelleştirmek için *Update-VMVersion'u* çalıştırın. 
4.  Azure portalına gidin ve Kurtarma Hizmetleri Kasası'ndaki sanal makinelerin çoğaltılan sistem durumunu doğrulayın. 

## <a name="next-steps"></a>Sonraki adımlar
Ana bilgisayarların yükseltmesi yapıldıktan sonra, çoğaltma ve olağanüstü durum kurtarma durumunun durumunu test etmek için bir [test başarısızlığı](tutorial-dr-drill-azure.md) gerçekleştirebilirsiniz.

