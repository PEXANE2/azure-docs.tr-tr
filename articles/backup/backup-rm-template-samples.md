---
title: Azure Resource Manager şablonları
description: Kurtarma Hizmetleri kasaları ve Azure Yedekleme özellikleriyle kullanılmak üzere Azure Kaynak Yöneticisi şablonları
ms.topic: sample
ms.date: 01/31/2019
ms.custom: mvc
ms.openlocfilehash: 3c05f345c6c5df5d0350c75e52381c24b0609501
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74172175"
---
# <a name="azure-resource-manager-templates-for-azure-backup"></a>Azure Backup için Azure Resource Manager şablonları

Aşağıdaki tabloda Kurtarma Hizmetleri kasaları ve Azure Backup özellikleri ile birlikte kullanılacak Azure Resource Manager şablonlarının bağlantıları bulunur. JSON sözdizimi ve özellikleri hakkında bilgi edinmek için [Microsoft.RecoveryServices kaynak türlerine](/azure/templates/microsoft.recoveryservices/allversions)bakın.

|   |   |
|---|---|
|**Kurtarma Hizmetleri kasası** | |
| [Kurtarma Hizmetleri kasası oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/101-recovery-services-vault-create)| Kurtarma Hizmetleri kasası oluşturun. Kasa Azure Backup ve Azure Site Recovery için kullanılabilir. |
|**Sanal makineleri yedekleme**| |
| [Kaynak Yöneticisi VM’lerini yedekleme](https://github.com/Azure/azure-quickstart-templates/tree/master/101-recovery-services-backup-vms) | Aynı kaynak grubundaki Kaynak Yöneticisi sanal makinelerini yedeklemek için kaynak var olan Kurtarma Hizmetleri kasasını ve Yedekleme ilkesini kullanın.|
| [IaaS sanal makinelerini Kurtarma Hizmetleri kasasına yedekleme](https://github.com/Azure/azure-quickstart-templates/tree/master/201-recovery-services-backup-classic-resource-manager-vms) | Klasik ve Kaynak Yöneticisi sanal makinelerini yedekleme şablonu. |
| [IaaS sanal makineleri için Haftalık Yedekleme ilkesi oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/101-recovery-services-weekly-backup-policy-create) | Şablon, Klasik ve Kaynak Yöneticisi-sanal makineleri yedeklemek için kullanılan Kurtarma Hizmetleri kasası ve haftalık yedekleme ilkesi oluşturur.|
| [IaaS sanal makineleri için Günlük Yedekleme ilkesi oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/101-recovery-services-daily-backup-policy-create) | Şablon, Klasik ve Kaynak Yöneticisi-sanal makineleri yedeklemek için kullanılan Kurtarma Hizmetleri kasası ve günlük yedekleme ilkesi oluşturur.|
| [Yedekleme özelliği etkin Windows Server VM dağıtma](https://github.com/Azure/azure-quickstart-templates/tree/master/101-recovery-services-create-vm-and-configure-backup) | Şablon, varsayılan yedekleme ilkesi etkinken bir Windows Server VM ve Kurtarma Hizmetleri kasası oluşturur.|
|**Yedekleme işlerini izleme** |  |
| [Azure Yedekleme ile Azure Monitör günlüklerini kullanma](https://github.com/Azure/azure-quickstart-templates/tree/master/101-backup-oms-monitoring) | Şablon, Yedekleme işlerini, yedekleme uyarılarını ve Kurtarma Hizmetleri kasalarınızda kullanılan Bulut depolamasını izlemenize ve geri yüklemenize olanak tanıyan Azure Yedekleme ile Azure Monitor günlüklerini dağıtıyor.|  
|**Azure VM'de SQL Server'ı yedekleme** |  |
| [Azure VM'de SQL Server'ı yedekleme](https://github.com/Azure/azure-quickstart-templates/tree/master/101-recovery-services-vm-workload-backup) | Şablon, Kurtarma Hizmetleri Kasası ve İş Yüküne Özgü Yedekleme İlkesi oluşturur. VM'yi Azure Yedekleme hizmetiyle kaydeder ve bu VM'de Korumayı Yapılandırır. Şu anda yalnızca SQL Galeri görüntüleri için çalışır. |
|   |   |
