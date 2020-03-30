---
title: Özel Bağlantı - Azure portalı - MariaDB için Azure Veritabanı
description: Azure portalından MariaDB için Azure Veritabanı için özel bağlantıyı nasıl yapılandırabilirsiniz öğrenin
author: kummanish
ms.author: manishku
ms.service: mariadb
ms.topic: conceptual
ms.date: 01/09/2020
ms.openlocfilehash: 3f421cad64caf91b898bb1ec13dc909b93b7f72d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79370347"
---
# <a name="create-and-manage-private-link-for-azure-database-for-mariadb-using-portal"></a>Portal'ı kullanarak MariaDB için Azure Veritabanı için Özel Bağlantı oluşturma ve yönetme

Özel Bitiş Noktası, Azure'daki özel bağlantının temel yapı taşıdır. Sanal Makineler (VM' ler) gibi Azure kaynaklarının özel bağlantı kaynaklarıyla özel olarak iletişim kurmasını sağlar.  Bu makalede, Azure Sanal Ağ'da VM ve Azure özel bitiş noktasına sahip MariaDB sunucusu için bir Azure Veritabanı oluşturmak için Azure portalını nasıl kullanacağınızı öğreneceksiniz.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

> [!NOTE]
> Bu özellik, MariaDB için Azure Veritabanı'nın Genel Amaç ve Bellek Optimize Edilmiş fiyatlandırma katmanlarını desteklediği tüm Azure bölgelerinde kullanılabilir.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma
[Azure portalında](https://portal.azure.com)oturum açın.

## <a name="create-an-azure-vm"></a>Azure VM oluşturma

Bu bölümde, Özel Bağlantı kaynağınıza (Azure'daki MariaDB sunucusu) erişmek için kullanılan VM'yi barındırmak için sanal ağ ve alt ağ oluşturursunuz.

### <a name="create-the-virtual-network"></a>Sanal ağ oluşturma
Bu bölümde, Özel Bağlantı kaynağınıza erişmek için kullanılan VM'yi barındıracak bir Sanal Ağ ve alt ağ oluşturursunuz.

1. Ekranın sol üst tarafında,**Networking** > **Sanal ağ**ağı ağı **oluştur'u** > seçin.
2. **Sanal ağ oluştur'da**bu bilgileri girin veya seçin:

    | Ayar | Değer |
    | ------- | ----- |
    | Adı | *MyVirtualNetwork*girin. |
    | Adres alanı | *10.1.0.0/16*girin. |
    | Abonelik | Aboneliğinizi seçin.|
    | Kaynak grubu | **Yeni Oluştur'u**seçin, *myResourceGroup'u*girin, ardından **Tamam'ı**seçin. |
    | Konum | **Batı Avrupa**'yı seçin.|
    | Subnet - Adı | *mySubnet*girin. |
    | Alt Ağ - Adres aralığı | *10.1.0.0/24*girin. |
    |||
3. Geri sini varsayılan olarak bırakın ve **Oluştur'u**seçin.

### <a name="create-virtual-machine"></a>Sanal Makine Oluştur

1. Azure portalında ekranın sol üst tarafında bir **kaynak** > **Oluştur** > **Sanal Makine**oluştur'u seçin.

2. Sanal makine Oluştur 'da **- Temel bilgiler,** bu bilgileri girin veya seçin:

    | Ayar | Değer |
    | ------- | ----- |
    | **PROJE DETAYLARI** | |
    | Abonelik | Aboneliğinizi seçin. |
    | Kaynak grubu | **MyResourceGroup'u**seçin. Bunu önceki bölümde oluşturdunuz.  |
    | **ÖRNEK DETAYLAR** |  |
    | Sanal makine adı | *myVm*girin. |
    | Bölge | **Batı Avrupa**'yı seçin. |
    | Kullanılabilirlik seçenekleri | Varsayılan **Bırak Altyapı artıklığı gerekli değil.** |
    | Görüntü | **Windows Server 2019 Datacenter'ı**seçin. |
    | Boyut | Varsayılan **Standart DS1 v2**bırakın. |
    | **YÖNETICI Hesabı** |  |
    | Kullanıcı adı | Seçtiğiniz bir kullanıcı adı girin. |
    | Parola | Seçtiğiniz bir parolayı girin. Parola en az 12 karakter uzunluğunda olmalı ve [tanımlanmış karmaşıklık gereksinimlerini](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm) karşılamalıdır.|
    | Parolayı Onayla | Parolayı yeniden girin. |
    | **GELEN BAĞLANTı NOKTASı Kuralları** |  |
    | Genel gelen bağlantı noktaları | Varsayılan **Yok'u**bırakın. |
    | **TASARRUF EDIN** |  |
    | Zaten bir Windows lisansınız var mı? | Varsayılan **No'yu**bırakın. |
    |||

1. **Sonraki'ni seçin: Diskler.**

1. **Sanal makine Oluştur 'da - Diskler,** varsayılanları bırakın ve **Sonraki: Ağ'** ı seçin.

1. **Sanal makine Oluştur 'da - Ağ oluşturma**, bu bilgileri seçin:

    | Ayar | Değer |
    | ------- | ----- |
    | Sanal ağ | Varsayılan **MyVirtualNetwork**bırakın.  |
    | Adres alanı | Varsayılan **10.1.0.0/24**bırakın.|
    | Alt ağ | Varsayılan **mySubnet'i (10.1.0.0/24)** bırakın.|
    | Genel IP | Varsayılan **(yeni) myVm-ip**bırakın. |
    | Genel gelen bağlantı noktaları | **Seçili bağlantı noktalarına izin ver'i**seçin. |
    | Gelen bağlantı noktalarını seçme | **HTTP** ve **RDP'yi**seçin.|
    |||


1. **İncele ve oluştur**’u seçin. Azure'un yapılandırmanızı doğruladığı **Gözden Geçirme + oluşturma** sayfasına götürülürnüz.

1. **Validation geçirilen** iletiyi gördüğünüzde **Oluştur'u**seçin.

## <a name="create-an-azure-database-for-mariadb"></a>MariaDB için Azure Veritabanı oluşturma

Bu bölümde, Azure'da MariaDB sunucusu için bir Azure Veritabanı oluşturursunuz. 

1. Azure portalında ekranın sol üst tarafında,**MariaDB için**kaynak > **Veritabanları** > Oluştur Azure **Veritabanı'nı**seçin.

1. **MariaDB için Azure Veritabanı'nda** şu bilgileri sağlayın:

    | Ayar | Değer |
    | ------- | ----- |
    | **Proje ayrıntıları** | |
    | Abonelik | Aboneliğinizi seçin. |
    | Kaynak grubu | **MyResourceGroup'u**seçin. Bunu önceki bölümde oluşturdunuz.|
    | **Sunucu ayrıntıları** |  |
    |Sunucu adı  | *Sunucumu*girin. Bu ad alınırsa, benzersiz bir ad oluşturun.|
    | Yönetici kullanıcı adı| Seçtiğiniz bir yönetici adını girin. |
    | Parola | Seçtiğiniz bir parolayı girin. Parola en az 8 karakter uzunluğunda olmalı ve tanımlanan gereksinimleri karşılamalıdır. |
    | Konum | MariaDB Sunucunuzun ikamet etmesini istediğiniz bir Azure bölgesi seçin. |
    |Sürüm  | Gerekli olan MariaDB sunucusunun veritabanı sürümünü seçin.|
    | İşlem + Depolama| İş yükünü temel alan sunucu için gereken fiyatlandırma katmanını seçin. |
    |||
 
7. **Tamam'ı**seçin. 
8. **İncele ve oluştur**’u seçin. Azure'un yapılandırmanızı doğruladığı **Gözden Geçirme + oluşturma** sayfasına götürülürnüz. 
9. Validation geçirilen iletiyi gördüğünüzde **Oluştur'u**seçin. 
10. Doğrulama geçti iletisini gördüğünüzde Oluştur'u seçin. 

## <a name="create-a-private-endpoint"></a>Özel uç nokta oluşturma

Bu bölümde, MariaDB sunucusuiçin özel bir bitiş noktası oluşturacaksınız. 

1. Azure portalında ekranın sol üst tarafında, > Kaynak**Ağı** > **Özel Bağlantısı** **Oluştur'u**seçin.
2. **Özel Bağlantı Merkezi**'nde - Genel Bakış , bir **hizmete özel bağlantı kurma**seçeneğine ilişkin Olarak **Başlat'ı**seçin.

    ![Özel Bağlantıgenel bakış](media/concepts-data-access-and-security-private-link/privatelink-overview.png)

1. **Özel bir bitiş noktası oluşturma - Temel bilgiler**, bu bilgileri girin veya seçin:

    | Ayar | Değer |
    | ------- | ----- |
    | **Proje ayrıntıları** | |
    | Abonelik | Aboneliğinizi seçin. |
    | Kaynak grubu | **MyResourceGroup'u**seçin. Bunu önceki bölümde oluşturdunuz.|
    | **Örnek Ayrıntıları** |  |
    | Adı | *MyPrivateEndpoint'i*girin. Bu ad alınırsa, benzersiz bir ad oluşturun. |
    |Bölge|**Batı Avrupa**'yı seçin.|
    |||
5. **Sonraki seçin: Kaynak**.
6. **Özel bir bitiş noktası oluştur - Kaynak**, bu bilgileri girin veya seçin:

    | Ayar | Değer |
    | ------- | ----- |
    |Bağlantı yöntemi  | Dizinimdeki bir Azure kaynağına bağlan'ı seçin.|
    | Abonelik| Aboneliğinizi seçin. |
    | Kaynak türü | **Microsoft.DBforMariaDB/servers'ı**seçin. |
    | Kaynak |*myServer'ı* seçin|
    |Hedef alt kaynak |*mariadbServer'ı* seçin|
    |||
7. **Sonraki'ni seçin: Yapılandırma.**
8. **Özel bir bitiş noktası oluşturma - Yapılandırma**, bu bilgileri girin veya seçin:

    | Ayar | Değer |
    | ------- | ----- |
    |**Ağ**| |
    | Sanal ağ| *MyVirtualNetwork'ünü*seçin. |
    | Alt ağ |  *mySubnet'i*seçin. |
    |**ÖZEL DNS ENTEGRASYON**||
    |Özel DNS bölgesiyle tümleştirme |**Evet'i**seçin. |
    |Özel DNS Bölgesi |Seçin *(Yeni)privatelink.mariadb.database.azure.com* |
    |||

1. **İncele ve oluştur**’u seçin. Azure'un yapılandırmanızı doğruladığı **Gözden Geçirme + oluşturma** sayfasına götürülürnüz. 
2. **Validation geçirilen** iletiyi gördüğünüzde **Oluştur'u**seçin. 

    ![Özel Bağlantı oluşturuldu](media/concepts-data-access-and-security-private-link/show-mariadb-private-link.png)

    > [!NOTE] 
    > Müşteri DNS ayarındaki FQDN, yapılandırılan özel IP'ye çözüm vermez. [Burada](../dns/dns-operations-recordsets-portal.md)gösterildiği gibi yapılandırılmış FQDN için bir DNS bölgesi kurmanız gerekecektir.

## <a name="connect-to-a-vm-using-remote-desktop-rdp"></a>Uzak Masaüstü (RDP) kullanarak VM'ye bağlanma


**myVm**oluşturduktan sonra, aşağıdaki gibi internetten bağlanın: 

1. Portalın arama çubuğuna *myVm*girin.

1. **Bağlan** düğmesini seçin. **Bağlan** düğmesini seçtikten sonra **sanal makineye bağlan** açılır.

1. **RDP Dosyasını İndir'i**seçin. Azure uzak masaüstü protokolü (*.rdp*) dosyası oluşturur ve bilgisayarınıza indirir.

1. *İndirilen.rdp* dosyasını açın.

    1. İstendiğinde **Bağlan**’ı seçin.

    1. VM oluştururken belirttiğiniz kullanıcı adı ve parolayı girin.

        > [!NOTE]
        > VM'yi oluşturduğunuzda girdiğiniz kimlik bilgilerini belirtmek için**farklı bir hesap kullanın,** **daha fazla seçenek** > seçmeniz gerekebilir.

1. **Tamam'ı**seçin.

1. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Sertifika uyarısı alırsanız **Evet** veya **Devam et'i**seçin.

1. VM masaüstü göründükten sonra, yerel masaüstünüze geri dönmek için en aza indirin.

## <a name="access-the-mariadb-server-privately-from-the-vm"></a>MariaDB sunucusuna VM'den özel olarak erişin

1.  *myVM*Uzak Masaüstünde, PowerShell'i açın.

2. Girin. `nslookup mydemomserver.privatelink.mariadb.database.azure.com` 

    Buna benzer bir ileti alırsınız:
    ```azurepowershell
    Server:  UnKnown
    Address:  168.63.129.16
    Non-authoritative answer:
    Name:    mydemoMariaDBserver.privatelink.mariadb.database.azure.com
    Address:  10.1.3.4
    ```

3. Kullanılabilir istemciyi kullanarak MariaDB sunucusu için özel bağlantı bağlantısını test edin. Aşağıdaki örnekte işlemi yapmak için [MySQL Workbench](https://dev.mysql.com/doc/workbench/en/wb-installing-windows.html) kullandık.


4. **Yeni bağlantıda,** bu bilgileri girin veya seçin:

    | Ayar | Değer |
    | ------- | ----- |
    | Sunucu türü| **MariaDB'yi**seçin.|
    | Sunucu adı| *mydemoserver.privatelink.mariadb.database.azure.com* seçin |
    | Kullanıcı adı | MariaDB username@servername sunucu oluşturma sırasında sağlanan kullanıcı adını girin. |
    |Parola |MariaDB sunucu oluşturma sırasında sağlanan bir parola girin. |
    |SSL|**Gerekli'yi**seçin.|
    ||

5. Test Bağlantısı veya **Tamam'ı**seçin. **Test Connection**

6. (İsteğe bağlı olarak) Sol menüden veritabanlarına göz atın ve MariaDB veritabanından bilgi oluşturun veya sorgula

7. Uzak masaüstü bağlantısını myVm'e kapatın.

## <a name="clean-up-resources"></a>Kaynakları temizleme
Özel bitiş noktası, MariaDB sunucusu ve VM'yi kullandığınızda, kaynak grubunu ve içerdiği tüm kaynakları silin:

1. Portalın üst kısmındaki **Arama** kutusuna *myResourceGroup'u* girin ve arama sonuçlarından *myResourceGroup'u* seçin.
2. **Kaynak grubunu sil**'i seçin.
3. **KAYNAK GRUBU ADINI YAZIN** ve **Sil'i**seçmek için MyResourceGroup'u girin.

## <a name="next-steps"></a>Sonraki adımlar

Bu nasıl yapılsa da, sanal ağda bir VM, MariaDB için bir Azure Veritabanı ve özel erişim için özel bir bitiş noktası oluşturdunuz. İnternetten bir VM'ye bağlandınız ve Private Link'i kullanarak MariaDB sunucusuna güvenli bir şekilde iletişim kurdunuz. Özel uç noktaları hakkında daha fazla bilgi edinmek için [Azure özel bitiş noktası nedir'e](https://docs.microsoft.com/azure/private-link/private-endpoint-overview)bakın.
