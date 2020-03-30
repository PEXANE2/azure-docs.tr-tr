---
title: Yönetim maliyetleri & fiyat rehberliği
description: Doğru SQL Server sanal makine fiyatlandırma modelini seçmek için en iyi uygulamaları sağlar.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-sql
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 08/09/2018
ms.author: mathoma
ms.reviewer: jroth
ms.custom: seo-lt-2019
ms.openlocfilehash: b7a8f7136d0899ab3afe04bce2d25bc2e7f8d3e2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75981722"
---
# <a name="pricing-guidance-for-azure-sql-server-vms"></a>Azure SQL Server VM'leri için fiyatlandırma yönergeleri

Bu makalede, [Azure'daki SQL Server sanal makineleri](virtual-machines-windows-sql-server-iaas-overview.md) için fiyatlandırma kılavuzu sağlanmaktadır. Maliyeti etkileyen çeşitli seçenekler vardır ve maliyetleri iş gereksinimleriyle dengeleyen doğru görüntüyü seçmek önemlidir.

> [!TIP]
> Yalnızca SQL Server sürümü ve sanal makine boyutunun belirli bir birleşimi için bir maliyet tahmini bulmanız gerekiyorsa, [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows) veya [Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux)için fiyatlandırma sayfasına bakın. **OS/Yazılım** listesinden platformunuzu ve SQL Server sürümünüzü seçin.
>
> ![VM Fiyatlandırma sayfasında UI](./media/virtual-machines-windows-sql-server-pricing-guidance/virtual-machines-pricing-ui.png)
>
> Veya sanal bir makine eklemek ve yapılandırmak için [fiyatlandırma hesaplayıcısını](https://azure.microsoft.com/pricing/#explore-cost) kullanın. 

## <a name="free-licensed-sql-server-editions"></a>Serbest lisanslı SQL Server sürümleri

Kavram kanıtı geliştirmek, sınamak veya oluşturmak istiyorsanız, serbestçe lisanslı **SQL Server Developer sürümünü**kullanın. Bu sürüm, her tür uygulamayı oluşturmanıza ve test edebilmenize olanak tanıyan SQL Server Enterprise sürümünün tüm özelliklerine sahiptir. Ancak, geliştirici sürümünü üretimde çalıştıramazsınız. Bir SQL Server Developer sürümü VM, ilişkili SQL Server lisanslama maliyetleri olmadığından, yalnızca VM maliyeti için ücrete tabidir.

Üretimde hafif bir iş yükü çalıştırmak istiyorsanız (<4 çekirdek, <1 GB bellek, <10 GB/veritabanı) serbest lisanslı **SQL Server Express sürümünü**kullanın. Bir SQL Server Express sürümü VM de sadece VM maliyeti için ücrete tabidir.

Bu geliştirme/test ve hafif üretim iş yükleri için, bu iş yüklerine uyan daha küçük bir VM boyutu seçerek de tasarruf edebilirsiniz. DS1v2 bazı senaryolarda iyi bir seçim olabilir.

Bu resimlerden biriyle bir SQL Server 2017 Azure VM oluşturmak için aşağıdaki bağlantılara bakın:

| Platform | Serbest lisanslı görüntüler |
|---|---|
| Windows Server 2016 | [SQL Server 2017 Geliştirici Azure VM](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017DeveloperonWindowsServer2016)<br/>[SQL Server 2017 Express Azure VM](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017ExpressonWindowsServer2016) |
| Red Hat Enterprise Linux | [SQL Server 2017 Geliştirici Azure VM](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017DeveloperonRedHatEnterpriseLinux74)<br/>[SQL Server 2017 Express Azure VM](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017ExpressonRedHatEnterpriseLinux74) |
| SUSE Linux Enterprise Server | [SQL Server 2017 Geliştirici Azure VM](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017DeveloperonSLES12SP2)<br/>[SQL Server 2017 Express Azure VM](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017ExpressonSLES12SP2) |
| Ubuntu | [SQL Server 2017 Geliştirici Azure VM](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017DeveloperonUbuntuServer1604LTS)<br/>[SQL Server 2017 Express Azure VM](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017ExpressonUbuntuServer1604LTS) |

## <a name="paid-sql-server-editions"></a>Ücretli SQL Server sürümleri

Hafif olmayan bir üretim iş yükünüz varsa, aşağıdaki SQL Server sürümlerinden birini kullanın:

| SQL Server Sürümü | İş yükü |
|-----|-----|
| Web | Küçük web siteleri |
| Standart | Küçük ve orta iş yükleri |
| Enterprise | Büyük veya görev açısından kritik iş yükleri|

Bu sürümler için SQL Server lisanslama için ödeme yapmak için iki seçeneğiniz vardır: *kullanım başına ödeme* yapmak veya kendi *lisansınızı getirmek (BYOL)*.

## <a name="pay-per-usage"></a>Kullanım başına ödeme

**Kullanım başına SQL Server lisansı ödemek** (gittiğiniz gibi **ödeme**olarak da bilinir) Azure VM'yi çalıştırmanın saniye başına maliyetinin SQL Server lisansının maliyetini içerdiği anlamına gelir. [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows) veya [Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux)için Azure VM fiyatlandırma sayfasında farklı SQL Server sürümlerinin (Web, Standart, Kurumsal) fiyatlandırmasını görebilirsiniz.

Maliyet, SQL Server'ın (2012 SP3-2017) tüm sürümleri için aynıdır. Saniye başına lisans lama maliyeti VM vCPUs sayısına bağlıdır.

Kullanım başına SQL Server lisansödemesi aşağıdakiler için önerilir:

- **Geçici veya periyodik iş yükleri.** Örneğin, bir etkinliği her yıl birkaç ay boyunca desteklemesi gereken bir uygulama veya Pazartesi günleri iş analizi.

- **Bilinmeyen kullanım ömrü veya ölçekle iş yükleri.** Örneğin, birkaç ay içinde gerekli olmayan veya talebe bağlı olarak daha fazla veya daha az işlem gücü gerektirebilecek bir uygulama.

Bu gittikçe öde resimlerinden biriyle bir SQL Server 2017 Azure VM oluşturmak için aşağıdaki bağlantılara bakın:

| Platform | Lisanslı görüntüler |
|---|---|
| Windows Server 2016 | [SQL Server 2017 Web Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017WebonWindowsServer2016)<br/>[SQL Server 2017 Standart Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017StandardonWindowsServer2016)<br/>[SQL Server 2017 Kurumsal Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017EnterpriseWindowsServer2016) |
| Red Hat Enterprise Linux | [SQL Server 2017 Web Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017WebonRedHatEnterpriseLinux74)<br/>[SQL Server 2017 Standart Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017StandardonRedHatEnterpriseLinux74)<br/>[SQL Server 2017 Kurumsal Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017EnterpriseonRedHatEnterpriseLinux74) |
| SUSE Linux Enterprise Server | [SQL Server 2017 Web Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017WebonSLES12SP2)<br/>[SQL Server 2017 Standart Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017StandardonSLES12SP2)<br/>[SQL Server 2017 Kurumsal Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017EnterpriseonSLES12SP2) |
| Ubuntu | [SQL Server 2017 Web Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017WebonUbuntuServer1604LTS)<br/>[SQL Server 2017 Standart Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017StandardonUbuntuServer1604LTS)<br/>[SQL Server 2017 Kurumsal Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017EnterpriseonUbuntuServer1604LTS) |

> [!IMPORTANT]
> Portalda bir SQL Server sanal makinesi oluşturduğunuzda, **boyut seç** penceresi tahmini maliyeti gösterir. Bu tahminin, herhangi bir işletim sistemi lisanslama maliyetiyle birlikte (Windows veya üçüncü taraf Linux işletim sistemleri) vm'yi çalıştırmak için yalnızca bilgi işlem maliyetleri olduğunu unutmayın.
>
> ![VM boyutunda bıçak seçin](./media/virtual-machines-windows-sql-server-pricing-guidance/sql-vm-choose-size-pricing-estimate.png)
>
>Web, Standart ve Kurumsal sürümleri için ek SQL Server lisanslama maliyetlerini içermez. En doğru fiyatlandırma tahminini elde etmek için, [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) veya [Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux/)fiyatlandırma sayfasında işletim sisteminizi ve SQL Server sürümünüzü seçin.

> [!NOTE]
> Artık kendi lisansınızı (BYOL) getirip geri getirmek için lisans modelini kullanım başına ödemeden değiştirmek mümkündür. Daha fazla bilgi için, [SQL VM'nin lisans modelini nasıl değiştireceğiniz](virtual-machines-windows-sql-ahb.md)e bakın. 

## <a name="bring-your-own-license-byol"></a><a id="byol"></a>Kendi lisansınızı getirin (BYOL)

**BYOL**olarak da adlandırılan **License Mobility aracılığıyla kendi SQL Server lisansınızı getirmek,** bir Azure VM'de Yazılım Güvencesi ile mevcut bir SQL Server Toplu Lisansı kullanmak anlamına gelir. BYOL kullanan bir SQL Server VM, Toplu Lisanslama programı aracılığıyla lisans ve Yazılım Güvencesi edindiğiniz için, SQL Server lisanslama için değil, yalnızca VM'yi çalıştırma maliyeti için ücret alınır.

> [!IMPORTANT]
> BYOL görüntüleri, Yazılım Güvencesi ile kurumsal bir anlaşma gerektirir. Şu anda Azure Bulut Çözüm Ortağı'nın (CSP) bir parçası olarak kullanılamazlar. CSP müşterileri, istediğiniz kadar öde görüntüsünü dağıtarak ve ardından Azure Karma [Avantajı'nı](virtual-machines-windows-sql-ahb.md)etkinleştirerek kendi lisanslarını getirebilirler.

> [!NOTE]
> BYOL görüntüleri şu anda yalnızca Windows sanal makineleri için kullanılabilir. Ancak, SQL Server'ı yalnızca Linux'a özel bir VM'ye el ile yükleyebilirsiniz. [Linux SQL VM SSS'deki](../../linux/sql/sql-server-linux-faq.md)yönergelere bakın.

Lisans Mobilitesi aracılığıyla kendi SQL lisansınızı getirmek için önerilir:

- **Sürekli iş yükleri.** Örneğin, iş operasyonlarını 24x7 desteklemesi gereken bir uygulama.

- **Bilinen yaşam süresi ve ölçeği olan iş yükleri.** Örneğin, tüm yıl için gerekli olan ve talebin tahmin edildiği bir uygulama.

BYOL'u SQL Server VM ile kullanmak için, bazı toplu lisans programları ve başkalarıyla isteğe bağlı satın alma yoluyla gerekli bir seçenek olan SQL Server Standard veya Enterprise and [Software Assurance](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default.aspx#tab=1)için lisansa sahip olmalısınız. Toplu Lisanslama programları aracılığıyla sağlanan fiyatlandırma düzeyi, anlaşmanın türüne ve SQL Server'a olan miktar ve bağlılığa bağlı olarak değişir. Ancak kural olarak, sürekli üretim iş yükleri için kendi lisansınızı getirmek aşağıdaki avantajlara sahiptir:

| BYOL parası | Açıklama |
|-----|-----|
| **Maliyet tasarrufları** | [Azure Karma Avantajı](https://azure.microsoft.com/pricing/hybrid-benefit/) %55'e kadar tasarruf sağlar. Daha fazla bilgi için [bkz.](virtual-machines-windows-sql-ahb.md) |
| **Serbest pasif ikincil çoğaltma** | Kendi lisansınızı getirmenin bir diğer yararı da, yüksek kullanılabilirlik amacıyla SQL Server başına [bir pasif ikincil yineleme için ücretsiz lisanslamadır.](https://azure.microsoft.com/pricing/licensing-faq/) Bu, yüksek kullanılabilirlikteki bir SQL Server dağıtımının lisans lama maliyetini yarıya düşürür (örneğin, Her Zaman Kullanılabilirlik Grupları'nı kullanır). Pasif ikincil çalıştırma hakları Fail-Over Sunucular Yazılım Güvencesi avantajı aracılığıyla sağlanır. |

Bu kendi lisans getir görüntülerinden biriyle bir SQL Server 2017 Azure VM oluşturmak için "{BYOL}" ile önceden belirlenmiş VM'lere bakın:

- [SQL Server 2017 Kurumsal Azure VM](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2017EnterpriseWindowsServer2016)
- [SQL Server 2017 Standart Azure VM](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2017StandardonWindowsServer2016)

> [!IMPORTANT]
> Azure'da kaç SQL Server lisansı kullandığınızı 10 gün içinde bize bildirin. Önceki resimlere bağlantılar bunu yapmak için nasıl talimatlar var.

> [!NOTE]
> Artık kendi lisansınızı (BYOL) getirip geri getirmek için lisans modelini kullanım başına ödemeden değiştirmek mümkündür. Daha fazla bilgi için, [SQL VM'nin lisans modelini nasıl değiştireceğiniz](virtual-machines-windows-sql-ahb.md)e bakın. 



## <a name="reduce-costs"></a>Maliyetleri azaltma

Gereksiz maliyetleri önlemek için en uygun sanal makine boyutunu seçin ve sürekli olmayan iş yükleri için aralıklı kapatmaları düşünün.

### <a name="correctly-size-your-vm"></a><a id="machinesize"></a>VM'nizi doğru boyutlandırın

SQL Server'ın lisans maliyeti doğrudan vCPUs sayısıile ilgilidir. CPU, bellek, depolama ve G/Ç bant genişliği için beklenen gereksinimlerinize uyan bir VM boyutu seçin. Makine boyutu seçeneklerinin tam listesi için [Windows VM boyutları](https://docs.microsoft.com/azure/virtual-machines/windows/sizes) ve Linux [VM boyutlarına](https://docs.microsoft.com/azure/virtual-machines/linux/sizes?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)bakın.

Belirli TÜRDE SQL Server iş yükleriyle iyi çalışan yeni makine boyutları vardır. Bu makinelerin boyutları yüksek düzeyde bellek, depolama ve G/Ç bant genişliği sağlar, ancak daha düşük sanallaştırılmış çekirdek sayısına sahiptir. Örneğin, aşağıdaki örneği göz önünde bulundurun:

| VM Boyutu | Sanal çekirdek | Bellek | Maksimum Diskler | Max I/O Throughput | SQL lisanslama maliyetleri | Toplam maliyet (İşlem + lisanslama) |
|---|---|---|---|---|---|---|
| **Standard_DS14v2** | 16 | 112 GB | 32 | 51.200 IOPS veya 768 MB/sn | | |
| **Standard_DS14-4v2** | 4 | 112 GB | 32 | 51.200 IOPS veya 768 MB/sn | %75 daha düşük | %57 daha düşük |

> [!IMPORTANT]
> Bu bir nokta-in-time örnektir. En son özellikler için, makine boyutları makalelerine ve [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) ve [Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux/)için Azure fiyatlandırma sayfasına bakın.

Önceki örnekte, **Standard_DS14v2** ve **Standard_DS14-4v2** belirtimlerinin vCPUs'lar dışında aynı olduğunu görebilirsiniz. **Standard_DS14-4v2** makine boyutunun sonundaki **-4v2** soneki aktif vCPUs sayısını gösterir. SQL Server lisanslama maliyetleri vCPUs sayısına bağlı olduğundan, bu ekstra vCPUs'ların gerekli olmadığı senaryolarda VM maliyetini önemli ölçüde azaltır. Bu bir örnektir ve bu sonek deseni ile tanımlanan kısıtlı vCPUs ile birçok makine boyutları vardır. Daha fazla bilgi için, [daha uygun maliyetli veritabanı çalışmaları için yeni Azure VM boyutlarını duyuran](https://azure.microsoft.com/blog/announcing-new-azure-vm-sizes-for-more-cost-effective-database-workloads/)blog gönderisine bakın.

### <a name="shut-down-your-vm-when-possible"></a>Mümkün olduğunda VM'nizi kapatın

Sürekli olarak çalışmayan iş yükleri kullanıyorsanız, etkin olmayan dönemlerde sanal makineyi kapatmayı düşünün. Sadece kullandığınız kadar ödersiniz.

Örneğin, sql server'ı yalnızca bir Azure VM üzerinde deniyorsanız, yanlışlıkla haftalarca çalışır durumda bırakarak ücretlendirmeye maruz kalmak istemezsinuz. Bir çözüm otomatik [kapatma özelliğini](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/)kullanmaktır.

![SQL VM otomatik kapatma](./media/virtual-machines-windows-sql-server-pricing-guidance/sql-vm-auto-shutdown.png)

Otomatik kapatma, [Azure DevTest Labs](https://azure.microsoft.com/services/devtest-lab)tarafından sağlanan daha büyük benzer özellikler kümesinin bir parçasıdır.

Diğer iş akışları için Azure [Otomasyonu](https://azure.microsoft.com/services/automation/)gibi bir komut dosyası çözümüyle Azure VM'leri otomatik olarak kapatmayı ve yeniden başlatmayı düşünün.

> [!IMPORTANT]
> VM'nizi kapatmak ve ayırmak, ücretleri önlemenin tek yoludur. VM'yi kapatmak için güç seçeneklerini durdurmanız veya kullanmanız hala kullanım ücretlerine neden olur.

## <a name="next-steps"></a>Sonraki adımlar

Genel Azure fiyatlandırma kılavuzu [için](../../../cost-management-billing/manage/getting-started.md)bkz. SQL Server da dahil olmak üzere en son Sanal Makineler fiyatlandırması için [Windows VM'ler](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) ve [Linux VM'ler](https://azure.microsoft.com/pricing/details/virtual-machines/linux/)için Azure VM Azure fiyatlandırma sayfasına bakın.

Azure Sanal Makinelerde çalışan SQL Server'a genel bakış için aşağıdaki makalelere bakın:

- [Windows VM'lerde SQL Server'a Genel Bakış](virtual-machines-windows-sql-server-iaas-overview.md)
- [Linux VM'lere SQL Server'a Genel Bakış](../../linux/sql/sql-server-linux-virtual-machines-overview.md)
