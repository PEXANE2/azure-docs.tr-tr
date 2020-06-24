---
title: 'Hızlı başlangıç: Ubuntu Veri Bilimi Sanal Makinesi oluşturma'
titleSuffix: Azure Data Science Virtual Machine
description: Analiz ve makine öğrenimi yapmak için bir Linux (Ubuntu) Veri Bilimi Sanal Makinesi yapılandırın ve oluşturun.
ms.service: machine-learning
ms.subservice: data-science-vm
author: lobrien
ms.author: laobri
ms.topic: quickstart
ms.date: 03/10/2020
ms.openlocfilehash: 375149047d51574e14df15b6385b8c296d49a8ec
ms.sourcegitcommit: bf99428d2562a70f42b5a04021dde6ef26c3ec3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85254710"
---
# <a name="quickstart-set-up-the-data-science-virtual-machine-for-linux-ubuntu"></a>Hızlı başlangıç: Linux için Veri Bilimi Sanal Makinesi ayarlama (Ubuntu)

Ubuntu 18,04 Veri Bilimi Sanal Makinesi ile çalışmaya başlayın.

## <a name="prerequisites"></a>Ön koşullar

Ubuntu 18,04 Veri Bilimi Sanal Makinesi oluşturmak için bir Azure aboneliğinizin olması gerekir. [Azure 'ı ücretsiz deneyin](https://azure.com/free).

>[!NOTE]
>Azure Ücretsiz hesapları GPU etkin sanal makine SKU 'Larını desteklemez.

## <a name="create-your-data-science-virtual-machine-for-linux"></a>Linux için Veri Bilimi Sanal Makinesi oluşturma

Aşağıda Ubuntu 18,04 Veri Bilimi Sanal Makinesi örneğini oluşturma adımları verilmiştir:

1. [Azure Portal](https://portal.azure.com)gidin. Henüz oturum açmadıysanız Azure hesabınızda oturum açmanız istenebilir.
1. "Veri bilimi sanal makinesi" ni yazarak ve "Veri Bilimi Sanal Makinesi-Ubuntu 18,04" öğesini seçerek sanal makine listesini bulun

1. Sonraki pencerede **Oluştur**' u seçin.

1. "Sanal makine oluşturma" dikey penceresine yönlendirilmelisiniz.
   
1. Sihirbazın her adımını yapılandırmak için aşağıdaki bilgileri girin:

    1. **Temel bilgiler**:
    
       * **Abonelik**: birden fazla aboneliğiniz varsa makinenin oluşturulacağı ve faturalandırılabileceği bir tane seçin. Bu abonelikte kaynak oluşturma ayrıcalıklarına sahip olmanız gerekir.
       * **Kaynak grubu**: yeni bir grup oluşturun veya var olan bir grubu kullanın.
       * **Sanal makine adı**: sanal makinenin adını girin. Bu ad Azure portal kullanılacaktır.
       * **Bölge**: en uygun veri merkezini seçin. En hızlı ağ erişimi için bu, verilerinizin çoğunu veya fiziksel konumunuza en yakın olan veri merkezdir. [Azure bölgeleri](https://azure.microsoft.com/global-infrastructure/regions/)hakkında daha fazla bilgi edinin.
       * **Görüntü**: varsayılan değeri bırakın.
       * **Boyut**: Bu seçenek, genel iş yükleri için uygun bir boyutla, oto olarak doldurulur. [Azure 'Da LINUX VM boyutları](../../virtual-machines/linux/sizes.md)hakkında daha fazla bilgi edinin.
       * **Kimlik doğrulama türü**: daha hızlı kurulum Için "parola" yı seçin. 
         
         > [!NOTE]
         > Jupi Terhub 'ı kullanmayı düşünüyorsanız, jupi Pahub 'ı SSH ortak anahtarlarını kullanacak *şekilde yapılandırılmadığından "* parola" seçeneğini belirlediğinizden emin olun.

       * **Kullanıcı adı**: yönetici kullanıcı adını girin. Bu Kullanıcı adını sanal makinenizde oturum açmak için kullanacaksınız. Bu Kullanıcı adının Azure Kullanıcı adınızla aynı olmaması gerekir. Büyük harfli *harfler kullanmayın.*
         
         > [!IMPORTANT]
         > Kullanıcı adınızla büyük harfli harfler kullanırsanız, JupyterHub çalışmaz ve 500 iç sunucu hatası ile karşılaşırsınız.

       * **Parola**: sanal makinenizde oturum açmak için kullanacağınız parolayı girin.    
    
   1. **İncele ve oluştur**’u seçin.
   1. **Gözden geçir + oluştur**
      * Girdiğiniz tüm bilgilerin doğru olduğundan emin olun. 
      * **Oluştur**'u seçin.
    
    Sağlama 5 dakika sürer. Durum Azure portal görüntülenir.

## <a name="how-to-access-the-ubuntu-data-science-virtual-machine"></a>Ubuntu Veri Bilimi Sanal Makinesi erişme

Ubuntu DSVM 'ye üç şekilde erişebilirsiniz:

  * Terminal oturumları için SSH
  * Grafik oturumları için X2Go
  * Jupyter not defterleri için JupyterHub ve JupyterLab

Ayrıca, sanal makine üzerinde jupi not defterlerini çalıştırmak ve ücretsiz hizmet katmanının sınırlamalarını atlamak için Azure Notebooks Veri Bilimi Sanal Makinesi ekleyebilirsiniz. Daha fazla bilgi için bkz. [Azure Notebooks projelerini yönetme ve yapılandırma](../../notebooks/configure-manage-azure-notebooks-projects.md#compute-tier).

### <a name="ssh"></a>SSH

VM 'nizi SSH kimlik doğrulamasıyla yapılandırdıysanız, metin kabuğu arabirimi için adım 3 ' ün **temel kavramlar** bölümünde oluşturduğunuz hesap kimlik bilgilerini kullanarak oturum açabilirsiniz. Windows üzerinde [Putty](https://www.putty.org)gıbı bir SSH istemci aracını indirebilirsiniz. Grafik Masaüstü (X pencere sistemi) tercih ediyorsanız, PuTTY üzerinde X11 iletmeyi kullanabilirsiniz.

> [!NOTE]
> X2Go istemcisi, test sırasında X11 iletmeyi daha iyi gerçekleştirdi. Grafik Masaüstü arabirimi için X2Go istemcisini kullanmanızı öneririz.

### <a name="x2go"></a>X2Go

Linux VM, X2Go Server ile zaten sağlanmış ve istemci bağlantılarını kabul etmeye hazır. Linux VM grafik masaüstüne bağlanmak için, istemciniz üzerinde aşağıdaki yordamı izleyin:

1. İstemci platformunuz için X2Go istemcisini [X2Go](https://wiki.x2go.org/doku.php/doc:installation:x2goclient)adresinden indirin ve yükleyin.
1. Oluşturduğunuz sanal makineyi açarak Azure portal bulabileceğiniz sanal makinenin genel IP adresini unutmayın.

   ![Ubuntu makinesi IP adresi](./media/dsvm-ubuntu-intro/ubuntu-ip-address.png)

1. X2Go istemcisini çalıştırın. "Yeni oturum" penceresi otomatik olarak açımazsa, oturum > yeni oturum ' a gidin.

1. Elde edilen yapılandırma penceresinde aşağıdaki yapılandırma parametrelerini girin:
   * **Oturum sekmesi**:
     * **Konak**: daha önce bir DEĞIŞIKLIK yaptığınız sanal makinenizin IP adresini girin.
     * **Oturum aç**: Linux VM 'de Kullanıcı adını girin.
     * **SSH bağlantı noktası**: 22 ' de varsayılan değer olarak bırakın.
     * **Oturum türü**: değeri **Xfce**olarak değiştirin. Şu anda, Linux VM yalnızca XFCE masaüstünü destekler.
   * **Medya sekmesi**: kullanmaya gerek duymuyorsanız, ses desteğini kapatabilir ve istemci yazdırmayı devre dışı bırakabilirsiniz.
   * **Paylaşılan klasörler**: VM 'de bağlamak istediğiniz istemci makine dizinini eklemek için bu sekmeyi kullanın. 

   ![X2go yapılandırması](./media/dsvm-ubuntu-intro/x2go-ubuntu.png)
1. **Tamam**’ı seçin.
1. VM 'niz için oturum açma ekranını açmak üzere X2Go penceresinin sağ bölmesindeki kutuya tıklayın.
1. VM 'niz için parola girin.
1. **Tamam**’ı seçin.
1. Bağlanmayı tamamlayacak güvenlik duvarınızı atlamak için X2Go izni vermeniz gerekebilir.
1. Artık Ubuntu DSVM 'nizin grafik arabirimini görmeniz gerekir. 


### <a name="jupyterhub-and-jupyterlab"></a>Jupi Terhub ve jupi Terlab

Ubuntu DSVM, çok kullanıcılı bir Jupyıter sunucusu olan [Juponterhub](https://github.com/jupyterhub/jupyterhub)'ı çalıştırır. Bağlanmak için aşağıdaki adımları uygulayın:

   1. Azure portal VM 'nizi arayıp seçerek sanal makinenizin genel IP adresini unutmayın.
      ![Ubuntu makinesi IP adresi](./media/dsvm-ubuntu-intro/ubuntu-ip-address.png)

   1. Yerel makinenizden bir Web tarayıcısı açın ve https: \/ /Your-VM-IP: 8000 ' e giderek daha önce bir yere göz ATTıĞıNıZ IP adresini kullanarak "sizin-VM-IP" yi değiştirin.
   1. Tarayıcınız büyük olasılıkla, bir sertifika hatası olduğunu söyleyen sayfayı doğrudan açmanızı önler. DSVM, otomatik olarak imzalanan bir sertifika aracılığıyla güvenlik sağlar. Tarayıcıların çoğu, bu uyarıdan sonra tıklama yapmanıza izin verir. Birçok tarayıcı, Web oturumunuzun tamamında sertifika hakkında bir tür görsel uyarı sağlamaya devam edecektir.
   1. VM 'yi oluşturmak için kullandığınız kullanıcı adını ve parolayı girin ve oturum açın. 

      ![Jupyıter oturumu gir](./media/dsvm-ubuntu-intro/jupyter-login.png)

>[!NOTE]
> Bu aşamada 500 hatası alırsanız, Kullanıcı adınızla büyük harfli harfler kullanmış olabilirsiniz. Bu, JUPITER hub ve kullandığı Pamatımkimlik Cator arasındaki bilinen bir etkileşimdir. 

   1. Kullanılabilen birçok örnek Not defterini inceleyin.

Jupyıterlab, yeni nesil jupi Not defterleri ve Jupde de mevcuttur. Bu dosyaya erişmek için JupyterHub 'da oturum açın ve https: \/ /Your-VM-IP: 8000/User/The-username/Lab URL 'sine gidin ve VM 'yi yapılandırırken seçtiğiniz kullanıcı adıyla "adınızı-username" değerini değiştirin. Yine, bir sertifika hatası nedeniyle, başlangıçta siteye erişiminizi engellemiş olabilirsiniz.

Aşağıdaki satırı ekleyerek JupyterLab 'ı varsayılan not defteri sunucusu olarak ayarlayabilirsiniz `/etc/jupyterhub/jupyterhub_config.py` :

```python
c.Spawner.default_url = '/lab'
```

## <a name="next-steps"></a>Sonraki adımlar

Öğrendiklerinizi ve araştırmayı nasıl devam edebilmeniz gerekir:

* [Linux için veri bilimi sanal makinesi veri bilimi](linux-dsvm-walkthrough.md) izlenecek yol, burada sağlanan Linux dsvm ile birkaç ortak veri bilimi görevinin nasıl yapılacağını gösterir. 
* Bu makalede açıklanan araçları deneyerek DSVM 'deki çeşitli veri bilimi araçlarını inceleyin. Ayrıca, `dsvm-more-info` VM 'de yüklü araçlar hakkında daha fazla bilgi için temel bir giriş ve işaretçiler için sanal makine içindeki kabukta de çalıştırabilirsiniz.  
* [Team Data Science sürecini](https://aka.ms/tdsp)kullanarak analitik çözümleri sistematik olarak derlemeyi öğrenin.
* Machine Learning ve Azure AI Hizmetleri 'ni kullanan veri analizi örnekleri için [Azure yapay zeka Galerisi](https://gallery.azure.ai/) ziyaret edin.
* Bu sanal makine için uygun [başvuru belgelerine](./reference-ubuntu-vm.md) bakın.
