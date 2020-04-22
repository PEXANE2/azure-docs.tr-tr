---
title: Azure Kalesi'nde VM'ler ve NSG'lerle çalışma
description: Bu makalede, Azure Kalesi ile NSG erişiminin nasıl dahil edilebildiğini açıklamaktadır.
services: bastion
author: charwen
ms.service: bastion
ms.topic: conceptual
ms.date: 04/20/2020
ms.author: charwen
ms.openlocfilehash: 0188f9bc1c7c0e8d7fed9f590d078085b175614f
ms.sourcegitcommit: ffc6e4f37233a82fcb14deca0c47f67a7d79ce5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81732191"
---
# <a name="working-with-nsg-access-and-azure-bastion"></a>NSG erişimi ve Azure Bastion ile çalışma

Azure Bastion ile çalışırken ağ güvenlik gruplarını (NSG' ler) kullanabilirsiniz. Daha fazla bilgi için [Güvenlik Grupları'na](../virtual-network/security-overview.md)bakın. 

![Mimari](./media/bastion-nsg/nsg-architecture.png)

Bu diyagramda:

* Bastion ana bilgisayarı sanal ağa dağıtılır.
* Kullanıcı herhangi bir HTML5 tarayıcısını kullanarak Azure portalına bağlanır.
* Kullanıcı RDP/SSH için Azure sanal makineye yönlendirin.
* Connect Tümleştirme - Tarayıcı içinde tek tıklamayla RDP/SSH oturumu
* Azure VM'de ortak IP gerekmez.

## <a name="network-security-groups"></a><a name="nsg"></a>Ağ güvenlik grupları

Bu bölümde, kullanıcı ile Azure Kalesi arasındaki ağ trafiği ve sanal ağınızdaki VM'leri hedeflemek için aşağıdakiler gösterilmektedir:

### <a name="azurebastionsubnet"></a><a name="apply"></a>AzureBastionSubnet

Azure Bastion özellikle ***AzureBastionSubnet'e***dağıtılır.

* **Giriş Trafiği:**

   * **Genel internetten Giriş Trafiği:** Azure Kalesi, giriş trafiği için ortak IP'de etkinleştirilmiş port 443'e ihtiyaç olan genel bir IP oluşturur. Port 3389/22 AzureBastionSubnet'te açılmak zorunda değildir.
   * **Azure Bastion denetim düzleminden Giriş Trafiği:** Denetim düzlemi bağlantısı için **GatewayManager** hizmet etiketinden gelen bağlantı 443'e olanak sağlar. Bu, Ağ Geçidi Yöneticisi'nin Azure Bastion ile konuşabilmesini sağlar.

* **Çıkış Trafiği:**

   * **VM'leri hedeflemek için Çıkış Trafiği:** Azure Bastion, özel IP üzerinden hedef VM'lere ulaşır. NSG'ler, 3389 ve 22 bağlantı noktası için diğer hedef VM alt ağlarına çıkış trafiğine izin vermeli.
   * **Azure'daki diğer ortak uç noktalara çıkış Trafiği:** Azure Bastion'un Azure'daki çeşitli ortak uç noktalara bağlanabilmesi gerekir (örneğin, tanılama günlüklerini ve ölçüm günlüklerini depolamak için). Bu nedenle, Azure Bastion'un 443 **-AzureCloud** hizmet etiketine giden olması gerekir.

### <a name="target-vm-subnet"></a>Hedef VM Subnet
Bu, RDP/SSH istediğiniz hedef sanal makineyi içeren alt ağdır.

   * **Azure Kalesi'nden Giriş Trafiği:** Azure Bastion, özel IP üzerinden hedef VM'ye ulaşır. RDP/SSH bağlantı noktalarının (sırasıyla 3389/22 bağlantı noktaları) hedef VM tarafında özel IP üzerinden açılması gerekir. En iyi uygulama olarak, bu kurala Azure Bastion Subnet IP adresi aralığını ekleyerek, hedef VM alt netindeki hedef VM'lerde bu bağlantı noktalarını yalnızca Bastion'un açabilmesine olanak tanır.


## <a name="next-steps"></a>Sonraki adımlar

Azure Bastion hakkında daha fazla bilgi için [SSS](bastion-faq.md)bölümüne bakın.
