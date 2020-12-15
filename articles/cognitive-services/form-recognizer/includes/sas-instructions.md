---
author: PatrickFarley
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: include
ms.date: 12/11/2020
ms.author: pafarley
ms.openlocfilehash: 8ad682ccd7feb9c7f089e9cf0f066cb1be8c756c
ms.sourcegitcommit: dfc4e6b57b2cb87dbcce5562945678e76d3ac7b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2020
ms.locfileid: "97505333"
---
Özel model eğitim verilerinize yönelik SAS URL 'sini almak için Azure portal depolama kaynağına gidin ve **Depolama Gezgini** sekmesini seçin. Kapsayıcınıza gidin, sağ tıklayın ve **paylaşılan erişim Imzasını al**' ı seçin. Depolama hesabının kendisi için değil, kapsayıcınıza yönelik SAS almak önemlidir. **Okuma** ve **Listeleme** izinlerinin işaretli olduğundan emin olun ve **Oluştur**' a tıklayın. Sonra **URL** bölümündeki değeri geçici bir konuma kopyalayın. Şu biçimde olmalıdır: `https://<storage account>.blob.core.windows.net/<container name>?<SAS value>`.
   > [!div class="mx-imgBorder"]
   > ![alt-text](../media/quickstarts/get-sas-url.png)