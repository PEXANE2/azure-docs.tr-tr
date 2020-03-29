---
title: StorSimple Data Manager'da iş başlatmak için Azure Otomasyonu'ndan yararlanın
description: StorSimple Data Manager işlerini tetiklemek için Azure Otomasyonu'nun nasıl kullanılacağını öğrenin
author: alkohli
ms.service: storsimple
ms.topic: conceptual
ms.date: 01/16/2018
ms.author: alkohli
ms.openlocfilehash: 034b4996672f0961cf31d342aa6055482f099b9f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76273985"
---
# <a name="use-azure-automation-to-trigger-a-job"></a>İşi tetiklemek için Azure Otomasyonu'ndan yararlanın

Bu makalede, StorSimple aygıt verilerini dönüştürmek için StorSimple Veri Yöneticisi hizmetindeki veri dönüştürme özelliğini nasıl kullanabileceğiniz açıklanmaktadır. Veri dönüştürme işini iki şekilde başlatabilirsiniz: 

 - .NET SDK’yı kullanma
 - Azure Otomasyon runbook'u kullanma
 
Bu makalede, bir Azure Otomasyon runbook oluşturmak ve daha sonra bir veri dönüştürme işi başlatmak için kullanmak nasıl ayrıntıları. .NET SDK üzerinden veri dönüşümü başlatma hakkında daha fazla bilgi edinmek için [veri dönüşüm işlerini tetiklemek için .NET SDK'yı kullanın.](storsimple-data-manager-dotnet-jobs.md)

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce şunları yaptığınızdan emin olun:

*   Azure PowerShell istemci bilgisayarda yüklendi. [Azure PowerShell'i indirin.](https://docs.microsoft.com/powershell/azure/azurerm/install-azurerm-ps)
*   Kaynak grubu içindeki StorSimple Data Manager hizmetinde doğru yapılandırılmış iş tanımı.
*   Dosyayı GitHub deposundan indirin. [`DataTransformationApp.zip`](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/raw/master/Azure%20Automation%20For%20Data%20Manager/DataTransformationApp.zip) 
*   Komut [`Trigger-DataTransformation-Job.ps1`](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/blob/master/Azure%20Automation%20For%20Data%20Manager/Trigger-DataTransformation-Job.ps1) dosyasını GitHub deposundan indirin.

## <a name="step-by-step-procedure"></a>Adım adım yordam

### <a name="set-up-the-automation-account"></a>Otomasyon hesabını ayarlama

1. Azure portalında bir Azure Çalıştır otomasyon hesabı oluşturun. Bunu yapmak için Azure **pazar > Her Şey'e** gidin ve ardından **Otomasyon'u**arayın. **Otomasyon hesaplarını**seçin.

    ![Otomasyon hesabı olarak Çalıştır'ı oluşturma](./media/storsimple-data-manager-job-using-automation/search-automation-account1.png)

2. Yeni bir otomasyon hesabı eklemek için **+ Ekle'yi**tıklatın.

    ![Otomasyon hesabı olarak Çalıştır'ı oluşturma](./media/storsimple-data-manager-job-using-automation/add-automation-account1.png)

3. Ekle **Otomasyonunda:**

   1. Otomasyon hesabınızın **adını** temin edin.
   2. StorSimple Data Manager hizmetinize bağlı **Abonelik'i** seçin.
   3. Yeni bir kaynak grubu oluşturun veya varolan bir kaynak grubundan seçim yap.
   4. Bir **Konum** seçin.
   5. Varsayılan **Create Run As hesabını** seçili bırakın.
   6. Panoya hızlı erişim için bir bağlantı almak için, **Panoya Pin'i**işaretleyin. **Oluştur'u**tıklatın.

      ![Otomasyon hesabı olarak Çalıştır'ı oluşturma](./media/storsimple-data-manager-job-using-automation/create-automation-run-as-account.png)
    
      Otomasyon hesabı başarıyla oluşturulduktan sonra size bildirilir.
    
      ![Otomasyon hesabının dağıtımına yönelik bildirim](./media/storsimple-data-manager-job-using-automation/deployment-automation-account-notification1.png)

      Daha fazla bilgi [için, Bir Run As hesabı oluşturun'a](../automation/automation-create-runas-account.md)gidin.

3. Yeni oluşturulan hesapta, **Paylaşılan Kaynaklar > Modülleri'ne** gidin ve **+ Ekle modülüne**tıklayın.

    ![İthalat modülü 1](./media/storsimple-data-manager-job-using-automation/import-module-1.png)

4. Yerel bilgisayarınızdan dosyanın `DataTransformationApp.zip` konumuna göz atın ve modülü seçin ve açın. Modülü almak için **Tamam'ı** tıklatın.

    ![İthalat modülü 2](./media/storsimple-data-manager-job-using-automation/import-module-2.png)

   Azure Otomasyon hesabınıza bir modül aktardığında, modül le ilgili meta verileri ayıklar. Bu işlem birkaç dakika sürebilir.

   ![İthalat modülü 4](./media/storsimple-data-manager-job-using-automation/import-module-4.png)

5. Modülün dağıtıldığına dair bir bildirim ve işlem tamamlandığında başka bir bildirim alırsınız.  **Modüller'deki** durum **Kullanılabilir'e**dönüşür.

    ![İthalat modülü 5](./media/storsimple-data-manager-job-using-automation/import-module-5.png)

### <a name="import-publish-and-run-automation-runbook"></a>Otomasyon runbook'u alma, yayımlama ve çalıştırma

İş tanımını tetiklemek için runbook'u almak, yayımlamak ve çalıştırmak için aşağıdaki adımları gerçekleştirin.

1. Azure portalında, Otomasyon hesabınızı açın. **Proses Otomasyonu > Runbook'lara** gidin ve **runbook ekle +'ya**tıklayın.

    ![Runbook 1 ekle](./media/storsimple-data-manager-job-using-automation/add-runbook-1.png)

2. **Runbook**Ekle'de, **varolan bir runbook'u içe aktar'ı**tıklatın.

3. Runbook dosyası için Azure `Trigger-DataTransformation-Job.ps1` PowerShell komut dosyası **dosyasına**işaret edin. Runbook türü otomatik olarak seçilir. Runbook için bir ad ve isteğe bağlı bir açıklama sağlayın. **Oluştur'u**tıklatın.

    ![Runbook 2 ekle](./media/storsimple-data-manager-job-using-automation/add-runbook-2.png)

4. Yeni runbook, Otomasyon hesabının runbook'ları listesinde görünür. Bu runbook'u seçin ve tıklatın.

    ![Runbook 3 ekle](./media/storsimple-data-manager-job-using-automation/add-runbook-3.png)

5. Runbook'u ve **Test** bölmesini tıklatın.

    ![Runbook 4 ekle](./media/storsimple-data-manager-job-using-automation/add-runbook-4.png)

6. StorSimple Data Manager hizmetinizin adı, ilişkili kaynak grubu ve iş tanımı adı gibi parametreleri sağlayın. Testi **başlatın.** Çalışma tamamlandığında rapor oluşturulur. Daha fazla bilgi için, [bir runbook test](../automation/automation-first-runbook-textual-powershell.md#step-3---test-the-runbook)etmek için nasıl gidin.

    ![Runbook 8 ekle](./media/storsimple-data-manager-job-using-automation/add-runbook-8.png)    

7. Test bölmesinde runbook'tan çıktıyı inceleyin. Memnun olursa, bölmeyi kapatın. **Yayımla'yı** ve onay istendiğinde runbook'u onaylayın ve yayımlayın'ı tıklatın.

    ![Runbook 6 ekle](./media/storsimple-data-manager-job-using-automation/add-runbook-6.png)

8. **Runbook'a** geri dön ve yeni oluşturulan runbook'u seçin.

    ![Runbook 7 ekle](./media/storsimple-data-manager-job-using-automation/add-runbook-7.png)

9. Runbook'u **başlatın.** **Başlat**runbook'una, tüm parametreleri girin. Veri dönüştürme işini göndermek ve başlatmak için **Tamam'ı** tıklatın.

10. Azure portalındaki iş ilerlemesini izlemek için StorSimple Data Manager hizmetinizdeki **İşler'e** gidin. İş ayrıntılarını görüntülemek için işi seçin ve tıklatın.

    ![Runbook 10 ekle](./media/storsimple-data-manager-job-using-automation/add-runbook-10.png)

## <a name="next-steps"></a>Sonraki adımlar

[Verilerinizi dönüştürmek için StorSimple Data Manager Kullanıcı UI'ı kullanın.](storsimple-data-manager-ui.md)
