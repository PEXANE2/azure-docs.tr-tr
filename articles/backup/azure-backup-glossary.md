---
title: Azure Backup sözlüğü
description: Bu makalede, Azure Backup ile kullanım için yararlı olan terimler tanımlanmaktadır.
ms.topic: conceptual
ms.date: 12/21/2020
ms.openlocfilehash: 5b575e0f56c9cf39987e9e77850ab1d9b2e80d93
ms.sourcegitcommit: 78ecfbc831405e8d0f932c9aafcdf59589f81978
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2021
ms.locfileid: "98723923"
---
# <a name="azure-backup-glossary"></a>Azure Backup sözlüğü

Bu terim sözlüğü Azure Backup kullanılırken yararlı olabilir.

>[!NOTE]
>
> - "(Iş yüküne özgü terim)" önekiyle işaretlenen koşullar, yalnızca Azure Backup desteklediği belirli iş yüklerinin belirli bir alt kümesinin içeriğiyle ilgili olan terimlere başvurur.
> - Azure Backup belgelerde yaygın olarak kullanılan ancak diğer Azure hizmetlerine başvuran şartlar için, ilgili Azure hizmetinin belgelerine doğrudan bir bağlantı sağlanır.

## <a name="afs-azure-file-shares"></a>AFS (Azure dosya paylaşımları)

[Azure dosyaları belgelerine](../storage/files/storage-files-introduction.md)bakın.

## <a name="alternate-location-recovery"></a>Alternatif konum kurtarma

Kurtarma noktasından, yedeklemelerin alındığı orijinal konumdan başka bir konuma yapılan bir kurtarma. Azure VM yedeklemesi kullanılırken bu, VM 'nin yedeklerin alındığı orijinal sunucu dışında bir sunucuya geri yüklenmesi anlamına gelir. Azure dosya paylaşma yedeklemesi kullanılırken bu, verileri yedeklenen dosya paylaşımından farklı bir dosya paylaşımında geri yükleme anlamına gelir.

## <a name="application-consistent-backup"></a>Uygulamayla tutarlı yedekleme

(İş yüküne özgü terim)

Uygulamayla tutarlı yedeklemeler bellek içeriğini ve bekleyen g/ç işlemlerini yakalar. Uygulamayla tutarlı anlık görüntüler, bir yedekleme gerçekleşmeden önce uygulama verilerinin tutarlılığını sağlamak için bir [VSS](#vss-windows-volume-shadow-copy-service) VSS Yazıcı (veya Linux için komut dosyaları öncesi veya sonrası) kullanır. [Daha fazla bilgi edinin](backup-azure-vms-introduction.md).

## <a name="azure-resource-manager-arm-templates"></a>Azure Resource Manager (ARM) şablonları

[ARM şablonları belgelerine](../azure-resource-manager/templates/overview.md)bakın.

## <a name="autoprotection-for-databases"></a>Oto Koruma (veritabanları için)

(İş yüküne özgü terim)

Otomatik koruma, tek başına SQL Server örneğindeki veya SQL Server Always on kullanılabilirlik grubundaki tüm veritabanlarını otomatik olarak korumanıza olanak sağlayan bir özelliktir. Yalnızca var olan veritabanları için yedeklemeleri etkinleştirmeyin, ancak gelecekte ekleyebileceğiniz tüm veritabanlarını da korur.

## <a name="availability-storage-replication-types"></a>Kullanılabilirlik (depolama çoğaltma türleri)

Azure Backup, depolama ve verilerinizi yüksek oranda kullanılabilir tutmak için üç tür çoğaltma sunar:

### <a name="lrs"></a>LRS

[Yerel olarak yedekli depolama (LRS)](../storage/common/storage-redundancy.md#locally-redundant-storage) , veri merkezindeki bir depolama ölçek biriminde yedekleme verilerinizi üç kez (yedekleme verilerinizin üç kopyasını oluşturur) çoğaltır. Yedekleme verilerinin tüm kopyaları aynı bölgede bulunur. LRS, yedekleme verilerinizi yerel donanım hatalarından korumak için düşük maliyetli bir seçenektir.

### <a name="grs"></a>GRS

[Coğrafi olarak yedekli depolama (GRS)](../storage/common/storage-redundancy.md#geo-redundant-storage) , varsayılan ve önerilen çoğaltma seçeneğidir. GRS, yedekleme verilerinizi kaynak verilerin birincil konumundan yüzlerce mil uzakta olan ikincil bir bölgeye çoğaltır. GRS 'nin maliyeti LRS 'den fazladır, ancak bölgesel bir kesinti olsa da, yedekleme verileriniz için GRS daha yüksek düzeyde dayanıklılık sağlar.

>[!NOTE]
>Çapraz bölge geri yükleme özelliği etkin olan GRS kasaları için yedekleme depolama alanı, GRS 'den RA-GRS (okuma erişimli Geo-Redundant depolama) sürümüne yükseltilir.

### <a name="zrs"></a>ZRS

Bölgesel olarak [yedekli depolama (ZRS)](../storage/common/storage-redundancy.md#zone-redundant-storage) , yedekleme verilerini [kullanılabilirlik bölgelerinde](../availability-zones/az-overview.md#availability-zones)çoğaltır ve bu da aynı bölgedeki yedekleme verileri uygunluğunu ve dayanıklılığı garanti altına alır. Bu nedenle, [veri](https://azure.microsoft.com/resources/achieving-compliant-data-residency-and-security-with-azure/) yerleşimi gerektiren kritik iş YÜKLERINIZ ZRS 'de yedeklenebilir.

## <a name="azure-command-line-interface-cli"></a>Azure Komut Satırı Arabirimi (CLI)

[Azure CLI belgeleri](/cli/azure/what-is-azure-cli)bölümüne bakın.

## <a name="azure-policy"></a>Azure İlkesi

[Azure ilkesi belgeleri](../governance/policy/overview.md)bölümüne bakın.

## <a name="azure-powershell"></a>Azure PowerShell

[Azure PowerShell belgelerine](/powershell/azure/)bakın.

## <a name="azure-resource-manager-arm"></a>Azure Resource Manager (ARM)

[Azure Resource Manager belgelerine](../azure-resource-manager/management/overview.md)bakın.

## <a name="azure-disk-encryption-ade"></a>Azure Disk Şifrelemesi (ADE)

[Azure disk şifrelemesi belgeleri](../security/fundamentals/azure-disk-encryption-vms-vmss.md)bölümüne bakın.

## <a name="backend-storage--cloud-storage--backup-storage"></a>Arka uç depolama/bulut depolama/yedekleme depolaması

Bir yedekleme örneği tarafından kullanılan gerçek depolama alanı. Yedekleme örneği için mevcut olan tüm bekletme noktalarının boyutunu içerir (yedekleme ve bekletme ilkesi tarafından tanımlanan şekilde).

## <a name="bare-metal-backup"></a>Tam yedekleme

Kullanıcı verileri dışında, işletim sistemi dosyalarının ve kritik birimlerdeki tüm verilerin yedeklenmesi. Tanım olarak, tam yedekleme bir sistem durumu yedeklemesi içerir. Bir bilgisayar başlamadığınızda ve her şeyi kurtarmanız gerektiğinde koruma sağlar. [Daha fazla bilgi edinin](backup-mabs-system-state-and-bmr.md).

## <a name="backup-extensions--vm-extensions"></a>Yedekleme uzantıları/VM uzantıları

(Azure VM iş yükü türüne özel)

Azure sanal makine (VM) uzantıları, dağıtım sonrası yapılandırma ve Azure VM'lerinde otomasyon görevleri sunan küçük uygulamalardır. Azure Backup, makinede çalışan Azure VM aracısına bir uzantı yükleyerek Azure VM 'lerini yedekler. Azure Backup bu uzantıları otomatik olarak yönetir ve kullanıcıların bu uzantıları el ile güncelleştirmesine gerek yoktur.

## <a name="backup-instance--backup-item"></a>Yedekleme örneği/yedekleme öğesi

Belirli bir yedekleme ve bekletme ilkesiyle bir kasaya yedeklenen veri kaynağı bir yedekleme örneği veya yedekleme öğesi oluşturur.

## <a name="backup-rule--backup-policy"></a>Yedekleme kuralı/yedekleme ilkesi

Yedekleme kuralı, bir DataSource üzerinde yedeklemelerin ne zaman ve ne sıklıkta gerçekleştirilmesi gerektiğini belirten Kullanıcı tanımlı bir kuraldır. Bazı iş yükü türlerinde, yedekleme ilkesi de veri kaynağına uygulanacak anlık görüntü yöntemini belirtmek için bir yol sağlar (tam, artımlı, fark). Yedekleme ilkeleri genellikle yedekleme kuralları ve bekletme kurallarının bir birleşimi olarak oluşturulur.

## <a name="backup-vault"></a>Yedekleme kasası

*Microsoft. DataProtection/Backupkaults* türünde bir Azure Resource Manager kaynağı. Şu anda yedekleme kasaları PostgreSQL için Azure veritabanlarını yedeklemek üzere kullanılır. [Yedekleme kasaları hakkında daha fazla bilgi edinin](backup-azure-recovery-services-vault-overview.md).

## <a name="bcdr-business-continuity-and-disaster-recovery"></a>BCDR (Iş sürekliliği ve olağanüstü durum kurtarma)

BCDR, uygulamaların ve iş yüklerinin planlanmış ve planlanmamış hizmet ya da Azure kesintileri sırasında en düşük iş kesintileriyle çalışmaya ve çalışır olmasını sağlamak için bir kuruluşun benimseme gereken bir dizi işlem içerir. Bir Sound BCDR stratejisi oluşturmanıza yardımcı olmak için Azure tarafından sunulan çeşitli hizmetler hakkında [daha fazla bilgi edinin](https://azure.microsoft.com/solutions/backup-and-disaster-recovery/#overview) .

## <a name="churn"></a>Karmaşası

Art arda iki yedekleme arasında yedeklenmekte olan verilerdeki değişiklik yüzdesi. Bunun nedeni, yeni verilerin eklenmesi veya mevcut verilerin değiştirilmesi veya silinmesi olabilir.

## <a name="crash-consistent-backup"></a>Kilitlenme tutarlı yedekleme

(İş yüküne özgü terim)

Kilitlenme tutarlı anlık görüntüler genellikle bir Azure VM yedekleme sırasında kapatılırsa oluşur. Yalnızca yedekleme sırasında diskte bulunan veriler yakalanır ve yedeklenir. [Daha fazla bilgi edinin](backup-azure-vms-introduction.md#snapshot-consistency).

## <a name="cross-region-restore-crr"></a>Bölgeler arası geri yükleme (CRR)

[Geri yükleme seçeneklerinden](backup-azure-arm-restore-vms.md#restore-options)biri olan çapraz bölge geri yükleme (CRR), yedekleme öğelerini bir [Azure eşlenmiş bölgesi](../best-practices-availability-paired-regions.md#what-are-paired-regions)olan ikincil bir bölgeye geri yüklemenize olanak tanır.

## <a name="data-box"></a>Veri kutusu

[Veri kutusu belgelerine](../databox/data-box-overview.md)bakın.

## <a name="datasource"></a>Veri kaynağı

Yedeklenmek üzere bir aday olan kaynak (Azure kaynağı, proxy-kaynak veya şirket içi kaynak). Örneğin, bir Azure VM veya bir Azure dosya paylaşımıdır.

## <a name="dpm-data-protection-manager"></a>DPM (Data Protection Manager)

(İş yüküne özgü terim)

[DPM belgelerine](/system-center/dpm/dpm-overview)bakın.

## <a name="expressroute"></a>ExpressRoute

[ExpressRoute belgelerine](../expressroute/expressroute-introduction.md)bakın.

## <a name="file-system-consistent-backup"></a>Dosya sistemiyle tutarlı yedekleme

(İş yüküne özgü terim)

Dosya sistemiyle tutarlı yedeklemeler, tüm dosyaların bir anlık görüntüsünü aynı anda alarak tutarlılık sağlar. [Daha fazla bilgi edinin](backup-azure-vms-introduction.md#snapshot-consistency).

## <a name="frontend-storage--source-size"></a>Ön uç depolama/kaynak boyutu

Bir veri kaynağı için Yedeklenecek verilerin boyutu. Bir veri kaynağının ön uç boyutu, [korunan örnek](#protected-instance) sayısını belirler.

## <a name="full-backup"></a>Tam yedekleme

Tam yedeklemelerde tüm veri kaynağının bir kopyası her yedekleme için depolanır.

## <a name="gfs-backup-policy"></a>GFS yedekleme Ilkesi

Bir GFS (büyükbabalar-son) yedekleme ilkesi, günlük yedekleme zamanlamalarına ek olarak haftalık, aylık ve yıllık yedekleme zamanlamaları tanımlamanızı sağlayan bir ilkedir. Haftalık yedeklemeler "dönemleri" olarak bilinir, aylık yedeklemeler "fakler" olarak bilinir ve yıllık yedeklemeler "her ne kadar sık kullanılanlar" olarak bilinir. Bu yedek kopya kümelerinin her biri farklı süreler boyunca tutulacak şekilde yapılandırılabilir ve bu da yedek kopyaların bekletme seçeneklerinin daha fazla özelleştirilmesini sağlar. GFS ilkeleri, yedeklemeleri uzun bir süre boyunca daha fazla depolama verimli şekilde saklamak için faydalıdır.

## <a name="iaas-vms--azure-vms"></a>IaaS VM 'Leri/Azure VM 'Leri

[Azure VM belgeleri](../virtual-machines/index.yml)bölümüne bakın.

## <a name="incremental-backup"></a>Artımlı yedekleme

Artımlı yedeklemeler yalnızca önceki yedeklemeden bu yana değişmiş olan blokları depolar.

## <a name="instant-restore"></a>Anında geri yükleme

(İş yüküne özgü terim) Anında geri yükleme, kasadaki anlık görüntünün kopyasından değil, bir makineyi doğrudan yedekleme anlık görüntüsünden geri yüklemeyi içerir. Anında geri yüklemeler bir kasadan geri yükleme işleminden daha hızlıdır. Kullanılabilir anlık geri yükleme noktalarının sayısı, anlık görüntüler için yapılandırılan saklama süresine bağlıdır. Şu anda yalnızca Azure VM yedeklemesi için geçerlidir.

## <a name="iops"></a>IOPS

Saniye başına giriş/çıkış işlemi.

## <a name="item-level-restore"></a>Öğe düzeyinde geri yükleme

(İş yüküne özgü terim)

Makinenin içindeki tek tek dosyalar veya klasörler kurtarma noktasından geri yükleniyor.

## <a name="job"></a>İş

Bir kullanıcı veya Azure Backup hizmeti tarafından oluşturulan, yedeklemeyle ilgili bir görev. İşler, zamanlanmış ya da isteğe bağlı (geçici) olabilir. Farklı türlerde iş vardır-yedekleme, geri yükleme, korumayı yapılandırma vb. [İşler hakkında daha fazla bilgi edinin](backup-azure-monitoring-built-in-monitor.md#backup-jobs-in-recovery-services-vault).

## <a name="mabs--azure-backup-server"></a>MABS/Azure Backup Sunucusu

(İş yüküne özgü terim)

Azure Backup Sunucusu ile, Hyper-V VM 'Leri, Microsoft SQL Server, SharePoint Server, Microsoft Exchange ve Windows istemcileri gibi uygulama iş yüklerini tek bir konsoldan koruyabilirsiniz. DPM 'den iş yükü yedekleme işlevselliğinin çoğunu devralır, ancak birkaç farklılık vardır. [Daha fazla bilgi edinin](backup-azure-microsoft-azure-backup.md)

## <a name="managed-disks"></a>Yönetilen diskler

[Yönetilen diskler belgesine](../virtual-machines/managed-disks-overview.md)bakın.

## <a name="mars-agent"></a>MARS Aracısı

(İş yüküne özgü terim)

**Azure Backup Aracısı** veya **Kurtarma Hizmetleri Aracısı** olarak da bilinen Mars Aracısı, şirket Içi makinelerden ve Azure VM 'lerinden verileri Azure 'Daki bir yedek kurtarma hizmetleri kasasına yedeklemek için Azure Backup tarafından kullanılır. [Daha fazla bilgi edinin](backup-support-matrix-mars-agent.md).

## <a name="nsg-network-security-group"></a>NSG (ağ güvenlik grubu)

[NSG belgelerine](../virtual-network/network-security-groups-overview.md)bakın.

## <a name="offline-seeding"></a>Çevrimdışı dengeli dağıtım

Çevrimdışı dengeli dağıtım, ağ bant genişliği kullanılmadan önce ilk (tam) yedeklemeyi çevrimdışı aktarma sürecine başvurur. Yedekleme verilerini fiziksel depolama cihazlarına kopyalamak için bir mekanizma sağlar, bu daha sonra yakın bir Azure veri merkezine gönderilir ve bir kurtarma hizmetleri kasasında karşıya yüklenir. [Daha fazla bilgi edinin](offline-backup-overview.md).

## <a name="on-demand-backup--ad-hoc-backup"></a>İsteğe bağlı yedekleme/geçici yedekleme

Bir kullanıcı tarafından tek seferlik bir gereksinim temelinde tetiklenen ve kaynak için yapılandırılmış yedekleme zamanlaması (ilke) temelinde olmayan bir yedekleme işi.

## <a name="original-location-recovery-olr"></a>Özgün konum kurtarma (OLR)

Geri yükleme noktasından, yedeklemelerin alındığı kaynak konuma, kurtarma noktasında depolanan durum değiştirilerek yapılan bir kurtarma. Azure VM yedeklemesi kullanılırken bu, VM 'nin yedeklemelerin alındığı orijinal sunucuya geri yüklenmesi anlamına gelir. Azure dosya paylaşma yedeklemesi kullanılırken bu, verileri yedeklenen dosya paylaşımında geri yükleme anlamına gelir.

## <a name="passphrase"></a>Deyimi

(İş yüküne özgü terim)

Bir parola, şirket içi veya yerel makinenizi, MARS aracısını veya Azure 'dan veya Azure 'dan yedekleme sırasında verileri şifrelemek ve şifrelerini çözmek için kullanılır.

## <a name="private-endpoint"></a>Özel uç nokta

[Özel uç nokta belgelerine](../private-link/private-endpoint-overview.md)bakın.

## <a name="protected-instance"></a>Korumalı örnek

Korumalı örnek, Azure 'a yedeklemeyi yapılandırmak için kullandığınız bilgisayarı, fiziksel veya sanal sunucuyu ifade eder.  Bir **Faturalandırma açısından**, bir makine Için korunan örnek sayısı, ön uç boyutunun bir işlevidir. Bu nedenle, tek bir yedekleme örneği (Azure 'a yedeklenen bir VM gibi), ön uç boyutuna bağlı olarak birden çok korumalı örneğe karşılık gelir. [Daha fazla bilgi edinin](https://azure.microsoft.com/pricing/details/backup/).

## <a name="rbac-role-based-access-control"></a>RBAC (rol tabanlı erişim denetimi)

[RBAC belgelerini](../role-based-access-control/overview.md)inceleyin.

## <a name="recovery-point-restore-point-retention-point--point-in-time-pit"></a>Kurtarma noktası/geri yükleme noktası/bekletme noktası/zaman aşımı (PIT)

Yedeklenmekte olan özgün verilerin bir kopyası. Bir saklama noktası bir zaman damgasıyla ilişkilendirilir, bu sayede bir öğeyi belirli bir noktaya geri yüklemek için kullanabilirsiniz.

## <a name="recovery-services-vault"></a>Kurtarma Hizmetleri kasası

*Microsoft. RecoveryServices/kaults* türünde bir Azure Resource Manager kaynağı. Şu anda, kurtarma hizmetleri kasaları şu iş yüklerini yedeklemek için kullanılır: Azure VM 'leri, Azure VM 'lerinde SQL, Azure VM 'lerinde SAP HANA ve Azure dosya paylaşımları. Ayrıca, MARS, Azure Backup Sunucusu (MABS) ve System Center DPM kullanarak şirket içi iş yüklerini yedeklemek için de kullanılır. [Kurtarma Hizmetleri kasaları hakkında daha fazla bilgi edinin](backup-azure-recovery-services-vault-overview.md).

## <a name="resource-group"></a>Kaynak grubu

[Azure Resource Manager belgelerine](../azure-resource-manager/management/manage-resource-groups-portal.md#what-is-a-resource-group)bakın.

## <a name="rest-api"></a>REST API

[Azure REST API belgelerine](/rest/api/azure/)bakın.

## <a name="retention-rule"></a>Bekletme kuralı

Yedeklemelerin ne kadar süreyle tutulacağını belirten Kullanıcı tanımlı bir kural.

## <a name="rpo-recovery-point-objective"></a>RPO (kurtarma noktası hedefi)

RPO, bir veri kaybı senaryosunda mümkün olan en büyük veri kaybını gösterir. Bu, yedekleme sıklığıyla belirlenir.

## <a name="rto-recovery-time-objective"></a>RTO (kurtarma süresi hedefi)

RTO, verilerin bir veri kaybı senaryosunda son kullanılabilir noktaya geri yüklenebileceği maksimum süreyi gösterir.

## <a name="scheduled-backup"></a>Zamanlanan yedekleme

Verilen öğe için yapılandırılan yedekleme ilkesi tarafından otomatik olarak tetiklenen bir yedekleme işi.

## <a name="secondary-region--paired-region"></a>İkincil bölge/eşleştirilmiş bölge

Bölgesel bir çift aynı coğrafya içindeki iki bölgeden oluşur. Biri birincil bölgedir ve diğeri ikincil bölgedir. Eşleştirilmiş bölgeler, bazı Azure Hizmetleri (GRS ayarlarıyla Azure Backup dahil) tarafından, iş sürekliliği ve veri kaybına karşı koruma sağlamak için kullanılır. [Daha fazla bilgi edinin](../best-practices-availability-paired-regions.md).

## <a name="soft-delete"></a>Geçici silme

Geçici silme, yedekleme verilerinin yanlışlıkla silinmesine karşı koruma sağlamaya yardımcı olan bir özelliktir. Geçici bir aktör bir yedeği silse (veya yedekleme verileri yanlışlıkla silinse), yedekleme verileri ek bir süre boyunca tutulur ve bu yedekleme öğesinin veri kaybı olmadan kurtarılmasını sağlar. [Daha fazla bilgi edinin](backup-azure-security-feature-cloud.md).

## <a name="snapshot"></a>Anlık Görüntü

Anlık görüntü, bir sanal sabit sürücünün (VHD) veya Azure dosya paylaşımının tam, salt okunurdur bir kopyasıdır. [Disk anlık görüntüleri](../virtual-machines/windows/snapshot-copy-managed-disk.md) ve [dosya anlık görüntüleri](../storage/files/storage-snapshots-files.md)hakkında daha fazla bilgi edinin.

## <a name="storage-account"></a>Depolama hesabı

[Depolama hesabı belgelerine](../storage/common/storage-account-overview.md)bakın.

## <a name="subscription"></a>Abonelik

Azure aboneliği, Azure’da kaynakların sağlanması için kullanılan mantıksal bir kapsayıcıdır. Sanal makineler (VM 'Ler), veritabanları ve daha fazlası gibi tüm kaynaklarınızın ayrıntılarını barındırır.

## <a name="system-state-backup"></a>Sistem durumu yedeklemesi

(İş yüküne özgü terim)

İşletim sistemi dosyalarını yedekler. Bu yedekleme, bir bilgisayar başladığında kurtarmanızı sağlar, ancak sistem dosyaları ve kayıt defteri kaybedilir. [Daha fazla bilgi edinin](backup-mabs-system-state-and-bmr.md).

## <a name="tenant"></a>Kiracı

Kiracı, bir kuruluşu temsil eden birimdir. Kuruluş veya uygulama geliştirici Microsoft'la bir ilişki oluşturduğunda, örneğin Azure'a, Microsoft Intune'a veya Microsoft 365'e kaydolduğunda kuruluşun veya uygulama geliştiricinin aldığı özel bir Azure AD örneğidir.

## <a name="unmanaged-disk"></a>Yönetilmeyen disk

[Yönetilmeyen diskler belgesine](../storage/common/storage-disaster-recovery-guidance.md#azure-unmanaged-disks)bakın.

## <a name="vault"></a>Kasa

Azure 'da yedekleme verilerinin barındırıldığı bir depolama varlığı. Ayrıca, RBAC ve faturalandırma birimi de vardır. Şu anda iki tür kasa ve kurtarma hizmetleri kasası ve yedekleme kasası vardır.

## <a name="vault-credentials"></a>Kasa kimlik bilgileri

Kasa kimlik bilgileri dosyası, her kasa için Portal tarafından oluşturulan bir sertifikadır. Bu, bir şirket içi sunucu kasayla kaydedilirken kullanılır. [Daha fazla bilgi edinin](backup-azure-dpm-introduction.md).

## <a name="vnet-virtual-network"></a>VNET (sanal ağ)

[VNET belgelerini](../virtual-network/virtual-networks-overview.md)inceleyin.

## <a name="vss-windows-volume-shadow-copy-service"></a>VSS (Windows Birim Gölge Kopyası Hizmeti)

[VSS belgelerine](/windows-server/storage/file-server/volume-shadow-copy-service)bakın.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Backup genel bakış](backup-overview.md)
- [Azure Backup mimarisi ve bileşenler](backup-architecture.md)