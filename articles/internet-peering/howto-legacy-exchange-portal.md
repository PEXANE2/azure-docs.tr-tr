---
title: Azure portal kullanarak eski bir Exchange eşlemesini Azure kaynağına dönüştürme
titleSuffix: Azure
description: Azure portal kullanarak eski bir Exchange eşlemesini Azure kaynağına dönüştürme
services: internet-peering
author: derekolo
ms.service: internet-peering
ms.topic: article
ms.date: 5/21/2020
ms.author: derekol
ms.openlocfilehash: ca26189709405ca6dc0d2954bd98f0d933963bf4
ms.sourcegitcommit: cf7caaf1e42f1420e1491e3616cc989d504f0902
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/22/2020
ms.locfileid: "83800835"
---
# <a name="convert-a-legacy-exchange-peering-to-an-azure-resource-by-using-the-azure-portal"></a>Azure portal kullanarak eski bir Exchange eşlemesini Azure kaynağına dönüştürme

Bu makalede, Azure portal kullanarak mevcut bir eski Exchange eşlemesinin Azure kaynağına nasıl dönüştürüleceği açıklanır.

İsterseniz, [PowerShell](howto-legacy-exchange-powershell.md)kullanarak bu kılavuzu tamamlayabilirsiniz.

## <a name="before-you-begin"></a>Başlamadan önce
* Yapılandırmaya başlamadan önce [önkoşulları](prerequisites.md) ve [Exchange eşleme](walkthrough-exchange-all.md) Kılavuzu ' nu gözden geçirin.

## <a name="convert-a-legacy-exchange-peering-to-an-azure-resource"></a>Eski bir Exchange eşlemesini Azure kaynağına dönüştürme

Bir Internet Exchange sağlayıcısı olarak, [eşleme oluşturarak]( https://go.microsoft.com/fwlink/?linkid=2129593)doğrudan eşleme isteği oluşturabilirsiniz.

1. **Eşleme oluştur** sayfasında, **temel bilgiler** sekmesinde, kutulara aşağıda gösterildiği gibi kutuyu girin:

>    [!div class="mx-imgBorder"]
>   ![Eşleme Hizmeti'ni kaydetme](./media/setup-basics-tab.png)

* Azure aboneliğinizi seçin.

* Kaynak grubu için, açılan listeden var olan bir kaynak grubunu seçebilir veya yeni oluştur ' u seçerek yeni bir grup oluşturabilirsiniz. Bu örnek için yeni bir kaynak grubu oluşturacağız.

* Ad, kaynak adına karşılık gelir ve seçtiğiniz herhangi bir şey olabilir.

* Mevcut bir kaynak grubunu seçerseniz bölge otomatik olarak seçilir. Yeni bir kaynak grubu oluşturmayı seçerseniz, kaynağın bulunmasını istediğiniz Azure bölgesini de seçmeniz gerekir.

>[!NOTE]
    Kaynak grubunun bulunduğu bölge, Microsoft ile eşleme oluşturmak istediğiniz konumdan bağımsızdır. Ancak, en yakın Azure bölgelerinde bulunan kaynak grupları içindeki eşleme kaynaklarınızı düzenlemek en iyi uygulamadır. Örneğin, Ashburn içindeki eşlemeler için Doğu ABD veya Doğu ABD2 içinde bir kaynak grubu oluşturabilirsiniz.

* **Peerasn** kutusunda ASN 'nizi seçin.

>[!IMPORTANT]  
    Bir eşleme isteği göndermeden önce, yalnızca onay durumu ile bir ASN seçebilirsiniz. PeerAsn isteğinizi gönderdikten sonra, ASN ilişkisinin onaylanabilmesi için 12 saat bekleyin. Seçtiğiniz ASN, doğrulama bekliyor ise, bir hata iletisi görürsünüz. Seçmeniz gereken ASN 'yi görmüyorsanız doğru aboneliği seçtiğinizden emin olun. Bu durumda, **[EŞDÜZEY ASN 'Yi Azure aboneliğine ilişkilendir](https://go.microsoft.com/fwlink/?linkid=2129592)**' i kullanarak peerasn 'yi zaten oluşturmuş olup olmadığınızı kontrol edin.

* **İleri ' yi seçin:** devam etmek için yapılandırma.

### <a name="convert-legacy-exchange-peering"></a><a name=create></a>Eski Exchange eşlemesini Dönüştür

**Eşleme** kaynağını kullanarak eski eşleme bağlantılarını dönüştürebilirsiniz.

#### <a name="launch-the-resource-and-configure-basic-settings"></a>Kaynağı başlatın ve temel ayarları yapılandırın
[!INCLUDE [direct-peering-basic](./includes/direct-portal-basic.md)]

#### <a name="configure-connections-and-submit"></a>Bağlantıları yapılandırma ve gönderme
[!INCLUDE [exchange-peering-configuration](./includes/exchange-portal-configuration-legacy.md)]

### <a name="verify-exchange-peering"></a><a name=get></a>Exchange eşlemesini doğrula
[!INCLUDE [peering-exchange-get-portal](./includes/exchange-portal-get.md)]

## <a name="additional-resources"></a>Ek kaynaklar

Daha fazla bilgi için bkz. [Internet eşlemesi SSS](faqs.md).

## <a name="next-steps"></a>Sonraki adımlar

* [Portalı kullanarak bir Exchange eşlemesi oluşturma veya değiştirme](howto-exchange-portal.md)
