---
title: VNET 'lerden Azure Data Lake Storage 1. bağlanma | Microsoft Docs
description: Azure sanal ağları 'ndan Azure Data Lake Storage 1. bağlanma
services: data-lake-store,data-catalog
documentationcenter: ''
author: esung22
manager: mtillman
editor: cgronlun
ms.assetid: 683fcfdc-cf93-46c3-b2d2-5cb79f5e9ea5
ms.service: data-lake-store
ms.devlang: na
ms.topic: how-to
ms.date: 01/31/2018
ms.author: elsung
ms.openlocfilehash: 080f1a55e70946281a11af44176600abfc5bc0e2
ms.sourcegitcommit: 374e47efb65f0ae510ad6c24a82e8abb5b57029e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2020
ms.locfileid: "85515711"
---
# <a name="access-azure-data-lake-storage-gen1-from-vms-within-an-azure-vnet"></a>Bir Azure sanal ağı içindeki VM 'lerden erişim Azure Data Lake Storage 1.
Azure Data Lake Storage 1., genel Internet IP adresleri üzerinde çalışan bir PaaS hizmetidir. Genel Internet 'e bağlanabilecek herhangi bir sunucu, genellikle Azure Data Lake Storage 1. uç noktalara bağlanabilir. Varsayılan olarak, Azure sanal ağları 'nda bulunan tüm VM 'Ler Internet 'e erişebilir ve bu nedenle Azure Data Lake Storage 1. erişebilir. Ancak, VNET 'teki VM 'Lerin Internet erişimi olmayan şekilde yapılandırılması mümkündür. Bu tür sanal makineler için Azure Data Lake Storage 1. erişimi de kısıtlıdır. Azure sanal ağları 'nda VM 'Ler için genel Internet erişimini engelleme, aşağıdaki yaklaşımlardan herhangi biri kullanılarak yapılabilir:

* Ağ güvenlik gruplarını (NSG) yapılandırarak
* Kullanıcı tanımlı yolları (UDR) yapılandırarak
* ExpressRoute kullanıldığında BGP (sektör standardı dinamik yönlendirme protokolü) aracılığıyla yollar değiş tokuşu yaparak Internet 'e erişimi engeller

Bu makalede, Azure VM 'lerinden Azure Data Lake Storage 1. erişimi etkinleştirmeyi öğrenirsiniz ve bu, daha önce listelenen üç yöntemden birini kullanarak kaynaklarla erişim ile sınırlıdır.

## <a name="enabling-connectivity-to-azure-data-lake-storage-gen1-from-vms-with-restricted-connectivity"></a>Kısıtlanmış bağlantısı olan VM 'lerden Azure Data Lake Storage 1. bağlantı etkinleştiriliyor
Bu VM 'lerden Azure Data Lake Storage 1. erişmek için, bunları Azure Data Lake Storage 1. hesabının kullanılabildiği bölgenin IP adresine erişecek şekilde yapılandırmanız gerekir. Hesaplarınızın () DNS adlarını çözümleyerek Data Lake Storage 1. hesap Bölgelerinizdeki IP adreslerini belirleyebilirsiniz `<account>.azuredatalakestore.net` . Hesaplarınızın DNS adlarını çözümlemek için, **nslookup**gibi araçları kullanabilirsiniz. Bilgisayarınızda bir komut istemi açın ve aşağıdaki komutu çalıştırın:

    nslookup mydatastore.azuredatalakestore.net

Çıktı aşağıdakine benzer. **Address** özelliğine karşı değer, Data Lake Storage 1. HESABıNıZLA ilişkili IP adresidir.

    Non-authoritative answer:
    Name:    1434ceb1-3a4b-4bc0-9c69-a0823fd69bba-mydatastore.projectcabostore.net
    Address:  104.44.88.112
    Aliases:  mydatastore.azuredatalakestore.net


### <a name="enabling-connectivity-from-vms-restricted-by-using-nsg"></a>NSG kullanılarak kısıtlanan VM 'lerden bağlantı etkinleştiriliyor
Internet 'e erişimi engellemek için bir NSG kuralı kullanıldığında, Data Lake Storage 1. IP adresine erişime izin veren başka bir NSG oluşturabilirsiniz. NSG kuralları hakkında daha fazla bilgi için bkz. [ağ güvenlik gruplarına genel bakış](../virtual-network/security-overview.md). NSG 'ler oluşturma hakkında yönergeler için bkz. [ağ güvenlik grubu oluşturma](../virtual-network/tutorial-filter-network-traffic.md).

### <a name="enabling-connectivity-from-vms-restricted-by-using-udr-or-expressroute"></a>UDR veya ExpressRoute kullanılarak kısıtlanan VM 'lerden bağlantı etkinleştiriliyor
Internet 'e erişimi engellemek için UDRs veya BGP tarafından değiştirilen yollar kullanıldığında, bu alt ağlardaki VM 'Lerin Data Lake Storage 1. uç noktalarına erişebilmeleri için özel bir yolun yapılandırılması gerekir. Daha fazla bilgi için bkz. [Kullanıcı tanımlı yollara genel bakış](../virtual-network/virtual-networks-udr-overview.md). UDRs oluşturmayla ilgili yönergeler için, [Kaynak Yöneticisi UDRs oluşturma](../virtual-network/tutorial-create-route-table-powershell.md)bölümüne bakın.

### <a name="enabling-connectivity-from-vms-restricted-by-using-expressroute"></a>ExpressRoute kullanılarak kısıtlanan VM 'lerden bağlantı etkinleştiriliyor
Bir ExpressRoute bağlantı hattı yapılandırıldığında, şirket içi sunucular Data Lake Storage 1. ortak eşleme aracılığıyla erişebilir. ExpressRoute [SSS](../expressroute/expressroute-faqs.md)' de genel eşleme Için ExpressRoute 'u yapılandırma hakkında daha fazla bilgi bulabilirsiniz.

## <a name="see-also"></a>Ayrıca bkz.
* [Azure Data Lake Storage 1. genel bakış](data-lake-store-overview.md)
* [Azure Data Lake Storage 1. Nesil'de depolanan verilerin güvenliğini sağlama](data-lake-store-security-overview.md)

