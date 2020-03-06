---
title: 'Hızlı başlangıç: Ubuntu Veri Bilimi Sanal Makinesi oluşturma'
titleSuffix: Azure Data Science Virtual Machine
description: Analiz ve makine öğrenimi yapmak için bir Linux (Ubuntu) Veri Bilimi Sanal Makinesi yapılandırın ve oluşturun.
ms.service: machine-learning
ms.subservice: data-science-vm
author: gvashishtha
ms.author: gopalv
ms.topic: quickstart
ms.date: 12/31/2019
ms.openlocfilehash: 5d139cbd59b1b3e63786ae22bbd3b934de37cd49
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78360669"
---
# <a name="quickstart-set-up-the-data-science-virtual-machine-for-linux-ubuntu"></a>Hızlı başlangıç: Linux için Veri Bilimi Sanal Makinesi ayarlama (Ubuntu)

Ubuntu 18,04 Veri Bilimi Sanal Makinesi ile çalışmaya başlayın.

## <a name="prerequisites"></a>Önkoşullar

Ubuntu 18,04 Veri Bilimi Sanal Makinesi oluşturmak için bir Azure aboneliğinizin olması gerekir. [Azure 'ı ücretsiz deneyin](https://azure.com/free).
Lütfen Azure Ücretsiz hesapları GPU etkin sanal makine SKU 'Larını desteklemez.

## <a name="create-your-data-science-virtual-machine-for-linux"></a>Linux için veri bilimi sanal makinenizi oluşturma

Aşağıda Ubuntu 18,04 Veri Bilimi Sanal Makinesi örneğini oluşturma adımları verilmiştir:

1. Daha önce oturum açmadıysanız Azure hesabınızda oturum açmanız istenebilir [Azure Portal](https://portal.azure.com) gidin.
1. "Veri bilimi sanal makinesi" ni yazarak ve "Veri Bilimi Sanal Makinesi-Ubuntu 18,04 Preview" öğesini seçerek sanal makine listesini bulun.

1. Sonraki pencerede **Oluştur**' u seçin.

1. "Sanal makine oluşturma" dikey penceresine yönlendirilmelisiniz.
   
1. Sihirbazın her adımını yapılandırmak için aşağıdaki bilgileri girin:

    1. **Temel bilgiler**:
    
       * **Abonelik**: birden fazla aboneliğiniz varsa makinenin oluşturulacağı ve faturalandırılabileceği bir tane seçin. Bu abonelikte kaynak oluşturma ayrıcalıklarına sahip olmanız gerekir.
       * **Kaynak grubu**: yeni bir grup oluşturun veya var olan bir grubu kullanın.
       * **Sanal makine adı**: sanal makinenin adını girin. Azure portal bu şekilde görünür.
       * **Bölge**: en uygun veri merkezini seçin. En hızlı ağ erişimi için bu, verilerinizin çoğunu veya fiziksel konumunuza en yakın olan veri merkezdir. [Azure bölgeleri](https://azure.microsoft.com/global-infrastructure/regions/)hakkında daha fazla bilgi edinin.
       * **Görüntü**: varsayılan değeri bırakın.
       * **Boyut**: Bu, genel iş yükleri için uygun bir boyutla otomatik olarak doldurulur. [Azure 'Da LINUX VM boyutları](../../virtual-machines/linux/sizes.md)hakkında daha fazla bilgi edinin.
       * **Kimlik doğrulama türü**: daha hızlı kurulum Için "parola" yı seçin. 
         
         > [!NOTE]
         > Jupi Terhub 'ı kullanmayı düşünüyorsanız, jupi Pahub 'ı SSH ortak anahtarlarını kullanacak *şekilde yapılandırılmadığından "* parola" seçeneğini belirlediğinizden emin olun.

       * **Kullanıcı adı**: yönetici kullanıcı adını girin. Bu, sanal makinenizde oturum açmak için kullanacağınız Kullanıcı adı ve Azure Kullanıcı adınızla aynı olmaması gerekir. Büyük harfli *harfler kullanmayın.*
         
         > [!NOTE]
         > Kullanıcı adınızla büyük harfli harfler kullanırsanız, JupyterHub çalışmaz ve 500 iç sunucu hatasıyla karşılaşacaksınız.

       * **Parola**: sanal makinenizde oturum açmak için kullanacağınız parolayı girin.    
    
   1. **İncele ve oluştur**’u seçin.
   1. **Gözden geçir + oluştur**
      * Girdiğiniz tüm bilgilerin doğru olduğundan emin olun. 
      * **Oluştur**’u seçin.
    
    Sağlama, yaklaşık 5 dakika sürer. Durum Azure portal görüntülenir.

## <a name="how-to-access-the-ubuntu-data-science-virtual-machine"></a>Ubuntu Veri Bilimi Sanal Makinesi erişme

Ubuntu DSVM 'ye üç şekilde erişebilirsiniz:

  * Terminal oturumlar için SSH
  * Grafik oturumlarını X2Go
  * JupyterHub ve Jupyter not defterleri için JupyterLab

Ayrıca, sanal makine üzerinde jupi not defterlerini çalıştırmak ve ücretsiz hizmet katmanının sınırlamalarını atlamak için Azure Notebooks Veri Bilimi Sanal Makinesi ekleyebilirsiniz. Daha fazla bilgi için bkz. [Azure Notebooks projelerini yönetme ve yapılandırma](../../notebooks/configure-manage-azure-notebooks-projects.md#compute-tier).

### <a name="ssh"></a>SSH

VM oluşturulduktan sonra SSH erişimiyle yapılandırıldıysa, SSH kullanarak oturum açabilirsiniz. Metin kabuğu arabirimi için adım 3 ' ün **temel kavramlar** bölümünde oluşturduğunuz hesap kimlik bilgilerini kullanın. Windows üzerinde [Putty](https://www.putty.org)gıbı bir SSH istemci aracını indirebilirsiniz. Grafik Masaüstü (X pencere sistemi) tercih ediyorsanız, PuTTY üzerinde X11 iletmeyi kullanabilirsiniz.

> [!NOTE]
> X2Go istemci testinde iletme X11 daha iyi gerçekleştirdi. X2Go istemci masaüstü bir grafik arabirim için kullanmanızı öneririz.

### <a name="x2go"></a>X2Go

Linux VM, X2Go Server ile zaten sağlanmış ve istemci bağlantılarını kabul etmeye hazır. Linux VM grafik masaüstüne bağlanmak için istemcinizi aşağıdaki yordamı tamamlayın:

1. İstemci platformunuz için X2Go istemcisini [X2Go](https://wiki.x2go.org/doku.php/doc:installation:x2goclient)adresinden indirin ve yükleyin.
1. Yeni oluşturduğunuz sanal makineyi açarak Azure portal bulabileceğiniz sanal makinenin genel IP adresini unutmayın.

   ![Ubuntu makinesi IP adresi](./media/dsvm-ubuntu-intro/ubuntu-ip-address.png)

1. X2Go istemcisini çalıştırın. "Yeni oturum" penceresi otomatik olarak açılamaz, oturum > yeni oturum ' a gidin.

1. Elde edilen yapılandırma penceresinde aşağıdaki yapılandırma parametrelerini girin:
   * **Oturum sekmesi**:
     * **Konak**: daha önce bir DEĞIŞIKLIK yaptığınız sanal makinenizin IP adresini girin.
     * **Oturum aç**: Linux VM 'de Kullanıcı adını girin.
     * **SSH bağlantı noktası**: 22 ' de varsayılan değer olarak bırakın.
     * **Oturum türü**: değeri **Xfce**olarak değiştirin. Şu anda, Linux VM yalnızca XFCE masaüstünü destekler.
   * **Medya sekmesi**: kullanmaya gerek duymuyorsanız, ses desteğini kapatabilir ve istemci yazdırmayı devre dışı bırakabilirsiniz.
   * **Paylaşılan klasörler**: istemci makinelerinizdeki DIZINLERIN Linux sanal makinesine bağlanmasını istiyorsanız, bu sekmede VM ile paylaşmak istediğiniz istemci makine dizinlerini ekleyin.

   ![X2go yapılandırması](./media/dsvm-ubuntu-intro/x2go-ubuntu.png)
1. **Tamam**’ı seçin.
1. VM 'niz için oturum açma ekranını açmak üzere X2Go penceresinin sağ bölmesindeki kutuya tıklayın.
1. VM 'niz için parola girin.
1. **Tamam**’ı seçin.
1. Bağlanmayı tamamlaması için güvenlik duvarınızı atlama X2Go izin vermeniz gerekebilir.
1. Artık Ubuntu DSVM 'nizin grafik arabirimini görmeniz gerekir. 


### <a name="jupyterhub-and-jupyterlab"></a>JupyterHub ve JupyterLab

Ubuntu DSVM, çok kullanıcılı bir Jupyıter sunucusu olan [Juponterhub](https://github.com/jupyterhub/jupyterhub)'ı çalıştırır. Bağlanmak için aşağıdaki adımları uygulayın:

   1. Azure portal VM 'nizi arayıp seçerek sanal makinenizin genel IP adresini unutmayın.
      Ubuntu makinesi IP adresini ![](./media/dsvm-ubuntu-intro/ubuntu-ip-address.png)

   1. Yerel makinenizden bir Web tarayıcısı açın ve https:\//Your-VM-IP: 8000 ' e giderek, "sizin-VM-IP" yi daha önce aldığınız IP adresiyle değiştirin.
   1. VM 'yi oluşturmak için kullandığınız kullanıcı adını ve parolayı girin ve oturum açın. 

      ![Jupyıter oturumu gir](./media/dsvm-ubuntu-intro/jupyter-login.png)

   1. Kullanılabilen birçok örnek Not defterini inceleyin.

JupyterLab, Jupyter Not defterlerinden ve JupyterHub, yeni nesil de kullanılabilir. Bu dosyaya erişmek için JupyterHub 'da oturum açın ve https:\//Your-VM-IP: 8000/User/-UserName/Lab URL 'sine gidin, VM 'yi yapılandırırken seçtiğiniz kullanıcı adıyla "adınızı-username" değerini değiştirin.

Bu satırı `/etc/jupyterhub/jupyterhub_config.py`ekleyerek, JupyterLab 'ı varsayılan not defteri sunucusu olarak ayarlayabilirsiniz:

```python
c.Spawner.default_url = '/lab'
```

## <a name="next-steps"></a>Sonraki adımlar

İşte öğrenme ve araştırma nasıl devam edebilirsiniz:

* [Linux için veri bilimi sanal makinesi veri bilimi](linux-dsvm-walkthrough.md) izlenecek yol, burada sağlanan Linux dsvm ile birkaç ortak veri bilimi görevinin nasıl yapılacağını gösterir. 
* Bu makalede açıklanan araçları deneyerek DSVM 'deki çeşitli veri bilimi araçlarını inceleyin. Ayrıca, VM 'de yüklü araçlar hakkında daha fazla bilgi için temel bir giriş ve işaretçiler için sanal makine içinde kabukta `dsvm-more-info` de çalıştırabilirsiniz.  
* [Team Data Science işlemini](https://aka.ms/tdsp)kullanarak, sistematik olarak uçtan uca analitik çözümler oluşturmayı öğrenin.
* Machine Learning ve Azure AI Hizmetleri 'ni kullanan veri analizi örnekleri için [Azure yapay zeka Galerisi](https://gallery.azure.ai/) ziyaret edin.
* Bu sanal makine için uygun [başvuru belgelerine](./reference-ubuntu-vm.md) bakın.
