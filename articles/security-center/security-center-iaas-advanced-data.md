---
title: Azure Güvenlik Merkezi 'nin SQL makineler için gelişmiş veri güvenliği (Önizleme)
description: Azure Güvenlik Merkezi 'nde SQL makinelerinde gelişmiş veri güvenliğini nasıl etkinleştirebileceğinizi öğrenin
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: ba46c460-6ba7-48b2-a6a7-ec802dd4eec2
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2020
ms.author: memildin
ms.openlocfilehash: 673524f08ecae28fb6f2e18f3c0c7230cf102b06
ms.sourcegitcommit: 374e47efb65f0ae510ad6c24a82e8abb5b57029e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2020
ms.locfileid: "85513468"
---
# <a name="advanced-data-security-for-sql-machines-preview"></a>SQL makineler için gelişmiş veri güvenliği (Önizleme)

Azure Güvenlik Merkezi 'nin SQL makineler için gelişmiş veri güvenliği, Azure 'da, diğer bulut ortamlarında ve hatta şirket içi makinelerde barındırılan SQL sunucularını korur. Bu, karma ortamları tam olarak desteklemek için Azure Native SQL sunucularınız için korumaların kapsamını genişletir.

Bu önizleme özelliği, olası veritabanı güvenlik açıklarını belirlemek ve azaltmak ve veritabanınıza tehditler oluşturabilecek anormal etkinlikleri saptamak için işlevsellik içerir: 

* **Güvenlik açığı değerlendirmesi** -olası veritabanı güvenlik açıklarını düzeltmenize, izlemeye ve yardımcı olmaya yönelik tarama hizmeti. Değerlendirme taramaları, SQL makinelerinizin güvenlik durumuna genel bir bakış ve güvenlik bulgularının ayrıntılarını sağlar.

* [Gelişmiş tehdit koruması](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-overview) -SQL Server 'ınızı SQL ekleme, deneme yanılma saldırıları ve ayrıcalık kötüye kullanımı gibi tehditler için sürekli olarak izleyen algılama hizmeti. Bu hizmet, Azure Güvenlik Merkezi 'nde şüpheli etkinliğin ayrıntıları, tehditleri hafifletmeye yönelik yönergeler ve Azure Sentinel ile araştırmalarınıza devam etme seçeneklerini içeren eyleme dayalı güvenlik uyarıları sağlar.

>[!TIP]
> SQL makinelerinde gelişmiş veri güvenliği, Azure Güvenlik Merkezi 'nin Azure SQL veritabanları, SYNAPSE ve SQL yönetilen örnekleri için zaten kullanılabilir olan [Gelişmiş veri güvenliği paketinin](https://docs.microsoft.com/azure/sql-database/sql-database-advanced-data-security)bir uzantısıdır.


## <a name="set-up-advanced-data-security-for-sql-machines"></a>SQL makineleri için gelişmiş veri güvenliğini ayarlama 

Azure Güvenlik Merkezi 'nin SQL makineler için gelişmiş veri güvenliğini ayarlamak iki adımdan oluşur:

* SQL sunucunuzun ana bilgisayarında Log Analytics aracısını sağlayın. Bu, Azure bağlantısı sağlar.

* Güvenlik Merkezi 'nin fiyatlandırma ve Ayarlar sayfasında isteğe bağlı paketi etkinleştirin.

Bunların her ikisi de aşağıda açıklanmıştır.

### <a name="step-1-provision-the-log-analytics-agent-on-your-sql-servers-host"></a>Adım 1. SQL sunucunuzun ana bilgisayarında Log Analytics aracısını sağlayın:

- **Azure VM 'de SQL Server** -SQL makineniz BIR Azure VM üzerinde barındırılıyorsa, [otomatik olarak Log Analytics aracısını](security-center-enable-data-collection.md#workspace-configuration)sağlayabilirsiniz. Alternatif olarak, el ile [Azure VM ekleme](quick-onboard-azure-stack.md#add-the-virtual-machine-extension-to-your-existing-azure-stack-virtual-machines)yordamını izleyebilirsiniz.

- **Azure arc SQL Server** -SQL Server bir [Azure yay](https://docs.microsoft.com/azure/azure-arc/) makinesinde barındırılıyorsa, güvenlik merkezi önerisi "Log Analytics Aracısı Windows tabanlı Azure yay makinelerinize (önizleme) yüklenmelidir" Log Analytics aracıyı dağıtabilirsiniz. Alternatif olarak, [Azure Arc belgelerindeki](https://docs.microsoft.com/azure/azure-arc/servers/manage-vm-extensions#enable-extensions-from-the-portal)el ile gerçekleştirilen yordamı izleyebilirsiniz.

- **Şirket içi SQL Server** -SQL Server Azure Arc olmadan şirket Içi bir Windows makinesinde barındırılıyorsa, Azure 'a bağlamak için iki seçeneğiniz vardır:
    
    - **Azure yayı dağıtma** -herhangi bir Windows makinesini Güvenlik Merkezi 'ne bağlayabilirsiniz. Bununla birlikte, Azure Arc *Tüm* Azure ortamlarınız genelinde daha derin tümleştirme sağlar. Azure yay ayarlarsanız, portalda **SQL Server – Azure yay** sayfasını görürsünüz ve güvenlik uyarılarınız ilgili sayfada özel bir **güvenlik** sekmesinde görüntülenir. Bu nedenle, ilk ve önerilen seçenek, [Azure Arc 'ı konakta ayarlamaya](https://docs.microsoft.com/azure/azure-arc/servers/onboard-portal#install-and-validate-the-agent-on-windows) ve yukarıdaki **azure Arc üzerinde SQL Server**yönergelerini izlemeye yöneliktir.
        
    - **Windows makinesini Azure Arc olmadan bağlama** -Windows makinesinde çalışan bir SQL Server Azure yay kullanmadan bağlamayı seçerseniz, [Windows makinelerini Azure izleyici 'ye bağlama](https://docs.microsoft.com/azure/azure-monitor/platform/agent-windows)bölümündeki yönergeleri izleyin.


### <a name="step-2-enable-the-optional-bundle-in-security-centers-pricing-and-settings-page"></a>Adım 2. Güvenlik Merkezi 'nin fiyatlandırma ve Ayarlar sayfasında isteğe bağlı paketi etkinleştirin:

1. Güvenlik Merkezi 'nin kenar çubuğundan **fiyatlandırma & ayarları** sayfasını açın.

    - **Azure Güvenlik Merkezi 'nin varsayılan çalışma alanını** ("defaultworkspace-[abonelik kimliğiniz]-[Bölge]" olarak adlandırılan) kullanıyorsanız ilgili **aboneliği**seçin.

    - **Varsayılan olmayan bir çalışma alanı**kullanıyorsanız ilgili **çalışma alanını** seçin (gerekirse filtreye çalışma alanının adını girin):

        ![başlık](./media/security-center-advanced-iaas-data/pricing-and-settings-workspaces.png)


1. **Makinelerdeki SQL sunucularının (Önizleme)** seçeneğini etkin olarak değiştirin. 

    [![İsteğe bağlı demeti olan güvenlik merkezi fiyatlandırma sayfası](media/security-center-advanced-iaas-data/sql-servers-on-vms-in-pricing-small.png)](media/security-center-advanced-iaas-data/sql-servers-on-vms-in-pricing-large.png#lightbox)

    Makinelerdeki SQL Server 'lar için gelişmiş veri güvenliği, seçilen çalışma alanına bağlı tüm SQL Server 'lar üzerinde etkinleştirilecek. SQL Server ilk yeniden başlatıldıktan sonra koruma tam olarak etkin olacaktır. 

    >[!TIP] 
    > Yeni bir çalışma alanı oluşturmak için [Log Analytics çalışma alanı oluşturma](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace)bölümündeki yönergeleri izleyin.


1. İsteğe bağlı olarak, güvenlik uyarıları için e-posta bildirimini yapılandırın. 
    Güvenlik Merkezi uyarıları oluşturulduğunda, bir e-posta bildirimi almak için alıcıların listesini ayarlayabilirsiniz. E-posta, Azure Güvenlik Merkezi 'ndeki tüm ilgili ayrıntıların bulunduğu uyarıya doğrudan bir bağlantı içerir. Daha fazla bilgi için bkz. [güvenlik uyarıları için e-posta bildirimleri ayarlama](https://docs.microsoft.com/azure/security-center/security-center-provide-security-contact-details).



## <a name="explore-vulnerability-assessment-reports"></a>Güvenlik açığı değerlendirme raporlarını keşfet

Güvenlik açığı değerlendirme hizmeti, veritabanlarınızı haftada bir kez tarar. Taramalar, hizmeti etkinleştirdiğiniz haftanın aynı gününde çalışır.

Güvenlik açığı değerlendirme panosu, tüm veritabanlarınızda, sağlıklı ve sağlıksız veritabanlarının bir özeti ve risk dağıtımına göre başarısız olan denetimlerin genel bir özeti hakkında genel bir bakış sağlar.

Güvenlik açığı değerlendirme sonuçlarını doğrudan güvenlik merkezi 'nden görüntüleyebilirsiniz.

1. Güvenlik Merkezi 'nin kenar çubuğundan **öneriler** sayfasını açın ve **makinelerdeki SQL veritabanı sunucularınızda öneri güvenlik açıklarını seçin (Önizleme)**. Daha fazla bilgi için bkz. [Güvenlik Merkezi önerileri](security-center-recommendations.md). 


    [![* * Makinelerdeki SQL veritabanlarınızdaki güvenlik açıkları düzeltilmelidir (Önizleme) * * öneri](media/security-center-advanced-iaas-data/data-and-storage-sqldb-vulns-on-vm.png)](media/security-center-advanced-iaas-data/data-and-storage-sqldb-vulns-on-vm.png#lightbox)

    Bu öneriye ilişkin ayrıntılı görünüm görüntülenir.

    [![Makinelerdeki SQL veritabanlarınızdaki * * güvenlik açıkları için ayrıntılı görünüm (Önizleme) * * öneri](media/security-center-advanced-iaas-data/all-servers-view.png)](media/security-center-advanced-iaas-data/all-servers-view.png#lightbox)

1. Daha fazla ayrıntı için ayrıntıya gidin:

    * Taranan kaynaklara (veritabanlarına) ve test edilen güvenlik denetimlerinin listesine genel bakış için ilgilendiğiniz sunucuyu seçin.

    * Belirli bir SQL veritabanı tarafından gruplanmış güvenlik açıklarına genel bir bakış için, ilgilendiğiniz veritabanını seçin.

    Her görünümde güvenlik denetimleri **önem derecesine**göre sıralanır. Bir **Açıklama**içeren ayrıntı bölmesini, bunu nasıl **düzeltebileceğiniz** ve **etki** veya **kıyaslama**gibi diğer ilgili bilgileri görmek için belirli bir güvenlik denetimine tıklayın.

## <a name="advanced-threat-protection-for-sql-servers-on-machines-alerts"></a>Makine uyarıları üzerinde SQL sunucuları için Gelişmiş tehdit koruması
Uyarılar, SQL makinelerine erişmek veya bu makinelere yararlanmak için olağandışı ve olası zararlı saldırılara göre oluşturulur. Bu olaylar, [Uyarı başvurusu SAYFASıNıN SQL veritabanı ve SQL veri ambarı uyarıları bölümünde](alerts-reference.md#alerts-sql-db-and-warehouse)gösterilen uyarıları tetikleyebilirsiniz.



## <a name="explore-and-investigate-security-alerts"></a>Güvenlik uyarılarını araştır ve araştır

Güvenlik uyarıları Güvenlik Merkezi 'nin Uyarılar sayfasında, kaynağın Güvenlik sekmesinde veya uyarı e-postalarında doğrudan bağlantı aracılığıyla kullanılabilir.

1. Uyarıları görüntülemek için Güvenlik Merkezi 'nin kenar çubuğundan **güvenlik uyarıları** ' nı seçin ve bir uyarı seçin.

1. Uyarılar, her birindeki ayrıntılı düzeltme adımları ve araştırma bilgileri ile kendine dahil olmak üzere tasarlanmıştır. Daha geniş bir görünüm için diğer Azure Güvenlik Merkezi ve Azure Sentinel özelliklerini kullanarak daha fazla araştırma yapabilirsiniz:

    * Daha fazla araştırmalar için SQL Server denetim özelliğini etkinleştirin. Azure Sentinel kullanıcısı kullanıyorsanız, Windows Güvenlik günlüğü etkinliklerinden SQL denetim günlüklerini Sentinel 'e yükleyebilir ve zengin araştırma deneyiminden yararlanabilirsiniz. [SQL Server denetimi hakkında daha fazla bilgi edinin](https://docs.microsoft.com/sql/relational-databases/security/auditing/create-a-server-audit-and-server-audit-specification?view=sql-server-ver15).
    * Güvenlik duruşunuzu geliştirmek için, her uyarıda belirtilen ana makine için Güvenlik Merkezi 'nin önerilerini kullanın. Bu, gelecekteki saldırılardan riskleri azaltır. 

    [Uyarıları yönetme ve uyarıları yanıtlama hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts).


## <a name="next-steps"></a>Sonraki adımlar

İlgili malzemeler için aşağıdaki makaleye bakın:

- [SQL veritabanı ve SQL veri ambarı için güvenlik uyarıları](alerts-reference.md#alerts-sql-db-and-warehouse)
- [Güvenlik uyarıları için e-posta bildirimleri ayarlama](security-center-provide-security-contact-details.md)
- [Azure Sentinel hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/sentinel/)
- [Azure Güvenlik Merkezi 'nin gelişmiş veri güvenliği paketi](https://docs.microsoft.com/azure/sql-database/sql-database-advanced-data-security)