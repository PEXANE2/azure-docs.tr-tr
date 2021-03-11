---
author: PatrickFarley
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: include
ms.date: 12/11/2020
ms.author: pafarley
ms.openlocfilehash: 45ac0e74733ade9801a6f3624d6200c594eff698
ms.sourcegitcommit: d135e9a267fe26fbb5be98d2b5fd4327d355fe97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/10/2021
ms.locfileid: "102623370"
---
Özel model eğitim verilerinize yönelik SAS URL 'sini almak için Azure portal depolama kaynağına gidin ve **Depolama Gezgini** sekmesini seçin. Kapsayıcınıza gidin, sağ tıklayın ve **paylaşılan erişim Imzasını al**' ı seçin. Depolama hesabının kendisi için değil, kapsayıcınıza yönelik SAS almak önemlidir. **Okuma**, **yazma**, **silme** ve **Listeleme** izinlerinin işaretli olduğundan emin olun ve **Oluştur**' a tıklayın. Sonra **URL** bölümündeki değeri geçici bir konuma kopyalayın. Şu biçimde olmalıdır: `https://<storage account>.blob.core.windows.net/<container name>?<SAS value>`.
