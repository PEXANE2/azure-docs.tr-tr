---
title: Cihaz modeli deposunun kavramlarını anlayın | Microsoft Docs
description: Bir çözüm geliştiricisi veya BT uzmanı olarak, cihaz modeli deposunun temel kavramları hakkında bilgi edinin.
author: rido-min
ms.author: rmpablos
ms.date: 09/30/2020
ms.topic: conceptual
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: 4e15ef5256c1552fc8ab7fb9bd84f15bb3433834
ms.sourcegitcommit: 33368ca1684106cb0e215e3280b828b54f7e73e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2020
ms.locfileid: "92131369"
---
# <a name="device-model-repository"></a>Cihaz modeli deposu

Cihaz modeli deposu (DMR), cihaz oluşturucuların IoT Tak ve Kullan cihaz modellerini yönetmesine ve paylaşmasına olanak sağlar. Cihaz modelleri, [dijital TWINS modelleme dili (DTDL)](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/dtdlv2.md)KULLANıLARAK tanımlanan JSON ld belgelerdir.

DMR, DTDL arabirimlerini cihaz ikizi modeli tanımlayıcısı (DTMı) temelinde bir klasör yapısına depolamak için bir model tanımlar. DTMı 'yi göreli bir yola dönüştürerek DMR 'de bir arabirim bulabilirsiniz. Örneğin, `dtmi:com:example:Thermostat;1` dtmı öğesine çevrilir `/dtmi/com/example/thermostat-1.json` .

## <a name="public-device-model-repository"></a>Ortak cihaz modeli deposu

Microsoft, şu özelliklerle genel bir DMR barındırır:

- Seçkin modeller. Microsoft, GitHub çekme isteği (PR) doğrulama iş akışını kullanarak tüm kullanılabilir arabirimleri gözden geçirir ve onaylar.
- Değiştirilemezlik.  Yayımlandıktan sonra bir arabirim güncelleştirilemiyor.
- Hiper ölçek. Microsoft, cihaz modellerini yayımlayabileceğiniz ve kullanabileceğiniz güvenli, ölçeklenebilir bir uç nokta oluşturmak için gerekli altyapıyı sağlar.

## <a name="custom-device-model-repository"></a>Özel cihaz modeli deposu

Özel bir DMR oluşturmak için, yerel dosya sistemi veya özel HTTP Web sunucuları gibi herhangi bir depolama ortamında aynı DMR modelini kullanabilirsiniz. Özel DMR 'den, genel DMR ile aynı şekilde, DMR 'e erişmek için kullanılan temel URL 'YI değiştirerek, cihaz modellerini alabilirsiniz.

> [!NOTE]
> Microsoft, genel DMR 'de cihaz modellerini doğrulamaya yönelik araçlar sağlar. Bu araçları özel depolarda yeniden kullanabilirsiniz.

## <a name="public-models"></a>Ortak modeller

Model deposunda depolanan ortak cihaz modelleri, herkesin uygulamalarında kullanması ve tümleştirilmesi için kullanılabilir. Ortak cihaz modelleri, cihaz oluşturucular ve çözüm geliştiricilerinin IoT Tak ve Kullan cihaz modellerini paylaşmasını ve yeniden kullanmasına olanak sağlayan açık bir ekonomik sistem sağlar.

Model deposunda model yayımlama hakkında yönergeler için model [Yayımlama](#publish-a-model) bölümüne bakın.

Kullanıcılar resmi [GitHub deposundan](https://github.com/Azure/iot-plugandplay-models)ortak arabirimlere gözatabilir, arama yapabilir ve görüntüleyebilir.

Klasörlerdeki tüm arabirimler `dtmi` genel uç noktada da kullanılabilir [https://devicemodels.azure.com](https://devicemodels.azure.com)

### <a name="resolve-models"></a>Modelleri çözümle

Bu arabirimlere programlı bir şekilde erişmek için, bir DTMı 'yi genel uç noktasını sorgulamak için kullanabileceğiniz göreli bir yola dönüştürmeniz gerekir. Aşağıdaki kod örneğinde bunun nasıl yapılacağı gösterilmektedir:

Bir DTMı 'yi mutlak bir yola dönüştürmek için `DtmiToPath` işlevini kullanın `IsValidDtmi` :

```cs
static string DtmiToPath(string dtmi)
{
    if (!IsValidDtmi(dtmi))
    {
        return null;
    }
    // dtmi:com:example:Thermostat;1 -> dtmi/com/example/thermostat-1.json
    return $"/{dtmi.ToLowerInvariant().Replace(":", "/").Replace(";", "-")}.json";
}

static bool IsValidDtmi(string dtmi)
{
    // Regex defined at https://github.com/Azure/digital-twin-model-identifier#validation-regular-expressions
    Regex rx = new Regex(@"^dtmi:[A-Za-z](?:[A-Za-z0-9_]*[A-Za-z0-9])?(?::[A-Za-z](?:[A-Za-z0-9_]*[A-Za-z0-9])?)*;[1-9][0-9]{0,8}$");
    return rx.IsMatch(dtmi);
}
```

Sonuç yolu ve deponun temel URL 'SI ile arabirimi elde etmemiz için şunu kullanabilirsiniz:

```cs
const string _repositoryEndpoint = "https://devicemodels.azure.com";

string dtmiPath = DtmiToPath(dtmi.ToString());
string fullyQualifiedPath = $"{_repositoryEndpoint}{dtmiPath}";
string modelContent = await _httpClient.GetStringAsync(fullyQualifiedPath);
```

## <a name="publish-a-model"></a>Modeli yayımlama

> [!Important]
> Genel DMR 'e model gönderebilmek için bir GitHub hesabınızın olması gerekir.

1. Genel GitHub deposunun çatalını oluştur: [https://github.com/Azure/iot-plugandplay-models](https://github.com/Azure/iot-plugandplay-models) .
1. Çatallanmış depoyu kopyalayın. Değişikliklerinizin dalından yalıtılmış olmasını sağlamak için isteğe bağlı olarak yeni bir dal oluşturun `main` .
1. Klasörü `dtmi` /dosya adı kuralını kullanarak yeni arabirimleri klasöre ekleyin. Bkz. [Add-model](#add-model) aracı.
1. [Değişiklikleri doğrulamak için betikler](#validate-files) bölümünü kullanarak cihaz modellerini yerel olarak doğrulayın.
1. Değişiklikleri yerel olarak işleyin ve Çatalınıza gönderin.
1. Çatalınızdan dalı hedefleyen bir çekme isteği oluşturun `main` . Bkz. [bir sorun veya çekme isteği belgeleri oluşturma](https://docs.github.com/free-pro-team@latest/desktop/contributing-and-collaborating-using-github-desktop/creating-an-issue-or-pull-request) .
1. [Çekme isteği gereksinimlerini](https://github.com/Azure/iot-plugandplay-models/blob/main/pr-reqs.md)gözden geçirin.

Çekme isteği, yeni gönderilen arabirimleri doğrulayan bir dizi GitHub eylemini tetikler ve çekme isteğinizin tüm denetimleri karşıladığından emin olur.

Microsoft, bir çekme isteğine üç iş günü içinde tüm denetimleri vererek yanıt verir.

### <a name="add-model"></a>Model Ekle

Aşağıdaki adımlarda add-model.js betiğin yeni bir arabirim eklemenize nasıl yardımcı olduğu gösterilmektedir. Bu betik Node.js çalıştırmasını gerektirir:

1. Bir komut isteminden yerel git deposuna gidin
1. `npm install` komutunu çalıştırın
1. `npm run add-model <path-to-file-to-add>` komutunu çalıştırın

Herhangi bir hata iletisi için konsol çıkışını izleyin.

### <a name="local-validation"></a>Yerel doğrulama

Sorunları önceden tanılamanıza yardımcı olmak üzere çekme isteğini göndermeden önce, aynı doğrulama denetimlerini yerel olarak çalıştırabilirsiniz.

#### <a name="validate-files"></a>dosyaları doğrula

`npm run validate-files <file1.json> <file2.json>` dosya yolunun beklenen klasör ve dosya adlarıyla eşleşip eşleşmediğini denetler.

#### <a name="validate-ids"></a>doğrulama kimlikleri

`npm run validate-ids <file1.json> <file2.json>` belgede tanımlanan tüm kimliklerin ana KIMLIğIN aynı kökünü kullanıp kullanmadığını denetler.

#### <a name="validate-deps"></a>Validate-Deps

`npm run validate-deps <file1.json> <file2.json>` Tüm bağımlılıkların klasörde kullanılabilir olduğunu denetler `dtmi` .

#### <a name="validate-models"></a>Validate-modeller

Cihaz modellerinizi yerel olarak doğrulamak için [Dtdl doğrulama örneğini](https://github.com/Azure-Samples/DTDL-Validator) çalıştırabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Önerilen sonraki adım [ıot Tak ve kullan mimarisini](concepts-architecture.md)gözden geçirmeye yönelik olur.
