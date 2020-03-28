---
title: Azure portalını Data Box Gateway dağıtımına hazırlama öğreticisi | Microsoft Docs
description: Azure Data Box Gateway'in dağıtımına yönelik ilk öğretici Azure portalının hazırlanmasını içerir.
services: databox-edge-gateway
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: tutorial
ms.date: 06/24/2019
ms.author: alkohli
ms.openlocfilehash: 74fec059bdffb91f5a7774d430e2f1897f0e863c
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79474467"
---
# <a name="tutorial-prepare-to-deploy-azure-data-box-gateway"></a>Öğretici: Azure Veri Kutusu Ağ Geçidi'ni dağıtmaya hazırlanın

Bu, Azure Data Box Gateway'inizi tamamen dağıtmak için gereken dağıtım öğreticileri serisinin ilk öğreticisidir. Bu öğreticide Data Box Gateway kaynağını dağıtmak için Azure portalının nasıl hazırlanacağı açıklanır.

Kurulum ve yapılandırma işlemini tamamlamak için yönetici ayrıcalıkları gerekir. Portal hazırlığı 10 dakikadan kısa sürer.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
>
> * Yeni kaynak oluşturma
> * Sanal cihaz görüntüsünü indirme
> * Etkinleştirme anahtarı alma

## <a name="get-started"></a>Kullanmaya başlayın

Data Box Gateway'inizi dağıtmak için, aşağıdaki öğreticileri belirtilen sırada izleyin.

| **#** | **Bu adımda** | **Bu belgeleri kullanın** |
| --- | --- | --- | 
| 1. |**[Azure portalını Data Box Gateway için hazırlama](data-box-gateway-deploy-prep.md)** |Data Box Gateway sanal cihazını sağlamadan önce Data Box Gateway kaynağınızı oluşturun ve yapılandırın. |
| 2. |**[Data Box Gateway'i Hyper-V'de sağlama](data-box-gateway-deploy-provision-hyperv.md)** <br><br><br>**[Data Box Gateway'i VMware'de sağlama](data-box-gateway-deploy-provision-vmware.md)**|Hyper-V için, Windows Server 2016 veya Windows Server 2012 R2 üzerinde Hyper-V çalıştıran konak sisteminde Data Box Gateway sanal cihazına sağlayın ve bağlayın. <br><br><br> VMware için, VMware ESXi 6.0, 6.5 veya 6.7 çalıştıran bir ana bilgisayar sisteminde bir Veri Kutusu Ağ Geçidi sanal aygıtı sağlama ve bağlanma.<br></br> |
| 3. |**[Data Box Gateway'i bağlama, kurma ve etkinleştirme](data-box-gateway-deploy-connect-setup-activate.md)** |Yerel web kullanıcı arabirimine bağlayın, cihaz kurulumunu tamamlayın ve cihazı etkinleştirin. Ardından SMB paylaşımlarını sağlayabilirsiniz.  |
| 4. |**[Verileri Data Box Gateway ile aktarma](data-box-gateway-deploy-add-shares.md)** |Paylaşımları ekleyin ve SMB veya NFS üzerinden paylaşımlara bağlanın. |

Artık Azure portalını ayarlamaya başlayabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Burada Data Box Gateway kaynağınız, Data Box Gateway cihazınız ve veri merkezi ağı için yapılandırma önkoşullarını bulabilirsiniz.

### <a name="for-the-data-box-gateway-resource"></a>Box Gateway kaynağı için

Başlamadan önce aşağıdakilerden emin olun:

* Microsoft Azure aboneliğiniz, Azure Yığını Kenarı kaynağı için etkinleştirilir. [Microsoft Kurumsal Sözleşme (EA) ,](https://azure.microsoft.com/overview/sales-number/)Bulut Çözüm Sağlayıcısı [(CSP)](https://docs.microsoft.com/partner-center/azure-plan-lp)veya Microsoft [Azure Sponsorluğu](https://azure.microsoft.com/offers/ms-azr-0036p/)gibi desteklenen bir abonelik kullandığınızdan emin olun.
* Veri Kutusu Kenarı/Veri Kutusu Ağ Geçidi, IoT Hub ve Azure Depolama kaynakları için kaynak grubu düzeyinde sahibinize veya katılımcıya erişiminiz vardır.
    - Herhangi bir Veri Kutusu Kenarı/ Veri Kutusu Ağ Geçidi kaynağı oluşturmak için, kaynak grubu düzeyinde katılımcı (veya daha yüksek) kapsamlı izinlere sahip olmalısınız. Ayrıca sağlayıcının `Microsoft.DataBoxEdge` kayıtlı olduğundan emin olmanız gerekir. Nasıl kaydolunca yayılıhakkında bilgi için [Kaynak Sağlayıcısını Kaydet'e](data-box-gateway-manage-access-power-connectivity-mode.md#register-resource-providers)gidin.
    - Bir Depolama hesabı kaynağı oluşturmak için yine kaynak grubu düzeyinde katılımcı veya daha yüksek erişim kapsamına almanız gerekir. Azure Depolama varsayılan olarak kayıtlı bir kaynak sağlayıcısıdır.
- Microsoft Graph API'ye yönetici veya kullanıcı erişiminiz var. Daha fazla bilgi için [Microsoft Graph izinleri başvurusuna](https://docs.microsoft.com/graph/permissions-reference)bakın.
- Erişim kimlik bilgilerine sahip bir Microsoft Azure Storage hesabınız var.

### <a name="for-the-data-box-gateway-device"></a>Data Box Gateway cihazı için

Sanal cihazı dağıtmadan önce şunlardan emin olun:

- Windows Server 2012 R2 veya sonraki lerinde Hyper-V veya bir aygıt hükmünde kullanılabilecek VMware (ESXi 6.0, 6.5 veya 6.7) çalıştıran bir ana bilgisayar sistemine erişebilirsiniz.
- Konak sistemi, Data Box sanal cihazınızı sağlamak için aşağıdaki kaynakları ayırabilir:
  
  - En az 4 sanal işlemci.
  - En az 8 GB RAM.
  - Bir ağ arabirimi.
  - 250 GB işletim sistemi diski.
  - Sistem verileri için 2 TB sanal disk.

### <a name="for-the-datacenter-network"></a>Veri merkezi ağı için

Başlamadan önce aşağıdakilerden emin olun:

- Veri merkezinizdeki ağ, Data Box Gateway cihazınızın ağ gereksinimlerine uygun olarak yapılandırılmış. Daha fazla bilgi için [Veri Kutusu Ağ Geçidi sistem gereksinimlerine](data-box-gateway-system-requirements.md)bakın.

- Veri Kutusu Ağ Geçidinizin normal çalışma koşulları için şunları olmalıdır:

    - Aygıtın güncel kalmasını sağlamak için en az 10 Mbps indirme bant genişliği.
    - Dosyaları aktarmak için en az 20 Mbps'lik yükleme ve indirme bant genişliği.

## <a name="create-a-new-resource"></a>Yeni kaynak oluşturma

Sanal cihazlarınızı yönetmek için bir Data Box Gateway kaynağınız varsa, bu adımı atlayın ve [Etkinleştirme anahtarını alma](#get-the-activation-key) bölümüne gidin.

Veri Kutusu Ağ Geçidi kaynağı oluşturmak için Azure portalında aşağıdaki adımları izleyin.

1. Oturum açtırmak için Microsoft Azure kimlik bilgilerinizi kullanın:

    - Bu URL'deki Azure [https://portal.azure.com](https://portal.azure.com)portalı: .
    - Veya bu URL'deki Azure [https://portal.azure.us](https://portal.azure.us)Resmi portalı: . Daha fazla bilgi için [portalı kullanarak Azure Devlet'e Bağlan'a](https://docs.microsoft.com/azure/azure-government/documentation-government-get-started-connect-with-portal)gidin.

2. Sol bölmede + **Kaynak oluştur'u**seçin. Veri **Kutusu Kenar / Veri Kutusu Ağ Geçidi**arayın. Veri Kutusu Kenarı / Veri Kutusu Ağ Geçidi'ni seçin. **Oluştur'u**seçin.
3. Veri Kutusu Ağ Geçidi aygıtı için kullanmak istediğiniz aboneliği seçin. Veri Kutusu Ağ Geçidi kaynağını dağıtmak istediğiniz bölgeyi seçin. Azure Yığını Kenarı kaynağının bulunduğu tüm bölgelerin listesi [için, bölgeye göre kullanılabilen Azure ürünlerine](https://azure.microsoft.com/global-infrastructure/services/?products=databox&regions=all)bakın. Cihazınızı dağıtmak istediğiniz coğrafi bölgeye yakın bir konum seçin. Veri **Kutusu Ağ Geçidi** seçeneğinde **Oluştur'u**seçin.

    ![Data Box Gateway hizmetini arama](media/data-box-gateway-deploy-prep/data-box-gateway-edge-sku.png)

4. Temel **Bilgiler** sekmesine aşağıdaki Proje **ayrıntılarını**girin veya seçin.
    
    |Ayar  |Değer  |
    |---------|---------|
    |Abonelik    |Bu, önceki seçime göre otomatik olarak doldurulur. Abonelik fatura hesabınıza bağlıdır. |
    |Kaynak grubu  |Mevcut grubu seçin veya yeni bir grup oluşturun.<br>[Azure Kaynak Grupları](../azure-resource-manager/management/overview.md) hakkında daha fazla bilgi edinin.     |

5. Aşağıdaki **Örnek ayrıntılarını**girin veya seçin.

    |Ayar  |Değer  |
    |---------|---------|
    |Adı   | Kaynağı tanımlamak için kolay bir ad.<br>Ad 2 ile 50 karakter arasında olmalı, harf, rakam ve kısa çizgilerden oluşmalıdır.<br> Ad bir harf veya rakamla başlar ve biter.        |   
    |Bölge     |Azure Yığını Kenarı kaynağının bulunduğu tüm bölgelerin listesi [için, bölgeye göre kullanılabilen Azure ürünlerine](https://azure.microsoft.com/global-infrastructure/services/?products=databox&regions=all)bakın. Azure Kamu için, [Azure bölgelerinde](https://azure.microsoft.com/global-infrastructure/regions/) listelenen tüm devlet bölgeleri kullanılabilir. <br> Cihazınızı dağıtmak istediğiniz coğrafi bölgeye yakın bir konum seçin.|
    
    ![Data Box Gateway kaynağını oluşturma](media/data-box-gateway-deploy-prep/data-box-gateway-resource.png)
    
6. **İncele ve oluştur**’u seçin.
 
7. Gözden **Geçirme + oluştur** sekmesinde Fiyatlandırma **ayrıntılarını,** **kullanım koşullarını**ve kaynağınızın ayrıntılarını gözden geçirin. **Oluştur'u**seçin.

    ![Veri Kutusu Ağ Geçidi kaynak ayrıntılarını gözden geçirin](media/data-box-gateway-deploy-prep/data-box-gateway-resource1.png)

Kaynağın oluşturulması birkaç dakika sürer. Kaynak başarıyla oluşturulduktan ve dağıtıldıktan sonra size bildirilir. **Kaynağa Git'i**seçin.

![Veri Kutusu Ağ Geçidi kaynak ayrıntılarını gözden geçirin](media/data-box-gateway-deploy-prep/data-box-gateway-resource2.png)

## <a name="download-the-virtual-device-image"></a>Sanal cihaz görüntüsünü indirme

Data Box Gateway kaynağı oluşturulduktan sonra, konak sisteminizde sanal makineyi sağlamak için uygun sanal cihaz görüntüsünü indirin. Sanal aygıt görüntüleri bir işletim sistemine özgür.

> [!IMPORTANT]
> Data Box Gateway üzerinde çalıştırılan yazılım yalnızca Data Box Gateway kaynağıyla kullanılabilir.

Sanal aygıt görüntüsü indirmek için [Azure portalında](https://portal.azure.com/) aşağıdaki adımları izleyin.

1. Oluşturduğunuz kaynakta genel **bakış'ı**seçin. Varolan bir Azure Veri Kutusu Ağ Geçidi kaynağınız varsa, kaynağı seçin ve **Genel Bakış'a**gidin. **Aygıt kurulumünü**seçin.

    ![Yeni Data Box Gateway kaynağı](media/data-box-gateway-deploy-prep/data-box-gateway-resource-created.png)

2. **İndirgörüntü** döşemesi üzerinde, VM'yi sağlamak için kullanılan ana bilgisayar sunucusundaki işletim sistemine karşılık gelen sanal aygıt görüntüsünü seçin. Görüntü dosyaları yaklaşık 5,6 GB'dır.
   
   * [Windows Server 2012 R2 ve sonraki sürümleri üzerinde Hyper-V için VHDX](https://aka.ms/dbe-vhdx-2012).
   * [VMWARE ESXi 6.0, 6.5 veya 6.7 için VMDK.](https://aka.ms/dbe-vmdk)

    ![Veri Kutusu Ağ Geçidi sanal cihaz görüntüsünü indirin](media/data-box-gateway-deploy-prep/data-box-gateway-download-image.png)

5. Dosyayı yerel sürücüye indirin ve sıkıştırmasını açın. Sıkıştırması açılan dosyanın konumunu not alın.


## <a name="get-the-activation-key"></a>Etkinleştirme anahtarı alma

Veri Kutusu Ağ Geçidi kaynağı çalışmaya devam ettikten sonra etkinleştirme anahtarını almanız gerekir. Bu anahtar Data Box Gateway cihazınızı etkinleştirmek ve kaynağa bağlamak için kullanılır. Bu anahtarı şimdi, Azure portalındayken alabilirsiniz.

1. Oluşturduğunuz kaynağı seçin ve ardından **Genel Bakış'ı**seçin. Aygıt **kurulumunda,** Yapıla'ya gidin ve döşemeyi **etkinleştirin.**

    ![Döşemeyi yapılandırma ve etkinleştirme](media/data-box-gateway-deploy-prep/data-box-gateway-configure-activate.png)

2. Etkinleştirme anahtarı oluşturmak için **Oluştur anahtarını** seçin. Anahtarı kopyalamak ve daha sonra kullanmak üzere kaydetmek için kopya simgesini seçin.

    ![Etkinleştirme anahtarını alma](media/data-box-gateway-deploy-prep/get-activation-key.png)

> [!IMPORTANT]
> - Etkinleştirme anahtarı oluşturulduktan üç gün sonra sona erer.
> - Anahtarın süresi dolduysa, yeni bir anahtar oluşturun. Eski anahtar geçerli olmaz.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, aşağıdaki Data Box Gateway konularını öğrendiniz:

> [!div class="checklist"]
> * Yeni kaynak oluşturma
> * Sanal cihaz görüntüsünü indirme
> * Etkinleştirme anahtarı alma

Data Box Gateway'inize sanal makine sağlamayı öğrenmek için sonraki öğreticiye ilerleyin. Konak işletim sisteminize bağlı olarak, şuradaki ayrıntılı yönergelere bakın:

> [!div class="nextstepaction"]
> [Data Box Gateway'i Hyper-V'de sağlama](./data-box-gateway-deploy-provision-hyperv.md)

OR

> [!div class="nextstepaction"]
> [Data Box Gateway'i VMware'de sağlama](./data-box-gateway-deploy-provision-vmware.md)


