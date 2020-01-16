---
title: Azure geçişi ile fiziksel sunucu değerlendirmesi desteği
description: Azure geçişi ile fiziksel sunucu değerlendirmesi desteği hakkında bilgi edinin.
ms.topic: conceptual
ms.date: 01/08/2020
ms.openlocfilehash: 32080605217cde78bd648ca6192f73d1025dea4c
ms.sourcegitcommit: dbcc4569fde1bebb9df0a3ab6d4d3ff7f806d486
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2020
ms.locfileid: "76028767"
---
# <a name="support-matrix-for-physical-server-assessment"></a>Fiziksel sunucu değerlendirmesi için destek matrisi 

Makineleri değerlendirmek ve Microsoft Azure buluta geçirmek için [Azure geçişi hizmetini](migrate-overview.md) kullanabilirsiniz. Bu makalede, şirket içi fiziksel sunucuları değerlendirmek ve geçirmek için destek ayarları ve sınırlamaları özetlenmektedir.


## <a name="overview"></a>Genel Bakış

Bu makaleyle Azure 'a geçiş için şirket içi makineleri değerlendirmek üzere Azure geçişi: Sunucu değerlendirmesi aracını bir Azure geçiş projesine ekleyin. [Azure geçişi](migrate-appliance.md)gereci dağıtırsınız. Gereç, şirket içi makineleri sürekli olarak bulur ve Azure 'a yapılandırma ve performans verilerini gönderir. Makine bulma işleminden sonra keşfedilen makineleri gruplar halinde toplar ve grup için bir değerlendirme çalıştırırsınız

## <a name="limitations"></a>Sınırlamalar

**Destek** | **Ayrıntılar**
--- | ---
**Değerlendirme limitleri**| Tek bir [projede](migrate-support-matrix.md#azure-migrate-projects)en fazla 35.000 fiziksel sunucuyu bulur ve değerlendirin.
**Proje limitleri** | Bir Azure aboneliğinde birden çok proje oluşturabilirsiniz. Bir proje, VMware VM 'leri, Hyper-V VM 'Leri ve fiziksel sunucuları, değerlendirme sınırlarına kadar içerebilir.
**Bulma** | Azure geçişi gereci en fazla 250 fiziksel sunucu bulabilir.
**Değerlendirme** | Tek bir gruba en fazla 35.000 makine ekleyebilirsiniz.<br/><br/> Tek bir değerlendirmede en fazla 35.000 makine değerlendirebilirsiniz.

Değerlendirmeler hakkında [daha fazla bilgi edinin](concepts-assessment-calculation.md) .




## <a name="physical-server-requirements"></a>Fiziksel sunucu gereksinimleri

| **Destek**                | **Ayrıntılar**               
| :-------------------       | :------------------- |
| **Fiziksel sunucu dağıtımı**       | Fiziksel sunucu tek başına olabilir veya bir kümede dağıtılabilir. |
| **İzinler**           | **Windows:** Bulmaya dahil etmek istediğiniz tüm Windows sunucularında yerel bir kullanıcı hesabı ayarlayın. Kullanıcı hesabının bu gruplara eklenmesi gerekir-uzak masaüstü kullanıcıları, performans Izleyicisi kullanıcıları ve performans günlüğü kullanıcıları. <br/> **Linux:** Bulunmasını istediğiniz Linux sunucularında bir kök hesabınız olması gerekir. |
| **İşletim sistemi** | Aşağıdakiler dışında tüm [Windows](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines) ve [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) işletim sistemleri desteklenir:<br/> Windows Server 2003 <br/> SUSE Linux|


## <a name="azure-migrate-appliance-requirements"></a>Azure geçiş gereç gereksinimleri

Azure geçişi, bulma ve değerlendirme için [Azure geçişi](migrate-appliance.md) gereci kullanır. Fiziksel sunucular için gereç, bir VM 'de veya fiziksel bir makinede çalıştırılabilir. Bu ayarı, Azure portal indirtiğiniz bir PowerShell betiği kullanarak ayarlarsınız.

- Fiziksel sunucular için [gereç gereksinimleri](migrate-appliance.md#appliance---physical) hakkında bilgi edinin.
- Gerecin erişmesi gereken [URL 'ler](migrate-appliance.md#url-access) hakkında bilgi edinin.

## <a name="port-access"></a>Bağlantı noktası erişimi

Aşağıdaki tabloda, değerlendirme için bağlantı noktası gereksinimleri özetlenmektedir.

**cihaz** | **bağlantı**
--- | ---
**Elektrikli** | TCP bağlantı noktası 3389 üzerindeki gelen bağlantılar, gereci Uzak Masaüstü bağlantılarına izin vermek için.<br/> 44368 numaralı bağlantı noktası ile gereç yönetimi uygulamasına uzaktan erişim için gelen bağlantılar: ``` https://<appliance-ip-or-name>:44368 ```<br/> Azure geçişi 'ne bulma ve performans meta verileri göndermek için 443, 5671 ve 5672 bağlantı noktalarında giden bağlantılar.
**Fiziksel sunucular** | **Windows:** Windows sunucularından yapılandırma ve performans meta verilerini çekmek için 443 numaralı bağlantı noktası, WinRM bağlantı noktaları 5985 (HTTP) ve 5986 (HTTPS) üzerinde gelen bağlantılar. <br/> **Linux:**  22 (UDP) bağlantı noktasındaki, Linux sunucularından yapılandırma ve performans meta verileri çekmek için gelen bağlantılar. |


## <a name="next-steps"></a>Sonraki adımlar

[Fiziksel sunucu değerlendirmesi Için hazırlanma](tutorial-prepare-physical.md).
