---
title: Amazon Web Services sanal makinenin dağıtımını otomatikleştirme
description: Bu makalede, Azure Otomasyonu kullanarak bir Amazon Web hizmeti VM 'sinin oluşturulmasını otomatik hale getirme gösterilmektedir
services: automation
ms.subservice: process-automation
ms.date: 03/16/2018
ms.topic: conceptual
ms.openlocfilehash: 52a887d2d934aa2f7e13f0c2fdb4332066aee0e7
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81604839"
---
# <a name="azure-automation-scenario---provision-an-aws-virtual-machine"></a>Azure Otomasyonu senaryosu-AWS sanal makinesi sağlama
Bu makalede, Amazon Web Service (AWS) aboneliğinizde bir sanal makine sağlamak için Azure Otomasyonu 'Nu nasıl kullanabileceğinizi öğrenirsiniz ve bu VM 'ye özel bir ad verebilirsiniz: AWS 'nin VM 'yi "etiketleme" olarak ifade eder.

## <a name="prerequisites"></a>Ön koşullar
Bir Azure Otomasyonu hesabınız ve bir Amazon Web Services (AWS) aboneliğine sahip olmanız gerekir. Azure Otomasyonu hesabı ayarlama ve AWS aboneliği kimlik bilgilerinizle yapılandırma hakkında daha fazla bilgi için, [Amazon Web Services kimlik doğrulamasını yapılandırma](automation-config-aws-account.md)konusunu gözden geçirin. Bu hesaba aşağıdaki bölümlerde başvuru yaptığınız için, bu hesap, devam etmeden önce AWS abonelik kimlik bilgilerinizle oluşturulmalıdır veya güncelleştirilir.

## <a name="deploy-amazon-web-services-powershell-module"></a>Amazon Web Services PowerShell modülünü dağıtma
VM sağlama runbook 'u, işini yapmak için AWS PowerShell modülünü kullanır. AWS abonelik kimlik bilgilerinizle yapılandırılmış Otomasyon hesabınıza modülünü eklemek için aşağıdaki adımları kullanın.  

1. Web tarayıcınızı açın ve [PowerShell Galerisi](https://www.powershellgallery.com/packages/AWSPowerShell/) gidin ve **Azure Otomasyonu 'na dağıt düğmesine**tıklayın.<br><br> ![AWS PS modülü Içeri aktarma](./media/automation-scenario-aws-deployment/powershell-gallery-download-awsmodule.png)
2. Azure oturum açma sayfasına götürülürsünüz ve kimlik doğrulamasından sonra, Azure portal yönlendirilecektir ve aşağıdaki sayfayla karşılaşırsınız:<br><br> ![Modül sayfasını içeri aktar](./media/automation-scenario-aws-deployment/deploy-aws-powershell-module-parameters.png)
3. Kullanılacak Otomasyon hesabını seçin ve dağıtımı başlatmak için **Tamam** ' a tıklayın.

   > [!NOTE]
   > Azure Otomasyonu bir PowerShell modülünü içeri aktardığında cmdlet 'leri ayıklar. Otomasyon modülün içeri ve cmdlet 'leri ayıklayarak tamamen bitene kadar etkinlikler görünmez. Bu işlem birkaç dakika sürebilir.  
   > <br>

1. Azure portalında, Otomasyon hesabınızı açın.
2. **Varlıklar** kutucuğuna tıklayın ve varlıklar bölmesinde **modüller**' i seçin.
3. Modüller sayfasında, **Awspowershell** modülünü listede görürsünüz.

## <a name="create-aws-deploy-vm-runbook"></a>AWS dağıtım VM runbook 'u oluşturma
AWS PowerShell modülü dağıtıldıktan sonra, bir PowerShell betiği kullanarak AWS 'de bir sanal makine sağlamayı otomatik hale getirmek için bir runbook 'u yazabilirsiniz. Aşağıdaki adımlarda, yerel PowerShell betiğini Azure Otomasyonu 'nda nasıl kullanabileceğiniz gösterilmektedir.  

> [!NOTE]
> Bu betikle ilgili daha fazla seçenek ve bilgi için lütfen [PowerShell Galerisi](https://www.powershellgallery.com/packages/New-AwsVM/)ziyaret edin.
> 

1. PowerShell oturumu açarak ve aşağıdaki komutu yazarak yeni-AwsVM PowerShell betiğini PowerShell Galerisi indirin:<br>
   ```powershell
   Save-Script -Name New-AwsVM -Path <path>
   ```
   <br>
2. Azure portal Otomasyon hesabınızı açın ve **Işlem Otomasyonu**altında **runbook 'lar** ' ı seçin.  
3. Runbook 'Lar sayfasında, **runbook Ekle**' yi seçin.
4. Runbook Ekle bölmesinde **hızlı oluştur** ' u seçerek yeni bir runbook oluşturun.
5. Runbook özellikleri bölmesinde, runbook 'larınız için bir ad yazın.
6. **Runbook türü** aşağı açılan listesinden **PowerShell**' i seçin ve ardından **Oluştur**' a tıklayın.<br><br> ![Runbook oluştur bölmesi](./media/automation-scenario-aws-deployment/runbook-quickcreate-properties.png)
6. PowerShell runbook 'Unu Düzenle sayfası göründüğünde, PowerShell betiğini kopyalayıp Runbook yazma tuvaline yapıştırın.<br><br> ![Runbook PowerShell betiği](./media/automation-scenario-aws-deployment/runbook-powershell-script.png)<br>
   
    > [!NOTE]
    > Örnek PowerShell betiği ile çalışırken aşağıdakileri aklınızda edin:
    > 
    > * Runbook, bir dizi varsayılan parametre değeri içerir. Tüm varsayılan değerleri değerlendirin ve gerektiğinde güncelleştirin.
    > * AWS kimlik bilgilerinizi öğesinden `AWScred`farklı adlı bir kimlik bilgisi varlığı olarak depoladıysanız, satır 57 ' de betiği uygun şekilde eşleşecek şekilde güncelleştirmeniz gerekir.  
    > * PowerShell 'de AWS CLı komutlarıyla çalışırken, özellikle bu örnek runbook ile, AWS bölgesini belirtmeniz gerekir. Aksi takdirde, cmdlet 'ler başarısız olur. Daha ayrıntılı bilgi için AWS 'yi görüntüle for PowerShell for PowerShell belgesinde [AWS bölgesini belirtin](https://docs.aws.amazon.com/powershell/latest/userguide/pstools-installing-specifying-region.html) .  
    >

7. AWS aboneliğinizden görüntü adlarının listesini almak için PowerShell ıSE ' yi başlatın ve AWS PowerShell modülünü içeri aktarın. İle ıSE ortamınızda değiştirerek `Get-AutomationPSCredential` AWS 'ye `AWScred = Get-Credential`karşı kimlik doğrulaması yapın. Bu bildirimde kimlik bilgileriniz istenir ve parola için Kullanıcı adı ve gizli erişim anahtarınız için erişim anahtarı KIMLIĞINIZ sağlayabilirsiniz. 

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
   ![AWS görüntülerini al](./media/automation-scenario-aws-deployment/powershell-ise-output.png)<br>  
8. Görüntü adlarından birini, runbook 'ta olduğu `$InstanceType`gibi bir Otomasyon değişkeninde kopyalayıp yapıştırın. Bu örnekte, ücretsiz AWS katmanlı aboneliğini kullanıyorsunuz, runbook örneğinizden **T2. Micro** kullanıyorsunuz.  
9. Runbook 'u kaydedin, ardından **Yayımla** ' ya tıklayarak runbook 'u yayımlayın ve istendiğinde **Evet** ' e tıklayın.

### <a name="testing-the-aws-vm-runbook"></a>AWS VM runbook 'unu test etme
Runbook 'u sınamaya devam etmeden önce, birkaç şeyi doğrulamanız gerekir:

* AWS 'ye `AWScred` karşı kimlik doğrulaması için çağrılan bir varlık oluşturulmuştur veya betik, kimlik bilgileri varlık adınızla başvuracak şekilde güncelleştirilmiştir.    
* AWS PowerShell modülü Azure Otomasyonu 'nda içeri aktarıldı.  
* Yeni bir runbook oluşturuldu ve parametre değerleri doğrulandı ve gerektiğinde güncelleştirildi.  
* **Ayrıntılı kayıtları günlüğe kaydedin** ve isteğe bağlı olarak **ilerleme kayıtlarını** Runbook işlem **günlüğü ve izleme** **altında olarak ayarlayın.**<br><br> ![Runbook günlüğü ve Izleme](./media/automation-scenario-aws-deployment/runbook-settings-logging-and-tracing.png).  

1. Runbook 'u başlatmak için **Başlat** ' a tıklayın, ardından runbook 'u Başlat bölmesi açıldığında **Tamam** ' a tıklayın.
2. Runbook 'U Başlat bölmesinde bir VM adı belirtin. Betikte önceden yapılandırdığınız diğer parametrelerin varsayılan değerlerini kabul edin. Runbook işini başlatmak için **Tamam** ' ı tıklatın.<br><br> ![New-AwsVM runbook 'u Başlat](./media/automation-scenario-aws-deployment/runbook-start-job-parameters.png)
3. Oluşturduğunuz runbook işi için bir Iş bölmesi açılır. Bu bölmeyi kapatın.
4. Runbook Iş bölmesinden **Tüm Günlükler** ' i seçerek işin ilerlemesini görüntüleyebilir ve çıkış akışlarını görüntüleyebilirsiniz.<br><br> ![Akış çıkışı](./media/automation-scenario-aws-deployment/runbook-job-streams-output.png)
5. VM 'nin sağlandığını doğrulamak için, şu anda oturum açmadıysanız AWS yönetim konsolunda oturum açın.<br><br> ![AWS konsolu dağıtılan VM](./media/automation-scenario-aws-deployment/aws-instances-status.png)

## <a name="next-steps"></a>Sonraki adımlar
* Grafik runbook 'ları kullanmaya başlamak için bkz. [ilk grafik runbook 'Um](automation-first-runbook-graphical.md).
* PowerShell Iş akışı runbook 'larını kullanmaya başlamak için bkz. [Ilk PowerShell iş akışı runbook 'Um](automation-first-runbook-textual.md).
* Runbook türleri ve bunların avantajları ve sınırlamaları hakkında daha fazla bilgi edinmek için bkz. [Azure Otomasyonu runbook türleri](automation-runbook-types.md).
* PowerShell betiği destek özelliği hakkında daha fazla bilgi için bkz. [Azure Otomasyonu 'Nda yerel PowerShell betiği desteği](https://azure.microsoft.com/blog/announcing-powershell-script-support-azure-automation-2/).