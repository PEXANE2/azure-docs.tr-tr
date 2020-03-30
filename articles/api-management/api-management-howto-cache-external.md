---
title: Azure API Yönetimi'nde harici önbellek kullanma | Microsoft Dokümanlar
description: Azure API Yönetimi'nde harici bir önbelleği nasıl yapılandıracağınızı ve kullanacağınızı öğrenin.
services: api-management
documentationcenter: ''
author: mikebudzynski
manager: erikre
editor: ''
ms.assetid: 740f6a27-8323-474d-ade2-828ae0c75e7a
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 05/15/2019
ms.author: apimpm
ms.openlocfilehash: 2e8863eed774884a99de8643c9e497378368d166
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "70072496"
---
# <a name="use-an-external-azure-cache-for-redis-in-azure-api-management"></a>Azure API Management'ta dış Azure Cache for Redis önbelleği kullanma

Azure API Yönetimi, yerleşik önbelleği kullanmanın yanı sıra Redis için harici bir Azure Önbelleğinde yanıtları önbelleğe almaya da olanak tanır.

Harici önbellek kullanmak, yerleşik önbelleğin birkaç sınırlamasının üstesinden gelinmesine olanak tanır. Eğer isterseniz özellikle yararlıdır:

* API Yönetimi güncelleştirmeleri sırasında önbelleğinizin düzenli olarak temizlenmesinden kaçının
* Önbellek yapılandırmanız üzerinde daha fazla denetime sahip
* API Yönetimi katmanınızın izin verdiğinden daha fazla veri önbelleğe
* API Yönetimi'nin Tüketim katmanı ile önbelleğe alma

Önbelleğe alma hakkında daha ayrıntılı bilgi için bkz. [API Management önbelleğe alma ilkeleri](api-management-caching-policies.md) ve [Azure API Management'te özel önbelleğe alma](api-management-sample-cache-by-key.md).

![Kendi önbelleğinizi APIM'e getirin](media/api-management-howto-cache-external/overview.png)

Öğrenecekleriniz:

> [!div class="checklist"]
> * API Yönetimi'nde harici önbellek ekleme

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdakileri yapmanız gerekir:

+ [Azure API Management örneği oluşturma](get-started-create-service-instance.md)
+ [Azure API Yönetiminde önbelleğe alma](api-management-howto-cache.md)

## <a name="create-azure-cache-for-redis"></a><a name="create-cache"> </a> Redis için Azure Önbelleği Oluşturma

Bu bölümde, Azure'da Redis için bir Azure Önbelleği nasıl oluşturulacağını açıklar. Azure içinde veya dışında Redis için zaten bir Azure Önbelleğiniz varsa, bir sonraki bölüme <a href="#add-external-cache">atlayabilirsiniz.</a>

[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="add-an-external-cache"></a><a name="add-external-cache"> </a>Harici önbellek ekleme

Azure API Yönetimi'ne Redis için harici bir Azure Önbelleği eklemek için aşağıdaki adımları izleyin.

![Kendi önbelleğinizi APIM'e getirin](media/api-management-howto-cache-external/add-external-cache.png)

> [!NOTE]
> Ayarda **kullanımı,** API Yönetimi'nin çok bölgesel bir yapılandırması durumunda hangi API Yönetimi bölgesel dağıtımının yapılandırılmış önbellekle iletişim kuracağını belirtir. **Varsayılan** olarak belirtilen önbellekler, bölgesel değeri olan önbellekler tarafından geçersiz kılınacaktır.
>
> Örneğin, API Yönetimi Doğu ABD, Güneydoğu Asya ve Batı Avrupa bölgelerinde barındırılıyorsa ve biri **Varsayılan,** diğeri Güneydoğu Asya için olmak üzere iki önbellek yapılandırılırsa, **Güneydoğu Asya'daki** API Yönetimi kendi önbelleğini kullanırken, diğer iki bölge **Varsayılan** önbellek girişini kullanır. **Southeast Asia**

### <a name="add-an-azure-cache-for-redis-from-the-same-subscription"></a>Aynı abonelikten Redis için Azure Önbelleği ekleme

1. Azure portalındaki API Yönetimi örneğinize göz atın.
2. Soldaki menüden **Dış önbellek** sekmesini seçin.
3. **+ Ekle** düğmesine tıklayın.
4. **Önbellek örneği** açılır alanında önbelleğinizi seçin.
5. **Varsayılan'ı** seçin veya açılır **alandan Kullanım'da** istenen bölgeyi belirtin.
6. **Kaydet**'e tıklayın.

### <a name="add-an-azure-cache-for-redis-hosted-outside-of-the-current-azure-subscription-or-azure-in-general"></a>Geçerli Azure aboneliği veya genel olarak Azure dışında barındırılan Redis için bir Azure Önbelleği ekleme

1. Azure portalındaki API Yönetimi örneğinize göz atın.
2. Soldaki menüden **Dış önbellek** sekmesini seçin.
3. **+ Ekle** düğmesine tıklayın.
4. **Önbellek örneği** açılır alanında **Özel'i** seçin.
5. **Varsayılan'ı** seçin veya açılır **alandan Kullanım'da** istenen bölgeyi belirtin.
6. **Bağlantı dizesi** alanında Redis bağlantı dizesini için Azure Önbelleğinizi sağlayın.
7. **Kaydet**'e tıklayın.

## <a name="use-the-external-cache"></a>Harici önbelleği kullanma

Dış önbellek Azure API Yönetimi'nde yapılandırıldıktan sonra önbelleğe alma ilkeleri aracılığıyla kullanılabilir. Ayrıntılı adımlar [için Azure API Yönetimi'nde performansı artırmak için önbelleğe ekle'ye](api-management-howto-cache.md) bakın.

## <a name="next-steps"></a><a name="next-steps"> </a>Sonraki adımlar

* Önbelleğe alma ilkeleri hakkında daha fazla bilgi için bkz. [API Management ilke başvurusu][API Management policy reference]’nda [Önbelleğe alma ilkeleri][Caching policies].
* Anahtar kullanım ilkesi ifadeleri hakkında daha fazla bilgi için bkz. [Azure API Management’te özel önbelleğe alma](api-management-sample-cache-by-key.md).

[API Management policy reference]: https://msdn.microsoft.com/library/azure/dn894081.aspx
[Caching policies]: https://msdn.microsoft.com/library/azure/dn894086.aspx
