---
title: "Ausführen von Hive-Abfragen per HDInsight .NET SDK – Azure | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie Hadoop-Aufträge per HDInsight .NET-SDK an Azure HDInsight Hadoop übermitteln können."
editor: cgronlun
manager: jhubbard
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
ms.assetid: 4e291890-f8b4-426c-b5e8-d4fd512ff042
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/20/2017
ms.author: jgao
ms.openlocfilehash: 93649c51c6dd6c158e3870a2fa75d92ae4579b7e
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/21/2018
---
# <a name="run-hive-queries-using-hdinsight-net-sdk"></a>Ausführen von Hive-Abfragen per HDInsight .NET-SDK
[!INCLUDE [hive-selector](../../../includes/hdinsight-selector-use-hive.md)]

Erfahren Sie, wie Sie Hive-Abfragen mit dem HDInsight .NET-SDK übermitteln. Sie schreiben ein C#-Programm, um eine Hive-Abfrage zum Auflisten von Hive-Tabellen zu senden, und die Ergebnisse werden angezeigt.

> [!NOTE]
> Die Schritte in diesem Artikel müssen auf einem Windows-Client ausgeführt werden. Informationen zur Verwendung eines Linux-, OS X- oder Unix-Clients für Hive erhalten Sie, indem Sie die Registerkartenauswahl am Anfang des Artikels nutzen.

## <a name="prerequisites"></a>Voraussetzungen
Bevor Sie mit diesem Artikel beginnen können, benötigen Sie Folgendes:

* **Einen Hadoop-Cluster in HDInsight**. Siehe [Erste Schritte bei der Verwendung von Linux-basiertem Hadoop in HDInsight](apache-hadoop-linux-tutorial-get-started.md).

    > [!WARNING]
    > Ab dem 15. September 2017 unterstützt das HDInsight .NET SDK nur zurückgegebene Hive-Abfrageergebnisse von Azure Storage-Konten. Wenn Sie dieses Beispiel mit einem HDInsight-Cluster verwenden, das Azure Data Lake Store als primären Speicher nutzt, können Sie keine Suchergebnisse mehr mit dem .NET SDK abrufen.

* **Visual Studio 2013/2015/2017**.

## <a name="run-a-hive-query"></a>Ausführen einer Hive-Abfrage
Das HDInsight .NET SDK enthält .NET-Clientbibliotheken, die das Arbeiten mit HDInsight-Clustern in .NET vereinfachen. 

**So übermitteln Sie Aufträge**

1. Erstellen Sie in Visual Studio eine C#-Konsolenanwendung.
2. Führen Sie in der NuGet-Paket-Manager-Konsole den folgenden Befehl aus:
   
        Install-Package Microsoft.Azure.Management.HDInsight.Job
3. Verwenden Sie den folgenden Code:

    ```csharp
        using System.Collections.Generic;
        using System.IO;
        using System.Text;
        using System.Threading;
        using Microsoft.Azure.Management.HDInsight.Job;
        using Microsoft.Azure.Management.HDInsight.Job.Models;
        using Hyak.Common;
   
        namespace SubmitHDInsightJobDotNet
        {
            class Program
            {
                private static HDInsightJobManagementClient _hdiJobManagementClient;
   
                private const string ExistingClusterName = "<Your HDInsight Cluster Name>";
                private const string ExistingClusterUri = ExistingClusterName + ".azurehdinsight.net";
                private const string ExistingClusterUsername = "<Cluster Username>";
                private const string ExistingClusterPassword = "<Cluster User Password>";
                
                // Only Azure Storage accounts are supported by the SDK
                private const string DefaultStorageAccountName = "<Default Storage Account Name>";
                private const string DefaultStorageAccountKey = "<Default Storage Account Key>";
                private const string DefaultStorageContainerName = "<Default Blob Container Name>";
   
                static void Main(string[] args)
                {
                    System.Console.WriteLine("The application is running ...");
   
                    var clusterCredentials = new BasicAuthenticationCloudCredentials { Username = ExistingClusterUsername, Password = ExistingClusterPassword };
                    _hdiJobManagementClient = new HDInsightJobManagementClient(ExistingClusterUri, clusterCredentials);
   
                    SubmitHiveJob();
   
                    System.Console.WriteLine("Press ENTER to continue ...");
                    System.Console.ReadLine();
                }
   
                private static void SubmitHiveJob()
                {
                    Dictionary<string, string> defines = new Dictionary<string, string> { { "hive.execution.engine", "tez" }, { "hive.exec.reducers.max", "1" } };
                    List<string> args = new List<string> { { "argA" }, { "argB" } };
                    var parameters = new HiveJobSubmissionParameters
                    {
                        Query = "SHOW TABLES",
                        Defines = defines,
                        Arguments = args
                    };
   
                    System.Console.WriteLine("Submitting the Hive job to the cluster...");
                    var jobResponse = _hdiJobManagementClient.JobManagement.SubmitHiveJob(parameters);
                    var jobId = jobResponse.JobSubmissionJsonResponse.Id;
                    System.Console.WriteLine("Response status code is " + jobResponse.StatusCode);
                    System.Console.WriteLine("JobId is " + jobId);
   
                    System.Console.WriteLine("Waiting for the job completion ...");
   
                    // Wait for job completion
                    var jobDetail = _hdiJobManagementClient.JobManagement.GetJob(jobId).JobDetail;
                    while (!jobDetail.Status.JobComplete)
                    {
                        Thread.Sleep(1000);
                        jobDetail = _hdiJobManagementClient.JobManagement.GetJob(jobId).JobDetail;
                    }
   
                    // Get job output
                    var storageAccess = new AzureStorageAccess(DefaultStorageAccountName, DefaultStorageAccountKey,
                        DefaultStorageContainerName);
                    var output = (jobDetail.ExitValue == 0)
                        ? _hdiJobManagementClient.JobManagement.GetJobOutput(jobId, storageAccess) // fetch stdout output in case of success
                        : _hdiJobManagementClient.JobManagement.GetJobErrorLogs(jobId, storageAccess); // fetch stderr output in case of failure
   
                    System.Console.WriteLine("Job output is: ");
   
                    using (var reader = new StreamReader(output, Encoding.UTF8))
                    {
                        string value = reader.ReadToEnd();
                        System.Console.WriteLine(value);
                    }
                }
            }
        }
    ```
4. Drücken Sie **F5**, um die Anwendung auszuführen.

Die Ausgabe der Anwendung sollte Folgendem ähneln:

![Ausgabe des HDInsight Hadoop Hive-Auftrags](./media/apache-hadoop-use-hive-dotnet-sdk/hdinsight-hadoop-use-hive-net-sdk-output.png)

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie mehrere Möglichkeiten zum Erstellen von HDInsight-Clustern kennengelernt. Weitere Informationen finden Sie in den folgenden Artikeln:

* [Erste Schritte mit Azure HDInsight](apache-hadoop-linux-tutorial-get-started.md)
* [Erstellen von Hadoop-Clustern in HDInsight](../hdinsight-hadoop-provision-linux-clusters.md)
* [Verwalten von Hadoop-Clustern in HDInsight mit dem Azure-Portal](../hdinsight-administer-use-management-portal.md)
* [HDInsight .NET SDK-Referenz](https://msdn.microsoft.com/library/mt271028.aspx)
* [Verwenden von Pig mit HDInsight](hdinsight-use-pig.md)
* [Verwenden von Sqoop mit HDInsight](apache-hadoop-use-sqoop-mac-linux.md)
* [Erstellen von .NET HDInsight-Anwendungen für die nicht interaktive Authentifizierung](../hdinsight-create-non-interactive-authentication-dotnet-applications.md)
 


