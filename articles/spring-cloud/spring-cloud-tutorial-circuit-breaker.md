---
title: 'Öğretici: Azure Spring Cloud ile devre kesici panosu kullanma'
description: Devre kesici panosunu Azure Spring Cloud ile nasıl kullanacağınızı öğrenin.
author: MikeDodaro
ms.author: brendm
ms.service: spring-cloud
ms.topic: tutorial
ms.date: 04/06/2020
ms.openlocfilehash: e5fbb14477275ca329243797b75550cfe92077b6
ms.sourcegitcommit: 595cde417684e3672e36f09fd4691fb6aa739733
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2020
ms.locfileid: "83701563"
---
# <a name="use-circuit-breaker-dashboard-with-azure-spring-cloud"></a>Azure Spring Cloud ile devre kesici panosunu kullanma
Yay [bulutu Netflix Turbin](https://github.com/Netflix/Turbine) , birden çok [hyte x](https://github.com/Netflix/Hystrix) ölçüm akışını toplamak için yaygın olarak kullanılır. böylece, akışlar hyçabax Panosu kullanılarak tek bir görünümde izlenebilir. Bu öğreticide, Azure Spring Cloud 'da nasıl kullanılacağı gösterilmektedir.
> [!NOTE]
> Netflix Hyçabax birçok mevcut yay bulutu uygulamasında yaygın olarak kullanılır, ancak artık etkin bir geliştirme aşamasındadır. Yeni proje geliştiriyorsanız, bunun yerine [resilience4j](https://github.com/resilience4j/resilience4j)gibi Spring Cloud devre kesici uygulamalarını kullanın. Bu öğreticide gösterilen türbin 'ten farklı olarak, yeni yay bulutu devre kesici çerçevesi, ölçüm verileri işlem hattının tüm uygulamalarını mikro ölçüm olarak birleştirir. Azure Spring Cloud 'da mikro ölçüm 'i desteklemeye devam ediyoruz, bu nedenle bu öğretici bu öğreticide ele alınmayacak.

## <a name="prepare-your-sample-applications"></a>Örnek uygulamalarınızı hazırlama
Örnek, bu [depodan](https://github.com/StackAbuse/spring-cloud/tree/master/spring-turbine)ele geçirildi.

Örnek depoyu geliştirme ortamınıza kopyalayın:
```
git clone https://github.com/Azure-Samples/Azure-Spring-Cloud-Samples.git
cd Azure-Spring-Cloud-Samples/hystrix-turbine-sample
```

Bu öğreticide kullanılacak 3 uygulamayı oluşturun:
* Kullanıcı Hizmeti: tek bir/personized/{ID} uç noktasına sahip basit bir REST hizmeti
* öneri-Service: tek bir uç noktası olan ve Kullanıcı Hizmeti tarafından çağrılacak olan basit bir REST hizmeti.
* hyçabax-Turbin: Hyçabax akışlarını ve diğer hizmetlerden Hyçabax ölçümleri akışını içeren bir Turbin hizmetini görüntüleyen bir Hybir x panosu hizmeti.
```
mvn clean package -D skipTests -f user-service/pom.xml
mvn clean package -D skipTests -f recommendation-service/pom.xml
mvn clean package -D skipTests -f hystrix-turbine/pom.xml
```
## <a name="provision-your-azure-spring-cloud-instance"></a>Azure Spring Cloud örneğinizi sağlama
[Azure CLI üzerinde bir hizmet örneği sağlayın](https://docs.microsoft.com/azure/spring-cloud/spring-cloud-quickstart-launch-app-cli#provision-a-service-instance-on-the-azure-cli)yordamını izleyin.

## <a name="deploy-your-applications-to-azure-spring-cloud"></a>Uygulamalarınızı Azure Spring Cloud 'a dağıtın
Bu uygulamalar **config Server**kullanmaz, bu nedenle Azure Spring Cloud Için **yapılandırma sunucusu** ayarlanması gerekmez.  Oluşturun ve aşağıdaki gibi dağıtın:
```azurecli
az spring-cloud app create -n user-service --is-public
az spring-cloud app create -n recommendation-service
az spring-cloud app create -n hystrix-turbine --is-public

az spring-cloud app deploy -n user-service --jar-path user-service/target/user-service.jar
az spring-cloud app deploy -n recommendation-service --jar-path recommendation-service/target/recommendation-service.jar
az spring-cloud app deploy -n hystrix-turbine --jar-path hystrix-turbine/target/hystrix-turbine.jar
```
## <a name="verify-your-apps"></a>Uygulamalarınızı doğrulayın
Tüm uygulamalar çalışır ve keşfedildikten sonra, `user-service` https:// <username> -User-Service.azuremicroservices.io/Personalized/1 yoluna tarayıcınızdan erişin. Kullanıcı Hizmeti erişebiliyorsanız `recommendation-service` , aşağıdaki çıktıyı almalısınız. Web sayfasını, işe yaramazsa birkaç kez yenileyin.
```json
[{"name":"Product1","description":"Description1","detailsLink":"link1"},{"name":"Product2","description":"Description2","detailsLink":"link3"},{"name":"Product3","description":"Description3","detailsLink":"link3"}]
```
## <a name="access-your-hystrix-dashboard-and-metrics-stream"></a>Hyçabax panonuza ve ölçüm akışına erişin
Ortak uç noktaları veya özel test uç noktaları kullanmayı doğrulayın.

### <a name="using-public-endpoints"></a>Ortak uç noktaları kullanma
Tarayıcınızla bir x-Turbin yolu ile erişin `https://<SERVICE-NAME>-hystrix-turbine azuremicroservices.io/hystrix` .  Aşağıdaki şekilde, bu uygulamada çalışan Hyçabax panosu gösterilmektedir.

![Hyçabax panosu](media/spring-cloud-circuit-breaker/hystrix-dashboard.png)

Turbin akış URL 'sini `https://<SERVICE-NAME>-hystrix-turbine.azuremicroservices.io/turbine.stream?cluster=default` metin kutusuna kopyalayın ve **akışı İzle**' ye tıklayın.  Bu, Panoyu görüntüler. Görüntüleyicide hiçbir şey gösterilmezse, `user-service` akışlar oluşturmak için uç noktalara basın.

![Hyçabax Stream ](media/spring-cloud-circuit-breaker/hystrix-stream.png) artık devre kesici panosu ile denemeler yapabilirsiniz.
> [!NOTE] 
> Üretimde, Hyçabax panosu ve ölçüm akışı Internet 'e sunulmamalıdır.

### <a name="using-private-test-endpoints"></a>Özel test uç noktalarını kullanma
Hyçabax ölçüm akışlarına Ayrıca adresinden de erişilebilir `test-endpoint` . Bir arka uç hizmeti olarak, için genel bir uç noktası atamadık `recommendation-service` , ancak ölçümleri şurada test uç noktası ile gösterebiliriz:`https://primary:<KEY>@<SERVICE-NAME>.test.azuremicroservices.io/recommendation-service/default/actuator/hystrix.stream`

![Hyçabax test-Endpoint Stream](media/spring-cloud-circuit-breaker/hystrix-test-endpoint-stream.png)

Bir Web uygulaması olarak Hyçabax panosu üzerinde çalışıyor olmalıdır `test-endpoint` . Düzgün çalışmıyorsa iki neden olabilir: Birincisi, ' `test-endpoint` den temel URL 'yi değiştirdi `/ to /<APP-NAME>/<DEPLOYMENT-NAME>` veya İkincisi, Web uygulaması statik kaynak için mutlak yol kullanıyor. Üzerinde çalışmasını sağlamak için `test-endpoint` ön uç dosyalarında el ile düzenlemeniz gerekebilir <base> .

## <a name="next-steps"></a>Sonraki adımlar
* [Azure CLı üzerinde bir hizmet örneği sağlama](https://docs.microsoft.com/azure/spring-cloud/spring-cloud-quickstart-launch-app-cli#provision-a-service-instance-on-the-azure-cli)
* [Azure yay bulutu 'nda bir Java Spring uygulamasını dağıtıma hazırlama](https://docs.microsoft.com/azure/spring-cloud/spring-cloud-tutorial-prepare-app-deployment)
