---
title: Azure Dosyaları ile kullanılmak üzere Linux'ta Bir Noktadan Siteye (P2S) VPN yapılandırın | Microsoft Dokümanlar
description: Azure Dosyaları ile kullanılmak üzere Linux'ta Bir Sayfa-To-Site (P2S) VPN nasıl yapılandırılabilen
author: roygara
ms.service: storage
ms.topic: overview
ms.date: 10/19/2019
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: cfff05ed52258ee448d83a521b99dca7d356a0f9
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80061058"
---
# <a name="configure-a-point-to-site-p2s-vpn-on-linux-for-use-with-azure-files"></a>Azure Dosyaları ile kullanılmak üzere Linux'ta Bir Noktadan Siteye (P2S) VPN yapılandırın
445 bağlantı noktasını açmadan Azure dosya paylaşımlarınızı SMB üzerinden SMB'ye monte etmek için Bir Yerden Kullanıma (P2S) VPN bağlantısını kullanabilirsiniz. Bir Noktadan Siteye VPN bağlantısı, Azure ile tek bir istemci arasındaki VPN bağlantısıdır. Azure Dosyaları ile Bir P2S VPN bağlantısı kullanmak için, bağlanmak isteyen her istemci için bir P2S VPN bağlantısının yapılandırılması gerekir. Şirket içi ağınızdan Azure dosya paylaşımlarınıza bağlanması gereken çok sayıda istemciniz varsa, her istemci için Site'den Siteye (S2S) VPN bağlantısı yerine kullanabilirsiniz. Daha fazla bilgi için Bkz. [Azure Dosyaları ile kullanılmak üzere Siteden Siteye VPN Yapılandır'](storage-files-configure-s2s-vpn.md)a bakın.

Azure Dosyaları için kullanılabilir ağ seçeneklerinin tam olarak tartışılması için makaleye devam etmeden önce [Azure Dosyaları ağ genel görünümünü](storage-files-networking-overview.md) okumanızı öneririz.

Makalede, Azure dosya paylaşımlarını doğrudan şirket içinde monte etmek için Linux'ta Bir Siteye Nokta VPN'i yapılandırma adımları ayrıntılı olarak anlatılıyor. Azure Dosya Eşitleme trafiğini VPN üzerinden yönlendirmek istiyorsanız, lütfen [Azure Dosya Eşitleme proxy'sini ve güvenlik duvarı ayarlarını yapılandırmaya](storage-sync-files-firewall-and-proxy.md)bakın.

## <a name="prerequisites"></a>Ön koşullar
- Azure CLI'nin en son sürümü. Azure CLI'yi nasıl yükleyin hakkında daha fazla bilgi için Azure [PowerShell CLI'yi yükleyin](https://docs.microsoft.com/cli/azure/install-azure-cli) ve işletim sisteminizi seçin. Azure PowerShell modüllerini Linux'ta kullanmayı tercih ederseniz, aşağıdaki talimatlar Azure CLI için sunulabilir.

- Şirket içinde monte etmek istediğiniz bir Azure dosya paylaşımı. Azure dosya paylaşımları, birden çok dosya paylaşımının yanı sıra blob kapsayıcıları veya kuyruklar gibi diğer depolama kaynaklarını dağıtabileceğiniz paylaşılan bir depolama havuzunun yanı sıra yönetim yapıları olan depolama hesaplarında dağıtılır. Azure dosya paylaşımı oluştur'da Azure dosya paylaşımlarını ve depolama hesaplarını nasıl dağıtabileceğiniz hakkında daha fazla bilgi [edinebilirsiniz.](storage-how-to-create-file-share.md)

- Şirket içinde monte etmek istediğiniz Azure dosya paylaşımını içeren depolama hesabı için özel bir bitiş noktası. Özel bir bitiş noktası oluşturma hakkında daha fazla bilgi edinmek için Azure [Dosyaları ağ uç noktalarını yapılandırma](storage-files-networking-endpoints.md?tabs=azure-cli)konusuna bakın. 

## <a name="install-required-software"></a>Gerekli yazılımı yükleme
Azure sanal ağ ağ geçidi, IPsec ve OpenVPN dahil olmak üzere çeşitli VPN protokollerini kullanarak VPN bağlantıları sağlayabilir. Bu kılavuz, IPsec'in nasıl kullanılacağını gösterir ve Linux'ta destek sağlamak için strongSwan paketini kullanır. 

> Ubuntu 18.10 ile doğrulandı.

```bash
sudo apt install strongswan strongswan-pki libstrongswan-extra-plugins curl libxml2-utils cifs-utils

installDir="/etc/"
```

### <a name="deploy-a-virtual-network"></a>Sanal ağ dağıtma 
Azure dosya paylaşımınıza ve diğer Azure kaynaklarına şirket içi bir Giriş VPN üzerinden erişmek için bir sanal ağ veya VNet oluşturmanız gerekir. Otomatik olarak oluşturacağınız P2S VPN bağlantısı, şirket içi Linux makineniz ile bu Azure sanal ağı arasında bir köprüdür.

Aşağıdaki komut dosyası, biri depolama hesabınızın hizmet bitiş noktası, diğeri depolama hesabınızın özel oluşturmadan şirket içinde depolama hesabına erişmek için gereken özel bitiş noktası için olmak üzere üç alt ağa sahip bir Azure sanal ağı oluşturur. değişebilecek depolama hesabının genel IP'si ve VPN hizmetini sağlayan sanal ağ ağ ağ geçidiniz için yönlendirme. 

Çevreniz `<region>`için `<resource-group>`uygun `<desired-vnet-name>` değerleri değiştirmeyi ve değiştirmeyi unutmayın.

```bash
region="<region>"
resourceGroupName="<resource-group>"
virtualNetworkName="<desired-vnet-name>"

virtualNetwork=$(az network vnet create \
    --resource-group $resourceGroupName \
    --name $virtualNetworkName \
    --location $region \
    --address-prefixes "192.168.0.0/16" \
    --query "newVNet.id" | tr -d '"')

serviceEndpointSubnet=$(az network vnet subnet create \
    --resource-group $resourceGroupName \
    --vnet-name $virtualNetworkName \
    --name "ServiceEndpointSubnet" \
    --address-prefixes "192.168.0.0/24" \
    --service-endpoints "Microsoft.Storage" \
    --query "id" | tr -d '"')

privateEndpointSubnet=$(az network vnet subnet create \
    --resource-group $resourceGroupName \
    --vnet-name $virtualNetworkName \
    --name "PrivateEndpointSubnet" \
    --address-prefixes "192.168.1.0/24" \
    --query "id" | tr -d '"')

gatewaySubnet=$(az network vnet subnet create \
    --resource-group $resourceGroupName \
    --vnet-name $virtualNetworkName \
    --name "GatewaySubnet" \
    --address-prefixes "192.168.2.0/24" \
    --query "id" | tr -d '"')
```

## <a name="create-certificates-for-vpn-authentication"></a>VPN kimlik doğrulaması için sertifikalar oluşturma
Şirket içi Linux makinelerinizden gelen VPN bağlantılarının sanal ağınıza erişmek için kimlik doğrulaması yapabilmesi için iki sertifika oluşturmanız gerekir: sanal makine ağ geçidine sağlanacak bir kök sertifika ve müşteri sertifikası kök sertifikası ile imzalanır. Aşağıdaki komut dosyası gerekli sertifikaları oluşturur.

```bash
rootCertName="P2SRootCert"
username="client"
password="1234"

mkdir temp
cd temp

sudo ipsec pki --gen --outform pem > rootKey.pem
sudo ipsec pki --self --in rootKey.pem --dn "CN=$rootCertName" --ca --outform pem > rootCert.pem

rootCertificate=$(openssl x509 -in rootCert.pem -outform der | base64 -w0 ; echo)

sudo ipsec pki --gen --size 4096 --outform pem > "clientKey.pem"
sudo ipsec pki --pub --in "clientKey.pem" | \
    sudo ipsec pki \
        --issue \
        --cacert rootCert.pem \
        --cakey rootKey.pem \
        --dn "CN=$username" \
        --san $username \
        --flag clientAuth \
        --outform pem > "clientCert.pem"

openssl pkcs12 -in "clientCert.pem" -inkey "clientKey.pem" -certfile rootCert.pem -export -out "client.p12" -password "pass:$password"
```

## <a name="deploy-virtual-network-gateway"></a>Sanal ağ ağ geçidini dağıtma
Azure sanal ağ ağ ağ geçidi, şirket içi Linux makinelerinizin bağlanacağı hizmettir. Bu hizmeti dağıtmak iki temel bileşen gerektirir: müşterilerinizin dünyanın neresinde olurlarsa olsunlar için ağ geçidini tanımlayacak genel bir IP ve daha önce oluşturduğunuz ve müşterilerinizin kimliğini doğrulamak için kullanılacak bir kök sertifika.

Bu kaynaklar `<desired-vpn-name-here>` için istediğiniz adla değiştirmeyi unutmayın.

> [!Note]  
> Azure sanal ağ ağ ağ geçidini dağıtmak 45 dakika kadar sürebilir. Bu kaynak dağıtılırken, bu bash komut dosyası dağıtımının tamamlanmasını engeller. Bu beklenen bir durumdur.

```bash
vpnName="<desired-vpn-name-here>"
publicIpAddressName="$vpnName-PublicIP"

publicIpAddress=$(az network public-ip create \
    --resource-group $resourceGroupName \
    --name $publicIpAddressName \
    --location $region \
    --sku "Basic" \
    --allocation-method "Dynamic" \
    --query "publicIp.id" | tr -d '"')

az network vnet-gateway create \
    --resource-group $resourceGroupName \
    --name $vpnName \
    --vnet $virtualNetworkName \
    --public-ip-addresses $publicIpAddress \
    --location $region \
    --sku "VpnGw1" \
    --gateway-typ "Vpn" \
    --vpn-type "RouteBased" \
    --address-prefixes "172.16.201.0/24" \
    --client-protocol "IkeV2" > /dev/null

az network vnet-gateway root-cert create \
    --resource-group $resourceGroupName \
    --gateway-name $vpnName \
    --name $rootCertName \
    --public-cert-data $rootCertificate \
    --output none
```

## <a name="configure-the-vpn-client"></a>VPN istemcisini yapılandırma
Azure sanal ağ ağ geçidi, şirket içi Linux makinenizde VPN bağlantısını başlatması gereken yapılandırma dosyalarını içeren indirilebilir bir paket oluşturur. Aşağıdaki komut dosyası, oluşturduğunuz sertifikaları doğru noktaya yerleştirilecek `ipsec.conf` ve dosyayı indirilebilir paketteki yapılandırma dosyasından doğru değerlerle yapılandıracaktır.

```bash
vpnClient=$(az network vnet-gateway vpn-client generate \
    --resource-group $resourceGroupName \
    --name $vpnName \
    --authentication-method EAPTLS | tr -d '"')

curl $vpnClient --output vpnClient.zip
unzip vpnClient.zip

vpnServer=$(xmllint --xpath "string(/VpnProfile/VpnServer)" Generic/VpnSettings.xml)
vpnType=$(xmllint --xpath "string(/VpnProfile/VpnType)" Generic/VpnSettings.xml | tr '[:upper:]' '[:lower:]')
routes=$(xmllint --xpath "string(/VpnProfile/Routes)" Generic/VpnSettings.xml)

sudo cp "${installDir}ipsec.conf" "${installDir}ipsec.conf.backup"
sudo cp "Generic/VpnServerRoot.cer" "${installDir}ipsec.d/cacerts"
sudo cp "${username}.p12" "${installDir}ipsec.d/private" 

echo -e "\nconn $virtualNetworkName" | sudo tee -a "${installDir}ipsec.conf" > /dev/null
echo -e "\tkeyexchange=$vpnType" | sudo tee -a "${installDir}ipsec.conf" > /dev/null
echo -e "\ttype=tunnel" | sudo tee -a "${installDir}ipsec.conf" > /dev/null
echo -e "\tleftfirewall=yes" | sudo tee -a "${installDir}ipsec.conf" > /dev/null
echo -e "\tleft=%any" | sudo tee -a "${installDir}ipsec.conf" > /dev/null
echo -e "\tleftauth=eap-tls" | sudo tee -a "${installDir}ipsec.conf" > /dev/null
echo -e "\tleftid=%client" | sudo tee -a "${installDir}ipsec.conf" > /dev/null
echo -e "\tright=$vpnServer" | sudo tee -a "${installDir}ipsec.conf" > /dev/null
echo -e "\trightid=%$vpnServer" | sudo tee -a "${installDir}ipsec.conf" > /dev/null
echo -e "\trightsubnet=$routes" | sudo tee -a "${installDir}ipsec.conf" > /dev/null
echo -e "\tleftsourceip=%config" | sudo tee -a "${installDir}ipsec.conf" > /dev/null 
echo -e "\tauto=add" | sudo tee -a "${installDir}ipsec.conf" > /dev/null

echo ": P12 client.p12 '$password'" | sudo tee -a "${installDir}ipsec.secrets" > /dev/null

sudo ipsec restart
sudo ipsec up $virtualNetworkName 
```

## <a name="mount-azure-file-share"></a>Azure dosya paylaşımını başlat
Artık Sayfadan Siteye VPN'inizi ayarladığınızda, Azure dosya paylaşımınızı monte edebilirsiniz. Aşağıdaki örnek, payı ısrarsız olarak monte edecektir. Kalıcı olarak monte etmek için [bkz.](storage-how-to-use-files-linux.md) 

```bash
fileShareName="myshare"

mntPath="/mnt/$storageAccountName/$fileShareName"
sudo mkdir -p $mntPath

storageAccountKey=$(az storage account keys list \
    --resource-group $resourceGroupName \
    --account-name $storageAccountName \
    --query "[0].value" | tr -d '"')

smbPath="//$storageAccountPrivateIP/$fileShareName"
sudo mount -t cifs $smbPath $mntPath -o vers=3.0,username=$storageAccountName,password=$storageAccountKey,serverino
```

## <a name="see-also"></a>Ayrıca bkz.
- [Azure Dosyaları ağına genel bakış](storage-files-networking-overview.md)
- [Azure Dosyaları ile kullanılmak üzere Windows'da Bir Noktaya Sayfa (P2S) VPN'i yapılandırma](storage-files-configure-p2s-vpn-windows.md)
- [Azure Dosyaları ile kullanılmak üzere Siteden Siteye (S2S) VPN yapılandırın](storage-files-configure-s2s-vpn.md)