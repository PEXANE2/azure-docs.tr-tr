---
title: Azure portal kullanarak eski bir doğrudan eşlemeyi Azure kaynağına dönüştürme
titleSuffix: Azure
description: Azure portal kullanarak eski bir doğrudan eşlemeyi Azure kaynağına dönüştürme
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: how-to
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: 496774f034c861131624b552b1acefca9f399ea8
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84700272"
---
# <a name="convert-a-legacy-direct-peering-to-an-azure-resource-by-using-the-azure-portal"></a>Azure portal kullanarak eski bir doğrudan eşlemeyi Azure kaynağına dönüştürme

Bu makalede, Azure portal kullanarak mevcut bir eski doğrudan eşlemeyi bir Azure kaynağına nasıl dönüştürebileceğiniz açıklanır.

İsterseniz, [PowerShell](howto-legacy-direct-powershell.md)kullanarak bu kılavuzu tamamlayabilirsiniz.

## <a name="before-you-begin"></a>Başlamadan önce
* Yapılandırmaya başlamadan önce [önkoşulları](prerequisites.md) ve [doğrudan eşleme](walkthrough-direct-all.md) kılavuzunu gözden geçirin.


## <a name="convert-a-legacy-direct-peering-to-an-azure-resource"></a>Eski bir doğrudan eşlemeyi Azure kaynağına dönüştürme

### <a name="sign-in-to-the-portal-and-select-your-subscription"></a>Portalda oturum açın ve aboneliğinizi seçin
[!INCLUDE [Account](./includes/account-portal.md)]

### <a name="convert-a-legacy-direct-peering"></a><a name=create></a>Eski bir doğrudan eşlemeyi Dönüştür

Bir Internet hizmet sağlayıcısı olarak, [eşleme oluşturma]( https://go.microsoft.com/fwlink/?linkid=2129593)kullanarak eski doğrudan eşleme bağlantılarını dönüştürebilirsiniz.

1. **Eşleme oluştur** sayfasında, **temel bilgiler** sekmesinde, kutulara aşağıda gösterildiği gibi kutuyu girin:

    > [!div class="mx-imgBorder"] 
    > ![Eşleme Hizmeti'ni kaydetme](./media/setup-basics-tab.png)

*    Azure aboneliğinizi seçin.

* Kaynak grubu için, açılan listeden var olan bir kaynak grubunu seçebilir veya yeni oluştur ' u seçerek yeni bir grup oluşturabilirsiniz. Bu örnek için yeni bir kaynak grubu oluşturacağız.

* Ad, kaynak adına karşılık gelir ve seçtiğiniz herhangi bir şey olabilir.

* Mevcut bir kaynak grubunu seçerseniz bölge otomatik olarak seçilir. Yeni bir kaynak grubu oluşturmayı seçerseniz, kaynağın bulunmasını istediğiniz Azure bölgesini de seçmeniz gerekir.

>[!NOTE]
>Kaynak grubunun bulunduğu bölge, Microsoft ile eşleme oluşturmak istediğiniz konumdan bağımsızdır. Ancak, en yakın Azure bölgelerinde bulunan kaynak grupları içindeki eşleme kaynaklarınızı düzenlemek en iyi uygulamadır. Örneğin, Ashburn içindeki eşlemeler için Doğu ABD veya Doğu ABD2 içinde bir kaynak grubu oluşturabilirsiniz.

* **Peerasn** kutusunda ASN 'nizi seçin.

>[!IMPORTANT] 
>Bir eşleme isteği göndermeden önce, yalnızca onay durumu ile bir ASN seçebilirsiniz. PeerAsn isteğinizi gönderdikten sonra, ASN ilişkisinin onaylanabilmesi için 12 saat bekleyin. Seçtiğiniz ASN, doğrulama bekliyor ise, bir hata iletisi görürsünüz. Seçmeniz gereken ASN 'yi görmüyorsanız doğru aboneliği seçtiğinizden emin olun. Bu durumda, **[EŞDÜZEY ASN 'Yi Azure aboneliğine ilişkilendir](https://go.microsoft.com/fwlink/?linkid=2129592)**' i kullanarak peerasn 'yi zaten oluşturmuş olup olmadığınızı kontrol edin.

#### <a name="launch-the-resource-and-configure-basic-settings"></a>Kaynağı başlatın ve temel ayarları yapılandırın
[!INCLUDE [direct-peering-basic](./includes/direct-portal-basic.md)]

#### <a name="configure-connections-and-submit"></a>Bağlantıları yapılandırma ve gönderme
[!INCLUDE [direct-peering-configuration](./includes/direct-portal-configuration-legacy.md)]

### <a name="verify-direct-peering"></a><a name=get></a>Doğrudan eşlemeyi doğrula
[!INCLUDE [peering-direct-get-portal](./includes/direct-portal-get.md)]

## <a name="additional-resources"></a>Ek kaynaklar

Daha fazla bilgi için bkz. [Internet eşlemesi SSS](faqs.md).

## <a name="next-steps"></a>Sonraki adımlar

* [Portalı kullanarak doğrudan eşleme oluşturma veya değiştirme](howto-direct-portal.md)
