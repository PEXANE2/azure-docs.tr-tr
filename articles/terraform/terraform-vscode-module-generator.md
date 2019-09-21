---
title: Azure 'da Yeumman kullanarak bir Terrayform temel şablonu oluşturma
description: Azure 'da Yeumman kullanarak bir Terrayform temel şablonu oluşturmayı öğrenin.
services: terraform
ms.service: azure
keywords: terraform, devops, sanal makine, azure, yeoman
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.topic: tutorial
ms.date: 09/20/2019
ms.openlocfilehash: 7a628eb02170346a826cab19498d6fdf40cebddd
ms.sourcegitcommit: f2771ec28b7d2d937eef81223980da8ea1a6a531
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71173143"
---
# <a name="create-a-terraform-base-template-in-azure-using-yeoman"></a>Azure 'da Yeumman kullanarak bir Terrayform temel şablonu oluşturma

[Terraform](https://docs.microsoft.com/azure/terraform/
), Azure'da kolayca altyapı oluşturmanın yolunu sağlar. [Yeoman](https://yeoman.io/), modül geliştiricisinin Terraform modülleri oluşturma işini büyük ölçüde kolaylaştırırken mükemmel bir *en iyi deneyim* çerçevesi sağlar.

Bu makalede, Yeoman modül oluşturucusunu kullanarak temel Terraform şablonu oluşturma hakkında bilgi edineceksiniz. Daha sonra, yeni Teraform şablonunuzu iki farklı yöntem kullanarak test etme hakkında bilgi edineceksiniz:

- Bu makalede oluşturduğunuz bir Docker dosyasını kullanarak Terrayform modülünüzü çalıştırın.
- Azure Cloud Shell ' de Teraform modülünüzü yerel olarak çalıştırın.

## <a name="prerequisites"></a>Önkoşullar

- **Azure aboneliği**: Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.
- **Visual Studio Code**: Yeumman Oluşturucu tarafından oluşturulan dosyaları incelemek için [Visual Studio Code](https://www.bing.com/search?q=visual+studio+code+download&form=EDGSPH&mkt=en-us&httpsmsn=1&refig=dffc817cbc4f4cb4b132a8e702cc19a3&sp=3&ghc=1&qs=LS&pq=visual+studio+code&sk=LS1&sc=8-18&cvid=dffc817cbc4f4cb4b132a8e702cc19a3&cc=US&setlang=en-US) kullanacağız. Ancak, tercih ettiğiniz herhangi bir kod düzenleyiciyi kullanabilirsiniz.
- **Terrayform**: Yeumman tarafından oluşturulan modülü çalıştırmak için [Terrayform](https://docs.microsoft.com/azure/virtual-machines/linux/terraform-install-configure ) yüklemesi gerekir.
- **Docker**: Yeumman Oluşturucu tarafından oluşturulan modülü çalıştırmak için [Docker](https://www.docker.com/get-started) 'ı kullanacağız. (Tercih ederseniz örnek modülü çalıştırmak için Docker yerine Ruby kullanabilirsiniz.)
- **Go programlama dili**: Yeumman tarafından üretilen test çalışmaları go 'da yazıldığı için bir [Go](https://golang.org/) yüklemesi gerekir.

>[!NOTE]
>Bu öğreticideki yordamların birçoğu, komut satırı girişleri içerir. Burada açıklanan adımlar tüm işletim sistemleri ve komut satırı araçları için geçerlidir. Örneklerimizde, PowerShell 'i yerel ortam için ve Cloud Shell ortamı için git Bash 'i kullanmayı seçtik.

## <a name="prepare-your-environment"></a>Ortamınızı hazırlama

### <a name="install-nodejs"></a>Node.js yükleme

Terraform'u Cloud Shell'de kullanabilmek için [Node.js](https://nodejs.org/en/download/) 6.0+ sürümünü yüklemeniz gerekir.

>[!NOTE]
>Node.js uygulamasının yüklü olup olmadığını belirlemek için bir terminal penceresi açıp `node --version` yazın.

### <a name="install-yeoman"></a>Yeoman’ı yükleme

Bir komut isteminden `npm install -g yo` komutunu girin

![Yeoman’ı yükleme](media/terraform-vscode-module-generator/ymg-npm-install-yo.png)

### <a name="install-the-yeoman-template-for-terraform-module"></a>Terraform modülü için Yeoman şablonunu yükleyin

Bir komut isteminden `npm install -g generator-az-terra-module` komutunu girin.

![generator-az-terra-module yükleyin](media/terraform-vscode-module-generator/ymg-pm-install-generator-module.png)

>[!NOTE]
>Yeoman’ın yüklü olduğunu doğrulamak için bir terminal penceresinden `yo --version` komutunu girin.

### <a name="create-an-empty-folder-to-hold-the-yeoman-generated-module"></a>Yeoman tarafından oluşturulan modülü tutmak için boş bir klasör oluşturun

Yeoman şablonu **geçerli dizinde** dosyaları oluşturur. Bu nedenle, bir dizin oluşturmanız gerekir.

>[!Note]
>Bu boş dizinin $GOPATH/src altına yerleştirilmesi gerekir. Bunu yapmak için yönergeleri [burada](https://github.com/golang/go/wiki/SettingGOPATH) bulabilirsiniz.

Bir komut isteminden:

1. Oluşturmak üzere olduğumuz yeni, boş dizini içermesini istediğiniz üst dizine gidin.
1. `mkdir <new-directory-name>` yazın.

    > [!NOTE]
    > Yeni `<new-directory-name>` dizininizin adıyla değiştirin. Biz bu örnekte, yeni dizini `GeneratorDocSample` olarak adlandırdık.

    ![mkdir](media/terraform-vscode-module-generator/ymg-mkdir-GeneratorDocSample.png)

1. `cd <new directory's name>` yazıp **enter** tuşuna basarak yeni dizine gidin.

    ![Yeni dizininize gidin](media/terraform-vscode-module-generator/ymg-cd-GeneratorDocSample.png)

    >[!NOTE]
    >Bu dizinin boş olduğundan emin olmak için `ls` yazın. Bu komutun elde edilen çıktısında listelenmiş hiçbir dosya olmamalıdır.

## <a name="create-a-base-module-template"></a>Temel modül şablonu oluşturma

Bir komut isteminden:

1. `yo az-terra-module` yazın.

1. Ekrandaki yönergeleri izleyerek aşağıdaki bilgileri sağlayın:

    - *Terraform modülü proje adı*

        ![Proje adı](media/terraform-vscode-module-generator/ymg-project-name.png)       

        >[!NOTE]
        >Bu örnekte `doc-sample-module` girdik.

    - *Docker görüntü dosyasını eklemek ister misiniz?*

        ![Docker görüntü dosyası eklensin mi?](media/terraform-vscode-module-generator/ymg-include-docker-image-file.png) 

        >[!NOTE]
        >`y` yazın. **N**seçeneğini belirlerseniz, oluşturulan modül kodu yalnızca yerel modda çalışmayı destekleyecektir.

3. Oluşturulan dosyaları görüntülemek için `ls` girin.

    ![Oluşturulan dosyaları listeleme](media/terraform-vscode-module-generator/ymg-ls-GeneratorDocSample-files.png)

## <a name="review-the-generated-module-code"></a>Oluşturulan modülü kodunu gözden geçirme

1. Visual Studio Code'u başlatma

1. Menü çubuğundan **Dosya > Klasör Aç**'ı ve oluşturduğunuz klasörü seçin.

    ![Visual Studio Code](media/terraform-vscode-module-generator/ymg-open-in-vscode.png)

Yeoman modül oluşturucusu tarafından oluşturulan dosyalardan bazılarına göz atalım.

>[!Note]
>Bu makalede, Yeoman modül oluşturucusu tarafından oluşturulan main.tf, variables.tf ve outputs.tf dosyalarını kullanacağız. Ancak, kendi modüllerinizi oluştururken bu dosyaları Terraform modülünüzün işlevselliğine uyacak şekilde düzenleyebilirsiniz. Bu dosyaların ve kullanımlarının daha ayrıntılı bir tartışması için bkz. [Terraform Modüllerinde Terratest.](https://mseng.visualstudio.com/VSJava/_git/Terraform?path=%2FTerratest%20Introduction.md&version=GBmaster)

### <a name="maintf"></a>main.tf

*random-shuffle* adlı bir modülü tanımlar. Girdi, *string_list* şeklindedir. Çıktı, permütasyon sayısıdır.

### <a name="variablestf"></a>variables.tf

Modül tarafından kullanılan girdi ve çıktı değişkenlerini tanımlar.

### <a name="outputstf"></a>outputs.tf

Modülün çıktısını tanımlar. Burada, yerleşik bir Terraform modülü olan **random_shuffle** tarafından döndürülen değerdir.

### <a name="rakefile"></a>Rakefile

Derleme adımlarını tanımlar. Bu adımlar şunlardır:

- **oluşturma**: Main.tf dosyasının biçimlendirmesini doğrular.
- **birim**: Oluşturulan modül iskelet 'i bir birim testi için kod içermez. Bir birim testi senaryosu belirtmek isterseniz, o kodu burada eklersiniz.
- **e2e**: Modülün uçtan uca testini çalıştırır.

### <a name="test"></a>test

- Test çalışmaları Go dilinde yazılır.
- Testteki tüm kodlar uçtan uca testlerdir.
- Uçtan uca testler **fixture** altında tanımlanan tüm öğeleri sağlamak üzere Terraform kullanır ve sonra **template_output.go** kodundaki çıktıyı önceden tanımlı beklenen değerler ile karşılaştırır.
- **Gopkg. Lock** ve **Gopkg. TOML**: Bağımlılıklarınızı tanımlayın. 

## <a name="test-your-new-terraform-module-using-a-docker-file"></a>Yeni Teraform modülünüzü bir Docker dosyası kullanarak test etme

>[!NOTE]
>Örneğimizde, modülü yerel bir modül olarak çalıştırıyoruz ve Azure’a gerçek anlamda temas etmiyoruz.

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

    ![Başarıyla derlendi](media/terraform-vscode-module-generator/ymg-successfully-built.png)

1. Komut isteminden `docker image ls` komutunu girin.

    Yeni oluşturduğunuz *terra-mod-example* modülünü listede görürsünüz.

    ![Depo sonuçları](media/terraform-vscode-module-generator/ymg-repository-results.png)

    >[!NOTE]
    >Modül adı *terra-mod-example*, yukarıdaki 1. adımda girdiğiniz komutta belirtilmiştir.

1. `docker run -it terra-mod-example /bin/sh` yazın.

    Docker şu anda çalışmaktadır ve `ls` girilerek dosya listelenebilir.

    ![Docker dosyasını listeleme](media/terraform-vscode-module-generator/ymg-list-docker-file.png)

### <a name="build-the-module"></a>Modülü oluşturma

1. `bundle install` yazın.

    **Paket tamamlandı** iletisini bekleyin, ardından sonraki adıma geçin.

1. `rake build` yazın.

    ![Rake derlemesi](media/terraform-vscode-module-generator/ymg-rake-build.png)

### <a name="run-the-end-to-end-test"></a>Uçtan uca testi çalıştırma

1. `rake e2e` yazın.

1. Birkaç dakika sonra **BAŞARILI** iletisi görünür.

    ![BAŞARILI](media/terraform-vscode-module-generator/ymg-pass.png)

1. Uçtan `exit` uca testi tamamlamak ve Docker ortamından çıkmak için girin.

## <a name="use-yeoman-generator-to-create-and-test-a-module-in-cloud-shell"></a>Cloud Shell bir modül oluşturmak ve test etmek için Yeumman oluşturucuyu kullanın

Önceki bölümde, bir Docker dosyası kullanarak bir Teraform modülünü test etme hakkında daha fazla öğrendiniz. Bu bölümde, Cloud Shell bir modül oluşturmak ve test etmek için Yeumman oluşturucuyu kullanacaksınız.

Docker dosyası kullanmak yerine Cloud Shell kullanmak işlemi büyük ölçüde basitleştirir. Cloud Shell kullanma:

- Node. js yüklemeniz gerekmez
- Yeumman yüklemeniz gerekmez
- Terrayform yüklemeniz gerekmez

Bu öğelerin tümü Cloud Shell önceden yüklenir.

### <a name="start-a-cloud-shell-session"></a>Cloud Shell oturumu başlatma

1. [Azure Portal](https://portal.azure.com/), [Shell.Azure.com](https://shell.azure.com)veya [Azure mobil uygulaması](https://azure.microsoft.com/features/azure-portal/mobile-app/)aracılığıyla Azure Cloud Shell bir oturum başlatın.

1. **Azure Cloud Shell hoş geldiniz** sayfası açılır. **Bash (Linux)** seçeneğini belirleyin. (Power Shell desteklenmez.)

    ![Azure Cloud Shell'e hoş geldiniz](media/terraform-vscode-module-generator/ymg-welcome-to-azure-cloud-shell.png)

    >[!NOTE]
    >Bu örnekte Bash (Linux) seçilmiştir.

1. Azure depolama hesabı oluşturmadıysanız aşağıdaki ekran açılır. **Depolama oluştur**'u seçin.

    ![Hiçbir depolama takılı değil](media/terraform-vscode-module-generator/ymg-you-have-no-storage-mounted.png)

1. Azure Cloud Shell, seçtiğiniz kabukta başlatılır ve sizin adınıza oluşturulan bulut sunucusuna ait bilgiler gösterilir.

    ![Bulut sürücünüz oluşturuldu](media/terraform-vscode-module-generator/ymg-your-cloud-drive-has-been-created-in.png)

### <a name="prepare-a-folder-to-hold-your-terraform-module"></a>Bir klasörü Tertuform modülünüzü tutacak şekilde hazırlama

1. Bu noktada, Cloud Shell sizin için ortam değişkeninizdeki GOPATH 'i zaten yapılandırmış olacak. Yolu görmek için girin `go env`.

1. Zaten yoksa $GOPATH klasörünü oluşturun: `mkdir ~/go` yazın.

1. $GOPATH klasörü içinde bir klasör oluşturun: `mkdir ~/go/src` yazın. Bu klasör, bir sonraki adımda oluşturacağınız `<your-module-name>` klasör gibi, oluşturabileceğiniz farklı proje klasörlerini tutmak ve düzenlemek için kullanılacaktır.

1. Teraform modülünüzü tutacak bir klasör oluşturun: `mkdir ~/go/src/<your-module-name>` yazın.

    >[!NOTE]
    >Bu örnekte, klasör adı için `my-module-name` seçtik.

1. Modül klasörünüze gidin: Girmesini`cd ~/go/src/<your-module-name>`

### <a name="create-and-test-your-terraform-module"></a>Terrayform modülünüzü oluşturun ve test edin

1. Sihirbazdaki `yo az-terra-module` yönergeleri girin ve izleyin.

    >[!NOTE]
    >Docker dosyalarını oluşturmak isteyip istemediğiniz sorulduğunda, girmeniz `N`gerekebilir.

1. Bağımlılıkları `bundle install` yüklemek için girin.

    **Paket tamamlandı** iletisini bekleyin, ardından sonraki adıma geçin.

1. Modülünüzü derlemek için yazın `rake build` .

    ![Rake derlemesi](media/terraform-vscode-module-generator/ymg-rake-build.png)

1. Uçtan `rake e2e` uca testi çalıştırmak için girin.

1. Birkaç dakika sonra **BAŞARILI** iletisi görünür.

    ![BAŞARILI](media/terraform-vscode-module-generator/ymg-pass.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure Terrayform Visual Studio Code uzantısını yükleyip kullanın.](https://docs.microsoft.com/azure/terraform/terraform-vscode-extension)
