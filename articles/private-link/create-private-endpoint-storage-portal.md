---
title: Azure Özel Uç Noktasını kullanıp depolama hesabına özel olarak bağlanma
description: Özel Bitiş Noktası'nı kullanarak Azure'daki bir depolama hesabına özel olarak nasıl bağlanışsüreceğinizi öğrenin.
services: private-link
author: malopMSFT
ms.service: private-link
ms.topic: article
ms.date: 09/16/2019
ms.author: allensu
ms.openlocfilehash: 8c76333d5a2be8a2c589dbe54389b023fef34854
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78252522"
---
# <a name="connect-privately-to-a-storage-account-using-azure-private-endpoint"></a>Azure Özel Uç Noktasını kullanıp depolama hesabına özel olarak bağlanma
Azure Private Endpoint, Azure'daki Özel Bağlantı'nın temel yapı taşıdır. Sanal makineler (VM'ler) gibi Azure kaynaklarının Özel Bağlantı kaynaklarıyla özel olarak iletişim kurmasını sağlar.

Bu Quickstart'ta, Azure portalını kullanarak Özel Bitiş Noktası olan bir depolama hesabı olan bir Azure sanal ağında Nasıl VM oluşturacağınızı öğreneceksiniz. Ardından, Depolama Hesabına VM'den güvenli bir şekilde erişebilirsiniz.

> [!NOTE]
> Özel uç nokta(lar) aynı alt ağda hizmet bitiş noktaları ile birlikte izin verilmez!

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

https://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="create-a-vm"></a>VM oluşturma
Bu bölümde, Özel Bağlantı Kaynağınıza (bu örnekte bir depolama hesabı) erişmek için kullanılan VM'yi barındırmak için sanal ağ ve alt ağ oluşturursunuz.

## <a name="virtual-network-and-parameters"></a>Sanal ağ ve parametreler

Bu bölümde, Özel Bağlantı kaynağınıza erişmek için kullanılan VM'yi barındırmak için sanal ağ ve alt ağ oluşturacaksınız.

Bu bölümde aşağıdaki bilgileri ile adımlarda aşağıdaki parametreleri değiştirmeniz gerekir:

| Parametre                   | Değer                |
|-----------------------------|----------------------|
| **\<kaynak-grup adı>**  | myResourceGroup |
| **\<sanal ağ adı>** | myVirtualNetwork          |
| **\<bölge adı>**          | Orta Batı ABD      |
| **\<IPv4 adres-boşluk>**   | 10.1.0.0\16          |
| **\<subnet-name>**          | mySubnet        |
| **\<alt net-adres aralığı>** | 10.1.0.0\24          |

[!INCLUDE [virtual-networks-create-new](../../includes/virtual-networks-create-new.md)]


### <a name="create-virtual-machine"></a>Sanal makine oluşturma

1. Azure portalında ekranın sol üst tarafında bir **kaynak** > **Oluştur** > **Sanal makine**oluştur'u seçin.

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
    ||

1. **İncele ve oluştur**’u seçin. Azure'un yapılandırmanızı doğruladığı **Gözden Geçirme + oluşturma** sayfasına götürülürnüz.

1. **Validation geçirilen** iletiyi gördüğünüzde **Oluştur'u**seçin.

## <a name="create-your-private-endpoint"></a>Özel Bitiş Noktanızı Oluşturun
Bu bölümde, özel bir Bitiş Noktası kullanarak özel bir depolama hesabı oluşturursunuz. 

1. Azure portalında ekranın sol üst **tarafında, kaynak** > **Depolama Deposu** > **hesabı**oluştur'u seçin.

1. **Depolama hesabı oluştur - Temel bilgiler**, bu bilgileri girin veya seçin:

    | Ayar | Değer |
    | ------- | ----- |
    | **PROJE DETAYLARI** | |
    | Abonelik | Aboneliğinizi seçin. |
    | Kaynak grubu | **MyResourceGroup'u**seçin. Bunu önceki bölümde oluşturdunuz.|
    | **ÖRNEK DETAYLAR** |  |
    | Depolama hesabı adı  | *Depohesabımı*girin. Bu ad alınırsa, benzersiz bir ad oluşturun. |
    | Bölge | **WestCentralUS'u**seçin. |
    | Performans| Varsayılan **Standardı**bırakın. |
    | Hesap türü | Varsayılan **Depolama (genel amaçlı v2)** bırakın. |
    | Çoğaltma | **Oku-erişim coğrafi yedekli depolama (RA-GRS)** seçin. |
    |||
  
3. **Sonraki'ni seçin: Ağ.**
4. **Depolama hesabı oluşturma - Ağ,** bağlantı yöntemi, Özel Bitiş **Noktası'nı**seçin.
5. **Depolama hesabı oluştur - Ağ**oluşturma , Özel Bitiş Noktası **Ekle'yi**seçin. 
6. **Özel Bitiş Noktası Oluştur'da**bu bilgileri girin veya seçin:

    | Ayar | Değer |
    | ------- | ----- |
    | **PROJE DETAYLARI** | |
    | Abonelik | Aboneliğinizi seçin. |
    | Kaynak grubu | **MyResourceGroup'u**seçin. Bunu önceki bölümde oluşturdunuz.|
    |Konum|**WestCentralUS'u**seçin.|
    |Adı|*MyPrivateEndpoint'i*girin.  |
    |Depolama alt kaynağı|Varsayılan **Blob**bırakın. |
    | **Ağ** |  |
    | Sanal ağ  | MyVirtualNetwork'ünü kaynak grubundan *myResourceGroup'u*seçin. *MyVirtualNetwork* |
    | Alt ağ | *mySubnet'i*seçin. |
    | **ÖZEL DNS ENTEGRASYON**|  |
    | Özel DNS bölgesiyle tümleştirme  | Varsayılan **Evet'i**bırakın. |
    | Özel DNS bölgesi  | Varsayılan **(Yeni) privatelink.blob.core.windows.net**bırakın. |
    |||
7. **Tamam'ı**seçin. 
8. **İncele ve oluştur**’u seçin. Azure'un yapılandırmanızı doğruladığı **Gözden Geçirme + oluşturma** sayfasına götürülürnüz. 
9. **Validation geçirilen** iletiyi gördüğünüzde **Oluştur'u**seçin. 
10. Yeni oluşturduğunuz depolama hesabı kaynağına göz atın.
11. Sol daki içerik menüsünden **Erişim Tuşları'nı** seçin.
12. Anahtar1 için bağlantı dizesindeki **Kopyala'yı** seçin.
 
## <a name="connect-to-a-vm-from-the-internet"></a>İnternet'ten bir sanal makineye bağlanma

Aşağıdaki gibi internetten VM *myVm* bağlanın:

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

## <a name="access-storage-account-privately-from-the-vm"></a>Depolama hesabına VM'den özel olarak erişin

Bu bölümde, Özel Bitiş Noktası'nı kullanarak depolama hesabına özel olarak bağlanabilirsiniz.

1. *myVM*Uzak Masaüstünde, PowerShell'i açın.
2. Enter `nslookup mystorageaccount.blob.core.windows.net` Buna benzer bir ileti alacaksınız:
    ```azurepowershell
    Server:  UnKnown
    Address:  168.63.129.16
    Non-authoritative answer:
    Name:    mystorageaccount123123.privatelink.blob.core.windows.net
    Address:  10.0.0.5
    Aliases:  mystorageaccount.blob.core.windows.net
    ```
3. [Microsoft Azure Depolama Gezgini](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer?toc=%2Fazure%2Fstorage%2Fblobs%2Ftoc.json&tabs=windows)'ni yükleyin.
4. Sağ tıklatarak **Depolama hesaplarını** seçin.
5. **Azure depolama alanına Bağlan'ı**seçin.
6. **Bağlantı dizesi kullan'ı**seçin.
7. **Sonraki'ni**seçin.
8. Daha önce kopyalanan bilgileri yapıştırarak bağlantı dizesini girin.
9. **Sonraki'ni**seçin.
10. **Bağlan**’ı seçin.
11. Depohesabımdaki Blob kapları'na göz atın 
12. (İsteğe bağlı olarak) Klasörler oluşturun ve/veya *mystorageaccount'a*dosya yükleyin. 
13. Uzak masaüstü bağlantısını *myVM'e*kapatın. 

Depolama hesabına erişmek için ek seçenekler:
- Microsoft Azure Depolama Gezgini, Windows, macOS ve Linux'taki Azure depolama verileriyle görsel olarak çalışmanızı sağlayan Microsoft'un bağımsız ücretsiz bir uygulamasıdır. Depolama hesabı içeriğine özel olarak göz atmak için uygulamayı yükleyebilirsiniz. 
 
- AzCopy yardımcı programı, Azure depolama için yüksek performanslı komut dosyası veri aktarımı için başka bir seçenektir. Blob, Dosya ve Tablo depolamaları arasında veri aktarmak için AzCopy kullanın. 


## <a name="clean-up-resources"></a>Kaynakları temizleme 
Özel Bitiş Noktası, depolama hesabı ve VM'yi kullandığınızda, kaynak grubunu ve içerdiği tüm kaynakları silin: 
1. Portalın üst kısmındaki **Arama** kutusuna *myResourceGroup'u* girin ve arama sonuçlarından *myResourceGroup'u* seçin. 
2. **Kaynak grubunu sil**'i seçin. 
3. **KAYNAK GRUBU ADINI YAZIN** ve **Sil'i**seçmek için *MyResourceGroup'u* girin. 

## <a name="next-steps"></a>Sonraki adımlar
Bu Quickstart'ta, sanal ağ ve depolama hesabında bir VM ve özel bitiş noktası oluşturdunuz. İnternetten bir VM'ye bağlandınız ve Private Link'i kullanarak depolama hesabına güvenli bir şekilde iletişim kurdunuz. Private Endpoint hakkında daha fazla bilgi edinmek için Azure [Özel Bitiş Noktası nedir?](private-endpoint-overview.md)
