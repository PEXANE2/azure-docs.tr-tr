---
title: Azure portalı, veri merkezi ortamını hazırlamak için Öğretici Azure Veri Kutusu Kenarı dağıtmak için | Microsoft Dokümanlar
description: Azure Veri Kutusu Kenarını dağıtma yla ilgili ilk öğretici, Azure portalının hazırlanmasını içerir.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 06/03/2019
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to prepare the portal to deploy Data Box Edge so I can use it to transfer data to Azure.
ms.openlocfilehash: 90ed4bf8f0389619f130e998ed76c720442092b2
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79474484"
---
# <a name="tutorial-prepare-to-deploy-azure-data-box-edge"></a>Öğretici: Azure Veri Kutusu Kenarı dağıtmaya hazırlanın  

Bu, Azure Veri Kutusu Kenarını tamamen dağıtmak için gereken dağıtım öğreticileri serisinin ilk öğreticisidir. Bu öğretici, Bir Veri Kutusu Kenarı kaynağını dağıtmak için Azure portalının nasıl hazırlanacağını açıklar.

Kurulum ve yapılandırma işlemini tamamlamak için yönetici ayrıcalıkları gerekir. Portal hazırlığı 10 dakikadan kısa sürer.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
>
> * Yeni kaynak oluşturma
> * Etkinleştirme anahtarı alma

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

### <a name="get-started"></a>Kullanmaya başlayın

Veri Kutusu Kenarını dağıtmak için, öngörülen sırayla aşağıdaki öğreticilere bakın.

| **#** | **Bu adımda** | **Bu belgeleri kullanın** |
| --- | --- | --- | 
| 1. |**[Azure portalını Data Box Edge için hazırlama](data-box-edge-deploy-prep.md)** |Data Box Edge fiziksel cihazını kurmadan önce Data Box Edge kaynağınızı oluşturun ve yapılandırın. |
| 2. |**[Azure Data Box Edge'i kurma](data-box-edge-deploy-install.md)**|Data Box Edge fiziksel cihazını kutusundan çıkarın, rafa yerleştirin ve kablolarını bağlayın.  |
| 3. |**[Veri Kutusu Kenarı'nı bağlama, ayarlama ve etkinleştirme](data-box-edge-deploy-connect-setup-activate.md)** |Yerel web kullanıcı arabirimine bağlayın, cihaz kurulumunu tamamlayın ve cihazı etkinleştirin. Cihaz SMB veya NFS paylaşımlarının kurulması için hazırdır.  |
| 4. |**[Verileri Data Box Edge ile aktarma](data-box-edge-deploy-add-shares.md)** |Paylaşımları ekleyin ve SMB veya NFS üzerinden paylaşımlara bağlanın. |
| 5. |**[Data Box Edge ile verileri dönüştürme](data-box-edge-deploy-configure-compute.md)** |Verileri Azure'a dönüştürmek için aygıttaki bilgi işlem modüllerini yapılandırın. |

Artık Azure portalını ayarlamaya başlayabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Aşağıda, Veri Kutusu Edge kaynağınız, Veri Kutusu Kenarı aygıtınız ve veri merkezi ağı için yapılandırma ön koşulları ve resuler ve veriler merkezi ağı verebilirsiniz.

### <a name="for-the-data-box-edge-resource"></a>Data Box Edge kaynağı için

Başlamadan önce aşağıdakilerden emin olun:

* Microsoft Azure aboneliğiniz, Azure Yığını Kenarı kaynağı için etkinleştirilir. [Microsoft Kurumsal Sözleşme (EA) ,](https://azure.microsoft.com/overview/sales-number/)Bulut Çözüm Sağlayıcısı [(CSP)](https://docs.microsoft.com/partner-center/azure-plan-lp)veya Microsoft [Azure Sponsorluğu](https://azure.microsoft.com/offers/ms-azr-0036p/)gibi desteklenen bir abonelik kullandığınızdan emin olun.
* Veri Kutusu Kenarı/Veri Kutusu Ağ Geçidi, IoT Hub ve Azure Depolama kaynakları için kaynak grubu düzeyinde sahibinize veya katılımcıya erişiminiz vardır.
    - Herhangi bir Veri Kutusu Kenarı/ Veri Kutusu Ağ Geçidi kaynağı oluşturmak için, kaynak grubu düzeyinde katılımcı (veya daha yüksek) kapsamlı izinlere sahip olmalısınız. Ayrıca sağlayıcının `Microsoft.DataBoxEdge` kayıtlı olduğundan emin olmanız gerekir. Nasıl kaydolunca yayılıhakkında bilgi için [Kaynak Sağlayıcısını Kaydet'e](data-box-edge-manage-access-power-connectivity-mode.md#register-resource-providers)gidin.
    - Herhangi bir IoT Hub kaynağı oluşturmak için Microsoft.Devices sağlayıcısının kayıtlı olduğundan emin olun. Nasıl kaydolunca yayılıhakkında bilgi için [Kaynak Sağlayıcısını Kaydet'e](data-box-edge-manage-access-power-connectivity-mode.md#register-resource-providers)gidin.
    - Bir Depolama hesabı kaynağı oluşturmak için yine kaynak grubu düzeyinde katılımcı veya daha yüksek erişim kapsamına almanız gerekir. Azure Depolama varsayılan olarak kayıtlı bir kaynak sağlayıcısıdır.
- Microsoft Graph API'ye yönetici veya kullanıcı erişiminiz var. Daha fazla bilgi için [Microsoft Graph izinleri başvurusuna](https://docs.microsoft.com/graph/permissions-reference)bakın.
- Erişim kimlik bilgilerine sahip bir Microsoft Azure Storage hesabınız var.

### <a name="for-the-data-box-edge-device"></a>Data Box Edge cihazı için

Fiziksel cihazı dağıtmadan önce şunlardan emin olun:

- Sevkiyat paketinde yer alan güvenlik bilgilerini incelediniz.
- Cihazı rafa monte etmek için veri merkezinizde standart 19" rafta kullanılabilen bir 1U yuvası na sahipsiniz.
- Aygıtın güvenli bir şekilde dinlenebileceği düz, kararlı ve düz bir çalışma yüzeyine erişebilirsiniz.
- Cihazı kurmak istediğiniz site, bağımsız bir kaynaktan veya kesintisiz güç kaynağına (UPS) sahip bir raf güç dağıtım ünitesinden (PDU) standart AC gücüne sahiptir.
- Fiziksel cihaza erişebiliyor olmanız gerekir.


### <a name="for-the-datacenter-network"></a>Veri merkezi ağı için

Başlamadan önce aşağıdakilerden emin olun:

- Veri merkezinizdeki ağ, Veri Kutusu Edge aygıtınızın ağ gereksinimlerine göre yapılandırılır. Daha fazla bilgi için [Bkz. Veri Kutusu Kenar Sistemi Gereksinimleri.](data-box-edge-system-requirements.md)

- Veri Kutusu Edge'inizin normal çalışma koşulları için şunları elde elabilirsiniz:

    - Aygıtın güncel kalmasını sağlamak için en az 10 Mbps indirme bant genişliği.
    - Dosyaları aktarmak için en az 20 Mbps'lik yükleme ve indirme bant genişliği.

## <a name="create-a-new-resource"></a>Yeni kaynak oluşturma

Fiziksel cihazlarınızı yönetmek için bir Data Box Edge kaynağınız varsa, bu adımı atlayın ve [Etkinleştirme anahtarını alma](#get-the-activation-key) bölümüne gidin.

Bir Veri Kutusu Kenarı kaynağı oluşturmak için Azure portalında aşağıdaki adımları izleyin.

1. Oturum açtırmak için Microsoft Azure kimlik bilgilerinizi kullanın 
    
    - Bu URL'deki Azure [https://portal.azure.com](https://portal.azure.com)portalı: .
    - Veya bu URL'deki Azure [https://portal.azure.us](https://portal.azure.us)Resmi portalı: . Daha fazla bilgi için [portalı kullanarak Azure Devlet'e Bağlan'a](https://docs.microsoft.com/azure/azure-government/documentation-government-get-started-connect-with-portal)gidin.

2. Sol bölmede + **Kaynak oluştur'u**seçin. Veri **Kutusu Kenar / Veri Kutusu Ağ Geçidi**arayın. **Veri Kutusu Kenarı / Veri Kutusu Ağ Geçidi'ni**seçin. **Oluştur'u**seçin.
3. Veri Kutusu Kenarı aygıtı için kullanmak istediğiniz aboneliği seçin. Data Box Edge kaynağını dağıtmak istediğiniz bölgeyi seçin. Azure Yığını Kenarı kaynağının bulunduğu tüm bölgelerin listesi [için, bölgeye göre kullanılabilen Azure ürünlerine](https://azure.microsoft.com/global-infrastructure/services/?products=databox&regions=all)bakın.

    Cihazınızı dağıtmak istediğiniz coğrafi bölgeye yakın bir konum seçin. Bölge yalnızca aygıt yönetimi için meta verileri depolar. Gerçek veriler herhangi bir depolama hesabında depolanabilir.
    
    Veri **Kutusu Kenarı** seçeneğinde **Oluştur'u**seçin.

    ![Data Box Edge hizmetini arama](media/data-box-edge-deploy-prep/data-box-edge-sku.png)

3. Temel **Bilgiler** sekmesine aşağıdaki Proje **ayrıntılarını**girin veya seçin.
    
    |Ayar  |Değer  |
    |---------|---------|
    |Abonelik    |Bu, önceki seçime göre otomatik olarak doldurulur. Abonelik fatura hesabınıza bağlıdır. |
    |Kaynak grubu  |Mevcut grubu seçin veya yeni bir grup oluşturun.<br>[Azure Kaynak Grupları](../azure-resource-manager/management/overview.md) hakkında daha fazla bilgi edinin.     |

4. Aşağıdaki **Örnek ayrıntılarını**girin veya seçin.

    |Ayar  |Değer  |
    |---------|---------|
    |Adı   | Kaynağı tanımlamak için kolay bir ad.<br>Ad 2 ile 50 karakter arasında olmalı, harf, rakam ve kısa çizgilerden oluşmalıdır.<br> Ad bir harf veya rakamla başlar ve biter.        |
    |Bölge     |Azure Yığını Kenarı kaynağının bulunduğu tüm bölgelerin listesi [için, bölgeye göre kullanılabilen Azure ürünlerine](https://azure.microsoft.com/global-infrastructure/services/?products=databox&regions=all)bakın. Azure Kamu kullanıyorsanız, tüm devlet bölgeleri [Azure bölgelerinde](https://azure.microsoft.com/global-infrastructure/regions/)gösterildiği gibi kullanılabilir.<br> Cihazınızı dağıtmak istediğiniz coğrafi bölgeye yakın bir konum seçin.|

    ![Proje ve örnek ayrıntıları](media/data-box-edge-deploy-prep/data-box-edge-resource.png)

5. **Sonraki Seçiniz: Gönderi adresi**.

    - Zaten bir aygıtınız varsa, **veri kutusu kenarı aygıtım olduğu**için açılan kutuyu seçin.
    - Sipariş ettiğiniz yeni aygıt buysa, cihazı sevk etmek için kişi adını, şirketi, adresi ve iletişim bilgilerini girin.

    ![Yeni cihazın gönderim adresi](media/data-box-edge-deploy-prep/data-box-edge-resource1.png)

6. **Sonraki'ni Seçin: Gözden Geçirin + oluşturun.**

7. Gözden **Geçirme + oluştur** sekmesinde Fiyatlandırma **ayrıntılarını,** **kullanım koşullarını**ve kaynağınızın ayrıntılarını gözden geçirin. Gizlilik koşullarını gözden **geçirdiğim**için açılan kutuyu seçin.

    ![Veri Kutusu Kenarı kaynak ayrıntılarını ve gizlilik koşullarını gözden geçirin](media/data-box-edge-deploy-prep/data-box-edge-resource2.png)

8. **Oluştur'u**seçin.

Kaynağın oluşturulması birkaç dakika sürer. Kaynak başarıyla oluşturulduktan ve dağıtıldıktan sonra size bildirilir. **Kaynağa Git'i**seçin.

![Veri Kutusu Kenarı kaynağına gitme](media/data-box-edge-deploy-prep/data-box-edge-resource3.png)

Sipariş verildikten sonra, Microsoft siparişi gözden geçirir ve size (e-posta yoluyla) gönderi mat.

![Veri Kutusu Kenarı siparişinin gözden geçirilmesi için bildirim](media/data-box-edge-deploy-prep/data-box-edge-resource4.png)

## <a name="get-the-activation-key"></a>Etkinleştirme anahtarı alma

Data Box Edge kaynağı çalışmaya devam ettikten sonra etkinleştirme anahtarını almanız gerekir. Bu anahtar Data Box Edge cihazınızı etkinleştirmek ve kaynağa bağlamak için kullanılır. Bu anahtarı şimdi, Azure portalındayken alabilirsiniz.

1. Oluşturduğunuz kaynağı seçin. **Genel Bakış'ı** seçin ve ardından **Aygıt kurulumünü**seçin.

    ![Aygıt kurulumünü seçin](media/data-box-edge-deploy-prep/data-box-edge-select-devicesetup.png)

2. **Etkinleştir** döşemesinde etkinleştirme anahtarı oluşturmak için **Oluştur anahtarını** seçin. Anahtarı kopyalamak ve daha sonra kullanmak üzere kaydetmek için kopya simgesini seçin.

    ![Etkinleştirme anahtarını alma](media/data-box-edge-deploy-prep/get-activation-key.png)

> [!IMPORTANT]
> - Etkinleştirme anahtarı oluşturulduktan üç gün sonra sona erer.
> - Anahtarın süresi dolduysa, yeni bir anahtar oluşturun. Eski anahtar geçerli olmaz.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, aşağıdaki Data Box Edge konularını öğrendiniz:

> [!div class="checklist"]
> * Yeni kaynak oluşturma
> * Etkinleştirme anahtarı alma

Data Box Edge'in nasıl yüklenir olduğunu öğrenmek için bir sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Azure Data Box Edge'i kurma](./data-box-edge-deploy-install.md)



