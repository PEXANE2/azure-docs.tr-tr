---
title: Quickstart - Docker konteynerini konteyner örneğine dağıt - Portal
description: Bu hızlı başlatmada, yalıtılmış bir Azure kapsayıcı örneğinde çalışan kapsayıcı web uygulamasını hızla dağıtmak için Azure portalını kullanırsınız
ms.topic: quickstart
ms.date: 03/09/2020
ms.custom: seodec18, mvc
ms.openlocfilehash: 7a872e955db46b76d3b12f8ffc38d4a8e497ea63
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79087971"
---
# <a name="quickstart-deploy-a-container-instance-in-azure-using-the-azure-portal"></a>Hızlı başlatma: Azure portalını kullanarak Azure'da bir kapsayıcı örneğini dağıtma

Azure'da sunucusuz Docker kapsayıcılarını basitlik ve hızla çalıştırmak için Azure Kapsayıcı Örnekleri'ni kullanın. Azure Kubernetes Hizmeti gibi tam bir kapsayıcı düzenleme platformuna ihtiyacınız olmadığında uygulamayı isteğe bağlı bir kapsayıcı örneğine dağıtın.

Bu hızlı başlangıçta, yalıtılmış bir Docker kapsayıcısı dağıtmak ve uygulamasını tam nitelikli bir etki alanı adı (FQDN) ile kullanılabilir hale getirmek için Azure portalını kullanırsınız. Birkaç ayarı yapılandırdıktan ve kapsayıcıyı dağıttıktan sonra çalışan uygulamaya göz atabilirsiniz:

![Azure Container Instances hizmetine dağıtılmış uygulamanın tarayıcıdaki görüntüsü][aci-portal-07]

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

https://portal.azure.com adresinden Azure portalında oturum açın.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz][azure-free-account] bir hesap oluşturun.

## <a name="create-a-container-instance"></a>Kapsayıcı örneği oluşturma

Kaynak > **Kapsayıcı** > **Örnekleri** **Oluştur'u**seçin.

![Azure portalında yeni bir kapsayıcı örneği oluşturmaya başlama][aci-portal-01]

Temel **Bilgiler** sayfasında, **Kaynak grubuna,** Kapsayıcı **adına**ve **Kapsayıcı resim** metin kutularına aşağıdaki değerleri girin. Diğer değerleri varsayılan değerlerinde bırakın ve **Tamam**’ı seçin.

* Kaynak grubu: **Yeni oluşturma** > `myresourcegroup`
* Kapsayıcı adı: `mycontainer`
* Görüntü kaynağı: **Quickstart görüntüleri**
* Konteyner görüntüsü: `mcr.microsoft.com/azuredocs/aci-helloworld` (Linux)

![Azure portalında yeni bir kapsayıcı örneği için temel ayarları yapılandırma][aci-portal-03]

Bu hızlı başlangıç için, genel Microsoft `aci-helloworld` görüntüsünü dağıtmak için varsayılan ayarları kullanırsınız. Bu örnek Linux görüntüsü, Node.js ile yazılmış statik bir HTML sayfasına hizmet veren küçük bir web uygulamasını paketler. Azure Konteyner Kayıt Defteri, Docker Hub veya diğer kayıt defterlerinde depolanan kendi kapsayıcı resimlerinizi de getirebilirsiniz.

**Ağ** sayfasında, kapsayıcınız için bir **DNS ad etiketi** belirtin. Ad, kapsayıcı örneğini oluşturduğunuz Azure bölgesinde benzersiz olmalıdır. Kapsayıcınız `<dns-name-label>.<region>.azurecontainer.io` konumunda genel kullanıma sunulacaktır. "DNS ad etiketi kullanılamıyor" hata iletisiyle karşılaşırsanız farklı bir DNS ad etiketi deneyin.

![Azure portalında yeni bir kapsayıcı örneği için ağ ayarlarını yapılandırma][aci-portal-04]

Diğer ayarları varsayılanlarına bırakın, ardından **Gözden Geçir + oluştur'u**seçin.

Doğrulama tamamlandığında, kapsayıcı ayarlarının bir özeti gösterilir. Kapsayıcı dağıtım isteğinizi göndermek için **Oluştur'u** seçin.

![Azure portalında yeni bir kapsayıcı örneği için ayarların özeti][aci-portal-05]

Dağıtım başladığında, dağıtımın devam ettiğini gösteren bir bildirim görüntülenir. Kapsayıcı grubu dağıtıldığında yeni bir bildirim daha görüntülenir.

**Kaynak Grupları** > **myresourcegroup** > **mycontainer'a**yönlendirerek kapsayıcı grubu için genel bakışı açın. Kapsayıcı örneğinin **Durum**’u ile birlikte **FQDN**’sini (tam etki alanı adı) not edin.

![Azure portalında kapsayıcı grubuna genel bakış][aci-portal-06]

*Çalışıyor***Durumunda** iken tarayıcınızda kapsayıcının FQDN’sine gidin.

![Azure Container Instances kullanılarak dağıtılmış uygulama tarayıcıda görüntüleniyor][aci-portal-07]

Tebrikler! Yalnızca birkaç ayarı yapılandırarak, Azure Container Instances’ta genel olarak erişilebilir bir uygulama dağıttınız.

## <a name="view-container-logs"></a>Kapsayıcı günlüklerini görüntüleme

Kapsayıcı örneğinin günlüklerini görüntülemek, kapsayıcınızın veya çalıştırdığı uygulamanın sorunlarını gidermede yardımcı olur.

Kapsayıcının günlüklerini görüntülemek için **Ayarlar** altında **Kapsayıcılar**’ı ve ardından **Günlükler**’i seçin. Uygulamayı tarayıcınızda görüntülediğinizde HTTP GET isteğinin oluşturulduğunu görmeniz gerekir.

![Azure portalında kapsayıcı günlükleri][aci-portal-11]

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kapsayıcıyla işiniz bittiğinde, *mycontainer* kapsayıcı örneğine ait **Genel bakış**’ı ve ardından **Sil**’i seçin.

![Azure portalında kapsayıcı örneğini silme][aci-portal-09]

Onay iletişim kutusu görüntülendiğinde **Evet**’i seçin.

![Azure portalında kapsayıcı örneği onayını silme][aci-portal-10]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, herkese açık bir Microsoft görüntüsünden bir Azure kapsayıcı örneği oluşturdunuz. Kapsayıcı görüntüsünü oluşturup özel bir Azure kapsayıcı kayıt defterinden dağıtmak istiyorsanız Azure Container Instances öğreticisine geçin.

> [!div class="nextstepaction"]
> [Azure Container Instances öğreticisi](./container-instances-tutorial-prepare-app.md)

<!-- IMAGES -->
[aci-portal-01]: ./media/container-instances-quickstart-portal/qs-portal-01.png
[aci-portal-03]: ./media/container-instances-quickstart-portal/qs-portal-03.png
[aci-portal-04]: ./media/container-instances-quickstart-portal/qs-portal-04.png
[aci-portal-05]: ./media/container-instances-quickstart-portal/qs-portal-05.png
[aci-portal-06]: ./media/container-instances-quickstart-portal/qs-portal-06.png
[aci-portal-07]: ./media/container-instances-quickstart-portal/qs-portal-07.png
[aci-portal-08]: ./media/container-instances-quickstart-portal/qs-portal-08.png
[aci-portal-09]: ./media/container-instances-quickstart-portal/qs-portal-09.png
[aci-portal-10]: ./media/container-instances-quickstart-portal/qs-portal-10.png
[aci-portal-11]: ./media/container-instances-quickstart-portal/qs-portal-11.png

<!-- LINKS - External -->
[azure-free-account]: https://azure.microsoft.com/free/