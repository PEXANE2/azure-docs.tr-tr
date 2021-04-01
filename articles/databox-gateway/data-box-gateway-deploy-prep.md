---
title: Azure portalını Data Box Gateway dağıtımına hazırlama öğreticisi | Microsoft Docs
description: Azure Data Box Gateway'in dağıtımına yönelik ilk öğretici Azure portalının hazırlanmasını içerir.
services: databox-edge-gateway
author: v-dalc
ms.service: databox
ms.subservice: gateway
ms.topic: tutorial
ms.date: 03/01/2021
ms.author: alkohli
ms.openlocfilehash: ceda5015770ad0b9898def181fa7199f119920db
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "101706104"
---
# <a name="tutorial-prepare-to-deploy-azure-data-box-gateway"></a>Öğretici: dağıtıma hazırlanma Azure Data Box Gateway

Bu, Azure Data Box Gateway'inizi tamamen dağıtmak için gereken dağıtım öğreticileri serisinin ilk öğreticisidir. Bu öğreticide Data Box Gateway kaynağını dağıtmak için Azure portalının nasıl hazırlanacağı açıklanır.

Kurulum ve yapılandırma işlemini tamamlamak için yönetici ayrıcalıkları gerekir. Portal hazırlığı 10 dakikadan kısa sürer.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
>
> * Yeni kaynak oluşturma
> * Sanal cihaz görüntüsünü indirme
> * Etkinleştirme anahtarı alma

## <a name="get-started"></a>başlarken

Data Box Gateway'inizi dağıtmak için, aşağıdaki öğreticileri belirtilen sırada izleyin.

| **#** | **Bu adımda** | **Bu belgeleri kullanın** |
| --- | --- | --- | 
| 1. |**[Azure portalını Data Box Gateway için hazırlama](data-box-gateway-deploy-prep.md)** |Data Box Gateway sanal cihazını sağlamadan önce Data Box Gateway kaynağınızı oluşturun ve yapılandırın. |
| 2. |**[Data Box Gateway'i Hyper-V'de sağlama](data-box-gateway-deploy-provision-hyperv.md)** <br><br><br>**[Data Box Gateway'i VMware'de sağlama](data-box-gateway-deploy-provision-vmware.md)**|Hyper-V için, Windows Server 2016 veya Windows Server 2012 R2 üzerinde Hyper-V çalıştıran konak sisteminde Data Box Gateway sanal cihazına sağlayın ve bağlayın. <br><br><br> VMware için, VMware ESXi 6,0, 6,5 veya 6,7 çalıştıran bir konak sisteminde Data Box Gateway sanal cihazına sağlayın ve bağlanın.<br></br> |
| 3. |**[Data Box Gateway'i bağlama, kurma ve etkinleştirme](data-box-gateway-deploy-connect-setup-activate.md)** |Yerel web kullanıcı arabirimine bağlayın, cihaz kurulumunu tamamlayın ve cihazı etkinleştirin. Ardından SMB paylaşımlarını sağlayabilirsiniz.  |
| 4. |**[Verileri Data Box Gateway ile aktarma](data-box-gateway-deploy-add-shares.md)** |Paylaşımları ekleyin ve SMB veya NFS üzerinden paylaşımlara bağlanın. |

Artık Azure portalını ayarlamaya başlayabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

Burada Data Box Gateway kaynağınız, Data Box Gateway cihazınız ve veri merkezi ağı için yapılandırma önkoşullarını bulabilirsiniz.

### <a name="for-the-data-box-gateway-resource"></a>Box Gateway kaynağı için

Başlamadan önce aşağıdakilerden emin olun:

* Microsoft Azure aboneliğiniz Azure Stack Edge kaynağı için etkinleştirildi. [Microsoft kurumsal anlaşma (EA)](https://azure.microsoft.com/overview/sales-number/), [bulut çözümü sağlayıcısı (CSP)](/partner-center/azure-plan-lp)veya [Microsoft Azure sponsorluğu](https://azure.microsoft.com/offers/ms-azr-0036p/)gibi desteklenen bir abonelik kullandığınızdan emin olun.
* Azure Stack Edge/Data Box Gateway, IoT Hub ve Azure depolama kaynakları için kaynak grubu düzeyinde sahip veya katkıda bulunan erişiminiz var.
    - Azure Stack Edge/Data Box Gateway kaynağı oluşturmak için, kaynak grubu düzeyinde katkıda bulunan (veya üzeri) izinlere sahip olmanız gerekir. Ayrıca, sağlayıcının kayıtlı olduğundan emin olmanız gerekir `Microsoft.DataBoxEdge` . Nasıl kaydedileceği hakkında bilgi için [Kaynak sağlayıcısını kaydetme](data-box-gateway-manage-access-power-connectivity-mode.md#register-resource-providers) bölümüne gidin.
    - Bir depolama hesabı kaynağı oluşturmak için, kaynak grubu düzeyinde katkıda bulunan veya daha yüksek erişim kapsamına ihtiyacınız vardır. Azure depolama, varsayılan olarak kayıtlı bir kaynak sağlayıcısıdır.
- Microsoft Graph API 'sine yönetici veya Kullanıcı erişimi vardır. Daha fazla bilgi için bkz. [Microsoft Graph izinleri başvurusu](/graph/permissions-reference).
- Erişim kimlik bilgilerine sahip bir Microsoft Azure Storage hesabınız var.

### <a name="for-the-data-box-gateway-device"></a>Data Box Gateway cihazı için

Sanal cihazı dağıtmadan önce şunlardan emin olun:

- Bir cihaz sağlamak için kullanılabilecek Windows Server 2012 R2 veya üzeri ya da VMware (ESXi 6,0, 6,5 veya 6,7) üzerinde Hyper-V çalıştıran bir konak sistemine erişiminiz var.
- Konak sistemi, Data Box sanal cihazınızı sağlamak için aşağıdaki kaynakları ayırabilir:
  
  - En az 4 sanal işlemci.
  - En az 8 GB RAM. En az 16 GB RAM kullanmanızı öneririz.
  - Bir ağ arabirimi.
  - 250 GB işletim sistemi diski.
  - Sistem verileri için 2 TB sanal disk.

### <a name="for-the-datacenter-network"></a>Veri merkezi ağı için

Başlamadan önce aşağıdakilerden emin olun:

- Veri merkezinizdeki ağ, Data Box Gateway cihazınızın ağ gereksinimlerine uygun olarak yapılandırılmış. Daha fazla bilgi için [Data Box Gateway sistem gereksinimleri](data-box-gateway-system-requirements.md)' ne bakın.

- Data Box Gateway normal işletim koşullarında, şunları yapmanız gerekir:

    - Cihazın güncel kalmasını sağlamak için en az 10 Mbps indirme bant genişliği.
    - Dosyaları aktarmak için en az 20 Mbps adanmış karşıya yükleme ve indirme bant genişliği.

## <a name="create-a-new-resource"></a>Yeni kaynak oluşturma

Sanal cihazlarınızı yönetmek için bir Data Box Gateway kaynağınız varsa, bu adımı atlayın ve [Etkinleştirme anahtarını alma](#get-the-activation-key) bölümüne gidin.

Bir Data Box Gateway kaynağı oluşturmak için Azure portal aşağıdaki adımları uygulayın.

1. Microsoft Azure kimlik bilgilerinizi kullanarak şu portallardan birinde oturum açın:

    - Bu URL 'de Azure portal: [https://portal.azure.com](https://portal.azure.com) .
    - Bu URL 'de Azure Kamu Portalı: [https://portal.azure.us](https://portal.azure.us) . Ayrıntılar için [Portal 'ı kullanarak Azure Kamu 'Ya bağlanma](../azure-government/documentation-government-get-started-connect-with-portal.md)bölümüne gidin.
    
2. **+ Kaynak oluştur**’u seçin.

    ![Kaynak oluştur düğmesi Azure Data Box Gateway](media/data-box-gateway-deploy-prep/data-box-gateway-create-a-resource.png)

3. Arama kutusuna **Data Box Gateway** yazın ve ENTER tuşuna basın.

    ![Data Box Gateway hizmetini arayın](media/data-box-gateway-deploy-prep/data-box-gateway-search-box.png)

    **Azure Data Box Gateway** öğesini seçin.

    ![Data Box Gateway SKU 'sunu seçin](media/data-box-gateway-deploy-prep/data-box-gateway-sku.png)

4. **Oluştur**’u seçin.

    ![Data Box Gateway kaynağını oluşturmak için Oluştur ' a tıklayın](media/data-box-gateway-deploy-prep/data-box-gateway-create.png)

5. **Temel Bilgiler** sekmesinde:

    Aşağıdaki **proje ayrıntılarını** girin veya seçin.
    
    |Ayar  |Değer  |
    |---------|---------|
    |Abonelik    |Data Box Gateway cihazınız için kullanmak istediğiniz aboneliği seçin. Abonelik fatura hesabınıza bağlıdır.|
    |Kaynak grubu  |Mevcut grubu seçin veya yeni bir grup oluşturun.<br>[Azure Kaynak Grupları](../azure-resource-manager/management/overview.md) hakkında daha fazla bilgi edinin.|

   Aşağıdaki **örnek ayrıntılarını** girin veya seçin.

    |Ayar  |Değer  |
    |---------|---------|
    |Ad   |Kaynağı tanımlamak için kolay bir ad.<br>Ad, harf, sayı ve kısa çizgi içeren 2 ila 50 karakter arasındadır. <br> Ad bir harf veya sayıyla başlamalı ve bitmelidir. |
    |Region  |Kaynağı dağıtmak istediğiniz bölgeyi seçin. Cihazınızı dağıtmak istediğiniz coğrafi bölgeye yakın bir konum seçin. <br> Veri tabanlı ağ geçidi/Azure Stack Edge kaynaklarının kullanılabildiği tüm bölgelerin listesi için bkz. [bölgeye göre kullanılabilir Azure ürünleri](https://azure.microsoft.com/global-infrastructure/services/?products=databox&regions=all). <br> Azure Kamu 'da, [Azure bölgelerinde](https://azure.microsoft.com/global-infrastructure/regions/) listelenen tüm kamu bölgeleri kullanılabilir.|

   Ardından, siparişinizi gözden geçirmek için **gözden geçir + oluştur** ' u seçin.

   ![Data Box Gateway sırası için proje ve örnek ayrıntısı girişi](media/data-box-gateway-deploy-prep/data-box-gateway-basics.png)

7. **Gözden geçir + oluştur** sekmesinde, **fiyatlandırma ayrıntılarını**, **kullanım koşulları** ve kaynağınızın ayrıntılarını gözden geçirin. **Oluştur**’u seçin.

    ![Gözden geçirilmek üzere Data Box Gateway kaynak ayrıntıları gösteriliyor](media/data-box-gateway-deploy-prep/data-box-gateway-resource-review-create.png)

Kaynağın oluşturulması birkaç dakika sürer. Kaynak başarıyla oluşturulup dağıtıldıktan sonra bilgilendirirsiniz. **Kaynağa git**’i seçin.

![Tamamlanmış bir Data Box Gateway sırası](media/data-box-gateway-deploy-prep/data-box-gateway-completed-order.png)

## <a name="download-the-virtual-device-image"></a>Sanal cihaz görüntüsünü indirme

Data Box Gateway kaynağı oluşturulduktan sonra, konak sisteminizde sanal makineyi sağlamak için uygun sanal cihaz görüntüsünü indirin. Sanal cihaz görüntüleri bir işletim sistemine özeldir.

> [!IMPORTANT]
> Data Box Gateway üzerinde çalıştırılan yazılım yalnızca Data Box Gateway kaynağıyla kullanılabilir.

Sanal cihaz görüntüsünü indirmek için [Azure Portal](https://portal.azure.com/) aşağıdaki adımları izleyin.

1. Oluşturduğunuz kaynakta **genel bakış**' ı seçin. Mevcut bir Azure Data Box Gateway kaynağınız varsa, kaynağı seçin ve **Genel Bakış ' a** gidin. **Cihaz kurulumunu** seçin.

    ![Yeni Data Box Gateway kaynağı](media/data-box-gateway-deploy-prep/data-box-gateway-resource-created.png)

2. **Görüntü indir** KUTUCUĞUNDA, VM sağlamak için kullanılan ana bilgisayar sunucusundaki işletim sistemine karşılık gelen sanal cihaz görüntüsünü seçin. Görüntü dosyaları yaklaşık 5,6 GB 'dir.
   
   * [Windows Server 2012 R2 ve sonraki sürümleri üzerinde Hyper-V için VHDX](https://aka.ms/dbe-vhdx-2012).
   * [VMware ESXi 6,0, 6,5 veya 6,7 Için VMDK](https://aka.ms/dbe-vmdk).

    ![Data Box Gateway sanal cihaz görüntüsünü indir](media/data-box-gateway-deploy-prep/data-box-gateway-download-image.png)

5. Dosyayı yerel sürücüye indirin ve sıkıştırmasını açın. Sıkıştırması açılan dosyanın konumunu not alın.


## <a name="get-the-activation-key"></a>Etkinleştirme anahtarı alma

Data Box Gateway kaynak çalışır duruma geçtikten sonra etkinleştirme anahtarını almanız gerekir. Bu anahtar Data Box Gateway cihazınızı etkinleştirmek ve kaynağa bağlamak için kullanılır. Bu anahtarı şimdi, Azure portalındayken alabilirsiniz.

1. Oluşturduğunuz kaynağı seçin ve ardından **genel bakış**' ı seçin. **Cihaz kurulumunda**, **Yapılandır ve Etkinleştir** kutucuğuna gidin.

    ![Kutucuğu Yapılandır ve Etkinleştir](media/data-box-gateway-deploy-prep/data-box-gateway-configure-activate.png)

2. Etkinleştirme anahtarı oluşturmak için **anahtar oluştur** ' u seçin. Anahtarı kopyalamak için Kopyala simgesini seçin ve daha sonra kullanmak üzere kaydedin.

    ![Etkinleştirme anahtarını alma](media/data-box-gateway-deploy-prep/get-activation-key.png)

> [!IMPORTANT]
> - Etkinleştirme anahtarı üretilmeden üç gün sonra dolar.
> - Anahtarın süresi dolmuşsa yeni bir anahtar oluşturun. Eski anahtar geçerli olmaz.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, aşağıdaki Data Box Gateway konularını öğrendiniz:

> [!div class="checklist"]
> * Yeni kaynak oluşturma
> * Sanal cihaz görüntüsünü indirme
> * Etkinleştirme anahtarı alma

Data Box Gateway'inize sanal makine sağlamayı öğrenmek için sonraki öğreticiye ilerleyin. Konak işletim sisteminize bağlı olarak, şuradaki ayrıntılı yönergelere bakın:

> [!div class="nextstepaction"]
> [Data Box Gateway'i Hyper-V'de sağlama](./data-box-gateway-deploy-provision-hyperv.md)

VEYA

> [!div class="nextstepaction"]
> [Data Box Gateway'i VMware'de sağlama](./data-box-gateway-deploy-provision-vmware.md)