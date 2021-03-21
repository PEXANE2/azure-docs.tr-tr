---
title: Azure Percept dev araçları paketi yükleyicisine genel bakış
description: Azure Percept ile gelişmiş geliştirmeyi hızlandırmak için geliştirme araçları paketi yükleyicisini kullanma hakkında daha fazla bilgi edinin
author: mimcco
ms.author: mimcco
ms.service: azure-percept
ms.topic: how-to
ms.date: 02/18/2021
ms.custom: template-how-to
ms.openlocfilehash: 66498fabadc0784a4a4ab1c3762daaaa9a5738c4
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102503224"
---
# <a name="dev-tools-pack-installer-overview"></a>Geliştirme araçları paketi yükleyicisine genel bakış

Geliştirme araçları paketi yükleyicisi, akıllı bir uç çözümü geliştirmek için gereken tüm araçları yükleyen ve yapılandıran tek bir-stop çözümüdür. Aşağıda listelenen yazılım paketlerinden herhangi birini zaten yüklediyseniz, araçlarınızın yükleyici yazılım sürümleriyle tutarlı olması için geliştirme araçları paketi yükleyicisi bu paketleri yeniden yükler.

## <a name="mandatory-tools-installed"></a>Zorunlu araçlar yüklendi

* [Visual Studio Code](https://code.visualstudio.com/)
* [Python 3,6 (Windows) veya 3,5 (Linux)](https://www.python.org/)
* [Docker 19,03](https://www.docker.com/)
* [PIP3](https://pip.pypa.io/en/stable/user_guide/)
* [TensorFlow 1,13](https://www.tensorflow.org/)
* [Azure Machine Learning SDK 1,1](https://docs.microsoft.com/python/api/overview/azure/ml/)

## <a name="optional-tools-available-for-installation"></a>Yükleme için kullanılabilen isteğe bağlı Araçlar

* [NVIDIA ayrıntılı AKıŞ SDK 5](https://developer.nvidia.com/deepstream-sdk) (NVIDIA Hızlandırıcılar için çözümler geliştirmek için araç seti)
* [Intel OpenVino Toolkit 2020,2](https://docs.openvinotoolkit.org/) (Intel hızlandırıcıları için çözüm geliştirmeye yönelik araç seti)
* [Lobe.ai](https://lobe.ai/)  
* [Streamaydınlatmalı](https://www.streamlit.io/)
* [Pytorch 1.4.0 (Windows) veya 1.2.0 (Linux)](https://pytorch.org/)
* [Miniconda3](https://docs.conda.io/en/latest/miniconda.html)
* [Chainer 5,2](https://chainer.org/)
* [Caffe](https://caffe.berkeleyvision.org/)
* [CUDA araç seti 10.0.130](https://developer.nvidia.com/cuda-toolkit)
* [Microsoft Cognitive Toolkit 2.5.1](https://www.microsoft.com/research/product/cognitive-toolkit/?lang=fr_ca)

## <a name="known-issues"></a>Bilinen Sorunlar

- Docker sistemde düzgün çalışmıyorsa isteğe bağlı Caffe yüklemesi başarısız olabilir. Caffe 'yi yüklemek isterseniz, geliştirme araçları paketi yükleyicisi aracılığıyla Caffe yüklemesine çalışmadan önce Docker 'ın yüklü olduğundan ve çalıştığından emin olun. 

- İsteğe bağlı CUDA yüklemesi uyumsuz sistemlerde başarısız olur. Dev araçları paketi yükleyicisi aracılığıyla [CUDA araç setini 10.0.130](https://developer.nvidia.com/cuda-toolkit) yüklemeye çalışmadan önce sistem uyumluluklarını doğrulayın.

## <a name="minimum-requirements"></a>Minimum gereksinimler

* Docker minimum gereksinimleri:

    * Windows:
        * https://docs.docker.com/docker-for-windows/install/#system-requirements

        - Windows 10 64-bit: Pro, Enterprise veya eğitim (derleme 16299 veya üzeri).

             Windows 10 Home için bkz. Windows Home 'da Docker Desktop 'ı Install.
           - Hyper-V ve kapsayıcılar Windows özelliklerinin etkinleştirilmesi gerekir.
           - Windows 10 ' da Istemci Hyper-V ' d e başarıyla çalıştırılması için aşağıdaki donanım önkoşulları gereklidir:

              - [Ikinci düzey adres çevirisi (SLAT)](https://en.wikipedia.org/wiki/Second_Level_Address_Translation) ile 64 bit işlemci
              - 4 GB sistem RAM 'i
              - BIOS düzeyinde donanım sanallaştırma desteğinin BIOS ayarları etkinleştirilmiş olması gerekir. Daha fazla bilgi için bkz. sanallaştırma.

        > [!NOTE]
        > Docker, Microsoft 'un Windows 10 işletim sistemi için destek yaşam döngüsünü temel alan Windows 'da Docker Desktop 'ı destekler. Daha fazla bilgi için bkz. [Windows yaşam döngüsü olgu sayfası](https://support.microsoft.com/help/13853/windows-lifecycle-fact-sheet).

    * Mac:
        * https://docs.docker.com/docker-for-mac/install/#system-requirements
       
        Docker Desktop 'ı başarıyla yüklemek için Mac 'nizin aşağıdaki gereksinimleri karşılaması gerekir:
         
         - Mac donanımı, genişletilmiş sayfa tabloları (EPT) ve Kısıtlanmamış mod da dahil olmak üzere, Intel işlemcisinin bellek yönetimi birimi (MMU) **sanallaştırmasına sahip bir 2010 veya daha yeni bir model olmalıdır**. Bir terminalde aşağıdaki komutu çalıştırarak makinenizin bu desteğe sahip olup olmadığını kontrol edebilirsiniz: ```sysctl kern.hv_support```

        Mac 'niz hiper yönetici çerçevesini destekliyorsa, komut yazdırılır ```kern.hv_support: 1``` .

         - **MacOS sürüm 10,14 veya daha yeni olmalıdır**. Yani, Mojave, Catalina veya Big Sur. MacOS 'un en son sürümüne yükseltmenizi öneririz.

        MacOS sürümünüzü sürüm 10,15 ' e yükselttikten sonra herhangi bir sorunla karşılaşırsanız, macOS 'un bu sürümü ile uyumlu olması için Docker Desktop 'ın en son sürümünü yüklemelisiniz.

        - En az 4 GB RAM.
        - Sürüm 4.3.30 öncesindeki VirtualBox, Docker Desktop ile uyumlu olmadığından yüklenmemelidir.

        > [!NOTE]
        > Docker, macOS 'un en son sürümlerinde Docker Desktop 'ı destekler. Yani, macOS 'un geçerli sürümü ve önceki iki sürüm. MacOS 'un yeni ana sürümleri genel kullanıma sunulduğunda, Docker en eski sürümü desteklemeyi durduruyor ve macOS 'ın en yeni sürümünü (önceki iki sürüme ek olarak) destekler. Docker Desktop Şu anda macOS Mojave, macOS Catalina ve macOS Big Sur 'i desteklemektedir.
        > 
        - Yükleyici, Apple M1 üzerinde desteklenmez.

## <a name="instructions"></a>Yönergeler

1. [Windows](https://go.microsoft.com/fwlink/?linkid=2132187), [Linux](https://go.microsoft.com/fwlink/?linkid=2132186)ve [Mac](https://go.microsoft.com/fwlink/?linkid=2132296)için geliştirme araçları paketi yükleyicisini indirin.

1. Platformunuza bağlı olarak, yükleyiciyi başlatırken bazı farklılıklar olacaktır.

    1. Windows için:
    
        1. Yükleme Sihirbazı 'nı açmak için **geliştirme-Araçlar-paketi-yükleyicisi** ' ne tıklayın.
        
    1. Mac için:
    
        1. İndirdikten sonra, Dev-Tools-Pack-Installer.app dosyasını uygulamalar klasörüne taşıyın.
        
        1. Yükleme Sihirbazı 'nı açmak için **dev-Tools-Pack-Installer.app** 'ye tıklayın.
        
        1. "Tanımlanamayan bir geliştirici" güvenlik iletişim kutusu alırsanız:
        
            1. Sistem Tercihleri-> güvenlik & gizlilik-> Genel ' e gidin ve "Dev-Tools-Pack-Installer.app" yanındaki "yine de aç" düğmesine tıklayın
        
            1. Dock 'ta yeniden elektron simgesine tıklayın
        
            1. Güvenlik iletişim kutusunda "Aç" düğmesine tıklayın
    
    1. Linux için:
    
        1. Tarayıcı tarafından istendiğinde, yükleyiciyi karşıdan yüklemeyi tamamlaması için "Kaydet" e tıklayın
        
        1. **. Appımage** dosyası Yöntem 1 ' e (CommandLine) yürütme izinleri ekleyin:
            
            1. Linux terminalini açma
            
            1. Yüklemeler klasörüne gitmek için terminaline aşağıdakini yazın
            
                1. CD ~/downloads/
                
            1. Appımage çalıştırılabilir dosyasını açmak için terminale aşağıdakini yazın
            
                1. chmod + x **geliştirme-Araçlar-paketi-yükleyici. Appımage**
                
            1. Yükleyiciyi çalıştırmak için terminalde şunları yazın
            
                1. ./Dev-\ımage-Pack-Installer.exe
        
        1. **. Appımage** dosyası yöntemi 2 ' ye (UI) yürütme izinleri ekleyin:
        
            1. . Appımage dosyasına sağ tıklayın ve Özellikler ' i seçin
            
            1. Izinleri aç sekmesi
            
            1. ' Program olarak dosya yürütmeye Izin ver ' kutusunu işaretleyin
            
            1. Özellikleri kapatın ve. appımage dosyasını açın

1. **Geliştirme araçları paketi yükleyicisini yükleme** sayfasında, yükleyicide bulunan her yazılım paketinin lisans sözleşmelerini görüntülemek Için **lisansı görüntüle** ' ye tıklayın. Lisans sözleşmeleri içindeki koşulları kabul ediyorsanız, kutuyu işaretleyin ve **İleri**' ye tıklayın.

    :::image type="content" source="./media/dev-tools-installer/dev-tools-license-agreements.png" alt-text="Yükleyicideki lisans sözleşmesi ekranı.":::

1. Microsoft gizlilik bildirimi ' ni gözden geçirmek için **Gizlilik bildirimi** ' ne tıklayın. Gizlilik bildirimi koşullarını kabul ediyorsanız ve Microsoft 'a Tanılama verileri göndermek istiyorsanız **Evet** ' i seçin ve **İleri**' ye tıklayın. Aksi takdirde **Hayır** ' ı seçin ve **İleri**' ye tıklayın.

    :::image type="content" source="./media/dev-tools-installer/dev-tools-privacy-statement.png" alt-text="Yükleyicideki gizlilik bildirimi sözleşmesi ekranı.":::

1. **Bileşenleri Yapılandır** sayfasında, yüklemek istediğiniz isteğe bağlı araçları seçin (varsayılan olarak, zorunlu araçlar yüklenir).

    1. Azure Percept DK 'nin bir parçası olan Azure Percept Audio SoM ile çalışıyorsanız, Intel OpenVino Toolkit ve Miniconda3 ' yi yüklediğinizden emin olun.

    1. Yüklemeye devam etmek için **yükleme** ' ye tıklayın.

    :::image type="content" source="./media/dev-tools-installer/dev-tools-configure-components.png" alt-text="Kullanılabilir yazılım paketlerini gösteren Yükleyici ekranı.":::

1. Seçilen tüm bileşenlerin başarılı bir şekilde yüklenmesinden sonra sihirbaz, **Kurulum Sihirbazı 'Nı tamamlama** sayfasına ilerler. Yükleyiciden çıkmak için **son** ' a tıklayın.

    :::image type="content" source="./media/dev-tools-installer/dev-tools-finish.png" alt-text="Yükleyici tamamlama ekranı.":::

## <a name="docker-status-check"></a>Docker durum denetimi

Yükleyici, Docker Desktop 'ın iyi çalışan bir durumda olduğunu doğrulamak için size bildirimde bulunursa aşağıdaki adımlara bakın:

   1. Windows:
   
      1. Sistem tepsisindeki gizli simgeleri Genişlet:
      
         1. Gizliyse sistem tepsisindeki gizli simgeler ' i Genişlet:

            :::image type="content" source="./media/dev-tools-installer/system-tray.png" alt-text="Sistem tepsisi.":::
         
         1. Docker masaüstü simgesinin ' Docker Desktop çalışıyor ' olduğunu doğrulayın:

            :::image type="content" source="./media/dev-tools-installer/docker-status-running.png" alt-text="Docker durumu.":::
         
         1. Sistem tepsisinde yukarıdaki simgeyi görmüyorsanız, Başlat menüsünden Docker Desktop ' ı başlatın.
         
         1. Docker yeniden başlatmanızı isterse, yükleyiciyi kapatmak ve yeniden başlatma tamamlandıktan sonra Docker çalışır durumda olduktan sonra yeniden başlatmak iyi olur. Başarıyla yüklenen üçüncü taraf uygulamaların algılanması ve otomatik olarak yeniden yüklenmesi gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Azure Percept DK için gelişmiş geliştirmeyi kullanmaya başlamak üzere [Azure Percept gelişmiş geliştirme deposuna](https://github.com/microsoft/azure-percept-advanced-development) göz atın.