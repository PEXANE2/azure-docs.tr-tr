---
title: Azure portal, Datacenter ortamını Azure Stack Edge dağıtımı için hazırlama öğreticisi | Microsoft Docs
description: Azure Stack Edge 'i dağıtmaya yönelik ilk öğretici Azure portal hazırlamayı içerir.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 06/11/2020
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to prepare the portal to deploy Azure Stack Edge so I can use it to transfer data to Azure.
ms.openlocfilehash: 1e95f635cfb354487298b340f8f25b15c3ca146a
ms.sourcegitcommit: 656c0c38cf550327a9ee10cc936029378bc7b5a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89088249"
---
# <a name="tutorial-prepare-to-deploy-azure-stack-edge"></a>Öğretici: Azure Stack Edge dağıtmaya hazırlanma  

Bu, Azure Stack kenarını tamamen dağıtmak için gereken dağıtım öğreticilerinde ilk öğreticidir. Bu öğreticide, Azure portal Azure Stack Edge kaynağını dağıtmaya yönelik nasıl hazırlanılacağı açıklanmaktadır.

Kurulum ve yapılandırma işlemini tamamlamak için yönetici ayrıcalıkları gerekir. Portal hazırlığı 10 dakikadan kısa sürer.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Yeni kaynak oluşturma
> * Etkinleştirme anahtarı alma

### <a name="get-started"></a>Kullanmaya başlayın

Azure Stack Edge dağıtımı için öncelikle ortamınızı hazırlamanız gerekir. Ortam çalışmaya başladıktan sonra, cihazı tam olarak dağıtmak için gerekli adımları ve gerekirse isteğe bağlı adımları ve yordamları izleyin. Adım adım dağıtım yönergeleri, bu gerekli ve isteğe bağlı adımların her birini ne zaman gerçekleştirmeniz gerektiğini gösterir.

| Adım | Açıklama |
| --- | --- |
| **Hazırlık** |Bu adımların, yaklaşan dağıtım için hazırlanmasından tamamlanması gerekir. |
| **[Dağıtım yapılandırma denetim listesi](#deployment-configuration-checklist)** |Dağıtımdan önce ve dağıtım sırasında bilgi toplamak ve bilgileri kaydetmek için bu denetim listesini kullanın. |
| **[Dağıtım önkoşulları](#prerequisites)** |Bunlar, ortamın dağıtım için hazır olduğunu doğrular. |
|  | |
|**Dağıtım öğreticileri** |Azure Stack Edge cihazınızı üretime dağıtmak için bu öğreticiler gereklidir. |
|**[1. Azure Stack Edge için Azure portal hazırlayın](azure-stack-edge-gpu-deploy-prep.md)** |Azure Stack Edge kaynağını bir Azure Stack Box Edge fiziksel cihazı yüklemeden önce oluşturun ve yapılandırın. |
|**[2. Azure Stack Edge 'i yükleyip](azure-stack-edge-gpu-deploy-install.md)**|Azure Stack Edge fiziksel cihazının paketini açın, raf ve kablo.  |
|**[3. Azure Stack Edge 'e bağlanma](azure-stack-edge-gpu-deploy-connect.md)** |Cihaz yüklendikten sonra, cihaz yerel Web Kullanıcı arabirimine bağlanın.  |
|**[4. Azure Stack Edge için ağ ayarlarını yapılandırma](azure-stack-edge-gpu-deploy-configure-network-compute-web-proxy.md)** |Cihazınızın işlem ağı ve Web proxy ayarları dahil olmak üzere ağı yapılandırın.   |
|**[5. Azure Stack Edge için cihaz ayarlarını yapılandırma](azure-stack-edge-gpu-deploy-set-up-device-update-time.md)** |Bir cihaz adı ve DNS etki alanı atayın, güncelleştirme sunucusunu ve cihaz zamanını yapılandırın. |
|**[6. Azure Stack Edge için güvenlik ayarlarını yapılandırma](azure-stack-edge-gpu-deploy-configure-certificates.md)** |Cihazınız için sertifikaları yapılandırın. Cihaz tarafından oluşturulan sertifikaları kullanın veya kendi sertifikalarınızı getirin.   |
|**[7. Azure Stack kenarı etkinleştirin](azure-stack-edge-gpu-deploy-activate.md)** |Cihazı etkinleştirmek için hizmetten etkinleştirme anahtarını kullanın. Cihaz, SMB veya NFS paylaşımlarını ayarlamaya veya REST aracılığıyla bağlamaya hazırlanıyor. |
|**[8A. Kenar paylaşımları ile veri aktarma](azure-stack-edge-j-series-deploy-add-shares.md)** |Paylaşımları ekleyin ve SMB veya NFS üzerinden paylaşımlara bağlanın. |
|**[8B. Edge depolama hesaplarıyla veri aktarma](azure-stack-edge-j-series-deploy-add-storage-accounts.md)** |Depolama hesapları ekleyin ve REST API 'Ler aracılığıyla blob depolamaya bağlanın. |


Artık Azure Stack Edge cihazınız için yazılım yapılandırmasıyla ilgili bilgi toplamaya başlayabilirsiniz.

## <a name="deployment-configuration-checklist"></a>Dağıtım yapılandırma denetim listesi

Cihazınızı dağıtmadan önce, Azure Stack Edge cihazınızda yazılımı yapılandırmak için bilgi toplamanız gerekir. Bu bilgilerin bir bölümünü önceden hazırlamak, cihazı ortamınızda dağıtma işlemini kolaylaştırmaya yardımcı olur. Cihazınızı dağıtırken yapılandırma ayrıntılarını göz önünde bırakmak için [Azure Stack Edge dağıtım yapılandırması denetim listesini](azure-stack-edge-gpu-deploy-checklist.md) kullanın.


## <a name="prerequisites"></a>Önkoşullar

Azure Stack Edge kaynağınız, Azure Stack Edge cihazınız ve veri merkezi ağı için yapılandırma önkoşulları aşağıda verilmiştir.

### <a name="for-the-azure-stack-edge-resource"></a>Azure Stack Edge kaynağı için

Başlamadan önce aşağıdakilerden emin olun:

- Microsoft Azure aboneliğiniz Azure Stack Edge kaynağı için etkinleştirilir. [Microsoft kurumsal anlaşma (EA)](https://azure.microsoft.com/overview/sales-number/), [bulut çözümü sağlayıcısı (CSP)](https://docs.microsoft.com/partner-center/azure-plan-lp)veya [Microsoft Azure sponsorluğu](https://azure.microsoft.com/offers/ms-azr-0036p/)gibi desteklenen bir abonelik kullandığınızdan emin olun. Kullandıkça Öde abonelikleri desteklenmez.
- Azure Stack Edge/Data Box Gateway, IoT Hub ve Azure depolama kaynakları için kaynak grubu düzeyinde sahip veya katkıda bulunan erişiminiz var.

    - Azure Stack Edge/Data Box Gateway kaynağı oluşturmak için, kaynak grubu düzeyinde katkıda bulunan (veya üzeri) izinlere sahip olmanız gerekir. Ayrıca, sağlayıcının kayıtlı olduğundan emin olmanız gerekir `Microsoft.DataBoxEdge` . Kaydetme hakkında daha fazla bilgi için, [kayıt kaynak sağlayıcısı](azure-stack-edge-manage-access-power-connectivity-mode.md#register-resource-providers)' na gidin.
    - Herhangi bir IoT Hub kaynağı oluşturmak için Microsoft. Devices sağlayıcısının kayıtlı olduğundan emin olun. Kaydetme hakkında daha fazla bilgi için, [kayıt kaynak sağlayıcısı](azure-stack-edge-manage-access-power-connectivity-mode.md#register-resource-providers)' na gidin.
    - Bir depolama hesabı kaynağı oluşturmak için, kaynak grubu düzeyinde katkıda bulunan veya daha yüksek erişim kapsamına ihtiyacınız vardır. Azure depolama, varsayılan olarak kayıtlı bir kaynak sağlayıcısıdır.
- Azure Active Directory Graph API için yönetici veya Kullanıcı erişiminiz var. Daha fazla bilgi için bkz. [Azure Active Directory Graph API](https://docs.microsoft.com/previous-versions/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes#default-access-for-administrators-users-and-guest-users-).
- Erişim kimlik bilgilerine sahip bir Microsoft Azure Storage hesabınız var.

### <a name="for-the-azure-stack-edge-device"></a>Azure Stack Edge cihazı için

Fiziksel cihazı dağıtmadan önce şunlardan emin olun:

- Sevkiyat paketine dahil edilen güvenlik bilgilerini gözden geçirdiniz.
- Veri merkezinizde bulunan ve cihazı bağlamak için standart 19 "rafındaki bir 1U yuvasına sahipsiniz.
- Cihazın güvenle geri kalanında düz, kararlı ve düzeyi bir iş yüzeyine erişebilirsiniz.
- Cihazı ayarlamayı planladığınız sitenin, bağımsız bir kaynaktan veya kesintisiz güç kaynağı (UPS) olan bir raf güç dağıtım biriminden (PDU) standart AC gücü vardır.
- Fiziksel cihaza erişebiliyor olmanız gerekir.


### <a name="for-the-datacenter-network"></a>Veri merkezi ağı için

Başlamadan önce aşağıdakilerden emin olun:

- Veri merkezinizdeki ağ, Azure Stack Edge cihazınız için ağ gereksinimlerine göre yapılandırılır. Daha fazla bilgi için bkz. [Azure Stack Edge sistem gereksinimleri](azure-stack-edge-system-requirements.md).

- Azure Stack Kenarlarınızın normal işletim koşullarında şunları yapabilirsiniz:

    - Cihazın güncel kalmasını sağlamak için en az 10 Mbps indirme bant genişliği.
    - Dosyaları aktarmak için en az 20 Mbps adanmış karşıya yükleme ve indirme bant genişliği.

## <a name="create-a-new-resource"></a>Yeni kaynak oluşturma

Fiziksel cihazınızı yönetmek için mevcut bir Azure Stack Edge kaynağınız varsa, bu adımı atlayın ve [etkinleştirme anahtarını almak](#get-the-activation-key)için gidin.

Azure Stack Edge kaynağı oluşturmak için Azure portal aşağıdaki adımları uygulayın.

1. Bu URL 'de Azure portal oturum açmak için Microsoft Azure kimlik bilgilerinizi kullanın: [https://portal.azure.com](https://portal.azure.com) .

2. Sol bölmede **+ kaynak oluştur**' u seçin. **Azure Stack Edge/Data Box Gateway**için arama yapın ve seçin. **Oluştur**’u seçin.

3. Azure Stack Edge cihazı için kullanmak istediğiniz aboneliği seçin. Bu fiziksel cihazı göndermek istediğiniz ülkeyi seçin. **Cihazları göster**' i seçin.

    ![Kaynak oluşturma 1](media/azure-stack-edge-gpu-deploy-prep/create-resource-1.png)

4. Cihaz türü seçin. **Azure Stack Edge Commercial**altında **GPU ile Azure Stack Edge** ' i seçin ve **Kaydol**' u seçin. 

    ![Kaynak oluştur 2](media/azure-stack-edge-gpu-deploy-prep/create-resource-2.png)

5. Kısa bir form görüntülenir. Formu doldurun ve **Gönder**' i seçin. Microsoft, aboneliğinizi etkinleştirir.

    ![Kaynak oluşturma 3](media/azure-stack-edge-gpu-deploy-prep/create-resource-3.png)

5. Abonelik etkinleştirildikten sonra kaynak oluşturmaya devam edebilmelisiniz. **Cihaz türü seç** dikey penceresinde **Seç**' i seçin. 

    ![Kaynak oluşturma 4](media/azure-stack-edge-gpu-deploy-prep/create-resource-4.png)


6. **Temel bilgiler** sekmesinde, aşağıdaki **proje ayrıntılarını**girin veya seçin.
    
    |Ayar  |Değer  |
    |---------|---------|
    |Abonelik    |Bu, önceki seçime göre otomatik olarak doldurulur. Abonelik fatura hesabınıza bağlıdır. |
    |Kaynak grubu  |Mevcut grubu seçin veya yeni bir grup oluşturun.<br>[Azure Kaynak Grupları](../azure-resource-manager/resource-group-overview.md) hakkında daha fazla bilgi edinin.     |

4. Aşağıdaki **örnek ayrıntılarını**girin veya seçin.

    |Ayar  |Değer  |
    |---------|---------|
    |Ad   | Kaynağı tanımlamak için kolay bir ad.<br>Ad 2 ile 50 karakter arasında olmalı, harf, rakam ve kısa çizgilerden oluşmalıdır.<br> Ad bir harf veya rakamla başlar ve biter.        |
    |Bölge     |Azure Stack Edge kaynağının kullanılabildiği tüm bölgelerin listesi için bkz. [bölgeye göre kullanılabilir Azure ürünleri](https://azure.microsoft.com/global-infrastructure/services/?products=databox&regions=all). Azure Kamu kullanıyorsanız, tüm kamu bölgeleri [Azure bölgelerinde](https://azure.microsoft.com/global-infrastructure/regions/)gösterildiği gibi kullanılabilir.<br> Cihazınızı dağıtmak istediğiniz coğrafi bölgeye yakın bir konum seçin.|

    ![Kaynak oluştur 5](media/azure-stack-edge-gpu-deploy-prep/create-resource-5.png)


5. **İleri ' yi seçin: sevkiyat adresi**.

    - Zaten bir cihazınız varsa, **Azure Stack Edge cihazım**için Birleşik giriş kutusunu seçin.

        ![Kaynak oluştur 6](media/azure-stack-edge-gpu-deploy-prep/create-resource-6.png)

    - Bu, sipariş ettiğiniz yeni bir cihaz ise, kişi adını, şirketi, cihazı sevk etmek için adresi ve iletişim bilgilerini girin.

        ![Kaynak oluşturma 7](media/azure-stack-edge-gpu-deploy-prep/create-resource-7.png)

6. **Sonraki: Gözden geçirme ve oluşturma**’yı seçin.

7. **Gözden geçir + oluştur** sekmesinde, **fiyatlandırma ayrıntılarını**, **kullanım koşulları**ve kaynağınızın ayrıntılarını gözden geçirin. **Gizlilik koşullarını Incelediğim**Birleşik giriş kutusunu seçin.

    ![Kaynak oluşturma 8](media/azure-stack-edge-gpu-deploy-prep/create-resource-8.png)

8. **Oluştur**’u seçin.

Kaynağın oluşturulması birkaç dakika sürer. Kaynak başarıyla oluşturulup dağıtıldıktan sonra bilgilendirirsiniz. **Kaynağa git**’i seçin.

![Azure Stack Edge kaynağına git](media/azure-stack-edge-deploy-prep/data-box-edge-resource3.png)

Sipariş yerleştirildikten sonra, Microsoft siparişi inceler ve gönderim ayrıntıları ile size (e-posta aracılığıyla) ulaşır.

![Azure Stack Edge sırasının incelenmesi için bildirim](media/azure-stack-edge-deploy-prep/data-box-edge-resource4.png)

## <a name="get-the-activation-key"></a>Etkinleştirme anahtarı alma

Azure Stack Edge kaynağı çalışır duruma geçtikten sonra etkinleştirme anahtarını almanız gerekir. Bu anahtar Azure Stack Edge cihazınızı etkinleştirmek ve kaynakla bağlamak için kullanılır. Bu anahtarı şimdi, Azure portalındayken alabilirsiniz.

1. Oluşturduğunuz kaynağı seçin. **Genel bakış** ' ı ve ardından **Cihaz kurulumu**' nu seçin.

    ![Cihaz kurulumunu seçin](media/azure-stack-edge-deploy-prep/data-box-edge-select-devicesetup.png)

2. **Etkinleştir** kutucuğunda bir etkinleştirme anahtarı oluşturmak Için **anahtar oluştur** ' u seçin. Anahtarı kopyalamak için Kopyala simgesini seçin ve daha sonra kullanmak üzere kaydedin.

    ![Etkinleştirme anahtarını alma](media/azure-stack-edge-deploy-prep/get-activation-key.png)

> [!IMPORTANT]
> - Etkinleştirme anahtarı üretilmeden üç gün sonra dolar.
> - Anahtarın süresi dolmuşsa yeni bir anahtar oluşturun. Eski anahtar geçerli olmaz.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, aşağıdaki gibi Azure Stack Edge konuları hakkında bilgi edindiniz:

> [!div class="checklist"]
> * Yeni kaynak oluşturma
> * Etkinleştirme anahtarı alma

Azure Stack Edge 'i nasıl yükleyeceğinizi öğrenmek için bir sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Azure Stack Edge 'i yükler](./azure-stack-edge-gpu-deploy-install.md)



