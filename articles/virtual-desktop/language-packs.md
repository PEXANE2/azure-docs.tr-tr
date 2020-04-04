---
title: Windows Sanal Masaüstünde Windows 10 VM'lere dil paketleri yükleme - Azure
description: Windows Sanal Masaüstü'nde Windows 10 çok oturumlu VM'ler için dil paketleri nasıl yüklenir?
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 04/03/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: c40df9d821e069e2cd5ff0c42d5841f6b9041c96
ms.sourcegitcommit: d597800237783fc384875123ba47aab5671ceb88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80634041"
---
# <a name="install-language-packs"></a>Dil paketlerini yükleme

Windows Sanal Masaüstü dağıtımlarını uluslararası olarak ayarladığınızda, dağıtımınızın birden çok dili desteklediğinden emin olmak iyi bir fikirdir. Kuruluşunuzun ihtiyacı olduğu kadar çok dili desteklemek için dil paketlerini Windows 10 Enterprise çok oturumlu sanal makine (VM) görüntüsüne yükleyebilirsiniz. Bu makalede, dil paketlerinin nasıl yüklenirve kullanıcılarınızın kendi görüntülü dillerini seçmesine izin veren görüntüleri nasıl yakalayabileceğiniz anlatılmaktadır.

[Azure portalı ile kullanılabilirlik bölgesinde Windows sanal makine oluştur'da](../virtual-machines/windows/create-portal-availability-zone.md)Azure'da VM dağıtımı hakkında daha fazla bilgi edinin.

>[!NOTE]
>Bu makale, Windows 10 Enterprise çok oturumlu VM'ler için geçerlidir.

## <a name="install-a-language-pack"></a>Dil paketi yükleme

Dil paketleri içeren bir VM görüntüsü oluşturmak için öncelikle bir makineye dil paketleri yüklemeniz ve bir görüntüsünü yakalamanız gerekir.

Dil paketlerini yüklemek için:

1. Yönetici olarak oturum açın.
2. En son Windows ve Windows Mağazası güncelleştirmelerini yüklediğinizden emin olun.
3. **Ayarlar** > **Saati & Dil** > **Bölgesi'ne**gidin.
4. **Ülke veya bölge**altında açılan menüden tercih ettiğiniz ülkeyi veya bölgeyi seçin.
    Bu örnekte, aşağıdaki ekran görüntüsünde gösterildiği gibi **Fransa'yı**seçeceğiz:

    ![Bölge sayfasının ekran görüntüsü. Şu anda seçilen bölge Fransa'dır.](media/region-page-france.png)

5. Bundan sonra **Dil'i**seçin ve ardından **dil ekle'yi**seçin. Listeden yüklemek istediğiniz dili seçin ve **ardından İleri'yi**seçin.
6. Yükle **dili özellikleri** penceresi açıldığında, dil paketini yükle etiketli onay kutusunu seçin **ve Windows görüntü mresim dili olarak ayarlayın.**
7. **Yükle**’yi seçin.
8. Aynı anda birden çok dil eklemek için, **dil ekle'yi**seçin ve ardından 5 ve 6 adımlarında bir dil eklemek için işlemi yineleyin. Yüklemek istediğiniz her dil için bu işlemi yineleyin. Ancak, aynı anda yalnızca bir dil görüntülü dil olarak ayarlayabilirsiniz.

    Hızlı bir görsel gösteri den başlayalım. Aşağıdaki resimler, Fransızca ve Felemenkçe dil paketlerinin nasıl yüklenir, ardından Fransızcayı görüntü dili olarak nasıl ayarlaylayamaize gösterilmektedir.

    ![İşlemin başındaki Dil sayfasının ekran görüntüsü. Seçili Windows görüntüleme dili İngilizcedir.](media/language-page-default.png)

    ![Dil seçim penceresinin ekran görüntüsü. Kullanıcı, Fransızca dil paketlerini bulmak için arama çubuğuna "Fransızca" girdi.](media/select-language-french.png)

    ![Yükle dil özellikleri sayfasının ekran görüntüsü. Tercih edilen dil olarak Fransızca seçilir. Seçili seçenekler "Görüntü dilimi ayarla", "Dil paketini yükle", "Konuşma tanıma" ve "El yazısı" dır.](media/install-language-features.png)

    Dil paketleriniz yüklendikten sonra dil paketlerinizin adlarının diller listesinde göründüğünü görmeniz gerekir.

    ![Yeni dil paketleri yüklü dil sayfasının ekran görüntüsü. Fransızca ve Hollanda dil paketleri "tercih edilen diller" altında listelenir.](media/language-page-complete.png)

9. Oturumunuzdan oturumunuzu imzalamanızı isteyen bir pencere belirirse. Oturum aç, sonra tekrar oturum aç. Görüntülü diliniz artık seçtiğiniz dil olmalıdır.

10.  Kontrol **Paneli** > **Saati ve Bölge** > **Bölgesine**gidin.

11.  **Bölge** penceresi açıldığında, **İdare** sekmesini seçin ve ardından **Ayarları Kopyala'yı**seçin.

12.  Hoş geldiniz ekranı **ve sistem hesapları ve** Yeni kullanıcı **hesapları**etiketli onay kutularını seçin.

13.  **Tamam'ı**seçin.

14.  Bir pencere açılır ve oturumunuzu yeniden başlatmanızı söyler. **Şimdi Yeniden Başlat'ı**seçin.

15.  Geri giriş yaptıktan **sonra, Denetim Masası** > **Saati ve Bölge** > **Bölgesi'ne**geri dön.

16.  **Yönetim** sekmesini seçin.

17.  **Sistemi değiştir'i seçin.**

18. **Geçerli sistem yerel öğesi**altındaki açılır menüde, kullanmak istediğiniz yerel dili seçin. Bundan sonra, **Tamam'ı**seçin.

19. Oturumunuzu yeniden başlatmak için **şimdi yeniden başlat'ı** seçin.

Tebrikler, dil paketlerinizi yükledin!

Devam etmeden önce, sisteminizin Windows ve Windows mağazasının en son sürümlerinin yüklü olduğundan emin olun.

## <a name="sysprep"></a>Sysprep

Daha sonra, görüntü yakalama işlemi için hazırlamak için makinesprep gerekir.

Makinenizi sysprep için:

1. Yönetici olarak PowerShell’i açın.
2. Doğru dizine gitmek için aşağıdaki cmdlet'i çalıştırın:
   
    ```powershell
    cd Windows\System32\Sysprep
    ```

3. Ardından, aşağıdaki cmdlet çalıştırın:
    
    ```powershell
    .\sysprep.exe
    ```

4. Sistem Hazırlama Aracı penceresi açıldığında, **Genelleolarak**etiketli onay kutusunu seçin, ardından **Kapatma Seçenekleri'ne** gidin ve açılan menüden **Kapat'ı** seçin.

>[!NOTE]
>Syprep işleminin tamamlanması birkaç dakika sürer. VM kapanınca, uzak oturumunuz kesilir.

### <a name="resolve-sysprep-errors"></a>Sysprep hatalarını çözme

Sysprep işlemi sırasında bir hata iletisi görürseniz, yapmanız gerekenler şunlardır:

1. **Drive C'yi** açın ve **Windows** > **System32 Sysprep** > **Panther'e**gidin, ardından **kurulum dosyasını** açın.

   Hata dosyasındaki metin, aşağıdaki resimde gösterildiği gibi belirli bir dil paketini kaldırmanız gerektiğini söyler. Bir sonraki adım için dil paketi adını kopyalayın.

   ![Setuperr dosyasının ekran görüntüsü. Paket adını içeren metin koyu mavi ile vurgulanır.](media/setuperr-package-name.png)

2. Yeni bir PowerShell penceresi açın ve dil paketini kaldırmak için adım 2'de kopyaladığınız paket adı ile aşağıdaki cmdlet'i çalıştırın:

   ```powershell
   Remove-AppxPackage <package name>
   ```

3. `Remove-AppxPackage` Cmdlet'i tekrar çalıştırarak paketi kaldırdığınızdan emin olun. Paketi başarıyla kaldırdıysanız, kaldırmaya çalıştığınız paketin orada olmadığını belirten bir ileti görmeniz gerekir.

4. Cmdlet'i `sysprep.exe` tekrar çalıştırın.

## <a name="capture-the-image"></a>Görüntüyü yakalama

Sisteminiz hazır olduğuna göre, diğer kullanıcıların yapılandırma işlemini yinelemek zorunda kalmadan sisteminize göre VM kullanmaya başlayabilmesi için bir görüntü yakalayabilirsiniz.

Görüntü yakalamak için:

1. Azure portalına gidin ve [bir dil paketi](#install-a-language-pack) ve [sysprep'te](#sysprep)yapılandırdığınız makinenin adını seçin.

2. **Capture'i**seçin.

3. Görüntünüz için **Ad** alanına bir ad girin ve aşağıdaki resimde gösterildiği gibi **Kaynak grubu** açılır menüsünü kullanarak kaynak grubuna atayın.

   ![Görüntü oluştur penceresinin ekran görüntüsü. Kullanıcının bu test görüntüsüne verdiği ad "vmwvd-image-fr" ve "testwvdimagerg" kaynak grubuna atamıştır.](media/create-image.png)

4. **Oluştur'u**seçin.

5. Yakalama işleminin tamamlanması için birkaç dakika bekleyin. Görüntü hazır olduğunda, Bildirimler Merkezi'nde görüntünün yakalandığını bildiren bir ileti görmeniz gerekir.

Artık yeni resminizi kullanarak bir VM dağıtabilirsiniz. VM'yi dağıttığınızda, [Azure portalı ile kullanılabilirlik bölgesinde Windows sanal makine oluştur'daki](../virtual-machines/windows/create-portal-availability-zone.md)yönergeleri uyguladığından emin olun.

### <a name="how-to-change-display-language-for-standard-users"></a>Standart kullanıcılar için görüntü dilini değiştirme

Standart kullanıcılar VM'lerindeki görüntü dilini değiştirebilir.

Görüntü dilini değiştirmek için:

1. Dil **Ayarları'na**gidin. Bunun nerede olduğunu bilmiyorsanız, Başlat Menüsündeki arama çubuğuna **Dil** girebilirsiniz.

2. Windows görüntülü dil açılır menüsünde, görüntülü dil olarak kullanmak istediğiniz dili seçin.

3. Oturumunuzu oturumunuzdan tamamlayın, sonra tekrar oturum açın. Görüntülü dil artık adım 2'de seçtiğiniz dil olmalıdır.
