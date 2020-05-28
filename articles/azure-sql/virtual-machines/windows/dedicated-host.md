---
title: Azure ayrılmış ana bilgisayar üzerinde SQL Server VM
description: Azure adanmış bir konakta SQL Server VM çalıştırmaya ilişkin ayrıntılar hakkında bilgi edinin.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: jroth
tags: azure-resource-manager
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 08/12/2019
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: a16ec7a3fa1f41d8c9339f84be6a0ffc3842d099
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84046029"
---
# <a name="sql-server-vm-on-an-azure-dedicated-host"></a>Azure ayrılmış ana bilgisayar üzerinde SQL Server VM 
[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]

Bu makalede, [Azure adanmış ana bilgisayar](/azure/virtual-machines/windows/dedicated-hosts)ile SQL Server VM kullanmanın özellikleri ayrıntılı olarak açıklanır. Azure adanmış ana bilgisayar hakkında ek bilgilere, [Azure adanmış ana bilgisayarı tanıtma](https://azure.microsoft.com/blog/introducing-azure-dedicated-host/)blog gönderisine ulaşabilirsiniz. 

## <a name="overview"></a>Genel Bakış
[Azure adanmış ana bilgisayar](/azure/virtual-machines/windows/dedicated-hosts) , bir veya daha fazla sanal makineyi bir Azure aboneliğine ayrılmış olarak barındırabilecek fiziksel sunucular sağlayan bir hizmettir. Adanmış konaklar, Microsoft 'un bir kaynak olarak sağlanmış olan veri merkezlerinde kullanılan fiziksel sunuculardır. Bir bölge, kullanılabilirlik alanı ve hata etki alanı içinde adanmış konaklar sağlayabilirsiniz. Daha sonra, gereksinimlerinizi en iyi şekilde karşılayan her yapılandırma için VM 'Leri doğrudan sağlanan konaklarınıza yerleştirebilirsiniz.

## <a name="limitations"></a>Sınırlamalar

- Sanal Makine Ölçek Kümeleri Şu anda adanmış konaklarda desteklenmiyor.
- Aşağıdaki VM Serisi destekleniyor: DSv3 ve ESv3. 

## <a name="licensing"></a>Lisanslama

SQL Server VM Azure ayrılmış bir konağa eklerken iki farklı lisans seçeneği arasından seçim yapabilirsiniz. 

  - **SQL VM lisanslama**: Bu, her SQL Server VM lisansı için ayrı olarak ödediğiniz mevcut lisanslama seçeneğidir. 
  - **Adanmış konak lisanslama**: Azure adanmış ana bilgisayar için sunulan yeni lisanslama modeli, SQL Server lisanslarının paketlenmiş ve ana bilgisayar düzeyinde için ödendiği yerdir. 


Mevcut SQL Server lisanslarını kullanmaya yönelik konak düzeyi seçenekleri: 
  - SQL Server Enterprise Edition Azure Hibrit Avantajı
    - SA veya aboneliği olan müşteriler tarafından kullanılabilir.
    - Tüm kullanılabilir fiziksel çekirdekleri lisanslayın ve sınırsız sanallaştırmadan yararlanın (konak tarafından desteklenen maks. sanal CPU 'lara kadar).
        - Azure adanmış ana bilgisayara Azure Hibrit Avantajı uygulama hakkında daha fazla bilgi için [Azure HIBRIT AVANTAJı SSS](https://azure.microsoft.com/pricing/hybrid-benefit/faq/)bölümüne bakın. 
  - SQL Server Lisans 1 Ekim 'Den önce alındı
      - SQL Server Enterprise sürümünde hem ana bilgisayar düzeyi hem de VM 'ler lisans seçenekleri vardır. 
      - SQL Server Standard Edition 'ın yalnızca sanal makine lisansı seçeneği kullanılabilir. 
          - Ayrıntılar için bkz. [Microsoft 'un ürün koşulları](https://www.microsoft.com/licensing/product-licensing/products). 
  - SQL Server adanmış bir konak düzeyi seçeneği seçili değilse, tıpkı çok kiracılı VM 'Lerde olduğu gibi, tek tek sanal makineler düzeyinde SQL Server AHB seçilebilir.



## <a name="provisioning"></a>Sağlama  
Adanmış konağa SQL Server VM sağlama diğer Azure sanal makinenden farklı değildir. [Azure PowerShell](../../../virtual-machines/windows/dedicated-hosts-powershell.md), [Azure Portal](../../../virtual-machines/windows/dedicated-hosts-portal.md)ve [Azure CLI](../../../virtual-machines/linux/dedicated-hosts-cli.md)kullanarak bunu yapabilirsiniz.

Ayrılmış konağa var olan bir SQL Server VM ekleme işlemi kapalı kalma süresi gerektirir, ancak verileri etkilemez ve veri kaybına neden olmayacaktır. Nonetheless, sistem veritabanları da dahil olmak üzere tüm veritabanlarının taşımadan önce yedeklenmesi gerekir.

## <a name="virtualization"></a>Sanallaştırma 

Adanmış bir konağın avantajlarından biri sınırsız sanallaştırmadır. Örneğin, 64 sanal çekirdek lisanslarına sahip olabilirsiniz, ancak ana bilgisayarı 128 sanal çekirdeğe sahip olacak şekilde yapılandırabilir, bu sayede sanal çekirdekleri yalnızca SQL Server lisanslarının ücretini ödeirsiniz. 

Ana bilgisayarınız olduğundan bu yana bir 1:2 oranıyla sanallaştırmayı ayarlamaya uygun olursunuz. 

## <a name="faq"></a>SSS

**S: Azure Hibrit Avantajı Azure ayrılmış ana bilgisayarında Windows Server/SQL Server lisansları için nasıl çalışır?**

Y: müşteriler, Azure Hibrit Avantajı kullanarak Azure adanmış ana bilgisayar üzerinde daha düşük bir ücret ödemek için mevcut Windows Server ve Yazılım Güvencesi kapsamındaki SQL Server lisanslarını veya uygun abonelik lisanslarını kullanabilir. Windows Server Datacenter ve SQL Server Enterprise Edition müşterileri, tüm Konağı lisanslarsa ve Azure Hibrit Avantajı kullanarak sınırsız sanallaştırma (ana bilgisayarda mümkün olduğunca çok sayıda Windows Server sanal makinesi olarak dağıtın) alır.  Azure ayrılmış ana bilgisayarındaki tüm Windows Server ve SQL Server iş yükleri, ek ücret ödemeden Windows Server ve SQL Server 2008/R2 için genişletilmiş güvenlik güncelleştirmelerine de uygundur. 

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki makaleleri inceleyin: 

* [Windows VM 'de SQL Server genel bakış](sql-server-on-azure-vm-iaas-what-is-overview.md)
* [Windows VM 'de SQL Server hakkında SSS](frequently-asked-questions-faq.md)
* [Windows VM üzerinde SQL Server için fiyatlandırma Kılavuzu](pricing-guidance.md)
* [Windows VM 'de SQL Server için sürüm notları](doc-changes-updates-release-notes.md)


