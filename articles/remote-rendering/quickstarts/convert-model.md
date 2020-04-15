---
title: Modeli dönüştürme
description: Özel bir modeliçin dönüşüm adımlarını gösteren hızlı başlat.
author: florianborn71
ms.author: flborn
ms.date: 01/23/2020
ms.topic: quickstart
ms.openlocfilehash: 7ba8d201c29b5e3835fec52d8c479a388ca07f71
ms.sourcegitcommit: 7e04a51363de29322de08d2c5024d97506937a60
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81312990"
---
# <a name="quickstart-convert-a-model-for-rendering"></a>Quickstart: Oluşturma için bir modeli dönüştürme

[Quickstart: Unity ile bir model oluşturma](render-model.md), yerleşik bir model işlemek için Birlik örnek projesini nasıl kullanacağınızı öğrendiniz. Bu kılavuz, kendi modellerinizi nasıl dönüştürdüğünüzü gösterir.

Şunları öğrenirsiniz:

> [!div class="checklist"]
>
> * Giriş ve çıktı için azure blob depolama hesabı ayarlama
> * Azure Uzaktan İşleme ile kullanılmak üzere bir 3B model yükleme ve dönüştürme
> * Dönüştürme yapılan 3B modeli işleme uygulamasına dahil etme

## <a name="prerequisites"></a>Ön koşullar

* Komple [Quickstart: Unity ile bir model oluşturma](render-model.md)
* Azure PowerShell'i yükleme [(dokümantasyon)](https://docs.microsoft.com/powershell/azure/)
  * Yönetici haklarıyla powershell açın
  * Çalıştırmak:`Install-Module -Name Az -AllowClobber`

## <a name="overview"></a>Genel Bakış

Sunucudaki işleyici, FBX veya GLTF gibi kaynak model biçimleriyle doğrudan çalışamaz. Bunun yerine, modelin özel bir ikili biçimde olmasını gerektirir.
Dönüştürme hizmeti, Azure blob depolama adresindeki modelleri tüketir ve dönüştürülmüş modelleri sağlanan Azure blob depolama kapsayıcısına geri yazar.

Gerekenler:

* Bir Azure aboneliği
* Aboneliğinizde bir 'StorageV2' hesabı
* Giriş modeliniz için bir blob depolama kabı
* Çıktı verileriniz için bir blob depolama kapsayıcısı
* Dönüştürmek için bir model, [örnek modellere](../samples/sample-model.md) bakın
  * Desteklenen kaynak [biçimleri](../how-tos/conversion/model-conversion.md#supported-source-formats) listesine bakın
  * Örnek dönüştürme komut dosyasını kullanmak için, modeli ve tüm dış bağımlılıkları (dış dokular veya geometri gibi) içeren bir giriş klasörü hazırladığınıza emin olun

## <a name="azure-setup"></a>Azure kurulumu

Henüz bir hesabınız yoksa, [https://azure.microsoft.com/get-started/](https://azure.microsoft.com/get-started/)ücretsiz hesap seçeneğini tıklayın ve talimatları izleyin.

Azure hesabınız olduktan sonra' [https://ms.portal.azure.com/#home](https://ms.portal.azure.com/#home)a gidin.

### <a name="storage-account-creation"></a>Depolama hesabı oluşturma

Blob depolama oluşturmak için öncelikle bir depolama hesabı gerekir.
Bir tane oluşturmak için "Kaynak oluştur" düğmesine tıklayın:

![Azure - kaynak ekleme](media/azure-add-a-resource.png)

Yeni ekrandan, sol tarafta **Depolama'yı** ve ardından Depolama hesabını seçin **- blob, dosya, tablo,** sonraki sütundan sıra:

![Azure - depolama alanı ekle](media/azure-add-storage.png)

Bu düğmeyi tıklattığınızda, doldurulması gereken depolama özellikleri yle birlikte aşağıdaki ekran açılır:

![Azure Kurulumu](media/azure-setup1.png)

Formu aşağıdaki şekilde doldurun:

* Açılan kutunun altındaki bağlantıdan yeni bir Kaynak Grubu oluşturun ve bu **ARR_Tutorial**
* Depolama **hesabı adı için**buraya benzersiz bir ad girin. **Bu ad genel olarak benzersiz olmalıdır,** aksi takdirde adı verilen hazır olduğunu bildiren bir istem olacaktır. Bu hızlı başlangıç kapsamında, biz **arrtutorialstorage**adını. Buna göre, bu hızlı başlangıç herhangi bir olay için adınızı ile değiştirmeniz gerekir.
* Size yakın bir **konum** seçin. İdeal olarak, diğer hızlı başlatmada işlemeyi ayarlamak için kullanılan aynı konumu kullanın.
* **Performans** 'Standart' olarak ayarlandı
* 'StorageV2 (genel amaçlı v2)' olarak ayarlanmış **hesap türü**
* **Çoğaltma** 'Okuma-erişim coğrafi yedekli depolama (RA-GRS)' olarak ayarlanır
* **Erişim katmanı** 'Hot' olarak ayarlandı

Diğer sekmelerde özelliklerin hiçbirinin değiştirilmesi gerekmez, bu nedenle **"Gözden Geçir + oluştur"** bölümüne devam edebilir ve kurulumu tamamlamak için adımları izleyebilirsiniz.

Web sitesi artık dağıtımınızın ilerlemesi hakkında sizi bilgilendirir ve sonunda "Dağıtımınız tamamlandı" bildirir. Sonraki adımlar için **"Kaynağa git"** düğmesine tıklayın:

![Azure Depolama oluşturma tamamlandı](./media/storage-creation-complete.png)

### <a name="blob-storage-creation"></a>Blob depolama oluşturma

Sonra iki blob kaplar, giriş ve çıktı için bir gerekir.

Yukarıdaki **"Kaynağa git"** düğmesinden, solda liste menüsü içeren bir panelin bulunduğu bir sayfaya geçersiniz. **"Blob hizmeti"** kategorisi altındaki listede **"Kapsayıcılar"** düğmesine tıklayın:

![Azure - Kapsayıcı ekle](./media/azure-add-containers.png)

**Giriş** blob depolama kabı oluşturmak için **"+ Konteyner"** düğmesine basın.
Oluştururken aşağıdaki ayarları kullanın:
  
* Ad = arrinput
* Genel erişim düzeyi = Özel

Kapsayıcı oluşturulduktan sonra **+ Kapsayıcı'yı** tekrar tıklatın ve **çıkış** kapsayıcısı için bu ayarları tekrarlayın:

* Ad = arroutput
* Genel erişim düzeyi = Özel

Şimdi iki blob depolama kapları olmalıdır:

![Blob Depolama Kurulumu](./media/blob-setup.png)

## <a name="run-the-conversion"></a>Dönüşümü çalıştırma

Varlık dönüştürme hizmetini aramayı kolaylaştırmak için bir yardımcı program komut dosyası salıyoruz. *Bu Scripts* klasöründe bulunan ve **Conversion.ps1**denir.

Özellikle, bu komut dosyası

1. yerel diskten giriş depolama kapsayıcısına belirli bir dizindeki tüm dosyaları yükler
1. giriş depolama kabından verileri alacak ve bir dönüşüm kimliği döndürecek bir dönüştürme başlatacak [varlık dönüştürme REST API](../how-tos/conversion/conversion-rest-api.md) çağırır
1. dönüşüm işlemi başarı veya başarısızlıkla sona erene kadar, alınan dönüşüm kimliğiyle dönüşüm durumu API'sini yoklama
1. çıkış depolamasında dönüştürülen varlığa bir bağlantı alır

Komut dosyası dosya *Scripts\arrconfig.json*kendi yapılandırmasını okur. JSON dosyanı bir metin düzenleyicisinde açın.

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

**Hesap Ayarları** grubu içindeki yapılandırma (hesap kimliği ve anahtar) [Unity quickstart ile bir model oluşturmadaki](render-model.md)kimlik bilgilerine benzer şekilde doldurulmalıdır.

**VarlıkDönüşüm Ayarları** grubunda, yukarıda görüldüğü gibi **kaynakGrubu**, **blobInputContainerName**ve **blobOutputContainerName'i** değiştirdiğinden emin olun.
**Arrtutorialstorage** değerinin depolama hesabı oluşturma sırasında seçtiğiniz benzersiz adla değiştirilmesi gerektiğini unutmayın.

**YerelAssetDirectoryPath'i,** dönüştürmek istediğiniz modeli içeren diskinizdeki dizini işaret etmek için değiştirin. Çift eğik çizgi (" ") kullanarak yolda düzgün bir şekilde ters eğik çizgilerden ("\\")\\\\kaçmaya dikkat edin.

**LocalAssetDirectoryPath'de** verilen yoldan tüm **veriler, inputFolderPath**tarafından verilen bir alt patavar altında **blobInputContainerName** blob konteynerine yüklenir. Yani "D:\\tmp\\robot" dizininin içeriğinin üzerindeki örnek yapılandırmada "robotDönüşüm" yolu altında depolama hesabı "arrtutorialstorage" blob konteyner "arrinput" yüklenecektir. Zaten varolan dosyalar üzerine yazılır.

**GirişAssetPath'i** dönüştürülecek modelin yoluna değiştirin - yol yerelAssetDirectoryPath'e göredir. Yol ayırıcısı olarak\\" " yerine "/" kullanın. Yani doğrudan "D:\\tmp\\robot" bulunan bir "robot.fbx" dosyası için "robot.fbx" kullanın.

Model dönüştürüldükten sonra **blobOutputContainerName**tarafından verilen depolama konteynerine geri yazılacaktır. Bir alt patina isteğe bağlı **çıktılarFolderPath**sağlayarak belirtilebilir. Yukarıdaki örnekte ortaya çıkan "robot.arrAsset" "dönüştürülmüş/robot" altında çıkış blob konteyner kopyalanacaktır.

Config ayarı **çıktısıAssetFileName** dönüştürülen varlığın adını belirler - parametre isteğe bağlıdır ve çıktı dosya adı aksi takdirde giriş dosyası adından çıkarılır. 

Bir PowerShell açın, [ön koşullarda](#prerequisites)belirtildiği gibi *Azure PowerShell'i* yüklediğinizden emin olun. Ardından aşağıdaki komutla aboneliğinize giriş yapın ve ekrandaki yönergeleri izleyin:

```PowerShell
Connect-AzAccount
```

> [!NOTE]
> Kuruluşunuzun birden fazla aboneliği olması durumunda, SubscriptionId ve Kiracı bağımsız değişkenlerini belirtmeniz gerekebilir. [Connect-AzAccount belgelerindeki](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount)ayrıntıları bulun.

Dizin `azure-remote-rendering\Scripts` değiştirin ve dönüşüm komut dosyası çalıştırın:

```PowerShell
.\Conversion.ps1 -UseContainerSas
```

Böyle bir şey görmelisiniz: ![Conversion.ps1](./media/successful-conversion.png)

Dönüştürme komut dosyası dönüştürülen model için *paylaşılan erişim imzası (SAS)* URI oluşturur. Artık bu URI'yi **Model Adı** olarak quickstart örnek uygulamasına kopyalayabilirsiniz (bkz. [Quickstart: Unity ile bir model oluşturma).](render-model.md)

![Birlikte modeli değiştirin](./media/replace-model-in-unity.png)

 Örnek şimdi yüklemeli ve özel model işlemek!

## <a name="optional-re-creating-a-sas-uri"></a>İsteğe bağlı: SAS URI'yi yeniden oluşturma

Dönüştürme komut dosyası tarafından oluşturulan SAS URI yalnızca 24 saat geçerli olacaktır. Ancak, süresi dolduktan sonra modelinizi yeniden dönüştürmeniz gerekmez. Bunun yerine, sonraki adımlarda açıklandığı gibi portalda yeni bir SAS oluşturabilirsiniz:

1. [Azure portalına](https://www.portal.azure.com) gidin
1. **Depolama hesabı** kaynağınıza ![tıklayın: İmza Erişimi](./media/portal-storage-accounts.png)
1. Aşağıdaki ekranda, sol paneldeki **Depolama gezginine** tıklayın ve çıkış modelinizi *(.arrAsset* dosyası) *arroutput* blob depolama kabında bulun. Dosyaya sağ tıklayın ve bağlam menüsünden Paylaşılan Erişim ![ **İmzasını Al'ı** seçin: İmza Erişimi](./media/portal-storage-explorer.png)
1. Son kullanma tarihini seçebileceğiniz yeni bir ekran açılır. **Oluştur'a**basın ve bir sonraki iletişim kutusunda gösterilen URI'yi kopyalayın. Bu yeni URI, komut dosyasının oluşturduğu geçici URI'nin yerini alır.

## <a name="next-steps"></a>Sonraki adımlar

Şimdi temel leri bildiğinize göre, daha derinlemesine bilgi edinmek için öğreticilerimize bir göz atın.

Model dönüştürme ayrıntılarını öğrenmek istiyorsanız, model [dönüşümü REST API'ye](../how-tos/conversion/conversion-rest-api.md)göz atın.

> [!div class="nextstepaction"]
> [Öğretici: Sıfırdan bir Birlik projesi kurma](../tutorials/unity/project-setup.md)
