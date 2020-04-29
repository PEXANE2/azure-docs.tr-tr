---
title: Tek başına kümeyi Temizleme
description: Bu öğreticide, tek başına Service Fabric kümenizdeki AWS veya Azure kaynaklarını temizlemeyi öğreneceksiniz.
author: dkkapur
ms.topic: tutorial
ms.date: 07/22/2019
ms.author: dekapur
ms.custom: mvc
ms.openlocfilehash: bfb23ca5f5eb9540491fbd05efdfd6997db15e6b
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "75639029"
---
# <a name="tutorial-clean-up-your-standalone-cluster"></a>Öğretici: Tek başına kümenizi temizleme

Service Fabric tek başına kümeleri, kendi ortamınızı seçme ve Service Fabric’in benimsediği "her işletim sistemi, her bulut" yaklaşımının bir parçası olarak bir küme oluşturma seçeneği sunar. Bu öğretici serisinde, AWS veya Azure üzerinde barındırılan bir tek başına küme oluşturur ve bu kümeye bir uygulama yüklersiniz.

Bu öğretici, bir serinin dördüncü bölümüdür. Öğreticinin bu bölümü, Service Fabric kümenizi barındırmak için oluşturduğunuz AWS veya Azure kaynaklarını nasıl temizleyekullanabileceğinizi gösterir.

Serinin dördüncü kısmında öğrenecekleriniz:

> [!div class="checklist"]
> * Service Fabric kümesini temizleme
> * AWS veya Azure kaynaklarınızı temizleyin

## <a name="clean-up-service-fabric-cluster"></a>Service Fabric kümesini temizleme

1. Service Fabric yüklemek için kullandığınız sanal makineye RDP.
2. PowerShell’i açın.
3. Dizini, ikinci öğreticide ayıklanan klasöre geçirin.
4. Service Fabric kümesini kaldırmak için aşağıdaki komutu çalıştırın:

  ```powershell
  .\RemoveServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.MultiMachine.json
  ```

5. `Y` İstendiğinde, Çıktınıza, kendi IP adreslerinizin yerini alarak aşağıdaki gibi görünür:

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

1. AWS hesabınızda oturum açın.
2. EC2 Konsolu'na gidin.
3. Öğreticinin birinci bölümünde oluşturduğunuz üç düğümü seçin.
4. **Eylemler** > **örneği durum** > **Sonlandır**' a tıklayın.

## <a name="clean-up-azure-resources"></a>Azure kaynaklarını Temizleme

1. Azure Portal’da oturum açın.
2. **Sanal makineler** bölümüne gidin.
3. Öğreticinin bir parçası olarak oluşturduğunuz üç düğüm için onay kutularını seçin.
4. **Sil**' e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Serinin dördüncü bölümünde, önceki adımlarda oluşturulan kaynaklarınızı nasıl temizleyeceğinizi öğrendiniz.

> [!div class="checklist"]
> * Kaynaklarınızı temizleme

> [!div class="nextstepaction"]
> [Başa dön](service-fabric-tutorial-standalone-create-infrastructure.md)
