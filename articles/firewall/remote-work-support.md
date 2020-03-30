---
title: Azure Güvenlik Duvarı uzaktan çalışma desteği
description: Bu makalede, Azure Güvenlik Duvarı'nın uzak iş gücü gereksinimlerinizi nasıl destekleyebilir olduğu gösterilmektedir.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: conceptual
ms.date: 03/25/2020
ms.author: victorh
ms.openlocfilehash: 5abe9344b0512433c48df50335cce5cf1e3e3547
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80289647"
---
# <a name="azure-firewall-remote-work-support"></a>Azure Güvenlik Duvarı uzaktan çalışma desteği

Azure Güvenlik Duvarı, Azure sanal ağ kaynaklarınızı koruyan yönetilen, bulut tabanlı bir ağ güvenlik hizmetidir. Yerleşik yüksek kullanılabilirliğe ve sınırsız bulut ölçeklenebilirliğine sahip, tam durum bilgisi olan bir hizmet olarak güvenlik duvarıdır. 

## <a name="firewall-rules"></a>Güvenlik duvarı kuralları

Azure Güvenlik Duvarı [DNAT kurallarını](rule-processing.md)kullanarak Sanal masaüstü altyapınızı (VDI) gelen RDP erişiminizi Güvence altına almak için Azure Güvenlik Duvarı'nı kullanabilirsiniz. Windows Sanal Masaüstü (WVD), sanal ağınıza gelen erişimi açmanızı gerektirmez. Ancak, sanal ağınızda çalışan WVD sanal makineleri için bir dizi giden ağ bağlantısına izin vermelisiniz. Daha fazla bilgi için [Windows Sanal Masaüstü nedir?](../virtual-desktop/overview.md#requirements)

Bu giden erişimi Azure Güvenlik Duvarı uygulama kurallarını kullanarak yapılandırabilirsiniz. Daha fazla bilgi için [Bkz. Öğretici: Azure portalını kullanarak Azure Güvenlik Duvarını dağıtın ve yapılandırın.](tutorial-firewall-deploy-portal.md)

## <a name="next-steps"></a>Sonraki adımlar

[Windows Sanal Masaüstü](https://docs.microsoft.com/azure/virtual-desktop/)hakkında daha fazla bilgi edinin.