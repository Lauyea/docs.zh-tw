---
title: 每個微服務的資料自主性
description: 每個微服務之資料自主性是微服務的其中一個重點。 每個微服務必須是其資料庫的唯一擁有者，不與任何其他人共用。 當然，微服務所有執行個體會連接到相同的高可用性資料庫。
ms.date: 09/20/2018
ms.openlocfilehash: ccb12451cd7cd44938e09d171eb29e614786f469
ms.sourcegitcommit: f20dd18dbcf2275513281f5d9ad7ece6a62644b4
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/30/2019
ms.locfileid: "68673175"
---
# <a name="data-sovereignty-per-microservice"></a><span data-ttu-id="914b2-105">每個微服務的資料自主性</span><span class="sxs-lookup"><span data-stu-id="914b2-105">Data sovereignty per microservice</span></span>

<span data-ttu-id="914b2-106">微服務架構的重要規則是，每個微服務必須擁有自己的網域資料和邏輯。</span><span class="sxs-lookup"><span data-stu-id="914b2-106">An important rule for microservices architecture is that each microservice must own its domain data and logic.</span></span> <span data-ttu-id="914b2-107">就像完整的應用程式擁有其邏輯和資料，每個微服務也必須在自發的生命週期裡擁有其邏輯和資料，並且每個微服務有獨立的部署。</span><span class="sxs-lookup"><span data-stu-id="914b2-107">Just as a full application owns its logic and data, so must each microservice own its logic and data under an autonomous lifecycle, with independent deployment per microservice.</span></span>

<span data-ttu-id="914b2-108">這表示，網域的概念模型將在各個子系統或微服務之間有所不同。</span><span class="sxs-lookup"><span data-stu-id="914b2-108">This means that the conceptual model of the domain will differ between subsystems or microservices.</span></span> <span data-ttu-id="914b2-109">請考慮企業應用程式，其中客戶關係管理 (CRM) 應用程式、交易式購買子系統和客戶支援子系統每個都呼叫不同的客戶實體屬性和資料，而每一者都使用不同的繫結內容 (BC)。</span><span class="sxs-lookup"><span data-stu-id="914b2-109">Consider enterprise applications, where customer relationship management (CRM) applications, transactional purchase subsystems, and customer support subsystems each call on unique customer entity attributes and data, and where each employs a different Bounded Context (BC).</span></span>

<span data-ttu-id="914b2-110">這個原則非常類似[網域導向設計 (DDD)](https://en.wikipedia.org/wiki/Domain-driven_design)，其中每個[繫結內容](https://martinfowler.com/bliki/BoundedContext.html)或自發子系統或服務必須擁有其網域模型 (資料加上邏輯和行為)。</span><span class="sxs-lookup"><span data-stu-id="914b2-110">This principle is similar in [Domain-driven design (DDD)](https://en.wikipedia.org/wiki/Domain-driven_design), where each [Bounded Context](https://martinfowler.com/bliki/BoundedContext.html) or autonomous subsystem or service must own its domain model (data plus logic and behavior).</span></span> <span data-ttu-id="914b2-111">每個 DDD 繫結內容相互關聯到一個商務微服務 (一或多個服務)。</span><span class="sxs-lookup"><span data-stu-id="914b2-111">Each DDD Bounded Context correlates to one business microservice (one or several services).</span></span> <span data-ttu-id="914b2-112">我們會在下一節從這點繼續展開繫結內容模式。</span><span class="sxs-lookup"><span data-stu-id="914b2-112">This point about the Bounded Context pattern is expanded in the next section.</span></span>

<span data-ttu-id="914b2-113">在另一方面，許多應用程式中使用的傳統 (單一資料) 方法，是單一的集中式資料庫或幾個資料庫。</span><span class="sxs-lookup"><span data-stu-id="914b2-113">On the other hand, the traditional (monolithic data) approach used in many applications is to have a single centralized database or just a few databases.</span></span> <span data-ttu-id="914b2-114">這通常是標準化的 SQL 資料庫，用於整個應用程式及其所有內部子系統，如圖 4-7 所示。</span><span class="sxs-lookup"><span data-stu-id="914b2-114">This is often a normalized SQL database that's used for the whole application and all its internal subsystems, as shown in Figure 4-7.</span></span>

![在傳統的方法中，所有服務之間會共用單一資料庫，通常使用分層架構。](./media/image7.png)

<span data-ttu-id="914b2-117">**圖 4-7**.</span><span class="sxs-lookup"><span data-stu-id="914b2-117">**Figure 4-7**.</span></span> <span data-ttu-id="914b2-118">資料自主性比較：單一資料庫與微服務</span><span class="sxs-lookup"><span data-stu-id="914b2-118">Data sovereignty comparison: monolithic database versus microservices</span></span>

<span data-ttu-id="914b2-119">集中式資料庫方法一開始看起來較簡單，而且似乎能重複使用不同子系統中的實體，讓一切一致。</span><span class="sxs-lookup"><span data-stu-id="914b2-119">The centralized database approach initially looks simpler and seems to enable reuse of entities in different subsystems to make everything consistent.</span></span> <span data-ttu-id="914b2-120">但事實上，您會得到很大的資料表，它們會服務許多不同子系統，且包含在大部分情況下不需要的屬性和資料行。</span><span class="sxs-lookup"><span data-stu-id="914b2-120">But the reality is you end up with huge tables that serve many different subsystems, and that include attributes and columns that aren't needed in most cases.</span></span> <span data-ttu-id="914b2-121">就像是嘗試使用相同的實體地圖來進行短距離的健走、一整天的汽車旅行，以及學習地理。</span><span class="sxs-lookup"><span data-stu-id="914b2-121">It's like trying to use the same physical map for hiking a short trail, taking a day-long car trip, and learning geography.</span></span>

<span data-ttu-id="914b2-122">一般具有單一關聯式資料庫的單一應用程式有兩個重要優點：[ACID 交易](https://en.wikipedia.org/wiki/ACID)和 SQL 語言，兩者都可在與您應用程式相關的所有資料表和資料之間使用。</span><span class="sxs-lookup"><span data-stu-id="914b2-122">A monolithic application with typically a single relational database has two important benefits: [ACID transactions](https://en.wikipedia.org/wiki/ACID) and the SQL language, both working across all the tables and data related to your application.</span></span> <span data-ttu-id="914b2-123">這種方法可用來輕鬆地撰寫查詢，以結合來自多個資料表的資料。</span><span class="sxs-lookup"><span data-stu-id="914b2-123">This approach provides a way to easily write a query that combines data from multiple tables.</span></span>

<span data-ttu-id="914b2-124">不過，當您移到微服務架構時，資料存取變得複雜許多。</span><span class="sxs-lookup"><span data-stu-id="914b2-124">However, data access becomes much more complex when you move to a microservices architecture.</span></span> <span data-ttu-id="914b2-125">但即使 ACID 交易可以或應該用於微服務或繫結內容中，每個微服務所擁有的資料仍是該微服務私用，並且只能透過其微服務 API 來存取。</span><span class="sxs-lookup"><span data-stu-id="914b2-125">But even when ACID transactions can or should be used within a microservice or Bounded Context, the data owned by each microservice is private to that microservice and can only be accessed via its microservice API.</span></span> <span data-ttu-id="914b2-126">封裝資料可確保微服務鬆散偶合，而且彼此獨立地持續改進。</span><span class="sxs-lookup"><span data-stu-id="914b2-126">Encapsulating the data ensures that the microservices are loosely coupled and can evolve independently of one another.</span></span> <span data-ttu-id="914b2-127">如果多個服務存取相同的資料，結構描述更新便需要所有服的協調性更新。</span><span class="sxs-lookup"><span data-stu-id="914b2-127">If multiple services were accessing the same data, schema updates would require coordinated updates to all the services.</span></span> <span data-ttu-id="914b2-128">這會破壞微服務生命週期自主。</span><span class="sxs-lookup"><span data-stu-id="914b2-128">This would break the microservice lifecycle autonomy.</span></span> <span data-ttu-id="914b2-129">但分散式資料結構表示您無法讓單一 ACID 交易跨越微服務。</span><span class="sxs-lookup"><span data-stu-id="914b2-129">But distributed data structures mean that you can't make a single ACID transaction across microservices.</span></span> <span data-ttu-id="914b2-130">這又表示當商務程序跨越多個微服務時，您必須使用最終一致性。</span><span class="sxs-lookup"><span data-stu-id="914b2-130">This in turn means you must use eventual consistency when a business process spans multiple microservices.</span></span> <span data-ttu-id="914b2-131">這要實作時比簡單的 SQL 聯結難上許多，因為您無法建立完整性條件約束，或在不同的資料庫之間使用分散式交易，我們將在稍後說明。</span><span class="sxs-lookup"><span data-stu-id="914b2-131">This is much harder to implement than simple SQL joins, because you can't create integrity constraints or use distributed transactions between separate databases, as we'll explain later on.</span></span> <span data-ttu-id="914b2-132">同樣地，許多其他關聯式資料庫功能無法跨多個微服務使用。</span><span class="sxs-lookup"><span data-stu-id="914b2-132">Similarly, many other relational database features aren't available across multiple microservices.</span></span>

<span data-ttu-id="914b2-133">更進一步地說，不同的微服務通常使用不同「種類」  的資料庫。</span><span class="sxs-lookup"><span data-stu-id="914b2-133">Going even further, different microservices often use different *kinds* of databases.</span></span> <span data-ttu-id="914b2-134">現代應用程式會存放和處理多種種類的資料，且關聯式資料庫不一定是最佳選擇。</span><span class="sxs-lookup"><span data-stu-id="914b2-134">Modern applications store and process diverse kinds of data, and a relational database isn't always the best choice.</span></span> <span data-ttu-id="914b2-135">對於某些使用案例，例如 Azure CosmosDB 或 MongoDB 的 NoSQL 資料庫，可能會有更方便的資料模型，並提供較 SQL Server 或 Azure SQL Database 等 SQL 資料庫更佳的效能和延展性。</span><span class="sxs-lookup"><span data-stu-id="914b2-135">For some use cases, a NoSQL database such as Azure CosmosDB or MongoDB might have a more convenient data model and offer better performance and scalability than a SQL database like SQL Server or Azure SQL Database.</span></span> <span data-ttu-id="914b2-136">在其他情況下，關聯式資料庫仍是最好的方法。</span><span class="sxs-lookup"><span data-stu-id="914b2-136">In other cases, a relational database is still the best approach.</span></span> <span data-ttu-id="914b2-137">因此，以微服務為基礎的應用程式通常會使用 SQL 和 NoSQL 資料庫的混合，這有時稱為[混合持續性 (Polyglot Persistence)](https://martinfowler.com/bliki/PolyglotPersistence.html) 方法。</span><span class="sxs-lookup"><span data-stu-id="914b2-137">Therefore, microservices-based applications often use a mixture of SQL and NoSQL databases, which is sometimes called the [polyglot persistence](https://martinfowler.com/bliki/PolyglotPersistence.html) approach.</span></span>

<span data-ttu-id="914b2-138">資料儲存體的資料分割、混合持續架構有許多優點。</span><span class="sxs-lookup"><span data-stu-id="914b2-138">A partitioned, polyglot-persistent architecture for data storage has many benefits.</span></span> <span data-ttu-id="914b2-139">這些包括鬆散偶合的服務和更佳的效能、延展性、成本與管理能力。</span><span class="sxs-lookup"><span data-stu-id="914b2-139">These include loosely coupled services and better performance, scalability, costs, and manageability.</span></span> <span data-ttu-id="914b2-140">不過，它可能導致某些分散式資料管理的挑戰，如本章稍後的[識別領域模型界限](identify-microservice-domain-model-boundaries.md)中所說明。</span><span class="sxs-lookup"><span data-stu-id="914b2-140">However, it can introduce some distributed data management challenges, as explained in "[Identifying domain-model boundaries](identify-microservice-domain-model-boundaries.md)" later in this chapter.</span></span>

## <a name="the-relationship-between-microservices-and-the-bounded-context-pattern"></a><span data-ttu-id="914b2-141">微服務和繫結內容模式之間的關聯性</span><span class="sxs-lookup"><span data-stu-id="914b2-141">The relationship between microservices and the Bounded Context pattern</span></span>

<span data-ttu-id="914b2-142">微服務概念衍生自[網域導向設計 (DDD)](https://en.wikipedia.org/wiki/Domain-driven_design) 中的[繫結內容 (BC) 模式](https://martinfowler.com/bliki/BoundedContext.html)。</span><span class="sxs-lookup"><span data-stu-id="914b2-142">The concept of microservice derives from the [Bounded Context (BC) pattern](https://martinfowler.com/bliki/BoundedContext.html) in [domain-driven design (DDD)](https://en.wikipedia.org/wiki/Domain-driven_design).</span></span> <span data-ttu-id="914b2-143">DDD 透過將大型模型分成多個 BC 並且明確指定其界限來處理大型模型。</span><span class="sxs-lookup"><span data-stu-id="914b2-143">DDD deals with large models by dividing them into multiple BCs and being explicit about their boundaries.</span></span> <span data-ttu-id="914b2-144">每個 BC 必須有自己的模型和資料庫。同樣地，每個微服務也擁有其相關資料。</span><span class="sxs-lookup"><span data-stu-id="914b2-144">Each BC must have its own model and database; likewise, each microservice owns its related data.</span></span> <span data-ttu-id="914b2-145">此外，每個 BC 通常都有它自己的[通用語言](https://martinfowler.com/bliki/UbiquitousLanguage.html)，協助軟體開發人員和網域專家之間的溝通。</span><span class="sxs-lookup"><span data-stu-id="914b2-145">In addition, each BC usually has its own [ubiquitous language](https://martinfowler.com/bliki/UbiquitousLanguage.html) to help communication between software developers and domain experts.</span></span>

<span data-ttu-id="914b2-146">通用語言中的詞彙 (主要為領域實體) 在不同繫結內容中可以有不同名稱，即使是不同領域實體都共用相同的識別 (亦即，用來從儲存體讀取實體的唯一識別碼)。</span><span class="sxs-lookup"><span data-stu-id="914b2-146">Those terms (mainly domain entities) in the ubiquitous language can have different names in different Bounded Contexts, even when different domain entities share the same identity (that is, the unique ID that's used to read the entity from storage).</span></span> <span data-ttu-id="914b2-147">比方說，在使用者設定檔繫結內容中，使用者網域實體可能會與排序之繫結內容中的買方網域實體共用身分識別。</span><span class="sxs-lookup"><span data-stu-id="914b2-147">For instance, in a user-profile Bounded Context, the User domain entity might share identity with the Buyer domain entity in the ordering Bounded Context.</span></span>

<span data-ttu-id="914b2-148">因此，微服務就像是繫結內容，但它也會指定其為一種分散式服務。</span><span class="sxs-lookup"><span data-stu-id="914b2-148">A microservice is therefore like a Bounded Context, but it also specifies that it's a distributed service.</span></span> <span data-ttu-id="914b2-149">它針對每個繫結內容建置為個別的處理序，且必須使用前文所述的分散式通訊協定，例如 HTTP/HTTPS、WebSockets 或 [AMQP](https://en.wikipedia.org/wiki/Advanced_Message_Queuing_Protocol)。</span><span class="sxs-lookup"><span data-stu-id="914b2-149">It's built as a separate process for each Bounded Context, and it must use the distributed protocols noted earlier, like HTTP/HTTPS, WebSockets, or [AMQP](https://en.wikipedia.org/wiki/Advanced_Message_Queuing_Protocol).</span></span> <span data-ttu-id="914b2-150">不過，繫結內容模式並未指定繫結內容是否為分散式服務，還是它只是單一部署應用程式中的邏輯界限 (例如泛型子系統)。</span><span class="sxs-lookup"><span data-stu-id="914b2-150">The Bounded Context pattern, however, doesn't specify whether the Bounded Context is a distributed service or if it's simply a logical boundary (such as a generic subsystem) within a monolithic-deployment application.</span></span>

<span data-ttu-id="914b2-151">值得強調的一點是，為每個繫結內容定義服務是個不錯的起點。</span><span class="sxs-lookup"><span data-stu-id="914b2-151">It's important to highlight that defining a service for each Bounded Context is a good place to start.</span></span> <span data-ttu-id="914b2-152">但您不必將設計限制於此。</span><span class="sxs-lookup"><span data-stu-id="914b2-152">But you don't have to constrain your design to it.</span></span> <span data-ttu-id="914b2-153">有時您必須設計由數個實體服務組成的繫結內容或商務微服務。</span><span class="sxs-lookup"><span data-stu-id="914b2-153">Sometimes you must design a Bounded Context or business microservice composed of several physical services.</span></span> <span data-ttu-id="914b2-154">但最後，這兩種模式 - 繫結內容和微服務 - 密切相關。</span><span class="sxs-lookup"><span data-stu-id="914b2-154">But ultimately, both patterns -Bounded Context and microservice- are closely related.</span></span>

<span data-ttu-id="914b2-155">DDD 藉由以分散式微服務的形式得到真實界限而從微服務獲益。</span><span class="sxs-lookup"><span data-stu-id="914b2-155">DDD benefits from microservices by getting real boundaries in the form of distributed microservices.</span></span> <span data-ttu-id="914b2-156">但如同不要在微服務之間共用模型，您可能也不想在繫結內容中共用模型。</span><span class="sxs-lookup"><span data-stu-id="914b2-156">But ideas like not sharing the model between microservices are what you also want in a Bounded Context.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="914b2-157">其他資源</span><span class="sxs-lookup"><span data-stu-id="914b2-157">Additional resources</span></span>

- <span data-ttu-id="914b2-158">**Chris Richardson：模式：每個服務的資料庫** </span><span class="sxs-lookup"><span data-stu-id="914b2-158">**Chris Richardson. Pattern: Database per service** </span></span>\
  <https://microservices.io/patterns/data/database-per-service.html>

- <span data-ttu-id="914b2-159">**Martin Fowler：BoundedContext** </span><span class="sxs-lookup"><span data-stu-id="914b2-159">**Martin Fowler. BoundedContext** </span></span>\
  <https://martinfowler.com/bliki/BoundedContext.html>

- <span data-ttu-id="914b2-160">**Martin Fowler：PolyglotPersistence** </span><span class="sxs-lookup"><span data-stu-id="914b2-160">**Martin Fowler. PolyglotPersistence** </span></span>\
  <https://martinfowler.com/bliki/PolyglotPersistence.html>

- <span data-ttu-id="914b2-161">**Alberto Brandolini.含內容對應的策略性領域導向設計** </span><span class="sxs-lookup"><span data-stu-id="914b2-161">**Alberto Brandolini. Strategic Domain Driven Design with Context Mapping** </span></span>\
  <https://www.infoq.com/articles/ddd-contextmapping>

>[!div class="step-by-step"]
><span data-ttu-id="914b2-162">[上一頁](microservices-architecture.md)
>[下一頁](logical-versus-physical-architecture.md)</span><span class="sxs-lookup"><span data-stu-id="914b2-162">[Previous](microservices-architecture.md)
[Next](logical-versus-physical-architecture.md)</span></span>