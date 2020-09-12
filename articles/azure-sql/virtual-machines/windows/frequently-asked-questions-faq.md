---
title: Azure 'da Windows Sanal Makineleri SQL Server SSS | Microsoft Docs
description: Bu makalede, Azure VM 'lerinde SQL Server çalıştırma hakkında sık sorulan sorulara yanıtlar sağlanmaktadır.
services: virtual-machines-windows
documentationcenter: ''
author: MashaMSFT
editor: ''
tags: azure-service-management
ms.assetid: 2fa5ee6b-51a6-4237-805f-518e6c57d11b
ms.service: virtual-machines-sql
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 08/05/2019
ms.author: mathoma
ms.openlocfilehash: a5f4ff3dade381cf1a68ac5e9e820be153acf5ee
ms.sourcegitcommit: de2750163a601aae0c28506ba32be067e0068c0c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2020
ms.locfileid: "89483754"
---
# <a name="frequently-asked-questions-for-sql-server-on-azure-vms"></a>Azure VM 'lerinde SQL Server için sık sorulan sorular
[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]

> [!div class="op_single_selector"]
> * [Windows](frequently-asked-questions-faq.md)
> * [Linux](../linux/frequently-asked-questions-faq.md)

Bu makalede, [Windows Azure sanal makinelerinde (VM) SQL Server](https://azure.microsoft.com/services/virtual-machines/sql-server/)çalıştırmaya yönelik en yaygın soruların yanıtları sunulmaktadır.

[!INCLUDE [support-disclaimer](../../../../includes/support-disclaimer.md)]

## <a name="images"></a><a id="images"></a> Yansımasını

1. **SQL Server sanal makine galeri görüntüleri kullanılabilir?** 

   Azure, hem Windows hem de Linux için tüm sürümlerde SQL Server tüm desteklenen ana sürümleri için sanal makine görüntülerini tutar. Daha fazla bilgi için [WINDOWS VM görüntülerinin](sql-server-on-azure-vm-iaas-what-is-overview.md#payasyougo) ve [Linux VM görüntülerinin](../linux/sql-server-on-linux-vm-what-is-iaas-overview.md#create)tüm listesini inceleyin.

1. **Mevcut SQL Server sanal makine galeri görüntüleri güncelleştirildi mi?**

   Her iki ayda bir sanal makine galerisinde SQL Server görüntüler en son Windows ve Linux güncelleştirmeleriyle güncelleştirilir. Windows görüntüleri için, önemli SQL Server güvenlik güncelleştirmeleri ve hizmet paketleri de dahil olmak üzere Windows Update önemli olarak işaretlenmiş tüm güncelleştirmeleri içerir. Linux görüntüleri için bu en son sistem güncelleştirmelerini içerir. SQL Server toplu güncelleştirmeler, Linux ve Windows için farklı şekilde işlenir. Linux için SQL Server toplu güncelleştirmeler de yenilemeye dahildir. Ancak şu anda Windows VM 'Ler SQL Server veya Windows Server toplu güncelleştirmeleriyle güncellenmez.

1. **SQL Server sanal makine görüntülerinin Galeriden kaldırılması olabilir mi?**

   Evet. Azure, ana sürüm ve sürüm başına yalnızca bir görüntü tutar. Örneğin, yeni bir SQL Server hizmet paketi yayınlandığında Azure bu hizmet paketinin galerisine yeni bir görüntü ekler. Önceki hizmet paketinin SQL Server görüntüsü hemen Azure portal kaldırılır. Ancak, sonraki üç ay için PowerShell 'den sağlama için hala kullanılabilir. Üç aydan sonra, önceki hizmet paketi görüntüsü artık kullanılamaz. SQL Server bir sürüm yaşam döngüsünün sonuna ulaştığında desteklenmiyorsa bu kaldırma ilkesi de geçerlidir.


1. **Azure portal görünmeyen SQL Server eski bir görüntüsünü dağıtmak mümkün mü?**

   Evet, PowerShell kullanarak. PowerShell kullanarak SQL Server VM 'Leri dağıtma hakkında daha fazla bilgi için, bkz. [Azure PowerShell ile SQL Server sanal makineler sağlama](create-sql-vm-powershell.md).
   
1. **SQL Server VM genelleştirilmiş bir Azure Marketi SQL Server görüntüsü oluşturup VM 'Leri dağıtmak için kullanabilir mi?**

   Evet, ancak her bir SQL Server VM portalda SQL Server VM yönetmeniz ve otomatik düzeltme eki uygulama ve otomatik yedeklemeler gibi özelliklerden yararlanmak için [SQL Server VM kaynak sağlayıcısıyla kaydetmeniz](sql-vm-resource-provider-register.md) gerekir. Kaynak sağlayıcısına kaydolurken her bir SQL Server VM için lisans türünü de belirtmeniz gerekecektir.

1. **Azure VM 'de generalize SQL Server Nasıl yaparım? ve yeni VM 'Leri dağıtmak için kullanın mi?**

   Bir Windows Server VM 'si (üzerinde SQL Server yüklü olmadan) dağıtabilir ve [SQL Sysprep](/sql/database-engine/install-windows/install-sql-server-using-sysprep?view=sql-server-ver15) Işlemini kullanarak Azure VM 'de SQL Server genelleştirin (Windows) SQL Server yükleme medyasını kullanabilirsiniz. [Yazılım güvencesi](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default?rtc=1&activetab=software-assurance-default-pivot%3aprimaryr3) sahibi olan müşteriler, yükleme medyasını [toplu lisanslama merkezinden](https://www.microsoft.com/Licensing/servicecenter/default.aspx)elde edebilir. Yazılım Güvencesi sahibi olmayan müşteriler, istenen sürüme sahip bir Azure Marketi SQL Server VM görüntüsünden kurulum medyasını kullanabilir.

   Alternatif olarak, Azure VM 'de SQL Server genelleştirmek için Azure Marketi 'ndeki SQL Server görüntülerden birini kullanın. Kendi görüntünüzü oluşturmadan önce kaynak görüntüde aşağıdaki kayıt defteri anahtarını silmeniz gerektiğini unutmayın. Bunun yapılmaması, SQL Server Kurulum önyükleme klasörü ve/veya SQL IaaS uzantısının başarısız durumuna atılamasından kaynaklanabilir.

   Kayıt defteri anahtarı yolu:  
   `Computer\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Setup\SysPrepExternal\Specialize`

   > [!NOTE]
   > Azure VM 'lerdeki SQL Server, özel Genelleştirilmiş görüntülerden dağıtılan olanlar da dahil olmak üzere, uyumluluk gereksinimlerini karşılamak ve otomatik düzeltme eki uygulama ve otomatik yedekleme gibi isteğe bağlı özelliklerden yararlanmak için [SQL VM kaynak sağlayıcısı 'na kaydedilmelidir](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-register-with-resource-provider?tabs=azure-cli%2Cbash) . Kaynak sağlayıcısı her bir SQL Server VM için [lisans türünü belirtmenizi](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-ahb?tabs=azure-portal) de sağlar.

1. **SQL Server VM dağıtmak için kendi VHD 'mi kullanabilir miyim?**

   Evet, ancak her bir SQL Server VM portalda SQL Server VM yönetmeniz ve otomatik düzeltme eki uygulama ve otomatik yedeklemeler gibi özelliklerden yararlanmak için [SQL Server VM kaynak sağlayıcısıyla kaydetmeniz](sql-vm-resource-provider-register.md) gerekir.

1. **Sanal makine galerisinde gösterilmez (örneğin, Windows 2008 R2 + SQL Server 2012), yapılandırma ayarları yapılabilir mi?**

   Hayır. SQL Server içeren sanal makine galeri görüntüleri için, Azure portal veya [PowerShell](create-sql-vm-powershell.md)aracılığıyla, belirtilen görüntülerden birini seçmeniz gerekir. Bununla birlikte, bir Windows sanal makinesini dağıtabilir ve kendisine SQL Server kendi kendine yükleyebilirsiniz. Daha sonra, Azure portal SQL Server VM yönetmek için [SQL Server VM SQL Server VM kaynak sağlayıcısına kaydetmeniz](sql-vm-resource-provider-register.md) ve otomatik düzeltme eki uygulama ve otomatik yedeklemeler gibi özelliklerden yararlanmalısınız. 


## <a name="creation"></a>Oluşturma

1. **SQL Server bir Azure sanal makinesi mi Nasıl yaparım??**

   En kolay yöntem SQL Server içeren bir sanal makine oluşturmaktır. Azure 'a kaydolma ve portaldan SQL Server VM oluşturma hakkında bir öğretici için, bkz. [Azure portal SQL Server sanal makinesi sağlama](create-sql-vm-portal.md). Saniye başına ödeme SQL Server Lisanslama kullanan bir sanal makine görüntüsü seçebilir veya kendi SQL Server lisansınızı getirmenize olanak sağlayan bir görüntü kullanabilirsiniz. Ayrıca, ücretsiz lisanslı bir sürüme (Geliştirici veya Express) sahip bir VM 'ye SQL Server el ile yükleme veya şirket içi bir lisans yeniden kullanma seçeneğiniz de vardır. Portalda SQL Server VM yönetmek için [SQL Server VM kaynak sağlayıcısına SQL Server VM](sql-vm-resource-provider-register.md) kaydettiğinizden ve otomatik düzeltme eki uygulama ve otomatik yedeklemeler gibi özelliklerden yararlanın. Kendi lisansınızı getirayarladıysanız [Azure üzerinde Yazılım Güvencesiyle Lisans Taşınabilirliği](https://azure.microsoft.com/pricing/license-mobility/)olması gerekir. Daha fazla bilgi için bkz. [SQL Server Azure VM’leri için fiyatlandırma kılavuzu](pricing-guidance.md).

1. **Şirket içi SQL Server veritabanından buluta nasıl geçirebilirim?**

   İlk olarak SQL Server örneği olan bir Azure sanal makinesi oluşturun. Daha sonra şirket içi veritabanlarınızı bu örneğe geçirin. Veri geçiş stratejileri için bkz. [Azure VM 'de SQL Server SQL Server veritabanını geçirme](migrate-to-vm-from-sql-server.md).

## <a name="licensing"></a>Lisanslama

1. **Lisanslı SQL Server kopyamı bir Azure sanal makinesine nasıl yükleyebilirim?**

   Bunu yapmanın üç yolu vardır. Bir Kurumsal Anlaşma (EA) müşterisiyseniz, [Lisansları destekleyen sanal makine görüntülerinden](sql-server-on-azure-vm-iaas-what-is-overview.md#BYOL)birini sağlayabilirsiniz. Bu, kendi lisansını getır (KLG) olarak da bilinir. [Yazılım güvencesi](https://www.microsoft.com/en-us/licensing/licensing-programs/software-assurance-default)varsa, mevcut bir Kullandıkça Öde (PAYG) görüntüsünde [Azure hibrit avantajı](licensing-model-azure-hybrid-benefit-ahb-change.md) etkinleştirebilirsiniz. Dilerseniz de SQL Server yükleme medyasını bir Windows Server sanal makinesine kopyalayabilir ve sonra sanal makineye SQL Server yükleyebilirsiniz. Portal yönetimi, otomatik yedekleme ve otomatik düzeltme eki uygulama gibi özellikler için SQL Server VM [kaynak sağlayıcısına](sql-vm-resource-provider-register.md) kaydettiğinizden emin olun. 

1. **Kullandıkça ödeme galeri görüntülerinden biri kullanılarak oluşturulan sanal makineyi kendi SQL Server lisansımı kullanmak için değiştirebilir miyim?**

   Evet. Bir Kullandıkça Öde (PAYG) Galerisi görüntüsünü, [Azure hibrit avantajı](https://azure.microsoft.com/pricing/hybrid-benefit/faq/)etkinleştirerek kendi lisansını getır (KLG) seçeneğine kolayca geçirebilirsiniz.  Daha fazla bilgi için bkz. [SQL Server VM için lisans modelini değiştirme](licensing-model-azure-hybrid-benefit-ahb-change.md). Şu anda bu özellik yalnızca genel ve Azure Kamu bulut müşterileri tarafından kullanılabilir.

1. **Lisanslama modellerini değiştirmek için SQL Server'ın kapalı kalması gerekir mi?**

   Hayır. Değişiklik hemen etkin olduğundan ve VM 'nin yeniden başlatılmasını gerektirmediğinden, [Lisans modelinin değiştirilmesi](licensing-model-azure-hybrid-benefit-ahb-change.md) SQL Server için kapalı kalma süresi gerektirmez. Ancak, SQL Server VM SQL Server VM kaynak sağlayıcısına kaydetmek için [SQL IaaS uzantısı](sql-server-iaas-agent-extension-automate-management.md) bir önkoşuldur ve SQL IaaS uzantısını _tam_ modda yüklemek SQL Server hizmetini yeniden başlatır. Bu nedenle, SQL IaaS uzantısının yüklenmesi gerekiyorsa, sınırlı işlevsellik için _basit_ modda yükleme yapın veya bir bakım penceresi sırasında _tam_ moda yükleme yapın. _Hafif_ modda yüklü olan SQL IaaS uzantısı dilediğiniz zaman _tam_ moda yükseltilebilir, ancak SQL Server hizmeti 'nin yeniden başlatılmasını gerektirir. 
   
1. **Klasik model kullanılarak dağıtılan bir SQL Server VM lisans modellerini değiştirmek mümkün mü?**

   Hayır. Lisanslama modellerinin değiştirilmesi, klasik bir sanal makinede desteklenmez. VM'nizi Azure Resource Manager modeline geçirebilir ve SQL Server VM kaynak sağlayıcısına kaydedebilirsiniz. VM, SQL Server VM kaynak sağlayıcısına kaydedildikten sonra VM'de lisanslama modelini değiştirme seçenekleri sunulacaktır.

1. **Aynı VM'de birden çok örneği yönetmek için Azure portalını kullanabilir miyim?**

   Hayır. Portal yönetimi, SQL Server VM kaynak sağlayıcısı tarafından sunulan ve SQL Server IaaS Aracısı uzantısından faydalanan bir özelliktir. Bu nedenle kaynak sağlayıcısı ve uzantı için aynı sınırlamalar geçerlidir. Portal, doğru şekilde yapılandırıldığı sürece yalnızca bir varsayılan örneği ya da bir adlandırılmış örneği yönetebilir. Bu sınırlamalar hakkında daha fazla bilgi için bkz. [IaaS Aracısı uzantısı SQL Server](sql-server-iaas-agent-extension-automate-management.md). 

1. **CSP abonelikleri Azure Hibrit Avantajı'nı etkinleştirebilir mi?**

   Evet, Azure Hibrit Avantajı CSP abonelikleri için kullanılabilir. CSP müşterilerinin öncelikle bir Kullandıkça Öde görüntüsünü dağıtması ve [Lisans modelini](licensing-model-azure-hybrid-benefit-ahb-change.md) kendi lisansını getir olarak değiştirmesi gerekir.
   
 
1. **Yalnızca bekleme/yük devretme için kullanılıyorsa Azure sanal makinesindeki SQL Server'ı lisanslamak için ödeme yapmam gerekir mi?**

   Bekleme bir ikincil kullanılabilirlik grubu veya yük devretme kümelenmiş örneği için ücretsiz bir pasif lisansa sahip olmak için, [ürün lisanslama koşullarına](https://www.microsoft.com/licensing/product-licensing/products)göre belirtilen aşağıdaki ölçütlerin tümünü karşılamanız gerekir:

   1. [Yazılım güvencesi](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default?activetab=software-assurance-default-pivot%3aprimaryr3)aracılığıyla [Lisans Taşınabilirliği](https://www.microsoft.com/licensing/licensing-programs/software-assurance-license-mobility?activetab=software-assurance-license-mobility-pivot:primaryr2) vardır. 
   1. Pasif SQL Server örneği, istemcilere SQL Server verileri sunmuyor veya etkin SQL Server iş yükleri çalıştırmıyor. Yalnızca birincil sunucuyla eşitlemek ve pasif veritabanını yarı etkin bekleme durumunda tutmak için kullanılıyor. Etkin SQL Server iş yüklerini çalıştıran istemcilere rapor veya ürün koşullarında belirtilen dışında herhangi bir işi gerçekleştirirken, bu veriler, ücretli bir lisanslı SQL Server örneği olmalıdır. İkincil örnekte şu etkinliklere izin verilir: veritabanı tutarlılık denetimleri veya CheckDB, tam yedeklemeler, işlem günlüğü yedeklemeleri ve kaynak kullanımı verilerini izleme. Ayrıca 90 günde kısa süreli olağanüstü durum kurtarma testi için birincil ve karşılık gelen olağanüstü durum kurtarma örneğini aynı anda çalıştırabilirsiniz. 
   1. Etkin SQL Server Lisansı yazılım güvencesi kapsamında ele alınmıştır ve yalnızca lisanslı etkin sunucu ile aynı işlem miktarına kadar **bir** pasif ikincil SQL Server örneğine izin verir. 
   1. İkincil SQL Server VM, Azure portal [olağanüstü durum kurtarma](business-continuity-high-availability-disaster-recovery-hadr-overview.md#free-dr-replica-in-azure) lisansını kullanır.
   
1. **Hangi örnekler pasif örnek olarak kabul edilir?**

   Pasif SQL Server örneği, istemcilere SQL Server verileri sunmuyor veya etkin SQL Server iş yükleri çalıştırmıyor. Yalnızca birincil sunucuyla eşitlemek ve pasif veritabanını yarı etkin bekleme durumunda tutmak için kullanılıyor. Etkin SQL Server iş yükleri çalıştıran istemcilere raporlar gibi veriler sunuyorsa veya ürün koşullarında belirtilenin dışında bir iş yüküne sahipse ücretli lisanslı bir SQL Server örneği olması gerekir. İkincil örnekte şu etkinliklere izin verilir: veritabanı tutarlılık denetimleri veya CheckDB, tam yedeklemeler, işlem günlüğü yedeklemeleri ve kaynak kullanımı verilerini izleme. Ayrıca 90 günde kısa süreli olağanüstü durum kurtarma testi için birincil ve karşılık gelen olağanüstü durum kurtarma örneğini aynı anda çalıştırabilirsiniz.
   

1. **Hangi senaryolar Olağanüstü Durum Kurtarma (DR) avantajından faydalanabilir?**

   [Lisanslama Kılavuzu](https://aka.ms/sql2019licenseguide) , olağanüstü durum kurtarma avantajı 'nın kullanıldığı senaryolar sağlar. Daha fazla bilgi için Ürün Koşulları sayfasını inceleyin ve lisanslama konusundaki ilgili kişilere veya hesap yöneticinize danışın.

1. **Hangi abonelikler Olağanüstü Durum Kurtarma (DR) avantajını destekler?**

   Sabit avantaj ile aynı abonelik haklarına sahip Yazılım Güvencesi sunan programlar DR avantajını destekler. Buna aşağıdakiler dahildir: Ancak, açık değer (OV), açık değer aboneliği (OVS), Kurumsal Anlaşma (EA), Kurumsal Anlaşma abonelik (EAS) ve sunucu ve bulut kaydı (SCE) ile sınırlı değildir. Daha fazla bilgi için [Ürün koşullarına](https://www.microsoft.com/licensing/product-licensing/products) başvurun ve lisanslama kişileriniz veya hesap yöneticinize danışın. 

   
 ## <a name="resource-provider"></a>Kaynak sağlayıcısı

1. **Yeni SQL Server VM kaynak sağlayıcısına ait VM mi kaydedilecek ek maliyetler mi olacak?**

   Hayır. SQL Server VM kaynak sağlayıcısı, ek ücret ödemeden Azure sanal makinesinde SQL Server için ek yönetilebilirlik sunar. 

1. **SQL Server VM kaynak sağlayıcısı tüm müşteriler için kullanılabilir mi?**
 
   Evet, SQL Server VM genel buluta, klasik model değil Kaynak Yöneticisi modeli kullanılarak dağıtıldığı sürece. Diğer tüm müşteriler yeni SQL Server VM kaynak sağlayıcısına kaydolayamaz. Ancak, yalnızca [yazılım güvencesi](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default?activetab=software-assurance-default-pivot%3aprimaryr3) avantajı olan müşteriler bir SQL Server VM [Azure hibrit avantajı (AHB)](https://azure.microsoft.com/pricing/hybrid-benefit/) etkinleştirerek kendi lisansını kullanabilir. 

1. **VM kaynağı taşındığında veya bırakıldığında kaynak sağlayıcısı (_Microsoft. SqlVirtualMachine_) kaynağına ne olur?** 

   Microsoft. COMPUTE/VirtualMachine kaynağı bırakılır veya taşındığında, ilişkili Microsoft. SqlVirtualMachine kaynağına işlem zaman uyumsuz olarak çoğaltılır.

1. **Kaynak sağlayıcısı (_Microsoft. SqlVirtualMachine_) kaynağı bırakıldığında VM 'ye ne olur?**

    Microsoft. SqlVirtualMachine kaynağı bırakıldığında Microsoft. COMPUTE/VirtualMachine kaynağı etkilenmez. Ancak, lisans değişiklikleri varsayılan olarak orijinal görüntü kaynağına geri gönderilir. 

1. **SQL Server VM kaynak sağlayıcısı ile kendi kendine dağıtılan SQL Server VM 'Leri kaydetmek mümkün mü?**

    Evet. SQL Server kendi medyanınızdan dağıttıysanız ve SQL IaaS uzantısını yüklediyseniz, SQL IaaS uzantısının sağladığı yönetilebilirlik avantajlarından yararlanmak için SQL Server VM kaynak sağlayıcısına kaydedebilirsiniz.    


## <a name="administration"></a>Yönetim

1. **Aynı VM 'ye SQL Server ikinci bir örneğini yükleyebilir miyim? Varsayılan örneğin yüklü özelliklerini değiştirebilir miyim?**

   Evet. SQL Server yükleme medyası **C** sürücüsündeki bir klasörde yer alır. Yeni SQL Server örnekleri eklemek veya makinedeki SQL Server diğer yüklü özelliklerini değiştirmek için bu konumdan **Setup.exe** çalıştırın. Otomatik yedekleme, otomatik düzeltme eki uygulama ve Azure Key Vault tümleştirme gibi bazı özelliklerin yalnızca varsayılan örneğe veya düzgün yapılandırılmış adlandırılmış bir örneğe göre çalışacağını unutmayın (bkz. soru 3). Azure Hibrit Avantajı veya **Kullandıkça Öde** lisans modeli [aracılığıyla yazılım güvencesi](licensing-model-azure-hybrid-benefit-ahb-change.md) kullanan müşteriler, ek lisanslama maliyetleri olmadan sanal makineye birden çok SQL Server örneğini yükleyebilir. Ek SQL Server örnekleri, doğru yapılandırılmadığı müddetçe sistem kaynaklarını zorlayabilir. 

1. **Bir VM 'deki en fazla örnek sayısı nedir?**
   2019 SQL Server SQL Server 2012, tek başına bir sunucuda [50 örneklerini](/sql/sql-server/editions-and-components-of-sql-server-version-15#RDBMSSP) destekleyebilir. Bu, şirket içi Azure 'da ne olursa olsun aynı sınırlandırdır. Ortamınızı daha iyi hazırlama hakkında bilgi edinmek için [en iyi uygulamalar](performance-guidelines-best-practices.md#multiple-instances) bölümüne bakın. 

1. **SQL Server'ın varsayılan örneğini kaldırabilir miyim?**

   Evet, ama bazı noktaları dikkate almalısınız. İlk olarak, SQL Server ilişkili faturalandırma VM 'nin lisans modeline bağlı olarak devam edebilir. İkincisi, önceki cevap ' de belirtildiği gibi, [SQL Server IaaS Aracısı uzantısına](sql-server-iaas-agent-extension-automate-management.md)güvenen özellikler vardır. IaaS uzantısını kaldırmadan varsayılan örneği de kaldırırsanız, uzantı varsayılan örneği aramaya devam eder ve olay günlüğü hataları oluşturabilir. Bu hatalar şu iki kaynaktan alınır: **Microsoft SQL Server kimlik bilgileri yönetimi** ve **ıaas Aracısı Microsoft SQL Server**. Hatalardan biri aşağıdakine benzer olabilir:

      SQL Server ile bağlantı kurulmaya çalışılırken ağ ile ilişkili veya örneğe özgü bir hata oluştu. Sunucu bulunamadı veya erişilebilir değildi.

   Varsayılan örneği kaldırmaya karar verirseniz Ayrıca [SQL Server IaaS Aracısı uzantısını](sql-server-iaas-agent-extension-automate-management.md) da kaldırın. 

1. **IaaS uzantılı SQL Server adlandırılmış bir örneğini kullanabilir miyim**?
   
   Evet, adlandırılmış örnek SQL Server tek örnek ise ve özgün varsayılan örnek [düzgün şekilde kaldırılmışsa](sql-server-iaas-agent-extension-automate-management.md#install-on-a-vm-with-a-single-named-sql-server-instance). Varsayılan örnek yoksa ve tek bir SQL Server VM birden çok adlandırılmış örnek varsa, SQL Server IaaS Aracısı uzantısı yüklenemeyecektir. 

1. **SQL Server'ı SQL Server VM'sinden tamamen kaldırabilir miyim?**

   Evet, ancak [SQL Server Azure VM 'leri Için fiyatlandırma Kılavuzu](pricing-guidance.md)' nda açıklandığı gibi SQL Server VM ücretlendirilmeye devam edersiniz. Artık SQL Server'a ihtiyacınız kalmadıysa yeni bir sanal makine dağıtabilir ve verilerle uygulamaları yeni sanal makineye geçirebilirsiniz. Ardından SQL Server sanal makinesini kaldırabilirsiniz.

1. **Aynı VM'de birden çok örneği yönetmek için Azure portalını kullanabilir miyim?**
   Hayır. Portal yönetimi, SQL Server IaaS Aracısı uzantısına dayanan SQL VM kaynak sağlayıcısı tarafından sağlanır. Bu nedenle, aynı sınırlamalar, kaynak sağlayıcısı uzantısı olarak da geçerlidir. Portal, doğru yapılandırıldığı sürece yalnızca bir varsayılan örneği veya bir adlandırılmış örneği yönetebilir. Daha fazla bilgi için bkz. [IaaS Aracısı uzantısı SQL Server](sql-server-iaas-agent-extension-automate-management.md) 
   
## <a name="updating-and-patching"></a>Güncelleştirme ve düzeltme eki uygulama

1. **Nasıl yaparım?, bir Azure sanal makinesinde SQL Server farklı bir sürüme/sürümüne değiştirilsin mi?**

   Müşteriler SQL Server'ın istenen sürümünü veya yayınını içeren kurulum medyasını kullanarak SQL Server sürümlerini/yayınlarını değiştirebilir. Yayın değiştikten sonra Azure portalını kullanarak VM için faturalamayı doğru yansıtacak şekilde VM'nin yayın özelliğini değiştirin. Daha fazla bilgi için bkz. [SQL Server VM sürümünü değiştirme](change-sql-server-edition.md). Farklı SQL Server sürümleri için faturalandırma farkı olmadığından SQL Server sürümünü değiştirdikten sonra başka bir şey yapmanız gerekmez.

1. **SQL Server sürümünü değiştirmek için kurulum medyasını nereden edinebilirim?**

   [Yazılım güvencesi](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default) sahibi olan müşteriler, yükleme medyasını [toplu lisanslama merkezinden](https://www.microsoft.com/Licensing/servicecenter/default.aspx)elde edebilir. Yazılım güvencesi olmayan müşteriler, istenen sürümü olan bir Azure Marketi SQL Server VM görüntüsünden kurulum medyasını kullanabilir.
   
1. **Güncelleştirmeler ve hizmet paketleri SQL Server VM'sine nasıl uygulanır?**

   Sanal makineler güncelleştirmeleri ne zaman ve nasıl uygulayacağınız dahil olmak üzere ana makine üzerinde denetim sağlar. İşletim sistemi için Windows güncelleştirmelerini el ile uygulayabilirsiniz veya [Otomatik Düzeltme Eki Uygulama](automated-patching.md) adlı bir zamanlama hizmetini etkinleştirebilirsiniz. Otomatik Düzeltme Eki Uygulama, önemli olarak işaretlenmiş tüm güncelleştirmeleri, bu kategorideki SQL Server güncelleştirmeleri de dahil olmak üzere yükler. SQL Server için diğer isteğe bağlı güncelleştirmeler el ile yüklenmelidir.

1. **SQL Server 2008/2008 R2 örneğinden SQL Server VM kaynak sağlayıcısına kaydettikten sonra yükseltebilir miyim?**

   Evet. SQL Server sürümünü yükseltmek için herhangi bir kurulum medyası kullanabilir ve sonra [SQL IaaS uzantı modınızı](sql-vm-resource-provider-register.md#management-modes) _aracıdan_ _tam_' a yükseltebilirsiniz. Bunun yapılması, Portal yönetilebilirliği, otomatik yedeklemeler ve otomatik düzeltme eki uygulama gibi SQL IaaS uzantısının tüm avantajlarına erişmenizi sağlayacaktır. 

1. **Destek süresi sona eren SQL Server 2008 ve SQL Server 2008 R2 örneklerim için ücretsiz genişletilmiş güvenlik güncelleştirmelerini nasıl edinebilirim?**

   SQL Server olduğu gibi bir Azure sanal makinesine taşıyarak, [ücretsiz genişletilmiş güvenlik güncelleştirmeleri](sql-server-2008-extend-end-of-support.md) alabilirsiniz. Daha fazla bilgi için bkz. [Destek süresi sonu seçenekleri](/sql/sql-server/end-of-support/sql-server-end-of-life-overview). 
  
   

## <a name="general"></a>Genel

1. **SQL Server yük devretme kümesi örneği (FCı) Azure VM 'lerinde destekleniyor mu?**

   Evet. Depolama alt sistemi için [Premium dosya paylaşımları (PFS)](failover-cluster-instance-premium-file-share-manually-configure.md) veya [depolama ALANLARı doğrudan (S2D)](failover-cluster-instance-storage-spaces-direct-manually-configure.md) kullanarak bir yük devretme kümesi örneği yükleyebilirsiniz. Premium dosya paylaşımları, birçok iş yükünün ihtiyaçlarını karşılayacak ıOPS ve aktarım hızı kapasitesi sağlar. GÇ yoğun iş yükleri için, manşlı Premium veya ultra disklere dayalı depolama alanları doğrudan kullanmayı düşünün. Alternatif olarak, [Azure sanal makinelerinde SQL Server Için yüksek kullanılabilirlik ve olağanüstü durum kurtarma](business-continuity-high-availability-disaster-recovery-hadr-overview.md#azure-only-high-availability-solutions)bölümünde açıklandığı gibi üçüncü taraf kümeleme veya depolama çözümlerini de kullanabilirsiniz.

   > [!IMPORTANT]
   > Şu anda _tam_ [SQL Server IaaS Aracısı uzantısı](sql-server-iaas-agent-extension-automate-management.md) , Azure 'da SQL Server FCI için desteklenmez. FCı 'ye katılan VM 'lerden _tam_ uzantıyı kaldırmanızı ve bunun yerine eklentiyi _hafif_ modda yüklemenizi öneririz. Bu uzantı, SQL Server için otomatik yedekleme ve düzeltme eki uygulama ve bazı Portal özellikleri gibi özellikleri destekler. Bu özellikler, _tam_ aracı kaldırıldıktan sonra SQL Server VM 'ler için çalışmayacaktır.

1. **SQL Server VM 'Ler ile SQL veritabanı hizmeti arasındaki fark nedir?**

   Kavramsal olarak, Azure sanal makinesinde SQL Server çalıştırmak, uzak bir veri merkezinde SQL Server çalıştırmanın farklı değildir. Buna karşılık, [Azure SQL veritabanı](../../database/sql-database-paas-overview.md) hizmet olarak veritabanı sağlar. SQL veritabanı ile veritabanlarınızı barındıran makinelere erişiminiz yok. Tam karşılaştırma için bkz. [bulut SQL Server seçme seçeneği: Azure SQL (PaaS) veritabanı veya Azure VM 'lerinde SQL Server (IaaS)](../../azure-sql-iaas-vs-paas-what-is-overview.md).

1. **Azure VM 'imde SQL Data araçları 'nı Nasıl yaparım? yüklensin mi?**

    Microsoft SQL Server veri araçlarından SQL Data araçları 'nı indirip yükleyin. [Visual Studio 2013 Için Iş zekası](https://www.microsoft.com/download/details.aspx?id=42313).

1. **SQL Server VM 'lerde desteklenen MSDTC ile dağıtılmış işlemler mi var?**
   
    Evet. SQL Server 2016 SP2 ve üzeri için Yerel DTC desteklenir. Ancak, bir yük devretme sırasında hareket durumunda olan işlemler başarısız olur ve yeniden denenmek zorunda olduğundan uygulamalar her zaman açık kullanılabilirlik grupları kullanılırken test edilmiş olmalıdır. Kümelenmiş DTC, Windows Server 2019 ile başlayarak kullanılabilir. 

## <a name="resources"></a>Kaynaklar

**Windows VM 'leri**:

* [Windows VM 'de SQL Server genel bakış](sql-server-on-azure-vm-iaas-what-is-overview.md)
* [Windows VM 'de SQL Server sağlama](create-sql-vm-portal.md)
* [Bir veritabanını Azure VM 'de SQL Server geçirme](migrate-to-vm-from-sql-server.md)
* [Azure sanal makinelerinde SQL Server için yüksek kullanılabilirlik ve olağanüstü durum kurtarma](business-continuity-high-availability-disaster-recovery-hadr-overview.md)
* [Azure sanal makinelerinde SQL Server için en iyi performans uygulamaları](performance-guidelines-best-practices.md)
* [Azure sanal makinelerinde SQL Server için uygulama desenleri ve geliştirme stratejileri](application-patterns-development-strategies.md)

**Linux VM 'leri**:

* [Linux VM 'de SQL Server genel bakış](../linux/sql-server-on-linux-vm-what-is-iaas-overview.md)
* [Linux VM üzerinde SQL Server sağlama](../linux/sql-vm-create-portal-quickstart.md)
* [SSS (Linux)](../linux/frequently-asked-questions-faq.md)
* [Linux üzerinde SQL Server belgeleri](https://docs.microsoft.com/sql/linux/sql-server-linux-overview)
