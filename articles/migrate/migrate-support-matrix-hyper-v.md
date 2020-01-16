---
title: Azure geçiş 'de Hyper-V değerlendirmesi desteği
description: Azure geçişi ile Hyper-V değerlendirmesi desteği hakkında bilgi edinin.
ms.topic: conceptual
ms.date: 01/08/2020
ms.openlocfilehash: 20bdbb16d2f0610f6519424141b09190eae3cc42
ms.sourcegitcommit: dbcc4569fde1bebb9df0a3ab6d4d3ff7f806d486
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2020
ms.locfileid: "76028794"
---
# <a name="support-matrix-for-hyper-v-assessment"></a>Hyper-V değerlendirmesi için destek matrisi

Bu makalede, Hyper-V VM 'lerini [Azure geçişi: Sunucu değerlendirmesi](migrate-services-overview.md#azure-migrate-server-assessment-tool) ile değerlendirmek için destek ayarları ve sınırlamaları özetlenmektedir. Hyper-V VM 'lerini Azure 'a geçirme hakkında bilgi arıyorsanız, [geçiş desteği matrisini](migrate-support-matrix-hyper-v-migration.md)gözden geçirin.

## <a name="overview"></a>Genel Bakış

Bu makaleyle Azure 'a geçiş için şirket içi makineleri değerlendirmek üzere Azure geçişi: Sunucu değerlendirmesi aracını bir Azure geçiş projesine ekleyin. [Azure geçişi](migrate-appliance.md)gereci dağıtırsınız. Gereç, şirket içi makineleri sürekli olarak bulur ve Azure 'a yapılandırma ve performans verilerini gönderir. Makine bulma işleminden sonra keşfedilen makineleri gruplar halinde toplar ve bir grup için değerlendirme çalıştırırsınız.


## <a name="limitations"></a>Sınırlamalar

**Destek** | **Ayrıntılar**
--- | ---
**Değerlendirme limitleri**| Tek bir [projedeki](migrate-support-matrix.md#azure-migrate-projects)35.000 adede kadar Hyper-V VM 'yi bulur ve değerlendirin.
**Proje limitleri** | Bir Azure aboneliğinde birden çok proje oluşturabilirsiniz. Bir proje, VMware VM 'leri, Hyper-V VM 'Leri ve fiziksel sunucuları, değerlendirme sınırlarına kadar içerebilir.
**Bulma** | Azure geçişi gereci en fazla 5000 Hyper-V VM 'yi bulabilir.<br/><br/> Gereç, 300 adede kadar Hyper-V konaklarına bağlanabilir.
**Değerlendirme** | Tek bir gruba en fazla 35.000 makine ekleyebilirsiniz.<br/><br/> Tek bir değerlendirmede 35.000 adede kadar VM 'yi değerlendirebilirsiniz.

Değerlendirmeler hakkında [daha fazla bilgi edinin](concepts-assessment-calculation.md) .



## <a name="hyper-v-host-requirements"></a>Hyper-V konağı gereksinimleri

| **Destek**                | **Ayrıntılar**               
| :-------------------       | :------------------- |
| **Konak dağıtımı**       | Hyper-V konağı tek başına olabilir veya bir kümede dağıtılabilir. |
| **İzinler**           | Hyper-V konağında yönetici izinlerine sahip olmanız gerekir. <br/> Alternatif olarak, yönetici izinleri atamak istemiyorsanız, bir yerel veya etki alanı kullanıcı hesabı oluşturun ve kullanıcıyı bu gruplara (uzak yönetim kullanıcıları, Hyper-V yöneticileri ve performans Izleyicisi kullanıcıları) ekleyin. |
| **Konak işletim sistemi** | Windows Server 2019, Windows Server 2016 veya Windows Server 2012 R2.<br/> Windows Server 2012 çalıştıran Hyper-V konaklarında yer alan VM'leri değerlendiremezsiniz. |
| **PowerShell uzaktan Iletişim**   | Her konakta etkin olmalıdır. |
| **Hyper-V çoğaltma**       | Hyper-V çoğaltması kullanıyorsanız (veya aynı VM tanımlayıcılarına sahip birden fazla sanal makine varsa) Azure geçişi 'ni kullanarak hem özgün hem de çoğaltılan VM 'Leri keşfetiyorsanız, Azure geçişi tarafından oluşturulan değerlendirme doğru olmayabilir. |


## <a name="hyper-v-vm-requirements"></a>Hyper-V VM gereksinimleri

| **Destek**                  | **Ayrıntılar**               
| :----------------------------- | :------------------- |
| **İşletim sistemi** | Azure tarafından desteklenen tüm [Windows](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines) ve [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) işletim sistemleri. |
| **Tümleştirme Hizmetleri**       | [Hyper-V tümleştirme hizmetlerinin](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/integration-services) , işletim sistemi bilgilerini yakalamak için değerlendirmekte olduğunuz VM 'lerde çalışıyor olması gerekir. |


## <a name="azure-migrate-appliance-requirements"></a>Azure geçiş gereç gereksinimleri

Azure geçişi, bulma ve değerlendirme için [Azure geçişi](migrate-appliance.md) gereci kullanır. Hyper-V için gereç bir Hyper-V VM üzerinde çalışır ve Azure portal indirdiğinizde, sıkıştırılmış bir Hyper-V VHD kullanılarak dağıtılır. 

- Hyper-V için [gereç gereksinimleri](migrate-appliance.md#appliance---hyper-v) hakkında bilgi edinin.
- Gerecin erişmesi gereken [URL 'ler](migrate-appliance.md#url-access) hakkında bilgi edinin.

## <a name="port-access"></a>Bağlantı noktası erişimi

Aşağıdaki tabloda, değerlendirme için bağlantı noktası gereksinimleri özetlenmektedir.

**cihaz** | **bağlantı**
--- | ---
**Elektrikli** | TCP bağlantı noktası 3389 üzerindeki gelen bağlantılar, gereci Uzak Masaüstü bağlantılarına izin vermek için.<br/> 44368 numaralı bağlantı noktası ile gereç yönetimi uygulamasına uzaktan erişim için gelen bağlantılar: ``` https://<appliance-ip-or-name>:44368 ```<br/> Azure geçişi 'ne bulma ve performans meta verileri göndermek için 443, 5671 ve 5672 bağlantı noktalarında giden bağlantılar.
**Hyper-V konağı/kümesi** | Genel Bilgi Modeli (CıM) oturumu kullanarak Hyper-V VM 'lerinin yapılandırma ve performans meta verilerini çekmek için WinRM bağlantı noktaları 5985 (HTTP) ve 5986 (HTTPS) üzerinde gelen bağlantılar.



## <a name="next-steps"></a>Sonraki adımlar

[Hyper-V VM değerlendirmesi için hazırlanma](tutorial-prepare-hyper-v.md)
