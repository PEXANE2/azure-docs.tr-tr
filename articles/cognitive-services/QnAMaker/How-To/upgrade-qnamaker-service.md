---
title: Soru-Cevap Oluşturma hizmetinizi yükseltin-Soru-Cevap Oluşturma
titleSuffix: Azure Cognitive Services
description: Kaynakları daha iyi yönetmek için Soru-Cevap Oluşturma hizmetlerinizi paylaşma veya yükseltme.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 03/25/2019
ms.author: diberry
ms.openlocfilehash: df4aa2d6a3c4690fb1fc38b0f4f7d49afccdd657
ms.sourcegitcommit: 36e9cbd767b3f12d3524fadc2b50b281458122dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2019
ms.locfileid: "69640478"
---
# <a name="share-or-upgrade-your-qna-maker-service"></a>Soru-Cevap Oluşturma hizmetinizi paylaşma veya yükseltme
Kaynakları daha iyi yönetmek için Soru-Cevap Oluşturma hizmetlerinizi paylaşma veya yükseltme. 

İlk oluşturulduktan sonra Soru-Cevap Oluşturma yığının tek tek bileşenlerini yükseltmeyi seçebilirsiniz. Bağımlı bileşenlerin ve SKU seçiminin ayrıntılarına bakın. [](https://aka.ms/qnamaker-docs-capacity)

## <a name="share-existing-services-with-qna-maker"></a>Mevcut hizmetleri Soru-Cevap Oluşturma ile paylaşma

Soru-Cevap Oluşturma çeşitli Azure kaynakları oluşturur. Yönetim ve maliyet paylaşımının avantajlarından yararlanmak için aşağıdaki tabloyu kullanarak neleri paylaşabdiklerinizi ve neleri paylaşabileceğinizi öğrenin:

|Hizmet|Paylaş|
|--|--|
|Bilişsel Hizmetler|X|
|Uygulama hizmeti planı|✔|
|App Service|X|
|Application Insights|✔|
|Arama hizmeti|✔|

## <a name="upgrade-qna-maker-management-sku"></a>Soru-Cevap Oluşturma yönetimi SKU 'SU yükselt

Bilgi tabanınızda, geçerli katmanınızın ötesinde daha fazla soru ve yanıt almanız gerektiğinde Soru-Cevap Oluşturma Hizmeti fiyatlandırma katmanınızı yükseltin. 

Soru-Cevap Oluşturma yönetim SKU 'sunu yükseltmek için:

1. Azure portal Soru-Cevap Oluşturma kaynağına gidin ve **fiyatlandırma katmanı**' nı seçin.

    ![Soru-Cevap Oluşturma kaynağı](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-resource.png)

2. Uygun SKU 'yu seçin ve **Seç**' e basın.

    ![Soru-Cevap Oluşturma fiyatlandırması](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-pricing-page.png)

## <a name="upgrade-app-service"></a>App Service 'i yükselt

 Bilgi tabanınızın istemci uygulamanızdan daha fazla istek sunması gerektiğinde, App Service fiyatlandırma katmanınızı yükseltin.

Uygulama hizmetinin [ölçeğini](https://docs.microsoft.com/azure/app-service/manage-scale-up) değiştirebilir veya azaltabilirsiniz.

1. Azure portal App Service kaynağına gidin ve gerektiğinde **ölçeği yukarı** veya **aşağı genişlet** seçeneğini belirleyin.

    ![Soru-Cevap Oluşturma App Service ölçeği](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-scale.png)

## <a name="upgrade-azure-search-service"></a>Azure Search hizmetini yükselt

Birçok bilgi tabanınız olduğunu planlarken Azure Search hizmeti fiyatlandırma katmanınızı yükseltin. 

Şu anda Azure Search SKU 'sunda yerinde yükseltme yapmak mümkün değildir. Ancak, istenen SKU ile yeni bir Azure Search kaynağı oluşturabilir, verileri yeni kaynağa geri yükleyebilir ve sonra Soru-Cevap Oluşturma yığınına bağlayabilirsiniz.

1. Azure portal yeni bir Azure Search kaynağı oluşturun ve istediğiniz SKU 'yu seçin.

    ![Azure Search kaynağı Soru-Cevap Oluşturma](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-azuresearch-new.png)

2. Dizinleri özgün Azure Search kaynağından yeni bir kaynağa geri yükleyin. Yedekleme geri yükleme örnek koduna [buradan](https://github.com/pchoudhari/QnAMakerBackupRestore)bakın.

3. Veriler geri yüklendikten sonra, yeni Azure Arama kaynağınız ' ne gidin, **anahtarlar**' ı seçin ve **adı** ve **yönetici anahtarı**' nı aklınızda olun.

    ![Azure Arama anahtarları Soru-Cevap Oluşturma](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-azuresearch-keys.png)

4. Yeni Azure Search kaynağını Soru-Cevap Oluşturma yığınına bağlamak için Soru-Cevap Oluşturma App Service 'e gidin.

    ![Soru-Cevap Oluşturma appservice](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-resource-list-appservice.png)

5. **Uygulama ayarları** ' nı seçin ve 3. adımdaki **AzureSearchName** ve **AzureSearchAdminKey** alanlarını değiştirin.

    ![Soru-Cevap Oluşturma appservice ayarı](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-settings.png)

6. App service'ı yeniden başlatın.

    ![Appservice 'i Soru-Cevap Oluşturma yeniden başlatma](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-restart.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Soru-Cevap Oluşturma API'si kullan](../Quickstarts/csharp.md)
