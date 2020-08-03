---
title: Azure Özel Bağlantı Hizmeti ile tümleştirme
description: Azure Key Vault Azure özel bağlantı hizmeti ile tümleştirme hakkında bilgi edinin
author: ShaneBala-keyvault
ms.author: sudbalas
ms.date: 03/08/2020
ms.service: key-vault
ms.subservice: general
ms.topic: quickstart
ms.custom: devx-track-azurecli
ms.openlocfilehash: 70a0620369792c1aaf2c11867fd468f42d6bb9ef
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87494698"
---
# <a name="integrate-key-vault-with-azure-private-link"></a>Key Vault'u Azure Özel Bağlantı ile tümleştirme

Azure özel bağlantı hizmeti, Azure hizmetlerine (örneğin, Azure Key Vault, Azure depolama ve Azure Cosmos DB) ve Azure 'da barındırılan müşteri/iş ortağı hizmetlerine sanal ağınızdaki özel bir uç nokta üzerinden erişmenizi sağlar.

Azure özel uç noktası, Azure özel bağlantısı tarafından desteklenen bir hizmete özel ve güvenli bir şekilde bağlanan bir ağ arabirimidir. Özel uç nokta, sanal ağınızdan bir özel IP adresi kullanarak hizmeti sanal ağınıza etkin bir şekilde getiriyor. Hizmete giden tüm trafik özel uç nokta aracılığıyla yönlendirilebilir, bu nedenle ağ geçitleri, NAT cihazları, ExpressRoute veya VPN bağlantıları ya da genel IP adresleri gerekmez. Sanal ağınız ve hizmet arasındaki trafik, Microsoft omurga ağı üzerinden geçer ve genel İnternet’ten etkilenme olasılığı ortadan kaldırılır. Bir Azure kaynağı örneğine bağlanarak, erişim denetimi için en yüksek düzeyde ayrıntı düzeyi sağlayabilirsiniz.

Daha fazla bilgi için bkz. [Azure özel bağlantısı nedir?](../../private-link/private-link-overview.md)

## <a name="prerequisites"></a>Ön koşullar

Bir anahtar kasasını Azure özel bağlantısıyla bütünleştirmek için şunlar gerekir:

- Bir Anahtar Kasası.
- Bir Azure sanal ağı.
- Sanal ağdaki bir alt ağ.
- Hem Anahtar Kasası hem de sanal ağ için sahip veya katkıda bulunan izinleri.

Özel uç noktanız ve sanal ağınız aynı bölgede olmalıdır. Portalı kullanarak özel uç nokta için bir bölge seçtiğinizde bu, yalnızca o bölgedeki sanal ağları otomatik olarak filtreleyecek. Anahtar kasanızın farklı bir bölgede olması olabilir.

Özel uç noktanız sanal ağınızda özel bir IP adresi kullanıyor.

## <a name="establish-a-private-link-connection-to-key-vault-using-the-azure-portal"></a>Key Vault Azure portal kullanarak bir özel bağlantı bağlantısı kurun 

İlk olarak, [Azure Portal kullanarak sanal ağ oluşturma](../../virtual-network/quick-create-portal.md) bölümündeki adımları izleyerek bir sanal ağ oluşturun

Daha sonra yeni bir Anahtar Kasası oluşturabilir veya var olan bir anahtar kasası ile özel bağlantı kurabilirsiniz.

### <a name="create-a-new-key-vault-and-establish-a-private-link-connection"></a>Yeni bir Anahtar Kasası oluşturma ve özel bağlantı bağlantısı kurma

[Azure Key Vault Azure Portal kullanarak bir gizli anahtarı ayarlama ve alma](../secrets/quick-create-portal.md) bölümündeki adımları izleyerek yeni bir Anahtar Kasası oluşturabilirsiniz

Anahtar Kasası temel bilgilerini yapılandırdıktan sonra, ağ sekmesini seçin ve şu adımları izleyin:

1. Ağ sekmesinde Özel uç nokta radyo düğmesini seçin.
1. Özel uç nokta eklemek için "+ Ekle" düğmesine tıklayın.

    ![Görüntü](../media/private-link-service-1.png)
 
1. Özel uç nokta oluştur dikey penceresinin "konum" alanında, sanal ağınızın bulunduğu bölgeyi seçin. 
1. "Ad" alanında, bu özel uç noktayı tanımlamanızı sağlayacak açıklayıcı bir ad oluşturun. 
1. Bu özel bitiş noktasının, açılan menüden oluşturulmasını istediğiniz sanal ağı ve alt ağı seçin. 
1. "Özel bölge DNS ile tümleştirin" seçeneğini değiştirmeden bırakın.  
1. "Tamam" ı seçin.

    ![Görüntü](../media/private-link-service-8.png)
 
Artık yapılandırılmış özel uç noktayı görebileceksiniz. Artık bu özel uç noktayı silme ve düzenleme seçeneğiniz vardır. "Gözden geçir + oluştur" düğmesini seçin ve Anahtar Kasası oluşturun. Dağıtımın tamamlanması 5-10 dakika sürer. 

### <a name="establish-a-private-link-connection-to-an-existing-key-vault"></a>Mevcut bir anahtar kasası ile özel bağlantı kurun

Zaten bir anahtar kasanız varsa, aşağıdaki adımları izleyerek bir özel bağlantı bağlantısı oluşturabilirsiniz:

1. Azure portalında oturum açın. 
1. Arama çubuğuna "Anahtar Kasası" yazın
1. Özel uç nokta eklemek istediğiniz listeden anahtar kasasını seçin.
1. Ayarlar altında "ağ" sekmesini seçin
1. Sayfanın üst kısmındaki özel uç nokta bağlantıları sekmesini seçin
1. Sayfanın üst kısmındaki "+ özel uç nokta" düğmesini seçin.

    ![Görüntü ](../media/private-link-service-3.png) ![ resmi](../media/private-link-service-4.png)

Bu dikey pencereyi kullanarak, içindeki herhangi bir Azure kaynağı için özel bir uç nokta oluşturmayı tercih edebilirsiniz. Açılır menüleri kullanarak bir kaynak türü seçebilir ve dizininizdeki bir kaynağı seçebilir ya da kaynak KIMLIĞI kullanarak herhangi bir Azure kaynağına bağlanabilirsiniz. "Özel bölge DNS ile tümleştirin" seçeneğini değiştirmeden bırakın.  

![Görüntü ](../media/private-link-service-3.png)
 ![ resmi](../media/private-link-service-4.png)

## <a name="establish-a-private-link-connection-to-key-vault-using-cli"></a>CLı kullanarak Key Vault özel bağlantı bağlantısı kurma

### <a name="login-to-azure-cli"></a>Azure CLı 'da oturum açma
```console
az login 
```
### <a name="select-your-azure-subscription"></a>Azure aboneliğinizi seçin 
```console
az account set --subscription {AZURE SUBSCRIPTION ID}
```
### <a name="create-a-new-resource-group"></a>Yeni Kaynak Grubu oluşturma 
```console
az group create -n {RG} -l {AZURE REGION}
```
### <a name="register-microsoftkeyvault-as-a-provider"></a>Microsoft. Keykasasını sağlayıcı olarak kaydetme 
```console
az provider register -n Microsoft.KeyVault
```
### <a name="create-a-new-key-vault"></a>Yeni bir Key Vault oluştur
```console
az keyvault create --name {KEY VAULT NAME} --resource-group {RG} --location {AZURE REGION}
```
### <a name="turn-on-key-vault-firewall"></a>Key Vault güvenlik duvarını Aç
```console
az keyvault update --name {KEY VAULT NAME} --resource-group {RG} --default-action deny
```
### <a name="create-a-virtual-network"></a>Sanal Ağ Oluşturma
```console
az network vnet create --resource-group {RG} --name {vNet NAME} --location {AZURE REGION}
```
### <a name="add-a-subnet"></a>Alt ağ ekleme
```console
az network vnet subnet create --resource-group {RG} --vnet-name {vNet NAME} --name {subnet NAME} --address-prefixes {addressPrefix}
```
### <a name="disable-virtual-network-policies"></a>Sanal ağ Ilkelerini devre dışı bırak 
```console
az network vnet subnet update --name {subnet NAME} --resource-group {RG} --vnet-name {vNet NAME} --disable-private-endpoint-network-policies true
```
### <a name="add-a-private-dns-zone"></a>Özel DNS bölgesi ekleme 
```console
az network private-dns zone create --resource-group {RG} --name privatelink.vaultcore.azure.net
```
### <a name="link-private-dns-zone-to-virtual-network"></a>Özel DNS bölgesini sanal ağa bağlama 
```console
az network private-dns link vnet create --resource-group {RG} --virtual-network {vNet NAME} --zone-name privatelink.vaultcore.azure.net --name {dnsZoneLinkName} --registration-enabled true
```
### <a name="add-private-dns-records"></a>Özel DNS kayıtları ekleme
```console
# https://docs.microsoft.com/en-us/azure/dns/private-dns-getstarted-cli#create-an-additional-dns-record
az network private-dns zone list -g $rg_name
az network private-dns record-set a add-record -g $rg_name -z "privatelink.vaultcore.azure.net" -n $vault_name -a $kv_network_interface_private_ip
az network private-dns record-set list -g $rg_name -z "privatelink.vaultcore.azure.net"

# From home/public network, you wil get a public IP. If inside a vnet with private zone, nslookup will resolve to the private ip.
nslookup $vault_name.vault.azure.net
nslookup $vault_name.privatelink.vaultcore.azure.net
```
### <a name="create-a-private-endpoint-automatically-approve"></a>Özel uç nokta oluştur (otomatik olarak Onayla) 
```console
az network private-endpoint create --resource-group {RG} --vnet-name {vNet NAME} --subnet {subnet NAME} --name {Private Endpoint Name}  --private-connection-resource-id "/subscriptions/{AZURE SUBSCRIPTION ID}/resourceGroups/{RG}/providers/Microsoft.KeyVault/vaults/ {KEY VAULT NAME}" --group-ids vault --connection-name {Private Link Connection Name} --location {AZURE REGION}
```
### <a name="create-a-private-endpoint-manually-request-approval"></a>Özel uç nokta oluşturma (El Ile onay ISTEME) 
```console
az network private-endpoint create --resource-group {RG} --vnet-name {vNet NAME} --subnet {subnet NAME} --name {Private Endpoint Name}  --private-connection-resource-id "/subscriptions/{AZURE SUBSCRIPTION ID}/resourceGroups/{RG}/providers/Microsoft.KeyVault/vaults/ {KEY VAULT NAME}" --group-ids vault --connection-name {Private Link Connection Name} --location {AZURE REGION} --manual-request
```
### <a name="show-connection-status"></a>Bağlantı durumunu göster 
```console
az network private-endpoint show --resource-group {RG} --name {Private Endpoint Name}
```
## <a name="manage-private-link-connection"></a>Özel bağlantı bağlantısını Yönet

Özel bir uç nokta oluşturduğunuzda bağlantının onaylanması gerekir. Özel bir uç noktası oluşturduğunuz kaynak dizininizdeki ise, yeterli izinlere sahip olduğunuz için bağlantı isteğini onaylayabileceksiniz; başka bir dizindeki bir Azure kaynağına bağlanıyorsanız, bu kaynağın sahibinin bağlantı isteğinizi onaylamasını beklemeniz gerekir.

Dört sağlama durumu vardır:

| Hizmet eylemi sağla | Hizmet tüketicisi özel uç nokta durumu | Açıklama |
|--|--|--|
| Hiçbiri | Beklemede | Bağlantı el ile oluşturulur ve özel bağlantı kaynağı sahibinden onay bekliyor. |
| Onaylama | Onaylandı | Bağlantı otomatik olarak veya el ile onaylandı ve kullanılabilir hale gelmiştir. |
| Reddet | Reddedildi | Bağlantı, özel bağlantı kaynağı sahibi tarafından reddedildi. |
| Kaldır | Bağlantı kesildi | Bağlantı, özel bağlantı kaynağı sahibi tarafından kaldırıldı, Özel uç nokta bilgilendirici hale gelir ve temizlik için silinmelidir. |
 
###  <a name="how-to-manage-a-private-endpoint-connection-to-key-vault-using-the-azure-portal"></a>Azure portal kullanarak Key Vault özel bir uç nokta bağlantısını yönetme 

1. Azure portalında oturum açın.
1. Arama çubuğuna "Anahtar Kasası" yazın
1. Yönetmek istediğiniz anahtar kasasını seçin.
1. "Ağ" sekmesini seçin.
1. Bekleyen bağlantı varsa, sağlama durumunda "bekliyor" ile listelenmiş bir bağlantı görürsünüz. 
1. Onaylamak istediğiniz özel uç noktayı seçin
1. Onayla düğmesini seçin.
1. Reddetmek istediğiniz özel uç nokta bağlantıları varsa, bekleyen bir istek ya da var olan bağlantı olup olmadığına bakılmaksızın bağlantıyı seçin ve "Reddet" düğmesine tıklayın.

    ![Görüntü](../media/private-link-service-7.png)

##  <a name="how-to-manage-a-private-endpoint-connection-to-key-vault-using-azure-cli"></a>Azure CLı kullanarak Key Vault özel bir uç nokta bağlantısını yönetme

### <a name="approve-a-private-link-connection-request"></a>Özel bağlantı bağlantısı Isteğini onaylama
```console
az keyvault private-endpoint-connection approve --approval-description {"OPTIONAL DESCRIPTION"} --resource-group {RG} --vault-name {KEY VAULT NAME} –name {PRIVATE LINK CONNECTION NAME}
```

### <a name="deny-a-private-link-connection-request"></a>Özel bağlantı bağlantısı Isteği reddetme
```console
az keyvault private-endpoint-connection reject --rejection-description {"OPTIONAL DESCRIPTION"} --resource-group {RG} --vault-name {KEY VAULT NAME} –name {PRIVATE LINK CONNECTION NAME}
```

### <a name="delete-a-private-link-connection-request"></a>Özel bağlantı bağlantısı Isteğini silme
```console
az keyvault private-endpoint-connection delete --resource-group {RG} --vault-name {KEY VAULT NAME} --name {PRIVATE LINK CONNECTION NAME}
```

## <a name="validate-that-the-private-link-connection-works"></a>Özel bağlantı bağlantısının çalışıp çalışmadığını doğrulama

Özel uç nokta kaynağının aynı alt ağı içindeki kaynakların, özel bir IP adresi üzerinden anahtar Kasanıza bağlı olduğunu ve doğru özel DNS bölge tümleştirmesini olduğunu doğrulamanız gerekir.

İlk olarak, [Azure Portal Windows sanal makinesi oluşturma](../../virtual-machines/windows/quick-create-portal.md) bölümündeki adımları izleyerek bir sanal makine oluşturun

"Ağ" sekmesinde:

1. Sanal ağ ve alt ağ belirtin. Yeni bir sanal ağ oluşturabilir veya var olan bir sanal ağı seçebilirsiniz. Mevcut bir tane seçilirse, bölgenin eşleştiğinden emin olun.
1. Genel bir IP kaynağı belirtin.
1. "NIC ağ güvenlik grubu" nda "hiçbiri" ni seçin.
1. "Yük Dengeleme" içinde "Hayır" ı seçin.

Komut satırını açın ve şu komutu çalıştırın:

```console
nslookup <your-key-vault-name>.vault.azure.net
```

Ortak bir uç nokta üzerinden bir anahtar kasasının IP adresini çözümlemek için NS arama komutunu çalıştırırsanız şuna benzer bir sonuç görürsünüz:

```console
c:\ >nslookup <your-key-vault-name>.vault.azure.net

Non-authoritative answer:
Name:    
Address:  (public IP address)
Aliases:  <your-key-vault-name>.vault.azure.net
```

Özel bir uç nokta üzerinden bir anahtar kasasının IP adresini çözümlemek için NS arama komutunu çalıştırırsanız şuna benzer bir sonuç görürsünüz:

```console
c:\ >nslookup your_vault_name.vault.azure.net

Non-authoritative answer:
Name:    
Address:  10.1.0.5 (private IP address)
Aliases:  <your-key-vault-name>.vault.azure.net
          <your-key-vault-name>.privatelink.vaultcore.azure.net
```

## <a name="troubleshooting-guide"></a>Sorun Giderme Kılavuzu

* Özel uç noktanın onaylanmış durumda olduğundan emin olmak için denetleyin. 
    1. Azure portal bunu denetleyebilir ve giderebilirsiniz. Key Vault kaynağını açın ve ağ seçeneğine tıklayın. 
    2. Ardından özel uç nokta bağlantıları sekmesini seçin. 
    3. Bağlantı durumunun onaylanmış ve sağlama durumunun başarılı olduğundan emin olun. 
    4. Ayrıca, Özel uç nokta kaynağına gidebilir ve aynı özellikleri orada gözden geçirebilir ve sanal ağın kullandığınız bir ile eşleşip eşleşmediğini iki kez kontrol edebilirsiniz.

* Özel DNS bölge kaynağınız olduğundan emin olun. 
    1. Tam adı olan bir Özel DNS Zone kaynağınız olmalıdır: privatelink.vaultcore.azure.net. 
    2. Bu ayarı nasıl ayarlayacağınızı öğrenmek için lütfen aşağıdaki bağlantıya bakın. [Özel DNS bölgeler](https://docs.microsoft.com/azure/dns/private-dns-privatednszone)
    
* Özel DNS bölgesinin sanal ağla bağlantılı olmadığından emin olmak için denetleyin. Hala genel IP adresini almaya devam ediyorsanız bu sorun olabilir. 
    1. Özel bölge DNS sanal ağa bağlı değilse, sanal ağdan kaynaklanan DNS sorgusu, anahtar kasasının genel IP adresini döndürür. 
    2. Azure portal Özel DNS bölgesi kaynağına gidin ve sanal ağ bağlantıları seçeneğine tıklayın. 
    4. Anahtar kasasına çağrı gerçekleştirecek sanal ağın listelenmesi gerekir. 
    5. Bu yoksa, ekleyin. 
    6. Ayrıntılı adımlar için, [sanal ağın özel DNS bölgeye bağlanması için](https://docs.microsoft.com/azure/dns/private-dns-getstarted-portal#link-the-virtual-network) aşağıdaki belgeye bakın

* Özel DNS bölgesinde Anahtar Kasası için bir kaydın eksik olmadığından emin olun. 
    1. Özel DNS bölgesi sayfasına gidin. 
    2. Genel Bakış ' A tıklayın ve anahtar kasanızın (ör. fabrikam) basit adına sahip bir kayıt olup olmadığını kontrol edin. Herhangi bir sonek belirtmeyin.
    3. Yazımı denetlediğinizden emin olun ve bir kayıt oluşturun ya da onarın. 3600 TTL (1 saat) kullanabilirsiniz. 
    4. Doğru özel IP adresini belirttiğinizden emin olun. 
    
* Bir kaydın doğru IP adresine sahip olduğundan emin olun. 
    1. Azure portal ' de özel uç nokta kaynağını açarak IP adresini doğrulayabilirsiniz 
    2. Azure portal (Key Vault kaynağında değil) Microsoft. Network/privateEndpoints kaynağına gidin
    3. Genel Bakış sayfasında ağ arabirimi ' ne bakın ve bu bağlantıya tıklayın. 
    4. Bağlantı, özel IP adresi özelliğini içeren NIC kaynağına genel bakışı gösterir. 
    5. Bunun, A kaydında belirtilen doğru IP adresi olduğunu doğrulayın.

## <a name="limitations-and-design-considerations"></a>Sınırlamalar ve tasarım konuları

> [!NOTE]
> Abonelik başına etkin olan özel uç noktaları olan anahtar kasalarının sayısı, ayarlanabilir bir kısıtlamadır. Aşağıda gösterilen sınır varsayılan sınırdır. Hizmetiniz için sınır artışı istemek istiyorsanız lütfen adresine bir e-posta gönderin akv-privatelink@microsoft.com . Bu istekleri durum temelinde onaylayacağız.

**Fiyatlandırma**: fiyatlandırma bilgileri için bkz. [Azure özel bağlantı fiyatlandırması](https://azure.microsoft.com/pricing/details/private-link/).

**Sınırlamalar**: Azure Key Vault Için özel uç nokta yalnızca Azure genel bölgelerinde kullanılabilir.

**Key Vault başına en fazla özel uç nokta sayısı**: 64.

**Abonelik başına özel uç noktalara sahip anahtar kasalarının varsayılan sayısı**: 400.

Daha fazla bilgi için bkz [. Azure özel bağlantı hizmeti: sınırlamalar](../../private-link/private-link-service-overview.md#limitations)

## <a name="next-steps"></a>Sonraki Adımlar

- [Azure özel bağlantısı](../../private-link/private-link-service-overview.md) hakkında daha fazla bilgi edinin
- [Azure Key Vault](overview.md) hakkında daha fazla bilgi edinin
