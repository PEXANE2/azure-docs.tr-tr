---
title: Bölge genelinde hatadan kurtarma
description: Azure App Service iş sürekliliği ve olağanüstü durum kurtarma (BCDR) yeteneklerini korumanıza nasıl yardımcı olduğunu öğrenin. Uygulamanızı Azure 'da bölge genelinde bir hatadan kurtarın.
ms.topic: how-to
ms.date: 06/09/2020
ms.custom: subject-moving-resources
ms.openlocfilehash: 1388dc11254324f74efcbaa55c97cac2ccd0c026
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87073740"
---
# <a name="move-an-app-service-app-to-another-region"></a>App Service uygulamasını başka bir bölgeye taşıma

Bu makalede, Azure bölgesini tümüyle etkileyen bir olağanüstü durum sırasında App Service kaynaklarını farklı bir Azure bölgesinde yeniden çevrimiçi hale getirme açıklanmaktadır. Bir olağanüstü durum, bir Azure bölgesinin tamamını çevrimdışı hale getirirse, bu bölgede barındırılan tüm App Service uygulamaları olağanüstü durum kurtarma moduna alınır. Uygulamayı farklı bir bölgeye geri yüklemenize veya etkilenen uygulamadan dosyaları kurtarmanızı sağlayacak özellikler mevcuttur.

App Service kaynaklar bölgeye özeldir ve bölgeler arasında taşınamaz. Uygulamayı farklı bir bölgedeki yeni bir uygulamaya geri yüklemeniz ve sonra yeni uygulama için yansıtma yapılandırması veya kaynaklar oluşturmanız gerekir.

## <a name="prerequisites"></a>Önkoşullar

- Yok. [Anlık görüntüden geri yükleme](app-service-web-restore-snapshots.md) , genellikle **Premium** katman gerektirir, ancak olağanüstü durum kurtarma modunda, etkilenen uygulamanın katmanı hangi katmanda olursa olsun, etkilenen uygulamanız için otomatik olarak etkinleştirilir.

## <a name="prepare"></a>Hazırlama

Etkilenen uygulamanın Şu anda kullandığı tüm App Service kaynaklarını belirler. Örneğin:

- App Service uygulamaları
- [App Service planları](overview-hosting-plans.md)
- [Dağıtım yuvaları](deploy-staging-slots.md)
- [Azure 'da satın alınan özel etki alanları](manage-custom-dns-buy-domain.md)
- [SSL sertifikaları](configure-ssl-certificate.md)
- [Azure sanal ağ tümleştirmesi](web-sites-integrate-with-vnet.md)
- [Karma bağlantılar](app-service-hybrid-connections.md).
- [Yönetilen kimlikler](overview-managed-identity.md)
- [Yedekleme ayarları](manage-backup.md)

İçeri aktarılan sertifikalar veya karma bağlantılar gibi belirli kaynaklar, diğer Azure hizmetleriyle tümleştirmeyi içerir. Bu kaynakları bölgeler arasında taşıma hakkında daha fazla bilgi için ilgili hizmetlere yönelik belgelere bakın.

## <a name="restore-app-to-a-different-region"></a>Uygulamayı farklı bir bölgeye geri yükleme

1. Etkilenen uygulamadan *farklı* bir Azure bölgesinde yeni bir App Service uygulaması oluşturun. Bu, olağanüstü durum kurtarma senaryosunda hedef uygulamadır.

1. [Azure Portal](https://portal.azure.com), etkilenen uygulamanın yönetim sayfasına gidin. Başarısız bir Azure bölgesinde, etkilenen uygulama bir uyarı metni gösterir. Uyarı metnine tıklayın.

    ![Etkilenen uygulamanın sayfasının ekran görüntüsü. Durumu açıklayan ve uygulamayı geri yüklemek için bir bağlantı sağlayan bir uyarı bildirimi görünür.](media/manage-disaster-recovery/restore-start.png)

1. **Yedeği geri yükle** sayfasında, geri yükleme işlemini aşağıdaki tabloya göre yapılandırın. İşiniz bittiğinde **Tamam**' a tıklayın.

   | Ayar | Değer | Açıklama |
   |-|-|-|
   | **Anlık görüntü (Önizleme)** | Bir anlık görüntü seçin. | En son iki anlık görüntü kullanılabilir. |
   | **Hedefi geri yükle** | **Var olan uygulama** | Aşağıdaki nota tıklayarak **geri yükleme hedef uygulamasını değiştirebilir** ve hedef uygulamayı seçebilirsiniz. Bir olağanüstü durum senaryosunda, anlık görüntüyü yalnızca farklı bir Azure bölgesindeki bir uygulamaya geri yükleyebilirsiniz. |
   | **Site yapılandırmasını geri yükle** | **Evet** | |

    ![Yedekleme geri yükleme sayfasının ekran görüntüsü. Belirli bir anlık görüntü, önceki tablonun listeleceği seçenekler ve Tamam düğmesi vurgulanır.](media/manage-disaster-recovery/restore-configure.png)

3. Etkilenen uygulamayı yansıtmak için Hedef uygulamadaki [diğer her şeyi](#prepare) yapılandırın ve yapılandırmanızı doğrulayın.

4. Özel etki alanı için hedef uygulamayı işaret etmeye hazırsanız, [etki alanı adını yeniden eşleyin](manage-custom-dns-migrate-domain.md#remap-the-active-dns-name).

## <a name="recover-app-content-only"></a>Yalnızca uygulama içeriğini kurtar

Dosyaları yalnızca etkilenen uygulamadan geri yüklemeden kurtarmak isterseniz, aşağıdaki adımları kullanın:

1. [Azure Portal](https://portal.azure.com), etkilenen uygulamanın yönetim sayfasına gidin ve **Yayımlama profilini al**' a tıklayın.

    ![Etkilenen uygulamanın sayfasının ekran görüntüsü. Bir uyarı bildirimi görünür ancak vurgulanmaz. Bunun yerine yayımlama profili Al öğesi vurgulanır.](media/manage-disaster-recovery/get-publish-profile.png)

1. İndirilen dosyayı açın ve adında bulunduğu yayımlama profilini bulun `ReadOnly - FTP` . Bu, olağanüstü durum kurtarma profilidir. Örneğin:

    ```xml
    <publishProfile profileName="%app-name% - ReadOnly - FTP" publishMethod="FTP" publishUrl="ftp://%ftp-site%/site/wwwroot" ftpPassiveMode="True" userName="%app-name%\$%app-name%" userPWD="" destinationAppUrl="http://%app-name%.azurewebsites.net" SQLServerDBConnectionString="" mySQLDBConnectionString="" hostingProviderForumLink="" controlPanelLink="http://windows.azure.com" webSystem="WebSites">
        <databases />
    </publishProfile>
    ```
    
    Üç öznitelik değerini kopyalayın: 
        
    - `publishUrl`: FTP ana bilgisayar adı
    - `userName`ve `userPWD` : FTP kimlik bilgileri

1. Seçtiğiniz FTP istemcisini kullanın, ana bilgisayar adı ve kimlik bilgilerini kullanarak etkilenen uygulamanın FTP ana bilgisayarına bağlanın.

1. Bağlandıktan sonra tüm */site/Wwwroot* klasörünü indirin. Aşağıdaki ekran görüntüsünde, [FileZilla](https://filezilla-project.org/)' de nasıl indirileceği gösterilmektedir.

    ![Bir FileZilla dosyası hiyerarşisinin ekran görüntüsü. Wwwroot klasörü vurgulanır ve kısayol menüsü görünür olur. Bu menüde, Indirme vurgulanır.](media/manage-disaster-recovery/download-content.png)

## <a name="next-steps"></a>Sonraki adımlar
[Azure 'da bir uygulamayı bir anlık görüntüden geri yükleme](app-service-web-restore-snapshots.md)
