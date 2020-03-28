---
title: Bir PyTorch modelini Azure İşlevleri uygulaması olarak dağıtma
description: Bir görüntüye 1000 ImageNet etiketinden 1'ini atamak için PyTorch'tan Azure Fonksiyonları ile önceden eğitilmiş bir ResNet 18 derin sinir ağı kullanın.
author: gvashishtha
ms.topic: tutorial
ms.date: 02/28/2020
ms.author: gopalv
ms.openlocfilehash: 17acb7e351d5f1c009a6a8a14717e987fae3e895
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "78379902"
---
# <a name="tutorial-deploy-a-pre-trained-image-classification-model-to-azure-functions-with-pytorch"></a>Öğretici: PyTorch ile Azure Fonksiyonlarına önceden eğitilmiş bir görüntü sınıflandırma modeli dağıtma

Bu makalede, bir görüntüyü içeriğine göre sınıflandırmak için önceden eğitilmiş bir modeli yüklemek için Python, PyTorch ve Azure Işlevleri'ni nasıl kullanacağınızı öğrenirsiniz. Tüm çalışmaları yerel olarak yaptığınız ve bulutta Azure kaynağı oluşturmadığınız için, bu öğreticiyi tamamlamak için hiçbir ücret alınmaz.

> [!div class="checklist"]
> * Python'da Azure İşlevleri geliştirmek için yerel bir ortamı niçin başlatma.
> * Önceden eğitilmiş bir PyTorch makine öğrenme modelini bir işlev uygulamasına aktarın.
> * Görüntüyü 1000 ImageNet [sınıfından](https://gist.github.com/yrevar/942d3a0ac09ec9e5eb3a)biri olarak sınıflandırmak için sunucusuz bir HTTP API oluşturun.
> * Bir web uygulamasından API'yi tüketin.

## <a name="prerequisites"></a>Ön koşullar

- Etkin bir aboneliği olan bir Azure hesabı. [Ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
- [Python 3.7.4 veya üzeri.](https://www.python.org/downloads/release/python-374/) (Python 3.8.x ve Python 3.6.x de Azure Fonksiyonları ile doğrulanır.)
- [Azure İşletme Temel Araçları](functions-run-local.md#install-the-azure-functions-core-tools)
- Visual Studio Code gibi bir kod [düzenleyicisi](https://code.visualstudio.com/)

### <a name="prerequisite-check"></a>Ön koşul kontrolü

1. Bir terminal veya komut `func --version` penceresinde, Azure İşlevler Temel Araçları'nın sürüm 2.7.1846 veya sonraki sürüm olup olmadığını denetlemek için çalıştırın.
1. Python `python --version` sürüm raporlarınızı 3.7.x olarak denetlemek için (Linux/MacOS) veya `py --version` (Windows) çalıştırın.

## <a name="clone-the-tutorial-repository"></a>Öğretici depoyu klonlama

1. Bir terminal veya komut penceresinde, Git'i kullanarak aşağıdaki depoyu kopyala:

    ```
    git clone https://github.com/Azure-Samples/functions-python-pytorch-tutorial.git
    ```

1. Klasöre gidin ve içeriğini inceleyin.

    ```
    cd functions-python-pytorch-tutorial
    ```

    - *başlangıç* öğretici için çalışma klasörüdür.
    - *sonu,* başvurunuz için nihai sonuç ve tam uygulamadır.
    - *kaynaklar* makine öğrenme modelini ve yardımcı kitaplıklarını içerir.
    - *frontend* işlev uygulaması çağıran bir web sitesidir.

## <a name="create-and-activate-a-python-virtual-environment"></a>Python sanal ortamı oluşturma ve etkinleştirme

*Başlat* klasörüne gidin ve aşağıdaki komutları çalıştırArak adlı `.venv`sanal bir ortam oluşturun ve etkinleştirin.


# <a name="bash"></a>[bash](#tab/bash)

```bash
cd start
python -m venv .venv
source .venv/bin/activate
```

Python venv paketini Linux dağıtımınıza yüklemediyse aşağıdaki komutu çalıştırın:

```bash
sudo apt-get install python3-venv
```

# <a name="powershell"></a>[Powershell](#tab/powershell)

```powershell
cd start
py -m venv .venv
.venv\scripts\activate
```

# <a name="cmd"></a>[Cmd](#tab/cmd)

```cmd
cd start
py -m venv .venv
.venv\scripts\activate
```

---

Bu etkinleştirilen sanal ortamda sonraki tüm komutları çalıştırın. (Sanal ortamdan çıkmak `deactivate`için çalıştırın.)


## <a name="create-a-local-functions-project"></a>Yerel işlevler projesi oluşturma

Azure İşlevlerinde işlev projesi, her biri belirli bir tetikleyiciye yanıt veren bir veya daha fazla ayrı işlev için bir kapsayıcıdır. Projedeki tüm işlevler aynı yerel ve barındırma yapılandırmalarını paylaşır. Bu bölümde, http bitiş noktası sağlayan adlandırılmış `classify` tek bir kazan işlevi içeren bir işlev projesi oluşturursunuz. Daha sonraki bir bölüme daha spesifik kod eklersiniz.

1. *Başlangıç* klasöründe, Python işlev uygulamasını başlatmak için Azure İşlevler Temel Araçları'nı kullanın:

    ```
    func init --worker-runtime python
    ```

    Başlatma dan sonra, *başlat* klasörü [local.settings.json](functions-run-local.md#local-settings-file) ve [host.json](functions-host-json.md)adlı yapılandırmadosyaları da dahil olmak üzere proje için çeşitli dosyalar içerir. *local.settings.json* Azure'dan indirilen sırları içerebildiği *için,..gitignore* dosyasında varsayılan olarak dosya kaynak denetiminden çıkarılır.

    > [!TIP]
    > Bir işlev projesi belirli bir çalışma süresine bağlı olduğundan, projedeki tüm işlevlerin aynı dille yazılması gerekir.

1. `--name` Bağımsız değişkenin işlevinizin benzersiz adı olduğu ve `--template` bağımsız değişkenin işlevin tetikleyicisini belirttiği aşağıdaki komutu kullanarak projenize bir işlev ekleyin. `func new`projenin seçilen diline uygun bir kod dosyası ve *function.json*adlı yapılandırma dosyası içeren işlev adı ile eşleşen bir alt klasör oluşturun.

    ```
    func new --name classify --template "HTTP trigger"
    ```

    Bu komut, işlevin adı eşleşen bir klasör oluşturur, *sınıflandırmak.* Bu klasörde iki * \_ \_\_\_* dosya vardır: işlev kodunu içeren init .py ve *işlev.json*, işlevin tetikleyici ve giriş ve çıkış bağlamaları açıklar. Bu dosyaların içeriği hakkında ayrıntılı bilgi için python quickstart'taki [dosya içeriğini incele](/azure/azure-functions/functions-create-first-azure-function-azure-cli?pivots=programming-language-python#optional-examine-the-file-contents) bölümüne bakın.


## <a name="run-the-function-locally"></a>İşlevi yerel olarak çalıştırma

1. Başlat klasöründe yerel Azure İşlevleri çalışma zamanı ana bilgisayarını başlatarak işlevi *başlatın:*

    ```
    func start
    ```

1. Çıktıda bitiş `classify` noktasının göründüğünü gördüğünüzde URL'ye ```http://localhost:7071/api/classify?name=Azure```gidin. "Merhaba Azure!" mesajı çıktıda görünmelidir.

1. Ana bilgisayarı durdurmak için **Ctrl**-**C'yi** kullanın.


## <a name="import-the-pytorch-model-and-add-helper-code"></a>PyTorch modelini içe aktarın ve yardımcı kodu ekleyin

Bir görüntüyü `classify` içeriğine göre sınıflandırmak için işlevi değiştirmek için önceden eğitilmiş bir [ResNet](https://arxiv.org/abs/1512.03385) modeli kullanırsınız. [PyTorch](https://pytorch.org/hub/pytorch_vision_resnet/)gelen önceden eğitilmiş model, 1000 [ImageNet sınıfları](https://gist.github.com/yrevar/942d3a0ac09ec9e5eb3a)1 içine bir görüntü sınıflar. Daha sonra projenize bazı yardımcı kodlar ve bağımlılıklar eklersiniz.

1. *Başlangıç* klasöründe, tahmin kodunu ve etiketleri sınıflandırma klasörüne kopyalamak için aşağıdaki komutu *çalıştırın.*

    # <a name="bash"></a>[bash](#tab/bash)

    ```bash
    cp ../resources/predict.py classify
    cp ../resources/labels.txt classify
    ```

    # <a name="powershell"></a>[Powershell](#tab/powershell)

    ```powershell
    copy ..\resources\predict.py classify
    copy ..\resources\labels.txt classify
    ```

    # <a name="cmd"></a>[Cmd](#tab/cmd)

    ```cmd
    copy ..\resources\predict.py classify
    copy ..\resources\labels.txt classify
    ```

    ---

1. *Sınıflandırma* klasöründe *predict.py* ve *labels.txt*adlı dosyalar içerdiğini doğrulayın. Değilse, *komutu başlangıç* klasöründe çalıştırdığınızdan denetleyin.

1. Bir metin düzenleyicisinde *start/requirements.txt'yi* açın ve yardımcı kodun gerektirdiği bağımlılıkları ekleyin ve aşağıdakigibi görünmelidir:

    ```txt
    azure-functions
    requests
    numpy==1.15.4
    https://download.pytorch.org/whl/cpu/torch-1.4.0%2Bcpu-cp36-cp36m-win_amd64.whl; sys_platform == 'win32' and python_version == '3.6'
    https://download.pytorch.org/whl/cpu/torch-1.4.0%2Bcpu-cp36-cp36m-linux_x86_64.whl; sys_platform == 'linux' and python_version == '3.6'
    https://download.pytorch.org/whl/cpu/torch-1.4.0%2Bcpu-cp37-cp37m-win_amd64.whl; sys_platform == 'win32' and python_version == '3.7'
    https://download.pytorch.org/whl/cpu/torch-1.4.0%2Bcpu-cp37-cp37m-linux_x86_64.whl; sys_platform == 'linux' and python_version == '3.7'
    https://download.pytorch.org/whl/cpu/torch-1.4.0%2Bcpu-cp38-cp38-win_amd64.whl; sys_platform == 'win32' and python_version == '3.8'
    https://download.pytorch.org/whl/cpu/torch-1.4.0%2Bcpu-cp38-cp38-linux_x86_64.whl; sys_platform == 'linux' and python_version == '3.8'
    torchvision==0.5.0
    ```

1. *Requirements.txt*kaydet, ardından bağımlılıkları yüklemek için *başlat* klasöründen aşağıdaki komutu çalıştırın.


    ```
    pip install --no-cache-dir -r requirements.txt
    ```

Yükleme birkaç dakika sürebilir ve bu süre zarfında bir sonraki bölümdeki işlevi değiştirmeye devam edebilirsiniz.
> [!TIP]
> >Windows'da, "EnvironmentError nedeniyle paketleri yükleyemediniz: [Errno 2] Böyle bir dosya veya dizin yok:" hatasıyla karşılaşabilirsiniz: *"sharded_mutable_dense_hashtable.cpython-37.pyc*gibi bir dosyaya uzun bir yol adı ve ardından . Genellikle, klasör yolunun derinliği çok uzun olduğundan bu hata olur. Bu durumda, uzun yolları `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\FileSystem@LongPathsEnabled` etkinleştirmek için `1` kayıt defteri anahtarını ayarlayın. Alternatif olarak, Python yorumcünüzün nerede yüklü olduğunu kontrol edin. Bu konumun uzun bir yolu varsa, daha kısa bir yolu olan bir klasörde yeniden yüklemeyi deneyin.

## <a name="update-the-function-to-run-predictions"></a>Tahminleri çalıştırmak için işlevi güncelleştirme

1. Bir metin düzenleyicisinde *.py'yi sınıflat/\_\_\_\_init* ve standart JSON kitaplığını ve *tahmin* yardımcılarını almak için varolan `import` ifadelerden sonra aşağıdaki satırları ekleyin:

    :::code language="python" source="~/functions-pytorch/end/classify/__init__.py" range="1-6" highlight="5-6":::

1. İşlevin `main` tüm içeriğini aşağıdaki kodla değiştirin:

    :::code language="python" source="~/functions-pytorch/end/classify/__init__.py" range="8-19":::

    Bu işlev, bir sorgu dize parametresi adlı `img`bir görüntü URL alır. Daha sonra `predict_image_from_url` pyTorch modelini kullanarak görüntüyü indirmek ve sınıflandırmak için yardımcı kitaplığından çağırır. İşlev daha sonra sonuçları ile bir HTTP yanıtı döndürür.

    > [!IMPORTANT]
    > Bu HTTP bitiş noktası başka bir etki alanında barındırılan `Access-Control-Allow-Origin` bir web sayfası tarafından çağrıldığı için, yanıt tarayıcının Çapraz Kaynak Paylaşımı (CORS) gereksinimlerini karşılamak için bir üstbilgi içerir.
    >
    > Bir üretim uygulamasında, `*` ek güvenlik için web sayfasının özel kaynağına değiştirin.

1. Değişikliklerinizi kaydedin, ardından bağımlılıkların yüklemeyi tamamladığını varsayarak, yerel işlev ana bilgisayarını `func start`yeniden . Sanal ortam etkinleştirilmiş *başlat* klasöründe ana bilgisayarı çalıştırdığından emin olun. Aksi takdirde ana bilgisayar başlar, ancak işlevi çalıştırırken hatalar görürsünüz.

    ```
    func start
    ```

1. Bir tarayıcıda, Bernese Mountain Dog görüntüsünün URL'si ile işlevi çağırmak için aşağıdaki URL'yi açın ve döndürülen JSON'un görüntüyü Berndağ Köpeği olarak sınıflandırdığını onaylayın.

    ```
    http://localhost:7071/api/classify?img=https://raw.githubusercontent.com/Azure-Samples/functions-python-pytorch-tutorial/master/resources/assets/Bernese-Mountain-Dog-Temperament-long.jpg
    ```

1. Bir sonraki adımda kullandığınız için ana bilgisayarı çalışır durumda tutun.

### <a name="run-the-local-web-app-front-end-to-test-the-function"></a>İşlevi test etmek için yerel web uygulaması ön ucunu çalıştırma

Başka bir web uygulamasından işlev bitiş noktasını çağırmak için deponun *ön uç* klasöründe basit bir uygulama bulunur.

1. Yeni bir terminal veya komut istemi açın ve sanal ortamı etkinleştirin (daha önce [Python sanal ortamı oluştur ve etkinleştir)](#create-and-activate-a-python-virtual-environment)altında açıklandığı gibi).

1. Deponun *ön uç* klasörüne gidin.

1. Python ile bir HTTP sunucusu başlatın:

    # <a name="bash"></a>[bash](#tab/bash)

    ```bash
    python -m http.server
    ```

    # <a name="powershell"></a>[Powershell](#tab/powershell)

    ```powershell
    py -m http.server
    ```

    # <a name="cmd"></a>[Cmd](#tab/cmd)

    ```cmd
    py -m http.server
    ```

1. Tarayıcıda, metin `localhost:8000`kutusuna aşağıdaki fotoğraf URL'lerinden birini girin veya herkese açık herhangi bir resmin URL'sini kullanın.

    - `https://raw.githubusercontent.com/Azure-Samples/functions-python-pytorch-tutorial/master/resources/assets/Bernese-Mountain-Dog-Temperament-long.jpg`
    - `https://github.com/Azure-Samples/functions-python-pytorch-tutorial/blob/master/resources/assets/bald-eagle.jpg?raw=true`
    - `https://raw.githubusercontent.com/Azure-Samples/functions-python-pytorch-tutorial/master/resources/assets/penguin.jpg`

1. Görüntüyü sınıflandırmak için işlev bitiş noktasını çağırmak için **Gönder'i** seçin.

    ![Tamamlanan projenin ekran görüntüsü](media/machine-learning-pytorch/screenshot.png)

    Tarayıcı, resim URL'sini gönderdiğinde bir hata bildiriyorsa, işlev uygulamasını çalıştırdığınız terminali kontrol edin. "Hiçbir modül 'PIL' bulunamadı" gibi bir hata görürseniz, daha önce oluşturduğunuz sanal ortamı etkinleştirmeden *başlangıç* klasöründe işlev uygulamasını başlatmış olabilirsiniz. Hataları hala görüyorsanız, `pip install -r requirements.txt` etkinleştirilen sanal ortamla yeniden çalıştırın ve hataları arayın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticinin tamamı makinenizde yerel olarak çalıştığından, temizleyecek Azure kaynakları veya hizmetleri yoktur.

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, PyTorch modelini kullanarak görüntüleri sınıflandırmak için Azure İşlevleriyle birlikte bir HTTP API bitiş noktası oluşturmayı ve özelleştirmeyi öğrendiniz. Ayrıca, bir web uygulamasından API'yi aramayı da öğrendiniz. Azure İşlevleri tarafından sağlanan sunucusuz bilgi işlem modelinde çalışırken, bu öğreticideki teknikleri, herhangi bir karmaşıklıkAP'ı oluşturmak için kullanabilirsiniz.

Ayrıca bkz:

- [Visual Studio Code'u kullanarak işlevi Azure'a dağıtın.](https://code.visualstudio.com/docs/python/tutorial-azure-functions)
- [Azure Fonksiyonları Python Geliştirici Kılavuzu](./functions-reference-python.md)


> [!div class="nextstepaction"]
> [Azure CLI Kılavuzu'nu kullanarak işlevi Azure İşlevlerine dağıtma](./functions-run-local.md#publish)
