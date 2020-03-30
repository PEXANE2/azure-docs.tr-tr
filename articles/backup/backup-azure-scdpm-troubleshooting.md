---
title: System Center Data Protection Manager sorunlarını giderme
description: Bu makalede, Sistem Merkezi Veri Koruma Yöneticisi'ni kullanırken karşılaşabileceğiniz sorunların çözümlerini keşfedin.
ms.topic: troubleshooting
ms.date: 01/30/2019
ms.openlocfilehash: bcb30fa7eb3e05099761fc751b09a9fb16134e34
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75664759"
---
# <a name="troubleshoot-system-center-data-protection-manager"></a>System Center Data Protection Manager sorunlarını giderme

Bu makalede, Veri Koruma Yöneticisi kullanırken karşılaşabileceğiniz sorunların çözümleri açıklanmaktadır.

System Center Veri Koruma Yöneticisi için en son sürüm notları için [Sistem Merkezi belgelerine](https://docs.microsoft.com/system-center/dpm/dpm-release-notes?view=sc-dpm-2016)bakın. [Bu matriste](https://docs.microsoft.com/system-center/dpm/dpm-protection-matrix?view=sc-dpm-2016)Veri Koruma Yöneticisi desteği hakkında daha fazla bilgi edinebilirsiniz.

## <a name="error-replica-is-inconsistent"></a>Hata: Yineleme tutarsız

Yineleme aşağıdaki nedenlerle tutarsız olabilir:

- Çoğaltma oluşturma işi başarısız olur.
- Değişiklik günlüğüyle ilgili sorunlar var.
- Birim düzeyi filtresi bit eşlemi hatalar içerir.
- Kaynak makine beklenmedik bir şekilde kapanır.
- Eşitleme günlüğü taşar.
- Kopya gerçekten tutarsız.

Bu sorunu gidermek için aşağıdaki eylemleri gerçekleştirin:

- Tutarsız durumu kaldırmak için tutarlılık denetimini el ile çalıştırın veya günlük tutarlılık denetimi zamanlayın.
- Microsoft Azure Yedekleme Sunucusu ve Veri Koruma Yöneticisi'nin en son sürümünü kullandığınızdan emin olun.
- **Otomatik Tutarlılık** ayarını etkinleştirildiğinden emin olun.
- Hizmetleri komut isteminden yeniden başlatmayı deneyin. Aşağıdaki `net stop dpmra` komutu `net start dpmra`kullanın.
- Ağ bağlantısı ve bant genişliği gereksinimlerini karşıladığınızdan emin olun.
- Kaynak makinenin beklenmedik bir şekilde kapatılıp kapatılıp kapatılmadığını kontrol edin.
- Diskin sağlıklı olduğundan ve yineleme için yeterli alan olduğundan emin olun.
- Aynı anda çalışan yinelenen yedekleme işleri olmadığından emin olun.

## <a name="error-online-recovery-point-creation-failed"></a>Hata: Çevrimiçi kurtarma noktası oluşturma başarısız oldu

Bu sorunu gidermek için aşağıdaki eylemleri gerçekleştirin:

- Azure Yedekleme aracısının en son sürümünü kullandığınızdan emin olun.
- Koruma görev alanında el ile bir kurtarma noktası oluşturmaya çalışın.
- Veri kaynağıüzerinde tutarlılık denetimi çalıştırdığınızdan emin olun.
- Ağ bağlantısı ve bant genişliği gereksinimlerini karşıladığınızdan emin olun.
- Yineleme verileri tutarsız bir durumdaolduğunda, bu veri kaynağının bir disk kurtarma noktası oluşturun.
- Yinelemenin mevcut olduğundan ve eksik olmadığından emin olun.
- Yinelemenin güncelleştirme sıra numarası (USN) günlüğünü oluşturmak için yeterli alana sahip olduğundan emin olun.

## <a name="error-unable-to-configure-protection"></a>Hata: Korumayı yapılandıramıyor

Bu hata, Veri Koruma Yöneticisi sunucusu korumalı sunucuya başlayamazken oluşur.

Bu sorunu gidermek için aşağıdaki eylemleri gerçekleştirin:

- Azure Yedekleme aracısının en son sürümünü kullandığınızdan emin olun.
- Veri Koruma Yöneticisi sunucunuz la korumalı sunucunuz arasında bağlantı (ağ/güvenlik duvarı/proxy) olduğundan emin olun.
- Bir SQL sunucusunu koruyorsanız, Oturum **Açma Özellikleri** > **NT AUTHORITY\SYSTEM** özelliğinin **sysadmin** ayarını etkin olarak gösterdiğinden emin olun.

## <a name="error-server-not-registered-as-specified-in-vault-credential-file"></a>Hata: Sunucu kasa kimlik dosyasında belirtildiği gibi kayıtlı değil

Bu hata, Veri Koruma Yöneticisi/Azure Yedekleme sunucu verileri için kurtarma işlemi sırasında oluşur. Kurtarma işleminde kullanılan kasa kimlik bilgileri dosyası, Veri Koruma Yöneticisi/Azure Yedekleme sunucusunun Kurtarma Hizmetleri kasasına ait değildir.

Bu sorunu gidermek için şu adımları gerçekleştirin:

1. Veri Koruma Yöneticisi/Azure Yedekleme sunucusunun kayıtlı olduğu Kurtarma Hizmetleri kasasından kasa kimlik bilgilerini indirin.
2. En son indirilen tonoz kimlik bilgilerini kullanarak sunucuyu kasaya kaydetmeyi deneyin.

## <a name="error-no-recoverable-data-or-selected-server-not-a-data-protection-manager-server"></a>Hata: Kurtarılabilir veri veya seçili sunucu yok, Veri Koruma Yöneticisi sunucusu yok

Bu hata aşağıdaki nedenlerle oluşur:

- Kurtarma Hizmetleri kasasına başka veri koruma yöneticisi/Azure Yedekleme sunucusu kayıtlı değildir.
- Sunucular henüz meta verileri yüklemedi.
- Seçili sunucu bir Veri Koruma Yöneticisi/Azure Yedekleme sunucusu değildir.

Diğer Veri Koruma Yöneticisi/Azure Yedekleme sunucuları Kurtarma Hizmetleri kasasına kaydolduğunda, sorunu gidermek için aşağıdaki adımları gerçekleştirin:

1. En son Azure Yedekleme aracısının yüklendiğinden emin olun.
2. En son aracının yüklendiğinden emin olduktan sonra, kurtarma işlemini başlatmadan önce bir gün bekleyin. Gece yedekleme işi, tüm korumalı yedeklemelerin meta verilerini buluta yükler. Yedekleme verileri daha sonra kurtarma için kullanılabilir.

## <a name="error-provided-encryption-passphrase-doesnt-match-passphrase-for-server"></a>Hata: Sağlanan şifreleme geçiş cümlesi sunucu için parola yla eşleşmiyor

Bu hata, Veri Koruma Yöneticisi/Azure Yedekleme sunucu verilerini kurtarırken şifreleme işlemi sırasında oluşur. Kurtarma işleminde kullanılan şifreleme parolası sunucunun şifreleme parolasıyla eşleşmez. Sonuç olarak, aracı verilerin şifresini çözemez ve kurtarma başarısız olur.

> [!IMPORTANT]
> Şifreleme parolasını unutur veya kaybederseniz, verileri kurtarmak için başka yöntem yoktur. Tek seçenek parolayı yeniden oluşturmaktır. Gelecekteki yedekleme verilerini şifrelemek için yeni parolayı kullanın.
>
> Verileri kurtarırken, her zaman Veri Koruma Yöneticisi/Azure Yedekleme sunucusuyla ilişkili aynı şifreleme parolasını sağlayın.
>
