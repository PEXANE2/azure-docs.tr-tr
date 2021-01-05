---
author: PatrickFarley
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: include
ms.date: 12/11/2020
ms.author: pafarley
ms.openlocfilehash: 3f4272f5445c8530e97726bd4fdca6bf662719c2
ms.sourcegitcommit: 02ed9acd4390b86c8432cad29075e2204f6b1bc3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/29/2020
ms.locfileid: "97807816"
---
Özel model eğitim verilerinize yönelik SAS URL 'sini almak için Azure portal depolama kaynağına gidin ve **Depolama Gezgini** sekmesini seçin. Kapsayıcınıza gidin, sağ tıklayın ve **paylaşılan erişim Imzasını al**' ı seçin. Depolama hesabının kendisi için değil, kapsayıcınıza yönelik SAS almak önemlidir. **Okuma** ve **Listeleme** izinlerinin işaretli olduğundan emin olun ve **Oluştur**' a tıklayın. Sonra **URL** bölümündeki değeri geçici bir konuma kopyalayın. Şu biçimde olmalıdır: `https://<storage account>.blob.core.windows.net/<container name>?<SAS value>`.