---
title: Machine Learning deneme kodunu üretim koduna Dönüştür
titleSuffix: Azure Machine Learning
description: MLOpsPython Code şablonunu kullanarak makine öğrenimi deneysel kodunu üretim koduna dönüştürmeyi öğrenin.
author: bjcmit
ms.author: brysmith
ms.service: machine-learning
ms.topic: tutorial
ms.date: 02/10/2020
ms.openlocfilehash: 5a7c4ce6d5868efef4cfb4fbe2183ec8337ff5b6
ms.sourcegitcommit: f915d8b43a3cefe532062ca7d7dbbf569d2583d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78301854"
---
# <a name="tutorial-convert-ml-experimental-code-to-production-code"></a>Öğretici: ML deneysel kodu üretim koduna Dönüştür

Machine Learning projesi, gerçek veri kümelerini kullanarak Jupyter Notebook gibi çevik araçlarla deneme gerektirir. Model üretime hazırsa, model kodu bir üretim kodu deposuna yerleştirilmelidir. Bazı durumlarda, model kodu, üretim kodu deposuna yerleştirilecek Python betiklerine dönüştürülmelidir. Bu öğreticide, deneme kodunu Python betiklerine aktarma hakkında önerilen bir yaklaşım ele alınmaktadır.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
>
> * Gereksiz kodu temizle
> * Kodu işlevlere Jupyter Notebook yeniden düzenleme
> * İlgili görevler için Python betikleri oluşturma
> * Birim testleri oluşturma

## <a name="prerequisites"></a>Önkoşullar

- [MLOpsPython şablonunu](https://github.com/microsoft/MLOpsPython/generate) oluşturun ve `experimentation/Diabetes Ridge Regression Training.ipynb` ve `experimentation/Diabetes Ridge Regression Scoring.ipynb` not defterlerini kullanın. Bu not defterleri, deneme 'den üretime dönüştürme örneği olarak kullanılır.
- Nbconvert 'i yükler. [Yükleme](https://nbconvert.readthedocs.io/en/latest/install.html) sayfasında, yalnızca __nbconvert 'i yükleme__ bölümündeki yükleme yönergelerini izleyin.

## <a name="remove-all-nonessential-code"></a>Tüm gereksiz kodu kaldır

Deneme sırasında yazılan bazı kodlar yalnızca keşif amaçlıdır. Bu nedenle, deneysel kodu üretim koduna dönüştürmenin ilk adımı, bu gereksiz kodu kaldırmayacak. Gerekli olmayan kodun kaldırılması kodu daha sürdürülebilir hale getirir. Bu bölümde `experimentation/Diabetes Ridge Regression Training.ipynb` Not defterinden kod kaldıracaksınız. `X` ve `y` şeklinin yanı sıra `features.describe` çağıran hücre yazdırma deyimleri yalnızca veri araştırmasına yöneliktir ve kaldırılabilirler. Gereksiz kodu kaldırdıktan sonra, `experimentation/Diabetes Ridge Regression Training.ipynb` markı olmadan aşağıdaki koda benzemelidir:

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

## <a name="refactor-code-into-functions"></a>Kodu işlevlere yeniden düzenleme

İkincisi, Juprivcode 'un işlevlere yeniden düzenlenmiş olması gerekir. Kodu işlevlere yeniden düzenleme, birim testini daha kolay hale getirir ve kodu daha sürdürülebilir hale getirir. Bu bölümde, yeniden düzenleme yapacaksınız:

- Diabetes Ridge Regresyon eğitimi Not defteri (`experimentation/Diabetes Ridge Regression Training.ipynb`)
- Diabetes Perdge regresyon Puanlama Not defteri (`experimentation/Diabetes Ridge Regression Scoring.ipynb`)

### <a name="refactor-diabetes-ridge-regression-training-notebook-into-functions"></a>Diabetes Ridge gerileme eğitim Not defterini işlevlere yeniden düzenleme

`experimentation/Diabetes Ridge Regression Training.ipynb`, aşağıdaki adımları izleyin:

1. `data` ve `alpha` parametreleri alan ve bir model döndüren `train_model`adlı bir işlev oluşturun.
1. Kodu, `train_model` işlevine "eğitim kümesindeki modeli eğitme" ve "doğrulama kümesindeki modeli Doğrula" başlıklarının altına kopyalayın.

`train_model` işlevi aşağıdaki kod gibi görünmelidir:

```python
def train_model(data, alpha):
    reg = Ridge(alpha=alpha)
    reg.fit(data["train"]["X"], data["train"]["y"])
    preds = reg.predict(data["test"]["X"])
    print("mse", mean_squared_error(
        preds, data["test"]["y"]))
    return reg
```

`train_model` işlevi oluşturulduktan sonra, "eğitim kümesindeki modeli eğitme" ve "doğrulama kümesindeki modeli Doğrula" başlıklarındaki kodu aşağıdaki ifadeyle değiştirin:

```python
reg = train_model(data, alpha)
```

Önceki ifade, `data` ve `alpha` parametrelerini geçirerek `train_model` işlevini çağırır ve modeli döndürür.

`experimentation/Diabetes Ridge Regression Training.ipynb`, aşağıdaki adımları izleyin:

1. `main`adlı yeni bir işlev oluşturun ve hiçbir parametre alıp hiçbir şey döndürmez.
1. Kodu, "verileri yükle", "verileri eğitim ve doğrulama kümelerine bölme" ve "modeli Kaydet" başlıklarındaki `main` işlevine kopyalayın.
1. Yeni oluşturulan çağrıyı `train_model` `main` işlevine kopyalayın.

`main` işlevi aşağıdaki kod gibi görünmelidir:

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

`main` işlevi oluşturulduktan sonra, "verileri yükle", "verileri eğitimlere ve doğrulama kümelerine Böl" ve "modeli Kaydet" başlıkları altındaki tüm kodları değiştirin ve aşağıdaki bildirimin `train_model` yeni oluşturulan çağrısıyla birlikte "modeli Kaydet" ifadesini kullanın:

```python
main()
```

Yeniden düzenleme işleminden sonra, `experimentation/Diabetes Ridge Regression Training.ipynb` markafter olmadan aşağıdaki koda benzemelidir:

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

### <a name="refactor-diabetes-ridge-regression-scoring-notebook-into-functions"></a>Diabetes Ridge Regresyon Puanlama Not defterini işlevlere yeniden düzenleme

`experimentation/Diabetes Ridge Regression Scoring.ipynb`, aşağıdaki adımları izleyin:

1. `init`adlı yeni bir işlev oluşturun ve hiçbir parametre alıp hiçbir şey döndürmez.
1. Kodu, "model yükleme" başlığının altına `init` işlevine kopyalayın.

`init` işlevi aşağıdaki kod gibi görünmelidir:

```python
def init():
    model_path = Model.get_model_path(
        model_name="sklearn_regression_model.pkl")
    model = joblib.load(model_path)
```

`init` işlevi oluşturulduktan sonra, "Load model" başlığı altındaki tüm kodu aşağıdaki gibi tek bir `init` çağrısıyla değiştirin:

```python
init()
```

`experimentation/Diabetes Ridge Regression Scoring.ipynb`, aşağıdaki adımları izleyin:

1. `raw_data` ve `request_headers` parametre olarak alan `run`adlı yeni bir işlev oluşturun ve sonuçların bir sözlüğünü aşağıdaki gibi döndürür:

    ```python
    {"result": result.tolist()}
    ```

1. Kodu, "verileri hazırlama" ve "veri puanı" başlıklarının altına `run` işlevine kopyalayın.

    `run` işlevi aşağıdaki koda benzemelidir (daha sonra `run` işlevi çağrıldığında kullanılacak `raw_data` ve `request_headers`değişkenlerini belirleyen deyimleri kaldırmayı unutmayın):

    ```python
    def run(raw_data, request_headers):
        data = json.loads(raw_data)["data"]
        data = numpy.array(data)
        result = model.predict(data)

        return {"result": result.tolist()}
    ```

`run` işlevi oluşturulduktan sonra, "veri hazırlama" ve "veri puanı" başlıklarının altındaki tüm kodu aşağıdaki kodla değiştirin:

```python
raw_data = '{"data":[[1,2,3,4,5,6,7,8,9,10],[10,9,8,7,6,5,4,3,2,1]]}'
request_header = {}
prediction = run(raw_data, request_header)
print("Test result: ", prediction)
```

Önceki kod, `raw_data` ve `request_header`değişkenlerini ayarlar, `run` işlevi `raw_data` ve `request_header`çağırır ve tahminleri yazdırır.

Yeniden düzenleme işleminden sonra, `experimentation/Diabetes Ridge Regression Scoring.ipynb` markafter olmadan aşağıdaki koda benzemelidir:

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

Kodun yeniden kullanılmasına daha fazla yardımcı olmak için, üçüncü ve ilgili işlevlerin Python dosyalarında birleştirilmesi gerekir. Bu bölümde, aşağıdaki Not defterleri için Python dosyaları oluşturacaksınız:

- Diabetes Ridge Regresyon eğitimi Not defteri (`experimentation/Diabetes Ridge Regression Training.ipynb`)
- Diabetes Perdge regresyon Puanlama Not defteri (`experimentation/Diabetes Ridge Regression Scoring.ipynb`)

### <a name="create-python-file-for-the-diabetes-ridge-regression-training-notebook"></a>Diabetes Ridge Regresyon eğitimi Not defteri için Python dosyası oluşturma

Aşağıdaki ifadeyi, nbconvert paketini ve `experimentation/Diabetes Ridge Regression Training.ipynb`yolunu kullanan bir komut isteminde çalıştırarak not defterinizi yürütülebilir bir betiğe dönüştürün:

```
jupyter nbconvert -- to script "Diabetes Ridge Regression Training.ipynb" –output train
```

Not defteri `train.py`'e dönüştürüldükten sonra tüm açıklamaları kaldırın. `train.py` dosyanız aşağıdaki kod gibi görünmelidir:

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

MLOpsPython deposundaki `diabetes_regression/training` dizininde bulunan `train.py` dosyası, komut satırı bağımsız değişkenlerini destekler (yani `build_id`, `model_name`ve `alpha`). Komut satırı bağımsız değişkenleri için destek, dinamik model adlarını ve `alpha` değerlerini desteklemek üzere `train.py` dosyanıza eklenebilir, ancak kodun başarıyla yürütülmesi gerekli değildir.

### <a name="create-python-file-for-the-diabetes-ridge-regression-scoring-notebook"></a>Diabetes Ridge Regresyon Puanlama Not defteri için Python dosyası oluşturma

Aşağıdaki ifadeyi, nbconvert paketini ve `experimentation/Diabetes Ridge Regression Scoring.ipynb`yolunu kullanan bir komut isteminde çalıştırarak not defterinizi yürütülebilir bir betiğe birlikte açın:

```
jupyter nbconvert -- to script "Diabetes Ridge Regression Scoring.ipynb" –output score
```

Not defteri `score.py`'e dönüştürüldükten sonra tüm açıklamaları kaldırın. `score.py` dosyanız aşağıdaki kod gibi görünmelidir:

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

`train_model` işlevin, betik genelinde görünür olması için genel bir değişken modeli örneğini oluşturmak için değiştirilmesi gerekir. `init` işlevinin başına aşağıdaki ifadeyi ekleyin:

```python
global model
```

Önceki deyimin eklendikten sonra, `init` işlevi aşağıdaki kod gibi görünmelidir:

```python
def init():
    global model

    # load the model from file into a global object
    model_path = Model.get_model_path(
        model_name="sklearn_regression_model.pkl")
    model = joblib.load(model_path)
```

## <a name="create-unit-tests-for-each-python-file"></a>Her Python dosyası için birim testleri oluşturma

Dördüncü, her Python dosyası için birim testlerinin oluşturulması gerekir. Bu, kodu daha sağlam ve bakımını daha kolay hale getirir. Bu bölümde, `train.py`' deki işlevlerden biri için bir birim testi oluşturacaksınız.

`train.py` iki işlev içerir: `train_model` ve `main`. Her işlevde bir birim testi gerekir, ancak bu öğreticide Pytest çerçevesini kullanarak `train_model` işlevi için yalnızca tek bir birim testi oluşturacağız. Pytest tek Python birimi test çatısı değildir, ancak en sık kullanılan bir biridir. Daha fazla bilgi için [Pytest](https://pytest.org)' i ziyaret edin.

Birim testi genellikle üç ana eylem içerir:

- Nesne düzenleme-gerekli nesneleri oluşturma ve ayarlama
- Bir nesne üzerinde işlem
- Onay beklenildiği onaylama

`train_model` için genel bir koşul `data` ve `alpha` bir değer geçirildiğinde geçirilir. Beklenen sonuç, `Ridge.train` ve `Ridge.predict` işlevlerinin çağrılması olmalıdır. Machine Learning eğitim yöntemleri genellikle hızlı çalışır olmadığından, `Ridge.train` çağrısı yeniden yapılır. `Ridge.train` dönüş değeri bir moclenmiş nesne olduğundan, `Ridge.predict`de açıklayacağız. `data` ve bir `alpha` değerin, sahte işlem kullanılarak Çağrılmakta olan `Ridge.train` ve `Ridge.predict` işlevlerinin beklenen sonucuyla birlikte `train_model` test etmek için birim testi aşağıdaki kod gibi görünmelidir:

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

## <a name="use-your-own-model-with-mlopspython-code-template"></a>MLOpsPython Code şablonuyla kendi modelinizi kullanın

Bu kılavuzdaki adımları takip ediyorsanız, MLOpsPython deposunda bulunan eğitme/puan/test betiklerine göre bağıntılı bir komut dosyası kümesi olacaktır.  Yukarıda bahsedilen yapıya göre aşağıdaki adımlar, kendi Machine Learning projeniz için bu dosyaları kullanmak için gerekenleri adım adım gösterecektir:

1. MLOpsPython [Başlarken](https://github.com/microsoft/MLOpsPython/blob/master/docs/getting_started.md) kılavuzunu izleyin
2. Proje Başlangıç noktanızı oluşturmak için MLOpsPython [bootstrap yönergelerini](https://github.com/microsoft/MLOpsPython/blob/master/bootstrap/README.md) izleyin
3. Eğitim kodunu değiştirme
4. Puan kodunu değiştirme
5. Değerlendirme kodunu güncelleştirme

### <a name="follow-the-getting-started-guide"></a>Başlarken Kılavuzunu izleyin
MLOpsPython yürütmek [için Başlarken altyapısının](https://github.com/microsoft/MLOpsPython/blob/master/docs/getting_started.md) ve işlem hatlarının yürütülmesi gerekir.

### <a name="follow-the-bootstrap-instructions"></a>Önyükleme yönergelerini izleyin

[MLOpsPython Repository kılavuzundaki önyükleme](https://github.com/microsoft/MLOpsPython/blob/master/bootstrap/README.md) , projeniz için depoyu hızlı bir şekilde hazırlamanıza yardımcı olur.

**Note:** Önyükleme betiği diabetes_regression klasörü seçtiğiniz proje adıyla yeniden adlandıracaktır, bu, yollar dahil edildiğinde projenize `[project name]` olarak başvuracağız.

### <a name="replace-training-code"></a>Eğitim kodunu değiştir

Çözümün kendi kodunuzla çalışması için, modeli eğitme ve ilgili birim testlerini kaldırmak veya değiştirmek için kullanılan kodu değiştirme gerekir. Bu adımları özellikle izleyin:

1. `[project name]/training/train.py`değiştirin. Bu betik, modelinizi yerel olarak veya Azure ML işlem üzerinde ister.
1. `[project name]/training/test_train.py` bulunan eğitim birimi testlerini kaldırın veya değiştirin

### <a name="replace-score-code"></a>Puan kodunu değiştir

Modelin gerçek zamanlı çıkarım özellikleri sağlaması için, puan kodunun değiştirilmesini gerekir. MLOpsPython şablonu, ACI, AKS veya Web Apps 'teki gerçek zamanlı Puanlama yapmak için modeli dağıtmak üzere puan kodunu kullanır. Puanlama tutmak istiyorsanız `[project name]/scoring/score.py`değiştirin.

### <a name="update-evaluation-code"></a>Değerlendirme kodunu Güncelleştir

MLOpsPython şablonu, yeni eğitilen modelin ve geçerli üretim modelinin performansını ortalama kareli hataya göre karşılaştırmak için evaluate_model betiğini kullanır. Yeni eğitilen modelin performansı geçerli üretim modelinden daha büyükse, işlem hatları devam eder. Aksi takdirde, işlem hatları iptal edilir. Değerlendirmeyi sürdürmek için, `[project name]/evaluate/evaluate_model.py` içindeki tüm `mse` örneklerini istediğiniz ölçüm ile değiştirin.

Değerlendirmeden kurtulmak için, DevOps işlem hattı değişkeni `RUN_EVALUATION` `.pipelines/[project name]-variables-template.yml` `false`olarak ayarlayın.

## <a name="next-steps"></a>Sonraki adımlar

Bir deneyden üretim koduna nasıl dönüştüreceğiniz artık, Web Hizmetleri olarak dağıtılan deneme çalıştırmalarını ve modellerini izlemeyi öğrenmek için aşağıdaki bağlantıları kullanın:

> [!div class="nextstepaction"]
> [Ml Web hizmeti uç noktalarından verileri izlemek ve toplamak](https://docs.microsoft.com/azure/machine-learning/how-to-enable-app-insights)
> [Azure ML deneme çalıştırmaları ve ölçümleri izleyin](https://docs.microsoft.com/azure/machine-learning/how-to-track-experiments)
