---
title: Kibana kullanarak Azure Veri Gezgini verileri görselleştirme
description: Bu makalede, kibana için bir veri kaynağı olarak Azure Veri Gezgini ayarlamayı öğreneceksiniz.
author: orspod
ms.author: orspodek
ms.reviewer: guregini
ms.service: data-explorer
ms.topic: conceptual
ms.date: 03/12/2020
ms.openlocfilehash: 30d74f36c6462d1fba039595d2ed6fe722b742e8
ms.sourcegitcommit: d322d0a9d9479dbd473eae239c43707ac2c77a77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2020
ms.locfileid: "79164819"
---
# <a name="visualize-data-from-azure-data-explorer-in-kibana-with-the-k2bridge-open-source-connector"></a>K2Bridge açık kaynaklı bağlayıcı ile kibana içindeki Azure Veri Gezgini verilerini görselleştirin

K2Bridge (kibana-kusto Köprüsü), veri kaynağı olarak Azure Veri Gezgini kullanmanıza ve bu verileri kibana ' de görselleştirmenize olanak sağlar. K2Bridge, bir kibana örneği ve Azure Veri Gezgini kümesi arasında proxy görevi gören [Açık kaynaklı](https://github.com/microsoft/K2Bridge) kapsayıcılı bir uygulamadır. Bu makalede, K2Bridge kullanarak bu bağlantıyı nasıl oluşturacağınız açıklanır.

K2Bridge, kibana sorgularını kusto sorgu diline (KQL) çevirir ve Azure Veri Gezgini sonuçlarını kibana 'e geri gönderir. 

   ![grafik](media/k2bridge/k2bridge-chart.png)

K2Bridge, kibana 'ın bulma sekmesini destekler, burada şunları yapabilirsiniz:
* Verileri arayın ve araştırın
* Sonuçları filtreleme
* Sonuçlar kılavuzunda alan ekleme veya kaldırma
* Kayıt içeriğini görüntüle
* Aramaları kaydetme ve paylaşma

Aşağıdaki görüntüde, Azure Veri Gezgini tarafından K2Bridge tarafından bağlantılı bir kibana örneği gösterilmektedir. Kibana sürümündeki Kullanıcı deneyimi değiştirilmez.

   [![kibana sayfası](media/k2bridge/k2bridge-kibana-page.png)](media/k2bridge/k2bridge-kibana-page.png#lightbox)

## <a name="prerequisites"></a>Önkoşullar

Kibana ' de Azure Veri Gezgini verileri görselleştirmeye başlamadan önce şunları hazırlayın:

* Kubernetes paket yöneticisi olan [helk v3](https://github.com/helm/helm#install)
* Azure Kubernetes hizmeti (AKS) kümesi veya başka bir Kubernetes kümesi (sürüm 1,14 sürüm 1,16 ' e sınanmış ve doğrulanmıştır). AKS kümesine ihtiyacınız varsa bkz [. Azure CLI kullanarak](https://docs.microsoft.com/azure/aks/kubernetes-walkthrough) aks kümesi dağıtma veya [Azure Portal kullanma](https://docs.microsoft.com/azure/aks/kubernetes-walkthrough-portal)
* Aşağıdakiler dahil olmak üzere bir [Azure Veri Gezgini kümesi](create-cluster-database-portal.md):
    * Azure Veri Gezgini kümesinin URL 'SI 
    * Veritabanı adı
    
* Azure Veri Gezgini 'de verileri görüntüleme yetkisine sahip bir Azure AD hizmet sorumlusu, aşağıdakiler dahil:
    * Istemci KIMLIĞI 
    * Istemci parolası

    ' Görüntüleyici ' iznine sahip bir hizmet sorumlusu önerilir. Daha yüksek izinlerin kullanılması önerilmez.

    * [Azure AD hizmet sorumlusu için kümenin görünüm Izinlerini ayarlayın](https://docs.microsoft.com/azure/data-explorer/manage-database-permissions#manage-permissions-in-the-azure-portal).

    Azure AD hizmet sorumlusu hakkında daha fazla bilgi için bkz. [Azure AD hizmet sorumlusu oluşturma](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#create-an-azure-active-directory-application).

## <a name="run-k2bridge-on-azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS) üzerinde K2Bridge çalıştırma

Varsayılan olarak, K2Bridges's Helm grafiği Microsoft 'un Container Registry (MCR) ' de bulunan genel kullanıma açık bir görüntüye başvurur. MCR herhangi bir kimlik bilgisi gerektirmez ve kullanıma hazır değildir.

1. Gerekli Held grafiklerini indirin.

1. Held 'ya Elaun arama bağımlılığı ekleyin. 
    Elaun arama bağımlılığının nedeni, K2Bridge meta verilerle ilgili istekleri (Dizin desenleri ve kayıtlı sorgular gibi) hizmet etmek için bir iç küçük Elasi arama örneği kullanır. Bu iç örneğe hiçbir iş verisi kaydedilmez ve uygulama ayrıntısı olarak kabul edilebilir. 

    1. Hele arama bağımlılığını eklemek için:

        ```bash
        helm repo add elastic https://helm.elastic.co
        helm repo update
        ```

    1. GitHub deposunun grafikler dizini altında K2Bridge grafiğini almak için:
        1. Depoyu [GitHub](https://github.com/microsoft/K2Bridge)'dan kopyalayın.
        1. K2Bridges kök deposu dizinine gidin.
        1. Çalıştırın:

            ```bash
            helm dependency update charts/k2bridge
            ```

1. K2Bridge dağıtma:

    1. Değişkenleri ortamınız için doğru değerlerle ayarlayın:

        ```bash
        ADX_URL=[YOUR_ADX_CLUSTER_URL] #For example, https://mycluster.westeurope.kusto.windows.net
        ADX_DATABASE=[YOUR_ADX_DATABASE_NAME]
        ADX_CLIENT_ID=[SERVICE_PRINCIPAL_CLIENT_ID]
        ADX_CLIENT_SECRET=[SERVICE_PRINCIPAL_CLIENT_SECRET]
        ADX_TENANT_ID=[SERVICE_PRINCIPAL_TENANT_ID]
        ```

    1. Seçim Azure Application Insights telemetrisini etkinleştirin. 
        Azure Application Insights ilk kez kullanıyorsanız, önce [bir Application Insights kaynağı oluşturmalısınız](https://docs.microsoft.com/azure/azure-monitor/app/create-new-resource). [İzleme anahtarını](https://docs.microsoft.com/azure/azure-monitor/app/create-new-resource#copy-the-instrumentation-key) bir değişkene kopyalamanız gerekecektir: 

        ```bash
        APPLICATION_INSIGHTS_KEY=[INSTRUMENTATION_KEY]
        COLLECT_TELEMETRY=true
        ```

    1. <a name="install-k2bridge-chart"></a>K2Bridge grafiğini yükler:

        ```bash
        helm install k2bridge charts/k2bridge -n k2bridge --set image.repository=$REPOSITORY_NAME/$CONTAINER_NAME --set settings.adxClusterUrl="$ADX_URL" --set settings.adxDefaultDatabaseName="$ADX_DATABASE" --set settings.aadClientId="$ADX_CLIENT_ID" --set settings.aadClientSecret="$ADX_CLIENT_SECRET" --set settings.aadTenantId="$ADX_TENANT_ID" [--set image.tag=latest] [--set privateRegistry="$IMAGE_PULL_SECRET_NAME"] [--set settings.collectTelemetry=$COLLECT_TELEMETRY]
        ```

        [Yapılandırma](https://github.com/microsoft/K2Bridge/blob/master/docs/configuration.md) ' da, tüm yapılandırma seçenekleri kümesini bulabilirsiniz.

    1. Komut çıktısı, kibana dağıtmak için çalıştırılacak sonraki hele komutunu önerir. İsteğe bağlı olarak şunu çalıştırın:

        ```bash
        helm install kibana elastic/kibana -n k2bridge --set image=docker.elastic.co/kibana/kibana-oss --set imageTag=6.8.5 --set elasticsearchHosts=http://k2bridge:8080
        ```
    1. Localhost üzerinde kibana erişmek için bağlantı noktası iletmeyi kullanın: 

        ```bash
        kubectl port-forward service/kibana-kibana 5601 --namespace k2bridge
        ```
    1. http://127.0.0.1:5601göz atarak kibana 'e bağlanın.

    1. Kibana 'i son kullanıcılara sunun. Bunu yapmak için birden çok yöntem vardır. Kullandığınız yöntem büyük ölçüde kullanım örneğine bağlıdır.

        Örnek:

        Hizmeti bir LoadBalancer hizmeti olarak kullanıma sunun. Bunu yapmak için, K2Bridge HELI install komutuna ([Yukarıdaki](#install-k2bridge-chart)) aşağıdaki parametreyi ekleyin:

        `--set service.type=LoadBalancer`
    
        Ardından şunu çalıştırın:

           ```bash
           kubectl get service -w -n k2bridge
           ```   
        Çıktının şöyle görünmesi gerekir: 

        ```bash
        NAME            TYPE           CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
        kibana-kibana   LoadBalancer   xx.xx.xx.xx   <pending>      5601:30128/TCP   4m24s
        ```
        Daha sonra görüntülenen dış IP 'yi kullanabilir ve şunları kullanarak bir tarayıcı açarak kibana 'e erişebilirsiniz: `\<EXTERNAL-IP>:5601`.

1. Verilerinize erişmek için Dizin düzenlerini yapılandırın:  
Yeni bir kibana örneğinde:
     1. Kibana 'i açın.
     1. Yönetime gidin.
     1. **Dizin düzenlerini**seçin. 
     1. Dizin kalıbı oluşturun.
Dizinin adı, yıldız işareti olmadan tablo adı veya işlev adıyla tam olarak eşleşmelidir. Listeden ilgili satırı kopyalayabilirsiniz.

> [!Note]
> Diğer Kubernetes sağlayıcıları üzerinde çalıştırmak için, `values.yaml` içindeki Elakes arama storageClassName ' i sağlayıcının önerdiği bir değere uyacak şekilde değiştirin.

## <a name="visualize-data"></a>Verileri görselleştirme

Azure Veri Gezgini, kibana için bir veri kaynağı olarak yapılandırıldığında, verileri incelemek için kibana kullanabilirsiniz. 

1. Kibana ' de, sol menüden **bul** sekmesini seçin.

1. Sol aşağı açılan listeden, araştırmak istediğiniz veri kaynağını tanımlayan bir dizin kalıbı (Bu örnekte bir Azure Veri Gezgini tablosu) seçin.
    
   ![Bir dizin kalıbı seçin](media/k2bridge/k2bridge-select-an-index-pattern.png)

1. Verilerinizde zaman filtresi alanı varsa, zaman aralığını belirtebilirsiniz. Sayfanın sağ üst kısmında bir zaman filtresi ayarlayın. Varsayılan olarak, bulma, son 15 dakikalık verileri gösterir.

   ![Zaman filtresi](media/k2bridge/k2bridge-time-filter.png)
    
1. Sonuçlar tablosu ilk 500 kaydı gösterir. Bir belgeyi, alan verilerini JSON veya tablo biçimlerinde incelemek için genişletebilirsiniz.

   ![Bir kaydı Genişlet](media/k2bridge/k2bridge-expand-record.png)

1. Varsayılan olarak, sonuçlar tablosu belge _source ve zaman alanı (varsa) için sütunlar içerir. Sol kenar çubuğundaki alan adının yanındaki **Ekle** ' yi seçerek sonuçlar tablosuna eklenecek belirli sütunları seçebilirsiniz.

   ![Belirli sütunlar](media/k2bridge/k2bridge-specific-columns.png)
    
1. Sorgu çubuğunda, verileri şu şekilde arayabilirsiniz:
    * Arama terimi girme
    * Lucene sorgu söz dizimini kullanma. 
    Örnek:
        * Bu değeri içeren tüm kayıtları bulmak için "hata" araması yapın. 
        * Durum değeri 200 olan tüm kayıtları almak için "durum: 200" ifadesini arayın. 
    * Mantıksal işleçleri (ve, veya DEĞIL) kullanma
    * Joker karakterler (yıldız "\*" veya soru işareti "?") kullanılıyor Örneğin:
        * Sorgu `"destination_city: L*"`, hedef şehir değerinin "l" ile başladığı kayıtlarla eşleşir (K2Bridge büyük/küçük harfe duyarlı değildir).

    ![Sorgu çalıştırma](media/k2bridge/k2bridge-run-query.png)
    
    > [!Tip]
    > Arama [sırasında daha](https://github.com/microsoft/K2Bridge/blob/master/docs/searching.md)fazla arama kuralı ve mantığı bulabilirsiniz.

1. Arama sonuçlarınızı filtrelemek için sayfanın sağ kenar çubuğundaki **alan listesini** kullanın. 
    Alan listesi şu şekilde görebileceğiniz yerdir:
    * Alan için en üstteki beş değer
    * Alanı içeren kayıt sayısı
    * Her bir değeri içeren kayıt yüzdesi. 
    
    >[!Tip]
    > Belirli bir değere sahip tüm kayıtları bulmak için (+) büyüteç simgesini kullanın.
    
    ![Alan listesi](media/k2bridge/k2bridge-field-list.png)
   
    Sonuçlar tablosundaki her bir kaydın sonuçlar tablosu biçim görünümündeki (+) büyüteç simgesini kullanarak da sonuçları filtreleyebilirsiniz.
    
     ![Tablo listesi](media/k2bridge/k2bridge-table-list.png)
    
1. Aramanızı **kaydetmek** veya **paylaşmak** için seçin.

     ![Aramayı Kaydet](media/k2bridge/k2bridge-save-search.png)
