---
title: Azure Olay Ağıt'ında özel konular için olağanüstü durum kurtarma
description: Bu öğretici, Olay Izgara hizmeti bir bölgede sağlıksız hale gelirse kurtarmak için olay mimarisini nasıl ayarlayabilirsiniz size yol açacaktır.
services: event-grid
author: banisadr
ms.service: event-grid
ms.topic: tutorial
ms.date: 01/21/2020
ms.author: babanisa
ms.openlocfilehash: 87f8f79e2cf125fa5735653153d8fcaa781f5200
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "76511527"
---
# <a name="build-your-own-disaster-recovery-for-custom-topics-in-event-grid"></a>Olay Izgara'sında özel konular için kendi olağanüstü durum kurtarma oluşturun
Olağanüstü durum kurtarma, ciddi bir uygulama işlevselliği kaybından kurtulmaya odaklanır. Bu öğretici, Olay Izgara hizmeti belirli bir bölgede sağlıksız hale gelirse kurtarmak için olay mimarisini nasıl ayarlayabilirsiniz size yol açacaktır.

Bu öğreticide, Event Grid'deki özel konular için etkin-pasif bir başarısız mimari oluşturmayı öğreneceksiniz. Konularınızı ve aboneliklerinizi iki bölgeye yansıtarak ve bir konu sağlıksız hale geldiğinde bir başarısızı yöneterek başarısız olacaksınız. Bu öğreticideki mimari tüm yeni trafik te başarısız olur. bu kurulumla, tehlikeye girmiş bölge tekrar sağlıklı hale gelene kadar uçuştaki olayların geri kazanılamayacağının farkında olmak önemlidir.

> [!NOTE]
> Olay Grid şimdi sunucu tarafında otomatik jeo durum kurtarma (GeoDR) destekler. Başarısız lık işlemi üzerinde daha fazla denetim istiyorsanız, istemci tarafı olağanüstü durum kurtarma mantığını yine de uygulayabilirsiniz. Otomatik GeoDR hakkında ayrıntılı bilgi için [Azure Olay Grid'de Sunucu tarafındaki coğrafi felaket kurtarma bilgisine](geo-disaster-recovery.md)bakın.

## <a name="create-a-message-endpoint"></a>İleti uç noktası oluşturma

Başarısız yapılandırmanızı test etmek için, etkinliklerinizi almak için bir bitiş noktasına ihtiyacınız vardır. Bitiş noktası, başarısız altyapınızın bir parçası değildir, ancak test etmeyi kolaylaştırmak için etkinlik işleyicimiz olarak hareket edecektir.

Testi basitleştirmek için, olay mesajlarını görüntüleyen önceden oluşturulmuş bir [web uygulaması](https://github.com/Azure-Samples/azure-event-grid-viewer) dağıtın. Dağıtılan çözüm bir App Service planı, App Service web uygulaması ve GitHub'dan kaynak kod içerir.

1. Çözümü aboneliğinize dağıtmak için **Azure'a Dağıt**'ı seçin. Azure portalında parametre değerlerini girin.

   <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure-Samples%2Fazure-event-grid-viewer%2Fmaster%2Fazuredeploy.json" target="_blank"><img src="https://azuredeploy.net/deploybutton.png"/></a>

1. Dağıtımın tamamlanması birkaç dakika sürebilir. Dağıtım başarıyla gerçekleştirildikten sonra, web uygulamanızı görüntüleyip çalıştığından emin olun. Web tarayıcısında şu adrese gidin: `https://<your-site-name>.azurewebsites.net`
Daha sonra ihtiyacınız olacağından bu URL'yi not aldığınızdan emin olun.

1. Siteyi görürsünüz ancak henüz yayımlanmış olay yoktur.

   ![Yeni siteyi görüntüleme](./media/blob-event-quickstart-portal/view-site.png)

[!INCLUDE [event-grid-register-provider-portal.md](../../includes/event-grid-register-provider-portal.md)]


## <a name="create-your-primary-and-secondary-topics"></a>Birincil ve ikincil konularınızı oluşturun

İlk olarak, iki Olay Izgara konu oluşturun. Bu konular birincil ve ikincil olarak hareket edecektir. Varsayılan olarak, olaylarınız birincil konunuz üzerinden akacaktır. Birincil bölgede bir hizmet kesintisi varsa, ikincil durumunuz devralır.

1. [Azure portalında](https://portal.azure.com)oturum açın. 

1. Ana Azure menüsünün sol üst köşesinden, Olay **Izgarasını** arama > **Tüm hizmetleri** seçin > **Olay Izgara Konuları'nı**seçin.

   ![Olay Izgara Konuları menüsü](./media/custom-disaster-recovery/select-topics-menu.png)

    Gelecekte daha kolay erişim için kaynak menüsüne eklemek için Olay Izgara Konuları'nın yanındaki yıldızı seçin.

1. Olay Izgara Konuları Menüsünde, birincil konunuzu oluşturmak için **+ADD'yi** seçin.

   * Konuya mantıksal bir ad verin ve izlemeyi kolaylaştırmak için sonek olarak "birincil" ekleyin.
   * Bu konunun bölgesi birincil bölgeniz olacaktır.

     ![Olay Izgara Konu birincil diyalog oluşturmak](./media/custom-disaster-recovery/create-primary-topic.png)

1. Konu oluşturulduktan sonra, konuya gidin ve **Konu Bitiş Noktasını**kopyalayın. Uri'ye daha sonra ihtiyacın olacak.

    ![Olay Izgara Birincil Konu](./media/custom-disaster-recovery/get-primary-topic-endpoint.png)

1. Daha sonra gereksinim duymanız gereken konuiçin erişim anahtarını alın. Kaynak menüsündeki **Access tuşlarına** tıklayın ve Anahtar 1'i kopyalayın.

    ![Birincil Konu Anahtarını Al](./media/custom-disaster-recovery/get-primary-access-key.png)

1. Konu çubuğunda, öğreticiye ön koşullarda yaptığınız etkinlik alıcısı web sitesini abone olan bir abonelik oluşturmak için **+Event Aboneliği'ni** tıklatın.

   * Olay aboneliğine mantıksal bir ad verin ve izlemeyi kolaylaştırmak için sonek olarak "birincil" ekleyin.
   * Endpoint Türü Web Kancası'nı seçin.
   * Bitiş noktasını etkinlik alıcınızın etkinlik URL'sine ayarlayın ve bu url'ye benzer bir şey görünmelidir:`https://<your-event-reciever>.azurewebsites.net/api/updates`

     ![Olay Izgara Birincil Etkinlik Aboneliği](./media/custom-disaster-recovery/create-primary-es.png)

1. İkincil konu nuzu ve aboneliğinizi oluşturmak için aynı akışı yineleyin. Bu kez, daha kolay izleme için "-birincil" soneki "-ikincil" ile değiştirin. Son olarak, farklı bir Azure Bölgesine koyduğunuzdan emin olun. İstediğiniz yere koyabilirsiniz, ancak [Azure Eşleştirilmiş Bölgeleri](../best-practices-availability-paired-regions.md)kullanmanız önerilir. İkincil konu ve aboneliği nbaşka bir bölgeye yerleştirmek, birincil bölge kapansa bile yeni etkinliklerinizin akmasını sağlar.

Şimdi olmalıdır:

   * Test için bir olay alıcıweb sitesi.
   * Birincil bölgenizdeki birincil konu.
   * Birincil konunuzu olay alıcısı web sitesine bağlayan birincil etkinlik aboneliği.
   * İkinci bölgenizde ikincil bir konu.
   * Birincil konunuzu olay alıcısı web sitesine bağlayan ikincil bir olay aboneliği.

## <a name="implement-client-side-failover"></a>İstemci tarafı yük devretmeyi uygulama

Artık bölgesel olarak gereksiz bir konu ve abonelik kurulumu çiftine sahip olduğunuza göre, istemci tarafı nın başarısız tarafını uygulamaya hazırsınız. Bunu başarmanın çeşitli yolları vardır, ancak tüm başarısız uygulamaların ortak bir özelliği olacaktır: bir konu artık sağlıklı değilse, trafik diğer konuya yönlendirilir.

### <a name="basic-client-side-implementation"></a>Temel istemci tarafı uygulaması

Aşağıdaki örnek kod, her zaman birincil konunuzda yayımlamaya çalışacak basit bir .NET yayımcısıdır. Başarılı olmazsa, ikincil konu üzerinde başarısız olacaktır. Her iki durumda da, aynı zamanda bir GET yaparak `https://<topic-name>.<topic-region>.eventgrid.azure.net/api/health`diğer konunun sağlık api denetler. Sağlıklı bir konu her zaman **200 OK** ile bir GET **/ api / sağlık** bitiş noktası yapılır yanıt vermelidir.

```csharp
using System;
using System.Net.Http;
using System.Collections.Generic;
using Microsoft.Azure.EventGrid;
using Microsoft.Azure.EventGrid.Models;
using Newtonsoft.Json;

namespace EventGridFailoverPublisher
{
    // This captures the "Data" portion of an EventGridEvent on a custom topic
    class FailoverEventData
    {
        [JsonProperty(PropertyName = "teststatus")]
        public string TestStatus { get; set; }
    }

    class Program
    {
        static void Main(string[] args)
        {
            // TODO: Enter the endpoint each topic. You can find this topic endpoint value
            // in the "Overview" section in the "Event Grid Topics" blade in Azure Portal..
            string primaryTopic = "https://<primary-topic-name>.<primary-topic-region>.eventgrid.azure.net/api/events";
            string secondaryTopic = "https://<secondary-topic-name>.<secondary-topic-region>.eventgrid.azure.net/api/events";

            // TODO: Enter topic key for each topic. You can find this in the "Access Keys" section in the
            // "Event Grid Topics" blade in Azure Portal.
            string primaryTopicKey = "<your-primary-topic-key>";
            string secondaryTopicKey = "<your-secondary-topic-key>";

            string primaryTopicHostname = new Uri( primaryTopic).Host;
            string secondaryTopicHostname = new Uri(secondaryTopic).Host;

            Uri primaryTopicHealthProbe = new Uri("https://" + primaryTopicHostname + "/api/health");
            Uri secondaryTopicHealthProbe = new Uri("https://" + secondaryTopicHostname + "/api/health");

            var httpClient = new HttpClient();

            try
            {
                TopicCredentials topicCredentials = new TopicCredentials(primaryTopicKey);
                EventGridClient client = new EventGridClient(topicCredentials);

                client.PublishEventsAsync(primaryTopicHostname, GetEventsList()).GetAwaiter().GetResult();
                Console.Write("Published events to primary Event Grid topic.");

                HttpResponseMessage health = httpClient.GetAsync(secondaryTopicHealthProbe).Result;
                Console.Write("\n\nSecondary Topic health " + health);
            }
            catch (Microsoft.Rest.Azure.CloudException e)
            {
                TopicCredentials topicCredentials = new TopicCredentials(secondaryTopicKey);
                EventGridClient client = new EventGridClient(topicCredentials);

                client.PublishEventsAsync(secondaryTopicHostname, GetEventsList()).GetAwaiter().GetResult();
                Console.Write("Published events to secondary Event Grid topic. Reason for primary topic failure:\n\n" + e);

                HttpResponseMessage health = httpClient.GetAsync(primaryTopicHealthProbe).Result;
                Console.Write("\n\nPrimary Topic health " + health);
            }

            Console.ReadLine();
        }

        static IList<EventGridEvent> GetEventsList()
        {
            List<EventGridEvent> eventsList = new List<EventGridEvent>();

            for (int i = 0; i < 5; i++)
            {
                eventsList.Add(new EventGridEvent()
                {
                    Id = Guid.NewGuid().ToString(),
                    EventType = "Contoso.Failover.Test",
                    Data = new FailoverEventData()
                    {
                        TestStatus = "success"
                    },
                    EventTime = DateTime.Now,
                    Subject = "test" + i,
                    DataVersion = "2.0"
                });
            }

            return eventsList;
        }
    }
}
```

### <a name="try-it-out"></a>Deneyin

Artık tüm bileşenlerinizi yerleştirdiğinize göre, başarısız uygulamanızı test edebilirsiniz. Yukarıdaki örneği Visual Studio kodunda veya en sevdiğiniz ortamda çalıştırın. Aşağıdaki dört değeri, konularınızdaki uç noktaları ve tuşları ile değiştirin:

   * primaryTopic - birincil konu için bitiş noktası.
   * ikincilTopic - ikincil konu için bitiş noktası.
   * primaryTopicKey - birincil konu için anahtar.
   * ikincilTopicKey - ikincil konu için anahtar.

Etkinlik yayımcısı çalıştırmayı deneyin. Test etkinliklerinizin aşağıdaki gibi Olay Izgara görüntüleyicinizde yer alınır.

![Olay Izgara Birincil Etkinlik Aboneliği](./media/custom-disaster-recovery/event-grid-viewer.png)

Başarısızlığınızın çalıştığından emin olmak için, birincil konu anahtarınızdaki birkaç karakteri değiştirip artık geçerli hale getiremezsiniz. Yayımcıyı tekrar çalıştırmayı deneyin. Olay Izgara görüntüleyicinizde yeni olayların göründüğünü görmeniz gerekir, ancak konsolunuza baktığınızda, bunların artık ikincil konu üzerinden yayımlandığını görürsünüz.

### <a name="possible-extensions"></a>Olası uzantılar

İhtiyaçlarınıza göre bu örneği genişletmenin birçok yolu vardır. Yüksek hacimli senaryolar için, konunun sistem durumu apisini bağımsız olarak düzenli olarak denetlemek isteyebilirsiniz. Bu şekilde, bir konu aşağı gitmek olsaydı, her bir yayımlama ile kontrol etmek gerekmez. Bir konunun sağlıklı olmadığını anladıktan sonra, ikincil konuya yayımlamayı varsayılan olarak belirleyebilirsiniz.

Benzer şekilde, belirli gereksinimlerinize göre geri dönüş mantığını uygulamak isteyebilirsiniz. En yakın veri merkezine yayımlama gecikme süresini azaltmak için kritik önemtaşıyorsa, başarısız olan bir konunun sistem durumu apisini düzenli aralıklarla inceleyebilirsiniz. Tekrar sağlıklı olduğunda, daha yakın veri merkezine geri dönmenin güvenli olduğunu anlarsınız.

## <a name="next-steps"></a>Sonraki adımlar

- [Http'deki bir bitiş noktasından olayları](./receive-events.md) nasıl alacağınızı öğrenin
- Etkinlikleri Karma Bağlantılara nasıl [yönlendireceklerini](./custom-event-to-hybrid-connection.md) keşfedin
- Azure [DNS ve Trafik Yöneticisi'ni kullanarak olağanüstü durum kurtarma](https://docs.microsoft.com/azure/networking/disaster-recovery-dns-traffic-manager) hakkında bilgi edinin
