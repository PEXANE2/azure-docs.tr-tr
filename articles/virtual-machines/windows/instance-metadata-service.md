---
title: Windows için Azure Instance Metadata Service
description: Azure Instance Metadata Service hakkında bilgi edinin ve Windows 'ta Şu anda çalışan sanal makine örnekleri hakkında nasıl bilgi sağladığını öğrenin.
services: virtual-machines
author: KumariSupriya
manager: paulmey
ms.service: virtual-machines
ms.subservice: monitoring
ms.topic: how-to
ms.workload: infrastructure-services
ms.date: 03/30/2020
ms.author: sukumari
ms.reviewer: azmetadatadev
ms.openlocfilehash: cc0d47807101a3cfbb26e7ea69cc7d117d3f9b31
ms.sourcegitcommit: 5e5a0abe60803704cf8afd407784a1c9469e545f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2020
ms.locfileid: "96435238"
---
# <a name="azure-instance-metadata-service"></a>Azure Instance Metadata Service

Azure Instance Metadata Service (IMDS), çalışmakta olan sanal makine örnekleri hakkında bilgi sağlar. Bunu, sanal makinelerinizi yönetmek ve yapılandırmak için kullanabilirsiniz.
Bu bilgiler SKU, depolama, ağ yapılandırması ve yaklaşan bakım olaylarını içerir. Mevcut verilerin tüm listesi için bkz. [metadata API 'leri](#metadata-apis).


IDS, sanal makine (VM) ve sanal makine ölçek kümesi örneklerinin örneklerini çalıştırmak için kullanılabilir. Tüm API 'ler, [Azure Resource Manager](/rest/api/resources/)kullanılarak oluşturulan ve yönetilen VM 'leri destekler. Yalnızca atsınanan ve ağ uç noktaları, klasik dağıtım modeli kullanılarak oluşturulan VM 'Leri destekler. Atsınanan uç nokta, bunu yalnızca sınırlı bir ölçüde yapar.

IDS, iyi bilinen, yönlendirilemeyen bir IP adresinde () bulunan bir REST uç noktasıdır `169.254.169.254` . Yalnızca VM 'nin içinden erişebilirsiniz. VM ve ıDS arasındaki iletişim, Konağı hiçbir şekilde bırakmıyor.
ISE 'leri sorgularken, HTTP istemcilerinizi VM içindeki Web ara sunucusunu atlayıp `169.254.169.254` aynı şekilde değerlendirin [`168.63.129.16`](../../virtual-network/what-is-ip-address-168-63-129-16.md) .

## <a name="security"></a>Güvenlik

IMDS uç noktasına yalnızca, yönlendirilemeyen bir IP adresi üzerinde çalışan sanal makine örneği içinden erişilebilir. Ayrıca, üst bilgiyle bir istek `X-Forwarded-For` hizmet tarafından reddedilir.
`Metadata: true`Gerçek isteğin, istemeyerek yeniden yönlendirmenin bir parçası olmamasını sağlamak için istekler de bir üst bilgi içermelidir.

> [!IMPORTANT]
> IDS gizli veriler için bir kanal değildir. Uç nokta, sanal makine üzerindeki tüm işlemlere açıktır. Bu hizmet aracılığıyla, VM içinde çalışan tüm uygulamalara paylaşılan bilgiler olarak sunulan bilgileri göz önünde bulundurun.

## <a name="usage"></a>Kullanım

### <a name="access-azure-instance-metadata-service"></a>Azure Instance Metadata Service erişme

IMDS 'ye erişmek için [Azure Resource Manager](/rest/api/resources/) veya [Azure Portal](https://portal.azure.com)bir VM oluşturun ve aşağıdaki örnekleri kullanın.
Daha fazla örnek için bkz. [Azure örnek meta verileri örnekleri](https://github.com/microsoft/azureimds).

Örnek kod, örnek için tüm meta verileri almak için aşağıda verilmiştir. Belirli bir veri kaynağına erişmek için, [meta veri API 'si](#metadata-apis) bölümüne bakın. 

**İstek**

```powershell
Invoke-RestMethod -Headers @{"Metadata"="true"} -Method GET -NoProxy -Uri http://169.254.169.254/metadata/instance?api-version=2020-09-01 | ConvertTo-Json
```
> [!NOTE]
> `-NoProxy`Bayrak yalnızca PowerShell 6 veya sonraki sürümlerde kullanılabilir. Proxy Kurulumu yoksa bayrağını atlayabilirsiniz.

**Response**

> [!NOTE]
> Yanıt bir JSON dizesidir. Harika yazdırma için REST sorgunuzu cmdlet 'i aracılığıyla boru yapın `ConvertTo-Json` .

```json
{
    "compute": {
        "azEnvironment": "AZUREPUBLICCLOUD",
        "isHostCompatibilityLayerVm": "true",
        "licenseType":  "Windows_Client",
        "location": "westus",
        "name": "examplevmname",
        "offer": "Windows",
        "osProfile": {
            "adminUsername": "admin",
            "computerName": "examplevmname",
            "disablePasswordAuthentication": "true"
        },
        "osType": "linux",
        "placementGroupId": "f67c14ab-e92c-408c-ae2d-da15866ec79a",
        "plan": {
            "name": "planName",
            "product": "planProduct",
            "publisher": "planPublisher"
        },
        "platformFaultDomain": "36",
        "platformUpdateDomain": "42",
        "publicKeys": [{
                "keyData": "ssh-rsa 0",
                "path": "/home/user/.ssh/authorized_keys0"
            },
            {
                "keyData": "ssh-rsa 1",
                "path": "/home/user/.ssh/authorized_keys1"
            }
        ],
        "publisher": "RDFE-Test-Microsoft-Windows-Server-Group",
        "resourceGroupName": "macikgo-test-may-23",
        "resourceId": "/subscriptions/xxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx/resourceGroups/macikgo-test-may-23/providers/Microsoft.Compute/virtualMachines/examplevmname",
        "securityProfile": {
            "secureBootEnabled": "true",
            "virtualTpmEnabled": "false"
        },
        "sku": "Windows-Server-2012-R2-Datacenter",
        "storageProfile": {
            "dataDisks": [{
                "caching": "None",
                "createOption": "Empty",
                "diskSizeGB": "1024",
                "image": {
                    "uri": ""
                },
                "lun": "0",
                "managedDisk": {
                    "id": "/subscriptions/xxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx/resourceGroups/macikgo-test-may-23/providers/Microsoft.Compute/disks/exampledatadiskname",
                    "storageAccountType": "Standard_LRS"
                },
                "name": "exampledatadiskname",
                "vhd": {
                    "uri": ""
                },
                "writeAcceleratorEnabled": "false"
            }],
            "imageReference": {
                "id": "",
                "offer": "UbuntuServer",
                "publisher": "Canonical",
                "sku": "16.04.0-LTS",
                "version": "latest"
            },
            "osDisk": {
                "caching": "ReadWrite",
                "createOption": "FromImage",
                "diskSizeGB": "30",
                "diffDiskSettings": {
                    "option": "Local"
                },
                "encryptionSettings": {
                    "enabled": "false"
                },
                "image": {
                    "uri": ""
                },
                "managedDisk": {
                    "id": "/subscriptions/xxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx/resourceGroups/macikgo-test-may-23/providers/Microsoft.Compute/disks/exampleosdiskname",
                    "storageAccountType": "Standard_LRS"
                },
                "name": "exampleosdiskname",
                "osType": "Linux",
                "vhd": {
                    "uri": ""
                },
                "writeAcceleratorEnabled": "false"
            }
        },
        "subscriptionId": "xxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx",
        "tags": "baz:bash;foo:bar",
        "version": "15.05.22",
        "vmId": "02aab8a4-74ef-476e-8182-f6d2ba4166a6",
        "vmScaleSetName": "crpteste9vflji9",
        "vmSize": "Standard_A3",
        "zone": ""
    },
    "network": {
        "interface": [{
            "ipv4": {
               "ipAddress": [{
                    "privateIpAddress": "10.144.133.132",
                    "publicIpAddress": ""
                }],
                "subnet": [{
                    "address": "10.144.133.128",
                    "prefix": "26"
                }]
            },
            "ipv6": {
                "ipAddress": [
                 ]
            },
            "macAddress": "0011AAFFBB22"
        }]
    }
}
```

### <a name="data-output"></a>Veri çıkışı

Varsayılan olarak, ıDS verileri JSON biçiminde () döndürür `Content-Type: application/json` . Ancak, bazı API 'Ler isteniyorsa verileri farklı biçimlerde döndürebilir.
Aşağıdaki tabloda API 'Lerin destekleyebileceği diğer veri biçimleri listelenmektedir.

API | Varsayılan veri biçimi | Diğer biçimler
--------|---------------------|--------------
/attested | json | yok
/identity | json | yok
/Instance | json | metin
/scheduledevents | json | yok

Varsayılan olmayan bir yanıt biçimine erişmek için istenen biçimi istekte bir sorgu dizesi parametresi olarak belirtin. Örnek:

```powershell
Invoke-RestMethod -Headers @{"Metadata"="true"} -Method GET -NoProxy -Uri "http://169.254.169.254/metadata/instance?api-version=2017-08-01&format=text"
```

> [!NOTE]
> İçindeki yaprak düğümleri için `/metadata/instance` `format=json` çalışmaz. Varsayılan biçim JSON olduğundan, bu sorgular için `format=text` açıkça belirtilmesi gerekir.

### <a name="version"></a>Sürüm

IDS 'ler sürümlenmiş ve HTTP isteğindeki API sürümünün belirtilmesi zorunludur.

Desteklenen API sürümleri şunlardır: 
- 2017-03-01
- 2017-04-02
- 2017-08-01 
- 2017-10-01
- 2017-12-01 
- 2018-02-01
- 2018-04-02
- 2018-10-01
- 2019-02-01
- 2019-03-11
- 2019-04-30
- 2019-06-01
- 2019-06-04
- 2019-08-01
- 2019-08-15
- 2019-11-01
- 2020-06-01
- 2020-07-15
- 2020-09-01
- 2020-10-01

> [!NOTE]
> Sürüm 2020-10-01, her bölgede henüz kullanılamayabilir.

Daha yeni sürümler eklendikçe, betiklerinizin belirli veri biçimlerinde bağımlılıkları varsa uyumluluk için eski sürümlere erişmeye devam edebilirsiniz.

Bir sürüm belirtmezseniz, en yeni Desteklenen sürümlerin listesini içeren bir hata alırsınız.

> [!NOTE]
> Yanıt bir JSON dizesidir. Aşağıdaki örnek, sürüm belirtilmediğinde hata koşulunu gösterir. Yanıt, okunabilirlik için oldukça yazdırılır.

**İstek**

```powershell
Invoke-RestMethod -Headers @{"Metadata"="true"} -Method GET -NoProxy -Uri http://169.254.169.254/metadata/instance
```

**Response**

```json
{
    "error": "Bad request. api-version was not specified in the request. For more information refer to aka.ms/azureimds",
    "newest-versions": [
        "2020-10-01",
        "2020-09-01",
        "2020-07-15"
    ]
}
```

## <a name="metadata-apis"></a>Meta veri API 'Leri

IDS, farklı veri kaynaklarını temsil eden birden çok API içerir.

API | Açıklama | Sunulan sürüm
----|-------------|-----------------------
/attested | Bkz. [Atsınanan veriler](#attested-data) | 2018-10-01
/identity | Bkz. [erişim belirteci alma](../../active-directory/managed-identities-azure-resources/how-to-use-vm-token.md) | 2018-02-01
/Instance | Bkz. [örnek API 'si](#instance-api) | 2017-04-02
/scheduledevents | [Zamanlanan olaylara](scheduled-events.md) bakın | 2017-08-01

## <a name="instance-api"></a>Örnek API 'SI

Örnek API 'SI, sanal makine, ağ ve depolama dahil olmak üzere VM örnekleri için önemli meta verileri kullanıma sunar. Aşağıdaki kategorilere şu şekilde erişebilirsiniz `instance/compute` :

Veriler | Description | Sunulan sürüm
-----|-------------|-----------------------
azEnvironment | VM 'nin çalıştığı Azure ortamı. | 2018-10-01
customData | Bu özellik şu anda devre dışı. | 2019-02-01
isHostCompatibilityLayerVm | VM 'nin konak uyumluluk katmanında çalışıp çalışmadığına göre tanımlar. | 2020-06-01
licenseType | [Azure hibrit avantajı](https://azure.microsoft.com/pricing/hybrid-benefit)için lisans türü. Bunun yalnızca AHB özellikli VM 'Ler için mevcut olduğunu unutmayın. | 2020-09-01
location | VM 'nin çalıştığı Azure bölgesi. | 2017-04-02
name | VM 'nin adı. | 2017-04-02
teklif | VM görüntüsü için teklif bilgileri. Bu yalnızca Azure görüntü Galerisi 'nden dağıtılan görüntülerde bulunur. | 2017-04-02
osProfile.adminUsername | Yönetici hesabının adını belirtir. | 2020-07-15
osProfile. computerName | Bilgisayar adını belirtir. | 2020-07-15
osProfile. Disablepasswordaduthentication | Parola kimlik doğrulamasının devre dışı bırakılıp bırakılmadığını belirtir. Bunun yalnızca Linux sanal makineleri için mevcut olduğunu unutmayın. | 2020-10-01
osType | Linux veya Windows. | 2017-04-02
Placementgroupıd | Sanal makine ölçek kümesinin [yerleştirme grubu](../../virtual-machine-scale-sets/virtual-machine-scale-sets-placement-groups.md) . | 2017-08-01
plan | Bir Azure Market görüntüsü ise VM için ad, ürün ve yayımcı 'yı içeren [plan](/rest/api/compute/virtualmachines/createorupdate#plan) . | 2018-04-02
platformUpdateDomain |  VM 'nin çalıştığı [etki alanını güncelleştirin](../manage-availability.md) . | 2017-04-02
platformFaultDomain | VM 'nin çalıştığı [hata etki alanı](../manage-availability.md) . | 2017-04-02
sağlayıcısını | VM sağlayıcısı. | 2018-10-01
publicKeys | VM ve yollara atanan [ortak anahtarların koleksiyonu](/rest/api/compute/virtualmachines/createorupdate#sshpublickey) . | 2018-04-02
yayımcı | VM görüntüsünün yayımcısı. | 2017-04-02
resourceGroupName | SANAL makinenizin [kaynak grubu](../../azure-resource-manager/management/overview.md) . | 2017-08-01
resourceId | Kaynağın [tam](/rest/api/resources/resources/getbyid) kimliği. | 2019-03-11
isteyin | VM görüntüsü için özel SKU. | 2017-04-02
securityProfile. secureBootEnabled | SANAL makinede UEFı güvenli önyükleme özelliğinin etkinleştirilip etkinleştirilmediğini belirler. | 2020-06-01
securityProfile. Virtualaltpmenabled | Sanal Güvenilir Platform Modülü (TPM) VM 'de etkin olup olmadığını belirler. | 2020-06-01
storageProfile | Bkz. [depolama profili](#storage-metadata). | 2019-06-01
subscriptionId | VM için Azure aboneliği. | 2017-08-01
etiketler | SANAL makinenizin [etiketleri](../../azure-resource-manager/management/tag-resources.md) .  | 2017-08-01
tagsList | Daha kolay programlı ayrıştırma için JSON dizisi olarak biçimlendirilen Etiketler.  | 2019-06-04
sürüm | VM görüntüsünün sürümü. | 2017-04-02
Kimliği | VM için [benzersiz tanımlayıcı](https://azure.microsoft.com/blog/accessing-and-using-azure-vm-unique-id/) . | 2017-04-02
vmScaleSetName | Sanal makine ölçek kümesinin [sanal makine ölçek kümesi adı](../../virtual-machine-scale-sets/overview.md) . | 2017-12-01
vmSize | Bkz. [VM boyutu](../sizes.md). | 2017-04-02
bölge | VM 'nizin [kullanılabilirlik bölgesi](../../availability-zones/az-overview.md) . | 2017-12-01

### <a name="sample-1-track-a-vm-running-on-azure"></a>Örnek 1: Azure 'da çalışan bir VM 'yi Izleme

Hizmet sağlayıcı olarak, yazılımınızı çalıştıran VM 'lerin sayısını izlemeniz veya VM 'nin benzersizlik izlemesi gereken aracıların olması gerekebilir. Bir VM 'nin benzersiz KIMLIĞINI alabilmesi için, bu `vmId` alanı IMDS 'den kullanın.

**İstek**

```powershell
Invoke-RestMethod -Headers @{"Metadata"="true"} -Method GET -NoProxy -Uri "http://169.254.169.254/metadata/instance/compute/vmId?api-version=2017-08-01&format=text"
```

**Response**

```text
5c08b38e-4d57-4c23-ac45-aca61037f084
```

### <a name="sample-2-placement-of-different-data-replicas"></a>Örnek 2: farklı veri çoğaltmalarının yerleştirilmesi

Bazı senaryolarda, farklı veri çoğaltmalarının yerleştirilmesi önemli öneme sahiptir. Örneğin, bir [Orchestrator](https://kubernetes.io/docs/user-guide/node-selection/) aracılığıyla yapılan [çoğaltma yerleşimi](https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html#Replica_Placement:_The_First_Baby_Steps) veya KAPSAYıCı yerleştirmesi, `platformFaultDomain` ve `platformUpdateDomain` VM 'nin üzerinde çalışıp çalışmadığını bilmeniz gerekebilir.
Bu kararları vermek için örneklerin [kullanılabilirlik alanları](../../availability-zones/az-overview.md) de kullanabilirsiniz.
Bu verileri, doğrudan ıMDS aracılığıyla sorgulayabilirsiniz.

**İstek**

```powershell
Invoke-RestMethod -Headers @{"Metadata"="true"} -Method GET -NoProxy -Uri "http://169.254.169.254/metadata/instance/compute/platformFaultDomain?api-version=2017-08-01&format=text"
```

**Response**

```text
0
```

### <a name="sample-3-get-more-information-about-the-vm-during-support-case"></a>Örnek 3: destek talebi sırasında VM hakkında daha fazla bilgi alın

Hizmet sağlayıcı olarak, VM hakkında daha fazla bilgi edinmek istediğiniz bir destek çağrısı alabilirsiniz. Müşterinin işlem meta verilerini paylaşmasını isteme Bu durumda yararlı olabilir.

**İstek**

```powershell
Invoke-RestMethod -Headers @{"Metadata"="true"} -Method GET -NoProxy -Uri http://169.254.169.254/metadata/instance/compute?api-version=2020-09-01
```

**Response**

> [!NOTE]
> Yanıt bir JSON dizesidir. Aşağıdaki örnek yanıt, okunabilirlik için oldukça yazdırılır.

```json
{
    "azEnvironment": "AZUREPUBLICCLOUD",
    "isHostCompatibilityLayerVm": "true",
    "licenseType":  "Windows_Client",
    "location": "westus",
    "name": "examplevmname",
    "offer": "Windows",
    "osProfile": {
        "adminUsername": "admin",
        "computerName": "examplevmname",
        "disablePasswordAuthentication": "true"
    },
    "osType": "linux",
    "placementGroupId": "f67c14ab-e92c-408c-ae2d-da15866ec79a",
    "plan": {
        "name": "planName",
        "product": "planProduct",
        "publisher": "planPublisher"
    },
    "platformFaultDomain": "36",
    "platformUpdateDomain": "42",
    "publicKeys": [{
            "keyData": "ssh-rsa 0",
            "path": "/home/user/.ssh/authorized_keys0"
        },
        {
            "keyData": "ssh-rsa 1",
            "path": "/home/user/.ssh/authorized_keys1"
        }
    ],
    "publisher": "RDFE-Test-Microsoft-Windows-Server-Group",
    "resourceGroupName": "macikgo-test-may-23",
    "resourceId": "/subscriptions/xxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx/resourceGroups/macikgo-test-may-23/providers/Microsoft.Compute/virtualMachines/examplevmname",
    "securityProfile": {
        "secureBootEnabled": "true",
        "virtualTpmEnabled": "false"
    },
    "sku": "Windows-Server-2012-R2-Datacenter",
    "storageProfile": {
        "dataDisks": [{
            "caching": "None",
            "createOption": "Empty",
            "diskSizeGB": "1024",
            "image": {
                "uri": ""
            },
            "lun": "0",
            "managedDisk": {
                "id": "/subscriptions/xxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx/resourceGroups/macikgo-test-may-23/providers/Microsoft.Compute/disks/exampledatadiskname",
                "storageAccountType": "Standard_LRS"
            },
            "name": "exampledatadiskname",
            "vhd": {
                "uri": ""
            },
            "writeAcceleratorEnabled": "false"
        }],
        "imageReference": {
            "id": "",
            "offer": "UbuntuServer",
            "publisher": "Canonical",
            "sku": "16.04.0-LTS",
            "version": "latest"
        },
        "osDisk": {
            "caching": "ReadWrite",
            "createOption": "FromImage",
            "diskSizeGB": "30",
            "diffDiskSettings": {
                "option": "Local"
            },
            "encryptionSettings": {
                "enabled": "false"
            },
            "image": {
                "uri": ""
            },
            "managedDisk": {
                "id": "/subscriptions/xxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx/resourceGroups/macikgo-test-may-23/providers/Microsoft.Compute/disks/exampleosdiskname",
                "storageAccountType": "Standard_LRS"
            },
            "name": "exampleosdiskname",
            "osType": "Linux",
            "vhd": {
                "uri": ""
            },
            "writeAcceleratorEnabled": "false"
        }
    },
    "subscriptionId": "xxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx",
    "tags": "baz:bash;foo:bar",
    "version": "15.05.22",
    "vmId": "02aab8a4-74ef-476e-8182-f6d2ba4166a6",
    "vmScaleSetName": "crpteste9vflji9",
    "vmSize": "Standard_A3",
    "zone": ""
}
```

### <a name="sample-4-get-the-azure-environment-where-the-vm-is-running"></a>Örnek 4: VM 'nin çalıştığı Azure ortamını alın

Azure 'da [Azure Kamu](https://azure.microsoft.com/overview/clouds/government/)gibi çeşitli bağımsız bulutlar vardır. Bazen bazı çalışma zamanı kararları almak için Azure ortamına ihtiyacınız vardır. Aşağıdaki örnekte, bu davranışı nasıl sağlayabileceğiniz gösterilmektedir.

**İstek**

```powershell
Invoke-RestMethod -Headers @{"Metadata"="true"} -Method GET -NoProxy -Uri "http://169.254.169.254/metadata/instance/compute/azEnvironment?api-version=2018-10-01&format=text"
```

**Response**

```text
AzurePublicCloud
```

Azure ortamının bulutu ve değerleri burada listelenmiştir.

 Bulut   | Azure ortamı
---------|-----------------
[Tüm genel kullanıma açık Azure bölgeleri](https://azure.microsoft.com/regions/)     | AzurePublicCloud
[Azure Devlet Kurumları](https://azure.microsoft.com/overview/clouds/government/)              | AzureUSGovernmentCloud
[Azure China 21Vianet](https://azure.microsoft.com/global-infrastructure/china/)         | AzureChinaCloud
[Azure Almanya](https://azure.microsoft.com/overview/clouds/germany/)                    | AzureGermanCloud

## <a name="network-metadata"></a>Ağ meta verileri 

Ağ meta verileri, örnek API 'sinin bir parçasıdır. Aşağıdaki ağ kategorileri `instance/network` uç nokta üzerinden kullanılabilir.

Veriler | Description | Sunulan sürüm
-----|-------------|-----------------------
IPv4/Privateıpaddress | VM 'nin yerel IPv4 adresi. | 2017-04-02
IPv4/Publicıpaddress | VM 'nin genel IPv4 adresi. | 2017-04-02
alt ağ/adres | VM 'nin alt ağ adresi. | 2017-04-02
alt ağ/ön ek | Alt ağ ön eki. Örnek: 24 | 2017-04-02
IPv6/IPAddress | VM 'nin yerel IPv6 adresi. | 2017-04-02
macAddress | VM MAC adresi. | 2017-04-02

> [!NOTE]
> Tüm API yanıtları JSON dizeleridir. Aşağıdaki örnek yanıtların hepsi okunabilirlik için oldukça yazdırılır.

#### <a name="sample-1-retrieve-network-information"></a>Örnek 1: ağ bilgilerini alma

**İstek**

```powershell
Invoke-RestMethod -Headers @{"Metadata"="true"} -Method GET -NoProxy -Uri http://169.254.169.254/metadata/instance/network?api-version=2017-08-01
```

**Response**

```json
{
  "interface": [
    {
      "ipv4": {
        "ipAddress": [
          {
            "privateIpAddress": "10.1.0.4",
            "publicIpAddress": "X.X.X.X"
          }
        ],
        "subnet": [
          {
            "address": "10.1.0.0",
            "prefix": "24"
          }
        ]
      },
      "ipv6": {
        "ipAddress": []
      },
      "macAddress": "000D3AF806EC"
    }
  ]
}

```

#### <a name="sample-2-retrieve-public-ip-address"></a>Örnek 2: genel IP adresini alma

```powershell
Invoke-RestMethod -Headers @{"Metadata"="true"} -Method GET -NoProxy -Uri "http://169.254.169.254/metadata/instance/network/interface/0/ipv4/ipAddress/0/publicIpAddress?api-version=2017-08-01&format=text"
```

## <a name="storage-metadata"></a>Depolama meta verileri

Depolama meta verileri, uç nokta altında örnek API 'sinin bir parçasıdır `instance/compute/storageProfile` .
VM ile ilişkili depolama disklerinin ayrıntılarını sağlar. 

Bir sanal makinenin depolama profili üç kategoriye ayrılmıştır: görüntü başvurusu, işletim sistemi diski ve veri diskleri.

Görüntü başvurusu nesnesi, işletim sistemi görüntüsü hakkında aşağıdaki bilgileri içerir:

Veriler    | Description
--------|-----------------
kimlik      | Kaynak kimliği
teklif   | Platform veya görüntü teklifi
yayımcı | Görüntü yayımcısı
isteyin     | Görüntü SKU 'SU
sürüm | Platformun veya görüntünün sürümü

İşletim sistemi diski nesnesi, VM tarafından kullanılan işletim sistemi diski hakkında aşağıdaki bilgileri içerir:

Veriler    | Description
--------|-----------------
önbelleği | Önbelleğe alma gereksinimleri
createOption | VM 'nin nasıl oluşturulduğu hakkında bilgi
diffDiskSettings | Kısa ömürlü disk ayarları
diskSizeGB | Diskin GB cinsinden boyutu
encryptionSettings | Disk için şifreleme ayarları
image   | Kaynak Kullanıcı görüntüsü sanal sabit diski
managedDisk | Yönetilen disk parametreleri
name    | Disk adı
osType  | Diske dahil edilen işletim sisteminin türü
sahip     | Sanal sabit disk
Writeivatorenabled | `writeAccelerator`Disk üzerinde etkin olup olmadığını belirtir

Veri diskleri dizisi, VM 'ye bağlı veri disklerinin bir listesini içerir. Her veri diski nesnesi şu bilgileri içerir:

Veriler    | Description
--------|-----------------
önbelleği | Önbelleğe alma gereksinimleri
createOption | VM 'nin nasıl oluşturulduğu hakkında bilgi
diffDiskSettings | Kısa ömürlü disk ayarları
diskSizeGB | Diskin GB cinsinden boyutu
image   | Kaynak Kullanıcı görüntüsü sanal sabit diski
'yi     | Diskin mantıksal birim numarası
managedDisk | Yönetilen disk parametreleri
name    | Disk adı
sahip     | Sanal sabit disk
Writeivatorenabled | `writeAccelerator`Disk üzerinde etkin olup olmadığını belirtir

Aşağıdaki örnek, sanal makinenin depolama bilgilerinin nasıl sorgulanalınacağını gösterir.

**İstek**

```powershell
Invoke-RestMethod -Headers @{"Metadata"="true"} -Method GET -NoProxy -Uri http://169.254.169.254/metadata/instance/compute/storageProfile?api-version=2019-06-01
```

**Response**

> [!NOTE]
> Yanıt bir JSON dizesidir. Aşağıdaki örnek yanıt, okunabilirlik için oldukça yazdırılır.

```json
{
    "dataDisks": [
      {
        "caching": "None",
        "createOption": "Empty",
        "diskSizeGB": "1024",
        "image": {
          "uri": ""
        },
        "lun": "0",
        "managedDisk": {
          "id": "/subscriptions/xxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx/resourceGroups/macikgo-test-may-23/providers/Microsoft.Compute/disks/exampledatadiskname",
          "storageAccountType": "Standard_LRS"
        },
        "name": "exampledatadiskname",
        "vhd": {
          "uri": ""
        },
        "writeAcceleratorEnabled": "false"
      }
    ],
    "imageReference": {
      "id": "",
      "offer": "UbuntuServer",
      "publisher": "Canonical",
      "sku": "16.04.0-LTS",
      "version": "latest"
    },
    "osDisk": {
      "caching": "ReadWrite",
      "createOption": "FromImage",
      "diskSizeGB": "30",
      "diffDiskSettings": {
        "option": "Local"
      },
      "encryptionSettings": {
        "enabled": "false"
      },
      "image": {
        "uri": ""
      },
      "managedDisk": {
        "id": "/subscriptions/xxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx/resourceGroups/macikgo-test-may-23/providers/Microsoft.Compute/disks/exampleosdiskname",
        "storageAccountType": "Standard_LRS"
      },
      "name": "exampleosdiskname",
      "osType": "Linux",
      "vhd": {
        "uri": ""
      },
      "writeAcceleratorEnabled": "false"
    }
}
```

## <a name="vm-tags"></a>VM etiketleri

VM etiketleri, örnek API 'sini uç noktanın altına dahil edilir `instance/compute/tags` .
Etiketler, bunları bir taksonomi halinde mantıksal olarak düzenlemek için Azure sanal makinenize uygulanmış olabilir. Aşağıdaki isteği kullanarak, bir VM 'ye atanan etiketleri alabilirsiniz.

**İstek**

```powershell
Invoke-RestMethod -Headers @{"Metadata"="true"} -Method GET -NoProxy -Uri "http://169.254.169.254/metadata/instance/compute/tags?api-version=2018-10-01&format=text"
```

**Response**

```text
Department:IT;Environment:Test;Role:WebRole
```

`tags`Alan, etiketleri noktalı virgülle ayrılmış bir dizedir. Bu çıktı, etiketlerde noktalı virgül kullanılıyorsa bir sorun olabilir. Etiketleri programlı olarak ayıklamak için bir Ayrıştırıcı yazılmışsa, alanına güvenmelisiniz `tagsList` . `tagsList`Alan, sınırlandırıcı olmayan BIR JSON dizisidir ve bu nedenle daha kolay ayrıştırılabilir.

**İstek**

```powershell
Invoke-RestMethod -Headers @{"Metadata"="true"} -Method GET -NoProxy -Uri http://169.254.169.254/metadata/instance/compute/tagsList?api-version=2019-06-04
```

**Response**

```json
[
  {
    "name": "Department",
    "value": "IT"
  },
  {
    "name": "Environment",
    "value": "Test"
  },
  {
    "name": "Role",
    "value": "WebRole"
  }
]
```

## <a name="attested-data"></a>Atsınanan veriler

IDS, belirtilen verilerin Azure 'dan geldiği garantisi sağlamaya yardımcı olur. Microsoft bu bilgilerin bir parçasını imzalar, böylece Azure Market 'teki bir görüntünün Azure 'da çalıştırdığınız bir görüntü olduğunu doğrulayabilirsiniz.

### <a name="sample-1-get-attested-data"></a>Örnek 1: atsınanan verileri al

> [!NOTE]
> Tüm API yanıtları JSON dizeleridir. Aşağıdaki örnek yanıtlar okunabilirlik için oldukça yazdırılır.

**İstek**

```powershell
Invoke-RestMethod -Headers @{"Metadata"="true"} -Method GET -NoProxy -Uri "http://169.254.169.254/metadata/attested/document?api-version=2018-10-01&nonce=1234567890"
```

> [!NOTE]
> IDS 'nin önbelleğe alma mekanizması nedeniyle, önceden önbelleğe alınmış bir `nonce` değer döndürülebilir.

`Api-version` zorunlu bir alandır. Desteklenen API sürümleri için [kullanım bölümüne](#usage) bakın.
`Nonce` isteğe bağlı, 10 basamaklı bir dizedir. Sağlanmazsa, ıDS geçerli Eşgüdümlü Evrensel zaman zaman damgasını yerinde döndürür.

**Response**

```json
{
 "encoding":"pkcs7","signature":"MIIEEgYJKoZIhvcNAQcCoIIEAzCCA/8CAQExDzANBgkqhkiG9w0BAQsFADCBugYJKoZIhvcNAQcBoIGsBIGpeyJub25jZSI6IjEyMzQ1NjY3NjYiLCJwbGFuIjp7Im5hbWUiOiIiLCJwcm9kdWN0IjoiIiwicHVibGlzaGVyIjoiIn0sInRpbWVTdGFtcCI6eyJjcmVhdGVkT24iOiIxMS8yMC8xOCAyMjowNzozOSAtMDAwMCIsImV4cGlyZXNPbiI6IjExLzIwLzE4IDIyOjA4OjI0IC0wMDAwIn0sInZtSWQiOiIifaCCAj8wggI7MIIBpKADAgECAhBnxW5Kh8dslEBA0E2mIBJ0MA0GCSqGSIb3DQEBBAUAMCsxKTAnBgNVBAMTIHRlc3RzdWJkb21haW4ubWV0YWRhdGEuYXp1cmUuY29tMB4XDTE4MTEyMDIxNTc1N1oXDTE4MTIyMDIxNTc1NlowKzEpMCcGA1UEAxMgdGVzdHN1YmRvbWFpbi5tZXRhZGF0YS5henVyZS5jb20wgZ8wDQYJKoZIhvcNAQEBBQADgY0AMIGJAoGBAML/tBo86ENWPzmXZ0kPkX5dY5QZ150mA8lommszE71x2sCLonzv4/UWk4H+jMMWRRwIea2CuQ5RhdWAHvKq6if4okKNt66fxm+YTVz9z0CTfCLmLT+nsdfOAsG1xZppEapC0Cd9vD6NCKyE8aYI1pliaeOnFjG0WvMY04uWz2MdAgMBAAGjYDBeMFwGA1UdAQRVMFOAENnYkHLa04Ut4Mpt7TkJFfyhLTArMSkwJwYDVQQDEyB0ZXN0c3ViZG9tYWluLm1ldGFkYXRhLmF6dXJlLmNvbYIQZ8VuSofHbJRAQNBNpiASdDANBgkqhkiG9w0BAQQFAAOBgQCLSM6aX5Bs1KHCJp4VQtxZPzXF71rVKCocHy3N9PTJQ9Fpnd+bYw2vSpQHg/AiG82WuDFpPReJvr7Pa938mZqW9HUOGjQKK2FYDTg6fXD8pkPdyghlX5boGWAMMrf7bFkup+lsT+n2tRw2wbNknO1tQ0wICtqy2VqzWwLi45RBwTGB6DCB5QIBATA/MCsxKTAnBgNVBAMTIHRlc3RzdWJkb21haW4ubWV0YWRhdGEuYXp1cmUuY29tAhBnxW5Kh8dslEBA0E2mIBJ0MA0GCSqGSIb3DQEBCwUAMA0GCSqGSIb3DQEBAQUABIGAld1BM/yYIqqv8SDE4kjQo3Ul/IKAVR8ETKcve5BAdGSNkTUooUGVniTXeuvDj5NkmazOaKZp9fEtByqqPOyw/nlXaZgOO44HDGiPUJ90xVYmfeK6p9RpJBu6kiKhnnYTelUk5u75phe5ZbMZfBhuPhXmYAdjc7Nmw97nx8NnprQ="
}
```

İmza blobu, belgenin [PKCS7](https://aka.ms/pkcs7)imzalı bir sürümüdür. İmzalama için kullanılan sertifikayı, VM 'ye özgü belirli ayrıntılarla birlikte içerir. 

Azure Resource Manager kullanılarak oluşturulan VM 'ler için, bu `vmId` `sku` `nonce` `subscriptionId` `timeStamp` belgenin oluşturulması ve sona erme tarihi ve görüntüyle ilgili plan bilgileri de dahildir. Plan bilgileri yalnızca Azure Market görüntüleri için doldurulur. 

Klasik dağıtım modeli kullanılarak oluşturulan VM 'Ler için yalnızca `vmId` doldurulacak garanti edilir. Yanıttan sertifikayı ayıklayabilir ve bu yanıtı kullanarak yanıtın geçerli olduğunu ve Azure 'dan geldiğini doğrulayabilirsiniz.

Belge aşağıdaki alanları içerir:

Veriler | Description | Sunulan sürüm
-----|-------------|-----------------------
licenseType | [Azure hibrit avantajı](https://azure.microsoft.com/pricing/hybrid-benefit)için lisans türü. Bunun yalnızca AHB özellikli VM 'Ler için mevcut olduğunu unutmayın. | 2020-09-01
nonce | İsteğe bağlı olarak istekle birlikte sağlanmış bir dize. Hayır `nonce` sağlanmazsa, geçerli Eşgüdümlü Evrensel Saat zaman damgası kullanılır. | 2018-10-01
plan | [Azure Marketi görüntü planı](/rest/api/compute/virtualmachines/createorupdate#plan). Plan KIMLIĞINI (adı), ürün görüntüsünü veya teklifi (ürün) ve yayımcı KIMLIĞINI (yayımcı) içerir. | 2018-10-01
zaman damgası/createdOn | İmzalanmış belgenin oluşturulduğu zamana yönelik Eşgüdümlü Evrensel zaman damgası. | 2018-20-01
zaman damgası/expiresOn | İmzalanmış belgenin süresi dolduğu zamana yönelik Eşgüdümlü Evrensel zaman damgası. | 2018-10-01
Kimliği |  VM için [benzersiz tanımlayıcı](https://azure.microsoft.com/blog/accessing-and-using-azure-vm-unique-id/) . | 2018-10-01
subscriptionId | VM için Azure aboneliği. | 2019-04-30
isteyin | VM görüntüsü için özel SKU. | 2019-11-01

### <a name="sample-2-validate-that-the-vm-is-running-in-azure"></a>Örnek 2: VM 'nin Azure 'da çalıştığını doğrulama

Azure Marketi 'ndeki satıcılar, yazılımlarının yalnızca Azure 'da çalışmak üzere lisanslanmasını sağlamak istiyor. Birisi VHD 'yi şirket içi bir ortama kopyarsa, satıcının bunu algılayabilmesi gerekir. IMDS aracılığıyla, bu satıcılar yalnızca Azure 'dan gelen yanıtı garanti eden imzalı verileri alabilir.

```powershell
# Get the signature
$attestedDoc = Invoke-RestMethod -Headers @{"Metadata"="true"} -Method GET -NoProxy -Uri http://169.254.169.254/metadata/attested/document?api-version=2020-09-01
# Decode the signature
$signature = [System.Convert]::FromBase64String($attestedDoc.signature)
```

İmzanın Microsoft Azure olduğunu doğrulayın ve hata için sertifika zincirini denetleyin.

```powershell
# Get certificate chain
$cert = [System.Security.Cryptography.X509Certificates.X509Certificate2]($signature)
$chain = New-Object -TypeName System.Security.Cryptography.X509Certificates.X509Chain
$chain.Build($cert)
# Print the Subject of each certificate in the chain
foreach($element in $chain.ChainElements)
{
    Write-Host $element.Certificate.Subject
}

# Get the content of the signed document
Add-Type -AssemblyName System.Security
$signedCms = New-Object -TypeName System.Security.Cryptography.Pkcs.SignedCms
$signedCms.Decode($signature);
$content = [System.Text.Encoding]::UTF8.GetString($signedCms.ContentInfo.Content)
Write-Host "Attested data: " $content
$json = $content | ConvertFrom-Json
# Do additional validation here
```

> [!NOTE]
> IDS 'nin önbelleğe alma mekanizması nedeniyle, önceden önbelleğe alınmış bir `nonce` değer döndürülebilir.

`nonce`Başlangıç isteğinde bir parametre sağladıysanız, imzalı belgedeki ' de karşılaştırılabilir `nonce` .

> [!NOTE]
> Genel bulut ve her bir sogeign bulutu için sertifika farklı olacaktır.

Bulut | Sertifika
------|------------
[Tüm genel kullanıma açık Azure bölgeleri](https://azure.microsoft.com/regions/) | *. metadata.azure.com
[Azure Devlet Kurumları](https://azure.microsoft.com/overview/clouds/government/)          | *. metadata.azure.us
[Azure China 21Vianet](https://azure.microsoft.com/global-infrastructure/china/)     | *. metadata.azure.cn
[Azure Almanya](https://azure.microsoft.com/overview/clouds/germany/)                | *. metadata.microsoftazure.de

> [!NOTE]
> Sertifikalar, genel bulut için tam olarak eşleşen bir eşleşmeyebilir `metadata.azure.com` . Bu nedenle, sertifika doğrulaması herhangi bir alt etki alanından ortak bir ada izin verilmelidir `.metadata.azure.com` .

Doğrulama sırasında ağ kısıtlamalarına bağlı olarak ara sertifikanın indirilebileceği durumlarda ara sertifikayı sabitleyebilir. Azure 'un standart PKI uygulaması olan sertifikaların üzerine kaydolur. Geçiş gerçekleştiğinde sabitlenmiş sertifikaları güncelleştirmeniz gerekir. Ara sertifikayı güncelleştirme değişikliği planlandığı zaman, Azure blogu güncelleştirilir ve Azure müşterileri bilgilendirilir. 

Ara sertifikaları [PKI deposunda](https://www.microsoft.com/pki/mscorp/cps/default.htm)bulabilirsiniz. Her bir bölgenin ara sertifikaları farklı olabilir.

> [!NOTE]
> Azure Çin 21Vianet için ara sertifika, her Baldaha fazla değil, DigiCert küresel kök CA 'dan oluşur.
Kök zincir yetkilisinin bir parçası olarak Azure Çin ara sertifikalarını sabitlediğiniz takdirde, ara sertifikaların güncellenmesi gerekir.

## <a name="failover-clustering-in-windows-server"></a>Windows Server 'da Yük Devretme Kümelemesi

Yük Devretme Kümelemesi ile ıMDS 'yi sorgularken, bazen yönlendirme tablosuna bir rota eklenmesi gerekir. Aşağıdaki adımları uygulayın:

1. Yönetici ayrıcalıklarına sahip bir komut istemi açın.

1. Aşağıdaki komutu çalıştırın ve IPv4 yol tablosundaki ağ hedefi () arabiriminin adresini göz önünde edin `0.0.0.0` .

```bat
route print
```

> [!NOTE]
> Aşağıdaki örnek çıktı, yük devretme kümesi etkin bir Windows Server VM 'sinden yapılır. Kolaylık olması için, çıkış yalnızca IPv4 yol tablosunu içerir.

```text
IPv4 Route Table
===========================================================================
Active Routes:
Network Destination        Netmask          Gateway       Interface  Metric
          0.0.0.0          0.0.0.0         10.0.1.1        10.0.1.10    266
         10.0.1.0  255.255.255.192         On-link         10.0.1.10    266
        10.0.1.10  255.255.255.255         On-link         10.0.1.10    266
        10.0.1.15  255.255.255.255         On-link         10.0.1.10    266
        10.0.1.63  255.255.255.255         On-link         10.0.1.10    266
        127.0.0.0        255.0.0.0         On-link         127.0.0.1    331
        127.0.0.1  255.255.255.255         On-link         127.0.0.1    331
  127.255.255.255  255.255.255.255         On-link         127.0.0.1    331
      169.254.0.0      255.255.0.0         On-link     169.254.1.156    271
    169.254.1.156  255.255.255.255         On-link     169.254.1.156    271
  169.254.255.255  255.255.255.255         On-link     169.254.1.156    271
        224.0.0.0        240.0.0.0         On-link         127.0.0.1    331
        224.0.0.0        240.0.0.0         On-link     169.254.1.156    271
  255.255.255.255  255.255.255.255         On-link         127.0.0.1    331
  255.255.255.255  255.255.255.255         On-link     169.254.1.156    271
  255.255.255.255  255.255.255.255         On-link         10.0.1.10    266
```

Aşağıdaki komutu çalıştırın ve `0.0.0.0` Bu örnekte () olan ağ hedefi () Için arabirimin adresini kullanın `10.0.1.10` .

```bat
route add 169.254.169.254/32 10.0.1.10 metric 1 -p
```

## <a name="managed-identity"></a>Yönetilen kimlik

Sistem tarafından atanan yönetilen bir kimlik, sanal makinede etkinleştirilebilir. VM 'ye bir veya daha fazla kullanıcı tarafından atanan yönetilen kimlik de atayabilirsiniz.
Ardından, ıMDS 'den Yönetilen kimlikler için belirteçler isteyebilirsiniz. Azure Key Vault gibi diğer Azure hizmetleriyle kimlik doğrulaması yapmak için bu belirteçleri kullanın.

Bu özelliği etkinleştirmeye yönelik ayrıntılı adımlar için bkz. [erişim belirteci alma](../../active-directory/managed-identities-azure-resources/how-to-use-vm-token.md).

## <a name="scheduled-events"></a>Zamanlanmış olaylar
Zamanlanan olayların durumunu ıMDS kullanarak elde edebilirsiniz. Ardından Kullanıcı, bu olaylar üzerinde çalıştırılacak bir eylemler kümesi belirtebilir. Daha fazla bilgi için bkz. [Zamanlanmış olaylar](scheduled-events.md). 

## <a name="regional-availability"></a>Bölgesel kullanılabilirlik

Hizmet, tüm Azure bulutlarında genel kullanıma sunulmuştur.

## <a name="sample-code-in-different-languages"></a>Farklı dillerdeki örnek kod

Aşağıdaki tabloda, sanal makine içinde farklı diller kullanılarak ıMDS çağırma örnekleri listelenmektedir:

Dil      | Örnek
--------------|----------------
C++ (Windows) | https://github.com/Microsoft/azureimds/blob/master/IMDSSample-windows.cpp
C#            | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.cs
Başlayın            | https://github.com/Microsoft/azureimds/blob/master/imdssample.go
Java          | https://github.com/Microsoft/azureimds/blob/master/imdssample.java
NodeJS        | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.js
Perl          | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.pl
PowerShell    | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.ps1
Puppet        | https://github.com/keirans/azuremetadata
Python        | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.py
Ruby          | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.rb
Visual Basic  | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.vb

## <a name="errors-and-debugging"></a>Hatalar ve hata ayıklama

Veri öğesi bulunamadı veya hatalı oluşturulmuş bir istek varsa, ıDS standart HTTP hataları döndürür. Örnek:

HTTP durum kodu | Nedeni
-----------------|-------
200 TAMAM |
400 Hatalı İstek | `Metadata: true`Yaprak düğüm sorgulanırken eksik başlık veya eksik parametre `format=json` .
404 Bulunamadı  | İstenen öğe yok.
405 yöntemine Izin verilmiyor | Yalnızca `GET` istekler desteklenir.
410 gitti | En fazla 70 saniye boyunca bir süre sonra yeniden deneyin.
429 Çok Fazla İstek Var | API Şu anda saniyede en fazla 5 sorgu desteklemektedir.
500 hizmet hatası     | Bir süre sonra yeniden deneyin.

### <a name="frequently-asked-questions"></a>Sık sorulan sorular

**Hatayı alıyorum `400 Bad Request, Required metadata header not specified` . Bu ne anlama geliyor?**

IDS, üstbilginin `Metadata: true` istek içine geçirilmesini gerektirir. Bu üstbilgiyi REST çağrısına geçirmek, ıMDS 'ye erişim sağlar.

**Neden Sanal makinem için işlem bilgilerini alamıyorum?**

Şu anda, ıDS yalnızca Azure Resource Manager oluşturulan örnekleri destekler.

**Azure Resource Manager bir süre önce VM 'imi oluşturdum. İşlem meta veri bilgilerini neden göremiyorum?**

VM 'nizi 2016 Eylül 'den sonra oluşturduysanız, işlem meta verilerini görmeye başlamak için bir [etiket](../../azure-resource-manager/management/tag-resources.md) ekleyin. VM 'nizi 2016 Eylül 'den önce oluşturduysanız meta verileri yenilemek için VM örneğine uzantıları veya veri diskleri ekleyin veya kaldırın.

**Neden yeni bir sürüm için doldurulmuş tüm verileri görmüyorum?**

VM 'nizi 2016 Eylül 'den sonra oluşturduysanız, işlem meta verilerini görmeye başlamak için bir [etiket](../../azure-resource-manager/management/tag-resources.md) ekleyin. VM 'nizi 2016 Eylül 'den önce oluşturduysanız meta verileri yenilemek için VM örneğine uzantıları veya veri diskleri ekleyin veya kaldırın.

**Hatayı neden alıyorum `500 Internal Server Error` `410 Resource Gone` ?**

İsteğinizi yeniden deneyin. Daha fazla bilgi için bkz. [geçici hata işleme](/azure/architecture/best-practices/transient-faults). Sorun devam ederse, VM için Azure portal bir destek sorunu oluşturun.

**Bu, sanal makine ölçek kümesi örnekleri için mi çalışıyor?**

Evet, sanal makine ölçek kümesi örnekleri için ıMDS kullanılabilir.

**Sanal makine ölçek kümelerinde etiketlerimi güncelleştirdim, ancak örneklerde gözükmez (tek örnekli VM 'lerden farklı olarak). Sorun mu yaşıyorsunuz?**

Şu anda sanal makine ölçek kümeleri için olan Etiketler yalnızca bir yeniden başlatma, yeniden görüntü veya örneğe disk değişikliği üzerinde VM 'yi gösterir.

**İstek, hizmete çağrımda neden zaman aşımına uğradı?**

Meta veri çağrılarının, VM 'nin birincil ağ kartına atanan birincil IP adresinden yapılması gerekir. Ayrıca, rotalarınızı değiştirdiyseniz, sanal makinenizin yerel yönlendirme tablosunda 169.254.169.254/32 adresi için bir yol olmalıdır.
   * <details>
        <summary>Yönlendirme tablonuz doğrulanıyor</summary>

        1. Yerel yönlendirme tablonuzu dökümünü alın ve ıMDS girişini bulun. Örnek:
            ```console
            > route print
            IPv4 Route Table
            ===========================================================================
            Active Routes:
            Network Destination        Netmask          Gateway       Interface  Metric
                      0.0.0.0          0.0.0.0      172.16.69.1      172.16.69.7     10
                    127.0.0.0        255.0.0.0         On-link         127.0.0.1    331
                    127.0.0.1  255.255.255.255         On-link         127.0.0.1    331
              127.255.255.255  255.255.255.255         On-link         127.0.0.1    331
                168.63.129.16  255.255.255.255      172.16.69.1      172.16.69.7     11
              169.254.169.254  255.255.255.255      172.16.69.1      172.16.69.7     11
            ... (continues) ...
            ```
        1. İçin bir yolun mevcut olduğunu doğrulayın `169.254.169.254` ve ilgili ağ arabirimini (örneğin, `172.16.69.7` ) unutmayın.
        1. Arabirim yapılandırmasını dökümünü alın ve yönlendirme tablosunda başvurulan öğesine karşılık gelen arabirimi MAC (fiziksel) adresini belirterek bulun.
            ```console
            > ipconfig /all
            ... (continues) ...
            Ethernet adapter Ethernet:

               Connection-specific DNS Suffix  . : xic3mnxjiefupcwr1mcs1rjiqa.cx.internal.cloudapp.net
               Description . . . . . . . . . . . : Microsoft Hyper-V Network Adapter
               Physical Address. . . . . . . . . : 00-0D-3A-E5-1C-C0
               DHCP Enabled. . . . . . . . . . . : Yes
               Autoconfiguration Enabled . . . . : Yes
               Link-local IPv6 Address . . . . . : fe80::3166:ce5a:2bd5:a6d1%3(Preferred)
               IPv4 Address. . . . . . . . . . . : 172.16.69.7(Preferred)
               Subnet Mask . . . . . . . . . . . : 255.255.255.0
            ... (continues) ...
            ```
        1. Arabirimin VM 'nin birincil NIC 'sine ve birincil IP 'ye karşılık geldiğini doğrulayın. Azure portal ağ yapılandırmasına bakarak veya Azure CLı ile arayarak birincil NIC ve IP 'yi bulabilirsiniz. Ortak ve özel IP 'Leri (CLı kullanıyorsanız MAC adresini) göz önünde bulabilirsiniz. PowerShell CLı örneği aşağıda verilmiştir:
            ```powershell
            $ResourceGroup = '<Resource_Group>'
            $VmName = '<VM_Name>'
            $NicNames = az vm nic list --resource-group $ResourceGroup --vm-name $VmName | ConvertFrom-Json | Foreach-Object { $_.id.Split('/')[-1] }
            foreach($NicName in $NicNames)
            {
                $Nic = az vm nic show --resource-group $ResourceGroup --vm-name $VmName --nic $NicName | ConvertFrom-Json
                Write-Host $NicName, $Nic.primary, $Nic.macAddress
            }
            # Output: wintest767 True 00-0D-3A-E5-1C-C0
            ```
        1. Eşleşiyorlarsa, birincil NIC ve IP 'nin hedefleneceği şekilde yönlendirme tablosunu güncelleştirin.
    </details>

## <a name="support"></a>Destek

Birden çok denemeden sonra meta veri yanıtı alamazsanız Azure portal bir destek sorunu oluşturabilirsiniz.
**Sorun türü** için **Yönetim**' i seçin. **Kategori** için **Instance Metadata Service**' yi seçin.

![Instance Metadata Service desteğinin ekran görüntüsü](./media/instance-metadata-service/InstanceMetadata-support.png)

## <a name="next-steps"></a>Sonraki adımlar

[VM için bir erişim belirteci alma](../../active-directory/managed-identities-azure-resources/how-to-use-vm-token.md)

[Zamanlanmış olaylar](scheduled-events.md)
