---
title: Azure'da sanal makinelerden SAP HANA'ya bağlantı kurulumu ( Büyük Örnekler) | Microsoft Dokümanlar
description: Azure'da SAP HANA'yı kullanmak için sanal makinelerden bağlantı kurulumu (Büyük Örnekler).
services: virtual-machines-linux
documentationcenter: ''
author: msjuergent
manager: patfilot
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/25/2019
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: fb6f88fbfcbd539603e435b11661c428d54f3c34
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74224722"
---
# <a name="connecting-azure-vms-to-hana-large-instances"></a>Azure VM'lerini HANA Büyük Örnekleri'ne bağlama

Makale [Azure'da SAP HANA nedir (Büyük Örnekler)?](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) Azure'daki SAP uygulama katmanıyla HANA Büyük Örnekleri'nin en az dağıtımının aşağıdaki gibi göründüğünden bahseder:

![Azure'da SAP HANA'ya bağlı Azure VNet (Büyük Örnekler) ve şirket içi](./media/hana-overview-architecture/image1-architecture.png)

Azure sanal ağ tarafına daha yakından bakıldığında, aşağıdakilere ihtiyaç vardır:

- SAP uygulama katmanının VM'lerini dağıtacağınız azure sanal ağının tanımı.
- Azure sanal ağında varsayılan bir alt ağ tanımı, gerçekte VM'lerin dağıtıldığı ağdır.
- Oluşturulan Azure sanal ağının en az bir VM alt ağına ve bir Azure ExpressRoute sanal ağ ağ geçidi alt ağına sahip olması gerekir. Bu alt ağlara, aşağıdaki bölümlerde belirtildiği ve tartışıldığı gibi IP adresi aralıkları atanmalıdır.


## <a name="create-the-azure-virtual-network-for-hana-large-instances"></a>HANA Büyük Örnekleri için Azure sanal ağı oluşturun

>[!Note]
>HANA Büyük Örnekleri için Azure sanal ağı, Azure Kaynak Yöneticisi dağıtım modeli kullanılarak oluşturulmalıdır. Genellikle klasik dağıtım modeli olarak bilinen eski Azure dağıtım modeli HANA Büyük Örnek çözümü tarafından desteklenmez.

Sanal ağı oluşturmak için Azure portalını, PowerShell'i, Azure şablonu'nu veya Azure CLI'yi kullanabilirsiniz. (Daha fazla bilgi için bkz: [Azure portalını kullanarak sanal ağ oluştur).](../../../virtual-network/manage-virtual-network.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#create-a-virtual-network) Aşağıdaki örnekte, Azure portalını kullanarak oluşturulan sanal bir ağa bakıyoruz.

Bu belgelerdeki **adres alanına** atıfta bulunulurken, Azure sanal ağının kullanmasına izin verilen adres alanına. Bu adres alanı aynı zamanda sanal ağın BGP rota yayılması için kullandığı adres aralığıdır. Bu **adres alanı** burada görülebilir:

![Azure portalında görüntülenen bir Azure sanal ağın adres alanı](./media/hana-overview-connectivity/image1-azure-vnet-address-space.png)

Önceki örnekte, 10.16.0.0/16 ile Azure sanal ağına kullanımı oldukça büyük ve geniş bir IP adresi aralığı verilmiştir. Bu nedenle, bu sanal ağ içinde sonraki alt ağların tüm IP adres aralıkları bu adres alanı içinde kendi aralıkları olabilir. Azure'da genellikle tek bir sanal ağ için bu kadar geniş bir adres aralığı önermiyoruz. Ancak Azure sanal ağında tanımlanan alt ağlara bakalım:

![Azure sanal ağ alt ağları ve IP adres aralıkları](./media/hana-overview-connectivity/image2b-vnet-subnets.png)

İlk VM alt ağına (burada "varsayılan" olarak adlandırılır) ve "GatewaySubnet" adlı bir alt ağa bakıyoruz.

Önceki iki grafikte, **sanal ağ adresi alanı** hem Azure **VM'nin alt net IP adres aralığını** hem de sanal ağ ağ ağ geçidini kapsar.

Sanal ağ **adresi alanını** her alt ağ tarafından kullanılan belirli aralıklarla kısıtlayabilirsiniz. Ayrıca, sanal **bir** ağın sanal ağ adres alanını burada gösterildiği gibi birden çok özel aralık olarak da tanımlayabilirsiniz:

![İki boş alana sahip Azure sanal ağ adres alanı](./media/hana-overview-connectivity/image3-azure-vnet-address-space_alternate.png)

Bu durumda, **sanal ağ adresi alanı** tanımlanan iki boşluk vardır. Bunlar, Azure VM'nin alt ağ IP adresi aralığı ve sanal ağ ağ ağ geçidi için tanımlanan IP adres aralıkları ile aynıdır. 

Bu kiracı alt ağları (VM alt ağları) için istediğiniz herhangi bir adlandırma standardına kullanabilirsiniz. Ancak, Azure (Büyük Örnekler) ExpressRoute devresi üzerinde SAP HANA'ya bağlanan **her sanal ağ için her zaman bir ve yalnızca bir ağ geçidi alt ağı olmalıdır.** ExpressRoute ağ geçidinin düzgün yerleştirildiğinden emin olmak için **bu ağ geçidi alt ağının "GatewaySubnet" olarak adlandırılması gerekir.**

> [!WARNING] 
> Ağ geçidi alt ağının her zaman "GatewaySubnet" olarak adlandırılması çok önemlidir.

Birden çok VM alt ağı ve bitişik olmayan adres aralıkları kullanabilirsiniz. Bu adres aralıkları, sanal **ağın sanal ağ adres alanı** tarafından karşılanmalıdır. Toplu halde olabilirler. Ayrıca, VM alt ağlarının ve ağ geçidi alt ağlarının tam aralıkları listesinde de bulunabilirler.

HANA Büyük Örnekleri'ne bağlanan bir Azure sanal ağı yla ilgili önemli gerçeklerin bir özeti aşağıda veda edilebilgilidir:

- HANA Büyük Örnekleri'nin ilk dağıtımını gerçekleştirirken **sanal ağ adresi alanını** Microsoft'a göndermeniz gerekir. 
- **Sanal ağ adresi alanı,** Azure VM'nin alt net IP adres aralığı ve sanal ağ ağ ağ geçidinin aralıklarını kapsayan daha geniş bir aralık olabilir.
- Veya VM subnet IP adresi aralığı(lar) ve sanal ağ ağ ağ geçidi IP adresi aralığının farklı IP adres aralıklarını kapsayan birden çok aralık gönderebilirsiniz.
- Tanımlanan **sanal ağ adresi alanı** BGP yönlendirme yayılımı için kullanılır.
- Ağ geçidi alt ağının adı şöyle olmalıdır: **"GatewaySubnet"**.
- Adres alanı, HANA Büyük Örnek birimlerinin Azure'dan trafiğe izin vermek veya izin vermemek için HANA Büyük Örnek tarafında bir filtre olarak kullanılır. Azure sanal ağının BGP yönlendirme bilgileri ve HANA Büyük Örnek tarafında filtreleme için yapılandırılan IP adresi aralıkları eşleşmelidir. Aksi takdirde, bağlantı sorunları oluşabilir.
- Sanal ağı **HANA Large Instance ExpressRoute'a bağlama** bölümünde, daha sonra tartışılan ağ geçidi alt ağı hakkında bazı ayrıntılar vardır.



## <a name="different-ip-address-ranges-to-be-defined"></a>Tanımlanacak farklı IP adresi aralıkları 

HANA Büyük Örnekleri dağıtmak için gerekli olan IP adres aralıklarından bazıları zaten tanıtıldı. Ama aynı zamanda önemli olan daha fazla IP adresi aralıkları vardır. Aşağıdaki IP adresi aralıklarının tamamının Microsoft'a gönderilmesi gerekmez. Ancak, ilk dağıtım için bir istek göndermeden önce bunları tanımlamanız gerekir:

- **Sanal ağ adresi alanı**: **Sanal ağ adresi alanı,** Azure sanal ağlarında adres alanı parametrenize atadığınız IP adres aralıklarıdır. Bu ağlar SAP HANA Büyük Örnek ortamına bağlanır. Bu adres alanı parametrenin çok satırlı bir değer olduğunu öneririz. Azure VM'nin alt net aralığından ve Azure ağ geçidinin alt ağ aralığından(lar) oluşmalıdır. Bu alt ağ aralığı önceki grafiklerde gösterilmiştir. Şirket içi veya sunucu IP havuzunuz veya ER-P2P adres aralıklarınız ile çakışmamalıdır. Bu IP adresi aralığı(lar) nasıl alabilirim? Kurumsal ağ ekibiniz veya hizmet sağlayıcınız, ağınızda kullanılmayan bir veya birden çok IP adresi aralığı(lar) sağlamalıdır. Örneğin, Azure VM'nizin alt ağı 10.0.1.0/24 ve Azure ağ geçidi alt ağınızın alt ağı 10.0.2.0/28'dir.  Azure sanal ağ adres alanınızın şu şekilde tanımlanmasını öneririz: 10.0.1.0/24 ve 10.0.2.0/28. Adres alanı değerleri toplanabilir, ancak bunları alt ağ aralıkları ile eşleştirmenizi öneririz. Bu şekilde, ağınızdaki daha büyük adres boşluklarında kullanılmayan IP adresi aralıklarını yanlışlıkla yeniden kullanmaktan kaçınabilirsiniz. **Sanal ağ adres alanı bir IP adres aralığıdır. İlk dağıtım istediğinizde Microsoft'a gönderilmesi gerekir.**
- **Azure VM subnet IP adres aralığı:** Bu IP adresi aralığı, Azure sanal ağ alt ağ parametresine atadığınız aralıktır. Bu parametre Azure sanal ağınızda dır ve SAP HANA Büyük Örnek ortamına bağlanır. Bu IP adresi aralığı, Azure VM'lerinize IP adresleri atamak için kullanılır. Bu aralığın dışında ip adresleri SAP HANA Büyük Örnek sunucu(lar) bağlanmak için izin verilir. Gerekirse, birden çok Azure VM alt ağı kullanabilirsiniz. Her Azure VM alt ağı için bir /24 CIDR bloğu öneririz. Bu adres aralığı, Azure sanal ağ adres alanında kullanılan değerlerin bir parçası olmalıdır. Bu IP adresi aralığını nasıl alabilirim? Kurumsal ağ ekibiniz veya hizmet sağlayıcınız, ağınızın içinde kullanılmayan bir IP adres aralığı sağlamalıdır.
- **Sanal ağ ağ geçidi alt ağ IP adresi aralığı:** Kullanmayı planladığınız özelliklere bağlı olarak önerilen boyut:
   - Ultra performanslı ExpressRoute ağ geçidi: /26 adres bloğu-- SUS'ların Tip II sınıfı için gereklidir.
   - YÜKSEK performanslı ExpressRoute sanal ağ ağ ağ geçidi (veya daha küçük): /27 adres bloğu kullanarak VPN ve ExpressRoute ile birlikte yaşama.
   - Diğer tüm durumlar: /28 adres bloğu. Bu adres aralığı ,"VNet adres alanı" değerlerinde kullanılan değerlerin bir parçası olmalıdır. Bu adres aralığı, Microsoft'a gönderdiğiniz Azure sanal ağ adresi alanı değerlerinde kullanılan değerlerin bir parçası olmalıdır. Bu IP adresi aralığını nasıl alabilirim? Kurumsal ağ ekibiniz veya hizmet sağlayıcınız, ağınızda şu anda kullanılmayan bir IP adres aralığı sağlamalıdır. 
- **ER-P2P bağlantısı için adres aralığı:** Bu aralık, SAP HANA Büyük Örnek ExpressRoute (ER) P2P bağlantınızın IP aralığıdır. Bu IP adresi aralığı bir /29 CIDR IP adres aralığı olmalıdır. Bu aralık, şirket içi veya diğer Azure IP adres aralıklarınızla çakışmamalıdır. Bu IP adresi aralığı, ExpressRoute sanal ağ geçidinizden SAP HANA Büyük Örnek sunucularına ER bağlantısını ayarlamak için kullanılır. Bu IP adresi aralığını nasıl alabilirim? Kurumsal ağ ekibiniz veya hizmet sağlayıcınız, ağınızda şu anda kullanılmayan bir IP adres aralığı sağlamalıdır. **Bu aralık bir IP adres aralığıdır. İlk dağıtım istediğinizde Microsoft'a gönderilmesi gerekir.**  
- **Sunucu IP havuzu adres aralığı:** Bu IP adresi aralığı, tek tek IP adresini HANA büyük örnek sunucularına atamak için kullanılır. Önerilen subnet boyutu bir /24 CIDR bloğudur. Gerekirse, en az 64 IP adresi ile daha küçük olabilir. Bu aralıktan, ilk 30 IP adresi Microsoft tarafından kullanılmak üzere ayrılmıştır. Aralığın boyutunu seçerken bu gerçeği hesaba kattığından emin olun. Bu aralık, şirket içi veya diğer Azure IP adreslerinizle çakışmamalıdır. Bu IP adresi aralığını nasıl alabilirim? Kurumsal ağ ekibiniz veya hizmet sağlayıcınız, ağınızda şu anda kullanılmayan bir IP adres aralığı sağlamalıdır.  **Bu aralık, ilk dağıtım isterken Microsoft'a gönderilmesi gereken bir IP adres aralığıdır.**

Sonunda Microsoft'a gönderilmesi gereken isteğe bağlı IP adresi aralıkları:

- Şirket içi HANA Büyük Örnek birimlerine doğrudan yönlendirmeyi etkinleştirmek için [ExpressRoute Global Reach'i](https://docs.microsoft.com/azure/expressroute/expressroute-global-reach) kullanmayı seçerseniz, başka bir /29 IP adresi aralığı ayırmanız gerekir. Bu aralık, daha önce tanımladığınız diğer IP adresi aralıklarıyla çakışmayabilir.
- Bir Azure bölgesindeki HANA Büyük Örnek kiracısından başka bir Azure bölgesindeki başka bir HANA Büyük Örnek kiracısına doğrudan yönlendirmeyi etkinleştirmek için [ExpressRoute Global Reach'i](https://docs.microsoft.com/azure/expressroute/expressroute-global-reach) kullanmayı seçerseniz, başka bir /29 IP adresi aralığı ayırmanız gerekir. Bu aralık, daha önce tanımladığınız diğer IP adresi aralıklarıyla çakışmayabilir.

ExpressRoute Global Reach ve HANA'nın büyük örnekleri etrafında kullanım hakkında daha fazla bilgi için belgeleri kontrol edin:

- [SAP HANA (Büyük Örnekler) ağ mimarisi](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-network-architecture)
- [Sanal ağı HANA'nın büyük örneklerine bağlayın](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-connect-vnet-express-route)
 
Daha önce açıklanan IP adresi aralıklarını tanımlamanız ve planlamanız gerekir. Ancak, bunların tümünün Microsoft'a iletilmesi gerekmez. Microsoft'a ad vermeniz gereken IP adresi aralıkları şunlardır:

- Azure sanal ağ adresi alanı(lar)
- ER-P2P bağlantısı için adres aralığı
- Sunucu IP havuzu adres aralığı

HANA Büyük Örnekleri'ne bağlanması gereken ek sanal ağlar eklerseniz, Microsoft'a eklediğiniz yeni Azure sanal ağ adresi alanını göndermeniz gerekir. 

Aşağıda, yapılandırmanız ve sonunda Microsoft'a sağlamanız gerektiğinden farklı aralıklara ve bazı örnek aralıklara bir örnek verilmiştir. Azure sanal ağ adresi alanının değeri ilk örnekte toplanmış değildir. Ancak, ilk Azure VM alt ağ IP adresi aralığı ve sanal ağ ağ geçidi alt ağ IP adresi aralığı aralıklarından tanımlanır. 

Azure sanal ağ adres alanının bir parçası olarak ek VM alt ağının ek IP adres aralıklarını yapılandırıp gönderdiğiniz de Azure sanal ağında birden çok VM alt ağı kullanabilirsiniz.

![Azure'da SAP HANA'da (Büyük Örnekler) minimum dağıtım için gereken IP adresi aralıkları](./media/hana-overview-connectivity/image4b-ip-addres-ranges-necessary.png)

Grafik, ExpressRoute Global Reach'in isteğe bağlı kullanımı için gerekli olan ek IP adresi aralığını(lar) göstermez.

Microsoft'a gönderdiğiniz verileri de toklayabilirsiniz. Bu durumda, Azure sanal ağının adres alanı yalnızca bir alan içerir. Ip adresi aralıklarını önceki örnekten kullanarak, toplanan sanal ağ adresi alanı aşağıdaki görüntü gibi görünebilir:

![Azure'da SAP HANA'da (Büyük Örnekler) minimum dağıtım için gerekli IP adres aralıklarının ikinci olasılığı](./media/hana-overview-connectivity/image5b-ip-addres-ranges-necessary-one-value.png)

Örnekte, Azure sanal ağının adres alanını tanımlayan iki küçük aralık yerine, 4096 IP adresini kapsayan daha büyük bir aralığımız vardır. Adres alanı nın bu kadar büyük bir tanımı, bazı büyük aralıkları kullanılmaz bırakır. Sanal ağ adresi alanı değeri(ler) BGP rota yayılımı için kullanıldığından, kullanılmayan aralıkların şirket içinde veya anızın başka bir yerinde kullanılması yönlendirme sorunlarına neden olabilir. Grafik, ExpressRoute Global Reach'in isteğe bağlı kullanımı için gerekli olan ek IP adresi aralığını(lar) göstermez.

Adres alanını kullandığınız gerçek alt ağ adres alanıyla sıkıca hizalamanızı öneririz. Gerekirse, sanal ağda kapalı kalma süresi ne olursa, her zaman daha sonra yeni adres alanı değerleri ekleyebilirsiniz.
 
> [!IMPORTANT] 
> ER-P2P, sunucu IP havuzu ve Azure sanal ağ adresi alanındaki her IP adresi aralığı, birbiriyle veya asanızda kullanılan başka bir aralıkla **çakışmamalıdır.** Her biri ayrı kalmalıdır. Önceki iki grafik teolduğu gibi, onlar da başka bir aralığın bir alt ağ olamaz. Aralıklar arasında çakışmalar oluşursa, Azure sanal ağı ExpressRoute devresine bağlanmayabilir.

## <a name="next-steps-after-address-ranges-have-been-defined"></a>Adres aralıkları tanımlandıktan sonraki adımlar
IP adresi aralıkları tanımlandıktan sonra aşağıdaki şeylerin gerçekleşmesi gerekir:

1. Azure sanal ağ adres alanı, ER-P2P bağlantısı ve sunucu IP havuzu adres aralığı için IP adres aralıklarını belgenin başında listelenen diğer verilerle birlikte gönderin. Bu noktada, sanal ağ ve VM alt ağlarını da oluşturmaya başlayabilirsiniz. 
2. ExpressRoute devresi, Microsoft tarafından Azure aboneliğiniz ve HANA Büyük Örnek damgası arasında oluşturulur.
3. Microsoft tarafından Büyük Örnek damgası üzerinde bir kiracı ağı oluşturulur.
4. Microsoft, AZURE'daki SAP HANA (Büyük Örnekler) altyapısındaki ağ ları, HANA Büyük Örnekleri ile iletişim kurandaki Azure sanal ağ adres alanınızdaki IP adreslerini kabul etmek üzere yapılandırır.
5. Satın aldığınız Azure (Büyük Örnekler) SKU'daki belirli SAP HANA'ya bağlı olarak, Microsoft kiracı ağında bir bilgi işlem birimi atar. Ayrıca depolama yı ayırır ve bağlar ve işletim sistemini (SUSE veya Red Hat Linux) yükler. Bu birimlerin IP adresleri, Microsoft'a gönderdiğiniz Server IP Pool adres aralığından çıkarılır.

Dağıtım işleminin sonunda Microsoft size aşağıdaki verileri sunar:
- Azure sanal ağınızı HANA Büyük Örnekleri'ne bağlayan ExpressRoute devresine bağlamak için gereken bilgiler:
     - Yetkilendirme anahtarı(lar)
     - ExpressRoute PeerID
- ExpressRoute devresi ve Azure sanal ağı kurduktan sonra HANA Büyük Örnekleri'ne erişmek için veriler.

Hana Büyük Örnekleri'ni [Azure (Büyük Örnekler) Kurulumu'nda SAP HANA](https://azure.microsoft.com/resources/sap-hana-on-azure-large-instances-setup/)belgesinde bağlama sırasını da bulabilirsiniz. Aşağıdaki adımların çoğu, bu belgede örnek bir dağıtımda gösterilir. 

## <a name="next-steps"></a>Sonraki adımlar

- Sanal [ağı HANA Büyük Örnek ExpressRoute'a bağlamaya](hana-connect-vnet-express-route.md)bakın.
