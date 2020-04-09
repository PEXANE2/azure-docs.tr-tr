---
title: Şirket içinde barındırılan tümleştirme çalışma zamanı oluşturma
description: Azure Veri Fabrikası'nda, veri fabrikalarının özel bir ağdaki veri depolarına erişmesini sağlayan kendi kendine barındırılan bir tümleştirme çalışma zamanı nasıl oluşturabilirsiniz öğrenin.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
author: nabhishek
ms.author: abnarain
manager: anandsub
ms.custom: seo-lt-2019
ms.date: 03/13/2020
ms.openlocfilehash: 3fe5a58046776d00ce68189cf724a995380869eb
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80887325"
---
# <a name="create-and-configure-a-self-hosted-integration-runtime"></a>Şirket içinde barındırılan tümleştirme çalışma zamanı oluşturma ve yapılandırma

Tümleştirme çalışma süresi (IR), Azure Veri Fabrikası'nın farklı ağ ortamlarında veri tümleştirme yetenekleri sağlamak için kullandığı bilgi işlem altyapısıdır. IR hakkında ayrıntılar [için, Tümleştirme çalışma zamanı genel görünümüne](concepts-integration-runtime.md)bakın.

Kendi kendine barındırılan tümleştirme çalışma zamanı, bir bulut veri deposu ile özel bir ağdaki veri deposu arasında kopyalama etkinlikleri çalıştırabilir. Ayrıca, şirket içi bir ağdaki bilgi işlem kaynaklarına veya Azure sanal ağına karşı dönüştürme etkinlikleri de gönderebilirsiniz. Kendi kendine barındırılan tümleştirme çalışma zamanının yüklenmesi için özel bir ağ içinde şirket içi bir makine veya sanal makine gerekir.  

Bu makalede, kendi barındırılan bir IR nasıl oluşturabileceğinizve yapılandırabileceğiniz açıklanmaktadır.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="setting-up-a-self-hosted-integration-runtime"></a>Kendi kendine barındırılan tümleştirme çalışma zamanı ayarlama

Kendi kendine barındırılan bir tümleştirme çalışma zamanı oluşturmak ve ayarlamak için aşağıdaki yordamları kullanın.

### <a name="create-a-self-hosted-ir-via-azure-powershell"></a>Azure PowerShell ile kendi kendine barındırılan bir IR oluşturun

1. Bu görev için Azure PowerShell'i kullanabilirsiniz. Örnek aşağıda verilmiştir:

    ```powershell
    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $resourceGroupName -DataFactoryName $dataFactoryName -Name $selfHostedIntegrationRuntimeName -Type SelfHosted -Description "selfhosted IR description"
    ```
  
2. Kendi kendine barındırılan tümleştirme çalışma süresini yerel bir makineye [indirin](https://www.microsoft.com/download/details.aspx?id=39717) ve kurun.

3. Kimlik doğrulama anahtarını alın ve kendi barındırılan tümleştirme çalışma zamanını anahtarla kaydedin. İşte bir PowerShell örneği:

    ```powershell

    Get-AzDataFactoryV2IntegrationRuntimeKey -ResourceGroupName $resourceGroupName -DataFactoryName $dataFactoryName -Name $selfHostedIntegrationRuntimeName  

    ```

### <a name="create-a-self-hosted-ir-via-azure-data-factory-ui"></a>Azure Veri Fabrikası Web UI ile kendi kendine barındırılan bir IR oluşturun

Azure Veri Fabrikası UI'sini kullanarak kendi kendine barındırılan bir IR oluşturmak için aşağıdaki adımları kullanın.

1. Azure Veri Fabrikası **UI'nin başlangıç** sayfasına girerek, en soldaki bölmedeki **Yazar** sekmesini seçin.

   ![Ana sayfa Yazar düğmesi](media/doc-common-process/get-started-page-author-button.png)

1. En sol bölmenin altındaki **Bağlantılar'ı** seçin ve **Bağlantılar** penceresinde **Tümleştirme çalışma sürelerini** seçin. **+Yeni'yi**seçin.

   ![Tümleştirme çalışma zamanı oluşturma](media/create-self-hosted-integration-runtime/new-integration-runtime.png)

1. **Tümleştirme çalışma zamanı kurulum** sayfasında **Azure, Kendi Kendine Barındırılan'ı**seçin ve ardından **Devam et'i**seçin. 

1. Aşağıdaki sayfada, Kendi Kendine Barındırılan BIR IR oluşturmak için **Kendi Kendine Barındırılan'ı** seçin ve ardından **Devam et'i**seçin.
   ![Selfhosted IR oluşturma](media/create-self-hosted-integration-runtime/new-selfhosted-ir.png)

1. IR'niz için bir ad girin ve **Oluştur'u**seçin.

1. **Tümleştirme çalışma zamanı kurulum** sayfasında, bilgisayarınızdaki açık kurulumu açmak için **Seçenek 1'in** altındaki bağlantıyı seçin. Veya el ile ayarlamak için **Seçenek 2** altındaki adımları izleyin. Aşağıdaki talimatlar manuel kuruluma dayanır:

   ![Tümleştirme çalışma zamanı kurulumu](media/create-self-hosted-integration-runtime/integration-runtime-setting-up.png)

    1. Kimlik doğrulama anahtarını kopyalayıp yapıştırın. **Tümleştirme çalışma saatini Karşıdan Yükle'yi seçin.**

    1. Şirket içinde barındırılan tümleştirme çalışma zamanını yerel Windows makinesine indirin. Yükleyiciyi çalıştırın.

    1. Kayıt **Tümleştirme Çalışma Süresi (Kendi barındırılan)** sayfasında, daha önce kaydettiğiniz anahtarı yapıştırın ve **Kaydol'u**seçin.
    
       ![Tümleştirme çalışma zamanını kaydetme](media/create-self-hosted-integration-runtime/register-integration-runtime.png)

    1. Yeni **Tümleştirme Runtime (Self barındırılan) Düğüm** sayfasında **Finish'i**seçin.

1. Kendi kendine barındırılan tümleştirme çalışma süresi başarıyla kaydedildikten sonra aşağıdaki pencereyi görürsünüz:

    ![Başarılı kayıt](media/create-self-hosted-integration-runtime/registered-successfully.png)

### <a name="set-up-a-self-hosted-ir-on-an-azure-vm-via-an-azure-resource-manager-template"></a>Azure Kaynak Yöneticisi şablonu aracılığıyla Azure VM'de kendi kendine barındırılan bir IR ayarlama

Kendi kendine barındırılan [IR şablonu oluştur'u](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vms-with-selfhost-integration-runtime)kullanarak azure sanal makinede kendi kendine barındırılan IR kurulumlarını otomatikleştirebilirsiniz. Şablon, bir Azure sanal ağında tamamen işlevsel, kendi kendine barındırılan bir IR'ye sahip olmak için kolay bir yol sağlar. Düğüm sayısını 2 veya daha yüksek olarak ayarladığınızda, IR yüksek kullanılabilirlik ve ölçeklenebilirlik özelliklerine sahiptir.

### <a name="set-up-an-existing-self-hosted-ir-via-local-powershell"></a>Yerel PowerShell üzerinden mevcut kendi kendine barındırılan bir IR ayarlama

Varolan kendi barındırılan bir IR'yi kurmak veya yönetmek için bir komut satırı kullanabilirsiniz. Bu kullanım özellikle kendi barındırılan IR düğümlerinin yüklenmesini ve kaydedilmesinin otomatikleştirilmesine yardımcı olabilir.

Dmgcmd.exe kendi kendine barındırılan yükleyici dahildir. Genellikle C:\Program Files\Microsoft Integration Runtime\3.0\Shared\ klasöründe bulunur. Bu uygulama çeşitli parametreleri destekler ve otomasyon için toplu komut dosyaları kullanılarak bir komut satırı üzerinden çağrılabilir.

Uygulamayı aşağıdaki gibi kullanın:

```powershell
dmgcmd [ -RegisterNewNode "<AuthenticationKey>" -EnableRemoteAccess "<port>" ["<thumbprint>"] -EnableRemoteAccessInContainer "<port>" ["<thumbprint>"] -DisableRemoteAccess -Key "<AuthenticationKey>" -GenerateBackupFile "<filePath>" "<password>" -ImportBackupFile "<filePath>" "<password>" -Restart -Start -Stop -StartUpgradeService -StopUpgradeService -TurnOnAutoUpdate -TurnOffAutoUpdate -SwitchServiceAccount "<domain\user>" ["<password>"] -Loglevel <logLevel> ]
```

Uygulamanın parametreleri ve özellikleri nin ayrıntıları aşağıda verilmiştir: 

| Özellik                                                    | Açıklama                                                  | Gerekli |
| ----------------------------------------------------------- | ------------------------------------------------------------ | -------- |
| **KayıtNewNode** "`<AuthenticationKey>`"                     | Belirtilen kimlik doğrulama anahtarıyla kendi barındırılan bir tümleştirme çalışma zamanı düğümkaydedin. | Hayır       |
| **KayıtNewNode** "`<AuthenticationKey>`" "`<NodeName>`"      | Belirtilen kimlik doğrulama anahtarı ve düğüm adı ile kendi kendine barındırılan bir tümleştirme çalışma zamanı düğümü kaydedin. | Hayır       |
| **Uzaktan Erişimi Etkinleştir** "`<port>`" ["`<thumbprint>`"]            | Yüksek kullanılabilirlik kümesi kurmak için geçerli düğümüzerinde uzaktan erişimi etkinleştirin. Veya Azure Veri Fabrikası'ndan geçmeden kimlik bilgilerini doğrudan kendi barındırılan IR'ye göre ayarlamayı etkinleştirin. Aynı ağdaki uzak bir makineden **New-AzDataFactoryV2LinkedServiceEncryptedCredential** cmdlet kullanarak ikinci yapmak. | Hayır       |
| **Uzaktan Erişim Kapsayıcısını Etkinleştirme** "`<port>`" ["`<thumbprint>`"] | Düğüm bir kapsayıcıda çalıştığında geçerli düğüme uzaktan erişimi etkinleştirin. | Hayır       |
| **Devre Dışı Uzaktan Erişim**                                         | Geçerli düğüme uzaktan erişimi devre dışı bırakın. Multinod kurulumu için uzaktan erişim gereklidir. **New-AzDataFactoryV2LinkedServiceEncryptedCredential** PowerShell cmdlet uzaktan erişim devre dışı bırakıldığında bile çalışır. Cmdlet, kendi barındırılan IR düğümüyle aynı makinede çalıştırıldığı sürece bu davranış geçerlidir. | Hayır       |
| **Anahtar** "`<AuthenticationKey>`"                                 | Önceki kimlik doğrulama anahtarının üzerine yazın veya güncelleştirin. Bu eylem ile dikkatli olun. Anahtar yeni bir tümleştirme çalışma zamanı ysa, önceki kendi barındırılan IR düğümünüz çevrimdışı olabilir. | Hayır       |
| **Yedekleme Dosyaoluşturma** "`<filePath>`" "`<password>`"            | Geçerli düğüm için bir yedek dosya oluşturun. Yedekleme dosyası düğüm anahtarı nı ve veri depolama kimlik bilgilerini içerir. | Hayır       |
| **ImportBackupFile** "`<filePath>`" "`<password>`"              | Yedek bir dosyadan düğümü geri yükleyin.                          | Hayır       |
| **Yeniden Başlatma**                                                     | Kendi barındırılan tümleştirme çalışma zamanı ana bilgisayar hizmetini yeniden başlatın.   | Hayır       |
| **Başlangıç**                                                       | Kendi kendine barındırılan tümleştirme çalışma zamanı ana bilgisayar hizmetini başlatın.     | Hayır       |
| **Durdur**                                                        | Kendi kendine barındırılan tümleştirme çalışma zamanı ana bilgisayar hizmetini durdurun.        | Hayır       |
| **Yükseltme Hizmetini Başlatma**                                         | Kendi kendine barındırılan tümleştirme çalışma zamanı yükseltme hizmetini başlatın.       | Hayır       |
| **StopUpgradeService**                                          | Kendi kendine barındırılan tümleştirme çalışma zamanı yükseltme hizmetini durdurun.        | Hayır       |
| **TurnonAutoUpdate**                                            | Kendi kendine barındırılan tümleştirme çalışma zamanı otomatik güncelleştirmesini açın.        | Hayır       |
| **TurnoffAutoUpdate**                                           | Kendi kendine barındırılan tümleştirme çalışma zamanı otomatik güncelleştirmesini kapatın.       | Hayır       |
| **SwitchServiceHesabı** "`<domain\user>`" ["`<password>`"]           | DIAHostService'i yeni bir hesap olarak çalışacak şekilde ayarlayın. Sistem hesapları ve sanal hesaplar için boş parola "" kullanın. | Hayır       |


## <a name="command-flow-and-data-flow"></a>Komut akışı ve veri akışı

Verileri şirket içi ve bulut arasında taşıdığınızda, etkinlik verileri şirket içi veri kaynağı ile bulut arasında aktarmak için kendi barındırılan bir tümleştirme çalışma zamanı kullanır.

Burada, kendi barındırılan bir IR ile kopyalama için veri akışı adımlarının üst düzey bir özeti veremiştir:

![Veri akışının üst düzey genel bakışı](media/create-self-hosted-integration-runtime/high-level-overview.png)

1. Bir veri geliştiricisi, PowerShell cmdlet kullanarak Azure veri fabrikasında kendi kendine barındırılan bir tümleştirme çalışma süresi oluşturur. Şu anda Azure portalı bu özelliği desteklemez.
1. Veri geliştiricisi, şirket içi bir veri deposu için bağlantılı bir hizmet oluşturur. Geliştirici bunu, hizmetin veri depolarına bağlanmak için kullanması gereken, barındırılan tümleştirme çalışma zamanı örneğini belirterek yapar.
1. Kendi kendine barındırılan tümleştirme çalışma zamanı düğümü, Windows Veri Koruma Uygulama Programlama Arabirimi 'ni (DPAPI) kullanarak kimlik bilgilerini şifreler ve kimlik bilgilerini yerel olarak kaydeder. Yüksek kullanılabilirlik için birden çok düğüm ayarlanmışsa, kimlik bilgileri diğer düğümler arasında daha da eşitlenir. Her düğüm DPAPI kullanarak kimlik bilgilerini şifreler ve yerel olarak depolar. Kimlik bilgisi eşitleme veri geliştiricisi için saydamdır ve kendi kendine barındırılan IR tarafından işlenir.
1. Azure Veri Fabrikası, işleri zamanlamak ve yönetmek için kendi barındırılan tümleştirme çalışma zamanıyla iletişim kurar. İletişim, paylaşılan [Azure Hizmet Veri Veri Merkezi Röle](https://docs.microsoft.com/azure/service-bus-relay/relay-what-is-it#wcf-relay) bağlantısını kullanan bir denetim kanalı üzerinden yapılır. Bir etkinlik işinin çalıştırılması gerektiğinde, Veri Fabrikası isteği kimlik bilgileriyle birlikte sıralar. Kimlik bilgilerinin kendi barındırılan tümleştirme çalışma zamanında zaten depolanmazsa bunu yapar. Kendi barındırılan tümleştirme çalışma süresi, sırayı sandıktan sonra işe başlar.
1. Kendi barındırılan tümleştirme çalışma zamanı, verileri şirket içi mağaza ve bulut depolama arasında kopyalar. Kopyanın yönü, kopya etkinliğinin veri ardışık lıkta nasıl yapılandırıldığına bağlıdır. Bu adım için, kendi kendine barındırılan tümleştirme çalışma süresi, güvenli bir HTTPS kanalı üzerinden Azure Blob depolama gibi bulut tabanlı depolama hizmetleriyle doğrudan iletişim kurar.

## <a name="considerations-for-using-a-self-hosted-ir"></a>Kendi kendine barındırılan bir IR kullanmak için dikkat edilmesi gerekenler

- Birden çok şirket içi veri kaynağı için tek bir kendi kendine barındırılan tümleştirme çalışma zamanı kullanabilirsiniz. Ayrıca, aynı Azure Etkin Dizin (Azure AD) kiracısı içindeki başka bir veri fabrikasıyla da paylaşabilirsiniz. Daha fazla bilgi için, [kendi kendine barındırılan tümleştirme çalışma süresini paylaşma'ya](#create-a-shared-self-hosted-integration-runtime-in-azure-data-factory)bakın.
- Tek bir makineye kendi kendine barındırılan tümleştirme çalışma zamanının yalnızca bir örneğini yükleyebilirsiniz. Şirket içi veri kaynaklarına erişmeniz gereken iki veri fabrikanız varsa, kendi kendine barındırılan IR'yi paylaşmak için [kendi barındırılan IR paylaşım özelliğini](#create-a-shared-self-hosted-integration-runtime-in-azure-data-factory) kullanın veya kendi barındırılan IR'yi her veri fabrikası için bir tane olmak üzere iki şirket içi bilgisayara yükleyin.  
- Kendi kendine barındırılan tümleştirme çalışma süresinin veri kaynağıyla aynı makinede olması gerekmez. Ancak, kendi kendine barındırılan tümleştirme çalışma süresinin veri kaynağına yakın olması, kendi kendine barındırılan tümleştirme çalışma süresinin veri kaynağına bağlanma süresini kısalttığı zamanı azaltır. Kendi kendine barındırılan tümleştirme çalışma zamanını, şirket içi veri kaynağını barındıran makineden farklı bir makineye yüklemenizi öneririz. Kendi kendine barındırılan tümleştirme çalışma zamanı ve veri kaynağı farklı makinelerde olduğunda, kendi kendine barındırılan tümleştirme çalışma zamanı kaynaklar için veri kaynağıyla rekabet etmez.
- Aynı şirket içi veri kaynağına bağlanan farklı makinelerde birden çok bağımsız tümleştirme çalışma süreniz olabilir. Örneğin, iki veri fabrikasına hizmet veren iki bağımsız tümleştirme çalışma süreniz varsa, aynı şirket içi veri kaynağı her iki veri fabrikasına da kaydedilebilir.
- Power BI senaryosuna hizmet etmek için bilgisayarınızda zaten yüklü bir ağ geçidi niz varsa, Veri Fabrikası için ayrı bir otomatik barındırılan tümleştirme çalışma süresini başka bir makineye yükleyin.
- Azure sanal ağında veri tümleştirmesini desteklemek için kendi barındırılan bir tümleştirme çalışma zamanı kullanın.
- Azure ExpressRoute kullanıyorsanız bile veri kaynağınızı güvenlik duvarının arkasındaki şirket içi veri kaynağı olarak değerlendirin. Hizmeti veri kaynağına bağlamak için kendi kendine barındırılan tümleştirme çalışma süresini kullanın.
- Veri deposu bir Azure Altyapısı'nda hizmet (IaaS) sanal makine olarak bulutta olsa bile, kendi kendine barındırılan tümleştirme çalışma süresini kullanın.
- Görevler, FIPS uyumlu şifrelemenin etkin olduğu bir Windows sunucusuna yüklediğiniz kendi barındırılan tümleştirme çalışma zamanında başarısız olabilir. Bu sorunu aşmak için sunucudaki FIPS uyumlu şifrelemeyi devre dışı kaldırın. FIPS uyumlu şifrelemeyi devre dışı düşürmek için, aşağıdaki kayıt defteri alt anahtarının değerini `HKLM\System\CurrentControlSet\Control\Lsa\FIPSAlgorithmPolicy\Enabled`1 (etkin) ile 0 (devre dışı) arasında değiştirin: .

## <a name="prerequisites"></a>Ön koşullar

- Windows'un desteklenen sürümleri şunlardır:
  + Windows 7 Service Pack 1
  + Windows 8.1
  + Windows 10
  + Windows Server 2008 R2 SP1
  + Windows Server 2012
  + Windows Server 2012 R2
  + Windows Server 2016
  + Windows Server 2019
   
   Etki alanı denetleyicisi üzerinde kendi kendine barındırılan tümleştirme çalışma zamanı yükleme desteklenmez.
- .NET Framework 4.6.1 veya sonraki gereklidir. Windows 7 makinesinde kendi kendine barındırılan tümleştirme çalışma süresini yüklüyorsanız,.NET Framework 4.6.1 veya sonraki lerini yükleyin. Ayrıntılar için [.NET Framework System Gereksinimlerine](/dotnet/framework/get-started/system-requirements) bakın.
- Kendi kendine barındırılan entegrasyon çalışma makinesi için önerilen minimum yapılandırma, 4 çekirdekli, 8 GB RAM ve 80 GB kullanılabilir sabit disk alanına sahip 2 GHz işlemcidir.
- Ana bilgisayar makinesi hazırda bekletme dese, kendi barındırılan tümleştirme çalışma zamanı veri isteklerine yanıt vermez. Kendi kendine barındırılan tümleştirme çalışma süresini yüklemeden önce bilgisayarda uygun bir güç planı yapılandırın. Makine hazırda bekleme değiştirmek için yürütülecek şekilde yürütülüyorsa, kendi kendine barındırılan bütünleşme zamanı yükley
- Kendi kendine barındırılan tümleştirme çalışma süresini başarıyla yüklemek ve yapılandırmak için makinede yönetici olmalısınız.
- Kopyalama etkinliği çalıştırmaları belirli bir sıklıkta gerçekleşir. Makinedeki işlemci ve RAM kullanımı, en yüksek ve boş zamanlarında aynı deseni izler. Kaynak kullanımı da büyük ölçüde taşınan veri miktarına bağlıdır. Birden çok kopyalama işi devam ederken, kaynak kullanımının yoğun zamanlarda artulaştığını görürsünüz.
- Parke, ORC veya Avro formatlarında veri ayıklama sırasında görevler başarısız olabilir. Parke hakkında daha fazla bilgi için [Azure Veri Fabrikası'nda Parke formatına](https://docs.microsoft.com/azure/data-factory/format-parquet#using-self-hosted-integration-runtime)bakın. Dosya oluşturma, kendi barındırılan tümleştirme makinesinde çalışır. Beklendiği gibi çalışmak için dosya oluşturma aşağıdaki ön koşulları gerektirir:
    - [Visual C++ 2010 Yeniden Dağıtılabilir](https://download.microsoft.com/download/3/2/2/3224B87F-CFA0-4E70-BDA3-3DE650EFEBA5/vcredist_x64.exe) Paket (x64)
    - Java Runtime (JRE) sürüm 8 gibi [OpenJDK benimsemek](https://adoptopenjdk.net/)gibi bir JRE sağlayıcıdan. Ortam değişkeninin `JAVA_HOME` ayarlandığından emin olun.

## <a name="installation-best-practices"></a>Yükleme için en iyi yöntemler

[Microsoft Download Center'dan](https://www.microsoft.com/download/details.aspx?id=39717)Yönetilen Kimlik kurulum paketini indirerek kendi kendine barındırılan tümleştirme çalışma süresini yükleyebilirsiniz. Makaleye bakın Adım adım yönergeler için [verileri şirket içi ve bulut arasında taşıyın.](tutorial-hybrid-copy-powershell.md)

- Makinenin hazırda beklemede kalmaması için, kendi barındırılan tümleştirme çalışma zamanı için ana makinede bir güç planı yapılandırın. Ana bilgisayar makinesi hazırda bekletiyorsa, kendi barındırılan tümleştirme çalışma süresi çevrimdışı olur.
- Kendi kendine barındırılan tümleştirme çalışma zamanıyla ilişkili kimlik bilgilerini düzenli olarak yedekler.
- Kendi kendine barındırılan IR kurulum işlemlerini otomatikleştirmek için lütfen [PowerShell üzerinden varolan kendi kendine barındırılan bir IR'yi ayarlama'ya](#setting-up-a-self-hosted-integration-runtime)bakın.  

## <a name="install-and-register-a-self-hosted-ir-from-microsoft-download-center"></a>Microsoft Download Center'dan kendi barındırılan bir IR yükleme ve kaydetme

1. [Microsoft tümleştirme çalışma zamanı indirme sayfasına](https://www.microsoft.com/download/details.aspx?id=39717)gidin.
1. **İndir'i**seçin, 64 bit sürümünü seçin ve **İleri'yi**seçin. 32 bit sürümü desteklenmiyor.
1. Yönetilen Kimlik dosyasını doğrudan çalıştırın veya sabit diskinize kaydedin ve çalıştırın.
1. **Karşılama** penceresinde bir dil seçin ve **İleri'yi**seçin.
1. Microsoft Yazılım Lisans Koşullarını kabul edin ve **İleri'yi**seçin.
1. Kendi kendine barındırılan tümleştirme çalışma süresini yüklemek için **klasörü** seçin ve **İleri'yi**seçin.
1. **Yüklemeye Hazır** sayfasında **Yükle'yi**seçin.
1. Yüklemeyi tamamlamak için **Bitir'i** seçin.
1. PowerShell'i kullanarak kimlik doğrulama anahtarını alın. Kimlik doğrulama anahtarını almak için powershell örneği aşağıda verilmiştir:

    ```powershell
    Get-AzDataFactoryV2IntegrationRuntimeKey -ResourceGroupName $resourceGroupName -DataFactoryName $dataFactoryName -Name $selfHostedIntegrationRuntime
    ```

1. Makinenizde çalışan Microsoft **Tümleştirme Runtime (Self-hosted)** penceresinde aşağıdaki adımları izleyin:

    1. Metin alanına kimlik doğrulama anahtarıyapıştırın.

    1. İsteğe bağlı olarak, anahtar metnini görmek için **kimlik doğrulama anahtarını göster'i** seçin.

    1. **Kaydol**’u seçin.

## <a name="high-availability-and-scalability"></a>Yüksek kullanılabilirlik ve ölçeklenebilirlik

Azure'da kendi kendine barındırılan bir tümleştirme çalışma zamanını birden çok şirket içi makine veya sanal makineyle ilişkilendirebilirsiniz. Bu makinelere düğüm denir. Kendi kendine barındırılan tümleştirme çalışma zamanıyla ilişkili en fazla dört düğüm ekleyebilirsiniz. Mantıksal bir ağ geçidi için ağ geçidi yüklü olan şirket içi makinelerde birden çok düğüm olmasının yararları şunlardır:

* Büyük veri çözümünüzde veya Veri Fabrikası ile bulut veri tümleştirmesinde artık tek hata noktası olmaması için, kendi kendine barındırılan tümleştirme çalışma zamanının daha yüksek kullanılabilirliği. Bu kullanılabilirlik, en fazla dört düğüm kullandığınızda sürekliliği sağlamaya yardımcı olur.
* Şirket içi ve bulut veri depoları arasında veri hareketi sırasında geliştirilmiş performans ve iş ortası. [Performans karşılaştırmaları](copy-activity-performance.md)hakkında daha fazla bilgi alın.

[İndirme Merkezi'nden](https://www.microsoft.com/download/details.aspx?id=39717)kendi kendine barındırılan tümleştirme çalışma zamanı yazılımını yükleyerek birden çok düğümü ilişkilendirebilirsiniz. Daha sonra, [öğreticide](tutorial-hybrid-copy-powershell.md)açıklandığı gibi, **New-AzDataFactoryV2IntegrationRuntimeKey** cmdlet elde edilen kimlik doğrulama tuşlarından birini kullanarak kaydolun.

> [!NOTE]
> Her düğümü ilişkilendirmek için kendi kendine barındırılan yeni bir tümleştirme çalışma zamanı oluşturmanız gerekmez. Kendi kendine barındırılan tümleştirme çalışma süresini başka bir makineye yükleyebilir ve aynı kimlik doğrulama anahtarını kullanarak kaydedebilirsiniz.

> [!NOTE]
> Yüksek kullanılabilirlik ve ölçeklenebilirlik için başka bir düğüm eklemeden önce, **intranet seçeneğine uzaktan erişimin** ilk düğümde etkin olduğundan emin olun. Bunu yapmak için,**Intranet'e Microsoft** **Tümleştirme Çalışma Zamanı Yapılandırma Yöneticisi** > **Ayarları** > Uzaktan erişim'i seçin.

### <a name="scale-considerations"></a>Ölçek hususları

#### <a name="scale-out"></a>Ölçeği genişletme

İşlemci kullanımı yüksekse ve kullanılabilir bellek kendi kendine barındırılan IR'de düşükse, yükü makineler arasında ölçeklendirmeye yardımcı olacak yeni bir düğüm ekleyin. Zaman ları dolduğu için etkinlikler başarısız olursa veya kendi barındırılan IR düğümü çevrimdışıysa, ağ geçidine bir düğüm eklerseniz yardımcı olur.

#### <a name="scale-up"></a>Ölçeği artırma

İşlemci ve kullanılabilir RAM iyi kullanılmadığında, ancak eşzamanlı işlerin yürütülmesi düğüm sınırlarına ulaştığında, düğümün çalıştırabileceği eşzamanlı işlerin sayısını artırarak ölçeklendirin. Kendi kendine barındırılan IR aşırı yüklü olduğundan, etkinlikler zaman dolduğunda da ölçeklendirmek isteyebilirsiniz. Aşağıdaki resimde gösterildiği gibi, bir düğüm için maksimum kapasiteyi artırabilirsiniz:  

![Düğümüzerinde çalıştırılabilen eşzamanlı işlerin sayısını artırma](media/create-self-hosted-integration-runtime/scale-up-self-hosted-IR.png)

### <a name="tlsssl-certificate-requirements"></a>TLS/SSL sertifika gereksinimleri

Entegrasyon çalışma zamanı düğümleri arasındaki iletişimi sağlamak için kullandığınız TLS/SSL sertifikası için gereksinimler şunlardır:

- Sertifika, herkese açık bir X509 v3 sertifikası olmalıdır. Bir ortak sertifika yetkilisi (CA) tarafından verilen sertifikaları kullanmanızı öneririz.
- Her tümleştirme çalışma zamanı düğümü bu sertifikaya güvenmelidir.
- Yalnızca son SAN öğesi kullanıldığından, Subject Alternative Name (SAN) sertifikalarını önermiyoruz. Diğer tüm SAN öğeleri yoksayılır. Örneğin, SN'leri **node1.domain.contoso.com** ve **node2.domain.contoso.com**olan bir SAN sertifikanız varsa, bu sertifikayı yalnızca tam nitelikli alan adı (FQDN) **node2.domain.contoso.com**olan bir makinede kullanabilirsiniz.
- Sertifika, TLS/SSL sertifikaları için Windows Server 2012 R2 tarafından desteklenen herhangi bir anahtar boyutunu kullanabilir.
- CNG anahtarlarını kullanan sertifikalar desteklenmez.  

> [!NOTE]
> Bu sertifika kullanılır:
>
> - Kendi barındırılan BIR IR düğümündeki bağlantı noktalarını şifrelemek için.
> - Düğümler arasında bağlantılı hizmetlerin kimlik bilgilerini eşitleme içeren durum eşitleme için düğümden düğüme iletişim için.
> - PowerShell cmdlet yerel bir ağ içinden bağlantılı hizmet kimlik bilgileri ayarları için kullanıldığında.
>
> Bu sertifikayı, özel ağ ortamınız güvenli değilse veya özel ağınızdaki düğümler arasındaki iletişimi güvenli hale getirmek istiyorsanız kullanmanızı öneririz.
>
> Kendi kendine barındırılan bir IR'den diğer veri depolarına geçişteki veri hareketi, bu sertifikanın ayarlanıp ayarlanmadığına bakılmaksızın her zaman şifreli bir kanalda gerçekleşir.

## <a name="create-a-shared-self-hosted-integration-runtime-in-azure-data-factory"></a>Azure Veri Fabrikası'nda paylaşılan kendi barındırılan tümleştirme çalışma zamanı oluşturma

Bir veri fabrikasında zaten ayarladığınız varolan kendi kendine barındırılan tümleştirme çalışma zamanı altyapısını yeniden kullanabilirsiniz. Bu yeniden kullanım, varolan paylaşılan kendi kendine barındırılan bir IR'ye başvurarak farklı bir veri fabrikasında bağlantılı, barındırılan bir tümleştirme çalışma süresi oluşturmanıza olanak tanır.

Bu özelliğin tanıtımını ve tanıtımını görmek için aşağıdaki 12 dakikalık videoyu izleyin:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Hybrid-data-movement-across-multiple-Azure-Data-Factories/player]

### <a name="terminology"></a>Terminoloji

- **Paylaşılan IR**: Fiziksel bir altyapı üzerinde çalışan, kendi kendine barındırılan özgün bir IR.  
- **Bağlantılı IR**: Başka bir paylaşılan IR'ye başvuran bir IR. Bağlantılı IR mantıksal bir IR ve başka bir paylaşılan kendi kendine barındırılan IR altyapısını kullanır.

### <a name="methods-to-share-a-self-hosted-integration-runtime"></a>Kendi kendine barındırılan tümleştirme çalışma süresini paylaşma yöntemleri

Birden çok veri fabrikasıyla barındırılan bir tümleştirme çalışma süresini paylaşmak için ayrıntılar için [paylaşılan kendi kendine barındırılan tümleştirme çalışma süresi oluşturma'ya](create-shared-self-hosted-integration-runtime-powershell.md) bakın.

### <a name="monitoring"></a>İzleme

#### <a name="shared-ir"></a>Paylaşılan IR

![Paylaşılan tümleştirme çalışma süresini bulmak için seçimler](media/create-self-hosted-integration-runtime/Contoso-shared-IR.png)

![Paylaşılan tümleştirme çalışma süresini izleme](media/create-self-hosted-integration-runtime/contoso-shared-ir-monitoring.png)

#### <a name="linked-ir"></a>Bağlantılı IR

![Bağlantılı tümleştirme çalışma zamanını bulmak için seçimler](media/create-self-hosted-integration-runtime/Contoso-linked-ir.png)

![Bağlantılı tümleştirme çalışma süresini izleme](media/create-self-hosted-integration-runtime/Contoso-linked-ir-monitoring.png)

### <a name="known-limitations-of-self-hosted-ir-sharing"></a>Kendi kendine barındırılan IR paylaşımının bilinen sınırlamaları

* Bağlı bir IR oluşturulduğu veri fabrikasının [Yönetilen Kimliği](https://docs.microsoft.com/azure/active-directory/managed-service-identity/overview)olmalıdır. Varsayılan olarak, Azure portalında veya PowerShell cmdlets'te oluşturulan veri fabrikaları, örtülü olarak oluşturulmuş yönetilen bir kimliğe sahiptir. Ancak bir Azure Kaynak Yöneticisi şablonu veya SDK aracılığıyla bir veri fabrikası oluşturulduğunda, **Kimlik** özelliğini açıkça ayarlamanız gerekir. Bu ayar, Kaynak Yöneticisi'nin Yönetilen Kimlik içeren bir veri fabrikası oluşturmalarını sağlar.

* Bu özelliği destekleyen Veri Fabrikası .NET SDK sürüm 1.1.0 veya daha sonra olmalıdır.

* İzin vermek için, paylaşılan IR'nin bulunduğu veri fabrikasında Sahip rolüne veya devralınan Kurucu rolüne ihtiyacınız vardır.

* Paylaşım özelliği yalnızca aynı Azure AD kiracıiçindeki veri fabrikaları için çalışır.

* Azure AD [konuk kullanıcıları](https://docs.microsoft.com/azure/active-directory/governance/manage-guest-access-with-access-reviews)için, arama anahtar sözcüğü kullanarak tüm veri fabrikalarını listeleyen Kullanıcı Arabirimi'ndeki arama işlevi [çalışmaz.](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes#SearchLimits) Ancak konuk kullanıcı veri fabrikasının sahibi olduğu sürece, arama işlevi olmadan IR'yi paylaşabilirsiniz. IR'yi paylaşması gereken veri fabrikasının Yönetilen Kimliği için, Bu Yönetilen Kimliği **Atama İzni** kutusuna girin ve Veri Fabrikası UI'ine **Ekle'yi** seçin.

  > [!NOTE]
  > Bu özellik yalnızca Veri Fabrikası V2'de kullanılabilir.

## <a name="notification-area-icons-and-notifications"></a>Bildirim alanı simgeleri ve bildirimleri

İmlecinizi bildirim alanındaki simge nin veya iletinin üzerine taşırsanız, kendi barındırılan tümleştirme çalışma zamanının durumu yla ilgili ayrıntıları görebilirsiniz.

![Bildirim alanındaki bildirimler](media/create-self-hosted-integration-runtime/system-tray-notifications.png)

## <a name="ports-and-firewalls"></a>Bağlantı noktaları ve güvenlik duvarları

Göz önünde bulundurulması gereken iki güvenlik duvarı vardır:

- Kuruluşun merkezi yönlendiriciüzerinde çalışan *kurumsal güvenlik duvarı*
- Kendi kendine barındırılan tümleştirme çalışma zamanının yüklendiği yerel makinede daemon olarak yapılandırılan *Windows güvenlik duvarı*

![Güvenlik duvarları](media/create-self-hosted-integration-runtime/firewall.png)

Kurumsal güvenlik duvarı düzeyinde, aşağıdaki etki alanlarını ve giden bağlantı noktalarını yapılandırmanız gerekir:

[!INCLUDE [domain-and-outbound-port-requirements](../../includes/domain-and-outbound-port-requirements.md)]

Windows güvenlik duvarı düzeyinde veya makine düzeyinde, bu giden bağlantı noktaları normalde etkinleştirilir. Değilse, etki alanlarını ve bağlantı noktalarını kendi barındırılan tümleştirme çalışma zamanı makinesinde yapılandırabilirsiniz.

> [!NOTE]
> Kaynağınıza ve lavabolarınıza bağlı olarak, şirket güvenlik duvarınızda veya Windows güvenlik duvarınızda ek etki alanları ve giden bağlantı noktalarına izin verebilirsiniz.
>
> Azure SQL Veritabanı ve Azure Veri Gölü gibi bazı bulut veritabanları için, güvenlik duvarı yapılandırmalarında kendi barındırılan tümleştirme çalışma zamanı makinelerinin IP adreslerine izin verebilirsiniz.

### <a name="copy-data-from-a-source-to-a-sink"></a>Verileri kaynaktan lavaboya kopyalama

Kurumsal güvenlik duvarında, kendi barındırılan tümleştirme çalışma zamanı makinesinin Windows güvenlik duvarında ve veri deposunda güvenlik duvarı kurallarını düzgün şekilde etkinleştirdiğinizden emin olun. Bu kuralları etkinleştirmek, kendi kendine barındırılan tümleştirme çalışma zamanının hem kaynağa hem de lavaboya başarılı bir şekilde bağlanmasını sağlar. Kopyalama işleminde yer alan her veri deposu için kuralları etkinleştirin.

Örneğin, şirket içi veri deposundan SQL Veritabanı lavabosına veya Azure SQL Veri Ambarı lavabosına kopyalamak için aşağıdaki adımları izleyin:

1. Hem Windows güvenlik duvarı hem de şirket güvenlik duvarı için 1433 bağlantı noktasında giden TCP iletişimine izin verin.
1. SQL Veritabanı'nın güvenlik duvarı ayarlarını, izin verilen IP adresleri listesine kendi barındırılan tümleştirme çalışma zamanı makinesinin IP adresini eklemek için yapılandırın.

> [!NOTE]
> Güvenlik duvarınız giden bağlantı noktası 1433'e izin vermiyorsa, kendi barındırılan tümleştirme çalışma süresi DOĞRUDAN SQL veritabanına erişemez. Bu durumda, SQL Veritabanı ve SQL Veri Ambarı için aşamalı bir [kopyasını](copy-activity-performance.md) kullanabilirsiniz. Bu senaryoda, veri hareketi için yalnızca HTTPS (bağlantı noktası 443) gerekir.

## <a name="proxy-server-considerations"></a>Proxy sunucu hususlar

Kurumsal ağ ortamınız Internet'e erişmek için bir proxy sunucusu kullanıyorsa, uygun proxy ayarlarını kullanmak için kendi barındırılan tümleştirme çalışma süresini yapılandırın. Proxy'yi ilk kayıt aşamasında ayarlayabilirsiniz.

![Proxy'yi belirtin](media/create-self-hosted-integration-runtime/specify-proxy.png)

Yapılandırılan, kendi kendine barındırılan tümleştirme çalışma süresi, bulut hizmetinin kaynağına ve hedefine (HTTP veya HTTPS protokolünü kullanan) bağlanmak için proxy sunucusunu kullanır. Bu nedenle ilk kurulum sırasında **Bağlantıyı Değiştir'i** seçin.

![Proxy'yi ayarlama](media/create-self-hosted-integration-runtime/set-http-proxy.png)

Üç yapılandırma seçeneği vardır:

- **Proxy kullanmayın**: Kendi kendine barındırılan tümleştirme çalışma süresi, bulut hizmetlerine bağlanmak için açıkça herhangi bir proxy kullanmaz.
- **Sistem proxy'sini kullanın**: Kendi kendine barındırılan tümleştirme çalışma süresi, diahost.exe.config ve diawp.exe.config'de yapılandırılan proxy ayarını kullanır. Bu dosyalar proxy yapılandırması belirtmezse, kendi kendine barındırılan tümleştirme çalışma süresi, proxy'den geçmeden doğrudan bulut hizmetine bağlanır.
- **Özel proxy kullanın**: Diahost.exe.config ve diawp.exe.config yapılandırmalarını kullanmak yerine, kendi kendine barındırılan tümleştirme çalışma zamanı için kullanmak üzere HTTP proxy ayarını yapılandırın. **Adres** ve **Bağlantı Noktası** değerleri gereklidir. **Kullanıcı Adı** ve **Parola** değerleri, proxy'nizin kimlik doğrulama ayarına bağlı olarak isteğe bağlıdır. Tüm ayarlar, kendi barındırılan tümleştirme çalışma zamanında Windows DPAPI ile şifrelenir ve makinede yerel olarak depolanır.

Güncelleştirilmiş proxy ayarlarını kaydettikten sonra tümleştirme çalışma zamanı ana bilgisayar hizmeti otomatik olarak yeniden başlatılır.

Proxy ayarlarını görüntülemek veya güncelleştirmek istiyorsanız, kendi kendine barındırılan tümleştirme çalışma zamanını kaydettikten sonra Microsoft Tümleştirme Runtime Configuration Manager'ı kullanın.

1. **Microsoft Tümleştirme Runtime Configuration Manager'ı**açın.
1. **Ayarlar** sekmesini seçin.
1. **HTTP Proxy**altında, Http Proxy **ayarkutusunu ayarlamak** için **Değiştir** bağlantısını seçin.
1. **Sonraki'ni**seçin. Daha sonra proxy ayarını kaydetmek ve tümleştirme çalışma zamanı ana bilgisayar hizmetini yeniden başlatmak için izninizi isteyen bir uyarı görürsünüz.

HTTP proxy'sini görüntülemek ve güncelleştirmek için configuration manager aracını kullanabilirsiniz.

![Proxy'yi görüntüleme ve güncelleştirme](media/create-self-hosted-integration-runtime/view-proxy.png)

> [!NOTE]
> NTLM kimlik doğrulaması olan bir proxy sunucusu ayarlarsanız, tümleştirme çalışma zamanı ana bilgisayar hizmeti etki alanı hesabı altında çalışır. Daha sonra etki alanı hesabının parolasını değiştirirseniz, hizmetin yapılandırma ayarlarını güncelleştirmeyi ve hizmeti yeniden başlatmayı unutmayın. Bu gereksinim nedeniyle, parolayı sık sık güncelleştirmenizi gerektirmeyen özel bir etki alanı hesabı kullanarak proxy sunucusuna erişmenizi öneririz.

### <a name="configure-proxy-server-settings"></a>Proxy sunucu ayarlarını yapılandırma

HTTP proxy için **Kullanım sistemi proxy** seçeneğini seçerseniz, kendi kendine barındırılan tümleştirme çalışma süresi diahost.exe.config ve diawp.exe.config proxy ayarları kullanır. Bu dosyalar proxy belirtmediğinde, kendi kendine barındırılan tümleştirme çalışma süresi, proxy'den geçmeden doğrudan bulut hizmetine bağlanır. Aşağıdaki yordam diahost.exe.config dosyasını güncelleştirmek için yönergeler sağlar:

1. Dosya Gezgini'nde, özgün dosyanın yedeklemesi olarak C:\Program Files\Microsoft Integration Runtime\3.0\Shared\diahost.exe.config'in güvenli bir kopyasını yapın.
1. Yönetici olarak çalışan Notepad'i açın.
1. Not Defteri'nde C:\Program Files\Microsoft Integration Runtime\3.0\Shared\diahost.exe.config metin dosyasını açın.
1. Varsayılan **system.net** etiketini aşağıdaki kodda gösterildiği gibi bulun:

    ```xml
    <system.net>
        <defaultProxy useDefaultCredentials="true" />
    </system.net>
    ```
    Daha sonra aşağıdaki örnekte gösterildiği gibi proxy sunucu ayrıntıları ekleyebilirsiniz:

    ```xml
    <system.net>
        <defaultProxy enabled="true">
              <proxy bypassonlocal="true" proxyaddress="http://proxy.domain.org:8888/" />
        </defaultProxy>
    </system.net>
    ```

    Proxy etiketi, ek özelliklerin '. `scriptLocation` Sözdizimi için [ \<proxy\> Öğesi'ne (Ağ Ayarları)](https://msdn.microsoft.com/library/sa91de1e.aspx) bakın.

    ```xml
    <proxy autoDetect="true|false|unspecified" bypassonlocal="true|false|unspecified" proxyaddress="uriString" scriptLocation="uriString" usesystemdefault="true|false|unspecified "/>
    ```
1. Yapılandırma dosyasını özgün konumuna kaydedin. Ardından, değişiklikleri alan kendi kendine barındırılan tümleştirme çalışma zamanı ana bilgisayar hizmetini yeniden başlatın.

   Hizmeti yeniden başlatmak için Denetim Masası'ndaki hizmetleri kullanın. Veya Tümleştirme Runtime Configuration Manager'dan **Hizmeti Durdur** düğmesini seçin ve ardından **Hizmeti Başlat'ı**seçin.

   Hizmet başlatılmıyorsa, düzenlediğiniz uygulama yapılandırma dosyasına yanlış XML etiket sözdizimini ekleme niz olasıdır.

> [!IMPORTANT]
> Hem diahost.exe.config ve diawp.exe.config güncellemeyi unutmayın.

Microsoft Azure'un şirketinizin izin listesinde olduğundan da emin olmanız gerekir. Geçerli Azure IP adreslerinin listesini [Microsoft Download Center'dan](https://www.microsoft.com/download/details.aspx?id=41653)indirebilirsiniz.

### <a name="possible-symptoms-for-issues-related-to-the-firewall-and-proxy-server"></a>Güvenlik duvarı ve proxy sunucusuyla ilgili sorunlar için olası belirtiler

Aşağıdakiler gibi hata iletileri görürseniz, olası nedeni güvenlik duvarı veya proxy sunucusunun yanlış yapılandırmasıdır. Bu tür yapılandırma, kendi kendine barındırılan tümleştirme çalışma zamanının kendisini doğrulamak için Veri Fabrikası'na bağlanmasını engeller. Güvenlik duvarınızın ve proxy sunucunuzun düzgün şekilde yapılandırıldığından emin olmak için önceki bölüme bakın.

* Kendi kendine barındırılan tümleştirme çalışma zamanını kaydetmeye çalıştığınızda, aşağıdaki hata iletisini alırsınız: "Bu Tümleştirme Runtime düğümkaydedemedi! Kimlik Doğrulama anahtarının geçerli olduğunu ve bu makinede entegrasyon hizmeti ana bilgisayar hizmetinin çalıştığını doğrulayın."
* Tümleştirme Runtime Configuration Manager'ı açtığınızda, **Bağlantısı kesilen** veya **bağlanan**bir durum görürsünüz. Windows olay günlüklerini görüntülediğinizde, **Olay Görüntüleyicisi** > **Uygulaması ve Hizmetleri Microsoft** > **Tümleştirme Çalışma Süresi**altında, bunun gibi hata iletileri görürsünüz:

    ```
    Unable to connect to the remote server
    A component of Integration Runtime has become unresponsive and restarts automatically. Component name: Integration Runtime (Self-hosted).
    ```

### <a name="enable-remote-access-from-an-intranet"></a>Bir intranetten uzaktan erişimi etkinleştirme

PowerShell'i, kendi kendine barındırılan tümleştirme çalışma süresini yüklediğiniz yer dışındaki ağ daki bir makineden kimlik bilgilerini şifrelemek için kullanıyorsanız, **Intranet'ten Uzaktan Erişim** seçeneğini etkinleştirebilirsiniz. Kendi kendine barındırılan tümleştirme çalışma süresini yüklediğiniz makinedeki kimlik bilgilerini şifrelemek için PowerShell'i çalıştırDıysanız, **Intranet'ten Uzaktan Erişimi**etkinleştiremezsiniz.

Yüksek kullanılabilirlik ve ölçeklenebilirlik için başka bir düğüm eklemeden önce **Intranet'ten Uzaktan Erişimi** etkinleştirin.  

Kendi kendine barındırılan tümleştirme çalışma zamanı kurulum sürümünü 3.3 veya daha sonra çalıştırdığınızda, varsayılan olarak kendi kendine barındırılan tümleştirme çalışma zamanı yükleyicisi, **intranet'ten uzaktan erişimi** kendi barındırılan tümleştirme çalışma saatinde devre dışı kalır.

Bir iş ortağından veya başkalarından gelen bir güvenlik duvarı kullandığınızda, bağlantı noktası 8060'ı veya kullanıcı tarafından yapılandırılan bağlantı noktasını el ile açabilirsiniz. Kendi kendine barındırılan tümleştirme çalışma zamanını ayarlarken güvenlik duvarı sorununuz varsa, güvenlik duvarını yapılandırmadan kendi kendine barındırılan tümleştirme çalışma süresini yüklemek için aşağıdaki komutu kullanın:

```
msiexec /q /i IntegrationRuntime.msi NOFIREWALL=1
```

Bağımsız olarak barındırılan tümleştirme çalışma saatinde bağlantı noktası 8060'ı açmamayı seçerseniz, veri depolama kimlik bilgilerini yapılandırmak için Kimlik Bilgilerini Ayarlama uygulaması dışındaki mekanizmaları kullanın. Örneğin, **New-AzDataFactoryV2LinkedServiceEncryptCredential** PowerShell cmdlet kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Adım adım talimatlar için [Bkz. Öğretici: Şirket içi verileri buluta kopyalayın.](tutorial-hybrid-copy-powershell.md)
