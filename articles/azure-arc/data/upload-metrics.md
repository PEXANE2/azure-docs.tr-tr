---
title: Ölçümleri Azure Izleyici 'ye yükleyin
description: Azure Arc etkin veri Hizmetleri ölçümlerini Azure Izleyici 'ye yükleme
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: twright-msft
ms.author: twright
ms.reviewer: mikeray
ms.date: 09/22/2020
ms.topic: how-to
zone_pivot_groups: client-operating-system-macos-and-linux-windows-powershell
ms.openlocfilehash: d7c611f1cdb5e3294e38f87c0534003813e50388
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "100575685"
---
# <a name="upload-metrics-to-azure-monitor"></a>Ölçümleri Azure Izleyici 'ye yükleyin

Düzenli olarak, izleme ölçümlerini dışa aktarabilir ve ardından bunları Azure 'a yükleyebilirsiniz. Verilerin dışarı ve karşıya yüklenmesi Ayrıca, Azure 'daki veri denetleyicisi, SQL yönetilen örnek ve PostgreSQL hiper ölçek sunucu grubu kaynaklarını oluşturur ve güncelleştirir.

> [!NOTE] 
> Önizleme dönemi boyunca, Azure Arc etkin veri hizmetlerini kullanma maliyeti yoktur.

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="prerequisites"></a>Önkoşullar

Devam etmeden önce, gerekli hizmet sorumlusunu oluşturup uygun bir role atadığınızdan emin olun. Ayrıntılar için bkz.
* [Hizmet sorumlusu oluşturun](upload-metrics-and-logs-to-azure-monitor.md#create-service-principal).
* [Hizmet sorumlusuna roller atama](upload-metrics-and-logs-to-azure-monitor.md#assign-roles-to-the-service-principal)

## <a name="upload-metrics"></a>Ölçümleri karşıya yükleme

Azure Arc veri Hizmetleri sayesinde ölçümleri toplayıp analiz edebilir, uyarılar oluşturabilir, bildirimler gönderebilir ya da otomatikleştirilmiş eylemleri tetikleyebilmeniz için ölçülerinizi Azure Izleyici 'ye yükleyebilirsiniz. 

Verilerinizi Azure Izleyici 'ye göndermek aynı zamanda ölçüm verilerini yerinde ve çok büyük ölçekte depolamanıza olanak tanıyarak, gelişmiş analizler için verilerin uzun süreli depolanmasını sağlar.

Azure Arc veri Hizmetleri 'ne sahip birden fazla siteniz varsa, Azure Izleyici 'yi siteler genelinde tüm günlüklerinizi ve ölçümlerinizi toplamak için merkezi bir konum olarak kullanabilirsiniz.

## <a name="set-final-environment-variables-and-confirm"></a>Son ortam değişkenlerini ayarlama ve onaylama

Bir ortam değişkeninde SPN yetkilisi URL 'sini ayarlayın:

::: zone pivot="client-operating-system-windows-command"

```console
SET SPN_AUTHORITY=https://login.microsoftonline.com
```

::: zone-end

::: zone pivot="client-operating-system-powershell"

```PowerShell
$Env:SPN_AUTHORITY='https://login.microsoftonline.com'
```

::: zone-end

::: zone pivot="client-operating-system-macos-and-linux"

```console
export SPN_AUTHORITY='https://login.microsoftonline.com'
```

::: zone-end

İsterseniz gerekli tüm ortam değişkenlerinin ayarlandığından emin olmak için işaretleyin:


::: zone pivot="client-operating-system-powershell"

```PowerShell
$Env:SPN_TENANT_ID
$Env:SPN_CLIENT_ID
$Env:SPN_CLIENT_SECRET
$Env:SPN_AUTHORITY
```


::: zone-end

::: zone pivot="client-operating-system-macos-and-linux"

```console
echo $SPN_TENANT_ID
echo $SPN_CLIENT_ID
echo $SPN_CLIENT_SECRET
echo $SPN_AUTHORITY
```

::: zone-end

::: zone pivot="client-operating-system-windows-command"

```console
echo %SPN_TENANT_ID%
echo %SPN_CLIENT_ID%
echo %SPN_CLIENT_SECRET%
echo %SPN_AUTHORITY%
```

::: zone-end

## <a name="upload-metrics-to-azure-monitor"></a>Ölçümleri Azure Izleyici 'ye yükleyin

Azure Arc etkin SQL yönetilen örnekleriniz ve Azure Arc etkin PostgreSQL hiper ölçek sunucu grupları için ölçümleri karşıya yüklemek için aşağıdaki CLı komutlarını çalıştırın:

1. İle veri denetleyicisinde oturum açın `azdata` .
 
1. Tüm ölçümleri belirtilen dosyaya aktar:

   ```console
   azdata arc dc export --type metrics --path metrics.json
   ```

2. Ölçümleri Azure izleyici 'ye yükleyin:

   ```console
   azdata arc dc upload --path metrics.json
   ```

   >[!NOTE]
   >İlk karşıya yükleme için Azure Arc etkin veri örnekleri oluşturulduktan en az 30 dakika bekleyin.
   >
   >`upload` `export` Azure izleyici, yalnızca son 30 dakikalık ölçümleri kabul ettiğinden, ölçümlerin doğru bir şekilde ayrıltığından emin olun. [Daha fazla bilgi edinin](../../azure-monitor/essentials/metrics-store-custom-rest-api.md#troubleshooting).


Dışarı aktarma sırasında "ölçümleri almak için hata" belirten bir hata görürseniz, aşağıdaki komutu çalıştırarak veri toplamanın olarak ayarlanmış olup olmadığını denetleyin `true` :

```console
azdata arc dc config show
```

"Güvenlik bölümü" nün altına bakın

```output
 "security": {
      "allowDumps": true,
      "allowNodeMetricsCollection": true,
      "allowPodMetricsCollection": true,
      "allowRunAsRoot": false
    },
```

`allowNodeMetricsCollection`Ve `allowPodMetricsCollection` özelliklerinin olarak ayarlandığını doğrulayın `true` .

## <a name="view-the-metrics-in-the-portal"></a>Ölçümleri portalda görüntüleme

Ölçümleriniz karşıya yüklendikten sonra Azure portal görüntüleyebilirsiniz.
> [!NOTE]
> Portalda ölçümleri görüntüleyebilmeniz için karşıya yüklenen verilerin işlenmesi birkaç dakika sürebileceğini lütfen unutmayın.


Portalda ölçümlerinizi görüntülemek için bu bağlantıyı kullanarak portalı açın: <https://portal.azure.com> sonra, arama çubuğunda veritabanı örneğinizi adıyla arayın:

Genel Bakış sayfasında CPU kullanımı ' nı görüntüleyebilir veya daha ayrıntılı ölçümler istiyorsanız sol Gezinti panelinden ölçümler ' e tıklayabilirsiniz.

Ölçüm ad alanı olarak SQL Server 'ı seçin:

Görselleştirmek istediğiniz ölçümü seçin (birden çok öğesini de seçebilirsiniz):

Sıklığı son 30 dakika olarak değiştirin:

> [!NOTE]
> Yalnızca son 30 dakikalık ölçümleri karşıya yükleyebilirsiniz. Azure Izleyici, 30 dakikadan daha eski ölçümleri reddeder.

## <a name="automating-uploads-optional"></a>Karşıya Yüklemeleri Otomatikleştirme (isteğe bağlı)

Ölçümleri ve günlükleri Zamanlanmış olarak yüklemek isterseniz, bir komut dosyası oluşturup birkaç dakikada bir Zamanlayıcı üzerinde çalıştırabilirsiniz. Bir Linux kabuğu betiği kullanarak karşıya yüklemeyi otomatikleştirme örneği aşağıda verilmiştir.

En sevdiğiniz metin/kod düzenleyicide, dosyaya aşağıdaki betiği ekleyin ve. sh (Linux/Mac) veya. cmd,. bat,. ps1 gibi bir betik yürütülebilir dosyası olarak kaydedin.

```console
azdata arc dc export --type metrics --path metrics.json --force
azdata arc dc upload --path metrics.json
```

Betik dosyasını çalıştırılabilir hale getirme

```console
chmod +x myuploadscript.sh
```

Betiği her 20 dakikada bir çalıştırın:

```console
watch -n 1200 ./myuploadscript.sh
```

Ayrıca, cron veya Windows Görev Zamanlayıcı gibi bir iş zamanlayıcısını ya da ansız, Pupevcil hayvan veya Chef gibi bir Orchestrator 'ı kullanabilirsiniz.

## <a name="general-guidance-on-exporting-and-uploading-usage-metrics"></a>Kullanım, ölçüm verme ve karşıya yükleme hakkında genel yönergeler

Azure Arc etkin veri Hizmetleri üzerinde oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemleri faturalandırma ve izleme amacıyla günlüğe kaydedilir. Bu CRUD işlemlerini izleyen ve tüketimi uygun şekilde hesaplayan arka plan hizmetleri mevcuttur. Kullanım veya tüketimin gerçek hesaplaması, zamanlanan bir şekilde gerçekleşir ve arka planda yapılır. 

Önizleme süresince bu işlem gecelik olur. Genel rehberlik, kullanımı günde yalnızca bir kez karşıya yüklemek içindir. Kullanım bilgileri aktarıldığında ve aynı 24 saatlik süre içinde birden çok kez karşıya yüklendiğinde, yalnızca kaynak envanteri Azure portal ' de güncelleştirilir ancak kaynak kullanımı değildir.

Azure izleyici, ölçümleri karşıya yüklemek için yalnızca son 30 dakikalık verileri kabul eder ([daha fazla bilgi edinin](../../azure-monitor/essentials/metrics-store-custom-rest-api.md#troubleshooting)). Ölçüm Yükleme Kılavuzu, verileri dışa aktarma dosyası oluşturulduktan hemen sonra, Azure portal ' de tüm veri kümesini görüntüleyebilmeniz için, ölçümleri karşıya yüklemedir. Örneğin, ölçümleri 2:00 PM tarihinde ve 2:50 PM 'de karşıya yükle komutunu çalıştırdıysanız. Azure Izleyici yalnızca son 30 dakikalık verileri kabul ettiğinden portalda herhangi bir veri göremeyebilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

[Günlükleri Azure Izleyici 'ye yükleme](upload-logs.md)

[Kullanım verilerini, ölçümleri ve günlükleri Azure Izleyici 'ye yükleme](upload-usage-data.md)

[Faturalama verilerini Azure 'a yükleyin ve Azure portal görüntüleyin](view-billing-data-in-azure.md)

[Azure Arc Data Controller kaynağını Azure portal görüntüle](view-data-controller-in-azure-portal.md)
