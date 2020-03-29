---
title: Azure DevTest Labs'da Azure DevOps'tan bir görüntü fabrikası çalıştırma
description: Bu makale, Azure DevOps'teki (eski adıyla Visual Studio Team Services) görüntü fabrikasını çalıştırmak için gereken tüm hazırlıkları kapsar.
services: devtest-lab, lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2020
ms.author: spelluru
ms.openlocfilehash: bb67f765684c77ed5f8527226bef578e450579e0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76758691"
---
# <a name="run-an-image-factory-from-azure-devops"></a>Azure DevOps’tan bir görüntü fabrikası çalıştırma
Bu makale, Azure DevOps'teki (eski adıyla Visual Studio Team Services) görüntü fabrikasını çalıştırmak için gereken tüm hazırlıkları kapsar.

> [!NOTE]
> Herhangi bir orkestrasyon motoru çalışacaktır! Azure DevOps zorunlu değildir. Görüntü fabrikası Azure PowerShell komut dosyaları kullanılarak çalıştırılır, böylece Windows Görev Zamanlayıcısı, diğer CI/CD sistemleri ve benzeri sistemler kullanılarak el ile çalıştırılabilir.

## <a name="create-a-lab-for-the-image-factory"></a>Görüntü fabrikası için bir laboratuvar oluşturma
Görüntü fabrikasını kurmanın ilk adımı Azure DevTest Labs'da bir laboratuvar oluşturmaktır. Bu laboratuvar, sanal makineleri oluşturduğumuz ve özel görüntüleri kaydettiğimiz görüntü fabrikası laboratuvarıdır. Bu laboratuvar genel görüntü fabrikası sürecinin bir parçası olarak kabul edilir. Bir laboratuvar oluşturduktan sonra, daha sonra ihtiyacınız olacağından adı kaydettiğinizden emin olun.

## <a name="scripts-and-templates"></a>Komut dosyaları ve şablonlar
Ekibiniz için görüntü fabrikasını benimsemenin bir sonraki adımı, nelerin mevcut olduğunu anlamaktır. Görüntü fabrika komut dosyaları ve şablonları [DevTest Labs GitHub Repo'da](https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/Scripts/ImageFactory)herkese açıktır. İşte parçaların bir anahat:

- Görüntü Fabrikası. Kök klasörü.
    - Yapılandırma. Görüntü fabrikasına girişler
        - Altın Görüntüler. Bu klasör, özel görüntülerin tanımlarını temsil eden JSON dosyaları içerir.
        - Labs.json. Takımların belirli özel görüntüler almak için kaydolduğu dosya.
- Komut dosyaları. Görüntü fabrikasının motoru.

Bu bölümdeki makaleler, bu komut dosyaları ve şablonlar hakkında daha fazla ayrıntı sağlar.

## <a name="create-an-azure-devops-team-project"></a>Azure DevOps takım projesi oluşturma
Azure DevOps, kaynak kodu depolamanızı ve Azure PowerShell'i tek bir yerde çalıştırmanızı sağlar. Görüntüleri güncel tutmak için yinelenen çalıştırmaları zamanlayabilirsiniz. Herhangi bir sorunu teşhis etmek için sonuçları günlüğe kaydetmek için iyi olanaklar vardır.  Ancak Azure DevOps'leri kullanmak bir gereklilik değildir, Ancak Azure'a bağlanabilen ve Azure PowerShell'i çalıştırabilen herhangi bir donanım/motor kullanabilirsiniz.

Bunun yerine kullanmak istediğiniz varolan bir DevOps hesabınız veya projeniz varsa, bu adımı atlayın.

Başlamak için Azure DevOps'te ücretsiz bir hesap oluşturun. Azure https://www.visualstudio.com/ **DevOps** (eski adıyla VSTS) altında ücretsiz **olarak Başlat'ı** ziyaret edin ve seçin. Benzersiz bir hesap adı seçmeniz ve Git'i kullanarak kodu yönetmeyi seçtiğinizden emin olmanız gerekir. Bu oluşturulduktan sonra URL'yi takım projenize kaydedin. Burada örnek bir `https://<accountname>.visualstudio.com/MyFirstProject`URL: .

## <a name="check-in-the-image-factory-to-git"></a>Git için görüntü fabrikasını kontrol edin
Görüntü fabrikası için tüm PowerShell, şablonlar ve yapılandırma [kamu DevTest Labs GitHub repo](https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/Scripts/ImageFactory)bulunmaktadır. Kodu yeni ekip projenize aktarmanın en hızlı yolu bir depo almaktır. Bu tüm DevTest Labs deposu çeker (böylece ekstra dokümanlar alırsınız, ve örnekler).

1. Önceki adımda oluşturduğunuz Azure DevOps projesini ziyaret edin (URL **https gibi\//\<görünüyor: hesap adı>.visualstudio.com/MyFirstProject).**
2. **Depo Alma'yı**seçin.
3. DevTest Labs Repo için **klon URL'sini** girin: `https://github.com/Azure/azure-devtestlab`.
4. **İçeri aktar**'ı seçin.

    ![İthal Git repo](./media/set-up-devops-lab/import-git-repo.png)

Yalnızca tam olarak ne gerekli (görüntü fabrikası dosyaları) kontrol etmeye karar verirseniz, Git repo'yu klonlamak ve yalnızca **komut dosyaları/ImageFactory** dizininde bulunan dosyaları itmek için [buradaki](https://www.visualstudio.com/en-us/docs/git/share-your-code-in-git-vs) adımları izleyin.

## <a name="create-a-build-and-connect-to-azure"></a>Yapı oluşturma ve Azure'a bağlanma
Bu noktada, Kaynak dosyaları Azure DevOps'lerde git repo'sunda depolanır. Azure PowerShell'i çalıştırmak için bir boru hattı ayarlamanız gerekir. Bu adımları yapmak için birçok seçenek vardır. Bu makalede, basitlik için yapı tanımı kullanırsınız, ancak DevOps Build, DevOps Release (tek veya birden çok ortam), Windows Görev Zamanlayıcısı gibi diğer yürütme motorları veya Azure PowerShell'i çalıştırabilecek diğer tümcüllerle çalışır.

> [!NOTE]
> Bazı PowerShell dosyaları oluşturmak için çok (10 +) özel görüntüler olduğunda çalıştırmak için uzun bir zaman alacağını akılda tutulması gereken önemli bir nokta. Ücretsiz barındırılan DevOps Build/Release aracıları 30 dakika süreye sahiptir, bu nedenle birçok resim oluşturmaya başladıktan sonra ücretsiz barındırılan aracıyı kullanamazsınız. Bu zaman aşımı mücadelesi, kullanmaya karar verirseniz kullanın, uzun süre çalışan Azure PowerShell komut dosyaları için tipik zaman aşımı sürelerini uzatabileceğinizi önceden doğrulamak iyidir. Azure DevOps durumunda, ücretli barındırılan Aracıları kullanabilir veya kendi yapı aracınızı kullanabilirsiniz.

1. Başlamak için DevOps Projenizin ana sayfasında **Oluştur'u** seçin:

    ![Kurulum Yap düğmesi](./media/set-up-devops-lab/setup-build-button.png)
2. Yapı için bir **ad** belirtin (örneğin: DevTest Labs'a Resim Oluşturma ve Teslim Etme).
3. **Boş** bir yapı tanımı seçin ve yapınızı oluşturmak için **Uygula'yı** seçin.
4. Bu aşamada, yapı aracısı için **Barındırılan'ı** seçebilirsiniz.
5. Yapı tanımını **kaydedin.**

    ![Derleme tanımı](./media/set-up-devops-lab/build-definition.png)

## <a name="configure-the-build-variables"></a>Yapı değişkenlerini yapılandırma
Komut satırı parametrelerini basitleştirmek için, görüntü fabrikasını bir yapı değişkeni kümesine iten anahtar değerleri kapsülleyin. **Değişkenler** sekmesini seçin ve birkaç varsayılan değişkenin listesini görürsünüz. Azure DevOps'a girilen değişkenlerin listesi aşağıda veda edebilirsiniz:


| Değişken Adı | Değer | Notlar |
| ------------- | ----- | ----- |
| Configurationlocation | /Scripts/ImageFactory/Configuration | Bu, **Yapılandırma** klasörüne giden depodaki tam yoldur. Yukarıdaki repo'nun tamamını ithal ettiyseniz, soldaki değer doğrudur. Aksi takdirde Yapılandırma konumuna işaret etmek için güncelleştirin. |
| DevTestLabName | MyImageFactory | Azure DevTest Labs'daki laboratuvarın adı, görüntü üretmek için fabrika olarak kullanılmıştır. Eğer yoksa, bir tane oluşturun. Laboratuvar,hizmet bitiş noktasının erişediği abonelikte olduğundan emin olun. |
| Görüntü Tutma | 1 | Her türde kaydetmek istediğiniz görüntü sayısı. Varsayılan değeri 1 olarak ayarlayın. |
| MakineŞifre | ******* | Sanal makineler için yerleşik yönetici hesap parolası. Bu geçici bir hesap, bu yüzden güvenli olduğundan emin olun. Güvenli bir dize olduğundan emin olmak için sağdaki küçük kilit simgesini seçin. |
| MachineUserName | ImageFactoryUser | Sanal makineler için yerleşik yönetici hesabı kullanıcı adı. Bu geçici bir hesap. |
| StandartTimeoutDakika | 30 | Zaman anına normal Azure işlemlerini beklememiz gerekir. |
| SubscriptionId |  0000000000-0000-0000-0000-0000000000000 | Laboratuvarın bulunduğu ve hizmet bitiş noktasının erişebileceği aboneliğin kimliği. |
| VMSize | Standard_A3 | **Oluştur** adımı için kullanılacak sanal makinenin boyutu. Oluşturulan VM'ler geçicidir. Boyutu [laboratuvar için etkin](devtest-lab-set-lab-policy.md)biri olmalıdır. Yeterli abonelik çekirdek [kotası](../azure-resource-manager/management/azure-subscription-service-limits.md)olduğunu onaylayın.

![Değişkenler oluşturma](./media/set-up-devops-lab/configure-build-variables.png)

## <a name="connect-to-azure"></a>Azure'a Bağlanma
Bir sonraki adım, hizmet ilkesini ayarlamaktır. Bu, DevOps yapı aracısının kullanıcı adına Azure'da çalışmasını sağlayan Azure Etkin Dizin'deki bir kimliktir. Kurmak için, ilk Azure PowerShell Build Step'i ekleyerek başlayın.

1. **Görev Ekle'yi**seçin.
2. Azure **PowerShell'i**arayın.
3. Bulduktan sonra, görevi yapıya eklemek için **Ekle'yi** seçin. Bunu yaptığınızda, görevin eklendiğinde sol tarafta göründüğünü görürsünüz.

![PowerShell adımını ayarlama](./media/set-up-devops-lab/set-up-powershell-step.png)

Bir hizmet prensibi kurmanın en hızlı yolu Azure DevOps'lerin bunu bizim için yapmasına izin vermektir.

1. Az önce eklediğiniz **görevi** seçin.
2. **Azure Bağlantı Türü**için Azure Kaynak **Yöneticisi'ni**seçin.
3. Hizmet ilkesini ayarlamak için **Yönet** bağlantısını seçin.

Daha fazla bilgi için [bu blog gönderisine](https://devblogs.microsoft.com/devops/automating-azure-resource-group-deployment-using-a-service-principal-in-visual-studio-online-buildrelease-management/) bakın. **Yönet** bağlantısını seçtiğinizde, Azure bağlantısını kurmak için DevOps'te (blog gönderisinde ikinci ekran görüntüsü) doğru yere inersiniz. Bunu ayarlarken **Azure Kaynak Yöneticisi Hizmet Bitiş Noktası'nı** seçtiğinizden emin olun.

## <a name="complete-the-build-task"></a>Yapı görevini tamamlama
Yapı görevini seçerseniz, doldurulması gereken sağ bölmede tüm ayrıntıları görürsünüz.

1. İlk olarak, yapı görevi adı: **Sanal Makineler oluşturun.**
2. **Azure Kaynak Yöneticisi'ni** seçerek oluşturduğunuz **hizmet ilkesini** seçin
3. Hizmet **bitiş noktasını**seçin.
4. **Script Yolu**için , seçin **... (elips)** sağda.
5. **MakeGoldenImageVMs.ps1** komut dosyasına gidin.
6. Komut Dosyası Parametreleri aşağıdaki gibi görünmelidir:`-ConfigurationLocation $(System.DefaultWorkingDirectory)$(ConfigurationLocation) -DevTestLabName $(DevTestLabName) -vmSize $(VMSize) -machineUserName $(MachineUserName) -machinePassword (ConvertTo-SecureString -string '$(MachinePassword)' -AsPlainText -Force) -StandardTimeoutMinutes $(StandardTimeoutMinutes)`

    ![Yapı tanımını tamamlama](./media/set-up-devops-lab/complete-build-definition.png)


## <a name="queue-the-build"></a>Yapıyı sıraya tonla
Yeni bir yapıyı sıraya alarak her şeyi doğru ayarladığınızdan doğru olalım. Yapı çalışırken Azure [portalına](https://portal.azure.com) geçin ve her şeyin doğru çalıştığını doğrulamak için resim fabrika laboratuarınızdaki **Tüm Sanal Makineler'i** seçin. Laboratuvarda üç sanal makinenin oluşturulduğunu görmelisiniz.

![Laboratuvardaki VM'ler](./media/set-up-devops-lab/vms-in-lab.png)

## <a name="next-steps"></a>Sonraki adımlar
Azure DevTest Labs'ı temel alan görüntü fabrikasını kurmanın ilk adımı tamamlandı. Serinin bir sonraki makalesinde, bu VM'ler genelleştirilmiş olsun ve özel görüntülere kaydedilir. Sonra, onları diğer tüm laboratuarlarınıza dağıttırın. Serinin bir sonraki makalesine bakın: [Özel görüntüleri kaydedin ve birden çok laboratuvara dağıtın.](image-factory-save-distribute-custom-images.md)
