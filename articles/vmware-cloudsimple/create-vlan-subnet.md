---
title: VN'ler/alt ağlar oluşturun - CloudSimple'a göre Azure VMware Çözümü
description: CloudSimple'a göre Azure VMware Çözümleri - Özel Bulutlarınız için VLAN'ların/alt ağların nasıl oluşturulup yönetilenve ardından güvenlik duvarı kurallarını nasıl uygulayacağınızı açıklar.
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/15/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: c0160513eb9abca54adbc3819b982348dc202c90
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77566004"
---
# <a name="create-and-manage-vlanssubnets-for-your-private-clouds"></a>Özel Bulutlarınız için V'ler/alt ağlar oluşturun ve yönetin

Özel Bulutlarınız için V'ler/alt ağlar oluşturmak ve yönetmek için Ağ sayfasındaki VNET'ler/Alt Ağlar sekmesini açın. Bir VLAN/alt ağ oluşturduktan sonra güvenlik duvarı kuralları uygulayabilirsiniz.

## <a name="create-a-vlansubnet"></a>VLAN/alt ağ oluşturma

1. [CloudSimple portalına erişin](access-cloudsimple-portal.md) ve yan menüde **Ağ'ı** seçin.
2. **V'ler/alt ağlar**seçin.
3. **VLAN/Subnet Oluştur'u**tıklatın.

    ![VLAN/alt ağ sayfası](media/vlan-subnet-page.png)

4. Yeni VLAN/alt net için Özel Bulut'u seçin.
5. Bir VLAN kimliği girin.
6. Alt ağ adını girin.
7. VLAN(subnet) üzerinde yönlendirmeyi etkinleştirmek için, alt ağ CIDR aralığını belirtin. CIDR aralığının şirket içi alt ağlarınız, Azure alt ağlarınız veya ağ geçidi alt ağınızla örtüşmediğinden emin olun.
8. **Gönder'i**tıklatın.

    ![VLAN/subnet oluşturma](media/create-new-vlan-subnet-details.png)


> [!IMPORTANT]
> Özel bulut başına 30 VLA'lık bir kota vardır. Bu limitler [desteğe başvurarak](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest)artırılabilir.

## <a name="use-vlan-information-to-set-up-a-distributed-port-group-in-vsphere"></a>vSphere'de dağıtılmış bir bağlantı noktası grubu kurmak için VLAN bilgilerini kullanma

vSphere'de dağıtılmış bir bağlantı noktası grubu oluşturmak için <a href="https://docs.vmware.com/en/VMware-vSphere/6.5/vsphere-esxi-vcenter-server-65-networking-guide.pdf" target="_blank">vSphere Ağ Kılavuzu'ndaki</a>VMware konusu 'Dağıtılmış bağlantı noktası grubu ekle' yönergelerini izleyin. Dağıtılmış bağlantı noktası grubunu ayarlarken, CloudSimple yapılandırmasından VLAN bilgilerini sağlayın.

![Dağıtılmış Bağlantı Noktası Grubu](media/distributed-port-group.png)

## <a name="select-a-firewall-table"></a>Güvenlik duvarı tablosu nu seçin

Güvenlik duvarı tabloları ve ilişkili kurallar **Ağ > Güvenlik Duvarı tabloları** sayfasında tanımlanır. Özel Bulut için VLAN/alt ağına uygulanacak güvenlik duvarı tablosunu seçmek için **VLAN/Subnet** sayfasındaKi **Güvenlik Duvarı tablosu eki'ni** tıklatın. Güvenlik duvarı tabloları oluşturma ve kuralları tanımlama yla ilgili talimatlar için [Güvenlik Duvarı Tabloları'na](firewall.md) bakın.

![Güvenlik duvarı tablosu bağlantısı](media/vlan-subnet-firewall-link.png)

> [!NOTE]
> Bir alt ağ tek bir güvenlik duvarı tablosuyla ilişkilendirilebilir. Güvenlik duvarı tablosu birden çok alt ağla ilişkilendirilebilir.

## <a name="edit-a-vlansubnet"></a>VLAN/alt ağı edin

Bir VLAN/Subnet ayarlarını yeniden yapmak için **VLANs/Subnets** sayfasında n'leri seçin ve **Düzelt** simgesini tıklatın. Değişiklik yapın ve **Submet'i**tıklatın.

## <a name="delete-a-vlansubnet"></a>VLAN/alt net silme

Bir VLAN/Subnet'i silmek için **VLANs/Subnets** sayfasında seçin ve **Sil** simgesine tıklayın. Onaylamak için **Sil'i** tıklatın.
