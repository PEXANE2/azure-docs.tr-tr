---
title: Azure özel bağlantısı ile bir Azure Cosmos hesabına bağlanma
description: Özel bir uç nokta oluşturarak bir VM 'den Azure Cosmos hesabına güvenli bir şekilde erişme hakkında bilgi edinin.
author: malopMSFT
ms.service: cosmos-db
ms.topic: how-to
ms.date: 11/04/2019
ms.author: allensu
ms.openlocfilehash: d074128376d481902a203de3d32ef89aa72d7b3a
ms.sourcegitcommit: c4ad4ba9c9aaed81dfab9ca2cc744930abd91298
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/12/2020
ms.locfileid: "84737725"
---
# <a name="connect-privately-to-an-azure-cosmos-account-using-azure-private-link"></a>Azure Özel Bağlantı’yı kullanıp Azure Cosmos hesabına özel olarak bağlanma

Azure özel uç noktası, Azure 'da özel bağlantı için temel yapı taşdır. Sanal makineler (VM) gibi Azure kaynaklarının özel bağlantı kaynaklarıyla özel olarak iletişim kurmasına olanak sağlar.

Bu makalede Azure portalını kullanarak Özel Uç Nokta ile Azure sanal ağında bir sanal makine ve bir Azure Cosmos hesabı oluşturmayı öğreneceksiniz. Ardından sanal makineden Azure Cosmos hesabına güvenle erişebilirsiniz.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Azure portal oturum açın [.](https://portal.azure.com)

## <a name="create-a-vm"></a>VM oluşturma

## <a name="virtual-network-and-parameters"></a>Sanal ağ ve parametreler

Bu bölümde, özel bağlantı kaynağına erişmek için kullanılan VM 'yi barındırmak için bir sanal ağ ve alt ağ oluşturacaksınız (Bu örnekteki bir Azure Cosmos hesabı).

Bu bölümde, adımlarda aşağıdaki parametreleri aşağıdaki bilgilerle değiştirmeniz gerekir:

| Parametre                   | Değer                |
|-----------------------------|----------------------|
| **\<resource-group-name>**  | myResourceGroup|
| **\<virtual-network-name>** | myVirtualNetwork         |
| **\<region-name>**          | Orta Batı ABD     |
| **\<IPv4-address-space>**   | 10.1.0.0 \ 16          |
| **\<subnet-name>**          | mySubnet        |
| **\<subnet-address-range>** | 10.1.0.0 \ 24          |

[!INCLUDE [virtual-networks-create-new](../../includes/virtual-networks-create-new.md)]

### <a name="create-the-virtual-machine"></a>Sanal makineyi oluşturma

1. Azure Portal ekranın sol üst tarafında **kaynak oluştur**  >  **işlem**  >  **sanal makinesi**' ni seçin.

1. **Sanal makine oluşturma-temel bilgiler**bölümünde, bu bilgileri girin veya seçin:

    | Ayar | Değer |
    | ------- | ----- |
    | **PROJE AYRıNTıLARı** | |
    | Abonelik | Aboneliğinizi seçin. |
    | Kaynak grubu | **Myresourcegroup**öğesini seçin. Bu, önceki bölümde oluşturdunuz.  |
    | **ÖRNEK AYRıNTıLARı** |  |
    | Sanal makine adı | *Myvm*' i girin. |
    | Bölge | **WestCentralUS**öğesini seçin. |
    | Kullanılabilirlik seçenekleri | Varsayılan **altyapı yedekliliği gerekli değildir**. |
    | Görüntü | **Windows Server 2019 Datacenter**öğesini seçin. |
    | Boyut | Varsayılan **Standart DS1 v2**' i bırakın. |
    | **YÖNETICI HESABı** |  |
    | Kullanıcı adı | Tercih ettiğiniz bir Kullanıcı adı girin. |
    | Parola | Tercih ettiğiniz parolayı girin. Parola en az 12 karakter uzunluğunda olmalı ve [tanımlanmış karmaşıklık gereksinimlerini](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm) karşılamalıdır.|
    | Parolayı Onayla | Parolayı yeniden girin. |
    | **GELEN BAĞLANTı NOKTASı KURALLARı** |  |
    | Ortak gelen bağlantı noktaları | Varsayılanı **yok**olarak bırakın. |
    | **TASARRUF EDIN** |  |
    | Zaten bir Windows lisansınız var mı? | Varsayılan **Hayır**olarak bırakın. |
    |||

1. **İleri ' yi seçin: diskler**.

1. **Sanal makine oluşturma-diskler**' de, varsayılan değerleri bırakın ve **İleri ' yi seçin: ağ**.

1. **Sanal makine oluşturma-ağ oluşturma**bölümünde şu bilgileri seçin:

    | Ayar | Değer |
    | ------- | ----- |
    | Sanal ağ | Varsayılan **MyVirtualNetwork**bırakın.  |
    | Adres alanı | Varsayılan **10.1.0.0/24**' i bırakın.|
    | Alt ağ | Varsayılan **Mysubnet (10.1.0.0/24)** olarak bırakın.|
    | Genel IP | Varsayılan **(yeni) myVm-ip**' i bırakın. |
    | Ortak gelen bağlantı noktaları | **Seçili bağlantı noktalarına Izin ver**' i seçin. |
    | Gelen bağlantı noktalarını seçin | **Http** ve **RDP**' yi seçin.|
    ||

1. **İncele ve oluştur**’u seçin. Azure 'un yapılandırmanızı doğruladığı, **gözden geçir + oluştur** sayfasına götürülürsünüz.

1. **Doğrulama başarılı** Iletisini gördüğünüzde **Oluştur**' u seçin.

## <a name="create-an-azure-cosmos-account"></a>Azure Cosmos hesabı oluşturma

[Azure Cosmos SQL API hesabı](../cosmos-db/create-cosmosdb-resources-portal.md#create-an-azure-cosmos-db-account)oluşturun. Kolaylık olması için, Azure Cosmos hesabını diğer kaynaklarla aynı bölgede ("WestCentralUS") oluşturabilirsiniz.

## <a name="create-a-private-endpoint-for-your-azure-cosmos-account"></a>Azure Cosmos hesabınız için özel bir uç nokta oluşturma

Bağlantılı makalenin [Azure Portal bölümünü kullanarak özel bağlantı oluşturma](../cosmos-db/how-to-configure-private-endpoints.md#create-a-private-endpoint-by-using-the-azure-portal) bölümünde açıklandığı gibi Azure Cosmos hesabınız için özel bir bağlantı oluşturun.

## <a name="connect-to-a-vm-from-the-internet"></a>İnternet'ten bir sanal makineye bağlanma

Aşağıdaki gibi, internet *'ten gelen VM VM* 'sine bağlanın:

1. Portalın arama çubuğunda *Myvm*' i girin.

1. **Bağlan** düğmesini seçin. **Bağlan** düğmesini seçtikten sonra **sanal makineye bağlan** açılır.

1. **RDP dosyasını indir**' i seçin. Azure bir Uzak Masaüstü Protokolü (*. rdp*) dosyası oluşturur ve bilgisayarınıza indirir.

1. İndirilen *. rdp* dosyasını açın.

    1. İstendiğinde **Bağlan**’ı seçin.

    1. VM oluştururken belirttiğiniz kullanıcı adını ve parolayı girin.

        > [!NOTE]
        > **More choices**  >  VM oluştururken girdiğiniz kimlik bilgilerini belirtmek için**farklı bir hesap kullan**' ı seçmeniz gerekebilir.

1. **Tamam**’ı seçin.

1. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Bir sertifika uyarısı alırsanız **Evet** ' i veya **devam et**' i seçin.

1. VM masaüstü seçildikten sonra, bunu yerel masaüstünüze geri dönmek için simge durumuna küçültün.  

## <a name="access-the-azure-cosmos-account-privately-from-the-vm"></a>Azure Cosmos hesabına VM 'den özel olarak erişin

Bu bölümde, Özel uç nokta kullanarak Azure Cosmos hesabına özel olarak bağlanacaksınız. 

1. IP adresi ve DNS eşlemesini dahil etmek için, sanal makinenizde *Myvm*'de oturum açın, dosyayı açın `c:\Windows\System32\Drivers\etc\hosts` ve ÖNCEKI adımdan gelen DNS bilgilerini aşağıdaki biçimde ekleyin:

   [Özel IP adresi] [Hesap uç noktası]. Documents. Azure. com

   **Örneğinde**

   10.1.255.13 mycosmosaccount.documents.azure.com

   10.1.255.14 mycosmosaccount-eastus.documents.azure.com


1. *Myvm*uzak masaüstünde [Microsoft Azure Depolama Gezgini](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer?toc=%2Fazure%2Fstorage%2Fblobs%2Ftoc.json&tabs=windows)' yi yükleyip.

1. Sağ tıklama ile **Cosmos DB hesapları (Önizleme)** öğesini seçin.

1. **Cosmos DB Bağlan**' ı seçin.

1. **API**’yi seçin.

1. Daha önce kopyalanmış bilgileri yapıştırarak bağlantı dizesini girin.

1. **İleri**’yi seçin.

1. **Bağlan**'ı seçin.

1. *Mycosmosaccount*Içindeki Azure Cosmos veritabanlarına ve kapsayıcılarına gözatamazsınız.

1. (İsteğe bağlı olarak) *mycosmosaccount*öğesine yeni öğeler ekleyin.

1. *Myvm*ile uzak masaüstü bağlantısını kapatın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Özel uç nokta, Azure Cosmos hesabı ve VM 'yi kullanarak işiniz bittiğinde, kaynak grubunu ve içerdiği tüm kaynakları silin: 

1. Portalın üst kısmındaki **arama** kutusuna *myresourcegroup* yazın ve arama sonuçlarından *myresourcegroup* öğesini seçin.

1. **Kaynak grubunu sil**'i seçin.

1. **Kaynak grubu adını yazın** ve **Sil**' i seçmek için *myresourcegroup* girin.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede bir sanal ağ, bir Azure Cosmos hesabı ve özel uç nokta üzerinde bir VM oluşturdunuz. VM 'ye internet 'ten bağlı ve özel bağlantı kullanarak Azure Cosmos hesabına güvenli bir şekilde Iletilecaksınız.

* Özel uç nokta hakkında daha fazla bilgi edinmek için bkz. [Azure özel uç noktası nedir?](private-endpoint-overview.md).

* Azure Cosmos DB ile kullanırken özel uç nokta sınırlaması hakkında daha fazla bilgi için, bkz. [Azure özel bağlantı Azure Cosmos DB](../cosmos-db/how-to-configure-private-endpoints.md) makalesi.
