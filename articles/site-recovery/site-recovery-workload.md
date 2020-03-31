---
title: Azure Site Kurtarma ile şirket içi uygulamalar için olağanüstü durum kurtarma hakkında
description: Azure Site Recovery hizmeti ile olağanüstü durum kurtarma kullanılarak korunabilen iş yüklerini açıklar.
ms.topic: conceptual
ms.date: 03/18/2020
ms.openlocfilehash: 2b901425a0020c0ccc7b834ee36d965910028018
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80062833"
---
# <a name="about-disaster-recovery-for-on-premises-apps"></a>Şirket içi uygulamalar için olağanüstü durum kurtarma hakkında

Bu makalede, [Azure Site Kurtarma](site-recovery-overview.md) hizmetiyle olağanüstü durum kurtarma için koruyabileceğiniz şirket içi iş yükleri ve uygulamalar açıklanmaktadır.

## <a name="overview"></a>Genel Bakış

Kuruluşların, iş yüklerini ve verileri güvenli ve planlı ve planlanmamış kapalı kalma sürelerinde kullanılabilir tutmak için bir iş sürekliliği ve olağanüstü durum kurtarma (BCDR) stratejisine ihtiyaçları vardır. Ve, düzenli çalışma koşullarına kurtarmak.

Site Recovery, BCDR stratejinize katkıda bulunan bir Azure hizmetidir. Site Recovery’yi kullanarak, buluta veya ikincil bir siteye uygulamayla tutarlı çoğaltma dağıtabilir. Çoğaltmayönetmek, olağanüstü durum kurtarma testi gerçekleştirmek ve başarısız lıkları ve geri dönüşleri çalıştırmak için Site Kurtarma'yı kullanabilirsiniz. Uygulamalarınız Windows veya Linux tabanlı bilgisayarlarda, fiziksel sunucularda, VMware'de veya Hyper-V'de çalıştırılabilir.

Site Kurtarma, SharePoint, Exchange, Dynamics, SQL Server ve Active Directory gibi Microsoft uygulamalarıyla tümleşir. Microsoft, Oracle, SAP ve Red Hat gibi önde gelen satıcılarla yakın çalışır. Çoğaltma çözümlerini her uygulama için ayrı olarak özelleştirebilirsiniz.

## <a name="why-use-site-recovery-for-application-replication"></a>Uygulama çoğaltma için neden Site Recovery'yi kullanmam gerekir?

Site Recovery, uygulama düzeyinde koruma ve kurtarmaya şu yollarla katkıda bulunur:

- App-agnostik ve desteklenen bir makinede çalışan tüm iş yükleri için çoğaltma sağlar.
- En kritik iş uygulamalarının gereksinimlerini karşılamak için 30 saniyegibi düşük kurtarma noktası hedefleri (RPO) ile yakın eşzamanlı çoğaltma.
- Tek veya çok katmanlı uygulamalar için uygulamayla tutarlı anlık görüntüler.
- SQL Server AlwaysOn ile tümleştirme ve diğer uygulama düzeyinde çoğaltma teknolojileri ile ortaklık. Örneğin, Active Directory çoğaltma, SQL AlwaysOn ve Exchange Veritabanı Kullanılabilirlik Grupları (DAGs).
- Tek bir tıklamayla tüm uygulama yığınını kurtarmanızı ve harici komut dosyalarını ve el ile eylemleri plana eklemenizi sağlayan esnek kurtarma planları.
- Uygulama ağı gereksinimlerini basitleştirmek için Site Kurtarma ve Azure'da gelişmiş ağ yönetimi. Düşük kurtarma süresi hedefleri (RTO) ağ geçişleri için IP adreslerini rezerve etme, yük dengelemeyi yapılandırma ve Azure Trafik Yöneticisi ile tümleştirme gibi ağ yönetimi.
- İndirilebilen ve kurtarma planlarıyla tümleştirilebilen üretime hazır ve uygulamaya özgü betikler sağlayan zengin bir otomasyon kitaplığı.

## <a name="workload-summary"></a>İş yükü özeti

Site Recovery, desteklenen bir makinede çalışan herhangi bir uygulamayı çoğaltabilir. Aşağıdaki tabloda belirtilen uygulamalar için ek testler yapmak için ürün ekipleriyle ortaklık yaptık.

| **İş yükü** |**Azure VM’lerini Azure’a çoğaltma** |**Hyper-V VM'lerini ikincil bir siteye çoğaltma** | **Hyper-V VM'lerini Azure'a çoğaltma** | **VMware VM'lerini ikincil bir siteye çoğaltma** | **VMware VM'lerini Azure'a çoğaltma** |
| --- | --- | --- | --- | --- |---|
| Active Directory, DNS |Evet |Evet |Evet |Evet |Evet|
| Web uygulamaları (IIS, SQL) |Evet |Evet |Evet |Evet |Evet|
| System Center Operations Manager |Evet |Evet |Evet |Evet |Evet|
| SharePoint |Evet |Evet |Evet |Evet |Evet|
| SAP<br/><br/>Küme olmayan seçenekler için SAP sitesini Azure'a çoğaltma |Evet (Microsoft tarafından test edilmiştir) |Evet (Microsoft tarafından test edilmiştir) |Evet (Microsoft tarafından test edilmiştir) |Evet (Microsoft tarafından test edilmiştir) |Evet (Microsoft tarafından test edilmiştir)|
| Exchange (DAG olmayan) |Evet |Evet |Evet |Evet |Evet|
| Uzak Masaüstü/VDI |Evet |Evet |Evet |Evet |Evet|
| Linux (işletim sistemi ve uygulamalar) |Evet (Microsoft tarafından test edilmiştir) |Evet (Microsoft tarafından test edilmiştir) |Evet (Microsoft tarafından test edilmiştir) |Evet (Microsoft tarafından test edilmiştir) |Evet (Microsoft tarafından test edilmiştir)|
| Dynamics AX |Evet |Evet |Evet |Evet |Evet|
| Windows Dosya Sunucusu |Evet |Evet |Evet |Evet |Evet|
| Citrix XenApp ve XenDesktop |Evet|Yok |Evet |Yok |Evet |

## <a name="replicate-active-directory-and-dns"></a>Active Directory'yi ve DNS'yi çoğaltma

Çoğu kurumsal uygulama için Active Directory ve DNS gereklidir. Olağanüstü durum kurtarma sırasında, iş yüklerini ve uygulamaları kurtarmadan önce bu altyapı bileşenlerini korumanız ve kurtarmanız gerekir.

Site Recovery'yi kullanarak Active Directory ve DNS için tamamen otomatik bir olağanüstü durum kurtarma planı oluşturabilirsiniz. Örneğin, SharePoint ve SAP üzerinden birincil bir ikincil siteye başarısız olmak için, ilk Active Directory üzerinde başarısız bir kurtarma planı ayarlayabilirsiniz. Ardından, Active Directory'ye dayanan diğer uygulamalarda başarısız olmak için uygulamaya özel ek bir kurtarma planı kullanın.

Active Directory ve DNS için olağanüstü durum kurtarma hakkında [daha fazla bilgi edinin.](site-recovery-active-directory.md)

## <a name="protect-sql-server"></a>SQL Server'ı koruma

SQL Server, şirket içi veri merkezinde birçok iş uygulaması için bir veri hizmetleri temeli sağlar. Site Kurtarma, SQL Server kullanan çok katmanlı kurumsal uygulamaları korumak için SQL Server HA/DR teknolojileri ile kullanılabilir.

Site Recovery şunları sağlar:

- SQL Server için basit ve ekonomik bir olağanüstü durum kurtarma çözümü. SQL Server tek başına sunucularının ve kümelerinin birden çok sürümünü Azure'a veya ikincil bir siteye çoğaltma olanağı.
- Azure Site Recovery kurtarma planları ile yük devretme ve yeniden çalışma gibi işlemleri yönetmek için SQL AlwaysOn Kullanılabilirlik Grupları ile tümleştirme olanağı.
- SQL Server veritabanları dahil olmak üzere bir uygulamadaki tüm katmanlara yönelik uçtan uca kurtarma planları.
- Sql Server'ı Site Kurtarma ile en yüksek yükler için ölçekleme, Azure'da daha büyük IaaS sanal makine boyutlarına _dönüştürerek._
- Kolay SQL Server olağanüstü durum kurtarma testi Üretim ortamınızı etkilemeden uyumluluk denetimlerini çalıştırmak ve verileri analiz etmek için test amaçlı yük devretme işlemini çalıştırabilirsiniz.

SQL sunucusu için olağanüstü durum kurtarma hakkında [daha fazla bilgi edinin.](site-recovery-sql.md)

## <a name="protect-sharepoint"></a>SharePoint'i koruma

Azure Site Recovery, SharePoint dağıtımlarının korunmasına şu şekilde yardımcı olur:

- Olağanüstü durum kurtarma için yedek bir gruba olan ihtiyacı ve bununla ilgili altyapı maliyetini ortadan kaldırır. Tüm çiftliği (web, uygulama ve veritabanı katmanları) Azure'da veya ikincil bir siteye çoğaltmak için Site Kurtarma'yı kullanın.
- Uygulama dağıtımını ve yönetimini basitleştirir. Birincil siteye dağıtılan güncelleştirmeler otomatik olarak çoğaltılır. Güncelleştirmeler, ikincil bir sitedeki bir çiftliğin başarısızlığı ve kurtarılmasından sonra kullanılabilir. Stand-by çiftliğinin güncel tutulmasıyla ilişkili yönetim karmaşıklığını ve maliyetlerini düşürür.
- Test ve hata ayıklama işlemleri için üretim benzeri isteğe bağlı bir kopya çoğaltma ortamı oluşturarak SharePoint uygulaması için geliştirme sürecini basitleştirir.
- SharePoint dağıtımlarını Azure'a geçirmek için Site Recovery'yi kullanarak buluta geçişi basitleştirir.

SharePoint için olağanüstü durum kurtarma hakkında [daha fazla bilgi edinin.](site-recovery-sharepoint.md)

## <a name="protect-dynamics-ax"></a>Dynamics AX'i koruma

Azure Site Recovery, Dynamics AX ERP çözümünüzün korunmasına şu şekillerde yardımcı olur:

- Tüm Dynamics AX ortamınızın (Web ve AOS katmanları, veritabanı katmanları, SharePoint) Azure'da veya ikincil bir siteye çoğaltılmasını yönetme.
- Dynamics AX dağıtımlarının buluta (Azure'a) geçişini basitleştirir.
- Test ve hata ayıklama işlemleri için üretim benzeri isteğe bağlı bir kopya oluşturarak Dynamics AX uygulamasının geliştirme ve test sürecini basitleştirir.

Dinamik AX için olağanüstü durum kurtarma hakkında [daha fazla bilgi edinin.](site-recovery-dynamicsax.md)

## <a name="protect-remote-desktop-services"></a>Uzak Masaüstü Hizmetlerini Koruyun

Uzak Masaüstü Hizmetleri (RDS), kullanıcıların her yerde çalışmasını sağlayan sanal masaüstü altyapısına (VDI), oturum tabanlı masaüstü bilgisayarlara ve uygulamalara olanak tanır.

Azure Site Kurtarma ile aşağıdaki hizmetleri çoğaltabilirsiniz:

- Yönetilen veya yönetilmeyen birleştirilmiş sanal masaüstü bilgisayarları ikincil bir siteye çoğaltma.
- Uzak uygulamaları ve oturumları ikincil bir siteye veya Azure'da çoğaltın.

Aşağıdaki tabloçoğaltma seçeneklerini gösterir:

| **RDS** |**Azure VM’lerini Azure’a çoğaltma** | **Hyper-V VM'lerini ikincil bir siteye çoğaltma** | **Hyper-V VM'lerini Azure'a çoğaltma** | **VMware VM'lerini ikincil bir siteye çoğaltma** | **VMware VM'lerini Azure'a çoğaltma** | **Fiziksel sunucuları ikincil bir siteye çoğaltma** | **Fiziksel sunucuları Azure'a çoğaltma** |
|---| --- | --- | --- | --- | --- | --- | --- |
| **Havuza Alınmış Sanal Masaüstü (yönetilmeyen)** |Hayır|Evet |Hayır |Evet |Hayır |Evet |Hayır |
| **Havuza Alınmış Sanal Masaüstü (yönetilen ve UPD'siz)** |Hayır|Evet |Hayır |Evet |Hayır |Evet |Hayır |
| **Uzak uygulamalar ve Masaüstü oturumları (UPD'siz)** |Evet|Evet |Evet |Evet |Evet |Evet |Evet |

RDS için olağanüstü durum kurtarma hakkında [daha fazla bilgi edinin.](/windows-server/remote/remote-desktop-services/rds-disaster-recovery-with-azure)

## <a name="protect-exchange"></a>Exchange'i koruma

Site Recovery, Exchange'in korunmasına şu şekilde yardımcı olur:

- Site Recovery, tek veya tek başına sunucu gibi küçük Exchange dağıtımları için Azure'a veya ikincil bir siteye çoğaltma ve yük devretme yapabilir.
- Site Recovery, daha büyük dağıtımlar için Exchange DAG'leri ile tümleşir.
- Exchange DAG'leri, kuruluşlarda Exchange olağanüstü durum kurtarma işlemleri için önerilen çözümdür.  Site Recovery kurtarma planlarında siteler arası DAG yük devretmelerini düzenlemek üzere DAG'ler bulunabilir.

Exchange için olağanüstü durum kurtarma hakkında daha fazla bilgi edinmek için exchange [DAGs](/Exchange/high-availability/database-availability-groups/database-availability-groups) ve [Exchange felaket kurtarma](/Exchange/high-availability/disaster-recovery/disaster-recovery)bakın.

## <a name="protect-sap"></a>SAP'yi koruma

Site Recovery'yi kullanarak SAP dağıtımınızı şu şekilde koruyun:

- Bileşenleri Azure'a çoğaltarak, şirket içinde çalışan SAP NetWeaver ve NetWeaver dışı üretim uygulamalarının korumasını etkinleştirin.
- Bileşenleri başka bir Azure veri merkezine çoğaltarak,Azure çalıştıran SAP NetWeaver ve NetWeaver dışı üretim uygulamalarının korumasını etkinleştirin.
- SAP dağıtımınızı Azure'a geçirmek için Site Recovery'yi kullanarak buluta geçişi basitleştirin.
- SAP uygulamaları test etmek için talep üzerine üretim kopyası oluşturarak SAP proje yükseltmelerini, testlerini ve prototip oluşturma işlemlerini basitleştirin.

SAP için olağanüstü durum kurtarma hakkında [daha fazla bilgi edinin.](site-recovery-sap.md)

## <a name="protect-internet-information-services"></a>İnternet Bilgi Hizmetlerini Koruyun

İnternet Bilgi Hizmetleri (IIS) dağıtımınızı korumak için Site Kurtarma'yı aşağıdaki gibi kullanın:

Azure Site Recovery, ortamınızdaki kritik bileşenleri soğuk bir uzak konuma veya Microsoft Azure gibi bir genel buluta çoğaltarak olağanüstü durum kurtarma sağlar. Web sunucusu ve veritabanına sahip sanal makineler kurtarma sitesine çoğaltıldığından, yapılandırma dosyaları veya sertifikaları için ayrı bir yedekleme gereksinimi yoktur. Yük devretme sonrasında değişen ortam değişkenlerine bağımlı uygulama eşlemeleri ve bağlamaları, olağanüstü durum kurtarma planlarına tümleştirilmiş betikler aracılığıyla güncelleştirilebilir. Sanal makineler kurtarma sitesinde sadece bir failover sırasında getirilir. Azure Site Kurtarma, size aşağıdaki özellikleri sağlayarak uçdan uca başarısız olmayı düzenlemenize de yardımcı olur:

- Çeşitli katmanlardaki sanal makinelerin kapatma ve başlatma sıralaması.
- Uygulama bağımlılıklarının ve sanal makinelerdeki bağlamaların güncelleştirmelerine izin vermek için komut dosyaları ekleme. Betikler ayrıca DNS sunucusunu kurtarma konumunu işaret edecek şekilde güncelleştirmek için kullanılabilir.
- Birincil ve kurtarma ağlarının eşlenmesiyle IP adreslerini sanal makinelere önceden başarısız olarak ayırın ve başarısız sonrası güncellenmesi gerekmeyen komut dosyaları kullanın.
- Bir felaket sırasında karışıklık için kapsamı ortadan kaldırır birden çok web uygulamaları için tek tıklamayla failover yeteneği.
- DR ayrıntıları için yalıtılmış bir ortamda kurtarma planlarını test edebilme olanağı.

IIS için olağanüstü durum kurtarma hakkında [daha fazla bilgi edinin.](site-recovery-iis.md)

## <a name="protect-citrix-xenapp-and-xendesktop"></a>Citrix XenApp ve XenDesktop’u koruma

Citrix XenApp ve XenDesktop dağıtımlarınızı korumak için aşağıdaki gibi Site Recovery kullanın:

- Citrix XenApp ve XenDesktop dağıtımının korunmasını etkinleştirin. Farklı dağıtım katmanlarını Azure'a kopyalayın: Active Directory, DNS sunucusu, SQL veritabanı sunucusu, Citrix Delivery Controller, StoreFront server, XenApp Master (VDA), Citrix XenApp License Server.
- Citrix XenApp ve XenDesktop dağıtımınızı Azure’a geçirmek için Site Recovery’yi kullanarak buluta geçişi basitleştirin.
- Test ve hata ayıklama uygulamaları için isteğe bağlı üretim benzeri bir kopya oluşturarak Citrix XenApp/XenDesktop testlerini basitleştirin.
- Bu çözüm yalnızca Windows Server sanal masaüstü bilgisayarlar için geçerlidir, istemci sanal masaüstü bilgisayarlar için değil. İstemci sanal masaüstleri henüz Azure'da lisanslama için desteklenmedi. Azure’da istemci/sunucu masaüstlerini lisanslama hakkında [daha fazla bilgi edinin](https://azure.microsoft.com/pricing/licensing-faq/).

Citrix XenApp ve XenDesktop dağıtımları için olağanüstü durum kurtarma hakkında [daha fazla bilgi edinin.](site-recovery-citrix-xenapp-and-xendesktop.md) Veya, [Citrix teknik inceleme](https://aka.ms/citrix-xenapp-xendesktop-with-asr)bakın.

## <a name="next-steps"></a>Sonraki adımlar

Azure VM için olağanüstü durum kurtarma hakkında [daha fazla bilgi edinin.](azure-to-azure-quickstart.md)
