---
title: Azure VMware Solutions (AVS)-Access vSphere Client
description: AVS özel bulutunuzun vCenter 'a nasıl erişebileceğinizi açıklar.
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/30/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: ad018ea89b194d42ab1867a0569725c4c3680f7d
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77022673"
---
# <a name="access-your-avs-private-cloud-vcenter-portal"></a>AVS özel bulut vCenter portala erişin

AVS özel bulut vCenter portalınızı Azure portal veya AVS portalından başlatabilirsiniz. vCenter portalı, AVS özel bulutunuzda VMware altyapısını yönetmenizi sağlar.

## <a name="before-you-begin"></a>Başlamadan önce

VCenter portalına erişmek için ağ bağlantısı kurulması ve DNS ad çözümlemesinin etkinleştirilmesi gerekir. Aşağıdaki seçeneklerden herhangi birini kullanarak, AVS özel bulutunuz için ağ bağlantısı kurabilirsiniz.

* [ExpressRoute kullanarak şirket içinden AVS 'ye bağlanma](on-premises-connection.md)
* [AVS özel bulutunuz için bir VPN bağlantısı yapılandırma](set-up-vpn.md)

AVS özel Cloud VMware altyapı bileşenlerinizin DNS ad çözümlemesini ayarlamak için bkz. [Şirket içi iş ISTASYONLARıNDAN AVS özel bulut vCenter erişimi IÇIN DNS yapılandırma](on-premises-dns-setup.md)

## <a name="sign-in-to-azure"></a>Azure'da oturum açın

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="access-vcenter-from-azure-portal"></a>Azure portal 'dan vCenter 'a erişme

Azure portal ' dan AVS özel bulutunuzun vCenter Portal 'ı başlatabilirsiniz.

1. **Tüm Hizmetler**’i seçin.

2. **AVS Hizmetleri**için arama yapın.

3. Bağlanmak istediğiniz AVS özel bulutunuzun AVS hizmetini seçin.

4. **Genel bakış** sayfasında, **VMware AVS özel bulutlarını görüntüle** ' ye tıklayın.

    ![AVS hizmetine genel bakış](media/cloudsimple-service-overview.png)

5. AVS özel bulutları listesinden AVS özel bulutu ' nı seçin ve **vSphere Istemcisini Başlat**' a tıklayın.

    ![VSphere Istemcisini Başlat](media/cloudsimple-service-launch-vsphere-client.png)

## <a name="access-vcenter-from-avs-portal"></a>AVS portalından vCenter 'a erişme

AVS portalından, AVS özel bulutunuzun vCenter Portal 'ı başlatabilirsiniz.

1. [AVS portalınıza](access-cloudsimple-portal.md)erişin.

2. **Kaynaklardan** erışmek Istediğiniz AVS özel bulutunu seçin ve **vSphere istemcisini Başlat**' a tıklayın.

    ![VSphere Istemcisini başlatma-kaynaklar](media/cloudsimple-portal-resources-launch-vcenter.png)

3. Ayrıca, AVS özel bulutunuzun Özet ekranından vCenter Portalı ' nı da başlatabilirsiniz.

    ![VSphere Istemcisini başlatma-Özet](media/cloudsimple-resources-summary-launch-vcenter.png)

## <a name="next-steps"></a>Sonraki adımlar

* [AVS özel bulutlarınız için VLAN/alt ağlar oluşturun ve yönetin](create-vlan-subnet.md)
* [AVS özel bulut izin modeli VMware vCenter](learn-private-cloud-permissions.md)