---
title: PyTorch modelini Azure Işlevleri uygulaması olarak dağıtma
description: Bir yansımaya 1 1000 ImageNet etiketi atamak için Azure Işlevleri ile PyTorch 'ten önceden eğitilen bir sinir ağı kullanın.
author: gvashishtha
ms.topic: tutorial
ms.date: 02/28/2020
ms.author: gopalv
ms.custom: tracking-python
ms.openlocfilehash: bbb784a1245ecc8d6f3d3aee45b729984568fc6c
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86506103"
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

1. Bir Terminal veya komut penceresinde, `func --version` Azure Functions Core Tools sürüm 2.7.1846 veya üzeri olduğunu denetlemek için komutunu çalıştırın.
1. `python --version` `py --version` Python sürüm raporlarınızı 3.7. x olarak denetlemek için (Linux/MacOS) veya (Windows) öğesini çalıştırın.

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

*Başlangıç* klasörüne gidin ve adlı bir sanal ortam oluşturmak ve etkinleştirmek için aşağıdaki komutları çalıştırın `.venv` .


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

Bu etkinleştirilmiş sanal ortamda sonraki tüm komutları çalıştırırsınız. (Sanal ortamdan çıkmak için, öğesini çalıştırın `deactivate` .)


## <a name="create-a-local-functions-project"></a>Yerel işlevler projesi oluşturma

Azure Işlevlerinde bir işlev projesi, her birinin belirli bir tetikleyiciye yanıt verdiği bir veya daha fazla bağımsız işlev için bir kapsayıcıdır. Projedeki tüm işlevler aynı yerel ve barındırma yapılandırmalarına sahiptir. Bu bölümde, `classify` http uç noktası sağlayan adlı tek bir ortak işlevi içeren bir işlev projesi oluşturacaksınız. Daha sonraki bir bölüme daha özel kod eklersiniz.

1. *Başlangıç* klasöründe, bir Python işlev uygulamasını başlatmak için Azure Functions Core Tools kullanın:

    ```
    func init --worker-runtime python
    ```

    Başlatma işleminden sonra *Başlangıç* klasörü, [local.settings.js](functions-run-local.md#local-settings-file) ve [host.js](functions-host-json.md)adlı yapılandırma dosyaları dahil olmak üzere proje için çeşitli dosyalar içerir. *local.settings.json* , Azure 'dan indirilen gizli dizileri içerebildiğinden, dosya, *. gitignore* dosyasında varsayılan olarak kaynak denetiminden çıkarılır.

    > [!TIP]
    > Bir işlev projesi belirli bir çalışma zamanına bağlı olduğundan, projedeki tüm işlevlerin aynı dille yazılması gerekir.

1. Aşağıdaki komutu kullanarak projenize bir işlev ekleyin; burada `--name` bağımsız değişken işlevinizin benzersiz adıdır ve `--template` bağımsız değişken işlevin tetikleyicisini belirtir. `func new`Projenin seçtiği dile uygun bir kod dosyası ve *üzerindefunction.js*adlı bir yapılandırma dosyası içeren işlev adıyla eşleşen bir alt klasör oluşturun.

    ```
    func new --name classify --template "HTTP trigger"
    ```

    Bu komut, işlevin adıyla eşleşen bir klasör oluşturur, *sınıflandır*. Bu klasörde iki dosya vardır: işlev kodunu içeren * \_ \_ init \_ \_ .* ger ve işlevin tetikleyicisini ve giriş ve çıkış bağlamalarını açıklayan *function.js*. Bu dosyaların içeriğiyle ilgili ayrıntılar için bkz. Python hızlı başlangıçta [Dosya Içeriğini İnceleme](./functions-create-first-azure-function-azure-cli.md?pivots=programming-language-python#optional-examine-the-file-contents) .


## <a name="run-the-function-locally"></a>İşlevi yerel olarak çalıştırma

1. *Başlangıç* klasöründeki yerel Azure işlevleri çalışma zamanı konağını başlatarak işlevi başlatın:

    ```
    func start
    ```

1. `classify`Uç noktanın çıktıda göründüğünü gördüğünüzde URL 'ye gidin ```http://localhost:7071/api/classify?name=Azure``` . "Merhaba Azure!" iletisi çıktıda görünmelidir.

1. **Ctrl** - Konağı durdurmak için CTRL**C** 'yi kullanın.


## <a name="import-the-pytorch-model-and-add-helper-code"></a>PyTorch modelini içeri aktarın ve yardımcı kodu ekleyin

`classify`Bir görüntüyü içeriğine göre sınıflandırmak için işlevi değiştirmek üzere, önceden eğitilen bir [ResNet](https://arxiv.org/abs/1512.03385) modeli kullanın. [Pytorch](https://pytorch.org/hub/pytorch_vision_resnet/)'den gelen, önceden eğitilen model, bir görüntüyü 1 1000 [imagenet sınıflarında](https://gist.github.com/yrevar/942d3a0ac09ec9e5eb3a)sınıflandırır. Daha sonra projenize bazı yardımcı kod ve bağımlılıklar eklersiniz.

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

1. *Sınıflandır* klasörünün *Predict.py* ve *labels.txt*adlı dosyaları içerdiğini doğrulayın. Aksi takdirde, komutu *Başlangıç* klasöründe çalıştırmanızdan emin olun.

1. Bir metin düzenleyicisinde *Başlat/requirements.txt* açın ve yardımcı kodu için gereken bağımlılıkları ekleyin; Bu, aşağıdaki gibi görünmelidir:

    ```txt
    azure-functions
    requests
    -f https://download.pytorch.org/whl/torch_stable.html
    torch==1.5.0+cpu
    torchvision==0.6.0+cpu
    ```

1. *requirements.txt*kaydedin, ardından bağımlılıkları yüklemek için *Başlangıç* klasöründen aşağıdaki komutu çalıştırın.


    ```
    pip install --no-cache-dir -r requirements.txt
    ```

Yükleme işlemi birkaç dakika sürebilir. bu sırada, sonraki bölümde işlevini değiştirmeye devam edebilirsiniz.
> [!TIP]
> >Windows 'da, "bir EnvironmentError nedeniyle paketler yüklenemedi: [errno 2] böyle bir dosya veya dizin yok:" arkasından *sharded_mutable_dense_hashtable. CPython-37. PYC*gibi bir dosyaya uzun bir yol adı gelmelidir. Genellikle, bu hata, klasör yolunun derinliği çok uzun hale geldiği için oluşur. Bu durumda, `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\FileSystem@LongPathsEnabled` `1` uzun yolları etkinleştirmek için kayıt defteri anahtarını olarak ayarlayın. Alternatif olarak, Python yorumlayıcının nereye yüklendiğini kontrol edin. Bu konumda uzun bir yol varsa, daha kısa bir yola sahip bir klasöre yeniden yüklemeyi deneyin.

## <a name="update-the-function-to-run-predictions"></a>Tahminleri çalıştırmak için işlevi güncelleştirin

1. *Sınıflandırma/ \_ \_ init \_ \_ .* düzenleyene BIR metin düzenleyicisinde açın ve `import` Standart JSON kitaplığını ve *tahmin* eden yardımcıları içeri aktarmak için mevcut deyimlerden sonra aşağıdaki satırları ekleyin:

    :::code language="python" source="~/functions-pytorch/end/classify/__init__.py" range="1-6" highlight="5-6":::

1. İşlevin tüm içeriğini `main` aşağıdaki kodla değiştirin:

    :::code language="python" source="~/functions-pytorch/end/classify/__init__.py" range="8-19":::

    Bu işlev, adlı bir sorgu dizesi parametresinde bir görüntü URL 'SI alır `img` . Daha sonra `predict_image_from_url` , PyTorch modelini kullanarak görüntüyü indirmek ve sınıflandırmak için yardımcı kitaplıktan çağrı yapılır. İşlev daha sonra sonuçlarıyla bir HTTP yanıtı döndürür.

    > [!IMPORTANT]
    > Bu HTTP uç noktası, başka bir etki alanında barındırılan bir Web sayfası tarafından çağrıldığından, yanıt, `Access-Control-Allow-Origin` tarayıcının çıkış noktaları arası kaynak paylaşımı (CORS) gereksinimlerini karşılamak için bir üst bilgi içerir.
    >
    > Bir üretim uygulamasında, `*` ek güvenlik için Web sayfasının belirli bir kaynağına geçiş yapın.

1. Yaptığınız değişiklikleri kaydedin, sonra bağımlılıkların yükleme tamamlandığını varsayarak, yerel işlev konağını ile yeniden başlatın `func start` . Konağı, sanal ortamın etkinleştirildiği *Başlangıç* klasöründe çalıştırdığınızdan emin olun. Aksi takdirde konak başlar, ancak işlevi çağırırken hata görürsünüz.

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

1. Bir tarayıcıda öğesine gidin ve `localhost:8000` ardından metin kutusuna aşağıdaki fotoğraf URL 'lerinden birini girin veya herkese açık olarak erişilebilen herhangi bir görüntünün URL 'sini kullanın.

    - `https://raw.githubusercontent.com/Azure-Samples/functions-python-pytorch-tutorial/master/resources/assets/Bernese-Mountain-Dog-Temperament-long.jpg`
    - `https://github.com/Azure-Samples/functions-python-pytorch-tutorial/blob/master/resources/assets/bald-eagle.jpg?raw=true`
    - `https://raw.githubusercontent.com/Azure-Samples/functions-python-pytorch-tutorial/master/resources/assets/penguin.jpg`

1. Görüntüyü sınıflandırmak için işlev uç noktasını çağırmak üzere **Gönder** ' i seçin.

    ![Tamamlanmış projenin ekran görüntüsü](media/machine-learning-pytorch/screenshot.png)

    Görüntü URL 'sini gönderdiğinizde tarayıcıda bir hata bildirirse, işlev uygulamasını çalıştırmakta olduğunuz terminali kontrol edin. "Modül bulunamadı" PIL ' "gibi bir hata görürseniz, önce daha önce oluşturduğunuz sanal ortamı etkinleştirmeden önce, işlev uygulamasını *Başlangıç* klasöründe başlatmış olabilirsiniz. Hala hata görüyorsanız, `pip install -r requirements.txt` sanal ortam etkinleştirildikten sonra yeniden çalıştırın ve hata olup olmadığına bakın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticinin tamamı makinenizde yerel olarak çalıştığından, temizleyebilen hiçbir Azure kaynağı veya hizmeti yoktur.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir HTTP API uç noktası oluşturmayı ve özelleştirmeyi öğrenmiş olursunuz. Ayrıca, API 'nin bir Web uygulamasından nasıl çağrılacağını öğrenirsiniz. Azure Işlevleri tarafından sunulan sunucusuz işlem modelinde çalışırken her türlü karmaşıklığın API 'Lerini oluşturmak için bu öğreticideki teknikleri kullanabilirsiniz.

Ayrıca bkz:

- [Visual Studio Code kullanarak Işlevi Azure 'A dağıtın](https://code.visualstudio.com/docs/python/tutorial-azure-functions).
- [Azure Işlevleri Python Geliştirici Kılavuzu](./functions-reference-python.md)


> [!div class="nextstepaction"]
> [Azure CLı kılavuzunu kullanarak işlevi Azure Işlevleri 'ne dağıtma](./functions-run-local.md#publish)
