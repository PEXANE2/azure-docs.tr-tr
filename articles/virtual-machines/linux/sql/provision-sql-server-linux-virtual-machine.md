---
title: Azure'da Linux SQL Server 2017 VM oluşturma | Microsoft Docs
description: Bu öğreticide Azure portalında Linux SQL Server 2017 sanal makinesi oluşturma adımları gösterilmiştir.
services: virtual-machines-linux
author: MashaMSFT
manager: craigg
ms.date: 10/22/2019
tags: azure-service-management
ms.topic: conceptual
ms.service: virtual-machines-sql
ms.workload: iaas-sql-server
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 43ba4eed4dcfd6d8e86c21f1ee5214108c44a8c2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80060242"
---
# <a name="provision-a-linux-sql-server-virtual-machine-in-the-azure-portal"></a>Azure portalında bir Linux SQL Server sanal makinesi sağlama

> [!div class="op_single_selector"]
> * [Linux](provision-sql-server-linux-virtual-machine.md)
> * [Windows](../../windows/sql/virtual-machines-windows-portal-sql-server-provision.md)

Bu hızlı başlangıç öğreticisinde, SQL Server 2017 yüklü bir Linux sanal makinesi oluşturmak için Azure portalını kullanırsınız.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

* [Galeriden bir Linux SQL VM oluşturma](#create)
* [ssh ile yeni VM'ye bağlanma](#connect)
* [SA parolasını değiştirme](#password)
* [Uzak bağlantılar için yapılandırma](#remote)

## <a name="prerequisites"></a>Ön koşullar

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free) bir hesap oluşturun.

## <a name="create-a-linux-vm-with-sql-server-installed"></a><a id="create"></a> SQL Server yüklü bir Linux VM oluşturma

1. [Azure portalında](https://portal.azure.com/)oturum açın.

1. Sol bölmede **Kaynak oluştur**'u seçin.

1. Kaynak **oluştur** bölmesinde Bilgi **İşlem'i**seçin.

1. **Öne Çıkanlar** başlığının yanındaki **tümünü** gör'üni seçin.

   ![Tüm VM görüntülerini inceleme](./media/provision-sql-server-linux-virtual-machine/azure-compute-blade.png)

1. Arama kutusunda, **SQL Server 2019'u**yazın ve aramayı başlatmak için **Enter'u** seçin.

1. **İşletim sistemi** > **Redhat**seçerek arama sonuçlarını sınırlandırın.

    ![SQL Server 2019 VM görüntüleri için arama filtresi](./media/provision-sql-server-linux-virtual-machine/searchfilter.png)

1. Arama sonuçlarından bir SQL Server 2019 Linux görüntüsü seçin. Bu **öğretici, RHEL74'te SQL Server 2019'u**kullanır.

   > [!TIP]
   > Developer sürümü, Enterprise edition'ın özellikleriyle test etmenizi veya geliştirmenizi sağlar, ancak SQL Server lisanslama maliyeti yoktur. Yalnızca Linux VM çalıştırma maliyetleri için ödeme yaparsınız.

1. **Oluştur'u**seçin. 


### <a name="set-up-your-linux-vm"></a>Linux VM'nizi ayarlama

1. Temel **Bilgiler** sekmesinde, **Abonelik** ve **Kaynak Grubu'nuzu**seçin. 

    ![Temel bilgiler penceresi](./media/provision-sql-server-linux-virtual-machine/basics.png)

1. **Sanal makine adına,** yeni Linux VM'niz için bir ad girin.
1. Ardından, aşağıdaki değerleri yazın veya seçin:
   * **Bölge**: Size en uygun Azure bölgesini seçin.
   * **Kullanılabilirlik seçenekleri**: Uygulamalarınız ve verileriniz için en iyi kullanılabilirlik ve artıklık seçeneğini seçin.
   * **Boyutu değiştir**: Makine boyutunu seçmek için bu seçeneği seçin ve bittiğinde **Seç'i**seçin. VM boyutları hakkında daha fazla bilgi için bkz. [Linux VM boyutları](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-sizes).

     ![VM boyutu seçme](./media/provision-sql-server-linux-virtual-machine/vmsizes.png)

   > [!TIP]
   > Geliştirme ve işlevsel test için **DS2** veya daha yüksek bir VM boyutu kullanın. Performans testi için **DS13** veya üzeri kullanın.

   * **Kimlik doğrulama türü**: **SSH ortak anahtarını**seçin.

     > [!Note]
     > Kimlik doğrulaması için SSH ortak anahtarı veya Parola kullanabilirsiniz. SSH daha güvenlidir. SSH anahtarı oluşturma talimatları için bkz. [Azure'daki Linux VM için Linux ve Mac üzerinde SSH anahtarı oluşturma](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-mac-create-ssh-keys).

   * **Kullanıcı Adı**: VM için Yönetici adını girin.
   * **SSH ortak anahtarı**: RSA ortak anahtarınızı girin.
   * **Ortak gelen bağlantı noktaları**: **Seçili bağlantı noktalarına izin ver'i** seçin ve Genel gelen bağlantı **noktalarını seç** listesindeki **SSH (22)** bağlantı noktasını seçin. Bu hızlı başlatmada, SQL Server yapılandırmasını bağlamak ve tamamlamak için bu adım gereklidir. SQL Server'a uzaktan bağlanmak istiyorsanız, sanal makine oluşturulduktan sonra Internet üzerinden bağlantılar için Microsoft SQL Server tarafından kullanılan varsayılan bağlantı noktasına (1433) trafik izni vermeniz gerekir.

     ![Gelen bağlantı noktaları](./media/provision-sql-server-linux-virtual-machine/port-settings.png)

1. Aşağıdaki ek sekmelerde ayarlarda istediğiniz değişiklikleri yapın veya varsayılan ayarları koruyun.
    * **Disk**
    * **Ağ Oluşturma**
    * **Yönetim**
    * **Misafir config**
    * **Etiketler**

1. **İncele ve oluştur**’u seçin.
1. Gözden **Geçir + oluştur** bölmesinde **Oluştur'u**seçin.

## <a name="connect-to-the-linux-vm"></a><a id="connect"></a> Linux VM'ye bağlanma

BASH kabuğu kullanıyorsanız **ssh** komutuyla Azure VM'ye bağlanabilirsiniz. Aşağıdaki komutta yer alan VM kullanıcı adı ve IP adresini Linux VM bilgileriyle değiştirin.

```bash
ssh azureadmin@40.55.55.555
```

VM'nizin IP adresini Azure portalında bulabilirsiniz.

![Azure portalında IP adresi](./media/provision-sql-server-linux-virtual-machine/vmproperties.png)

Windows'da çalışıyorsanız ve BASH kabuğunuz yoksa, PuTTY gibi bir SSH istemcisi yükleyin.

1. [PuTTY'yi indirin ve yükleyin](https://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

1. PuTTY'yi çalıştırın.

1. PuTTY yapılandırma ekranına VM'nizin genel IP adresini girin.

1. **Aç'ı** seçin ve istemlerde kullanıcı adınızı ve şifrenizi girin.

Linux VM'lerinize bağlanma hakkında daha fazla bilgi için bkz. [Portal kullanarak Azure’da bir Linux VM oluşturma](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-quick-create-portal).

> [!Note]
> Sunucunun ana bilgisayar anahtarının kayıt defterinde önbelleğe alınmaması yla ilgili bir PuTTY güvenlik uyarısı görürseniz, aşağıdaki seçeneklerden birini seçin. Bu ana bilgisayara güveniyorsanız, PuTTy'nin önbelleğine anahtarı eklemek ve bağlanmaya devam etmek için **Evet'i** seçin. Önbelleğe anahtarı eklemeden yalnızca bir kez bağlanmaya devam etmek istiyorsanız, **Hayır'ı**seçin. Bu ana bilgisayara güvenmiyorsanız, bağlantıyı bırakmak için **İptal'i** seçin.

## <a name="change-the-sa-password"></a><a id="password"></a> SA parolasını değiştirme

Yeni sanal makine SQL Server'ı rastgele bir SA parolasıyla yükler. SA girişi ile SQL Server'a bağlanmadan önce bu parolayı sıfırlayın.

1. Linux VM'nize bağlandıktan sonra yeni bir komut terminali açın.

1. Aşağıdaki komutları kullanarak SA parolasını değiştirin:

   ```bash
   sudo systemctl stop mssql-server
   sudo /opt/mssql/bin/mssql-conf set-sa-password
   ```

   Sorulduğunda yeni bir SA parolası girin ve tekrar girerek onaylayın.

1. SQL Server hizmetini yeniden başlatın.

   ```bash
   sudo systemctl start mssql-server
   ```

## <a name="add-the-tools-to-your-path-optional"></a>Yolunuza araçları ekleme (isteğe bağlı)

Varsayılan olarak SQL Server komut satırı araçları paketi dahil olmak üzere birkaç SQL Server [paketi](sql-server-linux-virtual-machines-overview.md#packages) yüklenmiştir. Araçlar paketi **sqlcmd** ve **bcp** araçlarını içerir. Kolaylık sağlaması amacıyla araçlar yolu olan `/opt/mssql-tools/bin/` girişini **PATH** ortam değişkeninize ekleyebilirsiniz.

1. **PATH** ortam değişkenini hem oturum açma bilgileriyle başlatılan oturumları hem de etkileşimli/oturum açma bilgisi olmadan başlatılan oturumları için değiştirmek üzere aşağıdaki komutları çalıştırın:

   ```bash
   echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile
   echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
   source ~/.bashrc
   ```

## <a name="configure-for-remote-connections"></a><a id="remote"></a> Uzak bağlantılar için yapılandırma gerçekleştirme

Azure VM üzerindeki SQL Server'a uzaktan bağlanmanız gerekirse ağ güvenlik grubu üzerinde bir gelen kuralı yapılandırmanız gerekir. Kural SQL Server'ın dinlediği bağlantı noktasından (varsayılan olarak 1433) gelen trafiğe izin verir. Aşağıdaki adımlar, bu işlemi Azure portalından nasıl yapacağınızı göstermektedir.

> [!TIP]
> Sağlama sırasında ayarlarda **MS SQL (1433)** gelen bağlantı noktasını seçtiyseniz, bu değişiklikler sizin için yapılmış olur. Güvenlik duvarını yapılandırmayla ilgili sonraki bölme geçebilirsiniz.

1. Portalda **Sanal makineler**'i ve ardından SQL Server VM'nizi seçin.
1. Sol gezinti bölmesinde, **Ayarlar**altında **Ağ'ı**seçin.
1. Ağ penceresinde, **Gelen Bağlantı Noktası Kuralları**altında gelen bağlantı noktasını **ekle'yi** seçin.

   ![Gelen bağlantı noktası kuralları](./media/provision-sql-server-linux-virtual-machine/networking.png)

1. **Hizmet** listesinde **MS SQL** girişini seçin.

    ![MS SQL güvenlik grubu kuralı](./media/provision-sql-server-linux-virtual-machine/sqlnsgrule.png)

1. VM kuralını kaydetmek için **Tamam**'a tıklayın.

### <a name="open-the-firewall-on-rhel"></a>RHEL güvenlik duvarını açma

Bu öğreticide nasıl Red Hat Enterprise Linux (RHEL) VM oluşturacağınız gösterilmiştir. RHEL VM'lerine uzaktan bağlanmak isterseniz Linux güvenlik duvarı üzerindeki 1433 numaralı bağlantı noktasını da açmanız gerekir.

1. RHEL VM'nize [bağlanın](#connect).

1. BASH kabuğunda aşağıdaki komutları çalıştırın:

   ```bash
   sudo firewall-cmd --zone=public --add-port=1433/tcp --permanent
   sudo firewall-cmd --reload
   ```

## <a name="next-steps"></a>Sonraki adımlar

Azure'da bir SQL Server 2017 sanal makinesi oluşturdunuz. Artık **sqlcmd** ile yerel olarak bağlanıp Transact-SQL sorguları çalıştırabilirsiniz.

Uzak SQL Server bağlantıları için Azure VM'yi yapılandırmışsanız, uzaktan bağlanabilmelisin. Windows'dan Linux üzerindeki SQL Server'a uzaktan bağlantı kurma örneği için bkz. [Linux üzerindeki SQL Server'a bağlanmak için Windows üzerinde SSMS kullanma](https://docs.microsoft.com/sql/linux/sql-server-linux-develop-use-ssms). Visual Studio Code bağlantısı kurmak için bkz. [SQL Server için Transact-SQL betikleri oluşturma ve çalıştırma amacıyla Visual Studio Code'u kullanma](https://docs.microsoft.com/sql/linux/sql-server-linux-develop-use-vscode)

Linux'ta SQL Server hakkında daha fazla bilgi için [Linux'ta SQL Server 2017'ye Genel Bakış bölümüne](https://docs.microsoft.com/sql/linux/sql-server-linux-overview)bakın. SQL Server 2017 Linux sanal makinelerini kullanma hakkında daha fazla bilgi için bkz. [Azure'daki SQL Server 2017 sanal makinelerine genel bakış](sql-server-linux-virtual-machines-overview.md).
