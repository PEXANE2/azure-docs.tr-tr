---
title: Yönetim çözümlerinde Azure Otomasyon kaynakları | Microsoft Dokümanlar
description: Yönetim çözümleri genellikle, izleme verilerini toplama ve işleme gibi işlemleri otomatikleştirmek için Azure Automation'daki runbook'ları içerir.  Bu makalede, runbook'ların ve ilgili kaynaklarının bir çözüme nasıl dahil edilebildiğini açıklanmaktadır.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 05/24/2017
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 8ef9f27546e9db95d5a41769e1b5bc7bc0c2f851
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77663071"
---
# <a name="adding-azure-automation-resources-to-a-management-solution-preview"></a>Azure Otomasyon kaynaklarının yönetim çözümüne eklenmesi (Önizleme)
> [!NOTE]
> Bu, şu anda önizlemede olan yönetim çözümleri oluşturmak için ön belgelerdir. Aşağıda açıklanan herhangi bir şema değişebilir.   


[Yönetim çözümleri]( solutions.md) genellikle, izleme verilerini toplama ve işleme gibi işlemleri otomatikleştirmek için Azure Automation'daki runbook'ları içerir.  Çalışma kitaplarına ek olarak, Otomasyon hesapları, çözümde kullanılan runbook'ları destekleyen değişkenler ve zamanlamalar gibi varlıkları içerir.  Bu makalede, runbook'ların ve ilgili kaynaklarının bir çözüme nasıl dahil edilebildiğini açıklanmaktadır.

> [!NOTE]
> Bu makaledeki örnekler, yönetim çözümleri için gerekli veya ortak olan ve Tasarım'da açıklanan parametreleri ve değişkenleri kullanır [ve Azure'da bir yönetim çözümü oluşturur]( solutions-creating.md) 


## <a name="prerequisites"></a>Ön koşullar
Bu makalede, aşağıdaki bilgileri zaten bildiğinizi varsayar.

- Nasıl [bir yönetim çözümü oluşturmak]( solutions-creating.md)için .
- Çözüm [dosyasının]( solutions-solution-file.md)yapısı.
- Kaynak [Yöneticisi şablonları](../../azure-resource-manager/templates/template-syntax.md) nasıl yazar?

## <a name="automation-account"></a>Otomasyon hesabı
Azure Otomasyonu'ndaki tüm kaynaklar bir [Otomasyon hesabında](../../automation/automation-security-overview.md#automation-account-overview)bulunur.  [Log Analytics çalışma alanı ve Otomasyon hesabında]( solutions.md#log-analytics-workspace-and-automation-account) açıklandığı gibi, Otomasyon hesabı yönetim çözümüne dahil değildir, ancak çözüm yüklenmeden önce var olmalıdır.  Kullanılamıyorsa, çözüm yüklemesi başarısız olur.

Her Otomasyon kaynağının adı, Otomasyon hesabının adını içerir.  Bu, **runbook** kaynağının aşağıdaki örneğinde olduğu gibi hesap Adı parametresi ile çözümyapılır.

    "name": "[concat(parameters('accountName'), '/MyRunbook'))]"


## <a name="runbooks"></a>Runbook'lar
Çözüm yüklendiğinde oluşturulsunlar diye çözüm dosyasına çözüm tarafından kullanılan runbook'ları eklemeniz gerekir.  Ancak şablonda runbook'un gövdesini içeremezsiniz, bu nedenle çalışma kitabını çözümünüzü yükleyen herhangi bir kullanıcı tarafından erişilebilen ortak bir konuma yayımlamalısınız.

[Azure Otomasyon runbook](../../automation/automation-runbook-types.md) kaynakları bir **microsoft.automation/automationAccounts/runbooks** türüne sahiptir ve aşağıdaki yapıya sahiptir. Bu, bu kod parçacıklarını çözüm dosyanıza kopyalayıp yapıştırabilmeniz ve parametre adlarını değiştirebilmeniz için ortak değişkenleri ve parametreleri içerir. 

    {
        "name": "[concat(parameters('accountName'), '/', variables('Runbook').Name)]",
        "type": "Microsoft.Automation/automationAccounts/runbooks",
        "apiVersion": "[variables('AutomationApiVersion')]",
        "dependsOn": [
        ],
        "location": "[parameters('regionId')]",
        "tags": { },
        "properties": {
            "runbookType": "[variables('Runbook').Type]",
            "logProgress": "true",
            "logVerbose": "true",
            "description": "[variables('Runbook').Description]",
            "publishContentLink": {
                "uri": "[variables('Runbook').Uri]",
                "version": [variables('Runbook').Version]"
            }
        }
    }


Runbook'ların özellikleri aşağıdaki tabloda açıklanmıştır.

| Özellik | Açıklama |
|:--- |:--- |
| runbookType |Runbook türlerini belirtir. <br><br> Komut dosyası - PowerShell komut dosyası <br>PowerShell - PowerShell iş akışı <br> GraphPowerShell - Grafik PowerShell komut dosyası runbook <br> GraphPowerShellWorkflow - GrafikPowerShell iş akışı runbook |
| logİlerleme |Runbook için [ilerleme kayıtlarının](../../automation/automation-runbook-output-and-messages.md) oluşturulup oluşturulmayacağını belirtir. |
| logVerbose |Runbook için [ayrıntılı kayıtların](../../automation/automation-runbook-output-and-messages.md) oluşturulup oluşturulmayacağını belirtir. |
| açıklama |Runbook için isteğe bağlı açıklama. |
| publishContentLink |Runbook'un içeriğini belirtir. <br><br>uri - Uri runbook içeriğine.  Bu, PowerShell ve Script runbook'ları için bir .ps1 dosyası ve Grafik runbook'u için dışa aktarılan grafik çalışma kitabı dosyası olacaktır.  <br> sürüm - Kendi izleme için runbook Sürümü. |


## <a name="automation-jobs"></a>Otomasyon işleri
Azure Otomasyonu'nda bir runbook başlattığınızda, bir otomasyon işi oluşturur.  Yönetim çözümü yüklendiğinde bir runbook'u otomatik olarak başlatmak için çözümünüze bir otomasyon iş kaynağı ekleyebilirsiniz.  Bu yöntem genellikle çözümün ilk yapılandırması için kullanılan runbook'ları başlatmak için kullanılır.  Runbook'u düzenli aralıklarla başlatmak için bir [zamanlama](#schedules) ve [iş zamanlamasını](#job-schedules) oluşturun

İş kaynakları bir **microsoft.automation/automationAccounts/jobs** türüne sahiptir ve aşağıdaki yapıya sahiptir.  Bu, bu kod parçacıklarını çözüm dosyanıza kopyalayıp yapıştırabilmeniz ve parametre adlarını değiştirebilmeniz için ortak değişkenleri ve parametreleri içerir. 

    {
      "name": "[concat(parameters('accountName'), '/', parameters('Runbook').JobGuid)]",
      "type": "Microsoft.Automation/automationAccounts/jobs",
      "apiVersion": "[variables('AutomationApiVersion')]",
      "location": "[parameters('regionId')]",
      "dependsOn": [
        "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('Runbook').Name)]"
      ],
      "tags": { },
      "properties": {
        "runbook": {
          "name": "[variables('Runbook').Name]"
        },
        "parameters": {
          "Parameter1": "[[variables('Runbook').Parameter1]",
          "Parameter2": "[[variables('Runbook').Parameter2]"
        }
      }
    }

Otomasyon işleri için özellikler aşağıdaki tabloda açıklanmıştır.

| Özellik | Açıklama |
|:--- |:--- |
| runbook |Başletmek için runbook adını içeren tek ad varlığı. |
| parametreler |Runbook tarafından gerekli her parametre değeri için varlık. |

İş, runbook adını ve runbook'a gönderilecek parametre değerlerini içerir.  Çalışma, çalışma defterinin çalışmadan önce oluşturulması gerektiğinden, başlangıç defterine [bağlı]( solutions-solution-file.md#resources) olmalıdır.  Başlatılması gereken birden çok runbook'unuzun varsa, önce çalıştırılması gereken diğer işlere bağlı bir iş alarak siparişlerini tanımlayabilirsiniz.

Bir iş kaynağının adı, genellikle bir parametre tarafından atanan bir GUID içermelidir.  Azure'da bir yönetim [çözümü dosyası oluşturmada]( solutions-solution-file.md#parameters)GUID parametreleri hakkında daha fazla bilgi edinebilirsiniz.  


## <a name="certificates"></a>Sertifikalar
[Azure Otomasyon sertifikaları](../../automation/automation-certificates.md) bir **microsoft.automation/automationAccounts/certificatetürüne** sahiptir ve aşağıdaki yapıya sahiptir. Bu, bu kod parçacıklarını çözüm dosyanıza kopyalayıp yapıştırabilmeniz ve parametre adlarını değiştirebilmeniz için ortak değişkenleri ve parametreleri içerir. 

    {
      "name": "[concat(parameters('accountName'), '/', variables('Certificate').Name)]",
      "type": "Microsoft.Automation/automationAccounts/certificates",
      "apiVersion": "[variables('AutomationApiVersion')]",
      "location": "[parameters('regionId')]",
      "tags": { },
      "dependsOn": [
      ],
      "properties": {
        "base64Value": "[variables('Certificate').Base64Value]",
        "thumbprint": "[variables('Certificate').Thumbprint]"
      }
    }



Sertifika kaynaklarının özellikleri aşağıdaki tabloda açıklanmıştır.

| Özellik | Açıklama |
|:--- |:--- |
| base64Değer |Sertifika için temel 64 değeri. |
| Parmak izi |Sertifika için parmak izi. |



## <a name="credentials"></a>Kimlik Bilgileri
[Azure Otomasyon kimlik bilgileri](../../automation/automation-credentials.md) bir **microsoft.automation/automationAccounts/credentials** türüne sahiptir ve aşağıdaki yapıya sahiptir.  Bu, bu kod parçacıklarını çözüm dosyanıza kopyalayıp yapıştırabilmeniz ve parametre adlarını değiştirebilmeniz için ortak değişkenleri ve parametreleri içerir. 


    {
      "name": "[concat(parameters('accountName'), '/', variables('Credential').Name)]",
      "type": "Microsoft.Automation/automationAccounts/credentials",
      "apiVersion": "[variables('AutomationApiVersion')]",
      "location": "[parameters('regionId')]",
      "tags": { },
      "dependsOn": [
      ],
      "properties": {
        "userName": "[parameters('credentialUsername')]",
        "password": "[parameters('credentialPassword')]"
      }
    }

Kimlik bilgileri özellikleri aşağıdaki tabloda açıklanmıştır.

| Özellik | Açıklama |
|:--- |:--- |
| userName |Kimlik bilgisi için kullanıcı adı. |
| password |Kimlik bilgisi için parola. |


## <a name="schedules"></a>Zamanlamalar
[Azure Otomasyon zamanlamaları](../../automation/automation-schedules.md) bir **microsoft.automation/automationAccounts/schedules** türüne sahiptir ve aşağıdaki yapıya sahiptir. Bu, bu kod parçacıklarını çözüm dosyanıza kopyalayıp yapıştırabilmeniz ve parametre adlarını değiştirebilmeniz için ortak değişkenleri ve parametreleri içerir. 

    {
      "name": "[concat(parameters('accountName'), '/', variables('Schedule').Name)]",
      "type": "microsoft.automation/automationAccounts/schedules",
      "apiVersion": "[variables('AutomationApiVersion')]",
      "tags": { },
      "dependsOn": [
      ],
      "properties": {
        "description": "[variables('Schedule').Description]",
        "startTime": "[parameters('scheduleStartTime')]",
        "timeZone": "[parameters('scheduleTimeZone')]",
        "isEnabled": "[variables('Schedule').IsEnabled]",
        "interval": "[variables('Schedule').Interval]",
        "frequency": "[variables('Schedule').Frequency]"
      }
    }

Zamanlama kaynaklarının özellikleri aşağıdaki tabloda açıklanmıştır.

| Özellik | Açıklama |
|:--- |:--- |
| açıklama |Zamanlama için isteğe bağlı açıklama. |
| startTime |DateTime nesnesi olarak zamanlamanın başlangıç saatini belirtir. Geçerli bir DateTime dönüştürülebilir eğer bir dize sağlanabilir. |
| ısenabled |Zamanlamanın etkin olup olmadığını belirtir. |
| interval |Zamanlama için aralık türü.<br><br>gün<br>saat |
| frequency |Zamanlamanın gün veya saat sayısı olarak ateşlemesi gereken sıklık. |

Zamanlamaların geçerli saatten daha büyük bir değere sahip bir başlangıç zamanı olmalıdır.  Ne zaman yükleneceğini bilmenin bir yolu olmayacağından, bu değeri bir değişkenle sağlayamazsınız.

Bir çözümde zamanlama kaynaklarını kullanırken aşağıdaki iki stratejiden birini kullanın.

- Zamanlamanın başlangıç saati için bir parametre kullanın.  Bu, kullanıcıdan çözümü yüklediğinde bir değer sağlamasını ister.  Birden çok zamanlamanız varsa, birden fazla zamanlama için tek bir parametre değeri kullanabilirsiniz.
- Çözüm yüklendiğinde başlayan bir runbook kullanarak zamanlamaları oluşturun.  Bu, kullanıcının bir saat belirtme gereksinimini kaldırır, ancak çözümünüzün zamanlamasını içeremezsiniz, bu nedenle çözüm kaldırıldığında kaldırılacaktır.


### <a name="job-schedules"></a>İş zamanlamaları
İş çizelgesi kaynakları, bir çalışma kitabını zamanlamayla bağlar.  Bir **microsoft.automation/automationAccounts/jobSchedules** türüne sahiptirler ve aşağıdaki yapıya sahiptirler.  Bu, bu kod parçacıklarını çözüm dosyanıza kopyalayıp yapıştırabilmeniz ve parametre adlarını değiştirebilmeniz için ortak değişkenleri ve parametreleri içerir. 

    {
      "name": "[concat(parameters('accountName'), '/', variables('Schedule').LinkGuid)]",
      "type": "microsoft.automation/automationAccounts/jobSchedules",
      "apiVersion": "[variables('AutomationApiVersion')]",
      "location": "[parameters('regionId')]",
      "dependsOn": [
        "[resourceId('Microsoft.Automation/automationAccounts/runbooks/', parameters('accountName'), variables('Runbook').Name)]",
        "[resourceId('Microsoft.Automation/automationAccounts/schedules/', parameters('accountName'), variables('Schedule').Name)]"
      ],
      "tags": {
      },
      "properties": {
        "schedule": {
          "name": "[variables('Schedule').Name]"
        },
        "runbook": {
          "name": "[variables('Runbook').Name]"
        }
      }
    }


İş zamanlamalarının özellikleri aşağıdaki tabloda açıklanmıştır.

| Özellik | Açıklama |
|:--- |:--- |
| zamanlama adı |Zamanlamanın adını içeren tek **ad** varlığı. |
| runbook adı  |Runbook adını içeren tek **ad** varlığı.  |



## <a name="variables"></a>Değişkenler
[Azure Otomasyon değişkenleri](../../automation/automation-variables.md) bir **microsoft.automation/automationAccounts/variable** türüne sahiptir ve aşağıdaki yapıya sahiptir.  Bu, bu kod parçacıklarını çözüm dosyanıza kopyalayıp yapıştırabilmeniz ve parametre adlarını değiştirebilmeniz için ortak değişkenleri ve parametreleri içerir.

    {
      "name": "[concat(parameters('accountName'), '/', variables('Variable').Name)]",
      "type": "microsoft.automation/automationAccounts/variables",
      "apiVersion": "[variables('AutomationApiVersion')]",
      "tags": { },
      "dependsOn": [
      ],
      "properties": {
        "description": "[variables('Variable').Description]",
        "isEncrypted": "[variables('Variable').Encrypted]",
        "type": "[variables('Variable').Type]",
        "value": "[variables('Variable').Value]"
      }
    }

Değişken kaynakların özellikleri aşağıdaki tabloda açıklanmıştır.

| Özellik | Açıklama |
|:--- |:--- |
| açıklama | Değişken için isteğe bağlı açıklama. |
| ısencrypted | Değişkenin şifrelenip şifrelenmemesi gerektiğini belirtir. |
| type | Bu özelliğin şu anda hiçbir etkisi yoktur.  Değişkenin veri türü ilk değere göre belirlenir. |
| value | Değişkenin değeri. |

> [!NOTE]
> **Tür** özelliğişu anda oluşturulan değişken üzerinde hiçbir etkisi yoktur.  Değişkenin veri türü değere göre belirlenir.  

Değişkenin başlangıç değerini ayarlarsanız, doğru veri türü olarak yapılandırılmalıdır.  Aşağıdaki tablo, izin verilebilen farklı veri türlerini ve bunların sözdizimini sağlar.  JSON'daki değerlerin her zaman tırnak içinde özel karakterler içeren tırnak işaretleriyle eklenmesinin beklendiğini unutmayın.  Örneğin, bir dize değeri dize etrafında tırnak işaretleri (kaçış karakteri kullanarak (\\)) olarak belirtilirken, sayısal bir değer bir tırnak kümesiyle belirtilir.

| Veri türü | Açıklama | Örnek | Giderir |
|:--|:--|:--|:--|
| string   | Değeri çift tırnak içine ala.  | "\"Merhaba\"dünya " | "Merhaba dünya" |
| sayısal  | Tek tırnak işaretleri yle sayısal değer.| "64" | 64 |
| boole  | tırnak içinde **doğru** veya **yanlış.**  Bu değerin küçük olması gerektiğini unutmayın. | "Doğru" | true |
| datetime | Serileştirilmiş tarih değeri.<br>Belirli bir tarih için bu değeri oluşturmak için PowerShell'deki ConvertTo-Json cmdlet'i kullanabilirsiniz.<br>Örnek: get-date "5/5/2017 13:14:57" \| ConvertTo-Json | "\\/Tarih(1495656897378)\\/" | 2017-05-24 13:14:57 |

## <a name="modules"></a>Modules
Yönetim çözümünüzün runbook'larınız tarafından kullanılan [küresel modülleri](../../automation/automation-integration-modules.md) tanımlaması gerekmez, çünkü bunlar Otomasyon hesabınızda her zaman kullanılabilir olacaktır.  Runbook'larınız tarafından kullanılan diğer modüller için bir kaynak eklemeniz gerekir.

[Entegrasyon modülleri](../../automation/automation-integration-modules.md) bir **microsoft.automation/automationAccounts/module** türüne sahiptir ve aşağıdaki yapıya sahiptir.  Bu, bu kod parçacıklarını çözüm dosyanıza kopyalayıp yapıştırabilmeniz ve parametre adlarını değiştirebilmeniz için ortak değişkenleri ve parametreleri içerir.

    {
      "name": "[concat(parameters('accountName'), '/', variables('Module').Name)]",
      "type": "Microsoft.Automation/automationAccounts/modules",
      "apiVersion": "[variables('AutomationApiVersion')]",
      "dependsOn": [
      ],
      "properties": {
        "contentLink": {
          "uri": "[variables('Module').Uri]"
        }
      }
    }


Modül kaynaklarının özellikleri aşağıdaki tabloda açıklanmıştır.

| Özellik | Açıklama |
|:--- |:--- |
| Contentlink |Modülün içeriğini belirtir. <br><br>uri - Uri modülün içeriğine.  Bu, PowerShell ve Script runbook'ları için bir .ps1 dosyası ve Grafik runbook'u için dışa aktarılan grafik çalışma kitabı dosyası olacaktır.  <br> sürüm - Kendi izleme için modülün sürümü. |

Runbook, çalışma kitabından önce oluşturulduğundan emin olmak için modül kaynağına bağlı olmalıdır.

### <a name="updating-modules"></a>Modülleri güncelleme
Zamanlama kullanan bir runbook içeren bir yönetim çözümünün güncelleştirmesi ve çözümünüzün yeni sürümünde bu runbook tarafından kullanılan yeni bir modül varsa, runbook modülün eski sürümünü kullanabilir.  Çözüme aşağıdaki runbook'ları eklemeli ve bunları diğer runbook'lardan önce çalıştırmak için bir iş oluşturmalısınız.  Bu, runbook'lar yüklenmeden önce tüm modüllerin gerektiği gibi güncellenmesini sağlar.

* [Update-ModulesinAutomationToLatestVersion,](https://www.powershellgallery.com/packages/Update-ModulesInAutomationToLatestVersion/1.03/) çözümünüzüzde runbook'lar tarafından kullanılan tüm modüllerin en son sürüm olmasını sağlayacaktır.  
* [ReRegisterAutomationSchedule-MS-Mgmt,](https://www.powershellgallery.com/packages/ReRegisterAutomationSchedule-MS-Mgmt/1.0/) en son modülleri kullanarak bunlara bağlı runbook'ların tüm zaman kaynaklarının yeniden kaydedilmesini sağlar.




## <a name="sample"></a>Örnek
Aşağıdaki kaynakları içeren bir çözüm örneği verem:

- Runbook.  Bu, herkese açık bir GitHub deposunda depolanan örnek bir runbook'tur.
- Çözüm yüklendiğinde runbook'u başlatan otomasyon işi.
- Runbook'u düzenli aralıklarla başlatmak için zamanlama ve iş çizelgesi.
- Sertifika.
- Kimlik bilgisi.
- Değişken.
- Modülü.  Bu, Log Analytics'e veri yazmak için [OMSIngestionAPI modülüdür.](https://www.powershellgallery.com/packages/OMSIngestionAPI/1.5) 

Örnek, kaynak tanımlarında hardcoding değerlerinin aksine, bir çözümde yaygın olarak kullanılacak [standart çözüm parametreleri]( solutions-solution-file.md#parameters) değişkenlerini kullanır.


    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "workspaceName": {
          "type": "string",
          "metadata": {
            "Description": "Name of Log Analytics workspace."
          }
        },
        "accountName": {
          "type": "string",
          "metadata": {
            "Description": "Name of Automation account."
          }
        },
        "workspaceregionId": {
          "type": "string",
          "metadata": {
            "Description": "Region of Log Analytics workspace."
          }
        },
        "regionId": {
          "type": "string",
          "metadata": {
            "Description": "Region of Automation account."
          }
        },
        "pricingTier": {
          "type": "string",
          "metadata": {
            "Description": "Pricing tier of both Log Analytics workspace and Azure Automation account."
          }
        },
        "certificateBase64Value": {
          "type": "string",
          "metadata": {
            "Description": "Base 64 value for certificate."
          }
        },
        "certificateThumbprint": {
          "type": "securestring",
          "metadata": {
            "Description": "Thumbprint for certificate."
          }
        },
        "credentialUsername": {
          "type": "string",
          "metadata": {
            "Description": "Username for credential."
          }
        },
        "credentialPassword": {
          "type": "securestring",
          "metadata": {
            "Description": "Password for credential."
          }
        },
        "scheduleStartTime": {
          "type": "string",
          "metadata": {
            "Description": "Start time for schedule."
          }
        },
        "scheduleTimeZone": {
          "type": "string",
          "metadata": {
            "Description": "Time zone for schedule."
          }
        },
        "scheduleLinkGuid": {
          "type": "string",
          "metadata": {
            "description": "GUID for the schedule link to runbook.",
            "control": "guid"
          }
        },
        "runbookJobGuid": {
          "type": "string",
          "metadata": {
            "description": "GUID for the runbook job.",
            "control": "guid"
          }
        }
      },
      "variables": {
        "SolutionName": "MySolution",
        "SolutionVersion": "1.0",
        "SolutionPublisher": "Contoso",
        "ProductName": "SampleSolution",
    
        "LogAnalyticsApiVersion": "2015-11-01-preview",
        "AutomationApiVersion": "2015-10-31",
    
        "Runbook": {
          "Name": "MyRunbook",
          "Description": "Sample runbook",
          "Type": "PowerShell",
          "Uri": "https://raw.githubusercontent.com/user/myrepo/master/samples/MyRunbook.ps1",
          "JobGuid": "[parameters('runbookJobGuid')]"
        },
    
        "Certificate": {
          "Name": "MyCertificate",
          "Base64Value": "[parameters('certificateBase64Value')]",
          "Thumbprint": "[parameters('certificateThumbprint')]"
        },
    
        "Credential": {
          "Name": "MyCredential",
          "UserName": "[parameters('credentialUsername')]",
          "Password": "[parameters('credentialPassword')]"
        },
    
        "Schedule": {
          "Name": "MySchedule",
          "Description": "Sample schedule",
          "IsEnabled": "true",
          "Interval": "1",
          "Frequency": "hour",
          "StartTime": "[parameters('scheduleStartTime')]",
          "TimeZone": "[parameters('scheduleTimeZone')]",
          "LinkGuid": "[parameters('scheduleLinkGuid')]"
        },
    
        "Variable": {
          "Name": "MyVariable",
          "Description": "Sample variable",
          "Encrypted": 0,
          "Type": "string",
          "Value": "'This is my string value.'"
        },
    
        "Module": {
          "Name": "OMSIngestionAPI",
          "Uri": "https://devopsgallerystorage.blob.core.windows.net/packages/omsingestionapi.1.3.0.nupkg"
        }
      },
      "resources": [
        {
          "name": "[concat(variables('SolutionName'), '[' ,parameters('workspacename'), ']')]",
          "location": "[parameters('workspaceRegionId')]",
          "tags": { },
          "type": "Microsoft.OperationsManagement/solutions",
          "apiVersion": "[variables('LogAnalyticsApiVersion')]",
          "dependsOn": [
            "[resourceId('Microsoft.Automation/automationAccounts/runbooks/', parameters('accountName'), variables('Runbook').Name)]",
            "[resourceId('Microsoft.Automation/automationAccounts/jobs/', parameters('accountName'), variables('Runbook').JobGuid)]",
            "[resourceId('Microsoft.Automation/automationAccounts/certificates/', parameters('accountName'), variables('Certificate').Name)]",
            "[resourceId('Microsoft.Automation/automationAccounts/credentials/', parameters('accountName'), variables('Credential').Name)]",
            "[resourceId('Microsoft.Automation/automationAccounts/schedules/', parameters('accountName'), variables('Schedule').Name)]",
            "[resourceId('Microsoft.Automation/automationAccounts/jobSchedules/', parameters('accountName'), variables('Schedule').LinkGuid)]",
            "[resourceId('Microsoft.Automation/automationAccounts/variables/', parameters('accountName'), variables('Variable').Name)]",
            "[resourceId('Microsoft.Automation/automationAccounts/modules/', parameters('accountName'), variables('Module').Name)]"
          ],
          "properties": {
            "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspacename'))]",
            "referencedResources": [
              "[resourceId('Microsoft.Automation/automationAccounts/modules/', parameters('accountName'), variables('Module').Name)]"
            ],
            "containedResources": [
              "[resourceId('Microsoft.Automation/automationAccounts/runbooks/', parameters('accountName'), variables('Runbook').Name)]",
              "[resourceId('Microsoft.Automation/automationAccounts/jobs/', parameters('accountName'), variables('Runbook').JobGuid)]",
              "[resourceId('Microsoft.Automation/automationAccounts/certificates/', parameters('accountName'), variables('Certificate').Name)]",
              "[resourceId('Microsoft.Automation/automationAccounts/credentials/', parameters('accountName'), variables('Credential').Name)]",
              "[resourceId('Microsoft.Automation/automationAccounts/schedules/', parameters('accountName'), variables('Schedule').Name)]",
              "[resourceId('Microsoft.Automation/automationAccounts/jobSchedules/', parameters('accountName'), variables('Schedule').LinkGuid)]",
              "[resourceId('Microsoft.Automation/automationAccounts/variables/', parameters('accountName'), variables('Variable').Name)]"
            ]
          },
          "plan": {
            "name": "[concat(variables('SolutionName'), '[' ,parameters('workspaceName'), ']')]",
            "Version": "[variables('SolutionVersion')]",
            "product": "[variables('ProductName')]",
            "publisher": "[variables('SolutionPublisher')]",
            "promotionCode": ""
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Runbook').Name)]",
          "type": "Microsoft.Automation/automationAccounts/runbooks",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "dependsOn": [
          ],
          "location": "[parameters('regionId')]",
          "tags": { },
          "properties": {
            "runbookType": "[variables('Runbook').Type]",
            "logProgress": "true",
            "logVerbose": "true",
            "description": "[variables('Runbook').Description]",
            "publishContentLink": {
              "uri": "[variables('Runbook').Uri]",
              "version": "1.0.0.0"
            }
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Runbook').JobGuid)]",
          "type": "Microsoft.Automation/automationAccounts/jobs",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "location": "[parameters('regionId')]",
          "dependsOn": [
            "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('Runbook').Name)]"
          ],
          "tags": { },
          "properties": {
            "runbook": {
              "name": "[variables('Runbook').Name]"
            },
            "parameters": {
              "targetSubscriptionId": "[subscription().subscriptionId]",
              "resourcegroup": "[resourceGroup().name]",
              "automationaccount": "[parameters('accountName')]"
            }
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Certificate').Name)]",
          "type": "Microsoft.Automation/automationAccounts/certificates",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "location": "[parameters('regionId')]",
          "tags": { },
          "dependsOn": [
          ],
          "properties": {
            "Base64Value": "[variables('Certificate').Base64Value]",
            "Thumbprint": "[variables('Certificate').Thumbprint]"
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Credential').Name)]",
          "type": "Microsoft.Automation/automationAccounts/credentials",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "location": "[parameters('regionId')]",
          "tags": { },
          "dependsOn": [
          ],
          "properties": {
            "userName": "[variables('Credential').UserName]",
            "password": "[variables('Credential').Password]"
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Schedule').Name)]",
          "type": "microsoft.automation/automationAccounts/schedules",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "tags": { },
          "dependsOn": [
          ],
          "properties": {
            "description": "[variables('Schedule').Description]",
            "startTime": "[variables('Schedule').StartTime]",
            "timeZone": "[variables('Schedule').TimeZone]",
            "isEnabled": "[variables('Schedule').IsEnabled]",
            "interval": "[variables('Schedule').Interval]",
            "frequency": "[variables('Schedule').Frequency]"
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Schedule').LinkGuid)]",
          "type": "microsoft.automation/automationAccounts/jobSchedules",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "location": "[parameters('regionId')]",
          "dependsOn": [
            "[resourceId('Microsoft.Automation/automationAccounts/runbooks/', parameters('accountName'), variables('Runbook').Name)]",
            "[resourceId('Microsoft.Automation/automationAccounts/schedules/', parameters('accountName'), variables('Schedule').Name)]"
          ],
          "tags": {
          },
          "properties": {
            "schedule": {
              "name": "[variables('Schedule').Name]"
            },
            "runbook": {
              "name": "[variables('Runbook').Name]"
            }
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Variable').Name)]",
          "type": "microsoft.automation/automationAccounts/variables",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "tags": { },
          "dependsOn": [
          ],
          "properties": {
            "description": "[variables('Variable').Description]",
            "isEncrypted": "[variables('Variable').Encrypted]",
            "type": "[variables('Variable').Type]",
            "value": "[variables('Variable').Value]"
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Module').Name)]",
          "type": "Microsoft.Automation/automationAccounts/modules",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "dependsOn": [
          ],
          "properties": {
            "contentLink": {
              "uri": "[variables('Module').Uri]"
            }
          }
        }
    
      ],
      "outputs": { }
    }




## <a name="next-steps"></a>Sonraki adımlar
* Toplanan verileri görselleştirmek için [çözümünüze bir görünüm ekleyin.]( solutions-resources-views.md)
