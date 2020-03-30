---
title: Azure SSS'de Windows Sanal Makinelerde SQL Server | Microsoft Dokümanlar
description: Bu makalede, Azure VM'lerde SQL Server'ı çalıştırma yla ilgili sık sorulan soruların yanıtları verilmektedir.
services: virtual-machines-windows
documentationcenter: ''
author: MashaMSFT
manager: felixwu
editor: ''
tags: azure-service-management
ms.assetid: 2fa5ee6b-51a6-4237-805f-518e6c57d11b
ms.service: virtual-machines-sql
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 08/05/2019
ms.author: mathoma
ms.openlocfilehash: 3b73c329c3db54ba78db15ced8e919af4d4a45d7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79249743"
---
# <a name="frequently-asked-questions-for-sql-server-running-on-windows-virtual-machines-in-azure"></a>Azure'da Windows sanal makineleri üzerinde çalıştırılan SQL Server için sık sorulan sorular

> [!div class="op_single_selector"]
> * [Windows](virtual-machines-windows-sql-server-iaas-faq.md)
> * [Linux](../../linux/sql/sql-server-linux-faq.md)

Bu makalede, [Azure'da Windows Sanal Makinelerde SQL Server'ı](https://azure.microsoft.com/services/virtual-machines/sql-server/)çalıştırma yla ilgili en sık sorulan sorulardan bazılarına yanıt verilmektedir.

> [!NOTE]
> Bu makalede, Windows VM'lerde SQL Server'a özgü sorunlar üzerinde duruluyor. Linux VM'lerde SQL Server çalıştırıyorsanız, [Linux SSS](../../linux/sql/sql-server-linux-faq.md)bölümüne bakın.

[!INCLUDE [support-disclaimer](../../../../includes/support-disclaimer.md)]

## <a name="images"></a><a id="images"></a>Görüntü

1. **Hangi SQL Server sanal makine galerisi görüntüleri mevcuttur?** 

   Azure, hem Windows hem de Linux için tüm sürümlerde desteklenen tüm büyük SQL Server sürümleri için sanal makine görüntüleri tutar. Daha fazla bilgi için [Windows VM görüntülerinin](virtual-machines-windows-sql-server-iaas-overview.md#payasyougo) ve Linux [VM görüntülerinin](../../linux/sql/sql-server-linux-virtual-machines-overview.md#create)tam listesine bakın.

1. **Mevcut SQL Server sanal makine galerisi görüntüleri güncelleştirildi mi?**

   Sanal makine galerisindeki SQL Server görüntüleri her iki ayda bir en son Windows ve Linux güncellemeleriyle güncellenir. Windows görüntüleri için bu, önemli SQL Server güvenlik güncelleştirmeleri ve hizmet paketleri de dahil olmak üzere Windows Update'te önemli olarak işaretlenmiş güncelleştirmeleri içerir. Linux görüntüleri için bu, en son sistem güncelleştirmelerini içerir. SQL Server kümülatif güncelleştirmeleri Linux ve Windows için farklı şekilde işlenir. Linux için, SQL Server kümülatif güncelleştirmeleri de yenilemeye dahildir. Ancak şu anda, Windows VM'ler SQL Server veya Windows Server kümülatif güncelleştirmeleri ile güncelleştirilmeyecektir.

1. **SQL Server sanal makine görüntüleri galeriden kaldırılabilir mi?**

   Evet. Azure, ana sürüm ve sürüm başına yalnızca bir görüntü tutar. Örneğin, yeni bir SQL Server hizmet paketi yayımlandığında, Azure bu hizmet paketi için galeriye yeni bir resim ekler. Önceki hizmet paketinin SQL Server görüntüsü Azure portalından hemen kaldırılır. Ancak, önümüzdeki üç ay boyunca PowerShell'den tedarik için hala kullanılabilir. Üç ay sonra, önceki hizmet paketi görüntüsü artık kullanılamaz. Bu kaldırma ilkesi, bir SQL Server sürümü yaşam döngüsünün sonuna ulaştığında desteklenmediğinde de geçerli olur.


1. **SQL Server'ın Azure portalında görünmeyen eski bir görüntüsünü dağıtmak mümkün mü?**

   Evet, PowerShell kullanarak. PowerShell kullanarak SQL Server VM'leri dağıtma hakkında daha fazla bilgi için [SQL Server sanal makinelerini Azure PowerShell ile nasıl sağsanız bulabilirsiniz.](virtual-machines-windows-ps-sql-create.md)

1. **SQL Server VM'min genelleştirilmiş bir Azure SQL Server Marketplace görüntüsünü oluşturabilir ve VM'leri dağıtmak için kullanabilir miyim?**

   Evet, ancak daha sonra portalda SQL Server VM'nizi yönetmek ve otomatik düzeltme ve otomatik yedekleme gibi özellikleri kullanmak için [her SQL Server VM'yi SQL Server VM kaynak sağlayıcısına kaydetmeniz](virtual-machines-windows-sql-register-with-resource-provider.md) gerekir. Kaynak sağlayıcısına kaydolurken, her SQL Server VM için lisans türünü belirtmeniz gerekir. 

1. **SQL Server VM dağıtmak için kendi VHD'mi kullanabilir miyim?**

   Evet, ancak daha sonra portalda SQL Server VM'nizi yönetmek ve otomatik düzeltme ve otomatik yedekleme gibi özellikleri kullanmak için [her SQL Server VM'yi SQL Server VM kaynak sağlayıcısına kaydetmeniz](virtual-machines-windows-sql-register-with-resource-provider.md) gerekir.

1. **Sanal makine galerisinde (Örneğin Windows 2008 R2 + SQL Server 2012) gösterilmeyen yapılandırmalar kurmak mümkün mü?**

   Hayır. SQL Server'ı içeren sanal makine galerisi resimleri için, azure portalı üzerinden veya [PowerShell](virtual-machines-windows-ps-sql-create.md)üzerinden sağlanan resimlerden birini seçmeniz gerekir. Ancak, bir Windows VM dağıtma ve kendi kendine SQL Server yükleme yeteneğine sahipsiniz. Daha sonra SQL Server VM'nizi portalda yönetmek ve otomatik düzeltme ve otomatik yedekleme gibi özellikleri kullanmak için [SQL Server VM kaynak sağlayıcısına SQL Server VM'inizi kaydetmeniz](virtual-machines-windows-sql-register-with-resource-provider.md) gerekir. 


## <a name="creation"></a>Oluşturma

1. **SQL Server ile nasıl bir Azure sanal makinesi oluşturabilirim?**

   En kolay yöntem, SQL Server içeren bir Sanal Makine oluşturmaktır. Azure'a kaydolma ve portaldan bir SQL Server VM oluşturma hakkında bir öğretici için Azure [portalında SQL Server sanal makine sağlama](virtual-machines-windows-portal-sql-server-provision.md)konusuna bakın. Saniyede ödemeli SQL Server lisansını kullanan sanal bir makine görüntüsü seçebilir veya kendi SQL Server lisansınızı getirmenize olanak tanıyan bir görüntü kullanabilirsiniz. Ayrıca, sql server'ı serbest lisanslı bir sürümle (Geliştirici veya Ekspres) veya şirket içi lisansı yeniden kullanarak vm'ye el ile yükleme seçeneğiniz de vardır. SQL Server VM'nizi portalda yönetmek ve otomatik düzeltme ve otomatik yedekleme gibi özelliklerden yararlanmak için [SQL Server VM kaynak sağlayıcınıza SQL Server VM'inizi kaydettiğinizden](virtual-machines-windows-sql-register-with-resource-provider.md) emin olun. Kendi lisansınızı getiriyorsanız, [Azure'da Yazılım Güvencesi ile Lisans Mobilite'ine](https://azure.microsoft.com/pricing/license-mobility/)sahip olmalısınız. Daha fazla bilgi için bkz. [SQL Server Azure VM’leri için fiyatlandırma kılavuzu](virtual-machines-windows-sql-server-pricing-guidance.md).

1. **Şirket içi SQL Server veritabanımı Bulut'a nasıl geçirebilirim?**

   Önce SQL Server örneği içeren bir Azure sanal makinesi oluşturun. Ardından şirket içi veritabanlarınızı bu örne geçirin. Veri geçiş stratejileri [için](virtual-machines-windows-migrate-sql.md)bkz.

## <a name="licensing"></a>Lisanslama

1. **Lisanslı SQL Server kopyamı bir Azure sanal makinesine nasıl yükleyebilirim?**

   Bunu yapmanın üç yolu vardır. Kurumsal bir anlaşma (EA) müşterisiyseniz, kendi lisansını getir (BYOL) olarak da bilinen [lisansları destekleyen sanal makine görüntülerinden](virtual-machines-windows-sql-server-iaas-overview.md#BYOL)birini sağlayabilirsiniz. [Yazılım güvenceniz](https://www.microsoft.com/en-us/licensing/licensing-programs/software-assurance-default)varsa, Azure [Karma Avantajını](virtual-machines-windows-sql-ahb.md) varsa istediğiniz kadar öde (PAYG) görüntüde etkinleştirebilirsiniz. Veya SQL Server yükleme ortamını Windows Server VM'ye kopyalayıp VM'ye SQL Server yükleyebilirsiniz. Portal yönetimi, otomatik yedekleme ve otomatik düzeltme gibi özellikler için SQL Server VM'inizi [kaynak sağlayıcısına](virtual-machines-windows-sql-register-with-resource-provider.md) kaydettiğinizden emin olun. 

1. **Kullandıkça ödeme galeri görüntülerinden biri kullanılarak oluşturulan sanal makineyi kendi SQL Server lisansımı kullanmak için değiştirebilir miyim?**

   Evet. [Azure Karma Avantajı'nı](https://azure.microsoft.com/pricing/hybrid-benefit/faq/)etkinleştirerek kendi lisansınızı (BYOL) getirmek için istediğiniz kadar öde (PAYG) galeri görüntüsünü kolayca değiştirebilirsiniz.  Daha fazla bilgi için, [SQL Server VM'nin lisans modelini nasıl değiştireceğiniz](virtual-machines-windows-sql-ahb.md)e bakın. Şu anda bu tesis yalnızca Public Cloud müşterileri için kullanılabilir.

1. **Lisanslama modellerini değiştirmek için SQL Server'ın kapalı kalması gerekir mi?**

   Hayır. Lisans [modelini değiştirmek,](virtual-machines-windows-sql-ahb.md) sql server için herhangi bir kapalı kalma süresi gerektirmez, çünkü değişiklik hemen etkili olur ve VM'nin yeniden başlatılmasını gerektirmez. Ancak, SQL Server VM'nizi SQL Server VM kaynak sağlayıcısına kaydetmek [için, SQL IaaS uzantısı](virtual-machines-windows-sql-server-agent-extension.md) bir ön koşuldur ve SQL IaaS uzantısını _tam_ modda yüklemek SQL Server hizmetini yeniden başlatır. Bu nedenle, SQL IaaS uzantısı yüklü olması gerekiyorsa, sınırlı işlevsellik için _hafif_ modda yükleyin veya bakım penceresi sırasında _tam_ modda yükleyin. _Hafif_ modda yüklü olan SQL IaaS uzantısı her zaman _tam_ moda yükseltilebilir, ancak SQL Server hizmetinin yeniden başlatılması nı gerektirir. 
   
1. **Klasik model kullanılarak dağıtılan bir SQL Server VM'de lisanslama modelini değiştirmek mümkün mü?**

   Hayır. Lisans modelini değiştirmek klasik bir VM'de desteklenmez. VM'nizi Azure Kaynak Yöneticisi modeline geçirebilir ve SQL Server VM kaynak sağlayıcısına kaydolabilirsiniz. VM, SQL Server VM kaynak sağlayıcısına kaydolduktan sonra, Lisans modeli değişiklikleri VM'de kullanılabilir.

1. **Aynı VM'de birden çok örneği yönetmek için Azure portalını kullanabilir miyim?**

   Hayır. Portal yönetimi, SQL Server IaaS Agent uzantısına dayanan SQL Server VM kaynak sağlayıcısı tarafından sağlanan bir özelliktir. Bu nedenle, aynı sınırlamalar uzantı ile aynı kaynak sağlayıcı için geçerlidir. Portal, doğru şekilde yapılandırıldığı sürece yalnızca bir varsayılan örneği veya adlandırılmış bir örneği yönetebilir. Bu sınırlamalar hakkında daha fazla bilgi için [SQL Server IaaS aracı uzantısına](virtual-machines-windows-sql-server-agent-extension.md)bakın. 

1. **CSP abonelikleri Azure Karma Avantajı'nı etkinleştirebilir mi?**

   Evet, Azure Karma Avantajı CSP abonelikleri için kullanılabilir. CSP müşterileri önce istediğiniz kadar öde görüntüsünü dağıtmalı ve ardından [lisans modelini](virtual-machines-windows-sql-ahb.md) kendi lisansınızı getirmek için değiştirmelidir.
   
 
1. **Yalnızca bekleme/yük devretme için kullanılıyorsa Azure sanal makinesindeki SQL Server'ı lisanslamak için ödeme yapmam gerekir mi?**

   Bekleme ikincil kullanılabilirlik grubu veya başarısız kümelenmiş örnek için ücretsiz pasif lisansa sahip olmak için, [Ürün Lisans Koşulları'nda](https://www.microsoft.com/licensing/product-licensing/products)belirtilen aşağıdaki ölçütlerin tümlerini karşılamanız gerekir:

   1. [Yazılım Güvencesi](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default?activetab=software-assurance-default-pivot%3aprimaryr3)ile [lisans hareketliliğiniz](https://www.microsoft.com/licensing/licensing-programs/software-assurance-license-mobility?activetab=software-assurance-license-mobility-pivot:primaryr2) var. 
   1. Pasif SQL Server örneği istemcilere SQL Server verileri sunmaz veya etkin SQL Server iş yüklerini çalıştırmaz. Yalnızca birincil sunucuyla eşitlemek ve pasif veritabanını sıcak bir bekleme durumunda korumak için kullanılır. Etkin SQL Server iş yüklerini çalıştıran istemcilere raporlar veya ürün koşullarında belirtilenler dışında herhangi bir iş gerçekleştirmek gibi veriler sunuyorsa, ücretli lisanslı bir SQL Server örneği olmalıdır. İkincil örnekte aşağıdaki aktiviteye izin verilir: veritabanı tutarlılık denetimleri veya CheckDB, tam yedeklemeler, işlem günlüğü yedeklemeleri ve kaynak kullanım verilerini izleme. Ayrıca, her 90 günde bir kısa olağanüstü durum kurtarma testi süreleri için birincil ve ilgili olağanüstü durum kurtarma örneğini aynı anda çalıştırabilirsiniz. 
   1. Etkin SQL Server lisansı Yazılım Güvencesi kapsamındadır ve yalnızca lisanslı etkin sunucuyla aynı miktarda bilgi işlemle **bir pasif** ikincil SQL Server örneğine izin verir. 
   1. İkincil SQL Server VM, Azure portalındaki [Olağanüstü Durum Kurtarma](virtual-machines-windows-sql-high-availability-dr.md#free-dr-replica-in-azure) lisansını kullanır.
   
1. **Pasif örnek olarak ne kabul edilir?**

   Pasif SQL Server örneği istemcilere SQL Server verileri sunmaz veya etkin SQL Server iş yüklerini çalıştırmaz. Yalnızca birincil sunucuyla eşitlemek ve pasif veritabanını sıcak bir bekleme durumunda korumak için kullanılır. Etkin SQL Server iş yüklerini çalıştıran istemcilere raporlar veya ürün koşullarında belirtilenler dışında herhangi bir iş gerçekleştirmek gibi veriler sunuyorsa, ücretli lisanslı bir SQL Server örneği olmalıdır. İkincil örnekte aşağıdaki aktiviteye izin verilir: veritabanı tutarlılık denetimleri veya CheckDB, tam yedeklemeler, işlem günlüğü yedeklemeleri ve kaynak kullanım verilerini izleme. Ayrıca, her 90 günde bir kısa olağanüstü durum kurtarma testi süreleri için birincil ve ilgili olağanüstü durum kurtarma örneğini aynı anda çalıştırabilirsiniz.
   

1. **Distaster Recovery (DR) avantajını hangi senaryolar kullanabilir?**

   [Lisans kılavuzu,](https://aka.ms/sql2019licenseguide) Olağanüstü Durum Kurtarma Avantajı'ndan yararlanılabilen senaryolar sağlar. Ürün Koşullarınıza bakın ve daha fazla bilgi için lisans kişilerinizle veya hesap yöneticinizle konuşun.

1. **Hangi abonelikler Olağanüstü Durum Kurtarma (DR) avantajını destekler?**

   Sabit bir avantaj olarak Yazılım Güvencesi eşdeğer abonelik hakları sunan kapsamlı programlar DR avantajını destekler. Buna dahildir. ancak, Açık Değer (OV), Açık Değer Aboneliği (OVS), Kurumsal Sözleşme (EA), Kurumsal Abonelik Sözleşmesi (EAS) ve Sunucu ve Bulut Kaydı (SCE) ile sınırlı değildir. [Ürün koşullarına](https://www.microsoft.com/licensing/product-licensing/products) bakın ve daha fazla bilgi için lisans kişilerinizle veya acocunt yöneticinizle konuşun. 

   
 ## <a name="resource-provider"></a>Kaynak sağlayıcısı

1. **VM'imi yeni SQL Server VM kaynak sağlayıcısına kaydettirmeek maliyetler getirir mi?**

   Hayır. SQL Server VM kaynak sağlayıcısı, Azure VM'deki SQL Server için ek ücret ödemeden ek yönetilebilirlik sağlar. 

1. **SQL Server VM kaynak sağlayıcısı tüm müşteriler için kullanılabilir mi?**
 
   Evet, SQL Server VM genel bulutta Kaynak Yöneticisi modelini kullanarak dağıtılmış olduğu sürece, klasik modeli değil. Diğer tüm müşteriler yeni SQL Server VM kaynak sağlayıcısına kaydolabilir. Ancak, yalnızca [Yazılım Güvencesi](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default?activetab=software-assurance-default-pivot%3aprimaryr3) avantajına sahip müşteriler, Azure [Karma Avantajını (AHB)](https://azure.microsoft.com/pricing/hybrid-benefit/) bir SQL Server VM'de etkinleştirerek kendi lisanslarını kullanabilir. 

1. **VM kaynağı taşınır veya bırakılırsa kaynak sağlayıcısına _(Microsoft.SqlVirtualMachine)_ ne olur?** 

   Microsoft.Compute/VirtualMachine kaynağı bırakıldığında veya taşındığında, ilişkili Microsoft.SqlVirtualMachine kaynağı işlemi eşit bir şekilde çoğaltmak üzere bildirilir.

1. **Kaynak sağlayıcısı _(Microsoft.SqlVirtualMachine)_ kaynağı bırakılırsa VM'ye ne olur?**

    Microsoft.Compute/VirtualMachine kaynağı, Microsoft.SqlVirtualMachine kaynağı bırakıldığında etkilenmez. Ancak, lisans değişiklikleri varsayılan olarak özgün görüntü kaynağına geri döner. 

1. **Sql Server VM kaynak sağlayıcısına kendi dağıtılan SQL Server VM'leri kaydetmek mümkün mü?**

    Evet. SQL Server'ı kendi ortamınızdan dağıttıysanız ve SQL IaaS uzantısını yüklediyseniz, SQL IaaS uzantısı tarafından sağlanan yönetilebilirlik avantajlarından yararlanmak için SQL Server VM'inizi kaynak sağlayıcısına kaydedebilirsiniz. Ancak, kendi kendine dağıtılan bir SQL Server VM'yi istediğiniz kadar öde'ye dönüştüremiyorsunuz.


   


## <a name="administration"></a>Yönetim

1. **Aynı VM'ye ikinci bir SQL Server örneği yükleyebilir miyim? Varsayılan örneğin yüklü özelliklerini değiştirebilir miyim?**

   Evet. SQL Server yükleme ortamı **C** sürücüsünde bir klasörde bulunur. Yeni SQL Server örnekleri eklemek veya makinede SQL Server'ın diğer yüklü özelliklerini değiştirmek için **setup.exe'yi** bu konumdan çalıştırın. Otomatik Yedekleme, Otomatik Yama ve Azure Anahtar Kasası Tümleştirmesi gibi bazı özelliklerin yalnızca varsayılan örneğe veya düzgün yapılandırılan adlandırılmış bir örneğe karşı çalıştığını unutmayın (Bkz. Soru 3). 

1. **SQL Server'ın varsayılan örneğini kaldırabilir miyim?**

   Evet, ama bazı noktaları dikkate almalısınız. İlk olarak, VM'nin lisans modeline bağlı olarak SQL Server ile ilişkili faturalandırma oluşmaya devam edebilir. İkinci olarak, önceki yanıtta belirtildiği gibi, [SQL Server IaaS Agent Extension](virtual-machines-windows-sql-server-agent-extension.md)dayanan özellikler vardır. IaaS uzantısını da kaldırmadan varsayılan örneği kaldırırsanız, uzantı varsayılan örneği aramaya devam eder ve olay günlüğü hataları oluşturabilir. Bu hatalar aşağıdaki iki kaynaktan gelir: **Microsoft SQL Server Kimlik Bilgileri Yönetimi** ve Microsoft SQL Server **IaaS Aracısı.** Hatalardan biri aşağıdakine benzer olabilir:

      SQL Server ile bağlantı kurulmaya çalışılırken ağ ile ilişkili veya örneğe özgü bir hata oluştu. Sunucu bulunamadı veya erişilebilir değildi.

   Varsayılan örneği kaldırmaya karar verirseniz, SQL [Server IaaS Aracı Uzantısı'nı](virtual-machines-windows-sql-server-agent-extension.md) da kaldırın. 

1. **IaaS uzantılı SQL Server adlı bir örneğini kullanabilir miyim?**
   
   Evet, adlandırılmış örnek SQL Server'daki tek örnekse ve özgün varsayılan örnek [düzgün bir şekilde kaldırıldıysa.](virtual-machines-windows-sql-server-agent-extension.md#install-on-a-vm-with-a-single-named-sql-server-instance) Varsayılan bir örnek yoksa ve tek bir SQL Server VM'de birden çok adlandırılmış örnek varsa, SQL Server IaaS aracı uzantısı yüklenmezse. 

1. **SQL Server'ı SQL Server VM'den tamamen kaldırabilir miyim?**

   Evet, ancak [SQL Server Azure VM'leriniziçin Fiyatlandırma kılavuzunda](virtual-machines-windows-sql-server-pricing-guidance.md)açıklandığı gibi SQL Server VM'niz için ücretlendirilmeye devam edersiniz. Artık SQL Server'a ihtiyacınız kalmadıysa yeni bir sanal makine dağıtabilir ve verilerle uygulamaları yeni sanal makineye geçirebilirsiniz. Ardından SQL Server sanal makinesini kaldırabilirsiniz.
   
## <a name="updating-and-patching"></a>Güncelleme ve Yama

1. **Azure sanal makinesinde SQL Server'ın farlı bir sürümüne/yayınına nasıl geçebilirim?**

   Müşteriler SQL Server'ın istenen sürümünü veya yayınını içeren kurulum medyasını kullanarak SQL Server sürümlerini/yayınlarını değiştirebilir. Yayın değiştikten sonra Azure portalını kullanarak VM için faturalamayı doğru yansıtacak şekilde VM'nin yayın özelliğini değiştirin. Daha fazla bilgi için, [bir SQL Server VM'nin değişiklik sürümüne](virtual-machines-windows-sql-change-edition.md)bakın. SQL Server'ın farklı sürümleri için faturalandırma farkı yoktur, bu nedenle SQL Server sürümü değiştirildikten sonra başka bir işlem gerekmez.

1. **SQL Server sürümünü veya sürümünü değiştirmek için kurulum ortamını nereden alabilirim?**

   [Yazılım güvencesine](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default) sahip müşteriler yükleme ortamlarını [Toplu Lisans Lama Merkezi'nden](https://www.microsoft.com/Licensing/servicecenter/default.aspx)edinebilirler. Yazılım güvencesi olmayan müşteriler, istedikleri sürümü olan Marketplace SQL Server VM görüntüsünden kurulum ortamını kullanabilir.
   
1. **SQL Server VM'de güncelleştirmeler ve hizmet paketleri nasıl uygulanır?**

   Sanal makineler güncelleştirmeleri ne zaman ve nasıl uygulayacağınız dahil olmak üzere ana makine üzerinde denetim sağlar. İşletim sistemi için, windows güncelleştirmelerini el ile uygulayabilir veya [Otomatik Yama](virtual-machines-windows-sql-automated-patching.md)adı verilen bir zamanlama hizmetini etkinleştirebilirsiniz. Otomatik Düzeltme Eki Uygulama, önemli olarak işaretlenmiş tüm güncelleştirmeleri, bu kategorideki SQL Server güncelleştirmeleri de dahil olmak üzere yükler. SQL Server için diğer isteğe bağlı güncelleştirmeler el ile yüklenmelidir.

1. **SQL Server 2008 / 2008 R2 örneğimi SQL Server VM kaynak sağlayıcısına kaydettikten sonra yükseltebilir miyim?**

   Evet. SQL Server'ın sürümünü ve sürümünü yükseltmek için herhangi bir kurulum ortamını kullanabilir ve ardından [SQL IaaS uzantı modunuzu](virtual-machines-windows-sql-register-with-resource-provider.md#management-modes)hiçbir _aracıdan_ _tam_olarak yükseltebilirsiniz. Bunu yapmak, portal yönetilebilirliği, otomatik yedeklemeler ve otomatik düzeltme ler gibi SQL IaaS uzantısının tüm avantajlarına erişmenizi sağlar. 

1. **Sql Server 2008 ve SQL Server 2008 R2 örnekleri için destek sonum için ücretsiz genişletilmiş güvenlik güncelleştirmelerini nasıl alabilirim?**

   SQL Server'ınızı bir Azure SQL sanal makinesine taşıyarak [ücretsiz genişletilmiş güvenlik güncelleştirmeleri](virtual-machines-windows-sql-server-2008-eos-extend-support.md) alabilirsiniz. Daha fazla bilgi için [destek seçeneklerinin sonuna](/sql/sql-server/end-of-support/sql-server-end-of-life-overview)bakın. 
  
   

## <a name="general"></a>Genel

1. **SQL Server başarısız küme örnekleri (FCI) Azure VM'lerde desteklendi mi?**

   Evet. Depolama alt sistemi için premium [dosya paylaşımları (PFS)](virtual-machines-windows-portal-sql-create-failover-cluster-premium-file-share.md) veya [depolama alanları doğrudan (S2D)](virtual-machines-windows-portal-sql-create-failover-cluster.md) kullanarak bir failover küme örneği yükleyebilirsiniz. Premium dosya paylaşımları, birçok iş yükünün gereksinimlerini karşılayacak IOPS ve üretim kapasiteleri sağlar. IO yoğun iş yükleri için, depolama alanlarını doğrudan manged premium veya ultra disklere dayalı olarak kullanmayı düşünün. Alternatif olarak, [Azure Sanal Makinelerde SQL Server için Yüksek kullanılabilirlik ve olağanüstü durum kurtarma'da](virtual-machines-windows-sql-high-availability-dr.md#azure-only-high-availability-solutions)açıklandığı gibi üçüncü taraf kümeleme veya depolama çözümlerini kullanabilirsiniz.

   > [!IMPORTANT]
   > Şu anda, _tam_ [SQL Server IaaS Agent Extension](virtual-machines-windows-sql-server-agent-extension.md) Azure'daki SQL Server FCI için desteklenmez. FCI'a katılan VM'lerin _tam_ uzantısını kaldırmanızı ve uzantıyı _hafif_ modda yüklemenizi öneririz. Bu uzantı, Otomatik Yedekleme ve Yama gibi özellikleri ve SQL Server için bazı portal özelliklerini destekler. Bu _özellikler, tam_ aracı yüklendikten sonra SQL Server VM'ler için çalışmaz.

1. **SQL Server VM'ler ile SQL Veritabanı hizmeti arasındaki fark nedir?**

   Kavramsal olarak, SQL Server'ı bir Azure sanal makinesinde çalıştırmak, uzak bir veri merkezinde SQL Server'ı çalıştırmaktan farklı değildir. Buna karşılık, [SQL Veritabanı](../../../sql-database/sql-database-technical-overview.md) veritabanı-as-a-hizmet sunar. SQL Veritabanı ile veritabanlarınızı barındıran makinelere erişiminiz olmaz. Tam bir karşılaştırma için [bkz: Bulut SQL Server seçeneğini seçin: Azure SQL (PaaS) Veritabanı veya SQL Server, Azure VM'lerde (IaaS)](../../../sql-database/sql-database-paas-vs-sql-server-iaas.md).

1. **Azure VM'me SQL Veri araçlarını nasıl yüklerim?**

    Microsoft SQL Server Veri [Araçları - Visual Studio 2013 için İş Zekası'ndan](https://www.microsoft.com/download/details.aspx?id=42313)SQL Veri araçlarını indirin ve kurun.

1. **MSDTC ile dağıtılmış hareketler SQL Server VM'lerde desteklenir mi?**
   
    Evet. Yerel DTC, SQL Server 2016 SP2 ve üzeri için desteklenir. Ancak, bir arıza sırasında uçuş sırasında yapılan işlemler başarısız olacağından ve yeniden deneneceği için, uygulamalar Her Zaman kullanılabilirlik gruplarını kullanırken test edilmelidir. Windows Server 2019'dan itibaren Clustered DTC kullanılabilir. 

## <a name="resources"></a>Kaynaklar

**Windows VMs**:

* [Windows VM'de SQL Server'a genel bakış.](virtual-machines-windows-sql-server-iaas-overview.md)
* [SQL Server Windows VM sağlama](virtual-machines-windows-portal-sql-server-provision.md)
* [Azure VM'de Veritabanını SQL Server'a Geçirme](virtual-machines-windows-migrate-sql.md)
* [Azure Sanal Makinelerde SQL Server için Yüksek Kullanılabilirlik ve Olağanüstü Durum Kurtarma](virtual-machines-windows-sql-high-availability-dr.md)
* [Azure Sanal Makinelerde SQL Server için performansa yönelik en iyi yöntemler](virtual-machines-windows-sql-performance.md)
* [Azure Sanal Makineler'de SQL Server için Uygulama Desenleri ve Geliştirme Stratejileri](virtual-machines-windows-sql-server-app-patterns-dev-strategies.md)

**Linux VMs**:

* [Linux VM'de SQL Server'a Genel Bakış](../../linux/sql/sql-server-linux-virtual-machines-overview.md)
* [SQL Server Linux VM sağlama](../../linux/sql/provision-sql-server-linux-virtual-machine.md)
* [SSS (Linux)](../../linux/sql/sql-server-linux-faq.md)
* [Linux üzerinde SQL Server belgeleri](https://docs.microsoft.com/sql/linux/sql-server-linux-overview)
