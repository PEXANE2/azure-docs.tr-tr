---
title: Apache Kafka REST proxy - Azure HDInsight
description: Azure HDInsight'ta Kafka REST proxy'sini kullanarak Apache Kafka işlemlerini nasıl yapacağınızı öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: hrasheed
ms.service: hdinsight
ms.topic: conceptual
ms.date: 04/03/2020
ms.openlocfilehash: 265e15713f8159e370ef22a197ffe931200a88f7
ms.sourcegitcommit: 31e9f369e5ff4dd4dda6cf05edf71046b33164d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81759001"
---
# <a name="interact-with-apache-kafka-clusters-in-azure-hdinsight-using-a-rest-proxy"></a>REST proxy kullanarak Azure HDInsight'taki Apache Kafka kümeleriyle etkileşimkurun

Kafka REST Proxy, HTTP üzerinden bir REST API üzerinden Kafka kümenizle etkileşimkurmanızı sağlar. Bu eylem, Kafka müşterilerinizin sanal ağınızın dışında olabileceği anlamına gelir. İstemciler Kafka kitaplıklarına güvenmek yerine Kafka kümesine basit HTTP aramaları yapabilir. Bu makalede, nasıl bir REST proxy kafka küme etkin oluşturmak için gösterecektir. Ayrıca, REST proxy'ye nasıl arama yapılacağını gösteren bir örnek kod sağlar.

## <a name="rest-api-reference"></a>REST API başvurusu

Kafka REST API tarafından desteklenen operasyonlar için [HDInsight Kafka REST Proxy API Referans'a](https://docs.microsoft.com/rest/api/hdinsight-kafka-rest-proxy)bakın.

## <a name="background"></a>Arka plan

![Kafka REST proxy tasarımı](./media/rest-proxy/rest-proxy-architecture.png)

API tarafından desteklenen operasyonların tam belirtimi için, [Bkz. Apache Kafka REST Proxy API.](https://docs.microsoft.com/rest/api/hdinsight-kafka-rest-proxy)

### <a name="rest-proxy-endpoint"></a>REST Proxy bitiş noktası

REST proxy ile HDInsight Kafka kümesi oluşturmak, Azure portalındaki HDInsight küme **Özelliklerinizde** bulabileceğiniz kümeniz için yeni bir genel bitiş noktası oluşturur.

### <a name="security"></a>Güvenlik

Kafka REST proxy'sine erişim Azure Active Directory güvenlik gruplarıyla yönetilir. Kafka kümesini oluştururken, Azure AD güvenlik grubuna REST uç noktası erişimi sağlayın. REST proxy'sine erişimi olan Kafka istemcilerinin grup sahibi tarafından bu gruba kaydedilmesi gerekmektedir. Grup sahibi Portal veya PowerShell üzerinden kayıt yaptırabilir.

REST proxy bitiş noktası istekleri için, istemci uygulamaları bir OAuth belirteci almalısınız. Belirteç, güvenlik grubu üyeliğini doğrulamak için kullanılır. Aşağıda, OAuth jetonunun nasıl alındığını gösteren bir [İstemci uygulama örneği](#client-application-sample) bulun. İstemci uygulaması, HTTP isteğindeki OAuth belirteci'ni REST proxy'sine geçirir.

> [!NOTE]  
> AAD güvenlik grupları hakkında daha fazla bilgi edinmek için [Azure Etkin Dizin gruplarını kullanarak uygulama ve kaynak erişimini yönet'e](../../active-directory/fundamentals/active-directory-manage-groups.md)bakın. OAuth belirteçlerinin nasıl çalıştığı hakkında daha fazla bilgi için [bkz.](../../active-directory/develop/v1-protocols-oauth-code.md)

## <a name="prerequisites"></a>Ön koşullar

1. Bir uygulamayı Azure AD'ye kaydedin. Kafka REST proxy ile etkileşim kurmak için yazdığınız istemci uygulamaları, Azure'a kimlik doğrulamak için bu uygulamanın kimliğini ve gizli sini kullanır.

1. Bir Azure AD güvenlik grubu oluşturun. Azure AD'ye kaydettiğiniz uygulamayı grubun **bir üyesi** olarak güvenlik grubuna ekleyin. Bu güvenlik grubu, hangi uygulamaların REST proxy ile etkileşime girebilmek için izin verildiğini denetlemek için kullanılır. Azure REKLAM grupları oluşturma hakkında daha fazla bilgi için temel [bir grup oluştur ve Azure Etkin Dizini'ni kullanarak üye ekleyin'](../../active-directory/fundamentals/active-directory-groups-create-azure-portal.md)e bakın.

    Grubu doğrulayın tür **Güvenlik.**
    ![Güvenlik Grubu](./media/rest-proxy/rest-proxy-group.png)

    Bu uygulamanın Grup üyesi olduğunu doğrulayın.
    ![Üyeliği Kontrol Et](./media/rest-proxy/rest-proxy-membergroup.png)

## <a name="create-a-kafka-cluster-with-rest-proxy-enabled"></a>REST proxy etkin leştirilmiş bir Kafka kümesi oluşturma

1. Kafka küme oluşturma iş akışı sırasında, **Güvenlik + ağ** sekmesinde Kafka REST **proxy'yi etkinleştir** seçeneğini işaretleyin.

     ![Kafka REST proxy'sini etkinleştirin ve güvenlik grubunu seçin](./media/rest-proxy/azure-portal-cluster-security-networking-kafka-rest.png)

1. **Güvenlik Grubu Seç'i**tıklatın. Güvenlik grupları listesinden, REST proxy'sine erişmek istediğiniz güvenlik grubunu seçin. Uygun güvenlik grubunu bulmak için arama kutusunu kullanabilirsiniz. Alttaki **Seç** düğmesini tıklatın.

     ![Kafka REST proxy'sini etkinleştirin ve güvenlik grubunu seçin](./media/rest-proxy/azure-portal-cluster-security-networking-kafka-rest2.png)

1. [Azure portalını kullanarak Azure HDInsight'ta Apache Kafka kümesi oluştur'da](https://docs.microsoft.com/azure/hdinsight/kafka/apache-kafka-get-started)açıklandığı gibi kümenizi oluşturmak için kalan adımları tamamlayın.

1. Küme oluşturulduktan sonra Kafka REST proxy URL'sini kaydetmek için küme özelliklerine gidin.

     ![REST proxy URL'sini görüntüle](./media/rest-proxy/apache-kafka-rest-proxy-view-proxy-url.png)

## <a name="client-application-sample"></a>İstemci başvuru örneği

Kafka kümenizdeki REST proxy ile etkileşimde kalmak için aşağıdaki python kodunu kullanabilirsiniz. Kod örneğini kullanmak için aşağıdaki adımları izleyin:

1. Örnek kodu Python yüklü bir makineye kaydedin.
1. Yürütme yaparak gerekli python `pip3 install msal`bağımlılıkları yükleyin.
1. Kod bölümünü **değiştirin Bu özellikleri yapılandırın** ve ortamınız için aşağıdaki özellikleri güncelleştirin:

    |Özellik |Açıklama |
    |---|---|
    |Kiracı Kimliği|Aboneliğinizin olduğu Azure kiracısı.|
    |İstemci Kimliği|Güvenlik grubuna kaydettiğiniz uygulamanın kimliği.|
    |İstemci Gizli Anahtarı|Güvenlik grubuna kaydettiğiniz uygulamanın sırrı.|
    |Kafkarest_endpoint|[Dağıtım bölümünde](#create-a-kafka-cluster-with-rest-proxy-enabled)açıklandığı gibi küme genel bakış özellikleri **sekmesinden** bu değeri alın. Bu aşağıdaki biçimde olmalıdır -`https://<clustername>-kafkarest.azurehdinsight.net`|

1. Komut satırından, python dosyasını`sudo python3 <filename.py>`

Bu kod aşağıdaki eylemi yapar:

1. Azure AD'den bir OAuth belirteci getirir.
1. Kafka REST proxy'ye nasıl istekte bulunulur.

Python'da OAuth belirteçleri alma hakkında daha fazla bilgi için [Python AuthenticationContext sınıfına](https://docs.microsoft.com/python/api/adal/adal.authentication_context.authenticationcontext?view=azure-python)bakın. Kafka REST proxy'si aracılığıyla oluşturulmayan veya silinmemiş bir `topics` gecikme görebilirsiniz. Bu gecikme, önbellek yenileme nedeniyle.

```python
#Required python packages
#pip3 install msal

import msal

#--------------------------Configure these properties-------------------------------#
# Tenant ID for your Azure Subscription
tenant_id = 'ABCDEFGH-1234-1234-1234-ABCDEFGHIJKL'
# Your Client Application Id
client_id = 'XYZABCDE-1234-1234-1234-ABCDEFGHIJKL'
# Your Client Credentials
client_secret = 'password'
# kafka rest proxy -endpoint
kafkarest_endpoint = "https://<clustername>-kafkarest.azurehdinsight.net"
#--------------------------Configure these properties-------------------------------#

# Scope
scope = 'https://hib.azurehdinsight.net/.default'
#Authority
authority = 'https://login.microsoftonline.com/' + tenant_id

# Create a preferably long-lived app instance which maintains a token cache.
app = msal.ConfidentialClientApplication(
    client_id , client_secret, authority,
    #cache - For details on how look at this example: https://github.com/Azure-Samples/ms-identity-python-webapp/blob/master/app.py
    )

# The pattern to acquire a token looks like this.
result = None

result = app.acquire_token_for_client(scopes=[scope])

print(result)
accessToken = result['access_token']

# relative url
getstatus = "/v1/metadata/topics"
request_url = kafkarest_endpoint + getstatus

# sending get request and saving the response as response object
response = requests.get(request_url, headers={'Authorization': accessToken})
print(response.content)
```

Kıvırma komutunu kullanarak AZURE for REST proxy'den nasıl bir belirteç alacağınız hakkında başka bir örneği aşağıda bulabilirsiniz. **Bir belirteç `scope=https://hib.azurehdinsight.net/.default` alırken belirtilen lere ihtiyacımız olduğuna dikkat edin.**

```cmd
curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -d 'client_id=<clientid>&client_secret=<clientsecret>&grant_type=client_credentials&scope=https://hib.azurehdinsight.net/.default' 'https://login.microsoftonline.com/<tenantid>/oauth2/v2.0/token'
```

## <a name="next-steps"></a>Sonraki adımlar

* [Kafka REST vekil API başvuru belgeleri](https://docs.microsoft.com/rest/api/hdinsight-kafka-rest-proxy/)
