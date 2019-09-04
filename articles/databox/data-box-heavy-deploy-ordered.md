---
title: Sipariş Azure Data Box Heavy öğretici | Microsoft Docs
description: Dağıtım önkoşullarını ve bir Azure Data Box Heavy nasıl sipariş alınacağını öğrenin
services: databox
author: alkohli
ms.service: databox
ms.subservice: heavy
ms.topic: tutorial
ms.date: 07/03/2019
ms.author: alkohli
ms.localizationpriority: high
ms.openlocfilehash: dce3549bde4c62245e1d2b1f8ac0c88c0b70260c
ms.sourcegitcommit: 6794fb51b58d2a7eb6475c9456d55eb1267f8d40
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70241399"
---
# <a name="tutorial-order-azure-data-box-heavy"></a>Öğretici: Sipariş Azure Data Box Heavy


Azure Data Box Heavy, şirket içi verilerinizi hızlı, kolay ve güvenilir bir şekilde Azure 'a aktarmanıza olanak sağlayan bir karma çözümdür. Verilerinizi Microsoft tarafından sağlanan bir 770 TB (yaklaşık kullanılabilir kapasite) depolama cihazına aktarır ve sonra cihazı geri sevk edersiniz. Bu veriler daha sonra Azure'a yüklenir.

Bu öğretici, bir Azure Data Box Heavy nasıl sipariş kullanabileceğinizi açıklar. Bu öğreticide şunları öğrenirsiniz:

> [!div class="checklist"]
> * Data Box Heavy önkoşulları
> * Data Box Heavy sıralama
> * Siparişi izleme
> * Siparişi iptal etme

## <a name="prerequisites"></a>Önkoşullar

Cihazı dağıtmadan önce Data Box hizmeti ve cihazı için aşağıdaki yapılandırma önkoşullarını tamamlayın.

### <a name="for-installation-site"></a>Yükleme sitesi için

Başlamadan önce aşağıdakilerden emin olun:

- Cihaz standart doorways ve entryyöntemlerle uyar. Ancak, cihazın tüm entrysize uygun olduğundan emin olun. Cihaz boyutları: width: 26 "uzunluğu: 48 "Yükseklik: 28 ".
- Taban katından başka bir tabanda yüklüyse, cihaza bir Asansör veya bir rampa üzerinden erişmeniz gerekir. Cihaz yaklaşık olarak% 500 lbs.
- Veri merkezinde, bu parmak izine sahip bir cihaza uyum sağlayan kullanılabilir bir ağ bağlantısına yakınlık taşıyan bir düz siteniz olduğundan emin olun.


### <a name="for-service"></a>Hizmet için

Başlamadan önce aşağıdakilerden emin olun:
- Erişim kimlik bilgilerine sahip bir Microsoft Azure Storage hesabınız var.
- Data Box hizmeti için kullandığınız aboneliğin aşağıdaki türlerden birinde olduğundan emin olun:
    - Microsoft Kurumsal Anlaşma (EA). [EA abonelikleri](https://azure.microsoft.com/pricing/enterprise-agreement/) hakkındaki yazıları okuyun.
    - Bulut Çözümü Sağlayıcısı (CSP). [Azure CSP programı](https://docs.microsoft.com/azure/cloud-solution-provider/overview/azure-csp-overview) hakkında daha fazla bilgi edinin.
    - Microsoft Azure Sponsorluğu. [Azure sponsorluğu programı](https://azure.microsoft.com/offers/ms-azr-0036p/) hakkında daha fazla bilgi edinin.

- Data Box Heavy bir sıra oluşturmak için aboneliğe sahip veya katkıda bulunan erişiminizin olduğundan emin olun.

### <a name="for-device"></a>Cihaz için

Başlamadan önce aşağıdakilerden emin olun:
- Cihazınız paketten yok.
- Veri merkezi ağına bağlı bir konak bilgisayarınız olmalıdır. Data Box Heavy, verileri bu bilgisayardan kopyalayacak. Ana bilgisayarınız, [Azure Data Box Heavy sistem gereksinimleri](data-box-system-requirements.md)bölümünde açıklandığı gibi desteklenen bir işletim sistemi çalıştırmalıdır.
- Yerel Kullanıcı arabirimine bağlanmak ve cihazı yapılandırmak için, RJ-45 kablosu olan bir dizüstü bilgisayarınız olması gerekir. Cihazın her düğümünü bir kez yapılandırmak için dizüstü bilgisayarı kullanın.
- Veri merkezinizin yüksek hızlı ağı olmalıdır. En az bir adet 10 GbE bağlantınızın olması önemle tavsiye edilir.
- Cihaz düğümü başına 1 40 Gbps veya 10 Gbps kablo gerekir. [MELLANOX MCX314A-BCCT](https://store.mellanox.com/products/mellanox-mcx314a-bcct-connectx-3-pro-en-network-interface-card-40-56gbe-dual-port-qsfp-pcie3-0-x8-8gt-s-rohs-r6.html) ağ arabirimiyle uyumlu kabloları seçin:

    - 40 Gbps kablosu için, kablonun cihaz sonunda QSFP + olması gerekir.
    - 10 Gbps kablo için, cihaza takılan uçta bir adet, bir uçta 10 G anahtarına bağlanan bir SFP + kablonuz olması gerekir. Bu bir uçta bir, QSFP + ila SFP + bağdaştırıcısı (veya QSA bağdaştırıcısı).
    - Güç kabloları cihaza dahildir.

## <a name="order-data-box-heavy"></a>Sipariş Data Box Heavy

Cihaz sipariş etmek için Azure portalında aşağıdaki adımları izleyin.

1. Microsoft Azure kimlik bilgilerini kullanarak şu URL’de oturum açın: [https://portal.azure.com](https://portal.azure.com).
2. **+ Kaynak oluştur** ' u seçin ve *Azure Data Box*arayın. **Azure Data Box**seçin.
    
   [![Azure Data Box arama 1](media/data-box-deploy-ordered/search-azure-data-box1.png)](media/data-box-deploy-ordered/search-azure-data-box1.png#lightbox)

3. **Oluştur**’u seçin.

4. Data Box hizmetinin bölgenizde kullanılabilir olup olmadığını denetleyin. Aşağıdaki bilgileri girin veya seçin ve **Uygula**' yı seçin.

    |Ayar  |Value  |
    |---------|---------|
    |Subscription     | Data Box hizmeti için bir EA, CSP veya Azure Sponsorluk aboneliği seçin. <br> Abonelik fatura hesabınıza bağlıdır.       |
    |Aktarım türü     | **Azure’a içeri aktar**’ı seçin.        |
    |Kaynak ülke     | Verilerinizin Şu anda bulunduğu ülkeyi/bölgeyi seçin.         |
    |Hedef Azure bölgesi     | Verileri aktarmak istediğiniz Azure bölgesini seçin.        |

    [![Data Box aile kullanılabilirliğini seçin](media/data-box-deploy-ordered/select-data-box-option1.png)](media/data-box-deploy-ordered/select-data-box-option1.png#lightbox)

5. **Data Box Heavy**seçin. Tek bir sipariş için kullanılabilir maksimum kapasite 770 TB 'tır.

    [![Data Box Heavy seçin](media/data-box-heavy-deploy-ordered/select-data-box-heavy.png)

6. **Sipariş** bölümünde **Sipariş ayrıntıları**'nı belirtin. Aşağıdaki bilgileri girin veya seçin ve Ileri ' **yi**seçin.
    
    |Ayar  |Value  |
    |---------|---------|
    |Name     | Siparişi takip etmek için kullanılacak kolay bir ad girin. <br> Ad harf, rakam ve tirelerden oluşan 3-24 karakter arası uzunlukta olabilir. <br> Ad bir harf veya sayıyla başlamalı ve bitmelidir.      |
    |Resource group     | Var olan bir taneyi kullanın veya yenisini oluşturun. <br> Kaynak grubu, birlikte yönetilebilen ve ya dağıtılabilen kaynaklardan oluşan mantıksal kapsayıcıdır.         |
    |Hedef Azure bölgesi     | Depolama hesabınız için bir bölge seçin. <br> Daha fazla bilgi için [bölge kullanılabilirliği](https://azure.microsoft.com/global-infrastructure/services/?products=databox)’ne gidin.        |
    |Depolama hedefi     | Depolama hesabı veya yönetilen diskler veya her ikisi arasından birini seçin. <br> Belirtilen Azure bölgesine göre filtrelenen listeden var olan bir veya daha fazla depolama hesabı seçin. <br>Data Box Heavy, en fazla 10 depolama hesabıyla bağlanabilir. <br> Ayrıca, yeni bir **genel amaçlı v1**, **genel amaçlı v2**veya **BLOB depolama hesabı**da oluşturabilirsiniz. <br> Azure Data Lake Storage Gen 2 hesapları desteklenmez. [Cihazlarınızla desteklenen depolama hesaplarına](data-box-heavy-system-requirements.md#supported-storage-accounts)bakın. <br>Sanal ağları olan depolama hesapları desteklenir. Data Box hizmetinin güvenli depolama hesaplarıyla çalışmasına izin vermek için, depolama hesabı ağ güvenlik duvarı ayarları içindeki Güvenilen hizmetleri etkinleştirin. Daha fazla bilgi için bkz. [Azure Data Box hizmetini Güvenilen hizmet olarak ekleme](../storage/common/storage-network-security.md#exceptions).|

    Depolama hesabı depolama hedefi olarak kullanılıyorsa aşağıdaki ekran görüntüsünü görürsünüz:

    ![Depolama hesabı için Data Box Heavy sıralaması](media/data-box-heavy-deploy-ordered/order-storage-account.png)

    Depolama hesabına depolama hedefi olarak ek olarak, şirket içi VHD 'lerden yönetilen diskler oluşturmak için de Data Box Heavy kullanıyorsanız, aşağıdaki bilgileri sağlamanız gerekir:

    |Ayar  |Value  |
    |---------|---------|
    |Kaynak grupları     | Şirket içi VHD 'lerden yönetilen diskler oluşturmayı düşünüyorsanız yeni kaynak grupları oluşturun. Mevcut bir kaynak grubunu yalnızca kaynak grubu, Data Box hizmeti tarafından yönetilen disk için bir Data Box Heavy sırası oluşturulurken daha önce oluşturulduysa kullanabilirsiniz. <br> Noktalı virgülle ayrılmış birden çok kaynak grubu belirtin. En fazla 10 kaynak grubu desteklenir.|

    ![Yönetilen disk için Data Box Heavy sıralaması](media/data-box-heavy-deploy-ordered/order-managed-disks.png)

    Yönetilen diskler için belirtilen depolama hesabı, hazırlama depolama hesabı olarak kullanılır. Data Box hizmeti, VHD 'leri yönetilen disklere dönüştürmeden ve kaynak gruplarına taşıyarak hazırlama depolama hesabına sayfa Blobları olarak yükler. Daha fazla bilgi için bkz. [Azure 'a veri yüklemeyi doğrulama](data-box-deploy-picked-up.md#verify-data-upload-to-azure).

7. **Teslimat adresi**’ne adınızı, soyadınızı, şirket adını, posta adresini ve geçerli bir telefon numarasını girin. **Adresi doğrula**' yı seçin. 

    Hizmet, teslimat adresinde hizmetin kullanılabilirlik durumunu doğrular. Hizmet belirtilen teslimat adresinde kullanılabilir durumdaysa bu konuda bir bildirim gönderilir. **İleri**’yi seçin.

8. **Bildirim ayrıntıları** sayfasında e-posta adresi belirtin. Hizmet, belirtilen e-posta adreslerine sipariş durumundaki güncelleştirmelerle ilgili bilgi gönderir.

    Grup yöneticisinin ayrılması durumunda da bildirim almaya devam etmek için bir grup e-postası kullanmanız önerilir.

9. **Özet** sayfasında sipariş, iletişim, bildirim ve gizlilik koşullarıyla ilgili bilgileri gözden geçirin. Gizlilik koşullarını kabul ettiğinizi belirten kutuyu işaretleyin.

10. **Sipariş**' i seçin. Siparişin oluşturulması birkaç dakika sürer.


## <a name="track-the-order"></a>Siparişi izleme

Siparişi verdikten sonra durumunu Azure portalından takip edebilirsiniz. Data Box Heavy siparişiniz ' ne gidin ve durumu görüntülemek için **genel bakış** ' a gidin. Portalda siparişin durumu **Sipariş edildi** olarak görünür.

Cihaz yoksa bir bildirim alırsınız. Cihaz varsa, Microsoft gönderilecek cihazı belirler ve gönderimi hazırlar. Cihaz hazırlanırken şu eylemler gerçekleştirilir:

- Cihazla ilişkili her depolama hesabı için SMB paylaşımları oluşturulur.
- Her paylaşım için, kullanıcı adı veya parola gibi erişim kimlik bilgileri oluşturulur.
- Cihaz kilidinin açılmasına yardımcı olan cihaz parolası da oluşturulur.
- Data Box Heavy, cihaza herhangi bir noktada yetkisiz erişimi engellemek için kilitlenir.

Cihazın hazırlanması tamamlandığında portalda sipariş durumu **İşlendi** olarak değişir.

![Data Box Heavy sırası işlendi](media/data-box-overview/data-box-order-status-processed.png)

Microsoft ardından cihazınızı hazırlar ve bölgeye uygun gönderim şirketine teslim eder. Cihaz gönderildikten sonra bir takip numarası iletilir. Portalda sipariş durumu **Yola çıktı** olarak değişir.

![Data Box Heavy sipariş gönderildi](media/data-box-overview/data-box-order-status-dispatched.png)

## <a name="cancel-the-order"></a>Siparişi iptal etme

Bu siparişi iptal etmek için Azure portalında **Genel bakış**'a gidin ve komut çubuğundan **İptal**'e tıklayın.

Sipariş verdikten sonra, sipariş durumu işlendi olmadan önce herhangi bir noktada siparişi iptal edebilirsiniz.
 
İptal edilen bir siparişi silmek için **Genel bakış**'a gidin ve komut çubuğundan **Sil**'e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şöyle Azure Data Box Heavy konular hakkında bilgi edindiniz:

> [!div class="checklist"]
> * Önkoşullar
> * Sipariş Data Box Heavy
> * Siparişi izleme
> * Siparişi iptal etme

Data Box Heavy ayarlamayı öğrenmek için bir sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Azure Data Box Heavy ayarlama](./data-box-heavy-deploy-set-up.md)
