---
title: Azure Site Kurtarma Hakkında
description: Bu sayfada, Azure Site Recovery hizmetine genel bir bakış sağlanmış ve olağanüstü durum kurtarma ile geçiş dağıtım senaryoları özetlenmiştir.
ms.topic: overview
ms.date: 03/17/2020
ms.custom: MVC
ms.openlocfilehash: ee387682e935522b37a5b4f9f32e53712bfb9150
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80067554"
---
# <a name="about-site-recovery"></a>Site Recovery Hakkında

Azure Site Recovery hizmetine hoş geldiniz! Bu makalede bir hizmet genel hatlarıyla kısaca ele alınmaktadır.

Kuruluş olarak, planlı ve planlanmamış kesintiler meydana geldiğinde verilerinizi ve uygulamalarınızı ve iş yüklerinizi çevrimiçi olarak güvende tutan bir iş sürekliliği ve olağanüstü durum kurtarma (BCDR) stratejisini benimsemeniz gerekir.

Azure Kurtarma Hizmetleri BCDR stratejinize katkıda bulunur:

- **Site Recovery hizmeti**: Site Recovery, kesintiler sırasında iş uygulamalarını ve iş yüklerini çalışır durumda tutarak iş sürekliliği sağlamaya yardımcı olur. Site Recovery, fiziksel ve sanal makineler (VM) üzerinde çalışan iş yüklerini birincil siteden ikincil bir konuma çoğaltır. Birincil sitenizde bir kesinti oluştuğunda ikinci konuma yük devredersiniz ve uygulamalara oradan erişirsiniz. Birincil konum tekrar çalışmaya başladıktan sonra o konuma geri dönebilirsiniz.
- **Yedekleme hizmeti**: [Azure Yedekleme](/azure/backup/) hizmeti verilerinizi güvenli ve kurtarılabilir tutar.

Site Recovery, şunlar için çoğaltmayı yönetebilir:

- Azure bölgeleri arasında çoğaltılan Azure VM’leri.
- Şirket içi VM'ler, Azure Stack VM'leri ve fiziksel sunucular.

## <a name="what-does-site-recovery-provide"></a>Site Recovery ne sağlar?

**Özellik** | **Şey**
--- | ---
**Basit BCDR çözümü** | Site Recovery hizmetini kullanarak Azure portalındaki tek bir konumdan çoğaltma, yük devretme ve yeniden çalışmayı yönetebilirsiniz.
**Azure VM çoğaltma** | Azure VM’lerin olağanüstü durum kurtarma özelliğini birincil bir bölgeden ikincil bölgeye ayarlayabilirsiniz.
**Şirket içi VM çoğaltma** | Şirket içi VM’leri ve fiziksel sunucuları Azure’a veya ikincil bir şirket içi veri merkezine çoğaltabilirsiniz. Azure’a çoğaltma seçeneği, ikincil bir veri merkezi kullanmanın maliyetini ve karmaşıklığını ortadan kaldırır.
**İş yükü çoğaltma** | Desteklenen Azure VM’lerinde, şirket içi Hyper-V ve VMware VM'leri ile Windows/Linux fiziksel sunucularında çalışan tüm iş yüklerini çoğaltın.
**Veri esnekliği** | Site Kurtarma, uygulama verilerini ele geçirmeden çoğaltmayı yönetir. Azure’a çoğaltılan veriler, dayanıklılık özelliği sunan Azure Depolama'da saklanır. Yük devretme işlemi gerçekleştiğinde, çoğaltılan veriler temel alınarak Azure VM'leri oluşturulur.
**RTO ve RPO hedefleri** | Kurtarma süresi hedeflerini (RTO) ve kurtarma noktası hedeflerini (RPO) kuruluş sınırları içinde tutun. Site Recovery, Azure VM’leri ve VMware VM’leri için sürekli çoğaltma sağlar ve Hyper-V için çoğaltma sıklığı 30 saniyeye kadar düşer. [Azure Traffic Manager](https://azure.microsoft.com/blog/reduce-rto-by-using-azure-traffic-manager-with-azure-site-recovery/) sayesinde, RTO’yu daha da düşürebilirsiniz.
**Yük devretme boyunca uygulamalarınızın tutarlı kalmasını sağlayın** | Uygulama içinde tutarlı anlık görüntülerle kurtarma noktalarını kullanarak çoğaltabilirsiniz. Bu anlık görüntüler, disk verilerini, bellekteki tüm verileri ve devam eden tüm işlemleri yakalar.
**Kesintisiz test etme** | Sürmekte olan çoğaltmayı etkilemeden kolayca olağanüstü durum kurtarma tatbikatları gerçekleştirebilirsiniz.
**Esnek yük devretme işlemleri** | Sıfır veri kaybıyla beklenen kesintiler için planlanmış kesintileri çalıştırabilirsiniz. Veya, beklenmeyen felaketler için çoğaltma sıklığına bağlı olarak en az veri kaybı yla planlanmamış arızalar. Yeniden kullanılabilir olduğunda, birincil sitenize kolayca geri dönebilirsiniz.
**Özelleştirilebilir kurtarma planları** | Kurtarma planlarını kullanarak, birden çok VM'de çalışan çok katmanlı uygulamaların başarısızlığını ve kurtarılmasını özelleştirebilir ve sıralayabilirsiniz. Bir kurtarma planında makineleri gruplayabilir ve isteğe bağlı olarak betikler ve el ile yapılan işlemler ekleyebilirsiniz. Kurtarma planları, Azure Otomasyonu runbook'ları ile tümleştirilebilir.
**BCDR tümleştirme** | Site Recovery, diğer BCDR teknolojileriyle tümleşir. Örneğin, kullanılabilir grupların yük devretme işlemlerini yönetmek amacıyla SQL Server AlwaysOn yerel desteği ile birlikte kurumsal iş yüklerinin SWL Server arka ucunu korumak için Site Recovery'yi kullanabilirsiniz.
**Azure otomasyonu tümleştirmesi** | Zengin Azure Otomasyonu kitaplığı, indirilebilen ve Site Recovery ile tümleştirilebilen üretime hazır ve uygulamaya özgü betikler sağlar.
**Ağ tümleştirmesi** | Site Kurtarma, uygulama ağı yönetimi için Azure ile tümleşir. Örneğin, IP adreslerini rezerve etmek için yük dengeleyicileri yapılandırın ve verimli ağ geçişleri için Azure Trafik Yöneticisi'ni kullanın.

## <a name="what-can-i-replicate"></a>Neleri çoğaltabilirim?

**Desteklenen** | **Şey**
--- | ---
**Çoğaltma senaryoları** | Azure VM’leri bir Azure bölgesinden diğerine çoğaltın.<br/><br/>  Şirket içi VMware VM'leri, Hyper-V VM'leri, fiziksel sunucuları (Windows ve Linux), Azure Stack VM'leri Azure'a kopyala.<br/><br/> AWS Windows örneklerini Azure'a çoğaltma.<br/><br/> Şirket içi VMware VM’leri, System Center VMM tarafından yönetilen Hyper-V VM’leri ve fiziksel sunucuları ikincil siteye çoğaltın.
**Bölgeler** | Site Recovery için [desteklenen bölgeleri](https://azure.microsoft.com/regions/services/) inceleyin. |
**Çoğaltılan makineler** | [Azure VM](azure-to-azure-support-matrix.md#replicated-machine-operating-systems) çoğaltma, [şirket içi VMware VM’ler ve fiziksel sunucular](vmware-physical-azure-support-matrix.md#replicated-machines) ve [şirket içi Hyper-V VM’lere](hyper-v-azure-support-matrix.md#replicated-vms) yönelik çoğaltma gereksinimlerini inceleyin.
**İş yükleri** | Çoğaltma için desteklenen bir makinede çalışan tüm iş yüklerini çoğaltabilirsiniz. Ayrıca, Site Kurtarma ekibi bir dizi [uygulama](site-recovery-workload.md#workload-summary)için uygulamaya özel testler yaptı.

## <a name="next-steps"></a>Sonraki adımlar

- [İş yükü desteği](site-recovery-workload.md) hakkında daha fazla bilgi edinin.
- [Bölgeler arasında Azure VM çoğaltma](azure-to-azure-quickstart.md) ile çalışmaya başlayın.
