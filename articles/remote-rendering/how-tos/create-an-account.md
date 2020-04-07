---
title: Azure Uzaktan İşlem hesabı oluşturma
description: Azure Uzaktan İşleme için hesap oluşturma adımlarını açıklar
author: florianborn71
ms.author: flborn
ms.date: 02/11/2020
ms.topic: how-to
ms.openlocfilehash: b9b72fb9e80c588eb3e6642d0228bffa50b35c6e
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80681304"
---
# <a name="create-an-azure-remote-rendering-account"></a>Azure Uzaktan İşlem hesabı oluşturma

Bu bölümde, **Azure Uzaktan İşlem** hizmeti için bir hesap oluşturma adımları size yol gösterin. Hızlı başlatmaları veya öğreticileri tamamlamak için geçerli bir hesap zorunludur.

## <a name="create-an-account"></a>Hesap oluşturma

Azure Uzaktan İşlem hizmeti için bir hesap oluşturmak için aşağıdaki adımlar gereklidir:

1. [Karışık Gerçeklik Önizleme sayfasına](https://aka.ms/MixedRealityPrivatePreview) gidin
1. 'Kaynak oluştur' düğmesini tıklatın
1. Arama alanında ("Pazar yerinde arama"), "Uzaktan İşleme" yazın ve 'enter' tuşuna basın.
1. Sonuç listesinde , "Uzaktan İşleme" döşemesini tıklatın
1. Bir sonraki ekranda "Oluştur" düğmesini tıklatın. Yeni bir Uzaktan Render hesabı oluşturmak için bir form açılır:
    1. Hesabın adına 'Kaynak Adı' ayarlama
    1. Gerekirse 'Abonelik'i güncelleştirme
    1. 'Kaynak grubu'nı seçtiğiniz bir kaynak grubuna ayarlama
1. Hesap oluşturulduktan sonra, hesap için gidin ve:
    1. Genel *Bakış* sekmesinde, 'Hesap Kimliği'ni
    1. Ayarlar *> Erişim Anahtarları* sekmesinde, 'Birincil anahtar' -bu hesabın gizli hesap anahtarıdır

### <a name="retrieve-the-account-information"></a>Hesap bilgilerini alma

Örnekler ve öğreticiler, hesap kimliği ve bir anahtar sağlamanızı gerektirir. Örneğin PowerShell örnek komut dosyaları için kullanılan **arrconfig.json** dosyasında:

```json
    "accountSettings": {
        "arrAccountId": "<fill in the account ID from the Azure portal>",
        "arrAccountKey": "<fill in the account key from the Azure portal>",
        "region": "<select from available regions>"
    },
```

*Bölge* seçeneğini doldurmak için [kullanılabilir bölgelerin listesine](../reference/regions.md) bakın.

Aşağıdaki adımlarda **`arrAccountKey`** açıklandığı gibi portaldaki değerler **`arrAccountId`** ve değerler bulunabilir:

* [Azure portalına](https://www.portal.azure.com) gidin
* **"Uzaktan İşleme Hesabınızı"** bulun - **"Son Kaynaklar"** listesinde olmalıdır. Ayrıca üst kısmında arama çubuğunda arayabilirsiniz. Bu durumda, kullanmak istediğiniz aboneliğin Varsayılan abonelik filtresinde (arama çubuğunun yanındaki filtre simgesi) seçildiğinden emin olun:

![Abonelik filtresi](./media/azure-subscription-filter.png)

Hesabınıza tıkladığınızda, **Hesap Kimliğini** hemen gösteren bu ekrana gelir:

![Azure hesap kimliği](./media/azure-account-id.png)

Anahtar için soldaki panelde **Erişim Tuşları'nı** seçin. Sonraki sayfada birincil ve ikincil bir anahtar gösterilmektedir:

![Azure erişim anahtarları](./media/azure-account-primary-key.png)

Değer birincil **`arrAccountKey`** veya ikincil anahtar olabilir.

## <a name="link-storage-accounts"></a>Bağlantı depolama hesapları

Bu paragraf, depolama hesaplarını Uzaktan İşlem hesabınıza nasıl bağladığını açıklar. Bir depolama hesabı bağlandığında, örneğin bir modeli yüklerken hesabınızdaki verilerle her etkileşimde kalmak istediğinizde bir SAS URI oluşturmanız gerekmez. Bunun yerine, depolama hesabı adlarını doğrudan [yükleme modeli bölümünde](../concepts/models.md#loading-models)açıklandığı gibi kullanabilirsiniz.

Bu paragraftaki adımların, bu alternatif erişim yöntemini kullanması gereken her depolama hesabı için gerçekleştirilmesi gerekir. Henüz depolama hesapları oluşturmadıysanız, hızlı başlatma yı işlemek [için bir modeli dönüştürmede](../quickstarts/convert-model.md#storage-account-creation)ilgili adımda yürüyebilirsiniz.

Şimdi bir depolama hesabınız olduğu varsayılır. Portaldaki depolama hesabına gidin ve bu depolama hesabı için **Erişim Denetimi (IAM)** sekmesine gidin:

![Depolama hesabı IAM](./media/azure-storage-account.png)

 Rol atamaları ekleyebildiğinizden emin olmak için bu depolama hesabı üzerinde sahip iznine sahip olduğundan emin olun. Erişiminiz yoksa, **rol atama ekle** seçeneği devre dışı bırakılır.

 Sonraki adımlarda açıklandığı gibi üç farklı rol eklemeniz gerekir. Üç erişim düzeyi nin de sağlamazsanız, depolama hesabına erişmeye çalışırken izin sorunları olacaktır.

 İlk rolü eklemek için "Rol ataması ekle" döşemesindeki **Ekle** düğmesine tıklayın:

![Depolama hesabı IAM](./media/azure-add-role-assignment.png)

* Atamak için ilk rol Yukarıdaki ekran görüntüsünde gösterildiği gibi **Sahibidir.** 
* * Açılır açılır**adak erişim atayın** **Uzaktan RenderIng Hesabı** seçin.
* Son açılır da aboneliğinizi ve Uzaktan İşleme hesabınızı seçin.

**Role** açılır tarihinden ilgili seçimler için iki kez daha yeni roller eklemeyi yineleyin:
* **Depolama Hesabı Katılımcısı**
* **Depolama Blob Veri Katılımcısı**

Diğer açılır lar ilk adımdaki gibi seçilir.

Üç rolü de eklediyseniz, Azure Uzaktan İşlem hesabınız, Yönetilen Hizmet Kimlikleri atanan sistemi kullanarak depolama hesabınıza erişebilir.

## <a name="next-steps"></a>Sonraki adımlar

* [Kimlik Doğrulaması](authentication.md)
* [Kimlik doğrulama için Azure Frontend API'lerini kullanma](frontend-apis.md)
* [Örnek PowerShell komut dosyaları](../samples/powershell-example-scripts.md)
