---
title: Windows Sanal Masaüstü için GPU yapılandırın - Azure
description: Windows Sanal Masaüstü'nde GPU hızlandırılmış işleme ve kodlama nasıl etkinleştirilir.
services: virtual-desktop
author: gundarev
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: denisgun
ms.openlocfilehash: 941a1ff23668a3202028e8b693b57d902095b3b2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78384816"
---
# <a name="configure-graphics-processing-unit-gpu-acceleration-for-windows-virtual-desktop"></a>Windows Sanal Masaüstü için grafik işleme birimi (GPU) ivmesi yapılandırma

Windows Sanal Masaüstü, gelişmiş uygulama performansı ve ölçeklenebilirlik için GPU hızlandırılmış görüntüleme ve kodlamayı destekler. GPU ivmesi özellikle grafik yoğun uygulamalar için çok önemlidir.

GPU için optimize edilmiş bir Azure sanal makinesi oluşturmak, ana bilgisayar havuzunuza eklemek ve işleme ve kodlama için GPU ivmesini kullanacak şekilde yapılandırmak için bu makaledeki yönergeleri izleyin. Bu makalede, zaten yapılandırılmış bir Windows Sanal Masaüstü kiracı var varsayar.

## <a name="select-a-gpu-optimized-azure-virtual-machine-size"></a>GPU için optimize edilmiş Azure sanal makine boyutu seçin

Azure, [gpu için optimize edilmiş sanal makine boyutları](/azure/virtual-machines/windows/sizes-gpu)sunar. Ev sahibi havuzunuz için doğru seçim, belirli uygulama iş yükleri, istenen kullanıcı deneyimi kalitesi ve maliyet gibi bir dizi etkene bağlıdır. Genel olarak, daha büyük ve daha yetenekli GPU'lar belirli bir kullanıcı yoğunluğunda daha iyi bir kullanıcı deneyimi sunar.

## <a name="create-a-host-pool-provision-your-virtual-machine-and-configure-an-app-group"></a>Ana bilgisayar havuzu oluşturun, sanal makinenizi sağlama ve uygulama grubunu yapılandırma

Seçtiğiniz boyutun VM'sini kullanarak yeni bir ana bilgisayar havuzu oluşturun. Talimatlar için [Bkz. Öğretici: Azure Marketi ile bir ana bilgisayar havuzu oluşturun.](/azure/virtual-desktop/create-host-pools-azure-marketplace)

Windows Sanal Masaüstü, aşağıdaki işletim sistemlerinde GPU hızlandırılmış görüntüleme ve kodlamayı destekler:

* Windows 10 sürüm 1511 veya daha yeni
* Windows Server 2016 veya daha yenisi

Ayrıca bir uygulama grubu yapılandırmanız veya yeni bir ana bilgisayar havuzu oluşturduğunuzda otomatik olarak oluşturulan varsayılan masaüstü uygulama grubunu ("Masaüstü Uygulama Grubu" olarak adlandırılır) kullanmanız gerekir. Talimatlar için [Bkz. Öğretici: Windows Sanal Masaüstü için uygulama gruplarını yönetin.](/azure/virtual-desktop/manage-app-groups)

## <a name="install-supported-graphics-drivers-in-your-virtual-machine"></a>Desteklenen grafik sürücülerini sanal makinenize yükleme

Windows Sanal Masaüstü'ndeki Azure N serisi VM'lerin GPU özelliklerinden yararlanmak için uygun grafik sürücülerini yüklemeniz gerekir. Sürücüleri el ile veya Azure VM uzantısı kullanarak uygun grafik satıcısından yüklemek için [Desteklenen işletim sistemleri ve sürücülerdeki](/azure/virtual-machines/windows/sizes-gpu#supported-operating-systems-and-drivers) yönergeleri izleyin.

Yalnızca Azure tarafından dağıtılan sürücüler Windows Sanal Masaüstü için desteklenir. Ayrıca, NVIDIA GPU'lu Azure VM'ler için Windows Sanal Masaüstü için yalnızca [NVIDIA GRID sürücüleri](/azure/virtual-machines/windows/n-series-driver-setup#nvidia-grid-drivers) desteklenir.

Sürücü yüklemeden sonra VM yeniden başlatma gerekir. Grafik sürücülerinin başarıyla yüklenmiş olduğunu doğrulamak için yukarıdaki yönergelerde doğrulama adımlarını kullanın.

## <a name="configure-gpu-accelerated-app-rendering"></a>GPU hızlandırılmış uygulama oluşturmayı yapılandırın

Varsayılan olarak, çok oturumlu yapılandırmalarda çalışan uygulamalar ve masaüstü bilgisayarlar CPU ile birlikte işlenir ve işleme için kullanılabilir GPU'lardan yararlanmaz. GPU hızlandırılmış işlemeyi etkinleştirmek için oturum ana bilgisayarı için Grup İlkesi'ni yapılandırın:

1. Yerel yönetici ayrıcalıklarına sahip bir hesabı kullanarak VM masaüstüne bağlanın.
2. Başlat menüsünü açın ve Grup İlkesi Düzenleyicisi'ni açmak için "gpedit.msc" yazın.
3. Ağacı Bilgisayar **Yapılandırma** > **Yönetim Şablonlarına** > gidin**Windows Bileşenleri** > **Uzak Masaüstü Hizmetleri** > **Uzak Masaüstü Oturum Ana Bilgisayar** > **Uzak Oturum Ortamı**.
4. İlke yi seçin **Tüm Uzak Masaüstü Hizmetleri oturumları için donanım varsayılan grafik bağdaştırıcısını kullanın** ve uzak oturumda GPU oluşturmayı etkinleştirmek için bu ilkeyi **Etkin** olarak ayarlayın.

## <a name="configure-gpu-accelerated-frame-encoding"></a>GPU hızlandırılmış çerçeve kodlamayı yapılandırma

Uzak Masaüstü, uzak masaüstü istemcilerine iletim için uygulamalar ve masaüstü bilgisayarlar tarafından işlenen tüm grafikleri (GPU veya CPU ile işlenmiş olsun) kodlar. Varsayılan olarak, Uzak Masaüstü bu kodlama için kullanılabilir GPU'dan yararlanmaz. GPU hızlandırılmış çerçeve kodlamasını etkinleştirmek için oturum ana bilgisayarı için Grup İlkesi'ni yapılandırın. Yukarıdaki adımları devam:

1. Uzak **Masaüstü bağlantıları için H.264/AVC 444 Grafik modunu önceliklendirme** ilkesini seçin ve bu ilkeyi uzak oturumda H.264/AVC 444 codec'i zorlamak için **Etkin** olarak ayarlayın.
2. Uzak **Masaüstü bağlantıları için H.264/AVC donanım kodlamasını yapılandır'ı** seçin ve uzak oturumda AVC/H.264 için donanım kodlamasını etkinleştirmek için bu ilkeyi **Etkin** olarak ayarlayın.

    >[!NOTE]
    >Windows Server 2016'da, **Her Zaman denemek**için **AVC Donanım Kodlamasını Tercih Etme** seçeneğini ayarlayın.

3. Grup ilkeleri düzenlendiğine göre, grup ilkesi güncelleştirmesini zorleyin. Komut İstemi'ni açın ve yazın:

    ```batch
    gpupdate.exe /force
    ```

4. Uzak Masaüstü oturumundan oturumu niçin tamamlayın.

## <a name="verify-gpu-accelerated-app-rendering"></a>GPU hızlandırılmış uygulama oluşturmayı doğrulayın

Uygulamaların görüntüleme için GPU kullandığını doğrulamak için aşağıdakilerden birini deneyin:

* NVIDIA GPU'lu Azure VM'ler için, uygulamalarınızı çalıştırırken GPU kullanımını kontrol etmek için sürücü `nvidia-smi` [yüklemesini doğrula'da](/azure/virtual-machines/windows/n-series-driver-setup#verify-driver-installation) açıklandığı gibi yardımcı programı kullanın.
* Desteklenen işletim sistemi sürümlerinde, GPU kullanımını denetlemek için Görev Yöneticisi'ni kullanabilirsiniz. Uygulamaların GPU'yu kullanıp kullanmadığını görmek için "Performans" sekmesinde GPU'yu seçin.

## <a name="verify-gpu-accelerated-frame-encoding"></a>GPU hızlandırılmış çerçeve kodlamadoğru

Uzak Masaüstü'nin GPU hızlandırılmış kodlama kullandığını doğrulamak için:

1. Windows Sanal Masaüstü istemcisini kullanarak VM masaüstüne bağlanın.
2. Olay Görüntüleyici'yi başlatın ve aşağıdaki düğüme gidin: **Uygulamalar ve Hizmetler Günlükleri** > **Microsoft** > **Windows** > **RemoteDesktopServices-RdpCoreCDV** > **Operasyonel**
3. GPU hızlandırılmış kodlamanın kullanılıp kullanılmamasını belirlemek için olay ID 170' i arayın. "AVC donanım kodlayıcısı etkin: 1" görürseniz, GPU kodlaması kullanılır.
4. AVC 444 modunun kullanılıp kullanılmamasını belirlemek için olay ID 162' yi arayın. "AVC Kullanılabilir: 1 Başlangıç Profili: 2048" görürseniz, AVC 444 kullanılır.

## <a name="next-steps"></a>Sonraki adımlar

Bu talimatlar, tek bir oturum ana bilgisayar VM'de GPU ivmesi ile çalışmaya devam edebilirsiniz. Daha büyük bir ana bilgisayar havuzunda GPU ivmesini etkinleştirmek için bazı ek hususlar:

* Sürücü yüklemesini ve birkaç VM'deki güncelleştirmeleri basitleştirmek için bir [VM uzantısı](/azure/virtual-machines/extensions/overview) kullanmayı düşünün. NVIDIA GPU'lu VM'ler için [NVIDIA GPU Sürücü Uzantısı'nı](/azure/virtual-machines/extensions/hpccompute-gpu-windows) kullanın ve AMD GPU'lu VM'ler için AMD GPU Sürücü Uzantısı'nı (yakında) kullanın.
* Grup ilkesi yapılandırmasını birkaç VM'de basitleştirmek için Etkin Dizin Grubu İlkesi'ni kullanmayı düşünün. Etkin Dizin etki alanında Grup İlkesi dağıtma hakkında bilgi için [bkz.](https://go.microsoft.com/fwlink/p/?LinkId=620889)
