---
title: Azure Güvenlik Duvarı Uzaktan çalışma desteği
description: Bu makalede, Azure Güvenlik duvarının uzak iş gücü gereksinimlerinizi nasıl destekleyedikleri gösterilmektedir.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: conceptual
ms.date: 03/25/2020
ms.author: victorh
ms.openlocfilehash: 5abe9344b0512433c48df50335cce5cf1e3e3547
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80289647"
---
# <a name="azure-firewall-remote-work-support"></a>Azure Güvenlik Duvarı Uzaktan çalışma desteği

Azure Güvenlik Duvarı, Azure sanal ağ kaynaklarınızı koruyan, yönetilen, bulut tabanlı bir ağ güvenlik hizmetidir. Yerleşik yüksek kullanılabilirliğe ve sınırsız bulut ölçeklenebilirliğine sahip, tam durum bilgisi olan bir hizmet olarak güvenlik duvarıdır. 

## <a name="firewall-rules"></a>Güvenlik duvarı kuralları

Azure Güvenlik Duvarı [DNAT kurallarını](rule-processing.md)kullanarak, sanal masaüstü ALTYAPıNıZıN (VDI) gelen RDP erişiminizi Azure sanal ağınıza güvenli hale getirmek Için Azure Güvenlik Duvarı 'nı kullanabilirsiniz. Windows sanal masaüstü (WVD), sanal ağınıza yönelik herhangi bir gelen erişimi açmanızı gerektirmez. Ancak, sanal ağınızda çalışan WVD sanal makineler için bir giden ağ bağlantısı kümesine izin vermeniz gerekir. Daha fazla bilgi için bkz. [Windows sanal masaüstü nedir?](../virtual-desktop/overview.md#requirements)

Bu giden erişimi, Azure Güvenlik Duvarı uygulama kurallarını kullanarak yapılandırabilirsiniz. Daha fazla bilgi için bkz. [öğretici: Azure Güvenlik duvarını Azure Portal kullanarak dağıtma ve yapılandırma](tutorial-firewall-deploy-portal.md).

## <a name="next-steps"></a>Sonraki adımlar

[Windows sanal masaüstü](https://docs.microsoft.com/azure/virtual-desktop/)hakkında daha fazla bilgi edinin.