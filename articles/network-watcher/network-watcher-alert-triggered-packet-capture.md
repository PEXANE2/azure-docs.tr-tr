---
title: Uyarılarla proaktif ağ izleme yapmak için paket yakalamayı kullanma - Azure İşlevleri
titleSuffix: Azure Network Watcher
description: Bu makalede, Azure Ağ İzleyicisi ile uyarı tetiklenen paket yakalama nasıl oluşturulacak açıklanmaktadır
services: network-watcher
documentationcenter: na
author: damendo
ms.assetid: 75e6e7c4-b3ba-4173-8815-b00d7d824e11
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: damendo
ms.openlocfilehash: ea506e137d71fc3124a4f93f1e97750a08dd4284
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76842946"
---
# <a name="use-packet-capture-for-proactive-network-monitoring-with-alerts-and-azure-functions"></a>Uyarılar ve Azure İşlevleri ile proaktif ağ izleme için paket yakalamayı kullanma

Network Watcher paket yakalama sanal makinelere girip çıkan trafiği izlemek için yakalama oturumları oluşturur. Yakalama dosyasında yalnızca izlemek istediğiniz trafiği izlemek için tanımlanan bir filtre olabilir. Bu veriler daha sonra bir depolama blob veya yerel konuk makinede saklanır.

Bu özellik, Azure İşlevler gibi diğer otomasyon senaryolarından uzaktan başlatılabilir. Paket yakalama, tanımlanmış ağ anormalliklerine dayalı proaktif yakalamalar çalıştırma olanağı sağlar. Diğer kullanım alanları arasında ağ istatistiklerinin toplanması, ağ ihlalleri hakkında bilgi almak, istemci-sunucu iletişiminin hata ayıklanması ve daha fazlası yer almaktadır.

Azure'da dağıtılan kaynaklar 7/24 çalışır. Siz ve personeliniz tüm kaynakların durumunu 7/24 etkin bir şekilde izleyemezsiniz. Örneğin, bir sorun 02:00'de oluşursa ne olur?

Azure ekosistemi içinden Ağ İzleyicisi'ni kullanarak, uyarı lar ve işlevler kullanarak, ağınızdaki sorunları çözmek için veriler ve araçlarla proaktif bir şekilde yanıt verebilirsiniz.

![Senaryo][scenario]


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Ön koşullar

* [Azure PowerShell'in](/powershell/azure/install-Az-ps)en son sürümü.
* Ağ İzleyicisi'nin varolan bir örneği. Zaten bir tane yoksa, [Ağ İzleyicisi'nin bir örneğini oluşturun.](network-watcher-create.md)
* [Windows uzantısı](../virtual-machines/windows/extensions-nwa.md) veya Linux sanal makine uzantısı ile Network Watcher ile aynı bölgede mevcut bir sanal [makine.](../virtual-machines/linux/extensions-nwa.md)

## <a name="scenario"></a>Senaryo

Bu örnekte, VM'niz normalden daha fazla TCP kesimi gönderiyor ve uyarılmayı istiyorsunuz. TCP segmentleri burada örnek olarak kullanılır, ancak herhangi bir uyarı koşulu kullanabilirsiniz.

Uyarılettiğinizde, iletişimin neden arttığını anlamak için paket düzeyinde veri almak istersiniz. Daha sonra sanal makineyi düzenli iletişime döndürmek için adımlar atabilirsiniz.

Bu senaryo, varolan bir Ağ İzleyicisi örneğine ve geçerli bir sanal makineye sahip bir kaynak grubuna sahip olduğunuzu varsayar.

Aşağıdaki liste, gerçekleşen iş akışının genel görünümüdür:

1. VM'nizde bir uyarı tetiklenir.
1. Uyarı, Azure işlevinizi bir web hook üzerinden çağırır.
1. Azure işleviniz uyarıyı işler ve ağ izleyicisi paket yakalama oturumunu başlatır.
1. Paket yakalama VM üzerinde çalışır ve trafik toplar.
1. Paket yakalama dosyası gözden geçirilmesi ve tanılanması için bir depolama hesabına yüklenir.

Bu işlemi otomatikleştirmek için, olay meydana geldiğinde tetiklemek için VM'mizde bir uyarı oluşturur ve bağlarız. Ayrıca Ağ İzleyicisi'ni aramak için bir işlev de oluştururuz.

Bu senaryo aşağıdakileri yapar:

* Paket yakalamayı başlatan bir Azure işlevi oluşturur.
* Sanal bir makinede bir uyarı kuralı oluşturur ve uyarı kuralını Azure işlevini aramak üzere yapılandırır.

## <a name="create-an-azure-function"></a>Azure işlevi oluşturma

İlk adım, uyarıyı işlemek ve paket yakalama oluşturmak için bir Azure işlevi oluşturmaktır.

1. Azure [portalında,](https://portal.azure.com) **kaynak** > **İşlem** > **İşlevi Uygulaması**Oluştur'u seçin.

    ![İşlev uygulaması oluşturma][1-1]

2. **İşlev Uygulaması** bıçağında aşağıdaki değerleri girin ve uygulamayı oluşturmak için **Tamam'ı** seçin:

    |**Ayar** | **Değer** | **Şey** |
    |---|---|---|
    |**Uygulama adı**|PacketCaptureExample|İşlev uygulamasının adı.|
    |**Abonelik**|[Aboneliğiniz] İşlev uygulamasını oluşturmak için abonelik.||
    |**Kaynak Grubu**|PacketCaptureRG|İşlev uygulamasını içerecek kaynak grubu.|
    |**Barındırma Planı**|Tüketim Planı| İşlev uygulamanızın kullandığı plan türü. Seçenekler Tüketim veya Azure Uygulama Hizmeti planıdır. |
    |**Konum**|Orta ABD| İşlev uygulamasını oluşturacak bölge.|
    |**Depolama Hesabı**|{otomatik olarak oluşturulmuş}| Azure İşlevler'in genel amaçlı depolama için ihtiyaç duyduğu depolama hesabı.|

3. **PacketCaptureExample Fonksiyon Uygulamaları** bıçağında, **Fonksiyonlar** > **Özel fonksiyon'u** >**+** seçin.

4. **HttpTrigger-Powershell'i**seçin ve ardından kalan bilgileri girin. Son olarak, işlevi oluşturmak için **Oluştur'u**seçin.

    |**Ayar** | **Değer** | **Şey** |
    |---|---|---|
    |**Senaryo**|Deneysel|Senaryo türü|
    |**İşlevinizi adlandırma**|AlertPacketCapturePowerShell|Fonksiyonun adı|
    |**Yetkilendirme düzeyi**|İşlev|İşlev için yetkilendirme düzeyi|

![Fonksiyonlar örneği][functions1]

> [!NOTE]
> PowerShell şablonu deneyseldir ve tam destek gerektirmez.

Özelleştirmeler bu örnek için gereklidir ve aşağıdaki adımlarla açıklanır.

### <a name="add-modules"></a>Modül ekle

Network Watcher PowerShell cmdlets kullanmak için, en son PowerShell modüllerini işlev uygulamasına yükleyin.

1. En son Azure PowerShell modülleri yüklü olan yerel makinenizde aşağıdaki PowerShell komutunu çalıştırın:

    ```powershell
    (Get-Module Az.Network).Path
    ```

    Bu örnekte, Azure PowerShell modüllerinizin yerel yolu verilmiştir. Bu klasörler daha sonraki bir adımda kullanılır. Bu senaryoda kullanılan modüller şunlardır:

   * Az.Network

   * Az.Accounts

   * Az.Resources

     ![PowerShell klasörleri][functions5]

1. **İşlev uygulama ayarlarını** > seçin**App Service Editor'a gidin.**

    ![İşlev uygulaması ayarları][functions2]

1. **AlertPacketCapturePowershell** klasörünü sağ tıklatın ve ardından **azuremodules**adlı bir klasör oluşturun. 

4. İhtiyacınız olan her modül için bir alt klasör oluşturun.

    ![Klasör ve alt klasörler][functions3]

    * Az.Network

    * Az.Accounts

    * Az.Resources

1. **Az.Network** alt klasörüne sağ tıklayın ve **ardından Dosyaları Yükle'yi**seçin. 

6. Azure modüllerinize gidin. Yerel **Az.Network** klasöründe klasördeki tüm dosyaları seçin. Sonra **Tamam**’ı seçin. 

7. **Az.Accounts** ve **Az.Resources**için bu adımları yineleyin.

    ![Dosyaları karşıya yükleme][functions6]

1. Bitirdikten sonra, her klasöryerel makinenizden PowerShell modül dosyalarına sahip olmalıdır.

    ![PowerShell dosyaları][functions7]

### <a name="authentication"></a>Kimlik doğrulaması

PowerShell cmdlets kullanmak için, kimlik doğrulaması gerekir. İşlev uygulamasında kimlik doğrulamasını yapılandırırsınız. Kimlik doğrulamayı yapılandırmak için ortam değişkenlerini yapılandırmanız ve işlev uygulamasına şifreli bir anahtar dosyası yüklemeniz gerekir.

> [!NOTE]
> Bu senaryo, Azure İşlevleriyle kimlik doğrulamanın nasıl uygulanacağının yalnızca bir örneğini sağlar. Bunu yapmanın başka yolları da var.

#### <a name="encrypted-credentials"></a>Şifrelenmiş kimlik bilgileri

Aşağıdaki PowerShell komut dosyası **PassEncryptKey.key**adlı bir anahtar dosyası oluşturur. Ayrıca, sağlanan parolanın şifreli bir sürümünü de sağlar. Bu parola, kimlik doğrulama için kullanılan Azure Etkin Dizin uygulaması için tanımlanan parolayla aynıdır.

```powershell
#Variables
$keypath = "C:\temp\PassEncryptKey.key"
$AESKey = New-Object Byte[] 32
$Password = "<insert a password here>"

#Keys
[Security.Cryptography.RNGCryptoServiceProvider]::Create().GetBytes($AESKey) 
Set-Content $keypath $AESKey

#Get encrypted password
$secPw = ConvertTo-SecureString -AsPlainText $Password -Force
$AESKey = Get-content $KeyPath
$Encryptedpassword = $secPw | ConvertFrom-SecureString -Key $AESKey
$Encryptedpassword
```

İşlev uygulamasının App Service Editor'ünde **AlertPacketCapturePowerShell**altında **anahtar** adı verilen bir klasör oluşturun. Ardından, önceki PowerShell örneğinde oluşturduğunuz **PassEncryptKey.key** dosyasını yükleyin.

![Fonksiyonlar anahtarı][functions8]

### <a name="retrieve-values-for-environment-variables"></a>Çevre değişkenleri için değerleri alma

Son gereksinim, kimlik doğrulama değerlerine erişmek için gerekli olan ortam değişkenlerini ayarlamaktır. Aşağıdaki liste oluşturulan ortam değişkenlerini gösterir:

* AzureIstemciKimliği

* AzureKiracı

* AzureCredPassword


#### <a name="azureclientid"></a>AzureIstemciKimliği

İstemci kimliği, Azure Etkin Dizini'ndeki bir uygulamanın Uygulama Kimliğidir.

1. Zaten kullanmak için bir uygulama yoksa, bir uygulama oluşturmak için aşağıdaki örneği çalıştırın.

    ```powershell
    $app = New-AzADApplication -DisplayName "ExampleAutomationAccount_MF" -HomePage "https://exampleapp.com" -IdentifierUris "https://exampleapp1.com/ExampleFunctionsAccount" -Password "<same password as defined earlier>"
    New-AzADServicePrincipal -ApplicationId $app.ApplicationId
    Start-Sleep 15
    New-AzRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $app.ApplicationId
    ```

   > [!NOTE]
   > Uygulamayı oluştururken kullandığınız parola, anahtar dosyasını kaydederken daha önce oluşturduğunuz parolayla aynı olmalıdır.

1. Azure portalında **Abonelikler'i**seçin. Kullanılacak aboneliği seçin ve ardından **Access denetimi 'ni (IAM)** seçin.

    ![Fonksiyonlar IAM][functions9]

1. Kullanılacak hesabı seçin ve ardından **Özellikler'i**seçin. Başvuru Kimliğini kopyalayın.

    ![Fonksiyonlar Uygulama Kimliği][functions10]

#### <a name="azuretenant"></a>AzureKiracı

Aşağıdaki PowerShell örneğini çalıştırarak kiracı kimliğini edinin:

```powershell
(Get-AzSubscription -SubscriptionName "<subscriptionName>").TenantId
```

#### <a name="azurecredpassword"></a>AzureCredPassword

AzureCredPassword ortamı değişkeninin değeri, aşağıdaki PowerShell örneğini çalıştırarak elde ettiğiniz değerdir. Bu örnek, önceki **Şifrelenmiş kimlik bilgileri** bölümünde gösterilenle aynıdır. Gereken değer değişkenin çıktısI. `$Encryptedpassword`  Bu, PowerShell komut dosyasını kullanarak şifrelediğiniz hizmet temel parolasIdır.

```powershell
#Variables
$keypath = "C:\temp\PassEncryptKey.key"
$AESKey = New-Object Byte[] 32
$Password = "<insert a password here>"

#Keys
[Security.Cryptography.RNGCryptoServiceProvider]::Create().GetBytes($AESKey) 
Set-Content $keypath $AESKey

#Get encrypted password
$secPw = ConvertTo-SecureString -AsPlainText $Password -Force
$AESKey = Get-content $KeyPath
$Encryptedpassword = $secPw | ConvertFrom-SecureString -Key $AESKey
$Encryptedpassword
```

### <a name="store-the-environment-variables"></a>Ortam değişkenlerini depolama

1. Fonksiyon uygulamasına gidin. Ardından **İşlev uygulama ayarlarını** > seçin**Uygulama ayarlarını yapılandırın.**

    ![Uygulama ayarlarını yapılandırma][functions11]

1. Ortam değişkenlerini ve değerlerini uygulama ayarlarına ekleyin ve sonra **Kaydet'i**seçin.

    ![Uygulama ayarları][functions12]

### <a name="add-powershell-to-the-function"></a>Fonksiyona PowerShell ekleme

Artık Azure işlevi içinden Ağ İzleyicisi'ne arama yapma zamanı. Gereksinimlere bağlı olarak, bu işlevin uygulanması değişebilir. Ancak, kodun genel akışı aşağıdaki gibidir:

1. Proses giriş parametreleri.
2. Sınırları doğrulamak ve ad çakışmalarını çözmek için varolan paket yakalamalarını sorgulayın.
3. Uygun parametrelerle bir paket yakalama oluşturun.
4. Anket paketi tamamlanana kadar düzenli olarak yakalar.
5. Paket yakalama oturumunun tamamladığını kullanıcıya bildirin.

Aşağıdaki örnek, işlevde kullanılabilecek PowerShell kodudur. **subscriptionId**, **resourceGroupName**ve **storageAccountName**için değiştirilmesi gereken değerler vardır.

```powershell
            #Import Azure PowerShell modules required to make calls to Network Watcher
            Import-Module "D:\home\site\wwwroot\AlertPacketCapturePowerShell\azuremodules\Az.Accounts\Az.Accounts.psd1" -Global
            Import-Module "D:\home\site\wwwroot\AlertPacketCapturePowerShell\azuremodules\Az.Network\Az.Network.psd1" -Global
            Import-Module "D:\home\site\wwwroot\AlertPacketCapturePowerShell\azuremodules\Az.Resources\Az.Resources.psd1" -Global

            #Process alert request body
            $requestBody = Get-Content $req -Raw | ConvertFrom-Json

            #Storage account ID to save captures in
            $storageaccountid = "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}"

            #Packet capture vars
            $packetcapturename = "PSAzureFunction"
            $packetCaptureLimit = 10
            $packetCaptureDuration = 10

            #Credentials
            $tenant = $env:AzureTenant
            $pw = $env:AzureCredPassword
            $clientid = $env:AzureClientId
            $keypath = "D:\home\site\wwwroot\AlertPacketCapturePowerShell\keys\PassEncryptKey.key"

            #Authentication
            $secpassword = $pw | ConvertTo-SecureString -Key (Get-Content $keypath)
            $credential = New-Object System.Management.Automation.PSCredential ($clientid, $secpassword)
            Connect-AzAccount -ServicePrincipal -Tenant $tenant -Credential $credential #-WarningAction SilentlyContinue | out-null


            #Get the VM that fired the alert
            if($requestBody.context.resourceType -eq "Microsoft.Compute/virtualMachines")
            {
                Write-Output ("Subscription ID: {0}" -f $requestBody.context.subscriptionId)
                Write-Output ("Resource Group:  {0}" -f $requestBody.context.resourceGroupName)
                Write-Output ("Resource Name:  {0}" -f $requestBody.context.resourceName)
                Write-Output ("Resource Type:  {0}" -f $requestBody.context.resourceType)

                #Get the Network Watcher in the VM's region
                $networkWatcher = Get-AzResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq $requestBody.context.resourceRegion}

                #Get existing packetCaptures
                $packetCaptures = Get-AzNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher

                #Remove existing packet capture created by the function (if it exists)
                $packetCaptures | %{if($_.Name -eq $packetCaptureName)
                { 
                    Remove-AzNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -PacketCaptureName $packetCaptureName
                }}

                #Initiate packet capture on the VM that fired the alert
                if ((Get-AzNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher).Count -lt $packetCaptureLimit){
                    echo "Initiating Packet Capture"
                    New-AzNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -TargetVirtualMachineId $requestBody.context.resourceId -PacketCaptureName $packetCaptureName -StorageAccountId $storageaccountid -TimeLimitInSeconds $packetCaptureDuration
                    Out-File -Encoding Ascii -FilePath $res -inputObject "Packet Capture created on ${requestBody.context.resourceID}"
                }
            } 
 ``` 
#### <a name="retrieve-the-function-url"></a>İşlev URL'sini alma 
1. İşlevinizi oluşturduktan sonra, uyarınızı işlevle ilişkili URL'yi aramak için yapılandırın. Bu değeri elde etmek için işlev uygulamanızdan işlev URL'sini kopyalayın.

    ![İşlev URL'sini bulma][functions13]

2. İşlev uygulamanızın işlev URL'sini kopyalayın.

    ![İşlev URL'sini kopyalama][2]

Webhook POST isteğinin yükünde özel özellikler gerekiyorsa, [Azure metrik uyarısı üzerinde bir web hook yapılandırma'ya](../azure-monitor/platform/alerts-webhooks.md)bakın.

## <a name="configure-an-alert-on-a-vm"></a>VM'de uyarı yapılandırma

Uyarılar, belirli bir metrik kendisine atanan bir eşiği geçtiğinde kişileri bildirmek üzere yapılandırılabilir. Bu örnekte, uyarı gönderilen TCP segmentlerindedir, ancak diğer birçok ölçüm için uyarı tetiklenebilir. Bu örnekte, işlevi çağırmak için bir webhook çağırmak için bir uyarı yapılandırılır.

### <a name="create-the-alert-rule"></a>Uyarı kuralını oluşturma

Varolan bir sanal makineye gidin ve ardından bir uyarı kuralı ekleyin. Uyarıları yapılandırma yla ilgili daha ayrıntılı belgeler Azure [hizmetleri için Azure Monitor - Azure portalında uyarı oluştur'da](../monitoring-and-diagnostics/insights-alerts-portal.md)bulunabilir. **Uyarı kural** kuralı nagöre aşağıdaki değerleri girin ve ardından **Tamam'ı**seçin.

  |**Ayar** | **Değer** | **Şey** |
  |---|---|---|
  |**Adı**|TCP_Segments_Sent_Exceeded|Uyarı kuralının adı.|
  |**Açıklama**|TCP segmentleri eşik aşıldı gönderildi|Uyarı kuralının açıklaması.|
  |**Ölçüm**|Gönderilen TCP segmentleri| Uyarıyı tetiklemek için kullanılacak metrik. |
  |**Koşul**|Büyüktür| Ölçümü değerlendirirken kullanılacak koşul.|
  |**Eşik**|100| Uyarıyı tetikleyen metrin değeri. Bu değer, ortamınız için geçerli bir değer olarak ayarlanmalıdır.|
  |**Dönem**|Son beş dakika içinde| Metrikteki eşiğin bakılabilmek için hangi dönemi belirler.|
  |**Webhook**|[webhook URL işlev uygulamasından]| Önceki adımlarda oluşturulan işlev uygulamasından webhook URL'si.|

> [!NOTE]
> TCP segmentleri ölçümü varsayılan olarak etkinleştirilir. [İzleme ve tanılamayı Etkinleştir'i](../monitoring-and-diagnostics/insights-how-to-use-diagnostics.md)ziyaret ederek ek ölçümleri etkinleştirme hakkında daha fazla bilgi edinin.

## <a name="review-the-results"></a>Sonuçları gözden geçirin

Uyarı tetikleyicileri için ölçütler sonra, bir paket yakalama oluşturulur. Ağ İzleyicisi'ne gidin ve ardından **Paket yakalama'yı**seçin. Bu sayfada, paket yakalama indirmek için paket yakalama dosya bağlantısını seçebilirsiniz.

![Paket yakalamayı görüntüleme][functions14]

Yakalama dosyası yerel olarak depolanırsa, sanal makinede oturum açarak dosyayı geri alabilirsiniz.

Azure depolama hesaplarından dosya indirme yle ilgili talimatlar için [bkz.](../storage/blobs/storage-dotnet-how-to-use-blobs.md) Kullanabileceğiniz başka bir araç [Depolama Gezgini](https://storageexplorer.com/)olduğunu.

Yakalama işleminizi indirdikten sonra **,.cap** dosyasını okuyabilen herhangi bir aracı kullanarak görüntüleyebilirsiniz. Aşağıdaki iki araç için bağlantılar şunlardır:

- [Microsoft İleti Çözümleyicisi](https://technet.microsoft.com/library/jj649776.aspx)
- [Wireshark](https://www.wireshark.org/)

## <a name="next-steps"></a>Sonraki adımlar

[Wireshark ile Paket yakalama analizini](network-watcher-deep-packet-inspection.md)ziyaret ederek paket yakalamalarınızı nasıl görüntüleyebilirsiniz öğrenin.


[1]: ./media/network-watcher-alert-triggered-packet-capture/figure1.png
[1-1]: ./media/network-watcher-alert-triggered-packet-capture/figure1-1.png
[2]: ./media/network-watcher-alert-triggered-packet-capture/figure2.png
[3]: ./media/network-watcher-alert-triggered-packet-capture/figure3.png
[functions1]:./media/network-watcher-alert-triggered-packet-capture/functions1.png
[functions2]:./media/network-watcher-alert-triggered-packet-capture/functions2.png
[functions3]:./media/network-watcher-alert-triggered-packet-capture/functions3.png
[functions4]:./media/network-watcher-alert-triggered-packet-capture/functions4.png
[functions5]:./media/network-watcher-alert-triggered-packet-capture/functions5.png
[functions6]:./media/network-watcher-alert-triggered-packet-capture/functions6.png
[functions7]:./media/network-watcher-alert-triggered-packet-capture/functions7.png
[functions8]:./media/network-watcher-alert-triggered-packet-capture/functions8.png
[functions9]:./media/network-watcher-alert-triggered-packet-capture/functions9.png
[functions10]:./media/network-watcher-alert-triggered-packet-capture/functions10.png
[functions11]:./media/network-watcher-alert-triggered-packet-capture/functions11.png
[functions12]:./media/network-watcher-alert-triggered-packet-capture/functions12.png
[functions13]:./media/network-watcher-alert-triggered-packet-capture/functions13.png
[functions14]:./media/network-watcher-alert-triggered-packet-capture/functions14.png
[scenario]:./media/network-watcher-alert-triggered-packet-capture/scenario.png
