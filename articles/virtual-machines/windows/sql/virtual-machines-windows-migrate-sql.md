---
title: VM'de SQL Server veritabanını SQL Server'a geçirme | Microsoft Dokümanlar
description: Bir Azure sanal makinesinde şirket içi bir kullanıcı veritabanını SQL Server'a nasıl geçirebilirsiniz hakkında bilgi edinin.
services: virtual-machines-windows
documentationcenter: ''
author: MashaMSFT
manager: craigg
editor: ''
tags: azure-service-management
ms.assetid: 00fd08c6-98fa-4d62-a3b8-ca20aa5246b1
ms.service: virtual-machines-sql
ms.workload: iaas-sql-server
ms.tgt_pltfrm: vm-windows-sql-server
ms.topic: article
ms.date: 08/18/2018
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: c8314b04c05e2ecba2715b807171b5c1a2fa988a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75646872"
---
# <a name="migrate-a-sql-server-database-to-sql-server-in-an-azure-vm"></a>Bir SQL Server veritabanını Azure VM’deki SQL Server’a geçirme

Bir Azure VM'de şirket içi SQL Server kullanıcı veritabanını SQL Server'a geçirmek için birkaç yöntem vardır. Bu makalede kısaca çeşitli yöntemler ele alınacak ve çeşitli senaryolar için en iyi yöntem önerecektir.


[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

  > [!NOTE]
  > SQL Server 2008 ve SQL Server 2008 R2, şirket içi durumlar için [destek yaşam döngülerinin sonuna](https://www.microsoft.com/sql-server/sql-server-2008) yaklaşıyor. Desteği genişletmek için, SQL Server örneğini bir Azure VM'sine geçirebilirsiniz veya şirket içinde tutmak için Genişletilmiş Güvenlik Güncelleştirmeleri satın alabilirsiniz. Daha fazla bilgi için bkz: [Azure ile SQL Server 2008 ve 2008 R2 desteğini genişlet](virtual-machines-windows-sql-server-2008-eos-extend-support.md)

## <a name="what-are-the-primary-migration-methods"></a>Birincil geçiş yöntemleri nelerdir?
Birincil geçiş yöntemleri şunlardır:

* Sıkıştırmayı kullanarak şirket içi yedekleme gerçekleştirin ve yedekleme dosyasını Azure sanal makinesine el ile kopyalayın
* URL'ye yedekleme yapın ve URL'den Azure sanal makinesine geri yükleme
* Verileri ayırın ve dosyaları Azure blob depolamasına kopyalayın ve ardından URL'den Azure VM'deki SQL Server'a takın
* Şirket içi fiziksel makineyi Hyper-V VHD'ye dönüştürün, Azure Blob depolama alanına yükleyin ve yüklenen VHD'yi kullanarak yeni VM olarak dağıtın
* Windows Alma/Dışa Aktarma Hizmeti kullanarak sabit diski gönderme
* Şirket içinde Bir AlwaysOn Kullanılabilirlik Grubu dağıtımınız varsa, Azure'da bir yineleme oluşturmak ve ardından başarısız olmak için [Azure Yineleme](../sqlclassic/virtual-machines-windows-classic-sql-onprem-availability.md) Ekle Sihirbazı'nı kullanın ve ardından başarısız olun ve kullanıcıları Azure veritabanı örneğine işaret edin
* Azure SQL Server örneğini abone olarak yapılandırmak ve ardından çoğaltmayı devre dışı bırakıp kullanıcıları Azure veritabanı örneğine işaret etmek için SQL Server [işlem çoğaltmasını](https://msdn.microsoft.com/library/ms151176.aspx) kullanın

> [!TIP]
> Veritabanlarını Azure'daki SQL Server VM'ler arasında taşımak için de aynı teknikleri kullanabilirsiniz. Örneğin, bir SQL Server galeri görüntüsü VM'yi bir sürümden/sürümden diğerine yükseltmenin desteklenen bir yolu yoktur. Bu durumda, yeni sürüm/sürüm ile yeni bir SQL Server VM oluşturmanız ve veritabanlarınızı taşımak için bu makaledeki geçiş tekniklerinden birini kullanmanız gerekir. 

## <a name="choosing-your-migration-method"></a>Geçiş yönteminizi seçme
En iyi veri aktarım performansı için, sıkıştırılmış bir yedekleme dosyası kullanarak veritabanı dosyalarını Azure VM'ye geçirin.

Veritabanı geçiş işlemi sırasında kapalı kalma süresini en aza indirmek için AlwaysOn seçeneğini veya işlem çoğaltma seçeneğini kullanın.

Yukarıdaki yöntemleri kullanmak mümkün değilse, veritabanınızı el ile geçirin. Bu yöntemi kullanarak, genellikle bir veritabanı yedeklemesi ve ardından veritabanı yedeklemesinin Azure'a bir kopyasını kullanarak başlar sınız ve ardından bir veritabanı geri yüklemesi gerçekleştirebilirsiniz. Ayrıca veritabanı dosyalarını Azure'a kopyalayıp ekleyebilirsiniz. Veritabanını Azure VM'ye geçirme işlemini gerçekleştirebileceğiniz çeşitli yöntemler vardır.

> [!NOTE]
> SQL Server'ın eski sürümlerinden SQL Server 2014 veya SQL Server 2016'ya yükselttiyseniz, değişikliklerin gerekli olup olmadığını göz önünde bulundurmalısınız. Geçiş projenizin bir parçası olarak SQL Server'ın yeni sürümü tarafından desteklenmeyen özelliklere olan tüm bağımlılıkları gidermenizi öneririz. Desteklenen sürümler ve senaryolar hakkında daha fazla bilgi için sql [server'a yükseltme](https://msdn.microsoft.com/library/bb677622.aspx)konusuna bakın.

Aşağıdaki tabloda birincil geçiş yöntemlerinin her biri listelenir ve her yöntemin kullanımının en uygun olduğu zaman açıklanır.

| Yöntem | Kaynak veritabanı sürümü | Hedef veritabanı sürümü | Kaynak veritabanı yedekleme boyutu kısıtlaması | Notlar |
| --- | --- | --- | --- | --- |
| [Sıkıştırmayı kullanarak şirket içi yedekleme gerçekleştirin ve yedekleme dosyasını Azure sanal makinesine el ile kopyalayın](#backup-and-restore) |SQL Server 2005 veya üzeri |SQL Server 2005 veya üzeri |[Azure VM depolama sınırı](https://azure.microsoft.com/documentation/articles/azure-resource-manager/management/azure-subscription-service-limits/) | Bu, veritabanlarını makineler arasında taşımak için çok basit ve iyi test edilmiş bir tekniktir. |
| [URL'ye yedekleme yapın ve URL'den Azure sanal makinesine geri yükleme](#backup-to-url-and-restore) |SQL Server 2012 SP1 CU2 veya üzeri |SQL Server 2012 SP1 CU2 veya üzeri |SQL Server 2016 için 12,8 TB <, aksi takdirde 1 TB < | Bu yöntem, yedekleme dosyasını Azure depolama alanını kullanarak VM'ye taşımanın başka bir yoludur. |
| [Verileri ayırın ve dosyaları Azure blob depolamasına kopyalayın ve ardından URL'den Azure sanal makinesindeki SQL Server'a takın](#detach-and-attach-from-url) |SQL Server 2005 veya üzeri |SQL Server 2014 veya üzeri |[Azure VM depolama sınırı](https://azure.microsoft.com/documentation/articles/azure-resource-manager/management/azure-subscription-service-limits/) |[Bu dosyaları Azure Blob depolama hizmetini kullanarak depolamayı](https://msdn.microsoft.com/library/dn385720.aspx) ve özellikle çok büyük veritabanlarıyla Azure VM'de çalışan SQL Server'a eklemeyi planlarken bu yöntemi kullanın |
| [Şirket içi makineyi Hyper-V VHD'lere dönüştürün, Azure Blob depolama alanına yükleyin ve yüklenen VHD'yi kullanarak yeni bir sanal makine dağıtın](#convert-to-vm-and-upload-to-url-and-deploy-as-new-vm) |SQL Server 2005 veya üzeri |SQL Server 2005 veya üzeri |[Azure VM depolama sınırı](https://azure.microsoft.com/documentation/articles/azure-resource-manager/management/azure-subscription-service-limits/) |Sql Server'ın eski bir sürümünde çalıştıracağınız bir veritabanını aktarırken veya diğer kullanıcı veritabanlarına ve/veya sistem veritabanlarına bağlı veritabanı geçişinin bir parçası olarak sistem ve kullanıcı veritabanlarını birlikte aktarırken [kendi SQL Server lisansınızı getirirken](../../../sql-database/sql-database-paas-vs-sql-server-iaas.md)kullanın. |
| [Windows Alma/Dışa Aktarma Hizmeti kullanarak sabit diski gönderme](#ship-hard-drive) |SQL Server 2005 veya üzeri |SQL Server 2005 veya üzeri |[Azure VM depolama sınırı](https://azure.microsoft.com/documentation/articles/azure-resource-manager/management/azure-subscription-service-limits/) |Çok büyük veritabanlarında olduğu gibi manuel kopyalama yöntemi çok yavaşolduğunda [Windows İçe Alma/Dışa Aktarma Hizmeti'ni](../../../storage/common/storage-import-export-service.md) kullanma |
| [Azure Çoğaltma Ekle Sihirbazı'nı kullanma](../sqlclassic/virtual-machines-windows-classic-sql-onprem-availability.md) |SQL Server 2012 veya üzeri |SQL Server 2012 veya üzeri |[Azure VM depolama sınırı](https://azure.microsoft.com/documentation/articles/azure-resource-manager/management/azure-subscription-service-limits/) |Kapalı kalma süresini en aza indirir, her zaman şirket içi dağıtım yaptığınızda kullanın |
| [SQL Server işlem çoğaltma kullanma](https://msdn.microsoft.com/library/ms151176.aspx) |SQL Server 2005 veya üzeri |SQL Server 2005 veya üzeri |[Azure VM depolama sınırı](https://azure.microsoft.com/documentation/articles/azure-resource-manager/management/azure-subscription-service-limits/) |Kapalı kalma süresini en aza indirmeniz gerektiğinde ve Her Zaman Şirket içi dağıtıma sahip olmadığınız zaman kullanın |

## <a name="backup-and-restore"></a>Yedekleme ve geri yükleme
Veritabanınızı sıkıştırma ile yedekleyin, yedeklemeyi VM'ye kopyalayın ve veritabanını geri yükleyin. Yedekleme dosyanız 1 TB'den büyükse, vm diskinin maksimum boyutu 1 TB olduğundan onu şeritlemeniz gerekir. Bu el ile yöntemi kullanarak bir kullanıcı veritabanını geçirmek için aşağıdaki genel adımları kullanın:

1. Şirket içi bir konuma tam bir veritabanı yedeklemesi gerçekleştirin.
2. İstediğiniz SQL Server sürümüyle sanal bir makine oluşturun veya yükleyin.
3. Gereksinimlerinize göre kurulum bağlantısı. Bkz. [Azure'da BIR SQL Server Sanal Makinesine Bağlan (Kaynak Yöneticisi)](virtual-machines-windows-sql-connect.md).
4. Uzak masaüstü, Windows Explorer veya bir komut isteminden kopyalama komutunu kullanarak yedek dosyanızı(lar) VM'nize kopyalayın.

## <a name="backup-to-url-and-restore"></a>URL'ye yedekleme ve geri yükleme
Yerel bir dosyaya yedeklemek yerine, [yedeklemeyi URL'ye](https://msdn.microsoft.com/library/dn435916.aspx) kullanabilir ve ardından URL'den VM'ye geri yükleyebilirsiniz. SQL Server 2016 ile çizgili yedekleme kümeleri desteklenir, performans için önerilir ve blob başına boyut sınırlarını aşması gerekir. Çok büyük veritabanları için Windows [İçe Aktarma/Verme Hizmeti'nin](../../../storage/common/storage-import-export-service.md) kullanılması önerilir.

## <a name="detach-and-attach-from-url"></a>URL'den ayırın ve iliştirin
Veritabanınızı ayırın ve dosyalarınızı kaydedin ve [Azure Blob depolamaalanına](https://msdn.microsoft.com/library/dn385720.aspx)aktarın. Ardından veritabanını AZURE VM'nize URL'den takın. Fiziksel veritabanı dosyalarının Blob depolama alanında ikamet etmesini istiyorsanız bunu kullanın. Bu, çok büyük veritabanları için yararlı olabilir. Bu el ile yöntemi kullanarak bir kullanıcı veritabanını geçirmek için aşağıdaki genel adımları kullanın:

1. Veritabanı dosyalarını şirket içi veritabanı örneğinden ayırın.
2. [AZCopy komut satırı yardımcı programını](../../../storage/common/storage-use-azcopy.md)kullanarak ayrılmış veritabanı dosyalarını Azure blob depolama alanına kopyalayın.
3. Veritabanı dosyalarını Azure URL'sinden Azure VM'deki SQL Server örneğine takın.

## <a name="convert-to-vm-and-upload-to-url-and-deploy-as-new-vm"></a>VM'ye dönüştürüp URL'ye yükleme ve yeni VM olarak dağıtma
Şirket içi SQL Server örneğindeki tüm sistem ve kullanıcı veritabanlarını Azure sanal makinesine geçirmek için bu yöntemi kullanın. Bu el ile yöntemi kullanarak tüm SQL Server örneğini geçirmek için aşağıdaki genel adımları kullanın:

1. Fiziksel veya sanal makineleri Hyper-V VHD'lere dönüştürün.
2. [Ekle-AzureVHD cmdlet'i](https://msdn.microsoft.com/library/windowsazure/dn495173.aspx)kullanarak VHD dosyalarını Azure Depolama'ya yükleyin.
3. Yüklenen VHD'yi kullanarak yeni bir sanal makine dağıtın.

> [!NOTE]
> Tüm uygulamayı geçirmek için [Azure Site Kurtarma](../../../site-recovery/site-recovery-overview.md)]'ni kullanmayı düşünün.

## <a name="ship-hard-drive"></a>Gemi sabit diski
Ağ üzerinden yüklemenin son derece pahalı veya mümkün olmadığı durumlarda büyük miktarda dosya verilerini Azure Blob depolamasına aktarmak için [Windows İçe Alma/Dışa Aktarma Hizmeti yöntemini](../../../storage/common/storage-import-export-service.md) kullanın. Bu hizmetle, bu verileri içeren bir veya daha fazla sabit diski, verilerinizin depolama hesabınıza yüklendiği bir Azure veri merkezine gönderirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Azure Sanal Makinelerde SQL Server'ı çalıştırma hakkında daha fazla bilgi için [Azure Sanal Makineler'de SQL Server'a genel bakış](virtual-machines-windows-sql-server-iaas-overview.md)bakın.

> [!TIP]
> SQL Server sanal makineleri hakkında sorularınız olursa [Sık Sorulan Sorular](virtual-machines-windows-sql-server-iaas-faq.md) bölümüne bakın.

Yakalanan bir görüntüden Bir Azure SQL Server Virtual Machine oluşturma yla ilgili talimatlar için, CSS SQL Server Engineers blogunda [yakalanan resimlerden Azure SQL sanal makinelerini 'klonlama' konulu İpuçları & Hileler'e](https://blogs.msdn.microsoft.com/psssql/2016/07/06/tips-tricks-on-cloning-azure-sql-virtual-machines-from-captured-images/) bakın.

