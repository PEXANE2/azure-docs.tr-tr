---
title: Azure API Management dış önbellek kullanma | Microsoft Docs
description: Azure API Management 'de dış önbellek yapılandırmayı ve kullanmayı öğrenin. Dış önbellek kullanmak, yerleşik önbelleğin bazı sınırlamalarını aşmanızı sağlar.
services: api-management
documentationcenter: ''
author: mikebudzynski
manager: erikre
editor: ''
ms.assetid: 740f6a27-8323-474d-ade2-828ae0c75e7a
ms.service: api-management
ms.topic: conceptual
ms.date: 04/26/2020
ms.author: apimpm
ms.openlocfilehash: cfb7dd7a3831d90235b25af9598cfbc137ffcb3d
ms.sourcegitcommit: 4e5560887b8f10539d7564eedaff4316adb27e2c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87904964"
---
# <a name="use-an-external-redis-compatible-cache-in-azure-api-management"></a>Azure API Management dış Redsıs uyumlu bir önbellek kullanın

Azure API Management, yerleşik önbelleğin kullanılmasıyla ilgili ek olarak, dış Red, uyumlu bir önbellekte yanıtları önbelleğe almaya izin verir, örneğin Redsıs için Azure Cache.

Dış önbellek kullanmak, yerleşik önbelleğin bazı sınırlamalarını aşmayı sağlar:

* API Management güncelleştirmeler sırasında önbelleğinizin düzenli aralıklarla silinmesini önleyin
* Önbellek yapılandırmanız üzerinde daha fazla denetime sahip
* API Management katmanından daha fazla veri önbelleğe almasına izin verir
* API Management tüketim katmanıyla önbelleğe alma kullanın
* [API Management kendi kendine barındırılan ağ geçitlerinde](self-hosted-gateway-overview.md) önbelleğe almayı etkinleştir

Önbelleğe alma hakkında daha ayrıntılı bilgi için bkz. [API Management önbelleğe alma ilkeleri](api-management-caching-policies.md) ve [Azure API Management'te özel önbelleğe alma](api-management-sample-cache-by-key.md).

![Kendi önbelleğinizi APıM 'e taşıyın](media/api-management-howto-cache-external/overview.png)

Öğrenecekleriniz:

> [!div class="checklist"]
> * API Management bir dış önbellek ekleyin

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdakileri yapmanız gerekir:

+ [Azure API Management örneği oluşturma](get-started-create-service-instance.md)
+ [Azure API Management önbelleğe almayı](api-management-howto-cache.md) anlama

## <a name="create-azure-cache-for-redis"></a><a name="create-cache"> </a> Redsıs Için Azure önbelleği oluşturma

Bu bölümde, Azure 'da redin için Azure önbelleğinin nasıl oluşturulacağı açıklanmaktadır. Zaten Azure 'un içinde veya dışında bir Azure önbelleğiniz varsa, sonraki bölüme <a href="#add-external-cache">atlayabilirsiniz</a> .

[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="deploy-redis-cache-to-kubernetes"></a><a name="create-cache"> </a> Redsıs önbelleğini Kubernetes 'e dağıtma

Önbelleğe alma için, şirket içinde barındırılan ağ geçitleri özel olarak dış önbelleklere güvenir. Ön belleğe alma işleminin etkili bir şekilde barındırılması için ve bağımlı oldukları önbelleğin, arama ve depolama gecikmeleri en aza indirmek için birbirlerine yakın olması gerekir. Redsıs önbelleğinin aynı Kubernetes kümesine veya yakında ayrı bir kümeye dağıtılmasının en iyi seçenek vardır. Redsıs önbelleğinin bir Kubernetes kümesine nasıl dağıtılacağını öğrenmek için bu [bağlantıyı](https://github.com/kubernetes/examples/tree/master/guestbook) izleyin.

## <a name="add-an-external-cache"></a><a name="add-external-cache"> </a>Dış önbellek Ekle

Azure API Management 'de Redsıs için dış Azure önbelleği eklemek üzere aşağıdaki adımları izleyin.

![Kendi önbelleğinizi APıM 'e taşıyın](media/api-management-howto-cache-external/add-external-cache.png)

> [!NOTE]
> Bu ayardan **kullanımı** , yapılandırılmış önbelleği kullanacak bir Azure bölgesi veya şirket içinde barındırılan ağ geçidi konumunu belirtir. **Varsayılan** olarak yapılandırılan önbellekler, belirli bir eşleşen bölge veya konum değeri olan önbellekler tarafından geçersiz kılınır.
>
> Örneğin, API Management Doğu ABD, Güneydoğu Asya ve Batı Avrupa bölgelerinde barındırılıyorsa ve iki önbellek yapılandırıldığında, biri **varsayılan** diğeri ve **Güneydoğu Asya**için bir tane yapılandırılmışsa, **Güneydoğu Asya** 'daki API Management kendi önbelleğini kullanır, ancak diğer iki bölge **varsayılan** önbellek girişini kullanır.

### <a name="add-an-azure-cache-for-redis-from-the-same-subscription"></a>Aynı aboneliğden Redsıs için Azure önbelleği ekleme

1. Azure portal API Management örneğinizi inceleyin.
2. Sol taraftaki menüden **dış önbellek** sekmesini seçin.
3. **+ Ekle** düğmesine tıklayın.
4. **Önbellek örneği** açılan alanında önbelleğinizi seçin.
5. **Varsayılan** ' ı seçin veya istediğiniz bölgeyi açılan **menüden kullan** alanından belirtin.
6. **Kaydet**’e tıklayın.

### <a name="add-an-azure-cache-for-redis-hosted-outside-of-the-current-azure-subscription-or-azure-in-general"></a>Geçerli Azure aboneliğinin veya genel olarak Azure 'un dışında barındırılan Red, için Azure önbelleği ekleme

1. Azure portal API Management örneğinizi inceleyin.
2. Sol taraftaki menüden **dış önbellek** sekmesini seçin.
3. **+ Ekle** düğmesine tıklayın.
4. **Önbellek örneği** açılan alanında **özel** ' i seçin.
5. **Varsayılan** ' ı seçin veya istediğiniz bölgeyi açılan **menüden kullan** alanından belirtin.
6. **Bağlantı dizesi** alanında redsıs bağlantı dizesi Için Azure önbelleğinizi sağlayın.
7. **Kaydet**’e tıklayın.

### <a name="add-a-redis-cache-to-a-self-hosted-gateway"></a>Kendi kendine barındırılan bir ağ geçidine Redsıs önbelleği ekleme

1. Azure portal API Management örneğinizi inceleyin.
2. Sol taraftaki menüden **dış önbellek** sekmesini seçin.
3. **+ Ekle** düğmesine tıklayın.
4. **Önbellek örneği** açılan alanında **özel** ' i seçin.
5. İstenen şirket içinde barındırılan ağ geçidi konumunu veya **varsayılan** , açılan **listeden kullan** alanından belirtin.
6. **Bağlantı dizesi** alanında redsıs Cache Bağlantı dizenizi belirtin.
7. **Kaydet**’e tıklayın.

## <a name="use-the-external-cache"></a>Dış önbelleği kullanma

Azure API Management 'de dış önbellek yapılandırıldıktan sonra, önbelleğe alma ilkeleri aracılığıyla kullanılabilir. Ayrıntılı adımlar için bkz. [Azure API Management performansı artırmak için önbelleğe alma ekleme](api-management-howto-cache.md) .

## <a name="next-steps"></a><a name="next-steps"> </a>Sonraki adımlar

* Önbelleğe alma ilkeleri hakkında daha fazla bilgi için bkz. [API Management ilke başvurusu][API Management policy reference]’nda [Önbelleğe alma ilkeleri][Caching policies].
* Anahtar kullanım ilkesi ifadeleri hakkında daha fazla bilgi için bkz. [Azure API Management’te özel önbelleğe alma](api-management-sample-cache-by-key.md).

[API Management policy reference]: ./api-management-policies.md
[Caching policies]: ./api-management-caching-policies.md
