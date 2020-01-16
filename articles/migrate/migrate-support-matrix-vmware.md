---
title: Azure geçişi 'nde VMware değerlendirmesi desteği
description: Azure geçişi 'nde VMware değerlendirmesi desteği hakkında bilgi edinin.
ms.topic: conceptual
ms.date: 01/08/2020
ms.openlocfilehash: 2a9c5504d99f439723a250b619b9f9b660ea9c59
ms.sourcegitcommit: dbcc4569fde1bebb9df0a3ab6d4d3ff7f806d486
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2020
ms.locfileid: "76029009"
---
# <a name="support-matrix-for-vmware-assessment"></a>VMware değerlendirmesi için destek matrisi 

Bu makalede, [Azure geçişi: Sunucu değerlendirmesi](migrate-services-overview.md#azure-migrate-server-migration-tool)Ile VMware VM 'lerini değerlendirmek için destek ayarları ve sınırlamaları özetlenmektedir. VMware VM 'lerini Azure 'a geçirme hakkında bilgi arıyorsanız, [geçiş desteği matrisini](migrate-support-matrix-vmware-migration.md)gözden geçirin.

## <a name="overview"></a>Genel Bakış

Bu makaleyle Azure 'a geçiş için şirket içi makineleri değerlendirmek üzere Azure geçişi: Sunucu değerlendirmesi aracını bir Azure geçiş projesine ekleyin. [Azure geçişi](migrate-appliance.md)gereci dağıtırsınız. Gereç, şirket içi makineleri sürekli olarak bulur ve Azure 'a yapılandırma ve performans verilerini gönderir. Makine bulma işleminden sonra keşfedilen makineleri gruplar halinde toplar ve bir grup için değerlendirme çalıştırırsınız.


## <a name="limitations"></a>Sınırlamalar

**Destek** | **Ayrıntılar**
--- | ---
**Değerlendirme limitleri**| Tek bir [projede](migrate-support-matrix.md#azure-migrate-projects)en fazla 35.000 VMware VM bulun ve değerlendirin.
**Proje limitleri** | Bir Azure aboneliğinde birden çok proje oluşturabilirsiniz. Bir proje, VMware VM 'leri, Hyper-V VM 'Leri ve fiziksel sunucuları, değerlendirme sınırlarına kadar içerebilir.
**Bulma** | Azure geçişi gereci vCenter Server en fazla 10.000 VMware VM 'yi bulabilir.
**Değerlendirme** | Tek bir gruba en fazla 35.000 makine ekleyebilirsiniz.<br/><br/> Tek bir değerlendirmede 35.000 adede kadar VM 'yi değerlendirebilirsiniz.

Değerlendirmeler hakkında [daha fazla bilgi edinin](concepts-assessment-calculation.md) .


## <a name="application-discovery"></a>Uygulama bulma

Azure geçişi: Sunucu değerlendirmesi, makineleri keşfetmenin yanı sıra makinelerde çalışan uygulamalar, rol ve özellikleri bulabilir. Uygulama envanterinizi keşfetmek, şirket içi iş yükleriniz için uyarlanmış bir geçiş yolu tanımlamanızı ve planlamanızı sağlar. 

**Destek** | **Ayrıntılar**
--- | ---
Keşif | Bulgu aracısız, makine konuk kimlik bilgileri kullanılarak ve WMI ve SSH çağrıları kullanılarak makinelere uzaktan erişim.
Desteklenen makineler | Şirket içi VMware VM 'Leri.
Makine işletim sistemi | Tüm Windows ve Linux sürümleri
Kimlik bilgileri | Şu anda tüm Windows sunucuları için bir kimlik bilgisi ve tüm Linux sunucuları için bir kimlik bilgisi kullanımını desteklemektedir.<br/><br/> Windows VM 'ler için bir Konuk Kullanıcı hesabı ve tüm Linux sanal makineleri için normal/normal kullanıcı hesabı (sudo erişimi olmayan) oluşturursunuz.
Sınırlar | Uygulama bulma için, her gereç için 10000 'e kadar keşfedebilirsiniz. 

## <a name="vmware-requirements"></a>VMware gereksinimleri

**VMware** | **Ayrıntılar**
--- | ---
**vCenter Server** | Bulmak ve değerlendirmek istediğiniz makineler vCenter Server sürüm 5,5, 6,0, 6,5 veya 6,7 tarafından yönetilmelidir.
**İzinler (değerlendirme)** | salt vCenter Server hesabı.
**İzinler (App-Discovery)** | salt okuma erişimi olan ve konuk Işlemleri > sanal makineler için etkinleştirilen ayrıcalıkların vCenter Server hesabı.
**İzinler (bağımlılık görselleştirme)** | Salt okuma erişimine sahip merkezi sunucu hesabı ve **sanal makineler** için etkinleştirilen ayrıcalıklar > **Konuk işlemleri**.


## <a name="azure-migrate-appliance-requirements"></a>Azure geçiş gereç gereksinimleri

Azure geçişi, bulma ve değerlendirme için [Azure geçişi](migrate-appliance.md) gereci kullanır. VMware için gereç, vCenter Server içeri aktarılan bir OVA şablonu kullanılarak dağıtılır. 

- VMware için [gereç gereksinimleri](migrate-appliance.md#appliance---vmware) hakkında bilgi edinin.
- Gerecin erişmesi gereken [URL 'ler](migrate-appliance.md#url-access) hakkında bilgi edinin.

## <a name="port-access"></a>Bağlantı noktası erişimi

**cihaz** | **bağlantı**
--- | ---
Elektrikli | TCP bağlantı noktası 3389 üzerindeki gelen bağlantılar, gereci Uzak Masaüstü bağlantılarına izin vermek için.<br/><br/> 44368 numaralı bağlantı noktası ile gereç yönetimi uygulamasına uzaktan erişim için gelen bağlantılar: ```https://<appliance-ip-or-name>:44368``` <br/><br/>Azure geçişi 'ne bulma ve performans meta verileri göndermek için 443, 5671 ve 5672 numaralı bağlantı noktası üzerinden giden bağlantılar.
vCenter server | TCP bağlantı noktası 443 ' deki gelen bağlantılar, gerecin değerlendirmeler için yapılandırma ve performans meta verilerini toplamasına izin verir. <br/><br/> Gereç, bağlantı noktası 443 ' de varsayılan olarak vCenter 'a bağlanır. VCenter sunucusu farklı bir bağlantı noktasını dinliyorsa, bulmayı ayarlarken bağlantı noktasını değiştirebilirsiniz.

## <a name="dependency-visualization"></a>Bağımlılık görselleştirme

Bağımlılık görselleştirme, değerlendirmek ve geçirmek istediğiniz makineler arasında bağımlılıkları görselleştirmenize yardımcı olur. Daha yüksek düzeyde güvenle makineler değerlendirmek istediğinizde genellikle bağımlılık eşlemesini kullanırsınız. VMware VM 'Leri için, bağımlılık görselleştirmesi aşağıdaki gibi desteklenir:

- **Aracısız bağımlılık görselleştirmesi**: Bu seçenek şu anda önizleme aşamasındadır. Makinelere herhangi bir aracı yüklemenizi gerektirmez.
    - Etkin olduğu makinelerden gelen TCP bağlantısı verilerini yakalayıp işe yarar. Bağımlılık keşfi başlatıldıktan sonra, Gereç, beş dakikalık yoklama aralığındaki makinelerden veri toplar.
    - Aşağıdaki veriler toplanır:
        - TCP bağlantıları
        - Etkin bağlantıları olan işlemlerin adları
        - Yukarıdaki işlemlerin çalıştırıldığı yüklü uygulamaların adları
        - Hayır. her yoklama aralığında algılanan bağlantıların sayısı
- **Aracı tabanlı bağımlılık görselleştirmesi**: aracı tabanlı bağımlılık görselleştirmesini kullanmak için, çözümlemek istediğiniz her şirket içi makineye aşağıdaki aracıları indirmeniz ve yüklemeniz gerekir.
    - Her makineye Microsoft Monitoring Agent 'ı (MMA) yükler. MMA aracısının nasıl yükleneceği hakkında [daha fazla bilgi edinin](how-to-create-group-machine-dependencies.md#install-the-mma) .
    - Her makineye bağımlılık aracısını yükler. Bağımlılık aracısının nasıl yükleneceği hakkında [daha fazla bilgi edinin](how-to-create-group-machine-dependencies.md#install-the-dependency-agent) .
    - Ayrıca, İnternet bağlantısı olmayan makineleriniz varsa, bu makinelere Log Analytics ağ geçidini indirip yüklemeniz gerekir.


## <a name="next-steps"></a>Sonraki adımlar

- Değerlendirme oluşturmak için en iyi uygulamaları [gözden geçirin](best-practices-assessment.md) .
- [VMware değerlendirmesi Için hazırlanma](tutorial-prepare-vmware.md) .
