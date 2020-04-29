---
title: Şirket içinde barındırılan tümleştirme çalışma zamanı oluşturma
description: Azure Data Factory ' de şirket içinde barındırılan bir tümleştirme çalışma zamanı oluşturmayı öğrenin. Bu, veri fabrikalarının özel bir ağdaki veri depolarına erişmesini sağlar.
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
ms.openlocfilehash: 6bc0f002c6927cfd9a314797663e1dabbac392b6
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81416634"
---
# <a name="create-and-configure-a-self-hosted-integration-runtime"></a>Şirket içinde barındırılan tümleştirme çalışma zamanı oluşturma ve yapılandırma

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Integration Runtime (IR), farklı ağ ortamlarında veri tümleştirme özellikleri sağlamak için Azure Data Factory tarafından kullanılan işlem altyapısıdır. IR ile ilgili ayrıntılar için bkz. [tümleştirme çalışma zamanına genel bakış](concepts-integration-runtime.md).

Şirket içinde barındırılan tümleştirme çalışma zamanı, bir bulut veri deposu ve bir özel ağdaki veri deposu arasında kopyalama etkinliklerini çalıştırabilir. Ayrıca, şirket içi bir ağda veya bir Azure sanal ağında işlem kaynaklarına karşı dönüştürme etkinliklerini de gönderebilir. Şirket içinde barındırılan tümleştirme çalışma zamanının yüklenmesi, şirket içi bir makineye veya özel bir ağ içindeki bir sanal makineye ihtiyaç duyuyor.  

Bu makalede, kendinden konak IR oluşturma ve yapılandırma açıklanmaktadır.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="setting-up-a-self-hosted-integration-runtime"></a>Şirket içinde barındırılan tümleştirme çalışma zamanı ayarlama

Şirket içinde barındırılan bir tümleştirme çalışma zamanı oluşturmak ve ayarlamak için aşağıdaki yordamları kullanın.

### <a name="create-a-self-hosted-ir-via-azure-powershell"></a>Azure PowerShell aracılığıyla kendinden konak IR oluşturma

1. Bu görev için Azure PowerShell kullanabilirsiniz. Örnek aşağıda verilmiştir:

    ```powershell
    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $resourceGroupName -DataFactoryName $dataFactoryName -Name $selfHostedIntegrationRuntimeName -Type SelfHosted -Description "selfhosted IR description"
    ```
  
2. Şirket içinde barındırılan tümleştirme çalışma zamanını yerel bir makineye [indirip](https://www.microsoft.com/download/details.aspx?id=39717) yükleyin.

3. Kimlik doğrulama anahtarını alın ve şirket içinde barındırılan tümleştirme çalışma zamanını anahtarla kaydettirin. Bir PowerShell örneği aşağıda verilmiştir:

    ```powershell

    Get-AzDataFactoryV2IntegrationRuntimeKey -ResourceGroupName $resourceGroupName -DataFactoryName $dataFactoryName -Name $selfHostedIntegrationRuntimeName  

    ```

### <a name="create-a-self-hosted-ir-via-azure-data-factory-ui"></a>Azure Data Factory Kullanıcı arabirimi aracılığıyla kendinden konak IR oluşturma

Azure Data Factory Kullanıcı arabirimini kullanarak şirket içinde barındırılan bir IR oluşturmak için aşağıdaki adımları kullanın.

1. Azure Data Factory Kullanıcı arabiriminin **Başlarken** sayfasında, en soldaki bölmedeki **Yazar** sekmesini seçin.

   ![Giriş sayfası yazar düğmesi](media/doc-common-process/get-started-page-author-button.png)

1. Sol bölmenin altındaki **bağlantıları** seçin ve **Bağlantılar** penceresinde **tümleştirme çalışma zamanları** ' nı seçin. **+ Yeni**seçeneğini belirleyin.

   ![Tümleştirme çalışma zamanı oluşturma](media/create-self-hosted-integration-runtime/new-integration-runtime.png)

1. **Tümleştirme çalışma zamanı kurulumu** sayfasında, **Azure, şirket içinde barındırılan**' ı seçin ve ardından **devam**' ı seçin. 

1. Aşağıdaki sayfada, kendinden konak IR oluşturmak için şirket içinde **barındırılan** ' i seçin ve ardından **devam**' ı seçin.
   ![Kendini barındıran IR oluşturma](media/create-self-hosted-integration-runtime/new-selfhosted-ir.png)

1. IR 'niz için bir ad girin ve **Oluştur**' u seçin.

1. **Tümleştirme çalışma zamanı kurulumu** sayfasında, **1. seçenek** ' in altındaki bağlantıyı seçerek hızlı kurulumu bilgisayarınızda açın. Veya el ile ayarlamak için **2. seçenek** bölümündeki adımları izleyin. Aşağıdaki yönergeler el ile kuruluma dayalıdır:

   ![Tümleştirme çalışma zamanı kurulumu](media/create-self-hosted-integration-runtime/integration-runtime-setting-up.png)

    1. Kimlik doğrulama anahtarını kopyalayıp yapıştırın. **Tümleştirme çalışma zamanını indir ve Yükle ' yi**seçin.

    1. Şirket içinde barındırılan tümleştirme çalışma zamanını yerel Windows makinesine indirin. Yükleyiciyi çalıştırın.

    1. **Kayıt Integration Runtime (Şirket içinde barındırılan)** sayfasında, daha önce kaydettiğiniz anahtarı yapıştırın ve **Kaydet**' i seçin.
    
       ![Tümleştirme çalışma zamanını kaydetme](media/create-self-hosted-integration-runtime/register-integration-runtime.png)

    1. **Yeni Integration Runtime (Şirket içinde barındırılan) düğümü** sayfasında **son**' u seçin.

1. Şirket içinde barındırılan tümleştirme çalışma zamanı başarıyla kaydedildikten sonra aşağıdaki pencereyi görürsünüz:

    ![Başarılı kayıt](media/create-self-hosted-integration-runtime/registered-successfully.png)

### <a name="set-up-a-self-hosted-ir-on-an-azure-vm-via-an-azure-resource-manager-template"></a>Azure Resource Manager şablonu aracılığıyla bir Azure VM üzerinde şirket içinde barındırılan bir IR ayarlama

Şirket içinde barındırılan IR kurulumunu, bir Azure sanal makinesinde [kendi kendine konak IR şablonu oluştur şablonunu](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vms-with-selfhost-integration-runtime)kullanarak otomatik hale getirebilirsiniz. Şablon, bir Azure sanal ağı içinde tamamen işlevsel şirket içinde barındırılan bir IR 'nin kolayca bir yolunu sağlar. IR, düğüm sayısını 2 veya daha yüksek olarak ayarladığınız sürece yüksek kullanılabilirlik ve ölçeklenebilirlik özelliklerine sahiptir.

### <a name="set-up-an-existing-self-hosted-ir-via-local-powershell"></a>Yerel PowerShell aracılığıyla mevcut bir şirket içinde barındırılan IR ayarlama

Şirket içinde barındırılan mevcut bir IR kurmak veya yönetmek için komut satırını kullanabilirsiniz. Bu kullanım özellikle, şirket içinde barındırılan IR düğümlerinin yüklenmesinin ve kaydının otomatikleştirilmesine yardımcı olabilir.

Dmgcmd. exe, şirket içinde barındırılan yükleyicide bulunur. Genellikle C:\Program Files\Microsoft Integration Runtime\3,\shared\ klasöründe bulunur. Bu uygulama, çeşitli parametreleri destekler ve otomasyon için Batch betikleri kullanılarak bir komut satırı aracılığıyla çağrılabilir.

Uygulamayı aşağıdaki gibi kullanın:

```powershell
dmgcmd [ -RegisterNewNode "<AuthenticationKey>" -EnableRemoteAccess "<port>" ["<thumbprint>"] -EnableRemoteAccessInContainer "<port>" ["<thumbprint>"] -DisableRemoteAccess -Key "<AuthenticationKey>" -GenerateBackupFile "<filePath>" "<password>" -ImportBackupFile "<filePath>" "<password>" -Restart -Start -Stop -StartUpgradeService -StopUpgradeService -TurnOnAutoUpdate -TurnOffAutoUpdate -SwitchServiceAccount "<domain\user>" ["<password>"] -Loglevel <logLevel> ]
```

Uygulamanın parametrelerinin ve özelliklerinin ayrıntıları şunlardır: 

| Özellik                                                    | Açıklama                                                  | Gerekli |
| ----------------------------------------------------------- | ------------------------------------------------------------ | -------- |
| **Registernewnode** "`<AuthenticationKey>`"                     | Şirket içinde barındırılan tümleştirme çalışma zamanı düğümünü belirtilen kimlik doğrulama anahtarıyla kaydedin. | Hayır       |
| **Registernewnode** "`<AuthenticationKey>`" "`<NodeName>`"      | Şirket içinde barındırılan tümleştirme çalışma zamanı düğümünü belirtilen kimlik doğrulama anahtarı ve düğüm adıyla kaydedin. | Hayır       |
| **Enableremoteaccess** "`<port>`" ["`<thumbprint>`"]            | Yüksek kullanılabilirlik kümesi ayarlamak için geçerli düğümde uzaktan erişimi etkinleştirin. Ya da Azure Data Factory aracılığıyla doğrudan şirket içinde barındırılan IR 'ye karşı kimlik bilgileri ayarlamayı etkinleştirin. İkinci olarak, aynı ağdaki bir uzak makineden **New-AzDataFactoryV2LinkedServiceEncryptedCredential** cmdlet 'ini kullanarak bunu yapabilirsiniz. | Hayır       |
| **Enableremoteaccessıncontainer** "`<port>`" ["`<thumbprint>`"] | Düğüm bir kapsayıcıda çalıştırıldığında geçerli düğüme uzaktan erişimi etkinleştirin. | Hayır       |
| **DisableRemoteAccess**                                         | Geçerli düğüme uzaktan erişimi devre dışı bırakın. Çok düğümlü kurulum için uzaktan erişim gerekir. **Yeni-AzDataFactoryV2LinkedServiceEncryptedCredential** PowerShell cmdlet 'i, uzaktan erişim devre dışı bırakıldığında bile hala çalışıyor. Bu davranış, cmdlet 'in şirket içinde barındırılan IR düğümüyle aynı makinede yürütüldüğü sürece doğrudur. | Hayır       |
| **Anahtar** "`<AuthenticationKey>`"                                 | Önceki kimlik doğrulama anahtarının üzerine yazın veya güncelleştirin. Bu eyleme dikkat edin. Anahtar yeni bir tümleştirme çalışma zamanı ise, önceki şirket içinde barındırılan IR düğümünüz çevrimdışı duruma geçebilir. | Hayır       |
| **Generatebackupfile** "`<filePath>`" "`<password>`"            | Geçerli düğüm için bir yedek dosya oluşturun. Yedekleme dosyası, düğüm anahtarını ve veri deposu kimlik bilgilerini içerir. | Hayır       |
| **Importbackupfile** "`<filePath>`" "`<password>`"              | Düğümü bir yedekleme dosyasından geri yükleyin.                          | Hayır       |
| **Yeniden Başlatma**                                                     | Şirket içinde barındırılan tümleştirme çalışma zamanı ana bilgisayarı hizmetini yeniden başlatın.   | Hayır       |
| **Başlangıç**                                                       | Şirket içinde barındırılan tümleştirme çalışma zamanı ana bilgisayarı hizmetini başlatın.     | Hayır       |
| **Durdur**                                                        | Şirket içinde barındırılan tümleştirme çalışma zamanı ana bilgisayarı hizmetini durdurun.        | Hayır       |
| **StartUpgradeService**                                         | Şirket içinde barındırılan tümleştirme çalışma zamanı yükseltme hizmetini başlatın.       | Hayır       |
| **StopUpgradeService**                                          | Şirket içinde barındırılan tümleştirme çalışma zamanı yükseltme hizmetini durdurun.        | Hayır       |
| **Turnonotomatik güncelleştirme**                                            | Şirket içinde barındırılan tümleştirme çalışma zamanı otomatik güncelleştirmesini açın.        | Hayır       |
| **TurnOffAutoUpdate**                                           | Şirket içinde barındırılan tümleştirme çalışma zamanı otomatik güncelleştirmesini devre dışı bırakın.       | Hayır       |
| **Switchserviceaccount** "`<domain\user>`" ["`<password>`"]           | DIAHostService ' i yeni bir hesap olarak çalışacak şekilde ayarlayın. Sistem hesapları ve sanal hesaplar için "" boş parolasını kullanın. | Hayır       |


## <a name="command-flow-and-data-flow"></a>Komut akışı ve veri akışı

Şirket içi ve bulut arasında veri taşıdığınızda, etkinlik şirket içi veri kaynağı ve bulut arasında veri aktarmak için şirket içinde barındırılan bir tümleştirme çalışma zamanı kullanır.

Şirket içinde barındırılan bir IR ile kopyalamaya yönelik veri akışı adımlarının üst düzey bir özeti aşağıda verilmiştir:

![Veri akışına üst düzey genel bakış](media/create-self-hosted-integration-runtime/high-level-overview.png)

1. Bir veri geliştiricisi, bir PowerShell cmdlet 'i kullanarak bir Azure Data Factory içinde şirket içinde barındırılan bir tümleştirme çalışma zamanı oluşturur. Şu anda Azure portal bu özelliği desteklemiyor.
1. Veri geliştiricisi, şirket içi veri deposu için bağlı bir hizmet oluşturur. Geliştirici bunu, hizmetin veri depolarına bağlanmak için kullanması gereken şirket içinde barındırılan tümleştirme çalışma zamanı örneğini belirterek yapar.
1. Şirket içinde barındırılan tümleştirme çalışma zamanı düğümü, Windows Data Protection uygulama programlama arabirimini (DPAPI) kullanarak kimlik bilgilerini şifreler ve kimlik bilgilerini yerel olarak kaydeder. Yüksek kullanılabilirlik için birden çok düğüm ayarlandıysa, kimlik bilgileri diğer düğümlerde daha fazla eşitlenir. Her düğüm, DPAPI kullanarak kimlik bilgilerini şifreler ve yerel olarak depolar. Kimlik bilgisi eşitleme, veri geliştiricisi tarafından saydamdır ve şirket içinde barındırılan IR tarafından işlenir.
1. Azure Data Factory işleri zamanlamak ve yönetmek için şirket içinde barındırılan tümleştirme çalışma zamanı ile iletişim kurar. İletişim, paylaşılan bir [Azure Service Bus geçişi](https://docs.microsoft.com/azure/service-bus-relay/relay-what-is-it#wcf-relay) bağlantısı kullanan bir denetim kanalı üzerinden yapılır. Bir etkinlik işinin çalıştırılması gerektiğinde, isteği herhangi bir kimlik bilgisi bilgisiyle birlikte Data Factory kuyruğa alır. Bu durum, kimlik bilgilerinin şirket içinde barındırılan tümleştirme çalışma zamanı 'nda zaten depolanmaması durumunda olur. Şirket içinde barındırılan tümleştirme çalışma zamanı, kuyruğu yokladıktan sonra işi başlatır.
1. Şirket içinde barındırılan tümleştirme çalışma zamanı, verileri şirket içi bir mağaza ve bulut depolama arasında kopyalar. Kopyanın yönü, kopyalama etkinliğinin veri ardışık düzeninde nasıl yapılandırıldığına bağlıdır. Bu adım için, şirket içinde barındırılan tümleştirme çalışma zamanı, güvenli bir HTTPS kanalı üzerinden Azure Blob depolama gibi bulut tabanlı depolama hizmetleriyle doğrudan iletişim kurar.

## <a name="considerations-for-using-a-self-hosted-ir"></a>Şirket içinde barındırılan IR kullanma konuları

- Birden çok şirket içi veri kaynağı için şirket içinde barındırılan tek bir tümleştirme çalışma zamanı kullanabilirsiniz. Aynı zamanda aynı Azure Active Directory (Azure AD) kiracısında bulunan başka bir veri fabrikasıyla de paylaşabilirsiniz. Daha fazla bilgi için bkz. [Şirket içinde barındırılan tümleştirme çalışma zamanını paylaşma](#create-a-shared-self-hosted-integration-runtime-in-azure-data-factory).
- Şirket içinde barındırılan tümleştirme çalışma zamanının yalnızca bir örneğini tek bir makineye yükleyebilirsiniz. Şirket içi veri kaynaklarına erişmesi gereken iki veri fabrikası varsa, şirket içinde barındırılan IR paylaşmak için şirket içinde barındırılan IR [paylaşma özelliğini](#create-a-shared-self-hosted-integration-runtime-in-azure-data-factory) kullanın veya her bir veri fabrikası için bir tane olmak üzere iki şirket içi bilgisayara şirket IÇINDE barındırılan IR 'yi yükleyebilirsiniz.  
- Şirket içinde barındırılan tümleştirme çalışma zamanının veri kaynağıyla aynı makinede olması gerekmez. Ancak, şirket içinde barındırılan tümleştirme çalışma zamanının veri kaynağına yakın olması, şirket içinde barındırılan tümleştirme çalışma zamanının veri kaynağına bağlanması için geçen süreyi azaltır. Şirket içinde barındırılan tümleştirme çalışma zamanını, şirket içi veri kaynağını barındıran bilgisayardan farklı bir makineye yüklemenizi öneririz. Şirket içinde barındırılan tümleştirme çalışma zamanı ve veri kaynağı farklı makinelerinizde, şirket içinde barındırılan tümleştirme çalışma zamanı, kaynaklar için veri kaynağıyla rekabet etmez.
- Aynı şirket içi veri kaynağına bağlanan farklı makinelerde şirket içinde barındırılan birden çok tümleştirme çalışma zamanı olabilir. Örneğin, iki veri fabrikası sunan iki şirket içinde barındırılan tümleştirme çalışma zamanı varsa, aynı şirket içi veri kaynağı her iki veri fabrikasıyla de kaydedilebilir.
- Bilgisayarınızda Power BI senaryoya yönelik olarak yüklü bir ağ geçidiniz varsa, başka bir makineye Data Factory için otomatik olarak barındırılan ayrı bir tümleştirme çalışma zamanı yükleyebilirsiniz.
- Bir Azure sanal ağı içindeki veri tümleştirmesini desteklemek için şirket içinde barındırılan bir tümleştirme çalışma zamanı kullanın.
- Azure ExpressRoute kullandığınızda bile, veri kaynağınızı bir güvenlik duvarının arkasındaki şirket içi veri kaynağı olarak değerlendirin. Hizmeti veri kaynağına bağlamak için şirket içinde barındırılan tümleştirme çalışma zamanını kullanın.
- Veri deposu bir Azure hizmet olarak altyapı (IaaS) sanal makinesi üzerinde bulutta olsa bile, şirket içinde barındırılan tümleştirme çalışma zamanını kullanın.
- Görevler, FIPS uyumlu şifreleme etkin olan bir Windows Server 'a yüklediğiniz şirket içinde barındırılan tümleştirme çalışma zamanında başarısız olabilir. Bu sorunu geçici olarak çözmek için sunucuda FIPS uyumlu şifrelemeyi devre dışı bırakın. FIPS uyumlu şifrelemeyi devre dışı bırakmak için, aşağıdaki kayıt defteri alt anahtarının değerini 1 (etkin) iken 0 (devre dışı) olarak `HKLM\System\CurrentControlSet\Control\Lsa\FIPSAlgorithmPolicy\Enabled`değiştirin:.

## <a name="prerequisites"></a>Ön koşullar

- Desteklenen Windows sürümleri şunlardır:
  + Windows 7 Service Pack 1
  + Windows 8.1
  + Windows 10
  + Windows Server 2008 R2 SP1
  + Windows Server 2012
  + Windows Server 2012 R2
  + Windows Server 2016
  + Windows Server 2019
   
   Şirket içinde barındırılan tümleştirme çalışma zamanının bir etki alanı denetleyicisine yüklenmesi desteklenmez.
- .NET Framework 4.6.1 veya üzeri gereklidir. Şirket içinde barındırılan tümleştirme çalışma zamanını bir Windows 7 makinesine yüklüyorsanız, .NET Framework 4.6.1 veya üstünü yükleyebilirsiniz. Ayrıntılar için [.NET Framework sistem gereksinimleri](/dotnet/framework/get-started/system-requirements) ' ne bakın.
- Şirket içinde barındırılan tümleştirme çalışma zamanı makinesi için önerilen en düşük yapılandırma, 4 çekirdek, 8 GB RAM ve 80 GB kullanılabilir sabit disk alanı olan 2 GHz bir işlemcisidir.
- Ana makine hazırda beklemesi durumunda, şirket içinde barındırılan tümleştirme çalışma zamanı veri isteklerine yanıt vermez. Şirket içinde barındırılan tümleştirme çalışma zamanını yüklemeden önce bilgisayarda uygun bir güç planı yapılandırın. Makine hazırda beklemeye yapılandırıldıysa, şirket içinde barındırılan tümleştirme çalışma zamanı yükleyicisi bir iletiyle uyarır.
- Şirket içinde barındırılan tümleştirme çalışma zamanını başarılı bir şekilde yüklemek ve yapılandırmak için makinede yönetici olmanız gerekir.
- Kopyalama-etkinlik çalıştırmaları belirli bir sıklıkta gerçekleşir. Makinedeki işlemci ve RAM kullanımı, yoğun ve boşta zamanlarla aynı düzene uyar. Kaynak kullanımı Ayrıca, taşınan veri miktarına büyük ölçüde bağlıdır. Birden çok kopyalama işi devam ederken, yoğun saatlerde kaynak kullanımını görürsünüz.
- , Parquet, ORC veya avro biçimlerinde veri ayıklama sırasında görevler başarısız olabilir. Parquet hakkında daha fazla bilgi için [Azure Data Factory Içindeki Parquet biçimine](https://docs.microsoft.com/azure/data-factory/format-parquet#using-self-hosted-integration-runtime)bakın. Dosya oluşturma, şirket içinde barındırılan tümleştirme makinesinde çalışır. Dosya oluşturma, beklendiği gibi çalışması için aşağıdaki önkoşulları gerektirir:
    - [Visual C++ 2010 yeniden dağıtılabilir](https://download.microsoft.com/download/3/2/2/3224B87F-CFA0-4E70-BDA3-3DE650EFEBA5/vcredist_x64.exe) Paket (x64)
    - Java Runtime (JRE) sürüm 8, bir JRE sağlayıcısından, [OpenJDK 'Yi benimseyin](https://adoptopenjdk.net/). `JAVA_HOME` Ortam değişkeninin ayarlandığından emin olun.

## <a name="installation-best-practices"></a>Yükleme için en iyi yöntemler

Şirket içinde barındırılan tümleştirme çalışma zamanını, [Microsoft Indirme merkezi](https://www.microsoft.com/download/details.aspx?id=39717)' nden yönetilen bir kimlik kurulum paketini indirerek yükleyebilirsiniz. Adım adım yönergeler için [verileri şirket içi ve bulut arasında taşıma](tutorial-hybrid-copy-powershell.md) makalesine bakın.

- Makinenin hazırda bekletmeden olmaması için, şirket içinde barındırılan tümleştirme çalışma zamanı için konak makinede bir güç planı yapılandırın. Ana makine hazırda bekletmeye başlarsa, şirket içinde barındırılan tümleştirme çalışma zamanı çevrimdışı olarak geçer.
- Şirket içinde barındırılan tümleştirme çalışma zamanı ile ilişkili kimlik bilgilerini düzenli olarak yedekleyin.
- Şirket içinde barındırılan IR kurulum işlemlerini otomatik hale getirmek için lütfen [PowerShell aracılığıyla mevcut bir şirket içinde BARıNDıRıLAN IR ayarlama](#setting-up-a-self-hosted-integration-runtime)bölümüne bakın.  

## <a name="install-and-register-a-self-hosted-ir-from-microsoft-download-center"></a>Microsoft Indirme merkezi 'nden şirket içinde barındırılan bir IR yükleme ve kaydetme

1. [Microsoft tümleştirme çalışma zamanı indirme sayfasına](https://www.microsoft.com/download/details.aspx?id=39717)gidin.
1. **İndir**' i seçin, 64 bit sürümünü seçin ve **İleri**' yi seçin. 32 bit sürümü desteklenmiyor.
1. Yönetilen kimlik dosyasını doğrudan çalıştırın veya sabit sürücünüze kaydedin ve çalıştırın.
1. **Hoş geldiniz** penceresinde bir dil seçin ve **İleri**' yi seçin.
1. Microsoft yazılımı lisans koşulları 'nı kabul edin ve **İleri ' yi**seçin.
1. Şirket içinde barındırılan tümleştirme çalışma zamanını yüklemek için **klasör** ' ü seçin ve **İleri**' yi seçin.
1. **Yüklemeye hazırlanma** sayfasında, **yükler**' i seçin.
1. Yüklemeyi tamamlamaya **son** ' u seçin.
1. PowerShell kullanarak kimlik doğrulama anahtarını alın. Kimlik doğrulama anahtarını almaya yönelik bir PowerShell örneği aşağıda verilmiştir:

    ```powershell
    Get-AzDataFactoryV2IntegrationRuntimeKey -ResourceGroupName $resourceGroupName -DataFactoryName $dataFactoryName -Name $selfHostedIntegrationRuntime
    ```

1. Makinenizde çalışan Microsoft Integration Runtime Configuration Manager **Integration Runtime (Şirket içinde barındırılan) Kaydet** penceresinde aşağıdaki adımları uygulayın:

    1. Kimlik doğrulama anahtarını metin alanına yapıştırın.

    1. İsteğe bağlı olarak, anahtar metnini görmek için **kimlik doğrulama anahtarını göster** ' i seçin.

    1. **Kaydol**’u seçin.

## <a name="high-availability-and-scalability"></a>Yüksek kullanılabilirlik ve ölçeklenebilirlik

Şirket içinde barındırılan bir tümleştirme çalışma zamanını Azure 'da birden çok şirket içi makine veya sanal makine ile ilişkilendirebilirsiniz. Bu makinelere düğüm denir. Şirket içinde barındırılan bir tümleştirme çalışma zamanı ile ilişkili en fazla dört düğümünüz olabilir. Mantıksal bir ağ geçidi için ağ geçidi yüklü olan şirket içi makinelerde birden çok düğüme sahip olmanın avantajları şunlardır:

* Şirket içinde barındırılan tümleştirme çalışma zamanının, büyük veri çözümünüzde veya Data Factory bulut veri tümleştirmesinde artık tek hata noktası olmaması için daha yüksek kullanılabilirlik. Bu kullanılabilirlik, en fazla dört düğüm kullandığınızda devamlılığını sağlamaya yardımcı olur.
* Şirket içi ve bulut veri depoları arasında veri taşıma sırasında geliştirilmiş performans ve verimlilik. [Performans karşılaştırmaları](copy-activity-performance.md)hakkında daha fazla bilgi alın.

Şirket içinde barındırılan tümleştirme çalışma zamanı yazılımını [Indirme merkezi](https://www.microsoft.com/download/details.aspx?id=39717)' nden yükleyerek birden çok düğümü ilişkilendirebilirsiniz. Ardından, [öğreticide](tutorial-hybrid-copy-powershell.md)açıklandığı gibi **New-AzDataFactoryV2IntegrationRuntimeKey** cmdlet 'inden alınan kimlik doğrulama anahtarlarından birini kullanarak kaydedin.

> [!NOTE]
> Her bir düğümü ilişkilendirmek için şirket içinde barındırılan yeni bir tümleştirme çalışma zamanı oluşturmanız gerekmez. Şirket içinde barındırılan tümleştirme çalışma zamanını başka bir makineye yükleyebilir ve aynı kimlik doğrulama anahtarını kullanarak kaydedebilirsiniz.

> [!NOTE]
> Yüksek kullanılabilirlik ve ölçeklenebilirlik için başka bir düğüm eklemeden önce, ilk düğümde **Intranete uzaktan erişim** seçeneğinin etkinleştirildiğinden emin olun. Bunu yapmak için **Microsoft Integration Runtime Configuration Manager** > **ayarları** > **intranete uzaktan erişim**' i seçin.

### <a name="scale-considerations"></a>Ölçek konuları

#### <a name="scale-out"></a>Ölçeği genişletme

İşlemci kullanımı yüksek olduğunda ve kullanılabilir bellek, şirket içinde barındırılan IR üzerinde düşükse, yük makineler arasında ölçeklendirmeye yardımcı olmak için yeni bir düğüm ekleyin. Etkinlikler, zaman aşımına uğradığından veya şirket içinde barındırılan IR düğümü çevrimdışı olduğu için başarısız olursa, ağ geçidine bir düğüm eklemenize yardımcı olur.

#### <a name="scale-up"></a>Ölçeği artırma

İşlemci ve kullanılabilir RAM iyi bir şekilde kullanıldığında, ancak eşzamanlı işlerin yürütülmesi düğümün sınırlarına ulaştığında, bir düğümün çalıştırabileceği eşzamanlı iş sayısını artırarak ölçeği artırır. Ayrıca, şirket içinde barındırılan IR aşırı yüklendiği için etkinliklerin zaman aşımına uğradığından ölçeğini ölçeklendirmek isteyebilirsiniz. Aşağıdaki görüntüde gösterildiği gibi, bir düğüm için maksimum kapasiteyi artırabilirsiniz:  

![Bir düğümde çalışabilecek eşzamanlı işlerin sayısını artırın](media/create-self-hosted-integration-runtime/scale-up-self-hosted-IR.png)

### <a name="tlsssl-certificate-requirements"></a>TLS/SSL sertifikası gereksinimleri

Tümleştirme çalışma zamanı düğümleri arasındaki iletişimin güvenliğini sağlamak için kullandığınız TLS/SSL sertifikası için gereksinimler şunlardır:

- Sertifika, genel olarak güvenilen bir x509 v3 sertifikası olmalıdır. Ortak iş ortağı sertifika yetkilisi (CA) tarafından verilen sertifikaları kullanmanızı öneririz.
- Her Integration Runtime düğümü bu sertifikaya güvenmelidir.
- Yalnızca son SAN öğesi kullanıldığından, konu diğer adı (SAN) sertifikalarını önermiyoruz. Diğer tüm SAN öğeleri yok sayılır. Örneğin, San 'Lar **node1.domain.contoso.com** ve **node2.DOMAIN.contoso.com**olan bir San sertifikanız varsa, bu sertifikayı yalnızca tam etkı alanı adı (FQDN) **node2.domain.contoso.com**olan bir makinede kullanabilirsiniz.
- Sertifika, TLS/SSL sertifikaları için Windows Server 2012 R2 tarafından desteklenen herhangi bir anahtar boyutunu kullanabilir.
- CNG anahtarları kullanan sertifikalar desteklenmez.  

> [!NOTE]
> Bu sertifika kullanılır:
>
> - Şirket içinde barındırılan IR düğümündeki bağlantı noktalarını şifrelemek için.
> - Düğümler arasında bağlı hizmetlerin kimlik bilgileri eşitlemesini içeren durum eşitlemeye yönelik düğümden düğüme iletişim için.
> - Bir PowerShell cmdlet 'i, bir yerel ağ içinden bağlı hizmet kimlik bilgisi ayarları için kullanıldığında.
>
> Özel ağ ortamınız güvenli değilse veya özel ağınız içindeki düğümler arasındaki iletişimin güvenliğini sağlamak istiyorsanız bu sertifikayı kullanmanızı öneririz.
>
> Şirket içinde barındırılan bir IR 'den diğer veri depolarına geçişte veri taşıma işlemi, bu sertifikanın ayarlanmış olup olmamasına bakılmaksızın her zaman şifrelenmiş bir kanalda gerçekleşir.

## <a name="create-a-shared-self-hosted-integration-runtime-in-azure-data-factory"></a>Azure Data Factory içinde, şirket içinde barındırılan, paylaşılan bir tümleştirme çalışma zamanı oluşturma

Zaten bir veri fabrikasında ayarlamış olduğunuz mevcut, şirket içinde barındırılan bir tümleştirme çalışma zamanı altyapısını yeniden kullanabilirsiniz. Bu yeniden kullanım, var olan bir paylaşılan şirket içinde barındırılan IR 'ye başvurarak farklı bir veri fabrikasında bağlı bir şirket içinde barındırılan tümleştirme çalışma zamanı oluşturmanıza olanak sağlar.

Bu özelliğin bir giriş ve tanıtım durumunu görmek için aşağıdaki 12 dakikalık videoyu izleyin:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Hybrid-data-movement-across-multiple-Azure-Data-Factories/player]

### <a name="terminology"></a>Terminoloji

- **PAYLAŞıLAN IR**: fiziksel bir altyapı üzerinde çalışan, özgün bir şirket IÇINDE barındırılan IR.  
- **Bağlı IR**: başka BIR paylaşılan IR 'ye başvuran bir IR. Bağlı IR mantıksal bir IR ve diğer bir paylaşılan şirket içinde barındırılan IR altyapısını kullanır.

### <a name="methods-to-share-a-self-hosted-integration-runtime"></a>Şirket içinde barındırılan tümleştirme çalışma zamanını paylaşma yöntemleri

Şirket içinde barındırılan tümleştirme çalışma zamanını birden çok veri fabrikası ile paylaşmak için bkz. Ayrıntılar için [paylaşılan bir şirket içinde barındırılan tümleştirme çalışma zamanı oluşturma](create-shared-self-hosted-integration-runtime-powershell.md) .

### <a name="monitoring"></a>İzleme

#### <a name="shared-ir"></a>Paylaşılan IR

![Paylaşılan tümleştirme çalışma zamanı bulma seçimleri](media/create-self-hosted-integration-runtime/Contoso-shared-IR.png)

![Paylaşılan tümleştirme çalışma zamanını izleme](media/create-self-hosted-integration-runtime/contoso-shared-ir-monitoring.png)

#### <a name="linked-ir"></a>Bağlı IR

![Bağlantılı tümleştirme çalışma zamanı bulma seçimleri](media/create-self-hosted-integration-runtime/Contoso-linked-ir.png)

![Bağlantılı tümleştirme çalışma zamanını izleme](media/create-self-hosted-integration-runtime/Contoso-linked-ir-monitoring.png)

### <a name="known-limitations-of-self-hosted-ir-sharing"></a>Şirket içinde barındırılan IR paylaşımının bilinen sınırlamaları

* Bağlı bir IR 'nin oluşturulduğu veri fabrikası, [yönetilen bir kimliğe](https://docs.microsoft.com/azure/active-directory/managed-service-identity/overview)sahip olmalıdır. Varsayılan olarak, Azure portal veya PowerShell cmdlet 'lerinde oluşturulan veri fabrikaları örtük olarak oluşturulmuş bir yönetilen kimliğe sahiptir. Ancak, bir veri fabrikası Azure Resource Manager şablonu veya SDK aracılığıyla oluşturulduğunda, **kimlik** özelliğini açıkça ayarlamanız gerekir. Bu ayar Kaynak Yöneticisi, yönetilen bir kimlik içeren bir veri fabrikası oluşturmayı sağlar.

* Bu özelliği destekleyen Data Factory .NET SDK sürümü 1.1.0 veya üzeri olmalıdır.

* İzin vermek için, paylaşılan IR 'nin bulunduğu veri fabrikasında sahip rolü veya devralınan sahip rolü gerekir.

* Paylaşma özelliği yalnızca aynı Azure AD kiracısındaki veri fabrikaları için geçerlidir.

* Azure AD [Konuk kullanıcıları](https://docs.microsoft.com/azure/active-directory/governance/manage-guest-access-with-access-reviews)için, bir arama anahtar sözcüğü kullanarak tüm veri fabrikalarını LISTELEYEN Kullanıcı arabirimindeki arama işlevleri [çalışmaz](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes#SearchLimits). Ancak Konuk Kullanıcı veri fabrikasının sahibi olduğu sürece, arama işlevi olmadan IR 'yi paylaşabilirsiniz. IR paylaşması gereken veri fabrikasının yönetilen kimliği için, **ata izin** kutusuna bu yönetilen kimliği girin ve Data Factory Kullanıcı arabiriminde **Ekle** ' yi seçin.

  > [!NOTE]
  > Bu özellik yalnızca Data Factory v2 sürümünde kullanılabilir.

## <a name="notification-area-icons-and-notifications"></a>Bildirim alanı simgeleri ve bildirimleri

İmlecinizi bildirim alanındaki simgenin veya iletinin üzerine taşırsanız, şirket içinde barındırılan tümleştirme çalışma zamanının durumuyla ilgili ayrıntıları görebilirsiniz.

![Bildirim alanındaki bildirimler](media/create-self-hosted-integration-runtime/system-tray-notifications.png)

## <a name="ports-and-firewalls"></a>Bağlantı noktaları ve güvenlik duvarları

Dikkate alınması gereken iki güvenlik duvarı vardır:

- Kuruluşun merkezi yönlendiricisinde çalışan *Kurumsal güvenlik duvarı*
- Şirket içinde barındırılan tümleştirme çalışma zamanının yüklü olduğu yerel makinede bir daemon olarak yapılandırılan *Windows Güvenlik Duvarı*

![Güvenlik duvarları](media/create-self-hosted-integration-runtime/firewall.png)

Şirket güvenlik duvarı düzeyinde, aşağıdaki etki alanlarını ve giden bağlantı noktalarını yapılandırmanız gerekir:

[!INCLUDE [domain-and-outbound-port-requirements](../../includes/domain-and-outbound-port-requirements.md)]

Windows güvenlik duvarı düzeyinde veya makine düzeyinde, bu giden bağlantı noktaları normalde etkindir. Bu değillerse, şirket içinde barındırılan tümleştirme çalışma zamanı makinesindeki etki alanlarını ve bağlantı noktalarını yapılandırabilirsiniz.

> [!NOTE]
> Kaynak ve havuzları temel alarak, kurumsal güvenlik duvarınızdaki veya Windows güvenlik duvarında ek etki alanlarına ve giden bağlantı noktalarına izin vermeniz gerekebilir.
>
> Azure SQL veritabanı ve Azure Data Lake gibi bazı bulut veritabanlarında, kendi güvenlik duvarı yapılandırmasındaki şirket içinde barındırılan tümleştirme çalışma zamanı makinelerinin IP adreslerine izin vermeniz gerekebilir.

### <a name="copy-data-from-a-source-to-a-sink"></a>Verileri bir kaynaktan havuza kopyalama

Şirket güvenlik duvarında güvenlik duvarı kurallarını, şirket içinde barındırılan tümleştirme çalışma zamanı makinesinin Windows güvenlik duvarını ve veri deposunun kendisini düzgün şekilde etkinleştirdiğinizden emin olun. Bu kuralların etkinleştirilmesi, şirket içinde barındırılan tümleştirme çalışma zamanının hem kaynak hem de havuza başarıyla bağlanmasını sağlar. Kopyalama işleminde yer alan her bir veri deposu için kuralları etkinleştirin.

Örneğin, şirket içi bir veri deposundan bir SQL veritabanı havuzuna veya bir Azure SQL veri ambarı havuzuna kopyalamak için aşağıdaki adımları uygulayın:

1. Hem Windows Güvenlik Duvarı hem de kurumsal güvenlik duvarı için bağlantı noktası 1433 üzerinde giden TCP iletişimine izin verin.
1. Şirket içinde barındırılan tümleştirme çalışma zamanı makinesinin IP adresini izin verilen IP adresleri listesine eklemek için SQL veritabanının güvenlik duvarı ayarlarını yapılandırın.

> [!NOTE]
> Güvenlik duvarınız giden bağlantı noktası 1433 ' a izin vermezse, şirket içinde barındırılan tümleştirme çalışma zamanı SQL veritabanına doğrudan erişemez. Bu durumda, [hazırlanan bir KOPYAYı](copy-activity-performance.md) SQL VERITABANı ve SQL veri ambarı 'na kullanabilirsiniz. Bu senaryoda, veri taşıma için yalnızca HTTPS (bağlantı noktası 443) gerekir.

## <a name="proxy-server-considerations"></a>Proxy sunucusu konuları

Şirket ağı ortamınız Internet 'e erişmek için bir proxy sunucusu kullanıyorsa, kendi kendine barındırılan tümleştirme çalışma zamanını uygun proxy ayarlarını kullanacak şekilde yapılandırın. İlk kayıt aşamasında ara sunucu ayarlayabilirsiniz.

![Proxy 'yi belirtin](media/create-self-hosted-integration-runtime/specify-proxy.png)

Yapılandırıldığında, şirket içinde barındırılan tümleştirme çalışma zamanı, bulut hizmetinin kaynağına ve hedefine (HTTP veya HTTPS protokolünü kullanan) bağlanmak için proxy sunucusunu kullanır. İlk kurulum sırasında **Bağlantıyı Değiştir** ' i seçmenizin nedeni budur.

![Proxy 'yi ayarlama](media/create-self-hosted-integration-runtime/set-http-proxy.png)

Üç yapılandırma seçeneği vardır:

- **Proxy kullanma**: şirket içinde barındırılan tümleştirme çalışma zamanı, bulut hizmetlerine bağlanmak için hiçbir proxy 'yi açık olarak kullanmaz.
- **Sistem proxy 'Si kullan**: şirket içinde barındırılan tümleştirme çalışma zamanı diahost. exe. config ve diawp. exe. config içinde yapılandırılan proxy ayarını kullanır. Bu dosyalar hiçbir proxy yapılandırması belirtmeyecekse, şirket içinde barındırılan tümleştirme çalışma zamanı, bir ara sunucuya geçmeden doğrudan bulut hizmetine bağlanır.
- **Özel proxy kullan**: diahost. exe. config ve diawp. exe. config içinde yapılandırma kullanmak yerine, şirket içinde barındırılan tümleştirme çalışma zamanı IÇIN kullanılacak http proxy ayarını yapılandırın. **Adres** ve **bağlantı noktası** değerleri gereklidir. Proxy 'nin kimlik doğrulama ayarına bağlı olarak, **Kullanıcı adı** ve **parola** değerleri isteğe bağlıdır. Tüm ayarlar, şirket içinde barındırılan tümleştirme çalışma zamanı üzerinde Windows DPAPI ile şifrelenir ve makinede yerel olarak depolanır.

Tümleştirme çalışma zamanı konak hizmeti, güncelleştirilmiş proxy ayarlarını kaydettikten sonra otomatik olarak yeniden başlatılır.

Şirket içinde barındırılan tümleştirme çalışma zamanını kaydettikten sonra, proxy ayarlarını görüntülemek veya güncelleştirmek istiyorsanız Microsoft Integration Runtime Configuration Manager kullanın.

1. **Microsoft Integration Runtime Configuration Manager**açın.
1. **Ayarlar** sekmesini seçin.
1. **Http proxy**altında bağlantıyı **Değiştir** ' i seçerek **http proxy 'yi ayarla** iletişim kutusunu açın.
1. **İleri**’yi seçin. Ardından, proxy ayarını kaydetme izninizin olduğunu soran bir uyarı görürsünüz ve Integration Runtime ana bilgisayar hizmetini yeniden başlatın.

HTTP proxy 'sini görüntülemek ve güncelleştirmek için Configuration Manager aracını kullanabilirsiniz.

![Ara sunucuyu görüntüleme ve güncelleştirme](media/create-self-hosted-integration-runtime/view-proxy.png)

> [!NOTE]
> NTLM kimlik doğrulaması ile bir ara sunucu ayarlarsanız, tümleştirme çalışma zamanı konak hizmeti etki alanı hesabı altında çalışır. Daha sonra etki alanı hesabının parolasını değiştirirseniz, hizmetin yapılandırma ayarlarını güncelleştirmeyi ve hizmeti yeniden başlatmayı unutmayın. Bu gereksinim nedeniyle, parolayı sık sık güncelleştirmenizi gerektirmeyen ayrılmış bir etki alanı hesabı kullanarak ara sunucuya erişmenizi öneririz.

### <a name="configure-proxy-server-settings"></a>Proxy sunucusu ayarlarını yapılandırma

HTTP proxy için **sistem proxy kullan** seçeneğini belirlerseniz, şirket içinde barındırılan tümleştirme çalışma zamanı, diahost. exe. config ve diawp. exe. config dosyasındaki proxy ayarlarını kullanır. Bu dosyalar proxy belirtmeksizin, şirket içinde barındırılan tümleştirme çalışma zamanı, bir ara sunucuya geçmeden doğrudan bulut hizmetine bağlanır. Aşağıdaki yordam diahost. exe. config dosyasını güncelleştirmek için yönergeler sağlar:

1. Dosya Gezgini 'nde, özgün dosyanın bir yedeklemesi olarak C:\Program Files\Microsoft Integration Runtime\3.0\Shared\diahost.exe.config ' ın güvenli bir kopyasını oluşturun.
1. Yönetici olarak çalışan Not defteri 'Ni açın.
1. Not defteri 'nde C:\Program Files\Microsoft Integration Runtime\3.0\Shared\diahost.exe.config. metin dosyasını açın.
1. Varsayılan **System.net** etiketini aşağıdaki kodda gösterildiği gibi bulun:

    ```xml
    <system.net>
        <defaultProxy useDefaultCredentials="true" />
    </system.net>
    ```
    Daha sonra aşağıdaki örnekte gösterildiği gibi proxy sunucu ayrıntılarını ekleyebilirsiniz:

    ```xml
    <system.net>
        <defaultProxy enabled="true">
              <proxy bypassonlocal="true" proxyaddress="http://proxy.domain.org:8888/" />
        </defaultProxy>
    </system.net>
    ```

    Proxy etiketi, ek özelliklerin gibi `scriptLocation`gerekli ayarları belirlemesine izin verir. Sözdizimi için bkz [ \<. proxy\> öğesi (ağ ayarları)](https://msdn.microsoft.com/library/sa91de1e.aspx) .

    ```xml
    <proxy autoDetect="true|false|unspecified" bypassonlocal="true|false|unspecified" proxyaddress="uriString" scriptLocation="uriString" usesystemdefault="true|false|unspecified "/>
    ```
1. Yapılandırma dosyasını özgün konumuna kaydedin. Ardından, değişiklikleri sağlayan şirket içinde barındırılan tümleştirme çalışma zamanı ana bilgisayarı hizmetini yeniden başlatın.

   Hizmeti yeniden başlatmak için Denetim Masası 'ndan hizmetler uygulamasını kullanın. Ya da Integration Runtime Configuration Manager, **hizmeti Durdur** düğmesini seçin ve ardından **Hizmeti Başlat**' ı seçin.

   Hizmet başlatılmazsa, büyük olasılıkla düzenlediğiniz uygulama yapılandırma dosyasında yanlış XML etiketi söz dizimi eklemiş olursunuz.

> [!IMPORTANT]
> Hem diahost. exe. config hem de diawp. exe. config ' i güncelleştirmeyi unutmayın.

Ayrıca, Microsoft Azure şirketinizin izin verilenler listesinde olduğundan emin olmanız gerekir. Geçerli Azure IP adreslerinin listesini [Microsoft Indirme merkezi](https://www.microsoft.com/download/details.aspx?id=41653)' nden indirebilirsiniz.

### <a name="possible-symptoms-for-issues-related-to-the-firewall-and-proxy-server"></a>Güvenlik Duvarı ve ara sunucu ile ilgili sorunlar için olası belirtiler

Aşağıdakiler gibi hata iletileri görürseniz, olası neden güvenlik duvarının veya ara sunucunun yapılandırması hatalı olur. Bu tür yapılandırma, şirket içinde barındırılan tümleştirme çalışma zamanının kendi kimliğini doğrulamak için Data Factory bağlanmasına engel olur. Güvenlik duvarınızın ve ara sunucunuzun düzgün yapılandırıldığından emin olmak için önceki bölüme bakın.

* Şirket içinde barındırılan tümleştirme çalışma zamanını kaydettirmeye çalıştığınızda şu hata iletisini alırsınız: "Bu Integration Runtime düğümü kaydedilemedi! Kimlik doğrulama anahtarının geçerli olduğunu ve tümleştirme hizmeti ana bilgisayar hizmetinin bu makinede çalıştığını doğrulayın. "
* Integration Runtime Configuration Manager açtığınızda, **bağlantısı kesilen** veya **bağlanan**bir durum görürsünüz. Windows olay günlüklerini görüntülediğinizde,**uygulama ve hizmet günlükleri** > **Microsoft Integration Runtime** **Olay Görüntüleyicisi** > altında, şöyle bir hata mesajı görürsünüz:

    ```
    Unable to connect to the remote server
    A component of Integration Runtime has become unresponsive and restarts automatically. Component name: Integration Runtime (Self-hosted).
    ```

### <a name="enable-remote-access-from-an-intranet"></a>İntranetten uzaktan erişimi etkinleştir

Şirket içinde barındırılan tümleştirme çalışma zamanını yüklediğiniz yerden başka bir ağa bağlı makineden kimlik bilgilerini şifrelemek için PowerShell kullanıyorsanız, **Intranet seçeneğinden uzaktan erişimi** etkinleştirebilirsiniz. Şirket içinde barındırılan tümleştirme çalışma zamanını yüklediğiniz makinede kimlik bilgilerini şifrelemek için PowerShell 'i çalıştırırsanız, **Intranetten uzaktan erişimi**etkinleştiremezsiniz.

Yüksek kullanılabilirlik ve ölçeklenebilirlik için başka bir düğüm eklemeden önce **Intranetten uzaktan erişimi** etkinleştirin.  

Şirket içinde barındırılan tümleştirme çalışma zamanı kurulumu 3,3 veya üstünü çalıştırdığınızda, varsayılan olarak şirket içinde barındırılan tümleştirme çalışma zamanı yükleyicisi, şirket içinde barındırılan tümleştirme çalışma zamanı makinesindeki **Intranetten uzaktan erişimi** devre dışı bırakır.

Bir iş ortağından veya diğer bilgisayarlardan bir güvenlik duvarı kullandığınızda, 8060 numaralı bağlantı noktasını veya Kullanıcı tarafından yapılandırılmış bağlantı noktasını el ile açabilirsiniz. Şirket içinde barındırılan tümleştirme çalışma zamanını ayarlarken bir güvenlik duvarı sorununuz varsa, güvenlik duvarını yapılandırmadan şirket içinde barındırılan tümleştirme çalışma zamanını yüklemek için aşağıdaki komutu kullanın:

```
msiexec /q /i IntegrationRuntime.msi NOFIREWALL=1
```

Şirket içinde barındırılan tümleştirme çalışma zamanı makinesinde 8060 numaralı bağlantı noktasını açmayı seçerseniz, veri deposu kimlik bilgilerini yapılandırmak için kimlik bilgileri uygulaması ayarı dışındaki mekanizmaları kullanın. Örneğin, **New-AzDataFactoryV2LinkedServiceEncryptCredential** PowerShell cmdlet 'ini kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Adım adım yönergeler için bkz. [öğretici: şirket içi verileri buluta kopyalama](tutorial-hybrid-copy-powershell.md).
