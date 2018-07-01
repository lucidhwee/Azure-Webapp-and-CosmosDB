
# .NET 웹 어플리케이션을 Azure App services(Web app)와 Azure Cosmos DB(SQL API) 기반으로 배포하기

> 주요 참조 원본 가이드 :  
> **Quickstart: Build a .NET web app with Azure Cosmos DB using the SQL API and the Azure portal** (https://docs.microsoft.com/en-us/azure/cosmos-db/create-sql-api-dotnet)
> 
<br /> 

## 준비사항
- Visual studio 2017 (Enterprise or Professional or Community 에디션)
- Azure 구독
- Git Client 설치 : https://git-scm.com/downloads  
<br /> 

## 데이터베이스 계정 및 컬렉션 생성
- 다음 링크의 **Create a database account 및 Add a collection** 단계 진행. 이때 컬렉션 명이 **Items** 여야 함을 주의
(https://docs.microsoft.com/en-us/azure/cosmos-db/create-sql-api-dotnet#create-a-database-account)
- **Add sample data 및 Query your data**  단계는 선택적으로 진행
<br /> 
  
## 샘플 어플리케이션 복제
 - Git CLI를 이용하거나 Visual studio의 Team Explorer 상에서 제공하는 기능을 이용하여 GitHub 상에 존재하는 샘플 어플리케이션을 로컬 PC(또는 개발 서버)에 복제
	 - 샘플 어플리케이션 위치 : https://github.com/Azure-Samples/documentdb-dotnet-todo 
	 - Git CLI 이용 시 명령줄 :
	 *git clone https://github.com/Azure-Samples/documentdb-dotnet-todo-app.git*	  
	- Visual Studio 이용 시 참조 링크 :  
![Clone your repo from other providers using Visual Studio](https://docs.microsoft.com/en-us/vsts/git/tutorial/_img/clone_other_providers.png?view=vsts)  
(https://docs.microsoft.com/en-us/vsts/git/tutorial/clone?view=vsts&tabs=visual-studio#clone-from-another-git-provider)
<br />  
  
## 코드 리뷰
- 코드 리뷰 단계는, *코드 상에서 어떻게 데이터베이스 리소스가 생성되는지*를 이해하고자 하는 경우 선택적으로 진행
 (https://docs.microsoft.com/en-us/azure/cosmos-db/create-sql-api-dotnet#review-the-code)
<br />  
  
## 커넥션 스트링 업데이트
- Azure Portal에서 연결이 필요한 Azure Cosmos DB 계정에 대한 **URI** 및 **Primary Key**를 획득
![View and copy an access key in the Azure portal, Keys blade](https://docs.microsoft.com/en-us/azure/cosmos-db/media/create-sql-api-dotnet/keys.png)
(https://docs.microsoft.com/en-us/azure/cosmos-db/create-sql-api-dotnet#update-your-connection-string)

- Visual Studio 2017 에서 **web.config** 파일을 오픈하여 **URI** 및 **Primary Key**를 수정
	- web.config 상의 **URI** 입력 위치 : 
	<*add key="endpoint" value=**"FILLME"** /*>    
	- web.config 상의 **Primary Key** 입력 위치 : 
 	<*add key="authKey" value=**"FILLME"** /*>  
- 그 외 기존 생성해두었던 **Database** 및 **Collection**을 활용하고자 하는 경우, 각각 **<*add key="database"...*>** 와 **<*add key="collection"...*>** 의 **value** 값을 알맞게 수정
<br />   
  	
## 로컬 PC(개발 서버)에서 웹앱 테스트
- Visual Studio 상에서 Nuget 통해 Cosmos DB 라이브러리(**Microsoft.Azure.DocumentDB**) 설치
(https://docs.microsoft.com/en-us/azure/cosmos-db/create-sql-api-dotnet#run-the-web-app)
- **CTRL + F5** 통해 to-do 앱 실행
- 실행된 브라우저 상에서 **Create New** 를 클릭하여 새로운 task가 정상적으로 생성되는지 확인
![Todo app with sample data](https://docs.microsoft.com/en-us/azure/cosmos-db/media/create-sql-api-dotnet/azure-comosdb-todo-app-list.png)

- Azure Portal 상의 **Cosmos DB - Data Explorer** 에서 새로이 생성된 Document 확인 가능
 ![Default query in Data Explorer is `SELECT * FROM c`](https://docs.microsoft.com/en-us/azure/includes/media/cosmos-db-create-sql-api-query-data/azure-cosmosdb-data-explorer-query.png)
(https://docs.microsoft.com/en-us/azure/cosmos-db/create-sql-api-dotnet#query-your-data)
<br /> 
  
## 로컬 테스트 완료된 웹앱을 Azure App services(Web app)으로 배포
- Visual studio의 **Solution Explorer** 상에서 **todo** 프로젝트의 *오른쪽 버튼* 클릭 후, **Publish** 클릭
![Publish from Solution Explorer](https://docs.microsoft.com/en-us/azure/app-service/media/app-service-web-get-started-dotnet-framework/solution-explorer-publish.png)
(https://docs.microsoft.com/en-us/azure/app-service/app-service-web-get-started-dotnet-framework#publish-to-azure)

- 이후 전환된 화면에서 **Microsoft Azure App Service** 선택 후, **Create New**를 선택하여 새로운 App Service를 생성하거나, 또는 **Select Existing**을 선택하여 기존 생성된 App Service 상에 배포
![Publish from project overview page](https://docs.microsoft.com/en-us/azure/app-service/media/app-service-web-get-started-dotnet-framework/publish-to-app-service.png)
	- Visual studio가 기존 **Azure account**와 연계되어 있지 않을 경우, [Sign in to Azure](https://docs.microsoft.com/en-us/azure/app-service/app-service-web-get-started-dotnet-framework#sign-in-to-azure) 단계를 통해 로그인 진행. 
	- **Create New**를 선택하여 App Service 를 새로이 생성 후 배포 시 참조 링크 : 
https://docs.microsoft.com/en-us/azure/app-service/app-service-web-get-started-dotnet-framework#create-an-app-service-plan
<br />   
  
## Azure App Services(Web app)에 배포된 웹앱 테스트
- Azure Portal 상의 Azure Web app 관리 화면에서 외부 접근 가능한 **URL** *(e.g. http://xxx.azurewebsites.net/)* 확인 및 복사
![App Service blade in Azure portal](https://docs.microsoft.com/en-us/azure/app-service/media/app-service-web-get-started-dotnet-framework/web-app-blade.png)
(https://docs.microsoft.com/en-us/azure/app-service/app-service-web-get-started-dotnet-framework#manage-the-azure-web-app)

- 브라우저 상에 복사한 URL *(e.g. http://xxx.azurewebsites.net/)* 을 입력하여 정상적으로 todo 앱이 동작하는지를 테스트
![Todo app with sample data](https://docs.microsoft.com/en-us/azure/cosmos-db/media/create-sql-api-dotnet/azure-comosdb-todo-app-list.png)
