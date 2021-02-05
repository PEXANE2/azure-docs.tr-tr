---
title: Modeli dönüştürme
description: Özel bir model için dönüştürme adımlarını gösteren hızlı başlangıç.
author: florianborn71
ms.author: flborn
ms.date: 01/23/2020
ms.topic: quickstart
ms.openlocfilehash: c9b5d525954e7f0742cd13fe4d64a73df64ea854
ms.sourcegitcommit: f377ba5ebd431e8c3579445ff588da664b00b36b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2021
ms.locfileid: "99594476"
---
# <a name="quickstart-convert-a-model-for-rendering"></a>Hızlı başlangıç: Modeli işlenmek üzere dönüştürme

[Hızlı başlangıç: Unity ile model işleme](render-model.md), bir yerleşik modeli Işlemek için Unity örnek projesini nasıl kullanacağınızı öğrendiniz. Bu kılavuzda kendi modellerinizi nasıl dönüştürebileceğiniz gösterilmektedir.

Şunları öğrenirsiniz:

> [!div class="checklist"]
>
> * Giriş ve çıkış için bir Azure Blob depolama hesabı ayarlama
> * Azure uzaktan Işleme ile kullanmak için bir 3B modeli yükleme ve dönüştürme
> * Dönüştürülmüş 3B modeli işleme için bir uygulamaya ekleme

## <a name="prerequisites"></a>Önkoşullar

* [Hızlı başlangıç: Unity ile model işleme](render-model.md)
* PowerShell betiği kullanılarak dönüştürme için: Install Azure PowerShell [(belgeler)](/powershell/azure/)
  * Yönetici haklarıyla bir PowerShell açın
  * Çalışmaz `Install-Module -Name Az -AllowClobber`

## <a name="overview"></a>Genel Bakış

Sunucu üzerindeki işleyici, FBX veya GLTF gibi kaynak model biçimleriyle doğrudan çalışabilir. Bunun yerine, modelin özel bir ikili biçimde olmasını gerektirir.
Dönüştürme hizmeti, Azure Blob depolama alanındaki modelleri kullanır ve dönüştürülen modelleri, belirtilen bir Azure Blob depolama kapsayıcısına geri yazar.

Gerekenler:

* Bir Azure aboneliği
* Aboneliğinizdeki bir ' StorageV2 ' hesabı
* Giriş modeliniz için bir BLOB depolama kapsayıcısı
* Çıkış verileriniz için bir BLOB depolama kapsayıcısı
* Dönüştürülecek bir model, bkz. [örnek modeller](../samples/sample-model.md)
  * [Desteklenen kaynak biçimlerinin](../how-tos/conversion/model-conversion.md#supported-source-formats) listesini görün
  * Örnek dönüştürme betiğini kullanmak için, modeli ve tüm dış bağımlılıkları (dış dokular veya geometri gibi) içeren bir giriş klasörü hazırlandığınızdan emin olun

## <a name="azure-setup"></a>Azure kurulumu

Henüz bir hesabınız yoksa, sayfasına gidin [https://azure.microsoft.com/get-started/](https://azure.microsoft.com/get-started/) , ücretsiz hesap seçeneğine tıklayın ve yönergeleri izleyin.

Bir Azure hesabınız olduğunda adresine gidin [https://ms.portal.azure.com/#home](https://ms.portal.azure.com/#home) .

### <a name="storage-account-creation"></a>Depolama hesabı oluşturma

BLOB depolama oluşturmak için önce bir depolama hesabına ihtiyacınız vardır.
Bir tane oluşturmak için, "kaynak oluştur" düğmesine tıklayın:

![Azure-Kaynak Ekle](media/azure-add-a-resource.png)

Yeni ekrandan, sol taraftaki **depolama** ' yı ve ardından **depolama hesabı-blob, dosya, tablo,** sonraki sütundan kuyruk ' u seçin:

![Azure-depolama ekleme](media/azure-add-storage.png)

Bu düğmeye tıkladığınızda, doldurulacak depolama özellikleriyle aşağıdaki ekran görüntülenir:

![Azure kurulumu](media/azure-setup1.png)

Formu aşağıdaki şekilde doldurun:

* Açılan kutunun altındaki bağlantıdan yeni bir kaynak grubu oluşturun ve bu **ARR_Tutorial** adlandırın
* **Depolama hesabı adı** için buraya benzersiz bir ad girin. **Bu ad genel olarak benzersiz olmalıdır**, aksi takdirde adın zaten alındığını bildiren bir istem olacaktır. Bu hızlı başlangıç kapsamında, **arrtutorialstorage** olarak adlandırın. Buna uygun olarak, bu hızlı başlangıçtaki herhangi bir oluşum için adınızı adınızla değiştirmeniz gerekir.
* Size yakın bir **konum** seçin. İdeal olarak, diğer hızlı başlangıçta işlemeyi ayarlamak için kullanılan konumu kullanır.
* ' Standart ' olarak ayarlanan **performans**
* **Hesap türü** ' StorageV2 (genel amaçlı v2) ' olarak ayarlandı
* **Çoğaltma** ' Okuma Erişimli Coğrafi olarak yedekli depolama (RA-GRS) ' olarak ayarlandı
* **Erişim katmanı** ' Hot ' olarak ayarlandı

Diğer sekmelerdeki özelliklerden hiçbirinin değiştirilmemesi gerekir, bu sayede **"gözden geçir + oluştur"** ile devam edebilir ve ardından kurulumu tamamlamaya yönelik adımları izleyebilirsiniz.

Web sitesi artık dağıtımınızın ilerleme durumu hakkında bilgi verir ve "dağıtımınız tamamlanmıştır". Sonraki adımlar için **"kaynağa git"** düğmesine tıklayın:

![Azure depolama oluşturma Tamam](./media/storage-creation-complete.png)

### <a name="blob-storage-creation"></a>BLOB depolama oluşturma

Ardından, biri giriş ve diğeri çıkış için olmak üzere iki blob kapsayıcıyla ihtiyacımız var.

Yukarıdaki **"kaynağa git"** düğmesine, sol taraftaki bir liste menüsü içeren bir panelin bulunduğu bir sayfaya ulaşırsanız. **"Blob hizmeti"** kategorisinin altındaki listede, **"kapsayıcılar"** düğmesine tıklayın:

![Azure-kapsayıcı ekleme](./media/azure-add-containers.png)

**Giriş** blobu depolama kapsayıcısını oluşturmak için **"+ Container"** düğmesine basın.
Oluştururken aşağıdaki ayarları kullanın:
  
* Ad = arrinput
* Genel erişim düzeyi = özel

Kapsayıcı oluşturulduktan sonra **+ Container** ' e tıklayın ve **Çıkış** kapsayıcısı için bu ayarlarla yineleyin:

* Ad = arroutput
* Genel erişim düzeyi = özel

Artık iki BLOB depolama Kapsayıcınız olmalıdır:

![BLOB depolama kurulumu](./media/blob-setup.png)

## <a name="run-the-conversion"></a>Dönüştürmeyi çalıştırma

Model dönüştürmeyi tetiklemenin üç farklı yolu vardır:

### <a name="1-conversion-via-the-arrt-tool"></a>1. ARRT aracı aracılığıyla dönüştürme

Dönüşümleri başlatmak ve işlenmiş sonuçla etkileşmek için [ARRT adlı bir UI tabanlı araç](./../samples/azure-remote-rendering-asset-tool.md) vardır.
![ARRT](./../samples/media/azure-remote-rendering-asset-tool.png "ARRT ekran görüntüsü")

### <a name="2-conversion-via-a-powershell-script"></a>2. bir PowerShell betiği aracılığıyla dönüştürme

Varlık dönüştürme hizmetini çağırmayı kolaylaştırmak için bir yardımcı program betiği sağlıyoruz. *Betikler* klasöründe bulunur ve **Conversion.ps1** olarak adlandırılır.

Özellikle, bu betik

1. belirli bir dizindeki tüm dosyaları yerel diskten giriş depolama kapsayıcısına yükler
1. , giriş depolama kapsayıcısından verileri alacak ve dönüştürme KIMLIĞI döndüren bir dönüştürme başlatan [varlık dönüştürme REST API](../how-tos/conversion/conversion-rest-api.md)çağırır
1. dönüştürme işlemi başarılı veya başarısız ile sonlanana kadar, alınan dönüştürme KIMLIĞIYLE birlikte dönüştürme durumu API 'sini yoklayın
1. çıktı depolama alanındaki dönüştürülmüş varlığın bir bağlantısını alır

Betik, *üzerindeScripts\arrconfig.js* dosya yapılandırmasını okur. Bu JSON dosyasını bir metin düzenleyicisinde açın.

```json
{
    "accountSettings": {
        "arrAccountId": "8*******-****-****-****-*********d7e",
        "arrAccountKey": "R***************************************l04=",
        "region": "<your-region>"
    },
    "renderingSessionSettings": {
        "vmSize": "standard",
        "maxLeaseTime": "1:00:00"
    },
    "assetConversionSettings": {
        "localAssetDirectoryPath": "D:\\tmp\\robot",
        "resourceGroup": "ARR_Tutorial",
        "storageAccountName": "arrexamplestorage",
        "blobInputContainerName": "arrinput",
        "inputFolderPath": "robotConversion",
        "inputAssetPath": "robot.fbx",
        "blobOutputContainerName": "arroutput",
        "outputFolderPath":"converted/robot",
        "outputAssetFileName": "robot.arrAsset"
    }
}
```

**Accountsettings** grubu içindeki (hesap kimliği ve anahtar) yapılandırma, [Unity hızlı başlangıç ile model işleme](render-model.md)içindeki kimlik bilgilerine benzer şekilde doldurulmalıdır.

**Assetconversionsettings** grubunun içinde, aşağıda görüldüğü gibi **resourceGroup**, **Blobınputcontainername** ve **bloi putcontainername** ' i değiştirdiğinizden emin olun.
**Arrtutorialstorage** değerinin, depolama hesabı oluşturma sırasında seçtiğiniz benzersiz adla değiştirilmeleri gerektiğini unutmayın.

**Localassetdirectorypath** öğesini, diskinizde yer alan ve dönüştürmek istediğiniz modeli içeren dizine işaret etmek üzere değiştirin. \\Çift ters eğik çizgi ("") kullanarak yoldaki ters eğik çizgileri ("") doğru bir şekilde kaçış konusunda dikkatli olun \\ \\ .

**Localassetdirectorypath** içinde verilen yoldaki tüm veriler, **ınputfolderpath** tarafından verilen bir alt yol altında **blobinputcontainername** blob kapsayıcısına yüklenir. Bu nedenle, "D: tmp robot" dizininin içeriğinin yukarıdaki örnek yapılandırmasında " \\ \\ robotConversion" yolu altındaki "arrtutorialstorage" depolama hesabının "arrinput" blob kapsayıcısına yüklenir. Zaten var olan dosyaların üzerine yazılacak.

**Inputassetpath** öğesini dönüştürülecek modelin yolu olarak değiştirin; yol, localAssetDirectoryPath öğesine görelidir. Yol ayırıcısı olarak "" yerine "/" kullanın \\ . Bu nedenle, doğrudan "D: tmp robot" içinde bulunan bir "robot. fbx" dosyası için \\ \\ "robot. fbx" kullanın.

Model dönüştürüldükten sonra, **Bloi Putcontainername** tarafından verilen depolama kapsayıcısına geri yazılır. İsteğe bağlı **Outputfolderpath** sağlanarak bir alt yol belirtilebilir. Yukarıdaki örnekte, sonuçta elde edilen "robot. arrAsset", "dönüştürülmüş/robot" altındaki çıkış blob kapsayıcısına kopyalanacaktır.

**Outputassetfilename** yapılandırma ayarı, dönüştürülmüş varlığın adını belirler-parametre isteğe bağlıdır ve çıkış dosya adı, aksi takdirde giriş dosyası adından çıkarılır.

Bir PowerShell açın, *Azure PowerShell* [önkoşullara](#prerequisites)belirtilen şekilde yüklediğinizden emin olun. Ardından aşağıdaki komutla aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin:

```PowerShell
Connect-AzAccount
```

> [!NOTE]
> Kuruluşunuzun birden fazla aboneliğine sahip olması durumunda, SubscriptionID ve Tenant bağımsız değişkenlerini belirtmeniz gerekebilir. [Connect-AzAccount belgelerindeki](/powershell/module/az.accounts/connect-azaccount)ayrıntıları bulun.

`azure-remote-rendering\Scripts`Dizine geçin ve dönüştürme betiğini çalıştırın:

```PowerShell
.\Conversion.ps1 -UseContainerSas
```

Şuna benzer bir şey görmeniz gerekir: ![Conversion.ps1](./media/successful-conversion.png)

### <a name="3-conversion-via-api-calls"></a>3. API çağrıları aracılığıyla dönüştürme

C# ve C++ API 'SI, hizmetle etkileşimde bulunmak için bir giriş noktası sağlar:
* [C# RemoteRenderingClient. StartAssetConversionAsync ()](/dotnet/api/microsoft.azure.remoterendering.remoterenderingclient.startassetconversionasync)
* [C++ RemoteRenderingClient:: StartAssetConversionAsync ()](/cpp/api/remote-rendering/remoterenderingclient#startassetconversionasync)


## <a name="insert-new-model-into-quickstart-sample-app"></a>Hızlı Başlangıç örnek uygulamasına yeni model Ekle

Dönüştürme betiği, dönüştürülmüş model için bir *paylaşılan erişim imzası (SAS)* URI 'si oluşturur. Artık bu URI 'yi **model adı** olarak hızlı başlangıç örnek uygulamasına kopyalayabilirsiniz (bkz. [hızlı başlangıç: Unity ile model oluşturma](render-model.md)).

![Unity 'de modeli değiştirme](./media/replace-model-in-unity.png)

 Örnek, şimdi özel modelinizi yükleyip işlemelidir!

## <a name="optional-re-creating-a-sas-uri"></a>İsteğe bağlı: SAS URI 'sini yeniden oluşturma

Dönüştürme betiği tarafından oluşturulan SAS URI 'SI yalnızca 24 saat için geçerli olacaktır. Ancak, bu süre dolduktan sonra modelinizi yeniden dönüştürmeniz gerekmez. Bunun yerine, sonraki adımlarda açıklandığı gibi portalda yeni bir SAS oluşturabilirsiniz:

1. [Azure portal](https://www.portal.azure.com)'a gidin
1. **Depolama hesabı** kaynağına tıklayın: ![ Seçili depolama hesabı kaynağını vurgulayan ekran görüntüsü.](./media/portal-storage-accounts.png)
1. Aşağıdaki ekranda, sol panelde **Depolama Gezgini** ' ne tıklayın ve *arroutput* BLOB depolama kapsayıcısında çıkış modelinizi (*. arrAsset* dosyası) bulun. Dosyaya sağ tıklayın ve bağlam menüsünden **paylaşılan erişim Imzası al** ' ı seçin: ![ imza erişimi](./media/portal-storage-explorer.png)
1. Sona erme tarihi seçebileceğiniz yeni bir ekran açılır. **Oluştur**' a basın ve sonraki iletişim kutusunda gösterilen URI 'yi kopyalayın. Bu yeni URI, betiğin oluşturulduğu geçici URI 'yi değiştirir.

## <a name="next-steps"></a>Sonraki adımlar

Artık temel bilgileri öğrenmiş olduğunuza göre, daha ayrıntılı bilgi edinmek için öğreticilerimize göz atın.

Model dönüştürmenin ayrıntılarını öğrenmek isterseniz, [model dönüştürme REST API](../how-tos/conversion/conversion-rest-api.md)göz atın.

> [!div class="nextstepaction"]
> [Öğretici: uzaktan işlenmiş modelleri görüntüleme](../tutorials/unity/view-remote-models/view-remote-models.md)