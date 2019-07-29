---
title: Verilerinizi dışarı veya silme-Özel Görüntü İşleme Hizmeti
titleSuffix: Azure Cognitive Services
description: Özel Görüntü İşleme Hizmeti verilerinizi dışarı veya silme hakkında bilgi edinin.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 03/21/2019
ms.author: pafarley
ms.openlocfilehash: b885f359d9416fbc5f778b094610260342a75f65
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68564217"
---
# <a name="export-or-delete-user-data-in-custom-vision"></a>Özel Görüntü İşleme Kullanıcı verilerini dışarı aktarma veya silme

Özel Görüntü İşleme, hizmeti çalıştırmak için Kullanıcı verilerini toplar, ancak müşteriler Özel Görüntü İşleme [eğitim API 'lerini](https://go.microsoft.com/fwlink/?linkid=865446)kullanarak verilerini görüntüleme, dışarı aktarma ve silme konusunda tam denetime sahiptir.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

Özel Görüntü İşleme Kullanıcı verilerini dışarı ve silmeyi öğrenmek için aşağıdaki tabloya bakın.

| Data | Dışarı aktarma Işlemi | İşlemi Siler |
| ---- | ---------------- | ---------------- |
| Hesap bilgileri (abonelik anahtarları) | [GetAccountInfo](https://go.microsoft.com/fwlink/?linkid=865446) | Azure portal kullanarak silin (Azure abonelikleri). Veya CustomVision.ai Settings sayfasındaki "Hesabınızı silme" düğmesini (Microsoft hesabı abonelikleri) kullanarak | 
| Yineleme ayrıntıları | [GetIteration](https://go.microsoft.com/fwlink/?linkid=865446) | [Deleteıteration](https://go.microsoft.com/fwlink/?linkid=865446) |
| Yineleme performansı ayrıntıları | [Getıterationperformance](https://go.microsoft.com/fwlink/?linkid=865446) | [Deleteıteration](https://go.microsoft.com/fwlink/?linkid=865446) | 
| Yineleme listesi | [GetIterations](https://go.microsoft.com/fwlink/?linkid=865446) | [Deleteıteration](https://go.microsoft.com/fwlink/?linkid=865446) |
| Projeler ve proje ayrıntıları | [GetProject](https://go.microsoft.com/fwlink/?linkid=865446) ve [getprojects](https://go.microsoft.com/fwlink/?linkid=865446) | [DeleteProject](https://go.microsoft.com/fwlink/?linkid=865446) | 
| Resim etiketleri | [Gettag](https://go.microsoft.com/fwlink/?linkid=865446) ve [getTags](https://go.microsoft.com/fwlink/?linkid=865446) | [DeleteTag](https://go.microsoft.com/fwlink/?linkid=865446) | 
| Görüntüler | [Gettaggedımages](https://go.microsoft.com/fwlink/?linkid=865446) (görüntü indirme için URI sağlar) ve [Getuntaggedimages](https://go.microsoft.com/fwlink/?linkid=865446) (görüntü indirme için URI sağlar) | [Deleteımages](https://go.microsoft.com/fwlink/?linkid=865446) | 
| Aktarılmış modeller | [GetExports](https://go.microsoft.com/fwlink/?linkid=865446) | Hesap silme işleminden sonra silindi |
