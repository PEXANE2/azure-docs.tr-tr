---
title: Amazon Web Hizmetlerinde VM dağıtımının otomatikleştirilmesi
description: Bu makale, Bir Amazon Web Hizmeti VM'sinin oluşturulmasını otomatikleştirmek için Azure Otomasyonunun nasıl kullanılacağını göstermektedir
services: automation
ms.subservice: process-automation
ms.date: 03/16/2018
ms.topic: conceptual
ms.openlocfilehash: 52a887d2d934aa2f7e13f0c2fdb4332066aee0e7
ms.sourcegitcommit: 5e49f45571aeb1232a3e0bd44725cc17c06d1452
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81604839"
---
# <a name="azure-automation-scenario---provision-an-aws-virtual-machine"></a>Azure Otomasyonu senaryosu - Bir AWS sanal makine sağlama
Bu makalede, Amazon Web Hizmeti (AWS) aboneliğinizde sanal bir makine sağlamak ve bu VM'ye AWS'nin VM'yi "etiketlemek" olarak adlandırdığı belirli bir ad vermek için Azure Otomasyonu'ndan nasıl yararlanabileceğinizi öğrenirsiniz.

## <a name="prerequisites"></a>Ön koşullar
Bir Azure Otomasyon hesabınız ve Bir Amazon Web Hizmetleri (AWS) aboneliğiniz olması gerekir. Azure Otomasyon hesabı açma ve AWS abonelik kimlik bilgilerinizle yapılandırma hakkında daha fazla bilgi [için, Amazon Web Hizmetleri ile Kimlik Doğrulamasını Yapılandır'ı](automation-config-aws-account.md)gözden geçirin. Bu hesap, aşağıdaki bölümlerde bu hesaba atıfta bulunarak, devam etmeden önce AWS abonelik kimlik bilgilerinizle oluşturulmalı veya güncelleştirilmelidir.

## <a name="deploy-amazon-web-services-powershell-module"></a>Amazon Web Hizmetleri PowerShell Modülü dağıtın
VM sağlama runbook'unuz, işini yapmak için AWS PowerShell modülünü kullanır. Modülü AWS abonelik kimlik bilgilerinizle yapılandırılan Otomasyon hesabınıza eklemek için aşağıdaki adımları kullanın.  

1. Web tarayıcınızı açın ve [PowerShell Galerisi'ne](https://www.powershellgallery.com/packages/AWSPowerShell/) gidin ve **Azure Otomasyonuna Dağıt düğmesine**tıklayın.<br><br> ![AWS PS Modül İthalat](./media/automation-scenario-aws-deployment/powershell-gallery-download-awsmodule.png)
2. Azure giriş sayfasına yönlendirilirsiniz ve kimlik doğrulaması yaptıktan sonra Azure portalına yönlendirilir ve aşağıdaki sayfayla birlikte sunulur:<br><br> ![İthalat Modülü sayfası](./media/automation-scenario-aws-deployment/deploy-aws-powershell-module-parameters.png)
3. Kullanmak için Otomasyon hesabını seçin ve dağıtımı başlatmak için **Tamam'ı** tıklatın.

   > [!NOTE]
   > Azure Automation bir PowerShell modülü aldığında cmdlets'i ayıklar. Otomasyon modülü ithal etme ve cmdlets çıkarma tamamen bitmiş olana kadar faaliyetler görünmez. Bu işlem birkaç dakika sürebilir.  
   > <br>

1. Azure portalında, Otomasyon hesabınızı açın.
2. **Varlıklar** döşemesine tıklayın ve Varlıklar bölmesine **Modülleri**seçin.
3. Modüller sayfasında listede **AWSPowerShell** modüllerini görürsünüz.

## <a name="create-aws-deploy-vm-runbook"></a>AWS dağıtım VM runbook oluşturma
AWS PowerShell Modülü dağıtıldıktan sonra, artık PowerShell komut dosyası kullanarak AWS'de sanal bir makineyi otomatikleştirmek için bir runbook yazabilirsiniz. Aşağıdaki adımlar, Azure Otomasyonu'nda yerel PowerShell komut dosyasının nasıl kullanılacağını göstermektedir.  

> [!NOTE]
> Bu komut dosyası yla ilgili daha fazla seçenek ve bilgi için lütfen [PowerShell Gallery'yi](https://www.powershellgallery.com/packages/New-AwsVM/)ziyaret edin.
> 

1. PowerShell'den PowerShell komut dosyası New-AwsVM'i powershell oturumunu açarak ve aşağıdaki komutu yazarak indirin:<br>
   ```powershell
   Save-Script -Name New-AwsVM -Path <path>
   ```
   <br>
2. Azure portalından Otomasyon hesabınızı açın ve **Proses Otomasyonu**altında **Runbook'ları** seçin.  
3. Runbook'lar sayfasından **runbook ekle'yi**seçin.
4. Runbook ekle bölmesine yeni bir runbook oluşturmak için **Hızlı Oluştur'u** seçin.
5. Runbook özellikleri bölmesinde, runbook'unuzun adını yazın.
6. **Runbook türü** açılır listesinden **PowerShell'i**seçin ve ardından **Oluştur'u**tıklatın.<br><br> ![Runbook bölmesi oluşturma](./media/automation-scenario-aws-deployment/runbook-quickcreate-properties.png)
6. PowerShell Runbook sayfasını edit edin, PowerShell komut dosyasını kopyalayın ve kopyalayarak çalışma defterine yapıştırın tuval.<br><br> ![Runbook PowerShell Script](./media/automation-scenario-aws-deployment/runbook-powershell-script.png)<br>
   
    > [!NOTE]
    > PowerShell komut dosyası örneğiyle çalışırken aşağıdakileri not edin:
    > 
    > * Runbook varsayılan parametre değerleri bir dizi içerir. Tüm varsayılan değerleri değerlendirin ve gerektiğinde güncelleştirin.
    > * AWS kimlik bilgilerinizi farklı olarak adlandırılan bir kimlik bilgisi `AWScred`varlığı olarak depolatıysanız, buna göre eşleşecek şekilde 57 satırında komut dosyasını güncelleştirmeniz gerekir.  
    > * PowerShell'deki AWS CLI komutlarıyla çalışırken, özellikle bu örnek runbook'ta AWS bölgesini belirtmeniz gerekir. Aksi takdirde, cmdlets başarısız olur. Daha fazla ayrıntı için PowerShell belgesi için AWS Araçları'nda [AWS Bölgesi ni belirtin.](https://docs.aws.amazon.com/powershell/latest/userguide/pstools-installing-specifying-region.html)  
    >

7. AWS aboneliğinizden görüntü adlarının listesini almak için PowerShell ISE'yi başlatın ve AWS PowerShell Modül'ü alın. İmKB ortamınızda `Get-AutomationPSCredential` `AWScred = Get-Credential`'' ile değiştirerek AWS'ye karşı kimlik doğrulaması Bu bildirim kimlik bilgileriniz için istenir ve kullanıcı adı için erişim anahtar kimliğinizi ve parola için gizli erişim anahtarınızı sağlayabilirsiniz. 

        ```powershell
        #Sample to get the AWS VM available images
        #Please provide the path where you have downloaded the AWS PowerShell module
        Import-Module AWSPowerShell
        $AwsRegion = "us-west-2"
        $AwsCred = Get-Credential
        $AwsAccessKeyId = $AwsCred.UserName
        $AwsSecretKey = $AwsCred.GetNetworkCredential().Password
   
        # Set up the environment to access AWS
        Set-AwsCredentials -AccessKey $AwsAccessKeyId -SecretKey $AwsSecretKey -StoreAs AWSProfile
        Set-DefaultAWSRegion -Region $AwsRegion
   
        Get-EC2ImageByName -ProfileName AWSProfile
        ```
        
    Aşağıdaki çıktı döndürülür:<br><br>
   ![AWS görüntüleri alın](./media/automation-scenario-aws-deployment/powershell-ise-output.png)<br>  
8. Çalışma kitabında başvurulan bir Otomasyon değişkenindeki görüntü adlarından birini `$InstanceType`kopyalayıp yapıştırın. Bu örnekte, ücretsiz AWS katmanlı aboneliğini kullandığınızdan, runbook örneğiniz için **t2.micro'yu** kullanırsınız.  
9. Runbook'u kaydedin, ardından runbook'u yayımlamak için **Yayımla'yı** tıklatın ve istendiğinde **Evet'i** tıklatın.

### <a name="testing-the-aws-vm-runbook"></a>AWS VM çalışma kitabını test etme
Runbook'u test etmeye devam etmeden önce birkaç şeyi doğrulamanız gerekir:

* AWS'ye karşı kimlik doğrulaması için çağrılan `AWScred` bir varlık oluşturuldu veya komut dosyası kimlik bilginizin varlık adına başvurmak üzere güncelleştirildi.    
* AWS PowerShell modülü Azure Otomasyonu'ndan alındı.  
* Yeni bir runbook oluşturuldu ve parametre değerleri doğrulandı ve gerektiğinde güncelleştirildi.  
* **Kayıt defteri kaydı** ve isteğe bağlı olarak **Runbook** işlemi altında **ilerleme kayıtlarını** günlüğe kaydetme ve izleme **Açık**olarak ayarlanmıştır.<br><br> ![Runbook Günlük ve](./media/automation-scenario-aws-deployment/runbook-settings-logging-and-tracing.png)İzleme .  

1. Runbook'u başlatmak için **Başlat'ı** tıklatın ve Başlat RunBook bölmesi açıldığında **Tamam'ı** tıklatın.
2. Başlangıç Runbook bölmesinde bir VM adı sağlayın. Komut dosyasında önceden yapılandırdığınız diğer parametreler için varsayılan değerleri kabul edin. Runbook işini başlatmak için **Tamam'ı** tıklatın.<br><br> ![Yeni AwsVM runbook'u başlat](./media/automation-scenario-aws-deployment/runbook-start-job-parameters.png)
3. Oluşturduğunuz runbook işi için Bir İş bölmesi açılır. Bu bölmeyi kapat.
4. Runbook İş bölmesinden **Tüm Günlükleri** seçerek işin ilerlemesini görüntüleyebilir ve çıktı akışlarını görüntüleyebilirsiniz.<br><br> ![Akış çıktısı](./media/automation-scenario-aws-deployment/runbook-job-streams-output.png)
5. VM'nin sağlandığını doğrulamak için, şu anda oturum açmadıysanız AWS Yönetim Konsolu'nda oturum açın.<br><br> ![AWS konsolu VM dağıtıldı](./media/automation-scenario-aws-deployment/aws-instances-status.png)

## <a name="next-steps"></a>Sonraki adımlar
* Grafik çalışma kitaplarıyla başlamak için [ilk grafik çalışma kitabıma](automation-first-runbook-graphical.md)bakın.
* PowerShell İş Akışı runbook'larına başlamak için [ilk PowerShell iş akışı çalışma kitabıma](automation-first-runbook-textual.md)bakın.
* Runbook türleri ve bunların avantajları ve sınırlamaları hakkında daha fazla bilgi için [Azure Otomasyon runbook türlerine](automation-runbook-types.md)bakın.
* PowerShell komut dosyası desteği özelliği hakkında daha fazla bilgi için [Azure Otomasyonu'nda Yerel PowerShell komut dosyası desteğine](https://azure.microsoft.com/blog/announcing-powershell-script-support-azure-automation-2/)bakın.