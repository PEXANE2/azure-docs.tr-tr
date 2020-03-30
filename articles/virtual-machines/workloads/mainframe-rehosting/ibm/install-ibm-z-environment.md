---
title: Azure'da IBM zD&T geliştirme/test ortamını yükleyin | Microsoft Dokümanlar
description: IBM Z Geliştirme ve Test Ortamını (zD&T) Azure Sanal Makine (VM) altyapısına hizmet olarak (IaaS) dağıtın.
services: virtual-machines-linux
ms.service: virtual-machines-linux
documentationcenter: ''
author: njray
ms.author: edprice
manager: edprice
editor: edprice
ms.topic: conceptual
ms.date: 04/02/2019
tags: ''
keywords: ''
ms.openlocfilehash: 67af4eae03b773fad9cf38964152c1fb9d623bd5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72025945"
---
# <a name="install-ibm-zdt-devtest-environment-on-azure"></a>Azure'da IBM zD&T geliştirme/test ortamını yükleme

IBM Z Systems'deki ana bilgisayar iş yükleri için bir geliştirme/test ortamı oluşturmak için, IBM Z Geliştirme ve Test Ortamını (zD&T) Azure Sanal Makine (VM) altyapısına hizmet olarak dağıtabilirsiniz (IaaS).

zD&T ile, daha az kritik geliştirme ve test ortamlarınız için x86 platformunun maliyet tasarruflarından yararlanabilir ve güncellemeleri Z Sistemi üretim ortamına geri itebilirsiniz. Daha fazla bilgi için [IBM ZD&T yükleme yönergelerine](https://www-01.ibm.com/support/docview.wss?uid=swg24044565#INSTALL)bakın.

Azure ve Azure Yığını aşağıdaki sürümleri destekler:

- zD&T Personal Edition
- zD&T Paralel Sysplex
- zD&T Enterprise Edition

zD&T'nin tüm sürümleri Windows Server'da değil, yalnızca x86 Linux sistemlerinde çalışır. Enterprise Edition, Red Hat Enterprise Linux (RHEL) veya Ubuntu/Debian'da desteklenir. Azure için hem RHEL hem de Debian VM görüntüleri kullanılabilir.

Bu makalede, zD&T Enterprise Edition'ı Azure'da nasıl ayarlayabileceğinizi gösterir, böylece ortamların oluşturulması ve yönetilmesi için zD&T Enterprise Edition web sunucusunu kullanabilirsiniz. zD&T'nin yüklenmesi herhangi bir ortam yüklemez. Bunları yükleme paketleri olarak ayrı olarak oluşturmanız gerekir. Örneğin, Uygulama Geliştiricileri Kontrollü Dağıtımlar (ADCD) test ortamlarının toplu görüntüleridir. Bunlar, IBM'in medya dağıtımında yer alan zip görüntülerinde bulunur. [Azure'da bir ADCD ortamının nasıl ayarlanır](demo.md)olduğunu görün.

Daha fazla bilgi için IBM Bilgi Merkezi'ndeki [zD&T Genel Bakışı'na](https://www.ibm.com/support/knowledgecenter/en/SSTQBD_12.0.0/com.ibm.zdt.overview.gs.doc/topics/c_product_overview.html) bakın.

Bu makalede, Azure'da Z Geliştirme ve Test Ortamı (zD&T) Enterprise Edition'ı nasıl ayarlayacağınızı gösterilmektedir. Ardından ZD&T Enterprise Edition web sunucusunu kullanarak Azure'da Z tabanlı ortamlar oluşturabilir ve yönetebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

> [!NOTE]
> IBM, zD&T Enterprise Edition'ın üretim ortamlarına*değil,* yalnızca dev/test ortamlarına yüklenmesini sağlar.

- Azure aboneliği. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

- Yalnızca IBM müşterileri ve iş ortakları tarafından kullanılabilen ortama erişmeniz gerekir. Daha fazla bilgi için IBM temsilcinize başvurun veya [zD&T](https://www.ibm.com/us-en/marketplace/z-systems-development-test-environment) web sitesindeki iletişim bilgilerine bakın.

- Bir [lisans sunucusu.](https://www.ibm.com/support/knowledgecenter/en/SSTQBD_12.0.0/com.ibm.zsys.rdt.tools.user.guide.doc/topics/zdt_ee.html) Bu ortamlara erişim için gereklidir. Yazılımı oluşturma şekliniz, yazılımı IBM'den nasıl lisansettiğinize bağlıdır:

     - **Donanım tabanlı lisanslama sunucusu,** yazılımın tüm bölümlerine erişmek için gerekli Olan Rasyonel Belirteçleri içeren bir USB donanım aygıtı gerektirir. Bunu IBM'den almanız gerekir.

     - **Yazılım tabanlı lisanslama sunucusu,** lisans anahtarlarının yönetimi için merkezi bir sunucu ayarlamanızı gerektirir. Bu yöntem tercih edilir ve ibm'den aldığınız anahtarları yönetim sunucusunda ayarlamanızı gerektirir.

## <a name="create-the-base-image-and-connect"></a>Temel görüntüyü oluşturun ve bağlanın

1. Azure portalında, istediğiniz işletim sistemi yapılandırmasıyla [bir VM oluşturun.](/azure/virtual-machines/linux/quick-create-portal) Bu makalede, Ubuntu 16.04 çalıştıran bir B4ms VM (4 vCPUs ve 16 GB bellek ile) varsayar.

2. VM oluşturulduktan sonra, SSH için 22, FTP için 21 ve web sunucusu için 9443 gelen bağlantı noktalarını açın.

3. **Bağlan** düğmesi aracılığıyla VM'nin **Genel Bakış** bıçağında gösterilen SSH kimlik bilgilerini alın. **SSH** sekmesini seçin ve SSH oturum açma komutunu panoya kopyalayın.

4. Yerel bilgisayarınızdan bir [Bash kabuğuna](/azure/cloud-shell/quickstart) oturum açın ve komutu yapıştırın. Bu formda **ssh\<kullanıcı kimliği\>\@\<IP\>Adresi**olacaktır. Kimlik bilgileriniz için istendiğinde, Ev dizininize bağlantı kurmak için bunları girin.

## <a name="copy-the-installation-file-to-the-server"></a>Yükleme dosyasını sunucuya kopyalama

Web sunucusu için yükleme dosyası **ZDT\_Install\_EE\_V12.0.0.1.tgz**. IBM tarafından sağlanan ortama dahildir. Bu dosyayı Ubuntu VM'nize yüklemeniz gerekir.

1. Komut satırından, yeni oluşturulan resimde her şeyin güncel olduğundan emin olmak için aşağıdaki komutu girin:

    ```
    sudo apt-get update
    ```

2. Yüklemek için dizin oluşturun:

    ```
    mkdir ZDT
    ```

3. Dosyayı yerel makinenizden VM'ye kopyalayın:

    ```
    scp ZDT_Install_EE_V12.0.0.1.tgz  your_userid@<IP Address /ZDT>   =>
    ```
    
> [!NOTE]
> Bu komut, yükleme dosyasını, istemcinizin Windows veya Linux çalıştırıp çalıştırmadığına bağlı olarak değişen, Ev dizininizdeki ZDT dizinine kopyalar.

## <a name="install-the-enterprise-edition"></a>Enterprise Edition'ı yükleyin

1. ZDT dizinine gidin ve Aşağıdaki komutları\_\_kullanarak\_ZDT Install EE V12.0.0.1.tgz dosyasını dekomprese edin:

    ```
    cd ZDT
    chmod 755 ZDT\_Install\_EE\_V12.0.0.0.tgz
    ```

2. Yükleyiciyi çalıştırın:

    ```
    ./ZDT_Install_EE_V12.0.0.0.x86_64
    ```

3. Enterprise Server'ı yüklemek için **1'i** seçin.

4. Lisans sözleşmelerini **girin** ve dikkatle okuyun. Lisansın sonunda, devam etmek için **Evet** girin.

5. Yeni oluşturulan kullanıcı için şifreyi değiştirmek istendiğinde, **ibmsys1**, **komut sudo passwd ibmsys1** kullanın ve yeni şifregirin.

6. Yüklemenin başarılı olup olmadığını doğrulamak için girin

    ```
    dpkg -l | grep zdtapp
    ```

7. Çıktının **zdtapp 12.0.0.0**dizesini içerdiğini doğrulayın, paket gazının başarıyla kurulduğunu belirten

### <a name="starting-enterprise-edition"></a>Kurumsal Sürümü Başlatma

Web sunucusu başlatıldığında, yükleme işlemi sırasında oluşturulan zD&T kullanıcı kimliği altında çalıştığını unutmayın.

1. Web sunucusunu başlatmak için aşağıdaki komutu çalıştırmak için kök Kullanıcı Kimliğini kullanın:

    ```
    sudo /opt/ibm/zDT/bin/startServer
    ```

2. URL çıktısını aşağıdaki gibi görünen komut dosyasına göre kopyalayın:

    ```
    https://<your IP address or domain name>:9443/ZDTMC/login.htm
    ```

3. zD&T yüklemenizin yönetim bileşenini açmak için URL'yi bir web tarayıcısına yapıştırın.

## <a name="next-steps"></a>Sonraki adımlar

[IBM zD&T v1'de Uygulama Geliştiricileri Kontrollü Dağıtım (ADCD) kurma](./demo.md)
