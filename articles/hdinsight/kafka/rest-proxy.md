---
title: Apache Kafka REST proxy-Azure HDInsight
description: Azure HDInsight üzerinde Kafka REST proxy kullanarak Apache Kafka işlemleri yapmayı öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: hrasheed
ms.service: hdinsight
ms.topic: conceptual
ms.date: 04/03/2020
ms.openlocfilehash: 265e15713f8159e370ef22a197ffe931200a88f7
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81759001"
---
# <a name="interact-with-apache-kafka-clusters-in-azure-hdinsight-using-a-rest-proxy"></a>REST proxy kullanarak Azure HDInsight 'ta Apache Kafka kümeleriyle etkileşim kurma

Kafka REST proxy, HTTP üzerinden bir REST API aracılığıyla Kafka kümeniz ile etkileşime girebilmenizi sağlar. Bu eylem, Kafka istemcilerinizin sanal ağınızın dışında olabileceği anlamına gelir. İstemciler, Kafka kitaplıklarına güvenmek yerine Kafka kümesine basit HTTP çağrıları yapabilir. Bu makalede, REST proxy etkin Kafka kümesinin nasıl oluşturulacağı gösterilmektedir. Ayrıca REST proxy 'ye nasıl çağrı yapılacağını gösteren örnek bir kod sağlar.

## <a name="rest-api-reference"></a>REST API başvurusu

Kafka REST API tarafından desteklenen işlemler için bkz. [HDInsight Kafka Rest Proxy API başvurusu](https://docs.microsoft.com/rest/api/hdinsight-kafka-rest-proxy).

## <a name="background"></a>Arka plan

![Kafka REST ara sunucu tasarımı](./media/rest-proxy/rest-proxy-architecture.png)

API tarafından desteklenen işlemlerin tam belirtimi için [Apache Kafka Rest Proxy API 'sine](https://docs.microsoft.com/rest/api/hdinsight-kafka-rest-proxy)bakın.

### <a name="rest-proxy-endpoint"></a>REST proxy uç noktası

REST proxy ile HDInsight Kafka kümesi oluşturma, kümeniz için yeni bir genel uç nokta oluşturur ve bu, Azure portal HDInsight kümesi **özelliklerinde** bulabilirsiniz.

### <a name="security"></a>Güvenlik

Kafka REST proxy 'sine erişim Azure Active Directory güvenlik grupları ile yönetilir. Kafka kümesini oluştururken REST uç noktası erişimi olan Azure AD güvenlik grubunu belirtin. REST proxy 'sine erişmesi gereken Kafka istemcileri, Grup sahibi tarafından bu gruba kaydedilmelidir. Grup sahibi portal veya PowerShell aracılığıyla kaydedebilir.

REST proxy uç noktası istekleri için, istemci uygulamaların bir OAuth belirteci alması gerekir. Belirteç, güvenlik grubu üyeliğini doğrulamak için kullanılır. Aşağıdaki bir OAuth belirtecinin nasıl alınacağını gösteren bir [istemci uygulaması örneği](#client-application-sample) bulun. İstemci uygulaması, HTTP isteğindeki OAuth belirtecini REST proxy 'ye geçirir.

> [!NOTE]  
> AAD güvenlik grupları hakkında daha fazla bilgi için bkz. [Azure Active Directory grupları kullanarak uygulama ve kaynak erişimini yönetme](../../active-directory/fundamentals/active-directory-manage-groups.md). OAuth belirteçlerinin nasıl çalıştığı hakkında daha fazla bilgi için bkz. [oauth 2,0 kod verme akışını kullanarak Azure Active Directory Web uygulamalarına erişimi yetkilendirme](../../active-directory/develop/v1-protocols-oauth-code.md).

## <a name="prerequisites"></a>Ön koşullar

1. Bir uygulamayı Azure AD'ye kaydedin. Kafka REST proxy ile etkileşimde bulunmak için yazdığınız istemci uygulamaları, Azure 'da kimlik doğrulaması yapmak için bu uygulamanın KIMLIĞINI ve parolasını kullanacaktır.

1. Bir Azure AD güvenlik grubu oluşturun. Azure AD 'ye kaydettiğiniz uygulamayı, grubun bir **üyesi** olarak güvenlik grubuna ekleyin. Bu güvenlik grubu, REST proxy ile etkileşime girmesine izin verilen uygulamaları denetlemek için kullanılacaktır. Azure AD grupları oluşturma hakkında daha fazla bilgi için bkz. [temel Grup oluşturma ve Azure Active Directory kullanarak üye ekleme](../../active-directory/fundamentals/active-directory-groups-create-azure-portal.md).

    Grubun **güvenlik**tür olduğunu doğrulayın.
    ![Güvenlik grubu](./media/rest-proxy/rest-proxy-group.png)

    Uygulamanın grubun üyesi olduğunu doğrulayın.
    ![Üyeliği denetle](./media/rest-proxy/rest-proxy-membergroup.png)

## <a name="create-a-kafka-cluster-with-rest-proxy-enabled"></a>REST proxy etkinken bir Kafka kümesi oluşturma

1. Kafka kümesi oluşturma iş akışı sırasında, **güvenlik + ağ** sekmesinde, **Kafka Rest proxy 'yi etkinleştir** seçeneğini işaretleyin.

     ![Kafka REST proxy 'yi etkinleştirin ve güvenlik grubunu seçin](./media/rest-proxy/azure-portal-cluster-security-networking-kafka-rest.png)

1. **Güvenlik grubu seç**' e tıklayın. Güvenlik grupları listesinden REST proxy 'sine erişimi olmasını istediğiniz güvenlik grubunu seçin. Uygun güvenlik grubunu bulmak için arama kutusunu kullanabilirsiniz. Alttaki **Seç** düğmesine tıklayın.

     ![Kafka REST proxy 'yi etkinleştirin ve güvenlik grubunu seçin](./media/rest-proxy/azure-portal-cluster-security-networking-kafka-rest2.png)

1. [Azure Portal kullanarak Azure HDInsight 'ta Apache Kafka kümesi oluşturma](https://docs.microsoft.com/azure/hdinsight/kafka/apache-kafka-get-started)bölümünde açıklandığı gibi kümenizi oluşturmak için kalan adımları izleyin.

1. Küme oluşturulduktan sonra, Kafka REST proxy URL 'sini kaydetmek için küme özelliklerine gidin.

     ![REST proxy URL 'sini görüntüle](./media/rest-proxy/apache-kafka-rest-proxy-view-proxy-url.png)

## <a name="client-application-sample"></a>İstemci uygulaması örneği

Kafka kümenizdeki REST proxy ile etkileşim kurmak için aşağıdaki python kodunu kullanabilirsiniz. Kod örneğini kullanmak için şu adımları izleyin:

1. Örnek kodu Python yüklü bir makineye kaydedin.
1. Yürüterek `pip3 install msal`gerekli Python bağımlılıklarını yükler.
1. Kod bölümünü değiştirin **Bu özellikleri yapılandırın** ve ortamınız için aşağıdaki özellikleri güncelleştirin:

    |Özellik |Açıklama |
    |---|---|
    |Kiracı Kimliği|Aboneliğinizin bulunduğu Azure kiracısı.|
    |İstemci Kimliği|Güvenlik grubuna kaydettiğiniz uygulamanın KIMLIĞI.|
    |İstemci Gizli Anahtarı|Güvenlik grubuna kaydettiğiniz uygulamanın gizli anahtarı.|
    |Kafkarest_endpoint|[Dağıtım bölümünde](#create-a-kafka-cluster-with-rest-proxy-enabled)açıklandığı gibi kümeye genel bakış ' daki **Özellikler** sekmesinden bu değeri alın. Aşağıdaki biçimde olmalıdır:`https://<clustername>-kafkarest.azurehdinsight.net`|

1. Komut satırından, öğesini yürüterek Python dosyasını yürütün`sudo python3 <filename.py>`

Bu kod aşağıdaki eylemi yapar:

1. Azure AD 'den bir OAuth belirteci getirir.
1. Kafka REST proxy 'sine nasıl istek yapılacağını gösterir.

Python 'da OAuth belirteçleri alma hakkında daha fazla bilgi için bkz. [Python AuthenticationContext sınıfı](https://docs.microsoft.com/python/api/adal/adal.authentication_context.authenticationcontext?view=azure-python). Kafka REST proxy 'si aracılığıyla oluşturulmamış `topics` veya silinmediği sürece bir gecikme görebilirsiniz. Bu gecikme, önbellek yenileme nedeniyle oluşur.

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

Bir kıvrımlı komutu kullanarak REST proxy için Azure 'dan bir belirteç alma hakkında başka bir örnek aşağıda bulabilirsiniz. **Belirteç alınırken, belirtilen için `scope=https://hib.azurehdinsight.net/.default` gerekli olduğunu unutmayın.**

```cmd
curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -d 'client_id=<clientid>&client_secret=<clientsecret>&grant_type=client_credentials&scope=https://hib.azurehdinsight.net/.default' 'https://login.microsoftonline.com/<tenantid>/oauth2/v2.0/token'
```

## <a name="next-steps"></a>Sonraki adımlar

* [Kafka REST Proxy API başvuru belgeleri](https://docs.microsoft.com/rest/api/hdinsight-kafka-rest-proxy/)
