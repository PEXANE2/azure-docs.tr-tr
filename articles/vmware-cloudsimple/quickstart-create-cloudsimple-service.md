---
title: 'Quickstart: VMware CloudSimple hizmeti oluşturun'
titleSuffix: Azure VMware Solution by CloudSimple
description: CloudSimple hizmetini oluşturmayı, düğüm satın alma ve düğüm rezerve etme
author: sharaths-cs
ms.author: dikamath
ms.date: 08/16/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: d7c137a75c0a021aa8bca3aec23da6c4d1ada300
ms.sourcegitcommit: af1cbaaa4f0faa53f91fbde4d6009ffb7662f7eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81868026"
---
# <a name="quickstart---create-azure-vmware-solution-by-cloudsimple-service"></a>Quickstart - CloudSimple hizmetine göre Azure VMware Çözümü Oluşturun

Başlamak için Azure portalında CloudSimple tarafından Azure VMware Çözümlü'nu oluşturun.

## <a name="vmware-solution-by-cloudsimple---service-overview"></a>CloudSimple tarafından VMware Çözüm - Hizmet genel bakış

CloudSimple hizmeti, CloudSimple tarafından Azure VMware Solution'ı tüketmenizi sağlar.  Hizmeti oluşturmak düğümleri sağlamanızı, düğümleri rezerve etmenizi ve özel bulutlar oluşturmanıza olanak tanır.  CloudSimple hizmetinin kullanılabildiği her Azure bölgesine CloudSimple hizmetini eklersiniz.  Hizmet, CloudSimple tarafından Azure VMware Solution'ın kenar ağını tanımlar.  Bu kenar ağı, vpn, ExpressRoute ve özel bulutlarınıza Internet bağlantısı içeren hizmetler için kullanılır.

CloudSimple hizmetini eklemek için bir ağ geçidi alt ağı oluşturmanız gerekir. Ağ geçidi alt ağı oluşturulurken kullanılır ve /28 CIDR bloğu gerektirir. Ağ geçidi alt ağ adresi alanı benzersiz olmalıdır. Şirket içi ağ adres alanlarınız veya Azure sanal ağ adres alanınızla çakışamaz.

## <a name="before-you-begin"></a>Başlamadan önce

Ağ geçidi alt ağı için bir /28 CIDR bloğu ayırın.  CloudSimple hizmeti başına bir ağ geçidi alt ağı gereklidir ve oluşturulduğu bölgeye özgüdür. Ağ geçidi alt ağı, CloudSimple kenar ağ hizmetleri tarafından Azure VMware Çözümü için kullanılır ve /28 CIDR bloğu gerektirir. Ağ geçidi alt ağ adresi alanı benzersiz olmalıdır. CloudSimple ortamıyla iletişim kuranabilecek herhangi bir ağla örtüşmemelidir.  CloudSimple ile iletişim sağlayan ağlar şirket içi ağları ve Azure sanal ağları içerir.

Ağ Önkoşulları Gözden [Geçirin.](cloudsimple-network-checklist.md) 

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="create-the-service"></a>Hizmeti oluşturma

1. **Tüm Hizmetler**’i seçin.
2. **CloudSimple Service**için arama .

    ![Search CloudSimple Service](media/create-cloudsimple-service-search.png)

3. **CloudSimple Services'ı**seçin.
4. Yeni bir hizmet oluşturmak için **Ekle'yi** tıklatın.

    ![CloudSimple Hizmeti Ekle](media/create-cloudsimple-service-add.png)

5. CloudSimple hizmetini oluşturmak istediğiniz aboneliği seçin.
6. Hizmet için kaynak grubunu seçin. Yeni bir kaynak grubu eklemek için **Yeni Oluştur'u**tıklatın.
7. Hizmeti tanımlamak için ad girin.
8. Hizmet ağ geçidi için CIDR'yi girin. Şirket içi alt ağların, Azure alt ağların veya planlanan CloudSimple alt ağlarınizle çakışan bir /28 alt ağı belirtin. Hizmet oluşturulduktan sonra CIDR'yi değiştiremezsiniz.

    ![CloudSimple hizmetini oluşturma](media/create-cloudsimple-service.png)

9. **Tamam**'a tıklayın.

Hizmet oluşturulur ve hizmetler listesine eklenir.

## <a name="provision-nodes"></a>Sağlama düğümleri

CloudSimple Private Cloud ortamı için ödeme olarak ödeme kapasitesi ayarlamak için, ilk hüküm düğümleri Azure portalında.

1. **Tüm Hizmetler**’i seçin.
2. **CloudSimple Düğümleri**arayın.

    ![Arama BulutuBasit Düğümler](media/create-cloudsimple-node-search.png)

3. **CloudSimple Düğümlerini**seçin.
4. Düğüm oluşturmak için **Ekle'yi** tıklatın.

    ![CloudSimple Düğümleri Ekle](media/create-cloudsimple-node-add.png)

5. CloudSimple düğümlerini sağlamak istediğiniz aboneliği seçin.
6. Düğümler için kaynak grubunu seçin. Yeni bir kaynak grubu eklemek için **Yeni Oluştur'u**tıklatın.
7. Düğümleri tanımlamak için önek girin.
8. Düğüm kaynaklarının konumunu seçin.
9. Düğüm kaynaklarını barındırmak için özel konumu seçin.
10. Düğüm [türünü](cloudsimple-node.md)seçin.
11. Hükmetmek için düğüm sayısını seçin.
12. **Gözden Geçir + Oluştur'u**seçin.
13. Ayarları gözden geçirin. Herhangi bir ayarı değiştirmek için **Önceki'yi**tıklatın.
14. **Oluştur**’u seçin.

## <a name="next-steps"></a>Sonraki adımlar

* [Özel Bulut oluşturun ve ortamı yapılandırın](quickstart-create-private-cloud.md)
* [CloudSimple hizmeti](https://docs.microsoft.com/azure/vmware-cloudsimple/cloudsimple-service) hakkında daha fazla bilgi edinin
