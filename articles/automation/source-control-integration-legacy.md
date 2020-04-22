---
title: Azure Otomasyonunda Kaynak Denetimi entegrasyonu - Eski
description: Bu makalede, Azure Otomasyonu'nda GitHub ile kaynak denetimi tümleştirmesi açıklanmaktadır.
services: automation
ms.subservice: process-automation
ms.date: 12/04/2019
ms.topic: conceptual
ms.openlocfilehash: dcadfcb4c2f8e6bc371b0a70b917c8c1e218fba9
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81679505"
---
# <a name="source-control-integration-in-azure-automation---legacy"></a>Azure Otomasyonunda kaynak denetimi entegrasyonu - Eski

> [!NOTE]
> Kaynak denetimi için yeni bir deneyim vardır. Yeni deneyim hakkında daha fazla bilgi edinmek için [Kaynak Denetimi (Önizleme) konusuna](source-control-integration.md)bakın.

Kaynak Denetimi tümleştirmesi, Otomasyon hesabınızdaki runbook'ları GitHub kaynak denetim deposuyla ilişkilendirmenize olanak tanır. Kaynak denetimi, ekibinizle kolayca işbirliği yapmanızı, değişiklikleri izlemenizi ve runbook'larınızın önceki sürümlerine geri dönmenizi sağlar. Örneğin, kaynak denetimi, kaynak denetimindeki farklı dalları geliştirme, test veya üretim Otomasyon hesaplarınızla senkronize etmenizi sağlayarak geliştirme ortamınızda test edilmiş kodu üretim Otomasyon hesabınıza tanıtmanızı kolaylaştırır.

Kaynak Denetimi, kaynak denetimi için kodu Azure Otomasyonu'ndan itmenize veya runbook'larınızı kaynak denetiminden Azure Otomasyonuna çekmenize olanak tanır. Bu makalede, Azure Otomasyon ortamınızda kaynak denetiminin nasıl ayarlanır. Azure Otomasyonunu GitHub deponuza erişmek ve kaynak denetimi tümleştirmesini kullanarak yapılabilecek farklı işlemlerde yürüyecek şekilde yapılandırarak işe başlıyoruz. 

> [!NOTE]
> Kaynak Kontrolü, [PowerShell İş Akışı runbook'ların](automation-runbook-types.md#powershell-workflow-runbooks) yanı sıra [PowerShell runbook'larını](automation-runbook-types.md#powershell-runbooks)çekmeve itme yi destekler. [Grafik çalışma kitapları](automation-runbook-types.md#graphical-runbooks) henüz desteklenmez.

## <a name="configuring-source-control"></a>Kaynak denetimini yapılandırma

Otomasyon hesabınız için kaynak denetimini yapılandırmak için gereken iki basit adım vardır ve yalnızca bir tane zaten bir GitHub hesabınız varsa. 

### <a name="create-a-github-repository"></a>GitHub deposu oluşturma

Zaten bir GitHub hesabınız ve Azure Otomasyonu'na bağlamak istediğiniz bir deponuz varsa, mevcut hesabınızda oturum açın ve aşağıdaki adım 2'den başlayın. Aksi takdirde, [GitHub](https://github.com/)gidin, yeni bir hesap için kaydolun ve [yeni bir depo oluşturun.](https://help.github.com/articles/create-a-repo/)

### <a name="set-up-source-control"></a>Kaynak denetimini ayarlama

1. Azure portalındaki Otomasyon Hesabı sayfasından, **Hesap Ayarları**altında **Kaynak Denetimi'ni tıklatın.**

2. GitHub hesap bilgilerinizi yapılandırabileceğiniz Kaynak Denetimi sayfası açılır. Yapılandırılacak parametrelerin listesi aşağıdadır:  

   | **Parametre** | **Açıklama** |
   |:--- |:--- |
   | Kaynak Seçin |Kaynağı seçin. Şu anda yalnızca **GitHub** desteklenir. |
   | Yetkilendirme |Azure Otomasyonu'nun GitHub deponuza erişmesini sağlamak için **Yetkilendirme** düğmesini tıklatın. GitHub hesabınızda zaten farklı bir pencerede oturum açtıysanız, bu hesabın kimlik bilgileri kullanılır. Yetkilendirme başarılı olduktan sonra, sayfa **GitHub**kullanıcı adınızı Yetkilendirme Özelliği altında gösterir. |
   | Depo seçin |Kullanılabilir depolar listesinden bir GitHub deposu seçin. |
   | Şube seçin |Kullanılabilir dallar listesinden bir dal seçin. Herhangi bir dal oluşturmadıysanız yalnızca **ana** dal gösterilir. |
   | Runbook klasör yolu |Runbook klasör yolu, kodunuzu itmek veya çekmek istediğiniz GitHub deposundaki yolu belirtir. **/foldername/subfoldername**biçiminde girilmelidir. Yalnızca runbook klasörü yolundaki runbook'lar Otomasyon hesabınızla senkronize edilir. Runbook klasörü yolunun alt klasörlerinde **runbook'lar eşitlenmez.** Depo **/** altındaki tüm runbook'ları eşitlemek için kullanın. |
3. Örneğin, **RootFolder**adlı bir klasör içeren **PowerShellScripts** adlı bir deponuz varsa, **alt klasör**ü. Her klasör düzeyini eşitlemek için aşağıdaki dizeleri kullanabilirsiniz:

   1. Runbook'ları **depodan**eşitlemek için runbook **/** klasör yolu.
   2. Runbook'ları **RootFolder'dan**eşitlemek için runbook klasör yolu **/RootFolder'dur.**
   3. Runbook'ları **Alt Klasör'den**eşitlemek için runbook klasör yolu **/RootFolder/SubFolder'dır.**
4. Parametreleri yapılandırıldıktan sonra Kaynak Denetimi Ayarla sayfasında görüntülenir.  

    ![Ayarları gösteren Kaynak denetim sayfası](media/source-control-integration-legacy/automation-SourceControlConfigure.png)
5. **Tamam'ı**tıklattığınızda, kaynak denetimi tümleştirmesi artık Otomasyon hesabınız için yapılandırılır ve GitHub bilgilerinizle güncelleştirilmelidir. Şimdi tüm kaynak denetimi eşitleme iş geçmişini görüntülemek için bu bölümü tıklayabilirsiniz.  

    ![Geçerli yapılandırılmış kaynak denetimi yapılandırması için değerler](media/source-control-integration-legacy/automation-RepoValues.png)
6. Kaynak denetimini ayarladıktan sonra, Otomasyon hesabınızda iki [değişken varlık](automation-variables.md) oluşturulur. Ayrıca, GitHub hesabınıza yetkili bir uygulama eklenir.

   * **Microsoft.Azure.Automation.SourceControl.Connection** değişkeni, aşağıda gösterildiği gibi bağlantı dizesinin değerlerini içerir.  

     | **Parametre** | **Değer** |
     |:--- |:--- |
     | `Name`  |Microsoft.Azure.Automation.SourceControl.Connection |
     | `Type`  |Dize |
     | `Value` |{"Şube":\<*Şube adınız*>,"RunbookFolderPath":\<*Runbook klasör* yolu\<>,"ProviderType":*GitHub için 1* \<değeri vardır>,"Depo":*Deponuzun adı*>,"Kullanıcı adı":\<*GitHub kullanıcı adınız*>} |

   * **Microsoft.Azure.Automation.SourceControl.OAuthToken**değişkeni, OAuthToken'inizin güvenli şifreli değerini içerir.  

     |**Parametre**            |**Değer** |
     |:---|:---|
     | `Name`  | `Microsoft.Azure.Automation.SourceControl.OAuthToken` |
     | `Type`  | `Unknown(Encrypted)` |
     | `Value` | <`Encrypted OAuthToken`> |  

     ![Kaynak denetim değişkenlerini gösteren bir pencere](media/source-control-integration-legacy/automation-Variables.png)  

   * **Otomasyon Kaynak Kontrolü,** GitHub hesabınıza yetkili bir uygulama olarak eklenir. Uygulamayı görüntülemek için GitHub giriş sayfanızdan **profil** > **Ayarları** > **Uygulamaları'na**gidin. Bu uygulama, Azure Otomasyon'un GitHub deponuzu bir Otomasyon hesabıyla senkronize etmesine olanak tanır.  

     ![GitHub'daki uygulama ayarları](media/source-control-integration-legacy/automation-GitApplication.png)

## <a name="using-source-control-in-automation"></a>Otomasyonda kaynak denetimi kullanma

Runbook check-in, Azure Otomasyonu'ndaki bir runbook'ta yaptığınız değişiklikleri kaynak denetim deponuza itmenize olanak tanır. Runbook'ta iade etmek için gereken adımlar aşağıda verilmiştir:

1. Otomasyon hesabınızdan [yeni bir metin çalışması oluşturun](automation-first-runbook-textual.md)veya [varolan, metinsel bir runbook'u edin.](automation-edit-textual-runbook.md) Bu runbook bir PowerShell İş Akışı veya PowerShell komut dosyası runbook olabilir.  
2. Runbook'unuzu ayarladıktan sonra kaydedin ve yönetme sayfasında **Check-in'e** tıklayın.  

    ![İadeyi GitHub düğmesine gösteren bir pencere](media/source-control-integration-legacy/automation-CheckinButton.png)

     > [!NOTE] 
     > Azure Otomasyonu'ndan yapılan iade, kaynak denetiminizde şu anda bulunan kodun üzerine yazar. Check-in için Git eşdeğer komut satırı yönergesi **git ekle + git commit + git itme**  

3. **Check-in'i**tıklattığınızda, bir onay iletisi ile istenir, devam etmek için **Evet'i** tıklatın.  

    ![Kaynak denetimine iadeyi onaylayan bir iletişim kutusu](media/source-control-integration-legacy/automation-CheckinMessage.png)
4. Check-in kaynak denetim runbook başlar: **Sync-MicrosoftAzureAutomationAccountToGitHubV1**. Bu runbook GitHub'a bağlanır ve Azure Otomasyonu'ndan deponuza değişiklikleri zorlar. İş geçmişinde denetlenenleri görüntülemek için **Kaynak Denetim Tümleştirme** sekmesine geri dön ve Depo Eşsenkronizasyon sayfasını açmak için tıklatın. Bu sayfa, tüm kaynak denetim işlerinizi gösterir. Görüntülemek istediğiniz işi seçin ve ayrıntıları görüntülemek için tıklatın.  

    ![Eşitleme işinin sonuçlarını gösteren bir pencere](media/source-control-integration-legacy/automation-CheckinRunbook.png)

   > [!NOTE]
   > Kaynak denetimi runbook'ları, görüntüleyemediğiniz veya edemeyeceğiniz özel Otomasyon runbook'larıdır. Bunlar runbook listenizde görünmese de, iş listenizde eşitleme işleri gösterir.

5. Değiştirilen runbook'un adı, denetlenen runbook için giriş parametresi olarak gönderilir. Repository Synchronization sayfasında runbook genişleterek [iş ayrıntılarını görüntüleyebilirsiniz.](automation-runbook-execution.md#viewing-job-status-from-the-azure-portal)  

    ![Eşitleme işi için girişi gösteren bir pencere](media/source-control-integration-legacy/automation-CheckinInput.png)
6. Değişiklikleri görüntülemek için iş tamamlandığında GitHub deponuzu yenileyin.  Deponuzda bir commit ileti ile bir taahhüt olmalıdır: **Azure Otomasyonunda Güncelleştirilmiş *Runbook Adı.* **  

### <a name="sync-runbooks-from-source-control-to-azure-automation"></a>Çalışma kitaplarını kaynak denetiminden Azure Otomasyonuna eşitleme

Depo Senkronizasyon sayfasındaki eşitleme düğmesi, deponuzun runbook klasörü yolundaki tüm runbook'ları Otomasyon hesabınıza çekmenize olanak tanır. Aynı depo birden fazla Otomasyon hesabına eşitlenebilir. Bir runbook'u eşitleme adımları aşağıda verilmiştir:

1. Kaynak denetimini ayarladığınız Otomasyon hesabından Kaynak Denetimi Tümleştirme/Depo Eşitleme sayfasını açın ve **Eşitle'yi**tıklatın.  Bir onay iletisi ile istenir, devam etmek için **Evet'i** tıklatın.  

    ![Tüm runbook'ların senkronize olacağını onaylayan iletiyle eşitleme düğmesi](media/source-control-integration-legacy/automation-SyncButtonwithMessage.png)

2. Sync, GitHub'a bağlanan ve deponuzdaki değişiklikleri Azure Otomasyonu'na çeken **Sync-MicrosoftAzureAutomationAccountFromGitHubV1** runbook'u başlatır. Bu eylem için Depo Eşsenkronizasyon sayfasında yeni bir iş görmeniz gerekir. Eşitleme işiyle ilgili ayrıntıları görüntülemek için iş ayrıntıları sayfasını açmak için tıklatın.  

    ![GitHub deposunda eşitleme işinin eşitleme sonuçlarını gösteren bir pencere](media/source-control-integration-legacy/automation-SyncRunbook.png)

    > [!NOTE]
    > Kaynak denetiminden gelen bir eşitleme, şu anda kaynak denetiminde olan **TÜM** runbook'lar için Otomasyon hesabınızda bulunan runbook'ların taslak sürümünün üzerine yazar. Eşitlemek için Git eşdeğer komut satırı yönergesi **git çekme**

![Askıya alınan kaynak denetimi eşitleme işindeki tüm günlükleri gösteren pencere](media/source-control-integration-legacy/automation-AllLogs.png)

## <a name="disconnecting-source-control"></a>Kaynak denetiminin kesilmesi

GitHub hesabınızla bağlantınızı kesmek için Depo Eşsenkronizasyon sayfasını açın ve **Bağlantıyı Kes'i**tıklatın. Kaynak denetimini kestikten sonra, daha önce eşitlenen runbook'lar Otomasyon hesabınızda kalır, ancak Depo Eşsenkronizasyon sayfası etkinleştirilmez.  

  ![Kaynak denetimini kesmek için Bağlantıyı Kesme düğmesini gösteren bir pencere](media/source-control-integration-legacy/automation-Disconnect.png)

## <a name="next-steps"></a>Sonraki adımlar

Kaynak denetimi tümleştirmesi hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:  

* [Azure Otomasyonu: Azure Otomasyonunda Kaynak Kontrol Entegrasyonu](https://azure.microsoft.com/blog/azure-automation-source-control-13/)  
* [Azure Otomasyonu: Azure DevOps kullanarak Runbook Kaynak Denetimini Tümleştirme](https://azure.microsoft.com/blog/azure-automation-integrating-runbook-source-control-using-visual-studio-online/)  
