---
title: Azure Geçişi'nde Hyper-V geçişi desteği
description: Azure Geçişi ile Hyper-V geçişi desteği hakkında bilgi edinin.
ms.topic: conceptual
ms.date: 04/15/2020
ms.openlocfilehash: 8ec0b72cac75518ac938faa202b28d055409e8dc
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81538197"
---
# <a name="support-matrix-for-hyper-v-migration"></a>Hyper-V geçişi için destek matrisi

Bu makalede, Azure Geçişi ile Hyper-V VM'lerin geçişine yönelik destek ayarları ve sınırlamaları [özetlenmiştir: Sunucu Geçişi.](migrate-services-overview.md#azure-migrate-server-migration-tool) Azure'a geçiş için Hyper-V VM'leri değerlendirme hakkında bilgi arıyorsanız, [değerlendirme destek matrisini](migrate-support-matrix-hyper-v.md)gözden geçirin.

## <a name="migration-limitations"></a>Geçiş sınırlamaları

Çoğaltma için aynı anda en fazla 10 VM seçebilirsiniz. Daha fazla makine yiyebilmek istiyorsanız, 10'dan oluşan gruplar halinde çoğalTın.


## <a name="hyper-v-hosts"></a>Hyper-V konakları

| **Destek**                | **Şey**               
| :-------------------       | :------------------- |
| **Dağıtım**       | Hyper-V ana bilgisayar tek başına veya bir kümede dağıtılabilir. <br/>Azure Geçir çoğaltma yazılımı (Hyper-V Çoğaltma sağlayıcısı) Hyper-V ana bilgisayarlarına yüklenir.|
| **İzinler**           | Hyper-V ana bilgisayarda yönetici izinlerine ihtiyacınız vardır. |
| **Ana bilgisayar işletim sistemi** | Windows Server 2019, Windows Server 2016 veya Windows Server 2012 R2. |
| **Bağlantı noktası erişimi** |  VM çoğaltma verilerini göndermek için HTTPS bağlantı noktası 443'teki giden bağlantılar.

### <a name="url-access-public-cloud"></a>URL erişimi (genel bulut)

Hyper-V ana bilgisayarlarındaki çoğaltma sağlayıcı yazılımının bu URL'lere erişmesi gerekir.

**URL** | **Şey**
--- | ---
login.microsoftonline.com | Active Directory'yi kullanarak denetime ve kimlik yönetimine erişin.
backup.windowsazure.com | Çoğaltma veri aktarımı ve koordinasyonu.
*.hypervrecoverymanager.windowsazure.com | Geçiş için kullanılır.
*.blob.core.windows.net | Depolama hesaplarına veri yükleyin. 
dc.services.visualstudio.com | Dahili izleme için kullanılan uygulama günlüklerini yükleyin.
time.windows.com | Sistem ve genel zaman arasındaki zaman eşitleme doğrular.

### <a name="url-access-azure-government"></a>URL erişimi (Azure Kamu)

Hyper-V ana bilgisayarlarındaki çoğaltma sağlayıcı yazılımının bu URL'lere erişmesi gerekir.

**URL** | **Şey**
--- | ---
login.microsoftonline.us | Active Directory'yi kullanarak denetime ve kimlik yönetimine erişin.
backup.windowsazure.us | Çoğaltma veri aktarımı ve koordinasyonu.
*.hypervrecoverymanager.windowsazure.us | Geçiş için kullanılır.
*.blob.core.usgovcloudapi.net | Depolama hesaplarına veri yükleyin.
dc.services.visualstudio.com | Dahili izleme için kullanılan uygulama günlüklerini yükleyin.
time.nist.gov | Sistem ve genel zaman arasındaki zaman eşitleme doğrular.


## <a name="hyper-v-vms"></a>Hyper-V Sanal Makineleri

| **Destek**                  | **Şey**               
| :----------------------------- | :------------------- |
| **İşletim sistemi** | Azure tarafından desteklenen tüm [Windows](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines) ve [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) işletim sistemleri. |
| **Azure için gerekli değişiklikler** | Bazı VM'ler Azure'da çalıştırabilmeleri için değişiklik gerektirebilir. Geçişden önce el ile ayarlamalar yapın. İlgili makaleler, bunun nasıl yapılacağını anlatan yönergeler içerir. |
| **Linux önyükleme**                 | /boot özel bir bölüm üzerindeyse, işletim sistemi diskinde yer almalı ve birden çok diske yayılmamalıdır.<br/> /boot kök (/) bölümünün bir parçasıysa, '/' bölümü işletim sistemi diskinde olmalı ve diğer disklere yayılmamalıdır. |
| **UEFI önyükleme**                  | Azure'da geçirilen VM otomatik olarak BIOS önyükleme VM'sine dönüştürülür. VM Windows Server 2012 ve daha sonra yalnızca çalışıyor olmalıdır. Os diskenin en fazla beş veya daha az bölümü olmalı ve işletim sistemi diskinin boyutu 300 GB'dan az olmalıdır.
  |
| **Disk boyutu**                  | Os diski için 2 TB, veri diskleri için 4 TB.
| **Disk numarası** | VM başına en fazla 16 disk.
| **Şifreli diskler/birimler**    | Geçiş için desteklenmez. |
| **RDM/geçiş diskleri**      | Geçiş için desteklenmez. |
| **Paylaşılan disk** | Paylaşılan diskleri kullanan VM'ler geçiş için desteklenmez.
| **NFS**                        | VM'lerde birim olarak monte edilen NFS birimleri çoğaltılamaz. |
| **Iscsı**                      | iSCSI hedefli VM'ler geçiş için desteklenmez.
| **Hedef disk**                | Yalnızca yönetilen disklerle Azure VM'lerine geçiş yapabilirsiniz. |
| **IPv6** | Desteklenmiyor.
| **NIC takım çalışması** | Desteklenmiyor.
| **Azure Site Recovery** | VM, Azure Site Kurtarma ile çoğaltma için etkinleştirildiyse, Azure Geçir Sunucusu Geçişi'ni kullanarak çoğaltma yapamazsınız.
| **Bağlantı Noktaları** | VM çoğaltma verilerini göndermek için HTTPS bağlantı noktası 443'teki giden bağlantılar.

## <a name="azure-vm-requirements"></a>Azure VM gereksinimleri

Azure'da çoğaltılan tüm şirket içi VM'ler bu tabloda özetlenen Azure VM gereksinimlerini karşılamalıdır.

**Bileşen** | **Gereksinimler** | **Şey**
--- | --- | ---
İşletim sistemi disk boyutu | 2.048 GB'a kadar. | Desteklenmezse denetim başarısız olur.
İşletim sistemi disk sayısı | 1 | Desteklenmezse denetim başarısız olur.
Veri diski sayısı | 16 ya da daha az. | Desteklenmezse denetim başarısız olur.
Veri diskboyutu | 4.095 GB'a kadar | Desteklenmezse denetim başarısız olur.
Ağ bağdaştırıcıları | Birden çok bağdaştırıcı desteklenir. |
Paylaşılan VHD | Desteklenmiyor. | Desteklenmezse denetim başarısız olur.
FC diski | Desteklenmiyor. | Desteklenmezse denetim başarısız olur.
BitLocker | Desteklenmiyor. | Bir makineiçin çoğaltmayı etkinleştirmeden önce BitLocker devre dışı edilmelidir.
VM adı | 1 ile 63 arasında karakter.<br/> Harfler, sayılar ve kısa çizgilerden oluşabilir.<br/><br/> Makine adı bir harf veya sayı ile başlayıp bitmelidir. |  Site Kurtarma'daki makine özelliklerindeki değeri güncelleştirin.
Geçiş-Windows'dan sonra bağlan | Geçişten sonra Windows çalıştıran Azure VM'lere bağlanmak için:<br/> - Geçiş ten önce şirket içi VM'de RDP'yi etkinleştirin. TCP ve UDP kurallarının **Ortak** profil için eklendiğinden ve tüm profillerde **Windows Güvenlik Duvarı** > **İzin Verilen Uygulamalar** içinde RDP’ye izin verildiğinden emin olun.<br/> Siteden siteye VPN erişimi için RDP'yi etkinleştirin **ve Etki Alanı** ve Özel ağlar için Windows Güvenlik **Duvarı** -> **İzin Verilen uygulamalara ve özelliklere** izin verin. Buna ek olarak, işletim sisteminin SAN **ilkesinin OnlineAll**olarak ayarlı olup olmadığını kontrol edin. [Daha fazla bilgi edinin](prepare-for-migration.md). |
Geçişten sonra bağlan-Linux | SSH kullanarak geçişten sonra Azure VM'lere bağlanmak için:<br/> Geçişten önce, şirket içi makinede Secure Shell hizmetinin Başlangıç olarak ayarlanıp ayarlılmadan ve güvenlik duvarı kurallarının SSH bağlantısına izin verdiğini kontrol edin.<br/> Azure VM'de başarısız olduktan sonra, VM üzerinden başarısız olan ağ güvenlik grubu kuralları ve bağlı olduğu Azure alt ağı için SSH bağlantı noktasına gelen bağlantılara izin verin. Ayrıca, VM için genel bir IP adresi ekleyin. |  

## <a name="next-steps"></a>Sonraki adımlar

Geçiş için [Hyper-V VM'leri geçirin.](tutorial-migrate-hyper-v.md)
