---
title: Verilerinizi görüntüleme veya silme-Özel Görüntü İşleme Hizmeti
titleSuffix: Azure Cognitive Services
description: Verileriniz üzerinde tam denetim sahibi olursunuz. Bu makalede, Özel Görüntü İşleme Hizmeti verilerinizi nasıl görüntüleyebileceğiniz, dışarı aktardığınızda veya silebileceğiniz açıklanır.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 03/21/2019
ms.author: pafarley
ms.openlocfilehash: fe17fa4349085be47201974b418493fe8d912ece
ms.sourcegitcommit: 03662d76a816e98cfc85462cbe9705f6890ed638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2020
ms.locfileid: "90527403"
---
# <a name="view-or-delete-user-data-in-custom-vision"></a>Özel Görüntü İşleme Kullanıcı verilerini görüntüleme veya silme

Özel Görüntü İşleme, hizmeti çalıştırmak için Kullanıcı verilerini toplar, ancak müşteriler Özel Görüntü İşleme [eğitim API 'lerini](https://go.microsoft.com/fwlink/?linkid=865446)kullanarak verilerini görüntüleme ve silme konusunda tam denetime sahiptir.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

Özel Görüntü İşleme Kullanıcı verilerini görüntülemeyi ve silmeyi öğrenmek için aşağıdaki tabloya bakın.

| Veriler | İşlemi görüntüle | Silme işlemi |
| ---- | ---------------- | ---------------- |
| Hesap bilgileri (abonelik anahtarları) | [Getaccountınfo](https://go.microsoft.com/fwlink/?linkid=865446) | Azure portal kullanarak silin (Azure abonelikleri). Veya CustomVision.ai Settings sayfasındaki "Hesabınızı silme" düğmesini (Microsoft hesabı abonelikleri) kullanarak | 
| Yineleme ayrıntıları | [GetIteration](https://go.microsoft.com/fwlink/?linkid=865446) | [Deleteıteration](https://go.microsoft.com/fwlink/?linkid=865446) |
| Yineleme performansı ayrıntıları | [Getıterationperformance](https://go.microsoft.com/fwlink/?linkid=865446) | [Deleteıteration](https://go.microsoft.com/fwlink/?linkid=865446) | 
| Yineleme listesi | [GetIterations](https://go.microsoft.com/fwlink/?linkid=865446) | [Deleteıteration](https://go.microsoft.com/fwlink/?linkid=865446) |
| Projeler ve proje ayrıntıları | [GetProject](https://go.microsoft.com/fwlink/?linkid=865446) ve [getprojects](https://go.microsoft.com/fwlink/?linkid=865446) | [DeleteProject](https://go.microsoft.com/fwlink/?linkid=865446) | 
| Resim etiketleri | [Gettag](https://go.microsoft.com/fwlink/?linkid=865446) ve [getTags](https://go.microsoft.com/fwlink/?linkid=865446) | [DeleteTag](https://go.microsoft.com/fwlink/?linkid=865446) | 
| Görüntüler | [Gettaggedimages](https://go.microsoft.com/fwlink/?linkid=865446) (resim indirmesi için URI sağlar) ve [Getuntaggedimages](https://go.microsoft.com/fwlink/?linkid=865446) (görüntü indirmesi için URI sağlar) | [Deleteımages](https://go.microsoft.com/fwlink/?linkid=865446) | 
| Aktarılmış yinelemeler | [GetExports](https://go.microsoft.com/fwlink/?linkid=865446) | Hesap silme işleminden sonra silindi |
