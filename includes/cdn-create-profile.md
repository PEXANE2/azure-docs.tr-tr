---
title: include dosyası
description: include dosyası
services: cdn
author: SyntaxC4
ms.service: azure-cdn
ms.topic: include
ms.date: 04/30/2020
ms.author: cfowler
ms.custom: include file
ms.openlocfilehash: d73a8d743a948cbd94a3af81fe2e77c45c0eeb67
ms.sourcegitcommit: 309a9d26f94ab775673fd4c9a0ffc6caa571f598
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2020
ms.locfileid: "82996254"
---
## <a name="create-a-new-cdn-profile"></a>Yeni bir CDN profili oluşturma

CDN profili, CDN uç noktaları için bir kapsayıcı olup bir fiyatlandırma katmanını belirtir.

1. Azure portal, **kaynak oluştur** ' u seçin (sol üst tarafta). **Yeni** bölmesi görüntülenir.
   
1. **CDN**'yi arayıp seçin, sonra **Oluştur**' u seçin:
   
    ![CDN kaynağını seçin](./media/cdn-create-profile/cdn-new-resource.png)

    **CDN profili** bölmesi görüntülenir.

1. Aşağıdaki değerleri girin:
   
    | Ayar  | Değer |
    | -------- | ----- |
    | **Adı** | Profil adınız için *CDN-profile-123* girin. Bu ad genel olarak benzersiz olmalıdır; zaten kullanımda ise, farklı bir ad girin. |
    | **Abonelik** | Açılan listeden bir Azure aboneliği seçin. |
    | **Kaynak grubu** | **Yeni oluştur** ' u seçin ve kaynak grubunuzun adı Için *cdnquickstart-RG* girin veya **var olanı kullan** ' ı seçin ve zaten grubunuz varsa *cdnquickstart-RG* öğesini seçin. | 
    | **Kaynak grubu konumu** | Aşağı açılan listeden yakınınızdaki bir konum seçin. |
    | **Fiyatlandırma Katmanı** | Açılan listeden bir **Standart Akamai** seçeneği belirleyin. (Akamai katmanının dağıtım süresi yaklaşık bir dakikadır. Microsoft katmanı yaklaşık 10 dakika sürer ve Verizon katmanları yaklaşık 90 dakika sürer.) |
    | **Şimdi yeni bir CDN uç noktası oluşturun** | Seçilmemiş şekilde bırakın. |  
   
    ![Yeni CDN profili](./media/cdn-create-profile/cdn-new-profile.png)

1. Profili oluşturmak için **Oluştur**’u seçin.

