---
title: Azure'da Windows VM'lere Genel Bakış
description: Azure'daki Windows sanal makinelerine genel bakış.
services: virtual-machines-windows
author: cynthn
manager: gwallace
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: overview
ms.date: 11/14/2019
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: efcbdb2ebd7f4830214dbd0f2ea2ea1cfe36c3de
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "75893509"
---
# <a name="windows-virtual-machines-in-azure"></a>Azure’da Windows sanal makineleri

Azure Virtual Machines (VM) Azure’un sunduğu [isteğe bağlı ve ölçeklenebilir işlem kaynağı türlerinden](/azure/architecture/guide/technology-choices/compute-decision-tree) biridir. Genellikle, sunulan diğer seçimlere göre bilgi işlem ortamınız üzerinde daha fazla denetime ihtiyacınız varsa, bir VM seçersiniz. Bu makalede VM oluşturmadan önce dikkat etmeniz gereken bilgilere, oluşturma yöntemine ve yönetim seçeneklerine yer verilmiştir.

Bir Azure VM, onu çalıştıran fiziksel donanımı satın almanıza ve muhafaza etmenize gerek kalmadan size sanallaştırma esnekliği sunar. Ancak yine de yapılandırma, düzeltme eki uygulama ve yazılım yükleme gibi görevleri gerçekleştirerek VM’nin bakımını yapmanız gerekir.

Azure sanal makineleri farklı amaçlarla kullanılabilir. Bazı örnekler şunlardır:

* **Geliştirme ve test etme** – Azure VM’leri bir uygulama için kod yazmak ve test yapmak için özel yapılandırmalara sahip bilgisayarları hızlıca oluşturmanızı sağlar.
* **Uygulamaları buluta taşıma** – Talep düzeyinde dalgalanmalar olan uygulamalarınızı Azure üzerindeki bir VM’de çalıştırmak mantıklıdır. İhtiyaç duyduğunuzda oluşturulan ek VM’ler için ödeme yapar, ihtiyaç kalmadığında bunları kapatırsınız.
* **Veri merkezini genişletme** – Bir Azure sanal ağı üzerindeki sanal makineleri kolayca kuruluşunuzun ağına bağlayabilirsiniz.

Uygulamanız tarafından kullanılan VM sayısı, ihtiyaçlarınıza göre ölçeklendirilebilir.

## <a name="what-do-i-need-to-think-about-before-creating-a-vm"></a>VM oluşturmadan önce dikkat etmem gereken noktalar nelerdir?
Azure’da uygulama altyapısı oluştururken [dikkate almanız gereken tasarım ölçütleri](https://docs.microsoft.com/azure/architecture/reference-architectures/n-tier/windows-vm) vardır. Başlamadan önce dikkat etmeniz gereken VM özellikleri şunlardır:

* Uygulama kaynaklarınızın adları
* Kaynakların depolanacağı konum
* VM’nin boyutu
* Oluşturulabilecek en fazla VM sayısı
* VM üzerinde çalışan işletim sistemi
* VM’nin başlatıldıktan sonraki yapılandırması
* VM’nin ihtiyaç duyduğu kaynaklar

### <a name="locations"></a>Konumlar
Azure’da oluşturulan tüm kaynaklar, dünyanın farklı yerindeki çeşitli [coğrafi bölgelere](https://azure.microsoft.com/regions/) dağıtılır. VM oluştururken bu bölgeler genelde **konum** olarak adlandırılır. Bir VM için konum, sanal sabit disklerin depolandığı yeri belirtir.

Bu tabloda, kullanılabilen konumların listesini edinme yöntemlerinden bazıları gösterilmektedir.

| Yöntem | Açıklama |
| --- | --- |
| Azure portalında |VM oluştururken listeden konum seçin. |
| Azure PowerShell |[Get-AzLocation](https://docs.microsoft.com/powershell/module/az.resources/get-azlocation) komutunu kullanın. |
| REST API |[List locations](https://docs.microsoft.com/rest/api/resources/subscriptions) işlemini kullanın. |
| Azure CLI |[az account list-locations](https://docs.microsoft.com/cli/azure/account?view=azure-cli-latest) işlemini kullanın. |

## <a name="availability"></a>Kullanılabilirlik
Azure, sanal makineyi tüm diskler için premium depolamayla dağıtmanız koşuluyla, tek örnekli sanal makinelerde endüstri lideri %99,9 kullanılabilirlik Hizmet Düzeyi Sözleşmesi'nin duyurusunu yaptı.  Dağıtımınızın standart %99,95 VM Hizmet Düzeyi Sözleşmesinin kapsamına girebilmesi için iş yükünüzü çalıştıran iki veya daha fazla VM’yi yine bir kullanılabilirlik kümesi içinde dağıtmanız gerekir. Bir kullanılabilirlik kümesi, VM’lerinizin Azure veri merkezlerinde birden çok hata etki alanına dağıtılmasını ve aynı zamanda dağıtımlarının farklı bakım aralıklarına sahip konaklara yapılmasını sağlar. [Azure SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) şartları, Azure’un tamamının kullanılabilirlik garantisini açıklamaktadır.


## <a name="vm-size"></a>VM boyutu
Kullandığınız VM’nin [boyutu](sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), çalıştırmak istediğiniz iş yüküne göre belirlenir. Seçtiğiniz boyut işlemci gücü, bellek ve depolama kapasitesi gibi ölçütleri belirler. Azure çok sayıda kullanım türünü desteklemek için büyük çeşitlilikteki boyutları sunar.

Azure, VM'nin boyutuna ve işletim sistemine bağlı olarak saatlik bir [fiyat](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) ücrete dahil eder. Kısmi saatler için, Azure yalnızca kullanılan dakikaları ücretlendirir. Depolama ayrı olarak fiyatlandırılır ve ücretlendirilir.

## <a name="vm-limits"></a>VM Sınırları
Aboneliğinizde, projeniz için birden fazla VM dağıtımını etkileyebilecek varsayılan [kota sınırları](../../azure-resource-manager/management/azure-subscription-service-limits.md) vardır. Geçerli sınırlar abonelik başına her bölge için 20 VM olarak belirlenmiştir. Sınırların [yükseltilmesini talep etmek için destek bileti oluşturabilirsiniz](../../azure-portal/supportability/resource-manager-core-quotas-request.md)

### <a name="operating-system-disks-and-images"></a>İşletim sistemi diskleri ve görüntüleri
Sanal makineler, kendi işletim sistemlerini (OS) ve verilerini depolamak için [sanal sabit diskleri (VHD)](managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) kullanır. VHD bir işletim sistemi yüklemek için seçebileceğiniz görüntüler için de kullanılır. 

Azure’da Windows Server işletim sistemlerinin farklı sürümleri ve türleri ile birlikte kullanılabilecek birçok [market görüntüsü](https://azure.microsoft.com/marketplace/virtual-machines/) bulunmaktadır. Market görüntüleri; görüntü yayımcısı, teklif, sku ve sürüm (genelde sürüm en son belirtilir) bilgileriyle tanımlanır. Yalnızca 64 bit işletim sistemleri desteklenir. Desteklenen konuk işletim sistemleri, roller ve özellikler hakkında daha fazla bilgi için bkz. [Microsoft Azure sanal makineleri için Microsoft sunucu yazılımı desteği](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines).

Bu tabloda bir görüntünün bilgilerine nasıl erişebileceğiniz gösterilmiştir.

| Yöntem | Açıklama |
| --- | --- |
| Azure portalında |Bir görüntüyü kullanmak istediğinizde değerler otomatik olarak belirtilir. |
| Azure PowerShell |[Get-AzVMImagePublisher](https://docs.microsoft.com/powershell/module/az.compute/get-azvmimagepublisher) -Konum *konumu*<BR>[Get-AzVMImageOffer](https://docs.microsoft.com/powershell/module/az.compute/get-azvmimageoffer) -Konum *konumu* *-PublisherPublisherName*<BR>[Get-AzVMImageSku](https://docs.microsoft.com/powershell/module/az.compute/get-azvmimagesku) -Konum *konumu* *-PublisherPublisherName* -Teklif *teklifName* |
| REST API'leri |[Görüntü yayımcılarını listeleme](https://docs.microsoft.com/rest/api/compute/platformimages/platformimages-list-publishers)<BR>[Görüntü tekliflerini listeleme](https://docs.microsoft.com/rest/api/compute/platformimages/platformimages-list-publisher-offers)<BR>[Görüntü sku’larını listeleme](https://docs.microsoft.com/rest/api/compute/platformimages/platformimages-list-publisher-offer-skus) |
| Azure CLI |[az vm resim listesi-yayıncılar](https://docs.microsoft.com/cli/azure/vm/image?view=azure-cli-latest) *--konum konumu*<BR>[az vm resim listesi-teklifler](https://docs.microsoft.com/cli/azure/vm/image?view=azure-cli-latest) --konum *konumu* *--publisherPublisherName*<BR>[az vm resim listesi-skus](https://docs.microsoft.com/cli/azure/vm?view=azure-cli-latest) --konum *konumu* *--publisherName* *--teklifname*|

[Kendi görüntünüzü yükleyip kullanmanız halinde](upload-generalized-managed.md) yayımcı adı, teklif ve sku kullanılmaz.

### <a name="extensions"></a>Uzantılar
VM [uzantıları](extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), dağıtım sonrası yapılandırma ve otomatik görevlerle VM’nize yeni özellikler ekler.

Uzantıları kullanarak şu genel görevleri gerçekleştirebilirsiniz:

* **Özel betik çalıştırma** – [Özel Betik Uzantısı](extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), VM hazırlandığında kendi betiğinizi çalıştırarak iş yükü yapılandırması gerçekleştirmenize yardımcı olur.
* **Yapılandırma dağıtma ve yönetme** – [PowerShell İstenen Durum Yapılandırması (DSC) Uzantısı](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), yapılandırma ve ortam yönetimi amacıyla bir VM üzerinde DSC kurulumu yapmanıza yardımcı olur.
* **Tanılama verilerini toplama** – [Azure Tanılama Uzantısı](extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), VM’i uygulama durumunu izlemek için kullanılabilecek tanılama verilerini toplayacak şekilde yapılandırmanıza yardımcı olur.

### <a name="related-resources"></a>İlgili kaynaklar
Bu tablodaki kaynaklar VM tarafından kullanılır ve VM oluşturulduğunda mevcut olmaları ya da oluşturulmaları gerekir.

| Kaynak | Gerekli | Açıklama |
| --- | --- | --- |
| [Kaynak grubu](../../azure-resource-manager/management/overview.md) |Evet |VM bir kaynak grubunda yer almalıdır. |
| [Depolama hesabı](../../storage/common/storage-create-storage-account.md) |Evet |VM, sanal sabit disklerini depolamak için bir depolama hesabına ihtiyaç duyar. |
| [Sanal ağ](../../virtual-network/virtual-networks-overview.md) |Evet |VM’in bir sanal ağa üye olması gerekir. |
| [Genel IP adresi](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) |Hayır |VM, uzaktan erişim için atanmış bir genel IP adresine sahip olabilir. |
| [Ağ arabirimi](../../virtual-network/virtual-network-network-interface.md) |Evet |VM’in ağda iletişim kurabilmek için ağ arabirimine ihtiyacı vardır. |
| [Veri diskleri](attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) |Hayır |VM, depolama olanaklarını genişletmek için veri disklerine sahip olabilir. |

## <a name="next-steps"></a>Sonraki adımlar

İlk VM'inizi oluşturun!

- [Portal](quick-create-portal.md)
- [Powershell](quick-create-powershell.md)
- [Azure CLI](quick-create-cli.md)

