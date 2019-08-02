---
title: Hyper-V değerlendirmesi ve geçişi için Azure geçişi destek matrisi
description: Azure geçişi hizmetini kullanarak Hyper-V değerlendirmesi ve geçişi için ayarları ve sınırlamaları özetler.
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 07/22/2019
ms.author: raynew
ms.openlocfilehash: 787bcf28a64c98f82deb0725e2362bf8144d6c4e
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68640858"
---
# <a name="support-matrix-for-hyper-v-assessment-and-migration"></a>Hyper-V değerlendirmesi ve geçiş için destek matrisi

Makineleri değerlendirmek ve Microsoft Azure buluta geçirmek için [Azure geçişi hizmetini](migrate-overview.md) kullanabilirsiniz. Bu makalede, şirket içi Hyper-V VM 'lerini değerlendirmek ve geçirmek için destek ayarları ve sınırlamaları özetlenmektedir.



## <a name="hyper-v-scenarios"></a>Hyper-V senaryoları

Tablo, Hyper-V VM 'Leri için desteklenen senaryoları özetler.

**Dağıtım** | **Bilgileri***
--- | ---
**Şirket içi Hyper-V VM 'lerini değerlendirme** | İlk değerlendirmenizi [ayarlayın](tutorial-prepare-hyper-v.md) .<br/><br/> Büyük ölçekli bir değerlendirme [çalıştırın](scale-hyper-v-assessment.md) .
**Hyper-V VM’lerini Azure’a geçirme** | Azure 'a geçiş [yapmayı deneyin](tutorial-migrate-hyper-v.md) .



## <a name="azure-migrate-projects"></a>Azure geçişi projeleri

**Destek** | **Ayrıntılar**
--- | ---
Azure izinleri | Azure geçişi projesi oluşturmak için abonelikte katkıda bulunan veya sahip izinlerinizin olması gerekir.
Hyper-V Sanal Makineleri | Tek bir projede 10.000 adede kadar Hyper-V VM 'yi değerlendirin. Bir Azure aboneliğinde birden çok projeniz olabilir. Bir proje, değerlendirme sınırlarına kadar hem VMware VM 'lerini hem de Hyper-V sanal makinelerini içerebilir.
Coğrafya | Azure geçişi projelerini çeşitli coğrafi bölgelerde oluşturabilirsiniz. Belirli ograflarda projeler oluşturabilseniz de, diğer hedef konumlar için makineleri değerlendirebilir veya geçirebilirsiniz. Proje Coğrafya yalnızca keşfedilen meta verileri depolamak için kullanılır.

  **Coğrafya** | **Meta veri depolama konumu**
  --- | ---
  Azure Kamu | ABD Devleti Virginia
  Asya Pasifik | Güneydoğu Asya veya Doğu Asya
  Avrupa | Güney Avrupa veya Batı Avrupa
  Birleşik Krallık | UK Güney veya UK Batı
  Amerika Birleşik Devletleri | Orta ABD veya Batı ABD 2


 > [!NOTE]
 > Azure Kamu desteği şu anda yalnızca Azure geçişi 'nin [eski sürümünde](https://docs.microsoft.com/azure/migrate/migrate-services-overview#azure-migrate-versions) sunulmaktadır.


## <a name="assessment-hyper-v-host-requirements"></a>Değerlendirme-Hyper-V ana bilgisayar gereksinimleri

| **Destek**                | **Ayrıntılar**               
| :-------------------       | :------------------- |
| **Konak dağıtımı**       | Hyper-V konağı tek başına olabilir veya bir kümede dağıtılabilir. |
| **İzinler**           | Hyper-V konağında yönetici izinlerine sahip olmanız gerekir. |
| **Konak işletim sistemi** | Windows Server 2016 veya Windows Server 2012 R2.<br/> Windows Server 2019 çalıştıran Hyper-V konaklarında bulunan VM 'Leri değerlendiremiyoruz. |
| **PowerShell uzaktan Iletişim**   | Her konakta etkin olmalıdır. |
| **Hyper-V çoğaltma**       | Hyper-V çoğaltması kullanıyorsanız (veya aynı VM tanımlayıcılarına sahip birden fazla sanal makine varsa) Azure geçişi 'ni kullanarak hem özgün hem de çoğaltılan VM 'Leri keşfetiyorsanız, Azure geçişi tarafından oluşturulan değerlendirme doğru olmayabilir. |


## <a name="assessment-hyper-v-vm-requirements"></a>Değerlendirme-Hyper-V VM gereksinimleri

| **Destek**                  | **Ayrıntılar**               
| :----------------------------- | :------------------- |
| **İşletim sistemi** | Azure tarafından desteklenen tüm [Windows](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines) ve [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) işletim sistemleri. |
| **İzinler**           | Değerlendirmek istediğiniz her Hyper-V VM üzerinde yönetici izinlerine sahip olmanız gerekir. |
| **Tümleştirme Hizmetleri**       | [Hyper-V tümleştirme hizmetlerinin](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/integration-services) , işletim sistemi bilgilerini yakalamak için değerlendirmekte olduğunuz VM 'lerde çalışıyor olması gerekir. |
| **UEFı önyüklemesi**                  | UEFı önyüklemesi olan VM 'Ler geçiş için desteklenmez. |
| **Şifrelenmiş diskler/birimler**    | Şifrelenmiş disklere/birimlere sahip VM 'Ler geçiş için desteklenmez. |
| **RDM/geçiş diskleri**      | VM 'Lerin RDM veya PASSTHROUGH diskleri varsa, bu diskler Azure 'a çoğaltılmaz. |
| **ALACAĞINI**                        | VM 'Lere birim olarak bağlanmış NFS birimleri çoğaltılmaz. |
| **Hedef disk**                | Azure geçişi değerlendirmeleri, Azure VM 'lerine yalnızca yönetilen disklerle geçiş yapmanızı önerir. |


## <a name="assessment-appliance-requirements"></a>Değerlendirme-gereç gereksinimleri

Değerlendirme için Azure geçişi, Hyper-V VM 'lerini bulmaya ve VM meta verilerini ve performans verilerini Azure geçişi 'ne göndermenizi sağlamak için basit bir gereç çalıştırır. Gereç bir Hyper-V VM 'sinde çalışır ve Azure portal indirdiğinizde, sıkıştırılmış bir Hyper-V VHD 'SI kullanarak ayarlanır. Aşağıdaki tablo gereç gereksinimlerini özetler.

| **Destek**                | **Ayrıntılar**               
| :-------------------       | :------------------- |
| **Azure geçişi projesi**  |  Bir gereç, tek bir projeyle ilişkilendirilebilir.<br/> Tek bir gereç ile 5000 adede kadar Hyper-V sanal makinesi bulabilirsiniz.
| **Hyper-V**    |  Gereci bir Hyper-V VM 'si olarak dağıtırsınız.<br/> Belirtilen gereç sanal makinesi, Hyper-V VM sürüm 5,0 ' dir.<br/> VM konağının Windows Server 2012 R2 veya üstünü çalıştırması gerekir.<br/> Gereç VM 'si için 16 GB RAM, 4 sanal işlemci ve 1 harici anahtar ayırmak üzere yeterli alana sahip olması gerekir.<br/> Gereç statik veya dinamik bir IP adresi ve internet erişimi gerektirir.


## <a name="assessment-appliance-url-access"></a>Değerlendirme-gereç URL 'SI erişimi

VM 'Leri değerlendirmek için, Azure geçişi gereci internet bağlantısı gerektirir.

- Gereci dağıttığınızda Azure geçişi, aşağıdaki tabloda özetlenen URL 'lere bir bağlantı denetimi yapar.
- URL tabanlı bir ara sunucu kullanıyorsanız, tablodaki URL 'lere erişime izin verin ve proxy 'nin URL 'Leri ararken alınan CNAME kayıtlarını çözümlediği emin olun.
- Bir kesintiye uğratan ara sunucu varsa, sunucu sertifikasını proxy sunucusundan gereç 'ya aktarmanız gerekebilir.


**URL** | **Ayrıntılar**  
--- | ---
*.portal.azure.com | Azure portal gezinti
*.windows.net | Azure aboneliğinizde oturum açın
*.microsoftonline.com | Gereç için hizmet iletişimine yönelik Azure Active Directory uygulamalar oluşturma.
Management.Azure.com | Gereç için hizmet iletişimine yönelik Azure Active Directory uygulamalar oluşturma.
dc.services.visualstudio.com | Günlüğe kaydetme ve izleme
*.vault.azure.net | Gereç ve hizmet arasında iletişim kurarken Azure Key Vault gizli dizileri yönetin.
aka.ms/* | Diğer adıyla bağlantılarına erişime izin ver.
https://download.microsoft.com/download/* | Microsoft Download sitesinden indirmelere izin verir.



## <a name="assessment-port-requirements"></a>Değerlendirme-bağlantı noktası gereksinimleri

Aşağıdaki tabloda, değerlendirme için bağlantı noktası gereksinimleri özetlenmektedir.

**cihaz** | **bağlantı**
--- | ---
**Elektrikli** | TCP bağlantı noktası 3389 üzerindeki gelen bağlantılar, gereci Uzak Masaüstü bağlantılarına izin vermek için.<br/> 44368 numaralı bağlantı noktası üzerinden gereç yönetimi uygulamasına uzaktan erişim için gelen bağlantılar:``` https://<appliance-ip-or-name>:44368 ```<br/> Azure geçişi 'ne bulma ve performans meta verileri göndermek için 443 numaralı bağlantı noktası üzerinden giden bağlantılar.
**Hyper-V konağı/kümesi** | Genel Bilgi Modeli (CıM) oturumu kullanarak Hyper-V VM 'lerinin yapılandırma ve performans meta verilerini çekmek için WinRM bağlantı noktaları 5985 (HTTP) ve 5986 (HTTPS) üzerinde gelen bağlantılar.

## <a name="migration-hyper-v-host-requirements"></a>Geçiş-Hyper-V ana bilgisayar gereksinimleri

| **Destek**                | **Ayrıntılar**               
| :-------------------       | :------------------- |
| **Konak dağıtımı**       | Hyper-V konağı tek başına olabilir veya bir kümede dağıtılabilir. |
| **İzinler**           | Hyper-V konağında yönetici izinlerine sahip olmanız gerekir. |
| **Konak işletim sistemi** | Windows Server 2019, Windows Server 2016 veya Windows Server 2012 R2. |

## <a name="migration-hyper-v-vm-requirements"></a>Geçiş-Hyper-V VM gereksinimleri

| **Destek**                  | **Ayrıntılar**               
| :----------------------------- | :------------------- |
| **İşletim sistemi** | Azure tarafından desteklenen tüm [Windows](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines) ve [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) işletim sistemleri. |
| **İzinler**           | Değerlendirmek istediğiniz her Hyper-V VM üzerinde yönetici izinlerine sahip olmanız gerekir. |
| **Tümleştirme Hizmetleri**       | [Hyper-V tümleştirme hizmetlerinin](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/integration-services) , işletim sistemi bilgilerini yakalamak için değerlendirmekte olduğunuz VM 'lerde çalışıyor olması gerekir. |
| **Azure için gereken değişiklikler** | Bazı VM 'Ler, Azure 'da çalışabilecek şekilde değişiklik gerektirebilir. Azure geçişi, bu değişiklikleri aşağıdaki işletim sistemleri için otomatik olarak yapar:<br/> -Red Hat Enterprise Linux 6.5 +, 7.0 +<br/> -CentOS 6.5 +, 7.0 +</br> -SUSE Linux Enterprise Server 12 SP1 +<br/> -Ubuntu 14.04 LTS, 16.04 LTS, 18.04 LTS<br/> -Detem 7, 8<br/><br/> Diğer işletim sistemleri için geçişten önce el ile ayarlamalar yapmanız gerekir. İlgili makaleler, bunun nasıl yapılacağı hakkında yönergeler içerir. |
| **Linux önyüklemesi**                 | /Boot ayrılmış bir bölümse, işletim sistemi diskinde bulunmalı ve birden çok diske yayılmamalıdır.<br/> /Boot kök (/) bölümünün parçasıysa, '/' bölümünün işletim sistemi diskinde olması ve diğer disklere yayılmamamakta olması gerekir. |
| **UEFı önyüklemesi**                  | UEFı önyüklemesi olan VM 'Ler geçiş için desteklenmez.  |
| **Disk boyutu**                  | işletim sistemi diski için 2 TB, veri diskleri için 4 TB.
| **Disk numarası** | VM başına en fazla 16 disk.
| **Şifrelenmiş diskler/birimler**    | Geçiş için desteklenmez. |
| **RDM/geçiş diskleri**      | Geçiş için desteklenmez. |
| **Paylaşılan disk** | Paylaşılan diskler kullanan VM 'Ler geçiş için desteklenmez.
| **ALACAĞINI**                        | VM 'Lere birim olarak bağlanmış NFS birimleri çoğaltılmaz. |
| **ISCSI**                      | Iscsı hedefleri olan VM 'Ler geçiş için desteklenmez.
| **Hedef disk**                | Azure VM 'lerine yalnızca yönetilen disklerle geçiş yapabilirsiniz. |
| **IPv6** | Desteklenmiyor.
| **NIC ekibi oluşturma** | Desteklenmiyor.
| **Azure Site Recovery** | VM, Azure Site Recovery ile çoğaltma için etkinleştirilmişse Azure geçişi geçiş sunucusu geçişini çoğaltamaz.





## <a name="migration-hyper-v-host-url-access"></a>Geçiş-Hyper-V konağı URL 'SI erişimi

Aşağıdaki tabloda, Hyper-V konakları için URL erişim gereksinimleri özetlenmektedir.

**URL** | **Ayrıntılar**  
--- | ---
login.microsoftonline.com | Active Directory kullanarak erişim denetimi ve kimlik yönetimi.
*.backup.windowsazure.com | Çoğaltma veri aktarımı ve düzenlemesi.
*.hypervrecoverymanager.windowsazure.com | Azure geçişi hizmeti URL 'Lerine bağlanın.
*.blob.core.windows.net | Verileri depolama hesaplarına yükleyin.
dc.services.visualstudio.com | İç izleme için kullanılan uygulama günlüklerini karşıya yükleyin.
time.windows.com | Sistem ve genel saat arasındaki saat eşitlemesini doğrular.

## <a name="migration-port-access"></a>Geçiş-bağlantı noktası erişimi

Aşağıdaki tabloda, VM geçişi için Hyper-V konaklarında ve VM 'lerde bağlantı noktası gereksinimleri özetlenmektedir.

**cihaz** | **bağlantı**
--- | ---
Hyper-V konakları/VM 'Ler | HTTPS bağlantı noktası 443 ' deki giden bağlantılar, VM çoğaltma verilerini Azure geçişi 'ne gönderir.




## <a name="next-steps"></a>Sonraki adımlar

Geçiş için [Hyper-V VM değerlendirmesi Için hazırlanma](tutorial-prepare-hyper-v.md) .
