---
title: Öğretici - Terratest kullanarak Azure'daki Terraform modüllerini test edin
description: Terraform modüllerinizi test etmek için Terratest’i kullanmayı öğrenin.
ms.topic: tutorial
ms.date: 10/26/2019
ms.openlocfilehash: 687a793af2b9b75efe463b042d121c32f18974d6
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79370806"
---
# <a name="tutorial-test-terraform-modules-in-azure-using-terratest"></a>Öğretici: Terratest kullanarak Azure'daki Terraform modüllerini test edin

> [!NOTE]
> Bu makaledeki örnek kod 0.12 sürümü (ve daha büyük) ile çalışmıyor.

Yeniden kullanılabilir, kullanılabilir ve test edilebilir bileşenler oluşturmak için Azure Terraform modüllerini kullanabilirsiniz. Terraform modülleri, altyapının kod işlemleri olarak uygulanmasında yararlı olan kapsüllemeleri içerir.

Terraform modülleri oluştururken kalite güvencesi uygulamak önemlidir. Ne yazık ki, sınırlı belgeler terraform modüllerinde birim testleri ve entegrasyon testleri nasıl yazar açıklamak için kullanılabilir. Bu öğretici, [Azure Terraform modüllerimizi](https://registry.terraform.io/browse?provider=azurerm)oluştururken benimsediğimiz bir test altyapılarını ve en iyi uygulamaları sunar.

En popüler test altyapılarına baktık ve Terraform modüllerimizi test etmek için [Terratest'i](https://github.com/gruntwork-io/terratest) seçtik. Terratest, Go kitaplığı olarak uygulanır. Terratest, http isteklerinde bulunmak ve belirli bir sanal makineye erişmek için SSH'yi kullanmak gibi ortak altyapı test görevleri için yardımcı işlevler ve desenler koleksiyonu sağlar. Aşağıdaki liste Terratest kullanmanın bazı önemli avantajlarını açıklar:

- **Altyapıyı kontrol etmek için uygun yardımcılar sağlar.** Bu özellik, gerçek altyapınızı gerçek ortamda doğrulamak istediğiniz zamanlar işinize yarar.
- **Klasör yapısı açıkça düzenlenmiştir.** Test çalışmalarınız net bir şekilde düzenlenir ve standart Terraform modülü klasör yapısını takip [edin.](https://www.terraform.io/docs/modules/create.html#standard-module-structure)
- **Tüm test örnekleri Go'da yazılır.** Terraform kullanan geliştiricilerin çoğu Go geliştiricileridir. Go geliştiricisiyseniz, Terratest'i kullanmak için başka bir programlama dili öğrenmeniz gerekmez. Ayrıca, Terratest test durumlarda çalıştırmak için gerekli olan tek bağımlılıkgo ve Terraform vardır.
- **Altyapı son derece genişletilebilir.** Azure'a özgü özellikler de dahil olmak üzere Terratest'in üstüne ek işlevler uzatabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Bu uygulamalı makale platformdan bağımsızdır. Bu makalede kullandığımız kod örneklerini Windows, Linux veya MacOS ile ilgili olarak çalıştırabilirsiniz. 

Başlamadan önce aşağıdaki yazılımı yükleyin:

- **Go programlama dili**: Terraform test durumlarda [Go](https://golang.org/dl/)yazılmıştır.
- **dep**: [dep](https://github.com/golang/dep#installation), Go’nun bağımlılık yönetimi aracıdır.
- **Azure CLI**: [Azure CLI,](/cli/azure/install-azure-cli?view=azure-cli-latest) Azure kaynaklarını yönetmek için kullanabileceğiniz bir komut satırı aracıdır. (Terraform, bir hizmet sorumlusu veya [Azure CLI aracılığıyla](https://www.terraform.io/docs/providers/azurerm/authenticating_via_azure_cli.html)Azure'a kimlik doğrulamasını destekler.)
- **mage**: Terratest servis taleplerini çalıştırmayı nasıl basitleştireceğimiz göstermek için [çalıştırılabilir mage'ı](https://github.com/magefile/mage/releases) kullanırız. 

## <a name="create-a-static-webpage-module"></a>Statik bir web sayfası modülü oluşturma

Bu öğreticide, tek bir HTML dosyasını Azure Depolama blob'una yükleyerek statik bir web sayfasını sağlayan bir Terraform modülü oluşturursunuz. Bu modül, dünyanın dört bir yanından gelen kullanıcılara modülün döndürdettiği bir URL aracılığıyla web sayfasına erişim sağlar.

> [!NOTE]
> [GOPATH](https://github.com/golang/go/wiki/SettingGOPATH) konumunuz altında bu bölümde açıklanan tüm dosyaları oluşturun.

İlk olarak, GoPath `staticwebpage` `src` klasörünüzün altında yeni bir klasör oluşturun. Bu öğreticinin genel klasör yapısı aşağıdaki örnekte gösterilmiştir. Yıldız işaretiyle işaretlenmiş `(*)` dosyalar bu bölümdeki birincil odak noktasıdır.

```
 📁 GoPath/src/staticwebpage
   ├ 📁 examples
   │   └ 📁 hello-world
   │       ├ 📄 index.html
   │       └ 📄 main.tf
   ├ 📁 test
   │   ├ 📁 fixtures
   │   │   └ 📁 storage-account-name
   │   │       ├ 📄 empty.html
   │   │       └ 📄 main.tf
   │   ├ 📄 hello_world_example_test.go
   │   └ 📄 storage_account_name_unit_test.go
   ├ 📄 main.tf      (*)
   ├ 📄 outputs.tf   (*)
   └ 📄 variables.tf (*)
```

Statik web sayfası modülü üç girişi kabul eder. Girişler şu şekilde `./variables.tf`bildirilir:

```hcl
variable "location" {
  description = "The Azure region in which to create all resources."
}

variable "website_name" {
  description = "The website name to use to create related resources in Azure."
}

variable "html_path" {
  description = "The file path of the static home page HTML in your local file system."
  default     = "index.html"
}
```

Makalede daha önce de belirttiğimiz gibi, bu modül de beyan `./outputs.tf`edilen bir URL çıktıları:

```hcl
output "homepage_url" {
  value = azurerm_storage_blob.homepage.url
}
```

Modülün ana mantığı dört kaynak sağlar:
- **kaynak grubu**: Kaynak grubunun `website_name` `-staging-rg`adı.
- **depolama hesabı**: Depolama hesabının `website_name` adı, . `data001` Depolama hesabının ad sınırlamalarına uymak için, modül tüm özel karakterleri kaldırır ve tüm depolama hesabı adında küçük harfler kullanır.
- **sabit ad kapsayıcısı**: `wwwroot` Konteyner adı verilir ve depolama hesabında oluşturulur.
- **tek HTML dosyası**: HTML dosyası `html_path` girişten okunur `wwwroot/index.html`ve .

Statik web sayfası modülünün mantığı `./main.tf` içinde uygulanır:

```hcl
resource "azurerm_resource_group" "main" {
  name     = "${var.website_name}-staging-rg"
  location = var.location
}

resource "azurerm_storage_account" "main" {
  name                     = "${lower(replace(var.website_name, "/[[:^alnum:]]/", ""))}data001"
  resource_group_name      = azurerm_resource_group.main.name
  location                 = azurerm_resource_group.main.location
  account_tier             = "Standard"
  account_replication_type = "LRS"
}

resource "azurerm_storage_container" "main" {
  name                  = "wwwroot"
  resource_group_name   = azurerm_resource_group.main.name
  storage_account_name  = azurerm_storage_account.main.name
  container_access_type = "blob"
}

resource "azurerm_storage_blob" "homepage" {
  name                   = "index.html"
  resource_group_name    = azurerm_resource_group.main.name
  storage_account_name   = azurerm_storage_account.main.name
  storage_container_name = azurerm_storage_container.main.name
  source                 = var.html_path
  type                   = "block"
  content_type           = "text/html"
}
```

### <a name="unit-test"></a>Birim testi

Terratest entegrasyon testleri için tasarlanmıştır. Bu amaçla, Terratest gerçek kaynakları gerçek bir ortamda karşılar. Bazen, tümleştirme testi işleri, özellikle de sağlanması gereken çok sayıda kaynağa sahipseniz, son derece büyük olabilir. Önceki bölümde atıfta bulunduğumuz depolama hesabı adlarını dönüştüren mantık iyi bir örnektir. 

Ama, biz gerçekten herhangi bir kaynak sağlamak gerekmez. Yalnızca adlandırma dönüştürme mantığının doğru olduğundan emin olmak istiyoruz. Terratest'in esnekliği sayesinde birim testlerini kullanabiliriz. Birim testleri yerel çalışan test çalışmalarıdır (internet erişimi gerekli olsa da). Birim test `terraform init` örnekleri `terraform plan` yürütmek ve çıkışları ayrıştırmak `terraform plan` ve karşılaştırmak için öznitelik değerlerini aramak için komutları.

Bu bölümün geri kalanında, depolama hesabı adlarını dönüştürmek için kullanılan mantığın doğru olduğundan emin olmak için bir birim testi uygulamak için Terratest'i nasıl kullandığımız açıklanmaktadır. Biz sadece bir yıldız `(*)`işareti ile işaretlenmiş dosyaları ilgileniyoruz.

```
 📁 GoPath/src/staticwebpage
   ├ 📁 examples
   │   └ 📁 hello-world
   │       ├ 📄 index.html
   │       └ 📄 main.tf
   ├ 📁 test
   │   ├ 📁 fixtures
   │   │   └ 📁 storage-account-name
   │   │       ├ 📄 empty.html                (*)
   │   │       └ 📄 main.tf                   (*)
   │   ├ 📄 hello_world_example_test.go
   │   └ 📄 storage_account_name_unit_test.go (*)
   ├ 📄 main.tf
   ├ 📄 outputs.tf
   └ 📄 variables.tf
```

İlk olarak, yer tutucu `./test/fixtures/storage-account-name/empty.html` olarak adlandırılan boş bir HTML dosyası kullanırız.

Dosya `./test/fixtures/storage-account-name/main.tf` test çantası çerçevesidir. Birim testlerinin de `website_name`girişi olan bir girişi kabul eder. Mantık burada gösterilmiştir:

```hcl
variable "website_name" {
  description = "The name of your static website."
}

module "staticwebpage" {
  source       = "../../../"
  location     = "West US"
  website_name = var.website_name
  html_path    = "empty.html"
}
```

Ana bileşeni birim testlerin `./test/storage_account_name_unit_test.go`uygulanmasıdır.

Go geliştiricileri muhtemelen birim testi nin klasik bir Go test işlevinin `*testing.T`imzasıyla eşleşen bir tür argüman kabul ettiğini fark edecektir.

Birim testin gövdesinde, değişken `testCases` (girdi`key` olarak ve `value` beklendiği gibi çıktı) tanımlanan toplam beş olgu vardır. Her birim test çalışması için, önce test fikstürü klasörünü çalıştırıp `terraform init` hedefliyoruz (`./test/fixtures/storage-account-name/`). 

Ardından, `terraform plan` belirli test çalışması girişi kullanan bir komut `website_name` (tanıma `tfOptions`bir göz atın) sonucu kaydeder `./test/fixtures/storage-account-name/terraform.tfplan` (genel klasör yapısında listelenmez).

Bu sonuç dosyası, resmi Terraform plan parer kullanılarak kod okunabilir bir yapıya ayrıştırılır.

Şimdi, ilgilendiğimiz özellikleri (bu durumda, `name` `azurerm_storage_account`) arıyoruz ve sonuçları beklenen çıktıyla karşılaştırıyoruz:

```go
package test

import (
    "os"
    "path"
    "testing"

    "github.com/gruntwork-io/terratest/modules/terraform"
    terraformCore "github.com/hashicorp/terraform/terraform"
)

func TestUT_StorageAccountName(t *testing.T) {
    t.Parallel()

    // Test cases for storage account name conversion logic
    testCases := map[string]string{
        "TestWebsiteName": "testwebsitenamedata001",
        "ALLCAPS":         "allcapsdata001",
        "S_p-e(c)i.a_l":   "specialdata001",
        "A1phaNum321":     "a1phanum321data001",
        "E5e-y7h_ng":      "e5ey7hngdata001",
    }

    for input, expected := range testCases {
        // Specify the test case folder and "-var" options
        tfOptions := &terraform.Options{
            TerraformDir: "./fixtures/storage-account-name",
            Vars: map[string]interface{}{
                "website_name": input,
            },
        }

        // Terraform init and plan only
        tfPlanOutput := "terraform.tfplan"
        terraform.Init(t, tfOptions)
        terraform.RunTerraformCommand(t, tfOptions, terraform.FormatArgs(tfOptions, "plan", "-out="+tfPlanOutput)...)

        // Read and parse the plan output
        f, err := os.Open(path.Join(tfOptions.TerraformDir, tfPlanOutput))
        if err != nil {
            t.Fatal(err)
        }
        defer f.Close()
        plan, err := terraformCore.ReadPlan(f)
        if err != nil {
            t.Fatal(err)
        }

        // Validate the test result
        for _, mod := range plan.Diff.Modules {
            if len(mod.Path) == 2 && mod.Path[0] == "root" && mod.Path[1] == "staticwebpage" {
                actual := mod.Resources["azurerm_storage_account.main"].Attributes["name"].New
                if actual != expected {
                    t.Fatalf("Expect %v, but found %v", expected, actual)
                }
            }
        }
    }
}
```

Birim testlerini çalıştırmak için komut satırında aşağıdaki adımları tamamlayın:

```azurecli
az login    # Required when no service principal environment variables are present
```

```shell
cd [Your GoPath]/src/staticwebpage
dep init    # Run only once for this folder
dep ensure  # Required to run if you imported new packages in test cases
cd test
go fmt
go test -run TestUT_StorageAccountName
```

Geleneksel Go test sonucu yaklaşık bir dakika içinde döner.

### <a name="integration-test"></a>Tümleştirme testi

Birim testlerinin aksine, tümleştirme testlerinin uçuça perspektif için kaynakları gerçek bir ortama sağlaması gerekir. Terratest bu tür bir görevde iyi iş yapar. 

Terraform modülleri için en iyi `examples` uygulamalar klasörü yüklemeyi içerir. Klasör `examples` bazı uç-uç örnekleri içerir. Gerçek verilerle çalışmaktan kaçınmak için, neden bu örnekleri tümleştirme testleri olarak test etmiyoruz? Bu bölümde, aşağıdaki klasör `(*)` yapısında yıldız işaretiyle işaretlenmiş üç dosyaya odaklanırız:

```
 📁 GoPath/src/staticwebpage
   ├ 📁 examples
   │   └ 📁 hello-world
   │       ├ 📄 index.html              (*)
   │       └ 📄 main.tf                 (*)
   ├ 📁 test
   │   ├ 📁 fixtures
   │   │   └ 📁 storage-account-name
   │   │       ├ 📄 empty.html
   │   │       └ 📄 main.tf
   │   ├ 📄 hello_world_example_test.go (*)
   │   └ 📄 storage_account_name_unit_test.go
   ├ 📄 main.tf
   ├ 📄 outputs.tf
   └ 📄 variables.tf
```

Örneklerle başlayalım. Klasörde yeni bir örnek klasör oluşturulur. `hello-world/` `./examples/` Burada, yüklenecek basit bir HTML sayfası `./examples/hello-world/index.html`salıyoruz: .

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>Hello World</title>
</head>
<body>
    <h1>Hi, Terraform Module</h1>
    <p>This is a sample web page to demonstrate Terratest.</p>
</body>
</html>
```

Terraform örneği `./examples/hello-world/main.tf` birim testinde gösterilene benzer. Önemli bir fark vardır: örnek de yüklenen HTML URL adlı `homepage`bir web sayfası olarak yazdırır.

```hcl
variable "website_name" {
  description = "The name of your static website."
  default     = "Hello-World"
}

module "staticwebpage" {
  source       = "../../"
  location     = "West US"
  website_name = var.website_name
}

output "homepage" {
  value = module.staticwebpage.homepage_url
}
```

Entegrasyon test dosyasında `./test/hello_world_example_test.go`Terratest ve klasik Go test fonksiyonlarını tekrar kullanıyoruz.

Birim testlerinden farklı olarak, tümleştirme testleri Azure'da gerçek kaynaklar oluşturur. Bu nedenle çakışmaları adlandırmaktan kaçınmak için dikkatli olmanız gerekir. (Depolama hesabı adları gibi bazı dünya çapında benzersiz adlara özel dikkat edin.) Bu nedenle, test mantığının ilk adımı Terratest tarafından `UniqueId()` sağlanan işlevi kullanarak randomize `websiteName` oluşturmaktır. Bu işlev, küçük harfler, büyük harfler veya sayılar içeren rasgele bir ad oluşturur. `tfOptions``./examples/hello-world/` klasörü hedefleyen tüm Terraform komutlarını yapar. Ayrıca randomize `websiteName` `website_name` olarak ayarlandıklarını da sağlar.

Ardından birer birer `terraform init`, `terraform apply` ve `terraform output` yürütülür. Terratest tarafından sağlanan `HttpGetWithCustomValidation()`başka bir yardımcı işlevi kullanıyoruz. HTML'nin döndürülen çıktı `homepage` URL'sine yüklendiğinden emin olmak için `terraform output`yardımcı işlevini kullanırız. HTTP GET durum kodunu `200` HTML içeriğiyle karşılaştırıp bazı anahtar kelimeler arıyoruz. Son olarak, Go'nun `defer` özelliğinden yararlanılarak `terraform destroy` komutunun yürütülmesi "taahhüt edildi".

```go
package test

import (
    "fmt"
    "strings"
    "testing"

    "github.com/gruntwork-io/terratest/modules/http-helper"
    "github.com/gruntwork-io/terratest/modules/random"
    "github.com/gruntwork-io/terratest/modules/terraform"
)

func TestIT_HelloWorldExample(t *testing.T) {
    t.Parallel()

    // Generate a random website name to prevent a naming conflict
    uniqueID := random.UniqueId()
    websiteName := fmt.Sprintf("Hello-World-%s", uniqueID)

    // Specify the test case folder and "-var" options
    tfOptions := &terraform.Options{
        TerraformDir: "../examples/hello-world",
        Vars: map[string]interface{}{
            "website_name": websiteName,
        },
    }

    // Terraform init, apply, output, and destroy
    defer terraform.Destroy(t, tfOptions)
    terraform.InitAndApply(t, tfOptions)
    homepage := terraform.Output(t, tfOptions, "homepage")

    // Validate the provisioned webpage
    http_helper.HttpGetWithCustomValidation(t, homepage, func(status int, content string) bool {
        return status == 200 &&
            strings.Contains(content, "Hi, Terraform Module") &&
            strings.Contains(content, "This is a sample web page to demonstrate Terratest.")
    })
}
```

Tümleştirme testlerini çalıştırmak için komut satırında aşağıdaki adımları tamamlayın:

```azurecli
az login    # Required when no service principal environment variables are present
```

```shell
cd [Your GoPath]/src/staticwebpage
dep init    # Run only once for this folder
dep ensure  # Required to run if you imported new packages in test cases
cd test
go fmt
go test -run TestIT_HelloWorldExample
```

Geleneksel Go test sonucu yaklaşık iki dakika içinde döner. Ayrıca, bu komutları çalıştırarak hem birim testleri hem de tümleştirme testlerini çalıştırabilirsiniz:

```shell
go fmt
go test
```

Tümleştirme testleri birim testlerinden çok daha uzun sürer (bir tümleştirme örneği için beş birim olgu için bir dakikaya kıyasla iki dakika). Ancak birim testlerini mi yoksa tümleştirme testlerini mi bir senaryoda kullanmak sizin kararınız. Tipik olarak, Terraform HCL fonksiyonlarını kullanarak karmaşık mantık için birim testleri kullanmayı tercih ediyoruz. Genellikle bir kullanıcının uçtan uca perspektifi için tümleştirme testleri kullanırız.

## <a name="use-mage-to-simplify-running-terratest-cases"></a>Terratest çalışmalarının yürütülmesini basitleştirmek için mage kullanma 
Azure Bulut Su Topunu'nda test servis taleplerini çalıştırmak için çeşitli dizinlerde farklı komutlar yürütülmesi gerekiyor. Bu süreci daha verimli hale getirmek için projemizde yapı sistemini tanıtıyoruz. Bu bölümde, iş için bir Go yapı sistemi, mage kullanın.

Mage tarafından gerekli tek `magefile.go` şey projenizin kök dizininde `(+)` (aşağıdaki örnekte işaretlenmiş):

```
 📁 GoPath/src/staticwebpage
   ├ 📁 examples
   │   └ 📁 hello-world
   │       ├ 📄 index.html
   │       └ 📄 main.tf
   ├ 📁 test
   │   ├ 📁 fixtures
   │   │   └ 📁 storage-account-name
   │   │       ├ 📄 empty.html
   │   │       └ 📄 main.tf
   │   ├ 📄 hello_world_example_test.go
   │   └ 📄 storage_account_name_unit_test.go
   ├ 📄 magefile.go (+)
   ├ 📄 main.tf
   ├ 📄 outputs.tf
   └ 📄 variables.tf
```

İşte bir örnek `./magefile.go`. Go'da yazılan bu yapı komut dosyasında beş yapı adımı uygularız:
- `Clean`: Adım, test yürütmeleri sırasında oluşturulan tüm oluşturulan ve geçici dosyaları kaldırır.
- `Format`: Adım `terraform fmt` çalışır `go fmt` ve kod tabanınızı biçimlendirmek için.
- `Unit`: Adım klasörün `TestUT_*` `./test/` altında tüm birim testleri (işlev adı kuralı kullanarak) çalışır.
- `Integration`: Adım `Unit`benzer , ancak birim testleri yerine, tümleştirme`TestIT_*`testleri yürütür ( ).
- `Full`: Adım `Clean`, `Format` `Unit`, `Integration` , çalışır ve sırayla.

```go
// +build mage

// Build a script to format and run tests of a Terraform module project
package main

import (
    "fmt"
    "os"
    "path/filepath"

    "github.com/magefile/mage/mg"
    "github.com/magefile/mage/sh"
)

// The default target when the command executes `mage` in Cloud Shell
var Default = Full

// A build step that runs Clean, Format, Unit and Integration in sequence
func Full() {
    mg.Deps(Unit)
    mg.Deps(Integration)
}

// A build step that runs unit tests
func Unit() error {
    mg.Deps(Clean)
    mg.Deps(Format)
    fmt.Println("Running unit tests...")
    return sh.RunV("go", "test", "./test/", "-run", "TestUT_", "-v")
}

// A build step that runs integration tests
func Integration() error {
    mg.Deps(Clean)
    mg.Deps(Format)
    fmt.Println("Running integration tests...")
    return sh.RunV("go", "test", "./test/", "-run", "TestIT_", "-v")
}

// A build step that formats both Terraform code and Go code
func Format() error {
    fmt.Println("Formatting...")
    if err := sh.RunV("terraform", "fmt", "."); err != nil {
        return err
    }
    return sh.RunV("go", "fmt", "./test/")
}

// A build step that removes temporary build and test files
func Clean() error {
    fmt.Println("Cleaning...")
    return filepath.Walk(".", func(path string, info os.FileInfo, err error) error {
        if err != nil {
            return err
        }
        if info.IsDir() && info.Name() == "vendor" {
            return filepath.SkipDir
        }
        if info.IsDir() && info.Name() == ".terraform" {
            os.RemoveAll(path)
            fmt.Printf("Removed \"%v\"\n", path)
            return filepath.SkipDir
        }
        if !info.IsDir() && (info.Name() == "terraform.tfstate" ||
            info.Name() == "terraform.tfplan" ||
            info.Name() == "terraform.tfstate.backup") {
            os.Remove(path)
            fmt.Printf("Removed \"%v\"\n", path)
        }
        return nil
    })
}
```

Tam bir test paketini yürütmek için aşağıdaki komutları kullanabilirsiniz. Kod, önceki bir bölümde kullandığımız çalışan adımlara benzer. 

```azurecli
az login    # Required when no service principal environment variables are present
```

```shell
cd [Your GoPath]/src/staticwebpage
dep init    # Run only once for this folder
dep ensure  # Required to run if you imported new packages in magefile or test cases
go fmt      # Only required when you change the magefile
mage
```

Son komut satırını ek mage adımlarıile değiştirebilirsiniz. Örneğin, kullanabilirsiniz `mage unit` veya `mage clean`. `dep` Komutları ve `az login` magefileyi yerleştirmek iyi bir fikirdir. Burada kodu göstermiyoruz. 

Mage ile, Go paket sistemini kullanarak adımları da paylaşabilirsiniz. Bu durumda, yalnızca ortak bir uygulamaya başvurarak ve bağımlılıkları beyan ederek tüm modüllerinizdeki magedosyalarını basitleştirebilirsiniz (`mg.Deps()`).

**İsteğe bağlı: Kabul testlerini çalıştırmak için hizmet temel ortam değişkenlerini ayarlama**
 
Sınamalardan `az login` önce yürütme yerine, hizmet temel ortamı değişkenlerini ayarlayarak Azure kimlik doğrulamasını tamamlayabilirsiniz. Terraform çevre [değişken adlarının](https://www.terraform.io/docs/providers/azurerm/index.html#testing)bir listesini yayımlar. (Bu ortam değişkenlerinden yalnızca ilk dördü gereklidir.) Terraform ayrıca [bu ortam değişkenlerinin değerini](https://www.terraform.io/docs/providers/azurerm/authenticating_via_service_principal.html)nasıl elde edilebildiğini açıklayan ayrıntılı yönergeler yayımlar.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"] 
> [Terratest GitHub sayfası](https://github.com/gruntwork-io/terratest).