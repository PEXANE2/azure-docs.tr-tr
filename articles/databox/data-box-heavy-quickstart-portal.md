---
title: Microsoft Azure Data Box Heavy için hızlı başlangıç | Microsoft Docs
description: Azure Data Box Heavy Azure portal hızlı bir şekilde dağıtmayı öğrenin
services: databox
author: alkohli
ms.service: databox
ms.subservice: heavy
ms.topic: quickstart
ms.date: 08/29/2019
ms.author: alkohli
ms.openlocfilehash: 433c3ac08d8d7d8158d4cc958979782e3b3192e1
ms.sourcegitcommit: 19a821fc95da830437873d9d8e6626ffc5e0e9d6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2019
ms.locfileid: "70164431"
---
::: zone target = "docs"

# <a name="quickstart-deploy-azure-data-box-heavy-using-the-azure-portal"></a>Hızlı Başlangıç: Azure portal kullanarak Azure Data Box Heavy dağıtma

Bu hızlı başlangıçta Azure Data Box Heavy Azure portal kullanılarak nasıl dağıtılacağı açıklanmaktadır. Bu adımlar, Azure 'a karşıya yüklemek üzere Data Box Heavy verilerin nasıl kablo, yapılandırma ve kopyalama işlemlerini içerir. Hızlı başlangıç Azure portalında ve cihazın yerel web kullanıcı arabiriminde gerçekleştirilir.

Ayrıntılı adım adım dağıtım ve izleme yönergeleri için [Öğreticiye gidin: Sipariş Azure Data Box Heavy](data-box-heavy-deploy-ordered.md)

## <a name="prerequisites"></a>Önkoşullar

Cihazı dağıtmadan önce yükleme sitesi, Data Box hizmeti ve cihaz için aşağıdaki yapılandırma önkoşullarını doldurun.

### <a name="for-installation-site"></a>Yükleme sitesi için

Başlamadan önce aşağıdakilerden emin olun:

- Cihaz tüm entrysize uygun olabilir. Cihaz boyutları: width: 26 "uzunluğu: 48 "Yükseklik: 28 ".
- Taban katından başka bir kata yüklemeyi planlıyorsanız bir Asansör veya bir rampa aracılığıyla cihaza erişiminiz vardır.
- Cihazı işlemek için iki kişi vardır. Cihaz yaklaşık olarak% 500 lbs. ve, tekerlekler üzerine gelir.
- Veri merkezinde, bu parmak izine sahip bir cihaza uyum sağlayan kullanılabilir bir ağ bağlantısına yakınlık içeren bir düz siteniz var.

### <a name="for-service"></a>Hizmet için

Başlamadan önce aşağıdakilerden emin olun:

- Erişim kimlik bilgilerine sahip bir Microsoft Azure Storage hesabınız var.
- Data Box hizmeti için kullandığınız abonelik [Microsoft kurumsal anlaşma (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/), [bulut çözümü sağlayıcısı (CSP)](https://docs.microsoft.com/azure/cloud-solution-provider/overview/azure-csp-overview)veya [Microsoft Azure sponsorluğu](https://azure.microsoft.com/offers/ms-azr-0036p/).
- Data Box Heavy bir sıra oluşturmak için aboneliğe sahip veya katkıda bulunan erişiminiz var.

### <a name="for-device"></a>Cihaz için

Başlamadan önce aşağıdakilerden emin olun:

- [Data Box Heavy için güvenlik yönergelerini](data-box-safety.md)gözden geçirdiniz.
- Veri merkezi ağına bağlı bir ana bilgisayar var. Data Box Heavy, verileri bu bilgisayardan kopyalayacak. Ana bilgisayarınızın [desteklenen bir işletim sistemi](data-box-heavy-system-requirements.md)çalıştırması gerekir.
- Yerel Kullanıcı arabirimine bağlanmak ve cihazı yapılandırmak için RJ-45 kablosu olan bir dizüstü bilgisayarınız var. Cihazın her düğümünü bir kez yapılandırmak için dizüstü bilgisayarı kullanın.
- Veri merkezinizde yüksek hızlı bir ağ vardır ve en az 1 10 GbE bağlantınız vardır.
- Cihaz düğümü başına 1 40 Gbps kablo veya 10 Gbps kablo gerekir. Mellanox MCX314A-BCCT ağ arabirimiyle uyumlu kabloları seçin:
    - 40 Gbps kablo için, kablonun cihaz sonunda QSFP + olması gerekir.
    - 10 GB/sn kablo için, cihaza takılan uçta bir düzey + ila SFP + bağdaştırıcısı (veya QSA bağdaştırıcısı) ile bir uçta 10-G anahtarına takılan bir SFP + kablonuz olması gerekir.
- Güç kabloları, cihazın arkasında bir tepsiye bulunur.


## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="order"></a>Sipariş verme

Bu adım yaklaşık 5 dakika sürer.

1. Azure portalında yeni bir Azure Data Box kaynağı oluşturun.
2. Bu hizmetin etkinleştirildiği mevcut aboneliği seçin ve aktarım türünü **İçeri aktarma** olarak belirleyin. Verilerin bulunduğu **Kaynak ülkeyi** ve veri aktarımı için **Azure hedef bölgesini** seçin.
3. **Data Box Heavy**seçin. Kullanılabilir en yüksek kapasite 770 TB 'tır ve daha büyük veri boyutları için birden çok sipariş oluşturabilirsiniz.
4. Sipariş ayrıntılarını ve sevkiyat bilgilerini girin. Hizmet bölgenizde kullanılabilir durumdaysa bildirim e-posta adreslerini girin, özeti gözden geçirin ve siparişi oluşturun.

Sipariş oluşturulduktan sonra cihaz gönderilmek üzere hazırlanır.

::: zone-end

::: zone target = "chromeless"

# <a name="cable-and-connect-to-your-device"></a>Kablo ve cihazınıza bağlanma

Önkoşulları inceledikten sonra cihazınıza kabloyla bağlanır ve yöneticinize bağlanırsınız.

::: zone-end

## <a name="cable-for-power"></a>Güç kablosu

Bu adım yaklaşık 5 dakika sürer.

Data Box Heavy aldığınızda, cihazı güç için kablosunu ve cihazı açmak için aşağıdaki adımları uygulayın.

1. Cihazla oynandığına veya cihazın hasarlı olduğuna ilişkin bir kanıt varsa, devam etmeyin. Size yeni bir cihaz gönderilmesi için Microsoft Desteği'ne başvurun.
2. Cihazı yükleme sitesine taşıyın ve arka tekerlekleri kilitleyin.
3. Dört güç kablosu tamamını cihazın arka kısmındaki güç tedariklerini bağlayın.
4. Cihaz düğümlerini açmak için ön düzlemdeki güç düğmelerini kullanın.

## <a name="cable-first-node-for-network"></a>Ağ için kablolu ilk düğüm

Bu adım yaklaşık 10-15 dakika sürer.

1. Konak bilgisayarınızı cihazdaki yönetim bağlantı noktasına (MGMT) bağlamak için bir RJ 45 CAT 6 ağ kablosu kullanın.
2. Twinax QSFP + bakır kablosunu kullanarak, veriler için en az 1 40 Gbps (1 Gbps üzerinden tercih edilen) ağ arabirimi, VERI 1 veya VERI 2 ' ye bağlanabilirsiniz. 10 Gbps bir anahtar kullanıyorsanız, veri için 40-10.000 ağ arabirimine bağlanmak üzere bir QSFP + ila SFP + bağdaştırıcısı (QSA bağdaştırıcısı) ile Twinax SFP + bakır kablo kullanın.
3. Cihazın kablolarını aşağıda gösterildiği gibi takın.  

    ![Data Box Heavy kablolu](media/data-box-heavy-quickstart-portal/data-box-heavy-ports-cabled.png)  

## <a name="configure-first-node"></a>İlk düğümü Yapılandır

Bu adımın tamamlanması yaklaşık 5-7 dakika sürer.

1. Cihaz parolasını almak için [Azure portalında](https://portal.azure.com) **Genel > Cihaz ayrıntıları**'na gidin. Aynı parola, cihazın her iki düğümü için de kullanılır.
2. Data Box Heavy bağlanmak için kullandığınız bilgisayardaki Ethernet bağdaştırıcısına 192.168.100.5 ve subnet 255.255.255.0 statik IP adresini atayın. Cihazın yerel web kullanıcı arabirimine `https://192.168.100.10` adresinden erişin. Siz cihazı açtıktan sonra bağlantı kurulması 5 dakika kadar sürebilir.
3. Azure portalından alınan parolayı kullanarak oturum açın. Web sitesinin güvenlik sertifikasında sorun olduğunu belirten bir hata görürsünüz. Web sayfasına ilerlemek için tarayıcıya özel yönergeleri izleyin.
4. Varsayılan olarak, arabirimler için ağ ayarları (MGMT hariç) DHCP olarak yapılandırılır. Gerekirse, bu arabirimleri statik olarak yapılandırabilir ve bir IP adresi sağlayabilirsiniz.

## <a name="cable-and-configure-the-second-node"></a>Kablo ve ikinci düğümü yapılandırma

Bu adım yaklaşık 15-20 dakika sürer.

İlk düğüm için kullanılan adımları uygulayın ve cihazdaki ikinci düğümü yapılandırın.  


::: zone target = "docs"

## <a name="copy-data"></a>Verileri kopyalama

Bu işlemi gerçekleştirmek için gereken süre, veri boyutınıza ve verilerin kopyalandığı ağın hızına bağlıdır.
 
1. Hem 40 GB/sn veri arabirimlerini paralel olarak kullanarak hem cihaz düğümlerine hem de verileri kopyalayın.

    - Bir Windows ana bilgisayarı kullanıyorsanız [Robocopy](https://technet.microsoft.com/library/ee851678.aspx)gıbı bir SMB uyumlu dosya kopyalama aracı kullanın.
    - NFS konağı için, `cp` veya `rsync` komutunu kullanarak verileri kopyalayın.
2. Şu yolu kullanarak cihazdaki paylaşımlara bağlanın:`\\<IP address of your device>\ShareName`. Paylaşma erişimi kimlik bilgilerini almak için Data Box Heavy yerel Web Kullanıcı arabirimindeki **& Kopyala** sayfasına gidin.
3. Paylaşımın ve klasör adlarının ve verilerin [Azure depolama ve Data Box Heavy hizmet sınırlarına](data-box-heavy-limits.md)ilişkin yönergeleri izlediğinden emin olun.

## <a name="prepare-to-ship"></a>Göndermeye Hazırlama

Bu işlemi tamamlamak için gereken süre verilerinizin boyutuna göre değişir.

1. Veri kopyalama işlemi herhangi bir hata olmadan tamamlandıktan sonra, yerel Web Kullanıcı arabirimindeki **göndermeye hazırlama** sayfasına gidin ve sevk hazırlığı ' ni başlatın.
2. Her iki düğümde de **göndermeye hazırlama** başarıyla tamamlandıktan sonra, cihazı yerel Web kullanıcı arabiriminden kapatın.

## <a name="ship-to-azure"></a>Azure'a gönderme

Bu işlemin tamamlanabilmesi yaklaşık 15-20 dakika sürer.

1. Kabloları kaldırın ve cihazın arkasında bulunan tepsiye geri döndürün.
2. Bölge taşıyıcınızın bir çekme zamanlaması yapın.
3. Toplama ile ilgili bilgi almak ve iade sevkiyat etiketini almak için [Data Box işlemlerine](mailto:DataBoxOps@microsoft.com) ulaşın.
4. İade sevkiyat etiketi, cihazın ön temizleme panelinde görünür olmalıdır.

## <a name="verify-data"></a>Verileri doğrulama

Bu işlemi tamamlamak için gereken süre verilerinizin boyutuna göre değişir.

1. Data Box Heavy cihaz Azure veri merkezi ağına bağlıyken, veriler Azure 'a otomatik olarak yüklenir.
2. Data Box hizmeti, veri kopyasının Azure portal aracılığıyla tamamlandığını bildirir.

    1. Hata günlüklerini kontrol ederek hata olup olmadığını kontrol edin ve gerekli eylemleri gerçekleştirin.
    2. Kaynaktan silmeden önce verilerinizin depolama hesaplarında olduğundan emin olun.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu adımın tamamlanması 2-3 dakika sürer.

- Sipariş işlenmeden önce Azure portal Data Box Heavy sırayı iptal edebilirsiniz. Siparişler işleme alındıktan sonra iptal edilemez. Sipariş, tamamlanma aşamasına gelene kadar ilerler. Siparişi iptal etmek için **Genel bakış**'a gidin ve komut çubuğundan **İptal**'e tıklayın.

- Siparişin durumu **Tamamlandı** veya **İptal edildi** olduğunda Azure portaldan silebilirsiniz. Siparişi silmek için **Genel bakış**'a gidin ve komut çubuğundan **Sil**'e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, verilerinizi Azure 'a aktarmaya yardımcı olmak için bir Data Box Heavy dağıttınız. Azure Data Box Heavy yönetimi hakkında daha fazla bilgi edinmek için aşağıdaki öğreticiye ilerleyin:

> [!div class="nextstepaction"]
> [Yönetmek için Azure portal kullanın Data Box Heavy](data-box-portal-admin.md)

::: zone-end
