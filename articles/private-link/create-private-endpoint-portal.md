---
title: Quickstart - Azure'da Özel Uç Noktaları Yönetme
description: Bu Quickstart'ta Azure portalını kullanarak Özel Bitiş Noktası oluşturmayı öğrenin
services: private-link
author: malopMSFT
ms.service: private-link
ms.topic: quickstart
ms.date: 09/16/2019
ms.author: allensu
ms.openlocfilehash: 1bdc089bb89a732e329bf7d3ffd3d5b5c09ba408
ms.sourcegitcommit: 0450ed87a7e01bbe38b3a3aea2a21881f34f34dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80637253"
---
# <a name="quickstart-create-a-private-endpoint-using-azure-portal"></a>Hızlı başlangıç: Azure portalLarını kullanarak Özel Bitiş Noktası Oluşturma

Özel Bitiş Noktası, Azure'daki özel bağlantının temel yapı taşıdır. Sanal Makineler (VM' ler) gibi Azure kaynaklarının özel bağlantı kaynaklarıyla özel olarak iletişim kurmasını sağlar. Bu Quickstart'ta, Azure portalını kullanarak Azure özel bitiş noktasına sahip bir SQL Veritabanı Sunucusu olan Azure Sanal Ağı'nda VM oluşturmayı öğreneceksiniz. Ardından, VM'den SQL Database Server'a güvenli bir şekilde erişebilirsiniz.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.


## <a name="sign-in-to-azure"></a>Azure'da oturum açma

https://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="create-a-vm"></a>VM oluşturma
Bu bölümde, Özel Bağlantı kaynağınıza erişmek için kullanılan VM'yi (bu örnekte Azure'daki bir SQL sunucusu) barındırmak için sanal ağ ve alt ağ oluşturursunuz.

## <a name="virtual-network-and-parameters"></a>Sanal ağ ve parametreler

Bu bölümde, Özel Bağlantı kaynağınıza erişmek için kullanılan VM'yi barındıracak bir Sanal Ağ ve alt ağ oluşturursunuz.

Bu bölümde aşağıdaki bilgileri ile adımlarda aşağıdaki parametreleri değiştirmeniz gerekir:

| Parametre                   | Değer                |
|-----------------------------|----------------------|
| **\<kaynak-grup adı>**  | myResourceGroup |
| **\<sanal ağ adı>** | myVirtualNetwork          |
| **\<bölge adı>**          | Orta Batı ABD    |
| **\<IPv4 adres-boşluk>**   | 10.1.0.0/16          |
| **\<subnet-name>**          | mySubnet        |
| **\<alt net-adres aralığı>** | 10.1.0.0/24          |

[!INCLUDE [virtual-networks-create-new](../../includes/virtual-networks-create-new.md)]

### <a name="create-virtual-machine"></a>Sanal Makine Oluştur

1. Azure portalında ekranın sol üst tarafında bir **kaynak** > **Oluştur** > **Sanal Makine**oluştur'u seçin.

1. Sanal makine Oluştur 'da **- Temel bilgiler,** bu bilgileri girin veya seçin:

    | Ayar | Değer |
    | ------- | ----- |
    | **PROJE DETAYLARI** | |
    | Abonelik | Aboneliğinizi seçin. |
    | Kaynak grubu | **MyResourceGroup'u**seçin. Bunu önceki bölümde oluşturdunuz.  |
    | **ÖRNEK DETAYLAR** |  |
    | Sanal makine adı | *myVm*girin. |
    | Bölge | **WestCentralUS'u**seçin. |
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

## <a name="create-a-sql-database-server"></a>SQL veritabanı sunucusu oluşturma
Bu bölümde, Azure'da bir SQL veritabanı sunucusu oluşturursunuz. 

1. Azure portalında ekranın sol üst tarafında > kaynak**Veritabanları** > **SQL veritabanı** **oluştur'u**seçin.

1. **SQL veritabanı oluştur - Temel bilgiler**, bu bilgileri girin veya seçin:

    | Ayar | Değer |
    | ------- | ----- |
    | **Veritabanı ayrıntıları** | |
    | Abonelik | Aboneliğinizi seçin. |
    | Kaynak grubu | **MyResourceGroup'u**seçin. Bunu önceki bölümde oluşturdunuz.|
    | **ÖRNEK DETAYLAR** |  |
    | Veritabanı adı  | *Veritabanımı*girin. Bu ad alınırsa, benzersiz bir ad oluşturun. |
    |||
5. **Sunucu'da**, **yeni oluştur'u**seçin. 
6. **Yeni sunucuda,** bu bilgileri girin veya seçin:

    | Ayar | Değer |
    | ------- | ----- |
    |Sunucu adı  | *Sunucumu*girin. Bu ad alınırsa, benzersiz bir ad oluşturun.|
    | Sunucu yöneticisi oturum açma| Seçtiğiniz bir yönetici adını girin. |
    | Parola | Seçtiğiniz bir parolayı girin. Parola en az 8 karakter uzunluğunda olmalı ve tanımlanan gereksinimleri karşılamalıdır. |
    | Konum | SQL Server'ınızın ikamet etmesini istediğiniz bir Azure bölgesi seçin. |
    
7. **Tamam'ı**seçin. 
8. **İncele ve oluştur**’u seçin. Azure'un yapılandırmanızı doğruladığı **Gözden Geçirme + oluşturma** sayfasına götürülürnüz. 
9. Validation geçirilen iletiyi gördüğünüzde **Oluştur'u**seçin. 
10. Doğrulama geçti iletisini gördüğünüzde Oluştur'u seçin. 

## <a name="create-a-private-endpoint"></a>Özel uç nokta oluşturma

Bu bölümde, bir SQL sunucusu oluşturacak ve özel bir bitiş noktası ekleyeceğiz. 

1. Azure portalında ekranın sol üst tarafında, **Kaynak** > **Oluştur** > **Özel Bağlantı Merkezi (Önizleme)** seçeneğini belirleyin.
2. **Özel Bağlantı Merkezi**'nde - Genel Bakış , bir **hizmete özel bağlantı kurma**seçeneğine ilişkin Olarak **Başlat'ı**seçin.
1. **Özel bir bitiş noktası oluştur 'da (Önizleme) - Temel bilgiler,** bu bilgileri girin veya seçin:

    | Ayar | Değer |
    | ------- | ----- |
    | **Proje ayrıntıları** | |
    | Abonelik | Aboneliğinizi seçin. |
    | Kaynak grubu | **MyResourceGroup'u**seçin. Bunu önceki bölümde oluşturdunuz.|
    | **ÖRNEK DETAYLAR** |  |
    | Adı | *MyPrivateEndpoint'i*girin. Bu ad alınırsa, benzersiz bir ad oluşturun. |
    |Bölge|**WestCentralUS'u**seçin.|
    |||
5. **Sonraki seçin: Kaynak**.
6. **Özel bir bitiş noktası oluştur - Kaynak**, bu bilgileri girin veya seçin:

    | Ayar | Değer |
    | ------- | ----- |
    |Bağlantı yöntemi  | Dizinimdeki bir Azure kaynağına bağlan'ı seçin.|
    | Abonelik| Aboneliğinizi seçin. |
    | Kaynak türü | **Microsoft.Sql/servers'ı**seçin. |
    | Kaynak |*myServer'ı* seçin|
    |Hedef alt kaynak |*sqlServer'ı* seçin|
    |||
7. **Sonraki'ni seçin: Yapılandırma.**
8. **Özel bir bitiş noktası oluştur (Önizleme) - Yapılandırmada,** bu bilgileri girin veya seçin:

    | Ayar | Değer |
    | ------- | ----- |
    |**Ağ**| |
    | Sanal ağ| *MyVirtualNetwork'ünü*seçin. |
    | Alt ağ | *mySubnet'i*seçin. |
    |**ÖZEL DNS ENTEGRASYON**||
    |Özel DNS bölgesiyle tümleştirme |**Evet'i**seçin. |
    |Özel DNS Bölgesi |Seçin *(Yeni)privatelink.database.windows.net* |
    |||

1. **İncele ve oluştur**’u seçin. Azure'un yapılandırmanızı doğruladığı **Gözden Geçirme + oluşturma** sayfasına götürülürnüz. 
2. **Validation geçirilen** iletiyi gördüğünüzde **Oluştur'u**seçin. 
 
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

## <a name="access-the-sql-database-server-privately-from-the-vm"></a>SQL veritabanı sunucusuna VM'den özel olarak erişin

1. *myVM*Uzak Masaüstünde, PowerShell'i açın.

2. `nslookup myserver.database.windows.net` yazın. 

    Buna benzer bir ileti alırsınız:
    ```azurepowershell
    Server:  UnKnown
    Address:  168.63.129.16
    Non-authoritative answer:
    Name:    myserver.privatelink.database.windows.net
    Address:  10.0.0.5
    Aliases:   myserver.database.windows.net
    ```
3. [SQL Server Management Studio'ya](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-2017)yükleyin.

4. **Sunucuya Bağlan'da**bu bilgileri girin veya seçin:

    | Ayar | Değer |
    | ------- | ----- |
    | Sunucu türü| **Veritabanı Altyapısı**’nı seçin.|
    | Sunucu adı| *myserver.database.windows.net* seçin |
    | Kullanıcı adı | SQL sunucu username@servername oluşturma sırasında sağlanan kullanıcı adını girin. |
    |Parola |SQL sunucusu oluşturma sırasında sağlanan bir parola girin. |
    |Parolayı hatırla|**Evet'i**seçin.|
    |||
1. **Bağlan**’ı seçin.
2. Sol menüden veritabanlarına göz atın.
3. (İsteğe bağlı olarak) Veritabanımdan bilgi oluşturun veya sorgula.
4. Uzak masaüstü bağlantısını *myVm'e*kapatın. 

## <a name="clean-up-resources"></a>Kaynakları temizleme 
Özel bitiş noktası, SQL sunucusu ve VM'yi kullandığınızda, kaynak grubunu ve içerdiği tüm kaynakları silin: 
1. Portalın üst kısmındaki **Arama** kutusuna *myResourceGroup'u* girin ve arama sonuçlarından *myResourceGroup'u* seçin. 
2. **Kaynak grubunu sil**'i seçin. 
3. **KAYNAK GRUBU ADINI YAZIN** ve **Sil'i**seçmek için MyResourceGroup'u girin.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlatmada, sanal ağda bir VM, bir SQL veritabanı sunucusu ve özel erişim için özel bir bitiş noktası oluşturdunuz. İnternetten bir VM'ye bağlandınız ve Private Link'i kullanarak SQL veritabanı sunucusuna güvenli bir şekilde iletişim kurdunuz. Özel bitiş noktaları hakkında daha fazla bilgi edinmek için Azure [özel bitiş noktası nedir?](private-endpoint-overview.md)
