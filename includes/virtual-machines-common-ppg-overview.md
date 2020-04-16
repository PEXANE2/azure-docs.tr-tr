---
title: include dosyası
description: include dosyası
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/30/2019
ms.author: zivr
ms.custom: include file
ms.openlocfilehash: fb2eb2d237a1245627bbdb6f4f2eacbb9966a2c6
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81421817"
---
VM'leri tek bir bölgeye yerleştirmek, örnekler arasındaki fiziksel mesafeyi azaltır. Bunları tek bir kullanılabilirlik bölgesine yerleştirmek de onları fiziksel olarak birbirine daha da yaklaştıracaktır. Ancak, Azure ayak izi büyüdükçe, tek bir kullanılabilirlik bölgesi birden çok fiziksel veri merkezine yayılabilir ve bu da bir ağ gecikmesinin uygulamanızı etkilemesine neden olabilir. 

VM'leri mümkün olduğunca yakın hale getirmek ve mümkün olan en düşük gecikmeyi elde etmek için, bunları bir yakınlık yerleşim grubu içinde dağıtmanız gerekir.

Yakınlık yerleşim grubu, Azure hesaplama kaynaklarının fiziksel olarak birbirine yakın olduğundan emin olmak için kullanılan mantıksal bir gruplandırmadır. Yakınlık yerleşimgrupları, düşük gecikme nin bir gereklilik olduğu iş yükleri için yararlıdır.


- Tek başına VM'ler arasında düşük gecikme.
- Tek bir kullanılabilirlik kümesinde veya sanal makine ölçeği kümesinde VM'ler arasında düşük gecikme gecikmesi. 
- Tek başına VM'ler, birden çok Kullanılabilirlik Kümesindeki VM'ler veya birden çok ölçek kümesi arasında düşük gecikme. Çok katmanlı bir uygulamayı bir araya getirmek için tek bir yerleşim grubunda birden çok işlem kaynağına sahip olabilirsiniz. 
- Farklı donanım türlerini kullanarak birden çok uygulama katmanı arasında düşük gecikme süresi. Örneğin, bir kullanılabilirlik kümesinde M-serisi kullanarak arka uç ve bir D-serisi örneğinde ön uç, bir ölçek kümesinde, tek bir yakınlık yerleşim grubunda çalıştırın.


![Yakınlık yerleşim grupları için grafik](./media/virtual-machines-common-ppg/ppg.png)

## <a name="using-proximity-placement-groups"></a>Yakınlık Yerleştirme Gruplarını Kullanma 

Yakınlık yerleşim grubu Azure'da yeni bir kaynak türüdür. Diğer kaynaklarla kullanmadan önce bir tane oluşturmanız gerekir. Oluşturulduktan sonra, sanal makineler, kullanılabilirlik kümeleri veya sanal makine ölçek kümeleri ile kullanılabilir. Yakınlık yerleşim grubu kimliğini sağlayan bilgi işlem kaynakları oluştururken bir yakınlık yerleşim grubu belirtirsiniz. 

Varolan bir kaynağı bir yakınlık yerleşim grubuna da taşıyabilirsiniz. Bir kaynağı yakınlık yerleşim grubuna aktarırken, ortak konum kısıtlamasını karşılamak için potansiyel olarak bölgedeki farklı bir veri merkezine yeniden dağıtılacak sayılacağına göre, önce varlığı durdurmanız (anlaşma nızı belirlemeniz) gerekir. 

Kullanılabilirlik kümeleri ve sanal makine ölçek kümeleri söz konusu olduğunda, yakınlık yerleşim grubunu tek tek sanal makineler yerine kaynak düzeyinde ayarlamanız gerekir. 

Yakınlık yerleşim grubu, sabitleme mekanizması yerine bir birlikte konum kısıtlamasIdır. Kullanmak için ilk kaynağın dağıtımı ile belirli bir veri merkezine sabitlenir. Yakınlık yerleşim grubunu kullanan tüm kaynaklar durdurulduktan (ayrıldıktan) veya silindikten sonra, artık sabitlenmez. Bu nedenle, birden çok VM serisi içeren bir yakınlık yerleşim grubu kullanırken, mümkün olduğunda bir şablonda gerekli tüm türleri önceden belirtmek veya başarılı bir dağıtım şansınızı artıracak bir dağıtım dizisini izlemek önemlidir. Dağıtımınız başarısız olursa, dağıtılacak ilk boyut olarak başarısız olan VM boyutuyla dağıtımı yeniden başlatın.

## <a name="what-to-expect-when-using-proximity-placement-groups"></a>Yakınlık Yerleştirme Gruplarını kullanırken neler bekleyebilirsiniz? 
Yakınlık yerleşim grupları aynı veri merkezinde ortak konum sunar. Ancak, yakınlık yerleşim grupları ek bir dağıtım kısıtlaması temsil ettiğiiçin, ayırma hataları oluşabilir. Yakınlık yerleşim işlemi gruplarını kullanırken tahsisat hatalarını gÜ

- Yakınlık yerleşim grubundaki ilk sanal makineyi istediğinizde, veri merkezi otomatik olarak seçilir. Bazı durumlarda, farklı bir sanal makine SKU için ikinci bir istek, bu veri merkezinde yoksa başarısız olabilir. Bu durumda, **overconstrainedAllocationRequest hatası döndürülür.** Bunu önlemek için, SNU'larınızı dağıtma sırasını değiştirmeyi deneyin veya her iki kaynağın da tek bir ARM şablonu kullanarak dağıtılmasını sağlar.
-   VM örneklerini ekleyip kaldırdığınız esnek iş yüklerinde, dağıtımınızda yakınlık yerleşim grubu kısıtlaması olması, **Ayırma Hatasıyla** sonuçlanan isteğin karşılanmamasına neden olabilir. 
- VM'lerinizi gerektiği gibi durdurmak (anlaşma yapmak) ve başlatmak elastikiyetelde etmenin başka bir yoludur. Bir VM'yi durdurduktan (anlaşma yaptığınızda) kapasite tutulmadığından, yeniden başlatma bir **TahsisAt Hatası** hatasına neden olabilir.


## <a name="best-practices"></a>En iyi uygulamalar 
- En düşük gecikme süremi için, hızlandırılmış ağ ile birlikte yakınlık yerleşim idurumlama gruplarını kullanın. Daha fazla bilgi için bkz: [Hızlandırılmış Ağ ile Bir Linux sanal makine oluştur](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) veya [Hızlandırılmış Ağ ile Bir Windows sanal makine oluşturun.](/azure/virtual-network/create-vm-accelerated-networking-powershell?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
- Tüm VM boyutlarını tek bir şablonda dağıtın. İhtiyacınız olan tüm VM SNU'ları ve boyutları desteklemeyen donanımlara inmemek için, tüm uygulama katmanlarını tek bir şablona dahil edin, böylece hepsi aynı anda dağıtılacaktır.
- Dağıtımınızı PowerShell, CLI veya SDK kullanarak komut dosyası yla komut `OverconstrainedAllocationRequest`dosyası yla yazıyorsanız, bir ayırma hatası alabilirsiniz. Bu durumda, varolan tüm VM'leri durdurmalı/işlem yapmalı ve dağıtım komut dosyasındaki sırayı başarısız olan VM SKU/boyutlarla başlayacak şekilde değiştirmelisiniz. 
- VM'lerin silindiği varolan bir yerleşim grubunu yeniden kullanırken, vm'ler eklemeden önce silme işleminin tam olarak tamamlanmasını bekleyin.
- Gecikme seçimi ilk öncelikinizse, VM' leri bir yakınlık yerleşim grubuna ve bütün çözümü bir dolum bölgesinde tıv' ları sızdır. Ancak, esneklik en önemli önceliğinizse, örneklerinizi birden çok kullanılabilirlik bölgesine yayıltın (tek bir yakınlık yerleşim grubu bölgeleri kaplayamaz).
