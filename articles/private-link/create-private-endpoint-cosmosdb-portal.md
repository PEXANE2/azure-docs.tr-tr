---
title: Azure Özel Bağlantısı ile Azure Cosmos hesabına bağlanma
description: Özel Bitiş Noktası oluşturarak Bir VM'den Azure Cosmos hesabına nasıl güvenli bir şekilde erişin gerektiğini öğrenin.
author: malopMSFT
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: allensu
ms.openlocfilehash: b7a50a2dabc9503ca5dbdd3388e29cfc69963885
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78252595"
---
# <a name="connect-privately-to-an-azure-cosmos-account-using-azure-private-link"></a>Azure Özel Bağlantısını kullanarak azure cosmos hesabına özel bağlanma

Azure Private Endpoint, Azure'daki Özel Bağlantı'nın temel yapı taşıdır. Sanal makineler (VM'ler) gibi Azure kaynaklarının Özel Bağlantı kaynaklarıyla özel olarak iletişim kurmasını sağlar.

Bu makalede, Azure portalını kullanarak Bir Özel Bitiş Noktası ile Bir Azure sanal ağında ve Azure Cosmos hesabında VM nasıl oluşturulacağı öğrenilir. Ardından, Azure Cosmos hesabına VM'den güvenli bir şekilde erişebilirsiniz.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[Azure portalında](https://portal.azure.com) oturum açın.

## <a name="create-a-vm"></a>VM oluşturma

## <a name="virtual-network-and-parameters"></a>Sanal ağ ve parametreler

Bu bölümde, Özel Bağlantı kaynağınıza (bu örnekte bir Azure Cosmos hesabı) erişmek için kullanılan VM'yi barındıracak bir sanal ağ ve alt ağ oluşturursunuz.

Bu bölümde aşağıdaki bilgileri ile adımlarda aşağıdaki parametreleri değiştirmeniz gerekir:

| Parametre                   | Değer                |
|-----------------------------|----------------------|
| **\<kaynak-grup adı>**  | myResourceGroup|
| **\<sanal ağ adı>** | myVirtualNetwork         |
| **\<bölge adı>**          | Orta Batı ABD     |
| **\<IPv4 adres-boşluk>**   | 10.1.0.0\16          |
| **\<subnet-name>**          | mySubnet        |
| **\<alt net-adres aralığı>** | 10.1.0.0\24          |

[!INCLUDE [virtual-networks-create-new](../../includes/virtual-networks-create-new.md)]

### <a name="create-the-virtual-machine"></a>Sanal makineyi oluşturma

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
    | Parola | Seçtiğiniz bir parola girin. Parola en az 12 karakter uzunluğunda olmalı ve [tanımlanmış karmaşıklık gereksinimlerini](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm) karşılamalıdır.|
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

## <a name="create-an-azure-cosmos-account"></a>Azure Cosmos hesabı oluşturma

Bir [Azure Cosmos SQL API hesabı](../cosmos-db/create-cosmosdb-resources-portal.md#create-an-azure-cosmos-db-account)oluşturun. Basitlik için Azure Cosmos hesabını diğer kaynaklarla aynı bölgede oluşturabilirsiniz (yani "WestCentralUS").

## <a name="create-a-private-endpoint-for-your-azure-cosmos-account"></a>Azure Cosmos hesabınız için Özel Bitiş Noktası Oluşturma

Bağlantılı [makalenin Azure portalı](../cosmos-db/how-to-configure-private-endpoints.md#create-a-private-endpoint-by-using-the-azure-portal) bölümünü kullanarak Özel Bağlantı Oluştur'da açıklandığı gibi Azure Cosmos hesabınız için özel bir bağlantı oluşturun.

## <a name="connect-to-a-vm-from-the-internet"></a>İnternet'ten bir sanal makineye bağlanma

Aşağıdaki gibi internetten VM *myVm* bağlanın:

1. Portalın arama çubuğuna *myVm*girin.

1. **Bağlan** düğmesini seçin. **Bağlan** düğmesini seçtikten sonra **sanal makineye bağlan** açılır.

1. **RDP Dosyasını İndir'i**seçin. Azure uzak masaüstü protokolü (*.rdp*) dosyası oluşturur ve bilgisayarınıza indirir.

1. İndirilen *.rdp* dosyasını açın.

    1. İstendiğinde **Bağlan**’ı seçin.

    1. VM oluştururken belirttiğiniz kullanıcı adı ve parolayı girin.

        > [!NOTE]
        > VM'yi oluşturduğunuzda girdiğiniz kimlik bilgilerini belirtmek için**farklı bir hesap kullanın,** **daha fazla seçenek** > seçmeniz gerekebilir.

1. **Tamam'ı**seçin.

1. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Sertifika uyarısı alırsanız **Evet** veya **Devam et'i**seçin.

1. VM masaüstü göründükten sonra, yerel masaüstünüze geri dönmek için en aza indirin.  

## <a name="access-the-azure-cosmos-account-privately-from-the-vm"></a>Azure Cosmos hesabına VM'den özel olarak erişin

Bu bölümde, Özel Bitiş Noktası'nı kullanarak Azure Cosmos hesabına özel olarak bağlanabilirsiniz. 

1. IP adresi ve DNS eşlecilik eklemek için, Sanal `c:\Windows\System32\Drivers\etc\hosts` makine *myVM*oturum, dosyayı açın ve aşağıdaki biçimde önceki adımdan DNS bilgileri içerir:

   [Özel IP Adresi] [Hesap bitiş noktası].documents.azure.com

   **Örnek:**

   10.1.255.13 mycosmosaccount.documents.azure.com

   10.1.255.14 mycosmosaccount-eastus.documents.azure.com


1. *myVM'in*Uzak Masaüstü'nde [Microsoft Azure Depolama Gezgini'ni](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer?toc=%2Fazure%2Fstorage%2Fblobs%2Ftoc.json&tabs=windows)yükleyin.

1. Sağ tıklatarak **Cosmos DB Hesapları'nı (Önizleme)** seçin.

1. **Cosmos DB'ye Bağlan'ı**seçin.

1. **API**’yi seçin.

1. Daha önce kopyalanan bilgileri yapıştırarak bağlantı dizesini girin.

1. **Sonraki'ni**seçin.

1. **Bağlan**’ı seçin.

1. *Mycosmosaccount*adresindeki Azure Cosmos veritabanlarına ve kapsayıcılarına göz atın.

1. (İsteğe bağlı olarak) *mikozmosaccount'a*yeni öğeler ekleyin.

1. Uzak masaüstü bağlantısını *myVM'e*kapatın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Özel Bitiş Noktası, Azure Cosmos hesabı ve VM'yi kullandığınızda, kaynak grubunu ve içerdiği tüm kaynakları silin: 

1. Portalın üst kısmındaki **Arama** kutusuna *myResourceGroup'u* girin ve arama sonuçlarından *myResourceGroup'u* seçin.

1. **Kaynak grubunu sil**'i seçin.

1. **KAYNAK GRUBU ADINI YAZIN** ve **Sil'i**seçmek için *MyResourceGroup'u* girin.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, sanal ağda bir VM, bir Azure Cosmos hesabı ve özel bitiş noktası oluşturdunuz. VM'ye internetten bağlandınız ve Özel Bağlantı'yı kullanarak Azure Cosmos hesabına güvenli bir şekilde iletişim kurdunuz.

* Private Endpoint hakkında daha fazla bilgi edinmek için Azure [Özel Bitiş Noktası nedir?](private-endpoint-overview.md)

* Azure Cosmos DB ile kullanırken Özel Bitiş Noktası sınırlaması hakkında daha fazla bilgi edinmek için [Azure Cosmos DB makalesini](../cosmos-db/how-to-configure-private-endpoints.md) içeren Azure Özel Bağlantı bölümüne bakın.
