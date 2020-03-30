---
title: 'Quickstart: CentOS Veri Bilimi Sanal Makine Oluşturma'
titleSuffix: Azure Data Science Virtual Machine
description: Analitik ve makine öğrenimi yapmak için Linux için Bir Veri Bilimi Sanal Makinesi (CentOS) yapılandırın ve oluşturun.
ms.service: machine-learning
ms.subservice: data-science-vm
author: gvashishtha
ms.author: gopalv
ms.topic: quickstart
ms.date: 09/13/2019
ms.openlocfilehash: 73541b31125ee6e99dc2351e26f6a564a1603487
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77526034"
---
# <a name="quickstart-set-up-a-centos-linux-data-science-virtual-machine-in-azure"></a>Quickstart: Azure'da Bir CentOS (Linux) Veri Bilimi Sanal Makinesi Ayarlama

CentOS tabanlı Veri Bilimi Sanal Makine ile çalışmaya devam edin.

## <a name="prerequisites"></a>Ön koşullar

CentOS Veri Bilimi Sanal Makinesi oluşturmak için bir **Azure aboneliğiniz**olması gerekir. [Ücretsiz abonelik oluşturun.](https://azure.com/free)

## <a name="create-your-centos-data-science-virtual-machine"></a>CentOS Veri Bilimi Sanal Makinenizi Oluşturun

CentOS Veri Bilimi Sanal Makine'nin bir örneğini oluşturma adımları şunlardır:

1. [Azure portalına](https://portal.azure.com)gidin. Azure hesabınızda oturum açmanız istenebilir. 
1. Arama çubuğuna "veri bilimi sanal makinesi" yazın ve CentOS DSVM'yi seçin.

    ![CentOS arama sonucu](./media/linux-dsvm-intro/search-centos.png)

1. Sonraki pencerede **Oluştur'u**seçin.

    [![](media/linux-dsvm-intro/create-centos.png "Button to create a CentOS machine")](media/linux-dsvm-intro/create-centos-expanded.png#lightbox)

1. "Sanal bir makine oluştur" bıçağına yönlendirilmelisiniz.
   
   ![CentOS Virtual Machine'e karşılık gelen temel ler sekmesi](./media/linux-dsvm-intro/review-create-centos.png)

1. Sihirbazın her adımını yapılandırmak için aşağıdaki bilgileri girin:

    1. **Temel Bilgiler**:
    
       * **Abonelik**: Birden fazla aboneliğiniz varsa, makinenin oluşturulacağı ve faturalandırılacağı aboneliği seçin. Bu abonelikte kaynak oluşturma ayrıcalıklarına sahip olmanız gerekir.
       * **Kaynak grubu**: Yeni bir grup oluşturun veya varolan bir grup kullanın.
       * **Sanal makine adı**: Sanal makinenin adını girin. Azure portalınızda bu şekilde görünür.
       * **Bölge**: En uygun veri merkezini seçin. En hızlı ağ erişimi için, verilerinizin çoğuna sahip olan veya fiziksel konumunuza en yakın olan veri merkezidir. [Azure Bölgeleri](https://azure.microsoft.com/global-infrastructure/regions/)hakkında daha fazla bilgi edinin.
       * **Resim**: Varsayılan değeri bırakın.
       * **Boyut**: Bu, genel iş yükleri için uygun bir boyutla otomatik doldurmalıdır. [Azure'da Linux VM boyutları](../../virtual-machines/linux/sizes.md)hakkında daha fazla bilgi edinin.
       * **Kimlik doğrulama türü**: Daha hızlı kurulum için "Parola"yı seçin. 
         
         > [!NOTE]
         > JupyterHub'ı kullanmayı planlıyorsanız, JupyterHub SSH ortak anahtarlarını kullanacak şekilde *yapılandırılmıdığından* "Parola"yı seçtiğinizden emin olun.

       * **Kullanıcı Adı**: Yönetici kullanıcı adını girin. Bu, sanal makinenize giriş yapmak için kullanacağınız kullanıcı adıdır ve Azure kullanıcı adınız ile aynı olması gerekmez. Büyük harfler *kullanmayın.*
         
         > [!NOTE]
         > Kullanıcı adınızda büyük harfler kullanırsanız, JupyterHub çalışmaz ve 500 dahili sunucu hatasıyla karşılaşırsınız.

       * **Şifre**: Sanal makinenize giriş yapmak için kullanacağınız şifreyi girin.    
    
   1. **İncele ve oluştur**’u seçin.
   1. **İnceleme+oluştur**
      * Girdiğiniz tüm bilgilerin doğru olduğundan doğrulayın. 
      * **Oluştur'u**seçin.
    
    Sağlama yaklaşık 5 dakika sürer. Durum Azure portalında görüntülenir.

## <a name="how-to-access-the-centos-data-science-virtual-machine"></a>CentOS Data Science Sanal Makine'ye nasıl erişilir?

CentOS DSVM'ye üç şekilde erişebilirsiniz:

  * Terminal oturumları için SSH
  * Grafik oturumları için X2Go
  * Jupyter not defterleri için JupyterHub ve JupyterLab

Ayrıca, VM'de Jupyter dizüstü bilgisayarları çalıştırmak ve ücretsiz hizmet katmanının sınırlamalarını atlamak için Azure Not Defterlerine bir Veri Bilimi Sanal Makinesi ekleyebilirsiniz. Daha fazla bilgi için Azure [Not Defterleri projelerini yönet ve yapılandırma](../../notebooks/configure-manage-azure-notebooks-projects.md#compute-tier)kınlarına bakın.

### <a name="ssh"></a>SSH

VM oluşturulduktan sonra, SSH erişimi ile yapılandırıldıysa, SSH kullanarak oturum açabilirsiniz. Metin kabuğu arabirimi için adım 3'ün **Temeller** bölümünde oluşturduğunuz hesap kimlik bilgilerini kullanın. Windows'da, [PuTTY](https://www.putty.org)gibi bir SSH istemci aracıindirebilirsiniz. Grafik bir masaüstü (X Pencere Sistemi) tercih ederseniz, PuTTY'de X11 yönlendirmeyi kullanabilirsiniz.

> [!NOTE]
> X2Go istemcisi testte X11 yönlendirmesinden daha iyi performans gösterdi. Grafiksel masaüstü arabirimi için X2Go istemcisini kullanmanızı öneririz.

### <a name="x2go"></a>X2Go

Linux VM zaten X2Go Server ile birlikte ve istemci bağlantıları kabul etmeye hazır. Linux VM grafik masaüstüne bağlanmak için istemciniz için aşağıdaki yordamı tamamlayın:

1. [X2Go](https://wiki.x2go.org/doku.php/doc:installation:x2goclient)istemci platformu için X2Go istemcisini indirin ve kurun.
1. Yeni oluşturduğunuz sanal makineyi açarak Azure portalında bulabileceğiniz sanal makinenin genel IP adresine dikkat edin.

   ![CentOS makine IP adresi](./media/linux-dsvm-intro/centos-ip-address.png)

1. X2Go istemcisini çalıştırın. "Yeni Oturum" penceresi otomatik olarak açMazsa, Oturum -> Yeni Oturum'a gidin.

1. Ortaya çıkan yapılandırma penceresinde aşağıdaki yapılandırma parametrelerini girin:
   * **Oturum sekmesi**:
     * **Ana bilgisayar**: Daha önce not aldığınız VM'nizin IP adresini girin.
     * **Giriş**: Linux VM'deki kullanıcı adını girin.
     * **SSH Bağlantı Noktası**: Varsayılan değer olan 22'de bırakın.
     * **Oturum Türü**: Değeri **XFCE**olarak değiştirin. Şu anda, Linux VM yalnızca XFCE masaüstünü destekler.
   * **Medya sekmesi**: Kullanmanız gerekmiyorsa ses desteğini ve istemci yazdırmayı kapatabilirsiniz.
   * **Paylaşılan klasörler**: Linux VM'ye monte edilmiş istemci makinelerinizden dizinler istiyorsanız, bu sekmede VM ile paylaşmak istediğiniz istemci makine dizinlerini ekleyin.

   ![X2go yapılandırması](./media/dsvm-ubuntu-intro/x2go-ubuntu.png)
1. **Tamam'ı**seçin.
1. VM'nizin oturum açma ekranını açmak için X2Go penceresinin sağ bölmesindeki kutuya tıklayın.
1. VM'nizin parolasını girin.
1. **Tamam'ı**seçin.
1. Bağlanmayı bitirmek için X2Go'ya güvenlik duvarınızı atlamak için izin vermeniz gerekebilir.
1. Şimdi CentOS DSVM için grafik arabirimi görmeniz gerekir. 


### <a name="jupyterhub-and-jupyterlab"></a>JupyterHub ve JupyterLab

CentOS DSVM [JupyterHub](https://github.com/jupyterhub/jupyterhub)çalışır , bir çok kullanıcılı Jupyter sunucu. Bağlanmak için aşağıdaki adımları izleyin:

   1. Azure portalında VM'nizi arayarak ve seçerek VM'niz için genel IP adresine dikkat edin.

       ![CentOS makine IP adresi](./media/linux-dsvm-intro/centos-ip-address.png)

   1. Yerel makinenizden bir web tarayıcısı açın\/ve https: /your-vm-ip:8000 adresine gidin ve "your-vm-ip"i daha önce not aldığınız IP adresiyle değiştirin.
   1. VM'yi oluşturmak için kullandığınız kullanıcı adını ve parolayı girin ve oturum açın. 

      ![Jupyter giriş girin](./media/dsvm-ubuntu-intro/jupyter-login.png)

   1. Kullanılabilen birçok örnek not defterine göz atın.

JupyterLab, Jupyter dizüstü ve JupyterHub yeni nesil, aynı zamanda mevcuttur. Erişmek için JupyterHub'da oturum açın ve ardından https\/URL'sine göz atın: /your-vm-ip:8000/user/your-username/lab adresinde vm yapılandırırken seçtiğiniz kullanıcı adı ile "kullanıcı adınız" değiştirin.

JupyterLab'ı varsayılan dizüstü bilgisayar sunucusu olarak `/etc/jupyterhub/jupyterhub_config.py`bu satırı şu satıra ekleyerek ayarlayabilirsiniz:

```python
c.Spawner.default_url = '/lab'
```

## <a name="next-steps"></a>Sonraki adımlar

Öğrenme nize ve araştırmanıza şu şekilde devam edebilirsiniz:

* [Linux için Veri Bilimi Sanal Makine üzerinde](linux-dsvm-walkthrough.md) walkthrough Veri bilimi nasıl Linux DSVM burada sağlanan ile birkaç ortak veri bilimi görevleri yapmak gösterir. 
* Bu makalede açıklanan araçları deneyerek DSVM'deki çeşitli veri bilimi araçlarını keşfedin. Ayrıca, `dsvm-more-info` temel bir giriş için sanal makinede ve dsvm yüklü araçlar hakkında daha fazla bilgi için işaretçiler için kabuk çalıştırabilirsiniz.  
* [Ekip Veri Bilimi Süreci'ni](https://aka.ms/tdsp)kullanarak sistematik olarak uçuca analitik çözümler oluşturmayı öğrenin.
* Azure [Yapay AI](https://gallery.azure.ai/) hizmetlerini kullanan makine öğrenimi ve veri analizi örnekleri için Azure Yapay Değer Galerisi'ni ziyaret edin.
* Bu sanal makine için uygun [başvuru belgelerine](./reference-centos-vm.md) başvurun.