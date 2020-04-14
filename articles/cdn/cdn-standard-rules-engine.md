---
title: Standart Azure CDN'de HTTPS'yi zorlamak için bir kural altyapısı kullanın | Microsoft Dokümanlar
description: Azure CDN'nin belirli içerik türlerinin teslimini engelleme, önbelleğe alma ilkesi tanımlama ve HTTP üstbilgileri değiştirme dahil olmak üzere HTTP isteklerini nasıl işleyeceğini özelleştirmek için Microsoft Standart Azure İçerik Teslim Ağı 'nın (Azure CDN) kural altyapısını kullanın. Bu makalede, kullanıcıları HTTPS'ye yönlendirmek için nasıl bir kural oluşturabilirsiniz öğrenin.
services: cdn
author: asudbring
ms.service: azure-cdn
ms.topic: article
ms.date: 11/01/2019
ms.author: allensu
ms.openlocfilehash: 91a442573139bf4fdd09978290bf2380c8bcb97e
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81259943"
---
# <a name="set-up-the-standard-rules-engine-for-azure-cdn"></a>Azure CDN için Standart kurallar altyapısını ayarlama

Bu makalede, Azure İçerik Teslim Ağı (Azure CDN) için Standart kurallar altyapısının nasıl ayarlanıp kullanılacağı açıklanmaktadır.

## <a name="standard-rules-engine"></a>Standart kurallar motoru

HTTP isteklerinin nasıl işleneceğini özelleştirmek için Azure CDN için Standart kurallar altyapısını kullanabilirsiniz. Örneğin, belirli protokolleri kullanmak, önbelleğe alma ilkesi tanımlamak veya bir HTTP üstbilgisini değiştirmek için içerik teslimini zorlamak için kurallar altyapısını kullanabilirsiniz. Bu makalede, kullanıcıları otomatik olarak HTTPS'ye yönlendiren bir kuralın nasıl oluşturulutamamı gösterilmiş olduğu gösteriş. 

> [!NOTE]
> Bu makalede açıklanan kurallar altyapısı yalnızca Microsoft'un Standart Azure CDN'si için kullanılabilir. 

## <a name="redirect-users-to-https"></a>Kullanıcıları HTTPS'ye yönlendirme

1. Microsoft profillerinizde Azure İçerik Dağıtım Ağı'na gidin.

1. **CDN profil** sayfasında, için kurallar oluşturmak istediğiniz bitiş noktasını seçin.
  
1. Kurallar **Motoru** sekmesini seçin.
   
    **Kurallar Altyapısı** bölmesi açılır ve kullanılabilir genel kuralların listesini görüntüler. 
   
    [![Azure CDN yeni kurallar sayfası](./media/cdn-standard-rules-engine/cdn-new-rule.png)](./media/cdn-standard-rules-engine/cdn-new-rule.png#lightbox)
   
   > [!IMPORTANT]
   > Birden çok kuralın listelenme sırası, kuralların nasıl işleneceğini etkiler. Bir kuralda belirtilen eylemler sonraki bir kural tarafından üzerine yazılabilir.
   >

1. **Kuralı Ekle'yi** seçin ve bir kural adı girin. Kural adları bir harfle başlamalı ve yalnızca sayılar ve harfler içerebilir.

1. Kuralın uygulandığı istek türünü belirlemek için bir eşleşme koşulu oluşturun:
    1. **Koşul Ekle'yi**seçin ve ardından **İstek protokolü** eşleşme koşulunu seçin.
    1. **İşleç** olarak **Eşittir**’i seçin.
    1. **Değer**için **HTTP'yi**seçin.
   
   [![Azure CDN kuralı eşleşme koşulu](./media/cdn-standard-rules-engine/cdn-match-condition.png)](./media/cdn-standard-rules-engine/cdn-match-condition.png#lightbox)
   
   > [!NOTE]
   > **Ekle koşul** açılır listesindeki birden çok eşleşme koşulu arasından seçim yapabilirsiniz. Maç koşullarının ayrıntılı bir listesi için, [Standart kurallar altyapısındaki Eşleşme koşulları bölümüne](cdn-standard-rules-engine-match-conditions.md)bakın.
   
1. Eşleşme koşulunu karşılayan isteklere uygulanacak eylemi seçin:
   1. **Eylem Ekle'yi**seçin ve ardından **URL'yi yeniden yönlendirmeyi**seçin.
   1. **Türü**için , **Found (302)** seçin.
   1. **Protokol**için **HTTPS'yi**seçin.
   1. Gelen değerleri kullanmak için diğer tüm alanları boş bırakın.
   
   [![Azure CDN kural eylemi](./media/cdn-standard-rules-engine/cdn-action.png)](./media/cdn-standard-rules-engine/cdn-action.png#lightbox)
   
   > [!NOTE]
   > Eylem açılır listesine **birden** çok eylem arasından seçim yapabilirsiniz. Eylemlerin ayrıntılı bir listesi için, [Standart kurallar altyapısındaki Eylemler bölümüne](cdn-standard-rules-engine-actions.md)bakın.

6. Yeni kuralı kaydetmek için **Kaydet'i** seçin. Kuralı kullanmak için artık kullanılabilir.
   
   > [!IMPORTANT]
   > Kural değişikliklerinin Azure CDN'de yayılması 15 dakika kadar sürebilir.
   >
   

## <a name="next-steps"></a>Sonraki adımlar

- [Azure CDN'ye genel bakış](cdn-overview.md)
- [Standart kural altyapısı başvurusu](cdn-standard-rules-engine-reference.md)
- [Standart kurallar motorundaki koşulları eşleştirme](cdn-standard-rules-engine-match-conditions.md)
- [Standart kurallar altyapısındaki eylemler](cdn-standard-rules-engine-actions.md)
