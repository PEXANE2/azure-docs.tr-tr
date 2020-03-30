---
title: Klasik bir web hizmetini yeniden eğitin
titleSuffix: ML Studio (classic) - Azure
description: Azure Machine Learning Studio'da (klasik) yeni eğitilmiş modeli kullanmak için bir modeli nasıl yeniden eğittiğinizi ve klasik bir web hizmetini nasıl güncelleştirtiğinizi öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: peterclu
ms.author: amlstudiodocs
ms.custom: seodec18, previous-ms.author=yahajiza, previous-author=YasinMSFT
ms.date: 02/14/2019
ms.openlocfilehash: 8094d64eab1a4b25a76554bf9eb6848c2e4d3493
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79204248"
---
# <a name="retrain-and-deploy-a-classic-studio-classic-web-service"></a>Klasik Studio (klasik) web hizmetini yeniden eğitin ve dağıtın

[!INCLUDE [Notebook deprecation notice](../../../includes/aml-studio-notebook-notice.md)]

Makine öğrenimi modellerini yeniden eğitmek, bunların doğru kalmasını ve mevcut en alakalı verilere dayanmasını sağlamanın bir yoludur. Bu makalede, klasik bir Studio (klasik) web hizmetini nasıl yeniden eğiteceğiniz gösterecektir. Yeni bir Studio (klasik) web hizmetini yeniden eğitmek için nasıl bir rehber için, [bu nasıl yapılır makalesini görüntüleyin.](retrain-machine-learning-model.md)

## <a name="prerequisites"></a>Ön koşullar

Bu makalede, hem yeniden eğitim denemeniz hem de tahmine dayalı bir denemeniz olduğunu varsayar. Bu adımlar [Retrain ve bir makine öğrenme modeli dağıtmak açıklanmıştır.](/azure/machine-learning/studio/retrain-machine-learning-model) Ancak, makine öğrenimi modelinizi yeni bir web hizmeti olarak dağıtmak yerine, tahmine dayalı denemenizi klasik bir web hizmeti olarak dağıtacaksınız.
     
## <a name="add-a-new-endpoint"></a>Yeni bir bitiş noktası ekleme

Dağıttığınız tahmine dayalı web hizmeti, özgün eğitim ve puanlama denemeleri eğitimli modelle eşit tutulan varsayılan bir puanlama bitiş noktası içerir. Web hizmetinizi yeni bir eğitilmiş modelle güncelleştirmek için yeni bir puanlama bitiş noktası oluşturmanız gerekir.

Bir web hizmetine yeni bir bitiş noktası eklemenin iki yolu vardır:

* Programlı olarak
* Azure Web Hizmetleri portalını kullanma

> [!NOTE]
> Eğitim Web Hizmeti'ne değil, Tahmine Dayalı Web Hizmeti'ne bitiş noktasını eklediğinizden emin olun. Hem Eğitim hem de Öngörücü Web Hizmeti'ni doğru bir şekilde dağıttıysanız, listelenen iki ayrı web hizmeti görmeniz gerekir. Predictive Web Hizmeti "[tahmine dayalı exp.]" ile sona ermelidir.
>

### <a name="programmatically-add-an-endpoint"></a>Programlı bir bitiş noktası eklemek

Bu [GitHub deposunda](https://github.com/hning86/azuremlps#add-amlwebserviceendpoint)sağlanan örnek kodu kullanarak puanlama uç noktaları ekleyebilirsiniz.

### <a name="use-the-azure-web-services-portal-to-add-an-endpoint"></a>Bitiş noktası eklemek için Azure Web Hizmetleri portalını kullanma

1. Machine Learning Studio'da (klasik), soldaki gezinti sütununda Web Hizmetleri'ni tıklatın.
1. Web hizmeti panosunun alt kısmında, **uç noktaları önizlemesini yönet'i**tıklatın.
1. **Ekle**’ye tıklayın.
1. Yeni bitiş noktası için bir ad ve açıklama yazın. Günlüğe kaydetme düzeyini ve örnek verilerin etkin olup olmadığını seçin. Günlük hakkında daha fazla bilgi için Machine [Learning web hizmetleri için günlük atmasını etkinleştir mesuliya](web-services-logging.md)bakın.

## <a name="update-the-added-endpoints-trained-model"></a>Eklenen uç noktanın eğitilmiş modelini güncelleştirme

### <a name="retrieve-patch-url"></a>PATCH URL'si alın

Web portalını kullanarak doğru PATCH URL'sini almak için aşağıdaki adımları izleyin:

1. Azure Machine [Learning Web Services](https://services.azureml.net/) portalında oturum açın.
1. En üstte **Web Hizmetleri'ni** veya **Klasik Web Hizmetleri'ni** tıklatın.
1. Birlikte çalıştığınız puanlama web hizmetini tıklatın (web hizmetinin varsayılan adını değiştirmediyseniz, "[Exp.]]") ile sona erer.
1. **+Yenİ'yi**tıklatın.
1. Bitiş noktası eklendikten sonra, bitiş noktası adını tıklatın.
1. **Yama** URL'sinin altında, yama yardım sayfasını açmak için **API Yardım'ı** tıklatın.

> [!NOTE]
> Predictive Web Hizmeti yerine Eğitim Web Hizmeti'ne bitiş noktasını eklediyseniz, **Kaynak Güncelleştir** bağlantısını tıklattığınızda aşağıdaki hatayı alırsınız: "Üzgünüz, ancak bu özellik bu bağlamda desteklenmez veya kullanılabilir değil. Bu Web Hizmetinin güncel kaynaklara sahip olmadığı. Verdiğimiz rahatsızlıktan dolayı özür dileriz ve bu iş akışını iyileştirmeye çalışıyoruz."
>

PATCH yardım sayfası kullanmanız gereken PATCH URL'sini içerir ve aramak için kullanabileceğiniz örnek kod sağlar.

![Yama URL'si.](./media/retrain-classic/ml-help-page-patch-url.png)

### <a name="update-the-endpoint"></a>Bitiş noktasını güncelleştir

Artık daha önce oluşturduğunuz puanlama bitiş noktasını güncelleştirmek için eğitilmiş modeli kullanabilirsiniz.

Aşağıdaki örnek kod, bitiş noktasını güncelleştirmek için *BaseLocation*, *RelativeLocation*, *SasBlobToken*ve PATCH URL'yi nasıl kullanacağınızı gösterir.

    private async Task OverwriteModel()
    {
        var resourceLocations = new
        {
            Resources = new[]
            {
                new
                {
                    Name = "Census Model [trained model]",
                    Location = new AzureBlobDataReference()
                    {
                        BaseLocation = "https://esintussouthsus.blob.core.windows.net/",
                        RelativeLocation = "your endpoint relative location", //from the output, for example: "experimentoutput/8946abfd-79d6-4438-89a9-3e5d109183/8946abfd-79d6-4438-89a9-3e5d109183.ilearner"
                        SasBlobToken = "your endpoint SAS blob token" //from the output, for example: "?sv=2013-08-15&sr=c&sig=37lTTfngRwxCcf94%3D&st=2015-01-30T22%3A53%3A06Z&se=2015-01-31T22%3A58%3A06Z&sp=rl"
                    }
                }
            }
        };

        using (var client = new HttpClient())
        {
            client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", apiKey);

            using (var request = new HttpRequestMessage(new HttpMethod("PATCH"), endpointUrl))
            {
                request.Content = new StringContent(JsonConvert.SerializeObject(resourceLocations), System.Text.Encoding.UTF8, "application/json");
                HttpResponseMessage response = await client.SendAsync(request);

                if (!response.IsSuccessStatusCode)
                {
                    await WriteFailedResponse(response);
                }

                // Do what you want with a successful response here.
            }
        }
    }

Arama için *apiKey* ve *bitiş noktasıUrl* uç nokta panosundan elde edilebilir.

*Kaynaklardaki* *Ad* parametresinin değeri, tahmin etimlemedenemesinde kaydedilen Eğitilen Modelin Kaynak Adı ile eşleşmelidir. Kaynak Adını almak için:

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Sol menüde **Machine Learning'i**tıklatın.
1. Ad altında, çalışma alanınızı tıklatın ve ardından **Web Hizmetleri'ni**tıklatın.
1. Adı **altında, Sayım Modeli [predictive exp.]** tıklatın.
1. Eklediğiniz yeni bitiş noktasını tıklatın.
1. Bitiş noktası panosunda **Kaynağı Güncelleştir'i**tıklatın.
1. Web hizmeti için Kaynak API Belgeleri Güncelleştir sayfasında, **Kaynak Adını** **Güncel Kaynaklar**altında bulabilirsiniz.

Bitiş noktasını güncelleştirmeyi tamamlamadan önce SAS belirteçinizin süresi doluyorsa, yeni bir belirteç elde etmek için İş Kimliği ile get gerçekleştirmeniz gerekir.

Kod başarıyla çalıştırıldığında, yeni bitiş noktası yeniden eğitilmiş modeli yaklaşık 30 saniye içinde kullanmaya başlamalıdır.

## <a name="next-steps"></a>Sonraki adımlar

Web hizmetlerini yönetme veya birden çok deneme yi izleme hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [Web Hizmetleri portalını keşfedin](manage-new-webservice.md)
* [Deneme yinelemelerini yönetme](manage-experiment-iterations.md)