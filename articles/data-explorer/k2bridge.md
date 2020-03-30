---
title: Kibana'yı kullanarak Azure Veri Gezgini'nden gelen verileri görselleştirin
description: Bu makalede, Azure Veri Gezgini'ni Kibana için veri kaynağı olarak nasıl ayarlayabileceğinizi öğreneceksiniz
author: orspod
ms.author: orspodek
ms.reviewer: guregini
ms.service: data-explorer
ms.topic: conceptual
ms.date: 03/12/2020
ms.openlocfilehash: fac9c78607e50dca384670bf4cc08b50f723312b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80065603"
---
# <a name="visualize-data-from-azure-data-explorer-in-kibana-with-the-k2bridge-open-source-connector"></a>K2Bridge açık kaynak bağlayıcısıyla Kibana'daki Azure Veri Gezgini verilerini görselleştirin

K2Bridge (Kibana-Kusto Bridge), Veri kaynağı olarak Azure Veri Gezgini'ni kullanmanıza ve Bu verileri Kibana'da görselleştirmenize olanak tanır. K2Bridge, Kibana örneği ile Azure Veri Gezgini kümesi arasında proxy görevi gören [açık kaynak kodlu](https://github.com/microsoft/K2Bridge) kapsayıcılı bir uygulamadır. Bu makalede, bu bağlantıyı oluşturmak için K2Bridge nasıl kullanılacağı açıklanmaktadır.

K2Bridge, Kibana sorgularını Kusto Query Language'e (KQL) çevirir ve Azure Veri Gezgini sonuçlarını Kibana'ya geri gönderir. 

   ![grafik](media/k2bridge/k2bridge-chart.png)

K2Bridge, Kibana'nın Keşfet sekmesini destekler:
* Verileri arama ve keşfetme
* Sonuçları filtreleme
* Sonuç ızgarasında alan ekleme veya kaldırma
* Kayıt içeriğini görüntüleme
* Aramaları kaydetme ve paylaşma

Aşağıdaki resimde, K2Bridge tarafından Azure Veri Gezgini'ne bağlı bir Kibana örneği gösterilmektedir. Kibana'daki kullanıcı deneyimi değişmedi.

   [![Kibana Sayfası](media/k2bridge/k2bridge-kibana-page.png)](media/k2bridge/k2bridge-kibana-page.png#lightbox)

## <a name="prerequisites"></a>Ön koşullar

Kibana'daki Azure Veri Gezgini'nden gelen verileri görselleştirmeden önce aşağıdakileri hazır layın:

* [Helm V3](https://github.com/helm/helm#install), Kubernetes paket yöneticisi
* Azure Kubernetes Service (AKS) kümesi veya diğer Kubernetes kümesi (sürüm 1.14 ile sürüm 1.16 sınandı ve doğrulandı). AKS kümesine ihtiyacınız varsa, Azure [CLI'yi kullanarak](https://docs.microsoft.com/azure/aks/kubernetes-walkthrough) veya [Azure portalını kullanarak](https://docs.microsoft.com/azure/aks/kubernetes-walkthrough-portal) bir AKS kümesini dağıt'a bakın
* Bir [Azure Veri Gezgini kümesi](create-cluster-database-portal.md), dahil:
    * Azure Veri Gezgini kümesinin URL'si 
    * Veritabanı adı
    
* Şunları da dahil olmak üzere Azure Veri Gezgini'nde verileri görüntülemeye yetkili bir Azure AD hizmeti müdürü:
    * İstemci Kimliği 
    * Müşteri Sırrı

    'Görüntüleyici' izniolan bir hizmet sorumlusu önerilir. Daha yüksek izinler kullanmak önerilmez.

    * [Azure AD hizmet ilkesi için kümenin görünüm izinlerini ayarlayın.](https://docs.microsoft.com/azure/data-explorer/manage-database-permissions#manage-permissions-in-the-azure-portal)

    Azure AD hizmet ilkesi hakkında daha fazla bilgi [için](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#create-an-azure-active-directory-application)bkz.

## <a name="run-k2bridge-on-azure-kubernetes-service-aks"></a>Azure Kubernetes Hizmetinde K2Bridge Çalıştır (AKS)

Varsayılan olarak, K2Bridges'in Miğfer grafiği, Microsoft'un Kapsayıcı Kayıt Defteri'nde (MCR) bulunan herkese açık bir görüntüye başvurur. MCR herhangi bir kimlik belgesi gerektirmez ve kutunun dışında çalışır.

1. Gerekli Helm grafiklerini indirin.

1. Helm'e Elasticsearch bağımlılığını ekleyin. 
    Elasticsearch bağımlılığının nedeni, K2Bridge'in meta verilerle ilgili istekleri (dizin desenleri ve kaydedilmiş sorgular gibi) hizmet etmek için küçük bir dahili Elastik Arama örneği kullanmasıdır. Bu iç örnekte hiçbir iş verisi kaydedilmez ve bu bir uygulama ayrıntısı olarak kabul edilebilir. 

    1. Helm'e Elasticsearch bağımlılığını eklemek için:

        ```bash
        helm repo add elastic https://helm.elastic.co
        helm repo update
        ```

    1. GitHub deposunun grafik dizininin altından K2Bridge grafiğini almak için:
        1. [GitHub'dan](https://github.com/microsoft/K2Bridge)depoyu klonla.
        1. K2Bridges kök deposu dizine gidin.
        1. Çalıştırın:

            ```bash
            helm dependency update charts/k2bridge
            ```

1. K2Bridge'i dağıtın:

    1. Değişkenleri ortamınız için doğru değerlerle ayarlayın:

        ```bash
        ADX_URL=[YOUR_ADX_CLUSTER_URL] #For example, https://mycluster.westeurope.kusto.windows.net
        ADX_DATABASE=[YOUR_ADX_DATABASE_NAME]
        ADX_CLIENT_ID=[SERVICE_PRINCIPAL_CLIENT_ID]
        ADX_CLIENT_SECRET=[SERVICE_PRINCIPAL_CLIENT_SECRET]
        ADX_TENANT_ID=[SERVICE_PRINCIPAL_TENANT_ID]
        ```

    1. (İsteğe bağlı) Azure Uygulama Öngörüleri telemetrisini etkinleştirin. 
        Azure Application Insights'ı ilk kez kullanıyorsanız, önce [bir Application Insights kaynağı oluşturmanız](https://docs.microsoft.com/azure/azure-monitor/app/create-new-resource)gerekir. [Enstrümantasyon anahtarını](https://docs.microsoft.com/azure/azure-monitor/app/create-new-resource#copy-the-instrumentation-key) bir değişkene kopyalamanız gerekir: 

        ```bash
        APPLICATION_INSIGHTS_KEY=[INSTRUMENTATION_KEY]
        COLLECT_TELEMETRY=true
        ```

    1. <a name="install-k2bridge-chart"></a>K2Bridge grafiğini yükleyin:

        ```bash
        helm install k2bridge charts/k2bridge -n k2bridge --set image.repository=$REPOSITORY_NAME/$CONTAINER_NAME --set settings.adxClusterUrl="$ADX_URL" --set settings.adxDefaultDatabaseName="$ADX_DATABASE" --set settings.aadClientId="$ADX_CLIENT_ID" --set settings.aadClientSecret="$ADX_CLIENT_SECRET" --set settings.aadTenantId="$ADX_TENANT_ID" [--set image.tag=latest] [--set privateRegistry="$IMAGE_PULL_SECRET_NAME"] [--set settings.collectTelemetry=$COLLECT_TELEMETRY]
        ```

        [Yapılandırma'da](https://github.com/microsoft/K2Bridge/blob/master/docs/configuration.md) yapılandırma seçeneklerinin tam kümesini bulabilirsiniz.

    1. Komut çıkışı, Kibana'yı dağıtmak için çalışacak bir sonraki Helm komutunu önerecektir. İsteğe bağlı olarak çalıştırın:

        ```bash
        helm install kibana elastic/kibana -n k2bridge --set image=docker.elastic.co/kibana/kibana-oss --set imageTag=6.8.5 --set elasticsearchHosts=http://k2bridge:8080
        ```
        
    1. Localhost'ta Kibana'ya erişmek için bağlantı noktasını kullanarak bağlantı noktasını kullanın: 

        ```bash
        kubectl port-forward service/kibana-kibana 5601 --namespace k2bridge
        ```
        
    1. Kibana'ya http://127.0.0.1:5601göz atarak bağlanın.

    1. Kibana'yı son kullanıcılara gösterin. Bunu yapmak için birden çok yöntem vardır. Kullandığınız yöntem büyük ölçüde kullanım örneğinize bağlıdır.

        Örnek:

        Hizmeti LoadBalancer hizmeti olarak ortaya çıkarın. Bunu yapmak için, `--set service.type=LoadBalancer` K2Bridge Helm yükleme komutuna parametre ekleyin ([yukarıda).](#install-k2bridge-chart)        
    
        Ardından şunu çalıştırın:
        
        ```bash
        kubectl get service -w -n k2bridge
        ```
        
        Çıktı aşağıdaki gibi görünmelidir: 

        ```bash
        NAME            TYPE           CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
        kibana-kibana   LoadBalancer   xx.xx.xx.xx   <pending>      5601:30128/TCP   4m24s
        ```
 
        Daha sonra oluşturulan external-IP'yi kullanabilir ve tarayıcıyı 'ya açarak `<EXTERNAL-IP>:5601`Kibana'ya erişmek için kullanabilirsiniz.

1. Verilerinize erişmek için dizin desenlerini yapılandırın:  
Yeni bir Kibana örneğinde:
     1. Kibana'yı aç.
     1. Yönetim'e gidin.
     1. **Dizin Desenleri'ni**seçin. 
     1. Dizin deseni oluşturun.
Dizin adı, yıldız işareti olmadan tablo adı veya işlev adı ile tam olarak eşleşmelidir. Listeden ilgili satırı kopyalayabilirsiniz.

> [!Note]
> Diğer Kubernetes sağlayıcılarıüzerinde çalışmak için, Sağlayıcı tarafından `values.yaml` önerilene uyacak şekilde Elasticsearch storageClassName'yi değiştirin.

## <a name="visualize-data"></a>Verileri görselleştirme

Azure Veri Gezgini Kibana için bir veri kaynağı olarak yapılandırıldığınızda, verileri keşfetmek için Kibana'yı kullanabilirsiniz. 

1. Kibana'da, sol menüde **Keşfet** sekmesini seçin.

1. Sol açılan listeden, keşfetmek istediğiniz veri kaynağını tanımlayan bir dizin deseni (bu durumda, bir Azure Veri Gezgini tablosu) seçin.
    
   ![Dizin deseni seçin](media/k2bridge/k2bridge-select-an-index-pattern.png)

1. Verilerinizin zaman filtresi alanı varsa, zaman aralığını belirtebilirsiniz. Sayfanın sağ üst kısmında bir zaman filtresi ayarlayın. Varsayılan olarak, Discover son 15 dakikanın verilerini gösterir.

   ![Zaman filtresi](media/k2bridge/k2bridge-time-filter.png)
    
1. Sonuç tablosu ilk 500 kaydı gösterir. Bir belgeyi alan verilerini JSON veya tablo biçimlerinde incelemek için genişletebilirsiniz.

   ![Kaydı genişletme](media/k2bridge/k2bridge-expand-record.png)

1. Varsayılan olarak, sonuç tablosu belge _source ve zaman alanı (varsa) için sütunlar içerir. Sol kenar çubuğundaki alan adının yanına **ekle** seçeneğini seçerek sonuç tablosuna eklenecek belirli sütunları seçebilirsiniz.

   ![Belirli sütunlar](media/k2bridge/k2bridge-specific-columns.png)
    
1. Sorgu çubuğunda, verileri aşağıdakilere göre arayabilirsiniz:
    * Arama terimi girme
    * Lucene sorgu sözdizimini kullanma. 
    Örnek:
        * Bu değeri içeren tüm kayıtları bulmak için "hata" araması yapın. 
        * 200 durum değeri olan tüm kayıtları almak için "durum: 200" araması yapın. 
    * Mantıksal işleçleri kullanma (VE, VEYA, Not)
    * Joker karakterler (yıldız işareti \* " " veya soru işareti "?") kullanma Örneğin:
        * Sorgu, `"destination_city: L*"` hedef şehir değerinin "l" ile başladığı kayıtları eşleştirecektir (K2Bridge büyük/küçük harf duyarlı değildir).

    ![Sorgu çalıştırma](media/k2bridge/k2bridge-run-query.png)
    
    > [!Tip]
    > [Arama'da](https://github.com/microsoft/K2Bridge/blob/master/docs/searching.md)daha fazla arama kuralı ve mantık bulabilirsiniz.

1. Arama sonuçlarınızı filtrelemek için sayfanın sağ kenar çubuğundaki **alan listesini** kullanın. 
    Alan listesi görebileceğiniz yerdir:
    * Alan için en iyi beş değer
    * Alanı içeren kayıt sayısı
    * Her değeri içeren kayıtların yüzdesi. 
    
    >[!Tip]
    > Belirli bir değere sahip tüm kayıtları bulmak için (+) büyüteç simgesini kullanın.
    
    ![Alan listesi](media/k2bridge/k2bridge-field-list.png)
   
    Sonuçlar tablosundaki her kaydın (+) büyüteç simgesini kullanarak da sonuçlara filtre uygulayabilirsiniz.
    
     ![Tablo listesi](media/k2bridge/k2bridge-table-list.png)
    
1. **Aramanızı Kaydet** veya **Paylaş'ı** seçin.

     ![Aramayı kaydet](media/k2bridge/k2bridge-save-search.png)
