---
title: 'Hızlı başlangıç: Ubuntu DSVM oluşturma'
description: Analiz ve makine öğrenimi yapmak için bir Linux (Ubuntu) Veri Bilimi Sanal Makinesi yapılandırın ve oluşturun.
ms.service: machine-learning
ms.subservice: data-science-vm
author: gvashishtha
ms.author: gopalv
ms.topic: quickstart
ms.date: 09/10/2019
ms.openlocfilehash: 951191422e80ef6224cd140beed782de31cb3822
ms.sourcegitcommit: 5f0f1accf4b03629fcb5a371d9355a99d54c5a7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/30/2019
ms.locfileid: "71677854"
---
# <a name="quickstart-set-up-the-data-science-virtual-machine-for-linux-ubuntu"></a>Hızlı başlangıç: Linux için Veri Bilimi Sanal Makinesi ayarlama (Ubuntu)

Ubuntu Veri Bilimi Sanal Makinesi ile çalışmaya başlayın.

## <a name="prerequisites"></a>Önkoşullar

Bir Windows Veri Bilimi Sanal Makinesi oluşturmak için bir Azure aboneliğinizin olması gerekir. [Azure 'ı ücretsiz deneyin](https://azure.com/free).
Lütfen Azure Ücretsiz hesapları GPU etkin sanal makine SKU 'Larını desteklemez.

## <a name="create-your-data-science-virtual-machine-for-linux"></a>Linux için veri bilimi sanal makinenizi oluşturma

Linux için veri bilimi sanal makinesi örneğini oluşturmak için adımlar şunlardır:

1. Daha önce oturum açmadıysanız Azure hesabınızda oturum açmanız istenebilir [Azure Portal](https://portal.azure.com) gidin.
1. "Veri bilimi sanal makinesi" yazın ve "Linux için Veri Bilimi Sanal Makinesi (Ubuntu)" öğesini seçerek sanal makine listesini bulun.
    
    ![Ubuntu VM listesi](./media/dsvm-ubuntu-intro/search-ubuntu.png)

1. Sonraki pencerede **Oluştur**' u seçin.

   [![](media/dsvm-ubuntu-intro/create-linux.png "Button to create an Ubuntu machine")](media/dsvm-ubuntu-intro/create-linux-expanded.png#lightbox)

1. [![](media/dsvm-ubuntu-intro/create-linux.png "Ubuntu makinesi oluşturma düğmesi")](media/dsvm-ubuntu-intro/create-linux-expanded.png#lightbox)
   
   !["Sanal makine oluşturma" dikey penceresine yönlendirilmelisiniz.](./media/dsvm-ubuntu-intro/review-create-ubuntu.png)

1. Ubuntu sanal makinesine karşılık gelen temel bilgiler sekmesi

    1. Sihirbazın her adımını yapılandırmak için aşağıdaki bilgileri girin:
    
       * **Temel**: **Abonelik**: birden fazla aboneliğiniz varsa makinenin oluşturulacağı ve faturalandırılabileceği bir tane seçin.
       * Bu abonelikte kaynak oluşturma ayrıcalıklarına sahip olmanız gerekir.
       * **Kaynak grubu**: yeni bir grup oluşturun veya var olan bir grubu kullanın. **Sanal makine adı**: sanal makinenin adını girin.
       * Azure portal bu şekilde görünür. **Bölge**: en uygun veri merkezini seçin. En hızlı ağ erişimi için bu, verilerinizin çoğunu veya fiziksel konumunuza en yakın olan veri merkezdir.
       * [Azure bölgeleri](https://azure.microsoft.com/global-infrastructure/regions/)hakkında daha fazla bilgi edinin.
       * **Görüntü**: varsayılan değeri bırakın. **Boyut**: Bu, genel iş yükleri için uygun bir boyutla otomatik olarak doldurulur.
       * [Azure 'Da LINUX VM boyutları](../../virtual-machines/linux/sizes.md)hakkında daha fazla bilgi edinin. 
         
         > [!NOTE]
         > **Kimlik doğrulama türü**: daha hızlı kurulum Için "parola" yı seçin.

       * Jupi Terhub 'ı kullanmayı düşünüyorsanız, jupi Pahub 'ı SSH ortak anahtarlarını kullanacak *şekilde yapılandırılmadığından "* parola" seçeneğini belirlediğinizden emin olun. **Kullanıcı adı**: yönetici kullanıcı adını girin. Bu, sanal makinenizde oturum açmak için kullanacağınız Kullanıcı adı ve Azure Kullanıcı adınızla aynı olmaması gerekir.
         
         > [!NOTE]
         > Büyük harfli *harfler kullanmayın.*

       * Kullanıcı adınızla büyük harfli harfler kullanırsanız, JupyterHub çalışmaz ve 500 iç sunucu hatasıyla karşılaşacaksınız.    
    
   1. **Parola**: sanal makinenizde oturum açmak için kullanacağınız parolayı girin.
   1. **İncele ve oluştur**’u seçin.
      * **Gözden geçir + oluştur** 
      * Girdiğiniz tüm bilgilerin doğru olduğundan emin olun.
    
    **Oluştur**'u seçin. Sağlama, yaklaşık 5 dakika sürer.

## <a name="how-to-access-the-ubuntu-data-science-virtual-machine"></a>Durum Azure portal görüntülenir.

Ubuntu Veri Bilimi Sanal Makinesi erişme

  * Ubuntu DSVM 'ye üç şekilde erişebilirsiniz:
  * Terminal oturumlar için SSH
  * Grafik oturumlarını X2Go

JupyterHub ve Jupyter not defterleri için JupyterLab Ayrıca, sanal makine üzerinde jupi not defterlerini çalıştırmak ve ücretsiz hizmet katmanının sınırlamalarını atlamak için Azure Notebooks Veri Bilimi Sanal Makinesi ekleyebilirsiniz.

### <a name="ssh"></a>Daha fazla bilgi için bkz. [Azure Notebooks projelerini yönetme ve yapılandırma](../../notebooks/configure-manage-azure-notebooks-projects.md#compute-tier).

SSH VM oluşturulduktan sonra SSH erişimiyle yapılandırıldıysa, SSH kullanarak oturum açabilirsiniz. Oluşturduğunuz hesabı kimlik bilgilerini kullan **Temelleri** bölümünde metin kabuk arabirimi için adım 3. Windows üzerinde [Putty](https://www.putty.org)gıbı bir SSH istemci aracını indirebilirsiniz.

> [!NOTE]
> Grafik Masaüstü (X pencere sistemi) tercih ediyorsanız, PuTTY üzerinde X11 iletmeyi kullanabilirsiniz. X2Go istemci testinde iletme X11 daha iyi gerçekleştirdi.

### <a name="x2go"></a>X2Go istemci masaüstü bir grafik arabirim için kullanmanızı öneririz.

X2Go Linux VM, X2Go Server ile zaten sağlanmış ve istemci bağlantılarını kabul etmeye hazır.

1. Linux VM grafik masaüstüne bağlanmak için istemcinizi aşağıdaki yordamı tamamlayın:
1. İstemci platformunuza yönelik X2Go istemcisini indirme ve yükleme [X2Go](https://wiki.x2go.org/doku.php/doc:installation:x2goclient).

   ![Yeni oluşturduğunuz sanal makineyi açarak Azure portal bulabileceğiniz sanal makinenin genel IP adresini unutmayın.](./media/dsvm-ubuntu-intro/ubuntu-ip-address.png)

1. Ubuntu makinesi IP adresi X2Go istemcisini çalıştırın.

1. "Yeni oturum" penceresi otomatik olarak açılamaz, oturum > yeni oturum ' a gidin.
   * Elde edilen yapılandırma penceresinde aşağıdaki yapılandırma parametrelerini girin:
     * **Oturum sekmesini**:
     * **Konak**: daha önce bir DEĞIŞIKLIK yaptığınız sanal makinenizin IP adresini girin.
     * **Oturum aç**: Linux VM 'de Kullanıcı adını girin.
     * **SSH bağlantı noktası**: 22, varsayılan değeri bırakın. **Oturum türü**: değere değiştirin **XFCE**.
   * Şu anda, Linux VM yalnızca XFCE masaüstünü destekler.
   * **Ortam sekmesini**: ses desteği ve bunları kullanmak gerekmiyorsa Yazdırma İstemcisi kapat.

   ![**Paylaşılan Klasörler**: Linux VM'de bağlı istemci makinelerden dizinleri istiyorsanız, bu sekmedeki VM ile paylaşmak istediğiniz istemci makine dizinlerine ekleyin.](./media/dsvm-ubuntu-intro/x2go-ubuntu.png)
1. X2go yapılandırması
1. **Tamam**’ı seçin.
1. VM 'niz için oturum açma ekranını açmak üzere X2Go penceresinin sağ bölmesindeki kutuya tıklayın.
1. VM 'niz için parola girin.
1. **Tamam**’ı seçin.
1. Bağlanmayı tamamlaması için güvenlik duvarınızı atlama X2Go izin vermeniz gerekebilir. 


### <a name="jupyterhub-and-jupyterlab"></a>Artık Ubuntu DSVM 'nizin grafik arabirimini görmeniz gerekir.

JupyterHub ve JupyterLab Ubuntu DSVM, çok kullanıcılı bir Jupyıter sunucusu olan [Juponterhub](https://github.com/jupyterhub/jupyterhub)'ı çalıştırır.

   1. Bağlanmak için aşağıdaki adımları uygulayın:
      Azure portal VM 'nizi arayıp seçerek sanal makinenizin genel IP adresini unutmayın.

   1. Ubuntu makinesi IP adresini ![](./media/dsvm-ubuntu-intro/ubuntu-ip-address.png)
   1. Yerel makinenizden bir Web tarayıcısı açın ve https:\//Your-VM-IP: 8000 ' e giderek, "sizin-VM-IP" yi daha önce aldığınız IP adresiyle değiştirin. 

      ![VM 'yi oluşturmak için kullandığınız kullanıcı adını ve parolayı girin ve oturum açın.](./media/dsvm-ubuntu-intro/jupyter-login.png)

   1. Jupyıter oturumu gir

Kullanılabilen birçok örnek Not defterini inceleyin. JupyterLab, Jupyter Not defterlerinden ve JupyterHub, yeni nesil de kullanılabilir.

Bu dosyaya erişmek için JupyterHub 'da oturum açın ve https:\//Your-VM-IP: 8000/User/-UserName/Lab URL 'sine gidin, VM 'yi yapılandırırken seçtiğiniz kullanıcı adıyla "adınızı-username" değerini değiştirin.

```python
c.Spawner.default_url = '/lab'
```

## <a name="next-steps"></a>Bu satırı `/etc/jupyterhub/jupyterhub_config.py`ekleyerek, JupyterLab 'ı varsayılan not defteri sunucusu olarak ayarlayabilirsiniz:

Sonraki adımlar

* İşte öğrenme ve araştırma nasıl devam edebilirsiniz: 
* [Linux için veri bilimi sanal makinesi veri bilimi](linux-dsvm-walkthrough.md) izlenecek yol, burada sağlanan Linux dsvm ile birkaç ortak veri bilimi görevinin nasıl yapılacağını gösterir. Bu makalede açıklanan araçları deneyerek DSVM 'deki çeşitli veri bilimi araçlarını inceleyin.  
* Ayrıca, VM 'de yüklü araçlar hakkında daha fazla bilgi için temel bir giriş ve işaretçiler için sanal makine içinde kabukta `dsvm-more-info` de çalıştırabilirsiniz.
* Sistematik olarak kullanarak uçtan uca analitik çözümler oluşturmayı öğrenin [Team Data Science Process](https://aka.ms/tdsp).
* Ziyaret [Azure AI Gallery](https://gallery.azure.ai/) Azure yapay ZEKA hizmetlerini kullanma makine öğrenimi ve veri analizi örnekleri için.
