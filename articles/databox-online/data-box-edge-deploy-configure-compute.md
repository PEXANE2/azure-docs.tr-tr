---
title: Azure Veri Kutusu Kenarı'nda bilgileri bilgi işlemle filtreleme, analiz etme öğreticisi | Microsoft Dokümanlar
description: Data Box Edge'deki işlem rolünü yapılandırmayı ve verileri Azure'a göndermeden önce dönüştürmek için kullanmayı öğrenin.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 09/03/2019
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to configure compute on Data Box Edge so I can use it to transform the data before sending it to Azure.
ms.openlocfilehash: b641ae62ba6e0cdacaeb46b1ffee2f02c7544763
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79239024"
---
# <a name="tutorial-transform-data-with-azure-data-box-edge"></a>Öğretici: Azure Veri Kutusu Kenarı ile verileri dönüştürme

Bu öğretici, Azure Veri Kutusu Kenarı aygıtınızda bir bilgi işlem rolünün nasıl yapılandırılabildiğini açıklar. İşlem rolünü yapılandırdıktan sonra, Veri Kutusu Kenarı verileri Azure'a göndermeden önce dönüştürebilir.

Bu yordamın tamamlanması yaklaşık 10 ila 15 dakika sürebilir.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * İşlemi yapılandırma
> * Paylaşım ekleme
> * İşlem modülü ekleme
> * Veri dönüştürme işlemini doğrulama ve verileri aktarma

 
## <a name="prerequisites"></a>Ön koşullar

Veri Kutusu Kenar ı aygıtınızda bir bilgi işlem rolü ayarlamadan önce şunları yapın:

- Veri Kutusu Kenarı aygıtınızı Connect'te açıklandığı gibi [etkinleştirdin, Azure Veri Kutusu Kenarı'nı kurdunuz ve etkinleştirdin.](data-box-edge-deploy-connect-setup-activate.md)


## <a name="configure-compute"></a>İşlemi yapılandırma

Veri Kutusu Kenarınızdaki bilgileri yapılandırmak için bir IoT Hub kaynağı oluşturursunuz.

1. Veri Kutusu Kenarı kaynağınızın Azure portalında Genel Bakış'a gidin. Sağ bölmede, **İşlem** döşemesinde **Başlat'ı**seçin.

    ![İşlemle başlayın](./media/data-box-edge-deploy-configure-compute/configure-compute-1.png)

2. **Yapılkenar bilgi** niçin yapılandırılan döşemede, **Yapılandırma İşlemini**seçin.
3. **Yapılışı Kenarı işlem** bıçağına aşağıdakileri girin:

   
    |Alan  |Değer  |
    |---------|---------|
    |IoT Hub     | **Yeni** veya **Varolan'dan**birini seçin. <br> Varsayılan olarak, bir IoT kaynağı oluşturmak için standart katman (S1) kullanılır. Ücretsiz bir katman IoT kaynağı kullanmak için bir tane oluşturun ve sonra varolan kaynağı seçin. <br> Her durumda, IoT Hub kaynağı, Veri Kutusu Kenarı kaynağı tarafından kullanılan aynı abonelik ve kaynak grubunu kullanır.     |
    |Adı     |IoT Hub kaynağınız için bir ad girin.         |

    ![İşlemle başlayın](./media/data-box-edge-deploy-configure-compute/configure-compute-2.png)

4. **Oluştur'u**seçin. IoT Hub kaynak oluşturma birkaç dakika sürer. IoT Hub kaynağı oluşturulduktan sonra, bilgi işlem yapılandırmasını göstermek için **yapılandırılan yapı** döşemesi güncelleştirmeleri. Kenar bilgi işlem rolünün yapılandırıldığını doğrulamak için, **Yapılandırılan bilgi** niçin **Görünüm İşlemi'ni** seçin.
    
    ![İşlemle başlayın](./media/data-box-edge-deploy-configure-compute/configure-compute-3.png)

    > [!NOTE]
    > IoT Hub'ı Veri Kutusu Kenarı aygıtıyla ilişkilendirilmeden önce **Yapılandırma İşlemi** iletişim kutusu kapatılırsa, IoT Hub'ı oluşturulur, ancak bilgi işlem yapılandırmasında gösterilmez. 
    
    Edge aygıtta Edge işlem rolü ayarlandığında, iki aygıt oluşturur: bir IoT aygıtı ve bir IoT Edge aygıtı. Her iki aygıt da IoT Hub kaynağında görüntülenebilir. Bu IoT Edge aygıtında da bir IoT Edge Çalışma Süresi çalışıyor. Bu noktada, IoT Edge cihazınız için yalnızca Linux platformu kullanılabilir.


## <a name="add-shares"></a>Paylaşım ekleme

Bu öğreticideki basit dağıtım için iki paylaşıma ihtiyacınız vardır: bir Edge paylaşımı ve başka bir Edge yerel paylaşımı.

1. Aşağıdaki adımları yaparak aygıta Edge paylaşımı ekleyin:

    1. Veri Kutusu Kenarı kaynağınızda, **Başlat'> Edge bilgi**işlem'ine gidin.
    2. Ekle **paylaşım(lar)** döşemesinde **Ekle'yi**seçin.
    3. Paylaşım **ekle** bıçağında, paylaşım adını sağlayın ve paylaşım türünü seçin.
    4. Kenar paylaşımını monte etmek için, **Kenar bilgiişlemiyle paylaşımı kullan**onay kutusunu seçin.
    5. Depolama **hesabı,** **Depolama hizmeti,** varolan bir kullanıcı yı seçin ve ardından **Oluştur'u**seçin.

        ![Kenar paylaşımı ekleme](./media/data-box-edge-deploy-configure-compute/add-edge-share-1.png) 

    Yerel bir NFS paylaşımı oluşturduysanız, dosyaları paylaşıma kopyalamak için aşağıdaki uzaktan eşitleme (rsync) komut seçeneğini kullanın:

    `rsync <source file path> < destination file path>`

    rsync komutu hakkında daha fazla bilgi için [Rsync belgelerine](https://www.computerhope.com/unix/rsync.htm)gidin.

    Edge payı oluşturulur ve başarılı bir oluşturma bildirimi alırsınız. Paylaşım listesi güncelleştirilmiş olabilir, ancak paylaşım oluşturmanın tamamlanmasını beklemeniz gerekir.

2. Önceki adımdaki tüm adımları yineleyerek ve **Kenar yerel paylaşım olarak Yapılandır'ı yapmak**için onay kutusunu seçerek Edge aygıtına Kenar yerel paylaşımı ekleyin. Yerel paylaşımdaki veriler aygıtta kalır.

    ![Kenar yerel paylaşım ekleme](./media/data-box-edge-deploy-configure-compute/add-edge-share-2.png)

  
3. Güncelleştirilmiş paylaşım listesini görmek için **Share(lar) ekle'yi** seçin.

    ![Güncelleştirilmiş paylaşım listesi](./media/data-box-edge-deploy-configure-compute/add-edge-share-3.png) 
 

## <a name="add-a-module"></a>Modül ekleme

Özel veya önceden oluşturulmuş bir modül ekleyebilirsiniz. Bu Edge aygıtında özel modül yok. Özel bir modül oluşturmayı öğrenmek [için Veri Kutusu Edge aygıtınız için c# modülü geliştirin.](data-box-edge-create-iot-edge-module.md)

Bu bölümde, Veri Kutusu Kenarınız için C# modülü geliştirin'de oluşturduğunuz IoT Edge aygıtına özel [bir modül eklersiniz.](data-box-edge-create-iot-edge-module.md) Bu özel modül, Edge aygıtındaki Edge yerel paylaşımından dosyaları alır ve aygıttaki Edge (bulut) paylaşımına taşır. Bulut paylaşımı, dosyaları bulut paylaşımıyla ilişkili Azure depolama hesabına iter.

1. **Başlat'> Edge'e gidin.** Modül **ekle** döşemesinde, senaryo türünü **basit**olarak seçin. **Ekle'yi**seçin.
2. **Yapılaşıve modül bıçağıekle,** aşağıdaki değerleri girin:

    
    |Alan  |Değer  |
    |---------|---------|
    |Adı     | Modül için benzersiz bir ad. Bu modül, Veri Kutusu Edge'inizle ilişkili IoT Edge aygıtına dağıtabileceğiniz bir docker kapsayıcısır.        |
    |Görüntü URI     | Modül için ilgili kapsayıcı görüntüsü için görüntü URI.        |
    |Gerekli kimlik bilgileri     | İşaretlenirse, eşleşen BIR URL'ye sahip modülleri almak için kullanıcı adı ve parola kullanılır.        |
    |Giriş payı     | Bir giriş paylaşımı seçin. Edge yerel payı bu durumda giriş payıdır. Burada kullanılan modül, dosyaları Edge yerel paylaşımından buluta yüklendikleri Edge paylaşımına taşır.        |
    |Çıktı payı     | Bir çıktı payı seçin. Edge payı bu durumda çıktı payıdır.        |
    |Tetikleyici türü     | **Dosya** veya **Zamanlama'dan**seçin. Giriş payına dosya gibi bir dosya olayı oluştuğunda dosya tetikler. Zamanlanmış bir tetikleyici, sizin tanımladığınız bir zamanlamaya göre devreye saçılır.         |
    |Tetikleyici adı     | Tetikleyiciniz için eşsiz bir isim.         |
    |Ortam değişkenleri| Modülünüzün çalışacağı ortamı tanımlamaya yardımcı olacak isteğe bağlı bilgiler.   |

    ![Modül ekleme ve yapılandırma](./media/data-box-edge-deploy-configure-compute/add-module-1.png)

3. **Ekle'yi**seçin. Modül eklenir. Modül dağıtılmış olduğunu belirtmek için modül döşemesi **ekle** güncellenir. 

    ![Modül dağıtıldı](./media/data-box-edge-deploy-configure-compute/add-module-2.png)

### <a name="verify-data-transform-and-transfer"></a>Veri dönüştürme işlemini doğrulama ve verileri aktarma

Son adım, modülün beklendiği gibi bağlı ve çalışır durumda olduğundan emin olmaktır. Modülün çalışma süresi durumu, IoT Hub kaynağındaki IoT Edge aygıtınız için çalışıyor olmalıdır.

Modülün çalıştığını doğrulamak için aşağıdakileri yapın:

1. Modül döşemesini **ekle'yi** seçin. Bu sizi **Modules** bıçağına götürür. Modüller listesinde, dağıttığınız modülü tanımlayın. Eklediğiniz modülün çalışma süresi durumu *çalışıyor*olmalıdır.

    ![Veri dönüştürmeyi doğrulama](./media/data-box-edge-deploy-configure-compute/verify-data-1.png)
 
1.  Dosya Gezgini'nde, daha önce oluşturduğunuz Edge yerel ve Kenar paylaşımlarına bağlanın.

    ![Veri dönüştürmeyi doğrulama](./media/data-box-edge-deploy-configure-compute/verify-data-2.png) 
 
1.  Yerel paylaşıma veri ekleyin.

    ![Veri dönüştürmeyi doğrulama](./media/data-box-edge-deploy-configure-compute/verify-data-3.png) 
 
    Veriler bulut paylaşımına taşınır.

    ![Veri dönüştürmeyi doğrulama](./media/data-box-edge-deploy-configure-compute/verify-data-4.png)  

    Veriler daha sonra bulut paylaşımından depolama hesabına itilir. Verileri görüntülemek için Depolama Gezgini'ne gidin.

    ![Veri dönüştürmeyi doğrulama](./media/data-box-edge-deploy-configure-compute/verify-data-5.png) 
 
Doğrulama işlemini tamamladınız.


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * İşlemi yapılandırma
> * Paylaşım ekleme
> * İşlem modülü ekleme
> * Veri dönüştürme işlemini doğrulama ve verileri aktarma

Veri Kutusu Edge cihazınızı nasıl yöneteceklerini öğrenmek için bkz:

> [!div class="nextstepaction"]
> [Data Box Edge yönetimi için yerel web arabirimi kullanma](data-box-edge-manage-access-power-connectivity-mode.md)
