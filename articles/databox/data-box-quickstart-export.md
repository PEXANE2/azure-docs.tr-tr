---
title: 'Hızlı Başlangıç: Microsoft Azure Data Box için verileri dışarı aktarma'
description: Azure portalda Azure Data Box verilerinizi hızla dışarı aktarmayı öğrenin
services: databox
author: priestlg
ms.service: databox
ms.subservice: pod
ms.topic: quickstart
ms.date: 07/17/2020
ms.author: v-grpr
ms.localizationpriority: high
ms.openlocfilehash: fbe14151845d19dd1d0f2a4069afb2badb125077
ms.sourcegitcommit: b8702065338fc1ed81bfed082650b5b58234a702
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88122258"
---
# <a name="quickstart-get-started-with-azure-data-box-to-export-data-from-azure"></a>Hızlı Başlangıç: Azure’dan verileri dışarı aktarmak için Azure Data Box kullanmaya başlama

Bu hızlı başlangıçta Azure portalı kullanarak Azure'daki verileri kendi konumunuza aktarma işlemi açıklanır. Bu, Azure'dan verileri dışarı aktarma işleminin kablo bağlantılarını yapma, işlemi yapılandırma ve verileri kopyalama adımlarından oluşur. Hızlı başlangıç Azure portalında ve cihazın yerel web kullanıcı arabiriminde gerçekleştirilir.

Ayrıntılı adım adım dağıtım ve izleme yönergeleri için, [Öğretici: Azure Data Box için dışarı aktarma siparişi oluşturma](data-box-deploy-export-ordered.md)

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce:

* Data Box hizmeti için kullandığınız aboneliğin aşağıdaki türlerden birinde olduğundan emin olun:
  * Microsoft Kurumsal Anlaşma (EA). [EA abonelikleri](https://azure.microsoft.com/pricing/enterprise-agreement/) hakkındaki yazıları okuyun.
  * Bulut Çözümü Sağlayıcısı (CSP). [Azure CSP programı](https://docs.microsoft.com/azure/cloud-solution-provider/overview/azure-csp-overview) hakkında daha fazla bilgi edinin.
  * Microsoft Azure Sponsorluğu. [Azure sponsorluğu programı](https://azure.microsoft.com/offers/ms-azr-0036p/) hakkında daha fazla bilgi edinin.

* Data Box siparişi oluşturmak için, abonelik üzerinde sahip veya katkıda bulunan erişimine sahip olduğunuzdan emin olun.
* [Data Box'ınızın güvenlik yönergelerini](data-box-safety.md) gözden geçirin.
* Verileri kendi konumunuza taşımak için bir Azure Data Box cihazınız vardır. Ana bilgisayarınız:
  * [Desteklenen bir işletim sistemi](data-box-system-requirements.md) çalıştırılmalıdır.
  * Yüksek hızlı ağa bağlısınız. En az bir adet 10 GbE bağlantınızın olması önemle tavsiye edilir. 10 GbE bağlantı yoksa, 1 GbE veri bağlantısı kullanılabilir ancak kopyalama hızı etkilenir.
* Data Box’ı yerleştirebileceğiniz düz bir yüzeye erişiminiz olmalıdır. Cihazı standart bir rafa yerleştirmek istiyorsanız, veri merkezi rafınızda bir 7U yuvası olmalıdır. Cihazı düz veya dik şekilde rafa yerleştirebilirsiniz.
* Data Box'ınızı ana bilgisayara bağlamak için aşağıdaki kabloları temin ettiniz.
  * İki adet 10 GbE SFP+ Twinax bakır kablo (DATA 1, DATA 2 ağ arabirimleri ile kullanın)
  * Bir RJ-45 CAT 6 ağ kablosu (yönetim (MGMT) ağ arabirimi ile kullanın)
  * Bir RJ-45 CAT 6A VEYA bir RJ-45 CAT 6 ağ kablosu (sırasıyla 10 Gb/sn veya 1 Gb/sn olarak yapılandırılmış DATA 3 ağ arabirimi ile birlikte kullanın)

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="order"></a>Sipariş verme

Bu adım yaklaşık 10 dakika sürer.

1. Azure portalında yeni bir Azure Data Box kaynağı oluşturun.
2. Bu hizmetin etkinleştirildiği mevcut aboneliği seçin ve aktarım türünü **Azure'a Aktarma** olarak belirleyin. Verilerin bulunduğu **Kaynak Azure bölgesini** ve veri aktarımı için **Hedef ülkeyi** seçin.
3. **Data Box**'ı seçin. Maksimum kullanılabilir kapasite 80 TB'tır ve daha büyük veri boyutları için birden çok sipariş oluşturabilirsiniz.
4. **Depolama hesabı ve dışarı aktarma türü ekle**'yi seçin ve sonra da **Dışarı Aktar** seçeneğini belirtin.
5. Sipariş ayrıntılarını ve sevkiyat bilgilerini girin. Hizmet bölgenizde kullanılabilir durumdaysa bildirim e-posta adreslerini girin, özeti gözden geçirin ve siparişi oluşturun.

Sipariş oluşturulduktan sonra cihaz gönderilmek üzere hazırlanır.

## <a name="cable"></a>Kablo

Bu adım yaklaşık 10 dakika sürer.

Data Box elinize ulaştığında cihazı kablolamak, bağlamak ve açmak için aşağıdaki adımları izleyin. Bu adım yaklaşık 10 dakika sürer.

1. Cihazla oynandığına veya cihazın hasarlı olduğuna ilişkin bir kanıt varsa devam etmeyin. Size yeni bir cihaz gönderilmesi için Microsoft Desteği'ne başvurun.
2. Cihazınızın kablolarını takmadan önce aşağıdaki kablolara sahip olduğunuzdan emin olun:

   * Cihaza bağlamak için bir ucunda IEC60320 C-13 bağlayıcısı bulunan, en az 10 A gücünde bir topraklı güç kablosu (pakete dahildir).
   * Bir RJ-45 CAT 6 ağ kablosu (yönetim (MGMT) ağ arabirimi ile kullanın)
   * İki adet 10 GbE SFP+ Twinax bakır kablo (10 Gb/sn DATA 1, DATA 2 ağ arabirimleri ile kullanın)
   * Bir RJ-45 CAT 6A VEYA bir RJ-45 CAT 6 ağ kablosu (sırasıyla 10 Gb/sn veya 1 Gb/sn olarak yapılandırılmış DATA 3 ağ arabirimi ile birlikte kullanın)

3. Cihazı kaldırın ve düz bir yüzeye yerleştirin.

4. Cihazın kablolarını aşağıda gösterildiği gibi takın.  

    ![Kablo bağlantısı yapılmış Data Box cihaz devre kartı](media/data-box-deploy-set-up/data-box-cabled-dhcp.png)  

    1. Güç kablosunu cihaza bağlayın.
    2. Konak bilgisayarınızı cihazdaki MGMT bağlantı noktasına bağlamak için bir RJ 45 CAT 6 ağ kablosu kullanın.
    3. Veriler için en az 10 Gb/sn'lik (1 Gb/sn'ye tercih edilir) ağ arabirimini, DATA 1 veya DATA 2'yi bağlamak üzere bir SFP + Twinax bakır kablo kullanın.
    4. Cihazı açın. Güç düğmesi cihazın ön panelindedir.

## <a name="connect"></a>Bağlan

Bu adımın tamamlanması yaklaşık 5-7 dakika sürer.

1. Cihazın parolasını almak için [Azure portalda](https://portal.azure.com) **Genel** > **Cihaz ayrıntıları**'na gidin.
2. Data Box'a bağlanmak için kullandığınız bilgisayardaki Ethernet bağdaştırıcısına 192.168.100.5 statik IP adresini ve 255.255.255.0 alt ağını atayın. Cihazın yerel web kullanıcı arabirimine `https://192.168.100.10` adresinden erişin. Siz cihazı açtıktan sonra bağlantı kurulması 5 dakika kadar sürebilir.
3. Azure portalından alınan parolayı kullanarak oturum açın. Web sitesinin güvenlik sertifikasında sorun olduğunu belirten bir hata görürsünüz. Web sayfasına ilerlemek için tarayıcıya özel yönergeleri izleyin.
4. Varsayılan olarak, 10 Gb/sn (veya 1 Gb/sn) veri arabirimi için ağ ayarları DHCP olarak yapılandırılır. Gerekirse, bu arabirimi statik olarak yapılandırabilir ve bir IP adresi sağlayabilirsiniz.

## <a name="copy-data"></a>Veri kopyalama

Bu işlemi tamamlamak için gereken süre verilerinizin boyutuna ve ağın hızına göre değişir.

1. Windows istemcisi kullanıyorsanız Robocopy gibi SMB uyumlu bir dosya kopyalama aracı kullanın. NFS konağı için, `cp` veya `rsync` komutunu kullanarak verileri kopyalayın. Robocopy kullanarak veri kopyalama hakkında daha fazla bilgi için [Robocopy](https://technet.microsoft.com/library/ee851678.aspx) adresine gidin.
2. Cihaz paylaşımlarına bağlanın ve verileri konak bilgisayara kopyalamaya başlayın.
<!-- 1. Connect to the device shares using the path:`\\<IP address of your device>\ShareName`. To get the share access credentials, go to the **Connect & copy** page in the local web UI of the Data Box. --> 

## <a name="ship-to-azure"></a>Azure'a gönderme

Bu işlemin tamamlanması yaklaşık 10-15 dakika kadar sürer.

1. Yerel web kullanıcı arabirimindeki **Göndermeye hazırla** sayfasına gidin ve gönderme hazırlığına başlayın.
2. Cihazı yerel web kullanıcı arabiriminden kapatın. Kabloları cihazdan çıkarın.
3. Cihaz ile beraber sağlanan güç kablosunu sararak emniyetli şekilde cihazın arkasına yerleştirin.
4. İade gönderim etiketinin E-ink ekranında görünür olduğundan emin olun. E-ink ekranında etiket görüntülenmiyorsa, Azure portalda **Genel Bakış** > **Gönderim etiketini indir**'e gidin. Gönderim etiketini indirip cihaza yapıştırın.
5. Cihazı iade ediyorsanız bölgenizdeki taşıyıcıdan bir teslim alma randevusu alın.
6. Data Box nakliyeciniz tarafından toplandıktan ve tarandıktan sonra, portaldaki sipariş durumu **Toplandı** olarak güncelleştirilir. Ayrıca bir takip numarası da görüntülenir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu adımın tamamlanması 2-3 dakika sürer.

* Data Box siparişini işleme alınmadan önce Azure portaldan iptal edebilirsiniz. Sipariş işleme alındıktan sonra iptal edilemez. Sipariş, tamamlanma aşamasına gelene kadar ilerler. Siparişi iptal etmek için **Genel Bakış**'a gidin ve komut çubuğundan **İptal**'i seçin.

* Siparişin durumu **Tamamlandı** veya **İptal edildi** olduğunda Azure portaldan silebilirsiniz. Siparişi silmek için **Genel Bakış**'a gidin ve komut çubuğundan **Sil**'i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta Azure'dan Azure Data Box'a aktarma siparişi dağıttınız. Azure Data Box yönetimi hakkında daha fazla bilgi edinmek için aşağıdaki öğreticiye geçin:

> [!div class="nextstepaction"]
> [Azure portalını kullanarak Data Box'ı yönetme](data-box-portal-admin.md)
