---
title: Azure Resource Manager Şablonları
description: Azure Site Recovery özellikleri kullanmak için Şablonlar Azure Resource Manager.
services: site-recovery
author: rishjai
manager: gaggupta
ms.service: site-recovery
ms.topic: article
ms.date: 02/04/2021
ms.author: rishjai
ms.openlocfilehash: efd5c1ab3e23348373be76c79d1b9689bc5f4941
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "101720537"
---
# <a name="azure-resource-manager-templates-for-azure-site-recovery"></a>Azure Site Recovery için Azure Resource Manager Şablonlar

Aşağıdaki tabloda Azure Site Recovery özellikleri kullanmak için Azure Resource Manager şablonlarının bağlantıları yer almaktadır.

| Şablon | Description |
|---|---|
|**Azure-Azure arası** | |
| [Kurtarma Hizmetleri Kasası oluşturma](./quickstart-create-vault-template.md)| Kurtarma Hizmetleri kasası oluşturun. Kasa Azure Backup ve Azure Site Recovery için kullanılabilir. |
| [Azure VM 'Leri için çoğaltmayı etkinleştirme](https://aka.ms/asr-arm-enable-replication) | Mevcut kasayı ve özel hedef ayarlarını kullanarak Azure VM 'Leri için çoğaltmayı etkinleştirin.|
| [Yük devretmeyi tetikleme ve yeniden koruma](https://aka.ms/asr-arm-failover-reprotect) | Bir Azure VM kümesi için yük devretme ve yeniden koruma işlemi tetikleyin. |
| [Azure VM 'Leri için uçtan uca DR akışı çalıştırma](https://aka.ms/asr-arm-e2e-flow) | Azure VM 'Leri için 540 ° akış olarak da bilinen, son olağanüstü durum kurtarma akışını (çoğaltma + yük devretmeyi etkinleştirin ve yeniden çalışma ve yeniden koruma) başlatın.|
|   |   |