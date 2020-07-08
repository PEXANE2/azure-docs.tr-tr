---
title: Azure portal kullanarak doğrudan eşleme oluşturma veya değiştirme
titleSuffix: Azure
description: Azure portal kullanarak doğrudan eşleme oluşturma veya değiştirme
services: internet-peering
author: derekolo
ms.service: internet-peering
ms.topic: how-to
ms.date: 5/19/2020
ms.author: derekol
ms.openlocfilehash: e49462612b58163c2ac51b78584761d0d8b8bd06
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84700574"
---
# <a name="create-or-modify-a-direct-peering-by-using-the-azure-portal"></a>Azure portal kullanarak doğrudan eşleme oluşturma veya değiştirme

Bu makalede, Azure portal kullanarak bir Internet hizmeti sağlayıcısı veya Internet Exchange sağlayıcısı için Microsoft doğrudan eşleme oluşturma açıklanmaktadır. Bu makalede ayrıca kaynağın durumunu denetleme, güncelleştirme veya silme ve sağlama işlemlerinin nasıl yapılacağı gösterilir.

İsterseniz, bu kılavuzu Azure [PowerShell](howto-direct-powershell.md)kullanarak tamamlayabilirsiniz.

## <a name="before-you-begin"></a>Başlamadan önce
* Yapılandırmaya başlamadan önce [önkoşulları](prerequisites.md) ve [doğrudan eşleme](walkthrough-direct-all.md) kılavuzunu gözden geçirin.
* Azure kaynaklarına Dönüştürülmeyen Microsoft ile doğrudan eşleme bağlantılarınız varsa, bkz. [portalı kullanarak eski bir doğrudan eşlemeyi Azure kaynağına dönüştürme](howto-legacy-direct-portal.md).

## <a name="create-and-provision-a-direct-peering"></a>Doğrudan eşleme oluşturma ve sağlama

### <a name="sign-in-to-the-portal-and-select-your-subscription"></a>Portalda oturum açın ve aboneliğinizi seçin
[!INCLUDE [Account](./includes/account-portal.md)]

### <a name="create-a-direct-peering"></a><a name=create></a>Doğrudan eşleme oluşturma

Bir Internet hizmet sağlayıcısı veya Internet Exchange sağlayıcısı olarak, [eşleme oluşturarak]( https://go.microsoft.com/fwlink/?linkid=2129593)yeni bir doğrudan eşleme isteği oluşturabilirsiniz.

1. **Eşleme oluştur** sayfasında, **temel bilgiler** sekmesinde, kutulara aşağıda gösterildiği gibi kutuyu girin:


    ![Eşleme Hizmeti'ni kaydetme](./media/setup-basics-tab.png)

2. Azure aboneliğinizi seçin.

3. Kaynak grubu için, açılan listeden var olan bir kaynak grubunu seçebilir veya yeni oluştur ' u seçerek yeni bir grup oluşturabilirsiniz. Bu örnek için yeni bir kaynak grubu oluşturacağız.

4. Ad, kaynak adına karşılık gelir ve seçtiğiniz herhangi bir şey olabilir.

5. Mevcut bir kaynak grubunu seçerseniz bölge otomatik olarak seçilir. Yeni bir kaynak grubu oluşturmayı seçerseniz, kaynağın bulunmasını istediğiniz Azure bölgesini de seçmeniz gerekir.

    >[!NOTE]
    > Kaynak grubunun bulunduğu bölge, Microsoft ile eşleme oluşturmak istediğiniz konumdan bağımsızdır. Ancak, en yakın Azure bölgelerinde bulunan kaynak grupları içindeki eşleme kaynaklarınızı düzenlemek en iyi uygulamadır. Örneğin, Ashburn içindeki eşlemeler için Doğu ABD veya Doğu ABD2 içinde bir kaynak grubu oluşturabilirsiniz.

6. **Peerasn** kutusunda ASN 'nizi seçin.

    >[!IMPORTANT]
    >Bir eşleme isteği göndermeden önce, yalnızca onay durumu ile bir ASN seçebilirsiniz. PeerAsn isteğinizi gönderdikten sonra, ASN ilişkisinin onaylanabilmesi için 12 saat bekleyin. Seçtiğiniz ASN, doğrulama bekliyor ise, bir hata iletisi görürsünüz. Seçmeniz gereken ASN 'yi görmüyorsanız doğru aboneliği seçtiğinizden emin olun. Bu durumda, **[EŞDÜZEY ASN 'Yi Azure aboneliğine ilişkilendir](https://go.microsoft.com/fwlink/?linkid=2129592)**' i kullanarak peerasn 'yi zaten oluşturmuş olup olmadığınızı kontrol edin.

7. **İleri ' yi seçin:** devam etmek için yapılandırma.



    ![Eşleme Hizmeti'ni kaydetme](./media/setup-direct-basics-filled-tab.png)


#### <a name="configure-connections-and-submit"></a>Bağlantıları yapılandırma ve gönderme
[!INCLUDE [direct-peering-configuration](./includes/direct-portal-configuration.md)]

### <a name="verify-direct-peering"></a><a name=get></a>Doğrudan eşlemeyi doğrula
[!INCLUDE [peering-direct-get-portal](./includes/direct-portal-get.md)]

## <a name="modify-a-direct-peering"></a><a name="modify"></a>Doğrudan eşlemeyi değiştirme
[!INCLUDE [peering-direct-modify-portal](./includes/direct-portal-modify.md)]

## <a name="deprovision-a-direct-peering"></a><a name="delete"></a>Doğrudan eşleme sağlamayı kaldırma
[!INCLUDE [peering-direct-delete-portal](./includes/delete.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [Portalı kullanarak Exchange eşlemesi oluşturma veya değiştirme](howto-exchange-portal.md)
* [Portalı kullanarak eski bir Exchange eşlemesini Azure kaynağına dönüştürme](howto-legacy-exchange-portal.md)

## <a name="additional-resources"></a>Ek kaynaklar

Daha fazla bilgi için bkz. [Internet eşlemesi SSS](faqs.md).
