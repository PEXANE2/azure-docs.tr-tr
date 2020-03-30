---
title: Azure Uygulama Öngörüleri ile istekleri izlemek için kod yazın | Microsoft Dokümanlar
description: İstekleriniz için profil alabilmeniz için Application Insights ile istekleri izlemek için kod yazın.
ms.topic: conceptual
author: cweining
ms.author: cweining
ms.date: 08/06/2018
ms.reviewer: mbullwin
ms.openlocfilehash: c59cbe852a91a91c7b3adb4452328700ec718a82
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77671605"
---
# <a name="write-code-to-track-requests-with-application-insights"></a>Uygulama Öngörüleri ile istekleri izlemek için kod yazma

Uygulamanızın profillerini Performans sayfasında görüntülemek için Azure Application Insights'ın uygulamanıza yönelik istekleri izlemesi gerekir. Application Insights, zaten enstrümantedilmiş çerçeveler üzerine oluşturulmuş uygulamalar için istekleri otomatik olarak izleyebilir. İki örnek ASP.NET ve ASP.NET Core'dir. 

Azure Bulut Hizmetleri çalışan rolleri ve Service Fabric stateless API'leri gibi diğer uygulamalar için, uygulama istatistiklerine isteklerinizin nerede başlayıp nerede biteceğini söylemek için kod yazmanız gerekir. Bu kodu yazdıktan sonra, istekleri telemetri Uygulama Öngörüleri'ne gönderilir. Performans sayfasında telemetriyi görüntüleyebilirsiniz ve bu istekler için profiller toplanır. 

İstekleri el ile izlemek için aşağıdakileri yapın:

  1. Uygulama ömrünün başlarında, aşağıdaki kodu ekleyin:  

        ```csharp
        using Microsoft.ApplicationInsights.Extensibility;
        ...
        // Replace with your own Application Insights instrumentation key.
        TelemetryConfiguration.Active.InstrumentationKey = "00000000-0000-0000-0000-000000000000";
        ```
      Bu global enstrümantasyon anahtarı yapılandırması hakkında daha fazla bilgi için, [Bkz. Uygulama Öngörüleri ile Hizmet Dokusu'yu Kullan.](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/blob/dev/appinsights/ApplicationInsights.md)  

  1. Enstrüman tonu yapmak istediğiniz herhangi bir `StartOperation<RequestTelemetry>` kod parçası için, aşağıdaki örnekte gösterildiği gibi, etrafında bir **kullanma** deyimi ekleyin:

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

        Başka `StartOperation<RequestTelemetry>` bir `StartOperation<RequestTelemetry>` kapsamda arama desteklenmez. Bunun yerine `StartOperation<DependencyTelemetry>` iç içe olan kapsamda kullanabilirsiniz. Örnek:  
        
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
