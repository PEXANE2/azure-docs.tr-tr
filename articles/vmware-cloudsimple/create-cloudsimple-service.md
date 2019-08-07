---
title: CloudSimple-Service ile Azure VMware çözümü oluşturma
description: Azure portal CloudSimple hizmetinin nasıl oluşturulacağını açıklar
author: sharaths-cs
ms.author: b-shsury
ms.date: 06/04/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 6986e0a7e6eee6dbbd43c72a415b01df7da7da51
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68812435"
---
# <a name="create-azure-vmware-solution-by-cloudsimple---service"></a>CloudSimple-Service ile Azure VMware çözümü oluşturma

CloudSimple ile Azure VMware çözümünü kullanmaya başlamak için Azure portal CloudSimple Service tarafından Azure VMware çözümünü oluşturun.

## <a name="before-you-begin"></a>Başlamadan önce

Ağ geçidi alt ağı için bir/28 CıDR bloğu ayırın.  CloudSimple hizmeti başına bir ağ geçidi alt ağı gerekir ve oluşturulduğu bölge için benzersizdir. Ağ geçidi alt ağı, uç ağ hizmetleri için kullanılır ve/28 CıDR bloğu gerektirir. Ağ geçidi alt ağ adresi alanı benzersiz olmalıdır. CloudSimple ortamıyla iletişim kuran herhangi bir ağla çakışmamalıdır.  CloudSimple ile iletişim kuran ağlarda şirket içi ağlar ve Azure sanal ağları bulunur.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="create-the-service"></a>Hizmeti oluşturma

1. **Tüm Hizmetler**’i seçin.

2. **Cloudsimple Hizmetleri**için arama yapın.

    ![CloudSimple hizmeti ara](media/create-cloudsimple-service-search.png)

3. **Cloudsimple Hizmetleri**' ni seçin.

4. Yeni bir hizmet oluşturmak için **Ekle** ' ye tıklayın.

    ![CloudSimple hizmeti Ekle](media/create-cloudsimple-service-add.png)

5. CloudSimple hizmetini oluşturmak istediğiniz aboneliği seçin.

6. Hizmet için kaynak grubunu seçin. Yeni bir kaynak grubu eklemek için **Yeni oluştur**' a tıklayın.

7. Hizmeti tanımlamak için ad girin.

8. Hizmet ağ geçidi için CıDR girin. Mevcut alt ağlarınızın hiçbiriyle çakışmayacak bir/28 alt ağı belirtin.  Bunlar şirket içi alt ağları, Azure alt ağlarını veya planlı CloudSimple alt ağlarını içerir. Hizmet oluşturulduktan sonra CıDR 'yi değiştiremezsiniz.

    ![CloudSimple hizmeti oluşturma](media/create-cloudsimple-service.png)

9. **Tamam**'ı tıklatın.

Hizmet oluşturulur ve hizmetler listesine eklenir.

## <a name="next-steps"></a>Sonraki adımlar

* [Özel bulut oluşturmayı](https://docs.azure.cloudsimple.com/create-private-cloud/) öğrenin
* [Özel bir bulut ortamını yapılandırmayı](quickstart-create-private-cloud.md) öğrenin
