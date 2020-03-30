---
title: Quickstart:UYGULAMALARıN HA profili oluşturma - Azure portalı - Azure Trafik Yöneticisi
description: Bu hızlı başlangıç makalesinde yüksek oranda kullanılabilir web uygulaması oluşturmak için bir Traffic Manager profili oluşturma adımlarını anlatılmaktadır.
services: traffic-manager
author: rohinkoul
manager: twooley
Customer intent: As an IT admin, I want to direct user traffic to ensure high availability of web applications.
ms.service: traffic-manager
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/28/2018
ms.author: rohink
ms.openlocfilehash: 559ed0a134bb6db78d1e89634138b4025e04152b
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "76934778"
---
# <a name="quickstart-create-a-traffic-manager-profile-using-the-azure-portal"></a>Hızlı başlangıç: Azure portalını kullanarak Trafik Yöneticisi profili oluşturma

Bu hızlı başlatma, web uygulamanız için yüksek kullanılabilirlik sağlayan bir Trafik Yöneticisi profilinin nasıl oluşturulacağını açıklar.

Bu hızlı başlangıçta, bir web uygulamasının iki örneğini okursunuz. Her biri farklı bir Azure bölgesinde çalışıyor. [Bitiş noktası önceliğine](traffic-manager-routing-methods.md#priority-traffic-routing-method)dayalı bir Trafik Yöneticisi profili oluşturursunuz. Profil, kullanıcı trafiğini web uygulamasını çalıştıran birincil siteye yönlendirir. Trafik Yöneticisi web uygulamasını sürekli olarak izler. Birincil site kullanılamıyorsa, yedekleme sitesine otomatik olarak başarısız olur.

Azure aboneliğiniz yoksa şimdi [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[Azure portalında](https://portal.azure.com)oturum açın.

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıç için, iki farklı Azure bölgesinde *(Doğu ABD* ve *Batı Avrupa)* dağıtılan bir web uygulamasının iki örneğine ihtiyacınız vardır. Her biri Trafik Yöneticisi için birincil ve başarısız uç nokta olarak hizmet verecektir.

1. Ekranın sol üst tarafında > kaynak**Web Web** > **Uygulaması** **Oluştur'u**seçin.

1. **Web Uygulaması Oluştur'da** **Temel Ler** sekmesinde aşağıdaki değerleri yazın veya seçin:

   - **Abonelik** > **Kaynak Grubu**: Yeni **Oluştur'u** seçin ve ardından **myResourceGroupTM1**yazın.
   - **Örnek Ayrıntılar** > **Adı**: *myWebAppEastUS*yazın.
   - **Örnek Ayrıntılar** > **Yayımla**: **Kod**Seç .
   - **Örnek Ayrıntılar** > **Çalışma Zamanı yığını**: **V4.7ASP.NET** seçin
   - **Örnek Ayrıntılar** > **İşletim Sistemi**: **Windows**seçin.
   - **Örnek Ayrıntılar** > **Bölge**: **Doğu ABD**seçin.
   - **Uygulama Hizmet Planı** > Windows**Planı (Doğu ABD)**: **Yeni Oluştur'u** seçin ve **ardından myAppServicePlanEastUS** yazın
   - **Uygulama Hizmet Planı** > **Sku ve boyutu**: Standart **S1**seçin.
   
3. **İzleme** sekmesini seçin veya **Sonraki:İzleme'yi**seçin.  **İzleme**altında, **Uygulama Öngörülerini** > Ayarla**Uygulama Öngörülerini** **No'ya**Etkinleştirin.

4. **Gözden Geçir'i** seçin ve oluşturun

5. Ayarları gözden geçirin ve ardından **Oluştur'u**tıklatın.  Web Uygulaması başarıyla dağıtıldığında, varsayılan bir web sitesi oluşturur.

6. *myWebAppWestEurope*adlı ikinci bir Web Uygulaması oluşturmak için adımları izleyin , *myResourceGroupTM2*bir **Kaynak Grubu** adı ile , *Batı Avrupa* **Bölgesi** , **myAppServicePlanWestEurope**bir **App Service Plan** adı , ve tüm diğer ayarları *myWebAppEastUS*aynı .

## <a name="create-a-traffic-manager-profile"></a>Traffic Manager profili oluşturma

Uç nokta önceliğine göre kullanıcı trafiğini yönlendiren bir Trafik Yöneticisi profili oluşturun.

1. Ekranın sol üst tarafında, **kaynak** > **Oluşturma** > **Trafik Yöneticisi profilini**seçin.
2. Trafik **Yöneticisi Oluştur profilinde,** bu ayarları girin veya seçin:

    | Ayar | Değer |
    | --------| ----- |
    | Adı | Trafik Yöneticisi profiliniz için benzersiz bir ad girin.|
    | Yönlendirme yöntemi | **Öncelik'i**seçin.|
    | Abonelik | Trafik yöneticisi profilinin uygulanmasını istediğiniz aboneliği seçin. |
    | Kaynak grubu | *MyResourceGroupTM1'i*seçin.|
    | Konum |Bu ayar, kaynak grubunun konumunu ifade eder. Genel olarak dağıtılacak Trafik Yöneticisi profili üzerinde hiçbir etkisi yoktur.|

3. **Oluştur'u**seçin.

## <a name="add-traffic-manager-endpoints"></a>Traffic Manager uç noktalarını ekleme

Tüm kullanıcı trafiğini yönlendirmek için birincil uç nokta olarak *Doğu ABD* bölgesindeki web sitesini ekleyin. *Batı Avrupa'da* bir failover bitiş noktası olarak web sitesi ekleyin. Birincil bitiş noktası kullanılamıyorsa, trafik otomatik olarak başarısız bitiş noktasına yönlendirir.

1. Portalın arama çubuğuna, önceki bölümde oluşturduğunuz Trafik Yöneticisi profil adını girin.
2. Arama sonuçlarından profili seçin.
3. **Trafik Yöneticisi profilinde**Ayarlar **bölümünde** **Uç Noktaları'nı**seçin ve sonra **Ekle'yi**seçin.
4. Şu ayarları girin veya seçin:

    | Ayar | Değer |
    | ------- | ------|
    | Tür | **Azure bitiş noktasını**seçin. |
    | Adı | *BenimPrimaryEndpoint*girin. |
    | Hedef kaynak türü | **Uygulama Hizmetini**seçin. |
    | Hedef kaynak | Doğu **ABD'de bir uygulama hizmeti** > **East US**seçin'i seçin. |
    | Öncelik | **1**'i seçin. Tüm trafik sağlıklı olduğunda bu bitiş noktasına gider. |

    ![Trafik Yöneticisi profilinize bir bitiş noktası eklediğiniz yerin ekran görüntüsü.](./media/quickstart-create-traffic-manager-profile/add-traffic-manager-endpoint.png)

5. **Tamam'ı**seçin.
6. İkinci Azure bölgeniz için bir başarısız uç nokta oluşturmak için şu ayarları içeren 3 ve 4 adımlarını yineleyin:

    | Ayar | Değer |
    | ------- | ------|
    | Tür | **Azure bitiş noktasını**seçin. |
    | Adı | *Benim FailoverEndpoint*girin. |
    | Hedef kaynak türü | **Uygulama Hizmetini**seçin. |
    | Hedef kaynak | Batı **Avrupa'da bir uygulama hizmeti** > **West Europe**seçin' i seçin. |
    | Öncelik | **2**seçin. Birincil bitiş noktası sağlıksızsa, tüm trafik bu başarısız uç noktasına gider. |

7. **Tamam'ı**seçin.

İki uç noktayı eklemeyi bitirdiğinizde, bunlar **Trafik Yöneticisi profilinde**görüntülenir. İzleme durumlarının artık **Çevrimiçi** olduğuna dikkat edin.

## <a name="test-traffic-manager-profile"></a>Traffic Manager profilini test etme

Bu bölümde, Trafik Yöneticisi profilinizin alan adını kontrol eedeceksiniz. Ayrıca, birincil bitiş noktasını kullanılamaz olarak da yapılandıracaksınız. Son olarak, web uygulamasının hala kullanılabilir olduğunu görebilirsiniz. Çünkü Trafik Yöneticisi trafiği başarısız bitiş noktasına gönderir.

### <a name="check-the-dns-name"></a>DNS adını kontrol edin

1. Portalın arama çubuğunda, önceki bölümde oluşturduğunuz **Trafik Yöneticisi profil** adını arayın.
2. Trafik yöneticisi profilini seçin. **Genel Bakış** görüntülenir.
3. **Traffic Manager profili** penceresinde yeni oluşturduğunuz Traffic Manager profilinin DNS adı görüntülenir.
  
   ![Trafik Yöneticisi DNS adınızın konumunun ekran görüntüsü](./media/quickstart-create-traffic-manager-profile/traffic-manager-dns-name.png)

### <a name="view-traffic-manager-in-action"></a>Traffic Manager'ın nasıl çalıştığını görün

1. Bir web tarayıcısında, Web Uygulamanızın varsayılan web sitesini görüntülemek için Trafik Yöneticisi profilinizin DNS adını girin.

    > [!NOTE]
    > Bu hızlı başlangıç senaryosunda, tüm istekler birincil bitiş noktasına yönlendirin. **Öncelik 1**olarak ayarlanır.

    ![Trafik Yöneticisi profilinin kullanılabilirliğini onaylamak için web sayfasının ekran görüntüsü](./media/quickstart-create-traffic-manager-profile/traffic-manager-test.png)

2. Trafik Yöneticisi'nin başarısız olduğunu görüntülemek için birincil sitenizi devre dışı edin:
    1. Trafik Yöneticisi Profili sayfasında, **Genel Bakış** **bölümünden, myPrimaryEndpoint'i**seçin.
    2. *BenimPrimaryEndpoint'te,* Devre Dışı > **Kaydet'i**seçin. **Disabled**
    3. **myPrimaryEndpoint'i**kapatın. Durumun şimdi **Devre Dışı bırakıldığına** dikkat edin.
3. Web sitesini yeni bir web tarayıcısı oturumunda görüntülemek için trafik yöneticisi profilinizin DNS adını önceki adımdan kopyalayın.
4. Web uygulamasının hala kullanılabilir olduğunu doğrulayın.

Birincil bitiş noktası kullanılamadığından, başarısız uç noktaya yönlendirildiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

İşi bittiğinde, kaynak gruplarını, web uygulamalarını ve ilgili tüm kaynakları silin. Bunu yapmak için, panonuzdaki her bir öğeyi seçin ve her sayfanın **üst** kısmındasil'i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir Trafik Yöneticisi profili oluşturdunuz. Yüksek kullanılabilirlik web uygulamaları için kullanıcı trafiğini yönlendirmenize olanak tanır. Trafiği yönlendirme hakkında daha fazla bilgi edinmek için Trafik Yöneticisi öğreticilerine devam edin.

> [!div class="nextstepaction"]
> [Traffic Manager öğreticileri](tutorial-traffic-manager-improve-website-response.md)
