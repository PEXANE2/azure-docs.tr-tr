---
title: Azure Güvenlik Duvarı Uzaktan çalışma desteği
description: Bu makalede, Azure Güvenlik duvarının uzak iş gücü gereksinimlerinizi nasıl destekleyedikleri gösterilmektedir.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: conceptual
ms.date: 05/04/2020
ms.author: victorh
ms.openlocfilehash: da5100fafc98ae38809c93e9b3db5ef41c58766a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "82863425"
---
# <a name="azure-firewall-remote-work-support"></a>Azure Güvenlik Duvarı Uzaktan çalışma desteği

Azure Güvenlik Duvarı, Azure sanal ağ kaynaklarınızı koruyan, yönetilen, bulut tabanlı bir ağ güvenlik hizmetidir. Yerleşik yüksek kullanılabilirliğe ve sınırsız bulut ölçeklenebilirliğine sahip, tam durum bilgisi olan bir hizmet olarak güvenlik duvarıdır.

## <a name="virtual-desktop-infrastructure-vdi-deployment-support"></a>Sanal Masaüstü Altyapısı (VDı) dağıtım desteği

Ana ilkelerden çalışma, çok sayıda BT kuruluşun kapasite, ağ, güvenlik ve idare için temel değişiklikleri ele vermesini gerektirir. Çalışanlar evden çalışırken şirket içi hizmetlerle ilişkili katmanlı güvenlik ilkeleri tarafından korunmaz. Azure 'daki sanal masaüstü altyapısı (VDı) dağıtımları, kuruluşların bu değişen ortama hızla yanıt vermesini sağlar. Ancak, bu VDı dağıtımlarından gelen/giden Internet erişimini korumak için bir yol gerekir. VDı dağıtımlarınızı korumak için Azure Güvenlik Duvarı [DNAT kurallarını](rule-processing.md) [tehdit zekası](threat-intel.md) tabanlı filtreleme yeteneklerini birlikte kullanabilirsiniz.

## <a name="azure-windows-virtual-desktop-support"></a>Azure Windows sanal masaüstü desteği

Windows sanal masaüstü, Azure 'da çalışan kapsamlı bir masaüstü ve uygulama sanallaştırma hizmetidir. Basitleştirilmiş Yönetim, çoklu oturum Windows 10, Office 365 ProPlus için iyileştirmeler ve Uzak Masaüstü Hizmetleri (RDS) ortamları için destek sunan tek sanal masaüstü altyapısı (VDı). Windows Masaüstü ve uygulamalarınızı Azure 'da dakikalar içinde dağıtabilir ve ölçeklendirebilirsiniz ve yerleşik güvenlik ve uyumluluk özellikleri edinebilirsiniz. Windows sanal masaüstü, sanal ağınıza yönelik herhangi bir gelen erişimi açmanızı gerektirmez. Ancak, sanal ağınızda çalışan Windows Sanal Masaüstü sanal makineleri için bir giden ağ bağlantısı kümesine izin vermeniz gerekir. Daha fazla bilgi için bkz. [Azure Güvenlik Duvarı 'Nı kullanarak Windows sanal masaüstü dağıtımlarını koruma](protect-windows-virtual-desktop.md).

## <a name="next-steps"></a>Sonraki adımlar

[Windows sanal masaüstü](https://docs.microsoft.com/azure/virtual-desktop/)hakkında daha fazla bilgi edinin.