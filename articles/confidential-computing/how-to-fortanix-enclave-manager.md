---
title: Fortanx Enclave Manager ile uygulama çalıştırma
description: Kapsayıcılı görüntülerinizi dönüştürmek için Fortanx Enclave Manager 'ı nasıl kullanacağınızı öğrenin
services: virtual-machines
author: JBCook
ms.service: virtual-machines
ms.subservice: workloads
ms.workload: infrastructure
ms.topic: how-to
ms.date: 8/12/2020
ms.author: JenCook
ms.openlocfilehash: 8996673cf8f01334e3dcd35aab8544643bd37360
ms.sourcegitcommit: c5021f2095e25750eb34fd0b866adf5d81d56c3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/25/2020
ms.locfileid: "88802226"
---
# <a name="how-to-run-an-application-with-fortanix-enclave-manager"></a>Nasıl yapılır: Fortanx Enclave Manager ile uygulama çalıştırma 

Fortanx [Enclave Manager](https://azuremarketplace.microsoft.com/marketplace/apps/fortanix.enclave_manager?tab=Overview) ve Fortanx [düğüm aracısını](https://azuremarketplace.microsoft.com/marketplace/apps/fortanix.rte_node_agent) [fortanx](https://www.fortanix.com/)' ten kullanarak uygulamanızı Azure gizli bilgi işlem ortamında çalıştırmaya başlayın.


Fortanx, Azure altyapısının üzerine inşa eden ürün ve hizmetlere sahip üçüncü taraf bir yazılım satıcısı. Azure 'da [Anjuna](https://azuremarketplace.microsoft.com/marketplace/apps/anjuna-5229812.aee-az-v1) ve [skoni](https://sconedocs.github.io)gibi benzer gizli bilgi işlem hizmetleri sunan başka üçüncü taraf sağlayıcılar vardır.  

> [!Note] 
 > BU BELGEDE BAŞVURULAN ÜRÜNLER MICROSOFT DENETIMI ALTıNDA DEĞIL. MICROSOFT BU BILGILERI SIZE KOLAYLıK SAĞLAMASı IÇIN SAĞLıYOR VE BU MICROSOFT DıŞı ÜRÜNLERIN BAŞVURUSU MICROSOFT TARAFıNDAN ONAY GÖSTERMEZ.



Bu öğreticide, uygulama görüntünüzü gizli işlem korumalı bir görüntüye nasıl dönüştürebileceğiniz gösterilmektedir. Bu ortam, Azure 'un DCsv2 serisi Intel SGX özellikli sanal makineler tarafından desteklenen [Fortanx](https://www.fortanix.com/) yazılımını kullanır. Bu çözüm, kimlik doğrulama ve veri erişim denetimi gibi kritik güvenlik ilkelerini düzenler.

 Fortanix 'e özgü destek için [Fortanix bolluk topluluğuna](https://fortanix.com/community/) katılarak kanal #enclavemanager kullanın.


## <a name="prerequisites"></a>Önkoşullar

1. Bir Fortanx Kuşyave yöneticisi hesabınız yoksa, başlamadan önce [kaydolun](https://em.fortanix.com/auth/sign-up) .
1. Dönüştürülmüş uygulama görüntülerini göndermek için özel bir [Docker](https://docs.docker.com/) kayıt defteri.
1. Azure aboneliğiniz yoksa başlamadan önce [bir hesap oluşturun](https://azure.microsoft.com/pricing/purchase-options/pay-as-you-go/) .

> [!NOTE]
> Ücretsiz deneme hesaplarının bu öğreticide kullanılan sanal makinelere erişimi yoktur. Lütfen Kullandıkça Öde aboneliğine yükseltin.

## <a name="add-an-application-to-fortanix-enclave-manager"></a>Fortanix Enclave Manager 'a uygulama ekleme
1. [Fortanix em](https://fortanix.com) 'de oturum açın
1. **Hesaplar** sayfasına gıdın ve **Hesap Ekle** ' yi seçerek yeni bir hesap oluşturun. 
    
[!div class="mx-imgBorder"]
![Hesap oluşturma](media/how-to-fortanix-enclave-manager/create-account.png)

1. Hesabınız oluşturulduktan sonra, yeni oluşturulan hesabı seçmek için **Seç** ' e tıklayın. Şimdi işlem düğümlerini kaydetmeye ve uygulamalar oluşturmaya başlayabiliriz. 
1. Uygulama eklemek için **+ uygulama** düğmesini seçin. Bu örnekte, bir Flask sunucusu şifreleme SISTEMI uygulaması ekliyoruz. 

1. Şifreleme işletim sistemi uygulaması için **Ekle** düğmesini seçin. 

    ![Uygulama ekleme](media/how-to-fortanix-enclave-manager/add-enclave-application.png)

    > [!NOTE]
    > Bu öğretici yalnızca şifreleme işletim sistemi uygulamaları eklemeyi içerir. EDP Rust uygulamalarını Fortanix Enclave Manager 'a getirme hakkında [daha fazla bilgi edinin](https://support.fortanix.com/hc/en-us/articles/360044746932-Bringing-EDP-Rust-Apps-to-Enclave-Manager) . 

6. Bu öğreticide, örnek uygulama için Fortanx ' in Docker kayıt defterini kullanacağız. Aşağıdaki bilgilerden ayrıntıları girin. Çıkış görüntüsünü tutmak için özel Docker kayıt defterinizi kullanın. 
 
    - **Uygulama adı**: Python uygulama sunucusu
    - **Açıklama**: Python Flask sunucusu
    - **Giriş resmi adı**: fortanx/Python-Flask
    - **Çıkış görüntüsü adı**: fortanx-Private/Python-Flask-SGX
    - **ISVPRODID**: 1
    - **Ivsvm**: 1
    - **Bellek boyutu**: 1 GB
    - **Iş parçacığı sayısı**: 128

    *Isteğe bağlı*: uygulamayı çalıştırın.
    - **Docker Hub**: [https://hub.docker.com/u/fortanix](https://hub.docker.com/u/fortanix)
    - **Uygulama**: fortanix/Python-Flask

        Aşağıdaki komutu çalıştırın:
         ```bash
            sudo docker run fortanix/python-flask
         ```

1. Sertifika ekleyin. Aşağıdaki ayrıntıları kullanarak bilgileri girin ve ardından **İleri**' yi seçin:
    - **Etki alanı**: MyApp. domain. Dom
    - **Tür**: sertifika veren yönetici tarafından verilen sertifika 
    - **Anahtar yolu**:/appkey.exe
    - **Anahtar türü**: RSA
    - **Sertifika yolu**:/appcert.exe
    - **RSA anahtar boyutu**: 2048 bit
    

## <a name="create-an-image"></a>Görüntü oluşturma
Bir Fortanx EM görüntüsü, bir uygulamanın yazılım sürümüdür veya sürümüdür. Her görüntü, bir şifreleme karması (MRENCLAVE) ile ilişkilendirilir. 
1. **Görüntü ekle** sayfasında, **Çıkış görüntüsü adı**IÇIN **kayıt defteri kimlik bilgilerini** girin. Bu kimlik bilgileri, görüntünün gönderildiği özel Docker kayıt defterine erişmek için kullanılır. 

    ![görüntü oluştur](media/how-to-fortanix-enclave-manager/create-image.png)
1. Resim etiketini girip **Oluştur**' u seçin.

    ![etiket ekle](media/how-to-fortanix-enclave-manager/add-tag.png)

## <a name="domain-and-image-allow-listing"></a>Etki alanı ve görüntü listeye izin ver 
Etki alanı izin verilenler listesine eklenen bir uygulama, Fortanx Enclave Manager 'dan bir TLS sertifikası alacak. Benzer şekilde, bir uygulama dönüştürülen görüntüden çalıştığında Fortanx Enclave Manager ile iletişim kurmaya çalışır. Uygulama daha sonra bir TLS sertifikası ister. 

Soldaki **Görevler** sekmesine geçin ve bekleyen istekleri etki alanı ve görüntüye izin verecek şekilde onaylayın. 

## <a name="enroll-compute-node-agent-in-azure"></a>Azure 'da işlem düğümü aracısını kaydetme

### <a name="generate-and-copy-join-token"></a>JOIN belirteci oluştur ve Kopyala
Fortanx Enclave Manager 'da bir belirteç oluşturacaksınız. Bu belirteç, Azure 'daki bir işlem düğümünün kendi kimliğini doğrulamasına izin verir. Bu belirteci Azure sanal makinenize sağlamanız gerekir.
1. Yönetim konsolunda **+ düğüm kaydet** düğmesini seçin. 
1. JOIN belirtecini oluşturmak için **belirteç oluştur** ' u seçin. Belirteci kopyalayın.

### <a name="enroll-nodes-into-fortanix-node-agent-in-azure-marketplace"></a>Azure Marketi 'nde düğümleri Fortanx düğüm aracısına kaydetme

Bir Fortanx düğüm Aracısı oluşturmak, Azure Kaynak grubunuza bir sanal makine, ağ arabirimi, sanal ağ, ağ güvenlik grubu ve genel IP adresi dağıtır. Azure aboneliğiniz, sanal makine için saatlik olarak faturalandırılır. Bir Fortanx düğüm Aracısı oluşturmadan önce DCsv2 serisi için Azure [sanal makine fiyatlandırma sayfasını](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) gözden geçirin. Kullanımda olmadığında Azure kaynaklarını silin. 

1. [Azure Marketi](https://azuremarketplace.microsoft.com/marketplace/) ' ne gidin ve Azure kimlik bilgilerinizle oturum açın.
1. Arama çubuğuna **Fortanix gizli bilgi Işlem düğüm Aracısı**yazın. Teklifin giriş sayfasına gitmek için **Fortanix gizli bilgi Işlem düğüm Aracısı** adlı arama kutusunda görünen uygulamayı seçin. 
     ![Market 'te ara](media/how-to-fortanix-enclave-manager/search-fortanix-marketplace.png)
1. **Şimdi al**' ı seçin, gerekirse bilgilerinizi girin ve **devam**' ı seçin. Azure portal yönlendirilirsiniz. 
1. Fortanix gizli bilgi Işlem düğüm Aracısı dağıtım sayfasını girmek için **Oluştur** ' u seçin.
1. Bu sayfada, bir sanal makineyi dağıtmak için bilgi girersiniz. Özellikle, bu VM, Azure 'dan Fortanx düğüm Aracısı yazılımının yüklü olduğu DCsv2 serisi Intel SGX özellikli bir sanal makinedir. Düğüm Aracısı, dönüştürülmüş resminizin Azure 'da Intel SGX düğümlerinde güvenli şekilde çalışmasına izin verir.  Sanal makineyi ve ilişkili kaynakları dağıtmak istediğiniz **aboneliği** ve **kaynak grubunu** seçin. 
 
    > [!NOTE]
    > Azure 'da DCsv2 serisi sanal makineler dağıtımı sırasında kısıtlamalar vardır. Ek çekirdek için kota istemeniz gerekebilir. Daha fazla bilgi için [Azure VM 'lerinde gizli bilgi işlem çözümleri](https://docs.microsoft.com/azure/confidential-computing/virtual-machine-solution) hakkında bilgi edinin. 

1. Kullanılabilir bir bölge seçin.
1. **Düğüm adı** alanına sanal makineniz için bir ad girin. 
1. Sanal makinede kimlik doğrulaması için Kullanıcı adı ve parola (veya SSH anahtarı) girin.
1. Varsayılan işletim sistemi disk boyutunu 200 olarak bırakın ve bir VM boyutu seçin (Standard_DC4s_v2, bu öğreticide yeterli olacaktır)
1. Daha önce oluşturulan belirteci, **JOIN belirteci** alanında yapıştırın.

     ![kaynağı dağıt](media/how-to-fortanix-enclave-manager/deploy-fortanix-node-agent.png)

1. **Gözden geçir + oluştur**' u seçin. Doğrulamanın başarılı olduğundan emin olun ve ardından **Oluştur**' u seçin. Tüm kaynaklar dağıtıladıktan sonra, işlem düğümü şimdi şifreleme Yöneticisi 'ne kaydedilir. 

## <a name="run-the-application-image-on-the-compute-node"></a>Uygulama görüntüsünü işlem düğümünde Çalıştır
Aşağıdaki komutu yürüterek uygulamayı çalıştırın. Düğüm IP 'sini, bağlantı noktasını ve dönüştürülmüş görüntü adını, belirli uygulamanıza giriş olarak değiştirdiğinizden emin olun. 
 
Bu öğreticide, yürütülecek komut şu şekilde olur:     

```bash
    sudo docker run `
        --device /dev/isgx:/dev/isgx `
        --device /dev/gsgx:/dev/gsgx `
        -v /var/run/aesmd/aesm.socket:/var/run/aesmd/aesm.socket `
        -e NODE_AGENT_BASE_URL=http://52.152.206.164:9092/v1/ fortanix-private/python-flask-sgx
```

olmadığı 
- *52.152.206.164* , düğüm Aracısı ana bilgisayar IP 'si
- *9092* , düğüm aracısının dinlediği bağlantı noktasıdır
- *fortanx-Private/Python-Flask-SGX* , Fortanix Enclave Manage web portalındaki **Images** tablosunun **görüntü adı** sütununun altındaki görüntüler sekmesinde bulunan dönüştürülmüş bir uygulamadır. 
    
## <a name="verify-and-monitor-the-running-application"></a>Çalışan uygulamayı doğrulama ve izleme
1. [Fortanx Enclave Manager](https://em.fortanix.com/console) 'a geri dönme
1. Düğümü kaydettiğiniz **hesabın** içinde çalıştığınızdan emin olun
1. Sol gezinti bölmesindeki en üstteki simgeyi seçerek **Yönetim konsoluna** gidin. 
1. **Uygulama** sekmesini seçin
1. İlişkili bir işlem düğümü olan çalışan bir uygulama olduğunu doğrulayın


## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında kaynak grubunu, sanal makineyi ve ilişkili kaynakları silebilirsiniz. Kaynak grubunun silinmesi, dönüştürülmüş görüntünüz ile ilişkili düğümlerin kaydını kaldırır. 

Sanal makine için kaynak grubunu seçin ve **Sil**' i seçin. Kaynakları silmeyi tamamlayacak kaynak grubunun adını onaylayın. \

Oluşturduğunuz Fortanix Enclave Manager hesabını silmek için, şifreleme Yöneticisi ' nde [hesaplar sayfasına](https://em.fortanix.com/accounts) gidin. Silmek istediğiniz hesabın üzerine gelin. Sağ üst köşedeki dikey siyah noktaları seçin ve **Hesabı Sil**' i seçin.

  ![delete](media/how-to-fortanix-enclave-manager/delete-account.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, uygulama görüntünüzü gizli bir bilgi işlem sanal makinesinin en üstünde çalışacak şekilde dönüştürmek için fortanx Araçları öğesini kullandınız. Azure 'daki sanal makinelerin gizli bilgi işlem hakkında daha fazla bilgi için bkz. [sanal makinelerdeki çözümler](virtual-machine-solutions.md). 

Azure 'un gizli bilgi işlem teklifleri hakkında daha fazla bilgi edinmek için bkz. [Azure gizli bilgi Işlem Özeti](overview.md)

