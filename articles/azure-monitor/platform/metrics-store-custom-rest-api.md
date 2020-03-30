---
title: REST API'yi kullanarak azure monitor metrik veritabanına ölçümler gönderme
description: BIR REST API kullanarak Azure kaynağı için özel ölçümleri Azure Monitor metrik deposuna gönderme
author: anirudhcavale
services: azure-monitor
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: ancav
ms.subservice: metrics
ms.openlocfilehash: 84709c022631543101889f784231158ebb96b6f3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77662273"
---
# <a name="send-custom-metrics-for-an-azure-resource-to-the-azure-monitor-metric-store-by-using-a-rest-api"></a>BIR REST API kullanarak Azure kaynağı için özel ölçümleri Azure Monitor metrik deposuna gönderme

Bu makalede, BIR REST API aracılığıyla Azure kaynakları için özel ölçümlerin Azure Monitor ölçüm deposuna nasıl gönderilebilirsiniz. Ölçümler Azure Monitor'da olduktan sonra, standart ölçümlerle yaptığınız her şeyi onlarla yapabilirsiniz. Örnekler, grafik, uyarı ve diğer dış araçlara yönlendirme vardır.  

>[!NOTE]  
>REST API yalnızca Azure kaynakları için özel ölçümler gönderilmesine izin verir. Farklı ortamlardaki veya şirket içinde kaynaklar için ölçümler göndermek için [Uygulama Öngörülerini](../../azure-monitor/app/api-custom-events-metrics.md)kullanabilirsiniz.    


## <a name="create-and-authorize-a-service-principal-to-emit-metrics"></a>Ölçümleri yalamak için bir hizmet müdürü oluşturma ve yetkilendirme 

Hizmet anabilim yerinde bulunan yönergeleri kullanarak Azure Etkin Dizin kiracınızda [bir hizmet yöneticisi oluşturun.](../../active-directory/develop/howto-create-service-principal-portal.md) 

Bu işlem den geçerken aşağıdakileri not edin: 

- Oturum açma URL'si için herhangi bir URL girebilirsiniz.  
- Bu uygulama için yeni bir istemci sırrı oluşturun.  
- Anahtarı ve istemci kimliğini sonraki adımlarda kullanmak üzere kaydedin.  

1. adım, İzleme Ölçümleri Yayıncısı'nin bir parçası olarak oluşturulan uygulamaya, ölçümleri karşı yaslamak istediğiniz kaynağa izin verin. Uygulamayı birçok kaynağa karşı özel ölçümler yaramak için kullanmayı planlıyorsanız, bu izinleri kaynak grubu veya abonelik düzeyinde verebilirsiniz. 

## <a name="get-an-authorization-token"></a>Yetkilendirme belirteci alın
Komut istemini açın ve aşağıdaki komutu çalıştırın:

```shell
curl -X POST https://login.microsoftonline.com/<yourtenantid>/oauth2/token -F "grant_type=client_credentials" -F "client_id=<insert clientId from earlier step>" -F "client_secret=<insert client secret from earlier step>" -F "resource=https://monitoring.azure.com/"
```
Erişim jetonunu yanıttan kaydedin.

![Erişim belirteci](./media/metrics-store-custom-rest-api/accesstoken.png)

## <a name="emit-the-metric-via-the-rest-api"></a>REST API ile metrik yayar 

1. Aşağıdaki JSON dosyasını bir dosyaya yapıştırın ve yerel bilgisayarınızda **custommetric.json** olarak kaydedin. JSON dosyasındaki saat parametresini güncelleştirin: 
    
    ```json
    { 
        "time": "2018-09-13T16:34:20", 
        "data": { 
            "baseData": { 
                "metric": "QueueDepth", 
                "namespace": "QueueProcessing", 
                "dimNames": [ 
                  "QueueName", 
                  "MessageType" 
                ], 
                "series": [ 
                  { 
                    "dimValues": [ 
                      "ImagesToProcess", 
                      "JPEG" 
                    ], 
                    "min": 3, 
                    "max": 20, 
                    "sum": 28, 
                    "count": 3 
                  } 
                ] 
            } 
        } 
    } 
    ``` 

1. Komut istemi pencerenizde, metrik verileri yayın: 
   - **azureRegion**. Ölçümleri yaydığınız kaynağın dağıtım bölgesiyle eşleşmesi gerekir. 
   - **resourceID**.  Ölçümü izlediğiniz Azure kaynağının kaynak kimliği.  
   - **AccessToken**. Daha önce edindiğiniz belirteci yapıştırın.

     ```Shell 
     curl -X POST https://<azureRegion>.monitoring.azure.com/<resourceId>/metrics -H "Content-Type: application/json" -H "Authorization: Bearer <AccessToken>" -d @custommetric.json 
     ```
1. JSON dosyasındaki zaman damgasını ve değerleri değiştirin. 
1. Önceki iki adımı birkaç kez yineleyin, böylece birkaç dakika verielde elabilirsiniz.

## <a name="troubleshooting"></a>Sorun giderme 
İşlemin bir bölümüyle ilgili bir hata iletisi alırsanız, aşağıdaki sorun giderme bilgilerini göz önünde bulundurun:

1. Azure kaynağınız olarak bir abonelik veya kaynak grubuna karşı ölçümler düzenleyemezsiniz. 
1. 20 dakikadan eski olan mağazaya metrik koyamazsın. Metrik mağaza uyarı ve gerçek zamanlı grafik için optimize edin. 
2. Boyut adlarının sayısı değerleri ve tersi eşleşmelidir. Değerleri kontrol edin. 
2. Özel ölçümleri desteklemeyen bir bölgeye karşı ölçümler yayıyor olabilirsiniz. [Desteklenen bölgelere](../../azure-monitor/platform/metrics-custom-overview.md#supported-regions)bakın. 



## <a name="view-your-metrics"></a>Ölçümlerinizi görüntüleme 

1. Azure Portal’da oturum açın. 

1. Sol menüde **Monitör'ü**seçin. 

1. **Monitör** **sayfasında, Ölçümler'i**seçin. 

   ![Ölçümleri Seçin](./media/metrics-store-custom-rest-api/metrics.png) 

1. Toplama süresini **Son 30 dakika**olarak değiştirin.  

1. **Kaynak** açılır menüsünde, metrik yayılan kaynağı seçin.  

1. Ad **boşlukları** açılır menüsünde **QueueProcessing'i**seçin. 

1. **Ölçümler** açılır menüsünde **QueueDepth'i**seçin.  

 
## <a name="next-steps"></a>Sonraki adımlar
- [Özel ölçümler](../../azure-monitor/platform/metrics-custom-overview.md)hakkında daha fazla bilgi edinin.

