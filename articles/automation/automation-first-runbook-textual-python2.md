---
title: Azure Otomasyonundaki ilk Python çalışma kitabım
description: Basit bir Python runbook oluşturma, test ve yayımlama size yol gösteren öğretici.
services: automation
ms.subservice: process-automation
ms.date: 03/19/2019
ms.topic: conceptual
ms.openlocfilehash: b17a0403a3b2a3ff8c3586ed26a4b833db54922d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75365946"
---
# <a name="my-first-python-runbook"></a>İlk Python runbook’um

> [!div class="op_single_selector"]
> - [Grafik](automation-first-runbook-graphical.md)
> - [Powershell](automation-first-runbook-textual-powershell.md)
> - [PowerShell İş Akışı](automation-first-runbook-textual.md)
> - [Python](automation-first-runbook-textual-python2.md)

Bu öğretici, Azure Otomasyonu'nda bir [Python runbook](automation-runbook-types.md#python-runbooks) oluşturma ile size yol sunar. Test ettiğiniz ve yayımladığınız basit bir çalışma kitabıyla başlarsınız. Ardından, bir Azure sanal makinesini başlatmayı içeren bir örnekle, bu runbook’u gerçekten Azure kaynaklarını yönetmek üzere değiştirin. Son olarak, runbook parametreleri ekleyerek runbook daha sağlam hale.

> [!NOTE]
> Python runbook başlatmak için bir webhook kullanma desteklenmez.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

- Azure aboneliği. Henüz bir hesabınız yoksa, [MSDN abone avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) veya ücretsiz bir [hesaba](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)kaydolabilirsiniz.
- Runbook’u tutacak ve Azure kaynaklarında kimlik doğrulamasını yapacak bir [Automation hesabı](automation-offering-get-started.md). Bu hesabın sanal makineyi başlatma ve durdurma izni olmalıdır.
- Azure sanal makinesi. Bu makineyi durdurup başlatacağınız için makinenin üretime yönelik bir VM olmaması gerekir.

## <a name="create-a-new-runbook"></a>Yeni bir runbook oluşturma

*Merhaba Dünya*metnini oluşturan basit bir runbook oluşturarak başlarsınız.

1. Azure portalında, Otomasyon hesabınızı açın.

    Automation hesabı sayfası size bu hesaptaki kaynakların hızlı bir görünümünü sağlar. Birkaç varlığınız zaten olmalıdır. Bu varlıkların çoğu, yeni bir Otomasyon hesabına otomatik olarak dahil edilen modüllerdir. Burada ayrıca [önkoşullarda](#prerequisites) belirtilen Kimlik Bilgileri varlığınız da bulunmalıdır.<br>

1. Runbook’ların listesini açmak için **İŞLEM YÖNETİMİ** altında **Runbooklar**’ı seçin.
1. Yeni bir runbook oluşturmak için **runbook + ekle'yi** seçin.
1. Runbook adını *MyFirstRunbook-Python*verin.
1. Bu durumda, bir [Python runbook](automation-runbook-types.md#python-runbooks) oluşturacaksın, bu nedenle **Runbook türü**için Python **2'yi** seçin.
1. Runbook’u oluşturmak için **Oluştur**’a tıklayın ve metin düzenleyicisini açın.

## <a name="add-code-to-the-runbook"></a>Runbook'a kod ekleme

Şimdi metin "Hello World" yazdırmak için basit bir komut ekleyin:

```python
print("Hello World!")
```

Runbook'u kaydetmek için **Kaydet'i** tıklatın.

## <a name="test-the-runbook"></a>Runbook'u test etme

Runbook’u üretimde kullanılabilir hale getirmek üzere yayımlamadan önce düzgün çalıştığından emin olmak için test etmeniz gerekir. Bir runbook'u test ettiğinizde, bunun **Taslak** sürümünü çalıştırır ve çıktısını etkileşimli olarak görüntülersiniz.

1. Test bölmesini açmak için **Test bölmesi**’ne tıklayın.
1. Testi başlatmak için **Başlat**’a tıklayın. Etkinleştirilen tek seçenek bu olmalıdır.
1. Bir [runbook işi](automation-runbook-execution.md) oluşturulur ve durumu görüntülenir.
   İş durumu, bulutta bir runbook çalışanının kullanılabilir hale gelmesinin beklendiğini gösteren şekilde *Sırada* olarak başlar. Bir işçi işi talep ettiğinde *başlangıç* ve runbook gerçekten çalışmaya başladığında *Çalıştırma'ya* geçer.
1. Runbook işi tamamlandığında çıktısı görüntülenir. Bu durumda, Merhaba *Dünya*görmelisiniz.
1. Tuvale geri dönmek için Test bölmesini kapatın.

## <a name="publish-and-start-the-runbook"></a>Runbook'u yayımla ve başlat

Oluşturduğunuz runbook hala taslak modunda. Üretimde çalıştırabilmeniz için yayımlamanız gerekir.
Bir runbook yayımladığınızda, taslak sürümüyle birlikte varolan yayımlanmış sürümün üzerine yazarsınız.
Bu durumda, runbook'u oluşturduğunuz için henüz yayınlanmış bir sürümünüz yok.

1. Runbook’u yayımlamak için **Yayımla**’ya tıklayın ve sorulduğunda **Evet**’e tıklayın.
1. **Runbook** bölmesinde runbook'u görüntülemek için sola kaydırırsanız, **Yayımlanmış** **Yazarlık Durumunu** gösterir.
1. **MyFirstRunbook-Python**bölmesini görüntülemek için sağa geri kaydırın.
   Üstteki seçenekler, runbook'u başlatmamıza, runbook'u görüntülememize veya gelecekte bir zamanda başlayacak şekilde zamanlamamıza olanak sağlar.
2. Runbook'u başlatmak istiyorsanız, **Başlat'ı** tıklatın ve Başlat Runbook bıçağı açıldığında **Tamam'ı** tıklatın.
3. Oluşturduğunuz runbook işi için bir iş bölmesi açılır. Bu bölmeyi kapatabilirsiniz, ancak bu durumda açık bırakın, böylece işin ilerlemesini izleyebilirsiniz.
1. İş durumu İş **Özeti'nde** gösterilir ve runbook'u test ettiğinizde gördüğünüz durumla eşleşir.
2. Runbook durumu olarak *Tamamlandı* gösterilince **Çıktı**’ya tıklayın. Çıktı bölmesi açıldı ve *Merhaba Dünya'nızı*görebilirsiniz.
3. Çıktı bölmesini kapatın.
4. Runbook işine ait Akışlar bölmesini açmak için **Tüm Günlükler**’e tıklayın. Çıktı akışında yalnızca *Merhaba Dünya* metnini görmelisiniz, ancak bu bölmede, runbook bunlara yazıyorsa Ayrıntılı ve Hata gibi runbook işine yönelik diğer akışlar da gösterilebilir.
5. MyFirstRunbook-Python bölmesine dönmek için Akışlar bölmesini ve İş bölmesini kapatın.
6. Bu runbook’a ait İşler bölmesini açmak için **İşler**’e tıklayın. Bu bölmede, bu runbook tarafından oluşturulan tüm işler listelenir. İşi yalnızca bir kez çalıştırdığınız için sadece bir işin listelendiğini görmeniz gerekir.
7. Runbook’u başlattığınızda, görüntülediğiniz iş bölmesini açmak için bu işe tıklayabilirsiniz. Böylece zaman içinde geri dönerek, belirli bir runbook için oluşturulan herhangi bir işin ayrıntılarını görüntüleyebilirsiniz.

## <a name="add-authentication-to-manage-azure-resources"></a>Azure kaynaklarını yönetmek için kimlik doğrulama ekleme

Runbook uygulamanızı test ettiniz ve yayımladınız, ancak şu ana kadar faydalı bir şey yapmadı. Bu runbook’un Azure kaynaklarını yönetmesini istiyorsunuz.
Azure kaynaklarını yönetmek için komut dosyasının Otomasyon Hesabınızdaki kimlik bilgilerini kullanarak kimlik doğrulaması gerekir. Size yardımcı olmak için, Azure kaynaklarının kimliğini doğrulamayı ve bunlarla etkileşimde bulunmasını kolaylaştırmak için [Azure Otomasyon uyruarı paketini](https://github.com/azureautomation/azure_automation_utility) kullanabilirsiniz.

> [!NOTE]
> Bir Run As Sertifikası olması için Otomasyon hesabının hizmet temel özelliğiyle oluşturulmuş olması gerekir.
> Otomasyon hesabınız hizmet ilkesiyle oluşturulmamadıysa, Python için Azure Yönetim [Kitaplıkları ile Authenticate'da](/azure/python/python-sdk-azure-authenticate)açıklanan yöntemi kullanarak kimlik doğrulaması yapabilirsiniz.

1. Metin düzenleyicisini MyFirstRunbook-Python bölmesine **tıklayarak** açın.

2. Azure'da kimlik doğrulaması yapmak için aşağıdaki kodu ekleyin:

   ```python
   import os
   from azure.mgmt.compute import ComputeManagementClient
   import azure.mgmt.resource
   import automationassets

   def get_automation_runas_credential(runas_connection):
       from OpenSSL import crypto
       import binascii
       from msrestazure import azure_active_directory
       import adal

       # Get the Azure Automation RunAs service principal certificate
       cert = automationassets.get_automation_certificate("AzureRunAsCertificate")
       pks12_cert = crypto.load_pkcs12(cert)
       pem_pkey = crypto.dump_privatekey(crypto.FILETYPE_PEM,pks12_cert.get_privatekey())

       # Get run as connection information for the Azure Automation service principal
       application_id = runas_connection["ApplicationId"]
       thumbprint = runas_connection["CertificateThumbprint"]
       tenant_id = runas_connection["TenantId"]

       # Authenticate with service principal certificate
       resource ="https://management.core.windows.net/"
       authority_url = ("https://login.microsoftonline.com/"+tenant_id)
       context = adal.AuthenticationContext(authority_url)
       return azure_active_directory.AdalAuthentication(
       lambda: context.acquire_token_with_client_certificate(
               resource,
               application_id,
               pem_pkey,
               thumbprint)
       )

   # Authenticate to Azure using the Azure Automation RunAs service principal
   runas_connection = automationassets.get_automation_connection("AzureRunAsConnection")
   azure_credential = get_automation_runas_credential(runas_connection)
   ```

## <a name="add-code-to-create-python-compute-client-and-start-the-vm"></a>Python Compute istemcisi oluşturmak ve VM'yi başlatmak için kod ekleyin

Azure VM'lerle çalışmak [için Python için Azure İşlem istemcisinin](https://docs.microsoft.com/python/api/azure-mgmt-compute/azure.mgmt.compute.computemanagementclient)bir örneğini oluşturun.

VM'yi başlatmak için İşlem istemcisini kullanın. Runbook'a aşağıdaki kodu ekleyin:

```python
# Initialize the compute management client with the RunAs credential and specify the subscription to work against.
compute_client = ComputeManagementClient(
    azure_credential,
    str(runas_connection["SubscriptionId"])
)


print('\nStart VM')
async_vm_start = compute_client.virtual_machines.start(
    "MyResourceGroup", "TestVM")
async_vm_start.wait()
```

_MyResourceGroup_ VM içeren kaynak grubunun adı dır ve _TestVM_ başlatmak istediğiniz VM'nin adıdır.

VM'yi başlattığını görmek için çalışma kitabını test edin ve çalıştırın.

## <a name="use-input-parameters"></a>Giriş parametrelerini kullanma

Runbook şu anda Kaynak Grubu ve VM adları için sabit kodlanmış değerler kullanır.
Şimdi giriş parametrelerinden bu değerleri alan kod ekleyelim.

Parametre `sys.argv` değerlerini almak için değişkeni kullanırsınız.
Diğer `import` ifadelerden hemen sonra runbook'a aşağıdaki kodu ekleyin:

```python
import sys

resource_group_name = str(sys.argv[1])
vm_name = str(sys.argv[2])
```

`sys` Bu, modülü içeri alır ve Kaynak Grubu ve VM adlarını tutmak için iki değişken oluşturur.
Bağımsız değişken listesinin öğesi, `sys.argv[0]`komut dosyasının adı dır ve kullanıcı tarafından giriş değildir dikkat edin.

Artık runbook'un son iki satırını, sabit kodlu değerleri kullanmak yerine giriş parametre değerlerini kullanmak üzere değiştirebilirsiniz:

```python
async_vm_start = compute_client.virtual_machines.start(
    resource_group_name, vm_name)
async_vm_start.wait()
```

Bir Python runbook başlattığınızda **(Test** sayfasında veya yayımlanmış bir runbook olarak), **Parametreler**altında **Başlat Runbook** sayfasında parametreler için değerleri girebilirsiniz.

İlk kutuya bir değer girmeye başladıktan sonra, gerektiği kadar parametre değeri girebilmeniz için ikinci bir değer görüntülenir.

Değerler, az önce eklediğiniz `sys.argv` koddaki dizi olarak komut dosyası için kullanılabilir.

Kaynak grubunuzun adını ilk parametrenin değeri olarak, VM'nin adını da ikinci parametrenin değeri olarak başlatın.

![Parametre değerlerini girin](media/automation-first-runbook-textual-python/runbook-python-params.png)

Runbook'u başlatmak için **Tamam'ı** tıklatın. Runbook çalışır ve belirttiğiniz VM'yi başlatır.

## <a name="error-handling-in-python"></a>Python'da Hata İşleme

Python runbook'larınızdan **WARNING**, **ERROR**ve **DEBUG** akışları gibi çeşitli akışları almak için aşağıdaki kuralları da kullanabilirsiniz.

```python
print("Hello World output") 
print("ERROR: - Hello world error")
print("WARNING: - Hello world warning")
print("DEBUG: - Hello world debug")
print("VERBOSE: - Hello world verbose")
```

Aşağıdaki örnek, bir `try...except` blokta kullanılan bu kuralı gösterir.

```python
try:
    raise Exception('one', 'two')
except Exception as detail:
    print 'ERROR: Handling run-time error:', detail
```

> [!NOTE]
> **sys.stderr** Azure Otomasyonu'nda desteklenmez.

## <a name="next-steps"></a>Sonraki adımlar

- PowerShell runbook'ları kullanmaya başlamak için bkz. [İlk PowerShell runbook uygulamam](automation-first-runbook-textual-powershell.md)
- Grafik runbook'ları kullanmaya başlamak için bkz. [İlk grafik runbook uygulamam](automation-first-runbook-graphical.md)
- PowerShell iş akışı runbook'larına başlamak için [ilk PowerShell iş akışı çalışma kitabıma](automation-first-runbook-textual.md) bakın
- Runbook türleri, avantajları ve sınırlamaları hakkında daha fazla bilgi için bkz. [Azure Automation runbook türleri](automation-runbook-types.md)
- Python ile Azure için geliştirme hakkında bilgi edinmek [için Python geliştiricileri için Azure'a](/azure/python/) bakın
- Örnek Python 2 runbook'larını görüntülemek için [Azure Automation GitHub'a](https://github.com/azureautomation/runbooks/tree/master/Utility/Python) bakın
