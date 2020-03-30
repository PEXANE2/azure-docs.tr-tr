---
title: Azure VM ayırma hatalarını giderme | Microsoft Dokümanlar
description: Azure'da bir VM oluştururken, yeniden başlattığınızda veya yeniden boyutlandırdığınızda gider oluşturma hataları
services: virtual-machines
documentationcenter: ''
author: JiangChen79
manager: felixwu
editor: ''
tags: top-support-issue,azure-resource-manager,azure-service-management
ms.assetid: 1ef41144-6dd6-4a56-b180-9d8b3d05eae7
ms.service: virtual-machines
ms.topic: troubleshooting
ms.date: 04/13/2018
ms.author: cjiang
ms.openlocfilehash: b4750ad9fdfa214aa4d7b6a0355c319e7eb1d9c3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77484409"
---
# <a name="troubleshoot-allocation-failures-when-you-create-restart-or-resize-vms-in-azure"></a>Azure’da VM’leri oluştururken, yeniden başlatırken ve yeniden boyutlandırırken ortaya çıkan ayırma hatalarında sorun giderme

Sanal bir makine (VM) oluşturduğunuzda, durdurulan (ayrılan) VM'leri yeniden başlattığınızda veya bir VM'yi yeniden boyutlandırdığınızda, Microsoft Azure bilgi işlem kaynaklarını aboneliğinize ayırır. Müşteri talebini desteklemek için her zaman tüm VM türlerine sahip olduğumuzdan emin olmak için sürekli olarak ek altyapı ve özelliklere yatırım yapıyoruz. Ancak, belirli bölgelerdeki Azure hizmetlerine yönelik talepte eşi görülmemiş bir artış nedeniyle zaman zaman kaynak ayırma hataları yla karşılaşabilirsiniz. Bu sorun, VM'ler aşağıdaki hata kodunu ve iletiyi görüntülerken bir bölgede VM'ler oluşturmaya veya başlatmaya çalıştığınızda oluşabilir:

**Hata kodu**: AllocationFailed veya ZonalAllocationFailed

**Hata iletisi**: "Ayırma başarısız oldu. Bu bölgede istenen VM boyutu için yeterli kapasiteye sahip değiliz. https:/aka.ms/allocation-guidance"\/de tahsis başarı olasılığını artırma hakkında daha fazla bilgi edinin

Bu makalede, bazı ortak ayırma hatalarının nedenleri açıklanmaktadır ve olası çözümler önerir.

Azure sorununuzun bu makalede ele alınmaması durumunda, [MSDN ve Yığın Taşma'daki Azure forumlarını ziyaret edin.](https://azure.microsoft.com/support/forums/) Sorununuzu bu forumlarda veya Twitter'da @AzureSupport yayınlayabilirsiniz. Ayrıca, [Azure destek](https://azure.microsoft.com/support/options/) sitesinde destek al'ı seçerek Bir Azure destek isteği nde bulunabilirsiniz.

Tercih ettiğiniz bölgede tercih edilen VM türü kullanıma sunulana kadar, dağıtım sorunlarıyla karşılaşan müşterilere aşağıdaki tablodaki kılavuzu geçici bir geçici çözüm olarak düşünmelerini tavsiye ederiz. 

Servis talebinizle en iyi eşleşen senaryoyu belirleyin ve ardından ayırma başarısı olasılığını artırmak için ilgili önerilen geçici çözümünü kullanarak ayırma isteğini yeniden deneyin. Alternatif olarak, her zaman daha sonra yeniden deneyebilirsiniz. Bunun nedeni, isteğinizi karşılamak için kümede, bölgede veya bölgede yeterince kaynağın serbest bırakılmış olmasıdır. 


## <a name="resize-a-vm-or-add-vms-to-an-existing-availability-set"></a>Bir veya birden çok VM'yi mevcut kullanılabilirlik kümesine yeniden boyutlandırma

### <a name="cause"></a>Nedeni

VM'yi yeniden boyutlandırma veya varolan kullanılabilirlik kümesine VM ekleme isteği, varolan kullanılabilirlik kümesini barındıran özgün kümede denenmelidir. İstenen VM boyutu küme tarafından desteklenir, ancak küme şu anda yeterli kapasiteye sahip olmayabilir. 

### <a name="workaround"></a>Geçici çözüm

VM farklı bir kullanılabilirlik kümesinin parçası olabilirse, farklı bir kullanılabilirlik kümesinde (aynı bölgede) bir VM oluşturun. Bu yeni VM daha sonra aynı sanal ağa eklenebilir.

Aynı kullanılabilirlik kümesindeki tüm VM'leri durdurun (anlaşma lı olun) ve ardından her birini yeniden başlatın.
Durdurmak için: Kaynak grupları > [kaynak grubunuz] > Kaynakları > [kullanılabilirlik kümenizi] > sanal makineler > [sanal makineniz] > Durdur'a tıklayın.
Tüm VM'ler durduktan sonra ilk VM'yi seçin ve ardından Başlat'ı tıklatın.
Bu adım, yeni bir ayırma denemesinin çalıştırılmasını ve yeterli kapasiteye sahip yeni bir kümenin seçilebilmesini sağlar.

## <a name="restart-partially-stopped-deallocated-vms"></a>Kısmen durdurulmuş (serbest bırakılmış) VM'leri yeniden başlatma

### <a name="cause"></a>Nedeni

Kısmi ayırma, bir kullanılabilirlik kümesindeki VM'leri bir veya daha fazla, ancak tümü yle değil, durdurmuş (ayrılmış) anlamına gelir. Bir VM'yi buldiğinizde, ilişkili kaynaklar serbest bırakılır. Kısmen ayrılmış kullanılabilirlik kümesinde VM'leri yeniden başlatmak, varolan kullanılabilirlik kümesine VM eklemekle aynıdır. Bu nedenle, ayırma isteği, yeterli kapasiteye sahip olmayan varolan kullanılabilirlik kümesini barındıran özgün kümede denenmelidir.

### <a name="workaround"></a>Geçici çözüm

Aynı kullanılabilirlik kümesindeki tüm VM'leri durdurun (anlaşma lı olun) ve ardından her birini yeniden başlatın.
Durdurmak için: Kaynak grupları > [kaynak grubunuz] > Kaynakları > [kullanılabilirlik kümenizi] > sanal makineler > [sanal makineniz] > Durdur'a tıklayın.
Tüm VM'ler durduktan sonra ilk VM'yi seçin ve ardından Başlat'ı tıklatın.
Bu, yeni bir ayırma denemesinin çalıştırılmasını ve yeterli kapasiteye sahip yeni bir kümenin seçilebilmesini sağlar.

## <a name="restart-fully-stopped-deallocated-vms"></a>Tamamen durdurulmuş (serbest bırakılmış) VM'leri yeniden başlatma

### <a name="cause"></a>Nedeni

Tam ayırma, tüm VM'leri bir kullanılabilirlik kümesinde durdurduğunuz (ayırdığınız) anlamına gelir. Bu VM'leri yeniden başlatmak için ayırma isteği, bölge veya bölge içinde istenen boyutu destekleyen tüm kümeleri hedefler. Bu makaledeki önerilere göre ayırma isteğinizi değiştirin ve ayırma başarısı olasılığını artırmak için isteği yeniden deneyin. 

### <a name="workaround"></a>Geçici çözüm

Dv1, DSv1, Av1, D15v2 veya DS15v2 gibi eski VM serileri veya boyutları kullanıyorsanız, daha yeni sürümlere geçmeyi düşünün. Belirli VM boyutları için bu önerilere bakın.
Farklı bir VM boyutu kullanma seçeneğiniz yoksa, aynı coğrafi kaynı niçin farklı bir bölgeye dağıtmayı deneyin. Her bölgedeki mevcut VM boyutları hakkında daha fazla bilgi içinhttps://aka.ms/azure-regions

Kullanılabilirlik bölgelerini kullanıyorsanız, bölge içinde istenen VM boyutu için kullanılabilir kapasiteye sahip olabilecek başka bir bölge deneyin.

Ayırma isteğiniz büyükse (500'den fazla çekirdek), isteği daha küçük dağıtımlara ayırmak için aşağıdaki bölümlerdeki kılavuza bakın.

[VM'yi yeniden dağıtmayı](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/redeploy-to-new-node-windows)deneyin. VM'nin yeniden dağıtılması VM'yi bölge içindeki yeni bir kümeye ayırır.

## <a name="allocation-failures-for-older-vm-sizes-av1-dv1-dsv1-d15v2-ds15v2-etc"></a>Daha eski VM boyutları (Av1, Dv1, DSv1, D15v2, DS15v2, vb.) için ayırma hataları

Azure altyapısını genişletirken, en son sanal makine türlerini desteklemek üzere tasarlanmış yeni nesil donanımları dağıtıyoruz. Eski seri VM'lerden bazıları en son nesil altyapımızda çalışmaz. Bu nedenle, müşteriler zaman zaman bu eski SUS'lar için ayırma hataları yaşayabilirler. Bu sorunu önlemek için, eski seri sanal makineleri kullanan müşterilerimizi aşağıdaki önerilere göre eşdeğer yeni VM'lere taşınmayı düşünmeye teşvik ediyoruz: Bu SANAL'lar en son donanım için optimize edilebilmektedir ve daha iyi avantajlardan yararlanmanızı sağlar fiyatlandırma ve performans. 

|Eski VM serisi/boyutu|Önerilen yeni VM serisi/boyutu|Daha fazla bilgi|
|----------------------|----------------------------|--------------------|
|Av1 serisi|[Av2 Serisi](../av2-series.md)|https://azure.microsoft.com/blog/new-av2-series-vm-sizes/
|Dv1 veya DSv1 serisi (D1-D5)|[Dv3 veya DSv3 serisi](../dv3-dsv3-series.md)|https://azure.microsoft.com/blog/introducing-the-new-dv3-and-ev3-vm-sizes/
|Dv1 veya DSv1 serisi (D11-D14)|[Ev3 veya ESv3 serisi](../ev3-esv3-series.md)|
|D15v2 veya DS15v2|Daha büyük VM boyutlarından yararlanmak için Kaynak Yöneticisi dağıtım modelini kullanıyorsanız, D16v3/DS16v3 veya D32v3/DS32v3'e taşınmayı düşünün. Bunlar en son nesil donanımüzerinde çalışacak şekilde tasarlanmıştır. VM örneğinizin tek bir müşteriye adanmış donanıma yalıtılmış olduğundan emin olmak için Kaynak Yöneticisi dağıtım modelini kullanıyorsanız, en son nesil donanımda çalışacak şekilde tasarlanmış yeni yalıtılmış VM boyutlarına, E64i_v3 veya E64is_v3 taşımayı düşünün. |https://azure.microsoft.com/blog/new-isolated-vm-sizes-now-available/

## <a name="allocation-failures-for-large-deployments-more-than-500-cores"></a>Daha büyük dağıtımlar (500 çekirdekten fazla) için ayırma hataları

İstenen VM boyutunun örnek sayısını azaltın ve ardından dağıtım işlemini yeniden deneyin. Ayrıca, daha büyük dağıtımlar için [Azure sanal makine ölçek kümelerini](https://docs.microsoft.com/azure/virtual-machine-scale-sets/)değerlendirmek isteyebilirsiniz. VM örneklerinin sayısı, isteğe veya tanımlı bir zamanlamaya yanıt olarak otomatik olarak artabilir veya azalabilir ve dağıtımlar birden çok kümeye yayılabilir diye ayırma başarısı şansınız artar. 

## <a name="background-information"></a>Arka plan bilgileri
### <a name="how-allocation-works"></a>Ayırma nasıl çalışır?
Azure veri merkezlerindeki sunucular kümelere bölünmüştür. Normalde, birden fazla kümede bir ayırma isteğinde bulunulur, ancak ayırma isteğindeki belirli kısıtlamalar, Azure platformunu yalnızca bir kümede istekte bulunmaya zorlar. Bu makalede, buna "kümeye sabitlenmiş" olarak atıfta bulunacağız. Aşağıdaki diyagram 1, birden çok kümede denenmiş normal bir ayırma durumunu göstermektedir. Diyagram 2, varolan Bulut Hizmeti CS_1 veya kullanılabilirlik kümesinin barındırıldığı yer olduğu için Cluster 2'ye sabitlenmiş bir ayırma durumunu gösterir.
![Tahsisat Diyagramı](./media/virtual-machines-common-allocation-failure/Allocation1.png)

### <a name="why-allocation-failures-happen"></a>Ayırma hataları neden olur?
Ayırma isteği bir kümeye sabitlendiğinde, kullanılabilir kaynak havuzu daha küçük olduğundan boş kaynak bulamama olasılığı daha yüksektir. Ayrıca, ayırma isteğiniz bir kümeye sabitlenmişse, ancak istediğiniz kaynak türü bu küme tarafından desteklenmiyorsa, kümenin boş kaynakları olsa bile isteğiniz başarısız olur. Aşağıdaki Diyagram 3, tek aday kümenin boş kaynakları olmadığından sabitlenmiş bir ayırmanın başarısız olduğu durumu göstermektedir. Diyagram 4, kümenin boş kaynakları olmasına rağmen tek aday küme istenen VM boyutunu desteklemediği için sabitlenmiş bir ayırmanın başarısız olduğu durumu gösterir.

![Sabitlenmiş Ayırma Hatası](./media/virtual-machines-common-allocation-failure/Allocation2.png)


