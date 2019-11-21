---
title: Quickstart - Create an Azure private DNS zone using the Azure portal
description: In this quickstart, you create and test a private DNS zone and record in Azure DNS. This is a step-by-step guide to create and manage your first private DNS zone and record using the Azure portal.
services: dns
author: asudbring
ms.service: dns
ms.topic: quickstart
ms.date: 10/11/2019
ms.author: allensu
ms.openlocfilehash: 48c5684dd491a91b6da212f78c535433ac2aecf7
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74210763"
---
# <a name="quickstart-create-an-azure-private-dns-zone-using-the-azure-portal"></a>Quickstart: Create an Azure private DNS zone using the Azure portal

This quickstart walks you through the steps to create your first private DNS zone and record using the Azure portal.

DNS bölgesi, belirli bir etki alanının DNS kayıtlarını barındırmak için kullanılır. Etki alanınızı Azure DNS'de barındırmaya başlamak için bir DNS bölgesi oluşturmanız gerekir. Ardından bu DNS bölgesinde etki alanınız için tüm DNS kayıtları oluşturulur. Sanal ağınızda özel bir DNS bölgesi yayımlamak için bölge içindeki kaynakları çözümleme izni olan sanal ağların listesini belirtmeniz gerekir.  These are called *linked* virtual networks. When autoregistration is enabled, Azure DNS also updates the zone records whenever a virtual machine is created, changes its' IP address, or is deleted.

Bu hızlı başlangıçta şunları yapmayı öğrenirsiniz:

> [!div class="checklist"]
> * Create a private DNS zone
> * Sanal ağ oluşturun
> * Link the virtual network
> * Test amaçlı sanal makineleri oluşturma
> * Ek bir DNS kaydı oluşturma
> * Özel bölgeyi test etme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

If you prefer, you can complete this quickstart using [Azure PowerShell](private-dns-getstarted-powershell.md) or [Azure CLI](private-dns-getstarted-cli.md).

## <a name="create-a-private-dns-zone"></a>Create a private DNS zone

The following example creates a DNS zone called **private.contoso.com** in a resource group called **MyAzureResourceGroup**.

A DNS zone contains the DNS entries for a domain. To start hosting your domain in Azure DNS, you create a DNS zone for that domain name.

![Private DNS zones search](media/private-dns-portal/search-private-dns.png)

1. On the portal search bar, type **private dns zones** in the search text box and press **Enter**.
1. Select **Private DNS zone**.
2. Select **Create private dns zone**.

1. On the **Create Private DNS zone** page, type or select the following values:

   - **Resource group**: Select **Create new**, enter *MyAzureResourceGroup*, and select **OK**. The resource group name must be unique within the Azure subscription. 
   -  **Name**: Type *private.contoso.com* for this example.
1. For **Resource group location**, select **West Central US**.

1. Select **Review + Create**.

1. **Oluştur**'u seçin.

Bölgenin oluşturulması birkaç dakika sürebilir.

## <a name="create-a-virtual-network"></a>Sanal ağ oluşturun

1. On the portal page upper left, select **Create a resource**, then **Networking**, then select **Virtual network**.
2. For **Name**, type **myAzureVNet**.
3. For **Resource group**, select **MyAzureResourceGroup**.
4. For **Location**, select **West Central US**.
5. Accept the other default values and select **Create**.

## <a name="link-the-virtual-network"></a>Link the virtual network

To link the private DNS zone to a virtual network, you create a virtual network link.

![Add virtual network link](media/private-dns-portal/dns-add-virtual-network-link.png)

1. Open the **MyAzureResourceGroup** resource group and select the **private.contoso.com** private zone.
2. On the left pane, select **Virtual network links**.
3. **Add (Ekle)** seçeneğini belirleyin.
4. Type **myLink** for the **Link name**.
5. For **Virtual network**, select **myAzureVNet**.
6. Select the **Enable auto registration** check box.
7. **Tamam**’ı seçin.

## <a name="create-the-test-virtual-machines"></a>Test amaçlı sanal makineleri oluşturma

Şimdi özel DNS bölgenizi test etmek için iki sanal makine oluşturun:

1. On the portal page upper left, select **Create a resource**, and then select **Windows Server 2016 Datacenter**.
1. Select **MyAzureResourceGroup** for the resource group.
1. Type **myVM01** - for the name of the virtual machine.
1. Select **West Central US** for the **Region**.
1. Type **azureadmin** for the administrator user name.
2. Type **Azure12345678** for the password and confirm the password.

5. For **Public inbound ports**, select **Allow selected ports**, and then select **RDP (3389)** for **Select inbound ports**.
10. Accept the other defaults for the page and then click **Next: Disks >** .
11. Accept the defaults on the **Disks** page, then click **Next: Networking >** .
1. Make sure that **myAzureVNet** is selected for the virtual network.
1. Accept the other defaults for the page, and then click **Next: Management >** .
2. For **Boot diagnostics**, select **Off**, accept the other defaults, and then select **Review + create**.
1. Review the settings and then click **Create**.

Repeat these steps and create another virtual machine named **myVM02**.

It will take a few minutes for both virtual machines to complete.

## <a name="create-an-additional-dns-record"></a>Ek bir DNS kaydı oluşturma

 The following example creates a record with the relative name **db** in the DNS Zone **private.contoso.com**, in resource group **MyAzureResourceGroup**. The fully qualified name of the record set is **db.private.contoso.com**. The record type is "A", with the IP address of **myVM01**.

1. Open the **MyAzureResourceGroup** resource group and select the **private.contoso.com** private zone.
2. **+ Kayıt kümesi**’ni seçin.
3. For **Name**, type **db**.
4. For **IP Address**, type the IP address you see for **myVM01**. This should be auto registered when the virtual machine started.
5. **Tamam**’ı seçin.

## <a name="test-the-private-zone"></a>Özel bölgeyi test etme

Now you can test the name resolution for your **private.contoso.com** private zone.

### <a name="configure-vms-to-allow-inbound-icmp"></a>Sanal makineleri gelen ICMP paketlerine izin verecek şekilde yapılandırma

Ad çözümlemesini test etmek için ping komutunu kullanabilirsiniz. Bunun için iki sanal makinedeki güvenlik duvarını da gelen ICMP paketlerine izin verecek şekilde yapılandırmanız gerekir.

1. myVM01 adlı sanal makineye bağlanın, yönetici ayrıcalıklarıyla bir Windows PowerShell penceresi açın.
2. Şu komutu çalıştırın:

   ```powershell
   New-NetFirewallRule –DisplayName "Allow ICMPv4-In" –Protocol ICMPv4
   ```

myVM02 için yineleyin.

### <a name="ping-the-vms-by-name"></a>Sanal makinelere ada göre ping gönderme

1. myVM02 Windows PowerShell komut isteminden otomatik olarak kaydedilen ana bilgisayar adını kullanarak myVM01 adlı makineye ping gönderin:
   ```
   ping myVM01.private.contoso.com
   ```
   Şuna benzer bir çıkışla karşılaşmanız gerekir:
   ```
   PS C:\> ping myvm01.private.contoso.com

   Pinging myvm01.private.contoso.com [10.2.0.4] with 32 bytes of data:
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128
   Reply from 10.2.0.4: bytes=32 time=1ms TTL=128
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128

   Ping statistics for 10.2.0.4:
       Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
   Approximate round trip times in milli-seconds:
       Minimum = 0ms, Maximum = 1ms, Average = 0ms
   PS C:\>
   ```
2. Şimdi önceden oluşturduğunuz **db** adına ping gönderin:
   ```
   ping db.private.contoso.com
   ```
   Şuna benzer bir çıkışla karşılaşmanız gerekir:
   ```
   PS C:\> ping db.private.contoso.com

   Pinging db.private.contoso.com [10.2.0.4] with 32 bytes of data:
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128

   Ping statistics for 10.2.0.4:
       Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
   Approximate round trip times in milli-seconds:
       Minimum = 0ms, Maximum = 0ms, Average = 0ms
   PS C:\>
   ```

## <a name="delete-all-resources"></a>Tüm kaynakları silme

When no longer needed, delete the **MyAzureResourceGroup** resource group to delete the resources created in this quickstart.


## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure DNS Private Zones scenarios](private-dns-scenarios.md)

