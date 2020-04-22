---
title: Azure Otomasyonu'nda Python runbook oluşturma
description: Basit bir Python runbook'u oluşturma, test etme ve yayımlama yı gösteren öğretici.
services: automation
ms.subservice: process-automation
ms.date: 04/19/2020
ms.topic: tutorial
ms.openlocfilehash: 116f2544b23ed5f9bc0fabbb945cb5cb2b51af96
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81726287"
---
# <a name="tutorial-create-a-python-runbook"></a>Öğretici: Python runbook oluşturma

Bu öğretici, Azure Otomasyonu'nda bir [Python runbook](../automation-runbook-types.md#python-runbooks) oluşturma ile size yol sunar. Python runbooks Python 2 altında derler. Azure portalındaki metin düzenleyicisini kullanarak runbook kodunu doğrudan ayarlayabilirsiniz.

> [!div class="checklist"]
> * Basit bir Python runbook oluşturma
> * Çalışma kitabını test edin ve yayımlayın
> * Runbook işinin durumunu çalıştırma ve izleme
> * Runbook parametreleri ile bir Azure sanal makinesi başlatmak için runbook'u güncelleştirme

> [!NOTE]
> Python runbook başlatmak için bir webhook kullanma desteklenmez.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

- Azure aboneliği. Henüz bir hesabınız yoksa, [MSDN abone avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) veya ücretsiz bir [hesaba](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)kaydolabilirsiniz.
- Runbook’u tutacak ve Azure kaynaklarında kimlik doğrulamasını yapacak bir [Automation hesabı](../automation-offering-get-started.md). Bu hesabın sanal makineyi başlatma ve durdurma izni olmalıdır.
- Azure sanal makinesi. Bu makineyi durdurup başlatacağınız için makinenin üretime yönelik bir VM olmaması gerekir.

## <a name="create-a-new-runbook"></a>Yeni bir runbook oluşturma

*Merhaba Dünya*metnini oluşturan basit bir runbook oluşturarak başlarsınız.

1. Azure portalında, Otomasyon hesabınızı açın.

    Automation hesabı sayfası size bu hesaptaki kaynakların hızlı bir görünümünü sağlar. Birkaç varlığınız zaten olmalıdır. Bu varlıkların çoğu, yeni bir Otomasyon hesabına otomatik olarak dahil edilen modüllerdir. Burada ayrıca [önkoşullarda](#prerequisites) belirtilen Kimlik Bilgileri varlığınız da bulunmalıdır.

2. Runbook listesini açmak için **Proses Otomasyonu** altındaki **Runbook'ları** seçin.

3. Yeni bir runbook oluşturmak için **runbook ekle'yi** seçin.

4. Runbook adını **MyFirstRunbook-Python**verin.

5. **Runbook türü**için Python **2'yi** seçin.

6. Runbook’u oluşturmak için **Oluştur**’a tıklayın ve metin düzenleyicisini açın.

## <a name="add-code-to-the-runbook"></a>Runbook'a kod ekleme

Şimdi metni `Hello World`yazdırmak için basit bir komut ekleyin.

```python
print("Hello World!")
```

Runbook'u kaydetmek için **Kaydet'i** tıklatın.

## <a name="test-the-runbook"></a>Runbook'u test etme

Runbook’u üretimde kullanılabilir hale getirmek üzere yayımlamadan önce düzgün çalıştığından emin olmak için test etmeniz gerekir. Bir runbook'u test ettiğinizde, bunun Taslak sürümünü çalıştırır ve çıktısını etkileşimli olarak görüntülersiniz.

1. Test bölmesini açmak için **Test bölmesi**’ne tıklayın.

2. Testi başlatmak için **Başlat**’a tıklayın. Etkinleştirilen tek seçenek bu olmalıdır.

3. Bir [runbook işi](../automation-runbook-execution.md) oluşturulur ve durumu görüntülenir.
   İş durumu, buluttaki bir runbook çalışanının kullanılabilir gelmesini beklediğini belirten Sıraya olarak başlar. Bir işçi işi talep ettiğinde başlangıç ve runbook gerçekten çalışmaya başladığında Çalıştırma'ya geçer.

4. Runbook işi tamamlandığında çıktısı görüntülenir. Bu durumda, görmelisiniz. `Hello World`

5. Tuvale geri dönmek için Test bölmesini kapatın.

## <a name="publish-and-start-the-runbook"></a>Runbook'u yayımla ve başlat

Oluşturduğunuz runbook hala Taslak modundadır. Üretimde çalıştırabilmeniz için yayımlamanız gerekir.
Bir runbook yayımladığınızda, taslak sürümüyle birlikte varolan yayımlanmış sürümün üzerine yazarsınız.
Bu durumda, runbook'u oluşturduğunuz için henüz yayınlanmış bir sürümünüz yok.

1. Runbook’u yayımlamak için **Yayımla**’ya tıklayın ve sorulduğunda **Evet**’e tıklayın.

2. **Runbook** sayfasında runbook'u görüntülemek için sola kaydırırsanız, **Yayımlanmış** **Yazarlık Durumunu** görmeniz gerekir.

3. **MyFirstRunbook-Python**bölmesini görüntülemek için sağa geri kaydırın.

   Üstteki seçenekler, runbook'u başlatmanızı, runbook'u görüntülemenizi veya gelecekte bir zamanda başlamak üzere zamanlamanızı sağlar.

4. **Başlat'ı** tıklatın ve Başlat Runbook bıçağı açıldığında **Tamam'ı** tıklatın.

5. Oluşturduğunuz runbook işi için Bir İş bölmesi açılır. Bu bölmeyi kapatabilirsiniz, ama açık bırakalım ki işin ilerlemesini izleyebilesin.

6. İş durumu İş **Özeti'nde** gösterilir ve runbook'u test ettiğinizde gördüğünüz durumla eşleşir.

7. Runbook durumu Tamamlandı'yı gösterdiğinde **Çıktı'yı**tıklatın. Çıktı bölmesi, görebileceğiniz `Hello World`bir şekilde açılır.

8. Çıktı bölmesini kapatın.

9. Runbook işine ait Akışlar bölmesini açmak için **Tüm Günlükler**’e tıklayın. Yalnızca Çıktı `Hello World` akışında görmeniz gerekir. Ancak, runbook onlara yazıyorsa, bu bölme, Verbose ve Hata gibi bir runbook iş için diğer akışları gösterebilir.

10. MyFirstRunbook-Python bölmesine dönmek için Akışlar bölmesini ve İş bölmesini kapatın.

11. Bu runbook için İşler sayfasını açmak için **İşler'i** tıklatın. Bu sayfa, bu runbook tarafından oluşturulan tüm işleri listeler. İşi yalnızca bir kez çalıştırdığınız için sadece bir işin listelendiğini görmeniz gerekir.

12. Runbook’u başlattığınızda, görüntülediğiniz iş bölmesini açmak için bu işe tıklayabilirsiniz. Bu bölme, zamanda geriye gitmenizi ve belirli bir runbook için oluşturulan herhangi bir işin ayrıntılarını görüntülemenizi sağlar.

## <a name="add-authentication-to-manage-azure-resources"></a>Azure kaynaklarını yönetmek için kimlik doğrulama ekleme

Runbook uygulamanızı test ettiniz ve yayımladınız, ancak şu ana kadar faydalı bir şey yapmadı. Bu runbook’un Azure kaynaklarını yönetmesini istiyorsunuz.
Bunu yapmak için komut dosyasının Otomasyon hesabınızdaki kimlik bilgilerini kullanarak kimlik doğrulaması yapması gerekir. Size yardımcı olmak için, Azure kaynaklarının kimliğini doğrulamayı ve bunlarla etkileşimde bulunmasını kolaylaştırmak için [Azure Otomasyon uyruarı paketini](https://github.com/azureautomation/azure_automation_utility) kullanabilirsiniz.

> [!NOTE]
> Bir Run As sertifikası olması için Otomasyon hesabının hizmet temel özelliğiyle oluşturulmuş olması gerekir.
> Otomasyon hesabınız hizmet ilkesiyle oluşturulmamadıysa, [Python için Azure Yönetim Kitaplıkları ile Authenticate'da](/azure/python/python-sdk-azure-authenticate)açıklandığı gibi kimlik doğrulaması yapabilirsiniz.

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

VM'yi başlatmak için işlem istemcisini kullanın. Runbook'a aşağıdaki kodu ekleyin:

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

VM içeren kaynak grubunun adı nerededir `MyResourceGroup` ve `TestVM` başlatmak istediğiniz VM'nin adıdır.

VM'yi başlattığını görmek için çalışma kitabını test edin ve çalıştırın.

## <a name="use-input-parameters"></a>Giriş parametrelerini kullanma

Runbook şu anda kaynak grubu ve VM adları için sabit kodlanmış değerler kullanır. Şimdi giriş parametrelerinden bu değerleri alan kod ekleyelim.

Parametre `sys.argv` değerlerini almak için değişkeni kullanırsınız. Diğer `import` ifadelerden hemen sonra runbook'a aşağıdaki kodu ekleyin:

```python
import sys

resource_group_name = str(sys.argv[1])
vm_name = str(sys.argv[2])
```

`sys` Bu, modülü içeri alır ve Kaynak Grubu ve VM adlarını tutmak için iki değişken oluşturur. Bağımsız değişken listesinin öğesi, `sys.argv[0]`komut dosyasının adı dır ve kullanıcı tarafından giriş değildir dikkat edin.

Artık runbook'un son iki satırını, sabit kodlu değerleri kullanmak yerine giriş parametre değerlerini kullanmak üzere değiştirebilirsiniz:

```python
async_vm_start = compute_client.virtual_machines.start(
    resource_group_name, vm_name)
async_vm_start.wait()
```

Python runbook'u başlattığınızda (Test bölmesinde veya yayımlanmış bir çalışma kitabı olarak), **Parametreler**altında Başlat Runbook sayfasındaki parametrelerin değerlerini girebilirsiniz.

İlk kutuya bir değer girmeye başladıktan sonra, gerektiğinde istediğiniz kadar parametre değeri girebilmeniz için ikinci bir değer görüntülenir.

Değerler, az önce eklediğiniz `sys.argv` koddaki gibi dizideki komut dosyası için kullanılabilir.

Kaynak grubunuzun adını ilk parametrenin değeri olarak, VM'nin adını da ikinci parametrenin değeri olarak başlatın.

![Parametre değerlerini girin](../media/automation-tutorial-runbook-textual-python/runbook-python-params.png)

Runbook'u başlatmak için **Tamam'ı** tıklatın. Runbook çalışır ve belirttiğiniz VM'yi başlatır.

## <a name="error-handling-in-python"></a>Python'da Hata İşleme

Python runbook'larınızdan WARNING, ERROR ve DEBUG akışları gibi çeşitli akışları almak için aşağıdaki kuralları da kullanabilirsiniz.

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
> Azure Otomasyonu `sys.stderr`desteklemez.

## <a name="next-steps"></a>Sonraki adımlar

- PowerShell runbook'lara başlamak için [powershell runbook oluştur'a](automation-tutorial-runbook-textual-powershell.md)bakın.
- Grafik çalışma kitaplarıyla başlamak için [bkz.](automation-tutorial-runbook-graphical.md)
- PowerShell iş akışı runbook'larına başlamak için [powershell iş akışı runbook'u oluştur'a](automation-tutorial-runbook-textual.md)bakın.
- Runbook türleri, avantajları ve sınırlamaları hakkında daha fazla bilgi için [Azure Otomasyon runbook türlerine](../automation-runbook-types.md)bakın.
- Python ile Azure için geliştirme hakkında bilgi edinmek [için Python geliştiricileri için Azure'a](/azure/python/)bakın.
- Örnek Python 2 runbook'larını görüntülemek için [Azure Automation GitHub'a](https://github.com/azureautomation/runbooks/tree/master/Utility/Python)bakın.
