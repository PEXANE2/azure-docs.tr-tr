---
title: Amazon Web Services sanal makinenin dağıtımını otomatikleştirme
description: Bu makalede, Azure Otomasyonu kullanarak bir Amazon Web hizmeti VM 'sinin oluşturulmasını otomatik hale getirme gösterilmektedir
services: automation
ms.subservice: process-automation
ms.date: 03/16/2018
ms.topic: conceptual
ms.openlocfilehash: b149a21ef60b22c2e549c91007265ce99babdbe4
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75420918"
---
# <a name="azure-automation-scenario---provision-an-aws-virtual-machine"></a>Azure Otomasyonu senaryosu-AWS sanal makinesi sağlama
Bu makalede, Amazon Web Service (AWS) aboneliğinizde bir sanal makine sağlamak için Azure Otomasyonu 'Nu nasıl kullanabileceğinizi öğrenirsiniz ve bu VM 'ye özel bir ad verebilirsiniz: AWS 'nin VM 'yi "etiketleme" olarak ifade eder.

## <a name="prerequisites"></a>Ön koşullar
Bu makalenin amaçları doğrultusunda, bir Azure Otomasyonu hesabına ve AWS aboneliğine sahip olmanız gerekir. Azure Otomasyonu hesabı ayarlama ve AWS aboneliği kimlik bilgilerinizle yapılandırma hakkında daha fazla bilgi için, [Amazon Web Services kimlik doğrulamasını yapılandırma](automation-config-aws-account.md)konusunu gözden geçirin. Bu hesaba aşağıdaki adımlarda başvuru yaptığınız için, bu hesap, devam etmeden önce kendi AWS abonelik kimlik bilgilerinizle oluşturulmalıdır veya güncelleştirilir.

## <a name="deploy-amazon-web-services-powershell-module"></a>Amazon Web Services PowerShell modülünü dağıtma
VM sağlama runbook 'u, işini yapmak için AWS PowerShell modülünden yararlanır. AWS abonelik kimlik bilgilerinizle yapılandırılan Otomasyon hesabınıza modülü eklemek için aşağıdaki adımları gerçekleştirin.  

1. Web tarayıcınızı açın ve [PowerShell Galerisi](https://www.powershellgallery.com/packages/AWSPowerShell/) gidin ve **Azure Otomasyonu 'na dağıt düğmesine**tıklayın.<br><br> AWS PS modülü Içeri aktarma](./media/automation-scenario-aws-deployment/powershell-gallery-download-awsmodule.png) ![
2. Azure oturum açma sayfasına götürülürsünüz ve kimlik doğrulamasından sonra, Azure portal yönlendirilecektir ve aşağıdaki sayfayla karşılaşırsınız:<br><br> ![Modül sayfasını içeri aktar](./media/automation-scenario-aws-deployment/deploy-aws-powershell-module-parameters.png)
3. Kullanılacak Otomasyon hesabını seçin ve dağıtımı başlatmak için **Tamam** ' a tıklayın.

   > [!NOTE]
   > Bir PowerShell modülünü Azure Otomasyonu 'na aktarırken, cmdlet 'leri de ayıkladıktan sonra modül cmdlet 'leri içeri ve ayıklamayı tamamen tamamlayana kadar bu etkinlikler görünmez. Bu işlem birkaç dakika sürebilir.  
   > <br>

1. Azure portal, adım 3 ' te başvurulan Otomasyon hesabınızı açın.
2. **Varlıklar** kutucuğuna tıklayın ve **varlıklar** bölmesinde **modüller** kutucuğunu seçin.
3. **Modüller** sayfasında, **awspowershell** modülünü listede görürsünüz.

## <a name="create-aws-deploy-vm-runbook"></a>AWS dağıtım VM runbook 'u oluşturma
AWS PowerShell modülü dağıtıldıktan sonra, bir PowerShell betiği kullanarak AWS 'de bir sanal makine sağlamayı otomatik hale getirmek için bir runbook 'u yazabilirsiniz. Aşağıdaki adımlarda, Azure Automation 'da yerel PowerShell betiğinin nasıl kullanılacağı gösterilmektedir.  

> [!NOTE]
> Bu betikle ilgili daha fazla seçenek ve bilgi için lütfen [PowerShell Galerisi](https://www.powershellgallery.com/packages/New-AwsVM/)ziyaret edin.
> 

1. PowerShell oturumu açarak ve aşağıdakileri yazarak PowerShell Galerisi yeni-AwsVM PowerShell betiğini indirin:<br>
   ```powershell
   Save-Script -Name New-AwsVM -Path <path>
   ```
   <br>
2. Azure portal Otomasyon hesabınızı açın ve soldaki **Işlem Otomasyon** bölümünde bulunan **runbook 'lar** ' ı seçin.  
3. Runbook **'lar** sayfasında, **runbook Ekle**' yi seçin.
4. **Runbook Ekle** bölmesinde **hızlı oluştur** ' u (yeni runbook oluştur) seçin.
5. **Runbook** özellikleri bölmesinde, runbook 'larınızın ad kutusuna bir ad yazın ve **runbook türü** aşağı açılan listesinden **PowerShell**' i seçin ve ardından **Oluştur**' a tıklayın.<br><br> ![runbook bölmesi oluştur](./media/automation-scenario-aws-deployment/runbook-quickcreate-properties.png)
6. PowerShell runbook 'Unu Düzenle sayfası göründüğünde, PowerShell betiğini kopyalayıp Runbook yazma tuvaline yapıştırın.<br><br> ![Runbook PowerShell betiği](./media/automation-scenario-aws-deployment/runbook-powershell-script.png)<br>
   
    > [!NOTE]
    > Örnek PowerShell betiği ile çalışırken aşağıdakileri aklınızda edin:
    > 
    > * Runbook, bir dizi varsayılan parametre değeri içerir. Tüm varsayılan değerleri değerlendirin ve gerektiğinde güncelleştirin.
    > * AWS kimlik bilgilerinizi **Awscred**'den farklı adlı bir kimlik bilgisi varlığı olarak depoladıysanız, satır 57 ' de betiği uygun şekilde eşleşecek şekilde güncelleştirmeniz gerekir.  
    > * PowerShell 'de AWS CLı komutlarıyla çalışırken, özellikle bu örnek runbook ile, AWS bölgesini belirtmeniz gerekir. Aksi takdirde, cmdlet 'ler başarısız olur. Daha ayrıntılı bilgi için AWS 'yi görüntüle for PowerShell for PowerShell belgesinde [AWS bölgesini belirtin](https://docs.aws.amazon.com/powershell/latest/userguide/pstools-installing-specifying-region.html) .  
    >

7. AWS aboneliğinizden görüntü adlarının listesini almak için PowerShell ıSE ' yi başlatın ve AWS PowerShell modülünü içeri aktarın. ISE ortamınızdaki **Get-AutomationPSCredential** **öğesini awscred = Get-Credential**ile değiştirerek AWS 'ye karşı kimlik doğrulaması yapın. Bu, kimlik bilgilerinizi girmenizi ister ve parola için Kullanıcı adı ve **gizli erişim anahtarı** Için **erişim anahtarı kimliğinizi** sağlayabiliriz. Aşağıdaki örneğe bakın:  

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
   AWS görüntülerini al ![](./media/automation-scenario-aws-deployment/powershell-ise-output.png)<br>  
8. Görüntü adlarından birini, runbook 'ta **$InstanceType**olarak başvurulmuş şekilde bir Otomasyon değişkeninde kopyalayıp yapıştırın. Bu örnekte, ücretsiz AWS katmanlı aboneliğini kullandığınız için, runbook örneğinizdeki **T2. Micro** ' i kullanırsınız.  
9. Runbook 'u kaydedin, ardından **Yayımla** ' ya tıklayarak runbook 'u yayımlayın ve istendiğinde **Evet** ' e tıklayın.

### <a name="testing-the-aws-vm-runbook"></a>AWS VM runbook 'unu test etme
Runbook 'u sınamaya devam etmeden önce birkaç şeyi doğrulamanız gerekir. Bu avantajlar şunlardır:  

* AWS 'ye karşı kimlik doğrulaması için bir varlık, **Awscred** adında oluşturulmuştur veya betik, kimlik bilgisi varlığınızın adına başvuracak şekilde güncelleştirilmiştir.    
* AWS PowerShell modülü Azure Otomasyonu 'nda içeri aktarıldı  
* Yeni bir runbook oluşturuldu ve parametre değerleri doğrulandı ve gerektiğinde güncelleştirildi  
* **Ayrıntılı kayıtları günlüğe kaydedin** ve isteğe bağlı olarak **ilerleme kayıtlarını** runbook ayarı **günlüğü ve izleme** **altında olarak ayarlayın.**<br><br> ![runbook günlüğe kaydetme ve Izleme](./media/automation-scenario-aws-deployment/runbook-settings-logging-and-tracing.png)  

1. Runbook 'u başlatmak istiyorsunuz, bu nedenle **Başlat** ' a tıklayın ve Runbook 'u Başlat bölmesi açıldığında **Tamam** ' a tıklayın.
2. Runbook 'U Başlat bölmesinde bir **VMName**sağlayın. Önceki betikte önceden yapılandırdığınız diğer parametrelerin varsayılan değerlerini kabul edin. Runbook işini başlatmak için **Tamam** ' ı tıklatın.<br><br> New-AwsVM runbook 'u başlatmak ![](./media/automation-scenario-aws-deployment/runbook-start-job-parameters.png)
3. Oluşturduğunuz runbook işi için bir iş bölmesi açılır. Bu bölmeyi kapatın.
4. Runbook işi sayfasından **Tüm Günlükler** kutucuğunu seçerek işin ilerlemesini görüntüleyebilir ve çıkış **akışlarını** görüntüleyebilirsiniz.<br><br> ![akışı çıkışı](./media/automation-scenario-aws-deployment/runbook-job-streams-output.png)
5. VM 'nin sağlandığını doğrulamak için, şu anda oturum açmadıysanız AWS yönetim konsolunda oturum açın.<br><br> ![AWS konsolu dağıtılan VM](./media/automation-scenario-aws-deployment/aws-instances-status.png)

## <a name="next-steps"></a>Sonraki adımlar
* Grafik runbook'ları kullanmaya başlamak için bkz. [İlk grafik runbook uygulamam](automation-first-runbook-graphical.md)
* PowerShell iş akışı runbook'larını kullanmaya başlamak için bkz. [İlk PowerShell iş akışı runbook uygulamam](automation-first-runbook-textual.md)
* Runbook türleri, avantajları ve sınırlamaları hakkında daha fazla bilgi için bkz. [Azure Automation runbook türleri](automation-runbook-types.md)
* PowerShell betik desteği özelliği hakkında daha fazla bilgi için bkz. [Azure Automation’da Yerel PowerShell betik desteği](https://azure.microsoft.com/blog/announcing-powershell-script-support-azure-automation-2/)


