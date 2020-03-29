---
title: Amazon Web Hizmetlerinde VM dağıtımının otomatikleştirilmesi
description: Bu makale, Bir Amazon Web Hizmeti VM'sinin oluşturulmasını otomatikleştirmek için Azure Otomasyonunun nasıl kullanılacağını göstermektedir
services: automation
ms.subservice: process-automation
ms.date: 03/16/2018
ms.topic: conceptual
ms.openlocfilehash: b149a21ef60b22c2e549c91007265ce99babdbe4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75420918"
---
# <a name="azure-automation-scenario---provision-an-aws-virtual-machine"></a>Azure Otomasyonu senaryosu - Bir AWS sanal makine sağlama
Bu makalede, Amazon Web Hizmeti (AWS) aboneliğinizde sanal bir makine sağlamak ve bu VM'ye AWS'nin VM'yi "etiketlemek" olarak adlandırdığı belirli bir ad vermek için Azure Otomasyonu'ndan nasıl yararlanabileceğinizi öğrenirsiniz.

## <a name="prerequisites"></a>Ön koşullar
Bu makalenin amaçları için bir Azure Otomasyon uyrması ve AWS aboneliğiniz olması gerekir. Azure Otomasyon hesabı açma ve AWS abonelik kimlik bilgilerinizle yapılandırma hakkında daha fazla bilgi [için, Amazon Web Hizmetleri ile Kimlik Doğrulamasını Yapılandır'ı](automation-config-aws-account.md)gözden geçirin. Bu hesap, aşağıdaki adımlarda bu hesaba başvururken, devam etmeden önce AWS abonelik kimlik bilgilerinizle oluşturulmalı veya güncelleştirilmelidir.

## <a name="deploy-amazon-web-services-powershell-module"></a>Amazon Web Hizmetleri PowerShell Modülü dağıtın
VM sağlama runbook'unuz, aws PowerShell modülünü işini yapmak için kullanabilmekiçin kullanılır. AWS abonelik kimlik bilgilerinizle yapılandırılan Otomasyon hesabınıza modülü eklemek için aşağıdaki adımları gerçekleştirin.  

1. Web tarayıcınızı açın ve [PowerShell Galerisi'ne](https://www.powershellgallery.com/packages/AWSPowerShell/) gidin ve **Azure Otomasyonuna Dağıt düğmesine**tıklayın.<br><br> ![AWS PS Modül İthalat](./media/automation-scenario-aws-deployment/powershell-gallery-download-awsmodule.png)
2. Azure giriş sayfasına yönlendirilirsiniz ve kimlik doğrulaması yaptıktan sonra Azure portalına yönlendirilir ve aşağıdaki sayfayla birlikte sunulur:<br><br> ![İthalat Modülü sayfası](./media/automation-scenario-aws-deployment/deploy-aws-powershell-module-parameters.png)
3. Kullanmak için Otomasyon Hesabı'nı seçin ve dağıtımı başlatmak için **Tamam'ı** tıklatın.

   > [!NOTE]
   > Azure Otomasyonuna bir PowerShell modülü aktarırken, cmdlets'i de ayıklıyor ve modül cmdlets'i tamamen alma ve çıkarma işlemi tamamlanana kadar bu etkinlikler görünmüyor. Bu işlem birkaç dakika sürebilir.  
   > <br>

1. Azure portalında, adım 3'te başvurulan Otomasyon hesabınızı açın.
2. **Varlıklar** döşemesine ve **Varlıklar** bölmesine tıklayın, **Modüller** döşemesini seçin.
3. **Modüller** sayfasında listede **AWSPowerShell** modüllerini görürsünüz.

## <a name="create-aws-deploy-vm-runbook"></a>AWS dağıtım VM runbook oluşturma
AWS PowerShell Modülü dağıtıldıktan sonra, artık PowerShell komut dosyası kullanarak AWS'de sanal bir makineyi otomatikleştirmek için bir runbook yazabilirsiniz. Aşağıdaki adımlar, Azure Otomasyonu'nda yerel PowerShell komut dosyasından nasıl yararlanılanın gösteriş yapılacağını göstermektedir.  

> [!NOTE]
> Bu komut dosyası yla ilgili daha fazla seçenek ve bilgi için lütfen [PowerShell Gallery'yi](https://www.powershellgallery.com/packages/New-AwsVM/)ziyaret edin.
> 

1. PowerShell'den PowerShell komut dosyası New-AwsVM'i powershell oturumunu açarak ve aşağıdakileri yazarak indirin:<br>
   ```powershell
   Save-Script -Name New-AwsVM -Path <path>
   ```
   <br>
2. Azure portalından Otomasyon hesabınızı açın ve soldaki **Süreç Otomasyonu** **bölümünderunbook'ları** seçin.  
3. **Runbook'lar** sayfasından **runbook ekle'yi**seçin.
4. **Runbook ekle** bölmesine Hızlı **Oluştur'u** (Yeni bir runbook oluştur) seçeneğini belirleyin.
5. **Runbook** özellikleri bölmesinde, runbook'unuzun Ad kutusuna bir ad yazın ve **Runbook türü** açılır listesinden **PowerShell'i**seçin ve ardından **Oluştur'u**tıklatın.<br><br> ![Runbook bölmesi oluşturma](./media/automation-scenario-aws-deployment/runbook-quickcreate-properties.png)
6. PowerShell Runbook sayfasını edit edin, PowerShell komut dosyasını kopyalayın ve kopyalayarak çalışma defterine yapıştırın tuval.<br><br> ![Runbook PowerShell Script](./media/automation-scenario-aws-deployment/runbook-powershell-script.png)<br>
   
    > [!NOTE]
    > PowerShell komut dosyası örneğiyle çalışırken aşağıdakileri not edin:
    > 
    > * Runbook varsayılan parametre değerleri bir dizi içerir. Tüm varsayılan değerleri değerlendirin ve gerektiğinde güncelleştirin.
    > * AWS kimlik bilgilerinizi **AWScred'den**farklı olarak adlandırılan bir kimlik bilgisi varlığı olarak depolatıysanız, komut dosyasını buna göre eşleştirmek için 57 satırında güncelleştirmeniz gerekir.  
    > * PowerShell'deki AWS CLI komutlarıyla çalışırken, özellikle bu örnek runbook'ta AWS bölgesini belirtmeniz gerekir. Aksi takdirde, cmdlets başarısız olur. Daha fazla ayrıntı için PowerShell belgesi için AWS Araçları'nda [AWS Bölgesi ni belirtin.](https://docs.aws.amazon.com/powershell/latest/userguide/pstools-installing-specifying-region.html)  
    >

7. AWS aboneliğinizden görüntü adlarının listesini almak için PowerShell ISE'yi başlatın ve AWS PowerShell Modül'ü alın. **AWScred = Get-Credential**ile İmKB ortamınızda **Get-AutomationPSCredential** değiştirerek AWS karşı kimlik doğrulaması . Bu, kimlik bilgileriniz için sizi ister ve parola için kullanıcı adı ve **Gizli Erişim Anahtarı** için Access Key **Kimliğinizi** sağlayabilirsiniz. Aşağıdaki örneğe bakın:  

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
8. Çalışma kitabında $InstanceType **olarak**atıfta bulunulan bir Otomasyon değişkenindeki görüntü adlarından birini kopyalayıp yapıştırın. Bu örnekte ücretsiz AWS katmanlı aboneliği kullandığınızdan, runbook örneğiniz için **t2.micro'yu** kullanırsınız.  
9. Runbook'u kaydedin, ardından runbook'u yayımlamak için **Yayımla'yı** tıklatın ve istendiğinde **Evet'i** tıklatın.

### <a name="testing-the-aws-vm-runbook"></a>AWS VM çalışma kitabını test etme
Runbook'u test etmeye devam etmeden önce birkaç şeyi doğrulamanız gerekir. Daha ayrıntılı şekilde belirtmek gerekirse:  

* AWS'ye karşı kimlik doğrulaması için bir varlık **AWScred** olarak oluşturuldu veya komut dosyası kimlik kıymetinizin adına başvurmak üzere güncelleştirildi.    
* AWS PowerShell modülü Azure Otomasyonu'nda alındı  
* Yeni bir runbook oluşturuldu ve parametre değerleri doğrulandı ve gerektiğinde güncelleştirildi  
* **Ayrıntılı kayıtları günlüğe kaydedin** ve isteğe bağlı olarak runbook ayarı altında **ilerleme kayıtlarını** günlüğe kaydetme **ve izleme** **Açık**olarak ayarlanmıştır.<br><br> ![Runbook Günlük ve İzleme](./media/automation-scenario-aws-deployment/runbook-settings-logging-and-tracing.png)  

1. Runbook'u başlatmak istiyorsanız, **Başlat'ı** tıklatın ve Başlat RunBook bölmesi açıldığında **Tamam'ı** tıklatın.
2. Başlat Runbook bölmesine bir **VMname**sağlayın. Komut dosyasında önceden yapılandırdığınız diğer parametreler için varsayılan değerleri kabul edin. Runbook işini başlatmak için **Tamam'ı** tıklatın.<br><br> ![Yeni AwsVM runbook'u başlat](./media/automation-scenario-aws-deployment/runbook-start-job-parameters.png)
3. Oluşturduğunuz runbook işi için bir iş bölmesi açılır. Bu bölmeyi kapat.
4. Runbook iş sayfasından **Tüm Günlükler** döşemesini seçerek işin ilerlemesini görüntüleyebilir ve çıktı **Akışları'nı** görüntüleyebilirsiniz.<br><br> ![Akış çıktısı](./media/automation-scenario-aws-deployment/runbook-job-streams-output.png)
5. VM'nin sağlandığını doğrulamak için, şu anda oturum açmadıysanız AWS Yönetim Konsolu'na giriş yapın.<br><br> ![AWS konsolu VM dağıtıldı](./media/automation-scenario-aws-deployment/aws-instances-status.png)

## <a name="next-steps"></a>Sonraki adımlar
* Grafik runbook'ları kullanmaya başlamak için bkz. [İlk grafik runbook uygulamam](automation-first-runbook-graphical.md)
* PowerShell iş akışı runbook'larına başlamak için [ilk PowerShell iş akışı çalışma kitabıma](automation-first-runbook-textual.md) bakın
* Runbook türleri, avantajları ve sınırlamaları hakkında daha fazla bilgi için bkz. [Azure Automation runbook türleri](automation-runbook-types.md)
* PowerShell betik desteği özelliği hakkında daha fazla bilgi için bkz. [Azure Automation’da Yerel PowerShell betik desteği](https://azure.microsoft.com/blog/announcing-powershell-script-support-azure-automation-2/)


