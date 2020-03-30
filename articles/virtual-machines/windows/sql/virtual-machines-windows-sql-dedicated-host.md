---
title: Azure Özel Ana Bilgisayar'da SQL Server VM
description: Azure Özel Ana Bilgisayar'da SQL Server VM çalıştırmanın ayrıntıları hakkında bilgi edinin.
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
ms.openlocfilehash: edb2d3fa670475d9b08fe05494035949181a9240
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75834343"
---
# <a name="sql-server-vm-on-an-azure-dedicated-host"></a>Azure Özel Ana Bilgisayar'da SQL Server VM 

Bu makalede, [Azure Özel Ana Bilgisayar](/azure/virtual-machines/windows/dedicated-hosts)ile bir SQL Server VM kullanmanın özellikleri ayrıntılı olarak açıklanmaktadır. Azure'a özel ana bilgisayar hakkında ek bilgiler, [Azure Özel Ana Bilgisayar'ı Tanıtma](https://azure.microsoft.com/blog/introducing-azure-dedicated-host/)adlı blog gönderisinde bulunabilir. 

## <a name="overview"></a>Genel Bakış
[Azure Özel Ana Bilgisayar,](/azure/virtual-machines/windows/dedicated-hosts) bir veya daha fazla sanal makineyi barındırabilen fiziksel sunucular sağlayan bir hizmettir. Özel ana bilgisayarlar, Microsoft'un veri merkezlerinde kullanılan ve kaynak olarak sağlanan fiziksel sunucularla aynıdır. Bir bölge, kullanılabilirlik bölgesi ve hata etki alanı içinde özel ana bilgisayarları sağlayabilirsiniz. Ardından, gereksinimlerinizi en iyi karşılayan yapılandırmada VM'leri doğrudan sağlanan ana bilgisayarlarınıza yerleştirebilirsiniz.

## <a name="limitations"></a>Sınırlamalar

- Sanal makine ölçek kümeleri şu anda özel ana bilgisayarlarda desteklenmez.
- Aşağıdaki VM serisi desteklenir: DSv3 ve ESv3. 

## <a name="licensing"></a>Lisanslama

SQL Server VM'nizi Azure Özel Ana Bilgisayar'a eklerken iki farklı lisans seçeneği arasında seçim yapabilirsiniz. 

  - **SQL VM lisanslama**: Her SQL Server VM lisansı için ayrı ayrı ödeme yaptığınız mevcut lisanslama seçeneğidir. 
  - **Özel ana bilgisayar lisansı**: SQL Server lisanslarının ana bilgisayar düzeyinde paketlendiği ve ödendiği Azure Özel Ana Bilgisayar için kullanılabilen yeni lisanslama modeli. 


Varolan SQL Server lisanslarını kullanmak için ana bilgisayar düzeyinde seçenekler: 
  - SQL Server Enterprise Edition Azure Karma Avantajı
    - SA veya aboneliği olan müşteriler tarafından kullanılabilir.
    - Mevcut tüm fiziksel çekirdekleri lisansla ve sınırsız sanallaştırmanın keyfini çıkarın (ana bilgisayar tarafından desteklenen maksimum vCPUs'a kadar).
        - Azure Karma Avantajını Azure Özel ana bilgisayara uygulama hakkında daha fazla bilgi için [Azure Karma Avantajı SSS'sine](https://azure.microsoft.com/pricing/hybrid-benefit/faq/)bakın. 
  - 1 Ekim'den Önce Alınan SQL Server Lisansları
      - SQL Server Enterprise sürümü hem ana bilgisayar düzeyinde hem de by-VM lisans seçeneklerine sahiptir. 
      - SQL Server Standard sürümü yalnızca by-VM lisans seçeneğine sahiptir. 
          - Ayrıntılar için [Microsoft'un Ürün Koşulları'na](https://www.microsoft.com/licensing/product-licensing/products)bakın. 
  - SQL Server'a özel ana bilgisayar düzeyi seçeneği seçili değilse, sql server AHB, tıpkı çok kiracılı VM'lerde olduğu gibi tek tek VM düzeyinde seçilebilir.



## <a name="provisioning"></a>Sağlama  
Sql Server VM'yi ilgili ana bilgisayara sağlamanın diğer Azure Sanal Makinesi'nden farkı yoktur. Bunu [Azure PowerShell,](../dedicated-hosts-powershell.md)Azure [portalı](../dedicated-hosts-portal.md)ve [Azure CLI'yi](../../linux/dedicated-hosts-cli.md)kullanarak yapabilirsiniz.

Özel ana bilgisayara varolan bir SQL Server VM ekleme işlemi kapalı kalma süresi gerektirir, ancak verileri etkilemez ve veri kaybı olmaz. Bununla birlikte, sistem veritabanları da dahil olmak üzere tüm veritabanları, taşımadan önce yedeklenmelidir.

## <a name="virtualization"></a>Sanallaştırma 

Özel bir ana bilgisayar avantajlarından biri sınırsız sanallaştırmaolduğunu. Örneğin, 64 vCores için lisansları olabilir, ancak 128 vCores için ana yapılandırılmışolabilir, böylece iki kez vCores olsun ama sadece yarısı SQL Server lisansları için ödeme. 

Ev sahibiniz olduğundan, sanallaştırmayı 1:2 oranıyla ayarlamaya uygunsunuz. 

## <a name="faq"></a>SSS

**S: Azure Özel Ana Bilgisayar'da Windows Server/SQL Server lisansları için Azure Karma Avantajı nasıl çalışır?**

C: Müşteriler, Azure Karma Avantajı'nı kullanarak Azure Özel Ana Bilgisayar'da indirimli ücret ödemek için Yazılım Güvencesi veya uygun abonelik lisansları ile mevcut Windows Server ve SQL Server lisanslarının değerini kullanabilir. Windows Server Datacenter ve SQL Server Enterprise Edition müşterileri, tüm ana bilgisayarı lisansladıklarında sınırsız sanallaştırma (ana bilgisayara ana bilgisayarda mümkün olduğunca çok Windows Server sanal makinesi dağıtma) alır ve Azure Karma Avantajı'nı kullanın.  Azure Özel Ana Bilgisayar'daki tüm Windows Server ve SQL Server iş yükleri, ek ücret ödemeden Windows Server ve SQL Server 2008/R2 için Genişletilmiş Güvenlik Güncelleştirmeleri için de uygundur. 

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki makalelere bakın: 

* [Windows VM'de SQL Server'a Genel Bakış](virtual-machines-windows-sql-server-iaas-overview.md)
* [Windows VM'de SQL Server için SSS](virtual-machines-windows-sql-server-iaas-faq.md)
* [Windows VM'de SQL Server için fiyatlandırma kılavuzu](virtual-machines-windows-sql-server-pricing-guidance.md)
* [Windows VM'de SQL Server için sürüm notları](virtual-machines-windows-sql-server-iaas-release-notes.md)


