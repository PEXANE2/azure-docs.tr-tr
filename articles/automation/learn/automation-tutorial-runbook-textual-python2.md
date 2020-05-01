---
title: Azure Otomasyonu 'nda Python runbook 'u oluşturma
description: Basit bir Python runbook 'u oluşturma, test etme ve yayımlamayı gösteren öğretici.
services: automation
ms.subservice: process-automation
ms.date: 04/19/2020
ms.topic: tutorial
ms.custom: has-adal-ref
ms.openlocfilehash: 2b20796fdcf71ccfb60c519d081b42fba982f0b6
ms.sourcegitcommit: 50ef5c2798da04cf746181fbfa3253fca366feaa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2020
ms.locfileid: "82608703"
---
# <a name="tutorial-create-a-python-runbook"></a>Öğretici: Python runbook 'u oluşturma

Bu öğreticide, Azure Otomasyonu 'nda bir [Python runbook](../automation-runbook-types.md#python-runbooks) oluşturma işlemi adım adım açıklanmaktadır. Python runbook 'ları Python 2 altında derlenir. Azure portal metin düzenleyicisini kullanarak runbook 'un kodunu doğrudan düzenleyebilirsiniz.

> [!div class="checklist"]
> * Basit bir Python runbook 'u oluşturma
> * Runbook 'u test etme ve yayımlama
> * Runbook işinin durumunu çalıştırma ve izleme
> * Runbook parametreleri ile bir Azure sanal makinesini başlatmak için Runbook 'u güncelleştirme

> [!NOTE]
> Python runbook 'u başlatmak için Web kancası kullanılması desteklenmez.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

- Azure aboneliği. Henüz bir hesabınız yoksa [MSDN abone avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) veya [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)için kaydolabilirsiniz.
- Runbook’u tutacak ve Azure kaynaklarında kimlik doğrulamasını yapacak bir [Automation hesabı](../automation-offering-get-started.md). Bu hesabın sanal makineyi başlatma ve durdurma izni olmalıdır.
- Azure sanal makinesi. Bu makineyi durdurup başlatacağınız için makinenin üretime yönelik bir VM olmaması gerekir.

## <a name="create-a-new-runbook"></a>Yeni runbook oluştur

*Merhaba Dünya*metin çıkışı veren basit bir runbook oluşturarak başlayın.

1. Azure portalında, Otomasyon hesabınızı açın.

    Automation hesabı sayfası size bu hesaptaki kaynakların hızlı bir görünümünü sağlar. Birkaç varlığınız zaten olmalıdır. Bu varlıkların çoğu, yeni bir Otomasyon hesabına otomatik olarak dahil edilen modüllerdir. Burada ayrıca [önkoşullarda](#prerequisites) belirtilen Kimlik Bilgileri varlığınız da bulunmalıdır.

2. Runbook 'ların listesini açmak için **Işlem Otomasyonu** altında **runbook 'lar** ' ı seçin.

3. Yeni bir runbook oluşturmak için **runbook Ekle** ' yi seçin.

4. Runbook **'A Myfirstrunbook-Python**adını verin.

5. **Runbook türü**için **Python 2** ' yi seçin.

6. Runbook’u oluşturmak için **Oluştur**’a tıklayın ve metin düzenleyicisini açın.

## <a name="add-code-to-the-runbook"></a>Runbook 'a kod ekleme

Artık metni `Hello World`yazdırmak için basit bir komut eklersiniz.

```python
print("Hello World!")
```

Runbook 'u kaydetmek için **Kaydet** ' e tıklayın.

## <a name="test-the-runbook"></a>Runbook'u test etme

Runbook’u üretimde kullanılabilir hale getirmek üzere yayımlamadan önce düzgün çalıştığından emin olmak için test etmeniz gerekir. Bir runbook'u test ettiğinizde, bunun Taslak sürümünü çalıştırır ve çıktısını etkileşimli olarak görüntülersiniz.

1. Test bölmesini açmak için **Test bölmesi**’ne tıklayın.

2. Testi başlatmak için **Başlat**’a tıklayın. Etkinleştirilen tek seçenek bu olmalıdır.

3. Bir [runbook işi](../automation-runbook-execution.md) oluşturulur ve durumu görüntülenir.
   İş durumu, bulutta bir runbook worker 'ın kullanılabilir hale gelmesini beklediğini belirten sıraya alındı olarak başlar. Bir çalışan işi talep ettiği zaman başlayacak ve ardından runbook gerçekten çalışmaya başladığında çalışır.

4. Runbook işi tamamlandığında çıktısı görüntülenir. Bu durumda, görmeniz `Hello World`gerekir.

5. Tuvale geri dönmek için Test bölmesini kapatın.

## <a name="publish-and-start-the-runbook"></a>Runbook 'u yayımlama ve başlatma

Oluşturduğunuz runbook hala Taslak modundadır. Bunu üretimde çalıştırmadan önce yayımlamanız gerekir.
Bir runbook yayımladığınızda, var olan yayımlanmış sürümün üzerine taslak sürümünü yazın.
Bu durumda, henüz runbook 'u oluşturduğunuz için yayınlanmış bir sürümdür.

1. Runbook’u yayımlamak için **Yayımla**’ya tıklayın ve sorulduğunda **Evet**’e tıklayın.

2. Runbook 'u runbook **'ları** sayfasında görüntülemek için sola kaydırırsanız **yayımlanmış**bir **yazma durumu** görmeniz gerekir.

3. **Myfirstrunbook-Python**bölmesini görüntülemek için sağa kaydırın.

   Üstteki seçenekler, runbook 'u başlatabilir, runbook 'u görüntüleyebilir veya gelecekte bir süre sonra başlayacak şekilde zamanlayabilirsiniz.

4. **Başlat** ' a tıklayın ve Runbook 'u Başlat dikey penceresi açıldığında **Tamam** ' a tıklayın.

5. Oluşturduğunuz runbook işi için bir Iş bölmesi açılır. Bu bölmeyi kapatabilir, ancak işin ilerlemesini izleyebilmeniz için açık bırakalım.

6. İş durumu **Iş özetinde** gösterilir ve Runbook 'u test ettiğinizde gördüğünüz durumları eşleştirir.

7. Runbook durumu tamamlandı olarak gösterildikten sonra **Çıkış**' a tıklayın. Burada görebileceğiniz `Hello World`çıkış bölmesi açılır.

8. Çıktı bölmesini kapatın.

9. Runbook işine ait Akışlar bölmesini açmak için **Tüm Günlükler**’e tıklayın. Yalnızca çıkış akışında görmeniz `Hello World` gerekir. Ancak bu bölme, runbook 'a yazıyorsa ayrıntılı ve hata gibi bir runbook işi için diğer akışları gösterebilir.

10. MyFirstRunbook-Python bölmesine dönmek için akışlar bölmesini ve Iş bölmesini kapatın.

11. Bu runbook 'un Işler sayfasını açmak için **işler** ' e tıklayın. Bu sayfada bu runbook tarafından oluşturulan tüm işler listelenir. İşi yalnızca bir kez çalıştırdığınız için sadece bir işin listelendiğini görmeniz gerekir.

12. Runbook’u başlattığınızda, görüntülediğiniz iş bölmesini açmak için bu işe tıklayabilirsiniz. Bu bölme, zaman içinde geri dönmenizi ve belirli bir runbook için oluşturulan herhangi bir işin ayrıntılarını görüntülemenizi sağlar.

## <a name="add-authentication-to-manage-azure-resources"></a>Azure kaynaklarını yönetmek için kimlik doğrulaması ekleme

Runbook uygulamanızı test ettiniz ve yayımladınız, ancak şu ana kadar faydalı bir şey yapmadı. Bu runbook’un Azure kaynaklarını yönetmesini istiyorsunuz.
Bunu yapmak için, komut dosyasının Otomasyon hesabınızdaki kimlik bilgilerini kullanarak kimlik doğrulaması yapması gerekir. Size yardımcı olmak için [Azure Otomasyonu yardımcı programını](https://github.com/azureautomation/azure_automation_utility) kullanarak Azure kaynaklarıyla kimlik doğrulaması ve etkileşim kurmayı kolaylaştırın.

> [!NOTE]
> Otomasyon hesabı, bir farklı çalıştır sertifikası olmak için hizmet sorumlusu özelliği ile oluşturulmuş olmalıdır.
> Otomasyon hesabınız hizmet sorumlusu ile oluşturulmadıysa, [Python Için Azure Yönetim kitaplıklarıyla kimlik doğrulama](/azure/python/python-sdk-azure-authenticate)konusunda açıklandığı gibi kimlik doğrulaması yapabilirsiniz.

1. MyFirstRunbook-Python bölmesinde **Düzenle** ' ye tıklayarak metinsel düzenleyiciyi açın.

2. Azure 'da kimlik doğrulaması yapmak için aşağıdaki kodu ekleyin:

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

## <a name="add-code-to-create-python-compute-client-and-start-the-vm"></a>Python Işlem istemcisi oluşturmak ve VM 'yi başlatmak için kod ekleme

Azure VM 'leriyle çalışmak için [Python Için Azure işlem istemcisinin](https://docs.microsoft.com/python/api/azure-mgmt-compute/azure.mgmt.compute.computemanagementclient)bir örneğini oluşturun.

VM 'yi başlatmak için işlem istemcisini kullanın. Aşağıdaki kodu runbook 'a ekleyin:

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

`MyResourceGroup` , Sanal makineyi içeren kaynak grubunun adıdır ve `TestVM` başlatmak istediğiniz VM 'nin adıdır.

VM 'nin başlatıldığını görmek için Runbook 'u yeniden test edin ve çalıştırın.

## <a name="use-input-parameters"></a>Giriş parametrelerini kullanma

Runbook Şu anda kaynak grubunun ve VM 'nin adları için sabit kodlanmış değerler kullanıyor. Şimdi Giriş parametrelerinden bu değerleri alan kodu ekleyelim.

Parametre değerlerini almak `sys.argv` için değişkenini kullanırsınız. Aşağıdaki kodu diğer `import` deyimlerden hemen sonra runbook 'a ekleyin:

```python
import sys

resource_group_name = str(sys.argv[1])
vm_name = str(sys.argv[2])
```

Bu, `sys` modülünü içeri aktarır ve kaynak grubunu ve VM adlarını tutmak için iki değişken oluşturur. Bağımsız değişken listesinin öğesi, `sys.argv[0]`, betiğin adı olduğunu ve Kullanıcı tarafından giriş olmadığına dikkat edin.

Artık runbook 'un son iki satırını, sabit kodlanmış değerler kullanmak yerine giriş parametresi değerlerini kullanacak şekilde değiştirebilirsiniz:

```python
async_vm_start = compute_client.virtual_machines.start(
    resource_group_name, vm_name)
async_vm_start.wait()
```

Bir Python runbook 'u başlattığınızda (test bölmesinde veya yayımlanan runbook olarak **) parametreler ' in altındaki runbook**'u Başlat sayfasında parametrelerin değerlerini girebilirsiniz.

İlk kutuya bir değer girmeyi başlattıktan sonra, ikinci olarak bir belirir ve bu şekilde gerektiği kadar parametre değeri girebilirsiniz.

Değerler `sys.argv` dizideki komut dosyasında, yeni eklediğiniz kodda olduğu gibi kullanılabilir.

İlk parametrenin değeri olarak kaynak grubunuzun adını ve ikinci parametrenin değeri olarak başlatılacak VM 'nin adını girin.

![Parametre değerlerini girin](../media/automation-tutorial-runbook-textual-python/runbook-python-params.png)

Runbook 'u başlatmak için **Tamam** ' ı tıklatın. Runbook çalışır ve belirttiğiniz VM 'yi başlatır.

## <a name="error-handling-in-python"></a>Python 'da hata Işleme

Ayrıca, Python runbook 'larınızdan uyarı, hata ve hata ayıklama akışları gibi çeşitli akışlar almak için aşağıdaki kuralları da kullanabilirsiniz.

```python
print("Hello World output")
print("ERROR: - Hello world error")
print("WARNING: - Hello world warning")
print("DEBUG: - Hello world debug")
print("VERBOSE: - Hello world verbose")
```

Aşağıdaki örnekte, bir `try...except` bloğunda kullanılan bu kural gösterilmektedir.

```python
try:
    raise Exception('one', 'two')
except Exception as detail:
    print 'ERROR: Handling run-time error:', detail
```

> [!NOTE]
> Azure Otomasyonu desteklemez `sys.stderr`.

## <a name="next-steps"></a>Sonraki adımlar

- PowerShell runbook 'larını kullanmaya başlamak için bkz. [PowerShell runbook 'U oluşturma](automation-tutorial-runbook-textual-powershell.md).
- Grafik runbook 'ları kullanmaya başlamak için bkz. [grafik runbook 'U oluşturma](automation-tutorial-runbook-graphical.md).
- PowerShell iş akışı runbook 'larını kullanmaya başlamak için bkz. [PowerShell iş akışı runbook 'U oluşturma](automation-tutorial-runbook-textual.md).
- Runbook türleri, avantajları ve sınırlamaları hakkında daha fazla bilgi için bkz. [Azure Otomasyonu runbook türleri](../automation-runbook-types.md).
- Python ile Azure için geliştirme hakkında bilgi edinmek için bkz. [Python geliştiricileri Için Azure](/azure/python/).
- Örnek Python 2 runbook 'larını görüntülemek için bkz. [Azure Otomasyonu GitHub](https://github.com/azureautomation/runbooks/tree/master/Utility/Python).
