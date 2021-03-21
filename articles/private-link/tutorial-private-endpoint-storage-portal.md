---
title: 'Öğretici: Azure özel uç noktası kullanarak bir depolama hesabına bağlanma'
titleSuffix: Azure Private Link
description: Bir depolama hesabına özel olarak bağlanmak için Azure özel uç noktasını kullanarak Bu öğreticiye başlayın.
author: asudbring
ms.author: allensu
ms.service: private-link
ms.topic: tutorial
ms.date: 9/25/2020
ms.openlocfilehash: eefef0eb61df9aa0597206b537f3d58e8b8b3213
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "96484830"
---
# <a name="tutorial-connect-to-a-storage-account-using-an-azure-private-endpoint"></a>Öğretici: Azure özel uç noktası kullanarak bir depolama hesabına bağlanma

Azure özel uç noktası, Azure 'da özel bağlantı için temel yapı taşdır. Sanal makineler (VM) gibi Azure kaynaklarının özel olarak özel bağlantı kaynaklarıyla iletişim kurmasını sağlar.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Bir sanal ağ ve savunma ana bilgisayarı oluşturun.
> * Sanal makine oluşturur.
> * Özel bir uç nokta ile depolama hesabı oluşturun.
> * Depolama hesabı özel uç noktasına yönelik test bağlantısı.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

* Bir Azure aboneliği

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[Azure portalında](https://portal.azure.com) oturum açın.

## <a name="create-a-virtual-network-and-bastion-host"></a>Sanal ağ ve savunma Konağı oluşturma

Bu bölümde, bir sanal ağ, alt ağ ve savunma ana bilgisayarı oluşturacaksınız. 

Savunma Konağı, Özel uç noktasını test etmek üzere sanal makineye güvenli bir şekilde bağlanmak için kullanılacaktır.

1. Ekranın sol üst kısmındaki **Kaynak oluştur > Ağ > Sanal ağ** seçeneğini belirleyin veya arama kutusuna **Sanal ağ** yazarak arama yapın.

2. **Sanal ağ oluştur**' da, **temel** bilgiler sekmesinde bu bilgileri girin veya seçin:

    | **Ayar**          | **Değer**                                                           |
    |------------------|-----------------------------------------------------------------|
    | **Proje ayrıntıları**  |                                                                 |
    | Abonelik     | Azure aboneliğinizi seçin                                  |
    | Kaynak Grubu   | **Myresourcegroup** seçin |
    | **Örnek ayrıntıları** |                                                                 |
    | Name             | **Myvnet** girin                                    |
    | Region           | **Doğu ABD** seçin |

3. **IP adresleri** sekmesini seçin veya sayfanın altındaki **Sonraki: IP adresleri** düğmesini seçin.

4. **IP adresleri** sekmesinde şu bilgileri girin:

    | Ayar            | Değer                      |
    |--------------------|----------------------------|
    | IPv4 adres alanı | **10.1.0.0/16** girin |

5. **Alt ağ adı** altında, **varsayılan** sözcük ' ı seçin.

6. **Alt ağı Düzenle**' de şu bilgileri girin:

    | Ayar            | Değer                      |
    |--------------------|----------------------------|
    | Alt ağ adı | **Mysubnet** girin |
    | Alt ağ adres aralığı | **10.1.0.0/24** girin |

7. **Kaydet**’i seçin.

8. **Güvenlik** sekmesini seçin.

9. **Bastionhost** altında **Etkinleştir**' i seçin. Bu bilgileri girin:

    | Ayar            | Değer                      |
    |--------------------|----------------------------|
    | Savunma adı | **Mybastionhost** girin |
    | AzureBastionSubnet adres alanı | **10.1.1.0/24** girin |
    | Genel IP Adresi | **Yeni oluştur**’u seçin. </br> **Ad** Için **Mybastionıp** girin. </br> **Tamam**’ı seçin. |


8. **Gözden geçir + oluştur** sekmesini seçin ya da **gözden geçir + oluştur** düğmesini seçin.

9. **Oluştur**’u seçin.

## <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

Bu bölümde, Özel uç noktayı test etmek için kullanılacak bir sanal makine oluşturacaksınız.


1. Portalın sol üst tarafında, **kaynak oluştur**  >  **işlem**  >  **sanal makinesi** ' ni seçin veya arama kutusunda **sanal makine** ara ' yı seçin.
   
2. **Sanal makine oluştur**' da **temel bilgiler** sekmesinde değerleri yazın veya seçin:

    | Ayar | Değer                                          |
    |-----------------------|----------------------------------|
    | **Proje ayrıntıları** |  |
    | Abonelik | Azure aboneliğinizi seçin |
    | Kaynak Grubu | **Myresourcegroup** seçin |
    | **Örnek ayrıntıları** |  |
    | Sanal makine adı | **Myvm 'yi** girin |
    | Region | **Doğu ABD** seçin |
    | Kullanılabilirlik seçenekleri | **Altyapı yedekliliği gerekli değil** ' i seçin |
    | Görüntü | **Windows Server 2019 Datacenter-Gen1** seçin |
    | Azure Spot örneği | **Hayır** seçin |
    | Boyut | VM boyutunu seçin veya varsayılan ayarı yapın |
    | **Yönetici hesabı** |  |
    | Kullanıcı adı | Kullanıcı adı girin |
    | Parola | Parola girin |
    | Parolayı onayla | Parolayı yeniden girin |

3. **Ağ** sekmesini seçin veya **Sonraki: diskler**' i ve sonra **İleri: ağ**' ı seçin.
  
4. Ağ sekmesinde, şunu seçin veya girin:

    | Ayar | Değer |
    |-|-|
    | **Ağ arabirimi** |  |
    | Sanal ağ | **myVNet** |
    | Alt ağ | **mySubnet** |
    | Genel IP | **Hiçbiri** seçeneğini belirtin. |
    | NIC ağ güvenlik grubu | **Temel**|
    | Genel gelen bağlantı noktaları | **Hiçbiri** seçeneğini belirtin. |
   
5. **Gözden geçir ve oluştur**’u seçin. 
  
6. Ayarları gözden geçirin ve ardından **Oluştur**' u seçin.

## <a name="create-storage-account-with-a-private-endpoint"></a>Özel bir uç nokta ile depolama hesabı oluşturma

Bu bölümde, bir depolama hesabı oluşturacak ve özel uç noktayı yapılandıracaksınız.

1. Sol taraftaki menüde, **kaynak**  >  **depolama**  >  **depolama hesabı** oluştur ' u veya arama kutusunda **depolama hesabı** ara ' yı seçin.

2. **Depolama hesabı oluştur** ' un **temel kavramlar** sekmesinde aşağıdaki bilgileri girin veya seçin:

    | Ayar | Değer                                          |
    |-----------------------|----------------------------------|
    | **Proje ayrıntıları** |  |
    | Abonelik | Azure aboneliğinizi seçin |
    | Kaynak Grubu | **Myresourcegroup** seçin |
    | **Örnek ayrıntıları** |  |
    | Depolama hesabı adı | **Mystorageaccount** değerini girin. Ad kullanılamıyorsa, benzersiz bir ad girin. |
    | Konum | **Doğu ABD** seçin |
    | Performans | Varsayılan **Standart** kalsın |
    | Hesap türü | Varsayılan depolamayı bırak **(genel amaçlı v2)** |
    | Çoğaltma| Varsayılan **Okuma Erişimli Coğrafi olarak yedekli depolamayı (RA-GRS)** bırakın |
   
3. **Ağ** sekmesini seçin veya **Sonraki: ağ** düğmesini seçin.

4. **Ağ** sekmesinde, **bağlantı yöntemi** altında **Özel uç nokta**' ı seçin.

5. **Özel uç noktada** **+ Ekle**' yi seçin.

6. **Özel uç nokta oluştur** ' da aşağıdaki bilgileri girin veya seçin:

    | Ayar | Değer                                          |
    |-----------------------|----------------------------------|
    | Abonelik | Azure aboneliğinizi seçin |
    | Kaynak Grubu | **Myresourcegroup** seçin |
    | Konum | **Doğu ABD** seçin |
    | Name | **Myprivateendpoint** girin |
    | Depolama alt kaynağı | Varsayılan **blobu** bırak |
    | **Ağ** |  |
    | Sanal ağ | **Myvnet** seçin |
    | Alt ağ | **mySubnet** öğesini seçin |
    | **Özel DNS tümleştirme** |
    | Özel DNS bölgesi ile tümleştirme | Varsayılan değeri bırak **Evet** |
    | Özel DNS Bölgesi | Varsayılan (yeni) privatelink.blob.core.windows.net kalsın |

7. **Tamam**’ı seçin.

8. **Gözden geçir ve oluştur**’u seçin.

9. **Oluştur**’u seçin.

10. Sol taraftaki Gezinti bölmesinde **kaynak grupları** ' nı seçin.

11. **myResourceGroup** öğesini seçin.

12. Önceki adımlarda oluşturduğunuz depolama hesabını seçin.

13. Depolama hesabının **Ayarlar** bölümünde **erişim anahtarları**' nı seçin.

14. **KEY1** için **bağlantı dizesinde** Kopyala ' yı seçin.

## <a name="test-connectivity-to-private-endpoint"></a>Özel uç nokta ile bağlantıyı sına

Bu bölümde, önceki adımda oluşturduğunuz sanal makineyi kullanarak özel uç nokta genelinde depolama hesabına bağlanırsınız.

1. Sol taraftaki Gezinti bölmesinde **kaynak grupları** ' nı seçin.

2. **myResourceGroup** öğesini seçin.

3. **Myvm**' yi seçin.

4. **Myvm** için genel bakış sayfasında **Bağlan** ' ı **seçin.**

5. Mavi **kullanımı** savunma düğmesini seçin.

6. Sanal makine oluşturma sırasında girdiğiniz kullanıcı adını ve parolayı girin.

7. Bağlandıktan sonra sunucuda Windows PowerShell 'i açın.

8. `nslookup <storage-account-name>.blob.core.windows.net` yazın. **\<storage-account-name>** Önceki adımlarda oluşturduğunuz depolama hesabının adıyla değiştirin.  Aşağıda görüntülene benzer bir ileti alacaksınız:

    ```powershell
    Server:  UnKnown
    Address:  168.63.129.16

    Non-authoritative answer:
    Name:    mystorageaccount8675.privatelink.blob.core.windows.net
    Address:  10.1.0.5
    Aliases:  mystorageaccount8675.blob.core.windows.net
    ```

    Depolama hesabı adı için **10.1.0.5** özel IP adresi döndürülür.  Bu adres, daha önce oluşturduğunuz sanal ağın alt ağıdır.

9. [Microsoft Azure Depolama Gezgini](../vs-azure-tools-storage-manage-with-storage-explorer.md?tabs=windows&toc=%2fazure%2fstorage%2fblobs%2ftoc.json) sanal makineye yükler.

10. **Microsoft Azure Depolama Gezgini** yüklendikten sonra **son** ' u seçin.  Uygulamayı açmak için kutuyu işaretli bırakın.

11. **Azure depolama 'Ya Bağlan** ekranında **bağlantı dizesi kullan**' ı seçin.

12. **İleri**’yi seçin.

13. **Görünen ad**' daki önceki adımlardan depolama hesabı adınızı girin.

14. **Bağlantı dizesi** altındaki kutuya, bağlantı dizesini önceki adımlarda kopyaladığınız depolama hesabından yapıştırın.

15. **İleri**’yi seçin.

16. **Bağlantı özetinde** ayarların doğru olduğundan emin olun.  

17. **Bağlan**’ı seçin.

18. **Myvm** bağlantısını kapatın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu uygulamayı kullanmaya devam etmeyecekecekseniz, sanal ağ, sanal makine ve depolama hesabını aşağıdaki adımlarla silin:

1. Sol taraftaki menüden **kaynak grupları**' nı seçin.

2. **myResourceGroup** öğesini seçin.

3. **Kaynak grubunu sil**'i seçin.

4. **Kaynak grubu adını türüne** **myresourcegroup** girin.

5. **Sil**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Özel bağlantı hizmeti oluşturmayı öğrenin:
> [!div class="nextstepaction"]
> [Özel Bağlantı hizmeti oluşturma](create-private-link-service-portal.md)