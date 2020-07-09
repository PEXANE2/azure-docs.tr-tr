---
title: Azure geçişi için Hyper-V geçişi desteği
description: Azure geçişi ile Hyper-V geçişi desteği hakkında bilgi edinin.
ms.topic: conceptual
ms.date: 04/15/2020
ms.openlocfilehash: 1ea7d139b3d3cc8c14e43ccfb7c233fcbe4c564c
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86122073"
---
# <a name="support-matrix-for-hyper-v-migration"></a>Hyper-V geçişi için destek matrisi

Bu makalede, Hyper-V VM 'lerini Azure geçişi ile geçirmeye yönelik destek ayarları ve sınırlamaları özetlenmektedir [: sunucu geçişi](migrate-services-overview.md#azure-migrate-server-migration-tool) . Hyper-V VM 'lerini Azure 'a geçiş için değerlendirme hakkında bilgi arıyorsanız, [değerlendirme desteği matrisini](migrate-support-matrix-hyper-v.md)gözden geçirin.

## <a name="migration-limitations"></a>Geçiş sınırlamaları

Çoğaltma için aynı anda en fazla 10 VM seçebilirsiniz. Daha fazla makine geçirmek istiyorsanız, 10 grup içinde çoğaltın.


## <a name="hyper-v-host-requirements"></a>Hyper-V konağı gereksinimleri

| **Destek**                | **Ayrıntılar**               
| :-------------------       | :------------------- |
| **Dağıtım**       | Hyper-V konağı tek başına olabilir veya bir kümede dağıtılabilir. <br/>Azure geçişi çoğaltma yazılımı (Hyper-V çoğaltma sağlayıcısı), Hyper-V konaklarına yüklenir.|
| **İzinler**           | Hyper-V konağında yönetici izinlerine sahip olmanız gerekir. |
| **Konak işletim sistemi** | Windows Server 2019, Windows Server 2016 veya Windows Server 2012 R2. |
| **Bağlantı noktası erişimi** |  VM çoğaltma verilerini göndermek için HTTPS bağlantı noktası 443 ' deki giden bağlantılar.


## <a name="hyper-v-vms"></a>Hyper-V Sanal Makineleri

| **Destek**                  | **Ayrıntılar**               
| :----------------------------- | :------------------- |
| **İşletim sistemi** | Azure tarafından desteklenen tüm [Windows](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines) ve [Linux](../virtual-machines/linux/endorsed-distros.md) işletim sistemleri. |
**Windows Server 2003** | Windows Server 2003 çalıştıran VM 'Ler için, geçişten önce [Hyper-V tümleştirme hizmetlerini yüklemeniz](prepare-windows-server-2003-migration.md) gerekir. | 
**Azure 'da Linux VM 'Leri** | Bazı VM 'Ler, Azure 'da çalışabilecek şekilde değişiklik gerektirebilir.<br/><br/> Linux için Azure geçişi, değişiklikleri bu işletim sistemleri için otomatik olarak yapar:<br/> -Red Hat Enterprise Linux 6.5 +, 7.0 +<br/> -CentOS 6.5 +, 7.0 +</br> -SUSE Linux Enterprise Server 12 SP1 +<br/> -Ubuntu 14.04 LTS, 16.04 LTS, 18.04 LTS<br/> -Detem 7, 8. Diğer işletim sistemleri için [gerekli değişiklikleri](prepare-for-migration.md#linux-machines) el ile yaparsınız.
| **Azure için gereken değişiklikler** | Bazı VM 'Ler, Azure 'da çalışabilecek şekilde değişiklik gerektirebilir. Geçişten önce ayarları el ile yapın. İlgili makaleler, bunun nasıl yapılacağı hakkında yönergeler içerir. |
| **Linux önyüklemesi**                 | /Boot ayrılmış bir bölümse, işletim sistemi diskinde bulunmalı ve birden çok diske yayılmamalıdır.<br/> /Boot kök (/) bölümünün parçasıysa, '/' bölümünün işletim sistemi diskinde olması ve diğer disklere yayılmamamakta olması gerekir. |
| **UEFı önyüklemesi**                  | Azure 'daki geçirilmiş VM otomatik olarak bir BIOS önyükleme VM 'sine dönüştürülür. VM yalnızca Windows Server 2012 ve üstünü çalıştırmalıdır. İşletim sistemi diski en fazla beş bölüm veya daha az olmalıdır ve işletim sistemi diskinin boyutu 300 GB 'tan az olmalıdır.|
| **Disk boyutu**                  | işletim sistemi diski için 2 TB, veri diskleri için 4 TB.|
| **Disk numarası** | VM başına en fazla 16 disk.|
| **Şifrelenmiş diskler/birimler**    | Geçiş için desteklenmez.|
| **RDM/geçiş diskleri**      | Geçiş için desteklenmez.|
| **Paylaşılan disk** | Paylaşılan diskler kullanan VM 'Ler geçiş için desteklenmez.|
| **NFS**                        | VM 'Lere birim olarak bağlanmış NFS birimleri çoğaltılmaz.|
| **ISCSı**                      | Iscsı hedefleri olan VM 'Ler geçiş için desteklenmez.
| **Hedef disk**                | Azure VM 'lerine yalnızca yönetilen disklerle geçiş yapabilirsiniz. |
| **IPv6** | Desteklenmiyor.|
| **NIC ekibi oluşturma** | Desteklenmiyor.|
| **Azure Site Recovery** | VM, Azure Site Recovery ile çoğaltma için etkinleştirilmişse Azure geçişi geçiş sunucusu geçişini çoğaltamaz.|
| **Adet** | VM çoğaltma verilerini göndermek için HTTPS bağlantı noktası 443 ' deki giden bağlantılar.|

### <a name="url-access-public-cloud"></a>URL erişimi (genel bulut)

Hyper-V konaklarındaki çoğaltma sağlayıcısı yazılımının bu URL 'Lere erişmesi gerekir.

**URL** | **Ayrıntılar**
--- | ---
login.microsoftonline.com | Active Directory kullanarak erişim denetimi ve kimlik yönetimi.
backup.windowsazure.com | Çoğaltma veri aktarımı ve düzenlemesi.
*.hypervrecoverymanager.windowsazure.com | Çoğaltma yönetimi için kullanılır.
*.blob.core.windows.net | Verileri depolama hesaplarına yükleyin. 
dc.services.visualstudio.com | İç izleme için kullanılan uygulama günlüklerini karşıya yükleyin.
time.windows.com | Sistem ve genel saat arasındaki saat eşitlemesini doğrular.

### <a name="url-access-azure-government"></a>URL erişimi (Azure Kamu)

Hyper-V konaklarındaki çoğaltma sağlayıcısı yazılımının bu URL 'Lere erişmesi gerekir.

**URL** | **Ayrıntılar**
--- | ---
login.microsoftonline.us | Active Directory kullanarak erişim denetimi ve kimlik yönetimi.
backup.windowsazure.us | Çoğaltma veri aktarımı ve düzenlemesi.
*. hypervrecoverymanager.windowsazure.us | Çoğaltma yönetimi için kullanılır.
*. blob.core.usgovcloudapi.net | Verileri depolama hesaplarına yükleyin.
dc.services.visualstudio.com | İç izleme için kullanılan uygulama günlüklerini karşıya yükleyin.
time.nist.gov | Sistem ve genel saat arasındaki saat eşitlemesini doğrular.

## <a name="azure-vm-requirements"></a>Azure VM gereksinimleri

Azure 'a çoğaltılan tüm şirket içi VM 'Lerin bu tabloda özetlenen Azure VM gereksinimlerini karşılaması gerekir.

**Bileşen** | **Gereksinimler** | **Ayrıntılar**
--- | --- | ---
İşletim sistemi disk boyutu | 2.048 GB 'a kadar. | Desteklenmiyorsa denetim başarısız olur.
İşletim sistemi disk sayısı | 1 | Desteklenmiyorsa denetim başarısız olur.
Veri diski sayısı | 16 veya daha az. | Desteklenmiyorsa denetim başarısız olur.
Veri diski boyutu | 4.095 GB 'a kadar | Desteklenmiyorsa denetim başarısız olur.
Ağ bağdaştırıcıları | Birden çok bağdaştırıcı desteklenir. |
Paylaşılan VHD | Desteklenmiyor. | Desteklenmiyorsa denetim başarısız olur.
FC diski | Desteklenmiyor. | Desteklenmiyorsa denetim başarısız olur.
BitLocker | Desteklenmiyor. | Bir makine için çoğaltmayı etkinleştirmeden önce BitLocker devre dışı bırakılmalıdır.
VM adı | 1 ile 63 karakter arasında.<br/> Harfler, sayılar ve kısa çizgilerden oluşabilir.<br/><br/> Makine adı bir harf veya sayıyla başlamalı ve bitmelidir. |  Site Recovery makine özelliklerindeki değeri güncelleştirin.
Geçişten sonra Bağlan-Windows | Geçişten sonra Windows çalıştıran Azure VM 'lerine bağlanmak için:<br/><br/> -Geçişten önce, şirket içi VM 'de RDP 'yi etkinleştirin. TCP ve UDP kurallarının **Ortak** profil için eklendiğinden ve tüm profillerde **Windows Güvenlik Duvarı** > **İzin Verilen Uygulamalar** içinde RDP’ye izin verildiğinden emin olun.<br/><br/> -Siteden siteye VPN erişimi için, RDP 'yi etkinleştirin ve **Windows Firewall**  ->  **etki alanı ile özel** ağlar için Windows Güvenlik Duvarı**izin verilen uygulamalar ve Özellikler** ' de RDP 'ye izin verin. Ayrıca, işletim sisteminin SAN ilkesinin **OnlineAll**olarak ayarlandığından emin olun. [Daha fazla bilgi edinin](prepare-for-migration.md). |
Geçişten sonra Bağlan-Linux | SSH kullanarak geçişten sonra Azure VM 'lerine bağlanmak için:<br/><br/> -Geçiş öncesinde, şirket içi makinede, Secure Shell hizmetinin başlangıç olarak ayarlandığını ve Güvenlik Duvarı kurallarının bir SSH bağlantısına izin vermeyi kontrol edin.<br/><br/> -Geçiş sonrasında Azure VM 'de, yük devredilen VM 'deki ağ güvenlik grubu kuralları için SSH bağlantı noktasına gelen bağlantılara ve bağlı olduğu Azure alt ağına izin verin. Ayrıca, VM için bir genel IP adresi ekleyin. |  

## <a name="next-steps"></a>Sonraki adımlar

Geçiş için [Hyper-V VM 'Lerini geçirin](tutorial-migrate-hyper-v.md) .
