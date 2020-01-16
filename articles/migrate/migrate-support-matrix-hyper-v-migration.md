---
title: Azure geçişi için Hyper-V geçişi desteği
description: Azure geçişi ile Hyper-V geçişi desteği hakkında bilgi edinin.
ms.topic: conceptual
ms.date: 01/08/2020
ms.openlocfilehash: 96bf423f25911d0befbfd420ac2fb01ba6c8fb65
ms.sourcegitcommit: dbcc4569fde1bebb9df0a3ab6d4d3ff7f806d486
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2020
ms.locfileid: "76030269"
---
# <a name="support-matrix-for-hyper-v-migration"></a>Hyper-V geçişi için destek matrisi

Bu makalede, Hyper-V VM 'lerini Azure geçişi ile geçirmeye yönelik destek ayarları ve sınırlamaları özetlenmektedir [: sunucu geçişi](migrate-services-overview.md#azure-migrate-server-migration-tool) . Hyper-V VM 'lerini Azure 'a geçiş için değerlendirme hakkında bilgi arıyorsanız, [değerlendirme desteği matrisini](migrate-support-matrix-hyper-v.md)gözden geçirin.

## <a name="migration-limitations"></a>Geçiş sınırlamaları

Çoğaltma için aynı anda en fazla 10 VM seçebilirsiniz. Daha fazla makine geçirmek istiyorsanız, 10 grup içinde çoğaltın.


## <a name="hyper-v-hosts"></a>Hyper-V ana bilgisayarları

| **Destek**                | **Ayrıntılar**               
| :-------------------       | :------------------- |
| **Dağıtım**       | Hyper-V konağı tek başına olabilir veya bir kümede dağıtılabilir. |
| **İzinler**           | Hyper-V konağında yönetici izinlerine sahip olmanız gerekir. |
| **Konak işletim sistemi** | Windows Server 2019, Windows Server 2016 veya Windows Server 2012 R2. |
| **URL erişimi** | Hyper-V konaklarının bu URL 'LERE erişmesi gerekir:<br/><br/> -login.microsoftonline.com: Active Directory kullanarak erişim denetimi ve kimlik yönetimi.<br/><br/> -*. backup.windowsazure.com: çoğaltma veri aktarımı ve düzenlemesi. Hizmet URL 'Lerini geçirin.<br/><br/> -*. blob.core.windows.net: verileri depolama hesaplarına yükleyin.<br/><br/> -dc.services.visualstudio.com: iç izleme için kullanılan uygulama günlüklerini karşıya yükleyin.<br/><br/> - time.windows.com | Sistem ve genel saat arasındaki saat eşitlemesini doğrular.
| **Bağlantı noktası erişimi** |  VM çoğaltma verilerini göndermek için HTTPS bağlantı noktası 443 ' deki giden bağlantılar.

## <a name="hyper-v-vms"></a>Hyper-V Sanal Makineleri

| **Destek**                  | **Ayrıntılar**               
| :----------------------------- | :------------------- |
| **İşletim sistemi** | Azure tarafından desteklenen tüm [Windows](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines) ve [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) işletim sistemleri. |
| **İzinler**           | Değerlendirmek istediğiniz her Hyper-V VM üzerinde yönetici izinlerine sahip olmanız gerekir. |
| **Tümleştirme Hizmetleri**       | [Hyper-V tümleştirme hizmetlerinin](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/integration-services) , işletim sistemi bilgilerini yakalamak için değerlendirmekte olduğunuz VM 'lerde çalışıyor olması gerekir. |
| **Azure için gereken değişiklikler** | Bazı VM 'Ler, Azure 'da çalışabilecek şekilde değişiklik gerektirebilir. Azure geçişi, bu değişiklikleri aşağıdaki işletim sistemleri için otomatik olarak yapar:<br/> -Red Hat Enterprise Linux 6.5 +, 7.0 +<br/> -CentOS 6.5 +, 7.0 +</br> -SUSE Linux Enterprise Server 12 SP1 +<br/> -Ubuntu 14.04 LTS, 16.04 LTS, 18.04 LTS<br/> -Detem 7, 8<br/><br/> Diğer işletim sistemleri için geçişten önce el ile ayarlamalar yapmanız gerekir. İlgili makaleler, bunun nasıl yapılacağı hakkında yönergeler içerir. |
| **Linux önyüklemesi**                 | /Boot ayrılmış bir bölümse, işletim sistemi diskinde bulunmalı ve birden çok diske yayılmamalıdır.<br/> /Boot kök (/) bölümünün parçasıysa, '/' bölümünün işletim sistemi diskinde olması ve diğer disklere yayılmamamakta olması gerekir. |
| **UEFı önyüklemesi**                  | Azure 'daki geçirilmiş VM otomatik olarak bir BIOS önyükleme VM 'sine dönüştürülür. VM yalnızca Windows Server 2012 ve üstünü çalıştırmalıdır. İşletim sistemi diski en fazla beş bölüm veya daha az olmalıdır ve işletim sistemi diskinin boyutu 300 GB 'tan az olmalıdır.
  |
| **Disk boyutu**                  | işletim sistemi diski için 2 TB, veri diskleri için 4 TB.
| **Disk numarası** | VM başına en fazla 16 disk.
| **Şifrelenmiş diskler/birimler**    | Geçiş için desteklenmez. |
| **RDM/geçiş diskleri**      | Geçiş için desteklenmez. |
| **Paylaşılan disk** | Paylaşılan diskler kullanan VM 'Ler geçiş için desteklenmez.
| **ALACAĞıNı**                        | VM 'Lere birim olarak bağlanmış NFS birimleri çoğaltılmaz. |
| **ISCSı**                      | Iscsı hedefleri olan VM 'Ler geçiş için desteklenmez.
| **Hedef disk**                | Azure VM 'lerine yalnızca yönetilen disklerle geçiş yapabilirsiniz. |
| **IPv6** | Desteklenmez.
| **NIC ekibi oluşturma** | Desteklenmez.
| **Azure Site Recovery** | VM, Azure Site Recovery ile çoğaltma için etkinleştirilmişse Azure geçişi geçiş sunucusu geçişini çoğaltamaz.
| **Bağlantı Noktaları** | VM çoğaltma verilerini göndermek için HTTPS bağlantı noktası 443 ' deki giden bağlantılar.

## <a name="azure-vm-requirements"></a>Azure VM gereksinimleri

Azure 'a çoğaltılan tüm şirket içi VM 'Lerin bu tabloda özetlenen Azure VM gereksinimlerini karşılaması gerekir. Site Recovery, çoğaltma için bir önkoşul denetimi çalıştırdığında, bazı gereksinimler karşılanmazsa denetim başarısız olur.

**Bileşen** | **Gereksinimler** | **Ayrıntılar**
--- | --- | ---
Konuk işletim sistemi | Geçiş için desteklenen VMware VM işletim sistemlerini doğrular.<br/> Desteklenen bir işletim sistemi üzerinde çalışan herhangi bir iş yükünü geçirebilirsiniz. | Desteklenmiyorsa denetim başarısız olur.
Konuk işletim sistemi mimarisi | 64 bit. | Desteklenmiyorsa denetim başarısız olur.
İşletim sistemi disk boyutu | 2\.048 GB 'a kadar. | Desteklenmiyorsa denetim başarısız olur.
İşletim sistemi disk sayısı | 1 | Desteklenmiyorsa denetim başarısız olur.
Veri diski sayısı | 64 veya daha az. | Desteklenmiyorsa denetim başarısız olur.
Veri diski boyutu | 4\.095 GB 'a kadar | Desteklenmiyorsa denetim başarısız olur.
Ağ bağdaştırıcıları | Birden çok bağdaştırıcı desteklenir. |
Paylaşılan VHD | Desteklenmez. | Desteklenmiyorsa denetim başarısız olur.
FC diski | Desteklenmez. | Desteklenmiyorsa denetim başarısız olur.
BitLocker | Desteklenmez. | Bir makine için çoğaltmayı etkinleştirmeden önce BitLocker devre dışı bırakılmalıdır.
VM adı | 1 ile 63 karakter arasında.<br/> Harfler, sayılar ve kısa çizgilerden oluşabilir.<br/><br/> Makine adı bir harf veya sayıyla başlamalı ve bitmelidir. |  Site Recovery makine özelliklerindeki değeri güncelleştirin.
Geçişten sonra Bağlan-Windows | Geçişten sonra Windows çalıştıran Azure VM 'lerine bağlanmak için:<br/> -Geçiş öncesinde, şirket içi VM 'de RDP 'yi mümkün. TCP ve UDP kurallarının **Ortak** profil için eklendiğinden ve tüm profillerde **Windows Güvenlik Duvarı** > **İzin Verilen Uygulamalar** içinde RDP’ye izin verildiğinden emin olun.<br/> Siteden siteye VPN erişimi için, RDP 'yi etkinleştirin ve **etki alanı ve özel** ağlar Için **izin verilen uygulamalar ve Özellikler** -> **Windows Güvenlik Duvarı** 'nda RDP 'ye izin verin. Ayrıca, işletim sisteminin SAN ilkesinin **OnlineAll**olarak ayarlandığından emin olun. [Daha fazla bilgi edinin](prepare-for-migration.md). |
Geçişten sonra Bağlan-Linux | SSH kullanarak geçişten sonra Azure VM 'lerine bağlanmak için:<br/> Geçişten önce, şirket içi makinede, Secure Shell hizmetinin başlangıç olarak ayarlandığını ve Güvenlik Duvarı kurallarının bir SSH bağlantısına izin vermeyi kontrol edin.<br/> Yük devretmenin ardından Azure VM 'de, yük devredilen VM 'deki ağ güvenlik grubu kuralları için SSH bağlantı noktasına gelen bağlantılara ve bağlı olduğu Azure alt ağına izin verin. Ayrıca, VM için bir genel IP adresi ekleyin. |  

## <a name="next-steps"></a>Sonraki adımlar

Geçiş için [Hyper-V VM 'Lerini geçirin](tutorial-migrate-hyper-v.md) .
