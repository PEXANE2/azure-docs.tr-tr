---
title: CloudSimple tarafından Azure VMware Çözümü - Access vSphere istemcisi
description: Özel Bulut'unuzun vCenter'ına nasıl erişilenleri açıklar.
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/30/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 18d9463bc512257034860e1188372879524924f7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77022673"
---
# <a name="access-your-private-cloud-vcenter-portal"></a>Özel Bulut vCenter portalınıza erişin

Özel Bulut vCenter portalınızı Azure portalından veya CloudSimple portalından başlatabilirsiniz.  vCenter portalı, Özel Bulut'unuzda VMware altyapısını yönetmenize olanak tanır.

## <a name="before-you-begin"></a>Başlamadan önce

VCenter portalına erişmek için ağ bağlantısı oluşturulmalı ve DNS ad çözümlemesi etkinleştirilmelidir.  Aşağıdaki seçeneklerden herhangi birini kullanarak Özel Bulut'unuza ağ bağlantısı kurabilirsiniz.

* [ExpressRoute'u kullanarak şirket içinde CloudSimple'a bağlanın](on-premises-connection.md)
* [CloudSimple Private Cloud'unuza VPN bağlantısı yapılandırma](set-up-vpn.md)

Özel Bulut VMware altyapı bileşenlerinizin DNS ad çözünürlüğünü ayarlamak [için, şirket içi iş istasyonlarından Özel Bulut vCenter erişimi için ad çözümü için DNS'yi Yapılandır'a](on-premises-dns-setup.md) bakın

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Azure portalında oturum [https://portal.azure.com](https://portal.azure.com)aç.

## <a name="access-vcenter-from-azure-portal"></a>Azure portalından vCenter'a erişin

Azure portalınızdan Özel Bulut'unuzun vCenter portalını başlatabilirsiniz.

1. **Tüm Hizmetler**’i seçin.

2. **CloudSimple Services'ı**arayın.

3. Bağlanmak istediğiniz Özel Bulut'un CloudSimple hizmetini seçin.

4. Genel **Bakış** sayfasında **VMware Özel Bulutları Görüntüle'yi** tıklatın

    ![CloudSimple hizmetine genel bakış](media/cloudsimple-service-overview.png)

5. Özel Bulutlar listesinden Özel Bulut'u seçin ve **vSphere İstemci Başlat'ı**tıklatın.

    ![VSphere İstemciyi Başlat](media/cloudsimple-service-launch-vsphere-client.png)

## <a name="access-vcenter-from-cloudsimple-portal"></a>CloudSimple portalından vCenter'a erişin

CloudSimple portalından Özel Bulut'unuzun vCenter portalını başlatabilirsiniz.

1. [CloudSimple portalınıza](access-cloudsimple-portal.md)erişin.

2. **Kaynaklardan** erişmek istediğiniz Özel Bulut'u seçin ve **Başlat vSphere İstemci'sini**tıklatın.

    ![vSphere İstemciyi Başlat - Kaynaklar](media/cloudsimple-portal-resources-launch-vcenter.png)

3. Ayrıca, özel bulutunuzun özet ekranından vCenter portalını da başlatabilirsiniz.

    ![vSphere İstemciyi Başlat - Özet](media/cloudsimple-resources-summary-launch-vcenter.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Özel Bulutlarınız için V'ler/alt ağlar oluşturun ve yönetin](create-vlan-subnet.md)
* [VMware vCenter CloudSimple Özel Bulut izin modeli](learn-private-cloud-permissions.md)