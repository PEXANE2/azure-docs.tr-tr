---
title: PyTorch modelini Azure Işlevleri uygulaması olarak dağıtma
description: Bir yansımaya 1 1000 ImageNet etiketi atamak için Azure Işlevleri ile PyTorch 'ten önceden eğitilen bir sinir ağı kullanın.
author: gvashishtha
ms.topic: tutorial
ms.date: 02/28/2020
ms.author: gopalv
ms.openlocfilehash: 17acb7e351d5f1c009a6a8a14717e987fae3e895
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78379902"
---
# <a name="tutorial-deploy-a-pre-trained-image-classification-model-to-azure-functions-with-pytorch"></a>Öğretici: PyTorch ile Azure Işlevlerine önceden eğitilen bir görüntü sınıflandırma modeli dağıtın

Bu makalede, bir görüntüyü içeriğine göre sınıflandırmak için önceden eğitilen bir model yüklemek üzere Python, PyTorch ve Azure Işlevlerini nasıl kullanacağınızı öğreneceksiniz. Tüm işleri yerel olarak yaptığınız ve bulutta hiçbir Azure kaynağı olmadığınızdan, bu öğreticiyi tamamlamaya yönelik bir maliyet yoktur.

> [!div class="checklist"]
> * Python 'da Azure Işlevleri geliştirmek için yerel bir ortam başlatın.
> * Önceden eğitilen PyTorch makinesi öğrenme modelini bir işlev uygulamasına aktarın.
> * Bir görüntüyü 1000 ImageNet [sınıflarından](https://gist.github.com/yrevar/942d3a0ac09ec9e5eb3a)biri olarak sınıflandırmak için SUNUCUSUZ bir HTTP API 'si oluşturun.
> * API 'YI bir Web uygulamasından tüketme.

## <a name="prerequisites"></a>Önkoşullar

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).
- [Python 3.7.4 veya üzeri](https://www.python.org/downloads/release/python-374/). (Python 3.8. x ve Python 3.6. x de Azure Işlevleri ile doğrulanır.)
- [Azure Functions Core Tools](functions-run-local.md#install-the-azure-functions-core-tools)
- [Visual Studio Code](https://code.visualstudio.com/) gibi bir kod Düzenleyicisi

### <a name="prerequisite-check"></a>Önkoşul denetimi

1. Bir Terminal veya komut penceresinde, Azure Functions Core Tools sürüm 2.7.1846 veya üzeri olduğunu denetlemek için `func --version` çalıştırın.
1. Python sürüm raporlarınızı 3.7. x olarak denetlemek için `python --version` (Linux/MacOS) veya `py --version` (Windows) çalıştırın.

## <a name="clone-the-tutorial-repository"></a>Öğretici deposunu Kopyala

1. Bir Terminal veya komut penceresinde, git kullanarak aşağıdaki depoyu kopyalayın:

    ```
    git clone https://github.com/Azure-Samples/functions-python-pytorch-tutorial.git
    ```

1. Klasöre gidin ve içeriğini inceleyin.

    ```
    cd functions-python-pytorch-tutorial
    ```

    - *Başlangıç* , öğreticinin çalışma klasörüdür.
    - *End* , başvurağınız için nihai sonuç ve tam uygulama.
    - *kaynaklar* makine öğrenimi modelini ve yardımcı kitaplıklarını içerir.
    - *ön uç* , işlev uygulamasını çağıran bir Web sitesidir.

## <a name="create-and-activate-a-python-virtual-environment"></a>Python sanal ortamı oluşturma ve etkinleştirme

*Başlangıç* klasörüne gidin ve `.venv`adlı bir sanal ortam oluşturmak ve etkinleştirmek için aşağıdaki komutları çalıştırın.


# <a name="bash"></a>[Bash](#tab/bash)

```bash
cd start
python -m venv .venv
source .venv/bin/activate
```

Python, venv paketini Linux dağıtımına yüklememediyse aşağıdaki komutu çalıştırın:

```bash
sudo apt-get install python3-venv
```

# <a name="powershell"></a>[PowerShell](#tab/powershell)

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

Bu etkinleştirilmiş sanal ortamda sonraki tüm komutları çalıştırırsınız. (Sanal ortamdan çıkmak için `deactivate`çalıştırın.)


## <a name="create-a-local-functions-project"></a>Yerel işlevler projesi oluşturma

Azure Işlevlerinde bir işlev projesi, her birinin belirli bir tetikleyiciye yanıt verdiği bir veya daha fazla bağımsız işlev için bir kapsayıcıdır. Projedeki tüm işlevler aynı yerel ve barındırma yapılandırmalarına sahiptir. Bu bölümde, HTTP uç noktası sağlayan `classify` adlı tek bir ortak işlevi içeren bir işlev projesi oluşturacaksınız. Daha sonraki bir bölüme daha özel kod eklersiniz.

1. *Başlangıç* klasöründe, bir Python işlev uygulamasını başlatmak için Azure Functions Core Tools kullanın:

    ```
    func init --worker-runtime python
    ```

    Başlatma işleminden sonra *Başlangıç* klasörü, [yerel. Settings. JSON](functions-run-local.md#local-settings-file) ve [Host. JSON](functions-host-json.md)adlı yapılandırma dosyaları dahil olmak üzere proje için çeşitli dosyaları içerir. *Local. Settings. JSON* , Azure 'dan indirilen gizli dizileri içerebildiğinden, dosya, *. gitignore* dosyasında varsayılan olarak kaynak denetiminden çıkarılır.

    > [!TIP]
    > Bir işlev projesi belirli bir çalışma zamanına bağlı olduğundan, projedeki tüm işlevlerin aynı dille yazılması gerekir.

1. Aşağıdaki komutu kullanarak projenize bir işlev ekleyin; burada `--name` bağımsız değişkeni işlevinizin benzersiz adıdır ve `--template` bağımsız değişkeni işlevin tetikleyicisini belirtir. `func new` projenin seçilen diline ve *function. JSON*adlı yapılandırma dosyasına uygun bir kod dosyası içeren işlev adıyla eşleşen bir alt klasör oluşturun.

    ```
    func new --name classify --template "HTTP trigger"
    ```

    Bu komut, işlevin adıyla eşleşen bir klasör oluşturur, *sınıflandır*. Bu klasörde iki dosya vardır: işlev kodunu içeren *\_\_init\_\_.* ger ve işlevin tetikleyicisini ve giriş ve çıkış bağlamalarını açıklayan *function. JSON*. Bu dosyaların içeriğiyle ilgili ayrıntılar için bkz. Python hızlı başlangıçta [Dosya Içeriğini İnceleme](/azure/azure-functions/functions-create-first-azure-function-azure-cli?pivots=programming-language-python#optional-examine-the-file-contents) .


## <a name="run-the-function-locally"></a>İşlevi yerel olarak çalıştırma

1. *Başlangıç* klasöründeki yerel Azure işlevleri çalışma zamanı konağını başlatarak işlevi başlatın:

    ```
    func start
    ```

1. Çıktıda `classify` uç noktası göründüğünü gördüğünüzde URL 'ye gidin, ```http://localhost:7071/api/classify?name=Azure```. "Merhaba Azure!" iletisi çıktıda görünmelidir.

1. Ana bilgisayarı durdurmak için **Ctrl**-**C** kullanın.


## <a name="import-the-pytorch-model-and-add-helper-code"></a>PyTorch modelini içeri aktarın ve yardımcı kodu ekleyin

Bir görüntüyü içeriğine göre sınıflandırmak için `classify` işlevini değiştirmek üzere, önceden eğitilen bir [ResNet](https://arxiv.org/abs/1512.03385) modeli kullanın. [Pytorch](https://pytorch.org/hub/pytorch_vision_resnet/)'den gelen, önceden eğitilen model, bir görüntüyü 1 1000 [imagenet sınıflarında](https://gist.github.com/yrevar/942d3a0ac09ec9e5eb3a)sınıflandırır. Daha sonra projenize bazı yardımcı kod ve bağımlılıklar eklersiniz.

1. *Başlangıç* klasöründe, tahmin kodunu ve etiketleri *sınıflandır* klasörüne kopyalamak için aşağıdaki komutu çalıştırın.

    # <a name="bash"></a>[Bash](#tab/bash)

    ```bash
    cp ../resources/predict.py classify
    cp ../resources/labels.txt classify
    ```

    # <a name="powershell"></a>[PowerShell](#tab/powershell)

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

1. *Sınıflandır* klasörünün *Predict.py* ve *labels. txt*adlı dosyaları içerdiğini doğrulayın. Aksi takdirde, komutu *Başlangıç* klasöründe çalıştırmanızdan emin olun.

1. *Start/requirements. txt* dosyasını bir metin düzenleyicisinde açın ve aşağıdaki gibi görünmesi gereken yardımcı kodu için gereken bağımlılıkları ekleyin:

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

1. *Requirements. txt*dosyasını kaydedin, ardından bağımlılıkları yüklemek için *Başlangıç* klasöründen aşağıdaki komutu çalıştırın.


    ```
    pip install --no-cache-dir -r requirements.txt
    ```

Yükleme işlemi birkaç dakika sürebilir. bu sırada, sonraki bölümde işlevini değiştirmeye devam edebilirsiniz.
> [!TIP]
> >Windows 'da, "bir EnvironmentError nedeniyle paketler yüklenemedi: [errno 2] böyle bir dosya veya dizin yok:" arkasından *sharded_mutable_dense_hashtable. CPython-37. PYC*gibi bir dosyaya uzun bir yol adı gelmelidir. Genellikle, bu hata, klasör yolunun derinliği çok uzun hale geldiği için oluşur. Bu durumda, uzun yolları etkinleştirmek için kayıt defteri anahtarı `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\FileSystem@LongPathsEnabled` `1` olarak ayarlayın. Alternatif olarak, Python yorumlayıcının nereye yüklendiğini kontrol edin. Bu konumda uzun bir yol varsa, daha kısa bir yola sahip bir klasöre yeniden yüklemeyi deneyin.

## <a name="update-the-function-to-run-predictions"></a>Tahminleri çalıştırmak için işlevi güncelleştirin

1. Standart JSON kitaplığı ve *tahmin* eden yardımcıları içeri aktarmak için, bir metin düzenleyicisinde *\_/\_init\_\_. Kopyala* ' yı açın ve var olan `import` deyimlerinden sonra aşağıdaki satırları ekleyin:

    :::code language="python" source="~/functions-pytorch/end/classify/__init__.py" range="1-6" highlight="5-6":::

1. `main` işlevinin tüm içeriğini aşağıdaki kodla değiştirin:

    :::code language="python" source="~/functions-pytorch/end/classify/__init__.py" range="8-19":::

    Bu işlev, `img`adlı bir sorgu dizesi parametresinde bir görüntü URL 'SI alır. Daha sonra, yardım kitaplığından `predict_image_from_url` çağırarak, PyTorch modelini kullanarak görüntüyü indirip sınıflandırın. İşlev daha sonra sonuçlarıyla bir HTTP yanıtı döndürür.

    > [!IMPORTANT]
    > Bu HTTP uç noktası, başka bir etki alanında barındırılan bir Web sayfası tarafından çağrıldığından, yanıt, tarayıcının çıkış noktaları arası kaynak paylaşımı (CORS) gereksinimlerini karşılamak için bir `Access-Control-Allow-Origin` üst bilgisi içerir.
    >
    > Bir üretim uygulamasında, ek güvenlik için `*` Web sayfasının belirli bir kaynağına değiştirin.

1. Yaptığınız değişiklikleri kaydedin, sonra bağımlılıkların yükleme tamamlandığını varsayarak, yerel işlev konağını `func start`yeniden başlatın. Konağı, sanal ortamın etkinleştirildiği *Başlangıç* klasöründe çalıştırdığınızdan emin olun. Aksi takdirde konak başlar, ancak işlevi çağırırken hata görürsünüz.

    ```
    func start
    ```

1. Bir tarayıcıda, bir Bernete Sıradağlar köpek resminin URL 'SI ile işlevi çağırmak için aşağıdaki URL 'yi açın ve döndürülen JSON 'ın görüntüyü Bernete Sıradağlar köpek olarak sınıflandırdığından emin olun.

    ```
    http://localhost:7071/api/classify?img=https://raw.githubusercontent.com/Azure-Samples/functions-python-pytorch-tutorial/master/resources/assets/Bernese-Mountain-Dog-Temperament-long.jpg
    ```

1. Bir sonraki adımda kullandığınız için Konağı çalışır durumda tutun.

### <a name="run-the-local-web-app-front-end-to-test-the-function"></a>İşlevi test etmek için yerel Web uygulaması ön ucu 'nı çalıştırın

İşlev uç noktasının başka bir Web uygulamasından çağrılması test etmek için deponun *ön uç* klasöründe basit bir uygulama vardır.

1. Yeni bir Terminal veya komut istemi açın ve sanal ortamı etkinleştirin (daha önce [bir Python sanal ortamı oluşturma ve etkinleştirme](#create-and-activate-a-python-virtual-environment)altında açıklandığı gibi).

1. Deponun *ön uç* klasörüne gidin.

1. Python ile HTTP sunucusu başlatma:

    # <a name="bash"></a>[Bash](#tab/bash)

    ```bash
    python -m http.server
    ```

    # <a name="powershell"></a>[PowerShell](#tab/powershell)

    ```powershell
    py -m http.server
    ```

    # <a name="cmd"></a>[Cmd](#tab/cmd)

    ```cmd
    py -m http.server
    ```

1. Bir tarayıcıda `localhost:8000`' a gidin, ardından metin kutusuna aşağıdaki fotoğraf URL 'Lerinden birini girin veya herkese açık olarak erişilebilen herhangi bir görüntünün URL 'sini kullanın.

    - `https://raw.githubusercontent.com/Azure-Samples/functions-python-pytorch-tutorial/master/resources/assets/Bernese-Mountain-Dog-Temperament-long.jpg`
    - `https://github.com/Azure-Samples/functions-python-pytorch-tutorial/blob/master/resources/assets/bald-eagle.jpg?raw=true`
    - `https://raw.githubusercontent.com/Azure-Samples/functions-python-pytorch-tutorial/master/resources/assets/penguin.jpg`

1. Görüntüyü sınıflandırmak için işlev uç noktasını çağırmak üzere **Gönder** ' i seçin.

    ![Tamamlanmış projenin ekran görüntüsü](media/machine-learning-pytorch/screenshot.png)

    Görüntü URL 'sini gönderdiğinizde tarayıcıda bir hata bildirirse, işlev uygulamasını çalıştırmakta olduğunuz terminali kontrol edin. "Modül bulunamadı" PIL ' "gibi bir hata görürseniz, önce daha önce oluşturduğunuz sanal ortamı etkinleştirmeden önce, işlev uygulamasını *Başlangıç* klasöründe başlatmış olabilirsiniz. Hala hata görüyorsanız, sanal ortam etkinken `pip install -r requirements.txt` yeniden çalıştırın ve hata olup olmadığına bakın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticinin tamamı makinenizde yerel olarak çalıştığından, temizleyebilen hiçbir Azure kaynağı veya hizmeti yoktur.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir HTTP API uç noktası oluşturmayı ve özelleştirmeyi öğrenmiş olursunuz. Ayrıca, API 'nin bir Web uygulamasından nasıl çağrılacağını öğrenirsiniz. Azure Işlevleri tarafından sunulan sunucusuz işlem modelinde çalışırken her türlü karmaşıklığın API 'Lerini oluşturmak için bu öğreticideki teknikleri kullanabilirsiniz.

Ayrıca bkz:

- [Visual Studio Code kullanarak Işlevi Azure 'A dağıtın](https://code.visualstudio.com/docs/python/tutorial-azure-functions).
- [Azure Işlevleri Python Geliştirici Kılavuzu](./functions-reference-python.md)


> [!div class="nextstepaction"]
> [Azure CLı kılavuzunu kullanarak işlevi Azure Işlevleri 'ne dağıtma](./functions-run-local.md#publish)
