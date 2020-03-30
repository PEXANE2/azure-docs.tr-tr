---
title: Azure Haritalar'a veri geri bildirimi sağlayın | Microsoft Azure Haritaları
description: Microsoft Azure Haritalar geri bildirim aracını kullanarak veri geri bildirimi sağlayın.
author: philmea
ms.author: philmea
ms.date: 08/19/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.custom: mvc
ms.openlocfilehash: 92469370f1ea64f5ee1bc2a84e47cc8b294b5375
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80335338"
---
# <a name="provide-data-feedback-to-azure-maps"></a>Azure Haritalar'a veri geri bildirimi sağlama

Azure Haritalar Mayıs 2018'den beri kullanılabilir. Azure Haritalar, kurumsal müşterilerimizi farklı türde iş kullanım örnekleriyle desteklemek için yeni harita verileri, kullanımı kolay REST API'leri ve güçlü SDK'lar sağlamaktadır. Gerçek dünya her saniye değişiyor ve müşterilerimize gerçek bir dijital temsil sunmak bizim için çok önemli. Tesis açmayı veya kapatmayı planlayan müşterilerimizin haritalarımızın derhal güncellenebisi gerekmektedir. Böylece, doğru tesislerde teslimat, bakım veya müşteri hizmetlerini verimli bir şekilde planlayabilirler. Müşterilerimizin doğrudan veri geri bildirimi sağlamalarını sağlamak için Azure Haritalar veri geri bildirim sitesini oluşturduk. Müşterilerin veri geri bildirimleri doğrudan veri sağlayıcılarımıza ve harita düzenleyicilerine gider. Geri bildirimleri hızlı bir şekilde değerlendirebilir ve haritalama ürünlerimize dahil edebilirler.  

[Azure Haritalar Veri geri bildirim sitesi,](https://feedback.azuremaps.com) müşterilerimizin özellikle ilgi çekici iş noktaları ve konut adresleri hakkında harita veri geri bildirimi sağlamaları için kolay bir yol sağlar. Bu makale, Azure Haritalar geri bildirim sitesini kullanarak farklı türde geri bildirimlerin nasıl sağlayabileceğiniz konusunda size yol göstermektedir.

## <a name="add-a-business-place-or-a-residential-address"></a>Bir iş yeri veya yerleşim adresi ekleme 

Eksik bir ilgi noktası veya bir konut adresi hakkında geri bildirimde bulunabilirsiniz. Bunu yapmanın iki yolu vardır. Azure Haritası veri geri bildirim sitesini açın, eksik konumun koordinatlarını arayın ve ardından "Yer ekle"yi tıklatın

  ![eksik konumu arama](./media/how-to-use-feedback-tool/search-poi.png)

Veya haritayla etkileşimkurabilirsiniz. Koordinata bir pin bırakmak için konuma tıklayın ve "Yer ekle"yi tıklatın.

  ![pin ekle](./media/how-to-use-feedback-tool/add-poi.png)

Tıkladıktan sonra, yer için ilgili ayrıntıları sağlamak için bir forma yönlendirilirsiniz.

  ![yer ekleme](./media/how-to-use-feedback-tool/add-a-place.png)

## <a name="fix-a-business-place-or-a-residential-address"></a>Bir iş yerini veya yerleşim adresini düzeltme 

Geri bildirim sitesi ayrıca bir iş yerini veya adresi aramanızı ve bulmanızı sağlar. Doğru değilse adresi veya pin konumunu düzeltmek için geri bildirim sağlayabilirsiniz. Adresi düzeltmek için geri bildirim sağlamak için, bir iş yeri veya yerleşim adresini aramak için arama çubuğunu kullanın. Sonuç listesinden ilgi alanınızın bulunduğu yere tıklayın. "Burayı düzeltin" seçeneğini tıklayın.

  ![düzeltmek için arama yeri](./media/how-to-use-feedback-tool/fix-place.png)

Adresi düzeltmek için geri bildirim sağlamak için "Yer düzelt" formunu doldurun ve ardından "gönder" düğmesine tıklayın.

  ![düzeltme formu](./media/how-to-use-feedback-tool/fix-form.png)

Yerin pin konumu yanlışsa, "Yeri düzelt" formundaki onay kutusunu işaretleyin ve üzerinde "Pin konumu yanlıştır" yazan bir konum dur. Pimi doğru konuma taşıyın ve ardından "gönder" düğmesini tıklatın.

  ![pin konumunu taşıma](./media/how-to-use-feedback-tool/move-pin.png)

## <a name="add-a-comment"></a>Yorum ekle 

Geri bildirim aracı, konumaramanızı beklemenin yanı sıra, konumla ilgili ayrıntılar için ücretsiz biçimli bir metin yorumu eklemenize de olanak tanır. Yorum eklemek için konumu arayın veya konumu tıklatın. "Yorum ekle"yi tıklatın, yorum yazın ve ardından "Gönder"i tıklatın.

  ![yorum ekle](./media/how-to-use-feedback-tool/add-comment.png)

## <a name="track-status"></a>Durumu izleme 

Ayrıca, "Durumu izlemek istiyorum" kutusunu işaretleyerek ve istekte bulunduktan sonra e-postanızı sağlayarak isteğinizin durumunu izleyebilirsiniz. E-postada, isteğinizin güncel durumunu belirten bir izleme bağlantısı alırsınız. 

  ![geri bildirim durumu](./media/how-to-use-feedback-tool/feedback-status.png)


## <a name="next-steps"></a>Sonraki adımlar

Azure Haritalar ile ilgili teknik soruları yayınlamak için şu adresi ziyaret edin:

* [Azure Haritalar Yığın Taşma](https://stackoverflow.com/questions/tagged/azure-maps)
* [Azure Haritalar geri bildirim forumu](https://feedback.azure.com/forums/909172-azure-maps)
