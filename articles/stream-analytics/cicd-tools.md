---
title: CI/CD araçları kullanarak Azure Stream Analytics işin derlemelerini, testlerini ve dağıtımlarını otomatikleştirin
description: Bu makalede Azure Stream Analytics bir projeyi otomatik olarak derlemek, test etmek ve dağıtmak için Azure Stream Analytics CI/CD araçlarının nasıl kullanılacağı açıklanır.
services: stream-analytics
author: su-jie
ms.author: sujie
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: how-to
ms.date: 09/10/2020
ms.openlocfilehash: e772701396f172eaab906f99463bd9019728b531
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90941192"
---
# <a name="automate-builds-tests-and-deployments-of-an-azure-stream-analytics-job-using-cicd-tools"></a>CI/CD araçları kullanarak Azure Stream Analytics işin derlemelerini, testlerini ve dağıtımlarını otomatikleştirin

Azure Stream Analytics Visual Studio Code veya Visual Studio projelerinizi otomatik olarak derlemek, test etmek ve dağıtmak için Azure Stream Analytics CI/CD NPM paketini kullanabilirsiniz. Projeler, geliştirme araçları kullanılarak oluşturulabilir veya var olan Stream Analytics işlerden verilebilirler. Bu makalede, NPM paketinin herhangi bir CI/CD sistemiyle nasıl kullanılacağı açıklanır. Azure Pipelines dağıtım için, bkz. [Azure DevOps kullanarak bir Stream Analytics işi IÇIN CI/CD işlem hattı oluşturma](set-up-cicd-pipeline.md).

## <a name="installation"></a>Yükleme

Paketi doğrudan [indirebilir](https://www.npmjs.com/package/azure-streamanalytics-cicd) veya komutunu kullanarak [genel olarak](https://docs.npmjs.com/downloading-and-installing-packages-globally) yükleyebilirsiniz `npm install -g azure-streamanalytics-cicd` . **Azure Pipelines**bir derleme Işlem hattının PowerShell veya Azure CLI betik görevinde de kullanılabilen komutunu kullanmanızı öneririz.

## <a name="build-the-project"></a>Projeyi derleme

**Asa-streamanalytics-cicd** NPM paketi, Stream Analytics [Visual Studio Code proje](quick-create-vs-code.md) veya [Visual Studio projelerinin](stream-analytics-quick-create-vs.md)Azure Resource Manager şablonlar oluşturmak için araçlar sağlar. Ayrıca, Visual Studio Code veya Visual Studio yüklemeden Windows, macOS ve Linux 'ta NPM paketini de kullanabilirsiniz.

Paketi yükledikten sonra, Stream Analytics projelerinizi derlemek için aşağıdaki komutu kullanın.

```powershell
azure-streamanalytics-cicd build -project <projectFullPath> [-outputPath <outputPath>]
```

*Build* komutu bir anahtar sözcük sözdizimi denetimi yapar ve Azure Resource Manager şablonunu çıkarır.

| Parametre | Açıklama |
|---|---|
| `-project` | Visual Studio Code projeniz için dosyanın **asaproj.js** mutlak yolu veya **[projeniz adınız]. aşama proj** for Visual Studio projesi. |
| `-outputPath` | Azure Resource Manager şablonları için çıkış klasörünün yolu. Belirtilmemişse, şablonlar geçerli dizine yerleştirilir. |

#### <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

```powershell
azure-streamanalytics-cicd build -project "/Users/username/projects/samplejob/asaproj.json"
```

#### <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

```powershell
azure-streamanalytics-cicd build -project "/Users/username/projects/samplejob/samplejob.asaproj"
```

--- 

Bir Stream Analytics projesi başarıyla oluşturduğunda, çıkış klasörü altında aşağıdaki iki dosyayı oluşturur:

* Şablon dosyası Azure Resource Manager

   `[ProjectName].JobTemplate.json`

* Azure Resource Manager parametre dosyası

   `[ProjectName].JobTemplate.parameters.json`

parameters.jsdosyadaki varsayılan parametreler Visual Studio Code veya Visual Studio projenizin ayarlarından alınır. Başka bir ortama dağıtmak istiyorsanız, parametreleri uygun şekilde değiştirin.

Tüm kimlik bilgileri için varsayılan değerler **null**. Azure 'a dağıtmadan önce değerleri ayarlamanız gerekir.

```json
"Input_EntryStream_sharedAccessPolicyKey": {
      "value": null
    },
```

Azure Data Lake Store Gen1 için yönetilen kimliği bir çıkış havuzu olarak kullanmak için, Azure 'a dağıtmadan önce PowerShell kullanarak hizmet sorumlusuna erişim sağlamanız gerekir. [Kaynak Yöneticisi şablonuyla yönetilen kimlik ile ADLS 1. dağıtma](stream-analytics-managed-identities-adls.md#resource-manager-template-deployment)hakkında daha fazla bilgi edinin.

## <a name="local-run"></a>Yerel çalıştırma

Projenizde yerel giriş dosyaları belirtilmişse, komutunu kullanarak bir Stream Analytics betiğini yerel olarak çalıştırabilirsiniz `localrun` .

```powershell
azure-streamanalytics-cicd localrun -project <projectFullPath> [-outputPath <outputPath>] [-customCodeZipFilePath <zipFilePath>]
```

| Parametre | Açıklama |
|---|---|
| `-project` | Visual Studio projesi için Visual Studio Code projenizin veya **[projeniz adınız]. aşama proj** dosyasının **asaproj.js** yolu. |
| `-outputPath` | Çıkış klasörünün yolu. Belirtilmemişse, çıkış sonuç dosyaları geçerli dizine yerleştirilir. |
| `-customCodeZipFilePath` | Kullanılan bir UDF veya seri hale getirici gibi C# özel kodu için zip dosyasının yolu. Dll 'Leri bir zip dosyasında paketleyin ve bu yolu belirtin. |

#### <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

```powershell
azure-streamanalytics-cicd localrun -project "/Users/roger/projects/samplejob/asaproj.json"
```

#### <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

```powershell
azure-streamanalytics-cicd localrun -project "/Users/roger/projects/samplejob/samplejob.asaproj"
```

---

> [!Note] 
> JavaScript UDF yalnızca Windows üzerinde kullanılabilir.

## <a name="automated-test"></a>Otomatikleştirilmiş test

Stream Analytics betiğinizin otomatikleştirilmiş testlerini yapılandırmak ve çalıştırmak için CI/CD NPM paketini kullanabilirsiniz.

### <a name="add-a-test-case"></a>Test çalışması Ekle

Test çalışmaları, test yapılandırma dosyasında açıklanmıştır. Başlamak için `addtestcase` komutunu kullanarak test yapılandırma dosyasına bir test çalışması şablonu ekleyin. Test yapılandırma dosyası yoksa, biri varsayılan olarak oluşturulur.

```powershell
azure-streamanalytics-cicd addtestcase -project <projectFullPath> [-testConfigPath <testConfigFileFullPath>]
```

| Parametre | Açıklama |
|---|---|
| `-project` | Visual Studio projesi için Visual Studio Code projenizin veya **[projeniz adınız]. aşama proj** dosyasının **asaproj.js** yolu. |
| `-testConfigPath` | Test yapılandırma dosyasının yolu. Belirtilmemişse dosya, dosya **üzerindeasaproj.js** geçerli **Dizin altında,** varsayılan dosya adı **testConfig.js**olacak şekilde aranır. Mevcut değilse yeni bir dosya oluşturulur. |

#### <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

```powershell
azure-streamanalytics-cicd addtestcase -project "/Users/roger/projects/samplejob/asaproj.json"
```

#### <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

```powershell
azure-streamanalytics-cicd addtestcase -project "/Users/roger/projects/samplejob/samplejob.asaproj"
```
---

Test yapılandırma dosyası boşsa, aşağıdaki içerik dosyaya yazılır. Aksi halde test çalışması, **TestCases**dizisine eklenir. Gerekli giriş yapılandırmaları, varsa giriş yapılandırma dosyalarına göre otomatik olarak doldurulur. Aksi takdirde, varsayılan değerler yapılandırılır. Test çalıştırılmadan önce her girişin **FilePath** ve beklenen çıkışın belirtilmesi gerekir. Yapılandırmayı el ile değiştirebilirsiniz.

Test doğrulamasının belirli bir çıktıyı yok saymasını istiyorsanız, beklenen bu çıkışın **gerekli** alanını **false**olarak ayarlayın.

```json
{
  "Script": "",
  "TestCases": [
    {
      "Name": "Case 1",
      "Inputs": [
        {
          "InputAlias": [Input alias string],
          "Type": "Data Stream",
          "Format": "JSON",
          "FilePath": [Required],
          "ScriptType": "InputMock"
        }
      ],
      "ExpectedOutputs": [
        {
          "OutputAlias": [Output alias string],
          "FilePath": "Required",
          "Required": true
        }
      ]
    }
  ]
}
```

### <a name="run-a-unit-test"></a>Birim testi çalıştırma

Projeniz için birden çok test çalışması çalıştırmak için aşağıdaki komutu kullanabilirsiniz. Test sonuçlarının bir özeti çıkış klasöründe oluşturulur. İşlem geçilen tüm testler için **0** koduyla çıkıyor; özel durum için **-1** oluştu; testler için **-2** başarısız oldu.

```powershell
azure-streamanalytics-cicd test -project <projectFullPath> [-testConfigPath <testConfigFileFullPath>] [-outputPath <outputPath>] [-customCodeZipFilePath <zipFilePath>]
```

| Parametre | Açıklama |
|---|---|
| `-project` | Visual Studio projesi için Visual Studio Code projenizin veya **[projeniz adınız]. aşama proj** dosyasının **asaproj.js** yolu. |
| `-testConfigPath` | Test yapılandırma dosyasının yolu. Belirtilmemişse dosya, dosya **üzerindeasaproj.js** geçerli **Dizin altında,** varsayılan dosya adı **testConfig.js**olacak şekilde aranır.
| `-outputPath` | Test sonucu çıkış klasörünün yolu. Belirtilmemişse, çıkış sonuç dosyaları geçerli dizine yerleştirilir. |
| `-customCodeZipFilePath` | Kullanılan UDF veya seri hale getirici gibi özel kod için zip dosyasının yolu. |

Tüm testler tamamlandığında, JSON biçimindeki test sonuçlarının bir özeti çıkış klasöründe oluşturulur. Özet dosyası **testResultSummary.jsolarak**adlandırılır.

```json
{
  "Total": (integer) total_number_of_test_cases,
  "Passed": (integer) number_of_passed_test_cases,
  "Failed": (integer) number_of_failed_test_cases,
  "Script": (string) absolute_path_to_asaql_file,
  "Results": [ (array) detailed_results_of_test_cases
    {
      "Name": (string) name_of_test_case,
      "Status": (integer) 0(passed)_or_1(failed),
      "Time": (string) time_span_of_running_test_case,
      "OutputMatched": [ (array) records_of_actual_outputs_equal_to_expected_outputs
        {
          "OutputAlias": (string) output_alias,
          "ExpectedOutput": (string) path_to_the_expected_output_file,
          "Output": (string) path_to_the_actual_output_file
        }
      ],
      "OutputNotEqual": [ (array) records_of_actual_outputs_not_equal_to_expected_outputs
        {
          "OutputAlias": (string) output_alias,
          "ExpectedOutput": (string) path_to_the_expected_output_file,
          "Output": (string) path_to_the_actual_output_file
        }
      ],
      "OutputMissing": [ (array) records_of_actual_outputs_missing
        {
          "OutputAlias": (string) output_alias,
          "ExpectedOutput": (string) path_to_the_expected_output_file,
          "Output": ""
        }
      ],
      "OutputUnexpected": [ (array) records_of_actual_outputs_unexpected
        {
          "OutputAlias": (string) output_alias,
          "ExpectedOutput": "",
          "Output": (string) path_to_the_actual_output_file
        }
      ],
      "OutputUnrequired": [ (array) records_of_actual_outputs_unrequired_to_be_checked
        {
          "OutputAlias": (string) output_alias,
          "ExpectedOutput": (string) path_to_the_expected_output_file,
          "Output": (string) path_to_the_actual_output_file
        }
      ]
    }
  ],
  "Time": (string) time_span_of_running_all_test_cases,
}
```

## <a name="deploy-to-azure"></a>Azure’a dağıtın

[Işinizi Azure 'a dağıtmak](../azure-resource-manager/templates/template-tutorial-use-parameter-file.md?tabs=azure-powershell#deploy-template)için derlemeden oluşturulan Azure Resource Manager şablonu ve parametre dosyalarını kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Stream Analytics için sürekli tümleştirme ve sürekli dağıtım](cicd-overview.md)
* [Azure Pipelines kullanarak Stream Analytics işi için CI/CD işlem hattı ayarlama](set-up-cicd-pipeline.md)
