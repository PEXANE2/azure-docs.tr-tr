---
title: MABS & Sistem Merkezi DPM destek matrisi
description: Bu makalede, şirket içi ve Azure VM kaynaklarını yedeklemek için Microsoft Azure Yedekleme Sunucusu (MABS) veya System Center DPM kullandığınızda Azure Yedekleme desteği özetlenmiştir.
ms.date: 02/17/2019
ms.topic: conceptual
ms.openlocfilehash: 6664f7b226b75b364fd1c83f2abc56b5a275eff9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77582662"
---
# <a name="support-matrix-for-backup-with-microsoft-azure-backup-server-or-system-center-dpm"></a>Microsoft Azure Yedekleme Sunucusu veya System Center DPM ile yedekleme için destek matrisi

Şirket içi makineleri ve iş yüklerini ve Azure sanal makinelerini (VM) yedeklemek için [Azure Yedekleme hizmetini](backup-overview.md) kullanabilirsiniz. Bu makalede, Microsoft Azure Yedekleme Sunucusu (MABS) veya System Center Data Protection Manager (DPM) ve Azure Yedekleme kullanarak makineleri yedeklemeye yönelik destek ayarları ve sınırlamaları özetlenmiştir.

## <a name="about-dpmmabs"></a>DPM/MABS Hakkında

[System Center DPM,](https://docs.microsoft.com/system-center/dpm/dpm-overview?view=sc-dpm-1807) kurumsal makinelerin ve verilerin yedeklemesi ve kurtarılmasını yapılandıran, kolaylaştıran ve yöneten kurumsal bir çözümdür. [System Center](https://www.microsoft.com/cloud-platform/system-center-pricing) ürün paketinin bir parçası.

MABS, şirket içi fiziksel sunucuları, VM'leri ve bunların üzerinde çalışan uygulamaları yedeklemek için kullanılabilen bir sunucu ürünüdür.

MABS, System Center DPM'yi temel almaktadır ve birkaç farklılıkla benzer işlevler sağlar:

- MABS çalıştırmak için Sistem Merkezi lisansı gerekmez.
- Azure, hem MABS hem de DPM için uzun vadeli yedekleme depolama alanı sağlar. Buna ek olarak, DPM bant üzerinde uzun vadeli depolama için verileri yedeklemenize olanak sağlar. MABS bu işlevselliği sağlamaz.
- Birincil Bir DPM sunucusunun ikincil bir DPM sunucusuyla yedeklenebilir. İkincil sunucu, birincil sunucu veritabanını ve birincil sunucuda depolanan veri kaynağı çoğaltmalarını korur. Birincil sunucu başarısız olursa, birincil sunucu tekrar kullanılabilir hale gelene kadar ikincil sunucu, birincil sunucu tarafından korunan iş yüklerini korumaya devam edebilir  MABS bu işlevselliği sağlamaz.

MABS'yi [Microsoft Download Center'dan](https://www.microsoft.com/download/details.aspx?id=57520)indirin. Şirket içinde veya Azure VM'de çalıştırılabilir.

DPM ve MABS, çok çeşitli uygulamaları, sunucu ve istemci işletim sistemlerini yedeklemeyi destekler. Birden çok yedekleme senaryosu sağlarlar:

- Sistem durumu veya çıplak metal yedekleme ile makine düzeyinde yedekleme yapabilirsiniz.
- Belirli birimleri, paylaşımları, klasörleri ve dosyaları yedekleyebilirsiniz.
- Optimize edilmiş uygulama ayarları kullanarak belirli uygulamaları yedekleyebilirsiniz.

## <a name="dpmmabs-backup"></a>DPM/MABS yedekleme

DPM/MABS ve Azure Yedekleme kullanarak yedekleme aşağıdaki gibi çalışır:

1. Yedeklenecek her makineye DPM/MABS koruma maddesi takılır.
1. Makineler ve uygulamalar DPM/MABS'deki yerel depolama alanına yedeklenir.
1. Microsoft Azure Kurtarma Hizmetleri (MARS) aracısı DPM sunucusu/MABS'de yüklenir.
1. MARS aracısı, Azure Yedekleme'yi kullanarak DPM/MABS disklerini Azure'daki yedek Kurtarma Hizmetleri kasasına yedekler.

Daha fazla bilgi için:

- MABS mimarisi hakkında [daha fazla bilgi edinin.](backup-architecture.md#architecture-back-up-to-dpmmabs)
- MARS ajanı için [desteklenenleri gözden geçirin.](backup-support-matrix-mars-agent.md)

## <a name="supported-scenarios"></a>Desteklenen senaryolar

**Senaryo** | **Aracı** | **Konum**
--- | --- | ---
**Şirket içi makineleri/iş yüklerini yedekleme** | DPM/MABS koruma maddesi yedeklemek istediğiniz makinelerde çalışır.<br/><br/> DPM/MABS sunucusundaki MARS aracısı.<br/> Bu özelliği etkinleştirmek için gereken Microsoft Azure Kurtarma Hizmetleri aracısının veya Azure Yedekleme aracısının en az sürümü 2.0.8719.0'dır.  | DPM/MABS şirket içinde çalışıyor olmalıdır.

## <a name="supported-deployments"></a>Desteklenen dağıtımlar

DPM/MABS aşağıdaki tabloda özetlendiği gibi dağıtılabilir.

**Dağıtım** | **Destek** | **Şey**
--- | --- | ---
**Şirket içinde konuşlandırıldı** | Fiziksel sunucu<br/><br/>Hyper-V VM<br/><br/> VMware VM | DPM/MABS VMware VM olarak yüklenmişse, yalnızca VMware VM'leri ve bu VM'lerde çalışan iş yüklerini yedekler.
**Azure Yığını VM olarak dağıtıldı** | Yalnızca MABS | DPM, Azure Stack VM'lerini yedeklemek için kullanılamaz.
**Azure VM olarak dağıtıldı** | Azure VM'leri ve bu VM'lerde çalışan iş yüklerini korur | Azure'da çalışan DPM/MABS şirket içi makineleri yedekleemez.

## <a name="supported-mabs-and-dpm-operating-systems"></a>Desteklenen MABS ve DPM işletim sistemleri

Azure Yedekleme, aşağıdaki işletim sistemlerinden herhangi birini çalıştıran DPM/MABS örneklerini yedekleyebilir. İşletim sistemleri en son hizmet paketlerini ve güncelleştirmeleri çalıştırıyor olmalıdır.

**Senaryo** | **DPM/MABS**
--- | ---
**Azure VM'de MABS** |  Windows 2016 Veri Merkezi.<br/><br/> Windows 2019 Veri Merkezi.<br/><br/> Pazardaki bir resimle başlamanızı öneririz.<br/><br/> En az dört çekirdekli ve 8 GB RAM ile Standard_A4_v2.
**Azure VM'de DPM** | Sistem Merkezi 2012 R2 Güncelleme 3 veya daha sonra.<br/><br/> [Sistem Merkezi tarafından gerekli](https://docs.microsoft.com/system-center/dpm/prepare-environment-for-dpm?view=sc-dpm-1807#dpm-server)windows işletim sistemi .<br/><br/> Pazardaki bir resimle başlamanızı öneririz.<br/><br/> En az dört çekirdekli ve 8 GB RAM ile Standard_A4_v2.
**Şirket içi MABS** |  MABS v3 ve sonrası: Windows Server 2016 veya Windows Server 2019
**Şirket içi DPM** | Fiziksel sunucu/Hyper-V VM: System Center 2012 SP1 veya sonrası.<br/><br/> VMware VM: Sistem Merkezi 2012 R2 Güncelleme 5 veya daha sonra.

>[!NOTE]
>Azure Yedekleme Sunucusu'nun yüklenmesi Windows Server Core veya Microsoft Hyper-V Server'da desteklenmez.

## <a name="management-support"></a>Yönetim desteği

**Sorunu** | **Şey**
--- | ---
**Yükleme** | DPM/MABS'yi tek amaçlı bir makineye tökezle.<br/><br/> DPM/MABS'yi etki alanı denetleyicisi üzerine, Application Server rol yüklemesi olan bir makineye, Microsoft Exchange Server veya System Center Operations Manager çalıştıran bir makineye veya küme düğümüne yüklemeyin.<br/><br/> [Tüm DPM sistem gereksinimlerini gözden geçirin.](https://docs.microsoft.com/system-center/dpm/prepare-environment-for-dpm?view=sc-dpm-1807#dpm-server)
**Domain** | DPM/MABS bir etki alanına birleştirilmelidir. Önce yükleyin ve ardından DPM/MABS'yi bir etki alanına katılın. Dağıtımdan sonra DPM/MABS'nin yeni bir etki alanına taşınması desteklenmez.
**Depolama** | Modern yedekleme depolama (MBS) DPM 2016/MABS v2 ve sonraki desteklenir. MABS v1 için kullanılamaz.
**MABS yükseltmesi** | Doğrudan MABS v3'ü yükleyebilir veya MABS v2'den MABS v3'e yükseltebilirsiniz. [Daha fazla bilgi edinin](backup-azure-microsoft-azure-backup.md#upgrade-mabs).
**Hareketli MABS** | MBS kullanıyorsanız, depolama alanını korurken MABS'nin yeni bir sunucuya taşınması desteklenir.<br/><br/> Sunucu orijinalle aynı ada sahip olmalıdır. Aynı depolama havuzunu tutmak ve veri kurtarma noktalarını depolamak için aynı MABS veritabanını kullanmak istiyorsanız, adı değiştiremezsiniz.<br/><br/> Geri yüklemeniz gerekeceği için MABS veritabanının bir yedeğine ihtiyacınız olacaktır.

## <a name="mabs-support-on-azure-stack"></a>Azure Yığını'nda MABS desteği

Azure Stack VM'lerin ve iş yüklerinin yedeklerini tek bir konumdan yönetebilmeniz için MABS'yi Azure Yığını VM'de dağıtabilirsiniz.

**Bileşen** | **Şey**
--- | ---
**Azure Yığını VM'de MABS** | En azından A2 boyutu. Azure Marketi'nden bir Windows Server 2012 R2 veya Windows Server 2016 görüntüsüyle başlamanızı öneririz.<br/><br/> MABS VM'ye başka bir şey yüklemeyin.
**MABS depolama** | MABS VM için ayrı bir depolama hesabı kullanın. MABS üzerinde çalışan MARS aracısının önbellek konumu ve buluttan geri yüklenen verileri tutmak için geçici depolamaya ihtiyacı vardır.
**MABS depolama havuzu** | MABS depolama havuzunun boyutu, MABS VM'ye bağlı disklerin sayısına ve boyutuna göre belirlenir. Her Azure Stack VM boyutunda maksimum sayıda disk vardır. Örneğin, A2 dört disktir.
**MABS tutma** | Yerel MABS disklerinde yedeklenen verileri beş günden fazla tutmayın.
**MABS ölçeği** | Dağıtımınızı büyütmek için MABS VM'nin boyutunu artırabilirsiniz. Örneğin, A'dan D'ye dizi değiştirebilirsiniz.<br/><br/> Azure yedeklemeile düzenli olarak veri boşaltdığınızdan da emin olabilirsiniz. Gerekirse, ek MABS sunucuları dağıtabilirsiniz.
**.NET Çerçevesi MABS üzerinde** | MABS VM'nin .NET Framework 3.3 SP1'e veya daha sonra üzerine yüklenmesi gerekir.
**MABS etki alanı** | MABS VM bir etki alanına birleştirilmelidir. Yönetici ayrıcalıklarına sahip bir etki alanı kullanıcısı VM'ye MABS yüklemelidir.
**Azure Yığını VM veri yedekleme** | Dosyaları, klasörleri ve uygulamaları yedekleyebilirsiniz.
**Desteklenen yedekleme** | Bu işletim sistemleri yedeklemek istediğiniz VM'ler için desteklenir:<br/><br/> Windows Server Yarı Yıllık Kanal (Datacenter, Kurumsal, Standart)<br/><br/> Windows Server 2016, Windows Server 2012 R2, Windows Server 2008 R2
**Azure Stack VM'ler için SQL Server desteği** | SQL Server 2016, SQL Server 2014, SQL Server 2012 SP1'i yedekleyin.<br/><br/> Yedekleyin ve bir veritabanı kurtarın.
**Azure Stack VM'leri için SharePoint desteği** | SharePoint 2016, SharePoint 2013, SharePoint 2010.<br/><br/> Bir çiftliği, veritabanını, ön ucunu ve web sunucusuna yedekleyin ve kurtarın.
**Yedeklenen VM'ler için ağ gereksinimleri** | Azure Yığını iş yükündeki tüm Sanal M'ler aynı sanal ağa ait olmalı ve aynı aboneğe ait olmalıdır.

## <a name="dpmmabs-networking-support"></a>DPM/MABS ağ desteği

### <a name="url-access"></a>URL erişimi

DPM sunucusu/MABS'nin bu URL'lere erişmesi gerekir:

- `http://www.msftncsi.com/ncsi.txt`
- *.Microsoft.com
- *.WindowsAzure.com
- *.microsoftonline.com
- *.windows.net

### <a name="azure-expressroute-support"></a>Azure ExpressRoute desteği

Verilerinizi Azure ExpressRoute üzerinden herkese açık bakış (eski devreler için kullanılabilir) ve Microsoft bakışlarıyla yedekleyebilirsiniz. Özel bakış lar üzerinden yedekleme desteklenmez.

Genel bakışla: Aşağıdaki etki alanlarının/adreslerine erişimsağlayın:

* `http://www.msftncsi.com/ncsi.txt`
* `microsoft.com`
* `.WindowsAzure.com`
* `.microsoftonline.com`
* `.windows.net`

Microsoft'un bakışlarıyla, lütfen aşağıdaki hizmetleri/bölgeleri ve ilgili topluluk değerlerini seçin:

* Azure Etkin Dizin (12076:5060)
* Microsoft Azure Bölgesi (Kurtarma Hizmetleri kasanızın konumuna göre)
* Azure Depolama (Kurtarma Hizmetleri kasanızın konumuna göre)

Daha fazla bilgi için [ExpressRoute yönlendirme gereksinimlerine](https://docs.microsoft.com/azure/expressroute/expressroute-routing)bakın.

>[!NOTE]
>Kamu Peering yeni devreler için amortismana.

### <a name="dpmmabs-connectivity-to-azure-backup"></a>Azure Yedekleme'ye DPM/MABS bağlantısı

Yedeklemelerin düzgün çalışması için Azure Yedekleme hizmetine bağlantı gereklidir ve Azure aboneliği etkin olmalıdır. Aşağıdaki tablo, bu iki şey gerçekleşmezse davranışı gösterir.

**MABS'den Azure'a** | **Abonelik** | **Yedekleme/Geri Yükleme**
--- | --- | ---
Bağlı | Etkin | DPM/MABS diskine geri dön.<br/><br/> Azure'a geri dön.<br/><br/> Diskten geri yükleyin.<br/><br/> Azure'dan geri yükleyin.
Bağlı | Süresi dolmuş/hükümden arındırılmış | Diskveya Azure'a yedekleme yok.<br/><br/> Aboneliğin süresi dolduysa, diskten veya Azure'dan geri yükleyebilirsiniz.<br/><br/> Abonelik kullanımdan kaldırıldıysa, diskten veya Azure'dan geri yükleyemezsiniz. Azure kurtarma noktaları silinir.
15 günden fazla bağlantı yok | Etkin | Diskveya Azure'a yedekleme yok.<br/><br/> Diskten veya Azure'dan geri yükleyebilirsiniz.
15 günden fazla bağlantı yok | Süresi dolmuş/hükümden arındırılmış | Diskveya Azure'a yedekleme yok.<br/><br/> Aboneliğin süresi dolduysa, diskten veya Azure'dan geri yükleyebilirsiniz.<br/><br/> Abonelik kullanımdan kaldırıldıysa, diskten veya Azure'dan geri yükleyemezsiniz. Azure kurtarma noktaları silinir.

## <a name="dpmmabs-storage-support"></a>DPM/MABS depolama desteği

DPM/MABS'ye yedeklenen veriler yerel disk depolama da depolanır.

**Depolama** | **Şey**
--- | ---
**Mbs** | Modern yedekleme depolama (MBS) DPM 2016/MABS v2 ve sonraki desteklenir. MABS v1 için kullanılamaz.
**Azure VM'de MABS depolama** | Veriler, DPM/MABS VM'ye bağlı ve DPM/MABS'de yönetilen Azure disklerinde depolanır. DPM/MABS depolama havuzu için kullanılabilecek disk sayısı VM boyutuyla sınırlıdır.<br/><br/> A2 VM: 4 disk; A3 VM: 8 disk; A4 VM: 16 disk, her disk için 1 TB maksimum boyutu ile. Bu, kullanılabilir toplam yedekleme depolama havuzunu belirler.<br/><br/> Yedeklediğiniz veri miktarı, ekli disklerin sayısına ve boyutuna bağlıdır.
**Azure VM'de MABS veri saklama** | Verileri DPM/MABS Azure diskinde bir gün boyunca saklamanızı ve daha uzun bekletme için DPM/MABS'den kasaya yedeklemenizi öneririz. Böylece, daha büyük miktarda veriyi Azure Yedekleme'ye boşaltarak koruyabilirsiniz.

### <a name="modern-backup-storage-mbs"></a>Modern yedekleme depolama (MBS)

DPM 2016/MABS v2'den (Windows Server 2016'da çalışır) ve daha sonra, modern yedekleme depolama (MBS) avantajlarından yararlanabilirsiniz.

- MBS yedeklemeleri Esnek Dosya Sistemi (ReFS) diskinde depolanır.
- MBS, daha hızlı yedekleme ve depolama alanının daha verimli kullanımı için ReFS blok klonlama kullanır.
- Yerel DPM/MABS depolama havuzuna birim eklediğinizde, bunları sürücü harfleri ile yapılandırırsınız. Daha sonra iş yükü depolamayı farklı birimlerde yapılandırabilirsiniz.
- Verileri DPM/MABS'ye yedeklemek için koruma grupları oluşturduğunuzda, kullanmak istediğiniz sürücüyü seçersiniz. Örneğin, yüksek performanslı bir sürücüde SQL veya diğer yüksek IOPS iş yüklerinin yedeklerini depolayıp daha düşük performanslı bir sürücüde daha az yedeklenen iş yüklerini depolayabilirsiniz.

## <a name="supported-backups-to-mabs"></a>MABS için desteklenen yedeklemeler

Azure Yedekleme Sunucusu ile koruyabileceğiniz çeşitli sunucular ve iş yükleri hakkında bilgi için [Azure Yedekleme Sunucusu Koruma Matrisi'ne](https://docs.microsoft.com/azure/backup/backup-mabs-protection-matrix#protection-support-matrix)bakın.

## <a name="supported-backups-to-dpm"></a>DPM için desteklenen yedeklemeler

Veri Koruma Yöneticisi ile koruyabileceğiniz çeşitli sunucular ve iş yükleri hakkında bilgi için [DPM neleri yedekleyebilir makalesine bakın?](https://docs.microsoft.com/system-center/dpm/dpm-protection-matrix?view=sc-dpm-2019)

- DPM/MABS tarafından desteklenen kümelenmiş iş yükleri, DPM/MABS ile aynı etki alanında veya alt/güvenilen bir etki alanında olmalıdır.
- Verileri güvenilmeyen etki alanlarında veya çalışma gruplarında yedeklemek için NTLM/sertifika kimlik doğrulaması kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- MABS mimarisi hakkında [daha fazla bilgi edinin.](backup-architecture.md#architecture-back-up-to-dpmmabs)
- MARS ajanı için desteklenenleri [gözden geçirin.](backup-support-matrix-mars-agent.md)
- Bir MABS sunucusu [ayarlayın.](backup-azure-microsoft-azure-backup.md)
- [DPM'yi ayarlayın.](https://docs.microsoft.com/system-center/dpm/install-dpm?view=sc-dpm-180)
