---
title: Azure Arc etkin sunucularıyla VM Uzantısı yönetimi
description: Azure Arc etkin sunucuları, Azure olmayan VM 'lerle dağıtım sonrası yapılandırma ve otomasyon görevleri sağlayan sanal makine uzantılarının dağıtımını yönetebilir.
ms.date: 09/02/2020
ms.topic: conceptual
ms.openlocfilehash: 988c4d7b2fcbffb95932fe70d8014de74dd33343
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90887759"
---
# <a name="virtual-machine-extension-management-with-azure-arc-enabled-servers"></a>Azure Arc etkin sunucularıyla sanal makine uzantısı yönetimi

Sanal makine (VM) uzantıları, Azure VM 'lerinde dağıtım sonrası yapılandırma ve otomasyon görevleri sağlayan küçük uygulamalardır. Örneğin bir sanal makinede yazılım yüklemesi gerekiyorsa, virüsten koruma gerekiyorsa veya içinde bir betik çalıştırılacaksa VM uzantısı kullanılabilir.

Azure yay özellikli sunucular, Azure VM uzantılarını Azure olmayan Windows ve Linux VM 'lerine dağıtmanıza olanak tanıdığından, karma makinenizin şirket içi, kenar ve diğer bulut ortamlarının yaşam döngülerinde yönetimini basitleştirir.

## <a name="key-benefits"></a>Önemli avantajlar

Azure Arc etkin sunucuları VM uzantısı desteği aşağıdaki önemli avantajları sağlar:

* [Azure Otomasyonu durum yapılandırması](../../automation/automation-dsc-overview.md) ' nı kullanarak yapılandırmaları merkezi olarak depolar ve DSC VM Uzantısı aracılığıyla etkinleştirilen karma bağlı makinelerin istenen durumunu koruyun.

* [Azure izleyici 'de Günlükler](../../azure-monitor/platform/data-platform-logs.md) ile analizler için günlük verilerini log ANALYTICS aracı VM Uzantısı aracılığıyla toplayın. Bu, çeşitli kaynaklardaki veriler arasında karmaşık analizler gerçekleştirmek için yararlıdır.

* [VM'ler için Azure izleyici](../../azure-monitor/insights/vminsights-overview.md), Windows ve Linux sanal makinelerinizin performansını analiz eder ve diğer kaynaklardaki ve dış süreçlerdeki işlem ve bağımlılıklarını izler. Bu, hem Log Analytics Aracısı hem de bağımlılık Aracısı VM uzantılarını etkinleştirerek elde edilir.

* Özel Betik uzantısını kullanarak karma bağlantılı makinelerde komut dosyalarını indirip yürütün. Bu uzantı, dağıtım sonrası yapılandırma, yazılım yükleme veya başka herhangi bir yapılandırma ya da yönetim görevi için yararlıdır.

## <a name="availability"></a>Kullanılabilirlik

VM Uzantısı işlevselliği yalnızca [Desteklenen bölgeler](overview.md#supported-regions)listesinde kullanılabilir. Makinenizi bu bölgelerden birine yüklediğinizden emin olun.

## <a name="extensions"></a>Uzantılar

Bu önizlemede, Windows ve Linux makinelerinde aşağıdaki VM uzantılarını destekliyoruz.

|Uzantı |İşletim Sistemi |Publisher |Ek bilgiler |
|----------|---|----------|-----------------------|
|CustomScriptExtension |Windows |Microsoft.Compute |[Windows Özel Betik uzantısı](../../virtual-machines/extensions/custom-script-windows.md)|
|DSC |Windows |Microsoft. PowerShell|[Windows PowerShell DSC Uzantısı](../../virtual-machines/extensions/dsc-windows.md)|
|Log Analytics aracısı |Windows |Microsoft. EnterpriseCloud. Monitoring |[Windows için Log Analytics VM Uzantısı](../../virtual-machines/extensions/oms-windows.md)|
|Microsoft bağımlılık Aracısı | Windows |Microsoft.Compute | [Windows için bağımlılık Aracısı sanal makine uzantısı](../../virtual-machines/extensions/agent-dependency-windows.md)|
|CustomScript|Linux |Microsoft. Azure. Extension |[Linux özel Betik uzantısı sürüm 2](../../virtual-machines/extensions/custom-script-linux.md) |
|DSC |Linux |Microsoft. OSTCExtensions |[Linux için PowerShell DSC Uzantısı](../../virtual-machines/extensions/dsc-linux.md) |
|Log Analytics aracısı |Linux |Microsoft. EnterpriseCloud. Monitoring |[Linux için Log Analytics VM Uzantısı](../../virtual-machines/extensions/oms-linux.md) |
|Microsoft bağımlılık Aracısı | Linux |Microsoft.Compute | [Linux için bağımlılık Aracısı sanal makine uzantısı](../../virtual-machines/extensions/agent-dependency-linux.md) |

VM uzantıları, Azure portal veya Arc etkin sunucularla yönetilen karma sunucularda Azure PowerShell Azure Resource Manager şablonlarla çalıştırılabilir.

Azure bağlı makine Aracısı paketi ve uzantı Aracısı bileşeni hakkındaki ayrıntılar hakkında bilgi edinmek için bkz. [aracıya genel bakış](agent-overview.md#agent-component-details).

## <a name="prerequisites"></a>Önkoşullar

Bu özellik, aboneliğinizdeki aşağıdaki Azure Kaynak sağlayıcılarına bağımlıdır:

* **Microsoft. HybridCompute**
* **Microsoft. GuestConfiguration**

Henüz kayıtlı değilse, [Azure kaynak sağlayıcıları 'Nı kaydetme](agent-overview.md#register-azure-resource-providers)bölümündeki adımları izleyin.

Linux için Log Analytics Agent VM uzantısı, hedef makinede Python 2. x ' in yüklü olmasını gerektirir.

### <a name="connected-machine-agent"></a>Bağlı makine Aracısı

Makinenizin Azure bağlı makine Aracısı için desteklenen Windows ve Linux işletim sistemi [sürümleriyle](agent-overview.md#supported-operating-systems) eşleştiğini doğrulayın.

Bu özellikle desteklenen bağlı makine aracısının en düşük sürümü:

* Windows-0.7. x
* Linux-0.8. x

Makinenizi gereken aracının sürümüne yükseltmek için bkz. [yükseltme Aracısı](manage-agent.md#upgrading-agent).

## <a name="enable-extensions-from-the-portal"></a>Portaldan uzantıları etkinleştirme

VM uzantıları, Azure portal aracılığıyla sunucu tarafından yönetilen makineye yönelik yaya uygulanabilir.

1. Tarayıcınızdan [Azure Portal](https://aka.ms/arcserver-preview)gidin.

2. Portalda, **sunucular-Azure Arc** ' a gidin ve listeden karma makinenizi seçin.

3. **Uzantılar**' ı seçin ve **Ekle**' yi seçin. Kullanılabilir uzantılar listesinden istediğiniz uzantıyı seçin ve sihirbazdaki yönergeleri izleyin. Bu örnekte, Log Analytics VM uzantısını dağıtacağız.

    ![Seçilen makine için VM Uzantısı Seç](./media/manage-vm-extensions/add-vm-extensions.png)

    Aşağıdaki örnek, Azure portal Log Analytics VM uzantısının yüklenmesini gösterir:

    ![Log Analytics VM uzantısını yükler](./media/manage-vm-extensions/mma-extension-config.png)

    Yüklemeyi gerçekleştirmek için çalışma alanı KIMLIĞI ve birincil anahtarı sağlamanız gerekir. Bu bilgilerin nasıl bulunacağını bilmiyorsanız bkz. [çalışma alanı kimliğini ve anahtarını alma](../../azure-monitor/platform/log-analytics-agent.md#workspace-id-and-key).

4. Belirtilen gerekli bilgileri onayladıktan sonra **Oluştur**' u seçin. Dağıtımın bir özeti görüntülenir ve dağıtımın durumunu gözden geçirebilirsiniz.

>[!NOTE]
>Birden çok uzantı birlikte toplanmış ve işlenebilir olsa da, bunlar hizmet temelli olarak yüklenir. İlk uzantı yüklemesi tamamlandıktan sonra, sonraki uzantının yüklenmesi denenir.

## <a name="azure-resource-manager-templates"></a>Azure Resource Manager şablonları

VM uzantıları, bir Azure Resource Manager şablonuna eklenebilir ve şablonun dağıtımıyla birlikte yürütülür. Yay etkin sunucular tarafından desteklenen VM uzantıları ile, desteklenen VM uzantısını Azure PowerShell kullanarak Linux veya Windows makinelerde dağıtabilirsiniz. Aşağıdaki her örnek, şablona sağlanacak örnek değerleri içeren bir şablon dosyası ve bir parametre dosyası içerir.

>[!NOTE]
>Birden çok uzantı birlikte toplanmış ve işlenebilir olsa da, bunlar hizmet temelli olarak yüklenir. İlk uzantı yüklemesi tamamlandıktan sonra, sonraki uzantının yüklenmesi denenir.

### <a name="deploy-the-log-analytics-vm-extension"></a>Log Analytics VM uzantısını dağıtma

Log Analytics aracısını kolayca dağıtmak için, aracıyı Windows veya Linux 'a yüklemek üzere aşağıdaki örnek verilmiştir.

#### <a name="template-file-for-linux"></a>Linux için şablon dosyası

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "vmName": {
            "type": "string"
        },
        "location": {
            "type": "string"
        },
        "workspaceId": {
            "type": "string"
        },
        "workspaceKey": {
            "type": "string"
        }
    },
    "resources": [
        {
            "name": "[concat(parameters('vmName'),'/OMSAgentForLinux')]",
            "type": "Microsoft.HybridCompute/machines/extensions",
            "location": "[parameters('location')]",
            "apiVersion": "2019-08-02-preview",
            "properties": {
                "publisher": "Microsoft.EnterpriseCloud.Monitoring",
                "type": "OmsAgentForLinux",
                "settings": {
                    "workspaceId": "[parameters('workspaceId')]"
                },
                "protectedSettings": {
                    "workspaceKey": "[parameters('workspaceKey')]"
                }
            }
        }
    ]
}
```

#### <a name="template-file-for-windows"></a>Windows için şablon dosyası

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "vmName": {
            "type": "string"
        },
        "location": {
            "type": "string"
        },
        "workspaceId": {
            "type": "string"
        },
        "workspaceKey": {
            "type": "string"
        }
    },
    "resources": [
        {
            "name": "[concat(parameters('vmName'),'/MicrosoftMonitoringAgent')]",
            "type": "Microsoft.HybridCompute/machines/extensions",
            "location": "[parameters('location')]",
            "apiVersion": "2019-08-02-preview",
            "properties": {
                "publisher": "Microsoft.EnterpriseCloud.Monitoring",
                "type": "MicrosoftMonitoringAgent",
                "autoUpgradeMinorVersion": true,
                "settings": {
                    "workspaceId": "[parameters('workspaceId')]"
                },
                "protectedSettings": {
                    "workspaceKey": "[parameters('workspaceKey')]"
                }
            }
        }
    ]
}
```

#### <a name="parameter-file"></a>Parametre dosyası

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "vmName": {
            "value": "<vmName>"
        },
        "location": {
            "value": "<region>"
        },
        "workspaceId": {
            "value": "<MyWorkspaceID>"
        },
        "workspaceKey": {
            "value": "<MyWorkspaceKey>"
        }
    }
}
```

Şablon ve parametre dosyalarını diske kaydedin ve parametre dosyasını dağıtımınız için uygun değerlerle düzenleyin. Daha sonra uzantıyı aşağıdaki komutla bir kaynak grubu içindeki tüm bağlı makinelere yükleyebilirsiniz. Komut, parametreleri ve parametre değerlerini içeren bir dosyayı belirtmek için Template ve *Templateparameterfile* parametresini belirtmek üzere *TemplateFile* parametresini kullanır.

```powershell
New-AzResourceGroupDeployment -ResourceGroupName "ContosoEngineering" -TemplateFile "D:\Azure\Templates\LogAnalyticsAgentWin.json" -TemplateParameterFile "D:\Azure\Templates\LogAnalyticsAgentWinParms.json"
```

### <a name="deploy-the-custom-script-extension"></a>Özel Betik uzantısını dağıtma

Özel Betik uzantısını kullanmak için aşağıdaki örnek Windows ve Linux 'ta çalışacak şekilde sunulmaktadır. Özel Betik uzantısı hakkında bilginiz yoksa, bkz. [Windows Için özel Betik uzantısı](../../virtual-machines/extensions/custom-script-windows.md) veya [Linux Için özel Betik uzantısı](../../virtual-machines/extensions/custom-script-linux.md). Bu uzantıyı karma makinelerle kullanırken anlamanız gereken farklı özellikler vardır:

* Azure sanal makine özel Betik uzantısı ile desteklenen işletim sistemlerinin listesi, Azure Arc özellikli sunucular için geçerli değildir. Yay etkin sunucular için desteklenen OSs listesi [burada](agent-overview.md#supported-operating-systems)bulunabilir.

* Azure sanal makine ölçek kümeleri veya klasik VM 'Lerle ilgili yapılandırma ayrıntıları geçerli değildir.

* Makinelerinizin dışarıdan bir betiği indirmesi ve yalnızca bir ara sunucu üzerinden iletişim kurabilmesi gerekiyorsa, proxy sunucusu çevresel değişkenini ayarlamak için [bağlı makine aracısını yapılandırmanız](manage-agent.md#update-or-remove-proxy-settings) gerekir.

Özel Betik uzantısı yapılandırması, betik konumu ve çalıştırılacak komut gibi şeyleri belirtir. Bu yapılandırma, hem Linux hem de Windows karma makineleri için aşağıda sağlanmış olan bir Azure Resource Manager şablonunda belirtilmiştir.

#### <a name="template-file-for-linux"></a>Linux için şablon dosyası

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "vmName": {
      "type": "string"
    },
    "location": {
      "type": "string"
    },
    "fileUris": {
      "type": "array"
    },
    "commandToExecute": {
      "type": "securestring"
    }
  },
  "resources": [
    {
      "name": "[concat(parameters('vmName'),'/CustomScript')]",
      "type": "Microsoft.HybridCompute/machines/extensions",
      "location": "[parameters('location')]",
      "apiVersion": "2019-08-02-preview",
      "properties": {
        "publisher": "Microsoft.Azure.Extensions",
        "type": "CustomScript",
        "autoUpgradeMinorVersion": true,
        "settings": {},
        "protectedSettings": {
          "commandToExecute": "[parameters('commandToExecute')]",
          "fileUris": "[parameters('fileUris')]"
        }
      }
    }
  ]
}
```

#### <a name="template-file-for-windows"></a>Windows için şablon dosyası

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "vmName": {
            "type": "string"
        },
        "location": {
            "type": "string"
        },
        "fileUris": {
            "type": "string"
        },
        "arguments": {
            "type": "securestring",
            "defaultValue": " "
        }
    },
    "variables": {
        "UriFileNamePieces": "[split(parameters('fileUris'), '/')]",
        "firstFileNameString": "[variables('UriFileNamePieces')[sub(length(variables('UriFileNamePieces')), 1)]]",
        "firstFileNameBreakString": "[split(variables('firstFileNameString'), '?')]",
        "firstFileName": "[variables('firstFileNameBreakString')[0]]"
    },
    "resources": [
        {
            "name": "[concat(parameters('vmName'),'/CustomScriptExtension')]",
            "type": "Microsoft.HybridCompute/machines/extensions",
            "location": "[parameters('location')]",
            "apiVersion": "2019-08-02-preview",
            "properties": {
                "publisher": "Microsoft.Compute",
                "type": "CustomScriptExtension",
                "autoUpgradeMinorVersion": true,
                "settings": {
                    "fileUris": "[split(parameters('fileUris'), ' ')]"
                },
                "protectedSettings": {
                    "commandToExecute": "[concat ('powershell -ExecutionPolicy Unrestricted -File ', variables('firstFileName'), ' ', parameters('arguments'))]"
                }
            }
        }
    ]
}
```

#### <a name="parameter-file"></a>Parametre dosyası

```json
{
  "$schema": "https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json#",
  "handler": "Microsoft.Azure.CreateUIDef",
  "version": "0.1.2-preview",
  "parameters": {
    "basics": [
      {}
    ],
    "steps": [
      {
        "name": "customScriptExt",
        "label": "Add Custom Script Extension",
        "elements": [
          {
            "name": "fileUris",
            "type": "Microsoft.Common.FileUpload",
            "label": "Script files",
            "toolTip": "The script files that will be downloaded to the virtual machine.",
            "constraints": {
              "required": false
            },
            "options": {
              "multiple": true,
              "uploadMode": "url"
            },
            "visible": true
          },
          {
            "name": "commandToExecute",
            "type": "Microsoft.Common.TextBox",
            "label": "Command",
            "defaultValue": "sh script.sh",
            "toolTip": "The command to execute, for example: sh script.sh",
            "constraints": {
              "required": true
            },
            "visible": true
          }
        ]
      }
    ],
    "outputs": {
      "vmName": "[vmName()]",
      "location": "[location()]",
      "fileUris": "[steps('customScriptExt').fileUris]",
      "commandToExecute": "[steps('customScriptExt').commandToExecute]"
    }
  }
}
```

### <a name="deploy-the-powershell-dsc-extension"></a>PowerShell DSC uzantısını dağıtma

PowerShell DSC uzantısını kullanmak için, Windows ve Linux 'ta çalışmak üzere aşağıdaki örnek verilmiştir. PowerShell DSC Uzantısı hakkında bilginiz yoksa bkz. [DSC uzantı işleyicisine genel bakış](../../virtual-machines/extensions/dsc-overview.md). Bu uzantıyı karma makinelerle kullanırken anlamanız gereken farklı özellikler vardır:

* Azure VM PowerShell DSC Uzantısı ile desteklenen işletim sistemlerinin listesi, Azure Arc özellikli sunucular için geçerli değildir. Yay etkin sunucular için desteklenen OSs listesi [burada](agent-overview.md#supported-operating-systems)bulunabilir.

* Makinelerinizin dışarıdan bir betiği indirmesi ve yalnızca bir ara sunucu üzerinden iletişim kurabilmesi gerekiyorsa, proxy sunucusu çevresel değişkenini ayarlamak için [bağlı makine aracısını yapılandırmanız](manage-agent.md#update-or-remove-proxy-settings) gerekir.

#### <a name="template-file-for-linux"></a>Linux için şablon dosyası

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "vmName": {
            "type": "string",
            "metadata": {
                "description": "Name of the vm, will be used as DNS Name for the Public IP used to access the Virtual Machine."
            }
        },
        "location": {
            "type": "string",
            "metadata": {
                "description": "Location for all resources."
            }
        },
        "mode": {
            "type": "string",
            "defaultValue": "Push",
            "metadata": {
                "description": "The functional mode, push MOF configuration (Push), distribute MOF configuration (Pull), install custom DSC module (Install)"
            },
            "allowedValues": [
                "Push",
                "Pull",
                "Install",
                "Register"
            ]
        },
        "fileUri": {
            "type": "string",
            "defaultValue": "",
            "metadata": {
                "description": "The uri of the MOF file/Meta MOF file/resource ZIP file"
            }
        },
        "registrationUrl": {
            "type": "string",
            "defaultValue": "",
            "metadata": {
                "description": "The URL of the Azure Automation account"
            }
        },
        "registrationKey": {
            "type": "string",
            "defaultValue": "",
            "metadata": {
                "description": "The access key of the Azure Automation account"
            }
        }
    },
    "resources": [
        {
            "name": "[concat(parameters('vmName'),'/DSCForLinux')]",
            "type": "Microsoft.HybridCompute/machines/extensions",
            "location": "[parameters('location')]",
            "apiVersion": "2019-08-02-preview",
            "properties": {
                "publisher": "Microsoft.OSTCExtensions",
                "type": "DSCForLinux",
                "settings": {
                    "Mode": "[parameters('mode')]",
                    "FileUri": "[parameters('fileUri')]"
                },
                "protectedSettings": {
                    "RegistrationUrl": "[parameters('registrationUrl')]",
                    "RegistrationKey": "[parameters('registrationKey')]"
                }
            }
        }
    ]
}
```

#### <a name="template-file-for-windows"></a>Windows için şablon dosyası

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "vmName": {
            "type": "string"
        },
        "location": {
            "type": "string"
        },
        "modulesUrl": {
            "type": "string"
        },
        "configurationFunction": {
            "type": "string"
        },
        "properties": {
            "type": "string",
            "defaultValue": ""
        },
        "dataBlobUri": {
            "type": "string",
            "defaultValue": ""
        },
        "wmfVersion": {
            "type": "string",
            "defaultValue": "latest",
            "allowedValues": [
                "4.0",
                "5.0",
                "5.1",
                "latest"
            ]
        },
        "privacy": {
            "type": "string",
            "defaultValue": ""
        },
        "autoUpdate": {
            "type": "bool",
            "defaultValue": false
        }
    },
    "resources": [
        {
            "name": "[concat(parameters('vmName'),'/Microsoft.Powershell.DSC')]",
            "type": "Microsoft.HybridCompute/machines/extensions",
            "location": "[parameters('location')]",
            "apiVersion": "2019-08-02-preview",
            "properties": {
                "publisher": "Microsoft.Powershell",
                "type": "DSC",
                "autoUpgradeMinorVersion": "[parameters('autoUpdate')]",
                "settings": {
                    "ModulesUrl": "[parameters('modulesUrl')]",
                    "ConfigurationFunction": "[parameters('configurationFunction')]",
                    "Properties": "[parameters('properties')]",
                    "WmfVersion": "[parameters('wmfVersion')]",
                    "Privacy": {
                        "DataCollection": "[parameters('privacy')]"
                    }
                },
                "protectedSettings": {
                    "DataBlobUri": "[parameters('dataBlobUri')]"
                }
            }
        }
    ]
}
```

#### <a name="parameter-file"></a>Parametre dosyası

```json
{
  "$schema": "https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json#",
  "handler": "Microsoft.Azure.CreateUIDef",
  "version": "0.1.2-preview",
  "parameters": {
    "basics": [
      {}
    ],
    "steps": [
      {
        "name": "dscExtension",
        "label": "Add DSC Extension",
        "elements": [
          {
            "name": "Mode",
            "type": "Microsoft.Common.OptionsGroup",
            "label": "Mode",
            "defaultValue": 0,
            "toolTip": "The functional mode, push MOF configuration (Push), distribute MOF configuration (Pull), install custom DSC module (Install)",
            "constraints": {
              "allowedValues": [
                {
                  "label": "Push",
                  "value": "Push"
                },
                {
                  "label": "Pull",
                  "value": "Pull"
                },
                {
                  "label": "Install",
                  "value": "Install"
                },
                {
                  "label": "Register",
                  "value": "Register"
                }
              ]
            },
            "visible": true
          },
          {
            "name": "FileUri",
            "type": "Microsoft.Common.FileUpload",
            "label": "File URI",
            "toolTip": "The uri of the MOF file/Meta MOF file/resource ZIP file",
            "constraints": {
              "required": false,
              "accept": ".psd1"
            },
            "options": {
              "multiple": false,
              "uploadMode": "url",
              "openMode": "binary",
              "encoding": "UTF-8"
            }
          },
          {
            "name": "RegistrationUrl",
            "type": "Microsoft.Common.TextBox",
            "label": "Registration URL",
            "toolTip": "The URL of the Azure Automation account",
            "constraints": {
              "required": false
            }
          },
          {
            "name": "RegistrationKey",
            "type": "Microsoft.Common.TextBox",
            "label": "Registration key",
            "toolTip": "The access key of the Azure Automation account",
            "constraints": {
              "required": false
            }
          }
        ]
      }
    ],
    "outputs": {
      "vmName": "[vmName()]",
      "location": "[location()]",
      "mode": "[steps('dscExtension').Mode]",
      "fileUri": "[steps('dscExtension').FileUri]",
      "registrationUrl": "[steps('dscExtension').RegistrationUrl]",
      "registrationKey": "[steps('dscExtension').RegistrationKey]"
    }
  }
}
```

### <a name="dependency-agent"></a>Bağımlılık aracısı

Azure Izleyici bağımlılık Aracısı uzantısını kullanmak için, Windows ve Linux 'ta çalışmak üzere aşağıdaki örnek verilmiştir. Bağımlılık aracısına alışkın değilseniz bkz. [Azure izleyici aracılarına genel bakış](../../azure-monitor/platform/agents-overview.md#dependency-agent).

#### <a name="template-file-for-linux"></a>Linux için şablon dosyası

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
  "parameters": {
    "vmName": {
      "type": "string",
      "metadata": {
        "description": "The name of existing Linux machine."
      }
    }
  },
  "variables": {
    "vmExtensionsApiVersion": "2017-03-30"
  },
  "resources": [
    {
      "type": "Microsoft.HybridCompute/machines/extensions",
      "name": "[concat(parameters('vmName'),'/DAExtension')]",
      "apiVersion": "[variables('vmExtensionsApiVersion')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
      ],
      "properties": {
        "publisher": "Microsoft.Azure.Monitoring.DependencyAgent",
        "type": "DependencyAgentLinux",
        "typeHandlerVersion": "9.5",
        "autoUpgradeMinorVersion": true
      }
    }
  ],
    "outputs": {
    }
}
```

#### <a name="template-file-for-windows"></a>Windows için şablon dosyası

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
  "parameters": {
    "vmName": {
      "type": "string",
      "metadata": {
        "description": "The name of existing Windows machine."
      }
    }
  },
  "variables": {
    "vmExtensionsApiVersion": "2017-03-30"
  },
  "resources": [
    {
      "type": "Microsoft.HybridCompute/machines/extensions",
      "name": "[concat(parameters('vmName'),'/DAExtension')]",
      "apiVersion": "[variables('vmExtensionsApiVersion')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
      ],
      "properties": {
        "publisher": "Microsoft.Azure.Monitoring.DependencyAgent",
        "type": "DependencyAgentWindows",
        "typeHandlerVersion": "9.5",
        "autoUpgradeMinorVersion": true
      }
    }
  ],
    "outputs": {
    }
}
```

## <a name="uninstall-extension"></a>Uzantıyı kaldır

Bir yay özellikli bir sunucudan bir veya daha fazla uzantının kaldırılması, yalnızca Azure portal üzerinden gerçekleştirilebilir. Bir uzantıyı kaldırmak için aşağıdaki adımları gerçekleştirin.

1. Tarayıcınızdan [Azure Portal](https://portal.azure.com)gidin.

2. Portalda, **sunucular-Azure Arc** ' a gidin ve listeden karma makinenizi seçin.

3. **Uzantılar**' ı seçin ve ardından yüklü uzantılar listesinden bir uzantı seçin.

4. **Kaldır** ' ı seçin ve doğrulamanız istendiğinde, devam etmek için **Evet** ' i seçin.

## <a name="troubleshooting"></a>Sorun giderme

Uzantı dağıtımlarının durumu hakkındaki veriler Azure portal alınabilir.

Aşağıdaki sorun giderme adımları tüm VM uzantıları için geçerlidir.

1. Konuk Aracısı günlüğünü denetlemek için, uzantınızın `%SystemDrive%\ProgramData\GuestConfig\ext_mgr_logs` Windows için ve altında Linux için sağlanması durumunda etkinliğe bakın `/var/lib/GuestConfig/ext_mgr_logs` .

2. Windows hakkında daha fazla ayrıntı için belirli bir uzantının uzantı günlüklerini denetleyin `%SystemDrive%\ProgramData\GuestConfig\extension_logs\<Extension>` . Uzantı çıkışı, Linux üzerinde yüklü her uzantı için bir dosyaya kaydedilir `/var/lib/GuestConfig/extension_logs` .

3. Hata kodları, bilinen sorunlar vb. için uzantıya özgü belge sorunlarını giderme bölümlerini denetleyin. Her uzantı için ek sorun giderme bilgileri, uzantının genel bakış konusunun **sorun giderme ve destek** bölümünde bulunabilir. Bu, günlüğe yazılan hata kodlarının açıklamasını içerir. Uzantı makaleleri, bu makalede daha önce bulunan [Uzantılar tablosuna](#extensions) bağlanır.

4. Sistem günlüklerine bakın. Uzantıya sahip olabilecek, Özel Paket Yöneticisi erişimi gerektiren başka bir uygulamanın uzun süre çalışan yüklemesi gibi diğer işlemleri denetleyin.

## <a name="next-steps"></a>Sonraki adımlar

- VM [Konuk yapılandırması](../../governance/policy/concepts/guest-configuration.md), makinenin beklenen Log Analytics çalışma alanına rapor olduğunu doğrulama, [VM 'lerle Azure izleyici](../../azure-monitor/insights/vminsights-enable-policy.md)ile izlemeyi etkinleştirme ve çok daha birçok şey için [Azure ilkesi](../../governance/policy/overview.md)'ni kullanarak makinenizi yönetmeyi öğrenin.

- [[Log Analytics Aracısı]](../../azure-monitor/platform/log-analytics-agent.md)hakkında daha fazla bilgi edinin. Windows ve Linux için Log Analytics Aracısı, işletim sistemi ve iş yükü izleme verilerini toplamak, Otomasyon Runbook 'larını veya Güncelleştirme Yönetimi gibi özellikleri kullanarak yönetmek ya da [Azure Güvenlik Merkezi](../../security-center/security-center-intro.md)gibi diğer Azure hizmetlerini kullanmak istediğinizde gereklidir.