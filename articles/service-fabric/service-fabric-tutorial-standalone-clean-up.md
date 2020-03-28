---
title: Bağımsız bir kümeyi temizleme
description: Bu eğitimde, bağımsız Service Fabric kümenizde AWS veya Azure kaynaklarını nasıl temizleyesiniz öğrenebilirsiniz.
author: dkkapur
ms.topic: tutorial
ms.date: 07/22/2019
ms.author: dekapur
ms.custom: mvc
ms.openlocfilehash: bfb23ca5f5eb9540491fbd05efdfd6997db15e6b
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75639029"
---
# <a name="tutorial-clean-up-your-standalone-cluster"></a>Öğretici: Tek başına kümenizi temizleme

Service Fabric tek başına kümeleri, kendi ortamınızı seçme ve Service Fabric’in benimsediği "her işletim sistemi, her bulut" yaklaşımının bir parçası olarak bir küme oluşturma seçeneği sunar. Bu öğretici seride, AWS veya Azure'da barındırılan bağımsız bir küme oluşturur ve içine bir uygulama yüklersiniz.

Bu öğretici, bir serinin dördüncü bölümüdür. Öğreticinin bu bölümü, Service Fabric kümenizi barındırmak için oluşturduğunuz AWS veya Azure kaynaklarını nasıl temizleyesiniz gösterir.

Serinin dördüncü kısmında öğrenecekleriniz:

> [!div class="checklist"]
> * Service Fabric kümesini temizleme
> * AWS veya Azure kaynaklarınızı temizleme

## <a name="clean-up-service-fabric-cluster"></a>Service Fabric kümesini temizleme

1. Service Fabric'i yüklemek için kullandığınız VM'ye RDP.
2. PowerShell’i açın.
3. Dizini, ikinci öğreticide ayıklanan klasöre geçirin.
4. Service Fabric kümesini kaldırmak için aşağıdaki komutu çalıştırın:

  ```powershell
  .\RemoveServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.MultiMachine.json
  ```

5. İstendiğinde girin, `Y` başarılı olduysa çıktınız aşağıdaki gibi görünecek ve kendi IP adresleriniz aşağıdaki lerle değiştirilmiştir:

  ```powershell
  Best Practices Analyzer completed successfully.
  Removing configuration from machine 172.31.21.141
  Removing configuration from machine 172.31.27.1
  Removing configuration from machine 172.31.20.163
  Configuration removed from machine 172.31.21.141
  Configuration removed from machine 172.31.27.1
  Configuration removed from machine 172.31.20.163
  The cluster is successfully removed.
  ```

## <a name="clean-up-aws-resources"></a>AWS kaynaklarını temizleme

1. AWS Hesabınızda oturum açın.
2. EC2 Konsolu'na gidin.
3. Öğreticinin birinci bölümünde oluşturduğunuz üç düğümü seçin.
4. **Eylemler** > **Örneği Durum** > **Sonlandırma'ya**tıklayın.

## <a name="clean-up-azure-resources"></a>Azure kaynaklarını temizleme

1. Azure Portal’da oturum açın.
2. **Sanal Makineler** bölümüne gidin.
3. Öğreticinin birinci bölümünde oluşturduğunuz üç düğüm için onay kutularını seçin.
4. **Sil'e**tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Serinin dördüncü bölümünde, önceki adımlarda oluşturulan kaynaklarınızı nasıl temizleyeceğinizi öğrendiniz.

> [!div class="checklist"]
> * Kaynaklarınızı temizleme

> [!div class="nextstepaction"]
> [Başa dön](service-fabric-tutorial-standalone-create-infrastructure.md)
