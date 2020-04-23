---
title: Azure Örneği Meta veri Hizmeti
description: VM'lerin bilgi işlem, ağ ve yaklaşan bakım olayları hakkında bilgi almak için yeniden hesaplı arabirim.
author: KumariSupriya
manager: paulmey
ms.service: virtual-machines
ms.subservice: monitoring
ms.topic: how-to
ms.workload: infrastructure-services
ms.date: 03/30/2020
ms.author: sukumari
ms.reviewer: azmetadata
ms.openlocfilehash: cb9453e1a25f4042c45d4e89229b555c996d4c8b
ms.sourcegitcommit: af1cbaaa4f0faa53f91fbde4d6009ffb7662f7eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81870076"
---
# <a name="azure-instance-metadata-service"></a>Azure Örneği Meta veri hizmeti

Azure Örneği Meta Veri Hizmeti (IMDS), şu anda çalışan sanal makine örnekleri hakkında bilgi sağlar ve sanal makinelerinizi yönetmek ve yapılandırmak için kullanılabilir.
Sağlanan bilgiler SKU, ağ yapılandırması ve yaklaşan bakım olaylarını içerir. Kullanılabilir verilerin tam listesi için meta [veri API'leri'ne](#metadata-apis)bakın.

Azure Örneği Meta veri hizmeti, [Azure Kaynak Yöneticisi](https://docs.microsoft.com/rest/api/resources/)aracılığıyla oluşturulan tüm IaaS VM'ler tarafından erişilebilen bir REST Bitiş Noktasıdır.
Bitiş noktası, yalnızca VM içinden erişilebilen,`169.254.169.254`iyi bilinen, routable olmayan bir IP adresinde kullanılabilir.

> [!IMPORTANT]
> Bu hizmet genellikle tüm Azure Bölgelerinde **kullanılabilir.**  Sanal makine örnekleri hakkında yeni bilgiler ortaya çıkarmak için düzenli olarak güncelleştirmeler alır. Bu sayfa, mevcut güncel [meta veri API'lerini](#metadata-apis) yansıtır.

## <a name="service-availability"></a>Hizmet kullanılabilirliği

Hizmet, genel olarak kullanılabilir Azure bölgelerinde kullanılabilir. Tüm API sürümü tüm Azure Bölgelerinde kullanılamayabilir.

Bölgeler                                        | Kullanılabilir -lik?                                 | Desteklenen Sürümler
-----------------------------------------------|-----------------------------------------------|-----------------
[Tüm Genel Olarak Kullanılabilen Küresel Azure Bölgeleri](https://azure.microsoft.com/regions/)     | Genel Kullanıma Sunulmuştur | 2017-04-02, 2017-08-01, 2017-12-01, 2018-02-01, 2018-04-02, 2018-10-01, 2019-02-01, 2019-03-11, 2019-04-30, 2019-06-01, 2019-06-04, 2019-08-01, 2019-08-15
[Azure Devlet Kurumları](https://azure.microsoft.com/overview/clouds/government/)              | Genel Kullanıma Sunulmuştur | 2017-04-02, 2017-08-01, 2017-12-01, 2018-02-01, 2018-04-02, 2018-10-01, 2019-02-01, 2019-03-11, 2019-04-30, 2019-06-01, 2019-06-04, 2019-08-01, 2019-08-15
[Azure Çin 21Vianet](https://www.azure.cn/)                                            | Genel Kullanıma Sunulmuştur | 2017-04-02, 2017-08-01, 2017-12-01, 2018-02-01, 2018-04-02, 2018-10-01, 2019-02-01, 2019-03-11, 2019-04-30, 2019-06-01, 2019-06-04, 2019-08-01, 2019-08-15
[Azure Almanya](https://azure.microsoft.com/overview/clouds/germany/)                    | Genel Kullanıma Sunulmuştur | 2017-04-02, 2017-08-01, 2017-12-01, 2018-02-01, 2018-04-02, 2018-10-01, 2019-02-01, 2019-03-11, 2019-04-30, 2019-06-01, 2019-06-04, 2019-08-01, 2019-08-15

Sürüm 2019-11-01 şu anda dağıtılmaktadır ve tüm bölgelerde kullanılamayabilir.

Bu tablo, hizmet güncelleştirmeleri ve/veya yeni desteklenen sürümler olduğunda güncelleştirilir.

Örnek Meta Veri Hizmeti'ni denemek için yukarıdaki bölgelerde [Azure Kaynak Yöneticisi'nden](https://docs.microsoft.com/rest/api/resources/) veya [Azure portalından](https://portal.azure.com) bir VM oluşturun ve aşağıdaki örnekleri izleyin.
IMDS'nin nasıl sorgulanabildiğini ek örnekler [Azure Örneği Meta veri Örnekleri'nde](https://github.com/microsoft/azureimds) bulabilirsiniz

## <a name="usage"></a>Kullanım

### <a name="versioning"></a>Sürüm Oluşturma

Örnek Meta veri hizmeti sürülür ve HTTP isteğindeki API sürümünün belirtilmesi zorunludur.

Bu [kullanılabilirlik tablosunda](#service-availability)listelenen en yeni sürümleri görebilirsiniz.

Daha yeni sürümler eklendikçe, komut dosyalarınızda belirli veri biçimlerine bağımlılık varsa, uyumluluk için eski sürümlere yine de erişilebilir.

Sürüm belirtilmediğinde, en yeni desteklenen sürümlerin listesiyle birlikte bir hata döndürülür.

> [!NOTE]
> Yanıt bir JSON dizesidir. Aşağıdaki örnek yanıt okunabilirlik için oldukça yazdırılmıştır.

**İstek**

```bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance"
```

**Yanıt**

```json
{
    "error": "Bad request. api-version was not specified in the request. For more information refer to aka.ms/azureimds",
    "newest-versions": [
        "2018-10-01",
        "2018-04-02",
        "2018-02-01"
    ]
}
```

### <a name="using-headers"></a>Üstbilgi kullanma

Örnek Meta veri hizmetini sorgularken, isteğin `Metadata: true` istemeden yeniden yönlendirilen olmadığından emin olmak için üstbilgi sağlamanız gerekir.

### <a name="retrieving-metadata"></a>Meta veri alma

Örnek meta verileri, [Azure Kaynak Yöneticisi](https://docs.microsoft.com/rest/api/resources/)kullanılarak oluşturulan/yönetilen VM'leri çalıştırmak için kullanılabilir. Aşağıdaki isteği kullanarak sanal makine örneği için tüm veri kategorilerini erişin:

```bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance?api-version=2017-08-01"
```

> [!NOTE]
> Tüm örnek meta veri sorguları büyük/küçük harf duyarlıdır.

### <a name="data-output"></a>Veri çıktısı

Varsayılan olarak, Örnek Meta veri Hizmeti verileri`Content-Type: application/json`JSON biçiminde döndürür ( ). Ancak, farklı API'ler istenirse verileri farklı biçimlerde döndürer.
Aşağıdaki tablo, API'lerin destekleyebilecek diğer veri biçimlerinin bir başvurusudur.

API | Varsayılan Veri Biçimi | Diğer Biçimler
--------|---------------------|--------------
/instance | json | metin
/scheduledevents | json | yok
/test edildi | json | yok

Varsayılan olmayan yanıt biçimine erişmek için, istekte istenen biçimi sorgu dize parametresi olarak belirtin. Örneğin:

```bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance?api-version=2017-08-01&format=text"
```

> [!NOTE]
> Yaprak düğümleri `format=json` için çalışmıyor. Varsayılan biçimi `format=text` json ise bu sorgular için açıkça belirtilmesi gerekir.

### <a name="security"></a>Güvenlik

Instance Meta data Service bitiş noktasına yalnızca routable olmayan bir IP adresinde çalışan sanal makine örneğinin içinden erişilebilir. Buna ek olarak, `X-Forwarded-For` üstbilgi içeren herhangi bir istek hizmet tarafından reddedilir.
İstekler, gerçek `Metadata: true` isteğin kasıtsız yeniden yönlendirmenin bir parçası değil, doğrudan amaçlandığını sağlamak için bir üstbilgi de içermelidir.

### <a name="error"></a>Hata

Bir veri öğesi bulunamazsa veya yanlış biçimlendirilmiş bir istek varsa, Örnek Meta veri hizmeti standart HTTP hatalarını döndürür. Örneğin:

HTTP Durum Kodu | Neden
----------------|-------
200 TAMAM |
400 Kötü İstek | Yaprak `Metadata: true` düğümü sorgularken üstbilgieksik veya biçim eksik
404 Bulunamadı | İstenen öğe yok
405 Yöntemine İzin Verilmiyor | Yalnızca `GET` istekler desteklenir
410 Gitti | En fazla 70 saniye süreyle bir süre sonra yeniden deneyin
429 Çok Fazla İstek Var | API şu anda saniyede en fazla 5 sorgu destekler
500 Servis Hatası     | Bir süre sonra yeniden deneyin

### <a name="examples"></a>Örnekler

> [!NOTE]
> Tüm API yanıtları JSON dizeleri vardır. Aşağıdaki tüm örnek yanıtlar okunabilirlik için oldukça yazdırılır.

#### <a name="retrieving-network-information"></a>Ağ bilgilerini alma

**İstek**

```bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance/network?api-version=2017-08-01"
```

**Yanıt**

> [!NOTE]
> Yanıt bir JSON dizesidir. Aşağıdaki örnek yanıt okunabilirlik için oldukça yazdırılmıştır.

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

#### <a name="retrieving-public-ip-address"></a>Genel IP adresi alma

```bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance/network/interface/0/ipv4/ipAddress/0/publicIpAddress?api-version=2017-08-01&format=text"
```

#### <a name="retrieving-all-metadata-for-an-instance"></a>Bir örnek için tüm meta verileri alma

**İstek**

```bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance?api-version=2019-06-01"
```

**Yanıt**

> [!NOTE]
> Yanıt bir JSON dizesidir. Aşağıdaki örnek yanıt okunabilirlik için oldukça yazdırılmıştır.

```json
{
  "compute": {
    "azEnvironment": "AzurePublicCloud",
    "customData": "",
    "location": "centralus",
    "name": "negasonic",
    "offer": "lampstack",
    "osType": "Linux",
    "placementGroupId": "",
    "plan": {
        "name": "5-6",
        "product": "lampstack",
        "publisher": "bitnami"
    },
    "platformFaultDomain": "0",
    "platformUpdateDomain": "0",
    "provider": "Microsoft.Compute",
    "publicKeys": [],
    "publisher": "bitnami",
    "resourceGroupName": "myrg",
    "resourceId": "/subscriptions/xxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx/resourceGroups/myrg/providers/Microsoft.Compute/virtualMachines/negasonic",
    "sku": "5-6",
    "storageProfile": {
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
    },
    "subscriptionId": "xxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx",
    "tags": "Department:IT;Environment:Test;Role:WebRole",
    "version": "7.1.1902271506",
    "vmId": "13f56399-bd52-4150-9748-7190aae1ff21",
    "vmScaleSetName": "",
    "vmSize": "Standard_A1_v2",
    "zone": "1"
  },
  "network": {
    "interface": [
      {
        "ipv4": {
          "ipAddress": [
            {
              "privateIpAddress": "10.1.2.5",
              "publicIpAddress": "X.X.X.X"
            }
          ],
          "subnet": [
            {
              "address": "10.1.2.0",
              "prefix": "24"
            }
          ]
        },
        "ipv6": {
          "ipAddress": []
        },
        "macAddress": "000D3A36DDED"
      }
    ]
  }
}
```

#### <a name="retrieving-metadata-in-windows-virtual-machine"></a>Windows Virtual Machine'de meta veri alma

**İstek**

Örnek meta veriler `curl` program aracılığıyla Windows'dan alınabilir:

```powershell
curl -H @{'Metadata'='true'} http://169.254.169.254/metadata/instance?api-version=2019-06-01 | select -ExpandProperty Content
```

Veya `Invoke-RestMethod` PowerShell cmdlet ile:

```powershell

Invoke-RestMethod -Headers @{"Metadata"="true"} -URI http://169.254.169.254/metadata/instance?api-version=2019-06-01 -Method get
```

**Yanıt**

> [!NOTE]
> Yanıt bir JSON dizesidir. Aşağıdaki örnek yanıt okunabilirlik için oldukça yazdırılmıştır.

```json
{
  "compute": {
    "azEnvironment": "AzurePublicCloud",
    "customData": "",
    "location": "centralus",
    "name": "negasonic",
    "offer": "lampstack",
    "osType": "Linux",
    "placementGroupId": "",
    "plan": {
        "name": "5-6",
        "product": "lampstack",
        "publisher": "bitnami"
    },
    "platformFaultDomain": "0",
    "platformUpdateDomain": "0",
    "provider": "Microsoft.Compute",
    "publicKeys": [],
    "publisher": "bitnami",
    "resourceGroupName": "myrg",
    "resourceId": "/subscriptions/xxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx/resourceGroups/myrg/providers/Microsoft.Compute/virtualMachines/negasonic",
    "sku": "5-6",
    "storageProfile": {
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
    },
    "subscriptionId": "xxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx",
    "tags": "Department:IT;Environment:Test;Role:WebRole",
    "version": "7.1.1902271506",
    "vmId": "13f56399-bd52-4150-9748-7190aae1ff21",
    "vmScaleSetName": "",
    "vmSize": "Standard_A1_v2",
    "zone": "1"
  },
  "network": {
    "interface": [
      {
        "ipv4": {
          "ipAddress": [
            {
              "privateIpAddress": "10.0.1.4",
              "publicIpAddress": "X.X.X.X"
            }
          ],
          "subnet": [
            {
              "address": "10.0.1.0",
              "prefix": "24"
            }
          ]
        },
        "ipv6": {
          "ipAddress": []
        },
        "macAddress": "002248020E1E"
      }
    ]
  }
}
```

## <a name="metadata-apis"></a>Meta veri API'leri

Aşağıdaki API'ler meta veri bitiş noktası üzerinden kullanılabilir:

Veriler | Açıklama | Tanıtılan Sürüm
-----|-------------|-----------------------
kanıtlanmıştır | Bkz. [Test Edilen Veriler](#attested-data) | 2018-10-01
identity | Azure kaynakları için yönetilen kimlikler. Erişim [jetonu edinin](../../active-directory/managed-identities-azure-resources/how-to-use-vm-token.md) bkz. | 2018-02-01
örnek | [Bkz. Örnek API](#instance-api) | 2017-04-02
planlanmış olaylar | [Zamanlanan Etkinliklere](scheduled-events.md) Bakın | 2017-08-01

### <a name="instance-api"></a>Örnek API

Aşağıdaki İşlem kategorileri Örnek API'de kullanılabilir:

> [!NOTE]
> Meta veri bitiş noktası aracılığıyla, aşağıdaki kategorilere instance/compute aracılığıyla erişilir

Veriler | Açıklama | Tanıtılan Sürüm
-----|-------------|-----------------------
azÇevre | VM'nin çalıştığı Azure Ortamı | 2018-10-01
Customdata | Bu özellik şu anda devre dışı bırakılmıştır ve kullanılabilir olduğunda bu belgeyi güncelleştireceğiz | 2019-02-01
location | VM'nin çalıştırolduğu Azure Bölgesi | 2017-04-02
ad | VM'nin adı | 2017-04-02
teklif | VM görüntüsü için bilgi sunun ve yalnızca Azure resim galerisinden dağıtılan resimler için mevcuttur | 2017-04-02
osType | Linux veya Windows | 2017-04-02
yerleşimGroupId | Sanal makine ölçek kümenizin [Yerleşim Grubu](../../virtual-machine-scale-sets/virtual-machine-scale-sets-placement-groups.md) | 2017-08-01
plan | Azure Marketi Görüntüsüyse VM için ad, ürün ve yayımcı [içeren](https://docs.microsoft.com/rest/api/compute/virtualmachines/createorupdate#plan) plan | 2018-04-02
platformUpdateDomain |  VM'nin çalıştıran [etki alanını güncelleştirme](manage-availability.md) | 2017-04-02
platformFaultDomain | VM'nin çalıştıran [hata etki alanı](manage-availability.md) | 2017-04-02
Sağlayıcı | VM Sağlayıcısı | 2018-10-01
publicKeys | VM ve yollara atanan [Ortak Anahtarların toplanması](https://docs.microsoft.com/rest/api/compute/virtualmachines/createorupdate#sshpublickey) | 2018-04-02
yayımcı | VM resminin yayıncısı | 2017-04-02
resourceGroupName | Sanal Makineniz için [kaynak grubu](../../azure-resource-manager/management/overview.md) | 2017-08-01
resourceId | Kaynağın [tam nitelikli](https://docs.microsoft.com/rest/api/resources/resources/getbyid) kimliği | 2019-03-11
Sku | VM görüntü için özel SKU | 2017-04-02
depolamaProfil | [Bkz. Depolama Profili](#storage-profile) | 2019-06-01
subscriptionId | Sanal Makine için Azure aboneliği | 2017-08-01
etiketler | Sanal Makine için [Etiketler](../../azure-resource-manager/management/tag-resources.md)  | 2017-08-01
etiketlerListe | Etiketler daha kolay programlı ayrışma için bir JSON dizisi olarak biçimlendirilmiş  | 2019-06-04
version | VM görüntüsünün sürümü | 2017-04-02
vmId | VM için [benzersiz tanımlayıcı](https://azure.microsoft.com/blog/accessing-and-using-azure-vm-unique-id/) | 2017-04-02
vmScaleSetName | [Sanal makine ölçeği kümesi](../../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) Sanal makine ölçek setinizin adı | 2017-12-01
vmSize | [VM boyutu](sizes.md) | 2017-04-02
bölge | Sanal makinenizin [Kullanılabilirlik Bölgesi](../../availability-zones/az-overview.md) | 2017-12-01

Aşağıdaki Ağ kategorileri Örnek API'de kullanılabilir:

> [!NOTE]
> Meta veri bitiş noktası aracılığıyla, aşağıdaki kategorilere örnek/ağ/arayüz üzerinden erişilir

Veriler | Açıklama | Tanıtılan Sürüm
-----|-------------|-----------------------
ipv4/privateIpAddress | VM'nin yerel IPv4 adresi | 2017-04-02
ipv4/publicIpAddress | VM'nin genel IPv4 adresi | 2017-04-02
alt ağ/adres | VM'nin alt net adresi | 2017-04-02
alt ağ/önek | Alt ağ öneki, örnek 24 | 2017-04-02
ipv6/ipAddress | VM'nin yerel IPv6 adresi | 2017-04-02
macAddress | VM mac adresi | 2017-04-02

## <a name="attested-data"></a>Kanıtlanmıştır Veriler

Instance Meta veri hizmeti tarafından sunulan senaryonun bir parçası, sağlanan verilerin Azure'dan geldiğine dair garanti ler sağlamaktır. Pazar yeri görüntülerinin Azure'da görüntü olarak görüntü olduğundan emin olabilmesi için bu bilgilerin bir kısmını imzalarız.

### <a name="example-attested-data"></a>Örnek Test Edilen Veriler

> [!NOTE]
> Tüm API yanıtları JSON dizeleri vardır. Aşağıdaki örnek yanıtlar okunabilirlik için oldukça yazdırılmıştır.

 **İstek**

 ```bash
curl -H Metadata:true "http://169.254.169.254/metadata/attested/document?api-version=2018-10-01&nonce=1234567890"

```

Api-sürüm zorunlu bir alandır. Desteklenen API sürümleri için [hizmet kullanılabilirliği bölümüne](#service-availability) bakın.
Nonce isteğe bağlı 10 basamaklı bir dizedir. Sağlanmazsa, IMDS geçerli UTC zaman damgasını yerine döndürür. IMDS'nin önbelleğe alma mekanizması sayesinde, önceden önbelleğe alınmış bir nonce değeri döndürülebilir.

 **Yanıt**

> [!NOTE]
> Yanıt bir JSON dizesidir. Aşağıdaki örnek yanıt okunabilirlik için oldukça yazdırılmıştır.

 ```json
{
 "encoding":"pkcs7","signature":"MIIEEgYJKoZIhvcNAQcCoIIEAzCCA/8CAQExDzANBgkqhkiG9w0BAQsFADCBugYJKoZIhvcNAQcBoIGsBIGpeyJub25jZSI6IjEyMzQ1NjY3NjYiLCJwbGFuIjp7Im5hbWUiOiIiLCJwcm9kdWN0IjoiIiwicHVibGlzaGVyIjoiIn0sInRpbWVTdGFtcCI6eyJjcmVhdGVkT24iOiIxMS8yMC8xOCAyMjowNzozOSAtMDAwMCIsImV4cGlyZXNPbiI6IjExLzIwLzE4IDIyOjA4OjI0IC0wMDAwIn0sInZtSWQiOiIifaCCAj8wggI7MIIBpKADAgECAhBnxW5Kh8dslEBA0E2mIBJ0MA0GCSqGSIb3DQEBBAUAMCsxKTAnBgNVBAMTIHRlc3RzdWJkb21haW4ubWV0YWRhdGEuYXp1cmUuY29tMB4XDTE4MTEyMDIxNTc1N1oXDTE4MTIyMDIxNTc1NlowKzEpMCcGA1UEAxMgdGVzdHN1YmRvbWFpbi5tZXRhZGF0YS5henVyZS5jb20wgZ8wDQYJKoZIhvcNAQEBBQADgY0AMIGJAoGBAML/tBo86ENWPzmXZ0kPkX5dY5QZ150mA8lommszE71x2sCLonzv4/UWk4H+jMMWRRwIea2CuQ5RhdWAHvKq6if4okKNt66fxm+YTVz9z0CTfCLmLT+nsdfOAsG1xZppEapC0Cd9vD6NCKyE8aYI1pliaeOnFjG0WvMY04uWz2MdAgMBAAGjYDBeMFwGA1UdAQRVMFOAENnYkHLa04Ut4Mpt7TkJFfyhLTArMSkwJwYDVQQDEyB0ZXN0c3ViZG9tYWluLm1ldGFkYXRhLmF6dXJlLmNvbYIQZ8VuSofHbJRAQNBNpiASdDANBgkqhkiG9w0BAQQFAAOBgQCLSM6aX5Bs1KHCJp4VQtxZPzXF71rVKCocHy3N9PTJQ9Fpnd+bYw2vSpQHg/AiG82WuDFpPReJvr7Pa938mZqW9HUOGjQKK2FYDTg6fXD8pkPdyghlX5boGWAMMrf7bFkup+lsT+n2tRw2wbNknO1tQ0wICtqy2VqzWwLi45RBwTGB6DCB5QIBATA/MCsxKTAnBgNVBAMTIHRlc3RzdWJkb21haW4ubWV0YWRhdGEuYXp1cmUuY29tAhBnxW5Kh8dslEBA0E2mIBJ0MA0GCSqGSIb3DQEBCwUAMA0GCSqGSIb3DQEBAQUABIGAld1BM/yYIqqv8SDE4kjQo3Ul/IKAVR8ETKcve5BAdGSNkTUooUGVniTXeuvDj5NkmazOaKZp9fEtByqqPOyw/nlXaZgOO44HDGiPUJ90xVYmfeK6p9RpJBu6kiKhnnYTelUk5u75phe5ZbMZfBhuPhXmYAdjc7Nmw97nx8NnprQ="
}
```

İmza blob belgenin [bir pkcs7](https://aka.ms/pkcs7) imzalı sürümüdür. VmId, sku, nonce, subscriptionId, timeStamp oluşturma ve belgenin sona ermesi için vm ayrıntıları ile birlikte imzalamak için kullanılan sertifika ve görüntü hakkında plan bilgileri içerir. Plan bilgileri yalnızca Azure Market yeri görüntüleri için doldurulur. Sertifika yanıttan ayıklanabilir ve yanıtın geçerli olduğunu ve Azure'dan geldiğini doğrulamak için kullanılabilir.

#### <a name="retrieving-attested-metadata-in-windows-virtual-machine"></a>Windows Virtual Machine'de doğrulanması doğruedilen meta verileri alma

 **İstek**

Örnek meta veriler PowerShell yardımcı programı `curl`üzerinden Windows'da alınabilir:

 ```powershell
curl -H @{'Metadata'='true'} "http://169.254.169.254/metadata/attested/document?api-version=2018-10-01&nonce=1234567890" | select -ExpandProperty Content
```

 Ya da `Invoke-RestMethod` cmdlet ile:

 ```powershell
Invoke-RestMethod -Headers @{"Metadata"="true"} -URI "http://169.254.169.254/metadata/attested/document?api-version=2018-10-01&nonce=1234567890" -Method get
```

Api-sürüm zorunlu bir alandır. Desteklenen API sürümleri için hizmet kullanılabilirliği bölümüne bakın.
Nonce isteğe bağlı 10 basamaklı bir dizedir. Sağlanmazsa, IMDS geçerli UTC zaman damgasını yerine döndürür. IMDS'nin önbelleğe alma mekanizması sayesinde, önceden önbelleğe alınmış bir nonce değeri döndürülebilir.

 **Yanıt**

> [!NOTE]
> Yanıt bir JSON dizesidir. Aşağıdaki örnek yanıt okunabilirlik için oldukça yazdırılmıştır.

 ```json
{
 "encoding":"pkcs7","signature":"MIIEEgYJKoZIhvcNAQcCoIIEAzCCA/8CAQExDzANBgkqhkiG9w0BAQsFADCBugYJKoZIhvcNAQcBoIGsBIGpeyJub25jZSI6IjEyMzQ1NjY3NjYiLCJwbGFuIjp7Im5hbWUiOiIiLCJwcm9kdWN0IjoiIiwicHVibGlzaGVyIjoiIn0sInRpbWVTdGFtcCI6eyJjcmVhdGVkT24iOiIxMS8yMC8xOCAyMjowNzozOSAtMDAwMCIsImV4cGlyZXNPbiI6IjExLzIwLzE4IDIyOjA4OjI0IC0wMDAwIn0sInZtSWQiOiIifaCCAj8wggI7MIIBpKADAgECAhBnxW5Kh8dslEBA0E2mIBJ0MA0GCSqGSIb3DQEBBAUAMCsxKTAnBgNVBAMTIHRlc3RzdWJkb21haW4ubWV0YWRhdGEuYXp1cmUuY29tMB4XDTE4MTEyMDIxNTc1N1oXDTE4MTIyMDIxNTc1NlowKzEpMCcGA1UEAxMgdGVzdHN1YmRvbWFpbi5tZXRhZGF0YS5henVyZS5jb20wgZ8wDQYJKoZIhvcNAQEBBQADgY0AMIGJAoGBAML/tBo86ENWPzmXZ0kPkX5dY5QZ150mA8lommszE71x2sCLonzv4/UWk4H+jMMWRRwIea2CuQ5RhdWAHvKq6if4okKNt66fxm+YTVz9z0CTfCLmLT+nsdfOAsG1xZppEapC0Cd9vD6NCKyE8aYI1pliaeOnFjG0WvMY04uWz2MdAgMBAAGjYDBeMFwGA1UdAQRVMFOAENnYkHLa04Ut4Mpt7TkJFfyhLTArMSkwJwYDVQQDEyB0ZXN0c3ViZG9tYWluLm1ldGFkYXRhLmF6dXJlLmNvbYIQZ8VuSofHbJRAQNBNpiASdDANBgkqhkiG9w0BAQQFAAOBgQCLSM6aX5Bs1KHCJp4VQtxZPzXF71rVKCocHy3N9PTJQ9Fpnd+bYw2vSpQHg/AiG82WuDFpPReJvr7Pa938mZqW9HUOGjQKK2FYDTg6fXD8pkPdyghlX5boGWAMMrf7bFkup+lsT+n2tRw2wbNknO1tQ0wICtqy2VqzWwLi45RBwTGB6DCB5QIBATA/MCsxKTAnBgNVBAMTIHRlc3RzdWJkb21haW4ubWV0YWRhdGEuYXp1cmUuY29tAhBnxW5Kh8dslEBA0E2mIBJ0MA0GCSqGSIb3DQEBCwUAMA0GCSqGSIb3DQEBAQUABIGAld1BM/yYIqqv8SDE4kjQo3Ul/IKAVR8ETKcve5BAdGSNkTUooUGVniTXeuvDj5NkmazOaKZp9fEtByqqPOyw/nlXaZgOO44HDGiPUJ90xVYmfeK6p9RpJBu6kiKhnnYTelUk5u75phe5ZbMZfBhuPhXmYAdjc7Nmw97nx8NnprQ="
}
```

İmza blob belgenin [bir pkcs7](https://aka.ms/pkcs7) imzalı sürümüdür. VmId, sku, nonce, subscriptionId, timeStamp oluşturma ve belgenin sona ermesi için vm ayrıntıları ile birlikte imzalamak için kullanılan sertifika ve görüntü hakkında plan bilgileri içerir. Plan bilgileri yalnızca Azure Market yeri görüntüleri için doldurulur. Sertifika yanıttan ayıklanabilir ve yanıtın geçerli olduğunu ve Azure'dan geldiğini doğrulamak için kullanılabilir.

## <a name="example-scenarios-for-usage"></a>Örnek kullanım senaryoları  

### <a name="tracking-vm-running-on-azure"></a>Azure üzerinde çalıştırılan VM'yi izleme

Bir hizmet sağlayıcısı olarak, yazılımınızı çalıştıran VM sayısını izlemeniz veya VM'nin benzersizliğini izlemesi gereken aracılara sahip olmanız gerekebilir. Bir VM için benzersiz bir kimlik alabilmek için, Örnek Meta veri Hizmeti'nden `vmId` alanı kullanın.

**İstek**

```bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute/vmId?api-version=2017-08-01&format=text"
```

**Yanıt**

```text
5c08b38e-4d57-4c23-ac45-aca61037f084
```

### <a name="placement-of-containers-data-partitions-based-faultupdate-domain"></a>Hata/güncelleştirme etki alanı temelinde kapsayıcıların, veri bölümlerinin yerleşimi

Belirli senaryolar için, farklı veri yinelemelerinin yerleştirilmesi çok önemlidir. Örneğin, bir [orkestratör](https://kubernetes.io/docs/user-guide/node-selection/) aracılığıyla [HDFS çoğaltma yerleşimi](https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html#Replica_Placement:_The_First_Baby_Steps) veya kapsayıcı `platformFaultDomain` `platformUpdateDomain` yerleşimi, VM'nin çalıştığını ve üzerinde çalıştığını bilmenizi gerektirebilir.
Bu kararları almak için durumlar için [Kullanılabilirlik Bölgeleri'ni](../../availability-zones/az-overview.md) de kullanabilirsiniz.
Bu verileri doğrudan Örnek Meta veri Hizmeti üzerinden sorgulayabilirsiniz.

**İstek**

```bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute/platformFaultDomain?api-version=2017-08-01&format=text"
```

**Yanıt**

```text
0
```

### <a name="getting-more-information-about-the-vm-during-support-case"></a>Destek olayı sırasında VM hakkında daha fazla bilgi alma

Bir servis sağlayıcısı olarak, VM hakkında daha fazla bilgi almak istediğiniz bir destek çağrısı alabilirsiniz. Müşteriden bilgi işlem meta verilerini paylaşmasını istemek, destek uzmanının Azure'daki VM türü hakkında bilgisi olan temel bilgileri sağlayabilir.

**İstek**

```bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute?api-version=2019-06-01"
```

**Yanıt**

> [!NOTE]
> Yanıt bir JSON dizesidir. Aşağıdaki örnek yanıt okunabilirlik için oldukça yazdırılmıştır.

```json
{
    "azEnvironment": "AzurePublicCloud",
    "customData": "",
    "location": "centralus",
    "name": "negasonic",
    "offer": "lampstack",
    "osType": "Linux",
    "placementGroupId": "",
    "plan": {
        "name": "5-6",
        "product": "lampstack",
        "publisher": "bitnami"
    },
    "platformFaultDomain": "0",
    "platformUpdateDomain": "0",
    "provider": "Microsoft.Compute",
    "publicKeys": [],
    "publisher": "bitnami",
    "resourceGroupName": "myrg",
    "resourceId": "/subscriptions/xxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx/resourceGroups/myrg/providers/Microsoft.Compute/virtualMachines/negasonic",
    "sku": "5-6",
    "storageProfile": {
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
    },
    "subscriptionId": "xxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx",
    "tags": "Department:IT;Environment:Test;Role:WebRole",
    "version": "7.1.1902271506",
    "vmId": "13f56399-bd52-4150-9748-7190aae1ff21",
    "vmScaleSetName": "",
    "vmSize": "Standard_A1_v2",
    "zone": "1"
}
```

### <a name="getting-azure-environment-where-the-vm-is-running"></a>VM'nin çalıştırıldığı Azure Ortamını alma

Azure'da Azure [Kamu](https://azure.microsoft.com/overview/clouds/government/)gibi çeşitli egemen bulutlar vardır. Bazen bazı çalışma zamanı kararları vermek için Azure Ortamı'na ihtiyacınız vardır. Aşağıdaki örnek, bu davranışı nasıl elde abileceğinizi gösterir.

**İstek**
```bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute/azEnvironment?api-version=2018-10-01&format=text"
```

**Yanıt**
```bash
AzurePublicCloud
```

Azure Ortamı'nın bölgeleri ve değerleri aşağıda listelenmiştir.

 Bölgeler | Azure Ortamı
---------|-----------------
[Tüm Genel Olarak Kullanılabilen Küresel Azure Bölgeleri](https://azure.microsoft.com/regions/)     | AzurePublicCloud
[Azure Devlet Kurumları](https://azure.microsoft.com/overview/clouds/government/)              | AzureUSGovernmentCloud
[Azure Çin 21Vianet](https://azure.microsoft.com/global-infrastructure/china)          | AzureChinaCloud
[Azure Almanya](https://azure.microsoft.com/overview/clouds/germany/)                    | AzureGermanCloud

### <a name="getting-the-tags-for-the-vm"></a>VM için etiketleri alma

Etiketler, mantıksal olarak taksonomi olarak düzenlemek için Azure VM'nize uygulanmış olabilir. VM'ye atanan etiketler aşağıdaki istek kullanılarak alınabilir.

**İstek**

```bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute/tags?api-version=2018-10-01&format=text"
```

**Yanıt**

```text
Department:IT;Environment:Test;Role:WebRole
```

Alan, `tags` etiketleri yarı iki nokta üst üste ile sınırlandırılmış bir dizedir. Etiketlerde yarı kolon kullanılıyorsa bu bir sorun olabilir. Etiketleri programlı bir şekilde ayıklamak için bir ayırıcı `tagsList` yazılırsa, sınırlayıcısı olmayan bir JSON dizisi olan alana güvenmeniz gerekir ve sonuç olarak ayrıştırılması daha kolaydır.

**İstek**

```bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute/tagsList?api-version=2019-06-04&format=JSON"
```

**Yanıt**

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

### <a name="validating-that-the-vm-is-running-in-azure"></a>VM'nin Azure'da çalıştırıldığını doğrulama

Pazar yeri satıcıları, yazılımlarının yalnızca Azure'da çalıştırmak için lisanslı olduğundan emin olmak ister. Eğer birisi VHD'yi şirket içinde kopyalarsa, o zaman bunu tespit etme yeteneğine sahip olmalıdır. Market satıcıları, Örnek Meta veri hizmetine çağrılayarak yalnızca Azure'dan yanıt almayı garanti eden imzalı veriler alabilir.

> [!NOTE]
> JQ yüklü olması gerekir.

**İstek**

 ```bash
  # Get the signature
   curl  --silent -H Metadata:True http://169.254.169.254/metadata/attested/document?api-version=2019-04-30 | jq -r '.["signature"]' > signature
  # Decode the signature
  base64 -d signature > decodedsignature
  #Get PKCS7 format
  openssl pkcs7 -in decodedsignature -inform DER -out sign.pk7
  # Get Public key out of pkc7
  openssl pkcs7 -in decodedsignature -inform DER  -print_certs -out signer.pem
  #Get the intermediate certificate
  wget -q -O intermediate.cer "$(openssl x509 -in signer.pem -text -noout | grep " CA Issuers -" | awk -FURI: '{print $2}')"
  openssl x509 -inform der -in intermediate.cer -out intermediate.pem
  #Verify the contents
  openssl smime -verify -in sign.pk7 -inform pem -noverify
 ```

 **Yanıt**

```json
Verification successful
{"nonce":"20181128-001617",
  "plan":
    {
     "name":"",
     "product":"",
     "publisher":""
    },
"timeStamp":
  {
    "createdOn":"11/28/18 00:16:17 -0000",
    "expiresOn":"11/28/18 06:16:17 -0000"
  },
"vmId":"d3e0e374-fda6-4649-bbc9-7f20dc379f34",
"subscriptionId": "xxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx",
"sku": "RS3-Pro"
}
```

Veriler | Açıklama
-----|------------
Nonce | Kullanıcı isteğe bağlı dize isteği ile birlikte verilir. İstekte nonce sağlanmışsa, geçerli UTC zaman damgası döndürülür
plan | Bir Azure Marketi Görüntüsünde VM [planı,](https://docs.microsoft.com/rest/api/compute/virtualmachines/createorupdate#plan) ad, ürün ve yayımcı içerir
zaman damgası/createdOn | İlk imzalanmış belgenin oluşturulduğu UTC zaman damgası
zaman damgası/süresiOn | İmzalanan belgenin süresinin dolduğu UTC zaman damgası
vmId |  VM için [benzersiz tanımlayıcı](https://azure.microsoft.com/blog/accessing-and-using-azure-vm-unique-id/)
subscriptionId | Sanal Makine için Azure aboneliği,`2019-04-30`
Sku | VM görüntü için özel SKU, tanıtıldı`2019-11-01`

#### <a name="verifying-the-signature"></a>İmzayı doğrulama

Yukarıdaki imzayı aldıktan sonra, imzanın Microsoft'tan geldiğini doğrulayabilirsiniz. Ayrıca ara sertifikayı ve sertifika zincirini de doğrulayabilirsiniz. Son olarak, abonelik kimliğinin doğru olduğunu doğrulayabilirsiniz.

> [!NOTE]
> Genel bulut ve egemen bulut sertifikası farklı olacaktır.

 Bulut | Sertifika
---------|-----------------
[Tüm Genel Olarak Kullanılabilen Küresel Azure Bölgeleri](https://azure.microsoft.com/regions/)     | *.metadata.azure.com
[Azure Devlet Kurumları](https://azure.microsoft.com/overview/clouds/government/)              | *.metadata.azure.us
[Azure Çin 21Vianet](https://azure.microsoft.com/global-infrastructure/china/)         | *.metadata.azure.cn
[Azure Almanya](https://azure.microsoft.com/overview/clouds/germany/)                    | *.metadata.microsoftazure.de

İmzalama için kullanılan sertifika etrafında bilinen bir sorun vardır. Sertifikalar, genel bulutiçin `metadata.azure.com` tam bir eşleşmeye sahip olmayabilir. Bu nedenle sertifika doğrulama herhangi `.metadata.azure.com` bir alt etki alanından ortak bir ad izin vermelidir.

```bash

# Verify the subject name for the main certificate
openssl x509 -noout -subject -in signer.pem
# Verify the issuer for the main certificate
openssl x509 -noout -issuer -in signer.pem
#Validate the subject name for intermediate certificate
openssl x509 -noout -subject -in intermediate.pem
# Verify the issuer for the intermediate certificate
openssl x509 -noout -issuer -in intermediate.pem
# Verify the certificate chain, for Azure China 21Vianet the intermediate certificate will be from DigiCert Global Root CA
openssl verify -verbose -CAfile /etc/ssl/certs/Baltimore_CyberTrust_Root.pem -untrusted intermediate.pem signer.pem
```

Doğrulama sırasında ağ kısıtlamaları nedeniyle ara sertifikanın indirilemediği durumlarda, ara sertifika sabitlenebilir. Ancak Azure, standart PKI uygulamasına göre sertifikaları devreder. Sabitlenmiş sertifikaların rollover olduğunda güncelleştirilmesi gerekir. Ara sertifikayı güncelleştirmek için bir değişiklik planlandığında, Azure blogu güncelleştirilir ve Azure müşterileri bildirilir. Ara sertifikalara [buradan](https://www.microsoft.com/pki/mscorp/cps/default.htm)ulabilirsiniz. Bölgelerin her biri için ara sertifikalar farklı olabilir.

> [!NOTE]
> Azure China 21Vianet'in ara sertifikası Baltimore yerine DigiCert Global Root CA'dan olacaktır.
Ayrıca, kök zinciri yetkisi değişikliğinin bir parçası olarak Azure China için ara sertifikaları sabitlediyseniz, ara sertifikaların güncellenmesi gerekir.

### <a name="failover-clustering-in-windows-server"></a>Windows Server'da Failover Kümeleme

Belirli senaryolar için, Failover Clustering ile Örnek Meta veri hizmeti sorgulanırken, yönlendirme tablosuna bir rota eklemek gerekir.

1. Yönetici ayrıcalıkları içeren komut istemini açın.

2. Aşağıdaki komutu çalıştırın ve IPv4 Rota`0.0.0.0`Tablosu'ndaki Ağ Hedefi Arabirimi 'nin () adresini not edin.

```bat
route print
```

> [!NOTE]
> Failover Cluster etkin bir Windows Server VM aşağıdaki örnek çıktı basitlik için yalnızca IPv4 Route Table içerir.

```bat
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
        224.0.0.0        240.0.0.0         On-link         10.0.1.10    266
  255.255.255.255  255.255.255.255         On-link         127.0.0.1    331
  255.255.255.255  255.255.255.255         On-link     169.254.1.156    271
  255.255.255.255  255.255.255.255         On-link         10.0.1.10    266
```

1. Aşağıdaki komutu çalıştırın ve bu örnekte`0.0.0.0`olan Ağ`10.0.1.10`Hedefi Için Arabirim () adresini kullanın.

```bat
route add 169.254.169.254/32 10.0.1.10 metric 1 -p
```

### <a name="storage-profile"></a>Depolama profili

Örnek Meta veri hizmeti, VM ile ilişkili depolama diskleri hakkında ayrıntılı bilgi sağlayabilir. Bu veriler instance/compute/storageProfil bitiş noktasında bulunabilir.

VM'nin depolama profili üç kategoriye ayrılır : resim başvurusu, işletim sistemi diski ve veri diskleri.

Görüntü başvuru nesnesi işletim sistemi görüntüsü hakkında aşağıdaki bilgileri içerir:

Veriler    | Açıklama
--------|-----------------
id      | Kaynak kimliği
teklif   | Platform veya pazar görüntüsü teklifi
yayımcı | Görüntü yayıncısı
Sku     | Görüntü sku
version | Platform veya pazar görüntüsünün sürümü

Os disk nesnesi VM tarafından kullanılan işletim sistemi diski hakkında aşağıdaki bilgileri içerir:

Veriler    | Açıklama
--------|-----------------
Önbelleğe alma | Önbelleğe alma gereksinimleri
createOption | VM'nin nasıl oluşturulduğu hakkında bilgi
diffDiskSettings | Geçici disk ayarları
diskSizeGB | GB'deki diskin boyutu
image   | Kaynak kullanıcı görüntüsü sanal sabit disk
Lun     | Diskin mantıksal birim numarası
yönetilenDisk | Yönetilen disk parametreleri
ad    | Disk adı
Vhd     | Sanal sabit disk
yazmaAcceleratorEtkin | Diskte hızlandırıcı yazıp etkinleştirilen olup olmadığı

Veri diskleri dizisi VM'ye iliştirilmiş veri disklerinin listesini içerir. Her veri disknesnesi aşağıdaki bilgileri içerir:

Veriler    | Açıklama
--------|-----------------
Önbelleğe alma | Önbelleğe alma gereksinimleri
createOption | VM'nin nasıl oluşturulduğu hakkında bilgi
diffDiskSettings | Geçici disk ayarları
diskSizeGB | GB'deki diskin boyutu
şifreleme Ayarları | Disk için şifreleme ayarları
image   | Kaynak kullanıcı görüntüsü sanal sabit disk
yönetilenDisk | Yönetilen disk parametreleri
ad    | Disk adı
osType  | Diskte bulunan işletim sistemi türü
Vhd     | Sanal sabit disk
yazmaAcceleratorEtkin | Diskte hızlandırıcı yazıp etkinleştirilen olup olmadığı

Aşağıda VM'nin depolama bilgilerinin nasıl sorgulanacaklarına bir örnek verilmiştir.

**İstek**

```bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute/storageProfile?api-version=2019-06-01"
```

**Yanıt**

> [!NOTE]
> Yanıt bir JSON dizesidir. Aşağıdaki örnek yanıt okunabilirlik için oldukça yazdırılmıştır.

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

### <a name="examples-of-calling-metadata-service-using-different-languages-inside-the-vm"></a>VM içinde farklı dillerkullanarak meta veri hizmeti arama örnekleri 

Dil | Örnek
---------|----------------
Ruby     | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.rb
Başlayın  | https://github.com/Microsoft/azureimds/blob/master/imdssample.go
Python   | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.py
C++      | https://github.com/Microsoft/azureimds/blob/master/IMDSSample-windows.cpp
C#       | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.cs
JavaScript | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.js
PowerShell | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.ps1
Bash       | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.sh
Perl       | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.pl
Java       | https://github.com/Microsoft/azureimds/blob/master/imdssample.java
Visual Basic | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.vb
Puppet | https://github.com/keirans/azuremetadata

## <a name="faq"></a>SSS

1. Ben hata `400 Bad Request, Required metadata header not specified`alıyorum. Bu ne anlama geliyor?
   * Örnek Meta veri hizmeti, `Metadata: true` üstbilginin istekte geçirilmesini gerektirir. Bu üstbilginin REST çağrısında geçirilmesi, Örnek Meta veri hizmetine erişim sağlar.
2. VM'im için neden bilgi almıyorum?
   * Şu anda Örnek Meta veri hizmeti yalnızca Azure Kaynak Yöneticisi ile oluşturulan örnekleri destekler. Gelecekte, Bulut Hizmeti VM'leri için destek eklenebilir.
3. Sanal Makinemi bir süre önce Azure Kaynak Yöneticisi aracılığıyla oluşturdum. Neden bilgi işlem meta veri bilgilerini göremiyorum?
   * Eyl 2016'dan sonra oluşturulan tüm Sanal O'm'lar için, işlem meta verilerini görmeye başlamak için bir [Etiket](../../azure-resource-manager/management/tag-resources.md) ekleyin. Eski Sanal Taşıtlar için (Eylül 2016'dan önce oluşturulmuş), meta verileri yenilemek için VM'ye uzantıları veya veri diskleri ekleyin/kaldırın.
4. Yeni sürüm için doldurulan tüm verileri göremiyorum
   * Eyl 2016'dan sonra oluşturulan tüm Sanal O'm'lar için, işlem meta verilerini görmeye başlamak için bir [Etiket](../../azure-resource-manager/management/tag-resources.md) ekleyin. Eski Sanal Taşıtlar için (Eylül 2016'dan önce oluşturulmuş), meta verileri yenilemek için VM'ye uzantıları veya veri diskleri ekleyin/kaldırın.
5. Neden hata `500 Internal Server Error`alıyorum?
   * Üstel geri dönüş sistemine göre isteğinizi yeniden deneyin. Sorun devam ederse Azure desteğine başvurun.
6. Ek soruları/yorumları nerede paylaşacağım?
   * Yorumlarınızı https://feedback.azure.comgönderin.
7. Bu Sanal Makine Ölçeği Set Örneği için çalışır mı?
   * Evet Meta veri hizmeti Ölçek Kümesi Örnekleri için kullanılabilir.
8. Hizmet için nasıl destek alabilirim?
   * Hizmet için destek almak için, Uzun yeniden denemelerden sonra meta veri yanıtı alamadığınız VM için Azure portalında bir destek sorunu oluşturun.
9. Servise çağrım için zaman zaman alabilirim?
   * Meta veri aramaları VM'nin birincil ağ kartına atanan birincil IP adresinden yapılmalıdır, ayrıca rotalarınızı değiştirmiş seniz ağ kartınızdan 169.254.0.0.0/16 adresi için bir rota olmalıdır.
10. Etiketlerimi sanal makine ölçeği kümesinde güncelledim, ancak VM'lerden farklı durumlarda görünmüyorlar mı?
    * Şu anda ScaleSets etiketleri yalnızca yeniden başlatma/yeniden görüntü/veya örnekte bir disk değişikliğinde VM'de gösterilmektedir.

    ![Örnek Meta veri desteği](./media/instance-metadata-service/InstanceMetadata-support.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Zamanlanan Etkinlikler](scheduled-events.md) hakkında daha fazla bilgi edinin
