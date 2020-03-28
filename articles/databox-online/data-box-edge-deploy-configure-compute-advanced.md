---
title: Azure Veri Kutusu Kenarı'nda bilgi işlemle gelişmiş dağıtım için verileri filtreleme, analiz etme öğreticisi | Microsoft Dokümanlar
description: Veri Kutusu Kenarı'ndaki bilgi işlem rolünü nasıl yapılandıracak ve Azure'a göndermeden önce gelişmiş dağıtım akışı için verileri dönüştürmek için nasıl kullanacağınızı öğrenin.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 05/20/2019
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to configure compute on Data Box Edge for advanced deployment flow so I can use it to transform the data before sending it to Azure.
ms.openlocfilehash: b446a3ebf92f6240d3bc02a148fbb8296efec926
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79239045"
---
# <a name="tutorial-transform-data-with-azure-data-box-edge-for-advanced-deployment-flow"></a>Öğretici: Gelişmiş dağıtım akışı için Azure Veri Kutusu Kenarı ile verileri dönüştürme

Bu öğretici, Azure Veri Kutusu Kenarı aygıtınızdaki gelişmiş bir dağıtım akışı için bir bilgi işlem rolünün nasıl yapılandırılabildiğini açıklar. İşlem rolünü yapılandırdıktan sonra, Veri Kutusu Kenarı verileri Azure'a göndermeden önce dönüştürebilir.

İşlem, cihazınızdaki basit veya gelişmiş dağıtım akışı için yapılandırılabilir.

|                  | Basit dağıtım                                | Gelişmiş dağıtım                   |
|------------------|--------------------------------------------------|---------------------------------------|
| Amaçlanan     | BT yöneticileri                                | Geliştiriciler                            |
| Tür             | Modülleri dağıtmak için Veri Kutusu Kenarı hizmetini kullanma      | Modülleri dağıtmak için IoT Hub hizmetini kullanma |
| Dağıtılan modüller | Tek                                           | Zincirli veya birden çok modül           |


Bu yordamın tamamlanması yaklaşık 20 ila 30 dakika sürebilir.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * İşlemi yapılandırma
> * Paylaşım ekleme
> * Tetikleyici ekleme
> * İşlem modülü ekleme
> * Veri dönüştürme işlemini doğrulama ve verileri aktarma

 
## <a name="prerequisites"></a>Ön koşullar

Veri Kutusu Kenar ı aygıtınızda bir bilgi işlem rolü ayarlamadan önce şunları yapın:

- Veri Kutusu Kenarı aygıtınızı Connect'te açıklandığı gibi [etkinleştirdin, Azure Veri Kutusu Kenarı'nı kurdunuz ve etkinleştirdin.](data-box-edge-deploy-connect-setup-activate.md)


## <a name="configure-compute"></a>İşlemi yapılandırma

Veri Kutusu Kenarınızdaki bilgileri yapılandırmak için bir IoT Hub kaynağı oluşturursunuz.

1. Veri Kutusu Kenarı kaynağınızın Azure portalında **Genel Bakış'a**gidin. Sağ bölmede, **İşlem** döşemesinde **Başlat'ı**seçin.

    ![İşlemle başlayın](./media/data-box-edge-deploy-configure-compute-advanced/configure-compute-1.png)

2. **Yapılkenar bilgi** niçin yapılandırılan döşemede, **Yapılandırma İşlemini**seçin.

    ![İşlemle başlayın](./media/data-box-edge-deploy-configure-compute-advanced/configure-compute-2.png)

3. **Yapılışı Kenarı işlem** bıçağına aşağıdakileri girin:

   
    |Alan  |Değer  |
    |---------|---------|
    |IoT Hub     | **Yeni** veya **Varolan'dan**birini seçin. <br> Varsayılan olarak, bir IoT kaynağı oluşturmak için standart katman (S1) kullanılır. Ücretsiz bir katman IoT kaynağı kullanmak için bir tane oluşturun ve sonra varolan kaynağı seçin. <br> Her durumda, IoT Hub kaynağı, Veri Kutusu Kenarı kaynağı tarafından kullanılan aynı abonelik ve kaynak grubunu kullanır.     |
    |Adı     |IoT Hub kaynağınız için bir ad girin.         |

    ![İşlemle başlayın](./media/data-box-edge-deploy-configure-compute-advanced/configure-compute-3.png)

4. **Oluştur'u**seçin. IoT Hub kaynak oluşturma birkaç dakika sürer. IoT Hub kaynağı oluşturulduktan sonra, bilgi işlem yapılandırmasını göstermek için **Kenarlıyı Yapıya İşlem** ini güncelleştirmeleri yapılandırır. Kenar bilgi işlem rolünün yapılandırıldığını doğrulamak için, **Yapılandırılan bilgi** niçin **config'i** görüntüleyin'i seçin.
    
    ![İşlemle başlayın](./media/data-box-edge-deploy-configure-compute-advanced/configure-compute-4.png)

    Edge aygıtta Edge işlem rolü ayarlandığında, iki aygıt oluşturur: bir IoT aygıtı ve bir IoT Edge aygıtı. Her iki aygıt da IoT Hub kaynağında görüntülenebilir. Bu IoT Edge aygıtında da bir IoT Edge Çalışma Süresi çalışıyor.

    Bu noktada, IoT Edge cihazınız için yalnızca Linux platformu kullanılabilir.


## <a name="add-shares"></a>Paylaşım ekleme

Bu öğreticide gelişmiş dağıtım için iki paylaşıma ihtiyacınız vardır: bir Edge paylaşımı ve başka bir Edge yerel paylaşımı.

1. Aşağıdaki adımları yaparak aygıta Edge paylaşımı ekleyin:

    1. Veri Kutusu Kenarı kaynağınızda, **Başlat'> Edge bilgi**işlem'ine gidin.
    2. Ekle **paylaşım(lar)** döşemesinde **Ekle'yi**seçin.
    3. Paylaşım **ekle** bıçağında, paylaşım adını sağlayın ve paylaşım türünü seçin.
    4. Kenar paylaşımını monte etmek için, **Kenar bilgiişlemiyle paylaşımı kullan**onay kutusunu seçin.
    5. Depolama **hesabı,** **Depolama hizmeti,** varolan bir kullanıcı yı seçin ve ardından **Oluştur'u**seçin.

        ![Kenar paylaşımı ekleme](./media/data-box-edge-deploy-configure-compute-advanced/add-edge-share-1.png)

    <!--If you created a local NFS share, use the following remote sync (rsync) command option to copy files onto the share:

    `rsync <source file path> < destination file path>`

    For more information about the rsync command, go to [Rsync documentation](https://www.computerhope.com/unix/rsync.htm).-->

    Kenar payı oluşturulduktan sonra başarılı bir oluşturma bildirimi alırsınız. Paylaşım listesi yeni paylaşımı yansıtacak şekilde güncelleştirilir.

2. Önceki adımdaki tüm adımları yineleyerek ve **Kenar yerel paylaşım olarak Yapılandır'ı yapmak**için onay kutusunu seçerek Edge aygıtına Kenar yerel paylaşımı ekleyin. Yerel paylaşımdaki veriler aygıtta kalır.

    ![Kenar yerel paylaşım ekleme](./media/data-box-edge-deploy-configure-compute-advanced/add-edge-share-2.png)

3. **Paylaşımlar** bıçağında, güncelleştirilmiş paylaşım listesini görürsünüz.

    ![Güncelleştirilmiş paylaşım listesi](./media/data-box-edge-deploy-configure-compute-advanced/add-edge-share-3.png)

4. Yeni oluşturulan yerel paylaşımın özelliklerini görüntülemek için, listeden payı seçin. Edge **bilgi işlem modülleri** kutusunun Yerel montaj noktasında, bu paylaşıma karşılık gelen değeri kopyalayın.

    Modülü dağıttığınızda bu yerel montaj noktasını kullanırsınız.

    !["Kenar bilgi işlem modülleri için yerel montaj noktası" kutusu](./media/data-box-edge-deploy-configure-compute-advanced/add-edge-share-4.png)
 
5. Oluşturduğunuz Kenar payının özelliklerini görüntülemek için listeden payı seçin. Edge **bilgi işlem modülleri** kutusunun Yerel montaj noktasında, bu paylaşıma karşılık gelen değeri kopyalayın.

    Modülü dağıttığınızda bu yerel montaj noktasını kullanırsınız.

    ![Özel modül ekleme](./media/data-box-edge-deploy-configure-compute-advanced/add-edge-share-5.png)


## <a name="add-a-trigger"></a>Tetikleyici ekleme

1. Edge **bilgi işlem > Tetikleyiciler**gidin. Seçin **+ Tetikleyici ekle**.

    ![Tetikleyici ekleme](./media/data-box-edge-deploy-configure-compute-advanced/add-trigger-1.png)

2. **Tetikleme ekle** bıçağına aşağıdaki değerleri girdiniz.

    |Alan  |Değer  |
    |---------|---------|
    |Tetikleyici adı     | Tetikleyiciniz için eşsiz bir isim.         |
    |Tetikleyici türü     | **Dosya** tetikleyicisi'ni seçin. Giriş payına dosya gibi bir dosya olayı oluştuğunda dosya tetikler. Diğer taraftan, zamanlanmış bir tetikleyici, sizin tanımladığınız bir zamanlamaya göre devreye saçılır. Bu örnekiçin, bir dosya tetikleyicigerekir.    |
    |Giriş payı     | Bir giriş paylaşımı seçin. Edge yerel payı bu durumda giriş payıdır. Burada kullanılan modül, dosyaları Edge yerel paylaşımından buluta yüklendikleri Edge paylaşımına taşır.        |

    ![Tetikleyici ekleme](./media/data-box-edge-deploy-configure-compute-advanced/add-trigger-2.png)

3. Tetikleyici oluşturulduktan sonra size bildirilir. Tetikleyiciler listesi yeni oluşturulan tetikleyiciyi görüntülemek için güncelleştirilir. Oluşturduğunuz tetikleyiciyi seçin.

    ![Tetikleyici ekleme](./media/data-box-edge-deploy-configure-compute-advanced/add-trigger-3.png)

4. Örnek rotayı kopyalayın ve kaydedin. Bu örnek rotayı değiştirecek ve daha sonra IoT Hub'ında kullanacaksınız.

    `"sampleroute": "FROM /* WHERE topic = 'mydbesmbedgelocalshare1' INTO BrokeredEndpoint(\"/modules/modulename/inputs/input1\")"`

    ![Tetikleyici ekleme](./media/data-box-edge-deploy-configure-compute-advanced/add-trigger-4.png)

## <a name="add-a-module"></a>Modül ekleme

Bu Edge aygıtında özel modül yok. Özel veya önceden oluşturulmuş bir modül ekleyebilirsiniz. Özel bir modül oluşturmayı öğrenmek [için Veri Kutusu Edge aygıtınız için c# modülü geliştirin.](data-box-edge-create-iot-edge-module.md)

Bu bölümde, Veri Kutusu Kenarınız için C# modülü geliştirin'de oluşturduğunuz IoT Edge aygıtına özel [bir modül eklersiniz.](data-box-edge-create-iot-edge-module.md) Bu özel modül, Edge aygıtındaki Edge yerel paylaşımından dosyaları alır ve aygıttaki Edge (bulut) paylaşımına taşır. Bulut paylaşımı, dosyaları bulut paylaşımıyla ilişkili Azure depolama hesabına iter.

1. **Başlat'> Edge'e gidin.** Modül **ekle** döşemesinde, gelişmiş **olarak**senaryo türünü seçin. **IoT Hub'ına Git'i**seçin.

    ![Gelişmiş dağıtımı seçin](./media/data-box-edge-deploy-configure-compute-advanced/add-module-1.png)

<!--2. In the **Configure and add module** blade, input the following values:  

    |Input share     | Select an input share. The Edge local share is the input share in this case. The module used here moves files from the Edge local share to an Edge share where they are uploaded into the cloud.        |
    |Output share     | Select an output share. The Edge share is the output share in this case.        |
-->

2. IoT Hub kaynağınızda, **IoT Edge aygıtına** gidin ve ardından IoT Edge aygıtınızı seçin.

    ![IoT Hub'daki IoT Edge aygıtına gidin](./media/data-box-edge-deploy-configure-compute-advanced/add-module-2.png)

3. **Aygıt ayrıntılarında,** **Modülleri Ayarla'yı**seçin.

    ![Modülleri Ayarla bağlantısı](./media/data-box-edge-deploy-configure-compute-advanced/add-module-3.png)

4. Modül Ekle altında **aşağıdakileri**yapın:

    1. Özel modülün konteyner kayıt defteri ayarlarının adını, adresini, kullanıcı adını ve parolasını girin.
    Ad, adres ve listelenen kimlik bilgileri eşleşen bir URL'ye sahip modülleri almak için kullanılır. Bu modülü dağıtmak için **Dağıtım modülleri** sayfasında **IoT Edge modülü**'nü seçin. Bu IoT Edge modülü, Veri Kutusu Kenarı aygıtınızla ilişkili IoT Edge aygıtına dağıtabileceğiniz bir docker kapsayıcısır.

        ![Modülleri Ayarla sayfası](./media/data-box-edge-deploy-configure-compute-advanced/add-module-4.png) 
 
    2. IoT Edge özel modül ayarlarını belirtin. Aşağıdaki değerleri girdin.
     
        |Alan  |Değer  |
        |---------|---------|
        |Adı     | Modül için benzersiz bir ad. Bu modül, Veri Kutusu Edge'inizle ilişkili IoT Edge aygıtına dağıtabileceğiniz bir docker kapsayıcısır.        |
        |Görüntü URI     | Modül için ilgili kapsayıcı görüntüsü için görüntü URI.        |
        |Gerekli kimlik bilgileri     | İşaretlenirse, eşleşen BIR URL'ye sahip modülleri almak için kullanıcı adı ve parola kullanılır.        |
    
        Kapsayıcı **Seçenekleri Oluştur** kutusuna, Edge payı ve Edge yerel paylaşımı için önceki adımlarda kopyaladığınız Kenar modülleri için yerel montaj noktalarını girin.

        > [!IMPORTANT]
        > Burada kullanılan yollar kapsayıcınıza monte edilir, bu nedenle kapsayıcınızdaki işlevselliğin beklediğiyle eşleşmelidir. [Özel bir modül oluştur'u](data-box-edge-create-iot-edge-module.md#update-the-module-with-custom-code)takip ediyorsanız, bu modülde belirtilen kod kopyalanan yolları bekler. Bu yolları değiştirmeyin.
    
        Kapsayıcı **Oluşturma Seçenekleri** kutusuna aşağıdaki örneği yapıştırabilirsiniz:
    
        ```
        {
          "HostConfig": 
          {
           "Binds": 
            [
             "/home/hcsshares/mydbesmbedgelocalshare1:/home/input",
             "/home/hcsshares/mydbesmbedgeshare1:/home/output"
            ]
           }
        }
        ```

        Modülünüz için kullanılan tüm çevresel değişkenleri sağlayın. Çevresel değişkenler, modülünüzün çalıştığı ortamı tanımlamaya yardımcı olan isteğe bağlı bilgiler sağlar.

        ![Kapsayıcı Oluşturma Seçenekleri kutusu](./media/data-box-edge-deploy-configure-compute-advanced/add-module-5.png) 
 
    4. Gerekirse, gelişmiş Kenar çalışma zamanı ayarlarını yapılandırın ve sonra **İleri'yi**tıklatın.

        ![Özel modül ekleme](./media/data-box-edge-deploy-configure-compute-advanced/add-module-6.png)
 
5.  **Yolları Belirt'** in altında, modüller arasında rotalar ayarlayın.  
    
    ![Rotaları Belirt](./media/data-box-edge-deploy-configure-compute-advanced/add-module-7.png)

    *Rotayı* daha önce kopyaladığınız aşağıdaki rota dizesiyle değiştirebilirsiniz. Bu örnekte, verileri bulut paylaşımına itecek yerel paylaşımın adını girin. Modülün `modulename` adını değiştirin. **Sonraki'ni**seçin.
        
    ```
    "route": "FROM /* WHERE topic = 'mydbesmbedgelocalshare1' INTO BrokeredEndpoint(\"/modules/filemove/inputs/input1\")"
    ```

    ![Rotaları Belirt bölümü](./media/data-box-edge-deploy-configure-compute-advanced/add-module-8.png)

6.  **İnceleme dağıtımı**altında, tüm ayarları gözden geçirin ve ardından dağıtım için modülü göndermek için **Gönder'i** seçin.

    ![Modülleri Ayarla sayfası](./media/data-box-edge-deploy-configure-compute-advanced/add-module-9.png)
 
    Bu eylem modül dağıtımını başlatır. Dağıtım tamamlandıktan sonra modülün **Çalışma Süresi durumu** **çalışır.**

    ![Özel modül ekleme](./media/data-box-edge-deploy-configure-compute-advanced/add-module-10.png)

## <a name="verify-data-transform-transfer"></a>Veri dönüştürmeyi, aktarmayı doğrulayın

Son adım, modülün beklendiği gibi bağlı ve çalışır durumda olduğundan emin olmaktır. Modülün çalışma süresi durumu, IoT Hub kaynağındaki IoT Edge aygıtınız için çalışıyor olmalıdır.

Veri dönüşümlerini ve Azure'a aktarımı doğrulamak için aşağıdaki adımları izleyin.
 
1.  Dosya Gezgini'nde, daha önce oluşturduğunuz Edge yerel ve Kenar paylaşımlarına bağlanın.

    ![Veri dönüştürmeyi doğrulama](./media/data-box-edge-deploy-configure-compute-advanced/verify-data-2.png)
 
1.  Yerel paylaşıma veri ekleyin.

    ![Veri dönüştürmeyi doğrulama](./media/data-box-edge-deploy-configure-compute-advanced/verify-data-3.png)
 
    Veriler bulut paylaşımına taşınır.

    ![Veri dönüştürmeyi doğrulama](./media/data-box-edge-deploy-configure-compute-advanced/verify-data-4.png)  

    Veriler daha sonra bulut paylaşımından depolama hesabına itilir. Verileri görüntülemek için depolama hesabınıza gidin ve ardından **Depolama Gezgini'ni**seçin. Yüklenen verileri depolama hesabınızda görüntüleyebilirsiniz.

    ![Veri dönüştürmeyi doğrulama](./media/data-box-edge-deploy-configure-compute-advanced/verify-data-5.png)
 
Doğrulama işlemini tamamladınız.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * İşlemi yapılandırma
> * Paylaşım ekleme
> * Tetikleyici ekleme
> * İşlem modülü ekleme
> * Veri dönüştürme işlemini doğrulama ve verileri aktarma

Veri Kutusu Edge cihazınızı nasıl yöneteceklerini öğrenmek için bkz:

> [!div class="nextstepaction"]
> [Data Box Edge yönetimi için yerel web arabirimi kullanma](data-box-edge-manage-access-power-connectivity-mode.md)
