---
title: CloudSimple tarafından Azure VMware çözümü-CloudSimple hizmeti oluşturma
description: Azure portal CloudSimple hizmetinin nasıl oluşturulacağını açıklar
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/19/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 99194c42dbc6ef07301be517021bf0fb4b4e7c23
ms.sourcegitcommit: 42748f80351b336b7a5b6335786096da49febf6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72173500"
---
# <a name="create-the-azure-vmware-solution-by-cloudsimple-service"></a>CloudSimple hizmeti tarafından Azure VMware çözümünü oluşturma

CloudSimple ile Azure VMware çözümünü kullanmaya başlamak için Azure portal CloudSimple Service tarafından Azure VMware çözümünü oluşturun.

> [!IMPORTANT]
> CloudSimple hizmetini oluşturmadan önce, Azure aboneliğinize Microsoft. Vmwarechoparlör basit kaynak sağlayıcısını kaydetmeniz gerekir. [Azure aboneliğinizde Microsoft. Vmwarechoparlör basit kaynak sağlayıcısını etkinleştirme](enable-cloudsimple-service.md)bölümündeki adımları izleyin.

## <a name="sign-in-to-azure"></a>Azure 'da oturum açın

[Azure portalı](https://portal.azure.com)’nda oturum açın.

## <a name="create-the-service"></a>Hizmeti oluşturun

1. **Tüm hizmetler**' i seçin.
2. **Cloudsimple Hizmetleri**için arama yapın.
    ![Search CloudSimple hizmeti @ no__t-1
3. **Cloudsimple Hizmetleri**' ni seçin.
4. Yeni bir hizmet oluşturmak için **Ekle** ' ye tıklayın.
    ![Cloudsimple hizmeti Ekle @ no__t-1
5. CloudSimple hizmetini oluşturmak istediğiniz aboneliği seçin.
6. Hizmet için kaynak grubunu seçin. Yeni bir kaynak grubu eklemek için **Yeni oluştur**' a tıklayın.
7. Hizmeti tanımlamak için ad girin.
8. Hizmet ağ geçidi için CıDR girin. Şirket içi alt ağlarınızın, Azure alt ağlarının veya planlı CloudSimple alt ağlarının hiçbiriyle çakışmayacak bir/28 alt ağı belirtin. Hizmet oluşturulduktan sonra CıDR 'yi değiştiremezsiniz.

    ![CloudSimple hizmeti oluşturma](media/create-cloudsimple-service.png)
9. **Tamam**'ı tıklatın.

Hizmet oluşturulur ve hizmetler listesine eklenir.

## <a name="next-steps"></a>Sonraki adımlar

* [Düğüm sağlamayı](create-nodes.md) öğrenin
* [Özel bulut oluşturmayı](create-private-cloud.md) öğrenin
* [Özel bir bulut ortamını yapılandırmayı](quickstart-create-private-cloud.md) öğrenin
