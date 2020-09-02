---
title: Azure savunma | Microsoft Docs
description: RDP/SSH bağlantı noktalarını dışarıdan açığa çıkarmadan sanal makinelerinize güvenli ve sorunsuz RDP/SSH bağlantısı sağlayan Azure savunma hakkında bilgi edinin.
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: overview
ms.date: 09/01/2020
ms.author: cherylmc
ms.openlocfilehash: 680eb8414696109e8cc15d82d8bd5f2c5740f488
ms.sourcegitcommit: 58d3b3314df4ba3cabd4d4a6016b22fa5264f05a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89291883"
---
# <a name="what-is-azure-bastion"></a>Azure Bastion nedir?

Azure savunma hizmeti, sanal ağınızın içinde sağladığınız yeni, platform tarafından yönetilen yeni bir PaaS hizmetidir. Doğrudan TLS üzerinden Azure portal sanal makinelerinize güvenli ve sorunsuz RDP/SSH bağlantısı sağlar. Azure Bastion aracılığıyla bağlandığınızda, sanal makinelerinizin bir genel IP adresi olması gerekmez.

Savunma, sağlandığı sanal ağdaki tüm VM 'lere güvenli RDP ve SSH bağlantısı sağlar. Azure savunma 'nın kullanılması, sanal makinelerinizin, RDP/SSH bağlantı noktalarını dış dünyaya sunulmasını sağlarken RDP/SSH kullanarak güvenli erişim sağlamaya devam eder. Azure savunma sayesinde, sanal makineye doğrudan Azure portal bağlanırsınız. Ek bir istemci, aracı veya yazılım parçası gerekmez.

## <a name="architecture"></a>Mimari

Azure savunma dağıtımı, abonelik/hesap veya sanal makine başına değil, sanal ağ başına değildir. Sanal ağınızda bir Azure savunma hizmeti sağladığınızda, RDP/SSH deneyimi aynı sanal ağdaki tüm VM 'leriniz için kullanılabilir.

RDP ve SSH, Azure 'da çalışan iş yüklerinize bağlanmak için kullanabileceğiniz temel yollardan biridir. RDP/SSH bağlantı noktalarının Internet üzerinden kullanıma sunulması istenmez ve önemli bir tehdit yüzeyi olarak görülür. Bunun nedeni genellikle protokol güvenlik açıklarına göre yapılır. Bu tehdit yüzeyini içermesi için, çevre ağınızın genel tarafında savunma Konakları (doğrudan geçiş sunucuları olarak da bilinir) dağıtabilirsiniz. Savunma ana bilgisayar sunucuları tasarlanmakta ve bağımsız saldırılara yönelik olarak yapılandırılmıştır. Savunma sunucuları Ayrıca, savunma ve ağ içinde oturmuş iş yükleri için RDP ve SSH bağlantısı sağlar.

![Azure savunma mimarisi](./media/bastion-overview/architecture.png)

Bu şekilde, bir Azure savunma dağıtımının mimarisi gösterilmektedir. Bu diyagramda:

* Savunma ana bilgisayarı sanal ağda dağıtılır.
* Kullanıcı herhangi bir HTML5 tarayıcısı kullanarak Azure portal bağlanır.
* Kullanıcı, bağlanılacak sanal makineyi seçer.
* Tek bir tıklama ile, RDP/SSH oturumu tarayıcıda açılır.
* Azure VM 'de genel IP gerekli değildir.

## <a name="key-features"></a>Önemli özellikler

Aşağıdaki özellikler mevcuttur:

* **RDP ve SSH doğrudan Azure Portal:** Yalnızca tek tıklamayla sorunsuz bir deneyim kullanarak doğrudan Azure portal RDP ve SSH oturumuna doğrudan ulaşabilirsiniz.
* **RDP/SSH IÇIN TLS üzerinden uzak oturum ve güvenlik duvarı geçişi:** Azure savunma, yerel cihazınıza otomatik olarak akan bir HTML5 tabanlı Web istemcisi kullanır. böylece, 443 numaralı bağlantı noktasında RDP/SSH oturumunuzu güvenli bir şekilde çapraz bir şekilde gezme olanağı sağlar.
* **Azure VM 'de genel IP gerekmez:** Azure savunma, sanal makinelinizdeki özel IP kullanarak Azure sanal makinenize yönelik RDP/SSH bağlantısını açar. Sanal makinenizde genel IP 'niz olması gerekmez.
* **NSG 'leri yönetmenin bir sorun yoktur:** Azure savunma, Azure 'dan gelen ve RDP/SSH bağlantısı sağlayan güvenli bir platform PaaS hizmetidir. Azure savunma alt ağında herhangi bir NSG uygulamanız gerekmez. Azure savunma, sanal makinelerinize özel IP üzerinden bağlandığından, NSG 'lerinizi yalnızca Azure 'dan gelen RDP/SSH 'ye izin verecek şekilde yapılandırabilirsiniz. Bu, sanal makinelerinize güvenli bir şekilde bağlanmak için gereken her seferinde NSG 'leri yönetmenin ne kadar olduğunu ortadan kaldırır.
* **Bağlantı noktası taramaya karşı koruma:** Sanal makinelerinizi ortak Internet 'e sunmak zorunda olmadığınızdan, VM 'niz, sanal ağınızın dışında bulunan standart dışı ve kötü amaçlı kullanıcılar tarafından bağlantı noktası taramaya karşı korunur.
* **Sıfır günlük saldırılara karşı koruma. Yalnızca bir yerde sağlamlaştırma:** Azure savunma, tam platform tarafından yönetilen bir PaaS hizmetidir. Sanal ağınızın çevre tarafında bulunduğundan, sanal ağınızdaki sanal makinelerin her birini sağlamlaştırma konusunda endişelenmeniz gerekmez. Azure platformu, sizin için Azure savunma ve sürekli güncel tutarak, sıfır günlük güvenlik açıklarından yararlanmaya karşı koruma sağlar.

## <a name="whats-new"></a><a name="new"></a>Yenilikler

RSS akışına abone olun ve [Azure Updates](https://azure.microsoft.com/updates/?category=networking&query=Azure%20Bastion) sayfasında en son Azure savunma özelliği güncelleştirmelerini görüntüleyin.

## <a name="faq"></a>SSS

[!INCLUDE [Bastion FAQ](../../includes/bastion-faq-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [Bir Azure savunma konak kaynağı oluşturun](bastion-create-host-portal.md).
* Azure'un diğer önemli [ağ özelliklerinden](../networking/networking-overview.md) bazıları hakkında bilgi edinin.
