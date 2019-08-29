---
title: SAP uygulamalarıyla en iyi ağ gecikmesi için Azure yakınlık yerleştirme grupları | Microsoft Docs
description: Azure yakınlık yerleştirme gruplarıyla SAP dağıtım senaryolarını açıklar
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
ms.date: 07/15/2019
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 02dcb7174dd9cb2926ef2fafda4b521b939ae68a
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70077979"
---
# <a name="azure-proximity-placement-groups-for-optimal-network-latency-with-sap-applications"></a>SAP uygulamalarıyla en iyi ağ gecikme süresi için Azure Yakınlık Yerleştirme Grupları
SAP NetWeaver veya SAP S/4HANA mimarisine dayanan SAP uygulamaları, SAP uygulama katmanı ve SAP veritabanı katmanı arasındaki ağ gecikmesinden duyarlıdır. Bu mimarilerin bu hassasiyetinin nedeni, uygulama katmanında çoğu iş mantığının yürütüldüğü olgusuna yöneliktir. İş mantığını yürütmenin bir sonucu olarak SAP uygulama katmanı, bir saniyede binlerce ve on binlerce üzerinde yüksek frekansta veritabanı katmanına sorgular. Çoğu durumda, bu sorguların doğası basittir. Genellikle, veritabanı katmanında 500 mikrosaniye 'den az veya daha az bir şekilde yürütülebilir. Uygulama katmanından veritabanı katmanına bu tür bir sorgu göndermek için ağ üzerinde harcanan süre, iş süreçlerini yürütmek için gereken sürenin büyük bir etkisi vardır. Ağ gecikmesi ile olan bu duyarlılık, en iyi ağ gecikmesini sağlamak için SAP dağıtım projelerinde zamanın nerede harcanması gerektiğine neden olur. [SAP Note #1100926-SSS: Ağ performansı](https://launchpad.support.sap.com/#/notes/1100926/E), SAP, ağ gecikmesini sınıflandırmasına ilişkin bazı yönergeler yayımladı.

Bir tarafta, birçok Azure bölgesinde, Kullanılabilirlik Alanları tanıtımı tarafından tetiklenen veri merkezlerinin sayısı grew olarak da tetiklenir. Diğer taraftan, özellikle yüksek kaliteli SAP sistemlerinde, M serisi ailesinden veya HANA büyük örneklerden daha özel VM SKU 'Ları kullanılmaktadır. Belirli bir Azure bölgesindeki tüm veri merkezlerinde bulunmayan Azure sanal makine türleri. Bu iki temel durumda, müşteriler, ağ gecikmesi en uygun aralıkta olmadığı ve bazı durumlarda SAP sistemlerinin en iyi performans performansını elde etmeyle sonuçlanmasıyla karşılaşacaktır.

Bu tür bir sorunu engellemek için Azure, [Azure yakınlık yerleşimi grubu](https://docs.microsoft.com/azure/virtual-machines/linux/co-location)adlı bir yapı sunar. Bu yeni işlev, zaten çeşitli farklı SAP sistemlerini dağıtmak için kullanılmıştır. Yakınlık yerleşimi gruplarının kısıtlamaları için başvurulan makaleye bakın. Bu makalede, Azure yakınlık yerleşimi gruplarının kullanılabileceği veya kullanılması gereken farklı SAP senaryoları ele alınacaktır.

## <a name="what-are-proximity-placement-groups"></a>Yakınlık yerleşimi grupları nedir? 
Bir Azure yakınlık yerleşimi grubu, bir Azure bölgesine ve bir Azure Kaynak grubuna bağlanan, bu, tanım aşamasında olan mantıksal bir yapıdır. VM 'lerin dağıtımı sırasında, bir yakınlık yerleşimi grubuna şunun tarafından başvuruluyor:

- Veri merkezinde kapatılacak ilk dağıtılan Azure VM. İlk sanal makine, Azure ayırma algoritmalarına göre son olarak belirli bir Azure kullanılabilirlik bölgesi için Kullanıcı tanımlarıyla birleştirilmiş bir veri merkezinde dağıtılan bir bağlayıcı VM olarak görülebilir.
- Tüm izleyen VM 'Ler, sonraki dağıtılan tüm Azure VM 'Leri, ilk sanal makine ile aynı veri merkezine yerleştirmek için yakınlık yerleşimi grubuna başvurulmaya dağıtılır.

> [!NOTE]
> İlk sanal makine ile aynı veri merkezinde belirli bir sanal makine türünü çalıştıran bir konak donanımı dağıtılırsa, talep edilen VM türünün dağıtımı başarılı olmayacaktır ve bir hata iletisiyle sona ermez. Bunlar, GPU veya HPC VM türlerindeki sanal makineler gibi çok sayıda temel olmayan VM 'Lerin, örneğin ilk VM türü olarak dağıtılan bir M serisi VM olduğu durumlar olabilir

Tek bir [Azure Kaynak grubu](https://docs.microsoft.com/azure/azure-resource-manager/manage-resources-portal) , kendisine atanmış birden fazla yakınlık yerleştirme grubuna sahip olabilir. Ancak, bir yakınlık yerleşimi grubu yalnızca bir Azure Kaynak grubuna atanabilir.

Yakınlık yerleşimi gruplarını kullanarak şunu bilmelisiniz:

- SAP sisteminizde en iyi performansı elde etmeniz ve bu sisteme ait tek bir Azure veri merkezinde yakınlık yerleşimi gruplarını kullanarak sınırlamanız durumunda, bu tür bir yakınlık yerleştirme grubundaki tüm VM aileleri türlerini birleştiremeyebilirsiniz. Bunun nedeni, belirli bir sanal makine türünü özel olarak çalıştırmak için gereken belirli ana bilgisayar donanımının, yerleştirme grubunun ' bağlayıcı VM ' ' de dağıtılan veri merkezinde mevcut olmaması olabilir
- Bu tür bir SAP sisteminin yaşam döngüsünde, sistemi başka bir veri merkezine taşımaya zorlanabilirsiniz. Bu tür bir taşıma, ölçek genişletme HANA DBMS katmanınızı örneğin dört düğümden 16 düğüme taşıma gibi bir şekilde yapmaya karar verdiğiniz durumlarda zorlanacak. Ancak aynı veri merkezinde zaten kullandığınız türde ek 12 VM almak için yeterli kapasite yok.
- Microsoft, donanım kullanımdan kaldırılması nedeniyle, aynı veri merkezi yerine başka bir veri merkezinde kullandığınız sanal makine türleri için kapasite oluşturabilir. Böyle bir örnek, yakınlık yerleşimi grubunun tüm VM 'lerini başka bir veri merkezine taşıyabilmeniz anlamına gelebilir.


## <a name="azure-proximity-placement-groups-with-sap-systems-using-azure-vms-exclusively"></a>Azure yakınlığı yerleştirme grupları yalnızca Azure VM 'Leri kullanan SAP sistemleri
Azure 'da SAP NetWeaver ve S/4HANA sistem dağıtımlarının büyük bölümü, [Hana büyük örnekleri](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture)kullanmaz. Bu tür sistemlerin dağıtımları için SAP uygulama katmanı ve DBMS katmanı arasında en iyi performansı sağlamak önemlidir. Bunu yapmak için, yalnızca bu sistem için bir Azure yakınlık yerleşimi grubu tanımlarsınız. Müşteri dağıtımlarının çoğunda, müşteriler SAP sistemleri için tek bir [Azure Kaynak grubu](https://docs.microsoft.com/azure/azure-resource-manager/manage-resources-portal) oluşturmayı tercih ediyor. Böyle bir durumda, örneğin, üretim ERP sistem kaynak grupları ve yakınlık yerleşimi grubu gibi bir 1:1 ilişki olacaktır. Bazı diğer dağıtım durumlarında, müşteriler kaynak gruplarını yatay olarak organize ediyor ve tüm üretim sistemlerini tek bir kaynak grubunda toplamıştır. Böyle bir durumda, üretim SAP sistemleri için kaynak grubunuz ve üretim SAP ERP, SAP BW vb. gibi çeşitli yakınlık yerleştirme grupları arasında 1 ila fazla ilişkiye sahip olursunuz. Tek bir yakınlık yerleşimi grubundaki birkaç veya hatta tüm SAP üretimini veya üretim dışı sistemleri paketlemek kaçınılmalıdır. Düşük gecikmeli ağ iletişimine sahip olmak için az sayıda SAP sistemi veya SAP sistemi ve çevreleyen bazı uygulamalar gerektiğinde, bu sistemleri tek bir yakınlık yerleşimi grubuna taşımayı düşünebilirsiniz. Bunun gibi önerilerin nedeni, yakınlık yerleşimi grubunda daha fazla sistem gruplandırıyorsanız, ne kadar yüksek olursa:

- Bu, yakınlık yerleşimi grubu ile bağlantılı olan belirli bir veri merkezinde çalıştıramıyoruz.
- M serisi gibi temel olmayan belirli VM 'Lerin kaynakları, zaman içinde var olan bir yakınlık yerleşimi grubuna ek yazılım eklerken daha fazla bilgi sorduğunuz anda artık karşılanmaz.

Aşağıda gösterildiği gibi ideal kullanım şöyle görünür:

![Tüm Azure VM 'Leri için yakınlık yerleşimi grupları](./media/sap-proximity-placement-scenarios/ppg-for-all-azure-vms.png)

Bu durumda, tek SAP sistemleri her biri bir yakınlık yerleşimi grubu olan bir kaynak grubunda gruplandırılır. HANA genişleme veya DBMS genişleme yapılandırması kullanmanıza bakılmaksızın bir bağımlılık yoktur.


## <a name="azure-proximity-placement-groups-and-hana-large-instances"></a>Azure yakınlık yerleştirme grupları ve HANA büyük örnekleri
SAP sistemlerinizden bazılarının, uygulama katmanı olarak [Hana büyük örneklerine](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) sahip olduğu durumlarda, Hana büyük örnek birimi Ile Azure VM 'ler arasındaki ağ gecikmesi açısından ciddi geliştirmeler elde edilebilir. [Düzeltme 4 satırları veya damgaları](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-network-architecture#networking-architecture-for-hana-large-instance)' nda dağıtılır. Geliştirmelerden biri, dağıtıldığı gibi HANA büyük örnek birimlerinin, zaten dağıtılan bir yakınlık yerleşimi grubunu edinmeleridir. Uygulama katmanı sanal makinelerinizi dağıtmak için bu yakınlık yerleşimi grubunu kullanabilirsiniz. Sonuç olarak, bu VM 'Ler HANA büyük örnek biriminizi barındıran veri merkezinde dağıtılır.

HANA büyük örnek biriminizdeki bir düzeltme 4 damgasında veya satırında dağıtılıp dağıtılmadığını algılamak için [Azure Portal aracılığıyla Azure Hana büyük örnekler denetimi](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-li-portal#look-at-attributes-of-single-hli-unit)makalesini kontrol edin. HANA büyük örnek biriminizle ilgili özniteliklere genel bakış ' da, aynı zamanda, HANA büyük örnek biriminiz için dağıtım zamanında oluşturulan yakınlık yerleşimi grubunun adını da öğrenebilirsiniz. Özniteliklerde genel bakışta görünen ada, yakınlık yerleşimi grubunun adı, uygulama katmanı VM 'lerinizi ' ye dağıtmak için kullanmanız gerekir.

Yalnızca Azure sanal makinelerini kullanan SAP sistemlerinin tersine, her kaç [Azure Kaynak grubunun](https://docs.microsoft.com/azure/azure-resource-manager/manage-resources-portal) kullanılması gerektiğini KARARLAMA, Hana büyük örnekleri kullanırken size bir dereceye kadar götürülürsünüz. Bir [Hana büyük örnek kiracının](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-know-terms) tüm Hana büyük örnek birimleri [burada](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-li-portal#display-of-hana-large-instance-units-in-the-azure-portal)açıklandığı gibi tek bir Azure Kaynak grubunda gruplandırılır. Örneğin, üretim ve üretim dışı veya belirli sistemler gibi ayrı kiracılara bir dağıtım yapmadığınız takdirde, tüm HANA büyük örnek birimleriniz, bir Azure ile 1:1 ilişkisi olan tek bir HANA büyük örnek kiracısında dağıtılır. kaynak grubu. Öte yandan, tüm tek birimlerde tanımlı ayrı bir yakınlık yerleşimi grubu olacaktır. 

Sonuç olarak, tek bir kiracının Azure Kaynak grupları ve yakınlık yerleşimi grupları arasındaki gruplandırma şöyle görünür:

![Tüm Azure sanal makineleri için yakınlık yerleştirme grupları](./media/sap-proximity-placement-scenarios/ppg-for-hana-large-instance-units.png)


## <a name="short-example-of-deploying-with-azure-proximity-placement-groups"></a>Azure yakınlık yerleşimi gruplarıyla dağıtmaya yönelik kısa örnek
Göstermek için Azure yakınlık yerleşimi gruplarını VM 'nizi dağıtmak üzere nasıl kullanabileceğinizi öğrenmek için Azure yakınlık yerleşimi grupları ile ilk küçük bir alıştırmada kullanımı gösteren PowerShell komutlarının bir listesini burada bulabilirsiniz.

[Azure Cloud Shell](https://azure.microsoft.com/features/cloud-shell/) ile oturum açıldıktan sonra ilk adım, komutu ile dağıtmak için kullanmak Istediğiniz doğru Azure aboneliğinde olup olmadığınızı denetmaktır:

<pre><code>
Get-AzureRmContext
</code></pre>

Farklı bir aboneliğe değiştirmeniz gerekiyorsa, bu komutu yürüterek bunu yapabilirsiniz:

<pre><code>
Set-AzureRmContext -Subscription "my PPG test subscription"
</code></pre>

Üçüncü bir adım olarak, bu komutla yeni bir Azure Kaynak grubu oluşturmak istersiniz:

<pre><code>
New-AzResourceGroup -Name "myfirstppgexercise" -Location "westus2"
</code></pre>

Yeni yakınlık yerleşimi grubunu şu şekilde oluşturabilirsiniz:

<pre><code>
New-AzProximityPlacementGroup -ResourceGroupName "myfirstppgexercise" -Name "letsgetclose" -Location "westus2"
</code></pre>

Şimdi, aşağıdaki gibi bir komutla ilk sanal makineyi bu yakınlık yerleşimi grubuna dağıtmaya başlayabilirsiniz:

<pre><code>
New-AzVm -ResourceGroupName "myfirstppgexercise" -Name "myppganchorvm" -Location "westus2" -OpenPorts 80,3389 -ProximityPlacementGroup "letsgetclose" -Size "Standard_DS11_v2"
</code></pre>

Yukarıdaki komutla Windows tabanlı bir sanal makine dağıtılır. Bu VM dağıtımı başarılı olduktan sonra, yakınlık yerleşimi grubunun veri merkezi kapsamı Azure bölgesi içinde tanımlanır. Son komutta olduğu gibi yakınlık yerleşimi grubuna başvuran sonraki tüm VM dağıtımları, VM türü bu veri merkezinde yer alan donanımda barındırılabilecek ve/veya bu sanal makine türü için kapasite kullanılabilir olduğu sürece aynı Azure veri merkezinde dağıtılır.

## <a name="combine-availability-sets-and-availability-zones-with-proximity-placement-groups"></a>Kullanılabilirlik kümelerini ve Kullanılabilirlik Alanları yakınlık yerleştirme gruplarıyla birleştirme 
SAP sistem dağıtımları için Kullanılabilirlik Alanları kullanarak, dezavantajlardan biri, SAP uygulama katmanının belirli bir bölgedeki kullanılabilirlik kümeleri kullanılarak denetlenemeyeceği gerçedir. SAP uygulama katmanının, DBMS katmanıyla aynı bölgelerde dağıtılmasını ve tek bir VM dağıtımında bir kullanılabilirlik bölgesine ve kullanılabilirlik kümesine başvurmasının desteklenmediği için, bir bölgeye başvurarak uygulama katmanınızı dağıtmaya zorlanırsınız Böylece, uygulama katmanı VM 'lerinin farklı güncelleştirme ve hata etki alanlarına yayılmasını sağlama özelliği de kaybedilir. Yakınlık yerleştirme gruplarıyla ilgili yardım ile bu kısıtlamayı aşalabilirsiniz. Dağıtım sırası şöyle görünür:

- Yakınlık yerleştirme grubu oluşturma
- Belirli bir Azure kullanılabilirlik alanına başvurarak, genellikle DBMS sunucusundan ' bağlayıcı VM ' 'nizi dağıtın
- Azure yakınlık grubuna başvuran bir kullanılabilirlik kümesi oluşturun (aşağıya bakın)
- Kullanılabilirlik kümesine ve yakınlık yerleşimi grubuna başvurarak uygulama katmanı VM 'lerini dağıtın

Yukarıdaki ilk VM 'yi yukarıda gösterildiği gibi dağıtmak yerine, VM 'yi dağıtmakta olduğu gibi bir Azure kullanılabilirlik bölgesine ve yakınlık yerleşimi grubuna başvurmış olursunuz:

<pre><code>
New-AzVm -ResourceGroupName "myfirstppgexercise" -Name "myppganchorvm" -Location "westus2" -OpenPorts 80,3389 -Zone "1" -ProximityPlacementGroup "letsgetclose" -Size "Standard_E16_v3"
</code></pre>

Bu sanal makinenin, SAP sistemimin veritabanı örneğini tek bir Azure kullanılabilirlik bölgesinde barındıracak başarılı bir şekilde dağıtımı, yakınlık yerleştirme grubunun kapsamı tanımladığınız kullanılabilirlik bölgesini temsil eden veri merkezlerinden birine düzeltildi .

Yönetim Hizmetleri VM 'lerini, DBMS VM 'leri ve aynı yakınlık yerleşimi grupları için aynı bölgeye (ler) başvurarak DBMs VM 'leriyle aynı şekilde dağıttığınızı varsayalım. Bir sonraki adımda, SAP sisteminizin uygulama katmanı için kullanmak istediğiniz kullanılabilirlik kümesi (ler) i oluşturmanız gerekir.
Yakınlık yerleşimi grubunun tanımlanması ve oluşturulması gerekir. Kullanılabilirlik kümesi oluşturma komutu, yakınlık yerleşimi grup KIMLIĞINE (adı değil) ek bir başvuru gerektirir. Yakınlık yerleşimi grubunun kimliğini şu kimlikle edinebilirsiniz:



<pre><code>
Get-AzProximityPlacementGroup -ResourceGroupName "myfirstppgexercise" -Name "letsgetclose"
</code></pre>

Kullanılabilirlik kümesini oluştururken, yönetilen diskler (farklı belirtilmedikçe varsayılan) ve yakınlık yerleşimi grupları kullanırken ek parametreleri göz önünde bulundurmanız gerekir:

<pre><code>
New-AzAvailabilitySet -ResourceGroupName "myfirstppgexercise" -Name "myppgavset" -Location "westus2" -ProximityPlacementGroupId "/subscriptions/my very long ppg id string" -sku "aligned" -PlatformUpdateDomainCount 3 -PlatformFaultDomainCount 2 
</code></pre>

İdeal olarak, üç hata etki alanı kullanmanız gerekir. Ancak, desteklenen hata etki alanlarının sayısı bölgeden bölgeye farklılık gösterebilir. Bu durumda, belirli bölgeler için mümkün olan en fazla hata etki alanı sayısı ikiydi. Uygulama katmanı sanal makinelerinizi dağıtmak için, burada gösterildiği gibi kullanılabilirlik kümesi adı ve yakınlık yerleşimi Grup adına bir başvuru eklemeniz gerekir:

<pre><code>
New-AzVm -ResourceGroupName "myfirstppgexercise" -Name "myppgavsetappvm" -Location "westus2" -OpenPorts 80,3389 -AvailabilitySetName "myppgavset" -ProximityPlacementGroup "letsgetclose" -Size "Standard_DS11_v2"
</code></pre>

Bu dizinin nihai sonucu, belirli bir kullanılabilirlik bölgesinde bulunan SAP sisteminizin bir DBMS katmanı ve bir SAP uygulama katmanı, DBMS VM 'leri ile aynı Azure veri merkezlerinde kullanılabilirlik kümeleri aracılığıyla bulunan bir SAP uygulama katmanında yer alacak. dağıtılan.

> [!NOTE]
> Yüksek kullanılabilirlik yapılandırması oluşturmak için bir DBMS sanal makinesini tek bir bölgeye ve ikinci DBMS sanal makinesine başka bir bölgeye dağıtırken, her bir bölgenin farklı yakınlık yerleşimi grupları gerekir. Kullanabileceğiniz kullanılabilirlik kümesi için aynı sı doğru

## <a name="get-an-existing-system-into-azure-proximity-placement-groups"></a>Azure yakınlık yerleşimi gruplarına mevcut bir sistemi alın
SAP sistemleri zaten dağıtılmış olduğundan, bazı kritik sistemlerinizden oluşan ağ gecikmesini iyileştirmek ve aynı veri merkezinde uygulama katmanını ve DBMS katmanını bulmak isteyebilirsiniz. Yakınlık yerleşimi grubu işlevinin genel önizleme aşamasında, sanal makinelerin silinmesi ve bu tür bir sanal makineyi yeni bir şekilde oluşturulması, bu tür bir yakının yakınlık yerleştirme gruplarına taşınmasını zorunlu hale getirmek için gereklidir. Bu işlevin bu aşamasında, bu kapatma sanal makinelerini yakınlık yerleşimi gruplarına atayabilmek için VM 'Lerin kapatılması yeterince iyi değildir.


## <a name="next-steps"></a>Sonraki Adımlar
Belgelere başvurun:

- [Azure planlama ve dağıtım denetim listesi üzerinde SAP iş yükü](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-deployment-checklist)
- [Önizleme: Azure CLı kullanarak yakınlık yerleşimi gruplarına VM 'Leri dağıtma](https://docs.microsoft.com/azure/virtual-machines/linux/proximity-placement-groups)
- [Önizleme: PowerShell kullanarak VM 'Leri yakınlık yerleşimi gruplarına dağıtma](https://docs.microsoft.com/azure/virtual-machines/windows/proximity-placement-groups)
- [SAP iş yükü için Azure sanal makineler DBMS dağıtımına yönelik konular](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_general)

