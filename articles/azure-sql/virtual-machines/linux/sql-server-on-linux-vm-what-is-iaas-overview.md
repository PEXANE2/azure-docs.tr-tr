---
title: Azure Linux Sanal Makinelerinde SQL Server'a Genel Bakış | Microsoft Docs
description: Azure Linux sanal makinelerinde tam SQL Server sürümlerini çalıştırma hakkında bilgi edinin. Tüm Linux SQL Server VM görüntülerinin ve ilgili içeriklerin doğrudan bağlantılarını alın.
services: virtual-machines-linux
documentationcenter: ''
author: MashaMSFT
manager: craigg
tags: azure-service-management
ms.service: virtual-machines-sql
ms.topic: conceptual
ms.workload: iaas-sql-server
ms.date: 04/10/2018
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 61b8982868bf14a7b5a5441049cb7fa21cdd9d6d
ms.sourcegitcommit: 309cf6876d906425a0d6f72deceb9ecd231d387c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2020
ms.locfileid: "84266041"
---
# <a name="overview-of-sql-server-on-azure-virtual-machines-linux"></a>Azure Sanal Makinelerinde SQL Server'a Genel Bakış (Linux)
[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]

> [!div class="op_single_selector"]
> * [Windows](../windows/sql-server-on-azure-vm-iaas-what-is-overview.md)
> * [Linux](sql-server-on-linux-vm-what-is-iaas-overview.md)

Azure sanal makinelerinde SQL Server, şirket içi donanım yönetmenize gerek kalmadan tam SQL Server sürümlerini Bulut’ta kullanmanıza olanak sağlar. SQL Server VM’leri, kullandıkça ödeme yaptığınızda lisanslama maliyetlerini de basitleştirir.

Azure sanal makineleri dünyanın birçok farklı [coğrafi bölgesinde](https://azure.microsoft.com/regions/) çalışır. Bu makineler, ayrıca çeşitli [makine boyutları](../../../virtual-machines/windows/sizes.md) sunar. Sanal makine resim galerisi, doğru sürüm ve işletim sistemiyle bir SQL Server VM’i oluşturmanızı sağlar. Bu, sanal makineleri birçok farklı SQL Server iş yükleri için iyi bir seçenek yapar. 

## <a name="get-started-with-sql-vms"></a><a id="create"></a> SQL VM’lerini kullanmaya başlayın

Kullanmaya başlamak için, gerekli olan sürüm ve işletim sisteminizle birlikte bir SQL Server sanal makine resmi seçin. Aşağıdaki bölümlerde SQL Server sanal makine galeri görüntüleri için Azure portalının doğrudan bağlantıları verilmektedir.

> [!TIP]
> SQL görüntülerinin fiyatlandırmasını anlamak için bkz. [Linux SQL Server VM’leri için fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/virtual-machines/linux/).

| Sürüm | İşletim Sistemi | Sürüm |
| --- | --- | --- |
| **SQL Server 2017** | Red Hat Enterprise Linux (RHEL) 7.4 |[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2017EnterpriseonRedHatEnterpriseLinux74), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2017StandardonRedHatEnterpriseLinux74), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2017WebonRedHatEnterpriseLinux74), [Express](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017ExpressonRedHatEnterpriseLinux74), [Developer](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017DeveloperonRedHatEnterpriseLinux74) |
| **SQL Server 2017** | SUSE Linux Enterprise Server (SLES) v12 SP2 |[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2017EnterpriseonSLES12SP2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2017StandardonSLES12SP2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2017WebonSLES12SP2), [Express](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017ExpressonSLES12SP2), [Developer](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017DeveloperonSLES12SP2) |
| **SQL Server 2017** | Ubuntu 16.04 LTS |[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2017EnterpriseonUbuntuServer1604LTS), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2017StandardonUbuntuServer1604LTS), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2017WebonUbuntuServer1604LTS), [Express](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017ExpressonUbuntuServer1604LTS), [Developer](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017DeveloperonUbuntuServer1604LTS) |

> [!NOTE]
> Kullanabileceğiniz Windows SQL Server sanal makine görüntülerini görmek için bkz. [Azure Sanal Makinelerinde SQL Server'a Genel Bakış (Windows)](../windows/sql-server-on-azure-vm-iaas-what-is-overview.md).

## <a name="installed-packages"></a><a id="packages"></a> Yüklü paketler

Linux üzerinde SQL Server yapılandırdıktan sonra veritabanı altyapısı paketini ve gereksinimlerinize bağlı olarak çeşitli isteğe bağlı paketleri yüklersiniz. SQL Server için Linux sanal makine görüntüleri birçok paketi otomatik olarak sizin için yükler. Aşağıdaki tabloda her dağıtımda yüklenen paketler gösterilmektedir.

| Dağıtım | [Veritabanı Altyapısı](https://docs.microsoft.com/sql/linux/sql-server-linux-setup) | [Araçlar](https://docs.microsoft.com/sql/linux/sql-server-linux-setup-tools) | [SQL Server Agent](https://docs.microsoft.com/sql/linux/sql-server-linux-setup-sql-agent) | [Tam metin arama](https://docs.microsoft.com/sql/linux/sql-server-linux-setup-full-text-search) | [SSIS](https://docs.microsoft.com/sql/linux/sql-server-linux-setup-ssis) | [HA eklentisi](https://docs.microsoft.com/sql/linux/sql-server-linux-business-continuity-dr) |
|---|---|---|---|---|---|---|
| RHEL | ![evet](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png) | ![evet](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png) | ![evet](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png) | ![evet](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png) | ![evet](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png) | ![hayır](./media/sql-server-on-linux-vm-what-is-iaas-overview/no.png) |
| SLES | ![evet](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png) | ![evet](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png) | ![evet](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png) | ![evet](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png) | ![hayır](./media/sql-server-on-linux-vm-what-is-iaas-overview/no.png) | ![hayır](./media/sql-server-on-linux-vm-what-is-iaas-overview/no.png) |
| Ubuntu | ![evet](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png) | ![evet](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png) | ![evet](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png) | ![evet](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png) | ![evet](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png) | ![evet](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png) |

## <a name="related-products-and-services"></a>İlgili ürün ve hizmetler

### <a name="linux-virtual-machines"></a>Linux Sanal Makineleri

* [Sanal Makinelere genel bakış](../../../virtual-machines/linux/overview.md)

### <a name="storage"></a>Depolama

* [Microsoft Azure Depolama'ya Giriş](../../../storage/common/storage-introduction.md)

### <a name="networking"></a>Ağ

* [Sanal ağa genel bakış](../../../virtual-network/virtual-networks-overview.md)
* [Azure'da IP adresleri](../../../virtual-network/public-ip-addresses.md)
* [Azure portalda Tam Etki Alanı Adı oluşturma](../../../virtual-machines/windows/portal-create-fqdn.md)

### <a name="sql"></a>SQL

* [Linux üzerinde SQL Server belgeleri](https://docs.microsoft.com/sql/linux)
* [Azure SQL Veritabanı karşılaştırması](../../azure-sql-iaas-vs-paas-what-is-overview.md)

## <a name="next-steps"></a>Sonraki adımlar

Azure Linux sanal makinelerinde SQL Server kullanmaya başlayın:

* [Azure portalında SQL Server VM’i oluşturma](sql-vm-create-portal-quickstart.md)

Linux üzerinde SQL VM'ler hakkında sık sorulan soruların yanıtlarını alın:

* [Azure Linux Sanal Makineler’de SQL Server ile ilgili SSS](frequently-asked-questions-faq.md)
