---
title: Azure Kalesi | Microsoft Dokümanlar
description: Azure Kalesi hakkında bilgi edinin
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: overview
ms.date: 01/31/2020
ms.author: cherylmc
ms.openlocfilehash: 299a69675eed1ba958c6d13cf447407450df2abb
ms.sourcegitcommit: 27bbda320225c2c2a43ac370b604432679a6a7c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80411116"
---
# <a name="what-is-azure-bastion"></a>Azure Bastion nedir?

Azure Bastion hizmeti, sanal ağınızda sağladığınız tam platform tarafından yönetilen yeni bir PaaS hizmetidir. TLS üzerinden doğrudan Azure portalında sanal makinelerinize güvenli ve sorunsuz RDP/SSH bağlantısı sağlar. Azure Bastion aracılığıyla bağlandığınızda, sanal makinelerinizin bir genel IP adresi olması gerekmez.

Bastion, sağlandığı sanal ağdaki tüm VM'lere güvenli RDP ve SSH bağlantısı sağlar. Azure Bastion'u kullanmak, sanal makinelerinizi RDP/SSH bağlantı noktalarını dış dünyaya maruz bırakmaktan korurken, RDP/SSH kullanarak güvenli erişim sağlar. Azure Bastion ile sanal makineye doğrudan Azure portalından bağlanırsınız. Ek bir istemciye, aracıya veya yazılım parçasına ihtiyacınız yoktur.

## <a name="architecture"></a>Mimari

Azure Bastion dağıtımı, abonelik/hesap veya sanal makine başına değil, sanal ağ başınadır. Sanal ağınızda bir Azure Bastion hizmeti sağladıktan sonra, RDP/SSH deneyimi tüm Sanal Ağlarınız tarafından kullanılabilir.

RDP ve SSH, Azure'da çalışan iş yüklerinize bağlanabileceğiniz temel araçlardan bazılarıdır. RDP/SSH bağlantı noktalarını Internet üzerinden teşhir etmek istenmez ve önemli bir tehdit yüzeyi olarak görülür. Bunun nedeni genellikle protokol açıklarıdır. Bu tehdit yüzeyini içermek için, çevre ağınızın ortak tarafında burç ana bilgisayarlarını (atlama sunucuları olarak da bilinir) dağıtabilirsiniz. Burç ana bilgisayar sunucuları saldırılara dayanacak şekilde tasarlanır ve yapılandırılır. Burç sunucuları ayrıca, kalenin arkasında oturan iş yüklerine ve ağ içinde daha fazlasına RDP ve SSH bağlantısı sağlar.

![architecture](./media/bastion-overview/architecture.png)

Bu şekil, Bir Azure Bastion dağıtımının mimarisini gösterir. Bu diyagramda:

* Bastion ana bilgisayarı sanal ağda dağıtılır.
* Kullanıcı herhangi bir HTML5 tarayıcısını kullanarak Azure portalına bağlanır.
* Kullanıcı bağlanmak için sanal makineyi seçer.
* Tek bir tıklamayla tarayıcıda RDP/SSH oturumu açılır.
* Azure VM'de ortak IP gerekmez.

## <a name="key-features"></a>Önemli özellikler

Aşağıdaki özellikler mevcuttur:

* **RDP ve SSH doğrudan Azure portalında:** Tek bir tıklama sorunsuz deneyimi kullanarak doğrudan AZURE portalında RDP ve SSH oturumuna ulaşabilirsiniz.
* **RDP/SSH için TLS ve güvenlik duvarı geçişi üzerinden Uzak Oturum:** Azure Bastion, yerel cihazınıza otomatik olarak aktarılan HTML5 tabanlı bir web istemcisi kullanır, böylece rdp/SSH oturumunuzu 443 bağlantı noktasında TLS üzerinden alır ve kurumsal güvenlik duvarlarını güvenli bir şekilde geçiş yapmanızı sağlar.
* **Azure VM'de Ortak IP gerekmez:** Azure Bastion, VM'nizde özel IP kullanarak Azure sanal makinenize RDP/SSH bağlantısını açar. Sanal makinenizde halka açık bir IP'ye ihtiyacınız yoktur.
* **NSG'leri yönetme nin güçlük dolmaması:** Azure Bastion, azure'un güvenli RDP/SSH bağlantısı sağlamak için dahili olarak sertleştirilmiş tam olarak yönetilen bir platform PaaS hizmetidir. Azure Bastion alt ağına nsg uygulamanız gerekmez. Azure Kalesi özel IP üzerinden sanal makinelerinize bağlandığından, NSG'lerinizi yalnızca Azure Kalesi'nden RDP/SSH'ye izin verecek şekilde yapılandırabilirsiniz. Bu, sanal makinelerinize güvenli bir şekilde bağlanmanız gerektiğinde NSG'leri yönetme zorunu ortadan kaldırır.
* **Bağlantı noktası taramasına karşı koruma:** Sanal makinelerinizi herkese açık Internet'e maruz bırakmanız gerekmedığından, SANAL M'leriniz sanal ağınızın dışında bulunan sahte kardes ve kötü niyetli kullanıcılar tarafından bağlantı noktası taramasına karşı korunur.
* **Sıfır gün açıklarına karşı koruyun. Yalnızca tek bir yerde sertleştirme:** Azure Bastion tamamen platform tarafından yönetilen bir PaaS hizmetidir. Sanal ağınızın çevresinde yer aldığı için, sanal ağınızdaki sanal makinelerin her birini sertleştirme konusunda endişelenmenize gerek yoktur. Azure platformu, Azure Kalesi'ni sertve her zaman sizin için güncel tutarak sıfır gün açıklarına karşı koruma da sunar.

## <a name="faq"></a>SSS

[!INCLUDE [Bastion FAQ](../../includes/bastion-faq-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Bastion ana bilgisayar kaynağı oluşturun.](bastion-create-host-portal.md)
* Azure'un diğer önemli [ağ özelliklerinden](../networking/networking-overview.md) bazıları hakkında bilgi edinin.
