---
title: Azure Site Kurtarma ile Hyper-V VM'lerin Azure'a olağanüstü kurtarma sına hazırlanın
description: Azure Site Kurtarma ile Azure'da olağanüstü durum kurtarma için şirket içi Hyper-V VM'leri nasıl hazırlayacağınızı öğrenin.
author: rayne-wiselman
ms.service: site-recovery
ms.topic: tutorial
ms.date: 11/12/2019
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: b36e4688ebd6e929a56869a6bb191d98b2f1f432
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79239843"
---
# <a name="prepare-on-premises-hyper-v-servers-for-disaster-recovery-to-azure"></a>Şirket içi Hyper-V sunucularını Azure’a olağanüstü durum kurtarma için hazırlama

Bu makalede, [Azure Site Kurtarma'yı](site-recovery-overview.md)kullanarak Hyper-VM'lerin Azure'a olağanüstü kurtarma sını ayarlamak istediğinizde şirket içi Hyper-V altyapınızı nasıl hazırlayacağınızı açıklar.


Bu, şirket içi Hyper-V V MM'ler için Azure'da olağanüstü durum kurtarma yı nasıl ayarladığınızı gösteren bir serinin ikinci öğreticisidir. İlk eğitimde, Hyper-V olağanüstü durum kurtarma için gereken [Azure bileşenlerini kurduk.](tutorial-prepare-azure.md)

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Hyper-V ana bilgisayarlarınız Sistem Merkezi VMM tarafından yönetiliyorsa Hyper-V gereksinimlerini ve VMM gereksinimlerini gözden geçirin.
> * Varsa VMM hazırlayın.
> * Azure konumlarına internet erişimini doğrulayın.
> * Azure'a geçtikten sonra bunlara erişebilmeniz için VM'ler hazırlayın.

> [!NOTE]
> Öğreticiler, bir senaryo için en basit dağıtım yolunu gösterir. Mümkün olduğunca varsayılan seçenekleri kullanır ve tüm olası ayarları ve yolları göstermez. Ayrıntılı talimatlar için, Site Kurtarma İçeriği Tablosunun Nasıl Yapılacağı bölümündeki makaleyi inceleyin.

## <a name="before-you-start"></a>Başlamadan önce

[Azure'u bu serinin ilk öğreticisinde](tutorial-prepare-azure.md)açıklandığı şekilde hazırladığınıza emin olun.

## <a name="review-requirements-and-prerequisites"></a>Gereksinimleri ve ön koşulları gözden geçirme

Hyper-V ana bilgisayarları ve VM'lerin gereksinimlere uyduğunu unutmayın.

1. Şirket içi sunucu gereksinimlerini [doğrulayın.](hyper-v-azure-support-matrix.md#on-premises-servers)
2. Azure'da çoğaltmak istediğiniz Hyper-V VM'lerin [gereksinimlerini denetleyin.](hyper-v-azure-support-matrix.md#replicated-vms)
3. Hyper-V ana bilgisayar ağlarını kontrol [edin;](hyper-v-azure-support-matrix.md#hyper-v-network-configuration) ve şirket içi Hyper-V ana bilgisayarları için ev sahibi ve konuk [depolama](hyper-v-azure-support-matrix.md#hyper-v-host-storage) desteği.
4. Yük devretmenin ardından [Azure ağ](hyper-v-azure-support-matrix.md#azure-vm-network-configuration-after-failover), [depolama](hyper-v-azure-support-matrix.md#azure-storage) ve [işlem](hyper-v-azure-support-matrix.md#azure-compute-features) için nelerin desteklendiğini denetleyin.
5. Azure’a çoğalttığınız şirket içi sanal makineleriniz, [Azure sanal makinesi gereksinimleri](hyper-v-azure-support-matrix.md#azure-vm-requirements) ile uyumlu olmalıdır.


## <a name="prepare-vmm-optional"></a>VMM hazırlama (isteğe bağlı)

Hyper-V ana bilgisayarları VMM tarafından yönetiliyorsa, şirket içi VMM sunucusunu hazırlamanız gerekir. 

- VMM sunucusunun bir veya daha fazla ana bilgisayar grubuyla en az bir buluta sahip olduğundan emin olun. VM'lerin çalıştırıldığı Hyper-V ana bilgisayar bulutta bulunmalıdır.
- VMM sunucusunu ağ eşleme için hazırlayın.

### <a name="prepare-vmm-for-network-mapping"></a>VMM'yi ağ eşleme için hazırlayın

VMM kullanıyorsanız, şirket içi VMM VM ağları ve Azure sanal ağları arasındaki [ağ eşleme haritaları.](site-recovery-network-mapping.md) Eşleme, Azure VM'lerinin başarısız olduktan sonra oluşturulduklarında doğru ağa bağlanmasını sağlar.

VMM'yi ağ eşleme için aşağıdaki şekilde hazırlayın:

1. Hyper-V ana bilgisayarlarının bulunduğu bulutla ilişkili bir [VMM mantıksal ağınız](https://docs.microsoft.com/system-center/vmm/network-logical) olduğundan emin olun.
2. Mantıksal ağa bağlı bir [VM ağınız](https://docs.microsoft.com/system-center/vmm/network-virtual) olduğundan emin olun.
3. VMM'de VM'leri VM ağına bağlayın.

## <a name="verify-internet-access"></a>İnternet erişimini doğrulama

1. Öğretici amaçları için, en basit yapılandırma Hyper-V ana bilgisayarları ve VMM sunucusu için bir proxy kullanmadan internete doğrudan erişim olmasıiçindir. 
2. Hyper-V ana bilgisayarlarının ve ilgiliyse VMM sunucusunun aşağıdaki gerekli URL'lere erişebileceğinden emin olun.   
3. IP adresine göre erişimi kontrol ediyorsanız, şundan emin olun:
    - IP adresi tabanlı güvenlik duvarı kuralları [Azure Veri Merkezi IP Aralıkları'na](https://www.microsoft.com/download/confirmation.aspx?id=41653)ve HTTPS (443) bağlantı noktasına bağlanabilir.
    - Aboneliğinizin Azure bölgesi için IP adres aralıklarına izin verin.
    
### <a name="required-urls"></a>Gerekli URL'ler


[!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]


## <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Yük devretmeden sonra Azure VM'lerine bağlanmak için hazırlık yapma

Bir başarısız senaryo sırasında çoğaltılan şirket içi ağınıza bağlanmak isteyebilirsiniz.

Başarısız olduktan sonra RDP kullanarak Windows VM'lerine bağlanmak için aşağıdaki gibi erişime izin verin:

1. İnternet üzerinden erişmek için, yük devretmeden önce şirket içi VM’de RDP’yi etkinleştirin. **Genel** profil için TCP ve UDP kurallarının eklenmiştir ve tüm profiller için **Windows Güvenlik Duvarı** > **İzin Verilen Uygulamalar'da** RDP'ye izin verilir.
2. Siteden siteye VPN üzerinden erişmek için, şirket içi makinede RDP’yi etkinleştirin. RDP'ye Etki Alanı ve Özel ağlar için **Windows Güvenlik Duvarı** -> İzin Verilen **uygulamalarda ve** **özelliklerde** izin verilmelidir.
   İşletim sisteminin SAN ilkesinin **OnlineAll** olarak ayarlandığından emin olun. [Daha fazla bilgi edinin](https://support.microsoft.com/kb/3031135). Bir yük devretme tetiklediğinizde VM’de bekleyen Windows güncelleştirmelerinin olmaması gerekir. Varsa, güncelleştirme tamamlanana kadar sanal makinede oturum açamazsınız.
3. Yük devretmeden sonra Windows Azure VM’sinde, VM’nin bir ekran görüntüsünü görmek için **Önyükleme tanılaması**’nı kontrol edin. Bağlanamıyorsanız, VM’nin çalıştığından emin olun ve şu [sorun giderme ipuçlarını](https://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx) gözden geçirin.

Başarısız olduktan sonra, azure vm'lere, çoğaltılan şirket içi VM ile aynı IP adresini veya farklı bir IP adresini kullanarak erişebilirsiniz. Başarısız olmak için IP adresleme ayarlama hakkında [daha fazla bilgi edinin.](concepts-on-premises-to-azure-networking.md)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Hyper-V VM'ler](tutorial-hyper-v-to-azure.md)
> için Azure'a olağanüstü durum kurtarma ayarlama[VMM bulutlarında Hyper-V VM'ler için Azure'a olağanüstü durum kurtarma ayarlama](tutorial-hyper-v-vmm-to-azure.md)
