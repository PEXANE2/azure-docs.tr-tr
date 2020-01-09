---
title: Azure Data Box Edge dağıtmak için Azure portal, veri merkezi ortamı hazırlama öğreticisi | Microsoft Docs
description: Azure Data Box Edge dağıtmaya yönelik ilk öğretici Azure portal hazırlamayı içerir.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 06/03/2019
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to prepare the portal to deploy Data Box Edge so I can use it to transfer data to Azure.
ms.openlocfilehash: d9778544fd9e20dc3244a37c644117d1cd505a96
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75438678"
---
# <a name="tutorial-prepare-to-deploy-azure-data-box-edge"></a>Öğretici: dağıtıma hazırlanma Azure Data Box Edge  

Bu, Azure Data Box Edge tamamen dağıtmak için gereken dağıtım öğreticilerinde ilk öğreticidir. Bu öğreticide, Data Box Edge bir kaynağı dağıtmak üzere Azure portal nasıl hazırlanacağı açıklanmaktadır.

Kurulum ve yapılandırma işlemini tamamlamak için yönetici ayrıcalıkları gerekir. Portal hazırlığı 10 dakikadan kısa sürer.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Yeni kaynak oluşturma
> * Etkinleştirme anahtarı alma

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

### <a name="get-started"></a>Kullanmaya Başlayın

Data Box Edge dağıtmak için, önceden tanımlanmış sırada aşağıdaki öğreticilere bakın.

| **#** | **Bu adımda** | **Bu belgeleri kullanın** |
| --- | --- | --- | 
| 1. |**[Azure portalını Data Box Edge için hazırlama](data-box-edge-deploy-prep.md)** |Data Box Edge fiziksel cihazını kurmadan önce Data Box Edge kaynağınızı oluşturun ve yapılandırın. |
| 2. |**[Azure Data Box Edge'i kurma](data-box-edge-deploy-install.md)**|Data Box Edge fiziksel cihazını kutusundan çıkarın, rafa yerleştirin ve kablolarını bağlayın.  |
| 3. |**[Bağlan, ayarla ve Etkinleştir Data Box Edge](data-box-edge-deploy-connect-setup-activate.md)** |Yerel web kullanıcı arabirimine bağlayın, cihaz kurulumunu tamamlayın ve cihazı etkinleştirin. Cihaz SMB veya NFS paylaşımlarının kurulması için hazırdır.  |
| 4. |**[Verileri Data Box Edge ile aktarma](data-box-edge-deploy-add-shares.md)** |Paylaşımları ekleyin ve SMB veya NFS üzerinden paylaşımlara bağlanın. |
| 5. |**[Data Box Edge ile veri dönüştürme](data-box-edge-deploy-configure-compute.md)** |Verileri Azure 'a taşırken verileri dönüştürmek için cihazdaki işlem modüllerini yapılandırın. |

Artık Azure portalını ayarlamaya başlayabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Aşağıda, Data Box Edge kaynağınız, Data Box Edge cihazınız ve veri merkezi ağı için yapılandırma önkoşulları verilmiştir.

### <a name="for-the-data-box-edge-resource"></a>Data Box Edge kaynağı için

Başlamadan önce aşağıdakilerden emin olun:

- Microsoft Azure aboneliğiniz Data Box Edge bir kaynak için etkinleştirilir. Kullandıkça Öde abonelikleri desteklenmez.
- Kaynak grubu düzeyinde Data Box Edge/Data Box Gateway, IoT Hub ve Azure depolama kaynakları için sahip veya katkıda bulunan erişiminiz var.

    - Herhangi bir Data Box Edge/Data Box Gateway kaynağı oluşturmak için, kaynak grubu düzeyinde katkıda bulunan (veya üzeri) izinlere sahip olmanız gerekir. Ayrıca `Microsoft.DataBoxEdge` sağlayıcısının kayıtlı olduğundan emin olmanız gerekir. Kaydetme hakkında daha fazla bilgi için, [kayıt kaynak sağlayıcısı](data-box-edge-manage-access-power-connectivity-mode.md#register-resource-providers)' na gidin.
    - Herhangi bir IoT Hub kaynağı oluşturmak için Microsoft. Devices sağlayıcısının kayıtlı olduğundan emin olun. Kaydetme hakkında daha fazla bilgi için, [kayıt kaynak sağlayıcısı](data-box-edge-manage-access-power-connectivity-mode.md#register-resource-providers)' na gidin.
    - Bir depolama hesabı kaynağı oluşturmak için, kaynak grubu düzeyinde katkıda bulunan veya daha yüksek erişim kapsamına ihtiyacınız vardır. Azure depolama, varsayılan olarak kayıtlı bir kaynak sağlayıcısıdır.
- Azure Active Directory Graph API için yönetici veya Kullanıcı erişiminiz var. Daha fazla bilgi için bkz. [Azure Active Directory Graph API](https://docs.microsoft.com/previous-versions/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes#default-access-for-administrators-users-and-guest-users-).
- Erişim kimlik bilgilerine sahip bir Microsoft Azure Storage hesabınız var.

### <a name="for-the-data-box-edge-device"></a>Data Box Edge cihazı için

Fiziksel cihazı dağıtmadan önce şunlardan emin olun:

- Sevkiyat paketine dahil edilen güvenlik bilgilerini gözden geçirdiniz.
- Veri merkezinizde bulunan ve cihazı bağlamak için standart 19 "rafındaki bir 1U yuvasına sahipsiniz.
- Cihazın güvenle geri kalanında düz, kararlı ve düzeyi bir iş yüzeyine erişebilirsiniz.
- Cihazı ayarlamayı planladığınız sitenin, bağımsız bir kaynaktan veya kesintisiz güç kaynağı (UPS) olan bir raf güç dağıtım biriminden (PDU) standart AC gücü vardır.
- Fiziksel cihaza erişebiliyor olmanız gerekir.


### <a name="for-the-datacenter-network"></a>Veri merkezi ağı için

Başlamadan önce aşağıdakilerden emin olun:

- Veri merkezinizdeki ağ, Data Box Edge cihazınız için ağ gereksinimlerine göre yapılandırılır. Daha fazla bilgi için bkz. [Data Box Edge sistem gereksinimleri](data-box-edge-system-requirements.md).

- Data Box Edge normal işletim koşullarında şunları yapabilirsiniz:

    - Cihazın güncel kalmasını sağlamak için en az 10 Mbps indirme bant genişliği.
    - Dosyaları aktarmak için en az 20 Mbps adanmış karşıya yükleme ve indirme bant genişliği.

## <a name="create-a-new-resource"></a>Yeni kaynak oluşturma

Fiziksel cihazlarınızı yönetmek için bir Data Box Edge kaynağınız varsa, bu adımı atlayın ve [Etkinleştirme anahtarını alma](#get-the-activation-key) bölümüne gidin.

Bir Data Box Edge kaynağı oluşturmak için Azure portal aşağıdaki adımları uygulayın.

1. Oturum açmak için Microsoft Azure kimlik bilgilerinizi kullanın 
    
    - Bu URL 'de Azure portal: [https://portal.azure.com](https://portal.azure.com).
    - Ya da Azure Kamu portalı şu URL 'de: [https://portal.azure.us](https://portal.azure.us). Daha fazla ayrıntı için [portalı kullanarak Azure Kamu 'Ya bağlanma](https://docs.microsoft.com/azure/azure-government/documentation-government-get-started-connect-with-portal)konusuna gidin.

2. Sol bölmede **+ kaynak oluştur**' u seçin. **Data Box Edge/Data Box Gateway**arayın. **Data Box Edge/Data Box Gateway**seçin. **Oluştur**’u seçin.
3. Data Box Edge cihaz için kullanmak istediğiniz aboneliği seçin. Data Box Edge kaynağını dağıtmak istediğiniz bölgeyi seçin. Bu sürüm için Doğu ABD, Güney Doğu Asya ve Batı Avrupa kullanılabilir. 

    Cihazınızı dağıtmak istediğiniz coğrafi bölgeye yakın bir konum seçin. Bölge yalnızca cihaz yönetimi için meta verileri depolar. Gerçek veriler herhangi bir depolama hesabında depolanabilir. 
    
    **Data Box Edge** seçeneğinde **Oluştur**' u seçin.

    ![Data Box Edge hizmetini arama](media/data-box-edge-deploy-prep/data-box-edge-sku.png)

3. **Temel bilgiler** sekmesinde, aşağıdaki **proje ayrıntılarını**girin veya seçin.
    
    |Ayar  |Değer  |
    |---------|---------|
    |Abonelik    |Bu, önceki seçime göre otomatik olarak doldurulur. Abonelik fatura hesabınıza bağlıdır. |
    |Kaynak grubu  |Mevcut grubu seçin veya yeni bir grup oluşturun.<br>[Azure Kaynak Grupları](../azure-resource-manager/management/overview.md) hakkında daha fazla bilgi edinin.     |

4. Aşağıdaki **örnek ayrıntılarını**girin veya seçin.

    |Ayar  |Değer  |
    |---------|---------|
    |Ad   | Kaynağı tanımlamak için kolay bir ad.<br>Ad 2 ile 50 karakter arasında olmalı, harf, rakam ve kısa çizgilerden oluşmalıdır.<br> Ad bir harf veya rakamla başlar ve biter.        |
    |Bölge     |Bu sürüm için Doğu ABD, Güney Doğu Asya ve Batı Avrupa kaynağınızın dağıtımı için kullanılabilir. Azure Kamu kullanıyorsanız, tüm kamu bölgeleri [Azure bölgelerinde](https://azure.microsoft.com/global-infrastructure/regions/)gösterildiği gibi kullanılabilir.<br> Cihazınızı dağıtmak istediğiniz coğrafi bölgeye yakın bir konum seçin.|

    ![Proje ve örnek ayrıntıları](media/data-box-edge-deploy-prep/data-box-edge-resource.png)

5. **İleri ' yi seçin: sevkiyat adresi**.

    - Zaten bir cihazınız varsa, **Data Box Edge cihazım**için Birleşik giriş kutusunu seçin.
    - Bu, sipariş ettiğiniz yeni bir cihaz ise, kişi adını, şirketi, cihazı sevk etmek için adresi ve iletişim bilgilerini girin.

    ![Yeni cihaz için sevkiyat adresi](media/data-box-edge-deploy-prep/data-box-edge-resource1.png)

6. **İleri ' yi seçin: gözden geçir + oluştur**.

7. **Gözden geçir + oluştur** sekmesinde, **fiyatlandırma ayrıntılarını**, **kullanım koşulları**ve kaynağınızın ayrıntılarını gözden geçirin. **Gizlilik koşullarını Incelediğim**Birleşik giriş kutusunu seçin.

    ![Data Box Edge kaynak ayrıntılarını ve gizlilik koşullarını gözden geçirin](media/data-box-edge-deploy-prep/data-box-edge-resource2.png)

8. **Oluştur**’u seçin.

Kaynağın oluşturulması birkaç dakika sürer. Kaynak başarıyla oluşturulup dağıtıldıktan sonra bilgilendirirsiniz. **Kaynağa Git**' i seçin.

![Data Box Edge kaynağına git](media/data-box-edge-deploy-prep/data-box-edge-resource3.png)

Sipariş yerleştirildikten sonra, Microsoft siparişi inceler ve gönderim ayrıntıları ile size (e-posta aracılığıyla) ulaşır.

![Data Box Edge sırasının incelenmesi için bildirim](media/data-box-edge-deploy-prep/data-box-edge-resource4.png)

## <a name="get-the-activation-key"></a>Etkinleştirme anahtarı alma

Data Box Edge kaynak çalışır duruma geçtikten sonra etkinleştirme anahtarını almanız gerekir. Bu anahtar Data Box Edge cihazınızı etkinleştirmek ve kaynağa bağlamak için kullanılır. Bu anahtarı şimdi, Azure portalındayken alabilirsiniz.

1. Oluşturduğunuz kaynağı seçin. **Genel bakış** ' ı ve ardından **Cihaz kurulumu**' nu seçin.

    ![Cihaz kurulumunu seçin](media/data-box-edge-deploy-prep/data-box-edge-select-devicesetup.png)

2. **Etkinleştir** kutucuğunda bir etkinleştirme anahtarı oluşturmak Için **anahtar oluştur** ' u seçin. Anahtarı kopyalamak için Kopyala simgesini seçin ve daha sonra kullanmak üzere kaydedin.

    ![Etkinleştirme anahtarını alma](media/data-box-edge-deploy-prep/get-activation-key.png)

> [!IMPORTANT]
> - Etkinleştirme anahtarı üretilmeden üç gün sonra dolar.
> - Anahtarın süresi dolmuşsa yeni bir anahtar oluşturun. Eski anahtar geçerli olmaz.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, aşağıdaki Data Box Edge konularını öğrendiniz:

> [!div class="checklist"]
> * Yeni kaynak oluşturma
> * Etkinleştirme anahtarı alma

Data Box Edge yüklemeyi öğrenmek için bir sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Data Box Edge yüklensin](./data-box-edge-deploy-install.md)



