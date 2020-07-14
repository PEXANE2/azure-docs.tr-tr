---
title: Kapsayıcılara ve bloblara anonim genel okuma erişimini engelleyin
titleSuffix: Azure Storage
description: ''
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 07/06/2020
ms.author: tamram
ms.reviewer: fryu
ms.openlocfilehash: 90d7cd65bbc07524391f34fe0efce2b044664cef
ms.sourcegitcommit: 3541c9cae8a12bdf457f1383e3557eb85a9b3187
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86209931"
---
# <a name="prevent-anonymous-public-read-access-to-containers-and-blobs"></a>Kapsayıcılara ve bloblara anonim genel okuma erişimini engelleyin

Azure depolama 'daki kapsayıcılara ve bloblara anonim genel okuma erişimi, verileri paylaşmanın kolay bir yoludur, ancak bir güvenlik riski de sunabilir. Anonim erişim bozacağından ' i etkinleştirmek ve verilerinize anonim erişimin nasıl değerlendirileceğini anlamak önemlidir. İşlemsel karmaşıklık, insan hatası veya genel olarak erişilebilen verilere karşı kötü amaçlı saldırı maliyetli veri ihlallerine neden olabilir. Microsoft, yalnızca uygulama senaryonuz için gerektiğinde anonim erişimi etkinleştirmenizi önerir.

Varsayılan olarak, bir depolama hesabı, kullanıcıya kapsayıcılara ve bloblara genel erişimi yapılandırmak için uygun izinlere sahip bir kullanıcı sağlar. Bu işlevi depolama hesabı düzeyinde devre dışı bırakabilirsiniz. böylece, hesaptaki kapsayıcılar ve Bloblar genel erişim için yapılandırılamaz.

Bu makalede, bir depolama hesabına karşı anonim isteklerin nasıl çözümleneceği ve tüm depolama hesabı veya tek bir kapsayıcı için anonim erişimin nasıl engelleneceği açıklanmaktadır.

## <a name="detect-anonymous-requests-from-client-applications"></a>İstemci uygulamalarından gelen Anonim istekleri Algıla

Bir depolama hesabı için genel okuma erişimini devre dışı bıraktığınızda, istekleri, genel erişim için yapılandırılmış olan kapsayıcılara ve bloblara reddetmeniz önerilir. Bir depolama hesabı için genel erişimin devre dışı bırakılması, bu depolama hesabındaki tüm kapsayıcılar için genel erişim ayarlarını geçersiz kılar. Depolama hesabı için genel erişim devre dışı bırakıldığında, bu hesaba yönelik ileride yapılacak anonim istekler başarısız olur.

Genel erişimin devre dışı bırakılmasını, istemci uygulamalarını nasıl etkileyebileceğini anlamak için, Microsoft bu hesap için günlüğe kaydetmeyi ve ölçümleri etkinleştirmenizi ve zaman aralığı boyunca anonim isteklerin desenlerini çözümlemeyi önerir. Depolama hesabına yönelik anonim isteklerin sayısını öğrenmek için ölçümleri kullanın ve hangi kapsayıcıların adsız olarak erişildiğini öğrenmek için günlükleri kullanın.

### <a name="monitor-anonymous-requests-with-metrics-explorer"></a>Ölçüm Gezgini ile anonim istekleri izleme

Bir depolama hesabına Anonim istekleri izlemek için Azure portal Azure Ölçüm Gezgini kullanın. Ölçüm Gezgini hakkında daha fazla bilgi için bkz. [Azure Ölçüm Gezgini](../../azure-monitor/platform/metrics-getting-started.md)kullanmaya başlama.

Anonim istekleri izleyen bir ölçüm oluşturmak için aşağıdaki adımları izleyin:

1. Azure portalda depolama hesabınıza gidin. **İzleme** bölümünde **ölçümler**' i seçin.
1. **Ölçüm ekle** seçeneğini belirleyin. **Ölçüm** iletişim kutusunda aşağıdaki değerleri belirtin:
    1. Kapsam alanını depolama hesabının adı olarak ayarlayın.
    1. **Ölçüm ad alanını** *BLOB*olarak ayarlayın. Bu ölçüm, istekleri yalnızca blob depolamaya göre rapor eder.
    1. **Ölçüm** alanını *işlemler*olarak ayarlayın.
    1. **Toplama** alanını *Sum*olarak ayarlayın.

    Yeni ölçüm, belirli bir zaman aralığı boyunca blob depolamaya göre işlem sayısı toplamını görüntüler. Ortaya çıkan ölçüm, aşağıdaki görüntüde gösterildiği gibi görünür:

    :::image type="content" source="media/anonymous-read-access-prevent/configure-metric-blob-transactions.png" alt-text="Blob işlemlerini toplamak için ölçümün nasıl yapılandırılacağını gösteren ekran görüntüsü":::

1. Ardından, anonim istekler için ölçüm üzerinde bir filtre oluşturmak üzere **Filtre Ekle** düğmesini seçin.
1. **Filtre** iletişim kutusunda aşağıdaki değerleri belirtin:
    1. **Özellik** değerini *kimlik doğrulama*olarak ayarlayın.
    1. **İşleç** alanını eşittir işareti (=) olarak ayarlayın.
    1. **Values** alanını *anonim*olarak ayarlayın.
1. Sağ üst köşede, ölçümü görüntülemek istediğiniz zaman aralığını seçin. Ayrıca, isteklerin toplanmasının ne kadar ayrıntılı olduğunu, 1 dakikadan 1 aya kadar bir yerde aralıklar belirterek belirtebilirsiniz.

Ölçümü yapılandırdıktan sonra, anonim istekler grafikte görünmeye başlar. Aşağıdaki görüntüde, son otuz dakika içinde toplanan anonim istekler gösterilmektedir.

:::image type="content" source="media/anonymous-read-access-prevent/metric-anonymous-blob-requests.png" alt-text="Blob depolamaya yönelik toplanmış Anonim istekleri gösteren ekran görüntüsü":::

Ayrıca, depolama hesabınızda belirli sayıda anonim istek yapıldığında size bildirimde bulunan bir uyarı kuralı yapılandırabilirsiniz. Daha fazla bilgi için bkz. [Azure izleyici kullanarak ölçüm uyarıları oluşturma, görüntüleme ve yönetme](../../azure-monitor/platform/alerts-metric.md).

### <a name="analyze-logs-to-identify-containers-receiving-anonymous-requests"></a>Anonim istek alma kapsayıcılarını tanımlamak için günlükleri analiz etme

Azure depolama günlükleri, bir isteğin nasıl yetkilendirildiği de dahil olmak üzere depolama hesabında yapılan isteklerle ilgili ayrıntıları yakalar. Hangi kapsayıcıların adsız istek aldığını öğrenmek için günlükleri çözümleyebilirsiniz.

Anonim istekleri değerlendirmek amacıyla Azure depolama hesabınıza yönelik istekleri günlüğe kaydetmek için Azure Izleyici 'de (Önizleme) Azure depolama günlüğü 'nü kullanabilirsiniz. Daha fazla bilgi için bkz. [Azure Storage 'ı izleme](../common/monitor-storage.md).

Azure Izleyici 'de Azure depolama günlüğü, günlük verilerini çözümlemek için günlük sorgularının kullanılmasını destekler. Günlükleri sorgulamak için bir Azure Log Analytics çalışma alanı kullanabilirsiniz. Günlük sorguları hakkında daha fazla bilgi edinmek için bkz. [öğretici: Log Analytics sorguları kullanmaya başlama](../../azure-monitor/log-query/get-started-portal.md).

#### <a name="create-a-diagnostic-setting-in-the-azure-portal"></a>Azure portal bir tanılama ayarı oluşturun

Azure depolama verilerini Azure Izleyici ile günlüğe kaydetmek ve Azure Log Analytics ile analiz etmek için, önce hangi istek türlerinin ve hangi depolama hizmetlerini günlüğe kaydetmek istediğinizi belirten bir tanılama ayarı oluşturmanız gerekir. Azure portal bir tanılama ayarı oluşturmak için aşağıdaki adımları izleyin:

1. Azure [izleyici önizlemesinde Azure Storage günlük](https://forms.microsoft.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRxW65f1VQyNCuBHMIMBV8qlUM0E0MFdPRFpOVTRYVklDSE1WUTcyTVAwOC4u)kaydına kaydolun.
1. Azure depolama hesabınızı içeren abonelikte yeni bir Log Analytics çalışma alanı oluşturun. Depolama hesabınız için günlüğe kaydetmeyi yapılandırdıktan sonra, Günlükler Log Analytics çalışma alanında kullanılabilir. Daha fazla bilgi için [Azure portal Log Analytics çalışma alanı oluşturma](../../azure-monitor/learn/quick-create-workspace.md)bölümüne bakın.
1. Azure portalda depolama hesabınıza gidin.
1. Izleme bölümünde **Tanılama ayarları (Önizleme)** öğesini seçin.
1. Blob depolamada yapılan istekleri günlüğe kaydetmek için **BLOB** ' u seçin.
1. **Tanılama ayarı Ekle**' yi seçin.
1. Tanılama ayarı için bir ad girin.
1. **Kategori ayrıntıları**' nın altında, **günlük** bölümünde günlüğe kaydedilecek istek türlerini seçin. Tüm anonim istekler okuma istekleri olacak, bu nedenle Anonim istekleri yakalamak için **Storageread** öğesini seçin.
1. **Hedef ayrıntıları**altında **Log Analytics gönder**' i seçin. Aşağıdaki görüntüde gösterildiği gibi aboneliğinizi ve daha önce oluşturduğunuz Log Analytics çalışma alanını seçin.

    :::image type="content" source="media/anonymous-read-access-prevent/create-diagnostic-setting-logs.png" alt-text="İstekleri günlüğe kaydetmek için bir tanılama ayarı oluşturmayı gösteren ekran görüntüsü":::

Tanılama ayarını oluşturduktan sonra depolama hesabına yönelik istekler daha sonra bu ayara göre günlüğe kaydedilir. Daha fazla bilgi için bkz. [Azure 'da kaynak günlüklerini ve ölçümleri toplamak için tanılama ayarı oluşturma](../../azure-monitor/platform/diagnostic-settings.md).

Azure Izleyici 'de Azure depolama günlüklerinde bulunan alanların bir başvurusu için bkz. [kaynak günlükleri (Önizleme)](../common/monitor-storage-reference.md#resource-logs-preview).

#### <a name="query-logs-for-anonymous-requests"></a>Anonim istekler için sorgu günlükleri

Azure Izleyici 'de Azure depolama günlükleri, bir depolama hesabına istek yapmak için kullanılan yetkilendirme türünü içerir. Günlük sorgunuzda, anonim istekleri görüntülemek için **AuthenticationType** özelliğini filtreleyin.

Blob depolamaya yönelik anonim istekler için son 7 güne ait günlükleri almak için Log Analytics çalışma alanınızı açın. Sonra, aşağıdaki sorguyu yeni bir günlük sorgusuna yapıştırın ve çalıştırın. Köşeli ayraçlar içindeki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın:

```kusto
StorageBlobLogs
| where TimeGenerated > ago(7d) and AuthenticationType == "Anonymous"
| project TimeGenerated, AccountName, AuthenticationType, Uri
```

Ayrıca, bu sorguya bağlı olarak, anonim istekler hakkında bildirim almak için bir uyarı kuralı yapılandırabilirsiniz. Daha fazla bilgi için bkz. [Azure izleyici kullanarak günlük uyarıları oluşturma, görüntüleme ve yönetme](../../azure-monitor/platform/alerts-log.md).

## <a name="remediate-anonymous-public-access"></a>Anonim genel erişimi düzelt

Depolama hesabınızdaki kapsayıcılara ve bloblara Anonim istekleri değerlendirdikten sonra, genel erişimi sınırlamak veya engellemek için işlem yapabilirsiniz. Depolama hesabınızdaki bazı kapsayıcıların genel erişim için kullanılabilir olması gerekiyorsa, Depolama hesabınızdaki her bir kapsayıcı için genel erişim ayarını yapılandırabilirsiniz. Bu seçenek, genel erişim üzerinde en ayrıntılı denetimi sağlar. Daha fazla bilgi için bkz. [bir kapsayıcı için genel erişim düzeyini ayarlama](anonymous-read-access-configure.md#set-the-public-access-level-for-a-container).

Gelişmiş güvenlik için, tüm depolama hesabı için genel erişimi devre dışı bırakabilirsiniz. Bir depolama hesabı için genel erişim ayarı, bu hesaptaki kapsayıcıların bağımsız ayarlarını geçersiz kılar. Bir depolama hesabı için genel erişimi devre dışı bıraktığınızda, genel erişime izin verecek şekilde yapılandırılmış tüm kapsayıcılara artık anonim olarak erişilemez. Daha fazla bilgi için bkz. [depolama hesabı için genel okuma erişimini etkinleştirme veya devre dışı bırakma](anonymous-read-access-configure.md#enable-or-disable-public-read-access-for-a-storage-account).

Senaryonuz belirli kapsayıcıların genel erişim için kullanılabilir olmasını gerektiriyorsa, bu kapsayıcıları ve bloblarını genel erişim için ayrılan depolama hesaplarına taşımak önerilir. Daha sonra diğer depolama hesapları için genel erişimi devre dışı bırakabilirsiniz.

### <a name="verify-that-public-access-to-a-blob-is-not-permitted"></a>Bir bloba genel erişime izin verilmediğini doğrulama

Belirli bir Blobun genel erişiminin reddedildiğini doğrulamak için blobu URL 'SI aracılığıyla indirmeyi deneyebilirsiniz. İndirme başarılı olursa blobu hala herkese açık olarak kullanılabilir olur. Depolama hesabı için genel erişim devre dışı bırakıldığından blob herkese açık bir şekilde erişilemezse, bu depolama hesabında genel erişime izin verilmediğini belirten bir hata iletisi görürsünüz.

Aşağıdaki örnek, bir blob 'u URL aracılığıyla indirmeyi denemek için PowerShell 'in nasıl kullanılacağını gösterir. Köşeli ayraçlar içindeki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın:

```powershell
$url = "<absolute-url-to-blob>"
$downloadTo = "<file-path-for-download>"
Invoke-WebRequest -Uri $url -OutFile $downloadTo -ErrorAction Stop
```

### <a name="verify-that-modifying-the-containers-public-access-setting-is-not-permitted"></a>Kapsayıcının ortak erişim ayarını değiştirmeye izin verilmediğini doğrula

Depolama hesabı için ortak erişim devre dışı bırakıldıktan sonra kapsayıcının genel erişim ayarının değiştirilemeyeceğini doğrulamak için, ayarı değiştirmeyi deneyebilirsiniz. Depolama hesabı için genel erişim devre dışıysa kapsayıcının ortak erişim ayarını değiştirmek başarısız olur.

Aşağıdaki örnek, bir kapsayıcının ortak erişim ayarını değiştirmeye çalışmak için PowerShell 'in nasıl kullanılacağını gösterir. Köşeli ayraçlar içindeki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın:

```powershell
$rgName = "<resource-group>"
$accountName = "<storage-account>"
$containerName = "<container-name>"

$storageAccount = Get-AzStorageAccount -ResourceGroupName $rgName -Name $accountName
$ctx = $storageAccount.Context

Set-AzStorageContainerAcl -Context $ctx -Container $containerName -Permission Blob
```

### <a name="verify-that-creating-a-container-with-public-access-enabled-is-not-permitted"></a>Ortak erişime sahip bir kapsayıcı oluşturmaya izin verilmediğini doğrulayın

Depolama hesabı için genel erişim devre dışıysa, ortak erişim etkin olan yeni bir kapsayıcı oluşturabileceksiniz. Doğrulamak için, ortak erişim etkinleştirilmiş bir kapsayıcı oluşturmayı deneyebilirsiniz.

Aşağıdaki örnek, ortak erişim etkinleştirilmiş bir kapsayıcı oluşturmayı denemek için PowerShell 'in nasıl kullanılacağını gösterir. Köşeli ayraçlar içindeki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın:
 
```powershell
$rgName = "<resource-group>"
$accountName = "<storage-account>"
$containerName = "<container-name>"

$storageAccount = Get-AzStorageAccount -ResourceGroupName $rgName -Name $accountName
$ctx = $storageAccount.Context

New-AzStorageContainer -Name $containerName -Permission Blob -Context $ctx
```

## <a name="next-steps"></a>Sonraki adımlar

- [Kapsayıcılar ve BLOB 'lar için anonim genel okuma erişimini yapılandırma](anonymous-read-access-configure.md)
- [.NET ile ortak kapsayıcılara ve bloblara anonim erişim](anonymous-read-access-client.md)