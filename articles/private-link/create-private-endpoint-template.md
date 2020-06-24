---
title: Azure özel bağlantısında özel uç nokta oluşturma
description: Bu hızlı başlangıçta özel uç nokta oluşturmak için bir Azure Resource Manager şablonu kullanırsınız.
services: private-link
author: mblanco77
ms.service: private-link
ms.topic: quickstart
ms.custom: subject-armqs
ms.date: 05/26/2020
ms.author: allensu
ms.openlocfilehash: a60edde222a6200a0378cd8c9c4f4774da9c2e50
ms.sourcegitcommit: 1383842d1ea4044e1e90bd3ca8a7dc9f1b439a54
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/16/2020
ms.locfileid: "84817959"
---
# <a name="quickstart-create-a-private-endpoint-by-using-an-azure-resource-manager-template"></a>Hızlı başlangıç: Azure Resource Manager şablonu kullanarak özel uç nokta oluşturma

Bu hızlı başlangıçta özel uç nokta oluşturmak için bir Azure Resource Manager şablonu kullanırsınız.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Bu hızlı başlangıcı [Azure Portal](create-private-endpoint-portal.md), [Azure POWERSHELL](create-private-endpoint-powershell.md)veya [Azure CLI](create-private-endpoint-cli.md)kullanarak da tamamlayabilirsiniz.

## <a name="prerequisite"></a>Önkoşul

Etkin aboneliği olan bir Azure hesabınızın olması gerekir. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="create-a-private-endpoint"></a>Özel uç nokta oluşturma

Bu şablon bir Azure SQL veritabanı örneği için özel bir uç nokta oluşturur.

### <a name="review-the-template"></a>Şablonu gözden geçirme

Bu hızlı başlangıçta kullanılan şablon [Azure hızlı başlangıç şablonlarından](https://azure.microsoft.com/resources/templates/).

:::code language="json" source="~/quickstart-templates/101-private-endpoint-sql/azuredeploy.json" range="001-295" highlight="131-156":::

Şablonda birden çok Azure kaynağı tanımlanmış:

- [**Microsoft. SQL/Servers**](/azure/templates/microsoft.sql/servers): örnek veritabanıyla SQL veritabanı örneği.
- [**Microsoft. SQL/Servers/veritabanları**](/azure/templates/microsoft.sql/servers/databases): örnek veritabanı.
- [**Microsoft. Network/virtualNetworks**](/azure/templates/microsoft.network/virtualnetworks): özel uç noktanın dağıtıldığı sanal ağ.
- [**Microsoft. Network/privateEndpoints**](/azure/templates/microsoft.network/privateendpoints): SQL veritabanı örneğine erişmek için özel uç nokta.
- [**Microsoft. Network/privateDnsZones**](/azure/templates/microsoft.network/privatednszones): özel uç nokta IP adresini çözümlemek için kullanılan bölge.
- [**Microsoft. Network/privateDnsZones/virtualNetworkLinks**](/azure/templates/microsoft.network/privatednszones/virtualnetworklinks)
- [**Microsoft. Network/privateEndpoints/privateDnsZoneGroups**](/azure/templates/microsoft.network/privateendpoints/privateDnsZoneGroups): özel uç noktasını özel bir DNS bölgesi ile ilişkilendirmek için kullanılan bölge grubu.
- [**Microsoft. Network/Publicıpaddresses**](/azure/templates/microsoft.network/publicIpAddresses): sanal makineye erişmek için kullanılan genel IP adresi.
- [**Microsoft. Network/NetworkInterfaces**](/azure/templates/microsoft.network/networkinterfaces): sanal makine için ağ arabirimi.
- [**Microsoft. COMPUTE/virtualMachines**](/azure/templates/microsoft.compute/virtualmachines): özel uç noktayla SQL veritabanı örneğine özel bağlantıyı test etmek için kullanılan sanal makine.

### <a name="deploy-the-template"></a>Şablonu dağıtma

Azure Resource Manager şablonunu Azure 'a dağıtma:

1. Azure 'da oturum açmak ve şablonu açmak için **Azure 'A dağıt**' ı seçin. Şablon özel uç nokta, SQL veritabanı örneği, ağ altyapısı ve doğrulanacak bir sanal makine oluşturur.

   [![Azure’a dağıtma](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-private-endpoint-sql%2Fazuredeploy.json)

2. Kaynak grubunuzu seçin veya oluşturun.
3. SQL Yöneticisi oturum açma ve parolasını yazın.
4. Sanal Makine Yöneticisi Kullanıcı adını ve parolasını yazın.
5. Hüküm ve koşullar ekstresini okuyun. Kabul ediyorsanız, satın alma sırasında **belirtilen hüküm ve koşulları kabul**ediyorum ' u seçin  >  **Purchase**. Dağıtımın tamamlanması 20 dakika veya daha uzun sürebilir.

## <a name="validate-the-deployment"></a>Dağıtımı doğrulama

> [!NOTE]
> Azure Resource Manager şablonu, myVm<b>{UniqueId}</b> kaynağı sanal MAKINESI ve SQL veritabanı SqlServer<b>{UniqueId}</b> kaynağı için benzersiz bir ad oluşturur. **{UniqueId}** için üretilen değeri değiştirin.

### <a name="connect-to-a-vm-from-the-internet"></a>İnternet'ten bir sanal makineye bağlanma

İnternet 'ten VM _{UniqueId}_ sanal makinesine şu şekilde bağlanın:

1. Portalın arama çubuğunda _Myvm {UniqueId}_ girin.

2. **Bağlan**'ı seçin. **Sanal makineye bağlan** açılır.

3. **RDP dosyasını indir**' i seçin. Azure bir Uzak Masaüstü Protokolü (_. rdp_) dosyası oluşturur ve bilgisayarınıza indirir.

4. İndirilen .rdp dosyasını açın.

   a. İstendiğinde **Bağlan**’ı seçin.

   b. VM oluştururken belirttiğiniz kullanıcı adını ve parolayı girin.

      > [!NOTE]
      > **More choices**  >  VM oluştururken girdiğiniz kimlik bilgilerini belirtmek için**farklı bir hesap kullan**' ı seçmeniz gerekebilir.

5. **Tamam**’ı seçin.

6. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Bir sertifika uyarısı alırsanız **Evet** ' i veya **devam et**' i seçin.

7. VM masaüstü görüntülendikten sonra, bunu yerel masaüstünüze geri dönmek için simge durumuna küçültün.

### <a name="access-the-sql-database-server-privately-from-the-vm"></a>SQL veritabanı sunucusuna VM 'den özel olarak erişme

Özel uç nokta kullanarak VM 'den SQL veritabanı sunucusuna nasıl bağlanaöğreneceksiniz.

1.  _Myvm {UniqueId}_ uzak masaüstünde PowerShell 'i açın.
2.  Şunu girin: nslookup SqlServer {UniqueId}. Database. Windows. net. 
    Şuna benzer bir ileti alacaksınız:

    ```
      Server:  UnKnown
      Address:  168.63.129.16
      Non-authoritative answer:
      Name:    sqlserver.privatelink.database.windows.net
      Address:  10.0.0.5
      Aliases:  sqlserver.database.windows.net
    ```

3.  SQL Server Management Studio 'i yükler.
4.   **Sunucuya Bağlan**' da bu bilgileri girin veya seçin:
    - **Sunucu türü**: **veritabanı altyapısını**seçin.
    - **Sunucu adı**: **SqlServer {UniqueId}. Database. Windows. net**öğesini seçin.
    - **Kullanıcı adı**: oluşturma sırasında sağlanmış bir Kullanıcı adı girin.
    - **Parola**: oluşturma sırasında bir parola girin.
    - **Parolayı anımsa**: **Evet**' i seçin.

5.  **Bağlan**'ı seçin.
6.  Soldaki menüden **veritabanlarına**gidin.
7.  İsteğe bağlı olarak, _örnek DB_'den bilgi oluşturabilir veya sorgulayabilirsiniz.
8.  _Myvm {UniqueId}_ Ile uzak masaüstü bağlantısını kapatın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Özel uç nokta ile oluşturduğunuz kaynaklara artık ihtiyacınız kalmadığında, kaynak grubunu silin. Bu, Özel uç noktayı ve ilgili tüm kaynakları kaldırır.

Kaynak grubunu silmek için `Remove-AzResourceGroup` cmdlet 'ini çağırın:

```azurepowershell-interactive
Remove-AzResourceGroup -Name <your resource group name>
```

## <a name="next-steps"></a>Sonraki adımlar

[Azure özel bağlantısı](private-link-overview.md)hakkında daha fazla bilgi edinin.
