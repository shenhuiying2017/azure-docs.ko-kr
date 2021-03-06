---
title: Azure PowerShell 스크립트 샘플 - 웹앱에 대한 예약된 백업 만들기 | Microsoft Docs
description: Azure PowerShell 스크립트 샘플 - 웹앱에 대한 예약된 백업 만들기
services: app-service\web
documentationcenter: ''
author: cephalin
manager: cfowler
editor: ''
tags: azure-service-management
ms.assetid: a2a27d94-d378-4c17-a6a9-ae1e69dc4a72
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: sample
ms.date: 10/30/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 4afa7c2b144fcb1d72d82f146719c26d7ceb4eb4
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/19/2018
ms.locfileid: "31586792"
---
# <a name="create-a-scheduled-backup-for-a-web-app"></a>웹앱에 대한 예약된 백업 만들기

이 샘플 스크립트는 App Service에 관련 리소스가 있는 웹앱을 만든 다음 이에 대한 예약된 백업을 만듭니다. 

필요한 경우 [Azure PowerShell 가이드](/powershell/azure/overview)에 있는 지침을 사용하여 Azure PowerShell을 설치한 다음, `Connect-AzureRmAccount`를 실행하여 Azure에 연결합니다. 

## <a name="sample-script"></a>샘플 스크립트

[!code-powershell[main](../../../powershell_scripts/app-service/backup-scheduled/backup-scheduled.ps1?highlight=1-4 "Create a scheduled backup for a web app")]

## <a name="clean-up-deployment"></a>배포 정리 

스크립트 샘플을 실행한 후에는 다음 명령을 사용하여 리소스 그룹, 웹앱 및 모든 관련된 리소스를 제거할 수 있습니다.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a>스크립트 설명

이 스크립트는 다음 명령을 사용합니다. 테이블에 있는 각 명령은 명령에 해당하는 문서에 연결됩니다.

| 명령 | 메모 |
|---|---|
| [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | 모든 리소스가 저장되는 리소스 그룹을 만듭니다. |
| [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) | Storage 계정을 만듭니다. |
| [New-AzureStorageContainer](/powershell/module/azure.storage/new-azurestoragecontainer) | Azure 저장소 컨테이너를 만듭니다. |
| [New-AzureStorageContainerSASToken](/powershell/module/azure.storage/new-azurestoragecontainersastoken) | Azure Storage 컨테이너에 대한 SAS 토큰을 생성합니다. |
| [New-AzureRmAppServicePlan](/powershell/module/azurerm.websites/new-azurermappserviceplan) | App Service 계획을 만듭니다. |
| [New-AzureRmWebApp](/powershell/module/azurerm.websites/new-azurermwebapp) | 웹앱을 만듭니다. |
| [Edit-AzureRmWebAppBackupConfiguration](/powershell/module/azurerm.websites/edit-azurermwebappbackupconfiguration) | 웹앱에 대한 백업 구성을 편집합니다. |
| [Get-AzureRmWebAppBackupList](/powershell/module/azurerm.websites/get-azurermwebappbackuplist) | 웹앱의 백업 목록을 가져옵니다. |
| [Get-AzureRmWebAppBackupConfiguration](/powershell/module/azurerm.websites/get-azurermwebappbackupconfiguration) | 웹앱에 대한 구성을 가져옵니다. |

## <a name="next-steps"></a>다음 단계

Azure PowerShell 모듈에 대한 자세한 내용은 [Azure PowerShell 설명서](/powershell/azure/overview)를 참조하세요.

Azure App Service Web Apps에 대한 추가 Azure PowerShell 샘플은 [Azure PowerShell 샘플](../app-service-powershell-samples.md)에서 확인할 수 있습니다.
