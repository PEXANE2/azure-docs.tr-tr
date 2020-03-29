---
title: SAP uygulamaları için Azure yakınlık yerleşim grupları | Microsoft Dokümanlar
description: Azure yakınlık yerleşim gruplarıyla SAP dağıtım senaryolarını açıklar
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: msjuergent
manager: bburns
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 01/17/2020
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 01ce1599f86082aef3ff53d298cc53896074af66
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76277590"
---
# <a name="azure-proximity-placement-groups-for-optimal-network-latency-with-sap-applications"></a>SAP uygulamalarıyla en iyi ağ gecikmesi için azure yakınlık yerleşim grupları
SAP NetWeaver veya SAP S/4HANA mimarisine dayalı SAP uygulamaları, SAP uygulama katmanı ile SAP veritabanı katmanı arasındaki ağ gecikmesine duyarlıdır. Bu duyarlılık, uygulama katmanında çalışan iş mantığının çoğunun sonucudur. SAP uygulama katmanı iş mantığını çalıştırdığından, sorguları veritabanı katmanına saniyede binlerce veya on binlerce hızla yüksek bir frekansta verir. Çoğu durumda, bu sorguların doğası basittir. Genellikle veritabanı katmanında 500 mikrosaniye veya daha kısa sürede çalıştırılabilir.

Ağda, uygulama katmanından veritabanı katmanına böyle bir sorgu göndermek ve sonucu geri almak için harcanan süre, iş süreçlerini çalıştırmak için gereken süre üzerinde büyük bir etkiye sahiptir. Ağ gecikmesine karşı bu duyarlılık, SAP dağıtım projelerinde en iyi ağ gecikmesini elde etmeniz innedenidir. Bkz. [SAP Note #1100926 - SSS:](https://launchpad.support.sap.com/#/notes/1100926/E) Ağ gecikmesinin nasıl sınıflandırılalalalalabildiğini anlatan yönergeler için ağ performansı.

Birçok Azure bölgesinde veri merkezi sayısı arttı. Bu büyüme, Kullanılabilirlik Bölgeleri'nin devreye girmesiyle de tetiklenmiştir. Aynı zamanda, müşteriler, özellikle high-end SAP sistemleri için, M-Serisi ailesinde daha özel VM SUS veya HANA Büyük Örnekleri kullanıyor. Bu Azure sanal makine türleri, belirli bir Azure bölgesindeki tüm veri merkezlerinde kullanılamaz. Bu iki eğilim nedeniyle, müşteriler en uygun aralıkta olmayan ağ gecikmesi yaşadılar. Bazı durumlarda, bu gecikme gecikmesi SAP sistemlerinin en düşük performansıyla sonuçlanır.

Bu sorunları önlemek için Azure [yakınlık yerleşim grupları](https://docs.microsoft.com/azure/virtual-machines/linux/co-location)sunar. Bu yeni işlevsellik zaten çeşitli SAP sistemleri dağıtmak için kullanılmıştır. Yakınlık yerleştirme gruplarına ilişkin kısıtlamalar için, bu paragrafın başında atıfta bulunulan makaleye bakın. Bu makalede, Azure yakınlık yerleşim gruplarının kullanabileceği veya kullanılması gereken SAP senaryoları ele alınmektedir.

## <a name="what-are-proximity-placement-groups"></a>Yakınlık yerleştirme grupları nelerdir? 
Azure yakınlık yerleşim grubu mantıksal bir yapıdır. Biri tanımlandığında, bir Azure bölgesine ve bir Azure kaynak grubuna bağlıdır. VM'ler dağıtıldığında, bir yakınlık yerleşim grubuna başvurulur:

- Veri merkezinde dağıtılan ilk Azure VM'si. İlk sanal makineyi, belirli bir Kullanılabilirlik Bölgesi için kullanıcı tanımlarıyla birleştirilen Azure ayırma algoritmalarına dayalı bir veri merkezinde dağıtılan bir "kapsam VM"i olarak düşünebilirsiniz.
- Sonraki tüm Sanal M'ler, daha sonra dağıtılan tüm Azure VM'lerini ilk sanal makineyle aynı veri merkezine yerleştirmek için yakınlık yerleşim grubuna başvuruyu dağıttı.

> [!NOTE]
> İlk VM'nin yerleştirildiği veri merkezinde belirli bir VM türünü çalıştırabilecek ana bilgisayar donanımı dağıtılmış değilse, istenen VM türündeki dağıtım başarılı olmaz. Bir başarısızlık mesajı alırsınız.

Tek bir [Azure kaynak grubunda](https://docs.microsoft.com/azure/azure-resource-manager/manage-resources-portal) birden çok yakınlık yerleşim grubu atanabilir. Ancak bir yakınlık yerleşim grubu yalnızca bir Azure kaynak grubuna atanabilir.

Yakınlık yerleşimi gruplarını kullandığınızda, şu hususları göz önünde bulundurun:

- SAP sisteminiz için en iyi performansı hedeflediğinizde ve yakınlık yerleşim işlemi gruplarını kullanarak sistem için tek bir Azure veri merkeziyle sınırladığınızda, yıl gruptaki her tüm VM ailesini biraraya getiremeyemeyebilirsiniz. Bu sınırlamalar, belirli bir VM türünü çalıştırmak için gereken ana bilgisayar donanımının yerleşim grubunun "kapsamlı VM"sinin dağıtıldığı veri merkezinde bulunmayabileceğinden oluşur.
- Böyle bir SAP sisteminin yaşam döngüsü sırasında, sistemi başka bir veri merkezine taşımak zorunda kalabileceğiniz. Ölçeklendirme HANA DBMS katmanınızın, örneğin, dört düğümden 16 düğüme geçmesi gerektiğine ve veri merkezinde kullandığınız türden ek 12 VM almak için yeterli kapasite olmadığına karar verirseniz, bu hareket gerekebilir.
- Donanımın devre dışı bırakılması nedeniyle Microsoft, başlangıçta kullandığınız veri merkezi yerine farklı bir veri merkezinde kullandığınız VM türü için kapasiteler oluşturabilir. Bu senaryoda, tüm yakınlık yerleşim grubunun VM'lerini başka bir veri merkezine taşımanız gerekebilir.

## <a name="proximity-placement-groups-with-sap-systems-that-use-only-azure-vms"></a>Yalnızca Azure VM'leri kullanan SAP sistemlerine sahip yakınlık yerleşim grupları
Azure'daki çoğu SAP NetWeaver ve S/4HANA sistem dağıtımları [HANA Büyük Örnekleri](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture)kullanmaz. HANA Büyük Örnekleri kullanmayan dağıtımlar için SAP uygulama katmanı ile DBMS katmanı arasında en iyi performansı sağlamak önemlidir. Bunu yapmak için, sadece sistem için bir Azure yakınlık yerleşim grubu tanımlayın.

Çoğu müşteri dağıtımında, müşteriler SAP sistemleri için tek bir [Azure kaynak grubu](https://docs.microsoft.com/azure/azure-resource-manager/manage-resources-portal) oluşturur. Bu durumda, örneğin üretim ERP sistem kaynak grubu ile yakınlık yerleşim grubu arasında bire bir ilişki vardır. Diğer durumlarda, müşteriler kaynak gruplarını yatay olarak düzenler ve tüm üretim sistemlerini tek bir kaynak grubunda toplar. Bu durumda, üretim SAP sistemleri için kaynak grubunuz ile üretim SAP ERP, SAP BW ve benzeri üretim için birkaç yakınlık yerleşim grupları arasında bir-çok ilişkisi olurdu.

Tek bir yakınlık yerleşim grubunda birkaç SAP üretim veya üretim dışı sistemi birleştirmekten kaçının. Az sayıda SAP sistemi veya SAP sistemi ve çevredeki bazı uygulamaların düşük gecikme süresi ağ iletişimine sahip olması gerektiğinde, bu sistemleri tek bir yakınlık yerleşim grubuna taşımayı düşünebilirsiniz. Bir yakınlık yerleşim grubunda ne kadar çok sistem gruplasanız, o kadar çok sistem demetlerinden kaçınmalısınız:

- Yakınlık yerleşim grubunun kapsama girdiği belirli veri merkezinde çalıştırılabilen bir VM türüne ihtiyacınız olduğunu.
- M-Serisi VM'ler gibi genel olmayan VM'lerin kaynakları, zaman içinde bir yakınlık yerleşim grubuna yazılım eklediğiniz için daha fazlasına ihtiyacınız olduğunda nihayetinde yerine getirilmemiş olabilir.

Burada ideal yapılandırma, açıklandığı gibi, nasıl göründüğü:

![Yalnızca Azure VM'leri olan yakınlık yerleşim grupları](./media/sap-proximity-placement-scenarios/ppg-for-all-azure-vms.png)

Bu durumda, tek SAP sistemleri her biri bir yakın yerleşim grubu yla birer kaynak grubunda gruplanır. HANA ölçeklendirme veya DBMS ölçeklendirme yapılandırmaları kullanıp kullanmadığınıza bağlılık yoktur.

## <a name="proximity-placement-groups-and-hana-large-instances"></a>Yakınlık yerleştirme grupları ve HANA Büyük Örnekleri
SAP sistemlerinizden bazıları uygulama katmanı için [HANA Büyük Örnekleri'ne](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) güveniyorsa, [Revizyon 4 satırlarında veya damgalarında](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-network-architecture#networking-architecture-for-hana-large-instance)dağıtılan HANA Büyük Örnekler birimlerini kullanırken HANA Büyük Örnekler birimi ile Azure VM'leri arasında ağ gecikmesinde önemli iyileştirmeler karşılaşabilirsiniz. Bir gelişme, HANA Büyük Örnekler birimlerinin, dağıtılırken, bir yakınlık yerleşim grubuyla konuşlandırmalarıdır. Bu yakınlık yerleşim grubunu uygulama katmanıvm'larınızı dağıtmak için kullanabilirsiniz. Sonuç olarak, bu Sanal Bilgisayarlar HANA Büyük Örnekler biriminizi barındıran aynı veri merkezinde dağıtılır.

HANA Büyük Örnekler biriminizin Bir Düzeltme 4 damgasında mı yoksa satırda mı dağıtılmadığını belirlemek için, [Azure HANA Büyük Örnekler denetimine Azure portalı üzerinden](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-li-portal#look-at-attributes-of-single-hli-unit)makaleyi denetleyin. HANA Büyük Örnekler biriminizin özniteliklerine genel bakışta, HANA Büyük Örnekler biriminiz dağıtıldığında oluşturulduğu için yakınlık yerleşim grubunun adını da belirleyebilirsiniz. Özniteliklere genel bakışta görünen ad, uygulama katmanı VM'lerinizi dağıtmanız gereken yakınlık yerleşim grubunun adıdır.

Yalnızca Azure sanal makinelerini kullanan SAP sistemleriyle karşılaştırıldığında, HANA Büyük Örnekleri'ni kullandığınızda, kaç [Azure kaynak grubu](https://docs.microsoft.com/azure/azure-resource-manager/manage-resources-portal) kullanacağınızkonusunda daha az esnekliğe sahip olursunuz. Bir [HANA Büyük Örnekler kiracıtüm HANA Büyük](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-know-terms) Örnekler [birimleri, bu makalede](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-li-portal#display-of-hana-large-instance-units-in-the-azure-portal)açıklandığı gibi, tek bir kaynak grubunda gruplanır. Örneğin, üretim ve üretim dışı sistemleri veya diğer sistemleri ayırmak için farklı kiracılara dağıtmadığınız sürece, tüm HANA Büyük Örnekler birimleriniz tek bir HANA Büyük Örnekler kiracısında dağıtılır. Bu kiracının bir kaynak grubuyla bire bir ilişkisi vardır. Ancak, tek birimlerin her biri için ayrı bir yakınlık yerleştirme grubu tanımlanır.

Sonuç olarak, Azure kaynak grupları ve tek bir kiracı için yakınlık yerleşim grupları arasındaki ilişkiler burada gösterildiği gibi olacaktır:

![Yakınlık yerleştirme grupları ve HANA Büyük Örnekleri](./media/sap-proximity-placement-scenarios/ppg-for-hana-large-instance-units.png)

## <a name="example-of-deployment-with-proximity-placement-groups"></a>Yakınlık yerleşim grupları ile dağıtım örneği
VM'lerinizi Azure yakınlık yerleşim gruplarıyla dağıtmak için kullanabileceğiniz bazı PowerShell komutları aşağıda verebilirsiniz.

[Azure Bulut Su Şur'da](https://azure.microsoft.com/features/cloud-shell/)oturum açtıktan sonra ilk adım, dağıtım için kullanmak istediğiniz Azure aboneliğinde olup olmadığınızı kontrol etmektir:

<pre><code>
Get-AzureRmContext
</code></pre>

Farklı bir aboneliğe geçiş yapmanız gerekiyorsa, bunu şu komutu çalıştırarak yapabilirsiniz:

<pre><code>
Set-AzureRmContext -Subscription "my PPG test subscription"
</code></pre>

Bu komutu çalıştırarak yeni bir Azure kaynak grubu oluşturun:

<pre><code>
New-AzResourceGroup -Name "myfirstppgexercise" -Location "westus2"
</code></pre>

Bu komutu çalıştırarak yeni yakınlık yerleşim ibaresini oluşturun:

<pre><code>
New-AzProximityPlacementGroup -ResourceGroupName "myfirstppgexercise" -Name "letsgetclose" -Location "westus2"
</code></pre>

Bunun gibi bir komut kullanarak ilk VM'inizi yakınlık yerleşim grubuna dağıtın:

<pre><code>
New-AzVm -ResourceGroupName "myfirstppgexercise" -Name "myppganchorvm" -Location "westus2" -OpenPorts 80,3389 -ProximityPlacementGroup "letsgetclose" -Size "Standard_DS11_v2"
</code></pre>

Önceki komut, Windows tabanlı bir VM dağıtır. Bu VM dağıtımı başarılı olduktan sonra, yakınlık yerleşim grubunun veri merkezi kapsamı Azure bölgesinde tanımlanır. Önceki komutta gösterildiği gibi yakınlık yerleşim grubuna başvuran sonraki tüm VM dağıtımları, VM türü bu veri merkezine yerleştirilen donanımda barındırılabildiği ve bu VM türü için kapasiteye sahip olduğu sürece aynı Azure veri merkezinde dağıtılacaktır. Kullanılabilir.

## <a name="combine-availability-sets-and-availability-zones-with-proximity-placement-groups"></a>Kullanılabilirlik kümelerini ve Kullanılabilirlik Bölgelerini yakınlık yerleşim gruplarıyla birleştirin
SAP sistem dağıtımları için Kullanılabilirlik Bölgeleri'ni kullanmanın dezavantajlarından biri, belirli bölge içindeki kullanılabilirlik kümelerini kullanarak SAP uygulama katmanını dağıtamamanızdır. SAP uygulama katmanının DBMS katmanıyla aynı bölgelerde dağıtılmasını istiyorsunuz. Tek bir VM dağıtılırken kullanılabilirlik bölgesine ve kullanılabilirlik kümesine başvurmak desteklenmez. Bu nedenle, daha önce, bir bölgeye başvurarak uygulama katmanınızı dağıtmak zorunda kaldınız. Uygulama katmanı VM'lerinin farklı güncelleştirme ve hata etki adlarına yayıldıklarından emin olma yeteneğini kaybettiniz.

Yakınlık yerleşim işlemi gruplarını kullanarak bu kısıtlamı atlayabilirsiniz. Dağıtım sırası aşağıda veda edin:

- Yakınlık yerleşim grubu oluşturun.
- Bir Kullanılabilirlik Bölgesi'ne başvurarak çapa VM'inizi (genellikle DBMS sunucusu) dağıtın.
- Azure yakınlık grubuna başvuran bir kullanılabilirlik kümesi oluşturun. (Bu makalenin ilerleyen saatlerinde ki komuta bakın.)
- Kullanılabilirlik kümesine ve yakınlık yerleşim grubuna başvurarak uygulama katmanı VM'lerini dağıtın.

Önceki bölümde gösterildiği gibi ilk VM'yi dağıtmak yerine, VM'yi dağıttığınızda bir Kullanılabilirlik Bölgesi'ne ve yakınlık yerleşim grubuna başvurursunuz:

<pre><code>
New-AzVm -ResourceGroupName "myfirstppgexercise" -Name "myppganchorvm" -Location "westus2" -OpenPorts 80,3389 -Zone "1" -ProximityPlacementGroup "letsgetclose" -Size "Standard_E16_v3"
</code></pre>

Bu sanal makinenin başarılı bir şekilde dağıtılması, SAP sisteminin veritabanı örneğini tek bir Kullanılabilirlik Bölgesinde barındırır. Yakınlık yerleşim grubunun kapsamı, tanımladığınız Kullanılabilirlik Bölgesini temsil eden veri merkezlerinden birine sabitlenir.

Merkezi Hizmetler VM'lerini, aynı bölge veya bölgelere ve aynı yakınlık yerleşim gruplarına atıfta bulunarak DBMS VM'lerle aynı şekilde dağıtdığınızı varsayalım. Bir sonraki adımda, SAP sisteminizin uygulama katmanı için kullanmak istediğiniz kullanılabilirlik kümelerini oluşturmanız gerekir.

Yakınlık yerleşim grubunu tanımlamanız ve oluşturmanız gerekir. Kullanılabilirlik kümesini oluşturma komutu, yakınlık yerleşim grubu kimliğine (ad için değil) ek bir başvuru gerektirir. Bu komutu kullanarak yakınlık yerleşim grubunun kimliğini alabilirsiniz:

<pre><code>
Get-AzProximityPlacementGroup -ResourceGroupName "myfirstppgexercise" -Name "letsgetclose"
</code></pre>

Kullanılabilirlik kümesini oluştururken, yönetilen diskleri (aksi belirtilmedikçe varsayılan) ve yakınlık yerleşim ibarelerini kullanırken ek parametreleri göz önünde bulundurmanız gerekir:

<pre><code>
New-AzAvailabilitySet -ResourceGroupName "myfirstppgexercise" -Name "myppgavset" -Location "westus2" -ProximityPlacementGroupId "/subscriptions/my very long ppg id string" -sku "aligned" -PlatformUpdateDomainCount 3 -PlatformFaultDomainCount 2 
</code></pre>

İdeal olarak, üç hata etki alanı kullanmanız gerekir. Ancak desteklenen hata etki alanlarının sayısı bölgeden bölgeye değişebilir. Bu durumda, belirli bölgeler için mümkün olan en fazla hata etki alanı sayısı ikidir. Uygulama katmanıVM'lerinizi dağıtmak için, kullanılabilirlik kümesi adınıza ve yakınlık yerleşimi grup adına burada gösterildiği gibi bir başvuru eklemeniz gerekir:

<pre><code>
New-AzVm -ResourceGroupName "myfirstppgexercise" -Name "myppgavsetappvm" -Location "westus2" -OpenPorts 80,3389 -AvailabilitySetName "myppgavset" -ProximityPlacementGroup "letsgetclose" -Size "Standard_DS11_v2"
</code></pre>

Bu dağıtımın sonucu:
- Sap sisteminiz için belirli bir Kullanılabilirlik Bölgesi veya Kullanılabilirlik Bölgesinde bulunan bir DBMS katmanı ve Merkezi Hizmetler.
- DBMS VM veya VM'lerle aynı Azure veri merkezlerinde kullanılabilirlik kümeleri aracılığıyla bulunan bir SAP uygulama katmanı.

> [!NOTE]
> Yüksek kullanılabilirlik yapılandırması oluşturmak için bir DBMS VM'yi bir bölgeye ve ikinci DBMS VM'yi başka bir bölgeye dağıttığınızdan, her bölge için farklı bir yakınlık yerleşim grubuna ihtiyacınız olur. Aynı durum, kullandığınız tüm kullanılabilirlik kümesi için de geçerlidir.

## <a name="move-an-existing-system-into-proximity-placement-groups"></a>Varolan bir sistemi yakınlık yerleşim gruplarına taşıma
SAP sistemleri zaten dağıtıldıysa, bazı kritik sistemlerinizin ağ gecikmesini optimize etmek ve uygulama katmanını ve DBMS katmanını aynı veri merkezinde bulmak isteyebilirsiniz. Tam bir Azure kullanılabilirlik kümesinin VM'lerini zaten kapsamda bulunan varolan bir yakınlık yerleşim grubuna taşımak için, kullanılabilirlik kümesinin tüm VM'lerini kapatmanız ve kullanılabilirlik kümesini Azure portalı aracılığıyla varolan yakınlık yerleşim grubuna atamanız gerekir, PowerShell veya CLI. Kullanılabilirlik kümesinin parçası olmayan bir VM'yi varolan bir yakınlık yerleşim grubuna taşımak istiyorsanız, VM'yi kapatmanız ve varolan bir yakınlık yerleşim grubuna atamanız gerekir. 


## <a name="next-steps"></a>Sonraki adımlar
Belgelere göz atın:

- [Azure'da SAP iş yükleri: planlama ve dağıtım denetim listesi](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-deployment-checklist)
- [Önizleme: Azure CLI'yi kullanarak VM'leri yakınlık yerleşim gruplarına dağıtma](https://docs.microsoft.com/azure/virtual-machines/linux/proximity-placement-groups)
- [Önizleme: PowerShell'i kullanarak vm'leri yakınlık yerleşim gruplarına dağıtın](https://docs.microsoft.com/azure/virtual-machines/windows/proximity-placement-groups)
- [SAP iş yükleri için Azure Sanal Makineler DBMS dağıtımında dikkat edilmesi gerekenler](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_general)

