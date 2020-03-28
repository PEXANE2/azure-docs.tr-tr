---
title: Makine öğrenimi deneme kodunu üretim koduna dönüştürme
titleSuffix: Azure Machine Learning
description: MLOpsPython kod şablonunu kullanarak makine öğrenimi deneysel kodunu üretim koduna nasıl dönüştüreceklerini öğrenin.
author: bjcmit
ms.author: brysmith
ms.service: machine-learning
ms.topic: tutorial
ms.date: 03/13/2020
ms.openlocfilehash: f40c2b5f7134458b3f8cb492652bebf14388634c
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79477145"
---
# <a name="tutorial-convert-ml-experimental-code-to-production-code"></a>Öğretici: ML deneysel kodunu üretim koduna dönüştürün

Bir makine öğrenme projesi hipotezler gerçek veri kümeleri kullanarak Jupyter Notebook gibi çevik araçlar ile test edilmiştir deneme gerektirir. Model üretime hazır olduğunda, model kodu bir üretim kodu deposuna yerleştirilmelidir. Bazı durumlarda, model kodunun üretim kodu deposuna yerleştirilebilmek için Python komut dosyasına dönüştürülmesi gerekir. Bu öğretici, deneme kodunun Python komut dosyasına nasıl dışa aktarılacak bir yaklaşım olduğunu kapsar.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
>
> * Gereksiz kodu temizleme
> * Jupyter Notebook kodunu işlevlere dönüştür
> * İlgili görevler için Python komut dosyaları oluşturma
> * Birim testleri oluşturma

## <a name="prerequisites"></a>Ön koşullar

- [MLOpsPython şablonunu](https://github.com/microsoft/MLOpsPython/generate) oluşturun `experimentation/Diabetes Ridge Regression Training.ipynb` ve `experimentation/Diabetes Ridge Regression Scoring.ipynb` not defterlerini kullanın. Bu not defterleri denemeden üretime dönüştürmeye örnek olarak kullanılır. Bu defterleri [https://github.com/microsoft/MLOpsPython/tree/master/experimentation](https://github.com/microsoft/MLOpsPython/tree/master/experimentation).
- nbconvert yükleyin. [Yükleme](https://nbconvert.readthedocs.io/en/latest/install.html) sayfasında __nbconvert yükleme__ bölümü altında sadece yükleme yönergeleri izleyin.

## <a name="remove-all-nonessential-code"></a>Tüm gereksiz kodu kaldırma

Deneme sırasında yazılan bazı kodlar yalnızca keşif amaçlıdır. Bu nedenle, deney kodunu üretim koduna dönüştürmek için ilk adım, bu gereksiz kodu kaldırmaktır. Gereksiz kodun kaldırılması da kodu daha koruyabilir hale getirecektir. Bu bölümde, `experimentation/Diabetes Ridge Regression Training.ipynb` kodu not defterinden kaldırırsınız. Şekli `X` yazdıran ifadeler `y` ve hücre `features.describe` araması yalnızca veri araştırması içindir ve kaldırılabilir. Gereksiz kodu kaldırdıktan `experimentation/Diabetes Ridge Regression Training.ipynb` sonra, işaretleme olmadan aşağıdaki kod gibi görünmelidir:

```python
from sklearn.datasets import load_diabetes
from sklearn.linear_model import Ridge
from sklearn.metrics import mean_squared_error
from sklearn.model_selection import train_test_split
import joblib

X, y = load_diabetes(return_X_y=True)

X_train, X_test, y_train, y_test = train_test_split(
        X, y, test_size=0.2, random_state=0)
data = {"train": {"X": X_train, "y": y_train},
        "test": {"X": X_test, "y": y_test}}

alpha = 0.5

reg = Ridge(alpha=alpha)
reg.fit(data["train"]["X"], data["train"]["y"])

preds = reg.predict(data["test"]["X"])
print("mse", mean_squared_error(preds, data["test"]["y"]))

model_name = "sklearn_regression_model.pkl"
joblib.dump(value=reg, filename=model_name)
```

## <a name="refactor-code-into-functions"></a>Yeniden düzenleme kodu işlevlere

İkinci olarak, Jupyter kodunun işlevlere yeniden dahil edilmesi gerekiyor. Kodu işlevlere yeniden düzenleme birim sınamasını kolaylaştırır ve kodun daha korunabilir hale getirir. Bu bölümde, yeniden faktör olacak:

- Diabetes Ridge Regresyon`experimentation/Diabetes Ridge Regression Training.ipynb`Eğitimi not defteri( )
- Diabetes Ridge Regresyon`experimentation/Diabetes Ridge Regression Scoring.ipynb`Puanlama defteri( )

### <a name="refactor-diabetes-ridge-regression-training-notebook-into-functions"></a>Fonksiyonlar içine Refactor Diabetes Ridge Regresyon Eğitim dizüstü

In `experimentation/Diabetes Ridge Regression Training.ipynb`, aşağıdaki adımları tamamlayın:

1. Parametreleri `train_model` `data` alan ve `alpha` bir modeli döndüren , adlı bir işlev oluşturun.
1. Kodu "Eğitim Setinde Model On" ve "Doğrulama Kümesinde Modeli Doğrula" `train_model` başlıkları altında işlevi kopyalayın.

İşlev `train_model` aşağıdaki kod gibi görünmelidir:

```python
def train_model(data, alpha):
    reg = Ridge(alpha=alpha)
    reg.fit(data["train"]["X"], data["train"]["y"])
    preds = reg.predict(data["test"]["X"])
    print("mse", mean_squared_error(
        preds, data["test"]["y"]))
    return reg
```

`train_model` İşlev oluşturulduktan sonra, kodu "Eğitim Setinde Model On" ve "Doğrulama Kümesinde Modeli Doğrula" başlıkları altında aşağıdaki ifadeyle değiştirin:

```python
reg = train_model(data, alpha)
```

Önceki deyim, `train_model` geçen işlevi `data` `alpha` ve parametreleri çağırır ve modeli döndürür.

In `experimentation/Diabetes Ridge Regression Training.ipynb`, aşağıdaki adımları tamamlayın:

1. Hiçbir parametre `main`almaz ve hiçbir şey döndürür adlı yeni bir işlev oluşturun.
1. Kodu "Veri Yükle", "Verileri Eğitim ve Doğrulama Kümelerine Bölme" ve "Modeli Kaydet" başlıkları altında işleve kopyalayın. `main`
1. Yeni oluşturulan aramayı `train_model` işleve `main` kopyalayın.

İşlev `main` aşağıdaki kod gibi görünmelidir:

```python
def main():

    model_name = "sklearn_regression_model.pkl"
    alpha = 0.5

    X, y = load_diabetes(return_X_y=True)

    X_train, X_test, y_train, y_test = train_test_split(
        X, y, test_size=0.2, random_state=0)
    data = {"train": {"X": X_train, "y": y_train},
            "test": {"X": X_test, "y": y_test}}

    reg = train_model(data, alpha)

    joblib.dump(value=reg, filename=model_name)
```

`main` İşlev oluşturulduktan sonra, "Veri Yükle", "Verileri Eğitim ve Doğrulama Kümelerine Bölme" ve "Modeli Kaydet" başlıkları `train_model` altındaki tüm kodu aşağıdaki ifadeyle değiştirin:

```python
main()
```

Yeniden düzenlemeden `experimentation/Diabetes Ridge Regression Training.ipynb` sonra, işaretleme olmadan aşağıdaki kod gibi görünmelidir:

```python
from sklearn.datasets import load_diabetes
from sklearn.linear_model import Ridge
from sklearn.metrics import mean_squared_error
from sklearn.model_selection import train_test_split
import joblib


def train_model(data, alpha):
    reg = Ridge(alpha=alpha)
    reg.fit(data["train"]["X"], data["train"]["y"])
    preds = reg.predict(data["test"]["X"])
    print("mse", mean_squared_error(
        preds, data["test"]["y"]))
    return reg

def main():

    model_name = "sklearn_regression_model.pkl"
    alpha = 0.5

    X, y = load_diabetes(return_X_y=True)

    X_train, X_test, y_train, y_test = train_test_split(
        X, y, test_size=0.2, random_state=0)
    data = {"train": {"X": X_train, "y": y_train},
            "test": {"X": X_test, "y": y_test}}

    reg = train_model(data, alpha)

    joblib.dump(value=reg, filename=model_name)

main()
```

### <a name="refactor-diabetes-ridge-regression-scoring-notebook-into-functions"></a>Refactor Diabetes Ridge Regresyon Fonksiyonlara Not Defteri Puanlama

In `experimentation/Diabetes Ridge Regression Scoring.ipynb`, aşağıdaki adımları tamamlayın:

1. Hiçbir parametre `init`almaz ve hiçbir şey döndürmeden adlandırılan yeni bir işlev oluşturun.
1. Kodu "Yük Modeli" başlığı altında `init` işleve kopyalayın.

İşlev `init` aşağıdaki kod gibi görünmelidir:

```python
def init():
    model_path = Model.get_model_path(
        model_name="sklearn_regression_model.pkl")
    model = joblib.load(model_path)
```

`init` İşlev oluşturulduktan sonra, "Yük Modeli" başlığı altındaki tüm kodu `init` aşağıdaki gibi tek bir çağrıyla değiştirin:

```python
init()
```

In `experimentation/Diabetes Ridge Regression Scoring.ipynb`, aşağıdaki adımları tamamlayın:

1. Parametre olarak `run`ve `request_headers` aşağıdaki `raw_data` gibi bir sonuç sözlüğü döndüren ve döndüren yeni bir işlev oluşturun:

    ```python
    {"result": result.tolist()}
    ```

1. Kodu "Veri Hazırlama" ve "Veri Puan" başlıkları `run` altında işleve kopyalayın.

    İşlev `run` aşağıdaki kod gibi görünmelidir (Değişkenleri `raw_data` ayarlayan ve `request_headers`işlev çağrıldığında daha `run` sonra kullanılacak olan ifadeleri kaldırmayı unutmayın):

    ```python
    def run(raw_data, request_headers):
        data = json.loads(raw_data)["data"]
        data = numpy.array(data)
        result = model.predict(data)

        return {"result": result.tolist()}
    ```

`run` İşlev oluşturulduktan sonra, "Veri Hazırlama" ve "Veri Puan" başlıkları altındaki tüm kodu aşağıdaki kodla değiştirin:

```python
raw_data = '{"data":[[1,2,3,4,5,6,7,8,9,10],[10,9,8,7,6,5,4,3,2,1]]}'
request_header = {}
prediction = run(raw_data, request_header)
print("Test result: ", prediction)
```

Önceki kod değişkenleri `raw_data` `request_header`ayarlar ve `run` , `raw_data` `request_header`işlevi çağırır ve , ve tahminleri yazdırır.

Yeniden düzenlemeden `experimentation/Diabetes Ridge Regression Scoring.ipynb` sonra, işaretleme olmadan aşağıdaki kod gibi görünmelidir:

```python
import json
import numpy
from azureml.core.model import Model
import joblib

def init():
    model_path = Model.get_model_path(
        model_name="sklearn_regression_model.pkl")
    model = joblib.load(model_path)

def run(raw_data, request_headers):
    data = json.loads(raw_data)["data"]
    data = numpy.array(data)
    result = model.predict(data)

    return {"result": result.tolist()}

init()
test_row = '{"data":[[1,2,3,4,5,6,7,8,9,10],[10,9,8,7,6,5,4,3,2,1]]}'
request_header = {}
prediction = run(test_row, {})
print("Test result: ", prediction)
```

## <a name="combine-related-functions-in-python-files"></a>Python dosyalarında ilgili işlevleri birleştirme

Üçüncü olarak, ilgili işlevlerin kodun yeniden kullanımına daha iyi yardımcı olması için Python dosyalarında birleştirilmesi gerekir. Bu bölümde, aşağıdaki not defterleri için Python dosyaları oluşturacaksınız:

- Diabetes Ridge Regresyon`experimentation/Diabetes Ridge Regression Training.ipynb`Eğitimi not defteri( )
- Diabetes Ridge Regresyon`experimentation/Diabetes Ridge Regression Scoring.ipynb`Puanlama defteri( )

### <a name="create-python-file-for-the-diabetes-ridge-regression-training-notebook"></a>Diabetes Ridge Regresyon Eğitimi dizüstü bilgisayarı için Python dosyası oluşturma

Not defterinizi, nbconvert paketini ve aşağıdaki yolu kullanan bir komut isteminde aşağıdaki `experimentation/Diabetes Ridge Regression Training.ipynb`deyimi çalıştırarak çalıştırılabilir bir komut dosyasına dönüştürün:

```
jupyter nbconvert -- to script "Diabetes Ridge Regression Training.ipynb" –output train
```

Not defteri dönüştürüldükten `train.py`sonra tüm yorumları kaldırın. Dosyanız `train.py` aşağıdaki kod gibi görünmelidir:

```python
from sklearn.datasets import load_diabetes
from sklearn.linear_model import Ridge
from sklearn.metrics import mean_squared_error
from sklearn.model_selection import train_test_split
import joblib


def train_model(data, alpha):
    reg = Ridge(alpha=alpha)
    reg.fit(data["train"]["X"], data["train"]["y"])
    preds = reg.predict(data["test"]["X"])
    print("mse", mean_squared_error(
        preds, data["test"]["y"]))
    return reg

def main():
    model_name = "sklearn_regression_model.pkl"
    alpha = 0.5

    X, y = load_diabetes(return_X_y=True)

    X_train, X_test, y_train, y_test = train_test_split(
        X, y, test_size=0.2, random_state=0)
    data = {"train": {"X": X_train, "y": y_train},
            "test": {"X": X_test, "y": y_test}}

    reg = train_model(data, alpha)

    joblib.dump(value=reg, filename=model_name)

main()
```

`train.py` MLOpsPython deposunda `diabetes_regression/training` dizinde bulunan dosya komut satırı bağımsız değişkenlerini `build_id`destekler `model_name`(yani , , ve `alpha`). Komut satırı bağımsız değişkenleri için `train.py` destek dinamik model adlarını ve `alpha` değerlerini desteklemek için dosyanıza eklenebilir, ancak kodun başarılı bir şekilde yürütülmesi gerekmez.

### <a name="create-python-file-for-the-diabetes-ridge-regression-scoring-notebook"></a>Diabetes Ridge Regresyon Puanlama not defteri için Python dosyası oluşturma

Not defterinizi, nbconvert paketini ve aşağıdaki yolu kullanan bir komut isteminde aşağıdaki `experimentation/Diabetes Ridge Regression Scoring.ipynb`deyimi çalıştırarak çalıştırılabilir bir komut dosyasına dahil edin:

```
jupyter nbconvert -- to script "Diabetes Ridge Regression Scoring.ipynb" –output score
```

Not defteri dönüştürüldükten `score.py`sonra tüm yorumları kaldırın. Dosyanız `score.py` aşağıdaki kod gibi görünmelidir:

```python
import json
import numpy
from azureml.core.model import Model
import joblib

def init():
    model_path = Model.get_model_path(
        model_name="sklearn_regression_model.pkl")
    model = joblib.load(model_path)

def run(raw_data, request_headers):
    data = json.loads(raw_data)["data"]
    data = numpy.array(data)
    result = model.predict(data)

    return {"result": result.tolist()}

init()
test_row = '{"data":[[1,2,3,4,5,6,7,8,9,10],[10,9,8,7,6,5,4,3,2,1]]}'
request_header = {}
prediction = run(test_row, request_header)
print("Test result: ", prediction)
```

İşlev, `train_model` genel değişken bir modeli komut dosyası nda görünür hale getirmek için değiştirilmesi gerekir. İşlevin başında aşağıdaki ifadeyi `init` ekleyin:

```python
global model
```

Önceki deyim ekledikten `init` sonra, işlev aşağıdaki kod gibi görünmelidir:

```python
def init():
    global model

    # load the model from file into a global object
    model_path = Model.get_model_path(
        model_name="sklearn_regression_model.pkl")
    model = joblib.load(model_path)
```

## <a name="create-unit-tests-for-each-python-file"></a>Her Python dosyası için birim testleri oluşturma

Dördüncü olarak, her Python dosyası için birim testleri oluşturulması gerekir, bu da kodu daha sağlam ve bakımı daha kolay hale getirir. Bu bölümde, `train.py`'deki işlevlerden biri için bir birim testi oluşturacaksınız.

`train.py`iki işlev `train_model` içerir: ve `main`. Her işlevin bir birim testine ihtiyacı vardır, ancak `train_model` bu öğreticide Pytest çerçevesini kullanarak işlev için yalnızca tek bir birim testi oluşturacağız. Pytest sadece Python birim test çerçevesi değil, ama en yaygın olarak kullanılan biridir. Daha fazla bilgi için [Pytest](https://pytest.org)adresini ziyaret edin.

Bir birim testi genellikle üç ana eylem içerir:

- Nesneyi düzenleme - gerekli nesneleri oluşturma ve ayarlama
- Bir nesneye göre hareket edin
- Bekleneni ortaya koy

Bunun için `train_model` ortak `data` bir `alpha` koşul, bir değerin ne zaman geçirildiğidir. Beklenen sonuç, ve `Ridge.train` `Ridge.predict` işlevlerin çağrılması gerektiğidir. Makine öğrenimi eğitim yöntemleri genellikle hızlı çalışmadığından, çağrı alay `Ridge.train` edilecektir. İade değeri alay `Ridge.train` edilen bir nesne olduğu için, `Ridge.predict`biz de alay edeceğiz. Geçişini `train_model` test etmek `data` için `alpha` birim testi ve `Ridge.train` beklenen `Ridge.predict` sonucu ve işlevleri ile bir değer alay ve Pytest çerçevesi kullanılarak çağrılan aşağıdaki kod gibi görünmelidir:

```python
import pytest
from code.training.train import train_model

class TestTrain:

    @staticmethod
    def test_train_model(mocker):
        # Arrange
        test_data = {"train": {"X": [[1, 2, 3]], "y": [0]},
                     "test": {"X": [[4, 5, 6]], "y": [0]}}
        test_alpha = 0.5
        mock_ridge_fit = mocker.patch('Ridge.fit')
        mock_ridge_predict = mocker.patch('Ridge.predict')

        # Act
        train_model(test_data, test_alpha)

        # Assert
        mock_ridge_fit.assert_called()
        mock_ridge_predict.assert_called()
```

## <a name="use-your-own-model-with-mlopspython-code-template"></a>MLOpsPython kod şablonu ile kendi modelinizi kullanın

Bu kılavuzdaki adımları takip ediyorsanız, MLOpsPython deposunda bulunan tren/puan/test komut dosyalarıyla ilişkili bir dizi komut dosyanız vardır.  Yukarıda belirtilen yapıya göre, kendi makine öğrenimi projeniz için bu dosyaları kullanmak için gerekenleri aşağıdaki adımlar ile yürüyecektir:

1. MLOpsPython [Başlarken](https://github.com/microsoft/MLOpsPython/blob/master/docs/getting_started.md) kılavuzunu izleyin
2. Proje başlangıç noktanızı oluşturmak için MLOpsPython [bootstrap yönergeleri](https://github.com/microsoft/MLOpsPython/blob/master/bootstrap/README.md) izleyin
3. Eğitim Kodunu Değiştir
4. Puan Kodunu Değiştir
5. Değerlendirme Kodunu Güncelleştir

### <a name="follow-the-getting-started-guide"></a>Başlarken Kılavuzunu Takip Edin
[Başlarken](https://github.com/microsoft/MLOpsPython/blob/master/docs/getting_started.md) kılavuzunu takip ederek MLOpsPython'u çalıştırmak için destekleyici altyapıya ve boru hatlarına sahip olmak gerekir.

### <a name="follow-the-bootstrap-instructions"></a>Bootstrap Yönergeleri izleyin

[MLOpsPython depo kılavuzundan Bootstrap](https://github.com/microsoft/MLOpsPython/blob/master/bootstrap/README.md) hızlı bir şekilde proje için depo hazırlamak için yardımcı olacaktır.

**Not:** Bootstrap komut dosyası diabetes_regression klasörü seçtiğiniz proje adına yeniden adlandıracağı için, projenize yolların dahil olduğu zaman olarak `[project name]` atıfta bulunacağız.

### <a name="replace-training-code"></a>Eğitim Kodunu Değiştir

Modeli eğitmek için kullanılan kodun değiştirilmesi ve çözümün kendi kodunuzla çalışması için ilgili birim testlerini kaldırması veya değiştirmesi gerekir. Aşağıdaki adımları özellikle izleyin:

1. Değiştir. `[project name]/training/train.py` Bu komut dosyası, modelinizi yerel olarak veya Azure ML bilgi işlem biriminde eğitir.
1. Eğitim birimi testlerini kaldırma veya değiştirme`[project name]/training/test_train.py`

### <a name="replace-score-code"></a>Puan Kodunu Değiştir

Modelin gerçek zamanlı çıkarım özellikleri sağlaması için puan kodunun değiştirilmesi gerekir. MLOpsPython şablonu, ACI, AKS veya Web uygulamalarında gerçek zamanlı puanlama yapmak için modeli dağıtmak için puan kodunu kullanır. Puanlamaya devam etmek istiyorsanız, değiştirin. `[project name]/scoring/score.py`

### <a name="update-evaluation-code"></a>Değerlendirme Kodunu Güncelleştir

MLOpsPython şablonu, yeni eğitilen modelin performansını ve Ortalama Kareli Hata'yı temel alan geçerli üretim modelini karşılaştırmak için evaluate_model komut dosyasını kullanır. Yeni eğitilen modelin performansı geçerli üretim modelinden daha iyiyse, ardışık işlemler devam eder. Aksi takdirde, ardışık hatlar iptal edilir. Değerlendirmeyi tutmak için, tüm `mse` `[project name]/evaluate/evaluate_model.py` in örneklerini istediğiniz metrikle değiştirin.

Değerlendirmeden kurtulmak `RUN_EVALUATION` `.pipelines/[project name]-variables-template.yml` için `false`DevOps ardışık hatlar değişkenini .

## <a name="next-steps"></a>Sonraki adımlar

Artık bir denemeden üretim koduna nasıl dönüştürüleceklerini anladığınızdan, web hizmetleri olarak dağıtılan deneme çalıştırmalarını ve modelleri izlemeyi öğrenmek için aşağıdaki bağlantıları kullanın:

> [!div class="nextstepaction"]
> [Azure ML deneme çalıştırmalarını ve ölçümlerini](https://docs.microsoft.com/azure/machine-learning/how-to-track-experiments)
> [izleyin ML web hizmeti uç noktalarından verileri izleyin ve toplayın](https://docs.microsoft.com/azure/machine-learning/how-to-enable-app-insights)
