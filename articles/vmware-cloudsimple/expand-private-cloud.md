---
title: CloudSimple Private Cloud ile Azure VMware Çözümlerini Genişletin
description: Varolan veya yeni kümeye kapasite eklemek için varolan CloudSimple Private Cloud'un nasıl genişletilebildiğini açıklar
author: sharaths-cs
ms.author: b-shsury
ms.date: 06/06/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: a82ba1b433e62ed1c4b72b8e942d4ade29f26c4a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77025308"
---
# <a name="expand-a-cloudsimple-private-cloud"></a>CloudSimple Private Cloud'u Genişletin

CloudSimple, Özel Bulut'u dinamik olarak genişletme esnekliği sağlar. Daha küçük bir yapılandırmayla başlayabilir ve daha yüksek kapasiteye ihtiyacınız olduğu için genişletebilirsiniz. Veya mevcut ihtiyaçlar temel alınıp tüketim arttıkça genişleyebilirsiniz.

Özel Bulut, bir veya daha fazla vSphere kümesinden oluşur. Her kümede 3 ila 16 düğüm olabilir.  Özel Bulut'u genişletirken, varolan kümeye düğümler ekler veya yeni bir küme oluşturursunuz. Varolan bir kümeyi genişletmek için ek düğümlerin varolan düğümlerle aynı tür (SKU) olması gerekir. Yeni bir küme oluşturmak için düğümler farklı bir türde olabilir. Özel Bulut sınırları hakkında daha fazla bilgi için [CloudSimple özel buluta genel bakış](cloudsimple-private-cloud.md) makalesindeki sınırlar bölümüne bakın.

Özel bir bulut vCenter varsayılan **Bir Datacenter** ile oluşturulur.  Her veri merkezi üst düzey bir yönetim varlığı olarak hizmet vermektedir.  CloudSimple, yeni bir küme için varolan veri merkezine ekleme veya yeni bir veri merkezi oluşturma seçeneği sunar.

CloudSimple, yeni küme yapılandırmasının bir parçası olarak VMware altyapısını yapılandırır.  Ayarlar vSAN disk grupları, VMware Yüksek Kullanılabilirlik ve Dağıtılmış Kaynak Zamanlayıcısı (DRS) için depolama ayarlarını içerir.

Özel Bulut birden çok kez genişletilebilir. Genişletme yalnızca genel düğüm sınırları içinde kaldığınız zaman yapılabilir. Varolan kümeye her özel bulutu genişlettiğinizde veya yeni bir bulut oluşturduğunuzda.

## <a name="before-you-begin"></a>Başlamadan önce

Özel Bulut'unuzu genişletmeden önce düğümlerin sağlanması gerekir.  Düğümleri sağlama hakkında daha fazla bilgi için CloudSimple - Azure makalesine göre [VMware Solution için Hüküm düğümlerine](create-nodes.md) bakın.  Yeni bir küme oluşturmak için, aynı SKU'dan en az üç kullanılabilir düğüme sahip olmalısınız.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Azure portalında oturum [https://portal.azure.com](https://portal.azure.com)aç.

## <a name="expand-a-private-cloud"></a>Özel Bulutu genişletme

1. [CloudSimple portalına erişin.](access-cloudsimple-portal.md)

2. **Kaynaklar** sayfasını açın ve genişletmek istediğiniz Özel Bulut'u seçin.

3. Özet bölümünde **Genişlet'i**tıklatın.

    ![Özel bulutu genişletme](media/resources-expand-private-cloud.png)

4. Varolan kümenizi genişletip genişletmeyeceğinizi veya yeni bir vSphere kümesi oluşturup oluşturmayacağınızı seçin. Değişiklik yaptığınızda, sayfadaki özet bilgiler güncelleştirilir.

    * Varolan kümenizi genişletmek için **varolan kümeyi genişlet'i**tıklatın. Genişletmek istediğiniz kümeyi seçin ve ekleyecek düğüm sayısını girin. Her kümeen fazla 16 düğüm alabilir.
    * Yeni bir küme eklemek için **yeni küme oluştur'u**tıklatın. Küme için bir ad girin. Varolan bir veri merkezi seçin veya yeni bir veri merkezi oluşturmak için bir ad girin. Düğüm türünü seçin. Yeni bir vSphere kümesi oluştururken farklı bir düğüm türü seçebilirsiniz, ancak varolan bir vSphere kümesini genişletirken seçemezsiniz. Düğüm sayısını seçin. Her yeni kümenin en az üç düğümü olmalıdır.

    ![Özel bulutu genişletin - düğüm ekleyin](media/resources-expand-private-cloud-add-nodes.png)

5. Özel bulutu genişletmek için **Gönder'i** tıklatın.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure'da VMware sanal makinelerini kullanma](quickstart-create-vmware-virtual-machine.md)
* [Özel Bulutlar](cloudsimple-private-cloud.md) hakkında daha fazla bilgi edinin