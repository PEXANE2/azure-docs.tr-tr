---
title: API 'Leri Azure API Management Power platform 'a aktarma | Microsoft Docs
description: API 'Leri API Management güç platformuna aktarma hakkında bilgi edinin.
services: api-management
documentationcenter: ''
author: miaojiang
manager: gwallace
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 05/01/2020
ms.author: apimpm
ms.openlocfilehash: 9af20972a47e2d0ad20de62f1bb9d10e4d43563c
ms.sourcegitcommit: 366e95d58d5311ca4b62e6d0b2b47549e06a0d6d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725955"
---
# <a name="export-apis-from-azure-api-management-to-the-power-platform"></a>API 'Leri Azure API Management Power platform 'a aktarın 

Microsoft [Güç platformunu](https://powerplatform.microsoft.com) kullanan vatandaşlık geliştiricilerin genellikle profesyonel geliştiriciler tarafından geliştirilen ve Azure 'da dağıtılan iş yeteneklerine ulaşması gerekir. [Azure API Management](https://aka.ms/apimrocks) , profesyonel geliştiricilerin arka uç hizmetini API olarak yayımlamasını ve bu API 'leri, vatandaşlık geliştiricileri tarafından tüketimine yönelik özel bağlayıcılar olarak kolayca (Power Apps ve güç otomatikleştirme) dışa aktarmaya olanak sağlar. 

Bu makalede, API 'Leri API Management Power platform 'a aktarma adımlarında izlenecek yol gösterilmektedir. 

## <a name="prerequisites"></a>Ön koşullar

+ Şu hızlı başlangıcı doldurun: [Azure API Management örneği oluşturma](get-started-create-service-instance.md)
+ API Management örneğiniz için güç platformuna aktarmak istediğiniz bir API olduğundan emin olun
+ Power Apps veya Power otomatikleştir [ortamınıza](https://docs.microsoft.com/powerapps/powerapps-overview#power-apps-for-admins) sahip olduğunuzdan emin olun 

## <a name="export-an-api"></a>API 'YI dışarı aktarma

1. Azure portal API Management hizmetinize gidin ve menüden **API 'leri** seçin.
2. Dışarı aktarmak istediğiniz API 'nin yanındaki üç noktaya tıklayın. 
3. **Export** (Dışarı aktar) öğesini seçin.
4. **Power Apps ve güç otomatikleştir '** i seçin.
5. API 'yi dışarı aktarmak için bir ortam seçin. 
6. Özel bağlayıcının adı olarak kullanılacak bir görünen ad belirtin.  
7. İsteğe bağlı, API bir OAuth 2,0 sunucusu tarafından korunuyorsa,,, `Client ID`ve `Client secret` `Authorization URL` `Token URL` `Refresh URL`dahil olmak üzere ek ayrıntılar da sağlamanız gerekecektir.  
8. **Export** (Dışarı aktar) öğesini seçin. 

Dışarı aktarma işlemi tamamlandıktan sonra, Power App veya Power otomatikleştirmek ortamınıza gidin. API 'YI özel bir bağlayıcı olarak görürsünüz.

## <a name="next-steps"></a>Sonraki adımlar

* [Güç platformu hakkında daha fazla bilgi edinin](https://powerplatform.microsoft.com/)
* [Öğreticileri izleyerek API Management ortak görevleri öğrenin](https://docs.microsoft.com/azure/api-management/import-and-publish)