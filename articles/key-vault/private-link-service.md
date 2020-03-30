---
title: Azure Özel Bağlantı Hizmeti ile tümleştirme
description: Azure Anahtar Kasası'nı Azure Özel Bağlantı Hizmeti ile nasıl entegre edebilirsiniz öğrenin
author: ShaneBala-keyvault
ms.author: sudbalas
ms.date: 03/08/2020
ms.service: key-vault
ms.topic: quickstart
ms.openlocfilehash: c24be648e4ca1433c7c2af3d659bf4520a7a188c
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79457296"
---
# <a name="integrate-key-vault-with-azure-private-link"></a>Anahtar Kasası'nı Azure Özel Bağlantısıyla Tümleştir

Azure Özel Bağlantı Hizmeti, Azure Hizmetlerine (örneğin Azure Key Vault, Azure Depolama ve Azure Cosmos DB) ve Azure barındırılan müşteri/iş ortağı hizmetlerine sanal ağınızdaki Özel Bitiş Noktası üzerinden erişmenizi sağlar.

Azure Özel Bitiş Noktası, sizi Azure Özel Bağlantısı ile çalışan bir hizmete özel ve güvenli bir şekilde bağlayan bir ağ arabirimidir. Özel bitiş noktası, VNet'inizden gelen özel bir IP adresini kullanır ve hizmeti VNet'inize etkin bir şekilde getirir. Hizmete giden tüm trafik özel bitiş noktasından yönlendirilebilir, bu nedenle ağ geçidi, NAT aygıtları, ExpressRoute veya VPN bağlantıları veya genel IP adresleri gerekmez. Sanal ağınız ve hizmet arasındaki trafik, Microsoft omurga ağı üzerinden geçer ve genel İnternet’ten etkilenme olasılığı ortadan kaldırılır. Bir Azure kaynağıörneğine bağlanarak erişim denetiminde en yüksek parçalılık düzeyini sağlayabilirsiniz.

Daha fazla bilgi için Azure [Özel Bağlantı nedir?](../private-link/private-link-overview.md)

## <a name="prerequisites"></a>Ön koşullar

Önemli bir kasayı Azure Özel Bağlantısıile tümleştirmek için aşağıdakilere ihtiyacınız olacaktır:

- Anahtar kasası.
- Azure sanal ağı.
- Sanal ağdaki bir alt ağ.
- Hem anahtar kasası hem de sanal ağ için sahip veya katılımcı izinleri.

Özel bitiş noktanız ve sanal ağınız aynı bölgede olmalıdır. Portalı kullanarak özel bitiş noktası için bir bölge seçtiğinizde, otomatik olarak yalnızca o bölgedeki sanal ağları filtreler. Anahtar kasanız farklı bir bölgede olabilir.

Özel bitiş noktanız sanal abunuzda özel bir IP adresi kullanır.

## <a name="establish-a-private-link-connection-to-key-vault-using-the-azure-portal"></a>Azure portalını kullanarak Key Vault'a özel bağlantı bağlantısı kurma 

İlk olarak, [Azure portalını kullanarak sanal ağ oluşturma](../virtual-network/quick-create-portal.md) adımlarını izleyerek sanal ağ oluşturun

Daha sonra yeni bir anahtar kasası oluşturabilir veya varolan bir anahtar kasasına özel bir bağlantı bağlantısı kurabilirsiniz.

### <a name="create-a-new-key-vault-and-establish-a-private-link-connection"></a>Yeni bir anahtar kasası oluşturun ve özel bir bağlantı bağlantısı kurun

Set'teki adımları izleyerek yeni bir anahtar Vault oluşturabilir [ve Azure portalını kullanarak Azure Key Vault'tan bir sır alabilirsiniz](quick-create-portal.md)

Anahtar kasa temellerini yapılandırdıktan sonra Ağ sekmesini seçin ve aşağıdaki adımları izleyin:

1. Ağ sekmesinde Özel Bitiş Noktası radyo düğmesini seçin.
1. Özel bir bitiş noktası eklemek için "+ Ekle" Düğmesini tıklatın.

    ![Görüntü](./media/private-link-service-1.png)
 
1. Özel Bitiş Noktası Blade Oluştur'un "Konum" alanında, sanal ağınızın bulunduğu bölgeyi seçin. 
1. "Ad" alanında, bu özel bitiş noktasını tanımlamanıza olanak tanıyan açıklayıcı bir ad oluşturun. 
1. Açılır menüden bu özel bitiş noktasının oluşturulmasını istediğiniz sanal ağı ve alt ağı seçin. 
1. "Özel bölge DNS ile tümleştirme" seçeneğini değiştirmeden bırakın.  
1. "Tamam"ı seçin.

    ![Görüntü](./media/private-link-service-2.png)
 
Artık yapılandırılan özel bitiş noktasını görebilirsiniz. Artık bu özel bitiş noktasını silme ve düzeltme seçeneğiniz var. "Gözden Geçir + Oluştur" düğmesini seçin ve anahtar kasasını oluşturun. Dağıtımın tamamlanması 5-10 dakika sürer. 

### <a name="establish-a-private-link-connection-to-an-existing-key-vault"></a>Varolan bir anahtar kasasına özel bağlantı bağlantısı kurma

Zaten bir anahtar kasanız varsa, aşağıdaki adımları izleyerek özel bir bağlantı bağlantısı oluşturabilirsiniz:

1. Azure Portal’da oturum açın. 
1. Arama çubuğunda "anahtar kasaları" yazın
1. Özel bir bitiş noktası eklemek istediğiniz listeden anahtar kasasını seçin.
1. Ayarlar altında "Ağ" sekmesini seçin
1. Sayfanın üst kısmındaki Özel bitiş noktası bağlantıları sekmesini seçin
1. Sayfanın üst kısmındaki "+ Private Endpoint" düğmesini seçin.

    ![Resim](./media/private-link-service-3.png) ![Görüntüsü](./media/private-link-service-4.png)

Bu bıçağı kullanırken herhangi bir Azure kaynağı için özel bir bitiş noktası oluşturmayı seçebilirsiniz. Açılan menüleri bir kaynak türü seçmek ve dizininizde bir kaynak seçmek için kullanabilir veya kaynak kimliği kullanarak herhangi bir Azure kaynağına bağlanabilirsiniz. "Özel bölge DNS ile tümleştirme" seçeneğini değiştirmeden bırakın.  

![Resim](./media/private-link-service-3.png)
![Görüntüsü](./media/private-link-service-4.png)

## <a name="establish-a-private-link-connection-to-key-vault-using-cli"></a>CLI kullanarak Key Vault'a özel bağlantı bağlantısı kurma

### <a name="login-to-azure-cli"></a>Azure CLI'ye Giriş
```console
az login 
```
### <a name="select-your-azure-subscription"></a>Azure Aboneliğinizi seçin 
```console
az account set --subscription {AZURE SUBSCRIPTION ID}
```
### <a name="create-a-new-resource-group"></a>Yeni Kaynak Grubu oluşturma 
```console
az group create -n {RG} -l {AZURE REGION}
```
### <a name="register-microsoftkeyvault-as-a-provider"></a>Microsoft.KeyVault'u sağlayıcı olarak kaydetme 
```console
az provider register -n Microsoft.KeyVault
```
### <a name="create-a-new-key-vault"></a>Yeni bir Anahtar Kasası Oluşturma
```console
az keyvault create --name {KEY VAULT NAME} --resource-group {RG} --location {AZURE REGION}
```
### <a name="turn-on-key-vault-firewall"></a>Anahtar Kasagüvenlik DuvarLarını Aç
```console
az keyvault update --name {KEY VAULT NAME} --resource-group {RG} --location {AZURE REGION} --default-action deny
```
### <a name="create-a-virtual-network"></a>Sanal Ağ Oluşturma
```console
az network vnet create --resource-group {RG} --name {vNet NAME} --location {AZURE REGION}
```
### <a name="add-a-subnet"></a>Alt ağ ekleme
```console
az network vnet subnet create --resource-group {RG} --vnet-name {vNet NAME} --name {subnet NAME} --address-prefixes {addressPrefix}
```
### <a name="disable-virtual-network-policies"></a>Sanal Ağ İlkelerini Devre Dışı 
```console
az network vnet subnet update --name {subnet NAME} --resource-group {RG} --vnet-name {vNet NAME} --disable-private-endpoint-network-policies true
```
### <a name="add-a-private-dns-zone"></a>Özel DNS Bölgesi Ekleme 
```console
az network private-dns zone create --resource-group {RG} --name privatelink.vaultcore.azure.net
```
### <a name="link-private-dns-zone-to-virtual-network"></a>Özel DNS Bölgesini Sanal Ağa Bağla 
```console
az network private-dns link vnet create --resoruce-group {RG} --virtual-network {vNet NAME} --zone-name privatelink.vaultcore.azure.net --name {dnsZoneLinkName} --registration-enabled true
```
### <a name="create-a-private-endpoint-automatically-approve"></a>Özel Bitiş Noktası Oluşturma (Otomatik Olarak Onayla) 
```console
az network private-endpoint create --resource-group {RG} --vnet-name {vNet NAME} --subnet {subnet NAME} --name {Private Endpoint Name}  --private-connection-resource-id "/subscriptions/{AZURE SUBSCRIPTION ID}/resourceGroups/{RG}/providers/Microsoft.KeyVault/vaults/ {KEY VAULT NAME}" --group-ids vault --connection-name {Private Link Connection Name} --location {AZURE REGION}
```
### <a name="create-a-private-endpoint-manually-request-approval"></a>Özel Bitiş Noktası Oluşturma (El Ile Onay İste) 
```console
az network private-endpoint create --resource-group {RG} --vnet-name {vNet NAME} --subnet {subnet NAME} --name {Private Endpoint Name}  --private-connection-resource-id "/subscriptions/{AZURE SUBSCRIPTION ID}/resourceGroups/{RG}/providers/Microsoft.KeyVault/vaults/ {KEY VAULT NAME}" --group-ids vault --connection-name {Private Link Connection Name} --location {AZURE REGION} --manual-request
```
### <a name="show-connection-status"></a>Bağlantı Durumunu Göster 
```console
az network private-endpoint show --resource-group {RG} --name {Private Endpoint Name}
```
## <a name="manage-private-link-connection"></a>Özel bağlantı bağlantısını yönetme

Özel bir bitiş noktası oluşturduğunuzda, bağlantının onaylanması gerekir. Özel bir bitiş noktası oluşturduğunuz kaynak dizininizdeyse, yeterli izine sahip olmanız koşuluyla bağlantı isteğini onaylayabilirsiniz; Başka bir dizindeki bir Azure kaynağına bağlanıyorsanız, bağlantı isteğinizi bu kaynağın sahibinin onaylamasını beklemeniz gerekir.

Dört hüküm veren durum vardır:

| Hizmet eylem sağlar | Hizmet tüketici özel bitiş noktası durumu | Açıklama |
|--|--|--|
| None | Beklemede | Bağlantı el ile oluşturulur ve Private Link kaynak sahibinden onay bekliyor. |
| Onaylama | Onaylandı | Bağlantı otomatik olarak veya el ile onaylandı ve kullanıma hazır. |
| Reddet | Reddedilen | Bağlantı, özel bağlantı kaynağı sahibi tarafından reddedildi. |
| Kaldır | Bağlantı kesildi | Bağlantı özel bağlantı kaynak sahibi tarafından kaldırıldı, özel bitiş noktası bilgilendirici hale gelir ve temizleme için silinmelidir. |
 
###  <a name="how-to-manage-a-private-endpoint-connection-to-key-vault-using-the-azure-portal"></a>Azure portalını kullanarak Key Vault'a özel uç nokta bağlantısını yönetme 

1. Azure portalında oturum açın.
1. Arama çubuğunda "anahtar kasaları" yazın
1. Yönetmek istediğiniz anahtar kasasını seçin.
1. "Ağ" sekmesini seçin.
1. Bekleyen herhangi bir bağlantı varsa, sağlama durumunda "Beklemede" ile listelenen bir bağlantı görürsünüz. 
1. Onaylamak istediğiniz özel bitiş noktasını seçin
1. Onayla düğmesini seçin.
1. Reddetmek istediğiniz özel uç nokta bağlantıları varsa, bekleyen bir istek veya varolan bağlantı olsun, bağlantıyı seçin ve "Reddet" düğmesini tıklatın.

    ![Görüntü](./media/private-link-service-7.png)

##  <a name="how-to-manage-a-private-endpoint-connection-to-key-vault-using-azure-cli"></a>Azure CLI'yi kullanarak Key Vault'a özel uç nokta bağlantısını yönetme

### <a name="approve-a-private-link-connection-request"></a>Özel Bağlantı Bağlantı İsteğini Onaylama
```console
az keyvault private-endpoint-connection approve --approval-description {"OPTIONAL DESCRIPTION"} --resource-group {RG} --vault-name {KEY VAULT NAME} –name {PRIVATE LINK CONNECTION NAME}
```

### <a name="deny-a-private-link-connection-request"></a>Özel Bağlantı Bağlantı İsteğini Reddetme
```console
az keyvault private-endpoint-connection reject --rejection-description {"OPTIONAL DESCRIPTION"} --resource-group {RG} --vault-name {KEY VAULT NAME} –name {PRIVATE LINK CONNECTION NAME}
```

### <a name="delete-a-private-link-connection-request"></a>Özel Bağlantı Bağlantı İsteğini Silme
```console
az keyvault private-endpoint-connection delete --resource-group {RG} --vault-name {KEY VAULT NAME} --name {PRIVATE LINK CONNECTION NAME}
```

## <a name="validate-that-the-private-link-connection-works"></a>Özel bağlantı bağlantısının çalıştığını doğrulayın

Özel bitiş noktası kaynağının aynı alt netindeki kaynakların özel bir IP adresi üzerinden anahtar kasanıza bağladığını ve doğru özel DNS bölge tümleştirmesine sahip olduklarını doğrulamanız gerekir.

İlk olarak, [Azure portalında Windows sanal makine oluştur'daki](../virtual-machines/windows/quick-create-portal.md) adımları izleyerek sanal bir makine oluşturun

"Ağ" sekmesinde:

1. Sanal ağ ve Subnet belirtin. Yeni bir sanal ağ oluşturabilir veya varolan bir ağ seçebilirsiniz. Varolan bir tane seçiyorsanız, bölgenin eşleştiğinden emin olun.
1. Genel IP kaynağı belirtin.
1. "NIC ağ güvenlik grubunda" "Yok" seçeneğini belirleyin.
1. "Yük dengelemesi"nde "Hayır"ı seçin.

Komut satırını açın ve aşağıdaki komutu çalıştırın:

```console
nslookup <your-key-vault-name>.vault.azure.net
```

Bir anahtar kasasının IP adresini genel bitiş noktası üzerinden çözmek için ns arama komutunu çalıştırıyorsanız, aşağıdaki gibi görünen bir sonuç görürsünüz:

```console
c:\ >nslookup <your-key-vault-name>.vault.azure.net

Non-authoritative answer:
Name:    
Address:  (public IP address)
Aliases:  <your-key-vault-name>.vault.azure.net
```

Bir anahtar kasasının IP adresini özel bir bitiş noktası üzerinden çözmek için ns arama komutunu çalıştırArsanız, aşağıdaki gibi görünen bir sonuç görürsünüz:

```console
c:\ >nslookup your_vault_name.vault.azure.net

Non-authoritative answer:
Name:    
Address:  10.1.0.5 (private IP address)
Aliases:  <your-key-vault-name>.vault.azure.net
          <your-key-vault-name>.privatelink.vaultcore.azure.net
```

## <a name="limitations-and-design-considerations"></a>Sınırlamalar ve Tasarım Konuları

**Fiyatlandırma**: Fiyatlandırma bilgileri için [Azure Özel Bağlantı fiyatlandırması'na](https://azure.microsoft.com/pricing/details/private-link/)bakın.

**Sınırlamalar**: Azure Anahtar Kasası için Özel Bitiş Noktası yalnızca Azure ortak bölgelerinde kullanılabilir.

**Anahtar Kasabaşına Maksimum Özel Uç Noktası Sayısı**: 64.

**Abonelik Başına Özel Uç Noktaları olan Maksimum Anahtar Kasa Sayısı**: 64.

Daha fazla kullanım için Azure [Özel Bağlantı hizmeti: Sınırlamalar](../private-link/private-link-service-overview.md#limitations)

## <a name="next-steps"></a>Sonraki Adımlar

- [Azure Özel Bağlantı](../private-link/private-link-service-overview.md) hakkında daha fazla bilgi edinin
- [Azure Anahtar Kasası](key-vault-overview.md) hakkında daha fazla bilgi edinin
