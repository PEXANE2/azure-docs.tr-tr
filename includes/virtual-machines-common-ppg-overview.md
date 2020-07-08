---
title: dosya dahil etme
description: dosya dahil etme
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/30/2019
ms.author: zivr
ms.custom: include file
ms.openlocfilehash: fb2eb2d237a1245627bbdb6f4f2eacbb9966a2c6
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "81421817"
---
VM 'Lerin tek bir bölgeye yerleştirilmesi, örnekler arasındaki fiziksel mesafeyi azaltır. Bunları tek bir kullanılabilirlik alanına koymak, bunları fiziksel olarak bir araya getirir. Ancak, Azure ayak izi büyüdükçe, tek bir kullanılabilirlik alanı birden fazla fiziksel veri merkezine yayılabilir ve bu da uygulamanızı etkileyen bir ağ gecikme süresi oluşmasına neden olabilir. 

Olası en düşük gecikme süresini elde etmek üzere VM 'Leri olabildiğince yakın bir şekilde almak için, bunları bir yakınlık yerleşimi grubu içinde dağıtmanız gerekir.

Yakınlık yerleşimi grubu, Azure işlem kaynaklarının fiziksel olarak birbirlerine yakın bir yerde bulunduğundan emin olmak için kullanılan mantıksal bir gruplandırmadır. Yakınlık yerleşimi grupları, düşük gecikme süresinin gereksinim olduğu iş yükleri için faydalıdır.


- Tek başına VM 'Ler arasında düşük gecikme süresi.
- Tek bir kullanılabilirlik kümesindeki veya bir sanal makine ölçek kümesindeki VM 'Ler arasında düşük gecikme süresi. 
- Tek başına VM 'Ler, birden çok kullanılabilirlik kümesindeki VM 'Ler veya birden çok ölçek kümesi arasında düşük gecikme süresi. Çok katmanlı bir uygulamayı bir araya getirmek için tek bir yerleştirme grubunda birden çok işlem kaynağınız olabilir. 
- Farklı donanım türleri kullanan birden çok uygulama katmanı arasında düşük gecikme süresi. Örneğin, arka ucu bir kullanılabilirlik kümesinde M serisi ile ve bir bir D serisi örneğinde, bir ölçek kümesinde, tek bir yakınlık yerleşimi grubunda ön uç ile çalışır.


![Yakınlık yerleşimi grupları için grafik](./media/virtual-machines-common-ppg/ppg.png)

## <a name="using-proximity-placement-groups"></a>Yakınlık yerleştirme gruplarını kullanma 

Yakınlık yerleşimi grubu, Azure 'da yeni bir kaynak türüdür. Diğer kaynaklarla kullanmadan önce bir tane oluşturmanız gerekir. Oluşturulduktan sonra sanal makineler, kullanılabilirlik kümeleri veya sanal makine ölçek kümeleri ile birlikte kullanılabilir. Yakınlık yerleşimi grubu KIMLIĞINI sağlayan işlem kaynaklarını oluştururken bir yakınlık yerleşimi grubu belirtirsiniz. 

Ayrıca, mevcut bir kaynağı bir yakınlık yerleşimi grubuna taşıyabilirsiniz. Bir kaynağı bir yakınlık yerleşimi grubuna taşırken, bir varlık potansiyel olarak bölgede farklı bir veri merkezine dağıtıldıktan sonra, birlikte bulundurma kısıtlamasını karşılamadan önce varlığı durdurmanız (serbest bırakma) gerekir. 

Kullanılabilirlik kümeleri ve sanal makine ölçek kümeleri söz konusu olduğunda, yakınlık yerleşimi grubunu ayrı sanal makineler yerine kaynak düzeyinde ayarlamanız gerekir. 

Yakınlık yerleşimi grubu, sabitleme mekanizması yerine bir birlikte bulundurma kısıtlamasıdır. Onu kullanmak için ilk kaynağın dağıtımıyla belirli bir veri merkezine sabitlenmiştir. Yakınlık yerleşimi grubunu kullanan tüm kaynaklar durdurulduktan (serbest bırakıldığında) veya silindikten sonra artık sabitlenemez. Bu nedenle, birden çok VM serisine sahip bir yakınlık yerleşimi grubu kullanırken, mümkün olduğunda bir şablonda gerekli türlerin tümünü belirtmek önemlidir veya dağıtım sırasını izleyerek başarılı bir dağıtıma yönelik olma olasılığını artırır. Dağıtımınız başarısız olursa, dağıtımı ilk boyut olarak başarısız olan VM boyutuyla birlikte yeniden başlatın.

## <a name="what-to-expect-when-using-proximity-placement-groups"></a>Yakınlık yerleşimi grupları kullanılırken beklenmeniz gerekenler 
Yakınlık yerleşimi grupları aynı veri merkezinde birlikte bulundurulan bir konum sunar. Bununla birlikte, yakınlık yerleşimi grupları ek bir dağıtım kısıtlamasını temsil ettiğinden, ayırma arızaları meydana gelebilir. Yakınlık yerleşimi gruplarını kullanırken ayırma başarısızlıklarını görebileceğiniz birkaç kullanım durumu vardır:

- Yakınlık yerleşimi grubundaki ilk sanal makineyi sorduğunuz zaman, veri merkezi otomatik olarak seçilir. Bazı durumlarda, farklı bir sanal makine SKU 'SU için ikinci bir istek, bu veri merkezinde yoksa başarısız olabilir. Bu durumda, bir **OverconstrainedAllocationRequest** hatası döndürülür. Bunu önlemek için, SKU 'larınızı dağıttığınız sırayı değiştirmeyi deneyin veya her iki kaynağın tek bir ARM şablonu kullanılarak dağıtılmasını sağlayabilirsiniz.
-   Sanal makine örneklerini eklediğiniz ve kaldırdığınız elastik iş yükleri söz konusu olduğunda, dağıtımınızda bir yakınlık yerleşimi grubu kısıtlamasının olması, bu isteğin karşılamamasından kaynaklanan, **Allocationfailure** hatasına neden olabilir. 
- Gerekli olduğu gibi sanal makinelerinizin durdurulması (serbest bırakma) ve başlatılması, esneklik elde etmenin başka bir yoludur. Bir VM 'yi durdurup (serbest bırakma) bir VM 'yi durdurup yeniden başlatmak, bir **Allocationfailure** hatasına neden olabilir.


## <a name="best-practices"></a>En iyi uygulamalar 
- En düşük gecikme süresi için, hızlandırılmış ağlarla birlikte yakınlık yerleştirme gruplarını kullanın. Daha fazla bilgi için bkz. [hızlandırılmış ağ Ile Linux sanal makinesi oluşturma](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) veya [hızlandırılmış ağ ile Windows sanal makinesi oluşturma](/azure/virtual-network/create-vm-accelerated-networking-powershell?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
- Tüm VM boyutlarını tek bir şablonda dağıtın. İhtiyaç duyduğunuz tüm VM SKU 'Larını ve boyutlarını desteklemeyen donanımlarda sahanın önüne geçmek için, tüm uygulama katmanlarını tek bir şablona dahil edin, böylece bunların hepsi aynı anda dağıtılacaktır.
- Dağıtımınızı PowerShell, CLı veya SDK kullanarak yaparsanız, bir ayırma hatası alabilirsiniz `OverconstrainedAllocationRequest` . Bu durumda, mevcut tüm VM 'Leri durdurup serbest bırakabilir ve dağıtım betiğindeki sırayı, başarısız olan VM SKU 'SU/boyutlarına başlayacak şekilde değiştirmelisiniz. 
- VM 'lerden silinen mevcut bir yerleştirme grubunu yeniden kullandığınızda, VM 'Leri eklemeden önce silme işleminin tam tamamlanmasını bekleyin.
- Gecikme süresi ilk önceliğiniz ise, VM 'Leri bir yakınlık yerleşimi grubuna ve çözümün tamamına bir kullanılabilirlik alanına yerleştirin. Ancak dayanıklılık en üst önceliğiniz ise, örneklerinizi birden çok kullanılabilirlik alanına (tek bir yakınlık yerleştirme grubu bölgelere yayılamaz) yayın.
