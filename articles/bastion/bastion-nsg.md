---
title: Azure 'da VM 'Ler ve NSG 'lerle çalışma
description: Bu makalede Azure savunma ile NSG erişimini birleştirme işlemi açıklanır
services: bastion
author: charwen
ms.service: bastion
ms.topic: conceptual
ms.date: 04/20/2020
ms.author: charwen
ms.openlocfilehash: e4782213b38ad9e265cc66c3073dc5f357c50561
ms.sourcegitcommit: 01cd19edb099d654198a6930cebd61cae9cb685b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/24/2020
ms.locfileid: "85321660"
---
# <a name="working-with-nsg-access-and-azure-bastion"></a>NSG erişimiyle ve Azure ile çalışma

Azure savunma ile çalışırken ağ güvenlik gruplarını (NSG 'ler) kullanabilirsiniz. Daha fazla bilgi için bkz. [güvenlik grupları](../virtual-network/security-overview.md).

:::image type="content" source="./media/bastion-nsg/figure-1.png" alt-text="NSG":::

Bu diyagramda:

* Savunma ana bilgisayarı sanal ağa dağıtılır.
* Kullanıcı herhangi bir HTML5 tarayıcısı kullanarak Azure portal bağlanır.
* Kullanıcı Azure sanal makinesine RDP/SSH 'ye gider.
* Tümleştirme-tarayıcı içinde tek tıklamayla RDP/SSH oturumu bağlayın
* Azure VM 'de genel IP gerekli değildir.

## <a name="network-security-groups"></a><a name="nsg"></a>Ağ güvenlik grupları

Bu bölümde, Kullanıcı ve Azure savunma arasındaki ağ trafiği ve sanal ağınızdaki VM 'Leri hedeflemek için:

### <a name="azurebastionsubnet"></a><a name="apply"></a>AzureBastionSubnet

Azure savunma, özellikle ***AzureBastionSubnet***'e dağıtılır.

* **Giriş trafiği:**

   * **Genel İnternet 'ten gelen trafik:** Azure savunma, giriş trafiği için genel IP üzerinde bağlantı noktası 443 ' ün etkin olmasını gerektiren bir genel IP oluşturur. 3389/22 numaralı bağlantı noktası, AzureBastionSubnet açık olması gerekmez.
   * **Azure savunma denetim düzleminin giriş trafiği:** Denetim düzlemi bağlantısı için **Gatewaymanager** hizmet etiketinden gelen bağlantı noktası 443 ' yı etkinleştirin. Bu, denetim düzlemi, diğer bir deyişle, ağ geçidi yöneticisinin Azure ile iletişim kurabilmesine olanak sağlar.

* **Çıkış trafiği:**

   * **Hedef VM 'Lere giden çıkış trafiği:** Azure savunma, hedef VM 'Lere özel IP üzerinden ulaşacaktır. NSG 'ler, bağlantı noktası 3389 ve 22 için diğer hedef VM alt ağlarına giden trafiğe izin vermek için gerekir.
   * **Azure 'da diğer genel uç noktalara giden trafik çıkışı:** Azure savunma 'nın Azure 'daki çeşitli genel uç noktalara bağlanabililmesi gerekir (örneğin, tanılama günlüklerini ve ölçüm günlüklerini depolamak için). Bu nedenle, Azure savunma 'nın 443 'e giden ve **Azurecı** hizmeti etiketi için çıkış yapması gerekir.

### <a name="target-vm-subnet"></a>Hedef VM alt ağı
Bu, RDP/SSH yapmak istediğiniz hedef sanal makineyi içeren alt ağıdır.

   * **Azure 'dan giriş trafiği:** Azure savunma, hedef VM 'ye özel IP üzerinden ulaşacaktır. RDP/SSH bağlantı noktaları (sırasıyla 3389/22), özel IP üzerinden hedef VM tarafında açılmalıdır. En iyi uygulama olarak, bu bağlantı noktalarını hedef VM alt ağınızdaki hedef VM 'lerde açabiliyor olması için Azure savunma alt ağı IP adresi aralığını bu kurala ekleyebilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar

Azure savunma hakkında daha fazla bilgi için bkz. [SSS](bastion-faq.md).
