---
title: Azure ön kapısının Standart/Premium (Önizleme) kaynağını ayarlama
description: Bu makalede Endpoint Manager ile bir Origin 'in nasıl yapılandırılacağı gösterilmektedir.
services: frontdoor
author: duongau
ms.service: frontdoor
ms.topic: how-to
ms.date: 02/18/2021
ms.author: qixwang
ms.openlocfilehash: ebc71ea2d354caf0c8f31b1231ecc1487237dd29
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101741897"
---
# <a name="set-up-an-azure-front-door-standardpremium-preview-origin"></a>Azure ön kapısının Standart/Premium (Önizleme) kaynağını ayarlama

> [!Note]
> Bu belge, Azure ön kapısının Standart/Premium (Önizleme) içindir. Azure ön kapısı hakkında bilgi mi arıyorsunuz? [Burada](../front-door-overview.md)görüntüleyin.

Bu makalede, var olan bir kaynak grubunda Azure ön kapısı Standart/Premium kaynağı oluşturma işlemlerinin nasıl yapılacağı gösterilir. 

> [!IMPORTANT]
> Azure ön kapı Standart/Premium (Önizleme) Şu anda genel önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Önkoşullar

Bir Azure ön kapısı Standart/Premium kaynağı oluşturabilmeniz için önce en az bir kaynak grubu oluşturmuş olmanız gerekir.

## <a name="create-a-new-azure-front-door-standardpremium-origin"></a>Yeni bir Azure ön kapısı Standart/Premium kaynağı oluşturma

1. [Azure Portal](https://portal.azure.com) oturum açın ve Azure ön kapısı Standart/Premium profilinize gidin.

1. **Kaynak grubu**' nu seçin. Ardından **+ Ekle** ' yi seçerek yeni bir kaynak grubu oluşturun.
   
    :::image type="content" source="../media/how-to-create-origin/select-add-origin.png" alt-text="Kaynak grubu giriş sayfasının ekran görüntüsü.":::

1. **Kaynak grubu Ekle** sayfasında, yeni kaynak grubu için benzersiz bir **ad** girin.

1. Ardından, bu kaynak grubuna yeni bir kaynak eklemek için **+ Kaynak Ekle** ' yi seçin. 

    :::image type="content" source="../media/how-to-create-origin/add-origin-view.png" alt-text="Kaynak Ekle sayfasının ekran görüntüsü.":::
  
    | Ayar | Değer |
    | --- | --- |
    | Ad | Yeni Azure ön kapı kaynağı için benzersiz bir ad girin. |   
    | Kaynak türü | Eklemek istediğiniz kaynak türü. Azure ön kapısının Standart/Premium uygulaması, App Service, bulut hizmeti veya depolama alanından uygulama başlangıcının otomatik olarak kullanılmasını destekler. Azure 'da veya Azure dışı arka uçta farklı bir kaynak istiyorsanız **özel ana bilgisayar**' ı seçin. |
    | Konak Adı  | Kaynak ana bilgisayar türü için **özel ana bilgisayar** seçmediyseniz, açılan listeden kaynak ana bilgisayar adını seçerek arka ucunuzu seçin. |
    | Kaynak ana bilgisayar üst bilgisi | Her istek için arka uca gönderilen ana bilgisayar üst bilgi değerini girin. Daha fazla bilgi için bkz. [kaynak ana bilgisayar üstbilgisi](concept-origin.md#hostheader). |
    | HTTP bağlantı noktası | HTTP protokolü için kaynağın desteklediği bağlantı noktası için bir değer girin. |
    | HTTPS bağlantı noktası | Kaynağın HTTPS protokolü için desteklediği bağlantı noktası için bir değer girin. |
    | Öncelik | Tüm trafik için birincil hizmet kaynağını kullanmak istediğinizde, farklı kaynağınıza öncelikler atayın. Ayrıca birincil veya yedek kaynağı kullanılamıyorsa yedeklemeler sağlayın. Daha fazla bilgi için bkz. [Öncelik](concept-origin.md#priority). |
    | Ağırlık | Trafiği, eşit veya ağırlık katlarına göre bir çıkış kümesi üzerinden dağıtmak için farklı kaynaklarına ağırlık atayın. Daha fazla bilgi için bkz. [ağırlıklar](concept-origin.md#weighted). |
    | Durum | Kaynağı etkinleştirmek için bu seçeneği belirleyin. |
    | Kural | Bu rotaya uygulanacak kural kümelerini seçin. Kuralların nasıl yapılandırılacağı hakkında daha fazla bilgi için bkz. [Azure ön kapısı için bir kural kümesi yapılandırma](how-to-configure-rule-set.md) | 

    > [!IMPORTANT]
    > Yapılandırma sırasında, kaynak ön kapı ortamlarından erişilemezse API 'Ler doğrulanmaz. Ön kapısının kaynağınıza ulaşabildiğinizden emin olun.

1. Yeni kaynak oluşturmak için **Ekle** ' yi seçin. Oluşturulan kaynak, grupla birlikte kaynak listesinde görünmelidir.
  
    :::image type="content" source="../media/how-to-create-origin/origin-list-view.png" alt-text="Liste görünümündeki kaynak ekran görüntüsü.":::

1. Kaynak grubunu geçerli uç noktaya eklemek için **Ekle** ' yi seçin. Kaynak grubu, kaynak grup paneli içinde görünmelidir.

## <a name="clean-up-resources"></a>Kaynakları temizleme
Artık ihtiyaç kalmadığında bir kaynak grubunu silmek için **.** .. öğesine tıklayın ve ardından açılan listeden **Sil** ' i seçin.

:::image type="content" source="../media/how-to-create-origin/delete-origin-group.png" alt-text="Kaynak grubunun nasıl silineceğini gösteren ekran görüntüsü.":::

Artık ihtiyaç kalmadığında bir kaynağı silmek için **.** .. öğesine tıklayın ve ardından açılan listeden **Sil** ' i seçin. 

:::image type="content" source="../media/how-to-create-origin/delete-origin-view.png" alt-text="Bir menşei silmenin ekran görüntüsü.":::

## <a name="next-steps"></a>Sonraki adımlar

Özel etki alanları hakkında bilgi edinmek için bkz. Azure ön kapı Standart/Premium uç noktanıza [özel etki alanı ekleme](how-to-add-custom-domain.md) .
