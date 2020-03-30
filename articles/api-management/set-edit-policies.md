---
title: Azure API Yönetimi ilkelerini ayarlama veya yönetme | Microsoft Dokümanlar
description: Bu konu, Azure API Yönetimi ilkelerinin nasıl ayarlanış yapılacağını veya nasıl edinilebildiğini gösterir.
services: api-management
documentationcenter: ''
author: mikebudzynski
manager: cflower
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 11/01/2018
ms.author: apimpm
ms.openlocfilehash: 2df57477ae5270405a1774b7a4f04ed185fea396
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "70071694"
---
# <a name="how-to-set-or-edit-azure-api-management-policies"></a>Azure API Management ilkelerini ayarlama veya düzenleme

İlke tanımı, gelen ve giden deyimler dizisi açıklayan bir XML belgesidir. XML doğrudan tanım penceresinde düzenlenebilir. Ayrıca, ilke penceresinin sağında sağlanan listeden önceden tanımlanmış bir ilke seçebilirsiniz. Geçerli kapsam için geçerli ifadeler etkinleştirilir ve vurgulanır. Etkin bir deyimi tıklatmak, tanım görünümünde imlecin bulunduğu yere uygun XML'yi ekler. 

İlkeler hakkında ayrıntılı bilgi için [Azure API Yönetimi'ndeki İlkeler'e](api-management-howto-policies.md)bakın.

## <a name="set-or-edit-a-policy"></a>İlke ayarlama veya ayarlama

Bir ilke ayarlamak veya ayarlamak için aşağıdaki adımları izleyin:

1. Azure portalında oturum [https://portal.azure.com](https://portal.azure.com)aç.
2. APIM örneğinize göz atın.
3. **API'ler** sekmesine tıklayın.

    ![İlkeyi düzenleme](./media/set-edit-policies/code-editor.png)

4. Daha önce içeri aktardığınız API'lerden birini seçin.
5. **Tasarım** sekmesini seçin.
6. İlkeyi uygulamak istediğiniz bir işlem seçin. İlkeyi tüm işlemlere uygulamak istiyorsanız, **Tüm işlemleri**seçin.
7. Gelen **</>** **işleme** veya **Giden işleme** bölümündeki (kod düzenleyicisi) simgesini seçin.
8. İstenilen ilke kodunu uygun bloklardan birine yapıştırın.

    ```XML
    <policies>
        <inbound>
            <base />
        </inbound>
        <backend>
            <base />
        </backend>
        <outbound>
            <base />
        </outbound>
        <on-error>
            <base />
        </on-error>
    </policies>
    ```
 
## <a name="configure-scope"></a>Kapsamı yapılandırma

İlkeler genel olarak veya bir Ürün, API veya Operasyon kapsamında yapılandırılabilir. İlke yapılandırmaya başlamak için öncelikle ilkenin uygulanması gereken kapsamı seçmeniz gerekir.

İlke kapsamları aşağıdaki sırada değerlendirilir:

1. Genel kapsam
2. Ürün kapsamı
3. API kapsamı
4. Çalışma kapsamı

İlkeler içindeki ifadeler, varsa `base` öğenin yerleşimine göre değerlendirilir. Genel ilke hiçbir ana `<base>` ilke vardır ve bu öğeyi kullanarak hiçbir etkisi yoktur.

İlke düzenleyicisindeki geçerli kapsamdaki ilkeleri görmek **için, seçili kapsam için etkin ilkeyi yeniden hesapla'yı**tıklatın.

### <a name="global-scope"></a>Genel kapsam

Genel kapsam, APIM örneğinizdeki **Tüm API'ler** için yapılandırılır.

1. [Azure portalında](https://portal.azure.com/) oturum açın ve APIM örneğinize gidin.
2. **Tüm API'leri**tıklatın.

    ![Genel kapsam](./media/api-management-howto-policies/global-scope.png)

3. Üçgen simgesini tıklatın.
4. **Kod düzenleyicisi**’ni seçin.
5. İlkeler ekleme veya bu ilkeleri edin.
6. **Kaydet**’e basın. 

    Değişiklikler hemen API Yönetimi ağ geçidine yayılır.

### <a name="product-scope"></a>Ürün kapsamı

Ürün kapsamı seçili ürün için yapılandırılır.

1. **Ürünler'i**tıklatın.

    ![Ürün kapsamı](./media/api-management-howto-policies/product-scope.png)

2. İlkeleri uygulamak istediğiniz ürünü seçin.
3. **İlkeler'i**tıklatın.
4. İlkeler ekleme veya bu ilkeleri edin.
5. **Kaydet**’e basın. 

### <a name="api-scope"></a>API kapsamı

API kapsamı, seçili API'nin **Tüm İşlemleri** için yapılandırılır.

1. İlkeleri uygulamak istediğiniz **API'yi** seçin.

    ![API kapsamı](./media/api-management-howto-policies/api-scope.png)

2. **Tüm işlemleri** seçin
3. Üçgen simgesini tıklatın.
4. **Kod düzenleyicisi**’ni seçin.
5. İlkeler ekleme veya bu ilkeleri edin.
6. **Kaydet**’e basın. 

### <a name="operation-scope"></a>Çalışma kapsamı 

İşlem kapsamı seçili işlem için yapılandırılır.

1. Bir **API**seçin.
2. İlkeleri uygulamak istediğiniz işlemi seçin.

    ![Çalışma kapsamı](./media/api-management-howto-policies/operation-scope.png)

3. Üçgen simgesini tıklatın.
4. **Kod düzenleyicisi**’ni seçin.
5. İlkeler ekleme veya bu ilkeleri edin.
6. **Kaydet**’e basın. 

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki ilgili konulara bakın:

+ [API'leri Dönüştür](transform-api.md)
+ [İlke](api-management-policy-reference.md) deyimlerinin ve ayarlarının tam listesi için İlke Başvurusu
+ [İlke örnekleri](policy-samples.md)
