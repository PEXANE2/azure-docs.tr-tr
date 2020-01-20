---
title: Kullanım DıŞı Azure DC/OS kümesini Marathon Kullanıcı arabirimi ile yönetme
description: Marathon web kullanıcı arabirimini kullanarak Azure Kapsayıcı Hizmeti küme hizmetine kapsayıcıları dağıtın.
author: iainfoulds
ms.service: container-service
ms.topic: conceptual
ms.date: 04/04/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: b251096915506c3c7a4eebf45b6a03e24779a3d8
ms.sourcegitcommit: 5397b08426da7f05d8aa2e5f465b71b97a75550b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2020
ms.locfileid: "76277798"
---
# <a name="deprecated-manage-an-azure-container-service-dcos-cluster-through-the-marathon-web-ui"></a>Kullanım DıŞı Marathon Web Kullanıcı arabirimi aracılığıyla Azure Container Service DC/OS kümesini yönetme

[!INCLUDE [ACS deprecation](../../../includes/container-service-deprecation.md)]

DC/OS, temel donanımı özetlerken, kümelenmiş iş yüklerini dağıtmak ve ölçeklendirmek için ortam sağlar. DC/OS’nin en üstünde, hesaplama iş yüklerini zamanlamayı ve yürütmeyi yöneten bir çerçeve vardır.

Çerçeveler birçok popüler iş yükü için kullanılabilir, ancak bu belgede Marathon ile kapsayıcıları dağıtmaya nasıl başlacağınız açıklanır. 


## <a name="prerequisites"></a>Ön koşullar
Bu örneklerin üzerinden geçmeden önce, Azure Kapsayıcı Hizmeti’nde yapılandırılan bir DC/OS kümeniz olması gerekir. Bu kümeye uzaktan bağlantınız olması da gerekir. Bu öğeler hakkında daha fazla bilgi için, aşağıdaki makalelere bakın:

* [Azure Container Service kümesi dağıtma](container-service-deployment.md)
* [Azure Container Service kümesine bağlanma](../container-service-connect.md)

> [!NOTE]
> Bu makalede, 80 yerel bağlantı noktası üzerinden DC/OS kümesine tünel yaptığınızı varsayılmaktadır.
>

## <a name="explore-the-dcos-ui"></a>DC/OS kullanıcı arabirimini keşfetme
Secure Shell (SSH) tüneli [oluşturulduğunda](../container-service-connect.md), http:\//localhost/. 'e gidin Bu, DC/OS web kullanıcı arabirimini yükler ve kullanılan kaynaklar, etkin aracılar ve çalışan hizmetler gibi, küme hakkında bilgileri gösterir.

![DC/OS Kullanıcı Arabirimi](./media/container-service-mesos-marathon-ui/dcos2.png)

## <a name="explore-the-marathon-ui"></a>Marathon kullanıcı arabirimini keşfetme
Marathon Kullanıcı arabirimini görmek için bkz. http:\//localhost/Marathon. Bu ekranda, Azure Kapsayıcı Hizmeti DC/OS kümesinde yeni kapsayıcı veya başka bir uygulama başlatabilirsiniz. Kapsayıcıları ve uygulamaları çalıştırma hakkında bilgileri de görebilirsiniz.  

![Marathon kullanıcı arabirimi](./media/container-service-mesos-marathon-ui/dcos3.png)

## <a name="deploy-a-docker-formatted-container"></a>Docker biçimli kapsayıcı dağıtma
Yeni kapsayıcıyı Marathon kullanarak dağıtmak için **Uygulama Oluştur**'a tıklayın ve form sekmelerine aşağıdaki bilgileri girin:

| Alan | Değer |
| --- | --- |
| Kimlik |nginx |
| Hafıza | 32 |
| Resim |nginx |
| Ağ |Bağlantı |
| Ana Bilgisayar Bağlantı Noktası |80 |
| Protokol |TCP |

![Yeni Uygulama Kullanıcı Arabirimi --Genel](./media/container-service-mesos-marathon-ui/dcos4.png)

![Yeni Uygulama Kullanıcı Arabirimi--Docker Kapsayıcısı](./media/container-service-mesos-marathon-ui/dcos5.png)

![Yeni Uygulama Kullanıcı Arabirimi--Bağlantı Noktaları ve Hizmet Bulma](./media/container-service-mesos-marathon-ui/dcos6.png)

Kapsayıcı bağlantı noktasını statik olarak aracıdaki bağlantı noktasıyla eşlemek istiyorsanız, JSON Modu’nu kullanmalısınız. Bunu yapmak için, geçişi kullanarak Yeni Uygulama Sihirbazı'nı **JSON Modu**’na geçirin. Ardından, uygulama tanımının `portMappings` bölümünün altına aşağıdaki ayarı girin. Bu örnek, kapsayıcının 80 numaralı bağlantı noktasını DC/OS aracının 80 numaralı bağlantı noktasına bağlar. Bu değişikliği yaptıktan sonra bu sihirbazı JSON modundan çıkarabilirsiniz.

```none
"hostPort": 80,
```

![Yeni Uygulama Kullanıcı Arabirimi--bağlantı noktası 80 örneği](./media/container-service-mesos-marathon-ui/dcos13.png)

Sistem durumu denetimlerini etkinleştirmek istiyorsanız **Sistem Durumu Denetimleri** sekmesinde bir yol belirleyin.

![Yeni Uygulama UI--durum denetimleri](./media/container-service-mesos-marathon-ui/dcos_healthcheck.png)

DC/OS kümesi bir grup özel ve ortak aracıyla dağıtılır. Kümenin İnternet’ten uygulamalara erişebilmesi için, uygulamaları ortak aracıya dağıtmanız gerekir. Bunu yapmak için, Yeni Uygulama Sihirbazı'nın **İsteğe Bağlı** sekmesini seçin ve **Kabul Edilen Kaynak Rolleri** için **slave_public** girin.

Ardından **Uygulama Oluştur**'a tıklayın.

![Yeni Uygulama Kullanıcı Arabirimi--ortak aracı ayarı](./media/container-service-mesos-marathon-ui/dcos14.png)

Marathon ana sayfasına geri döndüğünüzde, kapsayıcının dağıtımın durumunu görebilirsiniz. Başlangıçta **Dağıtılıyor** durumu görüntülenir. Dağıtım başarıyla tamamlandıktan sonra durum **Çalışıyor** olarak değişir.

![Marathon ana sayfası kullanıcı arabirimi--kapsayıcı dağıtım durumu](./media/container-service-mesos-marathon-ui/dcos7.png)

DC/OS Web Kullanıcı arabirimine (http:\//localhost/) geri döndüğünüzde, DC/OS kümesinde bir görevin (Bu örnekte, Docker biçimli bir kapsayıcı) çalıştığını görürsünüz.

![DC/OS web kullanıcı arabirimi--kümede çalışan görev](./media/container-service-mesos-marathon-ui/dcos8.png)

Görevin üzerinde çalıştığı küme düğümünü görmek için **Düğümler** sekmesine tıklayın.

![DC/OS web kullanıcı arabirimi--görev küme düğümü](./media/container-service-mesos-marathon-ui/dcos9.png)

## <a name="reach-the-container"></a>Kapsayıcıya ulaşın

Bu örnekte, uygulama ortak bir aracı düğümünde çalışmaktadır. Kümenin aracı FQDN 'sine göz atarak, uygulamaya internet üzerinden ulaşabilirsiniz: `http://[DNSPREFIX]agents.[REGION].cloudapp.azure.com`:

* **DNSPREFIX** Kümeyi dağıttığınızda sağladığınız DNS önekidir.
* **REGION** kaynak grubunuzun bulunduğu bölgedir.

    ![Internet 'ten NGINX](./media/container-service-mesos-marathon-ui/nginx.png)


## <a name="next-steps"></a>Sonraki adımlar
* [DC/OS ve Marathon API’si ile çalışma](container-service-mesos-marathon-rest.md)

* Mesos ile Azure Kapsayıcı Hizmeti’ne yakından bakın

    > [!VIDEO https://channel9.msdn.com/Events/Microsoft-Azure/AzureCon-2015/ACON203/player]
    > 
