---
title: Azure DNS - Azure portalında DNS bölgelerini yönetme | Microsoft Dokümanlar
description: Azure portalını kullanarak DNS bölgelerini yönetebilirsiniz. Bu makalede, Azure DNS'de DNS bölgeleri nasıl güncelleştirilir, silinir ve oluşturulur
services: dns
documentationcenter: na
author: rohinkoul
manager: timlt
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/18/2017
ms.author: rohink
ms.openlocfilehash: 002f210048c18c6dd99dfb5981bacce8666ee563
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76936774"
---
# <a name="how-to-manage-dns-zones-in-the-azure-portal"></a>Azure portalında DNS Bölgeleri nasıl yönetilir?

> [!div class="op_single_selector"]
> * [Portal](dns-operations-dnszones-portal.md)
> * [Powershell](dns-operations-dnszones.md)
> * [Azure klasik CLI](dns-operations-dnszones-cli-nodejs.md)
> * [Azure CLI](dns-operations-dnszones-cli.md)

Bu makalede, Azure portalını kullanarak DNS bölgelerinizi nasıl yönetebildiğiniz gösterilmektedir. Ayrıca, platformlar arası [Azure CLI'yi](dns-operations-dnszones-cli.md) veya Azure [PowerShell'i](dns-operations-dnszones.md)kullanarak DNS bölgelerinizi de yönetebilirsiniz.

## <a name="create-a-dns-zone"></a>DNS bölgesi oluşturma

1. Azure portalında oturum açın
2. Hub menüsünde, **DNS bölge oluştur** bıyığını açmak için **Ağ > DNS bölgesi>** bir kaynak oluştur'a gidin.

    ![DNS bölgesi](./media/dns-operations-dnszones-portal/openzone650.png)

4. **DNS bölgesi oluştur** dikey penceresinde aşağıdaki değerleri girin ve **Oluştur**’a tıklayın:


   | **Ayar** | **Değer** | **Şey** |
   |---|---|---|
   |**Adı**|contoso.com|DNS bölgesinin adı|
   |**Abonelik**|[Aboneliğiniz]|DNS bölgesini oluşturmak için bir abonelik seçin.|
   |**Kaynak grubu**|**Yeni oluştur:** contosoDNSRG|Bir kaynak grubu oluşturun. Kaynak grubu adı, seçili abonelik içinde benzersiz olmalıdır. Kaynak grupları hakkında daha fazla bilgi edinmek için [Kaynak Yöneticisi'ne](../azure-resource-manager/management/overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups) genel bakış makalesini okuyun.|
   |**Konum**|Batı ABD||

> [!NOTE]
> Kaynak grubu, kaynak grubunun konumunu ifade eder ve DNS bölgesini etkilemez. DNS bölgesinin konumu her zaman "genel" şeklindedir ve gösterilmez.

## <a name="list-dns-zones"></a>DNS bölgelerini listeleme

Azure portalında,**DNS bölgeleri****ağı yla** > ilgili **daha fazla hizmete** > gidin. Her DNS bölgesi kendi kaynağıdır ve kayıt kümelerinin sayısı ve ad sunucuları gibi bilgiler bu görünümden görüntülenebilir. Sütun **Adı SUNUCULARI** varsayılan görünümde değildir. Eklemek için **Sütunlar'ı**tıklatın, **Sunucular Adı'nı**seçin ve ardından **Bitti'yi**tıklatın.

![DNS bölgelerini listele](./media/dns-operations-dnszones-portal/listzones.png)

## <a name="delete-a-dns-zone"></a>DNS bölgesini silme

Portaldaki bir DNS bölgesine gidin. **DNS bölge** bıçak üzerinde, **bölge sil'i**tıklatın. Daha sonra DNS bölgesini silmek istediğinizi onaylamanız istenir. Bir DNS bölgesinin silinmesi, bölgede bulunan tüm kayıtları da siler.

## <a name="next-steps"></a>Sonraki adımlar

[Azure portalını kullanarak Azure DNS ile çalışmaya başlayın'ı](dns-getstarted-portal.md)ziyaret ederek DNS Bölgeniz ve kayıtlarınız ile nasıl çalışacağınızı öğrenin.