---
title: Azure ön kapı yolunu yapılandırma
description: Bu makalede, etki alanlarınız ve kaynak gruplarınız arasında bir rotanın nasıl yapılandırılacağı gösterilir.
services: frontdoor
author: duongau
ms.service: frontdoor
ms.topic: how-to
ms.date: 02/18/2021
ms.author: qixwang
ms.openlocfilehash: 94c22ffd423c32ba5f489298267464ca36abaecd
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "101100389"
---
# <a name="configure-an-azure-front-door-standardpremium-route"></a>Azure ön kapısının Standart/Premium yolunu yapılandırma

> [!Note]
> Bu belge, Azure ön kapısının Standart/Premium (Önizleme) içindir. Azure ön kapısı hakkında bilgi mi arıyorsunuz? [Burada](../front-door-overview.md)görüntüleyin.

Bu makalede, var olan bir uç nokta için Azure ön kapısı (AFD) yolu oluşturma konusunda kullanılan ayarların her biri açıklanmaktadır. Mevcut Azure ön kapısı uç noktanıza özel bir etki alanı ve kaynak ekledikten sonra, trafiği Aralarındaki trafiği yönlendirmek için etki alanları ve kaynaklar arasındaki ilişkiyi tanımlamak üzere Route 'u yapılandırmanız gerekir.

> [!IMPORTANT]
> Azure ön kapı Standart/Premium (Önizleme) Şu anda genel önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Önkoşullar

Bir Azure ön kapısı yolu yapılandırmadan önce, geçerli uç nokta içinde en az bir kaynak grubu ve bir özel etki alanı oluşturmuş olmanız gerekir. 

Bir kaynak grubu ayarlamak için, bkz. [yeni Azure ön kapısı Standart/Premium kaynak grubu oluşturma](how-to-create-origin.md). 

## <a name="create-a-new-azure-front-door-standardpremium-route"></a>Yeni bir Azure ön kapısı Standart/Premium yolu oluşturma

1. [Azure Portal](https://portal.azure.com) oturum açın ve Azure ön kapısı Standart/Premium profilinize gidin.

1. **Ayarlar** altında **Endpoint Manager** ' ı seçin.
   
    :::image type="content" source="../media/how-to-configure-route/select-endpoint-manager.png" alt-text="Ön kapı uç noktası Yöneticisi ayarlarının ekran görüntüsü." lightbox="../media/how-to-configure-route/select-endpoint-manager-expanded.png":::

1. Ardından, rotayı yapılandırmak istediğiniz uç nokta için **uç noktayı Düzenle** ' yi seçin.
   
    :::image type="content" source="../media/how-to-configure-route/select-edit-endpoint.png" alt-text="Uç noktayı Düzenle seçeneğinin ekran görüntüsü.":::

1. **Uç noktayı Düzenle** sayfası görüntülenir. Rotalar için **+ Ekle** ' yi seçin.
    
    :::image type="content" source="../media/how-to-configure-route/select-add-route.png" alt-text="Uç noktayı düzenleme sayfasında yol ekleme ' nin ekran görüntüsü.":::    
    
1. **Yol Ekle** sayfasında, aşağıdaki bilgileri girin veya seçin.

    :::image type="content" source="../media/how-to-configure-route/add-route-page.png" alt-text="Yol ekleme sayfasının ekran görüntüsü." lightbox="../media/how-to-configure-route/add-route-page-expanded.png"::: 

    | Ayar | Değer |
    | --- | --- |
    | Ad | Yeni yol için benzersiz bir ad girin. |   
    | Etki alanı| Doğrulanan ve başka bir rota ile ilişkilendirilmemiş bir veya daha fazla etki alanı seçin |
    | Eşleştirilecek desenler  | Bu yolun kabul edeceği tüm URL yolu desenlerini yapılandırın. Örneğin, `/images/*` URL 'deki tüm istekleri kabul etmek için bunu olarak ayarlayabilirsiniz `www.contoso.com/images/*` . AFD, ilk olarak tam eşleşmelere göre trafiği belirlemeyi deneyecek, tam eşleşme yolu yoksa eşleşen bir joker karakter yolunu arar. Eşleşen bir yol ile hiçbir yönlendirme kuralı bulunmazsa, isteği reddedin ve 400: Hatalı Istek hatası HTTP yanıtı döndürür. |
    | Kabul edilen protokoller | İstemci isteği yaparken Azure ön kapısının kabul etmesini istediğiniz protokolleri belirtin. |
    | Yeniden yönlendirme | HTTP isteğiyle gelen istek için HTTPS 'nin uygulanıp uygulanmadığını belirtin |
    | Kaynak grubu | Kaynağa geri dön isteği gerçekleştiğinde hangi kaynak grubunun iletilmesi gerektiğini seçin. |
    | Kaynak yolu | Önbelleğe almak istediğiniz kaynakların yolunu girin. Etki alanındaki herhangi bir kaynağın önbelleğe alınmasına izin vermek için bu ayarı boş bırakın. |
    | İletme Protokolü | İstek iletmek için kullanılan protokolü seçin. |
    | Önbelleğe Alma | Azure ön kapısına sahip statik içeriğin önbelleğe alınmasını etkinleştirmek için bu seçeneği belirleyin. |
    | Kural | Bu rotaya uygulanacak kural kümelerini seçin. Kuralların nasıl yapılandırılacağı hakkında daha fazla bilgi için bkz. [Azure ön kapısı için bir kural kümesi yapılandırma](how-to-configure-rule-set.md) | 

1. Yeni yolu oluşturmak için **Ekle** ' yi seçin. Yol, uç nokta için yollar listesinde görünür.
    
    :::image type="content" source="../media/how-to-configure-route/route-list-page.png" alt-text="Rotalar listesinin ekran görüntüsü.":::  
    
## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık ihtiyaç kalmadığında bir yolu silmek için, yolu seçin ve **Sil**' i seçin. 

:::image type="content" source="../media/how-to-configure-route/route-delete-page.png" alt-text="Bir rotanın nasıl silineceği ekran görüntüsü.":::  

## <a name="next-steps"></a>Sonraki adımlar
Özel etki alanları hakkında bilgi edinmek için Azure ön kapısına özel bir etki alanı eklemeye yönelik öğreticiye geçin.

> [!div class="nextstepaction"]
> [Özel etki alanı ekleme]()
