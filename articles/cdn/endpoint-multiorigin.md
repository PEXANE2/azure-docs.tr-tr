---
title: Azure CDN uç noktası çoklu kaynak
description: Azure CDN uç noktası birden çok kaynağı kullanmaya başlayın.
services: cdn
author: asudbring
manager: KumudD
ms.service: azure-cdn
ms.topic: how-to
ms.date: 5/8/2020
ms.author: allensu
ms.openlocfilehash: 628a284335b7285a2ee989511b130fb7d4d3cde6
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83597880"
---
# <a name="azure-cdn-endpoint-multi-origin"></a>Azure CDN uç noktası çoklu kaynak

Birden çok çıkış desteğiyle, bir Azure CDN uç noktası içinde birden fazla çıkış seçerek genel artıklık oluşturabilir ve kapalı kalma süresini ortadan kaldırabilirsiniz. Her bir kaynağın sistem durumunu inceleyerek ve gerektiğinde yük devreterek çok kaynaklı formalar tarafından sunulan yedeklilik riskidir. Çoklu kaynak ayarlamak için bir veya daha fazla kaynak grubu kurun. Her kaynak grubu, benzer iş yükleri alan bir veya daha fazla çıkış koleksiyonudur.

> [!NOTE]
> Şu anda bu özellik yalnızca Microsoft 'tan Azure CDN kullanılabilir. 

## <a name="create-the-origin-group"></a>Kaynak grubu oluşturma

1. [Azure Portal](https://portal.azure.com) oturum açın

2. Azure CDN profilinizi seçin ve ardından çoklu kaynak için yapılandırılacak uç noktayı seçin.

3. Uç nokta yapılandırmasındaki **Ayarlar** bölümünden **kaynak** ' ı seçin:

    :::image type="content" source="./media/endpoint-multiorigin/endpoint-multiorigin-1.png" alt-text="Azure CDN çoklu kaynak" border="true":::

4. Çoklu kaynak özelliğini etkinleştirmek için en az bir kaynak grubunuz olması gerekir. **Kaynak grubu oluştur**' u seçin:

    :::image type="content" source="./media/endpoint-multiorigin/endpoint-multiorigin-2.png" alt-text="Azure CDN çoklu kaynak" border="true":::

5. **Kaynak grubu yapılandırması Ekle** ' de aşağıdaki bilgileri girin veya seçin:

   | Ayar           | Değer                                                                 |
   |-------------------|-----------------------------------------------------------------------|
   | Kaynak grubu adı | Kaynak grubunuz için bir ad girin.                                   |
   | Araştırma yolu        | Sistem durumunu belirlemede kullanılan kaynak yolu. |
   | Araştırma aralığı    | 1, 2 veya 4 dakikalık bir yoklama aralığı seçin.                        |
   | Araştırma Protokolü    | **Http** veya **https**seçin.                                         |
   | Araştırma yöntemi      | **Baş** veya **Al**seçeneğini belirleyin.                                           |
    
   :::image type="content" source="./media/endpoint-multiorigin/endpoint-multiorigin-3.png" alt-text="Azure CDN çoklu kaynak" border="true":::

6. **Add (Ekle)** seçeneğini belirleyin.

7. Varsayılan kaynak grubunu seçmek için, **kaynak grubunu yapılandır**' ı seçin:

    :::image type="content" source="./media/endpoint-multiorigin/endpoint-multiorigin-4.png" alt-text="Azure CDN çoklu kaynak" border="true":::

8. Açılır kutuda kaynak grubunuzu seçin ve **Tamam**' ı seçin.

## <a name="add-multiple-origins"></a>Birden çok çıkış Ekle

1. Uç noktanız için kaynak ayarları ' nda **+ kaynak oluştur**' u seçin.

    :::image type="content" source="./media/endpoint-multiorigin/endpoint-multiorigin-5.png" alt-text="Azure CDN çoklu kaynak" border="true":::

2. **Kaynak Yapılandırması Ekle** ' de aşağıdaki bilgileri girin veya seçin:

   | Ayar           | Değer                                                                 |
   |-------------------|-----------------------------------------------------------------------|
   | Kaynak türü | **Depolama**, **bulut hizmeti**, **Web uygulaması**veya **özel başlangıç**seçin.                                   |
   | Çıkış noktası konağı        | Kaynak ana bilgisayar adını seçin veya girin.  Açılan listede, önceki ayarda belirttiğiniz türün tüm kullanılabilir kaynakları listelenir. Kaynak türü olarak **özel kaynak** ' ı seçtiyseniz, müşteri kaynak sunucunuzun etki alanını girin. |
   | Kaynak barındırma üst bilgisi    | Her istekle göndermek Azure CDN istediğiniz ana bilgisayar üst bilgisini girin veya varsayılan ' ı bırakın.                        |
   | HTTP bağlantı noktası   | HTTP bağlantı noktasını girin.                                         |
   | HTTPS bağlantı noktası     | HTTPS bağlantı noktasını girin.                                           |
   | Öncelik    | 1 ile 5 arasında bir sayı girin.       |
   | Ağırlık      | 1 ile 1000 arasında bir sayı girin.   |

    > [!NOTE]
    > Kaynak grubu içinde bir kaynak oluşturulduğunda, accorded bir öncelik ve ağırlık olmalıdır. Kaynak grubunun yalnızca bir kaynağı varsa, varsayılan öncelik ve ağırlık 1 olarak ayarlanır. Kaynak sağlıklı ise trafik en yüksek öncelik kaynaklarına yönlendirilir. Bir kaynağın sağlıksız olduğu belirlenirse, bağlantılar öncelik sırasına göre başka bir kaynağa atanır. İki kaynak aynı önceliğe sahip ise, trafik, kaynak için belirtilen ağırlığa göre dağıtılır 

    :::image type="content" source="./media/endpoint-multiorigin/endpoint-multiorigin-6.png" alt-text="Azure CDN çoklu kaynak" border="true":::

3. **Add (Ekle)** seçeneğini belirleyin.

4. Tüm kaynaklar için kaynak yolunu ayarlamak üzere **Kaynağı Yapılandır** ' ı seçin:

    :::image type="content" source="./media/endpoint-multiorigin/endpoint-multiorigin-7.png" alt-text="Azure CDN çoklu kaynak" border="true":::

5. **Tamam**’ı seçin.

## <a name="configure-origins-and-origin-group-settings"></a>Kaynakları ve kaynak grubu ayarlarını yapılandırma

Birkaç çıkış ve bir kaynak grubu olduktan sonra, kaynakları farklı gruplara ekleyebilir veya kaldırabilirsiniz. Aynı grup içindeki kaynaklar benzer iş yükleri için kullanılmalıdır. Trafik, sağlıklı durum, öncelik ve ağırlık değerine bağlı olarak bu kaynaklardan dağıtılır. 

1. Azure CDN uç noktasının kaynak ayarlarında, **kaynak grubunu yapılandır**' ı seçin:

    :::image type="content" source="./media/endpoint-multiorigin/endpoint-multiorigin-8.png" alt-text="Azure CDN çoklu kaynak" border="true":::

2. Açılır kutuda yapılandırmak istediğiniz kaynak grubunu seçin ve **Tamam**' ı seçin.

3. **Güncelleştirme kaynak grubu**' nda **+ Kaynak Ekle**: ' yi seçin.

    :::image type="content" source="./media/endpoint-multiorigin/endpoint-multiorigin-9.png" alt-text="Azure CDN çoklu kaynak" border="true":::

4. Açılır kutuda gruba eklenecek kaynağı seçin ve **Tamam**' ı seçin.

5. Kaynağı gruba eklendiğini doğrulayıp **Kaydet**' i seçin:

    :::image type="content" source="./media/endpoint-multiorigin/endpoint-multiorigin-10.png" alt-text="Azure CDN çoklu kaynak" border="true":::

## <a name="remove-origin-from-origin-group"></a>Kaynağı kaynak grubundan kaldır

1. Azure CDN uç noktasının kaynak ayarlarında, **kaynak grubunu yapılandır**' ı seçin:

    :::image type="content" source="./media/endpoint-multiorigin/endpoint-multiorigin-8.png" alt-text="Azure CDN çoklu kaynak" border="true":::

2. Açılır kutuda yapılandırmak istediğiniz kaynak grubunu seçin ve **Tamam**' ı seçin.

3. Kaynak grubundan bir kaynağı kaldırmak için, kaynak yanındaki çöp kutusu simgesini seçin ve **Kaydet**' i seçin:

    :::image type="content" source="./media/endpoint-multiorigin/endpoint-multiorigin-11.png" alt-text="Azure CDN çoklu kaynak" border="true":::

## <a name="next-steps"></a>Sonraki Adımlar
Bu makalede, uç nokta çoklu kaynak Azure CDN etkinleştirdiniz.

Azure CDN ve bu makalede bahsedilen diğer Azure hizmetleri hakkında daha fazla bilgi için bkz.:

* [Azure CDN](./cdn-overview.md)
* [Azure CDN ürün özelliğini karşılaştırın](./cdn-features.md)
