<properties
	pageTitle="Индексирование файлов мультимедиа с помощью индексатора мультимедийных данных Azure 2 (предварительная версия)"
	description="Индексатор мультимедийных данных Azure позволяет искать содержимое файлов мультимедиа и создавать полнотекстовую транскрипцию для скрытых субтитров и ключевых слов. В этом разделе показано, как использовать индексатор мультимедийных данных 2 (предварительная версия)."
	services="media-services"
	documentationCenter=""
	authors="Juliako"
	manager="erikre"
	editor=""/>

<tags
	ms.service="media-services"
	ms.workload="media"
	ms.tgt_pltfrm="na"
	ms.devlang="dotnet"
	ms.topic="article"
	ms.date="06/22/2016" 
	ms.author="adsolank;juliako;"/>


# Индексирование файлов мультимедиа с помощью индексатора мультимедийных данных Azure 2 (предварительная версия)

##Обзор

Обработчик мультимедиа **индексатор мультимедийных данных Azure 2 (предварительная версия)** позволяет сделать мультимедийные файлы и содержимое доступными для поиска, а также создавать дорожки для субтитров. По сравнению с предыдущей версией [индексатора мультимедийных данных](media-services-index-content.md), **индексатор мультимедийных данных Azure 2 (предварительная версия)** быстрее выполняет индексирование и предлагает более широкую поддержку языков. В число поддерживаемых языков входят английский, испанский, французский, немецкий, итальянский, китайский, португальский и арабский.

Сейчас обработчик мультимедиа **индексатор мультимедийных данных Azure 2 (предварительная версия)** доступен в предварительной версии.

В этом разделе показано, как создать задания индексирования с помощью **индексатора мультимедийных данных Azure 2 (предварительная версия)**.

>[AZURE.NOTE]Действительны следующие условия.
>
>Индексатор 2 не поддерживается в Azure в Китае и Azure для государственных организаций.
>
>Предварительная версия ограничена и предоставляет около 10 минут обработки, но является бесплатной для всех клиентов.
>
>При индексировании содержимого обязательно используйте файлы мультимедиа, содержащие отчетливую речь (без фоновой музыки, шума, эффектов или шипения микрофона). Примерами подходящего содержимого могут служить записи собраний, лекций или презентаций. Для индексирования, как правило, не подходит такое содержимое, как фильмы, телепередачи, любые материалы со смешанными аудио- и звуковыми эффектами, записи плохого качества с фоновым шумом (шипением).


В этом разделе приводятся сведения об **индексаторе мультимедийных данных Azure 2 (предварительная версия)** и демонстрируется его использование с пакетом SDK служб мультимедиа для .NET.

##Входные и выходные файлы

###Входные файлы

Аудио- или видеофайлы

###Выходные файлы

Задание индексирования может создавать файлы скрытых субтитров в следующих форматах:

- **SAMI**;
- **TTML**;
- **WebVTT**.

С помощью файлов скрытых субтитров в этих форматах аудио- и видеофайлы можно сделать доступным для людей с нарушениями слуха.

##Конфигурация задачи (предустановка)

При создании задачи индексирования с помощью **предварительной версии индексатора мультимедийных данных Azure 2** необходимо указать предустановку конфигурации.

Следующий объект JSON задает доступные параметры.

	{
	  "version":"1.0",
	  "Features":
	    [
	       {
	       "Options": {
	            "Formats":["WebVtt","ttml"],
	            "Language":"enUs",
	            "Type":"RecoOptions"
	       },
	       "Type":"SpReco"
	    }]
	}

##Поддерживаемые языки  

Индексатор мультимедийных данных Azure 2 (предварительная версия) поддерживает распознавание речи для следующих языков (при указании названия языка в файле конфигурации задачи используйте 4-значный код в квадратных скобках, как показано ниже):

- английский [EnUs];
- испанский [EsEs];
- китайский [ZhCn];
- французский [FrFr];
- немецкий [DeDe];
- итальянский [ItIt];
- португальский [PtBr];
- арабский (Египет) [ArEg].


## Пример кода

В следующей программе показано, как выполнить следующие задачи.

1. Создание ресурса-контейнера и отправка в него файла мультимедиа.
1. Создание задания с задачей индексирования на основе файла конфигурации, содержащего следующую предустановку JSON.
			
		{
		  "version":"1.0",
		  "Features":
		    [
		       {
		       "Options": {
		            "Formats":["WebVtt","ttml"],
		            "Language":"enUs",
		            "Type":"RecoOptions"
		       },
		       "Type":"SpReco"
		    }]
		}

1. Скачивание выходных файлов.
	
		using System;
		using System.Configuration;
		using System.IO;
		using System.Linq;
		using Microsoft.WindowsAzure.MediaServices.Client;
		using System.Threading;
		using System.Threading.Tasks;
		
		namespace IndexContent
		{
		    class Program
		    {
		        // Read values from the App.config file.
		        private static readonly string _mediaServicesAccountName =
		            ConfigurationManager.AppSettings["MediaServicesAccountName"];
		        private static readonly string _mediaServicesAccountKey =
		            ConfigurationManager.AppSettings["MediaServicesAccountKey"];
		
		        // Field for service context.
		        private static CloudMediaContext _context = null;
		        private static MediaServicesCredentials _cachedCredentials = null;
		
		        static void Main(string[] args)
		        {
		
		            // Create and cache the Media Services credentials in a static class variable.
		            _cachedCredentials = new MediaServicesCredentials(
		                            _mediaServicesAccountName,
		                            _mediaServicesAccountKey);
		            // Used the cached credentials to create CloudMediaContext.
		            _context = new CloudMediaContext(_cachedCredentials);
		
		            // Run indexing job.
		            var asset = RunIndexingJob(@"C:\supportFiles\Indexer\BigBuckBunny.mp4",
		                                        @"C:\supportFiles\Indexer\config.json");
		
		            // Download the job output asset.
		            DownloadAsset(asset, @"C:\supportFiles\Indexer\Output");
		        }
		
		        static IAsset RunIndexingJob(string inputMediaFilePath, string configurationFile)
		        {
		            // Create an asset and upload the input media file to storage.
		            IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
		                "My Indexing Input Asset",
		                AssetCreationOptions.None);
		
		            // Declare a new job.
		            IJob job = _context.Jobs.Create("My Indexing Job");
		
		            // Get a reference to Azure Media Indexer 2 Preview.
		            string MediaProcessorName = "Azure Media Indexer 2 Preview";
		
		            var processor = GetLatestMediaProcessorByName(MediaProcessorName);
		
		            // Read configuration from the specified file.
		            string configuration = File.ReadAllText(configurationFile);
		
		            // Create a task with the encoding details, using a string preset.
		            ITask task = job.Tasks.AddNew("My Indexing Task",
		                processor,
		                configuration,
		                TaskOptions.None);
		
		            // Specify the input asset to be indexed.
		            task.InputAssets.Add(asset);
		
		            // Add an output asset to contain the results of the job.
		            task.OutputAssets.AddNew("My Indexing Output Asset", AssetCreationOptions.None);
		
		            // Use the following event handler to check job progress.  
		            job.StateChanged += new EventHandler<JobStateChangedEventArgs>(StateChanged);
		
		            // Launch the job.
		            job.Submit();
		
		            // Check job execution and wait for job to finish.
		            Task progressJobTask = job.GetExecutionProgressTask(CancellationToken.None);
		
		            progressJobTask.Wait();
		
		            // If job state is Error, the event handling
		            // method for job progress should log errors.  Here we check
		            // for error state and exit if needed.
		            if (job.State == JobState.Error)
		            {
		                ErrorDetail error = job.Tasks.First().ErrorDetails.First();
		                Console.WriteLine(string.Format("Error: {0}. {1}",
		                                                error.Code,
		                                                error.Message));
		                return null;
		            }
		
		            return job.OutputMediaAssets[0];
		        }
		
		        static IAsset CreateAssetAndUploadSingleFile(string filePath, string assetName, AssetCreationOptions options)
		        {
		            IAsset asset = _context.Assets.Create(assetName, options);
		
		            var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
		            assetFile.Upload(filePath);
		
		            return asset;
		        }
		
		        static void DownloadAsset(IAsset asset, string outputDirectory)
		        {
		            foreach (IAssetFile file in asset.AssetFiles)
		            {
		                file.Download(Path.Combine(outputDirectory, file.Name));
		            }
		        }
		
		        static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
		        {
		            var processor = _context.MediaProcessors
		                .Where(p => p.Name == mediaProcessorName)
		                .ToList()
		                .OrderBy(p => new Version(p.Version))
		                .LastOrDefault();
		
		            if (processor == null)
		                throw new ArgumentException(string.Format("Unknown media processor",
		                                                           mediaProcessorName));
		
		            return processor;
		        }
		
		        static private void StateChanged(object sender, JobStateChangedEventArgs e)
		        {
		            Console.WriteLine("Job state changed event:");
		            Console.WriteLine("  Previous state: " + e.PreviousState);
		            Console.WriteLine("  Current state: " + e.CurrentState);
		
		            switch (e.CurrentState)
		            {
		                case JobState.Finished:
		                    Console.WriteLine();
		                    Console.WriteLine("Job is finished.");
		                    Console.WriteLine();
		                    break;
		                case JobState.Canceling:
		                case JobState.Queued:
		                case JobState.Scheduled:
		                case JobState.Processing:
		                    Console.WriteLine("Please wait...\n");
		                    break;
		                case JobState.Canceled:
		                case JobState.Error:
		                    // Cast sender as a job.
		                    IJob job = (IJob)sender;
		                    // Display or log error details as needed.
		                    // LogJobStop(job.Id);
		                    break;
		                default:
		                    break;
		            }
		        }
		    }
		}


##Схемы обучения работе со службами мультимедиа

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##Отзывы

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


## Связанные ссылки

[Общие сведения об аналитике служб мультимедиа Azure](media-services-analytics-overview.md)

[Демонстрационные материалы для медиааналитики Azure](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)

<!---HONumber=AcomDC_0629_2016-->