---
title: Azure 'da VM 'Ler ve NSG 'lerle çalışma | Microsoft Docs
description: Bu makalede Azure savunma ile NSG erişimini birleştirme işlemi açıklanır
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: conceptual
ms.date: 09/30/2019
ms.author: cherylmc
ms.openlocfilehash: 4f99b24435998fc4d0c7ab724c66a318586a80d4
ms.sourcegitcommit: 8bae7afb0011a98e82cbd76c50bc9f08be9ebe06
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/01/2019
ms.locfileid: "71694942"
---
# <a name="working-with-nsg-access-and-azure-bastion-preview"></a>NSG erişimiyle çalışma ve Azure savunma (Önizleme)

Azure savunma ile çalışırken ağ güvenlik gruplarını (NSG 'ler) kullanabilirsiniz. Daha fazla bilgi için bkz. [güvenlik grupları](../virtual-network/security-overview.md). 

> [!IMPORTANT]
> Bu genel önizleme bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılmamalıdır. Belirli özellikler desteklenmiyor olabilir, kısıtlı yeteneklere sahip olabilir veya tüm Azure konumlarında mevcut olmayabilir. Ayrıntılar için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>

![Mimari](./media/bastion-nsg/nsg_architecture.png)

Bu diyagramda:

* Savunma ana bilgisayarı sanal ağda dağıtılır.
* Kullanıcı herhangi bir HTML5 tarayıcısı kullanarak Azure portal bağlanır.
* Kullanıcı, bağlanılacak sanal makineyi seçer.
* Tek bir tıklama ile, RDP/SSH oturumu tarayıcıda açılır.
* Azure VM 'de genel IP gerekli değildir.

## <a name="nsg"></a>Ağ güvenlik grupları

* **AzureBastionSubnet:** Azure savunma, belirli AzureBastionSubnet dağıtılır.  
    * **Genel İnternet 'ten gelen trafik:** Azure savunma, giriş trafiği için genel IP üzerinde bağlantı noktası 443 ' ün etkin olmasını gerektiren bir genel IP oluşturur. 3389/22 numaralı bağlantı noktası, AzureBastionSubnet açık olması gerekmez.
    * **Hedef VM 'Lere giden çıkış trafiği:** Azure savunma, hedef VM 'Lere özel IP üzerinden ulaşacaktır. NSG 'ler, diğer hedef VM alt ağlarına giden trafiğe izin vermek için gerekir.
* **Hedef VM alt ağı:** Bu, RDP/SSH yapmak istediğiniz hedef sanal makineyi içeren alt ağıdır.
    * **Azure 'dan giriş trafiği:** Azure savunma, hedef VM 'ye özel IP üzerinden ulaşacaktır. RDP/SSH bağlantı noktaları (sırasıyla bağlantı noktaları 3389 ve 22), özel IP üzerinden hedef VM tarafında açılmalıdır.

## <a name="apply"></a>NSG 'leri AzureBastionSubnet 'e Uygula

**AzureBastionSubnet**'e NSG 'ler uygularsanız, Azure denetim düzlemi ve altyapısı için aşağıdaki iki hizmet etiketine izin verin:

* **Gatewaymanager (yalnızca Kaynak Yöneticisi)** : Bu etiket, Azure Gateway Manager hizmetinin adres öneklerini gösterir. Değer için GatewayManager belirtirseniz, GatewayManager 'a trafiğe izin verilir veya erişim engellenir.  AzureBastionSubnet üzerinde NSG 'ler oluşturuyorsanız, gelen trafik için GatewayManager etiketini etkinleştirin.

* **Azurecyüksek (yalnızca Kaynak Yöneticisi)** : Bu etiket, tüm veri MERKEZI genel IP adresleri dahil olmak üzere Azure için IP adresi alanını belirtir. Değer için Azurecı belirtirseniz, trafiğin Azure genel IP adresleri için izin verilir veya reddedilir. Belirli bir bölgedeki yalnızca Azurecı 'ye erişime izin vermek istiyorsanız bölgeyi belirtebilirsiniz. Örneğin, Doğu ABD bölgesinde yalnızca Azure azurecı 'ye erişime izin vermek istiyorsanız, bir hizmet etiketi olarak Azurecyüksek. EastUS belirtebilirsiniz. AzureBastionSubnet üzerinde NSG 'ler oluşturuyorsanız, giden trafik için Azurecfi etiketini etkinleştirin. Internet 'e gelen bağlantı noktası 443 ' ı açarsanız, gelen trafik için Azurecfi etiketini etkinleştirmeniz gerekmez.

## <a name="next-steps"></a>Sonraki adımlar

Azure savunma hakkında daha fazla bilgi için bkz. [SSS](bastion-faq.md)
