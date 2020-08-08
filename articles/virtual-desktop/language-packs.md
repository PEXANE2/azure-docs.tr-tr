---
title: Windows sanal masaüstü-Azure 'da Windows 10 VM 'lerine dil paketleri yüklemesi
description: Windows sanal masaüstü 'nde Windows 10 çoklu oturum VM 'Leri için dil paketleri nasıl yüklenir.
author: Heidilohr
ms.topic: how-to
ms.date: 04/03/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 542163511a1b4fc0acde9b44d73be6ffc042d5d3
ms.sourcegitcommit: 98854e3bd1ab04ce42816cae1892ed0caeedf461
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2020
ms.locfileid: "88008772"
---
# <a name="install-language-packs"></a>Dil paketlerini yükleme

Windows sanal masaüstü dağıtımlarını uluslararası olarak ayarlarken, dağıtımınızın birden çok dili desteklediğinden emin olmak iyi bir fikirdir. Kuruluşunuzun ihtiyaç duyacağı sayıda dili desteklemek için bir Windows 10 Enterprise multi-session sanal makinesi (VM) görüntüsüne dil paketleri yükleyebilirsiniz. Bu makalede, kullanıcılarınızın kendi görüntüleme dillerini seçmesini sağlayan dil paketlerini ve yakalama görüntülerini nasıl yükleyeceğiniz anlatılmaktadır.

Azure 'da bir sanal makine dağıtma hakkında daha fazla bilgi için [, Azure Portal ile bir kullanılabilirlik alanında Windows sanal makinesi oluşturun](../virtual-machines/windows/create-portal-availability-zone.md).

>[!NOTE]
>Bu makale, Windows 10 Enterprise çoklu oturum VM 'Leri için geçerlidir.

## <a name="install-a-language-pack"></a>Dil paketi yükler

Dil paketleriyle bir VM görüntüsü oluşturmak için, önce dil paketlerini bir makineye yüklemeniz ve bir görüntüsünü yakalamanız gerekir.

Dil paketlerini yüklemek için:

1. Yönetici olarak oturum açın.
2. En son Windows ve Windows Mağazası güncelleştirmelerini yüklediğinizden emin olun.
3. **Ayarlar**  >  **zaman & dil**  >  **bölgesine**gidin.
4. **Ülke veya bölge**altında, açılır menüden tercih ettiğiniz ülkeyi veya bölgeyi seçin.
    Bu örnekte, aşağıdaki ekran görüntüsünde gösterildiği gibi **Fransa**seçeceğiz:

    > [!div class="mx-imgBorder"]
    > ![Bölge sayfasının ekran görüntüsü. Şu anda seçili bölge Fransa ' dır.](media/region-page-france.png)

5. Bundan sonra **dil**' i seçin ve ardından **Dil ekle**' yi seçin. Listeden yüklemek istediğiniz dili seçin ve ardından **İleri**' yi seçin.
6. **Dil özelliklerini yükler** penceresi açıldığında, **dil paketini yükler ve Windows görüntüleme dili olarak ayarla**etiketli onay kutusunu seçin.
7. **Yükle**’yi seçin.
8. Aynı anda birden çok dil eklemek için **Dil ekle**' yi seçin ve ardından 5 ve 6. adımlarda bir dil eklemek için işlemi tekrarlayın. Yüklemek istediğiniz her dil için bu işlemi tekrarlayın. Ancak, tek seferde yalnızca bir dil görüntüleme diliniz olarak ayarlayabilirsiniz.

    Hızlı bir görsel tanıtım aracılığıyla çalıştıralım. Aşağıdaki resimlerde, Fransızca ve Felemenkçe dil paketlerinin nasıl yükleneceği gösterilmektedir ve ardından, görüntüleme dili olarak Fransızca ayarlanır.

    > [!div class="mx-imgBorder"]
    > ![İşlemin başındaki dil sayfasının ekran görüntüsü. Seçilen Windows görüntüleme dili Ingilizce.](media/language-page-default.png)

    > [!div class="mx-imgBorder"]
    > ![Dil seçimi penceresinin ekran görüntüsü. Kullanıcı, Fransızca dil paketlerini bulmak için arama çubuğuna "Fransızca" girdi.](media/select-language-french.png)

    > [!div class="mx-imgBorder"]
    > ![Dil özelliklerini yüklemeyi sayfasının ekran görüntüsü. Fransızca tercih edilen dil olarak seçilidir. Seçilen seçenekler "görüntüleme dilmi ayarla", "dil paketini yükler," "konuşma tanıma" ve "el yazısı" dir.](media/install-language-features.png)

    Dil Paketleriniz yüklendikten sonra dil paketlerinde dil paketlerinizin adlarının göründüğünü görmeniz gerekir.

    > [!div class="mx-imgBorder"]
    > ![Yeni dil paketlerinin yüklü olduğu dil sayfasının ekran görüntüsü. Fransızca ve Hollanda dil paketleri "tercih edilen diller" altında listelenmiştir.](media/language-page-complete.png)

9. Oturumunuz oturumunuzu kapatmak isteyip istemediğinizi soran bir pencere görünürse. Oturumu kapatın ve yeniden oturum açın. Görüntüleme diliniz artık seçtiğiniz dilde olmalıdır.

10.  **Denetim Masası**  >  **saat ve bölge**  >  **bölgesi**' ne gidin.

11.  **Bölge** penceresi açıldığında, **Yönetim** sekmesini seçin ve ardından **Ayarları Kopyala**' yı seçin.

12.  **Hoş geldiniz ekranı ve sistem hesapları** ve **Yeni Kullanıcı hesapları**etiketli onay kutularını seçin.

13.  **Tamam**’ı seçin.

14.  Bir pencere açılır ve oturumunuzu yeniden başlatmanızı söyleyecektir. **Şimdi yeniden Başlat**' ı seçin.

15.  Oturum açtıktan sonra, **Denetim Masası**  >  **saat ve bölge**  >  **bölgesi**' ne geri dönün.

16.  **Yönetim** sekmesini seçin.

17.  **Sistem yerel ayarını değiştir**' i seçin.

18. **Geçerli sistem yerel ayarı**altındaki açılan menüde, kullanmak istediğiniz yerel ayar dilini seçin. Bundan sonra **Tamam**' ı seçin.

19. Oturumunuzu bir kez daha yeniden başlatmak için **Şimdi yeniden Başlat** ' ı seçin.

Tebrikler, dil paketlerinizi yüklediniz!

Devam etmeden önce, sisteminizde en son Windows ve Windows Mağazası sürümlerinin yüklü olduğundan emin olun.

## <a name="sysprep"></a>Sysprep

Daha sonra, görüntü yakalama işlemine hazırlamak için makinenize Sysprep uygulamanız gerekir.

Makinenize Sysprep eklemek için:

1. Yönetici olarak PowerShell’i açın.
2. Doğru dizine gitmek için aşağıdaki cmdlet 'i çalıştırın:

    ```powershell
    cd Windows\System32\Sysprep
    ```

3. Sonra, aşağıdaki cmdlet 'i çalıştırın:

    ```powershell
    .\sysprep.exe
    ```

4. Sistem Hazırlama aracı penceresi açıldığında, **generalize**etiketli onay kutusunu seçin, ardından **kapatma seçenekleri** ' ne gidin ve açılan menüden **Kapat** ' ı seçin.

>[!NOTE]
>Syprep işleminin tamamlanması birkaç dakika sürer. VM kapandığı için uzak oturumunuzun bağlantısı kesilecek.

### <a name="resolve-sysprep-errors"></a>Sysprep hatalarını çözümle

Sysprep işlemi sırasında bir hata iletisi görürseniz şunları yapmanız gerekir:

1. **C sürücüsünü** açın ve **Windows**  >  **System32 Sysprep**  >  **Panther**adresine gidip **Setuperr** dosyasını açın.

   Hata dosyasındaki metinde, aşağıdaki görüntüde gösterildiği gibi belirli bir dil paketini kaldırmanız gerektiğini söyleyecektir. Sonraki adım için dil paketi adını kopyalayın.

   > [!div class="mx-imgBorder"]
   > ![Setuperr dosyasının ekran görüntüsü. Paket adına sahip metin koyu mavi renkle vurgulanır.](media/setuperr-package-name.png)

2. Yeni bir PowerShell penceresi açın ve dil paketini kaldırmak için adım 2 ' de kopyaladığınız paket adıyla aşağıdaki cmdlet 'i çalıştırın:

   ```powershell
   Remove-AppxPackage <package name>
   ```

3. Cmdlet 'i yeniden çalıştırarak paketi kaldırdığınızdan emin olun `Remove-AppxPackage` . Paketi başarıyla kaldırdıysanız, kaldırmaya çalıştığınız paketin orada olmadığını belirten bir ileti görmeniz gerekir.

4. `sysprep.exe`Cmdlet 'i yeniden çalıştırın.

## <a name="capture-the-image"></a>Görüntüyü yakala

Sisteminiz hazır olduğuna göre, diğer kullanıcıların yapılandırma işlemini yinelemek zorunda kalmadan sistem sanal makinelerini kullanmaya başlayabilmesi için bir görüntü yakalayabilirsiniz.

Bir görüntü yakalamak için:

1. Azure portal gidin ve [bir dil paketi](#install-a-language-pack) ve [Sysprep](#sysprep)yüklerken yapılandırdığınız makinenin adını seçin.

2. **Yakala**' yı seçin.

3. **Ad** alanına görüntünüz için bir ad girin ve aşağıdaki görüntüde gösterildiği gibi **kaynak grubu** açılır menüsünü kullanarak kaynak grubuna atayın.

   > [!div class="mx-imgBorder"]
   > ![Görüntü oluştur penceresinin ekran görüntüsü. Kullanıcının bu test görüntüsüne verdiği ad "vmwvd-Image-fr" dır ve bunu "testwvdimagerg" kaynak grubuna atamıştır.](media/create-image.png)

4. **Oluştur**’u seçin.

5. Yakalama işleminin tamamlanabilmesi için birkaç dakika bekleyin. Görüntü hazırsa, bildirim merkezinde görüntünün yakalandığını bildiren bir ileti görmeniz gerekir.

Artık yeni görüntünüzü kullanarak bir VM dağıtabilirsiniz. VM 'yi dağıtırken, [Azure Portal bir kullanılabilirlik alanında Windows sanal makinesi oluşturma](../virtual-machines/windows/create-portal-availability-zone.md)bölümündeki yönergeleri izlediğinizden emin olun.

### <a name="how-to-change-display-language-for-standard-users"></a>Standart kullanıcılar için görüntüleme dilini değiştirme

Standart kullanıcılar, VM 'lerinde görüntüleme dilini değiştirebilir.

Görüntüleme dilini değiştirmek için:

1. **Dil ayarları**' na gidin. Nerede olduğunu bilmiyorsanız, başlangıç menüsündeki arama çubuğuna **dil** girebilirsiniz.

2. Windows görüntüleme dili açılan menüsünde, görüntüleme diliniz olarak kullanmak istediğiniz dili seçin.

3. Oturumunuzu kapatın ve yeniden oturum açın. Görüntüleme dili artık adım 2 ' de seçtiğiniz bir görünüm olmalıdır.
