---
title: Azure aboneliğini CloudSimple ile Azure VMware çözümünde kaynak havuzlarıyla eşleyin
description: Azure VMware çözümünde bir kaynak havuzunun Azure aboneliğinizde CloudSimple tarafından nasıl eşleneceğini açıklar
author: sharaths-cs
ms.author: b-shsury
ms.date: 06/05/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 1bf721f35500d2ff1344996e7750c5e574f40f31
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68816274"
---
# <a name="map-resource-pools-from-your-private-cloud-to-your-azure-subscription"></a>Özel bulutunuzun kaynak havuzlarını Azure aboneliğinize eşleyin

Azure abonelik eşlemesi, özel bulut vCenter 'ınızdan kaynak havuzlarını Azure aboneliğinize eşlemenizi sağlar. Yalnızca CloudSimple hizmetini oluşturduğunuz aboneliğin eşlemesini yapabilirsiniz.  Azure portal bir VMware sanal makinesi oluşturmak, sanal makineyi eşlenen kaynak havuzunda dağıtır.  CloudSimple portalında, özel bulutlarınız için Azure aboneliğini görüntüleyebilir ve yönetebilirsiniz.

Bir abonelik, özel bulutun birden çok vCenter kaynak havuzuna eşlenebilir.  Her özel bulutun kaynak havuzlarını eşlemeniz gerekir.  Yalnızca eşleştirilmiş kaynak havuzları Azure portal bir VMware sanal makinesi oluşturmak için kullanılabilir.

> [!IMPORTANT]
> Kaynak havuzunu eşleme, tüm alt kaynak havuzlarını de eşler. Alt kaynak havuzları zaten eşlenmişse bir üst kaynak havuzu eşlenemez.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, aboneliğinizde bir CloudSimple hizmeti ve özel bulut olduğunu varsaymaktadır.  CloudSimple hizmeti oluşturmak için bkz. [hızlı başlangıç-hizmet oluşturma](quickstart-create-cloudsimple-service.md).  Özel bir bulut oluşturmanız gerekiyorsa bkz. [hızlı başlangıç-özel bir bulut ortamı yapılandırma](quickstart-create-private-cloud.md).

VCenter kümesini (kök kaynak havuzu) aboneliğinize eşleyebilirsiniz.  Yeni bir kaynak havuzu oluşturmak istiyorsanız, VMware belgeleri sitesinde [kaynak havuzu oluşturma](https://docs.vmware.com/en/VMware-vSphere/6.7/com.vmware.vsphere.resmgmt.doc/GUID-0F6C6709-A5DA-4D38-BE08-6CB1002DD13D.html) makalesine bakın.

## <a name="default-resource-group"></a>Varsayılan kaynak grubu

Azure portal yeni bir CloudSimple sanal makinesi oluşturmak, kaynak grubunu seçmenizi sağlar.  Eşlenmiş bir kaynak havuzundaki özel bulut vCenter üzerinde oluşturulan bir sanal makine, Azure portal görünür.  Bulunan sanal makine, varsayılan Azure Kaynak grubuna yerleştirilir.  Varsayılan kaynak grubunun adını değiştirebilirsiniz.

## <a name="map-azure-subscription"></a>Azure aboneliğini eşleme

1. [Cloudsimple portalına](access-cloudsimple-portal.md)erişin.

2. **Kaynaklar** sayfasını açın ve eşlemek istediğiniz özel bulutu seçin.

3. **Azure abonelikleri eşlemesini**seçin.

4. **Azure abonelik eşlemesini Düzenle**' ye tıklayın.

5. Kullanılabilir kaynak havuzlarını eşlemek için sol tarafta bunları seçin ve sağ oka tıklayın.

6. Eşlemeleri kaldırmak için, sağ tarafta seçin ve sola bakan oka tıklayın.

    ![Azure abonelikleri](media/resources-azure-mapping.png)

7. **Tamam**'ı tıklatın.

## <a name="change-default-resource-group-name"></a>Varsayılan kaynak grubu adını değiştir

1. [Cloudsimple portalına](access-cloudsimple-portal.md)erişin.

2. **Kaynaklar** sayfasını açın ve eşlemek istediğiniz özel bulutu seçin.

3. **Azure abonelikleri eşlemesini**seçin.

4. Azure Kaynak grubu adı bölümünde **Düzenle** ' ye tıklayın.

    ![Kaynak grubu adını Düzenle](media/resources-edit-resource-group-name.png)

5. Kaynak grubu için yeni bir ad girin ve **Gönder**' e tıklayın.

    ![Yeni kaynak grubu adı girin](media/resources-new-resource-group-name.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure 'da VMware VM 'lerini kullanma](quickstart-create-vmware-virtual-machine.md)
* [Cloudsimple sanal makineleri](cloudsimple-virtual-machines.md) hakkında daha fazla bilgi edinin