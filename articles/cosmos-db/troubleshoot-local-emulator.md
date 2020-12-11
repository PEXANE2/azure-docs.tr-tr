---
title: Azure Cosmos DB öykünücüsü 'nü kullanırken sorun giderme
description: Azure Cosmos DB öykünücüsü kullanırken hizmet kullanılamıyor, sertifika, şifreleme ve sürüm oluşturma sorunlarını troubleshot nasıl kullanacağınızı öğrenin.
ms.service: cosmos-db
ms.topic: troubleshooting
author: markjbrown
ms.author: mjbrown
ms.date: 09/17/2020
ms.custom: contperf-fy21q1
ms.openlocfilehash: bb07adafbc68ff1e9b7d3ee49bb7631dc4395d77
ms.sourcegitcommit: 3ea45bbda81be0a869274353e7f6a99e4b83afe2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2020
ms.locfileid: "97033468"
---
# <a name="troubleshoot-issues-when-using-the-azure-cosmos-db-emulator"></a>Azure Cosmos DB öykünücüsü 'nü kullanırken sorun giderme
[!INCLUDE[appliesto-all-apis](includes/appliesto-all-apis.md)]

Azure Cosmos DB Öykünücüsü, geliştirme amaçlı olarak Azure Cosmos DB hizmetine öykünen yerel bir ortam sağlar. Azure Cosmos DB öykünücüsünü yüklerken veya kullanırken karşılaştığınız sorunları gidermeye yardımcı olması için bu makaledeki ipuçlarını kullanın. 

Öykünücünün yeni bir sürümünü yüklediyseniz ve hatalarla karşılaşıyorsanız, verilerinizi sıfırladığınızdan emin olun. Sistem tepsisinde Azure Cosmos DB öykünücü simgesine sağ tıklayıp verileri Sıfırla... ' ya tıklayarak verilerinizi sıfırlayabilirsiniz. Bu, hataları gidermezse, varsa öykünücü ve öykünücü sürümlerini kaldırabilir, *C:\Program files\Azure Cosmos DB öykünücü* dizinini kaldırabilir ve öykünücüyü yeniden yükleyebilirsiniz. Yönergeler için bkz. [Yerel öykünücüden kaldırma](local-emulator.md#uninstall). Alternatif olarak, verilerin sıfırlanması işe yaramazsa, konum ' a gidin `%LOCALAPPDATA%\CosmosDBEmulator` ve klasörü silin.

## <a name="troubleshoot-corrupted-windows-performance-counters"></a>Bozuk Windows performans sayaçlarıyla ilgili sorunları giderme

* Azure Cosmos DB öykünücü kilitlenirse, döküm dosyalarını klasöründen toplayın, `%LOCALAPPDATA%\CrashDumps` onları sıkıştırarak ve [Azure Portal](https://portal.azure.com)bir destek bileti açın.

* İçinde kilitlenmeler yaşarsanız `Microsoft.Azure.Cosmos.ComputeServiceStartupEntryPoint.exe` , bu durum performans sayaçlarının bozulmuş durumda olduğu bir belirti olabilir. Genellikle bir yönetici komut isteminden aşağıdaki komutu çalıştırmak sorunu düzeltir:

  ```cmd
  lodctr /R
   ```

## <a name="troubleshoot-connectivity-issues"></a>Bağlantı sorunlarını giderme

* Bir bağlantı sorunuyla karşılaşırsanız, [izleme dosyaları toplayın](#trace-files), bunları sıkıştırın ve [Azure Portal](https://portal.azure.com)bir destek bileti açın.

* **Hizmet Kullanılamıyor** iletisi alırsanız öykünücü, ağ yığınını başlatamıyor olabilir. Pulse secure istemcisinin veya Juniper networks istemcisinin yüklü olup olmadığını denetleyin; bunların ağ filtresi sürücüleri soruna yol açıyor olabilir. Genellikle üçüncü taraf ağ filtresi sürücüleri kaldırıldığında sorun düzeltilir. Alternatif olarak, öykünücü ağ iletişimini normal Winsock 'a geçecek olan/Disablerıo ile öykünücüyü başlatın. 

* " **Yasak" ile karşılaşırsanız "ileti": "istek, Aktarım Protokolü veya şifre içinde yasaklanmış bir şifrelemeyle yapılıyor. Hesap SSL/TLS izin verilen en düşük protokol ayarı... "** bağlantı sorunları, bu durum işletim sisteminde genel değişikliklerden kaynaklanabilir (örneğin, Insider Preview Build 20170) veya varsayılan olarak TLS 1,3 ' i etkinleştiren tarayıcı ayarları olabilir. **Microsoft.Azure.Documents.DocumentClientException: istek, Aktarım Protokolü veya şifre 'nda yasaklanmış bir şifrelemeyle yapılmakta olduğundan, Cosmos öykünücüsüne karşı bir istek yürütmek IÇIN SDK kullanılırken benzer bir hata oluşabilir. Hesap SSL/TLS izin verilen en düşük protokol ayarını denetleyin**. Bu durum bu kez beklenen bir durumdur çünkü Cosmos öykünücüsü yalnızca TLS 1.2 protokolünü kabul eder ve bununla çalışır. Önerilen geçici çözüm, ayarların ve varsayılan olarak TLS 1,2 olarak değiştirilmesini sağlar; Örneğin, IIS Yöneticisi 'nde "siteler" e gidin-"varsayılan Web siteleri" > ve bağlantı noktası 8081 için "Site bağlamaları" ' nı bulun ve TLS 1,3 ' yi devre dışı bırakmak için düzenleyin. Buna benzer bir işlem Web tarayıcısında “Ayarlar” seçenekleriyle de yapılabilir.

* Öykünücü çalışırken bilgisayarınız uyku moduna geçer veya herhangi bir işletim sistemi güncelleştirmesi çalıştırırsa, bir **Service is currently unavailable** (Hizmet şu anda kullanılamıyor) iletisi alabilirsiniz. Windows bildirim tepsisinde görüntülenen simgeye sağ tıklayıp **verileri Sıfırla**' yı seçerek öykünücü verilerini sıfırlayın.

## <a name="collect-trace-files"></a><a id="trace-files"></a>İzleme dosyalarını toplama

Hata ayıklama izlemelerini toplamak için bir yönetici komut isteminden aşağıdaki komutları çalıştırın:

1. Öykünücünün yüklendiği yola gidin:

   ```bash
   cd /d "%ProgramFiles%\Azure Cosmos DB Emulator"
   ```

1. Öykünücüyü kapatın ve programın kapatıldığından emin olmak için sistem tepsisini izleyin. Tamamlanması birkaç dakika sürebilir. Azure Cosmos DB öykünücü Kullanıcı arabiriminde de **Çıkış** ' ı seçebilirsiniz.

   ```bash
   Microsoft.Azure.Cosmos.Emulator.exe /shutdown
   ```

1. Aşağıdaki komutla günlüğe kaydetmeyi Başlat:

   ```bash
   Microsoft.Azure.Cosmos.Emulator.exe /startwprtraces
   ```

1. Öykünücüyü Başlat

   ```bash
   Microsoft.Azure.Cosmos.Emulator.exe
   ```

1. Sorunu yeniden oluşturun. Veri Gezgini çalışmıyorsa, hatayı yakalamak için yalnızca birkaç saniye boyunca tarayıcının açılmasını beklemeniz gerekir.

1. Aşağıdaki komutla günlüğü durdurun:

   ```bash
   Microsoft.Azure.Cosmos.Emulator.exe /stopwprtraces
   ```
   
1. Yola gidin `%ProgramFiles%\Azure Cosmos DB Emulator` ve *docdbemulator_000001. etl* dosyasını bulun.

1. [Azure Portal](https://portal.azure.com) bir destek bileti açın ve yeniden üretme adımlarıyla birlikte. etl dosyasını dahil edin.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, yerel öykünücü ile ilgili sorunların nasıl ayıklanalınacağını öğrendiniz. Şimdi bir sonraki makaleye devam edebilirsiniz:

* [Java, Python ve Node.js uygulamalarıyla kullanmak üzere Azure Cosmos DB öykünücü sertifikalarını dışarı aktarın](local-emulator-export-ssl-certificates.md)
* [Öykünücüyü denetlemek için komut satırı parametrelerini ve PowerShell komutlarını kullanma](emulator-command-line-parameters.md)
