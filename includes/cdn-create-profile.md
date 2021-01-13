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
ms.openlocfilehash: c6352ee9d29e4e45aa4be449046a0715fee06047
ms.sourcegitcommit: 16887168729120399e6ffb6f53a92fde17889451
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98165918"
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
    | **Ad** | Profil adınız için *CDN-profile-123* girin. Bu ad genel olarak benzersiz olmalıdır; zaten kullanımda ise, farklı bir ad girin. |
    | **Abonelik** | Açılan listeden bir Azure aboneliği seçin. |
    | **Kaynak grubu** | **Yeni oluştur** ' u seçin ve kaynak grubunuzun adı Için *cdnquickstart-RG* girin veya **var olanı kullan** ' ı seçin ve zaten grubunuz varsa *cdnquickstart-RG* öğesini seçin. | 
    | **Kaynak grubu konumu** | Aşağı açılan listeden yakınınızdaki bir konum seçin. |
    | **Fiyatlandırma katmanı** | Açılan listeden bir **Standart Akamai** seçeneği belirleyin. (Akamai katmanının dağıtım süresi yaklaşık bir dakikadır. Microsoft katmanı yaklaşık 10 dakika sürer ve Verizon katmanları yaklaşık 30 dakika sürer.) |
    | **Şimdi yeni bir CDN uç noktası oluşturun** | Seçilmemiş şekilde bırakın. |  
   
    ![Yeni CDN profili](./media/cdn-create-profile/cdn-new-profile.png)

1. Profili oluşturmak için **Oluştur**’u seçin.

