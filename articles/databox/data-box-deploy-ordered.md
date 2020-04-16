---
title: Azure Veri Kutusu siparişi verme öğreticisi | Microsoft Dokümanlar
description: Dağıtım önkoşullarını ve Azure Data Box siparişi etmeyi öğrenin
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: tutorial
ms.date: 04/23/2019
ms.author: alkohli
ms.openlocfilehash: 46dd89694857138d28255d5b1a86a8c947680520
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81398691"
---
# <a name="tutorial-order-azure-data-box"></a>Öğretici: Azure Data Box sipariş etme

Azure Data Box, şirket içi verilerinizi Azure'a hızlı, kolay ve güvenilir bir şekilde aktarmanızı sağlayan bir hibrit çözümdür. Verilerinizi Microsoft tarafından sağlanan 80 TB'lık (kullanılabilir kapasite) depolama cihazına aktarabilir ve cihazı geri gönderebilirsiniz. Bu veriler daha sonra Azure'a yüklenir.

Bu öğreticide Azure Data Box sipariş etme adımları açıklanır. Bu öğreticide şunları öğrenirsiniz:

> [!div class="checklist"]
> * Data Box dağıtımı önkoşulları
> * Data Box sipariş etme
> * Siparişi izleme
> * Siparişi iptal etme

## <a name="prerequisites"></a>Ön koşullar

Cihazı dağıtmadan önce Data Box hizmeti ve cihazı için aşağıdaki yapılandırma önkoşullarını tamamlayın.

### <a name="for-service"></a>Hizmet için

[!INCLUDE [Data Box service prerequisites](../../includes/data-box-supported-subscriptions.md)]

### <a name="for-device"></a>Cihaz için

Başlamadan önce aşağıdakilerden emin olun:
- Veri merkezi ağına bağlı bir konak bilgisayarınız olmalıdır. Data Box verileri bu bilgisayardan kopyalayacaktır. [Azure Data Box sistem gereksinimleri](data-box-system-requirements.md) altında açıklandığı gibi konak bilgisayarınızın desteklenen bir işletim sistemini çalıştırması gerekir.
- Veri merkezinizin yüksek hızlı ağı olmalıdır. En az bir adet 10 GbE bağlantınızın olması önemle tavsiye edilir. 10 GbE bağlantı yoksa 1 GbE veri bağlantısı kullanılabilir; ancak, kopyalama hızı etkilenir.

## <a name="order-data-box"></a>Data Box sipariş etme

Cihaz sipariş etmek için Azure portalında aşağıdaki adımları izleyin.

1. Microsoft Azure kimlik bilgilerini kullanarak şu URL’de oturum açın: [https://portal.azure.com](https://portal.azure.com).
2. **+ Kaynak oluştur**’a tıklayın ve *Azure Data Box* araması yapın. **Azure Data Box**'a tıklayın.
    
   [![Azure Data Box arama 1](media/data-box-deploy-ordered/search-azure-data-box1.png)](media/data-box-deploy-ordered/search-azure-data-box1.png#lightbox)

3. **Oluştur'u**tıklatın.

4. Data Box'ın bölgenizde kullanılabilir olup olmadığını kontrol edin. Aşağıdaki bilgileri girin veya seçin ve sonra **Uygula**'ya tıklayın. 

    |Ayar  |Değer  |
    |---------|---------|
    |Abonelik     | Data Box hizmeti için bir EA, CSP veya Azure sponsorluk aboneliği seçin. <br> Abonelik fatura hesabınıza bağlıdır.       |
    |Aktarım türü     | **Azure’a içeri aktar**’ı seçin.        |
    |Kaynak ülke     |   Verilerinizin bulunduğu ülkeyi/bölgeyi seçin.         |
    |Hedef Azure bölgesi     |     Verileri aktarmak istediğiniz Azure bölgesini seçin.        |

5. Veri **Kutusu'nü**seçin. Tek bir sipariş için maksimum kullanılabilir kapasite 80 TB'dir. Daha büyük veri boyutları için birden fazla sipariş oluşturabilirsiniz.

      [![Veri Kutusu seçeneğini seçin 1](media/data-box-deploy-ordered/select-data-box-option1.png)](media/data-box-deploy-ordered/select-data-box-option1.png#lightbox)

6. **Sipariş** bölümünde **Sipariş ayrıntıları**'nı belirtin. Aşağıdaki bilgileri girin veya seçin ve sonra **İleri**'ye tıklayın.
    
    |Ayar  |Değer  |
    |---------|---------|
    |Adı     |  Siparişi takip etmek için kullanılacak kolay bir ad girin. <br> Ad harf, rakam ve tirelerden oluşan 3-24 karakter arası uzunlukta olabilir. <br> Ad bir harf veya sayıyla başlamalı ve bitmelidir.      |
    |Kaynak grubu     |   Var olan bir taneyi kullanın veya yenisini oluşturun. <br> Kaynak grubu, birlikte yönetilebilen ve ya dağıtılabilen kaynaklardan oluşan mantıksal kapsayıcıdır.         |
    |Hedef Azure bölgesi     | Depolama hesabınız için bir bölge seçin. <br> Daha fazla bilgi için [bölge kullanılabilirliği](data-box-overview.md#region-availability)’ne gidin.        |
    |Depolama hedefi     | Depolama hesabı veya yönetilen diskler ya da her ikisini seçin. <br> Belirtilen Azure bölgesine göre filtrelenen listeden var olan bir veya daha fazla depolama hesabı seçin. Data Box en çok 10 depolama hesabına bağlanabilir. <br> Dilerseniz yeni bir **Genel amaçlı v1**, **Genel amaçlı v2** veya **Blob depolama hesabı** da oluşturabilirsiniz. <br>Sanal ağları olan depolama hesapları desteklenir. Data Box hizmetinin güvenli depolama hesaplarıyla çalışmasına izin vermek için, depolama hesabı ağ güvenlik duvarı ayarları içinden güvenilen hizmetleri etkinleştirin. Daha fazla bilgi için Azure Veri Kutusu'nun güvenilir bir hizmet olarak nasıl [ekleyeceğinize](../storage/common/storage-network-security.md#exceptions)bakın.|

    Depolama hesabı depolama hedefi olarak kullanılıyorsa aşağıdaki ekran görüntüsünü görürsünüz:

    ![Depolama hesabı için Veri Kutusu sırası](media/data-box-deploy-ordered/order-storage-account.png)

    Şirket içi VHD'lerden yönetilen diskler oluşturmak için Veri Kutusu'nu kullanıyorsanız, aşağıdaki bilgileri de sağlamanız gerekir:

    |Ayar  |Değer  |
    |---------|---------|
    |Kaynak grupları     | Şirket içi VHD'lerden yönetilen diskler oluşturmayı düşünüyorsanız yeni kaynak grupları oluşturun. Varolan bir kaynak grubunu yalnızca Veri Kutusu hizmeti tarafından yönetilen disk için veri kutusu sırası oluşturulurken kaynak grubu daha önce oluşturulduysa kullanabilirsiniz. <br> Noktalı virgülle ayrılmış birden çok kaynak grubu belirtin. En fazla 10 kaynak grubu desteklenir.|

    ![Yönetilen disk için Veri Kutusu sırası](media/data-box-deploy-ordered/order-managed-disks.png)

    Yönetilen diskler için belirtilen depolama hesabı, hazırlama depolama hesabı olarak kullanılır. Data Box hizmeti, VHD'leri yönetilen disklere dönüştürmeden ve kaynak gruplarına taşımadan önce hazırlama depolama hesabına sayfa blobları olarak yükler. Daha fazla bilgi için bkz. [Azure'a veri yüklemeyi doğrulama](data-box-deploy-picked-up.md#verify-data-upload-to-azure).

7. **Teslimat adresi**’ne adınızı, soyadınızı, şirket adını, posta adresini ve geçerli bir telefon numarasını girin. **Adresi doğrula**'ya tıklayın. Hizmet, teslimat adresinde hizmetin kullanılabilirlik durumunu doğrular. Hizmet belirtilen teslimat adresinde kullanılabilir durumdaysa bu konuda bir bildirim gönderilir. **İleri**’ye tıklayın.

8. **Bildirim ayrıntıları** sayfasında e-posta adresi belirtin. Hizmet, belirtilen e-posta adreslerine sipariş durumundaki güncelleştirmelerle ilgili bilgi gönderir.

    Grup yöneticisinin ayrılması durumunda da bildirim almaya devam etmek için bir grup e-postası kullanmanız önerilir.

9. **Özet** sayfasında sipariş, iletişim, bildirim ve gizlilik koşullarıyla ilgili bilgileri gözden geçirin. Gizlilik koşullarını kabul ettiğinizi belirten kutuyu işaretleyin.

10. **Sipariş**'e tıklayın. Siparişin oluşturulması birkaç dakika sürer.


## <a name="track-the-order"></a>Siparişi izleme

Siparişi verdikten sonra durumunu Azure portalından takip edebilirsiniz. Veri Kutusu siparişinize gidin ve durumu görüntülemek için **Genel Bakış'a** gidin. Portalda siparişin durumu **Sipariş edildi** olarak görünür.

Cihaz yoksa bir bildirim alırsınız. Cihaz varsa, Microsoft gönderilecek cihazı belirler ve gönderimi hazırlar. Cihaz hazırlanırken şu eylemler gerçekleştirilir:

- Cihazla ilişkili her depolama hesabı için SMB paylaşımları oluşturulur.
- Her paylaşım için, kullanıcı adı veya parola gibi erişim kimlik bilgileri oluşturulur.
- Cihaz kilidinin açılmasına yardımcı olan cihaz parolası da oluşturulur.
- Data Box, herhangi bir noktada cihaza yetkisiz erişimi önlemek için kilitlenmiştir.

Cihazın hazırlanması tamamlandığında portalda sipariş durumu **İşlendi** olarak değişir.

![Data Box siparişi işlendi](media/data-box-overview/data-box-order-status-processed.png)

Microsoft ardından cihazınızı hazırlar ve bölgeye uygun gönderim şirketine teslim eder. Cihaz gönderildikten sonra bir takip numarası iletilir. Portalda sipariş durumu **Yola çıktı** olarak değişir.

![Data Box siparişi yola çıktı](media/data-box-overview/data-box-order-status-dispatched.png)

## <a name="cancel-the-order"></a>Siparişi iptal etme

Bu siparişi iptal etmek için Azure portalında **Genel bakış**'a gidin ve komut çubuğundan **İptal**'e tıklayın.

Sipariş verdikten sonra, sipariş durumu işlendi olmadan önce herhangi bir noktada siparişi iptal edebilirsiniz.
 
İptal edilen bir siparişi silmek için **Genel bakış**'a gidin ve komut çubuğundan **Sil**'e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide aşağıdaki Azure Data Box konularını öğrendiniz:

> [!div class="checklist"]
> * Data Box dağıtımı önkoşulları
> * Data Box sipariş etme
> * Siparişi izleme
> * Siparişi iptal etme

Data Box’ınızı ayarlama hakkında bilgi edinmek için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Azure Data Box’ınızı ayarlama](./data-box-deploy-set-up.md)


