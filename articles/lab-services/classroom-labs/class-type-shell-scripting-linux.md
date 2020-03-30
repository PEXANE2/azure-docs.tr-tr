---
title: Azure Lab Hizmetleri ile bir Linux kabuk komut dosyası laboratuvarı ayarlama | Microsoft Dokümanlar
description: Linux'ta kabuk komut dosyası yazmayı öğretmek için nasıl bir laboratuvar kurup kurup kurmayı öğrenin.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/30/2019
ms.author: spelluru
ms.openlocfilehash: 100a485588c77f6977001dae984b30ebcb1de557
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77443559"
---
# <a name="set-up-a-lab-to-teach-shell-scripting-on-linux"></a>Linux'ta kabuk komut dosyası öğretmek için bir laboratuvar ayarlayın
Bu makalede, Linux'ta kabuk komut dosyası öğretmek için nasıl bir laboratuvar kurabileceğinizi gösterilmektedir. Komut dosyası, yöneticilerin yinelenen görevlerden kaçınmasına olanak tanıyan sistem yönetiminin yararlı bir parçasıdır. Bu örnek senaryoda, sınıf geleneksel bash komut dosyalarını ve gelişmiş komut dosyalarını kapsar. Geliştirilmiş komut dosyaları bash komutları ve Ruby birleştiren komut dosyalarıvardır. Bu yaklaşım, Ruby'nin verileri çevreye aktarmasını ve komutların kabukla etkileşimkurmasını sağlar. 

Bu komut dosyası sınıflarını alan öğrenciler, Linux'un temellerini öğrenmek için bir Linux sanal makine olsun, ve aynı zamanda bash kabuk komut dosyası aşina olsun. Linux sanal makine uzak masaüstü erişimi etkin ve [gedit](https://help.gnome.org/users/gedit/stable/) ve [Visual Studio Code](https://code.visualstudio.com/) metin editörleri yüklü ile birlikte gelir.

## <a name="lab-configuration"></a>Laboratuvar yapılandırması
Bu laboratuarı kurmak için başlamak için bir Azure aboneliğine ihtiyacınız var. Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun. Azure aboneliğiniz olduktan sonra, Azure Laboratuvar Hizmetleri'nde yeni bir laboratuvar hesabı oluşturabilir veya varolan bir laboratuvar hesabını kullanabilirsiniz. Yeni bir laboratuvar hesabı oluşturmak için aşağıdaki [öğreticiye](tutorial-setup-lab-account.md)bakın: Laboratuvar Hesabı Kurma Öğreticisi.

Laboratuvar hesabını oluşturduktan sonra, laboratuvar hesabında aşağıdaki ayarları etkinleştirin: 

| Laboratuvar hesap ayarı | Yönergeler |
| ----------- | ------------ |  
| Pazar yeri görüntüleri | [Ubuntu Server 18.04 LTS](https://azuremarketplace.microsoft.com/marketplace/apps/canonical.0001-com-ubuntu-server-bionic) görüntüsünü laboratuvar hesabınızda kullanmak üzere etkinleştirin. Daha fazla bilgi için laboratuvar [oluşturucularının kullanabileceği Market görüntülerini belirt](specify-marketplace-images.md)ini görün. | 

Yeni bir laboratuvar oluşturmak ve aşağıdaki ayarları uygulamak için [bu öğreticiizleyin:](tutorial-setup-classroom-lab.md)

| Laboratuvar ayarları | Değer/talimatlar | 
| ------------ | ------------------ |
| Sanal makine (VM) boyutu | Küçük  |
| VM görüntü | [Ubuntu Server 18.04 LTS](https://azuremarketplace.microsoft.com/marketplace/apps/canonical.0001-com-ubuntu-server-bionic) |
| Uzak masaüstü bağlantısını etkinleştirme | Etkinleştirmek. <p>Bu ayarı etkinleştirmek, öğretmenlerin ve öğrencilerin uzak masaüstünü (RDP) kullanarak VM'lerine bağlanmasına olanak sağlar. Daha fazla bilgi için bkz: [Azure Lab Services'daki bir laboratuvarda Linux sanal makineleri için uzak masaüstünü etkinleştir.](how-to-enable-remote-desktop-linux.md#connect-to-the-template-vm) </p>|


## <a name="install-desktop-and-xrdp"></a>Masaüstü ve xrdp yükleme
[Ubuntu Server 18.04 LTS](https://azuremarketplace.microsoft.com/marketplace/apps/canonical.0001-com-ubuntu-server-bionic) görüntüsüvarsayılan olarak yüklü uzak masaüstü sunucusuna sahip değildir. Uzak masaüstü protokolü üzerinden bağlanmak için şablon makinesine gereken paketleri yüklemek için [Azure'daki bir Linux VM'ye bağlanmak için Uzak Masaüstü'nü Yükle ve yapılandırma](../../virtual-machines/linux/use-remote-desktop.md) yönergelerini izleyin.

## <a name="install-ruby"></a>Ruby’yi yükleme
Ruby bash komut dosyaları ile kombine edilebilir bir açık kaynak dinamik bir dildir. Bu bölümde `apt-get` [Ruby'nin](https://www.ruby-lang.org/)en son sürümünü yüklemek için nasıl kullanılacağı gösterilmektedir.

1. Aşağıdaki komutları çalıştırarak güncelleştirmeleri yükleyin:

    ```bash
    sudo apt-get update 
    sudo apt-get upgrade 
    ```
2.  [Ruby'yi](https://www.ruby-lang.org/)yükleyin.  Ruby bash komut dosyaları ile kombine edilebilir bir açık kaynak dinamik bir dildir. 
    
    ```bash
    sudo apt-get install ruby-full
    ```

## <a name="install-development-tools"></a>Geliştirme araçlarını yükleme
Bu bölümde, birkaç metin düzenleyicisinin nasıl yüklenirsiniz gösterilmektedir. Gedit gnome masaüstü ortamı için varsayılan metin düzenleyicisi. Genel amaçlı metin editörü olarak tasarlandı. Visual Studio Code hata ayıklama ve kaynak denetimi tümleştirme desteği içeren bir metin editörüdür.

> [!NOTE]
> Birkaç farklı metin editörleri mevcuttur. Visual Studio Code ve gedit sadece iki örnektir.

1. [Gedit yükleyin.](https://help.gnome.org/users/gedit/stable/)

    ```bash
    sudo apt-get install gedit
    ```
1. [Visual Studio Kodunu](https://code.visualstudio.com/)Yükleyin.  Visual Studio kodu Snap Store kullanılarak yüklenebilir.  Alternatif yükleme seçenekleri için [Visual Studio Code alternatif indirmelere](https://code.visualstudio.com/#alt-downloads)bakın.

    ```bash
    sudo snap install vscode --classic 
    ```

    Şablon artık güncelleştirildi ve laboratuarı tamamlamak için gereken programlama dili ve geliştirme araçlarına sahiptir. Şablon görüntüsü artık laboratuvarda yayımlanabilir. Şablonu laboratuvarda yayınlamak için şablon **sayfasındayayım** düğmesini seçin.  

## <a name="cost"></a>Maliyet 
Bu laboratuvarın maliyetini tahmin etmek isterseniz, aşağıdaki örneği kullanabilirsiniz:
 
20 saatlik planlanan ders süresi ve ödev veya ödevler için 10 saatlik kontenjanı olan 25 öğrenciden oluşan bir sınıf için, laboratuvar fiyatı: 

25 öğrenci * (20 + 10) saat * 20 Laboratuvar Birimi * 0,01 USD /saat = 150 USD

Fiyatlandırma hakkında daha fazla bilgi için aşağıdaki belgede bulunabilir: [Azure Lab Hizmetleri Fiyatlandırması.](https://azure.microsoft.com/pricing/details/lab-services/)

## <a name="conclusion"></a>Sonuç
Bu makalede, komut dosyası sınıfları için bir laboratuvar oluşturmak için adımlar boyunca yürüdü. Bu makalede, Linux makinesinde Ruby komut dosyası oluşturma araçları nın ayarlanması üzerinde durulsa da, aynı kurulum Linux'ta Python gibi diğer komut dosyası sınıfları için kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar
Sonraki adımlar herhangi bir laboratuvar kurmak için ortak:

- [Kullanıcı ekle](tutorial-setup-classroom-lab.md#add-users-to-the-lab)
- [Kota ayarlama](how-to-configure-student-usage.md#set-quotas-for-users)
- [Zamanlama ayarlama](tutorial-setup-classroom-lab.md#set-a-schedule-for-the-lab) 
- [Öğrencilere e-posta kayıt bağlantıları](how-to-configure-student-usage.md#send-invitations-to-users). 





