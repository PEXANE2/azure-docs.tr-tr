---
title: Azure CDN Standart kurallar altyapısını kullanarak HTTPS 'yi zorla | Microsoft Docs
description: Standart kurallar altyapısı, HTTP isteklerinin Microsoft 'un Azure CDN tarafından nasıl işlendiğini, belirli içerik türlerinin teslimini engelleme, önbelleğe alma ilkesi tanımlama ve HTTP üstbilgilerini değiştirme gibi özelleştirmenize olanak tanır.
services: cdn
author: mdgattuso
ms.service: azure-cdn
ms.topic: article
ms.date: 11/01/2019
ms.author: magattus
ms.openlocfilehash: b24c4a04e0c02258a918ee075066d90c22ea0c75
ms.sourcegitcommit: b2fb32ae73b12cf2d180e6e4ffffa13a31aa4c6f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2019
ms.locfileid: "73615929"
---
# <a name="azure-cdn-standard-rules-engine"></a>Standart kurallar altyapısını Azure CDN

> [!NOTE]
> Standart kurallar altyapısı yalnızca Microsoft 'tan Azure CDN için kullanılabilir. 

Azure CDN Standart kuralları altyapısı, HTTP isteklerinin işlenme biçimini özelleştirmenize olanak sağlar. Örneğin, belirli protokoller üzerinden içerik teslimi zorlama, önbelleğe alma ilkesi tanımlama veya bir HTTP üst bilgisini değiştirme. Bu öğreticide, kullanıcılarınızı otomatik olarak HTTPS 'ye yönlendiren bir kuralın nasıl oluşturulacağı gösterilmektedir. 


## <a name="tutorial"></a>Öğretici

1. Microsoft profillerdeki Azure CDN **CDN profili** sayfasında, kurallarını yapılandırmak istediğiniz uç noktayı seçin.
  
2. Sol taraftaki **kurallar altyapısı** sekmesini seçin.
   
    Kural altyapısı dikey penceresi, genel kuralla birlikte görüntülenir. 
   
    [![CDN yeni kurallar sayfası](./media/cdn-standard-rules-engine/cdn-new-rule.png)](./media/cdn-standard-rules-engine/cdn-new-rule.png#lightbox)
   
   > [!IMPORTANT]
   > Birden çok kuralın listelenme sırası, bunların nasıl işlendiğini etkiler. Sonraki bir kural, önceki bir kural tarafından belirtilen eylemleri geçersiz kılabilir.
   >

3. **Kural Ekle** düğmesine tıklayın ve bir kural adı sağlayın. Kural adları bir harfle başlamalıdır ve yalnızca rakam ve harf içermelidir.

4. Kuralın geçerli olduğu isteklerin türünü belirler. Açılan seçeneği kullanarak **İstek Protokolü** eşleştirme koşulunu seçin ve **http**değerine **eşit** değeri kullanın.
   
   [CDN kuralı eşleşme koşulunu ![](./media/cdn-standard-rules-engine/cdn-match-condition.png)](./media/cdn-standard-rules-engine/cdn-match-condition.png#lightbox)
   
   > [!NOTE]
   > Açılan listede birden fazla eşleşme koşulu vardır. Eşleşme koşullarının ayrıntılı bir listesi için bkz. [kural altyapısı eşleştirme koşulları](cdn-standard-rules-engine-match-conditions.md).
   
5. Tanımlanan isteklere uygulanacak eylemi seçin. Açılan menüyü kullanarak **URL yeniden yönlendirme** eylemini seçin ve tür için **bulunan (302)** değeri ve protokol için **https** kullanın. Gelen değerleri kullanmak için diğer tüm alanları boş bırakın.
   
   [CDN kuralı eylemini ![](./media/cdn-standard-rules-engine/cdn-action.png)](./media/cdn-standard-rules-engine/cdn-action.png#lightbox)
   
   > [!NOTE]
   > Açılan listede birden çok eylem var. Eylemlerin ayrıntılı bir listesi için bkz. [kural altyapısı eylemleri](cdn-standard-rules-engine-actions.md).

6. Yeni kuralı kaydetmek için **Kaydet** ' i seçin.  Yeni kural artık dağıtılacak.
   
   > [!IMPORTANT]
   > Kural değişikliklerinin Azure CDN üzerinden yayılması 15 dakikaya kadar sürebilir.
   >
   

## <a name="see-also"></a>Ayrıca bkz.

- [Azure CDN genel bakış](cdn-overview.md)
- [Standart kurallar altyapısı başvurusu](cdn-standard-rules-engine-reference.md)
- [Standart kurallar altyapısı eşleştirme koşulları](cdn-standard-rules-engine-match-conditions.md)
- [Standart kurallar altyapı eylemleri](cdn-standard-rules-engine-actions.md)
