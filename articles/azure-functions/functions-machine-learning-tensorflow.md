---
title: Azure 'da Machine Learning için Python ve TensorFlow kullanma
description: Bir görüntüyü içeriğine göre sınıflandırmak için bir makine öğrenimi modeliyle Python, TensorFlow ve Azure Işlevleri kullanın.
author: anthonychu
ms.topic: tutorial
ms.date: 01/15/2020
ms.author: antchu
ms.custom: mvc
ms.openlocfilehash: e98655dca7d682e5c42f3b0ae7f26c892bd12377
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76710710"
---
# <a name="tutorial-apply-machine-learning-models-in-azure-functions-with-python-and-tensorflow"></a>Öğretici: Python ve TensorFlow ile Azure Işlevleri 'nde makine öğrenimi modellerini uygulama

Bu makalede, bir görüntüyü içeriğine göre sınıflandırmak için Python, TensorFlow ve Azure Işlevlerini bir makine öğrenimi modeliyle nasıl kullanacağınızı öğreneceksiniz. Tüm işleri yerel olarak yaptığınız ve bulutta hiçbir Azure kaynağı olmadığınızdan, bu öğreticiyi tamamlamaya yönelik bir maliyet yoktur.

> [!div class="checklist"]
> * Python 'da Azure Işlevleri geliştirmek için yerel bir ortam başlatın.
> * Özel bir TensorFlow makine öğrenimi modelini bir işlev uygulamasına aktarın.
> * Bir köpek veya kedi içeren bir görüntüyü sınıflandırmak için sunucusuz bir HTTP API 'SI oluşturun.
> * API 'YI bir Web uygulamasından tüketme.

## <a name="prerequisites"></a>Ön koşullar 

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).
- [Python 3.7.4](https://www.python.org/downloads/release/python-374/). (Python 3.7.4 ve Python 3.6. x, Azure Işlevleri ile doğrulanır; Python 3,8 ve sonraki sürümler henüz desteklenmiyor.)
- [Azure Functions Core Tools](functions-run-local.md#install-the-azure-functions-core-tools)
- [Visual Studio Code](https://code.visualstudio.com/) gibi bir kod Düzenleyicisi

### <a name="prerequisite-check"></a>Önkoşul denetimi

1. Bir Terminal veya komut penceresinde, Azure Functions Core Tools sürüm 2.7.1846 veya üzeri olduğunu denetlemek için `func --version` çalıştırın.
1. Python sürüm raporlarınızı 3.7. x olarak denetlemek için `python --version` (Linux/MacOS) veya `py --version` (Windows) çalıştırın.

## <a name="clone-the-tutorial-repository"></a>Öğretici deposunu Kopyala

1. Bir Terminal veya komut penceresinde, git kullanarak aşağıdaki depoyu kopyalayın:

    ```
    git clone https://github.com/Azure-Samples/functions-python-tensorflow-tutorial.git
    ```

1. Klasöre gidin ve içeriğini inceleyin.

    ```
    cd functions-python-tensorflow-tutorial
    ```

    - *Başlangıç* , öğreticinin çalışma klasörüdür.
    - *End* , başvurağınız için nihai sonuç ve tam uygulama.
    - *kaynaklar* makine öğrenimi modelini ve yardımcı kitaplıklarını içerir.
    - *ön uç* , işlev uygulamasını çağıran bir Web sitesidir.
    
## <a name="create-and-activate-a-python-virtual-environment"></a>Python sanal ortamı oluşturma ve etkinleştirme

*Başlangıç* klasörüne gidin ve `.venv`adlı bir sanal ortam oluşturmak ve etkinleştirmek için aşağıdaki komutları çalıştırın. Azure Işlevleri tarafından desteklenen Python 3,7 ' i kullandığınızdan emin olun.


# <a name="bashtabbash"></a>[Bash](#tab/bash)

```bash
cd start
```

```bash
python -m venv .venv
```

```bash
source .venv/bin/activate
```

Python, venv paketini Linux dağıtımına yüklememediyse aşağıdaki komutu çalıştırın:

```bash
sudo apt-get install python3-venv
```

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)

```powershell
cd start
```

```powershell
py -m venv .venv
```

```powershell
.venv\scripts\activate
```

# <a name="cmdtabcmd"></a>[Cmd](#tab/cmd)

```cmd
cd start
```

```cmd
py -m venv .venv
```

```cmd
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

    Bu komut, işlevin adıyla eşleşen bir klasör oluşturur, *sınıflandır*. Bu klasörde iki dosya vardır: işlev kodunu içeren *\_\_init\_\_.* ger ve işlevin tetikleyicisini ve giriş ve çıkış bağlamalarını açıklayan *function. JSON*. Bu dosyaların içeriğiyle ilgili ayrıntılar için bkz. [Azure 'DA http tarafından tetiklenen bir Python Işlevi oluşturma-dosya Içeriğini İnceleme](functions-create-first-function-python.md#optional-examine-the-file-contents).


## <a name="run-the-function-locally"></a>İşlevi yerel olarak çalıştırma

1. *Başlangıç* klasöründeki yerel Azure işlevleri çalışma zamanı konağını başlatarak işlevi başlatın:

    ```
    func start
    ```
    
1. Çıktıda `classify` uç noktası göründüğünü gördüğünüzde URL 'ye gidin, ```http://localhost:7071/api/classify?name=Azure```. "Merhaba Azure!" iletisi çıktıda görünmelidir.

1. Ana bilgisayarı durdurmak için **Ctrl**-**C** kullanın.


## <a name="import-the-tensorflow-model-and-add-helper-code"></a>TensorFlow modelini içeri aktarma ve yardımcı kodu ekleme

Bir görüntüyü içeriğine göre sınıflandırmak için `classify` işlevini değiştirmek üzere, Azure Özel Görüntü İşleme Hizmeti ile eğitilen ve verilen önceden oluşturulmuş bir TensorFlow modeli kullanın. Daha önce Klonladığınız örneğin *Resources* klasöründe yer alan model, bir köpek veya kedi içerip içermediğini temel alarak bir görüntüyü sınıflandırır. Daha sonra projenize bazı yardımcı kod ve bağımlılıklar eklersiniz.

> [!TIP]
> Özel Görüntü İşleme Hizmeti ücretsiz katmanını kullanarak kendi modelinizi derlemek istiyorsanız, [örnek proje deposundaki](https://github.com/Azure-Samples/functions-python-tensorflow-tutorial/blob/master/train-custom-vision-model.md)yönergeleri izleyin.

1. *Başlangıç* klasöründe, model dosyalarını *sınıflandır* klasörüne kopyalamak için aşağıdaki komutu çalıştırın. Komutuna `\*` eklediğinizden emin olun. 

    # <a name="bashtabbash"></a>[Bash](#tab/bash)
    
    ```bash
    cp ../resources/model/* classify
    ```
    
    # <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)
    
    ```powershell
    copy ..\resources\model\* classify
    ```
    
    # <a name="cmdtabcmd"></a>[Cmd](#tab/cmd)
    
    ```cmd
    copy ..\resources\model\* classify
    ```
    
    ---
    
1. *Sınıflandır* klasörünün *model. PB* ve *labels. txt*adlı dosyaları içerdiğini doğrulayın. Aksi takdirde, komutu *Başlangıç* klasöründe çalıştırmanızdan emin olun.

1. *Başlangıç* klasöründe, yardımcı kod içeren bir dosyayı *sınıflandır* klasörüne kopyalamak için aşağıdaki komutu çalıştırın:

    # <a name="bashtabbash"></a>[Bash](#tab/bash)
    
    ```bash
    cp ../resources/predict.py classify
    ```
    
    # <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)
    
    ```powershell
    copy ..\resources\predict.py classify
    ```
    
    # <a name="cmdtabcmd"></a>[Cmd](#tab/cmd)
    
    ```cmd
    copy ..\resources\predict.py classify
    ```
    
    ---

1. *Sınıflandır* klasörünün artık *Predict.py*adlı bir dosya içerdiğini doğrulayın.

1. *Start/requirements. txt* dosyasını bir metin düzenleyicisinde açın ve yardımcı kodun gerektirdiği aşağıdaki bağımlılıkları ekleyin:

    ```txt
    tensorflow==1.14
    Pillow
    requests
    ```
    
1. *Requirements. txt dosyasını*kaydedin.

1. *Başlangıç* klasöründe aşağıdaki komutu çalıştırarak bağımlılıkları yükler. Yükleme işlemi birkaç dakika sürebilir. bu sırada, sonraki bölümde işlevini değiştirmeye devam edebilirsiniz.

    ```
    pip install --no-cache-dir -r requirements.txt
    ```
    
    Windows 'da, "bir EnvironmentError nedeniyle paketler yüklenemedi: [errno 2] böyle bir dosya veya dizin yok:" arkasından *sharded_mutable_dense_hashtable. CPython-37. PYC*gibi bir dosyaya uzun bir yol adı gelmelidir. Genellikle, bu hata, klasör yolunun derinliği çok uzun hale geldiği için oluşur. Bu durumda, uzun yolları etkinleştirmek için kayıt defteri anahtarı `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\FileSystem@LongPathsEnabled` `1` olarak ayarlayın. Alternatif olarak, Python yorumlayıcının nereye yüklendiğini kontrol edin. Bu konumda uzun bir yol varsa, daha kısa bir yola sahip bir klasöre yeniden yüklemeyi deneyin.

> [!TIP]
> İlk tahminini yapmak için *Predict.py* üzerinde çağrılırken, `_initialize` adlı bir Işlev, TensorFlow modelini diskten yükler ve genel değişkenlerde önbelleğe alır. Bu önbelleğe alma, sonraki tahminleri hızlandırır. Genel değişkenleri kullanma hakkında daha fazla bilgi için, [Azure Işlevleri Python Geliştirici Kılavuzu](functions-reference-python.md#global-variables)' na bakın.

## <a name="update-the-function-to-run-predictions"></a>Tahminleri çalıştırmak için işlevi güncelleştirin

1. Standart JSON kitaplığı ve *tahmin* eden yardımcıları içeri aktarmak için, bir metin düzenleyicisinde *\_/\_init\_\_. Kopyala* ' yı açın ve var olan `import` deyimlerinden sonra aşağıdaki satırları ekleyin:

    :::code language="python" source="~/functions-python-tensorflow-tutorial/end/classify/__init__.py" range="1-6" highlight="5-6":::

1. `main` işlevinin tüm içeriğini aşağıdaki kodla değiştirin:

    :::code language="python" source="~/functions-python-tensorflow-tutorial/end/classify/__init__.py" range="8-19":::

    Bu işlev, `img`adlı bir sorgu dizesi parametresinde bir görüntü URL 'SI alır. Daha sonra, TensorFlow modelini kullanarak görüntüyü indirmek ve sınıflandırmak için yardımcı kitaplığından `predict_image_from_url` çağırır. İşlev daha sonra sonuçlarıyla bir HTTP yanıtı döndürür. 

    > [!IMPORTANT]
    > Bu HTTP uç noktası, başka bir etki alanında barındırılan bir Web sayfası tarafından çağrıldığından, yanıt, tarayıcının çıkış noktaları arası kaynak paylaşımı (CORS) gereksinimlerini karşılamak için bir `Access-Control-Allow-Origin` üst bilgisi içerir.
    >
    > Bir üretim uygulamasında, ek güvenlik için `*` Web sayfasının belirli bir kaynağına değiştirin.

1. Yaptığınız değişiklikleri kaydedin, sonra bağımlılıkların yükleme tamamlandığını varsayarak, yerel işlev konağını `func start`yeniden başlatın. Konağı, sanal ortamın etkinleştirildiği *Başlangıç* klasöründe çalıştırdığınızdan emin olun. Aksi takdirde konak başlar, ancak işlevi çağırırken hata görürsünüz.

    ```
    func start
    ```

1. Bir tarayıcıda, bir Cat görüntüsünün URL 'siyle işlevi çağırmak için aşağıdaki URL 'YI açın ve döndürülen JSON 'ın görüntüyü bir kedi olarak sınıflandırdığından emin olun.

    ```
    http://localhost:7071/api/classify?img=https://raw.githubusercontent.com/Azure-Samples/functions-python-tensorflow-tutorial/master/resources/assets/samples/cat1.png
    ```
    
1. Bir sonraki adımda kullandığınız için Konağı çalışır durumda tutun. 

### <a name="run-the-local-web-app-front-end-to-test-the-function"></a>İşlevi test etmek için yerel Web uygulaması ön ucu 'nı çalıştırın

İşlev uç noktasının başka bir Web uygulamasından çağrılması test etmek için deponun *ön uç* klasöründe basit bir uygulama vardır.

1. Yeni bir Terminal veya komut istemi açın ve sanal ortamı etkinleştirin (daha önce [bir Python sanal ortamı oluşturma ve etkinleştirme](#create-and-activate-a-python-virtual-environment)altında açıklandığı gibi).

1. Deponun *ön uç* klasörüne gidin.

1. Python ile HTTP sunucusu başlatma:

    # <a name="bashtabbash"></a>[Bash](#tab/bash)

    ```bash 
    python -m http.server
    ```
    
    # <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)

    ```powershell
    py -m http.server
    ```

    # <a name="cmdtabcmd"></a>[Cmd](#tab/cmd)

    ```cmd
    py -m http.server
    ```

1. Bir tarayıcıda `localhost:8000`' a gidin, ardından metin kutusuna aşağıdaki fotoğraf URL 'Lerinden birini girin veya herkese açık olarak erişilebilen herhangi bir görüntünün URL 'sini kullanın.

    - `https://raw.githubusercontent.com/Azure-Samples/functions-python-tensorflow-tutorial/master/resources/assets/samples/cat1.png`
    - `https://raw.githubusercontent.com/Azure-Samples/functions-python-tensorflow-tutorial/master/resources/assets/samples/cat2.png`
    - `https://raw.githubusercontent.com/Azure-Samples/functions-python-tensorflow-tutorial/master/resources/assets/samples/dog1.png`
    - `https://raw.githubusercontent.com/Azure-Samples/functions-python-tensorflow-tutorial/master/resources/assets/samples/dog2.png`
    
1. Görüntüyü sınıflandırmak için işlev uç noktasını çağırmak üzere **Gönder** ' i seçin.

    ![Tamamlanmış projenin ekran görüntüsü](media/functions-machine-learning-tensorflow/functions-machine-learning-tensorflow-screenshot.png)

    Görüntü URL 'sini gönderdiğinizde tarayıcıda bir hata bildirirse, işlev uygulamasını çalıştırmakta olduğunuz terminali kontrol edin. "Modül bulunamadı" PIL ' "gibi bir hata görürseniz, önce daha önce oluşturduğunuz sanal ortamı etkinleştirmeden önce, işlev uygulamasını *Başlangıç* klasöründe başlatmış olabilirsiniz. Hala hata görüyorsanız, sanal ortam etkinken `pip install -r requirements.txt` yeniden çalıştırın ve hata olup olmadığına bakın.

> [!NOTE]
> Model her zaman görüntünün içeriğini bir kedi veya bir köpek olarak sınıflandırdığından, resmin her ikisi de içerip içermediğini belirtir. Örneğin, genellikle kedi olarak sınıflandırır ve kyaların görüntüleri, ancak elekülebilirlik, carrots ya da uçak gibi sınıflandırmacılar görüntüleri.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticinin tamamı makinenizde yerel olarak çalıştığından, temizleyebilen hiçbir Azure kaynağı veya hizmeti yoktur.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir TensorFlow modeli kullanarak görüntüleri sınıflandırmak için Azure Işlevleri ile bir HTTP API uç noktası oluşturmayı ve özelleştirmeyi öğrendiniz. Ayrıca, API 'nin bir Web uygulamasından nasıl çağrılacağını öğrenirsiniz. Azure Işlevleri tarafından sunulan sunucusuz işlem modelinde çalışırken her türlü karmaşıklığın API 'Lerini oluşturmak için bu öğreticideki teknikleri kullanabilirsiniz.

> [!div class="nextstepaction"]
> [Azure CLı kılavuzunu kullanarak işlevi Azure Işlevleri 'ne dağıtma](./functions-run-local.md#publish)

Ayrıca bkz:

- [Visual Studio Code kullanarak Işlevi Azure 'A dağıtın](https://code.visualstudio.com/docs/python/tutorial-azure-functions).
- [Azure Işlevleri Python Geliştirici Kılavuzu](./functions-reference-python.md)
