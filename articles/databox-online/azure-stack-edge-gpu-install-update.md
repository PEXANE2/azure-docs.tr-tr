---
title: Azure Stack Edge GPU cihazına güncelleştirme yüklemeyi | Microsoft Docs
description: Azure Stack Edge GPU cihazı için Azure portal ve yerel Web Kullanıcı arabirimini kullanarak güncelleştirmelerin nasıl uygulanacağını açıklar.
services: databox
author: alkohli
ms.service: databox
ms.topic: article
ms.date: 06/12/2020
ms.author: alkohli
ms.openlocfilehash: 0e973e41493b8e84f31a82d5379a01a0e851a48d
ms.sourcegitcommit: 656c0c38cf550327a9ee10cc936029378bc7b5a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89086997"
---
# <a name="update-your-azure-stack-edge-with-gpu"></a>Azure Stack Edge 'i GPU ile güncelleştirme 

Bu makalede, yerel Web Kullanıcı arabirimi aracılığıyla ve Azure portal aracılığıyla Azure Stack Edge ile güncelleştirme yüklemek için gereken adımlar açıklanmaktadır. Azure Stack Edge cihazınızı güncel tutmak için yazılım güncelleştirmelerini veya düzeltmeleri uygularsınız. 

> [!IMPORTANT]
> - Güncelleştirme **2008** , cihazınızdaki **2.1.1328.1904** yazılım sürümüne karşılık gelir. Bu güncelleştirme hakkında daha fazla bilgi için, [sürüm notları](azure-stack-edge-gpu-2008-release-notes.md)' na gidin.
>
> - Bir güncelleştirme veya Düzeltme yüklemenin cihazınızı yeniden başlatdığını aklınızda bulundurun. Bu güncelleştirme, iki güncelleştirmeyi sırayla uygulamanızı gerektirir. Önce cihaz yazılım güncelleştirmelerini ve sonra Kubernetes güncelleştirmelerini uygularsınız. Azure Stack Edge tek düğümlü bir cihaz olduğundan, sürmekte olan g/ç miktarı bozulur ve cihazınız, cihaz yazılım güncelleştirmesi için 30 dakikaya varan bir kesinti yaşar.

Güncelleştirmeleri cihazınıza yüklemek için önce güncelleştirme sunucusunun konumunu yapılandırmanız gerekir. Güncelleştirme sunucusu yapılandırıldıktan sonra, Azure portal Kullanıcı arabirimi veya yerel Web Kullanıcı arabirimi aracılığıyla güncelleştirmeleri uygulayabilirsiniz.

Bu adımların her biri aşağıdaki bölümlerde açıklanmıştır.

## <a name="configure-update-server"></a>Güncelleştirme sunucusunu yapılandır

1. Yerel Web Kullanıcı arabiriminde, **yapılandırma**  >  **güncelleştirme sunucusu**' na gidin. 
   
    ![Güncelleştirmeleri Yapılandır](./media/azure-stack-edge-gpu-install-update/configure-update-server-1.png)

2. **Güncelleştirme sunucu türünü seç**' de, açılan listeden Microsoft Update sunucusu (varsayılan) veya Windows Server Update Services seçeneklerinden birini belirleyin.  
   
    Windows Server Update Services 'den güncelleştirme yapıyorsanız, sunucu URI 'sini belirtin. Bu URI 'deki sunucu, güncelleştirmeleri bu sunucuya bağlı tüm cihazlara dağıtır.

    ![Güncelleştirmeleri Yapılandır](./media/azure-stack-edge-gpu-install-update/configure-update-server-2.png)
    
    WSUS sunucusu, güncelleştirmeleri bir yönetim konsolu aracılığıyla yönetmek ve dağıtmak için kullanılır. WSUS sunucusu, kuruluştaki diğer WSUS sunucuları için güncelleştirme kaynağı da olabilir. Bir güncelleştirme kaynağı görevi gören WSUS sunucusu yukarı akış sunucusu şeklinde adlandırılır. Bir WSUS uygulamasında, ağınızdaki en az bir WSUS sunucusu kullanılabilir güncelleştirme bilgilerini almak için Microsoft Update bağlanabilmelidir. Yönetici olarak, ağ güvenliğine ve yapılandırmaya göre (diğer WSUS sunucularının kaç tane Microsoft Update doğrudan bağlanmadığını belirleyebilirsiniz.
    
    Daha fazla bilgi için [Windows Server Update Services (WSUS)](https://docs.microsoft.com/windows-server/administration/windows-server-update-services/get-started/windows-server-update-services-wsus) sayfasına gidin

## <a name="use-the-azure-portal"></a>Azure portalını kullanma

Azure portal aracılığıyla güncelleştirmeleri yüklemenizi öneririz. Cihaz, güncelleştirmeleri günde bir kez otomatik olarak tarar. Güncelleştirmeler kullanılabilir olduktan sonra portalda bir bildirim görürsünüz. Daha sonra güncelleştirmeleri indirebilir ve yükleyebilirsiniz. 

> [!NOTE]
> Güncelleştirmeleri yüklemeye devam etmeden önce cihazın sağlıklı ve durum ' un **çevrimiçi** olarak göründüğünden emin olun.

1. Cihazınız için güncelleştirmeler kullanılabilir olduğunda bir bildirim görürsünüz. Bildirimi veya üstteki komut çubuğundan **cihazı Güncelleştir**' i seçin. Bu, cihaz yazılım güncelleştirmelerini uygulamanıza olanak tanır.

    ![Güncelleştirmeden sonra yazılım sürümü](./media/azure-stack-edge-gpu-install-update/portal-update-1.png)

2. **Cihaz güncelleştirmeleri** dikey penceresinde, sürüm notlarındaki yeni özelliklerle ilişkili lisans koşullarını gözden geçirdiğinizden emin olun.

    Güncelleştirmeleri **indirip yüklemeyi** veya yalnızca güncelleştirmeleri **indirmeyi** seçebilirsiniz. Daha sonra bu güncelleştirmeleri yüklemeyi tercih edebilirsiniz.

    ![Güncelleştirmeden sonra yazılım sürümü](./media/azure-stack-edge-gpu-install-update/portal-update-2a.png)    

    Güncelleştirmeleri indirmek ve yüklemek isterseniz, yükleme tamamlandıktan sonra güncelleştirmelerin otomatik olarak yüklenmesi seçeneğini işaretleyin.

    ![Güncelleştirmeden sonra yazılım sürümü](./media/azure-stack-edge-gpu-install-update/portal-update-2b.png)

3. Güncelleştirmelerin indirilmesi başlar. İndirmenin devam ettiğini belirten bir bildirim görürsünüz.

    ![Güncelleştirmeden sonra yazılım sürümü](./media/azure-stack-edge-gpu-install-update/portal-update-3.png)

    Azure portal bir bildirim başlığı da görüntülenir. Bu, indirme ilerleme durumunu gösterir. 

    ![Güncelleştirmeden sonra yazılım sürümü](./media/azure-stack-edge-gpu-install-update/portal-update-4.png)

    Bu bildirimi seçebilir veya güncelleştirmenin ayrıntılı durumunu görmek için **cihazı Güncelleştir** ' i seçebilirsiniz.

    ![Güncelleştirmeden sonra yazılım sürümü](./media/azure-stack-edge-gpu-install-update/portal-update-5.png)   


4. İndirme işlemi tamamlandıktan sonra, tamamlanma olduğunu göstermek için bildirim başlığı güncellenir. Güncelleştirmeleri indirip yüklemeyi seçerseniz, yükleme otomatik olarak başlatılır.

    ![Güncelleştirmeden sonra yazılım sürümü](./media/azure-stack-edge-gpu-install-update/portal-update-6.png)

    Yalnızca güncelleştirmeleri indirmeyi seçerseniz, **cihaz güncelleştirmeleri** dikey penceresini açmak için bildirimi seçin. **Yükle**’yi seçin.
  
    ![Güncelleştirmeden sonra yazılım sürümü](./media/azure-stack-edge-gpu-install-update/portal-update-7.png)

5. Yüklemenin devam ettiğini belirten bir bildirim görürsünüz.

    ![Güncelleştirmeden sonra yazılım sürümü](./media/azure-stack-edge-gpu-install-update/portal-update-8.png)

    Portal ayrıca yüklemenin devam ettiğini göstermek için bir bilgilendirme uyarısı görüntüler. Cihaz çevrimdışı ve bakım modunda.
    
    ![Güncelleştirmeden sonra yazılım sürümü](./media/azure-stack-edge-gpu-install-update/portal-update-9.png)

6. Bu 1 düğümlü bir cihaz olduğundan, güncelleştirmeler yüklendikten sonra cihaz yeniden başlatılır. Yeniden başlatma işlemi sırasında kritik uyarı, cihaz sinyalinin kaybolduğunu gösterir.

    ![Güncelleştirmeden sonra yazılım sürümü](./media/azure-stack-edge-gpu-install-update/portal-update-10.png)

    İlgili cihaz olayını görmek için uyarıyı seçin.
    
    ![Güncelleştirmeden sonra yazılım sürümü](./media/azure-stack-edge-gpu-install-update/portal-update-11.png)


7. Yeniden başlatmadan sonra cihaz yeniden bakım moduna konur ve bunu göstermek için bir bilgi uyarısı görüntülenir.

    En üstteki komut çubuğundan **cihazı Güncelleştir** ' i seçerseniz, güncelleştirmelerin ilerlemesini görebilirsiniz.   

8. Güncelleştirmeler yüklendikten sonra cihaz durumu **çevrimiçi** olarak güncelleştirilir. 

    ![Güncelleştirmeden sonra yazılım sürümü](./media/azure-stack-edge-gpu-install-update/portal-update-14.png)

    Üstteki komut çubuğundan **cihaz güncelleştirmeleri**' ni seçin. Güncelleştirmenin başarıyla yüklendiğini ve cihaz yazılımı sürümünün bu işlemi yansıttığını doğrulayın.

    ![Güncelleştirmeden sonra yazılım sürümü](./media/azure-stack-edge-gpu-install-update/portal-update-15.png)

9. Güncelleştirmelerin kullanılabilir olduğunu bir bildirim görürsünüz. Bunlar Kubernetes güncelleştirmeleridir. Bildirimi seçin veya üstteki komut çubuğundan **cihazı Güncelleştir** ' i seçin.

    ![Güncelleştirmeden sonra yazılım sürümü](./media/azure-stack-edge-gpu-install-update/portal-update-16.png)

10. Kubernetes güncelleştirmelerini indirin. Önceki güncelleştirme paketiyle karşılaştırıldığında paket boyutunun farklı olduğunu görebilirsiniz.

    ![Güncelleştirmeden sonra yazılım sürümü](./media/azure-stack-edge-gpu-install-update/portal-update-17.png)

    Yükleme işlemi, cihaz güncelleştirmeleriyle aynıdır. Önce güncelleştirmeler indirilir.

    ![Güncelleştirmeden sonra yazılım sürümü](./media/azure-stack-edge-gpu-install-update/portal-update-18.png)    
    
11. Güncelleştirmeler indirildikten sonra güncelleştirmeleri yükleyebilirsiniz. 

    ![Güncelleştirmeden sonra yazılım sürümü](./media/azure-stack-edge-gpu-install-update/portal-update-19.png)

    Güncelleştirmeler yüklendiği sürece cihaz, bakım moduna alınır. Cihaz, Kubernetes güncelleştirmeleri için yeniden başlamıyor. 

    Kubernetes güncelleştirmeleri başarıyla yüklendikten sonra, daha fazla güncelleştirme gerekmiyorsa başlık bildirimi kaybolur. Cihazınız artık cihaz yazılımının en son sürümü ve Kubernetes sürümüdür.

    ![Güncelleştirmeden sonra yazılım sürümü](./media/azure-stack-edge-gpu-install-update/portal-update-20.png)


## <a name="use-the-local-web-ui"></a>Yerel Web Kullanıcı arabirimini kullanma

Yerel Web Kullanıcı arabirimi kullanılırken iki adım vardır:

* Güncelleştirmeyi veya düzeltmeyi indirin
* Güncelleştirmeyi veya düzeltmeyi yükler

Bu adımların her biri, aşağıdaki bölümlerde ayrıntılı olarak açıklanmıştır.


### <a name="download-the-update-or-the-hotfix"></a>Güncelleştirmeyi veya düzeltmeyi indirin

Güncelleştirmeyi indirmek için aşağıdaki adımları gerçekleştirin. Güncelleştirmeyi Microsoft tarafından sağlanan konumdan veya Microsoft Update kataloğundan indirebilirsiniz.


Microsoft Update kataloğundan güncelleştirmeyi indirmek için aşağıdaki adımları uygulayın.

1. Tarayıcıyı başlatın ve adresine gidin [https://catalog.update.microsoft.com](https://catalog.update.microsoft.com) .

    ![Katalogda arama](./media/azure-stack-edge-gpu-install-update/download-update-1.png)

2. Microsoft Update kataloğunun arama kutusuna, indirmek istediğiniz güncelleştirme için düzeltme veya koşulların Bilgi Bankası (KB) numarasını girin. Örneğin **Azure Stack Edge**girin ve ardından **Ara**' ya tıklayın.
   
    Güncelleştirme listesi **Azure Stack Edge 2006**olarak görüntülenir.
   
    ![Katalogda arama](./media/azure-stack-edge-gpu-install-update/download-update-2b.png)

4. **İndir**'i seçin. *SoftwareUpdatePackage.exe* ile indirilecek iki dosya vardır ve sırasıyla cihaz yazılım güncelleştirmelerine ve Kubernetes güncelleştirmelerine karşılık gelen *Kubernetes_Package.exe* sonekleri vardır. Dosyaları yerel sistemdeki bir klasöre indirin. Ayrıca, klasörü cihazdan erişilebilen bir ağ paylaşımında da kopyalayabilirsiniz.

### <a name="install-the-update-or-the-hotfix"></a>Güncelleştirmeyi veya düzeltmeyi yükler

Güncelleştirme veya düzeltme yüklemesinden önce şunları yaptığınızdan emin olun:

 - Güncelleştirme veya düzeltme, ana bilgisayarınızda yerel olarak indirilir veya bir ağ paylaşımından erişilebilir.
 - Yerel Web Kullanıcı arabiriminin **genel bakış** sayfasında gösterildiği gibi cihazınızın durumu sağlıklı olur.

   ![cihaz güncelleştirme](./media/azure-stack-edge-gpu-install-update/local-ui-update-1.png) 

Bu yordamın tamamlandığı 20 dakika sürer. Güncelleştirmeyi veya düzeltmeyi yüklemek için aşağıdaki adımları gerçekleştirin.

1. Yerel Web Kullanıcı arabiriminde **bakım**  >  **yazılım güncelleştirmesi**' ne gidin. Çalıştırdığınız yazılım sürümünü bir yere unutmayın. 
   
   ![cihaz güncelleştirme](./media/azure-stack-edge-gpu-install-update/local-ui-update-2.png)

2. Güncelleştirme dosyasının yolunu belirtin. Ayrıca, bir ağ paylaşımında yer alıyorsa güncelleştirme yükleme dosyasına da gidebilirsiniz. *SoftwareUpdatePackage.exe* sonekine sahip yazılım güncelleştirme dosyasını seçin.

   ![cihaz güncelleştirme](./media/azure-stack-edge-gpu-install-update/local-ui-update-3a.png)

3. **Uygula**’yı seçin. 

   ![cihaz güncelleştirme](./media/azure-stack-edge-gpu-install-update/local-ui-update-4.png)

4. Onaylamanız istendiğinde, devam etmek için **Evet** ' i seçin. Cihaz tek düğümlü bir cihaz olduğundan, güncelleştirme uygulandıktan sonra cihaz yeniden başlatılır ve kapalı kalma süresi vardır. 
   
   ![cihaz güncelleştirme](./media/azure-stack-edge-gpu-install-update/local-ui-update-5.png)

5. Güncelleştirme başlar. Cihaz başarıyla güncelleştirildikten sonra yeniden başlatılır. Yerel Kullanıcı arabirimine bu süre içinde erişilemiyor.
   
6. Yeniden başlatma işlemi tamamlandıktan sonra **oturum açma** sayfasına yönlendirilirsiniz. Cihaz yazılımının güncelleştirildiğini doğrulamak için, yerel Web Kullanıcı arabiriminde **bakım**  >  **yazılım güncelleştirmesi**' ne gidin. Bu örnekteki görüntülenmiş yazılım sürümü **2.0.1257.1591**' dir.

   ![cihaz güncelleştirme](./media/azure-stack-edge-gpu-install-update/local-ui-update-6.png) 

7. Şimdi Kubernetes yazılım sürümünü güncelleştecaksınız. Yukarıdaki adımları tekrarlayın. *Kubernetes_Package.exe* sonekine sahip Kubernetes güncelleştirme dosyasına bir yol sağlayın.  

   <!--![update device](./media/azure-stack-edge-gpu-install-update/local-ui-update-7.png)--> 

8. **Uygula**’yı seçin. 

   ![cihaz güncelleştirme](./media/azure-stack-edge-gpu-install-update/local-ui-update-8.png)

9. Onaylamanız istendiğinde, devam etmek için **Evet** ' i seçin. 

10. Kubernetes güncelleştirmesi başarıyla yüklendikten sonra, **bakım**  >  **yazılımı güncelleştirmesinde**, görüntülenmiş yazılımlar ' da bir değişiklik yoktur. 


## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack kenarını yönetme](azure-stack-edge-manage-access-power-connectivity-mode.md)hakkında daha fazla bilgi edinin.
