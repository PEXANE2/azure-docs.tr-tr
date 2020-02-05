---
title: VLAN/alt ağlar oluşturma
description: Azure VMware çözümleri (AVS)-AVS özel bulutlarınız için VLAN 'Ları/alt ağları oluşturmayı ve yönetmeyi açıklar ve sonra güvenlik duvarı kurallarını uygular.
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/15/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 249c48500dbcd75f62f856b3345b3a2c02502d1a
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77024781"
---
# <a name="create-and-manage-vlanssubnets-for-your-avs-private-clouds"></a>AVS özel bulutlarınız için VLAN/alt ağlar oluşturun ve yönetin

AVS özel bulutlarınızın VLAN 'Ları/alt ağlarını oluşturmak ve yönetmek için ağ sayfasında VLAN/alt ağlar sekmesini açın. Bir VLAN/subnet oluşturduktan sonra güvenlik duvarı kuralları uygulayabilirsiniz.

## <a name="create-a-vlansubnet"></a>VLAN/subnet oluşturma

1. [AVS portalına erişin](access-cloudsimple-portal.md) ve yan menüdeki **ağ** ' ı seçin.
2. **VLAN/alt ağlar**' ı seçin.
3. **VLAN/subnet oluştur**' a tıklayın.

    ![VLAN/alt ağ sayfası](media/vlan-subnet-page.png)

4. Yeni VLAN/alt ağ için AVS özel bulutunu seçin.
5. Bir VLAN KIMLIĞI girin.
6. Alt ağ adını girin.
7. VLAN (alt ağ) üzerinde yönlendirmeyi etkinleştirmek için, alt ağ CıDR aralığını belirtin. CıDR aralığının, şirket içi alt ağlarınızın, Azure alt ağlarının veya ağ geçidi alt ağının herhangi biriyle çakışmadığından emin olun.
8. **Gönder**'e tıklayın.

    ![VLAN/subnet oluştur](media/create-new-vlan-subnet-details.png)


> [!IMPORTANT]
> Özel bulut başına 30 VLAN kotası vardır. Bu sınırlar, [Destek ile iletişim](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest)kurarak artırılabilir.

## <a name="use-vlan-information-to-set-up-a-distributed-port-group-in-vsphere"></a>VSphere içinde dağıtılmış bir bağlantı noktası grubu ayarlamak için VLAN bilgilerini kullanma

VSphere içinde dağıtılmış bir bağlantı noktası grubu oluşturmak için, <a href="https://docs.vmware.com/en/VMware-vSphere/6.5/vsphere-esxi-vcenter-server-65-networking-guide.pdf" target="_blank">vSphere ağ kılavuzunda</a>' dağıtılmış bağlantı noktası grubu ekleme ' VMware konusundaki yönergeleri izleyin. Dağıtılmış bağlantı noktası grubunu ayarlarken, AVS yapılandırmasından VLAN bilgilerini sağlayın.

![Dağıtılmış bağlantı noktası grubu](media/distributed-port-group.png)

## <a name="select-a-firewall-table"></a>Bir güvenlik duvarı tablosu seçin

Güvenlik Duvarı tabloları ve ilişkili kurallar, **ağ > güvenlik duvarı tabloları** sayfasında tanımlanmıştır. Bir AVS özel bulutu için VLAN/subnet 'e uygulanacak güvenlik duvarı tablosunu seçmek için **VLAN/subnet sayfasında** **güvenlik duvarı tablo eki** ' ne tıklayın. Güvenlik Duvarı tablolarını ayarlama ve kuralları tanımlama hakkında yönergeler için bkz. [güvenlik duvarı tabloları](firewall.md) .

![Güvenlik Duvarı tablosu bağlantısı](media/vlan-subnet-firewall-link.png)

> [!NOTE]
> Bir alt ağ, bir güvenlik duvarı tablosuyla ilişkilendirilebilir. Bir güvenlik duvarı tablosu, birden çok alt ağ ile ilişkilendirilebilir.

## <a name="edit-a-vlansubnet"></a>VLAN/subnet düzenleme

Bir VLAN/subnet ayarlarını düzenlemek için **VLAN/alt ağlar** sayfasında bunu seçin ve **Düzenle** simgesine tıklayın. Değişiklik yapıp **alt karşılandığından**' ye tıklayın.

## <a name="delete-a-vlansubnet"></a>VLAN/subnet silme

Bir VLAN/subnet silmek için **VLAN/alt ağlar** sayfasında bunu seçin ve **Sil** simgesine tıklayın. Onaylamak için **Sil** ' e tıklayın.
