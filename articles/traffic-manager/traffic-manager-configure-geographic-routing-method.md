---
title: Öğretici - Azure Trafik Yöneticisi ile coğrafi trafik yönlendirmeyi yapılandırma
description: Bu öğretici, Azure Trafik Yöneticisi'ni kullanarak coğrafi trafik yönlendirme yönteminin nasıl yapılandırılabildiğini açıklar
services: traffic-manager
author: rohinkoul
manager: kumudD
ms.service: traffic-manager
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/22/2017
ms.author: rohink
ms.openlocfilehash: 3eb3f354d51833e55f405ed35679f1a5882c057a
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "76938799"
---
# <a name="tutorial-configure-the-geographic-traffic-routing-method-using-traffic-manager"></a>Öğretici: Trafik Yöneticisi'ni kullanarak coğrafi trafik yönlendirme yöntemini yapılandırın

Coğrafi trafik yönlendirme yöntemi, isteklerin kaynaklandığı coğrafi konuma bağlı olarak trafiği belirli uç noktalara yönlendirmenize olanak tanır. Bu öğretici, bu yönlendirme yöntemiyle trafik yöneticisi profilini nasıl oluşturabileceğinizi ve bitiş noktalarını belirli coğrafyalardan trafik alacak şekilde nasıl yapılandırabileceğinizi gösterir.

## <a name="create-a-traffic-manager-profile"></a>Trafik Yöneticisi Profili Oluşturma

1. Bir tarayıcıdan [Azure portalında](https://portal.azure.com) oturum açın. Henüz bir hesabınız yoksa, [bir aylık ücretsiz denemeye](https://azure.microsoft.com/free/) kaydolabilirsiniz.
2. Kaynak > Oluştur**Trafik** > **Yöneticisi profili** >  **Oluştur'u**tıklatın.**Create**
4. Trafik **Yöneticisi Oluştur profilinde:**
    1. Profilinize bir ad sağlayın. Bu adın trafficmanager.net bölgesi içinde benzersiz olması gerekir. Trafik Yöneticisi profilinize erişmek için DNS `<profilename>.trafficmanager.net`adını kullanırsınız.
    2. **Coğrafi** yönlendirme yöntemini seçin.
    3. Altında bu profili oluşturmak istediğiniz aboneliği seçin.
    4. Varolan bir kaynak grubu kullanın veya bu profili altına yerleştirmek için yeni bir kaynak grubu oluşturun. Yeni bir kaynak grubu oluşturmayı seçerseniz, kaynak grubunun konumunu belirtmek için **Kaynak Grubu konumu** açılır ayinini kullanın. Bu ayar kaynak grubunun konumunu ifade eder ve genel olarak dağıtılan Trafik Yöneticisi profili üzerinde hiçbir etkisi yoktur.
    5. **Oluştur'u**tıklattıktan sonra, Trafik Yöneticisi profiliniz oluşturulur ve genel olarak dağıtılır.

![Traffic Manager profili oluşturma](./media/traffic-manager-geographic-routing-method/create-traffic-manager-profile.png)

## <a name="add-endpoints"></a>Uç nokta ekleme

1. Portalın arama çubuğunda oluşturduğunuz Trafik Yöneticisi profil adını arayın ve gösterildiğinde sonuca tıklayın.
2. Trafik Yöneticisi'nde **Ayarlar** -> **Bitiş Noktaları'na** gidin.
3. **Bitiş Noktası Ekle'yi**göstermek için **Ekle'yi** tıklatın.
3. **Ekle'yi** tıklatın ve görüntülenen **Ekle bitiş noktası,** aşağıdaki gibi tamamlayın:
4. Eklediğiniz bitiş noktasının türüne bağlı olarak **Türü** seçin. Üretimde kullanılan coğrafi yönlendirme profilleri için, birden fazla uç noktası olan bir alt profil içeren iç içe uç nokta türlerini kullanmanızı şiddetle öneririz. Daha fazla ayrıntı [için, coğrafi trafik yönlendirme yöntemleri hakkında SSS](traffic-manager-FAQs.md)bölümüne bakın.
5. Bu uç noktayı tanımak istediğiniz bir **Ad** belirtin.
6. Bu sayfadaki belirli alanlar eklediğiniz bitiş noktasının türüne bağlıdır:
    1. Bir Azure bitiş noktası ekliyorsanız, trafiği yönlendirmek istediğiniz kaynağa göre **Hedef kaynak türünü** ve **Hedef'i** seçin
    2. **Harici** bitiş noktası ekliyorsanız, bitiş noktanız için **Tam nitelikli alan adı (FQDN)** sağlayın.
    3. **İç içe bir uç nokta**ekliyorsanız, kullanmak istediğiniz alt profile karşılık gelen Hedef **kaynağını** seçin ve Minimum alt uç **puan sayısını**belirtin.
7. Coğrafi haritalama bölümünde, trafiğin bu bitiş noktasına gönderilmesini istediğiniz bölgeleri eklemek için açılır noktayı kullanın. En az bir bölge eklemeniz gerekir ve birden çok bölgenin eşlenmiş olması gerekir.
8. Bu profilin altına eklemek istediğiniz tüm uç noktalar için bunu yineleyin

![Traffic Manager uç noktası ekleme](./media/traffic-manager-geographic-routing-method/add-traffic-manager-endpoint.png)

## <a name="use-the-traffic-manager-profile"></a>Trafik Yöneticisi profilini kullanma
1.  Portalın arama çubuğunda, önceki bölümde oluşturduğunuz **Trafik Yöneticisi profil** adını arayın ve görüntülenen sonuçlarda trafik yöneticisi profiline tıklayın.
2. **Genel Bakış**'a tıklayın.
3. **Traffic Manager profili** penceresinde yeni oluşturduğunuz Traffic Manager profilinin DNS adı görüntülenir. Bu, yönlendirme türüne göre belirlendiği şekilde doğru uç noktaya yönlendirilen almak için herhangi bir istemci (örneğin, bir web tarayıcısı kullanarak ona yönlendirilerek) tarafından kullanılabilir.  Coğrafi yönlendirme durumunda, Trafik Yöneticisi gelen isteğin kaynağı IP'ye bakar ve kaynağının kaynağını belirler. Bu bölge bir bitiş noktasına eşlenirse, trafik oraya yönlendirilir. Bu bölge bitiş noktasına eşlenmezse, Trafik Yöneticisi bir NODATA sorgu yanıtı döndürür.

## <a name="next-steps"></a>Sonraki adımlar

- [Coğrafi trafik yönlendirme yöntemi](traffic-manager-routing-methods.md#geographic)hakkında daha fazla bilgi edinin.
- [Trafik Yöneticisi ayarlarını](traffic-manager-testing-settings.md)nasıl test edebilirsiniz öğrenin.
