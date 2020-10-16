---
title: 'Hızlı başlangıç: uygulamalar için bir profil oluşturma-Azure portal-Azure Traffic Manager'
description: Bu hızlı başlangıç makalesinde, Azure portal kullanarak yüksek düzeyde kullanılabilir bir Web uygulaması oluşturmak için bir Traffic Manager profilinin nasıl oluşturulacağı açıklanır.
services: traffic-manager
author: duongau
manager: twooley
Customer intent: As an IT admin, I want to direct user traffic to ensure high availability of web applications.
ms.service: traffic-manager
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/15/2020
ms.author: duau
ms.openlocfilehash: 45489d3500a4a744f2aeb34dc21122d180797133
ms.sourcegitcommit: ae6e7057a00d95ed7b828fc8846e3a6281859d40
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2020
ms.locfileid: "92101337"
---
# <a name="quickstart-create-a-traffic-manager-profile-using-the-azure-portal"></a>Hızlı başlangıç: Azure portal kullanarak Traffic Manager profili oluşturma

Bu hızlı başlangıçta, Web uygulamanız için yüksek kullanılabilirlik sunan bir Traffic Manager profilinin nasıl oluşturulacağı açıklanmaktadır.

Bu hızlı başlangıçta, bir Web uygulamasının iki örneği hakkında bilgi edineceksiniz. Bunların her biri farklı bir Azure bölgesinde çalışmaktadır. [Uç nokta önceliğine](traffic-manager-routing-methods.md#priority-traffic-routing-method)göre bir Traffic Manager profili oluşturacaksınız. Profil, Kullanıcı trafiğini Web uygulamasını çalıştıran birincil siteye yönlendirir. Traffic Manager Web uygulamasını sürekli izler. Birincil site kullanılamıyorsa, yedekleme sitesine otomatik yük devretme sağlar.

Azure aboneliğiniz yoksa şimdi [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıçta iki farklı Azure bölgesinde (*Doğu ABD* ve *Batı Avrupa*) dağıtılan bir Web uygulamasının iki örneğine ihtiyacınız olacaktır. Her biri, Traffic Manager için birincil ve yük devretme uç noktaları olarak görev yapar.

1. [Azure portalında](https://portal.azure.com) oturum açın.

1. Ekranın sol üst kısmında **kaynak oluştur**' u seçin. **Web uygulaması** araması yapın ve **Oluştur**' u seçin.

1. **Web uygulaması oluştur**' da, **temel bilgiler** sekmesinde aşağıdaki değerleri yazın veya seçin:

    | Ayar                 | Değer |
    | ---                     | --- |
    | Abonelik            | Aboneliğinizi seçin. |    
    | Kaynak grubu          | **Yeni oluştur** ' u seçin ve metin kutusuna *myResourceGroupTM1* girin.|
    | Name                    | Web uygulamanız için benzersiz bir **ad** girin. Bu örnek, *myWebAppEastUS*kullanır. |
    | Yayımla                 | **Kod**’u seçin. |
    | Çalışma zamanı yığını           | **ASP.net v 4.7**öğesini seçin. |
    | İşletim Sistemi        | **Windows**' u seçin. |
    | Bölge                  | **Doğu ABD**’yi seçin. |
    | Windows Planı            | **Yeni oluştur** ' u seçin ve metin kutusuna *myAppServicePlanEastUS* girin. |
    | Sku ve boyut            | **Standart S1 100 total ACU, 1,75-GB bellek ' i**seçin. |
   
1. **İzleme** sekmesini seçin veya **İleri: izleme**' yi seçin.  **İzleme**altında Application Insights **Application Insights**  >  **Etkinleştir** ' i belirleyin **No**.

1. **Gözden geçir ve Oluştur '** u seçin.

1. Ayarları gözden geçirin ve ardından **Oluştur**' u seçin.  Web uygulaması başarılı bir şekilde dağıttığında, varsayılan bir Web sitesi oluşturur.

1. *MyWebAppWestEurope*adlı Ikinci bir Web uygulaması oluşturmak için 1-6 adımlarını izleyin. **Kaynak grubu** adı, *Batı Avrupa* **bölgesiyle** ve **App Service plan** adı **myAppServicePlanWestEurope** *' dir.* Diğer tüm ayarlar *myWebAppEastUS*ile aynıdır.

## <a name="create-a-traffic-manager-profile"></a>Traffic Manager profili oluşturma

Uç nokta önceliğine göre Kullanıcı trafiğini yönlendiren bir Traffic Manager profili oluşturun.

1. Ekranın sol üst kısmında **kaynak oluştur**' u seçin. Sonra **Traffic Manager profili** araması yapın ve **Oluştur**' u seçin.
1. **Oluşturma Traffic Manager profilinde**, bu ayarları girin veya seçin:

    | Ayar | Değer |
    | --------| ----- |
    | Ad | Traffic Manager profiliniz için benzersiz bir ad girin.|
    | Yönlendirme yöntemi | **Öncelik**' i seçin.|
    | Abonelik | Traffic Manager profilinin uygulanmasını istediğiniz aboneliği seçin. |
    | Kaynak grubu | *MyResourceGroupTM1*öğesini seçin.|
    | Konum |Bu ayar, kaynak grubunun konumunu ifade eder. Genel olarak dağıtılacak Traffic Manager profili üzerinde hiçbir etkisi yoktur.|

1. **Oluştur**’u seçin.

## <a name="add-traffic-manager-endpoints"></a>Traffic Manager uç noktalarını ekleme

Tüm kullanıcı trafiğini yönlendirmek için birincil uç nokta olarak *Doğu ABD* bölgesindeki web sitesini ekleyin. *Batı Avrupa* Web sitesini yük devretme uç noktası olarak ekleyin. Birincil uç nokta kullanılamadığında, trafik otomatik olarak yük devretme uç noktasına yönlendirir.

1. Portalın arama çubuğunda, önceki bölümde oluşturduğunuz Traffic Manager profili adını girin.
1. Arama sonuçlarından profili seçin.
1. **Traffic Manager profili**' nde, **Ayarlar** bölümünde **uç noktalar**' ı seçin ve ardından **Ekle**' yi seçin.

    :::image type="content" source="./media/quickstart-create-traffic-manager-profile/traffic-manager-endpoint-menu.png" alt-text="Traffic Manager profilindeki uç nokta ayarları":::

1. Bu ayarları girin veya seçin:

    | Ayar | Değer |
    | ------- | ------|
    | Tür | **Azure uç noktası**' nı seçin. |
    | Name | *Myprimaryendpoint*girin. |
    | Hedef kaynak türü | **App Service**seçin. |
    | Hedef kaynak | Doğu ABD **bir App Service seçin öğesini**seçin  >  **East US**. |
    | Öncelik | **1**'i seçin. Tüm trafik sağlıklı olduğunda bu uç noktaya gider. |

    :::image type="content" source="./media/quickstart-create-traffic-manager-profile/add-traffic-manager-endpoint.png" alt-text="Traffic Manager profilindeki uç nokta ayarları":::
    
1. **Tamam**’ı seçin.
1. İkinci Azure bölgenize yönelik bir yük devretme uç noktası oluşturmak için bu ayarlarla 3 ve 4 numaralı adımları yineleyin:

    | Ayar | Değer |
    | ------- | ------|
    | Tür | **Azure uç noktası**' nı seçin. |
    | Name | *Myfailoverendpoint*girin. |
    | Hedef kaynak türü | **App Service**seçin. |
    | Hedef kaynak | Batı Avrupa **bir App Service seçin öğesini**seçin  >  **West Europe**. |
    | Öncelik | **2**öğesini seçin. Birincil uç nokta sağlıksız ise tüm trafik bu yük devretme uç noktasına gider. |

1. **Tamam**’ı seçin.

İki uç nokta eklemeyi tamamladığınızda, bunlar **Traffic Manager profilinde**görüntülenirler. İzleme durumlarının şimdi **çevrimiçi** olduğuna dikkat edin.

## <a name="test-traffic-manager-profile"></a>Traffic Manager profilini test etme

Bu bölümde, Traffic Manager profilinizin etki alanı adını kontrol edeceksiniz. Ayrıca birincil uç noktayı kullanılamaz olarak yapılandıracaksınız. Son olarak, Web uygulamasının hala kullanılabilir olduğunu görmeniz gerekir. Bunun nedeni, trafiği yük devretme uç noktasına gönderiyor Traffic Manager.

### <a name="check-the-dns-name"></a>DNS adını denetleyin

1. Portalın arama çubuğunda, önceki bölümde oluşturduğunuz **Traffic Manager profili** adını arayın.
1. Traffic Manager profilini seçin. **Genel bakış** görüntülenir.
1. **Traffic Manager profili** penceresinde yeni oluşturduğunuz Traffic Manager profilinin DNS adı görüntülenir.
  
    :::image type="content" source="./media/quickstart-create-traffic-manager-profile/traffic-manager-dns-name.png" alt-text="Traffic Manager profilindeki uç nokta ayarları":::

### <a name="view-traffic-manager-in-action"></a>Traffic Manager'ın nasıl çalıştığını görün

1. Web tarayıcısında, Web uygulamanızın varsayılan Web sitesini görüntülemek için Traffic Manager profilinizin DNS adını girin.

    > [!NOTE]
    > Bu hızlı başlangıç senaryosunda, tüm istekler birincil uç noktaya yönlendirir. **Öncelik 1**olarak ayarlanır.

    :::image type="content" source="./media/quickstart-create-traffic-manager-profile/traffic-manager-test.png" alt-text="Traffic Manager profilindeki uç nokta ayarları":::

1. Traffic Manager yük devretmeyi görüntülemek için birincil sitenizi devre dışı bırakın:
    1. Traffic Manager profili sayfasında, **genel bakış** bölümünden **Myprimaryendpoint**' i seçin.
    1. *Myprimaryendpoint*içinde, **devre dışı**  >  **Kaydet**' i seçin.
    1. **Myprimaryendpoint**öğesini kapatın. Durumun şimdi **devre dışı bırakıldığını** unutmayın.
1. Web sitesini yeni bir Web tarayıcısı oturumunda görüntülemek için, önceki adımda Traffic Manager profilinizin DNS adını kopyalayın.
1. Web uygulamasının hala kullanılabilir olduğunu doğrulayın.

Birincil uç nokta kullanılamıyor, bu nedenle yük devretme uç noktasına yönlendirilmiştiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

İşiniz bittiğinde, kaynak gruplarını, Web uygulamalarını ve tüm ilgili kaynakları silin. Bunu yapmak için, panonuzdaki her bir öğeyi seçin ve her sayfanın üst kısmında **Sil** ' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta bir Traffic Manager profili oluşturdunuz. Yüksek kullanılabilirliğe sahip Web uygulamaları için Kullanıcı trafiğini yönlendirme olanağı sağlar. Yönlendirme trafiği hakkında daha fazla bilgi edinmek için Traffic Manager öğreticilerine geçin.

> [!div class="nextstepaction"]
> [Traffic Manager öğreticileri](tutorial-traffic-manager-improve-website-response.md)
