---
title: REST API kullanarak Azure Izleyici ölçüm veritabanına ölçümleri gönderme
description: Bir Azure kaynağı için Azure Izleyici ölçüm deposuna bir REST API kullanarak özel ölçümler gönderme
author: anirudhcavale
services: azure-monitor
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: ancav
ms.subservice: metrics
ms.openlocfilehash: 84709c022631543101889f784231158ebb96b6f3
ms.sourcegitcommit: 747a20b40b12755faa0a69f0c373bd79349f39e3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2020
ms.locfileid: "77662273"
---
# <a name="send-custom-metrics-for-an-azure-resource-to-the-azure-monitor-metric-store-by-using-a-rest-api"></a>Bir Azure kaynağı için Azure Izleyici ölçüm deposuna bir REST API kullanarak özel ölçümler gönderme

Bu makalede, Azure Izleyici ölçüm deposuna bir REST API aracılığıyla Azure kaynakları için nasıl özel ölçümler gönderileceği gösterilir. Ölçümler Azure Izleyici 'de olduktan sonra, bunları standart ölçümlerle yaptığınız tüm şeyleri yapabilirsiniz. Örnekler grafikleme, uyarma ve diğer dış araçlara yönlendirme.  

>[!NOTE]  
>REST API yalnızca Azure kaynakları için özel ölçümler göndermeye izin verir. Farklı ortamlardaki veya Şirket içindeki kaynaklara yönelik ölçümleri göndermek için [Application Insights](../../azure-monitor/app/api-custom-events-metrics.md)kullanabilirsiniz.    


## <a name="create-and-authorize-a-service-principal-to-emit-metrics"></a>Ölçümleri göstermek için bir hizmet sorumlusu oluşturma ve yetkilendirme 

[Hizmet sorumlusu oluşturma](../../active-directory/develop/howto-create-service-principal-portal.md)bölümünde bulunan yönergeleri kullanarak Azure Active Directory kiracınızda bir hizmet sorumlusu oluşturun. 

Bu işlemi yaparken aşağıdakilere göz önünde olun: 

- Oturum açma URL 'si için herhangi bir URL girebilirsiniz.  
- Bu uygulama için yeni bir istemci gizli dizisi oluşturun.  
- Daha sonraki adımlarda kullanmak üzere anahtarı ve istemci KIMLIĞINI kaydedin.  

Uygulamanın 1. adım, ölçüm Izleme yayımcısı, ölçümleri sunmak istediğiniz kaynağa yönelik izinler olarak oluşturulmasını sağlayın. Uygulamayı birçok kaynağa karşı özel ölçümleri yayan kullanmayı planlıyorsanız, bu izinleri kaynak grubu veya abonelik düzeyinde verebilirsiniz. 

## <a name="get-an-authorization-token"></a>Yetkilendirme belirteci al
Bir komut istemi açın ve aşağıdaki komutu çalıştırın:

```shell
curl -X POST https://login.microsoftonline.com/<yourtenantid>/oauth2/token -F "grant_type=client_credentials" -F "client_id=<insert clientId from earlier step>" -F "client_secret=<insert client secret from earlier step>" -F "resource=https://monitoring.azure.com/"
```
Erişim belirtecini yanıttan kaydedin.

![erişim belirteci](./media/metrics-store-custom-rest-api/accesstoken.png)

## <a name="emit-the-metric-via-the-rest-api"></a>Ölçümü REST API aracılığıyla yayma 

1. Aşağıdaki JSON dosyasını bir dosyaya yapıştırın ve yerel bilgisayarınızda **custommetric. JSON** olarak kaydedin. JSON dosyasındaki zaman parametresini güncelleştirin: 
    
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

1. Komut istemi penceresinde, ölçüm verilerini gönderin: 
   - **Azureregion**. , Ölçümlerini yayan kaynağın dağıtım bölgesiyle eşleşmelidir. 
   - **RESOURCEID**.  Ölçümü izlemekte olduğunuz Azure kaynağının kaynak KIMLIĞI.  
   - **Accesstoken**. Daha önce aldığınız belirteci yapıştırın.

     ```Shell 
     curl -X POST https://<azureRegion>.monitoring.azure.com/<resourceId>/metrics -H "Content-Type: application/json" -H "Authorization: Bearer <AccessToken>" -d @custommetric.json 
     ```
1. JSON dosyasındaki zaman damgasını ve değerleri değiştirin. 
1. Önceki iki adımı birkaç kez tekrarlayın, bu nedenle birkaç dakika için verileriniz vardır.

## <a name="troubleshooting"></a>Sorun giderme 
İşlemin bir parçası ile bir hata iletisi alırsanız, aşağıdaki sorun giderme bilgilerini göz önünde bulundurun:

1. Azure kaynağınız olarak bir abonelik veya kaynak grubuna karşı ölçümler yapamazsınız. 
1. Bir ölçümü 20 dakikadan daha eski bir mağazaya koyamazsınız. Ölçüm deposu, uyarı ve gerçek zamanlı grafik için iyileştirilmiştir. 
2. Boyut adlarının sayısı, değerlerle eşleşmelidir ve tam tersi de geçerlidir. Değerleri denetleyin. 
2. Özel ölçümleri desteklemeyen bir bölgeye karşı ölçümleri yayan olabilirsiniz. Bkz. [Desteklenen bölgeler](../../azure-monitor/platform/metrics-custom-overview.md#supported-regions). 



## <a name="view-your-metrics"></a>Ölçümlerinizi görüntüleme 

1. Azure Portal’da oturum açın. 

1. Sol taraftaki menüde **izleyici**' yi seçin. 

1. **İzleyici** sayfasında **ölçümler**' i seçin. 

   ![Ölçümleri seçin](./media/metrics-store-custom-rest-api/metrics.png) 

1. Toplama süresini **son 30 dakika**olarak değiştirin.  

1. **Kaynak** açılan menüsünde, ölçümü dağıttığınız kaynağı seçin.  

1. **Ad alanları** açılan menüsünde **queueprocessing**' ı seçin. 

1. **Ölçümler** açılan menüsünde, **queuedepth**' ı seçin.  

 
## <a name="next-steps"></a>Sonraki adımlar
- [Özel ölçümler](../../azure-monitor/platform/metrics-custom-overview.md)hakkında daha fazla bilgi edinin.

