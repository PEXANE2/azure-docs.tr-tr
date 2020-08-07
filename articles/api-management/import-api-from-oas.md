---
title: Azure portalını kullanarak OpenAPI belirtimini içeri aktarma | Microsoft Docs
description: API Management ile bir Openapı belirtimini içeri aktarmayı öğrenin ve ardından API 'yi Azure ve geliştirici portalları ' nda test edin.
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 04/20/2020
ms.author: apimpm
ms.openlocfilehash: 346156c624a4c8b1c1d6cc490fc0559f364b4b9d
ms.sourcegitcommit: 4e5560887b8f10539d7564eedaff4316adb27e2c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87905576"
---
# <a name="import-an-openapi-specification"></a>OpenAPI belirtimini içeri aktarma

Bu makalede, https://conferenceapi.azurewebsites.net?format=json konumunda bulunan bir "OpenAPI belirtimi" arka uç API’sinin nasıl içeri aktarılacağı gösterilmektedir. Bu arka uç API’si, Microsoft tarafından sağlanır ve Azure’da barındırılır. Makale, APIM API’sinin nasıl test edileceğini de göstermektedir.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * "OpenAPI belirtimi" arka uç API’sini içeri aktarma
> * Azure portalında API’yi test etme
> * Geliştirici portalında API’yi test etme

## <a name="prerequisites"></a>Önkoşullar

Şu hızlı başlangıcı doldurun: [Azure API Management örneği oluşturma](get-started-create-service-instance.md)

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="import-and-publish-a-back-end-api"></a><a name="create-api"> </a>Arka uç API'sini içeri aktarma ve yayımlama

1. Azure portal API Management hizmetinize gidin ve menüden **API 'leri** seçin.
2. **Yeni API ekle** listesinden **OpenAPI belirtimi**’ni seçin.

    ![Openapı belirtimi](./media/import-api-from-oas/oas-api.png)
3. API ayarlarını girin. Oluşturma sırasında değerleri ayarlayabilir veya daha sonra **Ayarlar** sekmesine giderek yapılandırabilirsiniz. Ayarlar, [Ilk API 'Nizi Içeri aktarma ve yayımlama](import-and-publish.md#-import-and-publish-a-backend-api) öğreticisinde açıklanmıştır.
4. **Oluştur**’u seçin.

> [!NOTE]
> API içeri aktarma sınırlamaları, [başka bir makalede](api-management-api-import-restrictions.md)belgelenmiştir.

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-append-apis.md)]

[!INCLUDE [api-management-define-api-topics.md](../../includes/api-management-define-api-topics.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Yayımlanan API’yi dönüştürme ve koruma](transform-api.md)
