---
title: Azure Otomasyonu 'nda kaynak denetimi tümleştirmesi-eski
description: Bu makalede, Azure Otomasyonu 'nda GitHub ile kaynak denetimi tümleştirmesi açıklanmaktadır.
services: automation
ms.subservice: process-automation
ms.date: 12/04/2019
ms.topic: conceptual
ms.openlocfilehash: 651b97dabfd3cce858ea1f905a39c10bd7d81c41
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75417434"
---
# <a name="source-control-integration-in-azure-automation---legacy"></a>Azure Otomasyonu 'nda kaynak denetimi tümleştirmesi-eski

> [!NOTE]
> Kaynak denetimi için yeni bir deneyim vardır. Yeni deneyim hakkında daha fazla bilgi edinmek için bkz. [kaynak denetimi (Önizleme)](source-control-integration.md).

Kaynak denetimi tümleştirmesi Otomasyon hesabınızdaki runbook 'ları GitHub Kaynak denetimi deposu ile ilişkilendirmenize olanak tanır. Kaynak denetimi, takımınızla kolayca işbirliği yapmanızı, değişiklikleri izlemenizi ve Runbook 'larınızın önceki sürümlerine geri dönmeyi sağlar. Örneğin, kaynak denetimi, kaynak denetimindeki farklı dalları geliştirme, test veya üretim Otomasyon hesaplarınızla eşitlemenize olanak tanır. bu sayede geliştirme ortamınızda test edilmiş kodu üretim otomasyonuna yükseltmeniz kolaylaşır hesabı.

Kaynak denetimi, Azure Automation 'dan kaynak denetimine kod göndermenize veya Runbook 'larınızı kaynak denetiminden Azure Otomasyonu 'na çekbırakmanıza olanak tanır. Bu makalede, Azure Otomasyonu ortamınızda kaynak denetiminin nasıl ayarlanacağı açıklanır. Azure Otomasyonu 'Nu GitHub deponuza erişmek ve kaynak denetimi tümleştirmesi kullanılarak yapılabilecek farklı işlemlere yol gösterecek şekilde yapılandırarak başlayacağız. 

> [!NOTE]
> Kaynak denetimi PowerShell [Iş akışı runbook](automation-runbook-types.md#powershell-workflow-runbooks) 'larının yanı sıra [PowerShell runbook](automation-runbook-types.md#powershell-runbooks)'larının de çekmesini ve kullanılmasını destekler. [Grafik runbook 'ları](automation-runbook-types.md#graphical-runbooks) henüz desteklenmiyor.

Otomasyon hesabınız için kaynak denetimi yapılandırmak için gereken iki basit adım vardır ve yalnızca bir GitHub hesabınız varsa. Bunlar:

## <a name="step-1--create-a-github-repository"></a>1\. adım – GitHub deposu oluşturma

Azure Otomasyonu 'na bağlamak istediğiniz bir GitHub hesabınız ve bir deponuz varsa, mevcut hesabınızda oturum açın ve aşağıdaki adım 2 ' den başlayın. Aksi takdirde, [GitHub](https://github.com/)' a gidin, yeni bir hesap için kaydolun ve [Yeni bir depo oluşturun](https://help.github.com/articles/create-a-repo/).

## <a name="step-2--set-up-source-control-in-azure-automation"></a>2\. adım – Azure Otomasyonu 'nda kaynak denetimi ayarlama

1. Azure portal Otomasyon hesabı sayfasından, **Hesap ayarları**altında, **kaynak denetimi** ' ne tıklayın.

2. GitHub hesabı ayrıntılarınızı yapılandırabileceğiniz **kaynak denetimi** sayfası açılır. Yapılandırılacak parametrelerin listesi aşağıdadır:  

   | **Parametre** | **Açıklama** |
   |:--- |:--- |
   | Kaynak Seçin |Kaynağı seçin. Şu anda yalnızca **GitHub** desteklenir. |
   | Yetkilendirme |GitHub deponuza Azure Otomasyonu erişimi sağlamak için **Yetkilendir** düğmesine tıklayın. Zaten GitHub hesabınızda farklı bir pencerede oturum açtıysanız, bu hesabın kimlik bilgileri kullanılır. Yetkilendirme başarılı olduktan sonra, sayfa, **yetkilendirme özelliği**altında GitHub Kullanıcı adınızı gösterir. |
   | Depo seçin |Kullanılabilir depolar listesinden bir GitHub deposu seçin. |
   | Dal seçin |Kullanılabilir dallar listesinden bir dal seçin. Yalnızca dal oluşturmadıysanız, **ana** dal gösterilir. |
   | Runbook klasörü yolu |Runbook klasörü yolu, kodunuzu göndermek veya çekmek istediğiniz GitHub deposundaki yolu belirtir. **/FolderName/subklasöradı**biçiminde girilmesi gerekir. Yalnızca runbook klasörü yolundaki runbook 'lar Otomasyon hesabınızla eşitlenir. Runbook klasör yolunun alt klasörlerindeki runbook **'lar eşitlenmez.** Depodaki tüm runbook 'ları eşitlemek için **/** kullanın. |
3. Örneğin, **RootFolder**adlı ve **alt**klasörü adlı bir klasör içeren **powershellscripts** adlı bir havuzunuz varsa. Her klasör düzeyini eşitlemek için aşağıdaki dizeleri kullanabilirsiniz:

   1. Runbook 'ları **depodan**eşitlemek için, runbook klasörü yolu */*
   2. Runbook 'ları **RootFolder**'a eşitlemek için Runbook klasörü yolu */RootFolder* olur
   3. Runbook 'ları **alt**klasörden eşitlemek için, runbook klasörü yolu */RootFolder/alt klasörüdür*.
4. Parametreleri yapılandırdıktan sonra, **kaynak denetimi ayarlama** sayfasında görüntülenir.  

    ![Ayarları gösteren kaynak denetimi sayfası](media/source-control-integration-legacy/automation-SourceControlConfigure.png)
5. **Tamam**' a tıkladığınızda, kaynak denetimi tümleştirmesi artık Otomasyon hesabınız için yapılandırılmıştır ve GitHub bilgileriniz ile güncelleştirilmeleri gerekir. Artık, tüm kaynak denetimi eşitleme işi geçmişinizi görüntülemek için bu bölüme tıklayabilirsiniz.  

    ![Geçerli yapılandırılmış kaynak denetimi yapılandırması için değerler](media/source-control-integration-legacy/automation-RepoValues.png)
6. Kaynak denetimini ayarladıktan sonra, Otomasyon hesabınızda Iki [değişken varlık](automation-variables.md) oluşturulur. Ayrıca, bir yetkili uygulama GitHub hesabınıza eklenir.

   * **Microsoft. Azure. Automation. SourceControl. Connection** değişkeni, aşağıda gösterildiği gibi bağlantı dizesinin değerlerini içerir.  

     | **Parametre** | **Değer** |
     |:--- |:--- |
     | `Name`  |Microsoft. Azure. Automation. SourceControl. Connection |
     | `Type`  |Dize |
     | `Value` |{"Branch":\<*dal adınızı*>, "runbookfolderpath":\<*Runbook klasör yolu*>, "providertype":\<*GitHub > Için 1 değeri*, "depo" *:\<* , "Kullanıcı adı": *GitHub Kullanıcı adınızı*>} |

   * **Microsoft. Azure. Automation. SourceControl. OAuthToken**değişkeni, OAuthToken 'inizin güvenli şifreli değerini içerir.  

     |**Parametre**            |**Değer** |
     |:---|:---|
     | `Name`  | Microsoft. Azure. Automation. SourceControl. OAuthToken |
     | `Type`  | Bilinmiyor (şifreli) |
     | `Value` | <*şifreli OAuthToken*> |  

     ![Kaynak denetim değişkenlerini gösteren bir pencere](media/source-control-integration-legacy/automation-Variables.png)  

   * **Otomasyon kaynak denetimi** , GitHub hesabınıza yetkili bir uygulama olarak eklenir. Uygulamayı görüntülemek için: GitHub giriş sayfanızda, **profilinize** > **ayarları** > **uygulamalar**' a gidin. Bu uygulama, Azure Otomasyonu 'nun GitHub deponuzu bir Otomasyon hesabıyla eşitlemesini sağlar.  

     ![GitHub 'daki uygulama ayarları](media/source-control-integration-legacy/automation-GitApplication.png)

## <a name="using-source-control-in-automation"></a>Otomasyon 'da kaynak denetimini kullanma

### <a name="check-in-a-runbook-from-azure-automation-to-source-control"></a>Azure Otomasyonu 'ndan kaynak denetimine runbook 'u iade etme

Runbook denetimi, Azure Otomasyonu 'ndaki bir runbook 'ta yaptığınız değişiklikleri kaynak denetimi deponuza göndermenize olanak tanır. Aşağıda runbook 'u iade etme adımları verilmiştir:

1. Otomasyon hesabınızdan [Yeni bir metinsel runbook oluşturun](automation-first-runbook-textual.md)veya [var olan bir metin runbook 'unu düzenleyin](automation-edit-textual-runbook.md). Bu Runbook bir PowerShell Iş akışı ya da bir PowerShell betiği runbook 'u olabilir.  
2. Runbook 'unuzu düzenledikten sonra kaydedin ve **düzenleme** sayfasında **iade et** ' e tıklayın.  

    ![GitHub düğmesine iade etmeyi gösteren pencere](media/source-control-integration-legacy/automation-CheckinButton.png)

     > [!NOTE] 
     > Azure Otomasyonu 'nda iade etme, kaynak denetiinizdeki mevcut olan kodun üzerine yazar. Git, git **Add + git COMMIT + git Push** için git eşdeğeri komut satırı yönergesi  

3. **İade et**' e tıkladığınızda, bir onay iletisiyle istenir ve devam etmek için **Evet** ' e tıklayın.  

    ![Kaynak denetimine iade etmeyi onaylayan bir iletişim kutusu](media/source-control-integration-legacy/automation-CheckinMessage.png)
4. İade etme kaynak denetim runbook 'unu başlatır: **Sync-MicrosoftAzureAutomationAccountToGitHubV1**. Bu runbook GitHub 'a bağlanır ve değişiklikleri Azure Automation 'tan deponuza gönderir. İade edilen iş geçmişini görüntülemek için **kaynak denetimi tümleştirme** sekmesine dönün ve depo eşitleme sayfasını açmak için tıklayın. Bu sayfada tüm kaynak denetim işleriniz gösterilmektedir.  Görüntülemek istediğiniz işi seçin ve ayrıntılarını görüntülemek için tıklayın.  

    ![Eşitleme işinin sonuçlarını gösteren pencere](media/source-control-integration-legacy/automation-CheckinRunbook.png)

   > [!NOTE]
   > Kaynak denetimi runbook 'ları, görüntüleyemezsiniz veya düzenleyemezsiniz. Runbook listenizde gösterilmediğinden, işler listenizde gösterilen eşitleme işleri görürsünüz.

5. Değiştirilen runbook 'un adı, iade edilen runbook için bir giriş parametresi olarak gönderilir. Runbook 'u **Depo eşitleme** sayfasında genişleterek [iş ayrıntılarını görüntüleyebilirsiniz](automation-runbook-execution.md#viewing-job-status-from-the-azure-portal) .  

    ![Bir eşitleme işi için girişi gösteren pencere](media/source-control-integration-legacy/automation-CheckinInput.png)
6. Değişiklikleri görüntülemek için, iş tamamlandığında GitHub deponuzu yenileyin.  Bir COMMIT iletisi ile deponuzda bir kayıt olmalıdır: **Azure Otomasyonu 'Nda *Runbook adı* güncelleştirildi.**  

### <a name="sync-runbooks-from-source-control-to-azure-automation"></a>Runbook 'ları kaynak denetiminden Azure Otomasyonu 'na eşitleme

Depo eşitleme sayfasındaki Eşitle düğmesi, deponuzdaki runbook klasörü yolundaki tüm runbook 'ları Otomasyon hesabınıza çekmesini sağlar. Aynı depo birden fazla Otomasyon hesabıyla eşitlenebilir. Aşağıda runbook eşitleme adımları verilmiştir:

1. Kaynak denetimini ayarladığınız Otomasyon hesabından **kaynak denetimi tümleştirmesi/depo eşitleme** sayfasını açın ve **Eşitle**' ye tıklayın.  Bir onay iletisi alırsınız, devam etmek için **Evet** ' e tıklayın.  

    ![Tüm runbook 'ların eşitleneceğini onaylayan ileti ile Eşitle düğmesi](media/source-control-integration-legacy/automation-SyncButtonwithMessage.png)

2. Eşitleme runbook 'u başlatır: **Sync-MicrosoftAzureAutomationAccountFromGitHubV1**. Bu runbook GitHub 'a bağlanır ve değişiklikleri deponuzdan Azure Otomasyonu 'na çeker. Bu eylem için **Depo eşitleme** sayfasında yeni bir iş görmeniz gerekir. Eşitleme işi hakkındaki ayrıntıları görüntülemek için, iş ayrıntıları sayfasını açmak üzere tıklayın.  

    ![GitHub deposunda eşitleme işinin Eşitleme sonuçlarını gösteren bir pencere](media/source-control-integration-legacy/automation-SyncRunbook.png)

    > [!NOTE]
    > Kaynak denetiminden eşitleme, şu anda kaynak denetimindeki **Tüm** runbook 'Lar için Automation hesabınızda mevcut olan runbook 'ların taslak sürümünün üzerine yazar. Git 'in eşitleme için git eşdeğeri komut satırı yönergesi **git pull**

![Askıya alınmış bir kaynak denetimi eşitleme işinden tüm günlükleri gösteren bir pencere](media/source-control-integration-legacy/automation-AllLogs.png)

## <a name="disconnecting-source-control"></a>Kaynak denetiminin bağlantısı kesiliyor

GitHub hesabınızla bağlantıyı kesmek için depo eşitleme sayfasını açın ve **bağlantıyı kes**' e tıklayın. Kaynak denetimini kestikten sonra, daha önce eşitlenmiş olan runbook 'lar Otomasyon hesabınızda kalır, ancak depo eşitleme sayfası etkinleştirilmeyecektir.  

  ![Kaynak denetiminin bağlantısını kesmek için bağlantıyı kes düğmesini gösteren bir pencere](media/source-control-integration-legacy/automation-Disconnect.png)

## <a name="next-steps"></a>Sonraki adımlar

Kaynak denetimi tümleştirmesi hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:  

* [Azure Otomasyonu: Azure Otomasyonu 'nda kaynak denetimi tümleştirmesi](https://azure.microsoft.com/blog/azure-automation-source-control-13/)  
* [Azure Otomasyonu: Azure DevOps kullanarak runbook kaynak denetimini tümleştirme](https://azure.microsoft.com/blog/azure-automation-integrating-runbook-source-control-using-visual-studio-online/)  
