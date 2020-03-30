---
title: Azure İşlemi - Linux Tanı uzantısı
description: Azure'da çalışan Linux VM'lerden ölçümler toplamak ve olayları günlüğe kaydetmek için Azure Linux Tanı Uzantısı (LAD) nasıl yapılandırılabilen.
services: virtual-machines-linux
author: axayjo
manager: gwallace
ms.service: virtual-machines-linux
ms.tgt_pltfrm: vm-linux
ms.topic: article
ms.date: 12/13/2018
ms.author: akjosh
ms.openlocfilehash: 7a7c1af1193ba391550438229a22c4a8c116e6be
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80289184"
---
# <a name="use-linux-diagnostic-extension-to-monitor-metrics-and-logs"></a>Ölçümleri ve günlükleri izlemek için Linux Tanılama Uzantısı’nı kullanma

Bu belge sürüm 3.0 ve Linux Tanı Uzantısı yeni açıklar.

> [!IMPORTANT]
> Sürüm 2.3 ve daha büyük leri hakkında daha fazla bilgi için [bu belgeye](../linux/classic/diagnostic-extension-v2.md)bakın.

## <a name="introduction"></a>Giriş

Linux Tanı uzantısı, kullanıcının Microsoft Azure'da çalışan bir Linux VM'nin durumunu izlemesine yardımcı olur. Aşağıdaki özelliklere sahiptir:

* VM'den sistem performans ölçümlerini toplar ve bunları belirli bir depolama hesabında belirli bir tabloda saklar.
* Günlük olaylarını syslog'dan alır ve bunları belirlenen depolama hesabında belirli bir tabloda saklar.
* Kullanıcıların toplanan ve yüklenen veri ölçümlerini özelleştirmelerine olanak tanır.
* Kullanıcıların toplanan ve yüklenen olayların önem düzeylerini ve sislog olanaklarını özelleştirmelerine olanak tanır.
* Kullanıcıların belirtilen günlük dosyalarını belirlenmiş bir depolama tablosuna yüklemelerine olanak tanır.
* Belirlenen depolama hesabında ki rasgele EventHub uç noktalarına ve JSON biçimli blob'lara ölçümler ve günlük olayları göndermeyi destekler.

Bu uzantı, her iki Azure dağıtım modeliyle de çalışır.

## <a name="installing-the-extension-in-your-vm"></a>Uzantıyı sanal makinenize yükleme

Bu uzantıyı Azure PowerShell cmdlets, Azure CLI komut dosyaları, ARM şablonları veya Azure portalını kullanarak etkinleştirebilirsiniz. Daha fazla bilgi için [Uzantı Özellikleri'ne](features-linux.md)bakın.

Bu yükleme yönergeleri ve [indirilebilir örnek yapılandırma](https://raw.githubusercontent.com/Azure/azure-linux-extensions/master/Diagnostic/tests/lad_2_3_compatible_portal_pub_settings.json) LAD 3.0'ı şu şekilde yapılandırın:

* LAD 2.3 tarafından sağlanan aynı ölçümleri yakalayın ve saklayın;
* LAD 3.0'a yeni bir dosya sistemi ölçümleri kümesi yakalamak;
* LAD 2.3 tarafından etkinleştirilen varsayılan syslog koleksiyonunu yakalamak;
* VM ölçümlerinde grafik ve uyarı için Azure portalı deneyimini etkinleştirin.

İndirilebilir yapılandırma sadece bir örnektir; kendi ihtiyaçlarınıza uyacak şekilde değiştirin.

### <a name="prerequisites"></a>Ön koşullar

* **Azure Linux Aracısı sürüm 2.2.0 veya sonrası.** Azure VM Linux galeri görüntülerinin çoğu sürüm 2.2.7 veya sonraki sürümleri içerir. VM'de yüklü olan sürümü onaylamak için çalıştırın. `/usr/sbin/waagent -version` VM konuk aracının eski bir sürümünü çalıştırıyorsa, güncelleştirmek için [aşağıdaki yönergeleri](https://docs.microsoft.com/azure/virtual-machines/linux/update-agent) izleyin.
* **Azure CLI**. Makinenizde [Azure CLI ortamını ayarlayın.](https://docs.microsoft.com/cli/azure/install-azure-cli)
* Wget komutu, zaten yoksa: Çalıştır `sudo apt-get install wget`.
* Verileri depolamak için varolan bir Azure aboneliği ve içinde bulunan bir depolama hesabı.
* Desteklenen Linux dağıtımları listesihttps://github.com/Azure/azure-linux-extensions/tree/master/Diagnostic#supported-linux-distributions

### <a name="sample-installation"></a>Örnek kurulum

Çalıştırmadan önce ilk bölümdeki değişkenler için doğru değerleri doldurun:

```azurecli
# Set your Azure VM diagnostic variables correctly below
my_resource_group=<your_azure_resource_group_name_containing_your_azure_linux_vm>
my_linux_vm=<your_azure_linux_vm_name>
my_diagnostic_storage_account=<your_azure_storage_account_for_storing_vm_diagnostic_data>

# Should login to Azure first before anything else
az login

# Select the subscription containing the storage account
az account set --subscription <your_azure_subscription_id>

# Download the sample Public settings. (You could also use curl or any web browser)
wget https://raw.githubusercontent.com/Azure/azure-linux-extensions/master/Diagnostic/tests/lad_2_3_compatible_portal_pub_settings.json -O portal_public_settings.json

# Build the VM resource ID. Replace storage account name and resource ID in the public settings.
my_vm_resource_id=$(az vm show -g $my_resource_group -n $my_linux_vm --query "id" -o tsv)
sed -i "s#__DIAGNOSTIC_STORAGE_ACCOUNT__#$my_diagnostic_storage_account#g" portal_public_settings.json
sed -i "s#__VM_RESOURCE_ID__#$my_vm_resource_id#g" portal_public_settings.json

# Build the protected settings (storage account SAS token)
my_diagnostic_storage_account_sastoken=$(az storage account generate-sas --account-name $my_diagnostic_storage_account --expiry 2037-12-31T23:59:00Z --permissions wlacu --resource-types co --services bt -o tsv)
my_lad_protected_settings="{'storageAccountName': '$my_diagnostic_storage_account', 'storageAccountSasToken': '$my_diagnostic_storage_account_sastoken'}"

# Finallly tell Azure to install and enable the extension
az vm extension set --publisher Microsoft.Azure.Diagnostics --name LinuxDiagnostic --version 3.0 --resource-group $my_resource_group --vm-name $my_linux_vm --protected-settings "${my_lad_protected_settings}" --settings portal_public_settings.json
```

Bu örneklerde indirilen örnek yapılandırma, bir dizi standart veri toplar ve bunları tablo depolama alanına gönderir. Örnek yapılandırmanın URL'si ve içeriği değiştirilebilir. Çoğu durumda, json dosyasının bir kopyasını indirmeli ve gereksinimlerinize göre özelleştirmeli, ardından her seferinde bu URL'yi indirmek yerine yapılandırma dosyasının kendi sürümünü kullanarak oluşturabileceğiniz şablonlara veya otomasyona sahip olmalısınız.

#### <a name="powershell-sample"></a>PowerShell örneği

```powershell
$storageAccountName = "yourStorageAccountName"
$storageAccountResourceGroup = "yourStorageAccountResourceGroupName"
$vmName = "yourVMName"
$VMresourceGroup = "yourVMResourceGroupName"

# Get the VM object
$vm = Get-AzVM -Name $vmName -ResourceGroupName $VMresourceGroup

# Get the public settings template from GitHub and update the templated values for storage account and resource ID
$publicSettings = (Invoke-WebRequest -Uri https://raw.githubusercontent.com/Azure/azure-linux-extensions/master/Diagnostic/tests/lad_2_3_compatible_portal_pub_settings.json).Content
$publicSettings = $publicSettings.Replace('__DIAGNOSTIC_STORAGE_ACCOUNT__', $storageAccountName)
$publicSettings = $publicSettings.Replace('__VM_RESOURCE_ID__', $vm.Id)

# If you have your own customized public settings, you can inline those rather than using the template above: $publicSettings = '{"ladCfg":  { ... },}'

# Generate a SAS token for the agent to use to authenticate with the storage account
$sasToken = New-AzStorageAccountSASToken -Service Blob,Table -ResourceType Service,Container,Object -Permission "racwdlup" -Context (Get-AzStorageAccount -ResourceGroupName $storageAccountResourceGroup -AccountName $storageAccountName).Context

# Build the protected settings (storage account SAS token)
$protectedSettings="{'storageAccountName': '$storageAccountName', 'storageAccountSasToken': '$sasToken'}"

# Finally install the extension with the settings built above
Set-AzVMExtension -ResourceGroupName $VMresourceGroup -VMName $vmName -Location $vm.Location -ExtensionType LinuxDiagnostic -Publisher Microsoft.Azure.Diagnostics -Name LinuxDiagnostic -SettingString $publicSettings -ProtectedSettingString $protectedSettings -TypeHandlerVersion 3.0 
```

### <a name="updating-the-extension-settings"></a>Uzantı ayarlarını güncelleştirme

Korumalı veya Genel ayarlarınızı değiştirdikten sonra, aynı komutu çalıştırarak bunları VM'ye dağıtın. Ayarlarda herhangi bir değişiklik olursa, güncelleştirilmiş ayarlar uzantıya gönderilir. LAD yapılandırmayı yeniden yükler ve kendisini yeniden başlatır.

### <a name="migration-from-previous-versions-of-the-extension"></a>Uzantının önceki sürümlerinden geçiş

Uzantının en son sürümü **3.0'dır.** **Eski sürümler (2.x) amortismana alınır ve 31 Temmuz 2018 tarihinde veya sonrasında yayınlanamaz.**

> [!IMPORTANT]
> Bu uzantı, uzantının yapılandırmasına son dakika değişiklikleri sunar. Bu değişikliklerden biri, uzantının güvenliğini artırmak için yapıldı; sonuç olarak, 2.x ile geriye doğru uyumluluk sağlanamadı. Ayrıca, bu uzantınca için Uzantı Yayımcısı 2.x sürümleri için yayımcı farklıdır.
>
> 2.x'ten uzantının bu yeni sürümüne geçiş yapmak için eski uzantıyı kaldırmanız (eski yayımcı adı altında) ardından uzantının sürüm 3'ü yüklemeniz gerekir.

Öneri:

* Otomatik minör sürüm yükseltmesi etkinleştirilmiş uzantıyı yükleyin.
  * Klasik dağıtım modeli VM'lerde, uzantıyı Azure XPLAT CLI veya Powershell üzerinden yüklüyorsanız sürüm olarak '3.*' olarak belirtin.
  * Azure Kaynak Yöneticisi dağıtım modeli VM'lerde VM dağıtım şablonuna ""autoUpgradeMinorVersion" ekleyin: doğru.
* LAD 3.0 için yeni/farklı bir depolama hesabı kullanın. LAD 2.3 ve LAD 3.0 arasında bir hesap paylaşımını zahmetli hale getiren birkaç küçük uyumsuzluk vardır:
  * LAD 3.0 farklı bir ada sahip bir tabloda syslog olayları depolar.
  * Ölçümler için `builtin` karşıSpecifier dizeleri LAD 3.0'da farklılık gösterir.

## <a name="protected-settings"></a>Korumalı ayarlar

Bu yapılandırma bilgileri kümesi, depolama kimlik bilgileri gibi genel görünümden korunması gereken hassas bilgiler içerir. Bu ayarlar uzantı tarafından şifrelenmiş olarak aktarılır ve saklanır.

```json
{
    "storageAccountName" : "the storage account to receive data",
    "storageAccountEndPoint": "the hostname suffix for the cloud for this account",
    "storageAccountSasToken": "SAS access token",
    "mdsdHttpProxy": "HTTP proxy settings",
    "sinksConfig": { ... }
}
```

Adı | Değer
---- | -----
storageAccountName | Verilerin uzantı tarafından yazıldığı depolama hesabının adı.
depolamaAccountEndPoint | (isteğe bağlı) Depolama hesabının bulunduğu bulutu tanımlayan bitiş noktası. Bu ayar yoksa, LAD varsayılan olarak Azure `https://core.windows.net`genel bulutuna, . Azure Almanya, Azure Kamu veya Azure Çin'de bir depolama hesabı kullanmak için bu değeri buna göre ayarlayın.
depolamaAccountSasToken | Blob ve Tablo hizmetleri için bir`ss='bt'` [Hesap SAS belirteci](https://azure.microsoft.com/blog/sas-update-account-sas-now-supports-all-storage-services/) ( ), ekleme,`srt='co'`oluşturma, listeleme, güncelleme`sp='acluw'`ve yazma izinleri (). Önde *not* gelen soru işaretini (?) eklemeyin.
mdsdHttpProxy | (isteğe bağlı) Uzantın belirtilen depolama hesabına ve bitiş noktasına bağlanmasını sağlamak için HTTP proxy bilgileri gereklidir.
lavaboconfig | (isteğe bağlı) Ölçümlerin ve olayların teslim edilebildiği alternatif hedeflerin ayrıntıları. Uzantı tarafından desteklenen her veri lavabonun özel ayrıntıları, izleyen bölümlerde ele alınmıştır.

Kaynak Yöneticisi şablonu içinde bir SAS belirteci almak için **listAccountSas** işlevini kullanın. Örnek şablon için [bkz.](../../azure-resource-manager/templates/template-functions-resource.md#list-example)

Azure portalı üzerinden gerekli SAS belirtecinizi kolayca oluşturabilirsiniz.

1. Uzantın yazmasını istediğiniz genel amaçlı depolama hesabını seçin
1. Sol menünün Ayarlar kısmından "Paylaşılan erişim imzası" seçeneğini belirleyin
1. Daha önce açıklandığı gibi uygun bölümleri yapma
1. "SAS oluştur" düğmesini tıklatın.

![image](./media/diagnostics-linux/make_sas.png)

Oluşturulan SAS'ı storageAccountSasToken alanına kopyalayın; satır başlarını kaldırın soru işareti ("?").

### <a name="sinksconfig"></a>lavaboconfig

```json
"sinksConfig": {
    "sink": [
        {
            "name": "sinkname",
            "type": "sinktype",
            ...
        },
        ...
    ]
},
```

Bu isteğe bağlı bölüm, uzantın topladığı bilgileri gönderdiği ek hedefleri tanımlar. "Lavabo" dizisi, her ek veri lavabosu için bir nesne içerir. "Tür" özniteliği nesnedeki diğer öznitelikleri belirler.

Öğe | Değer
------- | -----
ad | Uzantı yapılandırmasında başka bir yerde bu lavabo başvurmak için kullanılan bir dize.
type | Tanımlanan lavabo türü. Bu tür durumlarda diğer değerleri (varsa) belirler.

Linux Diagnostic Extension sürümü 3.0 iki lavabo türünü destekler: EventHub ve JsonBlob.

#### <a name="the-eventhub-sink"></a>EventHub lavabosu

```json
"sink": [
    {
        "name": "sinkname",
        "type": "EventHub",
        "sasURL": "https SAS URL"
    },
    ...
]
```

"SasURL" girişi, verilerin yayımlanması gereken Olay Hub'ı için SAS belirteci de dahil olmak üzere tam URL'yi içerir. LAD, Gönder talebini etkinleştiren bir sas adlandırma ilkesi gerektirir. Bir örnek:

* Olay Hub'ları ad alanı oluşturma adlı`contosohub`
* Ad alanında bir Olay Hub'ı oluşturma`syslogmsgs`
* Gönderme talebini etkinleştiren Event Hub'ında `writer` Paylaşılan erişim ilkesi oluşturma

1 Ocak 2018'de UTC gece yarısına kadar bir SAS iyi oluşturduysanız, sasURL değeri şu olabilir:

```https
https://contosohub.servicebus.windows.net/syslogmsgs?sr=contosohub.servicebus.windows.net%2fsyslogmsgs&sig=xxxxxxxxxxxxxxxxxxxxxxxxx&se=1514764800&skn=writer
```

Olay Hub'ları için SAS belirteçleri hakkında daha fazla bilgi oluşturma ve alma hakkında daha fazla bilgi için [bu web sayfasına](https://docs.microsoft.com/rest/api/eventhub/generate-sas-token#powershell)bakın.

#### <a name="the-jsonblob-sink"></a>JsonBlob lavabosu

```json
"sink": [
    {
        "name": "sinkname",
        "type": "JsonBlob"
    },
    ...
]
```

JsonBlob lavaboya yönlendirilen veriler Azure depolama alanında lekeler halinde depolanır. LAD her örneği her lavabo adı için her saat bir damla oluşturur. Her blob her zaman bir nesnenin sözdizimi geçerli JSON dizi içerir. Diziye atomik olarak yeni girişler eklenir. Lekeler lavabo ile aynı adı taşıyan bir kapta saklanır. Blob kapsayıcı adları için Azure depolama kuralları JsonBlob lavabolarının adlarına uygulanır: 3 ile 63 küçük harfli alfasayısal ASCII karakterleri veya tireleri.

## <a name="public-settings"></a>Genel ayarlar

Bu yapı, uzantı tarafından toplanan bilgileri denetleyen çeşitli ayar blokları içerir. Her ayar isteğe bağlıdır. Belirtirseniz, `ladCfg`ayrıca belirtmeniz `StorageAccount`gerekir.

```json
{
    "ladCfg":  { ... },
    "perfCfg": { ... },
    "fileLogs": { ... },
    "StorageAccount": "the storage account to receive data",
    "mdsdHttpProxy" : ""
}
```

Öğe | Değer
------- | -----
StorageAccount | Verilerin uzantı tarafından yazıldığı depolama hesabının adı. [Korumalı ayarlarda](#protected-settings)belirtilen adla aynı olmalıdır.
mdsdHttpProxy | (isteğe bağlı) [Korumalı ayarlardaki](#protected-settings)yle aynı. Genel değer, ayarlanırsa, özel değer tarafından geçersiz kılınmış olur. [Korumalı ayarlara](#protected-settings)parola gibi gizli içeren proxy ayarlarını yerleştirin.

Kalan öğeler aşağıdaki bölümlerde ayrıntılı olarak açıklanmıştır.

### <a name="ladcfg"></a>ladCfg

```json
"ladCfg": {
    "diagnosticMonitorConfiguration": {
        "eventVolume": "Medium",
        "metrics": { ... },
        "performanceCounters": { ... },
        "syslogEvents": { ... }
    },
    "sampleRateInSeconds": 15
}
```

Bu isteğe bağlı yapı, Azure Ölçümleri hizmetine ve diğer veri lavabolarına teslim için ölçümlerin ve günlüklerin toplanmasını denetler. Her ikisini `performanceCounters` veya `syslogEvents` her ikisini de belirtmelisiniz. Yapıyı `metrics` belirtmeniz gerekir.

Öğe | Değer
------- | -----
eventVolume | (isteğe bağlı) Depolama tablosu içinde oluşturulan bölüm sayısını denetler. Biri `"Large"` `"Medium"`olmalı, ya `"Small"`da . Belirtilmemişse, varsayılan `"Medium"`değer .
sampleRateInSeconds | (isteğe bağlı) Ham (unaggregated) ölçümlerin toplanması arasındaki varsayılan aralık. Desteklenen en küçük örneklem hızı 15 saniyedir. Belirtilmemişse, varsayılan `15`değer .

#### <a name="metrics"></a>metrics

```json
"metrics": {
    "resourceId": "/subscriptions/...",
    "metricAggregation" : [
        { "scheduledTransferPeriod" : "PT1H" },
        { "scheduledTransferPeriod" : "PT5M" }
    ]
}
```

Öğe | Değer
------- | -----
resourceId | VM'nin veya VM'nin ait olduğu sanal makine ölçeğinin Azure Kaynak Yöneticisi kaynak kimliği. Yapılandırmada herhangi bir JsonBlob lavabosu kullanılıyorsa bu ayar da belirtilmelidir.
scheduledTransferPeriod | Toplam ölçümlerin hesaplanıp Azure Ölçümleri'ne aktarılmak üzere, IS 8601 zaman aralığı olarak ifade edilme sıklığı. En küçük aktarım süresi 60 saniye, yani PT1M'dir. En az bir zamanlanmış Transfer Dönemi belirtmeniz gerekir.

PerformanceCounters bölümünde belirtilen ölçümlerin örnekleri her 15 saniyede bir veya sayaç için açıkça tanımlanan örnek hızında toplanır. Birden çok zamanlanmışTransferPeriod frekansı görünürse (örnekte olduğu gibi), her toplama bağımsız olarak hesaplanır.

#### <a name="performancecounters"></a>performansSayaçları

```json
"performanceCounters": {
    "sinks": "",
    "performanceCounterConfiguration": [
        {
            "type": "builtin",
            "class": "Processor",
            "counter": "PercentIdleTime",
            "counterSpecifier": "/builtin/Processor/PercentIdleTime",
            "condition": "IsAggregate=TRUE",
            "sampleRate": "PT15S",
            "unit": "Percent",
            "annotation": [
                {
                    "displayName" : "Aggregate CPU %idle time",
                    "locale" : "en-us"
                }
            ]
        }
    ]
}
```

Bu isteğe bağlı bölüm, ölçümlerin toplanmasını denetler. Ham numuneler, bu değerleri üretmek için her [zamanlanan Transfer Dönemi](#metrics) için toplanır:

* ortalama
* minimum
* maksimum
* son toplanan değer
* toplam ı hesaplamak için kullanılan ham numune lerin sayısı

Öğe | Değer
------- | -----
Lavabo | (isteğe bağlı) LAD'nin toplu metrik sonuçlar gönderdiği lavaboların adlarının virgülle ayrılmış listesi. Toplanan tüm ölçümler listelenen her lavaboya yayınlanır. [LavabolarConfig](#sinksconfig)bakın . Örnek: `"EHsink1, myjsonsink"`.
type | Metrin gerçek sağlayıcısını tanımlar.
sınıf | "Sayaç" ile birlikte, sağlayıcının ad alanında belirli bir metrik tanımlar.
counter | "Sınıf" ile birlikte, sağlayıcının ad alanında belirli bir metrik tanımlar.
sayaçSpecifier | Azure Ölçümleri ad alanında belirli bir ölçüt tanımlar.
Durum | (isteğe bağlı) Ölçümün uygulandığı nesnenin belirli bir örneğini seçer veya o nesnenin tüm örneklerinde toplamayı seçer. Daha fazla bilgi `builtin` için metrik tanımlarına bakın.
sampleRate | Bu metrik için ham numunelerin toplanma hızını belirleyen IS 8601 aralığı. Ayarlanmazsa, toplama aralığı [sampleRateInSeconds](#ladcfg)değerine göre ayarlanır. Desteklenen en kısa örneklem hızı 15 saniyedir (PT15S).
unit | Bu dizeleri olmalıdır: "Count", "Bayt", "Saniye", "Yüzde", "CountPerSecond", "BytesPerSecond", "Millisaniye". Metrik için birimi tanımlar. Toplanan verilerin tüketicileri toplanan veri değerlerinin bu birime uygun olmasını bekler. LAD bu alanı yok sayar.
displayName | Bu verilere Azure Ölçümleri'nde eklenecek etiket (ilişkili yerel ayar tarafından belirtilen dilde). LAD bu alanı yok sayar.

Sayaç Specifier rasgele bir tanımlayıcıdır. Azure portal grafik ve uyarı özelliği gibi ölçümlerin tüketicileri, bir metriği veya bir metriyi tanımlayan "anahtar" olarak karşı Specifier'i kullanır. Ölçümler `builtin` için, `/builtin/`'ile başlayan karşıSpecifier değerlerini kullanmanızı öneririz. Bir metnin belirli bir örneğini topluyorsanız, örneğin tanımlayıcısını sayaç Specifier değerine eklemenizi öneririz. Bazı örnekler:

* `/builtin/Processor/PercentIdleTime`- Tüm vCPUs'larda ortalama boşta kalma süresi
* `/builtin/Disk/FreeSpace(/mnt)`- /mnt dosya sistemi için boş alan
* `/builtin/Disk/FreeSpace`- Tüm monte edilmiş dosya sistemlerinde ortalama boş alan

Ne LAD ne de Azure portalı, karşı Specifier değerinin herhangi bir desenle eşleşmesini beklemiyor. CounterSpecifier değerlerini oluşturma şekliniz konusunda tutarlı olun.

Belirttiğiniz `performanceCounters`zaman, LAD her zaman Azure depolama bir tabloya veri yazar. Aynı verileri JSON blobs ve/veya Olay Hub'larına yazabilirsiniz, ancak verileri tabloya depolamayı devre dışı kalamazsınız. Aynı depolama hesabı adı ve bitiş noktası kullanmak üzere yapılandırılan tanılama uzantısı tüm örnekleri, ölçümlerini ve günlüklerini aynı tabloya ekler. Aynı tablo bölümüne çok fazla VM yazıyorsa, Azure bu bölüme yazmaları azaltabilir. OlayHacim ayarı girişlerin 1 (Küçük), 10 (Orta) veya 100 (Büyük) farklı bölüme yayılmasına neden olur. Genellikle, "Orta" trafiğin daraltılmış olmadığından emin olmak için yeterlidir. Azure portalının Azure Ölçümleri özelliği, grafik oluşturmak veya uyarıları tetiklemek için bu tablodaki verileri kullanır. Tablo adı bu dizeleri biraraya getirerek:

* `WADMetrics`
* Tabloda depolanan toplanan değerler için "zamanlanmış Transfer Dönemi"
* `P10DV2S`
* Her 10 günde bir değişen "YYYYMMDD" şeklinde bir tarih

Örnekler `WADMetricsPT1HP10DV2S20170410` şunlardır ve `WADMetricsPT1MP10DV2S20170609`.

#### <a name="syslogevents"></a>syslogEtkinlikler

```json
"syslogEvents": {
    "sinks": "",
    "syslogEventConfiguration": {
        "facilityName1": "minSeverity",
        "facilityName2": "minSeverity",
        ...
    }
}
```

Bu isteğe bağlı bölüm, syslog'daki günlük olaylarının toplanmasını denetler. Bölüm atlanırsa, syslog olayları hiç yakalanmaz.

SyslogEventConfiguration koleksiyonu ilgi her syslog tesisi için bir giriş vardır. MinSeverity belirli bir tesis için "YOK" ise veya bu tesis öğede hiç görünmüyorsa, o tesisten hiçbir olay yakalanır.

Öğe | Değer
------- | -----
Lavabo | Tek tek günlük olaylarının yayımlandığı lavaboların adlarının virgülle ayrılmış bir listesi. SyslogEventConfiguration'daki kısıtlamalarla eşleşen tüm günlük olayları listelenen her lavaboda yayınlanır. Örnek: "EHforsyslog"
tesisAdı | Bir sislog tesis adı (örneğin\_"LOG USER" veya "LOG\_LOCAL0"). Tam liste için [syslog man sayfasının](http://man7.org/linux/man-pages/man3/syslog.3.html) "tesis" bölümüne bakın.
minSeverity | Sislog önem düzeyi ("LOG\_ERR" veya "LOG\_INFO" gibi). Tam liste için [syslog man sayfasının](http://man7.org/linux/man-pages/man3/syslog.3.html) "seviye" bölümüne bakın. Uzantı, tesise belirtilen düzeyde veya üstünde gönderilen olayları yakalar.

Belirttiğiniz `syslogEvents`zaman, LAD her zaman Azure depolama bir tabloya veri yazar. Aynı verileri JSON blobs ve/veya Olay Hub'larına yazabilirsiniz, ancak verileri tabloya depolamayı devre dışı kalamazsınız. Bu tablonun bölümleme davranışı, '' `performanceCounters`için açıklananla aynıdır. Tablo adı bu dizeleri biraraya getirerek:

* `LinuxSyslog`
* Her 10 günde bir değişen "YYYYMMDD" şeklinde bir tarih

Örnekler `LinuxSyslog20170410` şunlardır ve `LinuxSyslog20170609`.

### <a name="perfcfg"></a>perfCfg

Bu isteğe bağlı bölüm, rasgele [OMI](https://github.com/Microsoft/omi) sorgularının yürütülmesini denetler.

```json
"perfCfg": [
    {
        "namespace": "root/scx",
        "query": "SELECT PercentAvailableMemory, PercentUsedSwap FROM SCX_MemoryStatisticalInformation",
        "table": "LinuxOldMemory",
        "frequency": 300,
        "sinks": ""
    }
]
```

Öğe | Değer
------- | -----
ad alanı | (isteğe bağlı) Sorgunun yürütülmesi gereken OMI ad alanı. Belirtilmemişse, varsayılan değer [Sistem Merkezi Çapraz platform Sağlayıcıları](https://github.com/Microsoft/SCXcore)tarafından uygulanan "root/scx"tir.
sorgu | Yürütülecek OMI sorgusu.
tablo | (isteğe bağlı) Azure depolama tablosu, belirlenen depolama hesabında [(Bkz. Korumalı ayarlar).](#protected-settings)
frequency | (isteğe bağlı) Sorgunun yürütülmesi arasındaki saniye sayısı. Varsayılan değer 300 (5 dakika); minimum değer 15 saniyedir.
Lavabo | (isteğe bağlı) Ham örnek metrik sonuçlarının yayınlanması gereken ek lavaboların adlarının virgülle ayrılmış bir listesi. Bu ham örneklerin hiçbir toplama uzantısı veya Azure Ölçümleri tarafından hesaplanmaz.

"Tablo" veya "lavabolar" veya her ikisi belirtilmelidir.

### <a name="filelogs"></a>fileLoglar

Günlük dosyalarının yakalanmasını denetler. LAD, dosyaya yazıldığı nda yeni metin satırlarını yakalar ve bunları tablo satırlarına ve/veya belirtilen lavabolara (JsonBlob veya EventHub) yazar.

```json
"fileLogs": [
    {
        "file": "/var/log/mydaemonlog",
        "table": "MyDaemonEvents",
        "sinks": ""
    }
]
```

Öğe | Değer
------- | -----
 dosyası | İzlenecek ve ele geçirilecek günlük dosyasının tam yol adı. Yol adı tek bir dosya yı adlandırmalıdır; bir dizin adlandıramaz veya joker karakter içeremez.
tablo | (isteğe bağlı) Azure depolama tablosu, dosyanın "kuyruğundan" yeni satırların yazıldığı belirtilen belirtilen depolama hesabında (korumalı yapılandırmada belirtildiği gibi).
Lavabo | (isteğe bağlı) Günlük satırlarının gönderildiği ek lavaboların adlarının virgülle ayrılmış bir listesi.

"Tablo" veya "lavabolar" veya her ikisi belirtilmelidir.

## <a name="metrics-supported-by-the-builtin-provider"></a>Yerleşik sağlayıcı tarafından desteklenen ölçümler

Yerleşik metrik sağlayıcı, geniş bir kullanıcı kümesi için en ilginç ölçümler kaynağıdır. Bu ölçümler beş geniş sınıfa ayrılır:

* İşlemci
* Bellek
* Ağ
* Dosya Sistemi
* Disk

### <a name="builtin-metrics-for-the-processor-class"></a>İşlemci sınıfı için yerleşik ölçümler

Ölçümlerin İşlemci sınıfı VM'deki işlemci kullanımı hakkında bilgi sağlar. Yüzdeleri toplarken, sonuç tüm CPU'lar arasında ortalamadır. İki vCPU VM'de, bir vCPU %100 meşgul ve diğeri %100 boşta ysa, bildirilen PercentIdleTime 50 olurdu. Her vCPU aynı dönem için %50 meşgul sayılsaydı, bildirilen sonuç da 50 olacaktır. Dört vCPU VM, bir vCPU% 100 meşgul ve diğerleri boşta, bildirilen PercentIdleTime 75 olacaktır.

counter | Anlamı
------- | -------
YüzdeBoş Zaman | İşlemcilerin çekirdek boşta döngüyü çalıştırdığı toplama penceresi sırasındaki zaman yüzdesi
YüzdeİşlemciLik Süresi | Boşta olmayan bir iş parçacığı nın çalıştırılma süresi yüzdesi
YüzdeIoBekleme Süresi | IO işlemlerinin tamamlanmasını bekleyen zaman yüzdesi
YüzdeKesme Süresi | Donanım/yazılım kesintileri ve DPC'ler (ertelenmiş yordam çağrıları) yürütme süresi yüzdesi
YüzdeKullanıcı Zamanı | Toplama penceresi sırasında boşta olmayan zaman, normal öncelik te kullanıcıda harcanan zaman yüzdesi
YüzdeNiceTime | Boşta olmayan zaman, düşük (güzel) öncelik harcanan yüzdesi
YüzdeAyrıcalıklı Zaman | Boşta olmayan zaman, ayrıcalıklı (çekirdek) modunda harcanan yüzde

İlk dört sayaç %100'e kadar olmalıdır. Son üç sayaç da %100'e karşılık; yüzdeprocessortime, yüzdeiowaittime ve YüzdeInterruptTime toplamını bölerler.

Tüm işlemciler arasında toplanan tek bir `"condition": "IsAggregate=TRUE"`metrik elde etmek için. Dört vCPU VM'nin ikinci mantıksal işlemcisi gibi belirli bir işlemci `"condition": "Name=\\"1\\""`için metrik elde etmek için. Mantıksal işlemci numaraları aralıktadır. `[0..n-1]`

### <a name="builtin-metrics-for-the-memory-class"></a>Bellek sınıfı için yerleşik ölçümler

Ölçümlerin Bellek sınıfı bellek kullanımı, sayfalama ve değiştirme hakkında bilgi sağlar.

counter | Anlamı
------- | -------
Kullanılabilir Bellek | MiB'de mevcut fiziksel bellek
YüzdeKullanılabilir Bellek | Toplam belleğin yüzdesi olarak kullanılabilir fiziksel bellek
Kullanılmış Bellek | Kullanımda fiziksel bellek (MiB)
Kullanılan Bellek Yüzdesi | Toplam belleğin yüzdesi olarak kullanılan fiziksel bellek
PagesPersec | Toplam sayfalama (okuma/yazma)
PagesReadPersec | Destek deposundan okunan sayfalar (takas dosyası, program dosyası, eşlenen dosya, vb.)
SayfaYazılıPersec | Destek deposuna yazılan sayfalar (takas dosyası, eşlenen dosya, vb.)
AvailableSwap | Kullanılmayan takas alanı (MiB)
percentAvailableSwap | Toplam takas yüzdesi olarak kullanılmayan takas alanı
UsedSwap | Kullanım içi takas alanı (MiB)
yüzde Kullanılmış Takas | Toplam takas yüzdesi olarak kullanım içi takas alanı

Bu ölçüm sınıfının yalnızca tek bir örneği vardır. "Koşul" özniteliği yararlı ayarları vardır ve atlanmalıdır.

### <a name="builtin-metrics-for-the-network-class"></a>Ağ sınıfı için yerleşik ölçümler

Ölçümlerin Ağ sınıfı, önyüklemeden bu yana tek bir ağ arabirimlerindeki ağ etkinliği hakkında bilgi sağlar. LAD, ana bilgisayar ölçümlerinden alınabilen bant genişliği ölçümlerini göstermez.

counter | Anlamı
------- | -------
BaytTransmitted | Önyüklemeden beri gönderilen toplam bayt sayısı
BaytAlınan | Önyüklemeden bu yana alınan toplam bayt
Bytestotal | Önyüklemeden bu yana gönderilen veya alınan toplam bayt sayısı
Paketlerİletilensin | Önyüklemeden bu yana gönderilen toplam paketler
Alınan Paketler | Önyüklemeden bu yana alınan toplam paket sayısı
TotalRxHataları | Önyüklemeden bu yana alma hatası sayısı
TotalTxHatalar | Önyüklemeden bu yana iletim hatalarının sayısı
Toplam Çarpışmalar | Önyüklemeden bu yana ağ bağlantı noktaları tarafından bildirilen çakışan ların sayısı

 Bu sınıf örneklenmiş olsa da, LAD tüm ağ aygıtlarında toplanan Ağ ölçümlerini yakalamayı desteklemez. Eth0 gibi belirli bir arabirimin ölçümlerini elde `"condition": "InstanceID=\\"eth0\\""`etmek için.

### <a name="builtin-metrics-for-the-filesystem-class"></a>Filesystem sınıfı için yerleşik ölçümler

Ölçümlerin Filesystem sınıfı, dosya sistemi kullanımı hakkında bilgi sağlar. Mutlak ve yüzde değerleri, sıradan bir kullanıcıya (kök değil) görüntüleneceği için bildirilir.

counter | Anlamı
------- | -------
FreeSpace | Baytlarda kullanılabilir disk alanı
UsedSpace | Baytlarda kullanılan disk alanı
YüzdeFreeSpace | Boş alan yüzdesi
Yüzde UsedSpace | Yüzde kullanılan alan
YüzdeFreeInodes | Kullanılmayan inodes yüzdesi
YüzdeUsedInodes | Tüm dosya sistemlerinde özetlenen ayrılan (kullanımda) inode'lerin yüzdesi
BaytReadPerİkinci | Bayt saniyede okunur
BytesWrittenPerİkinci | Saniyede yazılan baytlar
BaytPerİkinci | Baytlar saniyede okunur veya yazılır
OkumaPersaniye | İşlemleri saniyede okuma
Yazma PerSecond | Saniyede yazma işlemleri
TransfersPerSecond | Saniyede okuma veya yazma işlemleri

Tüm dosya sistemlerinde toplanan değerler ayarlaelde `"condition": "IsAggregate=True"`edilebilir. "/mnt" gibi belirli bir monte edilmiş dosya sistemi için `"condition": 'Name="/mnt"'`değerler ayarlaelde edilebilir. 

**NOT**: JSON yerine Azure Portalı kullanıyorsanız, doğru koşul alanı formu Name='/mnt'

### <a name="builtin-metrics-for-the-disk-class"></a>Disk sınıfı için yerleşik ölçümler

Disk ölçümleri sınıfı, disk aygıtı kullanımı hakkında bilgi sağlar. Bu istatistikler tüm sürücü için geçerlidir. Bir aygıtta birden çok dosya sistemi varsa, bu aygıtın sayaçları etkin bir şekilde hepsinde toplanır.

counter | Anlamı
------- | -------
OkumaPersaniye | İşlemleri saniyede okuma
Yazma PerSecond | Saniyede yazma işlemleri
TransfersPerSecond | Saniyede toplam işlem
Ortalama Okuma Süresi | Okuma işlemi başına ortalama saniye
Ortalama Yazma Süresi | Yazma işlemi başına ortalama saniye
Ortalama Transfer Süresi | İşlem başına ortalama saniye
OrtalamaDiskQueueLength | Sıralanmış disk işlemlerinin ortalama sayısı
OkumaBytesPerSecond | Saniyede okunan bayt sayısı
YazmaBytesPerSecond | Saniyede yazılan bayt sayısı
BaytPerİkinci | Saniyede okunan veya yazılan bayt sayısı

Tüm diskler arasında toplanan değerler ayarlayarak `"condition": "IsAggregate=True"`elde edilebilir. Belirli bir aygıt için bilgi almak için (örneğin, /dev/sdf1), ayarlayın. `"condition": "Name=\\"/dev/sdf1\\""`

## <a name="installing-and-configuring-lad-30-via-cli"></a>CLI üzerinden LAD 3.0'ı yükleme ve yapılandırma

Korumalı ayarlarınızın PrivateConfig.json dosyasında ve ortak yapılandırma bilgilerinizin PublicConfig.json'da olduğunu varsayarsak, şu komutu çalıştırın:

```azurecli
az vm extension set *resource_group_name* *vm_name* LinuxDiagnostic Microsoft.Azure.Diagnostics '3.*' --private-config-path PrivateConfig.json --public-config-path PublicConfig.json
```

Komut, Azure CLI'nin Azure Kaynak Yönetimi modunu (kolu) kullandığınızı varsayar. Klasik dağıtım modeli (ASM) VM'leri için LAD'yi yapılandırmak için "asm" moduna`azure config mode asm`geçin ve komuttaki kaynak grubu adını atleyin. Daha fazla bilgi [için, çapraz platform CLI belgelerine](https://docs.microsoft.com/azure/xplat-cli-connect)bakın.

## <a name="an-example-lad-30-configuration"></a>Örnek lad 3.0 yapılandırması

Önceki tanımlara göre, burada bazı açıklama ile örnek LAD 3.0 uzantısı yapılandırması. Bu örneği servis talebinize uygulamak için kendi depolama hesabı adınızı, hesap SAS belirtecinizi ve EventHubs SAS belirteçlerinizi kullanmanız gerekir.

### <a name="privateconfigjson"></a>ÖzelConfig.json

Bu özel ayarları yapılandırmak:

* bir depolama hesabı
* eşleşen bir hesap SAS belirteci
* birkaç lavabo (JsonBlob veya SAS belirteçleri ile EventHubs)

```json
{
  "storageAccountName": "yourdiagstgacct",
  "storageAccountSasToken": "sv=xxxx-xx-xx&ss=bt&srt=co&sp=wlacu&st=yyyy-yy-yyT21%3A22%3A00Z&se=zzzz-zz-zzT21%3A22%3A00Z&sig=fake_signature",
  "sinksConfig": {
    "sink": [
      {
        "name": "SyslogJsonBlob",
        "type": "JsonBlob"
      },
      {
        "name": "FilelogJsonBlob",
        "type": "JsonBlob"
      },
      {
        "name": "LinuxCpuJsonBlob",
        "type": "JsonBlob"
      },
      {
        "name": "MyJsonMetricsBlob",
        "type": "JsonBlob"
      },
      {
        "name": "LinuxCpuEventHub",
        "type": "EventHub",
        "sasURL": "https://youreventhubnamespace.servicebus.windows.net/youreventhubpublisher?sr=https%3a%2f%2fyoureventhubnamespace.servicebus.windows.net%2fyoureventhubpublisher%2f&sig=fake_signature&se=1808096361&skn=yourehpolicy"
      },
      {
        "name": "MyMetricEventHub",
        "type": "EventHub",
        "sasURL": "https://youreventhubnamespace.servicebus.windows.net/youreventhubpublisher?sr=https%3a%2f%2fyoureventhubnamespace.servicebus.windows.net%2fyoureventhubpublisher%2f&sig=yourehpolicy&skn=yourehpolicy"
      },
      {
        "name": "LoggingEventHub",
        "type": "EventHub",
        "sasURL": "https://youreventhubnamespace.servicebus.windows.net/youreventhubpublisher?sr=https%3a%2f%2fyoureventhubnamespace.servicebus.windows.net%2fyoureventhubpublisher%2f&sig=yourehpolicy&se=1808096361&skn=yourehpolicy"
      }
    ]
  }
}
```

### <a name="publicconfigjson"></a>PublicConfig.json

Bu genel ayarlar LAD'nin:

* Yüzde-işlemci-zaman ve kullanılmış disk-alan ölçümlerini `WADMetrics*` tabloya yükleme
* Syslog tesisinden "kullanıcı" ve önem "bilgi" `LinuxSyslog*` tablosuna mesaj yükleme
* Ham OMI sorgu sonuçlarını (PercentProcessorTime ve PercentIdleTime) adlandırılmış `LinuxCPU` tabloya yükleyin
* Dosyadaki `/var/log/myladtestlog` eklenen satırları tabloya `MyLadTestLog` yükleme

Her durumda, veriler de yüklenir:

* Azure Blob depolama (konteyner adı JsonBlob lavaboda tanımlandığı gibi)
* EventHubs bitiş noktası (EventHubs lavaboda belirtildiği gibi)

```json
{
  "StorageAccount": "yourdiagstgacct",
  "ladCfg": {
    "sampleRateInSeconds": 15,
    "diagnosticMonitorConfiguration": {
      "performanceCounters": {
        "sinks": "MyMetricEventHub,MyJsonMetricsBlob",
        "performanceCounterConfiguration": [
          {
            "unit": "Percent",
            "type": "builtin",
            "counter": "PercentProcessorTime",
            "counterSpecifier": "/builtin/Processor/PercentProcessorTime",
            "annotation": [
              {
                "locale": "en-us",
                "displayName": "Aggregate CPU %utilization"
              }
            ],
            "condition": "IsAggregate=TRUE",
            "class": "Processor"
          },
          {
            "unit": "Bytes",
            "type": "builtin",
            "counter": "UsedSpace",
            "counterSpecifier": "/builtin/FileSystem/UsedSpace",
            "annotation": [
              {
                "locale": "en-us",
                "displayName": "Used disk space on /"
              }
            ],
            "condition": "Name=\"/\"",
            "class": "Filesystem"
          }
        ]
      },
      "metrics": {
        "metricAggregation": [
          {
            "scheduledTransferPeriod": "PT1H"
          },
          {
            "scheduledTransferPeriod": "PT1M"
          }
        ],
        "resourceId": "/subscriptions/your_azure_subscription_id/resourceGroups/your_resource_group_name/providers/Microsoft.Compute/virtualMachines/your_vm_name"
      },
      "eventVolume": "Large",
      "syslogEvents": {
        "sinks": "SyslogJsonBlob,LoggingEventHub",
        "syslogEventConfiguration": {
          "LOG_USER": "LOG_INFO"
        }
      }
    }
  },
  "perfCfg": [
    {
      "query": "SELECT PercentProcessorTime, PercentIdleTime FROM SCX_ProcessorStatisticalInformation WHERE Name='_TOTAL'",
      "table": "LinuxCpu",
      "frequency": 60,
      "sinks": "LinuxCpuJsonBlob,LinuxCpuEventHub"
    }
  ],
  "fileLogs": [
    {
      "file": "/var/log/myladtestlog",
      "table": "MyLadTestLog",
      "sinks": "FilelogJsonBlob,LoggingEventHub"
    }
  ]
}
```

`resourceId` Yapılandırmadaki vm veya sanal makine ölçeği kümesi ile eşleşmelidir.

* Azure platform ölçümleri grafik ve uyarı üzerinde çalıştığınız VM'nin kaynak Kimliğini bilir. ResourceId arama anahtarını kullanarak VM'nizin verilerini bulmayı bekler.
* Azure otomatik ölçeklendirmesini kullanıyorsanız, otomatik ölçek yapılandırmasındaki resourceId LAD tarafından kullanılan kaynak Kimliği ile eşleşmelidir.
* ResourceId LAD tarafından yazılmış JsonBlobs isimleri içine inşa edilmiştir.

## <a name="view-your-data"></a>Verilerinizi görüntüleme

Performans verilerini görüntülemek veya uyarıları ayarlamak için Azure portalını kullanın:

![image](./media/diagnostics-linux/graph_metrics.png)

Veriler `performanceCounters` her zaman bir Azure Depolama tablosunda depolanır. Azure Depolama API'leri birçok dil ve platformda kullanılabilir.

JsonBlob lavabolarına gönderilen [veriler, Korumalı ayarlarda](#protected-settings)adı verilen depolama hesabında lekeler halinde depolanır. Blob verilerini herhangi bir Azure Blob Depolama API'sini kullanarak tüketebilirsiniz.

Ayrıca, Azure Depolama'daki verilere erişmek için bu kullanıcı aracı araçlarını kullanabilirsiniz:

* Visual Studio Server Explorer.
* [Microsoft Azure Depolama Gezgini](https://azurestorageexplorer.codeplex.com/ "Azure Storage Gezgini").

Microsoft Azure Depolama Gezgini oturumunun bu görüntüsü, test VM'de doğru yapılandırılmış lad 3.0 uzantısından oluşturulan Azure Depolama tablolarını ve kapsayıcılarını gösterir. Görüntü, [lad 3.0 örneği yapılandırmasıyla](#an-example-lad-30-configuration)tam olarak eşleşmiyor.

![image](./media/diagnostics-linux/stg_explorer.png)

Bir EventHubs bitiş noktasında yayınlanan iletileri nasıl tüketiriz öğrenmek için ilgili [EventHubs belgelerine](../../event-hubs/event-hubs-what-is-event-hubs.md) bakın.

## <a name="next-steps"></a>Sonraki adımlar

* Topladığınız ölçümler için [Azure Monitörü'nde](../../monitoring-and-diagnostics/insights-alerts-portal.md) metrik uyarılar oluşturun.
* Ölçümleriniz için [izleme grafikleri](../../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md) oluşturun.
* Otomatik ölçeklendirmeyi denetlemek için ölçümlerinizi kullanarak [sanal makine ölçeği kümesioluşturmayı](../linux/tutorial-create-vmss.md) öğrenin.
