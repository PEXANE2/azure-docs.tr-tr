---
title: Azure Data Box Gateway serisi cihaza güncelleştirme yüklemeyi | Microsoft Docs
description: Azure Data Box Gateway serisi cihaz için Azure portal ve yerel Web Kullanıcı arabirimini kullanarak güncelleştirmelerin nasıl uygulanacağını açıklar.
services: databox
author: alkohli
ms.service: databox
ms.topic: article
ms.date: 06/30/2020
ms.author: alkohli
ms.openlocfilehash: 1b3f0faa2b5f67a23317935f0ad868e3872cf86e
ms.sourcegitcommit: 814778c54b59169c5899199aeaa59158ab67cf44
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2020
ms.locfileid: "90055972"
---
# <a name="update-your-azure-data-box-gateway"></a>Azure Data Box Gateway güncelleştirin

Bu makalede, Azure Data Box Gateway yerel Web Kullanıcı arabirimi üzerinden ve Azure portal aracılığıyla güncelleştirme yüklemek için gereken adımlar açıklanmaktadır. Data Box Gateway cihazınızı güncel tutmak için yazılım güncelleştirmelerini veya düzeltmeleri uygularsınız.

> [!IMPORTANT]
>
> - Güncelleştirme **1911** , cihazınızdaki **1.6.1049.786** yazılım sürümüne karşılık gelir. Bu güncelleştirme hakkında daha fazla bilgi için, [sürüm notları](data-box-gateway-1911-release-notes.md)' na gidin.
>
> - Güncelleştirme veya düzeltme yüklendiğinde cihazınızın yeniden başlatıldığını aklınızda bulundurun. Data Box Gateway'in tek düğümlü bir cihaz olduğu düşünüldüğünde devam eden her G/Ç kesintiye uğrar ve cihaz yazılım güncelleştirmesi için cihazınızda 30 dakikaya varan bir kapalı kalma süresi yaşanır.

Bu adımların her biri aşağıdaki bölümlerde açıklanmıştır.

## <a name="use-the-azure-portal"></a>Azure portalı kullanma

Azure portal aracılığıyla güncelleştirmeleri yüklemenizi öneririz. Cihaz, güncelleştirmeleri günde bir kez otomatik olarak tarar. Güncelleştirmeler kullanılabilir olduktan sonra portalda bir bildirim görürsünüz. Daha sonra güncelleştirmeleri indirebilir ve yükleyebilirsiniz.

> [!NOTE]
> Güncelleştirmeleri yüklemeye devam etmeden önce cihazın sağlıklı ve durum ' un **çevrimiçi** olarak göründüğünden emin olun.

1. Cihazınız için güncelleştirmeler kullanılabilir olduğunda bir bildirim görürsünüz. Bildirimi veya üstteki komut çubuğundan **cihazı Güncelleştir**' i seçin. Bu, cihaz yazılım güncelleştirmelerini uygulamanıza olanak tanır.

    ![Güncelleştirmeden sonra yazılım sürümü](./media/data-box-gateway-apply-updates/portal-apply-update-01a.png)

2. **Cihaz güncelleştirmeleri** dikey penceresinde, sürüm notlarındaki yeni özelliklerle ilişkili lisans koşullarını gözden geçirdiğinizden emin olun.

    Güncelleştirmeleri **indirip yüklemeyi** veya yalnızca güncelleştirmeleri **indirmeyi** seçebilirsiniz. Daha sonra da bu güncelleştirmeleri yükleyebilirsiniz.

    ![Güncelleştirmeden sonra yazılım sürümü](./media/data-box-gateway-apply-updates/portal-apply-update-02.png)

    Güncelleştirmeleri indirmek ve yüklemek isterseniz, yükleme tamamlandıktan sonra güncelleştirmelerin otomatik olarak yüklenmesi seçeneğini işaretleyin.

    ![Güncelleştirmeden sonra yazılım sürümü](./media/data-box-gateway-apply-updates/portal-apply-update-03.png)

3. Güncelleştirmelerin indirilmesi başlar. İndirmenin devam ettiğini belirten bir bildirim görürsünüz.

    ![Güncelleştirmeden sonra yazılım sürümü](./media/data-box-gateway-apply-updates/portal-apply-update-05.png)

    Azure portal bir bildirim başlığı da görüntülenir. Bu, indirme ilerleme durumunu gösterir.

    ![Güncelleştirmeden sonra yazılım sürümü](./media/data-box-gateway-apply-updates/portal-apply-update-08a.png)

    Bu bildirimi seçebilir veya güncelleştirmenin ayrıntılı durumunu görmek için **cihazı Güncelleştir** ' i seçebilirsiniz.

    ![Güncelleştirmeden sonra yazılım sürümü](./media/data-box-gateway-apply-updates/portal-apply-update-09.png)

4. İndirme işlemi tamamlandıktan sonra, tamamlanma olduğunu göstermek için bildirim başlığı güncellenir. Güncelleştirmeleri indirip yüklemeyi seçerseniz, yükleme otomatik olarak başlatılır.

    ![Güncelleştirmeden sonra yazılım sürümü](./media/data-box-gateway-apply-updates/portal-apply-update-10a.png)

    Yalnızca güncelleştirmeleri indirmeyi seçerseniz, **cihaz güncelleştirmeleri** dikey penceresini açmak için bildirimi seçin. **Yükle**'yi seçin.
  
    ![Güncelleştirmeden sonra yazılım sürümü](./media/data-box-gateway-apply-updates/portal-apply-update-11a.png)

5. Yüklemenin devam ettiğini belirten bir bildirim görürsünüz.

    ![Güncelleştirmeden sonra yazılım sürümü](./media/data-box-gateway-apply-updates/portal-apply-update-12a.png)

    Portal ayrıca yüklemenin devam ettiğini göstermek için bir bilgilendirme uyarısı görüntüler. <!-- The device goes offline and is in maintenance mode.-->

    <!-- ![Software version after update](./media/data-box-gateway-apply-updates/update-13.png)-->

6. Bu 1 düğümlü bir cihaz olduğundan, güncelleştirmeler yüklendikten sonra cihaz yeniden başlatılır. Yeniden başlatma işlemi sırasında kritik uyarı, cihaz sinyalinin kaybolduğunu gösterir.

    ![Güncelleştirmeden sonra yazılım sürümü](./media/data-box-gateway-apply-updates/portal-apply-update-19a.png)

    İlgili cihaz olayını görmek için uyarıyı seçin.

    ![Güncelleştirmeden sonra yazılım sürümü](./media/data-box-gateway-apply-updates/portal-apply-update-20a.png)

7. Güncelleştirmeler yüklendikten sonra cihaz durumu **çevrimiçi** olarak güncelleştirilir.

    ![Güncelleştirmeden sonra yazılım sürümü](./media/data-box-gateway-apply-updates/portal-apply-update-23a.png)

    Üstteki komut çubuğundan **cihaz güncelleştirmeleri**' ni seçin. Güncelleştirmenin başarıyla yüklendiğini ve cihaz yazılımı sürümünün bu işlemi yansıttığını doğrulayın.

    ![Güncelleştirmeden sonra yazılım sürümü](./media/data-box-gateway-apply-updates/portal-apply-update-24.png)

## <a name="use-the-local-web-ui"></a>Yerel Web Kullanıcı arabirimini kullanma

Yerel Web Kullanıcı arabirimi kullanılırken iki adım vardır:

- Güncelleştirmeyi veya düzeltmeyi indirin
- Güncelleştirmeyi veya düzeltmeyi yükler

Bu adımların her biri, aşağıdaki bölümlerde ayrıntılı olarak açıklanmıştır.

### <a name="download-the-update-or-the-hotfix"></a>Güncelleştirmeyi veya düzeltmeyi indirin

Güncelleştirmeyi indirmek için aşağıdaki adımları gerçekleştirin. Güncelleştirmeyi Microsoft tarafından sağlanan konumdan veya Microsoft Update kataloğundan indirebilirsiniz.

Microsoft Update kataloğundan güncelleştirmeyi indirmek için aşağıdaki adımları uygulayın.

1. Tarayıcıyı başlatın ve adresine gidin [https://catalog.update.microsoft.com](https://catalog.update.microsoft.com) .

   ![Katalogda arama](./media/data-box-gateway-apply-updates/download-update-1.png)

2. Microsoft Update kataloğunun arama kutusuna, indirmek istediğiniz güncelleştirme için düzeltme veya koşulların Bilgi Bankası (KB) numarasını girin. Örneğin, **Azure Data Box Gateway**girin ve ardından **Ara**' ya tıklayın.

   Güncelleştirme listesi **Azure Data Box Gateway 1911**olarak görüntülenir.

   ![Katalogda arama](./media/data-box-gateway-apply-updates/download-update-2.png)

3. **İndir**'i seçin. İndirmek için, aygıt yazılım güncelleştirmesine karşılık gelen *SoftwareUpdatePackage.exe* adlı tek bir dosya vardır. Dosyayı yerel sistemdeki bir klasöre indirin. Ayrıca, klasörü cihazdan erişilebilen bir ağ paylaşımında da kopyalayabilirsiniz.

   ![Katalogda arama](./media/data-box-gateway-apply-updates/download-update-3.png)

### <a name="install-the-update-or-the-hotfix"></a>Güncelleştirmeyi veya düzeltmeyi yükler

Güncelleştirme veya düzeltme yüklemesinden önce şunları yaptığınızdan emin olun:

- Güncelleştirme veya düzeltme, ana bilgisayarınızda yerel olarak indirilir veya bir ağ paylaşımından erişilebilir.
- Yerel Web Kullanıcı arabiriminin **genel bakış** sayfasında gösterildiği gibi cihazınızın durumu sağlıklı olur.

   ![cihaz güncelleştirme](./media/data-box-gateway-apply-updates/local-ui-update-1.png)

Bu yordamın tamamlandığı 20 dakika sürer. Güncelleştirmeyi veya düzeltmeyi yüklemek için aşağıdaki adımları gerçekleştirin.

1. Yerel Web Kullanıcı arabiriminde **bakım**  >  **yazılım güncelleştirmesi**' ne gidin. Çalıştırdığınız yazılım sürümünü bir yere unutmayın.

   ![cihaz güncelleştirme](./media/data-box-gateway-apply-updates/local-ui-update-2.png)

2. Güncelleştirme dosyasının yolunu belirtin. Ayrıca, bir ağ paylaşımında yer alıyorsa güncelleştirme yükleme dosyasına da gidebilirsiniz. *SoftwareUpdatePackage.exe* sonekine sahip yazılım güncelleştirme dosyasını seçin.

   ![cihaz güncelleştirme](./media/data-box-gateway-apply-updates/local-ui-update-3.png)

3. **Uygula**’yı seçin.

   ![cihaz güncelleştirme](./media/data-box-gateway-apply-updates/local-ui-update-4.png)

4. Onaylamanız istendiğinde, devam etmek için **Evet** ' i seçin. Cihaz tek düğümlü bir cihaz olduğundan, güncelleştirme uygulandıktan sonra cihaz yeniden başlatılır ve kapalı kalma süresi vardır.
   ![cihazı Güncelleştir](./media/data-box-gateway-apply-updates/local-ui-update-5.png)

5. Güncelleştirme başlar. Cihaz başarıyla güncelleştirildikten sonra yeniden başlatılır. Yerel Kullanıcı arabirimine bu süre içinde erişilemiyor.

6. Yeniden başlatma işlemi tamamlandıktan sonra **oturum açma** sayfasına yönlendirilirsiniz. Cihaz yazılımının güncelleştirildiğini doğrulamak için, yerel Web Kullanıcı arabiriminde **bakım**  >  **yazılım güncelleştirmesi**' ne gidin. Bu örnekteki görüntülenmiş yazılım sürümü **1.6.1049.786**' dir.

   ![cihaz güncelleştirme](./media/data-box-gateway-apply-updates/local-ui-update-6.png)

## <a name="next-steps"></a>Sonraki adımlar

[Azure Data Box Gateway yönetme](data-box-gateway-manage-users.md)hakkında daha fazla bilgi edinin.
