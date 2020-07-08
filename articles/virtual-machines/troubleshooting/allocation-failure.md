---
title: Azure VM ayırma hatalarında sorun giderme | Microsoft Docs
description: Azure 'da VM oluşturma, yeniden başlatma veya yeniden boyutlandırma sırasında ayırma hatalarıyla ilgili sorunları giderme
services: virtual-machines
documentationcenter: ''
author: DavidCBerry13
manager: felixwu
editor: ''
tags: top-support-issue,azure-resource-manager,azure-service-management
ms.assetid: 1ef41144-6dd6-4a56-b180-9d8b3d05eae7
ms.service: virtual-machines
ms.topic: troubleshooting
ms.date: 04/13/2018
ms.author: daberry
ms.openlocfilehash: fdbf07fa51adf8151e80d230734ebe53d36b5390
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "83124797"
---
# <a name="troubleshoot-allocation-failures-when-you-create-restart-or-resize-vms-in-azure"></a>Azure’da VM’leri oluştururken, yeniden başlatırken ve yeniden boyutlandırırken ortaya çıkan ayırma hatalarında sorun giderme

Bir sanal makine (VM) oluşturduğunuzda, yeniden başlatma (serbest bırakıldı) VM 'Leri yeniden başlatın veya bir VM 'yi yeniden boyutlandırdığınızda Microsoft Azure işlem kaynaklarını aboneliğinize ayırır. Her zaman müşteri talebini desteklemek için kullanılabilir tüm VM türlerini kullandığınızdan emin olmak için ek altyapıya ve özelliklere sürekli yatırım yaptık. Bununla birlikte, belirli bölgelerde Azure hizmetleri için isteğe bağlı olarak daha fazla büyüme nedeniyle kaynak ayırma hatalarıyla karşılaşabilirsiniz. Bu sorun, bir bölgede VM 'Leri oluşturmaya veya başlatmaya çalıştığınızda oluşabilir ve VM 'Ler aşağıdaki hata kodunu ve iletisini görüntüler:

**Hata kodu**: allocationfailed veya ZonalAllocationFailed

**Hata iletisi**: "ayırma başarısız oldu. Bu bölgedeki istenen VM boyutu için yeterli kapasiteye sahip değilsiniz. Https: \/ /aka.MS/Allocation-Guidance ", ayırma başarısını oluşma olasılığını artırma hakkında daha fazla bilgi edinin

Bu makalede bazı yaygın ayırma hatalarının nedenleri açıklanmakta ve olası düzeltmeler önerilmektedir.

Azure sorununuz bu makalede giderilmemişse, [MSDN ve Stack Overflow Azure forumlarını](https://azure.microsoft.com/support/forums/)ziyaret edin. Sorununuzu bu forumlarda veya @AzureSupport Twitter 'da gönderebilirsiniz. Ayrıca, [Azure](https://azure.microsoft.com/support/options/) destek sitesinde destek al ' ı seçerek bir Azure destek isteği oluşturabilirsiniz.

Tercih ettiğiniz sanal makine türü tercih ettiğiniz bölgede kullanılabilir olana kadar, dağıtım sorunlarıyla karşılaşan müşterilerimiz için aşağıdaki tabloda yer alan ve geçici bir geçici çözüm olarak kılavuzluk yapmayı öneririz. 

Büyük/küçük harflere en iyi eşleşen senaryoyu belirleyip, ayırma başarısını artırmak için ilgili önerilen geçici çözümü kullanarak ayırma isteğini yeniden deneyin. Alternatif olarak, daha sonra her zaman yeniden deneyebilirsiniz. Bunun nedeni, isteğinizi karşılamak için kümede, bölgede veya bölgede yeterli kaynak bırakılmış olabilir. 


## <a name="resize-a-vm-or-add-vms-to-an-existing-availability-set"></a>Bir veya birden çok VM'yi mevcut kullanılabilirlik kümesine yeniden boyutlandırma

### <a name="cause"></a>Nedeni

Bir VM 'yi yeniden boyutlandırma veya var olan bir kullanılabilirlik kümesine VM ekleme isteği, var olan kullanılabilirlik kümesini barındıran orijinal kümede denenmelidir. İstenen VM boyutu küme tarafından destekleniyor, ancak küme şu anda yeterli kapasiteye sahip olmayabilir. 

### <a name="workaround"></a>Geçici çözüm

VM farklı bir kullanılabilirlik kümesinin parçası olabilir, farklı bir kullanılabilirlik kümesinde (aynı bölgede) bir VM oluşturun. Bu yeni VM daha sonra aynı sanal ağa eklenebilir.

Aynı Kullanılabilirlik kümesindeki tüm VM 'Leri durdurun (serbest bırakın) ve ardından her birini yeniden başlatın.
Durdurmak için: kaynak grupları > [kaynak grubunuz] > kaynaklar > [kullanılabilirlik kümesi] > sanal makineler > [sanal makineniz] > Durdur ' a tıklayın.
Tüm VM 'Leri durdurduktan sonra, ilk VM 'yi seçin ve ardından Başlat ' a tıklayın.
Bu adım, yeni bir ayırma denemesinin çalıştırılmasının ve yeterli kapasiteye sahip yeni bir kümenin seçilebileceğinden emin olmanızı sağlar.

## <a name="restart-partially-stopped-deallocated-vms"></a>Kısmen durdurulmuş (serbest bırakılmış) VM'leri yeniden başlatma

### <a name="cause"></a>Nedeni

Kısmi olarak ayırmayı kaldırma, bir kullanılabilirlik kümesindeki VM 'Leri bir veya daha fazla değil, bir veya daha fazla sanal makineyi durdurduğunuzun anlamına gelir. Bir VM 'yi serbest bırakırsanız, ilişkili kaynaklar serbest bırakılır. Bir kısmen serbest bırakılmış kullanılabilirlik kümesindeki VM 'Lerin yeniden başlatılması, mevcut bir kullanılabilirlik kümesine VM ekleme ile aynıdır. Bu nedenle, ayırma isteğinin yeterli kapasiteye sahip olmayan mevcut kullanılabilirlik kümesini barındıran orijinal kümede denenmelidir.

### <a name="workaround"></a>Geçici çözüm

Aynı Kullanılabilirlik kümesindeki tüm VM 'Leri durdurun (serbest bırakın) ve ardından her birini yeniden başlatın.
Durdurmak için: kaynak grupları > [kaynak grubunuz] > kaynaklar > [kullanılabilirlik kümesi] > sanal makineler > [sanal makineniz] > Durdur ' a tıklayın.
Tüm VM 'Leri durdurduktan sonra, ilk VM 'yi seçin ve ardından Başlat ' a tıklayın.
Bu, yeni bir ayırma denemesinin çalıştırılmasının ve yeterli kapasiteye sahip yeni bir kümenin seçilebileceğinden emin olur.

## <a name="restart-fully-stopped-deallocated-vms"></a>Tamamen durdurulmuş (serbest bırakılmış) VM'leri yeniden başlatma

### <a name="cause"></a>Nedeni

Tam kaldırma, bir kullanılabilirlik kümesindeki tüm VM 'Leri durdurduğunuzun (serbest bırakıldığı) anlamına gelir. Bu VM 'Leri yeniden başlatmaya yönelik ayırma isteği bölge veya bölge içinde istenen boyutu destekleyen tüm kümeleri hedefleyecektir. Bu makaledeki öneriler uyarınca ayırma isteğinizi değiştirin ve başarılı ayırma olasılığını artırmak için isteği yeniden deneyin. 

### <a name="workaround"></a>Geçici çözüm

Eski VM serisini veya Dv1, DSv1, AV1, D15v2 veya DS15v2 gibi boyutları kullanırsanız, daha yeni sürümlere geçmeyi göz önünde bulundurun. Belirli VM boyutları için bu önerilere bakın.
Farklı bir VM boyutu kullanma seçeneğiniz yoksa, aynı coğrafi bölgede farklı bir bölgeye dağıtım yapmayı deneyin. Her bölgedeki kullanılabilir VM boyutları hakkında daha fazla bilgi içinhttps://aka.ms/azure-regions

Kullanılabilirlik alanları kullanıyorsanız, bölge dahilinde istenen VM boyutu için kullanılabilir kapasiteye sahip olabilecek başka bir bölge deneyin.

Ayırma isteğiniz büyükse (500 çekirdekten fazla), isteği daha küçük dağıtımlara bölmek için aşağıdaki bölümlerdeki kılavuza bakın.

[VM 'yi yeniden dağıtmaya](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/redeploy-to-new-node-windows)çalışın. VM 'nin yeniden dağıtılması, VM 'yi bölge içindeki yeni bir kümeye ayırır.

## <a name="allocation-failures-for-older-vm-sizes-av1-dv1-dsv1-d15v2-ds15v2-etc"></a>Daha eski VM boyutları (Av1, Dv1, DSv1, D15v2, DS15v2, vb.) için ayırma hataları

Azure altyapısını genişlettiğimiz gibi, en son sanal makine türlerini desteklemek için tasarlanan daha yeni nesil donanımlar dağıyoruz. Eski serisi sanal makinelerin bazıları en son nesil altyapımız üzerinde çalışmaz. Bu nedenle, müşteriler zaman zaman bu eski SKU 'Lar için ayırma hatalarıyla karşılaşabilir. Bu sorundan kaçınmak için, eski serisi sanal makineleri kullanan müşterilerin aşağıdaki önerilere göre daha yeni VM 'lere geçmeyi düşünmesini öneririz: Bu VM 'Ler en son donanımlar için iyileştirilmiştir ve daha iyi fiyatlandırma ve performanstan yararlanmanızı sağlar. 

|Eski VM-seri/boyut|Önerilen daha yeni VM-Serisi/boyutu|Daha fazla bilgi|
|----------------------|----------------------------|--------------------|
|AV1 serisi|[Av2 Serisi](../av2-series.md)|https://azure.microsoft.com/blog/new-av2-series-vm-sizes/
|Dv1 veya DSv1 serisi (D1 to D5)|[Dv3 veya DSv3 serisi](../dv3-dsv3-series.md)|https://azure.microsoft.com/blog/introducing-the-new-dv3-and-ev3-vm-sizes/
|Dv1 veya DSv1 serisi (D11 to D14)|[Ev3 veya ESv3 serisi](../ev3-esv3-series.md)|
|D15v2 veya DS15v2|Daha büyük VM boyutlarının avantajlarından yararlanmak için Teresource Manager dağıtım modelini kullanıyorsanız, D16v3/DS16v3 veya D32v3/DS32v3 ' a geçmeyi düşünün. Bunlar, en son nesil donanımda çalışacak şekilde tasarlanmıştır. VM örneğinizin tek bir müşteriye ayrılmış donanıma yalıtılmış olduğundan emin olmak için Kaynak Yöneticisi dağıtım modelini kullanıyorsanız, en son nesil donanımda çalışacak şekilde tasarlanan, E64i_v3 veya E64is_v3 yeni yalıtılmış VM boyutlarına geçmeyi göz önünde bulundurun. |https://azure.microsoft.com/blog/new-isolated-vm-sizes-now-available/

## <a name="allocation-failures-for-large-deployments-more-than-500-cores"></a>Daha büyük dağıtımlar (500 çekirdekten fazla) için ayırma hataları

İstenen VM boyutu örneklerinin sayısını azaltın ve dağıtım işlemini yeniden deneyin. Ayrıca, daha büyük dağıtımlar için [Azure sanal makine ölçek kümelerini](https://docs.microsoft.com/azure/virtual-machine-scale-sets/)değerlendirmek isteyebilirsiniz. Sanal makine örneklerinin sayısı isteğe bağlı olarak veya tanımlı bir zamanlamaya göre otomatik olarak artabilir veya azalabilir ve dağıtımlar birden fazla kümeye yayılabilecek olduğundan, başarılı bir ayırma şansınız olur. 

## <a name="background-information"></a>Arka plan bilgileri
### <a name="how-allocation-works"></a>Ayırma nasıl kullanılır?
Azure veri merkezlerindeki sunucular kümelere bölünmüştür. Normalde, birden fazla kümede bir ayırma isteğinde bulunulur, ancak ayırma isteğindeki belirli kısıtlamalar, Azure platformunu yalnızca bir kümede istekte bulunmaya zorlar. Bu makalede, "bir kümeye sabitlenmiş" olarak başvuracağız. Aşağıdaki şekil 1 ' de, birden çok kümede denenen normal ayırmanın durumu gösterilmektedir. Diyagram 2 ' de, var olan bulut hizmeti CS_1 veya kullanılabilirlik kümesinin barındırıldığı yerde, küme 2 ' ye sabitlenmiş bir ayırmanın durumu gösterilmektedir.
![Ayırma diyagramı](./media/virtual-machines-common-allocation-failure/Allocation1.png)

### <a name="why-allocation-failures-happen"></a>Ayırma hatalarının nedeni
Bir ayırma isteği bir kümeye sabitlendiğinde, kullanılabilir kaynak havuzu daha küçük olduğundan, ücretsiz kaynakları bulmasının daha büyük bir olasılığı vardır. Ayrıca, ayırma isteğiniz bir kümeye sabitlenir ancak istediğiniz kaynak türü bu küme tarafından desteklenmiyorsa, kümede boş kaynaklar olsa bile isteğiniz başarısız olur. Aşağıdaki diyagram 3, tek aday kümenin ücretsiz kaynakları olmadığından, sabitlenmiş bir ayırmanın başarısız olduğu durumu gösterir. Diyagram 4 ' te, küme boş kaynaklara sahip olsa bile, tek aday kümesi istenen VM boyutunu desteklemediğinden, sabitlenmiş bir ayırmanın başarısız olduğu durum gösterilmektedir.

![Sabitlenmiş ayırma hatası](./media/virtual-machines-common-allocation-failure/Allocation2.png)


