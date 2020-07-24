---
title: Öğretici-Azure yay bulutunu Azure Yük Dengeleme çözümleriyle tümleştirme
description: Azure yay bulutunu Azure Yük Dengeleme çözümleriyle tümleştirme
author: MikeDodaro
ms.author: brendm
ms.service: spring-cloud
ms.topic: how-to
ms.date: 04/20/2020
ms.custom: devx-track-java
ms.openlocfilehash: 3a7ed148c4bc8d58b2dcbc344656137a403c62eb
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87037551"
---
# <a name="integrate-azure-spring-cloud-with-azure-load-balance-solutions"></a>Azure Spring Cloud’u Azure Load Balance Çözümleriyle tümleştirme

Azure Spring Cloud, Azure 'da mikro hizmetleri destekler.  İş artırma, birden fazla Azure yay bulutu örneğinin yönetimiyle birden çok veri merkezi gerektirebilir.

Azure zaten farklı Yük Dengeleme çözümleri sağlıyor. Azure yay bulutunu Azure Yük Dengeleme çözümleriyle tümleştirmeye yönelik üç seçenek vardır:

1.  Azure yay bulutunu Azure Traffic Manager tümleştirme
2.  Azure yay bulutunu Azure uygulama ağ geçidi ile tümleştirme
3.  Azure yay bulutunu Azure ön kapısına tümleştirme

## <a name="prerequisites"></a>Önkoşullar

* Azure yay bulutu: [Azure yay bulut hizmeti oluşturma](https://docs.microsoft.com/azure/spring-cloud/spring-cloud-quickstart-launch-app-portal)
* Azure Traffic Manager: [Traffic Manager oluşturma](https://docs.microsoft.com/azure/traffic-manager/quickstart-create-traffic-manager-profile/)
* Azure uygulama ağ geçidi: [uygulama ağ geçidi oluşturma](https://docs.microsoft.com/azure/application-gateway/quick-create-portal)
* Azure ön kapı: [ön kapı oluşturma](https://docs.microsoft.com/azure/frontdoor/quickstart-create-front-door)

## <a name="integrate-azure-spring-cloud-with-azure-traffic-manager"></a>Azure yay bulutunu Azure Traffic Manager tümleştirme

Azure Spring Cloud 'ı Traffic Manager ile tümleştirmek için, genel uç noktalarını Traffic Manager uç noktaları olarak ekleyin ve ardından hem Traffic Manager hem de Azure yay bulutu için özel etki alanı yapılandırın.

### <a name="add-endpoint-in-traffic-manager"></a>Uç nokta Ekle Traffic Manager
Traffic Manager 'da uç noktalar ekleme:
1.  *Dış uç nokta*olacak **tür** belirtin.
1.  Her Azure yay bulutu genel uç noktasının tam etki alanı adını (FQDN) girin.
1. **Tamam** düğmesine tıklayın.

    ![Traffic Manager 1 ](media/spring-cloud-load-balancers/traffic-manager-1.png) ![ Traffic Manager 2](media/spring-cloud-load-balancers/traffic-manager-2.png)

### <a name="configure-custom-domain"></a>Özel etki alanını yapılandırma
Yapılandırmayı tamamlaması için:
1.  Etki alanı sağlayıcınızın web sitesinde oturum açın ve özel etki alanından Traffic Manager 'ın Azure varsayılan etki alanı adına bir CNAME kaydı eşlemesi oluşturun.
1.  [Azure Spring Cloud 'a özel etki alanı ekleme](spring-cloud-tutorial-custom-domain.md)yönergelerini izleyin.
1. Traffic Manager 'a, Azure Spring Cloud karşılık gelen App Service 'e özel etki alanı bağlamayı ekleyin ve SSL sertifikasını buraya yükleyin.

    ![Traffic Manager 3](media/spring-cloud-load-balancers/traffic-manager-3.png)

## <a name="integrate-azure-spring-cloud-with-azure-app-gateway"></a>Azure yay bulutunu Azure uygulama ağ geçidi ile tümleştirme

Azure Spring Cloud Service ile tümleştirme için aşağıdaki konfigürasyonları doldurun:

### <a name="configure-backend-pool"></a>Arka uç havuzunu yapılandırma
1. **Hedef türü** olarak *IP adresi* veya *FQDN*belirtin.
1. Azure Spring Cloud genel uç noktalarınızı girin.

    ![Uygulama ağ geçidi 1](media/spring-cloud-load-balancers/app-gateway-1.png)

### <a name="add-custom-probe"></a>Özel araştırma ekle
1. **Durum araştırmalarını** seçin **ve sonra özel** **araştırma** iletişim kutusunu açın. 
1. Anahtar noktası, **arka uç http ayarlarından konak adı Seç** seçeneğinden *Evet* ' i seçin.

    ![Uygulama ağ geçidi 2](media/spring-cloud-load-balancers/app-gateway-2.png)

### <a name="configure-http-setting"></a>Http ayarını Yapılandır
1.  Http **ayarları** ' nı seçin ve ardından **Ekle** ' ye tıklayın.
1.  **Yeni ana bilgisayar adıyla geçersiz kıl:** *Evet*' i seçin.
1.  **Ana bilgisayar adı geçersiz kılma**: **arka uç hedefinden seçim ana bilgisayar adını**seçin.
1.  **Özel araştırma kullan**: *Evet* ' i seçin ve yukarıda oluşturulan özel araştırmayı seçin.

    ![Uygulama ağ geçidi 3](media/spring-cloud-load-balancers/app-gateway-3.png)

## <a name="integrate-azure-spring-cloud-with-azure-front-door"></a>Azure yay bulutunu Azure ön kapısına tümleştirme

Azure Spring Cloud Service ile tümleştirme ve arka uç havuzunu yapılandırma 
1. **Arka uç havuzu ekleyin**.
1. Konak ekleyerek arka uç uç noktasını belirtin.

    ![Ön kapı 1](media/spring-cloud-load-balancers/front-door-1.png)

1.  **Arka uç ana bilgisayar türünü** *özel ana bilgisayar*olarak belirtin.
1.  Azure Spring Cloud genel uç noktalarınızın giriş FQDN 'SI, **arka uç ana bilgisayar adıdır**.
1.  Arka uç **ana bilgisayar** **adıyla**aynı olan varsayılanı kabul edin.

    ![Ön kapı 2](media/spring-cloud-load-balancers/front-door-2.png)

## <a name="next-steps"></a>Sonraki adımlar
* [Traffic Manager oluşturma](https://docs.microsoft.com/azure/traffic-manager/quickstart-create-traffic-manager-profile/)
* [Uygulama ağ geçidi oluşturma](https://docs.microsoft.com/azure/application-gateway/quick-create-portal)
* [Ön kapı oluşturma](https://docs.microsoft.com/azure/frontdoor/quickstart-create-front-door)
