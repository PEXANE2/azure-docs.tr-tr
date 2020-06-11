---
title: Azure özel uç nokta ARM şablonu
description: Azure özel bağlantısı hakkında bilgi edinin
services: private-link
author: mblanco77
ms.service: private-link
ms.topic: quickstart
ms.custom: subject-armqs
ms.date: 05/26/2020
ms.author: allensu
ms.openlocfilehash: af00119f1da3368b8592e020eee1ebb2a39a8501
ms.sourcegitcommit: eeba08c8eaa1d724635dcf3a5e931993c848c633
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/10/2020
ms.locfileid: "84669962"
---
# <a name="quickstart-create-a-private-endpoint---resource-manager-template"></a>Hızlı başlangıç: özel uç nokta Kaynak Yöneticisi şablonu oluşturma

Bu hızlı başlangıçta özel uç nokta oluşturmak için bir Kaynak Yöneticisi şablonu kullanırsınız.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Bu hızlı başlangıcı [Azure Portal](create-private-endpoint-portal.md), [Azure POWERSHELL](create-private-endpoint-powershell.md)veya [Azure CLI](create-private-endpoint-cli.md)kullanarak da tamamlayabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="create-an-private-endpoint"></a>Özel uç nokta oluşturma

Bu şablon, bir Azure SQL Server için özel bir uç nokta oluşturur.

### <a name="review-the-template"></a>Şablonu gözden geçirme

Bu hızlı başlangıçta kullanılan şablon [Azure hızlı başlangıç şablonlarından](https://azure.microsoft.com/resources/templates/101-private-endpoint-sql/).

:::code language="json" source="~/quickstart-templates/101-private-endpoint-sql/azuredeploy.json" range="001-295" highlight="131-156":::

Şablonda birden çok Azure kaynağı tanımlanmış:

- [**Microsoft. SQL/Servers**](/azure/templates/microsoft.sql/servers) : örnek veritabanıyla Azure SQL Server
- [**Microsoft. SQL/Servers/veritabanları**](/azure/templates/microsoft.sql/servers/databases) : örnek veritabanı
- [**Microsoft. Network/virtualNetworks**](/azure/templates/microsoft.network/virtualnetworks) : özel uç noktanın dağıtıldığı sanal ağ
- [**Microsoft. Network/privateEndpoints**](/azure/templates/microsoft.network/privateendpoints) : özel olarak Azure SQL Server 'a erişmek için özel uç nokta
- [**Microsoft. Network/privateDnsZones**](/azure/templates/microsoft.network/privatednszones) : özel uç nokta IP adresini çözümlemek için kullanılır
- [**Microsoft. Network/privateDnsZones/virtualNetworkLinks**](/azure/templates/microsoft.network/privatednszones/virtualnetworklinks)
- Özel uç noktayı özel bir DNS bölgesi ile ilişkilendirmek için [**Microsoft. Network/privateEndpoints/privateDnsZoneGroups**](/azure/templates/microsoft.network/privateendpoints/privateDnsZoneGroups) :
- [**Microsoft. Network/Publicıpaddresses**](/azure/templates/microsoft.network/publicIpAddresses) : sanal makineye erişmek IÇIN genel IP adresi
- [**Microsoft. Network/NetworkInterfaces**](/azure/templates/microsoft.network/networkinterfaces) : sanal makine Için ağ arabirimi
- [**Microsoft. COMPUTE/virtualMachines**](/azure/templates/microsoft.compute/virtualmachines) : özel uç noktadan Azure SQL Server 'a özel bağlantıyı test etmek için sanal makine

### <a name="deploy-the-template"></a>Şablonu dağıtma

Kaynak Yöneticisi şablonu Azure 'a dağıtma:

1. Azure 'da oturum açmak için **Azure 'A dağıt** ' ı seçin ve şablonu açın. Şablon özel uç nokta, Azure SQL Server, ağ altyapısı ve doğrulanacak bir sanal makine oluşturur.

   [![Azure’a dağıtma](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-private-endpoint-sql%2Fazuredeploy.json)

2. Kaynak grubunuzu seçin veya oluşturun,
3. SQL Yöneticisi oturum açma bilgilerini ve parolayı yazın
4. Sanal Makine Yöneticisi Kullanıcı adını ve parolasını yazın.
5. **Yukarıda belirtilen hüküm ve koşulları kabul ediyorum** ' u seçin ve ardından **satın al**' ı seçin. Dağıtımın tamamlanması 20 dakika veya daha uzun sürebilir.

## <a name="validate-the-deployment"></a>Dağıtımı doğrulama

> [!NOTE]
> ARM şablonu, myVm<b>{UniqueId}</b> kaynağı ve Azure SQL Server SqlServer<b>{UniqueId}</b> kaynağı için benzersiz bir ad oluşturuyor, lütfen <b>{UniqueId}</b> değerini üretilen değer ile değiştirin.

### <a name="connect-to-a-vm-from-the-internet"></a>İnternet'ten bir sanal makineye bağlanma

İnternet 'ten VM _{UniqueId}_ sanal makinesine şu şekilde bağlanın:

1. Portalın arama çubuğunda _Myvm {UniqueId}_ girin.

2. **Bağlan** düğmesini seçin. **Bağlan** düğmesini seçtikten sonra **sanal makineye bağlan** açılır.

3. **RDP dosyasını indir**' i seçin. Azure bir Uzak Masaüstü Protokolü (_. rdp_) dosyası oluşturur ve bilgisayarınıza indirir.

4. İndirilen. rdp dosyasını açın \* .

   a. İstendiğinde **Bağlan**’ı seçin.

   b. VM oluştururken belirttiğiniz kullanıcı adını ve parolayı girin.

      > [!NOTE]
      > **More choices**  >  VM oluştururken girdiğiniz kimlik bilgilerini belirtmek için**farklı bir hesap kullan**' ı seçmeniz gerekebilir.

5. **Tamam**’ı seçin.

6. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Bir sertifika uyarısı alırsanız **Evet** ' i veya **devam et**' i seçin.

7. VM masaüstü seçildikten sonra, bunu yerel masaüstünüze geri dönmek için simge durumuna küçültün.

### <a name="access-sql-database-server-privately-from-the-vm"></a>SQL veritabanı sunucusuna VM 'den özel olarak erişme

Bu bölümde, Özel uç nokta kullanarak VM 'den SQL veritabanı sunucusuna bağlanacaksınız.

1.  _Myvm {UniqueId}_ uzak masaüstünde PowerShell 'i açın.
2.  Nslookup SqlServer {UniqueId}. Database. Windows. net ' i girerek   Şuna benzer bir ileti alacaksınız:

    ```
      Server:  UnKnown
      Address:  168.63.129.16
      Non-authoritative answer:
      Name:    sqlserver.privatelink.database.windows.net
      Address:  10.0.0.5
      Aliases:  sqlserver.database.windows.net
    ```

3.  SQL Server Management Studio yüklensin
4.  Sunucuya Bağlan ' da bu bilgileri girin veya seçin: sunucu türü: veritabanı altyapısını seçin.
    Sunucu adı: Select SqlServer {UniqueId}. Database. Windows. net username: oluşturma sırasında sağlanmış bir Kullanıcı adı girin.
    Parola: oluşturma sırasında bir parola girin.
    Parolayı anımsa: Evet ' i seçin.

5.  **Bağlan**'ı seçin.
6.  Sol menüden **veritabanlarına** gözatamazsınız.
7.  I _Örnek DB_ 'den bilgi oluşturma veya sorgulama
8.  _Myvm {UniqueId}_ ile uzak masaüstü bağlantısını kapatın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Özel uç nokta ile oluşturduğunuz kaynaklara artık ihtiyacınız kalmadığında, kaynak grubunu silin. Bu, Özel uç noktayı ve ilgili tüm kaynakları kaldırır.

Kaynak grubunu silmek için `Remove-AzResourceGroup` cmdlet 'ini çağırın:

```azurepowershell-interactive
Remove-AzResourceGroup -Name <your resource group name>
```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure özel bağlantısı](private-link-overview.md) hakkında daha fazla bilgi edinin
