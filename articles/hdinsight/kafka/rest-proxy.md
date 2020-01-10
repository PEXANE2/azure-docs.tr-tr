---
title: Apache Kafka REST proxy-Azure HDInsight
description: Azure HDInsight üzerinde Kafka REST proxy kullanarak Apache Kafka işlemlerini nasıl gerçekleştireceğinizi öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: hrasheed
ms.service: hdinsight
ms.topic: conceptual
ms.date: 12/17/2019
ms.openlocfilehash: a64d03ebe7c8bbb4cfa9c7bd63a678892250373d
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75482874"
---
# <a name="interact-with-apache-kafka-clusters-in-azure-hdinsight-using-a-rest-proxy"></a>REST proxy kullanarak Azure HDInsight 'ta Apache Kafka kümeleriyle etkileşim kurma

Kafka REST proxy, HTTP üzerinden bir REST API aracılığıyla Kafka kümeniz ile etkileşim kurmanıza olanak sağlar. Bu, Kafka istemcilerinizin sanal ağ dışında olabileceği anlamına gelir. Ayrıca, istemciler Kafka kitaplıklarına güvenmek yerine Kafka kümesine ileti göndermek ve almak için basit HTTP çağrıları yapabilir.  

## <a name="background"></a>Arka plan

### <a name="architecture"></a>Mimari

REST proxy olmadan Kafka istemcilerinin Kafka kümesi veya eşlenmiş VNet ile aynı VNet 'te olması gerekir. REST proxy, her yerde bulunan veri üreticileri veya tüketicilere bağlanmanızı sağlar. REST proxy 'yi dağıtmak kümeniz için yeni bir genel uç nokta oluşturur ve bu, Portal ayarlarınızda bulabilirsiniz.

API tarafından desteklenen işlemlerin tam belirtimi için lütfen [Apache Kafka Rest Proxy API 'sine](https://docs.microsoft.com/rest/api/hdinsight-kafka-rest-proxy)bakın.

### <a name="security"></a>Güvenlik

Kafka REST proxy 'sine erişim Azure Active Directory güvenlik grupları ile yönetilir. Daha fazla bilgi için bkz. [Azure Active Directory grupları kullanarak uygulama ve kaynak erişimini yönetme](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-manage-groups).

REST proxy etkinken Kafka kümesini oluştururken REST uç noktasına erişimi olması gereken AAD güvenlik grubunu sağlarsınız. REST proxy 'sine erişmesi gereken Kafka istemcileri (uygulamalar) Grup sahibi tarafından bu gruba kaydedilmelidir. Grup sahibi bunu portal veya PowerShell aracılığıyla yapabilir.

REST proxy uç noktasına istek yapmadan önce, istemci uygulamanın doğru güvenlik grubunun üyeliğini doğrulamak için bir OAuth belirteci alması gerekir. OAuth belirteçlerinin nasıl çalıştığı hakkında daha fazla bilgi için bkz. [oauth 2,0 kod verme akışını kullanarak Azure Active Directory Web uygulamalarına erişimi yetkilendirme](../../active-directory/develop/v1-protocols-oauth-code.md). Python 'da OAuth belirteci getirme örneği için bkz. [istemci uygulaması örneği](#client-application-sample)

İstemci uygulaması OAuth belirtecine sahip olduktan sonra, bu belirteci REST proxy 'ye yapılan HTTP isteğinde iletmeleri gerekir.

## <a name="prerequisites"></a>Ön koşullar

1. Bir uygulamayı Azure AD'ye kaydedin. Kafka REST proxy ile etkileşimde bulunmak için yazdığınız istemci uygulamaları, Azure 'da kimlik doğrulaması yapmak için bu uygulamanın KIMLIĞINI ve parolasını kullanacaktır.
1. Azure AD güvenlik grubu oluşturun ve Azure AD 'ye kaydettiğiniz uygulamayı güvenlik grubuna ekleyin. Bu güvenlik grubu, REST proxy ile etkileşime girmesine izin verilen uygulamaları denetlemek için kullanılacaktır. Azure AD grupları oluşturma hakkında daha fazla bilgi için bkz. [temel Grup oluşturma ve Azure Active Directory kullanarak üye ekleme](../../active-directory/fundamentals/active-directory-groups-create-azure-portal.md).

## <a name="create-a-kafka-cluster-with-rest-proxy-enabled"></a>REST proxy etkinken bir Kafka kümesi oluşturma

1. Kafka kümesi oluşturma iş akışı sırasında, "güvenlik + ağ" sekmesinde "Kafka REST proxy 'yi etkinleştir" seçeneğini işaretleyin.

     ![Kafka REST proxy 'yi etkinleştirin ve güvenlik grubunu seçin](./media/apache-kafka-rest-proxy/apache-kafka-rest-proxy-enable.png)

1. **Güvenlik grubu seç**' e tıklayın. Güvenlik grupları listesinden REST proxy 'sine erişimi olmasını istediğiniz güvenlik grubunu seçin. Uygun güvenlik grubunu bulmak için arama kutusunu kullanabilirsiniz. Alttaki **Seç** düğmesine tıklayın.

     ![Kafka REST proxy 'yi etkinleştirin ve güvenlik grubunu seçin](./media/apache-kafka-rest-proxy/apache-kafka-rest-proxy-select-security-group.png)

1. [Azure Portal kullanarak Azure HDInsight 'ta Apache Kafka kümesi oluşturma](https://docs.microsoft.com/azure/hdinsight/kafka/apache-kafka-get-started)bölümünde açıklandığı gibi kümenizi oluşturmak için kalan adımları izleyin.

1. Küme oluşturulduktan sonra, Kafka REST proxy URL 'sini kaydetmek için küme özelliklerine gidin.

     ![REST proxy URL 'sini görüntüle](./media/apache-kafka-rest-proxy/apache-kafka-rest-proxy-view-proxy-url.png)

## <a name="client-application-sample"></a>İstemci uygulaması örneği

Kafka kümenizdeki REST proxy ile etkileşim kurmak için aşağıdaki python kodunu kullanabilirsiniz. Bu kod aşağıdakileri yapar:

1. Azure AD 'den bir OAuth belirteci getirir
1. Belirtilen konuyu oluşturur
1. Bu konuya ileti gönderir
1. Bu konudaki iletileri tüketir

Python 'da OAuth belirteçleri alma hakkında daha fazla bilgi için bkz. [Python AuthenticationContext sınıfı](https://docs.microsoft.com/python/api/adal/adal.authentication_context.authenticationcontext?view=azure-python). Kafka REST proxy 'si aracılığıyla oluşturulmamış veya silinmediği konular orada yansıtıldığından bir gecikme görebilirsiniz. Bu gecikme, önbelleğin yenilenmesi nedeniyle yapılır.

```python
#Required python packages
#pip3 install adal
#pip install msrestazure

import adal
from msrestazure.azure_active_directory import AdalAuthentication
from msrestazure.azure_cloud import AZURE_PUBLIC_CLOUD
import requests

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

#getting token
login_endpoint = AZURE_PUBLIC_CLOUD.endpoints.active_directory
resource = "https://hib.azurehdinsight.net"
context = adal.AuthenticationContext(login_endpoint + '/' + tenant_id)

token = context.acquire_token_with_client_credentials(
    resource,
    client_id,
    client_secret)

accessToken = 'Bearer ' + token['accessToken']

print(accessToken)

# relative url
getstatus = "/v1/metadata/topics"
request_url = kafkarest_endpoint + getstatus

# sending get request and saving the response as response object
response = requests.get(request_url, headers={'Authorization': accessToken})
print(response.content)
```

Kod örneğini kullanmak için şu adımları izleyin:

1. Örnek kodu Python yüklü bir makineye kaydedin.
1. `pip3 install adal` ve `pip install msrestazure`yürüterek gerekli Python bağımlılıklarını yükler.
1. Kodu değiştirin ve ortamınız için aşağıdaki özellikleri güncelleştirin:
    1.  *KIRACı kimliği* : aboneliğinizin bulunduğu Azure kiracısı.
    1.  *ISTEMCI kimliği* : güvenlik grubuna KAYDETTIĞINIZ uygulamanın kimliği.
    1.  *Istemci gizli anahtarı* : güvenlik grubunda kaydettiğiniz uygulamanın gizli anahtarı
    1.  *Kafkarest_endpoint* – [dağıtım bölümünde](#create-a-kafka-cluster-with-rest-proxy-enabled)açıklandığı gibi kümeye genel bakış ' daki "Özellikler" sekmesinden bu değeri alın. Şu biçimde olmalıdır: `https://<clustername>-kafkarest.azurehdinsight.net`
3. Komut satırından, `python <filename.py>` yürüterek Python dosyasını yürütün

## <a name="next-steps"></a>Sonraki adımlar

* [Kafka REST Proxy API başvuru belgeleri](https://docs.microsoft.com/rest/api/hdinsight-kafka-rest-proxy/)
* [Öğretici: Apache Kafka Producer ve tüketici API 'Lerini kullanma](apache-kafka-producer-consumer-api.md)