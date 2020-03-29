---
title: Verilerinizi dışa aktarma veya silme - Özel Vizyon Hizmeti
titleSuffix: Azure Cognitive Services
description: Verileriniz üzerinde tam denetime devam emlabilirsiniz. Bu makalede, Özel Vizyon Hizmeti'nde verilerinizi nasıl görüntüleyebilir, dışa aktarabileceğiniz veya silebileceğiniz açıklanmaktadır.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 03/21/2019
ms.author: pafarley
ms.openlocfilehash: 82d9f4508db376ebbe69ef772c15fb732391a31d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "73718976"
---
# <a name="export-or-delete-user-data-in-custom-vision"></a>Özel Görme'de kullanıcı verilerini dışa aktarma veya silme

Custom Vision hizmeti çalıştırmak için kullanıcı verilerini toplar, ancak müşteriler Özel Görme [Eğitim API'lerini](https://go.microsoft.com/fwlink/?linkid=865446)kullanarak verilerini görüntüleme, dışa aktarma ve silme üzerinde tam kontrole sahiptir.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

Özel Görme'de kullanıcı verilerini nasıl dışa aktarıp silebilirsiniz öğrenmek için aşağıdaki tabloya bakın.

| Veri | İhracat Operasyonu | Silme İşlemi |
| ---- | ---------------- | ---------------- |
| Hesap Bilgileri (Abonelik Anahtarları) | [GetAccountInfo](https://go.microsoft.com/fwlink/?linkid=865446) | Azure portalLarını kullanarak silin (Azure Abonelikleri). Veya CustomVision.ai ayarlar sayfasındaki "Hesabınızı Sil" düğmesini (Microsoft Hesap Abonelikleri) kullanma | 
| Yineleme Ayrıntıları | [GetIteration](https://go.microsoft.com/fwlink/?linkid=865446) | [Silme](https://go.microsoft.com/fwlink/?linkid=865446) |
| Yineleme Performans Detayları | [GetIterationPerformans](https://go.microsoft.com/fwlink/?linkid=865446) | [Silme](https://go.microsoft.com/fwlink/?linkid=865446) | 
| Yinelemeler listesi | [GetIterations](https://go.microsoft.com/fwlink/?linkid=865446) | [Silme](https://go.microsoft.com/fwlink/?linkid=865446) |
| Projeler ve proje detayları | [GetProject](https://go.microsoft.com/fwlink/?linkid=865446) ve [GetProjects](https://go.microsoft.com/fwlink/?linkid=865446) | [Proje silme](https://go.microsoft.com/fwlink/?linkid=865446) | 
| Resim etiketleri | [GetTag](https://go.microsoft.com/fwlink/?linkid=865446) ve [GetTags](https://go.microsoft.com/fwlink/?linkid=865446) | [Silme Etiketi](https://go.microsoft.com/fwlink/?linkid=865446) | 
| Görüntüler | [GetTaggedImages](https://go.microsoft.com/fwlink/?linkid=865446) (resim indirme için uri sağlar) ve [GetUntaggedImages](https://go.microsoft.com/fwlink/?linkid=865446) (resim indirme için uri sağlar) | [Görüntüleri Silme](https://go.microsoft.com/fwlink/?linkid=865446) | 
| Dışa Aktarılan Modeller | [GetExports](https://go.microsoft.com/fwlink/?linkid=865446) | Hesap silinmesi üzerine silindi |
