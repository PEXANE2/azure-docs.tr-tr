---
title: Cihaz modelleri deposunun kavramlarını anlayın | Microsoft Docs
description: Bir çözüm geliştiricisi veya BT uzmanı olarak, cihaz modelleri deposunun temel kavramları hakkında bilgi edinin.
author: rido-min
ms.author: rmpablos
ms.date: 11/17/2020
ms.topic: conceptual
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: 1a58a2f69b9c6c6742c4b9daf32dd0e13341aac1
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101742152"
---
# <a name="device-models-repository"></a>Cihaz modelleri deposu

Cihaz modelleri deposu (DMR), cihaz oluşturucuların IoT Tak ve Kullan cihaz modellerini yönetmesine ve paylaşmasına olanak sağlar. Cihaz modelleri, [dijital TWINS modelleme dili (DTDL)](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/dtdlv2.md)KULLANıLARAK tanımlanan JSON ld belgelerdir.

DMR, DTDL arabirimlerini cihaz ikizi modeli tanımlayıcısı (DTMı) temelinde bir klasör yapısına depolamak için bir model tanımlar. DTMı 'yi göreli bir yola dönüştürerek DMR 'de bir arabirim bulabilirsiniz. Örneğin, `dtmi:com:example:Thermostat;1` dtmı öğesine çevrilir `/dtmi/com/example/thermostat-1.json` .

## <a name="public-device-models-repository"></a>Ortak cihaz modelleri deposu

Microsoft, şu özelliklerle genel bir DMR barındırır:

- Seçkin modeller. Microsoft, GitHub çekme isteği (PR) doğrulama iş akışını kullanarak tüm kullanılabilir arabirimleri gözden geçirir ve onaylar.
- Değiştirilemezlik.  Yayımlandıktan sonra bir arabirim güncelleştirilemiyor.
- Hiper ölçek. Microsoft, cihaz modellerini yayımlayabileceğiniz ve kullanabileceğiniz güvenli, ölçeklenebilir bir uç nokta oluşturmak için gerekli altyapıyı sağlar.

## <a name="custom-device-models-repository"></a>Özel cihaz modelleri deposu

Yerel dosya sistemi veya özel HTTP Web sunucuları gibi herhangi bir depolama ortamında özel bir DMR oluşturmak için aynı DMR modelini kullanın. Özel DMR 'den cihaz modellerini genel DMR 'den aynı şekilde, DMR 'e erişmek için kullanılan temel URL 'YI değiştirerek alabilirsiniz.

> [!NOTE]
> Microsoft, genel DMR 'de cihaz modellerini doğrulamaya yönelik araçlar sağlar. Bu araçları özel depolarda yeniden kullanabilirsiniz.

## <a name="public-models"></a>Ortak modeller

Modeller deposunda depolanan ortak cihaz modelleri, herkesin uygulamalarında kullanması ve tümleştirilmesi için kullanılabilir. Ortak cihaz modelleri, cihaz oluşturucular ve çözüm geliştiricilerinin IoT Tak ve Kullan cihaz modellerini paylaşmasını ve yeniden kullanmasına olanak sağlayan açık bir ekonomik sistem sağlar.

Model oluşturma bölümünde model yayımlama hakkında yönergeler için [model yayımlama](#publish-a-model) bölümüne bakın.

Kullanıcılar resmi [GitHub deposundan](https://github.com/Azure/iot-plugandplay-models)ortak arabirimlere gözatabilir, arama yapabilir ve görüntüleyebilir.

Klasörlerdeki tüm arabirimler `dtmi` genel uç noktada da kullanılabilir [https://devicemodels.azure.com](https://devicemodels.azure.com)

### <a name="resolve-models"></a>Modelleri çözümle

Bu arabirimlere programlı bir şekilde erişmek için, bir DTMı 'yi genel uç noktasını sorgulamak için kullanabileceğiniz göreli bir yola dönüştürmeniz gerekir.

Bir DTMı 'yi mutlak bir yola dönüştürmek için, `DtmiToPath` işlevi ile birlikte kullanın `IsValidDtmi` :

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

1. Genel GitHub deposunu çatalla: [https://github.com/Azure/iot-plugandplay-models](https://github.com/Azure/iot-plugandplay-models) .
1. Ele geçirilen depoyu kopyalayın. Değişikliklerinizin dalından yalıtılmış olmasını sağlamak için isteğe bağlı olarak yeni bir dal oluşturun `main` .
1. Klasörü `dtmi` /dosya adı kuralını kullanarak yeni arabirimleri klasöre ekleyin. Daha fazla bilgi için bkz. [bir modeli `dtmi/` klasöre aktarma](#import-a-model-to-the-dtmi-folder).
1. Aracı kullanarak modelleri yerel olarak doğrulayın `dmr-client` . Daha fazla bilgi için bkz. [modelleri doğrulama](#validate-models).
1. Değişiklikleri yerel olarak işleyin ve Çatalınıza gönderin.
1. Çatalınızdan dalı hedefleyen bir çekme isteği oluşturun `main` . Bkz. [bir sorun veya çekme isteği belgeleri oluşturma](https://docs.github.com/free-pro-team@latest/desktop/contributing-and-collaborating-using-github-desktop/creating-an-issue-or-pull-request) .
1. [Çekme isteği gereksinimlerini](https://github.com/Azure/iot-plugandplay-models/blob/main/pr-reqs.md)gözden geçirin.

Çekme isteği, gönderilen arabirimleri doğrulayan bir GitHub eylemi kümesi tetikler ve çekme isteğinizin tüm gereksinimleri karşıladığından emin olur.

Microsoft, bir çekme isteğine üç iş günü içinde tüm denetimleri vererek yanıt verir.

### <a name="dmr-client-tools"></a>`dmr-client` Aracı

PR denetimleri sırasında modelleri doğrulamak için kullanılan araçlar, DTDL arabirimlerini yerel olarak eklemek ve doğrulamak için de kullanılabilir.

> [!NOTE]
> Bu araç, [.NET SDK](https://dotnet.microsoft.com/download) sürüm 3,1 veya üstünü gerektirir.

### <a name="install-dmr-client"></a>Yükleyeceğiniz `dmr-client`

```bash
curl -L https://aka.ms/install-dmr-client-linux | bash
```

```powershell
iwr https://aka.ms/install-dmr-client-windows -UseBasicParsing | iex
```

### <a name="import-a-model-to-the-dtmi-folder"></a>Klasöre model aktarma `dtmi/`

Modelinize zaten JSON dosyalarında depolandıysanız, bu `dmr-client import` komutu, `dtmi/` doğru dosya adlarıyla klasöre eklemek için kullanabilirsiniz:

```bash
# from the local repo root folder
dmr-client import --model-file "MyThermostat.json"
```

> [!TIP]
> `--local-repo`Yerel depo kök klasörünü belirtmek için bağımsız değişkenini kullanabilirsiniz.

### <a name="validate-models"></a>Modelleri doğrula

Şu komutla modellerinizi doğrulayabilirsiniz `dmr-client validate` :

```bash
dmr-client validate --model-file ./my/model/file.json
```

> [!NOTE]
> Doğrulama, tüm arabirimlerin DTDL dil belirtimiyle uyumlu olduğundan emin olmak için en son DTDL ayrıştırıcısı sürümünü kullanır.

Dış bağımlılıkları doğrulamak için yerel depoda mevcut olmaları gerekir. Modelleri doğrulamak için, `--repo` `local` bağımlılıkları çözümlemek üzere bir veya klasörü belirtmek için seçeneğini kullanın `remote` :

```bash
# from the repo root folder
dmr-client validate --model-file ./my/model/file.json --repo .
```

### <a name="strict-validation"></a>Katı doğrulama

DMR ek [gereksinimler](https://github.com/Azure/iot-plugandplay-models/blob/main/pr-reqs.md)içerir, `stict` modelinizi bunlara karşı doğrulamak için bayrağını kullanın:

```bash
dmr-client validate --model-file ./my/model/file.json --repo . --strict true
```

Hata iletileri için konsol çıktısını denetleyin.

### <a name="export-models"></a>Modelleri dışarı aktarma

Modeller, belirli bir depodan (yerel veya uzak) bir JSON dizisi kullanılarak tek bir dosyaya aktarılabilir:

```bash
dmr-client export --dtmi "dtmi:com:example:TemperatureController;1" -o TemperatureController.expanded.json
```

## <a name="next-steps"></a>Sonraki adımlar

Önerilen sonraki adım [ıot Tak ve kullan mimarisini](concepts-architecture.md)gözden geçirmeye yönelik olur.
