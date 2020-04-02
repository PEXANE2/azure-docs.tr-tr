---
title: Apache Kafka REST proxy - Azure HDInsight
description: Azure HDInsight'ta Kafka REST proxy'sini kullanarak Apache Kafka işlemlerini nasıl gerçekleştireceğinizi öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: hrasheed
ms.service: hdinsight
ms.topic: conceptual
ms.date: 04/01/2020
ms.openlocfilehash: 8997b385960c58b17747dfcfced74010af80550b
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80548189"
---
# <a name="interact-with-apache-kafka-clusters-in-azure-hdinsight-using-a-rest-proxy"></a>REST proxy kullanarak Azure HDInsight'taki Apache Kafka kümeleriyle etkileşimkurun

Kafka REST Proxy, HTTP üzerinden bir REST API üzerinden Kafka kümenizle etkileşimkurmanızı sağlar. Bu, Kafka müşterilerinizin sanal ağınızın dışında olabileceği anlamına gelir. Ayrıca, istemciler Kafka kitaplıklarına güvenmek yerine Kafka kümesine mesaj göndermek ve almak için basit HTTP aramaları yapabilir. Bu öğretici, bir REST proxy etkin Kafka küme oluşturmak ve REST proxy aramaları nasıl gösteren bir örnek kod sağlamak nasıl gösterecektir.

## <a name="rest-api-reference"></a>REST API başvurusu

Kafka REST API tarafından desteklenen operasyonların tam özellikleri için [HDInsight Kafka REST Proxy API Referans'a](https://docs.microsoft.com/rest/api/hdinsight-kafka-rest-proxy)bakın.

## <a name="background"></a>Arka plan

![Kafka REST proxy mimarisi](./media/rest-proxy/rest-proxy-architecture.png)

API tarafından desteklenen operasyonların tam özellikleri için, [Lütfen Apache Kafka REST Proxy API](https://docs.microsoft.com/rest/api/hdinsight-kafka-rest-proxy)bakın.

### <a name="rest-proxy-endpoint"></a>REST Proxy bitiş noktası

REST proxy ile HDInsight Kafka kümesi oluşturmak, Azure portalındaki HDInsight kümenizde bulabileceğiniz kümeniz için yeni bir genel bitiş noktası oluşturur.

### <a name="security"></a>Güvenlik

Kafka REST proxy'sine erişim Azure Active Directory güvenlik gruplarıyla yönetilir. REST proxy etkinken Kafka kümesini oluştururken, REST bitiş noktasına erişimi olması gereken Azure Etkin Dizin güvenlik grubunu sağlarsınız. REST proxy'sine erişmesi gereken Kafka istemcileri (uygulamalar) grup sahibi tarafından bu gruba kaydedilmelidir. Grup sahibi bunu Portal veya PowerShell üzerinden yapabilir.

REST proxy bitiş noktasına istekte bulunmadan önce, istemci uygulaması doğru güvenlik grubunun üyeliğini doğrulamak için bir OAuth belirteci almalıdır. Aşağıda, OAuth jetonunun nasıl alındığını gösteren bir [İstemci uygulama örneği](#client-application-sample) bulun. İstemci uygulaması OAuth belirteci ne zaman, onlar REST proxy yapılan HTTP isteği bu belirteci geçmek gerekir.

> [!NOTE]  
> AAD güvenlik grupları hakkında daha fazla bilgi edinmek için [Azure Etkin Dizin gruplarını kullanarak uygulama ve kaynak erişimini yönet'e](../../active-directory/fundamentals/active-directory-manage-groups.md)bakın. OAuth belirteçlerinin nasıl çalıştığı hakkında daha fazla bilgi için [bkz.](../../active-directory/develop/v1-protocols-oauth-code.md)

## <a name="prerequisites"></a>Ön koşullar

1. Bir uygulamayı Azure AD'ye kaydedin. Kafka REST proxy ile etkileşim kurmak için yazdığınız istemci uygulamaları, Azure'a kimlik doğrulamak için bu uygulamanın kimliğini ve gizli sini kullanır.

1. Bir Azure AD güvenlik grubu oluşturun ve Azure AD'de kaydettiğiniz uygulamayı grubun "üyesi" olarak güvenlik grubuna ekleyin. Bu güvenlik grubu, hangi uygulamaların REST proxy ile etkileşime girebilmek için izin verildiğini denetlemek için kullanılır. Azure REKLAM grupları oluşturma hakkında daha fazla bilgi için temel [bir grup oluştur ve Azure Etkin Dizini'ni kullanarak üye ekleyin'](../../active-directory/fundamentals/active-directory-groups-create-azure-portal.md)e bakın.

    Grubu doğrulama türü "Güvenlik" ![Güvenlik Grubu](./media/rest-proxy/rest-proxy-group.png)

    Bu uygulamanın Grup ![ÜyeSayısı'nın üyesi olduğunu doğrulayın](./media/rest-proxy/rest-proxy-membergroup.png)

## <a name="create-a-kafka-cluster-with-rest-proxy-enabled"></a>REST proxy etkin leştirilmiş bir Kafka kümesi oluşturma

1. Kafka küme oluşturma iş akışı sırasında"Güvenlik + ağ" sekmesinde "Kafka REST proxy'sini etkinleştir" seçeneğini işaretleyin.

     ![Kafka REST proxy'sini etkinleştirin ve güvenlik grubunu seçin](./media/rest-proxy/azure-portal-cluster-security-networking-kafka-rest.png)

1. **Güvenlik Grubu Seç'i**tıklatın. Güvenlik grupları listesinden, REST proxy'sine erişmek istediğiniz güvenlik grubunu seçin. Uygun güvenlik grubunu bulmak için arama kutusunu kullanabilirsiniz. Alttaki **Seç** düğmesini tıklatın.

     ![Kafka REST proxy'sini etkinleştirin ve güvenlik grubunu seçin](./media/rest-proxy/azure-portal-cluster-security-networking-kafka-rest2.png)

1. [Azure portalını kullanarak Azure HDInsight'ta Apache Kafka kümesi oluştur'da](https://docs.microsoft.com/azure/hdinsight/kafka/apache-kafka-get-started)açıklandığı gibi kümenizi oluşturmak için kalan adımları tamamlayın.

1. Küme oluşturulduktan sonra Kafka REST proxy URL'sini kaydetmek için küme özelliklerine gidin.

     ![REST proxy URL'sini görüntüle](./media/rest-proxy/apache-kafka-rest-proxy-view-proxy-url.png)

## <a name="client-application-sample"></a>İstemci başvuru örneği

Kafka kümenizdeki REST proxy ile etkileşimde kalmak için aşağıdaki python kodunu kullanabilirsiniz. Kod örneğini kullanmak için aşağıdaki adımları izleyin:

1. Örnek kodu Python yüklü bir makineye kaydedin.
1. Yürütme `pip3 install adal` ve `pip install msrestazure`.
1. Kod bölümünü *değiştirin Bu özellikleri yapılandırın* ve ortamınız için aşağıdaki özellikleri güncelleştirin:
    1.    *Kiracı Kimliği* – Aboneliğinizin olduğu Azure kiracısı.
    1.    *Müşteri Kimliği* – Güvenlik grubuna kaydettiğiniz başvurunun kimliği.
    1.    *Client Secret* – Güvenlik grubuna kaydettiğiniz uygulamanın sırrı
    1.    *Kafkarest_endpoint* – [dağıtım bölümünde](#create-a-kafka-cluster-with-rest-proxy-enabled)açıklandığı gibi küme genel bakış "özellikleri" sekmesinden bu değeri almak. Bu aşağıdaki biçimde olmalıdır -`https://<clustername>-kafkarest.azurehdinsight.net`
1. Komut satırından, python dosyasını`python <filename.py>`

Bu kod aşağıdakileri yapar:

1. Azure AD'den bir OAuth belirteci getirir
1. Kafka REST proxy'ye nasıl istekte bulunulur gösterilmektedir

Python'da OAuth belirteçleri alma hakkında daha fazla bilgi için [Python AuthenticationContext sınıfına](https://docs.microsoft.com/python/api/adal/adal.authentication_context.authenticationcontext?view=azure-python)bakın. Kafka REST proxy'si aracılığıyla oluşturulmayan veya silinmemiş konular burada yansıtılırken bir gecikme görebilirsiniz. Bu gecikme önbellek yenileme nedeniyle.

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

Kıvırma komutunu kullanarak AZURE for REST proxy'den nasıl bir belirteç alacağınız hakkında başka bir örneği aşağıda bulabilirsiniz. Bir belirteç `resource=https://hib.azurehdinsight.net` alırken belirtilen lere ihtiyacımız olduğuna dikkat edin.

```cmd
curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -d 'client_id=<clientid>&client_secret=<clientsecret>&grant_type=client_credentials&resource=https://hib.azurehdinsight.net' 'https://login.microsoftonline.com/<tenantid>/oauth2/token'
```

## <a name="next-steps"></a>Sonraki adımlar

* [Kafka REST vekil API başvuru belgeleri](https://docs.microsoft.com/rest/api/hdinsight-kafka-rest-proxy/)
