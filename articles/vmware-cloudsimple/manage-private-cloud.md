---
title: Azure VMware çözümlerini (AVS) özel bulutunu yönetme
description: AVS özel bulut kaynaklarınızı ve etkinliğinizi yönetmek için kullanılabilen özellikleri açıklar
author: sharaths-cs
ms.author: b-shsury
ms.date: 06/10/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 47bf2251f71204b99245c1a9d55ef87157c41dd8
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77014836"
---
# <a name="manage-avs-private-cloud-resources-and-activities"></a>AVS özel bulut kaynaklarını ve etkinliklerini yönetin

AVS özel bulutlar, AVS portalından yönetilir. Durumu, kullanılabilir kaynakları, AVS özel bulutundaki etkinliği ve AVS portalından diğer ayarları kontrol edin.

## <a name="sign-in-to-azure"></a>Azure'da oturum açın

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="access-the-avs-portal"></a>AVS portalına erişme

[AVS portalına](access-cloudsimple-portal.md)erişin.

## <a name="view-the-list-of-avs-private-clouds"></a>AVS özel bulutlarının listesini görüntüleyin

**Kaynaklar** sayfasındaki **AVS özel bulutlar** sekmesi, aboneliğinizdeki tüm AVS özel bulutlarını listeler. Bilgiler, vSphere kümelerinin adını, konumunu, AVS özel bulutunun geçerli durumunu ve kaynak bilgilerini içerir.

![AVS özel bulut sayfası](media/manage-private-cloud.png)

Ek bilgi ve eylemler için bir AVS özel bulutu seçin.

## <a name="avs-private-cloud-summary"></a>AVS özel bulut Özeti

Seçili AVS özel bulutunun kapsamlı bir özetini görüntüleyin. Özet sayfası, AVS özel bulutu 'nda dağıtılan DNS sunucularını içerir. Şirket içi DNS sunucularından DNS iletmeyi, AVS özel bulut DNS sunucularınıza ayarlayabilirsiniz. DNS iletimi hakkında daha fazla bilgi için bkz. [Şirket IÇINDEN AVS özel bulut vCenter için ad çözümlemesi IÇIN DNS 'ı yapılandırma](https://docs.azure.cloudsimple.com/on-premises-dns-setup/).

![AVS özel bulut Özeti](media/private-cloud-summary.png)

### <a name="available-actions"></a>Kullanılabilir eylemler

* [VSphere Istemcisini başlatın](https://docs.azure.cloudsimple.com/vsphere-access/). Bu AVS özel bulutu için vCenter 'a erişin.
* [Düğümleri satın alın](create-nodes.md). Bu AVS özel bulutuna düğümler ekleyin.
* [Öğesini genişletin](expand-private-cloud.md). Bu AVS özel bulutuna düğümler ekleyin.
* **Yenileyin**. Bu sayfadaki bilgileri güncelleştirin.
* **Silin**. Her zaman AVS özel bulutunu silebilirsiniz. **Silmeden önce tüm sistemleri ve verileri yedeklediğinizden emin olun.** Bir AVS özel bulutu silindiğinde tüm VM 'Ler, vCenter yapılandırması ve veriler silinir. Seçili AVS özel bulutu için Özet bölümünde **Sil** ' e tıklayın. Silme işlemi sonrasında, tüm AVS özel bulut verileri güvenli, yüksek oranda uyumlu bir işlem sürecinde silinir.
* [VSphere ayrıcalıklarını değiştirin](escalate-private-cloud-privileges.md). Bu AVS özel bulutundaki ayrıcalıklarınızı ilerletin.

## <a name="avs-private-cloud-vlanssubnets"></a>AVS özel bulutu VLAN 'LARı/alt ağları

Seçili AVS özel bulutu için tanımlı VLAN/alt ağların listesini görüntüleyin. Listede, AVS özel bulutu oluşturulduğunda oluşturulan yönetim VLAN 'Ları/alt ağları bulunur.

![AVS özel bulutu-VLAN 'Lar/alt ağlar](media/private-cloud-vlans-subnets.png) 

### <a name="available-actions"></a>Kullanılabilir eylemler

* [VLAN/alt ağlar ekleyin](https://docs.azure.cloudsimple.com/create-vlan-subnet/). Bu AVS özel bulutuna bir VLAN/alt küme ekleyin.

Aşağıdaki eylemler için bir VLAN/subnet seçin
* [Güvenlik duvarı tablosu ekleyin](https://docs.azure.cloudsimple.com/firewall/). Bu AVS özel bulutuna bir güvenlik duvarı tablosu ekleyin.
* **Düzenle**
* **Sil** (yalnızca Kullanıcı tanımlı VLAN/alt ağlar)

## <a name="avs-private-cloud-activity"></a>AVS özel bulut etkinliği

Seçili AVS özel bulutu için aşağıdaki bilgileri görüntüleyin. Etkinlik bilgileri, seçilen AVS özel bulutunun tüm etkinliklerinin filtrelenmiş bir listesidir. Bu sayfada en fazla 25 son etkinlik gösterilmektedir.

* Son Uyarılar
* Son olaylar
* Son görevler
* Son denetim

![AVS özel bulutu-etkinlik](media/private-cloud-activity.png)

## <a name="cloud-racks"></a>Bulut rafları

Bulut raflar, AVS özel bulutunuzun yapı taşlarıdır. Her raf bir kapasite birimi sağlar. AVS, bir AVS özel bulutu oluştururken veya genişletirken seçimlerinize göre bulut raflarını otomatik olarak yapılandırır. Her birinin atandığı AVS özel bulutu da dahil olmak üzere bulut raflarının tam listesini görüntüleyin.

![AVS özel bulutu-bulut rafları](media/private-cloud-cloudracks.png)

## <a name="vsphere-management-network"></a>vSphere yönetim ağı

Şu anda AVS özel bulutu üzerinde yapılandırılmış olan VMware yönetim kaynakları ve sanal makinelerin listesi. Bilgiler yazılım sürümünü, tam etki alanı adını (FQDN) ve kaynakların IP adresini içerir.

![AVS özel bulutu-vSphere yönetim ağı](media/private-cloud-vsphere-management-network.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure 'da VMware VM 'lerini kullanma](quickstart-create-vmware-virtual-machine.md)
* [AVS özel bulutları](cloudsimple-private-cloud.md) hakkında daha fazla bilgi edinin