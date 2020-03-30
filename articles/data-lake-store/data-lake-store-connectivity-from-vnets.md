---
title: VNET'lerden Azure Veri Gölü Depolama Gen1'e bağlanın | Microsoft Dokümanlar
description: Azure VNET'lerden Azure Veri Gölü Depolama Gen1'e bağlanın
services: data-lake-store,data-catalog
documentationcenter: ''
author: esung22
manager: mtillman
editor: cgronlun
ms.assetid: 683fcfdc-cf93-46c3-b2d2-5cb79f5e9ea5
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.date: 01/31/2018
ms.author: elsung
ms.openlocfilehash: c8d028a981d7811ed2c864db5750afc83ab93b2b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60878877"
---
# <a name="access-azure-data-lake-storage-gen1-from-vms-within-an-azure-vnet"></a>Azure VNET içindeki VM'lerden Azure Veri Gölü Depolama Gen1'e erişin
Azure Veri Gölü Depolama Gen1, genel Internet IP adreslerinde çalışan bir PaaS hizmetidir. Genel Internet'e bağlanabilen herhangi bir sunucu genellikle Azure Veri Gölü Depolama Gen1 uç noktalarına da bağlanabilir. Varsayılan olarak, Azure VNET'lerde bulunan tüm Sanal Taşıtlar Internet'e erişebilir ve bu nedenle Azure Veri Gölü Depolama Gen1'e erişebilir. Ancak, VNET'teki VM'leri Internet erişimi olmayacak şekilde yapılandırmak mümkündür. Bu tür Sanal Taşıtlar için Azure Veri Gölü Depolama Gen1'e erişim de kısıtlanmıştır. Azure VNET'lerde VMS'ler için genel Internet erişiminin engellenmesi aşağıdaki yaklaşımlardan herhangi biri kullanılarak yapılabilir:

* Ağ Güvenlik Gruplarını (NSG) yapılandırarak
* Kullanıcı Tanımlı Rotaları (UDR) yapılandırarak
* ExpressRoute kullanıldığında BGP (endüstri standardı dinamik yönlendirme protokolü) üzerinden rota alışverişi yaparak, Internet'e erişimi engelleyen

Bu makalede, daha önce listelenen üç yöntemden birini kullanarak kaynaklara erişmekle sınırlandırılmış olan Azure VM'lerden Azure Veri Gölü Depolama Gen1'e erişimi nasıl etkinleştireceğinizi öğreneceksiniz.

## <a name="enabling-connectivity-to-azure-data-lake-storage-gen1-from-vms-with-restricted-connectivity"></a>Sınırlı bağlantı yla VM'lerden Azure Veri Gölü Depolama Gen1'e bağlantı sağlama
Bu tür VM'lerden Azure Veri Gölü Depolama Gen1'e erişmek için, bunları Azure Veri Gölü Depolama Gen1 hesabının bulunduğu bölgenin IP adresine erişmek üzere yapılandırmanız gerekir. Hesaplarınızın DNS adlarını çözerek Veri Gölü Depolama Gen1 hesap bölgelerinizin IP`<account>.azuredatalakestore.net`adreslerini belirleyebilirsiniz . Hesaplarınızın DNS adlarını çözmek için **nslookup**gibi araçları kullanabilirsiniz. Bilgisayarınızda bir komut istemi açın ve aşağıdaki komutu çalıştırın:

    nslookup mydatastore.azuredatalakestore.net

Çıktı aşağıdakilere benzer. **Adres** özelliğine karşı değer, Veri Gölü Depolama Gen1 hesabınızla ilişkili IP adresidir.

    Non-authoritative answer:
    Name:    1434ceb1-3a4b-4bc0-9c69-a0823fd69bba-mydatastore.projectcabostore.net
    Address:  104.44.88.112
    Aliases:  mydatastore.azuredatalakestore.net


### <a name="enabling-connectivity-from-vms-restricted-by-using-nsg"></a>NSG kullanılarak kısıtlanan VM'lerden bağlantı sağlama
Internet'e erişimi engellemek için bir NSG kuralı kullanıldığında, Veri Gölü Depolama Gen1 IP Adresine erişime izin veren başka bir NSG oluşturabilirsiniz. NSG kuralları hakkında daha fazla bilgi için [Ağ güvenlik gruplarına genel bakış](../virtual-network/security-overview.md)alabakın. NSG'lerin nasıl oluşturulacağına ilişkin yönergeler için ağ güvenlik grubu oluşturma ya [da nasıl oluşturacağınız](../virtual-network/tutorial-filter-network-traffic.md)hakkında bilgi alabiliyorum.

### <a name="enabling-connectivity-from-vms-restricted-by-using-udr-or-expressroute"></a>UDR veya ExpressRoute kullanılarak kısıtlanan VM'lerden bağlantı sağlama
ÜD'ler veya BGP değiştirilen rotalar Internet'e erişimi engellemek için kullanıldığında, bu tür alt ağlardaki VM'lerin Veri Gölü Depolama Gen1 uç noktalarına erişebilmesi için özel bir rotanın yapılandırılması gerekir. Daha fazla bilgi için [Kullanıcı tanımlı rotalara genel bakış](../virtual-network/virtual-networks-udr-overview.md)alabakın. ÜD'ler oluşturma yönergeleri için Kaynak [Yöneticisi'nde UD'ler oluştur'a](../virtual-network/tutorial-create-route-table-powershell.md)bakın.

### <a name="enabling-connectivity-from-vms-restricted-by-using-expressroute"></a>ExpressRoute kullanılarak kısıtlanan VM'lerden bağlantı sağlama
Bir ExpressRoute devresi yapılandırıldığında, şirket içi sunucular genel bakış yoluyla Veri Gölü Depolama Gen1'e erişebilir. Public-resiyasyon için ExpressRoute'un yapılandırılması hakkında daha fazla bilgi [ExpressRoute SSS'de](../expressroute/expressroute-faqs.md)mevcuttur.

## <a name="see-also"></a>Ayrıca bkz.
* [Azure Veri Gölü Depolama Gen1'e Genel Bakış](data-lake-store-overview.md)
* [Azure Data Lake Storage 1. Nesil'de depolanan verilerin güvenliğini sağlama](data-lake-store-security-overview.md)

