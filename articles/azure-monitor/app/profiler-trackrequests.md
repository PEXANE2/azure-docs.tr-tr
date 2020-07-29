---
title: İstekleri Azure Application Insights izlemek için kod yazma | Microsoft Docs
description: İsteklerinize yönelik profilleri alabilmeniz için Application Insights olan istekleri izlemek için kod yazın.
ms.topic: conceptual
author: cweining
ms.author: cweining
ms.date: 08/06/2018
ms.reviewer: mbullwin
ms.openlocfilehash: c59cbe852a91a91c7b3adb4452328700ec718a82
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "77671605"
---
# <a name="write-code-to-track-requests-with-application-insights"></a>Application Insights ile istekleri izlemek için kod yazma

Performans sayfasında uygulamanızın profillerini görüntülemek için, Azure Application Insights uygulamanızın isteklerini izlemesi gerekir. Application Insights, zaten işaretlenmiş çerçeveler üzerinde oluşturulmuş uygulamalar için istekleri otomatik olarak izleyebilir. İki örnek ASP.NET ve ASP.NET Core. 

Azure Cloud Services çalışan rolleri ve durum bilgisiz API 'Leri Service Fabric gibi diğer uygulamalar için, isteklerinizin başlayacağı ve sona erdirmek Application Insights bildirmek için kod yazmanız gerekir. Bu kodu yazdıktan sonra, Application Insights telemetri gönderilir. Telemetri ' i performans sayfasında görüntüleyebilirsiniz ve bu istekler için profiller toplanır. 

İstekleri el ile izlemek için aşağıdakileri yapın:

  1. Uygulama ömrünün başlarında, aşağıdaki kodu ekleyin:  

        ```csharp
        using Microsoft.ApplicationInsights.Extensibility;
        ...
        // Replace with your own Application Insights instrumentation key.
        TelemetryConfiguration.Active.InstrumentationKey = "00000000-0000-0000-0000-000000000000";
        ```
      Bu genel izleme anahtarı yapılandırması hakkında daha fazla bilgi için bkz. [Application Insights ile Service Fabric kullanma](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/blob/dev/appinsights/ApplicationInsights.md).  

  1. İşaretlemek istediğiniz kod parçaları için, `StartOperation<RequestTelemetry>` Aşağıdaki örnekte gösterildiği gibi, etrafında bir **using** deyimleri ekleyin:

        ```csharp
        using Microsoft.ApplicationInsights;
        using Microsoft.ApplicationInsights.DataContracts;
        ...
        var client = new TelemetryClient();
        ...
        using (var operation = client.StartOperation<RequestTelemetry>("Insert_Your_Custom_Event_Unique_Name"))
        {
          // ... Code I want to profile.
        }
        ```

        `StartOperation<RequestTelemetry>`Başka bir kapsam içinde çağrı `StartOperation<RequestTelemetry>` desteklenmiyor. `StartOperation<DependencyTelemetry>`Bunun yerine iç içe kapsamda kullanabilirsiniz. Örneğin:  
        
        ```csharp
        using (var getDetailsOperation = client.StartOperation<RequestTelemetry>("GetProductDetails"))
        {
        try
        {
          ProductDetail details = new ProductDetail() { Id = productId };
          getDetailsOperation.Telemetry.Properties["ProductId"] = productId.ToString();
        
          // By using DependencyTelemetry, 'GetProductPrice' is correctly linked as part of the 'GetProductDetails' request.
          using (var getPriceOperation = client.StartOperation<DependencyTelemetry>("GetProductPrice"))
          {
              double price = await _priceDataBase.GetAsync(productId);
              if (IsTooCheap(price))
              {
                  throw new PriceTooLowException(productId);
              }
              details.Price = price;
          }
        
          // Similarly, note how 'GetProductReviews' doesn't establish another RequestTelemetry.
          using (var getReviewsOperation = client.StartOperation<DependencyTelemetry>("GetProductReviews"))
          {
              details.Reviews = await _reviewDataBase.GetAsync(productId);
          }
        
          getDetailsOperation.Telemetry.Success = true;
          return details;
        }
        catch(Exception ex)
        {
          getDetailsOperation.Telemetry.Success = false;
        
          // This exception gets linked to the 'GetProductDetails' request telemetry.
          client.TrackException(ex);
          throw;
        }
        }
        ```
