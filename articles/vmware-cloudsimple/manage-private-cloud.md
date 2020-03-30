---
title: Azure VMware Çözümlerini CloudSimple Private Cloud ile yönetin
description: CloudSimple Private Cloud kaynaklarınızı ve etkinliğinizi yönetmek için kullanılabilen yetenekleri açıklar
author: sharaths-cs
ms.author: b-shsury
ms.date: 06/10/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 620c0226d3aca907352658ebbe1b94c7673d91cd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77014836"
---
# <a name="manage-private-cloud-resources-and-activity"></a>Özel Bulut kaynaklarını ve etkinliğini yönetme

Özel bulutlar CloudSimple portalından yönetilir.  CloudSimple portalındaki durumu, kullanılabilir kaynakları, özel buluttaki etkinliği ve diğer ayarları denetleyin.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Azure portalında oturum [https://portal.azure.com](https://portal.azure.com)aç.

## <a name="access-the-cloudsimple-portal"></a>CloudSimple portalına erişim

[CloudSimple portalına](access-cloudsimple-portal.md)erişin.

## <a name="view-the-list-of-private-clouds"></a>Özel Bulutlar Listesini Görüntüle

**Kaynaklar** sayfasındaki **Özel Bulutlar** sekmesi aboneliğinizdeki tüm Özel Bulutları listeler. Bilgiler, vSphere kümelerinin adını, sayısını, konumunu, özel bulutun geçerli durumunu ve kaynak bilgilerini içerir.

![Özel Bulut sayfası](media/manage-private-cloud.png)

Ek bilgi ve eylemler için bir Özel Bulut seçin.

## <a name="private-cloud-summary"></a>Özel Bulut özeti

Seçili Özel Bulut'un kapsamlı bir özetini görüntüleyin.  Özet sayfası, Özel Bulut'ta dağıtılan DNS sunucularını içerir.  Şirket içi DNS sunucularından Özel Bulut DNS sunucularınıza DNS yönlendirmeayarlayabilirsiniz.  DNS iletme hakkında daha fazla bilgi için, [şirket için Özel Bulut vCenter için ad çözümü için DNS'yi yapılandırın'](https://docs.azure.cloudsimple.com/on-premises-dns-setup/)a bakın.

![Özel Bulut Özeti](media/private-cloud-summary.png)

### <a name="available-actions"></a>Kullanılabilir eylemler

* [Başlat vSphere istemcisi](https://docs.azure.cloudsimple.com/vsphere-access/). Bu Özel Bulut için vCenter'a erişin.
* [Satın alma düğümleri](create-nodes.md). Bu Özel Bulut'a düğüm ekleyin.
* [Genişletin.](expand-private-cloud.md) Bu Özel Bulut'a düğüm ekleyin.
* **Yenileyin.** Bu sayfadaki bilgileri güncelleştirin.
* **Sil.** Özel Bulut'u istediğiniz zaman silebilirsiniz. **Silmeden önce, tüm sistemleri ve verileri yedeklediğinizden emin olun.** Özel Bulut'u silmek tüm VM'leri, vCenter yapılandırmasını ve verileri siler. Seçili Özel Bulut'un özet bölümünde **Sil'i** tıklatın. Silme işleminden sonra, tüm Özel Bulut verileri güvenli ve son derece uyumlu bir silme işlemiyle silinir.
* [vSphere ayrıcalıklarını değiştirin.](escalate-private-cloud-privileges.md)  Bu Özel Bulut'taki ayrıcalıklarınızı artırın.

## <a name="private-cloud-vlanssubnets"></a>Özel Bulut VLANS/alt ağları

Seçili Özel Bulut için tanımlı VLAN'ların/alt ağların listesini görüntüleyin.  Liste, özel bulut oluşturulduğunda oluşturulan yönetim VLAN'larını/alt ağlarını içerir.

![Özel Bulut - VLANs/Subnets](media/private-cloud-vlans-subnets.png) 

### <a name="available-actions"></a>Kullanılabilir eylemler

* [VLANS/Subnets ekleyin.](https://docs.azure.cloudsimple.com/create-vlan-subnet/) Bu Özel Bulut'a bir VLAN/alt kümesi ekleyin.

Aşağıdaki eylemler için bir VLAN/Subnet seçin
* [Güvenlik duvarı tablosunu takın.](https://docs.azure.cloudsimple.com/firewall/) Bu Özel Bulut'a bir güvenlik duvarı tablosu takın.
* **Düzenle**
* **Silme** (yalnızca kullanıcı tanımlı VLAN'lar/Alt ağlar)

## <a name="private-cloud-activity"></a>Özel Bulut etkinliği

Seçili Özel Bulut için aşağıdaki bilgileri görüntüleyin.  Etkinlik bilgileri, seçili Özel Bulut için tüm etkinliklerin filtrelenmiş bir listesidir.  Bu sayfa, en fazla 25 yeni etkinlik gösterir.

* Son uyarılar
* Son olaylar
* Son görevler
* Son denetim

![Özel Bulut - Etkinlik](media/private-cloud-activity.png)

## <a name="cloud-racks"></a>Bulut Rafları

Bulut rafları, Özel Bulut'unuzun yapı taşlarıdır. Her raf bir kapasite birimi sağlar. CloudSimple, Bir Özel Bulut oluştururken veya genişletirken bulut raflarını seçimlerinize göre otomatik olarak yapılandırır.  Her birinin atandığı Özel Bulut da dahil olmak üzere bulut raflarının tam listesini görüntüleyin.

![Özel Bulut - Bulut Rafları](media/private-cloud-cloudracks.png)

## <a name="vsphere-management-network"></a>vSphere Yönetim Ağı

Şu anda Özel Bulut'ta yapılandırılan VMware yönetim kaynakları nın ve sanal makinelerin listesi. Bilgiler, yazılım sürümünü, tam nitelikli alan adını (FQDN) ve kaynakların IP adresini içerir.

![Özel Bulut - vSphere Yönetim Ağı](media/private-cloud-vsphere-management-network.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure'da VMware sanal makinelerini kullanma](quickstart-create-vmware-virtual-machine.md)
* [Özel Bulutlar](cloudsimple-private-cloud.md) hakkında daha fazla bilgi edinin