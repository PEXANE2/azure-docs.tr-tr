---
title: Öğretici - Yeoman'ı kullanarak Azure'da Terraform temel şablonu oluşturma
description: Yeoman'ı kullanarak Azure'da Terraform temel şablonu oluşturmayı öğrenin.
ms.topic: tutorial
ms.date: 10/26/2019
ms.openlocfilehash: 82c3f5e640789547abb716b55959e1821a61e6d0
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77472154"
---
# <a name="tutorial-create-a-terraform-base-template-in-azure-using-yeoman"></a>Öğretici: Yeoman'ı kullanarak Azure'da Terraform temel şablonu oluşturun

Bu eğitimde, [Terraform](/azure/terraform/) ve [Yeoman](https://yeoman.io/)kombinasyonunu nasıl kullanacağınızı öğreneceksiniz. Terraform, Azure'da altyapı oluşturmak için bir araçtır. Yeoman, Terraform modülleri oluşturmayı kolaylaştırır.

Bu makalede, aşağıdaki görevleri nasıl yapacağınızı öğrenirsiniz:
> [!div class="checklist"]
> * Yeoman modül jeneratörü kullanarak bir temel Terraform şablonu oluşturun.
> * Terraform şablonu iki farklı yöntem kullanarak test edin.
> * Docker dosyasını kullanarak Terraform modüllerini çalıştırın.
> * Terraform modüllerini Azure Bulut BulutU'da yerel olarak çalıştırın.

## <a name="prerequisites"></a>Ön koşullar

- **Azure aboneliği**: Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.
- **Visual Studio Code**: Platformunuz için [Visual Studio Code indirin.](https://code.visualstudio.com/download)
- **Terraform**: Yeoman tarafından oluşturulan modülü çalıştırmak için [Terraform yükleyin.](terraform-install-configure.md)
- **Docker**: Yeoman jeneratörü tarafından oluşturulan modülü çalıştırmak için [Docker'ı yükleyin.](https://www.docker.com/get-started)
- **Git programlama dili**: Yeoman tarafından oluşturulan test örnekleri Go dilini kullanarak kod olarak [Git](https://golang.org/) yükleyin.

>[!NOTE]
>Bu öğreticideki yordamların çoğu komut satırı arabirimini içerir. Açıklanan adımlar tüm işletim sistemleri ve komut satırı araçları için geçerlidir. Örnekler için PowerShell yerel çevre için, Git Bash ise bulut kabuğu ortamı için seçilmiştir.

## <a name="prepare-your-environment"></a>Ortamınızı hazırlama

### <a name="install-nodejs"></a>Node.js yükleme

Terraform'u Cloud Shell'de kullanabilmek için [Node.js](https://nodejs.org/en/download/) 6.0+ sürümünü yüklemeniz gerekir.

>[!NOTE]
>Node.js uygulamasının yüklü olup olmadığını belirlemek için bir terminal penceresi açıp `node --version` yazın.

### <a name="install-yeoman"></a>Yeoman’ı yükleme

Şu komutu çalıştırın:

```bash
npm install -g yo
```

![Yeoman’ı yükleme](media/terraform-vscode-module-generator/ymg-npm-install-yo.png)

### <a name="install-the-yeoman-template-for-terraform-module"></a>Terraform modülü için Yeoman şablonunu yükleyin

Şu komutu çalıştırın:

```bash
npm install -g generator-az-terra-module
```

![generator-az-terra-module yükleyin](media/terraform-vscode-module-generator/ymg-pm-install-generator-module.png)

Yeoman'ın yüklü olduğunu doğrulamak için aşağıdaki komutu çalıştırın:

```bash
yo --version
```

### <a name="create-a-directory-for-the-yeoman-generated-module"></a>Yeoman tarafından oluşturulan modül için bir dizin oluşturma

Yeoman şablonu geçerli dizinde dosyaları oluşturur. Bu nedenle, bir dizin oluşturmanız gerekir.

Bu boş dizinin $GOPATH/src altına yerleştirilmesi gerekir. Bu yol hakkında daha fazla bilgi için [GOPATH'i ayarla](https://github.com/golang/go/wiki/SettingGOPATH)makalesine bakın.

1. Yeni bir dizin oluşturmak için ana dizine gidin.

1. Yer tutucunun yerine aşağıdaki komutu çalıştırın. Bu örnekte, bir dizin adı `GeneratorDocSample` kullanılır.

    ```bash
    mkdir <new-directory-name>
    ```

    ![mkdir](media/terraform-vscode-module-generator/ymg-mkdir-GeneratorDocSample.png)

1. Yeni dizine gidin:

    ```bash
    cd <new-directory-name>
    ```

    ![Yeni dizininize gidin](media/terraform-vscode-module-generator/ymg-cd-GeneratorDocSample.png)

## <a name="create-a-base-module-template"></a>Temel modül şablonu oluşturma

1. Şu komutu çalıştırın:

    ```bash
    yo az-terra-module
    ```

1. Ekrandaki yönergeleri izleyerek aşağıdaki bilgileri sağlayın:

    - **Terraform modülü proje Adı** `doc-sample-module` - Örnek için bir değer kullanılır.

        ![Proje adı](media/terraform-vscode-module-generator/ymg-project-name.png)       


    - **Docker görüntü dosyasını eklemek ister misiniz?** - `y`Girin. Seçerseniz, `n`oluşturulan modül kodu yalnızca yerel modda çalıştırmayı destekler.

        ![Docker görüntü dosyası eklensin mi?](media/terraform-vscode-module-generator/ymg-include-docker-image-file.png) 

1. Oluşturulan dosyaları görüntülemek için dizin içeriğini listeleyin:

    ```bash
    ls
    ```

    ![Oluşturulan dosyaları listeleme](media/terraform-vscode-module-generator/ymg-ls-GeneratorDocSample-files.png)

## <a name="review-the-generated-module-code"></a>Oluşturulan modülü kodunu gözden geçirme

1. Visual Studio Code'u başlatma

1. Menü çubuğundan **Dosya > Klasör Aç**'ı ve oluşturduğunuz klasörü seçin.

    ![Visual Studio Code](media/terraform-vscode-module-generator/ymg-open-in-vscode.png)

Aşağıdaki dosyalar Yeoman modül jeneratörü tarafından oluşturulmuştur. Bu dosyalar ve kullanımları hakkında daha fazla bilgi için [Terraform Modülleri'nde Terratest'e bakın.](https://mseng.visualstudio.com/VSJava/_git/Terraform?path=%2FTerratest%20Introduction.md&version=GBmaster)

- `main.tf`- Adı verilen `random-shuffle`bir modül tanımlar. Giriş bir `string_list`. Çıktı, permütasyon sayısıdır.
- `variables.tf`- Modül tarafından kullanılan giriş ve çıkış değişkenlerini tanımlar.
- `outputs.tf`- Modülün çıkışlarını tanımlar. Burada, bir dahili Terraform modülü tarafından `random_shuffle`döndürülen değerdir.
- `Rakefile`- Yapı adımlarını tanımlar. Bu adımlar şunlardır:
    - `build`- main.tf dosyasının biçimlendirmesini doğrular.
    - `unit`- Oluşturulan modül iskeleti birim testi için kod içermez. Bir birim testi senaryosu belirtmek isterseniz, o kodu burada eklersiniz.
    - `e2e`- Modülün uçtan uca testini çalıştırın.
- `test`
    - Test çalışmaları Go dilinde yazılır.
    - Testteki tüm kodlar uçtan uca testlerdir.
    - Uçtan uca testler altında `fixture`tanımlanan tüm maddeleri sağlamayı dener. Dosyadaki `template_output.go` sonuçlar önceden tanımlanmış beklenen değerlerle karşılaştırılır.
    - `Gopkg.lock`ve `Gopkg.toml`: Bağımlılıkları tanımlar. 

## <a name="test-your-new-terraform-module-using-a-docker-file"></a>Docker dosyanızı kullanarak yeni Terraform modülünüzü test edin

Bu bölümde, Docker dosyası kullanılarak terraform modülünasıl test edileceğini gösterilmektedir.

>[!NOTE]
>Bu örnek modülü yerel olarak çalıştırZ; Azure'da değil.

### <a name="confirm-docker-is-installed-and-running"></a>Docker’ın yüklü ve çalışır durumda olduğunu onaylayın

Bir komut isteminden `docker version` komutunu girin.

![Docker sürümü](media/terraform-vscode-module-generator/ymg-docker-version.png)

Elde edilen çıktı, Docker'ın yüklü olduğunu onaylar.

Docker’ın gerçekten çalışır durumda olduğunu onaylamak için `docker info` girin.

![Docker bilgisi](media/terraform-vscode-module-generator/ymg-docker-info.png)

### <a name="set-up-a-docker-container"></a>Docker kapsayıcısı ayarlama

1. Bir komut isteminden şu komutu girin:

    `docker build --build-arg BUILD_ARM_SUBSCRIPTION_ID= --build-arg BUILD_ARM_CLIENT_ID= --build-arg BUILD_ARM_CLIENT_SECRET= --build-arg BUILD_ARM_TENANT_ID= -t terra-mod-example .`.

    **Başarıyla derlendi** iletisi gösterilir.

    ![Başarılı bir yapıyı gösteren ileti](media/terraform-vscode-module-generator/ymg-successfully-built.png)

1. Komut isteminden, `docker image ls` oluşturulan modülünüzü `terra-mod-example` görmek için girin.

    ![Yeni modülü içeren liste](media/terraform-vscode-module-generator/ymg-repository-results.png)

1. `docker run -it terra-mod-example /bin/sh` yazın. Komutu `docker run` çalıştırdıktan sonra Docker ortamındasınız. Bu noktada, `ls` komutu kullanarak dosyayı keşfedebilirsiniz.

    ![Docker içinde dosya listesi](media/terraform-vscode-module-generator/ymg-list-docker-file.png)

### <a name="build-the-module"></a>Modülü oluşturun

1. Şu komutu çalıştırın:

    ```bash
    bundle install
    ```

1. Şu komutu çalıştırın:

    ```bash
    rake build
    ```

    ![Rake derlemesi](media/terraform-vscode-module-generator/ymg-rake-build.png)

### <a name="run-the-end-to-end-test"></a>Uçuçtesti çalıştır

1. Şu komutu çalıştırın:

    ```bash
    rake e2e
    ```

1. Birkaç dakika sonra **BAŞARILI** iletisi görünür.

    ![BAŞARILI](media/terraform-vscode-module-generator/ymg-pass.png)

1. Testi `exit` tamamlamak ve Docker ortamından çıkmak için girin.

## <a name="use-yeoman-generator-to-create-and-test-a-module-in-cloud-shell"></a>Cloud Shell'de bir modül oluşturmak ve test etmek için Yeoman jeneratörü kullanın

Bu bölümde, Yeoman jeneratör oluşturmak ve Cloud Shell bir modül test etmek için kullanılır. Docker dosyası kullanmak yerine Cloud Shell'i kullanmak işlemi büyük ölçüde kolaylaştırır. Cloud Shell kullanılarak, aşağıdaki ürünlerin tümü önceden yüklenir:

- Node.js
- Yeoman
- Terraform

### <a name="start-a-cloud-shell-session"></a>Bulut Kabuğu oturumunu başlat

1. [Azure portalı](https://portal.azure.com/), [shell.azure.com](https://shell.azure.com)veya [Azure mobil uygulaması](https://azure.microsoft.com/features/azure-portal/mobile-app/)üzerinden bir Azure Bulut BulutU oturumu başlatın.

1. **Azure Bulut BulutU Bulutu'na Hoş Geldiniz** sayfası açılır. **Bash (Linux) seçin.**

    ![Azure Cloud Shell'e hoş geldiniz](media/terraform-vscode-module-generator/ymg-welcome-to-azure-cloud-shell.png)

1. Azure depolama hesabı oluşturmadıysanız aşağıdaki ekran açılır. **Depolama oluştur**'u seçin.

    ![Hiçbir depolama takılı değil](media/terraform-vscode-module-generator/ymg-you-have-no-storage-mounted.png)

1. Azure Cloud Shell, seçtiğiniz kabukta başlatılır ve sizin adınıza oluşturulan bulut sunucusuna ait bilgiler gösterilir.

    ![Bulut sürücünüz oluşturuldu](media/terraform-vscode-module-generator/ymg-your-cloud-drive-has-been-created-in.png)

### <a name="prepare-a-directory-to-hold-your-terraform-module"></a>Terraform modülünüze sahip olmak için bir dizin hazırlayın

1. Bu noktada, Cloud Shell sizin için ortam değişkenlerinde GOPATH'i zaten yapılandırmış olacaktır. Yolu görmek için `go env`girin.

1. $GOPATH dizini oluşturun, biri zaten yoksa: `mkdir ~/go`Girin.

1. $GOPATH dizini içinde bir dizin oluşturun. Bu dizin, bu örnekte oluşturulan farklı proje dizinlerini tutmak için kullanılır. 

    ```bash
    mkdir ~/go/src
    ```

1. Yer tutucunun yerine Terraform modülünüzün yerini tutmak için bir dizin oluşturun. Bu örnekte, bir dizin adı `my-module-name` kullanılır.

    ```bash
    mkdir ~/go/src/<your-module-name>
    ```

1. Modül dizininize gidin: 

    ```bash
    cd ~/go/src/<your-module-name>
    ```

### <a name="create-and-test-your-terraform-module"></a>Terraform modülünüzü oluşturun ve test edin

1. Aşağıdaki komutu çalıştırın ve yönergeleri izleyin. Docker dosyalarını oluşturmak isteyip istemediğiniz `N`sorulduğunda, .

    ```bash
    yo az-terra-module
    ```

1. Bağımlılıkları yüklemek için aşağıdaki komutu çalıştırın:

    ```bash
    bundle install
    ```

1. Modülü oluşturmak için aşağıdaki komutu çalıştırın:

    ```bash
    rake build
    ```

    ![Rake derlemesi](media/terraform-vscode-module-generator/ymg-rake-build.png)

1. Metni çalıştırmak için aşağıdaki komutu çalıştırın:

    ```bash
    rake e2e
    ```

    ![Test-pass sonuçları](media/terraform-vscode-module-generator/ymg-pass.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure Terraform Visual Studio Code uzantısını yükleyin ve kullanın.](/azure/terraform/terraform-vscode-extension)